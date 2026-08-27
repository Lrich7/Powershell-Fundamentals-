# Lab 09 --- Operators

## Lab Objective

In this lab, you will practice using PowerShell operators to calculate,
compare, test, and transform values.

You will:

-   Use arithmetic operators.
-   Use assignment operators.
-   Compare numbers and strings.
-   Practice case-sensitive comparison.
-   Use wildcard operators.
-   Test collection membership.
-   Combine Boolean conditions.
-   Test data types.
-   Use `-join` and `-split`.
-   Practice operator precedence.
-   Apply operators to realistic PowerShell data.

------------------------------------------------------------------------

## Before You Begin

Complete Lessons 01--09.

------------------------------------------------------------------------

# Exercise 1 --- Arithmetic

Run:

``` powershell
10 + 5
10 - 5
10 * 5
10 / 2
10 % 3
```

Record the results:

``` text
10 + 5 = ______
10 - 5 = ______
10 * 5 = ______
10 / 2 = ______
10 % 3 = ______
```

### Question

What does `%` return?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 2 --- Assignment

Run:

``` powershell
$count = 10
$count += 5
$count
```

Then:

``` powershell
$count -= 3
$count
```

Now:

``` powershell
$count *= 2
$count
```

### Question

What is the purpose of compound assignment operators?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 3 --- Comparisons

Run:

``` powershell
10 -eq 10
10 -ne 5
10 -gt 5
10 -ge 10
5 -lt 10
5 -le 5
```

What type of values are returned?

``` text
______________________
```

------------------------------------------------------------------------

# Exercise 4 --- String Comparison

Run:

``` powershell
'PowerShell' -eq 'powershell'
```

Then:

``` powershell
'PowerShell' -ceq 'powershell'
```

### Question

What does the `c` in `-ceq` indicate?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 5 --- Wildcards

Run:

``` powershell
'Server01' -like 'Server*'
```

Then:

``` powershell
'Server01' -like 'Server0?'
```

Now test:

``` powershell
'PC-001' -notlike 'Server*'
```

Record:

``` text
________________________________________
```

------------------------------------------------------------------------

# Exercise 6 --- Collection Membership

Create:

``` powershell
$servers = 'Server01', 'Server02', 'Server03'
```

Test:

``` powershell
$servers -contains 'Server02'
```

Then:

``` powershell
'Server02' -in $servers
```

Now test for `Server04`.

``` powershell
# Your command using -contains:
```

``` powershell
# Your command using -in:
```

Complete:

``` text
Collection -contains Value

Value __________ Collection
```

------------------------------------------------------------------------

# Exercise 7 --- Logical Operators

Run:

``` powershell
$department = 'IT'
$status = 'Active'
```

Test:

``` powershell
($department -eq 'IT') -and ($status -eq 'Active')
```

Change:

``` powershell
$status = 'Retired'
```

Run the test again.

Now create a condition that returns true when the status is either:

``` text
Active
OR
Available
```

``` powershell
# Your expression:
```

------------------------------------------------------------------------

# Exercise 8 --- -not

Run:

``` powershell
$isEnabled = $false
-not $isEnabled
```

Then:

``` powershell
!$isEnabled
```

### Question

What does `-not` do?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 9 --- Type Operators

Run:

``` powershell
$value = '25'
$value -is [string]
$value -is [int]
```

Then:

``` powershell
$value -as [int]
```

Inspect:

``` powershell
($value -as [int]).GetType()
```

### Challenge

Try converting:

``` powershell
'PowerShell' -as [int]
```

What is returned?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 10 --- Join

Create:

``` powershell
$computers = 'PC-001', 'PC-002', 'PC-003'
```

Run:

``` powershell
$computers -join ', '
```

Now join them with:

``` text
 |
```

between each value.

``` powershell
# Your command:
```

------------------------------------------------------------------------

# Exercise 11 --- Split

Run:

``` powershell
'PC-001,PC-002,PC-003' -split ','
```

Store:

``` powershell
$list = 'Laptop|Monitor|Printer' -split '\|'
```

Display:

``` powershell
$list
```

Count:

``` powershell
$list.Count
```

------------------------------------------------------------------------

# Exercise 12 --- Operator Precedence

Predict before running:

``` powershell
2 + 3 * 4
```

Prediction:

``` text
______________________
```

Run it.

Now:

``` powershell
(2 + 3) * 4
```

### Question

Why are the results different?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 13 --- Operators with Real Objects

Display running services:

``` powershell
Get-Service |
    Where-Object Status -eq 'Running'
```

Now keep running services whose names begin with `Win`:

``` powershell
Get-Service |
    Where-Object {
        ($_.Status -eq 'Running') -and
        ($_.Name -like 'Win*')
    }
```

Identify the operators being used:

``` text
Comparison: ______________________
Wildcard:   ______________________
Logical:    ______________________
```

------------------------------------------------------------------------

# End-of-Lab Challenge --- Asset Logic

Create:

``` powershell
$assetTag = 'LT-1001'
$deviceType = 'Laptop'
$status = 'Active'
$assignedTo = 'Alex'
```

Write expressions to test:

1.  Is the status `Active`?
2.  Is the device type `Laptop`?
3.  Is the asset tag like `LT-*`?
4.  Is the status Active **and** device type Laptop?
5.  Is the assigned user either `Alex` **or** `Jordan`?

``` powershell
# Your expressions:
```

### Bonus

Create:

``` powershell
$approvedTypes = 'Laptop', 'Monitor', 'Printer'
```

Test whether:

``` text
Laptop
```

is in the approved collection.

------------------------------------------------------------------------

# Knowledge Check

1.  Which operator means equal to?

    A. `=`\
    B. `-eq`\
    C. `==`\
    D. `-is`

2.  Which operator tests whether a collection contains a value?

    A. `-contains`\
    B. `-like`\
    C. `-join`\
    D. `-split`

3.  Which logical operator requires both conditions to be true?

    A. `-or`\
    B. `-not`\
    C. `-and`\
    D. `-xor`

4.  What does `-join` do?

    A. Splits text\
    B. Combines values into a string\
    C. Tests equality\
    D. Creates a function

5.  Why are parentheses useful?

    A. They can control evaluation order and improve readability.\
    B. They always create arrays.\
    C. They remove variables.\
    D. They stop a pipeline.

------------------------------------------------------------------------

# Lab Complete

Continue to:

[Lesson 10 — Files and Folders](../lessons/lesson-10-files-and-folders.md)


