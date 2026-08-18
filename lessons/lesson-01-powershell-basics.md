# https://learn.microsoft.com/en-us/powershell/scripting/learn/ps101/00-introduction?view=powershell-7.6 


## 📖 Learning Objectives

By the end of this lesson you should be able to:

- Explain what PowerShell is and how it is used in IT administration.
- Identify the PowerShell version running on your computer.
- Understand the basic Verb-Noun structure of PowerShell commands.
- Run basic PowerShell cmdlets and use simple parameters.
- Navigate files and folders from the PowerShell command line.
- Use tab completion and command history to work more efficiently.
- Recognize the difference between commands that retrieve information and commands that make changes.
- Understand at a basic level that PowerShell works with objects, not just text output.

---

1. Introduction to PowerShell

Briefly explain:
What PowerShell is
What administrators use it for
PowerShell vs Command Prompt
PowerShell vs Windows PowerShell 5.1
Why PowerShell is useful for IT administration



2. Opening PowerShell

Cover:

Windows Terminal
Windows PowerShell
PowerShell 7
Running normally vs Run as Administrator
How to tell which version you're running

#Powershell (maybe use screenshot)
#$PSVersionTable

Explain the important fields, especially PSVersion and PSEdition.

3. Understanding PowerShell Commands

Introduce the Verb-Noun format:

#Powershell (maybe use screenshot)
Get-Service
Get-Process
Get-Date
Get-Location

Explain that Get means retrieve information, while the noun tells PowerShell what you're working with.

Microsoft maintains a list of approved verbs, but at this point you only need a few such as:
Get
Set
New
Remove
Start
Stop
Restart

4. Running Your First Commands

Give several safe commands to experiment with:

#Powershell (maybe use screenshot)
Get-Date
Get-Location
Get-ChildItem
Get-Process
Get-Service
Get-ComputerInfo

Under each one, briefly explain what the command does.

For example:
#Powershell (maybe use screenshot)
Get-Process



5. Commands and Parameters

Introduce parameters without going too deep.

#Powershell (maybe use screenshot)
Get-Service -Name Spooler


Break it apart:

Get-Service    Command
-Name          Parameter
Spooler        Parameter value


Then another example:
#Powershell (maybe use screenshot)
Get-Process -Name explorer


I'd also introduce the concept of switches:
#Powershell (maybe use screenshot)
Get-ChildItem -Recurse

But save detailed parameter behavior for later.




6. Navigating the File System

This is worth teaching immediately because you'll use it constantly.

#Powershell (maybe use screenshot)
Get-Location
Get-ChildItem
Set-Location C:\
Set-Location ..



Also introduce the common aliases:

#Powershell (maybe use screenshot)
pwd
ls
cd

Explain an important best practice here:

Aliases are convenient when working interactively, but full command names are usually preferred when writing scripts because they're easier for other people to understand


7. Tab Completion

This should definitely be in Basics.

Have the learner type:

Get-Ser

and press Tab.

Then:
Get-Service -
and press Tab.


This teaches one of the easiest ways to explore PowerShell without memorizing everything.


8. Command History

Introduce:
#Powershell (maybe use screenshot)
Get-History

And keyboard shortcuts:

↑ — previous command
↓ — next command
Ctrl+C — stop a running command
Ctrl+L — clear the screen in many terminals

You can also mention:
#Powershell (maybe use screenshot)
Clear-Host



9. PowerShell Is Object-Based

I'd introduce this concept but not actually teach objects yet.

For example:
#Powershell (maybe use screenshot)
Get-Service


xplain:

PowerShell isn't simply displaying text. Get-Service returns objects containing information about services.

Then say:

We'll explore objects, properties, and methods in a later lesson.

That's enough for Lesson 1.

10. Basic Safety

Since you're learning this for IT administration, I'd put a small warning section here.

Explain the difference between commands that retrieve information:
#Powershell (maybe use screenshot)
Get-Service
Get-Process
Get-ChildItem

and commands that change something:
#Powershell (maybe use screenshot)
Stop-Service
Remove-Item
Set-Service
Restart-Computer

A simple beginner rule:

Commands beginning with Get are generally safe for exploration because they're retrieving information. Before running commands such as Set, Remove, Stop, or Restart, understand exactly what they will change.







# Summary

In this lesson, you learned:

- What PowerShell is
- How PowerShell commands are structured
- How to run basic cmdlets
- How parameters modify commands
- How to navigate the file system
- How to use tab completion
- How to view command history
- The basics of PowerShell objects
- Basic PowerShell safety

## Hands-On Lab

Ready to practice?

➡️ [Lab 01 — PowerShell Basics](../labs/lesson-01-lab-01-powershell-basics.md)



























