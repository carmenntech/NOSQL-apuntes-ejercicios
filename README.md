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
| $eq  | Valores iguales a un valor especifico  |
| $gt  | Valores más altos a un valor especifico  |
| $gte  | Valores más altos o iguales a un valor especifico  |
| $lt  | Valores más pequeños a un valor especifico  |
| $lte  | Valores más pequeños o iguales a un valor especifico  |
| $ne  | Todos los valores que no son iguales a un valor especifico  |
| $in  | Alguno de los valores que coincida con los valores especificos del array |
| $nin  | Ningun valor que coincida con los valores especificos del array  |

Ejemplos:

```
db.movies.find(
    {"released" :
        {$lt : new Date('2000-01-01')}
    }
).count()

db.movies.find(
  {"rated" :
    {$in : ["G", "PG", "PG-13"]}
  }
)
```

### 🧡 Consultas Documentos Anidados

Ejemplo: Devuelve los documentos de la colección clientes que tengan el par provincia:Madrid en el documento embebido “dirección”.

```
db.clientes.find( {"dirección.provincia":"Madrid"})
```

Ejemplo: Devuelve el documento de la colección clientes que tenga exactamente el array especificado (exactamente los mismos elementos).

```
db.clientes.find({"telefonos":["914556677","677445566"]})
```

El operador __$all__ busca todos aquellos documentos donde el valor del campo contiene todos los elementos, independientemente de su orden o tamaño.

Ejemplo: Buscar todas las películas disponibles en English, French y Cantonese.

```
db.movies.find(
    {"languages":{
        "$all" :[ "English", "French", "Cantonese"]
    }},
    {"languages" : 1, "_id": 0})
```


### 🧡 Otras consultas útiles 

__Condicion LIKE (contine una cadena de caracteres)__

/A./ -> Todos los nombres que empiecen por A

/.u./ -> Todos los nombres que contengan la u

```
db.alumnos.find({“nombre": /A./})
db.clientes.find({"nombre":/.u./}) 
```

__Incluir / Excluir campos en la consulta__

1-> Incluir el campo 
0-> Excluir el campo

```
db.clientes.find({"nombre":{$eq:"Alfredo"}},{_id:1, ciudad:1})
db.clientes.find({"nombre":{$eq:"Alfredo"}},{ciudad:0})
```


