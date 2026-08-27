[lesson-19-windows-administration.md](https://github.com/user-attachments/files/31518559/lesson-19-windows-administration.md)

# Lesson 19 --- Windows Administration with PowerShell

## Learning Objectives

By the end of this lesson, you will be able to:

-   Use PowerShell to collect common Windows system information.
-   Inspect services, processes, disks, event logs, and network
    configuration.
-   Understand the role of CIM in Windows administration.
-   Query Windows information with `Get-CimInstance`.
-   Start, stop, and restart services safely.
-   Inspect Windows event logs with PowerShell.
-   Review installed hotfix information where available.
-   Use PowerShell for repeatable Windows health checks.
-   Distinguish read-only discovery from configuration changes.
-   Build a basic Windows administration report.

------------------------------------------------------------------------

## PowerShell as a Windows Administration Tool

PowerShell was designed with system administration in mind.

Many Windows tasks that can be performed through graphical tools can
also be inspected or automated with PowerShell.

Examples include:

``` text
Services
Processes
Computer information
Operating system information
Disk space
Network configuration
Event logs
Hotfixes
Files and folders
Local configuration
```

> **Key Idea:** Use PowerShell first to discover and verify system
> state. Make configuration changes only after confirming the correct
> target and expected impact.

------------------------------------------------------------------------

# Computer Information

A useful starting command is:

``` powershell
Get-ComputerInfo
```

This can return a large amount of information.

Instead of displaying everything, select useful properties:

``` powershell
Get-ComputerInfo |
    Select-Object CsName,
        WindowsProductName,
        WindowsVersion,
        OsArchitecture
```

The exact properties available can vary by Windows and PowerShell
version.

Use:

``` powershell
Get-ComputerInfo | Get-Member
```

to inspect what your system provides.

------------------------------------------------------------------------

# Environment Information

PowerShell exposes useful environment variables.

Examples:

``` powershell
$env:COMPUTERNAME
$env:USERNAME
$env:USERDOMAIN
```

You can also inspect all environment variables:

``` powershell
Get-ChildItem Env:
```

This demonstrates that PowerShell providers can expose more than the
normal file system.

------------------------------------------------------------------------

# CIM

CIM stands for:

``` text
Common Information Model
```

PowerShell can use CIM to retrieve detailed management information from
Windows.

One of the primary commands is:

``` powershell
Get-CimInstance
```

For example:

``` powershell
Get-CimInstance Win32_OperatingSystem
```

------------------------------------------------------------------------

# Operating System Information

Use:

``` powershell
Get-CimInstance Win32_OperatingSystem |
    Select-Object Caption, Version, BuildNumber, LastBootUpTime
```

This can provide useful system information in a structured object.

------------------------------------------------------------------------

# Computer System Information

Use:

``` powershell
Get-CimInstance Win32_ComputerSystem |
    Select-Object Name, Manufacturer, Model, TotalPhysicalMemory
```

This is useful for inventory and troubleshooting.

------------------------------------------------------------------------

# BIOS Information

Use:

``` powershell
Get-CimInstance Win32_BIOS |
    Select-Object Manufacturer, SMBIOSBIOSVersion, SerialNumber
```

Depending on the hardware and manufacturer, this can provide useful
asset information.

------------------------------------------------------------------------

# Disk Information

A common administrative task is checking free disk space.

Use:

``` powershell
Get-CimInstance Win32_LogicalDisk -Filter "DriveType=3"
```

Select useful properties:

``` powershell
Get-CimInstance Win32_LogicalDisk -Filter "DriveType=3" |
    Select-Object DeviceID, Size, FreeSpace
```

Create readable GB values:

``` powershell
Get-CimInstance Win32_LogicalDisk -Filter "DriveType=3" |
    Select-Object DeviceID,
        @{
            Name = 'SizeGB'
            Expression = { [math]::Round($_.Size / 1GB, 2) }
        },
        @{
            Name = 'FreeGB'
            Expression = { [math]::Round($_.FreeSpace / 1GB, 2) }
        }
```

------------------------------------------------------------------------

# Services

You have already used:

``` powershell
Get-Service
```

Windows administration frequently requires checking service state.

Example:

``` powershell
Get-Service |
    Sort-Object Status, Name |
    Select-Object Name, DisplayName, Status
```

Find stopped services:

``` powershell
Get-Service |
    Where-Object Status -eq 'Stopped'
```

A stopped service is not automatically a problem. Many Windows services
are designed to start only when needed.

------------------------------------------------------------------------

# Starting and Stopping Services

PowerShell provides:

``` powershell
Start-Service
Stop-Service
Restart-Service
```

Example:

``` powershell
Restart-Service -Name Spooler -WhatIf
```

When supported, use `-WhatIf` before making changes.

> **Important:** Restarting or stopping a service can affect users and
> applications. Confirm the service and business impact first.

------------------------------------------------------------------------

# Processes

Use:

``` powershell
Get-Process
```

Find high-memory processes:

``` powershell
Get-Process |
    Sort-Object WorkingSet64 -Descending |
    Select-Object -First 10 Name,
        Id,
        @{
            Name = 'MemoryMB'
            Expression = { [math]::Round($_.WorkingSet64 / 1MB, 2) }
        }
```

You can also inspect CPU usage:

``` powershell
Get-Process |
    Sort-Object CPU -Descending |
    Select-Object -First 10 Name, Id, CPU
```

Remember that some process properties may require appropriate
permissions or may not contain a value in every situation.

------------------------------------------------------------------------

# Stopping Processes

PowerShell provides:

``` powershell
Stop-Process
```

Before stopping a process, identify it carefully:

``` powershell
Get-Process -Name notepad
```

Preview when supported:

``` powershell
Stop-Process -Name notepad -WhatIf
```

Never stop unfamiliar Windows or business-critical processes simply
because they appear to use resources.

------------------------------------------------------------------------

# Network Configuration

PowerShell includes Windows networking commands such as:

``` powershell
Get-NetIPConfiguration
```

This can display:

-   Network adapters
-   IP addresses
-   Default gateways
-   DNS information

Another useful command is:

``` powershell
Get-NetIPAddress
```

Filter IPv4 addresses:

``` powershell
Get-NetIPAddress -AddressFamily IPv4
```

------------------------------------------------------------------------

# Testing Network Connectivity

Use:

``` powershell
Test-Connection
```

Example:

``` powershell
Test-Connection Server01 -Count 2
```

For testing a TCP port, Windows PowerShell environments may provide:

``` powershell
Test-NetConnection Server01 -Port 443
```

This can help distinguish general network reachability from service-port
connectivity.

------------------------------------------------------------------------

# Event Logs

Windows event logs contain valuable troubleshooting information.

A primary PowerShell command is:

``` powershell
Get-WinEvent
```

Example:

``` powershell
Get-WinEvent -LogName System -MaxEvents 20
```

Select useful properties:

``` powershell
Get-WinEvent -LogName System -MaxEvents 20 |
    Select-Object TimeCreated, Id, LevelDisplayName, ProviderName, Message
```

------------------------------------------------------------------------

# Filtering Event Logs

Event logs can contain enormous numbers of records.

Filter as early as possible.

Example:

``` powershell
$startTime = (Get-Date).AddHours(-24)

Get-WinEvent -FilterHashtable @{
    LogName   = 'System'
    StartTime = $startTime
    Level     = 2
}
```

Event level `2` represents errors in common Windows event-log scenarios.

Using `-FilterHashtable` is often more efficient than retrieving an
entire log and filtering afterward.

------------------------------------------------------------------------

# Hotfix Information

On supported Windows systems, you may use:

``` powershell
Get-HotFix
```

Example:

``` powershell
Get-HotFix |
    Sort-Object InstalledOn -Descending |
    Select-Object -First 10 HotFixID, Description, InstalledOn
```

Be aware that `Get-HotFix` does not represent every possible update
mechanism or every installed component.

Use it as one source of information rather than assuming it is a
complete patch-management system.

------------------------------------------------------------------------

# Local Users and Groups

On supported Windows systems, the `Microsoft.PowerShell.LocalAccounts`
module provides commands such as:

``` powershell
Get-LocalUser
Get-LocalGroup
Get-LocalGroupMember
```

Example:

``` powershell
Get-LocalUser |
    Select-Object Name, Enabled, LastLogon
```

Inspect local Administrators group membership:

``` powershell
Get-LocalGroupMember -Group 'Administrators'
```

Availability can depend on PowerShell edition, architecture, and Windows
version.

------------------------------------------------------------------------

# Administrative Permissions

Some Windows commands require elevated permissions.

If a command returns:

``` text
Access denied
```

do not automatically assume the command is wrong.

Consider:

-   Does the operation require administrator rights?
-   Is organizational policy restricting it?
-   Are you targeting the correct system?
-   Is the module available?

Use elevated PowerShell only when the task requires it.

------------------------------------------------------------------------

# Building a Windows Health Report

You can combine several commands:

``` powershell
$os = Get-CimInstance Win32_OperatingSystem
$computer = Get-CimInstance Win32_ComputerSystem
$disks = Get-CimInstance Win32_LogicalDisk -Filter "DriveType=3"

$summary = [PSCustomObject]@{
    ComputerName = $env:COMPUTERNAME
    Manufacturer = $computer.Manufacturer
    Model        = $computer.Model
    OS           = $os.Caption
    Build        = $os.BuildNumber
    LastBoot     = $os.LastBootUpTime
}

$summary
```

Then create a disk report:

``` powershell
$disks |
    Select-Object DeviceID,
        @{
            Name = 'SizeGB'
            Expression = { [math]::Round($_.Size / 1GB, 2) }
        },
        @{
            Name = 'FreeGB'
            Expression = { [math]::Round($_.FreeSpace / 1GB, 2) }
        }
```

------------------------------------------------------------------------

# Windows Administration Workflow

A strong administrative pattern is:

``` text
Discover
   ↓
Inspect
   ↓
Filter
   ↓
Verify
   ↓
Preview
   ↓
Change
   ↓
Verify Again
```

For example:

``` powershell
Get-Service -Name Spooler
```

Then, if a restart is appropriate:

``` powershell
Restart-Service -Name Spooler -WhatIf
```

Only after confirming the target should you perform the actual change.

------------------------------------------------------------------------

# Key Takeaways

-   PowerShell can inspect and manage many areas of Windows.
-   `Get-ComputerInfo` provides broad computer information.
-   `Get-CimInstance` retrieves structured Windows management data.
-   CIM can provide OS, hardware, BIOS, and disk information.
-   `Get-Service` and `Get-Process` are central Windows administration
    commands.
-   Use service and process modification commands carefully.
-   `Get-NetIPConfiguration` and related commands inspect Windows
    networking.
-   `Test-Connection` and `Test-NetConnection` help troubleshoot
    connectivity.
-   `Get-WinEvent` is a powerful event-log command.
-   Filter event logs at the source when practical.
-   `Get-HotFix` provides useful but not necessarily complete update
    information.
-   Local account commands are available on supported Windows systems.
-   Some tasks require elevated permissions.
-   Prefer read-only discovery before making configuration changes.

------------------------------------------------------------------------

# Lab

Continue to:

[Lab 19 — Windows Administration](../labs/lesson-19-lab-19-windows-administration.md)

In the lab, you will collect Windows system information, inspect disks,
services, processes, networking, event logs, local accounts, and create
a small read-only Windows health report.

------------------------------------------------------------------------

# 🚀 Project Checkpoint

You have completed Lessons 16–19. Now apply error handling, modules, remoting concepts, and Windows administration skills to a realistic support scenario.

### [Project 04 — Windows Health Check Tool](../projects/project-04-windows-health-check.md)

Build a PowerShell troubleshooting tool that collects useful Windows health information and handles failures safely.

> **Tip:** Don't collect information just because you can. Think about what an IT technician would actually need when troubleshooting a computer.

------------------------------------------------------------------------

## Additional Resources

-   [Get-ComputerInfo --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-computerinfo?view=powershell-7.6)
-   [Get-CimInstance --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/cimcmdlets/get-ciminstance?view=powershell-7.6)
-   [Get-Service --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-service?view=powershell-7.6)
-   [Get-Process --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-process?view=powershell-7.6)
-   [Get-WinEvent --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.diagnostics/get-winevent?view=powershell-7.6)
-   [Get-NetIPConfiguration --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/nettcpip/get-netipconfiguration)
