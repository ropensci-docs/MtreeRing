# Automatic detection of tree-ring borders

This function is used to automatically detect tree ring borders along
the user-defined path.

## Usage

``` r
ring_detect(ring.data, seg = 1, auto.path = TRUE, manual = FALSE,
  method = "canny", incline = FALSE, sample.yr = NULL,
  watershed.threshold = "auto", watershed.adjust = 0.8,
  struc.ele1 = NULL, struc.ele2 = NULL, marker.correction = FALSE,
  default.canny = TRUE, canny.t1, canny.t2, canny.smoothing = 2,
  canny.adjust = 1.4, path.dis = 1, origin = 0,
  border.color = "black", border.type = 16, label.color = "black",
  label.cex = 1.2)
```

## Arguments

- ring.data:

  A magick image object produced by
  [`ring_read`](https://docs.ropensci.org/MtreeRing/reference/ring_read.md).

- seg:

  An integer specifying the number of image segments.

- auto.path:

  A logical value. If `TRUE`, a path is automatically created at the
  center of the image. If `FALSE`, the function allows the user to
  create a sub-image and a path by interactive clickings. See details
  below.

- manual:

  A logical value indicating whether to skip the automatic detection. If
  `TRUE`, ring borders are visually identified after creating the path.
  See
  [`ring_modify`](https://docs.ropensci.org/MtreeRing/reference/ring_modify.md)
  to learn how to mark tree rings by clicking on the image.

- method:

  A character string specifying how ring borders are detected. It
  requires one of the following characters: `"watershed"`, `"canny"`, or
  `"lineardetect"`. See details below.

- incline:

  A logical value indicating whether to correct ring widths. If `TRUE`,
  two horizontal paths are added to the image.

- sample.yr:

  `NULL` or an integer giving the year of formation of the left-most
  ring. If `NULL`, use the current year.

- watershed.threshold:

  The threshold used for producing the marker image, either a numeric
  from 0 to 1, or the character "auto" (using the Otsu algorithm), or a
  character of the form "XX%" (e.g., "58%").

- watershed.adjust:

  A numeric used to adjust the Otsu threshold. The default is 1 which
  means that the threshold will not be adjusted. The sizes of early-wood
  regions in the marker image will reduce along with the decrease of
  `watershed.adjust`.

- struc.ele1:

  `NULL` or a vector of length two specifying the width and height of
  the first structuring element. If `NULL`, the size of the structuring
  element is determined by the argument `dpi`.

- struc.ele2:

  `NULL` or a vector of length two specifying the width and height of
  the second structuring element. If `NULL`, the size of the structuring
  element is determined by the argument `dpi`.

- marker.correction:

  A logical value indicating whether to relabel early-wood regions by
  comparing the values of their left-side neighbours.

- default.canny:

  A logical value. If `TRUE`, upper and lower Canny thresholds are
  determined automatically.

- canny.t1:

  A numeric giving the threshold for weak edges.

- canny.t2:

  A numeric giving the threshold for strong edges.

- canny.smoothing:

  An integer specifying the degree of smoothing.

- canny.adjust:

  A numeric used as a sensitivity control factor for the Canny edge
  detector. The default is 1 which means that the sensitivity will not
  be adjusted. The number of detected borders will reduce along with the
  increase of this value.

- path.dis:

  A numeric specifying the perpendicular distance between two paths when
  the argument `incline = TRUE`. The unit is in mm.

- origin:

  A numeric specifying the origin in smoothed gray to find ring borders.
  See `ringBorders` from the package `measuRing`.

- border.color:

  Color for ring borders.

- border.type:

  Symbol for ring borders. See `pch` in
  [`points`](https://rdrr.io/r/graphics/points.html) for possible values
  and their shapes.

- label.color:

  Color for years and border numbers.

- label.cex:

  The magnification to be used for years and border numbers.

## Value

A matrix (grayscale image) or array (color image) representing the
tree-ring image.

## Details

If `auto.path = FALSE`, the user can create a rectangular sub-image and
a horizontal path by interactively clicking on the tree ring image. The
automatic detection will be performed within this rectangular sub-image.
To create a sub-image and a path, follow these steps.

- Step 1. Select the left and right edges of the rectangle

  The user can point the mouse at any desired locations and click the
  left mouse button to add each edge.

- Step 2. Select the top and bottom edges of the rectangle

  The user can point the mouse at any desired locations and click the
  left mouse button to add each edge. The width of the rectangle is
  defined as the distance between the top and bottom edges, and should
  not be unnecessarily large to reduce time consumption and memory
  usage. Creating a long and narrow rectangle if possible.

- Step 3. Create a path

  After creating the rectangular sub-image, the user can add a
  horizontal path by left-clicking on the sub-image (generally at the
  center of the sub-image, try to choose a clean defect-free area). Ring
  borders and other markers are plotted along this path. If
  `incline = TRUE`, two paths are added simultaneously.

After creating the sub-image and the path, this function will open
several graphics windows and plot detected ring borders on image
segments. The number of image segments is controlled by the argument
`seg`.

Argument `method` determines how ring borders are identified.

- If `method = "watershed"`, this function uses the watershed algorithm
  to obtain ring borders (Soille and Misson, 2001).

- If `method = "canny"`, this function uses the Canny algorithm to
  detect borders.

- If `method = "lineardetect"`, a linear detection algorithm from the
  package `measuRing` is used to identify ring borders (Lara et al.,
  2015). Note that `incline = TRUE` is not supported in this mode, and
  path will be automatically created at the center of the image.

If the argument `method = "watershed"` or `"canny"`, the original image
is processed by morphological openings and closings using rectangular
structuring elements of increasing size before detecting borders. The
first small structuring element is used to remove smaller dark spots in
early wood regions, and the second large structuring element is used to
remove light strips in late wood regions. More details about
morphological processing can be found at Soille and Misson (2001).

## Note

This function uses [`locator`](https://rdrr.io/r/graphics/locator.html)
to record mouse positions so it only works on "X11", "windows" and
"quartz" devices.

## References

Soille, P., Misson, L. (2001) Tree ring area measurements using
morphological image analysis. *Canadian Journal of Forest Research*
**31**, 1074-1083. doi: 10.1139/cjfr-31-6-1074

Lara, W., Bravo, F., Sierra, C.A. (2015) measuRing: An R package to
measure tree-ring widths from scanned images. *Dendrochronologia*
**34**, 43-50. doi: 10.1016/j.dendro.2015.04.002

## Author

Jingning Shi

## Examples

``` r
img.path <- system.file("001.png", package = "MtreeRing")

## Read a tree ring image:
t1 <- ring_read(img = img.path, dpi = 1200, plot = FALSE)

## Split a long core sample into 3 pieces to
## get better display performance and use the
## watershed algorithm to detect ring borders:
t2 <- ring_detect(t1, seg = 3, method = 'watershed', border.color = 'green')
#> Warning: The sampling year is set to the current year
```
