[lesson-12-loops.md](https://github.com/user-attachments/files/31517394/lesson-12-loops.md)

# Lesson 12 --- Loops

## Learning Objectives

By the end of this lesson, you will be able to:

-   Explain why loops are useful in PowerShell.
-   Use `foreach` to process every item in a collection.
-   Use `ForEach-Object` with pipeline input.
-   Use `for` loops when you need a counter.
-   Use `while` and `do` loops for condition-based repetition.
-   Use `break` and `continue` to control loop execution.
-   Choose an appropriate loop for a task.
-   Apply loops to common IT administration scenarios.

------------------------------------------------------------------------

## What Is a Loop?

A **loop** repeats a block of code.

Instead of writing:

``` powershell
"Checking PC-001"
"Checking PC-002"
"Checking PC-003"
```

you can store the computers in a collection:

``` powershell
$computers = 'PC-001', 'PC-002', 'PC-003'
```

and process them with a loop:

``` powershell
foreach ($computer in $computers) {
    "Checking $computer"
}
```

> **Key Idea:** Loops allow you to perform the same task against many
> values or objects without duplicating code.

------------------------------------------------------------------------

# The foreach Statement

`foreach` is one of the most useful loops in PowerShell.

Basic syntax:

``` powershell
foreach ($item in $collection) {
    # Commands
}
```

Example:

``` powershell
$computers = 'PC-001', 'PC-002', 'PC-003'

foreach ($computer in $computers) {
    "Computer: $computer"
}
```

PowerShell processes each item one at a time.

Conceptually:

``` text
Collection
   │
   ├── PC-001 → Run code
   ├── PC-002 → Run code
   └── PC-003 → Run code
```

------------------------------------------------------------------------

# Working with Object Collections

Loops can process complete PowerShell objects.

For example:

``` powershell
$services = Get-Service

foreach ($service in $services) {
    "$($service.Name) - $($service.Status)"
}
```

Because `$service` is an object, its properties remain available:

``` powershell
$service.Name
$service.Status
$service.DisplayName
```

------------------------------------------------------------------------

# ForEach-Object

PowerShell also provides:

``` powershell
ForEach-Object
```

This cmdlet processes objects arriving through the pipeline.

Example:

``` powershell
Get-Service |
    ForEach-Object {
        "$($_.Name) - $($_.Status)"
    }
```

Inside the script block:

``` powershell
$_
```

represents the current pipeline object.

------------------------------------------------------------------------

# foreach vs. ForEach-Object

These approaches solve similar problems but are used differently.

``` powershell
$services = Get-Service

foreach ($service in $services) {
    $service.Name
}
```

Pipeline version:

``` powershell
Get-Service |
    ForEach-Object {
        $_.Name
    }
```

A useful beginner guideline is:

``` text
Already have a collection → foreach
Working directly in a pipeline → ForEach-Object
```

Both are important PowerShell tools.

------------------------------------------------------------------------

# The for Loop

A `for` loop is useful when you need a numeric counter.

Basic structure:

``` powershell
for (initialization; condition; update) {
    # Commands
}
```

Example:

``` powershell
for ($i = 1; $i -le 5; $i++) {
    "Iteration $i"
}
```

This means:

``` text
Start $i at 1
Continue while $i <= 5
Increase $i by 1 each time
```

------------------------------------------------------------------------

# Using for with Arrays

Because arrays use numeric indexes, `for` can work well with them.

``` powershell
$servers = 'Server01', 'Server02', 'Server03'

for ($i = 0; $i -lt $servers.Count; $i++) {
    "Index $i contains $($servers[$i])"
}
```

Notice:

``` powershell
$i -lt $servers.Count
```

The loop stops before attempting to access an index that does not exist.

------------------------------------------------------------------------

# The while Loop

A `while` loop repeats while a condition remains true.

Example:

``` powershell
$count = 1

while ($count -le 5) {
    "Count: $count"
    $count++
}
```

PowerShell checks the condition **before** each iteration.

If the condition is false at the beginning, the loop may not run at all.

------------------------------------------------------------------------

# Avoid Infinite Loops

A loop becomes infinite when its stopping condition never becomes false.

For example:

``` powershell
$count = 1

while ($count -le 5) {
    "Count: $count"
}
```

`$count` never changes, so the condition remains true.

The corrected version is:

``` powershell
$count = 1

while ($count -le 5) {
    "Count: $count"
    $count++
}
```

> **Important:** When using condition-based loops, make sure something
> inside the loop eventually changes the condition.

------------------------------------------------------------------------

# do While

A `do while` loop runs the code first and checks the condition
afterward.

``` powershell
$count = 1

do {
    "Count: $count"
    $count++
} while ($count -le 5)
```

Because the condition is checked afterward, the loop runs at least once.

------------------------------------------------------------------------

# do Until

PowerShell also supports:

``` powershell
do {
    # Commands
} until ($condition)
```

Example:

``` powershell
$count = 1

do {
    "Count: $count"
    $count++
} until ($count -gt 5)
```

A useful distinction is:

``` text
while → Continue while condition is True
until → Continue until condition becomes True
```

------------------------------------------------------------------------

# break

Use:

``` powershell
break
```

to exit a loop immediately.

Example:

``` powershell
$numbers = 1..10

foreach ($number in $numbers) {
    if ($number -eq 5) {
        break
    }

    $number
}
```

The loop stops when `$number` reaches `5`.

------------------------------------------------------------------------

# continue

Use:

``` powershell
continue
```

to skip the remainder of the current iteration and move to the next one.

Example:

``` powershell
foreach ($number in 1..5) {
    if ($number -eq 3) {
        continue
    }

    $number
}
```

The value `3` is skipped.

------------------------------------------------------------------------

# Filtering Before Looping

Do not make a loop do unnecessary work when PowerShell can filter the
objects first.

Instead of processing every service:

``` powershell
foreach ($service in (Get-Service)) {
    # Check status here
}
```

you can filter first:

``` powershell
$runningServices = Get-Service |
    Where-Object Status -eq 'Running'

foreach ($service in $runningServices) {
    $service.Name
}
```

A useful pattern is:

``` text
Collect → Filter → Loop → Act
```

------------------------------------------------------------------------

# Practical IT Example --- Processing Assets

Suppose you import an asset list:

``` powershell
$assets = Import-Csv C:\Temp\assets.csv
```

You can process every asset:

``` powershell
foreach ($asset in $assets) {
    "Asset $($asset.AssetTag) is assigned to $($asset.AssignedTo)"
}
```

Or filter first:

``` powershell
$unassigned = $assets |
    Where-Object { [string]::IsNullOrWhiteSpace($_.AssignedTo) }

foreach ($asset in $unassigned) {
    "Unassigned asset: $($asset.AssetTag)"
}
```

------------------------------------------------------------------------

# Choosing a Loop

A simple guideline:

  Loop               Good Use
  ------------------ -------------------------------------------------
  `foreach`          Process every item in an existing collection
  `ForEach-Object`   Process objects directly in a pipeline
  `for`              Repeat with a counter or array index
  `while`            Repeat while a condition is true
  `do while`         Run once, then repeat while a condition is true
  `do until`         Run until a condition becomes true

------------------------------------------------------------------------

# Key Takeaways

-   Loops repeat PowerShell code.
-   `foreach` is excellent for processing collections.
-   `ForEach-Object` works naturally with the pipeline.
-   `for` is useful when a counter or index is needed.
-   `while` checks its condition before running.
-   `do while` and `do until` run at least once.
-   `break` exits a loop.
-   `continue` skips the current iteration.
-   Make sure condition-based loops can eventually stop.
-   Filter collections before looping when possible.
-   A common administrative pattern is:

``` text
Collect → Filter → Loop → Act
```

------------------------------------------------------------------------

# Lab

Continue to:

[Lab 12 --- Loops](../labs/lab-12-loops.md)

In the lab, you will process arrays and command output with `foreach`,
use `ForEach-Object`, build counter-based loops, practice `while` and
`do` loops, and use `break` and `continue`.

------------------------------------------------------------------------

## Additional Resources

-   [About ForEach --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_foreach?view=powershell-7.6)
-   [ForEach-Object --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/foreach-object?view=powershell-7.6)
-   [About For --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_for?view=powershell-7.6)
-   [About While --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_while?view=powershell-7.6)
-   [About Do --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_do?view=powershell-7.6)
