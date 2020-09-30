---
author:
  name: "Josh Duffney"
date: 2016-09-23
linktitle: Using Regex in PowerShell
type: post
type: posts
toc: true
title: Using Regex in PowerShell
aliases: 
- /APracticalGuideforUsingRegexinPowerShell
tags: ["PowerShell","regex","regular expression"]
categories: ["how-to"]
---

# Introduction

In this blog post you'll learn severals ways to use regular expression from within PowerShell. You've most likely used some of these
techniques before. Such as the *-match* operator or the *select-string* cmdlet, but probably weren't aware you were using regular expression.
This post will not teach you how to craft complex regular expressions. Instead it focuses on how to use them in PowerShell to find matches, replace
text, and to split on matches.

# -match Operator

The -match operator matches a string with regular expression. It returns a true or false statement indicating whether or not a match was found. Then it
stores all the matches found in a variable called $matches. A simple example of this is below, I'm matching the word Administrator from a distinguished name
from Active Directory. This example might not seem like it's using regular expression because it's using only literal characters *Administrator*, but it is.

```PowerShell
'CN=Administrator,CN=Users,DC=wef,DC=com' -match 'Administrator'
```

![matches](/img/posts/Practical-Guide-for-Using-Regex-in-PowerShell/matches.gif "matches")


# -match Operator with metacharacters
Let's make it a little more regex looking by replacing the literal characters *Administrator* with some regex metacharacters and a subexpression. I'll use the regex
expression *CN=(\w+)*. The C and N are still literal characters matching a capital C and a capital N, but \w is a metacharacters that matches any word character. The + sign is
another metacharacters that means to match one or more times. The parenthesis are used to capture the match found by \w+, in this example Administrator. The benefit
of using metacharacters is it makes the expression more dynamic just like parameters do for functions.

```powershell
'CN=Administrator,CN=Users,DC=wef,DC=com' -match 'CN=(\w+)'
```

![matchesmetachars](/img/posts/Practical-Guide-for-Using-Regex-in-PowerShell/matchesmetachars.gif "matchesmetachars")

# -notmatch and Where-Object
The -match operator has a few different versions you should be aware of. By default PowerShell is case-insensitive so there is a case-sensitive version of the -match
operator -cmatch. There are opposites of both of these operators -notmatch and -cnotmatch. I won't cover all of these variants, but it's worth taking a look at the
-notmatch operator. Sometimes it's easier to say what you do not want than what you do want. The below example demonstrates this by getting a list of services where the name 
of the service does not match a digit. 

```powershell
Get-Service | where Name -NotMatch '\d'
```

![notmatch](/img/posts/Practical-Guide-for-Using-Regex-in-PowerShell/notmatch.png "notmatch")

# -replace Operator

Matching text is always the first part of using regular expression and it's useful, but most of the time you want to do something with that match. Using the same example
as above I can use the -replace operator instead of -match to replace the entire string with what I wanted to match. The expression looks like this *'CN=(\w+).\*','$1'*.
I'm using a subexpression () to capture the match 'Administrator'. Matches found within subexpression are stored in variables. In PowerShell they start at the number 0 and increment
by one for each subexpression used. That is why you see the *'$1'* in the expression. If you're not familiar with -replace the syntax is *'this' -replace 'this','that'* which replace
the word this with that. Relating that to our expression *'CN=(\w+).\** matches the entire distinguished name and *'$1'* replaces it with the match found in the first subexpression. In
this case 'Administrator'.

```powershell
'CN=Administrator,CN=Users,DC=wef,DC=com' -replace 'CN=(\w+).*','$1'
```

![replace](/img/posts/Practical-Guide-for-Using-Regex-in-PowerShell/replace.png "replace")

# -split Operator

Sometimes you don't want to replace any text from a match but rather simply split it and use both parts to accomplish a given task. That's where the -split operator can come in handy.
I'll use an example of splitting a users's UPN into two sections the first will contain the user name and the second will contain the domain name. I'll use that information to query the user
with Get-ADUser and specify the server name.

```powershell
$split = 'ejohnson@wef.com' -split '@'

Get-ADUser -Identify $split[0] -server $split[1]
```

# Select-String cmdlet

Select-String searches for text and text patterns in either a string or a set of files. I use it mostly for looking through scripts I've written to identify which scripts
contain keywords I'm looking for. For example I could use it to search a folder for all scripts that are using Active Directory cmdlets with the verbs Get and Set. To do that, I 
could craft a simple expression that looks for a few verbs Get and Set followed by -AD then a series of words. That expression might look something like this *[GS]et-AD\w+*.
*[GS]* is a character class, but simply put it means allow G or S as the first character.

```powershell
Select-String -Pattern '[GS]et-AD\w+' -Path *.ps1 -List
```

![selectstring](/img/posts/Practical-Guide-for-Using-Regex-in-PowerShell/selectstring.png "selectstring")

# Switch Statements

Another way to use regex in PowerShell is within a switch statement. In the below example I'm using regular expression to validate user names in Active Directory. There are
three statements one validates service account names, another validates admin accounts and the last one identifies invalid characters. You could perhaps take this logic and 
add it to some PowerShell automation that creates user accounts. Each of these switch statements could execute different processes for creating Active Directory accounts. 

