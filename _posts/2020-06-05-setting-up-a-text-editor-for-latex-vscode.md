---
layout: post
title: "Setting up a Visual Studio Code for LaTeX"
date: 2020-06-05
image: images/desk-web.jpg
tags: [latex]
---

In a [previous post](/2020/05/30/setting-up-a-text-editor-for-LaTeX/) I showed how one can set up Atom to use for writing LaTeX, and discussed a number of benefits of using an editor such as Atom over the one that is by default available.
This article discusses how to set up Microsoft's Visual Studio Code.
This post is less detailed on why you might want to make use of this functionality, and so if some of the terms are not familiar, you should look at the [Atom guide](/2020/05/30/setting-up-a-text-editor-for-LaTeX/) for more details.

### Installation

There are a wealth of options to download VSCode on [the download page](https://code.visualstudio.com/download).
On Windows and Mac it is a simple download and install.
For Linux, there is the option of either a .deb/.rpm file, or downloading through [Snap](https://snapcraft.io).
It's also available on Arch based systems through pacman.

Once installed you are presented with the following screen. 
Whilst it works for writing text out of the box, you will want to install some extensions to cusomtise it for your own tastes.
These can be found in the extensions tab, where the red box is on the screenshot.
Alternatively press `C-shift-x`.

![vscode welcome](/images/vscode/vscodewelcome.png)
*Where to find the Marketplace. It is Microsoft, after all.*

The extensions are part of the reason that VSCode is so popular for use in software development: there is a *huge* community that use it and make extensions for it.
You can search for extensions after clicking on the tab.

### Recommended Packages

#### LaTeX Workshop

In contrast to Atom, you don't need to add any language support for LaTeX in VSCode, so this is the only extension that you *really* need.
VSCode is oriented at being more than a text editor, and towards an Integrated Development Environment for programming. 
[LaTeX Workhop](https://marketplace.visualstudio.com/vscode) very much follows in that regard, providing a lot of tools that you would need whilst writing.
Like [latex-tools for Atom](/2020/05/30/setting-up-a-text-editor-for-LaTeX/), it gives the option to compile the document from within the editor and has bibliography integration.
Not many commands are available by keybindings by default (though you can define your own keybindings):

- `C-alt-b`: compile the document.
- `C-alt-j`: sync to pdf from cursor.

However, you can open a tab on the left of the screen by clicking the TeX icon which provides much more:

![VSCode workshop tools](/images/vscode/vscworkshop-tools.png)
*Click the TeX logo on the left of the screen for a lot of useful tools.*

Among these are options, for instance:

1. build with luatex.
2. select which viewer to view the pdf in (either a tab within VSCode, an external pdf viewer or within a web browser).
3. search for citations.

Citation support is not quite as seamless as with Atom.
By default, when you type `\cite{}` in Atom, the bibliography immediately opens.
In VSCode, this sort of happens in the sense that the Intellisense tab shows a list of citation keys that narrows down as you type, but you can't search information about the citation itself.
You *can* do this, but you need to select *Open citation browser* under the *Miscellaneous* tab on the left.
This works, but is less seamless than in Atom, and you can only search by author name, which is not ideal.

![vscodecitation](/images/vscode/vscodecitation.gif)
*This is kind of annoying.*

One quite useful option is the *Snippet Panel*, which when clicked will open a tab on the right with symbols, which when selected will add in the relevant LaTeX code at your cursor in the document.
It's mostly maths symbols, but also Greek letters and some latin symbols.

![Snippet Panel](/images/vscode/vscodesymbols.png)
*Click on the symbol you want and the code goes in.*

One downside is that it's not immediately obvious how to use XeLaTeX to compile, but luatex works just as well for me.

#### Insert Unicode

This is more for writing linguistics papers.
As I've written about before, it is better to write directly in unicode for non-Latin and phonetic symbols rather than using, e.g. `tipa`.
I've nothing inherently against `tipa`, it works totally well for the output, but I find it difficult to read in the source, and you can't reuse that example outside of LaTeX.

The problem with writing in unicode is that you need to know the right code, and there is a lot.
[Insert Unicode](https://marketplace.visualstudio.com/items?itemName=brunnerh.insert-unicode) is a wonderful extension that allows you to search for characters by name or by code, and then select them to insert.
If you already the code, you don't need this, but if you know which symbol to look for, it will save you the time to look it up online.
What is so nice about this extension, and here it has the edge over the equivalent in Atom, is that you can save your frequently used symbols in whatever way you like.

![Insert Unicode](/images/vscode/insertunicode.gif)

To save whichever you want, press `C-shift-P` to bring up the command pallette, and search for *Insert Unicode: Manage Favourites*, and you can use the GUI.
Alternatively you can edit the settings.json file with your desired structure.

### Snippets

Unlike *latex-tools* for Atom, *Latex Workshop* does not define a large amount of snippets, which means that you will end up making your own.
VSCode stores snippets in *json* format.
As is common, the important elements for each snippet are the **prefix**, the **body** and the **description**.
Cursor positions are designated numerically after `$`, with `$1` denoting the first position, `$2` the second after pressing `tab` *etc*.
The **scope** of the snippet is handled automatically in VSCode, unlike in Atom, as each format has its own .json file.
Thus, your LaTeX snippets will be stored in the *latex.json* file.

To make a new snippet:

1. press `C-shift-p` and search for *Preferences: Configure User Snippets*.
2. Select *latex.json* from the next menu.
3. Write the snippet between the outer `{}` braces.

Below is an example for inserting small caps.

```json
{
    "small caps": {
    "prefix": "sc",
    "body": [
        "\\textsc{$1}",
    ],
    "description": "adds small caps"
    },
}
```

Be sure to make sure the commas are at the end of the lines.

## Customising

There is a *huge* amount that you can do with Visual Studio Code to set it up according to your own favourites.
[Top theme lists](https://dev.to/thegeoffstevens/50-vs-code-themes-for-2020-45cc) are not hard to find online, and can usually be added through the VSCode Marketplace.
Once installed, switching themes is very easy, open the command pallette and search for *Preferences: Color Theme* and you can preview them before selecting.[^kt]
Unlike with Atom, the UI and the text window are treated as one.

[^kt]: Or press `C-k C-t`.

![vscodetheme](/images/vscode/vscodetheme.gif)

### Ultimately, it's an editor for everything

VSCode is a brilliant piece of software and using it for LaTeX is just a small part of what you can do with it.
Like Atom, it too is built on [Electron](www.electronjs.org), so it's not exactly a lightweight program suitable for older computers, but an admirable amount of work has gone in to optimise it and reduce memory usage, so you shouldn't have any problems on most computers.
Furthermore, it's got some key functionality that Atom lacks.
When I write in LaTeX, I have to admit that I don't tend to use the build options that the editor allows for.
That is, I wouldn't build it using the options given above in *LaTeX Workshop*.
There's no problem with it, I just prefer to compile from the terminal and use the editor for just snippet and bibliography integration.
It's a personal choice, and it works for me.
VSCode is nice in this regard in that it includes a terminal within the editor, just press ``C-shift-```, so you can write, and then compile without leaving the window.

![Running in the terminal](/images/vscode/terminalrun.gif)
*Run in the terminal without leaving the program.*

Another draw towards using VSCode is that I use it for writing .html files, and it is nice to have the option to write LaTeX in the same program.
VSCode is really great for writing pretty much any format in, so if you find that you sometimes use LaTeX, sometimes Markdown, sometimes .html, sometimes Python *etc etc*, and you want to use the same program for all, then this is a great choice.
