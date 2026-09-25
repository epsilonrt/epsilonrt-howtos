# Les convertisseurs de piduino : `pido` et la classe `Converter`

Un **convertisseur** est un composant qui fait le lien entre le monde analogique
et le monde numérique : un CAN (convertisseur analogique-numérique) mesure une
tension, un CNA fait l'inverse. piduino étend cette notion aux composants qui se
pilotent de la même façon : un **expandeur de GPIO** (des broches
d'entrée-sortie supplémentaires, accessibles par un bus) se lit et s'écrit,
canal par canal, comme un convertisseur.

Tous ces composants sont utilisables par **la même interface** :

- en ligne de commande, avec l'option `-c` de `pido` ;
- dans un programme C++, avec la classe `Converter`.

Ce document présente cette interface, puis les deux composants I²C de la carte
d'extension **RPi Extended** utilisée en TP : le CAN **MAX11615** et l'expandeur
de GPIO **MAX7311**. Il se termine par la programmation avec la classe
`Converter`.

Les composants sont sur le bus I²C : l'activation du bus et l'adressage sont
expliqués dans [i2c.md](../i2c.md).

## 1. L'interface des convertisseurs dans `pido`

### Lister les convertisseurs

```bash
pido converters
```

La commande affiche le nom, le type et les paramètres de chaque convertisseur
disponible. Voici les deux lignes qui nous intéressent :

```
Name                Type      Parameters
--------------------------------------------------------------------------------
max1161x            adc       bus=id:max={12,13,14,15,16,17}:ref={ext,vdd,int1,int2,int3,int4}:fsr=value:bipolar={1,0}:clk={int,ext}
max7311             gpioexp   bus=id:addr={0x20...0xDE}:bustimeout={0,1}
```

### La syntaxe

On désigne le convertisseur avec l'option `-c`, suivie de son nom et de ses
paramètres. Les paramètres sont séparés par des deux-points, chacun sous la
forme `nom=valeur` :

```
pido -c <convertisseur>[:<paramètre>=<valeur>[:<paramètre>=<valeur>...]] [options] <commande> [canal] [valeur]
```

Exemple : `pido -c max1161x:bus=1:ref=int4 -m cread 0`. Les paramètres omis
prennent leur valeur par défaut. Le **canal** est un numéro qui commence à 0.

### Les commandes

| Commande | Convertisseurs | Effet |
|---|---|---|
| `cread [canal]` | CAN, capteurs | lit une valeur |
| `cwrite [canal] valeur` | CNA | écrit une valeur |
| `mode [canal] in`, `out`… | expandeurs | règle le mode d'un canal (ou de tous, si le canal est omis) |
| `read [canal]` | expandeurs | lit un canal (ou tous, en hexadécimal) |
| `write [canal] valeur` | expandeurs | écrit un canal (ou tous à la fois) |
| `toggle [canal]` | expandeurs | inverse l'état d'un canal |
| `blink canal` | expandeurs | fait clignoter un canal |
| `readall` | expandeurs | lit tous les canaux (en hexadécimal) |

Les commandes `mode`, `read`, `write`, `toggle`, `blink` et `readall` sont les
mêmes que pour les broches GPIO de la carte ; avec `-c`, elles agissent sur le
convertisseur au lieu des broches. Voir [pido](pido.md) pour la description
complète.

### Les options utiles

| Option | Effet |
|---|---|
| `-c convertisseur[:paramètres]` | choisit le convertisseur |
| `-m` | affiche la valeur en grandeur analogique (en volts) au lieu de la valeur numérique |
| `-a` | fait une moyenne sur plusieurs échantillons |
| `-d` | lecture différentielle (différence entre deux entrées) |
| `-x` | affiche en hexadécimal |
| `-p période` | période de clignotement pour `blink`, en millisecondes |

## 2. Le MAX11615 : convertisseur analogique-numérique

Le MAX11615 est un CAN de **12 bits** (4096 valeurs) à **8 entrées** analogiques,
commandé en I²C à l'adresse **0x33** (l'adresse est fixée par le composant, on
ne la donne pas).

