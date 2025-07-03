# SAP-ABAP
SUMMER INTERNSHIP
SAP ABAP at Exide Industries LTD

By- Aritra Banerjee			
Under the Supervision of-
Mr. Indrajit Saha

1.	 Problem Statement
The objective is to develop a custom ABAP report in SAP named ZMM_PO_REPORT_TEST to provide a comprehensive overview of the Purchase Order (PO) lifecycle in the Materials Management (MM) module. The report must retrieve and display PO data along with corresponding Goods Receipt (GR) and Invoice Receipt (IR) information in a single, integrated ALV output.
Users from procurement and finance departments need a unified tool to monitor the progress and status of POs, which are currently tracked through multiple transactions like SE38, SE11, ME23N and MIGO. This fragmentation creates inefficiencies and limits visibility.
The report should support:
●	Filtering based on PO number, PO date, document type, and company code.

●	A date range restriction to ensure performance and focus, limited to a maximum of 30 days.

●	Integration of data from SAP tables:

○	EKKO, EKPO – for PO details

○	MSEG – for GR details

○	RSEG – for IR details

○	LFA1 – for vendor information

●	An interactive ALV display that allows:

○	Subtotals and totals for quantities and invoice values

○	Hotspot functionality to click on a PO number and directly navigate to ME23N for detailed PO view

This report aims to streamline procurement operations by consolidating PO, GR, and IR data into a single view, enhancing both operational efficiency and data-driven decision-making.
REPORT NAME :- ZMM_PO_REPORT_TEST

Objective
To develop a custom ABAP ALV report ZMM_PO_REPORT_TEST that:
1.	Retrieves and displays PO header and item data.

2.	Tracks and shows corresponding Goods Receipts (GR).

3.	Includes Invoice Receipt (IR) details.

4.	Allows filtering by PO number, PO date, PO type, and company code.

5.	Restricts selection date range to 30 days.

6.	Provides an interactive ALV output with:

○	Summation of quantities and invoice amounts.

○	Grouping and subtotals by PO.

○	Hyperlinks for direct PO navigation.

7.	Enables accounting and purchasing users to monitor the full procurement lifecycle in one screen.


 Table of TCodes

TCode	Description	Usage in Report
ME23N	Display Purchase Order	Triggered via ALV hotspot
MIGO	Goods Receipt for PO	Source of GR data
SE38	Execute ABAP Report	Run report with selection screen
SE11	ABAP Dictionary	Used to view/edit table structures like EKKO, EKPO, and to understand field definitions.











OUTPUTS - 

 
FIG 1 - Purchase Order Details 

 
			FIG 2- Purchase Order with GR and IR 

 
		FIG 3 - Hyperlink on PO to open the ME23N


2. Problem Statement
The business requires a detailed and interactive SAP report to monitor Purchase Order (PO) item histories, including Goods Receipt (GR) and Invoice Receipt (IR) transactions for each PO item. Currently, analyzing PO histories involves manually checking multiple documents via various transactions (ME23N, MIGO, MIRO) and lacks a consolidated view at the PO item level.
To address this gap, the custom report ZMM_PO_ITEM_HISTORY is to be developed. It should:
●	Display basic PO header and item details.

●	On user interaction (clicking the PO number), fetch and display associated GR and IR history for a specific PO item.

●	Retrieve history using BAPI_PO_GETDETAIL to ensure accuracy and SAP standard logic.

●	Show the historical data in a popup ALV layout dynamically.

●	Validate input criteria (PO number, date range, etc.) to prevent overload and irrelevant data.

●	Enable the user to view the full lifecycle of a PO item from creation to receipt and invoice in a single interactive report.

REPORT NAME :- ZMM_PO_ITEM_HISTORY 







Transaction Codes (Tcodes) Used in the Report

TCode	Description	Purpose in the Report Context
SE38	ABAP Editor – Execute ABAP Reports	Used to create, edit, and run the custom report ZMM_PO_ITEM_HISTORY.
SE11	ABAP Dictionary	Used to view/edit table structures like EKKO, EKPO, and to understand field definitions.
ME21N	Create Purchase Order	Creates PO master and item data that the report will later retrieve and display.
MIGO	Goods Movement (GR)	Posts Goods Receipts for POs. GR data shown in the popup is based on MIGO transaction entries.

Function Modules Used
 1. BAPI_PO_GETDETAIL
●	Purpose: Fetches complete details of a Purchase Order including item history.

●	Used For:

○	Retrieving GR and IR history records via the PO_ITEM_HISTORY internal table.

○	Ensures data consistency as this BAPI uses SAP’s internal logic.
○	

●	Parameters Used:

○	PURCHASEORDER = wa_final-ebeln

○	HISTORY = 'X'

○	Returns data in PO_ITEM_HISTORY structure (BAPIEKBE type)

●	Advantage:
 SAP-standard BAPI which includes validated and up-to-date data. Avoids directly joining complex history tables (MKPF, MSEG, RBKP, RSEG).

