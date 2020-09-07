---
# Common-Defined params
title: "Introduction to Rcpp and RcppArmadillo"
date: "2020-09-07T10:00:00+09:00"
description: "Coding in C++ made easy by Rcpp"
categories:
  - "Rcpp"
tags:
  - "Rcpp"
menu: side # Optional, add page to a menu. Options: main, side, footer

# Theme-Defined params
#thumbnail: "/posts/unnamed-chunk-12-1.png" # Thumbnail image
lead: "You can actually code in C++ in RStudio" # Lead text
comments: true # Enable Disqus comments for specific page
authorbox: true # Enable authorbox for specific page
pager: true # Enable pager navigation (prev/next) for specific page
toc: true # Enable Table of Contents for specific page
mathjax: true # Enable MathJax for specific page
sidebar: "right" # Enable sidebar (on the right side) per page
widgets: # Enable sidebar widgets in given order per page
  - "search"
  - "recent"
  - "taglist"
---

 

##### **References** 

This post is based on several sources including; <br>

* https://dirk.eddelbuettel.com/papers/useR2019_rcpp_tutorial.pdf 

  (Intro to Rcpp and Rcpparmadillo)

* https://privefl.github.io/R-presentation/Rcpp.html 

  (Intro to Rcpp)

* https://teuder.github.io/rcpp4everyone_en/ 

  (for Rcpp object class, methods and functions) 

* http://arma.sourceforge.net/docs.html 

  (for RcppArmadillo class, methods and functions) 

* http://dirk.eddelbuettel.com/code/rcpp/Rcpp-sugar.pdf 

  (for Rcpp sugar functions, including prob dists) 

* https://teuder.github.io/rcpp4everyone_en/310_Rmath.html 

  (for Rmath in ``R::`` namespace, mostly prob dists) 

* https://www.w3schools.com/cpp/cpp_math.asp 

  (for cmath in ``std::`` namespace) 

  

and other pages annotated in the post below.



$~$

##### <br> **Required Packages**

``` r
sapply(c('Rcpp','RcppArmadillo', # Rcpp families
         'rbenchmark','microbenchmark','bench', # to compare speed
         'lobstr' # for object address
         ), require, character.only=T)
```

 

## 1. Main Tools for Exploring Rcpp

--------------------------------

 

<br> **evalCpp(code, depends = , plugins = )** : evaluates a single
C++ expression. You can declare includes and dependencies as well.

``` r
evalCpp("2+2")
```

    ## [1] 4

``` r
evalCpp("std::numeric_limits<double>::min()") # returns minimum integer in C++ capacity 
```

    ## [1] 2.225074e-308

<br> **cppFunction(code, depends = , plugins = )** : creates,
compiles and links a C++ file, and then creates an R function for access

``` r
src = 'int exampleCpp11(){
auto x = 10; // auto assign (guess) data type, cpp11 feature
return x;
}'
cppFunction(src, plugins = c('cpp11')) # include cpp11 plugin
exampleCpp11()
```

    ## [1] 10

``` r
cppFunction("int f(int a, int b){return(a+b);}")
f(1,2)
```

    ## [1] 3

<br> **sourceCpp()** : actual workhorse behind evalCpp() and
cppFunction(). bulids on and extends inline::cxxfunction() and made much
easier to use. plug-ins and dependencies are declared inside the code
file. we can skip creating a separate cpp file and instead include the
entire code chunk inside R markdown code chunk with engine set to be
Rcpp as shown below

``` cpp
#include <RcppArmadillo.h>
// [[Rcpp::depends(RcppArmadillo)]]
// [[Rcpp::plugins("cpp11")]]
using namespace Rcpp;
using namespace arma;
// convolution of two finite vectors
//[[Rcpp::export]]
vec convolution(vec x, vec y){
  auto nx = x.size(), ny = y.size();
  vec z(nx + ny - 1);
  for(int i=0; i < nx; i++){
    for(int j=0; j < ny; j++){
      z[i+j] += x[i] * y[j];
    }
  }
  return z;
}
```

``` r
t(convolution(1:3, 2:5))
```

    ##      [,1] [,2] [,3] [,4] [,5] [,6]
    ## [1,]    2    7   16   22   22   15

 



## 2. How fast is Rcpp?

-----------------

 