### Câblage sur la carte RPi Extended

Les entrées sont disponibles sur le connecteur **J3** (2 × 8 broches). Ce sont
les broches impaires :

| Broche de J3 | Entrée | Canal |
|---|---|---|
| 1 | AIN0 | 0 |
| 3 | AIN1 | 1 |
| 5 | AIN2 | 2 |
| 7 | AIN3 | 3 |
| 9 | AIN4 | 4 |
| 11 | AIN5 | 5 |
| 13 | AIN6 | 6 |
| 15 | VIN7 | 7 |

Parmi les broches paires, les broches 8 à 16 sont reliées à la masse et la
broche 2 à la broche REF du composant (voir plus loin).

### La tension de référence

La valeur numérique lue dépend de la **tension de référence** : la pleine
échelle du CAN. Elle se choisit avec le paramètre `ref` :

| `ref=` | Référence | Broche REF |
|---|---|---|
| `vdd` (par défaut) | l'alimentation du composant | inutilisée |
| `ext` | une tension externe (de 1 V à VDD), à préciser avec `fsr=` | entrée |
| `int1` | référence interne de 2,048 V | inutilisée, référence coupée entre les mesures |
| `int2` | référence interne de 2,048 V | inutilisée, référence toujours active |
| `int3` | référence interne de 2,048 V | sortie, référence coupée entre les mesures |
| `int4` | référence interne de 2,048 V | sortie, référence toujours active |

Sur la carte, la broche REF est découplée par un condensateur de 100 nF (C8) :
on peut utiliser la référence interne avec `ref=int4`. La pleine échelle est
alors de **2,048 V** : une tension d'entrée de 0 à 2,048 V donne une valeur de
0 à 4095, soit 0,5 mV par pas. **Une tension supérieure à la référence donne
la valeur maximale** ; ne dépassez jamais la tension d'alimentation (3,3 V).

Les autres paramètres :

| Paramètre | Rôle |
|---|---|
| `bus=numéro` | numéro du bus I²C (le bus par défaut si omis) |
| `max=numéro` | modèle de la famille (12 à 17), 15 par défaut pour le MAX11615 |
| `fsr=valeur` | pleine échelle en volts (obligatoire avec `ref=ext`) |
| `bipolar=1` | mode bipolaire, pour les mesures différentielles |
| `clk=int` ou `clk=ext` | horloge interne (par défaut) ou externe |

### Mesurer une tension

Pour lire la valeur numérique du canal 0 :

```bash
pido -c max1161x:bus=1:ref=int4 cread 0
```

On obtient un nombre entre 0 et 4095. Avec `-m`, `pido` la convertit en volts :

```bash
pido -c max1161x:bus=1:ref=int4 -m cread 0
```

La conversion est : **tension = valeur × pleine échelle / 4096**. Par exemple,
la valeur 1024 correspond à 1024 × 2,048 / 4096 = **0,512 V**.

Pour réduire le bruit, on moyenne plusieurs mesures avec `-a` :

```bash
pido -c max1161x:bus=1:ref=int4 -a -m cread 0
```

Pour mesurer la **différence** entre deux entrées, on utilise `-d` :

```bash
pido -c max1161x:bus=1:ref=int4 -d -m cread 0
```

Le canal 0 mesure alors AIN0 − AIN1 et le canal 1 mesure AIN1 − AIN0 ; les
canaux suivants procèdent de même pour les entrées suivantes deux à deux
(voir la table de canaux du datasheet du MAX11612 à MAX11617). Avec `bipolar=1`,
la tension différentielle peut être négative.

## 3. Le MAX7311 : expandeur de GPIO

Le MAX7311 ajoute **16 broches d'entrée-sortie** (IO0 à IO15) commandées en I²C.
Au démarrage, elles sont toutes en **entrées**. Chaque broche peut être
configurée en entrée ou en sortie.

### Câblage sur la carte RPi Extended

