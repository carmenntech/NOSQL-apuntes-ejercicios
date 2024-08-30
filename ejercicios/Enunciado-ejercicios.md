# ENUNCIADO EJERCICIOS 

 1- En la tabla __perfilesmongo__ selecionar a aquellos usuarios que no sean de Barcelona ni de Sevilla 

```
db.perfilesmongo.find(
  {"location" :
    {$nin : ["Sevilla", "Barcelona"]}
  }
)

```
 
 2- En la tabla __perfilesmongo__ selecionar el nombre de aquellos usuarios que tengan como jobtitle "Data Scientist"
 
```
db.perfilesmongo.find(
  {"jobtitle" : /Data Scientist/
  } , {_id:0, name:1}
)
```
 
 3- En la tabla __perfilesmongo__ selecionar a aquellos usuarios que se llamen "David" pero que se apelliden "Gil", además seleccionar a las personas que se apelliden "Fernandez" pero que no se llamen "Sergio"

```
db.perfilesmongo.find({
  $or: [
    { 
      $and: [
        { "name": /David/ },
        { "name": { $nin: [/Gil/] } }
      ] 
    },
    { 
      $and: [
        { "name": /Fernandez/ },
        { "name": { $nin: [/Sergio/] } }
      ] 
    }
  ]
})

```

 4- En la tabla __Keywords__ buscar los documentos que tienen fecha posterior a '2024-08-12'

```
db.keywords.find({ 
  "fecha": { $gte: new Date("2024-08-12") } 
})
```

5 - En la tabla __perfilesmongo__ cuenta el numero de documentos que tengan jobtitle 'Data Scientist'

```
db.perfilesmongo.find({ 
  "jobtitle": /Data Scientist/ } 
).count()
```

6 - En la tabla __todoskeywords__ busca todos los titulos unicos

```
db.todosKeywords.distinct("title")
```

7 - EN la tabla __todosKeywords__ cuenta el numero de documentos que en su descripcion aparezca la pablabra python

```
db.todosKeywords.find({ 
  "description": /python/ } 
).count()
```



8 - En la tabla __itemsperfilmongo__ busca los documentos con menos de 20 items 

Para hacer las consultas en Mongodb 

![image](https://github.com/user-attachments/assets/83aee315-7b4d-40e6-a1cd-6e9ca339b28b)


Para cambiar la coleccion que queremos trabajar

![image](https://github.com/user-attachments/assets/93d357c6-feea-4a67-aa55-dc90be13f2d9)
