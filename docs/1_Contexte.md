# Etape 1 : Contexte du tutoriel

Lors de ce tutoriel, nous allons créer un module Python pour une application concrète : **la récupération et le traitement de données GNSS**.

---

## GNSS et RTK

|Définition du GNSS|
|:-|
|On appelle GNSS la généralisation du système de positionnement par satellite GPS (américain) à d'autres constellations (GALILEO, Beidou, GLONASS, etc.).|
|Ce sont les initiales de "Global Navigation Satellite System".|

Le principe du **GNSS** est le suivant : utiliser le **temps de retard** entre l’émission de signaux par des satellites et leur réception par un appareil au sol, afin de **localiser** cet appareil.

Pour localiser un récepteur au sol, il faut donc au minimum :

- La **position des satellites**, généralement fournies par l'émetteur sous forme d’éphémérides.

- Les « **pseudo-distances** » entre le récepteur et au moins 4 satellites, calculées par le récepteur à partir des temps de retard de réception des signaux.  

Les mesures de pseudo-distance étant entachées de nombreuses sources d’erreurs (erreurs d’horloges, erreurs d’éphémérides, erreurs atmosphériques, multi-trajets dus à des obstacles, bruits dans les signaux), on obtient en général rarement mieux qu’une précision métrique sur la position du récepteur.

Les formes les plus modernes de navigation GNSS permettent en post-traitement d’obtenir une **précision centimétrique**, en se basant sur la stratégie "Real-Time Kinematics" (**RTK**) :

- Ne pas utiliser les temps de retard pour estimer les distances satellite-récépteur, mais la **phase du signal**, qui est moins affectée par le bruit.

- Utiliser un **satellite de référence** nommé « pivot », afin de compenser par soustraction les erreurs côté récepteur.

- Utiliser une **station au sol de référence** à proximité du récepteur (quelques dizaines de km maximum), afin de compenser par soustraction les erreurs côté satellite.

Voici un schéma de principe de cette méthode :

![RTK](img/RTK.jpg)

Une telle méthode nécessite donc :

* Des données issues d'un **réseau mondial de stations de référence** GNSS.

* Un critère de sélection pour le **choix du satellite de référence** ("pivot").

Pour répondre au 1er besoin, des réseaux de stations ont effectivement été mis en place, enregistrant 24h/24 les signaux GNSS avec un pas de 30 ou 1 s, pour les principales constellations GNSS. 
Certains sont privés et vendent ces données, d’autres sont publics et mettent à disposition ces données gratuitement.

L’« International GNSS Service » (**IGS**) est une fédération internationale d’agences / institutions / universités, mettant à disposition les observations de 512 stations situées dans 118 pays. 
Dans le cadre de ce tutoriel, nous allons étudier des données issues d’une station IGS : la **station GODS** appartenant à l’institut « Goddard Space Flight Center » (dépendance de la NASA située dans le Maryland).

![GODS](img/GODS.jpg)

Pour répondre au 2nd besoin, les données GNSS contiennent en général un **indicateur de qualité du signal** de chaque satellite, appelé le **C/N0**.

Ce critère permet de comparer le rapport signal sur bruit (SNR en anglais) de satellite fonctionnant sur des bandes de fréquences différentes.
**Plus le C/N0 est élevé, meilleure est la qualité du signal**.

|Objectif du module Python|
|:-|
|Le module Python que nous allons coder dans ce tutoriel permettra de traiter les données C/N0 de la station GODS, afin de choisir un satellite pivot pour chaque mesure GNSS.|

## Fichiers Rinex

Afin de rendre les données des stations de différents réseaux lisibles par tous, un **format standard** a été proposé par l’IGS : le **Rinex**. 

Un fichier Rinex est un fichier **ASCII**, constitué de :

- Un **en-tête** contenant les méta-données utiles.

- Une **liste des observations** pour chaque satellite visible, à chaque instant d’échantillonnage.