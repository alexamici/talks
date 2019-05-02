
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

@ul

- Apparently everybody
- ![growth_major_languages](assets/growth_major_languages.png)

@ulend

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
- In a lot of cases *buying* a machine with *enough memory* is the most efficient way to solve your *big* data problems.

@ulend

+++

### It's all about... Dask

@ul

- Dask is a flexible library for parallel computing in Python
  - Dynamic task scheduling
  -  data types

@ulend

---

## Temp

+++

<div class="left">

@ul

- ecCodes bindings via CFFI for Python 3 and PyPy
- GRIB-level API: *FileStream*, *FileIndex* and *Message*
- ‎‎CDM-level API: *Dataset* and *Variable*, inspired to *h5netcdf* and *netCDF4-Python*
- *xarray* read-only backend
- ... and more

@ulend

</div>

<div class="right">
![cfgrib-pypi](assets/cfgrib-pypi.png)
</div>

---

## Thank you!

Alessandro Amici, B-Open, Rome

@css[regular](@fa[twitter] @alexamici<br>@fa[github] @alexamici<br>@fa[firefox] http://bopen.eu)

@snap[south]
@css[regular](Slides: https://gitpitch.com/alexamici/talks)
@snapend