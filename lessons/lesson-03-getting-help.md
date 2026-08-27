[Uploading lesson-03-getting-help.md…]()

# Lesson 03 --- Getting Help

## Learning Objectives

By the end of this lesson, you will be able to:

-   Explain how PowerShell's built-in Help system supports command
    discovery and learning.
-   Use `Get-Help` to learn what a PowerShell command does.
-   Read the major sections of a PowerShell Help topic.
-   Use `-Examples`, `-Detailed`, and `-Full` to control the amount of
    Help displayed.
-   Find information about individual command parameters.
-   Search PowerShell Help when you do not know the exact command name.
-   Use `about_*` topics to learn PowerShell concepts.
-   Use `Update-Help` to download current Help content where supported.
-   Use online Help when additional or updated documentation is needed.

------------------------------------------------------------------------

## Why PowerShell Help Matters

PowerShell includes a built-in Help system designed to teach you how
commands and PowerShell concepts work.

You do **not** need to memorize the syntax and parameters for every
command.

Instead, learn how to ask PowerShell for the information you need.

The primary command for doing this is:

``` powershell
Get-Help
```

A useful habit is:

``` text
Find the command → Read the Help → Look at examples → Try the command
```

> **Key Idea:** Knowing how to use the Help system is more valuable than
> trying to memorize hundreds of commands and parameters.

------------------------------------------------------------------------

# Getting Basic Help

To view Help for a command, enter `Get-Help` followed by the command
name.

For example:

``` powershell
Get-Help Get-Service
```

Depending on the available Help content, PowerShell can provide
information such as:

-   Name
-   Synopsis
-   Syntax
-   Description
-   Parameters
-   Examples
-   Notes
-   Related links

Try another command:

``` powershell
Get-Help Get-Process
```

------------------------------------------------------------------------

# Understanding a Help Page

PowerShell Help pages contain several sections.

You do not need to memorize every section, but you should understand
what the major sections are used for.

## NAME

Shows the name of the command.

Example:

``` text
Get-Service
```

------------------------------------------------------------------------

## SYNOPSIS

Provides a short description of what the command does.

Think of the synopsis as the command's quick summary.

------------------------------------------------------------------------

## SYNTAX

Shows the valid ways the command can be used.

For example, you may see syntax containing:

``` text
[-Name <String[]>]
```

The syntax section helps you identify:

-   Available parameters
-   Required and optional parameters
-   Expected parameter values
-   Valid parameter combinations

PowerShell syntax may look complicated at first. Focus on recognizing
command names and parameters before worrying about every symbol.

------------------------------------------------------------------------

## DESCRIPTION

Provides a more complete explanation of what the command does.

If the synopsis does not give you enough information, read the
description.

------------------------------------------------------------------------

## PARAMETERS

Explains the parameters available for the command.

A parameter changes or controls the behavior of a command.

For example:

``` powershell
Get-Service -Name Spooler
```

In this command:

``` text
Get-Service
```

is the command, and:

``` text
-Name
```

is a parameter.

------------------------------------------------------------------------

## EXAMPLES

Shows examples of how the command can be used.

Examples are often one of the most useful parts of PowerShell Help
because they show actual command syntax.

------------------------------------------------------------------------

## RELATED LINKS

May contain links or references to related commands and documentation.

These can help you continue learning when the current Help page does not
answer your question.

------------------------------------------------------------------------

# Viewing Help Examples

When learning a new command, examples are often the best place to start.

Use:

``` powershell
Get-Help Get-Service -Examples
```

You can use the same pattern with other commands:

``` powershell
Get-Help Get-Process -Examples
```

``` powershell
Get-Help Stop-Service -Examples
```

> **Tip:** If you find a command but are unsure how to use it, try
> `Get-Help <command> -Examples`.

------------------------------------------------------------------------

# Viewing Detailed Help

Use `-Detailed` when the standard Help output does not provide enough
information.

``` powershell
Get-Help Get-Service -Detailed
```

Detailed Help provides additional information about the command and its
parameters.

A useful progression is:

``` powershell
Get-Help Get-Service
Get-Help Get-Service -Detailed
```

------------------------------------------------------------------------

# Viewing Full Help

Use `-Full` to display the most complete locally available Help
information.

``` powershell
Get-Help Get-Service -Full
```

Full Help can be useful when you need to investigate a command in more
depth.

You usually do not need to begin with `-Full`.

Start with basic Help or examples and increase the level of detail when
needed.

``` text
Basic Help
    ↓
Examples
    ↓
Detailed Help
    ↓
Full Help
```

------------------------------------------------------------------------

# Getting Help for a Parameter

Sometimes you already understand the command but need to know how a
particular parameter works.

For example:

``` powershell
Get-Help Get-Service -Parameter Name
```

This displays Help specifically for the `-Name` parameter.

Another example:

``` powershell
Get-Help Get-Process -Parameter Name
```

You can also request information about all parameters:

``` powershell
Get-Help Get-Service -Parameter *
```

This is useful when you want to quickly investigate the available
parameters without reading the entire Help topic.

------------------------------------------------------------------------

# Searching PowerShell Help

You may know what you want to accomplish without knowing the exact
command.

`Get-Help` can also help you search.

For example:

``` powershell
Get-Help *service*
```

This searches Help topics related to `service`.

You can try other terms:

``` powershell
Get-Help *process*
```

``` powershell
Get-Help *computer*
```

This complements the command-discovery skills from the previous lesson.

