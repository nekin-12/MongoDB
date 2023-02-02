# Préparation des données:
a. Importez un jeu de données de localisation, comme les restaurants dans une ville. Vous pouvez utiliser la commande mongoimport pour importer des données dans MongoDB.

>![image](img/import.png)

b. Assurez-vous d'avoir un champ de localisation géospatiale, comme la latitude et la longitude.
>![image](img/coord.png)

# Requêtes MongoDB:
a. Recherchez les restaurants qui sont ouverts à partir de 18h00. Utilisez la méthode find () et les opérateurs de comparaison pour trouver les documents qui correspondent à vos critères.

> Étant donné que ma collection n'avait pas de `key` Heure ni de `value` qui va avec, je l'ai donc crée en utilisant `aggregate` pour récupérer les `value` aléatoirement:
>
> ```
> db.michelin.aggregate(
>   [
>      { $set: { Hour: { $multiply: [ { $rand: {} }, 20 ] } } },
>      { $set: { Hour: { $floor: "$Hour" } } },
>      { $merge: "michelin" }
>   ]
>)
> ```
> 
> Recherche des restaurants ouvrant à 18h00:
> ```
> db.michelin.find({"Hour":{$eq:18}}, {"Name": 1, "Location":1, "Price":1})
> ```

b. Triez les restaurants par note, du plus haut au plus bas. Utilisez la méthode sort () pour trier les résultats.

> ```
> db.michelin.find().sort({"Award": 1})
> ```

# Indexation avec MongoDB:
a. Créez un index sur le champ d'ouverture des restaurants pour améliorer les performances de la recherche. Utilisez la méthode createIndex ().

> ```
> db.michelin.createIndex({"Hour":1}, {name: "openHour"})
> ```

b. Vérifiez que l'index a été créé en utilisant la méthode listIndexes ().

> Comme on peut le constater l'inex `openHour` à bien été créé.
> ```
> db.michelin.getIndexes()
>[
>  { v: 2, key: { _id: 1 }, name: '_id_' },
>  { v: 2, key: { Hour: 1 }, name: 'openHour' }
>]
> ```

# Requêtes géospatiales:
a. Recherchez les restaurants qui se trouvent à moins de 2 km d'une certaine localisation. Utilisez la méthode find () avec un opérateur géospatial pour trouver les restaurants à l'intérieur d'un cercle.

>J'ai commencé par créer la variable `METERS_PER_MILE` ayant pour `value: 1609.34` (ce qui équivaut à 1 miles).
>
> ```
>var METERS_PER_MILE = 1609.34
>```
> Ensuite j'effectue la recherche et j'utilise ma variable pour établir la distance de 2km
>```
> db.michelin.find({ coord: { $nearSphere: { $geometry: { type: "Point", coordinates: [ -73.93414657, 40.82302903 ] }, $maxDistance: 2 * METERS_PER_MILE } } })
> ```
>![image](img/find-point.png)

b. Recherchez les restaurants qui se trouvent dans un certain rayon autour d'un point de localisation spécifique. Utilisez la méthode find () avec un opérateur géospatial pour trouver les restaurants à l'intérieur d'un cercle.

> ```
> db.michelin.find({ coord:{ $geoWithin: { $centerSphere: [ [ -73.93414657, 40.82302903 ], 5 / 3963.2 ] } } })
> ```
>![image](img/find-radius-circle.png)

# Framework d'agrégation:
a. Calculez la moyenne des notes des restaurants. Utilisez le framework d'agrégation de MongoDB pour effectuer des calculs sur les données.

> ```
> db.michelin.aggregate([
>    {"$group" : {"_id":"$Award", count:{"$sum" : 5}}}
> ])
> ```

b. Trouvez les restaurants les plus populaires en fonction du nombre de commentaires. Utilisez le framework d'agrégation de MongoDB pour groupes les données et effectuer des calculs.

> ```
> 
> ```

# Export de la base de données:
a. Exportez les résultats des requêtes dans un fichier CSV pour un usage ultérieur. Utilisez la commande mongoexport pour exporter des données de MongoDB.

> ```
> 
> ```

