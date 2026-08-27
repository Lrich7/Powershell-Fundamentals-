[project-04-windows-health-check.md](https://github.com/user-attachments/files/31522104/project-04-windows-health-check.md)
# Project 04 --- Windows Health Check Tool

## Project Checkpoint

**Recommended after:** Lessons 16--19

This project should feel more like a real IT support request. You are
expected to discover some commands and properties yourself.

------------------------------------------------------------------------

## Scenario

A user reports:

> My computer has been slow lately and I think something may be wrong
> with it.

Build a PowerShell tool that gathers a useful diagnostic snapshot
without changing the computer.

------------------------------------------------------------------------

## Skills Practiced

-   Error handling
-   Modules
-   Windows administration
-   CIM
-   Functions
-   Structured objects
-   Scripts
-   Reporting
-   Optional remoting

------------------------------------------------------------------------

## Requirements

Create:

``` text
windows-health-check.ps1
```

The script must collect useful information about:

``` text
Computer / OS
Manufacturer and model
Serial number
Uptime / last boot
Memory
Disk space
Processes
Services
Network configuration
Recent system errors
```

Where appropriate, use CIM and PowerShell objects rather than parsing
formatted text.

------------------------------------------------------------------------

## Report Design

Do not dump every property from every command.

Choose the properties that would actually help an IT technician.

For example, ask yourself:

``` text
Do I need every process property?
Do I need every event from the System log?
Do I need every network adapter?
```

Your report should be useful, not merely large.

------------------------------------------------------------------------

## Functions

Use at least **three functions**.

Possible responsibilities:

``` text
Get-SystemSummary
Get-DiskHealth
Get-ProcessSummary
Get-ServiceSummary
Get-NetworkSummary
Get-RecentSystemError
```

Choose your own design.

------------------------------------------------------------------------

## Error Handling

At least two operations must have meaningful error handling.

Your script should:

-   Avoid empty `catch` blocks.
-   Give useful failure information.
-   Continue when a noncritical section fails where appropriate.
-   Stop when continuing would make the report invalid.

------------------------------------------------------------------------

## Disk Warning

Create a configurable disk-space warning.

For example, your script might accept:

``` text
-WarningPercent
```

or another sensible threshold.

Flag drives that fall below it.

Do not hard-code the result; calculate it from the disk data.

------------------------------------------------------------------------

## Output

Produce a useful report.

Choose one or more:

``` text
Console summary
CSV files
JSON
```

The output should include a timestamp and computer name.

------------------------------------------------------------------------

## Optional Remoting

If you have an **authorized remoting lab**, optionally allow:

``` text
-ComputerName
```

to collect information remotely.

Do not enable remoting or modify firewall/security settings merely to
complete the project.

A local-only version fully satisfies the project.

------------------------------------------------------------------------

## Minimum Deliverables

``` text
[ ] windows-health-check.ps1
[ ] At least 3 functions
[ ] Error handling
[ ] System information
[ ] Hardware information
[ ] Disk health
[ ] Process information
[ ] Service information
[ ] Network information
[ ] Recent event information
[ ] Configurable threshold
[ ] Structured output/report
[ ] Read-only behavior
```

------------------------------------------------------------------------

## Test Cases

Test at least:

  Test                                          Expected Result        Actual Result   Pass?
  --------------------------------------------- ---------------------- --------------- -------
  Normal local run                              Complete report                        
  Very high disk warning threshold              Disk warning appears                   
  One intentionally invalid/noncritical input   Useful error                           

------------------------------------------------------------------------

## Stretch Challenges

-   Add `-Verbose`.
-   Export separate report sections.
-   Package reporting functions into a module.
-   Add optional remote targets.
-   Add a simple health status such as `OK`, `Warning`, or `Review`.
-   Create an HTML report after researching `ConvertTo-Html`.

------------------------------------------------------------------------

## Reflection

Which failures should stop the script completely?

``` text
____________________________________________________
```

Which failures can be logged while the rest of the report continues?

``` text
____________________________________________________
```

What would you check manually after reviewing this report?

``` text
____________________________________________________
```

------------------------------------------------------------------------

## Project Complete

Continue with Lessons 20--22 before beginning the final project.
