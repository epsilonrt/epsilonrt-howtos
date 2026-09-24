# pido(1) — accès simple aux broches GPIO

*Traduction française de la page de manuel `pido(1)` de
[piduino](https://github.com/epsilonrt/piduino), d'après le fichier source
[`utils/pido/pido.1.in`](https://github.com/epsilonrt/piduino/blob/10754af/utils/pido/pido.1.in)
(commit 10754af, août 2025).*

## Nom

**pido** — permet à l'utilisateur d'accéder simplement aux broches GPIO, ou à
d'autres composants, comme des périphériques I²C ou SPI (CAN, CNA, capteurs
numériques, expandeurs de GPIO).

## Synopsis

```text
pido [-g1sfDxmad] { mode pin [value] |
                    pull pin [value] |
                    drive pin [value] |
                    write pin value |
                    toggle pin |
                    blink pin [value] |
                    read pin |
                    readall [connector] |
                    wfi pin edge [timeout_ms] |
                    pwm pin [value] |
                    pwmf pin [hz_freq] |
                    pwmr pin [range] |
                    pwrite pin value [range] [frequency] |
                    converters |
                    cwrite -c converter[:parameters] [chan] value |
                    cread -c converter[:parameters] [chan] |
                    -v | -w | -h }
```

## Description

**pido** est un outil en ligne de commande qui donne un accès simple aux
broches GPIO d'une carte Pi. Il est conçu pour des tests et des diagnostics
simples, mais il peut être utilisé dans des scripts shell pour un contrôle des
broches GPIO général, quoique un peu lent.

**pido** s'appuie sur la bibliothèque piduino
<https://github.com/epsilonrt/piduino>. La détection du modèle de carte est
automatique et utilise une base de données : l'utilisateur peut ainsi ajouter
une nouvelle « variante » de carte Pi **sans** programmer.

### Commandes

**mode** *pin* [**in** | **out** | **off** | **pwm** | **alt{0..9}**]
: Sans valeur, donne le mode actuel de la broche.

  Place la broche en mode *entrée* (*in*), *sortie* (*out*), *pwm*,
  *alt{0..9}* ou *off*. La désactivation d'une broche avec *off* n'est
  disponible que sur certains modèles de SoC (voir la fiche technique).

  Les modes ALT peuvent aussi être désignés par *alt0*, *alt1*, … *alt9*. Le
  nombre de modes alternatifs disponibles dépend du modèle de SoC (voir la
  fiche technique).

  Avec l'option **-c**, on peut désigner un expandeur de GPIO (par exemple un
  MCP23017 ou un MAX7311). Dans ce cas :

  - tous les modes ne sont généralement pas disponibles (seulement entrée et
    sortie) ;
  - on peut omettre le numéro de broche pour modifier toutes les broches à la
    fois.

**pull** *pin* [**up** | **down** | **off**]
: Sans valeur, donne l'état actuel de la résistance de tirage. Cette
  fonctionnalité n'est disponible que sur certains modèles de SoC (voir la fiche
  technique).

  Utilisez *up*, *down* ou *off* pour activer la résistance de tirage interne
  vers le haut (pull-up), vers le bas (pull-down), ou la désactiver (état
  haute impédance).

  Avec l'option **-c**, on peut désigner un expandeur de GPIO (par exemple un
  MCP23017 ou un MAX7311) ; dans ce cas, on peut omettre le numéro de broche
  pour modifier toutes les broches à la fois.

**drive** *pin* [**level**]
: Sans valeur, donne le niveau actuel du courant de sortie (drive strength) de
  la broche.

  Utilisez *level* pour régler ce niveau.

  Cette fonctionnalité n'est disponible que sur certains modèles de SoC (voir la
  fiche technique).

**write** *pin* **value**
: Écrit la valeur donnée sur la broche. Il faut d'abord mettre la broche en mode
  sortie.

  Avec l'option **-c**, on peut désigner un expandeur de GPIO (par exemple un
  MCP23017 ou un MAX7311) ; dans ce cas, on peut omettre le numéro de broche
  pour modifier toutes les broches à la fois.

**toggle** *pin*
: Change l'état d'une broche GPIO : de 0 à 1, ou de 1 à 0. Il faut d'abord
  mettre la broche en mode sortie.

  Avec l'option **-c**, on peut désigner un expandeur de GPIO (par exemple un
  MCP23017 ou un MAX7311) ; dans ce cas, on peut omettre le numéro de broche
  pour modifier toutes les broches à la fois.

