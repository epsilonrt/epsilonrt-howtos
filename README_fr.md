# epsilonrt-labs

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

Les dépôts de projets des étudiants sont regroupés dans l'organisation
[btsciel-toulon](https://github.com/btsciel-toulon) — pour l'essentiel privés,
puisqu'il s'agit de travaux en cours d'évaluation.

Cela dit, rien n'est enfermé dans ce cadre. Certains documents supposent un
environnement précis — postes Windows 11 en domaine Active Directory, disque
réseau personnel, conventions de nommage propres aux salles — mais la technique
sous-jacente est générale, et les parties spécifiques sont toujours explicitées.
Si vous arrivez ici par un moteur de recherche, vous êtes le bienvenu.

## Sommaire

Deux natures de contenu, volontairement séparées.

### [`labs/`](labs/) — tutoriels et travaux pratiques

Faits pour être lus du début à la fin : ils expliquent le principe avant le
geste, disent pourquoi les choses sont ainsi, et précisent ce qu'on doit voir à
chaque étape.

| Lab | Langue |
|---|---|
| [Clés SSH pour le développement à distance](labs/ssh-key/) — créer une paire de clés, la garder utilisable d'un poste partagé à l'autre, la déclarer sur un Raspberry Pi | français |
| [Git et GitHub pour les projets d'équipe](labs/github/) — principe du contrôle de version, création du compte GitHub, organisation d'un projet (équipe, dépôt, tableau Kanban), utilisation de Git dans VS Code | français |

### [`how-to/`](how-to/) — procédures ponctuelles

Faites pour être parcourues en diagonale : une manipulation précise, le
contexte supposé connu, et les commandes qui règlent la question. Classées par
outil.

| Procédure | Langue |
|---|---|
| [DFRobot FireBeetle 2 ESP32-C6 avec PlatformIO](how-to/platformio/boards/firebeetle2/) — ajouter la définition de carte et compiler avec le framework Arduino | français |

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
> https://github.com/btsciel-toulon/epsilonrt-labs

**Les extraits de code et les fichiers de configuration** échappent à cette
obligation : réutilisez-les librement, sans attribution. Les licences Creative
Commons ne sont pas conçues pour du logiciel, et une définition de carte ou
trois lignes de commande n'ont pas à traîner de contraintes derrière elles.

Si quelque chose ici vous fait gagner un après-midi, c'est déjà une raison
suffisante pour que ça existe.
