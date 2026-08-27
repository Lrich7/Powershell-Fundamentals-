[lesson-09-operators.md](https://github.com/user-attachments/files/31517164/lesson-09-operators.md)

# Lesson 09 --- Operators

## Learning Objectives

By the end of this lesson, you will be able to:

-   Explain what an operator is in PowerShell.
-   Use arithmetic operators to perform calculations.
-   Use assignment operators to store and update values.
-   Use comparison operators to compare values.
-   Use wildcard and regular expression comparison operators.
-   Use containment operators to test collections.
-   Use logical operators to combine conditions.
-   Understand operator precedence and use parentheses to make
    expressions clear.
-   Use type-related operators such as `-is` and `-as`.
-   Recognize common operators used with strings, arrays, and ranges.
-   Apply operators in filters, variables, and basic PowerShell
    expressions.

------------------------------------------------------------------------

## What Is an Operator?

An **operator** is a symbol or keyword that tells PowerShell to perform
an operation on one or more values.

You have already used several operators in previous lessons.

For example:

``` powershell
$count = 10
```

The:

``` text
=
```

is an assignment operator.

In:

``` powershell
10 + 5
```

the:

``` text
+
```

is an arithmetic operator.

And in:

``` powershell
$_.Status -eq 'Running'
```

the:

``` text
-eq
```

is a comparison operator.

Operators are used throughout PowerShell for:

-   Calculations
-   Assigning values
-   Comparing values
-   Filtering objects
-   Testing conditions
-   Working with strings
-   Working with collections
-   Controlling script logic

> **Key Idea:** Operators allow PowerShell to evaluate, compare,
> combine, and manipulate values.

------------------------------------------------------------------------

# Arithmetic Operators

Arithmetic operators perform mathematical operations.

Common PowerShell arithmetic operators include:

  Operator   Meaning
  ---------- ---------------------
  `+`        Addition
  `-`        Subtraction
  `*`        Multiplication
  `/`        Division
  `%`        Modulus / remainder

------------------------------------------------------------------------

## Addition

Use:

``` text
+
```

to add numbers.

For example:

``` powershell
10 + 5
```

returns:

``` text
15
```

You can also use variables:

``` powershell
$firstNumber = 10
$secondNumber = 5

$firstNumber + $secondNumber
```

------------------------------------------------------------------------

## Subtraction

Use:

``` text
-
```

to subtract values.

``` powershell
10 - 5
```

returns:

``` text
5
```

------------------------------------------------------------------------

## Multiplication

Use:

``` text
*
```

for multiplication.

``` powershell
10 * 5
```

returns:

``` text
50
```

------------------------------------------------------------------------

## Division

Use:

``` text
/
```

for division.

``` powershell
10 / 2
```

returns:

``` text
5
```

------------------------------------------------------------------------

## Modulus

The modulus operator is:

``` text
%
```

It returns the remainder after division.

For example:

``` powershell
10 % 3
```

returns:

``` text
1
```

because `10` divided by `3` leaves a remainder of `1`.

This is useful for tasks such as determining whether a number is even or
odd.

For example:

``` powershell
10 % 2
```

returns:

``` text
0
```

because `10` is evenly divisible by `2`.

------------------------------------------------------------------------

# Assignment Operators

Assignment operators store or update values in variables.

The most basic assignment operator is:

``` text
=
```

For example:

``` powershell
$count = 10
```

This assigns the value `10` to `$count`.

------------------------------------------------------------------------

# Compound Assignment Operators

PowerShell provides operators that combine an operation with assignment.

Common examples include:

  Operator   Example     Similar To
  ---------- ----------- ---------------
  `+=`       `$x += 5`   `$x = $x + 5`
  `-=`       `$x -= 5`   `$x = $x - 5`
  `*=`       `$x *= 2`   `$x = $x * 2`
  `/=`       `$x /= 2`   `$x = $x / 2`
  `%=`       `$x %= 3`   `$x = $x % 3`

For example:

``` powershell
$count = 10
$count += 5
```

Now `$count` contains:

``` text
15
```

------------------------------------------------------------------------

# Increment and Decrement

PowerShell supports increment and decrement operators.

To increase a numeric value by one:

``` powershell
$count++
```

To decrease it by one:

``` powershell
$count--
```

For example:

``` powershell
$count = 5
$count++
$count
```

returns:

``` text
6
```

You will often see these operators used in loops and counters.

------------------------------------------------------------------------

# Comparison Operators

Comparison operators compare values.

You used many of these in Lesson 06 when filtering objects.

Common comparison operators include:

  Operator   Meaning
  ---------- --------------------------
  `-eq`      Equal to
  `-ne`      Not equal to
  `-gt`      Greater than
  `-ge`      Greater than or equal to
  `-lt`      Less than
  `-le`      Less than or equal to

Comparison expressions normally return a Boolean value:

``` text
True
```

or:

``` text
False
```

------------------------------------------------------------------------

## Equal To

``` powershell
10 -eq 10
```

returns:

``` text
True
```

While:

``` powershell
10 -eq 5
```

returns:

``` text
False
```

------------------------------------------------------------------------

## Not Equal To

``` powershell
10 -ne 5
```

returns:

``` text
True
```

------------------------------------------------------------------------

## Greater Than

``` powershell
10 -gt 5
```

returns:

``` text
True
```

------------------------------------------------------------------------

## Greater Than or Equal To

``` powershell
10 -ge 10
```

returns:

``` text
True
```

------------------------------------------------------------------------

## Less Than

``` powershell
5 -lt 10
```

returns:

``` text
True
```

------------------------------------------------------------------------

## Less Than or Equal To

``` powershell
5 -le 5
```

returns:

``` text
True
```

------------------------------------------------------------------------

# Comparing Strings

Comparison operators also work with strings.

For example:

``` powershell
'PowerShell' -eq 'PowerShell'
```

returns:

``` text
True
```

By default, many PowerShell string comparison operators are
case-insensitive.

For example:

``` powershell
'PowerShell' -eq 'powershell'
```

normally returns:

``` text
True
```

------------------------------------------------------------------------

# Case-Sensitive Comparisons

PowerShell provides case-sensitive versions of comparison operators by
adding:

``` text
c
```

For example:

``` powershell
'PowerShell' -ceq 'powershell'
```

returns:

``` text
False
```

Case-sensitive versions include:

``` text
-ceq
-cne
-cgt
-cge
-clt
-cle
-clike
-cmatch
```

You can also explicitly use case-insensitive versions with:

``` text
i
```

such as:

``` text
-ieq
-ilike
-imatch
```

The normal operators are already case-insensitive in typical string
comparisons, so you will not always need the `i` versions.

------------------------------------------------------------------------

# Wildcard Comparison Operators

PowerShell provides:

``` text
-like
-notlike
```

for wildcard matching.

The most common wildcard is:

``` text
*
```

which represents zero or more characters.

For example:

``` powershell
'PowerShell' -like 'Power*'
```

returns:

``` text
True
```

Another example:

``` powershell
'Server01' -like 'Server*'
```

returns:

``` text
True
```

------------------------------------------------------------------------

# The ? Wildcard

The:

``` text
?
```

wildcard represents a single character.

For example:

``` powershell
'Server01' -like 'Server0?'
```

returns:

``` text
True
```

because the final `?` matches one character.

------------------------------------------------------------------------

# -notlike

Use:

``` text
-notlike
```

when a value should **not** match a wildcard pattern.

For example:

``` powershell
'Server01' -notlike 'PC*'
```

returns:

``` text
True
```

------------------------------------------------------------------------

# Regular Expression Operators

PowerShell provides:

``` text
-match
-notmatch
```

for regular expression matching.

For example:

``` powershell
'Server01' -match '^Server'
```

returns:

``` text
True
```

The:

``` text
^
```

means the pattern must appear at the beginning of the string.

Another example:

``` powershell
'Server01' -match '\d+$'
```

checks whether the value ends with one or more digits.

Regular expressions are a large topic by themselves.

For now, remember:

``` text
-like   → Wildcards
-match  → Regular expressions
```

------------------------------------------------------------------------

# Containment Operators

Containment operators test whether a collection contains a particular
value.

Two important operators are:

``` text
-contains
-notcontains
```

For example:

``` powershell
$servers = 'Server01', 'Server02', 'Server03'
```

Then:

``` powershell
$servers -contains 'Server02'
```

returns:

``` text
True
```

------------------------------------------------------------------------

# -notcontains

You can test whether a collection does **not** contain a value:

``` powershell
$servers -notcontains 'Server04'
```

This returns:

``` text
True
```

if `Server04` is not present.

------------------------------------------------------------------------

# -in and -notin

PowerShell also provides:

``` text
-in
-notin
```

These perform similar tests but reverse the order of the operands.

For example:

``` powershell
'Server02' -in $servers
```

returns:

``` text
True
```

Compare:

``` powershell
$servers -contains 'Server02'
```

with:

``` powershell
'Server02' -in $servers
```

A useful way to remember the difference is:

``` text
Collection -contains Value
Value -in Collection
```

------------------------------------------------------------------------

# Logical Operators

Logical operators combine or reverse conditions.

The most common are:

  Operator   Meaning
  ---------- ------------------------------------------
  `-and`     Both conditions must be true
  `-or`      At least one condition must be true
  `-xor`     One condition must be true, but not both
  `-not`     Reverses a Boolean condition
  `!`        Short form of `-not`

------------------------------------------------------------------------

# Using -and

Use:

``` text
-and
```

when both conditions must be true.

For example:

``` powershell
$age = 25

($age -ge 18) -and ($age -lt 65)
```

Both comparisons are true, so the result is:

``` text
True
```

You have already seen this used in filtering:

``` powershell
Get-Process |
    Where-Object {
        $_.CPU -gt 10 -and
        $_.Name -like 'p*'
    }
```

------------------------------------------------------------------------

# Using -or

Use:

``` text
-or
```

when either condition may be true.

For example:

``` powershell
$status = 'Warning'

($status -eq 'Warning') -or ($status -eq 'Error')
```

returns:

``` text
True
```

------------------------------------------------------------------------

# Using -xor

`-xor` means **exclusive OR**.

It returns `True` when one condition is true but not both.

For example:

``` powershell
$true -xor $false
```

returns:

``` text
True
```

While:

``` powershell
$true -xor $true
```

returns:

``` text
False
```

You will use `-and` and `-or` much more frequently, but it is useful to
recognize `-xor`.

------------------------------------------------------------------------

# Using -not

`-not` reverses a Boolean value.

For example:

``` powershell
-not $true
```

returns:

``` text
False
```

You can also use:

``` text
!
```

For example:

``` powershell
!$true
```

returns:

``` text
False
```

The longer `-not` form is often easier to read in beginner scripts.

------------------------------------------------------------------------

# Type Operators

PowerShell provides operators that can test or convert object types.

Two useful operators are:

``` text
-is
-isnot
```

For example:

``` powershell
$value = 'PowerShell'

$value -is [string]
```

returns:

``` text
True
```

Another example:

``` powershell
$value -is [int]
```

returns:

``` text
False
```

------------------------------------------------------------------------

# The -isnot Operator

Use:

``` text
-isnot
```

to test whether a value is not a particular type.

For example:

``` powershell
$value = 10

$value -isnot [string]
```

returns:

``` text
True
```

------------------------------------------------------------------------

# The -as Operator

The:

``` text
-as
```

operator attempts to convert a value to another type.

For example:

``` powershell
'25' -as [int]
```

returns the numeric value:

``` text
25
```

If PowerShell cannot perform the conversion using `-as`, the result is
typically `$null` rather than a terminating conversion error.

For example:

``` powershell
'PowerShell' -as [int]
```

does not produce a valid integer.

This differs from explicit casting:

``` powershell
[int]'PowerShell'
```

which produces a conversion error.

------------------------------------------------------------------------

# The Range Operator

You used the range operator in Lesson 08.

The range operator is:

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

It can also count downward:

``` powershell
5..1
```

returns:

``` text
5
4
3
2
1
```

This operator is especially useful when creating sequences or working
with loops.

------------------------------------------------------------------------

# The Join Operator

PowerShell provides:

``` text
-join
```

for combining multiple values into a single string.

For example:

``` powershell
$servers = 'Server01', 'Server02', 'Server03'
```

Then:

``` powershell
$servers -join ', '
```

returns:

``` text
Server01, Server02, Server03
```

This is useful when converting collections into readable text.

------------------------------------------------------------------------

# The Split Operator

PowerShell provides:

``` text
-split
```

for splitting text into multiple values.

For example:

``` powershell
'Server01,Server02,Server03' -split ','
```

returns:

``` text
Server01
Server02
Server03
```

The result is a collection of strings.

This creates a useful relationship:

``` text
Collection
   ↓
-join
   ↓
String

String
   ↓
-split
   ↓
Collection
```

------------------------------------------------------------------------

# Operator Precedence

When an expression contains multiple operators, PowerShell follows rules
that determine which operations happen first.

This is called **operator precedence**.

For example:

``` powershell
2 + 3 * 4
```

Multiplication occurs before addition.

The result is:

``` text
14
```

If you want the addition to happen first, use parentheses:

``` powershell
(2 + 3) * 4
```

The result is:

``` text
20
```

------------------------------------------------------------------------

# Use Parentheses for Clarity

Even when you understand operator precedence, parentheses can make your
intent easier to read.

For example:

``` powershell
($age -ge 18) -and ($age -lt 65)
```

is easier to understand than a long expression with no visual grouping.

This becomes especially important when you build complex conditions.

> **Tip:** Parentheses are not only about making PowerShell evaluate an
> expression correctly. They also make your code easier for people to
> understand.

------------------------------------------------------------------------

# Operators with Collections

Some comparison operators behave differently when the value on the left
is a collection.

For example:

``` powershell
1, 2, 3, 4, 5 -gt 3
```

PowerShell returns the elements that satisfy the comparison:

``` text
4
5
```

This can be useful, but it is important to recognize that comparisons
involving collections may return matching elements rather than a single
Boolean value.

For explicit collection membership tests, operators such as:

``` text
-contains
-in
```

are often easier to understand.

------------------------------------------------------------------------

# Operators in Where-Object

Operators become especially useful when filtering PowerShell objects.

For example:

``` powershell
Get-Service |
    Where-Object Status -eq 'Running'
```

uses:

``` text
-eq
```

Another example:

``` powershell
Get-Process |
    Where-Object CPU -gt 10
```

uses:

``` text
-gt
```

And:

``` powershell
Get-Service |
    Where-Object Name -like 'Win*'
```

uses:

``` text
-like
```

Complex filters can combine operators:

``` powershell
Get-Service |
    Where-Object {
        $_.Status -eq 'Running' -and
        $_.Name -like 'Win*'
    }
```

------------------------------------------------------------------------

# A Practical Operator Example

Suppose you have:

``` powershell
$computers = 'PC-001', 'PC-002', 'Server01', 'Server02'
```

You can test whether a computer exists:

``` powershell
$computers -contains 'PC-001'
```

You can filter the collection:

``` powershell
$computers -like 'Server*'
```

You can count the matching values:

``` powershell
($computers -like 'Server*').Count
```

You can join them into a string:

``` powershell
($computers -like 'Server*') -join ', '
```

This produces:

``` text
Server01, Server02
```

Several operators can work together to solve a practical task.

------------------------------------------------------------------------

# Build Expressions One Step at a Time

Complex expressions are easier to troubleshoot when built gradually.

Start with:

``` powershell
$process = Get-Process |
    Select-Object -First 1
```

Inspect a property:

``` powershell
$process.CPU
```

Test it:

``` powershell
$process.CPU -gt 10
```

Then combine conditions:

``` powershell
($process.CPU -gt 10) -and ($process.Name -like 'p*')
```

Building expressions gradually helps you see which part is producing an
unexpected result.

------------------------------------------------------------------------

# Operators Connect Earlier Lessons

Operators are already part of almost every PowerShell concept you have
learned.

``` text
Variables
   ↓
=

Math
   ↓
+ - * / %

Filtering
   ↓
-eq -gt -like -match

Collections
   ↓
-contains -in

Conditions
   ↓
-and -or -not

Types
   ↓
-is -as

Strings and Arrays
   ↓
-join -split

Sequences
   ↓
..
```

Learning these operators gives you the building blocks for more advanced
scripting.

------------------------------------------------------------------------

# Key Takeaways

-   Operators tell PowerShell to perform operations on values or
    objects.
-   Arithmetic operators include `+`, `-`, `*`, `/`, and `%`.
-   `=` assigns values to variables.
-   Compound assignment operators such as `+=` update existing values.
-   `++` and `--` increment and decrement numeric values.
-   Comparison operators such as `-eq`, `-ne`, `-gt`, and `-lt` compare
    values.
-   `-like` uses wildcard patterns.
-   `-match` uses regular expressions.
-   `-contains` and `-in` test collection membership.
-   `-and`, `-or`, `-xor`, and `-not` combine or reverse Boolean
    conditions.
-   `-is` and `-isnot` test object types.
-   `-as` attempts type conversion.
-   `-join` combines values into a string.
-   `-split` divides a string into multiple values.
-   `..` creates a range of values.
-   Operator precedence controls the order in which expressions are
    evaluated.
-   Parentheses make complex expressions easier to control and
    understand.
-   Operators are fundamental to filtering, conditions, loops, and
    PowerShell scripting.

------------------------------------------------------------------------

# Lab

Ready to practice PowerShell operators?

Continue to:

[Lab 09 --- Operators](../labs/lab-09-operators.md)

In the lab, you will practice arithmetic and assignment operators,
compare values, work with wildcards and collections, combine logical
conditions, test data types, join and split strings, and build practical
filtering expressions.

------------------------------------------------------------------------

## Additional Resources

-   [About Operators --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_operators?view=powershell-7.6)
-   [About Arithmetic Operators --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_arithmetic_operators?view=powershell-7.6)
-   [About Assignment Operators --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_assignment_operators?view=powershell-7.6)
-   [About Comparison Operators --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_comparison_operators?view=powershell-7.6)
-   [About Logical Operators --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_logical_operators?view=powershell-7.6)
-   [About Type Operators --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_type_operators?view=powershell-7.6)
-   [About Join --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_join?view=powershell-7.6)
-   [About Split --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_split?view=powershell-7.6)
-   [About Operator Precedence --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_operator_precedence?view=powershell-7.6)
