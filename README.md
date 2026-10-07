# Insurance Project SQL+PowerBI
Demontsrating pipeling from transforming dataset in MySQL Workbench and transporting it to PowerBI with further analysis and role modelling

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
#### The goal now would be to create a generla report with overall information about customers, policies, ages to then further enable filtering on the page.
- Step 9: Created three slicers "PolicyNumber", "ClaimNumber" and "CustomerID" to further filter data on the page.
- Step 10: Created three cards "Sum of PremiumAmount", "Sum of Coverage Amount" and "Sum of Claim Amount".
- Step 11: Created a Ribbon Chart with "Claim Status".
- Step 12: In order to add specific age groups according to "ClaimAmount" for the general report, in Power Query, created conditional column "Age Group":
```sql
if "Age" <= 24 then "Young Adult",
elif "Age" <= 60 then "Adult,
else "Elder"
```
- Step 13: Changed datatype to text for the column.
- Step 14: Created a line chart with "Age Group" and "Sum of ClaimAmount".
- Step 15: Created a bar chart with "PolicyType" and "Sum of Premium Amount".
- Step 16: Created a Matrix with "PolicyType", "ClaimStatus" and "Sum of Coverage Amount".
- Step 17: Finally, to further find inactive customers, created a conditional column "Active/Inactive" with categories "Active" and "Inactive" according to PolicyEndDate, where:
```sql
if PolicyEndDate<=2024/12/10, then "Inactive",
else "Active"
```
Step 18: Changed datatype to text for the created column.
Step 19: Now the column can be used for the donut chart.
Step 20: Placed all visualisations on the same page for further compact filtering where inputa according to needs can affect all visuals.

#### General overview:
<img width="602" height="337" alt="overall" src="https://github.com/user-attachments/assets/8f08f3d0-afc0-41ce-8757-5898b60c3965" />

#### After choosing "Auto" Policy:
<img width="604" height="334" alt="auto" src="https://github.com/user-attachments/assets/74598711-66a2-46fd-91f4-c774c27d19c4" />

#### After choosing "Auto" Policy and "Ender" Age Group:
<img width="602" height="335" alt="auto+elder" src="https://github.com/user-attachments/assets/2f0a338d-1ecd-420c-8855-30e1f2550216" />

- Step 21: To further filter data, on seperate page created a Table visual with all columns from the dataset.
- Step 22: In a drill-through section added "PolicyType".
- Step 23: Now, when chose "Auto" in PolicyType visualisation on the first page and clicked on a detailed view, fgor transferred to the second page with all information "Auto" Policy with customer data, claims infortmation, dates etc:
<img width="577" height="335" alt="drill through" src="https://github.com/user-attachments/assets/0acef38a-99e9-4633-a000-d758c75ffec1" />


### Modelling:
- Step 24: To maintain Data governance, created 2 roles:
- Step 25: in Manage roles under Modelling created a "Health Role", where "PolicyType" is set for "Health", and for "Travel Role" "PolicyType" is set "Travel".

#### Now the data is ready for filtering according to the needs of teams within company maintaining access to data:
<img width="611" height="336" alt="health role" src="https://github.com/user-attachments/assets/ebcec70d-1dc8-4dde-856c-13848085cfee" />

#### After uploading the report to PowerBI service, in Security section of the dashboard, emails have to be added for each role created.

