
## Meet dask and distributed the unsung heroes of Python Scientific data ecosystem

Alessandro Amici, B-Open, Rome

@css[regular](@fa[twitter] @alexamici<br>@fa[github] @alexamici<br>@fa[firefox] http://bopen.eu)

@snap[south small]
PyConX, 2019-05-03, Florence.
@snapend

---

## Scientific data

+++

### Data size / computing power classification

@ul

- Data size
  - small: fits into RAM
  - medium: fits into storage, but not into RAM
  - big: needs distributed storage
- Computing power
  - synchronous: single-core
  - parallel: multi-core on single-machine
  - distributed: multi-core on multi-machines

@ulend

+++

### Buy access to bigger machines!!!

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

### How is Python even a contender?

@ul

- Fast computing run-time
- **Apparently NOT!** Interpreted without a JIT compiler
- Memory-efficient data types
- **Apparently NOT!** Big native types and garbage collected
- Mature eco-systems and communities
  - Dedicated tools & libraries
    - **YES!** Numpy, Pandas, Xarray, Dask...
  - Distributed computing of big data
    - **YES!** Dask.distributed, Airflow...

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
- remaining issues
  - No native support for parallel execution
  - Lots of data copies in RAM and garbage collector

@ulend

+++

### The real limit of Python in science

@ul

- When used properly
  - Python is not (that) slow
  - But it needs explicit parallelism
  - And it is a memory hog!

@ulend

---

## Python for *medium* scientific data

+++

### Enter Dask

Dask is a flexible library for parallel computing

URL: https://dask.org/

Maintainer: Matthew Rocklin at NVIDIA

Github: https://github.com/dask/dask

+++

### It's all about blocks (and tasks)

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

A `dask.array` is made of `numpy.ndarray` blocks

![dask-array](assets/dask-array-black-text.png)

</div>

<div class="right">

A `dask.dataframe` is made of `pandas.DataFrame` blocks

![dask-array](assets/dask-dataframe.png)

</div>

+++

### Simple MapReduce-like task graph

![array-simple](assets/array-1d-sum.png)

```:python
>>> import dask.array as da
>>> x = da.ones((150000,), chunks=(50000,))
>>> x.sum()
```

+++

### Complex task graph

![array-complex](assets/array-xdotxT-mean-std.png)

```:python
>>> x = da.ones((15, 15), chunks=(5, 5))
>>> y = (x.dot(x.T + 1) - x.mean()).std()
```

+++

### Task schedulers

![dask-schedulers](assets/collections-schedulers.png)

- Synchronous - single-core
- Threaded and Multiprocessing - multi-core on single-machine
- Distributed - multi-core on multi-machines (see later)

+++

### Dask in action

```:python
>>> X = dask.array.random((100000, 100000), chunks=(1000, 1000))
>>> (X + X.T) - X.mean(axis=0)
```

![dask-in-action](assets/grid_search_schedule.gif)

+++

### What's the magics of Dask?

@ul

- Dask data types implement the native protocols
- Block-based operations and data access
- Dask-aware code only builds the task graph
- Non-Dask-aware operations trigger the execution *only* of the needed tasks
- A few tuning knobs and monitoring tools
- All with minimal code changes!

@ulend

---

## Python for *big* scientific data 

+++

### Enter Dask.distributed

Dask.distributed is a lightweight library for distributed computing in Python.

@ul

- Single scheduler / Many workers
- Multi-core culsters and experimental multi-GPU clusters
- Peer-to-peer data sharing
- Data Locality
- Monitoring application
- All with minimal code changes!

@ulend

+++

### Dask.distributed network structure

![network](assets/network-inverse.png)

- Scheduler is a *pet* service
- Warkers are *cattle* services

+++

### What's the magics of Dask.distributed?

@ul

- Distributed computing with minimal fuss
  - Easy cluster setup
  - Easy code changes in simple cases
- Can use all of the cluster RAM

@ulend

---

## Real world use case: Climate Change

+++

### Has the climate changed?

@ul

- Data describing past data
  - Climate Data Store - http://cds.climate.copernicus.eu
- Choose a reference period and compute the reference *climatology*
  - monthly temperature average for 1980-2010
- Compute the *anomaly* with respect to the reference
  - monthly temperature anomaly for 2018

@ulend

+++

### Climate data size

ERA5: high-resolution reanalysis of climate from 1979

![ERA5-sizes](assets/ERA5-sizes.png)

+++

### Compute temperature anomaly with xarray

```:python
import xarray as xr

tas = xr.open_mfdataset('ERA5-*.nc').t2m

# Choose a reference period and compute the reference climatology 
tas_ref = tas.sel(time=slice('1980', '2010'))
tas_ref_clima = tas_ref.groupby('time.month').mean()

# Compute the anomaly with respect to the reference
tas_2018 = tas.sel(time='2018')
tas_2018_anomaly = tas_2018.groupby('time.month') - tas_ref_clima

# No computation until here!
tas_2018_anomaly.plot(col='time', col_wrap=4)
```

+++

### Temparature anomaly for 2018

![tas-anomaly](assets/tas-anomaly.png)

---

## Thank you!

Alessandro Amici, B-Open, Rome

@css[regular](@fa[twitter] @alexamici<br>@fa[github] @alexamici<br>@fa[firefox] http://bopen.eu)

@snap[south]
@css[regular](Slides: https://gitpitch.com/alexamici/talks)
@snapend