# epsilonrt-howtos

*[English version](README.md)*

Ce dépôt centralise mes notes expliquant comment accomplir un certain nombre de
tâches et résoudre différents problèmes.

## À qui ça s'adresse

Ces notes sont écrites d'abord pour la section de techniciens supérieurs
**BTS CIEL** du
[lycée Rouvière – Suzanne Lefort-Rouquette](https://www.lycee-rouviere.fr/index.php/superieur/b-t-s/systemes-numeriques-option-b),
à Toulon.

*CIEL* signifie **Cybersécurité, Informatique et réseaux, Électronique**. La
formation prépare des techniciens qui interviennent aussi bien sur
l'électronique embarquée que sur les réseaux et l'administration système —
c'est-à-dire exactement l'étendue des sujets traités ici.

Cela dit, rien n'est enfermé dans ce cadre. Certains documents supposent un
environnement précis — postes Windows 11 en domaine Active Directory, disque
réseau personnel, conventions de nommage propres aux salles — mais la technique
sous-jacente est générale, et les parties spécifiques sont toujours explicitées.
Si vous arrivez ici par un moteur de recherche, vous êtes le bienvenu.

## Sommaire

| Sujet | Langue |
|---|---|
| [Clés SSH pour le développement à distance](dev/ssh-key/) — créer une paire de clés, la garder utilisable d'un poste partagé à l'autre, la déclarer sur un Raspberry Pi | français |
| [DFRobot FireBeetle 2 ESP32-C6 avec PlatformIO](dev/pio-firebeetle2/) — ajouter la définition de carte et compiler avec le framework Arduino | français |

## À propos des langues

Les documents destinés aux étudiants sont rédigés en **français**, la langue
d'enseignement. Les notes plus générales peuvent être en anglais. Le tableau
ci-dessus précise la langue de chaque entrée.

## Licence

**La documentation, les textes et les illustrations** sont diffusés sous
[Creative Commons Attribution 4.0 International](LICENSE) (CC BY 4.0). Vous
pouvez les partager et les adapter, y compris commercialement, à condition de
créditer la source :

> Pascal JEAN (epsilonrt), lycée Rouvière, Toulon —
> https://github.com/epsilonrt/epsilonrt-howtos

**Les extraits de code et les fichiers de configuration** échappent à cette
obligation : réutilisez-les librement, sans attribution. Les licences Creative
Commons ne sont pas conçues pour du logiciel, et une définition de carte ou
trois lignes de commande n'ont pas à traîner de contraintes derrière elles.

Si quelque chose ici vous fait gagner un après-midi, c'est déjà une raison
suffisante pour que ça existe.
