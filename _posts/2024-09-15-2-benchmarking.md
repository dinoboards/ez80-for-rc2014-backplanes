---
layout: post
title: Benchmarking
---

My primary goal with the eZ80 module is to achieve maximum compatibility with RC2014/RCBus modules and configurations. The eZ80 is a significantly faster processor. However, to ensure communication with older, slower retro modules, appropriate wait states must be introduced, which means some performance will be sacrificed. Despite this, the system remains much faster than a standard Z80 running at 7MHz.

I ported some benchmarking software from the z88dk library, which includes the classic Dhrystone and Whetstone benchmarks to give me some insights on the degree of performance this system operates at, compared to a stock RC2014 build.

I ran these benchmarks on various configurations and at different CPU frequencies, and the results were quite interesting.

When the eZ80 boots up, it detects the current CPU frequency and adjusts the timing for accessing the external ROM/RAM module and its own on-chip flash ROM. I chose conservative settings and calculations. As a faster oscillator is installed, the boot-up firmware will increase the extra wait states required when accessing memory and I/O devices.

The purpose of this benchmarking was not to showcase raw speed. Other eZ80 systems can achieve higher speeds by using fast RAM chips, typically SMD RAM chips soldered next to the CPU.  The fast RAM chips allow the eZ80 to run at its full potential. But I tend to think, if I wanted a faster solution, I could probably achieve orders of magnitude increases by running everything in JavaScript within my main PC's browser.

## The Results


### Stock RC2014 System @ 7.372Mhz
First, I ran the two benchmarking utilities using a stock RC2014 system with a standard Z80 CPU running at 7.372MHz, an SIO/2 module for serial communication, and a V9958 VDP module to generate a 50Hz timer tick for time reference.

* **Dhrystone score: 556.2**
* **Whetston score: 8**

### Stock RC2014 System @ 20Mhz*

I then conducted the same tests, using my Turbo CPU module - the fastest standard Z80 I have.  This is a CPU module for the RC2014/RCBus systems, that can run a standard Z80 at 20Mhz.  (If you are not familiar with this module, check it out over in my Tindie Store [https://www.tindie.com/stores/dinotron/](https://www.tindie.com/stores/dinotron/))

* **Dhrystone score: 1144.2**
* **Whetston score: 16.5**

> \* The Turbo CPU does not run its Z80 at a constant 20Mhz - it will, if any I/O operations are performed, briefly slowdown to a more typical speed (7.372Mhz).  The interrupt timer tick from the V9958 will cause the CPU to do some I/O operations, thus down clocking the CPU for short bursts.

### eZ80 System @ various clock frequencies

Below is the result of running the benchmark applications for the eZ80 at different CPU Frequencies.  Note how the wait states increase as the clock goes up.

|Cpu Frequency | Memory W/S | I/O W/S | Flash W/S	| Dhrystone Score | Whetstone Score |
|--------------|------------|---------|-----------|-----------------|-----------------|
|    7.37	     |     1      |    6    |     0     |     1149.7      |     14.4        |
|    14.75     |     1      |    6    |     1     |     2301.1      |     28.9        |
|    16.00     |     2      |    6    |     1     |     1680.4      |     21.1        |
|    18.43     |     2      |    6    |     1     |     1938.9      |     24.3        |
|    20.00     |     2      |    6    |     1     |     2103.1      |     26.4        |
|    24.00     |     2      |    5    |     1     |     2520.3      |     31.7        |
| *   **25.00**     |     **2**      |    **5**    |     **1**     |     **2625.6**      |     **33.0**     *   |
|    32.00     |     3      |    7    |     2     |     2536.1      |     31.8        |
|    35.00     |     4      |    7    |     2     |     2226.0      |     28.0        |
|    40.00     |     4      |    9    |     2     |     2544.0      |     32.0        |

Paradoxically, it does seem that sometimes, a faster CPU frequency, actually results in slower
performance.  For example, the 14.74MHz CPU outperforms the 18.43MHz CPU because the latter is heavily impacted by the need for an extra wait state.

I know that some configuration are perhaps overly conservative (eg: 32Mhz will run at 2 W/S just fine) - and with some more time to experiment, more 'performance' can be achieved.

Despite this, the current solution is still significantly faster than the stock RC2014. Additionally, the solution remains highly compatible, even when overclocking the eZ80.

> And yes, I have successfully overclocked my eZ80 to 40MHz, even though Zilog rates it for a maximum speed of 20MHz. I am eager to test even higher frequencies, but I lack the necessary oscillators to generate higher clock rates.

Next, I need to start generating some Mandelbrot sets!
