[lesson-16-error-handling.md](https://github.com/user-attachments/files/31518470/lesson-16-error-handling.md)

# Lesson 16 --- Error Handling

## Learning Objectives

By the end of this lesson, you will be able to:

-   Explain why error handling matters in PowerShell.
-   Distinguish between terminating and non-terminating errors.
-   Use `-ErrorAction` to control error behavior.
-   Use `try`, `catch`, and `finally`.
-   Inspect error information with `$_` and `$Error`.
-   Use `throw` to generate an error intentionally.
-   Use `Write-Warning` for conditions that are not failures.
-   Validate input and prerequisites before performing work.
-   Add useful error handling to scripts and functions.
-   Avoid hiding errors that should be investigated.

------------------------------------------------------------------------

## Why Error Handling Matters

PowerShell scripts interact with files, services, computers, users,
APIs, and many other systems.

Things can go wrong.

For example:

-   A file may not exist.
-   A computer may be offline.
-   Access may be denied.
-   A service may not exist.
-   A user may provide invalid input.
-   A command may fail unexpectedly.

Without error handling, a script may stop unexpectedly or continue with
incomplete data.

> **Key Idea:** Good error handling helps a script fail predictably,
> explain what happened, and avoid continuing when doing so would be
> unsafe.

------------------------------------------------------------------------

# Errors Are Objects

PowerShell errors contain structured information.

Run a command that fails:

``` powershell
Get-Item C:\DoesNotExist.txt
```

PowerShell displays an error, but the error also contains information
that can be inspected.

The automatic variable:

``` powershell
$Error
```

stores recent errors.

The newest error is normally:

``` powershell
$Error[0]
```

You can inspect it:

``` powershell
$Error[0] | Get-Member
```

This reinforces an important PowerShell concept:

``` text
Errors are structured information too.
```

------------------------------------------------------------------------

# Non-Terminating Errors

Many PowerShell cmdlets produce **non-terminating errors**.

This means PowerShell reports the problem but may continue processing.

Example:

``` powershell
Get-Item C:\DoesNotExist.txt
'PowerShell continued.'
```

Depending on the command and error, the second statement may still run.

This behavior is useful when processing many objects, but sometimes you
need a failure to stop the current operation.

------------------------------------------------------------------------

# Terminating Errors

A terminating error stops the current operation.

`try` and `catch` are designed to handle terminating errors.

For commands that normally produce non-terminating errors, you can often
use:

``` powershell
-ErrorAction Stop
```

to make the error catchable.

Example:

``` powershell
Get-Item C:\DoesNotExist.txt -ErrorAction Stop
```

------------------------------------------------------------------------

# The -ErrorAction Parameter

Many PowerShell cmdlets support the common parameter:

``` text
-ErrorAction
```

Common values include:

  Value                Purpose
  -------------------- -------------------------------------------------------
  `Continue`           Display the error and continue
  `Stop`               Treat the error as terminating
  `SilentlyContinue`   Suppress the displayed error and continue
  `Ignore`             Suppress and do not add the error to the error stream
  `Inquire`            Ask what to do
  `Suspend`            Primarily associated with older workflow scenarios

For beginner scripting, the most important are:

``` text
Continue
Stop
SilentlyContinue
```

------------------------------------------------------------------------

# Use SilentlyContinue Carefully

You may see:

``` powershell
Get-Process -Name notepad -ErrorAction SilentlyContinue
```

This can be useful when a missing result is expected.

However, do not automatically hide errors.

This:

``` powershell
-ErrorAction SilentlyContinue
```

can make troubleshooting harder if the error represents a real problem.

> **Best Practice:** Suppress an error only when you understand why it
> can safely be ignored.

------------------------------------------------------------------------

# try and catch

Use:

``` powershell
try {
    # Code that may fail
}
catch {
    # Code that runs when a terminating error occurs
}
```

Example:

``` powershell
try {
    Get-Item C:\DoesNotExist.txt -ErrorAction Stop
}
catch {
    'The file could not be retrieved.'
}
```

Because `-ErrorAction Stop` turns the cmdlet error into a terminating
error, `catch` can handle it.

------------------------------------------------------------------------

# Inspecting the Error in catch

Inside a `catch` block:

``` powershell
$_
```

represents the current error.

Example:

``` powershell
try {
    Get-Item C:\DoesNotExist.txt -ErrorAction Stop
}
catch {
    "Error: $($_.Exception.Message)"
}
```

This gives the user useful information about the failure.

------------------------------------------------------------------------

# Exception Information

A useful property is:

``` powershell
$_.Exception.Message
```

You can also inspect the error:

``` powershell
$_ | Get-Member
```

or:

``` powershell
$_.Exception | Get-Member
```

Do not assume an error is only display text. PowerShell provides
structured error information that scripts can use.

------------------------------------------------------------------------

# finally

A `finally` block runs whether the `try` block succeeds or fails.

Basic structure:

``` powershell
try {
    # Attempt work
}
catch {
    # Handle failure
}
finally {
    # Always run
}
```

Example:

``` powershell
try {
    'Starting operation.'
}
catch {
    'Operation failed.'
}
finally {
    'Operation finished.'
}
```

`finally` is useful for cleanup tasks that should happen regardless of
success or failure.

------------------------------------------------------------------------

# throw

Use:

``` powershell
throw
```

to intentionally generate a terminating error.

Example:

``` powershell
$path = 'C:\DoesNotExist'

if (-not (Test-Path $path)) {
    throw "Required path does not exist: $path"
}
```

This is useful when your script detects a situation that makes it unsafe
or impossible to continue.

------------------------------------------------------------------------

# Validate Before Acting

Not every problem needs `try` and `catch`.

Sometimes you can check prerequisites first.

Example:

``` powershell
$path = 'C:\Temp\assets.csv'

if (-not (Test-Path $path)) {
    Write-Warning "File not found: $path"
    return
}
```

Then continue:

``` powershell
$assets = Import-Csv -Path $path
```

A useful pattern is:

``` text
Validate → Attempt → Catch → Report
```

------------------------------------------------------------------------

# Write-Warning

Use:

``` powershell
Write-Warning
```

when something deserves attention but is not necessarily a terminating
failure.

Example:

``` powershell
if ($assets.Count -eq 0) {
    Write-Warning 'No asset records were found.'
}
```

Warnings are different from errors.

Use them when the script can reasonably continue.

------------------------------------------------------------------------

# Error Handling in Functions

Functions should communicate failures clearly.

Example:

``` powershell
function Import-AssetData {
    [CmdletBinding()]
    param (
        [Parameter(Mandatory)]
        [string]$Path
    )

    if (-not (Test-Path $Path)) {
        throw "Asset file not found: $Path"
    }

    try {
        Import-Csv -Path $Path -ErrorAction Stop
    }
    catch {
        throw "Unable to import asset data: $($_.Exception.Message)"
    }
}
```

The function checks a prerequisite and handles an import failure.

------------------------------------------------------------------------

# Error Handling in Loops

When processing many items, decide whether one failure should stop
everything.

Example:

``` powershell
$files = 'file1.txt', 'file2.txt', 'file3.txt'

foreach ($file in $files) {
    try {
        Get-Item $file -ErrorAction Stop
    }
    catch {
        Write-Warning "Could not retrieve $file"
        continue
    }
}
```

Here, one missing file does not prevent the remaining files from being
processed.

This is a design decision.

Sometimes the correct action is to continue.

Sometimes the correct action is to stop.

------------------------------------------------------------------------

# \$ErrorActionPreference

PowerShell has a preference variable:

``` powershell
$ErrorActionPreference
```

It controls the default behavior for many non-terminating errors.

You can view it:

``` powershell
$ErrorActionPreference
```

You may see:

``` text
Continue
```

Changing this affects more than one command, so use it deliberately.

For beginner scripts, specifying:

``` powershell
-ErrorAction Stop
```

on commands inside `try` blocks is often clearer.

------------------------------------------------------------------------

# Practical IT Example --- Importing an Asset File

``` powershell
param (
    [Parameter(Mandatory)]
    [string]$InputPath
)

if (-not (Test-Path $InputPath)) {
    Write-Warning "Input file does not exist: $InputPath"
    return
}

try {
    $assets = Import-Csv -Path $InputPath -ErrorAction Stop
}
catch {
    Write-Warning "Unable to import the asset file: $($_.Exception.Message)"
    return
}

if ($assets.Count -eq 0) {
    Write-Warning 'The asset file contains no records.'
    return
}

"Imported $($assets.Count) asset records."
```

This is much safer than assuming the file and data are always valid.

------------------------------------------------------------------------

# Do Not Use Empty catch Blocks

Avoid:

``` powershell
try {
    Get-Item $path -ErrorAction Stop
}
catch {
}
```

The failure disappears without explanation.

At minimum, record or communicate useful information:

``` powershell
catch {
    Write-Warning "Unable to retrieve $path: $($_.Exception.Message)"
}
```

------------------------------------------------------------------------

# Error Handling Is Part of Script Design

A useful script workflow is:

``` text
Input
  ↓
Validate
  ↓
Attempt Work
  ↓
Success?
 ┌───────┴───────┐
Yes              No
 ↓                ↓
Continue       Handle Error
                  ↓
             Stop or Continue
```

Ask:

``` text
What can fail?
Can I validate it first?
Should the script stop?
Can it safely continue?
What information will help troubleshoot the failure?
```

------------------------------------------------------------------------

# Key Takeaways

-   Errors contain structured information.
-   `$Error` stores recent errors.
-   Many cmdlet errors are non-terminating by default.
-   `-ErrorAction Stop` is commonly used with `try` and `catch`.
-   `try` contains code that may fail.
-   `catch` handles terminating errors.
-   `finally` runs whether the operation succeeds or fails.
-   `$_` inside `catch` represents the current error.
-   `$_.Exception.Message` provides useful error text.
-   `throw` intentionally generates a terminating error.
-   Validate known prerequisites before performing work.
-   `Write-Warning` is appropriate for non-fatal concerns.
-   Do not hide errors without a reason.
-   Decide intentionally whether a failure should stop processing or
    allow the script to continue.

------------------------------------------------------------------------

# Lab

Continue to:

[Lab 16 --- Error Handling](../labs/lab-16-error-handling.md)

In the lab, you will generate and inspect errors, use `-ErrorAction`,
build `try`/`catch`/`finally` blocks, validate files and input, use
`throw`, and add useful error handling to a small administrative script.

------------------------------------------------------------------------

## Additional Resources

-   [About Try Catch Finally --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_try_catch_finally?view=powershell-7.6)
-   [About Throw --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_throw?view=powershell-7.6)
-   [About Common Parameters --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_commonparameters?view=powershell-7.6)
-   [About Preference Variables --- Microsoft
    Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_preference_variables?view=powershell-7.6)
