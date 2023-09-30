
# Start to code

[Back to Home](./../../../README.MD)

## Starting page

### Basics

When you start creating your game, you should have included these file:

`#include <SPI.h>` (To communicate with the Gamebuino's screen)

`#include <Gamebuino.h>` (To get the pre made library)

`Gamebuino gb` (To create a Gamebuino object named gb)

Then, you'll need to initialize the Gamebuino object in the **setup** function using this: `gb.begin();` and display the menu using the gb.titleScreen function: `gb.titleScreen(F("The name of your game"));`

For now, your code should look like this:

```
#include <SPI.h>
#include <Gamebuino.h>
 
Gamebuino gb;
 
void setup () {
    gb.begin();
   
    gb.titleScreen(F("My game"));
}
```

### Customize the title screen

The function `gb.titleScreen()` display only a title, to display a picture, create an 8x8px up to 64*30px `PNG/JPG/etc`. file and upload it on the Bitmap encoder in the [Download](./download.md), If you want to edit it later, take the binary with multi-line (Wrap output line = true) or if you want it in one simple line choose hexadecimal with a single line (Wrap output line = false)

## Bitmaps

Do you want to create a bitmap? or maybe create a map of them? It's here

### Create a bitmap

Note: If in your game, you enable persistence, you'll need to draw the bitmap each frame.

First, you need to create the bitmap on a byte array like this:

```
const PROGMEM byte Name[] {
    /*The size of the bitmap*/
    8,8,
    /*The bitmap itself*/
    B00000000,
    B00000000,
    B00000000,
    B00000000,
    B00000000,
    B00000000,
    B00000000,
    B00000000
};
```

(You can make it using hexadecimal but it will be harder to modify after.)

0 = a blank pixel/white and 1 = a black pixel

### Draw the bitmap

To draw a bitmap, use :

```
gb.display.drawBitmap(x, y, bitmap, rotation, flip);
```

First, there's the coordinate to draw on the screen, x and y. Replace bitmap with the name of the bitmap (It's "Name" in the exemple above). Rotation and flip are optional, replace rotation with NOROT (Not rotation : 0°), ROTCCW (Rotate counterclockwise: -90° or 270°), ROT180 (180 Rotation : 180°) and ROTCW (Rotate clockwise: 90°). For flip, use NOFLIP (No flip), FLIPH (Flip horizontally), FLIPV (Flip vertically) and FLIPHV (Reverse the bitmap, vertically and horizontally).

### Draw a map

(Work in progress)

## General information

Saved on September 30, 2023
