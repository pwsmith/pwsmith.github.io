---
  layout: post
  title: Reveal.js for linguistics Part II - Creating a basic slideshow
  date: 2020-01-25s
  image: images/presentations.jpg
  tags: [revealjs, study, presentations, how-to]
---

With revealjs outlined in a [previous post](/2020/01/11/revealjs-for-linguistics-part-2-basic-presentations/), with its pros and cons outlined there, now I will outline how to build a basic slideshow.
Note that this is written for people unfamiliar with html and css. 
If you are comfortable with these, then this doesn't add much that you can't figure out from the [demo](https://revealjs.com/#/).

## Installation

In order to use reveal.js, you need to download the source files at the [Gihhub repository](https://github.com/hakimel/reveal.js/).
If you have a github account already, you can fork the repo or download the source files 

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

