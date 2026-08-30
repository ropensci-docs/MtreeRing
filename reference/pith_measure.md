# Calibrate ring-width series

This function can calibrate the ring-width series using arcs of inner
rings.

## Usage

``` r
pith_measure(ring.data, inner.arc = TRUE, last.yr = NULL,
  color = "black", border.type = 16, label.cex = 1.5)
```

## Arguments

- ring.data:

  A magick image object produced by
  [`ring_read`](https://docs.ropensci.org/MtreeRing/reference/ring_read.md).

- inner.arc:

  A logical value indicating whether to calibrate the ring-width series
  using the arcs of inner rings. See details below.

- last.yr:

  `NULL` or an integer giving the year of formation of the left-most
  ring. If `NULL`, border numbers (starting from 1) are used instead of
  years.

- color:

  Color for labels.

- border.type:

  Symbol for ring borders. See `pch` in
  [`points`](https://rdrr.io/r/graphics/points.html) for possible values
  and shapes.

- label.cex:

  The magnification to be used for years or border numbers.

## Value

A data frame of the calibrated ring-width series. The measurements units
are millimeters (mm)

## Details

This function allows the user to create a path, and manually mark ring
borders by clicking on the graphical window.

An example demonstrated with pictures can be found in the package
vignette. Type `vignette('pith-MtreeRing')` to see this example.

- If `inner.arc = TRUE`, the ring-width series is calibrated using arcs
  of inner rings (Duncan, 1989).

  **Step1**. You can click the left mouse button to add a horizontal
  path. The path should traverse an appropriate arc (read the reference
  below for more details).

  **Step2**. You can add three points to the selected arc by
  left-clicking. The first point should be placed on the left endpoint
  of the arc, and the second point is placed on the right endpoint.

  After adding these two points, a vertical dashed line will be plotted
  automatically according to the (x,y) positions of endpoints you just
  added. The third points should be placed on the intersection of the
  vertical dashed line and the selected arc.

  **Step3**. you are prompted to mark tree rings along the path by
  left-clicking on the image. Every click draws a point. Note that the
  left endpoint of the arc will be considered as the last ring border
  without the need to mark it.

  After marking tree rings, the identification process does not
  automatically stop by itself. On the Windows platform, the
  identification process can be terminated by clicking the second button
  and selecting **Stop** from the menu. On the MacOS system, you can
  press the **Escape** key to terminate this process.

  The ring-width series are corrected using formulas proposed by Duncan
  (1989).

- If `inner.arc = FALSE`, the user can create a path which matches the
  direction of wood growth.

  **Step1**. You can add two points by left-clicking on the image. Every
  click draws a point. A path passing through these two points will be
  plotted. The path should follow the rays from bark to pith.

  **Step2**. You can mark tree rings along the path by left-clicking on
  the image. The termination of identification process is similar.

## References

Duncan R. (1989) An evaluation of errors in tree age estimates based on
increment cores in Kahikatea (Dacrycarpus dacrydiodes). *New Zealand
Natural Sciences* **16(4)**, 1-37.

## Author

Jingning Shi

## Examples

``` r
img.path <- system.file("missing_pith.png", package = "MtreeRing")

## Read the image:
t1 <- ring_read(img = img.path, dpi = 1200, plot = FALSE)

## Use the arcs of inner rings to calibrate ring-width series:
t2 <- pith_measure(t1, inner.arc = TRUE, last.yr = 2016)
#> Error in round(step1$y): non-numeric argument to mathematical function

## Try another method to measure ring widths:
t3 <- pith_measure(t1, inner.arc = FALSE, last.yr = 2016)
#> Error in model.frame.default(formula = path.position$y ~ path.position$x +     1, drop.unused.levels = TRUE): invalid type (NULL) for variable 'path.position$y'
```
