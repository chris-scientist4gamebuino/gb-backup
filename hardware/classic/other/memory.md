
# Memory

[Back to Home](./../../../README.MD)

Here is listed and explained of some useful Arduino storage keyword, classes, and function.

See [Reference](./../reference/README.MD) for all Gamebuino specific functions.

## EEPROM

EEPROM persists after the Gamebuino has been shut down, making it perfect for game saves. There are 1024 bytes of EEPROM.

There are no special Gamebuino libraries for reading from or writing to EEPROM, because these functions come from the standard Arduino libraries. You need to use `#include <EEPROM.h>` for these functions.

Here are a few useful functions for working with EEPROM:

- `EEPROM.read(address) //int` [see details](./eeprom-read.md)
- `EEPROM.write(address, value) //int, byte` [see details](./eeprom-write.md)
- `EEPROM.update(address, value) //int, byte` [see details](./eeprom-update.md)
- `EEPROM.get(address, data) //int, primitive type (eg. float) or a custom struct`
- `EEPROM.put(address, data) //int, primitive type (eg. float) or a custom struct`

### How Game Switching Works

You may notice that saved data for a game works even when you switch games with the loader, and yet games have access to the entire EEPROM! How is saved data preserved even when another game can write to EEPROM?

The answer lies in the Gamebuino [loader](https://github.com/Gamebuino/Gamebuino-Classic/tree/master/examples/4.Utilities/Loader). When the loader starts, it checks to see whether there are any bytes in EEPROM that are not zero. If so, it saves the contents of EEPROM into a `<GAME NAME>.SAV` file on the SD card. Later, when a game is about to be loaded, it checks for a `<GAME NAME>.SAV` file, and writes the contents to EEPROM if it is found. If not, it clears the EEPROM.

This approach allows games to use the entire contents of EEPROM without worrying about corrupting other games' save data, and without worrying about writing save data to the SD card.

## General information

Saved on September 30, 2023
