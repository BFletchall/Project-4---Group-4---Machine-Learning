# Project-4-Group-4-Machine-Learning

<img src="https://github.com/BFletchall/Project-4-Group-4-Machine-Learning/blob/main/constructionimage.jpg" alt="GitHub Project Image" width="50%">


## Purpose

Wonder Woman Construction Company Inc currently bids for jobs based on an industry software program that calculates time via tasks. The task times used by the software are set based on historical company data, and WWC Inc. would like to build a tool that uses their current company task time benchmarks to estimate the bids. 

WWC has not attempted this task before because they did not have a robust data set of historical time spent per job. A new data collectiont tool was recently put in place at WWC, and this new data will be used to build a new model to estimate Job Times. If the new model build is successful, WWC could then estimate bids base on their current company data versus outdated historical data. 

 **The key questions this Analysis will answer are:**  
 - Is the new data collection tool able to provide enough historical data for a new model to be built?
 - What are the most important factors for estimating job time, and is the new data collection tool able to provide it?
 - Can a new model with a minimum of 80% accuracy be created from the current historical data? 
- How should WWC Inc. change their bid strategy based on this data? 

Below are the steps taken to setup and run the model.   

## **1. Collection and Data Preparation**
We prepared the collected data by revieweing all fields available via the new WWC data collection tool, and included only relevant fields. 

Run the "[data_prep.ipynb]([link_to_your_page](https://github.com/BFletchall/Project-4-Group-4-Machine-Learning/blob/main/data_prep.ipynb))" to generate final cleaned data file.
           

## **2. Database setup and loading**  
- Setup a SQLite database using sqlalchemy and stored the cleaned data file into the database to be utilized for running the model.

## **3. Data Preprocessing** 
Some of the things that we did to Preprocess the data so that its ready for training and modeling include:
- Handling missing values  
We removed rows of any values that were blank or did not provide any data values that were meaninful to the model
- Binning 
We reduced the complexity of the data by categorizing data based on Assembly main piece production code by having a cut off value and grouping values that were less than 50. This helped take care of any outlier jobs that occur occasionally and may have skewed the model
- Encoding Categorical Variables
We used the get dummies to ensure all the data was in trainable format to encode them.   
- Train test splitting was done to the data so that the model can have training and testing data  

## **4. Feature Model Selection & Model Training**  
- We used several models to determine the best model for the data i.e.  
    - Linear regression
    - Random Forest Regression Modeling
    - Random Forest Classifier

## **5. Model Evaluation:**  
We used several ways of evaluating the model for accuracy:  
- Random Forest Classifier accuracy 
- Random Forest Regression Score
- Random Forest Mean Absolute Error
- Confusion Matrix

## **6. Model Intepretation & Analysis:**  
The Analysis of the model 

## **8. Documents Created and Updated:**
 
- **data_prep.ipynb** [(Link)]  
    Contains the code of the initial ingestion of the data and initial data cleanup. 
- **SQLite_database_creation.ipynb**  
 Contains the code to setup the database and load data into the database  contains the following new documents [(Link)]  
    
 - **data Folder** contains the following new documents [(Link)]
    - **AppServices_AllClaimingWorkSegments.xlsx** = initial data collection file 
    - **combined_data.csv** = output of the initial data cleaning
    


- **models** contains the following new documents [(Link)]: 
    - **claimed_time.db** = The database file 
    - **linear_regression_model.ipynb** = regression model file  
    - **random_forest_regression_model.ipynb** = First random forest regression optimization file
    - **random_forest_regression_model2.ipynb** = Second random forest regression optimization file 
- **analysis folder**  
 Contains the analysis of the model  optimization file
    
