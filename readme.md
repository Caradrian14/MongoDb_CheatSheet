# Chear Sheet MongoDb

## Instalacion linux
- (Documentacion Oficial) [https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/#std-label-install-mdb-community-ubuntu]

- `sudo apt-get install gnupg curl`
- `curl -fsSL https://www.mongodb.org/static/pgp/server-8.0.asc | \sudo gpg -o /usr/share/keyrings/mongodb-server-8.0.gpg \ --dearmor`
- `echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-8.0.gpg ] https://repo.mongodb.org/apt/ubuntu noble/mongodb-org/8.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-8.0.list`
- `sudo apt-get update`
- `sudo apt-get install -y mongodb-org`

## Iniciar en linux
Vamos a inicializarlo en terminal
- Primero comprobamos que no este inicializado: `sudo systemctl status mongod`
- Si no lo esta lo inicializamos : `sudo systemctl start mongod`
- Si queremos arrancarlo al inicia sistema: `sudo systemctl enable mongod`

## Logs de MongoDb
Consultar los logs: `sudo journalctl -u mongod`

## Al empezar 
Cuando s einicial, aparece un 'test' parpadeante que es el que nos indica que estamos en la base de datos test que es la inicial
- Mostrar las bases de datos: `show dbs`
- se puede **crear o cambiar** (cuidado con esto) una base de datos usando : `use appdb` o el nombre de la bbdd
- Limpar la terminal de mongodb: `cls`
- Salir de la mongodb: `exit`
- Ver la bd de datos actual `db`

## Bases de datos VS Colecciones
En MongoDB, la estructura de almacenamiento de datos se organiza principalmente en bases de datos y colecciones.
### Base de Datos
Es un contenedor físico para las colecciones. Es como un espacio donde se agrupan conjuntos de datos relacionados. Se utilizan para organizar y separar datos en diferentes contextos o aplicaciones. Por ejemplo, podrías tener una base de datos para una aplicación de blog y otra para una aplicación de comercio electrónico. Puedes crear una base de datos simplemente seleccionándola con el comando use nombre_de_la_base_de_datos.

### Colecciones
Una colección en MongoDB es un grupo de documentos. Es similar a una tabla en las bases de datos relacionales, pero a diferencia de las tablas, las colecciones no requieren que todos los documentos tengan la misma estructura. Se utilizan para almacenar documentos que tienen un propósito o características similares. Por ejemplo, en una base de datos de blog, podrías tener una colección para los posts y otra para los comentarios. Una de las ventajas de MongoDB es que los documentos dentro de una colección pueden tener diferentes campos. Esto permite una gran flexibilidad en cómo se almacenan y organizan los datos.

## Borrar base de datos
para borrar `db.dropDatabase()` esto es general para todas las db.

## Inserccion
- `db.users.insertOne({ name: "John" })` inserta un dato
No hay esquemas no hay columnas, solo hay documentos que son objetos JSON. S epuede añador lo que quieras
- `db.users.insertOne({ name: "Sally", age: 19, address: { street: "978 North St"}, hobbies: ["Running"] })` se puede meter de todo basicamente
- `db.users.insertMany([{name: "Jill""}, {name: "Mike"}])` para insertar multiples

## encontrar datos
- `db.users.find()` todo
- `db.users.find().limit(2)` solo dos 
- `db.users.find().sort({ name: 1}).limit(2)` los entrega ordenados alfabeticamente
- `db.users.find().sort({ name: -1}).limit(2)` los entrega ordenados alfabeticamente a la inversa
- `db.users.find().sort({ age: -1 ,name: -1}).limit(2)` añador mas filtros
- `db.users.find().sort({ age: -1 ,name: -1}).skip(1).limit(2)` skip te permite s
altarte el primer dato, si varias el numero a 2 o 3 se salta ese numero de datos
- `db.users.find({ name: "Jill"})` busca exactamente lo que quieres en base a los parametrso que le pidas. Cuidado que diferencia entre strings y ints
- `db.users.find({ name: "Jill"}, {name: 1, age: 1, _id: 0})` Con este formato lo que hacemos es decirle que nos pase en el formato que queremos, ademas con el _id le decimos que n nos pase el id que tiene forma de hash.

- `db.users.find({ name: "Jill"}, {age: 0})` Con este formato nos devuelve todos los campos menos el que le hemos indicado que no

## Querys Complejos
- `db.users.find({ name: {$eq: "Jill"}})` nos da lo que le pidamos que sea
- `db.users.find({ name: {$ne: "Jill"}})` nos da todo lo que le pidamos que NO sea
- `db.users.find({ age: {$gt: 19}})` nos da todo lo que sea mas grande que lo que le hayamos pedido.
- `db.users.find({ age: {$gte: 19}})` nos da todo lo que sea mas grande o igual que lo que le hayamos pedido.
- `db.users.find({ age: {$lte: 19}})` nos da todo lo que sea mas pequeño o igual que lo que le hayamos pedido.
- `db.users.find({ age: {$lt: 19}})` nos da todo lo que sea mas pequeño que lo que le hayamos pedido.
- `db.users.find({ name: {$in: ["Paco", "Sally"]}})` nos da lo que este en el rango
- `db.users.find({ name: {$nin: ["Paco", "Sally"]}})` nos da lo que NO este en el rango
- `db.users.find({ age: {$gte: 20, $lte:40})` mas de 20 menos de 40
- `db.users.find({ age: {$gte: 20, $lte:40}, name: "Paco"})` mas de 20 menos de 40

### Existe
- `db.users.find({ age: {$exist: true}})` nos da lo que exista el campo indicado
- `db.users.find({ age: {$exist: false}})` nos da lo que no exista el campo indicado


### And
- `db.users.find({ $and: [{age: 26}, { name: "Paco"}]})` aunque no hace falta por qu ehay otras formas de hacerlo
### or
- `db.users.find({ $or: [{age: {lte: 26}}, { name: "Paco"}]})` ó
### not
- `db.users.find({ age : {$not: { $lte: 20} }})` negacion

### Comparacion de datos
- `db.users.find({$expr: { $gt: ["$debt", "$balance"]} })` comparacion, de que la deuda sea mayor que el balance de los usuarios. para cceder a las columnas se usa el $ 

## Updates
- `db.users.updateOne({ age:26},{$set: { age:27 }} )` actualizamos una edad

### Incrementar
- `db.users.updateOne({ age:26},{$inc: { age: 3 }} )` incrementamos una edad

### renombrar columna
- `db.users.updateOne({ age:26},{$rename: { name:"firstName" }} )` actualizamos una edad

### quitar columna 
- `db.users.updateOne({ age:26 }, { $unset: {age:""} })` deshace la coluna

### añade columna 
- `db.users.updateOne({ age:26 }, { $push: {hobbies:"paint Warhammers"} })` añade la coluna


### atualiza muchos
- `db.users.updateMany({ address:{$exists: true} }, { $unset: { address: ""}} )`

### replace
- `db.users.replaceOne( { age:30}, {name: "John"})` remplaza lo qu ehaya en el objeto por el objeto. Por lo general se prefiere usar update


## Delete
- `db.users.deleteOne( {name: "John"})` borrar
- `db.users.deleteMany({ age: {$exists: false}})` borra lo que no tenga eddad en la tabla.



## Operaciones de Agregación
Se dedican a procesar multiples documentos y devolver resultados computados. En otrtas palabras procesa los datos y los tranforman en otras cosas de interes.

Si asumimos que tenemos una coleccion de datos:
```
{
  "_id": ObjectId("..."),
  "producto": "Laptop",
  "cantidad": 5,
  "precio": 1000,
  "fecha": ISODate("2023-10-01T00:00:00Z")
}
```
Y queremos calcular el total de ventas que es cantitad por precio para cada producto.
Se puede usar la siguiente estructura con un pipline de agregación
```
db.ventas.aggregate([
  {
    $group: {
      _id: "$producto",
      totalVentas: { $sum: { $multiply: ["$cantidad", "$precio"] } }
    }
  }
])
  
```
Que es lo que hace:
- $group: Esta etapa agrupa los documentos por el campo especificado, en este caso, "$producto".
- _id: "$producto": Indica que queremos agrupar los documentos por el campo producto.
- totalVentas: Es un campo calculado que suma el resultado de multiplicar cantidad por precio para cada documento en el grupo.

## Indexaciones en MongoDb
Un índice en MongoDB es una estructura de datos que almacena una pequeña parte de la colección de datos de una manera que es fácil de traer. Los índices permiten a MongoDB resolver las consultas de manera eficiente.

### Conceptos de la indexación
- MongoDB crea automáticamente, y por defecto, un índice único en el campo _id durante la creación de una colección.

- Puedes crear índices en uno o más campos de una colección para mejorar el rendimiento de las consultas. `db.coleccion.createIndex({ nombre: 1 });` 1 indica que es orden ascentiente, -1 es descendiente.

- Tipos de indice
    - indice unico: Asegura que no haya documentos con valores duplicados en el campo indexado. `db.coleccion.createIndex({ email: 1 }, { unique: true });`
    - indice compuesto: `db.coleccion.createIndex({ nombre: 1, apellido: -1 });`
    - indice Multiclave: Índice en un campo que es un array.
    - Índice de Texto: Para realizar búsquedas de texto en contenido de strings.
    - Índice Hashed: Índice en el hash de un campo, útil para operaciones de sharding basadas en hash.

### Consideraciones
Mejora del Rendimiento de las Consultas: Mongo solo escanea pequeñas porciones del codigo y no todo el codigo. Ademas, con los indices se puedes devolver datos ya ordenados por lo que es mas eficiente.

- Hay que tener en cuenta que se realiza un consumo de memoria por cada indice, no queremos pasarnos.
- Menos velocidad a la hora de escribir, por que hay qu eactualizarlos.
- Se puede usar el metodo `explain()` y entender como se hacen las consultas. Por ejemplo `db.coleccion.find({ nombre: "Ejemplo" }).explain("executionStats");`


## Transacciones en MongoDb 
Permiten ejecutar múltiples operaciones como una única unidad atómica, lo que significa que todas las operaciones dentro de una transacción se completan o ninguna se completa. Esto es útil para mantener la consistencia de los datos en situaciones donde se deben realizar múltiples cambios relacionados en la base de datos.

MongoDB soporta transacciones en conjuntos de réplicas (replica sets) y en implementaciones de sharding.

Tenemos dos colecciones, `cuentas` y `transacciones`, y quieres transferir dinero de una cuenta a otra.

```
const session = db.getMongo().startSession();
session.startTransaction();

try {
    const cuentas = session.getDatabase('tuBaseDeDatos').cuentas;
    const transacciones = session.getDatabase('tuBaseDeDatos').transacciones;

    // Queremos transferir 100 de la cuenta A a la cuenta B
    const cuentaA = cuentas.findOne({ nombre: "CuentaA" });
    const cuentaB = cuentas.findOne({ nombre: "CuentaB" });

    if (!cuentaA || !cuentaB) {
        throw new Error("Una de las cuentas no existe");
    }

    if (cuentaA.saldo < 100) {
        throw new Error("Saldo insuficiente en la CuentaA");
    }

    // Actualizar saldos
    cuentas.updateOne(
        { nombre: "CuentaA" },
        { $inc: { saldo: -100 } },
        { session }
    );

    cuentas.updateOne(
        { nombre: "CuentaB" },
        { $inc: { saldo: 100 } },
        { session }
    );

    // Registrar la transacción
    transacciones.insertOne(
        {
            de: "CuentaA",
            a: "CuentaB",
            cantidad: 100,
            fecha: new Date()
        },
        { session }
    );

    // Confirmar la transacción
    session.commitTransaction();
} catch (error) {
    // Abortar la transacción en caso de error
    console.error("Error durante la transacción:", error);
    session.abortTransaction();
} finally {
    session.endSession();
}

```

### Consideraciones
- Las transacciones solo están disponibles en conjuntos de réplicas de MongoDB y no en implementaciones standalone. Asegúrarse de que todos los nodos del conjunto de réplicas estén ejecutando una versión de MongoDB que soporte transacciones (versión 4.0 o superior).

- Las transacciones requieren una sesión. Usa startSession para iniciar una sesión y session.startTransaction para comenzar una transacción.

-Todas las operaciones dentro de una transacción deben ser atómicas. Todas las operaciones se completan con éxito o ninguna se aplica.

- Es crucial manejar los errores adecuadamente y abortar la transacción si algo sale mal.

- Las transacciones pueden tener un impacto en el rendimiento, especialmente en sistemas con alta carga de operaciones. Usa transacciones solo cuando sea necesario para mantener la consistencia de los datos.

- Las transacciones tienen un límite de tiempo de ejecución. Si una transacción se ejecuta durante demasiado tiempo, puede ser abortada automáticamente.

- MongoDB utiliza el nivel de aislamiento "snapshot" para las transacciones, lo que significa que las operaciones dentro de una transacción ven un snapshot consistente de los datos desde el inicio de la transacción.
