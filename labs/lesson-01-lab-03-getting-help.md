[lab-03-getting-help.md](https://github.com/user-attachments/files/31521121/lab-03-getting-help.md)
# Lab 03 --- Getting Help

## Lab Objective

In this lab, you will practice using PowerShell's built-in Help system
to learn unfamiliar commands.

You will:

-   Use basic `Get-Help`.
-   Read synopsis, syntax, description, parameters, and examples.
-   Use `-Examples`, `-Detailed`, and `-Full`.
-   Request help for a specific parameter.
-   Search Help when you do not know the exact topic.
-   Explore `about_*` conceptual Help.
-   Understand `Update-Help` and online Help.
-   Solve tasks by using documentation rather than memorization.

------------------------------------------------------------------------

## Before You Begin

Complete Lessons 01--03.

> **Note:** Local Help content varies by PowerShell version and
> computer. If your Help appears incomplete, that is part of what this
> lab teaches you to recognize.

------------------------------------------------------------------------

# Exercise 1 --- Basic Help

Run:

``` powershell
Get-Help Get-Service
```

Locate the following sections if they are available:

``` text
NAME
SYNOPSIS
SYNTAX
DESCRIPTION
RELATED LINKS
```

### Question

Using the `SYNOPSIS` or `DESCRIPTION`, explain what `Get-Service` does
in your own words.

``` text
____________________________________________________
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 2 --- Read Syntax

Run:

``` powershell
Get-Help Get-Service
```

Look at the `SYNTAX` section.

Do not worry about understanding every bracket or symbol.

Find the parameter:

``` text
-Name
```

### Question

Based on the syntax, does `Get-Service` support a `-Name` parameter?

``` text
Answer: ______________________
```

Now verify by running:

``` powershell
Get-Service -Name Spooler
```

------------------------------------------------------------------------

# Exercise 3 --- Examples

Run:

``` powershell
Get-Help Get-Service -Examples
```

Read at least two examples.

Then:

``` powershell
Get-Help Get-Process -Examples
```

### Question

Why can examples be more useful than reading only the syntax?

``` text
____________________________________________________
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 4 --- Detailed Help

Run:

``` powershell
Get-Help Get-Service -Detailed
```

Compare it with:

``` powershell
Get-Help Get-Service
```

### Question

What additional information did you notice?

``` text
____________________________________________________
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 5 --- Full Help

Run:

``` powershell
Get-Help Get-Service -Full
```

This output can be long.

Do not memorize it.

Your goal is to become comfortable locating information.

Find information about:

``` text
-Name
-DisplayName
```

Record one fact about each:

``` text
-Name:
____________________________________________________

-DisplayName:
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 6 --- Parameter-Specific Help

Instead of reading the entire Help page, ask PowerShell about one
parameter.

Try:

``` powershell
Get-Help Get-Service -Parameter Name
```

Then:

``` powershell
Get-Help Get-Process -Parameter Name
```

### Question

When would parameter-specific Help be useful?

``` text
____________________________________________________
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 7 --- Search Help

Suppose you want information about PowerShell services but do not know
exactly which Help topic to open.

Try:

``` powershell
Get-Help *Service*
```

Review the results.

Now search:

``` powershell
Get-Help *Process*
```

### Challenge

Search Help for topics related to:

``` text
wildcards
```

Write the command you used:

``` powershell
# Your command:
```

------------------------------------------------------------------------

# Exercise 8 --- about\_\* Topics

PowerShell includes conceptual Help topics whose names begin with:

``` text
about_
```

Try:

``` powershell
Get-Help about_Wildcards
```

If available, read the beginning of the topic.

Then search for conceptual topics:

``` powershell
Get-Help about_*
```

Find one topic that looks interesting or useful.

``` text
Topic: __________________________________
```

Explain what it appears to teach:

``` text
____________________________________________________
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 9 --- Is Your Help Current?

PowerShell can download updated Help for modules that support updatable
Help.

The command is:

``` powershell
Update-Help
```

For this training lab, you do **not** need to run it if:

-   You are on a managed company computer.
-   You do not have appropriate permissions.
-   Your environment does not allow it.

Instead, view its Help:

``` powershell
Get-Help Update-Help
```

### Question

What is the purpose of `Update-Help`?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 10 --- Online Help

Many Help topics can open their current online documentation using:

``` powershell
Get-Help Get-Service -Online
```

This may open a browser.

If you do not want to open a browser, simply inspect:

``` powershell
Get-Help Get-Service -Full
```

and locate the related online documentation information.

### Think About It

When might online Help be preferable to local Help?

``` text
____________________________________________________
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 11 --- Use Help to Solve a Task

You know:

``` powershell
Get-ChildItem
```

lists items in a location.

Your task is:

> Determine which parameter tells `Get-ChildItem` to search child
> directories recursively.

Do not use the lesson.

Use PowerShell Help.

Possible starting points:

``` powershell
Get-Help Get-ChildItem
```

or:

``` powershell
Get-Help Get-ChildItem -Full
```

Record the parameter:

``` text
Answer: ______________________
```

Now run the command using that parameter in a small folder.

If the output becomes too large:

``` text
Ctrl+C
```

will stop the running command.

------------------------------------------------------------------------

# Exercise 12 --- Find → Help → Example → Run

Your task:

> Retrieve information about the `Spooler` Windows service.

You must use the complete workflow.

### Find

Use `Get-Command` to identify the service retrieval command.

``` powershell
# Your discovery command:
```

### Help

Read its Help:

``` powershell
# Your help command:
```

### Example

Display only examples:

``` powershell
# Your examples command:
```

### Run

Use what you learned to retrieve the `Spooler` service.

``` powershell
# Your final command:
```

------------------------------------------------------------------------

# End-of-Lab Challenge --- Help Desk Without Google

Pretend you have no internet access.

Use only:

``` text
Get-Command
Get-Help
```

and commands you discover.

## Task 1

Find a command that lists running processes.

Then find the parameter used to request a process by name.

``` text
Command:   ______________________
Parameter: ______________________
```

## Task 2

Find the PowerShell command that clears the console.

Display its Help.

``` text
Command: ______________________
```

## Task 3

Find the conceptual Help topic for wildcards.

``` text
Topic: ______________________
```

## Task 4

Find a command related to command history.

Read its examples.

``` text
Command: ______________________
```

## Task 5

Choose one PowerShell command you have **not** studied yet.

Use Help to answer:

``` text
Command:
________________________________________

What does it do?
________________________________________
________________________________________

One parameter:
________________________________________

One example of how it can be used:
________________________________________
________________________________________
```

Do not run it if you are unsure whether it changes the system.

------------------------------------------------------------------------

# Knowledge Check

1.  Which option shows command examples?

    A. `-Full`\
    B. `-Examples`\
    C. `-SyntaxOnly`\
    D. `-Demo`

2.  Which command requests Help for only the `Name` parameter of
    `Get-Service`?

    A. `Get-Help Get-Service -Name`\
    B. `Get-Service -Help Name`\
    C. `Get-Help Get-Service -Parameter Name`\
    D. `Help-Parameter Get-Service Name`

3.  What are `about_*` topics primarily used for?

    A. Starting services\
    B. Explaining PowerShell concepts and language features\
    C. Installing Windows updates\
    D. Creating aliases

4.  What does `Update-Help` do?

    A. Updates PowerShell itself\
    B. Downloads updated Help content for supported modules\
    C. Updates Windows\
    D. Updates your scripts

5.  What should you do when you find an unfamiliar command capable of
    changing the system?

    A. Run it and see what happens.\
    B. Read its Help and understand it before running it.\
    C. Add `Get-` to its name.\
    D. Run it as administrator.

------------------------------------------------------------------------

# Lab Complete

You have practiced using PowerShell itself as a learning tool.

Next, you will inspect what PowerShell commands actually return:

> **Objects.**

Continue to:

[Lab 04 --- Working with Objects](lab-04-working-with-objects.md)
