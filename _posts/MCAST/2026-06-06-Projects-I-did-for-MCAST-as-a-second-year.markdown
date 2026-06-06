---
layout: post
title: "Projects I did for MCAST as a second year"
date:   2026-02-14 13:50 +0100
categories: MCAST
---

This post is a general description of all the projects I have done for MCAST
during my time as a second year level 4 student. It also contains links to the
source code for each project.

## Introduction to Mobile Application Design

Tools used:
- [Ionic framework](https://ionicframework.com/)

This was probably the project I had the most *fun* with however, that is likely
due the fact I had the most time to complete it as it is the first one I did.
It was a mock food delivery application with inspiration from Wolt's mobile
application. It was quite simple, in fact I did most of it within the confines
of what we had learned in our lectures.

The application allows for the user to order items using two buttons, a plus
and a minus button and then complete their order and see what they have ordered
beforehand on a modal. It also lets them switch outlets which changes the name
of the restaurant in the top right corner, in this case "Fudi" to show their
selected location I.E "Fudi Mosta".

[Source code on Codeberg](https://codeberg.org/ParadiseOfMagic/Fudi_Delivery_Application)

## Server Side Scripting

Tools used:
- [Php](https://www.php.net/)
- [Mariadb](https://mariadb.org/)
- [Apache http server](https://httpd.apache.org/)

This is a website that allows users to add games to a list with details such as
release year, publisher and cover. It only allowed registered users to add
games and administrator users to remove and update game entries. The website is
hosted at a [InfinityFree
subdomain](https://keoncachiaswd20252026.infinityfree.me/index.php?i=1). Please
note that for whatever reason the register page does not work, I could not find
what the problem was unfortunately.

[Source code on Codeberg](https://codeberg.org/ParadiseOfMagic/UltimateGameList)

## Project

Tools used:
- [Python](https://www.python.org)
- [Tkinter](https://docs.python.org/3/library/tkinter.html)
- [Scikit learn](https://scikit-learn.org/stable/index.html)
- [NumPy](https://numpy.org/)
- [Nltk](https://www.nltk.org/)
- [Pytest](https://docs.pytest.org/en/stable/)
- [Black](https://black.readthedocs.io/en/stable/)

The topic for my year was **Natural Learning Processing** or just NLP. I did a
similarity analysis tool for webpages with a GUI using Tkinter that could fetch
HTML from URLs and parse them. The project was much simpler than I had
anticipated though, I suppose most of the code is in its libraries.

I had also added unit tests using Pytest, a tool I was familiar with from my
[contribution to Qtile](https://github.com/qtile/qtile/pull/5735). I thought
this part was required especially since we had a topic about unit tests but, as
it turns out it was not. At least it added to the "maintainability" of my
project which was a required task. I had also used a formatter, Black and
Git as my Version Control System, both tools I'm quite familiar with by now.

[Source code on Codeberg](https://codeberg.org/ParadiseOfMagic/SWPSA)


### Some extra tidbits

Now that I am concluded with my level 4 studies, I am thinking of advancing up
to level 6. As for which diploma I'll choose, at the time of writing I am still
waiting for the new course details to be published. Which will be sometime
during July.

I did all of these projects with GNU Emacs as my text editor this time. While I
enjoy a lot of aspects of Emacs such as Elisp and its wide range of uses such
as Email and music playing, unfortunately the keybinds gave me repetitive
strain injury so I am probably going to stick with Neovim and similar editors
from now on.
