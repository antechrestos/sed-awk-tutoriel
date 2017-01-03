# Dojo Sed et Awk en bash

Prérequis: notions de base en `bash`

## Rappel sur les pipes

On entend souvent "piper des  process", faire un "pipe".... Mais c'est quoi au  juste?

- "Piper" un process ne consiste pas à en râper un des côtés pour gagner à tous les coups..
- Pour un bon "pipe" il faut deux.... processus

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

**Important** Le flux de données est un flux de lignes. Cette notion se retrouvera aussi pour `awk`.

### Substitution

La plus fréquentes utilisation est la substitution. Essayez donc cette commande:


```bash
echo "Je suis un grand fan de Robbie Williams" | sed -e "s?b*ie?bin?"
```

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
echo "Le miaulement du chat c'est mew mew mew" | sed "s?ew?iaou?g"
```

---

De même on peut remplacer:

- que le second: 

```bash
echo "Le miaulement du chat c'est mew mew mew" | sed "s?ew?iaou?2"
```

- tous à partir du second

```bash
echo "Le miaulement du chat c'est mew mew mew" | sed "s?ew?iaou?2g"
```

---

Aller plus loin capturer/réutiliser le résultat de l'expression régulière


### Sur les fichiers

Par défaut `sed` crache le résultat sur la sortie standard. Si on veut modifier un fichier on pourrait faire:
```bash
sed "s?expr1?expr2?g" mon_fichier.txt > mon_fichier.txt-next
mv -f mon_fichier.txt-next mon_fichier.txt 
```

Mais heureusement il y a l'option `-i`

Exercice: Faites passer `sed -i` sur *data/sed/replace.txt* et constatez qu'il a bien été mis à jour

### Suppression

Une autre commande peut être la suppression: Celà se fait avec `d`

Effectuez la commande

```bash
sed "d" data/sed/replace.txt
```

### Restriction de l'éxécution sur une sélection de lignes

---
#### Par numéro

Comme vu dans l'exemple précédent, effectuer une commande peut être utile en la restreignant sur certaines lignes

Effectuer la commande:

```bash
sed "11,$ d" data/sed/replace.txt
```

Ici on spécifie un range de ligne. Que veux dire le dollar? Comment le faire que sur une ligne?

---
#### Par match d'expression régulière

La synthaxe suivante permet de restreindre l'exécution d'une commande d'édition vue précedemment à une ligne respectant une expression régulière donnée:

```bash
sed "/<expression régulière>/ <commande d'édition>" 
```

**Exercice:**

 Afficher à l'écran le fichier *data/sed/replace.txt* sans la ligne `1: toto` en utilisant la commande de suppression **et** un match d'expression régulière.


### Insertion avant/après

La commande `i`/`a` permet pour une ligne donnée:

- `i` pour (**insert**): insérer la ligne **avant** la ligne courante
- `a` pour (**append**): ajouter la ligne **après** la ligne courante

**Exercices:**

1. en une ligne de commande afficher le fichier *data/sed/fruits.txt* en y ajoutant la banane **et** le durian à leur place
2. pareil que **1 ** mais en utilisant une seule fois l'argument `-e`
3. ajouter le zatte à sa place sans utiliser le numéro de la ligne sous forme de numéro mais le charactère spécial désignant "*la dernière ligne*"

### Autre commandes:

- `r <path>`: lire le contenu d'un fichier après la ligne

```bash
sed "/cerise/ r data/sed/replace.txt" data/sed/fruits.txt
```

- `w <path>`: sauvegarder dans un fichier

```bash
sed "/a/ w data/sed/new-file.txt" data/sed/fruits.txt
```

- `p` pour afficher.

**Exercices:**
 
- afficher les lignes entre  `cerise` et `mangue`. 
- sauvegarder ces mêmes lignes dans un nouveau fichier
- exercice supplémentaire: étant donné que `'1,3!d'`revient à supprimer toutes les lignes **sauf** celles allant de 1 à 3, refaire le premier exercice avec la commande `d`

### Niveau ++ - retour aux substitutions

- réutiliser le pattern matché en première instruction en seconde avec le caractère `&`
- transformer un pattern matché en majuscule avec `\U` ou minuscule `\L` 
- protéger un match avec `\(<pattern>\ )` et le réutiliser avec `\1`
```bash
sed '/orange/ s?\(a\).*?\1?' data/sed/fruits.txt
```

## Awk

### Présentation

Awk est un programme traitant une source de donnée. Il découpe cette source en "record" (par défaut un record correspond à une ligne) et ensuite découpe ce record en champs selon un séparateur (espace par défaut).
 Chacun de ces champs pourra être référencé par l'expression `$1` pour le premier champ, `$2`pour le second, ... et `$NF` pour le dernier, l'expression `NF` désignant le nombre de champs (*number of fields*).

Un programme awk est composé de série de blocs

Enfin la synthaxe rappelle un peu celle du C.

### Structure du programme awk

Comme dit précedemment un programme awk est une série de blocs:

```awk
'motif1 { action-1 } motif2 { action-2 } …'
```

- un motif est une condition (pattern sur la ligne/champ, numéro de ligne...). Si la condition est remplie, l'action est appliquée. Si aucun motif fourni,  l'action sera appliquée à toutes les lignes
- le bloc d'action n'est pas obligratoire; si omis toute la ligne est affichée

---


Que va afficher la commande suivante?
```bash
echo "titi toto tutu
toto
tata
titi tutu" | awk "/toto/ {print NR, \": Toto is here\" } /tutu/ {print NR, \": Tutu is here\"} /titi/ {print NR, \": Titi is here\"} {print NR, \": I am happy\"}"
```

### Synthaxe des motifs

Deux motifs spéciaux:

- `BEGIN` qui précède un bloc d'initialisation
- `END` pour le bloc de fonction finales
- expression (ex: `$3 ~ /toto/` signifie le champs n°3 matche le pattern toto. `NR==2` signifie qu'on est sur la ligne n°2
- expression1, expression2 pour les intervales (ex: `NR==1, NR==3` pour lignes entre 1 et 3)

---
**Exercice:**
Sur le fichier *data/awk/expertises.csv*

- Afficher uniquement les lignes où quelqu'un travaille dans le SI
- Afficher les noms uniquement (sans entête)
- Afficher uniquement le nom et le domaine d'expertise
- Afficher les noms des lignes comprises entre 2 et 4
- Afficher le nombre de personnes dans le SI
- Afficher les compétences des personnes qui ne sont pas dans le SI

**Pour info**: 

* `FS` (*field separator*) peut être valorisé après les blocs ou avec l'option `-F` (`" "`par défaut)
* `OFS`(*output field separator*) peut être lui aussi valorisé (`" "`par défaut)
* `NR`: number of record (numéro de ligne)
* `ORS`: le séparateur de record en sortie (`\n` par défaut)
* `print $1, $3` affiche `$1OFS$2ORS`

### Les variables

Définir une variable avec l'option `-v`  ou simplement après les blocs d'inscription

```bash
awk -v toto=1 '{print toto}' data/awk/expertises.csv
 # ou
awk  '{print toto}' toto=1  data/awk/expertises.csv 
```

### Les tableaux

La tableaux sont plutôt des dictionnaires qui peuvent être indexés par entiers mais aussi par chaîne de caractère.
On peut ainsi se servir de l'indexation pour avoir une clause d'unicité.

Pour itérer un tableau:

```awk
tableau["toto"]=3;
tableau[2]=1;
for(idx in tableau){print idx, "=", tableau[idx]}
```

---

**Exercice:**
- Afficher de manière unique les entités des personnes du fichier *data/awk/expertises.csv*, sur une ligne, séparées par `,`
- Afficher par entité le nombre de personnes
- Afficher la première personne de chaque entité (

### Ecrire dans un fichier

En awk les instructions suivantes sont valides
 
```bash
# affiche toute la ligne dans le fichier désigné par la valeur du premier champs
print > $1
# pareil mais en précisant l'extension
print > $1".txt"
# pareil en mode append
print >> $1".txt"
```
----

**Exercices:**

- Toujours à partir du fichier *data/awk/expertises.csv *, en une commande, créer un fichier csv par entité contenant le nom de la personne
- Sur le fichier *data/awk/config.ini*, séparer les properties en fichiers distincts ie. les propriétés de la section `[auth]` doivent se retrouver dans le fichier `auth.csv`

### Ecrire dans une variable shell

Pas vraiment possible mais en récupérant le résultat **et** en utilisant `eval` ça devrait pouvoir se faire

Exercice: Réaliser une commande imbriquée telle que si j'aurais le résultat suivant aux commandes:
```bash
$> echo $SI
Dupont Dupond
$> echo $Testing_Entity
Duran
$> echo $Inovation
DrMaboul DrIgor
```

### Quelques petits trucs pour finir
On peut affecter les variables `$i` comme par exemple:

```bash
awk -F, '{$1=++i FS $1;}{print }' data/awk/expertises.csv
```
 qui va préfixer le numéro de ligne et les séparateur à `$1`
 
 Exercice: faire la même chose avec `$0`. Que vaut `$1` après ajout du préfixe? Pourquoi?

## Merci à vous!
 