**blink** *pin*
: Fait clignoter la broche donnée. Appuyez sur Ctrl-C pour quitter. La période
  de clignotement, en millisecondes, se règle avec l'option **-p** ; sa valeur
  par défaut est 1000 (elle ne peut pas être inférieure à 2 ms).

  Remarque : cette commande place explicitement la broche en mode sortie.

  Avec l'option **-c**, on peut désigner un expandeur de GPIO (par exemple un
  MCP23017 ou un MAX7311) ; dans ce cas, on peut omettre le numéro de broche
  pour modifier toutes les broches à la fois.

**read** *pin*
: Lit la valeur numérique de la broche donnée et affiche 0 ou 1 pour représenter
  le niveau logique correspondant.

  Avec l'option **-c**, on peut désigner un expandeur de GPIO (par exemple un
  MCP23017 ou un MAX7311) ; dans ce cas, on peut omettre le numéro de broche
  pour lire toutes les broches à la fois (au format hexadécimal).

**readall** [*connector*]
: Affiche un tableau de la valeur de toutes les broches GPIO. Ces valeurs sont
  les valeurs réellement lues si la broche est en mode entrée, ou la dernière
  valeur écrite si elle est en mode sortie. Tous les connecteurs sont affichés
  par défaut ; pour n'en afficher qu'un seul, indiquez simplement son numéro
  (inscrit au-dessus du tableau).

  Avec l'option **-c**, on peut désigner un expandeur de GPIO (par exemple un
  MCP23017 ou un MAX7311) ; dans ce cas, le numéro de connecteur est ignoré et
  toutes les broches de l'expandeur sont lues (au format hexadécimal).

**wfi** *pin* **rising** | **falling** | **both** [*timeout_ms*]
: Place la broche donnée dans le mode d'interruption indiqué (front montant
  *rising*, front descendant *falling*, ou les deux *both*), puis attend que
  l'interruption se produise. L'attente n'est pas active : elle ne consomme
  aucun temps processeur.

**pwm** *pin* [*value*]
: Écrit une valeur de PWM (de 0 à la plage, *Range*) sur la broche donnée. Si la
  fréquence n'a pas été réglée avec la commande **pwmf**, elle est fixée à
  environ 1000 Hz et la plage à 1024 lors du premier appel (valeurs par
  défaut).

  Sans valeur, lit la valeur de PWM actuelle.

  Remarque : la broche doit disposer d'une fonction PWM matérielle (voir la
  fiche technique) et doit d'abord être mise en mode PWM.

**pwmf** *pin* [*hz_freq*]
: Modifie la fréquence de PWM de la broche donnée. Ce changement peut affecter
  la fréquence des autres broches PWM (voir la fiche technique).

  Sans valeur, lit la fréquence de PWM actuelle.

  Remarque : la broche doit disposer d'une fonction PWM matérielle (voir la
  fiche technique) et doit d'abord être mise en mode PWM.

**pwmr** *pin* [*range*]
: Modifie la plage de PWM de la broche donnée. Ce changement devrait affecter la
  fréquence et le rapport cyclique du signal PWM ; il sera donc probablement
  nécessaire de modifier aussi ces valeurs pour obtenir l'effet voulu (voir la
  fiche technique).

  Sans valeur, lit la plage de PWM actuelle.

  Remarque : la broche doit disposer d'une fonction PWM matérielle (voir la
  fiche technique) et doit d'abord être mise en mode PWM.

**pwrite** *pin* **value** [*range*] [*frequency*]
: Écrit la valeur donnée sur la broche à l'aide d'un PWM logiciel. La valeur
  doit être comprise entre 0 et la plage (1024 par défaut). La fréquence est
  facultative et vaut 200 Hz par défaut. Cette commande est utile pour les
  broches qui n'ont pas de PWM matériel. Elle bloque jusqu'à ce que
  l'utilisateur l'interrompe avec Ctrl-C.

**converters**
: Liste tous les convertisseurs (CAN ou CNA) disponibles, utilisables avec les
  commandes **cwrite** et **cread**.

**cwrite** **-c** *converter[:parameters]* [*chan*] *value*
: Écrit la valeur donnée sur le convertisseur indiqué (CNA). Le convertisseur
  doit être désigné avec l'option **-c** ; on peut y joindre des paramètres tels
  que le numéro de bus, la tension de référence, la pleine échelle, etc. Si le
  convertisseur demande un canal précis, celui-ci doit aussi être fourni.

**cread** **-c** *converter[:parameters]* [*chan*]
: Lit une valeur sur le convertisseur indiqué (CAN ou capteur). Si *chan* n'est
  pas précisé, le canal par défaut est utilisé. Le convertisseur doit être
  désigné avec l'option **-c** ; on peut y joindre des paramètres tels que le
  numéro de bus, la tension de référence, la pleine échelle, etc.

  Les options **-m**, **-a** et **-d** permettent de modifier le comportement
  de la lecture.

