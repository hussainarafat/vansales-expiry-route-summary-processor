# Sales Summary Processor Executable README

## Overview

This executable (.exe) processes retail sales data from Excel binary files (.xlsb) to generate detailed summaries of sales and expiry transactions. It categorizes transactions into "Retail Sales" or "Retail Expiry" based on return quantities and location, computes invoice values by adjusting for discounts and returns, and produces three key summary tables:

- **Final Summary**: Aggregates customer counts, invoice counts, and values by transaction type (Sales Invoice or Expiry Invoice) and customer type (Cash or Credit), including totals and averages like sales per invoice.
- **Location Summary**: Breaks down customers, routes, sales value, expiry value, and expiry percentage by location, with a total row.
- **Sales Bin Summary**: Categorizes routes by net sales value into predefined bins (e.g., 0-25k, 25k-50k) and counts the number of routes in each bin, with a total.

The .exe handles multiple input files, creating one output Excel sheet per input file in a single .xlsx report. It is a bundled version of a Python script using libraries like Pandas, NumPy, and XlsxWriter. Logging provides progress and error details via console.

## Why the .exe is ~200MB

This .exe was created using PyInstaller, which bundles the original ~17KB Python script with the full Python interpreter and all required dependencies (e.g., Pandas for data processing, NumPy for computations, pyxlsb for .xlsb reading, and XlsxWriter for output formatting). These libraries add significant size because they include pre-compiled code, modules, and resources for standalone execution on Windows without needing Python installed. This ensures compatibility but results in a larger file. If size is a concern, consider running the original Python script directly (requires Python and libraries installed) or exploring the cloud alternative below.

## Recommended Alternative: Cloud-Based Processing

For larger datasets, multiple users, or to avoid local storage/processing overhead, uploading data to the cloud and computing there is often better. Benefits include:
- **Scalability**: Handle massive files without straining your local machine's RAM/CPU.
- **Cost-Efficiency**: Pay-per-use on platforms like AWS, Google Cloud, or Azure (e.g., via Lambda functions or Colab notebooks).
- **Collaboration**: Share processed reports easily without distributing large .exe files.
- **Maintenance**: Automatic updates to libraries; no need to rebuild .exe for fixes.
- **Setup Suggestion**: Upload .xlsb files to cloud storage (e.g., S3), run the Python script in a serverless environment (e.g., AWS Lambda with layers for dependencies), or use Jupyter notebooks on Google Colab for free interactive processing.

If interested, the original Python script can be adapted for cloud deployment—contact for details.

## Required File Type

- **Input File Type**: Excel Binary Workbook (.xlsb). Uses pyxlsb for reading; compact for large data.
- **Output File Type**: Standard Excel Workbook (.xlsx), generated automatically with formatted tables.
- **Note**: Does not support .xlsx, .csv, or .xls directly. Convert to .xlsb via Excel if needed.

## Required Sheets and Columns

The .exe reads from the default (first) sheet in the .xlsb file. Data must be tabular: headers in row 1, data from row 2.

Expected columns (case-sensitive).

- **Location**: String; e.g., 'Outlet'. For categorization/summaries. Required.
- **Route Code**: String/Int; Route ID. For counts/binning. Required.
- **Customer Type**: String; 'Cash Customer' or 'Credit Customer'. Required.
- **Customer Code**: String/Int; Unique customer ID. For unique counts. Required.
- **Invoice Number**: String/Int; Unique invoice ID. For invoice counts. Required.
- **Sales Qty Value**: Float/Int; Sales quantity. For value computation. Required.
- **Good Return Qty Value**: Float/Int; Good returns. Adjusted in sales. Required.
- **Discount Value**: Float/Int; Discount amount. Subtracted from values. Required.
- **Bad Return Qty Value**: Float/Int; Bad returns. Triggers 'Retail Expiry'. Optional (0 if missing).
- **Defective Return Qty Value**: Float/Int; Defective returns. Triggers 'Retail Expiry'. Optional (0 if missing).
- **Transaction Type**: String; Script-assigned ('Retail Sales'/'Retail Expiry'). Computed; don't include.
- **Invoice Value**: Float; Script-computed (sales/returns - discounts). Computed; don't include.

Notes: Columns by name, not letter. Numerics clean/filled with 0. Strings consistent. Minimum: First 7 for basics.

## File Placement

- **Input Files**: Place all .xlsb files in the same directory as the .exe. It scans the current directory for `*.xlsb` files.
- **Output File**: Saved in the same directory as `Sales Summary Generated on - [Month Year].xlsx` (e.g., `Sales Summary Generated on - August 2025.xlsx`).
- **Execution Environment**: Windows OS. Run in a folder with read/write permissions. No Python installation needed.

## Step-by-Step Usage

1. **Prepare Input Files**:
   - Ensure data in .xlsb format with required columns.
   - Place files in the .exe's directory (e.g., `sales_data.xlsb`).
   - Supports multiple files; each becomes a sheet in output.

2. **Run the Executable**:
   - Double-click the .exe (or run via command prompt: `sales_summary_processor.exe`).
   - Console window opens, logging progress (e.g., "Reading file: sales_data.xlsb...").

3. **Review Output**:
   - .xlsx generated in same directory.
   - Open in Excel: Sheets named after input files.
   - Contains Final Summary (top-left), Location Summary (below-left), Sales Bin Summary (below-right).
   - Formatted with red headers, numbers, percentages.

4. **Verify Logs**:
   - Console shows timings, warnings, errors. Close window when done.

## Common Errors and Troubleshooting Tips

- **No .xlsb Files Found**: Ensure files in same directory; check extensions.
- **Error Reading [file].xlsb**: File corrupted? Re-save as .xlsb in Excel.
- **Permission Denied on Output**: Close existing output file; ensure write access.
- **Empty/Zeros in Summaries**: Check data rows; verify returns/locations for classification.
- **Large File Crashes**: For big datasets, RAM may be insufficient—switch to cloud.
- **Windows-Specific**: If antivirus flags .exe, add exception (common for PyInstaller bundles).
- **Slow Performance**: Large files tax local resources; consider cloud for speed.
- **Tip**: Backup inputs. For issues, capture console output.

