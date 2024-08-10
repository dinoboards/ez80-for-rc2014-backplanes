---
layout: post
title: Battery Power and clocks
---

A new PCB arrived last week.  This iteration, V1.5, has a number of improvements, including a small battery holder for CR1216/1225 3V Lithium Batteries.

This battery supplies power to the on-chip Real-Time-Clock circuit.  So now the eZ80 remembers the time.

<figure style="text-align: center"><img style="width: 80%; height: 80%"  src="{{ site.baseurl }}/assets/images/ez80-1.5.jpg"></figure>

But in addition to keeping time, I now have a known and consistent clock signal.  I can use this clock signal for other features:

1. Generating a 50Hz or 60Hz system level timer counter (similar to RomWBW HBIOS system timer).
2. Detect the current CPU operating frequency.

As the main CPU oscillator is mounted in a 8 PIN socket, I can easily change the frequency that drives the CPU.  I have a number of compatible oscillators on hand that I can test the board and CPU at: (7.3728 MHz, 14.745 MHz, 16 MHz, 18.432 MHz, 20 MHz, 24 MHz, 25 MHz, 32 MHz).

> The eZ80 variant I have selected (eZ80F92) is rated at a maximum of 20 MHz.  But my experiments demonstrate that it can works just as reliability when **overlocked** to 32MHz.  Maybe even more.

When I change the clock frequency though, without changing the UART's selected clock divider for its baud rate generator, I needed to adjust my PC's serial baud rate.  For example, if I double the clock frequency, I would need to double the baud rate of my connecting serial adapter.

As I now can reliably detect at boot up, the current installed CPU frequency, I have updated the software to configure the on-chip baud rate divider to always ensure I have the desired baud rate (typically configured to 115200).

This speeds up the testing of the system at different CPU clock frequency, across a range of  RC2014/RCBus modules -- helping me to verify I have good level of general compatibility.

<figure style="text-align: center"><img style="width: 80%; height: 80%"  src="{{ site.baseurl }}/assets/images/ez80-installed.jpg"></figure>

## RomWBW Boot


Here is an example of a current boot configuration for the system.

<figure style="text-align: center"><img style="width: 80%; height: 80%"  src="{{ site.baseurl }}/assets/images/hbios-boot.png"></figure>

