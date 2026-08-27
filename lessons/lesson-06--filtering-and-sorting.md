[lesson-06-filtering-and-sorting.md](https://github.com/user-attachments/files/31517053/lesson-06-filtering-and-sorting.md)

# Lesson 06 --- Filtering and Sorting

## Learning Objectives

By the end of this lesson, you will be able to:

-   Explain why filtering data early is useful in PowerShell.
-   Use command parameters to filter results when supported.
-   Use `Where-Object` to filter objects in the pipeline.
-   Use common PowerShell comparison operators.
-   Filter text using `-like`, `-notlike`, `-match`, and `-notmatch`.
-   Combine multiple filtering conditions with logical operators.
-   Use `Sort-Object` to sort objects by one or more properties.
-   Sort results in ascending and descending order.
-   Remove duplicate values with `Sort-Object -Unique`.
-   Combine filtering, sorting, and `Select-Object` in practical
    pipelines.
-   Build filtering and sorting commands one stage at a time.

------------------------------------------------------------------------

## Why Filter and Sort Data?

PowerShell commands can return a large amount of information.

For example:

``` powershell
Get-Service
```

may return many Windows services.

Likewise:

``` powershell
Get-Process
```

may return dozens or hundreds of running processes.

Most of the time, you do not need every object returned by a command.

You might want:

-   Only running services
-   Only stopped services
-   Processes using more than a certain amount of CPU time
-   Files with a particular extension
-   Objects whose names match a pattern
-   Results sorted alphabetically
-   Numeric results sorted from highest to lowest

PowerShell provides tools that let you reduce and organize this
information.

Two of the most important are:

``` powershell
Where-Object
Sort-Object
```

> **Key Idea:** Filtering determines **which objects you want**. Sorting
> determines **the order you want them in**.

------------------------------------------------------------------------

# Filtering as Early as Possible

There are often multiple ways to filter PowerShell results.

When a command provides a parameter that performs the filtering
directly, that is often the best place to start.

For example:

``` powershell
Get-Service -Name Spooler
```

This asks `Get-Service` to retrieve the specific service.

Compare that with:

``` powershell
Get-Service | Where-Object Name -eq 'Spooler'
```

Both can produce the desired service, but the first command asks the
source command to return only what you requested.

A useful general rule is:

> **Filter as early as reasonably possible.**

If the original command supports the filtering you need, use its
parameters.

If it does not, `Where-Object` gives you a flexible way to filter
objects in the pipeline.

------------------------------------------------------------------------

# Filtering with Where-Object

`Where-Object` filters objects based on their properties.

For example:

``` powershell
Get-Service | Where-Object Status -eq 'Running'
```

This means:

1.  Get the service objects.
2.  Examine the `Status` property.
3.  Keep objects whose status equals `Running`.

Conceptually:

``` text
Get-Service
     │
     ▼
All Service Objects
     │
     ▼
Where-Object
Status = Running
     │
     ▼
Running Services
```

------------------------------------------------------------------------

# Comparison Operators

PowerShell uses comparison operators to compare values.

Some of the most common are:

  Operator      Meaning
  ------------- -------------------------------------
  `-eq`         Equal to
  `-ne`         Not equal to
  `-gt`         Greater than
  `-ge`         Greater than or equal to
  `-lt`         Less than
  `-le`         Less than or equal to
  `-like`       Matches a wildcard pattern
  `-notlike`    Does not match a wildcard pattern
  `-match`      Matches a regular expression
  `-notmatch`   Does not match a regular expression

You will use these operators frequently when filtering PowerShell
objects.

------------------------------------------------------------------------

# Equal To: -eq

`-eq` means **equal to**.

For example:

``` powershell
Get-Service | Where-Object Status -eq 'Stopped'
```

This keeps services whose `Status` property equals `Stopped`.

Another example:

``` powershell
Get-Process | Where-Object Name -eq 'notepad'
```

------------------------------------------------------------------------

# Not Equal To: -ne

`-ne` means **not equal to**.

For example:

``` powershell
Get-Service | Where-Object Status -ne 'Running'
```

This keeps services whose status is not `Running`.

------------------------------------------------------------------------

# Greater Than: -gt

`-gt` means **greater than**.

For example:

``` powershell
Get-Process | Where-Object CPU -gt 10
```

This keeps process objects whose `CPU` property is greater than `10`.

------------------------------------------------------------------------

# Greater Than or Equal To: -ge

`-ge` means **greater than or equal to**.

``` powershell
Get-Process | Where-Object CPU -ge 10
```

------------------------------------------------------------------------

# Less Than: -lt

`-lt` means **less than**.

For example:

``` powershell
Get-Process | Where-Object CPU -lt 5
```

------------------------------------------------------------------------

# Less Than or Equal To: -le

`-le` means **less than or equal to**.

``` powershell
Get-Process | Where-Object CPU -le 5
```

------------------------------------------------------------------------

# Filtering Text with -like

`-like` allows you to compare text using wildcard patterns.

The most common wildcard is:

``` text
*
```

The `*` represents zero or more characters.

For example:

``` powershell
Get-Service | Where-Object Name -like 'Win*'
```

This keeps services whose names begin with `Win`.

You can also search for text anywhere in a value:

``` powershell
Get-Service | Where-Object DisplayName -like '*Windows*'
```

This keeps services whose display name contains `Windows`.

------------------------------------------------------------------------

# The ? Wildcard

PowerShell also supports:

``` text
?
```

The `?` wildcard represents a single character.

For example:

``` powershell
'Test1' -like 'Test?'
```

returns:

``` text
True
```

because `?` matches the final single character.

You will usually use `*` more frequently, but it is useful to recognize
both wildcard characters.

------------------------------------------------------------------------

# Filtering with -notlike

`-notlike` is the opposite of `-like`.

For example:

``` powershell
Get-Service | Where-Object Name -notlike 'Win*'
```

This keeps service objects whose names do **not** begin with `Win`.

------------------------------------------------------------------------

# Pattern Matching with -match

PowerShell also provides:

``` text
-match
```

`-match` uses **regular expressions**.

For example:

``` powershell
Get-Service | Where-Object Name -match '^Win'
```

The `^` in this regular expression means the text must begin with `Win`.

Regular expressions are much more powerful than wildcards, but they are
also more advanced.

At this stage, remember:

``` text
-like   → wildcard matching
-match  → regular expression matching
```

You can learn more advanced regular expressions later.

------------------------------------------------------------------------

# Filtering with a Script Block

You will frequently see `Where-Object` written with a script block:

``` powershell
Get-Service | Where-Object { $_.Status -eq 'Running' }
```

Inside the braces:

``` powershell
$_
```

represents the **current object moving through the pipeline**.

Therefore:

``` powershell
$_.Status
```

means:

> Look at the `Status` property of the current object.

Then:

``` powershell
$_.Status -eq 'Running'
```

asks:

> Does the current object's Status property equal Running?

If the result is `True`, the object remains in the pipeline.

If the result is `False`, the object is filtered out.

------------------------------------------------------------------------

# Simple Syntax vs. Script Block Syntax

For simple comparisons, PowerShell allows a shorter form:

``` powershell
Get-Service | Where-Object Status -eq 'Running'
```

You can also write:

``` powershell
Get-Service | Where-Object { $_.Status -eq 'Running' }
```

Both are valid for this example.

The script block form becomes especially useful when you need more
complex conditions.

------------------------------------------------------------------------

# Combining Conditions with -and

Sometimes one condition is not enough.

Use:

``` text
-and
```

when **both conditions must be true**.

For example:

``` powershell
Get-Process |
    Where-Object { $_.CPU -gt 10 -and $_.Name -like 'p*' }
```

This keeps processes where:

``` text
CPU is greater than 10
AND
Name begins with p
```

Both conditions must be true.

------------------------------------------------------------------------

# Combining Conditions with -or

Use:

``` text
-or
```

when **either condition can be true**.

For example:

``` powershell
Get-Service |
    Where-Object {
        $_.Status -eq 'Stopped' -or
        $_.Name -like 'Win*'
    }
```

An object is kept if either condition evaluates to `True`.

------------------------------------------------------------------------

# Using -not

`-not` reverses a Boolean condition.

For example:

``` powershell
Get-Service |
    Where-Object { -not ($_.Status -eq 'Running') }
```

This keeps objects where the condition `Status equals Running` is not
true.

In this particular example, using `-ne` would be simpler:

``` powershell
Get-Service | Where-Object Status -ne 'Running'
```

But it is useful to recognize `-not` because you will encounter it in
more complex PowerShell expressions.

------------------------------------------------------------------------

# Parentheses and Complex Conditions

Parentheses can make complex conditions easier to understand and
control.

For example:

``` powershell
Get-Process |
    Where-Object {
        ($_.CPU -gt 10) -and
        ($_.Name -like 'p*')
    }
```

When expressions become more complicated, parentheses can help make your
intent clear.

> **Tip:** Prefer readable filtering expressions over trying to make
> every command as short as possible.

------------------------------------------------------------------------

# Discovering Properties Before Filtering

You can only filter by a property if you know that property exists.

If you are unsure what properties an object has, use:

``` powershell
Get-Process | Get-Member
```

Or:

``` powershell
Get-Service | Get-Member
```

You can focus on properties with:

``` powershell
Get-Service | Get-Member -MemberType Property
```

Then choose a useful property for your filter.

This reinforces the object-discovery workflow from Lesson 04.

``` text
Run
 ↓
Inspect with Get-Member
 ↓
Identify a Property
 ↓
Filter with Where-Object
```

------------------------------------------------------------------------

# Sorting with Sort-Object

Filtering controls which objects remain.

`Sort-Object` controls the order of those objects.

For example:

``` powershell
Get-Service | Sort-Object Name
```

This sorts service objects by their `Name` property.

Another example:

``` powershell
Get-Process | Sort-Object CPU
```

This sorts process objects by their `CPU` property.

------------------------------------------------------------------------

# Ascending Sort Order

By default, `Sort-Object` sorts values in ascending order.

For text, this generally means:

``` text
A → Z
```

For numbers:

``` text
Lowest → Highest
```

Example:

``` powershell
Get-Process | Sort-Object CPU
```

------------------------------------------------------------------------

# Descending Sort Order

Use:

``` text
-Descending
```

to reverse the sort order.

For example:

``` powershell
Get-Process | Sort-Object CPU -Descending
```

Now the largest CPU values appear first.

This is especially useful when looking for the highest or largest
values.

------------------------------------------------------------------------

# Finding the Top Results

You can combine `Sort-Object` with `Select-Object`.

For example:

``` powershell
Get-Process |
    Sort-Object CPU -Descending |
    Select-Object -First 10 Name, Id, CPU
```

This pipeline:

1.  Gets processes.
2.  Sorts them by CPU from highest to lowest.
3.  Keeps the first ten.
4.  Displays selected properties.

This is a very common PowerShell pattern.

------------------------------------------------------------------------

# Sorting by Multiple Properties

`Sort-Object` can sort by more than one property.

For example:

``` powershell
Get-Service | Sort-Object Status, Name
```

PowerShell first sorts by:

``` text
Status
```

and then sorts objects with the same status by:

``` text
Name
```

This can make large sets of results much easier to review.

------------------------------------------------------------------------

# Removing Duplicate Values

`Sort-Object` can also return unique values with:

``` text
-Unique
```

For example:

``` powershell
Get-Service |
    Select-Object -ExpandProperty Status |
    Sort-Object -Unique
```

This can return the unique service status values found in the results.

You will learn more about `-ExpandProperty` as you continue working with
objects.

For now, recognize that `Sort-Object -Unique` can be useful when you
want a distinct list of values.

------------------------------------------------------------------------

# Filter First, Sort Second

When possible, reduce the number of objects before performing additional
work.

For example:

``` powershell
Get-Service |
    Where-Object Status -eq 'Running' |
    Sort-Object Name
```

This:

1.  Retrieves services.
2.  Keeps only running services.
3.  Sorts the remaining services.

Conceptually:

``` text
Get-Service
     │
     ▼
All Services
     │
     ▼
Where-Object
Keep Running
     │
     ▼
Sort-Object
Sort by Name
```

This is usually easier to reason about than sorting data you plan to
discard.

------------------------------------------------------------------------

# Filtering Files

The same concepts apply to other types of PowerShell objects.

For example:

``` powershell
Get-ChildItem
```

returns file and directory objects for the current location.

You can inspect them:

``` powershell
Get-ChildItem | Get-Member
```

Then filter them.

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

You can then sort the results:

``` powershell
Get-ChildItem |
    Where-Object Name -like '*.txt' |
    Sort-Object Name
```

------------------------------------------------------------------------

# Sorting Files by Size

File objects contain a `Length` property representing file size in
bytes.

You can sort files by size:

``` powershell
Get-ChildItem -File |
    Sort-Object Length -Descending
```

Then display the largest files first:

``` powershell
Get-ChildItem -File |
    Sort-Object Length -Descending |
    Select-Object -First 10 Name, Length
```

Notice how the same pipeline concepts work with files, processes,
services, and many other PowerShell objects.

------------------------------------------------------------------------

# Filtering vs. Formatting

Filtering and formatting are different operations.

Filtering determines:

> Which objects should remain?

Formatting determines:

> How should the final results look on the screen?

For example:

``` powershell
Get-Service |
    Where-Object Status -eq 'Running' |
    Sort-Object Name |
    Select-Object Name, Status |
    Format-Table
```

A useful rule remains:

``` text
Filter
  ↓
Sort
  ↓
Select
  ↓
Format
```

Formatting commands should generally come last.

------------------------------------------------------------------------

# Order Matters

Pipeline order can change your results.

Consider:

``` powershell
Get-Process |
    Sort-Object CPU -Descending |
    Select-Object -First 5 Name, CPU
```

This sorts **all** processes and then selects the top five.

Now compare:

``` powershell
Get-Process |
    Select-Object -First 5 Name, CPU |
    Sort-Object CPU -Descending
```

This selects five processes first and sorts only those five.

The results may be very different.

> **Key Idea:** Read a pipeline from left to right and think about what
> objects remain after each stage.

------------------------------------------------------------------------

# Build Filters One Step at a Time

When creating a more complicated filter, build it gradually.

Start with:

``` powershell
Get-Process
```

Then:

``` powershell
Get-Process |
    Where-Object CPU -gt 10
```

Then add sorting:

``` powershell
Get-Process |
    Where-Object CPU -gt 10 |
    Sort-Object CPU -Descending
```

Then choose the properties:

``` powershell
Get-Process |
    Where-Object CPU -gt 10 |
    Sort-Object CPU -Descending |
    Select-Object Name, Id, CPU
```

Testing each stage makes mistakes much easier to find.

------------------------------------------------------------------------

# A Practical Filtering Workflow

Suppose you want to find running services whose names begin with `Win`.

## Step 1 --- Get the Objects

``` powershell
Get-Service
```

## Step 2 --- Inspect Properties if Needed

``` powershell
Get-Service | Get-Member
```

## Step 3 --- Filter by Status

``` powershell
Get-Service |
    Where-Object Status -eq 'Running'
```

## Step 4 --- Add Another Condition

``` powershell
Get-Service |
    Where-Object {
        $_.Status -eq 'Running' -and
        $_.Name -like 'Win*'
    }
```

## Step 5 --- Sort the Results

``` powershell
Get-Service |
    Where-Object {
        $_.Status -eq 'Running' -and
        $_.Name -like 'Win*'
    } |
    Sort-Object Name
```

## Step 6 --- Select Useful Properties

``` powershell
Get-Service |
    Where-Object {
        $_.Status -eq 'Running' -and
        $_.Name -like 'Win*'
    } |
    Sort-Object Name |
    Select-Object Name, DisplayName, Status
```

You created a useful report by combining several simple PowerShell
commands.

------------------------------------------------------------------------

# Expanding the PowerShell Workflow

The discovery workflow from the previous lessons continues to grow.

``` text
Find the command
      ↓
Get-Command

Learn the command
      ↓
Get-Help

Run the command
      ↓
Inspect the objects
      ↓
Get-Member

Choose the objects
      ↓
Where-Object

Organize the objects
      ↓
Sort-Object

Choose the information
      ↓
Select-Object
```

A practical example:

``` powershell
Get-Process |
    Where-Object CPU -gt 10 |
    Sort-Object CPU -Descending |
    Select-Object Name, Id, CPU
```

These same skills will continue to appear throughout your PowerShell
training.

------------------------------------------------------------------------

# Key Takeaways

-   Filtering determines **which objects remain**.
-   Sorting determines **the order of the objects**.
-   Filter with a command's own parameters when an appropriate parameter
    is available.
-   `Where-Object` provides flexible pipeline filtering.
-   `$_` represents the current pipeline object inside many script
    blocks.
-   `-eq` and `-ne` test equality.
-   `-gt`, `-ge`, `-lt`, and `-le` compare values.
-   `-like` performs wildcard matching.
-   `-match` performs regular expression matching.
-   `-and`, `-or`, and `-not` allow you to build more complex
    conditions.
-   `Sort-Object` sorts objects by one or more properties.
-   `-Descending` reverses the default sort order.
-   `Sort-Object -Unique` can help produce unique values.
-   Pipeline order matters.
-   `Get-Member` helps you discover which properties are available for
    filtering and sorting.
-   Build complicated filters and pipelines one stage at a time.
-   A useful general pattern is:

``` text
Get → Filter → Sort → Select → Format
```

------------------------------------------------------------------------

# Lab

Ready to practice filtering and sorting PowerShell objects?

Continue to:

[Lab 06 — Filtering and Sorting](../labs/lesson-06-lab-06-filtering-and-sorting.md)


In the lab, you will filter services, processes, and files; practice
comparison and logical operators; sort objects by different properties;
find top results; and build multi-condition filters.

------------------------------------------------------------------------

## Additional Resources

-   [Where-Object --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/where-object?view=powershell-7.6)
-   [Sort-Object --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/sort-object?view=powershell-7.6)
-   [About Comparison Operators --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_comparison_operators?view=powershell-7.6)
-   [About Logical Operators --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_logical_operators?view=powershell-7.6)
-   [About Wildcards --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_wildcards?view=powershell-7.6)
-   [About Regular Expressions --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_regular_expressions?view=powershell-7.6)
