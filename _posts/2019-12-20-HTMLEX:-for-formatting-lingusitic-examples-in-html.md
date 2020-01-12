---
layout: post
date: 2019-12-20
image: images/html.jpg
tags: [revealjs, html, css, open-source]
---

# HTMLEX

In the last semester I have been putting my teaching slides together using [reveal.js](https://github.com/hakimel/reveal.js/), rather than beamer as I usually would. 
Reveal.js is a javascript library for creating slide decks, where the slides are written in html format and styled with css and javascript.
Overall it's been a good experience, and I like using thi software. 
I will shortly post some articles about using reveal.js for making linguistics slides, but before that, I want to share a small part of it.

I have been wanting to use reveal.js rather than beamer for a while, since the sldies look really nice on [their example](https://github.com/hakimel/reveal.js/wiki/Example-Presentations), and being effectively webpages, are interactive, rather than static like pdf files.
Also, whilst beamer is a great piece of software, I wanted to try something new as the results turn out very similar in beamer, and customisation is difficult, [but not impossible](https://github.com/pwsmith/beamerthemesof).
One thing that has held stopped me up until this semester is the formatting required by linguists: we need to label examples with numbers and frequently need to line up morpheme glosses.
Both of these things are done automatically in LaTeX using `gb4e` but I couldn't find an alternative for html based on a quick few looks.

When I finally did start using reveal.js, I came across [this answer](https://linguistics.stackexchange.com/questions/3/how-do-i-format-an-interlinear-gloss-for-html/159#159) on stackexchange about the very issue of formatting glossing in html.
So, I modified the answer gave, and have built `htmlex`, which is a css file that defines elements that can be used for formatting linguistic examples when writing in html.
It's available [on my github page](https://github.com/pwsmith/htmlex) and available on an open source license.
It's intended to be a html equivalent to `gb4e/linguex`, for those of a LaTeX background.
It allows you to create output like the following that can be used in html pages like this, as well as html slides built using, for example, reveal.js or [impress.js](https://github.com/impress/impress.js/).


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
			<div class="example-sentence"><p>Its beak is too shiny.</p></div>
		</div>
		<div class="individual-example">
			<div class="example-number"><p></p></div>
			<div class="ab-counter"><p class='ab'></p></div>
			<div class="judgement"><p>*</p></div>
			<div class="example-sentence"><p>My that's duck.</p></div>
		</div>
		<div class="individual-example">
			<div class="example-number"><p></p></div>
			<div class="ab-counter"><p class='ab'></p></div>
			<div class="judgement"><p></p></div>
			<div class="example-sentence"><p>Her eggs are so smooth.</p></div>
		</div>
</div>

<div class="gloss-example-container">
            <div class='gloss-individual-example'>
            <div class='example-number'><p class='ex'></p></div>
            <div class="ab-counter"><p class='ab'></p></div>
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
            <div class='gloss-individual-example'>
                <div class='example-number'><p></p></div>
                <div class="ab-counter"><p class='ab'></p></div>
                <div class='judgement'><p></p></div>
                <div class='gloss-example'>
                    <ol class='sentence'>
                        <li class="gloss-individual-word">
                            <ol class='word'>
                                <li class="target-word">Oh</li>
                                <li class="target-gloss">Oh</li>
                            </ol>
                        </li>
                        <li class="gloss-individual-word">
                            <ol class='word'>
                                <li class="target-word">dat</li>
                                <li class="target-gloss">that</li>
                            </ol>
                        </li>
                        <li class="gloss-individual-word">
                            <ol class='word'>
                                <li class="target-word">was</li>
                                <li class="target-gloss">be.<span class='smallcaps'>3.sg.past</span></li>
                            </ol>
                        </li>
                        <li class="gloss-individual-word">
                            <ol class='word'>
                                <li class="target-word">een</li>
                                <li class="target-gloss">a</li>
                            </ol>
                        </li>
                        <li class="gloss-individual-word">
                            <ol class='word'>
                                <li class="target-word">wonder!</li>
                                <li class="target-gloss">miracle</li>
                            </ol>
                        </li>
                    </ol>
                <p class='translation'>'Oh that was a miracle!'</p>
                </div>
                </div>
        </div>

For more information, and guide for usage, see [https://github.com/pwsmith/htmlex](https://github.com/pwsmith/htmlex).