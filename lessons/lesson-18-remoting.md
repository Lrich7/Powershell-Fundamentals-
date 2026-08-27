[lesson-18-remoting.md](https://github.com/user-attachments/files/31518530/lesson-18-remoting.md)

# Lesson 18 --- PowerShell Remoting

## Learning Objectives

By the end of this lesson, you will be able to:

-   Explain what PowerShell remoting is.
-   Distinguish local commands from remote commands.
-   Understand the purpose of PowerShell remoting transports at a
    beginner level.
-   Use `Test-WSMan` where WS-Management remoting is applicable.
-   Use `Invoke-Command` for one-to-one and one-to-many remote
    execution.
-   Create interactive remote sessions with `Enter-PSSession`.
-   Create reusable sessions with `New-PSSession`.
-   Close sessions with `Exit-PSSession` and `Remove-PSSession`.
-   Understand how variables behave inside remote commands.
-   Use `$using:` to pass local values into remote script blocks.
-   Recognize authentication, authorization, firewall, and configuration
    requirements.
-   Use remoting cautiously in administrative environments.

------------------------------------------------------------------------

## What Is PowerShell Remoting?

PowerShell remoting allows you to run PowerShell commands on another
computer.

Instead of signing into each server individually, an administrator can
execute commands remotely.

Conceptually:

``` text
Your Computer
     │
     │ PowerShell Remoting
     ↓
Remote Computer
     │
     ↓
Command Runs There
     │
     ↓
Results Return to You
```

> **Key Idea:** PowerShell remoting allows administrators to manage
> remote systems using the same command and object concepts used
> locally.

------------------------------------------------------------------------

# Why Remoting Is Useful

Without remoting, an administrator might need to:

``` text
Connect to Server01
Run command
Disconnect

Connect to Server02
Run command
Disconnect

Connect to Server03
Run command
Disconnect
```

With remoting, PowerShell can perform work against multiple systems from
one administrative session.

This can be useful for:

-   Gathering system information
-   Checking services
-   Reviewing configuration
-   Running administrative commands
-   Managing multiple servers
-   Automating repetitive remote tasks

------------------------------------------------------------------------

# Remoting Requires Configuration

Remote commands do not work simply because two computers are connected
to the same network.

Remoting depends on several factors, including:

-   PowerShell configuration
-   Operating system
-   Network connectivity
-   Firewall rules
-   Authentication
-   Authorization
-   Endpoint configuration
-   The remoting transport being used
-   Organizational security policy

Do not enable or modify remoting settings on company systems without
understanding your organization's configuration and security
requirements.

------------------------------------------------------------------------

# Windows PowerShell Remoting and WS-Management

Traditional PowerShell remoting on Windows commonly uses:

``` text
WS-Management
```

and the Windows Remote Management service:

``` text
WinRM
```

You may encounter commands such as:

``` powershell
Test-WSMan
```

and:

``` powershell
Enable-PSRemoting
```

`Enable-PSRemoting` changes system configuration to allow PowerShell
remoting.

> **Important:** Do not enable remoting simply for practice on a managed
> device. Use a lab environment or follow your organization's approved
> configuration.

------------------------------------------------------------------------

# PowerShell Remoting over SSH

Modern PowerShell can also support remoting over:

``` text
SSH
```

This is especially useful across different operating systems.

The exact setup differs from WS-Management remoting.

At this stage, focus on understanding the remoting model rather than
configuring production endpoints.

------------------------------------------------------------------------

# Testing WS-Management

Where WS-Management is configured, you can test it with:

``` powershell
Test-WSMan -ComputerName Server01
```

A successful response indicates that the WS-Management service can
respond.

It does **not** automatically mean you have permission to run every
remote administrative command.

Connectivity and authorization are separate concerns.

------------------------------------------------------------------------

# Invoke-Command

One of the most important remoting commands is:

``` powershell
Invoke-Command
```

Basic example:

``` powershell
Invoke-Command `
    -ComputerName Server01 `
    -ScriptBlock {
        Get-Service
    }
```

The commands inside:

``` powershell
-ScriptBlock { }
```

run on the remote computer.

------------------------------------------------------------------------

# A Simple Remote Command

Example:

``` powershell
Invoke-Command `
    -ComputerName Server01 `
    -ScriptBlock {
        Get-Date
    }
```

The date is retrieved from:

``` text
Server01
```

not necessarily from your local computer.

This distinction is important.

------------------------------------------------------------------------

# One-to-Many Remoting

`Invoke-Command` can target multiple computers.

Example:

``` powershell
$computers = 'Server01', 'Server02', 'Server03'

Invoke-Command `
    -ComputerName $computers `
    -ScriptBlock {
        Get-Service -Name Spooler
    }
```

PowerShell can return results from all targeted systems.

This is where remoting becomes especially powerful for administration.

------------------------------------------------------------------------

# Remote Output Is Still Object-Based

Remote output returns to your session in a serialized form.

For many reporting and inspection tasks, it still behaves much like
familiar PowerShell objects.

Example:

``` powershell
$results = Invoke-Command `
    -ComputerName Server01 `
    -ScriptBlock {
        Get-Service
    }
```

Then:

``` powershell
$results |
    Select-Object Name, Status, PSComputerName
```

The property:

``` text
PSComputerName
```

can help identify which remote system produced a result.

------------------------------------------------------------------------

# Deserialized Objects

Remote objects are often returned as **deserialized** representations.

This means their property data is preserved, but they may not have all
of the live methods of the original object on the remote system.

You may see type names beginning with:

``` text
Deserialized.
```

This is an important remoting concept:

``` text
Command executes remotely
        ↓
Object is serialized
        ↓
Data travels to your computer
        ↓
Deserialized representation is created
```

If an action must use a live object's methods, it is often better to
perform that action inside the remote script block.

------------------------------------------------------------------------

# Interactive Remoting

Use:

``` powershell
Enter-PSSession
```

to start an interactive remote session.

Example:

``` powershell
Enter-PSSession -ComputerName Server01
```

Your prompt typically changes to indicate the remote computer.

Commands you enter now execute remotely.

To leave:

``` powershell
Exit-PSSession
```

------------------------------------------------------------------------

# Invoke-Command vs. Enter-PSSession

A useful guideline:

``` text
Invoke-Command
    ↓
Run commands remotely and return results
    ↓
Excellent for automation and multiple computers

Enter-PSSession
    ↓
Interactive remote shell
    ↓
Useful for troubleshooting one computer
```

For repeatable administration, `Invoke-Command` is generally more
automation-friendly.

------------------------------------------------------------------------

# Persistent Sessions

You can create a reusable PowerShell session:

``` powershell
$session = New-PSSession -ComputerName Server01
```

Inspect it:

``` powershell
$session
```

Then use it:

``` powershell
Invoke-Command `
    -Session $session `
    -ScriptBlock {
        Get-Date
    }
```

A persistent session can retain remote session state between commands.

------------------------------------------------------------------------

# Removing Sessions

When finished:

``` powershell
Remove-PSSession $session
```

You can view current sessions:

``` powershell
Get-PSSession
```

Cleaning up sessions is a good administrative habit.

------------------------------------------------------------------------

# Local vs. Remote Variables

Consider:

``` powershell
$serviceName = 'Spooler'
```

Then:

``` powershell
Invoke-Command `
    -ComputerName Server01 `
    -ScriptBlock {
        Get-Service -Name $serviceName
    }
```

The remote session does not automatically use your local variable the
way you might expect.

The script block runs in a different session.

------------------------------------------------------------------------

# The \$using: Scope Modifier

To reference a local value inside a remote script block, you can use:

``` powershell
$using:
```

Example:

``` powershell
$serviceName = 'Spooler'

Invoke-Command `
    -ComputerName Server01 `
    -ScriptBlock {
        Get-Service -Name $using:serviceName
    }
```

The local value is supplied to the remote command.

------------------------------------------------------------------------

# Using -ArgumentList

Another approach is to pass arguments.

Example:

``` powershell
$serviceName = 'Spooler'

Invoke-Command `
    -ComputerName Server01 `
    -ScriptBlock {
        param ($Name)

        Get-Service -Name $Name
    } `
    -ArgumentList $serviceName
```

This approach can make dependencies explicit inside the remote script
block.

------------------------------------------------------------------------

# Authentication and Authorization

Remote access requires appropriate identity and permissions.

Being able to reach a computer over the network does not automatically
grant administrative access.

Depending on the environment, remoting may involve:

-   Kerberos
-   NTLM
-   SSH authentication
-   Certificates
-   Explicit credentials
-   Domain configuration
-   Endpoint permissions

Avoid hard-coding passwords into PowerShell scripts.

------------------------------------------------------------------------

# Credentials

Some remoting commands support:

``` text
-Credential
```

For example:

``` powershell
$credential = Get-Credential
```

Then, where appropriate:

``` powershell
Invoke-Command `
    -ComputerName Server01 `
    -Credential $credential `
    -ScriptBlock {
        Get-Date
    }
```

`Get-Credential` provides a credential object rather than placing a
plain-text password directly in the script.

Whether explicit credentials are appropriate depends on your
environment.

------------------------------------------------------------------------

# The Double-Hop Problem

You may eventually encounter a situation like:

``` text
Your Computer
     ↓
Server01
     ↓
FileServer01
```

A remote session to `Server01` may not automatically be able to reuse
your credentials to access another network resource.

This is commonly called the:

``` text
double-hop problem
```

It involves authentication delegation and should not be solved by
weakening security settings casually.

At this stage, recognize the term and investigate approved
authentication approaches if you encounter it.

------------------------------------------------------------------------

# Run Filtering Remotely When Possible

Suppose you only need running services.

Less efficient:

``` powershell
Invoke-Command `
    -ComputerName Server01 `
    -ScriptBlock {
        Get-Service
    } |
    Where-Object Status -eq 'Running'
```

This retrieves all service data before filtering locally.

Instead:

``` powershell
Invoke-Command `
    -ComputerName Server01 `
    -ScriptBlock {
        Get-Service |
            Where-Object Status -eq 'Running'
    }
```

Now the filtering happens remotely.

A useful rule is:

> Do as much filtering and processing on the remote system as practical
> before sending results across the network.

------------------------------------------------------------------------

# Remoting with Multiple Computers

Example:

``` powershell
$servers = 'Server01', 'Server02', 'Server03'

$results = Invoke-Command `
    -ComputerName $servers `
    -ScriptBlock {
        Get-Service |
            Where-Object Status -eq 'Stopped' |
            Select-Object Name, DisplayName, Status
    }
```

Then locally:

``` powershell
$results |
    Select-Object PSComputerName, Name, DisplayName, Status |
    Sort-Object PSComputerName, Name
```

This creates a multi-server report.

------------------------------------------------------------------------

# Error Handling with Remoting

Remote computers may be offline or inaccessible.

You can combine remoting with Lesson 16:

``` powershell
try {
    Invoke-Command `
        -ComputerName Server01 `
        -ScriptBlock {
            Get-Date
        } `
        -ErrorAction Stop
}
catch {
    Write-Warning "Remote command failed: $($_.Exception.Message)"
}
```

This makes remoting scripts more resilient.

------------------------------------------------------------------------

# Remoting in Functions

You can wrap remote tasks in functions.

Example:

``` powershell
function Get-RemoteServiceStatus {
    [CmdletBinding()]
    param (
        [Parameter(Mandatory)]
        [string[]]$ComputerName,

        [string]$ServiceName = 'Spooler'
    )

    Invoke-Command `
        -ComputerName $ComputerName `
        -ScriptBlock {
            param ($Name)

            Get-Service -Name $Name
        } `
        -ArgumentList $ServiceName
}
```

This combines:

``` text
Functions
Parameters
Arrays
Remoting
Objects
```

------------------------------------------------------------------------

# Remoting Safety

Remote PowerShell can make changes across many computers quickly.

Before running a change against many systems:

``` text
1. Test the command locally or in a lab
2. Test against one remote system
3. Confirm the target list
4. Use read-only discovery first
5. Use -WhatIf when the remote command supports it
6. Expand to a small group
7. Verify results
8. Only then consider broader execution
```

Do not begin with a destructive command against every computer in an
environment.

------------------------------------------------------------------------

# Practical Read-Only Example

A safe beginner remoting task is inventory collection.

``` powershell
$servers = 'Server01', 'Server02'

$results = Invoke-Command `
    -ComputerName $servers `
    -ScriptBlock {
        [PSCustomObject]@{
            ComputerName = $env:COMPUTERNAME
            PowerShell   = $PSVersionTable.PSVersion.ToString()
            CheckedAt    = Get-Date
        }
    }

$results |
    Select-Object ComputerName, PowerShell, CheckedAt
```

This demonstrates remoting without modifying the target systems.

------------------------------------------------------------------------

# Key Takeaways

-   PowerShell remoting runs commands on remote computers.
-   Remoting requires network, authentication, authorization, and
    endpoint configuration.
-   Traditional Windows PowerShell remoting commonly uses
    WS-Management/WinRM.
-   Modern PowerShell can also support SSH-based remoting.
-   `Test-WSMan` can test WS-Management availability.
-   `Invoke-Command` is central to remote automation.
-   `Invoke-Command` can target multiple computers.
-   `Enter-PSSession` provides an interactive remote shell.
-   `New-PSSession` creates reusable remote sessions.
-   `Remove-PSSession` cleans up persistent sessions.
-   Remote output is commonly returned as deserialized objects.
-   `$using:` can pass local variable values into remote script blocks.
-   `-ArgumentList` provides another way to pass values.
-   Do not hard-code passwords in scripts.
-   The double-hop problem involves credential delegation across
    multiple remote connections.
-   Filter and process remotely when practical.
-   Combine remoting with error handling.
-   Test remote changes on a small scope before expanding them.

------------------------------------------------------------------------

# Lab

Continue to:

[Lab 18 --- PowerShell Remoting](../labs/lab-18-remoting.md)

The lab should be completed only in an approved environment where
PowerShell remoting is already configured or where you are authorized to
configure it. You will inspect remoting commands, test connectivity, run
read-only remote commands, compare `Invoke-Command` and interactive
sessions, pass variables to remote script blocks, and collect a small
multi-computer report.

------------------------------------------------------------------------

## Additional Resources

-   [About Remote --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_remote?view=powershell-7.6)
-   [About Remote Requirements --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_remote_requirements?view=powershell-7.6)
-   [Invoke-Command --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/invoke-command?view=powershell-7.6)
-   [Enter-PSSession --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/enter-pssession?view=powershell-7.6)
-   [New-PSSession --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/new-pssession?view=powershell-7.6)
-   [About PSSessions --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_pssessions?view=powershell-7.6)
-   [About Remote Variables --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_remote_variables?view=powershell-7.6)
-   [PowerShell Remoting over SSH --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/scripting/security/remoting/ssh-remoting-in-powershell)