2. REUSE_ALV_GRID_DISPLAY
●	Purpose: Display the main PO report in an interactive ALV grid.

●	Used For:

○	Showing the list of PO items.

○	Supporting sorting, totaling (EKPO_MENGE), and hotspot click on PO number.

3. REUSE_ALV_POPUP_TO_SELECT
●	Purpose: Display the GR/IR item history for a PO item in a popup screen.

●	Used For:

○	Compact display of BAPIEKBE history data (GR and IR) when user clicks on PO number.

○	Selection screen is non-editable (i_allow_no_selection = 'X')
Core Functional Flow
1.	Selection Screen:

○	Input: PO number range, PO date, PO type, company code.

○	Validations: Date range ≤ 30 days, PO range ≤ 600 numbers.

2.	Main ALV Output:

○	Display PO header and item details from EKKO, EKPO.

3.	On Hotspot Click (PO Number):

○	BAPI BAPI_PO_GETDETAIL is called with a history flag.

○	PO item-wise GR and IR history retrieved and filtered.

○	Result shown in a popup ALV via REUSE_ALV_POPUP_TO_SELECT.



OUTPUTS : 
 
FIG 1- PO Item History
 
FIG 2 -  Hyperlink on PO to show GR history 


 
FIG 3 - Hyperlink on PO to show IR history









3.  Problem Statement
In many business scenarios, there is a need to upload bulk data from Excel files into SAP for validation, reporting, or further processing. Manual entry of such data can be time-consuming, error-prone, and inefficient.
To address this, the report ZMM_UPLOAD_REPORT is developed to provide a simple Excel file upload interface in SAP, allowing users to:
●	Upload an Excel file from the local system.

●	Convert its contents into an internal table using the standard SAP function module.

●	Display the uploaded data in a clean ALV Grid output for review or further processing.

REPORT NAME - ZMM_UPLOAD_REPORT

 Features of the Report
●	File selection popup (F4_FILENAME) to pick an Excel file from desktop.

●	Upload of .xls format Excel files (not .xlsx).

●	Data conversion using SAP function module TEXT_CONVERT_XLS_TO_SAP.

●	ALV output display of uploaded data in columns like:

○	Message ID

○	Name

○	Message Type

○	Message Number



Transaction Codes (Tcodes) Used
TCode	Description	Usage in the Report
SE38	ABAP Editor / Report Execution	Used to create, modify, and execute the report ZMM_UPLOAD_REPORT.
SE11	Data Dictionary	Used to define and view structures, data types (like RLGRAP-FILENAME) used in the report.



OUTPUT - 

 
FIG 1- Excel file upload
 
FIG 2 - Execution of the Excel file




4.  Problem Statement

In SAP MM, Purchase Orders (POs) are crucial for procurement, but creating them manually in ME21N is time-consuming, especially when dealing with bulk POs involving multiple line items and account assignments.
This program solves the problem by:
●	Allowing bulk creation of POs by uploading data from an Excel spreadsheet.

●	Processing both header and item-level data, including account assignments.

●	Using BAPI_PO_CREATE1 to create the POs programmatically.

●	Logging and displaying system return messages (success or failure) in the report output.
REPORT NAME - ZMM_UPLOAD_REPORT_2
Transaction Codes (Tcodes) Used
TCode	Description	Purpose in Context
SE11	Data Dictionary	To inspect tables like EKKO, EKPO, EKNK, SANKR, etc.
ME21N	Create Purchase Order	Standard SAP Tcode for manual PO creation (replaced here by automation).
SE37	Function Module Editor	To view/test function modules like BAPI_PO_CREATE1, etc.

 Function Modules Used and Their Purpose


Function Module	Purpose
F4_FILENAME	Provides file selection popup for choosing Excel file path
TEXT_CONVERT_XLS_TO_SAP	Converts Excel file data into internal table
CONVERSION_EXIT_ALPHA_INPUT	Ensures numeric values like vendor numbers, GL accounts have leading zeros
BAPI_PO_CREATE1	Main BAPI used to create Purchase Orders in SAP
BAPI_TRANSACTION_COMMIT	Commits the transaction after successful PO creation
5 . Problem Statement 
In SAP Materials Management (MM), once a Purchase Order (PO) is created and goods are physically received, a Goods Receipt (GR) must be posted to update stock and initiate payment processes. While GRs can be created using standard TCodes like MIGO, doing so manually for large numbers of PO items or via custom automation can be inefficient.
To simplify this, the report ZMM_GR_CREATION allows the user to:
●	Enter a PO number, item, and posting dates.

●	Automatically fetch PO item details from SAP.

●	Call BAPI_GOODSMVT_CREATE to post a GR with movement type 101.

●	Display success or failure messages in an ALV report using the OO ALV (Object-Oriented ALV Grid).

