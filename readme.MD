# TP Modelos y consultas básicas
## Implementación de Sequelize en el proyecto
1. Instalar los siguientes módulos:
~~~~
npm install sequelize-cli -g
npm install sequelize
npm install mysql2
~~~~
- `sequelize-cli` nos permitirá correr comandos de sequelize en consola. Se sugiere instalarlo de forma global
- `sequelize` y `mysql2` son las dependencias necesarias para implementar una base de datos creada con SQL en nuestro proyecto utilizando `sequelize` como ORM 

2. Crear el archivo `.sequelizerc` en la carpeta raíz del proyecto con el siguiente contenido:
~~~
const path = require('path')
module.exports = {
'config': path.resolve('./src/database/config', 'config.js'),
'models-path': path.resolve('./src/database/models'),
'seeders-path': path.resolve('./src/database/seeders'),
'migrations-path': path.resolve('./src/database/migrations'),
}
~~~
3. Ejecutar el comando `sequelize init` en consola. Esto creará una estructura de carpetas, según las rutas configuradas en el archivo `.sequelizerc`
4. Instalar el módulo  `dotenv` para implementar variables de entorno
~~~
npm i dotenv
~~~
5. Crear en la carpeta raíz del proyecto los archivos `.env` y `.env.example` donde en el primero irán el conjunto de clave y valor de las variables de entorno y en el segundo solo las claves. Cabe aclarar que el `.env` se debe incluir en el `.gitignore`
6. Escribir todas las variables de entorno que sean necesarias. Ejemplo:
~~~
DB_USERNAME=
DB_PASSWORD=
DB_DATABASE=
DB_PORT=
DB_HOST=
~~~
- `DB_USERNAME` Y `DB_PASSWORD` son las credenciales de acceso al servicio de MySQL.
- `DB_DATABASE` es el nombre del esquema de nuestra base de datos.
- `DB_PORT` es el puerto de conexión. Por defecto es *3306*. 
- `DB_HOST` es la direción IP donde está alojada nuestra base de datos. Por lo general en local es *127.0.0.1*
7. Implementar el uso de las variables de entorno escribiendo en la primera línea de `./app.js` y de `/database/config/config.js` la siguiente línea de código:
~~~
require('dotenv').config();
~~~
8. Exportar el objeto contenido en el archivo `/database/config/config.js` e implementar el uso de las variables de entornos creadas en el punto 6. Ejemplo:
~~~
module.exports = {
  development: {
    username: process.env.DB_USERNAME,
    password: process.env.DB_PASSWORD,
    database: process.env.DB_DATABASE,
    host: process.env.DB_HOST,
    dialect: "mysql",
    port : process.env.DB_PORT
  },
  test: {
    username: "root",
    password: null,
    database: "database_test",
    host: "127.0.0.1",
    dialect: "mysql",
  },
  production: {
    username: "root",
    password: null,
    database: "database_production",
    host: "127.0.0.1",
    dialect: "mysql",
  },
};
~~~
## Crear los modelos
### Modelo Movie
~~~
module.exports = (sequelize, dataTypes) => {

    const alias = "Movie";

    const cols = {
        id : {
            type : dataTypes.INTEGER.UNSIGNED, //se setea el tiempo de datos, en este caso un INTEGER y UNSIGNED, es decir un entero positivo
            autoIncrement : true, //es incrementable
            allowNull : false, //no soporta nulos
            primaryKey : true, //es la clave primaria
        },
        title : {
            type : dataTypes.STRING(500), //se setea el tipo de datos como una cadena de caracteres que recibe como máximo 500 caracteres
            allowNull : false, //no soporta nulos
        },
        rating : {
            type : dataTypes.DECIMAL(3,1).UNSIGNED, //se setea el tipo de datos como un DECIMAL que puede contener hasta 3 dígitos, donde solo uno se utiliza como decimal
            allowNull : false, //no soporta nulo
        },
        awards : {
            type : dataTypes.INTEGER.UNSIGNED, //se setea el tipo de datos como un entero positivo
            allowNull : false, //no soporta nulo
            defaultValue : 0 //se define el valor por defecto
        },
        release_date : {
            type : dataTypes.DATE, //se setea el tipo de datos como una fecha
            allowNull : false //no soporta nulo
        },
        length : {
            type : dataTypes.INTEGER, //se setea el tipo de datos como un entero
            allowNull : true //si soporta nulo
        },
        genre_id : {
            type : dataTypes.INTEGER, //se setea el tipo de datos como un entero
            allowNull : true //si soporta nulo
        },
    }

    const config = {
        tableName : "movies", //en el caso que el nombre de la tabla difiera del nombre del modelo
        timestamps : true, //se setea en false si no existen las columnas TIMESTAMPS (marcas de tiempo) en la tabla
        underscored : true, //se setea en true si el nombre las columnas de los TIMESTAMPS utilizan un guíon bajo (_) en vez de `camelCase`
    }

    const Movie = sequelize.define(alias,cols,config);
    
    return Movie

}
~~~