Le composant est à l'adresse **0x20** (ses trois broches d'adresse AD0, AD1 et
AD2 sont à la masse). Ses 16 broches sont accessibles sur deux connecteurs
de 8 broches :

| Connecteur | Broches | Entrées-sorties | Canaux |
|---|---|---|---|
| J5 (PIO1) | 1 à 8 | IO0 à IO7 | 0 à 7 |
| J6 (PIO2) | 1 à 8 | IO8 à IO15 | 8 à 15 |

La sortie d'interruption du composant est reliée à une broche de la carte
Raspberry Pi (GPIO_GEN6).

Le paramètre `bus` choisit le bus I²C. L'adresse par défaut du composant est
0x20 : c'est celle de la carte, il n'y a donc pas besoin de la préciser. Le
paramètre `bustimeout` (1 par défaut) active le délai de sécurité du composant
en cas de blocage du bus.

### Piloter les broches

Au démarrage, tous les canaux sont des entrées : `write` ne renvoie pas
d'erreur, mais il ne modifie pas la broche. Il faut d'abord passer le canal en
sortie.

> **Limitation de la version actuelle de `pido`.** La commande `mode` ne
> permet pas de changer le mode d'un canal du MAX7311 : elle échoue avec le
> message « Unable to set/get mode on converter » (avec l'option `-D`, la cause
> apparaît : « Input has always been pulled up »). Les canaux restent donc en
> entrée, et `write` n'a aucun effet sur la LED.

Trois solutions permettent de piloter une broche malgré tout :

- la commande `mode out` **sans numéro de canal**, qui passe les 16 canaux en
  sortie d'un coup (elle fonctionne, contrairement à `mode 0 out`). Attention :
  les 16 broches deviennent des sorties, vérifiez qu'aucune n'est reliée à une
  autre sortie :

  ```bash
  pido -c max7311:bus=1 mode out
  pido -c max7311:bus=1 write 0 1
  pido -c max7311:bus=1 write 0 0
  ```

- la commande `blink`, qui configure elle-même le canal en sortie (voir plus
  bas) ;
- l'écriture directe dans le registre de configuration du composant, avec les
  `i2c-tools` (voir [i2c.md](../i2c.md)). Le registre 0x06 configure IO0 à IO7 :
  un bit à 1 est une entrée, un bit à 0 une sortie. Pour passer **uniquement**
  IO0 en sortie, on écrit 0xFE, puis `write` fonctionne :

  ```bash
  i2cset -y 1 0x20 0x06 0xfe
  pido -c max7311:bus=1 write 0 1
  pido -c max7311:bus=1 write 0 0
  ```

  Pour remettre IO0 en entrée : `i2cset -y 1 0x20 0x06 0xff`.

Pour lire le mode d'un canal et l'état d'une entrée :

```bash
pido -c max7311:bus=1 mode 1
pido -c max7311:bus=1 read 1
```

Le mode d'un canal en entrée s'affiche `in up` : les entrées du MAX7311 ont une
résistance de tirage vers le haut, qu'on ne peut pas désactiver. Pour une
lecture de toutes les broches d'un coup, on omet le canal :

```bash
pido -c max7311:bus=1 read
```

`pido` affiche alors une valeur hexadécimale de 16 bits : le bit 0 est l'entrée
IO0, le bit 15 l'entrée IO15.

Les modes disponibles sont `in`, `out` et `activelow` (ce dernier inverse le
niveau lu ou écrit).

La commande `blink` configure le canal en sortie, puis l'inverse à chaque
demi-période. Pour faire clignoter IO0 avec une période de 500 ms (Ctrl+C pour
arrêter) :

```bash
pido -c max7311:bus=1 -p 500 blink 0
```

À l'arrêt, `pido` essaie de remettre le canal dans son mode d'origine : il
échoue pour la même raison que `mode` (message « Unable to set/get mode on
converter »), et le canal reste donc configuré en sortie, à l'état bas. La LED
s'éteint.

**Prudence :** pour voir le résultat, connectez une LED en série avec une
résistance (par exemple 330 Ω) entre IO0 (broche 1 de J5) et la masse de la
carte. Ne branchez jamais une LED sans résistance.