Let us see a simple example to illustrate speed enhance brought by Rcpp. Consider a recursive function defined as below **(fibonacci function)**;
$$
f(n) = 
\begin{cases}
n \quad &(n < 2)\\
f(n-1)+f(n-2)\quad &(n \geq 2)
\end{cases}
$$

To implement this function in R we would have

``` r
f = function(n){
  ifelse(n<2, n, f(n-1)+f(n-2)) # recursive function
}
sapply(0:10, f)
```

    ##  [1]  0  1  1  2  3  5  8 13 21 34 55

<br> R is particularly slow in performing loops and repeated function calls. Since we have a recursive function, R is pretty slow at returning a function value as *n* grows larger.

``` r
rbenchmark::benchmark(f(10), f(15), f(20))[,1:4]
```

    ##    test replications elapsed relative
    ## 1 f(10)          100    0.04      1.0
    ## 2 f(15)          100    0.44     11.0
    ## 3 f(20)          100    4.30    107.5

<br> Rcpp alternative would be

``` r
src = 'int g(int n){
  if (n<2) return n;
  return g(n-1)+g(n-2);
}'
cppFunction(src)
sapply(0:10, g)
```

    ##  [1]  0  1  1  2  3  5  8 13 21 34 55

<br> which is way faster than R implementation.

``` r
bench::mark(f(20), g(20))
```

    ## Warning: Some expressions had a GC in every iteration; so filtering is disabled.
    
    ## # A tibble: 2 x 6
    ##   expression      min   median `itr/sec` mem_alloc `gc/sec`
    ##   <bch:expr> <bch:tm> <bch:tm>     <dbl> <bch:byt>    <dbl>
    ## 1 f(20)        46.6ms   53.2ms      19.0        0B     20.9
    ## 2 g(20)        22.6us   23.6us   41087.     2.49KB      0

``` r
res = microbenchmark::microbenchmark(f(20), g(20))
res
```

    ## Unit: microseconds
    ##   expr     min      lq      mean   median      uq     max neval
    ##  f(20) 39004.1 40723.2 43160.746 41561.85 42823.5 83602.0   100
    ##  g(20)    22.4    22.8    30.303    33.70    34.6    60.1   100

``` r
suppressMessages(microbenchmark:::autoplot.microbenchmark(res))
```

![](/posts/unnamed-chunk-12-1.png)

 

### 3. More Rcpp examples

------------------

 

##### **1. finding max of a vector (in an inefficient way)**

``` cpp
#include <Rcpp.h>
using namespace Rcpp;
//[[Rcpp::export]]
double getMax(NumericVector v){
  int n = v.size();
  double m = v[0];
  for(int i=0; i<n; i++){
    if (v[i] > m) {
      Rcout << "Now " << m << "\n";
      m = v[i];
    }
  }
  return m;
}
```

``` r
getMax(c(1,4,5,1,2))
```

    ## Now 1
    ## Now 4
    
    ## [1] 5

##### <br> **2. column sums function**

``` cpp
#include <Rcpp.h>
using namespace Rcpp;
//[[Rcpp::export]]
NumericVector colSums(NumericMatrix mat){
  // "size_t" stands for a really large integer, and is used for a large iteration
  // Rcpp alternative is "R_xlen_t"
  size_t cols = mat.cols(); // .cols(): n of columns
  NumericVector res(cols);
  for(size_t i=0; i<cols; i++){
    res[i] = sum(mat.column(i));
  }
  return(res);
}
// read https://teuder.github.io/rcpp4everyone_en/100_matrix.html#member-functions-1
// for more Rcpp::NumericMatrix methods
```

``` r
colSums(diag(3))
```

    ## [1] 1 1 1

##### <br> **3. pmax in Rcpp**

``` cpp
#include <Rcpp.h>
using namespace Rcpp;
//[[Rcpp::export]]
NumericVector pmax_rcpp(NumericVector x, NumericVector y){
  int n = std::max(x.size(), y.size());
  NumericVector x_recl = rep_len(x, n);
  NumericVector y_recl = rep_len(y, n);
  NumericVector out(n);
  for(int i=0; i<n; i++){
    out[i] = std::max(x_recl[i], y_recl[i]);
  }
  return out;
}
```

##### <br> **Note: using C++ math functions compared to Rcpp sugar**

