# Edit ring borders visually

This function can remove existing ring borders or add new borders.

## Usage

``` r
ring_modify(ring.data, del = NULL, del.u = NULL, del.l = NULL,
  add = FALSE)
```

## Arguments

- ring.data:

  A matrix or array produced by
  [`ring_detect`](https://docs.ropensci.org/MtreeRing/reference/ring_detect.md).

- del:

  A numeric vector giving the border numbers to be removed.

- del.u:

  A numeric vector giving the border numbers to be removed on the upper
  path.

- del.l:

  A numeric vector giving the border numbers to be removed on the lower
  path.

- add:

  A logical value indicating whether to add new ring borders.

## Value

A matrix (grayscale image) or array (color image) representing the tree
ring image.

## Details

This function is used to remove existing ring borders, or to add new
borders by interactively clicking on the image segments.

If the user creates one path (`incline = FALSE`), the argument `del` is
used to remove ring borders. If the user creates two paths
(`incline = TRUE`), arguments `del.u` and `del.l` are used to remove
ring borders.

If `add = TRUE`, graphics windows opened by
[`ring_detect`](https://docs.ropensci.org/MtreeRing/reference/ring_detect.md)
will be activated sequentially. When a graphics window is activated, the
user can add new borders by left-clicking the mouse along the path.
Every click draws a point representing the ring border. Type
`vignette('detection-MtreeRing')` to see an example of adding ring
borders.

The identification process does not automatically stop by itself.

- On the Windows system, the identification process can be terminated by
  pressing the right mouse button and selecting **Stop** from the menu.

- On the MacOS system, for a X11 device the identification process is
  terminated by pressing any mouse button other than the first, and for
  a quartz device this process is terminated by pressing the **ESC**
  key.

Once the user terminates the identification process, the current
graphics window will be closed automatically, and the graphics window of
the following segment is activated. When all graphics windows are
closed, `ring_modify` will re-open graphics windows and plot new
borders.

This function can perform both deletion and addition in one call. The
removal of ring borders takes precedence over addition.

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

## Do not modify t2, but create a new array object t3. 
## Remove some borders without adding new borders:
t3 <- ring_modify(ring.data = t2, del = c(1, 3, 5, 19:21), add = FALSE)
```
