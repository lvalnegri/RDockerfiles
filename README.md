## Dockerfiles to build *R* conotainers

### base
 - Ubuntu 20.04 LTS full upgraded
 - public shared repository
 - non-root user

### Rbase




### Rmin
A minimum package installation:
 - devtools, usethis, roxygen2
 - ggplot2
 - data.table
 - htmlwidgets, DT, dygraphs, leaflet
 - rmarkdown, shiny, xaringan, 

### Rtidy
An alternative minimum package installation for the tidyverse user:
 - tidyverse, tidymodels
 - htmlwidgets, DT, dygraphs, mapview
 - rmarkdown, shiny

### Rspatial
 - `Rmin`
 - Linux Libraries
 - R spatial packages: rgdal, rgeos, sp, sf, tmap, maptools, mapview

### RRS: R + RStudioServer (RRSm or RRSs)
 - RStudio Server 



### RSS: R + ShinyServer (RSSm or RSSs)
 - 

I suggest you run a single app on any container. To make it viewable through an `nginx` server installed on the host machine, run the container using the following command:
```

```
making changes where needed. Then add in the `nginx` configuration the following *block* in the `server` directive:
```

```

### Rapi
 - plumber + future