``` cpp
#include <Rcpp.h>
using namespace Rcpp;
//[[Rcpp::export]]
void cppmath(){
  double x = 3.7, y = 1.2;
  Rcout << std::abs(x); // read https://www.w3schools.com/cpp/cpp_math.asp for more
  Rcout << std::cos(x) << std::endl;
  Rcout << abs(x); // most are included in Rcpp sugar
  Rcout << cos(x) << std::endl;
  // except for max and min. Note that Rcpp max, min applies to a vector
  Rcout << std::max(x,y); 
  NumericVector z = NumericVector::create(x,y);
  Rcout << max(z) << std::endl;
}
```

``` r
cppmath()
```

    ## 3.7-0.8481
    ## 3.7-0.8481
    ## 3.73.7

##### <br> **4. lapply mean in Cpp**

``` cpp
#include <Rcpp.h>
using namespace Rcpp;
//[[Rcpp::export]]
NumericVector f4(List x){
  int n = x.size();
  NumericVector res(n);
  for(int i=0; i<n; i++){
    res[i] = mean( as<NumericVector>(x[i]) );
    // mean: Rcpp function for NumericVector -> use as<type>(x) to coerce x into "type"
  }
  return res;  
}
```

``` r
x = lapply(1:10, runif)
isTRUE(all.equal(f4(x), sapply(x, mean)))
```

    ## [1] TRUE

 



### 4. Cautions with Rcpp Functions

----------------------------

 

##### **1. NA\_LOGICAL in Rcpp is evaluated as TRUE by Cpp bool type.** 

Do NOT use NA\_LOGICAL for if condition as it is, and be specific to include `if(condition==TRUE)`

``` r
src = 'List scalar_missings(){
  int int_s = NA_INTEGER;
  String chr_s = NA_STRING; // scalar type corresponding to the element of CharacterVector
  bool lgl_s = NA_LOGICAL; // will be coerced to "TRUE" bool
  double num_s = NA_REAL;
  return List::create(int_s, chr_s, lgl_s, num_s);
}'
cppFunction(src)
str(scalar_missings())
```

    ## List of 4
    ##  $ : int NA
    ##  $ : chr NA
    ##  $ : logi TRUE
    ##  $ : num NA

##### <br>**2. When dealing with Rcpp objects, Use clone() to avoid coupling.** 

Assigning an object to another object is to create another alias for the original value. In R, if you change the value of the original vector, R first makes a copy of it and then performs modification. But in Rcpp you have to explicitly copy the object. 

**This is NOT the case for C++ object such as** `int`. In general, to call by reference, you have to use `&`,
e.g. `int& a = b`. Rcpp objects by default assign only reference with `=`, probably for memory issue, so to add `&` for Rcpp objects does nothing new (besides adding a mere “visual sugar”)
<a href="https://stackoverflow.com/questions/48363076/declare-a-variable-as-a-reference-in-rcpp" class="uri">https://stackoverflow.com/questions/48363076/declare-a-variable-as-a-reference-in-rcpp</a>

``` R
library(lobstr)
x = y = 1:3; obj_addr(x) == obj_addr(y)
```

    ## [1] TRUE

``` r
x[1] = 1; obj_addr(x) == obj_addr(y)
```

    ## [1] FALSE

``` cpp
#include <Rcpp.h>
using namespace Rcpp;
//[[Rcpp::plugins("cpp11")]]
//[[Rcpp::export]]
void test(){
  NumericVector v1 = {1, 2, 3};
  NumericVector v2 = v1; // shallow copy
  NumericVector v3 = clone(v1); // deep copy
  
  v1[0] = 100; 
  
  Rcout << "v1 = " << v1 << std::endl; 
  Rcout << "v2 = " << v2 << std::endl; // modified (coupled)
  Rcout << "v3 = " << v3 << std::endl; // intact
}
```

``` r
test()
```

    ## v1 = 100 2 3
    ## v2 = 100 2 3
    ## v3 = 1 2 3

##### <br> **3. Return type of operator** `[]` **is NOT a vector, but rather a vector proxy** `Vector::Proxy`. 

Always remember to coerce the vector proxy into vector with `as<T>()`

