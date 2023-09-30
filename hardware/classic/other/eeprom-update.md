
# EEPROM.update()

[Back to Home](./../../../README.MD)

## Description

Write the value corresponding to the address. This function clear the current value

## Syntax

```
EEPROM.update(address, value);
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
    EEPROM.update(0, levels_unlocked);
}
```

## See also

- [Memory](./memory.md)
- [EEPROM.read()](./eeprom-read.md)
- [EEPROM.write()](./eeprom-write.md)

## General information

Saved on September 30, 2023

