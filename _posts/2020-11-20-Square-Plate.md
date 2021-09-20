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

We would like to find time \\( t = t^{'} \\) such that \\( u(t^{'}) = 1 \\) at the center of the plate.

We are given the correct time \\(t^{'} = 0.424011387033 \\). 

To do this, we will implement a finite difference scheme with both forward and backward time-stepping. 


### Solution ###

First, we note that the temperature of the plate obeys the heat equation: 
$$
\frac{\partial u}{\partial t} = \alpha \Delta u
$$
where $\Delta$ is the standard Laplacian operator (we work in coordinates \\( \{x, y, t\}\\)). Furthermore, \\( \alpha \\) is some positive coefficient of the medium. We will use \\( \alpha = 1\\) for simplicity. 

We denote \\( u(x,y,t) = u_{i,j}^{k}\\) where \\( i, j\\) represent the spatial coordinates and \\(k\\) represents the time-step. 

We can solve this problem using two different implementations: a forward Euler method (both CPU and GPU) and a backward Euler method (CPU). 


### Implementation 1 - Forward Euler Method

The forward difference is given by 

$$
\frac{du}{dx} \approx \frac{u_{i+1} - u_{i}}{h}
$$

Further, second-order derivatives are approximated by: 

$$
\frac{d}{dx} \left [\frac{d}{dx} u \right] =
\frac{u_{i+1} - 2u_{i} - u_{i-1}}{h^2}
$$

Applying these two relations to the heat equation, we get: 

$$
\frac{u_{i,j}^{k+1} - u_{i,j}^{k}}{\Delta t} 
= 
(\frac{u_{i+1,j}^{k} - 2u_{i,j}^{k} - u_{i-1,j}^{k}}{h^2}
+ 
\frac{u_{i,j+1}^{k} - 2u_{i,j}^{k} - u_{i,j-1}^{k}}{h^2})
$$

We are able to obtain our final equation:

$$
u_{i,j}^{k+1} = (1-4C)u_{i,j}^{k} + C(u_{i+1,j}^{k} + 
u_{i-1,j}^{k} + u_{i,j+1}^{k} +
u_{i,j-1}^{k})
$$

where \\( C = \frac{\Delta t}{h^2} \\). 

This method is stable if: \\( \Delta t \leq \frac{h^2}{4} \\). 


### A CPU Implementation ###

First we have a standard CPU computing method for the Euler forward method. We have used NUMBA's parallel processing to accelerate the performance. 

{::options parse_block_html="true" /}
<details>
    <summary markdown="span">
        Let's see some code!
    </summary>
    ```python
    print('Hello World!')
    ```
</details>
<br/>

{::options parse_block_html="false" /}

### A GPU Implementation ###

We also present a GPU computing method for the Euler forward scheme. In this method, we use a GPU kernel which is very similar to Assignment 3. The CPU controls each time-step iteration, while the GPU performs the computation of the square plate for each time-step. 


### Implementation 2: Backward Euler Method

The backward difference is given by: 

$$
\frac{du}{dx} \approx \frac{u_{i} - u_{i-1}}{h}
$$

Further, second-order derivatives are approximated by: 

$$
\frac{d}{dx} \left [\frac{d}{dx} u \right] =
\frac{u_{i+1} - 2u_{i} + u_{i-1}}{h^2}
$$

Applying these two relations to the heat equation, we get: 

$$
\frac{u_{i,j}^{k} - u_{i,j}^{k-1}}{\Delta t} 
=
(\frac{u_{i+1,j}^{k} - 2u_{i,j}^{k} + u_{i-1,j}^{k}}{h^2}
+ 
\frac{u_{i,j+1}^{k} - 2u_{i,j}^{k} + u_{i,j-1}^{k}}{h^2})
$$

Therefore, we get our final equation of

$$
u_{i,j}^{k-1} =
(1 + 4C) u_{i,j}^{k} - 
C (u_{i+1,j}^{k} + u_{i-1,j}^{k} + u_{i,j+1}^{k} + u_{i,j-1}^{k} )
$$

where \\( C = \frac{\Delta t}{h^2} \\).

We note that this method is unconditionally stable. 

Unlike the forward method, we cannot use this equation alone. We note that we may express the same equation as a time-dependent matrix equation: 

$$
u^{k-1} = A u^{k}
$$

Then applying the inverse matrix on both sides, we get:

$$
A^{-1} u^{k-1} = u^{k}
$$


### A CPU Implementation ###

In this implementation, we require that we generate the sparse matrix A. We solve the matrix equation using ``spsolve`` in every time iteration to find the next configuration of the plate. 