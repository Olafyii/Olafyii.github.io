---
layout: post
date: 2019-05-05 15:16:00 +0800
title: Lated Tips
catalog: true
header-style: text
tags:
    - cheatsheet
---



- equation align:

```latex
\usepackage{amsmath}
\begin{equation}
\begin{split}
    &\gamma ( I ) = \sum _ { t = 1 } ^ { T } \gamma _ { t } \\
    &=\left[ \sum _ { t = 1 } ^ { T } \gamma _ { 1 } \left( x _ { t } \right) , \sum _ { t = 1 } ^ { T } \gamma _ { 2 } \left( x _ { t } \right) , \ldots , \sum _ { t = 1 } ^ { T } \gamma _ { N } \left( x _ { t } \right) \right]
\end{split}
\end{equation}
```

- Add table quicker: use excel2latex
- Super link:

```latex
\usepackage{hyperref}
\hypersetup{
    colorlinks=true,
    linkcolor=blue,
    filecolor=magenta,      
    urlcolor=cyan,
}
\href{http://www.kanglw.com/}{Liwei's homepage}
```

- reference

- ```latex
  \bibliography{name} % then add reference in name.bib, then \cite{...}
  ```