# Boosting Vaccination Rates

![flu vaccine bottle](https://github.com/snakeeyes021/flu-shot-learning/raw/main/pictures/2018FLUVaccine.jpeg)

## Overview and Business Understanding
During the spring of 2009, a new contagion raced around the world. This novel strain of influenza A (a new type of H1N1 virus) affected 60 million people worldwide and [caused between 151,000-575,400 deaths](https://www.cdc.gov/flu/pandemic-resources/2009-h1n1-pandemic.html) in the first year of its appearance. Unlike the seasonal flu, H1N1, commonly referred to as "swine flu," primarily sickened young people. Older individuals who had been exposed to a similar H1N1 variant earlier in their lives benefited from having immune systems primed with protective antibodies. A vaccine to this novel H1N1 virus was developed in the fall of 2009, but it was too late to be included in the seasonal flu vaccine for that year. This necessitated receiving 2 different flu vaccines for the flu season of 2009-10, especially for those considered highest at risk.

To help monitor H1N1 and seasonal vaccine rates, the Centers for Disease Control (CDC), in conjunction with the National Center for Immunization and Respiratory Diseases (NCIRD) and the National Center for Health Statistics (NCHS), conducted a phone survey throughout the United States from September 2009 through June 2010. The target population was anyone living in the country who was at least 6 months old. In addition to seeing if the respondent had received the seasonal flu and/or H1N1 vaccine, the survey also gathered data on respondents' views about vaccine safety and effectiveness, precautionary measures they took to avoid getting the flu, their level of concern about becoming sick from the flu, and basic demographic information. Over 70,000 household interviews were conducted, 20% of those being child interviews. These responses comprise the National 2009 H1N1 Flu Survey (NHFS), the dataset upon which this study is based. This survey is currently part of a data science competition and can be accessed at [DrivenData](https://www.drivendata.org/competitions/66/flu-shot-learning/page/210/).

Knowing that the next pandemic is a matter of "when" rather than "if," the National Institutes of Health's Office of Communications and Public Liaison (OCPL) and Office of Legislative Policy and Analysis (OLPA) are searching for ways to increase vaccination rates among the American population. These organizations have contracted our company, 3 Sweaters Consulting, a data science consulting firm, to study the NHFS in order to better understand why people did or did not receive the flu/H1N1 vaccine. Using insights gained from analyzing the NHFS, we will provide recommendations to the OCPL and OLPA regarding public health outreach and policy with the goal of increasing vaccination rates.

This project is a hypothetical presentation to the NIH, as commissioned by their Office of Communications and Public Liaison (OCPL) and Office of Legislative Policy and Analysis (OLPA).

## The Dataset
The version of the NHFS available at [DrivenData](https://www.drivendata.org/competitions/66/flu-shot-learning/page/210/) contains survey responses from 26,707 individuals. There are 2 target variables–whether or not someone received the H1N1 vaccine and whether or not someone received the seasonal influenza vaccine. This study will examine the probabilities of an individual getting these vaccines. Of the respondents, 14,272 did not receive the seasonal flu vaccine and 12,435 did. These amounts are roughly symmetrical while the number of people who did not receive the H1N1 vaccine far surpasses the number who did (21,033 to 5,674). This presents an obvious class imbalance, with those who received the H1N1 vaccine comprising about 20% of the respondents. However, this imbalance is not so great that we believe it necessitates artificially enlarging the minority group. We will, though, ensure that the train-test split contains equal proportions of this class through stratification.

![Comparison of Vaccination Status](https://user-images.githubusercontent.com/89176309/151572417-975323eb-5673-4ea8-a092-2ad59a47d4a6.png)

The data was split into training and test sets before any data cleaning and processing occurred in order to ensure there would be no data leakage. Each of the 35 features is categorical data. Most of the columns that have missing values have fewer than 1,000 missing data points. Those we replaced with the mode value of the column. A few features (health insurance, employment industry, and occupation) are missing about half their values, potentially due to the respondent not being in the work force at the time of the survey (student, retired, or unemployed). We converted these missing values into a new value of "unknown".

## Modeling

The models we used were appropriate for a categorical target (two targets, actually). Because we had both seasonal flu and H1N1 flu as target variables, the models had to run for each variable.  Tuned models used GridSearchCV to isolate the best hyperparameters. The models we used were:

- Logistic Regression
- Naive Bayes
- Random Forest
- Histogram-based gradient boosting
- SVC

Since we are running both logistic regression and tree-based models as well as running each model on both diseases targets, we set up our pipeline inside of a collection of functions (two main functions, and a few smaller auxiliary functions) that provide a number of quality of life improvements in training the models. The first function runs the data through the cleaning steps, which are branched (values are imputed and certain ordinal object columns are ordinally encoded, then for linear models, the entire dataset is one-hot encoded, but for tree-based models, only the object columns are one-hot encoded, while the numeric columns are min-max scaled). The function then trains the model on a user-chosen preprocessing branch and passes y predictions (and optionally, y probabilities) for both target variables to the next function, which calculates a number of scoring metrics (recall, precision, accuracy, and f1, as well as an AUC score if y probabilities are provided). These results and a confusion matrix are printed for each target, and the results, along with the fitted estimator, y predictions, etc., are stored in a model dictionary which is automatically pickled. 

(Note: the pickle files hosted on this repo are out of date, as they are too large to be hosted on github. The most current pickle files can be downloaded from [here](https://drive.google.com/drive/folders/1vVga_rfuCLoPhoDb3encokDiEy61_VYJ?usp=sharing)).

## Evaluation
To evaluate our models, we used the precision metric. This measures how often we are predicting that people will get vaccinated when they actually did not. We believe this is the most valuable metric as we'd rather spend extra resources on those who would get vaccinated without intervention than overlook those who did not get vaccinated but we predicted they did. For predicting whether or not someone would receive the H1N1 vaccination, our best model was the tuned random forest with a precision score of 81%. This means of the people we predicted as receiving the vaccine, 19% of those predictions were incorrect, and those people did not actually receive it. The accuracy of this model is 83%, meaning that 83% of our total predictions were correct. For predicting whether or not someone would receive the seasonal influenza vaccination, our best model was the tuned Support Vector Classifier. It has a precision score of 78%, meaning that 22% of our positive predictions (predictions of those who got the vaccine) were incorrect (they did not actually receive it). The accuracy for this model is 78%.

<p float="left">
  <img src="https://github.com/snakeeyes021/flu-shot-learning/raw/main/graphs/Tuned%20Random%20Forest%20-%20H1N1.png" width="400" />
  <img src="https://github.com/snakeeyes021/flu-shot-learning/raw/main/graphs/Tuned%20SVC%20-%20Seasonal%20Flu%20-%20updated.png" width="400" /> 
</p>


We calculated the permutation feature importance for each of our best models to find the top influential factors that our models used to predict whether someone would or would not get a vaccine, for both H1N1 and seasonal flu vaccines. For the top 4 variables, 3 were shared between the two models:
- Level of concern about getting sick without a vaccine
- Opinion on vaccine effectiveness
- If the vaccine was recommended by a doctor

Our H1N1 vaccine model also had health insurance as a top feature in permutation importance (#2) and the seasonal flu vaccine model included age 65+ in its top variables (#3).

We ran partial dependence plots on the top three variables for each model to see the relative strengths of each value of each feature with respect to the model's predictive power. See the relevant section in the notebook for a full explanation of this data.

![partial_dependence_rf](https://user-images.githubusercontent.com/26641674/151712822-354b5387-6ae4-4acb-9ba6-d3362340a00b.png)

![partial_dependence_sv](https://user-images.githubusercontent.com/26641674/151712827-556be311-52e2-4a4d-a5c2-d0033bcd7176.png)

When looking at the difference in vaccine rates between the seasonal flu and H1N1 vaccines, we also noted some groups who were relatively more likely to get the seasonal flu vaccine than the H1N1. The largest relative drop in percentage between those who got the seasonal flu and those who got the H1N1 vaccine are found in these groups:
- Unmarried individuals
- Black people
- People 65+
- Middle class (above poverty and below $75k)

Finally, we measured the success of our models to see our overall improvement over our baselines. Seen here are our H1N1 models.

![H1N1 Models](https://github.com/snakeeyes021/flu-shot-learning/raw/main/graphs/fixedH1N1_models.png)

## Recommendations and Next Steps
Based on the interpretations of our best models, we recommend the following to help the NIH increase vaccination rates:

1.) Emphasize to medical professionals the importance of consistently recommending vaccinations for their patients

2.) Conduct a public awareness campaign about the safety and effectiveness of vaccines

3.) Target outreach efforts to those groups who are more likely to get seasonal than the H1N1 (unmarried people, black people, those 65 and older, and middle class). These are groups we believe the NIH should target in the event of a pandemic as they may already be open to getting vaccinations but need more awareness about a novel contagion in order to receive an additional shot. More broadly, we also recommend targeting those groups unlikely to get the seasonal vaccines as a way to increase vaccine rates overall (18-34 year olds, Hispanic people, those living in poverty).

To better refine these suggestions, we suggest examining the data geographically to see if different parts of the country reveal predictors for getting or not getting vaccines. The respondent's region of the country was noted during the survey but anonymized to protect their identity. Being able to study this data through the prism of geography may reveal how to adapt outreach efforts based on region. Secondly, if feasible, we recommend supplementing this data with interviews by people who are more representative of the racial breakdown in the U.S. (this survey only has 8% black respondents and 6.5% Hispanic respondents). 

We believe these suggestions with help the NIH move forward with increasing vaccine acceptance among the American population. There will be another pandemic. Having a population that is aware of the effectiveness of vaccinations and willing to receive them is one of the most powerful ways to blunt the impact of the next virus that goes viral.


## Repository Structure
```


├── data
|   ├── comp_submissions
|      ├── my_submission1272022103630.csv
|      ├── my_submission1272022105147.csv
|      ├── my_submission_0.csv
|      ├── my_submission_1.csv
|      ├── my_submission_2.csv
|   ├── H1N1_Flu_Vaccines.csv
|   ├── model_scoring_dict.pickle
|   ├── models_dict.pickle
|   ├── submission_format.csv
|   ├── test_set_features.csv
|   ├── training_set_features.csv
|   ├── training_set_labels.csv
    
├── graphs 
|   ├── 151572417-975323eb-5673-4ea8-a092-2ad59a47d4a6.png
|   ├── SSNL_models.png
|   ├── Tuned Random Forest - H1N1.png
|   ├── Tuned SVC - Seasonal Flu - updated.png
|   ├── fixedH1N1_models.png
|   ├── image (10).png
|   ├── image (7).png
|   ├── image (8).png
|   ├── image (9).png
|   ├── partial_dependence_rf.png
|   ├── partial_dependence_sv.png

├── personal notebooks
|   ├── Naive Bayes.ipynb
|   ├── RandomForest_H1N1.ipynb
|   ├── Sally-EDA.ipynb
|   ├── jeff.ipynb
|   ├── jeff_analysis_notebook.ipynb
|   ├── matt notebook-instantiation bug fix.ipynb
|   ├── matt notebook.ipynb
|   ├── matt scratch.ipynb
    
├── pictures
|   ├── 2018FLUVaccine.jpeg
|   ├── acip-flu-recs800.jpeg
|   ├── cdc.jpeg

├── .gitignore

├── Boosting Vaccination Rates Presentation.pdf

├── Combined Notebook.ipynb

├── README.md
```
## For more information
Check out the full [Jupyter notebook](https://github.com/snakeeyes021/flu-shot-learning/blob/main/Combined%20Notebook.ipynb) and the [presentation](https://github.com/snakeeyes021/flu-shot-learning/raw/main/Boosting%20Vaccination%20Rates%20Presentation.pdf).
