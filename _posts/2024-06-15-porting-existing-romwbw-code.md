---
layout: post
title: Porting existing RomWBW code
---

Porting RomWBW to support the eZ80 CPU requires a number of things to be achieved:

1. Existing hardware drivers must be updated to ensure all I/O operation supply the required 16 bit address, and not assume an 8 bit address will be sufficient.
2. Interrupt handlers need to continue to work.
3. New drivers need to be developed for the eZ80 on-chip UART and Real-Time-Clock and other on-chip services.

RomWBW contains many systems (BASIC, various CP/M implementation, and other applications).  Most of the hardware access is delegated through `HBIOS` and its embedded drivers.  This should make it farily straightforward to port the code.

`HBIOS` also implements the interrupt handling logic.  It can be configured for standard single routine or a vector table.  I will discuss interrupt handling and options in another post.

# Converting to 16 bit I/O addressing

All `HBIOS` code, including within the various drivers, assume it only needs to present a valid lower 8 bit address for any IO access.  But for an eZ80 configuration, we need to comply with the need to present the correct full 16 bit address.

Given the way I have configured the CPU, all external I/O access (other modules on the bus backplane), needs to use the address range of $FFxx.

A lot of code in `HBIOS` use the typical Z80 IO instructions, such as:

```
OUT     ($54), A        ; Send A to I/O port $54

  ...

IN      A, ($54)        ; Retrieve value from port $54 into A
```

\* <small>Notice how the IO port address is only 8 bits.</small>

> Although existing Z80 based system typically dont care about the upper 8 bits when I/O ports are addressed, the Z80 is actually presenting a full 16 bit address.  In the case of `OUT ($54), A`, the A register is loaded into the top 8 bits -- which is not particularly useful.

To convert the code above, to ensure we target the IO ports within the 16 bit address range of $FF00 to $FFFF, we could change the code to something like:

```
LD      BC, $FF54
OUT     (C), A          ; aka OUT (BC), A

  ...

LD      BC, $FF54
IN      A, (BC)         ; aka IN A, (BC)
```

This might require us to save the 'BC' register on the stack, if that register is used within the specific code section.


Another option, is to take advantage of the eZ80 `RST.L XX` instruction.  This instruction, specific to the eZ80, is similar to the standard `RST` instructions.  The `.L` variant will cause the CPU to enter ADL (24 bit mode), and jump to a routine within the on-chip flash.

So I can write a routine that will perform the actual I/O operation correctly.  This routine, can walk the stack, and inspect the next instruction, interpret it and ensure the upper byte is always $FF.

So with the code above, we could now achieve the same effect with:

```
RST.L   $08
OUT     ($54), A        ; Send A to I/O port $FF54

  ...

RST.L   $08
IN      A, ($54)        ; Retrieve value from port $FF54 into A
```

There will naturally be a fair performance cost for the 2nd option, but it will make porting much much easier and safer.


