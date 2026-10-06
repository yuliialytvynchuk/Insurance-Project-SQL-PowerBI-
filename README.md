# Insurance-Project-SQL-PowerBI-
Demontsrating pipeling from transforming dataset in MySQL Workbench and transporting it to PowerBI with further analysis

## Steps followed:

### Data Transformation in MySQL Workbench

- Step 1: Created a database InsuranceDB.
- Step 2: Loaded data from a csv file to the database (encountered issues changing the datatype from text to date while loading the data).
- Step 3: Previewed the database:
```sql
SELECT * FROM insurancedata;
```
- Step 4: Proceeded to changing the datatype from text to date for one of the columns:
```sql
ALTER TABLE insurancedata
ADD COLUMN PolicyStartDateNew DATE;
UPDATE insurancedata
SET PolicyStartDateNew = STR_TO_DATE(PolicyStartDate, '%d-%m-%Y');
ALTER TABLE insurancedata
DROP COLUMN PolicyStartDate;
ALTER TABLE insurancedata
RENAME COLUMN PolicyStartDateNew TO PolicyStartDate;
```
- Step 5: Did the same also for PolicyEndDate and ClaimDate columns.
- Step 6: Now all datatypes are correct.

###  Working on the dataset in PowerBI

- Step 7: Loaded the dataset from MySQL Workbench to PowerBI (Get Data - More - MySQL Datasets).
- Step 8: Chose "Transform Data" and checked "Column distribution", "Column quality" and "Column profile" options for 4 tables present in the Dataset to see detailed overview of each column.
- 


- Step : To create age groups, in Power Query, created conditional column "Age Group":
```sql
if "Age" <= 24 then "Young Adult",
elif "Age" <= 60 then "Adult,
else "Elder"
```
- Step : Changed datatype to text for the column.
