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
---
![image](https://github.com/user-attachments/assets/d5b471db-8963-49c8-9b8f-0c372aabcc76)
---
Paso 1: Crear el contenedor server_db1
---
![image](https://github.com/user-attachments/assets/2436cdd4-78fb-44b8-b369-dcb0bf64c120)
---
Ejecuta el siguiente comando en la terminal:
docker run --name server_db1 -e POSTGRES_USER=admin -e POSTGRES_PASSWORD=admin -p 5432:5432 -d postgres
---
![image](https://github.com/user-attachments/assets/ee0a06b1-8f1c-4474-9b13-d402f5c3ebd4)
---

Paso 2: Conectar un administrador de base de datos
Configura herramientas como Powershell para conectarte al contenedor server_db1 usando:
- Host: localhost
- Puerto: 5432
- Usuario: admin
- Contraseña: admin
---
![image](https://github.com/user-attachments/assets/11846e82-807e-4063-8395-4516d607e0ad)
---
Paso 3: Crear la base de datos test
En la consola de PostgreSQL o desde el administrador, ejecuta:
CREATE DATABASE test;
---
![image](https://github.com/user-attachments/assets/1bbe622b-8c85-405f-be99-86626adcc75b)
---
Paso 4: Crear la tabla customer
Conéctate a la base de datos test:
\c test
---
![image](https://github.com/user-attachments/assets/8799a0a1-6e2f-4ac2-accd-f69016c8613a)
---
Y luego crea la tabla:
CREATE TABLE customer (
    id SERIAL PRIMARY KEY,
    fullname VARCHAR(255) NOT NULL,
    status VARCHAR(50) NOT NULL
);
---
![image](https://github.com/user-attachments/assets/770456f7-e794-423f-a5f0-c34d499d9bea)
---
Paso 5: Insertar datos
Inserta un registro de prueba:
INSERT INTO customer (fullname, status) VALUES ('Fausto Saquinaula', 'activo');
---
![image](https://github.com/user-attachments/assets/96de1133-70d6-4df0-af5a-02f8546ccc91)
---
Paso 6: Validar datos insertados
Verifica que el registro se haya guardado:
SELECT * FROM customer;
---
![image](https://github.com/user-attachments/assets/c363c816-0a28-4998-a811-dcab9dc8816f)
---
Paso 7: Detener y eliminar el contenedor
Detén el contenedor:
docker stop server_db1
---
![image](https://github.com/user-attachments/assets/28f51ed5-ef1f-4af4-aadc-35a9e09c3bfd)
---
Y elimínalo:
docker rm server_db1
---
![image](https://github.com/user-attachments/assets/d7e7b278-45a7-45ae-9d3e-96cd007ba050)
![image](https://github.com/user-attachments/assets/40e64d48-c619-4105-8cc6-a8edc1b46921)
---
Paso 8: Volver a crear el contenedor
Repite el comando de creación, pero verifica que la base de datos test ya no existe, lo que demuestra que los datos no se han conservado.
---
![image](https://github.com/user-attachments/assets/a7db26cb-e791-46a4-b95e-ad92216236d6)
---
Parte 2: Base de datos con volumen
Paso 1: Crear un volumen
docker volume create pgdata
---
![image](https://github.com/user-attachments/assets/c8de2412-7270-40d1-bd1b-4242d3cd3ae6)
---
Paso 2: Crear el contenedor server_db2
docker run --name server_db2 -e POSTGRES_USER=admin -e POSTGRES_PASSWORD=admin -p 5432:5432 -v pgdata:/var/lib/postgresql/data -d postgres
---
![image](https://github.com/user-attachments/assets/5dfc2c61-26d5-4574-9086-62498f902d4b)
---

Paso 3: Crear la base de datos y tabla
Repite los pasos de la parte 1 para:
- Crear la base de datos test.
---
![image](https://github.com/user-attachments/assets/75f2d643-eb9f-43de-98df-0123f20895b1)
---
- Crear la tabla customer.
---
![image](https://github.com/user-attachments/assets/6491fac2-d9f7-471b-82f5-2241e9a4365a)
---
- Insertar y verificar datos.
---
![image](https://github.com/user-attachments/assets/184d3255-15b2-4674-9bdd-f43a7b9f6af7)
![image](https://github.com/user-attachments/assets/1b8c1b8d-2be2-4b24-8ad2-86a721a9b0f1)
---
Paso 4: Detener y eliminar el contenedor
docker stop server_db2
docker rm server_db2
---
![image](https://github.com/user-attachments/assets/43af2b4d-d780-4200-8a84-63f65451b033)
![image](https://github.com/user-attachments/assets/1d3b2e33-bf9d-495b-8986-316ac18b1ea2)
![image](https://github.com/user-attachments/assets/bbedfc6e-154d-475c-9fd2-3802d6d8c3bc)
---
Paso 5: Volver a crear el contenedor con el volumen
docker run --name server_db2 -e POSTGRES_USER=admin -e POSTGRES_PASSWORD=admin -p 5432:5432 -v pgdata:/var/lib/postgresql/data -d postgres
---
![image](https://github.com/user-attachments/assets/0f13c825-2860-4341-975a-6d72a80355dd)
![image](https://github.com/user-attachments/assets/3491a583-c49f-4b90-a9f5-2a21063302e9)
---

Paso 6: Verificar persistencia de datos
Confirma que la base de datos y los registros se mantuvieron:
SELECT * FROM customer;
---
![image](https://github.com/user-attachments/assets/87fe60f0-e16c-40fa-b504-7c00b9c359ed)
---
9. Resultados esperados
- Base de datos sin volumen: Los datos no se mantienen después de eliminar el contenedor.
- Base de datos con volumen: Los datos persisten gracias al uso del volumen pgdata.
---
![image](https://github.com/user-attachments/assets/ae4acb15-791e-4274-b2a4-b95b5b2937ec)
---
10. Bibliografía
Docker Inc. (n.d.). Docker Documentation. Recuperado de https://docs.docker.com
PostgreSQL Global Development Group. (n.d.). PostgreSQL Documentation. Recuperado de https://www.postgresql.org/docs/

