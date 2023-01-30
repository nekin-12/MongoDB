Créez une base de données sample nommée "sample_db" et une collection appelée "employees".
Insérez les documents suivants dans la collection "employees":

{
   name: "John Doe",
   age: 35,
   job: "Manager",
   salary: 80000
}

{
   name: "Jane Doe",
   age: 32,
   job: "Developer",
   salary: 75000
}

{
   name: "Jim Smith",
   age: 40,
   job: "Manager",
   salary: 85000
}

# Exo

### Écrivez une requête MongoDB pour trouver tous les documents dans la collection "employees".
``` 
db.employees.find() 
```
> Permet d'afficher toute les données du document 

#

### Écrivez une requête pour trouver tous les documents où l'âge est supérieur à 33.
``` 
 db.employees.find({"age":{$gt:33}})
``` 
> Permet d'afficher seulement les données supèrieur à 33 par compris dans la clé "age"

#

### Écrivez une requête pour trier les documents dans la collection "employees" par salaire décroissant
``` 
db.employees.find().sort()({"salary":-1})
``` 
> Permet d'afficher toutes les données trier par rapport à la clé "salary" en ordre décroissant 

#

### Écrivez une requête pour sélectionner uniquement le nom et le job de chaque document.
``` 
db.employees.find({}, {"name": 1,"job":1})
``` 
> Récupère toute les données mais n'affiche que les données en rapport avec les clé "name" & "job" 

#

### Écrivez une requête pour compter le nombre d'employés par poste.
``` 
db.employees.aggregate([
    {"$group" : {"_id":"$job", count:{"$sum" : 1}}}
])
``` 
> Utilisation de l'aggregation qui permet de réaliser une serie d'opération sur la collection de document. Les opération "$group", "count" et "$sum" permettent de regrouper pour de compter et faire la somme des documents "id" ayant le même document "job"

#

### Écrivez une requête pour mettre à jour le salaire de tous les développeurs à 80000.
``` 

``` 
>

