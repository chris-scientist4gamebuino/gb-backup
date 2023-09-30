
# EEPROM.read()

[Back to Home](./../../../README.MD)

## Description

Read the value corresponding to the adress

## Syntax

```
EEPROM.read(address);
```

## Parameters

address: the location to read from, starting from 0 (int)

## Returns

The value stored in that location (Byte)

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
    levels_unlocked = EEPROM.read(0);
}
```

## See also

- [Memory](./memory.md)
- [EEPROM.write()](./eeprom-write.md)
- [EEPROM.update()](./eeprom-update.md)

## General information

Saved on September 30, 2023