## 4. Programmer avec la classe `Converter`

La classe `Converter` de piduino donne la même interface à tous les
convertisseurs. Un programme peut donc utiliser un CAN, un CNA ou un expandeur
sans dépendre du composant précis : il suffit de changer la chaîne de
description passée à la fonction `Converter::factory ()`, qui utilise la même
syntaxe que l'option `-c` de `pido`.

```cpp
#include <Piduino.h>
#include <Converters.h>
```

### Les méthodes principales

| Méthode | Rôle |
|---|---|
| `Converter::factory ("nom:paramètres")` | crée le convertisseur décrit par la chaîne |
| `open ()`, `close ()` | ouvre et ferme le composant ; `open ()` renvoie `false` en cas d'échec |
| `readChannel (canal)` | lit la valeur numérique d'un canal (0 à 4095 pour le MAX11615, 0 ou 1 pour un canal de MAX7311) |
| `digitalToValue (valeur)` | convertit une valeur numérique en grandeur analogique (en volts) |
| `readValue (canal)` | lit directement en volts |
| `readAverage (canal, différentiel, nombre)` | moyenne de plusieurs lectures |
| `setMode (mode, canal)` | règle le mode d'un canal : `Converter::DigitalOutput`, `Converter::DigitalInput`, `Converter::ActiveLow` |
| `writeChannel (valeur, canal)` | écrit un canal |
| `toggle (canal)` | inverse un canal |
| `read ()`, `write (valeur)` | lit ou écrit tous les canaux d'un coup |
| `numberOfChannels ()`, `resolution ()`, `fullScaleRange ()` | caractéristiques du composant |

### Exemple 1 : mesurer une tension avec le MAX11615 (`AdcSimple`)

