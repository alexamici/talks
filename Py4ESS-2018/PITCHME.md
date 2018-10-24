
### cfgrib: easy and efficient GRIB file access in xarray


Alessandro Amici, B-Open, Rome

[@alexamici](https://twitter.com/alexamici) - http://bopen.eu

<span class='small'>
Workshop on developing Python frameworks for earth system sciences, 2018-10-30, ECMWF, Reading.
</span>

---

### Motivation

Make GRIB files first-class citizens in Python numerical stack via xarray

Map a GRIB file to the Unidata's *Common Data Model* version 4 like a NetCDF-4 with CF-Conventions

+++

### State of the art

- PyNIO
 - Pros: xarray backend, conda
 - Cons: no PyPI, Python 2-only, read-only
- Iris-grib
 - Pros: xarray conversion, read-write, conda
 - Cons: Python 2-only, domain specific
- GRIB Feature Collections - Cons: JAVA
- pygrib, pupygrib, ecCodes - Cons: no CMD

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
- PyPI binary package does not include ecCodes,
- incomplete documentation, for now,
- no *Windows* support, for now,
- rely on *ecCodes* for the CF attributes of the data variables,
- rely on *ecCodes* for the `gridType` handling.

---

### Teams and Credits

 * Coordinators (ECMWF): S. Siemen, I. Russell and B. Raoult
 * Developers (B-Open): A. Amici, A. Barghini and L. Barcaroli

---

### Thank you!
#### Do we have time for Q/A?

Alessandro Amici, B-Open, Rome

[@alexamici](https://twitter.com/alexamici) - http://bopen.eu

<span class='small'>
Slides: https://gitpitch.com/alexamici/talks
</span>
