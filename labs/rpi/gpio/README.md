# GPIO — principes des entrées-sorties d'une carte Raspberry Pi

Ce dossier explique, avec des schémas et des exemples pour la bibliothèque
[piduino](https://github.com/epsilonrt/piduino), trois notions de base des
entrées-sorties d'une carte Raspberry Pi : générer un signal PWM, détecter un
front par interruption et dialoguer avec des composants sur le bus I²C.

Chaque document se lit en entier : il explique le principe avant de passer à la
pratique.

| Document | Sujet |
|---|---|
| [`pwm.md`](pwm.md) | Le signal PWM : période, fréquence, rapport cyclique, génération par un timer matériel (fréquence, `range`, réglage avec `pido`), obtention d'une tension continue par filtrage (compromis ondulation / temps de réaction, filtre RC et filtre de Sallen-Key) |
| [`interrupt.md`](interrupt.md) | Lire une entrée par scrutation et ses inconvénients, principe de l'interruption sur front (circuit de détection, routine d'interruption), exemple avec piduino |
| [`i2c.md`](i2c.md) | Le bus I²C : principe, signaux (START, STOP, acquittement), adresse sur 7 bits et piège de l'alignement en hexadécimal, activation du bus sur la Raspberry Pi, scan et lecture/écriture de registres avec les `i2c-tools` |

## Bibliothèque piduino

Le sous-dossier [`piduino`](piduino/) rassemble la documentation de la
bibliothèque piduino et de ses commandes `pido` et `pinfo`.

| Document | Sujet |
|---|---|
| [`piduino/README.md`](piduino/README.md) | Présentation de piduino et de ses deux commandes |
| [`piduino/pinfo.md`](piduino/pinfo.md) | Traduction française de la page de manuel `pinfo(1)` |
| [`piduino/pido.md`](piduino/pido.md) | Traduction française de la page de manuel `pido(1)` |
| [`piduino/converters.md`](piduino/converters.md) | Les convertisseurs de piduino : interface de `pido -c`, MAX11615 (CAN) et MAX7311 (expandeur de GPIO) de la carte RPi Extended, programmation avec la classe `Converter` |
