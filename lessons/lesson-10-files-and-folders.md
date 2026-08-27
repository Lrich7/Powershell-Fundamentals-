[lesson-10-files-and-folders.md](https://github.com/user-attachments/files/31517205/lesson-10-files-and-folders.md)

# Lesson 10 --- Files and Folders

## Learning Objectives

By the end of this lesson, you will be able to:

-   Explain how PowerShell works with the file system.
-   Identify your current location with `Get-Location`.
-   Navigate folders with `Set-Location`.
-   List files and folders with `Get-ChildItem`.
-   Use relative and absolute paths.
-   Create new files and folders with `New-Item`.
-   Copy files and folders with `Copy-Item`.
-   Move and rename items with `Move-Item` and `Rename-Item`.
-   Remove files and folders with `Remove-Item`.
-   Test whether a path exists with `Test-Path`.
-   Read text files with `Get-Content`.
-   Write and append text with `Set-Content` and `Add-Content`.
-   Filter and sort file-system objects using the PowerShell pipeline.
-   Use `-WhatIf` when supported to preview potentially destructive file
    operations.

------------------------------------------------------------------------

## PowerShell and the File System

Working with files and folders is one of the most common administrative
tasks in PowerShell.

PowerShell provides commands for:

-   Navigating folders
-   Listing files
-   Creating folders
-   Creating files
-   Copying files
-   Moving files
-   Renaming files
-   Deleting files
-   Reading file contents
-   Writing to files
-   Searching and filtering file-system objects

Many of these commands use the noun:

``` text
Item
```

or:

``` text
ChildItem
```

For example:

``` powershell
Get-ChildItem
New-Item
Copy-Item
Move-Item
Rename-Item
Remove-Item
```

> **Key Idea:** PowerShell treats files and folders as objects, so the
> object, pipeline, filtering, and sorting skills from earlier lessons
> also apply to the file system.

------------------------------------------------------------------------

# Finding Your Current Location

Your current PowerShell location determines where relative file-system
commands operate.

Use:

``` powershell
Get-Location
```

You may see output similar to:

``` text
Path
----
C:\Users\Username
```

The current location is sometimes called the:

``` text
working directory
```

or:

``` text
current directory
```

------------------------------------------------------------------------

# pwd

PowerShell provides an alias:

``` powershell
pwd
```

for:

``` powershell
Get-Location
```

If you are learning PowerShell, it is generally better to become
familiar with the full command names first.

Aliases are convenient, but full cmdlet names make scripts easier to
understand.

------------------------------------------------------------------------

# Changing Your Location

Use:

``` powershell
Set-Location
```

to change your current location.

For example:

``` powershell
Set-Location C:\Windows
```

Then check:

``` powershell
Get-Location
```

------------------------------------------------------------------------

# cd

PowerShell supports:

``` powershell
cd
```

as an alias for:

``` powershell
Set-Location
```

For example:

``` powershell
cd C:\Windows
```

Again, aliases are useful interactively, but learning the full
PowerShell command names helps you understand what the command is
actually doing.

------------------------------------------------------------------------

# Moving Up One Folder

The path:

``` text
..
```

represents the parent directory.

For example:

``` powershell
Set-Location ..
```

moves up one folder.

You can also use:

``` powershell
cd ..
```

------------------------------------------------------------------------

# Listing Files and Folders

Use:

``` powershell
Get-ChildItem
```

to list the items in the current location.

For example:

``` powershell
Get-ChildItem
```

This can return both files and directories.

You can also specify a path:

``` powershell
Get-ChildItem C:\Windows
```

------------------------------------------------------------------------

# dir and ls

PowerShell provides familiar aliases for `Get-ChildItem`, including:

``` powershell
dir
```

and:

``` powershell
ls
```

All of these can refer to `Get-ChildItem` in PowerShell.

For training and scripts, prefer:

``` powershell
Get-ChildItem
```

because its purpose is clearer.

------------------------------------------------------------------------

# Files and Folders Are Objects

Run:

``` powershell
Get-ChildItem
```

Then inspect the returned objects:

``` powershell
Get-ChildItem | Get-Member
```

File and directory objects contain useful properties.

Depending on the item type, you may see properties such as:

``` text
Name
FullName
Extension
Length
CreationTime
LastWriteTime
Directory
Attributes
```

This means you can filter, sort, and select files just like other
PowerShell objects.

------------------------------------------------------------------------

# Showing Only Files

Use:

``` powershell
Get-ChildItem -File
```

This returns only files.

For example:

``` powershell
Get-ChildItem C:\Windows -File
```

------------------------------------------------------------------------

# Showing Only Directories

Use:

``` powershell
Get-ChildItem -Directory
```

This returns only directories.

For example:

``` powershell
Get-ChildItem C:\Windows -Directory
```

------------------------------------------------------------------------

# Searching Recursively

Use:

``` text
-Recurse
```

to search through subdirectories.

For example:

``` powershell
Get-ChildItem C:\Temp -Recurse
```

This searches `C:\Temp` and its subfolders.

> **Caution:** Recursive searches can return a very large amount of data
> when used on large directory trees.

You can limit the search to files:

``` powershell
Get-ChildItem C:\Temp -File -Recurse
```

------------------------------------------------------------------------

# Paths

A **path** tells PowerShell where a file or folder is located.

For example:

``` text
C:\Users\Username\Documents\report.txt
```

PowerShell commonly works with two types of paths:

``` text
Absolute paths
Relative paths
```

------------------------------------------------------------------------

# Absolute Paths

An absolute path specifies the full location of an item.

For example:

``` text
C:\Temp\Logs\system.log
```

This identifies the location regardless of your current working
directory.

Example:

``` powershell
Get-ChildItem C:\Temp\Logs
```

------------------------------------------------------------------------

# Relative Paths

A relative path is interpreted based on your current location.

For example:

``` text
.\Logs
```

The:

``` text
.
```

represents the current location.

If your current location is:

``` text
C:\Temp
```

then:

``` text
.\Logs
```

refers to:

``` text
C:\Temp\Logs
```

------------------------------------------------------------------------

# Current and Parent Directory Symbols

Two useful path symbols are:

``` text
.    Current directory
..   Parent directory
```

For example:

``` powershell
Get-ChildItem .
```

lists the current directory.

And:

``` powershell
Get-ChildItem ..
```

lists the parent directory.

------------------------------------------------------------------------

# Paths with Spaces

Paths may contain spaces.

For example:

``` text
C:\Program Files
```

When a path contains spaces, place it inside quotation marks:

``` powershell
Get-ChildItem 'C:\Program Files'
```

Using quotes around paths is a good habit when the path may contain
spaces or special characters.

------------------------------------------------------------------------

# Joining Paths

PowerShell provides:

``` powershell
Join-Path
```

to combine path components.

For example:

``` powershell
$folder = 'C:\Temp'
$file = 'report.txt'

Join-Path $folder $file
```

This returns a properly combined path.

You can store it:

``` powershell
$path = Join-Path $folder $file
```

Then:

``` powershell
$path
```

This is often safer and clearer than manually building paths as strings.

------------------------------------------------------------------------

# Creating a Folder

Use:

``` powershell
New-Item
```

to create files and folders.

To create a directory:

``` powershell
New-Item -Path C:\Temp\PowerShellLab -ItemType Directory
```

You can also store the result:

``` powershell
$folder = New-Item -Path C:\Temp\PowerShellLab -ItemType Directory
```

Because `New-Item` returns an object, you can inspect it:

``` powershell
$folder | Get-Member
```

------------------------------------------------------------------------

# Creating a File

To create an empty file:

``` powershell
New-Item -Path C:\Temp\PowerShellLab\example.txt -ItemType File
```

You can verify that it exists with:

``` powershell
Get-ChildItem C:\Temp\PowerShellLab
```

------------------------------------------------------------------------

# Testing Whether a Path Exists

Before working with a file or folder, it can be useful to check whether
it exists.

Use:

``` powershell
Test-Path
```

For example:

``` powershell
Test-Path C:\Temp
```

The result is a Boolean value:

``` text
True
```

or:

``` text
False
```

You can store the result:

``` powershell
$exists = Test-Path C:\Temp
```

This will become especially useful when you begin writing scripts with
conditions.

------------------------------------------------------------------------

# Copying Files

Use:

``` powershell
Copy-Item
```

to copy an item.

For example:

``` powershell
Copy-Item -Path C:\Temp\example.txt -Destination C:\Temp\Backup\
```

The original file remains in place.

------------------------------------------------------------------------

# Copying and Renaming at the Same Time

You can copy a file to a new name:

``` powershell
Copy-Item `
    -Path C:\Temp\example.txt `
    -Destination C:\Temp\example-backup.txt
```

This creates a copy with a different filename.

------------------------------------------------------------------------

# Copying Folders

You can copy folders as well.

For example:

``` powershell
Copy-Item `
    -Path C:\Temp\PowerShellLab `
    -Destination C:\Temp\PowerShellLabBackup `
    -Recurse
```

`-Recurse` includes the folder's contents and subfolders.

------------------------------------------------------------------------

# Moving Files

Use:

``` powershell
Move-Item
```

to move an item.

For example:

``` powershell
Move-Item `
    -Path C:\Temp\example.txt `
    -Destination C:\Temp\Archive\
```

Unlike `Copy-Item`, the original item is moved to the new location.

------------------------------------------------------------------------

# Renaming Files and Folders

Use:

``` powershell
Rename-Item
```

to rename an item.

For example:

``` powershell
Rename-Item `
    -Path C:\Temp\old-name.txt `
    -NewName new-name.txt
```

You can also rename directories:

``` powershell
Rename-Item `
    -Path C:\Temp\OldFolder `
    -NewName NewFolder
```

------------------------------------------------------------------------

# Removing Files

Use:

``` powershell
Remove-Item
```

to delete an item.

For example:

``` powershell
Remove-Item C:\Temp\example.txt
```

> **Caution:** `Remove-Item` can permanently delete files. Do not assume
> deleted items will be sent to the Windows Recycle Bin.

Always verify the path before running a removal command.

------------------------------------------------------------------------

# Using -WhatIf

Many PowerShell commands that make changes support:

``` text
-WhatIf
```

`-WhatIf` shows what PowerShell **would do** without actually performing
the operation.

For example:

``` powershell
Remove-Item C:\Temp\example.txt -WhatIf
```

This is an excellent safety tool when learning PowerShell.

Before running a potentially destructive command, try:

``` powershell
Remove-Item C:\Temp\PowerShellLab -Recurse -WhatIf
```

Review the result before deciding whether to run the command without
`-WhatIf`.

> **Best Practice:** Use `-WhatIf` whenever possible when testing
> commands that rename, move, overwrite, or delete important data.

------------------------------------------------------------------------

# Removing a Folder

To remove an empty folder:

``` powershell
Remove-Item C:\Temp\PowerShellLab
```

If the directory contains items, you may need:

``` powershell
Remove-Item C:\Temp\PowerShellLab -Recurse
```

Preview it first:

``` powershell
Remove-Item C:\Temp\PowerShellLab -Recurse -WhatIf
```

Be especially careful with:

``` text
-Recurse
```

because it can affect everything below the specified path.

------------------------------------------------------------------------

# Reading Text Files

Use:

``` powershell
Get-Content
```

to read a text file.

For example:

``` powershell
Get-Content C:\Temp\notes.txt
```

PowerShell returns the contents of the file.

You can store the result:

``` powershell
$content = Get-Content C:\Temp\notes.txt
```

Then:

``` powershell
$content
```

------------------------------------------------------------------------

# Reading the First Few Lines

`Get-Content` can limit the amount of content returned.

For example:

``` powershell
Get-Content C:\Temp\large-log.txt -TotalCount 10
```

This returns the first ten lines.

You may also encounter:

``` powershell
Get-Content C:\Temp\large-log.txt -Tail 10
```

which returns the last ten lines.

This is useful when examining log files.

------------------------------------------------------------------------

# Writing Text with Set-Content

Use:

``` powershell
Set-Content
```

to write content to a file.

For example:

``` powershell
Set-Content `
    -Path C:\Temp\notes.txt `
    -Value 'PowerShell training notes'
```

If the file does not exist, PowerShell can create it.

If the file already contains text, `Set-Content` replaces the existing
content.

> **Important:** `Set-Content` overwrites the current file contents.

------------------------------------------------------------------------

# Appending Text with Add-Content

Use:

``` powershell
Add-Content
```

when you want to add text without replacing the existing content.

For example:

``` powershell
Add-Content `
    -Path C:\Temp\notes.txt `
    -Value 'Lesson 10 complete.'
```

The new text is added to the file.

------------------------------------------------------------------------

# Set-Content vs. Add-Content

A useful way to remember the difference is:

``` text
Set-Content
    ↓
Set or replace the file contents

Add-Content
    ↓
Append additional content
```

Be careful when using `Set-Content` with an existing file.

------------------------------------------------------------------------

# Clearing File Contents

Use:

``` powershell
Clear-Content
```

to remove the contents of a file without deleting the file itself.

For example:

``` powershell
Clear-Content C:\Temp\notes.txt
```

The file remains, but its contents are cleared.

------------------------------------------------------------------------

# Filtering Files by Extension

Because files are objects, you can use `Where-Object`.

For example:

``` powershell
Get-ChildItem |
    Where-Object Extension -eq '.txt'
```

Or use wildcard matching:

``` powershell
Get-ChildItem |
    Where-Object Name -like '*.txt'
```

You can also use `Get-ChildItem` parameters directly where appropriate:

``` powershell
Get-ChildItem -Filter '*.txt'
```

Remember the earlier guideline:

> Filter at the source when a command provides an appropriate filtering
> parameter.

------------------------------------------------------------------------

# Sorting Files

You can sort files by name:

``` powershell
Get-ChildItem -File |
    Sort-Object Name
```

Or by modification time:

``` powershell
Get-ChildItem -File |
    Sort-Object LastWriteTime -Descending
```

This displays recently modified files first.

------------------------------------------------------------------------

# Sorting Files by Size

File objects contain a:

``` text
Length
```

property.

You can sort files from largest to smallest:

``` powershell
Get-ChildItem -File |
    Sort-Object Length -Descending
```

Then select the largest files:

``` powershell
Get-ChildItem -File |
    Sort-Object Length -Descending |
    Select-Object -First 10 Name, Length
```

------------------------------------------------------------------------

# Displaying Useful File Properties

The default file listing does not show every available property.

Use `Select-Object`:

``` powershell
Get-ChildItem -File |
    Select-Object Name, Extension, Length, LastWriteTime
```

You can inspect all available properties with:

``` powershell
Get-ChildItem -File |
    Select-Object -First 1 |
    Get-Member
```

------------------------------------------------------------------------

# Finding Recently Modified Files

Because `LastWriteTime` is a `DateTime` value, you can compare it with
another date.

For example:

``` powershell
$cutoff = (Get-Date).AddDays(-7)
```

Then:

``` powershell
Get-ChildItem -File |
    Where-Object LastWriteTime -ge $cutoff
```

This returns files modified within approximately the last seven days in
the current directory.

You can combine it with recursive searching:

``` powershell
Get-ChildItem -File -Recurse |
    Where-Object LastWriteTime -ge $cutoff
```

Use recursive searches carefully on large directory structures.

------------------------------------------------------------------------

# Finding Large Files

You can filter by the `Length` property.

For example:

``` powershell
Get-ChildItem -File |
    Where-Object Length -gt 1MB
```

PowerShell supports size suffixes such as:

``` text
KB
MB
GB
```

For example:

``` powershell
1KB
1MB
1GB
```

This makes file-size comparisons easier to read.

A useful pipeline is:

``` powershell
Get-ChildItem -File |
    Where-Object Length -gt 10MB |
    Sort-Object Length -Descending |
    Select-Object Name, Length
```

------------------------------------------------------------------------

# Working with File Paths in Variables

Paths are often stored in variables.

For example:

``` powershell
$source = 'C:\Temp\PowerShellLab'
$destination = 'C:\Temp\Backup'
```

Then:

``` powershell
Copy-Item `
    -Path $source `
    -Destination $destination `
    -Recurse
```

This makes commands easier to update and reuse.

You can also build paths:

``` powershell
$logFolder = 'C:\Temp\Logs'
$logFile = Join-Path $logFolder 'training.log'
```

Then:

``` powershell
$logFile
```

------------------------------------------------------------------------

# A Safe File-Management Workflow

Suppose you want to clean up old files.

A safe workflow is:

## Step 1 --- Identify the Location

``` powershell
$folder = 'C:\Temp\PowerShellLab'
```

## Step 2 --- Verify It Exists

``` powershell
Test-Path $folder
```

## Step 3 --- Inspect the Files

``` powershell
Get-ChildItem $folder -File
```

## Step 4 --- Build the Filter

``` powershell
$cutoff = (Get-Date).AddDays(-30)

Get-ChildItem $folder -File |
    Where-Object LastWriteTime -lt $cutoff
```

## Step 5 --- Preview the Removal

``` powershell
Get-ChildItem $folder -File |
    Where-Object LastWriteTime -lt $cutoff |
    Remove-Item -WhatIf
```

## Step 6 --- Review Before Making Changes

Only remove `-WhatIf` after confirming that the command targets exactly
the files you intend to remove.

> **Key Idea:** With file operations, discovery and verification should
> happen before modification or deletion.

------------------------------------------------------------------------

# File Management and the Pipeline

The file system demonstrates how the PowerShell concepts from earlier
lessons work together.

``` text
Get files
    ↓
Get-ChildItem

Inspect objects
    ↓
Get-Member

Filter objects
    ↓
Where-Object

Sort objects
    ↓
Sort-Object

Choose properties
    ↓
Select-Object

Take action
    ↓
Copy-Item / Move-Item / Remove-Item
```

For example:

``` powershell
Get-ChildItem C:\Temp -File |
    Where-Object Extension -eq '.log' |
    Sort-Object LastWriteTime -Descending |
    Select-Object Name, Length, LastWriteTime
```

This creates a simple report of log files without changing anything.

------------------------------------------------------------------------

# Use Discovery Before Making Changes

If you are unsure how a file-management command works, use the discovery
tools from earlier lessons.

For example:

``` powershell
Get-Help Remove-Item
```

Then:

``` powershell
Get-Help Remove-Item -Examples
```

And:

``` powershell
Get-Help Remove-Item -Parameter WhatIf
```

Likewise:

``` powershell
Get-Command *-Item
```

can help you discover other commands that work with items.

This reinforces the course's core workflow:

``` text
Find → Learn → Inspect → Test → Change
```

------------------------------------------------------------------------

# Key Takeaways

-   `Get-Location` displays your current PowerShell location.
-   `Set-Location` changes your current location.
-   `Get-ChildItem` lists files and directories.
-   Files and folders are PowerShell objects with properties and
    methods.
-   `-File` returns files and `-Directory` returns directories.
-   `-Recurse` searches through subdirectories.
-   Absolute paths specify a full location.
-   Relative paths are based on the current location.
-   `.` represents the current directory and `..` represents the parent
    directory.
-   `Join-Path` safely combines path components.
-   `New-Item` creates files and directories.
-   `Test-Path` checks whether a path exists.
-   `Copy-Item` copies items.
-   `Move-Item` moves items.
-   `Rename-Item` renames items.
-   `Remove-Item` deletes items and should be used carefully.
-   `-WhatIf` can preview many potentially destructive operations.
-   `Get-Content` reads text files.
-   `Set-Content` writes or replaces file contents.
-   `Add-Content` appends content.
-   `Clear-Content` clears a file without deleting it.
-   File objects can be filtered, sorted, and selected with the same
    pipeline tools used throughout PowerShell.
-   A good file-management habit is:

``` text
Find → Verify → Preview → Change
```

------------------------------------------------------------------------

# Lab

Ready to practice managing files and folders with PowerShell?

Continue to:

[Lab 10 --- Files and Folders](../labs/lab-10-files-and-folders.md)

In the lab, you will navigate the file system, create a safe practice
directory, create and edit files, inspect file objects, copy and move
items, filter and sort files, test paths, and use `-WhatIf` before
performing removal operations.

------------------------------------------------------------------------

# 🚀 Project Checkpoint

You have completed Lessons 06–10. Now combine your skills with filtering, sorting, variables, arrays, operators, and the filesystem.

### [Project 02 — File & Folder Inventory Tool](../projects/project-02-file-folder-inventory.md)

Build a read-only PowerShell tool that inventories files, analyzes file sizes, filters results, and identifies the largest files in a folder.

> **Tip:** Try solving the requirements using what you have learned before looking for outside examples.

------------------------------------------------------------------------

## Additional Resources

-   [Get-ChildItem --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-childitem?view=powershell-7.6)
-   [Set-Location --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/set-location?view=powershell-7.6)
-   [New-Item --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/new-item?view=powershell-7.6)
-   [Copy-Item --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/copy-item?view=powershell-7.6)
-   [Move-Item --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/move-item?view=powershell-7.6)
-   [Rename-Item --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/rename-item?view=powershell-7.6)
-   [Remove-Item --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/remove-item?view=powershell-7.6)
-   [Test-Path --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/test-path?view=powershell-7.6)
-   [Get-Content --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-content?view=powershell-7.6)
-   [Set-Content --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/set-content?view=powershell-7.6)
-   [Add-Content --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/add-content?view=powershell-7.6)
-   [About FileSystem Provider --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_filesystem_provider?view=powershell-7.6)
