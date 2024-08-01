# Apuntes NoSQL 

## 1. Consultas de selección 

### 🧡 Consultas básicas

+ __Mostrar todos los documentos de una colección__

` db.usuarios.find() `

+ __Mostrar segun una condición de igualdad__

$eq se usa como comparador de igualdad 

```
db.alumnos.find({"author": “Juanito"})
db.alumnos.find({“autor”:{$eq:”Juanito”}}
```
+ __Funcion pretty__

Con la función pretty muesta un resultado bien formateado.

` db.alumnos.find({"author": “Juanito"}).pretty() `

+ __Funcion distinct__

Usamos distinct para mostrar los valores unicos de un campo. Por ejemplo, para mostrar los usuarios unicos de la coleccion usuarios

` db.usuarios.distinct("users") `

### 🧡 Operadores logicos

+ __$and Operator__

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

+ __$or Operator__

``
db.alumnos.find ({$or: [{"Edad":{$gte:18}},{"nombre":"Belen"}]})
``

+ __$not Operator__

Ejemplo: Devuelva todas las películas que no tienen 5 o más comentarios:

``
db.movies.find(
  {"num_mflix_comments" :
    {$not: {$gte : 5}}
  }
)
``

+ __Combinación de varias condiciones__

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
+ __Operador $exists__

Este operador devuelve todos los documentos que tienen o no un determinado campo

```
db.alumnos.find({“titulación”: {$exists:true}})
db.alumnos.find({“edad”: {$exists:false}})
```

+ __Operador $type__

Este operador devuelve los documentos cuyo campo sea de un determinado tipo

