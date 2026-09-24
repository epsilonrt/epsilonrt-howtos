# piduino — outils en ligne de commande

[piduino](https://github.com/epsilonrt/piduino) est une bibliothèque C++ qui
donne accès aux entrées-sorties (GPIO, I²C, SPI, UART) des cartes Raspberry Pi,
NanoPi, Orange Pi et Banana Pi avec une API aussi proche que possible du langage
Arduino. Elle installe aussi deux commandes utilisables directement dans le
terminal.

Ce dossier rassemble la traduction française de leur documentation, tirée du
[wiki de piduino](https://github.com/epsilonrt/piduino/wiki).

| Commande | Rôle |
|---|---|
| [`pinfo`](pinfo.md) | Afficher les informations sur la carte (modèle, SoC, mémoire, bus I²C et ports série disponibles) |
| [`pido`](pido.md) | Manipuler les broches GPIO : afficher leur état, changer leur mode, écrire ou lire un niveau logique, activer une résistance de tirage, attendre un front |

Les pages de manuel complètes s'obtiennent sur la carte avec `man pinfo` et
`man pido`.
