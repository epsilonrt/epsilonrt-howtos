# Utiliser Git dans VS Code

*Contexte : postes Windows 11 des salles du BTS CIEL, disque personnel `U:`.
Ce document suppose que vous avez terminé le
[doc1](doc1-git-github-et-creation-du-compte.md) (compte GitHub, Git installé
et configuré) et lu le [doc2](doc2-travailler-en-projet.md).*

Ce document montre, geste par geste, comment faire dans **VS Code** tout ce
que les deux premiers ont décrit : récupérer le dépôt de votre équipe,
enregistrer votre travail, le partager, récupérer celui des autres.

Vous n'aurez pas besoin de taper de commande Git : VS Code s'en charge. Pour
chaque geste, l'**équivalent en ligne de commande** est donné dans un encadré,
pour que vous fassiez le lien avec ce que fait réellement Git. Ne le
tapez que si vous êtes curieux.

À lire en entier une fois, puis à utiliser comme aide-mémoire.

---

## 1. Où travailler

**Tous vos dépôts sont clonés sur votre disque personnel `U:`**, dans un
dossier que vous créez pour l'occasion, par exemple `U:\projets`. Ce disque vous
suit d'un poste à l'autre : vous retrouvez votre dossier de travail partout.

Deux règles :

- **Un dossier par dépôt**, créé par le clone, à l'intérieur de `U:\projets`.
  Ne clonez jamais un dépôt *dans* un autre.
- **Poussez avant de partir.** `U:` est pratique, mais la copie de référence de
  votre travail est sur GitHub. Ce qui n'est pas poussé n'existe que chez vous.

## 2. Cloner le dépôt de l'équipe

À faire **une fois par projet**.

1. Ouvrez le dépôt de votre équipe sur GitHub (par exemple
   `github.com/btsciel-toulon/pp2027-r1`).
2. Cliquez sur le bouton vert **Code**, onglet **HTTPS**, puis sur l'icône de
   copie à côté de l'adresse.
3. Dans VS Code, ouvrez la palette de commandes avec **Ctrl+Maj+P**, tapez
   `Git: Clone` (ou `Git : Cloner`) et validez.
4. Collez l'adresse, validez.
5. Choisissez le dossier dans lequel créer le clone : `U:\projets`, puis
   **Sélectionner comme emplacement du dépôt**.
6. À la première utilisation, une fenêtre vous demande de vous **connecter à
   GitHub** : acceptez. Votre navigateur s'ouvre ; connectez-vous avec votre
   compte et autorisez l'accès. Vous n'aurez plus à le refaire.
7. VS Code demande s'il doit **ouvrir le dépôt cloné** : répondez **Ouvrir**.

**Ce que vous devez voir :** le contenu du dépôt dans l'explorateur de fichiers
à gauche, et, en bas à gauche de la fenêtre, le nom de la branche : **`main`**.

> **Équivalent en ligne de commande :**
>
> ```
> git clone https://github.com/btsciel-toulon/pp2027-r1.git
> ```

> **Si l'authentification se bloque :** fermez la fenêtre de connexion, puis
> recommencez le clone. Si cela persiste, prévenez votre professeur : ne
> cherchez pas à créer un « jeton d'accès » de votre propre initiative.

## 3. Le panneau Contrôle de code source

Tout ce qui concerne Git se passe dans un seul endroit : le **panneau
Contrôle de code source** (*Source Control*).

**Pour l'ouvrir :** cliquez sur l'icône en forme de branche dans la barre de
gauche, ou tapez **Ctrl+Maj+G**. Un petit nombre sur l'icône indique combien de
fichiers ont changé.

Le panneau contient :

- une **zone de message** en haut, pour écrire le message d'un commit ;
- la liste **Modifications** (*Changes*) : les fichiers que vous avez modifiés
  depuis le dernier commit ;
- la liste **Modifications indexées** (*Staged Changes*) : ceux que vous avez
  choisis pour le prochain commit. Elle n'apparaît que si vous en avez.

Une lettre à droite de chaque fichier dit ce qui lui est arrivé :

| Lettre | Signification |
|---|---|
| **M** | *Modified* : fichier existant, modifié |
| **U** | *Untracked* : nouveau fichier que Git ne connaît pas encore |
| **A** | *Added* : nouveau fichier déjà indexé |
| **D** | *Deleted* : fichier supprimé |
| **C** | *Conflict* : conflit à résoudre (voir section 8) |

