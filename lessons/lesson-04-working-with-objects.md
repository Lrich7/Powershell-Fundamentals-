[lesson-04-working-with-objects.md](https://github.com/user-attachments/files/31517032/lesson-04-working-with-objects.md)

# Lesson 04 --- Working with Objects

## Learning Objectives

By the end of this lesson, you will be able to:

-   Explain what an object is in PowerShell.
-   Understand the difference between text and structured PowerShell
    objects.
-   Identify an object's properties and methods.
-   Use `Get-Member` to inspect objects.
-   Access individual object properties with dot notation.
-   Use `Select-Object` to choose specific properties.
-   Use `Where-Object` to filter objects.
-   Recognize the role objects play in the PowerShell pipeline.
-   Use Help and object discovery together when working with unfamiliar
    output.

------------------------------------------------------------------------

## PowerShell Works with Objects

One of the most important concepts in PowerShell is that PowerShell
works with **objects**.

When you run a command such as:

``` powershell
Get-Service
```

PowerShell displays information that looks like a table.

You might see output similar to:

``` text
Status   Name               DisplayName
------   ----               -----------
Running  EventLog           Windows Event Log
Running  Spooler            Print Spooler
Stopped  W32Time            Windows Time
```

It is easy to think that PowerShell simply returned formatted text.

It did not.

`Get-Service` returned **objects** that contain structured information
about Windows services.

PowerShell then formatted those objects so they were easy for you to
read.

> **Key Idea:** What you see on the screen is often only a formatted
> view of the underlying objects.

------------------------------------------------------------------------

# What Is an Object?

An object is a structured representation of something.

That "something" might be:

-   A Windows service
-   A running process
-   A file
-   A folder
-   A user
-   A computer
-   A network adapter
-   A PowerShell command
-   A Microsoft 365 resource

Objects contain information about the thing they represent.

Two important parts of an object are:

``` text
Properties
Methods
```

------------------------------------------------------------------------

# Properties

A **property** describes something about an object.

For example, a service object can have properties such as:

``` text
Name
DisplayName
Status
ServiceType
StartType
```

Think of properties as information about the object.

For a person, properties might be:

``` text
Name
Department
JobTitle
EmailAddress
```

For a file, properties might include:

``` text
Name
Length
Extension
CreationTime
LastWriteTime
```

Properties answer questions such as:

> What information does this object contain?

------------------------------------------------------------------------

# Methods

A **method** is an action that an object can perform.

Methods are different from properties.

A property describes the object.

A method performs an action.

Conceptually:

``` text
Object
├── Properties → Information about the object
└── Methods    → Actions the object can perform
```

You do not need to memorize methods at this point.

The important concept is understanding that PowerShell objects can
contain both **data** and **actions**.

------------------------------------------------------------------------

# Discovering Objects with Get-Member

How do you know what properties and methods an object contains?

Use:

``` powershell
Get-Member
```

`Get-Member` is one of PowerShell's most important object-discovery
commands.

However, `Get-Member` normally needs an object to inspect.

This is where the **pipeline** begins to become important.

Run:

``` powershell
Get-Service | Get-Member
```

The `|` character is called the **pipeline operator**.

It sends the objects produced by `Get-Service` to `Get-Member`.

Conceptually:

``` text
Get-Service
     │
     ▼
 Service Objects
     │
     ▼
 Get-Member
```

`Get-Member` then tells you what those service objects contain.

------------------------------------------------------------------------

# Reading Get-Member Output

When you run:

``` powershell
Get-Service | Get-Member
```

you will see information including:

``` text
TypeName
Name
MemberType
Definition
```

The output can look overwhelming at first.

For now, focus mainly on:

``` text
Property
Method
```

You may also see other member types such as:

``` text
AliasProperty
CodeProperty
ScriptProperty
```

You do not need to understand every member type yet.

The goal is to learn how to investigate an unfamiliar object.

------------------------------------------------------------------------

# Viewing Only Properties

You can ask `Get-Member` to display properties:

``` powershell
Get-Service | Get-Member -MemberType Property
```

This makes it easier to see the information available on service
objects.

Try the same idea with processes:

``` powershell
Get-Process | Get-Member -MemberType Property
```

And files:

``` powershell
Get-ChildItem | Get-Member -MemberType Property
```

Different object types have different properties.

------------------------------------------------------------------------

# Viewing Methods

You can also inspect methods:

``` powershell
Get-Service | Get-Member -MemberType Method
```

Again, you do not need to memorize these methods.

The important skill is knowing how to discover them.

------------------------------------------------------------------------

# Different Commands Return Different Objects

Compare:

``` powershell
Get-Service | Get-Member
```

with:

``` powershell
Get-Process | Get-Member
```

and:

``` powershell
Get-ChildItem | Get-Member
```

Each command returns a different type of object.

Because the objects represent different things, they contain different
properties and methods.

For example:

``` text
Get-Service
    ↓
Service objects
    ↓
Name, Status, DisplayName, StartType...

Get-Process
    ↓
Process objects
    ↓
Name, Id, CPU, WorkingSet...

Get-ChildItem
    ↓
File and directory objects
    ↓
Name, Length, Extension, LastWriteTime...
```

> **Key Idea:** Understanding what type of object a command returns
> helps you understand what you can do with that output.

------------------------------------------------------------------------

# Accessing Object Properties

Once you know that an object contains a property, you can access that
property directly.

First, store an object in a variable:

``` powershell
$service = Get-Service -Name Spooler
```

Now inspect the variable:

``` powershell
$service
```

To access the service's `Name` property:

``` powershell
$service.Name
```

To access its status:

``` powershell
$service.Status
```

To access its display name:

``` powershell
$service.DisplayName
```

The period between the variable and property is called **dot notation**.

``` text
$service.Status
    │       │
    │       └── Property
    │
    └────────── Object stored in the variable
```

The basic pattern is:

``` powershell
$object.Property
```

------------------------------------------------------------------------

# Discover First, Then Use

If you do not know which properties an object has, do not guess.

Use:

``` powershell
$service | Get-Member
```

Then access the property you need:

``` powershell
$service.Status
```

This creates another useful PowerShell workflow:

``` text
Run → Inspect → Use
```

For example:

``` powershell
$service = Get-Service -Name Spooler

$service | Get-Member

$service.Status
```

------------------------------------------------------------------------

# Selecting Properties with Select-Object

PowerShell commands often return more information than you need.

`Select-Object` allows you to choose which properties you want to
display.

For example:

``` powershell
Get-Service | Select-Object Name, Status
```

This selects only the `Name` and `Status` properties.

Another example:

``` powershell
Get-Process | Select-Object Name, Id, CPU
```

Instead of working with every available property, you can focus on the
information that matters to you.

------------------------------------------------------------------------

## Selecting the First Few Objects

`Select-Object` can also limit the number of objects returned.

For example:

``` powershell
Get-Process | Select-Object -First 5
```

This returns the first five process objects.

You can combine this with property selection:

``` powershell
Get-Process | Select-Object -First 5 Name, Id, CPU
```

------------------------------------------------------------------------

# Filtering Objects with Where-Object

Once commands return objects, you can filter them based on their
properties.

One common command for this is:

``` powershell
Where-Object
```

For example, you can display only running services:

``` powershell
Get-Service | Where-Object Status -eq 'Running'
```

Conceptually:

``` text
Get-Service
     │
     ▼
All Service Objects
     │
     ▼
Where-Object
Status equals Running
     │
     ▼
Running Services
```

------------------------------------------------------------------------

# Comparison Operators

The previous example uses:

``` text
-eq
```

`-eq` means:

``` text
equal to
```

PowerShell uses comparison operators such as:

``` text
-eq    Equal to
-ne    Not equal to
-gt    Greater than
-lt    Less than
-ge    Greater than or equal to
-le    Less than or equal to
```

You will use comparison operators frequently when filtering objects.

For now, focus on recognizing what they do rather than memorizing every
operator.

------------------------------------------------------------------------

# Filtering with a Script Block

You may also see `Where-Object` written like this:

``` powershell
Get-Service | Where-Object { $_.Status -eq 'Running' }
```

This syntax introduces two important pieces.

The braces:

``` text
{ }
```

create a **script block**.

Inside the script block:

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

The complete condition:

``` powershell
$_.Status -eq 'Running'
```

means:

> Keep the current object if its Status property equals Running.

Do not worry if this syntax feels unfamiliar. You will see it repeatedly
as you continue learning PowerShell.

------------------------------------------------------------------------

# Objects and the Pipeline

The pipeline is one of the most powerful features in PowerShell.

The pipeline operator is:

``` text
|
```

It takes the output from one command and sends it to another command.

For example:

``` powershell
Get-Service | Get-Member
```

PowerShell is passing **service objects**, not simply copying text from
one command to another.

Another example:

``` powershell
Get-Service | Where-Object Status -eq 'Running'
```

Service objects are passed to `Where-Object`, which examines their
`Status` property.

Then:

``` powershell
Get-Service |
    Where-Object Status -eq 'Running' |
    Select-Object Name, Status
```

Conceptually:

``` text
Get-Service
     │
     ▼
Service Objects
     │
     ▼
Where-Object
Keep Running Services
     │
     ▼
Select-Object
Choose Name and Status
     │
     ▼
Final Output
```

This is the foundation of working effectively with PowerShell.

------------------------------------------------------------------------

# A Practical Object Discovery Workflow

Suppose you want to work with running processes but do not know what
information PowerShell provides about them.

## Step 1 --- Run the Command

``` powershell
Get-Process
```

## Step 2 --- Inspect the Objects

``` powershell
Get-Process | Get-Member
```

## Step 3 --- Identify Useful Properties

You discover properties such as:

``` text
Name
Id
CPU
WorkingSet
```

## Step 4 --- Select the Information You Need

``` powershell
Get-Process | Select-Object Name, Id, CPU
```

## Step 5 --- Filter the Objects

For example:

``` powershell
Get-Process | Where-Object CPU -gt 10
```

The workflow becomes:

``` text
Run → Inspect → Select → Filter
```

This pattern can be used with many different PowerShell commands.

------------------------------------------------------------------------

# Combining Command and Object Discovery

The skills from the previous lessons now begin to work together.

Suppose you need information about Windows services.

### Find the command

``` powershell
Get-Command *Service*
```

### Learn about the command

``` powershell
Get-Help Get-Service -Examples
```

### Run the command

``` powershell
Get-Service
```

### Discover the returned object

``` powershell
Get-Service | Get-Member
```

### Select useful properties

``` powershell
Get-Service | Select-Object Name, Status, StartType
```

### Filter the objects

``` powershell
Get-Service |
    Where-Object Status -eq 'Running' |
    Select-Object Name, Status, StartType
```

You now have a broader PowerShell discovery workflow:

``` text
Find → Learn → Run → Inspect → Select → Filter
```

You will continue building on this workflow throughout the course.

------------------------------------------------------------------------

# Objects vs. Text

Traditional command-line tools often produce text.

PowerShell is different because it normally passes structured objects
between PowerShell commands.

Consider:

``` powershell
Get-Service | Where-Object Status -eq 'Running'
```

PowerShell does not need to search visually through formatted text for
the word `Running`.

It can inspect the actual:

``` text
Status
```

property of each service object.

This makes commands more predictable and allows complex tasks to be
built by combining relatively simple commands.

> **Key Idea:** PowerShell's object-based pipeline lets commands work
> with structured data rather than forcing you to parse formatted text.

------------------------------------------------------------------------

# Don't Memorize Every Property

Just as you do not need to memorize every PowerShell command, you do not
need to memorize every object's properties and methods.

Use discovery tools.

``` text
What command do I need?
        ↓
Get-Command

How does the command work?
        ↓
Get-Help

What did the command return?
        ↓
Get-Member

What information do I want?
        ↓
Select-Object

Which objects do I want?
        ↓
Where-Object
```

These tools allow you to investigate PowerShell as you work.

------------------------------------------------------------------------

# Key Takeaways

-   PowerShell commands return **objects**, not just formatted text.
-   Objects contain structured information.
-   **Properties** describe an object.
-   **Methods** represent actions associated with an object.
-   `Get-Member` shows the properties and methods available on an
    object.
-   The pipeline operator `|` passes objects from one command to
    another.
-   Dot notation can access an object's individual properties.
-   `Select-Object` chooses specific objects or properties.
-   `Where-Object` filters objects based on their properties.
-   `$_` represents the current pipeline object inside many script
    blocks.
-   Different commands return different types of objects.
-   You do not need to memorize every property or method---use
    PowerShell's discovery tools.
-   The skills from earlier lessons combine into a useful workflow:

``` text
Find → Learn → Run → Inspect → Select → Filter
```

------------------------------------------------------------------------

# Lab

Ready to practice working with PowerShell objects?

Continue to:

[Lab 04 — Working with Objects](../labs/lesson-04-lab-04-working-with-objects.md)


In the lab, you will inspect objects with `Get-Member`, identify useful
properties, access properties with dot notation, select information with
`Select-Object`, and filter objects with `Where-Object`.

------------------------------------------------------------------------

## Additional Resources

-   [Discovering Objects, Properties, and Methods --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/scripting/learn/ps101/03-discovering-objects?view=powershell-7.6)
-   [Get-Member --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/get-member?view=powershell-7.6)
-   [Select-Object --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/select-object?view=powershell-7.6)
-   [Where-Object --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/where-object?view=powershell-7.6)
-   [About Pipelines --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_pipelines?view=powershell-7.6)
