# Lab 17 --- Modules

## Lab Objective

In this lab, you will learn how reusable PowerShell tools are organized
into modules.

You will:

-   Identify loaded and available modules.
-   Discover which module provides a command.
-   Import a module.
-   Inspect module commands.
-   Review `$env:PSModulePath`.
-   Create a simple `.psm1` script module.
-   Export selected functions.
-   Import and test your own IT tools module.

------------------------------------------------------------------------

## Exercise 1 --- Find a Command's Module

Run:

``` powershell
Get-Command Get-Service |
    Select-Object Name, CommandType, Source
```

Record the source:

``` text
________________________________________
```

Now inspect two other commands you already know.

``` powershell
# Your commands:
```

------------------------------------------------------------------------

## Exercise 2 --- Loaded vs. Available Modules

Run:

``` powershell
Get-Module
```

Then:

``` powershell
Get-Module -ListAvailable |
    Sort-Object Name |
    Select-Object Name, Version, Path
```

Explain the difference:

``` text
Get-Module:
____________________________________________________

Get-Module -ListAvailable:
____________________________________________________
```

------------------------------------------------------------------------

## Exercise 3 --- Import a Module

Run:

``` powershell
Import-Module Microsoft.PowerShell.Management
```

Verify:

``` powershell
Get-Module Microsoft.PowerShell.Management
```

Then discover its commands:

``` powershell
Get-Command -Module Microsoft.PowerShell.Management
```

Choose three useful commands:

``` text
1. ______________________
2. ______________________
3. ______________________
```

------------------------------------------------------------------------

## Exercise 4 --- PSModulePath

Display:

``` powershell
$env:PSModulePath
```

Split it into individual paths:

``` powershell
$env:PSModulePath -split [IO.Path]::PathSeparator
```

Why does PowerShell need module search paths?

``` text
____________________________________________________
```

------------------------------------------------------------------------

## Exercise 5 --- Find Installed Module Tools

Check whether these commands exist:

``` powershell
Get-Command Find-Module -ErrorAction SilentlyContinue
Get-Command Get-InstalledModule -ErrorAction SilentlyContinue
```

If available, inspect their Help.

``` powershell
Get-Help Find-Module -Examples
```

> Do not install random modules just to complete this lab. Module
> source, publisher, repository, and trust should be reviewed first.

------------------------------------------------------------------------

# Exercise 6 --- Create Your Module Folder

Create a safe module workspace:

``` powershell
$labRoot = Join-Path $HOME 'PowerShell-Lab17'
$moduleRoot = Join-Path $labRoot 'ITTools'

New-Item -Path $moduleRoot -ItemType Directory -Force
```

Your module will be:

``` text
ITTools
└── ITTools.psm1
```

------------------------------------------------------------------------

# Exercise 7 --- Add a Function

Create:

``` text
ITTools.psm1
```

Add:

``` powershell
function Get-ServiceSummary {
    [CmdletBinding()]
    param (
        [ValidateSet('Running', 'Stopped')]
        [string]$Status = 'Running'
    )

    Get-Service |
        Where-Object Status -eq $Status |
        Select-Object Name, DisplayName, Status
}
```

Import the module by path:

``` powershell
Import-Module (Join-Path $moduleRoot 'ITTools.psm1') -Force
```

Test:

``` powershell
Get-ServiceSummary
```

------------------------------------------------------------------------

# Exercise 8 --- Add More Reusable Tools

Add at least two more functions from earlier labs.

Good candidates:

``` text
Get-FileReport
Test-AssetAssignment
```

After editing the module:

``` powershell
Import-Module (Join-Path $moduleRoot 'ITTools.psm1') -Force
```

Discover your functions:

``` powershell
Get-Command -Module ITTools
```

------------------------------------------------------------------------

# Exercise 9 --- Export Selected Functions

At the bottom of the module, add:

``` powershell
Export-ModuleMember -Function `
    Get-ServiceSummary,
    Get-FileReport,
    Test-AssetAssignment
```

Create one small helper function but do **not** export it.

Reimport the module and inspect:

``` powershell
Get-Command -Module ITTools
```

Did the private helper appear?

``` text
Yes / No
```

------------------------------------------------------------------------

# Exercise 10 --- Remove and Reimport

Run:

``` powershell
Remove-Module ITTools
```

Check:

``` powershell
Get-Module ITTools
```

Then reimport it.

This helps demonstrate that module functions belong to the loaded module
rather than permanently becoming built-in commands.

------------------------------------------------------------------------

# End-of-Lab Project --- Build ITTools

Create a reusable `ITTools` module containing at least three functions
from previous labs.

Requirements:

``` text
[ ] ITTools.psm1
[ ] At least 3 useful functions
[ ] Approved Verb-Noun names
[ ] Parameters where appropriate
[ ] Error handling where appropriate
[ ] Export-ModuleMember
[ ] Import succeeds
[ ] Get-Command -Module ITTools shows intended public functions
[ ] Each exported function is tested
```

### Bonus

Research `New-ModuleManifest` using PowerShell Help and create:

``` text
ITTools.psd1
```

Do not worry about filling every manifest field. Focus on understanding
what a manifest is for.

------------------------------------------------------------------------

# Knowledge Check

1.  What extension identifies a script module?

    A. `.ps1` B. `.psm1` C. `.csv` D. `.psx`

2.  Which command lists modules currently loaded?

    A. `Get-Module` B. `Find-Module` C. `Get-Command` D. `Show-Module`

3.  What does `Get-Module -ListAvailable` do?

    A. Lists only loaded modules B. Finds modules available in module
    locations C. Installs modules D. Updates modules

4.  Why review module trust before installation?

    A. Modules can contain executable PowerShell code B. Modules cannot
    be removed C. All modules require payment D. Modules disable Help

------------------------------------------------------------------------

# Lab Complete

Continue to:

[Lesson 18 — Remoting](../lessons/lesson-18-remoting.md)

