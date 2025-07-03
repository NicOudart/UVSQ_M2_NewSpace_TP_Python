# Etape 3 : Traitement des données S1C - détermination du pivot

Nous allons maintenant voir comment écrire une fonction pour **manipuler les données** S1C, afin de déterminer le satellite pivot à chaque instant.

---

## Compléter function_pivot.py

Ouvrez le fichier "function_pivot.py" de votre projet.

Complétez-la avec le code suivant :

~~~
###############################################################################
#HEADER

#This function manipulates data from a Pandas DataFrame returned by function
#"read", in order to return a DataFrame containing 2 columns: the observation 
#date, and the pivot satellite number.

#Inputs:
#   -df_sat_data: Pandas DataFrame as returned by the "read" function. 
    
#Outputs:
#   -df_pivot: Pandas DataFrame containing the selected pivot for each epoch.
    
#Author: Arthur DENT

###############################################################################

#Libraries importation:---------------------------------------------------------

import pandas as pd

#Function definition:----------------------------------------------------------

def pivot(df_sat_data):
    
    #Initialize a DataFrame which will contain the selected pivot for each epoch:
    df_pivot = pd.DataFrame(columns=('date','sat'))
    
    #Iterations on the epochs in the input GPS observation DataFrame:
    for epoch in range(1,df_sat_data['epoch'].max()+1):
        
        #Select the portion of the input DataFrame containing observations for 
        #this epoch:
        df_sat_epoch = df_sat_data[df_sat_data['epoch']==epoch]
        
        #Add at the right index in the pivot DataFrame the date / satellite
        #corresponding to the highest S1C:
        df_pivot.loc[epoch-1] = df_sat_epoch[['date','sat']].loc[df_sat_epoch['s1c'].idxmax()]
            
    return df_pivot
~~~

Cette fonction manipule les données du DataFrame issu du fichier Rinex, afin de créer un nouveau DataFrame contenant le satellite GPS choisi comme pivot à chaque instant.

Nous allons voir dans cette partie quelques manipulations utiles pour récupérer des informations dans un DataFrame.

Si vous connaissez déjà bien Pandas, et que vous avez tout compris du code ci-dessus, vous pouvez directement passer à la partie suivante.

## Sélection de données dans un DataFrame

Voici quelques façons de manipuler des DataFrames qui vous seront utiles pour votre projet.

Tout d'abord, on peut initialiser un DataFrame vide, en ne donnant que le nom des colonnes :

~~~
df = pd.DataFrame(columns=('colonne_1','colonne_2','colonne_3'))
~~~

Et on peut ensuite y ajouter une ligne à l'index "i" avec une commande du type :

~~~
df.loc[i] = ...
~~~

On peut également accéder à une ligne à l'index "i" avec la même méthode :

~~~
ligne = df.loc[i]
~~~

Il est aussi possible de sélectionner des colonnes d'un DataFrame avec une commande du type :

~~~
df2 = df[['colonne_2','colonne_3']]
~~~

Enfin, on peut sélectionner une partie d'un DataFrame avec des conditions sur les valeurs.

Par exemple, si pour sélectionner la portion d'un DataFrame pour laquelle les valeurs de la colonne 2 sont supérieures à 0, on écrit :

~~~
df2 = df[df['colonne_2']>0]
~~~

Il est même possible de combiner des conditions.
Par exemple, pour sélectionner la portion d'un DataFrame pour laquelle les valeurs de la colonne 2 sont supérieures à 0, et les valeurs de la colonne 3 sont égales à 1 :

~~~
df[(df['colonne_2']>0)&(df['colonne_3']==1)]
~~~

Il existe des méthodes associées aux DataFrame qui permettent de récupérer des information statistiques sur les données :

- df.min() : le minimum.

- df.idxmin() : l'indice du minimum.

- df.max() : le maximum.

- df.idxmax() : l'indice du maximum.

- df.sum() : la somme.

- df.mean() : la moyenne.

- df.median() : la médiane.

Et beaucoup d'autres !

Avec tous ces éléments, essayez de comprendre les manipulations de DataFrames réalisées par notre fonction "pivot".

---

Nous avons à présent dans notre projet une fonction permettant de choisir un satellite pivot pour chaque instant.
Dans la partie suivante, nous allons coder des fonctions pour réaliser un affichage de ces données, et les exporter au format CSV.