**Cliquez sur un fichier** de la liste : VS Code affiche à gauche l'ancienne
version et à droite la nouvelle, avec les lignes modifiées en couleur. C'est
la meilleure façon de relire ce que vous allez enregistrer, avant de le
valider.

> Un fichier binaire — un schéma Proteus, par exemple — s'affiche comme
> *« The file is not displayed in the text editor because it is either binary… »*.
> C'est normal : Git sait qu'il a changé, mais pas dire *ce qui* a changé
> dedans.

Selon la langue de VS Code, les boutons portent des noms français ou anglais :

| Anglais | Français |
|---|---|
| Source Control | Contrôle de code source |
| Changes | Modifications |
| Stage | Indexer |
| Staged Changes | Modifications indexées |
| **Commit** | **Valider** |
| Sync Changes | Synchroniser les modifications |
| Pull / Push | Tirer / Envoyer (*Pull* / *Push* dans le menu) |

> **Équivalent en ligne de commande :** `git status` liste les fichiers
> modifiés.

## 4. Enregistrer son travail : indexer puis valider

C'est le geste que vous ferez le plus souvent. Il se fait en deux temps,
comme dans le schéma du doc1.

**Étape 1 — Choisir ce qui sera enregistré (indexer).**

Dans la liste **Modifications**, passez la souris sur un fichier et cliquez
sur le **`+`** qui apparaît. Le fichier passe dans **Modifications indexées**.
Le **`+`** à côté du titre **Modifications** indexe tous les fichiers d'un
coup.

Relisez d'abord ce que vous indexez : un fichier qui ne devrait pas être là
(voir section 7) ne doit pas passer dans la liste.

**Étape 2 — Écrire le message et valider.**

Cliquez dans la zone de message, saisissez le message, puis cliquez sur le
bouton **✓ Valider** (*Commit*), ou tapez **Ctrl+Entrée**.

**Ce que vous devez voir :** la zone de message se vide, les fichiers
disparaissent de la liste. Le commit existe maintenant, **sur votre disque**.

> **Si vous validez sans avoir rien indexé**, VS Code propose d'indexer
> *tous* les fichiers modifiés avant de valider. Cela va vite, mais vous ne
> maîtrisez plus ce qui part : préférez indexer vous-même.

### Bien écrire un message

Un bon message dit **ce qui a été fait**, en une ligne, en français, au présent
ou à l'infinitif :

| ✅ Bon | ❌ À éviter |
|---|---|
| `Ajoute le pilote du capteur DS18B20` | `modif` |
| `Corrige la lecture négative de la température` | `ça marche` |
| `Termine le schéma d'alimentation (Closes #7)` | `fichiers` |

**Pour fermer une tâche du tableau**, ajoutez le mot-clé et le numéro de
l'issue, comme expliqué au doc2, section 6 : `(Closes #7)`. Pour seulement
la mentionner : `(#7)`.

> **Équivalent en ligne de commande :**
>
> ```
> git add hardware/alimentation.pdsprj
> ```
>
> ```
> git commit -m "Termine le schéma d'alimentation (Closes #7)"
> ```

## 5. Partager et récupérer : synchroniser

Votre commit n'est encore que chez vous. Pour l'envoyer à GitHub — et pour
récupérer ce que vos coéquipiers ont envoyé — utilisez le bouton
**Synchroniser les modifications** (*Sync Changes*), qui apparaît dans le
panneau à la place du bouton Valider dès qu'il y a un commit à envoyer.

Il fait **deux choses dans l'ordre** : d'abord un *pull* (il ramène chez vous
les commits des autres), puis un *push* (il envoie les vôtres). Les nombres
`↓ 2 ↑ 1` indiquent, dans la barre d'état en bas à gauche, combien de commits
sont à recevoir (↓) et à envoyer (↑).

**Ce que vous devez voir après une synchronisation :** les nombres `↓` et `↑`
disparaissent. Sur GitHub, votre commit apparaît dans le dépôt.

### Le rythme à adopter

| Moment | Geste |
|---|---|
| **En arrivant**, avant de toucher à un fichier | Synchroniser, pour récupérer le travail des autres |
| **Après chaque tâche terminée** | Valider, puis synchroniser |
| **Avant de partir** | Synchroniser, même si votre travail n'est pas fini. Un travail à demi terminé, poussé, vaut mieux qu'un travail perdu |

> **Équivalent en ligne de commande :**
>
> ```
> git pull
> ```
>
> ```
> git push
> ```

### Vérifier sur GitHub

