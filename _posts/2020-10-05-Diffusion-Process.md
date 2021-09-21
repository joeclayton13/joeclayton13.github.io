---
layout: post
mathjax: true
title: A Discrete Diffusion Process
categories: Physics
---

The goal of this post is to simulate a discrete diffusion process. We are given a 2-dimensional grid with points $(i,j)$, such that $i,j\in \{1,\dots, N \}$, and start with an initial distribution $u_0 (i,j)$ of function valus on the grid points. The distribution process follows the following recurrence relation: 

$$
u_{n+1}(i,j) = \frac{1}{4}[u_n(i+1, j) + u_n(i-1, j) + u_n(i, j+1) + u_n(i, j-1)]
$$

The following boundary conditions are also applied:
    
$$
u_n(i, 0) = u_0(i, 0) \;\;\;\;\;\; u_n(i, N+1) = u_0(i, N+1) \;\;\;\;\;\; u_n(0, j) = u_0(0, j) \;\;\;\;\;\; u_n(N+1, j) = u_0(N+1, j)
$$