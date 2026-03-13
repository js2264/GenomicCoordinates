# Convert to GPos object

Converts character strings representing single genomic positions to GPos
objects.

## Usage

``` r
as_gpos(.data, ...)
```

## Arguments

- .data:

  A character vector of genomic position strings

- ...:

  Additional arguments (unused)

## Value

A GPos object

## Examples

``` r
as_gpos("chr1:1000")
#> UnstitchedGPos object with 1 position and 0 metadata columns:
#>       seqnames       pos strand
#>          <Rle> <integer>  <Rle>
#>   [1]     chr1      1000      *
#>   -------
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths
as_gpos("chr1:1,000:+")
#> UnstitchedGPos object with 1 position and 0 metadata columns:
#>       seqnames       pos strand
#>          <Rle> <integer>  <Rle>
#>   [1]     chr1      1000      +
#>   -------
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths
as_gpos(c("chr1:1000", "chr2:2000", "chr3:3000"))
#> UnstitchedGPos object with 3 positions and 0 metadata columns:
#>       seqnames       pos strand
#>          <Rle> <integer>  <Rle>
#>   [1]     chr1      1000      *
#>   [2]     chr2      2000      *
#>   [3]     chr3      3000      *
#>   -------
#>   seqinfo: 3 sequences from an unspecified genome; no seqlengths
```
