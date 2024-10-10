Reading Data From the Web
================

Load the necessary libraries.

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

``` r
url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

drug_use_html = read_html(url)
```

Get the pieces I actually need.

``` r
marj_use_df = 
  drug_use_html |> 
  html_table() |> 
  first() |> 
  slice(-1)
```

Read in cost of living data.

``` r
nyc_cost_df = 
  read_html("https://www.bestplaces.net/cost_of_living/city/new_york/new_york") |> 
  html_table(header = TRUE) |> 
  first()
```

## CSS Selectors!!

``` r
swm_url = "https://www.imdb.com/list/ls070150896/"

swm_html = read_html(swm_url)
```

``` r
swm_title_vec = 
  swm_html |> 
  html_elements(".ipc-title-link-wrapper .ipc-title__text") |> 
  html_text()

swm_runtime_vec = 
  swm_html |> 
  html_elements(".dli-title-metadata-item:nth-child(2)") |> 
  html_text()

swm_score_vec = 
  swm_html |> 
  html_elements(".metacritic-score-box") |> 
  html_text()

swm_df = 
  tibble(
    title = swm_title_vec,
    runtime = swm_runtime_vec,
    score = swm_score_vec,
  )
```

Let’s import some books.

``` r
books_html = read_html("http://books.toscrape.com")

books_html |> 
  html_elements(".price_color") |> 
  html_text()
```

    ##  [1] "£51.77" "£53.74" "£50.10" "£47.82" "£54.23" "£22.65" "£33.34" "£17.93"
    ##  [9] "£22.60" "£52.15" "£13.99" "£20.66" "£17.46" "£52.29" "£35.02" "£57.25"
    ## [17] "£23.88" "£37.59" "£51.33" "£45.17"

## Use API

Get water data.

``` r
nyc_water = 
  GET("https://data.cityofnewyork.us/resource/ia2d-e54m.csv") |> 
  content()
```

    ## Rows: 45 Columns: 4
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## dbl (4): year, new_york_city_population, nyc_consumption_million_gallons_per...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

Get BRFSS data

``` r
brfss_df = 
  GET("https://chronicdata.cdc.gov/resource/acme-vg9e.csv",
      query = list("$limit" = 5000)) |> 
  content()
```

    ## Rows: 5000 Columns: 23
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (16): locationabbr, locationdesc, class, topic, question, response, data...
    ## dbl  (6): year, sample_size, data_value, confidence_limit_low, confidence_li...
    ## lgl  (1): locationid
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

Pokemon API

