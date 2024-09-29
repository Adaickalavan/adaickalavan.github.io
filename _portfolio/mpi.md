---
title: "Parallel Programming in Python with MPI"
excerpt: "MPI, Python"
header:
  teaser: /assets/images/mpi/servers.png
date: "2024-09-01" 
---

## Introduction

We explore parallel computation in Python using MPI, via `mpi4py` package. Read the docs of [`mpi4py`](https://mpi4py.readthedocs.io/en/stable/). MPI, stands for message passing interface, and it enables distributed memory parallelization.

Executing `mpiexec -n <num_processes> python3.10 <file_name>` runs the code on multiple independent processes (i.e., ranks) and adds them to an MPI communicator, which enables the processes to send and receive information to one another via MPI. 

The `mpi4py` package provides uppercase and lowercase communication methods. Lowercase methods like `comm.send()` use Python’s pickle module to transmit objects in a serialized manner, whereas uppercase versions like `comm.Send()` transmits data contained in a contiguous memory buffer, as featured in the MPI standard.

The following tools will be used in this project:
+ MPI (`mpi4py`)
+ Python

## Learning Outcome

Find the source code in the [repository](https://github.com/Adaickalavan/mpi).

At the end of this project, we should be able to:
+ Use MPI in Python for parallel programming in distributed memory applications. 

## Project Structure

The project structure is as follows:

```text
mpi                            # Repository root
├── pi_montecarlo.py           # Monte Carlo estimate of Pi value
├── .gitignore
├── LICENSE                          
├── Makefile                   # To format the code
└── README.md                                 
```

## Installation
Install OpenMPI in Ubuntu system.
```bash
$ cd <path>/mpi
$ python3.10 -m venv ./.venv
$ source ./.venv/bin/activate
$ pip install --upgrade pip
$ pip install black[jupyter] isort
$ sudo apt-get install libopenmpi-dev
$ pip install mpi4py
```

Run the program.
```bash
$ cd <path>/mpi
$ mpiexec -n <num_processes> python3.10 <file_name>
# For example
$ mpiexec -n 4 python3.10 pi_montecarlo.py
```

## Examples

1. [Monte Carlo Pi](https://github.com/Adaickalavan/mpi/blob/main/pi_montecarlo.py)
    
    The value of $\pi$ can be calculated using MonteCarlo method. Consider a unit radius (i.e., $r = 1$) circle inscribed inside a square with sides of length 2r. Area of circle, area of square, and their ratios are as follows. 

    $$
    \begin{align*}
    A_{circle} &= \pi r^2 \\
    A_{square} &= 2r \times 2r = 4r^2 \\
    ratio_{area} &= \frac{A_{circle}}{A_{square}} = \frac{\pi}{4}
    \end{align*}
    $$

    Randomly sample N uniform samples inside the square. Probability of sampling the circle, square, and their ratio are as follows.
    
    $$
    \begin{align*}
    P_{circle} &= \frac{N_{circle}}{N} \\
    P_{square} &= 1 \\
    ratio_{probability} &= \frac{P_{circle}}{P_{square}} = \frac{N_{circle}}{N}
    \end{align*}
    $$

    Here, $N_{circle}$ is the number samples which fell inside the circle. A sample is inside the circle if $x^2 + y^2 <= r^2$, where $x$ and $y$ are the coordinates of the sample which are evenly distributed from -r to r.

    Since ratio of sampling probabilities equals ratio of areas, we have:

    $$
    \begin{align*}
    ratio_{area} &= ratio_{probability} \\
    \frac{\pi}{4} &= \frac{N_{circle}}{N} \\
    \pi &= \frac{4 * N_{circle}}{N} 
    \end{align*}
    $$

    Accuracy of computed $\pi$ increases with $N$. As a point of reference, the first 7 digits of the true value of $\pi$ is 3.141592 .

    In the given code, each rank works on a subset of $N$ samples. Then, the results of each rank are summed into a single value in the root rank for final processing.

    {: .notice--warning}
    It is imperative that the pseudo random number sequence generated in each rank are independent and do not overlap. Overlapping sequences lead to over-sampling and bad statistical properties. We could generate a large set of random numbers in one process and broadcast to each rank, but this may cause memory issues for large arrays.