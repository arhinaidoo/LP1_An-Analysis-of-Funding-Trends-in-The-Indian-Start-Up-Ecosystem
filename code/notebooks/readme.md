# Notebooks
<H1 style="text-align: center;font-size:30px ; font-family: 'Times New Roman'; color:#00008B"> Project Title</H1>
<p style="text-align: center;font-size:22px ;font-weight: bold ; font-family: 'Times New Roman'"> Analysis of Funding Trends in the Indian Startup Ecosystem</p>
<H1 style="text-align: center;font-size:30px ; font-family: 'Times New Roman'; color: #00008B"> Project Description</H1>
#### The goal of this initiative is to provide information to important players who are considering entering the Indian startup ecosystem.

#### In order to do this, I will examine important indicators of investment received by startups in India from 2018 to 2021. Management will make strategic business decisions using these information.

#### I created a hypothesis to test the dataset so that I might get a valid conclusion. In order to determine which year is more likely to attract investment from investors, the data were analyzed in terms of year founded and funding amount received.


<H1 style="text-align: center;font-size:30px ; font-family: 'Times New Roman'; color: #00008B"> Questions :</H1>
<ol type="1"style="font-size:17px;font-weight: bold;font-family: 'Times New Roman'"> 
    <li>Does the sort of industry have an impact on funding success?</li>
    <li>Can the success of obtaining finance from investors be impacted by location?</li>
    <li>Which stage receives more investment from investors for start-ups?</li>
    <li>Who makes the biggest investments among investors?</li>
    <li>Can the startup's stage effect the amount of funding it receives from investors?</li>
</ol>
<H1 style="text-align: center;font-size:30px ; font-family: 'Times New Roman'; color: #00008B"> Hypothesis :</H1>
#### I created the null and alternate hypotheses to concentrate on two groups: companies that are technology-biased and startups that are not technology-biased, in order to further evaluate and analyze the data. Hence, the following two hypotheses are presented:

#### `NULL`: The year of start up  does not influence the funding received. 
#### `ALTERNATE`: The year of start up influences the funding received.
<H1 style="text-align: center;font-size:30px ; font-family: 'Times New Roman'; color: #00008B"> Data Preparation :</H1>
## Importing all Relevant Libraries
import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
import scipy
from scipy import stats
from summarytools import dfSummary
import warnings
warnings.filterwarnings("ignore")

sns.set_theme(style="whitegrid")

import os

# numpy and pandas will be used for data handling
# seaborn and matplotlib will be used for data visualization
## Data Loading
# For CSV, use pandas.read_csv, this helps you to load the CSV file or data and to hande the dataframe
data_2018 = pd.read_csv('startup_funding2018.csv')
data_2019 = pd.read_csv('startup_funding2019.csv')
data_2020 = pd.read_csv('startup_funding2020.csv')
data_2021 = pd.read_csv('startup_funding2021.csv')
<H1 style="text-align: center;font-size:30px ; font-family: 'Times New Roman'; color: #00008B"> DATA CLEANING :</H1>

<p style="text-align: center;font-size:17px ; font-family: 'Times New Roman';">
    In this section, the following properties of the data loaded will be analysed:
</p>  
<ul style="font-size:17px ; font-family: 'Times New Roman';">
    <li> Data Types </li>
    <li> Missing Data </li>
    <li> Categorical data values </li>
    <li> Continous data values </li>
    <li> Duplicated data </li>
    <li> Standard rule base check, that is, checks relating to industry standards, example email, dates, websites, currencies etc</li> 
# 2018 Data Set
data_2018.info()
data_2018
### Renaming Columns to reflect consistency
# rename the column headings to be consistent with other data set
data_2018.rename(columns={'Company Name':'Company/Brand','Industry':'Sector','Round/Series':'Stage','Amount':'Amount($)','Location':'HeadQuarter','About Company':'What it does'}, inplace=True)
### Cleaning the Sector Column
# Cleaning the Sector column to get of multiple sectors represented as one. 
#This is done by first stipping and using slicing to select the first part.
data_2018['Sector'] = data_2018['Sector'].str.split(',').str[0] 
data_2018['Sector'].unique()                            
#rename similar sectors with wrong or different spelling to make it consistent
data_2018.replace({'Sector':{'E-Commerce Platforms':'E-Commerce','Autonomous Vehicles':'Automotive','eSports':'Sports',
                           'Funding Platform':'Crowdfunding','Crowdsourcing':'Crowdfunding','Last Mile Transportation':'Transportation',
                           'Delivery':'Logistics','Delivery Service':'Logistics','Children':'Child Care','Travel':'Transportation','Health Care':'Healthcare',
                           'Commercial Real Estate':'Real Estate','Cooking':'Catering','Fantasy Sports':'Sports','Consumer':'Consumer Goods',
                           'Consumer Lending':'Consumer Goods','Credit':'Credit Cards','Business Travel':'Transportation','Hospital':'Healthcare',
                           'Basketball':'Sports','Alternative Medicine':'Healthcare','Renewable Energy':'Energy','Clean Energy':'Energy','Music':'Music Streaming',
                           'Health Diagnostics':'Healthcare','Medical':'Healthcare','Enterprise Software':'ERP','Enterprise Resource Planning (ERP)':'ERP',
                           'Audio':'Music Streaming','Air Transportation':'Aerospace','Wellness':'Fitness','Beauty':'Cosmetics','Big Data':'Analytics',
                           'Food and Beverage':'Catering','Wealth Management':'Finance','Social Media':'Digital Media','Broadcasting':'Media and Entertainment',
                           'News':'Media and Entertainment','Packaging Services':'Logistics','Mobile':'Mobile Phones','Medical Device':'Healthcare',
                           'Continuing Education':'Education','Dental':'Healthcare','Android':'Software','Banking':'Financial Services','Training':'Education',
                           'Digital Marketing':'Advertising','Marketplace':'Advertising','Consumer Electronics':'Consumer Goods','Rental':'Facilities Support Services',
                           'Retail':'Business Development','3D Printing':'Creative Agency','Electric Vehicle':'Automotive','Industrial Automation':'Manufacturing',
                           'Battery':'Energy','Reading Apps':'Apps','B2B':'E-Commerce','Communities':'Environmental Consulting','Smart Cities':'Environmental Consulting',
                           'Health Insurance':'Insurance','Commercial':'Business Development','Digital Entertainment':'Digital Media',
                           'Cloud Infrastructure':'Cloud Computing','Marketing':'Advertising','Events':'Creative Agency','Food Delivery':'Catering',
                           'Artificial Intelligence':'AI','Business Intelligence':'ERP','Information Services':'IT','Information Technology':'IT',
                           'E-Learning':'Education','Tourism':'Hospitality','Industrial':'Manufacturing','Internet':'IT','Internet of Things':'IoT',
                           'Farming':'Agriculture','Food Processing':'Catering','Collaboration':'Human Resources','Computer':'IT','Career Planning':'Human Resources',
                           'Wedding':'Customer Service','Online Games':'Gaming','—':'Unknown','Brand Marketing':'Advertising'}}, inplace=True)
### Cleaning the Stage Column
data_2018['Stage'].unique()
data_2018.replace({'Stage':{'Pre-Seed':'Seed','Private Equity':'Seed','Venture - Series Unknown':'Seed',
                           'Debt Financing':'Seed','Post-IPO Debt':'Seed','Corporate Round':'Seed','Undisclosed':'Seed',
                           'Post-IPO Equity':'Seed','Non-equity Assistance':'Seed','Funding Round':'Seed','Debt':'Seed',
                            'Angel':'Seed','Secondary Market':'Seed','Equity':'Seed','Grant':'Seed','Unknown':'Seed'}},inplace=True)
data_2018['Stage'].unique()
data_2018.loc[data_2018['Stage']=='https://docs.google.com/spreadsheets/d/1x9ziNeaz6auNChIHnMI8U6kS7knTr3byy_YBGfQaoUA/edit#gid=1861303593']
Looking at the 'What it does' and 'Company/Brand', it is obvious the sector should be in the FinTech Sector, this needs to be corrected and the stage replaced with 'Unknwon' since there is no futher information to tell the stage
# correcting the name of the sector from transportation to FinTech based on the information above.
data_2018.iloc[178].replace({'Transportation':'FinTech'},inplace=True) 
# correcting the stage name to Unknown since there is no information to correct it to the right stage. 
data_2018.iloc[178].replace({'https://docs.google.com/spreadsheets/d/1x9ziNeaz6auNChIHnMI8U6kS7knTr3byy_YBGfQaoUA/edit#gid=1861303593':'Seed'},inplace=True) 
data_2018['Stage'].unique()
### Cleaning the Amount($) Column
Since some of the currencies are in ₹ (rupees) and others in $ (dollars), the rupees are changed to dollars based on the current exchange rate.

Duty is to select all rows where 'Amount' column is in rupees
# since some of the currencies are in ₹ (rupees) and others in $ (dollars), the rupees are changed to dollars based on the current exchange rate
# select all rows where 'Amount($)' column is in rupees

