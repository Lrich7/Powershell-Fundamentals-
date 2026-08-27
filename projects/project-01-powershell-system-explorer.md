# Project 01 --- PowerShell System Explorer

## Project Checkpoint

**Recommended after:** Lessons 01--05

This is your first project. Unlike a lab, you will receive requirements
rather than every command you need.

Use the skills from Lessons 01--05 and PowerShell's discovery tools to
solve the problem.

------------------------------------------------------------------------

## Scenario

A new IT technician needs a quick way to explore a Windows computer
without clicking through several graphical tools.

Build a PowerShell script that collects and displays useful information
about the local computer.

------------------------------------------------------------------------

## Skills Practiced

-   Finding commands
-   `Get-Help`
-   PowerShell objects
-   `Get-Member`
-   `Select-Object`
-   The pipeline
-   Basic command organization

------------------------------------------------------------------------

## Requirements

Create:

``` text
system-explorer.ps1
```

Your script must display useful information from at least **three** of
these categories:

``` text
Processes
Services
Computer / operating system information
Network information
Disk information
```

For each category:

-   Use PowerShell commands that return objects.
-   Select only useful properties.
-   Sort or filter at least one result.
-   Keep the output readable.

Your script must also include comments explaining the major sections.

------------------------------------------------------------------------

## Discovery Rules

Do not look for a complete finished script first.

Use:

``` powershell
Get-Command
Get-Help
Get-Member
```

when you do not know how to retrieve something.

Examples of questions you should be able to solve:

``` text
How do I find commands related to services?
What properties does this command return?
Which property should I sort on?
How do I display only the fields I care about?
```

------------------------------------------------------------------------

## Minimum Deliverables

``` text
[ ] system-explorer.ps1
[ ] At least 3 information categories
[ ] Pipeline usage
[ ] Select-Object
[ ] At least one filter or sort
[ ] Comments
[ ] Script runs without modifying the computer
```

------------------------------------------------------------------------

## Suggested Output

Your exact output does not need to match this:

``` text
=== COMPUTER ===
Computer: PC-001
Windows: ...

=== DISKS ===
Drive   FreeSpace
-----   ---------

=== SERVICES ===
Name             Status
----             ------
...
```

Focus on useful data rather than decorative formatting.

------------------------------------------------------------------------

## Stretch Challenges

Try one or more:

-   Display only running services.
-   Show the top 10 processes by a useful property.
-   Display only active network information.
-   Add the current date/time to the report.
-   Export one section to CSV.

------------------------------------------------------------------------

## Reflection

Which command did you have to discover yourself?

``` text
____________________________________________________
```

Which PowerShell object property was most useful?

``` text
____________________________________________________
```

What would you add in version 2?

``` text
____________________________________________________
```

------------------------------------------------------------------------

## Project Complete

Continue with Lessons 06--10 before beginning Project 02.
