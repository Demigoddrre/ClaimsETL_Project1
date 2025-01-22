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
   - Generates:
     - `ProcessName`: Static value (`LoadClaims`).
     - `ErrorDateTime`: Captured using system functions.
     - Custom `ErrorDescription` (to be implemented).

3. **OLE DB Destination**:
   - Configured to insert valid rows into the `dbo.Stg_Claims` table.
   - Error rows redirected to the `dbo.ETL_Log` table.

4. **Error Logging**:
   - Tracks error details using columns such as:
     - `ErrorCode`
     - `SourceRow`
     - `ErrorDescription` (to be implemented).

---

## **Steps Completed**
1. **Flat File Source**:
   - Set up connection and mapped columns.
   - Configured error outputs for problematic rows.

2. **Error Handling**:
   - Redirected error outputs to `dbo.ETL_Log`.
   - Mapped existing error fields (`ErrorCode`, `SourceRow`) for tracking.

3. **OLE DB Destination**:
   - Configured to load valid rows into the `dbo.Stg_Claims` staging table.

---

## **Challenges Encountered**
1. **Warnings in Derived Column Transformation**:
   - Issue: Mismatched data types when redirecting error rows.
   - Resolution: Adjusted data type configurations and expressions.

2. **Project Execution Errors**:
   - Issue: Azure-Integrated SSIS project caused unexpected runtime issues.
   - Resolution: Recreated project as a standard SSIS project.

3. **Data Type Mismatches**:
   - Issue: Aligning input column types with destination columns.
   - Resolution: Used explicit data type conversions and validated mappings.

---

## **Next Steps**
1. **Enhance Derived Column Transformation**:
   - Implement custom error descriptions.
   - Refine metadata generation (e.g., `ErrorDateTime`).

2. **Run and Test the Package**:
   - Validate data flow:
     - Staging table: Confirm valid rows are inserted.
     - Error logging: Verify rows with issues are redirected.

3. **Verify Logging Tables**:
   - Check `dbo.ETL_Log` and `dbo.ETL_FileTracking` for completeness and accuracy.

4. **Document Workflow**:
   - Update `WorkflowDiagram.png` with final mappings and flow.

---

## **How to Run the Package**
1. Open the SSIS project in Visual Studio.
2. Select `LoadClaims.dtsx` in the **Solution Explorer**.
3. Click the **Start** button (green arrow) to execute the package.
4. Monitor the **Output** window for success/failure messages.
5. Review results:
   - **Staging Table**: Query `dbo.Stg_Claims` to validate data loading.
   - **Error Log Table**: Check `dbo.ETL_Log` for error details.

---

## **Future Enhancements**
- Automate package execution using SQL Server Agent.
- Add validation rules for null values and data format discrepancies.
- Develop incremental load processes for updating claims data.
- Expand ETL workflows to include fact and dimension tables for analytics.

---
