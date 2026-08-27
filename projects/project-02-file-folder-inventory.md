# Project 02 --- File and Folder Inventory Tool

## Project Checkpoint

**Recommended after:** Lessons 06--10

This project gives you less command-by-command guidance than Project 01.

------------------------------------------------------------------------

## Scenario

A user reports that a folder is taking up too much space. IT needs a
safe way to review its contents before deciding whether anything should
be archived or removed.

Build a **read-only** PowerShell inventory tool.

------------------------------------------------------------------------

## Skills Practiced

-   Filtering and sorting
-   Variables
-   Arrays and collections
-   Operators
-   Files and folders
-   Pipeline processing
-   Object properties

------------------------------------------------------------------------

## Requirements

Create:

``` text
file-inventory.ps1
```

The script must:

1.  Accept or define a folder to inspect.
2.  Verify that the folder exists.
3.  Retrieve files recursively.
4.  Display the total number of files.
5.  Identify the largest files.
6.  Sort results from largest to smallest.
7.  Allow useful filtering, such as by extension or size.
8.  Display:
    -   File name
    -   Full path
    -   Extension
    -   Size
    -   Last modified date
9.  Make **no changes** to the files.

------------------------------------------------------------------------

## Safety Rule

This project is inventory only.

Do not use:

``` text
Remove-Item
Move-Item
Rename-Item
```

against the target data.

The purpose is to learn how to collect and analyze filesystem
information before taking action.

------------------------------------------------------------------------

## Calculated Data Challenge

Raw file sizes are commonly returned in bytes.

Create a useful size field such as:

``` text
SizeMB
```

or:

``` text
SizeGB
```

Round it to a reasonable number of decimal places.

Use what you learned about objects, operators, and `Select-Object`.

------------------------------------------------------------------------

## Required Filters

Your finished tool should be able to answer at least three questions:

``` text
What are the 10 largest files?
How many files are in the folder?
Which files match a particular extension?
```

Add one question of your own:

``` text
____________________________________________________
```

------------------------------------------------------------------------

## Minimum Deliverables

``` text
[ ] file-inventory.ps1
[ ] Folder existence check
[ ] Recursive file collection
[ ] Variables
[ ] Filtering
[ ] Sorting
[ ] Calculated file size
[ ] Useful selected properties
[ ] File count
[ ] Read-only behavior
```

------------------------------------------------------------------------

## Stretch Challenges

-   Add a minimum file-size threshold.
-   Count files by extension.
-   Find files older than a chosen date.
-   Store multiple extensions in an array and report on them.
-   Export the largest-file results to CSV.
-   Make the target folder a script parameter.

------------------------------------------------------------------------

## Testing

Test against a safe folder you control.

Record:

``` text
Folder tested:
____________________________________________________

Number of files:
____________________________________________________

Largest file:
____________________________________________________
```

------------------------------------------------------------------------

## Reflection

Which lesson was most useful for this project?

``` text
____________________________________________________
```

What would make this tool more useful for a help desk?

``` text
____________________________________________________
```

------------------------------------------------------------------------

## Project Complete


Continue to:

[Lesson 11 — Working with Data](../lessons/lesson-11-working-with-data.md)
