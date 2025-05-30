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
