
# gb.menu() for Gamebuino Classic

[Back to Home](./../../../README.MD) | [Back to Classic Reference Summary](./README.MD)

## Description

Displays a menu to select from a list of items.

## Syntax

```
gb.menu(menu, MENULENGTH);
```

## Parameters

- menu (char** PROGMEM): the items to select from, stored as a PROGMEM array of strings (see example below).
- MENULENGTH (byte): the number of items in the menu.

## Returns

The number of the item selected, or -1 if menu is left without selecting an item (char).

## General information

Saved on September 30, 2023
