# refsplitr <img src="man/figures/logo.png" align="right" height="68" />
<!-- badges: start -->

[![Project Status: Active The project has reached a stable, usable state and is being actively developed.](http://www.repostatus.org/badges/latest/active.svg)](http://www.repostatus.org/#active)[![](https://badges.ropensci.org/256_status.svg)](https://github.com/ropensci/onboarding/issues/256)
[![Coverage Status](https://coveralls.io/repos/github/ropensci/refsplitr/badge.svg?branch=master)](https://coveralls.io/github/ropensci/refsplitr?branch=master)
[![status](https://joss.theoj.org/papers/3e46a4970fea0da996251617d2fa85ca/status.svg)](https://joss.theoj.org/papers/3e46a4970fea0da996251617d2fa85ca)[![R-CMD-check](https://github.com/embruna/refsplitr/workflows/R-CMD-check/badge.svg)](https://github.com/embruna/refsplitr/actions)
<!-- badges: end -->
             
refsplitr: author name disambiguation, author georeferencing, and mapping of coauthorship networks with Web of Science data. 

refsplitr (v1.0) is an R package to parse and organize reference records downloaded from the Web of Science citation database, disambiguate the names of authors, geocode their locations, and generate/visualize coauthorship networks. The Web of Science (WOS) is a toll-access literature and citation database maintained by Clarivate Analytics that indexes articles from ~12,000 academic journals. WOS records  include a diversity of data about each article (e.g., article title, journal name, author names, author addresses, number of times the article has been cited, funding sources), making them very useful for studying patterns of scientific productivity, coauthorship, research impact, and other Science of Science topics. Because bulk WOS records and API access to the WOS are very expensive, researchers at WOS-subscribing institutions often gather data by conducting WOS searches and downloading reference records in batches. However, this requires a cumbersome process of extracting, merging, and correcting data from the downloaded records prior to conducting any analyses. refsplitr will rapidly merge and process reference data files downloaded from the WOS, and then process and organize them in a format amenable for use in scientometric, social network, and Science of Science analyses. 

Support for the development of refsplitr was provided by grants from the [University of Florida Center for Latin American Studies](http://www.latam.ufl.edu/) and the [University of Florida Informatics Institute](https://informatics.institute.ufl.edu/).

## Installation

You can install the development version from [GitHub](https://github.com/) with:

``` r
# install.packages("devtools")
devtools::install_github("ropensci/refsplitr")
```

## Workflow

There are four steps in the `refsplitr` package's workflow:

1.  Importing and tidying Web of Science reference records (be sure to download records using the procedure in Appendix 1 of the [vignette](https://docs.ropensci.org/refsplitr/articles/refsplitr.html))
2.  Author name disambiguation and parsing of author addresses
3.  Georeferencing of author institutions. (*Important Note*: Google has changed its API requirements, which means users now have register with Google prior to georeferencing. For additional details see the [`ggmap`](https://github.com/dkahle/ggmap) repository and the instructions below.)
4.  Data visualization

The procedures required for these four steps,each of which is implemented with a simple command, are described in detail in the `refsplitr` [vignette](https://docs.ropensci.org/refsplitr/articles/refsplitr.html). An example of this workflow is provided below:


``` r
# load the Web of Science records into a dataframe
dat1 <- references_read(data = system.file("extdata", "example_data.txt", package = "refsplitr"), dir = FALSE)

# load the Web of Science records into a dataframe
dat1 <- references_read(data = system.file("extdata", "example_data.txt", package = "refsplitr"), dir = FALSE)
# disambiguate author names and parse author address
dat2 <- authors_clean(references = dat1)

# after revieiwng disambiguation, merge any necessary corrections
dat3 <- authors_refine(dat2$review, dat2$prelim)

# georeference the author locations
dat4 <- authors_georef(dat3)

# generate a map of coauthorships; this is only one of the five possible visualizations  
plot_net_address(dat4$addresses)
```

## Registering with Google for an API key

1.  Install and load the `ggmap` package

```r

install.packages("ggmap")
library(ggmap)

```

1.  Register for a Google [Geocoding API](https://developers.google.com/maps/documentation/geocoding/overview) by following the instructions on the `READ ME` of the [`ggmap`](https://github.com/dkahle/ggmap) repository.

2.  Once you have your API key, add it to your `~/.Renviron` with the following:

```r 
`ggmap::register_google(key = "[your key]", write = TRUE)`
```

3. You should now be able to use `authors_georef()` as described in the vignette. Note that `refsplitr` currently has a limit of 2500 API calls per day. We are working on including the ability for users to select their own limits.

***Remember***: Your API key is unique and for you alone. Don't share it with other users or record it in a script file that is saved in a public repository. If need be you can visit the same website where you initially registered and generate a new key.


## Improvements & Suggestions

We welcome any suggestions for package improvement or ideas for features to include in future versions. If you have Issues, Feature Requests and Pull Requests, [here is how to contribute](https://github.com/ropensci/refsplitr/blob/master/CONTRIBUTING.md). We expect everyone contributing to the package to abide by our [Code of Conduct](https://github.com/ropensci/refsplitr/blob/master/CODE_OF_CONDUCT.md). 

<center>
<img src="man/figures/coauthor_connections_BITR.png" height="400">
</center>
<center>
Map of georeferenced article coauthorships generated with refsplitr.
</center>


## Contributors
* [Auriel Fournier](https://github.com/aurielfournier), Porzana Solutions
* [Matt Boone](https://github.com/birderboone), Porzana Solutions
* [Forrest Stevens](http://forreststevens.com/teaching/research.html), University of Louisville
* [Emilio M. Bruna](https://github.com/embruna), University of Florida

## Citation

Auriel M.V. Fournier, Matthew E. Boone, Forrest R. Stevens, and 
    Emilio M. Bruna Developer (2020). refsplitr: author name disambiguation, author 
    georeferencing, and mapping of coauthorship networks with Web of Science 
    data. R package version 1.0.0. https://github.com/ropensci/refsplitr
  
    @Manual{,
    title = {refsplitr: author name disambiguation, author georeferencing, 
    and mapping of coauthorship networks with Web of Science data.},
    author = {Fournier, Auriel M.V., Matthew E. Boone, Forrest R. Stevens, and Emilio M. Bruna},
    year = {2020},
    note = {R package version 1.0.0.},
    url={https://github.com/ropensci/refsplitr},
    }
    
_Note regarding package development history_: The early development of `refsplitr`- initially known as `refnet` - was by Forrest Stevens and Emilio M. Bruna and was done on [r-forge](https://r-forge.r-project.org/projects/refnet/). In December 2017 Bruna moved it to Github and hired [Porzana Solutions](https://github.com/aurielfournier) to finalize the package and prepare it for submission to CRAN.  _Please make all suggestions for changes via this Github repository - do not make a repo mirror of the R-forge version._

[![ropensci_footer](https://ropensci.org/public_images/ropensci_footer.png)](https://ropensci.org)
