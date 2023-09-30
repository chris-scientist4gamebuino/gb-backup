
# Gamebuino Classic Reference

[Back to Home](./../../../README.MD) | [Go to Hardware description of Classic](./../README.MD)

## Core

- [gb.begin()](./gb-begin.md)
- [gb.titleScreen(F("name"), logo)](./gb-titleScreen.md)
- [gb.update()](./gb-update.md)
- [gb.setFrameRate(fps)](./gb-setFrameRate.md)
- [gb.pickRandomSeed()](./gb-pickRandomSeed.md)
- [gb.changeGame()](./gb-changeGame.md)
- [gb.frameCount](./gb-frameCount.md)
- [gb.getDefaultName(string) // 10 char](./gb-getDefaultName.md)

### User interface

- [gb.menu(menu, length)](./gb-menu.md)
- [gb.keyboard(string, length)](./gb-keyboard.md)
- [gb.popup(F("text"), duration)](./gb-popup.md)

### Physics

- [gb.collidePointRect(x1, y1, x2, y2, w, h)](./gb-collidePointRect.md)
- [gb.collideRectRect(x1, y1, w1, h1, x2, y2, w2, h2)](./gb-collideRectRect.md)
- [gb.collideBitmapBitmap(x1, y1, b1, x2, y2, b2)](./gb-collideBitmapBitmap.md)

### Performance monitor

- [gb.getCpuLoad()](./gb-getCpuLoad.md)
- [gb.getFreeRam()](./gb-getFreeRam.md)
- [gb.frameDurationMiros](./gb-frameDurationMicros.md)

## Display

Usedul constants : `LCDWIDTH`, `LCDHEIGHT`, `gb.display.fontWidth` and `gb.display.fontHeight`

### General

- [gb.display.clear()](./gb-display-clear.md)
- [gb.display.update()](./gb-display-update.md)
- [gb.display.fillScreen(color)](./gb-display-fillScreen.md)
- [gb.display.setColor(color, background)](./gb-display-setColor.md)
- [gb.display.persistence = true/false](./gb-display-persistence.md)

### Pixels

- [gb.display.drawPixel(x, y)](./gb-display-drawPixel.md)
- [gb.display.getPixel(x, y)](./gb-display-getPixel.md)

### Lines

- [gb.display.drawLine(x0, y0, x1, y1)](./gb-display-drawLine.md)
- [gb.display.drawFastVLine(x, y, h)](./gb-display-drawFastVLine.md)
- [gb.display.drawFastHLine(x, y, w)](./gb-display-drawFastHLine.md)

### Rectangles

- [gb.display.drawRect(x, y, w, h)](./gb-display-drawRect.md)
- [gb.display.fillRect(x, y, w, h)](./gb-display-fillRect.md)
- gb.display.drawRoundRect(x, y, w, h, r)
- gb.display.fillRoundRect(x, y, w, h, r)

### Circles

- [gb.display.drawCircle(x, y, r)](./gb-display-drawCircle.md)
- [gb.display.fillCircle(x, y, r)](./gb-display-fillCircle.md)
- [gb.display.drawCircleHelper(x, y, r, corner)](./gb-display-drawCircleHelper.md)
- [gb.display.fillCircleHelper(x, y, r, corner, delta)](./gb-display-fillCircleHelper.md)

### Triangles

- [gb.display.drawTriangle(x0, y0, x1, y1, x2, y2)](./gb-display-drawTriangle.md)
- [gb.display.fillTriangle(x0, y0, x1, y1, x2, y2)](./gb-display-fillTriangle.md)

### Bitmaps

- [gb.display.drawBitmap(x, y, bitmap, rotation, flip)](./gb-display-drawBitmap.md)

#### Constants

- Rotation : `NOROT`, `ROTCCW`, `ROT180`, `ROTCW`
- Flip : `NOFLIP`, `FLIPH`, `FLIPV`, `FLIPVH`

### Text

- [gb.display.print(text or numbers)](./gb-display-print.md)
- [gb.display.println(text or numbers)](./gb-display-println.md)
- [gb.display.drawChar(x, y, char, size)](./gb-display-drawChar.md)
- [gb.display.setFont(font)](./gb-display-setFont.md)
- [gb.display.cursorX = x](./gb-display-cursorX.md)
- [gb.display.cursorY = y](./gb-display-cursorY.md)
- [gb.display.fontSize = size](./gb-display-fontSize.md)
- [gb.display.textWrap = true/false](./gb-display-textWrap.md)

### Advanced

- [gb.display.getBuffer()](./gb-display-getBuffer.md)
- gb.display.getBitmapPixel(bitmap, x, y)

## Sound

[Sound](./../learning/sound.md) : overall explanation of how the sound library works

### Tracks

- gb.sound.playTrack(track, channel)
- gb.sound.stopTrack(channel)
- gb.sound.changePatternSet(patternSet, channel)

### Pattern

- gb.sound.playPattern(sound, channel)
- gb.sound.stopPattern(chanel)
- gb.sound.setPatternLooping(boolean, channel)
- gb.sound.isPlaying(channel)

### Notes & commands

- gb.sound.playNote(pitch, duration, channel)
- gb.sound.command(command, x, y, channel)
- gb.sound.changeInstrumentSet(instrumentSet, channel)

### Build-in sounds

- gb.sound.playOK()
- gb.sound.playCancel()
- gb.sound.playTick()

### Misc

- [gb.sound.setVolume(volume, channel)](./gb-sound-setVolume.md)
- gb.sound.getVolume(channel)
- gb.sound.setInstruments(instruments, channel)
- gb.sound.prescaler

## Battery

- [gb.battery.voltage](./gb-battery-voltage.md)
- [gb.battery.level](./gb-battery-level.md)
- [gb.battery.show = true/false](./gb-battery-show.md)

## Backlight

- gb.backlight.set(brightness)
- gb.backlight.automatic = true/false
- gb.backlight.backlightValue
- gb.backlight.ambientLight

The following should be set using SETTINGS.HEX

- gb.backlight.backlightMin = 0 to 255
- gb.backlight.backlightMax = 0 to 255
- gb.backlight.ambientLightMin = 0 to 1024
- gb.backlight.ambientLightMax = 0 to 1024
