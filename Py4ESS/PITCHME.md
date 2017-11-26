
### A common data model approach to NetCDF and GRIB data harmonisation


Alessandro Amici, B-Open, Rome

[@alexamici](https://twitter.com/alexamici) - http://bopen.eu

<span style='font-size: 50%;'>
Workshop on developing Python frameworks for earth system sciences, 2017-11-28, ECMWF, Reading.
</span>

---

### Motivation: forecast data and tools

 * ECMWF
   * Weather forecasts, re-analyses, satellite and in-situ observations
   * N-dimensional gridded data in GRIB
   * Archive: Meteorological Archival and Retrieval System (MARS)
   * A lot of tools: Metview, Magics, ecCodes...

---

### Motivation: climate data

 * Copernicus Climate Change Service (C3S)
   * Re-analyses, seasonal forecasts, climate projections, satellite and in-situ observations
   * N-dimensional gridded data in many dialects of NetCDF and GRIB
   * Archive: Climate Data Store (CDS)

---

### Harmonisation strategic choices

 * Python 3 programming language
 * `xarray.DataArray` data structure
   * coordinates harmonisation
     * matching labels, units and values
   * standardised metadata / CF Conventions

---

### xarray data model

```python
>>> import xarray as xr
>>> xr.open_dataset('ERA5-tuv-europe.nc')
<xarray.Dataset>
Dimensions:    (latitude: 42, level: 5, longitude: 73, time: 4)
Coordinates:
  * longitude  (longitude) float32 -27.0 -26.0 -25.0 -24.0 -23.0 -22.0 -21.0 ...
  * latitude   (latitude) float32 74.0 73.0 72.0 71.0 70.0 69.0 68.0 67.0 ...
  * level      (level) int32 300 500 700 850 1000
  * time       (time) datetime64[ns] 2017-06-01 2017-06-01T12:00:00 ...
Data variables:
    t          (time, level, latitude, longitude) float64 220.6 220.8 221.0 ...
    u          (time, level, latitude, longitude) float64 -11.93 -12.68 ...
    v          (time, level, latitude, longitude) float64 5.13 6.681 8.024 ...
Attributes:
    Conventions:  CF-1.6
    history:      2017-11-26 11:12:39 GMT by grib_to_netcdf-2.5.0: grib_to_ne...
```

---

### xarray.DataArray

```python
>>> xr.open_dataset('ERA5-tuv-europe.nc', chunks={})
<xarray.DataArray 't' (time: 4, level: 5, latitude: 42, longitude: 73)>
dask.array<open_dataset-..., shape=(4, 5, 42, 73), dtype=float64, chunksize=(4, 5, 42, 73)>
Coordinates:
  * longitude  (longitude) float32 -27.0 -26.0 -25.0 -24.0 -23.0 -22.0 -21.0 ...
  * latitude   (latitude) float32 74.0 73.0 72.0 71.0 70.0 69.0 68.0 67.0 ...
  * level      (level) int32 300 500 700 850 1000
  * time       (time) datetime64[ns] 2017-06-01 2017-06-01T12:00:00 ...
Attributes:
    units:          K
    long_name:      Temperature
    standard_name:  air_temperature
```

---

### ECMWF harmonisation projects

 * pythonisation of ECMWF data and tools
   * xarray GRIB driver (uses ecCodes) @fa[git-square]
   * high level python bindings
     * Metview @fa[git-square], Magics @fa[check-square]
   * low level python bindings
     * ecCodes @fa[check-square], odb-api @fa[check-square]

---

### C3S harmonisation projects

 * CDS common data model @fa[git-square]
   * definition, importer and compliance checker
 * CF Conventions aware variables operations @fa[git-square]
   * use metadata for units conversion, plot labeling...
   * add metadata to results, update attributes (e.g. units), track provenance...

---

### xarray-grib-driver

```python
>>> import xarray_grib
>>> store = xarray_grib.GribDataStore('forecast.grib')
>>> forecast = xr.open_dataset(store)
>>> forecast.t
DataArray()
```

---

### Teams and Credits

 * Coordinators: B. Rault (ECMEF / C3S), S. Siemen (ECMWF), C. Bergeron (C3S) and A. Amici
 * Developers and scientific advisers:
   * metview: I. Russell (ECMWF) and M. Di Bari
   * xarray-grib-driver: A. Amici and L. Barcaroli
   * cds-cmor-tables: C. Cagnazzo, F. Fierli (ISAC-CNR) and G. Cammarota
   * cdstools: G. Cammarota and F. Nazzaro

---

### Thank you!
#### Do we have time for Q/A?

Alessandro Amici, B-Open, Rome

[@alexamici](https://twitter.com/alexamici) - http://bopen.eu

<span style='font-size: 50%;'>
Slides: https://gitpitch.com/alexamici/talks
</span>
