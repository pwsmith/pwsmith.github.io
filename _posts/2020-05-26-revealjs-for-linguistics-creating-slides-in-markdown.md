---
layout: post
title: "Reveal.js for linguistics: using Markdown to write slideshows"
date: 2020-05-26
image: images/presentations.jpg
tags: [reveal.js, pandoc, markdown, presentations]
---



## What is Markdown

Markdown is a markdown language that uses an extremely simple syntax, which then gets formatted correctly in the output. 
The writer writes the files in plain text, and uses special tags for simple formatting.
For instance enclosing text in asterisks will render as italics in the output: `*in the source code, this block is enclosed with italics*` will yield *in the source code, this block is enclosed with italics*.
Enclosing text in `**double asterisks**` will output as **boldface**
Similarly, writing the following:

```markdown
- Some sentence here.
- Another here.
- And another here.
```

will yield a bulleted list:

- Some sentence here.
- Another here.
- And another here.

Markdown is increasingly being used in many many ways, such as blogging: this post is written in markdown for instance.
It has the advantage of being very simple to write, having a lot of support in applications that are based in plain text, as well as the added bonus that the source files are very easy to read.
Markdown compares very favourably in this regard to, for isstance, text that is written for LaTeX processing.

Using markdown, it is very easy to write slide shows, handling most of the general formatting you need for your slides.
Although reveal.js is a framework that at its most basic uses .html input, it works very well with reveal.js, making it very easy to quickly create slide shows.

There are in fact two ways in which one can write the source file in Markdown for use with a reveal slide deck, and I will discuss each in turn here.
Each has benefits and drawbacks, which I'll try to highlight.

## Method 1: Embedding a markdown file in the main html document

The first method calls a file written in markdown from the main html document.

### Prerequisites

In order to use this method you need to:

