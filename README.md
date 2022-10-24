# -Framingham-Heart-Study-Analysis

# Intruduction

The Framingham Study is a “population-based, observational cohort study” started in 1948 by a federal public health agency. The goal has been to investigate cardiovascular disease. This data is a small subset of the larger study. The full data is complex and includes genetic and imaging results as well as the demographics and clinical data included in the subset. The full data has been instrumental in furthering understanding of cardiac conditions and outcomes.

# Data Exploration and Munging

Our original dataframe had 11,627 records with 39 variables.First we had to set up the data in our R Studio.
Looking at the data initially, it appears that patients were followed for at least 3 periods, so many of the 11,000 + records were duplicated, and triplicated. Outcome data for each line was final outcome data and this was repeated for each period the patient was present so it was not unique for each visit.
Many of the data items were factor variables, so we converted those variables to factors. This included risk factors like sex, smoking history, education, prevalence of disease (diabetes, hypertension, CHD), use of medication, as well as outcome occurrences such as CHD events, strokes and death.


For the numerically coded values, we wanted to be able to interpret them with English text not using a key, so we created labels for all the factor data

Now that we had a data frame with data we could read, we wanted to understand the outcomes better. We knew that many of the records were repeat patients so we had to split them up into different periods. We separated the data frame into 3 different data frames by the period. We then dropped repeated columns that didn’t change over the evaluation period or weren’t measured in that period. The columns we dropped were:

sex (from data frames 2 and 3)
education (from data frames 2 and 3)
period labels (from all data frames)
time (from data from 1 as it was always 0)
HDL and LDL (from data frames 1 and 2) as these were only measured at the 3rd period
outcome data (from data frames 2 and 3) as these were the same for all 3 periods

We then relabeled the columns except for RANDID (patient identifier in order to eventually join) with the period the new data came from.
We created by using inner joins so it is just all the patients that presented for all 3 periods.

#Explore data analysis
We did some Bar Plots for Factor Variables Looking Based on ANYCHD Occuring.
Gender and current smoker shows some relation with CHD. 

Bar Plots for Factor Variables Looking Based on ANYCHD Occuring: 
we found the mean value of age of non-CHD and CHD occurred has significance difference (than other).

# The Model:
Running a regression model with many variables including irrelevant ones will lead to a needlessly complex model. Forward, backward and Stepwise regression are methods of selecting important variables fo fitting regression models with the choice of predictive variables, carried out by an automatic procedure.
We tested full-model(with all features),nothing model(with no fetaures), backward regression(based on full model), foward regression(based on nothing model), step-wise regression.

According to the forward, backward and step-wise regression results, all three regressions have shown that the 10 significant predictors are SEX, presence of angina(PREVAP), presence of Myocardial Infarction(PREVMI), total cholesterol (TOTCHOL), AGE, and systolic blood pressure (SYSBP), heartrate, glucose level,CURSMOKECurrently Smoker, BMI. We called the final regression stepwise.model after taking these predictors in the logistic regression.

We employed model regularization with Lasso ( Least Absolute Shrinkage and Selection Operator)to prevent over fitting. Lasso uses shrinkage. Shrinkage is where data values are shrunk towards a central point as the mean. The lasso procedure encourages simple, sparse models

Since the AIC score for all three models are similar (full.model=2748, stepwise.model.f=2738, and lasso.model=2770) but the stepwise model has the least number of predictor, we conclude that the stepwise regression model has the best fit among the three regression models.