Ouvrez le dépôt sur GitHub, onglet **Code** : la date et le message de votre
dernier commit sont affichés au-dessus de la liste des fichiers. Si vous avez
utilisé `Closes #n`, ouvrez le tableau et vérifiez que la carte a changé de
colonne.

## 6. Retrouver l'historique

**Dans VS Code :** ouvrez un fichier dans l'éditeur, puis, dans l'explorateur de
fichiers à gauche, développez la section **Chronologie** (*Timeline*) tout en
bas. Elle liste les commits qui ont touché ce fichier ; cliquez sur l'un
d'eux pour voir ce qui a changé.

**Sur GitHub :** page du dépôt → **Commits**, ou ouvrez un fichier → **History**.
Cliquer sur un commit affiche les fichiers modifiés et leur contenu avant et
après.

> **Équivalent en ligne de commande :** `git log --oneline`.

## 7. Ne versionner que ce qui doit l'être

Tout ce qui se trouve dans le dossier de votre dépôt n'a pas vocation à être
enregistré. Certains fichiers sont **fabriqués** par vos outils (compilation,
sauvegardes) et n'apportent rien à l'équipe : ils encombrent l'historique et
provoquent des conflits.

Le dépôt de votre équipe contient un fichier **`.gitignore`**, qui liste ce
que Git doit ignorer. Les fichiers ignorés n'apparaissent **jamais** dans la
liste des modifications. Voici ce qu'il doit couvrir :

| À ne pas versionner | Pourquoi |
|---|---|
| Le dossier `.pio` de PlatformIO | Compilations et bibliothèques téléchargées : PlatformIO les refabrique |
| Les caches de VS Code (`.vscode/ipch` et similaires) | Fichiers propres à votre poste |
| Les sauvegardes et fichiers temporaires de Proteus (dossier *Project Backups*, fichiers *Autosaved*) | Ils changent à chaque enregistrement et n'ont aucun intérêt |
| Vos fichiers de configuration personnelle | Propres à votre poste |

> **Si un fichier de ce type apparaît dans la liste des modifications**, ne
> l'indexez pas, et prévenez votre professeur : le `.gitignore` du dépôt
> est incomplet, et il faut le corriger pour toute l'équipe.

### Jamais de secret dans un commit

Ne validez **jamais** un mot de passe, une clé d'API, un identifiant Wi-Fi ou un
jeton d'accès — ce qui est très tentant dans un programme pour ESP32 qui se
connecte à un réseau. Même dans un dépôt privé, ce qui a été poussé
reste dans l'historique, y compris si vous le supprimez ensuite : il faudrait
alors changer le secret. Mettez ces valeurs dans un fichier ignoré par
`.gitignore`, et non dans le code.

## 8. Résoudre un conflit

Un **conflit** survient quand deux personnes ont modifié **les mêmes lignes
d'un même fichier** : au moment de la synchronisation, Git ne sait pas laquelle
garder. Il ne choisit pas à votre place, et vous demande de trancher.

Les bonnes pratiques du doc2, section 7 (répartir les tâches, un seul à la fois
sur un schéma) sont là pour les éviter. Si cela arrive quand même :

### Un fichier texte (code, documentation)

1. La synchronisation s'arrête avec un message qui parle de conflit. Le fichier
   concerné porte la lettre **C** dans le panneau, sous une section **Modifications
   de la fusion** (*Merge Changes*).
