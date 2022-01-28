# Boosting Vaccination Rates

*** add link to presentation

*** check structure of respitory

*** instructions for navigating the repository(?)

*** add graphs from best models

![flu vaccine bottle](2018FLUVaccine.jpeg)

## Overview and Business Understanding
During the spring of 2009, a new contagion raced around the world. This novel strain of influenza A (a new type of H1N1 virus) affected 60 million people worldwide and [caused between 151,000-575,400 deaths](https://www.cdc.gov/flu/pandemic-resources/2009-h1n1-pandemic.html) in the first year of its appearance. Unlike the seasonal flu, H1N1, commonly referred to as "swine flu," primarily sickened young people. Older individuals who had been exposed to a similar H1N1 variant earlier in their lives benefited from having immune systems primed with protective antibodies. A vaccine to this novel H1N1 virus was developed in the fall of 2009, but it was too late to be included in the seasonal flu vaccine for that year. This necessitated receiving 2 different flu vaccines for the flu season of 2009-10, especially for those considered highest at risk.

To help monitor H1N1 and seasonal vaccine rates, the Centers for Disease Control (CDC), in conjunction with the National Center for Immunization and Respiratory Diseases (NCIRD) and the National Center for Health Statistics (NCHS), conducted a phone survey throughout the United States from September 2009 through June 2010. The target population was anyone living in the country who was at least 6 months old. In addition to seeing if the respondent had received the seasonal flu and/or H1N1 vaccine, the survey also gathered data on respondents' views about vaccine safety and effectiveness, precautionary measures they took to avoid getting the flu, their level of concern about becoming sick from the flu, and basic demographic information. Over 70,000 household interviews were conducted, 20% of those being child interviews. These responses comprise the National 2009 H1N1 Flu Survey (NHFS), the dataset upon which this study is based. This survey is currently part of a data science competition and can be accessed at [DrivenData](https://www.drivendata.org/competitions/66/flu-shot-learning/page/210/).

Knowing that the next pandemic is a matter of "when" rather than "if," the National Institutes of Health's Office of Communications and Public Liaison (OCPL) and Office of Legislative Policy and Analysis (OLPA) are searching for ways to increase vaccination rates among the American population. These organizations have contracted our company, 3 Sweaters Consulting, a data science consulting firm, to study the NHFS in order to better understand why people did or did not receive the flu/H1N1 vaccine. Using insights gained from analyzing the NHFS, we will provide recommendations to the OCPL and OLPA regarding public health outreach and policy with the goal of increasing vaccination rates.

## The Dataset
The version of the NHFS available at [DrivenData](https://www.drivendata.org/competitions/66/flu-shot-learning/page/210/) contains survey responses from 26,707 individuals. There are 2 target variables–whether or not someone received the H1N1 vaccine and whether or not someone received the seasonal influenza vaccine. This study will examine the probabilities of an individual getting these vaccines. Of the respondents, 14,272 did not receive the seasonal flu vaccine and 12,435 did. These amounts are roughly symmetrical while the number of people who did not receive the H1N1 vaccine far surpasses the number who did (21,033 to 5,674). This presents an obvious class imbalance, with those who received the H1N1 vaccine comprising about 20% of the respondents. However, this imbalance is not so great that we believe it necessitates artificially enlarging the minority group. We will, though, ensure that the train-test split contains equal proportions of this class through stratification.

![Comparison of Vaccination Status](https://user-images.githubusercontent.com/89176309/151572417-975323eb-5673-4ea8-a092-2ad59a47d4a6.png)

The data was split into training and test sets before any data cleaning and processing occurred in order to ensure there would be no data leakage. Each of the 35 features is categorical data. Most of the columns that have missing values have fewer than 1,000 missing data points. Those we replaced with the median value of the column. A few features (health insurance, employment industry, and occupation) are missing about half their values, likely due to the respondent not being in the work force at the time of the survey (student, retired, or unemployed). We converted these missing values into a new value of "unknown".

Since we are running both logistic regression and tree-based models, we'll have different preprocessing needs. To accomplish this, we set up a functions for a pipeline that will impute missing values and one-hot encode every feature for linear models; for non-linear models, the function will encode certain ordinal categories (such as education level and age group) as numerical and then min-max scale all ordinal features. We then created a function that takes in a tuned estimator as well as a choice of a preprocessor ("linear" or "tree") and returns a number of scoring metrics (recall, precision, accuracy, f1, and AUC score (if y_score is provided)). It also outputs a confusion matrix. This function saves the model and its results in a scoring dictionary.

## Modeling

- Logistic Regression
- Naive Bayes
- Random Forest
- Histogram-based gradient boosting
- SVC

### Cleaning

Our modeling strategy began with a basic logistic regression to establish a baseline (and we additionally duplicated the competition's baseline model for reference). We performed a train-test split, and preprocessed the data by imputing the missing values with the mode, since most categories, including the numeric ones, are categorical (though for three columns where half the data was missing, we imputed this with 'unknown'). Then we one-hot encoded the non-numeric columns, and min-max scaled the numeric columns. Since we are examing both the seasonal and H1N1 flu strains, we are making seperate predictions for both models, and in some cases averaging the result. 

# Before we do the models...Refactor into a pipeline

Now that we've got a basic and working but still fairly rough model with our cleaning steps mostly sorted out, we'll build a pipeline. We'll also code some of our graphing and scoring steps as a function so we can easily spit out several metrics for each model.  We'll also first redo our train-test split with with stratify because the H1N1-vaccinated class is slightly imbalanced. It likely won't be a drastic improvement, but we may be able to squeeze a small amount of extra juice out of any models going forward.




## Evaluation
To evaluate our models, we used the precision metric. This measures how often we are predicting that people will get vaccinated when they actually did not. We believe this is the most valuable metric as we'd rather spend extra resources on those who would get vaccinated without intervention than overlook those who did not get vaccinated but we predicted they did. For predicting whether or not someone would receive the H1N1 vaccination, our best model was the tuned random forest with a precision score of 78%. This means we incorrectly predicted 22% of people as getting the vaccine who did not actually receive it. The accuracy of this model is 85%, meaning that 85% of our predictions were correct. For predicting whether or not someone would receive the seasonal influenza vaccination, our best model was the tuned Support Vector Classifier. It has a precision score of 77%, meaning that we incorrectly predicted 23% of people as getting the vaccine who did not actually receive it. The accuracy for this model is 78%.

<img width="879" alt="Screen Shot 2022-01-28 at 2 10 48 PM" src="https://user-images.githubusercontent.com/93277808/151615024-c3dae1af-a35a-44a1-8632-7e80dc94d2c8.png">

We calculated the permutation feature importance and found that the top 3 most influential factors in our model to predict whether someone would or would not get a vaccine, for both H1N1 and seasonal flu vaccines, were:
- Level of concern about getting sick without a vaccine
- Opinion on vaccine effectiveness
- If the vaccine was recommended by a doctor

As these partial dependence plots illustrate, each predictor has a positive relation with the criteria:

<img width="585" alt="Screen Shot 2022-01-28 at 2 15 20 PM" src="https://user-images.githubusercontent.com/93277808/151615356-fc0ab9ea-77a9-411e-a7d0-b5e28f4ff2ea.png">

Interestingly, the top demographic variables for whether someone received the H1N1 vaccine differed from those for the seasonal flu vaccine. This is not altogether surprising since the seasonal flu vaccine has been available for over 70 years. The H1N1 vaccine was developed in response to an emergent pandemic and people were likely not very familiar with the novel pathogen or the availability of its vaccine. Since our clients are interested in vaccine acceptance in the face of a new pandemic, looking at the demographic particulars for the H1N1 vaccine may be instructive. The demographics that had the largest influence on our H1N1 model were:
- 35-44 year olds
- those living in poverty
- people not married
Each of these predictors has a **negative** relation with the criteria, meaning that these groups of people were unlikely to get the H1N1 vaccine.

## Recommendations and Next Steps
Based on the interpretations of our best models, we recommend the following to help the NIH increase vaccination rates:

1.) Emphasize to medical professionals the importance of consistently recommending vaccinations for their patients

2.) Conduct a public awareness campaign about the safety and effectiveness of vaccines

3.) Target outreach efforts to those in the 35-44 year old age range, those living in poverty, and those who are not married

To better refine these suggestions, we suggest examining the data geographically to see if different parts of the country show reveal predictors for getting or not getting vaccines. The respondent's region of the country was noted during the survey but anonymized to protect their identity. Being able to study this data through the prism of geography may reveal how to adapt outreach efforts based on region. Secondly, if feasible, we recommend supplementing this data with interviews by people who are more representative of the racial breakdown in the U.S. (this survey only has 8% black respondents and 6.5% Hispanic respondents). 

We believe these suggestions with help the NIH move forward with increasing vaccine acceptance among the American population. There will be another pandemic. Having a population that is aware of the effectiveness of vaccinations and willing to receive them is one of the most powerful ways to blunt the next virus that goes viral.


### Repository Structure
```

├── data
|   ├── test_set_features.csv
|   ├── training_set_features.csv
|   ├── training_set_labels.csv

├── .gitignore

├── Combined Notebook.ipynb

├── README.md

├── Presentation.pdf
