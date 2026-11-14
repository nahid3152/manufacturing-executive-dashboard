# DAX Measures Reference

This document is extracted from the uploaded Power BI Project semantic-model TMDL files. Measure expressions are reproduced from the project definition.

**Measures found:** 66 (64 in `_measure` plus 2 in `Management Alerts`).

> Note: This is a source-code inventory, not a validation of business-rule correctness. In particular, threshold values and denominator choices should be confirmed with business stakeholders before operational use.

## PRODUCTION

### 'Total Production'

```dax
SUM(Production[Actual Production])
		displayFolder: PRODUCTION
		lineageTag: bdee6520-c905-4dfa-ab78-a6f1b3cfe738

		annotation PBI_FormatHint = {"isGeneralNumber":true}
```

### 'Production Target'

```dax
SUM(Production[Production Target])
		displayFolder: PRODUCTION
		lineageTag: 93b77835-54b8-42f7-b32e-11c53318b4cc

		annotation PBI_FormatHint = {"isGeneralNumber":true}
```

### 'Good Production'

```dax
SUM(Production[Good Production])
		displayFolder: PRODUCTION
		lineageTag: a4dd1831-2345-46da-bf52-bed46ecbeb2f

		annotation PBI_FormatHint = {"isGeneralNumber":true}
```

### 'Production Achievement %'

```dax
DIVIDE(
			    [Total Production],
			    [Production Target],
			    0
			)
		formatString: 0.0%;-0.0%;0.0%
		displayFolder: PRODUCTION
		lineageTag: c02a0294-543c-411a-81e4-237c9beff3a4
```

### 'Production Previous Month'

```dax
CALCULATE(
			    [Total Production],
			    DATEADD(Date_Table[Date], -1, MONTH)
			)
		displayFolder: PRODUCTION
		lineageTag: 826e751e-d829-4bf3-abee-0b8141d3020b

		annotation PBI_FormatHint = {"isGeneralNumber":true}
```

### 'Production MoM %'

```dax
DIVIDE(
			    [Total Production] - [Production Previous Month],
			    [Production Previous Month],
			    0
			)
		formatString: 0.0%;-0.0%;0.0%
		displayFolder: PRODUCTION
		lineageTag: ce326cae-b577-488c-9c80-8671a95ce024
```

### 'Production Variance'

```dax
[Total Production] - [Production Target]
		displayFolder: PRODUCTION
		lineageTag: c2ef094f-f18e-4f3a-97ea-3910ac9dbb5c

		annotation PBI_FormatHint = {"isGeneralNumber":true}
```

### 'Production Variance %'

```dax
DIVIDE(
			    [Production Variance],
			    [Production Target],
			    0
			)
		displayFolder: PRODUCTION
		lineageTag: b634f6b7-3f04-4980-aa36-a5d4a067b5fa

		annotation PBI_FormatHint = {"isGeneralNumber":true}
```

### 'Production Variance Status'

```dax
SWITCH(
			    TRUE(),
			    [Production Variance] < 0, "Below Target",
			    [Production Variance] > 0, "Above Target",
			    "On Target"
			)
		displayFolder: PRODUCTION
		lineageTag: 49c06544-463d-455e-8881-a58e147d80c0
```

## DOWNTIME

### 'Total Downtime'

```dax
SUM(Downtime[Downtime Hours])
		displayFolder: DOWNTIME
		lineageTag: f1b72d41-055c-40b4-bddd-5f82ff6ef8de

		annotation PBI_FormatHint = {"isGeneralNumber":true}
```

### 'Downtime Events'

```dax
COUNTROWS(Downtime)
		formatString: #,0
		displayFolder: DOWNTIME
		lineageTag: c6869170-b325-4c98-88b1-1728a723fe77
```

### 'Avg Downtime per Event'

```dax
DIVIDE(
			    [Total Downtime],
			    [Downtime Events],
			    0
			)
		displayFolder: DOWNTIME
		lineageTag: 99a0ce36-58d5-4951-9e37-b39455c1b78d

		annotation PBI_FormatHint = {"isGeneralNumber":true}
```

