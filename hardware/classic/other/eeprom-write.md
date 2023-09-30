
# EEPROM.write()

[Back to Home](./../../../README.MD)

## Description

Write the value corresponding to the address if the address is empty

## Syntax

```
EEPROM.write(address, value);
```

## Parameters

- address: the location to read from, starting from 0 (int)
- value: the value to write, from 0 to 255 (byte)

## Returns

None

## Example

```
#include <EEPROM.h>
#include <SPI.h>
#include <Gamebuino.h>
 
Gamebuino gb;
 
byte levels_unlocked = 0;
 
void setup()
{
    gb.begin();
    gb.titleScreen(F("Read EEPROM"));
    EEPROM.write(0, levels_unlocked);
}
```

## See also

- [Memory](./memory.md)
- [EEPROM.read()](./eeprom-read.md)
- [EEPROM.update()](./eeprom-update.md)

## General information

Saved on September 30, 2023
