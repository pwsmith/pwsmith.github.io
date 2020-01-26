---
layout: post
title: Reveal.js for Linguistics - how to make a fully formatted citation list
date: 2020-01-25
image: images/presentations.jpg
tags: [revealjs, presentation, latex, guides, pandoc]
---

In this post I will show how to make a fully formatted citation list if you have made slides in reveal.js.
This is useful if you are giving an academic presentation with the slides, and want to follow the convention of giving a short citation note in the slide, with the full information at the end.

## Why?

One of the drawbacks of reveal.js, that I noted [in the overview](/2020/01/11/reveal.js-for-linguistics/) is that there is no native way of making a citation list in html.
That is, there is no equivalent of bibtex/biblatex that will automatically scan your file for citation keys, and output a formatted list of references at the end of your document based upon what it finds.
In the same post, I outlined why I don't think that this is a problem in and of itself — a reference list is effectively a way of pointing someone viewing your slides to a source, and there are html-based ways of doing that, such as making a hyperlink using the `<a>...</a>` tag.
Yet, despite some positives, there are a couple of drawbacks: unpublished work that is not associated with a url is difficult to incorporate, and, whilst it is easy to click on links if you are viewing the slides at a later point, if you are in the audience at the talk and don't have online access to the slides, then you can't check which exact source is being used.[^host]
It is also a convention of academia that one provides references as well, so it is understandable to want a formatted list at the end.

[^host]: That said, if you use reveal.js to make slides, then it makes sense to host the slides online, say for instance on Github Pages, and give the audience a link at the beginning of the talk.

## Prerequisites

