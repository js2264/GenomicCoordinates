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
