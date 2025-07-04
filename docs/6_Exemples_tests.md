# Etape 6 : Exemples et tests

Nous allons maintenant voir comment écrire des scripts d'**exemple** et de **test** pour notre projet Python.

---

## Compléter PyS1C_example.py

Ouvrez le fichier "PyS1C_example.py" de votre projet.

Complétez-la avec le code suivant :

~~~
###############################################################################
#HEADER

#This script applies the PyS1C functions to the example Rinex file provided 
#along with this project (station GODS - 04/05/2024) :
    
#   -Import GPS S1C data from the Rinex file.
#   -Choose the pivot GPS satellite for each epoch.
#   -Display a figure of the chosen pivot satellite as a function of time.
#   -Export as a CSV file the chosen pivot satellite as a function of time.

#Author: Arthur DENT

###############################################################################

#Libraries importation:--------------------------------------------------------
import os
import PyS1C

#Output directory / name:------------------------------------------------------

#Define the CSV output file path:
output_path = os.path.dirname(__file__)

#Define the CSV output file name:
output_name = 'Pivot_GODS00USA_R_20241250000_01D_30S_MO'

#Input directory:--------------------------------------------------------------

#Define the path of the input Rinex file:
input_path = os.path.join(os.path.dirname(__file__),'GODS00USA_R_20241250000_01D_30S_MO.rnx')

#Example script:---------------------------------------------------------------

#Read S1C data from the Rinex file:
df_sat_data = PyS1C.read(input_path)

#Choose the pivot satellite for each epoch:
df_pivot = PyS1C.pivot(df_sat_data)

#Display as a figure the chosen pivot satellite as a function of time:
fig_pivot = PyS1C.display(df_pivot)

#Export as a CSV file the chosen pivot satellite as a function of time:
PyS1C.export(df_pivot,output_path,output_name)
~~~

Pour que ce code puisse tourner, il faut tout d'abord que PyS1C soit installé sur votre version de Python.
Il ne faut donc pas oublier de faire tourner cette commande au préalable :

~~~
pip install PyS1C
~~~

Et à chaque mise à jour du projet :

~~~
pip install PyS1C --upgrade
~~~

Essayez de comprendre ce que ce script fait à partir des commentaires.

## Les scripts d'exemple

Le code Python précédent est un script, qui applique le projet à notre exemple concret d'un fichier Rinex issu de la station GODS le 04/05/2024.
**Il fait appel à chacune des fonctions du projet, dans l'ordre logique de son fonctionnement**.

C'est en général ce que l'on veut faire avec un **script d'exemple**.

L'idée est de faire comprendre à l'utilisateur comment le projet fonctionne, sur un **exemple représentatif** de ce pour quoi le projet a été conçu.

Dans l'idéal, il faudrait que le script soit executable directement par l'utilisateur, d'un simple clic.
On peut néanmoins définir des paramètres d'entrée modifiables en début de script.

Pour cela, il faut impérativement que le script soit **bien commenté !**
L'utilisateur doit pouvoir le lire comme un tutoriel.

## Compléter PyS1C_unit_test.py

Ouvrez le fichier "PyS1C_unit_test.py" de votre projet.

Complétez-la avec le code suivant :

~~~
###############################################################################
#HEADER

#This script automatize unit tests for functions "read" and "pivot" of PyS1C.
#Tests are performed on the example Rinex file provided along with this project 
#(station GODS - 04/05/2024).

#Author: Arthur DENT

###############################################################################

#Libraries importation:--------------------------------------------------------
import os
import PyS1C

#Input directory:--------------------------------------------------------------

#Define the path of the input Rinex file:
input_path = os.path.join(os.path.dirname(__file__),'GODS00USA_R_20241250000_01D_30S_MO.rnx')

#Unit tests script:------------------------------------------------------------