# this should be done before stripping off the currency symbol in order to identify the values and make the conversion 
rupees_index = data_2018.index[data_2018['Amount($)'].str.contains('₹')]
rupees_index 
# removing or stripping all special characters form the amount column
#data_2018["Amount"]=data_2018.Amount.apply(lambda x: str(x).replace("-",np.nan))
data_2018['Amount($)']=data_2018['Amount($)'].replace('—', np.nan)    # replacing '-' in the amount column with 0
data_2018['Amount($)']=data_2018['Amount($)'].apply(lambda x: str(x).replace("₹" ,""))   # replacing '₹' in the amount column with nothing
data_2018['Amount($)']=data_2018['Amount($)'].apply(lambda x: str(x).replace("$" ,""))   # replacing '$' in the amount column with nothing
data_2018['Amount($)']=data_2018['Amount($)'].apply(lambda x: str(x).replace("," ,""))   # replacing ',' in the amount column with nothing
# change the data type from a string to a float
data_2018['Amount($)']=pd.to_numeric(data_2018['Amount($)'], errors='coerce')  
#Convert the rows with rupees to dollars
#Multiply the rupees values in the amount column with 0.0146 which is the average 2018 exchange rate from rupees to dollars
# the is done on only the records with rupees whose indexes were stored to the rupees_index variable
data_2018.loc[rupees_index,['Amount($)']]=data_2018.loc[rupees_index,['Amount($)']].values*0.0146
data_2018.loc[rupees_index,["Amount($)"]].head()
# all nan values are to be filled with median on the 
data_2018['Amount($)']=data_2018['Amount($)'].fillna(data_2018['Amount($)'].median())
### Cleaning the HeadQuarter Column
#Strip the location data to only the city-area.
# use ',' as the delimeter to separate them and select preferred choice with slicing
data_2018['HeadQuarter']=data_2018['HeadQuarter'].str.split(',').str[0]     
data_2018['HeadQuarter'].unique()
# replacing wrongly spelt names and similar names entered differently
data_2018.replace({'HeadQuarter':{'Bangalore':'Bengaluru','Bangalore City':'Bengaluru','New Delhi':'Delhi','India':'Delhi'}}, inplace=True)
### Creating an Artificial `Founded` Column 
This column is being created to hold the year in which the startups were founded. 
This is on the operational assumption that empty values are to be replace with the year in which the data set is located, hence 2018 will be used for all the entries in this data set. 
# creating a founded column and assign 2018 as the value for all
data_2018['Founded']=2018
# checking for null values
data_2018.isnull().sum()
# checking for duplicate entries
data_2018.duplicated().value_counts()
 # remove the duplicate record
data_2018=data_2018.drop_duplicates(keep='first')  
# 2019 Data Set
data_2019.head()
data_2019.info()
### Cleaning the Founded Column
# convert the data type from float to a string
data_2019['Founded']=data_2019['Founded'].astype(str)   
data_2019['Founded']=data_2019.Founded.str.split('.').str[0]    # removing the decimal points in the year
data_2019['Founded'].unique()
# changing the data type to numeric
data_2019['Founded']=pd.to_numeric(data_2019['Founded'],errors='coerce', downcast='unsigned')
# all nan values are to be filled with unknown on the 
data_2019['Founded']=data_2019['Founded'].fillna(2019)
data_2019['Founded'].unique()
### Cleaning the HeadQuarter Column
data_2019['HeadQuarter'].unique()
# replacing wrongly spelt names and similar names entered differently
data_2019.replace({'HeadQuarter':{'Bangalore':'Bengaluru','New Delhi':'Delhi','nan':'Delhi'}}, inplace=True)
# all nan values are to be filled with Delhi on the assuption that this is an India Start Up data set
data_2019['HeadQuarter']=data_2019['HeadQuarter'].fillna('Delhi')
### Cleaning the Sector Column
data_2019['Sector'].unique()
#rename similar sectors with wrong or different spelling to make it consistent
data_2019.replace({'Sector':{'Ecommerce':'E-Commerce','Edtech':'EdTech','Interior design':'Home Decor','Virtual Banking':'Financial Services',
                           'AgriTech':'AgTech','E-commerce & AR':'E-Commerce','Fintech':'FinTech','Infratech':'IT','B2B Supply Chain':'E-Commerce',
                           'Marketing & Customer loyalty':'Customer Service','Automobile & Technology':'Automotive','Transport & Rentals':'Transportation',
                           'HR tech':'Human Resources','AI & Tech':'AI','E-commerce':'E-Commerce','Travel':'Transportation','Health Care':'Healthcare',
                           'Commercial Real Estate':'Real Estate','Cooking':'Catering','Fantasy Sports':'Sports','Consumer':'Consumer Goods',
                           'Healthtech':'Healthcare','E-Sports':'Sports','Mutual Funds':'Finance','Legal tech':'Legal','Accomodation':'Real Estate',
                           'Consumer Lending':'Consumer Goods','Credit':'Credit Cards','Business Travel':'Transportation','Hospital':'Healthcare',
                           'Automobile':'Automotive','Alternative Medicine':'Healthcare','Renewable Energy':'Energy','Clean Energy':'Energy','Music':'Music Streaming',
                           'Health Diagnostics':'Healthcare','Medical':'Healthcare','Enterprise Software':'ERP','Enterprise Resource Planning (ERP)':'ERP',
                           'Audio':'Music Streaming','Air Transportation':'Aerospace','Yoga & wellness':'Fitness','Beauty':'Cosmetics','Big Data':'Analytics',
                           'Food and Beverage':'Catering','Wealth Management':'Finance','Social Media':'Digital Media','Broadcasting':'Media and Entertainment',
                           'News':'Media and Entertainment','Packaging Services':'Logistics','Mobile':'Mobile Phones','Medical Device':'Healthcare',
                           'Continuing Education':'Education','Dental':'Healthcare','Android':'Software','Banking':'Financial Services','Training':'Education',
                           'Digital Marketing':'Advertising','Marketplace':'Advertising','Consumer Electronics':'Consumer Goods','Rental':'Facilities Support Services',
                           'Retail':'Business Development','3D Printing':'Creative Agency','Electric Vehicle':'Automotive','Industrial Automation':'Manufacturing',
                           'Battery':'Energy','Reading Apps':'Apps','B2B':'E-Commerce','Communities':'Environmental Consulting','Smart Cities':'Environmental Consulting',
                           'Health Insurance':'Insurance','Commercial':'Business Development','Digital Entertainment':'Digital Media','Food & tech':'Catering',
                           'Cloud Infrastructure':'Cloud Computing','Marketing':'Advertising','Events':'Creative Agency','Food tech':'Catering',
                           'Artificial Intelligence':'AI','Business Intelligence':'ERP','Information Services':'IT','Information Technology':'IT',
                           'E-Learning':'Education','Tourism':'Hospitality','Industrial':'Manufacturing','Internet':'IT','Internet of Things':'IoT',
                           'Jewellery':'Fashion','Food & Nutrition':'Healthcare','Robotics & AI':'AI', 'E-marketplace':'E-Commerce','Foodtech':'Catering',
                           'Insurance technology':'Insurance','Pharmaceutical':'Healthcare','Safety tech':'Cybersecurity',
                           'Technology':'IT','Farming':'Agriculture','Food Processing':'Catering','Collaboration':'Human Resources','Computer':'IT','Career Planning':'Human Resources',
                           'Wedding':'Customer Service','Games':'Gaming'}}, inplace=True)
data_2019['Sector'].unique()
# all nan values are to be filled with unknown on the 
data_2019['Sector']=data_2019['Sector'].fillna('Unknown')
### Cleaning the Amount($) Column
# stripping off special characters
#data_2019.replace({'Amount($)':{'$':'',',':''}},inplace=True)
data_2019["Amount($)"]=data_2019['Amount($)'].apply(lambda x: str(x).replace("$" ,"")) 
data_2019["Amount($)"]=data_2019['Amount($)'].apply(lambda x: str(x).replace("," ,""))
data_2019["Amount($)"]=data_2019['Amount($)'].replace("Undisclosed" ,np.nan)
# change the data type to a float
# The downcast parameter is used to set the data type to float
data_2019['Amount($)'] = pd.to_numeric(data_2019['Amount($)'], errors='coerce', downcast='float')                                                            
# all nan values are to be filled with median on the 
data_2019['Amount($)']=data_2019['Amount($)'].fillna(data_2019['Amount($)'].median())
### Cleaning the Stage Column
data_2019['Stage'].unique()
# replacing wrongly spelt and similar enteries to promote consistency
data_2019.replace({'Stage':{'Seed fund':'Seed','Post series A':'Series A',
                           'Pre series A':'Series A','Series B+':'Series B','Fresh funding':'Seed',
                           'Pre-series A':'Series A','Seed round':'Seed','Seed funding':'Seed','Debt':'Seed',
                            'Angel':'Seed','Secondary Market':'Seed','Equity':'Seed','Grant':'Seed'}},inplace=True)
data_2019['Stage'].unique()
# all nan values are to be filled with unknown on the 
data_2019['Stage']=data_2019['Stage'].fillna('Seed')
### Cleaning the Founders Column
# all nan values are to be filled with unknown on the 
data_2019['Founders']=data_2019['Founders'].fillna('Unknown')
data_2019.isnull().sum()
# checking for duplicate entries
data_2019.duplicated().value_counts()
# 2020 Data Set
data_2020.head()
data_2020.info()
### Cleaning the Founded Column
data_2020['Founded'].unique()
# all - values are to be filled with nan 
data_2020['Founded']=data_2020['Founded'].replace({'-':np.nan},inplace=True)
# changing the data type to numeric
data_2020['Founded']=pd.to_numeric(data_2020['Founded'],errors='coerce',downcast='signed')
# all nan values are to be filled with 2020
data_2020['Founded']=data_2020['Founded'].fillna(2020)
### Cleaning the HeadQuarter Column
data_2020['HeadQuarter'].unique()
#strip and select only the City name, which is the first part and ignore the country 
data_2020['HeadQuarter']=data_2020['HeadQuarter'].str.split(',').str[0] 
# replacing wrongly spelt names and similar names entered differently
data_2020.replace({'HeadQuarter':{'Bangalore':'Bengaluru','New Delhi':'Delhi','nan':'Delhi','Bangaldesh':'Bangladesh',
                                  'Banglore':'Bengaluru'}}, inplace=True)
