# Exercices sur les index géographiques


**Exercice 1**

```js
var KilometresEnRadians = function(kilometres){ var rayonTerrestreEnKm = 6371;
return kilometres / rayonTerrestreEnKm;
};
var salle = db.salles.findOne({"adresse.ville": "Nîmes"});
var requete = { };
db.salles.find(requete ... };
```

Vous disposez du code JavaScript suivant qui comporte une fonction de conversion d’une distance exprimée en kilomètres vers des radians ainsi que d’un document dont les coordonnées serviront de centre à notre sphère de recherche. Écrivez la requête qui affichera le nom des salles situées dans un rayon de 60 kilomètres et qui programment du Blues ainsi que de la Soul dans leur liste de genres musicaux.
Voici la requete a effectuer :
```js
var KilometresEnRadians = function(kilometres) {
  var rayonTerrestreEnKm = 6371;
  return kilometres / rayonTerrestreEnKm;
};

var rayon = 60;
var rayonEnRadians = KilometresEnRadians(rayon);

var salle = db.salles.findOne({"adresse.ville": "Nîmes"});

var requete = {
  "adresse.localisation": {
    $geoWithin: {
      $centerSphere: [salle.adresse.localisation.coordinates, rayonEnRadians]
    }
  },
  "styles": {
    $all: ["blues", "soul"]
  }
};
var sallesTrouvees = db.salles.find(requete);

sallesTrouvees.forEach(function(salle) {
  print(salle.nom);
});
```



**Exercice 2**

```js
var marseille = {"type": "Point", "coordinates": [43.300000, 5.400000]}
 db.salles.find(...)
```

Écrivez la requête qui permet d’obtenir la ville des salles situées dans un rayon de 100 kilomètres autour de Marseille, triées de la plus proche à la plus lointaine :

```bash
db.salles.createIndex({ "adresse.localisation": "2dsphere" })
```

```js

var marseille = {"type": "Point", "coordinates": [43.300000, 5.400000]}
db.salles.find({
  "adresse.localisation": {
    $near: {
      $geometry: marseille,
      $maxDistance: 100000
    }
  }
}, {
  "adresse.ville": true
}).sort({
  "adresse.localisation": true
})
```
