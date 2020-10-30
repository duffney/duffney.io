---
author:
  name: "Josh Duffney"
date: 2016-08-16
linktitle: Splatting Parameters Inside Advanced Functions
toc: true
type: post
type: posts
title: Splatting Parameters Inside Advanced Functions
tags: ["PowerShell","Splat"]
categories: ["how-to"]
---

In Windows PowerShell terms, splatting is a way of bundling parameters to send to a command. 
[Don Jones 2011](https://technet.microsoft.com/en-us/magazine/gg675931.aspx) Splatting is most often used when 
providing parameters and their values to a cmdlet in the form of a hashtable. The main benefit of splatting
is that it makes the code more aesthetic. They make it cleaner and easier to read. In this post, you'll learn how to splat
parameters inside advanced functions. 


## Advanced Functions without Splatting

To demonstrate the advantages of using splatting in advanced functions, I've written a simple advanced function that gathers 
all the members of an Active Directory group and then gets additional properties for those members. The function accepts
two mandatory parameters. The first parameters Identity is the name of the group that will be queried. The second parameters
properties is used to specify which Active Directory user properties you'd like to gather. While this does work, there is a problem.
What if I don't want any additional properties? At this point it's to bad so sad, it's mandatory.

```powershell
function Get-ADGroupMemberProperties {
[CmdletBinding()]
param(
    [Parameter(Mandatory=$true)]
    [string]$Identity,
    [Parameter(Mandatory=$true)]
    $Properties
)

begin {
    $members = (Get-ADGroupMember -Identity $Identity).ObjectGUID
}

process {
    foreach ($member in $members) {
        Get-ADUser -Identity $member -Properties $Properties
    }
}

end {
}
}
```

## IF Else Everywhere

One possible solution and the most common is the use of if and else statements. Below shows the process block
with some added if else statements that allow me to use the function without using the Properties parameter. I would
also need to change [Parameter(Mandatory=$true)] to [Parameter(Mandatory=$false)] for the Properties parameter. 
Does this work? Yes, but what happens when I start adding more parameter? That's right more and more if else statements.

```powershell
process {
    foreach ($member in $members) {
        
        if ($PSBoundParameters.ContainsKey('Properties')){
            Get-ADUser -Identity $member -Properties $Properties
        } else {
            Get-ADUser -Identity $member
        }
    }
}   
```

## Splatting Parameters in Advanced Functions

Now, that you've seen what it looks like to use if else statements let's take a look at how I can improve the code
by using splatting. In the below snippet of code I have the proces block again. I've removed the if else and replaced
it with a hashtable called $params that will be splatted to the Get-ADUser cmdlet inside the foreach loop. I'm still using
a single if statement to determine if the $Properties parameter was used or not and if it was I'm adding it to the $params hashtable.

```powershell
process {
    foreach ($member in $members) {
        
        $params = @{
            'Identity' = $member
        }

        if ($PSBoundParameters.ContainsKey('Properties')){
            $params.Add('Properties',$Properties)
        }

        Get-ADUser @params
    }
}
```

## Complete Code

{% gist 444c3dbfadf915326f074fa1e274f9a9 %}