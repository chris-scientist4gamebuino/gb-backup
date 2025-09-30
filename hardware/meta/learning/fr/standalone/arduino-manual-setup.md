
# Configuration manuelle d'Arduino

[Retour au sommaire des standalone](README.MD)

Si vous avez rencontré un problème lors de l'installation rapide, où que vous souhaitez mettre un peu plus les mains dans le cambouis, vous êtes au bon endroit !

S'adresse au débutant.

## Téléchargez et installez l'IDE Arduino

Rendez vous sur le site arduino.cc, [téléchargez Arduino IDE](https://www.arduino.cc/en/software/#ide) et installez-le en suivant les instruction.

- Si vous avez déjà Arduino, mettez le à jour!
- Vous aurez besoin d'un accès administrateur à l'ordinateur
- Sous Windows : installez bien le driver lorsque ça vous est proposé

![Adruino IDE : ouvrir un nouveau projet](/assets/img/env/arduino_ide/arduino_ide_open.png)

## Installez la carte

- Lancez l'IDE Arduino

![Lancer Arduino IDE](/assets/img/env/arduino_ide/arduino_ide_launch.gif)

- Cliquez sur Fichier/Préférences

![Arduino IDE : Préférences](/assets/img/env/arduino_ide/arduino_ide_preferences.png)

- Copiez l'adresse suivante dans "URL de gestionnaire de cartes supplémentaires". Si vous avez d'autres adresses déjà présentes, séparez-les par une virgule "," : `https://lab.gamebuino.com/arduino/package_gamebuino_index.json` ou (alternatif) `https://gamebuino.com/sdk/package_gamebuino_index.json`
- Tant que vous y êtes, vous pouvez cocher "Afficher les résultats détaillés pendant la compilation" et pendant le "téléversement" pour avoir plus d'informations sur ce qui se passe derrière le rideau.

![Arduino IDE : panneau Preférénces](/assets/img/env/arduino_ide/arduino_ide_preferences_panel.png)

- Cliquez sur "OK"
- Cliquez sur "Outils/Types de carte/Gestionnaire de carte..."

![Arduino IDE : menu Outils](/assets/img/env/arduino_ide/arduino_ide_tools_menu.png)

- Attendez que la liste de cartes soit mise à jour. Nous allons à présent installer deux cartes
- Cherchez "Arduino SAMD" et cliquez sur "Installer"

![Arduino IDE : carte Arduino SAMD](/assets/img/env/arduino_ide/arduino_ide_board_samd.gif)

- Cherchez "Gamebuino META" et cliquez sur "Installer"

![Arduino IDE : carte Gamebuino META](/assets/img/env/arduino_ide/arduino_ide_meta_board.gif)

- Relaxez-vous pendant que ça télécharge. Vous pouvez parcourir les Créations et donner quelques likes pour passer le temps :)
- Cliquez sur "Fermer"
- Cliquez sur "Outils/Types de carte/Gamebuino META" (en bas de la liste) pour sélectionner le bon périphérique

![Arduino IDE : sélectionner la carte](/assets/img/env/arduino_ide/arduino_ide_select_board.png)


- Linux uniquement : donnez les permissions au port série avec une des commandes suivantes, puis fermez votre session utilisateur et reconnectez vous.
    - Raspberry + Archlinux : `sudo usermod -a -G uucp $USER`
    - Ubuntu : `sudo usermod -a -G dialout $USER`

## Installez la bibliothèque

- Lancez l'IDE Arduino (si vous l'avez fermé)
- Cliquez sur "Croquis/Inclure une bibliothèque/Gérer les bibliothèques"

![Arduino IDE : sketch](/assets/img/env/arduino_ide/arduino_ide_sketch.png)

- Fermez les yeux, respirez profondément et pensez au jeu que vous aimeriez faire pendant que ça télécharge.
- Cliquez sur "Fermer"

Si vous disposez d'une Gamebuino, il est temps de tester votre premier programme !

## Information générale

Créée le 30 septembre 2025, via la page française officielle
