
# gb.titleScreen() for Gamebuino Classic

[Back to Home](./../../../README.MD) | [Back to Classic Reference Summary](./README.MD)

## Description

Displays the title screen. Should be called after the [gb.begin()](./gb-begin.md) function to appear on startup. You should also allow the user to get back to the title screen when the user presses the "C" button to leave the game, as the title screens allows you to switch between games.

In the title screen, you can do the following:
- Button A: Leaves the title screen (so it gets back to the game)
- Button B: Toggle sound (muted/unmuted). If you hold down B while you start up your Gamebuino, it will be muted and won't play the start-up sound.
- Button C: Loads LOADER.HEX, which backs up the EEPROM on the micro SD card and allows you to load another game.

## Syntax

```
gb.titleScreen(F("name"), logo);
```

## Parameters

- F("name") (optional): Replace "name" with the name of your game (but keep the " "). Will be displayed in the start menu.
- logo (optional): The logo of your game. Any size from 8*8px up to 64*30px, or, if no name is specified up to 64*36px

## Returns

None

## See also

- [gb.begin()](./gb-begin.md)

## General information

Saved on September 30, 2023
