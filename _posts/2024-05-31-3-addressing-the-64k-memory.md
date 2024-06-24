---
layout: post
title:  "Addressing the 64K memory"
---

​The eZ80 has 4 configurable Chip Select signals (CS0 to CS3), in addition to the MREQ and IOREQ signals. The chip-select signals can be configured to activate for range of memory or IO requests.

As the MREQ and IOREQ for the eZ80 assume a much larger address range than the RC2014™ modules will support, (24bit for memory and 16bit for IO), I need to utilise the CSx signals to provide compatible signals for the backplane.

For memory, I intend to configure CS3 to activate for a 64K memory range within the 16MB range of the eZ80.  In other words CS3 becomes the equivalent of the Z80's MREQ signal.

In the configuration for this chip-select, I can also specify an appropriate 'addressing' mode and wait state delays.  This should enable the 'slower' memory to work.

So the CS3, will need to be configured to activate for address range, say, 0xB9 0000 to 0xB9 FFFF.  CS3 will be wired, via the 74HCT245 buffers, to the backplane's MREQ line.

Now, when the eZ80 addresses any memory within the address range 0xB9XXXX, it will be directed to the backplane's installed memory module.

With such a configuration, the eZ80 can be configured to execute instruction in it's 64K Z80 compatible mode, where the upper 8 bits of the address is set with the use of the special register (MBASE).
