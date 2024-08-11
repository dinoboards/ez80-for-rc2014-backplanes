---
layout: post
title: Power on reset
---

One issue I hit when I was attempting to port the V9958 driver code (TMS9918 compatible driver), was an apparent intermittent fault.  After a few frustrating hours, I figured out that the V9958 was not being 'reset' on power up - and could not be initialised correctly by the CPU.

I think the V9958 is very fussy on requiring that its held in RESET during power-up.  If not, it will be in a state that does not seem to be correctable in software.

I realised that I forgotten to include a Power-On-Reset circuit in my design.

A quick work around is to press the RESET button on the backplane after initial powering on the kit.


> The original RC2014 Dual clock module does include a Power-On-Reset circuit, but this module is not required for the eZ80 Interface module as it has its own high speed clock on board.

I have drafted a new PCB design that includes a Power-on-Reset facility; this should ensure the whole system is held in RESET state for a few milliseconds when its initially powered on, and allow the V9958 to initialise itself correctly.

