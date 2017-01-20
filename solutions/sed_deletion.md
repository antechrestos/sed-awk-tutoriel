Afficher à l’écran le fichier data/sed/replace.txt sans la ligne 1: toto en utilisant la commande de suppression et un match d’expression régulière
```bash
sed '/^1:/ d' data/sed/replace.txt
```
Ici le pattern s'applique sur le charactère de début de ligne, le caractère 1 puis les deux points.
