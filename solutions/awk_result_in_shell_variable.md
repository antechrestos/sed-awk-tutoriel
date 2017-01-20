Exercice: Réaliser une commande imbriquée telle que si j'aurais le résultat suivant aux commandes:
```bash
$> echo $SI
Dupont Dupond
$> echo $Testing_Entity
Duran
$> echo $Inovation
DrMaboul DrIgor
```

Dans un premier temps on va essayer d'avoir le résultat suivant:

```
Inovation="DrMaboul DrIgor"
SI="Dupont Dupond"
Testing_Entity="Duran"
```

Si on regarde le manual de awk (`man awk`), on voit la fonction `gsub` qui permet de faire de la substitution; ici, `gsub(" ", "_", $2)` va remplacer les espaces dans l'entité par des espaces soulignés. Ceci étant traité on obtient le résultat au dessus facilement:

```bash
awk -F"," -v OFS="=" 'NR > 1{gsub(" ","_", $2);if(!tableau[$2]){tableau[$2]=$1}else{tableau[$2]=tableau[$2] " " $1}}END{for (idx in tableau){print idx,"\"" tableau[idx] "\""}}' data/awk/expertises.csv
```

Maintenant in suffit de le passer à la fonction `eval` (on utilise la notation $(commande) qui permet de pré-exécuter la commande), puis `eval`évaluera la sortie standard de la première commande comme paramétre d'entrée

```bash
eval $(awk -F"," -v OFS="=" 'NR > 1{gsub(" ","_", $2);if(!tableau[$2]){tableau[$2]=$1}else{tableau[$2]=tableau[$2] " " $1}}END{for (idx in tableau){print idx,"\"" tableau[idx] "\""}}' data/awk/expertises.csv)
```

Il suffit de faire `echo $SI` pour constater le résultat
