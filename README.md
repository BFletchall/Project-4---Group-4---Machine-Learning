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

| Field                                   | Definition                                                                                | Relevant to Project? |
|-----------------------------------------|--------------------------------------------------------------------------------------------|----------------------|
| Assembly_MainPieceProductionCode        | Piece Category: Beam, Pipe, Door Plate, Backer Bar, Crane Grider, Bollard, Alum Pipe, Galv Stringer, etc. | Yes                  |
| Assembly_MainPartDimension             | Inches by pound                                                                           | Yes                  |
| Assembly_MainPartFinishDescr           | Abbreviation + finish of the part description                                             | Yes                  |
| Assembly_MainPartGrade                 | Grade of instance                                                                         | Yes                  |
| Assembly_MainPartLengthFt              | Part length                                                                               | Yes                  |
| Assembly_MainPartShape                 | Shape of instance                                                                         | Yes                  |
| Assembly_NumSmallParts                 | Number of things to assemble per instance (not per employee per instance)                 | Yes                  |
| Assembly_SurfaceAreaEachSqFt          | Part surface area                                                                         | Yes                  |
| Assembly_TotalQuantityInJob            | Quantity per instance                                                                     | Yes                  |
| Assembly_WeightEachLbs                | Part weight                                                                               | Yes                  |
| EPM_AdjustedStationName                | Use this column to identify which task of the instance it is on. One instance can contain multiple station names (or tasks). | Yes                  |
| EPM_ProductionControlItemID           | Unique identifier of a group of instances. Unique to job number and main mark.            | Yes                  |
| EPM_InstanceNumber                     | Instance Number - we want to find time spent per instance                                  | Yes                  |
| TimeInSeconds_ThisWorkSegment         | Time spent per instance in seconds - no need to calculate time from start and end, it done already | Yes, this is unit of time it takes per instance. TARGET OF MODEL |
| Assembly_EstTotalHours_ThisLaborGroup | Software Estimated time, in hours. This is what we are comparing to.                       | Yes, as this is current estimate of time being used by WWC to bid jobs, but needs to be converted to seconds. |
| EPM_MainMark                            | Indentifer of group of ControlItemIds, which then have a group of instances                | Yes, but is a descriptor that is not necessary to model |
| NumberWithDash                         | Project number. Unique to site/project. Will contain multiple Main Marks that have mutiple ControlItemIds which then have mutiple instances | Yes, but is a descriptor that is not necessary to model |
| AdjustedEnd                            | Not in use, so remove                                                                     | No                   |
| AdjustedStart                          | Not in use, so remove                                                                     | No                   |
| AS_EntityID                            | Remove                                                                                     | No                   |
| AS_ProjectID                           | AppServices Project ID - used in the software. Unique to each project. But use Project Number instead, this is not needed. | No                   |
| Assembly_MainPartFinishAbbrev         | Finish of the part abbreviated - do not use                                                | No                   |
| EPM_LaborGroupID                       | Not needed                                                                                | No                   |
| EPM_LaborGroupName                     | Do not use for model, use later                                                            | No                   |
| EPM_LaborGroupNumber                   | Do not use for model, use later                                                            | No                   |
| EPM_LotNumber                          | Not relevant                                                                               | No                   |
| EPM_PhaseNumber                        | Not relevant                                                                               | No                   |
| EPM_ProductionControlID                | Software Name Production Control ID. Not needed                                             | No                   |
| EPM_SequenceID                         | Lot sequence - break job down into parts, not relevant                                      | No                   |
| EPM_StationID                          | Not needed                                                                                | No                   |
| EPM_StationName                        | Remove                                                                                     | No                   |
| EPMItemStationInstanceSummaryWorkSegmentID | Remove                                                                                     | No                   |
| JobName                                | Name of the site or project they are working on                                             | No                   |
| STS_Barcode                            | Not needed                                                                                | No                   |
| TimeClcok_WorkSegmentEnd               | Remove                                                                                     | No                   |
| TimeClock_WorkSegmentStart             | remove                                                                                     | No                   |
| TimeClockWorkSegmentRecordID           | Remove                                                                                     | No                   |
| TimeInSeconds_AdjustedTime            | Not in use, so remove                                                                     | No                   |
| TimeInSeconds_TimeClockWorkSegment    | Don’t need                                                                                | No                   |
| TC_EmployeeID                          | Time clock worker id                                                                      | No - no PII          |
| AS_EmployeeName                        | Name of the worker                                                                        | No - no PII          |
| TimeStamp_End                         | End of the instance work                                                                   | No, calc'd in other column |
| TimeStamp_Start                       | Start of the instance work                                                                 | No, calc'd in other column |

