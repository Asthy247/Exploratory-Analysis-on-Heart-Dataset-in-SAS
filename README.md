# Exploratory-Analysis-on-Heart-Dataset-in-SAS

# Project Overview

This repository contains a self-contained SAS Studio script (heart_descriptives.sas) that demonstrates a robust data analysis workflow. It covers essential steps including initial data inspection, rule-based data cleaning, feature engineering (deriving new variables), generating comprehensive descriptive statistics, and creating informative visualizations. The project utilizes the SASHELP.HEART dataset, a readily available sample dataset in SAS, entirely within the temporary WORK library for ease of reproduction.
This project is ideal for showcasing proficiency in Base SAS programming, data manipulation, and the ability to transform raw data into actionable insights, suitable for a data analyst or data scientist portfolio.

# Dataset
**Name**: SASHELP.HEART

**Source**: Built-in SAS sample library.

**Description**: This dataset contains health-related information, often used in SAS examples. It includes variables such as AgeAtStart, Sex, Weight, Height, Cholesterol, Systolic and Diastolic blood pressure, and Smoking_Status. It provides a good foundation for exploring health trends and relationships.

# Objectives
The primary objectives of this project are:

**Safe Data Copying:** Copy the built-in SASHELP.HEART dataset to the temporary WORK library for safe modification.

**Initial Data Assessment:** Perform a preliminary scan for variable attributes and missing values.

**Rule-Based Data Cleaning & Filtering:** Apply business logic to filter out irrelevant observations (e.g., non-adults) and ensure data completeness for key health metrics.

**Feature Engineering**: Create new, insightful variables like Body Mass Index (BMI) and a categorized Blood Pressure (BP) status based on existing data.

**Descriptive Statistics:** Generate comprehensive summary statistics to understand the distribution, central tendency, and variability of key health metrics, both overall and segmented by demographic factors.

**Basic Visualizations:** Create simple plots to visually represent data distributions and relationships.

**Code Organization & Export:** Demonstrate clear, commented SAS code and export the cleaned data for potential further use.

# Code Walkthrough (heart_descriptives.sas)
**The SAS script is organized into logical sections:**


**1. Data Copying**
**PROC SQL**: Safely copies SASHELP.HEART to work.heart_raw, ensuring the original source data remains untouched.


**2. Initial Inspection**
**PROC CONTENTS:** Provides a detailed overview of work.heart_raw's structure, variable types, lengths, and labels.
<img width="1023" height="650" alt="image" src="https://github.com/user-attachments/assets/dc1f4bbe-1554-4e55-a777-afa9e3a29398" />
<img width="446" height="444" alt="image" src="https://github.com/user-attachments/assets/751afda9-be38-4fde-907a-7065b2931a43" />

**PROC FREQ with NLEVELS:** Scans all variables for missing values and counts unique levels, helping to identify data quality issues like unexpected values or high rates of missingness.
<img width="553" height="602" alt="image" src="https://github.com/user-attachments/assets/97752aff-92af-4cd9-94f8-fdd82ef9fe16" />


**3. Data Cleaning & Feature Engineering (DATA step)****
**Filtering: **The DATA step creates two new datasets: work.heart_clean (for records meeting criteria) and work.heart_excluded (for records that are filtered out).

**Adult Filter**: Excludes individuals under 18 years old from heart_clean.

**Completeness Rule****: Ensures records in heart_clean have either complete Height and Weight OR complete Systolic and Diastolic blood pressure readings. Records failing this rule are excluded.

**Derived Fields:**

**bmi:** Calculates Body Mass Index using Weight and Height (assuming Height is in inches for the * 703 conversion factor).

**bp_cat:** Categorizes blood pressure into "Normal", "Elevated", "Stage 1", and "Stage 2" based on standard systolic and diastolic thresholds.


**4. Quick Sanity Checks (After Cleaning)**
**PROC SQL:** Compares row counts across work.heart_clean, work.heart_excluded, and work.heart_raw to verify the impact of the filtering rules.

**PROC MEANS**: Generates essential descriptive statistics (N, NMiss, Mean, Std Dev, Min, P25, Median, P75, Max) for key numerical variables in work.heart_clean, segmented by Sex.

**PROC FREQ:** Displays the distribution of the newly created bp_cat variable.

