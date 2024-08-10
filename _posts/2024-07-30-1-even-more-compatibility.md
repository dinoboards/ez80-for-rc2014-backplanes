---
layout: post
title: Even more compatibility
---

In my previous post, I looked at how to improve compatibility with modules where the write logic is triggered on the positive edge of the `WR` signal.  This is important for Modules such at the Compact Flash and banking Memory Modules.  But I have since found I introduced an issue with the next module I attempted to test with; the [SN76489 sound module by J.B. Langston](https://github.com/jblang/SN76489).

This is a retro sound module based on the SN76489 sound chip.  Its a fairly simple chip and given its age, its no surprise its a chip that can not be sent data, as quickly as the eZ80 is capable of achieving.

This chip only accepts writes - it does not allow its internal registers to be read.  So the issue is only related to timing of data writes (eZ80 to SN76489).

It transpired, that one thing I forgot to consider, was the eZ80 `WAIT` input signal.  This is a signal that the eZ80 (just like the Z80), can receive from 'slow' chips to cause the eZ80 to extend the writing (or reading) cycles.  The SN76489 module connects the `READY` output to the CPU's `WAIT` signal for just such a purpose.  The logic I implemented in the ATF16V8 PLD did not take the `WAIT` signal into account.  As such, it would deactivate the `BUS-WR` signal well before the SN76489 had time to load the data into its internal registers.

A quick bodge on the current PCB and an update to the PLD code, and suddenly I hear retro 8-bit sound coming from this old sound chip.

> I did have to change the software a bit, to ensure the I/O is sent to the correct port still and also introduce a small delay between writes.  All this code is in my branch of the RomWBW code.

Basically now, the logic is, if the `WAIT` signal is activated during a write cycle, the `BUS-WR` is not pre-released and is logically the same as the eZ80's `WR` signal.

Once I got the SN76489 sound chip working, I found the next module I tested, the [RC2014 YM/AY Sound Card](https://www.tindie.com/products/semachthemonkey/ym2149-sound-card-for-rc2014-retro-computer/) worked without any more hardware modifications.  Only required me to update the software again to ensure correct I/O addressing.

With the auto selection of the correct eZ80 bus cycles for I/O operations, I was able to play 8-bit chip-tunes with the `TUNE.COM` CP/M app, with the eZ80 overclocked all the way up to 32Mhz!
