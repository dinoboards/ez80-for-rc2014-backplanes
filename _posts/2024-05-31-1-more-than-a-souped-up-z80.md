---
layout: post
title:  "The eZ80 is more than a souped up Z80"
---

<p>The eZ80 is kind of compatible with the Z80, but has a number of key differences that need to be considered for this to work:<br></p>

<ol><li>The eZ80 works on 3.3V.&nbsp; All the eZ80 inputs are 5V tolerant for any inputs.</li><li>The eZ80 works at a much higher rate, so could easily attempt to interface with other hardware and memory far faster than they could expect to handle.</li><li>Many of the control signals for the eZ80 are mappable to Z80 signals (such as RD, WR, NMI, HALT, DATA and at least the first 16 address lines), but some are not going to work 'out-of-the-box'</li><li>The eZ80 instructions are backwards compatible with the Z80, but key differences arise with the processing of interrupts and the handling the larger address range.</li><li>The eZ80 instructions can operate in 2 modes.&nbsp; The Z80 'backwards' compatible mode, where instructions are scoped to a 64K segment of the full 16MB range.&nbsp; And the full mode, where the instructions can access the full 24-but 16MB directly.</li><li>It has a lot of GPIO pins for general use, but some are multiplexed with specific functions, such as the UART.</li></ol>
