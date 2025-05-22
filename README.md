# 2024-2025-4GP-Rafaelo-G-ElioGalin

# Projet Capteur

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


## Contexte du Projet
Dans le Cadre du cours " Du capteur au banc de test", qui se base sur l'article "Pencil Drawn Strain Gauges and Chemiresistors on Paper" de Cheng-Wei Lin et al., les étudiants sont invités à caractériser un capteur particulier composé d'une simple feuille de papier et d'une couche de graphite. Cette couche de graphite joue le rôle d'une resistance variable qui dépend de la flexion du papier. Le but de ce projet est de réaliser un banc de test permettant d'étudier les variations de cette résistance en fonction de la déformation du capteur.


## Livrables

- Un PCB contenant différents modules se reliant à une carte Arduino UNO.
- Un code Arduino pour pouvoir controler les différents modules sur le PCB.
- Un application permettant la connection au PCB par Bluetooth.
- Une datasheet du capteur graphite.


## LTSpice 

La résistance du capteur graphite est de l'ordre du Méga Ohm et ne peut donc pas se mesurer simplemen. Nous utilison une alimentation 5V, nous avons donc :
U = R * I --> I = U/R --> I très faible.

Pour pouvoir obtenir des valeurs acceptables, nous avons réalisé un circuit amplificateur à transimpédance : 

![LTSpice-shema](/Photos/Circuit-LTSpice.PNG)

Pour l'amplificateur opérationnel nécessaire au circuit, nous avons choisi le LTC1050 pour sa capacité à accepter un courant d'entrée faible et un offset de tension bas.

Afin d'améliorer le rapport Signal/Bruit, nous avons ajouté trois filtres : 

- filtre passe bas passif en entrée (R1,C1) avec une fréquence de coupure fc1 = 16Hz.
- filtre passe bas actif couplé à l'amplificateur opérationnel (R3,C4) avec une Fréquence de coupure fc2 = 1.6Hz. Ce filtre permet de s'isoler du bruit 50Hz du réseau électrique.
- filtre en sortie (R6,C2), avec une fréquence de coupure fc3 = 1.6kHz. Ce filtre permet de s'affranchir du bruit généré par notre circuit.


## Réalisation informatique du PCB sur KiCAD

Pour la réalisation du PCB, nous avons utilisé le logiciel KiCAD 6.0. Ce logiciel permet de créer le cricuit électrique ainsi que le PCB grâce à ses nombreuses librairies de composants.

La réalisation du PCB implique l'utilisation de différents modules qu'il faut pouvoir afficher sur le shéma : 

- Un module Bluetooth HC-05.
- Un écran OLED
- Un potentiomètre digital qui remplace la résistance R2 de notre circuit LTSpice.

Dans un premier temps, nous avons effectuée le shéma électrique : 

![kicad-shema](/Photos/Kicad-photo.png)

Dans un second temps, une fois tous les composants reliés entre eux, nous avons réalisé le PCB :

![PCB-shema](/Photos/PCB-Projet-Capteur.PNG)

Voici un visuel 3D du PCB pour donner une idée du placement des composants :

![pcb-3d](/Photos/Vue-3D-PCB.PNG)

Vous pouvez tout retrouver en détail dans le dossier [Kicad](/kicad/).


## Réalisation physique du PCB

Grâce à l'aide de Catherine Crouzet, nous avons pu fabriquer le PCB nous mêmes.

Pour ce faire, nous avons tout d'abord créé un fichier Gerber depuis KiCAD :

![gerber](/Photos/Gerber.PNG)

Ce filtre sert de masque de gravure. On place le masque sur une plaquette en epoxy jaune recouverte d'une fine couche de cuivre, elle même recouverte d'une couche de résine. Il sert de porotection pour la résine lors de son passage sous une lampe UV. Apres son passage sous la lampe à UV, la plaquette est déposé dans un bain "révélateur", qui retire la couche de résine qui a été en contact avec les UV, et aussi le cuivre en dessous de cette résine. Après un contrôle visuel pour s'assurer de la bonne exécution du processus, la plaque est retiré du bain et lavée à l'eau pour retirer le produit du bain et ensuite à l'acétone pour retirer la couche de résine qui a protégé les pistes en cuivre. Le PCB est maintenant près, on peut couper les parties non nécessaire de la carte pour un rendu plus propre.

