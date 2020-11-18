Writing functions
================

## Do something simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1]  0.46279451  0.66269614  0.82360902 -1.32886971 -0.63449696  0.94576588
    ##  [7]  1.75165498 -0.09880251 -0.92009865 -0.00913375  0.12285412  2.05657450
    ## [13]  1.04235602  0.72607275  0.87409396 -0.49228935  0.01661861  0.08725249
    ## [19] -0.32551175  0.65140470 -0.86284094 -1.75332184 -0.29184721 -0.67516557
    ## [25] -0.35732644  1.60705059  0.18248145 -1.23736888 -1.33350476 -1.69270140

I want a function to compute z-scores

``` r
z_scores = function(x) {
  
  if (!is.numeric(x)) {
    stop("Input must be numeric")
  }
  
  if (length(x) < 3) {
    stop("Input must have at least three numbers")
  }
  
  z = (x - mean(x)) / sd(x)
  
  return(z)

}

z_scores(x_vec)
```

    ##  [1]  0.46279451  0.66269614  0.82360902 -1.32886971 -0.63449696  0.94576588
    ##  [7]  1.75165498 -0.09880251 -0.92009865 -0.00913375  0.12285412  2.05657450
    ## [13]  1.04235602  0.72607275  0.87409396 -0.49228935  0.01661861  0.08725249
    ## [19] -0.32551175  0.65140470 -0.86284094 -1.75332184 -0.29184721 -0.67516557
    ## [25] -0.35732644  1.60705059  0.18248145 -1.23736888 -1.33350476 -1.69270140

Try my function on some other things. These should give errors.

``` r
z_scores(3)
```

    ## Error in z_scores(3): Input must have at least three numbers

``` r
z_scores("my name is jeff")
```

    ## Error in z_scores("my name is jeff"): Input must be numeric

``` r
z_scores(mtcars)
```

    ## Error in z_scores(mtcars): Input must be numeric

``` r
z_scores(c(TRUE, TRUE, FALSE, TRUE))
```

    ## Error in z_scores(c(TRUE, TRUE, FALSE, TRUE)): Input must be numeric
