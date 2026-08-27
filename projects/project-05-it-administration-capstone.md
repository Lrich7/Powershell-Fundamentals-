# Project 05 --- IT Administration Capstone

## Final Project

**Recommended after:** Lessons 20--22

This is the least guided project in the course.

You are expected to plan the solution, discover commands, read Help,
test safely, and document your decisions.

------------------------------------------------------------------------

# Choose One Project Track

Choose **one** of the following.

## Track A --- Active Directory Audit

Build a read-only AD DS reporting tool.

Possible requirements:

``` text
Enabled and disabled users
Users missing important fields
Computer inventory
Disabled computers
Group inventory
Known group membership
Department or OU reporting
```

Use only an authorized Active Directory environment.

------------------------------------------------------------------------

## Track B --- Microsoft 365 Audit

Build a read-only Microsoft 365 reporting tool.

Possible requirements:

``` text
Users
Groups
Account status
Shared mailboxes
Mailbox inventory
License SKU information
```

Use only an authorized Microsoft 365 tenant and least-privilege
permissions.

------------------------------------------------------------------------

## Track C --- IT Administration Toolkit

Combine several useful tools from earlier projects into a reusable
toolkit.

Possible components:

``` text
System information
Disk health
Service checks
File inventory
Asset reporting
Windows health reporting
```

You may package reusable functions into a PowerShell module.

------------------------------------------------------------------------

# Phase 1 --- Define the Problem

Create:

``` text
PROJECT-PLAN.md
```

Document:

``` text
Problem
Goal
Users / audience
Inputs
Outputs
Dependencies
Required permissions
Risks
Safety controls
Success criteria
```

Do this **before** building the final script.

------------------------------------------------------------------------

# Phase 2 --- Define Scope

Write what the project will do:

``` text
____________________________________________________
____________________________________________________
```

Write what it will **not** do:

``` text
____________________________________________________
____________________________________________________
```

For a first production-style version, prefer read-only reporting over
automatic changes.

------------------------------------------------------------------------

# Phase 3 --- Build the Project

Your project must include:

``` text
README.md
PROJECT-PLAN.md
main .ps1 script
output folder or defined output location
sample data where appropriate
```

Your script must demonstrate appropriate use of:

``` text
Parameters
Functions
Objects
Pipelines
Filtering / sorting
Conditions
Collections
Error handling
Modules where needed
Structured output
Logging
```

------------------------------------------------------------------------

# Phase 4 --- Safety

Your project must not contain:

``` text
Plain-text passwords
Client secrets
Access tokens
Private keys
Production credentials
```

For AD or Microsoft 365:

-   Verify the target environment.
-   Verify the signed-in account.
-   Use only approved permissions.
-   Keep the first version read-only.
-   Do not test destructive commands in production.

If you later add changes, design them separately with
preview/confirmation controls such as `-WhatIf` where supported.

------------------------------------------------------------------------

# Phase 5 --- Logging

Create a useful log that records events such as:

``` text
Script start
Target/environment
Major operation
Records processed
Warnings
Errors
Output created
Script completion
```

Do not log credentials or secrets.

------------------------------------------------------------------------

# Phase 6 --- Error Handling

Identify at least three realistic failure points.

Examples:

``` text
Required module missing
Authentication failure
Input file missing
AD query failure
Graph query failure
Exchange connection failure
Output path unavailable
Export failure
```

Document how your script responds to each.

------------------------------------------------------------------------

# Phase 7 --- Documentation

Your `README.md` should explain:

``` text
What the project does
Requirements
PowerShell version
Required modules
Required permissions
How to run it
Parameters
Examples
Output files
Known limitations
Safety notes
```

Include comment-based Help in the main script.

------------------------------------------------------------------------

# Phase 8 --- Testing

Create a test table.

At minimum test:

  Test                         Expected Result   Actual Result   Pass?
  ---------------------------- ----------------- --------------- -------
  Normal successful run                                          
  Missing/invalid dependency                                     
  Invalid input or target                                        
  Empty result set                                               
  Output/export test                                             

Add tests specific to your chosen track.

------------------------------------------------------------------------

# Phase 9 --- Final Review

Before declaring the project complete, answer:

### Does the script solve the original problem?

``` text
Yes / No
```

### Can another IT technician understand how to run it?

``` text
Yes / No
```

### Does it fail safely?

``` text
Yes / No
```

### Does it expose any credentials or secrets?

``` text
Yes / No
```

### Is the output useful enough to make an IT decision?

``` text
Yes / No
```

### Have you tested it against the smallest reasonable scope first?

``` text
Yes / No
```

------------------------------------------------------------------------

# Required Deliverables

``` text
[ ] PROJECT-PLAN.md
[ ] README.md
[ ] Main .ps1 script
[ ] At least 3 meaningful functions
[ ] Parameters
[ ] Input validation
[ ] Error handling
[ ] Logging
[ ] Structured objects
[ ] Pipeline usage
[ ] Filtering / sorting
[ ] Structured report output
[ ] Test results
[ ] Safety documentation
[ ] No embedded secrets
```

------------------------------------------------------------------------

# Optional Advanced Features

Choose only if they improve the project:

``` text
[ ] Custom PowerShell module
[ ] JSON configuration
[ ] HTML report
[ ] Multiple report formats
[ ] Remoting
[ ] Microsoft Graph
[ ] Exchange Online
[ ] Active Directory
[ ] SupportsShouldProcess
[ ] -WhatIf / -Confirm
[ ] Pester tests
```

------------------------------------------------------------------------

# Final Presentation

Be able to explain your project without reading the code line by line.

Answer:

1.  What problem does it solve?
2.  Why did you automate this task?
3.  What permissions does it need?
4.  What are the biggest risks?
5.  How did you reduce those risks?
6.  How does the script handle failure?
7.  What output does it create?
8.  What did testing uncover?
9.  What would you add in version 2?

------------------------------------------------------------------------

# Course Completion Reflection

## A PowerShell skill I can now use confidently

``` text
____________________________________________________
```

## A PowerShell skill I need more practice with

``` text
____________________________________________________
```

## A real IT task I could now automate or report on

``` text
____________________________________________________
```

## The first thing I will verify before running automation in production

``` text
____________________________________________________
```

------------------------------------------------------------------------

# Capstone Complete

You have progressed from discovering individual PowerShell commands to
planning, building, testing, and documenting an administrative
automation project.

The goal is not to memorize PowerShell.

The goal is to know how to:

``` text
Discover
Understand
Inspect
Build
Test
Troubleshoot
Automate
Verify
Document
```

Those skills transfer to new PowerShell commands, modules, services, and
IT environments long after this course is complete.