2. Cliquez sur le fichier. Dans le code, VS Code encadre la zone en conflit et
   propose, au-dessus : **Accepter la modification actuelle** (la vôtre),
   **Accepter la modification entrante** (celle de l'autre) ou **Accepter les
   deux**. Un bouton **Résoudre dans l'éditeur de fusion** ouvre une vue à trois
   volets, plus confortable.
3. Choisissez, et relisez : le code doit être **cohérent** et compiler, pas
   seulement avoir perdu ses marqueurs `<<<<<<<`.
4. Enregistrez le fichier, **indexez-le** (`+`), puis **validez**.
5. Synchronisez.

En cas de doute sur ce qui doit être conservé, **demandez à votre coéquipier
qui a écrit l'autre version**. Choisir au hasard, c'est effacer son travail.

### Un fichier Proteus (`.pdsprj`)

Ne cherchez pas à le fusionner : c'est un fichier binaire, VS Code ne peut pas
combiner les deux versions. Ne bricolez pas : **arrêtez-vous et prévenez votre
professeur**, avant toute autre action. La version de chacun est conservée
dans l'historique, rien n'est perdu tant que vous ne forcez rien.

> **Équivalent en ligne de commande :** après avoir résolu, `git add` puis
> `git commit`. Les marqueurs `<<<<<<<`, `=======` et `>>>>>>>` visibles dans le
> fichier sont ceux que VS Code met en forme.

## 9. Si vous ouvrez le projet sur votre carte CM5

Si vous travaillez sur une carte Raspberry Pi CM5 depuis VS Code (extension
Remote-SSH), le panneau Contrôle de code source de la fenêtre distante agit
sur le dépôt qui se trouve **sur la carte**. Les gestes sont **exactement les
mêmes** que dans ce document.

Trois choses changent :

- il s'agit d'un **autre clone** du dépôt, distinct de celui de `U:`. Les deux
  ne communiquent que par GitHub : ce que vous validez sur l'un n'existe pas
  sur l'autre tant que vous n'avez pas fait un push d'un côté et un pull de
  l'autre. **Synchronisez toujours avant de changer d'environnement** ;
- **Git doit connaître votre identité sur la carte**, comme sur le poste.
  Dans un terminal ouvert sur la carte, refaites les deux commandes
  `git config --global user.name` et `git config --global user.email` du doc1,
  section 6 ;
- l'authentification GitHub se fait normalement à travers VS Code, sans que
  vous ayez à faire quoi que ce soit. Si un identifiant est demandé, arrêtez-vous
  et prévenez votre professeur.

## 10. En cas de problème

| Symptôme | Cause | Que faire |
|---|---|---|
| *« detected dubious ownership in repository »* à l'ouverture ou au clone | Git se méfie du dossier : propriétaire des fichiers différent de votre utilisateur, fréquent sur un disque réseau | Ne suivez pas les conseils trouvés sur Internet : prévenez votre professeur |
| Le clone ou une opération Git est très lent sur `U:` | Beaucoup de petits fichiers sur un disque réseau, en particulier le dossier `.pio` de PlatformIO | Vérifiez que `.pio` est bien ignoré (section 7) ; sinon prévenez votre professeur |
| Le bouton de synchronisation ne fait rien | Rien à envoyer ni à recevoir, ou pas de commit | Vérifiez que vous avez bien validé (zone de message vide, liste **Modifications** sans le fichier) |
| *« Please tell me who you are »* au moment de valider | `user.name` / `user.email` non configurés | Doc1, section 6 |
| *« rejected … fetch first »*, ou push refusé | Vos coéquipiers ont poussé avant vous | Synchronisez : le pull d'abord, puis le push |
| Le fichier que vous cherchez n'apparaît pas dans **Modifications** | Il est ignoré par `.gitignore`, ou vous ne l'avez pas enregistré | Enregistrez-le (Ctrl+S) ; s'il est ignoré, c'est voulu |
| La liste des modifications est énorme et contient des fichiers que vous n'avez pas touchés | Un dossier généré (`.pio`…) n'est pas ignoré, ou un outil a reformaté des fichiers | Ne validez rien ; prévenez votre professeur |
| Vous avez validé un fichier par erreur, mais pas encore poussé | — | Prévenez votre professeur, qui vous montrera comment défaire proprement |
| Vous avez poussé un secret (mot de passe, clé) | — | **Prévenez tout de suite votre professeur** : il faut changer le secret, le supprimer ne suffit pas |

## 11. À retenir

- Tout Git passe par le **panneau Contrôle de code source** (**Ctrl+Maj+G**).
- **Indexer** (`+`) choisit ce qui sera enregistré ; **valider** crée le commit ;
  **synchroniser** envoie vos commits et ramène ceux des autres.
- Un commit **n'est partagé qu'après la synchronisation**.
- Le rythme : **synchroniser en arrivant, valider après chaque tâche, synchroniser
  avant de partir**.
- Un bon message dit ce qui a été fait ; `Closes #n` ferme la tâche `#n`.
- Ne versionnez **ni fichiers générés, ni secrets**. Et si un fichier
  inattendu apparaît, n'indexez pas et demandez.
- Un conflit sur du **texte** se tranche dans VS Code, avec le coéquipier
  concerné. Sur un **fichier Proteus**, on ne tranche pas seul : on prévient.
- Sur la carte CM5, c'est un **autre clone** : synchronisez avant de changer
  d'environnement.

**Précédent :** [doc2 — Travailler en projet](doc2-travailler-en-projet.md).
