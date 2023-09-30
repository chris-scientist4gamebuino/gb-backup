
# gb.display.setFont() for Gamebuino Classic

[Back to Home](./../../../README.MD) | [Back to Classic Reference Summary](./README.MD)

## Description

Change the font used in `gb.display.print` and `gb.display.drawChar`

The font can be either one of the default fonts or a custom font. The tools to create your own are on [GitHub](https://github.com/Rodot/Gamebuino-Font).

To be able to use one of the default fonts, don't forget to add `extern const byte font3x3[];` at the beginning of your sketch (see example below).

**Default fonts**


| font3x3 | font3x5 | font5x7 |
|:-------:|:-------:|:-------:|

## Syntax

```
gb.display.setFont(font);
```

## Parameters

- font: the font you want to use.

## Returns

None

## See also

- [gb.display.print()](./gb-display-print.md)
- gb.display.setTextSize

## General information

Saved on September 30, 2023
