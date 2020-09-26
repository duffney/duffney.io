---
author:
  name: "Josh Duffney"
date: 2020-03-11
lastmod: 2020-03-11
linktitle: Using PowerShell Markdown Cmdlets
toc: true
type: post
type: posts
title: Using PowerShell Markdown Cmdlets
tags: ["powershell","markdown"]
categories: ["how-to"]
---

The vast majority of the technical documentation written today is written in Markdown. From Jane's dev blog to Microsoft's PowerShell documentation, markdown is behind it. Markdown is a light weight markup language with plain-text-formatting syntax. Markup languages were designed to be easy to write using a generic text editor and easy to read in its raw from without rendering. Markdown's success is largely in part because it does this very well. It is easy to read without being rendered. It also does not require a bunch of opening and closing tags. Which are difficult to read and time consuming to type. Markdown has an extensive list of features that allow you to; style font, define headings, create tables, create hyperlinks, define code snippets, and much more. Recently, a few new Markdown cmdlets were introduced to PowerShell. These cmdlets allow you to work with Markdown from the terminal. They allow you to render the markdown as HTML or to view it within a terminal window using AsVT100 Encoding. In this blog post, you'll learn how to use these cmdlets to render and output Markdown using PowerShell.

# Converting from Markdown to HTML or AsTV100 Encoded Strings

