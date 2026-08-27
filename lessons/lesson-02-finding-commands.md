[lesson-02-finding-commands.md](https://github.com/user-attachments/files/31516052/lesson-02-finding-commands.md)

# Lesson 02 --- Finding Commands

## Learning Objectives

By the end of this lesson, you will be able to:

-   Explain why command discovery is an important PowerShell skill.
-   Use `Get-Command` to find available PowerShell commands.
-   Search for commands by name, verb, noun, and module.
-   Use wildcards when you only know part of a command name.
-   Use `Get-Help` to learn how a command works.
-   View examples, detailed help, full help, and parameter-specific
    help.
-   Update the local PowerShell Help system.
-   Read basic PowerShell command syntax.
-   Use a simple **Find → Learn → Run** workflow when working with
    unfamiliar commands.

------------------------------------------------------------------------

## Why Command Discovery Matters

PowerShell contains thousands of commands, and additional modules can
add even more.

You do **not** need to memorize every PowerShell command.

Instead, one of the most important PowerShell skills is learning how to
find the command you need and then determine how to use it.

PowerShell provides built-in tools specifically for this purpose.

Two of the most important are:

``` powershell
Get-Command
Get-Help
```

Throughout this lesson, you will use these commands to discover other
PowerShell commands and learn how they work.

> **Key Idea:** Good PowerShell users do not memorize every command.
> They learn how to find the right command when they need it.

------------------------------------------------------------------------

## The PowerShell Verb-Noun Naming Convention

As introduced in Lesson 1, PowerShell cmdlets generally follow a
**Verb-Noun** naming convention.

For example:

``` powershell
Get-Service
Get-Process
Stop-Service
Restart-Computer
Get-ChildItem
```

The **verb** describes the action being performed.

The **noun** describes the object or resource the command works with.

For example:

``` text
Get-Service
│   │
│   └── Noun: Service
│
└────── Verb: Get
```

This predictable naming system makes PowerShell commands easier to
discover.

If you know you need to work with services, you can search for commands
containing the noun `Service`.

If you know you want to retrieve information, you can search for
commands using the verb `Get`.

------------------------------------------------------------------------

# Finding Commands with Get-Command

`Get-Command` is one of the primary tools used to discover commands
available in PowerShell.

## List Available Commands

Run:

``` powershell
Get-Command
```

This displays commands available in your current PowerShell environment.

Depending on your system, the list may be very large.

`Get-Command` can return several command types, including:

-   Cmdlets
-   Functions
-   Aliases
-   Scripts
-   Applications

Because there may be thousands of available commands, you will usually
search rather than display everything.

------------------------------------------------------------------------

## Search for a Specific Command

If you already know the command name, you can search for it directly:

``` powershell
Get-Command Get-Service
```

PowerShell returns information about the command, such as its command
type, name, version, and source.

------------------------------------------------------------------------

## Search Using Wildcards

Wildcards are useful when you know only part of a command name.

The `*` wildcard represents zero or more characters.

For example:

``` powershell
Get-Command *Service*
```

This searches for commands containing the word `Service`.

Another example:

``` powershell
Get-Command *Process*
```

This can help you discover commands related to processes even if you do
not know their exact names.

You can also search for commands ending with a particular noun:

``` powershell
Get-Command -Name *-Process
```

> **Tip:** When you know what you want to manage but do not know the
> exact command, try searching with a wildcard.

------------------------------------------------------------------------

## Search by Verb

PowerShell commands use standardized verbs such as:

-   `Get`
-   `Set`
-   `New`
-   `Remove`
-   `Start`
-   `Stop`
-   `Restart`

You can search for commands that use a particular verb:

``` powershell
Get-Command -Verb Get
```

This displays commands that use the `Get` verb.

Another example:

``` powershell
Get-Command -Verb Stop
```

This displays commands designed to stop something.

------------------------------------------------------------------------

## Search by Noun

You can also search by noun.

For example:

``` powershell
Get-Command -Noun Service
```

This displays commands whose noun is `Service`.

You might see commands such as:

``` text
Get-Service
New-Service
Restart-Service
Set-Service
Start-Service
Stop-Service
```

This is useful when you know **what** you want to manage but are not
sure what actions are available.

------------------------------------------------------------------------

## Search by Verb and Noun

You can combine the `-Verb` and `-Noun` parameters to make your search
more specific.

``` powershell
Get-Command -Verb Get -Noun Service
```

This searches for commands that:

-   Use the verb `Get`
-   Use the noun `Service`

The result should include:

``` text
Get-Service
```

------------------------------------------------------------------------

# Finding Commands in a Module

PowerShell commands are often organized into **modules**.

A module is a package containing related PowerShell commands and other
resources.

You will learn more about modules later. For now, it is useful to know
that `Get-Command` can show the commands provided by a particular
module.

For example:

``` powershell
Get-Command -Module Microsoft.PowerShell.Management
```

This lists commands available from the `Microsoft.PowerShell.Management`
module.

You can also inspect the module associated with a specific command:

``` powershell
Get-Command Get-Service
```

Look at the **Source** column in the output.

> **Note:** Do not worry about memorizing module names yet. The
> important concept is that PowerShell commands can be grouped into
> modules.

------------------------------------------------------------------------

# Learning About Commands with Get-Help

Finding a command is only the first step.

Once you find a command, you need to know:

-   What does it do?
-   What parameters does it accept?
-   Which parameters are required?
-   What syntax should I use?
-   Are there examples?

PowerShell's built-in Help system answers these questions.

The primary command is:

``` powershell
Get-Help
```

------------------------------------------------------------------------

## Basic Help

To view help for a command:

``` powershell
Get-Help Get-Service
```

The Help system can display information such as:

-   Command name
-   Synopsis
-   Syntax
-   Description
-   Parameters
-   Related links

------------------------------------------------------------------------

## View Detailed Help

For additional information, use `-Detailed`:

``` powershell
Get-Help Get-Service -Detailed
```

This provides more information about the command and its parameters.

------------------------------------------------------------------------

## View Examples

One of the most useful ways to learn a new PowerShell command is to view
examples.

``` powershell
Get-Help Get-Service -Examples
```

This displays examples showing how the command can be used.

> **Tip:** When learning a new command, `Get-Help <command> -Examples`
> is often one of the fastest ways to understand it.

------------------------------------------------------------------------

## View Full Help

To display all available Help information:

``` powershell
Get-Help Get-Service -Full
```

This provides the most complete local Help view for the command.

------------------------------------------------------------------------

## Get Help for a Specific Parameter

Sometimes you understand the command but need more information about one
parameter.

For example:

``` powershell
Get-Help Get-Service -Parameter Name
```

This displays Help specifically for the `-Name` parameter.

You can also use a wildcard to display information about all parameters:

``` powershell
Get-Help Get-Service -Parameter *
```

------------------------------------------------------------------------

# Updating PowerShell Help

PowerShell Help content may not be fully installed or may become
outdated.

You can download updated Help content with:

``` powershell
Update-Help
```

Depending on your environment and the modules being updated, you may
need to run PowerShell with elevated permissions.

If you receive errors for individual modules, that does not necessarily
mean the entire Help update failed. Some modules may not support
downloadable Help or may require additional permissions.

After updating Help, try:

``` powershell
Get-Help Get-Service -Full
```

------------------------------------------------------------------------

# Understanding Command Syntax

PowerShell can show you the syntax required to run a command.

For example:

``` powershell
Get-Command Get-Service -Syntax
```

You may see syntax similar to:

``` text
Get-Service [[-Name] <String[]>] [<CommonParameters>]
```

At first, PowerShell syntax can look complicated. You do not need to
understand every symbol immediately.

Here are a few important pieces.

## Parameters

A parameter begins with a hyphen:

``` text
-Name
```

Parameters allow you to control how a command behaves.

Example:

``` powershell
Get-Service -Name Spooler
```

Here, `-Name` is a parameter.

------------------------------------------------------------------------

## Parameter Values

Some parameters expect a value.

For example:

``` text
<String[]>
```

`String` means the parameter expects text.

The `[]` after the data type indicates that it can accept more than one
value.

You will learn more about PowerShell data types later.

------------------------------------------------------------------------

## Optional Items

Items displayed inside square brackets in syntax documentation are
generally optional.

For example:

``` text
[-Name <String[]>]
```

This means the `-Name` parameter is not always required.

> **Important:** Do not type the syntax documentation's square brackets
> unless the command specifically requires brackets as part of a value.

------------------------------------------------------------------------

## Parameter Sets

Some commands can be used in several different ways.

When you view syntax, you may see multiple syntax lines.

Each line represents a different **parameter set**.

You do not need to master parameter sets yet. For now, remember:

> Multiple syntax lines mean the command supports multiple valid
> combinations of parameters.

------------------------------------------------------------------------

# The Find → Learn → Run Workflow

A useful habit when working with PowerShell is:

``` text
Find → Learn → Run
```

### Step 1 --- Find the Command

Suppose you need to work with Windows services but do not remember the
command.

Search for it:

``` powershell
Get-Command *Service*
```

### Step 2 --- Learn the Command

Once you find `Get-Service`, learn how it works:

``` powershell
Get-Help Get-Service
```

Then look at examples:

``` powershell
Get-Help Get-Service -Examples
```

You can also inspect its syntax:

``` powershell
Get-Command Get-Service -Syntax
```

### Step 3 --- Run the Command

Now run it:

``` powershell
Get-Service
```

This simple workflow can be reused throughout your PowerShell training:

``` powershell
# Find
Get-Command *Service*

# Learn
Get-Help Get-Service -Examples

# Run
Get-Service
```

------------------------------------------------------------------------

# Key Takeaways

-   You do not need to memorize every PowerShell command.
-   PowerShell cmdlets generally follow a predictable **Verb-Noun**
    naming convention.
-   `Get-Command` helps you discover available commands.
-   Wildcards such as `*` help when you know only part of a command
    name.
-   `Get-Command -Verb` searches by action.
-   `Get-Command -Noun` searches by the resource being managed.
-   `Get-Command -Module` shows commands provided by a module.
-   `Get-Help` explains how a command works.
-   `Get-Help -Examples` is especially useful when learning an
    unfamiliar command.
-   `Update-Help` downloads updated Help content where supported.
-   `Get-Command <command> -Syntax` displays command syntax.
-   A useful PowerShell workflow is **Find → Learn → Run**.

------------------------------------------------------------------------

# Lab

Ready to practice finding commands?

Continue to:

[Lab 02 --- Finding Commands](../labs/lab-02-finding-commands.md)

In the lab, you will practice searching for commands, using PowerShell
Help, reading command syntax, and solving command-discovery challenges
without being given every command in advance.

------------------------------------------------------------------------

## Additional Resources

-   [Discover
    PowerShell](https://learn.microsoft.com/en-us/powershell/scripting/discover-powershell?view=powershell-7.6)
-   [Get-Command](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/get-command?view=powershell-7.6)
-   [Get-Help](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/get-help?view=powershell-7.6)
-   [About Command
    Syntax](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_command_syntax?view=powershell-7.6)
-   [About
    Modules](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_modules?view=powershell-7.6)
