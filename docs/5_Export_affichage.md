# Etape 4 : Export et affichage des résultats

Nous allons maintenant voir comment écrire une fonction pour **afficher**, et une fonction pour **exporter** le satellite pivot choisi au cours du temps.

---

## Compléter function_display.py

~~~
###############################################################################
#HEADER

#This short function returns a figure showing the GPS satellite number of the 
#selected pivot, as a function of time.

#Inputs:
#   -df_pivot: Pandas DataFrame as returned by the "pivot" function. 
    
#Outputs:
#   -fig_pivot: Matplotlib axes object of the selected pivot satellite as a 
#               function of time.
    
#Author: Arthur DENT

###############################################################################

#Function definition:----------------------------------------------------------

def display(df_pivot):
    
    fig_pivot = df_pivot.plot(kind='scatter',x='date',y='sat',title='Selected GPS pivot satellite',xlabel='Date - Time',ylabel='GPS satellite number',color='r',grid='True',yticks=[i for i in range(1,df_pivot['sat'].max()+1)])
    
    return fig_pivot
~~~

## Compléter function_export.py

~~~
###############################################################################
#HEADER

#This function writes a CSV file containing the GPS satellite number of the
#selected pivot, as a function of time. The path of and name of the CSV file is 
#defined by the user.

#Inputs:
#   -df_pivot: Pandas DataFrame as returned by the "pivot" function.
#   -file_path: path of the directory where the CSV file will be saved.
#   -file_name: name of the CSV file to be saved.
    
#Outputs:
#   -None.
    
#Author: Arthur DENT

###############################################################################

#Libraries importation:---------------------------------------------------------

import os

#Function definition:----------------------------------------------------------

def export(df_pivot,file_path,file_name):
    
    #Open the CSV file at the given path, with the given name:
    file = open(os.path.join(file_path,file_name+'.csv'),'w')
    
    #Write the header, containing the columns names:
    file.write('date,pivot\n')
    
    #Iterate on the rows of the Pandas DataFrame:
    for idx in range(len(df_pivot)):
        
        #Write a line containing a date / pivot satellite number couple:
        file.write(str(df_pivot['date'].loc[idx])+','+str(df_pivot['sat'].loc[idx])+'\n')
    
    #Close the CSV file:
    file.close()
~~~