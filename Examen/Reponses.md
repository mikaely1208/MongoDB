# Examen 01/02/2024


## 1. 

### Voici les données : 
```json
{
    person_id: "6392529400",
    firstname: "Elise",
    lastname: "Smith",
    dateofbirth: ISODate("1972-01-13T09:32:07Z"),
    vocation: "ENGINEER",
    address: {
      number: 5625,
      street: "Tipa Circle",
      city: "Wojzinmoj",
    },
  },
  {
    person_id: "1723338115",
    firstname: "Olive",
    lastname: "Ranieri",
    dateofbirth: ISODate("1985-05-12T23:14:30Z"),
    gender: "FEMALE",
    vocation: "ENGINEER",
    address: {
      number: 9303,
      street: "Mele Circle",
      city: "Tobihbo",
    },
  },
  {
    person_id: "8732762874",
    firstname: "Toni",
    lastname: "Jones",
    dateofbirth: ISODate("1991-11-23T16:53:56Z"),
    vocation: "POLITICIAN",
    address: {
      number: 1,
      street: "High Street",
      city: "Upper Abbeywoodington",
    },
  },
  {
    person_id: "7363629563",
    firstname: "Bert",
    lastname: "Gooding",
    dateofbirth: ISODate("1941-04-07T22:11:52Z"),
    vocation: "FLORIST",
    address: {
      number: 13,
      street: "Upper Bold Road",
      city: "Redringtonville",
    },
  },
  {
    person_id: "1029648329",
    firstname: "Sophie",
    lastname: "Celements",
    dateofbirth: ISODate("1959-07-06T17:35:45Z"),
    vocation: "ENGINEER",
    address: {
      number: 5,
      street: "Innings Close",
      city: "Basilbridge",
    },
  },
  {
    person_id: "7363626383",
    firstname: "Carl",
    lastname: "Simmons",
    dateofbirth: ISODate("1998-12-26T13:13:55Z"),
    vocation: "ENGINEER",
    address: {
      number: 187,
      street: "Hillside Road",
      city: "Kenningford",
    },
  }
```
### La consigne  :
> **Vous devez interroger une collection de personnes pour trouver les trois individus les plus jeunes qui ont un emploi dans le domaine de l'ingénierie, triés par le plus jeune en premier**

### Réponse :
```js
db.persons.aggregate([
  {
    $match: {
      vocation: "ENGINEER" // on cherche le métier 
    }
  },
  {
    $sort: {
      dateofbirth: -1 // on trie par date d'anniversaire la plus récente 
    }
  },
  {
    $limit: 3 // on se limite aux trois premiers 
  },
    {
        $project: {
        firstname: true, //on affiche le prenom
        dateofbirth: true, // la date de naissance
        vocation: true // et le métier 
        }
    }
])
```


## 2.

### Voici les données :

```json
{
    customer_id: "elise_smith@myemail.com",
    orderdate: ISODate("2020-05-30T08:35:52Z"),
    value: NumberDecimal("231.43"),
  },
  {
    customer_id: "elise_smith@myemail.com",
    orderdate: ISODate("2020-01-13T09:32:07Z"),
    value: NumberDecimal("99.99"),
  },
  {
    customer_id: "oranieri@warmmail.com",
    orderdate: ISODate("2020-01-01T08:25:37Z"),
    value: NumberDecimal("63.13"),
  },
  {
    customer_id: "tj@wheresmyemail.com",
    orderdate: ISODate("2019-05-28T19:13:32Z"),
    value: NumberDecimal("2.01"),
  },
  {
    customer_id: "tj@wheresmyemail.com",
    orderdate: ISODate("2020-11-23T22:56:53Z"),
    value: NumberDecimal("187.99"),
  },
  {
    customer_id: "tj@wheresmyemail.com",
    orderdate: ISODate("2020-08-18T23:04:48Z"),
    value: NumberDecimal("4.59"),
  },
  {
    customer_id: "elise_smith@myemail.com",
    orderdate: ISODate("2020-12-26T08:55:46Z"),
    value: NumberDecimal("48.50"),
  },
  {
    customer_id: "tj@wheresmyemail.com",
    orderdate: ISODate("2021-02-29T07:49:32Z"),
    value: NumberDecimal("1024.89"),
  },
  {
    customer_id: "elise_smith@myemail.com",
    orderdate: ISODate("2020-10-03T13:49:44Z"),
    value: NumberDecimal("102.24"),
  }
```

