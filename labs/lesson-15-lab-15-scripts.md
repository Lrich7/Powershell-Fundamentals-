# Lab 15 --- Scripts

## Lab Objective

This lab is your first larger PowerShell build.

Instead of practicing isolated syntax, you will create a reusable `.ps1`
administrative script.

You will:

-   Create and run a script.
-   Organize code into sections.
-   Add comments.
-   Add script parameters.
-   Use functions.
-   Use conditions and pipelines.
-   Use `$PSScriptRoot`.
-   Add `Write-Verbose`.
-   Add comment-based help.
-   Test a script safely.
-   Build a repeatable IT reporting tool.

------------------------------------------------------------------------

## Before You Begin

Complete Lessons 01--15.

Create:

``` powershell
$labRoot = Join-Path $HOME 'PowerShell-Lab15'
New-Item -Path $labRoot -ItemType Directory -Force
Set-Location $labRoot
```

Use a code editor such as Visual Studio Code, PowerShell ISE where
available, or another plain-text editor.

------------------------------------------------------------------------

# Exercise 1 --- Your First Script

Create:

``` text
hello.ps1
```

Add:

``` powershell
'Hello from a PowerShell script!'
```

Save it.

Run:

``` powershell
.\hello.ps1
```

### Question

What does `.\` tell PowerShell?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 2 --- Add a Parameter

Create:

``` text
show-computer.ps1
```

Add:

``` powershell
param (
    [string]$ComputerName = $env:COMPUTERNAME
)

"Computer selected: $ComputerName"
```

Run:

``` powershell
.\show-computer.ps1
```

Then:

``` powershell
.\show-computer.ps1 -ComputerName 'PC-001'
```

------------------------------------------------------------------------

# Exercise 3 --- Build a Script Folder

Inside `$labRoot`, create:

``` text
Data
Reports
```

Then create:

``` text
Data\assets.csv
```

with:

``` csv
AssetTag,DeviceType,AssignedTo,Status
LT-1001,Laptop,Alex,Active
LT-1002,Laptop,,Available
MON-2001,Monitor,Jordan,Active
PRN-3001,Printer,,Available
```

------------------------------------------------------------------------

# Exercise 4 --- Use \$PSScriptRoot

Create:

``` text
asset-report.ps1
```

Start with:

``` powershell
$dataPath = Join-Path $PSScriptRoot 'Data\assets.csv'
$reportPath = Join-Path $PSScriptRoot 'Reports\unassigned-assets.csv'
```

### Question

Why is `$PSScriptRoot` preferable to hard-coding your own user profile
path?

``` text
____________________________________________________
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 5 --- Add a Function

Inside `asset-report.ps1`, create:

``` powershell
function Get-UnassignedAsset {
    param (
        [array]$Assets
    )

    $Assets |
        Where-Object {
            [string]::IsNullOrWhiteSpace($_.AssignedTo)
        }
}
```

Do not run the entire script yet.

------------------------------------------------------------------------

# Exercise 6 --- Add the Main Workflow

Below the function, add logic that:

1.  Tests whether `$dataPath` exists.
2.  Imports the CSV.
3.  Calls `Get-UnassignedAsset`.
4.  Sorts by `DeviceType`, then `AssetTag`.
5.  Displays the result.
6.  Exports the result to `$reportPath`.

Try to write it before looking back at earlier labs.

------------------------------------------------------------------------

# Exercise 7 --- Add a Condition

If the data file does not exist, the script should display a clear
message rather than continuing.

Example behavior:

``` text
Asset data file was not found.
```

Use:

``` text
if
else
```

or an early exit approach discussed in your lesson.

------------------------------------------------------------------------

# Exercise 8 --- Add Write-Verbose

Add useful diagnostic messages such as:

``` powershell
Write-Verbose "Importing asset data from $dataPath"
```

and:

``` powershell
Write-Verbose "Exporting report to $reportPath"
```

Run normally:

``` powershell
.\asset-report.ps1
```

Then:

``` powershell
.\asset-report.ps1 -Verbose
```

> If you want a script-level `-Verbose` common parameter, use
> `[CmdletBinding()]` before `param()`.

------------------------------------------------------------------------

# Exercise 9 --- Add a Script Parameter

Modify the script so the caller can choose the report output filename or
path.

For example:

``` powershell
param (
    [string]$OutputPath = (Join-Path $PSScriptRoot 'Reports\unassigned-assets.csv')
)
```

Use the parameter throughout the script.

Test both:

``` powershell
.\asset-report.ps1
```

and a custom output path.

