---
layout: post
title: Not all IOs are the same
---

<p>Unlike memory, IO will be a bit more tricky.</p>

<p>If I were to run any existing Z80 software on the eZ80, I need to first configure the EZ80 to switch to its Z80 compatible mode.&nbsp; As described in the previous journal entry, this mode allows the original Z80 instructions to execute within a 16 bit address range, where the top 8 bit address value is held in the special register MBASE.&nbsp; So for the most part, code will operate as intended.</p>


<p>But this is not as simple for IO instructions.&nbsp; In the eZ80&nbsp;the IO instructions will apply a 16bit value to the address bus, allowing for a full 64K addressing range for IO.&nbsp; (<em>The eZ80&nbsp;does not use the full 24bit for IO access).</em></p>


<p>Technically the Z80 also addresses its IO using a 16 bit address.&nbsp; But most designs just ignored the upper 8 bits of the address. &nbsp;For RC2014&trade; and like systems, modules are typically designed to consider only the lower 8 bit of the address, giving us a maximum of 256 IO port addresses.&nbsp; Software written will expect this to be the case, and so does not 'worry' about what value it places in the upper 8 bits.</p>


<p>This mean, existing software might address port 0x45 with a 'random' or undefined value for the upper 8 bits.&nbsp; For example: it might address it as 0x1245 and then later on address it as 0xE445.&nbsp; As the hardware ignores the upper byte, it all just works as expected.</p>


<p>Does this not means that the eZ80&nbsp;and Z80 address IO in the same way?</p>


<p>Yes, but there is a problem that will need to be considered.&nbsp; The eZ80&nbsp;has many on-chip systems, UARTS, timers, and other system.&nbsp; These all have associated IO ports to control them.&nbsp; Zilog has assigned them within its reserved address range of 0x0080 to 0x00FF.</p>


<p>That means if any existing software designed for the original Z80, makes an attempt to write to some port, say at address 0x9F, it will place a random/undefined value on the upper 8 bits of the address. If this happens to be 0x00, then it will not access the device it intends, but rather the internal system port.</p>


<p>Any attempt to run existing code designed for the Z80 in the eZ80 in compatibility mode, may still need be modified to avoid port conflict issues.</p>


<p>The other issue is the timing/wait states applied to IO access.&nbsp; If no wait states are applied, then the IO access will be way to fast.&nbsp; To enable some wait states, I need to configure a chip-select line, (similar to how memory is done).&nbsp; <em>(This could perhaps be done off chip, but requires more external chips - I'd rather try and use the inbuilt features of the eZ80.)</em></p>


<p>Similar to how CS3 will be wired to the backplane's MREQ signal, I intend to wire the CS2 signal to the backplane's IOREQ line.&nbsp; The appropriate wait states and address mode (Z80) can then be configured.</p>


<p>CS2 will be configured to activate, whenever the CPU is instructed to address IO ports in the range of 0xFF00 to 0xFFFF.&nbsp; This will mean than existing software that address IO ports, will need to be modified to ensure it sets the upper 8 bit to 0xFF.</p>

