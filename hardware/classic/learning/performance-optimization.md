
# Performance optimization

[Back to Home](./../../../README.MD) | [Back to Learning for Classic](./README.MD) | [Go to Classic Reference Summary](./../reference/README.MD)

## Introduction

**The following apply to Gamebuino, Arduino and any 8-bit microcontroller as well.**

Gamebuino is a video game console which has limited hardware (8-bits CPU running at 16Mhz and 2KB of RAM...). It's enough if you want to program Pong or Tetris, but if you plan to do some advanced games with multiplayer and artificial intelligence, you will probably have to optimize your game for it to run fast enough and fit in the chip's memory.

**Most of the optimization work is done at high level, by designing smart algorithms.** But sometimes it's not enough, and to go further you have to get into more system-specific programming to gain speed and memory.

## Premature optimization

**Don't optimize something that doesn't need to be.** First write a clean code, something easy to read and maintain. Then, only if you actually need performance improvement, benchmark your code to know what is most time consuming and likely to be optimized. Don't try to guess what should be optimized, because you'll be wrong most of the time, and you'll spend time optimizing something that isn't the bottleneck.

## Don't be smart

**"You should think REALLY, REALLY hard about sacrificing code clarity for the sake of speed or space."** westfw on Arduino forum

## Variables

### data types

**You should always use the smallest variable type you need.** Because Gamebuino runs on a 8-bits microcontroller, 16 and 32 bits variables takes a lot of time and memory, and floating point variables are even worse than integers.

**Most of the time you can re-arrange math to avoid the need of floating point.** For example, the battery voltage is stored in mV instead of V, so we can use a unsigned int instead of a floating point.

**Arduino data types**

