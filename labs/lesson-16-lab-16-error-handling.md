# Lab 16 --- Error Handling

## Lab Objective

In this lab, you will make PowerShell scripts fail more safely and
explain what went wrong.

You will practice:

-   Inspecting errors as objects.
-   Understanding non-terminating and terminating errors.
-   Using `-ErrorAction`.
-   Building `try`, `catch`, and `finally` blocks.
-   Reading `Exception.Message`.
-   Validating prerequisites.
-   Using `throw` and `Write-Warning`.
-   Handling failures inside loops.
-   Improving the asset script from Lab 15.

------------------------------------------------------------------------

## Exercise 1 --- Generate and Inspect an Error

Run:

``` powershell
Get-Item "$HOME\DoesNotExist.txt"
```

Now inspect the newest error:

``` powershell
$Error[0]
$Error[0] | Get-Member
```

Record one useful property or member you discovered:

``` text
________________________________________
```

------------------------------------------------------------------------

## Exercise 2 --- Non-Terminating Errors

Run:

``` powershell
Get-Item "$HOME\DoesNotExist.txt"
'PowerShell continued.'
```

Did the second statement run?

``` text
Yes / No
```

This demonstrates why some cmdlet failures do not automatically stop a
script.

------------------------------------------------------------------------

## Exercise 3 --- ErrorAction Stop

Run:

``` powershell
try {
    Get-Item "$HOME\DoesNotExist.txt" -ErrorAction Stop
}
catch {
    'The file could not be retrieved.'
}
```

Now change the catch block to show:

``` powershell
$_.Exception.Message
```

``` powershell
# Your revised block:
```

------------------------------------------------------------------------

## Exercise 4 --- finally

Create a `try/catch/finally` block where:

-   `try` attempts to retrieve a missing file.
-   `catch` reports the error.
-   `finally` displays `Operation finished.`

``` powershell
# Your solution:
```

Run it again using a file that actually exists.

Does `finally` run in both cases?

``` text
Yes / No
```

------------------------------------------------------------------------

## Exercise 5 --- Validate Before Acting

Create a safe path:

``` powershell
$path = Join-Path $HOME 'PowerShell-Lab16'
```

Write logic that:

1.  Uses `Test-Path`.
2.  Displays a warning if the path does not exist.
3.  Creates nothing and changes nothing.

``` powershell
# Your solution:
```

Why is checking a known prerequisite often better than waiting for a
command to fail?

``` text
____________________________________________________
```

------------------------------------------------------------------------

## Exercise 6 --- throw

Create:

``` powershell
$requiredFile = Join-Path $HOME 'missing-assets.csv'
```

Then:

``` powershell
if (-not (Test-Path $requiredFile)) {
    throw "Required file does not exist: $requiredFile"
}
```

What happened?

``` text
____________________________________________________
```

When would intentionally stopping a script be safer than continuing?

``` text
____________________________________________________
```

------------------------------------------------------------------------

## Exercise 7 --- Warning vs. Error

Create:

``` powershell
$assets = @()
```

If there are no records, issue:

``` powershell
Write-Warning 'No asset records were found.'
```

Explain why a warning can be appropriate here:

``` text
____________________________________________________
```

------------------------------------------------------------------------

## Exercise 8 --- Errors Inside a Loop

Create:

``` powershell
$files = @(
    (Join-Path $HOME 'DoesNotExist1.txt')
    $PROFILE
    (Join-Path $HOME 'DoesNotExist2.txt')
)
```

Process each path with `foreach`.

Inside the loop:

-   Try `Get-Item -ErrorAction Stop`.
-   Catch failures.
-   Use `Write-Warning`.
-   Continue to the next file.

``` powershell
# Your solution:
```

------------------------------------------------------------------------

# End-of-Lab Project --- Harden the Asset Audit Script

Take the `asset-audit.ps1` script from Lab 15.

Improve it so that it:

1.  Validates the input path.
2.  Uses `try/catch` around `Import-Csv`.
3.  Uses `-ErrorAction Stop` where appropriate.
4.  Reports useful exception information.
5.  Warns if zero records are imported.
6.  Stops safely when the input data cannot be used.
7.  Uses `try/catch` around the final export.
8.  Never uses an empty `catch` block.
9.  Keeps `-Verbose` support.
10. Still produces the original report when everything succeeds.

Use this design pattern:

``` text
Input
  ↓
Validate
  ↓
Attempt
  ↓
Catch Failure
  ↓
Report
  ↓
Stop or Continue Intentionally
```

### Testing Checklist

``` text
[ ] Valid CSV succeeds
[ ] Missing CSV is handled
[ ] Invalid/unreadable input produces a useful message
[ ] Empty input is handled
[ ] Export failure is handled
[ ] Successful run still creates the report
```

------------------------------------------------------------------------

# Knowledge Check

1.  What commonly makes a non-terminating cmdlet error catchable?

    A. `-Verbose` B. `-ErrorAction Stop` C. `-Force` D. `-Confirm`

2.  Inside `catch`, what does `$_` represent?

    A. Current error B. Current file C. Previous pipeline D. PowerShell
    version

3.  What does `finally` do?

    A. Runs only after success B. Runs only after failure C. Runs
    whether the attempt succeeds or fails D. Hides errors

4.  When is `throw` useful?

    A. When continuing would be invalid or unsafe B. To format a
    table C. To import modules D. To sort data

------------------------------------------------------------------------

# Lab Complete

Continue to:

[Lab 17 --- Modules](lab-17-modules.md)
