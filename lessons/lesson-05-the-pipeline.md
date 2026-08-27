[lesson-05-the-pipeline.md](https://github.com/user-attachments/files/31517042/lesson-05-the-pipeline.md)

# Lesson 05 --- The Pipeline

## Learning Objectives

By the end of this lesson, you will be able to:

-   Explain what the PowerShell pipeline is and why it is useful.
-   Use the pipeline operator (`|`) to send objects from one command to
    another.
-   Explain how PowerShell passes objects through the pipeline instead
    of only formatted text.
-   Use `Get-Member` to inspect objects moving through the pipeline.
-   Use `Where-Object` to filter pipeline objects.
-   Use `Select-Object` to choose properties and limit results.
-   Use `Sort-Object` to organize pipeline output.
-   Use `Measure-Object` to count and summarize objects.
-   Recognize basic pipeline parameter binding.
-   Build readable multi-stage PowerShell pipelines.

------------------------------------------------------------------------

## What Is the PowerShell Pipeline?

The **pipeline** is one of the most important features in PowerShell.

The pipeline allows you to take the output from one command and send it
directly to another command.

The pipeline operator is the vertical bar:

``` text
|
```

For example:

``` powershell
Get-Service | Get-Member
```

This can be read as:

> Get the services and send those service objects to `Get-Member`.

Conceptually:

``` text
Get-Service
     │
     ▼
Service Objects
     │
     ▼
Get-Member
```

The output from the first command becomes input for the next command.

> **Key Idea:** PowerShell pipelines pass objects between commands,
> allowing small commands to be combined into larger tasks.

------------------------------------------------------------------------

# Objects Through the Pipeline

In the previous lesson, you learned that PowerShell commands normally
return **objects**.

This is what makes the PowerShell pipeline especially powerful.

Consider:

``` powershell
Get-Service | Where-Object Status -eq 'Running'
```

`Get-Service` returns service objects.

Those objects contain properties such as:

``` text
Name
DisplayName
Status
StartType
```

PowerShell sends those objects through the pipeline to `Where-Object`.

`Where-Object` can then inspect the actual `Status` property of each
object.

Conceptually:

``` text
Get-Service
     │
     ▼
Service Objects
     │
     ▼
Where-Object
Check Status Property
     │
     ▼
Running Service Objects
```

PowerShell is not simply searching formatted text on the screen.

It is working with structured objects.

------------------------------------------------------------------------

# The Pipeline Operator

The pipeline operator is:

``` text
|
```

On most keyboards, it shares a key with the backslash:

``` text
\
```

You typically type the pipe character using:

``` text
Shift + \
```

A basic pipeline looks like:

``` powershell
Command-1 | Command-2
```

The general idea is:

``` text
Output from Command 1
        ↓
Input to Command 2
```

You can continue adding commands:

``` powershell
Command-1 | Command-2 | Command-3
```

Each command performs another operation on the objects moving through
the pipeline.

------------------------------------------------------------------------

# A Simple Pipeline

Start with:

``` powershell
Get-Service
```

This returns service objects.

Now send those objects to `Select-Object`:

``` powershell
Get-Service | Select-Object Name, Status
```

The first command retrieves the services.

The second command selects only the properties you want to see.

``` text
Get-Service
     │
     ▼
All Service Objects
     │
     ▼
Select-Object
Name, Status
     │
     ▼
Final Output
```

------------------------------------------------------------------------

# Inspecting Pipeline Objects

When building a pipeline, it is important to know what type of object is
moving through it.

Use:

``` powershell
Get-Member
```

For example:

``` powershell
Get-Service | Get-Member
```

Or:

``` powershell
Get-Process | Get-Member
```

This helps answer:

> What type of object did the previous command return?

and:

> What properties and methods are available?

This is especially useful when you are not sure which property to
filter, sort, or select.

------------------------------------------------------------------------

# Filtering with Where-Object

`Where-Object` filters objects moving through the pipeline.

For example:

``` powershell
Get-Service | Where-Object Status -eq 'Running'
```

This returns only services whose `Status` property equals `Running`.

Another example:

``` powershell
Get-Service | Where-Object Status -eq 'Stopped'
```

The pipeline becomes:

``` text
Get-Service
     │
     ▼
All Services
     │
     ▼
Where-Object
Status = Stopped
     │
     ▼
Stopped Services
```

------------------------------------------------------------------------

# Using \$\_ in the Pipeline

You may also see `Where-Object` written using a script block:

``` powershell
Get-Service | Where-Object { $_.Status -eq 'Running' }
```

Inside the script block:

``` powershell
$_
```

represents the **current object in the pipeline**.

Therefore:

``` powershell
$_.Status
```

means:

> Access the `Status` property of the current object.

The condition:

``` powershell
$_.Status -eq 'Running'
```

means:

> Keep this object if its Status property equals Running.

------------------------------------------------------------------------

# Filtering with Comparison Operators

PowerShell includes several comparison operators that are commonly used
with `Where-Object`.

``` text
-eq    Equal to
-ne    Not equal to
-gt    Greater than
-ge    Greater than or equal to
-lt    Less than
-le    Less than or equal to
-like  Matches a wildcard pattern
```

For example:

``` powershell
Get-Process | Where-Object CPU -gt 10
```

This returns processes whose `CPU` property is greater than `10`.

Another example:

``` powershell
Get-Service | Where-Object Name -like 'Win*'
```

The `*` wildcard allows the command to match service names beginning
with `Win`.

------------------------------------------------------------------------

# Selecting Properties with Select-Object

`Select-Object` allows you to control which properties appear in your
results.

For example:

``` powershell
Get-Service | Select-Object Name, Status
```

You can select several properties:

``` powershell
Get-Service | Select-Object Name, DisplayName, Status
```

For processes:

``` powershell
Get-Process | Select-Object Name, Id, CPU
```

This is useful because PowerShell objects often contain many more
properties than are displayed by default.

------------------------------------------------------------------------

# Limiting Results with Select-Object

`Select-Object` can also limit the number of objects that continue
through the pipeline.

For example:

``` powershell
Get-Process | Select-Object -First 5
```

This returns the first five objects.

You can also use:

``` powershell
Get-Process | Select-Object -Last 5
```

A common pattern is:

``` powershell
Get-Process | Select-Object -First 5 Name, Id
```

------------------------------------------------------------------------

# Sorting Objects with Sort-Object

`Sort-Object` sorts objects based on one or more properties.

For example:

``` powershell
Get-Service | Sort-Object Status
```

This sorts services by their `Status` property.

You can sort processes by name:

``` powershell
Get-Process | Sort-Object Name
```

Or sort a numeric property:

``` powershell
Get-Process | Sort-Object CPU
```

------------------------------------------------------------------------

## Sorting in Descending Order

By default, `Sort-Object` sorts in ascending order.

Use `-Descending` to reverse the order.

For example:

``` powershell
Get-Process | Sort-Object CPU -Descending
```

This can be useful when looking for processes using the most CPU time.

You can combine this with `Select-Object`:

``` powershell
Get-Process |
    Sort-Object CPU -Descending |
    Select-Object -First 10 Name, Id, CPU
```

This gives you the first ten processes after sorting by CPU in
descending order.

------------------------------------------------------------------------

# Measuring Objects with Measure-Object

`Measure-Object` can count objects moving through the pipeline.

For example:

``` powershell
Get-Service | Measure-Object
```

Look at the:

``` text
Count
```

property.

This tells you how many service objects were passed to `Measure-Object`.

You can combine filtering and measuring:

``` powershell
Get-Service |
    Where-Object Status -eq 'Running' |
    Measure-Object
```

This counts the running services.

------------------------------------------------------------------------

# Building Multi-Stage Pipelines

The real power of the pipeline becomes clear when several commands are
combined.

For example:

``` powershell
Get-Service |
    Where-Object Status -eq 'Running' |
    Sort-Object Name |
    Select-Object Name, DisplayName, Status
```

Each command has a specific job.

``` text
Get-Service
     │
     ▼
Retrieve Service Objects
     │
     ▼
Where-Object
Keep Running Services
     │
     ▼
Sort-Object
Sort by Name
     │
     ▼
Select-Object
Choose Properties
     │
     ▼
Final Output
```

Instead of needing one giant command that does everything, PowerShell
allows you to combine smaller commands.

> **Key Idea:** A PowerShell pipeline is often a series of simple
> commands, with each command performing one part of the task.

------------------------------------------------------------------------

# Order Matters

The order of commands in a pipeline can affect the result.

Consider:

``` powershell
Get-Process |
    Sort-Object CPU -Descending |
    Select-Object -First 5 Name, CPU
```

This means:

1.  Get all processes.
2.  Sort all processes by CPU.
3.  Keep the first five.

This can identify high-CPU processes.

Now consider:

``` powershell
Get-Process |
    Select-Object -First 5 Name, CPU |
    Sort-Object CPU -Descending
```

This means:

1.  Get all processes.
2.  Keep the first five.
3.  Sort only those five.

These pipelines do **not** necessarily produce the same result.

> **Important:** Think about what each command is doing to the objects
> before they move to the next stage.

------------------------------------------------------------------------

# Pipeline Input

For a pipeline to work, the next command must be able to accept the
objects being passed to it.

PowerShell uses **parameter binding** to determine where pipeline input
belongs.

You do not need to understand all of the parameter-binding rules yet.

For now, remember:

> Not every command can accept every type of object from the pipeline.

You can use Help to investigate whether a parameter accepts pipeline
input.

For example:

``` powershell
Get-Help Stop-Service -Full
```

In the parameter information, you may see whether a parameter accepts
pipeline input.

This is another reason `Get-Help` is important even after you become
familiar with PowerShell.

------------------------------------------------------------------------

# Pipeline by Value and Property Name

PowerShell can bind pipeline input in different ways.

Two important concepts you will encounter are:

``` text
ByValue
ByPropertyName
```

## ByValue

PowerShell attempts to pass the entire object to a compatible parameter.

Conceptually:

``` text
Object
  ↓
Compatible Parameter
```

## ByPropertyName

PowerShell may also match properties on the incoming object with
compatible parameter names.

Conceptually:

``` text
Incoming Object
├── Name
├── Status
└── DisplayName
       │
       ▼
Parameter with matching name
```

You do not need to master these rules yet.

At this stage, the goal is simply to understand **why some commands work
together naturally in a pipeline while others do not**.

------------------------------------------------------------------------

# Formatting and the Pipeline

PowerShell includes formatting commands such as:

``` powershell
Format-Table
Format-List
```

For example:

``` powershell
Get-Service | Format-Table Name, Status
```

or:

``` powershell
Get-Service | Format-List Name, DisplayName, Status
```

These commands control how output is displayed.

However, formatting commands should normally be placed at the **end** of
a pipeline.

For example:

``` powershell
Get-Service |
    Where-Object Status -eq 'Running' |
    Sort-Object Name |
    Format-Table Name, Status
```

Why?

Because formatting commands prepare objects for display rather than
normal object processing.

A good beginner rule is:

> **Filter, sort, and select first. Format last.**

------------------------------------------------------------------------

# Writing Readable Pipelines

Short pipelines can fit comfortably on one line:

``` powershell
Get-Service | Where-Object Status -eq 'Running'
```

Longer pipelines are often easier to read when written across multiple
lines.

PowerShell allows a line break after the pipeline operator:

``` powershell
Get-Service |
    Where-Object Status -eq 'Running' |
    Sort-Object Name |
    Select-Object Name, DisplayName, Status
```

This makes it easier to see each stage of the pipeline.

For longer scripts, readability is important.

------------------------------------------------------------------------

# A Practical Pipeline Workflow

Suppose you want a list of running Windows services sorted by name.

## Step 1 --- Get the Objects

``` powershell
Get-Service
```

## Step 2 --- Inspect Them if Needed

``` powershell
Get-Service | Get-Member
```

## Step 3 --- Filter the Objects

``` powershell
Get-Service |
    Where-Object Status -eq 'Running'
```

## Step 4 --- Sort the Results

``` powershell
Get-Service |
    Where-Object Status -eq 'Running' |
    Sort-Object Name
```

## Step 5 --- Select the Properties

``` powershell
Get-Service |
    Where-Object Status -eq 'Running' |
    Sort-Object Name |
    Select-Object Name, DisplayName, Status
```

You built the pipeline one stage at a time.

This is often much easier than trying to write the entire command at
once.

------------------------------------------------------------------------

# Build Pipelines One Step at a Time

When learning PowerShell, avoid trying to create a complicated pipeline
immediately.

Start with:

``` powershell
Get-Service
```

Make sure it works.

Then add:

``` powershell
Get-Service |
    Where-Object Status -eq 'Running'
```

Check the result.

Then add another stage:

``` powershell
Get-Service |
    Where-Object Status -eq 'Running' |
    Sort-Object Name
```

Finally:

``` powershell
Get-Service |
    Where-Object Status -eq 'Running' |
    Sort-Object Name |
    Select-Object Name, Status
```

> **Tip:** Build and test pipelines from left to right, one command at a
> time.

This makes troubleshooting much easier.

------------------------------------------------------------------------

# Putting the Discovery Skills Together

The skills from the previous lessons now combine into a powerful
workflow.

``` text
What command do I need?
        ↓
Get-Command

How does the command work?
        ↓
Get-Help

What objects does it return?
        ↓
Get-Member

Which objects do I need?
        ↓
Where-Object

How should they be organized?
        ↓
Sort-Object

Which information do I need?
        ↓
Select-Object

How many objects are there?
        ↓
Measure-Object
```

A practical pipeline might look like:

``` powershell
Get-Process |
    Where-Object CPU -gt 10 |
    Sort-Object CPU -Descending |
    Select-Object Name, Id, CPU
```

You do not need to memorize every possible pipeline combination.

Learn how the pieces work, then combine them as needed.

------------------------------------------------------------------------

# Key Takeaways

-   The pipeline operator is `|`.
-   The pipeline sends output from one command to another.
-   PowerShell normally passes **objects**, not just formatted text.
-   `Get-Member` helps identify the objects and properties available in
    a pipeline.
-   `Where-Object` filters objects.
-   `Select-Object` chooses properties or limits results.
-   `Sort-Object` sorts objects by their properties.
-   `Measure-Object` can count and summarize pipeline objects.
-   `$_` represents the current pipeline object in many script blocks.
-   Pipeline order matters.
-   PowerShell uses parameter binding to connect pipeline objects to
    command parameters.
-   Formatting commands such as `Format-Table` should generally come at
    the end of a pipeline.
-   Complex pipelines are easier to understand when built and tested one
    stage at a time.
-   PowerShell's pipeline allows simple commands to be combined into
    powerful administrative workflows.

------------------------------------------------------------------------

# Lab

Ready to practice building PowerShell pipelines?

Continue to:

[Lab 05 --- The Pipeline](../labs/lab-05-the-pipeline.md)

In the lab, you will build pipelines one stage at a time, inspect
pipeline objects, filter and sort results, select properties, count
objects, and troubleshoot pipelines that do not produce the expected
result.

------------------------------------------------------------------------



# 🚀 Project Checkpoint

You have completed Lessons 01–05. Now put those skills together in your first independent PowerShell project.

### [Project 01 — PowerShell System Explorer](../projects/project-01-powershell-system-explorer.md)

Use command discovery, PowerShell Help, objects, and the pipeline to build a useful system exploration tool.

> **Tip:** Unlike the labs, projects provide less step-by-step guidance. Use `Get-Command`, `Get-Help`, and `Get-Member` when you get stuck.
> 
------------------------------------------------------------------------

## Additional Resources

-   [About Pipelines --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_pipelines?view=powershell-7.6)
-   [About Pipeline Chain Operators --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_pipeline_chain_operators?view=powershell-7.6)
-   [Where-Object --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/where-object?view=powershell-7.6)
-   [Select-Object --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/select-object?view=powershell-7.6)
-   [Sort-Object --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/sort-object?view=powershell-7.6)
-   [Measure-Object --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/measure-object?view=powershell-7.6)
