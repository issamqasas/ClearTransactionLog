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

###  Simple recovery model
from it's name , if you have this Model , you will never face an issue of transaction log grough , since basically you have only transaction logs for the current transactions.
once commited , the logs will be truncated.But this recovery model is not recomended for critical applications since data recovery only avialable through Data backup , not Logs Backup.
### Full Recovery model
in this Rercovery model , transaction logs will be growing untill the limit defined , or until filling the Hardsisk , the only solution is to make a logs backup, then transaction logs will be trancated.
- #**Note:-**
you can't take transaction log backup without first making a full data backup.
## steps to solve the issue
when we check our Database recovery  model , we found it **Full Recovery Model** so we will solve this matter
we have two solutions here :-
# Solution  1 :- logs data is not important

if you don;t need the logs , just do the following
1. change the database recovery model to simple
2. the transaction logs will be truncated automatically
3.  shrink the log file
4.  change the database recovery model to full again

```sql
        ALTER DATABASE testdb
	SET RECOVERY SIMPLE

        GO

        DBCC SHRINKFILE (testdb_log, 3)

        GO

        ALTER DATABASE testdb
	SET RECOVERY FULL
```

# Solution  2 :- logs data is important
if you need the logs then continue the steps below
### 1. take full data backup
- make a full data backup as shown below
![image](https://github.com/user-attachments/assets/ca99b589-7f31-4ee8-b655-559d484de21b)

or using the folloiwng script
```sql
   BACKUP DATABASE testdb TO DISK = 'C:\issam\data backup\testdb_usingscript.back'
```
![image](https://github.com/user-attachments/assets/e65b2d38-ea10-4984-8ba9-bbcd235fa588)
### 2. take transaction log backup
use this script 
```sql
BACKUP LOG testdb to disk= 'C:\issam\data backup\testdb_logs_usingscript.back'
```
![image](https://github.com/user-attachments/assets/af6fb0ef-e80c-4715-b0d0-ffd2222debec)

### 3. shrink transaction log
select the database 
then select the file type :- log
![image](https://github.com/user-attachments/assets/1b115d61-495f-4d9b-9230-f530e7011167)
 or using the script 
 ```sql
SELECT name FROM sys.master_files WHERE type_desc = 'LOG'

DBCC SHRINKFILE (AIMSPRODWB_log, 3)
        
```
#References
- https://www.mssqltips.com/tutorial/how-to-shrink-the-transaction-log/
- https://www.sqlshack.com/sql-server-transaction-log-backup-truncate-and-shrink-operations/