| **Type**       | **Min**        | **Max**        | **Size** | **Perf** |
|----------------|----------------|----------------|----------|----------|
| byte           | 0              | 255            | 8        | :)       |
| char           | -128           | 127            | 8        | :)       |
| unsigned int   | 0              | 65 535         | 16       | :/       |
| int            | -32 768        | 37 767         | 16       | :/       |
| unsigned long  | 0              | 4,294,967,265  | 32       | :(       |
| long           | -2,147,483,648 | 2,147,483,647  | 32       | :(       |
| float / double | -3.4028235E+38 | 3.40282325E+38 | 32       | :'(      |

### #define

You can use `#define` for constant variables that will never change, like pin numbers. This way it won't take any space in the chip's memory.

Here is an example :

```
#define ledPin 13
```

### progmem

For constant large arrays or bitmaps you can use progmem to avoid loading them in RAM. [Arduino article](http://arduino.cc/en/Reference/PROGMEM) explains that~~, for more info read a complete tutorial~~.

## Operations & Math

### Arithmetic operators

Addition is the fastest, multiplication is slower, and division is even slower.

To multiply or divide by a power of 2 you can use bitwise operators `<<` and `>>` but remember, "Don't be smart".

### Math

Avoid using sin() cos() and these kind of operations, because they are very slow and take a lot of memory. Moreover, they manipulate floating point variables, which are also time and memory consuming.

## Loops

## Input / Output

You can make input/output a lot faster by directly accessing the ports / timers with native AVR code instead of the Arduino Library. But you will loose portability, simplicity and maintainability of your code. **Do that only if you absolutely need it.** For example, to generate the sound, the Gamebuino Library have to change an output thousands of times per second, so I had no choice. But if you want to blink a led, just don't do that.

### Digital Output

#### PWM / Analog Output

First you should have a good understanding of what is PWM. You can read [Wikipedia's article](http://en.wikipedia.org/wiki/Pulse-width_modulation).

##### PWM output

In arduino code, to change a PWM output's duty cycle you use the function :

```
analogWrite(pin, value);
```

To change the PWM output with native AVR code, it's more complicated, more difficult to read and maintain, and less portable. But it's way faster, and for some critical tasks you may need that extra speed. One good example is generating the sound signal of your Gamebuino : the output changes thousands times per second, so I could not afford the use of analogWrite().

##### PWM frequency

Disclaimer : changing PWM frequency won't increase performance and will mess with timers (i.e. with millis(), sound, etc).

## Communication

### Serial

To increase Gamebuino's / Arduino's USB communication speed, you can **increase the baud rate** (number of pulse per second). Usually 9600 is the default value, but you can use 115200 without problem. To set the baudrate, just call this function at the beginning of your setup() function :

```
Serial.begin(115200);
```

Warning : **the baud rate of the PC have to be the same** or you will end receiving hieroglyphs.

Note : You should be able to run at 1 000 000 bauds, but Arduino application only support speeds up to 115 200 bauds.

Note : Starting from Arduino 1.0, serial is no longer blocking. Data will be sent in background using interruptions. To waits for the transmission of outgoing serial data to complete, use Serial.flush().

### SPI

You can increase the SPI speed by changing the clock divider using [SPISetClockDivider](http://arduino.cc/en/Reference/SPISetClockDivider).

```
SPI.setClockDivider(SPI_CLOCK_DIV2) //runs the SPI at maximum speed (8Mhz)
```

Warning : On Gamebuino, screen's specified maximum SPI frequency is 4Mhz, so if you use a clock divider < 4 it will act erratically.

### I2C / TWI

Gamebuino's atmega328 can reach I2C speeds up to 400Khz.

Edit Arduino/libraries/Wire/utility/wire.h and change the line 27 from

```
#ifndef TWI_FREQ
#define TWI_FREQ 100000L
#endif
```

to

```
#ifndef TWI_FREQ
#define TWI_FREQ 40000L
#endif
```

For older versions of arduino go to Arduino forum.

Note : check that your I2C module is compatible with high speed I2C (400kHz).

## Clock frequency

You can make your Gamebuino / Arduino / Microcontroller run faster by increasing the clock frequency. To do so, you have to change the crystal for one with higher frequency. But you're limited by the chip specifications and the power voltage.

In the case of Gamebuino, the crystal is a 16Mhz one, while it's powered at 3.3V.

Error creating thumbnail: Unable to save thumbnail to destination
As we can see above (plot from atmega datasheet p.316), Gamebuino is already out of the "safe operating area". So technically, you're Gamebuino is overclocked. But it's not really a problem, because the "safe operating area" guaranties you that you microcontroller will still work even at 70°C, and that's not the kind of temperatures a Gamebuino should reach (I don't think the LiPo battery would like it).

Well, the point is, you can overclock your Gamebuino to 20Mhz, but it could become unstable. Moreover, it's an hardware change, and nobody will want your games if they require to mess with a solder iron.

Anyway, if you still want to do so, change the crystal, and you'll have to change the following :

In Arduino/hardware/arduino/boards.txt change the corresponding xxx.build.f_cpu to the right value (16000000L for 16Mhz, 20000000L for 20Mhz).
Optionnal : in Gamebuino Library, you will have to change the sound's interrupt interval, or all the music will be more acute because the music's frequency will also increase.

## Arduino vs native AVR

### Comparison

You should stick to Arduino code unless absolutely necessary, for the sake of portability and ease of use.

But if you want to do some advance, high perfomance stuff then you'll have to use native AVR code, without Arduino library that make things simpler but slower.

### Benchmarks

analogOutput() : 127 cycles

fast PWM : 2 cycles

random() : 2190 cycles

fast random : 9 cycles

## Assembly language

If native AVR code is still to slow, or if you want to know exactly what happen clock cycle by clock cycle, you can use assembly language.

## Futher reading

- "optimizing code?" on Arduino Forum
- [PDF Resources : Atmel AVR4027: Tips and Tricks to Optimize Your C Code for 8-bit AVR Microcontrollers](./../../../pdf/other/AVR4027_performance_tips.pdf)
- [PDF Resources : Atmel AVR035: Efficient C Coding for AVR](./../../../pdf/other/AVR035_efficient_C_coding.pdf)
- Chapter [17](http://downloads.gamedev.net/pdf/gpbb/gpbb17.pdfhttp://downloads.gamedev.net/pdf/gpbb/gpbb17.pdf) and [18](http://downloads.gamedev.net/pdf/gpbb/gpbb18.pdf) of Michael Abrash's [Graphics Programmer's Black Book](http://www.gamedev.net/page/resources/_/technical/graphics-programming-and-theory/graphics-programming-black-book-r1698) are about Conway's Game of Life optimization, but shows how much "Don't be smart" is important
- [Game programming patterns](http://gameprogrammingpatterns.com/)

## General information

Saved on September 30, 2023
