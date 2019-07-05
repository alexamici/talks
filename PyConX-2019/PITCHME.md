---?color=#F7F2D3
@snap[north span-100]
## Meet dask & distributed
### The unsung heroes of Python Scientific Data Ecosystem
<br>
Alessandro Amici, B-Open, Rome
<br>
@css[text-05](PyConX, 2019-05-03, Florence.)
@snapend

@snap[south-west span-30 text-07]
@fa[twitter] @alexamici
@snapend

@snap[south span-30 text-07]
@fa[github] @alexamici
@snapend

@snap[south-east span-30 text-07]
@fa[firefox] http://bopen.eu
@snapend

---

## Scientific data

+++

@snap[north span-100 h4-beige]
#### Data Size / Computing Power Classification
@snapend

<br>

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

## Buy access to bigger machines!!!

+++

@snap[north span-100 h4-beige]
#### Scientific Data Fields
@snapend

<br>

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

@snap[north span-100 h4-beige]
#### Key Features of the Technologies
@snapend

<br>

@ul

- Fast computing run-time
- Memory-efficient data types
- Mature eco-systems and communities
  - Dedicated tools & libraries
  - Distributed computing of big data

@ulend

+++

@snap[north span-100 h4-beige]
#### How is Python even a Contender?
@snapend

@ul[text-09](false)

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

