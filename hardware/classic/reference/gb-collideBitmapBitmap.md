
# gb.collideBitmapBitmap() for Gamebuino Classic

[Back to Home](./../../../README.MD) | [Back to Classic Reference Summary](./README.MD)

## Description

Checks pixel by pixel if two bitmaps are overlapping.

## Syntax

```
gb.collideBitmapBitmap(x1, y1, b1, x2, y2, b2);
```

## Parameters

- x1: horizontal coordinate of the first bitmap
- y1: vertical coordinate of the first bitmap
- b1: first bitmap
- x2: horizontal coordinate of the second bitmap
- y2: vertical coordinate of the the second bitmap
- b2: second bitmap

## Returns

- true: if the two bitmaps have at least one overlapping pixel
- false: if the bitmaps are not overlapping

## See also

- [gb.collidePointRect()](./gb-collidePointRect.md)
- [gb.collideRectRect()](./gb-collideRectRect.md)
- [gb.display.drawBitmap()](./gb-display-drawBitmap.md)

## General information

Saved on September 30, 2023