### La consigne  :

> **Vous devez générer un rapport pour montrer ce que chaque client de la boutique a acheté en 2020. Vous regrouperez les enregistrements de commandes individuelles par client, en notant la date du premier achat de chaque client, le nombre de commandes qu'ils ont passées, la valeur totale de toutes leurs commandes, et une liste de leurs articles de commande triés par date.**

### Réponse :

```js
db.orders.aggregate([
  {
    $match: {
      orderdate: {
        $gte: ISODate("2020-01-01T00:00:00Z"), // on cherche les dates supérieures à 2020
        $lt: ISODate("2021-01-01T00:00:00Z") // et inférieures à 2021
      }
    }
  },
  {
    $group: {
      _id: "$customer_id", // on groupe par client
      firstorderdate: {
        $min: "$orderdate" // on cherche la date de la première commande
      },
      numorders: {
        $sum: 1 // on compte le nombre de commande
      },
      totalvalue: {
        $sum: "$value" // on additionne la valeur de toutes les commandes
      },
      orders: {
        $push: {
          orderdate: "$orderdate", // on affiche la date de la commande
          value: "$value" // et la valeur de la commande
        }
      }
    }
  },
  {
    $project: {
      _id: 0,
      customer_id: "$_id",
      firstorderdate: true,
      numorders: true,
      totalvalue: true,
      orders: {
        orderdate: 1,
        value: 1
      }
    }
  },
  {
    $sort: {
      customer_id: 1 // on trie par client
    }
  }
])
```

## 3.

### Voici les données :

```json
{
    order_id: 6363763262239,
    products: [
      {
        prod_id: "abc12345",
        name: "Asus Laptop",
        price: NumberDecimal("431.43"),
      },
      {
        prod_id: "def45678",
        name: "Karcher Hose Set",
        price: NumberDecimal("22.13"),
      },
    ],
  },
  {
    order_id: 1197372932325,
    products: [
      {
        prod_id: "abc12345",
        name: "Asus Laptop",
        price: NumberDecimal("429.99"),
      },
    ],
  },
  {
    order_id: 9812343774839,
    products: [
      {
        prod_id: "pqr88223",
        name: "Morphy Richards Food Mixer",
        price: NumberDecimal("431.43"),
      },
      {
        prod_id: "def45678",
        name: "Karcher Hose Set",
        price: NumberDecimal("21.78"),
      },
    ],
  },
  {
    order_id: 4433997244387,
    products: [
      {
        prod_id: "def45678",
        name: "Karcher Hose Set",
        price: NumberDecimal("23.43"),
      },
      {
        prod_id: "jkl77336",
        name: "Picky Pencil Sharpener",
        price: NumberDecimal("0.67"),
      },
      {
        prod_id: "xyz11228",
        name: "Russell Hobbs Chrome Kettle",
        price: NumberDecimal("15.76"),
      },
    ],
  }
```

### La consigne  :

