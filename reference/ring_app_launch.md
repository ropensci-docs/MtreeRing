# Run Shiny-based Application

Run a Shiny-based application within the system's default web browser.

## Usage

``` r
ring_app_launch(launch.browser = TRUE)
```

## Arguments

- launch.browser:

  A logical value. If `FALSE`, a built-in browser will be launched
  automatically after the app is started. If `TRUE`, the system's
  default web browser is used instead. This argument only works for
  RStudio. See details below.

## Details

`launch.browser = FALSE` is not recommended, as the file renaming does
not work on the RStudio built-in browser when saving the data.

A workflow for the Shiny app can be found here:
<https://ropensci.github.io/MtreeRing/articles/app-MtreeRing.html>. Most
steps are demonstrated with a gif to make the workflow more
understandable.

To stop the app, go to the R console and press the Escape key. You can
also click the stop sign icon in the upper right corner of the RStudio
console.

## Author

Jingning Shi, Wei Xiang