### 'Downtime %'

```dax
DIVIDE(
			    [Total Downtime],
			    SUM(Production[Run Hours]) + [Total Downtime],
			    0
			)
		formatString: 0.0%;-0.0%;0.0%
		displayFolder: DOWNTIME
		lineageTag: fc566911-11a7-4da3-85bc-f15f84b99750
```

### 'Critical Downtime'

```dax
CALCULATE(
			    [Downtime Events],
			    Downtime[Impact] = "High"
			)
		formatString: #,0
		displayFolder: DOWNTIME
		lineageTag: ac470342-7c22-4207-9cb3-59b71b4a6319
```

### 'Affected Machines'

```dax
DISTINCTCOUNT(Downtime[Machine ID])
		formatString: 0
		displayFolder: DOWNTIME
		lineageTag: 7ffad926-f70b-4ca6-a29b-4eba39b25327
```

## ORDERS

### 'Total Orders'

```dax
DISTINCTCOUNT(Orders[Order ID])
		formatString: #,0
		displayFolder: ORDERS
		lineageTag: 4a2de17c-61b7-4190-818f-9870a505b697
```

### 'Delivered Orders'

```dax
CALCULATE(
			    [Total Orders],
			    Orders[Delivery Status] = "Delivered"
			)
		formatString: #,0
		displayFolder: ORDERS
		lineageTag: 0d3a1111-d5ac-4533-92e0-91fdf8fa87a0
```

### 'On-Time Orders'

```dax
CALCULATE(
			    [Total Orders],
			    Orders[On Time] = "Yes"
			)
		formatString: #,0
		displayFolder: ORDERS
		lineageTag: db0849b5-9622-4fc7-9da8-20802f284df9
```

### 'On-Time Delivery %'

```dax
DIVIDE(
			    [On-Time Orders],
			    [Delivered Orders],
			    0
			)
		formatString: 0.0%;-0.0%;0.0%
		displayFolder: ORDERS
		lineageTag: 3cd16492-1f7d-49c5-b16e-c46381d6bcf2
```

### 'Pending Orders'

```dax
CALCULATE(
			    [Total Orders],
			    Orders[Delivery Status] = "Pending"
			)
		formatString: #,0
		displayFolder: ORDERS
		lineageTag: 7f63642c-fead-4d66-9b56-8fc91b9bd6af
```

### 'Late Orders'

```dax
CALCULATE(
			    [Total Orders],
			    Orders[On Time] = "No"
			)
		formatString: #,0
		displayFolder: ORDERS
		lineageTag: 7489a839-7e66-471b-b5ce-f8678cb4cb50
```

## OEE

### 'Availability %'

```dax
DIVIDE(
			    SUM(Production[Run Hours]),
			    SUM(Production[Run Hours]) +
			    SUM(Production[Downtime Hours]),
			    0
			)
		formatString: 0.0%;-0.0%;0.0%
		displayFolder: OEE
		lineageTag: df6f29a7-2a29-49a9-b74d-a56ee1a8c67f
```

### 'Performance %'

```dax
DIVIDE(
			    [Total Production],
			    [Production Target],
			    0
			)
		formatString: 0.0%;-0.0%;0.0%
		displayFolder: OEE
		lineageTag: 888b2cd7-d3fa-44b8-9d55-97d32052b9e9
```

### 'Quality %'

```dax
DIVIDE(
			    [Good Production],
			    [Total Production],
			    0
			)
		formatString: 0.0%;-0.0%;0.0%
		displayFolder: OEE
		lineageTag: ad06b535-6dcc-4a70-8fa5-751c0eec033b
```

### 'OEE %'

```dax
[Availability %] *
			[Performance %] *
			[Quality %]
		formatString: 0.0%;-0.0%;0.0%
		displayFolder: OEE
		lineageTag: 6a8ada0c-9a1e-4ea4-aa96-d28f69442738
```

