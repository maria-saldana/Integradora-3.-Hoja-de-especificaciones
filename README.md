Guia de MongoDB Usos y comandos
INDICE
Como empezar
Iniciar servidor - mongod | mongo
Cofigurar teminal
Comandos basicos
Mostrar DBs - show dbs
Crear DB - use {db}
Mostrar DBs en uso- db
Crear Coleccion (Implicito) - .insert( )
Crear Coleccion (Explicito) - createCollection( )
Mostrar Colecciones - show collections
Eliminar Coleccion - .drop( )
Eliminar DB - .dropDatabase( )
CRUD
Crear
Un documento - .insert( )
Varios documentos - .insert( )
Arreglos - .insert( )
Mostrar
Todos - .find( )
Solo uno - .findOne( )
Parametro de busqueda - .find( { } )
Filtro de campos - .find( { }, { } )
Cursores - .find( )
Formato - .pretty( )
forEach - .forEach( )
count - .count( )
sort - .sort( )
limit - .limit( )
skip - .skip( )
count vs size .size( )
Arreglos
slice - .find( { }, { } )
in - .find( { }, { } )
Actualizar
save - .save( )
update - .update( )
Arreglos - .update( { }, { } )
Agregar - $push: { }
push - $push: { }
each - $each: [ ]
position - $position
sort - $sort
Eliminar
pull - $pull: {}
Eliminar
remove - .remove( )
Funciones
Varibles
Operadores
Comparacion
Logicos
Bucles
for
Avanzado
Grupos - aggregate( [ ] )
Agrupacion - $group { }
Repeticion de grupos - $sum : 1
Suma de campos - $sum:{ }
promedio - $avg: { }
Expresiones regulares - .find ( { } )
Like - / /
Cualquier zona - / /
Al final - / $/
Al inicio - /^ /




Como empezar
Iniciar servidor
Para iniciar el servidor primero es necesario una serie de pasos para poder usar los comandos de MongoDB

Configurar MongoDB y CMD
Configurar MongoDb para su uso
Ir a la unidad c:
crear carpetas: data/db
Configurar el cmd para usar comandos de MondoDB
Copiar la ruta bin en la carpeta de instalacion de MongoDB
Default C:\Program Files\MongoDB\Server\4.0\bin
Seguir la ruta panel de control > Sistema y seguridad > Sistema
Entrar a Configuracion Avanzada del Sistema
Variables de Entorno > Nuevo
Nombre: MongoDB | Valor: Ruta de carpeta bin
Si todo salio bien ya puedes acceder a los comandos de MongoDb desde el CMD




Arrancar el servidor
shell
mongod
2019-06-15T13:19:01.725-0500 I NETWORK  [initandlisten] waiting for connections on port 27017
Esto significa que el servidor ya esta activo


Para ingresar los comandos se necesita abir otra terminal de comandos ya que esta esta ocupada con el servidor

Para ingresar todos los comandos tenemos que activar los comandos de MongoDB

shell
mongo
Con esto ya podemos empezar a ingresar comandos






Comandos basicos
Mostrar las Bases de Datos
shell
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB



Crear DB
shell
> use Pruebas
switched to db Pruebas
Nota 1
Si la DB no existe entonces se va ha crear, pero esta no aparecera en la lista de DB hasta que se carguen datos en esta



Mostrar Base de Datos en uso
shell
> db
Pruebas



Crear Coleccion de manera implicita / Insertar datos en Coleccion
db.coleccion.insert( { documento })

db.nombreColeccion.insert([{ documento1 }, { documento2 }, ...])

shell
> db.persona.insert({
... "nombre": "Alan",
... "apellido": "Vazquez",
... })
WriteResult({ "nInserted" : 1 }) 
Nota 1
Se pueden insertar varias lineas en la terminal mientras no se cierren los parentesis de la funcion
Nota 2
En caso de que no exista la Coleccion en la que se quieren insertar datos se crea la Coleccion



Crear Coleccion de manera explicita
db.createCollection("nombreColeccion")

shell
> db.createCollection("productos")
{ "ok" : 1 }



Mostrar Colecciones
shell
> show collections
persona
productos



Eliminar Coleccion
db.nombreColeccion.drop()

