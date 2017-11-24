
### A common data model approach to NetCDF and GRIB data harmonisation

Alessandro Amici, B-Open, Rome

[@alexamici](https://twitter.com/alexamici) - http://bopen.eu

---

### Motivation: forecast data and tools

 * ECMWF:
   * Weather forecasts, re-analyses, satellite and in-situ observations
   * N-dimensional gridded data in GRIB
   * Archive: Meteorological Archival and Retrieval System (MARS)
   * A lot of tools: Metview, Magics, ecCodes...

---

### Motivation: climate data

 * Copernicus Climate Change Service, by ECMWF:
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

### Harmonisation strategic needs

 * pythonisation pf ECMWF tools
   * xarray GRIB driver  @fa[git-square]
   * python bindings
     * Metview, Python 3  @fa[git-square]
     * Magics, Python 2/3 @fa[check-square]

---

### Harmonisation strategic needs

 * CDS common data model @fa[git-square]
   * importer and compliance checker
 * CF Conventions aware variables operations @fa[git-square]
   * use metadata
     * units conversion, plot labeling...
   * add metadata to results
     * update attributes (e.g. units), track provenance...

---

### 

---

### Thank you!

Alessandro Amici, B-Open, Rome

[@alexamici](https://twitter.com/alexamici) - http://bopen.eu
