
### cfgrib: easy and efficient GRIB file access in xarray

Alessandro Amici, B-Open, Rome

[@alexamici](https://twitter.com/alexamici) - http://bopen.eu

<span class='small'>
Workshop on developing Python frameworks for earth system sciences,
2018-10-30, ECMWF, Reading.
</span>

---

### Motivation

Here at ECMWF we @fa[heart fa-spin red] the GRIB format...

... and we @fa[heart fa-spin red] Open Source...

... and we @fa[heart fa-spin red] Python 

+++

### Motivation

We would love the GRIB format to be a first-class citizens in the
Python numerical stack, with as good a support as netCDF!

---

### Desiderata

- full GRIB support in *xarray*
  - robust map to Unidata's *Common Data Model v4* with CF-Conventions
  - gateway to Numpy, Matplotlib, Jupyter, Dask, Iris, etc.

- user friendly install
  - full support of Python 3 and PyPy3
  - major distribution channels: PyPI, conda, source

+++

### GRIB support in Python

- pygrib, pupygrib, ecCodes - No CMD
- PyNIO
 - Pros: xarray backend, conda
 - Cons: partial CDM support, Python 2-only, no PyPI, read-only
- Iris-grib
 - Pros: xarray conversion, read-write, conda
 - Cons: Python 2-only, domain specific

+++

### Here comes *cfgrib*

Python interface to map GRIB files to the NetCDF Common Data Model following the CF Conventions.
The high level API is designed to support a GRIB backend for xarray
and it is inspired by NetCDF-python and h5netcdf.
Low level access and decoding is performed via the ECMWF ecCodes library.

---

### cfgrib features in beta
   
- reads most GRIB 1 and 2 files,
- supports all modern versions of Python 3.7, 3.6, 3.5 and 2.7, plus PyPy and PyPy3,
- works on most *Linux* distributions and *MacOS*, *ecCodes* C-library is the only system dependency,
- you can `pip install cfgrib` with no install time build (binds with *CFFI* ABI mode),
- reads the data lazily and efficiently in terms of both memory usage and disk access.

+++

### cfgrib work in progress

- **Alpha** supports saving the index of a GRIB file to disk, to save a full-file scan on open,
- **Pre-Alpha** support to write carefully-crafted `xarray.Dataset`'s to a GRIB2 file,
- the target is mostly correctness, but we started working on performance.

+++

### cfgrib limitations

- no *conda* package, for now,
- *PyPI* binary package does not include ecCodes,
- incomplete documentation, for now,
- no *Windows* support, for now,
- rely on *ecCodes* for the CF attributes of the data variables,
- rely on *ecCodes* for the `gridType` handling.

---

### Team

- ECMWF
  - Stephan Siemen, Iain Russell and Baudouin Raoult
- B-Open
  - Alessandro Amici, Aureliana Barghini and Leonardo Barcaroli

---

### Thank you!

Alessandro Amici, B-Open, Rome

[@alexamici](https://twitter.com/alexamici) - http://bopen.eu

<span class='small'>
Slides: https://gitpitch.com/alexamici/talks
</span>
