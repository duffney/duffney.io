---
author:
  name: "Josh Duffney"
date: 2015-04-10
linktitle: Run CMD Commands within a PowerShell Script
type: post
type: posts
title: Run CMD Commands within a PowerShell Script
aliases: 
- /RunCMD-withinPowerShell
tags: ["PowerShell","CMD","Command Prompt"]
categories: ["how-to"]
---

**Applies to: Windows PowerShell 2.0+**

Sometimes when you enter commands into PowerShell they don't execute the same way as they would in the command prompt. I ran into this issue with an uninstall string for a security software called Cylance Protect. The uninstall string looks like this:

```powershell
msiexec /Lvx* c:\Temp\MsiUnInstall.log /x {2E64FC5C-9286-4A31-916B-0D8AE4B22954} /qn
```

When I executed it within the command prompt it ran as expected, however when executed in PowerShell it pulled up the msi info page. The way I resolved this was by using cmd C\ followed by my uninstall command. The below code demonstrates this. Long story short use cmd /C "Command" to run cmd commands inside a PowerShell script.

# Run CMD commands in PowerShell

```powershell
cmd /c "msiexec /Lvx* c:\Temp\MsiUnInstall.log /x {2E64FC5C-9286-4A31-916B-0D8AE4B22954} /qn"
```

## Filepath has spaces

```powershell
cmd /c "`"C:\Program Files\Sophos\Endpoint Defense\SEDcli.exe`" -TPoff $tamperpassword"
```

Contribution [Gabriel McColl](https://twitter.com/gabrielmccoll)

<br>

---

Learn how I broke the chains of imposter syndrome.

**Subscribe** to **the 4-hour engineer** email list.

<style> .gumroad-follow-form-embed { zoom: 1; } .gumroad-follow-form-embed:before, .gumroad-follow-form-embed:after { display: table; line-height: 0; content: ""; } .gumroad-follow-form-embed:after { clear: both; } .gumroad-follow-form-embed * { margin: 0; border: 0; padding: 0; outline: 0; box-sizing: border-box !important; float: left !important; } .gumroad-follow-form-embed input { border-radius: 4px; border-top-right-radius: 0; border-bottom-right-radius: 0; font-family: -apple-system, ".SFNSDisplay-Regular", "Helvetica Neue", Helvetica, Arial, sans-serif; font-size: 15px; line-height: 20px; background: #fff; border: 1px solid #ddd; border-right: 0; color: #aaa; padding: 10px; box-shadow: inset 0 1px 0 rgba(0, 0, 0, 0.02); background-position: top right; background-repeat: no-repeat; text-rendering: optimizeLegibility; font-smoothing: antialiased; -webkit-appearance: none; -moz-appearance: caret; width: 50% !important; height: 40px !important; } .gumroad-follow-form-embed button { border-radius: 4px; border-top-left-radius: 0; border-bottom-left-radius: 0; box-shadow: 0 1px 1px rgba(0, 0, 0, 0.12); -webkit-transition: all .05s ease-in-out; transition: all .05s ease-in-out; display: inline-block; padding: 11px 15px 12px; cursor: pointer; color: #fff; font-size: 15px; line-height: 100%; font-family: -apple-system, ".SFNSDisplay-Regular", "Helvetica Neue", Helvetica, Arial, sans-serif; background: #36a9ae; border: 1px solid #31989d; filter: "progid:DXImageTransform.Microsoft.gradient(startColorstr=#5ccfd4, endColorstr=#329ca1, GradientType=0)"; background: -webkit-linear-gradient(#5ccfd4, #329ca1); background: linear-gradient(to bottom, #5ccfd4, #329ca1); height: 40px !important; width: 35% !important; } </style> <form action="https://gumroad.com/follow_from_embed_form" class="form gumroad-follow-form-embed" method="post"> <input name="seller_id" type="hidden" value="7807279384399"> <input name="email" placeholder="Your email address" type="email"> <button data-custom-highlight-color="" type="submit">Subscribe</button> </form>
<br>