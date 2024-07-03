---
layout: post
title: HBIOS booted on the eZ80
---

I just managed to get <a href="https://github.com/wwarthen/RomWBW" target="_blank">RomWBW</a> HBIOS's to boot over the eZ80's UART, using a standard <a href="https://rc2014.co.uk/modules/512k-rom-512k-ram-module/" target="_blank">RC2014 memory module</a>.

I had to create a new driver and configuration within RomWBW for the eZ80 processor/configuration.&nbsp; I added a new serial driver to the code, to target the on-chip UART.&nbsp; That was the easy part.&nbsp; I also had to update all the HBIOS memory banking code that targets the RC2014 512K RAM/ROM module.&nbsp; This module splits its 512k ROM and 512k RAM into 16K pages.&nbsp; These 16K pages, can then be mapped to segments within the 64K address range that the Z80 uses.<em>&nbsp; (eZ80 runs RomWBW in Z80 compatibility mode, and so will need to do the same).</em>


<figure><img style="width: 937px; height: 767px;" width="937" height="767" src="{{ site.baseurl }}/assets/images/hbios-first-boot.png"></figure>

This <em>banking </em>is done by writing to the I/O ports at addresses $78, $79, $7A and $7B, for the first, second, third and last 16K pages of the 64K address range

As mentioned in previous posts, the eZ80 forces the use of 16 bit addressing for it I/O operations, so I needed to update the code that writes to these ports to apply a full 16 bit address.&nbsp; $78 must become $FF78 and $79 must become $FFF9 and so on.

I had ran some simple tests, to confirm that I could write the correct code to switch the Banks to read the ROM pages and read/write to the RAM pages.&nbsp; That all seemed to work.&nbsp; So I expected it should just be a matter of updating the code in RomWBW where it writes to these ports to ensure the correct full 16 bit address is applied.

How innocent was I this morning!

It just refused to work.&nbsp; I was getting some very strange errors indeed!

After a lot of cursing, I discovered that whenever code is executed to switch Bank 1, it would switch Bank 1 correctly, but it would also cause Bank 3 to change to a random page!

All other Banks switched just fine.&nbsp; Weirdly if I re-switch Bank 3 after changing Bank 1, it would work, and all the banks would have the correct pages assigned.&nbsp; Only when the page for Bank 1 was updated, would anything go wrong.&nbsp; I tired running the eZ80 with a much slower clock - but the problem still persisted.

I then discovered, through a lot of diagnosing, that if I replaced the 74HCT670 chips in my <em>RC2014 512K RAM/ROM</em>, with 74HC670 everything would work just fine.

Why might you ask?&nbsp; I don't think its related to the input voltages, as the chips are receiving a good 5V TTL signal from the 74HCT245 chips.

Not really sure, but I wondering if this might be a subtle timing difference between the eZ80 and the Z80.&nbsp; The eZ80 has been configured to address I/O operations using the Z80 hardware bus timing.&nbsp; But when I looked a little closer at the eZ80 timing diagram and the Z80 timing diagram, it seems to suggest that the Z80 will hold the address and data lines a little bit after the WR line is de-activated.&nbsp; For the eZ80 this seems tighter.&nbsp; The eZ80 seems to change the Address and Data lines as the WR signal is going from LOW (active) to HIGH (in-active).

The 74HCx670 require that their inputs are held stable while the WE line is active (the WE is linked to the WR signal from the backplane).&nbsp; Here's the note within the TI spec sheet:

> The Write Address (WA0 and WA1) to the “internal latches” must be stable while WE is LOW for conventional operation.

My thinking is, the WE line is going HIGH at the same time that the address lines are changing state -- causing the 670's to go a little screwy.&nbsp; And maybe the slight difference in timing of the chips is enough to work around the issue.

I can't be sure though, that this issue is not also common with a Z80 driving the system -- not sure if the RAM/ROM module requires 74HC670 when its running in a stock Z80 system - perhaps I just populated the RAM/ROM incorrectly.

I am not really sure.&nbsp; I will need to do some more testing to confirm this - my conclusions could be very wrong.&nbsp; Further investigation is required.

But nonetheless, today was a good day - I have my RC2014 running with an eZ80 CPU! Cool!

(My fork of the RomWBW changes can be found on my <a href="https://github.com/vipoo/RomWBW/tree/dean-ez80">github site</a>).