**5. Visualizations (PROC SGPLOT)**
**BMI by Sex**: A VBOX plot (boxplot) visualizes the distribution of BMI across male and female participants, allowing for easy comparison of central tendency, spread, and potential outliers.

**Systolic vs Diastolic (colored by BP category)**: A SCATTER plot shows the relationship between systolic and diastolic blood pressure, with points colored by the derived bp_cat variable. Reference lines are added to indicate the thresholds for blood pressure categories, providing visual context to the categorizations.

# Key Findings & Actual Results**
Upon running this script, the following observations and results were obtained:

**Initial Inspection (work.heart_raw):**

The dataset contained 5209 observations and 17 variables.

**Missing values were identified:**

**Smoking_Status**: 36 missing observations.

**Weight:** 6 missing observations.

**Cholesterol**: 152 missing observations.

**Other variables like AgeAtStart, Height, Diastolic, Systolic, and MRW**** also had varying numbers of missing values.

**Sex and BP_Status** categories were clean. Smoking_Status had a blank category for missing values.

**Cleaning Impact (work.heart_clean vs. work.heart_raw):**

The filtering rules (Adult filter and completeness rule for vital signs) did not result in any rows being excluded from the work.heart_clean dataset. All 5209 original observations were retained, indicating they met the defined criteria.

The DATA step successfully imputed missing Smoking_Status values to **'UNKNOWN'** and standardized casing for categorical variables.

**Derived Fields:**

A new numerical variable, bmi, was successfully calculated.

