# GenomicCoordinates: Main parsing function

Automatically parse genomic coordinate strings into the most appropriate
Bioconductor object type (GRanges, GPos, GInteractions, or IRanges).
Parse strings into appropriate genomic objects

## Usage

``` r
GenomicCoordinates(x, force_class = NULL)
```

## Arguments

- x:

  Character string or vector of genomic coordinates

- force_class:

  Optional class to force ("GRanges", "GPos", "GInteractions",
  "IRanges")

## Value

GRanges, GPos, GInteractions, or IRanges object

## Details

This is the main function of the GenomicCoordinates package. It
automatically detects the most appropriate object type based on the
input string format and returns the corresponding Bioconductor object.

## Examples

``` r
# Auto-detection examples
GenomicCoordinates("chr1:1000-2000")           # Returns GRanges
#> GRanges object with 1 range and 0 metadata columns:
#>       seqnames    ranges strand
#>          <Rle> <IRanges>  <Rle>
#>   [1]     chr1 1000-2000      *
#>   -------
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths
GenomicCoordinates("chr1:1000")                # Returns GPos  
#> UnstitchedGPos object with 1 position and 0 metadata columns:
#>       seqnames       pos strand
#>          <Rle> <integer>  <Rle>
#>   [1]     chr1      1000      *
#>   -------
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths
GenomicCoordinates("chr1:1-10|chr2:4-40")      # Returns GInteractions
#> GInteractions object with 1 interaction and 0 metadata columns:
#>       seqnames1   ranges1 strand1     seqnames2   ranges2 strand2
#>           <Rle> <IRanges>   <Rle>         <Rle> <IRanges>   <Rle>
#>   [1]      chr1      1-10       * ---      chr2      4-40       *
#>   -------
#>   regions: 2 ranges and 0 metadata columns
#>   seqinfo: 2 sequences from an unspecified genome; no seqlengths
GenomicCoordinates("1000-2000")               # Returns IRanges
#> IRanges object with 1 range and 0 metadata columns:
#>           start       end     width
#>       <integer> <integer> <integer>
#>   [1]      1000      2000      1001

# Force specific class
GenomicCoordinates("chr1:1000", force_class = "GRanges")
#> GRanges object with 1 range and 0 metadata columns:
#>       seqnames    ranges strand
#>          <Rle> <IRanges>  <Rle>
#>   [1]     chr1      1000      *
#>   -------
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths

# Enhanced format support
GenomicCoordinates("chr1:100,000-200,000")     # Comma-separated
#> GRanges object with 1 range and 0 metadata columns:
#>       seqnames        ranges strand
#>          <Rle>     <IRanges>  <Rle>
#>   [1]     chr1 100000-200000      *
#>   -------
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths
GenomicCoordinates("chr1 1000 2000")           # Space-delimited
#> GRanges object with 1 range and 0 metadata columns:
#>       seqnames    ranges strand
#>          <Rle> <IRanges>  <Rle>
#>   [1]     chr1 1000-2000      *
#>   -------
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths
```
