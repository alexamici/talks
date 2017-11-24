
## A common data model approach to NetCDF and GRIB data harmonisation

Alessandro Amici, B-Open, Rome

[@alexamici](https://twitter.com/alexamici) - http://bopen.eu

---

 * Copernicus Climate Change Service, entrusted to ECMWF
   * Re-analysis, seasonal forecasts, climate projections, observations
   * N-dimensional gridded data in many dialects of NetCDF
   * Point data in NetCDF format (?)
   * Archive: Climate Data Store (CDS)
 * ECMWF
   * Weather forecasts, satellite and in-situ observations
   * N-dimensional gridded data in GRIB
   * Point data in BUFR and ODB
   * Archive: Meteorological Archival and Retrieval System (MARS)

---

## Strategic choices

 * Python programming language
 * `xarray.DataArray` as founding data structure

```python
>>> import xarray as xr
>>> data = xr.open_dataarray('ERA5.nc')
>>> data

```

---

## Thank you!


Alessandro Amici, B-Open, Rome

[@alexamici](https://twitter.com/alexamici) - http://bopen.eu
