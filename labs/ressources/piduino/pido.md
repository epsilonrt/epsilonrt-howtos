# pido — manipuler les broches GPIO

*Traduction française de la page [Pido](https://github.com/epsilonrt/piduino/wiki/Pido)
du wiki de [piduino](https://github.com/epsilonrt/piduino). Les sorties de
terminal sont conservées telles quelles.*

La commande `pido` permet de modifier le mode et la résistance de tirage d'une
broche, de lire ou d'écrire des états logiques ou analogiques (PWM), etc.

Sur une Raspberry Pi modèle B, elle permet par exemple d'obtenir :

```text
$ pido readall
                                    P1 (#1)
+-----+-----+----------+------+---+----++----+---+------+----------+-----+-----+
| sOc | iNo |   Name   | Mode | V | Ph || Ph | V | Mode |   Name   | iNo | sOc |
+-----+-----+----------+------+---+----++----+---+------+----------+-----+-----+
|     |     |     3.3V |      |   |  1 || 2  |   |      | 5V       |     |     |
|   2 |   8 |    GPIO2 |   IN | 1 |  3 || 4  |   |      | 5V       |     |     |
|   3 |   9 |    GPIO3 |   IN | 1 |  5 || 6  |   |      | GND      |     |     |
|   4 |   7 |    GPIO4 |   IN | 1 |  7 || 8  | 1 | ALT0 | TXD0     | 15  | 14  |
|     |     |      GND |      |   |  9 || 10 | 1 | ALT0 | RXD0     | 16  | 15  |
|  17 |   0 |   GPIO17 |   IN | 0 | 11 || 12 | 0 | IN   | GPIO18   | 1   | 18  |
|  27 |   2 |   GPIO27 |   IN | 0 | 13 || 14 |   |      | GND      |     |     |
|  22 |   3 |   GPIO22 |   IN | 0 | 15 || 16 | 0 | IN   | GPIO23   | 4   | 23  |
|     |     |     3.3V |      |   | 17 || 18 | 0 | IN   | GPIO24   | 5   | 24  |
|  10 |  12 |   GPIO10 |   IN | 0 | 19 || 20 |   |      | GND      |     |     |
|   9 |  13 |    GPIO9 |   IN | 0 | 21 || 22 | 0 | IN   | GPIO25   | 6   | 25  |
|  11 |  14 |   GPIO11 |   IN | 0 | 23 || 24 | 1 | IN   | GPIO8    | 10  | 8   |
|     |     |      GND |      |   | 25 || 26 | 1 | IN   | GPIO7    | 11  | 7   |
+-----+-----+----------+------+---+----++----+---+------+----------+-----+-----+
| sOc | iNo |   Name   | Mode | V | Ph || Ph | V | Mode |   Name   | iNo | sOc |
+-----+-----+----------+------+---+----++----+---+------+----------+-----+-----+

                                    P5 (#2)
+-----+-----+----------+------+---+----++----+---+------+----------+-----+-----+
| sOc | iNo |   Name   | Mode | V | Ph || Ph | V | Mode |   Name   | iNo | sOc |
+-----+-----+----------+------+---+----++----+---+------+----------+-----+-----+
|     |     |       5V |      |   |  1 || 2  |   |      | 3.3V     |     |     |
|  28 |  17 |   GPIO28 |   IN | 0 |  3 || 4  | 0 | IN   | GPIO29   | 18  | 29  |
|  30 |  19 |   GPIO30 |   IN | 0 |  5 || 6  | 0 | IN   | GPIO31   | 20  | 31  |
|     |     |      GND |      |   |  7 || 8  |   |      | GND      |     |     |
+-----+-----+----------+------+---+----++----+---+------+----------+-----+-----+
```

Sur une NanoPi Neo Plus 2, on peut afficher par exemple :

```text
$ pido readall
                                          CON1 (#1)
+-----+-----+----------+------+------+---+----++----+---+------+------+----------+-----+-----+
| sOc | iNo |   Name   | Mode | Pull | V | Ph || Ph | V | Pull | Mode |   Name   | iNo | sOc |
+-----+-----+----------+------+------+---+----++----+---+------+------+----------+-----+-----+
|     |     |     3.3V |      |      |   |  1 || 2  |   |      |      | 5V       |     |     |
|  12 |   8 |  I2C0SDA | ALT2 |  OFF |   |  3 || 4  |   |      |      | 5V       |     |     |
|  11 |   9 |  I2C0SCK | ALT2 |  OFF |   |  5 || 6  |   |      |      | GND      |     |     |
|  91 |   7 |  GPIOG11 |  OFF |  OFF |   |  7 || 8  |   | OFF  | ALT2 | UART1TX  | 15  | 86  |
|     |     |      GND |      |      |   |  9 || 10 |   | OFF  | ALT2 | UART1RX  | 16  | 87  |
|   0 |   0 |   GPIOA0 |  OFF |  OFF |   | 11 || 12 |   | OFF  | OFF  | GPIOA6   | 1   | 6   |
|   2 |   2 |   GPIOA2 |  OFF |  OFF |   | 13 || 14 |   |      |      | GND      |     |     |
|   3 |   3 |   GPIOA3 |  OFF |  OFF |   | 15 || 16 |   | OFF  | OFF  | GPIOG8   | 4   | 88  |
|     |     |     3.3V |      |      |   | 17 || 18 |   | OFF  | OFF  | GPIOG9   | 5   | 89  |
|  22 |  12 |   GPIOC0 |  OFF |  OFF |   | 19 || 20 |   |      |      | GND      |     |     |
|  23 |  13 |   GPIOC1 |  OFF |  OFF |   | 21 || 22 |   | OFF  | OFF  | GPIOA1   | 6   | 1   |
|  24 |  14 |   GPIOC2 |  OFF |  OFF |   | 23 || 24 |   | UP   | OFF  | GPIOC3   | 10  | 25  |
+-----+-----+----------+------+------+---+----++----+---+------+------+----------+-----+-----+
| sOc | iNo |   Name   | Mode | Pull | V | Ph || Ph | V | Pull | Mode |   Name   | iNo | sOc |
+-----+-----+----------+------+------+---+----++----+---+------+------+----------+-----+-----+

                 DBG_UART (#2)
+-----+-----+----------+------+------+---+----+
| sOc | iNo |   Name   | Mode | Pull | V | Ph |
+-----+-----+----------+------+------+---+----+
|     |     |      GND |      |      |   |  1 |
|     |     |       5V |      |      |   |  2 |
|   4 |  17 |  UART0TX | ALT2 |  OFF |   |  3 |
|   5 |  18 |  UART0RX | ALT2 |   UP |   |  4 |
+-----+-----+----------+------+------+---+----+

                   INNER (#3)
+-----+-----+----------+------+------+---+----+
| sOc | iNo |   Name   | Mode | Pull | V | Ph |
+-----+-----+----------+------+------+---+----+
|  10 |  19 |  GPIOA10 |  OFF |  OFF |   |  1 |
| 104 |  32 |  PWR_LED |  OUT |  OFF | 1 |  2 |
+-----+-----+----------+------+------+---+----+

                   CON2 (#4)
+-----+-----+----------+------+------+---+----+
| sOc | iNo |   Name   | Mode | Pull | V | Ph |
+-----+-----+----------+------+------+---+----+
|     |     |       5V |      |      |   |  1 |
|     |     |  USB-DP1 |      |      |   |  2 |
|     |     |  USB-DM1 |      |      |   |  3 |
|     |     |  USB-DP2 |      |      |   |  4 |
|     |     |  USB-DM2 |      |      |   |  5 |
| 105 |  20 |  GPIOL11 |  OFF |  OFF |   |  6 |
|  17 |  11 |  GPIOA17 |  OFF |  OFF |   |  7 |
|  18 |  31 |  GPIOA18 |  OFF |  OFF |   |  8 |
|  19 |  30 |  GPIOA19 |  OFF |  OFF |   |  9 |
|  20 |  21 |  GPIOA20 |  OUT |  OFF | 0 | 10 |
|  21 |  22 |  GPIOA21 |  OFF |  OFF |   | 11 |
|     |     |      GND |      |      |   | 12 |
+-----+-----+----------+------+------+---+----+
| sOc | iNo |   Name   | Mode | Pull | V | Ph |
+-----+-----+----------+------+------+---+----+
```

Comme on peut le voir ci-dessus, la NanoPi Neo Plus 2 possède 4 « connecteurs ».
Le connecteur `INNER` correspond à des signaux internes à la carte, qui peuvent
être utiles à manipuler (ici, on y trouve le signal de la LED ON et celui de la
LED STATUS (GPIOA10)).

Notez aussi que, dans le cas de la NanoPi, la commande `readall` affiche une
colonne `Pull` qui indique l'état de la résistance de tirage (cette
fonctionnalité n'est pas disponible sur une Raspberry Pi, car le BCM2835 ne sait
pas le faire).

On peut indiquer à la commande `readall` le numéro du connecteur à afficher (ce
numéro figure au-dessus de son tableau, après le `#`), par exemple :

```text
$ pido readall 1
                                          CON1 (#1)
+-----+-----+----------+------+------+---+----++----+---+------+------+----------+-----+-----+
| sOc | iNo |   Name   | Mode | Pull | V | Ph || Ph | V | Pull | Mode |   Name   | iNo | sOc |
+-----+-----+----------+------+------+---+----++----+---+------+------+----------+-----+-----+
|     |     |     3.3V |      |      |   |  1 || 2  |   |      |      | 5V       |     |     |
|  12 |   8 |  I2C0SDA | ALT2 |  OFF |   |  3 || 4  |   |      |      | 5V       |     |     |
|  11 |   9 |  I2C0SCK | ALT2 |  OFF |   |  5 || 6  |   |      |      | GND      |     |     |
|  91 |   7 |  GPIOG11 |  OFF |  OFF |   |  7 || 8  |   | OFF  | ALT2 | UART1TX  | 15  | 86  |
|     |     |      GND |      |      |   |  9 || 10 |   | OFF  | ALT2 | UART1RX  | 16  | 87  |
|   0 |   0 |   GPIOA0 |  OFF |  OFF |   | 11 || 12 |   | OFF  | OFF  | GPIOA6   | 1   | 6   |
|   2 |   2 |   GPIOA2 |  OFF |  OFF |   | 13 || 14 |   |      |      | GND      |     |     |
|   3 |   3 |   GPIOA3 |  OFF |  OFF |   | 15 || 16 |   | OFF  | OFF  | GPIOG8   | 4   | 88  |
|     |     |     3.3V |      |      |   | 17 || 18 |   | OFF  | OFF  | GPIOG9   | 5   | 89  |
|  22 |  12 |   GPIOC0 |  OFF |  OFF |   | 19 || 20 |   |      |      | GND      |     |     |
|  23 |  13 |   GPIOC1 |  OFF |  OFF |   | 21 || 22 |   | OFF  | OFF  | GPIOA1   | 6   | 1   |
|  24 |  14 |   GPIOC2 |  OFF |  OFF |   | 23 || 24 |   | UP   | OFF  | GPIOC3   | 10  | 25  |
+-----+-----+----------+------+------+---+----++----+---+------+------+----------+-----+-----+
| sOc | iNo |   Name   | Mode | Pull | V | Ph || Ph | V | Pull | Mode |   Name   | iNo | sOc |
+-----+-----+----------+------+------+---+----++----+---+------+------+----------+-----+-----+
```

Pour mettre la broche numéro 0 en sortie :

```text
$ pido mode 0 out
```

Par défaut, c'est la numérotation de la colonne `iNo` qui est utilisée, mais on
peut aussi désigner le signal `0` par `1.11` :

```text
$ pido mode 1.11 out
```

Cette notation `C.N` permet de désigner rapidement la broche `N` (ici 11) du
connecteur `C` (ici 1).

Pour mettre cette sortie à l'état haut :

```text
$ pido write 0 1
```

Pour la mettre à l'état bas :

```text
$ pido write 0 0
```

On peut aussi inverser son état :

```text
$ pido toggle 0
```

Ou générer un signal carré sur la broche :

```text
$ pido blink 0 100
```

Pour la mettre en entrée avec une résistance de tirage vers le haut (pull-up) :

```text
$ pido mode 0 in
$ pido pull 0 up
```

Et pour la lire :

```text
$ pido read 0
```

On peut aussi attendre un front descendant sur cette entrée :

```text
$ pido wfi 0 falling
```

Voir aussi la page de manuel `pido(1)`.
