### 1. Título
Gestión y persistencia de bases de datos PostgreSQL en contenedores Docker.
---
![image](https://github.com/user-attachments/assets/3269f058-a3fb-4575-9c9f-a24037815401)
---
### 2. Tiempo de duración
120 minutos.
---
### 3. Fundamentos
Docker es una plataforma que permite ejecutar aplicaciones en entornos aislados llamados contenedores. Cada contenedor incluye todo lo necesario para que la aplicación funcione, independientemente de la máquina donde se implemente.
PostgreSQL, por su parte, es un sistema de gestión de bases de datos relacional. En esta práctica, se utiliza Docker para ejecutar dos instancias de PostgreSQL:
- Una sin volumen, donde los datos no persisten al eliminar el contenedor.
- Otra con un volumen, que asegura la persistencia de datos.

Conceptos clave:
- Contenedores: Entornos aislados que encapsulan aplicaciones.
- Docker Volumes: Espacios de almacenamiento externos al contenedor que garantizan que los datos no se pierdan al eliminar el contenedor.
- Persistencia de datos: La capacidad de mantener los datos almacenados más allá del ciclo de vida del contenedor.


### 4. Conocimientos previos
Para realizar esta práctica, el estudiante necesita dominar:
- Comandos básicos de Docker (docker run, docker exec, docker stop, etc.).
- Fundamentos de bases de datos relacionales (creación de tablas, consultas SQL).
- Configuración de herramientas como DataGrip o TablePlus para conectarse a bases de datos.


### 5. Objetivos a alcanzar
- Implementar y gestionar contenedores PostgreSQL con y sin volúmenes.
- Configurar la persistencia de datos en Docker.
- Realizar operaciones básicas en bases de datos relacionales.


### 6. Equipo necesario
- Computador con sistema operativo Windows/Linux/Mac.
- Docker v20.x instalado.
- Administrador de bases de datos (como DataGrip o TablePlus).


### 7. Material de apoyo
- Documentación oficial de Docker.
- Documentación oficial de PostgreSQL.
- Guía de asignatura.
- Docker cheat sheet.


### 8. Procedimiento
Parte 1: Base de datos sin volumen
Paso 1: Crear el contenedor server_db1
Ejecuta el siguiente comando en la terminal:
docker run --name server_db1 -e POSTGRES_USER=admin -e POSTGRES_PASSWORD=admin -p 5432:5432 -d postgres


Paso 2: Conectar un administrador de base de datos
Configura herramientas como DataGrip para conectarte al contenedor server_db1 usando:
- Host: localhost
- Puerto: 5432
- Usuario: admin
- Contraseña: admin

Paso 3: Crear la base de datos test
En la consola de PostgreSQL o desde el administrador, ejecuta:
CREATE DATABASE test;


Paso 4: Crear la tabla customer
Conéctate a la base de datos test:
\c test


Y luego crea la tabla:
CREATE TABLE customer (
    id SERIAL PRIMARY KEY,
    fullname VARCHAR(255) NOT NULL,
    status VARCHAR(50) NOT NULL
);


Paso 5: Insertar datos
Inserta un registro de prueba:
INSERT INTO customer (fullname, status) VALUES ('Juan Pérez', 'activo');


Paso 6: Validar datos insertados
Verifica que el registro se haya guardado:
SELECT * FROM customer;


Paso 7: Detener y eliminar el contenedor
Detén el contenedor:
docker stop server_db1


Y elimínalo:
docker rm server_db1


Paso 8: Volver a crear el contenedor
Repite el comando de creación, pero verifica que la base de datos test ya no existe, lo que demuestra que los datos no se han conservado.

Parte 2: Base de datos con volumen
Paso 1: Crear un volumen
docker volume create pgdata


Paso 2: Crear el contenedor server_db2
docker run --name server_db2 -e POSTGRES_USER=admin -e POSTGRES_PASSWORD=admin -p 5432:5432 -v pgdata:/var/lib/postgresql/data -d postgres


Paso 3: Crear la base de datos y tabla
Repite los pasos de la parte 1 para:
- Crear la base de datos test.
- Crear la tabla customer.
- Insertar y verificar datos.

Paso 4: Detener y eliminar el contenedor
docker stop server_db2
docker rm server_db2


Paso 5: Volver a crear el contenedor con el volumen
docker run --name server_db2 -e POSTGRES_USER=admin -e POSTGRES_PASSWORD=admin -p 5432:5432 -v pgdata:/var/lib/postgresql/data -d postgres


Paso 6: Verificar persistencia de datos
Confirma que la base de datos y los registros se mantuvieron:
SELECT * FROM customer;



9. Resultados esperados
- Base de datos sin volumen: Los datos no se mantienen después de eliminar el contenedor.
- Base de datos con volumen: Los datos persisten gracias al uso del volumen pgdata.

Capturas de pantalla incluidas en cada paso.

10. Bibliografía
Docker Inc. (n.d.). Docker Documentation. Recuperado de https://docs.docker.com
PostgreSQL Global Development Group. (n.d.). PostgreSQL Documentation. Recuperado de https://www.postgresql.org/docs/

Este es un informe detallado que cumple con la estructura solicitada. Puedes completarlo con capturas de pantalla y subirlo a Git. ¡Avísame si necesitas ayuda adicional! 🚀✨