Le programme suivant est l'exemple
[`AdcSimple`](https://github.com/epsilonrt/piduino/blob/master/examples/Converters/AdcSimple/main.cpp)
de piduino. Il mesure la tension du canal 0 du MAX11615 toutes les secondes et
l'affiche dans la console.

```cpp
#include <Piduino.h>
#include <Converters.h>

// Create a MAX11615 ADC instance
std::unique_ptr<Converter> adc (Converter::factory ("max1161x:bus=1:max=15:ref=int4"));

void setup() {

  Console.begin (115200);

  if (!adc->open()) {
    Console.println ("Failed to open ADC");
    exit (EXIT_FAILURE);
  }
}

void loop() {
  // Read digital value from channel 0
  long digitalValue = adc->readChannel (0);

  // Convert to analog voltage
  double voltage = adc->digitalToValue (digitalValue);

  Console.print ("Channel 0: ");
  Console.print (digitalValue);
  Console.print (" (");
  Console.print (voltage);
  Console.println ("V)");

  delay (1000);
}
```

Exemple d'affichage, avec une entrée non connectée (la valeur varie librement) :

```
Channel 0: 2492 (1.25V)
Channel 0: 2019 (1.01V)
Channel 0: 1642 (0.82V)
```

Ce que fait le programme :

- **`Converter::factory (...)`** construit le convertisseur à partir de la
  chaîne `max1161x:bus=1:max=15:ref=int4` : un MAX11615 sur le bus 1, avec la
  référence interne de 2,048 V. C'est la chaîne qu'on passerait à `pido -c`.
  Le pointeur intelligent `std::unique_ptr` libère l'objet à la fin du
  programme.
- **`adc->open ()`** ouvre le composant ; si le composant ne répond pas (bus
  non activé, mauvais câblage), il renvoie `false` et le programme s'arrête.
- **`adc->readChannel (0)`** lit la valeur numérique du canal 0 (de 0 à 4095).
- **`adc->digitalToValue (digitalValue)`** la convertit en volts, avec la
  pleine échelle du composant (2,048 V ici) : c'est la formule
  valeur × pleine échelle / 4096 vue plus haut.

### Exemple 2 : faire clignoter une LED avec le MAX7311

Le même schéma (créer, ouvrir, utiliser) sert pour l'expandeur de GPIO. Ce
programme fait clignoter une LED connectée au canal 0 du MAX7311, c'est-à-dire
IO0, la broche 1 de J5. Il suit le modèle de l'exemple `Blink` d'Arduino.

```cpp
#include <Piduino.h>
#include <Converters.h>

const int ledChannel = 0; // IO0 of the MAX7311 (pin 1 of J5)

// Create a MAX7311 GPIO expander instance (default address 0x20)
std::unique_ptr<Converter> expander (Converter::factory ("max7311:bus=1"));

void setup() {

  Console.begin (115200);

  if (!expander->open()) {
    Console.println ("Failed to open the MAX7311");
    exit (EXIT_FAILURE);
  }

  // Configure the channel as an output
  if (!expander->setMode (Converter::DigitalOutput, ledChannel)) {
    Console.println ("Failed to set the channel as an output");
    exit (EXIT_FAILURE);
  }
}

void loop() {

  expander->writeChannel (1, ledChannel); // LED on
  delay (500);
  expander->writeChannel (0, ledChannel); // LED off
  delay (500);
}
```

Ce que fait le programme :

- **`Converter::factory ("max7311:bus=1")`** construit l'expandeur ; l'adresse
  0x20 est celle par défaut.
- **`setMode (Converter::DigitalOutput, ledChannel)`** configure le canal en
  sortie (les broches sont en entrées au démarrage).
- **`writeChannel (1, ledChannel)`** et **`writeChannel (0, ledChannel)`**
  allument et éteignent la LED. On pourrait remplacer les deux lignes par un
  seul `expander->toggle (ledChannel)`, qui inverse l'état du canal.

Comparez avec l'exemple 1 : le CAN et l'expandeur sont manipulés avec les
mêmes méthodes (`factory`, `open`, `readChannel` ou `writeChannel`). C'est
l'intérêt de la classe `Converter` : changer de composant revient surtout à
changer la chaîne de description.

À l'arrêt du programme, le canal reste configuré en sortie : il n'est pas
remis en entrée.

### Compiler et exécuter

Ces programmes se compilent comme les autres exemples de piduino, avec CMake :
le fichier `CMakeLists.txt` de l'exemple `AdcSimple` convient, en changeant le
nom du projet. Pour un programme d'un seul fichier, on peut aussi appeler
directement le compilateur, en demandant à `pkg-config` les options de
piduino :

```bash
g++ -std=c++11 blink.cpp -o blink $(pkg-config --cflags --libs piduino)
```

Les deux programmes de ce document ont été compilés et exécutés sur un Compute
Module 5 avec piduino 0.7.3 : la valeur du canal 0 du MAX11615 s'affiche chaque
seconde, et le canal 0 du MAX7311 clignote.

## 5. À retenir

- Un **convertisseur** piduino (CAN, CNA, expandeur de GPIO, capteur) s'utilise
  par la même interface : avec `pido -c nom:paramètres`, ou en C++ avec la
  classe `Converter`.
- `pido converters` liste les convertisseurs et leurs paramètres. Les
  paramètres se notent `nom=valeur`, séparés par des deux-points.
- **MAX11615** (adresse 0x33, connecteur J3) : `pido -c max1161x:ref=int4 -m cread 0`
  lit l'entrée AIN0 en volts. La référence interne de 2,048 V est choisie par
  `ref=int4`.
- **MAX7311** (adresse 0x20, connecteurs J5 et J6) : `read` lit une broche,
  `blink` la fait clignoter et `write` l'écrit une fois qu'elle est configurée
  en sortie. Dans la version actuelle de `pido`, la commande `mode` ne peut pas
  la passer en sortie : on passe par `blink`, par un programme, ou par le
  registre 0x06 avec `i2cset`.
- En programmation : `Converter::factory ()` crée le composant, `open ()`
  l'ouvre, puis `readChannel ()` ou `writeChannel ()` le lit ou l'écrit.