> **Vous souhaitez générer un rapport de vente au détail pour lister la valeur totale et la quantité de produits chers vendus (d'une valeur supérieure à 15 dollars). Les données sources sont une liste de commandes de magasin, où chaque commande contient l'ensemble des produits achetés dans le cadre de la commande. Operateur : _$unwind_**

### Réponse :

L'opérateur $unwind me sert à dérouler un tableau, il peut le transformer en une liste d'éléments.

```js
db.orders.aggregate([
  {
    $unwind: "$products" // on déroule les produits
  },
  {
    $match: {
      "products.price": {
        $gt: NumberDecimal("15.00") // on cherche les produits dont le prix est supérieur à 15(dollars)
      }
    }
  },
  {
    $group: {
      _id: null,
      totalvalue: {
        $sum: "$products.price" // on additionne la valeur de tous les produits
      },
      totalquantity: {
        $sum: 1 // on compte le nombre de produits
      }
    }
  },
  {
    $project: {
      _id: 0,
      totalvalue: true,
      totalquantity: true
    }
  }
])
```

## 4.

### Voici les données 

```js
{
    firstname: "Elise",
    lastname: "Smith",
    vocation: "ENGINEER",
    language: "English",
  },
  {
    firstname: "Olive",
    lastname: "Ranieri",
    vocation: "ENGINEER",
    language: ["Italian", "English"],
  },
  {
    firstname: "Toni",
    lastname: "Jones",
    vocation: "POLITICIAN",
    language: ["English", "Welsh"],
  },
  {
    firstname: "Bert",
    lastname: "Gooding",
    vocation: "FLORIST",
    language: "English",
  },
  {
    firstname: "Sophie",
    lastname: "Celements",
    vocation: "ENGINEER",
    language: ["Gaelic", "English"],
  },
  {
    firstname: "Carl",
    lastname: "Simmons",
    vocation: "ENGINEER",
    language: "English",
  },
  {
    firstname: "Diego",
    lastname: "Lopez",
    vocation: "CHEF",
    language: "Spanish",
  },
  {
    firstname: "Helmut",
    lastname: "Schneider",
    vocation: "NURSE",
    language: "German",
  },
  {
    firstname: "Valerie",
    lastname: "Dubois",
    vocation: "SCIENTIST",
    language: "French",
  }
```

### La consigne  :

> **Vous souhaitez interroger une collection de personnes où chaque document contient des données sur une ou plusieurs langues parlées par la personne. Le résultat de la requête doit être une liste alphabétiquement triée de langues uniques qu'un développeur peut ensuite utiliser pour peupler une liste de valeurs dans un widget de liste déroulante d'une interface utilisateur. Cet exemple est l'équivalent d'une instruction `SELECT DISTINCT` en SQL.**

### Réponse :

```js
db.persons.aggregate([
    {
        $unwind: "$language" // on déroule les langues
    },
    {
        $group: {
            _id: null, 
            languages: {
                $addToSet: "$language" // on ajoute les langues dans un tableau
            }
        }
    },
    {
        $sort: {
        // on trie ensuite par ordre alphabétique
            languages: -1
        }
    },
    {
        $project: {
            _id: 0,
            languages: 1
        }
    }
])
```


## 5.

### Voici les données 

```json
{
    id: "a1b2c3d4",
    name: "Asus Laptop",
    category: "ELECTRONICS",
    description: "Good value laptop for students",
  },
  {
    id: "z9y8x7w6",
    name: "The Day Of The Triffids",
    category: "BOOKS",
    description: "Classic post-apocalyptic novel",
  },
  {
    id: "ff11gg22hh33",
    name: "Morphy Richards Food Mixer",
    category: "KITCHENWARE",
    description: "Luxury mixer turning good cakes into great",
  },
  {
    id: "pqr678st",
    name: "Karcher Hose Set",
    category: "GARDEN",
    description: "Hose + nosels + winder for tidy storage",
  }


  {
    customer_id: "elise_smith@myemail.com",
    orderdate: ISODate("2020-05-30T08:35:52Z"),
    product_id: "a1b2c3d4",
    value: NumberDecimal("431.43"),
  },
  {
    customer_id: "tj@wheresmyemail.com",
    orderdate: ISODate("2019-05-28T19:13:32Z"),
    product_id: "z9y8x7w6",
    value: NumberDecimal("5.01"),
  },
  {
    customer_id: "oranieri@warmmail.com",
    orderdate: ISODate("2020-01-01T08:25:37Z"),
    product_id: "ff11gg22hh33",
    value: NumberDecimal("63.13"),
  },
  {
    customer_id: "jjones@tepidmail.com",
    orderdate: ISODate("2020-12-26T08:55:46Z"),
    product_id: "a1b2c3d4",
    value: NumberDecimal("429.65"),
  }
```

### La consigne  :

> **Vous souhaitez générer un rapport pour lister tous les achats en magasin pour 2020, en montrant le nom et la catégorie du produit pour chaque commande, plutôt que l'ID du produit. Pour ce faire, vous devez prendre la collection de commandes des clients et joindre chaque enregistrement de commande au produit correspondant dans la collection de produits. Il y a une relation plusieurs-à-un entre les deux collections, résultant en une jointure un-à-un lors de la correspondance d'une commande à un produit. La jointure utilisera une comparaison de champ unique entre les deux côtés, basée sur l'ID du produit. Commençons par choisir le jeu de données et le préparer pour le pipeline d'agrégation. Operateur: _$lookup_**

### Réponse :

```js
db.orders.aggregate([
  {
    $match: {
      orderdate: {
        $gte: ISODate("2020-01-01T00:00:00Z"), // on cherche les dates supérieures à 2020
        $lt: ISODate("2021-01-01T00:00:00Z") // et inférieures à 2021
      }
    }
  },
  {
    $lookup: {
      from: "products", // on fait une jointure avec la collection products
      localField: "product_id", // on cherche le produit par son id
      foreignField: "id", // on cherche le produit par son id
      as: "product" // on affiche le produit
    }
  },
  {
    $project: {
      _id: 0,
      customer_id: true,
      orderdate: true,
      product: {
        name: true,
        category: true
      }
    }
  },
  {
    $sort: {
      orderdate: 1 // on trie par date de commande
    }
  }
])
```


## 6.

### Voici les données 

```json
{
    name: "Asus Laptop",
    variation: "Ultra HD",
    category: "ELECTRONICS",
    description: "Great for watching movies",
  },
  {
    name: "Asus Laptop",
    variation: "Normal Display",
    category: "ELECTRONICS",
    description: "Good value laptop for students",
  },
  {
    name: "The Day Of The Triffids",
    variation: "1st Edition",
    category: "BOOKS",
    description: "Classic post-apocalyptic novel",
  },
  {
    name: "The Day Of The Triffids",
    variation: "2nd Edition",
    category: "BOOKS",
    description: "Classic post-apocalyptic novel",
  },
  {
    name: "Morphy Richards Food Mixer",
    variation: "Deluxe",
    category: "KITCHENWARE",
    description: "Luxury mixer turning good cakes into great",
  },
  {
    name: "Karcher Hose Set",
    variation: "Full Monty",
    category: "GARDEN",
    description: "Hose + nosels + winder for tidy storage",
  }

  {
    customer_id: "elise_smith@myemail.com",
    orderdate: ISODate("2020-05-30T08:35:52Z"),
    product_name: "Asus Laptop",
    product_variation: "Normal Display",
    value: NumberDecimal("431.43"),
  },
  {
    customer_id: "tj@wheresmyemail.com",
    orderdate: ISODate("2019-05-28T19:13:32Z"),
    product_name: "The Day Of The Triffids",
    product_variation: "2nd Edition",
    value: NumberDecimal("5.01"),
  },
  {
    customer_id: "oranieri@warmmail.com",
    orderdate: ISODate("2020-01-01T08:25:37Z"),
    product_name: "Morphy Richards Food Mixer",
    product_variation: "Deluxe",
    value: NumberDecimal("63.13"),
  },
  {
    customer_id: "jjones@tepidmail.com",
    orderdate: ISODate("2020-12-26T08:55:46Z"),
    product_name: "Asus Laptop",
    product_variation: "Normal Display",
    value: NumberDecimal("429.65"),
  }

```

### La consigne  :

> **Vous voulez générer un rapport pour lister toutes les commandes effectuées pour chaque produit en 2020. Pour ce faire, vous devez prendre une collection de produits d'une boutique et joindre chaque enregistrement de produit à toutes ses commandes stockées dans une collection de commandes. Il y a une relation un-à-plusieurs entre les deux collections, basée sur la correspondance de deux champs de chaque côté. Plutôt que de joindre sur un seul champ tel que product_id (qui n'existe pas dans ce jeu de données), vous devez utiliser deux champs communs pour joindre (product_name et product_variation).**


