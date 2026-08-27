# Lab 14 --- Functions

## Lab Objective

In this lab, you will turn repeated PowerShell logic into reusable
commands.

You will:

-   Create and call functions.
-   Follow `Verb-Noun` naming.
-   Add parameters.
-   Add parameter types and defaults.
-   Require parameters.
-   Understand PowerShell function output.
-   Use `[CmdletBinding()]`.
-   Add basic parameter validation.
-   Build reusable IT helper functions.

------------------------------------------------------------------------

## Before You Begin

Complete Lessons 01--14.

------------------------------------------------------------------------

# Exercise 1 --- Basic Function

Create:

``` powershell
function Show-WelcomeMessage {
    'Welcome to PowerShell training.'
}
```

Run:

``` powershell
Show-WelcomeMessage
```

Run it again.

### Question

What problem do functions solve?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 2 --- Verb-Noun Naming

Run:

``` powershell
Get-Verb
```

Find three approved verbs that would make sense for functions:

``` text
1. ______________________
2. ______________________
3. ______________________
```

Why should custom functions follow PowerShell naming conventions?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 3 --- Parameter

Create:

``` powershell
function Get-Greeting {
    param (
        $Name
    )

    "Hello, $Name"
}
```

Call:

``` powershell
Get-Greeting -Name 'Alex'
```

Then call it with another name.

------------------------------------------------------------------------

# Exercise 4 --- Multiple Parameters

Create:

``` powershell
function Show-Asset {
    param (
        $AssetTag,
        $AssignedTo
    )

    "Asset $AssetTag is assigned to $AssignedTo."
}
```

Call it using named parameters.

``` powershell
# Your command:
```

------------------------------------------------------------------------

# Exercise 5 --- Parameter Types

Create:

``` powershell
function Add-Numbers {
    param (
        [int]$FirstNumber,
        [int]$SecondNumber
    )

    $FirstNumber + $SecondNumber
}
```

Test:

``` powershell
Add-Numbers -FirstNumber 10 -SecondNumber 5
```

Try a numeric string.

Then try an obviously nonnumeric string.

### Question

Why can typed parameters be useful?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 6 --- Default Values

Create:

``` powershell
function Get-Greeting {
    param (
        [string]$Name = 'User'
    )

    "Hello, $Name"
}
```

Run:

``` powershell
Get-Greeting
```

Then:

``` powershell
Get-Greeting -Name 'Jordan'
```

------------------------------------------------------------------------

# Exercise 7 --- Mandatory Parameter

Create:

``` powershell
function Show-Computer {
    param (
        [Parameter(Mandatory)]
        [string]$ComputerName
    )

    "Computer: $ComputerName"
}
```

Call it with the parameter.

Then call it without the parameter and observe PowerShell's behavior.

------------------------------------------------------------------------

# Exercise 8 --- Function Output

Create:

``` powershell
function Get-ExampleOutput {
    'First'
    'Second'
    'Third'
}
```

Store:

``` powershell
$result = Get-ExampleOutput
```

Display:

``` powershell
$result
$result.Count
```

### Question

What happens to uncaptured output produced inside a PowerShell function?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 9 --- CmdletBinding

Create:

``` powershell
function Get-ServiceSummary {
    [CmdletBinding()]
    param (
        [string]$Status = 'Running'
    )

    Get-Service |
        Where-Object Status -eq $Status |
        Select-Object Name, DisplayName, Status
}
```

Run:

``` powershell
Get-ServiceSummary
```

Then:

``` powershell
Get-ServiceSummary -Status Stopped
```

------------------------------------------------------------------------

# Exercise 10 --- ValidateSet

Improve the parameter:

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

Try:

``` powershell
Get-ServiceSummary -Status Invalid
```

### Question

What benefit does validation provide?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 11 --- Build a Test Function

Create:

``` powershell
function Test-AssetAssignment {
    param (
        [string]$AssignedTo
    )

    if ([string]::IsNullOrWhiteSpace($AssignedTo)) {
        $false
    }
    else {
        $true
    }
}
```

Test:

``` powershell
Test-AssetAssignment -AssignedTo 'Alex'
Test-AssetAssignment -AssignedTo ''
```

------------------------------------------------------------------------

# End-of-Lab Challenge --- Build an IT Helper Function

Build:

``` text
Get-FileReport
```

Requirements:

-   Accept a mandatory `Path`.
-   Accept an optional integer `Top` with a default of `10`.
-   Verify that the path exists.
-   Retrieve files recursively.
-   Sort largest to smallest.
-   Return the first `$Top` files.
-   Output:
    -   `Name`
    -   `FullName`
    -   `Length`
    -   `LastWriteTime`

Do not delete or modify files.

Starter only:

``` powershell
function Get-FileReport {
    [CmdletBinding()]
    param (
        # Define parameters
    )

    # Your code
}
```

Test it against a safe folder.

### Bonus

Add `[ValidateRange()]` to prevent invalid values for `Top`.

------------------------------------------------------------------------

# Knowledge Check

1.  What naming convention should PowerShell functions generally follow?

    A. Noun-Verb\
    B. Verb-Noun\
    C. Function-Name\
    D. Action-Command

2.  What does `param()` define?

    A. Function inputs\
    B. Output formatting\
    C. Modules\
    D. Loops

3.  What does `[Parameter(Mandatory)]` do?

    A. Makes input required\
    B. Makes output mandatory\
    C. Requires administrator access\
    D. Creates a variable

4.  What does `[ValidateSet()]` do?

    A. Restricts input to approved values\
    B. Sorts a collection\
    C. Creates a set object\
    D. Exports data

5.  Why use functions?

    A. To make reusable, organized commands\
    B. To avoid all variables\
    C. To replace PowerShell Help\
    D. To disable the pipeline

------------------------------------------------------------------------

# Lab Complete

Continue to:

[Lesson 15 — Scripts](../lessons/lesson-15-scripts.md)

