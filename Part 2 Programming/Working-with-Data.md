# Data Types

All variables store values of a certain data type. The data type tells R
what variables are appropriate for what functions. For example, you
cannot multiply a character (also known as a string) by a numeric
(number). Here are the basic data types below:

-   Numeric: Any real number. R uses either integers for whole numbers
    or floating point numbers for decimals. Examples include “47”,
    “53.0”, “-23.2183”.
-   Characters: This datatype is used to store text data. In order to
    define a text as a string, it must be enclosed in either single
    (‘Hello’) or double quotations (“World”).
-   Logical: This datatype represents boolean values, which are TRUE or
    FALSE.
-   Complex: This datatype represents imaginary numbers. It is composed
    of two numbers, the real part and the imaginary part. Complex
    numbers are not often used, but are necessary in certain
    calculations. Examples include “9 + 3i”, “1.5 + 2i”, “-5 - 1.5i”.
-   Factors: This datatype is for categorical objects. They represent a
    group of possible values for a certain category. For example, a
    factor could be a set of colors like “Red”, “Yellow”, “Green” etc.
    while another category could be “Freshman”, “Sophomore”, “Junior”
    etc. Factors can be ordinal (order) or nominal (no order), which can
    be useful for data manipulation.

# Vectors and Data Frames

Vectors are a list of objects of the same data type. Similar to arrays
in other programming languages, they are indexed, so you can access the
elements in that list by order (note that vectors are 1 indexed unlike
in other programming languages which are 0 indexed)

Dataframes are similar to vectors, but instead they are 2d, similar to a
matrix. Unlike lists, the entirety of their values are not the same data
type - rather, only values in the same column are required to be the
same data type.

Each column in a dataframe has a name. For each column, each index
corresponding to a particular row or observation.

Dataframes can be manually created similar to variables. We can use the
head() function to preview them, as well as the view() function.

      name_data <- data.frame(
        Name = c("Alice", "Bob", "Charlie"),
        Age = c(25, 30, 35),
        Score = c(90, 85, 88)
      )
      
      head(name_data)

    ##      Name Age Score
    ## 1   Alice  25    90
    ## 2     Bob  30    85
    ## 3 Charlie  35    88

However, most dataframes you work with are usually imported from another
source, like a .csv or .xlsx file.

# Importing Data

There are two ways of importing data through a direct import or through
commands.

By clicking on file in the toolbar then import dataset, click on the
correct option corresponding to the file import. Then you can put in the
file url or location in order to import the dataset.

This creates a new dataframe in the environment you can work on. This
dataframe can be accessed in scripts by simply calling its name.

You can also import data through commands through read\_excel, read\_csv
or other functions depending on the file extension.

      data <- read_excel("path/to/your/file.xlsx")

Note that the default directory is the directory your script is in.

# Manipulating data frame basics

Dataframes are essentially arrays of vectors. Values of a dataframe can
be accessed by column

    print(name_data$Name)

    ## [1] "Alice"   "Bob"     "Charlie"

    print(name_data[,"Name"])

    ## [1] "Alice"   "Bob"     "Charlie"

Values of a dataframe can also be accessed by row (R uses a 1 index)

    print(name_data[1, ])

    ##    Name Age Score
    ## 1 Alice  25    90

Rows can be subsetted as well, meaning that a new dataframe of selected
rows and columns will be returned

    print(name_data[1:2, c("Name", "Age")])

    ##    Name Age
    ## 1 Alice  25
    ## 2   Bob  30

# Dplyr

Dplyr is one of the most commonly used dataframe manipulation libraries,
which allows for filtering, selection and more useful functions. In
order to use all dplyr functions, you must use

    if (!requireNamespace("dplyr", quietly = TRUE)) { # checks if the package is in the namespace.
        install.packages("dplyr")
    }
    library("dplyr")

    ## Warning: package 'dplyr' was built under R version 4.4.1

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

# Filter

Dplyr has a filter() function that allows you to subset rows based off
of their values. The first argument of filter is the name of your
dataframe, then following parameters are conditions the rows must pass.
For example, we can select all the students with a score of over 85 with

    filtered_frame <- filter(name_data, Score > 85)
    head(filtered_frame)

    ##      Name Age Score
    ## 1   Alice  25    90
    ## 2 Charlie  35    88

Filtering allows us to work with missing and incomplete values. Take the
NYC flights dataset.

    if (!requireNamespace("nycflights13", quietly = TRUE)) { # checks if the package is in the namespace.
        install.packages("nycflights13")
    }
    library("nycflights13")

    head(flights)

    ## # A tibble: 6 × 19
    ##    year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##   <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ## 1  2013     1     1      517            515         2      830            819
    ## 2  2013     1     1      533            529         4      850            830
    ## 3  2013     1     1      542            540         2      923            850
    ## 4  2013     1     1      544            545        -1     1004           1022
    ## 5  2013     1     1      554            600        -6      812            837
    ## 6  2013     1     1      554            558        -4      740            728
    ## # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
    ## #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    ## #   hour <dbl>, minute <dbl>, time_hour <dttm>

