# Installation {#installation}

Discoveryengine (and all packages we use in Prospect Development) is installed on the R drive. The most important step to make sure you can access these packages is to set up your default library path to the shared packages repository.

Run a quick test to see your current library path:


```r
.libPaths()
```

The first result returned shoud be "\\\\ur2.urel.berkeley.edu/ur_unitshares/Prospect Development/Prospect Analysis/pd-shared-r-packages". If it's not, follow the steps below to change your default library path.

Step 1: Open a new RStudio session and run the following script:


```r
# the first part sets up the new package directory
package_dir <- "\\\\ur2.urel.berkeley.edu/ur_unitshares/Prospect Development/Prospect Analysis/pd-shared-r-packages"
if (!dir.exists(package_dir)) dir.create(package_dir, recursive = TRUE)

# then make a configuration file that notifies R of the new package directory
# change [username] to your actual username in your P drive - usually your last name followed by your first initial
if (!dir.exists(package_dir)) dir.create(package_dir, recursive = TRUE)
env_file <- "\\ur2.urel.berkeley.edu\UR_USERS\[usernmae]"

# and add the new location to the configuration file
cat("\nR_LIBS_USER='", package_dir, "'",
    sep = "", file = env_file, append = TRUE)
```

Step 2:

Quit your current RStudio session and open a new one. Try running the following test again:


```r
.libPaths()
```

If the PD shared packages depository is returned, you are ready to start using Disco Engine. If not, contact the Analytics Team to troubleshoot your situation.

## Before you continue

At this point, you should have already: 
- tested your CDW connection
- run a test query

If you haven't completed all of those items, head over to section [Test CDW](#test-cdw) and complete those steps, and then return here.

## A disco test

In order to make sure everything is working properly, we'll run a quick disco test:


```r
# you'll always load the discoveryengine package before doing anythying else
library(discoveryengine)
```

```
## Welcome to Disco Engine version 0.4.2
## Cheat sheet: https://cwolfsonseeley.github.io/discodocs/cheat-sheet.html
## News and bugfixes: https://github.com/cwolfsonseeley/discoveryengine/blob/master/NEWS.md
```

```r
# make sure we can create a disco definition
high_cap = has_capacity(1)
# make sure that things print properly
high_cap
```

```
## LISTBUILDER DEFINITION (type: entity_id)
## .   source: d_entity_mv.entity_id (entity_id)
## .   logic: capacity_rating_code IN (1)
```

```r
# make sure we can send the defintion to the data warehouse
display(high_cap)
```

```
## Warning: `tbl_df()` was deprecated in dplyr 1.0.0.
## Please use `tibble::as_tibble()` instead.
## This warning is displayed once every 8 hours.
## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was generated.
```

```
## # A tibble: 347 x 1
##    entity_id
##        <dbl>
##  1      1969
##  2      3105
##  3      3422
##  4      7011
##  5      7644
##  6     12178
##  7     12453
##  8     18139
##  9     20125
## 10     20227
## # ... with 337 more rows
```

If you got this far without any problems, then congrats! You are a disco dancer!
