---
  layout: post
  title: Reveal.js for linguistics Part II - Basic elements of a slideshow
  date: 2020-01-30
  image: images/presentations.jpg
  tags: [revealjs, study, presentations, how-to]
---

With revealjs outlined in a [previous post](/2020/01/11/revealjs-for-linguistics-part-2-basic-presentations/), with its pros and cons outlined there, now I will outline how to build a basic slideshow.
This is written for people unfamiliar with html and css, and contains only 
If you are comfortable with these, then this doesn't add much that you can't figure out from the [demo](https://revealjs.com/#/).
In general, the demo is the good place to start, this guide is for those creating an academic talk and want an easy point of entry.

**Note**: This is not exahustive, and will be updated regularly.

## Installation

In order to use reveal.js, you need to download the source files at the [Gihhub repository](https://github.com/hakimel/reveal.js/).
If you have a github account already, you can fork the repo or download the source files,a nd to get started quickly, edit the `index.html` file.

## Basic elements

- Slides are contained within `<section>...</section>` tags.
- `<section>` elements can be nested, in which case you create a vertical slide underneath the current one. This is useful when you have an after-point to the current slide, or you want to contain all slides underneath sections (like a paper).
- Unordered lists are created with `<ul>...</ul>`
- Ordered list are created with `<ol>...</ol>`
- In both list types, `<li>...</li>` introduce list elements.

## Incremental reveal

- Incremental uncover of elements is handled by putting `class="fragment"` within the elements:

```html
<ul>
  <li>This is an unordered list.</li>
  <li class="fragment">This element will be uncovered by advancing the presentation</li>
  <li class="fragment">And then this one will</ul>
</ul>
```

- You can specify the order of increments by providing a `data-fragment-index` value:

```html
<ul>
  <li class="fragment" data-fragment-index="3">This element will be the third uncovered.</li>
  <li class="fragment" data-fragment-index="1">This element will be the first uncovered.</li>
  <li class="fragment" data-fragment-index="2">This element will be the second uncovered.</ul>
</ul>
```

## Making multiple columns

You can make multiple columns using css flex.
A good guide to flex can be found [here](https://css-tricks.com/snippets/css/a-guide-to-flexbox/).
Enough to get going should be the following css:

```css
.columnscontainer {
    display: flex;
    justify-content: space-between;
    /* align-items: baseline; */
  }

.containerbaseline{
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  
  .col {
    flex: 1;
  }
  
  .colgrow {
    flex: 2;
  }
```

And then use as follows:

```html
<div class="columnscontainer">
  <div class="col">
    <p>This will be in the first column</p>
  </div>
  <div class="col">
    <p>This will be in the second column.</p>
  </div>
</div>
```

## Linguistic Examples

In order to put lingusitic examples in, see the guide at [htmlex](https://github.com/pwsmith/htmlex).
Short version: include the css file in that repo in your header, and then lay out the examples as follows.
The following is for non-glossed examples:

```html
<div class="example-container">
    <div class="individual-example">
        <div class="example-number"><p class='ex'></p></div>
        <div class="ab-counter"><p class='ab'></p></div>
        <div class="judgement"><p></p></div>
        <div class="example-sentence"><p>That's not my duck.</p></div>
    </div>
    <div class="individual-example">
        <div class="example-number"><p></p></div>
        <div class="ab-counter"><p class='ab'></p></div>
        <div class="judgement"><p>#</p></div>
        <div class="example-sentence"><p>It's beak is too shiny.</p></div>
    </div>
</div>
```

And for an example that needs glosses:

```html
<div class="gloss-example-container">
    <div class='gloss-individual-example'>
        <div class='example-number'><p class='ex'></p></div>
        <div class='judgement'><p>#</p></div>
        <div class='gloss-example'>
            <ol class='sentence'>
                <li class="gloss-individual-word">
                    <ol class='word'>
                        <li class="target-word">Ik</li>
                        <li class="target-gloss">I.<span class='smallcaps'>nom</span></li>
                    </ol>
                </li>
                <li class="gloss-individual-word">
                    <ol class='word'>
                        <li class="target-word">zag</li>
                        <li class="target-gloss">see.<span class='smallcaps'>past</span></li>
                    </ol>
                </li>
                <li class="gloss-individual-word">
                    <ol class='word'>
                        <li class="target-word">twee</li>
                        <li class="target-gloss">two</li>
                    </ol>
                </li>
                <li class="gloss-individual-word">
                    <ol class='word'>
                        <li class="target-word">ber-en,</li>
                        <li class="target-gloss">bear-<span class='smallcaps'>pl</span></li>
                    </ol>
                </li>
                <li class="gloss-individual-word">
                    <ol class='word'>
                        <li class="target-word">brood-je-s</li>
                        <li class="target-gloss">bread-<span class='smallcaps'>dim-pl</span></li>
                    </ol>
                </li>
                <li class="gloss-individual-word">
                    <ol class='word'>
                        <li class="target-word">smer-en</li>
                        <li class="target-gloss">spread-<span class='smallcaps'>inf</span></li>
                    </ol>
                </li>
            </ol>
            <p class='translation'>'I saw two bears, spreading on sandwiches.'</p>
        </div>
    </div>
</div>
```