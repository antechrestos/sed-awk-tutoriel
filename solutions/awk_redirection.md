- Toujours à partir du fichier *data/awk/expertises.csv *, en une commande, créer un fichier csv par entité contenant le nom de la personne
Facile

```bash
awk -F"," 'NR > 1 {print $1 >> $2".txt" }' data/awk/expertises.csv
```

- Sur le fichier *data/awk/config.ini*, séparer les properties en fichiers distincts ie. les propriétés de la section `[auth]` doivent se retrouver dans le fichier `auth.csv`
Cet exercice est bien plus compliqué. Déjà on choisit les séparateurs `[`et `]` comme ceci `-F[][]`.
Ensuite si on regarde bien, on voit que les entêtes on deux séparateurs. On peut donc "s'échauffer en tapant la commande `awk -F[][] '{print NF, $0}' data/awk/config.ini` qui va nous afficher le nombre de champs par lignes. Une fois cela vu, nous allons pourvoir discriminer les débuts de sections et les sections elles mêmes: un début de section a trois champs, le nom de la section étant en seconde position (chaîne vide en première et troisième position). Les propriétés ont moins de trois sections.

```bash
awk -F[][] 'NF == 3 {current_section=$2} NF == 1 {print $1 >> current_section".properties"}' data/awk/config.ini
```

- Dans le cas précédents nous pourrions être mis en défaut si un petit malin entrait une propriété du genre `property_name = [ value1, value2, value3]`. En effet nous n'avons pas été assez discriminents sur le premier, et trop sur le second. Pour être parfait, nous aurions dû écrire:

```bash
# Basé sur les motifs
awk -F[][] 'NF == 3 && $1 == "" && $3 == "" {current_section=$2} NF >= 1 {print $1 >> current_section".properties"}' data/awk/config.ini
# sans motif, avec une expression sinon si
awk -F[][] '{if(NF == 3 && $1 == "" && $3 == ""){current_section=$2} else if (NF >= 1){print $1 >> current_section".properties"}}' data/awk/config.ini
```