# all nan values are to be filled with Delhi on the assuption that this is an India Start Up data set
data_2020['HeadQuarter']=data_2020['HeadQuarter'].fillna('Delhi')
### Cleaning the Sector Column
data_2020['Sector'].unique()
#rename similar sectors with wrong or different spelling to make it consistent
data_2020.replace({'Sector':{'Ecommerce':'E-Commerce','Edtech':'EdTech','Interior design':'Home Decor','Virtual Banking':'Financial Services',
                           'AgriTech':'AgTech','E-commerce & AR':'E-Commerce','Fintech':'FinTech','Infratech':'IT','B2B Supply Chain':'E-Commerce',
                           'Marketing & Customer loyalty':'Customer Service','Automobile & Technology':'Automotive','Transport & Rentals':'Transportation',
                           'HR tech':'Human Resources','AI & Tech':'AI','E-commerce':'E-Commerce','Travel':'Transportation','Health Care':'Healthcare',
                           'Commercial Real Estate':'Real Estate','Cooking':'Catering','Fantasy Sports':'Sports','Consumer':'Consumer Goods',
                           'Healthtech':'Healthcare','E-Sports':'Sports','Mutual Funds':'Finance','Legal tech':'Legal','Accomodation':'Real Estate',
                           'Consumer Lending':'Consumer Goods','Credit':'Credit Cards','Business Travel':'Transportation','Hospital':'Healthcare',
                           'Automobile':'Automotive','Alternative Medicine':'Healthcare','Renewable Energy':'Energy','Clean Energy':'Energy','Music':'Music Streaming',
                           'Health Diagnostics':'Healthcare','Medical':'Healthcare','Enterprise Software':'ERP','Enterprise Resource Planning (ERP)':'ERP',
                           'Audio':'Music Streaming','Air Transportation':'Aerospace','Yoga & wellness':'Fitness','Beauty':'Cosmetics','Big Data':'Analytics',
                           'Food and Beverage':'Catering','Wealth Management':'Finance','Social Media':'Digital Media','Broadcasting':'Media and Entertainment',
                           'News':'Media and Entertainment','Packaging Services':'Logistics','Mobile':'Mobile Phones','Medical Device':'Healthcare',
                           'Continuing Education':'Education','Dental':'Healthcare','Android':'Software','Banking':'Financial Services','Training':'Education',
                           'Digital Marketing':'Advertising','Marketplace':'Advertising','Consumer Electronics':'Consumer Goods','Rental':'Facilities Support Services',
                           'Retail':'Business Development','3D Printing':'Creative Agency','Electric Vehicle':'Automotive','Industrial Automation':'Manufacturing',
                           'Battery':'Energy','Reading Apps':'Apps','B2B':'E-Commerce','Communities':'Environmental Consulting','Smart Cities':'Environmental Consulting',
                           'Health Insurance':'Insurance','Commercial':'Business Development','Digital Entertainment':'Digital Media','Food & tech':'Catering',
                           'Cloud Infrastructure':'Cloud Computing','Marketing':'Advertising','Events':'Creative Agency','Food tech':'Catering',
                           'Artificial Intelligence':'AI','Business Intelligence':'ERP','Information Services':'IT','Information Technology':'IT',
                           'E-Learning':'Education','Tourism':'Hospitality','Industrial':'Manufacturing','Internet':'IT','Internet of Things':'IoT',
                           'Jewellery':'Fashion','Food & Nutrition':'Healthcare','Robotics & AI':'AI', 'E-marketplace':'E-Commerce','Foodtech':'Catering',
                           'Insurance technology':'Insurance','Pharmaceutical':'Healthcare','Safety tech':'Cybersecurity',"Fashion startup":'Fashion',
                             'Deisgning':'Creative Agency','Pharmacy':'Healthcare','Consultancy':'Creative Agency','Tech hub':'IT','E-connect':'Digital Media',
                             'B2B Agritech':'AgTech','Food diet':'Healthcare','Preschool Daycare':'Child Care','AI Robotics':'AI','SaaS/Edtech':'Saas',
                             'Transport & Rentals':'Transportation','Transport Automation':'Transportation','Neo-banking':'Financial Services',
                             'Ad-tech':'Advertising','E-mobility':'Mobile Payment','E tailor':'E-Commerce','Estore':'E-Commerce','Housing & Rentals':'Real Estate',
                             'AI & Deep learning':'AI','Sales & Services':'Business Development','VR & SaaS':'Saas','Hygiene':'CleanTech','Cleantech':'CleanTech',
                             'Visual Media':'Media and Entertainment','Content Marktplace':'Advertising','Machine Learning':'AI','AI & Media':'AI',
                             'E-tail':'E-Commerce','Automotive and Rentals':'Automotive','ravel tech':'Transportation','AI & Data science':'AI',
                             'Finance company':'Finance','Tech company':'IT','Solar Monitoring Company':'Energy','Video sharing platform':'Media and Entertainment',
                             'Gaming startup':'Gaming','Video streaming platform':'Media and Entertainment','Consumer appliances':'Consumer Goods',
                             'Blockchain startup':'Blockchain','Conversational AI platform':'AI','SaaS platform':'Saas','AI platform':'AI',
                             'Escrow':'Legal','Networking platform':'IT','Spacetech':'Aerospace','Trading platform':'E-Commerce','AI Company':'AI',
                             'Photonics startup':'Media and Entertainment','Entertainment':'Media and Entertainment','Scanning app':'Apps',
                             'Skincare startup':'Cosmetics','Food and Beverages':'Catering','FoodTech':'Catering','Biotechnology company':'Environmental Consulting',
                             'Proptech':'Facilities Support Services','Fitness startup':'Fitness','PaaS startup':'Saas','Beverages':'Catering',
                             'Automobiles':'Automotive','Deeptech':'AI','EV startup':'Environmental Consulting','AR/VR startup':'IT','Recruitment startup':'Human Resources',
                             'Tourism & EV':'Hospitality','QSR startup':'Customer Service','Video platform':'Media and Entertainment',
                             'Fusion beverages':'Catering','Job portal':'Human Resources','Dairy startup':'Catering','Content management':'Creative Agency',
                             'Work fulfillment':'Human Resources','HR Tech':'Human Resources','Software Company':'Software','Soil-Tech':'Agriculture',
                             'Travel & SaaS':'Transportation','Hygiene management':'CleanTech','Food & Bevarages':'Catering','Food Delivery':'Catering',
                             'Virtual auditing startup':'FinTech','HealthTech':'Healthcare','AI startup':'AI','Tech Startup':'IT','Medtech':'Healthcare',
                             'Tyre management':'Automotive','Cloud company':'Cloud Computing','Software company':'Software','Venture capitalist':'Finance',
                             'Renewable player':'Energy','IoT startup':'IoT','SaaS startup':'Saas','Aero company':'Aerospace','Marketing company':'Advertising',
                             'Retail startup':'Business Development','Co-working Startup':'Human Resources','—':'Unknown','Brand Marketing':'Advertising',
                             'Fertility tech':'Healthcare','Luxury car startup':'Automotive','FM':'Media and Entertainment','Food':'Catering',
                             'Nutrition sector':'Healthcare','Tech platform':'IT','Video':'Media and Entertainment','Retail Tech':'Business Development',
                             'HeathTech':'Healthcare','Sles and marketing':'Advertising','LegalTech':'Legal','Car Service':'Automotive',
                             'Bike marketplace':'Automotive','Agri tech':'AgTech','Reatil startup':'Business Development','AR platform':'IT',
                             'Content marketplace':'Creative Agency','Interior Design':'Home Decor','Home Design':'Home Decor','InsureTech':'Insurance',
                             'Rental space':'Facilities Support Services','Ayurveda tech':'IT','Packaging solution startup':'Logistics',
                             'Sanitation solutions':'CleanTech','HealthCare':'Healthcare','AI Startup':'AI','Solar solution':'Energy','Tech':'IT',
                             'Jewellery startup':'Fashion','Multinational conglomerate company':'Business Development','Deeptech startup':'AI',
                             'Social Network':'Digital Media','Publication':'Creative Agency','Venture capital':'Finance','Entreprenurship':'Business Development',
                             'E-market':'E-Commerce','Media & Networking':'Media and Entertainment','Automation tech':'Automotive','eMobility':'Mobile Payment',
                             'Food devlivery':'Catering','Warehouse':'Facilities Support Services','Online financial service':'FinTech',
                             'Eyeglasses':'Healthcare','Battery design':'Energy','Online credit management startup':'FinTech','Beverage':'Catering',
                             'TravelTech':'Transportation','Startup laboratory':'Business Development','Personal care startup':'Fitness',
                             'Customer service company':'Customer Service','SaaS\xa0\xa0startup':'Saas','Marketing startup':'Advertising',
                             'Service industry':'Human Resources','Social media':'Digital Media','R startup':'IT','HR Tech startup':'Human Resources',
                             'AR startup':'IT','Automotive Startup':'Automotive','Food Startup':'Catering','EdTech Startup':'Education',
                             'Car Trade':'Automotive','EdtTech':'Education','AI Platform':'AI','Automation':'Automotive','Solar SaaS':'Saas',
                             'WL & RAC protection':'Environmental Consulting','Social commerce':'E-Commerce','Home interior services':'Home Decor',
                             'Agritech startup':'AgTech','API platform':'Software','Deep Tech':'AI','Electricity':'Energy','Automotive company':'Automotive',
                             'FMCG':'Legal','Insurance Tech':'Insurance','Video personalization':'Media and Entertainment','Biomaterial startup':'Manufacturing',
                             'Health':'Healthcare','Craft Beer':'Creative Agency','Investment':'Finance','Linguistic Spiritual':'Education',
                             'Battery manufacturer':'Energy','Nano Distribution Network':'IT','AI health':'AI','Dating app':'Dating',
                             'Media':'Media and Entertainment','Healthcare/Edtech':'Healthcare','Social Commerce':'E-Commerce','Agritech/Commerce':'AgTech',
                             'Mobility tech':'Mobile Phones','Social e-commerce':'E-Commerce','Food & Logistics':'Logistics','SpaceTech':'Aerospace',
                             'Nutrition Tech':'Healthcare','HR':'Human Resources','nan':'Unknown','Agritech':'AgTech','AR/VR':'IT',
                             'Appliance':'Consumer Goods','Mental Health':'Healthcare','Solar Solution':'Energy','Saas':'SaaS','B2B marketplace':'E-Commerce',
                             'Fashion Tech':'Fashion','Nutrition tech':'Healthcare','Health & Wellness':'Fitness','Cloud Kitchen':'Cloud Computing',
                             'IoT/Automobile':'IoT','Eye Wear':'Healthcare','Digital tech':'IT','Data Intelligence':'ERP','Co-living':'Human Resources',
                             'Food & Beverages':'Catering','Defense tech':'Cybersecurity','Construction tech':'Construction','Nutrition':'Healthcare',
                             'Coworking':'Human Resources','Micro-mobiity':'Mobile Phones','Auto-tech':'Automotive','Robotics':'AI','Logitech':'IT',
                             'Med Tech':'Healthcare','Life sciences':'Healthcare','Retail Aggregator':'Business Development','Deep Tech AI':'AI',
                             'Biotech':'Environmental Consulting','HrTech':'Human Resources','AI & Debt':'AI','Transport':'Transportation',
                             'Co-working':'Human Resources','Insurtech':'Insurance','Automotive tech':'Automotive','EV':'Environmental Consulting',
                             'Supply chain, Agritech':'Logistics','Pharma':'Healthcare','Foodtech & Logistics':'Logistics','Housing':'Real Estate',
                             'Data Analytics':'ERP','Investment Tech':'Finance','Biopharma':'Healthcare','Dairy':'Catering','Beauty & wellness':'Fitness',
                             'Media Tech':'Media and Entertainment','E store':'E-Commerce','Data Science':'ERP','Travel tech':'Transportation',
                             'Techonology':'IT','Taxation':'Accounting','Automobile Technology':'Automotive','Food Industry':'Catering','Financial Services':'Financial Services',
                             'Interior & decor':'Home Decor','Health and Fitness':'Fitness','Location Analytics':'ERP','Mobility/Transport':'Transportation',
                           'Technology':'IT','Farming':'Agriculture','Food Processing':'Catering','Collaboration':'Human Resources','Computer':'IT','Career Planning':'Human Resources',
                           'Wedding':'Customer Service','Games':'Gaming'}}, inplace=True)
