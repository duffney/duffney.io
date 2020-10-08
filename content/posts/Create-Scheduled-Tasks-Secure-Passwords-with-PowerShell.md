---
author:
  name: "Josh Duffney"
date: 2016-06-22
linktitle: Create Scheduled Tasks Secure Passwords with PowerShell
type: post
type: posts
toc: true
title: Create Scheduled Tasks Secure Passwords with PowerShell
aliases: 
- /Create-ScheduledTasks-SecurePassword
tags: ["PowerShell","ScheduledTask","Passwords"]
categories: ["how-to"]
---

**Applies to: Windows PowerShell 3.0,4.0,5.0**

While writing the DSC configuration for some Jenkins slaves, I discovered the Register-ScheduledTask cmdlet only accepts string variables. 
This forced me to store my service account password as clear text, which made me cringe. I knew there had to be a better way, even if the cmdlet
did not allow a credential object to be passed to it. In this post you'll learn how to extract the password from a PS Credential object.
I'll store those properties in variables and then provide them to the Register-ScheduledTask cmdlet.

# Create the Action

The below section of code creates the trigger for the ScheduledTask, I'll use this later when I create the ScheduledTask. It simply calls the script named
PowerShellscript.ps1 from C:\script and execute it. 

```powershell
$Action = New-ScheduledTaskAction -Execute 'C:\Windows\System32\WindowsPowerShellv1.0\powershell.exe' -Argument "-NonInteractive -NoLogo `
-NoProfile -File 'C:\scripts\PowerShellscript.ps1'" -WorkingDirectory "C:\scripts"
```


# Define the Trigger

Next I'm defining the trigger and storing it into a variable called $Trigger. I'm having it run upon startup, but delaying it 5 minutes
after the machine is online.

```powershell
$Trigger = New-ScheduledTaskTrigger -RandomDelay (New-TimeSpan -Minutes 5) -AtStartup
```

# Configure the Settings

This was the trickiest part, trying to determine which parameters matched the GUI settings. Below I'm telling the task to; not stop when the idle ends, 
setting the restart interval to 1 minute, setting the reset count to 10 tries, starting when available, and removing the idle timeout option. $Settings.ExecutionTimeLimit = "PT0S"
is removing the idle timeout, that took a bit to figure out. 

```powershell
$Settings = New-ScheduledTaskSettingsSet -DontStopOnIdleEnd -RestartInterval (New-TimeSpan -Minutes 1) -RestartCount 10 -StartWhenAvailable
$Settings.ExecutionTimeLimit = "PT0S"
```

# New PS Credential Object

Now I have to create a PS Credential object that I can extract out the password from. This was the fun part as I hadn't dealt with this to much before.
Mainly because most cmdlets allow for encrypted credentials to be passed to them... at any rate. Lines 1-3 create the PS credential object and line 4 creates
a variable with the password in it. It's a string, but at least it's not stored in the script. Perhaps I should of titled the post secureish? If you don't like the
read-host prompt, you can turn this code into an advanced function and require a credential parameter. Once I finish mine, I'll be sure to link it here.

```powershell
$SecurePassword = $password = Read-Host -AsSecureString
$UserName = "svc_account"
$Credentials = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
$Password = $Credentials.GetNetworkCredential().Password 
```

# Register the ScheduledTask

Lastly I have to create the ScheduledTask and then register it. If you just create it, it will only be stored in memory. Registering it is what adds it to the task
scheduler. 

```powershell
$Task = New-ScheduledTask -Action $Action -Trigger $Trigger -Settings $Settings
$Task | Register-ScheduledTask -TaskName 'ScheduledTaskName' -User "svc_account" -Password $Password
```

# Complete script


```powershell
$Action = New-ScheduledTaskAction -Execute 'C:\Windows\System32\WindowsPowerShellv1.0\powershell.exe' -Argument "-NonInteractive -NoLogo `
-NoProfile -File 'C:\scripts\PowerShellscript.ps1'" -WorkingDirectory "C:\scripts"

$Trigger = New-ScheduledTaskTrigger -RandomDelay (New-TimeSpan -Minutes 5) -AtStartup

$Settings = New-ScheduledTaskSettingsSet -DontStopOnIdleEnd -RestartInterval (New-TimeSpan -Minutes 1) -RestartCount 10 -StartWhenAvailable
$Settings.ExecutionTimeLimit = "PT0S"

$SecurePassword = $password = Read-Host -AsSecureString
$UserName = "svc_account"
$Credentials = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
$Password = $Credentials.GetNetworkCredential().Password 

$Task = New-ScheduledTask -Action $Action -Trigger $Trigger -Settings $Settings
$Task | Register-ScheduledTask -TaskName 'ScheduledTaskName' -User "svc_account" -Password $Password
```


# Sources
[How To Build Scheduled Tasks In PowerShell](http://www.tomsitpro.com/articles/powershell-build-scheduled-tasks,2-832.html)


[Working with Passwords, Secure Strings and Credentials in Windows PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/4546.working-with-passwords-secure-strings-and-credentials-in-windows-powershell.aspx)


[Scheduled Task via Powershell](http://powershell.com/cs/forums/t/20758.aspx)

<br>

---

7 Lessons you need to unlock your learning. 

**Subscribe** to **the 4-hour engineer** email list.

<style> .gumroad-follow-form-embed { zoom: 1; } .gumroad-follow-form-embed:before, .gumroad-follow-form-embed:after { display: table; line-height: 0; content: ""; } .gumroad-follow-form-embed:after { clear: both; } .gumroad-follow-form-embed * { margin: 0; border: 0; padding: 0; outline: 0; box-sizing: border-box !important; float: left !important; } .gumroad-follow-form-embed input { border-radius: 4px; border-top-right-radius: 0; border-bottom-right-radius: 0; font-family: -apple-system, ".SFNSDisplay-Regular", "Helvetica Neue", Helvetica, Arial, sans-serif; font-size: 15px; line-height: 20px; background: #fff; border: 1px solid #ddd; border-right: 0; color: #aaa; padding: 10px; box-shadow: inset 0 1px 0 rgba(0, 0, 0, 0.02); background-position: top right; background-repeat: no-repeat; text-rendering: optimizeLegibility; font-smoothing: antialiased; -webkit-appearance: none; -moz-appearance: caret; width: 50% !important; height: 40px !important; } .gumroad-follow-form-embed button { border-radius: 4px; border-top-left-radius: 0; border-bottom-left-radius: 0; box-shadow: 0 1px 1px rgba(0, 0, 0, 0.12); -webkit-transition: all .05s ease-in-out; transition: all .05s ease-in-out; display: inline-block; padding: 11px 15px 12px; cursor: pointer; color: #fff; font-size: 15px; line-height: 100%; font-family: -apple-system, ".SFNSDisplay-Regular", "Helvetica Neue", Helvetica, Arial, sans-serif; background: #36a9ae; border: 1px solid #31989d; filter: "progid:DXImageTransform.Microsoft.gradient(startColorstr=#5ccfd4, endColorstr=#329ca1, GradientType=0)"; background: -webkit-linear-gradient(#5ccfd4, #329ca1); background: linear-gradient(to bottom, #5ccfd4, #329ca1); height: 40px !important; width: 35% !important; } </style> <form action="https://gumroad.com/follow_from_embed_form" class="form gumroad-follow-form-embed" method="post"> <input name="seller_id" type="hidden" value="7807279384399"> <input name="email" placeholder="Your email address" type="email"> <button data-custom-highlight-color="" type="submit">Subscribe</button> </form>
<br>
