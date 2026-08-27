[lab-10-files-and-folders.md](https://github.com/user-attachments/files/31521720/lab-10-files-and-folders.md)
# Lab 10 --- Files and Folders

## Lab Objective

In this lab, you will build and manage a **safe practice file
structure** with PowerShell.

You will:

-   Navigate the file system.
-   Create folders and files.
-   Work with absolute and relative paths.
-   Use `Join-Path`.
-   Test paths.
-   Write and append file content.
-   Copy, move, and rename items.
-   Filter and sort file objects.
-   Use `-WhatIf` before deleting data.
-   Complete a small file-management project.

------------------------------------------------------------------------

## Before You Begin

Complete Lessons 01--10.

### Safety Rule

This lab creates its own practice directory so you do not need to work
with important files.

Use:

``` powershell
$labRoot = Join-Path $HOME 'PowerShell-Lab10'
```

All creation, copying, moving, and deletion exercises should remain
inside this folder.

> **Do not substitute a production, shared, or important folder.**

------------------------------------------------------------------------

# Exercise 1 --- Create the Lab Folder

Create the path variable:

``` powershell
$labRoot = Join-Path $HOME 'PowerShell-Lab10'
```

Check whether it exists:

``` powershell
Test-Path $labRoot
```

If it does not exist, create it:

``` powershell
New-Item -Path $labRoot -ItemType Directory
```

Move into it:

``` powershell
Set-Location $labRoot
```

Verify:

``` powershell
Get-Location
```

------------------------------------------------------------------------

# Exercise 2 --- Build a Folder Structure

Create:

``` text
PowerShell-Lab10
├── Data
├── Logs
├── Archive
└── Reports
```

Use `New-Item`.

Example:

``` powershell
New-Item -Path (Join-Path $labRoot 'Data') -ItemType Directory
```

Create the remaining folders yourself.

Verify:

``` powershell
Get-ChildItem $labRoot -Directory
```

------------------------------------------------------------------------

# Exercise 3 --- Create Files

Create three text files in `Data`.

Suggested names:

``` text
computer1.txt
computer2.txt
computer3.txt
```

Use:

``` powershell
New-Item
```

Example:

``` powershell
$dataPath = Join-Path $labRoot 'Data'
New-Item -Path (Join-Path $dataPath 'computer1.txt') -ItemType File
```

Create the other two.

------------------------------------------------------------------------

# Exercise 4 --- Write Content

Write:

``` text
PC-001
```

to `computer1.txt`.

Example:

``` powershell
Set-Content `
    -Path (Join-Path $dataPath 'computer1.txt') `
    -Value 'PC-001'
```

Write different computer names to the other files.

Read them:

``` powershell
Get-Content (Join-Path $dataPath 'computer1.txt')
```

------------------------------------------------------------------------

# Exercise 5 --- Append Content

Append:

``` text
Status: Active
```

to `computer1.txt`:

``` powershell
Add-Content `
    -Path (Join-Path $dataPath 'computer1.txt') `
    -Value 'Status: Active'
```

Read the file again.

### Question

What is the difference between `Set-Content` and `Add-Content`?

``` text
____________________________________________________
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 6 --- Inspect File Objects

Run:

``` powershell
Get-ChildItem $dataPath -File
```

Then:

``` powershell
Get-ChildItem $dataPath -File |
    Get-Member
```

Display:

``` powershell
Get-ChildItem $dataPath -File |
    Select-Object Name, Length, CreationTime, LastWriteTime
```

------------------------------------------------------------------------

# Exercise 7 --- Copy a File

Copy:

``` text
computer1.txt
```

to `Archive`.

Create:

``` powershell
$archivePath = Join-Path $labRoot 'Archive'
```

Then copy the file.

``` powershell
# Your Copy-Item command:
```

Verify:

``` powershell
Get-ChildItem $archivePath
```

### Question

Does the original file still exist in `Data`?

``` text
Answer: ______________________
```

------------------------------------------------------------------------

# Exercise 8 --- Rename the Copy

Rename the copied file in `Archive` to:

``` text
computer1-backup.txt
```

Use:

``` powershell
Rename-Item
```

``` powershell
# Your command:
```

Verify:

``` powershell
Get-ChildItem $archivePath
```

------------------------------------------------------------------------

# Exercise 9 --- Move a File

Move:

``` text
computer3.txt
```

from `Data` to `Archive`.

Use:

``` powershell
Move-Item
```

``` powershell
# Your command:
```

Verify both folders afterward.

### Question

How is `Move-Item` different from `Copy-Item`?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 10 --- Create Log Files

Create:

``` powershell
$logPath = Join-Path $labRoot 'Logs'
```

Create several files:

``` text
system.log
application.log
backup.log
notes.txt
```

You may use `New-Item`.

Add a line of sample content to each.

------------------------------------------------------------------------

# Exercise 11 --- Filter Log Files

Display only `.log` files:

``` powershell
Get-ChildItem $logPath -File |
    Where-Object Extension -eq '.log'
```

Now try source filtering:

``` powershell
Get-ChildItem $logPath -File -Filter '*.log'
```

### Question

Which version filters at the source?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 12 --- Sort Files

Sort log files by name:

``` powershell
Get-ChildItem $logPath -File |
    Sort-Object Name
```

Now sort files by:

``` text
LastWriteTime
```

newest first.

``` powershell
# Your command:
```

------------------------------------------------------------------------

# Exercise 13 --- Test Paths

Test:

``` powershell
Test-Path $dataPath
```

Test a file that exists.

``` powershell
# Your command:
```

Then test:

``` text
does-not-exist.txt
```

inside the lab folder.

``` powershell
# Your command:
```

Record:

``` text
Existing path result: ____________________
Missing path result:  ____________________
```

------------------------------------------------------------------------

# Exercise 14 --- Preview a Deletion

List the archive:

``` powershell
Get-ChildItem $archivePath
```

Choose one practice file.

Preview its removal:

``` powershell
Remove-Item `
    -Path (Join-Path $archivePath 'computer1-backup.txt') `
    -WhatIf
```

### Question

Did `-WhatIf` actually delete the file?

``` text
Answer: ______________________
```

Explain why `-WhatIf` is useful:

``` text
____________________________________________________
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 15 --- Delete a Practice File

Only after confirming that you are targeting a file under:

``` text
$labRoot
```

remove one practice file.

First verify the path:

``` powershell
Get-Item (Join-Path $archivePath 'computer1-backup.txt')
```

Preview:

``` powershell
Remove-Item `
    -Path (Join-Path $archivePath 'computer1-backup.txt') `
    -WhatIf
```

Then, if correct:

``` powershell
Remove-Item `
    -Path (Join-Path $archivePath 'computer1-backup.txt')
```

Verify that it is gone.

------------------------------------------------------------------------

# End-of-Lab Challenge --- Build a File Report

IT wants a report of the files inside your Lab 10 folder.

Your report should:

1.  Search all files below `$labRoot`.
2.  Sort them from largest to smallest.
3.  Display only:
    -   `Name`
    -   `Directory`
    -   `Length`
    -   `LastWriteTime`
4.  Return the first 10 results.

Build it yourself.

``` powershell
# Your solution:
```

### Bonus Challenge --- Log Inventory

Create a pipeline that:

1.  Searches the lab recursively.
2.  Keeps only `.log` files.
3.  Sorts them by name.
4.  Displays `Name`, `FullName`, and `Length`.

``` powershell
# Your solution:
```

------------------------------------------------------------------------

# Cleanup

You may keep the lab files for later practice.

If you choose to remove the entire lab folder, **verify the path
first**:

``` powershell
$labRoot
```

Then preview:

``` powershell
Remove-Item $labRoot -Recurse -WhatIf
```

Only if the displayed path is exactly your practice folder should you
consider:

``` powershell
Remove-Item $labRoot -Recurse
```

You do not need to perform the cleanup.

------------------------------------------------------------------------

# Knowledge Check

1.  Which command lists files and folders?

    A. `Get-ChildItem`\
    B. `Get-ItemList`\
    C. `Show-Files`\
    D. `List-Directory`

2.  Which command checks whether a path exists?

    A. `Get-Path`\
    B. `Test-Path`\
    C. `Check-Item`\
    D. `Find-Path`

3.  What does `Add-Content` do?

    A. Replaces all existing content\
    B. Appends content\
    C. Deletes a file\
    D. Renames a file

4.  Which command moves an item?

    A. `Move-Item`\
    B. `Copy-Item`\
    C. `Set-Location`\
    D. `Rename-Item`

5.  What is the purpose of `-WhatIf`?

    A. Automatically backs up files\
    B. Previews what a supported command would do\
    C. Forces the command to run\
    D. Opens Help

------------------------------------------------------------------------

# Lab Complete

You have now used PowerShell to manage a complete practice file
structure.

The next part of the course moves into working with structured data and
increasingly realistic administrative tasks.


Continue to:

### [Project 02 — File & Folder Inventory Tool](../projects/project-02-file-folder-inventory.md)

Build a read-only PowerShell tool that inventories files, analyzes file sizes, filters results, and identifies the largest files in a folder.

> **Tip:** Try solving the requirements using what you have learned before looking for outside examples.

