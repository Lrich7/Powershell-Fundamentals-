[lab-02-finding-commands.md](https://github.com/user-attachments/files/31521095/lab-02-finding-commands.md)

# Lab 02 --- Finding Commands

## Lab Objective

In this lab, you will practice discovering PowerShell commands rather
than memorizing them.

You will:

-   Use `Get-Command`.
-   Search with wildcards.
-   Search by verb and noun.
-   Identify command types.
-   Identify the module that provides a command.
-   Use command discovery to solve simple IT tasks.
-   Practice the **Find → Learn → Run** workflow.

------------------------------------------------------------------------

## Before You Begin

Complete:

-   Lesson 01 --- PowerShell Basics
-   Lesson 02 --- Finding Commands

This lab uses safe discovery and read-only commands.

------------------------------------------------------------------------

# Exercise 1 --- Explore Get-Command

Run:

``` powershell
Get-Command
```

You will probably receive a very large list.

Instead of trying to read all of it, run:

``` powershell
Get-Command Get-Service
```

Look for:

``` text
CommandType
Name
Version
Source
```

### Questions

What is the command type of `Get-Service` on your system?

``` text
Answer: ______________________
```

What is shown in the `Source` field?

``` text
Answer: ______________________
```

------------------------------------------------------------------------

# Exercise 2 --- Search with Wildcards

You remember that PowerShell has commands related to services, but you
do not remember their exact names.

Search:

``` powershell
Get-Command *Service*
```

Review the results.

Now search:

``` powershell
Get-Command *Process*
```

### Question

What does the `*` character allow you to do?

``` text
Answer:
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 3 --- Search by Noun

Find commands whose noun is `Service`:

``` powershell
Get-Command -Noun Service
```

You may see commands such as:

``` text
Get-Service
New-Service
Restart-Service
Set-Service
Start-Service
Stop-Service
```

### Questions

Which command sounds like it would retrieve services?

``` text
Answer: ______________________
```

Which command sounds like it would restart a service?

``` text
Answer: ______________________
```

Which one would you consider safer to explore on a production computer?

``` text
Answer: ______________________
```

Why?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 4 --- Search by Verb

Find commands that use the `Get` verb:

``` powershell
Get-Command -Verb Get
```

Now find commands that use:

``` text
Stop
```

You determine the correct `Get-Command` syntax.

Write it here:

``` powershell
# Your command:
```

Do the same for:

``` text
Restart
```

``` powershell
# Your command:
```

> **Do not run the Stop or Restart commands you discover.** You are only
> discovering their names.

------------------------------------------------------------------------

# Exercise 5 --- Combine Verb and Noun

Find the command that:

``` text
Retrieves + Services
```

Use both `-Verb` and `-Noun`.

``` powershell
# Your command:
```

Expected command discovered:

``` text
______________________
```

Now try to discover a command that:

``` text
Stops + Process
```

Again, **discover it only**.

``` powershell
# Your discovery command:
```

What command did you find?

``` text
______________________
```

------------------------------------------------------------------------

# Exercise 6 --- Command Types

Search for:

``` powershell
Get-Command cd
```

Then:

``` powershell
Get-Command Get-Service
```

Compare the `CommandType`.

You may encounter command types such as:

``` text
Alias
Function
Cmdlet
Application
```

### Questions

What type of command is `cd` in your PowerShell session?

``` text
Answer: ______________________
```

What type is `Get-Service`?

``` text
Answer: ______________________
```

------------------------------------------------------------------------

# Exercise 7 --- Find Commands from a Module

Run:

``` powershell
Get-Command -Module Microsoft.PowerShell.Management
```

Do not try to memorize the list.

Find three commands that look useful for IT administration.

``` text
1. ______________________________

2. ______________________________

3. ______________________________
```

### Think About It

What does a module appear to do?

``` text
Answer:
____________________________________________________
____________________________________________________
```

You will study modules in depth later.

------------------------------------------------------------------------

# Exercise 8 --- Discovery Scenario: Files

You are asked:

> Find PowerShell commands related to items or files. You do not
> remember the exact command.

Try a wildcard search containing:

``` text
Item
```

``` powershell
# Your command:
```

Look through the results.

Can you find the command used in Lesson 01 to retrieve child items in a
directory?

``` text
Answer: ______________________
```

Run that safe `Get` command.

------------------------------------------------------------------------

# Exercise 9 --- Discovery Scenario: Processes

Your supervisor asks:

> What PowerShell commands are available for working with processes?

Do **not** search the internet.

Use PowerShell itself.

Write the discovery command:

``` powershell
# Your command:
```

From the results, identify:

``` text
Command that retrieves processes: ____________________

Command that starts a process:     ____________________

Command that stops a process:      ____________________
```

Only run the command that retrieves information.

------------------------------------------------------------------------

# Exercise 10 --- Find → Learn → Run

You need to retrieve information about Windows services.

### Step 1 --- Find

Use `Get-Command` to find the appropriate command.

``` powershell
# Your discovery command:
```

### Step 2 --- Learn

Once you identify it, use:

``` powershell
Get-Help <command>
```

Replace `<command>` with the command you found.

### Step 3 --- Run

Run the safe command.

``` powershell
# Your final command:
```

This is the workflow you should begin using whenever you encounter an
unfamiliar PowerShell task:

``` text
Find → Learn → Run
```

------------------------------------------------------------------------

# End-of-Lab Challenge --- Command Detective

For each task, **do not start by guessing the final command**.

Use `Get-Command` to discover it.

## Task 1

Find a command that retrieves the current date.

``` text
Discovered command: ______________________
```

## Task 2

Find commands related to locations.

``` text
Discovery command:
__________________________________________

Command that retrieves your location:
__________________________________________
```

## Task 3

Find commands whose noun is `Process`.

``` text
Discovery command:
__________________________________________
```

Identify at least three:

``` text
1. ______________________
2. ______________________
3. ______________________
```

## Task 4

Find a command that retrieves PowerShell command history.

``` text
Discovered command: ______________________
```

## Task 5

Find all commands on your system containing:

``` text
Computer
```

How many useful-looking commands can you identify?

``` text
__________________________________________
```

> Do not run unfamiliar commands that appear capable of changing,
> restarting, or removing something.

------------------------------------------------------------------------

# Knowledge Check

1.  What is the primary PowerShell command for discovering commands?

    A. `Find-PowerShell`\
    B. `Get-Command`\
    C. `Search-Command`\
    D. `Get-Cmdlet`

2.  What does `*` represent in a wildcard search?

    A. Exactly one character\
    B. Zero or more characters\
    C. A required parameter\
    D. A module

3.  Which search is most useful when you know you want to work with
    services but do not know which actions are available?

    A. `Get-Command -Noun Service`\
    B. `Get-Service -All`\
    C. `Get-Help Service`\
    D. `Find-Service`

4.  What is the recommended workflow from the lesson?

    A. Run → Guess → Search\
    B. Memorize → Run → Troubleshoot\
    C. Find → Learn → Run\
    D. Search Internet → Copy → Run

5.  Why should you inspect the verb of an unfamiliar command before
    running it?

    A. The verb can help indicate whether the command retrieves
    information or makes a change.\
    B. Only `Get` commands accept parameters.\
    C. The verb identifies the Windows version.\
    D. PowerShell commands do not have nouns.

------------------------------------------------------------------------

# Lab Complete

You have practiced finding commands without needing to memorize their
names.

Next, you will go deeper into PowerShell's built-in documentation.

Continue to:

[Lab 03 --- Getting Help](lab-03-getting-help.md)
