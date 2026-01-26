---
title: From Firehose to Patch Panel
description: How a network diagram became a rack-mounted project.
author: cody_angeline
date: 2026-01-24 00:00:00 -0700
categories: [Projects, Networks, Story]
tags: [netgear, ubiquiti, design]
pin: true
math: true
mermaid: true
image:
  path: /assets/img/posts/network_rack.jpeg
  alt: "The keystone patch panel and Netgear GS716T in my 70U office rack."
---

### An Ode to My Pain

My major in college was broad in a way that only an academic committee could love. It was an ambitious fusion of Computer Engineering, Computer Science, Software Engineering, and IT — four disciplines compressed into a single degree program that looked impressive on a syllabus and felt like a weekly endurance test in practice. Twelve hours of study a week was often the minimum just to walk away with a B.

That is not to say it was incoherent. Plenty of courses resonated — architecture, operating systems, and even the electives I took purely because I enjoyed them. I was the odd student who picked algebra on purpose. But networking had a different energy. It was not easier, just... cleaner. The abstractions lined up with the physical world in a way the rest of the curriculum rarely did. Building fictional topologies for an imaginary university felt like stepping into a domain with its own quiet logic.

### A Light in the Darkness

For all the chaos, the curriculum never failed to spark curiosity. The only real salve for the weekly academic beatings was discovering authors in the coursework who felt like future mentors. Somewhere along the way I developed a strange hobby — I started building a library. Every time I survived another crucible, I added a book to a growing repository I could return to later, and it has served me well.

But not everything went on a shelf. Some courses left me with hardware instead of hardcovers. Networking was one of them. What began as a fictional campus topology in a classroom slowly turned into a small lab in my home office — a little cluster of gear I imagined would one day resemble a node in the network I had designed on paper.

### Blinking Lights, False Confidence

Once I thought I knew what I would need to finally ditch the monthly rental fee for my ISP's all-in-one router, I did what any overconfident student with a credit card and a half-formed understanding of networking would do: I started buying hardware. I committed to a mostly-Netgear LAN and sourced a Netgear CM100v2 modem, a Ubiquiti EdgeRouter, a Netgear GS716T Layer 3 switch, and a Netgear VLAN-capable WAP.

I pulled the cable line into my office, ran Cat5e through every room, mounted everything neatly on the rack, and stepped back to admire my work. It looked like a real network — blinking lights, tidy cabling, the whole thing humming with purpose. 

I was ready to rock... right?

### School of Hard Knocks

"I know how to do that — I took a class once."  
Famous first words of a hard lesson in the making — at least that has been my experience. VLANs felt like a sensible a place to begin, so I pulled up the manual and began sketching a topology that felt reasonable. Once the segments were named and the CIDRs assigned, it was time to bring the diagram to life. What I wanted seemed simple enough: four VLANs, each with a clear purpose, and one dedicated maintenance port. 

So naturally, I started with the maintenance port; easy. Create the VLANs on one setting page, configure port membership on another, remove the default VLAN from the maintenance port and... bricked. Locked myself out. Well, that is why they put factory-reset buttons on these things. Fine. Reset. Try again.

Create the VLANs, set the IP addresses, configure the port membership, remove the default VLAN from the maintenance port and... bricked. Again. And again. Whoever says documentation writers have it easy has clearly never tried to follow a vendor's idea of "clear instructions." At this point I was starting to understand why seasoned network engineers swear by console access.

And just as I reached for that idea, Netgear swatted my hand away — apparently some models do not support console access at all.

Gods be kind — I was losing my mind.

### Post-Boss-Fight Fatigue

The deed was done. I had conquered Bowser and saved the princess. The switch was VLAN'd up, the trunk was set, everything was ready to jam. But it had already been more than a couple of weekends, and I had other things I probably should have been doing. I would be lying if I said this had been fun anyway.

So, I gave it a little pat — metaphorically of course — and told it I would be back. When I had time. When life slowed down. "Go to sleep for now," I thought. "I'll be right back."

But like all projects without immediate utility, it stayed on the rack. Days became months, months became years, and the switch waited — quietly, patiently — until the need for a network hero finally returned.