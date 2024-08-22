Logistic regression is used when you want to predict either the presence
of the absence of a certain characteristic based on the values of the
predictor variables. It is similar to a linear regression, however the
binary logistic regression predicts if the dependent variable will be
either one value or another.

# Example

What characteristics lead to heart disease?

Import our dataset

    library(readr)
    heart_2022_no_nans <- read_csv("heart_2022_no_nans.csv")

    ## Rows: 246022 Columns: 40
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (34): State, Sex, GeneralHealth, LastCheckupTime, PhysicalActivities, Re...
    ## dbl  (6): PhysicalHealthDays, MentalHealthDays, SleepHours, HeightInMeters, ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    head(heart_2022_no_nans)

    ## # A tibble: 6 × 40
    ##   State  Sex   GeneralHealth PhysicalHealthDays MentalHealthDays LastCheckupTime
    ##   <chr>  <chr> <chr>                      <dbl>            <dbl> <chr>          
    ## 1 Alaba… Fema… Very good                      4                0 Within past ye…
    ## 2 Alaba… Male  Very good                      0                0 Within past ye…
    ## 3 Alaba… Male  Very good                      0                0 Within past ye…
    ## 4 Alaba… Fema… Fair                           5                0 Within past ye…
    ## 5 Alaba… Fema… Good                           3               15 Within past ye…
    ## 6 Alaba… Male  Good                           0                0 Within past ye…
    ## # ℹ 34 more variables: PhysicalActivities <chr>, SleepHours <dbl>,
    ## #   RemovedTeeth <chr>, HadHeartAttack <chr>, HadAngina <chr>, HadStroke <chr>,
    ## #   HadAsthma <chr>, HadSkinCancer <chr>, HadCOPD <chr>,
    ## #   HadDepressiveDisorder <chr>, HadKidneyDisease <chr>, HadArthritis <chr>,
    ## #   HadDiabetes <chr>, DeafOrHardOfHearing <chr>,
    ## #   BlindOrVisionDifficulty <chr>, DifficultyConcentrating <chr>,
    ## #   DifficultyWalking <chr>, DifficultyDressingBathing <chr>, …

# Clean our dataset

    library(dplyr)

    cleaned_heart <- heart_2022_no_nans

We cannot use categorical variables like Sex, AlcoholDrinkers or
SmokerStatus, so we convert them to factors in order to create dummy
variables. We set their orders so that 0 indicates the absence of the
factor, and the greatest level is the most extreme version of the factor

    cleaned_heart$SmokerStatus <- trimws(cleaned_heart$SmokerStatus)
    cleaned_heart$SmokerStatus <- factor(cleaned_heart$SmokerStatus,
                                            levels = c("Never smoked",
                                                       "Former smoker",
                                                       "Current smoker - now smokes some days",
                                                       "Current smoker - now smokes every day"
                                                       ))
    cleaned_heart$Sex <- factor(cleaned_heart$Sex,
                                  levels = c("Female", "Male"))

    cleaned_heart$AlcoholDrinkers <- trimws(cleaned_heart$AlcoholDrinkers)
    cleaned_heart$AlcoholDrinkers <- factor(cleaned_heart$AlcoholDrinkers,
                                           levels = c("No", "Yes"))

    ### Remaining cleanup code hidden, see .rmd for implementation

In order to avoid overfitting, we split our data set into a training
dataset. We will use the caret package to do so. Then we can run the
logistic model through glm. We use family = binomial() because our
dependent variable, HadHeartAttack is binary (yes or no)

    require(caret)


    index <- createDataPartition(cleaned_heart$HadHeartAttack, p = .2, list = FALSE) # splits the data into groups. 20% goes to training
    train <- cleaned_heart[index, ]
    test <- cleaned_heart[-index, ]

    logistic_model <- glm(HadHeartAttack ~ ., family = binomial(), train)

Basically, a logistic regression is an equation in the form of $$

b(x) =