### Réponse :

```js
db.orders.aggregate([
  {
    $match: {
      orderdate: {
        $gte: ISODate("2020-01-01T00:00:00Z"), // on cherche les dates supérieures à 2020
        $lt: ISODate("2021-01-01T00:00:00Z") // et inférieures à 2021
      }
    }
  },
  {
    $lookup: {
      from: "products", // on fait une jointure avec la collection products
      let: {
        product_name: "$product_name", // on cherche le produit par son nom
        product_variation: "$product_variation" // on cherche le produit par sa variation
      },
      pipeline: [
        {
          $match: {
            $expr: {
              $and: [
                {
                  $eq: [
                    "$name",
                    "$$product_name" // on cherche le produit par son nom
                  ]
                },
                {
                  $eq: [
                    "$variation",
                    "$$product_variation" // on cherche le produit par sa variation
                  ]
                }
              ]
            }
          }
        }
      ],
      as: "product" // on affiche le produit
    }
  },
  {
    $project: {
      _id: 0,
      customer_id: true,
      orderdate: true,
      product: {
        name: true,
        variation: true
      }
    }
  },
  {
    $sort: {
      orderdate: 1 // on trie par date de commande
    }
  }
])
```

## 7.

### Voici les données :

 https://www.kaggle.com/datasets/joebeachcapital/tornados/data