------------------------------------------------------------------------

# Exercise 10 --- Add Comment-Based Help

At the top of the script, add help containing at least:

``` text
.SYNOPSIS
.DESCRIPTION
.PARAMETER
.EXAMPLE
```

Then try:

``` powershell
Get-Help .\asset-report.ps1
```

and:

``` powershell
Get-Help .\asset-report.ps1 -Examples
```

------------------------------------------------------------------------

# Exercise 11 --- Organize the Script

Review your script.

Use clear sections such as:

``` powershell
# Parameters

# Paths

# Functions

# Main
```

Remove unnecessary experimentation and duplicate commands.

### Review Questions

Can another person tell what the script does?

``` text
Yes / No
```

Are paths portable?

``` text
Yes / No
```

Does it change anything outside the lab folder?

``` text
Yes / No
```

------------------------------------------------------------------------

# Exercise 12 --- Execution Policy Awareness

View:

``` powershell
Get-ExecutionPolicy
```

and:

``` powershell
Get-ExecutionPolicy -List
```

Do **not** change company security settings simply to complete this lab.

If script execution is blocked on a managed computer, review:

``` powershell
Get-Help about_Execution_Policies
```

and follow your organization's policy.

### Question

Is execution policy a reason to blindly lower security settings?

``` text
Answer: ______________________
```

------------------------------------------------------------------------

# End-of-Lab Project --- Asset Audit Script

Your final script should be named:

``` text
asset-audit.ps1
```

## Scenario

> IT maintains a CSV hardware inventory. You need a repeatable script
> that identifies assets needing assignment review and creates a report.

## Requirements

Your script must:

1.  Be a `.ps1` file.
2.  Use `[CmdletBinding()]`.
3.  Accept an optional input CSV path.
4.  Accept an optional output CSV path.
5.  Use `$PSScriptRoot` for sensible defaults.
6.  Test whether the input file exists.
7.  Import the CSV.
8.  Use at least one function.
9.  Identify records with an empty `AssignedTo`.
10. Sort the results.
11. Display a useful console summary.
12. Export the report.
13. Use `Write-Verbose` at least twice.
14. Include comments.
15. Include comment-based help.

### Suggested Console Summary

Your script might report information such as:

``` text
Total assets: 4
Unassigned assets: 2
Report created successfully.
```

Do not copy that output requirement blindly; generate the counts from
your actual data.

------------------------------------------------------------------------

# Testing Checklist

Before considering the script complete, test:

``` text
[ ] Default input path works
[ ] Default output path works
[ ] Custom output path works
[ ] Missing input file is handled
[ ] Empty AssignedTo values are detected
[ ] Output CSV opens correctly
[ ] -Verbose produces useful messages
[ ] Get-Help displays script help
[ ] Script does not modify the source CSV
```

------------------------------------------------------------------------

# Stretch Challenge

Add a parameter:

``` text
-Status
```

that optionally limits the report to a specific asset status.

For example:

``` powershell
.\asset-audit.ps1 -Status Available
```

Do not hard-code the answer. Use your function, conditions, filtering,
and parameters.

------------------------------------------------------------------------

# Reflection

At this point you have moved from typing isolated commands to creating a
repeatable administrative workflow.

Your script can now combine:

``` text
Commands
Variables
Objects
Pipelines
Arrays
Operators
Files
Data
Loops
Conditions
Functions
Parameters
Help
```

That is the foundation of PowerShell automation.

------------------------------------------------------------------------

# Knowledge Check

1.  What extension does a PowerShell script use?

    A. `.cmd`\
    B. `.ps1`\
    C. `.psm`\
    D. `.pwsh`

2.  What does `$PSScriptRoot` represent?

    A. The Windows directory\
    B. The directory containing the running script\
    C. The current user's desktop\
    D. The PowerShell installation directory

3.  What does `param()` do in a script?

    A. Defines script parameters\
    B. Imports modules\
    C. Starts loops\
    D. Exports reports

4.  Why use `Write-Verbose`?

    A. To provide optional diagnostic/detail output\
    B. To force errors\
    C. To delete logs\
    D. To replace comments

5.  Why should administrative scripts be tested carefully?

    A. Repeatable automation can also repeat mistakes quickly.\
    B. PowerShell scripts cannot be edited later.\
    C. Every script requires administrator access.\
    D. Scripts cannot use `-WhatIf`.

------------------------------------------------------------------------

# Lab Complete

You have completed the first major PowerShell scripting milestone.

The next lessons can build on this script by adding stronger error
handling, reusable modules, remoting, and real administrative tooling.
