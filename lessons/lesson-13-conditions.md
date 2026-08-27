[lesson-13-conditions.md](https://github.com/user-attachments/files/31517397/lesson-13-conditions.md)

# Lesson 13 --- Conditions

## Learning Objectives

By the end of this lesson, you will be able to:

-   Explain conditional logic in PowerShell.
-   Use `if`, `elseif`, and `else`.
-   Build conditions with comparison and logical operators.
-   Test strings, numbers, Boolean values, and `$null`.
-   Use `switch` when comparing one value against several possibilities.
-   Combine conditions with loops and command output.
-   Apply conditional logic to common IT administration tasks.

------------------------------------------------------------------------

## What Is a Condition?

A condition allows PowerShell to make a decision.

For example:

``` text
IF a service is stopped
THEN display a warning
```

PowerShell evaluates an expression as either:

``` text
True
False
```

and decides which code should run.

> **Key Idea:** Conditions allow scripts to behave differently depending
> on the data or situation they encounter.

------------------------------------------------------------------------

# The if Statement

Basic syntax:

``` powershell
if ($condition) {
    # Run this code if the condition is true
}
```

Example:

``` powershell
$age = 25

if ($age -ge 18) {
    'Adult'
}
```

Because:

``` powershell
$age -ge 18
```

evaluates to `True`, the code inside the braces runs.

------------------------------------------------------------------------

# if and else

Use `else` when you want another action if the condition is false.

``` powershell
$serviceStatus = 'Stopped'

if ($serviceStatus -eq 'Running') {
    'Service is running.'
}
else {
    'Service is not running.'
}
```

Only one branch runs.

------------------------------------------------------------------------

# elseif

Use `elseif` when there are several possibilities.

``` powershell
$status = 'Warning'

if ($status -eq 'OK') {
    'Everything is normal.'
}
elseif ($status -eq 'Warning') {
    'Review the system.'
}
else {
    'Unknown status.'
}
```

PowerShell evaluates the conditions from top to bottom.

Once a matching branch runs, the remaining branches are skipped.

------------------------------------------------------------------------

# Comparison Operators in Conditions

The operators from Lesson 09 become especially important here.

Examples:

``` powershell
$count -eq 10
$count -ne 0
$count -gt 5
$count -ge 5
$count -lt 100
$count -le 100
$name -like 'Server*'
$name -match '^Server'
```

Each expression can produce a result that PowerShell uses in a
condition.

------------------------------------------------------------------------

# Logical Operators

You can combine conditions with:

``` text
-and
-or
-not
```

Example:

``` powershell
$department = 'IT'
$status = 'Active'

if (($department -eq 'IT') -and ($status -eq 'Active')) {
    'Active IT record.'
}
```

Both conditions must be true.

------------------------------------------------------------------------

# Using -or

``` powershell
$status = 'Warning'

if (($status -eq 'Warning') -or ($status -eq 'Error')) {
    'Attention is required.'
}
```

Only one condition needs to be true.

------------------------------------------------------------------------

# Using -not

``` powershell
$isEnabled = $false

if (-not $isEnabled) {
    'The account is disabled.'
}
```

For Boolean variables, this is often easier to read than comparing
explicitly to `$false`.

------------------------------------------------------------------------

# Testing Boolean Values

If a variable already contains `$true` or `$false`, you can use it
directly.

``` powershell
$isOnline = $true

if ($isOnline) {
    'Computer is online.'
}
```

You do not need:

``` powershell
if ($isOnline -eq $true)
```

The shorter version is normally clearer.

------------------------------------------------------------------------

# Testing for \$null

You can test whether a value is missing:

``` powershell
$result = $null

if ($null -eq $result) {
    'No result was returned.'
}
```

As discussed earlier, placing `$null` on the left is a useful PowerShell
convention:

``` powershell
$null -eq $result
```

------------------------------------------------------------------------

# Testing Empty Strings

Sometimes a value is not `$null` but is empty or contains only spaces.

PowerShell and .NET provide:

``` powershell
[string]::IsNullOrWhiteSpace()
```

Example:

``` powershell
$assignedTo = ''

if ([string]::IsNullOrWhiteSpace($assignedTo)) {
    'This asset is unassigned.'
}
```

This is useful when processing CSV or inventory data.

------------------------------------------------------------------------

# Nested Conditions

An `if` statement can contain another `if` statement.

Example:

``` powershell
$status = 'Active'
$department = 'IT'

if ($status -eq 'Active') {
    if ($department -eq 'IT') {
        'Active IT record.'
    }
}
```

Nested conditions are valid, but too many levels can make scripts
difficult to read.

Often a combined condition is simpler:

``` powershell
if (($status -eq 'Active') -and ($department -eq 'IT')) {
    'Active IT record.'
}
```

------------------------------------------------------------------------

# The switch Statement

When you are comparing one value against several possible values,
`switch` can be easier to read than many `elseif` statements.

Example:

``` powershell
$status = 'Warning'

switch ($status) {
    'OK' {
        'Everything is normal.'
    }

    'Warning' {
        'Review the system.'
    }

    'Error' {
        'Immediate attention required.'
    }

    default {
        'Unknown status.'
    }
}
```

------------------------------------------------------------------------

# switch with Wildcards

`switch` supports additional matching modes.

For example:

``` powershell
$computer = 'Server01'

switch -Wildcard ($computer) {
    'Server*' {
        'This is a server.'
    }

    'PC-*' {
        'This is a workstation.'
    }

    default {
        'Unknown device type.'
    }
}
```

------------------------------------------------------------------------

# Conditions with Command Output

You can use command results in conditions.

Example:

``` powershell
$service = Get-Service -Name Spooler

if ($service.Status -eq 'Running') {
    'Print Spooler is running.'
}
else {
    'Print Spooler is not running.'
}
```

This combines commands, objects, properties, operators, and conditions.

------------------------------------------------------------------------

# Test-Path in a Condition

`Test-Path` returns a Boolean value, making it ideal for `if`.

``` powershell
$path = 'C:\Temp'

if (Test-Path $path) {
    "The path $path exists."
}
else {
    "The path $path does not exist."
}
```

A common file-management pattern is:

``` powershell
if (-not (Test-Path $path)) {
    New-Item -Path $path -ItemType Directory
}
```

------------------------------------------------------------------------

# Conditions Inside Loops

Loops and conditions work together naturally.

``` powershell
$services = Get-Service

foreach ($service in $services) {
    if ($service.Status -eq 'Stopped') {
        "Stopped: $($service.Name)"
    }
}
```

You could often filter first:

``` powershell
$stoppedServices = Get-Service |
    Where-Object Status -eq 'Stopped'

foreach ($service in $stoppedServices) {
    "Stopped: $($service.Name)"
}
```

Use the approach that makes the task easiest to understand.

------------------------------------------------------------------------

# Practical IT Example --- Asset Status

Suppose:

``` powershell
$assets = Import-Csv C:\Temp\assets.csv
```

You can classify records:

``` powershell
foreach ($asset in $assets) {
    if ($asset.Status -eq 'Retired') {
        "RETIRED: $($asset.AssetTag)"
    }
    elseif ([string]::IsNullOrWhiteSpace($asset.AssignedTo)) {
        "UNASSIGNED: $($asset.AssetTag)"
    }
    else {
        "ACTIVE: $($asset.AssetTag) - $($asset.AssignedTo)"
    }
}
```

The script now makes decisions for each record.

------------------------------------------------------------------------

# Keep Conditions Readable

Avoid cramming too much logic into one line.

Instead of:

``` powershell
if (($status -eq 'Active') -and ($department -eq 'IT') -and ($type -eq 'Laptop') -and ($age -lt 5)) {
```

consider storing meaningful tests:

``` powershell
$isActive = $status -eq 'Active'
$isIT = $department -eq 'IT'
$isLaptop = $type -eq 'Laptop'
$isCurrent = $age -lt 5

if ($isActive -and $isIT -and $isLaptop -and $isCurrent) {
    'Record matches requirements.'
}
```

Readable scripts are easier to troubleshoot and maintain.

------------------------------------------------------------------------

# Key Takeaways

-   Conditions allow PowerShell to make decisions.
-   `if` runs code when a condition is true.
-   `else` handles the alternative.
-   `elseif` handles additional possibilities.
-   Comparison operators build tests.
-   `-and`, `-or`, and `-not` combine or reverse conditions.
-   Boolean variables can be tested directly.
-   `$null` represents no value.
-   `[string]::IsNullOrWhiteSpace()` is useful for missing text values.
-   `switch` is useful when one value has several possible matches.
-   Conditions work naturally with loops, objects, and command output.
-   Keep complex conditions readable.

------------------------------------------------------------------------

# Lab

Continue to:

[Lab 13 — Conditions](../labs/lesson-13-lab-13-conditions.md)


In the lab, you will build `if`, `elseif`, and `else` statements,
combine operators, test paths and object properties, use `switch`, and
add decision-making to loops.

------------------------------------------------------------------------

## Additional Resources

-   [About If --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_if?view=powershell-7.6)
-   [About Switch --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_switch?view=powershell-7.6)
-   [About Logical Operators --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_logical_operators?view=powershell-7.6)
-   [About Comparison Operators --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_comparison_operators?view=powershell-7.6)
