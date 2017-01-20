En tapant:

```bash
awk -F, '{$0=++i FS $1;}{print $1}' data/awk/expertises.csv
```

On remarque que le premier champs vaut le numéro préfixé: ceci est dû au fait qu'affecter `$0` relance une passe de découpage des champs et la réaffectation de ces derniers. Celà peut être utile mais attention tout de même!
