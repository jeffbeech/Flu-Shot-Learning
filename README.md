# Project Title Here

README.md includes concise summary of project with all data science steps

README.md links to presentation and sources

README.md includes instructions for navigating the repository

For this project, the README.md file should contain:

- [ ] Overview
- [ ] Business and Data Understanding
- [ ] Explain your stakeholder audience and dataset choice here
- [ ] Modeling
- [ ] Evaluation
- [ ] Conclusion

![cdc](cdc.jpeg)
![flu vaccone](2018FLUVaccine.jpeg)
![flu vaccine bottle](acip-flu-recs800.jpeg)

## Overview
Although most people today associate the coronavirus with a pandemic, just over a decade ago another contagion raced around the globe. During the spring of 2009, a novel strain of influenza A emerged. This new type of H1N1 virus, commonly referred to as "swine flu," affected 60 million people worldwide and caused between 151,000-575,400 deaths in the first year of its appearance. Unlike the seasonal flu, H1N1 primarily sickened young people owing to the presence of familiar H1N1 antibodies in older individuals who had been exposed to a similar pathogen earlier in their lives. A vaccine to this novel H1N1 virus was developed that fall, but it was too late to be included in the seasonal flu vaccine for that year. This necessitated receiving 2 different flu vaccines for the flu season of 2009-10, especially for those considered highest at risk.

To help monitor H1N1 and seasonal vaccine rates, the Centers for Disease Control (CDC), in conjunction with the National Center for Immunization and Respiratory Diseases (NCIRD) and the National Center for Health Statistics (NCHS), conducted a phone survey throughout the United States from September 2009 through June 2010. The target population was anyone living in the country who was at least 6 months old. In addition to seeing if the respondent had received the seasonal flu and/or H1N1 vaccine, the survey also gathered data on respondents' views about vaccine safety and effectiveness, precautionary measures they took to avoid getting the flu, their level of concern about becoming sick from the flu, and basic demographic information. Over 70,000 household interviews were conducted, 20% of those being child interviews. These responses comprise the National 2009 H1N1 Flu Survey (NHFS), the dataset upon which this study is based.

Owing to the current climate of vaccine hesitancy in regards to the Covid-19 vaccination, the National Institutes of Health's Office of Communications and Public Liaison (OCPL) and Office of Legislative Policy and Analysis (OLPA) is searching for ways to increase vaccination rates for the coronavirus among the American population. These organizations have contracted us, 3 Sweaters, a data science consulting company, to study the NHFS in order to better understand why people did or did not receive the flu/H1N1 vaccine. With this information, we will provide recommendations to inform public health outreach and policy with the goal of increasing vaccination rates against Covid-19.

Talk about EDA
- more people got the seasonal flu vaccine than H1N1
- 

Talk about Data Cleaning Steps
- Most columns filled in missing values with the median response, except for the columns that had a high (how high?) number of missing values: health insurance, employment industry, and employement occupation. Since missing values here likely indicated that the respondent was not currently in the work force (student, retired, unemployed), we transformed these missing values into an unknown category.
- Scaling
- Label Encoding/OHE
- 

## Modeling

## Evaluation

## Conclusions
Recommendations:
1. ?
2. ?
3. ?

possible ideas for next steps:
- examine data geographically to see if different regions have different factors for getting/not getting vaccinated
- get more recent data to see if/how vaccine attitudes have changed recently
- focus on acquiring more data from non-white groups; this dataset is highly imbalanced toward whites (79% of respondents)

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
