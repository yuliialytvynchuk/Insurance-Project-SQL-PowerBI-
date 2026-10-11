# Insurance Data Analysis | MySQL, PowerBI
This project demonstrates an end-to-end data analytics workflow, from data preparation in MySQL Workbench to interactive reporting, drill-through analysis, and role-based data access in Power BI.

## Steps followed:

### Data Transformation in MySQL Workbench

- Step 1: Created a database InsuranceDB.
- Step 2: Imported data from a CSV file into the database. During the import process, encountered issues converting date columns from text to the DATE data type.
- Step 3: Previewed the imported data to check the contents of the insurancedata table::
```sql
SELECT * FROM insurancedata;
```
- Step 4: Converted the PolicyStartDate column from text to DATE by creating a temporary replacement column, converting the values, and replacing the original column:
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
- Step 5: Applied the same conversion process to the PolicyEndDate and ClaimDate columns.
- Step 6: Now all datatypes are correct.

###  Working with the dataset in PowerBI

The goal was to create a general insurance report covering customers, policies, claims, and age groups, with interactive filters that allow users to explore the data according to their needs.

- Step 7: Connected Power BI Desktop to the MySQL database (Get Data - More - MySQL Datasets) and loaded the dataset.
- Step 8: Opened Power Query Editor and, under the View tab in the Data Preview section, enabled Column Distribution, Column Quality, and Column Profile for the four tables to inspect data quality and column distributions.
- Step 9: Created three slicers PolicyNumber, ClaimNumber and CustomerID to filter the report by policy, claim, or customer.
- Step 10: Created three card visuals to display the sum of PremiumAmount, CoverageAmount, and ClaimAmount, providing a high-level overview of the insurance data.
- Step 11: Created a ribbon chart using Claim Status to compare the relative ranking of claim statuses across the chart's category axis.
- Step 12: Created a conditional column named Age Group in Power Query to group customers according to age:
```sql
if "Age" <= 24 then "Young Adult",
elif "Age" <= 60 then "Adult,
else "Elder"
```
- Step 13: Changed the data type of the new Age Group column to text.
- Step 14: Created a line chart with Age Group and Sum of ClaimAmount.
- Step 15: Created a bar chart using PolicyType and Sum of Premium Amount to compare claim amounts across the age groups.
- Step 16: Created a matrix visual with PolicyType, ClaimStatus and Sum of Coverage Amount to compare coverage amounts across policy types and claim statuses.
- Step 17: Created a conditional column Active/Inactive to classify policies based on their end dates, using 10 December 2024 as the reference date:
```sql
if PolicyEndDate<=2024/12/10, then "Inactive",
else "Active"
```
Step 18: Changed datatype to text for the created column.
Step 19: Used the new column in a donut chart to visualise the distribution of active and inactive policies.
Step 20: Arranged all visualisations on one report page so that users could interact with the slicers and explore how the selected filters affected the visuals.

#### General overview:
<img width="602" height="337" alt="overall" src="https://github.com/user-attachments/assets/8f08f3d0-afc0-41ce-8757-5898b60c3965" />

#### After selecting Auto Policy:
<img width="604" height="334" alt="auto" src="https://github.com/user-attachments/assets/74598711-66a2-46fd-91f4-c774c27d19c4" />

#### After selecting Auto Policy and Ender Age Group:
<img width="602" height="335" alt="auto+elder" src="https://github.com/user-attachments/assets/2f0a338d-1ecd-420c-8855-30e1f2550216" />

- Step 21: Created a separate report page containing a table visual with the available dataset columns to display detailed records.
- Step 22: Added PolicyType to the drill-through filters on the detail page.
- Step 23: Enabled drill-through from the main report. By selecting Auto in the PolicyType visual and navigating to the detail page, users can view the corresponding records, including customer details, claims information, and dates:
<img width="577" height="335" alt="drill through" src="https://github.com/user-attachments/assets/0acef38a-99e9-4633-a000-d758c75ffec1" />


### Modelling:
- Step 24: Created two roles in Power BI to demonstrate role-based data access and support data governance.
- Step 25: In Manage roles under Modelling section, created a Health Role (configured a filter on PolicyType to display records where the value is Health) and a Travel Role (configured a filter on PolicyType to display records where the value is Travel.

#### The report can now be configured to display different sets of data according to the user's assigned role.
<img width="611" height="336" alt="health role" src="https://github.com/user-attachments/assets/ebcec70d-1dc8-4dde-856c-13848085cfee" />

#### After publishing the report to Power BI Service, the roles can be tested and users can be assigned to the appropriate roles through the report's semantic model security settings.