The `ConvertFrom-Markdown` cmdlet converts the contents of a string or file to a MarkdownInfo object. Once converted (rendered) into a MarkdownInfo object it can be displayed in two ways, as html or as a VT100-encoded string. If you choose to convert the Markdown to html, you can either view the html code within the terminal to confirm it rendered properly. Choosing to convert it to a VT100-encoded string specifies if the output should be encoded as a string with VT100 escape codes. ANSI VT100 escape sequences are then used to provide special syntax highlighting within the terminal. The value this cmdlet provides is the ability to generate rendered output. For example when you provide a URL within a Markdown file ConvertFrom-Markdown will add the necessary html syntax to make it a link within html. You do not have to type a paragraph tag or an anchor tag around the url. The ConvertFrom-Markdown cmdlet will do that for you. CovertFrom-Markdown uses markdig which is a Markdown processor for .Net. You can view many of the supported features and examples on the [markdig](https://github.com/lunet-io/markdig) GitHub page. Below are just a few of the things you can do with the ConvertFrom-Markdown cmdlet to render Markdown to html or to a VT100-encoded string.

## Auto Generate Links

```powershell
'http://duffney.io/' | ConvertFrom-Markdown
```

![convertfrom-markdown](/img/posts/using-powershell-markdown-cmdlets/convertfrom-markdown.png "convertfrom-markdown")

## Creating a Task List

```powershell
$taskList = @"
- [ ] Item1
- [x] Item2
- [ ] Item3
- Item4
"@
$taskList | ConvertFrom-Markdown
```

![convertfrom-markdownlist](/img/posts/using-powershell-markdown-cmdlets/convertfrom-markdownlist.png "convertfrom-markdownlist")

## Emphasis

```powershell
'**#PS7** >_ ~~Soon~~ ^Now!^ ' | ConvertFrom-Markdown  -AsVT100EncodedString
```

![emphasis](/img/posts/using-powershell-markdown-cmdlets/emphasis.png "emphasis")

_Notice that some of the styling does not display when converted to a VT100 encoded string. It will however, display properly when rendered in html and displayed in a web browser._

_Learn more about the Markdown [emphasis](https://github.com/lunet-io/markdig/blob/master/src/Markdig.Tests/Specs/EmphasisExtraSpecs.md) with markdig._

# Displaying Markdown with PowerShell

Converting Markdown to html or to VT100-encoded strings is neat, but it doesn't help give you an idea of what the rendered markdown will look like. That is where the `Show-Markdown` cmdlet comes in. This cmdlet allows you to view the MarkdownInfo object through either a web browser or within the terminal using ANSI escape sequences to provide color coding and other visual aids.

## Show in a Web Browser

The Show-Markdown cmdlet has a `-UseBrowser` parameter that will display the html output from ConvertFrom-Markdown in your web browser. This gives you the ability to see what the end result of the markdown will look like. Using a Markdown definition list as an example, you can create the markdown using a here-string in PowerShell and store it in a variable. You then pipe that stored variable to the ConvertFrom-Markdown cmdlet which will convert the Markdown to html. After the Markdown is converted to html you pipe that to the Show-Markdown cmdlet with the -UseBrowser parameter. The end result will be the Markdown within the here string displayed as an html page in your browser!

```powershell
$defList = @"
PowerShell
:   PowerShell is a task-based command-line shell and scripting language built on .NET. PowerShell helps system administrators and power-users
     rapidly automate tasks that manage operating systems (Linux, macOS, and Windows) and processes.
    > Supported and experimental platforms:

    - Windows
    - Linux
    - macOS
    - ARM (including Windows 10 ARM32/ARM64 and Raspbian)

    ```powershell
    Test
    ```

    [Getting Started with Windows PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/getting-started/getting-started-with-windows-powershell?view=powershell-7)
:   [Sample scripts for system administration](https://docs.microsoft.com/en-us/powershell/scripting/samples/sample-scripts-for-administration?view=powershell-7)
"@
$defList | ConvertFrom-Markdown | Show-Markdown -UseBrowser
```

## Show Markdown within the Terminal

For those of us who prefer the command line over graphical interfaces there is also an option for you. The Show-Markdown cmdlet also supports outputting Markdown in the terminal. It does this by using ANSI escape sequences to add text style and coloring to the Markdown being displayed. You can get the current ANSI escape sequences used to modify the display with the `Get-MarkdownOption` cmdlet. You'll notice different ANSI escape sequences are used for all six different header options. In addition to the headers; code, links, images, bold and italics also have unique sequences. The numbers represented in the sequence are arguments that are pasted to the ANSI function `m`.

![Get-MarkdownOption](/img/posts/using-powershell-markdown-cmdlets/Get-MarkdownOption.png "Get-MarkdownOption")

Header2 for example has a sequence of `[4;93m`. The number 4 indicates that the text will be underlined. Following the 4 and after a semi colon is the number 93, which represents an orange foreground color. The function m that is after 93 is fed those arguments resulting with all Header2 being displayed in an orange foreground and underlined.

```powershell
'## Header2' | Show-Markdown
```

![Header2](/img/posts/using-powershell-markdown-cmdlets/Header2.png "Header2")

## Changing the Markdown Options

You have the ability to change the escape sequences being used to style the different properties of the Markdown output. You can do that by directly using the `Set-MarkdownOption` cmdlet or by storing the current settings in a variable and then passing that PSMarkdownOptionInfo object to the Set-MarkdownOption cmdlet. Using some sample Markdown you can get a better idea of what it will look like displayed in the terminal. When using a dark themed terminal the Code highlight is washed out and difficult to see. Also italics isn't italic. Both of these can be changed using the Set-MarkdownOption cmdlet to change the escape sequence associated with that property.

The current Code property uses the ANSI escape sequence `[107;95m`, 107 is modifying the background to a bright white color and 95 is changing the foreground color to a light purple color. These numbers are within a range defined in the SGR (Select Graphic Rendition) that set display attributes. The range for bright background color is 100-107 and the range for bright foreground color is 90-97. Using some PowerShell and an ANSI escape sequence you can output all the different combinations and pick one that sticks out to you.

```powershell
$sample = @"
# Heading1

## Heading2

### Heading3

``Get-Help Show-Markdown -Online``

[Show-Markdown PowerShell Docs](https://docs.microsoft.com/en-us/powershell/module/Microsoft.PowerShell.Utility/Show-Markdown?view=powershell-7)

**Bold**

*Italics*
"@
$sample | Show-Markdown
```

![sampleMarkdown](/img/posts/using-powershell-markdown-cmdlets/sampleMarkdown.png "sampleMarkdown")

```powershell
# Display all bright foreground and background color combinations
$fgColors = '90','91','92','93','94','95','96','97'
$bgColors = '100','101','102','103','104','105','106','107'

foreach ($fgColor in $fgColors)
{
    foreach ($bgColor in $bgColors){
        Write-Output "`e[$($fgColor);$($bgColor)m[$bgcolor;$fgcolor`m  Get-Help Show-Markdown -Online`e[0m"
    }
}
```

![ansiBrightColors](/img/posts/using-powershell-markdown-cmdlets/ansiBrightColors.png "ansiBrightColors")

Choosing the third sequence that was outputted from the above script you can now modify the Code property and which will change the Markdown options and ultimately the colors you see on the screen. The simplest way to do this is to use the Set-MarkdownOption cmdlet with the `-Code` parameter. Giving that parameter the value of `[102;90m` will change the code sections being displayed to a green background with a fadded black foreground color. You do not include any escape characters as part of the sequence.

```powershell
Set-MarkdownOption -Code '[102;90m'

'Get-Help Show-Markdown -Online' | Show-Markdown
```

![customCodeProp](/img/posts/using-powershell-markdown-cmdlets/customCodeProp.png "customCodeProp")

# Conclusion

It is now possible with PowerShell alone to generate html files from Markdown. That html code can be used for blog posts, wiki documents, or other projects you might prefer to write in Markdown first. While you might not want to completely switch your authoring workflow to the terminal, perhaps a nice break from your daily IDE is all you need. In that case, you can still author and view Markdown all within the comfort of your terminal of choice. It is also a great way to learn some ANSI escape sequences. Not everything you learn as to be practical. Sometimes just learn for fun!

# Acknowledgements

Shout out to [Rob Pleau](https://twitter.com/rjpleau) who wrote about this when it was introduced in PowerShell 6.1.

[PowerShell and Markdown](https://ephos.github.io/posts/2018-8-1-PowerShell-Markdown#in-the-console)

Shout out to [David Coulter](https://www.linkedin.com/in/davidcoulter/) and [Sean Wheeler](https://www.linkedin.com/in/scriptingsean/) for introducing me to Markdig and how they use it when authoring Markdown.

[markdig](https://github.com/lunet-io/markdig)

_Special thanks to [Jeff Hicks](https://twitter.com/JeffHicks) for including me and putting together the PowerShell 7 Blog week!_

# PowerShell 7 Blog Week Authors

| Author | Twitter | Blog |
| :----- | :----- | :----- |
|Josh King|@WindosNZ|[https://toastit.dev/](https://toastit.dev/) |
|Josh Duffney|@joshduffney|[http://duffney.io/](http://duffney.io/) |
|Adam Bertram|@adbertram|[https://adamtheautomator.com/](https://adamtheautomator.com/) |
|Mike Kanakos|@MikeKanakos|[https://www.networkadm.in/](https://www.networkadm.in/) |
|Jonathan Medd|@jonathanmedd|[https://www.jonathanmedd.net/](https://www.jonathanmedd.net/) |
|Thomas Lee|@doctordns|[https://tfl09.blogspot.com/](https://tfl09.blogspot.com/) |
|Prateek Singh|@singhprateik|[https://ridicurious.com/](https://ridicurious.com/) |
|Dave Carroll| @thedavecarroll|[https://powershell.anovelidea.org/](https://powershell.anovelidea.org/) |
|Dan Franciscus|@dan_franciscus|[https://winsysblog.com/](https://winsysblog.com/) |
|Jeff Hicks|@jeffhicks| [https://jdhitsolutions.com/](https://jdhitsolutions.com/)|
|Tommy Maynard|@thetommymaynard| [https://tommymaynard.com/](https://tommymaynard.com/) |
