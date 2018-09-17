# RPharma2018packages
Collection of favorite packages from RPharma conf

**This is a packrat project** Please use packrat to work here

## Package list

- [Packrat](#packrat) - package dependency manager
- [Archivist](#archivist) - Session manager
- [logR](#logr) - Extended logging solution
- Diffdf - data.frame comparison
- Yardstick - linear model 
- RInno - Shiny Apps as Windows Applications

## Packrat

Packrat allows you to store all packages you are using for a certain session/project. Packrat is also
used for this project to store the packages shown here. The main guide for packrat can be found
at the [RStudio blog describing](https://rstudio.github.io/packrat/commands.html) it

```
install.packages("packrat")
packrat::unbundle(“packrat/bundles/…..tar.gz”)

```

## Archivist

an R package designed to improve the management of results
of data analysis. Key functionalities of this package include: 

(i) management of local
and remote repositories which contain R objects and their meta-data (objects’ properties
and relations between them);

(ii) archiving R objects to repositories;

(iii) sharing and
retrieving objects (and it’s pedigree) by their unique hooks;

(iv) searching for objects
with specific properties or relations to other objects;

(v) verification of object’s identity
and context of it’s creation.

### Example for task (ii) - restore models

This example gives a list of models stored inside the package

```
library(archivist)
models <- asearch("pbiecek/graphGallery", patterns = "class:lm")
modelsBIC <- sapply(models, BIC)

sort(modelsBIC)
```

### Example for task (i) - store objects locally

A data.frame comes with this repository and is stored in the `arepo` folder. Your task is
to create a new data.frame, store it in the `arepo_new` folder and add it to the 
restored data.frame. If everything works out the sum of the data.frames shows up
to be 2 for position (1,1).

```
library(archivist)

repo <- "arepo_new"
createLocalRepo(repoDir = repo, default = TRUE)

df <- data.frame(x=c(1,2),y=c(2,3))
saveToRepo(df)

setLocalRepo("arepo")
df2 <- loadFromLocalRepo("4a7369a8c51cb1e7efda0b46dad8195e",value = TRUE)

df_test <- df + df2

print(df_test[1,1]==2)

```

## logR

### Prequisites

First you need to install PostGres via

**Linux**

if you have docker then: docker run --rm -p 127.0.0.1:5432:5432 -e POSTGRES_PASSWORD=postgres --name pg-logr postgres:9.5

**Windows**

https://www.enterprisedb.com/downloads/postgres-postgresql-downloads

install setting password to `postgres` and start via:

C:\Program Files\PostgreSQL\9.5\bin\postgres.exe

### Example 1

```
# create logr table
logR_schema()

# make some logging and calls

logR(1+2) # OK
#[1] 3
logR(log(-1)) # warning
#[1] NaN
f = function() stop("an error")
logR(r <- f()) # stop
#NULL
g = function(n) data.frame(a=sample(letters, n, TRUE))
logR(df <- g(4)) # out rows
#  a
#1 u
#2 c
#3 w
#4 p

# try CTRL+C / 'stop' button to interrupt
logR(Sys.sleep(5))

# wrapper to: dbReadTable(conn = getOption("logR.conn"), name = "logr")
logR_dump()
```


