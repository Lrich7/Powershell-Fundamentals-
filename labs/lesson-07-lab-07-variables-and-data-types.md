# Lab 07 --- Variables and Data Types

## Lab Objective

In this lab, you will practice storing and reusing PowerShell data.

You will:

-   Create and update variables.
-   Store strings and numbers.
-   Store PowerShell objects.
-   Inspect types with `.GetType()`.
-   Practice string expansion.
-   Work with Boolean values.
-   Create a basic array.
-   Practice type casting.
-   Understand `$null`.

------------------------------------------------------------------------

## Before You Begin

Complete Lessons 01--07.

This lab is read-only except for variables created in your PowerShell
session.

------------------------------------------------------------------------

# Exercise 1 --- Create Variables

Create:

``` powershell
$computerName = 'PC-001'
$department = 'IT'
$count = 10
```

Display each variable:

``` powershell
$computerName
$department
$count
```

### Question

What character begins a PowerShell variable name?

``` text
______________________
```

------------------------------------------------------------------------

# Exercise 2 --- Update a Variable

Run:

``` powershell
$status = 'Pending'
```

Display it.

Now:

``` powershell
$status = 'Complete'
```

Display it again.

### Question

What happened to the original value?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 3 --- Numbers

Run:

``` powershell
$count = 10
$count + 5
```

Now:

``` powershell
$count++
$count
```

Then:

``` powershell
$count--
$count
```

### Question

What do `++` and `--` do?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 4 --- Inspect Data Types

Run:

``` powershell
$name = 'PowerShell'
$name.GetType()
```

Then:

``` powershell
$number = 42
$number.GetType()
```

Now:

``` powershell
$date = Get-Date
$date.GetType()
```

Record the type names:

``` text
$name:   ______________________
$number: ______________________
$date:   ______________________
```

------------------------------------------------------------------------

# Exercise 5 --- Why Type Matters

Run:

``` powershell
10 + 5
```

Then:

``` powershell
'10' + '5'
```

Record the results:

``` text
Numbers: ______________________
Strings: ______________________
```

Explain why they differ:

``` text
____________________________________________________
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 6 --- Store Command Output

Run:

``` powershell
$service = Get-Service -Name Spooler
```

Display it:

``` powershell
$service
```

Inspect it:

``` powershell
$service.GetType()
```

Then access:

``` powershell
$service.Name
$service.Status
$service.DisplayName
```

### Question

Did the variable store only the text displayed on the screen?

``` text
Answer: ______________________
```

------------------------------------------------------------------------

# Exercise 7 --- Double Quotes

Run:

``` powershell
$name = 'Alex'
"Hello, $name"
```

Now:

``` powershell
'Hello, $name'
```

### Question

Complete:

``` text
Double quotes: ______________________________________

Single quotes: ______________________________________
```

------------------------------------------------------------------------

# Exercise 8 --- Subexpressions

Run:

``` powershell
$service = Get-Service -Name Spooler
```

Then:

``` powershell
"The $($service.DisplayName) service is $($service.Status)."
```

Now create your own sentence using:

``` text
$service.Name
```

and:

``` text
$service.Status
```

``` powershell
# Your command:
```

------------------------------------------------------------------------

# Exercise 9 --- Boolean Values

Run:

``` powershell
$isEnabled = $true
$isEnabled
$isEnabled.GetType()
```

Now:

``` powershell
10 -gt 5
```

and:

``` powershell
10 -lt 5
```

### Question

What type of result do comparison expressions produce?

``` text
______________________
```

------------------------------------------------------------------------

# Exercise 10 --- Basic Arrays

Create:

``` powershell
$computers = 'PC-001', 'PC-002', 'PC-003'
```

Display:

``` powershell
$computers
```

Count the values:

``` powershell
$computers.Count
```

Retrieve the first:

``` powershell
$computers[0]
```

Retrieve the second:

``` powershell
$computers[1]
```

### Question

What number does PowerShell use for the first array index?

``` text
______________________
```

------------------------------------------------------------------------

# Exercise 11 --- Type Casting

Run:

``` powershell
[int]$number = '25'
```

Inspect:

``` powershell
$number.GetType()
```

Now try:

``` powershell
[int]'PowerShell'
```

### Question

Why did the second conversion fail?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 12 --- \$null

Run:

``` powershell
$result = $null
```

Test it:

``` powershell
$null -eq $result
```

Then:

``` powershell
$result = 'Found'
$null -eq $result
```

### Question

What does `$null` represent?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# End-of-Lab Challenge --- Build a Device Record

Create variables for:

``` text
Asset tag
Computer name
Department
Assigned user
Active status
```

Suggested value types:

``` text
Text
Text
Text
Text
Boolean
```

Then create a sentence using variable expansion that reads similar to:

``` text
Asset LT-1001 is PC-001, assigned to Alex in IT. Active: True
```

Do not copy that exact record; create your own values.

``` powershell
# Your variables:
```

``` powershell
# Your output sentence:
```

### Bonus

Store the `Spooler` service in a variable and output a sentence
describing its current status.

------------------------------------------------------------------------

# Knowledge Check

1.  How do PowerShell variable names begin?

    A. `%`\
    B. `$`\
    C. `@`\
    D. `#`

2.  Which method identifies a value's type?

    A. `.GetType()`\
    B. `.Type()`\
    C. `Get-DataType`\
    D. `Get-MemberType`

3.  Which quotes normally expand variables?

    A. Single quotes\
    B. Double quotes\
    C. Neither\
    D. Both always behave identically

4.  What is `$true`?

    A. A string\
    B. A Boolean value\
    C. A command\
    D. An array

5.  What does `$null` represent?

    A. Zero\
    B. An empty string only\
    C. Absence of a value\
    D. False only

------------------------------------------------------------------------

# Lab Complete

Continue to:

[Lab 08 --- Arrays and Collections](lab-08-arrays-and-collections.md)
