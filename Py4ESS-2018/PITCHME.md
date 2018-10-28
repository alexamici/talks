
## cfgrib: easy and efficient GRIB file access in xarray

Alessandro Amici, B-Open, Rome

@css[regular](@fa[twitter] @alexamici<br>@fa[github] @alexamici<br>@fa[firefox] http://bopen.eu)

@snap[south small]
Workshop on developing Python frameworks for earth system sciences,
2018-10-30, ECMWF, Reading.
@snapend

---

## The motivation

+++

### Here at ECMWF...

@ul

- ... we @fa[heart red] the GRIB format...
- ... and we @fa[heart red] Open Source...
- ... and we @fa[heart red] Python...
- ... but we were @fa[frown-o] about GRIB support in Python

@ulend

+++

### The goal

We would love the GRIB format to be a first-class citizens in the
Python numerical stack, with as good a support as netCDF!

+++

### Desiderata

@ul

- full GRIB support in *xarray*
  - gateway to the Python numerical stack: *Numpy*, *Matplotlib*, *Jupyter*, *Dask*, *Scipy*, *Pandas*, *Iris*, etc.
  - robust map to Unidata's *Common Data Model v4* with *CF-Conventions*
- delightful (!) install experience
  - full support of Python 3 and *PyPy*
  - major distribution channels: *PyPI*, *conda*, source

@ulend

---

## The journey

+++

### GRIB support in Python

- *pygrib*, *pupygrib*, *ecCodes* - No CMD
- *PyNIO*
 - Pros: xarray backend, conda
 - Cons: partial CDM support, Python 2-only, no PyPI, read-only
- *Iris-grib*
 - Pros: xarray conversion, read-write, conda
 - Cons: Python 2-only, domain specific

+++

### Here comes *cfgrib* (was *xarray-grib-driver*)

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

+++

### Storyline

- 2016-10: first prototype by ECMWF
- 2017-09: start of private *xarray-grib-driver*
- 2018-05: start of public *cfgrib*
- 2018-07: first public **alpha** release of *cfgrib*
- 2018-10: *cfgrib* enters **beta**

---

### 

---

## To summarise

+++

### cfgrib features in beta

@ul

- reads most GRIB 1 and 2 files,
- supports all modern versions of Python 3.7, 3.6, 3.5 and 2.7, plus PyPy and PyPy3,
- works on most *Linux* distributions and *MacOS*, *ecCodes* C-library is the only system dependency,
- you can `pip install cfgrib` with no install time build (binds with *CFFI* ABI mode),
- reads the data lazily and efficiently in terms of both memory usage and disk access.

@ulend

+++

### cfgrib work in progress

@ul

- **Alpha** supports writing the index of a GRIB file to disk, to save a full-file scan on open,
- **Pre-Alpha** support to write carefully-crafted `xarray.Dataset`'s to a GRIB2 file.

@ulend

+++

### cfgrib limitations

@ul

- no *conda* package, for now,
- *PyPI* binary package does not include ecCodes, for now,
- incomplete documentation, for now,
- no *Windows* support, for now,
- rely on *ecCodes* for the CF attributes of the data variables,
- rely on *ecCodes* for the `gridType` handling.

@ulend

---

## The team

- ECMWF
  - Stephan Siemen, Iain Russell and Baudouin Raoult
- B-Open
  - Alessandro Amici, Aureliana Barghini and Leonardo Barcaroli

---

## Thank you!

Alessandro Amici, B-Open, Rome

@css[regular](@fa[twitter] @alexamici<br>@fa[github] @alexamici<br>@fa[firefox] http://bopen.eu)

@snap[south]
@css[regular](Slides: https://gitpitch.com/alexamici/talks)
@snapend