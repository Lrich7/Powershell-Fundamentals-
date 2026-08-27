[lesson-14-functions.md](https://github.com/user-attachments/files/31517408/lesson-14-functions.md)

# Lesson 14 --- Functions

## Learning Objectives

By the end of this lesson, you will be able to:

-   Explain why functions are useful.
-   Create and call basic PowerShell functions.
-   Pass information into functions with parameters.
-   Use multiple parameters.
-   Define parameter data types.
-   Set default parameter values.
-   Use `return` appropriately.
-   Understand PowerShell's output behavior.
-   Create basic advanced functions with `[CmdletBinding()]`.
-   Add parameter validation.
-   Write reusable functions for common IT tasks.

------------------------------------------------------------------------

## What Is a Function?

A **function** is a named block of reusable PowerShell code.

Instead of repeating the same commands throughout a script, you can
place them inside a function and call that function whenever needed.

Basic syntax:

``` powershell
function Get-Greeting {
    'Hello from PowerShell!'
}
```

Run it:

``` powershell
Get-Greeting
```

> **Key Idea:** Functions turn repeated PowerShell logic into reusable
> commands.

------------------------------------------------------------------------

# PowerShell Function Naming

PowerShell commands normally follow:

``` text
Verb-Noun
```

Your functions should follow the same pattern.

Examples:

``` text
Get-ComputerStatus
Test-AssetAssignment
New-Report
Show-ServiceStatus
```

When possible, use an approved PowerShell verb.

You can view approved verbs with:

``` powershell
Get-Verb
```

This makes your functions feel consistent with built-in PowerShell
commands.

------------------------------------------------------------------------

# Creating a Basic Function

Example:

``` powershell
function Show-WelcomeMessage {
    'Welcome to PowerShell training.'
}
```

Call it:

``` powershell
Show-WelcomeMessage
```

The function remains available in the current PowerShell session after
it is defined.

------------------------------------------------------------------------

# Functions with Variables

Functions can contain normal PowerShell commands and variables.

``` powershell
function Show-CurrentTime {
    $time = Get-Date
    "Current time: $time"
}
```

Run:

``` powershell
Show-CurrentTime
```

------------------------------------------------------------------------

# Parameters

Parameters allow callers to provide information to a function.

Example:

``` powershell
function Get-Greeting {
    param (
        $Name
    )

    "Hello, $Name"
}
```

Call it:

``` powershell
Get-Greeting -Name 'Alex'
```

The value `'Alex'` is assigned to `$Name` inside the function.

------------------------------------------------------------------------

# Multiple Parameters

Functions can accept multiple parameters.

``` powershell
function Show-Asset {
    param (
        $AssetTag,
        $AssignedTo
    )

    "Asset $AssetTag is assigned to $AssignedTo."
}
```

Call it:

``` powershell
Show-Asset -AssetTag 'LT-1001' -AssignedTo 'Alex'
```

Named parameters make the command easier to understand.

------------------------------------------------------------------------

# Parameter Types

You can specify the type a parameter should accept.

``` powershell
function Add-Numbers {
    param (
        [int]$FirstNumber,
        [int]$SecondNumber
    )

    $FirstNumber + $SecondNumber
}
```

Call:

``` powershell
Add-Numbers -FirstNumber 10 -SecondNumber 5
```

Type declarations can make function behavior more predictable.

------------------------------------------------------------------------

# Default Parameter Values

Parameters can have defaults.

``` powershell
function Get-Greeting {
    param (
        [string]$Name = 'User'
    )

    "Hello, $Name"
}
```

Calling:

``` powershell
Get-Greeting
```

returns a greeting using `User`.

Calling:

``` powershell
Get-Greeting -Name 'Alex'
```

uses the supplied value instead.

------------------------------------------------------------------------

# Mandatory Parameters

PowerShell can require a parameter.

A common approach uses:

``` powershell
[Parameter(Mandatory)]
```

Example:

``` powershell
function Show-Computer {
    param (
        [Parameter(Mandatory)]
        [string]$ComputerName
    )

    "Computer: $ComputerName"
}
```

If you call the function without the required parameter, PowerShell
prompts for it.

------------------------------------------------------------------------

# CmdletBinding

You can make a function behave more like a PowerShell cmdlet by adding:

``` powershell
[CmdletBinding()]
```

Example:

``` powershell
function Get-ComputerInfo {
    [CmdletBinding()]
    param (
        [Parameter(Mandatory)]
        [string]$ComputerName
    )

    [PSCustomObject]@{
        ComputerName = $ComputerName
        CheckedAt    = Get-Date
    }
}
```

Functions using `[CmdletBinding()]` are commonly called **advanced
functions**.

This is the foundation for creating professional reusable PowerShell
tools.

------------------------------------------------------------------------

# Function Output

PowerShell automatically sends uncaptured output to the success output
stream.

For example:

``` powershell
function Get-Numbers {
    1
    2
    3
}
```

Calling:

``` powershell
Get-Numbers
```

returns all three values.

You do not need to use `return` simply to produce normal function
output.

------------------------------------------------------------------------

# Using return

`return` can output a value and exit the current function scope.

Example:

``` powershell
function Test-Number {
    param (
        [int]$Number
    )

    if ($Number -lt 0) {
        return 'Negative numbers are not allowed.'
    }

    "Number accepted: $Number"
}
```

If the number is negative, the function exits early.

> **Important:** PowerShell functions can output anything written to the
> success stream, not only values following `return`.

------------------------------------------------------------------------

# Returning Objects

Functions are especially useful when they return structured objects.

``` powershell
function Get-AssetRecord {
    param (
        [string]$AssetTag,
        [string]$AssignedTo,
        [string]$Status
    )

    [PSCustomObject]@{
        AssetTag   = $AssetTag
        AssignedTo = $AssignedTo
        Status     = $Status
    }
}
```

Use it:

``` powershell
$asset = Get-AssetRecord `
    -AssetTag 'LT-1001' `
    -AssignedTo 'Alex' `
    -Status 'Active'
```

Then:

``` powershell
$asset.AssetTag
```

Functions that return objects work naturally with the pipeline.

------------------------------------------------------------------------

# Parameter Validation

PowerShell can validate parameter values.

Example:

``` powershell
function Set-AssetStatus {
    param (
        [Parameter(Mandatory)]
        [ValidateSet('Active', 'Available', 'Retired')]
        [string]$Status
    )

    "Status selected: $Status"
}
```

PowerShell rejects values outside the allowed set.

------------------------------------------------------------------------

# ValidateRange

For numeric values, you can use:

``` powershell
[ValidateRange(1, 100)]
```

Example:

``` powershell
function Test-Percentage {
    param (
        [ValidateRange(0, 100)]
        [int]$Percent
    )

    "Percent: $Percent"
}
```

------------------------------------------------------------------------

# Functions Can Call Other Commands

A function can contain the commands you have already learned.

Example:

``` powershell
function Get-RunningService {
    Get-Service |
        Where-Object Status -eq 'Running' |
        Sort-Object Name
}
```

Call:

``` powershell
Get-RunningService
```

You have converted a multi-stage pipeline into a reusable command.

------------------------------------------------------------------------

# Functions Can Accept Paths

Example:

``` powershell
function Get-LargeFile {
    param (
        [Parameter(Mandatory)]
        [string]$Path,

        [long]$MinimumSize = 10MB
    )

    Get-ChildItem -Path $Path -File |
        Where-Object Length -gt $MinimumSize |
        Sort-Object Length -Descending
}
```

Call:

``` powershell
Get-LargeFile -Path C:\Temp
```

Or:

``` powershell
Get-LargeFile -Path C:\Temp -MinimumSize 50MB
```

------------------------------------------------------------------------

# Functions and the Pipeline

A well-designed function can return objects that users can continue
processing.

For example:

``` powershell
Get-LargeFile -Path C:\Temp |
    Select-Object Name, Length
```

Or:

``` powershell
Get-LargeFile -Path C:\Temp |
    Export-Csv C:\Temp\large-files.csv -NoTypeInformation
```

This is one reason returning objects is usually better than creating
preformatted text.

------------------------------------------------------------------------

# Avoid Formatting Inside Reusable Functions

A reusable function should usually return objects instead of ending
with:

``` powershell
Format-Table
```

For example, prefer:

``` powershell
function Get-RunningService {
    Get-Service |
        Where-Object Status -eq 'Running' |
        Select-Object Name, DisplayName, Status
}
```

Then the caller decides:

``` powershell
Get-RunningService | Format-Table
```

or:

``` powershell
Get-RunningService | Export-Csv .\services.csv -NoTypeInformation
```

Remember:

``` text
Reusable function → Return useful objects
Caller → Decide how to display/export them
```

------------------------------------------------------------------------

# Function Scope

Variables created inside a function normally belong to the function's
local scope.

Example:

``` powershell
function Test-Scope {
    $message = 'Inside the function'
    $message
}
```

After the function finishes, `$message` is not automatically a normal
variable in the calling scope.

You will encounter PowerShell scope in more detail as your scripts
become more advanced.

------------------------------------------------------------------------

# Practical IT Function

Here is a simple reusable function:

``` powershell
function Get-AssetSummary {
    [CmdletBinding()]
    param (
        [Parameter(Mandatory)]
        [string]$Path
    )

    $assets = Import-Csv -Path $Path

    $assets |
        Group-Object Status |
        Select-Object Name, Count
}
```

Call:

``` powershell
Get-AssetSummary -Path C:\Temp\assets.csv
```

This wraps a useful reporting workflow in a reusable command.

------------------------------------------------------------------------

# Build Functions Gradually

A good workflow is:

``` text
Write working commands
       ↓
Test the commands
       ↓
Identify repeated logic
       ↓
Wrap it in a function
       ↓
Add parameters
       ↓
Return useful objects
       ↓
Add validation
```

Do not begin by trying to build a complicated function all at once.

------------------------------------------------------------------------

# Key Takeaways

-   Functions create reusable PowerShell commands.
-   Use the `Verb-Noun` naming convention.
-   `Get-Verb` displays approved PowerShell verbs.
-   `param()` defines function parameters.
-   Parameters can have types and default values.
-   `[Parameter(Mandatory)]` can require input.
-   `[CmdletBinding()]` creates the foundation of an advanced function.
-   Validation attributes can restrict parameter input.
-   PowerShell automatically outputs uncaptured success-stream values.
-   `return` can output a value and exit a function early.
-   Functions can return complete PowerShell objects.
-   Reusable functions should generally avoid formatting their output.
-   Functions can combine commands, pipelines, loops, and conditions
    into reusable tools.
-   Build and test the underlying commands before wrapping them in a
    function.

------------------------------------------------------------------------

# Lab

Continue to:

[Lab 14 — Functions](../labs/lesson-14-lab-14-functions.md)

In the lab, you will create basic functions, add parameters and types,
use mandatory parameters and validation, return custom objects, and turn
existing PowerShell command sequences into reusable functions.

------------------------------------------------------------------------

## Additional Resources

-   [About Functions --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_functions?view=powershell-7.6)
-   [About Functions Advanced --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_functions_advanced?view=powershell-7.6)
-   [About Functions Advanced Parameters --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_functions_advanced_parameters?view=powershell-7.6)
-   [Get-Verb --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/get-verb?view=powershell-7.6)
