# Etape 2 : Lire les données d'un fichier

Nous allons maintenant voir comment écrire une fonction pour **extraire les données** qui nous intéressent d'un fichier Rinex.

---

## Compléter function_read.py

Ouvrez le fichier "function_read.py" de votre projet.

Complétez-la avec le code suivant :

~~~
###############################################################################
#HEADER

#This function reads a Rinex file, and returns a Pandas DataFrame containing 4
#columns: the observation date, the number of the epoch (time sample) since the
#beginning of the file, the GPS satellite number, and the S1C observation.

#Inputs:
#   -file_path: path of the Rinex file
    
#Outputs:
#   -df_sat_data: Pandas DataFrame containing the S1C satellite observations.
    
#Author: Arthur DENT

###############################################################################

#Libraries importation:---------------------------------------------------------

from datetime import datetime
import pandas as pd

#Function definition:----------------------------------------------------------

def read(file_path):
    
    #Initialize a dictionnary of GPS data:
    dic_sat_data = {'date': [], 'epoch': [], 'sat': [], 's1c': []}
    
    #Initialize the epoch number:
    epoch = 0
    
    #Open the Rinex file in "read" mode:
    file = open(file_path,'r')
    
    #Read the 1st line:
    line = file.readline()
    
    #Skip all lines until the end of the header:
    while 'END OF HEADER' not in line:
        
        line = file.readline()
    
    #Read all data lines until the end of the file:
    while line!='':
        
        #If the line contains a date:
        if line[0]=='>':
            
            data_date = line.split() #Separate the date elements
            
            date = datetime(int(data_date[1]),int(data_date[2]),int(data_date[3]),int(data_date[4]),int(data_date[5]),int(float(data_date[6]))) #Create a datetime object with this date 
            
            epoch += 1 #Increment the epoch by 1
        
        #If the line contains a GPS satellite data:
        if line[0]=='G':
            
            #Retrieve the satellite number as an integer:
            sat = int(line[1:3])
                     
            #If the S1C information is not empty:
            if line[45]!=' ':
                
                #Retrieve the S1C information as a float:
                s1c = float(line[43:49])
            
                #Add the date, the epoch, the GPS satellite number, and the S1C
                #to the dictionnary:
                dic_sat_data['date'] += [date]
                dic_sat_data['epoch'] += [epoch]
                dic_sat_data['sat'] += [sat]
                dic_sat_data['s1c'] += [s1c]
        
        #Read the next line:
        line = file.readline()
    
    #Convert the dictionnary to a Pandas DataFrame object:
    df_sat_data = pd.DataFrame(dic_sat_data)
    
    return df_sat_data
~~~

Essayez de comprendre ce que fait cette fonction grâce à ces commentaires.

D'ailleurs, **n'oubliez pas de commenter vos propres programmes durant le projet évalué !**
Un programme commenté est plus simple à maintenir et à partager.

Si vous n'avez pas compris certains passages de ce code, vous trouverez ci-dessous quelques rappels de Python dont vous aurez besoin pour votre projet.

Si vous êtes à l'aise en Python, et que vous avez tout compris du code ci-dessus, vous pouvez directement passer à la partie suivante.

## Les types

Les objets que l'on manipule en Python peuvent avoir **différents types**.

Voici les types d'objets "natifs" (toujours présents) en Python :

- **Integer** (int) : nombre entier positif ou négatif.

- **Float** (float) : nombre réel représenté en "point flottant".

- **Complex** (complex) : nombre complexe, avec une représentation du type "3+1j" avec "j" la racine de -1.

- **String** (str) : une chaine de charactères, définie entre guillemets.

- **Boolean** (bool) : "True" ou "False", utiles pour des opérations d'algèbre booléenne.

- Les conteneurs : **Listes**, **dictionnaires**, **sets** et **tuples** permettent de ranger de différentes manières d'autres objets. 
Nous verrons en détails dans ce tutoriel les listes et les dictionnaires.
Il est à noter qu'un string est théoriquement un conteneur.

