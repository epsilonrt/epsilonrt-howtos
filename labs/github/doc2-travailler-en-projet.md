# Travailler en projet : équipes, dépôt et tableau Kanban

*Contexte : BTS CIEL, projets menés en équipe dans l'organisation GitHub
`btsciel-toulon`. Ce document suppose que vous avez terminé le
[doc1](doc1-git-github-et-creation-du-compte.md) : compte créé, invitation
acceptée, Git configuré.*

Ce document explique **comment votre équipe s'organise** : où se trouve son
travail, comment répartir les tâches, et comment relier chaque tâche à ce que
vous produisez — code, schéma, carte. Les gestes Git eux-mêmes, dans VS Code,
sont dans le [doc3](doc3-git-dans-vscode.md) : lisez-le en parallèle.

À lire en entier avant le premier projet, puis à reprendre au besoin.

---

## 1. Vue d'ensemble

Pour chaque projet, votre professeur prépare trois choses :

```
   équipe GitHub  ──── a le droit d'écrire dans ────►  dépôt (code, schémas, docs)
                                                          │
                                                          │ lié à
                                                          ▼
                                                   tableau Kanban (les tâches)
```

- l'**équipe** : la liste des personnes qui travaillent ensemble ;
- le **dépôt** : là où vivent tous vos fichiers et leur historique ;
- le **tableau** : la liste des tâches à faire, en cours et terminées.

Vous n'avez **rien à créer**. Votre travail consiste à découper le projet en
tâches, à les réaliser, et à garder le dépôt et le tableau cohérents entre eux.

## 2. Ce que prépare votre professeur

1. Il **constitue les équipes** de projet et les crée sur GitHub. Vous
   êtes ajouté à la vôtre.
2. Il crée pour chaque équipe **un dépôt à partir d'un modèle**. Le modèle
   contient déjà la structure de dossiers, le fichier de présentation du sujet
   et la liste des fichiers que Git doit ignorer. Votre dépôt est donc
   autonome : il n'est lié ni au modèle ni aux dépôts des autres équipes.
3. Il crée **un tableau Kanban** à partir d'un gabarit commun, le relie à votre
   dépôt et donne l'accès à votre équipe.

Les noms suivent une convention, où `AAAA` est **l'année du diplôme** de votre
promotion. `pp` désigne le projet professionnel de deuxième année, `mp` le
mini-projet de première année :

| Objet | Nom | Exemple (équipe 1, diplôme 2027) |
|---|---|---|
| Équipe GitHub | `ppAAAA-t<n>` | `pp2027-t1` |
| Dépôt | `ppAAAA-r<n>` | `pp2027-r1` |
| Tableau Kanban | `ppAAAA-p<n>` | `pp2027-p1` |

**Vos droits.** Votre équipe peut **écrire** dans son dépôt (lire, modifier,
envoyer ses commits). Elle ne peut ni le supprimer, ni changer sa visibilité,
ni voir les dépôts des autres équipes. Les professeurs ont accès à tous les
dépôts et à tous les tableaux.