data_2020['Sector'].unique()
# all nan values are to be filled with Unknown
data_2020['Sector']=data_2020['Sector'].fillna('Unknown')
### Cleaning the Amount($) Column
#First change the data type to a string to work on the special characters
data_2020['Amount($)']=data_2020['Amount($)'].astype(str) 
# retrieveing records with Undisclosed in the Amount($) column
data_2020.loc[data_2020['Amount($)']=='Undiclsosed']
#und_1=data_2020['Amount($)']=='Undiclsosed'
#und_2=data_2020['Amount($)']=='Undislosed'
#und_3=data_2020['Amount($)']=='Undisclosed'
# correcting the name of the Amount($) from Undiclsosed to 0 based on the information above.
data_2020.iloc[665].replace({'Undiclsosed':np.nan},inplace=True) 
# retrieveing records with Undislosed in the Amount($) column
data_2020.loc[data_2020['Amount($)']=='Undislosed']
# correcting the name of the Amount($) from Undislosed to 0 based on the information above.
data_2020.iloc[665].replace({'Undislosed':np.nan},inplace=True) 
# retrieveing records with Undisclosed in the Amount($) column
data_2020.loc[data_2020['Amount($)']=='Undisclosed']
# replacing all Undisclosed in the Amount($) column with 0
data_2020.loc[data_2020['Amount($)']=='Undisclosed',['Amount($)']]=[0]
# removing all special characters in the colum
data_2020["Amount($)"]=data_2020['Amount($)'].apply(lambda x: str(x).replace("$" ,"")) 
data_2020["Amount($)"]=data_2020['Amount($)'].apply(lambda x: str(x).replace("," ,""))
# change the data type to a float
# The downcast parameter is used to set the data type to float
data_2020['Amount($)'] = pd.to_numeric(data_2020['Amount($)'], errors='coerce', downcast='float') 
# Replacing nan values with median
data_2020['Amount($)']=data_2020['Amount($)'].fillna(data_2020['Amount($)'].median())
### Cleaning the Stage Column
data_2020['Stage'].unique()
data_2020.replace({'Stage':{'Pre-Seed':'Seed','Pre-series A':'Series A','Pre-series':'Seed','Pre-series C':'Series C',
                            'Pre-Series B':'Series B','Series A-1':'Series A','Seed Funding':'Seed','Seed round':'Seed',
                            'Pre-seed Round':'Seed','Pre seed Round':'Seed','Seed Round & Series A':'Series A',
                            'Pre Series A':'Series A','Angel Round':'Seed','Pre series A1':'Series A','Series E2':'Series E',
                            'Pre series A':'Series A','Seed Round':'Seed','Bridge Round':'Seed','Bridge':'Seed',
                            'Edge':'Seed','Pre seed round':'Seed','Pre series B':'Series B','Pre series C':'Series C',
                            'Pre series A1':'Series A','Series D1':'Series D','Mid series':'Seed','Series C, D':'Series C',
                           'Pre-series B':'Series B','Series B2':'Series B','Pre- series A':'Series A',
                           'PPre-seed':'Seed','Seed funding':'Seed','Seed A':'Series A','Pre-seed':'Seed','Debt':'Seed','Debt':'Seed',
                            'Angel':'Seed','Secondary Market':'Seed','Equity':'Seed','Grant':'Seed',
                            'Seed Investment':'Seed'}},inplace=True)
data_2020['Stage'].unique()
# replacing all nan with Unknown
data_2020['Stage']=data_2020['Stage'].fillna('Seed')
### Cleaning the `Founders` Column
# all nan values are to be filled with unknown on the 
data_2020['Founders']=data_2020['Founders'].fillna('Seed')
### Cleaning the `Investor` Column
# all nan values are to be filled with unknown on the 
data_2020['Investor']=data_2020['Investor'].fillna('Unknown')
# checking for duplicate entries
data_2020.duplicated().value_counts()
# remove the duplicate record
data_2020=data_2020.drop_duplicates(keep='first')  
data_2020.isnull().sum()
# 2021 Data Set
data_2021.head()
data_2021.info()
### Cleaning the Founded Column
data_2021['Founded'].unique()
# convert the data type from float to a string
data_2021['Founded']=data_2021['Founded'].astype(str) 

# removing the decimal points in the year, slicing is used to pick the whole number only
data_2021['Founded']=data_2021.Founded.str.split('.').str[0]
# changing the data type to numeric
data_2021['Founded']=pd.to_numeric(data_2021['Founded'],errors='coerce',downcast='unsigned')
# replacing nan with 2021 since it is a 2021 data set
data_2021['Founded']=data_2021['Founded'].fillna(2021)
data_2021['Founded'].unique()
### Cleaning the HeadQuarter Column
data_2021['HeadQuarter'].unique()
# checking for the record with Information Technology & Services at the headquater column
data_2021.loc[data_2021['HeadQuarter']=='Information Technology & Services']
It is realized that the values at the Sector column and the HeadQuarter column has been interchanged. This is corrected in the cell below.
# correcting the wrong placement of the HeadQuarter and Sector values
data_2021.loc[data_2021['Company/Brand']=='Peak',['HeadQuarter','Sector']]=['Manchester','IT']
# checking for the record with Online Media\t#REF! at the headquater column
data_2021.loc[data_2021['HeadQuarter']=='Online Media\t#REF!']
The correction of the transposition is done in the cell below
# correcting the wrong placement of the HeadQuarter and Sector values
data_2021.loc[data_2021['Company/Brand']=='Sochcast',['HeadQuarter','Sector','What it does']]=['Mountain View','Media and Entertainment','Sochcast is an Audio experiences company that give the listener and creators an Immersive Audio experience']
# checking for the record with Gurugram\t#REF! at the headquater column
data_2021.loc[data_2021['HeadQuarter']=='Gurugram\t#REF!']
The correction together with the sector which has now been identified is done in the code below
# correcting the wrong placement of the HeadQuarter and Sector values
data_2021.loc[data_2021['Company/Brand']=='MoEVing',['HeadQuarter','Sector']]=['Gurugram','Automotive']
# checking for the record with Food & Beverages at the headquater column
data_2021.loc[data_2021['HeadQuarter']=='Food & Beverages']
This investigation also revealed a duplicated entry. The second entry with index 255 will be dropped and then the correction will be done
# dropping the duplicate with its index
data_2021.drop(index=[255],axis=0, inplace=True)