```powershell
$UserNames = 'svc_jenkins','duffney.admin','<jduffney>'

foreach ($UserName in $UserNames) {
    switch -regex ($UserName) {
        'svc_[^"/\\\[\]:;|=,+\*\?<>*]+' {"service account [$UserName]"}
        '[^"/\\\[\]:;|=,+\*\?<>*]+\.admin' {"admin account [$UserName]"}
        '["/\\\[\]:;|=,+\*\?<>*]+' {"[$UserName] contains invalid characters"}
    }   
}
```

# Regex Object Matching

So far I've been using operators, cmdlets and statements that handle regex in an integrated manner. This means it has been hiding much of the regular expression engine from us.
I mention this because the regex object, which I'll cover next is not integrated. It is referred to as procedural and object-oriented. If you have experience with other programing
languages such as java or C# this will look very familiar, if not don't worry it's really not that difficult.

To create the regex object, you create a variable and type cast it to *[regex]*. The variable contains the regular expression you want to use. I will be using the regex object to
extract a domain name from an Active Directory distinguished name. Below has a distinguished name stored in a variable called $DN then I create the regex object and specify the
regular expression to use. Breaking down the regular expression, *(?:.\*?)* matches the entire distinguished name. *.* matches any character and the *** is a quantifier that matches zero or more times.
It's also option because of the *?*. What is interesting about the first part of this expression is the *?:* it is a non-capturing subexpression. You'll see what that means in a minute.
*DC=(.\*)* is the second half of the expression and matches the literal characters *DC=* then matches any character zero or more times *.\**. *(.\*)* is within a capturing subexpression which I'll use
later to obtain the domain name.

```powershell
$DN = 'CN=Administrator,CN=Users,DC=wef,DC=com'
[regex]$rx='(?:.*?)DC=(.*)'
```

If you ran the above code, you noticed that it didn't actually do anything nor did it populate the $matches variable. This is because, it's an object and to interact with objects you use their methods.
To take a look at all the methods pipe $rx to get-member *$rx | Get-Member*. The first method I'll demonstrate is the match method. The syntax is *$rx* followed by *.match()* inside *()*
is the thing you want to match against. In this case it's the string variable *$dn* that contains the distinguished name. Take a look at the below screenshot and notice that there are only 2 captures, $0 and $1.
This is because I used non-capturing parenthesis for the first subexpression. You can also quickly identify the domain name for this distinguished name by looking at the contents of $1 *wef,DC=com*. However, I
highly doubt management would accept it in this format so let's move on to the replace method and clean it up.  

```powershell
$rx.Match($DN)
```

![regexmatch](/img/posts/Practical-Guide-for-Using-Regex-in-PowerShell/regexmatch.png "regexmatch")

# Regex Object Replace

At this point, I've successfully matched the last half of the distinguished name which contains the domain name. However, that is stored within a capture group $1. It also isn't formatted correctly. It should be wef.com, not
wef,DC=com. So I really have two problems to solve. First, replace the entire distinguished name with $1. Secondly, I need to replace *,DC=* with *.* to make wef.com. To accomplish this, I'm using both the regex replace method
and the -replace operator. Below starts by showing the output of just the replace method then combines it with the -replace operator to clean up the formating.

```powershell
$rx.Replace($DN,'$1')

$rx.Replace($DN,'$1') -replace ',DC=','.'
```

![regexreplace](/img/posts/Practical-Guide-for-Using-Regex-in-PowerShell/regexreplace.png "regexreplace")

# Regex Object Split

The regex object also has a split method that can be used to, well split text where a regular expression matches. Previously, I've been creating regex objects and storing them in variables.
You don't have to do that if you are not going to reuse the expression. Below is an example that takes a URL and split off the HTTP or HTTPS prefix, leaving just the server name and path.

```powershell
[Regex]::Split('http://duffney.io/CreateJNLPScheduledTasksWithPS','^https?://')
```

![regexsplit](/img/posts/Practical-Guide-for-Using-Regex-in-PowerShell/regexsplit.png "regexsplit")

# Sources

[Ultimate PowerShell Prompt Customization and Git Setup Guide](https://hodgkins.io/ultimate-powershell-prompt-and-git-setup)

[Mastering Regular Expressions](https://www.amazon.com/Mastering-Regular-Expressions-Jeffrey-Friedl/dp/0596528124)

[PowerShell in Depth Chapter 13](https://www.amazon.com/PowerShell-Depth-Don-Jones/dp/1617292184/ref=sr_1_1?s=books&ie=UTF8&qid=1474652757&sr=1-1&keywords=powershell+in+depth)

## Pluralsight Course

If you want to learn more about regular expression, check out my Pluralsight course titled [Introduction to Regular Expression (Regex)](https://app.pluralsight.com/library/courses/regular-expression-introduction/table-of-contents).

<br>

---

<br>

<div align="center">
<a href="https://twitter.com/joshduffney">Follow me on Twitter</a>

<br>
<br>