Le "typage" (la définition du type d'un objet à sa création) est automatique en Python.
C'est bien pratique, mais cela peut être source d'erreurs.
**Faites-y attention !**

Il est possible de convertir un objet d'un type en autre, avec les fonctions natives : "int()", "float()", "complex()", "str()" ou "bool()".

Essayez de comprendre quel est le type de chaque objet utilisé dans notre fonction. 
Repérez les conversions d'un type à un autre, et essayez de deviner leur utilité.
Certaines seront plus claires dans la suite de cette partie.

## Les fonctions

Il est courant en programmation de ranger un groupe d'instructions sous la forme de "**fonctions**".

On peut voir une fonction comme une boîte noire qui prend des objets en entrée, et les utilise pour générer de nouveaux objets en sortie.

Les entrées et les sorties peuvent être n'importe quel objet Python.
Par contre attention, le type de ces objets n'est définit nulle part dans la fonction : il faudra bien préciser en commentaires et dans la documentation le type des entrées / sorties, pour qu'un utilisateur ne se trompe pas.

En Python, on définit une fonction de la manière suivante :

~~~
def nom_de_la_fonction(entree_1,entree_2,entree_3,...):

	...

	return sortie_1,sortie_2,sortie_3,...
~~~

**Faites bien attention à ce que le contenu de la fonction soit indenté !**
Le ":", les indentations et le "return" sont les indicateurs qui permettent à Python de comprendre où commence la fonction, quelles sont les instructions de la fonction, et où fini la fonction.

Pour appeler cette fonction avec des entrées données, et en récupérer les sorties, il suffira de taper la commande suivante :

~~~
sortie_1,sortie_2,sortie_3,... = nom_de_la_fonction(entree_1,entree_2,entree_3,...)
~~~

Il est conseillé de mettre comme entrées de la fonction tous les paramètres que l'on pourra vouloir faire varier d'une application de la fonction à une autre, quitte à leur donner des valeurs par défaut.

En Python, pour donner une valeur par défaut de 31 à la 3ème entrée d'une fonction, il suffit par exemple d'écrire :

~~~
def nom_de_la_fonction(entree_1,entree_2,entree_3=31,...):

	...

	return sortie_1,sortie_2,sortie_3,...
~~~

On peut alors appeler la fonction sans la 3ème entrée, qui prendra alors la valeur 31.

Attention, lorsqu'un objet est mis en entrée d'une fonction, et que cette fonction réalise des opération sur l'objet, il restera inchangé.
En effet, la fonction ne travaille pas directement sur les entrées, mais sur une copie de ces entrées : des "**variables locales**".

Pour qu'une fonction modifie un objet, il faut utiliser l'instruction "global", pour indiquer à la fonction qu'il s'agit d'une "**variable globale**".

Par exemple :

~~~
global nom_de_la_variable = 10
~~~

Comprenez-vous à présent ce que prend la fonction "read" en entrée et en sortie ?

## Les listes et les dictionnaires

En Python comme dans d'autres langages de programmation, il est parfois pratique de pouvoir ranger des données dans des "**conteneurs**".

On peut voir un conteneur comme un classeur, dans lequel on vient ranger des informations, avec différents systèmes de rangement.

Dans ce tutoriel, nous utilisons 2 types de conteneurs natifs Python : les **listes** et les **dictionnaires**.

Les listes comme les dictionnaires peuvent contenir n'importe quel type d'objet Python, mais classés de manière différente.

### Listes

Une liste range les objets **dans un certain ordre** : on associe à chaque objet un **index**.

Une liste est définie à l'aide de "[ ]".

On peut par exemple définir une liste de la manière suivante :

~~~
nom_de_la_liste = [59,'Lille',34.8]
~~~

Pour récupérer le 2ème élément de la liste dans une variable, on utilise la commande :

~~~
variable = nom_de_la_liste[1]
~~~

Pour modifier le 3ème élément de la liste, on utilise la commande :

~~~
nom_de_la_liste[2] = 'Toulouse'
~~~

Et oui, les indices d'une liste vont de 0 à la taille de la liste - 1.
Il est d'ailleurs possible de récupérer la taille de la liste avec la commande suivante :

~~~
len(nom_de_la_liste)
~~~

On peut également initialiser une liste vide, et y ajouter des éléments petit à petit de la manière suivante :

~~~
nom_de_la_liste = []
nom_de_la_liste += [59]
nom_de_la_liste += ['Lille']
nom_de_la_liste += [34.8]
~~~

Ceci est possible car le "+" pour une liste n'est pas l'opérateur "addition" mais "concaténer".

Attention, n'oubliez pas que pour une liste, l'ordre de concaténation compte : le "+" n'est pas symétrique !

Enfin, on peut récupérer un morceau d'une liste, en utilisant le caractère ":".

Par exemple, pour récupérer les éléments de l'indice 1 à l'indice 3 d'une liste, on écrit :

~~~
nom_de_la_liste[1:3]
~~~

### Dictionnaires

Un dictionnaire associe à chaque objet une **clé** : un string qui sert "d'étiquette" pour retrouver l'objet.

Un dictionnaire est défini à l'aide de "{ }".

On peut par exemple définir un dictionnaire de la manière suivante :

~~~
nom_du_dictionnaire = {'departement': 59, 'ville': 'Lille', 'surface': 34.8}
~~~

Pour récupérer l'élément associé à la clé "departement" dans une variable, on utilise la commande :

~~~
variable = nom_du_dictionnaire['departement']
~~~

Pour modifier l'élément associé à la clé "ville", on utilise la commande :

~~~
nom_du_dictionnaire['ville'] = 'Toulouse'
~~~

On peut également initialiser un dictionnaire vide, et y ajouter des éléments petit à petit de la manière suivante :

~~~
nom_du_dictionnaire = {}
nom_du_dictionnaire['departement'] = 59
nom_du_dictionnaire['ville'] = 'Lille'
nom_du_dictionnaire['surface'] = 34.8
~~~

Pour un dictionnaire, l'ordre d'ajout des éléments n'a pas d'importance.

## Ouvrir et lire un fichier

Pour **ouvrir un fichier** avec Python, on peut utiliser la fonction native "**open()**".

Cette fonction prend 2 entrées : le chemin du fichier sur l'ordinateur, et le mode d'ouverture.

Le mode d'ouverture est indiqué par une des 4 chaines de caractères suivantes :

- "r" : mode lecture, retourant une erreur si le fichier n'existe pas.

- "a" : mode ajout, ajoutant du contenu en fin de fichier, et créant le fichier s'il n'existe pas.

- "w" : mode écriture, créant le fichier s'il n'existe pas.

- "x" : mode création, retournant une erreur si le fichier existe déjà.

On peut également écrire "r+" pour ouvrir un fichier en mode lecture **et** écriture.

Si le fichier à ouvrir est en binaire, il faut ajouter un "b" au mode.

Comprenez-vous alors cette ligne de code de notre fonction ?

~~~
file = open(file_path,'r')
~~~

Une fois le fichier ouvert, on peut le lire ligne par ligne avec la fonction "readline()" : 

~~~
line = file.readline()
~~~

Si l'on appelle une 1ère fois la fonction, on récupère la 1ère ligne. 
Si on l'appelle une 2nde fois, on récupère la 2ème ligne.
Et ainsi de suite.

La variable "line" contient alors une chaine de caractères, dont on peut récupérer les éléments de la même manière que pour une liste.
Par exemple :

~~~
caractere = line[45]
~~~

Il est également possible de récupérer toutes les lignes sous la forme d'une liste, avec :

~~~
lines = file.readlines()
~~~

## Boucles et conditions

## Les dates

## Les DataFrame Pandas

