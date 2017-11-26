
### A common data model approach to NetCDF and GRIB data harmonisation


Alessandro Amici, B-Open, Rome

[@alexamici](https://twitter.com/alexamici) - http://bopen.eu

<span style='font-size: 75%;'>Workshop on developing Python frameworks for earth system sciences, 2017-11-28, Reading.</span>

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

### Examples

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
#### Do we have time for questions?

Alessandro Amici, B-Open, Rome

[@alexamici](https://twitter.com/alexamici) - http://bopen.eu