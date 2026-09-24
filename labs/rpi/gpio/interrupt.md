# Détecter un front : la scrutation et l'interruption

Un bouton, un capteur de passage, un signal d'horloge : ce qui compte bien
souvent n'est pas *l'état* d'une broche, mais le **moment où il change**.
Ce document compare deux façons de le savoir : lire la broche en boucle
(la **scrutation**) ou laisser le matériel prévenir le programme (l'**interruption**).

## 1. La scrutation et ses inconvénients

La scrutation (en anglais *polling*) consiste à lire la broche encore et encore,
et à regarder si elle a changé. C'est ce que fait l'exemple `Button` de piduino :

```cpp
void loop () {

  // read the state of the pushbutton value:
  buttonState = digitalRead (buttonPin);

  // check if the pushbutton is pressed. If it is, the buttonState is LOW:
  if (buttonState == LOW) {
    digitalWrite (ledPin, HIGH);
  }
  else {
    digitalWrite (ledPin, LOW);
  }
}
```

Cela fonctionne, mais cette méthode a plusieurs défauts :

- **Elle occupe le processeur en permanence.** La boucle tourne à vide pour
  savoir si quelque chose s'est passé, même quand rien ne se passe. Le
  processeur ne peut pas se consacrer à autre chose, ni se mettre en veille.
- **Elle peut rater un événement.** Si la broche change et revient à son état
  initial *entre deux lectures*, le programme ne voit rien. Avec une boucle qui
  contient un `delay (100)`, une impulsion de 50 ms peut passer inaperçue.
- **Le temps de réaction est variable.** Le programme ne remarque le front que
  lors de la lecture suivante : en moyenne une demi-période de scrutation plus
  tard, et au pire une période entière. Plus la boucle fait de choses, plus
  c'est long.
- **Elle ne détecte pas un front, seulement un état.** Pour savoir que le
  bouton *vient d'être* appuyé, il faut mémoriser l'état précédent et le
  comparer au nouvel état, à chaque tour de boucle.
- **Elle complique le programme.** Dès qu'il y a plusieurs entrées à surveiller
  et d'autres tâches à effectuer, il faut tout entrelacer dans la même boucle.

## 2. Le principe de l'interruption sur front

Avec une interruption, on inverse le rôle : ce n'est plus le programme qui va
voir si la broche a changé, c'est **la broche qui prévient le programme**. En
attendant, le processeur est libre de faire autre chose, ou de dormir.

Pour prévenir le programme, un circuit matériel surveille la broche en
permanence. En voici le principe (la réalisation exacte dépend du processeur).

![Circuit de détection de front : un bouton avec résistance de tirage est relié à la broche GPIO ; le signal traverse un trigger de Schmitt, deux bascules qui mémorisent l'état actuel (Q1) et l'état précédent (Q2), des détecteurs de front montant et descendant, une sélection du front choisi, puis un drapeau d'interruption mémorisé qui envoie une demande d'interruption au processeur.](interrupt-detection.svg)

### Le circuit extérieur

Le bouton est relié à la broche GPIO. Une **résistance de tirage** (*pull-up*)
relie la broche à 3,3 V : bouton relâché, la broche est au niveau haut ; bouton
appuyé, elle est reliée à la masse, donc au niveau bas. La Raspberry Pi possède
une résistance de tirage interne, que `pinMode (pin, INPUT_PULLUP)` active :
aucun composant externe n'est alors nécessaire, il suffit du bouton entre la
broche et la masse.

### Le circuit de détection dans le processeur

1. Un **trigger de Schmitt** nettoie le signal : il évite qu'un niveau
   intermédiaire, ou un peu de bruit, soit lu tantôt comme 0, tantôt comme 1.
2. Deux **bascules** échantillonnent la broche à chaque coup d'horloge.
   La bascule A mémorise l'état actuel (**Q1**) ; la bascule B mémorise
   l'état précédent (**Q2**).
3. On compare les deux :
   - **Front montant** : la broche est passée de 0 à 1, donc Q1 = 1 et Q2 = 0 ;
   - **Front descendant** : la broche est passée de 1 à 0, donc Q1 = 0 et Q2 = 1.
4. Le programme choisit quel front l'intéresse : `RISING` (montant),
   `FALLING` (descendant) ou `CHANGE` (les deux). Seul le front choisi est
   transmis.
5. Le front détecté positionne un **drapeau d'interruption**, qui garde le
   souvenir de l'événement jusqu'à ce que le logiciel le remette à zéro. Le
   front ne peut donc pas être manqué, même si le processeur n'est pas
   disponible à cet instant.
6. Le drapeau envoie une **demande d'interruption** (IRQ) au contrôleur
   d'interruptions, qui prévient le processeur.

C'est le point clé : le circuit voit le front *à la place* du programme, et
le mémorise.

## 3. L'interruption de programme et la routine d'interruption

Quand le processeur reçoit la demande, il exécute un petit morceau de programme
que vous avez écrit : la **routine d'interruption** (en anglais *ISR*, pour
*Interrupt Service Routine*). C'est une fonction comme une autre, mais que vous
n'appelez jamais vous-même : c'est le matériel qui la déclenche.

