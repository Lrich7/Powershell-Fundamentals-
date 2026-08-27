# Lab 20 --- Active Directory

## Lab Objective

This is an **environment-dependent, read-only lab**.

If you have authorized access to an Active Directory Domain Services
environment and the ActiveDirectory module, you will practice:

-   Discovering the ActiveDirectory module.
-   Finding users, groups, and computers.
-   Requesting additional properties.
-   Filtering directory objects.
-   Reviewing group membership.
-   Working with organizational units and distinguished names.
-   Exporting directory reports.

You will **not** create, disable, move, delete, or modify directory
objects in this lab.

------------------------------------------------------------------------

## Safety

Active Directory commands can affect authentication and access across an
organization.

This lab focuses on:

``` text
Get
Inspect
Filter
Report
```

not:

``` text
Create
Set
Disable
Remove
Move
Reset
```

Use only an AD environment you are authorized to query.

------------------------------------------------------------------------

# Exercise 1 --- Check the Module

Run:

``` powershell
Get-Module -ListAvailable ActiveDirectory
```

If available:

``` powershell
Import-Module ActiveDirectory
```

Then:

``` powershell
Get-Command -Module ActiveDirectory
```

If it is unavailable, use the **Offline Practice Track** later in this
lab.

------------------------------------------------------------------------

# Exercise 2 --- Find Your Own or a Lab User

Use an authorized test identity or your own directory identity:

``` powershell
Get-ADUser -Identity <username>
```

Do not guess other employee usernames merely for practice.

Inspect:

``` powershell
Get-ADUser -Identity <username> |
    Get-Member
```

------------------------------------------------------------------------

# Exercise 3 --- Request Additional Properties

Retrieve:

``` text
SamAccountName
DisplayName
Department
Title
Enabled
```

Remember that additional AD properties may need to be requested
explicitly.

``` powershell
# Your command:
```

------------------------------------------------------------------------

# Exercise 4 --- Filter Users

Use a safe query to retrieve users in your authorized scope.

Practice filtering for enabled accounts.

``` powershell
# Your command:
```

Then select a small, useful property set.

------------------------------------------------------------------------

# Exercise 5 --- Groups

Discover:

``` powershell
Get-ADGroup
```

Use a known lab or authorized group:

``` powershell
Get-ADGroup -Identity <group>
```

Inspect its members:

``` powershell
Get-ADGroupMember -Identity <group>
```

Do not add or remove anyone.

------------------------------------------------------------------------

# Exercise 6 --- Computers

Use:

``` powershell
Get-ADComputer
```

Build a read-only report showing useful computer properties available in
your environment.

Potential fields include:

``` text
Name
Enabled
OperatingSystem
LastLogonDate
```

Request additional properties as needed.

``` powershell
# Your command:
```

------------------------------------------------------------------------

# Exercise 7 --- Organizational Units

Use the lesson's AD discovery commands to inspect OUs.

Identify one authorized OU and record its distinguished name:

``` text
OU:
________________________________________

Distinguished Name:
________________________________________
```

### Question

Why is a distinguished name useful in AD PowerShell?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 8 --- SearchBase

If you have an authorized OU, limit a query to it with `-SearchBase`.

Example goal:

> Retrieve computer objects only from a specific lab or departmental OU.

``` powershell
# Your command:
```

Why is limiting scope important?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 9 --- Export a Read-Only Report

Create a report of authorized AD users containing a limited set of
useful properties.

Export with:

``` powershell
Export-Csv
```

Suggested fields:

``` text
SamAccountName
DisplayName
Department
Title
Enabled
```

``` powershell
# Your solution:
```

------------------------------------------------------------------------

# End-of-Lab Project --- AD Audit

## Scenario

> IT needs a read-only Active Directory inventory for review. No
> directory changes are authorized.

Create a script that generates up to three reports:

``` text
users.csv
computers.csv
groups.csv
```

Requirements:

``` text
[ ] Checks that the ActiveDirectory module is available
[ ] Imports it safely
[ ] Uses Get-ADUser
[ ] Uses Get-ADComputer
[ ] Uses Get-ADGroup
[ ] Requests useful properties
[ ] Limits scope where appropriate
[ ] Sorts output
[ ] Exports clean CSV reports
[ ] Uses error handling
[ ] Makes no AD changes
```

### Optional Findings

If appropriate for your lab environment, identify:

``` text
Disabled users
Disabled computers
Users with blank Department
Computer OS distribution
Group membership for a known authorized group
```

------------------------------------------------------------------------

# Offline Practice Track --- No AD Lab Available

You can still practice the data-processing skills.

Create:

``` powershell
$users = @(
    [PSCustomObject]@{
        SamAccountName = 'alex'
        DisplayName    = 'Alex Smith'
        Department     = 'IT'
        Title          = 'IT Specialist'
        Enabled        = $true
    }
    [PSCustomObject]@{
        SamAccountName = 'jordan'
        DisplayName    = 'Jordan Lee'
        Department     = 'Operations'
        Title          = 'Coordinator'
        Enabled        = $true
    }
    [PSCustomObject]@{
        SamAccountName = 'formeruser'
        DisplayName    = 'Former User'
        Department     = ''
        Title          = ''
        Enabled        = $false
    }
)
```

Practice:

1.  Find enabled users.
2.  Find disabled users.
3.  Find users with a blank department.
4.  Sort by department and display name.
5.  Export a CSV report.

``` powershell
# Your solutions:
```

This does not replace experience with AD cmdlets, but it lets you
practice the object, filtering, reporting, and audit logic safely.

------------------------------------------------------------------------

# Knowledge Check

1.  Which module commonly provides AD DS PowerShell commands?

    A. `ActiveDirectory` B. `Microsoft.Graph` C.
    `ExchangeOnlineManagement` D. `WindowsUpdate`

2.  Which command retrieves AD users?

    A. `Get-ADUser` B. `Get-EntraUser` C. `Get-UserAccount` D.
    `Find-DomainUser`

3.  Why use `-SearchBase`?

    A. To limit a directory search to a particular location/scope B. To
    reset passwords C. To install RSAT D. To rename a domain

4.  Why is this lab read-only?

    A. AD changes can affect organizational authentication and access B.
    PowerShell cannot modify AD C. CSV cannot store AD data D. Get
    commands require no permissions

------------------------------------------------------------------------

# Lab Complete

You have moved from PowerShell fundamentals into real administrative
reporting while keeping the practice safe and auditable.


Continue to:

[Lesson 21 — Microsoft 365](../lessons/lesson-21-microsoft-365.md)

