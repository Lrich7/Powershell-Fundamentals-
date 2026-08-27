# Lab 13 --- Conditions

## Lab Objective

In this lab, you will make PowerShell choose different actions based on
data.

You will:

-   Use `if`.
-   Use `if` / `else`.
-   Use `elseif`.
-   Combine conditions with logical operators.
-   Test Boolean values.
-   Test `$null` and empty strings.
-   Use `switch`.
-   Combine conditions with loops.
-   Build simple IT decision logic.

------------------------------------------------------------------------

## Before You Begin

Complete Lessons 01--13.

------------------------------------------------------------------------

# Exercise 1 --- if

Run:

``` powershell
$freeSpaceGB = 25

if ($freeSpaceGB -lt 30) {
    'Warning: Disk space is low.'
}
```

Change the value to `50`.

What happens?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 2 --- if / else

Run:

``` powershell
$serviceStatus = 'Stopped'

if ($serviceStatus -eq 'Running') {
    'Service is running.'
}
else {
    'Service is not running.'
}
```

Change the status to `Running`.

### Question

How many branches run?

``` text
______________________
```

------------------------------------------------------------------------

# Exercise 3 --- elseif

Create:

``` powershell
$freeSpaceGB = 15
```

Write logic that outputs:

``` text
Critical
```

when space is less than `10`;

``` text
Warning
```

when space is less than `30`;

otherwise:

``` text
OK
```

``` powershell
# Your condition:
```

Test values:

``` text
5
20
75
```

------------------------------------------------------------------------

# Exercise 4 --- Logical Operators

Create:

``` powershell
$department = 'IT'
$status = 'Active'
```

Run:

``` powershell
if (($department -eq 'IT') -and ($status -eq 'Active')) {
    'Active IT record.'
}
```

Now write a condition that displays a warning when status is:

``` text
Warning
OR
Error
```

``` powershell
# Your condition:
```

------------------------------------------------------------------------

# Exercise 5 --- Boolean Values

Run:

``` powershell
$isOnline = $true

if ($isOnline) {
    'Computer is online.'
}
```

Now:

``` powershell
$isOnline = $false

if (-not $isOnline) {
    'Computer is offline.'
}
```

### Question

Why is `if ($isOnline)` usually clearer than `if ($isOnline -eq $true)`?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 6 --- Test for \$null

Run:

``` powershell
$result = $null

if ($null -eq $result) {
    'No result was returned.'
}
```

Now assign:

``` powershell
$result = 'Found'
```

Run the condition again.

------------------------------------------------------------------------

# Exercise 7 --- Empty and Whitespace Strings

Run:

``` powershell
$assignedTo = ''

if ([string]::IsNullOrWhiteSpace($assignedTo)) {
    'This asset is unassigned.'
}
```

Test:

``` powershell
$assignedTo = '   '
```

Then:

``` powershell
$assignedTo = 'Alex'
```

### Question

Why can this test be more reliable than checking only:

``` powershell
$assignedTo -eq ''
```

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 8 --- Conditions with Real Command Output

Retrieve:

``` powershell
$spooler = Get-Service -Name Spooler
```

Check its status without changing it:

``` powershell
if ($spooler.Status -eq 'Running') {
    'Spooler is running.'
}
else {
    'Spooler is not running.'
}
```

> This exercise reports status only. Do not start or stop the service.

------------------------------------------------------------------------

# Exercise 9 --- switch

Create:

``` powershell
$deviceType = 'Laptop'
```

Use:

``` powershell
switch ($deviceType) {
    'Laptop'  { 'Portable computer' }
    'Monitor' { 'Display device' }
    'Printer' { 'Printing device' }
    default   { 'Unknown device type' }
}
```

Test all four paths.

------------------------------------------------------------------------

# Exercise 10 --- Conditions Inside a Loop

Create:

``` powershell
$assets = @(
    [PSCustomObject]@{ AssetTag='LT-1001'; AssignedTo='Alex' }
    [PSCustomObject]@{ AssetTag='LT-1002'; AssignedTo='' }
    [PSCustomObject]@{ AssetTag='MON-2001'; AssignedTo='Jordan' }
)
```

Process:

``` powershell
foreach ($asset in $assets) {
    if ([string]::IsNullOrWhiteSpace($asset.AssignedTo)) {
        "$($asset.AssetTag): UNASSIGNED"
    }
    else {
        "$($asset.AssetTag): Assigned to $($asset.AssignedTo)"
    }
}
```

------------------------------------------------------------------------

# Exercise 11 --- Decision Order

Consider:

``` powershell
$value = 5
```

If you test:

``` text
less than 30
```

before:

``` text
less than 10
```

what problem could occur in an `elseif` chain?

``` text
____________________________________________________
____________________________________________________
```

------------------------------------------------------------------------

# End-of-Lab Challenge --- Asset Status Review

Create or import several asset records containing:

``` text
AssetTag
DeviceType
AssignedTo
Status
```

For every asset, output one of these classifications:

``` text
ACTION REQUIRED
AVAILABLE
ASSIGNED
REVIEW
```

Rules:

-   If `Status` is `Retired` → `ACTION REQUIRED`
-   Else if `AssignedTo` is empty → `AVAILABLE`
-   Else if `Status` is `Active` → `ASSIGNED`
-   Otherwise → `REVIEW`

Use:

``` text
foreach
if
elseif
else
```

``` powershell
# Your solution:
```

### Bonus

Use `switch` to produce a separate description based on `DeviceType`.

------------------------------------------------------------------------

# Knowledge Check

1.  Which keyword adds another condition after `if`?

    A. `elseif`\
    B. `otherwise`\
    C. `then`\
    D. `next`

2.  Which logical operator requires both expressions to be true?

    A. `-or`\
    B. `-and`\
    C. `-not`\
    D. `-like`

3.  What is a useful way to test an empty or whitespace-only string?

    A. `[string]::IsNullOrWhiteSpace()`\
    B. `Get-EmptyString`\
    C. `Test-String`\
    D. `-contains`

4.  When is `switch` useful?

    A. Comparing one value against several possibilities\
    B. Sorting files\
    C. Importing CSV\
    D. Creating arrays

------------------------------------------------------------------------

# Lab Complete

Continue to:

[Lesson 14 — Functions](../lessons/lesson-14-functions.md)