This method requires use of the command line and the ability to run a python script.
So, you should have a basic knowledge of the command line on your computer (nothing complex, but how to open the terminal, how to navigate to a directory, and how to put in basic commands), as well as having [Python](https://www.python.org/) installed.
Furthermore, we will be using [Pandoc](https://pandoc.org) to generate the citation list.

### Install Python

You can check if Python is already installed on your computer by typing into the command line:

```shell
python --version
```

If the output gives (with the value of X depending on what version is installed on your system), then you're good.[^python2]

```shell
Python 3.X.X
```

[^python2]:If you get the response:```Python 2.X.X```, then you should consider upgrading Python. Python 2 is [in the process of being deprecated](http://pyfound.blogspot.com/2019/12/python-2-sunset.html?utm_source=feedburner&utm_medium=feed&utm_campaign=Feed%3A+PythonSoftwareFoundationNews+%28Python+Software+Foundation+News%29), and will no longer be stable after April 2020.

Note that if you are using a Mac, it's likely that you'll have both Python 2 and Python 3 installed.[^windows]
You can check if Python 3 is available on your system by typing:

[^windows]: This may also be the case for Windows, but I don't know.

```shell
python3 --version
```

If this is the case, then wherever I use the command `python` in this guide, you should replace it with `python3`.
If you receive a notice `command not found: python`, then you need to [install python](https://www.python.org/downloads/).

### Install Pandoc

Pandoc is a command line tool useful for converting documents from one file type to another.
Instructions for installing it on a variety of systems can be found [here](https://pandoc.org/installing.html).[^pandocinstall]
We will use the filter `pandoc-citeproc`, which sometimes is not included in the installation (e.g. if you're installing on Linux).


[^pandocinstall]: If you're using Linux, then pandoc can be installed using various package managers, but be aware that you should upgrade the package to the latest version after installation, as older versions are often installed. I'm assuming that for this guide, the latest version is installed on your system — on mine it is 2.8.

### Bibliography

Finally, you'll need a LaTeX style bibliography.
If you don't have one already, you can make one using [Bibdesk](https://bibdesk.sourceforge.io/), [Jabref](https://www.jabref.org/) or [Zotero](https://www.zotero.org/) (amongst others).

## Step 1: Incorporating citation keys

The first step is to incorporate citation keys within the document.
In [my overview](/2020/01/11/reveal.js-for-linguistics/), I noted that given that reveal.js makes slide decks that are web-based, it makes sense to make use of this and cite other works as a hyperlink where possible, to take the reader right to the selected work.
I will take this approach here.
Given that the slides are written in html, we will be using the `<a>...</a>` tag, where (the `href` value is set to the webpage of the citation, and the text between the tags is what you want displayed on the slide).
Below is a sample document before we've put in the citation keys, which I'll be using as a toy example for the remainder of the tutorial:

```html
<!-- aba.html -->
<html>
    <head>
        <link rel="stylesheet" href="linguistic-examples.css">
    </head>
    <body>
        <title>Generating html references</title>
    </body>
    <p><a href="https://www.glossa-journal.org/article/10.5334/gjgl.362/">Moskal (2018)</a> shows that suppletion for the exclusive happens only when the inclusive pronoun is also suppletive.
    This follows in work on *ABA patterns in suppletion in <a href="https://mitpress.mit.edu/books/universals-comparative-morphology">Bobaljik (2012)</a> and <a href="https://link.springer.com/article/10.1007%2Fs11049-018-9425-0">Smith et al (2019)</a></p>
</html>
```

In order to eventually access the bibliography entry for the citation, one should also incorporate a citation key using `data-citation-key`, which is a custom attribute added to `<a>` elements in order to specify a citation key.[^name-of-citekey] 
The value of `data-citation-key` should correspond to the citation key in your .bib file.
As we will eventually be converting a markdown document to generate the bibliography, we need to include `@` immediately before the citation key value, as this is the citation identifier in markdown. 
A basic citation would then look like the following:

[^name-of-citekey]: The only thing that is important here is the prefix `data`; `-citation-key` is used as its semantically easy to follow. However, one can use whatever you want, e.g. `data-key`, `data-citation`, `data-ref`.

```html
<a href="https://www.glossa-journal.org/article/10.5334/gjgl.362/" data-citation-key="@moskal2018">Moskal (2018)</a>
```

**Important**: the value *after* `@` in the field for `data-citation-key` *must* match the citation key in your .bib file.
That is, the value of `data-citation-key` should be of the form `@+cite_key`.

Our toy source file would then look like the following, once the `data-citation-key` values are added:[^yearsmatch]

[^yearsmatch]: In my bibliography, the years of the first and third citation key doesn't match the publication year. This doesn't matter, I'm just noting in case you're wondering *why* it appears that that is the case.

```html
<html>
    <head>
        <link rel="stylesheet" href="linguistic-examples.css">
    </head>
    <body>
        <title>Generating html references</title>
    </body>
    <p><a href="https://www.glossa-journal.org/article/10.5334/gjgl.362/" data-citation-key="@moskal2017">Moskal (2018)</a> shows that suppletion for the exclusive happens only when the inclusive pronoun is also suppletive.
    This follows in work on *ABA patterns in suppletion in <a href="https://mitpress.mit.edu/books/universals-comparative-morphology" data-citation-key="@bobaljik2012">Bobaljik (2012)</a> and <a href="https://link.springer.com/article/10.1007%2Fs11049-018-9425-0" data-citation-key="@smithetal2016">Smith et al (2019)</a></p>
</html>
```

## Step 2: Extracting the citation keys

Once all of the citation keys are in the html source file, they need to be extracted so that there is a list containing all the citation keys that you want to then be references.
We will use Python for this step. 
Overall, we want to be able to call the .py script from the command line, and in the same command specify which .html source file to look for the citations in.
So, we will aim for the following:

```shell
python SCRIPT.py SOURCE.html
```

For the first step, we will use the `sys` library in python, which should already be present in your Python installation. 
In order to extract the citation keys we will use [Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/), which is a Python library for processing the html code underlying web pages.
Firstly, install Beautiful Soup on your system using the following at the command line:[^pip3]

[^pip3]: If you are using a Mac with both Python 2 and Python 3 installed, you should use pip3 instead of pip.

```shell
pip install --user beautifulsoup4
```

Make a file — we'll call it `refgrab.py` here, but the name doesn't matter too much — and insert the following:

```python
# rebgrab.py
import sys
from bs4 import BeautifulSoup

file = sys.argv[1]

with open(file, "r") as fp:
    soup = BeautifulSoup(fp)

for link in soup.find_all('a'):
    print(link.get('data-citation-key'))
```

What this does:
1. Imports the `sys` library.
1. Imports the Beautiful Soup library.
2. Scans the file picked out by `sys.argv[1]` (the second file you specify on the command line, in our case, `aba.html`), looks for all `<a>` elements in that file and returns a list of each value of `data-citation-key` associated with each `<a>`.[^missingas]
This list is what we will use to build our reference list.

We can run this script on our toy example using the following:

[^missingas]: If you use `<a>` elements that are not citations, then you will end up with some empty references in the markdown file that is generated. Dont' worry about this: Pandoc will run fine anyway.

```shell
python refgrab.py aba.html
```

It will return the following to the terminal window:

```
@moskal2017
@bobaljik2012
@smithetal2016
```

## Step 3: Making the reference list

Now that we have a way to extract all the citation keys from our html document, we need a way to allow Pandoc to access them. 
One way to do this is to print the output of `refgrab.py` to its own file.
To do this, redirect the output of `refgrab.py` to a temporary markdown file `refs_temp.md` that we'll delete later on:

```shell
python refgrab.py aba.html > refs_temp.md
```

This will produce a document like the following, from our sample file above:

```markdown
<!--- refs_temp.md --->
@moskal2018
@bobaljik2012
@smithetal2016
```

Alternatively, if you don't want to generate an intermediate file, you can pipe the output of the python script directly to Pandoc, as described [later on](#saving-time).

## Step 4: Generating the citation list

With the file `refs_temp.md`, we can now build a citation list of what is contained within.
To do this, run the following command, subsituting the bibliography path for the relevant one for your bibliography.
If you chose a name other than `refs_temp` for the markdown file in the previous step, use that name instead of `refs_temp.`[^irrelevantnames]

[^irrelevantnames]: You can pick whatever names you want for this file, and `references.html`: they are temporary ones and we'll delete them in Step 6.

```shell
pandoc --bibliography /home/pwsmith/Dropbox/Ducks/biblio.bib --filter pandoc-citeproc refs_temp.md -o references.html
```

## Step 5: Incorporating the citations into your document

Finally, with the references generated, you should end up with a file called `references.html` with the following as its content:

```html
<!-- references.html -->
<p><span class="citation" data-cites="moskal2017">Moskal (2018)</span> <span class="citation" data-cites="bobaljik2012">Bobaljik (2012)</span> <span class="citation" data-cites="smithetal2016">Smith et al. (2019)</span></p>
<div id="refs" class="references hanging-indent" role="doc-bibliography">
    <div id="ref-bobaljik2012">
        <p>Bobaljik, Jonathan D. 2012. <em>Universals in Comparative Morphology</em>. Cambridge, MA: MIT Press.</p>
    </div>
    <div id="ref-moskal2017">
        <p>Moskal, Beata. 2018. “Suppletion Patterns in Clusivity and the Role of Markedness.” <em>Glossa</em> to appear, expected publication in 2018.</p>
    </div>
    <div id="ref-smithetal2016">
        <p>Smith, Peter W., Beata Moskal, Ting Xu, Jungmin Kang, and Jonathan D. Bobaljik. 2019. “Case and Number Suppletion in Pronouns.” <em>Natural Language and Linguistic Theory</em> 37 (3): 1029–1101.</p>
    </div>
</div>
```

Pandoc generates both the reference list, which is what we are after, as well as the intext citations, which we don't need.
What remains then is to select all the lines contained within the `<div id="refs">` and copy-paste that into the appropriate place in your html document.
Note:
- If you're making a webpage, then you can most likely copy the entire "refs" div.
- If you're making slides in reveal.js, then you'll most likely need to split the references over multiple slides so put in `<section>...</section>` elements where needed (and don't copy the entire "refs" div).

The output will render as follows in your browser:

<div class="highlight">
<div id="refs" class="references hanging-indent" role="doc-bibliography">
    <div id="ref-bobaljik2012">
        <p>Bobaljik, Jonathan D. 2012. <em>Universals in Comparative Morphology</em>. Cambridge, MA: MIT Press.</p>
    </div>
    <div id="ref-moskal2017">
        <p>Moskal, Beata. 2018. “Suppletion Patterns in Clusivity and the Role of Markedness.” <em>Glossa</em> to appear, expected publication in 2018.</p>
    </div>
    <div id="ref-smithetal2016">
        <p>Smith, Peter W., Beata Moskal, Ting Xu, Jungmin Kang, and Jonathan D. Bobaljik. 2019. “Case and Number Suppletion in Pronouns.” <em>Natural Language and Linguistic Theory</em> 37 (3): 1029–1101.</p>
    </div>
</div>
</div>

## Step 6: Deleting the temp files (optional)

The last step is to remove the file `refgrab.py`, and, if you want, the `references.html`, as they are no longer needed and can be easily regenerated following the steps above if they are.
To remove them, simply run the command:[^tempnames]

[^tempnames]: Agian, if you used different names for the intermediate files, be sure to use those instead.

```shell
rm refs_temp.md references.html
```

## Saving time

If you're comfortable with all of the above steps, you can simply run one command (and not build `refs_temp.md`) by piping the output of the python script directly into pandoc.
In which case, steps 3 and 4 are conflated into one:

```shell
python refgrab.py SOURCE.html | pandoc --bibliography /home/pwsmith/Dropbox/Ducks/biblio.bib --filter pandoc-citeproc -o references.html
```

Again, be sure to put in the relevant names and pathways for the python script, source file and bibliography.

## Notes