``` r
src = 'void test(){
  NumericVector v = {1,2,3,4,5};
  IntegerVector i = {1,3}; // vector of indices
  
  // compile error with this code
  // double x1 = sum(v[i]);
  
  // instead, try make a copy,
  NumericVector vi = v[i];
  double x2 = sum(vi);
  Rcout << x2 << std::endl;
  
  // or without having to make a copy, coerce into NumericVector
  double x3 = sum(as<NumericVector>(v[i]));
  Rcout << x3 << std::endl;
}'
cppFunction(src, plugins = c('cpp11'))
test()
```

    ## 6
    ## 6

##### <br> **4. In C++, integer division, where denominator and the numerator are both integer, returns quotient, i.e. truncates to the largest smaller integer.** 

To get double value from integer operation,
use typecasting. For detail, read
<a href="https://stackoverflow.com/questions/3144610/integer-division-how-do-you-produce-a-double" class="uri">https://stackoverflow.com/questions/3144610/integer-division-how-do-you-produce-a-double</a>

``` cpp
#include <Rcpp.h>
using namespace Rcpp;
//[[Rcpp::export]]
double int_div1(){
  int x=3, y=2;
  double z = x/y; // returns 1.0
  return(z);
}

// Need explicit typecasting 
// 1) explicit
//[[Rcpp::export]]
double int_div2(){
  int x=3, y=2;
  double z = (double) x / y; // typecast 3L into 3.0 explicitly, and y coerced to double naturally
  return z;
}

// 2) implicit 
//[[Rcpp::export]]
double int_div3(){
  int x=3, y=2;
  double z = x*1.0 / y; // typecast 3L into 3.0 implicitly, and y coerced to double naturally
  return z;
}
```

``` r
c(int_div1(), int_div2(), int_div3())
```

    ## [1] 1.0 1.5 1.5

 



## 5. RcppArmadillo

 

Rcpp vectors and matrices do not support linear algebra. We need RcppArmadillo for math operations.

#### <br> What is Armadillo? <br>

(<a href="https://dirk.eddelbuettel.com/papers/useR2019_rcpp_tutorial.pdf" class="uri">https://dirk.eddelbuettel.com/papers/useR2019_rcpp_tutorial.pdf</a>)
<br>

-   Armadillo is a C++ linear algebra library.
-   The syntax is designed to be similar to Matlab.
-   Supports integer, floating point and complex numbers.
-   Employs delayed evaluation approach for compiling, allowing for
    combining several operations into one and doing away with temporary
    objects.
-   Due to speed and integration capabilities, appropriate for
    converting research code (e.g. in MATLAB) into production
    environments.

#### <br>RccpArmadillo examples <br>

<br>

Read <a href="http://arma.sourceforge.net/docs.html#part_classes" class="uri">http://arma.sourceforge.net/docs.html#part_classes </a>for class and methods. <br>

##### <br>**1. column sums**

``` cpp
#include <RcppArmadillo.h>
// [[Rcpp::depends(RcppArmadillo)]]
// [[Rcpp::export]]
arma::rowvec colSums(arma::mat M){ // default vec is alias for colvec
  size_t cols = M.n_cols;
  arma::rowvec res(cols);
  
  for(size_t i=0; i<cols; i++){
    res[i] = sum(M.col(i)); // column accessor is col(i)
  }
  
  return res;
}
```

``` r
colSums(diag(3))
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    1    1

##### <br> **2. get eigen values** 

Refer to <a href="http://arma.sourceforge.net/docs.html#part_decompdense" class="uri">http://arma.sourceforge.net/docs.html#part_decompdense </a>for more functions of vector/matrix

``` r
src = "arma::vec getEigenValues(arma::mat M){
  return arma::eig_sym(M);
}"
cppFunction(src, depends = "RcppArmadillo")

M = cbind(c(1,-1),c(-1,1))
getEigenValues(M)
```

    ##      [,1]
    ## [1,]    0
    ## [2,]    2

``` r
eigen(M)[["values"]]
```

    ## [1] 2 0

##### <br> **3. inner and outer products**

``` cpp
#include <RcppArmadillo.h>
// [[Rcpp::depends(RcppArmadillo)]]

// Outer product
//[[Rcpp::export]]
arma::mat outer_arma(const arma::colvec & x){ 
  // do not copy x (call by reference "&"), 
  // but prevent accidentally modifying x by declaring "const"
  arma::mat M = x * x.t(); // * does matrix multiplication
  return M;
}

