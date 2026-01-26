---
title: Discovery Through Failure
description: How an abandoned Arduino experiment became the foundation for a serious engineering project.
author: cody_angeline
date: 2026-01-02 00:00:00 -0700
categories: [Projects, Embedded Systems, Oscilloscope, Story]
tags: [compose-ui, oscilloscope, lvgl, arduino, smt32, teensy, ui, embedded c++, domain-driven design]
pin: true
math: true
mermaid: true
image:
  path: /assets/img/posts/mcu_screen.jpeg
  alt: "An STM32, ATmega2560 (Mega-compatible), Arduino Uno, and Tinyduino (FPGA) surrounding a Makeronics breadboard. Center: Adafruit-HX8357D display driven by a Teensy 4.1."
---

### Every blank page fights back.

Green-field development is rewarding because it is difficult. In my mind, any problem worth solving demands a bit of pioneering spirit — a willingness to walk into a domain with no map and sketch one as you go. You start with a few assumptions, gather enough information to turn those assumptions into doodles, and then implement just enough to watch them crystallize into structure — familiar in shape, but never exactly what you imagined. That moment, when the idea stops being hypothetical and starts pushing back, is where the real engineering begins.

And that is exactly what happened when I returned to the oscilloscope. The moment I committed to building it for real, the project stopped being a daydream and started behaving like a domain with opinions. Embedded constraints and UI demands surfaced one after another — the level bosses of the path I had chosen — each one forcing me to refine the model I thought I understood. What I had in my head was a clean, contract-driven constellation of subsystems. What I had on the bench was a handful of boards, a display, and a long list of unanswered questions.

### Iteration 1: A functional but chaotic prototype.

The first iteration of the project was written entirely as an Arduino sketch. I leaned on Adafruit’s built in libraries and a kind of domain specific, function driven style — not the functional paradigm, but the colloquial pattern you fall into when state decides which functions run. It was rapid, almost frictionless, and I had graticules on the screen in about three days. As a proof of concept, I was delighted. It was pure programming — the kind of work that feels like revisiting algebra after weeks of calculus, a quick return to fundamentals that rewards you immediately.

After my buzz wore off, it was obvious that the first real enhancement would be moving to OOP, and the graticules were the low-hanging fruit. One of the nice things about Arduino sketches is that they are ultimately just C++ under the hood — a thin adapter, some conveniences, and a bit of branding on top of a library. Which meant the next step was unavoidable: I needed to learn C++. So, I grabbed the one pedagogical C++ book I had on my shelf and let [`Brian Overland`](https://books.google.com/books/about/C++_Without_Fear.html?id=4IQHCwAAQBAJ) walk me through my initiation.

### Iteration 2: OOP, LVGL, and the illusion of progress.

Once you learn a paradigm, every implementation of it tends to look the same — emphasis on look. That mantra held true right up until I met the Pratt clockwise spiral model, raw pointers, and the whole headers versus .cpp ritual — my first real introduction to the C++ kitchen-sink house of pain. But I cracked on, and before I knew it, I had written my first C++ program in OOP; Graticules lives.

Then reality tapped me on the shoulder. Without layers, all I really had was a 2D drawing space — and how exactly was I supposed to render a waveform on that? I could invent some method of dynamically redrawing the screen for every point a waveform streamed across, but that sounded miserable. Surely someone had solved this already.

Oh hello, LVGL.

### Iteration 3: Wait, what do you mean I can't do that?

One constraint still needed to be dealt with: the Arduino’s sluggish processor and tiny memory footprint. After a bit of digging, the Teensy emerged as the clear winner — best bang for the buck. Into the cart it went. Alright, great - I had discovered fire — or so I thought.

Before touching rendering, I decided to clean things up: create a wrapper around the driver, define an abstract Display class, and inject the driver into it. main() would act as orchestration, and I’d introduce a Screen class to wrap LVGL and initialize it with a Display through the same DI pattern. A few initializers, and voilà. See? That was not so hard.

Everything was wired up. It compiled on the first try. And then I uploaded it. Uhhh… why do these graticules look like they are wrapping over themselves? Time to add some debug. Validate the Graticules dimensions [check]. Validate the Display dimensions [check]. Validate that LVGL created the widget and that its size matches Graticules [check]. Validate that the active LVGL display matches our Display object [check]. The math checks out. The LVGL sniff tests pass. So, what on earth is going on?

After some research, to my dismay I discover the real constraints: embedded developers treat heap allocation as a last resort, virtual tables are frowned upon because they consume flash, and runtime polymorphism — the backbone of the dependency injection pattern I had just built — is basically off-limits. Wonderful. I am really starting to miss having an operating system.

I clearly needed some new mentors, and I was delighted to find
[`Stroustrup`](https://books.google.com/books/about/The_C++_Programming_Language.html?id=LqRVAAAAMAAJ),
and [`Meyers`](https://www.google.com/books/edition/Effective_Modern_C++/ZDhIBQAAQBAJ?hl=en&gbpv=0).
After a bit of light reading and a couple of months of redesign, I finally had a model that would cure my LVGL heartburn. My codebase joyfully welcomed templates and namespaces. main() kept its role as the orchestrator and gained a few new companions: a Registry to hold widget attributes, a Builder that configured widgets and carried state, and a Pool to store the configured widgets that Screen would consume.

With this new compile time polymorphic approach, ComposeUI took shape — and for a brief, shining moment, it felt like all my problems had been solved.

### Iteration 4: That medicine did not make the pain go away.

The graticules were still overlapping, and at this point the architecture was fully embedded-friendly. So, what was I missing? Ah — another intricacy. I had not considered that the display driver and LVGL might not even agree on the same coordinate system.

To understand what was happening, I had to step back and think about the data flow. LVGL calls a flush callback on the display driver, and that callback writes pixels from the buffer LVGL produces when it runs my graticules’ render function. If the two systems do not share the same notion of origin or orientation, the output will look correct in one model and completely wrong in the other.

So, I built a test. First, I made the display driver mimic exactly how it writes pixels when LVGL calls it. Then I added visual markers so I could see where the display thought its origin was. After that, I added a method that wrote pixels in the same spots using LVGL. Once the display was faithfully mimicking the callback, its output looked correct. Time to see what LVGL was doing.

And that’s when things got interesting. LVGL’s orientation matched the display’s — so the coordinate system was not the culprit. But LVGL was drawing the origin pixel multiple times, marching it down the screen until the bottom left and right pixels were drawn. Why?

Ah. LVGL uses clipping regions when it draws. The region does not update until new data enters the buffer, so the same pixel gets redrawn across the entire clipped area. And to make things even more exciting, the opacity of the active screen matters too — if the screen is opaque, you’ll see every layer LVGL draws, not just the final one. Once I fixed that, it was truly voilà; Graticules lives.

### The End: For now...

In the end, a bug that was not a bug at all pushed me to build something that did not exist in the LVGL world: a declarative framework designed specifically for embedded systems. Funnily enough, the misconfiguration saved me from the unpredictable UI behavior I would have faced later, thanks to my old operating systems developer habits.

And for now, [`ComposeUI`](/Projects/) is complete enough that its presence in my portfolio feels genuinely novel. With any luck, its utility will spare someone else the trail I had to blaze.