Suite à ce processus, nous avons percé les trous dans lesquels des composants ainsi que des connecteurs des différents modules, puis nous les avons soudés.


## Code Arduino

Les differents modules sont contrôlés par un code Arduino 

Au branchement de l'Arduino UNO, après avoir envoyé le code, on peut voir sur l'écran OLED un menu déroulant:

-Menu 1 : Mesure Graphite
-Menu 2 : Infos Createurs

La navigation se fait avec l'encodeur rotatoir, le faire tourner permets de faire defiler les menus, et le cliquer permet d'accéder au menu sélectionné.

Vous trouverez le code Arduino [ici](/Code-Arduino/)

## Application

Nous avons utilisé  le site MIT App Inventor pour réalisé l'application qui reçoit et affiche en temps réel les valeurs mesurées par le Capeur graphite sur un graphe.

PHOTO

L'application est assez simple. Elle permet :
- De se connecter à un appareil via le bluetooth.
- De démarrer une acquisition et de la mettre en pause.
- d'effacer le graphe.
- De s'adapter en cas de changement de position du téléphone en offrant un mode paysage.

Vous la trouverez [ici](/Application Projet Capteur/)

## Datasheet

Nous avons écrit une datasheet qui caractérise le capteur graphite. Nous avons réalisé les mesures avec notre système et à l'aide d'un module 3D composé de demi-cercles permettant une déformation croissante précise.

Nous avons pu tracer ces courbes caractéristique de différents types de crayons (2H,HB,2B,4B,6B): 

Compression :

![graphe de compression](/Photos/Figure_compression.PNG)

Tension:

![graphe de tension](/Photos/Figure_tension.PNG)

Vous pouvez retrouver cette datasheet [ici](/Datasheet/)

## Conclusion

Tous nos résultats sont à  prendre avec prudence. Nous ne pouvons pas affirmer avoir réalisé un dépot de graphite parfaitement uniforme sur tous les capteurs, ce qui a pu avoir un impact important sur la valeur de leur résistance, et donc sur leur comprtement. De plus, la qualité du matériel utilisé n'est pas  optimale : les composants et les connexions présentent des imperfections qui ont pu influencer les mesures obtenues.

Cela dit, ce capteur présente plusieurs avantages : il est très versatile, peu coûteux, et extrêmement simple d’utilisation. L’électronique associée peut être facilement miniaturisée, et le capteur s’avère relativement sensible. Son principal défaut reste cependant l’homogénéité du dépôt de graphite. Si ce point pouvait être amélioré, le capteur pourrait être envisagé pour des applications industrielles, notamment pour des usages ponctuels, à grande échelle, ou à usage unique.
Dans un cadre personnel ou éducatif, ce capteur est particulièrement adapté. Il possède une forte valeur pédagogique, comme en témoigne ce projet, en permettant d’explorer de nombreux domaines scientifiques et techniques.

Ce capteur est-il industrialisable ? 

Oui et non.

Il pourrait trouver une application dans des secteurs comme l’aviation, où l’on utilise un très grand nombre de capteurs de contrainte. Son faible coût pourrait représenter un avantage économique. Néanmoins, ses limitations électroniques — notamment en termes de fiabilité et de miniaturisation — pourraient poser problème dans des environnements contraints.

Il pourrait en revanche être largement démocratisé dans le domaine de l’éducation, comme outil d’apprentissage ou de découverte.

Mais malgré son coût très faible, ce capteur reste très niche et la présence d’options plus performantes sur le marché rendent sa rentabilité à l'échelle industrielle peu crédible. Nous pensons donc qu’il serait difficilement industrialisable à grande échelle.

## Auteurs

Rafaëlo GERARDIN : <gerardin@insa-toulouse.fr>
Elio GALIN : <galin@insa-toulouse.fr>


