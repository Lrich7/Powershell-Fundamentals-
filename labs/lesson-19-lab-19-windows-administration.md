# Lab 19 --- Windows Administration

## Lab Objective

This lab shifts from learning individual PowerShell features to using
them for a realistic Windows support task.

You will collect:

-   Computer and OS information.
-   Manufacturer, model, and serial information.
-   Memory information.
-   Disk information.
-   Service and process information.
-   Network configuration.
-   Event log information.
-   Hotfix information where available.

Most of the lab is intentionally **read-only**.

------------------------------------------------------------------------

# Scenario

> **IT Ticket --- Slow Computer / Low Storage**
>
> A user reports that their Windows computer has been running slowly and
> may be low on storage.
>
> Use PowerShell to collect useful diagnostic information. Do not make
> configuration changes.

------------------------------------------------------------------------

# Exercise 1 --- Computer Information

Start with:

``` powershell
Get-ComputerInfo
```

Instead of keeping all output, identify useful properties for:

``` text
Computer name
Windows product
Windows version
Architecture
```

Build a clean `Select-Object` command.

``` powershell
# Your command:
```

------------------------------------------------------------------------

# Exercise 2 --- Operating System with CIM

Run:

``` powershell
Get-CimInstance Win32_OperatingSystem
```

Build a report showing:

``` text
Caption
Version
BuildNumber
LastBootUpTime
```

``` powershell
# Your command:
```

------------------------------------------------------------------------

# Exercise 3 --- Hardware Information

Use:

``` powershell
Get-CimInstance Win32_ComputerSystem
```

Retrieve:

``` text
Name
Manufacturer
Model
TotalPhysicalMemory
```

Then use:

``` powershell
Get-CimInstance Win32_BIOS
```

to retrieve the serial number.

Record:

``` text
Manufacturer: __________________________
Model:        __________________________
Serial:       __________________________
```

------------------------------------------------------------------------

# Exercise 4 --- Disk Space

Discover the appropriate CIM information for logical disks using the
lesson examples or `Get-Help`.

Build a report containing:

``` text
Drive
Total size
Free space
```

### Bonus

Use calculated properties to display size values in GB.

``` powershell
# Your command:
```

Which drive has the least free space?

``` text
________________________________________
```

------------------------------------------------------------------------

# Exercise 5 --- Processes

Find the 10 processes with the highest memory usage.

Use object discovery if you do not remember the relevant property.

Display:

``` text
Name
Id
Memory property
```

``` powershell
# Your solution:
```

------------------------------------------------------------------------

# Exercise 6 --- Services

Retrieve services.

Determine:

``` text
How many are Running?
How many are Stopped?
```

``` powershell
# Your commands:
```

Then inspect the `Spooler` service.

> Do not change its state for this ticket.

------------------------------------------------------------------------

# Exercise 7 --- Network Configuration

Use the network commands introduced in the lesson to collect useful
configuration.

Identify:

``` text
Interface
IPv4 address
Default gateway
DNS information
```

The exact output may vary by Windows version and network configuration.

Write the command(s) you used:

``` powershell
# Your commands:
```

------------------------------------------------------------------------

# Exercise 8 --- Event Logs

Retrieve recent System log errors using the event-log technique from the
lesson.

Limit the result so you do not return thousands of events.

Display useful fields such as:

``` text
TimeCreated
Id
ProviderName
LevelDisplayName
Message
```

``` powershell
# Your command:
```

Do not assume every error explains the user's problem. Event logs
require interpretation.

------------------------------------------------------------------------

# Exercise 9 --- Hotfix Information

Where supported:

``` powershell
Get-HotFix
```

Sort newest first and display a small set of results.

``` powershell
# Your command:
```

If the command is unavailable or incomplete on your system, record that
instead of treating it as a lab failure.

------------------------------------------------------------------------

# End-of-Lab Project --- Windows Health Report

Create:

``` text
windows-health-report.ps1
```

The script should gather and display or export:

``` text
Computer name
Manufacturer
Model
Serial number
Windows version
Last boot
Total memory
Disk free space
Top 10 memory-consuming processes
Running/stopped service counts
Network configuration
Recent System errors
Hotfix information where available
```

Requirements:

``` text
[ ] Read-only
[ ] Uses functions
[ ] Uses CIM
[ ] Uses pipelines
[ ] Uses custom objects where useful
[ ] Uses error handling
[ ] Uses clear section headings or structured output
[ ] Can export useful results
```

### Challenge Rule

Do not copy every command directly from the lesson. Use:

``` text
Get-Command
Get-Help
Get-Member
```

when you get stuck.

That is part of the lab.

------------------------------------------------------------------------

# Knowledge Check

1.  What does CIM provide?

    A. Structured management information B. Only text files C. Microsoft
    365 licenses D. Git repositories

2.  Which approach should normally come first in administration?

    A. Change configuration immediately B. Discover and verify system
    state C. Restart the computer D. Delete logs

3.  Why use `Select-Object` with large system-information commands?

    A. To focus the report on useful properties B. To modify Windows C.
    To install CIM D. To create users

------------------------------------------------------------------------

# Lab Complete

Continue to:

[Lesson 20 — Active Directory](../lessons/lesson-20-active-directory.md)