This dataset contains missing or incomplete values. We can remove the
rows with missing values like with functions below.

    head(
      filter(flights, if_any(everything(), is.na))
    )

    ## # A tibble: 6 × 19
    ##    year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##   <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ## 1  2013     1     1     1525           1530        -5     1934           1805
    ## 2  2013     1     1     1528           1459        29     2002           1647
    ## 3  2013     1     1     1740           1745        -5     2158           2020
    ## 4  2013     1     1     1807           1738        29     2251           2103
    ## 5  2013     1     1     1939           1840        59       29           2151
    ## 6  2013     1     1     1952           1930        22     2358           2207
    ## # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
    ## #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    ## #   hour <dbl>, minute <dbl>, time_hour <dttm>

R considers NA values to be values that exist, but their values are
unknown. Attempting to run most logical operators with NA values will
often return NA

    print(NA > 5) # returns NA
    print(NA == 10) # returns NA
    print(NA + 10) # returns NA
    print(NA * 2) # returns NA
    print(NA * 0) # returns NA


    print(NA == NA) # NA is simply a value R is not sure of. This means that it cannot confirm if one unknown value is equal to another unknown

    ## [1] NA
    ## [1] NA
    ## [1] NA
    ## [1] NA
    ## [1] NA
    ## [1] NA

Because we cannot use NA == NA, we need to use the function is.na(value)
in order to check if something is NA.

# Arrange

Arrange is the dplyr function that allows for changing the order of
rows. It takes in a dataframe as its first parameter, and multiple
columns in subsequent parameters. By default, it orders the rows in
descending order by first column, then second column and so on.

    head(
      arrange(flights, year, month, day)
    )

    ## # A tibble: 6 × 19
    ##    year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##   <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ## 1  2013     1     1      517            515         2      830            819
    ## 2  2013     1     1      533            529         4      850            830
    ## 3  2013     1     1      542            540         2      923            850
    ## 4  2013     1     1      544            545        -1     1004           1022
    ## 5  2013     1     1      554            600        -6      812            837
    ## 6  2013     1     1      554            558        -4      740            728
    ## # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
    ## #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    ## #   hour <dbl>, minute <dbl>, time_hour <dttm>

By default items are sorted in ascending order. You can use desc() in
order to make the order be descending instead.

    head(
      arrange(flights, year, month, desc(day))
    )

    ## # A tibble: 6 × 19
    ##    year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##   <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ## 1  2013     1    31        1           2100       181      124           2225
    ## 2  2013     1    31        4           2359         5      455            444
    ## 3  2013     1    31        7           2359         8      453            437
    ## 4  2013     1    31       12           2250        82      132              7
    ## 5  2013     1    31       26           2154       152      328             50
    ## 6  2013     1    31       34           2159       155      135           2315
    ## # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
    ## #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    ## #   hour <dbl>, minute <dbl>, time_hour <dttm>

# Select

Select is a dplyr function that allows you to create a dataframe with
only selected columns. It’s similar to previous methods of selecting
columns, but allows for reordering column names.

    head(
      select(flights, carrier, year, month, day)
    )

    ## # A tibble: 6 × 4
    ##   carrier  year month   day
    ##   <chr>   <int> <int> <int>
    ## 1 UA       2013     1     1
    ## 2 UA       2013     1     1
    ## 3 AA       2013     1     1
    ## 4 B6       2013     1     1
    ## 5 DL       2013     1     1
    ## 6 UA       2013     1     1

If you want to reorder the columns so that a few specific columns are in
front, you can use the everything() helper to get the remaining columns.

    head(
      select(flights, carrier, year, month, day, everything())
    )

    ## # A tibble: 6 × 19
    ##   carrier  year month   day dep_time sched_dep_time dep_delay arr_time
    ##   <chr>   <int> <int> <int>    <int>          <int>     <dbl>    <int>
    ## 1 UA       2013     1     1      517            515         2      830
    ## 2 UA       2013     1     1      533            529         4      850
    ## 3 AA       2013     1     1      542            540         2      923
    ## 4 B6       2013     1     1      544            545        -1     1004
    ## 5 DL       2013     1     1      554            600        -6      812
    ## 6 UA       2013     1     1      554            558        -4      740
    ## # ℹ 11 more variables: sched_arr_time <int>, arr_delay <dbl>, flight <int>,
    ## #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    ## #   hour <dbl>, minute <dbl>, time_hour <dttm>

# Mutate