# correcting the wrong placement of the HeadQuarter and Sector values
data_2021.loc[data_2021['Company/Brand']=='MasterChow',['HeadQuarter','Sector']]=['Hauz Khas', 'Catering']
# checking for the record with Computer Games at the headquater column
data_2021.loc[data_2021['HeadQuarter']=='Computer Games']
This investigation also revealed a duplicated entry. The second entry with index 111 will be dropped and then the correction will be done. 

It also revealed that the amount and stage column has been interchanged. 
# dropping the duplicate with its index
data_2021.drop(index=[111],axis=0, inplace=True)

# correcting the wrong placement of the HeadQuarter and Sector values
data_2021.loc[data_2021['Company/Brand']=='FanPlay',['HeadQuarter','Sector','Stage','Amount($)']]=['New York', 'Gaming','Unknown',1200000]
# checking for the record with Pharmaceuticals\t#REF! at the headquater column
data_2021.loc[data_2021['HeadQuarter']=='Pharmaceuticals\t#REF!']
This investigation also revealed a duplicated entry. The second entry with index 256 will be dropped and then the correction will be done
# dropping the duplicate with its index
data_2021.drop(index=[256],axis=0, inplace=True)

# correcting the wrong placement of the HeadQuarter and Sector values
data_2021.loc[data_2021['Company/Brand']=='Fullife Healthcare',['HeadQuarter','Sector']]=['Unknown', 'Healthcare']
data_2021.replace({'HeadQuarter':{'New Delhi':'Delhi','Small Towns, Andhra Pradesh':'Andhra Pradesh',
                                 'Thiruvananthapuram':'Trivandrum','Mountain View, CA':'Mountain View',
                                 'Mangalore':'Bengaluru','Bangalore':'Bengaluru'}}, inplace=True)
