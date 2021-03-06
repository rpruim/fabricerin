
<!-- README.md is generated from README.Rmd. Please edit that file -->

# fabricerin

<!-- badges: start -->

[![Travis build
status](https://travis-ci.com/feddelegrand7/fabricerin.svg?branch=master)](https://travis-ci.com/feddelegrand7/fabricerin)
[![CRAN\_time\_from\_release](https://www.r-pkg.org/badges/ago/fabricerin)](https://cran.r-project.org/package=fabricerin)
[![CRAN\_latest\_release\_date](https://www.r-pkg.org/badges/last-release/fabricerin)](https://cran.r-project.org/package=fabricerin)
[![metacran
downloads](https://cranlogs.r-pkg.org/badges/fabricerin)](https://cran.r-project.org/package=fabricerin)
[![metacran
downloads](https://cranlogs.r-pkg.org/badges/grand-total/fabricerin)](https://cran.r-project.org/package=fabricerin)
[![license](https://img.shields.io/github/license/mashape/apistatus.svg)](https://choosealicense.com/licenses/mit/)
[![R
badge](https://img.shields.io/badge/Build%20with-♥%20JS%20and%20R-pink)](https://github.com/feddelegrand7/fabricerin)

<!-- badges: end -->

The `fabricerin` (spelled **fabrikerine**) package allows you to create
easily canvas elements within your Shiny app and RMarkdown documents.
Thanks to [Garrick Aden-Buie](https://twitter.com/grrrck?lang=en), you
can also use it within your
[xaringan](https://github.com/yihui/xaringan) slides. You can use the
canvas to render shapes, images and text. You can also create a canvas
for drawing/taking notes purposes. Under the hoods, `fabricerin` relies
on the [fabricjs](http://fabricjs.com/) JavaScript library.

## Installation

You can install `fabricerin` from
[CRAN](https://CRAN.R-project.org/package=fabricerin) with:

``` r
install.packages("fabricerin")
```

You can install the development version from
[GitHub](https://github.com/) with:

``` r
# install.packages("remotes")

remotes::install_github("feddelegrand7/fabricerin")
```

## Examples:

First of all, I’d like to state that all the example provided apply the
same way to Shiny and Rmd documents. `fabricerin` is not an R wrapper
for the fabricjs library. The package doesn’t cover all the capabilities
of the library. The `fabricerin` package relies only on some specified
features that according to me will help Shiny/Rmd users. Of course, if
you need some improvement, feel free to create a Pull Request.

### `fabric_drawing()` Create a canvas for taking notes

------------------------------------------------------------------------

`fabric_drawing()` is pretty useful when you want to teach something and
write some notes at the same time, below I provide an example using the
`xaringan` package. Inside a `xaringan` slide you can just (for example)
run R code in the left and take notes in the right:

> Important: When you change a color, make sure that the **erase** box
> is not checked.

![](man/figures/t_example.gif)

`fabric_drawing()` can be used the same way in Shiny:

``` r
library(shiny)
library(fabricerin)


ui <- fluidPage(
  
  
  fabric_drawing(cid = "canvas123")

)

server <- function(input, output){}

shinyApp(ui, server)
```

![](man/figures/example1.gif)

## `fabric_shape()` Render shape objects in canvas

------------------------------------------------------------------------

Currently, `fabricerin` supports three types of shapes: Rectangle,
Triangle, Circle and Polygon. The user can interact with the shape and
modify its position, size and rotation. If you want to disable this
interactivity, you can set `selectable =FALSE`

``` r
library(shiny)
library(fabricerin)


ui <- fluidPage(
  

  fabric_shape(cid = "canvaId", # canvas id
               cfill = "orange", # canvas color
               cwidth = 800, # the width of the canvas
               cheight = 600, # the height of the canvas
               shapeId = "shapeId", # shape id
               shape = "Rect", 
               fill = "red", # shape color
               width = 400, 
               height = 400, 
               left = 100, # the position of the shape from the left relative to the canvas
               top = 100, # the position of the shape from the top relative to the canvas
               strokecolor = "darkblue", 
               strokewidth = 5, 
               selectable = TRUE)
  
)

server <- function(input, output){}

shinyApp(ui, server)
```

![](man/figures/example3.gif)

You can add as many shape as you want to an existing canvas using the
`fabric_shape_add()` function. **Don’t forget to reference the
preexisting canvas with its ID:**

``` r
library(shiny)
library(fabricerin)


ui <- fluidPage(
  
  
  fabric_shape(cid = "canvaId", 
               shapeId = "cr1", 
               shape = "Circle", 
               radius = 30, 
               left = 100), 
  
  fabric_shape_add(cid = "canvaId", 
                   shapeId = "cr2", 
                   shape = "Circle", 
                   radius = 30, 
                   left = 200),
  
  fabric_shape_add(cid = "canvaId", 
                   shapeId = "cr3", 
                   shape = "Circle", 
                   radius = 30, 
                   left = 300),
  
  fabric_shape_add(cid = "canvaId", 
                   shapeId = "cr4", 
                   shape = "Circle", 
                   radius = 30, 
                   left = 400)
  
)

server <- function(input, output){}

shinyApp(ui, server)
```

![](man/figures/example4.png)

## `fabric_image()` Render images in canvas

------------------------------------------------------------------------

You can insert an image within a canvas a play with it using the
`fabric_image()` function. Note that this function accepts only URL
external images.

``` r
ui <- fluidPage(

fabric_image(cid = "cimage",
              cfill = "lightblue",
              imgId = "Rimg",
              imgsrc = "https://upload.wikimedia.org/wikipedia/commons/thumb/1/1b/R_logo.svg/724px-R_logo.svg.png")

              )

server <- function(input, output) {}


shinyApp(ui = ui, server = server)
```

![](man/figures/example5.gif)

Similar to shapes, you can add images to preexisting canvas using the
`fabric_image_add()` function:

``` r
library(shiny)
library(fabricerin)

ui <- fluidPage(

 fabric_image(cid = "cimage",
              imgId = "Rimg",
              imgsrc = "https://upload.wikimedia.org/wikipedia/commons/thumb/1/1b/R_logo.svg/724px-R_logo.svg.png",
              imgheight = 200,
              imgwidth = 200),
 fabric_image_add(cid = "cimage",
                  imgId = "rstudioimg",
                  imgsrc = "https://raw.githubusercontent.com/rstudio/hex-stickers/master/PNG/dplyr.png",
                  imgwidth = 200,
                  imgheight = 200,
                  left = 400)
                  )

server <- function(input, output) {}

shinyApp(ui = ui, server = server)
```

![](man/figures/r_dplyr_logo.png)

## `fabric_text()` Render text elements in canvas

------------------------------------------------------------------------

The `fabric_text()` function has many arguments, feel free to check them
out:

``` r
ui <- fluidPage(

fabric_text(cid = "cId",
          textId = "text",
          text = " 'But A Hero Is A Guy Who Gives Out The Meat To Everyone Else. \\n I Want To Eat The Damn Meat!' \\n Monkey D. Luffy",
          cfill = "#DD5347",
          left = 120,
          shadowCol = "blue",
          fontSize = 20,
          fontWeight = "bold",
          lineHeight = 3
          )
)
server <- function(input, output) {}

shinyApp(ui = ui, server = server)
```

![](man/figures/example6.gif)

Here also, we can use the `fabric_text_add()` function to incorporate a
text object within a canvas element:

``` r
library(shiny)
library(fabricerin)


ui <- fluidPage(

fabric_shape(cid = "canvas123",
              cfill = "lightblue",
              cwidth = 1000,
              shapeId = "tri1",
              shape = "Triangle",
              fill = "darkblue"),

fabric_text_add(cid = "canvas123",
                 textId = "txt1",
                 text = "This is a darkblue Triangle !",
                 left = 350
                 )

                 )

server <- function(input, output) {}

shinyApp(ui = ui, server = server)
```

![](man/figures/example7.png)

## `fabric_curtail()` Add a background or an overlay image to a canvas

You can set an image as a background or as a foreground (overlay) for a
canvas as follows:

> Note that due to security reasons, you won’t be able to replicate the
> following example on some images’ sources.

``` r
ui <- fluidPage(

 fabric_shape(cid = "canvas123",
              shapeId = "tri1",
              shape = "Triangle",
              fill = "lightblue"),

fabric_curtail(cid = "canvas123",
              imgsrc = "https://st.depositphotos.com/1642482/1904/i/950/depositphotos_19049237-stock-photo-leaf.jpg",
              type = "background"

              )

)

server <- function(input, output) {}


shinyApp(ui = ui, server = server)
```

![](man/figures/example8.png)

## Code of Conduct

Please note that the fabricerin project is released with a [Contributor
Code of
Conduct](https://contributor-covenant.org/version/2/0/CODE_OF_CONDUCT.html).
By contributing to this project, you agree to abide by its terms.
