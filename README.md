# Dojo Sed et Awk en bash

Prérequis: notions de base en `bash`

## Rappel sur les pipes

On entend souvent "piper des  process", faire un "pipe".... Mais c'est quoi au  juste?

- "Piper" un process ne consiste pas à en râper un des côtés pour gagner à tous les coups..
- Pour un bon "pipe" il faut deux.... processus

---

Exemple d'un pipe:
```bash
echo -n "pipe me"  | wc -c
```
Dans l'exemple ci-dessus on a réalisé un pipe entre deux processus
- `echo -n "pipe me"`: va écrire sur **sa** sortie standard la chaine de charactère `"pipe me"`
- `wc -c` va compter les chactactères écrits sur **son ** entrée sandard.

---
**En bref**: un pipe est le branchement de la **sortie standard** d'un premier processus sur l'**entréé standard** d'un second processus.

Peut s'enchaîner pour créer des **pipelines**:

```bash
ls -1 | grep \.py$ | wc -l
```


## Sed

### Présentation
Comme son  nom l'indique (stream editor), sed permet de faire de l'édition sur un flux de données (textuelles). Il s'utilise très souvent sur des pipe (ça tombe bien).

### Substitution
La plus fréquentes utilisation est la substitution. Essayez donc cette commande:


```bash
echo "Je suis un grand fan de Robbie Williams" | sed -e "s?b*ie?bin?"
```
---

- Un premier s pour "substitute"
- Le séparateur ×`?`: cela peut être un `/`... J'ai même essayé avec un `r`et ça fonctionne...Mais cela devient peu lisible!
- une expression régulière
- le séparateur (ici il doit être le même que le premier sinon on s'en sort pas)
- l'expression de remplacement
- le séparateur

---
Essayez donc la commande:
```bash
echo "Le miaulement du chat c'est mew mew mew" | sed -e "s?ew?iaou?"
```
---

Seulement une seule occurence a été changée; ajoutez donc le flag `g` à votre expression de sed:
```bash
echo "Le miaulement du chat c'est mew mew mew" | sed -e "s?ew?iaou?g"
```
### Sur les fichiers
Par défaut `sed`crache le résultat sur la sortie standard. Si on veut modifier un fichier on pourrait faire:
```bash
sed -e "s?expr1?expr2?g" mon_fichier.txt > mon_fichier.txt-next
mv -f mon_fichier.txt-next mon_fichier.txt 
```
---
Mais heureusement il y a l'option `-i`

Exercice: Faites passer `sed -i`sur *data/sed/replace.txt* et constatez qu'il a bien été mis à jour

### Autre commande: la suppression

Une autre commande peut être la suppression: Celà se fait avec `d`

Effectuez la commande
```bash
sed -e "d" data/sed/replace.txt
```

### Exécution de la commande sed sur numéro de ligne

Comme vu dans l'exemple précédent, effectuer une commande peut être utile en la restreignant sur certaines lignes

---
Effectuer la commande:

```bash
sed -e "11,$ d" data/sed/replace.txt
```
Ici: on spécifie un range de ligne. Que veux dire le dollar? Comment le faire que sur une ligne?

### Par match d'expression régulière
La synthaxe suivante permet de restreindre l'exécution d'une commande d'édition vue précedemment à une ligne respectant une expression régulière donnée:

```bash
sed -e "/<expression régulière>/ <commande d'édition>" 
```

---
Exercice:
Supprimer du fichier *data/sed/replace.txt* la ligne `1: toto` en utilisant la commande de suppression **et** un match d'expression régulière.



