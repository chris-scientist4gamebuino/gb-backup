
# gb.getFreeRam() for Gamebuino Classic

[Back to Home](./../../../README.MD) | [Back to Classic Reference Summary](./README.MD)

## Description

Returns the amount of free RAM in number of bytes by measuring the distance between he heap and the stack. Results are not really consistent as the measure is taken in a single place in the program, and it doesn't take in account background operations (interrupts). When your Gamebuino acts randomly, you're probably out of RAM. More info here: [Memory optimization](./../learning/memory-optimization.md).

## Syntax

```
gb.getFreeRam();
```

## Parameters

None

## Returns

int: the amount of free RAM in number of bytes

## See also

- [Memory optimization](./../learning/memory-optimization.md)
- [Performance optimization](./../learning/performance-optimization.md)

## General information

Saved on September 30, 2023
