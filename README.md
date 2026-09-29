# Import Data using Transform Maps (Spreadsheet) 

## 1. Brainstorming & Ideation Phase

**Problem statement:** Organizations receive bulk employee data in Excel format from HR or external systems. Entering it manually into ServiceNow is slow, error-prone, and creates duplicate records when the same data is received again.

**Idea:** Use ServiceNow Import Sets (staging table) and Transform Maps (field mapping) to load spreadsheet data automatically into a custom Employee table, and use Coalesce to prevent duplicates.

**Ideas considered**

| Idea | Decision |
|---|---|
| Manual data entry record by record | Rejected: slow and error-prone |
| Import Set + Transform Map | **Selected**: native, scalable, no code |
| Custom script or integration | Rejected: too complex for a beginner project |

**Expected outcome:** Clean, duplicate-free employee data in ServiceNow, plus reports and a dashboard for visibility.

---

## 2. Requirement Analysis Phase

**Functional requirements**
- Accept employee data from an Excel (.xlsx) file.
- Store it first in an Import Set (staging) table.
- Map source fields to a custom target table.
- Avoid duplicates on re-import (Coalesce on Employee ID).
- Provide reports (Department, Location, List) and a dashboard.

**Non-functional requirements**
- Accuracy and data integrity.
- Repeatable imports.
- Simple for admins and HR to use.

**Data requirements:** Employee ID, Employee Name, Email, Department, Location.

**Tools and platform:** ServiceNow Personal Developer Instance (PDI), Google Sheets / Excel, admin role.

---

## 3. Project Design Phase

**Data flow**

Excel file → Load Data → Import Set table (`u_employee_import`) → Transform Map (`Sample Spreadsheet Import`) → Target table (`u_employee_test`) → Reports → Dashboard

**Target table: Employee Test (`u_employee_test`)**

| Field | Type |
|---|---|
| Employee ID | String (Coalesce key) |
| Employee Name | String |
| Email | String |
| Department | String |
| Location | String |

**Design components**
- Import Set table: Employee Import (`u_employee_import`).
- Transform Map: Sample Spreadsheet Import, with a 1:1 field mapping.
- Coalesce: Employee ID, so existing records are updated instead of duplicated.
- Reports: Employees by Department (Pie), Employees by Location (Bar), Employee List Report (List).
- Dashboard: Employee Analytics Dashboards.

---

## 4. Project Planning Phase

| Step | Activity | Output |
|---|---|---|
| 1 | Prepare spreadsheet | Sample Spreadsheet.xlsx |
| 2 | Create custom table and fields | Employee Test table |
| 3 | Load data into Import Set | Employee Import table |
| 4 | Create and run Transform Map | Records in Employee Test |
| 5 | Enable Coalesce | Duplicate prevention |
| 6 | Re-import with changed and new data | Insert and update validation |
| 7 | Create reports | 3 reports |
| 8 | Create dashboard and add reports | Employee Analytics Dashboards |
| 9 | Testing, documentation, demo | Final deliverables |

**Risks and mitigation**
- Wrong field mapping: verify with Auto Map Matching Fields and Mapping Assist.
- Duplicate records: enable Coalesce.
- Dashboard creation access: enable Admin Overrides on the `pa_dashboards` ACL.

---

## 5. Project Development Phase

**Phase 1 – Prepare the spreadsheet:** Create a Google Sheet with sample employee data and download it as `Sample Spreadsheet.xlsx`.

**Phase 2 – Create the custom table:** Go to Tables > Create New. Label: Employee Test, Name: `u_employee_test`. Open Form Layout and add the String fields Employee ID, Employee Name, Email, Department, and Location. Save.

**Phase 3 – Import Set table:** Go to Load Data. Label: Employee Import, Name: `u_employee_import`. Choose the Excel file, submit, and click Create Transform Map.

**Phase 4 – Transform Map:** Name: Sample Spreadsheet Import. Source: Employee Import, Target: Employee Test. Click Auto Map Matching Fields, then use Mapping Assist to verify the mapping. Save, then Transform.

**Phase 5 – Validate:** Open the Employee Test table and confirm that all records were created. Use Personalize List Columns to arrange the fields.

**Phase 6 – Coalesce:** Open the transform map, go to the Field Maps related list, and set Coalesce = true on the Employee ID field map. Save.

**Phase 7 – Re-import new data:** Load Data > existing table Employee Import > file source, Sheet 1, Header row 1 > Submit > Run Transform. The file contained 4 rows:
- 2 existing IDs with a changed name or email (updated).
- 2 new IDs (inserted).

**Phase 8 – Reports:** Create the following reports on the Employee Test table.
1. Employees by Department: Pie chart, Group by Department, Aggregation Count.
2. Employees by Location: Bar chart, Group by Location, Aggregation Count.
3. Employee List Report: List with all five columns.

**Phase 9 – Dashboard:** Enable Admin Overrides for Create on the `pa_dashboards` ACL, then open `PA_DASHBOARDS.FORM` and create Employee Analytics Dashboards. Open each report, then Share > Add to Dashboards, and select the dashboard.

---

## 6. Project Testing Phase

| Test | Input | Expected result | Actual result |
|---|---|---|---|
| Initial import | Sample spreadsheet | All records created in Employee Test | Passed |
| Field mapping | Auto Map + Mapping Assist | Source and target fields mapped | Passed |
| Re-import with changes | 4 rows (2 changed, 2 new) | 2 inserted, 2 updated | Passed |
| Duplicate test | Same sheet uploaded again | 0 inserted, 0 updated, 4 ignored | Passed |
| Reports | Employee Test data | Charts and list show correct data | Passed |
| Dashboard | 3 reports added | All widgets visible | Passed |

**Verification points:** Transform History shows insert, update, and ignored counts, and Employee Test shows the updated names and emails.

---

## 7. Project Documentation Phase

**Deliverables**
- Sample Spreadsheet.xlsx (source data).
- Custom table Employee Test (`u_employee_test`).
- Import Set table Employee Import (`u_employee_import`).
- Transform Map Sample Spreadsheet Import with Coalesce on Employee ID.
- 3 reports and the Employee Analytics Dashboards dashboard.
- Screenshots of each step (table, field mapping, transform result, transform history, reports, dashboard).

**Key learnings**
- An Import Set is a staging area, and the Transform Map moves the data to the target table.
- Coalesce turns repeated imports into updates instead of duplicates.
- Reports and dashboards make the data visible to HR and administrators.

---

## 8. Project Demonstration Phase

**Demo flow**
1. Show the source Excel file.
2. Show the Employee Test table structure.
3. Load the data and show the Import Set table.
4. Show the Transform Map and its field mappings, then run the transform.
5. Show the imported records in Employee Test.
6. Enable Coalesce, upload the modified file, and show Transform History (2 inserted, 2 updated).
7. Upload the same file again and show 4 ignored.
8. Show the three reports and the dashboard.

**Conclusion:** The project delivers an automated, accurate, duplicate-free employee data import using Import Sets, Transform Maps, and Coalesce. Reports and a dashboard give HR and administrators one place to monitor employee data trends and data quality.
