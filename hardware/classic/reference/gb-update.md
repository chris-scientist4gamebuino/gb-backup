
# gb.update() for Gamebuino Classic

[Back to Home](./../../../README.MD) | [Back to Classic Summary](./README.MD)

## Description

Returns true and updates everything (display, sound, batter monitor, etc.) at a fixed frequency (20 times per second by default).

It should be used in a specific conditional structure, see "Syntax".

## Syntax

```
while(1){
  if(gb.update()){
    //your game here
  }
}
```

## Parameters

None

## Returns

boolean: true if enough time has elapsed since the last frame (20 frames per second = 50ms per frame).

## See also

- [gb.begin()](./gb-begin.md)
- [gb.setFrameRate()](./gb-setFrameRate.md)

## General information

Saved on September 30, 2023