### La consigne  :

> **Analyser le format des données et identifier les champs contenant des informations géographiques pertinentes (comme la localisation des tornades).Créer un index géospatial sur le champ contenant les informations de localisation des tornades.** 

### Réponse :
Creation de l'index géospatial sur le champ loc
```js
db.tornadoes.createIndex({loc: "2dsphere"})
```


## 8.


### La consigne  :

> **Écrire des requêtes (structure) pour répondre à plusieurs scénarios,  :**

    1. Identifier toutes les tornades survenues dans un rayon spécifique autour d'un point donné.

    2. Calculer le nombre de tornades par état ou région.

    3. Trouver les tornades les plus proches d'une ville spécifique.

### Réponse :

1. Structure de requetes pour identifier toutes les tornades survenues dans un rayon spécifique autour d'un point donné :

```js
db.tornadoes.find({ // on cherche les tornades
    loc: { 
        $near: { // on cherche les tornades les plus proches
            $geometry: {
                type: "Point", // on cherche les points
                coordinates: [longitude, latitude] // par les coordonnées
            },
            $maxDistance: 100000 // on spécifie la distance maximale ici 100000 mètres 
        }
    }
})
```

2. Structure de requetes pour calculer le nombre de tornades par état ou région :

```js
db.tornadoes.aggregate([
    {
        $group: {
            _id: "$state", // on groupe par état(région)
            count: {
                $sum: 1 // on compte le nombre de tornades
            }
        }
    },
    {
        $sort: {
            count: -1 // on trie par nombre de tornades
        }
    }
])
```

3. Structure de requetes pour trouver les tornades les plus proches d'une ville spécifique :

```js
db.tornadoes.aggregate([
    {
        $geoNear: {
            near: {
                type: "Point", // on cherche les points
                coordinates: [longitude, latitude] // par les coordonnées
            },
            distanceField: "distance.calculee", // on affiche la distance calculée
            maxDistance: 100000, // on spécifie la distance maximale ici 100000 mètres 
            query: {
                state: "villeauhasard" // on cherche les tornades dans l'état qu'on veut
            },
            includeLocalisation: "dist.location", // on affiche la localisation
            num: 3 // on se limite aux 3 premiers
        }
    }
])
```



