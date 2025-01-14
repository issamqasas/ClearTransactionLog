# ClearTransactionLog
sql database log file is increasing 
## Introduction
the purpose of this artivcle is not to educate you about the importance of logs , or how it works , you can find more details in the references below , 
the main pupose is to solve a common issuse found in your work environment in SQL Server , and how to solve it quickly.

## MainIssue
as you can see the Database log seems to be higher that the Data itself, according to this issue , the transaction file should be increase either to Harddisk limit and shutdown the DB , 
or untill it reachs **2097 GB**
![image](https://github.com/user-attachments/assets/4ade4f16-6e7c-4e1b-9551-c94bd573eabc)
## Recovery  Models
- before solving such an issue , you have to check your **Database Recovery Model** , since the solution depends on your Database Recovery Model
- When you create a new Database , your recovery model will be by default the same Recovery model defined in the System **model** DB 
![image](https://github.com/user-attachments/assets/38de1861-af3d-451a-b838-327f8c0a95ba)
- you can check and change your DB Recovermodel by selecting rightclick the Database and select **properties** the select **option** from the left menu and you can see the recover model in right panel

###  Full recovery model



