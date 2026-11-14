# Data Model & Relationships

This reference is extracted from the uploaded semantic-model TMDL definitions.

## Core model tables

| Table | Columns | Role inferred from fields |
|---|---:|---|
| `Cost` | 10 | Manufacturing cost fact |
| `Date_Table` | 9 | Calendar dimension (calculated table) |
| `Digital_Projects` | 8 | Digital investment / benefits |
| `Downtime` | 8 | Downtime event fact |
| `Energy` | 8 | Energy consumption and cost fact |
| `Machine_Master` | 5 | Machine / process / plant lookup |
| `'Management Alerts'` | 2 | Calculated alert category table |
| `Monthly_Summary` | 7 | Monthly aggregated production summary |
| `Orders` | 9 | Customer order and delivery fact |
| `Production` | 12 | Production performance fact |
| `_measure` | 1 | Central DAX measure table |

## Declared relationships

The project declares the following column-to-column relationships. The TMDL file does not explicitly state cardinality or cross-filter direction for these relationships, so those properties are intentionally not inferred here.

| From | To |
|---|---|
| `Monthly_Summary.Month` | `LocalDateTable_bd29f652-979a-445c-8d31-69ce1e72cf74.Date` |
| `Orders.'Due Date'` | `LocalDateTable_b4bd206d-2a85-4a02-a48c-e75ab76276b6.Date` |
| `Orders.'Delivery Date'` | `LocalDateTable_448814d3-1083-45f9-9983-75046d888181.Date` |
| `Production.Date` | `Date_Table.Date` |
| `Downtime.Date` | `Date_Table.Date` |
| `Orders.'Order Date'` | `Date_Table.Date` |
| `Energy.Date` | `Date_Table.Date` |
| `Cost.Date` | `Date_Table.Date` |
| `Production.'Machine ID'` | `Machine_Master.'Machine ID'` |
| `Downtime.'Machine ID'` | `Machine_Master.'Machine ID'` |
| `Energy.'Machine ID'` | `Machine_Master.'Machine ID'` |
| `Cost.'Machine ID'` | `Machine_Master.'Machine ID'` |

## Additional Power BI date tables

The model also contains three hidden `LocalDateTable_*` tables associated with date hierarchies for `Orders[Due Date]`, `Orders[Delivery Date]`, and `Monthly_Summary[Month]`, plus a hidden/private `DateTableTemplate_*` table. These are Power BI-generated date table structures, not additional business subject-area tables.

## Calendar table

`Date_Table` is a calculated calendar table covering **1 January 2026 through 30 June 2026**. It adds Year, Month No, Month, Year Month, Quarter, Day, Day Name, and Year Month Sort columns.

## Modeling observations

- `Production`, `Downtime`, `Energy`, and `Cost` are linked to `Date_Table` through their `Date` columns.
- `Orders[Order Date]` is linked to `Date_Table[Date]`. Separate hidden local date tables are associated with `Orders[Due Date]` and `Orders[Delivery Date]`.
- `Production`, `Downtime`, `Energy`, and `Cost` are linked to `Machine_Master` by `Machine ID`.
- `Monthly_Summary[Month]` is associated with an auto-generated local date table rather than the declared `Date_Table` relationship.
- The `_measure` table stores the main DAX measure library; `Management Alerts` contains two additional measures and a calculated alert-category table.
