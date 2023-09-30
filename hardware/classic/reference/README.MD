
# Gamebuino Classic Reference

[Back to Home](./../../../README.MD) | [Go to Hardware description of Classic](./../README.MD)

## Core

- [gb.begin()](./gb-begin.md)
- [gb.titleScreen(F("name"), logo)](./gb-titleScreen.md)
- [gb.update()](./gb-update.md)
- [gb.setFrameRate(fps)](./gb-setFrameRate.md)
- [gb.pickRandomSeed()](./gb-pickRandomSeed.md)
- [gb.changeGame()](./gb-changeGame.md)
- gb.frameCount
- gb.getDefaultName(string) // 10 char

### User interface

- gb.menu(menu, length)
- gb.keyboard(string, length)
- gb.popup(F("text"), duration)

### Physics

- gb.collidePointRect(x1, y1, x2, y2, w, h)
- gb.collideRectRect(x1, y1, w1, h1, x2, y2, w2, h2)

### Performance monitor

- gb.getCpuLoad()
- gb.getFreeRam()
- gb.frameDurationMiros

## Display

Usedul constants : `LCDWIDTH`, `LCDHEIGHT`, `gb.display.fontWidth` and `gb.display.fontHeight`

### General

- gb.display.clear()
- gb.display.update()
- gb.display.fillScreen(color)
- gb.display.setColor(color, background)
- gb.display.persistence = true/false

### Pixels

- gb.display.drawPixel(x, y)
- gb.display.getPixel(x, y)

### Lines

- gb.display.drawLine(x0, y0, x1, y1)
- gb.display.drawFastVLine(x, y, h)
- gb.display.drawFastHLine(x, y, w)

### Circles

- gb.display.drawCircle(x, y, r)
- gb.display.fillCircle(x, y, r)
- gb.display.drawCircleHelper(x, y, r, corner)
- gb.display.fillCircleHelper(x, y, r, corner, delta)

### Triangles

- gb.display.drawTriangle(x0, y0, x1, y1, x2, y2)
- gb.display.fillTriangle(x0, y0, x1, y1, x2, y2)

### Bitmaps

- gb.display.drawBitmap(x, y, bitmap, rotation, flip)

#### Constants

- Rotation : `NOROT`, `ROTCCW`, `ROT180`, `ROTCW`
- Flip : `NOFLIP`, `FLIPH`, `FLIPV`, `FLIPVH`

### Text

- gb.display.print(text or numbers)
- gb.display.println(text or numbers)
- gb.display.drawChar(x, y, char, size)
- gb.display.setFont(font)
- gb.display.cursorX = x
- gb.display.cursorY = y
- gb.display.fontSize = size
- gb.display.textWrap = true/false

### Advanced

- gb.display.getBuffer()
- gb.display.getBitmapPixel(bitmap, x, y)

### Sound

- Sound : overall explanation of how the sound library works

#### Tracks

- gb.sound.playTrack(track, channel)
- gb.sound.stopTrack(channel)
- gb.sound.changePatternSet(patternSet, channel)

#### Pattern

- gb.sound.playPattern(sound, channel)
- gb.sound.stopPattern(chanel)
- gb.sound.setPatternLooping(boolean, channel)
- gb.sound.isPlaying(channel)

#### Notes & commands

- gb.sound.playNote(pitch, duration, channel)
- gb.sound.command(command, x, y, channel)
- gb.sound.changeInstrumentSet(instrumentSet, channel)

#### Build-in sounds

- gb.sound.playOK()
- gb.sound.playCancel()
- gb.sound.playTick()

#### Misc

- gb.sound.setVolume(volume, channel)
- gb.sound.getVolume(channel)
- gb.sound.setInstruments(instruments, channel)
- gb.sound.prescaler

### Battery

- gb.battery.voltage
- gb.battery.level
- gb.battery.show = true/false

### Backlight

- gb.backlight.set(brightness)
- gb.backlight.automatic = true/false
- gb.backlight.backlightValue
- gb.backlight.ambientLight

The following should be set using SETTINGS.HEX

- gb.backlight.backlightMin = 0 to 255
- gb.backlight.backlightMax = 0 to 255
- gb.backlight.ambientLightMin = 0 to 1024
- gb.backlight.ambientLightMax = 0 to 1024
