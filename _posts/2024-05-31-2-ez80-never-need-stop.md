---
layout: post
title:  "eZ80 never need stop"
---

<p>I want this new eZ80 module to operate on a backplane, with an original Z80 module also.&nbsp; <strong>Dual CPU mode</strong>.&nbsp;&nbsp;</p>

<p>This will allow it to function within my Yellow MSX platform, where the standard Z80 can operate in true timing accurate backward compatible mode for games etc, and the eZ80&nbsp;can be switched on specifically by applications and services to get true Super performance.&nbsp;&nbsp;<br></p>


<p>This is akin to how the MSX Turbo machines operated.&nbsp; They had a standard Z80 running at the 3.5Mhz for games, and a Zilog R800 for the high performance mode. The MSX BIOS code has a bit of support, built in, for this CPU switching.</p>


<p>The Dual CPU mode should be optional also, so that the eZ80&nbsp;can be the only CPU in the backplane, if desired.</p>


<p>There are 2 ways I considered for this to be achieved.</p>


<ol><li>Co-ordinate the BUSREQ/BUSACK signals of both the Z80 and eZ80&nbsp;, so that at any one point in time, only one processor is active.</li><li>The eZ80&nbsp;is always active, and its bus access is granted as required, through some buffers.&nbsp;&nbsp;</li></ol>


<p>I went for option 2.</p>


<p>In this design, the eZ80&nbsp;is isolated from the main buses via 74HCT245 buffer chips.&nbsp; Enough for all its address, control and data signals.&nbsp; The buffer chips are in 'disabled' state, when the Z80 is accessing the buses.&nbsp;&nbsp;</p>


<figure><img style="width: 327px; height: 268.99px;" width="327" height="268.99" class="lazy" src="9482271717198311859.png"></figure>


<p>When its time for the eZ80&nbsp;to access the bus, the Z80's BUSREQ signal is 'activated'.&nbsp; This will cause the Z80 to HALT, and release the bus.&nbsp; Once its released the bus, the Z80's BUSACK signal is activated.&nbsp; By connecting the BUSACK signal to the 74HCT245 buffer chip's enable signal (EZ_ONBUS), the eZ80&nbsp;will only be able to access the bus, when the Z80 has fully released it.<br></p>


<p><em>An advantage of using these 74HCT245 buffer chips, is that they will step up the eZ80&nbsp;'s 3.3V signals to a good 5V signal.&nbsp; Although I suspect the 3.3V signal level will be 'enough' - having the higher voltage and drive would, I think, help with interfacing to lots of modules in the backplane, older modules, and generally help with signal to noise issues.</em></p>


<p>Here how I think I can make the Dual CPU mode work.&nbsp;<em> (At this stage, not really sure if this will work.)</em><br></p>


<p>1. Z80 has the main bus access.</p>


<ul><li>The Z80's BUSREQ and BUSACK are inactive (high).</li><li>The eZ80&nbsp;'s buffer chips 'Enabled' input is wired to the Z80's BUSACK.</li><li>As such, the buffer chips are 'disabled'.</li><li>eZ80&nbsp;is unable to access the bus, except for the INT signal.</li><li>eZ80&nbsp;is operating on its internal ROM/RAM.</li><li>eZ80&nbsp;has a GPIO pin connected to the INT, configured for output.</li><li>eZ80&nbsp;can signal to the Z80 via this interrupt signal.</li><li>The BUSACK signal is also wired to a eZ80&nbsp;GPIO pin, so the eZ80&nbsp;can identify if it has access to the bus.</li></ul>


<p>2. The Z80's BUSREQ is activated (LOW).</p>


<ul><li>Only the Z80 can do this, by writing to a 74HC74 flip-flop, to trigger the BUSREQ signal.</li></ul>


<p>3. The Z80 will be halted, as its BUSREQ is now activated (LOW).</p>


<p>4. The Z80 will release the address, control, and data lines.</p>


<p>5. The Z80 will activate its BUSACK signal (LOW).</p>


<p>6. The eZ80&nbsp;buffer chips will become enabled.</p>


<p>7. The eZ80&nbsp;'s GPIO mapped to the BUSACK will inform the eZ80&nbsp;that is now has access.</p>


<ul><li>The eZ80&nbsp;could be 'interrupted' by this GPIO/BUSACK signal or it may simply be polling this pin.</li><li>The eZ80&nbsp;will change it GPIO pin mapped to the INT line from output to input.</li><li>As the eZ80&nbsp;now has access to the buses, it can, when it want to give control back to the Z80, write to the 74HC74 flip-flop to release the BUSREQ signal.</li></ul>

{% include comments.html %}
