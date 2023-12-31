## Material for the course:
# An introduction to parallel programming using Message Passing with MPI
## Authors: Joachim Hein, Soheil Soltani, Pedro Ojeda, and Xin Li


* Load the correspoding modules for the compilers depending on the
system that you are using

* Compile the C examples by running the following command on the
terminal:
    make allc 

* Compile the Fortran examples by running the following command on the
terminal:
    make allf

* Clean out the executables
    make clean

* Running scritps:
    sbatch run.sh integration2D_c 10000

* 2D integration background

In this example you will calculate the double integral:

<img src="https://render.githubusercontent.com/render/math?math=\int_{0}^{\pi}\int_{0}^{\pi} \sin(x %2B y) dx dy">

One way to parallelize this calculation is as follows. We start by discretizing the 
range in both *x* and *y* into bins of equal size. This will create a grid of *nxn* bins 
where *n* is an input parameter provided by the user. We can parallelize the code in one
direction only, for instance in the *x* direction. In this case, the first bin in *x* will
be computed by process 1, the second bin by process 2, and so on up to the process *N*.
This procedure will be repeated until the last bin *n* is computed, see the figure below.

```
y
|
| Pi
|   |   |   |   |   |   |   |   |    | 
|   |   |   |   |   |   |   |   |    | 
|   |   |   |   |   |   |   |   |    | 
|   |   |   |   |   |   |   |   |    | 
| 1 | 2 |...| N | 1 | 2 |...| N |....|  
|   |   |   |   |   |   |   |   |    | 
|   |   |   |   |   |   |   |   |    | 
|   |   |   |   |   |   |   |   |    | 
|   |   |   |   |   |   |   |   |    | 
|   |   |   |   |   |   |   |   |    | 
---------------------------------------------->
                                     Pi       x
```

Notice that during the computation of each bin in *x* you can perform the regular
(not parallel) integration in *y* over the entire range 
<img src="https://render.githubusercontent.com/render/math?math=0<y<\pi">. 
As a first approximation, we can take the volume generated by each grid element
(i,j) being equal to <img src="https://render.githubusercontent.com/render/math?math=\sin(x_{i} %2B y_{j}) dx dy">,
with <img src="https://render.githubusercontent.com/render/math?math=dx=dy=\pi/n"> and 
<img src="https://render.githubusercontent.com/render/math?math=x_{i},y_{j}"> being the center 
of the grid element. Each process can accumulate a local value of the integral which
can then be reduced into a total integral value.

This problem is useful for two reasons, first we know the exact value of the integral
(0) and second because it includes a double integral, the computation is heavier 
enough to detect the effects of a parallel implementation.