$$

    summary(logistic_model)

    ## 
    ## Call:
    ## glm(formula = HadHeartAttack ~ ., family = binomial(), data = train)
    ## 
    ## Coefficients:
    ##                                                                             Estimate
    ## (Intercept)                                                                -7.214192
    ## StateAlaska                                                                 0.148652
    ## StateArizona                                                                0.021677
    ## StateArkansas                                                               0.137725
    ## StateCalifornia                                                             0.156412
    ## StateColorado                                                               0.383177
    ## StateConnecticut                                                            0.014500
    ## StateDelaware                                                               0.055512
    ## StateDistrict of Columbia                                                  -0.065362
    ## StateFlorida                                                               -0.019570
    ## StateGeorgia                                                                0.162594
    ## StateGuam                                                                  -0.250357
    ## StateHawaii                                                                 0.024567
    ## StateIdaho                                                                  0.083031
    ## StateIllinois                                                              -0.132237
    ## StateIndiana                                                                0.083885
    ## StateIowa                                                                  -0.067115
    ## StateKansas                                                                -0.124990
    ## StateKentucky                                                              -0.015356
    ## StateLouisiana                                                             -0.252503
    ## StateMaine                                                                  0.191376
    ## StateMaryland                                                               0.079814
    ## StateMassachusetts                                                          0.152220
    ## StateMichigan                                                              -0.005076
    ## StateMinnesota                                                             -0.025065
    ## StateMississippi                                                            0.020371
    ## StateMissouri                                                              -0.036066
    ## StateMontana                                                                0.194750
    ## StateNebraska                                                               0.265257
    ## StateNevada                                                                -0.195493
    ## StateNew Hampshire                                                          0.091513
    ## StateNew Jersey                                                            -0.152315
    ## StateNew Mexico                                                             0.083716
    ## StateNew York                                                              -0.214284
    ## StateNorth Carolina                                                         0.271661
    ## StateNorth Dakota                                                           0.191686
    ## StateOhio                                                                   0.073052
    ## StateOklahoma                                                               0.049770
    ## StateOregon                                                                -0.169634
    ## StatePennsylvania                                                          -0.376472
    ## StatePuerto Rico                                                           -0.045128
    ## StateRhode Island                                                           0.108801
    ## StateSouth Carolina                                                        -0.369362
    ## StateSouth Dakota                                                           0.092625
    ## StateTennessee                                                             -0.212784
    ## StateTexas                                                                  0.246025
    ## StateUtah                                                                  -0.138668
    ## StateVermont                                                                0.360987
    ## StateVirgin Islands                                                        -0.760726
    ## StateVirginia                                                               0.185611
    ## StateWashington                                                             0.039802
    ## StateWest Virginia                                                          0.364935
    ## StateWisconsin                                                             -0.180195
    ## StateWyoming                                                                0.388596
    ## SexMale                                                                     0.820940
    ## GeneralHealthFair                                                          -0.095355
    ## GeneralHealthGood                                                          -0.450288
    ## GeneralHealthVery good                                                     -0.760462
    ## GeneralHealthExcellent                                                     -1.020783
    ## PhysicalHealthDays                                                         -0.001395
    ## MentalHealthDays                                                            0.002967
    ## LastCheckupTimeWithin past 2 years (1 year but less than 2 years ago)      -0.254450
    ## LastCheckupTimeWithin past 5 years (2 years but less than 5 years ago)     -0.184470
    ## LastCheckupTime5 or more years ago                                         -0.447180
    ## PhysicalActivitiesYes                                                      -0.053256
    ## SleepHours                                                                 -0.013415
    ## RemovedTeeth1 to 5                                                          0.167155
    ## RemovedTeeth6 or more, but not all                                          0.280219
    ## RemovedTeethAll                                                             0.638310
    ## HadAnginaYes                                                                2.312607
    ## HadStrokeYes                                                                0.737399
    ## HadAsthmaYes                                                                0.130163
    ## HadSkinCancerYes                                                            0.006297
    ## HadCOPDYes                                                                 -0.093373
    ## HadDepressiveDisorderYes                                                   -0.058271
    ## HadKidneyDiseaseYes                                                         0.012672
    ## HadArthritisYes                                                             0.026055
    ## HadDiabetesYes                                                              0.328621
    ## DeafOrHardOfHearingYes                                                      0.021841
    ## BlindOrVisionDifficultyYes                                                  0.085792
    ## DifficultyConcentratingYes                                                  0.151220
    ## DifficultyWalkingYes                                                        0.002542
    ## DifficultyDressingBathingYes                                               -0.063229
    ## DifficultyErrandsYes                                                        0.138167
    ## SmokerStatusFormer smoker                                                   0.272938
    ## SmokerStatusCurrent smoker - now smokes some days                           0.185590
    ## SmokerStatusCurrent smoker - now smokes every day                           0.415763
    ## ECigaretteUsageNot at all (right now)                                      -0.029808
    ## ECigaretteUsageUse them some days                                          -0.189178
    ## ECigaretteUsageUse them every day                                          -0.046965
    ## ChestScanYes                                                                0.563855
    ## RaceEthnicityCategoryHispanic                                               0.198669
    ## RaceEthnicityCategoryMultiracial, Non-Hispanic                             -0.082047
    ## RaceEthnicityCategoryOther race only, Non-Hispanic                          0.351468
    ## RaceEthnicityCategoryWhite only, Non-Hispanic                               0.065174
    ## AgeCategoryAge 25 to 29                                                    -0.208417
    ## AgeCategoryAge 30 to 34                                                     0.437960
    ## AgeCategoryAge 35 to 39                                                     0.940537
    ## AgeCategoryAge 40 to 44                                                     0.977578
    ## AgeCategoryAge 45 to 49                                                     1.509304
    ## AgeCategoryAge 50 to 54                                                     1.508142
    ## AgeCategoryAge 55 to 59                                                     1.841354
    ## AgeCategoryAge 60 to 64                                                     1.872491
    ## AgeCategoryAge 65 to 69                                                     2.005174
    ## AgeCategoryAge 70 to 74                                                     2.078595
    ## AgeCategoryAge 75 to 79                                                     2.090126
    ## AgeCategoryAge 80 or older                                                  2.383784
    ## HeightInMeters                                                              0.683526
    ## WeightInKilograms                                                          -0.012046
    ## BMI                                                                         0.040025
    ## AlcoholDrinkersYes                                                         -0.132003
    ## HIVTestingYes                                                               0.143664
    ## FluVaxLast12Yes                                                            -0.185361
    ## PneumoVaxEverYes                                                            0.152805
    ## TetanusLast10TdapYes, received tetanus shot but not sure what type          0.108770
    ## TetanusLast10TdapYes, received tetanus shot, but not Tdap                   0.113427
    ## TetanusLast10TdapNo, did not receive any tetanus shot in the past 10 years  0.112519
    ## HighRiskLastYearYes                                                         0.217618
    ## CovidPosYes                                                                 0.045516
    ##                                                                            Std. Error
    ## (Intercept)                                                                  1.720572
    ## StateAlaska                                                                  0.322705
    ## StateArizona                                                                 0.295125
    ## StateArkansas                                                                0.313367
    ## StateCalifornia                                                              0.310396
    ## StateColorado                                                                0.307695
    ## StateConnecticut                                                             0.307792
    ## StateDelaware                                                                0.354443
    ## StateDistrict of Columbia                                                    0.417817
    ## StateFlorida                                                                 0.281711
    ## StateGeorgia                                                                 0.294760
    ## StateGuam                                                                    0.409742
    ## StateHawaii                                                                  0.308998
    ## StateIdaho                                                                   0.329348
    ## StateIllinois                                                                0.371231
    ## StateIndiana                                                                 0.291699
    ## StateIowa                                                                    0.300725
    ## StateKansas                                                                  0.298599
    ## StateKentucky                                                                0.338484
    ## StateLouisiana                                                               0.341556
    ## StateMaine                                                                   0.289670
    ## StateMaryland                                                                0.279106
    ## StateMassachusetts                                                           0.305152
    ## StateMichigan                                                                0.299762
    ## StateMinnesota                                                               0.289063
    ## StateMississippi                                                             0.342114
    ## StateMissouri                                                                0.310759
    ## StateMontana                                                                 0.310947
    ## StateNebraska                                                                0.292677
    ## StateNevada                                                                  0.394978
    ## StateNew Hampshire                                                           0.311587
    ## StateNew Jersey                                                              0.324249
    ## StateNew Mexico                                                              0.331214
    ## StateNew York                                                                0.284440
    ## StateNorth Carolina                                                          0.347381
    ## StateNorth Dakota                                                            0.342336
    ## StateOhio                                                                    0.279060
    ## StateOklahoma                                                                0.328073
    ## StateOregon                                                                  0.364341
    ## StatePennsylvania                                                            0.380129
    ## StatePuerto Rico                                                             0.335152
    ## StateRhode Island                                                            0.332547
    ## StateSouth Carolina                                                          0.303491
    ## StateSouth Dakota                                                            0.310781
    ## StateTennessee                                                               0.340944
    ## StateTexas                                                                   0.283496
    ## StateUtah                                                                    0.318368
    ## StateVermont                                                                 0.300114
    ## StateVirgin Islands                                                          0.674520
    ## StateVirginia                                                                0.291099
    ## StateWashington                                                              0.272641
    ## StateWest Virginia                                                           0.305291
    ## StateWisconsin                                                               0.297718
    ## StateWyoming                                                                 0.337332
    ## SexMale                                                                      0.070624
    ## GeneralHealthFair                                                            0.090995
    ## GeneralHealthGood                                                            0.101643
    ## GeneralHealthVery good                                                       0.113568
    ## GeneralHealthExcellent                                                       0.141180
    ## PhysicalHealthDays                                                           0.002922
    ## MentalHealthDays                                                             0.003271
    ## LastCheckupTimeWithin past 2 years (1 year but less than 2 years ago)        0.113251
    ## LastCheckupTimeWithin past 5 years (2 years but less than 5 years ago)       0.152417
    ## LastCheckupTime5 or more years ago                                           0.184890
    ## PhysicalActivitiesYes                                                        0.055407
    ## SleepHours                                                                   0.014525
    ## RemovedTeeth1 to 5                                                           0.060465
    ## RemovedTeeth6 or more, but not all                                           0.073044
    ## RemovedTeethAll                                                              0.083000
    ## HadAnginaYes                                                                 0.053549
    ## HadStrokeYes                                                                 0.073362
    ## HadAsthmaYes                                                                 0.066439
    ## HadSkinCancerYes                                                             0.071695
    ## HadCOPDYes                                                                   0.070082
    ## HadDepressiveDisorderYes                                                     0.068593
    ## HadKidneyDiseaseYes                                                          0.078875
    ## HadArthritisYes                                                              0.052568
    ## HadDiabetesYes                                                               0.055247
    ## DeafOrHardOfHearingYes                                                       0.065206
    ## BlindOrVisionDifficultyYes                                                   0.084705
    ## DifficultyConcentratingYes                                                   0.075820
    ## DifficultyWalkingYes                                                         0.065617
    ## DifficultyDressingBathingYes                                                 0.103089
    ## DifficultyErrandsYes                                                         0.085539
    ## SmokerStatusFormer smoker                                                    0.054784
    ## SmokerStatusCurrent smoker - now smokes some days                            0.138808
    ## SmokerStatusCurrent smoker - now smokes every day                            0.085972
    ## ECigaretteUsageNot at all (right now)                                        0.066043
    ## ECigaretteUsageUse them some days                                            0.189641
    ## ECigaretteUsageUse them every day                                            0.205398
    ## ChestScanYes                                                                 0.055760
    ## RaceEthnicityCategoryHispanic                                                0.140100
    ## RaceEthnicityCategoryMultiracial, Non-Hispanic                               0.206878
    ## RaceEthnicityCategoryOther race only, Non-Hispanic                           0.154264
    ## RaceEthnicityCategoryWhite only, Non-Hispanic                                0.099043
    ## AgeCategoryAge 25 to 29                                                      0.513186
    ## AgeCategoryAge 30 to 34                                                      0.424175
    ## AgeCategoryAge 35 to 39                                                      0.382375
    ## AgeCategoryAge 40 to 44                                                      0.375400
    ## AgeCategoryAge 45 to 49                                                      0.362140
    ## AgeCategoryAge 50 to 54                                                      0.357571
    ## AgeCategoryAge 55 to 59                                                      0.353212
    ## AgeCategoryAge 60 to 64                                                      0.351975
    ## AgeCategoryAge 65 to 69                                                      0.351960
    ## AgeCategoryAge 70 to 74                                                      0.353122
    ## AgeCategoryAge 75 to 79                                                      0.355697
    ## AgeCategoryAge 80 or older                                                   0.356230
    ## HeightInMeters                                                               0.970342
    ## WeightInKilograms                                                            0.008877
    ## BMI                                                                          0.025854
    ## AlcoholDrinkersYes                                                           0.051520
    ## HIVTestingYes                                                                0.055856
    ## FluVaxLast12Yes                                                              0.054195
    ## PneumoVaxEverYes                                                             0.058681
    ## TetanusLast10TdapYes, received tetanus shot but not sure what type           0.066469
    ## TetanusLast10TdapYes, received tetanus shot, but not Tdap                    0.093060
    ## TetanusLast10TdapNo, did not receive any tetanus shot in the past 10 years   0.068359
    ## HighRiskLastYearYes                                                          0.147143
    ## CovidPosYes                                                                  0.054765
    ##                                                                            z value
    ## (Intercept)                                                                 -4.193
    ## StateAlaska                                                                  0.461
    ## StateArizona                                                                 0.073
    ## StateArkansas                                                                0.439
    ## StateCalifornia                                                              0.504
    ## StateColorado                                                                1.245
    ## StateConnecticut                                                             0.047
    ## StateDelaware                                                                0.157
    ## StateDistrict of Columbia                                                   -0.156
    ## StateFlorida                                                                -0.069
    ## StateGeorgia                                                                 0.552
    ## StateGuam                                                                   -0.611
    ## StateHawaii                                                                  0.080
    ## StateIdaho                                                                   0.252
    ## StateIllinois                                                               -0.356
    ## StateIndiana                                                                 0.288
    ## StateIowa                                                                   -0.223
    ## StateKansas                                                                 -0.419
    ## StateKentucky                                                               -0.045
    ## StateLouisiana                                                              -0.739
    ## StateMaine                                                                   0.661
    ## StateMaryland                                                                0.286
    ## StateMassachusetts                                                           0.499
    ## StateMichigan                                                               -0.017
    ## StateMinnesota                                                              -0.087
    ## StateMississippi                                                             0.060
    ## StateMissouri                                                               -0.116
    ## StateMontana                                                                 0.626
    ## StateNebraska                                                                0.906
    ## StateNevada                                                                 -0.495
    ## StateNew Hampshire                                                           0.294
    ## StateNew Jersey                                                             -0.470
    ## StateNew Mexico                                                              0.253
    ## StateNew York                                                               -0.753
    ## StateNorth Carolina                                                          0.782
    ## StateNorth Dakota                                                            0.560
    ## StateOhio                                                                    0.262
    ## StateOklahoma                                                                0.152
    ## StateOregon                                                                 -0.466
    ## StatePennsylvania                                                           -0.990
    ## StatePuerto Rico                                                            -0.135
    ## StateRhode Island                                                            0.327
    ## StateSouth Carolina                                                         -1.217
    ## StateSouth Dakota                                                            0.298
    ## StateTennessee                                                              -0.624
    ## StateTexas                                                                   0.868
    ## StateUtah                                                                   -0.436
    ## StateVermont                                                                 1.203
    ## StateVirgin Islands                                                         -1.128
    ## StateVirginia                                                                0.638
    ## StateWashington                                                              0.146
    ## StateWest Virginia                                                           1.195
    ## StateWisconsin                                                              -0.605
    ## StateWyoming                                                                 1.152
    ## SexMale                                                                     11.624
    ## GeneralHealthFair                                                           -1.048
    ## GeneralHealthGood                                                           -4.430
    ## GeneralHealthVery good                                                      -6.696
    ## GeneralHealthExcellent                                                      -7.230
    ## PhysicalHealthDays                                                          -0.477
    ## MentalHealthDays                                                             0.907
    ## LastCheckupTimeWithin past 2 years (1 year but less than 2 years ago)       -2.247
    ## LastCheckupTimeWithin past 5 years (2 years but less than 5 years ago)      -1.210
    ## LastCheckupTime5 or more years ago                                          -2.419
    ## PhysicalActivitiesYes                                                       -0.961
    ## SleepHours                                                                  -0.924
    ## RemovedTeeth1 to 5                                                           2.764
    ## RemovedTeeth6 or more, but not all                                           3.836
    ## RemovedTeethAll                                                              7.690
    ## HadAnginaYes                                                                43.187
    ## HadStrokeYes                                                                10.052
    ## HadAsthmaYes                                                                 1.959
    ## HadSkinCancerYes                                                             0.088
    ## HadCOPDYes                                                                  -1.332
    ## HadDepressiveDisorderYes                                                    -0.850
    ## HadKidneyDiseaseYes                                                          0.161
    ## HadArthritisYes                                                              0.496
    ## HadDiabetesYes                                                               5.948
    ## DeafOrHardOfHearingYes                                                       0.335
    ## BlindOrVisionDifficultyYes                                                   1.013
    ## DifficultyConcentratingYes                                                   1.994
    ## DifficultyWalkingYes                                                         0.039
    ## DifficultyDressingBathingYes                                                -0.613
    ## DifficultyErrandsYes                                                         1.615
    ## SmokerStatusFormer smoker                                                    4.982
    ## SmokerStatusCurrent smoker - now smokes some days                            1.337
    ## SmokerStatusCurrent smoker - now smokes every day                            4.836
    ## ECigaretteUsageNot at all (right now)                                       -0.451
    ## ECigaretteUsageUse them some days                                           -0.998
    ## ECigaretteUsageUse them every day                                           -0.229
    ## ChestScanYes                                                                10.112
    ## RaceEthnicityCategoryHispanic                                                1.418
    ## RaceEthnicityCategoryMultiracial, Non-Hispanic                              -0.397
    ## RaceEthnicityCategoryOther race only, Non-Hispanic                           2.278
    ## RaceEthnicityCategoryWhite only, Non-Hispanic                                0.658
    ## AgeCategoryAge 25 to 29                                                     -0.406
    ## AgeCategoryAge 30 to 34                                                      1.032
    ## AgeCategoryAge 35 to 39                                                      2.460
    ## AgeCategoryAge 40 to 44                                                      2.604
    ## AgeCategoryAge 45 to 49                                                      4.168
    ## AgeCategoryAge 50 to 54                                                      4.218
    ## AgeCategoryAge 55 to 59                                                      5.213
    ## AgeCategoryAge 60 to 64                                                      5.320
    ## AgeCategoryAge 65 to 69                                                      5.697
    ## AgeCategoryAge 70 to 74                                                      5.886
    ## AgeCategoryAge 75 to 79                                                      5.876
    ## AgeCategoryAge 80 or older                                                   6.692
    ## HeightInMeters                                                               0.704
    ## WeightInKilograms                                                           -1.357
    ## BMI                                                                          1.548
    ## AlcoholDrinkersYes                                                          -2.562
    ## HIVTestingYes                                                                2.572
    ## FluVaxLast12Yes                                                             -3.420
    ## PneumoVaxEverYes                                                             2.604
    ## TetanusLast10TdapYes, received tetanus shot but not sure what type           1.636
    ## TetanusLast10TdapYes, received tetanus shot, but not Tdap                    1.219
    ## TetanusLast10TdapNo, did not receive any tetanus shot in the past 10 years   1.646
    ## HighRiskLastYearYes                                                          1.479
    ## CovidPosYes                                                                  0.831
    ##                                                                            Pr(>|z|)
    ## (Intercept)                                                                2.75e-05
    ## StateAlaska                                                                0.645054
    ## StateArizona                                                               0.941448
    ## StateArkansas                                                              0.660300
    ## StateCalifornia                                                            0.614324
    ## StateColorado                                                              0.213016
    ## StateConnecticut                                                           0.962427
    ## StateDelaware                                                              0.875547
    ## StateDistrict of Columbia                                                  0.875688
    ## StateFlorida                                                               0.944615
    ## StateGeorgia                                                               0.581213
    ## StateGuam                                                                  0.541192
    ## StateHawaii                                                                0.936630
    ## StateIdaho                                                                 0.800959
    ## StateIllinois                                                              0.721681
    ## StateIndiana                                                               0.773673
    ## StateIowa                                                                  0.823397
    ## StateKansas                                                                0.675518
    ## StateKentucky                                                              0.963814
    ## StateLouisiana                                                             0.459742
    ## StateMaine                                                                 0.508825
    ## StateMaryland                                                              0.774908
    ## StateMassachusetts                                                         0.617896
    ## StateMichigan                                                              0.986490
    ## StateMinnesota                                                             0.930902
    ## StateMississippi                                                           0.952519
    ## StateMissouri                                                              0.907607
    ## StateMontana                                                               0.531110
    ## StateNebraska                                                              0.364770
    ## StateNevada                                                                0.620637
    ## StateNew Hampshire                                                         0.768987
    ## StateNew Jersey                                                            0.638536
    ## StateNew Mexico                                                            0.800457
    ## StateNew York                                                              0.451239
    ## StateNorth Carolina                                                        0.434199
    ## StateNorth Dakota                                                          0.575525
    ## StateOhio                                                                  0.793492
    ## StateOklahoma                                                              0.879421
    ## StateOregon                                                                0.641507
    ## StatePennsylvania                                                          0.321989
    ## StatePuerto Rico                                                           0.892889
    ## StateRhode Island                                                          0.743535
    ## StateSouth Carolina                                                        0.223588
    ## StateSouth Dakota                                                          0.765673
    ## StateTennessee                                                             0.532561
    ## StateTexas                                                                 0.385490
    ## StateUtah                                                                  0.663157
    ## StateVermont                                                               0.229041
    ## StateVirgin Islands                                                        0.259403
    ## StateVirginia                                                              0.523719
    ## StateWashington                                                            0.883932
    ## StateWest Virginia                                                         0.231943
    ## StateWisconsin                                                             0.545010
    ## StateWyoming                                                               0.249334
    ## SexMale                                                                     < 2e-16
    ## GeneralHealthFair                                                          0.294677
    ## GeneralHealthGood                                                          9.42e-06
    ## GeneralHealthVery good                                                     2.14e-11
    ## GeneralHealthExcellent                                                     4.82e-13
    ## PhysicalHealthDays                                                         0.633042
    ## MentalHealthDays                                                           0.364354
    ## LastCheckupTimeWithin past 2 years (1 year but less than 2 years ago)      0.024655
    ## LastCheckupTimeWithin past 5 years (2 years but less than 5 years ago)     0.226165
    ## LastCheckupTime5 or more years ago                                         0.015579
    ## PhysicalActivitiesYes                                                      0.336459
    ## SleepHours                                                                 0.355722
    ## RemovedTeeth1 to 5                                                         0.005701
    ## RemovedTeeth6 or more, but not all                                         0.000125
    ## RemovedTeethAll                                                            1.47e-14
    ## HadAnginaYes                                                                < 2e-16
    ## HadStrokeYes                                                                < 2e-16
    ## HadAsthmaYes                                                               0.050096
    ## HadSkinCancerYes                                                           0.930015
    ## HadCOPDYes                                                                 0.182750
    ## HadDepressiveDisorderYes                                                   0.395590
    ## HadKidneyDiseaseYes                                                        0.872359
    ## HadArthritisYes                                                            0.620141
    ## HadDiabetesYes                                                             2.71e-09
    ## DeafOrHardOfHearingYes                                                     0.737662
    ## BlindOrVisionDifficultyYes                                                 0.311142
    ## DifficultyConcentratingYes                                                 0.046102
    ## DifficultyWalkingYes                                                       0.969093
    ## DifficultyDressingBathingYes                                               0.539649
    ## DifficultyErrandsYes                                                       0.106258
    ## SmokerStatusFormer smoker                                                  6.29e-07
    ## SmokerStatusCurrent smoker - now smokes some days                          0.181216
    ## SmokerStatusCurrent smoker - now smokes every day                          1.32e-06
    ## ECigaretteUsageNot at all (right now)                                      0.651745
    ## ECigaretteUsageUse them some days                                          0.318495
    ## ECigaretteUsageUse them every day                                          0.819136
    ## ChestScanYes                                                                < 2e-16
    ## RaceEthnicityCategoryHispanic                                              0.156175
    ## RaceEthnicityCategoryMultiracial, Non-Hispanic                             0.691667
    ## RaceEthnicityCategoryOther race only, Non-Hispanic                         0.022705
    ## RaceEthnicityCategoryWhite only, Non-Hispanic                              0.510513
    ## AgeCategoryAge 25 to 29                                                    0.684652
    ## AgeCategoryAge 30 to 34                                                    0.301839
    ## AgeCategoryAge 35 to 39                                                    0.013904
    ## AgeCategoryAge 40 to 44                                                    0.009212
    ## AgeCategoryAge 45 to 49                                                    3.08e-05
    ## AgeCategoryAge 50 to 54                                                    2.47e-05
    ## AgeCategoryAge 55 to 59                                                    1.86e-07
    ## AgeCategoryAge 60 to 64                                                    1.04e-07
    ## AgeCategoryAge 65 to 69                                                    1.22e-08
    ## AgeCategoryAge 70 to 74                                                    3.95e-09
    ## AgeCategoryAge 75 to 79                                                    4.20e-09
    ## AgeCategoryAge 80 or older                                                 2.21e-11
    ## HeightInMeters                                                             0.481173
    ## WeightInKilograms                                                          0.174796
    ## BMI                                                                        0.121592
    ## AlcoholDrinkersYes                                                         0.010402
    ## HIVTestingYes                                                              0.010110
    ## FluVaxLast12Yes                                                            0.000626
    ## PneumoVaxEverYes                                                           0.009214
    ## TetanusLast10TdapYes, received tetanus shot but not sure what type         0.101755
    ## TetanusLast10TdapYes, received tetanus shot, but not Tdap                  0.222902
    ## TetanusLast10TdapNo, did not receive any tetanus shot in the past 10 years 0.099763
    ## HighRiskLastYearYes                                                        0.139153
    ## CovidPosYes                                                                0.405904
    ##                                                                               
    ## (Intercept)                                                                ***
    ## StateAlaska                                                                   
    ## StateArizona                                                                  
    ## StateArkansas                                                                 
    ## StateCalifornia                                                               
    ## StateColorado                                                                 
    ## StateConnecticut                                                              
    ## StateDelaware                                                                 
    ## StateDistrict of Columbia                                                     
    ## StateFlorida                                                                  
    ## StateGeorgia                                                                  
    ## StateGuam                                                                     
    ## StateHawaii                                                                   
    ## StateIdaho                                                                    
    ## StateIllinois                                                                 
    ## StateIndiana                                                                  
    ## StateIowa                                                                     
    ## StateKansas                                                                   
    ## StateKentucky                                                                 
    ## StateLouisiana                                                                
    ## StateMaine                                                                    
    ## StateMaryland                                                                 
    ## StateMassachusetts                                                            
    ## StateMichigan                                                                 
    ## StateMinnesota                                                                
    ## StateMississippi                                                              
    ## StateMissouri                                                                 
    ## StateMontana                                                                  
    ## StateNebraska                                                                 
    ## StateNevada                                                                   
    ## StateNew Hampshire                                                            
    ## StateNew Jersey                                                               
    ## StateNew Mexico                                                               
    ## StateNew York                                                                 
    ## StateNorth Carolina                                                           
    ## StateNorth Dakota                                                             
    ## StateOhio                                                                     
    ## StateOklahoma                                                                 
    ## StateOregon                                                                   
    ## StatePennsylvania                                                             
    ## StatePuerto Rico                                                              
    ## StateRhode Island                                                             
    ## StateSouth Carolina                                                           
    ## StateSouth Dakota                                                             
    ## StateTennessee                                                                
    ## StateTexas                                                                    
    ## StateUtah                                                                     
    ## StateVermont                                                                  
    ## StateVirgin Islands                                                           
    ## StateVirginia                                                                 
    ## StateWashington                                                               
    ## StateWest Virginia                                                            
    ## StateWisconsin                                                                
    ## StateWyoming                                                                  
    ## SexMale                                                                    ***
    ## GeneralHealthFair                                                             
    ## GeneralHealthGood                                                          ***
    ## GeneralHealthVery good                                                     ***
    ## GeneralHealthExcellent                                                     ***
    ## PhysicalHealthDays                                                            
    ## MentalHealthDays                                                              
    ## LastCheckupTimeWithin past 2 years (1 year but less than 2 years ago)      *  
    ## LastCheckupTimeWithin past 5 years (2 years but less than 5 years ago)        
    ## LastCheckupTime5 or more years ago                                         *  
    ## PhysicalActivitiesYes                                                         
    ## SleepHours                                                                    
    ## RemovedTeeth1 to 5                                                         ** 
    ## RemovedTeeth6 or more, but not all                                         ***
    ## RemovedTeethAll                                                            ***
    ## HadAnginaYes                                                               ***
    ## HadStrokeYes                                                               ***
    ## HadAsthmaYes                                                               .  
    ## HadSkinCancerYes                                                              
    ## HadCOPDYes                                                                    
    ## HadDepressiveDisorderYes                                                      
    ## HadKidneyDiseaseYes                                                           
    ## HadArthritisYes                                                               
    ## HadDiabetesYes                                                             ***
    ## DeafOrHardOfHearingYes                                                        
    ## BlindOrVisionDifficultyYes                                                    
    ## DifficultyConcentratingYes                                                 *  
    ## DifficultyWalkingYes                                                          
    ## DifficultyDressingBathingYes                                                  
    ## DifficultyErrandsYes                                                          
    ## SmokerStatusFormer smoker                                                  ***
    ## SmokerStatusCurrent smoker - now smokes some days                             
    ## SmokerStatusCurrent smoker - now smokes every day                          ***
    ## ECigaretteUsageNot at all (right now)                                         
    ## ECigaretteUsageUse them some days                                             
    ## ECigaretteUsageUse them every day                                             
    ## ChestScanYes                                                               ***
    ## RaceEthnicityCategoryHispanic                                                 
    ## RaceEthnicityCategoryMultiracial, Non-Hispanic                                
    ## RaceEthnicityCategoryOther race only, Non-Hispanic                         *  
    ## RaceEthnicityCategoryWhite only, Non-Hispanic                                 
    ## AgeCategoryAge 25 to 29                                                       
    ## AgeCategoryAge 30 to 34                                                       
    ## AgeCategoryAge 35 to 39                                                    *  
    ## AgeCategoryAge 40 to 44                                                    ** 
    ## AgeCategoryAge 45 to 49                                                    ***
    ## AgeCategoryAge 50 to 54                                                    ***
    ## AgeCategoryAge 55 to 59                                                    ***
    ## AgeCategoryAge 60 to 64                                                    ***
    ## AgeCategoryAge 65 to 69                                                    ***
    ## AgeCategoryAge 70 to 74                                                    ***
    ## AgeCategoryAge 75 to 79                                                    ***
    ## AgeCategoryAge 80 or older                                                 ***
    ## HeightInMeters                                                                
    ## WeightInKilograms                                                             
    ## BMI                                                                           
    ## AlcoholDrinkersYes                                                         *  
    ## HIVTestingYes                                                              *  
    ## FluVaxLast12Yes                                                            ***
    ## PneumoVaxEverYes                                                           ** 
    ## TetanusLast10TdapYes, received tetanus shot but not sure what type            
    ## TetanusLast10TdapYes, received tetanus shot, but not Tdap                     
    ## TetanusLast10TdapNo, did not receive any tetanus shot in the past 10 years .  
    ## HighRiskLastYearYes                                                           
    ## CovidPosYes                                                                   
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 19773  on 46074  degrees of freedom
    ## Residual deviance: 13585  on 45956  degrees of freedom
    ##   (3130 observations deleted due to missingness)
    ## AIC: 13823
    ## 
    ## Number of Fisher Scoring iterations: 8

