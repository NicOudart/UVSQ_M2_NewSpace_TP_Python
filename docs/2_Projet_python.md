# Etape 1 : Structurer un projet Python

Pour commencer, nous allons voir comment structurer un **projet Python** de manière à ce qu'il soit importable / utilisable le plus simplement possible.

---

## Structure générale

Il n'y a pas vraiment de règles fixes pour structuration d'un projet Python.
Cependant, il y a quelques "bonnes pratiques" suivies par la plupart des développeurs.

En génénal, un projet Python est rangé dans un dossier contenant au minimum les éléments suivants :

![Structure du projet](img/Projet_structure.png)

On y trouve 4 sous-dossiers :

- **src** : qui contiendra les codes sources de notre projet, sous la forme d'un **Package**.

- **docs** : qui contiendra la documentation de notre projet, en langage **Markdown**.

- **examples** : qui contiendra des scripts Python d'exemple, appliquant les fonctions de notre projet à un cas concret.

- **test** : qui contiendra des scripts Python pour tester automatiquement les fonctions de notre projet.

Et 3 fichiers :

- **readme.md** : qui contiendra un descriptif rapide du projet, en langage **Markdown**.

- **requirements.txt** : qui contiendra la liste des bibliothèques nécessaires à faire fonctionner le projet.

- **setup.py** : qui contiendra des informations sur le projet, nécessaires à installer le projet avec la commande "pip install".

Créez un dossier sur votre ordinateur, du nom de "PyS1C", qui contiendra notre projet.

Dans ce dossier, créez des sous-dossiers "src", "docs", "examples" et "test".
Créez aussi des fichiers vides "readme.md", "requirements.txt" et "setup.py".

Dans cette partie du tutoriel, nous allons commencer à remplir ces sous-dossiers et fichiers.

## Dossier src et Packages

En général, le **code source** d'un projet Python sera rangé dans le dossier "src", sous la forme de **fonctions** (ou de classes en Programmation Orientée Objet).

On peut ranger les fonctions avec 2 niveaux de hierarchisation :

- Un **module** est un fichier Python contenant un ensemble de fonctions.

- Un **package** est un dossier contenant un ensemble de modules.

On peut alors importer n'importe quelle fonction contenue dans un module, lui-même contenu dans un package, de la manière suivante :

~~~
from package.module import fonction
~~~

Vous avez sûrement déjà vu ce type de commandes : c'est ce que vous utilisez pour importer des fonctions de bibliothèques Python telles que Numpy ou Matplotlib.

Pour faire simple dans ce tutoriel, notre projet "PyS1C" ne contiendra qu'un seul package "PyS1C", et chaque module ne contiendra qu'une seule fonction.

Créez un dossier "PyS1C" dans votre dossier "src".

Dans ce dossier, créez les modules vides suivants : 

- "function_read.py"

- "function_display.py"

- "function_pivot.py"

- "function_export.py"

Afin qu'un dossier contenant des modules soit reconnu comme un package, il faut qu'il contienne un fichier "\_init_.py", appelé **initialisateur**.
Comme son nom l'indique, il est executé automatiquement à l'initialisation du package.

Ce fichier peut en théorie être vide, mais on s'en sert souvent pour importer les fonctions de tous les modules du package.

Pour notre tutoriel, créez un fichier "\_init_.py" dans le package "PyS1C", contenant les commandes suivantes :

~~~
from .function_read import read
from .function_display import display
from .function_pivot import pivot
from .function_export import export
~~~

Le package importera ainsi les fonctions des différents modules à son initialisation.

Les fonctions "read", "display", "pivot" et "export" seront définies plus tard dans ce tutoriel.

Votre package a alors la structure suivante :

![Structure du package](img/Package_structure.png)

## Dossier docs et Markdown

En général, la **documentation** d'un projet Python sera rangé dans le dossier "docs", sous la forme de fichers **Markdown**.

Le "Markdown" est un langage de programmation dit de "balisage", comme le HTML, mais à la syntaxe beaucoup plus simple.
Il est couramment utilisé dans le domaine "Open-Source" pour générer la documentation de projets de programmation.

Nous verrons quelques bases de ce langage plus tard dans ce tutoriel.

En attendant, créez dans votre dossier "docs" un fichier Markdown vide "PyS1C_documentation.md" :

![Structure de la documentation](img/Docs_structure.png)

## Dossiers examples et test

En général, des scripts Python d'**exemple** et de **test** d'un projet sont rangés dans les dossiers "examples" et "test".

Les scripts d'exemple permettent à un utilisateur d'appliquer les différentes fonctionnalités du projet Python à un exemple simple, afin de comprendre rapidement son fonctionnement.

Dans le cadre de ce tutoriel, nous implémenterons un unique script d'exemple, en fournissant un fichier Rinex d'exemple provenant de la station GODS.

Créez dans le dossier "example" un script Python vide "PyS1C_example.py", et ajouter le fichier Rinex qui vous a été fourni [ici](https://github.com/NicOudart/UVSQ_M2_NewSpace_TP_Python/tree/main/example) :

![Structure des exemples](img/Example_structure.png)

Les scripts de test permettent d'automatiser la phase de test du projet, afin de vérifier qu'une modification n'a pas introduit des bugs ou dégradé les performances. 
Toute la difficulté des tests est alors d'anticiper les cas d'utilisation du projet.

Vous trouverez souvent dans les tests automatiques d'un projet de programmation les 3 types de tests :

- **Tests unitaires** : L'idée est de tester chaque fonction indépendemment.

- **Tests d'intégration** : L'idée est de tester l'articulation des différentes fonctions entre elles.

- **Tests de validation / de performances** : L'idée est de vérifier que le projet est bien conforme au cahier des charges.

Dans le cadre de ce tutoriel, nous n'implémenterons que les tests unitaires, sous la forme d'un unique script Python.

Créez dans le dossier "test" un script Python vide "Py_S1C_unit_test.py" :

![Structure des tests](img/Test_structure.png)

## Readme

## Requirements

## Setup