---
title: The Oscilloscope I Never Bought
description: The origin story behind my oscilloscope project and the journey that eventually led to building ComposeUI.
author: cody_angeline
date: 2026-01-01 00:00:00 -0700
categories: [Projects, Embedded Systems, Oscilloscope, Story]
tags: [compose-ui, oscilloscope, lvgl, arduino, smt32, teensy, ui, embedded c++, domain-driven design] 
pin: true
math: true
mermaid: true
image:
  path: /assets/img/posts/emi_notes.jpeg
  alt: EMC reference notes, calculator, and electrodynamics book from my time at the U.S. Army Electronic Proving Grounds.
---

### Some tools you buy. Some tools you build.

![Desktop View](/assets/img/posts/emi_patch.jpg){: width="160" height="160" .rounded .left}
In my twenties, while studying computer science at my local community college, I worked at the U.S. Army’s Electronic Proving Grounds on Fort Huachuca in Arizona, performing environmental and electromagnetic compliance testing on military systems. Many days were spent automating measurement devices, amplifiers, and signal generators in the control rooms of anechoic chambers, or setting up equipment beside temperature and vibration chambers.

Of all the tools I worked with, the oscilloscope was my favorite — not because it was simple or friendly, but because it demanded real understanding. It is easy to misconfigure, easy to misread, and perfectly capable of giving you answers that make no sense unless you understand the underlying network and the measurement system itself. It can reveal the behavior of a system, but only if you approach it with precision, curiosity, and a solid mental model.

Naturally, I started looking at buying one of my own, but they were expensive, and keeping a used scope calibrated would have been its own project. So, the idea of buying commercial off the shelf really lost its luster quickly. And once the “just buy one” path closed, the more interesting question surfaced: why not just build one?

I had tinkered with Arduinos for years, so when the idea of building my own oscilloscope crossed my mind, I already had boards lying around. I figured I would start with the presentation layer because it was the one domain I firmly understood, and in my mind, it lived as its own subsystem — it did not need to know anything about sampling or buffering, that is a separate subsystem.

So, I picked up an Adafruit HX8357D display and got to work. I wired everything up, ran the Adafruit examples, and experimented with driving the display with Arduino sketch. It was early, exploratory work — just getting a feel for the domain. But life got in the way, as it tends to do, and the project drifted onto the back burner.

Years later, after moving through a couple of organizations, I realized I needed to step out of test and evaluation and fully commit to software engineering. That meant building a few technically serious portfolio pieces, and the first choice was obvious. I would build the oscilloscope I never bought. What followed was a series of hard lessons in embedded constraints, C++, and UI architecture.