## ENERGY & COST

### 'Total Energy'

```dax
SUM(Energy[Energy kWh])
		displayFolder: ENERGY & COST
		lineageTag: 47b995fe-0aef-4914-8665-00711f585b99

		annotation PBI_FormatHint = {"isGeneralNumber":true}
```

### 'Energy Cost'

```dax
SUM(Energy[Energy Cost])
		formatString: #,0.00
		displayFolder: ENERGY & COST
		lineageTag: f0f17e3e-019c-4f8d-841c-14e40ed15b36
```

### 'Total Production Cost'

```dax
SUM(Cost[Total Cost])
		displayFolder: ENERGY & COST
		lineageTag: 03bfe35e-c834-455e-afea-7b6d2529d0f1

		annotation PBI_FormatHint = {"isGeneralNumber":true}
```

### 'Cost Per Unit'

```dax
DIVIDE(
			    [Total Production Cost],
			    [Total Production],
			    0
			)
		displayFolder: ENERGY & COST
		lineageTag: f8f83601-ca97-40db-8b66-89a5900c5a37

		annotation PBI_FormatHint = {"isGeneralNumber":true}
```

### 'Energy per Unit'

```dax
DIVIDE(
			    [Total Energy],
			    [Total Production],
			    0
			)
		displayFolder: ENERGY & COST
		lineageTag: 5ce3ddc2-6669-4291-a4f6-25e733282532

		annotation PBI_FormatHint = {"isGeneralNumber":true}
```

### 'Energy Cost per Unit'

```dax
DIVIDE(
			    [Energy Cost],
			    [Total Production],
			    0
			)
		displayFolder: ENERGY & COST
		lineageTag: b50a8afd-b178-4aad-ad0f-9846caf094ff

		annotation PBI_FormatHint = {"isGeneralNumber":true}
```

### 'Energy Efficiency Status'

```dax
SWITCH(
			    TRUE(),
			    [Energy per Unit] > 1.20, "High Energy",
			    [Energy per Unit] > 1.00, "Energy Watch",
			    "Energy Efficient"
			)
		displayFolder: ENERGY & COST
		lineageTag: e32dee47-c6e4-4d48-a1fa-571d2326ceba
```

### 'Energy Saving Potential'

```dax
VAR CurrentEnergy = [Total Energy]
			VAR TargetEnergy = [Total Production] * 1.00
			RETURN
			MAX(
			    0,
			    CurrentEnergy - TargetEnergy
			)
		displayFolder: ENERGY & COST
		lineageTag: 1ab40eb1-9ce3-4359-b78a-64543ec57c4f

		annotation PBI_FormatHint = {"isGeneralNumber":true}
```

### 'Executive Insight'

```dax
VAR ProductionStatus =
			    IF([Production Achievement %] < 0.90, "Production is below target.", "")
			VAR OEEStatus =
			    IF([OEE %] < 0.70, "OEE requires management attention.", "")
			VAR DowntimeStatus =
			    IF([Total Downtime] >= 150, "Downtime is high.", "")
			VAR DeliveryStatus =
			    IF([On-Time Delivery %] < 0.85, "Delivery performance is at risk.", "")
			RETURN
			    SWITCH(
			        TRUE(),
			        ProductionStatus <> "", ProductionStatus,
			        OEEStatus <> "", OEEStatus,
			        DowntimeStatus <> "", DowntimeStatus,
			        DeliveryStatus <> "", DeliveryStatus,
			        "Overall manufacturing performance is on track."
			    )
		displayFolder: ENERGY & COST
		lineageTag: 5e784861-642d-4c9c-94bf-018d905eb908
```

## COLOR

### 'Achievement Color'

```dax
SWITCH(
			    TRUE(),
			    [Production Achievement %] < 0.90, "#EF4444",
			    [Production Achievement %] < 1.00, "#F59E0B",
			    "#22C55E"
			)
		displayFolder: COLOR
		lineageTag: da87bf08-2626-46e9-a573-ac68fd13e28c
```

