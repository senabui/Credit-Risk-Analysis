# Credit-Risk-Analysis
## Designed and implemented a comprehensive data processing and analysis pipeline for credit risk analysis. This project integrates multiple technologies to build an end-to-end big data solution. 

The dataset contains 74 columns of customer credit history including member id, loan amount, funded amount, interest rate, installments, loan grade definitions, home ownership status, annual income, verification status, loan status, customer inquiries, late payments, and so on. 

Data cleaning was performed using Pyspark before throwing into Athena to query. A total of 37 columns unnecessary for our analysis were removed. 

<img src="Images/Removed columns 1.png" width=900>
<img src="Images/Removed columns 2.png" width=900>

##### Data Transformation
Looked deeper into transformations for my categorical columns of ‘grade’, ‘emp_length’, ‘home_ownership’, 'verification_status’, and ‘purpose’. I combined similar default indicators together and excluded values such as ‘Current’,
‘Issued’, and ‘In Grace Period’ from 'loan_status' so that I can focus on definitive loan outcomes. This will provide a clearer signal for default prediction. I used binary encoding where 1 = Default/High Risk and 0 = Non-Default/Low Risk. Further transformations were made after I had queried and loaded up my data into a Spark dataframe including standardizing our numeric features using StandardScaler, converting my vectors from my categorical columns into actual python arrays, and then converting them back to the proper vectors for my ML processing, and checking for class imbalance and then applying weights to level out the class imbalance in binary ‘loan_status’. 

##### Data Merging & Aggregation
I then stored my final transformed and preprocessed dataframe df_final to an S3 bucket in parquet format. This data was also kept in an external table in Athena and integrated into my workflow to facilitate effective querying, and to load up the data for data analysis and machine learning later on. After quering the final preprocessesed and cleaned dataset from Athena, I then read the query results directly into a Spark dataframe to proceed with the machine learning model development. 

##### Machine Learning Model Development
Spark MLlib was used to predict whether a customer would default or  not. I used Random Forest as the machine learning model as it does not overfit easily with the proper tuning and provides feature importance for interpratibility. 

##### Evaluation & Results
I evaluated my model’s performance using ROC AUC, PR AUC, F1 score, and accuracy metrics. For a credit risk model, I believe the overall performance to be moderate. My ROC AUC (0.7040) is acceptable but not excellent, I would prefer it to be above 0.80+ at least. With my PR AUC at 0.4021, it suggests that my model still struggles with correctly identifying defaulters despite applying class weights to 'loan_status'. Both the F1 score (0.6689) and accuracy (0.64-04) are reasonable but still can be improved.

<img src="Images/Confusion Matrix Credit Risk Analysis.png" width=650>
<img src="Images/Predicted Probabilities by Class .png" width=900>

##### Architecture Diagrams
Dataflow through our system.
<img src="Images/Architecture diagram 1.jpeg" width=900>

Full architecture running the pipeline on a specified cadence, incorporating tools for orchestrating our pipeline.
<img src="Images/Architecture diagram 2.jpeg" width=900>

