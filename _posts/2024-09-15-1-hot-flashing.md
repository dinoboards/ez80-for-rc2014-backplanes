---
layout: post
title: Hot Flashing
---

Been working on understanding and writing code to re-flash the on-chip ROM from a CP/M application.

I have written an application that allows for the 'updating' of the on-chip ROM without the need for the Zilog Programmer.

Designed a system, where I can install dual firmwares.  A default/fallback bootable firmware and an 'alternative' firmware.

The first firmware is flashed, using the Zilog programmer, within the first 64K page of the 128K on-chip ROM.

The CP/M application `FWUPDATE.COM` can then be executed within CP/M, to flash an 'alternative' firmware in the 2nd 64K page.

On reboot, the system sees this new 'alternative' image and boots using that version.  Should this new 'alt' firmware fail to launch HBIOS/CPM successfully, the main boot loader will switch back to the main 'firmware'.

<figure style="text-align: center"><img style="width: 60%; height: 60%"  src="{{ site.baseurl }}/assets/images/flashing.png"></figure>

> A future goal will be to develop, using something like a PI Pico, to create my own external programmer.  A Pi Pico, or other similar cheap microcontroller, will be a lot cheaper than the Zilog Programmer.