### 'OEE Color'

```dax
SWITCH(
			    TRUE(),
			    [OEE %] < 0.70, "#EF4444",
			    [OEE %] < 0.85, "#F59E0B",
			    "#22C55E"
			)
		displayFolder: COLOR
		lineageTag: 1a029073-d48c-4c0d-bcb8-a76925da25fe
```

### 'Downtime Color'

```dax
VAR Hours = [Total Downtime]
			RETURN
			SWITCH(
			    TRUE(),
			    Hours >= 150, "#EF4444",
			    Hours >= 100, "#F59E0B",
			    "#38BDF8"
			)
		displayFolder: COLOR
		lineageTag: d8e122bc-a4f8-4dce-8c26-fc8c846295de
```

### 'Delivery Color'

```dax
SWITCH(
			    TRUE(),
			    [On-Time Delivery %] < 0.85, "#EF4444",
			    [On-Time Delivery %] < 0.95, "#F59E0B",
			    "#22C55E"
			)
		displayFolder: COLOR
		lineageTag: aade5195-1cde-4b85-b44a-9fedf10e34e8
```

### 'Achievement Machine Color'

```dax
VAR Achievement = [Production Achievement %]
			RETURN
			SWITCH(
			    TRUE(),
			    Achievement < 0.90, "#EF4444",
			    Achievement < 1.00, "#F59E0B",
			    "#22C55E"
			)
		displayFolder: COLOR
		lineageTag: 051a3cd6-e79d-4299-b860-6174158f8950
```

### 'Production Variance Color'

```dax
SWITCH(
			    TRUE(),
			    [Production Variance] < 0, "#EF4444",
			    [Production Variance] > 0, "#22C55E",
			    "#38BDF8"
			)
		displayFolder: COLOR
		lineageTag: 364a52da-4e7c-4c7d-b1ec-fa78936c7b32
```

### 'Management Alert Count Color'

```dax
SWITCH(
			    TRUE(),
			    [Management Alert Count] = 0, "#22C55E",
			    [Management Alert Count] = 1, "#F59E0B",
			    "#EF4444"
			)
		displayFolder: COLOR
		lineageTag: 8d37a469-da64-445b-af03-fdb6c8fca674
```

### 'Alert Priority Color'

```dax
VAR Priority =
			    SELECTEDVALUE('Management Alerts'[Priority])
			VAR Area =
			    SELECTEDVALUE('Management Alerts'[Area])
			RETURN
			    SWITCH(
			        TRUE(),
			        Area = "Production" && [Production Achievement %] < 0.90, "#EF4444",
			        Area = "OEE" && [OEE %] < 0.70, "#EF4444",
			        Area = "Downtime" && [Total Downtime] >= 150, "#EF4444",
			        Area = "Delivery" && [On-Time Delivery %] < 0.85, "#EF4444",
			        Priority = "Attention", "#F59E0B",
			        "#22C55E"
			    )
		displayFolder: COLOR
		lineageTag: a2be5d21-d1a3-4493-a14d-c59771d1089b
```

### 'Alert Message Color'

```dax
VAR Priority =
			    [Alert Priority]
			RETURN
			    SWITCH(
			        Priority,
			        "Critical", "#EF4444",
			        "Normal", "#22C55E",
			        "#CBD5E1"
			    )
		displayFolder: COLOR
		lineageTag: 6c503fb8-99d8-479c-b241-5893b3c079c4
```

## COST

### 'Material Cost'

```dax
SUM(Cost[Material Cost])
		displayFolder: COST
		lineageTag: 83bfaf11-816e-419f-a3fb-19d2204df332

		annotation PBI_FormatHint = {"isGeneralNumber":true}
```

### 'Labor Cost'

```dax
SUM(Cost[Labor Cost])
		displayFolder: COST
		lineageTag: ef3ec874-e585-410e-8c58-379ca88a59c6

		annotation PBI_FormatHint = {"isGeneralNumber":true}
```

