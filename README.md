# 2024-2025-4GP-Rafaelo-G-ElioGalin

# Projet Capteur

## Principe du Projet
Dans le Cadre du cours " Du capteur au banc de test", il a été proposé un projet.
Ce cours se basant sur l'article "Pencil Drawn Strain Gauges and Chemiresistors on Paper" de Cheng-Wei Lin et al., propose d'utiliser un capteur special, composé d'une simple feuille de papier et d'une couche de graphite. Cette couche de graphite permets de créer un resistance dont le but de ce projet est de réaliser un banc de test qui nous permettra d'étudier ses variations en fonction de la déformation du capteur.


## Somaire

- [Principe du Projet](#principe-du-projet)
- [Livrables](#livrables)
- [LTSpice](#ltspice)
- [Réalisation informatique du PCB sur KiCAD](#Realisation-informatique-du-pcb-sur-kicad)
- [Réalisation physique du PCB](#Réalisation-physique-du-PCB)
- [Code Arduino](#code-arduino)
- [Application](#application)
- [Datasheet](#datasheet)
- [Conclusion](#conclusion)
- [Auteurs](#auteurs)



## Livrables

- Un PCB contenant différents modules se reliant à une carte Arduino UNO.
- Un code Arduino pour pouvoir controler les différents modules sur le PCB.
- Un application permettant la connection au PCB par Bluetooth.
- Une datasheet du capteur graphite.


## LTSpice 

La résistance du capteur graphite ne peut pas être mesuré simplement à cause de sa valeur de l'orde du Méga Ohms. Et dans notre cas, nous utilison une alimentation de 5V ce qui induit par la formule :
U = R * I --> I = U/R --> I très faible.

Pour pouvoir obtenir des valeurs acceptable, il nous a fallu réaliser un circuit amplificateur à transimpédance : 

![LTSpice-shema](/Photos/Circuit_LTSpice.PNG)

Pour l'amplificateur opérationnel necessaire au circuit, nous avons choisi le LTC1050 pour sa capacité accepter un courant d'entrée faible et un offset de tension bas.

POur ameliorer le rapport Signal/Bruit, nous avons ajouté 3 types de filtres : 

- Un filtre passe bas passif en entrée(R1,C1) avec une Fréquence de coupure fc1 = 16Hz.
- UN filtre passe bas actif couplé à l'amplificateur opérationel(R3,C4) avec une Fréquence de coupure fc2 = 1.6Hz. Ce filtre permet de se libérer du bruit du 50Hz du réseau élèctrique.
- UN filtre en sortie(R6,C2), avec une fréquence de coupure fc3 = 1.6kHz. Ce filtre permet de s'affranchir du bruit généré par notre circuit.

## Réalisation informatique du PCB sur KiCAD

Pour la réalisation du PCB, nousa vons utilisé le logiciel KiCAD 6.0. Ce logiciel permet la création de cricuit électrique ainsi que de PCB grâce a ses nombreuses librairies de composants.

La réalisation du PCB implique l'utilisation de différents modules qu'il faut pouvoir afficher sur le shéma : 

- Un module Bluetooth HC-05.
- Un écran OLED
- Un potentiomètre digital qui remplace la résistance R2 de notre circuit LTSpice.

Dans un premier temps, nous avons effectuée le shéma électrique : 

![kicad-shema](/Photos/Kicad_photo.png)

Dans un second temps, un fois que tous les composants étaient bien relié entre eux, nous avons réalisé le PCB :

![PCB-shema](/Photos/PCB-Projet_Capteur.png)

Voici un visuel 3D de ce PCB pour avoir une idée du placement des composants :

![pcb-3d](/Photos/Vue-3D_PCB.png)

Vous pouvez tout retrouver dans le dossier [Kicad](/kicad/capteur graphite CouBeh).

## Réalisation physique du PCB

Grâce à l'aide de Catherine Crouzet, nous avons pu fabrique le PCB nous même.

Pour ce faire, on creer un fichier Gerber depuis KiCAD :
PHOTO

Ce filtre sert de masque de gravure. Il est placé sur une plaquette en epoxy jaune recouverte d'une fine couche de cuivre. 

Suite au processus, il faut percer les trous dans lequels les composants viendrons se loger, et souder les composants passifs ainsi que des connecteurs pour les différents modules pour facilité leur utilisation.(facile de changer de composants lorsqu'il y a un problème).

PHOTO peut etre

## Code Arduino

Pour pouvoir contrôler les differents modules sur le Arduino UNO, nous avons réalisé un code Arduino. Nous utilisons les libraires : 

- Adafruit_SSD1306 pour contrôler l'écran OLED
- SoftwareSerial pour contrôler le Bluetooth

Vous pouvez trouver le code Arduino [ici]()

Au branchement de l'Arduino UNO, après avoir envoyé le code, on peut voir sur l'écran OLED un menu déroulant:

-Menu 1 : Graphite sensor

La navigation se fait avec l'encodeur rotatoir, le faire tourner permets de faire defiler les menus, et le cliquer permet d'accéder au menu sélectionné.

## Application

Nous avons utilisé  le site MIT App Inventor pour réalisé l'application. Elle recoit et affiche sur un graphe les valeurs du Capeur graphite.

PHOTO

L'application est assez simple, Elle permet :
- De se connecter à un appareil via le bluetooth.
- De démarrer une acquisition et de la mettre en pause.
- d'effacer le graphe.
- De s'adapter en cas de changement de position du téléphone en offrant un mode paysage.

Vous pouvez la retrouver [ici](/Application Projet Capteur/)

## Datasheet

Pour la réalisation de la datasheet, nous avons caractérisé le capteur graphite grâce à notre système ainsi que d'une impression 3D d'un module composé de demi-cercles offrant une déformation croissante.

Nous avons pu tracer ces courbes caractéristique de différents types de crayons (2H,HB,2B,4B,6B): 

[graphe de compression](/Photos/Graphe_compression.png)

[graphe de flexion](/Photos/Grape_flexion.png)

Vous pouvez retrouver cette datasheet [ici](/Datasheet/)

## Conclusion

Tous résultats est à  prendre avec des pincettes, nous ne pouvons pas affirmer avoir réalisé un dépots parfaitement uniforme sur tous les capteurs, ce qui peut avoir un fort impact sur la valeur de resistance du capteur, et donc de son comprtement. De plus, la qualité du matériel utilisé n'est pas forcement adéquate, les composants ne sont pas parfait, les connexions non plus, et cela peut jouer beaucoup sur la valeur mesuré.

Cependant, Ce capteur à l'avantage d'être très versatile, peu couteux et très simple à mettre en place, l'électronique peu etre miniaturisé facillement et il est assez sensible. Si son principal problème, l'homogénisation du dépots de graphite, peut être corrigé, alors il pourrait potentiellement être utilisé dans un cadre industriel pour des applications rapide, à usage unique ou en très grande quantité.
Ou bien dans un cadre personnel car possède aussi une grande valeur pédagogique comme on a pu le voir à travers ce projet, il permet de toucher à pleins de domaines différents. 

Donc est ce qu'il pourrait être industrialisable ? 

Oui et non.

Il pourrait trouver un secteur d'utilisation comme l'aviation qui a besoin d'un très grand nombre de capteurs de contraintes, et dont l'aspect économique pourrait intéresser. Même si ses contraintes électronique pourrait poser problème par soucis de place malgrès une miniaturisation. 
Il pourrait être démocratisé dans l'éducation comme outils d'apprentissage, ou de découverte. 

Mais comme il amènerai peu de profit à son fabricant à cause de son utilisation un peu niche et d'options plus performantes sur le marché, et ce malgrès son coût très faible, nous pensons qu'il serait difficilement industrialisable. 

## Auteurs

Rafaëlo GERARDIN : <gerardin@insa-toulouse.fr>
Elio GALIN : <galin@insa-toulouse.fr>

