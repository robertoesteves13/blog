---
title: "Muista Development 1"
date: 2024-05-22T16:17:48-03:00
draft: false
---

# Introduction

Long time no see! I have been busy with things in life, but things started to
get in order now. After reviewing all the progress that I've made with muista,
I decided to scrap everything because I noticed one thing... **my thought process
was flawed!**.

You see, I always try to start a new project by jumping straight to programming.
But this project has something different from the "toy examples" I've made to
learn some technology. I want to make a *product*, a *FOSS product* to be precise,
and I want it to be very good at its objective. Analyse the following phrase:

> Muista is a program for structuring a well-defined language course with
> rich-text support focused in immersive learning.

When you see this, it might be a simple thing to program and should not take too
much time. But I missed to ask a question about something really crucial in
software development when you want to make something really good... **how are you using it?**

For those who doesn't know, immersive learning is the proccess of learning a
language by absorbing the content of some media in their language. For example,
let's say you like cooking and you want to learn finnish. One of the most effective
way to learn is to look for a recipe for ruisleipä (rye bread), look for keywords
and try to memorize them. You'll start to notice that these keywords repeats
a lot on texts of the same topic, thus understanding the media you're consuming
becomes easier.

# Realization

When I asked the usability question, a lot of ideas came into my mind, and my head started
to spin in circles! I have so many good ideas that I want to implement, but they
are so much my mind started to explode! When I started to feel this way, my decision
was to postpone this project until I get more technical experience to make it true,
but this wasn't the thing I was lacking at...

Everything started when I took software engineering at college. In my mind, it was
going to be boring and would have a lot of whiteboard masturbation.[^1] But then
I noticed some important tool to organize my mind and focus on what's the most important... **use cases!**

It's a tool for listing what the user needs on a system and a description on how it
will work. You generally extract them by what the user is telling their needs and
categorize them in order of importance and ease of development. In my case, the user
is myself, so I prepared the text and started to separate each use case based on
what I've written. It resulted in 27 use cases for now, but I think it will have
more when I start to plan it more.

When you're done, you start doing the first task that's the most important to
develop and the hardest one, the reason being to minimize risk already at the start.
It doesn't matter if it depends on another use case to be done in pesperctive of
software usability, because we don't want to use the system *yet*. We just want
to see if the hardest thing to program is actually possible and try to avoid
frustration at the start.

# Devlog

There wasn't so much development in code, but I took one day to separate the use
cases and categorize them. I don't want to spend time on UI, but on actual BLL code.
I still fear how to use this tool efficiently, because I don't want to make something
to use as a library, but when integrating with the presentation layer it's a disaster.

I don't want to go full OOP style with crazy UMLs and event diagrams everywhere.
I'm using this tool solely for sorting my mind and prioritize on important things,
since I praise more on data-oriented design than notebook object-oriented programming,
but I believe in big companies this will be the norm since OOP is predominant in the
industry and not inherently bad, but I would say it's misused in some cases (inheritance,
for example).

When I listed all the use cases, something became very clear. An system inside the
program crucial for the user, something so important it would be game changer in the
world of language-learning software, **a dictionary!**

No, it's not an ordinary one, but something specifically crafted for learners.
There will be a lot of use cases that will be using it, and will be responsible for
almost all functionalities that the program will have. Some of its features will
include:

- See word definition
- See categories for the word definition
- See relations with other words

It will also be used to track word expertise and give hints on how to improve
familiarity for related words, seeking a saner way find out what's important to
memorize.

# Final thoughts

In the next post, I will share a dictionary implementation specifically for language
learning. I will probably add more things but this is already a good start!

[^1]: This is a term when something is overplanned for the sake of it, and have littl
pragmatic vision on the things proposed. I'm a not a kink, please...
