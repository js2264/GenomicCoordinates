# Conversion methods for genomic coordinates

Methods to convert character strings to GRanges, GPos, and GInteractions
objects with support for various string formats including
comma-separated numbers and space-delimited coordinates.

Extensions to IRanges parsing to handle comma-separated numbers and
space-delimited coordinates.

## Usage

``` r
# S4 method for class 'character'
as_granges(.data, ..., keep_mcols = TRUE)

# S4 method for class 'character'
as_gpos(.data, ...)

# S4 method for class 'character'
as_ginteractions(
  .data,
  ...,
  keep.extra.columns = TRUE,
  starts.in.df.are.0based = FALSE
)

# S4 method for class 'character'
as_iranges(.data, ..., keep_mcols = TRUE)
```

## Arguments

- .data:

  A character vector of coordinate strings

- ...:

  Additional arguments (unused)

- keep_mcols:

  Ignored for character input (included for generic compatibility with
  plyranges)

- keep.extra.columns:

  Ignored for character input (included for generic compatibility with
  plyinteractions)

- starts.in.df.are.0based:

  Ignored for character input (included for generic compatibility with
  plyinteractions)

## Value

The appropriate Bioconductor object type

An IRanges object

## Examples

``` r
# GRanges conversion
as_granges("chr1:1000-2000")
#> GRanges object with 1 range and 0 metadata columns:
#>       seqnames    ranges strand
#>          <Rle> <IRanges>  <Rle>
#>   [1]     chr1 1000-2000      *
#>   -------
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths
as_granges("chr1:1,000-2,000:+")
#> GRanges object with 1 range and 0 metadata columns:
#>       seqnames    ranges strand
#>          <Rle> <IRanges>  <Rle>
#>   [1]     chr1 1000-2000      +
#>   -------
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths
as_granges(c("chr1:1000-2000", "chr2:3000-4000"))
#> GRanges object with 2 ranges and 0 metadata columns:
#>       seqnames    ranges strand
#>          <Rle> <IRanges>  <Rle>
#>   [1]     chr1 1000-2000      *
#>   [2]     chr2 3000-4000      *
#>   -------
#>   seqinfo: 2 sequences from an unspecified genome; no seqlengths

# GPos conversion
as_gpos("chr1:1000")
#> UnstitchedGPos object with 1 position and 0 metadata columns:
#>       seqnames       pos strand
#>          <Rle> <integer>  <Rle>
#>   [1]     chr1      1000      *
#>   -------
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths
as_gpos(c("chr1:1000", "chr2:2000"))
#> UnstitchedGPos object with 2 positions and 0 metadata columns:
#>       seqnames       pos strand
#>          <Rle> <integer>  <Rle>
#>   [1]     chr1      1000      *
#>   [2]     chr2      2000      *
#>   -------
#>   seqinfo: 2 sequences from an unspecified genome; no seqlengths

# GInteractions conversion
as_ginteractions("chr1:1-10|chr2:20-30")
#> GInteractions object with 1 interaction and 0 metadata columns:
#>       seqnames1   ranges1 strand1     seqnames2   ranges2 strand2
#>           <Rle> <IRanges>   <Rle>         <Rle> <IRanges>   <Rle>
#>   [1]      chr1      1-10       * ---      chr2     20-30       *
#>   -------
#>   regions: 2 ranges and 0 metadata columns
#>   seqinfo: 2 sequences from an unspecified genome; no seqlengths

as_iranges("1000-2000")
#> IRanges object with 1 range and 0 metadata columns:
#>           start       end     width
#>       <integer> <integer> <integer>
#>   [1]      1000      2000      1001
as_iranges("1,000-2,000")
#> IRanges object with 1 range and 0 metadata columns:
#>           start       end     width
#>       <integer> <integer> <integer>
#>   [1]      1000      2000      1001
as_iranges(c("100-200", "300-400"))
#> IRanges object with 2 ranges and 0 metadata columns:
#>           start       end     width
#>       <integer> <integer> <integer>
#>   [1]       100       200       101
#>   [2]       300       400       101
```
