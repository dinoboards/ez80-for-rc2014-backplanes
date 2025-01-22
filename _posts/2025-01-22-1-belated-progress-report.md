---
layout: post
title: Belated Progress Report
---

Ok, so its been quite a while since I provided an update for the *eZ80 For RC* module.  What can I say, I am easily distracted.

Despite this lack of communicating, I have nonetheless, still found some time to make progress on various ventures related to the *ez80 For RC* module.  There are quite a few things I am working on,
because, I get easily distracted.  I keep starting a little project, but then I think - oh - what about this 'other' thing.  (So unlike me!)

I will try and summarise all that I have been working on over the last few months:

## SRAM Memory Module

As per my last entry, I developed the Memory Module to give the system access to 2MB of linear addressable memory.  Nothing changed here, other than the development of software to run on this memory.

## CLang Compiler & runtime

To build applications for the system that can run in ADL mode on the memory module, I need a way to write such code easily and quickly.

The Options for writing code for the eZ80 (ADL):

1. Write in assembler.  If I write my code in assembler, I can target ADL mode, using the 24 bit logic of the CPU.  But its certainly a time consuming and error prone process.  Fine for time critical code sections and low level logic, but for general application development - its rather slow.

2. Write C code with Zilog's ZDS IDE.  Zilog's official C compiler can target the eZ80's ADL mode, allowing for full 24bit addressing and logic.  But the C compiler is an older version of C (C99), so has some nice features missing.  But more impactful, is the fact that its very much a Windows Application, tuned to the idea of firmware development, not general application development.

3. Z88DK.  The Z88DK system is an amazing cross-platform Z80 solution.  It can target the eZ80 CPU, unfortunately though, only for the standard Z80 mode.  So its a no-go for me as it can't produce eZ80 ADL code.

4. CLang Port.  The CLang port, developed for the TI-84 Plus CE/TI-83 calculator, and then later ported for the Agon, does indeed provide a modern C language compiler that can target the full ADL mode feature of the eZ80.  But....

There were some challenges with the CLang tool chain for use within the RCBus/RC2014 ecosystems.  The runtime for the *CE* calculator and the updated runtime for the *Agon* are developed for their respective Operating Systems.  The question arose, how to make it work on my CP/M based RCBus/RC2014 system?

I have done a fair bit to convert/port the Clang compiler to my platform - probably too much to cover in this posting - I will try and post a separate entry detailing all the work.

Suffice it to say, I now have a cross-compiler that I can run on my PC and build CP/M 'executables' that can be copied to my kit and run - taking full advantage of the eZ80 processor.