shell
> db.productos.drop()
true



Eliminar DB
shell
> db.dropDatabase()
{ "dropped" : "Pruebas", "ok" : 1 }









CRUD
Crear
Un documento
db.nomreColeccion.insert({ nombreValor : valor, ...})

shell
> db.persona.insert({ nombre: "Alan", edad: 20 })
WriteResult({ "nInserted" : 1 })
Nota 1
Se pueden omitir las comillas en los nombres de los campos.
Nota 2
MongoDB inserta un id por defaul cuando este se omite
    "_id": ObjectId("<<Hash>>")
En caso de que se tenga un id personalizado para el documento es preferible usar el del documento.
se tiene que especificar como {"_id": <<Valor_Unico>>}
Nota 3
Se pueden insertar diferentes campos en la misma coleccion
    Cada documento puede tener sus propios campos distintos sin necesidad de alterar a los demas, como en caso de SQL



Varios documentos
db.nombreColeccion.insert ( [ { documento1}, { documento2 }, { ... } ] )

shell
> db.persona.insert([
... {
... nombre: "Isaac",
... edad: 20,
... activo: true
... },
... {nombre: "Fernanda",
... edad: 21
... }])
BulkWriteResult({
        "writeErrors" : [ ],
        "writeConcernErrors" : [ ],
        "nInserted" : 2,
        "nUpserted" : 0,
        "nMatched" : 0,
        "nModified" : 0,
        "nRemoved" : 0,
        "upserted" : [ ]
})



Arreglos - update
db.nombreColeccion.insert ( { nombreArreglo : [ valor1, valor2, ... ] } )

shell
> db.prueba.insert([
... {nombre: "test 1", miArreglo: [1, 5, 7, 9] },
... {nombre: "test 2", miArreglo: [8, 4, 3] }
... ] )
BulkWriteResult({
        "writeErrors" : [ ],
        "writeConcernErrors" : [ ],
        "nInserted" : 2,
        "nUpserted" : 0,
        "nMatched" : 0,
        "nModified" : 0,
        "nRemoved" : 0,
        "upserted" : [ ]
})





Mostrar
Todos
db.nombreColeccion.find()

shell
> db.persona.find()
{ "_id" : ObjectId("5d057016581553215508f3bc"), "nombre" : "Alan", "edad" : 20 }
{ "_id" : ObjectId("5d057354581553215508f3bd"), "nombre" : "Isaac", "edad" : 20, "activo" : true }
{ "_id" : ObjectId("5d057354581553215508f3be"), "nombre" : "Fernanda", "edad" : 21 }



Solo uno
db.nombreColeccion.findOne()

shell
> db.persona.findOne()
{
        "_id" : ObjectId("5d057016581553215508f3bc"),
        "nombre" : "Alan",
        "edad" : 20
}



Parametro de busqueda
shell
db.nombreColeccion.find( { parametrosBusqueda } )

> db.persona.find({edad:20})
{ "_id" : ObjectId("5d057016581553215508f3bc"), "nombre" : "Alan", "edad" : 20 }
{ "_id" : ObjectId("5d057354581553215508f3bd"), "nombre" : "Isaac", "edad" : 20, "activo" : true }
Nota 1
Se pueden insertar los parametros que se desee dentro de los mismo corchetes separando por ","



Filtro de campos
db.nombreColeccion.find( { parametrosBusqueda }, { campo1 : 1, campo2 : 0, ... } )

Se especifica un 0 o false para excluir el valor

Se especifica un 1 o true para mostrar el valor

shell
> db.persona.find({}, {_id:0})
{ "nombre" : "Alan", "edad" : 20 }
{ "nombre" : "Isaac", "edad" : 20, "activo" : true }
{ "nombre" : "Fernanda", "edad" : 21 }
shell
> db.persona.find({}, {nombre:1})
{ "_id" : ObjectId("5d057016581553215508f3bc"), "nombre" : "Alan" }
{ "_id" : ObjectId("5d057354581553215508f3bd"), "nombre" : "Isaac" }
{ "_id" : ObjectId("5d057354581553215508f3be"), "nombre" : "Fernanda" }
Nota 1
El _id siempre se mostrara al menos que se excluya manualmente



