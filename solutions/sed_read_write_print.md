- afficher les lignes entre  `cerise` et `mangue`. 
Plusieurs solutions ici. Il est à noter qu'il faut utiliser l'option `-n` de `sed`pour le rendre silencieux quand on ne lui a pas demandé d'afficher quelque chose.

```bash
#Par numéro de ligne
sed -n '2,10 p' data/sed/fruits.txt
# Par expression régulière
sed -n '/^c/,/^m/ p' data/sed/fruits.txt

```

- sauvegarder ces mêmes lignes dans un nouveau fichier
Pareil que ci dessus mais en utilisant l'opération `w`

```bash
#Par numéro de ligne
sed -n '2,10 w file1.txt' data/sed/fruits.txt
# Par expression régulière
sed -n '/^c/,/^m/ file2.txt' data/sed/fruits.txt
```

- exercice supplémentaire: étant donné que `'1,3!d'`revient à supprimer toutes les lignes **sauf** celles allant de 1 à 3, refaire le premier exercice avec la commande `d`
Facile quand on a fait le premier:

```bash
#Par numéro de ligne
sed  '2,10! d' data/sed/fruits.txt
# Par expression régulière
sed  '/^c/,/^m/! d' data/sed/fruits.txt
```
