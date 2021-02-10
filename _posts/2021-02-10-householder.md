---
title:  "Householder Transformation"
date:   2021-02-10
categories: blog
tags:
  - linear algebra
classes: wide
---

In the [previous post](blog/ai-roadmap), I said I've got some linear algebra background, but it got me thinking. Can I implement e.g. an SVD from scratch? The answer is that certainly not from the top of my head. It sounds like a challenge that fits into this blog's purpose so let's do the reading, choose an achievable goal and implement something. Vague enough, huh?

Disclaimer: I'm not going to shoot for mathematical rigorousness in the formulation and definitely won't aim for a very efficient implementation.

For me, one of the most exciting lectures was [Analysis of Matrices](https://portal.vik.bme.hu/kepzes/targyak/VIMAD569/en/). I wanted to learn about the topic in a more comprehensive way. At the time, I didn't own the suggested textbook. Some years ago, though, for nostalgic reasons I guess, I managed to buy a second-hand copy of _Rózsa Pál: Lineáris algebra és alkalmazásai, Műszaki könyvkiadó, Budapest, 1974_. Yeah, this is Hungarian, sorry. The 3rd edition is available somewhere online, I think.

For reference, it was section 11.3, "numerical solutions for the eigenvalue problem "where I started reading. I didn't get too far because I decided to start to implement the Householder transformation. It's the first step in the book to determine the eigenvalues and eigenvectors of a symmetric matrix A.

Given a symmetric matrix \\(A\\), we're to find an orthogonal matrix \\(H\\) so \\(HAH^T\\) is a tridiagonal matrix. We'll see later why tridiagonality is useful (TODO).

Just a reminder that we like orthogonal matrices, i.e. \\(Q\\) matrices that satisfy \\(Q^{-1}=Q^T\\) because transforming "things" with them is "revertable", "easy to revert" and they also preserve distances. So if there's a representation of \\(X\\) in an orthogonal basis \\(Q\\) where a particular computation is easier done, due to the properties of \\(QX\\), then let's do the calculation on \\(QX\\) and "transform back" the result to \\(X\\)'s space with \\(Q^{-1}\\) which is just the \\(Q^T\\) transpose.

The Householder transformation suggests a series of reflection transformations, parametrized by the normal vector \\(u\\) of the reflective plane to map a particular vector \\(a\\) so, its reflection has a limited number of non-zero components.

Such a reflection matrix \\(H_i\\) is

$$H_i = I - 2u u^T$$

where

$$u=\frac{1}{d}
\begin{bmatrix}
0 \\ a_2 \pm b \\ a_3 \\ \vdots \\ a_n
\end{bmatrix}$$

$$b^2 = \sum^{n}_{k=2} a_k^2$$

$$d = \sqrt{(a_2 \pm b)^2 + a_3^2 + \dots + a_n^2}$$

This construction will bring \\(a\\) to \\(a'=H_ia\\), only the first two components of \\(a'\\) are non-zero.

$$a'= \begin{bmatrix}
c_1 \\ c_2 \\ 0 \\ \vdots \\ 0
\end{bmatrix}$$

The process is to take the first column of \\(A\\) as \\(\[a_1 a_2 \dots a_n\]^T\\), calculate the corresponding \\(u\\) and the reflective transformation \\(H_1\\) from that. Since \\(A\\) is symmetric, \\(H_1AH_1\\) will clear the first row and the first column too, except for the first two components both in the row and the column.

We can repeat this for the lower-right \\((n-1)\times(n-1)\\) sub-matrix of \\(H_1AH_1\\) to determine \\(H_2\\). Applying to the previous result, \\(H_2H_1AH_1H_2\\) similarly clears the second row and column of \\(A\\).

And so on, \\(H_{n-2} H_{n-3} \dots H_1 A H_1 \dots H_{n-3} H_{n-2}\\) will leave a tridiagonal matrix. We've found \\(H=H_{n-2} H_{n-3} \dots H_1\\).

For the implementation, I first wrote a small matrix representation class just for fun. I believe it's easier to read the code built on top of those matrix operators and helper functions.

[Here's the implementation](https://github.com/balintfodor/linalg-sandbox/blob/fad934aa28f3ada808e8687b9a0c54b097b31f0a/src/householder.cpp).