## Options

**-g**
: Utilise les numéros de broches du SoC plutôt que ceux de PiDuino.

**-1**
: Utilise les numéros de broches des connecteurs plutôt que ceux de PiDuino. Un
  numéro s'écrit sous la forme C.N ; par exemple, 1.5 désigne la broche 5 du
  connecteur 1.

**-s**
: Utilise les numéros de broches du système plutôt que ceux de PiDuino.

**-D**
: Active le mode débogage.

**-f**
: Force l'utilisation de l'interface de périphérique Gpio2 (`/dev/gpiochipX`)
  pour contrôler les fonctions des broches.

**-p** *\<period_ms\>*
: Règle la période de clignotement en millisecondes (1000 ms par défaut, jamais
  inférieure à 2 ms).

**-x**
: Affiche les valeurs au format hexadécimal.

**-c** *converter[:parameters]*
: Indique le convertisseur à utiliser et ses options (par exemple
  `-c max1161x:bipolar=1`).

**-m**
: Affiche les valeurs du CAN ou du capteur numérique sous forme analogique
  (tension, température, etc.).

**-a**
: Calcule une moyenne sur plusieurs échantillons (leur nombre dépend du
  convertisseur utilisé).

**-d**
: Lit les valeurs du CAN en mode différentiel.

**-v**
: Affiche la version de PiDuino.

**-w**
: Affiche l'avis de garantie.

**-h**
: Affiche un bref résumé d'utilisation.

## Environnement

`PIDUINO_CONN_INFO`
: Si cette variable est définie, elle permet à l'utilisateur d'indiquer
  l'emplacement de la base de données PiDuino utilisée par pido. SQLite3, MySQL,
  PostgreSQL et ODBC sont pris en charge d'emblée. La syntaxe est décrite sur
  <http://cppcms.com/sql/cppdb/connstr.html>.

## Fichiers

`/etc/piduino.conf`
: Fichier de configuration de PiDuino, pour indiquer le modèle de carte à
  utiliser ou l'emplacement de la base de données PiDuino.

`<répertoire des données>/piduino.db`
: Fichier de base de données SQLite 3 local de PiDuino, utilisé par défaut. Le
  répertoire dépend de l'installation.

## Exemples

Les commandes ci-dessous sont suivies de leur effet.

La numérotation physique de la forme C.N, par exemple *1.11*, permet de
désigner rapidement la broche N (ici 11) du connecteur C (ici 1).

Le moyen le plus rapide d'obtenir la liste des différences entre les
numérotations de broches est de lancer la commande `pido readall`.

```text
pido mode 0 out            # Met la broche 0 en sortie
pido mode 1.11 out         # Met la broche 11 du connecteur 1 en sortie (identique à la broche 0 sur NanoPi et Raspberry Pi)
pido write 0 1             # Met la broche 0 à l'état haut
pido toggle 0              # Inverse l'état de la broche 0
pido blink 0 100           # Fait clignoter la broche 0 avec une période de 100 ms
pido mode 0 in             # Met la broche 0 en entrée
pido pull 0 up             # Active la résistance de pull-up de la broche 0
pido read 0                # Lit la broche 0
pido wfi 0 falling         # Attend une interruption sur un front descendant de la broche 0
pido converters            # Liste tous les convertisseurs disponibles
pido -c gpiopwm:18:1024:500 cwrite 0 512
                           # PWM logiciel sur la broche 18, rapport cyclique de 50 %
pido -c max1161x:bus=1:max=15:ref=int4 cread 0
                           # Lit le canal 0 du CAN (MAX11615 sur le bus 1, tension de référence interne de 2,048 V)
pido -c max1161x:bus=1:max=15:bipolar=1 -md cread 0
                           # Lit le CAN en différentiel entre les canaux 0 et 1, en valeur analogique (MAX11615 sur le bus 1, mode bipolaire)
```

## Voir aussi

[pinfo(1)](pinfo.md)

Page du wiki de PiDuino : <https://github.com/epsilonrt/piduino/wiki/PiDuino>

## Signaler des bogues

Merci de signaler les bogues sur <https://github.com/epsilonrt/piduino/issues>.

## Auteur

Pascal JEAN, alias epsilonrt

## Copyright

Copyright (c) 2018-2025 Pascal JEAN. Ce logiciel est libre ; voir les sources
pour les conditions de copie. Il n'y a AUCUNE garantie, pas même de
COMMERCIALISATION ou d'ADÉQUATION À UN USAGE PARTICULIER.