### 'Overhead Cost'

```dax
SUM(Cost[Overhead Cost])
		displayFolder: COST
		lineageTag: 489203d6-2a30-42cb-9f66-7875f0f03710

		annotation PBI_FormatHint = {"isGeneralNumber":true}
```

### 'Material Cost %'

```dax
DIVIDE(
			    [Material Cost],
			    [Total Production Cost],
			    0
			)
		displayFolder: COST
		lineageTag: b87db45f-8816-4f32-91cf-5f73afdd5e22

		annotation PBI_FormatHint = {"isGeneralNumber":true}
```

### 'Labor Cost %'

```dax
DIVIDE(
			    [Labor Cost],
			    [Total Production Cost],
			    0
			)
		displayFolder: COST
		lineageTag: c220f329-6553-4e0a-bc3a-d8eacf7fbcdd

		annotation PBI_FormatHint = {"isGeneralNumber":true}
```

### 'Cost Efficiency Status'

```dax
SWITCH(
			    TRUE(),
			    [Cost Per Unit] <= 0, "No Cost Data",
			    [Cost Per Unit] > 1.10, "High Cost",
			    [Cost Per Unit] > 1.00, "Cost Watch",
			    "Cost Efficient"
			)
		displayFolder: COST
		lineageTag: 8dca0062-ae97-43f0-95df-8c9bb1aa1c55
```

### 'Cost Saving Potential'

```dax
VAR CurrentCost = [Total Production Cost]
			VAR TargetCost = [Total Production] * 1.00
			RETURN
			MAX(
			    0,
			    CurrentCost - TargetCost
			)
		displayFolder: COST
		lineageTag: 3fcb0ac6-ff54-4030-b33d-6795c352db90

		annotation PBI_FormatHint = {"isGeneralNumber":true}
```

## ROI

### 'Total Projects'

```dax
COUNTROWS(Digital_Projects)
		formatString: 0
		displayFolder: ROI
		lineageTag: c593588f-089a-4341-ac21-e251c24f6e04
```

### 'Total Investment'

```dax
SUM(Digital_Projects[Investment])
		formatString: #,0
		displayFolder: ROI
		lineageTag: 5b335b42-fc44-4f88-b07d-af21917648f1
```

### 'Annual Benefit'

```dax
SUM(Digital_Projects[Annual Benefit])
		formatString: #,0
		displayFolder: ROI
		lineageTag: bd8b58fc-6ab8-4a81-a04a-0a041b89b8b1
```

### 'Avg ROI Improvement %'

```dax
AVERAGE(Digital_Projects[ROI Improvement %])
		formatString: 0.0%;-0.0%;0.0%
		displayFolder: ROI
		lineageTag: d6cad833-7760-4ae2-baf1-b47d52c058e2
```

### 'Net Annual Benefit'

```dax
[Annual Benefit] - [Total Investment]
		formatString: #,0
		displayFolder: ROI
		lineageTag: 23f26f0a-6770-45fd-a74e-687db8b7be87
```

### 'Projects with Positive ROI'

```dax
CALCULATE(
			    [Total Projects],
			    Digital_Projects[ROI Improvement %] > 0
			)
		formatString: 0
		displayFolder: ROI
		lineageTag: dade8bbe-6a09-4f23-8bbe-349793c6c5d7
```

## Alert

### 'Production Alert'

```dax
IF(
			    [Production Achievement %] < 0.90,
			    "Production below target",
			    "Production on track"
			)
		displayFolder: Alert
		lineageTag: 7e227408-5f10-4134-91d5-c96d71d168f4
```

### 'OEE Alert'

```dax
IF(
			    [OEE %] < 0.70,
			    "OEE critical",
			    "OEE on track"
			)
		displayFolder: Alert
		lineageTag: a3481e13-0086-44ba-9616-90dc7639d1d7
```

### 'Downtime Alert'

