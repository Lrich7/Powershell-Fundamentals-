# Lab 08 --- Arrays and Collections

## Lab Objective

In this lab, you will practice storing and processing multiple values
and objects.

You will:

-   Create arrays.
-   Use `@()`.
-   Access values with indexes.
-   Use negative indexes and ranges.
-   Count collection items.
-   Add items to arrays.
-   Store command output in collections.
-   Use `foreach` and `ForEach-Object`.
-   Filter and sort stored collections.
-   Extract property values.

------------------------------------------------------------------------

## Before You Begin

Complete Lessons 01--08.

------------------------------------------------------------------------

# Exercise 1 --- Create an Array

Create:

``` powershell
$servers = 'Server01', 'Server02', 'Server03'
```

Display:

``` powershell
$servers
```

Check:

``` powershell
$servers.Count
```

------------------------------------------------------------------------

# Exercise 2 --- Explicit Array Syntax

Create another array:

``` powershell
$devices = @(
    'LT-1001'
    'LT-1002'
    'MON-2001'
    'PRN-3001'
)
```

Display:

``` powershell
$devices
```

### Question

What does `@()` allow you to create explicitly?

``` text
______________________
```

------------------------------------------------------------------------

# Exercise 3 --- Indexes

Using `$devices`, retrieve:

``` text
First item
Second item
Last item
```

Commands:

``` powershell
# First:
```

``` powershell
# Second:
```

``` powershell
# Last:
```

### Question

What does index `-1` mean?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 4 --- Ranges

Create:

``` powershell
$numbers = 1..10
```

Display items at indexes `2` through `5`:

``` powershell
$numbers[2..5]
```

Now retrieve the first three devices from `$devices` using an index
range.

``` powershell
# Your command:
```

------------------------------------------------------------------------

# Exercise 5 --- Add an Item

Start with:

``` powershell
$servers = 'Server01', 'Server02'
```

Add:

``` powershell
$servers += 'Server03'
```

Display:

``` powershell
$servers
```

### Question

Is `+=` convenient for small arrays?

``` text
Answer: ______________________
```

Why might other collection types be better for collections that change
constantly?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 6 --- Store Command Output

Run:

``` powershell
$services = Get-Service
```

Count:

``` powershell
$services.Count
```

Inspect the first object:

``` powershell
$services[0]
```

Then:

``` powershell
$services[0].Name
```

### Question

What does `$services` contain?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 7 --- Filter a Stored Collection

Using `$services`:

``` powershell
$runningServices = $services |
    Where-Object Status -eq 'Running'
```

Count:

``` powershell
$runningServices.Count
```

Then:

``` powershell
$runningServices |
    Sort-Object Name |
    Select-Object -First 10 Name, Status
```

------------------------------------------------------------------------

# Exercise 8 --- foreach

Create:

``` powershell
$computers = 'PC-001', 'PC-002', 'PC-003'
```

Process each value:

``` powershell
foreach ($computer in $computers) {
    "Checking $computer"
}
```

### Question

What value does `$computer` represent during each iteration?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 9 --- ForEach-Object

Run:

``` powershell
$computers |
    ForEach-Object {
        "Checking $_"
    }
```

Compare with the previous `foreach`.

Complete:

``` text
foreach is useful when:
____________________________________________________

ForEach-Object is useful when:
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 10 --- Object Collections

Use:

``` powershell
$services |
    Select-Object -First 5 |
    ForEach-Object {
        "$($_.Name): $($_.Status)"
    }
```

### Question

What does `$_` represent inside `ForEach-Object`?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 11 --- Extract Property Values

Run:

``` powershell
$services.Name
```

Then:

``` powershell
$services |
    Select-Object -ExpandProperty Name
```

### Question

What kind of values are being returned?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 12 --- Unique Collection Values

Run:

``` powershell
$services |
    Select-Object -ExpandProperty Status |
    Sort-Object -Unique
```

What unique values appear?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 13 --- Empty Array vs. \$null

Create:

``` powershell
$empty = @()
```

Check:

``` powershell
$empty.Count
```

Now:

``` powershell
$nothing = $null
```

### Question

Explain the difference:

``` text
@():
____________________________________________________

$null:
____________________________________________________
```

------------------------------------------------------------------------

# End-of-Lab Challenge --- Device Processing

Create this collection:

``` powershell
$devices = @(
    'LT-1001'
    'LT-1002'
    'MON-2001'
    'LT-1003'
    'PRN-3001'
)
```

Complete these tasks:

1.  Display the total count.
2.  Display the first item.
3.  Display the last item.
4.  Display only items beginning with `LT-`.
5.  Sort the collection alphabetically.
6.  Use `foreach` to output:

``` text
Processing device: <device>
```

Write your commands:

``` powershell
# Your solution:
```

### Bonus

Store all running Windows services in a variable, count them, sort them,
and display the first 10 names.

------------------------------------------------------------------------

# Knowledge Check

1.  What index represents the first item in a PowerShell array?

    A. `1`\
    B. `0`\
    C. `-1`\
    D. `First`

2.  What does `-1` retrieve?

    A. First item\
    B. Nothing\
    C. Last item\
    D. All items

3.  What does `.Count` provide?

    A. The first value\
    B. Number of items\
    C. Data type\
    D. Sort order

4.  What does `$_` usually represent inside `ForEach-Object`?

    A. The whole array\
    B. Current pipeline object\
    C. PowerShell version\
    D. Previous command

5.  What is the difference between `$null` and `@()`?

    A. There is none.\
    B. `$null` is no value; `@()` is an empty collection.\
    C. `@()` is always false.\
    D. `$null` is an array.

------------------------------------------------------------------------

# Lab Complete

Continue to:

[Lesson 09 — Operators](../lessons/lesson-09-operators.md)

