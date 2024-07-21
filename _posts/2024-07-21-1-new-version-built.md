---
layout: post
title: New version built
---

I have achieved some success since my self inflicted RTC issue.

I built a new eZ80 CPU module paired with a newly designed Bus interface module.  The new interface module has solved some critical compatibility issues I was having.

I seem to be getting better at hand soldering the SMD component.  The eZ80 pins are quite tiny - and my hands do shake quite a bit - but I got there.

## Some images of the assembling

I start by assembling the CPU module, soldering all the SMD components first.  The resistors and capacitors are easy.  The eZ80 is a little tricky.  Once its done, I double check I don't have any shorts.

<figure><img style="width: 600px" src="{{ site.baseurl }}/assets/images/cpu-module-part-assembled.jpg">
</figure>
<div style="margin-left: 3em;width: 600px">The partially assembled eZ80 CPU module.  Just need to finish soldering some of the remaining PCB pins.  (had to use a 470 ohm through-hole resistor as I did not have a SMD version on hand)</div>

<br/>


<figure><img style="width: 600px" src="{{ site.baseurl }}/assets/images/position-some-pins-at-a-time.jpg"></figure>
<div style="margin-left: 3em;width: 600px">As the PCB pins are quite small, I use the interface PCB to align a few pin.</div>

<br/>

<figure><img style="width: 600px" src="{{ site.baseurl }}/assets/images/cpu-mounted-for-pin-soldering.jpg"></figure>
<div style="margin-left: 3em;width: 600px">Then I can insert the CPU module and solder them.  Repeating this for a small set of pins, until all are done.</div>

<br/>

<figure><img style="width: 600px" src="{{ site.baseurl }}/assets/images/cpu-module-underside.jpg"></figure>
<div style="margin-left: 3em;width: 600px">And the underside of the module, with all the pins finally soldered.  Only need to solder the ZDI 6 pin header.</div>

<br/>


<figure><img style="width: 600px" src="{{ site.baseurl }}/assets/images/cpu-and-interface-assembled.jpg"></figure>
<div style="margin-left: 3em;width: 600px">The completed solution - ready for testing.</div>

<br/>


