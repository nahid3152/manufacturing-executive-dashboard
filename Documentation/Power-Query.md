# Power Query (M) Reference

The semantic model contains eight Excel-based Power Query imports. Each query reads a worksheet from `Smart_Manufacturing_6_Month_Complete_Data.xlsx`, promotes the first row to headers, and applies explicit column data types.

## Source configuration

The project currently stores a local Windows file path in each query. The path has been redacted below for privacy and portability. Before refreshing on another computer, replace `<PATH_TO_SAMPLE_WORKBOOK>` with the actual workbook path or parameterize the source.

```m
Source = Excel.Workbook(File.Contents("<PATH_TO_SAMPLE_WORKBOOK>"), null, true)
```

## Cost

Worksheet import and type-conversion steps:

```powerquery
let
				    Source = Excel.Workbook(File.Contents("<PATH_TO_SAMPLE_WORKBOOK>"), null, true),
				    Cost_Sheet = Source{[Item="Cost",Kind="Sheet"]}[Data],
				    #"Promoted Headers" = Table.PromoteHeaders(Cost_Sheet, [PromoteAllScalars=true]),
				    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"Date", type datetime}, {"Machine ID", type text}, {"Machine Name", type text}, {"Process", type text}, {"Plant", type text}, {"Material Cost", type number}, {"Labor Cost", type number}, {"Overhead Cost", type number}, {"Total Cost", type number}, {"Cost per Unit", type number}})
				in
				    #"Changed Type"
```

## Digital_Projects

Worksheet import and type-conversion steps:

```powerquery
let
				    Source = Excel.Workbook(File.Contents("<PATH_TO_SAMPLE_WORKBOOK>"), null, true),
				    Digital_Projects_Sheet = Source{[Item="Digital_Projects",Kind="Sheet"]}[Data],
				    #"Promoted Headers" = Table.PromoteHeaders(Digital_Projects_Sheet, [PromoteAllScalars=true]),
				    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"Project ID", type text}, {"Project Name", type text}, {"Category", type text}, {"Business Objective", type text}, {"Owner", type text}, {"Investment", Int64.Type}, {"Annual Benefit", Int64.Type}, {"ROI Improvement %", type number}})
				in
				    #"Changed Type"
```

## Downtime

Worksheet import and type-conversion steps:

```powerquery
let
				    Source = Excel.Workbook(File.Contents("<PATH_TO_SAMPLE_WORKBOOK>"), null, true),
				    Downtime_Sheet = Source{[Item="Downtime",Kind="Sheet"]}[Data],
				    #"Promoted Headers" = Table.PromoteHeaders(Downtime_Sheet, [PromoteAllScalars=true]),
				    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"Date", type datetime}, {"Machine ID", type text}, {"Machine Name", type text}, {"Process", type text}, {"Downtime Reason", type text}, {"Downtime Hours", type number}, {"Start Time", type time}, {"Impact", type text}})
				in
				    #"Changed Type"
```

## Energy

Worksheet import and type-conversion steps:

```powerquery
let
				    Source = Excel.Workbook(File.Contents("<PATH_TO_SAMPLE_WORKBOOK>"), null, true),
				    Energy_Sheet = Source{[Item="Energy",Kind="Sheet"]}[Data],
				    #"Promoted Headers" = Table.PromoteHeaders(Energy_Sheet, [PromoteAllScalars=true]),
				    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"Date", type datetime}, {"Machine ID", type text}, {"Machine Name", type text}, {"Process", type text}, {"Plant", type text}, {"Energy kWh", type number}, {"Energy per Unit", type number}, {"Energy Cost", type number}})
				in
				    #"Changed Type"
```

## Machine_Master

Worksheet import and type-conversion steps:

```powerquery
let
				    Source = Excel.Workbook(File.Contents("<PATH_TO_SAMPLE_WORKBOOK>"), null, true),
				    Machine_Master_Sheet = Source{[Item="Machine_Master",Kind="Sheet"]}[Data],
				    #"Promoted Headers" = Table.PromoteHeaders(Machine_Master_Sheet, [PromoteAllScalars=true]),
				    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"Machine ID", type text}, {"Machine Name", type text}, {"Process", type text}, {"Plant", type text}, {"Daily Capacity", Int64.Type}})
				in
				    #"Changed Type"
```

## Monthly_Summary

Worksheet import and type-conversion steps:

```powerquery
let
				    Source = Excel.Workbook(File.Contents("<PATH_TO_SAMPLE_WORKBOOK>"), null, true),
				    Monthly_Summary_Sheet = Source{[Item="Monthly_Summary",Kind="Sheet"]}[Data],
				    #"Promoted Headers" = Table.PromoteHeaders(Monthly_Summary_Sheet, [PromoteAllScalars=true]),
				    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"Month", type date}, {"Production_Target", type number}, {"Actual_Production", type number}, {"Good_Production", type number}, {"Downtime_Hours", type number}, {"Achievement %", type number}, {"Quality %", type number}})
				in
				    #"Changed Type"
```

## Orders

Worksheet import and type-conversion steps:

```powerquery
let
				    Source = Excel.Workbook(File.Contents("<PATH_TO_SAMPLE_WORKBOOK>"), null, true),
				    Orders_Sheet = Source{[Item="Orders",Kind="Sheet"]}[Data],
				    #"Promoted Headers" = Table.PromoteHeaders(Orders_Sheet, [PromoteAllScalars=true]),
				    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"Order ID", type text}, {"Order Date", type datetime}, {"Customer", type text}, {"Product Category", type text}, {"Order Qty", Int64.Type}, {"Due Date", type datetime}, {"Delivery Date", type datetime}, {"On Time", type text}, {"Delivery Status", type text}})
				in
				    #"Changed Type"
```

## Production

Worksheet import and type-conversion steps:

```powerquery
let
				    Source = Excel.Workbook(File.Contents("<PATH_TO_SAMPLE_WORKBOOK>"), null, true),
				    Production_Sheet = Source{[Item="Production",Kind="Sheet"]}[Data],
				    #"Promoted Headers" = Table.PromoteHeaders(Production_Sheet, [PromoteAllScalars=true]),
				    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"Date", type datetime}, {"Machine ID", type text}, {"Machine Name", type text}, {"Process", type text}, {"Plant", type text}, {"Production Target", type number}, {"Actual Production", type number}, {"Good Production", type number}, {"Downtime Hours", type number}, {"Run Hours", type number}, {"Performance %", type number}, {"Quality %", type number}})
				in
				    #"Changed Type"
```

## Refresh notes

- The workbook must contain worksheets with the exact names used by the queries: Cost, Digital_Projects, Downtime, Energy, Machine_Master, Monthly_Summary, Orders, and Production.
- The expected column names and types are encoded in each `Table.TransformColumnTypes` step.
- The source is an Excel workbook using `Excel.Workbook` and `File.Contents`; no database/API connector is declared in these query definitions.
