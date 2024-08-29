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
 
´´´

´´´
 
 3- En la tabla __perfilesmongo__ selecionar a aquellos usuarios que se llamen "David" pero que se apelliden "Gil", además seleccionar a las personas que se apelliden "Fernandez" pero que no se llamen "Sergio"

´´´

´´´

 4- En la tabla __Keywords__ buscar los documentos que tienen fecha posterior a '2024-08-12'

´´´

´´´


Para hacer las consultas en Mongodb 

![image](https://github.com/user-attachments/assets/83aee315-7b4d-40e6-a1cd-6e9ca339b28b)


Para cambiar la coleccion que queremos trabajar

![image](https://github.com/user-attachments/assets/93d357c6-feea-4a67-aa55-dc90be13f2d9)
