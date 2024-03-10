**Tabla de contenido**

- [Comandos relacionados a contenedores](#comandos-relacionados-a-contenedores)
- [Comandos de MySQL (Contenedor MySQL)](#comandos-de-mysql-contenedor-mysql)
  - [Scripts (Querys) MySQL](#scripts-querys-mysql)
- [Comandos de Postgres (Contenedor Postgres)](#comandos-de-postgres-contenedor-postgres)
- [Comandos de Ubuntu (Contenedor Ubuntu)](#comandos-de-ubuntu-contenedor-ubuntu)

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
