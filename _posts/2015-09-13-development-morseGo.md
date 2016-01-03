---
layout: post
title:  "morseGo"
date:   2015-09-13
categories: development
tag: golang, project
---

# Morse Code Golang CLI

## Simple little command line converting Morse Code written in Go

# What Morse Code?

## [Morse Code](https://en.wikipedia.org/wiki/Morse_code)

# How to use it

## This is able to convert from words to morse code, or convert from morse code to words.
```
./morseGo -h
Usage of ./morseGo:
  -atm
        To need Alphabet words ex) -atm 'a b'
  -mta
        To need morse codes for alphabet ex) -mta '. .-  . .-'


$ ./morseGo -atm "a b"
.-  -...

$ ./morseGo -mta ".-  -..."
a b
```

# Where is it?
![morseGo]({{ site.url }}/assets/icon/gitHub.png)[morseGo](https://github.com/hellworld-io/morseGo)
