
## Meet dask and distributed the unsung heroes of Python Scientific data ecosystem

Alessandro Amici, B-Open, Rome

@css[regular](@fa[twitter] @alexamici<br>@fa[github] @alexamici<br>@fa[firefox] http://bopen.eu)

@snap[south small]
PyConX,
2019-05-03, Florence.
@snapend

---

## Scientific data

+++

### Scientific data fields

@ul

- Data science / Big data
  - Business Intelligence, Finance, Research & Development...
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

---

## Python for scientific data

+++

### How is Python even a contender?

@ul

- Fast computing run-time
  - **Not really!** Interpreted without a JIT compiler
- Memory-efficient data types
  - **Not really!** Big native types and garbage collected
- Mature eco-systems and communities
  - **YES!** Dedicated tools & libraries: Numpy, Pandas, xarray, Dask...
  - **YES!** Distributed computing of big data: Dask.distributed...

@ulend

+++

### When is your data actually *big*?

Your data is *big* when it doesn't fit the *memory*
of the *larger* machine you have access to.

In a lot of cases *buying* a machines with *enough memory*
solves the *big* data problems.

+++

### Presenting *cfgrib*

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

## User journey

+++

### Install *ecCodes* C-library

With conda
```shell
$ conda install eccodes
```
On Ubuntu
```shell
$ sudo apt-get install libeccodes0
```
On MacOS with Homebrew
```shell
$ brew install eccodes
```

---

## Thank you!

Alessandro Amici, B-Open, Rome

@css[regular](@fa[twitter] @alexamici<br>@fa[github] @alexamici<br>@fa[firefox] http://bopen.eu)

@snap[south]
@css[regular](Slides: https://gitpitch.com/alexamici/talks)
@snapend