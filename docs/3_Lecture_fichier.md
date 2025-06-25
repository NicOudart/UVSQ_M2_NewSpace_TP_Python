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



## Les types

## Ouvrir et lire un fichier

## Les boucles

## Les DataFrame Pandas

