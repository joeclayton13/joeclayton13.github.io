---
layout: post
mathjax: true 
title: Visualising the Mandelbrot Set
categories: Mathematics
tags: python
---


### Definition ###
The Mandelbrot set is defined as the set of all values $c$ in the complex plane where the recursion relation 
$$
z_{n+1} = z_n^2 + c
$$
is bounded as $n \to \infty $, and $ z_0 = 0 $. That is, a point $c$ is in the Mandelbrot set if $ |z_n | \leq 2 $ for all $n >0$.


### Code ###

<details>
<summary>Mandelbrot Code</summary>
<p>

```python
@njit
def mandelbrot(cx, cy, max_iters): 
    """
    Checks if a complex number (represented by pixel (x, iy)) is in the mandelbrot set
    calculates zn+1 = zn^2 + c
    cx: Re(c)
    cy: Im(c)
    """
    
    ### Starting at the point (0,0i),
    z = complex(0)
    c = complex(cx, cy)
    
    for i in range(max_iters): 
        
        # Mandelbrot Condition to break out 
        if abs(z) > 4: 
            # If not in Mandelbrot, we assign it black colour
            return 0
            
        # Update
        z = z**2 + c

    # if in Mandelbrot, we give it white colour
    return 255
```
</p>
</details>


<details>
<summary> Code to Evaluate the Mandelbrot Function </summary>
<p>

```python
def eval_mandelbrot(height, width, x_start, y_start, x_end, y_end, max_iters:int): 
    '''
    cx: Re(c)
    cy: Im(c)
    '''
    x = np.linspace(x_start, x_end, width)
    y = np.linspace(y_start, y_end, height)

    result = np.zeros((width, height))
    for i, cx in enumerate(x): # Rows
        for j, cy in enumerate(y): 
            res = mandelbrot(cx,cy, max_iters)
            result[i,j] = res
            
    return result
```
</p>
</details>


The above function generates the following static image

![](/Images/Mandelbrot/Mandelbrot.png?raw=true)

It's cool to zoom in and see how complex the Mandelbrot set can be. 


It's also interesting to see how the Mandelbrot set forms as the number of iterations increases. 