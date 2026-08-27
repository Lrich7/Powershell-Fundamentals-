[lesson-20-active-directory.md](https://github.com/user-attachments/files/31518571/lesson-20-active-directory.md)

# Lesson 20 --- Active Directory with PowerShell

## Learning Objectives

By the end of this lesson, you will be able to:

-   Explain how PowerShell is used with Active Directory Domain
    Services.
-   Identify the ActiveDirectory PowerShell module.
-   Use `Get-ADUser`, `Get-ADGroup`, and `Get-ADComputer`.
-   Search and filter directory objects.
-   Select useful Active Directory properties.
-   Inspect group membership.
-   Understand common user-management commands.
-   Understand organizational units and distinguished names.
-   Export Active Directory information to CSV.
-   Apply safe practices before modifying directory objects.

------------------------------------------------------------------------

## Active Directory and PowerShell

Active Directory Domain Services (AD DS) is widely used to manage
identities and computers in Windows domain environments.

PowerShell can automate many Active Directory tasks, including:

``` text
Finding users
Finding computers
Reviewing groups
Checking group membership
Creating accounts
Updating attributes
Disabling accounts
Moving objects
Resetting passwords
Generating reports
```

> **Key Idea:** Active Directory PowerShell commands can affect
> authentication and access across an organization. Learn the read-only
> commands first and verify targets carefully before making changes.

------------------------------------------------------------------------

# Active Directory vs. Microsoft Entra ID

These are related identity technologies, but they are not the same
system.

``` text
Active Directory Domain Services
    ↓
Traditional Windows domain directory
Often hosted on Windows Server domain controllers

Microsoft Entra ID
    ↓
Cloud identity and access service
Used by Microsoft 365 and Azure
```

An organization can use:

-   AD DS only
-   Microsoft Entra ID only
-   Both in a hybrid identity environment

The commands in this lesson focus primarily on **Active Directory Domain
Services**.

------------------------------------------------------------------------

# The ActiveDirectory Module

Many AD DS PowerShell commands are provided by the:

``` text
ActiveDirectory
```

module.

Check whether it is available:

``` powershell
Get-Module -ListAvailable ActiveDirectory
```

Import it:

``` powershell
Import-Module ActiveDirectory
```

Discover its commands:

``` powershell
Get-Command -Module ActiveDirectory
```

The module is commonly installed through Windows administration tools
such as RSAT or on systems with appropriate Active Directory management
components.

------------------------------------------------------------------------

# Finding a User

Use:

``` powershell
Get-ADUser
```

Example:

``` powershell
Get-ADUser -Identity username
```

The identity can be specified in several supported forms, depending on
the object and environment.

------------------------------------------------------------------------

# Getting Additional User Properties

By default, `Get-ADUser` returns a limited property set.

Request additional properties:

``` powershell
Get-ADUser -Identity username `
    -Properties DisplayName, Department, Title, Enabled
```

Then select them:

``` powershell
Get-ADUser -Identity username `
    -Properties DisplayName, Department, Title, Enabled |
    Select-Object SamAccountName,
        DisplayName,
        Department,
        Title,
        Enabled
```

------------------------------------------------------------------------

# Finding Multiple Users

Use:

``` powershell
Get-ADUser -Filter *
```

This can return many users, so filter carefully in large environments.

Example:

``` powershell
Get-ADUser -Filter "Department -eq 'IT'" `
    -Properties Department |
    Select-Object Name, SamAccountName, Department
```

------------------------------------------------------------------------

# Filtering Active Directory

Many Active Directory cmdlets support:

``` text
-Filter
```

Filtering at the source is generally preferable to retrieving every
directory object and filtering locally.

For example:

``` powershell
Get-ADUser `
    -Filter "Enabled -eq 'True'" |
    Select-Object Name, SamAccountName, Enabled
```

The exact filter syntax supported by AD cmdlets differs from normal
`Where-Object` script blocks, so use:

``` powershell
Get-Help Get-ADUser -Parameter Filter
```

when needed.

------------------------------------------------------------------------

# SearchBase

Active Directory is hierarchical.

You can restrict searches to a particular organizational unit with:

``` text
-SearchBase
```

Example:

``` powershell
Get-ADUser `
    -Filter * `
    -SearchBase 'OU=Employees,DC=contoso,DC=com'
```

Use the actual distinguished name from your environment.

This can make searches faster and reduce accidental targeting.

------------------------------------------------------------------------

# Distinguished Names

Active Directory objects have a distinguished name, often abbreviated:

``` text
DN
```

Example:

``` text
CN=Alex Smith,OU=Employees,DC=contoso,DC=com
```

A DN identifies the object's location in the directory hierarchy.

You will commonly encounter components such as:

``` text
CN = Common Name
OU = Organizational Unit
DC = Domain Component
```

------------------------------------------------------------------------

# Finding Groups

Use:

``` powershell
Get-ADGroup
```

Example:

``` powershell
Get-ADGroup -Identity 'IT Support'
```

Find groups by filter:

``` powershell
Get-ADGroup -Filter "Name -like '*IT*'" |
    Select-Object Name, GroupScope, GroupCategory
```

------------------------------------------------------------------------

# Group Membership

Use:

``` powershell
Get-ADGroupMember
```

Example:

``` powershell
Get-ADGroupMember -Identity 'IT Support'
```

Select useful properties:

``` powershell
Get-ADGroupMember -Identity 'IT Support' |
    Select-Object Name, ObjectClass
```

For nested membership, you may encounter:

``` powershell
Get-ADGroupMember -Identity 'IT Support' -Recursive
```

Use recursive membership carefully in large or deeply nested group
structures.

------------------------------------------------------------------------

# Finding a User's Group Membership

Use:

``` powershell
Get-ADPrincipalGroupMembership
```

Example:

``` powershell
Get-ADPrincipalGroupMembership -Identity username |
    Select-Object Name, GroupScope, GroupCategory
```

This is useful for reviewing a user's direct group memberships.

------------------------------------------------------------------------

# Finding Computers

Use:

``` powershell
Get-ADComputer
```

Example:

``` powershell
Get-ADComputer -Identity PC-001
```

Request additional properties:

``` powershell
Get-ADComputer -Identity PC-001 `
    -Properties OperatingSystem, LastLogonDate, Enabled |
    Select-Object Name, OperatingSystem, LastLogonDate, Enabled
```

------------------------------------------------------------------------

# Building a Computer Inventory

Example:

``` powershell
Get-ADComputer -Filter * `
    -Properties OperatingSystem, LastLogonDate, Enabled |
    Select-Object Name,
        OperatingSystem,
        LastLogonDate,
        Enabled |
    Sort-Object Name
```

Export:

``` powershell
Get-ADComputer -Filter * `
    -Properties OperatingSystem, LastLogonDate, Enabled |
    Select-Object Name,
        OperatingSystem,
        LastLogonDate,
        Enabled |
    Export-Csv C:\Temp\ad-computers.csv -NoTypeInformation
```

This is a useful read-only administration project.

------------------------------------------------------------------------

# Creating Users

The ActiveDirectory module provides:

``` powershell
New-ADUser
```

Creating users involves many organizational decisions, including:

-   Naming conventions
-   User principal names
-   Organizational units
-   Password handling
-   Account enablement
-   Group membership
-   Required attributes
-   Licensing in hybrid/cloud environments

Before automating account creation, document the organization's approved
onboarding process.

Use:

``` powershell
Get-Help New-ADUser -Full
```

before attempting creation.

------------------------------------------------------------------------

# Modifying Users

Use:

``` powershell
Set-ADUser
```

to modify supported user properties.

For example, organizations may use it to update:

``` text
Department
Title
Manager
Office
Description
```

Do not test modification commands against production users casually.

Use a lab account and approved procedures.

------------------------------------------------------------------------

# Disabling and Enabling Accounts

The module provides:

``` powershell
Disable-ADAccount
Enable-ADAccount
```

Example syntax:

``` powershell
Disable-ADAccount -Identity username
```

Account disablement can immediately affect a person's ability to
authenticate.

For offboarding or emergency termination processes, follow the
organization's documented procedure rather than treating this as an
isolated command.

------------------------------------------------------------------------

# Group Membership Changes

PowerShell provides:

``` powershell
Add-ADGroupMember
Remove-ADGroupMember
```

Example syntax:

``` powershell
Add-ADGroupMember `
    -Identity 'IT Support' `
    -Members username
```

Before modifying group membership:

``` text
Verify the user
Verify the group
Understand the access the group grants
Confirm approval
Make the change
Verify membership afterward
```

Security-group membership can grant access to sensitive systems and
data.

------------------------------------------------------------------------

# Password Resets

The ActiveDirectory module includes:

``` powershell
Set-ADAccountPassword
```

Password operations should follow organizational security policy.

Do not place passwords in plain text inside scripts, Git repositories,
or training examples intended for reuse.

------------------------------------------------------------------------

# Moving AD Objects

PowerShell provides:

``` powershell
Move-ADObject
```

This can move directory objects between organizational units.

Moving an object may affect:

-   Group Policy
-   Delegated administration
-   Automation
-   Application assumptions

Always verify the source object and destination OU before moving
anything.

------------------------------------------------------------------------

# Read-Only AD Reporting

A strong beginner Active Directory project is reporting.

For example, enabled users:

``` powershell
Get-ADUser -Filter "Enabled -eq 'True'" `
    -Properties Department, Title |
    Select-Object Name,
        SamAccountName,
        Department,
        Title |
    Sort-Object Name
```

Or disabled users:

``` powershell
Get-ADUser -Filter "Enabled -eq 'False'" |
    Select-Object Name, SamAccountName
```

------------------------------------------------------------------------

# Practical User Report

``` powershell
$users = Get-ADUser `
    -Filter * `
    -Properties DisplayName,
        Department,
        Title,
        Enabled,
        LastLogonDate

$users |
    Select-Object SamAccountName,
        DisplayName,
        Department,
        Title,
        Enabled,
        LastLogonDate |
    Sort-Object DisplayName |
    Export-Csv C:\Temp\ad-user-report.csv -NoTypeInformation
```

This combines:

``` text
Modules
Objects
Properties
Filtering
Sorting
CSV export
```

------------------------------------------------------------------------

# Active Directory Safety Workflow

For directory changes, use a deliberate process:

``` text
Find
 ↓
Inspect
 ↓
Confirm Identity
 ↓
Confirm Scope / OU
 ↓
Confirm Authorization
 ↓
Preview Where Possible
 ↓
Change
 ↓
Verify
 ↓
Document
```

A typo in an Active Directory command can affect real accounts and
access.

------------------------------------------------------------------------

# Key Takeaways

-   The ActiveDirectory module provides PowerShell commands for AD DS.
-   AD DS and Microsoft Entra ID are different identity systems.
-   `Get-ADUser` retrieves user objects.
-   `Get-ADGroup` and `Get-ADGroupMember` inspect groups and membership.
-   `Get-ADPrincipalGroupMembership` reviews a principal's group
    memberships.
-   `Get-ADComputer` retrieves computer objects.
-   `-Properties` requests additional directory properties.
-   `-Filter` and `-SearchBase` help narrow searches.
-   Distinguished names identify objects and locations in AD.
-   AD modification cmdlets can affect authentication and authorization.
-   Start with read-only reporting and use lab accounts for modification
    practice.
-   Never store plain-text passwords in reusable scripts.
-   Verify directory changes after making them.

------------------------------------------------------------------------

# Lab

Continue to:

[Lab 20 — Active Directory](../labs/lesson-20-lab-20-active-directory.md)

The lab should be completed in an authorized AD DS environment or lab
domain. It will focus first on read-only discovery: users, groups,
computers, properties, filters, membership, and CSV reporting. Any
modification exercises should use designated lab accounts only.

------------------------------------------------------------------------

## Additional Resources

-   [ActiveDirectory Module --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/activedirectory/)
-   [Get-ADUser --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/activedirectory/get-aduser)
-   [Get-ADGroup --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/activedirectory/get-adgroup)
-   [Get-ADGroupMember --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/activedirectory/get-adgroupmember)
-   [Get-ADComputer --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/activedirectory/get-adcomputer)
-   [New-ADUser --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/activedirectory/new-aduser)
-   [Set-ADUser --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/activedirectory/set-aduser)
