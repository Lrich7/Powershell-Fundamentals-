# Lab 22 --- Automation Projects Capstone

## Capstone Objective

This is the final lab of the PowerShell Fundamentals course.

Unlike earlier labs, this is **not** a step-by-step worksheet.

You will plan, build, test, document, and review a complete PowerShell
automation project.

Your project should demonstrate that you can combine the skills from the
course rather than simply copy individual cmdlets.

------------------------------------------------------------------------

# What You Are Demonstrating

Your capstone should use several of these skills appropriately:

``` text
Command discovery
PowerShell Help
Objects
Pipelines
Filtering
Sorting
Variables
Data types
Arrays / collections
Operators
Files
CSV / JSON
Loops
Conditions
Functions
Scripts
Error handling
Modules
Remoting
Windows administration
Active Directory
Microsoft 365
```

You do **not** need to force every topic into one script.

Choose the tools that make sense for the problem.

> **Key Idea:** Good automation is repeatable, understandable, testable,
> and safe.

------------------------------------------------------------------------

# Phase 1 --- Choose a Project

Choose **one** of the projects below, or design a comparable project of
your own.

------------------------------------------------------------------------

## Project A --- IT Asset Audit Tool

### Scenario

IT maintains a hardware inventory and needs regular reports showing:

``` text
Active assets
Available assets
Unassigned assets
Retired assets
Counts by device type
```

### Possible Input

``` text
assets.csv
```

### Possible Outputs

``` text
active-assets.csv
available-assets.csv
unassigned-assets.csv
retired-assets.csv
asset-summary.csv
automation.log
```

### Skills

``` text
Import-Csv
Objects
Filtering
Sorting
Functions
Conditions
Group-Object
Export-Csv
Logging
Error handling
```

------------------------------------------------------------------------

## Project B --- Windows Health Reporter

### Scenario

Help desk staff need a repeatable tool that collects useful
troubleshooting information from a Windows computer.

### Possible Report Data

``` text
Computer name
Manufacturer
Model
Serial number
Windows version
Last boot
Memory
Disk free space
Top processes
Service status
Network configuration
Recent event log errors
```

### Skills

``` text
CIM
Processes
Services
Networking
Event logs
Functions
Custom objects
Error handling
CSV / JSON
```

------------------------------------------------------------------------

## Project C --- Large File Analyzer

### Scenario

IT needs a safe reporting tool to identify large files that may deserve
cleanup review.

### Requirements

The tool must **not delete anything**.

It should:

``` text
Accept a path
Accept a size threshold
Search recursively
Find large files
Convert bytes to MB/GB
Sort largest to smallest
Export a report
Summarize total findings
```

### Skills

``` text
Files and folders
Parameters
Where-Object
Sort-Object
Calculated properties
Functions
Error handling
CSV
```

------------------------------------------------------------------------

## Project D --- Active Directory Audit

Use only an authorized AD DS lab or production environment where
read-only queries are approved.

### Possible Reports

``` text
Enabled users
Disabled users
Users missing department
Computer inventory
Disabled computers
Group inventory
Authorized group membership review
```

### Skills

``` text
ActiveDirectory module
Get-ADUser
Get-ADComputer
Get-ADGroup
Filtering
SearchBase
Functions
Error handling
CSV
```

The project should remain read-only unless specific modification
exercises are separately authorized.

------------------------------------------------------------------------

## Project E --- Microsoft 365 Audit Pack

Use only an authorized Microsoft 365 tenant.

### Possible Reports

``` text
Users
Groups
Shared mailboxes
Mailbox inventory
License SKUs
```

### Skills

``` text
Microsoft Graph
Exchange Online
Authentication context
Least privilege
Modules
Functions
Error handling
CSV
```

Keep the capstone read-only unless a separate approved change
requirement exists.

------------------------------------------------------------------------

## Project F --- Multi-Computer Health Check

Use only an authorized remoting lab.

### Possible Report Data

``` text
Computer
OS
Last boot
Disk space
Selected services
PowerShell version
Checked time
```

### Skills

``` text
Remoting
Invoke-Command
Collections
Functions
Error handling
Custom objects
CSV
```

------------------------------------------------------------------------

## Project G --- New Employee Preparation Validator

### Scenario

IT receives onboarding requests, but submitted information is sometimes
incomplete.

Instead of creating accounts automatically, build a validation tool.

