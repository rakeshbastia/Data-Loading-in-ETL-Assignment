## Question 1: Data Understanding
# Identify all data quality issues present in the dataset that can cause problems during data loading.
# Answer:

There are several data quality issues in the given dataset:
1. Duplicate Order_ID values are present. For example, O101, O120, and O138 appear more
than once.
2. Missing Sales_Amount values are present in records such as O102, O108, O117, O126, and
O134.
3. Invalid numeric values are present in Sales_Amount, such as:
o Three Thousand
o Five Thousand
o Seven Thousand
4. Inconsistent date formats are used. Some dates are written as 12-01-2024, some as
2024/01/18, and some as 31-01-24.
5. Because of these issues, some records may fail during loading or may produce incorrect
results after loading.
The dataset contains these mixed values and formats in the supplied records.

## Question 2: Primary Key Validation
  **Assume Order_ID is the Primary Key.
  a) Is the dataset violating the Primary Key rule?**
# Answer:

Yes. The dataset violates the Primary Key rule because a primary key must contain unique
values, but some Order_ID values are repeated.
b) Which record(s) cause this violation?
Answer:
The repeated Order_ID values are:
• O101 - appears more than once with the same transaction details.
• O120 - appears more than once with the same transaction details.
• O138 - appears twice, but the Sales_Amount and Order_Date are different.
The records for O101 and O138 can be seen in the dataset, while O120 also appears more than
once.

**Question 3: Missing Value Analysis
Which column(s) contain missing values?
Answer:**

The Sales_Amount column contains missing values.
a) List the affected records
The affected records are:
Order_ID Customer_ID Sales_Amount Order_Date
O102 C002 NULL 15-01-2024
O108 C008 NULL 2024/01/28
O117 C017 NULL 06-02-2024
O126 C026 NULL 15-02-2024
O134 C034 NULL 23-02-2024
These missing Sales_Amount values are visible in the supplied dataset.
b) Explain why loading these records without handling missing values is risky.
Answer:
Loading records with missing Sales_Amount can create problems because sales calculations may
become incomplete or incorrect.
For example, if these values are used to calculate total sales, the database or BI tool may ignore
the NULL values. Also, if the Sales_Amount column is defined as NOT NULL, the records may
fail to load.
So, missing values should be handled before loading.

**Question 4: Data Type Validation
Identify records where Sales_Amount violates expected data type rules.
a) Which record(s) will fail numeric validation?
Answer:**

The following records contain text instead of numeric values:
Order_ID Sales_Amount
O104 Three Thousand
O114 Five Thousand
O129 Seven Thousand
These values need to be converted into numeric values before loading.
They can be converted as:
Three Thousand → 3000
Five Thousand → 5000
Seven Thousand → 7000
b) What would happen if this dataset is loaded into a SQL table with
Sales_Amount as DECIMAL?
Answer:
The text values cannot normally be stored directly in a DECIMAL column.
The database may:
• reject those records,
• generate a conversion error, or
• stop/fail the loading process depending on the ETL tool and database settings.
Therefore, the values should be converted to numbers before loading.

**Question 5: Date Format Consistency
The Order_Date column has multiple formats.
a) List all date formats present in the dataset.
Answer:**

The dataset contains these date formats:
1. DD-MM-YYYY
Example:
12-01-2024
2. YYYY/MM/DD
Example:
2024/01/18
3. DD-MM-YY
Example:
31-01-24
The supplied dataset contains all three styles.
b) Why is this a problem during data loading?
Answer:
Different date formats can create problems because the database or ETL tool may expect one
specific format.
For example, one value may be interpreted as DD-MM-YYYY, while another may be interpreted
differently depending on the system.
This can cause:
• loading errors,
• incorrect dates,
• wrong sorting,
• incorrect monthly or daily sales reports.
Therefore, all dates should be converted into one consistent format before loading.

**Question 6: Load Readiness Decision
a) Should this dataset be loaded directly into the database?
Answer:**

No, the dataset should not be loaded directly into the database.
b) Justify your answer with at least three reasons.
Answer:
There are several reasons:
1. Duplicate Order_ID values violate the primary key rule.
2. Missing Sales_Amount values can affect calculations and may violate database constraints.
3. Text values in Sales_Amount will fail numeric validation.
4. Different date formats can cause incorrect date loading.
5. Loading the data without cleaning can lead to incorrect reports and BI results.
Therefore, the data should first go through a cleaning and validation process.

