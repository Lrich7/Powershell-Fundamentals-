[lab-11-working-with-data.md](https://github.com/user-attachments/files/31521768/lab-11-working-with-data.md)
# Lab 11 --- Working with Data

## Lab Objective

In this lab, you will practice turning PowerShell objects into useful
administrative data.

You will:

-   Create custom objects with `[PSCustomObject]`.
-   Export objects to CSV.
-   Import CSV data.
-   Inspect imported records.
-   Filter and sort imported data.
-   Export a report without type information.
-   Work with JSON.
-   Build a small asset inventory report.

------------------------------------------------------------------------

## Before You Begin

Complete Lessons 01--11.

Create a safe lab folder:

``` powershell
$labRoot = Join-Path $HOME 'PowerShell-Lab11'
New-Item -Path $labRoot -ItemType Directory -Force
Set-Location $labRoot
```

------------------------------------------------------------------------

# Exercise 1 --- Create a Custom Object

Create an asset object:

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

Inspect it:

``` powershell
$asset | Get-Member
```

Access one property:

``` powershell
$asset.AssetTag
```

### Question

Why is a custom object more useful than placing all four values in one
long string?

``` text
____________________________________________________
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 2 --- Create Multiple Records

Create:

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
    [PSCustomObject]@{
        AssetTag   = 'PRN-3001'
        DeviceType = 'Printer'
        AssignedTo = ''
        Status     = 'Available'
    }
)
```

Display:

``` powershell
$assets
```

Count:

``` powershell
$assets.Count
```

------------------------------------------------------------------------

# Exercise 3 --- Export to CSV

Create a path:

``` powershell
$csvPath = Join-Path $labRoot 'assets.csv'
```

Export:

``` powershell
$assets |
    Export-Csv -Path $csvPath -NoTypeInformation
```

Verify:

``` powershell
Get-Item $csvPath
```

Open the file in a text editor or inspect it with:

``` powershell
Get-Content $csvPath
```

### Question

What became the CSV column headers?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 4 --- Import CSV Data

Import the file:

``` powershell
$importedAssets = Import-Csv $csvPath
```

Display:

``` powershell
$importedAssets
```

Inspect one record:

``` powershell
$importedAssets[0] | Get-Member
```

### Question

Does `Import-Csv` give you structured objects whose properties match the
column names?

``` text
Answer: ______________________
```

------------------------------------------------------------------------

# Exercise 5 --- Filter Imported Data

Find laptops:

``` powershell
$importedAssets |
    Where-Object DeviceType -eq 'Laptop'
```

Find available assets:

``` powershell
$importedAssets |
    Where-Object Status -eq 'Available'
```

Find unassigned assets:

``` powershell
$importedAssets |
    Where-Object {
        [string]::IsNullOrWhiteSpace($_.AssignedTo)
    }
```

### Question

Why is `IsNullOrWhiteSpace()` useful for imported data?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 6 --- Sort and Select

Create a clean inventory view:

``` powershell
$importedAssets |
    Sort-Object AssetTag |
    Select-Object AssetTag, DeviceType, AssignedTo, Status
```

Now display available assets sorted by device type:

``` powershell
# Your command:
```

------------------------------------------------------------------------

# Exercise 7 --- Create a Report

Create:

``` powershell
$reportPath = Join-Path $labRoot 'available-assets.csv'
```

Export only available assets:

``` powershell
$importedAssets |
    Where-Object Status -eq 'Available' |
    Sort-Object DeviceType |
    Export-Csv -Path $reportPath -NoTypeInformation
```

Verify the report.

------------------------------------------------------------------------

# Exercise 8 --- Work with JSON

Convert your asset collection to JSON:

``` powershell
$json = $assets | ConvertTo-Json
$json
```

Save it:

``` powershell
$jsonPath = Join-Path $labRoot 'assets.json'
$json | Set-Content $jsonPath
```

Read and convert it back:

``` powershell
$jsonAssets = Get-Content $jsonPath -Raw |
    ConvertFrom-Json
```

Display:

``` powershell
$jsonAssets
```

------------------------------------------------------------------------

# Exercise 9 --- CSV vs. JSON

In your own words:

``` text
CSV is useful when:
____________________________________________________

JSON is useful when:
____________________________________________________
```

------------------------------------------------------------------------

# End-of-Lab Challenge --- Asset Audit

You have been asked:

> Create a report of all unassigned company assets.

Using the imported `assets.csv` data:

1.  Import the CSV.
2.  Find records where `AssignedTo` is empty or whitespace.
3.  Sort by `DeviceType`, then `AssetTag`.
4.  Display:
    -   `AssetTag`
    -   `DeviceType`
    -   `Status`
5.  Export the results as:

``` text
unassigned-assets.csv
```

Write your solution:

``` powershell
# Your solution:
```

### Bonus

Create another report containing only active laptops.

------------------------------------------------------------------------

# Knowledge Check

1.  Which command imports CSV data as PowerShell objects?

    A. `Get-Csv`\
    B. `Import-Csv`\
    C. `Read-Csv`\
    D. `ConvertFrom-CsvFile`

2.  Why use `-NoTypeInformation` with `Export-Csv`?

    A. To avoid unnecessary type metadata in the CSV.\
    B. To remove headers.\
    C. To encrypt the file.\
    D. To delete properties.

3.  What creates a convenient custom PowerShell object?

    A. `[PSCustomObject]`\
    B. `[CSV]`\
    C. `[ArrayOnly]`\
    D. `[ObjectFile]`

4.  Which pair works with JSON?

    A. `Export-Json` and `Import-Json`\
    B. `ConvertTo-Json` and `ConvertFrom-Json`\
    C. `Write-Json` and `Read-Json`\
    D. `Get-Json` and `Set-Json`

------------------------------------------------------------------------

# Lab Complete

Continue to:

[Lesson 12 — Loops](../lessons/lesson-12-loops.md)

