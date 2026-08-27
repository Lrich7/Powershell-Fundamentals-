[lesson-08-arrays-and-collections (1).md](https://github.com/user-attachments/files/31517106/lesson-08-arrays-and-collections.1.md)
# Lesson 08 --- Arrays and Collections

## Learning Objectives

By the end of this lesson, you will be able to:

-   Explain the difference between a single value and a collection of
    values.
-   Create basic PowerShell arrays.
-   Access array elements by index.
-   Use positive and negative indexes.
-   Use index ranges to retrieve multiple elements.
-   Determine the number of items in a collection.
-   Add items to basic arrays.
-   Use `foreach` to work with each item in a collection.
-   Store command output containing multiple objects in variables.
-   Filter, sort, and select objects stored in collections.
-   Understand the purpose of the array subexpression operator `@()`.
-   Recognize the difference between arrays and other collection types.
-   Use common collection properties and methods safely.

------------------------------------------------------------------------

## What Is a Collection?

A **collection** is a group of values or objects.

Instead of storing only one value:

``` powershell
$computer = 'PC-001'
```

you can store several values together:

``` powershell
$computers = 'PC-001', 'PC-002', 'PC-003'
```

Now `$computers` represents a collection of computer names.

Collections are extremely common in PowerShell because many commands
return more than one object.

For example:

``` powershell
Get-Service
```

normally returns a collection of service objects.

Likewise:

``` powershell
Get-Process
```

returns a collection of process objects.

> **Key Idea:** PowerShell is designed to work naturally with
> collections of objects, not just one object at a time.

------------------------------------------------------------------------

# What Is an Array?

An **array** is one of the most common collection types in PowerShell.

An array can contain multiple values.

For example:

``` powershell
$servers = 'Server01', 'Server02', 'Server03'
```

Display the variable:

``` powershell
$servers
```

PowerShell returns each item:

``` text
Server01
Server02
Server03
```

You can also create arrays containing numbers:

``` powershell
$numbers = 10, 20, 30, 40, 50
```

------------------------------------------------------------------------

# Creating Arrays with @()

PowerShell also supports explicit array syntax using:

``` text
@()
```

For example:

``` powershell
$servers = @(
    'Server01'
    'Server02'
    'Server03'
)
```

This can be easier to read when an array contains many values.

You can also write:

``` powershell
$servers = @('Server01', 'Server02', 'Server03')
```

Both create an array.

------------------------------------------------------------------------

# The Array Subexpression Operator

The syntax:

``` text
@()
```

is called the **array subexpression operator**.

It can also ensure that command output is treated as an array.

For example:

``` powershell
$services = @(Get-Service)
```

This guarantees that `$services` is treated as an array even if the
command returns only one object.

This becomes useful in scripts where you want predictable collection
behavior.

For example:

``` powershell
$services.Count
```

can then be used consistently.

------------------------------------------------------------------------

# Accessing Array Elements

Each item in an array has a numbered position called an **index**.

PowerShell array indexing begins at:

``` text
0
```

Consider:

``` powershell
$servers = 'Server01', 'Server02', 'Server03'
```

The indexes are:

``` text
Index 0 → Server01
Index 1 → Server02
Index 2 → Server03
```

To access the first item:

``` powershell
$servers[0]
```

To access the second:

``` powershell
$servers[1]
```

To access the third:

``` powershell
$servers[2]
```

> **Important:** PowerShell arrays start counting at zero.

------------------------------------------------------------------------

# Accessing the Last Item

You can use a negative index to count backward from the end of an array.

For example:

``` powershell
$servers[-1]
```

returns the last item.

With:

``` powershell
$servers = 'Server01', 'Server02', 'Server03'
```

the result is:

``` text
Server03
```

You can also use:

``` powershell
$servers[-2]
```

to retrieve the second item from the end.

------------------------------------------------------------------------

# Selecting Multiple Array Elements

You can request multiple indexes at once.

For example:

``` powershell
$servers[0,2]
```

This returns the items at indexes `0` and `2`.

You can also use a range.

For example:

``` powershell
$numbers = 10, 20, 30, 40, 50
```

Then:

``` powershell
$numbers[1..3]
```

returns the values stored at indexes:

``` text
1
2
3
```

which are:

``` text
20
30
40
```

------------------------------------------------------------------------

# The Range Operator

PowerShell's range operator is:

``` text
..
```

For example:

``` powershell
1..5
```

returns:

``` text
1
2
3
4
5
```

You can use this to quickly create a numeric array:

``` powershell
$numbers = 1..10
```

Then:

``` powershell
$numbers
```

returns the numbers from `1` through `10`.

------------------------------------------------------------------------

# Counting Items

Arrays and many other collections provide a:

``` text
Count
```

property.

For example:

``` powershell
$servers.Count
```

If the array contains three items, the result is:

``` text
3
```

You may also encounter:

``` powershell
$servers.Length
```

For arrays, `Count` and `Length` commonly return the number of elements.

`Count` is often especially readable when thinking about collections.

------------------------------------------------------------------------

# Arrays Can Contain Objects

Arrays are not limited to strings and numbers.

They can contain PowerShell objects.

For example:

``` powershell
$services = Get-Service
```

If multiple services are returned, `$services` contains a collection of
service objects.

You can access an individual service:

``` powershell
$services[0]
```

Because the item is still an object, you can access its properties:

``` powershell
$services[0].Name
```

``` powershell
$services[0].Status
```

This connects arrays directly to the object concepts from Lesson 04.

------------------------------------------------------------------------

# Inspecting Collections

You can inspect objects in a collection with:

``` powershell
$services | Get-Member
```

This tells you about the objects moving through the pipeline.

You can also inspect the collection variable itself using:

``` powershell
$services.GetType()
```

Depending on what was returned, the exact type can vary.

This is an important distinction:

``` text
The collection has a type.
The objects inside the collection also have types.
```

PowerShell's pipeline usually works with the **individual objects inside
the collection**.

------------------------------------------------------------------------

# Adding Items to an Array

You can add an item to an array using:

``` text
+=
```

For example:

``` powershell
$servers = 'Server01', 'Server02'
```

Then:

``` powershell
$servers += 'Server03'
```

Now:

``` powershell
$servers
```

contains:

``` text
Server01
Server02
Server03
```

This is convenient for small arrays.

However, standard PowerShell arrays have a fixed size internally. Using
`+=` creates a new array and copies the existing values.

For small scripts this is usually fine.

For very large or frequently changing collections, other collection
types may be more efficient.

------------------------------------------------------------------------

# Removing Items from an Array

Standard arrays do not have a simple built-in operation that removes an
element and resizes the same array.

Instead, you commonly create a new result by filtering the existing
array.

For example:

``` powershell
$servers = 'Server01', 'Server02', 'Server03'
```

To exclude `Server02`:

``` powershell
$servers = $servers | Where-Object { $_ -ne 'Server02' }
```

Now `$servers` contains:

``` text
Server01
Server03
```

This is another example of how PowerShell's pipeline works naturally
with collections.

------------------------------------------------------------------------

# Working with Each Item

A common task is performing the same action for every item in a
collection.

PowerShell provides several ways to do this.

One of the easiest to understand is the:

``` text
foreach
```

statement.

For example:

``` powershell
$servers = 'Server01', 'Server02', 'Server03'

foreach ($server in $servers) {
    $server
}
```

This means:

> For each item in `$servers`, temporarily call the current item
> `$server` and run the code inside the braces.

------------------------------------------------------------------------

# Understanding foreach

Consider:

``` powershell
foreach ($server in $servers) {
    "Checking $server"
}
```

Conceptually:

``` text
$servers
   │
   ├── Server01 → "Checking Server01"
   ├── Server02 → "Checking Server02"
   └── Server03 → "Checking Server03"
```

The loop processes each item one at a time.

You will learn more about loops in a later lesson. For now, understand
how collections naturally lead to repeating actions.

------------------------------------------------------------------------

# ForEach-Object

PowerShell also provides the pipeline command:

``` powershell
ForEach-Object
```

For example:

``` powershell
$servers |
    ForEach-Object {
        "Checking $_"
    }
```

Inside the script block:

``` powershell
$_
```

represents the current object in the pipeline.

You have already seen `$_` with `Where-Object`.

The same idea applies here.

------------------------------------------------------------------------

# foreach vs. ForEach-Object

You will commonly see both:

``` powershell
foreach ($item in $collection) {
    # Do something
}
```

and:

``` powershell
$collection | ForEach-Object {
    # Do something with $_
}
```

Both can process collections.

At this stage, focus on recognizing the syntax and understanding that
each approach lets you perform work for each item.

A later lesson on loops and control flow can explore these approaches in
greater depth.

------------------------------------------------------------------------

# Filtering Collections

Collections work naturally with `Where-Object`.

For example:

``` powershell
$services = Get-Service
```

Now filter the stored collection:

``` powershell
$services |
    Where-Object Status -eq 'Running'
```

You can store the filtered collection:

``` powershell
$runningServices = $services |
    Where-Object Status -eq 'Running'
```

Then:

``` powershell
$runningServices.Count
```

tells you how many objects were stored in the filtered collection.

------------------------------------------------------------------------

# Sorting Collections

You can sort a collection with:

``` powershell
$services |
    Sort-Object Name
```

Or:

``` powershell
$processes = Get-Process

$processes |
    Sort-Object CPU -Descending
```

Again, you can store the sorted results:

``` powershell
$sortedProcesses = $processes |
    Sort-Object CPU -Descending
```

------------------------------------------------------------------------

# Selecting from Collections

Use `Select-Object` to choose properties:

``` powershell
$services |
    Select-Object Name, Status
```

Or select only the first few objects:

``` powershell
$services |
    Select-Object -First 5
```

You can combine these concepts:

``` powershell
$services |
    Where-Object Status -eq 'Running' |
    Sort-Object Name |
    Select-Object -First 10 Name, Status
```

This is the same pipeline workflow you have already learned, now applied
to a collection stored in a variable.

------------------------------------------------------------------------

# Property Enumeration

PowerShell can often retrieve a property from every object in a
collection.

For example:

``` powershell
$services = Get-Service
```

Then:

``` powershell
$services.Name
```

PowerShell can return the `Name` property from the service objects in
the collection.

Likewise:

``` powershell
$services.Status
```

can return the status values.

This is called **member-access enumeration**.

You may also see the same result written using the pipeline:

``` powershell
$services | Select-Object -ExpandProperty Name
```

Both approaches can be useful.

------------------------------------------------------------------------

# Select-Object -ExpandProperty

Normally:

``` powershell
$services |
    Select-Object Name
```

returns objects containing a `Name` property.

Using:

``` powershell
$services |
    Select-Object -ExpandProperty Name
```

returns the values of the `Name` property themselves.

This distinction becomes useful when you need a simple collection of
property values.

For example:

``` powershell
$serviceNames = Get-Service |
    Select-Object -ExpandProperty Name
```

Now `$serviceNames` contains service name values.

------------------------------------------------------------------------

# Unique Values in a Collection

Sometimes you want to know which distinct values exist in a collection.

For example:

``` powershell
Get-Service |
    Select-Object -ExpandProperty Status |
    Sort-Object -Unique
```

This extracts the `Status` values and returns the unique results.

The pattern is:

``` text
Objects
   ↓
Extract a Property
   ↓
Sort
   ↓
Keep Unique Values
```

------------------------------------------------------------------------

# Collections from Command Output

One of the most important things to understand about PowerShell is that
command output may contain:

``` text
Zero objects
One object
Many objects
```

For example:

``` powershell
Get-Process
```

usually returns many objects.

But:

``` powershell
Get-Process -Name notepad
```

might return:

-   No objects if Notepad is not running
-   One object if one Notepad process is running
-   Multiple objects if several are running

This can affect how scripts behave.

Using:

``` powershell
$processes = @(Get-Process -Name notepad -ErrorAction SilentlyContinue)
```

ensures that `$processes` behaves as an array.

Then:

``` powershell
$processes.Count
```

gives you a predictable way to count the results.

------------------------------------------------------------------------

# Empty Collections

An array can contain zero items.

For example:

``` powershell
$items = @()
```

Then:

``` powershell
$items.Count
```

returns:

``` text
0
```

This is useful when you want to initialize an empty collection before
adding or processing values.

------------------------------------------------------------------------

# \$null vs. an Empty Array

These are not the same concept:

``` powershell
$value = $null
```

and:

``` powershell
$value = @()
```

`$null` represents the absence of a value.

An empty array is a collection that currently contains zero items.

Conceptually:

``` text
$null
  ↓
No value

@()
  ↓
A collection with 0 items
```

This distinction becomes important in scripts that test command results.

------------------------------------------------------------------------

# Other Collection Types

Arrays are only one type of collection.

PowerShell and .NET provide many other collection types.

You may encounter:

``` text
Array
ArrayList
List
Hashtable
Dictionary
Queue
Stack
```

You do not need to learn all of these yet.

For beginner PowerShell work, standard arrays and command-generated
collections are a good starting point.

------------------------------------------------------------------------

# Generic Lists

For collections that change frequently, you may eventually encounter
generic lists.

For example:

``` powershell
$list = [System.Collections.Generic.List[string]]::new()
```

Then:

``` powershell
$list.Add('Server01')
$list.Add('Server02')
```

Generic lists can add and remove items more efficiently than repeatedly
using `+=` with large arrays.

This is an advanced concept for now.

The important takeaway is:

> Arrays are excellent for many tasks, but PowerShell can also use other
> .NET collection types when needed.

------------------------------------------------------------------------

# Hashtables Are Collections Too

You will later work with another important collection type called a
**hashtable**.

A hashtable stores information using key-value pairs.

For example:

``` powershell
$user = @{
    Name       = 'Alex'
    Department = 'IT'
    Enabled    = $true
}
```

This is different from a normal array because values are retrieved using
keys rather than numeric positions.

Hashtables deserve their own focused lesson, so you do not need to
explore them deeply yet.

------------------------------------------------------------------------

# A Practical Collection Workflow

Suppose you want to examine running services.

## Step 1 --- Store the Collection

``` powershell
$services = Get-Service
```

## Step 2 --- Count the Objects

``` powershell
$services.Count
```

## Step 3 --- Inspect the Objects

``` powershell
$services | Get-Member
```

## Step 4 --- Filter the Collection

``` powershell
$runningServices = $services |
    Where-Object Status -eq 'Running'
```

## Step 5 --- Count the Filtered Results

``` powershell
$runningServices.Count
```

## Step 6 --- Sort the Collection

``` powershell
$runningServices |
    Sort-Object Name
```

## Step 7 --- Select Useful Properties

``` powershell
$runningServices |
    Sort-Object Name |
    Select-Object Name, DisplayName, Status
```

This workflow combines variables, objects, collections, filtering,
sorting, and the pipeline.

------------------------------------------------------------------------

# Collections Connect the Previous Lessons

Arrays and collections bring many earlier PowerShell concepts together.

``` text
Run a command
      ↓
Receive objects
      ↓
Store them in a variable
      ↓
Collection of objects
      ↓
Count / Index / Enumerate
      ↓
Pipeline
      ↓
Filter / Sort / Select
      ↓
Process each object
```

For example:

``` powershell
$processes = Get-Process

$processes |
    Where-Object CPU -gt 10 |
    Sort-Object CPU -Descending |
    Select-Object Name, Id, CPU
```

The variable `$processes` contains a collection, and each pipeline stage
works with the objects in that collection.

------------------------------------------------------------------------

# Key Takeaways

-   A collection contains multiple values or objects.
-   Arrays are one of the most common PowerShell collection types.
-   Arrays can contain strings, numbers, PowerShell objects, or other
    values.
-   Array indexing begins at `0`.
-   Negative indexes count backward from the end of an array.
-   The range operator `..` can generate sequences and select index
    ranges.
-   The `Count` property returns the number of items in many
    collections.
-   `@()` can explicitly create an array or ensure command output is
    treated as an array.
-   `+=` can add items to basic arrays but creates a new array
    internally.
-   `foreach` and `ForEach-Object` can process each item in a
    collection.
-   Collections work naturally with the PowerShell pipeline.
-   `Where-Object`, `Sort-Object`, and `Select-Object` can process
    collections of objects.
-   `Select-Object -ExpandProperty` extracts property values.
-   `$null` is different from an empty array.
-   PowerShell supports many collection types beyond arrays.
-   The object and pipeline skills from earlier lessons become
    especially powerful when working with collections.

------------------------------------------------------------------------

# Lab

Ready to practice working with arrays and collections?

Continue to:

[Lab 08 --- Arrays and
Collections](../labs/lab-08-arrays-and-collections.md)

In the lab, you will create arrays, access values by index, use ranges
and negative indexes, count items, add values, store command results in
collections, process objects with `foreach`, and combine collections
with filtering, sorting, and the pipeline.

------------------------------------------------------------------------

## Additional Resources

-   [About Arrays --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_arrays?view=powershell-7.6)
-   [About Array --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_array?view=powershell-7.6)
-   [About ForEach --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_foreach?view=powershell-7.6)
-   [ForEach-Object --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/foreach-object?view=powershell-7.6)
-   [About Member-Access Enumeration --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_member-access_enumeration?view=powershell-7.6)
-   [About Null --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_null?view=powershell-7.6)
