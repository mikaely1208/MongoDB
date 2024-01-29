# Cours : Mango DB

## Objectifs
- Apprendre à importer et exporter de données dans différents formats.
- Optimiser l'usage des collections en utilisant l'indexation.
- Effectuer des requêtes pour extraire de façon efficace l'information contenue dans une base de données.
- Apprendre à optimiser le temps d'exécution des requêtes en utilisant des index<sup>*</sup>.
- Exploiter les fonctionnalités proposées par MongoDB.
- Étudier le stockage et la manipulation de fichiers volumineux dans MongoDB.


<sup>*</sup> Les index sont des structures de données qui permettent d'accélérer les requêtes en stockant les valeurs d'un champ ou d'un ensemble de champs spécifiés dans un ordre particulier. Les index sont similaires aux index d'un livre, qui facilitent la recherche d'informations spécifiques dans un livre. Les index peuvent être créés sur un seul champ ou sur plusieurs champs d'une collection.

## Le format JSON : Pourquoi ?

- <b>JSON</b> est un format universel très répandu.
- Aucune structure n'est imposée (seule condition la syntaxe).
- Les documents permettent de regrouper des informations.
- Les bases de données stockant des documents permettent la gestion d'une quantité immense d'information.

## La notation JSON

Les propriétés d'un objet JSON sont représentées par des paires clé/valeur. 

```json
{"clé": "valeur"; "autreclé": "autrevaleur"}
```
```
{
    "nom": "Dupont",
    "prenom": "Jean",
    "age": 25
    "parents" : [
        {"nom": "Dupont", "prenom": "Jean", "age": 25},
        {"nom": "Dupond", "prenom": "Jeanne", "age": 24}
    ]
}
```
Les types de données JSON sont :
- numérique
- chaîne de caractères
- booléen
- tableau
- objet
- null(absence de valeur)



## MongoDB : C'est quoi ?

<b>MongoDB</b> est un SGBD orienté documents et cross-platform. 

La base de données est le conteneur physique des collections. Chaque base de données possède son propre ensemble de fichiers sur le disque. Une seule instance de MongoDB peut gérer plusieurs bases de données.

Une collection est un ensemble de documents MongoDB. C'est l'équivalent d'une table dans un SGBDR. Ils n'imposent pas de schéma précis.

Les documents présents au sein d'une même collection peuvent avoir différents champs. 

Malgré cela, tous les documents d'une même collection sont généralement similaires ou ont un usage similaire.



## MongoDB vs SGDBR

| MongoDB | SGBDR |
| --- | --- |
| Base de données | Base de données |
| Collection | Table |
| Document | Tuple, Ligne, Row |
| Champ, Field | Colonne |
| Document imbriqué | Jointure |
| Clé primaire | Clé primaire |
| mongo | mysql |
| mongod | mysqld |

## Les avantages de MongoDB

- Pas de schéma imposé.
- Une structure de données simple : objet
- Pas de jointure complexe.
- Des requêtes imbriquées
- Des index

## Dans quel domaine ?

- Applications web
- Big Data
- Data Hub 

## Installation

- MongoDB Atlas
- Permet de gérer des clusters
- Se charge de maintenir et de déployer la BDD
- Utilise le fournisseur de cloud de notre choix

## MongoDB Compass

- Interface graphique pour MongoDB
- Permet de visualiser les données
- Permet de créer des requêtes
- Permet de créer des index
- Permet de créer des collections
- Permet de créer des bases de données

## Creation d'une base de données

On peut utilser la commande `use` pour créer une base de données.

```bash
use <nom de la base de données>
```

## Création d'une collection

On peut utiliser la commande `db.createCollection()` pour créer une collection.

```bash
db.createCollection("nom de la collection")
```

## Insertion de données

On peut utiliser la commande `db.collection.insert()` pour insérer des données dans une collection.

```bash
db.collection.insert({nom: "Dupont", prenom: "Jean", age: 25})
```

## Recherche de données

On peut utiliser la commande `db.collection.findOne()` pour rechercher des données dans une collection.

```bash
db.collection.findOne(<filtre>, {<projection>})
```

## Mise à jour de données

On peut utiliser la commande `db.collection.update()` pour mettre à jour des données dans une collection.

```bash
db.collection.update({nom: "Dupont"}, {$set: {age: 26}})
```

## Suppression de données

On peut utiliser la commande `db.collection.remove()` pour supprimer des données dans une collection.

```bash
db.collection.remove({nom: "Dupont"})
```

## Suppression d'une collection

On peut utiliser la commande `db.collection.drop()` pour supprimer une collection.

```bash
db.collection.drop()
```

## Suppression d'une base de données

On peut utiliser la commande `db.dropDatabase()` pour supprimer une base de données.

```bash
db.dropDatabase()
```

## Insert Many

On peut utiliser la commande `db.collection.insertMany()` pour insérer plusieurs documents dans une collection. Par exemple ici on en rentre 20 différentes.
    
```bash
db.collection.insertMany([
    {nom: "Dupont", prenom: "Dean", age: 21},
    {nom: "Dupont", prenom: "Jean", age: 22},
    {nom: "Dupont", prenom: "Kean", age: 23},
    {nom: "Dupont", prenom: "Qean", age: 24},
    {nom: "Dupont", prenom: "Lean", age: 25},
    {nom: "Dupont", prenom: "Oean", age: 26},
    {nom: "Dupont", prenom: "Aean", age: 27},
    {nom: "Dupont", prenom: "Pean", age: 28},
    {nom: "Dupont", prenom: "Tean", age: 29}
])
```





  