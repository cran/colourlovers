# R client for the COLOURlovers API #

[![CRAN Version](http://www.r-pkg.org/badges/version/colourlovers)](http://cran.r-project.org/package=colourlovers)
![Downloads](http://cranlogs.r-pkg.org/badges/colourlovers)
[![Build Status](https://travis-ci.org/leeper/colourlovers.png?branch=master)](https://travis-ci.org/leeper/colourlovers)
[![codecov.io](http://codecov.io/github/leeper/colourlovers/coverage.svg?branch=master)](http://codecov.io/github/leeper/colourlovers?branch=master)

The **colourlovers** package connects R to the [COLOURlovers](http://www.colourlovers.com/) API. COLOURlovers is a social networking site for sharing colors, color palettes, and color-rich visual designs. The social networking features of the site mean that COLOURlovers provides not only rich, original color palettes to use in R graphics but also provides ratings and community evaluations of those palettes, helping R graphics designers to utilize visually pleasing color combinations.

## License ##

The **colourlovers** package is released under GPL-2, while the COLOURlovers community-generated data returned by the API is available under the [Creative Commons Attribution-NonCommercial-ShareAlike 3.0](http://creativecommons.org/licenses/by-nc-sa/3.0/) license.

## Installation ##

You can find a stable release on [CRAN](http://cran.r-project.org/web/packages/colourlovers/index.html):


```R
install.packages("colourlovers")
```

or install the latest development version from GitHub using [Hadley's](http://had.co.nz/) [devtools](http://cran.r-project.org/web/packages/devtools/index.html) package:

```R
if (!require("devtools")) {
    install.packages("devtools")
    library("devtools")
}
install_github("leeper/colourlovers")
```

## Using colourlovers in R graphics ##

The `plot` method for the various **colourlovers** functions pulls PNG-formatted images from the COLOURlovers website and displays them in R graphics, which is helpful for previewing particular colors, palettes, or patterns. But, using the returned colors in R graphics requires extracting the relevant colors and using them in some way. Thus the function `swatch` extracts color information from any of the function return values, and converts them to a character vector of hexidecimal color representations, which can easily be directly plugged into subsequent graphics calls.

Here's a simple `barplot` example using the built-in `VADeaths` dataset redesigned using four different top color patterns:


```r
library("colourlovers")
```

```
## Warning: package 'colourlovers' was built under R version 3.2.3
```

```r
palette1 <- clpalette('113451')
palette2 <- clpalette('92095')
palette3 <- clpalette('629637')
palette4 <- clpalette('694737')
```

```R
layout(matrix(1:4, nrow=2))
par(mar=c(2,2,2,2))
barplot(VADeaths, col = swatch(palette1)[[1]], border = NA)
barplot(VADeaths, col = swatch(palette2)[[1]], border = NA)
barplot(VADeaths, col = swatch(palette3)[[1]], border = NA)
barplot(VADeaths, col = swatch(palette4)[[1]], border = NA)
```

The result:

![Matrix of plots](http://i.imgur.com/KQOFx9G.png)


## Details of Package Functionality ##

The API functionality is broken down into five categories: colors, palettes, patterns, lovers, and statistics. The next sections provide examples of each.

Note that the `clcolor()`, `clcolors()`, `clpalette()`, `clpalettes()`, `clpattern()`, and `clpatterns()` functions all have S3 `plot()` methods. These methods produce either simple plots of colors, palettes, and patterns using `rasterImage()` (and the `png::readPNG()` function) or a pie chart of the returned color values (e.g., `plot(obj, type='pie')`).

Additionally the `swatch` function extracts colors returned by any of those functions to make them easily usable in subsequent graphics calls. For example:


```r
swatch(clcolors('random'))
```

```
## [[1]]
## [1] "#0745C0"
```

```r
swatch(clpalettes('random'))
```

```
## [[1]]
## [1] "#C9C783" "#F8E9C5" "#CDAD70" "#BFD0B0" "#44372E"
```

### Get Colors ###

Two functions retrieve information about individual colors from COLOURlovers. The first, `clcolors()` (in plural form), searches for colors according a number of named attributes.


```r
clcolors('top')[[1]]
```

```
## Pattern ID:      14 
## Title:           Black 
## Created by user: ninjascience 
## Date created:    2004-12-17 08:36:26 
## Views:           127535 
## Votes:           1415 
## Comments:        1784 
## Hearts:          4.5 
## Rank:            1 
## URL:             http://www.colourlovers.com/color/000000/Black 
## Image URL:       
## Colors:          #000000
```

The second function, `clcolor()` (in singular form), retrieves information about a single color based on its six-character hexidecimal representation.


```r
clcolor('6B4106')
```

```
## Pattern ID:      903893 
## Title:           wet dirt 
## Created by user: jessicabrown 
## Date created:    2008-03-17 11:22:21 
## Views:           417 
## Votes:           1 
## Comments:        0 
## Hearts:          0 
## Rank:            0 
## URL:             http://www.colourlovers.com/color/6B4106/wet_dirt 
## Image URL:       
## Colors:          #6B4106
```

`clcolor()` automatically removes leading hashes (`#`) and trailing alpha-transparency values, allowing colors returned **grDevices** functions to be passed directly to `clcolor`. For example:


```r
clcolor(hsv(.5,.5,.5))
```

```
## Pattern ID:      34152 
## Title:           H, S & B 
## Created by user: serafim 
## Date created:    2005-12-15 08:07:52 
## Views:           547 
## Votes:           5 
## Comments:        4 
## Hearts:          0 
## Rank:            0 
## URL:             http://www.colourlovers.com/color/408080/H_S_B 
## Image URL:       
## Colors:          #408080
```

```r
clcolor(rgb(0, 1, 0, .5))
```

```
## Pattern ID:      1513 
## Title:           Primary Green 
## Created by user: il morto 
## Date created:    2005-07-04 09:53:05 
## Views:           3858 
## Votes:           52 
## Comments:        32 
## Hearts:          0 
## Rank:            522 
## URL:             http://www.colourlovers.com/color/00FF00/Primary_Green 
## Image URL:       
## Colors:          #00FF00
```

```r
clcolor(gray(.5))
```

```
## Pattern ID:      8335 
## Title:           917~choice grey 
## Created by user: DESIGNJUNKEE 
## Date created:    2005-08-29 11:30:26 
## Views:           7343 
## Votes:           43 
## Comments:        16 
## Hearts:          5 
## Rank:            722 
## URL:             http://www.colourlovers.com/color/808080/917~choice_grey 
## Image URL:       
## Colors:          #808080
```

The response includes RGB and HSV representations of the requested color, a URL for for an image of the color, and COLOURlovers ratings (views, votes, comments, hearts, and rank) for the color.

Here's an example of the image URL at work, using the `plot` method for a `clcolor` object:

```R
plot(clcolor('00FF00'))
```

[![Primary Green #00FF00](http://www.colourlovers.com/img/00FF00/100/100/Primary_Green.png)](http://www.colourlovers.com/img/00FF00/100/100/Primary_Green.png)

### Get Palettes ###

Palettes are sets of colors created by COLOURlovers users. They show potentially attractive combinations of colors, and are the most useful aspect of the COLOURlovers API in an R context.

Two functions are provided for using palettes. One, `clpalettes` (in plural form), searches for palettes by user, hue(s), color(s) (in hexidecimal representation), or keyword(s).

```R
top <- clpalettes('top')
# plot all top palettes (interactively)
plot(top)
# plot them all as pie charts of the included colors
plot(top, type='pie')
# extract color swatches from new palettes
swatch(top)
```

The other function, `clpalette()` (in singular form), retrieves a pattern by its identifying number.

```R
palette1 <- clpalette('113451')
# plot the palette
plot(palette1)
```

Here's an example of the image URL at work (credit "Anaconda" (113451) by kunteper):

[![Anaconda (113451) by kunteper](http://www.colourlovers.com/paletteImg/2B2D42/7A7D7F/B1BBCF/6E0B21/9B4D73/Anaconda.png)](http://www.colourlovers.com/paletteImg/2B2D42/7A7D7F/B1BBCF/6E0B21/9B4D73/Anaconda.png)


### Get Patterns (Designs) ###

Patterns are images created on COLOURlovers using a specified palette of colors. They serve as examples of how to use the palette in practice.

Two functions are provided for using patterns. One, `clpatterns()` (in plural form), searches for patterns according to user, hue(s), color(s) (in hexidecimal representation), or keyword(s).


```r
clpatterns('top')[[1]]
```

```
## Pattern ID:      5025312 
## Title:           A Beautiful Forever 
## Created by user: paintglass 
## Date created:    2015-12-03 21:31:11 
## Views:           3100 
## Votes:           16777215 
## Comments:        6 
## Hearts:          0 
## Rank:            0 
## URL:             http://www.colourlovers.com/pattern/5025312/A_Beautiful_Forever 
## Image URL:       
## Colors:          #470026, #D10070, #F2430C, #CDA62F, #FFF3E6
```

The other function, `clpattern()` (in singular form), retrieves a pattern by its identifying number.


```r
pattern1 <- clpattern('1451')
# extract colors from the pattern
swatch(pattern1)
```

```
## [[1]]
## [1] "#52202E" "#1A1313" "#F7F6A8" "#C4F04D"
```

The response includes the creator's username, COLOURlovers ratings (views, votes, comments, hearts, and rank), the palette of colors (in hexidecimal representation) used in the pattern, and URLs for the images of the pattern.

Here's an example of the image URL at work (credit "Geek Chic" (1451) by _183):

```R
plot(clpattern('1451'))
```

[![Geek Chic (1451) by _183](http://colourlovers.com.s3.amazonaws.com/images/patterns/1/1451.png)](http://colourlovers.com.s3.amazonaws.com/images/patterns/1/1451.png)


Using `plot(clpattern('1451'), type='pie')`, the `plot` method extracts the `swatch` for the pattern (or a palette or color) and displays the results as a pie chart, with each color labeled:

[![Pie chart for pattern 1451](http://i.imgur.com/ESgl9pYm.png)](http://imgur.com/ESgl9pY)

### Get Lovers (Users) ###

Two functions are provided for viewing information about COLOURlovers use. One, `cllovers()` (in plural form), serves as a limited search function for identifying users. The function provides an argument `set` to identify "new" or "top"-rated users, sorted by one or more attributes.


```r
cllovers(set='top')[[1]]
```

```
## Lover username:      popok302 
## Registered:          2015-06-19 18:37:46 
## Last active:         2015-06-25 12:38:25 
## Rating:              16777215 
## Location:             
## Colors:              0 
## Palettes:            0 
## Patterns:            0 
## Comments made:       0 
## Lovers:              0 
## Comments on profile: 0 
## URL:                 http://www.colourlovers.com/lover/popok302 
## API URL:             http://www.colourlovers.com/api/lover/popok302
```

The other function, `cllover()` (in singular form), pulls the same information for a named user.


```r
cllover('COLOURlovers')
```

```
## Lover username:      colourlovers 
## Registered:          2007-09-11 02:44:32 
## Last active:         2011-07-21 12:26:10 
## Rating:              0 
## Location:             
## Colors:              0 
## Palettes:            0 
## Patterns:            0 
## Comments made:       0 
## Lovers:              0 
## Comments on profile: 0 
## URL:                 http://www.colourlovers.com/lover/colourlovers 
## API URL:             http://www.colourlovers.com/api/lover/colourlovers
```


### Get Statistics ###

**colourlovers** provides a function, `clstats()`, to return basic counts of available colors, palettes, patterns, and lovers. There is no obvious R application for this information, but is provided for the sake of completeness.


```r
clstats('colors')
```

```
## Total colors: 1
```

```r
clstats('palettes')
```

```
## Total palettes: 1
```

```r
clstats('patterns')
```

```
## Total patterns: 1
```

```r
clstats('lovers')
```

```
## Total lovers: 1
```

