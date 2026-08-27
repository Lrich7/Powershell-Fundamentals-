[lesson-11-working-with-data.md](https://github.com/user-attachments/files/31519353/lesson-11-working-with-data.md)

# Lesson 11 --- Working with Data

## Learning Objectives

By the end of this lesson, you will be able to:

-   Explain why structured data is useful in PowerShell.
-   Create custom objects with `[PSCustomObject]`.
-   Import structured data from CSV files with `Import-Csv`.
-   Export PowerShell objects to CSV with `Export-Csv`.
-   Use `-NoTypeInformation` when exporting CSV data.
-   Filter, sort, and select properties from imported records.
-   Convert PowerShell objects to JSON with `ConvertTo-Json`.
-   Convert JSON back into PowerShell objects with `ConvertFrom-Json`.
-   Recognize when CSV or JSON is appropriate.
-   Build simple reports from administrative data.

------------------------------------------------------------------------

## Why Work with Data?

PowerShell becomes especially useful when you stop thinking about one
computer, one user, or one asset at a time.

IT administrators regularly work with collections of data such as:

-   Computer inventories
-   Asset lists
-   User accounts
-   License assignments
-   Service information
-   Software inventories
-   Audit results
-   Configuration information

For example, an asset inventory might contain:

``` text
AssetTag   DeviceType   AssignedTo   Status
--------   ----------   ----------   ------
LT-1001    Laptop       Alex         Active
LT-1002    Laptop                    Available
MON-2001   Monitor      Jordan       Active
```

PowerShell can import this information, turn each row into an object,
filter it, sort it, modify how it is displayed, and export new reports.

> **Key Idea:** Structured data lets PowerShell work with individual
> properties instead of forcing you to parse plain text.

------------------------------------------------------------------------

# Objects Are Structured Data

You have already worked with PowerShell objects.

For example:

``` powershell
Get-Service
```

returns service objects.

Each object contains properties such as:

``` text
Name
DisplayName
Status
```

You can select those properties:

``` powershell
Get-Service |
    Select-Object Name, DisplayName, Status
```

You can filter them:

``` powershell
Get-Service |
    Where-Object Status -eq 'Running'
```

You can sort them:

``` powershell
Get-Service |
    Sort-Object Name
```

The same concepts apply when the data comes from a CSV file, JSON file,
API, Microsoft 365, Active Directory, or another source.

------------------------------------------------------------------------

# Creating Custom Objects

Sometimes you need to create your own structured data.

PowerShell provides:

``` powershell
[PSCustomObject]
```

Example:

``` powershell
$asset = [PSCustomObject]@{
    AssetTag   = 'LT-1001'
    DeviceType = 'Laptop'
    AssignedTo = 'Alex'
    Status     = 'Active'
}
```

Display it:

``` powershell
$asset
```

PowerShell presents the properties in a structured format.

You can access individual properties:

``` powershell
$asset.AssetTag
```

``` powershell
$asset.Status
```

You can inspect the object:

``` powershell
$asset | Get-Member
```

------------------------------------------------------------------------

# Why Not Just Use a String?

You could store asset information like this:

``` powershell
$asset = 'LT-1001, Laptop, Alex, Active'
```

But PowerShell does not automatically know which portion represents:

``` text
AssetTag
DeviceType
AssignedTo
Status
```

A custom object does:

``` powershell
$asset = [PSCustomObject]@{
    AssetTag   = 'LT-1001'
    DeviceType = 'Laptop'
    AssignedTo = 'Alex'
    Status     = 'Active'
}
```

Now you can write:

``` powershell
$asset.DeviceType
```

or:

``` powershell
$asset.AssignedTo
```

> **Key Idea:** Use properties to give data meaning.

------------------------------------------------------------------------

# Creating a Collection of Custom Objects

You can place multiple custom objects into an array:

``` powershell
$assets = @(
    [PSCustomObject]@{
        AssetTag   = 'LT-1001'
        DeviceType = 'Laptop'
        AssignedTo = 'Alex'
        Status     = 'Active'
    }

    [PSCustomObject]@{
        AssetTag   = 'LT-1002'
        DeviceType = 'Laptop'
        AssignedTo = ''
        Status     = 'Available'
    }

    [PSCustomObject]@{
        AssetTag   = 'MON-2001'
        DeviceType = 'Monitor'
        AssignedTo = 'Jordan'
        Status     = 'Active'
    }
)
```

You can now use the pipeline normally:

``` powershell
$assets |
    Where-Object DeviceType -eq 'Laptop'
```

or:

``` powershell
$assets |
    Sort-Object AssetTag
```

------------------------------------------------------------------------

# CSV Files

CSV stands for:

``` text
Comma-Separated Values
```

A CSV file stores table-like information.

Example:

``` csv
AssetTag,DeviceType,AssignedTo,Status
LT-1001,Laptop,Alex,Active
LT-1002,Laptop,,Available
MON-2001,Monitor,Jordan,Active
PRN-3001,Printer,,Available
```

The first row contains column names:

``` text
AssetTag
DeviceType
AssignedTo
Status
```

Each remaining row represents a record.

CSV is common in IT because it works well with:

-   Excel
-   Inventory exports
-   User lists
-   Reports
-   Asset management systems
-   Administrative scripts

------------------------------------------------------------------------

# Importing CSV Data

Use:

``` powershell
Import-Csv
```

Example:

``` powershell
$assets = Import-Csv C:\Temp\assets.csv
```

Display:

``` powershell
$assets
```

PowerShell converts each CSV row into an object.

The CSV headers become properties.

For example:

``` powershell
$assets[0].AssetTag
```

``` powershell
$assets[0].DeviceType
```

``` powershell
$assets[0].AssignedTo
```

This is one of the most important features of `Import-Csv`.

------------------------------------------------------------------------

# Inspect Imported Objects

Do not guess what an imported record contains.

Use:

``` powershell
$assets[0] | Get-Member
```

You can also inspect a sample record:

``` powershell
$assets |
    Select-Object -First 1
```

This is the same object-discovery process used earlier in the course.

A useful workflow is:

``` text
Import
   ↓
Inspect
   ↓
Filter
   ↓
Sort
   ↓
Select
   ↓
Export
```

------------------------------------------------------------------------

# Filtering Imported Data

Suppose:

``` powershell
$assets = Import-Csv C:\Temp\assets.csv
```

Find laptops:

``` powershell
$assets |
    Where-Object DeviceType -eq 'Laptop'
```

Find available assets:

``` powershell
$assets |
    Where-Object Status -eq 'Available'
```

Find a specific asset:

``` powershell
$assets |
    Where-Object AssetTag -eq 'LT-1001'
```

Everything you learned about `Where-Object` still applies.

------------------------------------------------------------------------

# Finding Empty Values

Administrative data often contains blank fields.

For example:

``` csv
LT-1002,Laptop,,Available
```

The `AssignedTo` value is empty.

You could test:

``` powershell
$assets |
    Where-Object AssignedTo -eq ''
```

However, real data may contain:

-   Empty strings
-   Spaces
-   `$null`

A useful .NET method is:

``` powershell
[string]::IsNullOrWhiteSpace()
```

Example:

``` powershell
$assets |
    Where-Object {
        [string]::IsNullOrWhiteSpace($_.AssignedTo)
    }
```

This is especially useful for identifying unassigned equipment or
incomplete records.

------------------------------------------------------------------------

# Sorting Imported Data

Sort by asset tag:

``` powershell
$assets |
    Sort-Object AssetTag
```

Sort by device type and then asset tag:

``` powershell
$assets |
    Sort-Object DeviceType, AssetTag
```

Sort descending:

``` powershell
$assets |
    Sort-Object AssetTag -Descending
```

------------------------------------------------------------------------

# Selecting Report Columns

You may not want every property in a report.

Use:

``` powershell
Select-Object
```

Example:

``` powershell
$assets |
    Select-Object AssetTag, DeviceType, AssignedTo
```

This is useful when the source data contains more information than the
final report needs.

------------------------------------------------------------------------

# Exporting CSV Data

Use:

``` powershell
Export-Csv
```

Example:

``` powershell
$assets |
    Export-Csv C:\Temp\asset-report.csv -NoTypeInformation
```

A common pattern is:

``` powershell
Get-Something |
    Where-Object ... |
    Sort-Object ... |
    Select-Object ... |
    Export-Csv ...
```

For example:

``` powershell
$assets |
    Where-Object Status -eq 'Available' |
    Sort-Object DeviceType |
    Select-Object AssetTag, DeviceType, Status |
    Export-Csv C:\Temp\available-assets.csv -NoTypeInformation
```

------------------------------------------------------------------------

# Why Use -NoTypeInformation?

You will commonly see:

``` powershell
-NoTypeInformation
```

with `Export-Csv`.

Example:

``` powershell
Export-Csv C:\Temp\report.csv -NoTypeInformation
```

It prevents unnecessary PowerShell type information from being written
into CSV output on Windows PowerShell versions where that metadata would
otherwise be included.

Modern PowerShell versions generally omit that type information by
default, but using `-NoTypeInformation` remains common and makes the
intent clear.

------------------------------------------------------------------------

# Do Not Use Format-Table Before Export-Csv

This is an important PowerShell reporting rule.

Do **not** build a CSV like this:

``` powershell
Get-Service |
    Format-Table Name, Status |
    Export-Csv C:\Temp\services.csv
```

`Format-Table` prepares objects for screen display. It does not prepare
clean data for export.

Instead:

``` powershell
Get-Service |
    Select-Object Name, Status |
    Export-Csv C:\Temp\services.csv -NoTypeInformation
```

> **Key Idea:** Use `Select-Object` to choose data. Use `Format-*`
> commands only when formatting output for display.

------------------------------------------------------------------------

# Import-Csv Data Types

There is an important detail about CSV files:

``` powershell
Import-Csv
```

normally imports field values as strings.

For example, suppose a CSV contains:

``` csv
Computer,FreeSpaceGB
PC-001,100
PC-002,25
```

After importing:

``` powershell
$data = Import-Csv C:\Temp\computers.csv
```

you may need to convert a numeric field when numeric behavior matters:

``` powershell
[int]$data[0].FreeSpaceGB
```

or during filtering:

``` powershell
$data |
    Where-Object {
        [int]$_.FreeSpaceGB -lt 30
    }
```

> **Key Idea:** Data that looks numeric in a CSV may still be imported
> as text.

------------------------------------------------------------------------

# JSON

JSON stands for:

``` text
JavaScript Object Notation
```

JSON is another common structured-data format.

It is widely used by:

-   APIs
-   Cloud services
-   Configuration files
-   Microsoft Graph
-   Web applications
-   Automation tools

Example:

``` json
{
  "AssetTag": "LT-1001",
  "DeviceType": "Laptop",
  "AssignedTo": "Alex",
  "Status": "Active"
}
```

JSON can represent structures that are more complex than a simple table.

------------------------------------------------------------------------

# Convert PowerShell Objects to JSON

Use:

``` powershell
ConvertTo-Json
```

Example:

``` powershell
$asset = [PSCustomObject]@{
    AssetTag   = 'LT-1001'
    DeviceType = 'Laptop'
    AssignedTo = 'Alex'
    Status     = 'Active'
}

$asset | ConvertTo-Json
```

You can save the result:

``` powershell
$asset |
    ConvertTo-Json |
    Set-Content C:\Temp\asset.json
```

------------------------------------------------------------------------

# Convert JSON to PowerShell Objects

Use:

``` powershell
ConvertFrom-Json
```

Example:

``` powershell
$json = Get-Content C:\Temp\asset.json -Raw

$asset = $json | ConvertFrom-Json
```

Now:

``` powershell
$asset.AssetTag
```

works again because PowerShell has converted the JSON into an object.

------------------------------------------------------------------------

# Why Use -Raw with Get-Content?

Normally:

``` powershell
Get-Content
```

can return a text file one line at a time.

For JSON, you often want the entire file as one string.

Use:

``` powershell
Get-Content C:\Temp\asset.json -Raw
```

Then:

``` powershell
Get-Content C:\Temp\asset.json -Raw |
    ConvertFrom-Json
```

------------------------------------------------------------------------

# CSV vs. JSON

A simple guideline:

  Format   Good For
  -------- -----------------------------------------------------------
  CSV      Rows and columns, Excel, simple reports, inventory lists
  JSON     Nested data, APIs, configuration, cloud/service responses

Use CSV when the information naturally looks like a spreadsheet.

Use JSON when the data has more complex structure or needs to interact
with systems that use JSON.

------------------------------------------------------------------------

# Practical IT Example --- Unassigned Asset Report

Suppose:

``` powershell
$assets = Import-Csv C:\Temp\assets.csv
```

You need a report of unassigned equipment.

Filter:

``` powershell
$unassigned = $assets |
    Where-Object {
        [string]::IsNullOrWhiteSpace($_.AssignedTo)
    }
```

Review:

``` powershell
$unassigned |
    Sort-Object DeviceType, AssetTag |
    Select-Object AssetTag, DeviceType, Status
```

Export:

``` powershell
$unassigned |
    Sort-Object DeviceType, AssetTag |
    Select-Object AssetTag, DeviceType, Status |
    Export-Csv C:\Temp\unassigned-assets.csv -NoTypeInformation
```

This is a realistic example of using PowerShell to transform operational
data into an actionable report.

------------------------------------------------------------------------

# Practical IT Example --- Service Report

PowerShell data does not have to come from a file.

You can export command output directly:

``` powershell
Get-Service |
    Where-Object Status -eq 'Running' |
    Sort-Object Name |
    Select-Object Name, DisplayName, Status |
    Export-Csv C:\Temp\running-services.csv -NoTypeInformation
```

The object pipeline makes this possible.

------------------------------------------------------------------------

# A Useful Reporting Pattern

Many PowerShell reporting tasks follow this pattern:

``` text
Collect
   ↓
Filter
   ↓
Sort
   ↓
Select
   ↓
Export
```

For example:

``` powershell
Import-Csv C:\Temp\assets.csv |
    Where-Object Status -eq 'Available' |
    Sort-Object DeviceType |
    Select-Object AssetTag, DeviceType, Status |
    Export-Csv C:\Temp\available-assets.csv -NoTypeInformation
```

This pattern will appear repeatedly in real PowerShell administration.

------------------------------------------------------------------------

# Common Beginner Mistakes

## Treating Imported Data as Plain Text

Avoid trying to parse CSV output manually when `Import-Csv` can create
objects.

Use:

``` powershell
Import-Csv
```

and work with properties.

------------------------------------------------------------------------

## Formatting Before Exporting

Avoid:

``` powershell
Format-Table
```

before:

``` powershell
Export-Csv
```

Use:

``` powershell
Select-Object
```

instead.

------------------------------------------------------------------------

## Assuming CSV Numbers Are Numeric

Remember that imported CSV fields are generally strings.

Convert when necessary:

``` powershell
[int]$_.FreeSpaceGB
```

------------------------------------------------------------------------

## Overwriting a Report Accidentally

Before exporting important data, confirm the destination path.

You can store it:

``` powershell
$reportPath = 'C:\Temp\report.csv'
```

Then inspect:

``` powershell
$reportPath
```

before exporting.

------------------------------------------------------------------------

# Key Takeaways

-   PowerShell works best with structured objects.
-   `[PSCustomObject]` lets you create your own structured records.
-   `Import-Csv` turns CSV rows into PowerShell objects.
-   CSV headers become object properties.
-   `Export-Csv` turns PowerShell objects into table-like files.
-   `-NoTypeInformation` is commonly used for clean CSV output and
    compatibility.
-   Imported CSV values are generally strings.
-   `ConvertTo-Json` converts objects to JSON.
-   `ConvertFrom-Json` converts JSON into PowerShell objects.
-   CSV is excellent for simple tables and reports.
-   JSON is common with APIs and complex structured data.
-   Do not use `Format-Table` before `Export-Csv`.
-   A useful reporting workflow is:

``` text
Collect → Filter → Sort → Select → Export
```

------------------------------------------------------------------------

# Lab

Continue to:

[Lab 11 --- Working with Data](../labs/lab-11-working-with-data.md)

In the lab, you will create custom asset objects, export and import CSV
data, identify unassigned assets, work with JSON, and build an asset
audit report.

------------------------------------------------------------------------

## Additional Resources

-   [Import-Csv --- Microsoft
    Learn](https://learn.microsoft.com/powershell/module/microsoft.powershell.utility/import-csv)
-   [Export-Csv --- Microsoft
    Learn](https://learn.microsoft.com/powershell/module/microsoft.powershell.utility/export-csv)
-   [about PSCustomObject --- Microsoft
    Learn](https://learn.microsoft.com/powershell/module/microsoft.powershell.core/about/about_pscustomobject)
-   [ConvertTo-Json --- Microsoft
    Learn](https://learn.microsoft.com/powershell/module/microsoft.powershell.utility/convertto-json)
-   [ConvertFrom-Json --- Microsoft
    Learn](https://learn.microsoft.com/powershell/module/microsoft.powershell.utility/convertfrom-json)
