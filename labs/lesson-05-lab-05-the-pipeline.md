# Lab 05 --- The Pipeline

## Lab Objective

In this lab, you will combine the skills from Lessons 01--05.

You will:

-   Send objects through the pipeline.
-   Inspect pipeline objects with `Get-Member`.
-   Filter with `Where-Object`.
-   Choose properties with `Select-Object`.
-   Limit results.
-   Sort with `Sort-Object`.
-   Count objects with `Measure-Object`.
-   Build multi-stage pipelines.
-   Troubleshoot a pipeline one stage at a time.

------------------------------------------------------------------------

## Before You Begin

Complete Lessons 01--05.

This lab uses read-only commands.

> **Goal:** Do not think of the pipeline as one giant command. Think of
> it as a series of small stages, with objects moving from one stage to
> the next.

------------------------------------------------------------------------

# Exercise 1 --- Your First Pipeline

Run:

``` powershell
Get-Service
```

Now:

``` powershell
Get-Service | Get-Member
```

Explain the pipeline in your own words:

``` text
Get-Service produces:
____________________________________________________

Those results are sent to:
____________________________________________________

The second command does:
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 2 --- Select Properties

Run:

``` powershell
Get-Service |
    Select-Object Name, Status
```

Now add another property:

``` powershell
Get-Service |
    Select-Object Name, DisplayName, Status
```

### Question

What is flowing through the pipeline from `Get-Service`?

``` text
A. Plain formatted text
B. Service objects
C. CSV rows
D. Command names
```

Answer:

``` text
______________________
```

------------------------------------------------------------------------

# Exercise 3 --- Limit Results

Use:

``` powershell
Get-Process |
    Select-Object -First 5
```

Now select only useful properties:

``` powershell
Get-Process |
    Select-Object -First 5 Name, Id
```

Try:

``` powershell
Get-Service |
    Select-Object -First 10 Name, Status
```

### Question

What does:

``` text
-First 10
```

do?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 4 --- Filter Services

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

Then:

``` powershell
Get-Service |
    Where-Object Status -eq 'Running' |
    Select-Object Name, DisplayName, Status
```

### Describe Each Stage

``` text
Stage 1 — Get-Service:
____________________________________________________

Stage 2 — Where-Object:
____________________________________________________

Stage 3 — Select-Object:
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 5 --- Sort Objects

Run:

``` powershell
Get-Service |
    Sort-Object Name
```

Now reverse the order:

``` powershell
Get-Service |
    Sort-Object Name -Descending
```

Try processes:

``` powershell
Get-Process |
    Sort-Object Id
```

### Question

What does `Sort-Object` need in order to know how to organize the
objects?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 6 --- Filter, Sort, Select

Build this pipeline one step at a time.

### Stage 1

``` powershell
Get-Service
```

Confirm that it works.

### Stage 2

``` powershell
Get-Service |
    Where-Object Status -eq 'Running'
```

Confirm that only running services remain.

### Stage 3

Add sorting:

``` powershell
Get-Service |
    Where-Object Status -eq 'Running' |
    Sort-Object Name
```

### Stage 4

Select useful properties:

``` powershell
Get-Service |
    Where-Object Status -eq 'Running' |
    Sort-Object Name |
    Select-Object Name, DisplayName, Status
```

This is a complete multi-stage pipeline.

### Question

Why is building a pipeline one stage at a time useful?

``` text
____________________________________________________
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 7 --- Count Objects

Run:

``` powershell
Get-Service |
    Measure-Object
```

Look for:

``` text
Count
```

Now count only running services:

``` powershell
Get-Service |
    Where-Object Status -eq 'Running' |
    Measure-Object
```

Count stopped services:

``` powershell
Get-Service |
    Where-Object Status -eq 'Stopped' |
    Measure-Object
```

Record your results:

``` text
Total services:   ______________________
Running services: ______________________
Stopped services: ______________________
```

Your counts may differ from another computer. That is expected.

------------------------------------------------------------------------

# Exercise 8 --- Inspect Before You Filter

Your task:

> Display only process names and IDs.

Pretend you do not remember the correct process ID property.

Start with:

``` powershell
Get-Process | Get-Member
```

Find the correct properties.

Then build the final pipeline.

``` powershell
# Your command:
```

### Lesson

When you do not know what property to use:

``` text
Inspect the objects first.
```

------------------------------------------------------------------------

# Exercise 9 --- Work with Files

Move to a folder containing files.

For example:

``` powershell
Set-Location $HOME
```

Run:

``` powershell
Get-ChildItem
```

Inspect:

``` powershell
Get-ChildItem | Get-Member
```

Now try:

``` powershell
Get-ChildItem |
    Sort-Object Name |
    Select-Object Name, LastWriteTime
```

If your environment supports `-File`:

``` powershell
Get-ChildItem -File |
    Sort-Object Length -Descending |
    Select-Object -First 10 Name, Length
