- Afficher uniquement les lignes où quelqu'un travaille dans le SI

```bash
# Par pattern
awk -F, '$2 ~ /SI/ {print $0}' data/awk/expertises.csv
# ou plus simplement car le bloc par défaut est d'afficher la ligne
awk -F, '$2 ~ /SI/ ' data/awk/expertises.csv
# Par égalité
awk -F, '$2 == "SI"' data/awk/expertises.csv
```

- Afficher les noms uniquement (sans entête)

```bash
awk -F, 'NR > 1 {print $1}' data/awk/expertises.csv
awk -F, 'NR != 1 {print $1}' data/awk/expertises.csv

```

- Afficher uniquement le nom et le domaine d'expertise

```bash
# Ici on affiche le premier champs puis le troisième séparés par le séparateur de sortie OFS (" " par défaut)
awk -F, '{print $1, $3}' data/awk/expertises.csv
# en concaténant des chaines de caractère
awk -F, '{print $1 ":" $3}' data/awk/expertises.csv
```

- Afficher les noms des lignes comprises entre 2 et 4

```bash
awk -F, 'NR==2, NR==4{print $1}' data/awk/expertises.csv

```

- Afficher le nombre de personnes dans le SI

```bash
# Par défaut une variable non initialisée vaut nul (assimilable à zéro. On incrémente quand il faut, et on affiche à la fin
awk -F, '$2 ~ /SI/ {cpt++}END{print cpt}' data/awk/expertises.csv
```

- Afficher les compétences des personnes qui ne sont pas dans le SI

```bash
awk -F, '$2 !~ /SI/ {print $3}' data/awk/expertises.csv
```