Sometimes you will need to add new columns based off existing columns.
For example, you might want to add the average speed (km/h) of a plane.
We can do so with the mutate function

    head(
      mutate(flights,
        speed = distance / air_time * 60 # we multiply by 60 bc air_time is in minutes       
      )
    )

    ## # A tibble: 6 × 20
    ##    year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##   <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ## 1  2013     1     1      517            515         2      830            819
    ## 2  2013     1     1      533            529         4      850            830
    ## 3  2013     1     1      542            540         2      923            850
    ## 4  2013     1     1      544            545        -1     1004           1022
    ## 5  2013     1     1      554            600        -6      812            837
    ## 6  2013     1     1      554            558        -4      740            728
    ## # ℹ 12 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
    ## #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    ## #   hour <dbl>, minute <dbl>, time_hour <dttm>, speed <dbl>

This appends a new “speed” column to the end of our dataframe with
values equal to the formula we gave it. If we want to create a new
dataframe entirely, we can simply use transmute.

    head(
      transmute(flights,
        speed = distance / air_time * 60 # we multiply by 60 bc air_time is in minutes       
      )
    )

    ## # A tibble: 6 × 1
    ##   speed
    ##   <dbl>
    ## 1  370.
    ## 2  374.
    ## 3  408.
    ## 4  517.
    ## 5  394.
    ## 6  288.

Not only that, we can create new variables using values of columns we
are mutating.

    head(
      transmute(flights,
        gain = dep_delay - arr_delay,
        speed = distance / air_time * 60, # we multiply by 60 bc air_time is in minutes
        gain_speed = gain * speed,
      )
    )

    ## # A tibble: 6 × 3
    ##    gain speed gain_speed
    ##   <dbl> <dbl>      <dbl>
    ## 1    -9  370.     -3330.
    ## 2   -16  374.     -5988.
    ## 3   -31  408.    -12660.
    ## 4    17  517.      8784.
    ## 5    19  394.      7489.
    ## 6   -16  288.     -4602.

We are not only limited to the basic arithmetic functions. We can use
any function in the environment when mutating dataframes.

# Group by

Group by allows you to create a group rows in an existing dataframe.
Basically, functions are then applied on each group of rows as if they
were their own dataframe until the dataframe is ungrouped.

Lets say you wanted to find the mean flight delay.

    summarise(flights, delay = mean(dep_delay, na.rm = TRUE))

    ## # A tibble: 1 × 1
    ##   delay
    ##   <dbl>
    ## 1  12.6

This returns the mean flight delay for all flights. However, what if we
wanted the mean flight delay for everything on the same day? We would
need to use group\_by() to do so.

    by_day <- group_by(flights, year, month, day)
    summarise(by_day, delay = mean(dep_delay, na.rm = TRUE))

    ## `summarise()` has grouped output by 'year', 'month'. You can override using the
    ## `.groups` argument.

    ## # A tibble: 365 × 4
    ## # Groups:   year, month [12]
    ##     year month   day delay
    ##    <int> <int> <int> <dbl>
    ##  1  2013     1     1 11.5 
    ##  2  2013     1     2 13.9 
    ##  3  2013     1     3 11.0 
    ##  4  2013     1     4  8.95
    ##  5  2013     1     5  5.73
    ##  6  2013     1     6  7.15
    ##  7  2013     1     7  5.42
    ##  8  2013     1     8  2.55
    ##  9  2013     1     9  2.28
    ## 10  2013     1    10  2.84
    ## # ℹ 355 more rows

This returns the mean delay for each day of the year through grouped
data.

# Removing NA

Note that because NA pollutes most functions, we can opt to remove it.
All of dplyr’s transformation functions have a na.rm parameter which can
be set to TRUE in order to prevent them from running arithmetic on NA
values.

    summarise(flights, delay = mean(dep_delay, na.rm = TRUE))

    ## # A tibble: 1 × 1
    ##   delay
    ##   <dbl>
    ## 1  12.6

    summarise(flights, delay = mean(dep_delay, na.rm = FALSE))

    ## # A tibble: 1 × 1
    ##   delay
    ##   <dbl>
    ## 1    NA

# The Pipe Operator

In other notebooks using dplyr you will often see people use the %&gt;%,
like so.

    flights %>%
      select(carrier, year, month, day, everything()) %>%
      arrange(year, month, desc(day))

which is equivalent to

    flights <- select(flights,carrier,year,month,day,everything())
    flights <- arrange(flights, year, month, desc(day))

Basically, the pipe operator is an alternative to repeatedly typing in
the dataframe parameter over and over. Proponents of the pipe say that
coding this way is more readable - however, because it only available
the dplyr/tidyverse, I personally do not use it because the pipe does
not work for functions outside of those libraries and thus leads to
inconsistent coding patterns. Regardless of whether or not you will use
the pipe, it is still important to understand what it does in case
others do use it.