# replacing nan with Delhi on the assumption that i is an India startup data set
data_2021['HeadQuarter']=data_2021['HeadQuarter'].fillna('Delhi')
### Cleaning the Sector Column
data_2021['Sector'].unique()
#rename similar sectors with wrong or different spelling to make it consistent
data_2021.replace({'Sector':{'Ecommerce':'E-Commerce','Edtech':'EdTech','Interior design':'Home Decor','Virtual Banking':'Financial Services',
                           'AgriTech':'AgTech','E-commerce & AR':'E-Commerce','Fintech':'FinTech','Infratech':'IT','B2B Supply Chain':'E-Commerce',
                           'Marketing & Customer loyalty':'Customer Service','Automobile & Technology':'Automotive','Transport & Rentals':'Transportation',
                           'HR tech':'Human Resources','AI & Tech':'AI','E-commerce':'E-Commerce','Travel':'Transportation','Health Care':'Healthcare',
                           'Commercial Real Estate':'Real Estate','Cooking':'Catering','Fantasy Sports':'Sports','Consumer':'Consumer Goods',
                           'Healthtech':'Healthcare','E-Sports':'Sports','Mutual Funds':'Finance','Legal tech':'Legal','Accomodation':'Real Estate',
                           'Consumer Lending':'Consumer Goods','Credit':'Credit Cards','Business Travel':'Transportation','Hospital':'Healthcare',
                           'Automobile':'Automotive','Alternative Medicine':'Healthcare','Renewable Energy':'Energy','Clean Energy':'Energy','Music':'Music Streaming',
                           'Health Diagnostics':'Healthcare','Medical':'Healthcare','Enterprise Software':'ERP','Enterprise Resource Planning (ERP)':'ERP',
                           'Audio':'Music Streaming','Air Transportation':'Aerospace','Yoga & wellness':'Fitness','Beauty':'Cosmetics','Big Data':'Analytics',
                           'Food and Beverage':'Catering','Wealth Management':'Finance','Social Media':'Digital Media','Broadcasting':'Media and Entertainment',
                           'News':'Media and Entertainment','Packaging Services':'Logistics','Mobile':'Mobile Phones','Medical Device':'Healthcare',
                           'Continuing Education':'Education','Dental':'Healthcare','Android':'Software','Banking':'Financial Services','Training':'Education',
                           'Digital Marketing':'Advertising','Marketplace':'Advertising','Consumer Electronics':'Consumer Goods','Rental':'Facilities Support Services',
                           'Retail':'Business Development','3D Printing':'Creative Agency','Electric Vehicle':'Automotive','Industrial Automation':'Manufacturing',
                           'Battery':'Energy','Reading Apps':'Apps','B2B':'E-Commerce','Communities':'Environmental Consulting','Smart Cities':'Environmental Consulting',
                           'Health Insurance':'Insurance','Commercial':'Business Development','Digital Entertainment':'Digital Media','Food & tech':'Catering',
                           'Cloud Infrastructure':'Cloud Computing','Marketing':'Advertising','Events':'Creative Agency','Food tech':'Catering',
                           'Artificial Intelligence':'AI','Business Intelligence':'ERP','Information Services':'IT','Information Technology':'IT',
                           'E-Learning':'Education','Tourism':'Hospitality','Industrial':'Manufacturing','Internet':'IT','Internet of Things':'IoT',
                           'Jewellery':'Fashion','Food & Nutrition':'Healthcare','Robotics & AI':'AI', 'E-marketplace':'E-Commerce','Foodtech':'Catering',
                           'Insurance technology':'Insurance','Pharmaceutical':'Healthcare','Safety tech':'Cybersecurity',"Fashion startup":'Fashion',
                             'Deisgning':'Creative Agency','Pharmacy':'Healthcare','Consultancy':'Creative Agency','Tech hub':'IT','E-connect':'Digital Media',
                             'B2B Agritech':'AgTech','Food diet':'Healthcare','Preschool Daycare':'Child Care','AI Robotics':'AI','SaaS/Edtech':'Saas',
                             'Transport & Rentals':'Transportation','Transport Automation':'Transportation','Neo-banking':'Financial Services',
                             'Ad-tech':'Advertising','E-mobility':'Mobile Payment','E tailor':'E-Commerce','Estore':'E-Commerce','Housing & Rentals':'Real Estate',
                             'AI & Deep learning':'AI','Sales & Services':'Business Development','VR & SaaS':'Saas','Hygiene':'CleanTech','Cleantech':'CleanTech',
                             'Visual Media':'Media and Entertainment','Content Marktplace':'Advertising','Machine Learning':'AI','AI & Media':'AI',
                             'E-tail':'E-Commerce','Automotive and Rentals':'Automotive','ravel tech':'Transportation','AI & Data science':'AI',
                             'Finance company':'Finance','Tech company':'IT','Solar Monitoring Company':'Energy','Video sharing platform':'Media and Entertainment',
                             'Gaming startup':'Gaming','Video streaming platform':'Media and Entertainment','Consumer appliances':'Consumer Goods',
                             'Blockchain startup':'Blockchain','Conversational AI platform':'AI','SaaS platform':'Saas','AI platform':'AI',
                             'Escrow':'Legal','Networking platform':'IT','Spacetech':'Aerospace','Trading platform':'E-Commerce','AI Company':'AI',
                             'Photonics startup':'Media and Entertainment','Entertainment':'Media and Entertainment','Scanning app':'Apps',
                             'Skincare startup':'Cosmetics','Food and Beverages':'Catering','FoodTech':'Catering','Biotechnology company':'Environmental Consulting',
                             'Proptech':'Facilities Support Services','Fitness startup':'Fitness','PaaS startup':'Saas','Beverages':'Catering',
                             'Automobiles':'Automotive','Deeptech':'AI','EV startup':'Environmental Consulting','AR/VR startup':'IT','Recruitment startup':'Human Resources',
                             'Tourism & EV':'Hospitality','QSR startup':'Customer Service','Video platform':'Media and Entertainment',
                             'Fusion beverages':'Catering','Job portal':'Human Resources','Dairy startup':'Catering','Content management':'Creative Agency',
                             'Work fulfillment':'Human Resources','HR Tech':'Human Resources','Software Company':'Software','Soil-Tech':'Agriculture',
                             'Travel & SaaS':'Transportation','Hygiene management':'CleanTech','Food & Bevarages':'Catering','Food Delivery':'Catering',
                             'Virtual auditing startup':'FinTech','HealthTech':'Healthcare','AI startup':'AI','Tech Startup':'IT','Medtech':'Healthcare',
                             'Tyre management':'Automotive','Cloud company':'Cloud Computing','Software company':'Software','Venture capitalist':'Finance',
                             'Renewable player':'Energy','IoT startup':'IoT','SaaS startup':'Saas','Aero company':'Aerospace','Marketing company':'Advertising',
                             'Retail startup':'Business Development','Co-working Startup':'Human Resources','—':'Unknown','Brand Marketing':'Advertising',
                             'Fertility tech':'Healthcare','Luxury car startup':'Automotive','FM':'Media and Entertainment','Food':'Catering',
                             'Nutrition sector':'Healthcare','Tech platform':'IT','Video':'Media and Entertainment','Retail Tech':'Business Development',
                             'HeathTech':'Healthcare','Sles and marketing':'Advertising','LegalTech':'Legal','Car Service':'Automotive',
                             'Bike marketplace':'Automotive','Agri tech':'AgTech','Reatil startup':'Business Development','AR platform':'IT',
                             'Content marketplace':'Creative Agency','Interior Design':'Home Decor','Home Design':'Home Decor','InsureTech':'Insurance',
                             'Rental space':'Facilities Support Services','Ayurveda tech':'IT','Packaging solution startup':'Logistics',
                             'Sanitation solutions':'CleanTech','HealthCare':'Healthcare','AI Startup':'AI','Solar solution':'Energy','Tech':'IT',
                             'Jewellery startup':'Fashion','Multinational conglomerate company':'Business Development','Deeptech startup':'AI',
                             'Social Network':'Digital Media','Publication':'Creative Agency','Venture capital':'Finance','Entreprenurship':'Business Development',
                             'E-market':'E-Commerce','Media & Networking':'Media and Entertainment','Automation tech':'Automotive','eMobility':'Mobile Payment',
                             'Food devlivery':'Catering','Warehouse':'Facilities Support Services','Online financial service':'FinTech',
                             'Eyeglasses':'Healthcare','Battery design':'Energy','Online credit management startup':'FinTech','Beverage':'Catering',
                             'TravelTech':'Transportation','Startup laboratory':'Business Development','Personal care startup':'Fitness',
                             'Customer service company':'Customer Service','SaaS\xa0\xa0startup':'Saas','Marketing startup':'Advertising',
                             'Service industry':'Human Resources','Social media':'Digital Media','R startup':'IT','HR Tech startup':'Human Resources',
                             'AR startup':'IT','Automotive Startup':'Automotive','Food Startup':'Catering','EdTech Startup':'Education',
                             'Car Trade':'Automotive','EdtTech':'Education','AI Platform':'AI','Automation':'Automotive','Solar SaaS':'Saas',
                             'WL & RAC protection':'Environmental Consulting','Social commerce':'E-Commerce','Home interior services':'Home Decor',
                             'Agritech startup':'AgTech','API platform':'Software','Deep Tech':'AI','Electricity':'Energy','Automotive company':'Automotive',
                             'FMCG':'Legal','Insurance Tech':'Insurance','Video personalization':'Media and Entertainment','Biomaterial startup':'Manufacturing',
                             'Health':'Healthcare','Craft Beer':'Creative Agency','Investment':'Finance','Linguistic Spiritual':'Education',
                             'Battery manufacturer':'Energy','Nano Distribution Network':'IT','AI health':'AI','Dating app':'Dating',
                             'Media':'Media and Entertainment','Healthcare/Edtech':'Healthcare','Social Commerce':'E-Commerce','Agritech/Commerce':'AgTech',
                             'Mobility tech':'Mobile Phones','Social e-commerce':'E-Commerce','Food & Logistics':'Logistics','SpaceTech':'Aerospace',
                             'Nutrition Tech':'Healthcare','HR':'Human Resources','nan':'Unknown','Agritech':'AgTech','AR/VR':'IT',
                             'Appliance':'Consumer Goods','Mental Health':'Healthcare','Solar Solution':'Energy','Saas':'SaaS','B2B marketplace':'E-Commerce',
                             'Fashion Tech':'Fashion','Nutrition tech':'Healthcare','Health & Wellness':'Fitness','Cloud Kitchen':'Cloud Computing',
                             'IoT/Automobile':'IoT','Eye Wear':'Healthcare','Digital tech':'IT','Data Intelligence':'ERP','Co-living':'Human Resources',
                             'Food & Beverages':'Catering','Defense tech':'Cybersecurity','Construction tech':'Construction','Nutrition':'Healthcare',
                             'Coworking':'Human Resources','Micro-mobiity':'Mobile Phones','Auto-tech':'Automotive','Robotics':'AI','Logitech':'IT',
                             'Med Tech':'Healthcare','Life sciences':'Healthcare','Retail Aggregator':'Business Development','Deep Tech AI':'AI',
                             'Biotech':'Environmental Consulting','HrTech':'Human Resources','AI & Debt':'AI','Transport':'Transportation',
                             'Co-working':'Human Resources','Insurtech':'Insurance','Automotive tech':'Automotive','EV':'Environmental Consulting',
                             'Supply chain, Agritech':'Logistics','Pharma':'Healthcare','Foodtech & Logistics':'Logistics','Housing':'Real Estate',
                             'Data Analytics':'ERP','Investment Tech':'Finance','Biopharma':'Healthcare','Dairy':'Catering','Beauty & wellness':'Fitness',
                             'Media Tech':'Media and Entertainment','E store':'E-Commerce','Data Science':'ERP','Travel tech':'Transportation',
                             'Techonology':'IT','Taxation':'Accounting','Automobile Technology':'Automotive','Food Industry':'Catering','Financial Services':'Financial Services',
                             'Interior & decor':'Home Decor','Health and Fitness':'Fitness','Location Analytics':'ERP','Mobility/Transport':'Transportation',
                           'Technology':'IT','Farming':'Agriculture','Food Processing':'Catering','Collaboration':'Human Resources','Computer':'IT','Career Planning':'Human Resources',
                           'Wedding':'Customer Service','Games':'Gaming','EdTech':'Education','B2B E-commerce':'E-Commerce','Home services':'Home Decor',
                            'B2B service':'E-Commerce','Electronics':'Automotive','IT startup':'IT','Oil and Energy':'Energy','Milk startup':'Catering',
                            'AI Chatbot':'AI','Food delivery':'Catering','Fantasy sports':'Sports','Video communication':'Media and Entertainment',
                            'Skill development':'Education','Recruitment':'Human Resources','Computer Games':'Gaming','Apparel & Fashion':'Fashion',
                            'Logistics & Supply Chain':'Logistics','SportsTech':'Sports','HRTech':'Human Resources','Wine & Spirits':'Catering',
                            '-Commerce':'E-Commerce','Mechanical & Industrial Engineering':'Manufacturing','Lifestyle':'Fitness','Computer software':'Software',
                            'Tech startup':'IT','Digital mortgage':'FinTech','Information Technology & Services':'IT','Furniture':'Home Decor',
                            'Tobacco':'Consumer Goods','Insuretech':'Insurance','MLOps platform':'FinTech','Venture Capital':'Finance','Pet care':'Veterinary',
                            'Drone':'Aeorspace','Wholesale':'Business Development','Spiritual':'Education','E-learning':'Education',
                            'Consumer Services':'Customer Service','Venture Capital & Private Equity':'Finance','Health':'Helathcare',
                             'Health, Wellness & Fitness':'Fitness','Education Management':'Education','Computer Software':'Software',
                             'Software Startup':'Software','Digital platform':'Digital Media','B2B Ecommerce':'E-Commerce',
                             'Online Media':'Media and Entertainment','Mobile Games':'Gaming','Food Production':'Catering',
                             'Podcast':'Media and Entertainment','Content publishing':'Creative Agency','Water purification':'Environmental Consulting',
                             'Content commerce':'Creative Agency','Innovation Management':'Creative Agency','Celebrity Engagement':'Media and Entertainment',
                             'Personal Care':'Consumer Goods','Cannabis startup':'Consumer Goods','Blogging':'Media and Entertainment',
                             'BioTechnology':'Environmental Consulting','B2B Marketplace':'E-Commerce','Health care':'Helathcare',
                             'Social audio':'Media and Entertainment','Fashion and lifestyle':'Fashion','Delivery service':'Logistics',
                            'Computer & Network Security':'IT','OTT':'IT','Capital Markets':'Finance','Social network':'Digital Media',
                             'Hospital & Health Care':'Helathcare','Music Streaming':'Media and Entertainment','Mobility':'Mobile Phones',
                             'Digital platform':'Digital Media','B2B Manufacturing':'Manufacturing','Solar':'Energy','TaaS startup':'SaaS',
                            'Manufacturing startup':'Manufacturing','Vehicle repair startup':'Automotive','Advisory firm':'Legal',
                            'Legaltech':'Legal','Pollution control equiptment':'Environmental Consulting','Fashion & Lifestyle':'Fashion',
                            'D2C':'E-Commerce','Environmental Services':'Environmental Consulting','Merchandise':'Business Development',
                            'Facilities Services':'Facilities Support Services','Marketing & Advertising':'Advertising',
                            'Eyewear':'Helathcare','Healtcare':'Helathcare','D2C Business':'E-Commerce',
                            'Biotechnology':'Environmental Consulting','NFT Marketplace':'E-Commerce','Aeorspace':'Aerospace',
                            'Consumer software':'Consumer Goods','Social community':'Digital Media','Fishery':'Agriculture',
                             'Renewables & Environment':'Energy','Online storytelling':'Media and Entertainment','Aviation':'Aerospace',
                            'IT company':'IT','Environmental service':'Environmental Consulting','Job discovery platform':'Human Resources',
                            'D2C Fashion':'Fashion','Heathcare':'Helathcare','CRM':'ERP','D2C startup':'E-Commerce',
                            'Innovation management':'Creative Agency','Community platform':'Digital Media',
                            'Networking':'IT','Consumer service':'Customer Service','Consumer goods':'Consumer Goods',
                            'MarTech':'Manufacturing','Content creation':'Creative Agency','Augmented reality':'IT',
                           'Crypto':'Cryptocurrency','Bike Rental':'Automotive','Beauty products':'Cosmetics',
                             'FemTech':'Agriculture','Supply chain platform':'Logistics','Social platform':'Digital Media',
                            'AI company':'AI','Sports startup':'Sports','Clothing':'Fashion','Analytics':'ERP',
                            'IoT platform':'IoT','Commerce':'E-Commerce','Matrimony':'Cultural',
                             'Defense & Space':'Cybersecurity','Business Supplies & Equipment':'Business Development',
                            'NFT':'IT','Oil & Energy':'Energy','Company-as-a-Service':'SaaS','Textiles':'Fashion',
                            'Professional Training & Coaching':'Education','Housing Marketplace':'Real Estate',
                            'Real estate':'Real Estate','Furniture Rental':'Home Decor','Equity Management':'Finance',
                           'Cloud kitchen':'Cloud Computing','Community':'Cultural','Higher Education':'Education',
                            'Mechanical Or Industrial Engineering':'Manufacturing','D2C jewellery':'Fashion','Helathcare':'Healthcare',
                            'Sales and Distribution':'Business Development','Translation & Localization':'Cultural',
                            'Investment Banking':'Finance','Femtech':'Agriculture','sports':'Sports',
                            'Foootwear':'Fashion','Legal Services':'Legal','Arts & Crafts':'Creative Agency',
                            'Investment Management':'Finance','Management Consulting':'Human Resources',
                             'B2B startup':'E-Commerce','Telecommunications':'Telecommuncation','Product studio':'Media and Entertainment',
                             'Aviation & Aerospace':'Aerospace','Staffing & Recruiting':'Human Resources',
                            'Design':'Home Decor','B2B Travel':'Transportation'}}, inplace=True)
