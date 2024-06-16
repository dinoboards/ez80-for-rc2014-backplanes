
# eZ80 CPU for RC2014 and other backplanes

## An eZ80 CPU Module that works within the RC2014 ecosystem and other similar backplanes

The goal of this project is to design and build a module for the RC2014™/RCBus and my Yelllow MSX Series, that can operate as a complete CPU module and drive the various available modules.

The eZ80 Zilog CPU is an updated version of Z80 CPU. It comes in a few variations with many on chip facilities in addition to the basic CPU, such as flash ROM, RAM, GPIO and other IO services. See the Wikipedia page for basic overview of the CPU (https://en.wikipedia.org/wiki/Zilog_eZ80)


---

<h2><strong>Project Objectives<br></strong></h2>

<ul><li>Interface an eZ80 CPU to drive the various TTL 5V modules for the RC2014<strong>&trade;</strong> and other similar backplanes.</li><li>Operate in a Dual-CPU mode - so that the original Z80 CPU and eZ80 can alternate access to the address/control and data buses.</li><li>Figure out how to get software working easy in the system.&nbsp; Look at RomWBW and my Yellow MSX series. cross compiler tools etc.</li><li>Utilise some extra pins on the 80 pin backplanes to support a direct 24 bit address range for a possible 'Large Linear Memory Module'.</li><li>Make it work with RomWBW and my Yellow MSX configurations.</li><li>Learn SMD and how to hand solder surface mounted components.</li></ul>

<h2><strong>Design Details</strong></h2>

<p>I have journaled some of my thinking around the design and learnings in the project log.&nbsp; If this is the your first viewing of the project, you may want to read through these journal entries sorted by 'Oldest' first. Click here:&nbsp;<a href="https://hackaday.io/project/196330/logs?sort=oldest">Journal Log</a></p>

<h2><strong>Which eZ80?</strong></h2>

<p>The eZ80 was originally released around the turn of the century.&nbsp; There are a number of variants available today.&nbsp; They all comes with additional features within the chip, such as Flash ROM, RAM, GPIO, UART, I2C and timers.&nbsp; Some can run at up to 20Mhz and other up to 50Mhz.&nbsp;&nbsp;</p>

<p>The key feature of the CPU above the original Z80, is its ability to address a full 16MB of memory.&nbsp; It has 24 address lines (8 more then the 16 for the Z80).&nbsp; Its has features built in to help run existing Z80 software in 'compatible' mode on this chip.&nbsp;&nbsp;</p>

<p>I choose the&nbsp;<strong>eZ80F92 </strong>variant for my designed.&nbsp; It can operate at up to 20Mhz, has 128K of on-chip Flash ROM and 8K of RAM.&nbsp; And lots of other features: UARTS, GPIO, timers, SPI and i2c.&nbsp; I may not be able to use all these features in my design though.</p>

<h2><strong>Surface Mount Device challenge</strong></h2>

<p>I have never worked with SMD stuff before.&nbsp; The eZ80F92&nbsp;comes in a 100 pin LQFP package.&nbsp; Its pins are very tiny - and with my aging eyes, might be a challenge for me to hand assemble.&nbsp; Of course, the PCB fabricator can assemble these things relativity cheaply - that may be an option.&nbsp;&nbsp;</p>

<p>But I don't want to make an all SMD module.&nbsp; So I intend to place the eZ80F92 on an adapter board, with pins, that can be inserted into a conventional module PCB.</p>

<h2><strong>Inspiration</strong></h2>

<p>Of course, this is not the first hobby, DIY, retro solution using the eZ80 CPU.&nbsp; There are a few out there that inspired me.</p>

<ul><li><a href="https://github.com/TheByteAttic/AgonLight">Agon</a>&nbsp;- a cool little single board retro machine, with large following and lots of open source material available.</li><li><a href="https://drive.google.com/file/d/1xoRq0Suo46uGw3tNLPOU8g6G4coTdOsZ/view">eZ-Tiny</a>&nbsp;- it might be small, but it is still very capable.</li><li>The&nbsp;&nbsp;<a href="https://hackaday.com/2020/02/23/a-z80-computer-at-the-next-level/">Z20X computer</a>&nbsp;- this seems to be abandon now and the original website is gone - but it gave me the inspiration for a CPU breakout module.&nbsp;</li><li><a href="https://rc2014.co.uk/">RC2014</a>- where it all began for me.</li><li>And my own <a href="https://github.com/vipoo/yellow-msx-series-for-rc2014">Yellow MSX project</a>.</li></ul>


<p>Below is a 10 second demo of the first operating prototype, driving the RC2014 Digital IO module.&nbsp; It just flashes the LEDS, so nothing very impressive - just confirms that the eZ80 is able to do I/O operations to another module.&nbsp; Lots of updates and software required to enable full operation.&nbsp; Eg: running CP/M, Basic, and eventually getting it to work in my Yellow MSX configuration.</p>

<div class="video-container"><iframe style="width: 500px; height: 281px;" src="//www.youtube.com/embed/s31_nAPKu_E" frameborder="0" allowfullscreen=""></iframe></div>