```dax
IF(
			    [Total Downtime] >= 150,
			    "High downtime",
			    "Downtime under control"
			)
		displayFolder: Alert
		lineageTag: 1c58f4f8-74b7-43ce-9007-ddd5aeed614f
```

### 'Delivery Alert'

```dax
IF(
			    [On-Time Delivery %] < 0.85,
			    "Delivery risk",
			    "Delivery on track"
			)
		displayFolder: Alert
		lineageTag: 8a3a8f7e-c271-45d8-9c7b-4174bf7e1083
```

### 'Management Alert'

```dax
VAR ProductionStatus =
			    IF([Production Achievement %] < 0.90, 1, 0)
			
			VAR OEEStatus =
			    IF([OEE %] < 0.70, 1, 0)
			
			VAR DowntimeStatus =
			    IF([Total Downtime] >= 150, 1, 0)
			
			VAR DeliveryStatus =
			    IF([On-Time Delivery %] < 0.85, 1, 0)
			
			VAR TotalAlerts =
			    ProductionStatus +
			    OEEStatus +
			    DowntimeStatus +
			    DeliveryStatus
			
			RETURN
			SWITCH(
			    TRUE(),
			    TotalAlerts = 0, "✓ All key KPIs are on track",
			    TotalAlerts = 1, "⚠ 1 management attention required",
			    "🔴 Multiple management attention areas"
			)
		displayFolder: Alert
		lineageTag: c677ea07-f061-4928-9803-ccf842b1d2d9
```

### 'Management Alert Count'

```dax
VAR ProductionStatus =
			    IF([Production Achievement %] < 0.90, 1, 0)
			VAR OEEStatus =
			    IF([OEE %] < 0.70, 1, 0)
			VAR DowntimeStatus =
			    IF([Total Downtime] >= 150, 1, 0)
			VAR DeliveryStatus =
			    IF([On-Time Delivery %] < 0.85, 1, 0)
			RETURN
			    ProductionStatus +
			    OEEStatus +
			    DowntimeStatus +
			    DeliveryStatus
		formatString: 0
		displayFolder: Alert
		lineageTag: 0c63138e-69f9-45b8-8bab-b5086c9c93c7
```

## Uncategorized

### 'Machine Achievement %'

```dax
DIVIDE(
			    [Total Production],
			    [Production Target],
			    0
			)
		lineageTag: ae4e72ef-3f22-4b03-af1f-127b33705ffb

		annotation PBI_FormatHint = {"isGeneralNumber":true}
```

### 'Machine Risk Score'

```dax
VAR Achievement = [Machine Achievement %]
			VAR Downtime = [Total Downtime]
			RETURN
			    (1 - Achievement) * 100
			    + Downtime
		lineageTag: 2b316f69-90c3-4b5b-8201-801ce1576550

		annotation PBI_FormatHint = {"isGeneralNumber":true}
```

## Management Alerts

### 'Alert Message'

```dax
VAR Area =
			    SELECTEDVALUE('Management Alerts'[Area])
			RETURN
			    SWITCH(
			        Area,
			        "Production", [Production Alert],
			        "OEE", [OEE Alert],
			        "Downtime", [Downtime Alert],
			        "Delivery", [Delivery Alert],
			        BLANK()
			    )
		lineageTag: b02e3742-8af4-487b-b2d8-58fbd3ba9321
```

### 'Alert Priority'

```dax
VAR Area =
			    SELECTEDVALUE('Management Alerts'[Area])
			RETURN
			    SWITCH(
			        TRUE(),
			        Area = "Production" && [Production Achievement %] < 0.90, "Critical",
			        Area = "OEE" && [OEE %] < 0.70, "Critical",
			        Area = "Downtime" && [Total Downtime] >= 150, "Critical",
			        Area = "Delivery" && [On-Time Delivery %] < 0.85, "Critical",
			        "Normal"
			    )
		lineageTag: 39c0fd09-d2ba-4ece-b7bf-aaeaa4a9c9d0
```