// Inner product
//[[Rcpp::export]]
double inner_arma(const arma::colvec & x){
  double v = arma::as_scalar(x.t() * x); // transform vector with 1 element to a scalar
  return v;
}
//
```

``` r
outer_arma(1:3); inner_arma(1:3)
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    2    3
    ## [2,]    2    4    6
    ## [3,]    3    6    9
    
    ## [1] 14

##### <br> <b>Note: what are `const` and `&` for?</b> <br>

 In C++, you may assign only a reference and avoid copying by using `&`. Read 

https://www.geeksforgeeks.org/references-in-c/

https://stackoverflow.com/questions/44795308/why-put-const-in-c

for more detail.

``` cpp
#include <Rcpp.h>
using namespace std;
//[[Rcpp::export]]
void test(){
  int x = 10;
  int y = x; 
  cout << "1) int y = x all 10" << endl;
  x = 15;
  cout << "x changed to 15 " << "value of y is " << y << endl;
  y = 20;
  cout << "y changed to 20 " << "value of x is " << x << endl;
  
  cout  << endl;
  int a = 10;
  int& b = a;
  cout << "2) int& b = a all 10" << endl;
  a = 15;
  cout << "a changed to 15 " << "value of b is " << b << endl;
  b = 20;
  cout << "b changed to 20 " << "value of a is " << a << endl;
}
```

``` r
test()
```

<br> Note that assigning with `&` is to make another alias for the same value with the same address. So you might accidentally modify the original variable. To prevent this, we use `const`. **The combination of
`const` and `&` is preferable if your function takes in a huge data, and does not necessarily need copy or modification of the input.**

 

## 6. RcppArmadillo: FastLM example

 

Computing value of parameters $\hat{\beta}_p, \hat{\sigma}^2$ and their standard deviation **fast** was a motivation behind building
RcppArmadillo library.

``` cpp
#include <RcppArmadillo.h>
//[[Rcpp::depends(RcppArmadillo)]]
using namespace Rcpp;

//[[Rcpp::export]]
List fastLm(const arma::mat& X, const arma::colvec& y){ // call reference of design matrix X and target y
  int n = X.n_rows, k = X.n_cols;
  
  arma::colvec coef = arma::solve(X, y); // solve underdetermined linear system with OLS method
  arma::colvec resid = y - X * coef;
  
  double sig2 = arma::as_scalar(arma::trans(resid) * resid / (n-k));
  arma::colvec std_err = arma::sqrt(sig2 * arma::diagvec(arma::pinv(arma::trans(X)*X)));
  
  return List::create(Named("coefficients") = coef,
                      Named("stderr") = std_err,
                      Named("df.residual") = n-k);
}
```

``` r
n = 1e3; k = 1e1
X = runif(n*k); dim(X) = c(n,k)
beta = seq_len(k)
sigma = 1/2
y = X %*% beta + matrix(rnorm(n, mean=0, sd=sigma), nrow=n)

benchmark(fastLm(X, y))
```

    ##           test replications elapsed relative user.self sys.self user.child
    ## 1 fastLm(X, y)          100    0.03        1      0.03        0         NA
    ##   sys.child
    ## 1        NA

<br> A few points worth noted in this example:

1.  Since usually *X*, *y* are heavy, we used `const` and `&` combo as
    introduced above to 1) avoid copying huge data and 2) prevent
    unintentionally modifying *X*, *y*.
2.  `arma::solve` solves system of linear equation. If the system is
    underdetermined *n* \> *k* as in this case, `arma::solve` uses SVD
    based OLS method to get the best approximation to *y*. For more
    info, read a reply in
    <a href="https://stackoverflow.com/questions/50121897/approximate-solution-to-linear-system-in-armadillo-library" class="uri">https://stackoverflow.com/questions/50121897/approximate-solution-to-linear-system-in-armadillo-library</a>
3.  `arma::as_scalar` coerce a single element vector / matrix into
    scalar.
4.  `arma::diagvec(A)` extracts a main diagonal from a matrix *A* and
    returns as colvec
5.  `pinv(A)` stands for Moore-Penrose pseudo-inverse $A^T(AA^T)^{-1}$.
    https://en.wikipedia.org/wiki/Moore%E2%80%93Penrose_inverse