A new character variable, bp_cat, was successfully created, categorizing blood pressure into "Normal", "Elevat" (Elevated), and "Stage" (combining Stage 1 and Stage 2 based on the data's distribution relative to the thresholds).

**Descriptive Statistics (work.heart_clean):**

**Overall Numerical Summaries:**

**AgeAtStart**: **Mean** ~44.07, **Std Dev** ~8.57 (N=5209).

**Weight**: **Mean** ~153.09, **Std Dev** ~28.92 (N=5203, reflecting 6 original missing values).

**Cholesterol**: **Mean** ~227.42, **Std Dev** ~44.94 (N=5057, reflecting 152 original missing values).

**bmi**: **Mean** ~25.59 (N=5203, as it depends on Weight/Height).

**Segmented by Sex:**

**Female (N=2873):** **Mean AgeAtStart** ~44.05, **Weight** ~141.39, **Height** ~62.57, **bmi** ~25.42, **Systolic** ~136.89, **Diastolic** ~84.65, **Cholesterol** ~228.54.

**Male (N=2336)**: **Mean AgeAtStart** ~44.09, **Weight** ~167.47, **Height** ~67.57, **bmi** ~25.78, **Systolic** ~136.94, **Diastolic** ~86.23, **Cholesterol** ~226.05.

**Observation**: Males generally show higher average Weight, Height, Systolic, and Diastolic blood pressure compared to females, while AgeAtStart, bmi, and Cholesterol averages are relatively similar between sexes.

**Distribution of Blood-Pressure Category (bp_cat):**

**Elevat**: 437

**Normal**: 799

**Stage:** 3973

**Observation**: The majority of individuals fall into the "Stage" blood pressure category, indicating a significant portion of the dataset represents individuals with elevated or high blood pressure according to the defined thresholds.


# Data Visualizations 
# BMI by Sex (VBOX plot - Boxplot)
<img width="752" height="568" alt="image" src="https://github.com/user-attachments/assets/ac521e24-9a9f-4132-b18a-841d2c1633ef" />
This plot visually compares the distribution of Body Mass Index (BMI) between "Female" and "Male" participants.

**Central Tendency**: The diamond shape within each box represents the mean BMI for that sex, while the horizontal line inside the box is the median. You can observe that the mean and median BMI are quite similar for both sexes, hovering around 25-26.

**Spread (Interquartile Range - IQR)**: The box itself represents the middle 50% of the data (from the 25th percentile to the 75th percentile). The boxes for both female and male BMI appear to have a similar spread.

**Outliers**: The individual circles extending beyond the "whiskers" (the lines extending from the box) indicate potential outliers. Both sexes show a number of outliers, particularly on the higher end of the BMI scale, suggesting some individuals with significantly higher BMI values.

**Overall:** The plot suggests that while there might be slight differences in the exact distribution, the overall BMI ranges and central tendencies are quite similar between males and females in this dataset, although there's a notable presence of higher BMI outliers in both groups.

# Systolic vs Diastolic (colored by BP category - SCATTER plot)

<img width="739" height="553" alt="image" src="https://github.com/user-attachments/assets/6e4f9498-a22b-40d2-9787-66aa3c94a395" />

This scatter plot visualizes the relationship between Systolic and Diastolic blood pressure readings for each individual.

**Color-Coding:** The points are colored based on the derived bp_cat variable (Blood Pressure category).

The cluster of green/teal circles in the lower-left quadrant represents individuals in the "**Normal**" blood pressure category.

The cluster of blue/purple circles (slightly to the right of normal) likely represents the "**Elevated**" category.

The large cluster of red circles (spreading across higher systolic and diastolic values) represents the "**Stage**" category (which combines Stage 1 and Stage 2 hypertension as per your code's definition).

**Reference Lines**: The dashed grey lines indicate the thresholds used to define the blood pressure categories (e.g., 120, 130, 140 for Systolic on the X-axis, and 80, 90 for Diastolic on the Y-axis). These lines provide visual context for how the bp_cat variable was derived.

**Observation**: The plot clearly shows the progression of blood pressure from normal to elevated to various stages of hypertension. It highlights that a significant portion of the dataset falls into the "Stage" (red) category, which aligns with the PROC FREQ output for bp_cat showing "Stage" as the most frequent category. This visualization effectively communicates the distribution of blood pressure levels within the dataset and how they relate to the defined clinical categories.

# Recommendations:
Based on the descriptive analysis of the SASHELP.HEART dataset, several actionable recommendations can be made:

Investigate "Stage" Blood Pressure Category: Given that the majority of individuals (3973 out of 5209) fall into the "Stage" blood pressure category, further investigation is warranted. This could involve:

Subgroup Analysis: Deep-diving into the characteristics (age, smoking status, cholesterol levels) of individuals in the "Stage" category to identify specific risk factors or commonalities.

Targeted Interventions: If this represents a real-world population, these findings highlight a significant public health concern, suggesting a need for targeted health programs or interventions aimed at managing and reducing high blood pressure.

Address Missing Data for Cholesterol and Weight/Height:

While the current analysis proceeded by keeping observations with some missing data, the presence of 152 missing Cholesterol values and 6 missing Weight/Height values (which impacts bmi calculation) suggests potential data quality issues.

Data Collection Improvement: Recommend reviewing data collection processes to minimize future missingness for these critical variables.

Imputation Strategies: For future, more advanced analyses (e.g., predictive modeling), consider implementing imputation techniques (e.g., mean imputation, regression imputation) for missing Cholesterol, Weight, and Height values to retain more observations and potentially improve model accuracy.

Explore BMI Outliers: The visualizations (boxplots) indicated the presence of outliers in BMI for both sexes.

Outlier Analysis: Conduct a deeper analysis of these outlier individuals to understand if they represent data entry errors, extreme but valid cases, or specific subgroups with unique characteristics. This can inform whether these outliers should be treated specially in future modeling.

Consider Sex-Specific Health Programs: The descriptive statistics showed notable differences in average Weight, Height, Systolic, and Diastolic blood pressure between males and females.

Tailored Approaches: For health programs or clinical guidelines, these differences suggest that sex-specific considerations or tailored approaches might be beneficial rather than a one-size-fits-all strategy.

# Conclusion:
This "Heart Health Analysis" project successfully demonstrates a comprehensive data analysis workflow in SAS Studio. From safely copying raw data and performing initial inspections, the script effectively applies rule-based data cleaning and feature engineering to create a refined dataset. The subsequent descriptive statistics provide valuable insights into the demographic and health characteristics of the population, highlighting key trends and distributions, such as the high prevalence of individuals in the "Stage" blood pressure category. Finally, the inclusion of PROC SGPLOT visualizations effectively communicates complex relationships, such as BMI distribution by sex and the categorization of blood pressure.
This project serves as a robust portfolio piece, showcasing strong proficiency in Base SAS programming, meticulous data handling, analytical thinking, and the ability to translate data into meaningful observations, all crucial skills for a data analyst or data scientist role.
