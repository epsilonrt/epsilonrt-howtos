# piduino — outils en ligne de commande

[piduino](https://github.com/epsilonrt/piduino) est une bibliothèque C++ qui
donne accès aux entrées-sorties (GPIO, I²C, SPI, UART) des cartes Raspberry Pi,
NanoPi, Orange Pi et Banana Pi avec une API aussi proche que possible du langage
Arduino. Elle installe aussi deux commandes utilisables directement dans le
terminal.

Ce dossier rassemble la traduction française de leurs **pages de manuel**
(`man pinfo` et `man pido` sur la carte).

| Commande | Rôle |
|---|---|
| [`pinfo(1)`](pinfo.md) | Afficher les informations sur la carte (modèle, SoC, mémoire, bus I²C et ports série disponibles) |
| [`converters.md`](converters.md) | Guide d'utilisation des convertisseurs (CAN, CNA, expandeurs de GPIO) avec `pido -c` et la classe `Converter` : MAX11615 et MAX7311 de la carte RPi Extended, exemples de programmes |
| [`pido(1)`](pido.md) | Manipuler les broches GPIO, mais aussi des composants I²C ou SPI (expandeurs, CAN, CNA, capteurs) : afficher leur état, changer leur mode, écrire ou lire, PWM, attendre un front |
