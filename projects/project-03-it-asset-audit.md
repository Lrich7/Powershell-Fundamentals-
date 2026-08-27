# Project 03 --- IT Asset Audit Tool

## Project Checkpoint

**Recommended after:** Lessons 11--15

At this point, you know enough PowerShell to build a small
administrative tool instead of a collection of individual commands.

------------------------------------------------------------------------

## Scenario

IT maintains a CSV inventory of company hardware.

Management wants simple reports showing which assets are assigned,
available, unassigned, and retired.

Build a reusable PowerShell asset audit script.

------------------------------------------------------------------------

## Skills Practiced

-   Working with CSV data
-   Objects and custom objects
-   Loops
-   Conditions
-   Functions
-   Scripts and parameters
-   Filtering and sorting
-   Exporting reports

------------------------------------------------------------------------

## Sample Input

Create or use an `assets.csv` file with fields similar to:

``` csv
AssetTag,DeviceType,AssignedTo,Status
LT-1001,Laptop,Alex,Active
LT-1002,Laptop,,Available
MON-2001,Monitor,Jordan,Active
PRN-3001,Printer,,Available
LT-1003,Laptop,Taylor,Retired
```

You may add more sample records.

------------------------------------------------------------------------

## Requirements

Create:

``` text
asset-audit.ps1
```

Your script must:

1.  Import asset data from CSV.
2.  Verify the input file exists.
3.  Count the total assets.
4.  Identify unassigned assets.
5.  Identify available assets.
6.  Identify retired assets.
7.  Group or summarize assets by device type.
8.  Sort report output logically.
9.  Use at least **two functions**.
10. Use at least one loop.
11. Use conditions.
12. Export useful CSV reports.
13. Accept at least one script parameter.

------------------------------------------------------------------------

## Required Reports

Create an output folder and generate at least:

``` text
unassigned-assets.csv
available-assets.csv
retired-assets.csv
```

Also create a summary on screen containing information such as:

``` text
Total assets
Assigned assets
Unassigned assets
Available assets
Retired assets
Counts by device type
```

Generate the counts from the data. Do not hard-code them.

------------------------------------------------------------------------

## Function Requirement

Decide how to break the problem into reusable pieces.

Possible ideas include:

``` text
Import-AssetData
Get-UnassignedAsset
Get-AssetSummary
Export-AssetReport
```

You do not have to use these names.

Choose functions that make sense for your design.

------------------------------------------------------------------------

## Data Quality Challenge

Real inventories are not always perfect.

Your script should recognize at least one data-quality problem, such as:

``` text
Blank AssetTag
Blank DeviceType
Blank AssignedTo
Unknown Status
```

Decide whether the problem should be:

``` text
Reported
Skipped
Flagged for review
```

Document your decision.

------------------------------------------------------------------------

## Minimum Deliverables

``` text
[ ] asset-audit.ps1
[ ] Sample assets.csv
[ ] At least 2 functions
[ ] At least 1 parameter
[ ] Loop usage
[ ] Conditions
[ ] CSV import
[ ] CSV export
[ ] Multiple reports
[ ] Summary output
[ ] Data-quality check
```

------------------------------------------------------------------------

## Stretch Challenges

-   Add JSON output.
-   Allow filtering by device type.
-   Add a report timestamp.
-   Add an `AssetAge` field if purchase dates are available.
-   Create a report of records requiring manual review.
-   Use `$PSScriptRoot` so the project is portable.

------------------------------------------------------------------------

## Reflection

What part of this script should become a reusable function?

``` text
____________________________________________________
```

What could go wrong if this were used with a real company inventory?

``` text
____________________________________________________
```

What would you validate before trusting the report?

``` text
____________________________________________________
```

------------------------------------------------------------------------

## Project Complete

Continue with Lessons 16--19 before beginning Project 04.