# Interpreting the output

Each of the variables have a p value. If the p values are below certain
values (usually 0.05), we declare them as significant. Consider
HadStrokeYes, which has a P value of less than 0.01. We can say there is
a significant correlation between having a stroke and having a heart
attack. Compare that to someone who received a Tetanus shot, having a p
value in between 0.7 and 0.9. It seems there is no significant
correlation between the getting a shot and getting a heart attack.

# Interpretting Accuracy

We evaluate the accuracy of our model by having it predict values in
both the test and training dataset. Then we use a classification table
in order find the differences between the actual values and the
prediction

    threshold <- 0.5 # Threshold is how confident we are when we want to declare the likelihood of a heart attack

    pred_train_prob <- predict(logistic_model, train, type = "response")

    train$pred_heartattack <- factor(
      ifelse(pred_train_prob >= threshold, "Yes", "No"),
      levels = c("Yes", "No")
    )
    # Generating the classification tables
    ctab_train <- confusionMatrix(train$HadHeartAttack, train$pred_heartattack)

    ## Warning in confusionMatrix.default(train$HadHeartAttack,
    ## train$pred_heartattack): Levels are not in the same order for reference and
    ## data. Refactoring data to match.

    pred_test_prob <- predict(logistic_model, test, type = "response")

    test$pred_heartattack <- factor(
      ifelse(pred_test_prob >= threshold, "Yes", "No"),
      levels = c("Yes", "No")
    )
    # Generating the classification tables
    ctab_test <- confusionMatrix(test$HadHeartAttack, test$pred_heartattack)

    ## Warning in confusionMatrix.default(test$HadHeartAttack, test$pred_heartattack):
    ## Levels are not in the same order for reference and data. Refactoring data to
    ## match.

    print("Confusion Matrix Train")

    ## [1] "Confusion Matrix Train"

    print(ctab_train)

    ## Confusion Matrix and Statistics
    ## 
    ##           Reference
    ## Prediction   Yes    No
    ##        Yes   609  1951
    ##        No    485 43030
    ##                                          
    ##                Accuracy : 0.9471         
    ##                  95% CI : (0.945, 0.9492)
    ##     No Information Rate : 0.9763         
    ##     P-Value [Acc > NIR] : 1              
    ##                                          
    ##                   Kappa : 0.3104         
    ##                                          
    ##  Mcnemar's Test P-Value : <2e-16         
    ##                                          
    ##             Sensitivity : 0.55667        
    ##             Specificity : 0.95663        
    ##          Pos Pred Value : 0.23789        
    ##          Neg Pred Value : 0.98885        
    ##              Prevalence : 0.02374        
    ##          Detection Rate : 0.01322        
    ##    Detection Prevalence : 0.05556        
    ##       Balanced Accuracy : 0.75665        
    ##                                          
    ##        'Positive' Class : Yes            
    ## 

    print("Confusion Matrix Test")

    ## [1] "Confusion Matrix Test"

    print(ctab_test)

    ## Confusion Matrix and Statistics
    ## 
    ##           Reference
    ## Prediction    Yes     No
    ##        Yes   2424   7783
    ##        No    1777 172459
    ##                                           
    ##                Accuracy : 0.9482          
    ##                  95% CI : (0.9471, 0.9492)
    ##     No Information Rate : 0.9772          
    ##     P-Value [Acc > NIR] : 1               
    ##                                           
    ##                   Kappa : 0.3144          
    ##                                           
    ##  Mcnemar's Test P-Value : <2e-16          
    ##                                           
    ##             Sensitivity : 0.57701         
    ##             Specificity : 0.95682         
    ##          Pos Pred Value : 0.23748         
    ##          Neg Pred Value : 0.98980         
    ##              Prevalence : 0.02278         
    ##          Detection Rate : 0.01314         
    ##    Detection Prevalence : 0.05534         
    ##       Balanced Accuracy : 0.76691         
    ##                                           
    ##        'Positive' Class : Yes             
    ## 

