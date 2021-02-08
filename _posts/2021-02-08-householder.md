---
title:  "Householder Transformation"
date:   2021-02-08
categories: blog
tags:
  - linear algebra
---

In the [previous post](blog/ai-roadmap) I said I've got some linear algebra background but it got me thinking. Can I implement e.g. an SVD from scratch? The answer is that certainly not from the top of my head. It sounds like a challenge that fits into this blog's purpose so let's do the reading, choose an achievable goal and implement something. Vague enough, huh? :)

Disclaimer: I'm not gonna shoot for mathematical rigorousness in the formulation and definitely won't aim for a very efficient implementation.

For me one of the most exciting lectures was [Analysis of Matrices](https://portal.vik.bme.hu/kepzes/targyak/VIMAD569/en/). I wanted to learn about the topic in a more comprehensive way than they taught during the undergraduate school. At the time I didn't own the suggested textbook but some years ago, for nostalgic reasons I guess, I managed to buy a second-hand copy of _Rózsa Pál: Lineáris algebra és alkalmazásai, Műszaki könyvkiadó, Budapest, 1974_. Yeah, this is Hungarian, sorry. The 3rd edition is available somewhere online I think.

For reference, it was section 11.3, "numerical solutions for the eigenvalue problem" where I started reading. I didn't get too far because I decided to start to implement the Householder transformation. It's the first step in the book to determine the eigenvalues and eigenvectors of a symmetric matrix A.

