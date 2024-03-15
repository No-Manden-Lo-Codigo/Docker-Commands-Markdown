**Tabla de contenido**

- [Comandos relacionados a contenedores](#comandos-relacionados-a-contenedores)
- [Comandos de MySQL (Contenedor MySQL)](#comandos-de-mysql-contenedor-mysql)
  - [Scripts (Querys) MySQL](#scripts-querys-mysql)
- [Comandos de Postgres (Contenedor Postgres)](#comandos-de-postgres-contenedor-postgres)
  - [Scripts (querys) Postgres](#scripts-querys-postgres)
- [Comandos de Ubuntu (Contenedor Ubuntu)](#comandos-de-ubuntu-contenedor-ubuntu)
  - [Logs](#logs)

---

### Comandos relacionados a contenedores

- Comando para crear un contenedor MySQL

```bash
docker run --name nombre_contenedor -e MYSQL_ROOT_PASSWORD=123 -d mysql
```

- Comando para crear un contenedor PostgreSQL

```bash
docker run --name nombre_contenedor -e POSTGRES_PASSWORD=tu_contrasena -p 5432:5432 -d postgres:nombre_version
```

Ejemplo:

```bash
docker run --name mi_postgres -e POSTGRES_PASSWORD=123 -p 5432:5432 -d postgres:latest
```

- Comando para crear un contenedor de ubuntu en docker

```bash
docker run --name nombre_contenedor -it ubuntu bash
```

- Abrir contenedor de docker en `wsl` / `Powershell` / `cmd`

```bash
docker exec -it NOMBRE_DEL_CONTENEDOR /bin/bash
```

- Salir del contenedor en wsl / Powershell / cmd

```bash
exit
```

---

### Comandos de MySQL (Contenedor MySQL)

- Comando para iniciar mysql (usuario root)

```bash
mysql -u root -p
```

#### Scripts (Querys) MySQL

- Crear roles en MySQL

```sql
CREATE ROLE my_role;
```

- Darle permisos a los roles

```sql
GRANT SELECT, INSERT, UPDATE ON database_name.* TO my_role;
```

- Quitar permisos

```sql
REVOKE SELECT ON DB_clase_S6.* FROM 'r_escritura'@'%';
```

- Crear usuario en MySQL

Si no se indica el ip o el puerto, se crea el usuario con permisos de conectarse desde cualquier ip y puerto, `%`.

Esto es util para conectar un usuario desde un contenedor que no sea el de MySQL.

```sql
CREATE USER 'newuser' IDENTIFIED BY 'password';
```

```sql
CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
```

```sql
CREATE USER 'newuser'@'3306' IDENTIFIED BY 'password';
```

- Darle roles al usuario en MySQL

```sql
GRANT 'TestRole_ReadOnly' TO 'newuser';
```

```sql
GRANT 'TestRole_ReadOnly' TO 'newuser'@'localhost';
```

```sql
GRANT 'TestRole_ReadOnly' TO 'newuser'@'3306';
```

- Ver los usuarios en MySQL

```sql
SELECT user, host FROM mysql.user;
```

- Darle uso de todos los servidores y bases de datos al rol especificado.

```sql
GRANT USAGE ON *.* TO 'u_lectura'@'3306';
```

- Darle todos los privilegios a un usuario en una sola base de datos.

```sql
GRANT ALL PRIVILEGES ON database_name.* TO 'username';
```

```sql
GRANT ALL PRIVILEGES ON database_name.* TO 'username'@'localhost';
```

- Darle todos los privilegios a un usuario en todas las bases de datos.

```sql
GRANT ALL PRIVILEGES ON *.* TO 'username';
```

- Verificar privilegios

```sql
SHOW GRANTS FOR 'nombre_usuario'@'host(puerto)';
```

- Alterar y cambiar host de un usuario (Alter and change host for user).

```sql
rename user 'USERNAME'@'HOST' to 'USERNAME'@'HOST';
```

- Eliminar host (To erase host).

```sql
rename user 'USERNAME'@'HOST' to 'USERNAME'
```

---

### Comandos de Postgres (Contenedor Postgres)

- Comando para iniciar PostgreSQL

```bash
psql -h localhost -p 5432 -U tu_usuario -d tu_base_de_datos
```

Ejemplo: iniciando PostgreSQL con el usuario predeterminado, en la base de datos predeterminada. La primera vez que iniciamos en PostgreSQL debe ser de esta forma, ya que no hay más usuarios creados.

```bash
psql -h localhost -p 5432 -U postgres -d postgres
```

---

#### Scripts (querys) Postgres

```postgres
\c nombreBaseDatos
```

- esto cambia a la base de datos sin tener que salir y conectarse de nuevo al usuario.

```postgres
\dt
```

- Lista las tablas de una base de datos

```sql
CREATE ROLE nombreRol;
```

- Crea un rol en Postgres.

```sql
CREATE USER nombreUsuario WITH PASSWORD 'contrasenia';
```

- Crea un usuario en Postgres.

```sql
GRANT nombreRol TO nombreUsuario;
```

- Le asigna un rol a un usuario.

```sql
GRANT SELECT, INSERT, UPDATE, DELETE, CREATE ON ALL TABLES IN SCHEMA public to nombreRol;

GRANT SELECT, INSERT, UPDATE, DELETE, CREATE ON ALL TABLES IN SCHEMA public to nombreUsuario;
```

- Le da permisos a un rol o a un usuario.

Algunos ejemplos en postgres:

```sql
create table Productos(
id_producto int primary key,
nombre varchar(150),
descripcion varchar(255),
precio DECIMAL(10, 2)
);

create table cliente(
id_cliente int primary key,
nombre varchar(150),
telefono varchar(20),
correo varchar(150)
);

INSERT INTO cliente (id_cliente, nombre, telefono, correo)
VALUES
    (1, 'Ana López', '555-1234', 'ana.lopez@example.com'),
    (2, 'Juan Martínez', '555-5678', 'juan.martinez@example.com'),
    (3, 'María Rodríguez', '555-9876', 'maria.rodriguez@example.com'),
    (4, 'Carlos García', '555-4321', 'carlos.garcia@example.com'),
    (5, 'Laura Pérez', '555-8765', 'laura.perez@example.com');


INSERT INTO cliente (id_cliente, nombre, telefono, correo) VALUES (1, 'Ana López', '555-1234', 'ana.lopez@example.com');

INSERT INTO cliente (id_cliente, nombre, telefono, correo) VALUES (2, 'Juan Martínez', '555-5678', 'juan.martinez@example.com');

INSERT INTO cliente (id_cliente, nombre, telefono, correo) VALUES (3, 'María Rodríguez', '555-9876', 'maria.rodriguez@example.com');

INSERT INTO cliente (id_cliente, nombre, telefono, correo) VALUES (4, 'Carlos García', '555-4321', 'carlos.garcia@example.com');

INSERT INTO cliente (id_cliente, nombre, telefono, correo) VALUES (5, 'Laura Pérez', '555-8765', 'laura.perez@example.com');



INSERT INTO cliente (id_cliente, nombre, telefono, correo) VALUES (6, 'Laura Pérez', '555-8765', 'laura.perez@example.com'); 



GRANT INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO rolewrite;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO roleread;

```

Nota: estos ejemplos ya fueron utilizados.

---

### Comandos de Ubuntu (Contenedor Ubuntu)

- Instalar MySQL en Ubuntu

```bash
apt-get install mysql-client
```

- Conectar MySQL desde Ubuntu

Usando el puerto:

```bash
mysql -h 172.17.0.2 -p 3306 -u root -p
```

Usando solo el IP:

```bash
mysql -h 172.17.0.2 -u root -p
```

- Comando para instalar PostgreSQL en Ubuntu

```bash
apt-get install -y postgresql
```

- Instalar el comando ping en ubuntu

```bash
apt-get install -y inetutils-ping
```

- Instalar el comando telnet en ubuntu.

```bash
apt-get install -y telnet
```

- Utilizar el comando telnet.

`telnet [hostname or IP address] [port]`

Ejemplos:

```bash
telnet 192.168.1.1 80
```

```bash
telnet 172.17.0.2 3306
```

```bash
apt install -y openssh-client
```

- Instala ssh en el contenedor.

```bash
ssh clay@64.23.217.34
```

- Conecta a un servidor remoto por ssh. Para este ejemplo la clave era `1234`.

```bash
adduser nombreUsuario
```

- Crea un nuevo usuario en Ubuntu.

```bash
usermod -aG sudo nombreUsuario
```

- Darle privilegios de administrador a un usuario en Ubuntu.

```bash
su nombreUsuario
```

- su (switch user) cambia de usuario en Ubuntu.

```bash
su - nombreUsuario
```

- Cambia de usuario, pero cambia las variables de entorno entre otras cosas.

Diferencias entre `su` y `su -`

`su usuario`: Esta forma de usar el comando su cambia al usuario especificado, pero no cambia completamente el entorno del shell. Esto significa que algunas variables de entorno y configuraciones del shell pueden permanecer como estaban antes de ejecutar el comando su. Por ejemplo, el directorio de inicio (HOME) y la variable de entorno PATH pueden seguir siendo las del usuario original. Esto se conoce como un cambio de usuario "suave".

`su - usuario`: En cambio, su - usuario cambia al usuario especificado y también inicializa el entorno del shell como si el usuario especificado hubiera iniciado sesión de manera normal. Esto incluye establecer el directorio de inicio (HOME) en el del usuario especificado y cargar las configuraciones de inicio del shell, como los archivos .bashrc, .bash_profile, etc. Esto se conoce como un cambio de usuario "duro".

`su usuario` realiza un cambio de usuario sin cambiar completamente el entorno del shell, mientras que `su - usuario` realiza un cambio de usuario y establece completamente el entorno del shell como si el usuario especificado hubiera iniciado sesión normalmente.

```bash
deluser nombreUsuario
```

- Borrar un usuario.

```bash
deluser --remove-home nombreUsuario
```

- Esto elimina el usuario y su directorio de inicio, junto con todos los archivos y directorios asociados.

---

#### Logs

```bash
docker logs nombreContenedor
```

- Este comando devuelve el log que tiene el contenedor.

```bash
docker logs PostgresCont1 > postgreslog.txt
docker logs MySQLCont1 > mysqllog.txt
docker logs UbuntuCont1 > ubuntulog.txt
```

- Este comando redirige el contenido del log a un archivo txt.
