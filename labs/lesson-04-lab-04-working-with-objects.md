[Uploading lab-04-working-with-objects.md…]()

# Lab 04 --- Working with Objects

## Lab Objective

In this lab, you will practice working with PowerShell objects.

You will:

-   Recognize that command output contains structured objects.
-   Inspect objects with `Get-Member`.
-   Identify properties and methods.
-   Access properties using dot notation.
-   Choose properties with `Select-Object`.
-   Perform basic filtering with `Where-Object`.
-   Use command Help and object discovery together.

------------------------------------------------------------------------

## Before You Begin

Complete Lessons 01--04.

This lab uses read-only commands.

------------------------------------------------------------------------

# Exercise 1 --- What Is Behind the Table?

Run:

``` powershell
Get-Service
```

PowerShell displays service information in a table.

Now run:

``` powershell
Get-Service | Get-Member
```

Look near the top of the output for:

``` text
TypeName
```

Record the type name shown on your computer:

``` text
TypeName:
____________________________________________________
```

### Question

Did `Get-Service` return only plain text?

``` text
Answer: ______________________
```

Explain:

``` text
____________________________________________________
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 2 --- Properties vs. Methods

Run:

``` powershell
Get-Service | Get-Member
```

Look at the:

``` text
MemberType
```

column.

Find examples of properties.

Record three:

``` text
1. ______________________
2. ______________________
3. ______________________
```

Now find one method if your service object exposes one:

``` text
Method: ______________________
```

### Question

What is the basic difference between a property and a method?

``` text
Property:
____________________________________________________

Method:
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 3 --- Inspect Process Objects

Run:

``` powershell
Get-Process | Get-Member
```

Compare the members with those from:

``` powershell
Get-Service | Get-Member
```

Find three process properties that look useful:

``` text
1. ______________________
2. ______________________
3. ______________________
```

### Think About It

Why don't service objects and process objects have exactly the same
properties?

``` text
____________________________________________________
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 4 --- Select Properties

Run:

``` powershell
Get-Service | Select-Object Name, Status
```

Compare it with:

``` powershell
Get-Service
```

Now try:

``` powershell
Get-Service |
    Select-Object Name, DisplayName, Status
```

### Question

Did `Select-Object` change the actual Windows services?

``` text
Answer: ______________________
```

What did it change?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 5 --- Inspect File Objects

Run:

``` powershell
Get-ChildItem | Get-Member
```

Depending on your current folder, you may have files, directories, or
both.

Move to a folder containing files if necessary.

For example:

``` powershell
Set-Location $HOME
```

Then:

``` powershell
Get-ChildItem
```

Inspect the objects:

``` powershell
Get-ChildItem | Get-Member
```

Look for properties such as:

``` text
Name
FullName
Length
Extension
CreationTime
LastWriteTime
```

The exact members can depend on the object type.

------------------------------------------------------------------------

# Exercise 6 --- Select File Properties

Run:

``` powershell
Get-ChildItem |
    Select-Object Name, LastWriteTime
```

If the folder contains files, try:

``` powershell
Get-ChildItem -File |
    Select-Object Name, Length, LastWriteTime
```

> `-File` availability depends on the PowerShell environment and
> provider.

### Question

Which property appears to represent file size?

``` text
Answer: ______________________
```

------------------------------------------------------------------------

# Exercise 7 --- Dot Notation

Retrieve one service:

``` powershell
Get-Service -Name Spooler
```

Store it temporarily in a variable:

``` powershell
$service = Get-Service -Name Spooler
```

> Variables will be covered in depth later. For now, this simply gives
> us an easy way to inspect one object.

Access its `Name` property:

``` powershell
$service.Name
```

Then:

``` powershell
$service.Status
```

And:

``` powershell
$service.DisplayName
```

### Question

What does the dot in:

``` powershell
$service.Status
```

allow you to access?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 8 --- Filter Objects

Run:

``` powershell
Get-Service |
    Where-Object Status -eq 'Running'
```

Now:

``` powershell
Get-Service |
    Where-Object Status -eq 'Stopped'
```

### Question

What property is `Where-Object` examining?

``` text
Answer: ______________________
```

What value is it comparing against in the first command?

``` text
Answer: ______________________
```

------------------------------------------------------------------------

# Exercise 9 --- Filter Then Select

Combine the concepts:

``` powershell
Get-Service |
    Where-Object Status -eq 'Running' |
    Select-Object Name, DisplayName, Status
```

Read the command from left to right.

Fill in the workflow:

``` text
Get-Service
    ↓
________________________________________
    ↓
________________________________________
```

------------------------------------------------------------------------

# Exercise 10 --- Use Get-Member to Solve a Problem

Your task:

> Display process names and their process IDs.

You know:

``` powershell
Get-Process
```

but pretend you do not remember the property that stores the ID.

Use:

``` powershell
Get-Process | Get-Member
```

Find the appropriate property.

Then build:

``` powershell
Get-Process |
    Select-Object <property1>, <property2>
```

Write your final command:

``` powershell
# Your command:
```

------------------------------------------------------------------------

# Exercise 11 --- Help + Object Discovery

Your task:

> Find the properties available on objects returned by `Get-ChildItem`.

Use both:

``` powershell
Get-Help Get-ChildItem
```

and:

``` powershell
Get-ChildItem | Get-Member
```

### Question

What does Help tell you that `Get-Member` does not?

``` text
____________________________________________________
```

What does `Get-Member` tell you that Help may not show as clearly?

``` text
____________________________________________________
```

This is a powerful PowerShell habit:

``` text
Command Help + Object Inspection
```

------------------------------------------------------------------------

# End-of-Lab Challenge --- Inspect an Unfamiliar Object

Use:

``` powershell
Get-Date
```

Do not assume the displayed date is just text.

## Task 1

Inspect it:

``` powershell
# Your Get-Member command:
```

## Task 2

Find properties representing:

``` text
Year
Month
Day
Hour
```

## Task 3

Store the result:

``` powershell
$date = Get-Date
```

Display only the year using dot notation.

``` powershell
# Your command:
```

## Task 4

Use `Select-Object` to display:

``` text
Year
Month
Day
DayOfWeek
```

``` powershell
# Your command:
```

## Task 5

In your own words, explain why working with the underlying object is
more useful than treating the displayed date as plain text.

``` text
____________________________________________________
____________________________________________________
____________________________________________________
```

------------------------------------------------------------------------

# Knowledge Check

1.  Which command is used to inspect the properties and methods of
    PowerShell objects?

    A. `Get-Help`\
    B. `Get-Member`\
    C. `Get-Property`\
    D. `Show-Object`

2.  A property represents:

    A. Information about an object\
    B. An action the object performs\
    C. A PowerShell module\
    D. A command alias

3.  A method generally represents:

    A. A display format\
    B. Information only\
    C. An action associated with an object\
    D. A Help topic

4.  What does `Select-Object Name, Status` do?

    A. Stops all services except those named.\
    B. Selects those properties for the output.\
    C. Renames the properties.\
    D. Deletes the other properties from Windows.

5.  Why is `Get-Member` important?

    A. It tells you what data and capabilities an object exposes.\
    B. It installs PowerShell modules.\
    C. It changes Windows services.\
    D. It replaces `Get-Help`.

------------------------------------------------------------------------

# Lab Complete

You can now inspect PowerShell objects and identify useful properties.

The next lab uses those objects in one of PowerShell's most important
features:

> **The Pipeline.**

Continue to:

[Lab 05 --- The Pipeline](lab-05-the-pipeline.md)
