# Technical Architecture

## Report layer
- Power BI report definition is stored in the PBIP report project.
- The report definition contains 9 pages: Overview, Production, OEE, Downtime, Delivery, Energy, Cost, Digital ROI, and Management Intelligence.
- `Management Intelligence` is marked `HiddenInViewMode` in the report page definition.

## Semantic model
- Import-mode tables are defined in TMDL.
- Eight Power Query queries import Excel worksheets.
- `Date_Table` and `Management Alerts` are calculated tables.
- DAX measures are primarily centralized in `_measure`, with two additional measures in `Management Alerts`.

## Data source
The eight Power Query imports reference a local Excel workbook named `Smart_Manufacturing_6_Month_Complete_Data.xlsx`. The TMDL contains a machine-specific absolute Windows path. That path is deliberately not reproduced in this public documentation. For portability, replace it with a parameter or a path valid in the target environment.

## Refresh and deployment considerations
1. Keep the source workbook available at the configured path or update the source parameter.
2. Validate refresh in Power BI Desktop after moving the project.
3. Configure credentials and gateway requirements in Power BI Service if the source remains on a local machine.
4. Confirm date relationships and business definitions before using the model for operational reporting.
5. The published report uses Publish to web, so report content is publicly accessible. Do not connect confidential data or credentials to this public report.

## Audit scope
This document describes what is present in the uploaded PBIP ZIP. It does not claim that refresh, gateway setup, DAX business logic, or production readiness has been tested.
