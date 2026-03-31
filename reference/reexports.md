# Re-exported functions from plyranges and plyinteractions

These generics are re-exported from plyranges and plyinteractions to
provide conversion functions for character strings.

## Usage

``` r
as_granges(.data, ..., keep_mcols = TRUE)

as_iranges(.data, ..., keep_mcols = TRUE)

as_ginteractions(
  .data,
  ...,
  keep.extra.columns = TRUE,
  starts.in.df.are.0based = FALSE
)
```

## Arguments

- .data:

  Object to convert

- ...:

  Additional arguments passed to methods

- keep_mcols:

  Logical; whether to keep metadata columns (plyranges)

- keep.extra.columns:

  Logical; whether to keep extra columns (plyinteractions)

- starts.in.df.are.0based:

  Logical; whether starts are 0-based (plyinteractions)

## Value

A Bioconductor object

## Examples

``` r
as_granges("chr1:1000-2000")
#> GRanges object with 1 range and 0 metadata columns:
#>       seqnames    ranges strand
#>          <Rle> <IRanges>  <Rle>
#>   [1]     chr1 1000-2000      *
#>   -------
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths
as_iranges("1000-2000")
#> IRanges object with 1 range and 0 metadata columns:
#>           start       end     width
#>       <integer> <integer> <integer>
#>   [1]      1000      2000      1001
as_ginteractions("chr1:1-10|chr2:20-30")
#> GInteractions object with 1 interaction and 0 metadata columns:
#>       seqnames1   ranges1 strand1     seqnames2   ranges2 strand2
#>           <Rle> <IRanges>   <Rle>         <Rle> <IRanges>   <Rle>
#>   [1]      chr1      1-10       * ---      chr2     20-30       *
#>   -------
#>   regions: 2 ranges and 0 metadata columns
#>   seqinfo: 2 sequences from an unspecified genome; no seqlengths
```
