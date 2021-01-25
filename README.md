## Dockerfiles to build *R* containers
Notice that the following builds are supposed to be used on systems with a shared *public* repository as explained in [this repo](), therefore taking advantages of software and configurations already present on the host, like an *Nginx* web server and an *R* package library.  

### base
 - Ubuntu 20.04 LTS full upgraded
 - public shared repository
 - non-root user

### Rbase




### Rmin
My preferred minimum package installation:
 - data.table, fst
 - ggplot2
 - htmlwidgets, DT, dygraphs, leaflet
 - mlr3, caret, randomForest, XGBoost, glmnet
 - rmarkdown, shiny, xaringan, 
 - devtools, usethis, roxygen2

### Rtidy
An alternative minimum package installation for the *tidyverse* user:
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