``` r
pokemon = 
  GET("https://pokeapi.co/api/v2/pokemon/ditto") |> 
  content()

pokemon
```

    ## $abilities
    ## $abilities[[1]]
    ## $abilities[[1]]$ability
    ## $abilities[[1]]$ability$name
    ## [1] "limber"
    ## 
    ## $abilities[[1]]$ability$url
    ## [1] "https://pokeapi.co/api/v2/ability/7/"
    ## 
    ## 
    ## $abilities[[1]]$is_hidden
    ## [1] FALSE
    ## 
    ## $abilities[[1]]$slot
    ## [1] 1
    ## 
    ## 
    ## $abilities[[2]]
    ## $abilities[[2]]$ability
    ## $abilities[[2]]$ability$name
    ## [1] "imposter"
    ## 
    ## $abilities[[2]]$ability$url
    ## [1] "https://pokeapi.co/api/v2/ability/150/"
    ## 
    ## 
    ## $abilities[[2]]$is_hidden
    ## [1] TRUE
    ## 
    ## $abilities[[2]]$slot
    ## [1] 3
    ## 
    ## 
    ## 
    ## $base_experience
    ## [1] 101
    ## 
    ## $cries
    ## $cries$latest
    ## [1] "https://raw.githubusercontent.com/PokeAPI/cries/main/cries/pokemon/latest/132.ogg"
    ## 
    ## $cries$legacy
    ## [1] "https://raw.githubusercontent.com/PokeAPI/cries/main/cries/pokemon/legacy/132.ogg"
    ## 
    ## 
    ## $forms
    ## $forms[[1]]
    ## $forms[[1]]$name
    ## [1] "ditto"
    ## 
    ## $forms[[1]]$url
    ## [1] "https://pokeapi.co/api/v2/pokemon-form/132/"
    ## 
    ## 
    ## 
    ## $game_indices
    ## $game_indices[[1]]
    ## $game_indices[[1]]$game_index
    ## [1] 76
    ## 
    ## $game_indices[[1]]$version
    ## $game_indices[[1]]$version$name
    ## [1] "red"
    ## 
    ## $game_indices[[1]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/1/"
    ## 
    ## 
    ## 
    ## $game_indices[[2]]
    ## $game_indices[[2]]$game_index
    ## [1] 76
    ## 
    ## $game_indices[[2]]$version
    ## $game_indices[[2]]$version$name
    ## [1] "blue"
    ## 
    ## $game_indices[[2]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/2/"
    ## 
    ## 
    ## 
    ## $game_indices[[3]]
    ## $game_indices[[3]]$game_index
    ## [1] 76
    ## 
    ## $game_indices[[3]]$version
    ## $game_indices[[3]]$version$name
    ## [1] "yellow"
    ## 
    ## $game_indices[[3]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/3/"
    ## 
    ## 
    ## 
    ## $game_indices[[4]]
    ## $game_indices[[4]]$game_index
    ## [1] 132
    ## 
    ## $game_indices[[4]]$version
    ## $game_indices[[4]]$version$name
    ## [1] "gold"
    ## 
    ## $game_indices[[4]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/4/"
    ## 
    ## 
    ## 
    ## $game_indices[[5]]
    ## $game_indices[[5]]$game_index
    ## [1] 132
    ## 
    ## $game_indices[[5]]$version
    ## $game_indices[[5]]$version$name
    ## [1] "silver"
    ## 
    ## $game_indices[[5]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/5/"
    ## 
    ## 
    ## 
    ## $game_indices[[6]]
    ## $game_indices[[6]]$game_index
    ## [1] 132
    ## 
    ## $game_indices[[6]]$version
    ## $game_indices[[6]]$version$name
    ## [1] "crystal"
    ## 
    ## $game_indices[[6]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/6/"
    ## 
    ## 
    ## 
    ## $game_indices[[7]]
    ## $game_indices[[7]]$game_index
    ## [1] 132
    ## 
    ## $game_indices[[7]]$version
    ## $game_indices[[7]]$version$name
    ## [1] "ruby"
    ## 
    ## $game_indices[[7]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/7/"
    ## 
    ## 
    ## 
    ## $game_indices[[8]]
    ## $game_indices[[8]]$game_index
    ## [1] 132
    ## 
    ## $game_indices[[8]]$version
    ## $game_indices[[8]]$version$name
    ## [1] "sapphire"
    ## 
    ## $game_indices[[8]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/8/"
    ## 
    ## 
    ## 
    ## $game_indices[[9]]
    ## $game_indices[[9]]$game_index
    ## [1] 132
    ## 
    ## $game_indices[[9]]$version
    ## $game_indices[[9]]$version$name
    ## [1] "emerald"
    ## 
    ## $game_indices[[9]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/9/"
    ## 
    ## 
    ## 
    ## $game_indices[[10]]
    ## $game_indices[[10]]$game_index
    ## [1] 132
    ## 
    ## $game_indices[[10]]$version
    ## $game_indices[[10]]$version$name
    ## [1] "firered"
    ## 
    ## $game_indices[[10]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/10/"
    ## 
    ## 
    ## 
    ## $game_indices[[11]]
    ## $game_indices[[11]]$game_index
    ## [1] 132
    ## 
    ## $game_indices[[11]]$version
    ## $game_indices[[11]]$version$name
    ## [1] "leafgreen"
    ## 
    ## $game_indices[[11]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/11/"
    ## 
    ## 
    ## 
    ## $game_indices[[12]]
    ## $game_indices[[12]]$game_index
    ## [1] 132
    ## 
    ## $game_indices[[12]]$version
    ## $game_indices[[12]]$version$name
    ## [1] "diamond"
    ## 
    ## $game_indices[[12]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/12/"
    ## 
    ## 
    ## 
    ## $game_indices[[13]]
    ## $game_indices[[13]]$game_index
    ## [1] 132
    ## 
    ## $game_indices[[13]]$version
    ## $game_indices[[13]]$version$name
    ## [1] "pearl"
    ## 
    ## $game_indices[[13]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/13/"
    ## 
    ## 
    ## 
    ## $game_indices[[14]]
    ## $game_indices[[14]]$game_index
    ## [1] 132
    ## 
    ## $game_indices[[14]]$version
    ## $game_indices[[14]]$version$name
    ## [1] "platinum"
    ## 
    ## $game_indices[[14]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/14/"
    ## 
    ## 
    ## 
    ## $game_indices[[15]]
    ## $game_indices[[15]]$game_index
    ## [1] 132
    ## 
    ## $game_indices[[15]]$version
    ## $game_indices[[15]]$version$name
    ## [1] "heartgold"
    ## 
    ## $game_indices[[15]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/15/"
    ## 
    ## 
    ## 
    ## $game_indices[[16]]
    ## $game_indices[[16]]$game_index
    ## [1] 132
    ## 
    ## $game_indices[[16]]$version
    ## $game_indices[[16]]$version$name
    ## [1] "soulsilver"
    ## 
    ## $game_indices[[16]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/16/"
    ## 
    ## 
    ## 
    ## $game_indices[[17]]
    ## $game_indices[[17]]$game_index
    ## [1] 132
    ## 
    ## $game_indices[[17]]$version
    ## $game_indices[[17]]$version$name
    ## [1] "black"
    ## 
    ## $game_indices[[17]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/17/"
    ## 
    ## 
    ## 
    ## $game_indices[[18]]
    ## $game_indices[[18]]$game_index
    ## [1] 132
    ## 
    ## $game_indices[[18]]$version
    ## $game_indices[[18]]$version$name
    ## [1] "white"
    ## 
    ## $game_indices[[18]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/18/"
    ## 
    ## 
    ## 
    ## $game_indices[[19]]
    ## $game_indices[[19]]$game_index
    ## [1] 132
    ## 
    ## $game_indices[[19]]$version
    ## $game_indices[[19]]$version$name
    ## [1] "black-2"
    ## 
    ## $game_indices[[19]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/21/"
    ## 
    ## 
    ## 
    ## $game_indices[[20]]
    ## $game_indices[[20]]$game_index
    ## [1] 132
    ## 
    ## $game_indices[[20]]$version
    ## $game_indices[[20]]$version$name
    ## [1] "white-2"
    ## 
    ## $game_indices[[20]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/22/"
    ## 
    ## 
    ## 
    ## 
    ## $height
    ## [1] 3
    ## 
    ## $held_items
    ## $held_items[[1]]
    ## $held_items[[1]]$item
    ## $held_items[[1]]$item$name
    ## [1] "metal-powder"
    ## 
    ## $held_items[[1]]$item$url
    ## [1] "https://pokeapi.co/api/v2/item/234/"
    ## 
    ## 
    ## $held_items[[1]]$version_details
    ## $held_items[[1]]$version_details[[1]]
    ## $held_items[[1]]$version_details[[1]]$rarity
    ## [1] 5
    ## 
    ## $held_items[[1]]$version_details[[1]]$version
    ## $held_items[[1]]$version_details[[1]]$version$name
    ## [1] "ruby"
    ## 
    ## $held_items[[1]]$version_details[[1]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/7/"
    ## 
    ## 
    ## 
    ## $held_items[[1]]$version_details[[2]]
    ## $held_items[[1]]$version_details[[2]]$rarity
    ## [1] 5
    ## 
    ## $held_items[[1]]$version_details[[2]]$version
    ## $held_items[[1]]$version_details[[2]]$version$name
    ## [1] "sapphire"
    ## 
    ## $held_items[[1]]$version_details[[2]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/8/"
    ## 
    ## 
    ## 
    ## $held_items[[1]]$version_details[[3]]
    ## $held_items[[1]]$version_details[[3]]$rarity
    ## [1] 5
    ## 
    ## $held_items[[1]]$version_details[[3]]$version
    ## $held_items[[1]]$version_details[[3]]$version$name
    ## [1] "emerald"
    ## 
    ## $held_items[[1]]$version_details[[3]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/9/"
    ## 
    ## 
    ## 
    ## $held_items[[1]]$version_details[[4]]
    ## $held_items[[1]]$version_details[[4]]$rarity
    ## [1] 5
    ## 
    ## $held_items[[1]]$version_details[[4]]$version
    ## $held_items[[1]]$version_details[[4]]$version$name
    ## [1] "firered"
    ## 
    ## $held_items[[1]]$version_details[[4]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/10/"
    ## 
    ## 
    ## 
    ## $held_items[[1]]$version_details[[5]]
    ## $held_items[[1]]$version_details[[5]]$rarity
    ## [1] 5
    ## 
    ## $held_items[[1]]$version_details[[5]]$version
    ## $held_items[[1]]$version_details[[5]]$version$name
    ## [1] "leafgreen"
    ## 
    ## $held_items[[1]]$version_details[[5]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/11/"
    ## 
    ## 
    ## 
    ## $held_items[[1]]$version_details[[6]]
    ## $held_items[[1]]$version_details[[6]]$rarity
    ## [1] 5
    ## 
    ## $held_items[[1]]$version_details[[6]]$version
    ## $held_items[[1]]$version_details[[6]]$version$name
    ## [1] "diamond"
    ## 
    ## $held_items[[1]]$version_details[[6]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/12/"
    ## 
    ## 
    ## 
    ## $held_items[[1]]$version_details[[7]]
    ## $held_items[[1]]$version_details[[7]]$rarity
    ## [1] 5
    ## 
    ## $held_items[[1]]$version_details[[7]]$version
    ## $held_items[[1]]$version_details[[7]]$version$name
    ## [1] "pearl"
    ## 
    ## $held_items[[1]]$version_details[[7]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/13/"
    ## 
    ## 
    ## 
    ## $held_items[[1]]$version_details[[8]]
    ## $held_items[[1]]$version_details[[8]]$rarity
    ## [1] 5
    ## 
    ## $held_items[[1]]$version_details[[8]]$version
    ## $held_items[[1]]$version_details[[8]]$version$name
    ## [1] "platinum"
    ## 
    ## $held_items[[1]]$version_details[[8]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/14/"
    ## 
    ## 
    ## 
    ## $held_items[[1]]$version_details[[9]]
    ## $held_items[[1]]$version_details[[9]]$rarity
    ## [1] 5
    ## 
    ## $held_items[[1]]$version_details[[9]]$version
    ## $held_items[[1]]$version_details[[9]]$version$name
    ## [1] "heartgold"
    ## 
    ## $held_items[[1]]$version_details[[9]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/15/"
    ## 
    ## 
    ## 
    ## $held_items[[1]]$version_details[[10]]
    ## $held_items[[1]]$version_details[[10]]$rarity
    ## [1] 5
    ## 
    ## $held_items[[1]]$version_details[[10]]$version
    ## $held_items[[1]]$version_details[[10]]$version$name
    ## [1] "soulsilver"
    ## 
    ## $held_items[[1]]$version_details[[10]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/16/"
    ## 
    ## 
    ## 
    ## $held_items[[1]]$version_details[[11]]
    ## $held_items[[1]]$version_details[[11]]$rarity
    ## [1] 5
    ## 
    ## $held_items[[1]]$version_details[[11]]$version
    ## $held_items[[1]]$version_details[[11]]$version$name
    ## [1] "black"
    ## 
    ## $held_items[[1]]$version_details[[11]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/17/"
    ## 
    ## 
    ## 
    ## $held_items[[1]]$version_details[[12]]
    ## $held_items[[1]]$version_details[[12]]$rarity
    ## [1] 5
    ## 
    ## $held_items[[1]]$version_details[[12]]$version
    ## $held_items[[1]]$version_details[[12]]$version$name
    ## [1] "white"
    ## 
    ## $held_items[[1]]$version_details[[12]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/18/"
    ## 
    ## 
    ## 
    ## $held_items[[1]]$version_details[[13]]
    ## $held_items[[1]]$version_details[[13]]$rarity
    ## [1] 5
    ## 
    ## $held_items[[1]]$version_details[[13]]$version
    ## $held_items[[1]]$version_details[[13]]$version$name
    ## [1] "black-2"
    ## 
    ## $held_items[[1]]$version_details[[13]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/21/"
    ## 
    ## 
    ## 
    ## $held_items[[1]]$version_details[[14]]
    ## $held_items[[1]]$version_details[[14]]$rarity
    ## [1] 5
    ## 
    ## $held_items[[1]]$version_details[[14]]$version
    ## $held_items[[1]]$version_details[[14]]$version$name
    ## [1] "white-2"
    ## 
    ## $held_items[[1]]$version_details[[14]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/22/"
    ## 
    ## 
    ## 
    ## $held_items[[1]]$version_details[[15]]
    ## $held_items[[1]]$version_details[[15]]$rarity
    ## [1] 5
    ## 
    ## $held_items[[1]]$version_details[[15]]$version
    ## $held_items[[1]]$version_details[[15]]$version$name
    ## [1] "x"
    ## 
    ## $held_items[[1]]$version_details[[15]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/23/"
    ## 
    ## 
    ## 
    ## $held_items[[1]]$version_details[[16]]
    ## $held_items[[1]]$version_details[[16]]$rarity
    ## [1] 5
    ## 
    ## $held_items[[1]]$version_details[[16]]$version
    ## $held_items[[1]]$version_details[[16]]$version$name
    ## [1] "y"
    ## 
    ## $held_items[[1]]$version_details[[16]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/24/"
    ## 
    ## 
    ## 
    ## $held_items[[1]]$version_details[[17]]
    ## $held_items[[1]]$version_details[[17]]$rarity
    ## [1] 5
    ## 
    ## $held_items[[1]]$version_details[[17]]$version
    ## $held_items[[1]]$version_details[[17]]$version$name
    ## [1] "omega-ruby"
    ## 
    ## $held_items[[1]]$version_details[[17]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/25/"
    ## 
    ## 
    ## 
    ## $held_items[[1]]$version_details[[18]]
    ## $held_items[[1]]$version_details[[18]]$rarity
    ## [1] 5
    ## 
    ## $held_items[[1]]$version_details[[18]]$version
    ## $held_items[[1]]$version_details[[18]]$version$name
    ## [1] "alpha-sapphire"
    ## 
    ## $held_items[[1]]$version_details[[18]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/26/"
    ## 
    ## 
    ## 
    ## $held_items[[1]]$version_details[[19]]
    ## $held_items[[1]]$version_details[[19]]$rarity
    ## [1] 5
    ## 
    ## $held_items[[1]]$version_details[[19]]$version
    ## $held_items[[1]]$version_details[[19]]$version$name
    ## [1] "sun"
    ## 
    ## $held_items[[1]]$version_details[[19]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/27/"
    ## 
    ## 
    ## 
    ## $held_items[[1]]$version_details[[20]]
    ## $held_items[[1]]$version_details[[20]]$rarity
    ## [1] 5
    ## 
    ## $held_items[[1]]$version_details[[20]]$version
    ## $held_items[[1]]$version_details[[20]]$version$name
    ## [1] "moon"
    ## 
    ## $held_items[[1]]$version_details[[20]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/28/"
    ## 
    ## 
    ## 
    ## $held_items[[1]]$version_details[[21]]
    ## $held_items[[1]]$version_details[[21]]$rarity
    ## [1] 5
    ## 
    ## $held_items[[1]]$version_details[[21]]$version
    ## $held_items[[1]]$version_details[[21]]$version$name
    ## [1] "ultra-sun"
    ## 
    ## $held_items[[1]]$version_details[[21]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/29/"
    ## 
    ## 
    ## 
    ## $held_items[[1]]$version_details[[22]]
    ## $held_items[[1]]$version_details[[22]]$rarity
    ## [1] 5
    ## 
    ## $held_items[[1]]$version_details[[22]]$version
    ## $held_items[[1]]$version_details[[22]]$version$name
    ## [1] "ultra-moon"
    ## 
    ## $held_items[[1]]$version_details[[22]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/30/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $held_items[[2]]
    ## $held_items[[2]]$item
    ## $held_items[[2]]$item$name
    ## [1] "quick-powder"
    ## 
    ## $held_items[[2]]$item$url
    ## [1] "https://pokeapi.co/api/v2/item/251/"
    ## 
    ## 
    ## $held_items[[2]]$version_details
    ## $held_items[[2]]$version_details[[1]]
    ## $held_items[[2]]$version_details[[1]]$rarity
    ## [1] 50
    ## 
    ## $held_items[[2]]$version_details[[1]]$version
    ## $held_items[[2]]$version_details[[1]]$version$name
    ## [1] "diamond"
    ## 
    ## $held_items[[2]]$version_details[[1]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/12/"
    ## 
    ## 
    ## 
    ## $held_items[[2]]$version_details[[2]]
    ## $held_items[[2]]$version_details[[2]]$rarity
    ## [1] 50
    ## 
    ## $held_items[[2]]$version_details[[2]]$version
    ## $held_items[[2]]$version_details[[2]]$version$name
    ## [1] "pearl"
    ## 
    ## $held_items[[2]]$version_details[[2]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/13/"
    ## 
    ## 
    ## 
    ## $held_items[[2]]$version_details[[3]]
    ## $held_items[[2]]$version_details[[3]]$rarity
    ## [1] 50
    ## 
    ## $held_items[[2]]$version_details[[3]]$version
    ## $held_items[[2]]$version_details[[3]]$version$name
    ## [1] "platinum"
    ## 
    ## $held_items[[2]]$version_details[[3]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/14/"
    ## 
    ## 
    ## 
    ## $held_items[[2]]$version_details[[4]]
    ## $held_items[[2]]$version_details[[4]]$rarity
    ## [1] 50
    ## 
    ## $held_items[[2]]$version_details[[4]]$version
    ## $held_items[[2]]$version_details[[4]]$version$name
    ## [1] "heartgold"
    ## 
    ## $held_items[[2]]$version_details[[4]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/15/"
    ## 
    ## 
    ## 
    ## $held_items[[2]]$version_details[[5]]
    ## $held_items[[2]]$version_details[[5]]$rarity
    ## [1] 50
    ## 
    ## $held_items[[2]]$version_details[[5]]$version
    ## $held_items[[2]]$version_details[[5]]$version$name
    ## [1] "soulsilver"
    ## 
    ## $held_items[[2]]$version_details[[5]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/16/"
    ## 
    ## 
    ## 
    ## $held_items[[2]]$version_details[[6]]
    ## $held_items[[2]]$version_details[[6]]$rarity
    ## [1] 50
    ## 
    ## $held_items[[2]]$version_details[[6]]$version
    ## $held_items[[2]]$version_details[[6]]$version$name
    ## [1] "black"
    ## 
    ## $held_items[[2]]$version_details[[6]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/17/"
    ## 
    ## 
    ## 
    ## $held_items[[2]]$version_details[[7]]
    ## $held_items[[2]]$version_details[[7]]$rarity
    ## [1] 50
    ## 
    ## $held_items[[2]]$version_details[[7]]$version
    ## $held_items[[2]]$version_details[[7]]$version$name
    ## [1] "white"
    ## 
    ## $held_items[[2]]$version_details[[7]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/18/"
    ## 
    ## 
    ## 
    ## $held_items[[2]]$version_details[[8]]
    ## $held_items[[2]]$version_details[[8]]$rarity
    ## [1] 50
    ## 
    ## $held_items[[2]]$version_details[[8]]$version
    ## $held_items[[2]]$version_details[[8]]$version$name
    ## [1] "black-2"
    ## 
    ## $held_items[[2]]$version_details[[8]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/21/"
    ## 
    ## 
    ## 
    ## $held_items[[2]]$version_details[[9]]
    ## $held_items[[2]]$version_details[[9]]$rarity
    ## [1] 50
    ## 
    ## $held_items[[2]]$version_details[[9]]$version
    ## $held_items[[2]]$version_details[[9]]$version$name
    ## [1] "white-2"
    ## 
    ## $held_items[[2]]$version_details[[9]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/22/"
    ## 
    ## 
    ## 
    ## $held_items[[2]]$version_details[[10]]
    ## $held_items[[2]]$version_details[[10]]$rarity
    ## [1] 50
    ## 
    ## $held_items[[2]]$version_details[[10]]$version
    ## $held_items[[2]]$version_details[[10]]$version$name
    ## [1] "x"
    ## 
    ## $held_items[[2]]$version_details[[10]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/23/"
    ## 
    ## 
    ## 
    ## $held_items[[2]]$version_details[[11]]
    ## $held_items[[2]]$version_details[[11]]$rarity
    ## [1] 50
    ## 
    ## $held_items[[2]]$version_details[[11]]$version
    ## $held_items[[2]]$version_details[[11]]$version$name
    ## [1] "y"
    ## 
    ## $held_items[[2]]$version_details[[11]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/24/"
    ## 
    ## 
    ## 
    ## $held_items[[2]]$version_details[[12]]
    ## $held_items[[2]]$version_details[[12]]$rarity
    ## [1] 50
    ## 
    ## $held_items[[2]]$version_details[[12]]$version
    ## $held_items[[2]]$version_details[[12]]$version$name
    ## [1] "omega-ruby"
    ## 
    ## $held_items[[2]]$version_details[[12]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/25/"
    ## 
    ## 
    ## 
    ## $held_items[[2]]$version_details[[13]]
    ## $held_items[[2]]$version_details[[13]]$rarity
    ## [1] 50
    ## 
    ## $held_items[[2]]$version_details[[13]]$version
    ## $held_items[[2]]$version_details[[13]]$version$name
    ## [1] "alpha-sapphire"
    ## 
    ## $held_items[[2]]$version_details[[13]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/26/"
    ## 
    ## 
    ## 
    ## $held_items[[2]]$version_details[[14]]
    ## $held_items[[2]]$version_details[[14]]$rarity
    ## [1] 50
    ## 
    ## $held_items[[2]]$version_details[[14]]$version
    ## $held_items[[2]]$version_details[[14]]$version$name
    ## [1] "sun"
    ## 
    ## $held_items[[2]]$version_details[[14]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/27/"
    ## 
    ## 
    ## 
    ## $held_items[[2]]$version_details[[15]]
    ## $held_items[[2]]$version_details[[15]]$rarity
    ## [1] 50
    ## 
    ## $held_items[[2]]$version_details[[15]]$version
    ## $held_items[[2]]$version_details[[15]]$version$name
    ## [1] "moon"
    ## 
    ## $held_items[[2]]$version_details[[15]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/28/"
    ## 
    ## 
    ## 
    ## $held_items[[2]]$version_details[[16]]
    ## $held_items[[2]]$version_details[[16]]$rarity
    ## [1] 50
    ## 
    ## $held_items[[2]]$version_details[[16]]$version
    ## $held_items[[2]]$version_details[[16]]$version$name
    ## [1] "ultra-sun"
    ## 
    ## $held_items[[2]]$version_details[[16]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/29/"
    ## 
    ## 
    ## 
    ## $held_items[[2]]$version_details[[17]]
    ## $held_items[[2]]$version_details[[17]]$rarity
    ## [1] 50
    ## 
    ## $held_items[[2]]$version_details[[17]]$version
    ## $held_items[[2]]$version_details[[17]]$version$name
    ## [1] "ultra-moon"
    ## 
    ## $held_items[[2]]$version_details[[17]]$version$url
    ## [1] "https://pokeapi.co/api/v2/version/30/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $id
    ## [1] 132
    ## 
    ## $is_default
    ## [1] TRUE
    ## 
    ## $location_area_encounters
    ## [1] "https://pokeapi.co/api/v2/pokemon/132/encounters"
    ## 
    ## $moves
    ## $moves[[1]]
    ## $moves[[1]]$move
    ## $moves[[1]]$move$name
    ## [1] "transform"
    ## 
    ## $moves[[1]]$move$url
    ## [1] "https://pokeapi.co/api/v2/move/144/"
    ## 
    ## 
    ## $moves[[1]]$version_group_details
    ## $moves[[1]]$version_group_details[[1]]
    ## $moves[[1]]$version_group_details[[1]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[1]]$version_group_details[[1]]$move_learn_method
    ## $moves[[1]]$version_group_details[[1]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[1]]$version_group_details[[1]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[1]]$version_group
    ## $moves[[1]]$version_group_details[[1]]$version_group$name
    ## [1] "red-blue"
    ## 
    ## $moves[[1]]$version_group_details[[1]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/1/"
    ## 
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[2]]
    ## $moves[[1]]$version_group_details[[2]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[1]]$version_group_details[[2]]$move_learn_method
    ## $moves[[1]]$version_group_details[[2]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[1]]$version_group_details[[2]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[2]]$version_group
    ## $moves[[1]]$version_group_details[[2]]$version_group$name
    ## [1] "yellow"
    ## 
    ## $moves[[1]]$version_group_details[[2]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/2/"
    ## 
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[3]]
    ## $moves[[1]]$version_group_details[[3]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[1]]$version_group_details[[3]]$move_learn_method
    ## $moves[[1]]$version_group_details[[3]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[1]]$version_group_details[[3]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[3]]$version_group
    ## $moves[[1]]$version_group_details[[3]]$version_group$name
    ## [1] "gold-silver"
    ## 
    ## $moves[[1]]$version_group_details[[3]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/3/"
    ## 
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[4]]
    ## $moves[[1]]$version_group_details[[4]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[1]]$version_group_details[[4]]$move_learn_method
    ## $moves[[1]]$version_group_details[[4]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[1]]$version_group_details[[4]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[4]]$version_group
    ## $moves[[1]]$version_group_details[[4]]$version_group$name
    ## [1] "crystal"
    ## 
    ## $moves[[1]]$version_group_details[[4]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/4/"
    ## 
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[5]]
    ## $moves[[1]]$version_group_details[[5]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[1]]$version_group_details[[5]]$move_learn_method
    ## $moves[[1]]$version_group_details[[5]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[1]]$version_group_details[[5]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[5]]$version_group
    ## $moves[[1]]$version_group_details[[5]]$version_group$name
    ## [1] "ruby-sapphire"
    ## 
    ## $moves[[1]]$version_group_details[[5]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/5/"
    ## 
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[6]]
    ## $moves[[1]]$version_group_details[[6]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[1]]$version_group_details[[6]]$move_learn_method
    ## $moves[[1]]$version_group_details[[6]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[1]]$version_group_details[[6]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[6]]$version_group
    ## $moves[[1]]$version_group_details[[6]]$version_group$name
    ## [1] "emerald"
    ## 
    ## $moves[[1]]$version_group_details[[6]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/6/"
    ## 
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[7]]
    ## $moves[[1]]$version_group_details[[7]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[1]]$version_group_details[[7]]$move_learn_method
    ## $moves[[1]]$version_group_details[[7]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[1]]$version_group_details[[7]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[7]]$version_group
    ## $moves[[1]]$version_group_details[[7]]$version_group$name
    ## [1] "firered-leafgreen"
    ## 
    ## $moves[[1]]$version_group_details[[7]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/7/"
    ## 
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[8]]
    ## $moves[[1]]$version_group_details[[8]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[1]]$version_group_details[[8]]$move_learn_method
    ## $moves[[1]]$version_group_details[[8]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[1]]$version_group_details[[8]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[8]]$version_group
    ## $moves[[1]]$version_group_details[[8]]$version_group$name
    ## [1] "diamond-pearl"
    ## 
    ## $moves[[1]]$version_group_details[[8]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/8/"
    ## 
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[9]]
    ## $moves[[1]]$version_group_details[[9]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[1]]$version_group_details[[9]]$move_learn_method
    ## $moves[[1]]$version_group_details[[9]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[1]]$version_group_details[[9]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[9]]$version_group
    ## $moves[[1]]$version_group_details[[9]]$version_group$name
    ## [1] "platinum"
    ## 
    ## $moves[[1]]$version_group_details[[9]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/9/"
    ## 
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[10]]
    ## $moves[[1]]$version_group_details[[10]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[1]]$version_group_details[[10]]$move_learn_method
    ## $moves[[1]]$version_group_details[[10]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[1]]$version_group_details[[10]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[10]]$version_group
    ## $moves[[1]]$version_group_details[[10]]$version_group$name
    ## [1] "heartgold-soulsilver"
    ## 
    ## $moves[[1]]$version_group_details[[10]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/10/"
    ## 
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[11]]
    ## $moves[[1]]$version_group_details[[11]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[1]]$version_group_details[[11]]$move_learn_method
    ## $moves[[1]]$version_group_details[[11]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[1]]$version_group_details[[11]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[11]]$version_group
    ## $moves[[1]]$version_group_details[[11]]$version_group$name
    ## [1] "black-white"
    ## 
    ## $moves[[1]]$version_group_details[[11]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/11/"
    ## 
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[12]]
    ## $moves[[1]]$version_group_details[[12]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[1]]$version_group_details[[12]]$move_learn_method
    ## $moves[[1]]$version_group_details[[12]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[1]]$version_group_details[[12]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[12]]$version_group
    ## $moves[[1]]$version_group_details[[12]]$version_group$name
    ## [1] "colosseum"
    ## 
    ## $moves[[1]]$version_group_details[[12]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/12/"
    ## 
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[13]]
    ## $moves[[1]]$version_group_details[[13]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[1]]$version_group_details[[13]]$move_learn_method
    ## $moves[[1]]$version_group_details[[13]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[1]]$version_group_details[[13]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[13]]$version_group
    ## $moves[[1]]$version_group_details[[13]]$version_group$name
    ## [1] "xd"
    ## 
    ## $moves[[1]]$version_group_details[[13]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/13/"
    ## 
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[14]]
    ## $moves[[1]]$version_group_details[[14]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[1]]$version_group_details[[14]]$move_learn_method
    ## $moves[[1]]$version_group_details[[14]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[1]]$version_group_details[[14]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[14]]$version_group
    ## $moves[[1]]$version_group_details[[14]]$version_group$name
    ## [1] "black-2-white-2"
    ## 
    ## $moves[[1]]$version_group_details[[14]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/14/"
    ## 
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[15]]
    ## $moves[[1]]$version_group_details[[15]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[1]]$version_group_details[[15]]$move_learn_method
    ## $moves[[1]]$version_group_details[[15]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[1]]$version_group_details[[15]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[15]]$version_group
    ## $moves[[1]]$version_group_details[[15]]$version_group$name
    ## [1] "x-y"
    ## 
    ## $moves[[1]]$version_group_details[[15]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/15/"
    ## 
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[16]]
    ## $moves[[1]]$version_group_details[[16]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[1]]$version_group_details[[16]]$move_learn_method
    ## $moves[[1]]$version_group_details[[16]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[1]]$version_group_details[[16]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[16]]$version_group
    ## $moves[[1]]$version_group_details[[16]]$version_group$name
    ## [1] "omega-ruby-alpha-sapphire"
    ## 
    ## $moves[[1]]$version_group_details[[16]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/16/"
    ## 
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[17]]
    ## $moves[[1]]$version_group_details[[17]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[1]]$version_group_details[[17]]$move_learn_method
    ## $moves[[1]]$version_group_details[[17]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[1]]$version_group_details[[17]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[17]]$version_group
    ## $moves[[1]]$version_group_details[[17]]$version_group$name
    ## [1] "sun-moon"
    ## 
    ## $moves[[1]]$version_group_details[[17]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/17/"
    ## 
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[18]]
    ## $moves[[1]]$version_group_details[[18]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[1]]$version_group_details[[18]]$move_learn_method
    ## $moves[[1]]$version_group_details[[18]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[1]]$version_group_details[[18]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[18]]$version_group
    ## $moves[[1]]$version_group_details[[18]]$version_group$name
    ## [1] "ultra-sun-ultra-moon"
    ## 
    ## $moves[[1]]$version_group_details[[18]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/18/"
    ## 
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[19]]
    ## $moves[[1]]$version_group_details[[19]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[1]]$version_group_details[[19]]$move_learn_method
    ## $moves[[1]]$version_group_details[[19]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[1]]$version_group_details[[19]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[19]]$version_group
    ## $moves[[1]]$version_group_details[[19]]$version_group$name
    ## [1] "lets-go-pikachu-lets-go-eevee"
    ## 
    ## $moves[[1]]$version_group_details[[19]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/19/"
    ## 
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[20]]
    ## $moves[[1]]$version_group_details[[20]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[1]]$version_group_details[[20]]$move_learn_method
    ## $moves[[1]]$version_group_details[[20]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[1]]$version_group_details[[20]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[20]]$version_group
    ## $moves[[1]]$version_group_details[[20]]$version_group$name
    ## [1] "sword-shield"
    ## 
    ## $moves[[1]]$version_group_details[[20]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/20/"
    ## 
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[21]]
    ## $moves[[1]]$version_group_details[[21]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[1]]$version_group_details[[21]]$move_learn_method
    ## $moves[[1]]$version_group_details[[21]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[1]]$version_group_details[[21]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[21]]$version_group
    ## $moves[[1]]$version_group_details[[21]]$version_group$name
    ## [1] "brilliant-diamond-and-shining-pearl"
    ## 
    ## $moves[[1]]$version_group_details[[21]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/23/"
    ## 
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[22]]
    ## $moves[[1]]$version_group_details[[22]]$level_learned_at
    ## [1] 1
    ## 
    ## $moves[[1]]$version_group_details[[22]]$move_learn_method
    ## $moves[[1]]$version_group_details[[22]]$move_learn_method$name
    ## [1] "level-up"
    ## 
    ## $moves[[1]]$version_group_details[[22]]$move_learn_method$url
    ## [1] "https://pokeapi.co/api/v2/move-learn-method/1/"
    ## 
    ## 
    ## $moves[[1]]$version_group_details[[22]]$version_group
    ## $moves[[1]]$version_group_details[[22]]$version_group$name
    ## [1] "scarlet-violet"
    ## 
    ## $moves[[1]]$version_group_details[[22]]$version_group$url
    ## [1] "https://pokeapi.co/api/v2/version-group/25/"
    ## 
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $name
    ## [1] "ditto"
    ## 
    ## $order
    ## [1] 214
    ## 
    ## $past_abilities
    ## list()
    ## 
    ## $past_types
    ## list()
    ## 
    ## $species
    ## $species$name
    ## [1] "ditto"
    ## 
    ## $species$url
    ## [1] "https://pokeapi.co/api/v2/pokemon-species/132/"
    ## 
    ## 
    ## $sprites
    ## $sprites$back_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/132.png"
    ## 
    ## $sprites$back_female
    ## NULL
    ## 
    ## $sprites$back_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/shiny/132.png"
    ## 
    ## $sprites$back_shiny_female
    ## NULL
    ## 
    ## $sprites$front_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/132.png"
    ## 
    ## $sprites$front_female
    ## NULL
    ## 
    ## $sprites$front_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/shiny/132.png"
    ## 
    ## $sprites$front_shiny_female
    ## NULL
    ## 
    ## $sprites$other
    ## $sprites$other$dream_world
    ## $sprites$other$dream_world$front_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/132.svg"
    ## 
    ## $sprites$other$dream_world$front_female
    ## NULL
    ## 
    ## 
    ## $sprites$other$home
    ## $sprites$other$home$front_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/home/132.png"
    ## 
    ## $sprites$other$home$front_female
    ## NULL
    ## 
    ## $sprites$other$home$front_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/home/shiny/132.png"
    ## 
    ## $sprites$other$home$front_shiny_female
    ## NULL
    ## 
    ## 
    ## $sprites$other$`official-artwork`
    ## $sprites$other$`official-artwork`$front_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/132.png"
    ## 
    ## $sprites$other$`official-artwork`$front_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/shiny/132.png"
    ## 
    ## 
    ## $sprites$other$showdown
    ## $sprites$other$showdown$back_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/showdown/back/132.gif"
    ## 
    ## $sprites$other$showdown$back_female
    ## NULL
    ## 
    ## $sprites$other$showdown$back_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/showdown/back/shiny/132.gif"
    ## 
    ## $sprites$other$showdown$back_shiny_female
    ## NULL
    ## 
    ## $sprites$other$showdown$front_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/showdown/132.gif"
    ## 
    ## $sprites$other$showdown$front_female
    ## NULL
    ## 
    ## $sprites$other$showdown$front_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/showdown/shiny/132.gif"
    ## 
    ## $sprites$other$showdown$front_shiny_female
    ## NULL
    ## 
    ## 
    ## 
    ## $sprites$versions
    ## $sprites$versions$`generation-i`
    ## $sprites$versions$`generation-i`$`red-blue`
    ## $sprites$versions$`generation-i`$`red-blue`$back_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-i/red-blue/back/132.png"
    ## 
    ## $sprites$versions$`generation-i`$`red-blue`$back_gray
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-i/red-blue/back/gray/132.png"
    ## 
    ## $sprites$versions$`generation-i`$`red-blue`$back_transparent
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-i/red-blue/transparent/back/132.png"
    ## 
    ## $sprites$versions$`generation-i`$`red-blue`$front_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-i/red-blue/132.png"
    ## 
    ## $sprites$versions$`generation-i`$`red-blue`$front_gray
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-i/red-blue/gray/132.png"
    ## 
    ## $sprites$versions$`generation-i`$`red-blue`$front_transparent
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-i/red-blue/transparent/132.png"
    ## 
    ## 
    ## $sprites$versions$`generation-i`$yellow
    ## $sprites$versions$`generation-i`$yellow$back_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-i/yellow/back/132.png"
    ## 
    ## $sprites$versions$`generation-i`$yellow$back_gray
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-i/yellow/back/gray/132.png"
    ## 
    ## $sprites$versions$`generation-i`$yellow$back_transparent
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-i/yellow/transparent/back/132.png"
    ## 
    ## $sprites$versions$`generation-i`$yellow$front_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-i/yellow/132.png"
    ## 
    ## $sprites$versions$`generation-i`$yellow$front_gray
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-i/yellow/gray/132.png"
    ## 
    ## $sprites$versions$`generation-i`$yellow$front_transparent
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-i/yellow/transparent/132.png"
    ## 
    ## 
    ## 
    ## $sprites$versions$`generation-ii`
    ## $sprites$versions$`generation-ii`$crystal
    ## $sprites$versions$`generation-ii`$crystal$back_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-ii/crystal/back/132.png"
    ## 
    ## $sprites$versions$`generation-ii`$crystal$back_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-ii/crystal/back/shiny/132.png"
    ## 
    ## $sprites$versions$`generation-ii`$crystal$back_shiny_transparent
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-ii/crystal/transparent/back/shiny/132.png"
    ## 
    ## $sprites$versions$`generation-ii`$crystal$back_transparent
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-ii/crystal/transparent/back/132.png"
    ## 
    ## $sprites$versions$`generation-ii`$crystal$front_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-ii/crystal/132.png"
    ## 
    ## $sprites$versions$`generation-ii`$crystal$front_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-ii/crystal/shiny/132.png"
    ## 
    ## $sprites$versions$`generation-ii`$crystal$front_shiny_transparent
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-ii/crystal/transparent/shiny/132.png"
    ## 
    ## $sprites$versions$`generation-ii`$crystal$front_transparent
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-ii/crystal/transparent/132.png"
    ## 
    ## 
    ## $sprites$versions$`generation-ii`$gold
    ## $sprites$versions$`generation-ii`$gold$back_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-ii/gold/back/132.png"
    ## 
    ## $sprites$versions$`generation-ii`$gold$back_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-ii/gold/back/shiny/132.png"
    ## 
    ## $sprites$versions$`generation-ii`$gold$front_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-ii/gold/132.png"
    ## 
    ## $sprites$versions$`generation-ii`$gold$front_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-ii/gold/shiny/132.png"
    ## 
    ## $sprites$versions$`generation-ii`$gold$front_transparent
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-ii/gold/transparent/132.png"
    ## 
    ## 
    ## $sprites$versions$`generation-ii`$silver
    ## $sprites$versions$`generation-ii`$silver$back_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-ii/silver/back/132.png"
    ## 
    ## $sprites$versions$`generation-ii`$silver$back_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-ii/silver/back/shiny/132.png"
    ## 
    ## $sprites$versions$`generation-ii`$silver$front_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-ii/silver/132.png"
    ## 
    ## $sprites$versions$`generation-ii`$silver$front_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-ii/silver/shiny/132.png"
    ## 
    ## $sprites$versions$`generation-ii`$silver$front_transparent
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-ii/silver/transparent/132.png"
    ## 
    ## 
    ## 
    ## $sprites$versions$`generation-iii`
    ## $sprites$versions$`generation-iii`$emerald
    ## $sprites$versions$`generation-iii`$emerald$front_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-iii/emerald/132.png"
    ## 
    ## $sprites$versions$`generation-iii`$emerald$front_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-iii/emerald/shiny/132.png"
    ## 
    ## 
    ## $sprites$versions$`generation-iii`$`firered-leafgreen`
    ## $sprites$versions$`generation-iii`$`firered-leafgreen`$back_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-iii/firered-leafgreen/back/132.png"
    ## 
    ## $sprites$versions$`generation-iii`$`firered-leafgreen`$back_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-iii/firered-leafgreen/back/shiny/132.png"
    ## 
    ## $sprites$versions$`generation-iii`$`firered-leafgreen`$front_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-iii/firered-leafgreen/132.png"
    ## 
    ## $sprites$versions$`generation-iii`$`firered-leafgreen`$front_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-iii/firered-leafgreen/shiny/132.png"
    ## 
    ## 
    ## $sprites$versions$`generation-iii`$`ruby-sapphire`
    ## $sprites$versions$`generation-iii`$`ruby-sapphire`$back_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-iii/ruby-sapphire/back/132.png"
    ## 
    ## $sprites$versions$`generation-iii`$`ruby-sapphire`$back_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-iii/ruby-sapphire/back/shiny/132.png"
    ## 
    ## $sprites$versions$`generation-iii`$`ruby-sapphire`$front_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-iii/ruby-sapphire/132.png"
    ## 
    ## $sprites$versions$`generation-iii`$`ruby-sapphire`$front_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-iii/ruby-sapphire/shiny/132.png"
    ## 
    ## 
    ## 
    ## $sprites$versions$`generation-iv`
    ## $sprites$versions$`generation-iv`$`diamond-pearl`
    ## $sprites$versions$`generation-iv`$`diamond-pearl`$back_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-iv/diamond-pearl/back/132.png"
    ## 
    ## $sprites$versions$`generation-iv`$`diamond-pearl`$back_female
    ## NULL
    ## 
    ## $sprites$versions$`generation-iv`$`diamond-pearl`$back_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-iv/diamond-pearl/back/shiny/132.png"
    ## 
    ## $sprites$versions$`generation-iv`$`diamond-pearl`$back_shiny_female
    ## NULL
    ## 
    ## $sprites$versions$`generation-iv`$`diamond-pearl`$front_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-iv/diamond-pearl/132.png"
    ## 
    ## $sprites$versions$`generation-iv`$`diamond-pearl`$front_female
    ## NULL
    ## 
    ## $sprites$versions$`generation-iv`$`diamond-pearl`$front_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-iv/diamond-pearl/shiny/132.png"
    ## 
    ## $sprites$versions$`generation-iv`$`diamond-pearl`$front_shiny_female
    ## NULL
    ## 
    ## 
    ## $sprites$versions$`generation-iv`$`heartgold-soulsilver`
    ## $sprites$versions$`generation-iv`$`heartgold-soulsilver`$back_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-iv/heartgold-soulsilver/back/132.png"
    ## 
    ## $sprites$versions$`generation-iv`$`heartgold-soulsilver`$back_female
    ## NULL
    ## 
    ## $sprites$versions$`generation-iv`$`heartgold-soulsilver`$back_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-iv/heartgold-soulsilver/back/shiny/132.png"
    ## 
    ## $sprites$versions$`generation-iv`$`heartgold-soulsilver`$back_shiny_female
    ## NULL
    ## 
    ## $sprites$versions$`generation-iv`$`heartgold-soulsilver`$front_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-iv/heartgold-soulsilver/132.png"
    ## 
    ## $sprites$versions$`generation-iv`$`heartgold-soulsilver`$front_female
    ## NULL
    ## 
    ## $sprites$versions$`generation-iv`$`heartgold-soulsilver`$front_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-iv/heartgold-soulsilver/shiny/132.png"
    ## 
    ## $sprites$versions$`generation-iv`$`heartgold-soulsilver`$front_shiny_female
    ## NULL
    ## 
    ## 
    ## $sprites$versions$`generation-iv`$platinum
    ## $sprites$versions$`generation-iv`$platinum$back_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-iv/platinum/back/132.png"
    ## 
    ## $sprites$versions$`generation-iv`$platinum$back_female
    ## NULL
    ## 
    ## $sprites$versions$`generation-iv`$platinum$back_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-iv/platinum/back/shiny/132.png"
    ## 
    ## $sprites$versions$`generation-iv`$platinum$back_shiny_female
    ## NULL
    ## 
    ## $sprites$versions$`generation-iv`$platinum$front_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-iv/platinum/132.png"
    ## 
    ## $sprites$versions$`generation-iv`$platinum$front_female
    ## NULL
    ## 
    ## $sprites$versions$`generation-iv`$platinum$front_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-iv/platinum/shiny/132.png"
    ## 
    ## $sprites$versions$`generation-iv`$platinum$front_shiny_female
    ## NULL
    ## 
    ## 
    ## 
    ## $sprites$versions$`generation-v`
    ## $sprites$versions$`generation-v`$`black-white`
    ## $sprites$versions$`generation-v`$`black-white`$animated
    ## $sprites$versions$`generation-v`$`black-white`$animated$back_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-v/black-white/animated/back/132.gif"
    ## 
    ## $sprites$versions$`generation-v`$`black-white`$animated$back_female
    ## NULL
    ## 
    ## $sprites$versions$`generation-v`$`black-white`$animated$back_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-v/black-white/animated/back/shiny/132.gif"
    ## 
    ## $sprites$versions$`generation-v`$`black-white`$animated$back_shiny_female
    ## NULL
    ## 
    ## $sprites$versions$`generation-v`$`black-white`$animated$front_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-v/black-white/animated/132.gif"
    ## 
    ## $sprites$versions$`generation-v`$`black-white`$animated$front_female
    ## NULL
    ## 
    ## $sprites$versions$`generation-v`$`black-white`$animated$front_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-v/black-white/animated/shiny/132.gif"
    ## 
    ## $sprites$versions$`generation-v`$`black-white`$animated$front_shiny_female
    ## NULL
    ## 
    ## 
    ## $sprites$versions$`generation-v`$`black-white`$back_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-v/black-white/back/132.png"
    ## 
    ## $sprites$versions$`generation-v`$`black-white`$back_female
    ## NULL
    ## 
    ## $sprites$versions$`generation-v`$`black-white`$back_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-v/black-white/back/shiny/132.png"
    ## 
    ## $sprites$versions$`generation-v`$`black-white`$back_shiny_female
    ## NULL
    ## 
    ## $sprites$versions$`generation-v`$`black-white`$front_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-v/black-white/132.png"
    ## 
    ## $sprites$versions$`generation-v`$`black-white`$front_female
    ## NULL
    ## 
    ## $sprites$versions$`generation-v`$`black-white`$front_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-v/black-white/shiny/132.png"
    ## 
    ## $sprites$versions$`generation-v`$`black-white`$front_shiny_female
    ## NULL
    ## 
    ## 
    ## 
    ## $sprites$versions$`generation-vi`
    ## $sprites$versions$`generation-vi`$`omegaruby-alphasapphire`
    ## $sprites$versions$`generation-vi`$`omegaruby-alphasapphire`$front_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-vi/omegaruby-alphasapphire/132.png"
    ## 
    ## $sprites$versions$`generation-vi`$`omegaruby-alphasapphire`$front_female
    ## NULL
    ## 
    ## $sprites$versions$`generation-vi`$`omegaruby-alphasapphire`$front_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-vi/omegaruby-alphasapphire/shiny/132.png"
    ## 
    ## $sprites$versions$`generation-vi`$`omegaruby-alphasapphire`$front_shiny_female
    ## NULL
    ## 
    ## 
    ## $sprites$versions$`generation-vi`$`x-y`
    ## $sprites$versions$`generation-vi`$`x-y`$front_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-vi/x-y/132.png"
    ## 
    ## $sprites$versions$`generation-vi`$`x-y`$front_female
    ## NULL
    ## 
    ## $sprites$versions$`generation-vi`$`x-y`$front_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-vi/x-y/shiny/132.png"
    ## 
    ## $sprites$versions$`generation-vi`$`x-y`$front_shiny_female
    ## NULL
    ## 
    ## 
    ## 
    ## $sprites$versions$`generation-vii`
    ## $sprites$versions$`generation-vii`$icons
    ## $sprites$versions$`generation-vii`$icons$front_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-vii/icons/132.png"
    ## 
    ## $sprites$versions$`generation-vii`$icons$front_female
    ## NULL
    ## 
    ## 
    ## $sprites$versions$`generation-vii`$`ultra-sun-ultra-moon`
    ## $sprites$versions$`generation-vii`$`ultra-sun-ultra-moon`$front_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-vii/ultra-sun-ultra-moon/132.png"
    ## 
    ## $sprites$versions$`generation-vii`$`ultra-sun-ultra-moon`$front_female
    ## NULL
    ## 
    ## $sprites$versions$`generation-vii`$`ultra-sun-ultra-moon`$front_shiny
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-vii/ultra-sun-ultra-moon/shiny/132.png"
    ## 
    ## $sprites$versions$`generation-vii`$`ultra-sun-ultra-moon`$front_shiny_female
    ## NULL
    ## 
    ## 
    ## 
    ## $sprites$versions$`generation-viii`
    ## $sprites$versions$`generation-viii`$icons
    ## $sprites$versions$`generation-viii`$icons$front_default
    ## [1] "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-viii/icons/132.png"
    ## 
    ## $sprites$versions$`generation-viii`$icons$front_female
    ## NULL
    ## 
    ## 
    ## 
    ## 
    ## 
    ## $stats
    ## $stats[[1]]
    ## $stats[[1]]$base_stat
    ## [1] 48
    ## 
    ## $stats[[1]]$effort
    ## [1] 1
    ## 
    ## $stats[[1]]$stat
    ## $stats[[1]]$stat$name
    ## [1] "hp"
    ## 
    ## $stats[[1]]$stat$url
    ## [1] "https://pokeapi.co/api/v2/stat/1/"
    ## 
    ## 
    ## 
    ## $stats[[2]]
    ## $stats[[2]]$base_stat
    ## [1] 48
    ## 
    ## $stats[[2]]$effort
    ## [1] 0
    ## 
    ## $stats[[2]]$stat
    ## $stats[[2]]$stat$name
    ## [1] "attack"
    ## 
    ## $stats[[2]]$stat$url
    ## [1] "https://pokeapi.co/api/v2/stat/2/"
    ## 
    ## 
    ## 
    ## $stats[[3]]
    ## $stats[[3]]$base_stat
    ## [1] 48
    ## 
    ## $stats[[3]]$effort
    ## [1] 0
    ## 
    ## $stats[[3]]$stat
    ## $stats[[3]]$stat$name
    ## [1] "defense"
    ## 
    ## $stats[[3]]$stat$url
    ## [1] "https://pokeapi.co/api/v2/stat/3/"
    ## 
    ## 
    ## 
    ## $stats[[4]]
    ## $stats[[4]]$base_stat
    ## [1] 48
    ## 
    ## $stats[[4]]$effort
    ## [1] 0
    ## 
    ## $stats[[4]]$stat
    ## $stats[[4]]$stat$name
    ## [1] "special-attack"
    ## 
    ## $stats[[4]]$stat$url
    ## [1] "https://pokeapi.co/api/v2/stat/4/"
    ## 
    ## 
    ## 
    ## $stats[[5]]
    ## $stats[[5]]$base_stat
    ## [1] 48
    ## 
    ## $stats[[5]]$effort
    ## [1] 0
    ## 
    ## $stats[[5]]$stat
    ## $stats[[5]]$stat$name
    ## [1] "special-defense"
    ## 
    ## $stats[[5]]$stat$url
    ## [1] "https://pokeapi.co/api/v2/stat/5/"
    ## 
    ## 
    ## 
    ## $stats[[6]]
    ## $stats[[6]]$base_stat
    ## [1] 48
    ## 
    ## $stats[[6]]$effort
    ## [1] 0
    ## 
    ## $stats[[6]]$stat
    ## $stats[[6]]$stat$name
    ## [1] "speed"
    ## 
    ## $stats[[6]]$stat$url
    ## [1] "https://pokeapi.co/api/v2/stat/6/"
    ## 
    ## 
    ## 
    ## 
    ## $types
    ## $types[[1]]
    ## $types[[1]]$slot
    ## [1] 1
    ## 
    ## $types[[1]]$type
    ## $types[[1]]$type$name
    ## [1] "normal"
    ## 
    ## $types[[1]]$type$url
    ## [1] "https://pokeapi.co/api/v2/type/1/"
    ## 
    ## 
    ## 
    ## 
    ## $weight
    ## [1] 40

``` r
pokemon$height
```

    ## [1] 3

``` r
pokemon$abilities
```

    ## [[1]]
    ## [[1]]$ability
    ## [[1]]$ability$name
    ## [1] "limber"
    ## 
    ## [[1]]$ability$url
    ## [1] "https://pokeapi.co/api/v2/ability/7/"
    ## 
    ## 
    ## [[1]]$is_hidden
    ## [1] FALSE
    ## 
    ## [[1]]$slot
    ## [1] 1
    ## 
    ## 
    ## [[2]]
    ## [[2]]$ability
    ## [[2]]$ability$name
    ## [1] "imposter"
    ## 
    ## [[2]]$ability$url
    ## [1] "https://pokeapi.co/api/v2/ability/150/"
    ## 
    ## 
    ## [[2]]$is_hidden
    ## [1] TRUE
    ## 
    ## [[2]]$slot
    ## [1] 3
