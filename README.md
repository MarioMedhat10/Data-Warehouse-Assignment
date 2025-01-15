# DWH SSIS Packages for Data Integration

This repository contains three SSIS (SQL Server Integration Services) packages designed to handle different data integration tasks. Below is a detailed explanation of each package and its functionality.

## Problem 1: Load University Data from API

**File:** `Problem 1.dtsx`

### Description
This SSIS package is designed to load university data from an external API into a SQL Server database. The package performs the following steps:

1. **Truncate the Table:**
   - An Execute SQL Task truncates the `University` table to remove any existing data.
   - SQL Statement: `TRUNCATE TABLE University`

2. **Load Data from API:**
   - A Script Task fetches data from the API `http://universities.hipolabs.com/search?`.
   - The data is deserialized into a list of `University` objects.
   - Each university record is inserted into the `University` table.

## Problem 2: Update Employee Data with Historical Tracking (SCD type 6)

**File:** `Problem 2.dtsx`

### Description
This SSIS package updates employee data with historical tracking. It processes employee data and updates the target table with new and changed records while maintaining historical data.

### Steps
1. **Get Last Extract Date:**
   - An Execute SQL Task retrieves the last extract date from the `Employee_Type6` table.
   - SQL Statement: `SELECT MAX(Insert_Date) FROM Employee_Type6 WHERE is_valid = 1`

2. **Data Flow Task:**
   - **Source:** Fetches employee data from the `Employee` table based on the last extract date.
   - **Lookup:** Checks existing records in the target table.
   - **Conditional Split:** Routes data based on changes in `City` and `Email`.
   - **Derived Columns:** Creates metadata columns for historical tracking.
   - **Data Conversion:** Converts data types as needed.
   - **OLE DB Command:** Updates old versions of records.
   - **OLE DB Destination:** Loads new and updated records into the `Employee_Type6` table.

## Problem 3: Update Employee Data with Versioning

**File:** `Problem 3.dtsx`

### Description
This SSIS package updates employee data with versioning. It processes employee data and updates the target table with new versions of records while maintaining historical data.

### Steps
1. **Get Last Extract Date:**
   - An Execute SQL Task retrieves the last extract date from the `Employee_Q3_Target` table.
   - SQL Statement: `SELECT MAX(Insert_Date) FROM Employee_Q3_Target WHERE Active_Flag = 1`

2. **Data Flow Task:**
   - **Source:** Fetches employee data from the `Employee_Q3` table based on the last extract date.
   - **Lookup:** Checks existing records in the target table.
   - **Conditional Split:** Routes data based on changes in `Schedule_Date`.
   - **Derived Columns:** Creates metadata columns for versioning.
   - **OLE DB Command:** Updates old versions of records.
   - **OLE DB Destination:** Loads new and updated records into the `Employee_Q3_Target` table.

## How to Use

1. Open the SSIS project in Visual Studio 2022.
2. Configure the connection managers to point to your SQL Server instance.
3. Execute the packages in the order of `Problem 1.dtsx`, `Problem 2.dtsx`, and `Problem 3.dtsx`.

## Requirements

- SQL Server Integration Services (SSIS)
- Visual Studio 2022
- SQL Server Database

## License

This project is licensed under the MIT License.
