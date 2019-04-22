# Mod2_Project2
# NYC Graduation Rates: 
## Are 3rd - 8th Grade Math Scores predictive?

## Project Overview

Typically, graduation outcomes are considered in the context of high school. However, I was interested to see if elementary and middle school math scores were helpful in predicting NYC high school graduation rates on a district level.

The data used for this project was sourced via NYC OpenData. In particular, 5 datasets  were used:
* <a href=https://data.cityofnewyork.us/Education/2005-2011-Graduation-Outcomes-School-Level-Gender/khqi-x3p3>2005 - 2011 Graduation Outcomes - School Level - - Gender</a>
* <a href=https://data.cityofnewyork.us/Education/2005-2011-Graduation-Outcomes-School-Level-Ethnici/6jad-5sav>2005 - 2011 Graduation Outcomes - School Level - - Ethnicity</a>
* <a href=https://data.cityofnewyork.us/Education/2005-2011-Graduation-Outcomes-School-Level-Classes/cma4-zi8m>2005 - 2011 Graduation Outcomes - School Level - Classes of - Total Cohort</a>
* <a href=https://data.cityofnewyork.us/Education/2006-2012-Math-Test-Results-District-Gender/qphc-zrtc>2006 - 2012 Math Test Results - District - Gender</a>
* <a href=https://data.cityofnewyork.us/Education/2006-2012-Math-Test-Results-District-Ethnicity/usap-qc7e>2006 - 2012 Math Test Results - District - Ethnicity</a>

In total, 50,431 data points were related to high school graduation outcomes by gender, ethniticy, and total cohort. With an additional 9,408 data points related to math test results for 3rd thru 8th grade students.  After the data was cleansed, aggregated and merged, 212 data points remained for analysis at a District level. 

<b><u> Key Questions For This Project</u></b>:
* Can math scores be used for predicting high school graduation outcomes?
* Which features are relevant when evaluating graduation outcomes?
* Which features have the most impact to the evaluation?
* Lastly, is it possible to predict NYC Graduation Rates at a district level?


## Key Variables

<b><u>Target Variable</u>:</b> Total Grads Pct of Cohort
* This variable reflects the total percentage of students whom graduated.
* The source data reflects this at the individual School and Cohort Level.
* This is also aggregated by Ethnicity or Gender - depending on data set.

<b><u>Feature Variables</u>:</b> Math Test Results 
* Pct Level 3 - Percentage of students who scored in the Level 3 range. (3 = Meeting Learning Standards)
* Pct Level 4 - Percentage of students who scored in the Level 4 range. (4 = Meeting Learning Standards with Distinction)
* Pct Level 3 & 4 - Percentage of students who scored in Level 3 & Level 4 range. (Level 3 and 4 combined)

## Data Collection and Analysis

The 5 data sources were imported via API from NYC OpenData and into individual Pandas DataFrames.  first cleaned by removing columns with all null values such as: ICO Number, and Cupping Procedure and Descriptors. Next, all non-critical columns were dropped, along with any rows containing NULL or Nan values. Afterwards, dataframes were merged by 'dbn', 'cohort_year', and 'cohort_category'.

The data types and formats of the data values in some columns were altered for better processing. The percentage column values were stripped of the '%' sign and converted to a float. Also, the rows which contained "All Grades" in the 'grade' column were removed to avoid duplication when aggregating the dataset. Here's the Nullity Matrix for the final dataframe:

<p align="center">
  <img src="https://user-images.githubusercontent.com/43836014/56518173-c3c52180-650c-11e9-8849-5121cc327692.png" title="MSNO Nullity Matrix of Final DataFrame">
</p>

<p align="center">
  <img src="https://user-images.githubusercontent.com/43836014/56527610-e90d5c00-651b-11e9-9394-50f0100831cf.png" title="Distribution Plot of Target Variable">
</p>

## Linear Regression Model Preparation and Testing

