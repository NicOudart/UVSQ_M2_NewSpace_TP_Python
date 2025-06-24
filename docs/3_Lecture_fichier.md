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

## Les listes et les dictionnaires

## Les types

## Ouvrir et lire un fichier

## Les boucles

## Les DataFrame Pandas

