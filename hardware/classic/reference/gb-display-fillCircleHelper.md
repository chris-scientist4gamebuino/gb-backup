
# gb.display.fillCircleHelper() for Gamebuino Classic

[Back to Home](./../../../README.MD) | [Back to Classic Reference Summary](./README.MD)

## Description

Draws and fills half of a circle at a specified location. The color used is defined using [gb.display.setColor()](./gb-display-setColor.md). If you specify values that cause parts of the circle to be off screen, then they will not be rendered.

## Syntax

```
gb.display.fillCircleHelper(x, y, r, corner, delta);
```

## Parameters

- x: horizontal coordinate of the centre of the circle.
- y: vertical coordinate of the centre of the circle.
- w: radius of the circle.
- corner: which part to draw:
    - 1: left half
    - 2: right half
- delta: extra length added in between each quarter (EG, larger delta = longer semicircle)

## Returns

None

## See also

- [gb.display.setColor()](./gb-display-setColor.md)

## General information

Saved on September 30, 2023
