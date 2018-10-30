
## cfgrib: easy and efficient GRIB file access in xarray

Alessandro Amici, B-Open, Rome

@css[regular](@fa[twitter] @alexamici<br>@fa[github] @alexamici<br>@fa[firefox] http://bopen.eu)

@snap[south small]
Workshop on developing Python frameworks for earth system sciences,
2018-10-30, ECMWF, Reading.
@snapend

---

## Motivation

+++

### Here at ECMWF...

@ul

- ... we @fa[heart red] the GRIB format...
- ... and we @fa[heart red] Open Source...
- ... and we @fa[heart red] Python...
- ... but we were @fa[frown-o] about GRIB support in Python

@ulend

+++

### Goal

We would love the GRIB format to be a first-class citizens in the
Python numerical stack, with as good a support as netCDF!

ECMWF partnered with B-Open to make that happen.

---

## Development

+++

### Requirements

@ul

- full GRIB support in *xarray*
  - gateway to the Python numerical stack: *Numpy*, *Matplotlib*, *Jupyter*, *Dask*, *Scipy*, *Pandas*, *Iris*, etc.
  - robust map to Unidata's *Common Data Model v4* with *CF-Conventions*
- delightful (!) install experience
  - full support of Python 3 and *PyPy*
  - major distribution channels: *PyPI*, *conda*, source

@ulend

+++

### State of the art

- *pygrib*, *pupygrib*, *ecCodes* - No CMD
- *PyNIO*
 - Pros: xarray backend, conda
 - Cons: partial CDM support, Python 2-only, no PyPI, read-only
- *Iris-grib*
 - Pros: xarray conversion, read-write, conda
 - Cons: Python 2-only, domain specific

+++

### Storyline

@ul

- 2016-10: first prototype by ECMWF
- 2017-09: start of private *xarray-grib* by B-Open
- 2018-05: start of public *cfgrib* on GitHub
- 2018-07: first public **alpha** release of *cfgrib*
- 2018-10: *cfgrib* enters **beta**
- 2018-XX: *xarray* v0.11 will have a *cfgrib* backend <span class="regular">`xr.open_dataset('data.grib', engine='cfgrib')`</span>

@ulend

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

+++

### Install *cfgrib*

Install *cfgrib*
```shell
$ pip install cfgrib
```

Run *cfgrib* selfcheck
```shell
$ python -m cfgrib selfcheck
Found: ecCodes v2.7.0.
Your system is ready.
```

Install *xarray*
```shell
$ pip install xarray>=0.10.9
```

+++

### GRIB Dataset

```python
>>> import cfgrib
>>> ds = cfgrib.open_dataset('era5-levels-members.grib')
>>> ds
<xarray.Dataset>
Dimensions:        (isobaricInhPa: 2, latitude: 61, longitude: 120, number: 10, time: 4)
Coordinates:
  * number         (number) int64 0 1 2 3 4 5 6 7 8 9
  * time           (time) datetime64[ns] 2017-01-01 ... 2017-01-02T12:00:00
    step           timedelta64[ns] ...
  * isobaricInhPa  (isobaricInhPa) float64 850.0 500.0
  * latitude       (latitude) float64 90.0 87.0 84.0 81.0 ... -84.0 -87.0 -90.0
  * longitude      (longitude) float64 0.0 3.0 6.0 9.0 ... 351.0 354.0 357.0
    valid_time     (time) datetime64[ns] ...
Data variables:
    z              (number, time, isobaricInhPa, latitude, longitude) float32 ...
    t              (number, time, isobaricInhPa, latitude, longitude) float32 ...
Attributes:
    GRIB_edition:            1
    GRIB_centre:             ecmf
    GRIB_centreDescription:  European Centre for Medium-Range Weather Forecasts
    GRIB_subCentre:          0
    history:                 GRIB to CDM+CF via cfgrib-0.9.../ecCodes-2...
```

+++

### Naming from ecCodes

@ul

- Attributes with the `GRIB_` prefix are *ecCodes* keys both coded and computed. Mostly namespace and edition independent keys
- Variable name is defined by *ecCodes*:
 - `GRIB_cfVarName` @fa[long-arrow-right] variable name
- CF attributes are provided *ecCodes*:
 - `GRIB_name` @fa[long-arrow-right] `long_name`,
 - `GRIB_units` @fa[long-arrow-right] `units`
 - `GRIB_cfName` @fa[long-arrow-right] `standard_name`

@ulend

+++

### GRIB DataArray
 
```python
>>> ds.t
<xarray.DataArray 't' (number: 10, time: 4, isobaricInhPa: 2, latitude: 61, longitude: 120)>
[585600 values with dtype=float32]
Coordinates:
  * number         (number) int64 0 1 2 3 4 5 6 7 8 9
  * time           (time) datetime64[ns] 2017-01-01 ... 2017-01-02T12:00:00
    step           timedelta64[ns] ...
  * isobaricInhPa  (isobaricInhPa) float64 850.0 500.0
  * latitude       (latitude) float64 90.0 87.0 84.0 81.0 ... -84.0 -87.0 -90.0
  * longitude      (longitude) float64 0.0 3.0 6.0 9.0 ... 351.0 354.0 357.0
    valid_time     (time) datetime64[ns] ...
Attributes:
    GRIB_paramId:                             130
    GRIB_shortName:                           t
    GRIB_units:                               K
    GRIB_missingValue:                        9999
    GRIB_typeOfLevel:                         isobaricInhPa
    GRIB_gridType:                            regular_ll
    ...
    standard_name:                            air_temperature
    long_name:                                Temperature
    units:                                    K
```

+++

### Geographic coordinates

Computed by *ecCodes* based on `GRIB_gridType`: `regular_ll`, `regular_gg`, etc. 
```python
>>> ds.latitude
<xarray.DataArray 'latitude' (latitude: 61)>
array([ 90.,  87.,  ... -87., -90.])
Coordinates:
  * latitude  (latitude) float64 90.0 87.0 84.0 81.0 ... -81.0 -84.0 -87.0 -90.0
Attributes:
    units:          degrees_north
    standard_name:  latitude
    long_name:      latitude
>>> ds.longitude
<xarray.DataArray 'longitude' (longitude: 120)>
array([  0.,   3.,  ... 354., 357.])
Coordinates:
  * longitude  (longitude) float64 0.0 3.0 6.0 9.0 ... 348.0 351.0 354.0 357.0
Attributes:
    units:          degrees_east
    standard_name:  longitude
    long_name:      longitude
```

+++

### Vertical level coordinate

Variable name from *ecCodes* `GRIB_typeOfLevel`: `isobaricInhPa`, `surface`, `hybrid`, etc.
```python
>>> ds.isobaricInhPa
<xarray.DataArray 'isobaricInhPa' (isobaricInhPa: 2)>
array([850., 500.])
Coordinates:
  * isobaricInhPa  (isobaricInhPa) float64 850.0 500.0
Attributes:
    units:          hPa
    positive:       down
    standard_name:  air_pressure
    long_name:      pressure
```

+++

### Everything looks perfect, right?

+++

### Wrong!

Very first bug report:
```python
>>> ds = cfgrib.open_dataset('nam.t00z.awp21100.tm00.grib2')
Traceback (most recent call last):
  File "\<stdin\>", line 1, in <module>
  ...
  File ".../cfgrib/dataset.py", line 150, in enforce_unique_attributes
    raise ValueError("multiple values for unique attribute %r: %r" % (key, values))
ValueError: multiple values for unique attribute
    'typeOfLevel': ['hybrid', 'cloudBase', 'unknown', 'cloudTop']
```

---

## The devil is in the details

+++

### Common Data Model

- *xarray* is based on the concept of hypercubes
- `xr.DataArray` is N-dimensional array
- Dimensions are labeled by 1D coordinates
- `xr.Dataset` is a container of data variables **with homogeneous coordinates**

+++

### GRIB Data Model

- A GRIB *stream*, a file, is list of GRIB *messages*
- A GRIB *message* contains a single geographic *field* with `latitude`, `longitude`
- *Message* metadata (keys) can be regarded as additional coordinates: `time`, `level`, etc.
- *MARS* retrievals are typically nice hypercubes
- *Messages* in a *stream* are completely independent, **there's no guarantee**

+++

### GRIB is a generic container

- North American Model (NAM) GRIB2
 - variable `gh` for `isobaricInhPa`, `cloudBase`, `cloudTop`, `maxWind` and `isothermZero`
- Global Forecast System (GFS) v4 GRIB2
 - variables `gh` and `clwmr` are defined on different values of `isobaricInhPa`

+++

### Message filtering

```python
>>> cfgrib.open_dataset('nam.t00z.awp21100.tm00.grib2',
...     backend_kwargs=dict(filter_by_keys={'typeOfLevel': 'cloudTop'}))
<xarray.Dataset>
Dimensions:     (x: 93, y: 65)
Coordinates:
    time        datetime64[ns] ...
    step        timedelta64[ns] ...
    cloudTop    int64 ...
    latitude    (y, x) float64 ...
    longitude   (y, x) float64 ...
    valid_time  datetime64[ns] ...
Dimensions without coordinates: x, y
Data variables:
    pres        (y, x) float32 ...
    gh          (y, x) float32 ...
    t           (y, x) float32 ...
Attributes:
    GRIB_edition:            2
    GRIB_centre:             kwbc
    GRIB_centreDescription:  US National Weather Service - NCEP 
    GRIB_subCentre:          0
    history:                 GRIB to CDM+CF via cfgrib-0.9.../ecCodes-2.8...
```

---

## To summarise

+++

### cfgrib features in beta

@ul

- *xarray* backend starting with v0.11
- reads most GRIB 1 and 2 files,
- supports all modern versions of Python 3.7, 3.6, 3.5 and 2.7, plus PyPy and PyPy3,
- works on most *Linux* distributions and *MacOS*, *ecCodes* C-library is the only system dependency,
- you can `pip install cfgrib` with no compile,
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
- *PyPI* binary package does not include *ecCodes*, for now,
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