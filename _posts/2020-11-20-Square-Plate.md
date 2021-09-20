---
layout: post
mathjax: true
title: Heating a Square Plate
categories: Physics
---

### Problem Statement ### 
We will consider a square plate with sides \\( \left[ -1,1 \right] \times \left[ -1,1 \right]\\). We have the following initial conditions at time $t=0$: 

*   The temperature of 3 sides is  $u_0=0$.
*   The temperature of the fourth side is $u_0=5$.

We would like to find time \\(t = t^{*} \\) such that \\( u(t^{*}) = 1 \\) at the center of the plate.

We are given the correct time \\(t^{*} = 0.424011387033 \\). 

To do this, we will implement a finite difference scheme with both forward and backward time-stepping. 