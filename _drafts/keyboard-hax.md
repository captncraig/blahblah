---
layout: post
title:  "TeleTypewriter Part 1: Mapping the Membrane"
date:   2018-03-06 22:17:22 -0500
tags: hardware hacking
categories: blog
series: typo
---

There are maybe five creations I have wanted to build since I can remember. My "forever projects". 
I want to build a full CPU out of [7400 series](https://en.wikipedia.org/wiki/7400_series) logic chips with brainfuck as its' ISA. 
I want to build a complete mechanical replica of an enigma machine. 
I want a typewriter that can type by itself, I want to make a [piet](http://www.dangermouse.net/esoteric/piet.html) interpreter in piet, and I want to make my own [sci](https://en.wikipedia.org/wiki/List_of_Sierra%27s_Creative_Interpreter_games#List_of_Games) interpreter.

Some of these have been done before, and none are particularly useful, but that is not really the point, is it? I will sometimes go months or years without thinking about these goals, but will invariably dig one out and be infatuated with the idea for some period of time, solve some new aspect of the puzzle (sometimes only in my head), and probably file it back into my imagination. I don't think actually finishing them is my primary goal necessarily, but they certainly make me happy to think about. 

Well I'm back in the saddle on one of 'em, and I'm gonna make it happen. All thanks to this beauty I managed to salvage:

img 0

That beauty is a Brother GX-6750 electronic typewriter, manufactured sometime in the mid 90's. I was able to grab it for free in good working condition, and I couldn't be more thrilled. 

### So what am I going to do with it?

This is gonna be a rather long series. I'm going to document my process, and the things I learn along the way. I have a bunch of cool ideas, and I'll get into specific applications later, but they all rely on two main capabilities:

1. I need to be able to read what keys are typed.
2. I need to be able to simulate keypresses.

If I can do both of these things independently, I can do anything. If I can make an interface unit that arbitrates between the keyboard and the motherboard of this typewriter, my controller can read, intercept, and even create new key presses. In essence, I am making a "smart keyboard" to plug into the typewriter, so that I can program it as I want.

### So what is a keyboard anyway

This unit doesn't have just **any** keyboard. According to the sales brochure it has a "Perfectype® professional touch keyboard". Sounds fancy. Let's take it apart:

img1 (not exactly cherry mxs)

Under the keys we find this cool transparent membrane thing. The orange plunger things are kinda springy. I assume that's where the professional touch comes from. 

The whole thing is a single circuit. The ribbon on the end has 16 conductors, and plugs directly into the main pcb. What circuit? If its anything like other keyboards I've seen, those 16 lines will be 8 scan lines that get pulled low one at a time, and 8 signal lines that get connected in various combinations depending on which scan line is active and which keys are pressed. How do we know what is what though? 

One method would be to hook up 16 wires and write some code to discover the connections with a bunch of trial and error. Or maybe we observe the lines as the typewriter is driving them, and again do a bunch of trial and error. Or...

### Let's just trace it!

<style>
    object {
        width: 100%;
    }

</style>

The cool thing about clear circuits is that you can look at them. With a little bit of vector grease, I can colorize it to show exactly where each line goes:

<img src='{{"/assets/membrane.png" | absolute_url}}'>