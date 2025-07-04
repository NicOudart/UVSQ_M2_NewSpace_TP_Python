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