# GenomicCoordinates: Enhanced String Parsing for Genomic Coordinates

## Introduction

The standard Bioconductor packages support basic coordinate strings like
`IRanges("1000-2000")` and `GRanges("chr1:1000-2000")`. The
`GenomicCoordinates` package extends these string parsing capabilities
to support a wide variety of genomic coordinate string formats. In
particular, it support automatic detection to coerce string coordinates
into the most appropriate Bioconductor object type (`GRanges`, `GPos`,
`GInteractions`, or `IRanges`), based on the string format.

In addition to the standard string format `CHR:START-END(:STRAND)`,
`GenomicCoordinates` adds support for:

- **Comma-separated coordinates**: `"chr1:1,000-2,000"`,
  `"chr1:100,000-200,000"`;
- **Space-delimited coordinates**: `"chr1 1000 2000"`;
- **Irregular spacing**: `"chr1 1000 2000"`,
  `"chr1: 1-10 | chr2: 20-30"`;
- **Complex strings with unclear formatting**:
  `"chr1 1 10:+ | chr2:20,000-30,000"`.

Key `GenomicCoordinates` benefits include:

1.  **Broader format support**: see above;
2.  **Automatic object detection**: Get the most appropriate
    Bioconductor object type automatically;
3.  **Seamless integration**: Works with existing Bioconductor workflows
    through explicit conversion functions;
4.  **Performance**: Supports parsing of large coordinate vectors;

### Installation

``` r

# Install from Bioconductor (when available)
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install("GenomicCoordinates")

# Or install development version from GitHub
BiocManager::install("js2264/GenomicCoordinates")
```

### Quick start

``` r

library(GenomicCoordinates)
#> Loading required package: GenomicRanges
#> Loading required package: stats4
#> Loading required package: BiocGenerics
#> Loading required package: generics
#> 
#> Attaching package: 'generics'
#> The following objects are masked from 'package:base':
#> 
#>     as.difftime, as.factor, as.ordered, intersect, is.element, setdiff,
#>     setequal, union
#> 
#> Attaching package: 'BiocGenerics'
#> The following objects are masked from 'package:stats':
#> 
#>     IQR, mad, sd, var, xtabs
#> The following objects are masked from 'package:base':
#> 
#>     anyDuplicated, aperm, append, as.data.frame, basename, cbind,
#>     colnames, dirname, do.call, duplicated, eval, evalq, Filter, Find,
#>     get, grep, grepl, is.unsorted, lapply, Map, mapply, match, mget,
#>     order, paste, pmax, pmax.int, pmin, pmin.int, Position, rank,
#>     rbind, Reduce, rownames, sapply, saveRDS, table, tapply, unique,
#>     unsplit, which.max, which.min
#> Loading required package: S4Vectors
#> 
#> Attaching package: 'S4Vectors'
#> The following object is masked from 'package:utils':
#> 
#>     findMatches
#> The following objects are masked from 'package:base':
#> 
#>     expand.grid, I, unname
#> Loading required package: IRanges
#> Loading required package: Seqinfo
```

The main function
[`GenomicCoordinates()`](../reference/GenomicCoordinates.md) (and its
alias `GCoordinates()`) automatically detects the most appropriate
object type and returns the corresponding Bioconductor object:

``` r

# Standard genomic ranges
GenomicCoordinates("chr1:1000-2000")
#> GRanges object with 1 range and 0 metadata columns:
#>       seqnames    ranges strand
#>          <Rle> <IRanges>  <Rle>
#>   [1]     chr1 1000-2000      *
#>   -------
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths

# Single positions (returns GPos)
GenomicCoordinates("chr1:1000:-")
#> UnstitchedGPos object with 1 position and 0 metadata columns:
#>       seqnames       pos strand
#>          <Rle> <integer>  <Rle>
#>   [1]     chr1      1000      -
#>   -------
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths

# Genomic interactions (returns GInteractions)
GenomicCoordinates("chr1:1-10:-|chr2:20-30:+")
#> GInteractions object with 1 interaction and 0 metadata columns:
#>       seqnames1   ranges1 strand1     seqnames2   ranges2 strand2
#>           <Rle> <IRanges>   <Rle>         <Rle> <IRanges>   <Rle>
#>   [1]      chr1      1-10       - ---      chr2     20-30       +
#>   -------
#>   regions: 2 ranges and 0 metadata columns
#>   seqinfo: 2 sequences from an unspecified genome; no seqlengths

# Pure numeric ranges (returns IRanges)
GenomicCoordinates("1,000-2,000")
#> IRanges object with 1 range and 0 metadata columns:
#>           start       end     width
#>       <integer> <integer> <integer>
#>   [1]      1000      2000      1001
```

