
---
output: html_document
editor_options: 
  chunk_output_type: console
---
# Child Labor Laws as an IV

2SLS estimates of the returns to schooling using child labor laws as instruments for years of schooling [@AcemogluAngrist2000].
This replicates Table 6.3 of *Mastering 'Metrics*.


```r
library("tidyverse")
library("broom")
library("clubSandwich")
```

Function to calculate clustered standard errors and return a tidy data frame of the coefficients and standard errors.

```r
cluster_se <- function(mod, cluster, type = "CR2") {
  vcov <- vcovCR(mod, cluster = cluster, type = type)
  coef_test(mod, vcov = vcov) %>%
    rownames_to_column(var = "term") %>%
    as_tibble() %>%
    select(term, estimate = beta, std.error = SE)
}
```

Load the `child_labor` data.

```r
data("child_labor", package = "masteringmetrics")
child_labor <- mutate(child_labor,
                      year = factor(year),
                      yob_fct = factor(yob),
                      sob = factor(sob))
```

## First stages and reduced forms

Column 1. Years of Schooling.

```r
mod1 <- lm(indEduc ~ year + yob_fct + sob + cl7 + cl8 + cl9,
           data = child_labor, weights = weight)
```


In Stata, these are estimated with `[pw=weight]`.


## OLS and IV returns

## References {-}

-   <http://masteringmetrics.com/wp-content/uploads/2015/02/ReadMe_ChildLaborLaws.txt>
-   <http://masteringmetrics.com/wp-content/uploads/2015/02/AA_regs.do>
