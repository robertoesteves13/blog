---
title: "Muista Development - Part 0"
date: 2023-07-11T14:20:15-03:00
---

I've been learning finnish since 3 years ago, starting with language learning apps
(specially the green owl one), Youtube videos and also TV shows from the country,
at which point I decided to get enrolled ina course where the embassy of Finland
in Brazil recommended (always send your questions to the local embassies of a
country that you have interested to learn their language, they are generally
receptive and tries their best to answer your questions!).

However, I find very difficulty to get familiarity with words in the language,
despite getting exposure to them a lot of times. I tried to use anki, a really
good flashcard software used a lot to memorize new words of a language, but the
biggest flaw I believe is that you only memorize the word at it's own, not the
meaning that it has on a certain text. That becomes a lot more obvious with sentences
like **Kuusi Palaa**, that can mean:

- The spruce is on fire
- The spruce returns
- The number six is on fire
- The number six returns
- Six of them are on fire
- Six of them return
- Your moon is on fire
- Your moon returns
- Six pieces

It's just a lot of things to memorize for a single sentence, and it's not this
particular example! Finnish has a lot of single words with multiple meanings that
depends on the context. In short, using anki to learn languages isn't very
effective for finnish.

That's why I had an idea, what if I create a program where the user can put any
text inside of it, then it can mark words that he wants to memorize (also annotating
other things like the normal form, type, etc...) and later it has a quiz where
it shows the word, the additional annotations and the whole sentence where this
word was marked initially?

That's why I want to create a new program called **Muista**! I had a lot of attempts
of trying to create this type of program, but I always struggled to conform with
user experience, developer experience and what I believe it's the best for the
desktop world.

I'm opposed to the modern way to do desktop development, which are only an instance
of Chromium/Webkit running an HTML page with CSS and ECMAscript. It might be good
for UI and security, but it becomes a burden when you want to interact with the
underlying system below it. When I tried tauri, using something like SQLite was
painful because it made me code the same data model *twice*, and the IPC communication
via tauri commands results in **frequent Serialization/Deserialization of JSON**, making
the program much slower if you try to stream megabytes of data.

But I also wanted to be cross-platform! So a lot of frameworks like GTK (yes, I
know you can use it on Windows and MacOS, but it leaves a lot to be desired on
these platforms), WPF and others aren't suitable. After lots of time choosing,
I decided to use the same GUI library that anki also uses: **Qt**!

It has good support on most operating systems, it's very mature and have the performance I desire
for my program. A lot of people says it's very bloated, but even it being a fair
criticsm, it's what I also liked about it: it has SQL support and a lot of widgets
to make complex interfaces. I believe it's going to be a good library to use for
this case.

I have a lot of work to do, specially designing and prototyping it. 