```

### Question

What does the last pipeline attempt to show?

``` text
____________________________________________________
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 10 --- Measure Processes

Count the processes currently returned by PowerShell:

``` powershell
Get-Process |
    Measure-Object
```

Record the count:

``` text
Process count: ______________________
```

Now display the first ten processes after sorting by name:

``` powershell
Get-Process |
    Sort-Object Name |
    Select-Object -First 10 Name, Id
```

------------------------------------------------------------------------

# Exercise 11 --- Pipeline Troubleshooting

Suppose you want to filter an object but do not know which property to
use.

Use this troubleshooting pattern:

``` text
1. Run the first command.
2. Inspect its output.
3. Pipe it to Get-Member.
4. Find the property you need.
5. Add the next pipeline stage.
6. Check the output again.
7. Continue building.
```

Practice with:

``` powershell
Get-Process
```

Use `Get-Member` to find a property that appears to represent the
process ID.

Then use `Select-Object` to display:

``` text
Name
ID property
```

------------------------------------------------------------------------

# Exercise 12 --- Read a Pipeline

Without running it first, explain what you expect this pipeline to do:

``` powershell
Get-Service |
    Where-Object Status -eq 'Running' |
    Sort-Object DisplayName |
    Select-Object -First 10 Name, DisplayName, Status
```

Write your prediction:

``` text
____________________________________________________
____________________________________________________
____________________________________________________
```

Now run it.

Was your prediction correct?

``` text
Yes / No
```

If not, what was different?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# End-of-Lab Challenge --- IT Service Report

You have received this request:

> IT needs a quick console report showing the first 15 **running Windows
> services**, sorted alphabetically by display name. The report should
> show only the service name, display name, and status.

Do not copy an earlier pipeline directly.

Build it one stage at a time.

## Requirements

Your final pipeline must use:

``` text
Get-Service
Where-Object
Sort-Object
Select-Object
```

It must:

1.  Retrieve services.
2.  Keep only running services.
3.  Sort by `DisplayName`.
4.  Return the first 15.
5.  Display:
    -   `Name`
    -   `DisplayName`
    -   `Status`

Write your final pipeline:

``` powershell
# Your solution:
```

------------------------------------------------------------------------

# Bonus Challenge --- Answer a Question with the Pipeline

Without manually counting rows, determine:

> How many Windows services are currently running on this computer?

Your solution should use:

``` text
Get-Service
Where-Object
Measure-Object
```

``` powershell
# Your solution:
```

Now determine how many are stopped.

``` powershell
# Your solution:
```

------------------------------------------------------------------------

# Final Reflection --- Lessons 01--05

You now have enough PowerShell knowledge to solve a basic task without
being handed the final command.

For an unfamiliar task, the workflow should begin to look like:

``` text
What do I need?
      ↓
Find the command
      ↓
Read its Help
      ↓
Run it safely
      ↓
Inspect its objects
      ↓
Choose useful properties
      ↓
Filter / Sort / Measure
      ↓
Build the pipeline
```

### Reflection Questions

1.  Which command do you use to discover PowerShell commands?

``` text
________________________________________
```

2.  Which command do you use to learn how a command works?

``` text
________________________________________
```

3.  Which command helps reveal an object's properties and methods?

``` text
________________________________________
```

4.  What character creates a PowerShell pipeline?

``` text
________________________________________
```

5.  Why are objects important to the pipeline?

``` text
____________________________________________________
____________________________________________________
```

------------------------------------------------------------------------

# Knowledge Check

1.  What does the pipeline operator do?

    A. Converts every object to text.\
    B. Sends output objects from one command to another.\
    C. Runs PowerShell as administrator.\
    D. Separates command parameters.

2.  Which command filters pipeline objects?

    A. `Where-Object`\
    B. `Get-Member`\
    C. `Measure-Object`\
    D. `Get-Command`

3.  Which command is used to sort objects?

    A. `Order-Object`\
    B. `Arrange-Object`\
    C. `Sort-Object`\
    D. `Select-Object`

4.  Which command can count objects moving through a pipeline?

    A. `Count-Object`\
    B. `Measure-Object`\
    C. `Get-Count`\
    D. `Select-Object`

5.  What should you do when you do not know which property to filter or
    sort by?

    A. Guess repeatedly.\
    B. Convert the output to text.\
    C. Inspect the objects with `Get-Member`.\
    D. Restart PowerShell.

------------------------------------------------------------------------

# Lab Complete

You have completed the foundational PowerShell labs for Lessons 01--05.

At this point, you should be beginning to think in terms of:

``` text
Commands → Objects → Pipeline
```

rather than memorizing isolated command syntax.

Continue to:

[Lesson 06 — Filtering and Sorting](../lessons/lesson-06--filtering-and-sorting.md)

