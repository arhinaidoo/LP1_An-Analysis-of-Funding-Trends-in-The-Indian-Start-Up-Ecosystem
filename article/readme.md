# Article
# Title
An Analysis of Funding Trends in The Indian Start-Up Ecosystem

# Content
India is situated in the continent of Asia. It lies completely in the Northern hemisphere and Eastern hemisphere between latitudes 84′ N and 37°6'N and longitudes 68°7′ E and 97°25′ E. India is divided by Tropic of Cancer 23°30′ N in almost two equal parts (source). India is known to be a land of ancient civilizations. India's social, economic, and cultural configurations are the products of a long process of regional expansion (source). 
In recent years, lots of start-ups have emerged from India and there has been growing concerns to answer questions on India's start-up ecosystem.
Being trained as a Data Analyst, I was concerned and excited to give insights to some of the questions regarding the Indian start-up ecosystem. This articles walks you through the stages in the analysis process. 
The CRoss Industry Standard Process for Data Mining (CRISP-DM) is a process model that serves as the base for a data science process. It has six sequential phases:
Business understanding - What does the business need?
Data understanding - What data do we have / need? Is it clean?
Data preparation - How do we organize the data for modeling?
Modeling - What modeling techniques should we apply?
Evaluation - Which model best meets the business objectives?
Deployment - How do stakeholders access the results?

The CRIPS-DM model was used at the foundation to this analysis. 
Business Understanding
The Business Understanding phase focuses on understanding the objectives and requirements of the project, in this case the analysis on the Indian start-up ecosystem. 
Thde title for the project. the objectives or questions to be answered and the hypothesis to be tested were asked at this stage. 
The following questions were asked to guide the analysis process:
Does the sort of industry have an impact on funding success?
Can the success of obtaining finance from investors be impacted by location?
Which stage receives more investment from investors for start-ups?
Who makes the biggest investments among investors?
Can the startup's stage effect the amount of funding it receives from investors?

Data Understanding
The relevant data set that was to be used for the analysis were collected at this stage. The collected data for the analysis were from 2018, 2019, 2020, and 2021.The data was described, explored and their quality determined. 
The first task after collecting the data set was to setup and install the relevant packages or libraries for data handling and data visualization as they were the much-needed libraries in this project. 
Find the imported libraries below. 
import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
from scipy import stats
from summarytools import dfSummary
import warnings
warnings.filterwarnings("ignore")

sns.set_theme(style="whitegrid")

import os

# numpy and pandas will be used for data handling
# seaborn and matplotlib will be used for data visualization
The next step was to load the data set for the analysis. This is also shown below.
# For CSV, use pandas.read_csv, this helps you to load the CSV file or data and to hande the dataframe
data_2018 = pd.read_csv('startup_funding2018.csv')
data_2019 = pd.read_csv('startup_funding2019.csv')
data_2020 = pd.read_csv('startup_funding2020.csv')
data_2021 = pd.read_csv('startup_funding2021.csv')
The summarized information of the 21018 data set after loading it is seen below. 
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 526 entries, 0 to 525
Data columns (total 6 columns):
 #   Column         Non-Null Count  Dtype 
---  ------         --------------  ----- 
 0   Company Name   526 non-null    object
 1   Industry       526 non-null    object
 2   Round/Series   526 non-null    object
 3   Amount         526 non-null    object
 4   Location       526 non-null    object
 5   About Company  526 non-null    object
dtypes: object(6)
memory usage: 24.8+ KB
This was inconsistent with the three other data set as they had consistent column names, and the number of columns were almost the same except the 2020 data set which has an extra empty column. 
The column names in the 2018 data set were renamed to reflect this consistency. This is also shown below
# rename the column headings to be consistent with other data set
data_2018.rename(columns={'Company Name':'Company/Brand','Industry':'Sector','Round/Series':'Stage','Amount':'Amount($)','Location':'HeadQuarter','About Company':'What it does'}, inplace=True)
Key issues found in the data set are stated below:
The 2018 data set has some of the values in Dollars and other in rupees.
Some of the values in the amount column were given as undisclosed, - and some were transposed with other columns across the four data sets.
the 2018 data set did not have a Founded column, the other data set had of the records missing data in this column. 
Some of the sectors were similar but were wrongly spelt leading to multiple sectors, eg, E-Commerce, ecommerce, E-commerce etc.
There were multiple stage names even those they were close. 
Some had missing values for the HeadQuarter column.

The underlying assumptions and procedures in correcting the issues are stated below:
All missing values at the founded column were replaced with the years for which the data set was from. Example, replace missing values in the 2020 data set with 2020.
The missing values at the amount column were replace with the median value.
Multiple stage names were collated into one.
Multiple sector names including those with wrong spelling were corrected and merged into one. 
Missing values at the Founders column were replaced with unknown.
Missing values at the headQuarter column were replaced with Delhi on the assumption that it's an Indian start-up data hence should be based in Delhi, India.
The What it does, and Company/Brand were not really addressed since they had little effect on our questions and analysis. However. they gave insights in cleaning the data and placing certain record under the right sector since there were sufficient information at the what it does and brand name column to suggest the sector. 

Data Preparation 
After an extensive Exploratory Data Analysis (EDA) phase, the data set were concatenated as one and saved as a new comma separated (csv)file which was reloaded into the pandas data frame and subjected to analysis. The final data set was of shape (2854, 10). The extra column was rather removed.
Evaluation 
The analysis of the data revealed the following details. 
A correlation between Amount and Year FoundedTop 10 Sectors with most FundingFunding as received by all sectorsTop 10 locations with most fundingStages with the most fundingTop 10 InvestorsTrend of years founded and funding.In summary, the analysis indicated that the FinTech sector received more funding, and most of these start-ups were at the seed stage. Bengaluru had most of the start-ups with enough funding and the Sales Force Ventures and Dragoneer Investment group were the top investors. 
Please click on this here for a more interactive visualization from PowerBI
## Author
Stephen Arhin-Aidoo


## Link to draft article
https://medium.com/@stephen.aidoo/an-analysis-of-funding-trends-in-the-indian-start-up-ecosystem-e2e624ce40fd