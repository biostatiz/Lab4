Activity Four - Lab1
================
Taehoon Ha
10/04/2018

### 1. Write a function that normalizes a vector (with two parameters):

-   vector to be normalized.
-   parameter that allows normalize the vector two ways: only centered with zero mean and also standardize with mean zero and standard deviation 1.

Example:
\* input(1, 2, 3, 4) \* centered vector is

``` r
standardize <- function (vec, centered_only = F) {
  
  # create an empty vector in order to save the output result
  result <- c()
  
  # centered_only = F
  if (!centered_only) {
    for (i in 1:length(vec)) {
      result[i] <- (vec[i] - mean(vec)) / sd(vec) 
    }
  
  # centered_only = T
  } else {
    for (i in 1:length(vec)) {
      result[i] <- vec[i] - mean(vec) 
    }
  }
  
  # return the result vector
  return(result)  
}

a <- c(1, 2, 3, 4)
standardize(a, centered_only = T)
```

    ## [1] -1.5 -0.5  0.5  1.5

``` r
standardize(a, centered_only = F)
```

    ## [1] -1.1618950 -0.3872983  0.3872983  1.1618950

### 2. Modify previous function in order to handle missing values in a vector.

``` r
standardize <- function (vec, centered_only=F, na.rm=F) {
  
  # create an empty vector in order to save the output result
  result <- c()
  
  # 1. centered_only = F
  if(!centered_only) {
    
    # 1.a centered_only = F, na.rm = F
    if(!na.rm) {
      for (i in 1:length(vec)) {
        result[i] <- (vec[i] - mean(vec)) / sd(vec)}}
    
    # 1.b centered_only = F, na.rm = T
    else {
      for (i in 1:length(vec)) {
        result[i] <- (vec[i] - mean(vec, na.rm = T)) / sd(vec, na.rm = T)
      }
    }
  }
  
  # 2. centered_only = F
  else {
    
    # 2.a centered_only = T, na.rm = F
    if(!na.rm) {
      for (i in 1:length(vec)) {
        result[i] <- vec[i] - mean(vec)}}
    
    # 2.b centered_only = T, na.rm = T
    else {
      for (i in 1:length(vec)) {
        result[i] <- vec[i] - mean(vec, na.rm = T)
      }
    }
  }
  
  # return the result
  return(result)
}

standardize(c(1,2,3,4,NA))
```

    ## [1] NA NA NA NA NA

``` r
standardize(c(1,2,3,4,NA), na.rm = T)
```

    ## [1] -1.1618950 -0.3872983  0.3872983  1.1618950         NA

### 3. Create a 4 by 4 matrix

Create a function to calculate sum of matrix elements without built-in function sum()

``` r
# The function below automatically generates 4 by 4 table filled with 1:16 vector and calculated the sum of matrix.

matrix_sum <- function() {
  
  # generating a 4 by 4 table filled with the given vector, 1:16
  mat <- matrix(1:16, 4, 4)
  
  # the vector for saving the sum
  mat.sum <- 0
  
  # row-indexing from 1 to 4
  for (i in 1:nrow(mat)) {
    
    # col-indexing begin from 1 to 4
    for (j in 1:ncol(mat)) {
      
      # saving the sum result in the vector
      mat.sum <- mat.sum + mat[i, j]
    }
  }
  
  # show the matrix used and return the sum of the matrix
  print(mat)
  return(mat.sum)
}

matrix_sum()
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    5    9   13
    ## [2,]    2    6   10   14
    ## [3,]    3    7   11   15
    ## [4,]    4    8   12   16

    ## [1] 136

``` r
# Advanced

matrix_sum <- function (vec, nrow, ncol) {
  mat <- matrix(vec, nrow = nrow, ncol = ncol)
  mat.sum <- 0
  
  for (i in 1:nrow(mat)) {
    for (j in 1:ncol(mat)) {
      mat.sum <- mat.sum + mat[i, j]
    }
  }
  print(mat)
  cat(paste0("\nThis is ", nrow, " by ", ncol, " matrix.\n", "The sum of matrix is: \n"))
  return(mat.sum)
}

matrix_sum(vec = 1:16, nrow = 4, ncol = 4)
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    5    9   13
    ## [2,]    2    6   10   14
    ## [3,]    3    7   11   15
    ## [4,]    4    8   12   16
    ## 
    ## This is 4 by 4 matrix.
    ## The sum of matrix is:

    ## [1] 136

``` r
# Other Example
matrix_sum(vec = 1:100, nrow = 20, ncol = 5)
```

    ##       [,1] [,2] [,3] [,4] [,5]
    ##  [1,]    1   21   41   61   81
    ##  [2,]    2   22   42   62   82
    ##  [3,]    3   23   43   63   83
    ##  [4,]    4   24   44   64   84
    ##  [5,]    5   25   45   65   85
    ##  [6,]    6   26   46   66   86
    ##  [7,]    7   27   47   67   87
    ##  [8,]    8   28   48   68   88
    ##  [9,]    9   29   49   69   89
    ## [10,]   10   30   50   70   90
    ## [11,]   11   31   51   71   91
    ## [12,]   12   32   52   72   92
    ## [13,]   13   33   53   73   93
    ## [14,]   14   34   54   74   94
    ## [15,]   15   35   55   75   95
    ## [16,]   16   36   56   76   96
    ## [17,]   17   37   57   77   97
    ## [18,]   18   38   58   78   98
    ## [19,]   19   39   59   79   99
    ## [20,]   20   40   60   80  100
    ## 
    ## This is 20 by 5 matrix.
    ## The sum of matrix is:

    ## [1] 5050
