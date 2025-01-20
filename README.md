# **Claims ETL Project**

## **Overview**
The **Claims ETL Project** is part of the larger **NorthwellPracticeDWH** initiative. This project focuses on extracting, transforming, and loading claims data into a Data Warehouse for analysis, reporting, and business intelligence. The primary goal is to streamline the ETL workflow and ensure robust error handling, logging, and data validation.

---

## **Directory Structure**
```
ClaimsETL_Project/
├── SSIS_Packages/
│   ├── LoadClaims.dtsx         # SSIS package for loading claims data
│   ├── TransformClaims.dtsx    # SSIS package for transforming claims data
│   ├── Package.dtsx            # Master package to orchestrate multiple ETL tasks
├── Connection_Managers/
│   ├── ClaimsCSV_Conn          # Flat File Connection Manager for the claims CSV
│   ├── OLEDB_Conn              # OLE DB Connection Manager for SQL Server
├── Logs/
│   ├── ETL_FileTracking.sql    # SQL logging table for file ingestion
│   ├── ETL_Log.sql             # SQL logging table for error handling and process tracking
├── Documentation/
│   ├── README.md               # This file
│   ├── WorkflowDiagram.png     # Visual representation of the ETL process
├── SampleCSV/
│   ├── claim_data.csv          # Sample claims data for testing
```

---

## **Current SSIS Package: `LoadClaims.dtsx`**

### **Purpose**
The `LoadClaims.dtsx` package handles the ingestion of claims data from a flat file (CSV format) into the staging table (`dbo.Stg_Claims`). It includes robust error handling and logging to ensure data integrity.

### **Progress**
1. **Flat File Source**:
   - Configured to read from the sample CSV file located in the `SampleCSV/` directory.
   - Columns mapped to the corresponding staging table fields.
   - Error output configured to redirect problematic rows for logging.

2. **Derived Column Transformation**:
   - To be added for:
     - Generating `ProcessName` (static value: `LoadClaims`).
     - Capturing `ErrorDateTime` using system variables.
     - Generating custom error descriptions.

3. **OLE DB Destination**:
   - Configured to insert valid rows into the `dbo.Stg_Claims` table.
   - Error rows redirected to an error-handling destination.

4. **Error Logging**:
   - Errors redirected to the `dbo.ETL_Log` table for detailed logging.
   - Flat File Source errors tracked with columns such as:
     - `ErrorCode`
     - `SourceRow`
     - `ErrorDescription` (to be generated via Derived Column Transformation).

---

## **Steps Completed**
1. **Flat File Source**:
   - Set up connection and mappings.
   - Configured error outputs to redirect to an error-handling path.

2. **Error Handling**:
   - Configured error redirection to the `dbo.ETL_Log` table.
   - Mapped existing columns (`ErrorCode`, `SourceRow`) to the logging table.

3. **OLE DB Destination**:
   - Configured to insert valid rows into the staging table (`dbo.Stg_Claims`).

---

## **Next Steps**
1. **Add Derived Column Transformation**:
   - Create custom columns:
     - `ProcessName`: Static value (`LoadClaims`).
     - `ErrorDateTime`: Captured using `GETDATE()` or a system variable.
     - `ErrorDescription`: Derived based on error codes or flat file issues.

2. **Test the Package**:
   - Run the package to ensure data flows from the Flat File Source to:
     - Staging table for valid rows (`dbo.Stg_Claims`).
     - Error logging table (`dbo.ETL_Log`) for problematic rows.

3. **Verify Staging Table Data**:
   - Query `dbo.Stg_Claims` in SQL Server Management Studio (SSMS) to validate data insertion.

4. **Verify Logging**:
   - Check the `dbo.ETL_Log` table for logged errors and confirm proper error handling.

5. **Document Results**:
   - Update the `WorkflowDiagram.png` with the final configuration.
   - Log key outcomes of the test in the `Logs/` directory.

---

## **How to Run the Package**
1. Open the SSIS project in Visual Studio.
2. Select the `LoadClaims.dtsx` package in the **Solution Explorer**.
3. Execute the package:
   - Validate the data flow.
   - Monitor the staging table and logs for results.
4. Address any issues in error handling or data mapping.

---

## **Future Enhancements**
- Automate the package execution using SQL Server Agent.
- Implement incremental data loading for new or updated claims data.
- Add data validation rules to flag discrepancies during the transformation process.
- Integrate additional ETL tasks (`TransformClaims.dtsx`) to load data into Fact and Dimension tables.