1. Install [Node.js](https://nodejs.org) (**Note:** I *strongly* recommend doing this with [nvm](https://github.com/nvm-sh/nvm)). 
2. Fork the reveal.js repository from [Github](https://github.com/hakimel/reveal.js/)

### Making the slides

Once you have forked the repo from github, open the directory in whatever text editor you like ([Visual Studio Code](https://code.visualstudio.com/) is a good one for this).
The forked repository contains a number of demo files that you can alter.
Within the `index.html` file, delete everything between the `<div class="slides">...</div>` and insert the following:

```html
<section data-markdown="example.md"
         data-separator="^\n\n\n"
         data-separator-vertical="^\n\n"
         data-separator-notes="^Note:"
         data-charset="utf-8">
</section>
```

Then, create a file within the same level as `demo.html` (or alter the path of `data-markdown` accordingly), and write your slides.
The file name should match whatever value is given for `data-markdown` in the above code, and should have the extension *.md*.


Certain variables are defined in the above code.
`data-separator-notes` refers to what sequence of characters will start a note (for using speaker notes).
`data-separator` refers to what separates slides.
`^\n\n\n` here means three blank lines delimit a slide.
So, the following will produce two slides, because of the three blank lines between the first slide's content and the heading of the second slide:

```markdown
# First slide

Content of first slide



# Second slide

```

`data-separator-vertical` refers to how a vertical slide is defined.
Here, `^\n\n` means that two blank lines will embed a vertical slide.

```markdown
# First slide

First slide content


Content on slide nested under first slide



# Second slide

Second slide content
```

This is the example given in the user guide.
However, I find using blank lines to be difficult to see and judge.
Luckily, you can change the delimiters, to get something that works for you.
For instance, with the following, you will begin a nested vertical slide when a new line starts (indicated by '^') with a sequence of three hyphens and a new horizontal slide when a new line starts wtih with a sequence of five hyphens.

```html
<section data-markdown="example.md"
         data-separator="^-----"
         data-separator-vertical="^---"
         data-separator-notes="^Note:"
         data-charset="UTF-8">
</section>
``` 

```markdown
# First slide

Here is some content
---
This will start a nested slide
-----
# Second slide

Here is some content --- note that the sequnce of three dashes here will **not** start a nested slide, since they do not start a line.

So don't worry, fans of LaTeX em-dash :)
---
In contrast to the above, which will ensure that this is on a nested slide.
```

Markdown syntax is very easy and very readable, however, you will most likely want some flexibility in customisation.
You can add in attributes to elements using css comments.
So, the following will assign the class "green" to the text.
Thus, whatever you style "green" as in a linked css file, it will appear as such.

```markdown
Here is some text. <!-- .element: class="green" -->
```

Whilst this method works, it is at times a little imprecise.
So, you can also, when you need interleave html with inline css code throughout the markdown, and it will still render correctly.
Thus, below is equivalent to the above:

```html
<p style: class="green">Here is some text.</p>
```

### Viewing the slides

Normally with reveal.js presentations, you can simply open the file in a web browser and it will render correctly. 
However, note that using the method of an external markdown file, in order to actually view your slides, you need to run them on a local web server.
Navigate to the `reveal.js` directory and run the following commands:

```shell
$ npm install
$ npm start
```

Assuming that npm installs all the relevant packages correctly, then you will be able to view your slides at `http://localhost:8000`.[^normalway].

[^normalway]: You can also run these commands when you are writing your slides in html, and use the live preview to automatically update them when you save the file.

### Positives

The major benefit of using this method is that you can write with markdown and the result is immediately available.
In fact, once you run `npm start`, the slides will render as you save the document, meaning you can see them online as you type.
This is a big positive over the pandoc method described below, which requires you to compile them each time.
Secondly, your code is flexible: you can define whatever separators for the slides you want, so you can make it readable to yourself in whatever way works for you.

### Negatives

One of the biggest disadvantages to this is that you need all the additional software to see the slides. 
If you stick to projecting from your local computer, it's not a problem, however, if you need to use a different one, then you need to hope that it has Node.js installed, and hope it's a compatible version.
There's testing your slides in advance of your talk at a conference, and then there's this.

Secondly, this method does require some extra software.
Once you run `npm install`, the installed `node_modules` directory gets very big, around 400mb.
As a one-off, this is fine, but if you want multiple different reveal.js directories, your hard drive will fill up fast.


## Method 2: Using pandoc

It is also possible to use [pandoc](https://pandoc.org),  which is freely available, cross-platform document conversion software.
Pandoc is a command line utility that is able to convert text files between a number of formats and into a variety of outputs. 
For our purposes, it is able to convert a markdown file into a hmtl file with all the necessary reveal.js boilerplate.

### Prerequisites

There are detailed instructions on how to install pandoc [here](https://pandoc.org/installing.html)

If you install from a package manager, be sure to check that the version is at least 2.5 or higher. 

**Important**: you still need to download or clone the reveal.js repository and place the pandoc output in the right place.

### Basic use

To create a simple slide deck, write the content in markdown.
The metadata, such as title, author and date should go in the following format in the top lines of the file:

```markdown
---
title: Hello
author: Peter
date: May 26th 2020
---

# Here is the first slide

- The first bullet point.
- The second one.
- And the third.

# This will start a new slide.

- New bullet point.
- etc.
- etc.

```
Then, in a terminal window run the following, substituting the capital letters for your relevant file names:

```shell
pandoc -t revealjs INPUTFILE.md -s -o OUTPUTFILE.html
```

If you want bullet points to be uncovered one by one, then include the option -i (for incremental):

```shell
pandoc -t revealjs INPUTFILE.md -s -i -o OUTPUTFILE.html
```

The output of this command will be a file with all the necessary reveal.js boilerplate, and your markdown file converted into HTML.
The file should then be placed in a position where the links to the relevant reveal.js files can be located.
By default, this is the directory **above** the reveal.js you cloned earlier.


## Customisations

Whilst the above will create a fully functioning reveal.js slide deck, you may wish to customise it further.
For instance, the default theme is the 'black' theme, but can be changed in the following way:

```shell
-V theme=$THEMENAME
```

Any of the variables that can be set in the configuration of reveal.js can be tweaked this way. 
For instance, if you wish to change the transition speed and use the beige theme, you can use:

```shell
pandoc -t revealjs INPUTFILE.md -s -i -o OUTPUTFILE.html -V theme=beige -V transitionSpeed=fast
```

Citations can be added using regular markdown citation syntax, and running with the `--filter  pandoc-citeproc` option.
Note that you should then specify the path to the bibliography in the metadata of your markdown file.
For instance:

```markdown
---
title: Hello
author: Peter
date: today
bibliography: /path/to/your/bibliography
---

@moskal2015 shows that case suppletion is unattested in lexical nouns but is attested in pronouns.

```

```shell
pandoc -t revealjs --filter pandoc-citeproc INPUTFILE.md -s -o OUTPUTFILE.html
```

Finally, trees can be added in as an image file.
There may be ways to first run them through latex to output them as an image file automatically (see a discussion from Pavel Iosad [here](https://www.anghyflawn.net/blog/2014/teaching-slides-with-pandoc-and-reveal-js/)), so it is possible to do it like this.

As with the other markdown option above, when it comes to presenting you need a reveal.js installation on the presentation computer.
In this case it is easier as, if you are using a different computer to your own, all you need to do is copy the root directory of your sildes.
That is, there is no need to install npm modules so it is more lightweight than the other option.

### Negatives

Probably the biggest drawback of using pandoc and markdown is that there is no live update as you are working.
So, you must compile the slides each time you want to see changes, or read the source code.
This is not such a problem, and is relatively quick, but it is still a disruption if you like seeing the output to check as you go along.

### Postscript

If you use pandoc and you decide you'd rather your output be a beamer presentation rather than reveal.js, then assuming there is not too much cutsom css/html in your source code, then use the following command:

```shell
pandoc -t beamer INPUTFILE.md -o OUTPUTFILE.pdf
```

and you have the slides in beamer instead, nice!
**Note, for this, you need LaTeX installed on your system.**
