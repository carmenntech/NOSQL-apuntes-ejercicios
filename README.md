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







