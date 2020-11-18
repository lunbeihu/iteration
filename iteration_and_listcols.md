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
    ## -3.12436 -0.65979 -0.02698 -0.01385  0.63977  3.26331

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
    ##  [1] 2.969349 3.576863 2.575052 3.335534 2.954255 1.687575 2.304987 2.880612
    ##  [9] 3.155372 3.365927 2.377851 3.558658 3.732030 2.155189 3.347361 1.937294
    ## [17] 2.571307 4.966458 4.669810 3.288997
    ## 
    ## $b
    ##  [1]  4.10738644  5.51801934 -6.15140594 -2.35265708 -0.78726070  6.47526415
    ##  [7] -6.31691226 -1.70266696  0.79967297 -2.57023790 -1.93601866  3.58588314
    ## [13] -0.56850677 -3.13420152  4.37284762 -7.58979997  4.05140396  6.86662697
    ## [19] -2.88815160  1.46359442  3.92600990  1.58062652  5.02458593  3.71056813
    ## [25] -3.02657311  4.08223245 -4.37771540 -0.05231276 11.11743124  7.42388691
    ## 
    ## $c
    ##  [1] 10.333203  9.949422 10.094933 10.142680 10.404907 10.040265 10.315041
    ##  [8] 10.336313 10.019949 10.191283  9.982974  9.720871 10.164089  9.751071
    ## [15]  9.789987 10.095296  9.850408 10.008236  9.926729 10.092553  9.974730
    ## [22] 10.279697 10.009939 10.199827 10.367735 10.124526  9.796342 10.096348
    ## [29]  9.803340 10.239894 10.086646 10.029781  9.932762  9.874753 10.242832
    ## [36] 10.057171  9.656523 10.293614  9.895189 10.092709
    ## 
    ## $d
    ##  [1] -3.181631 -2.729099 -2.339308 -1.364337 -2.775633 -2.297165 -1.005164
    ##  [8] -2.255553 -3.259052 -3.661655 -3.933702 -2.114463 -3.060370 -3.126155
    ## [15] -1.579821 -3.206012 -2.390044 -1.997903 -2.408760 -2.921548

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
    ## 1  3.07 0.828

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.02  4.59

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.1 0.191

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.58 0.752

Let’s use a for loop:

``` r
output = vector("list", length = 4)

for (i in 1:4) {
  
  output[[i]] = mean_and_sd(list_norm[[i]])
  
}
```
