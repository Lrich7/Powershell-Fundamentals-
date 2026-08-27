[lesson-22-automation-projects.md](https://github.com/user-attachments/files/31518667/lesson-22-automation-projects.md)

# Lesson 22 --- Automation Projects

## Learning Objectives

By the end of this lesson, you will be able to:

-   Plan a PowerShell automation project before writing code.
-   Break an administrative process into smaller steps.
-   Identify inputs, outputs, dependencies, and risks.
-   Choose appropriate PowerShell commands and modules.
-   Build automation using functions and a main script.
-   Add validation, logging, and error handling.
-   Use `-WhatIf` and confirmation patterns where appropriate.
-   Build useful read-only reports before automating changes.
-   Test automation in stages.
-   Document and version-control PowerShell projects.
-   Apply the skills from the entire course to realistic IT projects.

------------------------------------------------------------------------

## From Commands to Automation

At the beginning of this course, PowerShell may have looked like
individual commands:

``` powershell
Get-Service
Get-Process
Get-ChildItem
```

You then learned how those commands connect to:

``` text
Objects
Pipelines
Variables
Collections
Operators
Files
Data
Loops
Conditions
Functions
Scripts
Error handling
Modules
Remoting
Windows administration
Directory administration
Microsoft 365
```

Now the goal is to combine those skills into complete automation
projects.

> **Key Idea:** Good automation is not simply making a task run
> automatically. It makes the task repeatable, understandable, testable,
> and safe.

------------------------------------------------------------------------

# Start with the Process

Before writing PowerShell, write down the manual process.

Suppose the task is:

``` text
Create an asset report.
```

Break it down:

``` text
1. Locate the asset data
2. Import the records
3. Validate required columns
4. Identify active assets
5. Identify unassigned assets
6. Group devices by type
7. Export reports
8. Record when the report was generated
```

Now you have a workflow that PowerShell can automate.

------------------------------------------------------------------------

# Define the Goal

A good project begins with a clear statement.

Example:

``` text
Goal:
Create a repeatable PowerShell script that imports the company
asset inventory and produces active, unassigned, and summary CSV reports.
```

Avoid vague goals such as:

``` text
Automate assets.
```

A clear goal helps define when the project is finished.

------------------------------------------------------------------------

# Define Inputs and Outputs

Ask:

``` text
What information does the script need?
What should the script produce?
```

Example:

``` text
Inputs
------
assets.csv
Output folder

Outputs
-------
active-assets.csv
unassigned-assets.csv
asset-summary.csv
log file
```

This helps define script parameters.

------------------------------------------------------------------------

# Identify Dependencies

Your automation may depend on:

``` text
PowerShell version
Modules
Permissions
Network access
Input files
Microsoft 365 connectivity
Active Directory connectivity
Remote computers
Folder locations
```

Document dependencies instead of assuming they exist.

For scripts, you may use:

``` powershell
#Requires -Version 7.0
```

or module requirements when appropriate.

------------------------------------------------------------------------

# Identify Risk

Classify what the automation does.

## Low Risk

``` text
Read information
Generate reports
Export CSV files
Check configuration
```

## Higher Risk

``` text
Modify users
Change group membership
Restart services
Move files
Disable accounts
Delete objects
Bulk changes
```

Start with low-risk automation whenever possible.

------------------------------------------------------------------------

# Build a Read-Only Version First

Suppose your final goal is to disable stale accounts.

Do **not** begin with:

``` powershell
Disable-ADAccount
```

Begin with a report identifying the accounts that would be targeted.

Conceptually:

``` text
Find candidates
      ↓
Export report
      ↓
Human review
      ↓
Approved target list
      ↓
Automation change
```

This creates a safety boundary between discovery and action.

------------------------------------------------------------------------

# Project Structure

A small PowerShell project might look like:

``` text
AssetReport\
│
├── README.md
├── asset-report.ps1
├── data\
│   └── assets.csv
├── output\
├── logs\
└── functions\
```

Larger projects may eventually become modules.

For beginner projects, keep the structure understandable rather than
overly complex.

------------------------------------------------------------------------

# Script Header

Document the purpose of the script.

Example:

``` powershell
<#
.SYNOPSIS
Creates asset inventory reports.

.DESCRIPTION
Imports asset inventory data and generates reports for active,
unassigned, and summarized assets.

.PARAMETER InputPath
Path to the asset inventory CSV.

.PARAMETER OutputFolder
Folder where reports are written.

.EXAMPLE
.\asset-report.ps1 -InputPath .\data\assets.csv
#>
```

------------------------------------------------------------------------

# Parameters

Avoid hard-coding values that should be configurable.

Example:

``` powershell
param (
    [Parameter(Mandatory)]
    [string]$InputPath,

    [string]$OutputFolder = (Join-Path $PSScriptRoot 'output')
)
```

Now the same script can work with different files and locations.

------------------------------------------------------------------------

# Validate Inputs

Before doing work:

``` powershell
if (-not (Test-Path $InputPath)) {
    throw "Input file not found: $InputPath"
}
```

Check output locations:

``` powershell
if (-not (Test-Path $OutputFolder)) {
    New-Item -Path $OutputFolder -ItemType Directory
}
```

Validation prevents confusing failures later.

------------------------------------------------------------------------

# Functions

Break repeated or logically separate work into functions.

Example:

``` powershell
function Import-AssetInventory {
    param (
        [Parameter(Mandatory)]
        [string]$Path
    )

    Import-Csv -Path $Path -ErrorAction Stop
}
```

Another:

``` powershell
function Get-UnassignedAsset {
    param (
        [Parameter(Mandatory)]
        [object[]]$Asset
    )

    $Asset |
        Where-Object {
            [string]::IsNullOrWhiteSpace($_.AssignedTo)
        }
}
```

Functions make the main script easier to read.

------------------------------------------------------------------------

# Main Script

After defining functions, the main workflow should be easy to
understand.

Example:

``` powershell
$assets = Import-AssetInventory -Path $InputPath

$unassigned = Get-UnassignedAsset -Asset $assets

$unassigned |
    Export-Csv `
        -Path (Join-Path $OutputFolder 'unassigned-assets.csv') `
        -NoTypeInformation
```

The main script reads like the process itself.

------------------------------------------------------------------------

# Error Handling

Wrap operations that may fail.

Example:

``` powershell
try {
    $assets = Import-AssetInventory -Path $InputPath
}
catch {
    Write-Error "Asset import failed: $($_.Exception.Message)"
    return
}
```

Do not use `try/catch` randomly around every line.

Use it where you have a meaningful recovery or reporting strategy.

------------------------------------------------------------------------

# Logging

Automation should provide a record of important events.

A simple logging function:

``` powershell
function Write-Log {
    param (
        [Parameter(Mandatory)]
        [string]$Message,

        [string]$Path = (Join-Path $PSScriptRoot 'automation.log')
    )

    $entry = '{0} - {1}' -f (Get-Date -Format 'yyyy-MM-dd HH:mm:ss'), $Message

    Add-Content -Path $Path -Value $entry
}
```

Use:

``` powershell
Write-Log 'Asset report started.'
```

and:

``` powershell
Write-Log "Imported $($assets.Count) records."
```

------------------------------------------------------------------------

# What Should Be Logged?

Useful events include:

``` text
Script start
Script finish
Input source
Number of records processed
Important decisions
Changes made
Warnings
Errors
Output locations
```

Avoid logging sensitive information such as passwords, access tokens, or
secrets.

------------------------------------------------------------------------

# Verbose Output

Logging and verbose output serve different purposes.

Use:

``` powershell
Write-Verbose
```

for optional live diagnostic information.

Example:

``` powershell
Write-Verbose "Importing $InputPath"
```

Use a log file when you need a persistent record.

------------------------------------------------------------------------

# SupportsShouldProcess

Advanced functions that make changes can support:

``` text
-WhatIf
-Confirm
```

with:

``` powershell
[CmdletBinding(SupportsShouldProcess)]
```

Example structure:

``` powershell
function Remove-OldFile {
    [CmdletBinding(SupportsShouldProcess)]
    param (
        [Parameter(Mandatory)]
        [string]$Path
    )

    if ($PSCmdlet.ShouldProcess($Path, 'Remove file')) {
        Remove-Item -Path $Path
    }
}
```

Now the function can support:

``` powershell
Remove-OldFile -Path C:\Temp\old.log -WhatIf
```

This is a major safety improvement for functions that make changes.

------------------------------------------------------------------------

# Dry Runs

For higher-risk automation, design a way to see the intended actions
before applying them.

Possible approaches include:

``` text
-WhatIf
Preview report
Read-only mode
Approved input list
Test environment
```

The goal is to answer:

``` text
Exactly what will this automation change?
```

before it changes anything.

------------------------------------------------------------------------

# Test in Stages

Do not test a new bulk automation project against the entire
environment.

Use:

``` text
1 record
   ↓
Small test group
   ↓
Lab environment
   ↓
Limited production sample
   ↓
Broader deployment
```

Verify results after each stage.

------------------------------------------------------------------------

# Project 1 --- Windows Health Report

A useful beginner automation project can collect:

``` text
Computer name
Manufacturer
Model
Windows version
Last boot time
Disk space
Selected service status
Recent system errors
```

Potential output:

``` text
windows-health.csv
```

Skills used:

``` text
CIM
Objects
Calculated properties
Event logs
Functions
CSV
Error handling
```

------------------------------------------------------------------------

# Project 2 --- Asset Inventory Report

Input:

``` text
assets.csv
```

Outputs:

``` text
active-assets.csv
unassigned-assets.csv
retired-assets.csv
asset-summary.csv
```

Skills used:

``` text
Import-Csv
Objects
Where-Object
Group-Object
Functions
Conditions
Export-Csv
Logging
```

------------------------------------------------------------------------

# Project 3 --- Active Directory User Audit

In an authorized AD environment, create a read-only report containing:

``` text
Display name
Username
Department
Enabled status
Last logon date
Group information
```

Skills used:

``` text
ActiveDirectory module
Filtering
Properties
Functions
Error handling
CSV
```

Keep the first version read-only.

------------------------------------------------------------------------

# Project 4 --- Microsoft 365 User Report

In an authorized Microsoft 365 tenant, collect:

``` text
Display name
User principal name
Department
Account enabled status
Selected licensing information
```

Skills used:

``` text
Microsoft Graph
Modules
Authentication
Objects
Permissions
CSV
Error handling
```

Again, reporting is a safer starting point than bulk cloud changes.

------------------------------------------------------------------------

# Project 5 --- Multi-Computer Service Report

In an authorized remoting lab:

``` text
Computer
Service
Status
Checked time
```

Skills used:

``` text
Remoting
Arrays
Invoke-Command
Functions
Error handling
Objects
CSV
```

Start with only a few lab computers.

------------------------------------------------------------------------

# Project 6 --- New Employee Preparation Report

Rather than immediately automating account creation, begin with an input
validation tool.

Input might include:

``` text
First name
Last name
Department
Manager
Job title
Start date
Requested hardware
Required access
```

The script can:

``` text
Validate required fields
Normalize names
Generate proposed usernames
Identify missing information
Produce a preparation report
```

This can later become the foundation for more advanced onboarding
automation.

------------------------------------------------------------------------

# Project 7 --- Offboarding Checklist Generator

Input:

``` text
Username
Manager
Termination date
Standard or emergency offboarding
```

Output:

``` text
Account actions required
Group/access review
Mailbox actions
License actions
Hardware return
Documentation checklist
```

Initially, generate the checklist without making changes.

This allows the automation to assist the administrator while preserving
human review.

------------------------------------------------------------------------

# Project 8 --- Large File Report

Build a script that:

``` text
Accepts a folder
Scans files
Finds files above a size threshold
Calculates readable sizes
Sorts largest to smallest
Exports a CSV report
```

Skills used:

``` text
Files
Parameters
Calculated properties
Functions
Error handling
CSV
```

This is an excellent standalone portfolio project.

------------------------------------------------------------------------

# Git and Version Control

Store PowerShell projects in version control when appropriate.

Useful repository files include:

``` text
README.md
scripts
modules
sample data
documentation
```

Do **not** commit:

``` text
Passwords
Secrets
Access tokens
Private keys
Production exports containing sensitive information
```

Use `.gitignore` where appropriate to keep generated reports, logs, or
local configuration out of the repository.

------------------------------------------------------------------------

# README Documentation

A useful project README should explain:

``` text
What the project does
Requirements
Required modules
Permissions
Inputs
Outputs
How to run it
Examples
Known limitations
Safety considerations
```

Documentation is part of the automation project, not an afterthought.

------------------------------------------------------------------------

# Code Review Checklist

Before considering a script complete, ask:

``` text
Does it have a clear purpose?
Are parameters used instead of unnecessary hard-coded values?
Are inputs validated?
Are errors handled meaningfully?
Does it preserve objects where practical?
Does it avoid exposing secrets?
Are destructive operations protected?
Can I preview changes?
Does it log useful information?
Is the code readable?
Is there help/documentation?
Have I tested a small scope?
Have I verified the results?
```

------------------------------------------------------------------------

# Course Automation Pattern

Across this course, a strong PowerShell automation pattern has emerged:

``` text
Discover
   ↓
Get Help
   ↓
Inspect Objects
   ↓
Collect Data
   ↓
Filter / Sort
   ↓
Store in Variables
   ↓
Loop / Decide
   ↓
Wrap Reusable Logic in Functions
   ↓
Validate
   ↓
Handle Errors
   ↓
Preview Changes
   ↓
Act
   ↓
Verify
   ↓
Export / Log
   ↓
Document
```

This is more valuable than memorizing hundreds of cmdlets.

------------------------------------------------------------------------

# Final Challenge

Build one complete PowerShell project using the course concepts.

A good first choice is:

``` text
IT System Inventory Reporter
```

The project should:

``` text
1. Accept parameters
2. Validate input
3. Use at least two functions
4. Retrieve structured PowerShell objects
5. Filter or transform data
6. Use a condition
7. Use a loop where appropriate
8. Handle at least one possible error
9. Produce a structured report
10. Write useful log information
11. Include comment-based help
12. Include a README
```

Optional advanced additions:

``` text
Remote computer support
Active Directory data
Microsoft 365 data
-WhatIf support
Module packaging
```

------------------------------------------------------------------------

# Key Takeaways

-   Automation begins with understanding the manual process.
-   Define a clear goal before writing code.
-   Identify inputs, outputs, dependencies, permissions, and risk.
-   Build read-only discovery and reporting before automated changes.
-   Use parameters instead of unnecessary hard-coded values.
-   Validate prerequisites before performing work.
-   Use functions to organize reusable logic.
-   Handle errors where you can respond meaningfully.
-   Log important events without recording secrets.
-   `SupportsShouldProcess` can add `-WhatIf` and `-Confirm` behavior to
    advanced functions.
-   Test automation in progressively larger stages.
-   Document requirements, usage, examples, and safety considerations.
-   Use version control for maintainable PowerShell projects.
-   Never store secrets or sensitive production exports in public
    repositories.
-   The most important PowerShell skill is not memorizing commands---it
    is knowing how to discover, combine, test, and safely automate them.

------------------------------------------------------------------------

# Lab

Continue to:

[Lab 22 — Automation Projects](../labs/lesson-22-lab-22-automation-projects.md)

The lab serves as a capstone. You will plan and build a small PowerShell
automation project using parameters, functions, objects, pipelines,
conditions, error handling, logging, structured output, and
documentation.

------------------------------------------------------------------------

 🎓 Final Project

You have completed all 22 lessons and are ready to put your PowerShell skills together in the final course project.

### [Project 05 — IT Administration Capstone](../projects/project-05-it-administration-capstone.md)

Choose an Active Directory audit, Microsoft 365 audit, or IT administration toolkit and build a complete PowerShell solution.

Your final project should demonstrate your ability to plan, build, test, troubleshoot, document, and safely automate an IT task.

> **Challenge:** This project provides the least step-by-step guidance. Use everything you have learned throughout the course—including `Get-Command`, `Get-Help`, and `Get-Member`—to find your own solutions.

------------------------------------------------------------------------

## Additional Resources

-   [PowerShell Documentation --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/)
-   [About Scripts --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_scripts?view=powershell-7.6)
-   [About Functions Advanced --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_functions_advanced?view=powershell-7.6)
-   [About Functions Advanced Methods --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_functions_advanced_methods?view=powershell-7.6)
-   [About Try Catch Finally --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_try_catch_finally?view=powershell-7.6)
-   [About Comment Based Help --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_comment_based_help?view=powershell-7.6)
