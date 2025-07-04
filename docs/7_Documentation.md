# Etape 7 : Documentation

Pour finir, nous allons voir comment écrire une **documentation** en **Markdown** pour notre projet Python.

---

## Les bases du Markdown

Comme expliqué précédemment, le Markdown est un **langage de balisage** très simple, couramment utilisé pour générer la documentation de projets Python Open-Source.

Nous allons ici voir les bases de ce langage.

### Les titres

Pour indiquer qu'une ligne est un titre, on ajoute un "#" devant.
Pour indiquer qu'il s'agit d'un sous-titre, on ajoute un "##" devant.
Pour indiquer qu'il s'agit d'un sous-sous-titre, on ajoute un "###" devant.
Etc.

Par exemple :

```
# Titre
## Sous-titre
### Sous-sous-titre
```

Attention, il ne faut pas oublier de mettre un espace entre les "#" et le titre.

On peut aussi séparer des sections du document par des lignes, avec la commande "---".

Par exemple :

```
# Titre 1

---

# Titre 2
```

### Les puces

Pour faire une liste d'éléments avec des puces devant, on met soit "*" soit "*" avant chaque élément.

Par exemple :

```
* Element 1
* Element 2
* Element 3
```
Comme pour les titres, il ne faut pas oublier de mettre un espace entre "*" / "-" et les éléments.

Les puces peuvent également être des numéros si on utilise l'écriture suivante :

```
1. Element 1
2. Element 2
3. Element 3
```

### Gras et italique

Pour faire ressortir certains mots dans les phrases, on peut les mettre en gras ou en italique.

Pour indiquer qu'un mot est en gras, on l'entoure de "**".

Par exemple :

```
**mots en gras**
```

Pour indiquer qu'un mot est en italique, on l'entoure de "_".

Par exemple :

```
_mots en italique_
```

### Liens et illustrations

Pour ajouter une image d'illustration dans le document, il faut utiliser la commande :

```
![texte_de_remplacement](chemin_de_l_image "légende")
```

Le texte de remplacement s'affichera si l'image n'est pas trouvée.
Le chemin de l'image doit être écrit en relatif par rapport au dossier dans lequel ce trouve le fichier Markdown.

Pour ajouter un lien vers un site web, il faut utiliser la commande :

```
[texte](url_du_site)
```

Le texte est celui qui sera affiché pour le lien.
L'utilisateur pourra juste cliquer dessus pour ouvrir le site web.

### Tableaux

On peut facilement afficher des tableaux avec Markdown, de la manière suivante :

```
|Colonne 1|Colonne 2|Colonne 3|
|:--------|:-------:|--------:|
|1        |2        |3        |
|4        |5        |6        |
```

Ce qui donnera ce tableau :

|Colonne 1|Colonne 2|Colonne 3|
|:--------|:-------:|--------:|
|1        |2        |3        |
|4        |5        |6        |

- Si on écrit ":---" les éléments de la colonne seront alignés à gauche.

- Si on écrit ":---:" les éléments de la colonne seront centrés.

- Si on écrit "---:" les éléments de la colonne seront alignés à droite.

## Compléter PyS1C_documentation.md

Maintenant que vous avez les bases pour écrire un document en Markdown, **écrivez une courte documentation pour notre projet**.

La documentation d'un projet Python contient en général les éléments suivants :

- Un court descriptif du projet et de ses objectifs.

- Comment importer le projet.

- Un descriptif des fonctions contenues dans le projet (entrées, sorties).

- Des tutoriels.

- Les références, les auteurs, la license du projet.

Si vous êtes curieux, le site web de ce tutoriel a été généré à partir de fichiers Markdown en utilisant un outil appelé "**MKDocs**", et est mis en ligne par une plateforme appelée "**ReadTheDocs**".
Il est assez courant que les projets Python open-source partagent leur documentation de cette manière.

Des outils d'hébergement de projets tels que "**GitHub**" sont également capables d'interpréter du Markdown pour afficher une documentation de manière lisible.

---

**Ce tutoriel est terminé !**
C'est maintenant à vous de jouer.
Réfléchissez au projet que vous voulez développer pour la prochaine séance.