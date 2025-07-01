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
        df_pivot.loc[epoch] = df_sat_epoch[['date','sat']].loc[df_sat_epoch['s1c'].idxmax()]
            
    return df_pivot
~~~