Cursores
Formato
db.nombreColeccion.find().pretty()

shell
> db.persona.find().pretty()
{
        "_id" : ObjectId("5d057016581553215508f3bc"),
        "nombre" : "Alan",
        "edad" : 20
}
{
        "_id" : ObjectId("5d057354581553215508f3bd"),
        "nombre" : "Isaac",
        "edad" : 20,
        "activo" : true
}
{
        "_id" : ObjectId("5d057354581553215508f3be"),
        "nombre" : "Fernanda",
        "edad" : 21
}



forEach
db.nombreColeccion.find.().forEach( bloqueDeFunciones )

Recorre la lista de documentos de una coleccion
shell
> db.cicloFor.find().forEach(
... function(d){ print( d.valor ) }
... )
0                     
1
2
3
...
100

count
db.nombreColeccion.find.().count()

shell
> db.cicloFor.find().count()
101

sort
db.nombreColeccion.find.().sort( { campoOrdenar : [1/-1] } )

shell
> db.orden.insert([
... {valor: "f"},
... {valor: "c"},
... {valor: "a"},
... {valor: "ab"},
... {valor: "aa"}
... ])
BulkWriteResult({
        "writeErrors" : [ ],
        "writeConcernErrors" : [ ],
        "nInserted" : 5,
        "nUpserted" : 0,
        "nMatched" : 0,
        "nModified" : 0,
        "nRemoved" : 0,
        "upserted" : [ ]
})
shell
> db.orden.find({},{_id: 0}).sort({valor:1})
{ "valor" : "a" }
{ "valor" : "aa" }
{ "valor" : "ab" }
{ "valor" : "c" }
{ "valor" : "f" }
shell
> db.orden.find({},{_id: 0}).sort({valor:-1})
{ "valor" : "f" }
{ "valor" : "c" }
{ "valor" : "ab" }
{ "valor" : "aa" }
{ "valor" : "a" }
Nota 1
Indicar el valor en el campo segun el orden deseado:
     1 = ascendente
    -1 = descendente 

limit
db.nombreColeccion.find.().limit( numeroValoresMostrar )

shell
> db.prueba.find({},{_id:0}).limit(5)
{ "valor" : 1 }
{ "valor" : 2 }
{ "valor" : 3 }
{ "valor" : 4 }
{ "valor" : 5 }

skip
db.nombreColeccion.find.().skip( numeroValoresOmitir )

shell
> db.prueba.find({},{_id:0}).skip(10).limit(5)
{ "valor" : 11 }
{ "valor" : 12 }
{ "valor" : 13 }
{ "valor" : 14 }
{ "valor" : 15 }

count vs size
db.nombreColeccion.find.( ).skip( numeroValoresOmitidos ).size( )

shell
> db.prueba.find({},{_id:0}).sort({valor:1}).skip(10).size()
90
shell
> db.prueba.find({},{_id:0}).skip(10).count()
100
Nota 1
count: Devuelve el numero total de documentos encontrados dentro el metodo find()
size: Devuelve el numero de documentos restantes dentro del skip [size = count - skip]



Arreglos - find
slice
db.nombreColeccionfind( { }, { nombreArreglo : {$slice : numeroElementos } } )

shell
> db.prueba.find({},{_id:false}).pretty()
{
        "miArreglo" : [
                "C#",
                "JavaScript",
                "Python",
                "Visual Basic",
                "Java"
        ]
}
shell
> db.prueba.find({},{_id:false, miArreglo: {$slice: 3}})
{ "miArreglo" : [ "C#", "JavaScript", "Python" ] }
shell
> db.prueba.find({},{_id:false, miArreglo: {$slice: -3}})
{ "miArreglo" : [ "Python", "Visual Basic", "Java" ] }
shell
> db.prueba.find({},{_id:false, miArreglo: {$slice: [1, 3]}})
{ "miArreglo" : [ "JavaScript", "Python", "Visual Basic" ] }