### Possible Input Fields

``` text
FirstName
LastName
Department
Manager
JobTitle
StartDate
DeviceType
RequestedAccess
```

### Tool Behavior

``` text
Validate required fields
Identify missing values
Normalize names
Generate a proposed username
Flag incomplete requests
Create a preparation report
```

This is a strong automation project because it improves a real process
without making high-risk account changes.

------------------------------------------------------------------------

## Project H --- Offboarding Checklist Generator

### Scenario

IT needs a consistent checklist for employee offboarding.

### Input

``` text
User
Manager
Termination date
Standard / emergency
```

### Output

A structured checklist covering items such as:

``` text
Account review
Session revocation
License review
Mailbox actions
OneDrive actions
Hardware return
Asset recovery
Documentation
```

The first version should **generate the checklist only** rather than
automatically disabling accounts.

------------------------------------------------------------------------

# Phase 2 --- Write the Project Plan

Before writing code, create:

``` text
PROJECT-PLAN.md
```

Include:

## Goal

What problem does the automation solve?

``` text
____________________________________________________
```

## Inputs

What information does it need?

``` text
____________________________________________________
```

## Outputs

What should it produce?

``` text
____________________________________________________
```

## Dependencies

Examples:

``` text
PowerShell version
Modules
Permissions
Input files
Network access
Tenant access
AD access
```

``` text
____________________________________________________
```

## Risk Level

Choose:

``` text
Low
Medium
High
```

Explain:

``` text
____________________________________________________
```

## Safety Controls

Examples:

``` text
Read-only design
Input validation
-WhatIf
Preview report
Small test scope
Error handling
No secrets in code
```

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Phase 3 --- Design the Workflow

Create a simple flow before coding.

Example:

``` text
Input
  ↓
Validate
  ↓
Collect
  ↓
Inspect
  ↓
Filter
  ↓
Transform
  ↓
Summarize
  ↓
Export
  ↓
Log
  ↓
Verify
```

Write your project's actual flow:

``` text
________________________________________
↓
________________________________________
↓
________________________________________
↓
________________________________________
↓
________________________________________
```

------------------------------------------------------------------------

# Phase 4 --- Create the Project Structure

A suggested layout:

``` text
MyPowerShellProject\
│
├── README.md
├── PROJECT-PLAN.md
├── project.ps1
├── data\
├── output\
├── logs\
└── samples\
```

Not every project needs every folder.

Use a structure that keeps the project understandable.

------------------------------------------------------------------------

# Phase 5 --- Build Parameters

Your script must use at least one meaningful parameter.

Examples:

``` powershell
param (
    [Parameter(Mandatory)]
    [string]$InputPath,

    [string]$OutputFolder = (Join-Path $PSScriptRoot 'output')
)
```

Avoid unnecessary hard-coded values.

------------------------------------------------------------------------

# Phase 6 --- Validate Inputs

Your script should validate important prerequisites.

Examples:

``` powershell
Test-Path
Get-Module -ListAvailable
ValidateSet
ValidateRange
$null checks
IsNullOrWhiteSpace
```

Ask:

``` text
What must be true before this automation can safely continue?
```

Implement those checks.

------------------------------------------------------------------------

# Phase 7 --- Use Functions

Your capstone must contain at least **two functions**.

Good function boundaries might be:

``` text
Import-ProjectData
Test-ProjectInput
Get-ReportData
Export-ProjectReport
Write-Log
Get-SystemSummary
```

Functions should have clear purposes.

Avoid creating a function merely to satisfy the requirement.

------------------------------------------------------------------------

# Phase 8 --- Use Structured Objects

At least part of your script should return or construct useful
PowerShell objects.

Example:

``` powershell
[PSCustomObject]@{
    ComputerName = $env:COMPUTERNAME
    CheckedAt    = Get-Date
    Status       = 'Healthy'
}
```

Prefer objects over manually formatted strings when the data may be
filtered, sorted, exported, or reused.

------------------------------------------------------------------------

# Phase 9 --- Add Error Handling

Identify at least one operation that can realistically fail.

Examples:

``` text
CSV import
File access
Module import
Remote connection
Graph connection
AD query
Export
```

Handle it meaningfully with:

``` powershell
try
catch
finally
-ErrorAction Stop
throw
Write-Warning
```

Do not use empty `catch` blocks.

