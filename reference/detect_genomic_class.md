# Detect the appropriate class for genomic strings

Utility function to determine what class a genomic string should be
parsed as, without actually performing the parsing.

## Usage

``` r
detect_genomic_class(x)
```

## Arguments

- x:

  Character string or vector

## Value

Character vector of predicted classes

## Examples

``` r
detect_genomic_class("chr1:1000-2000")
#> [1] "GRanges"
detect_genomic_class("chr1:1000")
#> [1] "GPos"
detect_genomic_class(c("chr1:1-10|chr2:20-30", "1000-2000"))
#> [1] "GInteractions" "IRanges"      
```
