---
layout: post
title: RC2014/RCBus Mapping
---

As the eZ80 CPU has some differences and additional control signals, I need to consider how some of these signals have to be mapped, and how their slight difference may impact compatibility. I have already had to 'resolve' issues with the timing of the `WR` signal to ensure operation of the Bank Memory module and the Compact Flash module.

Many people have installed various alternative CPUs into their RC2014/RCBus based systems, from the various Zilog processors (Z180, Z280) and Intel 8080 to CPUs with very different timings and signals, such at the 6502. The eZ80 is now, just another CPU for the platform.

On the electrical front, the eZ80 operates at 3.3V, but is 5V tolerant.  The interface module I have built, uses a number of 74HC245 buffer chips to step-up all outgoing signals to 5V.  The frequency of these signals are going to be higher and have a much quicker rise and fall times compared to a stock Z80 system.  In all my testing to date, I have the eZ80 configured to operate in Z80 Bus Mode.  So in theory, an eZ80 operating at 20Mhz will drive the Address and Data Buses at a similar rate as a Z80 operating at 20Mhz - although its not quite that simple.  (Of course the eZ80 will still run at full speed when operating from its on-chip RAM/ROM, and combined with the chips ability to execute more instructions per clock cycle -- we will still have a very fast processor compared to a standard Z80)

Nonetheless, any CPU operating at 20Mhz will typically exceed the operating timings requirements for many RC2014/RCBus modules.

The eZ80 can be configured, as mentioned, to operate its Address, Data and Control Bus in various 'Modes' of operation.  It can be configured to its default eZ80 Mode, or a 'compatible' Z80 Mode, or if you wish, an Intel or even the very different Motorola Mode.

In the default eZ80 Bus Mode the CPU's reads and writes can be conducted within a single clock cycle.  Compared this to the Z80 mode, where the reads/writes are conducted over at least 3 clock cycles.  Of course, the targeted memory, or I/O device, can send a WAIT signal back to the CPU, extending the time taken for any transfer.  But this requires the memory to be able to trigger that WAIT signal before the CPU has moved onto the next cycle.  A lot of retro modules use older chips that could not possibly operate at these data rates.  Even the modern memory chips (AS6C4008 SRAM) typically used in the RC2014/RCBus systems will struggle at this rate.

Thankfully, the eZ80 can also be configured to include its own additional WAIT states.  So with the appropriate configuration, we can drive our Memory and I/O devices at a rate that is kept within the required timing specifications.

The modes are configured through the use of the eZ80's Chip Select signals (`CS0`, `CS1`, `CS2`, `CS3`).  Each of these `CSx` lines can be configured to only be activated for specific address ranges. For example, I have configured `CS3` to use *Z80 Compatible Bus Mode* and only activate when a memory read or write happens in the address range of 0x030000 to 0x03FFFF.  And configured `CS2` to also use *Z80 Compatible Bus Mode* and activate when an I/O request happens in the address range of 0xFF00 to 0xFFFF.

### The eZ80 to RC2014/RCBus mapping

Below is a brief description of some of the key signals on the RC2014/RCBus backplanes and the eZ80 equivalent and any differences.

#### A0-A15 and D0-D7

These signals are as expected; the address from the CPU and the bi-directional data bus.

#### 19 M1

The eZ80 does not have an `M1` signal - it has `INSTRD` (instruction read) signal.  For simplicities sake, I have not mapped this line -- I just pulled it high with a 10K resistor.

#### 21 - Bus Clock

My eZ80 CPU module has an onboard oscillator.  I have tested oscillator's with frequencies from as low as 7.3728MHz up to the CPU's rated 20MHZ and overclocked to 32Mhz.  But I don't send this clock signal over the bus.  I use one of the on-chip counters to divide the CPU clock by 4 and optionally send that over the bus.

By supplying a more 'typical' clock rate on the bus, any modules that need a clock, for example the various retro sound modules, can be operated without the need for a separate clock module.

