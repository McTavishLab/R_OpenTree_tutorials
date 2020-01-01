---
title: Setup
---

For the R section of the workshop, we recommend using RStudio ([installation instructions](https://opentreeoflife.github.io/SSBworkshop/#setup)), but you can use the R GUI (from a mac) or R from Terminal; feel free to use whatever works better for you.

From within R, install the packages `rotl`, `ape`, `devtools` and `stringr` with the function `install.packages()`,
and the package `datelife` with the function `install_github()`.

Load them into your workspace with `library()` or `require()`.

When in doubt, follow the code chunk below.

**Hint**:
If you do not want to load the packages, you can call functions specifying their package using two colons and the syntax `package_name::function_name()`.

This implies more typing, but gives more clarity to reproduce the workflow later. So we will use that syntax for this tutorial.

An exception to this, are functions from packages that are "preloaded" --such as `library()` from `base`, or `install.packages()` from `utils`; these can simply be called by their name.

**Code chunk**:

```{r}
install.packages(c("rotl", "ape", "devtools", "stringr"))
library(rotl)
library(devtools)
library(stringr)
devtools::install_github("phylotastic/datelife")
library(datelife)
```
{% include links.md %}