## Enhanced format support

### Comma-separated coordinates

One of the key limitations of standard Bioconductor string parsing is
the lack of support for comma-separated numbers, which are commonly used
in genomic databases and publications.

``` r

# GenomicCoordinates handles comma-separated numbers seamlessly
GenomicCoordinates("chr1:100,000-200,000")
#> GRanges object with 1 range and 0 metadata columns:
#>       seqnames        ranges strand
#>          <Rle>     <IRanges>  <Rle>
#>   [1]     chr1 100000-200000      *
#>   -------
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths

# Works with single positions too
GenomicCoordinates("chr1:1,234,567")
#> UnstitchedGPos object with 1 position and 0 metadata columns:
#>       seqnames       pos strand
#>          <Rle> <integer>  <Rle>
#>   [1]     chr1   1234567      *
#>   -------
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths

# And with strand information
GenomicCoordinates("chr1:100,000-200,000:+")
#> GRanges object with 1 range and 0 metadata columns:
#>       seqnames        ranges strand
#>          <Rle>     <IRanges>  <Rle>
#>   [1]     chr1 100000-200000      +
#>   -------
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths
```

### Space-delimited coordinates

Many genomic file formats and databases use space-delimited coordinates.
`GenomicCoordinates` supports these formats with flexible spacing:

``` r

# Basic space-delimited format
GenomicCoordinates("chr1 1000 2000")
#> GRanges object with 1 range and 0 metadata columns:
#>       seqnames    ranges strand
#>          <Rle> <IRanges>  <Rle>
#>   [1]     chr1 1000-2000      *
#>   -------
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths

# Irregular spacing is handled automatically
GenomicCoordinates("chr1    1000     2000")
#> GRanges object with 1 range and 0 metadata columns:
#>       seqnames    ranges strand
#>          <Rle> <IRanges>  <Rle>
#>   [1]     chr1 1000-2000      *
#>   -------
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths

# Mixed format with strand
GenomicCoordinates("chr1  1000   2000:+")
#> GRanges object with 1 range and 0 metadata columns:
#>       seqnames    ranges strand
#>          <Rle> <IRanges>  <Rle>
#>   [1]     chr1 1000-2000      +
#>   -------
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths

# Single positions with spaces
GenomicCoordinates("chr1 1000")
#> UnstitchedGPos object with 1 position and 0 metadata columns:
#>       seqnames       pos strand
#>          <Rle> <integer>  <Rle>
#>   [1]     chr1      1000      *
#>   -------
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths
```

### Complex chromosome names

Genome assemblies often contain complex chromosome names that standard
parsers struggle with:

``` r

# Chromosome names with spaces (common in some organisms)
GenomicCoordinates("chr I:1000-2000")
#> GRanges object with 1 range and 0 metadata columns:
#>       seqnames    ranges strand
#>          <Rle> <IRanges>  <Rle>
#>   [1]    chr I 1000-2000      *
#>   -------
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths

# Scaffold and contig names
GenomicCoordinates("scaffold_123:1000-2000")
#> GRanges object with 1 range and 0 metadata columns:
#>           seqnames    ranges strand
#>              <Rle> <IRanges>  <Rle>
#>   [1] scaffold_123 1000-2000      *
#>   -------
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths
GenomicCoordinates("GL000001.1:1000-2000")
#> GRanges object with 1 range and 0 metadata columns:
#>         seqnames    ranges strand
#>            <Rle> <IRanges>  <Rle>
#>   [1] GL000001.1 1000-2000      *
#>   -------
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths

# Organism-specific naming conventions
GenomicCoordinates("2L:1000-2000")  # Drosophila
#> GRanges object with 1 range and 0 metadata columns:
#>       seqnames    ranges strand
#>          <Rle> <IRanges>  <Rle>
#>   [1]       2L 1000-2000      *
#>   -------
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths
GenomicCoordinates("chrUn_GL000220v1:1000-2000")  # Unplaced scaffolds
#> GRanges object with 1 range and 0 metadata columns:
#>               seqnames    ranges strand
#>                  <Rle> <IRanges>  <Rle>
#>   [1] chrUn_GL000220v1 1000-2000      *
#>   -------
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths
```