One issue with this though, is the V2 revision of the RC2014 Compact Flash module.  It uses this Bus-Clock signal to adjust the timing of the `WR` signal.

Because this generated Bus-Clock is not phased with the timing of the `WR` signal, there may be issues, as it's been designed with the expectation that the bus clock is the CPU clock. I have yet to test V2 of this module, but I suspect it will only work if I configure the eZ80 to have lots of additional wait states.

#### 22 INT

Technically the eZ80 (or specifically the eZ80F92), does not have a general purpose `INT` signal.  Many of the GPIO's can be independently configured to trigger an interrupt request in the CPU.

I have tired the eZ80's GPIO pin `PB4` to the `INT` line of the RC2014/RCBus.

#### 23 MREQ

This signal is *not* the eZ80's `MREQ` signal.  This signal is driven by the eZ80's `CS3` signal.

The intent of this signal is to indicate a 16 bit only memory device access.  (See `CS0` and `CS1` below for potential 24 bit memory access)

Although the upper address lines (A16-A23) will also be activated, any attached *original* 16 bit memory module will not have any connection to these new address lines, so of course, they are ignored.

`CS3` is configured in the eZ80 boot code, (at time of writing), to only activate for memory addresses in the range of 0x030000 to 0x03FFFF.  Its also configured to have an appropriate bus timing to avoid attempting to read/write as a rate faster than the typical memory modules can achieve.

The timing of this signal is a little different than the `MREQ` of the Z80 or eZ80.  Is will be activated a little bit before the standard `MREQ` would.  Testing so far indicates that this does not introduce any compatibility issues.

#### 24 WR

This signal is not the eZ80 `WR` signal. As mentioned above and in previous posts, the timing of the eZ80's `WR` signal during I/O operation had to be modified to have a reduced period of activation, to enable a longer post activation hold time.  This enables any device that expects a valid address (or data) still present to have time to acquire the address/data state.

For I/O requests, the `WR` line goes low when the eZ80's `WR` line goes low, but is pulled high a cycle or 2 before the eZ80's `WR` line goes high.  For memory request, the `WR` signal has the same timing as the eZ80's `WR` signal.

#### 26 IORQ

This signal is generated from the eZ80's `IORQ` signal, but with a slight difference in timing when conducting writes.

Due to the way the eZ80 has been configured, the `IORQ` is expected to only activate when the eZ80's `CS2` signal is also activated.  `CS2` is configured to only trigger for any I/O operation within the address range of 0xFF00 to 0xFFFF.

To align with the 'shortened' `WR` signals, the `IORQ` will also go high as the bus version of the `WR` signal goes high.

This signal operates in a similar way to `MREQ`, in that its designed to work with original (8 bit addressed) RC2014 and compatible modules.

The `CS2` timing and bus mode are configured within the eZ80 to ensure compatibility with typical retro I/O modules.

> The `IORQ` will activate if the CPU's attempts to access any address outside of the configured range of `CS2`.  In this case, the busses will be operating in eZ80 mode and will be well outside of the timing requirements for many RC2014/RCBus modules.

#### 35-36 TX/RX

Not connected. The interface module has UART signals (RX,RX,CTS,RTS).  But I have not mapped these to the bus.

#### 37-40 & 77-80 USER1-USER8

Not connected.

#### 49-56 A16-A23

The higher address signals of the eZ80.

#### 59 RFSH

Not connected.  The eZ80 does not have such a signal.

#### 63 HALT

Not connected.  Perhaps in a later revision of the Interface Module, I may map this signal to the eZ80's `HALT` signal.

#### 66 NMI

Not connected.  Perhaps in a later iteration of the Interface Module, I may map this signal to the eZ80's `NMI` signal.

#### 48 CS0, 49 CS1

Connected to the eZ80 chip select signals.  Not buffered, as such they operate at 3.3V.

