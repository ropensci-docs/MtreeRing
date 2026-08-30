# Read and plot a tree-ring image file

This function can read an image file from the hard disk and plot it in a
newly-opened graphics device.

## Usage

``` r
ring_read(img, dpi = NULL, RGB = c(0.299, 0.587, 0.114),
  plot = FALSE, rotate = 0, magick = TRUE)
```

## Arguments

- img:

  A character string indicating the path of the image file. Supported
  formats include png, tiff, jpg and bmp.

- dpi:

  An integer specifying the dpi of the image file. A minimum of 300 dpi
  is required when running automatic detection.

- RGB:

  A numeric vector of length 3 giving the weight of RGB channels.

- plot:

  A logical value indicating whether to plot the tree ring image when
  reading it. If `FALSE`, the image is not plotted until function
  [`ring_detect`](https://docs.ropensci.org/MtreeRing/reference/ring_detect.md)
  or
  [`pith_measure`](https://docs.ropensci.org/MtreeRing/reference/pith_measure.md)
  is called.

- rotate:

  An integer specifying how many degrees to rotate (clockwise). It
  requires one of the following values: `0`, `90`, `180` or `270`.

- magick:

  A logical value. If `TRUE`, `magick` is used to read the tree ring
  image. If `FALSE`, packages `png`, `jpg` and `tiff` are used instead.
  See details below.

## Value

A magick image object containing the image data.

## Details

Proper image preparation has a great influence on the measurement of
ring widths. A tree-ring image should not contain irrelevant or
redundant features, such as wooden mounts where cores are glued. The
larger the file size of an image, the slower the image processing
operation will be.

**Pith side** of a wood sample should be placed on the **right side** of
a graphics window. Use `rotate` to change its position.

It is highly recommended to use the default value `magick = TRUE`,
because `magick` can significantly reduce the memory usage when reading
a large file. If image data is stored in a non-standard format, image
reading may fail. In that case you can set `magick = FALSE` to avoid the
use of `magick`.

## Author

Jingning Shi

## Examples

``` r
img.path <- system.file("001.png", package = "MtreeRing")

## Read and plot the image:
t1 <- ring_read(img = img.path, dpi = 1200, plot = TRUE)
```
