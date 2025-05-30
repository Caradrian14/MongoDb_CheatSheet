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

## Bases de datos VS Colecciones
En MongoDB, la estructura de almacenamiento de datos se organiza principalmente en bases de datos y colecciones.
### Base de Datos
Es un contenedor físico para las colecciones. Es como un espacio donde se agrupan conjuntos de datos relacionados. Se utilizan para organizar y separar datos en diferentes contextos o aplicaciones. Por ejemplo, podrías tener una base de datos para una aplicación de blog y otra para una aplicación de comercio electrónico. Puedes crear una base de datos simplemente seleccionándola con el comando use nombre_de_la_base_de_datos.

### Colecciones
Una colección en MongoDB es un grupo de documentos. Es similar a una tabla en las bases de datos relacionales, pero a diferencia de las tablas, las colecciones no requieren que todos los documentos tengan la misma estructura. Se utilizan para almacenar documentos que tienen un propósito o características similares. Por ejemplo, en una base de datos de blog, podrías tener una colección para los posts y otra para los comentarios. Una de las ventajas de MongoDB es que los documentos dentro de una colección pueden tener diferentes campos. Esto permite una gran flexibilidad en cómo se almacenan y organizan los datos.