## Automatic object type detection

One of the most powerful features of `GenomicCoordinates` is its ability
to automatically detect the most appropriate Bioconductor object type
based on the input format.

### Single positions vs. ranges

``` r

# Single positions automatically return GPos objects
pos_result <- GenomicCoordinates("chr1:1000")
class(pos_result)
#> [1] "UnstitchedGPos"
#> attr(,"package")
#> [1] "GenomicRanges"
pos(pos_result)
#> [1] 1000

# Ranges return GRanges objects
range_result <- GenomicCoordinates("chr1:1000-2000")
class(range_result)
#> [1] "GRanges"
#> attr(,"package")
#> [1] "GenomicRanges"
start(range_result)
#> [1] 1000
end(range_result)
#> [1] 2000
```

### Genomic interactions

Strings containing the pipe character (`|`) are automatically parsed as
genomic interactions:

``` r

# Simple interaction
interaction <- GenomicCoordinates("chr1:1-10|chr2:20-30")
class(interaction)
#> [1] "GInteractions"
#> attr(,"package")
#> [1] "InteractionSet"

# Check the anchor points
InteractionSet::anchors(interaction, "first")
#> GRanges object with 1 range and 0 metadata columns:
#>       seqnames    ranges strand
#>          <Rle> <IRanges>  <Rle>
#>   [1]     chr1      1-10      *
#>   -------
#>   seqinfo: 2 sequences from an unspecified genome; no seqlengths
InteractionSet::anchors(interaction, "second")
#> GRanges object with 1 range and 0 metadata columns:
#>       seqnames    ranges strand
#>          <Rle> <IRanges>  <Rle>
#>   [1]     chr2     20-30      *
#>   -------
#>   seqinfo: 2 sequences from an unspecified genome; no seqlengths

# Works with complex and formatting too
GenomicCoordinates("chr1 1000 10000  |  chr2:20,000-30,000")
#> GInteractions object with 1 interaction and 0 metadata columns:
#>       seqnames1    ranges1 strand1     seqnames2     ranges2 strand2
#>           <Rle>  <IRanges>   <Rle>         <Rle>   <IRanges>   <Rle>
#>   [1]      chr1 1000-10000       * ---      chr2 20000-30000       *
#>   -------
#>   regions: 2 ranges and 0 metadata columns
#>   seqinfo: 2 sequences from an unspecified genome; no seqlengths
```

### IRanges for non-genomic coordinates

When no chromosome information is present, the parser returns `IRanges`
objects:

``` r

# Pure numeric ranges
numeric_range <- GenomicCoordinates("1000-2000")
class(numeric_range)
#> [1] "IRanges"
#> attr(,"package")
#> [1] "IRanges"

# Works with comma-separated numbers
GenomicCoordinates("1,000-2,000")
#> IRanges object with 1 range and 0 metadata columns:
#>           start       end     width
#>       <integer> <integer> <integer>
#>   [1]      1000      2000      1001

# And space-delimited format
GenomicCoordinates("1000 2000")
#> IRanges object with 1 range and 0 metadata columns:
#>           start       end     width
#>       <integer> <integer> <integer>
#>   [1]      1000      2000      1001
```

## Forcing object types

Sometimes you may want to force a specific object type, regardless of
the automatic detection:

``` r

# Force a single position to be a GRanges instead of GPos
GenomicCoordinates("chr1:1000", force_class = "GRanges")
#> GRanges object with 1 range and 0 metadata columns:
#>       seqnames    ranges strand
#>          <Rle> <IRanges>  <Rle>
#>   [1]     chr1      1000      *
#>   -------
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths

# Force a single position to be a GPos
# instead of GRanges (removes the `end` coordinate)
GenomicCoordinates("chr1:1000-2000", force_class = "GPos")
#> UnstitchedGPos object with 1 position and 0 metadata columns:
#>       seqnames       pos strand
#>          <Rle> <integer>  <Rle>
#>   [1]     chr1      1000      *
#>   -------
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths

# Force a genomic range to be an IRanges (extracts just coordinates)
GenomicCoordinates("chr1:1000-2000", force_class = "IRanges")
#> IRanges object with 1 range and 0 metadata columns:
#>           start       end     width
#>       <integer> <integer> <integer>
#>   [1]      1000      2000      1001

# Force a single range to be a GInteractions (creates self-interaction)
GenomicCoordinates("chr1:1000-2000", force_class = "GInteractions")
#> GInteractions object with 1 interaction and 0 metadata columns:
#>       seqnames1   ranges1 strand1     seqnames2   ranges2 strand2
#>           <Rle> <IRanges>   <Rle>         <Rle> <IRanges>   <Rle>
#>   [1]      chr1 1000-2000       * ---      chr1 1000-2000       *
#>   -------
#>   regions: 1 ranges and 0 metadata columns
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths
```

