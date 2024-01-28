
<!-- README.md is generated from README.Rmd. Please edit that file -->

# recPhyloParse

<!-- badges: start -->

[![R-CMD-check](https://github.com/fmarotta/recPhyloParse/actions/workflows/R-CMD-check.yaml/badge.svg)](https://github.com/fmarotta/recPhyloParse/actions/workflows/R-CMD-check.yaml)
<!-- badges: end -->

recPhyloParse provides a way to import reconciliated phylogenies into R
in a way that makes it easy to plot the trees with ggplot2. The starting
point is an XML file following the [recPhyloXML
format](http://phylariane.univ-lyon1.fr/recphyloxml/), which will be
parsed into an [R6 object](https://r6.r-lib.org/index.html). During the
parsing, the coordinates and attributes of all the plot elements will be
appropriately extracted into data.frames, so that standard ggplot2 geoms
can be used.

## Installation

You can install the development version of recPhyloParse from
[GitHub](https://github.com/) with:

``` r
# install.packages("devtools")
devtools::install_github("fmarotta/recPhyloParse")
```

## Example

This is a basic example which shows you how to solve a common problem:

``` r

library(recPhyloParse)
library(ggplot2)

recphylo_file <- system.file("extdata", "example_1.recphyloxml", package = "recPhyloParse")
recphylo <- RecPhylo$new(recphylo_file, branch_length_scale = 5, x_padding = 4)
#> Warning: 'branch_length' not found in clade ABCD. Setting branch length to 1
#> automatically.

ggplot() +
  geom_line(data = recphylo$spEdges, aes(x, y, group = group)) +
  geom_line(data = recphylo$recGeneEdges, aes(x, y, group = group, color = gsub("l+$", "", lineage), linetype = event_type), show.legend = F) +
  scale_linetype_manual(values = c("loss_v" = 2, "transferBack" = 3), na.value = 1) +
  theme_void() +
  NULL
```

<img src="man/figures/README-unnamed-chunk-2-1.png" width="100%" />

## Bugs and missing features

- We support only one gene tree per species tree.
- BifurcationOut events are currently not supported.
- We don’t have the ability to (easily) flip the children of a node
  (both gene and species tree).
- We make no attempt to minimise the edge crossings for lateral
  transfers.
- We do not add clade attributes and internal elements as columns in the
  resulting data.frame (we could add all these:
  <http://www.phyloxml.org/documentation/version_1.20/phyloxml.html>)