The classification tables we generate give us several important values.

-   Accuracy: This is the success rate of our model. Basically, accuracy
    is the number of correct predictions divided by the total number of
    predictions.

-   Sensitivity: This is the ‘true positive rate’. Basically, it is the
    proportion of correct true positives divided by the total number of
    predicted positives.

-   Specificity: This is the true negative rate for predictions. It is
    the number of true negatives divided by the total number of
    predicted negatives.

-   Pos Pred Value: This is the rate of successful positive predictions.
    It is calculated by the number of true predicted positives divided
    by the number of positives in the dataset.

-   Neg Pred Val: This is the rate of successful negative predictions.
    It is calculated by the number of true negatives divided by the
    number of negatives in the dataset.

Notice we also have two classification tables: one showing the
comparison between our training data and our test data. If the values
change too much from training to test data (e.g our model is much less
accurate for our test data than training data), it can be an indication
of overfitting, meaning certain variables have an overestimated effect.

Ideally, for each of these categories, we should try to make them as
high as possible. This tutorial does not cover data preprocessing, which
are techniques which can be used to restructure our data in order to
produce a higher quality model.

## Sources/further reading

Dataset:
<https://www.kaggle.com/datasets/kamilpytlak/personal-key-indicators-of-heart-disease>

<https://online.stat.psu.edu/stat462/node/207/>
<https://www.ibm.com/docs/en/spss-statistics/beta?topic=regression-binary-logistic>
<https://medium.com/analytics-vidhya/machine-learning-ii-logistic-regression-explained-data-pre-processing-hands-on-kaggle-728e6a9d4bbf>
<https://utsavdesai26.medium.com/demystifying-classification-evaluation-metrics-accuracy-precision-recall-and-more-613dc7cc44b2>