The following variables were used during linear regression model testing: total grads percentage of cohort, district, gender ethnicity, total regents percentage of cohort by gender, advanced regents percentage of cohort by gender, regents without advanced percentage of cohort by gender, total regents percentage of cohort by ethnicity, advanced regents percentage of cohort by ethnicity, regents without advanced percentage of cohort by ethnicity, mean scale score by gender, percentage level 3 by gender, percentage level 4 by gender, percentage level 3 and 4 by gender, mean scale score by ethnicity, percentage level 3 by ethnicity, percentage level 4 by ethnicity, percentage level 3 and 4 by ethnicity. These variables were selected based on their integrity and relationship to target variable.

Because the target variable is a percentage, all feature percentages are also expressed in percentage form. Thus they would need to be divided by 100 for decimial conversion.

Prior to testing, the correlation between target variable and features were analyzed using a correlation table, heatmap, and correlation scatter matrix. The following are a few key observations:

* Total Regents Percentage of Cohort by Gender: 0.8
* Total Regents Percentage of Cohort by Ethnicity: 0.75

Below you'll find the correlation heatmap where the black color range indicates a positive correlation and the red color range indicates a negative correlation. The darker the color, the higher the correlation in the pair. The lighter the color, the lower the correlation in the pair.

<p align="center">
  <img src="https://user-images.githubusercontent.com/43836014/56529716-3212df80-651f-11e9-91a7-02d68c577620.png" title="Correlation Heatmap">
</p>

The following Pair Plot shows potential linear relationship between a few features which will be explored further in the process:

<p align="center">
  <img src="https://user-images.githubusercontent.com/43836014/56533005-dba89f80-6524-11e9-828b-45d42f4792c4.png" title="Pair Plot">
</p>

An analysis of the Pair Plot led me to creating my initial linear regression model via the StatsModel package:

<p align="center">
  <img src="https://user-images.githubusercontent.com/43836014/56534241-435fea00-6527-11e9-84cf-455a41ad45ac.png" title="Initial MLR Model">
</p>

Key observations:
* 19 Attributes
* High R-squared value of 0.922
* High Adj. R-squared value of 0.914
* Various Features w/high P-values

Next, I removed 5 features with high P-values to create my second regression model as follows:

<p align="center">
  <img src="https://user-images.githubusercontent.com/43836014/56535105-00067b00-6529-11e9-9e7c-5326f284df6a.png" title="Second MLR Model">
</p>

Key observations:
* 14 Attributes
* Slightly lower R-squared value of 0.920 (-0.002)
* Slightly lower Adj. R-squared value of 0.913 (-0.001)
* Fewer Features w/high P-values

Lastly, I removed the remaining high P-value features to create my final regression model:

<p align="center">
  <img src="https://user-images.githubusercontent.com/43836014/56535541-e7e32b80-6529-11e9-87ed-15ebaf8e3467.png" title="Final MLR Model">
</p>

Key observations:
* 8 Attributes
* Significantly lower R-squared value of 0.247 (-0.673)
* Significantly lower Adj. R-squared value of 0.221 (-0.692)
* Increased Features w/high P-values

As a result, I decided to move forward with the second model since it had a higher R-squared value than the third model and fewer high P-value features than the first model. Analysis of the Residual and Q-Q Plots also supported use of the second model:

<p align="center">
  <img src="https://user-images.githubusercontent.com/43836014/56536320-c125f480-652b-11e9-82b6-b5ef5217ad37.png" title="Residual Plot">
</p>

<p align="center">
  <img src="https://user-images.githubusercontent.com/43836014/56536362-dc90ff80-652b-11e9-952f-e122658ed69c.png" title="Q-Q Plot">
</p>

## Findings
* pct_level_3_by_gender- had the highest negative impact
* pct_level_3_and_4_by_ethnicity had the second highest negative impact
* pct_level_3_and_4_by_gender- had the highest positive impact
* pct_level_4_by_ethnicity had the second highest positive impact 

In conclusion, my model showed Math Score features were the strongest predictors of Graduation Outcomes. For example, a 1 unit percentage increase in pct_level_3_by_gender would increase total_grads_pct_of_cohort by ~1.4%
