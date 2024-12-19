# **Informe: Creación de un Servidor Web y Base de Datos con Docker**

Este informe describe cómo configurar y gestionar dos contenedores Docker:  

1. Un servidor web basado en la imagen `php:7.4-apache` para servir archivos `HTML` y `PHP`.

2. Un servidor de base de datos utilizando la imagen oficial de `mariadb`.

---
Antes de comenzar nos aseguraremos de que nuestro entorno de trabajo este completamente limpio.

<p align="center">
  <img src="imagenes\Maquina_Limpia.png" alt="Conf">
</p>

## **1. Configuración del Servidor Web**

Se configuró un contenedor llamado `web` basado en la imagen oficial `php:7.4-apache`, accesible desde el puerto `8000`. En el directorio raíz `/var/www/html` del servicio web se colocaron dos archivos:

- `index.html` con un mensaje de bienvenida.

- `index.php` con un script que muestra la información del sistema PHP.

### **Pasos realizados:**

1. **Creación y arranque del contenedor**:

   ```bash
   docker run -d --name web -p 8000:80 php:7.4-apache
   ```

<p align="center">
  <img src="imagenes\Descarga_PHP.png" alt="Conf">
</p>

2. **Creación de los archivos en el contenedor**:
   Existen tres métodos para copiar los archivos en el contenedor. En este caso, se utilizó `docker cp`.

   - **Archivo `index.html`**:

     ```bash
     echo '<h1>HOLA SOY Sergio Morato</h1>' > index.html
     docker cp index.html web:/var/www/html/index.html
     ```

    <p align="center">
      <img src="imagenes\Creacion_HTML.png" alt="Conf">
    </p>

   - **Archivo `index.php`**:

     ```bash
     echo '<?php echo phpinfo(); ?>' > index.php
     docker cp index.php web:/var/www/html/index.php
     ```

    <p align="center">
      <img src="imagenes\Creacion_PHP.png" alt="Conf">
    </p>

3. **Verificación**:

   Se comprobó el acceso a los archivos desde un navegador web:
   - Archivo `index.html`: [http://<IP_LOCAL>:8000/index.html](http://<IP_LOCAL>:8080/index.html)  
   - Archivo `index.php`: [http://<IP_LOCAL>:8000/index.php](http://<IP_LOCAL>:8080/index.php)

#### **Pantallazos entregados**:

- **Pantallazo del navegador mostrando `index.html`**.

    <p align="center">
      <img src="imagenes\index_HTML.png" alt="Conf">
    </p>

- **Pantallazo del navegador mostrando `index.php`**.

    <p align="center">
      <img src="imagenes\INDEX_PHP.png" alt="Conf">
    </p>

1. **Tamaño del contenedor**

   Se verificó el tamaño del contenedor después de agregar los archivos con el comando:

   ```bash
   docker ps -s
   ```

#### **Pantallazo entregado**:
- **Pantallazo del tamaño del contenedor web tras añadir los archivos**.

    <p align="center">
      <img src="imagenes\Tamaño_1.png" alt="Conf">
    </p>

---

## **2. Configuración del Servidor de Base de Datos**

Se configuró un contenedor llamado `bbdd` utilizando la imagen oficial `mariadb`. Se especificaron las variables de entorno necesarias para configurar la base de datos automáticamente.

### **Pasos realizados:**

1. **Variables de entorno configuradas**:
   - Contraseña de root: `root`.
   - Base de datos creada automáticamente: `prueba`.
   - Usuario: `invitado`.
   - Contraseña: `invitado`.

2. **Creación y arranque del contenedor**:

   ```bash
   docker run -d --name bbdd -p 3336:3306 \
       -e MARIADB_ROOT_PASSWORD=root \
       -e MARIADB_DATABASE=prueba \
       -e MARIADB_USER=invitado \
       -e MARIADB_PASSWORD=invitado \
       mariadb
   ```

    <p align="center">
      <img src="imagenes\Descarga_MariaDB.png" alt="Conf">
    </p>

3. **Conexión desde un cliente de base de datos**:
   Desde el equipo anfitrión (donde está instalado Docker), se utilizó un cliente de base de datos para conectarse al servidor y verificar la creación de la base de datos `prueba` y el usuario `invitado`.

    ```bash
    mysql -u invitado -p -h 0.0.0.0 -P 3336
    ```

   - **Parámetros de conexión**:
     - **Host**: `<IP_LOCAL>`  
     - **Puerto**: `3336`  
     - **Usuario**: `invitado`  
     - **Contraseña**: `invitado`  

   - **Comandos SQL ejecutados**:
  
     ```sql
     SHOW DATABASES;
     ```

#### **Pantallazos entregados**:

- **Pantallazo del cliente de base de datos mostrando la conexión con el usuario invitado**.

    <p align="center">
      <img src="imagenes\Conexion_DB.png" alt="Conf">
    </p>

- **Pantallazo verificando la existencia de la base de datos `prueba`**.

    <p align="center">
      <img src="imagenes\DATABASES.png" alt="Conf">
    </p>

- **Intento de eliminar la imagen de `mariadb`**:
   Se intentó eliminar la imagen `mariadb` mientras el contenedor `bbdd` estaba creado, obteniendo un error de dependencia.

   - **Comando utilizado**:

     ```bash
     docker rmi mariadb
     ```

#### **Pantallazo entregado**:

- **Pantallazo del error al intentar eliminar la imagen `mariadb`**.

    <p align="center">
      <img src="imagenes\ERROR.png" alt="Conf">
    </p>

---

## **Conclusión**

El ejercicio permitió poner en práctica la creación y gestión de contenedores Docker para un servidor web y un servidor de base de datos. Se logró:
- Configurar y servir archivos HTML y PHP desde un contenedor web.

- Configurar un servidor de base de datos con variables de entorno personalizadas.
- Verificar que el sistema impide eliminar imágenes en uso.

---

## **Autores**
- Sergio Morato

---