**Où retrouver votre dépôt.** Sur [github.com/btsciel-toulon](https://github.com/btsciel-toulon),
les seuls dépôts que vous voyez sont les vôtres. Le tableau se trouve dans
l'onglet **Projects** de votre dépôt.

> **Vous ne voyez rien ?** Trois causes possibles : vous n'avez pas accepté
> l'invitation à l'organisation (retour au doc1, section 5), vous n'êtes pas
> connecté avec le bon compte, ou votre professeur n'a pas encore créé les
> équipes. Dans le doute, prévenez-le.

## 3. Récupérer le dépôt sur votre poste

Une seule fois par projet, vous **clonez** le dépôt de votre équipe dans votre
espace personnel `U:`. La marche à suivre est dans le
[doc3](doc3-git-dans-vscode.md), section 2.

Ensuite, ce dossier est le vôtre : c'est là que vous travaillez, et VS Code
l'ouvre directement.

## 4. Le tableau Kanban

### Le principe

Un tableau **Kanban** représente l'avancement d'un travail en colonnes. Chaque
tâche est une **carte** qui se déplace de gauche à droite, à mesure qu'elle
avance :

| À faire | En cours | Terminé |
|---|---|---|
| Écrire le pilote du capteur | Dessiner le schéma d'alimentation | Choisir le microcontrôleur |
| Router la carte | | |

Les intitulés exacts des colonnes sont ceux du tableau de votre équipe. Trois
règles suffisent pour qu'un Kanban soit utile :

- **Une carte = une tâche assez petite** pour être terminée en une à quelques
  séances. « Faire la carte » n'est pas une tâche, c'est un projet.
  « Dessiner le schéma de l'étage d'alimentation » en est une.
- **Une carte = une personne responsable.** Ce n'est pas la seule à y
  travailler, mais c'est elle qui rend des comptes sur son avancement.
- **Peu de cartes « En cours » à la fois.** Terminez ce que vous avez commencé
  avant de démarrer autre chose.

### Une carte doit être une *issue*

Sur GitHub, une carte peut être de deux natures :

- une simple **note**, qui n'existe que dans le tableau ;
- une **issue** : un vrai objet du dépôt, qui a un numéro (`#12`), une page, un
  historique, et **que l'on peut relier aux commits**.

**Seule une issue peut être fermée par un commit.** C'est ce qui fait tout
l'intérêt de la suite : faites donc de chaque tâche une issue.

### Créer une tâche

Deux façons, au choix :

**Depuis le tableau.** Ouvrez le tableau, cliquez sur **+ Add item**, saisissez
le titre de la tâche, puis validez. Vous obtenez une note. Ouvrez le menu de la
carte (**⋯**) → **Convert to issue** → choisissez le dépôt de votre équipe.

**Depuis le dépôt.** Onglet **Issues** de votre dépôt → **New issue** → titre
et description → **Create**. Dans la colonne de droite de l'issue, section
**Projects**, sélectionnez le tableau de votre équipe : la carte apparaît.

Dans les deux cas, complétez la fiche de la tâche :

- un **titre** qui commence par un verbe : *« Écrire le pilote du capteur
  DS18B20 »* ;
- dans la **description**, ce qu'il faut obtenir pour dire que c'est fini — le
  critère de réussite. Une liste de cases à cocher (`- [ ] …`) fonctionne bien ;
- un **responsable** (*Assignees*).

**Ce que vous devez voir :** la carte dans la colonne **À faire**, avec son
numéro `#` et le nom de son responsable.

### Faire vivre le tableau

Déplacez la carte quand son état change : de **À faire** vers **En cours**
quand vous commencez, vers **Terminé** quand c'est fini. Glissez-la dans la
colonne, ou modifiez son champ **Status**.

## 5. Lier une tâche à ce que vous produisez

Une tâche seule ne dit pas où se trouve son résultat. Reliez chaque issue aux
éléments du dépôt qu'elle concerne, pour que n'importe quel membre de
l'équipe — ou votre professeur — puisse passer de l'une aux autres en un clic.

### Ranger au bon endroit

Le dépôt est déjà organisé par le modèle : **respectez sa structure**, sans
créer vos propres dossiers en parallèle. Code, schémas Proteus, documentation
et fichiers de configuration ont chacun leur emplacement. En cas de doute,
demandez.

### Les liens possibles

| Vous voulez relier… | Comment | Exemple |
|---|---|---|
| une **autre tâche** | écrire son numéro | `#12` |
| un **commit** | coller son identifiant, sept caractères suffisent | `a3f9c1e` |
| un **fichier** de code | ouvrir le fichier sur GitHub, appuyer sur la touche **`y`** (l'adresse devient permanente), la copier dans l'issue | `…/blob/a3f9c1e/src/capteur.cpp` |
| des **lignes précises** de code | dans le même fichier, sélectionner les lignes, puis **`y`** et copier | idem, avec `#L10-L25` |
| un **schéma** ou une **carte** Proteus | ouvrir le fichier `.pdsprj` dans le dépôt, appuyer sur **`y`** et copier l'adresse. GitHub ne sait pas l'afficher : le lien mène à la page du fichier | `…/blob/a3f9c1e/hardware/alimentation.pdsprj` |
| un **document** ou une **image** | même chose : **`y`** et copier | `…/blob/a3f9c1e/docs/schema-bloc.png` |

La touche **`y`** est importante : elle fige l'adresse sur la version du
fichier à cet instant. Un lien copié sans elle suit la version la plus
récente, et ne montrera plus ce que vous décriviez dès que le fichier aura
changé.

Écrivez ces liens dans la **description** ou dans un **commentaire** de
l'issue. GitHub les transforme automatiquement en références cliquables. Dans
l'autre sens, chaque commit qui mentionne `#12` apparaît dans la
chronologie de l'issue : le lien se fait donc dans les deux directions.

## 6. Valider une tâche en commitant

C'est le geste central de cette organisation : **le commit qui réalise une
tâche la ferme**.

### La règle

Dans le **message du commit**, écrivez un mot-clé suivi du numéro de l'issue :

```
Ajoute le pilote du capteur DS18B20 (Closes #12)
```

Les mots-clés reconnus sont `Closes`, `Fixes` et `Resolves` (et leurs
variantes `Close`, `Fixed`, etc.), en majuscules ou non. Prenez l'habitude
d'écrire toujours `Closes`. Quand ce commit arrive sur GitHub, sur la branche principale `main` :

1. l'issue `#12` est **fermée** automatiquement ;
2. la carte passe dans **Terminé** — si le tableau de votre équipe est
   configuré pour cela, ce que vous pouvez vérifier au point suivant ;
3. le commit est **affiché dans l'historique de l'issue**, donc relié à elle
   pour de bon.

> **Vérifiez toujours.** Après votre push, ouvrez le tableau : la carte doit
> avoir changé de colonne. Si l'issue est bien fermée mais que la carte est
> restée dans **En cours**, déplacez-la à la main et signalez-le à votre
> professeur, qui corrigera le tableau.

### Plusieurs tâches d'un coup

Répétez le mot-clé devant **chaque** numéro. `Closes #12, #13` ne ferme que la
première ; il faut écrire :

```
Ajoute les capteurs de température et d'humidité (Closes #12, closes #13)
```

### Un commit qui fait avancer sans terminer

Si vous avancez sur une tâche sans la finir, mentionnez simplement son numéro,
**sans mot-clé** :

```
Ébauche du pilote DS18B20, lecture encore instable (#12)
```

La tâche reste ouverte, mais le commit apparaît dans son historique. Sans
mot-clé, vous gardez le suivi sans fermer prématurément.

### Un exemple complet

Vous devez dessiner le schéma d'alimentation, tâche `#7`.

1. Vous déplacez la carte `#7` dans **En cours**.
2. Vous modifiez `hardware/alimentation.pdsprj` dans Proteus, puis vous
   l'enregistrez.
3. Dans VS Code, vous **indexez** le fichier et vous **validez** avec le
   message `Termine le schéma d'alimentation (Closes #7)`.
4. Vous **synchronisez** (push).
5. Sur GitHub, l'issue `#7` est fermée, la carte est dans **Terminé**, et le
   fichier est relié dans l'historique.

Les étapes 3 et 4 sont détaillées dans le [doc3](doc3-git-dans-vscode.md),
sections 4 et 5.

### Si vous vous êtes trompé

Vous avez fermé une tâche par erreur ? Ouvrez l'issue et cliquez sur **Reopen**.
Un message de commit ne se modifie pas après le push : c'est un principe de
Git, pas un oubli. Une correction se fait par un nouveau commit.

## 7. Travailler à plusieurs sans se marcher dessus

- **Répartissez les tâches** pour que deux personnes ne modifient pas le
  même fichier en même temps. C'est la meilleure façon d'éviter les conflits.
- **Les fichiers Proteus (`.pdsprj`) ne se fusionnent pas.** Ce sont des
  fichiers binaires : si deux coéquipiers en modifient un en parallèle, Git ne
  peut pas combiner leurs versions, et l'un des deux perd son travail. La règle
  est simple : **un seul à la fois sur un schéma**. Vous annoncez à l'équipe
  que vous le prenez, vous le modifiez, puis vous **validez et poussez
  aussitôt**, pour libérer le fichier.
- **Récupérez avant de commencer.** Au début de chaque séance, faites un
  **pull** pour ramener le travail des autres.
- **Poussez avant de partir.** Votre dépôt est sur `U:`, mais la copie de
  référence est sur GitHub. Ce qui n'est pas poussé n'existe que chez vous, et
  vos coéquipiers ne le voient pas.
- **Validez souvent, par petites unités.** Un commit = un sujet. Six commits
  clairs valent mieux qu'un seul « tout et n'importe quoi ».

Vous travaillez directement sur la branche principale `main`. Les branches et
les *pull requests* sont un sujet à part, qui n'est pas traité ici.

## 8. En cas de problème

| Symptôme | Cause | Que faire |
|---|---|---|
| Le commit est poussé, mais l'issue reste ouverte | Pas de mot-clé, mauvais numéro, ou faute de frappe (`Closes #12` fonctionne, `Closes 12` ou `Closes# 12` non) | Fermez l'issue à la main et corrigez votre habitude ; le message du commit, lui, ne peut plus changer |
| L'issue est fermée, mais la carte n'a pas bougé | L'automatisme n'est pas actif sur le tableau | Déplacez la carte à la main et prévenez votre professeur |
| `Closes #12` n'a aucun effet | La carte `#12` est une note et non une issue, ou le commit n'est pas sur `main` | Convertissez la note en issue (section 4) ; vérifiez que vous avez bien poussé |
| Le numéro `#12` désigne une autre tâche que celle que vous vouliez | Numéro mal recopié | Vérifiez le numéro sur la carte avant chaque commit |
| Un coéquipier a écrasé votre schéma | Deux personnes ont modifié le même `.pdsprj` | Retrouvez l'ancienne version dans l'historique du fichier sur GitHub, et respectez la règle « un seul à la fois » |
| Le dépôt de l'équipe est introuvable | Invitation non acceptée, ou mauvais compte | Doc1, section 5 ; sinon prévenez votre professeur |

## 9. À retenir

- Votre professeur crée **l'équipe, le dépôt et le tableau** ; vous n'avez rien
  à créer, mais vous devez les faire vivre.
- **Une tâche = une carte = une issue**, avec un titre à l'infinitif, un
  critère de fin et un responsable.
- Reliez chaque tâche à ses productions : numéros `#12`, identifiants de
  commit, liens permanents (touche **`y`**) vers les fichiers.
- Pour **terminer une tâche**, mettez `Closes #n` dans le message du commit, puis
  poussez : l'issue se ferme et la carte doit passer dans **Terminé**.
  Vérifiez-le.
- Pour **avancer sans terminer**, mentionnez `#n` sans mot-clé.
- **Un seul à la fois** sur un fichier Proteus. Pull en arrivant, push en
  partant.

**Précédent :** [doc1 — Git, GitHub et création du compte](doc1-git-github-et-creation-du-compte.md).
**Suite :** [doc3 — Git dans VS Code](doc3-git-dans-vscode.md).