The repo for the tool chain is at: [https://github.com/dinoboards/ez80-clang](https://github.com/dinoboards/ez80-clang)

And within my main repo, I have a number of sample applications, that are compiled as ADL mode applications: [https://github.com/dinoboards/ez80-for-rc/tree/main/apps](https://github.com/dinoboards/ez80-for-rc/tree/main/apps)

## CH376 True USB Module

When I developed the [Yellow MSX Cassette+USB](https://www.dinoboards.com.au/cassette-and-usb) module for my MSX series, I also wrote the code required to fully access the USB protocol.  It is able to enumerate devices over USB Hubs, Mass Storage devices, USB Floppy drives, Printers, Keyboards, etc. All the code written for this USB logic, was developed using the Z88DK and SDCC C compiler.

I have wanted for a while now, to port this code into RomWBW to enable true USB support within the RCBus/RC2014 platform.

And I have done that.  The code is not yet official yet... But its ready for merging into Wayne Warthen RomWBW's repo.  I will submit it to Wayne, once he completes his current release's beta phase.  In the meantime, the code is available on my fork of RomWBW.

My patch of RomWBW with full USB support is available at: [https://github.com/dinoboards/RomWBW/tree/dean-ch376-usb-native-5](https://github.com/dinoboards/RomWBW/tree/dean-ch376-usb-native-5)

<figure style="text-align: center"><img style="width: 60%; height: 60%"  src="{{ site.baseurl }}/assets/images/ch376-usb-module-profile.jpg"></figure>

I will have this as a kit on my Tindie store shortly.

## BBCBasic port

So now that I have a way to assemble/compile application code for the system, I decided to have a go at porting the BBCBasic language.

BBCBasic started out on the Acorn BBC Computers.  A version for CP/M on the Z80 was written by R. T. Russell.  This was later ported for the Agon.  The Agon port included both changing the code to support its OS (MOS) and its graphics system.  Dean Belfield then continued this porting to enable BBCBasic to run in full ADL mode.

I took Dean's code and then effectively undid the MOS and graphic porting - converting it back to CP/M - but kept its ADL target.  So I now can type at my CP/M console:

```
EXE BBCBASIC
```

I can run the BBCBasic interpreter, with access to 2MB of RAM, running on CP/M.  At this stage I still have some code to write to enable it to access the various graphic capabilities of the V9958/TMS9918 video modules commonly available for the RCBus/RC2014 systems.

Code for my port can be found at: [https://github.com/dinoboards/ez80-for-rc-bbc-basic/tree/dean-converting-to-ez80-for-rc](https://github.com/dinoboards/ez80-for-rc-bbc-basic/tree/dean-converting-to-ez80-for-rc)

## Wolfenstein 3D port

This was another distraction.  I wanted to see if I could get Wolfenstein 3D running on the eZ80.  I now had a C compiler, and a  c-runtime that included some graphic support. So is it possible to run Wolf3d on my retro kit computer?

The original Wolfenstein for the x86 was developed for VGA graphics and the x86 segmented memory module (lots of far pointers, EMS, XMS memory).  There is a bit of x86 assembly for some of the critical routines.  It ran on 286/286 with frequency around 8 to 16Mhz.  Could the eZ80 running at 25 or even overclocked upto 40Mhz compete?

The 286/386 CPUs has some key advantages over the eZ80 - perhaps the primary one being is that they are not 8 bit processors!.  I am not sure of the full extent of the differences between the processors, but I suspect the eZ80 is missing quite a few features that might be critical for the performance needed to run Wolfenstien.

At first I had to convert some of the graphic operations that were tuned to a resolution of 320x200 to my V9958 VDP module's 256x212.

I have tried to take advantage of the eZ80's 24 bit registers/operations, and begun to convert some of the critical functions to assembly.

I have no idea if I can make this a truly 'playable' version - But when I first started, with no optimisation, I was able to get the demos playing at about 1 FPS.  After some optimisation, got it upto 4FPS.  And I know there is still a lot of optimisation potential.  Perhaps I can do it, if I dont get distracted again!

The code for this port can be found at: [https://github.com/dinoboards/ez80-for-rc/tree/dean/wolf3d/apps/wolf3d](https://github.com/dinoboards/ez80-for-rc/tree/dean/wolf3d/apps/wolf3d)

> I had not based my port on the very original Wolf3D code.  Instead I used the 'NakedWolf3D' port. The original C code has a lot of segmented memory access (lots of near and far pointers) and some x86 assembly.  The NakedWolf3D is all C code - but targeting SDL for sound, control and graphics.  But its an easier place to start for the porting.

<figure style="text-align: center"><img style="width: 50%; height: 50%"  src="{{ site.baseurl }}/assets/images/wolf3d.jpg"></figure>

## HDMI V9958 Compatible FPGA Module

A module I had developed quite a while ago, then kind of abandon, was to the use the Tang Nano FPGA module to fully emulate the V9958 VDP module.  This kit is now, I think, done.  There may be some minor compatibility issues with the emulation of the VDP, but for the most part, all code targeted for the V9958/V9938 and TMS9918 just work.

A couple of key advantages of this module over the original is:

* Its output is HDMI - so much easier to connect to modern displays - with a nice crisp image.
* It a lot faster than the original chip - so my Wolf3d conversion is a little more ... possible.

<figure style="text-align: center"><img style="width: 50%; height: 50%"  src="{{ site.baseurl }}/assets/images/hdmi-module.jpg"></figure>

## Z80 all the way down

With the HDMI V9958 Module mentioned above, it raises that 'discussion' about retro.  Is it retro if its not original hardware?  Is emulation as good as the original?  I can run pretty much any Z80 based system in my modern PC's web browser.  Javascript can emulate Z80 code faster than original hardware could ever hope to achieve.  This is an indication of how things have changed.

Rather than 'porting' code, to solve the 'compatibility' issues with running original Z80 code, could the eZ80 emulate a Z80?.  Can I get the eZ80 to work on my Yellow MSX system, running MSX code?  The emulator would need to map the I/O appropriately (as per the porting work done for RomWBW).  Would it be fast enough?  Can an overclocked eZ80 running at around 30Mhz emulate sufficiently a standard Z80 running at 4Mhz?

This is just an idea for the moment.

## DRAM Memory Module

Another idea, that I have also done some exploration with, is to try and develop a 32 pin DIMM DRAM interface module for the ez80.  Can I provide the eZ80 with access to a full 16Mb of dynamic RAM DIMM?  This would be cool if I can figure it out - and find the time - and not get distracted again.

<figure style="text-align: center"><img style="width: 50%; height: 50%"  src="{{ site.baseurl }}/assets/images/32-pin-dimm.jpg"></figure>


