- en une ligne de commande afficher le fichier *data/sed/fruits.txt* en y ajoutant la banane **et** le durian à leur place. 
Dans ce cas, on va le faire en deux instructions. Noteez qu'on ajoute bien l'expression  `-e` pour chaque instruction dès qu'on en a plusieurs.

```bash
sed -e "/^a/ a banane" -e "/^c/a durian" data/sed/fruits.txt
```
	
- pareil que **1 ** mais en un seul bloc d'instruction. 
Dans ce cas on va entourer notre bloc d'instructions de ̀{}` et faire des retour à la ligne pour les séparer

```bash
sed  "/^c/ {
i banane
a durian
}" data/sed/fruits.txt
```

- ajouter le zatte à sa place sans utiliser le numéro de la ligne sous forme de numéro mais le charactère spécial désignant "*la dernière ligne*". 
Ici `$` désigne le caractère de fin de ligne. Bien utiliser des simples quotes pour éviter que le shell interprête le `$`!

```bash
sed '$ a zatte' data/sed/fruits.txt
```