![Interruption de programme : dans le principe, le processeur suspend le programme principal au moment du front, exécute la routine d'interruption, puis reprend le programme là où il s'était arrêté ; avec piduino sous Linux, la routine s'exécute dans un fil d'exécution séparé, réveillé par le noyau, pendant que le programme principal continue.](interrupt-programme.svg)

Dans le principe (première moitié du schéma) :

1. le programme s'exécute normalement ;
2. un front arrive : le processeur suspend le programme, mémorise où il en
   était et exécute la routine d'interruption ;
3. la routine se termine : le programme reprend exactement là où il s'était
   arrêté, sans s'apercevoir de rien.

**Sous Linux, avec piduino, c'est un peu différent** (seconde moitié du
schéma). Le noyau Linux détecte le front, puis piduino le signale à un **fil
d'exécution** (*thread*) qu'il a créé et qui attendait cet événement. Ce fil
appelle votre routine. Le programme principal n'est pas suspendu : il continue à
s'exécuter en parallèle. Pour l'utilisateur, le résultat est le même (la
routine est appelée à chaque front), mais le temps de réaction est plus long
et moins régulier que sur un microcontrôleur : il dépend de la charge de la
carte et de l'ordonnancement de Linux.

### Bonnes pratiques

- **Une routine doit être courte.** Elle fait le strict nécessaire (noter un
  instant, mettre à jour une variable, changer une sortie) et rend la main. Les
  traitements longs se font dans le programme principal.
- **Les variables partagées demandent de la prudence.** Une variable écrite par
  la routine et lue par `loop ()` peut être modifiée en plein milieu d'une
  lecture. Avec piduino, les deux s'exécutent dans des fils d'exécution
  différents : utilisez des types prévus pour cela (`std::atomic`).
- **Les rebonds d'un bouton produisent plusieurs interruptions.** Un seul
  appui peut appeler la routine plusieurs fois : voir le paragraphe 5.

### Comparaison

| | Scrutation | Interruption |
|---|---|---|
| Processeur | occupé en permanence | libre entre deux événements |
| Impulsion brève | peut être manquée | mémorisée par le drapeau |
| Temps de réaction | variable, dépend de la boucle | indépendant de la boucle du programme |
| Détection d'un front | à programmer (mémoriser l'état précédent) | faite par le matériel |
| Plusieurs entrées | boucle de plus en plus lourde | une routine par entrée |

## 4. Exemple avec piduino

