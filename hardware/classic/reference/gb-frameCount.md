
# gb.frameCount for Gamebuino Classic

[Back to Home](./../../../README.MD) | [Back to Classic Reference Summary](./README.MD)

## Description

This variable is incremented each time gb.update() is called, so it represents the number of frames rendered since the program began running. It should be preferred over millis() to measure time. It can be used for periodic event timing (like an animated bitmap) using modulo.

## Syntax

```
int count = gb.frameCount;
```

## Parameters

None

## Returns

unsigned long: Number of frames rendered since the program began running (20 frames per second by default)

## See also

- [gb.update()](./gb-update.md)

## General information

Saved on September 30, 2023
