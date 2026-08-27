[lab-21-microsoft-365.md](https://github.com/user-attachments/files/31521931/lab-21-microsoft-365.md)
# Lab 21 --- Microsoft 365 Administration with PowerShell

## Lab Objective

This is an **environment-dependent, read-only lab**.

If you have authorized access to a Microsoft 365 tenant, you will
practice:

-   Discovering Microsoft Graph PowerShell commands.
-   Connecting to Microsoft Graph with read-only scopes.
-   Verifying your tenant, account, and granted scopes.
-   Retrieving Microsoft 365 users and groups.
-   Reviewing tenant license SKU information.
-   Discovering Exchange Online PowerShell.
-   Connecting to Exchange Online.
-   Retrieving mailbox and shared mailbox information.
-   Exporting read-only Microsoft 365 reports.
-   Disconnecting cleanly.
-   Applying least-privilege and cloud-administration safety practices.

You will **not** create, delete, disable, license, or modify users or
mailboxes in this lab.

------------------------------------------------------------------------

## Safety and Requirements

Use only a Microsoft 365 tenant you are authorized to administer or
query.

Before performing any cloud administrative task, verify:

``` text
Tenant
Account
Permissions / scopes
Target
Expected result
```

Do not store:

``` text
Passwords
Client secrets
Access tokens
Private keys
```

inside scripts, notes, or Git repositories.

> **Important:** This lab begins with read-only reporting. Do not
> broaden requested scopes or permissions simply to make an exercise
> work.

------------------------------------------------------------------------

# Exercise 1 --- Check Microsoft Graph PowerShell

Check for installed Microsoft Graph modules:

``` powershell
Get-Module -ListAvailable Microsoft.Graph*
```

If the modules are present, inspect a few:

``` powershell
Get-Module -ListAvailable Microsoft.Graph* |
    Sort-Object Name |
    Select-Object Name, Version, Path
```

If they are not installed, do not install them on a managed computer
without authorization.

### Question

Why should you verify a module's source and your organization's policy
before installing it?

``` text
____________________________________________________
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 2 --- Discover Graph Commands

If available, inspect commands from a Graph submodule:

``` powershell
Get-Command -Module Microsoft.Graph.Users
```

Search specifically for:

``` text
Get-MgUser
Connect-MgGraph
Get-MgContext
```

Use:

``` powershell
Get-Command Get-MgUser
```

and:

``` powershell
Get-Help Get-MgUser
```

### Question

What PowerShell discovery workflow are you using?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 3 --- Connect with Read-Only Scope

If authorized, connect using the least privilege needed for user
reporting.

For example:

``` powershell
Connect-MgGraph -Scopes 'User.Read.All'
```

The exact sign-in and consent behavior depends on your tenant.

Do not request broader permissions unless the exercise actually requires
them and the tenant administrator has approved them.

------------------------------------------------------------------------

# Exercise 4 --- Verify the Graph Context

After connecting:

``` powershell
Get-MgContext
```

Record:

``` text
Signed-in account:
________________________________________

Tenant:
________________________________________

Relevant granted scope(s):
________________________________________
```

### Safety Question

Why should you verify the tenant **before** running administrative
commands?

``` text
____________________________________________________
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 5 --- Retrieve Users

With appropriate permissions:

``` powershell
Get-MgUser -All |
    Select-Object DisplayName,
        UserPrincipalName,
        AccountEnabled
```

Now request department as an additional property:

``` powershell
Get-MgUser -All `
    -Property Id,
        DisplayName,
        UserPrincipalName,
        AccountEnabled,
        Department |
    Select-Object DisplayName,
        UserPrincipalName,
        Department,
        AccountEnabled
```

### Challenge

Create a pipeline that shows only enabled users and sorts them by
display name.

``` powershell
# Your command:
```

------------------------------------------------------------------------

# Exercise 6 --- Export a User Report

Create a safe report folder:

``` powershell
$reportFolder = Join-Path $HOME 'PowerShell-Lab21'

if (-not (Test-Path $reportFolder)) {
    New-Item -Path $reportFolder -ItemType Directory
}
```

Export a read-only user report:

``` powershell
$userReport = Join-Path $reportFolder 'm365-users.csv'
```

Build the pipeline yourself using:

``` text
Get-MgUser
Select-Object
Sort-Object
Export-Csv
```

Suggested fields:

``` text
DisplayName
UserPrincipalName
Department
AccountEnabled
```

``` powershell
# Your solution:
```

------------------------------------------------------------------------

# Exercise 7 --- Retrieve Microsoft 365 Groups

With appropriate permissions, retrieve groups:

``` powershell
Get-MgGroup -All |
    Select-Object DisplayName,
        Mail,
        SecurityEnabled,
        MailEnabled
```

### Questions

How many groups are returned?

``` text
________________________________________
```

Do all groups have the same combination of:

``` text
SecurityEnabled
MailEnabled
```

``` text
Yes / No
```

What does that suggest?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 8 --- License SKU Information

If your permissions allow it:

``` powershell
Get-MgSubscribedSku
```

Inspect the returned object:

``` powershell
Get-MgSubscribedSku | Get-Member
```

Select a few useful fields available in your tenant.

``` powershell
# Your command:
```

> Do not assume SKU names or product identifiers will remain unchanged
> forever. Microsoft cloud licensing evolves over time.

------------------------------------------------------------------------

# Exercise 9 --- Check Exchange Online PowerShell

Check:

``` powershell
Get-Module -ListAvailable ExchangeOnlineManagement
```

Discover:

``` powershell
Get-Command Connect-ExchangeOnline -ErrorAction SilentlyContinue
Get-Command Get-EXOMailbox -ErrorAction SilentlyContinue
```

If the module is unavailable, complete the Exchange review questions
instead of installing it without approval.

------------------------------------------------------------------------

# Exercise 10 --- Connect to Exchange Online

If authorized:

``` powershell
Connect-ExchangeOnline
```

Verify that you are using the intended account and tenant.

Then retrieve a small mailbox sample:

``` powershell
Get-EXOMailbox -ResultSize 20 |
    Select-Object DisplayName,
        UserPrincipalName,
        RecipientTypeDetails
```

------------------------------------------------------------------------

# Exercise 11 --- Shared Mailboxes

Retrieve shared mailboxes:

``` powershell
Get-EXOMailbox `
    -RecipientTypeDetails SharedMailbox `
    -ResultSize Unlimited |
    Select-Object DisplayName,
        PrimarySmtpAddress
```

Count them:

``` powershell
Get-EXOMailbox `
    -RecipientTypeDetails SharedMailbox `
    -ResultSize Unlimited |
    Measure-Object
```

### Question

Why is shared-mailbox inventory a good beginner Microsoft 365 PowerShell
task?

``` text
____________________________________________________
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 12 --- Export a Mailbox Report

Create:

``` powershell
$mailboxReport = Join-Path $reportFolder 'mailboxes.csv'
```

Export a read-only report containing:

``` text
DisplayName
UserPrincipalName
PrimarySmtpAddress
RecipientTypeDetails
```

Sort by display name.

``` powershell
# Your solution:
```

------------------------------------------------------------------------

# Exercise 13 --- Disconnect Exchange Online

When finished:

``` powershell
Disconnect-ExchangeOnline
```

This is a good session-cleanup habit.

------------------------------------------------------------------------

# Exercise 14 --- Legacy Module Awareness

Search your available modules for:

``` text
AzureAD
MSOnline
```

If you find them, do not assume they are the preferred tools for new
automation.

### Question

What is the modern direction for many Microsoft cloud identity scenarios
covered in the lesson?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# End-of-Lab Project --- Microsoft 365 Audit Pack

## Scenario

> IT leadership wants a current, read-only Microsoft 365 administrative
> snapshot. No tenant changes are authorized.

Create a PowerShell script:

``` text
m365-audit.ps1
```

The script should create reports for as many of these as your
permissions allow:

``` text
users.csv
groups.csv
shared-mailboxes.csv
mailboxes.csv
license-skus.csv
```

## Requirements

``` text
[ ] Verifies required module availability
[ ] Connects only with approved/read-only permissions
[ ] Displays or verifies Graph context
[ ] Retrieves users
[ ] Retrieves groups
[ ] Retrieves licensing SKU information where permitted
[ ] Retrieves Exchange mailbox data where permitted
[ ] Creates a report folder
[ ] Sorts and selects useful properties
[ ] Uses Export-Csv
[ ] Uses error handling
[ ] Disconnects Exchange Online when finished
[ ] Makes no Microsoft 365 changes
[ ] Stores no credentials or secrets in the script
```

### Suggested Script Flow

``` text
Check Dependencies
      ↓
Connect
      ↓
Verify Context
      ↓
Collect
      ↓
Filter / Sort / Select
      ↓
Export
      ↓
Report Success / Errors
      ↓
Disconnect
```

------------------------------------------------------------------------

# Offline Practice Track --- No Microsoft 365 Tenant

If you do not have an authorized tenant, create simulated data:

``` powershell
$users = @(
    [PSCustomObject]@{
        DisplayName       = 'Alex Smith'
        UserPrincipalName = 'alex@example.com'
        Department        = 'IT'
        AccountEnabled    = $true
    }
    [PSCustomObject]@{
        DisplayName       = 'Jordan Lee'
        UserPrincipalName = 'jordan@example.com'
        Department        = 'Operations'
        AccountEnabled    = $true
    }
    [PSCustomObject]@{
        DisplayName       = 'Former User'
        UserPrincipalName = 'former@example.com'
        Department        = ''
        AccountEnabled    = $false
    }
)
```

Practice:

1.  Find enabled users.
2.  Find disabled users.
3.  Find blank departments.
4.  Sort users by display name.
5.  Export a CSV.
6.  Build a summary count.

Then create simulated mailbox objects:

``` powershell
$mailboxes = @(
    [PSCustomObject]@{
        DisplayName          = 'Alex Smith'
        PrimarySmtpAddress   = 'alex@example.com'
        RecipientTypeDetails = 'UserMailbox'
    }
    [PSCustomObject]@{
        DisplayName          = 'IT Support'
        PrimarySmtpAddress   = 'itsupport@example.com'
        RecipientTypeDetails = 'SharedMailbox'
    }
)
```

Find and export only shared mailboxes.

------------------------------------------------------------------------

# Knowledge Check

1.  What does `Connect-MgGraph` do?

    A. Connects PowerShell to Microsoft Graph B. Installs Microsoft
    365 C. Creates a mailbox D. Enables AD DS

2.  Why should scopes follow least privilege?

    A. To request only permissions actually needed B. To make commands
    run faster only C. To avoid using objects D. To replace
    authentication

3.  Which command helps confirm the current Graph account, tenant, and
    scopes?

    A. `Get-MgContext` B. `Get-MgUser` C. `Get-EXOMailbox` D.
    `Get-Module`

4.  Which module commonly provides Exchange Online PowerShell commands?

    A. `ExchangeOnlineManagement` B. `Microsoft.Graph.Users` C.
    `ActiveDirectory` D. `CimCmdlets`

5.  What is the safest first Microsoft 365 automation project?

    A. Bulk disabling accounts B. Read-only reporting C. Deleting
    mailboxes D. Removing licenses

------------------------------------------------------------------------

# Lab Complete

Continue to:

[Lab 22 --- Automation Projects](lab-22-automation-projects.md)
