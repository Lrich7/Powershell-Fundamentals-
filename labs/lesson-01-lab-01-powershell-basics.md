[lab-01-powershell-basics.md](https://github.com/user-attachments/files/31521059/lab-01-powershell-basics.md)

# Lab 01 --- PowerShell Basics

## Lab Objective

In this lab, you will practice the foundational skills from Lesson 01:

-   Identify your PowerShell version and edition.
-   Run safe information-gathering cmdlets.
-   Recognize the `Verb-Noun` command pattern.
-   Use basic parameters.
-   Navigate the file system.
-   Use aliases interactively.
-   Practice tab completion and command history.
-   Distinguish commands that retrieve information from commands that
    make changes.

------------------------------------------------------------------------

## Before You Begin

### Requirements

You need:

-   A Windows computer.
-   Windows PowerShell 5.1 or PowerShell 7.
-   A normal PowerShell session.

Administrator rights are **not required** for this lab.

> **Safety:** This lab is intentionally read-only. Do not experiment
> with commands such as `Remove-Item`, `Stop-Service`, or
> `Restart-Computer`.

------------------------------------------------------------------------

# Exercise 1 --- Identify Your PowerShell Environment

Open PowerShell or PowerShell 7.

Run:

``` powershell
$PSVersionTable
```

Find the following values:

``` text
PSVersion
PSEdition
```

Record them:

``` text
PowerShell Version: ______________________

PowerShell Edition: ______________________
```

### Think About It

If `PSEdition` shows:

``` text
Desktop
```

you are normally using Windows PowerShell.

If it shows:

``` text
Core
```

you are normally using modern PowerShell.

------------------------------------------------------------------------

# Exercise 2 --- Run Your First Commands

Run each command separately:

``` powershell
Get-Date
```

``` powershell
Get-Location
```

``` powershell
Get-ChildItem
```

``` powershell
Get-Process
```

``` powershell
Get-Service
```

If available on your system, also try:

``` powershell
Get-ComputerInfo
```

### Questions

1.  Which command displayed the current date and time?

``` text
Answer: ______________________
```

2.  Which command showed your current folder?

``` text
Answer: ______________________
```

3.  Which command displayed running processes?

``` text
Answer: ______________________
```

4.  What do all of these command names have in common?

``` text
Answer: ______________________
```

------------------------------------------------------------------------

# Exercise 3 --- Recognize Verb-Noun

Look at:

``` text
Get-Service
Get-Process
Get-Date
Get-Location
```

For:

``` powershell
Get-Service
```

identify:

``` text
Verb: ______________________
Noun: ______________________
```

Now do the same for:

``` powershell
Get-Process
```

``` text
Verb: ______________________
Noun: ______________________
```

### Challenge

Without running anything, which of these commands sounds like it would
**change** something rather than simply retrieve information?

``` text
Get-Service
Set-Service
Get-Process
Stop-Service
Get-ChildItem
Remove-Item
```

Write your choices:

``` text
________________________________________
```

------------------------------------------------------------------------

# Exercise 4 --- Use Parameters

Run:

``` powershell
Get-Service -Name Spooler
```

Break the command into its parts:

``` text
Command:         ______________________
Parameter:       ______________________
Parameter Value: ______________________
```

Now run:

``` powershell
Get-Process -Name explorer
```

If Explorer is running, PowerShell should return information about that
process.

Try:

``` powershell
Get-ChildItem -Recurse
```

> **Note:** If your current folder contains many files, press `Ctrl+C`
> to stop the command.

### Question

What did adding `-Recurse` change about `Get-ChildItem`?

``` text
Answer:
____________________________________________________
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 5 --- Navigate the File System

Display your current location:

``` powershell
Get-Location
```

List the contents:

``` powershell
Get-ChildItem
```

Move to the root of the C drive:

``` powershell
Set-Location C:\
```

Verify:

``` powershell
Get-Location
```

List the contents:

``` powershell
Get-ChildItem
```

Move into a folder that exists on your computer.

For example:

``` powershell
Set-Location C:\Users
```

Move up one level:

``` powershell
Set-Location ..
```

### Question

What does:

``` powershell
Set-Location ..
```

do?

``` text
Answer:
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 6 --- Try Common Aliases

Run:

``` powershell
pwd
```

Then:

``` powershell
ls
```

Try:

``` powershell
cd C:\
```

These are convenient aliases for interactive use.

Now compare:

``` powershell
pwd
```

with:

``` powershell
Get-Location
```

### Question

Why are full command names usually preferred in scripts?

``` text
Answer:
____________________________________________________
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 7 --- Use Tab Completion

Type:

``` text
Get-Ser
```

Do **not** press Enter.

Press:

``` text
Tab
```

PowerShell should attempt to complete the command.

Now type:

``` text
Get-Service -
```

Press `Tab` several times.

Watch PowerShell cycle through available parameter names.

### Try It Yourself

Start typing:

``` text
Get-Pro
```

Use tab completion to complete the command.

### Question

Why is tab completion useful?

``` text
Answer:
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 8 --- Command History

Run several harmless commands:

``` powershell
Get-Date
Get-Location
Get-Service
```

Now run:

``` powershell
Get-History
```

Use the:

``` text
↑
```

arrow key to move backward through commands you previously entered.

Use:

``` text
↓
```

to move forward again.

Try:

``` powershell
Clear-Host
```

Then press the up arrow.

### Question

Did clearing the screen erase your command history?

``` text
Answer: ______________________
```

------------------------------------------------------------------------

# Exercise 9 --- Basic Safety

For each command below, classify it as:

``` text
GET / READ
```

or:

``` text
CHANGE
```

  Command              Classification
  -------------------- ----------------
  `Get-Service`        
  `Get-Process`        
  `Get-ChildItem`      
  `Stop-Service`       
  `Remove-Item`        
  `Set-Service`        
  `Restart-Computer`   

> **Beginner Rule:** Commands beginning with `Get` are generally good
> commands for exploration because they retrieve information. Never
> assume that an unfamiliar command is safe just because of its
> name---use PowerShell Help before running it.

------------------------------------------------------------------------

# End-of-Lab Challenge

Complete the following tasks **without copying the commands from the
exercises above**.

1.  Display your PowerShell version.
2.  Display the current date and time.
3.  Determine your current folder.
4.  List the contents of the current folder.
5.  Display information about the `Spooler` service.
6.  Display information about the `explorer` process.
7.  Move to `C:\`.
8.  Move into `C:\Users`.
9.  Move back up one directory.
10. Display your command history.

### Bonus

Use tab completion for at least two of the commands rather than typing
the complete command name.

------------------------------------------------------------------------

# Knowledge Check

1.  What naming pattern do most PowerShell cmdlets follow?

    A. Noun-Verb\
    B. Verb-Noun\
    C. Command-Parameter\
    D. Object-Action

2.  Which command shows the current PowerShell version?

    A. `Get-Version`\
    B. `$PSVersionTable`\
    C. `Get-PowerShell`\
    D. `PowerShell -Version`

3.  What is `-Name` in this command?

``` powershell
Get-Service -Name Spooler
```

A. A cmdlet\
B. An alias\
C. A parameter\
D. A service

4.  Which command changes your current directory?

    A. `Get-Location`\
    B. `Set-Location`\
    C. `Get-ChildItem`\
    D. `Get-History`

5.  Why should you be cautious with commands beginning with verbs such
    as `Remove`, `Stop`, `Set`, or `Restart`?

    A. They cannot use parameters.\
    B. They may make changes to the system.\
    C. They only work in Command Prompt.\
    D. They always require internet access.

------------------------------------------------------------------------

# Lab Complete

You should now be comfortable with the basic PowerShell console and
several safe commands.

The next lab focuses on an essential PowerShell skill:

> **Finding commands instead of memorizing them.**

Continue to:

[Lesson 02 — Finding Commands](../lessons/lesson-02-finding-commands.md)