```
db.alumnos.find ({“edad”: {$type: “int”}})
db.pacientes.find({“dirección”: {$type:”string”}})
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

+ __Condicion LIKE (contine una cadena de caracteres)__

/A./ -> Todos los nombres que empiecen por A

/.u./ -> Todos los nombres que contengan la u

```
db.alumnos.find({“nombre": /A./})
db.clientes.find({"nombre":/.u./}) 
```

+ __Incluir / Excluir campos en la consulta__

1-> Incluir el campo 

0-> Excluir el campo

```
db.clientes.find({"nombre":{$eq:"Alfredo"}},{_id:1, ciudad:1})
db.clientes.find({"nombre":{$eq:"Alfredo"}},{ciudad:0})
```

## 2. Modificar o insertar documentos 

+ Método __update__ modifica un documento o documentos existentes en una colección

```
db.alumnos.update (
{"_id": 11},
{$set : {“titulación" :”GII” }})
```

Si el campo “titulación” no existe en el documento, éste se añade, en caso contrario, se actualiza su valor

+ __Operadores de Update__

| Nombre  | Descripcion |
| ------------- | ------------- |
| $set  | Actualiza el valor en el campo del documento  |
| $unset  | Eleminar el valor en el campo del documento  |
| $setOnInsert  | Actualiza el dato solo en caso de insertarlo, no en el caso de modificarlo si ya existiera  |
| $inc  | Incrementa el valor del campo segun una especifica cantidad  |
| $mul  | Multiplica el valor del campo segun una especifica cantidad   |
| $rename  | Renombra el campo  |
| $min  | Solo actualiza el campo si el valor especifico es menor que el valor del campo existente  |
| $max  |  Solo actualiza el campo si el valor especifico es mayor que el valor del campo existente  |
| $currentDate  | Mete la fecha actual como valor de campo  |

+ __opcion upsert:true__

Es necesario poner la opción upsert al valor true (por defecto, FALSE). De esta forma se actualizará, si existe, el documento y en caso contrario, se insertará

Actualización o update (si se encuentra) o inserción insert (si no se encuentra), que se abrevia con upsert.

```
db.alumnos.update(
{ “Edad": { $gte: 18 } },
{ $set: { "mayorEdad": true }, $inc:{“Edad”:1} },
{ upsert: true }
)
```
+ __insertar documentos__
  
Se puede usar tanto la funcion insert o la funcion insertMany, que toma un array de documentos.

```
db.new_movies.insert({"_id": 2, "title": "Baby Driver"})
db.new_movies.insert({"_id": 3, "title" : "Logan"})
```

```
db.new_movies.insertMany([
    {"_id" : 2, "title": "Baby Driver"},
    {"_id" : 3, "title": "Logan"}
])
```

## 3. Eliminar documentos

+ __Método deleteMany__

Borra todos los documentos de una colección que satisfagan la condición:

`db.alumnos.deleteMany ({“campus": “Móstoles})`

+ __Método remove__

Borra los documentos de la colección, donde la clave “clave” sea igual al “valor”

`db.colección.remove({“clave”:”valor”})`

Borra todos los documentos de una colección

`db.colección.remove({})`

Tambien para borrar todos los documentos de una coleccion podemos usar 

`db.colección.drop ()`

## 4.Data Aggregation (Framework)

La aggregation pipeline le permite definir una serie de etapas que filtran, fusionan y organizan datos con mucho más control que el comando de find estándar.

El elemento clave de la agregación se llama pipeline. Un pipeline es una serie de instrucciones, donde la entrada a cada instrucción es la salida de la anterior.

Aggregate sintaxis

```
var pipeline = [] // Pipeline es una matriz de etapas.
var options = {}
db.movies.aggregate(pipeline, options)
```

pipeline sintaxis

```
var pipeline = [
        { $match: { "location.address.state": "MN"} },
        { $project: { "location.address.city": 1 } },
        { $sort: { "location.address.city": 1 } },
        { $limit: 3 }
     ];
```
![image](https://github.com/user-attachments/assets/62e87e5b-3891-40c7-a333-a51f09c4f805)

### Operadores Aggregation

+ __$sample__

Devuelve un conjunto de N documentos de una colección al azar. Ejemplo: Obtener tres provincias al azar:

```
db.libros.aggregate([{$sample: { provincias: <3>}}])
```
+ __$match__

Filtra los documentos que cumplen las condiciones, esta etapa conviene ponerla al principio. 
Ejemplo; filtra los libros por autor 

`db.libros.aggregate( [{$match : {autor : "Cervantes"}}])`

+ __$project__

Pasan a la siguiente etapa con los campos especificados. Los campos pueden ser preexistentes o creados en esta etapa.

  - <campo>: <1 o true> -- se incluye el campo (también el _id)
  - <campo>: <0 o false> -- se excluye (e impide cualquier otro tipo de especificación, salvo       excluir campos, en este caso por defecto sí aparecen)
  - _id: <0 o false> -- se suprime el _id (por defecto, sí se  incluye)
  + <campo>: <expresión> -- añade o resetea el valor
  + <camponuevo>: $<campo> -- para renombrar un campo
  + En embebidos, “a.b.c” : <1/0/exp> o a:{b:{c: <1/0/exp>}}

ejemplos:
  
```
Devuelve únicamente el Nombre de las provincias y su _id:

db.provincias.aggregate ([{$project:{"Nombre":1}}])

Se renombra el campo Nombre y se excluye el _id:

([{$project:{"Provincia":"$Nombre","_id":0}} ])
```

+ __$sort__

Ordena los documentos. Ejemplo: Devuelve únicamente el Nombre de las provincias y su _id, ordenados ascendentemente por la Superficie de la provincia

`db.provincias.aggregate ([{$sort:{Superficie:1}},
{$project:{"Nombre":1}}])`


+ __$limit__

Limita el número de documentos que pasan a la siguiente etapa, sin afectar al contenido

`db.libros.aggregate( { $limit : 5 });`

+ __$out__

Escribe la salida a una colección, debe ser la última etapa del pipeline

` $out: "<colección de salida>" }`

+ __$group__

La etapa $group le permite agrupar (o agregar) documentos en función de una condición específica.

La implementación básica de la etapa $group acepta solo una clave _id , siendo el valor una expresión. El campo _id es obligatorio.

Esta expresión define los criterios por los cuales la canalización agrupa los documentos.

Este valor se convierte en el _id del documento recién generado con un documento generado para cada _id único que crea la etapa $group.

Al agregar, necesitamos decirle a la canalización que queremos acceder al campo del documento que está agregando actualmente.

![image](https://github.com/user-attachments/assets/07dd04a3-3810-4702-b9d6-2313779946ec)

+ __$unwind__

Descompone un documento en tantos documentos como elementos tenga el array.

```
{"_id": 1, "item": "ABC1", tallas: ["S","M","L"]}

db.inventory.aggregate([{$unwind : "$tallas" }])

{ "_id" : 1, "item" : "ABC1", "tallas" : "S" }
{ "_id" : 1, "item" : "ABC1", "tallas" : "M" }
{ "_id" : 1, "item" : "ABC1", "tallas" : "L" }
```

+ __$sortByCount__

Agrupa los documentos y los cuenta por grupos, devolviendo en el resultado el _id correspondiente a la agrupación y un valor con el número de documentos

Es equivalente a $group + $sort


+ __$addFields__

Añade nuevos campos a documentos, manteniendo los que existen. Es equivalente a un $project que mantenga todos los campos y añada otros nuevos. Si el campo ya existe (incluyendo _id), se
sobreescribe.

+ __$bucket__

Clasifica los documentos en grupos (buckets) 

![image](https://github.com/user-attachments/assets/4ebe8a79-db74-4147-8639-bff78391f2a2)

$bucketAuto

![image](https://github.com/user-attachments/assets/019d96bc-3f2a-4867-9e94-cd3569d8d91d)

## 5. Joins en NoSQL

En MongoDB, los joins de colecciones se realizan mediante el paso de agregación $lookup.

![image](https://github.com/user-attachments/assets/12a34e1c-fd4a-4cdd-9614-15946de0405e)

![image](https://github.com/user-attachments/assets/e35dc559-8c0d-49ef-bf1e-8eb307703f10)

