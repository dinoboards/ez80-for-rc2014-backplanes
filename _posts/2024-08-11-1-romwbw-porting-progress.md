---
layout: post
title: RomWBW porting progress
---

## Show me the code!

If you want to see the code I have been writing/porting to make RomWBW work with my eZ80 module, you can find it in 2 repos:

1. My RomWBW fork - this has all the driver updates - mostly related to I/O addressing and some tweaks to timing/delays: [https://github.com/dinoboards/RomWBW/tree/dean-ez80](https://github.com/dinoboards/RomWBW/tree/dean-ez80)
2. My eZ80 firmware code.  This is the code that is loaded onto the eZ80's on-chip ROM.  It's the thing that runs on first boot and configures the chip as required to start RomWBW - it also manages interrupt marshalling. [https://github.com/dinoboards/rc2014-ez80](https://github.com/dinoboards/rc2014-ez80)

## RC2014/RomWBW porting progress:

So far I have manage to port and verify the following modules with my eZ80:

1. [Rc2014 Digital IO](https://rc2014.co.uk/modules/digital-io/)
2. [RC2014 512K ROM/RAM module](https://rc2014.co.uk/modules/512k-rom-512k-ram-module/)
3. [RC2014 Compact Flash Module (original version V1.0)](https://rc2014.co.uk/modules/compact-flash-module/compact-flash-module-v1/)
4. [SN76489 sound module by J.B. Langston](https://github.com/jblang/SN76489)
5. [RC2014 YM/AY Sound Card](https://www.tindie.com/products/semachthemonkey/ym2149-sound-card-for-rc2014-retro-computer/)
6. [RC2014 IDE Adapter](https://github.com/electrified/rc2014-82c55-ide)
7. [WD37C65 Based Floppy Adapter](https://www.smbaker.com/z80-retrocomputing-part-14-rc2014-floppy-controller-boards)*
8. [Yellow MSX V9958 VDP](https://www.tindie.com/products/dinotron/v99x8-msx-rgb-video-module-for-rc2014-v38/)**
9. [Yellow MSX YM2149 Game](https://www.tindie.com/products/dinotron/ym2149-msx-game-board-for-rc2014/)
10. [Yellow MSX Keyboard](https://www.tindie.com/products/dinotron/msx-keyboard-designed-for-rc2014/)
11. [Yellow MSX USB (CH376)](https://www.tindie.com/products/dinotron/msx-cassette-usb-module-designed-for-rc2014/)

\* I am not quite sure where I got this module from, but its based on *Dr. Scott M. Baker's* design.

\** As this is driven by the TMS9918 driver, I hope/suspect that the TMS9918 VDP modules would also work - but I have yet to be able to confirm.

