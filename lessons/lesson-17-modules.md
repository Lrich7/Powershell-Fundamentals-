[lesson-17-modules.md](https://github.com/user-attachments/files/31518478/lesson-17-modules.md)

# Lesson 17 --- Modules

## Learning Objectives

By the end of this lesson, you will be able to:

-   Explain what a PowerShell module is.
-   Identify commands and modules available on a system.
-   Use `Get-Module` and `Get-Command`.
-   Import modules with `Import-Module`.
-   Understand PowerShell module auto-loading.
-   Find installed modules with `Get-InstalledModule` where applicable.
-   Find modules from registered repositories with `Find-Module`.
-   Install and update modules using the appropriate module-management
    commands available in your environment.
-   Understand `$env:PSModulePath`.
-   Recognize a basic script module (`.psm1`) and module manifest
    (`.psd1`).
-   Understand why module source and trust should be reviewed before
    installation.

------------------------------------------------------------------------

## What Is a PowerShell Module?

A **module** is a package of reusable PowerShell functionality.

Modules can contain:

-   Functions
-   Cmdlets
-   Variables
-   Classes
-   Providers
-   Help content
-   Other supporting files

Many PowerShell commands you use are provided by modules.

> **Key Idea:** Modules organize related PowerShell tools so they can be
> discovered, loaded, shared, and maintained together.

------------------------------------------------------------------------

# Commands Come from Modules

Run:

``` powershell
Get-Command Get-Service
```

Look at the command information.

You can also request:

``` powershell
Get-Command Get-Service |
    Select-Object Name, CommandType, Source
```

The `Source` property can identify the module that provides the command.

Another example:

``` powershell
Get-Command |
    Select-Object -First 20 Name, CommandType, Source
```

------------------------------------------------------------------------

# Listing Loaded Modules

Use:

``` powershell
Get-Module
```

This displays modules currently loaded in your PowerShell session.

You may notice that not every installed module appears here.

That is because:

``` powershell
Get-Module
```

normally shows **loaded** modules.

------------------------------------------------------------------------

# Listing Available Modules

Use:

``` powershell
Get-Module -ListAvailable
```

This searches module locations and lists modules available to
PowerShell.

Because a system may contain many modules, you can filter:

``` powershell
Get-Module -ListAvailable |
    Sort-Object Name |
    Select-Object Name, Version, Path
```

------------------------------------------------------------------------

# Importing a Module

Use:

``` powershell
Import-Module
```

to load a module.

Example:

``` powershell
Import-Module Microsoft.PowerShell.Management
```

Then:

``` powershell
Get-Module Microsoft.PowerShell.Management
```

------------------------------------------------------------------------

# Module Auto-Loading

Modern PowerShell can automatically load a module when you run one of
its commands.

This means you do not always need to manually run:

``` powershell
Import-Module
```

before using a command.

For example, if PowerShell knows an available module contains a command,
calling that command may cause the module to load automatically.

Manual imports are still useful when you want to be explicit about
dependencies or versions.

------------------------------------------------------------------------

# Finding Commands in a Module

Use:

``` powershell
Get-Command -Module Microsoft.PowerShell.Management
```

This displays commands provided by that module.

You can also inspect a loaded module:

``` powershell
Get-Module Microsoft.PowerShell.Management |
    Select-Object Name, Version, ExportedCommands
```

------------------------------------------------------------------------

# Module Information

You can inspect a module:

``` powershell
$module = Get-Module Microsoft.PowerShell.Management
```

Then:

``` powershell
$module | Get-Member
```

Useful properties can include:

``` text
Name
Version
Path
Description
ExportedCommands
```

Again, PowerShell exposes useful information as objects.

------------------------------------------------------------------------

# Where PowerShell Looks for Modules

PowerShell searches locations listed in:

``` powershell
$env:PSModulePath
```

Display it:

``` powershell
$env:PSModulePath
```

The exact paths depend on:

-   Operating system
-   PowerShell edition
-   Installation
-   User configuration

Do not assume every computer uses the same module paths.

------------------------------------------------------------------------

# PowerShell Repositories

PowerShell can obtain modules from registered repositories.

A widely used public repository is the PowerShell Gallery.

Before installing third-party modules, consider:

-   Who publishes the module?
-   Is the source trusted?
-   Is the module maintained?
-   What commands or permissions will it use?
-   Does your organization allow it?

> **Important:** A PowerShell module contains executable code. Treat
> module installation like software installation.

------------------------------------------------------------------------

# Finding Modules

Depending on the module-management tooling available in your PowerShell
environment, you may use commands such as:

``` powershell
Find-Module
```

For example:

``` powershell
Find-Module -Name Pester
```

You can inspect information before installing:

``` powershell
Find-Module -Name Pester |
    Select-Object Name, Version, Repository, Description
```

------------------------------------------------------------------------

# Installing Modules

A common command in environments using PowerShellGet is:

``` powershell
Install-Module
```

Example:

``` powershell
Install-Module -Name Pester -Scope CurrentUser
```

Using:

``` text
-Scope CurrentUser
```

can install a module for the current user without installing it
system-wide.

Do not install modules on production or managed systems without
following organizational policy.

------------------------------------------------------------------------

# Modern Module Management

Newer PowerShell environments may also use the **PSResourceGet** module
and commands such as:

``` powershell
Find-PSResource
Install-PSResource
Update-PSResource
```

For example:

``` powershell
Find-PSResource -Name Pester
```

Which management commands are available depends on your environment.

Discover them:

``` powershell
Get-Command *-Module
```

and:

``` powershell
Get-Command *-PSResource
```

Then use:

``` powershell
Get-Help <CommandName> -Full
```

before installing or changing modules.

------------------------------------------------------------------------

# Installed vs. Loaded

These concepts are different:

``` text
Installed / Available
        ↓
Module exists on the computer

Loaded
        ↓
Module is active in the current PowerShell session
```

A module can be installed without currently being loaded.

Use:

``` powershell
Get-Module -ListAvailable
```

for available modules.

Use:

``` powershell
Get-Module
```

for loaded modules.

------------------------------------------------------------------------

# Removing a Module from the Session

Use:

``` powershell
Remove-Module
```

to unload a module from the current session.

Example:

``` powershell
Remove-Module Microsoft.PowerShell.Management
```

This does **not** uninstall the module from the computer.

It removes it from the current session.

------------------------------------------------------------------------

# Updating Modules

In environments using PowerShellGet, you may encounter:

``` powershell
Update-Module
```

With PSResourceGet, you may encounter:

``` powershell
Update-PSResource
```

Before updating modules used by production scripts, consider
compatibility.

A newer module version can introduce changes.

Test important scripts before rolling module updates into production
use.

------------------------------------------------------------------------

# Module Versions

A computer may have multiple versions of a module.

Inspect versions:

``` powershell
Get-Module -ListAvailable |
    Sort-Object Name, Version |
    Select-Object Name, Version, Path
```

You can import a particular version when needed:

``` powershell
Import-Module SomeModule -RequiredVersion 1.2.3
```

Use the actual module and version available in your environment.

Version awareness becomes important when scripts depend on specific
functionality.

------------------------------------------------------------------------

# What Is a Script Module?

A PowerShell script module commonly uses the extension:

``` text
.psm1
```

For example:

``` text
CompanyTools.psm1
```

A basic module might contain:

``` powershell
function Get-CompanyGreeting {
    'Hello from CompanyTools.'
}
```

Save the function in:

``` text
CompanyTools.psm1
```

Then import it:

``` powershell
Import-Module .\CompanyTools.psm1
```

Now run:

``` powershell
Get-CompanyGreeting
```

------------------------------------------------------------------------

# Module Folder Structure

A basic module may look like:

``` text
CompanyTools\
│
├── CompanyTools.psm1
└── CompanyTools.psd1
```

The:

``` text
.psm1
```

file contains PowerShell module code.

The:

``` text
.psd1
```

file can be a module manifest containing metadata and configuration.

------------------------------------------------------------------------

# Module Manifests

A module manifest can contain information such as:

-   Module version
-   Author
-   Description
-   Compatible PowerShell versions
-   Functions to export
-   Required modules

You can create a manifest with:

``` powershell
New-ModuleManifest
```

You do not need to build a production module yet, but understanding the
structure prepares you for organizing reusable PowerShell tools.

------------------------------------------------------------------------

# Scripts vs. Modules

A script typically performs a workflow:

``` text
report.ps1
```

A module usually packages reusable commands:

``` text
CompanyTools.psm1
```

Conceptually:

``` text
Function
   ↓
Reusable command

Several related functions
   ↓
Module

Module commands + workflow
   ↓
Script / automation
```

------------------------------------------------------------------------

# Module Dependencies

A script may depend on a particular module.

Before using it, you can check:

``` powershell
Get-Module -ListAvailable -Name SomeModule
```

Scripts can also declare module requirements with `#Requires`.

Example:

``` powershell
#Requires -Modules SomeModule
```

If the requirement is unavailable, PowerShell prevents the script from
running normally.

This makes dependencies clearer.

------------------------------------------------------------------------

# Practical Administrative Example

Suppose your organization develops several functions:

``` text
Get-AssetSummary
Test-AssetAssignment
Export-AssetReport
```

Instead of copying these functions into every script, you could
eventually place them in:

``` text
CompanyAssetTools
```

Then scripts could import the module and use the shared commands.

This improves:

-   Reuse
-   Consistency
-   Maintenance
-   Version control
-   Documentation

------------------------------------------------------------------------

# Safe Module Workflow

Before adopting a module:

``` text
Find
  ↓
Review Source / Publisher
  ↓
Inspect Version and Documentation
  ↓
Install in an Appropriate Scope
  ↓
Import
  ↓
Discover Commands
  ↓
Read Help
  ↓
Test
  ↓
Use in Automation
```

Do not treat an internet-hosted PowerShell module as harmless data. It
is code.

------------------------------------------------------------------------

# Key Takeaways

-   Modules package related PowerShell functionality.
-   `Get-Module` shows loaded modules.
-   `Get-Module -ListAvailable` shows modules available to PowerShell.
-   `Import-Module` loads a module.
-   PowerShell can auto-load modules when commands are used.
-   `Get-Command -Module` lists commands from a module.
-   `$env:PSModulePath` contains module search paths.
-   Module management tooling can vary by environment.
-   PowerShellGet commonly provides commands such as `Find-Module` and
    `Install-Module`.
-   PSResourceGet provides newer commands such as `Find-PSResource` and
    `Install-PSResource`.
-   Review module trust and source before installation.
-   `Remove-Module` unloads a module but does not uninstall it.
-   Module versions can affect script compatibility.
-   `.psm1` files contain script module code.
-   `.psd1` files can provide module manifests.
-   Modules are a natural next step for organizing reusable functions.

------------------------------------------------------------------------

# Lab

Continue to:

[Lab 17 — Modules](../labs/lesson-17-lab-17-modules.md)


In the lab, you will identify loaded and available modules, discover
module commands, import and remove modules, inspect module paths, safely
explore repository commands, and create a small local script module from
functions you have already learned to write.

------------------------------------------------------------------------

## Additional Resources

-   [About Modules --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_modules?view=powershell-7.6)
-   [Import-Module --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/import-module?view=powershell-7.6)
-   [Get-Module --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/get-module?view=powershell-7.6)
-   [About PSModulePath --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_psmodulepath?view=powershell-7.6)
-   [PowerShellGet --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/gallery/powershellget/overview)
-   [Microsoft.PowerShell.PSResourceGet --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.psresourceget/)
