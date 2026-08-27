# Lab 12 --- Loops

## Lab Objective

In this lab, you will practice repeating PowerShell tasks efficiently.

You will:

-   Process collections with `foreach`.
-   Process pipeline objects with `ForEach-Object`.
-   Use a `for` loop and array indexes.
-   Use `while`, `do while`, and `do until`.
-   Use `break` and `continue`.
-   Filter before looping.
-   Apply the `Collect → Filter → Loop → Act` pattern.

------------------------------------------------------------------------

## Before You Begin

Complete Lessons 01--12.

------------------------------------------------------------------------

# Exercise 1 --- foreach

Create:

``` powershell
$computers = 'PC-001', 'PC-002', 'PC-003'
```

Process each:

``` powershell
foreach ($computer in $computers) {
    "Checking $computer"
}
```

Modify the loop so it displays:

``` text
Beginning check for PC-001
Beginning check for PC-002
Beginning check for PC-003
```

``` powershell
# Your loop:
```

------------------------------------------------------------------------

# Exercise 2 --- Loop Through Objects

Store:

``` powershell
$services = Get-Service
```

Display each service name and status:

``` powershell
foreach ($service in $services) {
    "$($service.Name) - $($service.Status)"
}
```

### Question

Why can you use `$service.Name` inside the loop?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 3 --- ForEach-Object

Run:

``` powershell
Get-Service |
    Select-Object -First 10 |
    ForEach-Object {
        "$($_.Name) - $($_.Status)"
    }
```

Complete:

``` text
Inside ForEach-Object, $_ represents:
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 4 --- foreach vs. ForEach-Object

Create a collection first and process it with `foreach`.

Then process `Get-Service` directly through `ForEach-Object`.

Write one sentence explaining when you would choose each:

``` text
foreach:
____________________________________________________

ForEach-Object:
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 5 --- for Loop

Run:

``` powershell
for ($i = 1; $i -le 5; $i++) {
    "Iteration $i"
}
```

Identify:

``` text
Starting value: ____________________
Condition:      ____________________
Update:         ____________________
```

------------------------------------------------------------------------

# Exercise 6 --- for with an Array

Create:

``` powershell
$servers = 'Server01', 'Server02', 'Server03'
```

Run:

``` powershell
for ($i = 0; $i -lt $servers.Count; $i++) {
    "Index $i contains $($servers[$i])"
}
```

### Question

Why does the condition use:

``` powershell
$i -lt $servers.Count
```

instead of `-le`?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 7 --- while

Run:

``` powershell
$count = 1

while ($count -le 5) {
    "Count: $count"
    $count++
}
```

### Safety Question

What would happen if `$count++` were removed?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 8 --- do while

Run:

``` powershell
$count = 1

do {
    "Count: $count"
    $count++
} while ($count -le 3)
```

Now start `$count` at `10` but keep the condition `$count -le 3`.

### Question

How many times does the body run?

``` text
______________________
```

Why?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 9 --- do until

Run:

``` powershell
$count = 1

do {
    "Count: $count"
    $count++
} until ($count -gt 5)
```

Complete:

``` text
while continues while a condition is __________________.

until continues until a condition becomes ______________.
```

------------------------------------------------------------------------

# Exercise 10 --- break

Run:

``` powershell
foreach ($number in 1..10) {
    if ($number -eq 5) {
        break
    }

    $number
}
```

Which values display?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 11 --- continue

Run:

``` powershell
foreach ($number in 1..5) {
    if ($number -eq 3) {
        continue
    }

    $number
}
```

Which value is skipped?

``` text
______________________
```

------------------------------------------------------------------------

# Exercise 12 --- Filter Before Looping

Instead of looping through every service, collect running services
first:

``` powershell
$runningServices = Get-Service |
    Where-Object Status -eq 'Running'
```

Then:

``` powershell
foreach ($service in $runningServices) {
    $service.Name
}
```

Write the pattern:

``` text
__________ → __________ → __________ → __________
```

------------------------------------------------------------------------

# End-of-Lab Challenge --- Process an Asset List

Use the CSV created in Lab 11, or create a small asset collection
manually.

Your task:

> Display a message for every unassigned asset.

The output should resemble:

``` text
Unassigned asset: LT-1002
```

Requirements:

1.  Collect/import the asset records.
2.  Filter unassigned assets before the loop.
3.  Use `foreach`.
4.  Display the asset tag for each result.

``` powershell
# Your solution:
```

### Bonus

Use a `for` loop to display every asset along with its numeric array
index.

------------------------------------------------------------------------

# Knowledge Check

1.  Which loop is a good choice for an existing collection?

    A. `foreach`\
    B. `switch`\
    C. `if`\
    D. `catch`

2.  Which command processes objects directly in a pipeline?

    A. `ForEach-Object`\
    B. `Loop-Object`\
    C. `Repeat-Object`\
    D. `Where-Object`

3.  What does `break` do?

    A. Skips one iteration\
    B. Exits the loop\
    C. Restarts the loop\
    D. Deletes the collection

4.  What does `continue` do?

    A. Exits PowerShell\
    B. Skips the remainder of the current iteration\
    C. Ends every loop\
    D. Creates a new loop

5.  What should every condition-based loop have?

    A. A way for its stopping condition eventually to be reached\
    B. `Get-Service`\
    C. An administrator session\
    D. A CSV file

------------------------------------------------------------------------

# Lab Complete

Continue to:

[Lab 13 --- Conditions](lab-13-conditions.md)