REPORT FILE - ZMM_GR_CREATION
Transaction Codes (Tcodes) Used
TCode	Description	Usage in This Report
SE38	ABAP Editor	Used to create, edit, and run the custom report ZMM_GR_CREATION.
SE11	Data Dictionary	Used to explore the table structures like EKPO and data elements used in parameters.
SE37	Function Builder	To view and test function modules like BAPI_GOODSMVT_CREATE and BAPI_TRANSACTION_COMMIT.
MIGO	Goods Movement	Standard SAP TCode to post GR. This program replicates its core functionality programmatically.
Function Modules Used
Function Module	Purpose in Code
BAPI_GOODSMVT_CREATE	Posts the Goods Receipt document based on PO item data.
BAPI_TRANSACTION_COMMIT	Commits the transaction to finalize the GR document in SAP.
BAPI_TRANSACTION_
ROLLBACK	Rolls back the transaction in case of any error during GR creation.





6.  Problem Statement
The goal is to design and implement a Module Pool SAP ABAP program (ZLIB_MP_2) for managing a Library System within an organization. The application should enable users to create, edit, and view records related to:
1.	Library Master Data

2.	Book Master Data

3.	Book Issue Transactions

The system must handle user interactions through custom SAP screens using dialog programming. It should support:
●	Data validation and processing via user commands (sy-ucomm)

●	Screen navigation and status handling using CALL SCREEN

●	Integration with custom transparent tables:

○	YLIBY_MASTER – for library details

○	YLIB_BOOK_MASTER – for book metadata

○	YSTD_MASTER – for book issue and return history

The system will be accessed using custom transaction codes and must be user-friendly, ensuring accurate data maintenance for the library and supporting all common operations such as insert, update, and display from the same program.

Step-by-Step Implementation:
🔹 Step 1: Create Custom Tables (Data Dictionary)
Use T-Code: SE11
Create the following transparent tables:
1.	YLIBY_MASTER – Library master data

○	Fields: LIBY_ID,  LIBY_NAME .

2.	YLIB_BOOK_MASTER – Book master data

○	Fields: BOOK_ID , BOOK_NAME , BOOK_ATHR , BOOK_PBLSH. 

3.	YSTD_MASTER – Student Details Master

○	Fields: STD_ID ,STD_NAME , STD_DPRT, STD_SEM. 

Save and activate all tables.
Step 2: Create the Module Pool Program
Use T-Code: SE38
 Program Name: ZLIB_MP_2
●	Set program type as "Executable Program".

●	Use INCLUDE statements for screen logic:

○	ZLIB_MP_2_STATUS_0100O01, ZLIB_MP_2_USER_COMMAND_0100I01 – for screen 100 logic

○	ZLIB_MP_2_STATUS_0200O01, ZLIB_MP_2_USER_COMMAND_0200I01 – for screen 200 logic


🔹 Step 3: Create Screens

 Create two screens:
🔸 Screen 100 – Library & Book Master
●	Elements: Input fields for YLIBY_MASTER

●	Buttons: SAVE, DISPLAY, BACK

●	Logic handled via MODULE status_0100 OUTPUT and MODULE user_command_0100 INPUT

🔸 Screen 200 – Book Issue Screen
●	Elements: Input fields from YSTD_MASTER

●	Buttons: ISSUE BOOK, BACK

●	Logic handled via MODULE status_0200 OUTPUT and MODULE user_command_0200 INPUT

🔹 Step 4: Define Transaction Codes
 Create the following custom TCodes:
TCode	Function	Screen
YLIB_1	Insert Library/Book	100
YLIB_2	Edit/Update Library/Book	100
YLIB_3	Display Library/Book	100
🔹 Step 5: PF-Status and Titlebar
●	Create status PF-STATUS_100 and PF-STATUS_200

○	Buttons: SAVE, BACK, DISPLAY, EXIT, etc.


🔹 Step 6: Write Logic in Includes
Inside includes like ZLIB_MP_2_USER_COMMAND_0100I01, implement:
●	Data validation

●	Insert/update logic using INSERT, MODIFY, or SELECT

●	Navigation handling with CALL SCREEN or LEAVE PROGRAM

🔹 Step 7: Testing
Use your defined TCodes (YLIB_1, YLIB_2, YLIB_3) to:
●	Insert library & book details

●	Issue a book to a student

●	Display entries for verification





T-Codes Used and Their Purposes
TCode	Used For	Description
SE11	Data Dictionary	Create and activate transparent tables: YLIBY_MASTER and YLIB_BOOK_MASTER.
SE38	ABAP Editor	Create the module pool program ZLIB_MP_2 and include screens and logic.
YLIB_1	Insert Mode (Screen 100)	Allows users to input and save new library and book records.
YLIB_2	Edit Mode (Screen 100)	Allows users to fetch and update existing records based on primary keys.
YLIB_3	Display Mode (Screen 100)	Shows all details of the records in read-only mode for review purposes.
OUTPUTS : 
 
FIG 1- YLIB_1 - Library Data insert

 
FIG 2 - YLIBY_MASTER data entries
(Library data insert successful)

 
FIG 3- YLIB_2 - Library Data updated


 
FIG 4 - YLIBY_MASTER data entries
(Library data update successful)




 
FIG 3- YLIB_3 - Library Data display
