
# gb.display.drawCircleHelper() for Gamebuino Classic

[Back to Home](./../../../README.MD) | [Back to Classic Reference Summary](./README.MD)

## Description

Draws part of a circle at a specified location. The color used is defined using [gb.display.setColor()](./gb-display-setColor.md). If you specify values that cause parts of the circle to be off screen, then they will not be rendered.

## Syntax

```
gb.display.drawCircleHelper(x, y, r, corner);
```

## Parameters

- x: horizontal coordinate of the centre of the circle.
- y: vertical coordinate of the centre of the circle.
- r: radius of the circle.
- corner: which corner to draw:
    - 1: top left
    - 2: top right
    - 4: bottom right
    - 8: bottom left

## Returns

None

## See also

- [gb.display.setColor()](./gb-display-setColor.md)

## General information

Saved on September 30, 2023
