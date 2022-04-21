# MapReduce

URL ➡️ https://pdos.csail.mit.edu/6.824/papers/mapreduce.pdf

## État du fichier

Notes prises sur l'intro et petite vue du code C++ à la fin. Là tout de suite il manque à peu près tout
ce qui est important: comment les données sont partitionnées, c'est quoi GFS, les exemples de ce qu'on
peut faire avec et des idées d'implémentation.

Il faut croiser avec le reste de [6.824 2022 Lecture 1](https://pdos.csail.mit.edu/6.824/notes/l01.txt).

## Abstract

**Concept, objectif**

- MapReduce a été conçu chez Google.
- À la fois une façon de concevoir certaines taches et une implémentation de celle ci.
- MapReduce permet de traiter et de produire de très gros datasets.

**map et reduce**

- Beaucoup de calculs sur de grands ensembles de données peuvent être implementés sur le modèle de MapReduce.
- L'association des fonctions map et reduce, issues des langages fonctionnels.
- Dans MapReduce, on manipule de grands dictionnaires (paires clés/valeurs).
- L'objectif est de laisser l'utilisateur fournir les deux fonctions à passer respectivement à map et à
reduce sous forme de programmes.

**programmes conçus pour le parallélisme**

- Les programmes écrits de cette manière peuvent être parallélisé (clusters) assez facilement.
- Pour map, c'est évident, puisque la fonction utilisateur s'applique sans effets de bords.
- Pour reduce c'est plus compliqué.

**runtime system**

MapReduce est un *runtime system*, capable de partitionner les données d'entrées, de configurer l'éxecution des
programmes sur un cluster de machines et de gérer les communications nécessaires entre celles-ci. (Quelles
communications?)

**chez Google**

>hundreds of MapReduce programs have been implemented and upwards of one thousand MapReduce jobs are
>executed on Google’s clusters every day.

## 1. Introduction

**chez Google, types de programmes**

- En entrée on a souvent des grandes quantités de données brutes (pages indexées, logs, etc.)
- Sur plusieurs années des centaines de programmes spécialisés pour ces traitements ont été écrits.
- Pour produire des *index inversés*, des représentations des graphs qui décrivent la structure des pages indexées,
le nombre de pages visitées par nom de domaine, les requêtes les plus fréquentes sur une journée etc.

**problématiques adressées par MapReduce**

- Ces calculs sont simples mais distribuer la charge sur des milliers de machines est nécessaire pour
finir en un temps raisonnable.
- Distribuer les données et gérer les erreurs.
- MapReduce répond à la complexité engendrée par ces besoins et toutes les implémentations redondantes
en identifiant un pattern.

**pattern**

Beaucoup de programmes peuvent se résumer à:

1. Appliquer une "map operation" sur chaque enregistrement pour produire un ensemble de paires clé/valeur
intermédiaire.
2. Appliquer une "reduce operation" sur toutes les valeurs qui partagent la même clé.

**re-execution**

>[...] re-execution as the primary mechanism for fault tolerance.

L'état est conservé sur disque ?

**library**

MapReduce serait une librairie dans le cas qu'étudie l'article.

## Sections

- **2.** Basic programming model and examples
- **3.** an implementation of MapReduce
- **4.** refinements of the programming model
- **5.** performance measurements
- **6.** Experience at Google
- **7.** related and future work

## 2. Programming Model

- MapReduce récupère ce que map produit et pour chaque clé, le passe à reduce.
- reduce produit classiquement une ou zéro valeur par clé.
- les valeurs intermédiaires sont fournies à reduce via un itérateur de manière
à gérer des listes trop grandes pour la RAM

### Countword en pseudocode

```
map(String key, String value):
  // key: document name
  // value: document contents
  for each word w in value:
    EmitIntermediate(w, "1");

reduce(String key, Iterator values):
  // key: a word
  // values: a list of counts
  int result = 0;
  for each v in values:
    result += ParseInt(v);
  Emit(AsString(result));
```

- Noter l'itérateur en argument de reduce, tant que map appelle EmitIntermediate j'imagine.
- Il manque la spécification mapreduce qui donne les fichiers d'entrée et de sortie et quelques options
- L'utilisateur appelle la fonction MapReduce en lui passant la spécification.
- Le code de l'utilisateur est linké avec la librairie MapReduce (C++)

### L'implémentation dans l'appendice A

- On a une classe pour map (hérite de Mapper) et une pour reduce (Reducer).
- Des types/classes pour représenter les données (MapInput, ReduceInput).
