
### A common data model approach to NetCDF and GRIB data harmonisation

Alessandro Amici, B-Open, Rome

[@alexamici](https://twitter.com/alexamici) - http://bopen.eu

---

### Motivation: climate data

 * Copernicus Climate Change Service, by ECMWF:
   * Re-analyses, seasonal forecasts, climate projections, satellite and in-situ observations
   * N-dimensional gridded data in many dialects of NetCDF and GRIB
   * Archive: Climate Data Store (CDS)

---

### Motivation: forecast data and tools

 * ECMWF:
   * Weather forecasts, re-analyses, satellite and in-situ observations
   * N-dimensional gridded data in GRIB
   * Archive: Meteorological Archival and Retrieval System (MARS)
   * A lot of tools: Metview, Magics, ecCodes...

---

### Harmonisation strategic choices

 * Python programming language
 * `xarray.DataArray` data structure
   * coordinates harmonisation
     * matching labels, units and values
   * standardised metadata / CF Conventions

---

### Harmonisation strategic needs

 * CDS common data model
   * xarray GRIB driver
   * importer and compliance checker for NetCDF
 * CF Conventions aware variables operations
   * use metadata
     * units conversion, plot labeling...
   * add metadata to results
     * update attributes (e.g. units), track provenance...

---

### Thank you!

Alessandro Amici, B-Open, Rome

[@alexamici](https://twitter.com/alexamici) - http://bopen.eu
