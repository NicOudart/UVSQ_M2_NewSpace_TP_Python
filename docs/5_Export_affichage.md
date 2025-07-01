# Etape 4 : Export et affichage des résultats

Nous allons maintenant voir comment écrire une fonction pour **afficher**, et une fonction pour **exporter** le satellite pivot choisi au cours du temps.

---

## Compléter function_display.py

~~~
###############################################################################
#HEADER

#This short function returns a figure of the GPS satellite number of the 
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

~~~