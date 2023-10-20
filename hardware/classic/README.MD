
# Hardware of Gamebuino Classic

[Back to Home](./../../README.MD) | [Go to Reference of Gamebuino Classic](./reference/README.MD)

## Specifications

- CPU : atmega328 @ 16Mhz
- Display : 84*48px monochrome + auto backlight
- Sound : magnetic speaker, 4 channels
- Input : Arrows + A B C buttons
- Communication : 1x micro USB, 1x micro SD card, 2x I2C, 1x ICSP
- Battery : ~12-24h life, 300mAh LiPo battery, charged through USB
- Power : 3.3V, 150mA max
- Dimensions : ~credit card sized : 90x45x12 mm (3.5x1.8x0.5")

## Changelog

### Gamebuino R1 (Early bird version)

- first public release

### Gamebuino R2 (First batch of 1000)

- "sound" connector added
- Photo resistor spacer taller (3mm -> 6mm)
- Acrylic case minor changes

### Gamebuino R3

- Brighter and whiter backlight LED
- Slightly different speaker
- 0805 components instead of 0603
- Screen contact improved
- More stable bootloader by Jonnection

### Gamebuino R4

- RT/TX LEDs bug fixed
- Changed speaker for a piezo
- Added 3.5mm audio jack
- Added battery connector

## Schematics

[PDF Resources : Gamebuino R2 schematics](./../../pdf/classic/schematics/Gamebuino_r2_schematics.pdf)

## 3D Model

[Gamebuino R2 CAD.STEP](./../../other-files/classic/3d-model/Gamebuino_r2_cad.STEP)

STEP is the industry standard format for 3D files. You can open it using the free CAD (Computer Assisted Design) software [Design Spark](http://www.rs-online.com/designspark/electronics/eng/page/mechanical), or any other CAD software like SolidWorks, Catia, etc. Google Sketch is free but only the paid version can open STEP files.

## Case

If you want to build your own case, wether it is by laser cutting acrylic, 3D printing ABS, machining stainless steel or carving wood, you can use the following template. Make sure to select a scale of 100% when you print it.

- [PDF Resources : Gamebuino case](./../../pdf/classic/case/Gamebuino_case.pdf)
- [PDF Resources : Gamebuino case simple](./../../pdf/classic/case/Gamebuino_case_simple.pdf)

## Bootloader

The [bootloader](./other/bootloader.md) is a small program that permanently resides in Gamebuino memory which allows you to load games via SD card or USB port. Under normal circumstances loading a new game into Gamebuino would require you to use an external programmer connected to the Gamebuino's ICSP port. The [bootloader](./other/bootloader.md) makes this process easier and faster by eliminating the need for a separate hardware programmer.

## Bill Of Materials

```
BOTTOM LAYER SMT ----------------------------------
Quantity	Label		Value
Resistors	
	7	R1-R6, R10	1k
	5	R7-R9, R11, R12	4k7
	3	R13-R15, TH1	10k
	1	R16		100k
	2	R17,R18		1M
Ceramic capacitors
	2	C1,C2		22p
	4	C3-C6		0.1u
	3	C7-C9		1u
	2	C10,C11		10u
Integrated Circuits
	1	U1		ATMEGA328P_32PIN
	1	U2		MCP73833-FCI/UN
	1	U3		FTDI FT232RL
	1	Q1		MMBT2222A
	1	Q2		MIC5205
Diodes
	1	D1		LL4148
Connectors
	1	SD		micro SD
	1	USB		Micro USB B
TOP LAYER ------------------------------------------
	1	X1		16MHz
	1	BAT		240 mAh LiPo BATTERY
	7	BTN0-BTN3,BTNA,BTNB,BTNC
	1	BUZZER		Magnetic speaker
	9	CHAR,FULL-LED4,PWR,RX,TX	LED
	2	J1,J2		I2C connectors
	1	ON   OFF	SWITCH
	1	PR1		GL5539
	1	SCREEN		Nokia 5110 LCD
```

## General information

Saved on September 30, 2023
