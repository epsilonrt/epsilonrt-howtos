# pinfo — informations sur la carte

*Traduction française de la page [Pinfo](https://github.com/epsilonrt/piduino/wiki/Pinfo)
du wiki de [piduino](https://github.com/epsilonrt/piduino). Les sorties de
terminal sont conservées telles quelles.*

La commande `pinfo` donne les informations sur la carte Pi.

Sur une Raspberry Pi modèle B :

```text
$ pinfo
Name            : RaspberryPi B
Family          : RaspberryPi
Database Id     : 9
Manufacturer    : Sony
Board Revision  : 0xe
SoC             : Bcm2708 (Broadcom)
Memory          : 512MB
GPIO Id         : 2
PCB Revision    : 2
Serial Ports    : /dev/ttyAMA0
```

Sur une NanoPi Neo Plus 2 :

```text
$ pinfo
Name            : NanoPi Neo+ 2
Family          : NanoPi
Database Id     : 36
Manufacturer    : Friendly ARM
Board Tag       : nanopineoplus2
SoC             : H5 (Allwinner)
Memory          : 1024MB
GPIO Id         : 4
I2C Buses       : /dev/i2c-0
Serial Ports    : /dev/ttyS0,/dev/ttyS1
```

Voir aussi la page de manuel `pinfo(1)`.