Le programme ci-dessous est l'exemple
[`examples/Interrupt/main.cpp`](https://github.com/epsilonrt/piduino/blob/master/examples/Interrupt/main.cpp)
de la bibliothèque piduino. Il détecte les fronts montants et descendants d'une
broche, recopie l'état de la broche sur une LED, et affiche dans la console le
temps écoulé entre deux fronts.

```cpp
#include <Piduino.h>  // All the magic is here ;-)

// <DANGER> Be careful !!! Before launching this program :
//    -> Check that the pin below is well connected to an LED ! <-
const int ledPin = 0; // Header Pin 11: GPIO17 for RPi, GPIOA0 for NanoPi
const int irqPin = 3; // Header Pin 15: GPIO22 for RPi, GPIOA3 for NanoPi

unsigned long t1, t2; // for calculating time differences between interruptions

// -----------------------------------------------------------------------------
// Interrupt Service Routine
// Called at each interruption triggered by a rising or falling edge
void isr() {
  int value;

  t2 = millis(); // second time

  // reads the binary value
  value = digitalRead (irqPin);

  // copy irq value to led
  digitalWrite (ledPin, value);

  // prints the time difference between edges and the state of the irq pin.
  Console.print (t2 - t1);
  Console.print (":\t");
  Console.println (value);
  t1 = t2; // the new time becomes the first for the next irq
}

void setup() {

  Console.begin (115200);
  // initialize digital pin ledPin as an output.
  pinMode (ledPin, OUTPUT);
  // initialize digital pin irqPin as an input with pull-up (for button ?)
  pinMode (irqPin, INPUT_PULLUP);
  // attach interrupt service routine isr to irqPin, called for each edge
  attachInterrupt (irqPin, isr, CHANGE);
}

void loop () {

  // Press Ctrl+C to abort ...
  delay (-1); // nothing to do, we sleep ...
}
```

### Ce que fait ce programme

- **`pinMode (irqPin, INPUT_PULLUP)`** configure la broche en entrée avec la
  résistance de tirage interne. Un bouton câblé entre la broche et la masse
  suffit.
- **`attachInterrupt (irqPin, isr, CHANGE)`** relie la routine `isr` à la
  broche : elle sera appelée à chaque front. Le troisième argument choisit le
  front : `RISING`, `FALLING` ou `CHANGE`. C'est l'instruction qui configure le
  circuit de détection vu au paragraphe 2.
- **`isr ()`** est la routine d'interruption. Elle relève l'heure avec
  `millis ()`, lit l'état de la broche, le recopie sur la LED, puis affiche
  l'écart avec le front précédent.
- **`loop ()`** ne fait rien : `delay (-1)` endort le programme indéfiniment.
  Tout le travail se fait dans la routine. C'est l'opposé de l'exemple de
  scrutation du paragraphe 1, où la boucle ne s'arrêtait jamais.

### Faire fonctionner l'exemple

Les numéros de broche de l'exemple suivent la numérotation de piduino
(voir [pinfo](../../ressources/piduino/pinfo.md)), qui n'est pas celle du
connecteur : sur une Raspberry Pi, `irqPin` (3) est la broche 15 du connecteur
(GPIO22) et `ledPin` (0) la broche 11 (GPIO17). Sur une carte qui n'a pas ce
brochage, adaptez les deux constantes.

1. Branchez une LED, avec sa résistance, sur la broche `ledPin`.
2. Pour tester sans bouton, reliez `irqPin` par un fil à une autre broche
   (la broche 10 de piduino, GPIO8 sur une Raspberry Pi, soit la broche 24 du
   connecteur) et configurez celle-ci en sortie :

   ```bash
   pido mode 10 out
   ```

3. Compilez l'exemple (le fichier `CMakeLists.txt` est fourni avec les sources)
   et lancez-le. L'exemple précise qu'il faut les droits administrateur pour
   accéder aux broches :

   ```bash
   sudo ./Interrupt
   ```

4. Depuis un autre terminal, faites clignoter la broche 10 à 100 ms :

   ```bash
   pido blink 10 100
   ```

La console affiche alors l'écart en millisecondes entre deux fronts, suivi du
nouvel état de la broche. Le premier écart est grand (il compte depuis le
démarrage), les suivants valent environ 100 :

```
Press Ctrl+C to abort ...
27047:  0
100:    1
100:    0
100:    1
```

Si vous remplacez le fil par un bouton (entre `irqPin` et la masse), vous
obtenez un affichage à chaque appui et à chaque relâchement, avec les rebonds
du contact en prime : plusieurs lignes très rapprochées (1 ou 2 ms d'écart) pour
un seul appui.

## 5. Les rebonds d'un bouton

### Le problème

Un contact mécanique ne passe pas proprement d'un état à l'autre. Quand on
appuie sur un bouton, les deux lames métalliques se touchent, rebondissent, se
touchent de nouveau, et cela pendant quelques millisecondes avant que le
contact soit stable. La broche voit donc une série de fronts très rapprochés,
alors que l'utilisateur n'a appuyé qu'**une seule fois**.

Une interruption sur front est justement faite pour ne rien rater : elle
détecte tous ces fronts parasites, et la routine est appelée plusieurs fois
pour un seul appui. Le compteur d'appuis est faux, la LED clignote, l'action
est exécutée plusieurs fois. Avec l'exemple du paragraphe 4, on voit ces
rebonds à la console : des lignes très rapprochées (1 ou 2 ms d'écart) pour un
seul appui.

### Les solutions

- **Matériel** : un filtre RC (résistance et condensateur) dont la constante de
  temps dépasse la durée des rebonds, suivi d'un trigger de Schmitt. Le
  condensateur lisse les rebonds, le trigger remet le signal en forme. Cela
  demande des composants supplémentaires.
- **Logiciel, dans la routine** : ignorer un front qui arrive trop peu de temps
  après le précédent, en comparant `millis ()` à l'instant du front précédent.
  La routine est quand même appelée à chaque rebond ; elle ne fait simplement
  rien.
- **Filtrage par le noyau Linux, avec piduino** : le noyau sait filtrer les
  rebonds lui-même, avant que la routine ne soit appelée. C'est la solution la
  plus simple.

### Le filtrage des rebonds de piduino

On indique une durée de filtrage, en **millisecondes**, en plus du front à
détecter. Un changement d'état n'est signalé que s'il est resté stable pendant
cette durée : les rebonds plus courts sont ignorés et la routine n'est appelée
qu'**une fois** par appui.

![Chronogramme des rebonds d'un bouton : à l'appui et au relâchement, la broche oscille plusieurs fois avant de se stabiliser ; sans filtrage, la routine est appelée à chaque front, soit douze fois ; avec un filtrage de durée D, le noyau attend que le signal reste stable pendant D et la routine n'est appelée qu'une fois pour l'appui et une fois pour le relâchement, avec un retard D.](interrupt-rebond.svg)

La fonction `attachInterrupt ()` de la classe `Pin` accepte cette durée en
paramètre. Voici l'exemple
[`examples/NoArduino/Gpio/Interrupt/main.cpp`](https://github.com/epsilonrt/piduino/blob/master/examples/NoArduino/Gpio/Interrupt/main.cpp),
qui utilise directement la classe `Pin` plutôt que les fonctions Arduino,
modifié pour un bouton : on ne détecte que l'appui (front descendant, puisque
la broche est tirée vers le haut) et on filtre pendant 20 ms.

```cpp
Pin &irq = gpio.pin (irqPin);

// ...

irq.setPull (Pin::PullUp);    // pull-up resistor: released = high, pressed = low
irq.setMode (Pin::ModeInput);
irq.attachInterrupt (isr, Pin::EdgeFalling, 20); // falling edge only, 20 ms debounce
```

Le troisième paramètre est la durée de filtrage. Sans lui (comme dans
l'exemple du paragraphe 4), aucun filtrage n'est demandé au noyau. Sous le
capot, `attachInterrupt ()` appelle la fonction `setDebounce ()` de la couche
GPIO de piduino, qui transmet la durée au noyau Linux.

Les fonctions Arduino `attachInterrupt (broche, routine, front)` n'ont pas ce
paramètre : pour filtrer les rebonds, il faut passer par la classe `Pin`.

### Choisir la durée

- **Trop courte** : des rebonds passent encore.
- **Trop longue** : on rate les appuis rapides, et la réaction est retardée
  d'autant, puisque le front n'est signalé qu'une fois la durée écoulée.
- Pour un bouton, quelques millisecondes à une vingtaine de millisecondes
  conviennent en général ; le mieux est de la régler en observant les
  affichages de l'exemple.
- **Ne filtrez pas un signal rapide.** Un signal de 1 kHz (période de 1 ms) est
  entièrement effacé par un filtre de 20 ms. Le filtrage est fait pour les
  contacts mécaniques.

## 6. À retenir

- La **scrutation** lit l'état de la broche en boucle : elle occupe le
  processeur, peut rater une impulsion brève et réagit avec un délai variable.
- L'**interruption** confie la surveillance à un circuit matériel : il détecte
  le front (montant, descendant ou les deux), le mémorise dans un drapeau et
  prévient le processeur.
- La **routine d'interruption** est appelée à chaque front, sans que le
  programme ait à la lancer. Elle doit rester courte.
- Avec piduino, `attachInterrupt (broche, routine, front)` installe la routine.
  Elle s'exécute dans un fil d'exécution séparé du programme principal.
- Un bouton mécanique rebondit : une seule pression déclenche plusieurs
  interruptions. Prévoyez un filtrage, matériel ou logiciel, ou demandez au noyau
  de filtrer avec le paramètre de durée de `Pin::attachInterrupt ()`.
