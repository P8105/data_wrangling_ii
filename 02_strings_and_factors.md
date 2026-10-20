Strings and factors
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
library(p8105.datasets)
```

## strings and regex

``` r
string_vec = c("my", "name", "is", "jeff")

str_detect(string_vec, "jeff")
```

    ## [1] FALSE FALSE FALSE  TRUE

``` r
str_detect(string_vec, "My")
```

    ## [1] FALSE FALSE FALSE FALSE

``` r
str_detect(string_vec, "my")
```

    ## [1]  TRUE FALSE FALSE FALSE

``` r
str_replace(string_vec, "my", "My")
```

    ## [1] "My"   "name" "is"   "jeff"

``` r
string_vec = c(
  "i think we all rule for participating",
  "i think i have been caught",
  "i think this will be quite fun actually",
  "it will be fun, i think"
  )

str_detect(string_vec, "i think")
```

    ## [1] TRUE TRUE TRUE TRUE

``` r
str_detect(string_vec, "^i think")
```

    ## [1]  TRUE  TRUE  TRUE FALSE

``` r
str_detect(string_vec, "i think$")
```

    ## [1] FALSE FALSE FALSE  TRUE

``` r
string_vec = c(
  "Time for a Pumpkin Spice Latte!",
  "went to the #pumpkinpatch last weekend",
  "Pumpkin Pie is obviously the best pie",
  "SMASHING PUMPKINS -- LIVE IN CONCERT!!"
  )

str_detect(string_vec, "[Pp]umpkin")
```

    ## [1]  TRUE  TRUE  TRUE FALSE

``` r
string_vec = c(
  '7th inning stretch',
  '1st half soon to begin. Texas won the toss.',
  'she is 5 feet 4 inches tall',
  '3AM - cant sleep :('
  )

str_detect(string_vec, "[0-9][a-zA-Z]")
```

    ## [1]  TRUE  TRUE FALSE  TRUE

``` r
string_vec = c(
  'Its 7:11 in the evening',
  'want to go to 7-11?',
  'my flight is AA711',
  'NetBios: scanning ip 203.167.114.66'
  )

str_detect(string_vec, "7.11")
```

    ## [1]  TRUE  TRUE FALSE  TRUE

``` r
string_vec = c(
  'The CI is [2, 5]',
  ':-]',
  ':-[',
  'I found the answer on pages [6-7]'
  )

str_detect(string_vec, "\\[")
```

    ## [1]  TRUE FALSE  TRUE  TRUE

## quick note on factors

``` r
vec_sex = factor(c("male", "male", "female", "female"))

as.numeric(vec_sex)
```

    ## [1] 2 2 1 1

``` r
vec_sex = fct_relevel(vec_sex, "male")

as.numeric(vec_sex)
```

    ## [1] 1 1 2 2

## Import NSDUH

``` r
url = "https://www.samhsa.gov/data/sites/default/files/NSDUHsaeShortTermCHG2015/NSDUHsaeShortTermCHG2015.htm"

drug_use_html = 
  read_html(url)
```

Pull the table out

``` r
marj_df = 
  drug_use_html |> 
  html_table() |> 
  first() |> 
  slice(-1) |> 
  select(-contains("P Value")) |> 
  pivot_longer(
    -State, 
    names_to = "age_year", 
    values_to = "prev"
  ) |> 
  separate(age_year, into = c("age", "year"), sep = "\\(") |> 
  mutate(
    year = str_replace(year, "\\)", ""), 
    prev = str_replace(prev, "[a-c]", ""),
    prev = as.numeric(prev)
  )
```

make a quick plot

``` r
marj_df |> 
  filter(age == "12-17") |> 
  mutate(State = fct_reorder(State, prev)) |> 
  ggplot(aes(x = State, y = prev, color = year)) + 
  geom_point() + 
  theme(axis.text.x = element_text(angle = 90, hjust = 1))
```

<img src="02_strings_and_factors_files/figure-gfm/unnamed-chunk-11-1.png" alt="" width="90%" />

## NYC Airbnb

``` r
data(nyc_airbnb)

nyc_airbnb |> 
  filter(
    neighbourhood_group == "Manhattan",
    price < 1000) |> 
  mutate(neighbourhood = fct_reorder(neighbourhood, price)) |> 
  ggplot(aes(x = neighbourhood, y = price)) + 
  geom_boxplot() + 
  coord_flip()
```

<img src="02_strings_and_factors_files/figure-gfm/unnamed-chunk-12-1.png" alt="" width="90%" />

## Restaurant inspections
