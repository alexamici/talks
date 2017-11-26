
### A common data model approach to NetCDF and GRIB data harmonisation


Alessandro Amici, B-Open, Rome

[@alexamici](https://twitter.com/alexamici) - http://bopen.eu

<span class='small'>
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

### xarray.DataArray data model

ECMWF NetCDF dilect

```python
>>> import xarray as xr
>>> ta_era5 = xr.open_dataset('ERA5-tuv-europe.nc', chunks={}).t
>>> ta_era5
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

### xarray.DataArray data model

CMIP5 NetCDF dilect

```python
>>> ta_CMIP5 = xr.open_dataset('ta_6hrPlev_CMCC-CM_decadal2005_r1i3p1_2017060100-2017063018.nc', chunks={}).ta
>>> ta_CMIP5
<xarray.DataArray 'ta' (time: 120, plev: 3, lat: 240, lon: 480)>
dask.array<open_dataset-..., shape=(120, 3, 240, 480), dtype=float64, chunksize=(120, 3, 240, 480)>
Coordinates:
  * time     (time) datetime64[ns] 2017-06-01 2017-06-01T06:00:00 ...
  * plev     (plev) float64 8.5e+04 5e+04 2.5e+04
  * lat      (lat) float64 -89.43 -88.68 -87.94 -87.19 -86.44 -85.69 -84.95 ...
  * lon      (lon) float64 0.0 0.75 1.5 2.25 3.0 3.75 4.5 5.25 6.0 6.75 7.5 ...
Attributes:
    standard_name:     air_temperature
    long_name:         Air Temperature
    units:             K
    original_name:     t
    cell_measures:     area: areacella
    associated_files:  baseURL: http://cmip-pcmdi.llnl.gov/CMIP5/dataLocation...
    history:           2012-04-13T21:01:28Z altered by CMOR: Inverted axis: lat.
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

<span class='small'>
Slides: https://gitpitch.com/alexamici/talks
</span>