## Working with vectors

`GenomicCoordinates` efficiently handles vectors of coordinate strings:

``` r

# Mixed vector of single positions and ranges
mixed_coords <- c("chr1:1000", "chr2:2000-3000", "chr3:5000")
GenomicCoordinates(mixed_coords)
#> GRanges object with 3 ranges and 0 metadata columns:
#>       seqnames    ranges strand
#>          <Rle> <IRanges>  <Rle>
#>   [1]     chr1      1000      *
#>   [2]     chr2 2000-3000      *
#>   [3]     chr3      5000      *
#>   -------
#>   seqinfo: 3 sequences from an unspecified genome; no seqlengths

# All single positions return GPos
single_positions <- c("chr1:1000", "chr2:2000", "chr3:3000")
GenomicCoordinates(single_positions)
#> UnstitchedGPos object with 3 positions and 0 metadata columns:
#>       seqnames       pos strand
#>          <Rle> <integer>  <Rle>
#>   [1]     chr1      1000      *
#>   [2]     chr2      2000      *
#>   [3]     chr3      3000      *
#>   -------
#>   seqinfo: 3 sequences from an unspecified genome; no seqlengths

# Vector of interactions
interactions <- c("chr1:1-10|chr2:20-30", "chr3:100-200|chr4:300-400")
GenomicCoordinates(interactions)
#> GInteractions object with 2 interactions and 0 metadata columns:
#>       seqnames1   ranges1 strand1     seqnames2   ranges2 strand2
#>           <Rle> <IRanges>   <Rle>         <Rle> <IRanges>   <Rle>
#>   [1]      chr1      1-10       * ---      chr2     20-30       *
#>   [2]      chr3   100-200       * ---      chr4   300-400       *
#>   -------
#>   regions: 4 ranges and 0 metadata columns
#>   seqinfo: 4 sequences from an unspecified genome; no seqlengths
```

## Explicit conversion functions

`GenomicCoordinates` provides explicit conversion functions that work
with the enhanced string parsing capabilities:

``` r

# Convert to GRanges
as_granges("chr1:100,000-200,000")
#> GRanges object with 1 range and 0 metadata columns:
#>       seqnames        ranges strand
#>          <Rle>     <IRanges>  <Rle>
#>   [1]     chr1 100000-200000      *
#>   -------
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths

# Convert to GPos
as_gpos("chr1:1,234,567")
#> UnstitchedGPos object with 1 position and 0 metadata columns:
#>       seqnames       pos strand
#>          <Rle> <integer>  <Rle>
#>   [1]     chr1   1234567      *
#>   -------
#>   seqinfo: 1 sequence from an unspecified genome; no seqlengths

# Convert to GInteractions
as_ginteractions("chr1:1,000-10,000|chr2:20,000-30,000")
#> GInteractions object with 1 interaction and 0 metadata columns:
#>       seqnames1    ranges1 strand1     seqnames2     ranges2 strand2
#>           <Rle>  <IRanges>   <Rle>         <Rle>   <IRanges>   <Rle>
#>   [1]      chr1 1000-10000       * ---      chr2 20000-30000       *
#>   -------
#>   regions: 2 ranges and 0 metadata columns
#>   seqinfo: 2 sequences from an unspecified genome; no seqlengths

# Convert to IRanges
as_iranges("1,000-2,000")
#> IRanges object with 1 range and 0 metadata columns:
#>           start       end     width
#>       <integer> <integer> <integer>
#>   [1]      1000      2000      1001
```

## Class detection without parsing

The [`detect_genomic_class()`](../reference/detect_genomic_class.md)
function allows you to determine what object type would be returned
without actually performing the parsing:

``` r

# Detect classes for various input types
inputs <- c(
    "chr1:1000-2000",
    "chr1:1000",
    "chr1:1-10|chr2:20-30",
    "1000-2000"
)

detect_genomic_class(inputs)
#> [1] "GRanges"       "GPos"          "GInteractions" "IRanges"
```

