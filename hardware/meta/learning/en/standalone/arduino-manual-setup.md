
# Arduino manual setup

[Back to index of english standalone](README.MD)

If you had an issue during the quick setup, or if you want to know how it works, you're in the right place!

For the beginner.

## Download and install the Arduino IDE

Go to the official Arduino website, and [download Arduino IDE](https://www.arduino.cc/en/software/#ide).

- This might require administrator rights on the computer
- Compatible with Windows, Mac, Linux, and even Raspberry Pi!
- Download and install the software
- Windows users: Install the drivers when asked to

![Adruino IDE : open new project](/assets/img/env/arduino_ide/arduino_ide_open.png)

Now, we need to setup two key elements to be able to program on your Gamebuino: the board and the libraries.

## Install the boards

- Launch the Arduino IDE

![Launch Arduino IDE](/assets/img/env/arduino_ide/arduino_ide_launch.gif)

- Click on "Files/Preferences"

![Arduino IDE : Preferences](/assets/img/env/arduino_ide/arduino_ide_preferences.png)

- Copy the following address in "Additional Boards Manager URLs:". If you already have other URLs present, separate them with a comma ",": `https://lab.gamebuino.com/arduino/package_gamebuino_index`.json or (alternatively) `https://gamebuino.com/sdk/package_gamebuino_index.json`.
- You can check "Show verbose output during compilation" and during "upload" to have more info about what is going on under the hood.

![Arduino IDE : Preferences panel](/assets/img/env/arduino_ide/arduino_ide_preferences_panel.png)

- Click "OK"
- Click on "Tools/Boards/Board Manager..."

![Arduino IDE : Tools menu](/assets/img/env/arduino_ide/arduino_ide_tools_menu.png)

- Wait for the boards list to be updated. We will now download two boards
- Search for "Arduino SAMD" and install it

![Arduino IDE : Arduino SAMD board](/assets/img/env/arduino_ide/arduino_ide_board_samd.gif)

- Search for "Gamebuino META" and install it

![Arduino IDE : Gamebuino META board](/assets/img/env/arduino_ide/arduino_ide_meta_board.gif)

- Sit down and relax while it's downloading. You can check out the Creations and give a few likes to kill time :)
- Click "Close"
- Click on "Tools/Boards/Gamebuino META" (at the bottom of the list) to select the right device

![Arduino IDE : select board](/assets/img/env/arduino_ide/arduino_ide_select_board.png)

- Linux only: Give permission to the serial port with one of the following commands depending on your system, then log out and in again
    - Raspberry + Archlinux: `sudo usermod -a -G uucp $USER`
    - Ubuntu: `sudo usermod -a -G dialout $USER`

## Install the library

- Launch the Arduino IDE (if you closed it)
- Click on "Sketch/Include Library/Manage Libraries"

![Arduino IDE : sketch](/assets/img/env/arduino_ide/arduino_ide_sketch.png)

- Close your eyes, breathe deeply and think about the game you want to make while it's downloading.
- Click on "Close"

If you have a Gamebuino, it's time to try out your first code!

## General information

Created at September 30, 2025 from english official page
