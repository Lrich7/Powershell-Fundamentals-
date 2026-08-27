# Lab 06 --- Filtering and Sorting

## Lab Objective

In this lab, you will practice controlling which PowerShell objects
remain in the pipeline and how those objects are organized.

You will:

-   Filter at the source when possible.
-   Use `Where-Object`.
-   Practice comparison operators.
-   Filter text with `-like` and `-match`.
-   Combine conditions with `-and` and `-or`.
-   Sort objects in ascending and descending order.
-   Sort by multiple properties.
-   Find top results.
-   Use `Sort-Object -Unique`.
-   Build increasingly independent filtering pipelines.

------------------------------------------------------------------------

## Before You Begin

Complete Lessons 01--06.

This lab uses read-only commands.

------------------------------------------------------------------------

# Exercise 1 --- Filter at the Source

Retrieve the Print Spooler service directly:

``` powershell
Get-Service -Name Spooler
```

Now retrieve all services and filter afterward:

``` powershell
Get-Service |
    Where-Object Name -eq 'Spooler'
```

Both should locate the same service.

### Question

Which version filters earlier?

``` text
Answer: ______________________
```

Why is filtering at the source often preferable?

``` text
____________________________________________________
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 2 --- Filter by Status

Display only running services:

``` powershell
Get-Service |
    Where-Object Status -eq 'Running'
```

Display only stopped services:

``` powershell
Get-Service |
    Where-Object Status -eq 'Stopped'
```

Now display services whose status is **not** running:

``` powershell
# Your command:
```

Which comparison operator did you use?

``` text
______________________
```

------------------------------------------------------------------------

# Exercise 3 --- Numeric Comparisons

Inspect processes:

``` powershell
Get-Process |
    Select-Object Name, Id, CPU
```

Filter processes whose CPU value is greater than `10`:

``` powershell
Get-Process |
    Where-Object CPU -gt 10
```

Your computer may return many, few, or no results.

Try another threshold if needed.

### Challenge

Display processes whose ID is greater than `1000`.

``` powershell
# Your command:
```

Then sort those processes by ID:

``` powershell
# Your command:
```

------------------------------------------------------------------------

# Exercise 4 --- Wildcard Filtering

Find services whose names begin with:

``` text
Win
```

``` powershell
Get-Service |
    Where-Object Name -like 'Win*'
```

Now find services whose display names contain:

``` text
Windows
```

``` powershell
# Your command:
```

### Question

What does `*` represent?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 5 --- -like vs. -match

Run:

``` powershell
Get-Service |
    Where-Object Name -like 'Win*'
```

Now:

``` powershell
Get-Service |
    Where-Object Name -match '^Win'
```

Compare the results.

### Question

Complete:

``` text
-like uses: ___________________________

-match uses: __________________________
```

You do not need to master regular expressions yet.

------------------------------------------------------------------------

# Exercise 6 --- Script Block Filtering

Compare:

``` powershell
Get-Service |
    Where-Object Status -eq 'Running'
```

with:

``` powershell
Get-Service |
    Where-Object { $_.Status -eq 'Running' }
```

### Question

What does `$_` represent?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 7 --- Multiple Conditions

Find running services whose names begin with `Win`:

``` powershell
Get-Service |
    Where-Object {
        $_.Status -eq 'Running' -and
        $_.Name -like 'Win*'
    }
```

Now create a filter that keeps a service when:

``` text
Status is Stopped
OR
Name begins with Win
```

``` powershell
# Your command:
```

### Question

When should you use `-and` instead of `-or`?

``` text
____________________________________________________
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 8 --- Basic Sorting

Sort services alphabetically:

``` powershell
Get-Service |
    Sort-Object Name
```

Reverse the order:

``` powershell
Get-Service |
    Sort-Object Name -Descending
```

Now sort services by:

``` text
Status
then Name
```

``` powershell
# Your command:
```

------------------------------------------------------------------------

# Exercise 9 --- Find the Top Processes

Run:

``` powershell
Get-Process |
    Sort-Object CPU -Descending |
    Select-Object -First 10 Name, Id, CPU
```

### Question

Why must `Sort-Object` appear before `Select-Object -First 10` if the
goal is to find the largest CPU values?

``` text
____________________________________________________
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 10 --- Unique Values

Run:

``` powershell
Get-Service |
    Select-Object -ExpandProperty Status |
    Sort-Object -Unique
```

### Question

What did `-Unique` do?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 11 --- Filtering Files

Move to a folder containing files:

``` powershell
Set-Location $HOME
```

Find text files:

``` powershell
Get-ChildItem -File |
    Where-Object Extension -eq '.txt'
```

If you do not have `.txt` files, choose an extension that exists.

Now sort matching files by name:

``` powershell
# Your command:
```

------------------------------------------------------------------------

# End-of-Lab Challenge --- Process Review

You receive this request:

> IT wants a console report of the 10 processes with the highest CPU
> values. Show only the process name, ID, and CPU value.

Build the pipeline yourself.

Requirements:

``` text
Get-Process
Sort-Object
Select-Object
```

Final command:

``` powershell
# Your solution:
```

### Bonus

Create another report that shows only processes:

-   Whose names begin with `s`
-   Sorted alphabetically
-   Showing `Name` and `Id`

``` powershell
# Your solution:
```

------------------------------------------------------------------------

# Knowledge Check

1.  Which command filters pipeline objects?

    A. `Select-Object`\
    B. `Where-Object`\
    C. `Sort-Object`\
    D. `Get-Member`

2.  What does `-gt` mean?

    A. Equal to\
    B. Less than\
    C. Greater than\
    D. Not equal to

3.  Which operator uses wildcard matching?

    A. `-match`\
    B. `-like`\
    C. `-eq`\
    D. `-and`

4.  Which switch reverses the normal sort direction?

    A. `-Reverse`\
    B. `-Descending`\
    C. `-Backwards`\
    D. `-HighFirst`

5.  Why does pipeline order matter?

    A. Each stage changes which objects are available to the next
    stage.\
    B. PowerShell executes pipelines right to left.\
    C. Sorting always happens automatically.\
    D. It does not matter.

------------------------------------------------------------------------

# Lab Complete

You have practiced narrowing and organizing PowerShell data.

Continue to:

[Lab 07 --- Variables and Data
Types](lab-07-variables-and-data-types.md)
