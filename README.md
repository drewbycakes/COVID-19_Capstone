![alt text](https://github.com/drewbycakes/COVID-19_Capstone/blob/main/Images/COVID_Title.png?raw=true)

## Technical Summary

In this project, I create a predictive classification model geared towards a hospital's analytics team seeking to make informed decisions for resources and personnel allocation based on predicted inpatient length of stay. The target variable is classified into three labels: `"Short-Term"`, `"Medium-Term"`, and `"Long-Term"` based on the number of days a patient is predicted to stay. The model uses fourteen features to predict the length of stay -- with `Type of Admission`, `Severity of Illness`, `Bed Grade`, and `Ward Type` having the most influence. The final model produced is a tuned `XGBoost model` that is able to predict target labels with `54% accuracy`. With further research and implementation, classifiers like these could be used in emergency situations to properly allocate resources based on predictive knowledge of how long a patient is expected to admitted for.

## The Motivation

It goes without saying that the onset of the COVID-19 virus has had a drastic impact on the world as we know it today. At the beginning of the pandemic, hospitals were overrun with patients needing medical attention and it became clear that hospital infrastructure was not equipped for this unprecedented event.

![alt text](https://github.com/drewbycakes/COVID-19_Capstone/blob/main/Images/Nurses_2.png?raw=true)

The motivation for this project came from a conversation I had with my roommate who is a nurse here in San Francisco who worked on a COVID unit during the pandemic. She told me that one of the biggest factors leading to inadequate care was the lack of hospital resources. Without proper resources, sick people were not able to receive proper care. One of major contributing factors to this came from not knowing how long a patient would be hospitalized for when they are admitted. With such a huge influx of more long-term residential patients, hospitals were quickly drained of resources such as PPE, certain medications, and even beds themselves. 

Predictive modeling has many applications in the healthcare field and I wanted to use this project as an opportunity to show how machine learning could be used to help alleviate the burden of this type of issue in the future. In this project, I build a classifier model that can be used to predict the length of stay (LOS) of a COVID-19 patient given certain predictive features. With further research and implementation, classifiers like these could be used in emergency situations to properly allocate resources based on predictive knowledge of how long a patient is expected to admitted for. 

## Business Understanding
 
**Stakeholder**: Analytics team for a group of hospitals looking to predict length of stay (LOS) estimates for COVID-19 inpatients. 

**Problem**: Effectively predict the length of stay of a COVID-19 patient given similar past patient profiles in order to better allocate hospital resources and personnel. 

**Target**: Length of stay (LOS)
* Length of stay is defined as a categorical variable in this dataset broken into 10 day increments.
* In order to fit with my model, this was encoded using a LabelEncoder as follows:
    
| Label | # of Days |
| --- | --- |
| 0 | 0-10 Days |
| 1 | 11-20 Days |
| 2 | 21-30 Days |
| 3 | 31-40 Days |
| 4 | 41-50 Days |
| 5 | 51-60 Days |
| 6 | 61-70 Days |
| 7 | 71-80 Days |
| 8 | 81-90 Days |
| 9 | 91-100 Days |
| 10 | >100 Days |

![alt text](https://github.com/drewbycakes/COVID-19_Capstone/blob/main/Images/LOS_count.png?raw=true)

We can see that the vast majority of patients are hospitalized for less than 60 days. In the context of my problem, I want to help hospitals allocate resources and personnel effectively. In doing so, it may not be necessary to have 10 different prediction labels and instead am going to separate into three types of stay: 

| Label | Type of Stay | # of Days |
| --- | --- | --- | 
| 0 | Short-Term | 0-20 Days |
| 1 | Medium-Term | 21-50 Days |
| 2 | Long-Term | > 51 Days |

* **Short-Term**: less than 20 days
    * Quick turnaround for hospital 
    * Less personnel & resources 
    * Lower risk for long-term complications and/or death 

* **Medium-Term**: 21 to 60 days 
    * More intensive care
    * Moderate use of personnel & resources 
    * Increased risk for long-term complications and/or death

* **Long-Term**: more than 60 days 
    * Most intensive care 
    * Heavy use of personnel & resources 
    * Significant risk for long-term complications and/or death

## Data Understanding

My data came from a series of analytic healthcare data provided on Kaggle by Vidhya Healthcare Analytics to be used as a means of building and sharing predictive models for COVID-19 related projects. The dataset can be found linked in my repository. 

The data was collected anonymously in order to protect the identity of the individuals and hospitals.

The dataset was originally comprised of 18 column features including my target `Stay` as well as over 300,000 sample patient data points. The features primarily consisted of coded geographical information and admission information. The features are described here: 

| Feature | Description | Notes |
| --- | --- | --- |
| case_ID | Case ID registered in Hospital | 
| Hospital_code | Unique code for the Hospital | Codes 1-30 for 30 Hospitals included in data |
| Hospital_type_code | Unique code for the type of Hospital | Codes a-g for 7 types of hospitals included (e.g. general, research, etc.) |
| City_Code_Hospital | City Code of the Hospital | Codes 1-13 for 13 cities included in data |
| Hospital_region_code | Region Code of the Hospital | Codes X, Y, Z for three regions included in data |
| Available Extra Rooms in Hospital | Number of extra rooms available in the Hospital | 
| Department | Department overlooking the case | 
| Ward_Type | Unique code for the Ward type | Unique code P-U for type of Ward |
| Ward_Facility_Code | Unique code for the Ward facility | Unique code A-F for Ward facility |
| Bed Grade | Condition of bed in the Ward | Scale grade 1-4 |
| patientid | Unique patient ID number | 
| City_Code_Patient | City code for the patient | Codes 1-38 for 38 cities included in data |
| Type of Admission | Admission type registered by the Hospital | Categorized as Trauma, Emergency, or Urgent |
| Severity of Illness | Severity of the illness recorded at the time of admission | Categorized as Minor, Moderate, or Extreme |
| Visitors with Patient | Number of visitors with the patient | 
| Age | Age of patient |
| Admission_Deposit | Deposit made at time of admission |

Since I intended my model to be predictive, I removed features that were purely descriptive (i.e. `case_ID`, `patientid`) or were determined after the patient was released (i.e `Visitors with Patient`.) I also removed `Hospital_region_code` since `City_Code_Hospital` would provide more detailed geographic importance.  

The data was split into categorical and numerical columns. Data was checked for duplicates, missing values, and  I used a `LabelEncoder()` for the categorical variables and scaled the numerical values using `StandardScaler()` for modeling purposes. 

## Modeling 

**Model 1**: Decision Tree Classifier
* Cross-validated to predict the target class with `44.1% Accuracy`.

**Model 2**: Random Forest Classifier
* Cross-validated to predict the target class with `43.8% Accuracy`.

**Model 3**: XGBoost Classifier 
* Cross-validated to predict the target class with `54.2% Accuracy`. 

**Final Model**: Tuned XGBoost Classifier 
* Able to predict the target class with `54.4% Accuracy`.

The feature importance for the final model is shown here: 

![alt text](https://github.com/drewbycakes/COVID-19_Capstone/blob/main/Images/Feature_Importance.png?raw=true)

The features that most influenced the model were `Ward Type`, `Type of Type`, `Severity of Illness`, and `Bed Grade`. 

## Evaluation & Conclusions

In the context of my Business Problem, I am happy with a 54.6% Accuracy. My model is intended to help hospitals make informed data-driven decisions on how to best allocate their resources and personnel during time of high occupancy such as COVID-19. Being able to better separate "Short-Term", "Medium-Term", and "Long-Term" patients will allow hospitals to more effectivly care for their patients. 

Based on the modeling and feature importance, the visuals below show how the model can be implemented in a hospital setting: 

![alt text](https://github.com/drewbycakes/COVID-19_Capstone/blob/main/Images/Key_Indicators.png?raw=true)

Upon deployment, the following steps would be followed: 

**1. Admission** 
* Upon admission, patient profile would be used to predict "Short-Term", "Medium-Term", or "Long-term" stay based on key predictors. 

**2. Placement**
* Based on classification, the patient could then be placed in a section of the hospital dedicated to patients with a similar profile. 

**3. Treatment & Discharge**
* Resources and personnel can be allocated in an appopriate manner in order to best meet patient needs and expedite the discharge process.

![alt text](https://github.com/drewbycakes/COVID-19_Capstone/blob/main/Images/Model_Deployment.png?raw=true)

With more patients placed in better hospital settings, they will be able to provide more appropriate healthcare services and expedite the discharge process. This will make room for additional patients who need treatment without overwhelming the current system.

## Future Research

The data lent itself to a nice analysis of Length of Stay in the context of the problem I set out to work on. In addition to the data provided, I would be interested to see how other factors play a role in LOS predictions. In particular, I am interested in:

![alt text](https://github.com/drewbycakes/COVID-19_Capstone/blob/main/Images/Future_Research.png?raw=true)

**1. Vaccinations**
* Did the individual recieve a vaccination?
* What type? 
* How many doses? 

**2. Variants** 
* Does being infected with the Delta or Omicron variant affect a LOS prediction? 

**3. Regionality**
* Does being in an urban/rural area affect LOS prediction? 
* Does community income level play a role? 

**4. Hospital Information**
* Does hospital funding play a significant role? 
* What resources are available to begin with?

## Project Directory

|── [.ipnyb_checkpoints](https://github.com/drewbycakes/COVID-19_Capstone/tree/main/.ipynb_checkpoints)
<br>
|── [images](https://github.com/drewbycakes/COVID-19_Capstone/tree/main/Images) 
<br>
|── [.gitignore](https://github.com/drewbycakes/COVID-19_Capstone/blob/main/.gitignore) 
<br>
|── [COVID-19_Predictor_FINAL.ipynb](https://github.com/drewbycakes/COVID-19_Capstone/blob/main/COVID-19_Predictor_FINAL.ipynb) 
<br>
|── [data](https://github.com/drewbycakes/COVID-19_Capstone/tree/main/Data) 
<br>
└── [README.md](https://github.com/drewbycakes/COVID-19_Capstone/blob/main/README.md)