in
db.nombreColeccion.find( _{ nombreArreglo : { $in : [ valoresBusqueda ] } )

Para negra la busqueda, excluir las concidencias usar: $nin

shell
> db.prueba.find({},{_id:false}).pretty()
{
        "miArreglo" : [
                "C#",
                "JavaScript",
                "Python",
                "Visual Basic",
                "Java"
        ]
}
shell
> db.prueba.find({miArreglo: {$in: ["C#", "SQL"]} }, {_id: false} )
{ "miArreglo" : [ "C#", "JavaScript", "Python", "Visual Basic", "Java" ] }
Nota 1
Solo es necesario una concidencia de todos los parametros de busqueda para que este retorne el documento





Actualizar
Save
db.nombreColeccion.save( { _id o documento }, { campoNombre : nuevoValor } )

shell
> db.persona.save( { "_id" : ObjectId("5d057016581553215508f3bc") }, { edad : 30 })
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
Nota 1
Se debe indicar el campo "_id" porque sino se va ha crear un nuevo documento.
No basta con que los campos de busqueda sean unicos
Nota 2
En caso de que el campo ha actualizar no exista este se va ha agregar



Update
db.nombreColeccion.update(

{ valoresBusqueda },

{ $set : { valoresModificar, ValoresCrear }, $unset : {campoEliminar : valorEliminar} },

{ multi: boolean } )

$set: Establece que solo es campo se va ha modificar o crear. 
Sino se coloca esta expresion se tendra que ingresar el documento completo

$unset: Elimina los campos que concidan con el valor indicado

multi: Inidica que se van ha modificar todos los documentos que concidan con los campos de la busqueda en caso de que su valor sea "true".
Sino se especifica el valor por default es "false" por lo que solo se modificara el primer documento que se encuentre
shell
> db.persona.update({nombre: "Fernanda"}, {edad: 23, activo: true})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })



Arreglos
Agregar - update
push
db.nombreColeccion.update(

{ valoresBusqueda },

{ $push : { valoresAñadir } )


db.nombreColeccion.update( { },

{ $addToSet : { valoresAñadir } )

shell
> db.prueba.find({}, {_id:0})
{ "nombre" : "test , "miArreglo" : [ 8, 4, 3] }
shell
> db.prueba.update( {}, {$addToSet: {miArreglo: 2} })
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
shell
> db.prueba.find({}, {_id:0})
{ "nombre" : "test 1", "miArreglo" : [ 8, 4, 3, 2] }

shell
> db.prueba.update( {}, {$addToSet: {miArreglo: [6, 0]} })
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
shell
> db.prueba.find({}, {_id:0})
{ "nombre" : "test 1", "miArreglo" : [ 8, 4, 3, 2, [ 6, 0 ] ] }
Nota 1
Si el elemento ya existe dentro del Array este ya no se volvera ha agregar.
$push: permite agregar elementos existentes en el array



each
db.nombreColeccion.update( { }, { $push : { nombreArreglo : { $each : [ valor1, valor2, ... ] } )

shell
> db.prueba.find({}, {_id:0})
{ "nombre" : "test 1", "miArreglo" : [ 8, 4, 3 ] }
shell
> db.prueba.update( {},
... {$push: { miArreglo :
... {$each: [2, 3, 6] }
... } } )
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
shell
> db.prueba.find({}, {_id:0})
{ "nombre" : "test 1", "miArreglo" : [ 8, 4, 3, 2, 3, 6 ] }
Nota 1
En lugar de usar $push tambien se peuede utilizar $addToSet



position
db.nombreColeccion.update( { }, { $push : { nombreArreglo : { $position : indiceArreglo } )

shell
> db.prueba.find({}, {_id:0})
{ "nombre" : "test 1", "miArreglo" : [ 8, 4, 3 ] }
shell
> db.prueba.update( {},
... {$push: { miArreglo :
... {$each: [2, 3, 6], $position: 1}
... } } )
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
shell
> db.prueba.find({}, {_id:0})
{ "nombre" : "test 1", "miArreglo" : [ 8, 2, 3, 6, 4, 3 ] }
Nota 1
El metodo $position solo esta disponible con $push



sort
db.nombreColeccion.update( { }, { $push : { nombreArreglo : { $sort : [1/-1] } )

shell
> db.prueba.find({}, {_id:0})
{ "nombre" : "test 1", "miArreglo" : [ 8, 4, 3 ] }
shell
> db.prueba.update( {},
... {$push: { miArreglo :
... {$each: [2, 3, 6], $sort: 1}
... } } )
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
shell
> db.prueba.find({}, {_id:0})
{ "nombre" : "test 1", "miArreglo" : [ 2, 3, 3, 4, 6, 8 ] }
Nota 1
Este metodo se puede usar para ordenar el arreglo sino se le pasa ningun valor



Eliminar - update (arreglos)
pull
db.nombreColeccion.update( { }, { $pull : { nombreArreglo : valorArreglo )

db.nombreColeccion.update( { }, { $pullAll : { nombreArreglo : [ valor1, valor2, ... ] )

shell
> var arreglo = new Array();
> var tamanoArray = 20;
> for (i = 1; i <= tamanoArray; i++){
...     arreglo.push(Math.round(Math.random()*100));
... }
> db.prueba.insert({nombre: "test 1", miArreglo: arreglo} )
WriteResult({ "nInserted" : 1 })
shell
> db.prueba.find()
{ "_id" : ObjectId("5d08006569eac3f57c534494"), "nombre" : "test 1", "miArreglo" : [ 36, 80, 47, 14, 9 ] }
shell
> db.prueba.find({}, {_id:0})
{ "_id" : ObjectId("5d0801d069eac3f57c534495"), "nombre" : "test 1", "miArreglo" : [ 36, 80, 47, 14, 9, 69, 92, 21, 22, 65, 4, 36, 9, 22, 34, 23, 59, 97, 33, 92 ] }
shell
> db.prueba.find({}, {_id:0})
{ "_id" : ObjectId("5d0801d069eac3f57c534495"), "nombre" : "test 1", "miArreglo" : [ 36, 80, 47, 14, 9, 69, 92, 21, 22, 65, 4, 36, 9, 22, 34, 23, 59, 97, 33, 92 ] }
shell
> db.prueba.update({}, {$pull: {miArreglo: {$gte: 50} } } )
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
shell
> db.prueba.find({}, {_id: 0} )
{ "nombre" : "test 1", "miArreglo" : [ 36, 47, 14, 9, 21, 22, 4, 36, 9, 22, 34, 23, 33 ] }
Nota 1
Para eliminar varios valores manualmente se utiliza:
$pullAll: [valor1, valor2, ...]








Eliminar
Remove
db.nombreColeccion.remove( { camposBusqueda } )

A diferencia de la funcion update() la funcion remove() elimina todos los documentos que concidan con sus parametros de busqueda
shell
> db.persona.remove({ "_id" : ObjectId("5d057016581553215508f3bc") })
WriteResult({ "nRemoved" : 1 })









Funciones
Variables
var nombreVariable = script, funcion, documento, etc

Se puede asgnar un script a una variable y usarla cuando sea oportuno, como una funcion de una buqueda en especifico

shell
> var test = db.persona.findOne({nombre: "Alan"})
shell
> test
{
        "_id" : ObjectId("5d057016581553215508f3bc"),
        "nombre" : "Alan",
        "edad" : 20
}
shell
> test.edad
20
Nota 1
Se puede especificar el .edad debido a que hace uso de la funcion findOne().
Gracias a eso se puede acceder a los campos del resultado de la busqueda
Igualmente si se accede a un atributo que no existe este se crea





Operadores
Comparacion
$gt > (greater than)
$gte >= (greater than equals)
$lt < (less than)
$lte <= (less than equals)
shell
> db.persona.find({edad: {$gt: 20}}, {_id: 0})
{ "nombre" : "Fernanda", "edad" : 21 }

Logicos
$ne != (Negacion/diferente)
shell
> db.persona.find({nombre: {$ne: "Alan"}}, {_id: 0, nombre: 1})
{ "nombre" : "Isaac" }
{ "nombre" : "Fernanda" }





Bucles
for
for( varContador ; limiteCiclo ; aumento/decremento) { funciones }

shell
> for(i = 0; i <= 100; i++){
... db.cicloFor.insert({valor: i})
... }
WriteResult({ "nInserted" : 1 })
Esto inserta valores del 0 al 100 en la coleccion cicloFor










Avanzado
aggregate
group
Agrupacion
db.nombreColeccion.aggregate( [ { $group : " nombreCampoAgrupar "} ] )

shell
> db.peliculas.insert([
... {categoria: "accion", nombre: "2 Guns", valor: 100},
... {categoria: "accion", nombre: "Ant-Man", valor: 50},
... {categoria: "accion", nombre: "Avatar 3", valor: 150},
... {categoria: "Ciencia Ficcion", nombre: "Alien", valor: 200},
... {categoria: "Comedia", nombre: "The Adventure", valor: 300},
... {categoria: "Comedia", nombre: "Agua y Sal", valor: 250}
])
BulkWriteResult({
        "writeErrors" : [ ],
        "writeConcernErrors" : [ ],
        "nInserted" : 6,
        "nUpserted" : 0,
        "nMatched" : 0,
        "nModified" : 0,
        "nRemoved" : 0,
        "upserted" : [ ]
})
shell
> db.peliculas.aggregate( [ {$group : {_id: "$categoria"} } ] )
{ "_id" : "Comedia" }
{ "_id" : "Ciencia Ficcion" }
{ "_id" : "accion" }

Repeticion de grupos
shell
> db.peliculas.aggregate( [ {$group : {_id: "$categoria", "repetidos": {$sum: 1}} } ] )
{ "_id" : "Comedia", "repetidos" : 2 }
{ "_id" : "Ciencia Ficcion", "repetidos" : 1 }
{ "_id" : "accion", "repetidos" : 3 }

Suma de campos por grupo
shell
> db.peliculas.aggregate( [ {$group : {_id: "$categoria", "repetidos": {$sum: 1}, "sumaValor": {$sum: "$valor" } } } ] )
{ "_id" : "Comedia", "repetidos" : 2, "sumaValor" : 550 }
{ "_id" : "Ciencia Ficcion", "repetidos" : 1, "sumaValor" : 200 }
{ "_id" : "accion", "repetidos" : 3, "sumaValor" : 300 }

Promedio de grupos
shell
> db.peliculas.aggregate( [ {$group : {
..._id: "$categoria", 
..."repetidos": {$sum: 1}, 
..."sumaValor": {$sum: "$valor" }, 
...promedioValor: {$avg: "$valor"} 
...} } ] )
{ "_id" : "Comedia", "repetidos" : 2, "sumaValor" : 550 }
{ "_id" : "Ciencia Ficcion", "repetidos" : 1, "sumaValor" : 200 }
{ "_id" : "accion", "repetidos" : 3, "sumaValor" : 300 }





Expresiones regulares
'Like'
shell
> db.prueba.insert ([
... {correo: "alan@gmail.com"},
... {correo: "alan@outlook.com"},
... {correo: "test@live.mx"}
... ])
BulkWriteResult({
        "writeErrors" : [ ],
        "writeConcernErrors" : [ ],
        "nInserted" : 3,
        "nUpserted" : 0,
        "nMatched" : 0,
        "nModified" : 0,
        "nRemoved" : 0,
        "upserted" : [ ]
})

Cualquier zona
/ texto /

Busca contenido ingresado en cualquier parte del texto

shell
> db.prueba.find({correo: /@g/})
{ "_id" : ObjectId("5d08ec22ad79d8f46660eef4"), "correo" : "alan@gmail.com" }

Al final
/ texto$ /

Busca contenido ingresado al final del texto

shell
> db.prueba.find({correo: /mx$/})
{ "_id" : ObjectId("5d08ec22ad79d8f46660eef6"), "correo" : "test@live.mx" }

Al inicio
/ ^ texto /

busca contenido ingresado al inicio parte del texto

shell
> db.prueba.find({correo: /^a/})
{ "_id" : ObjectId("5d08ec22ad79d8f46660eef4"), "correo" : "alan@gmail.com" }
{ "_id" : ObjectId("5d08ec22ad79d8f46660eef5"), "correo" : "alan@outlook.com" }





About
Guia de comandos y usos de MongoDB

Resources
 Readme
 Activity
Stars
 3 stars
Watchers
 0 watching
Forks
 4 forks
Report repository
Releases
No releases published
Packages
No packages published
Contributors
1
@AlanIsaacV
AlanIsaacV Alan Vazquez
Footer
