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
    ##       Min.    1st Qu.     Median       Mean    3rd Qu.       Max. 
    ## -3.0250564 -0.6681156  0.0067688 -0.0001876  0.6412569  2.8293075

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
    ##  [1] 4.4162845 4.6506957 2.2688810 2.2779059 1.7643482 2.9834506 2.3105245
    ##  [8] 0.4288039 1.7089883 2.5137296 4.9726186 2.6832158 2.8046601 2.4063958
    ## [15] 4.2727957 3.5525557 2.9148742 3.2724970 3.4159683 4.1652148
    ## 
    ## $b
    ##  [1] -4.7365335  4.3765145 -2.0269351  6.5118561  5.4932195 -9.4707314
    ##  [7] -7.3264903  3.7187152 -0.1838389  9.5571963  5.7023535 10.2138860
    ## [13]  6.5072339 -5.9681724 -0.4992492 -0.1989565  3.6947740  2.0225932
    ## [19]  7.9253368  6.5400630 -2.3494874  6.4500660  3.9631295 -6.2733418
    ## [25] -5.5007857  6.0709250 10.1995013  3.8310247  4.3241397  1.9490698
    ## 
    ## $c
    ##  [1] 10.194185 10.326245 10.119129  9.356349  9.917576 10.280140  9.494737
    ##  [8] 10.191615 10.052005  9.840877 10.288783  9.646949  9.795515 10.103415
    ## [15]  9.710059  9.871945  9.984958 10.191715 10.168242  9.983193  9.868787
    ## [22] 10.012231 10.319600  9.844508 10.142621  9.680254  9.892778  9.842088
    ## [29]  9.947565 10.054060 10.236893  9.887029  9.872611 10.410336  9.574379
    ## [36] 10.328695 10.221319 10.194075  9.965864 10.069922
    ## 
    ## $d
    ##  [1] -0.7911103 -3.5358615 -1.3281090 -1.6585875 -2.4384045 -3.3439904
    ##  [7] -1.9578935 -3.6592781 -3.6701318 -2.9877927 -1.9665945 -3.9724918
    ## [13] -1.8606265 -3.3405643 -4.2858909 -2.6065784 -2.9195893 -3.9990686
    ## [19] -2.3946106 -3.7941397

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
    ## 1  2.99  1.13

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.15  5.50

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.245

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.83 0.995

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