This is useful for:

- Pre-processing pipelines where you need to route different input types
- Validation of input data
- Building user interfaces that adapt based on input format

## Session Information

``` r

sessionInfo()
#> R Under development (unstable) (2026-03-28 r89738)
#> Platform: x86_64-pc-linux-gnu
#> Running under: Ubuntu 24.04.4 LTS
#> 
#> Matrix products: default
#> BLAS:   /usr/lib/x86_64-linux-gnu/openblas-pthread/libblas.so.3 
#> LAPACK: /usr/lib/x86_64-linux-gnu/openblas-pthread/libopenblasp-r0.3.26.so;  LAPACK version 3.12.0
#> 
#> locale:
#>  [1] LC_CTYPE=en_US.UTF-8       LC_NUMERIC=C              
#>  [3] LC_TIME=en_US.UTF-8        LC_COLLATE=en_US.UTF-8    
#>  [5] LC_MONETARY=en_US.UTF-8    LC_MESSAGES=en_US.UTF-8   
#>  [7] LC_PAPER=en_US.UTF-8       LC_NAME=C                 
#>  [9] LC_ADDRESS=C               LC_TELEPHONE=C            
#> [11] LC_MEASUREMENT=en_US.UTF-8 LC_IDENTIFICATION=C       
#> 
#> time zone: UTC
#> tzcode source: system (glibc)
#> 
#> attached base packages:
#> [1] stats4    stats     graphics  grDevices utils     datasets  methods  
#> [8] base     
#> 
#> other attached packages:
#> [1] GenomicCoordinates_0.99.3 GenomicRanges_1.63.1     
#> [3] Seqinfo_1.1.0             IRanges_2.45.0           
#> [5] S4Vectors_0.49.0          BiocGenerics_0.57.0      
#> [7] generics_0.1.4            BiocStyle_2.39.0         
#> 
#> loaded via a namespace (and not attached):
#>  [1] SummarizedExperiment_1.41.1 rjson_0.2.23               
#>  [3] xfun_0.57                   bslib_0.10.0               
#>  [5] htmlwidgets_1.6.4           plyranges_1.31.5           
#>  [7] Biobase_2.71.0              lattice_0.22-9             
#>  [9] vctrs_0.7.2                 tools_4.7.0                
#> [11] bitops_1.0-9                curl_7.0.0                 
#> [13] parallel_4.7.0              tibble_3.3.1               
#> [15] pkgconfig_2.0.3             Matrix_1.7-5               
#> [17] desc_1.4.3                  cigarillo_1.1.0            
#> [19] lifecycle_1.0.5             compiler_4.7.0             
#> [21] Rsamtools_2.27.1            textshaping_1.0.5          
#> [23] Biostrings_2.79.5           plyinteractions_1.9.2      
#> [25] codetools_0.2-20            InteractionSet_1.39.0      
#> [27] htmltools_0.5.9             sass_0.4.10                
#> [29] RCurl_1.98-1.18             yaml_2.3.12                
#> [31] pkgdown_2.2.0               pillar_1.11.1              
#> [33] crayon_1.5.3                jquerylib_0.1.4            
#> [35] BiocParallel_1.45.0         DelayedArray_0.37.0        
#> [37] cachem_1.1.0                abind_1.4-8                
#> [39] tidyselect_1.2.1            digest_0.6.39              
#> [41] restfulr_0.0.16             dplyr_1.2.0                
#> [43] bookdown_0.46               fastmap_1.2.0              
#> [45] grid_4.7.0                  cli_3.6.5                  
#> [47] SparseArray_1.11.11         magrittr_2.0.4             
#> [49] S4Arrays_1.11.1             XML_3.99-0.23              
#> [51] httr_1.4.8                  rmarkdown_2.31             
#> [53] XVector_0.51.0              matrixStats_1.5.0          
#> [55] otel_0.2.0                  ragg_1.5.2                 
#> [57] evaluate_1.0.5              knitr_1.51                 
#> [59] BiocIO_1.21.0               rtracklayer_1.71.3         
#> [61] rlang_1.1.7                 Rcpp_1.1.1                 
#> [63] glue_1.8.0                  BiocManager_1.30.27        
#> [65] jsonlite_2.0.0              R6_2.6.1                   
#> [67] MatrixGenerics_1.23.0       GenomicAlignments_1.47.0   
#> [69] systemfonts_1.3.2           fs_2.0.1
```