For example:

``` powershell
Get-Command *Service*
```

finds commands, while:

``` powershell
Get-Help *Service*
```

searches available Help topics.

------------------------------------------------------------------------

# Learning with about\_ Topics

PowerShell Help is not limited to individual commands.

It also includes conceptual Help topics.

These topics generally begin with:

``` text
about_
```

You can discover them with:

``` powershell
Get-Help about_*
```

Examples include topics related to:

-   Aliases
-   Arrays
-   Comparison operators
-   Execution policies
-   Functions
-   Pipelines
-   Variables
-   Wildcards

To open a specific topic, use its Help topic name.

For example:

``` powershell
Get-Help about_Aliases
```

Another useful topic is:

``` powershell
Get-Help about_Wildcards
```

> **Key Idea:** Command Help teaches you how to use a command. `about_*`
> Help topics teach you how PowerShell concepts work.

You will encounter many of these concepts in later lessons.

------------------------------------------------------------------------

# Updating PowerShell Help

PowerShell's detailed Help content may need to be downloaded or updated.

Use:

``` powershell
Update-Help
```

`Update-Help` downloads current Help content for modules that support
updateable Help.

Depending on your system and the modules being updated, you may need
elevated permissions.

You may also see errors for individual modules. This does not always
mean the entire update failed. Some modules may not provide updateable
Help or may have other requirements.

After updating Help, try:

``` powershell
Get-Help Get-Service -Full
```

> **Tip:** If a Help topic appears incomplete, updating Help is a good
> troubleshooting step.

------------------------------------------------------------------------

# Using Online Help

PowerShell can open the online documentation for commands that provide
an online Help link.

Use:

``` powershell
Get-Help Get-Service -Online
```

This opens the online Help page in your default web browser.

Online Help can be useful when:

-   Local Help is missing.
-   You want current documentation.
-   You want easier navigation through related documentation.
-   You need additional examples or notes.

Another example:

``` powershell
Get-Help Get-Process -Online
```

------------------------------------------------------------------------

# Get-Help vs. Get-Command

`Get-Command` and `Get-Help` work together, but they answer different
questions.

## Get-Command

Use `Get-Command` to answer:

> **What command should I use?**

Example:

``` powershell
Get-Command *Service*
```

## Get-Help

Use `Get-Help` to answer:

> **How do I use this command?**

Example:

``` powershell
Get-Help Get-Service
```

Then:

``` powershell
Get-Help Get-Service -Examples
```

Together, they form an important PowerShell discovery workflow.

------------------------------------------------------------------------

# A Practical Help Workflow

Suppose you need to work with processes but do not remember the exact
PowerShell commands.

## Step 1 --- Find Commands

``` powershell
Get-Command *Process*
```

You discover:

``` text
Get-Process
```

## Step 2 --- Read Basic Help

``` powershell
Get-Help Get-Process
```

## Step 3 --- Look at Examples

``` powershell
Get-Help Get-Process -Examples
```

## Step 4 --- Investigate a Parameter

``` powershell
Get-Help Get-Process -Parameter Name
```

## Step 5 --- Run the Command

``` powershell
Get-Process
```

The workflow becomes:

``` text
Find → Help → Examples → Run
```

This process can be repeated whenever you encounter an unfamiliar
PowerShell command.

------------------------------------------------------------------------

# Don't Memorize Everything

As you continue learning PowerShell, commands will become more familiar
through repeated use.

However, even experienced administrators regularly consult Help and
documentation.

Instead of trying to remember everything, practice asking questions such
as:

``` text
What command do I need?
        ↓
Get-Command

What does this command do?
        ↓
Get-Help

How do I use it?
        ↓
Get-Help -Examples

What does this parameter do?
        ↓
Get-Help -Parameter

I need more information.
        ↓
Get-Help -Detailed / -Full / -Online
```

Learning this workflow will make later PowerShell topics much easier.

------------------------------------------------------------------------

# Key Takeaways

-   `Get-Help` is PowerShell's built-in command Help system.
-   Basic Help provides an overview of a command.
-   `-Examples` displays practical examples.
-   `-Detailed` provides additional command information.
-   `-Full` displays the most complete locally available Help.
-   `-Parameter` displays information about individual parameters.
-   Wildcards can be used to search available Help topics.
-   `about_*` topics explain PowerShell concepts rather than individual
    commands.
-   `Update-Help` downloads updated Help content for supported modules.
-   `-Online` opens available online documentation.
-   `Get-Command` helps answer **"What command should I use?"**
-   `Get-Help` helps answer **"How do I use it?"**
-   You do not need to memorize PowerShell. Learn how to find reliable
    information.

------------------------------------------------------------------------

# Lab

Ready to practice using the PowerShell Help system?

Continue to:

[Lab 03 --- Getting Help](../labs/lab-03-getting-help.md)

In the lab, you will practice reading Help pages, finding examples,
investigating parameters, searching Help topics, and using PowerShell's
Help system to answer questions without being given every answer in
advance.

------------------------------------------------------------------------

## Additional Resources

-   [Get-Help --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/get-help?view=powershell-7.6)
-   [Update-Help --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/update-help?view=powershell-7.6)
-   [About the PowerShell Help
    System](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_help_system?view=powershell-7.6)
-   [PowerShell 101 --- The Help
    System](https://learn.microsoft.com/en-us/powershell/scripting/learn/ps101/02-help-system?view=powershell-7.6)
