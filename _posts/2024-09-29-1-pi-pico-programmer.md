---
layout: post
title: PI Pico Programmer
---

This weekend, I made significant progress in making a PI Pico module connect to the eZ80.  Enabling the ability to erase and write to the on-chip flash ROM.  This will be an alternative and cheaper way to program the eZ80 compared to Zilog's Smart Programmer.

The Pi Pico Programmer connects to the eZ80's ZDI interface, a 2 pin (similar to i2c) serial interface.  The Programmer can transmit commands to halt, inspect, debug, erase and write to the on-chip 128K Flash ROM.

Getting it operational was quite challenging.  My first challenge, that took way longer than I had expected, was to install the compiler/build tool chain for the Pico.  Once that was done, I had the ability to write C code for the Pico that can drive some GPIO pins.  So it was just a matter of discovering the ZDI signalling protocol to command the eZ80 to erase and write to its flash (and do a few other operational things)

This proprietary interface is a bit like I2C, but not quite...  Its documented by Zilog in the eZ80 manual.  I did find though, that the documentation is sometimes a little brief or confusing.

The development process mostly entailed me flashing some code on the Pi Pico - verifying that the GPIOs are toggling as expected - then connecting it for real to my eZ80 and see if that's actually what Zilog wants!  A lot of head scratching and trying over and over.


The _Pi Pico Programmer_ is now able to to program/flash the eZ80 - it still needs some polish - but is sufficient for now.

I intend to detail the solution, with appropriate pictures in the main repo.  I have also designed a small PCB to mount the Pi Pico and a 6 pin header.  Should make for a very neat solution.

> I am not the first to use a cheap modern Microcontroller to program an eZ80.  There are some projects for the Agon using its onboard ESP32.
