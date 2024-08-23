    #Load in the dataset
    library(dplyr)

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

    library(readr)
    Olympic_Athlete_Bio <- read_csv("Olympic_Athlete_Bio.csv")

    ## Rows: 155861 Columns: 10

    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (8): name, sex, born, weight, country, country_noc, description, special...
    ## dbl (2): athlete_id, height
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    Olympic_Athlete_Bio <- Olympic_Athlete_Bio %>% mutate_at(c('weight'), as.double)

Calculate the BMI

    Olympic_Athlete_Bio$BMI <- Olympic_Athlete_Bio$weight / (Olympic_Athlete_Bio$height/100)^2

Perform the ANOVA Test Null Hypothesis: The mean BMI across the
countries are the same Alt Hypothesis: At least one mean BMI across the
countries are different

    athlete_bio_aov <- aov(Olympic_Athlete_Bio$BMI~factor(Olympic_Athlete_Bio$country_noc)) 
    summary(athlete_bio_aov)

    ##                                             Df Sum Sq Mean Sq F value Pr(>F)
    ## factor(Olympic_Athlete_Bio$country_noc)    227  26456  116.55    13.4 <2e-16
    ## Residuals                               103923 903595    8.69               
    ##                                            
    ## factor(Olympic_Athlete_Bio$country_noc) ***
    ## Residuals                                  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 51710 observations deleted due to missingness

Because we P value is less than 0.05, we reject the null hypothesis. We
accept that at least one mean BMI across the countries are different.