**Question 7: Pre-Load Validation Checklist
List the exact pre-load validation checks you would perform on this dataset before loading.
Answer:**

Before loading this dataset, I would perform these checks:
1. Primary Key Check - Confirm that every Order_ID is unique.
2. Missing Value Check - Find NULL values in important columns such as Sales_Amount.
3. Data Type Check - Make sure every Sales_Amount is numeric.
4. Date Format Check - Make sure all Order_Date values follow one standard format.
5. Duplicate Record Check - Find repeated or duplicate transactions.
6. Null/Constraint Check - Confirm that required fields are not empty.
7. Range Check - Verify that Sales_Amount values are valid and not unexpected negative values.
8. Record Count Check - Compare the number of source records with the number of records
prepared for loading.
9. Final Validation - Run all checks again after cleaning before sending the data to the database.

**Question 8: Cleaning Strategy
Describe the step-by-step cleaning actions required to make this dataset load-ready.
Answer:**

I would clean the dataset in the following order:
Step 1: Identify duplicates
Find repeated Order_ID values such as O101, O120, and O138.
For exact duplicate transactions such as repeated O101 and O120, the duplicate copy can be
removed.
For O138, the two records have different amounts and dates, so they should not be deleted
blindly. The source system should be checked to determine which transaction is correct.
Step 2: Handle missing Sales_Amount
Identify the records with NULL Sales_Amount:
O102
O108
O117
O126
O134
The missing amounts should be obtained from the source if possible. If they cannot be recovered,
the records should be handled according to the business rule rather than assigning random
values.
Step 3: Convert text amounts to numbers
Convert:
Three Thousand → 3000
Five Thousand → 5000
Seven Thousand → 7000
Step 4: Standardize dates
Convert all dates to one common format, for example:
YYYY-MM-DD
Step 5: Run validation again
After cleaning, check the primary key, missing values, data types, and date format again.
Step 6: Load the cleaned dataset
Only after all important checks pass should the data be loaded into the database.

**Question 9: Loading Strategy Selection
Assume this dataset represents daily sales data.
a) Should a Full Load or Incremental Load be used?
Answer:**

For daily sales data, an Incremental Load would normally be used.
b) Justify your choice.
Answer:
Incremental loading means loading only the new or changed records instead of loading the entire
dataset every day.
For daily sales data, this is useful because the number of new transactions generated each day is
usually much smaller than the full historical dataset.
It can:
• reduce processing time,
• reduce database load,
• avoid unnecessary reprocessing,
• make the ETL process more efficient.
A common approach is to do an initial Full Load for historical data and then use Incremental
Loads for subsequent daily data.

**Question 10: BI Impact Scenario
Assume this dataset was loaded without cleaning and connected to a BI dashboard.
a) What incorrect results might appear in Total Sales KPI?
Answer:**

The Total Sales KPI could be incorrect in two directions.
It could be overstated because duplicate records may be counted more than once.
For example, the exact duplicate transactions for O101 and O120 would add an extra:
4500 + 7600 = 12,100
if both duplicate copies are included.
It could also be understated if NULL or invalid text Sales_Amount values are ignored by the BI
tool.
The three text amounts represent:
3000 + 5000 + 7000 = 15,000
If these are ignored instead of converted, the sales total would miss those amounts.
The O138 duplicate is also problematic because its two records contain different Sales_Amount
values, so the correct transaction cannot be determined from the dataset alone.
b) Which records specifically would cause misleading insights?
Answer:
The main problematic records are:
Duplicate records:
• O101
• O120
• O138
Missing Sales_Amount:
• O102
• O108
• O117
• O126
• O134
Invalid text Sales_Amount:
• O104
• O114
• O129
Inconsistent date formats:
Several records use different date formats, which could affect date-based dashboard analysis.
c) Why would BI tools not detect these issues automatically?
Answer:
BI tools mainly work with the data they receive. They do not automatically know the business
rules of the dataset.
For example, a BI tool may not know that:
• Order_ID should be unique,
• "Three Thousand" should actually mean 3000,
• NULL sales values need to be investigated,
• two different transactions with the same Order_ID are a duplicate problem.
Therefore, data quality and validation should be performed during the ETL process before the
data reaches the BI dashboard.
