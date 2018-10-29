
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

ECMWF hired B-Open to make that happen.

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
- 2018-XX: *xarray* v0.11 is released with *cfgrib* backend

@ulend

+++

### Here comes *cfgrib*

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

- Attributes with the `GRIB_` prefix are *ecCodes* keys both coded and computed
- Variable names are *ecCodes* `cfVarName` if available or `shortName`
  - for example for variable 2m temperature `shortName=='2t'` and `cfVarName=='t2m'`
- CF attributes are taken from *ecCodes*: `name` -> `long_name`, `units` -> `units` and `cfName` -> `standard_name`

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

### GRIB time coordinate

```python
>>> ds.time
<xarray.DataArray 'time' (time: 4)>
array(['2017-01-01T00:00:00.000000000', '2017-01-01T12:00:00.000000000',
       '2017-01-02T00:00:00.000000000', '2017-01-02T12:00:00.000000000'],
      dtype='datetime64[ns]')
Coordinates:
  * time        (time) datetime64[ns] 2017-01-01 ... 2017-01-02T12:00:00
    valid_time  (time) datetime64[ns] ...
Attributes:
    standard_name:  forecast_reference_time
    long_name:      initial time of forecast
```

+++

### GRIB step coordinate

```python
>>> ds.step
<xarray.DataArray 'step' ()>
array(0, dtype='timedelta64[ns]')
Coordinates:
    step     timedelta64[ns] 00:00:00
Attributes:
    standard_name:  forecast_period
    long_name:      time since forecast_reference_time
```

+++

### GRIB vertical level coordinate

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

### GRIB geographic coordinates

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

### GRIB valid_time coordinate

```python
>>> ds.valid_time
<xarray.DataArray 'valid_time' (time: 4)>
array(['2017-01-01T00:00:00.000000000', '2017-01-01T12:00:00.000000000',
       '2017-01-02T00:00:00.000000000', '2017-01-02T12:00:00.000000000'],
      dtype='datetime64[ns]')
Coordinates:
  * time        (time) datetime64[ns] 2017-01-01 ... 2017-01-02T12:00:00
    valid_time  (time) datetime64[ns] 2017-01-01 ... 2017-01-02T12:00:00
Attributes:
    standard_name:  time
    long_name:      time
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