
# gb.display.drawChar() for Gamebuino Classic

[Back to Home](./../../../README.MD) | [Back to Classic Reference Summary](./README.MD)

## Description

Draws a character at a specified location. The font is defined using [gb.display.setFont()](./gb-display-setFont.md). If you specify values that cause parts of the char to be off screen, then parts will not be rendered.

## Syntax

```
gb.display.drawChar(x, y, char, size);
```

## Parameters

- x: horizontal coordinate of the top left of the character
- y: vertical coordinate of the top left of the character
- char: the character to draw; char is the number of the Decimal character in the ASCII printable characters table.
- size: the size of the character. Changing the size changes what one pixel is viewed as - EG 4 would make each pixel a 4x4 group of pixels

## Returns

None

## Example

```
#include <SPI.h>
#include <Gamebuino.h>
Gamebuino gb;
extern const byte font5x7[]; //use the font5x7 for uppercase letters
void setup(){
    gb.begin();
    gb.display.setFont(font5x7); 
}
void loop(){
    if(gb.update()){
        gb.display.drawChar(10, 10,  90, 3);  // 90 =Z (DECIMAL char)
    }
}
```

## See also

- [gb.display.setFont()](./gb-display-setFont.md)

## General information

Saved on September 30, 2023
