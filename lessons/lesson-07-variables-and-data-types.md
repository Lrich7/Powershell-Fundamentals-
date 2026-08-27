[lesson-07-variables-and-data-types.md](https://github.com/user-attachments/files/31517063/lesson-07-variables-and-data-types.md)

# Lesson 07 --- Variables and Data Types

## Learning Objectives

By the end of this lesson, you will be able to:

-   Explain what variables are and why they are useful in PowerShell.
-   Create, read, update, and remove variables.
-   Store command output and PowerShell objects in variables.
-   Use meaningful variable names.
-   Explain what a data type is.
-   Identify common PowerShell data types.
-   Use `GetType()` to inspect the type of a value or object.
-   Understand the difference between strings, numbers, Boolean values,
    arrays, and other objects.
-   Create and work with basic arrays.
-   Use type casting when a specific data type is needed.
-   Understand the basics of variable expansion in strings.
-   Recognize `$null` and test whether a variable contains a value.

------------------------------------------------------------------------

## What Is a Variable?

A **variable** is a named location used to store a value.

In PowerShell, variable names begin with:

``` text
$
```

For example:

``` powershell
$name = 'PowerShell'
```

In this example:

``` text
$name
```

is the variable name, and:

``` text
PowerShell
```

is the value stored in the variable.

You can display the value by entering:

``` powershell
$name
```

PowerShell returns:

``` text
PowerShell
```

> **Key Idea:** Variables allow you to store information so you can
> reuse it later instead of repeatedly typing or retrieving the same
> value.

------------------------------------------------------------------------

# Creating Variables

PowerShell creates a variable when you assign a value to it.

For example:

``` powershell
$computerName = 'PC-001'
```

You can then display it:

``` powershell
$computerName
```

Another example:

``` powershell
$department = 'IT'
```

You do not normally need a special command to create a basic PowerShell
variable.

The assignment operator is:

``` text
=
```

The basic pattern is:

``` powershell
$variableName = value
```

------------------------------------------------------------------------

# Using Meaningful Variable Names

Variable names should describe the information they contain.

For example:

``` powershell
$computerName = 'PC-001'
$userName = 'jsmith'
$department = 'IT'
```

These are easier to understand than:

``` powershell
$x = 'PC-001'
$y = 'jsmith'
$z = 'IT'
```

Short names can be useful in some situations, but descriptive names make
scripts easier to read and maintain.

> **Tip:** Write variable names so that someone reading your script
> later can understand what the value represents.

------------------------------------------------------------------------

# Updating Variables

You can replace the value stored in a variable by assigning a new value.

For example:

``` powershell
$status = 'Pending'
```

Check the value:

``` powershell
$status
```

Then change it:

``` powershell
$status = 'Complete'
```

Now:

``` powershell
$status
```

returns:

``` text
Complete
```

The old value has been replaced.

------------------------------------------------------------------------

# Storing Numbers

Variables can store numbers.

For example:

``` powershell
$count = 10
```

You can use numeric variables in calculations:

``` powershell
$count + 5
```

You can also update a value based on its current value:

``` powershell
$count = $count + 1
```

PowerShell also supports the increment operator:

``` powershell
$count++
```

This increases the value by one.

Likewise:

``` powershell
$count--
```

decreases it by one.

------------------------------------------------------------------------

# Storing Command Output

Variables become especially useful when they store output from
PowerShell commands.

For example:

``` powershell
$services = Get-Service
```

The variable now contains the service objects returned by `Get-Service`.

Display them:

``` powershell
$services
```

You can pass the stored objects through the pipeline:

``` powershell
$services | Where-Object Status -eq 'Running'
```

Or inspect them:

``` powershell
$services | Get-Member
```

This connects variables directly to the object and pipeline concepts
from previous lessons.

------------------------------------------------------------------------

# Storing a Single Object

You can store one specific object:

``` powershell
$spooler = Get-Service -Name Spooler
```

Now:

``` powershell
$spooler
```

contains the service object.

Because it is an object, you can access its properties with dot
notation:

``` powershell
$spooler.Name
```

``` powershell
$spooler.Status
```

``` powershell
$spooler.DisplayName
```

Variables do not simply store text. They can store complete PowerShell
objects.

------------------------------------------------------------------------

# What Is a Data Type?

A **data type** describes the kind of data a value represents.

For example:

``` text
"PowerShell"  → Text
42            → Integer
3.14          → Decimal number
$true         → Boolean
```

PowerShell uses .NET types underneath its object system.

You do not need to memorize all of the available .NET types.

At this stage, focus on recognizing several common types and
understanding why type can affect how PowerShell treats a value.

------------------------------------------------------------------------

# Common PowerShell Data Types

Some common types you will encounter include:

  PowerShell/.NET Type          Common Name            Example
  ----------------------------- ---------------------- ----------------
  `System.String`               String / text          `'PowerShell'`
  `System.Int32`                Integer                `42`
  `System.Double`               Decimal number         `3.14`
  `System.Boolean`              Boolean                `$true`
  `System.DateTime`             Date and time          `Get-Date`
  `System.Array` / `Object[]`   Collection of values   `1,2,3`

You will encounter many other object types as you work with PowerShell
commands.

------------------------------------------------------------------------

# Discovering a Data Type with GetType()

You can ask an object what type it is by using:

``` powershell
.GetType()
```

For example:

``` powershell
$name = 'PowerShell'
$name.GetType()
```

You should see type information showing that the value is a string.

Try:

``` powershell
$count = 10
$count.GetType()
```

The result should indicate an integer type.

You can also use:

``` powershell
(Get-Date).GetType()
```

This shows that `Get-Date` returns a `DateTime` object.

------------------------------------------------------------------------

# GetType() vs. Get-Member

You now have two useful object discovery tools.

## GetType()

Use:

``` powershell
$value.GetType()
```

to answer:

> What type of object is this?

## Get-Member

Use:

``` powershell
$value | Get-Member
```

to answer:

> What properties and methods does this object have?

For example:

``` powershell
$date = Get-Date

$date.GetType()
```

Then:

``` powershell
$date | Get-Member
```

These tools complement each other.

------------------------------------------------------------------------

# Strings

A **string** represents text.

For example:

``` powershell
$message = 'Hello'
```

You can confirm the type:

``` powershell
$message.GetType()
```

Strings can contain letters, numbers, spaces, and symbols.

For example:

``` powershell
$assetTag = 'LT-10025'
```

Even though the value contains numbers, it is still text because it is
stored as a string.

------------------------------------------------------------------------

# Single Quotes and Double Quotes

PowerShell supports both single and double quotation marks for strings.

For example:

``` powershell
$name = 'Lyle'
```

and:

``` powershell
$name = "Lyle"
```

both create strings.

However, there is an important difference when variables appear inside
the string.

------------------------------------------------------------------------

# Variable Expansion

Double-quoted strings expand variables.

For example:

``` powershell
$name = 'Alex'

$message = "Hello, $name"
```

Display:

``` powershell
$message
```

The result is:

``` text
Hello, Alex
```

PowerShell replaced `$name` with its stored value.

This is called **variable expansion** or **string interpolation**.

------------------------------------------------------------------------

# Single-Quoted Strings

Single-quoted strings normally treat variable references as literal
text.

For example:

``` powershell
$name = 'Alex'

$message = 'Hello, $name'
```

The result is:

``` text
Hello, $name
```

PowerShell does not expand the variable.

A useful beginner rule is:

``` text
"Double quotes" → Variables can expand
'Single quotes' → Text is generally literal
```

------------------------------------------------------------------------

# Expressions Inside Strings

Sometimes you want to access a property or evaluate an expression inside
a double-quoted string.

Use the subexpression operator:

``` text
$()
```

For example:

``` powershell
$service = Get-Service -Name Spooler
```

Then:

``` powershell
"The service status is $($service.Status)"
```

PowerShell evaluates:

``` powershell
$service.Status
```

and inserts the result into the string.

Another example:

``` powershell
"Today is $(Get-Date)"
```

You will see `$()` frequently in PowerShell scripts.

------------------------------------------------------------------------

# Integers and Decimal Numbers

PowerShell can work directly with numeric values.

For example:

``` powershell
$number = 10
```

``` powershell
$number + 5
```

returns:

``` text
15
```

Decimal values are also supported:

``` powershell
$price = 19.95
```

You can inspect the types:

``` powershell
$number.GetType()
```

``` powershell
$price.GetType()
```

PowerShell chooses an appropriate numeric type based on the value and
context.

------------------------------------------------------------------------

# Why Data Types Matter

Consider:

``` powershell
10 + 5
```

PowerShell performs numeric addition and returns:

``` text
15
```

Now compare:

``` powershell
'10' + '5'
```

Because these values are strings, PowerShell joins the text and returns:

``` text
105
```

The values may look similar on the screen, but their types are
different.

> **Key Idea:** The data type affects what PowerShell can do with a
> value and how operators behave.

------------------------------------------------------------------------

# Boolean Values

A Boolean value represents:

``` text
True
False
```

PowerShell provides:

``` powershell
$true
```

and:

``` powershell
$false
```

For example:

``` powershell
$isEnabled = $true
```

Check its type:

``` powershell
$isEnabled.GetType()
```

Boolean values become especially important when you begin working with
conditions and control flow.

------------------------------------------------------------------------

# Comparison Results Are Boolean

Comparison operations usually return Boolean values.

For example:

``` powershell
10 -gt 5
```

returns:

``` text
True
```

While:

``` powershell
10 -lt 5
```

returns:

``` text
False
```

You used these comparison operators in the previous lesson when
filtering objects.

Now you can see that the filter conditions are expressions that evaluate
to Boolean values.

------------------------------------------------------------------------

# Arrays

An **array** is a collection that can contain multiple values.

For example:

``` powershell
$computers = 'PC-001', 'PC-002', 'PC-003'
```

Display the variable:

``` powershell
$computers
```

PowerShell outputs each value.

You can also create a numeric array:

``` powershell
$numbers = 1, 2, 3, 4, 5
```

------------------------------------------------------------------------

# Accessing Array Items

Array positions are identified using an **index**.

PowerShell array indexes begin at:

``` text
0
```

For example:

``` powershell
$computers = 'PC-001', 'PC-002', 'PC-003'
```

The indexes are:

``` text
Index 0 → PC-001
Index 1 → PC-002
Index 2 → PC-003
```

Access the first item:

``` powershell
$computers[0]
```

Access the second:

``` powershell
$computers[1]
```

------------------------------------------------------------------------

# Counting Array Items

Arrays have a `Count` property.

For example:

``` powershell
$computers.Count
```

If the array contains three items, PowerShell returns:

``` text
3
```

This demonstrates again that collections in PowerShell are objects with
useful properties.

------------------------------------------------------------------------

# Selecting Multiple Array Items

You can request more than one index:

``` powershell
$computers[0,2]
```

You can also use a range:

``` powershell
$numbers = 1, 2, 3, 4, 5

$numbers[1..3]
```

This returns the items at indexes `1`, `2`, and `3`.

Remember that indexing begins at zero.

------------------------------------------------------------------------

# The @() Array Operator

PowerShell provides the array subexpression operator:

``` text
@()
```

This can be useful when you want to ensure that a result is treated as
an array.

For example:

``` powershell
$services = @(Get-Service)
```

You do not need to use `@()` for every collection.

For now, recognize the syntax because you will encounter it in scripts.

------------------------------------------------------------------------

# Type Casting

Sometimes you want to explicitly tell PowerShell which data type a value
should use.

This is called **type casting** or **type conversion**.

For example:

``` powershell
[int]$number = '10'
```

PowerShell converts the string:

``` text
10
```

into an integer where possible.

You can verify:

``` powershell
$number.GetType()
```

------------------------------------------------------------------------

# Common Type Casts

Examples include:

``` powershell
[string]$name = 'PowerShell'
```

``` powershell
[int]$count = 10
```

``` powershell
[double]$price = 19.95
```

``` powershell
[bool]$enabled = $true
```

``` powershell
[datetime]$today = Get-Date
```

You do not need to explicitly type every variable.

PowerShell can usually determine the type automatically.

Type declarations become useful when you need to require or convert a
particular type.

------------------------------------------------------------------------

# Automatic Type Conversion

PowerShell often attempts to convert values automatically when
necessary.

For example:

``` powershell
[int]$number = '25'
```

PowerShell can convert the text `'25'` into the number `25`.

However:

``` powershell
[int]$number = 'PowerShell'
```

cannot be converted into a valid integer and will produce an error.

This is one reason understanding data types is important.

------------------------------------------------------------------------

# The \$null Value

PowerShell provides a special value:

``` powershell
$null
```

`$null` represents the absence of a value.

For example:

``` powershell
$result = $null
```

You can test whether a variable has no value:

``` powershell
$null -eq $result
```

If `$result` is null, this returns:

``` text
True
```

A commonly recommended PowerShell style is to place `$null` on the left
side of the comparison:

``` powershell
$null -eq $result
```

rather than:

``` powershell
$result -eq $null
```

This can avoid unexpected behavior when the variable contains a
collection.

------------------------------------------------------------------------

# Removing Variables

PowerShell provides:

``` powershell
Remove-Variable
```

For example:

``` powershell
$name = 'PowerShell'
```

Then:

``` powershell
Remove-Variable -Name name
```

Notice that when using `-Name`, you specify:

``` text
name
```

rather than:

``` text
$name
```

You can also clear a variable's value:

``` powershell
Clear-Variable -Name name
```

`Clear-Variable` keeps the variable but removes its current value.

------------------------------------------------------------------------

# Viewing Variables

You can view variables in the current PowerShell session with:

``` powershell
Get-Variable
```

To view a specific variable:

``` powershell
Get-Variable -Name computerName
```

You will notice many variables that you did not create.

PowerShell automatically provides a number of built-in and preference
variables.

You will encounter some of these later in the course.

------------------------------------------------------------------------

# Variables and the Pipeline

Variables work naturally with the pipeline.

For example:

``` powershell
$services = Get-Service
```

Now filter the stored objects:

``` powershell
$services |
    Where-Object Status -eq 'Running'
```

Then sort them:

``` powershell
$services |
    Where-Object Status -eq 'Running' |
    Sort-Object Name
```

Then select properties:

``` powershell
$services |
    Where-Object Status -eq 'Running' |
    Sort-Object Name |
    Select-Object Name, DisplayName, Status
```

The variable stores the objects so you can reuse them without running
`Get-Service` again.

------------------------------------------------------------------------

# A Practical Variable Workflow

Suppose you need to investigate a Windows service.

## Step 1 --- Store the Object

``` powershell
$service = Get-Service -Name Spooler
```

## Step 2 --- Inspect Its Type

``` powershell
$service.GetType()
```

## Step 3 --- Inspect Its Members

``` powershell
$service | Get-Member
```

## Step 4 --- Access Properties

``` powershell
$service.Name
```

``` powershell
$service.Status
```

## Step 5 --- Use the Values in a Message

``` powershell
"The $($service.DisplayName) service is currently $($service.Status)."
```

You are combining several concepts from previous lessons:

``` text
Commands
   ↓
Objects
   ↓
Variables
   ↓
Properties
   ↓
Data Types
   ↓
Reusable Values
```

------------------------------------------------------------------------

# Variables Are Objects Too

A useful way to think about PowerShell variables is:

> A variable holds a reference to a value or object.

That means the same object-discovery skills still apply.

For example:

``` powershell
$date = Get-Date
```

Then:

``` powershell
$date.GetType()
```

and:

``` powershell
$date | Get-Member
```

and:

``` powershell
$date.Year
```

and:

``` powershell
$date.DayOfWeek
```

The variable gives you a convenient name for the object while preserving
its properties and methods.

------------------------------------------------------------------------

# Build on What You Already Know

The concepts from earlier lessons now connect together.

``` text
Find a command
      ↓
Get-Command

Learn the command
      ↓
Get-Help

Run the command
      ↓
Receive objects

Inspect the objects
      ↓
Get-Member

Store the objects
      ↓
$variable

Filter and sort
      ↓
Where-Object / Sort-Object

Reuse the results
      ↓
Properties / Pipeline / Expressions
```

PowerShell becomes much easier once you stop thinking of variables as
simple text containers and start thinking of them as names that can
refer to complete objects.

------------------------------------------------------------------------

# Key Takeaways

-   PowerShell variable names begin with `$`.
-   The `=` operator assigns a value to a variable.
-   Variables can store text, numbers, Boolean values, arrays, command
    output, and complete PowerShell objects.
-   Use meaningful variable names to make scripts easier to understand.
-   A data type describes the kind of value or object being stored.
-   Common types include strings, integers, decimal numbers, Boolean
    values, dates, and arrays.
-   `.GetType()` identifies an object's type.
-   `Get-Member` shows an object's properties and methods.
-   Double-quoted strings support variable expansion.
-   Single-quoted strings generally treat variable references literally.
-   `$()` can evaluate expressions inside expandable strings.
-   Arrays store multiple values.
-   Array indexing begins at `0`.
-   The `Count` property tells you how many items are in a collection.
-   Type casting can explicitly convert or constrain a value's type.
-   `$null` represents the absence of a value.
-   Variables work naturally with PowerShell objects and pipelines.
-   Variables allow you to store results once and reuse them throughout
    a command sequence or script.

------------------------------------------------------------------------

# Lab

Ready to practice working with PowerShell variables and data types?

Continue to:

[Lab 07 --- Variables and Data
Types](../labs/lab-07-variables-and-data-types.md)

In the lab, you will create and update variables, store command output,
inspect data types, work with strings and numbers, practice variable
expansion, create arrays, access array elements, use type casting, and
work with `$null`.

------------------------------------------------------------------------

## Additional Resources

-   [About Variables --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_variables?view=powershell-7.6)
-   [About Automatic Variables --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_automatic_variables?view=powershell-7.6)
-   [About Arrays --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_arrays?view=powershell-7.6)
-   [About Type Conversion --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_type_conversion?view=powershell-7.6)
-   [About Quoting Rules --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_quoting_rules?view=powershell-7.6)
-   [About Null --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_null?view=powershell-7.6)
