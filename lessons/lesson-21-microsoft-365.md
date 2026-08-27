[lesson-21-microsoft-365.md](https://github.com/user-attachments/files/31518583/lesson-21-microsoft-365.md)

# Lesson 21 --- Microsoft 365 Administration with PowerShell

## Learning Objectives

By the end of this lesson, you will be able to:

-   Explain how PowerShell is used to administer Microsoft 365 services.
-   Understand that Microsoft 365 administration uses multiple
    PowerShell modules.
-   Recognize Microsoft Graph PowerShell, Exchange Online PowerShell,
    and other service-specific tooling.
-   Connect to Microsoft Graph PowerShell with appropriate scopes.
-   Discover Microsoft Graph commands.
-   Connect to Exchange Online PowerShell.
-   Retrieve basic user, group, mailbox, and licensing information.
-   Understand delegated permissions and least privilege.
-   Export Microsoft 365 information for reporting.
-   Disconnect sessions when appropriate.
-   Apply safe practices to cloud administration.

------------------------------------------------------------------------

## PowerShell in Microsoft 365

Microsoft 365 is made up of multiple cloud services.

Examples include:

``` text
Microsoft Entra ID
Exchange Online
Microsoft Teams
SharePoint Online
OneDrive
Microsoft 365 Groups
Licensing
```

PowerShell can help administrators manage and report on these services.

> **Key Idea:** There is not one single PowerShell module that manages
> every Microsoft 365 service. Administrators use Microsoft Graph
> PowerShell and service-specific modules depending on the task.

------------------------------------------------------------------------

# Microsoft Graph PowerShell

Microsoft Graph provides access to many Microsoft cloud resources.

The Microsoft Graph PowerShell SDK exposes Graph functionality through
PowerShell commands.

You may encounter modules beginning with:

``` text
Microsoft.Graph
```

Check availability:

``` powershell
Get-Module -ListAvailable Microsoft.Graph*
```

Discover commands:

``` powershell
Get-Command -Module Microsoft.Graph.Users
```

The exact submodules installed depend on your environment and
installation method.

------------------------------------------------------------------------

# Installing Microsoft Graph PowerShell

If authorized to install modules, Microsoft Graph PowerShell can be
obtained from the PowerShell Gallery.

A common installation command is:

``` powershell
Install-Module Microsoft.Graph -Scope CurrentUser
```

Modern environments may also use PSResourceGet.

Before installing modules on a managed computer, follow organizational
policy and verify the module source.

------------------------------------------------------------------------

# Connecting to Microsoft Graph

A common connection command is:

``` powershell
Connect-MgGraph
```

Graph uses permissions called:

``` text
scopes
```

For example, a read-only connection might request:

``` powershell
Connect-MgGraph -Scopes 'User.Read.All'
```

The permissions required depend on the command and data being accessed.

------------------------------------------------------------------------

# Least Privilege

Do not request broad permissions simply because they are available.

A good rule is:

> Request only the permissions required for the administrative task.

For reporting, prefer read-only scopes when possible.

Some scopes require administrator consent.

Your signed-in account must also be authorized for the operation.

------------------------------------------------------------------------

# Checking the Graph Context

After connecting:

``` powershell
Get-MgContext
```

This can show information such as:

-   Account
-   Tenant
-   Authentication type
-   Granted scopes

This is useful for confirming which identity and permissions are
currently in use.

------------------------------------------------------------------------

# Finding Microsoft 365 Users

With the appropriate Graph permissions:

``` powershell
Get-MgUser
```

Select useful properties:

``` powershell
Get-MgUser -All |
    Select-Object DisplayName,
        UserPrincipalName,
        AccountEnabled
```

Microsoft Graph often returns a default set of properties.

Request additional properties when needed.

Example:

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

------------------------------------------------------------------------

# Microsoft 365 Groups

Microsoft Graph can retrieve groups:

``` powershell
Get-MgGroup
```

Example:

``` powershell
Get-MgGroup -All |
    Select-Object DisplayName, Mail, SecurityEnabled, MailEnabled
```

Group types and behavior vary, so do not assume every group is the same
type of Microsoft 365 resource.

------------------------------------------------------------------------

# Licensing Information

Licensing data can also be accessed through Microsoft Graph with
appropriate permissions.

For example, you may encounter commands such as:

``` powershell
Get-MgSubscribedSku
```

This can help identify tenant subscription SKUs.

User licensing information may be exposed through user license
properties or related Graph commands.

Because licensing structures and product identifiers can change, inspect
the objects:

``` powershell
Get-MgSubscribedSku | Get-Member
```

and consult current Microsoft documentation when building licensing
automation.

------------------------------------------------------------------------

# Exchange Online PowerShell

Exchange Online administration commonly uses the:

``` text
ExchangeOnlineManagement
```

module.

Check whether it is available:

``` powershell
Get-Module -ListAvailable ExchangeOnlineManagement
```

Connect:

``` powershell
Connect-ExchangeOnline
```

The exact authentication experience depends on the environment and
account.

------------------------------------------------------------------------

# Finding Mailboxes

After connecting to Exchange Online:

``` powershell
Get-EXOMailbox
```

Example:

``` powershell
Get-EXOMailbox -ResultSize 20 |
    Select-Object DisplayName,
        UserPrincipalName,
        RecipientTypeDetails
```

Exchange Online provides many commands for mailboxes, recipients,
permissions, and configuration.

------------------------------------------------------------------------

# Shared Mailboxes

You can identify shared mailboxes:

``` powershell
Get-EXOMailbox `
    -RecipientTypeDetails SharedMailbox `
    -ResultSize Unlimited |
    Select-Object DisplayName,
        PrimarySmtpAddress
```

This is useful for inventory and reporting.

Before changing mailbox permissions or recipient configuration, verify
the mailbox and approved access.

------------------------------------------------------------------------

# Mailbox Permissions

Exchange provides commands for inspecting permissions.

For example, depending on the permission type, you may use commands such
as:

``` powershell
Get-MailboxPermission
```

or other Exchange recipient permission commands.

Permission management can affect access to company email.

Start with read-only inspection and consult command help before making
changes.

------------------------------------------------------------------------

# Disconnecting Exchange Online

When finished:

``` powershell
Disconnect-ExchangeOnline
```

This closes the Exchange Online PowerShell connection.

------------------------------------------------------------------------

# Microsoft Teams PowerShell

Microsoft Teams also has PowerShell administration tooling.

Check availability:

``` powershell
Get-Module -ListAvailable MicrosoftTeams
```

Commands and capabilities evolve over time.

Use:

``` powershell
Get-Command -Module MicrosoftTeams
```

and current Microsoft documentation to discover supported administrative
tasks.

------------------------------------------------------------------------

# SharePoint Online PowerShell

SharePoint Online has its own administrative PowerShell tooling.

Depending on the task and environment, administrators may encounter the
SharePoint Online Management Shell and Microsoft Graph-based approaches.

Because Microsoft 365 management evolves, always verify which supported
module applies to the specific service and task.

------------------------------------------------------------------------

# Legacy Modules

Older Microsoft 365 documentation may reference modules such as:

``` text
AzureAD
MSOnline
```

For new automation, do not assume old examples represent the current
recommended approach.

Microsoft Graph PowerShell is the primary direction for many Microsoft
Entra and Microsoft 365 identity scenarios.

When maintaining older scripts, plan migrations carefully and verify
current Microsoft guidance.

------------------------------------------------------------------------

# Discover Before You Automate

The discovery workflow from the beginning of this course still applies.

For Graph:

``` powershell
Get-Command Get-MgUser
Get-Help Get-MgUser
```

For Exchange:

``` powershell
Get-Command Get-EXOMailbox
Get-Help Get-EXOMailbox
```

For a module:

``` powershell
Get-Command -Module ExchangeOnlineManagement
```

The Microsoft 365 command surface is large.

You do not need to memorize every cmdlet.

------------------------------------------------------------------------

# Exporting a User Report

Example with Microsoft Graph:

``` powershell
$users = Get-MgUser -All `
    -Property DisplayName,
        UserPrincipalName,
        AccountEnabled,
        Department

$users |
    Select-Object DisplayName,
        UserPrincipalName,
        Department,
        AccountEnabled |
    Sort-Object DisplayName |
    Export-Csv C:\Temp\m365-users.csv -NoTypeInformation
```

This combines cloud administration with the reporting skills from Lesson
11.

------------------------------------------------------------------------

# Exporting a Mailbox Report

Example:

``` powershell
Get-EXOMailbox -ResultSize Unlimited |
    Select-Object DisplayName,
        UserPrincipalName,
        PrimarySmtpAddress,
        RecipientTypeDetails |
    Sort-Object DisplayName |
    Export-Csv C:\Temp\mailboxes.csv -NoTypeInformation
```

Read-only reports are excellent first Microsoft 365 PowerShell projects.

------------------------------------------------------------------------

# Cloud Administration Safety

Cloud PowerShell commands can affect users across an entire tenant.

Before a change:

``` text
Confirm tenant
   ↓
Confirm signed-in account
   ↓
Confirm permissions/scopes
   ↓
Retrieve target
   ↓
Inspect target
   ↓
Test with read-only commands
   ↓
Make the smallest necessary change
   ↓
Verify
   ↓
Document
```

A command that targets the wrong tenant or wrong set of users can have
company-wide consequences.

------------------------------------------------------------------------

# Authentication and Secrets

Avoid storing:

``` text
Passwords
Client secrets
Access tokens
Certificates with private keys
```

directly in scripts or Git repositories.

Interactive learning examples can use approved sign-in methods.

Production automation should use secure authentication methods
appropriate to the environment.

------------------------------------------------------------------------

# Practical Microsoft 365 Reporting Project

A useful beginner project is to create separate reports for:

``` text
Users
Groups
Shared mailboxes
Licensing
```

For example:

``` powershell
$reportFolder = 'C:\Temp\M365Reports'

if (-not (Test-Path $reportFolder)) {
    New-Item -Path $reportFolder -ItemType Directory
}
```

Then export read-only data from the services you are authorized to
access.

This is safer and more educational than beginning with bulk account
changes.

------------------------------------------------------------------------

# Key Takeaways

-   Microsoft 365 administration spans multiple services and PowerShell
    modules.
-   Microsoft Graph PowerShell is important for many Microsoft cloud
    identity and directory tasks.
-   `Connect-MgGraph` connects to Graph with requested permission
    scopes.
-   `Get-MgContext` helps verify the current Graph connection.
-   `Get-MgUser` and `Get-MgGroup` retrieve cloud directory objects.
-   Request the least privilege needed for a task.
-   Exchange Online commonly uses the `ExchangeOnlineManagement` module.
-   `Connect-ExchangeOnline` establishes an Exchange Online session.
-   `Get-EXOMailbox` retrieves mailbox information.
-   `Disconnect-ExchangeOnline` closes the Exchange connection.
-   Teams and SharePoint have service-specific administration tooling.
-   Older AzureAD and MSOnline examples should not automatically be used
    for new automation.
-   Start Microsoft 365 PowerShell learning with read-only reports.
-   Never commit credentials, secrets, or tokens to source control.
-   Always verify the tenant, account, permissions, and targets before
    making cloud changes.

------------------------------------------------------------------------

# Lab

Continue to:

[Lab 21 --- Microsoft 365](../labs/lab-21-microsoft-365.md)

The lab should use an authorized Microsoft 365 tenant. It will focus on
connecting safely, discovering Graph and Exchange commands, retrieving
users, groups, and mailbox information, inspecting permissions/context,
and exporting read-only reports.

------------------------------------------------------------------------

## Additional Resources

-   [Microsoft Graph PowerShell Documentation --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/microsoftgraph/)
-   [Microsoft Graph PowerShell SDK Overview --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/microsoftgraph/overview)
-   [Connect-MgGraph --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.graph.authentication/connect-mggraph)
-   [Get-MgUser --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.graph.users/get-mguser)
-   [Exchange Online PowerShell --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/exchange/exchange-online-powershell)
-   [Connect-ExchangeOnline --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/exchangepowershell/connect-exchangeonline)
-   [Get-EXOMailbox --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/exchangepowershell/get-exomailbox)
-   [Microsoft Teams PowerShell Overview --- Microsoft
    Learn](https://learn.microsoft.com/en-us/microsoftteams/teams-powershell-overview)
