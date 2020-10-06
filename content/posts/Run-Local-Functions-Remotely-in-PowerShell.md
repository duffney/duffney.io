---
author:
  name: "Josh Duffney"
date: 2016-08-09
linktitle: Run Local Functions Remotely in PowerShell
type: post
type: posts
toc: true
title: Run Local Functions Remotely in PowerShell
aliases: 
- /RunLocalFunctionsRemotely
tags: ["PowerShell","functions","PowerShell Remoting"]
categories: ["how-to"]
---

Have you ever had functions loaded into your local PowerShell session and needed to run them on a remote system? 

The typical solution to this problem is to copy the code to the remote system and then load the functions on the remote system to use them. What if I told youit is possible to run functions you have stored on your local machine and execute them remotely with Invoke-Command? This post will teach
you how to do this.

# Execute Local Functions Remotely

To demonstrate how to do this, I'll use a famous Hell-World example. I have written an extremely simple function that writes
"Hello, World!" to the console. It is called MyFunction and has no parameters. I've loaded to into my local PowerShell session
and to run it remotely I will use Invoke-Command. Here is the trick, place a $ symbol out side the curly braces of the scriptblock.
Then put Function: inside the scriptblock before the name of the function as seen below.

```powershell
function MyFunction ()
{
    Write-Host 'Hello, World!'
}

Invoke-Command -ComputerName DC1 `
-ScriptBlock ${Function:MyFunction}
```


![MyFunction](/img/posts/Run-Local-Functions-Remotely-in-PowerShell/MyFunction.gif "MyFunction")


# Using Parameters

While the above example works, most functions aren't that simple. Most require several parameters and how do you pass those
parameters to the function running remotely? To illustrate, I've written another simple function called Get-NetConfig. Provided
an InterfaceIndex and AddressFamily parameter, it will gather information about the network settings of the system it runs on. 
In the below example I am passing the values for the InterfaceIndex and AddressFamily by using the -ArgumentList parameter of
Invoke-Command.

```powershell
function Get-NetConfig
{
    [CmdletBinding()]
    Param (
        $InterfaceIndex,
        $AddressFamily
    )

    $NodeName = $env:ComputerName
    $NetAdapter = Get-NetAdapter -InterfaceIndex $InterfaceIndex
    $IPconfig = Get-NetIPAddress -InterfaceIndex $InterfaceIndex -AddressFamily $AddressFamily
    $DNS = (Get-DnsClientServerAddress -InterfaceIndex $InterfaceIndex).ServerAddresses
    $obj = [PSCustomObject]@{NodeName=$NodeName;IPAddress=$IPconfig.IPAddress;MacAddress=$NetAdapter.MacAddress;DNS=$DNS}
    $obj
}

Invoke-Command -ComputerName DC1 `
-ScriptBlock ${Function:Get-NetConfig} `
-ArgumentList '5','IPV4' |
Select-Object NodeName,IPaddress,MacAddress,DNS
```


![Get-NetConfig](/img/posts/Run-Local-Functions-Remotely-in-PowerShell/Get-NetConfig.gif "Get-NetConfig")


# Changing Parameter Order

Often you do not specify the parameters in the order they are listed in the function. This is a problem,
because executing function remotely requires they be in the exact same order. This means that if I provided
AddressFamily first and IPAddress second in the above example it would error out. There are ways around this
of course. Method 1 would be to set default values for all the parameters, but that's not an ideal solution.
Method 2 is setting the position of the parameter to the order you want. In the below code I'm giving AddressFamily
the first position by specify 0 and assigning position 1, the second position to AddressFamily.


```powershell
function Get-NetConfig
{
    [CmdletBinding()]
    Param (
        [Parameter(Position = 1)] #<--- Second Param
        $InterfaceIndex,
        [Parameter(Position = 0)] #<-- First Param
        $AddressFamily
    )

    $NodeName = $env:ComputerName
    $NetAdapter = Get-NetAdapter -InterfaceIndex $InterfaceIndex
    $IPconfig = Get-NetIPAddress -InterfaceIndex $InterfaceIndex -AddressFamily $AddressFamily
    $DNS = (Get-DnsClientServerAddress -InterfaceIndex $InterfaceIndex).ServerAddresses
    $obj = [PSCustomObject]@{NodeName=$NodeName;IPAddress=$IPconfig.IPAddress;MacAddress=$NetAdapter.MacAddress;DNS=$DNS}
    $obj
}

Invoke-Command -ComputerName DC1 `
-ScriptBlock ${Function:Get-NetConfig} `
-ArgumentList 'IPV4','5' | #<---- Changed Arguement Order
Select-Object NodeName,IPaddress,MacAddress,DNS
```


![PositionalParams](/img/posts/Run-Local-Functions-Remotely-in-PowerShell/PositionalParams.gif "PositionalParams")

<br>

---

Learn how I broke the chains of imposter syndrome.

**Subscribe** to **the 4-hour engineer** email list.

<style> .gumroad-follow-form-embed { zoom: 1; } .gumroad-follow-form-embed:before, .gumroad-follow-form-embed:after { display: table; line-height: 0; content: ""; } .gumroad-follow-form-embed:after { clear: both; } .gumroad-follow-form-embed * { margin: 0; border: 0; padding: 0; outline: 0; box-sizing: border-box !important; float: left !important; } .gumroad-follow-form-embed input { border-radius: 4px; border-top-right-radius: 0; border-bottom-right-radius: 0; font-family: -apple-system, ".SFNSDisplay-Regular", "Helvetica Neue", Helvetica, Arial, sans-serif; font-size: 15px; line-height: 20px; background: #fff; border: 1px solid #ddd; border-right: 0; color: #aaa; padding: 10px; box-shadow: inset 0 1px 0 rgba(0, 0, 0, 0.02); background-position: top right; background-repeat: no-repeat; text-rendering: optimizeLegibility; font-smoothing: antialiased; -webkit-appearance: none; -moz-appearance: caret; width: 50% !important; height: 40px !important; } .gumroad-follow-form-embed button { border-radius: 4px; border-top-left-radius: 0; border-bottom-left-radius: 0; box-shadow: 0 1px 1px rgba(0, 0, 0, 0.12); -webkit-transition: all .05s ease-in-out; transition: all .05s ease-in-out; display: inline-block; padding: 11px 15px 12px; cursor: pointer; color: #fff; font-size: 15px; line-height: 100%; font-family: -apple-system, ".SFNSDisplay-Regular", "Helvetica Neue", Helvetica, Arial, sans-serif; background: #36a9ae; border: 1px solid #31989d; filter: "progid:DXImageTransform.Microsoft.gradient(startColorstr=#5ccfd4, endColorstr=#329ca1, GradientType=0)"; background: -webkit-linear-gradient(#5ccfd4, #329ca1); background: linear-gradient(to bottom, #5ccfd4, #329ca1); height: 40px !important; width: 35% !important; } </style> <form action="https://gumroad.com/follow_from_embed_form" class="form gumroad-follow-form-embed" method="post"> <input name="seller_id" type="hidden" value="7807279384399"> <input name="email" placeholder="Your email address" type="email"> <button data-custom-highlight-color="" type="submit">Subscribe</button> </form>
<br>