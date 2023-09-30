
# gb.display.getPixel() for Gamebuino Classic

[Back to Home](./../../../README.MD) | [Back to Classic Reference Summary](./README.MD)

## Description

Read the color of the pixel stored in the display buffer at the given coordinates.

## Syntax

```
gb.display.getPixel(x, y);
```

## Parameters

- x: horizontal coordinate. Should be between 0 (left of the display) and LCDWIDTH (right of the display)
- y: vertical coordinate. Should be between 0 (top of the display) and LCDHEIGHT (bottom of the display)

## Returns

color of the pixel. 0 for WHITE and 1 for BLACK.

## General information

Saved on September 30, 2023
