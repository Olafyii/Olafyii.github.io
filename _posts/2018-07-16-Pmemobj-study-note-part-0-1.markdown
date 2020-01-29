---
layout: post
title: Pmemobj study note part 0&1Memory"---lecture notes
date: 2018-07-16 15:14:00 +0800
categories: persistent memory
excerpt: gaga
---

### Part 0

This is a study note for [this page](http://pmem.io/2015/06/12/pmem-model.html).

Nowadays, there are two kinds of memory:

- fast, byte addressable, volatile memory
- slower, persistent, block storage

This library, combined with the right hardware (NVDIMMs---non-volatile dual-in-line memory module), provides a way of utilizing a third type of memory: fast, byte addressable and persistent.

It's not necessary to run this library with a real persistent memory in the platform, all that is really necessary is a device with a file system (performance will be different).

 

### Part 1 - accessing the persistent memory

##### Memory pool

Persistent memory is exposed by the OS as memory-mapped files, which is called pools.

![img](/assets/images/screenshot66.png)

 

##### Persistent pointers

```c++
typedef struct pmemoid { 
        uint64_t pool_uuid_lo; 
        uint64_t off; 
} PMEMoid;
```

In a very basic view, a regular pointer is a number of bytes between the start of the virtual address space to the beginning of the thing it points to. The persistent pointer is twice the size of a regular pointer and contains the **offset from the start of the pool** and **unique id of the pool**.

The pool id is used to figure out where the pool is currently mapped (because the actual address of the memory mapped region can be different each time the application started).

If the virtual address the pool is mapped at is known, we can get the direct pointer by performing a simple addition:

```c++
(void *)((uint64_t)pool + oid.off)
```

All open pools are stored in a [cuckoo hash table](http://en.wikipedia.org/wiki/Cuckoo_hashing) with 2 hashing functions.

 

##### The root object

The known location you can always look for in the memory pool is the root object. It's the anchor to which all the memory structures can be attached. The root object is **initially zeroed**.

```c++
PMEMoid root = pmemobj_root(pop, sizeof (struct my_root));
```

The root object is allocated from the pool and when an in-place reallocation is impossible a new object will be created with a different offset. So **be careful when trying to store the root persistent pointer** because it might change.

 

##### Safely storing data

When creating programs that write to persistent memory we have to be extra careful to make sure that the application is always in a state we can recognize and use, regardless of the exact moment is was interrupted.

Consider the following code:

```c++
void set_name(const char *my_name) {
memcpy(root->name, my_name, strlen(my_name));
}
```

If the application crashes in the middle of the copying or if cacheline didn't get flushed in time, we might not get what we want.

It should be written like this

```c++
void set_name(const char *my_name) {
root->length = strlen(my_name);
pmemobj_persist(&root->length, sizeof (root->length));
pmemobj_memcpy_persist(root->name, my_name, root->length);
}
```

The <span style="color: #0000ff;">_persist </span> suffixed functions make sure that the range of memory they operate on is flushed from the CPU and safely stored on the medium, whatever that might be.

```c++
void pmemobj_persist(PMEMobjpool *pop, const void *addr,
	size_t len);
```

<span style="color: #0000ff;">pmemobj_persist()</span> forces any changes in the range [*addr*, *addr*+*len*) to be stored durably in persistent memory.

The fundamental principle is that, on the current hardware architecture, only 8 bytes of memory can be written in an **atomic** way(原子操作). So this is correct:

```c++
root->u64var = 123;
pmemobj_persist(&root->u64var, 8);
```

But this is wrong:

```c++
root->u64var = 123;
root->u32var = 321;
pmemobj_persist(&root->u64var, 12);
```

#####  

 

##### Example

```c++
/*
 * layout.h -- example from introduction part 1
 */


#define LAYOUT_NAME "intro_1"
#define MAX_BUF_LEN 10

struct my_root {
	size_t len;
	char buf[MAX_BUF_LEN];
};

/*
 * writer.c -- example from introduction part 1
 */

#include <stdio.h>
#include <string.h>
#include <libpmemobj.h>

#include "layout.h"

int
main(int argc, char *argv[])
{
	if (argc != 2) {
		printf("usage: %s file-name\n", argv[0]);
		return 1;
	}

	PMEMobjpool *pop = pmemobj_create(argv[1], LAYOUT_NAME,
				PMEMOBJ_MIN_POOL, 0666);

	if (pop == NULL) {
		perror("pmemobj_create");
		return 1;
	}

	PMEMoid root = pmemobj_root(pop, sizeof(struct my_root));
	struct my_root *rootp = pmemobj_direct(root);

	char buf[MAX_BUF_LEN] = {0};
	if (scanf("%9s", buf) == EOF) {
		fprintf(stderr, "EOF\n");
		return 1;
	}

	rootp->len = strlen(buf);
	pmemobj_persist(pop, &rootp->len, sizeof(rootp->len));

	pmemobj_memcpy_persist(pop, rootp->buf, buf, rootp->len);

	pmemobj_close(pop);

	return 0;
}

/*
 * reader.c -- example from introduction part 1
 */

#include <stdio.h>
#include <string.h>
#include <libpmemobj.h>

#include "layout.h"

int
main(int argc, char *argv[])
{
	if (argc != 2) {
		printf("usage: %s file-name\n", argv[0]);
		return 1;
	}

	PMEMobjpool *pop = pmemobj_open(argv[1], LAYOUT_NAME);
	if (pop == NULL) {
		perror("pmemobj_open");
		return 1;
	}

	PMEMoid root = pmemobj_root(pop, sizeof(struct my_root));
	struct my_root *rootp = pmemobj_direct(root);

	if (rootp->len == strlen(rootp->buf))
		printf("%s\n", rootp->buf);

	pmemobj_close(pop);

	return 0;
}
```

Functions:

- <span style="color: #0000ff;">pmemobj_create</span>

  Creates a pool. It takes the usual paramaters, and creates a file pus a layout. A layout is a string of your choosing that identifies the pool.

  ```c++
  PMEMobjpool *pmemobj_create(const char *path, const char *layout, size_t poolsize, mode_t mode);
  ```

- <span style="color: #0000ff;">pmemobj_open</span>

   Opens existing pools. Requires that the layout passed to it matches the one the pool was created with.

  ```c++
  PMEMobjpool *pmemobj_open(const char *path, const char *layout);
  ```

- <span style="color: #0000ff;">pmemobj_close</span>

   Release the pool when it is no longer used.

  ```c++
  void pmemobj_close(PMEMobjpool *pop);
  ```

- <span style="color: #0000ff;">pmemobj_check</span>

   Verifies if all the required metadata is consistent.

  ```c++
  int pmemobj_check(const char *path, const char *layout);
  ```

- <span style="color: #0000ff;">pmemobj_direct</span>

   Takes the PMEMoid (persistent pointer) and turns it into a regular one that can be dereferenced.

  ```c++
  void *pmemobj_direct(PMEMoid oid);
  ```

- <span style="color: #0000ff;">pmemobj_persist</span>

   forces any changes in the range [addr, addr+len) to be stored durably in persistent memory.

  ```c++
void pmemobj_persist(PMEMobjpool *pop, const void *addr, size_t len);
  ```