Run the "[data_prep.ipynb](https://github.com/BFletchall/Project-4-Group-4-Machine-Learning/blob/main/data_prep.ipynb)" to generate final cleaned data file.
           

## **2. Database setup and loading**  
- Setup a SQLite database using sqlalchemy and stored the cleaned data file into the database to be utilized for running the model.
- Run the "[SQLite_database_creation.ipynb](https://github.com/BFletchall/Project-4-Group-4-Machine-Learning/blob/main/SQLite_database_creation.ipynb)" to generate SQLite database.

## **3. Model Design, Fitting & Training** 
We removed rows of any values that were blank or did not provide any data values that were meaninful to the model. 

| Column to Remove                        | Reason to remove                                         
|-----------------------------------------|----------------------------------------------|
| EPM_MainMark                            | Descriptor of Job, not impactful variable    |
| NumberWithDash                          | Descriptor of Job, not impactful variable    |       
| Assembly_EstTotalHours_ThisLaborGroup    | Outcome variable we'll compare against result of new model    |     

Featues & Target values were selected for the model:

| Column Included in Model                | Target or Feature?                                       
|-----------------------------------------|---------------------|
| EPM_ProductionControlItemID             | Feature             |
| EPM_InstanceNumber                  | Feature             |
| Assembly_MainPieceProductionCode     | Feature             |
| Assembly_MainPartLengthFt          | Feature             |
| Assembly_WeightEachLbs             | Feature             |
| Assembly_SurfaceAreaEachSqFt        | Feature             |
| Assembly_MainPartShape               | Feature             |
| Assembly_MainPartDimension           | Feature             |
| Assembly_MainPartFinishDescr          | Feature             |
| Assembly_TotalQuantityInJob          | Feature             |
| Assembly_NumSmallParts                | Feature             |
| EPM_AdjustedStationName                 | Feature             |           
| TimeInSeconds_ThisWorkSegment          | Target          |

Additional Model prep steps included:
- Encoding Categorical Variables
We used the get dummies to ensure all the data was in trainable format to encode them.

- Train test splitting was done to the data so that the model can have training and testing data

Find this work in lines 1 to 15 of "[linear_regression_model_FINAL.ipynb](https://github.com/BFletchall/Project-4-Group-4-Machine-Learning/blob/main/models/linear_regression_model_FINAL.ipynb)". 

## **4. Model, Build Evaluation & Optimizations:**  
Team experimented and optimized three models:

- Linear Regression Model was executed, but data was found to have no linear relationship.
  
  <img src="https://github.com/BFletchall/Project-4-Group-4-Machine-Learning/blob/main/models/actual_vs_predicted.png" alt="Linear Regression Results" width="50%">

   - Find the code for this model in "[linear_regression_model_FINAL.ipynb](https://github.com/BFletchall/Project-4-Group-4-Machine-Learning/blob/main/models/linear_regression_model_FINAL.ipynb)". 
 
- Random Forest Regression Model was tried next as it can capture non-linear relsationships between variables.
   - Optimizations were also applied to the model including:
   - Binning of "Assembly_MainPieceProductionCode" to only include the 50 most commonly used variables, as this would simfily the complexity of the model and hopefully identify a pattern to predict time per Job.
   - However, optimization and design of Random Forest Model did not result in 80% accuracy.
   - Find the code for this model version in "[random_forest_regression_withoutestimate-FINAL.ipynb](https://github.com/BFletchall/Project-4-Group-4-Machine-Learning/blob/main/models/random_forest_regression_withoutestimate-FINAL.ipynb)"

    
 <img src="https://github.com/BFletchall/Project-4-Group-4-Machine-Learning/blob/main/models/RDM_actual_vs_predicted.png" alt="Linear Regression Results" width="50%">

 