data_2021['Sector'].unique()
# replacing nan with unknown
data_2021['Sector']=data_2021['Sector'].fillna('Unknown')
### Cleaning the Stage Column
data_2021['Stage'].unique()
data_2021.replace({'Stage':{'Pre-Seed':'Seed','Pre-series A':'Series A','Pre-series':'Seed','Pre-series C':'Series C',
                            'Pre-Series B':'Series B','Series A-1':'Series A','Seed Funding':'Seed','Seed round':'Seed',
                            'Pre-seed Round':'Seed','Pre seed Round':'Seed','Series A2':'Series A','Seies A':'Series A',
                            'Pre Series A':'Series A','Angel Round':'Angel','Pre series A1':'Series A','Pre-series A1':'Series A',
                            'Pre series A':'Series A','Seed Round':'Seed','Bridge Round':'Seed','Bridge':'Seed','Series F2':'Series F',
                            'Edge':'Seed','Pre seed round':'Seed','Series B3':'Series B','Pre series C':'Series C',
                            'Series A+':'Series A','Series D1':'Series D','Early seed':'Seed','Series C, D':'Series C',
                           'Pre-series B':'Series B','Series B2':'Series B','Pre- series A':'Series A','Series F1':'Series F','Debt':'Seed',
                            'Angel':'Seed','Debt':'Seed','Unknown':'Seed',
                            'Angel':'Seed','Secondary Market':'Seed','Equity':'Seed','Grant':'Seed',
                           'Seed+':'Seed','PE':'Seed','Seed A':'Series A','Pre-seed':'Seed'}},inplace=True)
data_2021['Stage'].unique()
# checking for the record with $300000 at the Stage column
data_2021.loc[data_2021['Stage']=='$300000']
The inestigation reveals that the amount and stage column has been interchanged. Two different records had this transposition error. The code below is used to correct them correct them below.
# correcting the wrong placement of the HeadQuarter and Sector values
data_2021.loc[data_2021['Company/Brand']=='Little Leap',['Amount($)','Sector','Stage']]=[300000, 'Education','Seed']
# correcting the secod record found above
data_2021.loc[data_2021['Company/Brand']=='BHyve',['Amount($)','Stage']]=[300000,'Seed']
 # checking for the record with $6000000 at the Stage column
data_2021.loc[data_2021['Stage']=='$6000000']
This output revelaed that the sector named will have to be consistent with others. Aahain the stage column and the Amount column has been transposed. These are corrected in the code below
# correcting the wrong placement of the HeadQuarter and Sector values
data_2021.loc[data_2021['Company/Brand']=='MYRE Capital',['Amount($)','Sector','Stage']]=[6000000, 'Real Estate','Seed']
 # checking for the record with $1000000 at the Stage column
data_2021.loc[data_2021['Stage']=='$1000000']
This output revelaed that the sector named will have to be consistent with others. Aahain the stage column and the Amount column has been transposed. These are corrected in the code below
# correcting the wrong placement of the HeadQuarter and Sector values
data_2021.loc[data_2021['Company/Brand']=='Saarthi Pedagogy',['Amount($)','Sector','Stage']]=[1000000, 'Education','Seed']
data_2021['Stage'].unique()
# replacing nan with unknown
data_2021['Stage']=data_2021['Stage'].fillna('Seed')
### Cleaning the Amount($) Column
data_2021['Amount($)'].unique()
Values such as `Undisclosed`, `Series C`, `Seed`,`$Undisclosed`, `$undisclosed`, `Pre-series A` all wrongly entered into the Amount($) column. This will have to be investigated and corrected.
# checking for undisclosed in the amount column
data_2021.loc[data_2021['Amount($)']=='Undisclosed']
The invetsigation output above revealed  that none of the records had the amount wrongly entered in a different column. It is obvious that the amount was not disclosed. Theey are replace with 0. 
# replacing the Undisclosed with nan
data_2021.replace({'Amount($)':{'Undisclosed',np.nan}}, inplace=True)
# checking for $Undisclosed in the amount column
data_2021.loc[data_2021['Amount($)']=='$Undisclosed']
The invetsigation output above with 73 records revealed that none of the records had the amount wrongly entered in a different column. It is obvious that the amount was not disclosed. They are replace with 0.
# replacing the $Undisclosed with nan
data_2021.replace({'Amount($)':{'$Undisclosed',np.nan}}, inplace=True)
# checking for $undisclosed in the amount column
data_2021.loc[data_2021['Amount($)']=='$undisclosed']
The invetsigation output above revealed that none of the records had the amount wrongly entered in a different column. It is obvious that the amount was not disclosed. They are replace with 0
# replacing the $undisclosed with 0
data_2021.replace({'Amount($)':{'$undisclosed',np.nan}}, inplace=True)
# checking for Series C in the amount column
data_2021.loc[data_2021['Amount($)']=='Series C']
This output revelas that there haas been some transposition at the investor, amount and stage column.

This is corrected in the code below
# correcting the wrong placement of the HeadQuarter and Sector values
data_2021.loc[data_2021['Company/Brand']=='Fullife Healthcare',['Amount($)','Investor','Stage']]=[22000000, 'Morgan Stanley','Series C']
# checking for Seed in the amount column
data_2021.loc[data_2021['Amount($)']=='Seed']
The output of the investigation shows that two records were involved with this error. The stage, investor and Amount columns were interchanged. This is corrected in the code below
# correcting the wrong placement of the HeadQuarter and Sector values
data_2021.loc[data_2021['Company/Brand']=='MoEVing',['Amount($)','Investor','Stage']]=[5000000, 'Unknown','Seed']

#correcting the second record
data_2021.loc[data_2021['Company/Brand']=='Godamwale',['Amount($)','Investor','Stage', 'Sector']]=[1000000, 'Unknown','Seed','Logistics']
# checking for Pre-series A in the amount column
data_2021.loc[data_2021['Amount($)']=='Pre-series A']
The invetsor, Amount, and stage columns has been interchanged. The Sector column will be renamed to be consistent with others. 

This is correcetd in the code below
data_2021.loc[data_2021['Company/Brand']=='AdmitKard',['Amount($)','Investor','Stage', 'Sector']]=[1000000, 'Unknown','Series A','Education']
#First change the data type to a string to work on the special characters
data_2021['Amount($)']=data_2021['Amount($)'].astype(str) 
# removing all special characters in the colum
data_2021["Amount($)"]=data_2021['Amount($)'].apply(lambda x: str(x).replace("$" ,"")) 
data_2021["Amount($)"]=data_2021['Amount($)'].apply(lambda x: str(x).replace("," ,""))
# change the data type to a float
# The downcast parameter is used to set the data type to float
data_2021['Amount($)'] = pd.to_numeric(data_2021['Amount($)'], errors='coerce', downcast='float') 
#replacing nan with median
data_2021['Amount($)']=data_2021['Amount($)'].fillna(data_2021['Amount($)'].median())
### Cleaning the `Founders` Column
# all nan values are to be filled with unknown on the 
data_2021['Founders']=data_2021['Founders'].fillna('Unknown')
### Cleaning the `Investor` Column
# all nan values are to be filled with unknown on the 
data_2021['Investor']=data_2021['Investor'].fillna('Unknown')
# checking for duplicate entries
data_2021.duplicated().value_counts()
# remove the duplicate record
data_2021=data_2021.drop_duplicates(keep='first')  
# checking for null values
data_2021.isna().value_counts()
# Combining the Various Data Sets `2018`, `2019`, `2020`, `2021` into one
# #creating a variable to hold the different dataframes
data=[data_2018,data_2019,data_2020,data_2021]
#concatenating the varibale holding the various dataframes
combined_data=pd.concat(data,ignore_index=True)
combined_data.head()
combined_data.shape
### Saving the combined data set as a new file and reloading it for analysis
#saving the new dataframe after concatenation in order to have it work on it without having to change the other datasets
combined_data.to_csv('combined_data.csv')
combined_data.head()
## Exploratory Data Analysis (EDA)
# dropping unwanted column 
combined_data.drop('Unnamed: 9',axis=1, inplace=True)
combined_data.head()
dfSummary(combined_data, is_collapsible = False)
This summary output revelas one duplicated entry after merging the four data sets as one. The duplicate has to be dropped.
    Since the initial 2018 data set did not have the `Founders` and `Investor` columns, it has been filled with `nan`. The cleaned records are identified with `Unknown`
# Confirming checks for duplicate entries
combined_data.duplicated().value_counts()
### Visually Inspecting Missing Data
plt.figure(figsize=(10,6))
sns.heatmap(combined_data.isna(),
            cmap="YlGnBu",
            cbar_kws={'label': 'Missing Data'})
