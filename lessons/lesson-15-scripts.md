[lesson-15-scripts.md](https://github.com/user-attachments/files/31517416/lesson-15-scripts.md)

# Lesson 15 --- Scripts

## Learning Objectives

By the end of this lesson, you will be able to:

-   Explain what a PowerShell script is.
-   Create and run a `.ps1` script.
-   Organize a script into readable sections.
-   Add comments and comment-based help.
-   Accept script parameters with `param()`.
-   Combine variables, conditions, loops, functions, and pipelines in a
    script.
-   Understand execution policy at a beginner level.
-   Use `$PSScriptRoot` to work with files stored alongside a script.
-   Use `Write-Verbose` for optional diagnostic output.
-   Understand the importance of testing and safe change controls.
-   Build a small administrative script from beginning to end.

------------------------------------------------------------------------

## What Is a PowerShell Script?

A PowerShell script is a text file containing PowerShell commands.

PowerShell script files use the extension:

``` text
.ps1
```

For example:

``` text
inventory-report.ps1
```

Instead of entering commands one at a time in the console, you can save
them in a script and run the entire workflow when needed.

> **Key Idea:** A script combines PowerShell commands into a repeatable,
> documented automation workflow.

------------------------------------------------------------------------

# Your First Script

Create a file named:

``` text
hello.ps1
```

Add:

``` powershell
'Hello from a PowerShell script!'
```

Save the file.

From PowerShell, navigate to its directory:

``` powershell
Set-Location C:\Temp\PowerShellScripts
```

Then run:

``` powershell
.\hello.ps1
```

The:

``` text
.\
```

tells PowerShell to run the script from the current directory.

------------------------------------------------------------------------

# Why . Is Used

PowerShell does not normally search the current directory for commands
automatically.

If the script is in your current folder, use:

``` powershell
.\script.ps1
```

You can also run a script using its full path:

``` powershell
C:\Temp\PowerShellScripts\hello.ps1
```

------------------------------------------------------------------------

# Scripts Build on Everything You Have Learned

A script can contain:

``` text
Commands
Variables
Objects
Pipelines
Arrays
Operators
Loops
Conditions
Functions
File operations
Data import/export
```

For example:

``` powershell
$services = Get-Service

$runningServices = $services |
    Where-Object Status -eq 'Running'

$runningServices |
    Sort-Object Name |
    Select-Object Name, DisplayName, Status
```

Saving this in a `.ps1` file turns the workflow into a repeatable
script.

------------------------------------------------------------------------

# Comments

Comments explain what your code is doing.

A single-line comment begins with:

``` text
#
```

Example:

``` powershell
# Get all Windows services
$services = Get-Service
```

PowerShell ignores the comment when executing the script.

------------------------------------------------------------------------

# Block Comments

PowerShell also supports block comments:

``` powershell
<#
This is a multi-line comment.

It can contain several lines of notes.
#>
```

Use comments to explain:

-   Purpose
-   Important assumptions
-   Unusual logic
-   Safety concerns
-   Why a particular approach was chosen

Avoid comments that simply repeat obvious code.

------------------------------------------------------------------------

# Script Parameters

Scripts can accept parameters just like functions.

Place:

``` powershell
param()
```

near the beginning of the script.

Example:

``` powershell
param (
    [string]$ComputerName
)

"Computer selected: $ComputerName"
```

Save as:

``` text
show-computer.ps1
```

Run:

``` powershell
.\show-computer.ps1 -ComputerName 'PC-001'
```

------------------------------------------------------------------------

# Mandatory Script Parameters

You can require input:

``` powershell
param (
    [Parameter(Mandatory)]
    [string]$Path
)
```

Then the script can use:

``` powershell
$Path
```

throughout its workflow.

------------------------------------------------------------------------

# Default Parameter Values

Example:

``` powershell
param (
    [string]$OutputPath = 'C:\Temp\report.csv'
)
```

The caller can use the default:

``` powershell
.\report.ps1
```

or supply another value:

``` powershell
.\report.ps1 -OutputPath C:\Reports\services.csv
```

------------------------------------------------------------------------

# A Simple Script Structure

A beginner-friendly script can be organized like this:

``` powershell
#Requires -Version 7.0

<#
.SYNOPSIS
Creates a simple service report.
#>

param (
    [string]$OutputPath = 'C:\Temp\services.csv'
)

# Variables

# Functions

# Main script

# Export / final output
```

Not every script needs every section.

The goal is to make the script easy to follow.

------------------------------------------------------------------------

# Comment-Based Help

PowerShell supports structured help comments.

Example:

``` powershell
<#
.SYNOPSIS
Creates a report of running services.

.DESCRIPTION
Retrieves running Windows services, sorts them by name,
and exports selected properties to a CSV file.

.PARAMETER OutputPath
Specifies where the CSV report will be saved.

.EXAMPLE
.\service-report.ps1 -OutputPath C:\Temp\services.csv
#>
```

Good help makes scripts easier for other administrators---and your
future self---to use.

------------------------------------------------------------------------

# Script Functions

Functions can keep a script organized.

Example:

``` powershell
function Get-RunningService {
    Get-Service |
        Where-Object Status -eq 'Running' |
        Sort-Object Name
}
```

Later:

``` powershell
$services = Get-RunningService
```

This makes the main portion of the script easier to read.

------------------------------------------------------------------------

# Main Script Logic

A useful design is to separate reusable functions from the main
workflow.

Example:

``` powershell
function Get-RunningService {
    Get-Service |
        Where-Object Status -eq 'Running'
}

# Main script

$services = Get-RunningService

$services |
    Sort-Object Name |
    Select-Object Name, DisplayName, Status
```

The bottom section shows the overall workflow while the function
contains the reusable details.

------------------------------------------------------------------------

# \$PSScriptRoot

Scripts often need to access files stored in the same directory as the
script.

PowerShell provides:

``` powershell
$PSScriptRoot
```

This contains the directory where the running script is located.

For example:

``` powershell
$dataPath = Join-Path $PSScriptRoot 'assets.csv'
```

Now the script can find `assets.csv` relative to itself instead of
relying on the user's current working directory.

This is extremely useful for portable scripts.

------------------------------------------------------------------------

# Example Script Folder

You might organize a small tool like:

``` text
AssetReport\
│
├── asset-report.ps1
├── assets.csv
└── output\
```

Inside the script:

``` powershell
$inputPath = Join-Path $PSScriptRoot 'assets.csv'
$outputPath = Join-Path $PSScriptRoot 'output\active-assets.csv'
```

The script can now work regardless of where the user launches it from.

------------------------------------------------------------------------

# Execution Policy

PowerShell has an **execution policy** feature that can affect whether
scripts are allowed to run.

Check the policies in effect:

``` powershell
Get-ExecutionPolicy -List
```

Execution policy helps reduce accidental script execution, but it is
**not a security boundary**.

Do not change execution policy simply because a script will not run
without first understanding:

-   Which policy is blocking the script
-   Which scope the policy applies to
-   Whether organizational policy controls the setting
-   Whether the script is trusted

> **Important:** On managed company devices, follow your organization's
> security policies rather than bypassing script restrictions.

------------------------------------------------------------------------

# Script Signing and Trust

In professional environments, scripts may be digitally signed.

Signing can help verify:

-   Who published the script
-   Whether the script was changed after signing

You do not need to implement script signing in this beginner lesson, but
you should understand why organizations may require trusted or signed
scripts.

------------------------------------------------------------------------

# Write-Output

PowerShell can send objects to the success output stream with:

``` powershell
Write-Output
```

For example:

``` powershell
Write-Output 'Report complete.'
```

However, PowerShell also outputs expressions directly:

``` powershell
'Report complete.'
```

For reusable automation, think carefully about what should be **data
output** versus what is merely a status message.

------------------------------------------------------------------------

# Write-Verbose

For optional diagnostic information, use:

``` powershell
Write-Verbose
```

Example:

``` powershell
Write-Verbose "Importing data from $Path"
```

When a script or advanced function supports verbose output, the caller
can request it with:

``` powershell
-Verbose
```

This is preferable to filling normal output with debugging messages.

------------------------------------------------------------------------

# Write-Warning

Use:

``` powershell
Write-Warning
```

for warning messages.

Example:

``` powershell
Write-Warning 'No active assets were found.'
```

Warnings are visually and logically different from normal data output.

------------------------------------------------------------------------

# Safe Administrative Scripts

Scripts can make large numbers of changes very quickly.

Before automating a change:

``` text
1. Retrieve the targets
2. Verify the targets
3. Test on a small sample
4. Use -WhatIf where supported
5. Review output
6. Make the change
7. Verify the result
```

Automation increases both your speed and the potential impact of
mistakes.

------------------------------------------------------------------------

# Build Read-Only Scripts First

When learning, start with scripts that:

``` text
Collect
Inspect
Filter
Sort
Report
Export
```

before scripts that:

``` text
Delete
Disable
Modify
Move
Overwrite
```

Read-only reporting scripts are an excellent way to practice safely.

------------------------------------------------------------------------

# Practical Script --- Service Report

Example:

``` powershell
<#
.SYNOPSIS
Exports a report of running services.
#>

param (
    [string]$OutputPath = 'C:\Temp\running-services.csv'
)

$services = Get-Service |
    Where-Object Status -eq 'Running' |
    Sort-Object Name |
    Select-Object Name, DisplayName, Status

$services |
    Export-Csv -Path $OutputPath -NoTypeInformation

"Report created: $OutputPath"
```

Run:

``` powershell
.\service-report.ps1
```

or:

``` powershell
.\service-report.ps1 -OutputPath C:\Reports\services.csv
```

------------------------------------------------------------------------

# Practical Script --- Asset Report

The following example combines several lessons:

``` powershell
<#
.SYNOPSIS
Creates an active asset report from CSV data.
#>

param (
    [Parameter(Mandatory)]
    [string]$InputPath,

    [string]$OutputPath = (Join-Path $PSScriptRoot 'active-assets.csv')
)

if (-not (Test-Path $InputPath)) {
    Write-Warning "Input file not found: $InputPath"
    return
}

$assets = Import-Csv -Path $InputPath

$activeAssets = $assets |
    Where-Object Status -eq 'Active' |
    Sort-Object AssetTag |
    Select-Object AssetTag, DeviceType, AssignedTo, Status

$activeAssets |
    Export-Csv -Path $OutputPath -NoTypeInformation

"Exported $($activeAssets.Count) active assets to $OutputPath"
```

This script uses:

``` text
Parameters
Variables
Conditions
Objects
Pipelines
CSV data
Filtering
Sorting
Properties
Export
```

------------------------------------------------------------------------

# Test Scripts in Stages

Do not wait until a large script is finished before testing it.

A better workflow is:

``` text
Write a small section
      ↓
Run it
      ↓
Inspect the output
      ↓
Add the next section
      ↓
Run again
```

This is the same incremental approach you have used for pipelines and
functions.

------------------------------------------------------------------------

# Avoid Hard-Coding When Practical

Hard-coded values make scripts less reusable.

Instead of:

``` powershell
Import-Csv C:\Users\Alex\Desktop\assets.csv
```

consider:

``` powershell
param (
    [string]$InputPath
)

Import-Csv -Path $InputPath
```

Or use `$PSScriptRoot` for files shipped with the script.

Parameters make scripts easier to reuse in different environments.

------------------------------------------------------------------------

# Script Naming

Use descriptive filenames.

Good examples:

``` text
get-service-report.ps1
export-asset-inventory.ps1
find-large-files.ps1
test-device-list.ps1
```

Avoid vague names such as:

``` text
script1.ps1
test.ps1
new.ps1
```

A useful filename should give you a reasonable idea of the script's
purpose.

------------------------------------------------------------------------

# Version Control

PowerShell scripts are plain-text files, which makes them well suited
for version control systems such as Git.

Version control can help you:

-   Track changes
-   Restore earlier versions
-   Review modifications
-   Document why changes were made
-   Collaborate with other administrators

As scripts become more important, version control becomes increasingly
valuable.

------------------------------------------------------------------------

# From Commands to Automation

The progression of this course now looks like:

``` text
Commands
   ↓
Objects
   ↓
Pipeline
   ↓
Variables
   ↓
Collections
   ↓
Operators
   ↓
Data
   ↓
Loops
   ↓
Conditions
   ↓
Functions
   ↓
Scripts
   ↓
Repeatable Automation
```

A script is not a completely separate PowerShell concept.

It is a way of combining the concepts you have already learned into a
reusable workflow.

------------------------------------------------------------------------

# Key Takeaways

-   PowerShell scripts use the `.ps1` extension.
-   `.\script.ps1` runs a script from the current directory.
-   Scripts combine the PowerShell concepts learned throughout the
    course.
-   Comments document code and intent.
-   Comment-based help documents how a script should be used.
-   `param()` allows scripts to accept input.
-   Functions help organize and reuse script logic.
-   `$PSScriptRoot` identifies the script's directory.
-   Execution policy can affect script execution but is not a security
    boundary.
-   Follow organizational policy on managed systems rather than
    bypassing restrictions.
-   `Write-Verbose` provides optional diagnostic information.
-   Administrative scripts should be tested carefully before making
    changes.
-   Start with read-only/reporting automation before destructive
    automation.
-   Avoid hard-coded values when parameters or relative paths are more
    appropriate.
-   Git and other version-control systems are useful for maintaining
    scripts.
-   Build scripts incrementally and test each stage.

------------------------------------------------------------------------

# Lab

Continue to:

[Lab 15 — Scripts](../labs/lesson-15-lab-15-scripts.md)


In the lab, you will create and run `.ps1` files, add parameters and
comments, organize functions and main script logic, use `$PSScriptRoot`,
build a report from structured data, and combine the concepts from
Lessons 01--15 into a small administrative script.

------------------------------------------------------------------------

# 🚀 Project Checkpoint

You have completed Lessons 11–15. It's time to combine data, loops, conditions, functions, and scripts into a practical IT project.

### [Project 03 — IT Asset Audit Tool](../projects/project-03-it-asset-audit.md)

Build a reusable PowerShell script that imports asset data, identifies important inventory conditions, and generates useful IT reports.

> **Tip:** Think about how functions can break a larger problem into smaller, reusable pieces.


------------------------------------------------------------------------

## Additional Resources

-   [About Scripts --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_scripts?view=powershell-7.6)
-   [About Running Scripts --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_scripts?view=powershell-7.6)
-   [About Execution Policies --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.6)
-   [About Comment Based Help --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_comment_based_help?view=powershell-7.6)
-   [About Automatic Variables --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_automatic_variables?view=powershell-7.6)
-   [About Requires --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_requires?view=powershell-7.6)
