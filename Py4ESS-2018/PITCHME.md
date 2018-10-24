
### cfgrib: easy and efficient GRIB file access in xarray


Alessandro Amici, B-Open, Rome

[@alexamici](https://twitter.com/alexamici) - http://bopen.eu

<span class='small'>
Workshop on developing Python frameworks for earth system sciences, 2018-10-30, ECMWF, Reading.
</span>

---

### Motivation

Make GRIB files first-class citizens in Python numerical stack via xarray

+++

### cfgrib features
   
- support reading most GRIB 1 and 2 files,
- support all modern versions of Python 3.7, 3.6, 3.5 and 2.7, plus PyPy and PyPy3,
- support most Linux distributions and MacOS,
- only system dependency is the ecCodes C-library,
- no install time build (binds with *CFFI* ABI mode),
- read the data lazily and efficiently in terms of both memory usage and disk access.

+++

### cfgrib work in progress

- **Alpha** support for saving the index of a GRIB file to disk, saves a full-file scan on open,
- **Pre-Alpha** limited support to write carefully-crafted `xarray.Dataset`'s to a GRIB2 file,
- the target is mostly correctness, but we started working on performance.

+++

### cfgrib limitations

- incomplete documentation, for now,
- no Windows support, for now,
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
