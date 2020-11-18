Iteration and listcols
================

## Lists

You can put anything in a list.

``` r
l = list(
  vec_numeric = 5:8,
  vec_logical = c(TRUE, TRUE, FALSE, TRUE, FALSE, FALSE),
  mat = matrix(1:8, nrow = 2, ncol = 4),
  summary     = summary(rnorm(1000))
  )
```

``` r
l
```

    ## $vec_numeric
    ## [1] 5 6 7 8
    ## 
    ## $vec_logical
    ## [1]  TRUE  TRUE FALSE  TRUE FALSE FALSE
    ## 
    ## $mat
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    3    5    7
    ## [2,]    2    4    6    8
    ## 
    ## $summary
    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ## -3.66544 -0.61634  0.03159  0.02901  0.67744  3.34194

``` r
l$vec_numeric
```

    ## [1] 5 6 7 8

``` r
l[[1]]
```

    ## [1] 5 6 7 8

``` r
mean(l[["vec_numeric"]])
```

    ## [1] 6.5

## `for` loop

Create a new list.

``` r
list_norm = 
  list(
    a = rnorm(20, mean = 3, sd = 1),
    b = rnorm(30, mean = 0, sd = 5),
    c = rnorm(40, mean = 10, sd = .2),
    d = rnorm(20, mean = -3, sd = 1)
  )
```

``` r
list_norm
```

    ## $a
    ##  [1] 4.043003 3.683418 2.195923 2.389058 2.653400 2.380032 4.738035 1.872313
    ##  [9] 1.446745 1.970921 2.721178 2.247343 1.470484 4.231599 3.851118 3.018072
    ## [17] 2.352171 3.193289 2.546633 3.385953
    ## 
    ## $b
    ##  [1] -11.8462929   2.7677202  -0.1220924   1.5655992   4.4534298  -3.1785426
    ##  [7]  -4.3359935  -7.0102545   4.6986308   2.4208202   6.9853706  -0.9227830
    ## [13]   1.7792315  -5.2476121   7.0317992   6.5324075   1.3760856  -5.2163109
    ## [19]   7.4348466   2.1486370   3.3255072  -2.5526787   9.9784770   7.4093971
    ## [25]  -6.9485493   3.5004538  -1.1098987  -0.1308668   1.7477796   7.0121195
    ## 
    ## $c
    ##  [1]  9.920360 10.323109 10.392457 10.237573 10.247890 10.005058  9.700357
    ##  [8] 10.036144 10.094995 10.169399  9.574160 10.246872 10.043013  9.987429
    ## [15]  9.885477  9.934068  9.947660  9.783002  9.870572 10.294183  9.686252
    ## [22]  9.969903  9.791503 10.135952 10.183529 10.082382 10.077828 10.030119
    ## [29] 10.019285  9.880016 10.092275  9.893637  9.825787  9.857204  9.684416
    ## [36]  9.908825 10.381569 10.381981  9.874306 10.234916
    ## 
    ## $d
    ##  [1] -2.581551 -3.408214 -2.345054 -3.991553 -4.083915 -4.516727 -2.645467
    ##  [8] -3.847885 -4.179043 -3.337268 -2.262273 -3.591069 -3.287120 -3.447061
    ## [15] -3.589716 -3.594522 -3.893320 -3.298054 -4.714375 -3.326409

Pause and get my old function.

``` r
mean_and_sd = function(x) {
  
  if (!is.numeric(x)) {
    stop("Input must be numeric")
  }
  
  if (length(x) < 3) {
    stop("Input must have at least three numbers")
  }
  
  mean_x = mean(x)
  sd_x = sd(x)
  
  tibble(
    mean = mean_x,
    sd = sd_x
  )

}
```

I can apply that function to each list element.

``` r
mean_and_sd(list_norm[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.82 0.924

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.12  5.21

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.207

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.50 0.667

Let’s use a for loop:

``` r
output = vector("list", length = 4)

for (i in 1:4) {
  
  output[[i]] = mean_and_sd(list_norm[[i]])
  
}
```

## Let’s try map\!

``` r
output = map(list_norm, mean_and_sd)
```

what if you want a different function.

``` r
output = map(list_norm, median)
```

``` r
output = map_dbl(list_norm, median, .id = "input")
```

``` r
output = map_df(list_norm, mean_and_sd, .id = "input")
```

## List columns\!

``` r
listcol_df = 
  tibble(
    name = c("a", "b", "c", "d"),
    samp = list_norm
  )
```

``` r
listcol_df %>% pull(name)
```

    ## [1] "a" "b" "c" "d"

``` r
listcol_df %>% pull(samp) 
```

    ## $a
    ##  [1] 4.043003 3.683418 2.195923 2.389058 2.653400 2.380032 4.738035 1.872313
    ##  [9] 1.446745 1.970921 2.721178 2.247343 1.470484 4.231599 3.851118 3.018072
    ## [17] 2.352171 3.193289 2.546633 3.385953
    ## 
    ## $b
    ##  [1] -11.8462929   2.7677202  -0.1220924   1.5655992   4.4534298  -3.1785426
    ##  [7]  -4.3359935  -7.0102545   4.6986308   2.4208202   6.9853706  -0.9227830
    ## [13]   1.7792315  -5.2476121   7.0317992   6.5324075   1.3760856  -5.2163109
    ## [19]   7.4348466   2.1486370   3.3255072  -2.5526787   9.9784770   7.4093971
    ## [25]  -6.9485493   3.5004538  -1.1098987  -0.1308668   1.7477796   7.0121195
    ## 
    ## $c
    ##  [1]  9.920360 10.323109 10.392457 10.237573 10.247890 10.005058  9.700357
    ##  [8] 10.036144 10.094995 10.169399  9.574160 10.246872 10.043013  9.987429
    ## [15]  9.885477  9.934068  9.947660  9.783002  9.870572 10.294183  9.686252
    ## [22]  9.969903  9.791503 10.135952 10.183529 10.082382 10.077828 10.030119
    ## [29] 10.019285  9.880016 10.092275  9.893637  9.825787  9.857204  9.684416
    ## [36]  9.908825 10.381569 10.381981  9.874306 10.234916
    ## 
    ## $d
    ##  [1] -2.581551 -3.408214 -2.345054 -3.991553 -4.083915 -4.516727 -2.645467
    ##  [8] -3.847885 -4.179043 -3.337268 -2.262273 -3.591069 -3.287120 -3.447061
    ## [15] -3.589716 -3.594522 -3.893320 -3.298054 -4.714375 -3.326409

``` r
listcol_df %>% 
  filter(name == "a")
```

    ## # A tibble: 1 x 2
    ##   name  samp        
    ##   <chr> <named list>
    ## 1 a     <dbl [20]>

Let’s try some operations.

``` r
mean_and_sd(listcol_df$samp[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.82 0.924

``` r
mean_and_sd(listcol_df$samp[[2]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.12  5.21

Can I just … map?

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.82 0.924
    ## 
    ## $b
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.12  5.21
    ## 
    ## $c
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.207
    ## 
    ## $d
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.50 0.667

So … can I add a list column??

``` r
listcol_df = 
  listcol_df %>% 
  mutate(
    summary = map(samp, mean_and_sd),
    medians = map_dbl(samp, median))
```