- A revised Random Forest Model was executed again, but this time the Software Estimate time was included in model to deteremine if it would help improve accuracy.
   - Converted "Assembly_EstTotalHours_ThisLaborGroup" to seconds to ensure it aligned with actual job time field "TimeInSeconds_ThisWorkSegment".
   - Included same binning strategy from first Random Forest Model design
   - Results drastically improved, with the result improving to 69% accuracy
   - Find the code for this model version in "[random_forest_regression_withestimate-FINAL.ipynb](https://github.com/BFletchall/Project-4-Group-4-Machine-Learning/blob/main/models/regressionmodel_withestimate-FINAL.ipynb))"
 <img src="https://github.com/BFletchall/Project-4-Group-4-Machine-Learning/blob/main/models/RDM_withSoftware_actual_vs_predicted.png" alt="Linear Regression Results" width="50%">

## **5. Model Selection:**  
Summary of Model Results:

| Model | Features | Target | Model Evaluation | Model Selection |
|-------|----------|--------|------------------|----------------|
| Linear Regression | - ProductionControlItemID <br> - InstanceNumber <br> - MainPieceProductionCode <br> - MainPartLengthFt <br> - WeightEachLbs <br> - SurfaceAreaEachSqFt <br> - MainPartShape <br> - MainPartDimension <br> - MainPartFinishDescr <br> - TotalQuantityInJob <br> - NumSmallParts <br> - AdjustedStationName | TimeInSeconds (Actual) | R^2 Score: -2.612 <br> Mean Absolute Error: 3.853 | Not recommended |
| Random Forest without Software Estimate | - ProductionControlItemID <br> - InstanceNumber <br> - MainPieceProductionCode <br> - MainPartLengthFt <br> - WeightEachLbs <br> - SurfaceAreaEachSqFt <br> - MainPartShape <br> - MainPartDimension <br> - MainPartFinishDescr <br> - TotalQuantityInJob <br> - NumSmallParts <br> - AdjustedStationName | TimeInSeconds (Actual) | R^2: -0.002 <br> MAE: 1465.566 | Not recommended |
| Random Forecast with Software Estimate | - ProductionControlItemID <br> - InstanceNumber <br> - MainPieceProductionCode <br> - MainPartLengthFt <br> - WeightEachLbs <br> - SurfaceAreaEachSqFt <br> - MainPartShape <br> - MainPartDimension <br> - MainPartFinishDescr <br> - TotalQuantityInJob <br> - NumSmallParts <br> - AdjustedStationName <br> - EstTotalSeconds (from Software) | TimeInSeconds (Actual) | R^2: 0.691 <br> MAE: 1854.615 | Best option with highest accuracy score |

## **6. New Model v. current Software Estimator Tool:** 
- New Model does not improve upon current estimates provided by WWC Software
 <img src="https://github.com/BFletchall/Project-4-Group-4-Machine-Learning/blob/main/models/Software_actual_vs_predicted.png" alt="Linear Regression Results" width="75%">

- Review of Feature Importance indicate Software Estimate is the top contributor to predicting job time
 <img src="https://github.com/BFletchall/Project-4-Group-4-Machine-Learning/blob/main/models/Top12Features.png" alt="Linear Regression Results" width="75%">

## **7. FInal Conclusions:** 
- Model build project did not result in model with minimum of 80% accuracy
- WWC should continue to use Software Estimator Tool while the new data collection process is put in place, as the current most important factor in predicting time for jobs is the Software Estimate 
- Once additional data is collected, ensure it reflects only completed jobs to make sure time reflect completed jobs v. jobs still in progress
- Try the model build exercise again with updated data

    
