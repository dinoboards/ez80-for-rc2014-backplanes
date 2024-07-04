---
layout: post
title: Issue with Data bus Buffering
---

As mentioned in a previous post [HBIOS booted on the eZ80]({% link _posts/2024-06-02-hbios-booted-on-the-ez80.md %}), there was an issue with the Memory Module - where it would only work if I used the 74HC670 chips (instead of the similar 74HCT670).  I think the problem for this might have to do with a problem I had discovered with the way I was buffering the Data lines (D0-D7) between the eZ80 and the backplane.

I noticed that I was 'disabling' the buffer chip when the eZ80's `RD` or `WR` was not active.  This is certainly not right.

> The memory module is a bank memory system.  It uses the 74HCx670 chips to manage the banking, allowing a normal Z80 to address more than 64K.  The 74HCx670 'latch' or store a specific higher address value for each of the 4 banks within the 64K range.  To assign a specific value, the processor needs to do an I/O write to the 670's.

As the 74HCx670 will latch the data lines as the `WR` signal goes from low to high, it would be trying to read data lines that are at the same time, transitioning to a high-impedance state (floating in-active).  Thus preventing the correct transfer of the data.

I have fix this, so that the buffered data lines are stable after the `WR` goes high, as required by the eZ80 timing.

I have yet to re-test the Memory Module, using the 74HCT670 variants - Will try and confirm this soon.

