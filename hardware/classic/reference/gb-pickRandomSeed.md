
# gb.pickRandomSeed() for Gamebuino Classic

[Back to Home](./../../../README.MD) | [Back to Classic Summary](./README.MD)

## Description

Picks a random seed using a mix of the battery voltage, ambient light sensor and the time elapsed since start up. It should be placed right after [gb.begin()](./gb-begin.md) and [gb.titleScreen()](./gb-titleScreen.md), this way the random seed will depend on how long the user takes to press "A" to leave the title screen.

## Syntax

```
gb.pickRandomSeed();
```

## Parameters

None

## Returns

None

## General information

Saved on September 30, 2023
