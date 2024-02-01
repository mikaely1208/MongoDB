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

## Aggregation

Les requetes d'aggregation permettent de traiter les données et de retourner un résultat. Concrètement, elles permettent de faire des jointures, des regroupements, des tris, des calculs, etc.

### Utilisation de l'aggregation

On peut utiliser la commande `db.collection.aggregate()` pour utiliser une aggregation. Par exemple ici on utilise une aggregation.

```bash
db.collection.aggregate([
    {$match: {nom: "Dupont"}},
    {$group: {_id: "$nom", total: {$sum: "$age"}}}
    {$project: {_id: 0, nom: "$_id", total: 1}}
])
```
### Les différents opérateurs d'aggregation sont :

- **$match** : Filtre les documents 

_Exemple_ : 
```bash
db.collection.aggregate({$match: {nom: "Dupont"}})
# Filtre les documents dont le nom est "Dupont"
```
- **$group** : Regroupe les documents

_Exemple_ : 
```bash
db.collection.aggregate({$group: {_id: "$nom", total: {$sum: "$age"}}})
# Regroupe les documents par nom et calcule la somme des âges
```

- **$project** : Modifie la structure des documents

_Exemple_ : 
```bash
db.collection.aggregate({$project: {_id: 0, nom: "$_id", total: 1}})
# Supprime le champ "_id" et renomme le champ "_id" en "nom"
```

- **$sort** : Trie les documents

_Exemple_ : 
```bash
db.collection.aggregate({$sort: {nom: 1}})
# Trie par ordre alphabétique croissant

db.collection.aggregate({$sort: {nom: -1}})
# Trie par ordre alphabétique décroissant
```


- **$limit** : Limite le nombre de documents

_Exemple_ : 
```bash
db.collection.aggregate({$limit: 5})
# Limite à 5 documents
```

- **$skip** : Ignore les documents

_Exemple_ : 
```bash
db.collection.aggregate({$skip: 5})
# Ignore les 5 premiers documents
```

- **$unwind** : Décompose les tableaux

_Exemple_ : 
```bash
db.collection.aggregate({$unwind: "$parents"})
# Décompose le tableau "parents"
```

- **$lookup** : Effectue une jointure

_Exemple_ : 
```bash
db.collection.aggregate({$lookup: {from: "collection", localField: "nom", foreignField: "nom", as: "resultat"}})
# Effectue une jointure entre la collection courante et la collection "collection" sur le champ "nom" et retourne le résultat dans le champ "resultat"
```




## Utilisation de pipeline 

On peut utiliser la commande `db.collection.aggregate()` pour utiliser un pipeline. Par exemple ici on utilise un pipeline. 

### Pipeline

Un pipeline est une suite d'opérations qui sont appliquées à un ensemble de documents. Les opérations sont appliquées dans l'ordre dans lequel elles apparaissent dans le pipeline.

```js
var pipeline = [
    {$match: {nom: "Dupont"}},
    {$group: {_id: "$nom", total: {$sum: "$age"}}}
    {$project: {_id: 0, nom: "$_id", total: 1}}
]
```

```bash
db.collection.aggregate(pipeline)
```



## GéoJSON

### Intégrez des données géographiques

On peut utiliser la commande `db.collection.insert()` pour insérer des données dans une collection. Par exemple ici on insère des données géographiques.

```bash
db.collection.insert({
    type: "Feature",
    properties: {
        name: "Paris",
        population: 2200000
    },
    geometry: {
        type: "Point",
        coordinates: [2.3522, 48.8566]
    }
})
```

On peut aussi intégrer des types différents de données géographiques tel que :

- Point
- MultiPoint
- LineString
- MultiLineString
- Polygon
- MultiPolygon
- GeometryCollection
- Feature
- FeatureCollection


### Recherchez des données géographiques

On peut utiliser la commande `db.collection.find()` pour rechercher des données dans une collection. Par exemple ici on recherche des données géographiques.

```bash
db.collection.find({
    geometry: {
        $geoIntersects: {
            $geometry: {
                type: "Point",
                coordinates: [2.3522, 48.8566]
            }
        }
    }
})
```

On peut utiliser des opération géographiques tel que :

- $geoIntersects : Retourne les documents dont la géométrie intersecte la géométrie spécifiée
exemple : `db.collection.find({geometry: {$geoIntersects: {$geometry: {type: "Point", coordinates: [2.3522, 48.8566]}}}})`

- $geoWithin : Retourne les documents dont la géométrie est entièrement contenue dans la géométrie spécifiée
exemple : `db.collection.find({geometry: {$geoWithin: {$geometry: {type: "Point", coordinates: [2.3522, 48.8566]}}}})`

- $near : Retourne les documents géographiquement proches d'un point
exemple : `db.collection.find({geometry: {$near: {$geometry: {type: "Point", coordinates: [2.3522, 48.8566]}}}})`

- $nearSphere : Retourne les documents géographiquement proches d'un point en utilisant la distance sphérique
exemple : `db.collection.find({geometry: {$nearSphere: {$geometry: {type: "Point", coordinates: [2.3522, 48.8566]}}}})`









  