# Introduction

R is the primary language used in coding for RStudio. There are two
different types of file that support operations in R.

## Basic Operations

R supports basic math operations with the common ones listed below.

    # Addition
    2 + 3

    ## [1] 5

    # Subtraction
    5 - 2

    ## [1] 3

    # Multiplication
    4 * 3

    ## [1] 12

    # Division
    10 / 2

    ## [1] 5

    # Exponentiation
    2^3

    ## [1] 8

# Variables

Variables are used to store specific values to keep track of. Variables
can be initialized with either the &lt;- or -&gt; operator, such as

      count <- 49
      "Heldi" -> current_name

Variables are useful for calculations, especially for values that change
such as for example

    price <- 30
    items_sold <- 60
    revenue <- price * items_sold

Not only that, variables can be initialized to multiple times.

    total_sheep <- 50
    new_sheep <- 10
    total_sheep <- total_sheep + new_sheep # this reassigns total sheep to be its new value plus the old value

# Naming Conventions for Variables

Variables should consist of only alphanumeric characters and ’\_’
characters. The style of variable names depends on the lab and
researcher, but it’s important to be consistent. Here are some common
naming conventions

my\_variable\_name - snake\_case

My\_Variable\_Name - Title\_Case

MyVariableName - PascalCase

myVariableName - camelCase

Generally I prefer snake\_case for writing in R, but as long as you are
consistent with your naming conventions you are good to go.

# Data Types

Variables hold specific data types. Every value a variable hold has its
own data type. Unlike in other programming languages, variables can hold
any value. We’ll learn more about data types in the following pages

# Functions

Functions are reusable blocks of code that allow you to do the same
actions repeatedly. For example, the sqrt() function returns the square
root of the number.

Functions have a list of named arguments and default arguments.
Arguments, which are basically your input values, need to be of a
specific data type to work. Lets take the round function for example

    print(round(3.145)) # this prints "3"

    ## [1] 3

However, the round function actually has two parameters, “x”, which is
our numeric vector (basically a number) and “digits”. By default, digits
= 0, which means it rounds to the 0 digits after the decimal point. We
can put in the parameter either in order or by name.

    print(round(3.145, 3)) # this prints "3.145"

    ## [1] 3.145

    print(round(3.145, digits=2)) # this prints "3.14"

    ## [1] 3.14

    print(round(x=3.145, digits=1)) # this prints "3.1"

    ## [1] 3.1

    print(round(digits=1, x=3.145)) # this also prints "3.1"

    ## [1] 3.1

We can also define our own functions. We can define a function by
assigning a value to a variable, like so:

    custom_round <- function(x, digits = 0) {
      # Scale X up.
      factor <- 10^digits
      scaled_x <- x * factor
      
      # generates two possible values
      floorX <- floor(scaled_x)
      ceilX <- ceiling(scaled_x)
      
      result <- -1 # initializes result
      #finds which value is closer to X, and picks that one.
      
      if (abs(scaled_x - floorX) <= abs(scaled_x - ceilX )) { # we replicate R's implementation by rounding down when it is 0.5
        result <- floorX
      }
      else {
        result <- ceilX
      }
      
      # Scale back to the original number
      result <- result / factor
      
      return(result)
    }

    print(custom_round(3.145, 3)) # this prints "3.145"

    ## [1] 3.145

    print(custom_round(3.145, digits=2)) # this prints "3.14"

    ## [1] 3.14

    print(custom_round(x=3.145, digits=1)) # this prints "3.1"

    ## [1] 3.1

    print(custom_round(digits=1, x=3.145)) # this also prints "3.1"

    ## [1] 3.1

Note that functions are stored in a variable with their parameter names
as arguments. Certain functions require arguments, like how round always
requires a value in x while digits is a default argument because it
chooses 0 if not specified.

Functions can also have variables inside them, like resultX and ceilX.
However, these variables are limited in scope to that function, meaning
that the function and other commands outside of that function can affect
them. While functions can affect variables outside of the function or
its parameters, it is not recommended to do so.

# Library Imports

Unfortunately, writing functions takes time and energy. Even a simple
function like round required about 10 lines of code for a naive
implementation. Fortunately, R allows us to write premade functions and
export them so that other people can use them.

Libraries are essentially bundles of functions that can be imported into
projects. Generally libraries fulfil specific niches that not every
project needs. Some examples of common R libraries include Dplyr, which
is used for data manipulation ggplot, which is used for data
visualization (e.g graphs) tidyverse, a collection of packages designed
to provide easy out of the box functionality for your use.

Libraries can be installed by using the command

    install.packages("your_package_name")
    library("your_package_name")

This adds the package to the environment for you to run in the command
line or in scripts.

Note that when you install packages in a script, every time you run that
script it demands an environment restart in order to avoid duplicating
that existing package. Theres two ways of avoiding this Run your code
without the imports once you’ve imported the packages Use
requireNameSpace to see if it is already installed, like so

        if (!requireNamespace("your_package_name", quietly = TRUE)) { # checks if the package is in the namespace.
          install.packages("your_package_name")
        }
      library("your_package_name")