---?color=linear-gradient(180deg, black 22%, #F7F2D3 22%)

@snap[north span-100 h2-beige]
## Python
@snapend

## For *small* scientific data

+++

@snap[north span-100 h4-beige]
#### It's all about dedicated data types (and bindings)
@snapend

<br>

@ul[text-09]

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

@snap[north span-100 h4-beige]
#### The real limit of Python in Science
@snapend

@ul

- When used properly
  - Python is not (that) slow
  - But it needs explicit parallelism
  - And it is a memory hog!

@ulend

---?color=linear-gradient(180deg, black 22%, #F7F2D3 22%)

@snap[north span-100 h2-beige]
## Python
@snapend

## For *medium* scientific data

+++?color=linear-gradient(180deg, black 22%, #F7F2D3 22%)

@snap[north span-100 h2-beige]
## Enter Dask
@snapend

Dask is a flexible library for parallel computing

https://dask.org and https://github.com/dask/dask

(mostly) by Matthew Rocklin at NVIDIA

+++

@snap[north span-100 h4-beige]
#### It's all about Blocks (and Tasks)
@snapend

<br>

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

@snap[north span-100 h4-beige]
#### Block-based Data Types
@snapend

@snap[west span-50 text-07]

A `dask.array` is made of `numpy.ndarray` blocks

![dask-array](assets/dask-array-black-text.png)

@snapend

@snap[east span-50 text-07 top-pad-4em]

A `dask.dataframe` is made of `pandas.DataFrame` blocks

![dask-array](assets/dask-dataframe.png)

@snapend

+++?color=#F7F2D3

@snap[north span-100]
#### Simple MapReduce-like task graph
@snapend

<br>

@img[span-28 bg-beige](assets/array-1d-sum.png)

```:python
>>> import dask.array as da
>>> x = da.ones((150000,), chunks=(50000,))
>>> x.sum()
```

+++?color=#F7F2D3

@snap[north span-100]
#### Complex Task Graph
@snapend

<br>

@img[span-85 img-shadow bg-beige](assets/array-xdotxT-mean-std.png)

```:python
>>> x = da.ones((15, 15), chunks=(5, 5))
>>> y = (x.dot(x.T + 1) - x.mean()).std()
```

+++

@snap[north span-100 h4-beige]
#### Task Schedulers
@snapend

<br>

![dask-schedulers](assets/collections-schedulers.png)

<br>

@ul[text-08](false)
- Synchronous - single-core
- Threaded and Multiprocessing - multi-core on single-machine
- Distributed - multi-core on multi-machines (see later)
@ulend

+++?color=#F7F2D3

@snap[north span-100]
#### Dask in Action
@snapend


```:python
>>> X = dask.array.random((100000, 100000), chunks=(1000, 1000))
>>> (X + X.T) - X.mean(axis=0)
```

![dask-in-action](assets/grid_search_schedule.gif)

+++

@snap[north span-100 h4-beige]
#### What's the magics of Dask?
@snapend

@ul

- Dask data types implement the native protocols
- Block-based operations and data access
- Dask-aware code only builds the task graph
- Non-Dask-aware operations trigger the execution *only* of the needed tasks
- A few tuning knobs and monitoring tools
- All with minimal code changes!

@ulend

---?color=linear-gradient(180deg, black 22%, #F7F2D3 22%)

@snap[north span-100 h2-beige]
## Python
@snapend

## For *big*<br>scientific data 

+++?color=linear-gradient(180deg, black 18%, #F7F2D3 18%)

@snap[north span-100 h3-beige]
### Enter Dask.distributed
@snapend

A lightweight library for distributed computing.

@ul[text-09](false)
<br>
- Single scheduler / Many workers
- Multi-core clusters and experimental multi-GPU clusters
- Peer-to-peer data sharing
- Data Locality
- Monitoring application
- All with minimal code changes!

@ulend

+++

@snap[north span-100 h4-beige]
#### Dask.distributed Network Structure
@snapend

<br>
![network](assets/network-inverse.png)

- Scheduler is a *pet* service
- Workers are *cattle* services

+++

@snap[north span-100 h4-beige]
#### What's the magic of Dask.distributed?
@snapend

@ul

- Distributed computing with minimal fuss
  - Easy cluster setup
  - Easy code changes in simple cases
- Can use all of the cluster RAM

@ulend

---

@snap[north span-100 h4-beige]
#### Real World Use Case
@snapend

## Climate Change

+++

@snap[north span-100 h4-beige]
#### Has the Climate changed?
@snapend

<br>

@ul

- Data describing past data
  - Climate Data Store: http://cds.climate.copernicus.eu
- Choose a reference period and compute the reference *climatology*
  - monthly temperature average for 1981-2010
- Compute the *anomaly* with respect to the reference
  - monthly temperature anomaly for 2018

@ulend

+++

@snap[north span-100 h4-beige]
#### Climate Data Size
@snapend

ERA5: high-resolution reanalysis of climate from 1979

![ERA5-sizes](assets/ERA5-sizes.png)

+++

@snap[north span-100]
#### Compute Temperature Anomaly with Xarray
@snapend

<br>

```:python
import xarray as xr

tas = xr.open_mfdataset('ERA5-*.nc', chunks={...}).t2m

# Choose a reference period and compute the reference climatology 
tas_ref = tas.sel(time=slice('1981', '2010'))
tas_ref_clima = tas_ref.groupby('time.season').mean('time')

# Compute the anomaly with respect to the reference
tas_2018 = tas.sel(time='2018')
tas_2018_seasons = tas_2018.groupby('time.season').mean('time')
tas_2018_anomaly = tas_2018_seasons - tas_ref_clima
```

@snap[south span-100 text-07 text-smallcaps]
Ready to crunch 1.4 Tb of data!
@snapend

+++

@snap[north span-100]
#### Hand-made Temperature Anomaly for 2018
@snapend

<br><br>

```:python
# Plotting triggers the computation
tas_2018_anomaly.plot(col='season', col_wrap=2)
```

@img[bg-beige](assets/tas-anomaly.png)

+++

@snap[north span-100 h4-beige]
#### Official Temperature Anomaly for 2018
@snapend

<br><br>

European State of the Climate 2018: http://climate.copernicus.eu/ESOTC

![official-tas-anomaly](assets/C3S_ESOTC18_General_European_temp_Fig4_branded_web.png)

---?color=#F7F2D3
@snap[north span-100 text-08]
Slides:<br>https://gitpitch.com/alexamici/talks
@snapend

## Thank you!

Alessandro Amici, B-Open, Rome

@snap[south-west span-30 text-07]
@fa[twitter] @alexamici
@snapend

@snap[south span-30 text-07]
@fa[github] @alexamici
@snapend

@snap[south-east span-30 text-07]
@fa[firefox] http://bopen.eu
@snapend