#Check the "read" function:
df_sat_data = PyS1C.read(input_path)
assert list(df_sat_data.columns.values)==['date', 'epoch', 'sat', 's1c'], 'PyS1C.read: Wrong DataFrame columns!'
assert len(df_sat_data)==29047, 'PyS1C.read: Wrong number of lines in the DataFrame!'
assert str(df_sat_data['date'].loc[0])=='2024-05-04 00:00:00' and str(df_sat_data['date'].loc[29046])=='2024-05-04 23:59:30', 'PyS1C.read: Wrong time period in the DataFrame'
assert df_sat_data['epoch'].max()==2880, 'PyS1C.read: Wrong number of epochs!'
assert df_sat_data['sat'].max()<=32 and df_sat_data['sat'].min()>=1, 'PyS1C.read: Invalid GPS satellite number!'
assert round(df_sat_data['s1c'].mean(),5)==43.20223, 'PyS1C.read: Problem with the S1C values!'
print('PyS1C.read: checked!')

#Check the "pivot" function:
df_pivot = PyS1C.pivot(df_sat_data)
assert list(df_pivot.columns.values)==['date', 'sat'], 'PyS1C.pivot: Wrong DataFrame columns!'
assert len(df_pivot)==2880, 'PyS1C.pivot: Wrong number of lines in the DataFrame!'
assert str(df_pivot['date'].loc[0])=='2024-05-04 00:00:00' and str(df_pivot['date'].loc[2879])=='2024-05-04 23:59:30', 'PyS1C.pivot: Wrong time period in the DataFrame'
assert df_pivot['sat'].max()<=32 and df_pivot['sat'].min()>=1, 'PyS1C.pivot: Invalid GPS satellite number!'
assert round(df_pivot['sat'].mean(),5)==14.32083, 'PyS1C.pivot: Problem with the pivot numbers!'
print('PyS1C.pivot: checked!')
~~~

Comme vous pouvez le voir, ces tests sont basés sur la méthode "assert" de Python.
Voyons comment réaliser des tests sous Python, et comment fonctionne cette méthode "assert".

## Les tests unitaires

Il est important de pouvoir vérifier en un seul clic qu'**une modification de notre projet n'entrainera pas des bugs**.
D'où l'intérêt d'écrire des scripts de tests, afin d'**automatiser** cette vérification.

Comme nous l'avons expliqué plus tôt lors de ce tutoriel, nous n'implémenterons ici que des **tests unitaires**, et uniquement ceux des fonctions de lecture ("read") et de traitement ("pivot").
C'est aussi ce qui sera attendu de vous pour votre projet évalué.

S'il bien entendu impossible d'être complètement exhaustifs dans nos tests, l'idée est d'anticiper le maximum de bugs possibles, en réalisant des **tests représentatifs des cas d'utilisations** de votre projet.

Voici quelques fonctions utiles pour réaliser des tests de vos fonctions.

Ce n'est pas le cas dans notre projet ici, mais il est recommandé d'ajouter à vos fonctions des vérifications de vos entrées / sorties, afin de retourner un **message d'erreur** avec la méthode "**raise ValueError**" :

~~~
if ...:
	raise ValueError('...')
~~~

Vous pouvez mettre le message d'erreur que vous voulez.
Il est à noter qu'il existe d'autres types d'erreurs que "ValueError" en Python.

Lors de vos tests, pour vérifier que les erreurs sont bien détectées (que la fonction retourne un message d'erreur), vous pouvez utiliser "**try**" et "**except**" :

~~~
try: 
	...
except ValueError:
	print('Error detected!')
~~~

L'idée est que le programme essaye de faire tourner les lignes de codes dans le "try", et que si une erreur "ValueError" est 

Enfin, comme pour notre projet, vous pouvez utiliser la méthode "**assert**" afin de vérifier que la fonction renvoit bien la sortie à laquelle vous vous attendez.
Dans le cas contraire, la méthode affichera un message d'erreur.

Par exemple, si on veut vérifier qu'une fonction "test_func()" retourne bien 2 quand on lui donne 1 en entrée :

~~~
assert test_func(1)==2, 'test_func NOK'
~~~

Si la fonction ne retourne pas la valeur attendue, le message "test_func NOK" s'affichera.

Essayez à présent de comprendre les test qui sont réalisés par notre script de tests.
Avez-vous des idées d'autres tests qui pourraient être réalisés ?

---

Nous avons à présent dans notre projet des scripts d'exemple et de test, appliquant notre projet à l'exemple concret de la station GODS.
Dans la dernière partie de ce tutoriel, nous allons écrire une documentation en Markdown pour notre projet.