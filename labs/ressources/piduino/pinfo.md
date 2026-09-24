# pinfo(1) — informations sur la carte Pi

*Traduction française de la page de manuel `pinfo(1)` de
[piduino](https://github.com/epsilonrt/piduino), d'après le fichier source
[`utils/pinfo/pinfo.1.in`](https://github.com/epsilonrt/piduino/blob/10754af/utils/pinfo/pinfo.1.in)
(commit 10754af, août 2025).*

## Nom

**pinfo** — permet à l'utilisateur d'obtenir des informations sur la carte Pi.

## Synopsis

```text
pinfo [-alMnfisrtgmpbIPSvwh]
```

## Description

**pinfo** est un outil en ligne de commande qui permet à l'utilisateur d'obtenir
des informations sur la carte Pi.

**pinfo** s'appuie sur la bibliothèque piduino
<https://github.com/epsilonrt/piduino>. La détection du modèle de carte est
automatique et utilise une base de données qui permet à un simple utilisateur
d'ajouter une nouvelle « variante » de carte Pi **sans** programmer.

Si une seule information est demandée, elle est affichée sans mise en forme ;
sinon, chaque information est affichée sur une ligne précédée de son nom.

## Options

**-a**, **--all**
: Affiche toutes les informations sur la carte Pi (comportement par défaut).

**-l**, **--list**
: Affiche la liste de toutes les cartes disponibles dans la base de données.

**-M**, **--model**
: Affiche le nom du modèle de la carte (raspberrypi3, nanopi2, …).

**-n**, **--name**
: Affiche le nom « lisible » de la carte.

**-f**, **--family**
: Affiche la famille de la carte (raspberrypi, nanopi, …).

**-i**, **--id**
: Affiche l'identifiant dans la base de données piduino (pour le débogage).

**-s**, **--soc**
: Affiche le modèle de SoC (bcm2708, …).

**-r**, **--revision**
: Affiche le numéro de révision de la carte en hexadécimal (préfixé par 0x).

**-t**, **--tag**
: Affiche l'étiquette d'identification de la carte.

**-g**, **--gpio**
: Affiche le numéro de révision du GPIO en décimal.

**-m**, **--mem**
: Affiche la taille de la RAM en mégaoctets.

**-p**, **--pcb**
: Affiche le numéro de révision du circuit imprimé sous la forme M.m.

**-b**, **--builder**
: Affiche le nom du fabricant.

**-I**, **--i2c**
: Affiche les bus I²C disponibles sur le SoC.

**-P**, **--spi**
: Affiche les bus SPI disponibles sur le SoC.

**-S**, **--serial**
: Affiche les ports série disponibles sur le SoC.

**-v**, **--version**
: Affiche la version de piduino.

**-w**, **--warranty**
: Affiche l'avis de garantie.

**-h**, **--help**
: Affiche un bref résumé d'utilisation.

## Valeur de retour

Renvoie 0 si la carte a été trouvée, 1 sinon.

## Environnement

`PIDUINO_CONN_INFO`
: Si cette variable est définie, elle permet à l'utilisateur d'imposer
  l'emplacement de la base de données PiDuino utilisée par pinfo. SQLite3,
  MySQL, PostgreSQL et ODBC sont pris en charge d'emblée. La syntaxe est décrite
  sur <http://cppcms.com/sql/cppdb/connstr.html>.

## Fichiers

`/etc/piduino.conf`
: Fichier de configuration de PiDuino, pour imposer le modèle de carte à
  utiliser ou indiquer l'emplacement de la base de données PiDuino.

`<répertoire des données>/piduino.db`
: Fichier de base de données SQLite 3 local de PiDuino, utilisé par défaut. Le
  répertoire dépend de l'installation.

## Voir aussi

[pido(1)](pido.md)

Page du wiki de PiDuino : <https://github.com/epsilonrt/piduino/wiki/PiDuino>

## Signaler des bogues

Merci de signaler les bogues sur <https://github.com/epsilonrt/piduino/issues>.

## Auteur

Pascal JEAN, alias epsilonrt

## Copyright

Copyright (c) 2018-2025 Pascal JEAN. Ce logiciel est libre ; voir les sources
pour les conditions de copie. Il n'y a AUCUNE garantie, pas même de
COMMERCIALISATION ou d'ADÉQUATION À UN USAGE PARTICULIER.
