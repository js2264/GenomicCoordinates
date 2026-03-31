# GenomicCoordinates: Enhanced string parsing for genomic coordinates

The GenomicCoordinates package extends the string parsing capabilities
for genomic coordinates in Bioconductor. It supports various string
formats including comma-separated numbers, space-delimited coordinates,
and automatically detects whether to return GRanges, GPos, or
GInteractions objects.

## Supported formats

- Standard format: "chr1:1000-2000", "chr1:1000-2000:+"

- Comma-separated: "chr1:1,000-2,000", "chr1:1,000,000-2,000,000"

- Space-delimited: "chr1 1000 2000"

- Single positions: "chr1:1000" (returns GPos)

- Interactions: "chr1:1000-2000\|chr2:3000-4000" (returns GInteractions)

## Main functions

- `GenomicCoordinates(x)`: Main function - auto-detect and convert to
  appropriate type

- `detect_genomic_class(x)`: Detect appropriate class without parsing

- `as_granges(x)`: Convert character to GRanges

- `as_gpos(x)`: Convert character to GPos

- `as_iranges(x)`: Convert character to IRanges

- `as_ginteractions(x)`: Convert character to GInteractions

## See also

Useful links:

- <https://github.com/js2264/GenomicCoordinates>

- Report bugs at <https://github.com/js2264/GenomicCoordinates/issues>

## Author

**Maintainer**: Jacques Serizay <jacquesserizay@gmail.com>
([ORCID](https://orcid.org/0000-0002-4295-0624))
