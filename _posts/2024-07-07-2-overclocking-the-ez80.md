---
layout: post
title: Overclocking the eZ80
---

In addition to testing my recent changes around the data buffering, I also conducted some overclocking tests.

I have my RC2014 backplane populated with the following modules:

1. eZ80 CPU with Interface module V1.3 (bodge fix applied for `WR`/`EZ_WR`)
2. RC2014 512K RAM/ROM - containing my ported version of RomWBW
3. RC2014 Digital Input/Output
4. V9958 (Yellow MSX) VDP Module
5. RC2014 Compact Flash (Original version directly connected to the bus)

I have tested the eZ80 CPU (rated at 20Mhz) with various clock frequencies: (7.3728Mhz, 14.7456Mhz, 18.432Mhz, 24Mhz, 25Mhz and 32Mhz)

With the eZ80 CPU clocked at 7.3728Mhz -- all modules worked correctly -- including the Compact Flash module.

At 14.7456Mhz, the Compact Flash module would not work (got detected but read/writes were corrupting).

I continued to test from 14.7456 all the way up to 32Mhz (12Mhz above Zilog's specification).  Was very surprised that at all frequencies up to and including 32Mhz, the system booted up just fine.

> Although the CPU is running super fast, given the way the addressing mode is configured, the 'equivalent' bus frequency would be around 6-10Mhz.

So I have my eZ80 CPU running super fast at 32Mhz.  Would love to start doing some benchmarks at some point.
