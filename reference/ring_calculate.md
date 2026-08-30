# Generate a ring-width series

This function can calculate ring widths according to coordinates of
detected ring borders.

## Usage

``` r
ring_calculate(ring.data, seriesID)
```

## Arguments

- ring.data:

  A matrix or array produced by
  [`ring_detect`](https://docs.ropensci.org/MtreeRing/reference/ring_detect.md)
  or
  [`ring_modify`](https://docs.ropensci.org/MtreeRing/reference/ring_modify.md).

- seriesID:

  A character string specifying the column name of the ring-width
  series.

## Value

A data frame. The series ID is the column name and years are row names.
The measurements units are millimeters (mm).

## Author

Jingning Shi

## Examples

``` r
img.path <- system.file("001.png", package = "MtreeRing")

## Read a tree ring image:
t1 <- ring_read(img = img.path, dpi = 1200)

## Split a long core sample into 3 pieces to
## get better display performance and use the
## watershed algorithm to detect ring borders:
t2 <- ring_detect(ring.data = t1, seg = 3, method = 'watershed')
#> Warning: The sampling year is set to the current year

## Calculate ring widths from the attribute list of t2:
rw.df <- ring_calculate(ring.data = t2, seriesID = "940220")
```
