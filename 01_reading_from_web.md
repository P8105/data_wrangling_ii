Reading data from the web
================

``` r
library(rvest)
```

    ## 
    ## Attaching package: 'rvest'

    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

``` r
library(httr)
```

## Import NSDUH

``` r
url = "https://www.samhsa.gov/data/sites/default/files/NSDUHsaeShortTermCHG2015/NSDUHsaeShortTermCHG2015.htm"

drug_use_html = 
  read_html(url)
```

Pull the tables out

``` r
marj_df = 
  drug_use_html |> 
  html_table() |> 
  first() |> 
  slice(-1)
```

## import some SW data

pull in the html

``` r
swm_html = 
  read_html_live("https://www.imdb.com/list/ls070150896/")
```

Next up … selector gadget

``` r
swm_title_vec = 
  swm_html |> 
  html_elements(".ipc-title-link-wrapper .ipc-title__text") |> 
  html_text()

swm_metascore_vec = 
  swm_html |> 
  html_elements(".metacritic-score-box") |> 
  html_text()

swm_runtime_vec = 
  swm_html |> 
  html_elements(".base .ipc-inline-list__item:nth-child(2)") |> 
  html_text()

swm_df = 
  tibble(
    title = swm_title_vec,
    score = swm_metascore_vec,
    runtime = swm_runtime_vec
  )
```
