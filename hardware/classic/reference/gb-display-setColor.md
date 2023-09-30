
# gb.display.setColor() for Gamebuino Classic

[Back to Home](./../../../README.MD) | [Back to Classic Reference Summary](./README.MD)

## Description

Changes the color used to draw on the screen by all the display functions.

The colors can be either `WHITE`, `BLACK`, `GRAY` or `INVERT`.

Most of the display functions only have one color, the foreground color. But some functions also have a background color:

- [gb.display.drawBitmap()](./gb-display-drawBitmap.md)
- [gb.display.drawChar()](./gb-display-drawChar.md)
- [gb.display.print()](./gb-display-print.md)

`INVERT` color won't work with the `drawCircle` and `fillCircle` function, because the algorithm they use passes several times over the same pixel, so the color will toggle several times and be inconsistent.

## Syntax

```
gb.display.setColor(foregroundColor, backgroundColor);
```

## Parameters

- foregroundColor : `WHITE`, `BLACK`, `GRAY` or `INVERT`
- backgroundColor (optional) : `WHITE`, `BLACK`, `GRAY` or `INVERT`. If no background color is specified, the background is transparent.

## Returns

None

## General information

Saved on September 30, 2023
