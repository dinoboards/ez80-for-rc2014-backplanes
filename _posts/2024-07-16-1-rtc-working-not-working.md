---
layout: post
title: RTC Working Not Working
---

Well this was an interesting day.  Spent much of it getting the Real Time Clock working.  Created code for the on-chip firmware to initialise and configure the on-chip RTC.  Added a RTC driver in my RomWBW branch to interface with it.  All working perfectly.

My interface module only provides a small 2 pin connector for connecting an external 3V power source.  I hacked up a connection to a 3V button battery.  (The current PCB design does not have room for a battery holder).

At the end of the day - after getting it all working, I thought I would try to power the RTC with the main 3.3V power on the board.  I jumpered the main 3.3V line to the RTC power input.  But I think I might have over-volt'ed the CPU - as no matter what I did thereafter, the RTC would not work anymore.  Inspected the clock/crystal with the oscilloscope, and measured all 0V - no oscillations!

The rest of the chip worked fine.

I had a closer look at how I traced the power lines for the RTC, and realised that it was a direct connection.  I think at least I should have had a decoupler capacitor.  Looking at the reference design for the eZ80 dev board, it looks like they also have a 100 ohm resistor between the main 3V3 line and the RTC power input.  Hmm live and learn!

I will redesign the PCB - I think I can fit a small battery holder on the PCB - and just power it directly from a battery always.

I did try and replace the crystal, in the hope that maybe it was the fault - but alas - no luck.  I will have to soldered up another eZ80.  Time for some squinting to hand solder those tiny pins!
