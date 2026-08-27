# Lab 18 --- PowerShell Remoting

## Lab Objective

This is an **environment-dependent lab**.

You will practice remoting only if you have an authorized second Windows
computer, VM, or lab environment already configured for PowerShell
remoting.

You will:

-   Distinguish local and remote execution.
-   Check WS-Management connectivity where applicable.
-   Use `Invoke-Command`.
-   Use `Enter-PSSession`.
-   Create and remove reusable sessions.
-   Pass local variables with `$using:`.
-   Run read-only checks against one or more authorized systems.
-   Handle unavailable remote computers.

------------------------------------------------------------------------

## Important Safety and Environment Note

Do **not** enable or reconfigure PowerShell remoting on a company system
simply to complete this lab.

Commands such as:

``` powershell
Enable-PSRemoting
```

change system configuration.

Use this lab only where remoting is already authorized and configured.

If you do not have a suitable environment, complete the **Review Track**
at the end instead.

------------------------------------------------------------------------

# Exercise 1 --- Local Baseline

Run locally:

``` powershell
hostname
Get-Date
Get-Service -Name Spooler
```

Record the local computer name:

``` text
________________________________________
```

------------------------------------------------------------------------

# Exercise 2 --- Define an Authorized Target

If you have a lab target:

``` powershell
$computerName = 'YOUR-LAB-COMPUTER'
```

Do not use an arbitrary company device.

------------------------------------------------------------------------

# Exercise 3 --- Test WS-Management

Where WS-Man remoting is applicable:

``` powershell
Test-WSMan -ComputerName $computerName
```

If it fails, record the error rather than changing security or firewall
settings.

``` text
Result:
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 4 --- Invoke-Command

Run a read-only remote command:

``` powershell
Invoke-Command -ComputerName $computerName -ScriptBlock {
    hostname
}
```

Then:

``` powershell
Invoke-Command -ComputerName $computerName -ScriptBlock {
    Get-Date
}
```

Compare local and remote output.

------------------------------------------------------------------------

# Exercise 5 --- Remote System Information

Use:

``` powershell
Invoke-Command -ComputerName $computerName -ScriptBlock {
    Get-CimInstance Win32_OperatingSystem |
        Select-Object Caption, Version, BuildNumber, LastBootUpTime
}
```

What computer actually executed `Get-CimInstance`?

``` text
________________________________________
```

------------------------------------------------------------------------

# Exercise 6 --- Interactive Session

If permitted:

``` powershell
Enter-PSSession -ComputerName $computerName
```

Inside the session:

``` powershell
hostname
Get-Location
Get-Service -Name Spooler
```

Exit:

``` powershell
Exit-PSSession
```

------------------------------------------------------------------------

# Exercise 7 --- Reusable PSSession

Create:

``` powershell
$session = New-PSSession -ComputerName $computerName
```

Inspect:

``` powershell
$session
```

Use it:

``` powershell
Invoke-Command -Session $session -ScriptBlock {
    hostname
}
```

Remove it:

``` powershell
Remove-PSSession $session
```

------------------------------------------------------------------------

# Exercise 8 --- Local Variables and \$using:

Create locally:

``` powershell
$serviceName = 'Spooler'
```

Use it remotely:

``` powershell
Invoke-Command -ComputerName $computerName -ScriptBlock {
    Get-Service -Name $using:serviceName
}
```

### Question

Why is `$using:` needed?

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Exercise 9 --- Multiple Authorized Targets

Only if you have multiple authorized lab machines:

``` powershell
$computers = 'LAB-PC1', 'LAB-PC2'
```

Run:

``` powershell
Invoke-Command -ComputerName $computers -ScriptBlock {
    [PSCustomObject]@{
        ComputerName = $env:COMPUTERNAME
        UserName     = $env:USERNAME
        Date         = Get-Date
    }
}
```

Observe how PowerShell identifies results from different systems.

------------------------------------------------------------------------

# Exercise 10 --- Handle a Failure

Use an intentionally invalid lab hostname:

``` powershell
$badComputer = 'DefinitelyNotARealComputer123'
```

Attempt a read-only remote command with error handling:

``` powershell
try {
    Invoke-Command -ComputerName $badComputer -ScriptBlock {
        hostname
    } -ErrorAction Stop
}
catch {
    Write-Warning $_.Exception.Message
}
```

This combines Lesson 16 with remoting.

------------------------------------------------------------------------

# End-of-Lab Challenge --- Remote Health Snapshot

Using one or more **authorized lab computers**, return:

``` text
Computer name
OS caption
OS version
Last boot time
Spooler status
```

Requirements:

-   Use `Invoke-Command`.
-   Return structured objects.
-   Do not make configuration changes.
-   Handle connection failure.
-   Display or export a clean report.

``` powershell
# Your solution:
```

------------------------------------------------------------------------

# Review Track --- No Remoting Environment Available

If you cannot perform the live exercises, answer these instead:

1.  What is the difference between `Invoke-Command` and
    `Enter-PSSession`?
2.  What does `New-PSSession` create?
3.  Why should `Remove-PSSession` be used when a reusable session is no
    longer needed?
4.  What problem does `$using:` solve?
5.  Name four things that can prevent remoting from working.
6.  Why should you not run `Enable-PSRemoting` on company equipment just
    to practice?
7.  Write, but do not execute, an `Invoke-Command` example that
    retrieves `Get-Date` from `Server01`.

------------------------------------------------------------------------

# Lab Complete

Continue to:

[Lab 19 --- Windows Administration](lab-19-windows-administration.md)