------------------------------------------------------------------------

# Phase 10 --- Add Logging

Your project should create a basic log.

Example:

``` powershell
function Write-Log {
    param (
        [Parameter(Mandatory)]
        [string]$Message,

        [string]$Path = (Join-Path $PSScriptRoot 'logs\project.log')
    )

    $line = '{0} - {1}' -f `
        (Get-Date -Format 'yyyy-MM-dd HH:mm:ss'),
        $Message

    Add-Content -Path $Path -Value $line
}
```

Useful events:

``` text
Script started
Input source
Records processed
Warnings
Errors
Output created
Script completed
```

Do not log credentials or secrets.

------------------------------------------------------------------------

# Phase 11 --- Add Help

Your script must include comment-based help containing:

``` text
.SYNOPSIS
.DESCRIPTION
.PARAMETER
.EXAMPLE
```

Test:

``` powershell
Get-Help .\project.ps1
```

------------------------------------------------------------------------

# Phase 12 --- Test Incrementally

Do not run the completed project for the first time against the largest
possible target.

Test:

``` text
Single sample
   ↓
Small data set
   ↓
Expected failure
   ↓
Expected success
   ↓
Larger safe scope
```

Record at least three tests:

  Test   Expected Result   Actual Result   Pass?
  ------ ----------------- --------------- -------
                                           
                                           
                                           

------------------------------------------------------------------------

# Phase 13 --- Create README.md

Your README should explain:

``` text
Project name
Purpose
Requirements
Modules
Permissions
Inputs
Outputs
How to run
Examples
Known limitations
Safety notes
```

A person who did not write the script should be able to understand how
to use it.

------------------------------------------------------------------------

# Required Capstone Features

Your final project must include:

``` text
[ ] .ps1 script
[ ] Clear goal
[ ] At least 1 parameter
[ ] At least 2 functions
[ ] Variables
[ ] PowerShell objects
[ ] Pipeline usage
[ ] Filtering or sorting
[ ] At least 1 condition
[ ] Collection processing where appropriate
[ ] Input validation
[ ] Error handling
[ ] Structured output such as CSV or JSON
[ ] Basic logging
[ ] Comment-based help
[ ] README.md
[ ] PROJECT-PLAN.md
[ ] Test results
```

------------------------------------------------------------------------

# Safety Requirements

Your capstone must also satisfy:

``` text
[ ] No plain-text passwords
[ ] No secrets or access tokens in source files
[ ] No destructive production testing
[ ] Targets are verified
[ ] Scope is limited appropriately
[ ] Read-only version built first where practical
[ ] Change operations use -WhatIf / ShouldProcess where appropriate
[ ] Output is verified
```

------------------------------------------------------------------------

# Optional Advanced Features

These are optional, not required:

``` text
[ ] Package functions into a module
[ ] SupportsShouldProcess
[ ] -WhatIf / -Confirm
[ ] Remote computer support
[ ] Active Directory integration
[ ] Microsoft Graph integration
[ ] Exchange Online reporting
[ ] JSON configuration file
[ ] HTML report
[ ] Separate log levels
[ ] Unit testing / Pester exploration
```

------------------------------------------------------------------------

# Final Demonstration

Be prepared to explain:

1.  What problem does your script solve?
2.  What are its inputs?
3.  What are its outputs?
4.  Which parts could fail?
5.  How does it handle those failures?
6.  What safety controls did you add?
7.  Why did you choose your functions?
8.  What would you improve in version 2?

------------------------------------------------------------------------

# Final Reflection

Answer in your project documentation:

## Most useful PowerShell concept

``` text
____________________________________________________
```

## Hardest concept

``` text
____________________________________________________
```

## One task you could automate in a real IT environment

``` text
____________________________________________________
```

## One thing you would verify before automating a production change

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Capstone Complete

You have reached the point where PowerShell is no longer just a
collection of commands.

The core workflow is now:

``` text
Discover
   ↓
Learn
   ↓
Inspect
   ↓
Collect
   ↓
Filter / Sort
   ↓
Store / Transform
   ↓
Decide / Loop
   ↓
Build Functions
   ↓
Handle Errors
   ↓
Automate
   ↓
Verify
   ↓
Document
```

The goal of the course was never to memorize every PowerShell command.

It was to learn how to **discover, understand, combine, test, and safely
automate PowerShell tasks**.
