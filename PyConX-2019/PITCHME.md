
## Meet dask and distributed the unsung heroes of Python Scientific data ecosystem

Alessandro Amici, B-Open, Rome

@css[regular](@fa[twitter] @alexamici<br>@fa[github] @alexamici<br>@fa[firefox] http://bopen.eu)

@snap[south small]
PyConX, 2019-05-03, Florence.
@snapend

---

## Scientific data

+++

### Scientific data fields

@ul

- Data science / Big data
  - Analytics, Finance, Research & Development...
  - Based on *records*
  - Prevalent technologies: R, JAVA, Hadoop, Spark...
- Scientific computing
  - Physics, Engineering, Biology...
  - Based on *hyper-cubes*
  - Prevalent technologies: FORTRAN, C / C++, MATLAB, HPC / MPI...

@ulend

+++

### Key features of the technologies

@ul

- Fast computing run-time
- Memory-efficient data types
- Mature eco-systems and communities
  - Dedicated tools & libraries
  - Distributed computing of big data

@ulend

+++

### Who in their right mind would use Python???

+++

### Apparently a lot of people

![growth_major_languages](assets/growth_major_languages.png)

---

## Python for scientific data

+++

### How is Python even a contender?

@ul

- Fast computing run-time
- **Apparently NOT!** Interpreted without a JIT compiler
- Memory-efficient data types
- **Apparently NOT!** Big native types and garbage collected
- Mature eco-systems and communities
  - Dedicated tools & libraries
    - **YES!** Numpy, Pandas, xarray, Dask...
  - Distributed computing of big data
    - **YES!** Dask.distributed...

@ulend

---

## Python for *small* scientific data 

+++

### It's all about dedicated data types (and bindings)

@ul

- `numpy.ndarray`, `pandas.DataFrame`...
  - C-level data types and methods
  - Compact in-memory representation of data
  - High-level views classes
  - Fast C-level computations of a lot of operations
  - Tons of efficient tools including bindings to most C / C++ / FORTRAN libraries
- remaining issues are mostly about *RAM* management
  - Python copies data a lot
  - RAM management is mostly automatic

@ulend

+++

### The real limit of Python in science

@ul

- Python is not slow
- Python is a memory hog!
- In real life, you will get your scientific computation terminated by the OOM killer more often than by any timeout

@ulend

---

## Python for *big* scientific data 

+++

### When is your data actually *big*?

@ul

- Your data is *big* when it doesn't fit into the *memory* of the *larger* machine you *have*.
- In a lot of cases *buying access* to a machine with *enough memory* is the most efficient way to solve your *big* data problems.

@ulend

+++

### It's all about blocks (and tasks)

Dask is a flexible library for parallel computing

@ul

- Scalable block-based data types
  - `dask.array`, `dask.dataframe`
- Scalable block-based operations
  - divided into tasks operating on blocks
  - tasks are arranged into a task graph
  - task graphs can be optimised
- Dynamic task scheduling
  - configurable schedulers
  - lazy and parallel computation of tasks

@ulend

+++

### Block-based data types

<div class="left">

![dask-array](assets/dask-array-black-text.png)

</div>

<div class="right">

![dask-array](assets/dask-array-black-text.png)

</div>

+++

### Simple MapReduce task graph

![array-simple](assets/array-1d-sum.png)

```
>>> import dask.array as da
>>> x = da.ones((150000,), chunks=(50000,))
>>> x.sum()
```

+++

### Complex task graph

![array-complex](assets/array-xdotxT-mean-std.png)

```
>>> x = da.ones((15, 15), chunks=(5, 5))
>>> y = (x.dot(x.T + 1) - x.mean()).std()
```


+++

### Task schedulers

![dask-schedulers](assets/collections-schedulers.png)

- Synchronous - for testing
- Threaded and Multiprocessing - single machine
- Distributed - multiple machines

+++

### Dask in action

```
>>> X = dask.array.random((100000, 100000), chunks=(1000, 1000))
>>> (X + X.T) - X.mean(axis=0)
```

![dask-in-action](assets/grid_search_schedule.gif)

---

## Thank you!

Alessandro Amici, B-Open, Rome

@css[regular](@fa[twitter] @alexamici<br>@fa[github] @alexamici<br>@fa[firefox] http://bopen.eu)

@snap[south]
@css[regular](Slides: https://gitpitch.com/alexamici/talks)
@snapend