The heatmap indicate a clustered missing values for the founders and Investors column. This is as a result of the 2018 data set not having values for this field before the data set merge.
## Univariate Analysis
### 1. Analysis on The `Amount($)` Column
combined_data['Amount($)'].head(20)
# calculate basic statistical measures
combined_data['Amount($)'].describe()
The above metrics are visually inspected with a box plot below
sns.boxplot(data=combined_data, x='Amount($)')
There are clear outliers in the amount column after which the rest of the values are clustered together
# calculate z-scores
z_score=scipy.stats.zscore(combined_data['Amount($)'])
z_score.head(20)
The z score is a type of statistical measurement that gives an idea of how far a raw score is from the mean of a distribution
# find the indices of the outliers
outliers_index = np.where(z_score >3)[0]
outliers_index
# remove the outliers
combined_data = combined_data.drop(combined_data.index[outliers_index])
sns.boxplot(data=combined_data, x='Amount($)')
There seem to be more outliers which has the potential of distorting the mean. The median will however be less affected
### 2. Analysis of the `Stage` Column
(combined_data["Stage"].value_counts(normalize=True)*100)
The high count of unnknowns is as a result of the 2018 data set. The info above is in percentage terms. This is graphically represented in the graph below
   fig=sns.catplot(y="Stage",data=combined_data, kind="count", height=4, aspect=1.5)
### 3. Analysis of the `Sector` Column
(combined_data["Sector"].value_counts(normalize=True)*100)
The FinTech sector has the highest percentage of 9.2%, followed by Healthcare 6.8% etc. This is also represented graphically below
   sns.catplot(y="Sector",data=combined_data, kind="count", height=12.7, aspect=1.2)
Since there are about 90 different sectors, it will not be ideal to represent them all on a graph as it will not be visually appeaing, there compromising on undesrtanding. Futher analysis will sort either the top 120 or the least 10 for analysis
### 4. Analysis of the Founded Column
  sns.catplot(y="Founded",data=combined_data, kind="count", height=6, aspect=1)
## Multivariate Analysis
### 1. Analysis of the `Founded` and `Amount($)` Columns
# Group the DataFrame by the 'Stage' column and count the occurrences of each stage
amt_stage_count = combined_data.groupby('Founded')['Amount($)'].count()
amt_stage_count.head()
sns.lineplot(data=combined_data,
            y='Founded',
            x='Amount($)'
            )
The graph shows that only a few start up from 2012 and beyond have received enough funding. Again the funding for start-up took a marginal increase since 2012 and has since be on a stable rise
### 2. Analysis of the `Amount($)` and the `Founded` Column
sns.regplot(x='Amount($)', y='Founded', data=combined_data, scatter_kws={'color':'red'}, line_kws={'color':'blue'})

The regression graph indicated a weak and negative relationship between year of funding and amount of funding
af_correlation = combined_data[['Amount($)', 'Founded']].corr()
sns.heatmap(af_correlation, annot=True)

### 3. Analysis of Stage and Founded Column
   fig=sns.catplot(y="Stage",x="Founded",data=combined_data, kind="strip", height=4, aspect=1.5)
It is observed that the seed stage has more spread across the various years as compared to the other stages. They are clustered between 2010 and 2021
### 4. Analysis of all other  Numeric Columns
sns.pairplot(combined_data, vars=None, hue='Stage', diag_kind='auto', markers='*', plot_kws=None, diag_kws=None, grid_kws=None, height=3.5,aspect=1.5)
## Answering The Business Questions
### Question 1. Does the sort of industry have an impact on funding success?
# Group the DataFrame by the 'sector' column and count the occurrences of each stage
sectoramt_count = combined_data.groupby('Sector')['Amount($)'].count().reset_index()

# # Sort the counts in descending order and select the top 10 values
top_count = sectoramt_count.sort_values(by='Amount($)', ascending=False).head(10)


# # Create the vertical bar chart
figure = plt.figure(figsize = (10, 5))
topgraph = sns.barplot(data=top_count, x='Amount($)', y='Sector',orient='h')

# Add labels and title
plt.ylabel("Sector")
plt.xlabel("Amount($)\n\n")
plt.title("Top 10 Sectors With Most Start-up Investment\n")
plt.show()

# Group the DataFrame by the 'sector' column and count the occurrences of each stage
sectoramt_count = combined_data.groupby('Sector')['Amount($)'].count().reset_index()

# # Sort the counts in descending order and select the last 10 values
bottom_count = sectoramt_count.sort_values(by='Amount($)', ascending=False).tail(10)

# # Create the vertical bar chart
figure = plt.figure(figsize = (10, 5))
bottomgraph = sns.barplot(data=bottom_count, x='Amount($)', y='Sector',orient='h')

# Add labels and title
plt.ylabel("Sector")
plt.xlabel("Amount($)")
plt.title("Bottom 10 Sectors With Most Start-up Investment\n")
plt.show()

gp=top_count = sectoramt_count.sort_values(by='Amount($)', ascending=False)

The two graph above confirms that the the sector or industry of the start-up can influence the funding it receives. Other varibales like the stage of the start-up might come to play. However, the sector affects the amount of funding received.

The first graph which showed the top 10 indicates that most of the funding were received in technology dominated sectors.
The second graph which show the bottom 10. indicates less difference in the sectors as most of them received similar funding
sns.catplot(data=gp, y='Sector',x='Amount($)', kind='bar', height=14, aspect=1.2)
### Question 2. Can the success of obtaining finance from investors be impacted by location?
# Group the DataFrame by the 'headquater' column and count the occurrences of each stage
hqamt_grp = combined_data.groupby('HeadQuarter')['Amount($)'].sum().reset_index()

# # Sort the counts in descending order and select the top 10 values
top_grp = hqamt_grp.sort_values(by = 'Amount($)', ascending = False).head(10)

# # Create the vertical bar chart
sns.barplot(data=top_grp, x='Amount($)', y='HeadQuarter',orient='h',)

plt.ylabel("Location")
plt.xlabel("Amount($)")
plt.title("Top 10 Location With Most Start-up Investment\n")
plt.show()

The graph supports the notion that the location of the start up can  affect its funding. 
### Question 3. Which stage receives more investment from investors for start-ups?
# Group the DataFrame by the 'Stage' column and count the occurrences of each stage
stageamt_grp = combined_data.groupby('Stage')['Amount($)'].sum().reset_index()

# # Sort the counts in descending order and select the top 10 values
stageamt_grp = stageamt_grp.sort_values(by = 'Amount($)', ascending = False).head(10)


figure = plt.figure(figsize = (10, 5))

# Create a bar chart using seaborn
sns.barplot(y='Stage', x='Amount($)', data=stageamt_grp)

# Add labels and title
plt.ylabel("Stage")
plt.xlabel("Amount($)")
plt.title("Top 10 Stages With Most Start-up Investment")

# Rotate y-labels by 30 degrees
#ax.set_yticklabels(ax.get_yticklabels(), rotation=30)


# set y ticks and labels
plt.xticks(rotation = 0)

# Show the plot
plt.show()

The seed stage has the highest start-up investment
### Question 4. Who makes the biggest investments among investors?

# Group the DataFrame by the 'investors' column and count the occurrences of each stage
invamt_grp = combined_data.groupby('Investor')['Amount($)'].sum().reset_index()

# # Sort the counts in descending order and select the top 10 values
invamt_grp = invamt_grp.sort_values(by = 'Amount($)', ascending = False).head(10)


figure = plt.figure(figsize = (10, 5))

# Create a bar chart using seaborn
sns.barplot(y='Investor', x='Amount($)', data=invamt_grp)

# Add labels and title
plt.ylabel("Investor")
plt.xlabel("Amount($)")
plt.title("Top 10 Investors with the Most Investment")

# Rotate y-labels by 30 degrees
#ax.set_yticklabels(ax.get_yticklabels(), rotation=30)


# set y ticks and labels
plt.xticks(rotation = 0)

# Show the plot
plt.show()
### Question 5. Can the startup's Year of founding affect the Amount of funding it receives from investors?
combined_data['Founded']=combined_data['Founded'].astype(str)
combined_data['Founded']=combined_data.Founded.str.split('.').str[0]

# Group the DataFrame by the 'investors' column and count the occurrences of each stage
famt_grp = combined_data.groupby('Founded')['Amount($)'].sum().reset_index()

# # Sort the counts in descending order and select the top 10 values
famt_grp = famt_grp.sort_values(by = 'Amount($)', ascending = False).head(10)


figure = plt.figure(figsize = (10, 5))

# Create a bar chart using seaborn
sns.lineplot(x='Founded', y='Amount($)', data=famt_grp)

# Add labels and title
plt.xlabel("Founded")
plt.ylabel("Amount($)")
plt.title("Top 10 Year Founded with the Most Investment\n")

# Rotate y-labels by 30 degrees
#ax.set_yticklabels(ax.get_yticklabels(), rotation=30)


# set y ticks and labels
plt.xticks(rotation = 0)

# Show the plot
plt.show()
Year founded was on a sinificant rise in 2019 after years of fluctuating runs. However it to a sharp decline in 2019 and re sparked in 2020. Again 2021 missed out on the top 10 as it was on a heavy decline
### Hypothesis Testing
# # Create a variable to hold the columns on which hypothesis is to be tested
# hypothesis = pd.crosstab(combined_data['Founded'], combined_data['Amount($)'])

combined_data['Founded']=combined_data['Founded'].astype(int)

# # Perform the t test
t,p=stats.ttest_ind(combined_data['Founded'],combined_data['Amount($)'])
print("P-value:", p)
print("T-value:", t)
since the p-value  is less than the the significant value of 0.05, the null hypothesis is rejected.

Please host your standalone notebook in this folder.