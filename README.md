# Apuntes NoSQL 

## 1. Consultas de selección 

### 🧡 Consultas básicas

__Mostrar todos los documentos de una colección__

` db.usuarios.find() `

__Mostrar segun una condición de igualdad__

$eq se usa como comparador de igualdad 

```
db.alumnos.find({"author": “Juanito"})
db.alumnos.find({“autor”:{$eq:”Juanito”}}
```
__Funcion pretty__

Con la función pretty muesta un resultado bien formateado.

` db.alumnos.find({"author": “Juanito"}).pretty() `

__Funcion distinct__

Usamos distinct para mostrar los valores unicos de un campo. Por ejemplo, para mostrar los usuarios unicos de la coleccion usuarios

` db.usuarios.distinct("users") `

### 🧡 Operadores logicos

__$and Operator__

Películas sin clasificación que se lanzaron en 2008:

``
db.movies.countDocuments(
  {$and :
    [{"rated" : "UNRATED"}, {"year" : 2008}]
  }
)
``

En las consultas de MongoDB, el operador $and está implícito y se incluye de forma predeterminada si un documento de consulta tiene más de una condición.

``
db.movies.countDocuments (
  {"rated": "UNRATED", "year" : 2008}
)
``

__$or Operator__

``
db.alumnos.find ({$or: [{"Edad":{$gte:18}},{"nombre":"Belen"}]})
``

__$not Operator__

Ejemplo: Devuelva todas las películas que no tienen 5 o más comentarios:

``
db.movies.find(
  {"num_mflix_comments" :
    {$not: {$gte : 5}}
  }
)
``

__Combinación de varias condiciones__

Encontrar los títulos y años de estreno de películas de drama o crimen en cuya producción han colaborado Leonardo DiCaprio y Martin Scorsese.

```
db.alumnos.find( {
$and : [
{ $or : [ { "nombre" :"Juanito" }, {"nombre" :"Belen" } ] },
{ $or : [ { "Edad" : 18 }, { "Edad": { $gt : 18 } } ] }
]
} )
```

### 🧡 Operadores condicionales


| Nombre  | Descripcion |
| ------------- | ------------- |
| $eq  | Content Cell  |
| $gt  | Content Cell  |
| $gte  | Content Cell  |
| $lt  | Content Cell  |
| $lte  | Content Cell  |
| $ne  | Content Cell  |
| $in  | Content Cell  |
| $nin  | Content Cell  |




