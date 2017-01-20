- Afficher de manière unique les entités des personnes du fichier *data/awk/expertises.csv*, sur une ligne, séparées par `,`
Ici l'idée est de se servir de l'unicité des clefs sur un dictionnaire d'une part, et modifier le séparateur de record en sortie (`ORS`))

```bash
awk -F"," -v ORS=, 'NR > 1 {tableau[$2]=1} END{for(idx in tableau){print idx;}}' data/awk/expertises.csv
```

- Afficher par entité le nombre de personnes
Ici encore on se sert de l'unicité des clefs d'un tableau. Petite astuce, si une ntrée d'un tableau n'est pas initialisée, sa valeur par défaut est `null`qui vaut `0`en valeur numérique. On modifie le séparateur de champs de sortie (`OFS` qui vaut `" "` par défaut) pour rendre le résultat plus présentable 


```bash
awk -F"," -v OFS=": " 'NR > 1 {tableau[$2]++} END{for(idx in tableau){print idx, tableau[idx];}}' data/awk/expertises.csv
```

- Afficher la première personne de chaque entité
Ici encore on se sert d'un tableau. Comme précédemment, une entrée non initialisée peut être vue comme une valeur fausse. Ainsi la seule condition `!tableau[$2]` se traduit *"si l'entrée de l'entité est nulle"*....

```bash
awk -F"," -v OFS=": " 'NR > 1 {if(!tableau[$2]){tableau[$2]=$1}} END{for(idx in tableau){print idx, tableau[idx];}}' data/awk/expertises.csv
```