# **Informe: Creación y Uso de Volúmenes y Bind Mount en Docker**

Este informe detalla los pasos realizados para configurar y gestionar volúmenes Docker, así como el uso de bind mounts para compartir datos entre contenedores.

---

## **1. Creación y Uso de Volúmenes**

### **1.1 Creación de volúmenes**
Se crearon dos volúmenes llamados `volumen_datos` y `volumen_web` utilizando la siguiente orden:

```bash
docker volume create volumen_datos
docker volume create volumen_web
```

### **1.2 Arranque de contenedores con volúmenes**

1. **Arranque del contenedor `c1` (servidor web)**:
   Este contenedor utiliza la imagen `php:7.4-apache` y monta el volumen `volumen_web` en `/var/www/html`. Está configurado para ser accesible en el puerto `8080`.

   ```bash
   docker run -d --name c1 -p 8080:80 -v volumen_web:/var/www/html php:7.4-apache
   ```

2. **Arranque del contenedor `c2` (servidor de base de datos)**:
   Este contenedor utiliza la imagen `mariadb`, monta el volumen `volumen_datos` en `/var/lib/mysql` y configura la contraseña de root como `admin`.

   ```bash
   docker run -d --name c2 -e MARIADB_ROOT_PASSWORD=admin -v volumen_datos:/var/lib/mysql mariadb
   ```

### **1.3 Proceso para borrar `volumen_datos`**
1. Se detuvo y eliminó el contenedor `c2`:

   ```bash
   docker stop c2
   docker rm c2
   ```

2. Se procedió al borrado del volumen:
   ```bash
   docker volume rm volumen_datos
   ```

### **1.4 Creación y uso del archivo `index.html` en `c1`**
1. Se creó y copió el archivo `index.html` al contenedor `c1`:

   ```bash
   echo '<h1>HOLA SOY Sergio Morato</h1>' > index.html
   docker cp index.html c1:/var/www/html/index.html
   ```

2. Se verificó su visualización accediendo al contenedor a través del puerto `8080`.

<p align="center">
  <img src="imagenes\INDEX.png" alt="Conf">
</p>

1. Posteriormente, se eliminó el contenedor `c1` y se creó el contenedor `c3`, montando el mismo volumen `volumen_web` y configurándolo para servir en el puerto `8081`:
   ```bash
   docker stop c1
   docker rm c1
   docker run -d --name c3 -p 8081:80 -v volumen_web:/var/www/html php:7.4-apache
   ```

#### **Pantallazos entregados**:
- **Pantallazo de los volúmenes creados (`volumen_datos` y `volumen_web`)**.

<p align="center">
  <img src="imagenes\Volumenes1.png" alt="Conf">
</p>

- **Pantallazo del comando para arrancar `c1`**.

<p align="center">
  <img src="imagenes\Volumenes3.png" alt="Conf">
</p>

- **Pantallazo del comando para arrancar `c2`**.

<p align="center">
  <img src="imagenes\Volumenes3.png" alt="Conf">
</p>

- **Pantallazo del proceso para borrar `volumen_datos`**.

<p align="center">
  <img src="imagenes\Volumenes5.png" alt="Conf">
</p>

- **Pantallazo del borrado de `c1` y la creación de `c3`**.

<p align="center">
  <img src="imagenes\Volumenes6.png" alt="Conf">
</p>

- **Pantallazo del acceso al contenedor `c3` mostrando `index.html`.**

<p align="center">
  <img src="imagenes\Volumenes7.png" alt="Conf">
</p>

---

## **2. Bind Mount para Compartir Datos**

Se creó un directorio local llamado `saludo` con un archivo `index.html`, que fue compartido mediante bind mount en dos contenedores (`c1` y `c2`) basados en la imagen `php:7.4-apache`.

### **2.1 Creación del directorio y archivo local**

1. Se creó el directorio y el archivo `index.html`:

   ```bash
   mkdir saludo
   echo '<h1>HOLA SOY Sergio Morato</h1>' > saludo/index.html
   ```

### **2.2 Arranque de contenedores con bind mount**

1. **Arranque del contenedor `c1` (puerto 8181)**:

   ```bash
   docker run -d --name c1 -p 8181:80 -v $(pwd)/saludo:/var/www/html php:7.4-apache
   ```

2. **Arranque del contenedor `c2` (puerto 8282)**:

   ```bash
   docker run -d --name c2 -p 8282:80 -v $(pwd)/saludo:/var/www/html php:7.4-apache
   ```

### **2.3 Modificación del archivo `index.html`**

El archivo `index.html` se modificó sin necesidad de reiniciar los contenedores:

```bash
echo '<h1>HOLA, CONTENIDO MODIFICADO</h1>' > saludo/index.html
```

Se verificó que los cambios eran visibles accediendo a los contenedores `c1` (puerto 8181) y `c2` (puerto 8282).

#### **Pantallazos entregados**:

- **Pantallazo del comando para arrancar `c1` (puerto 8181)**.
  
- **Pantallazo del comando para arrancar `c2` (puerto 8282)**.
- **Pantallazo mostrando el contenido de `index.html` en `c1`**.
- **Pantallazo mostrando el contenido de `index.html` en `c2`**.
- **Pantallazo accediendo a los contenedores después de modificar `index.html`.**

---

## **Conclusión**

Esta actividad permitió comprender el uso y gestión de volúmenes en Docker, tanto mediante volúmenes estándar como bind mounts. Se verificó la persistencia de datos y el impacto de los volúmenes en la flexibilidad de los contenedores. Asimismo, se exploró cómo compartir directorios locales con múltiples contenedores y observar los cambios en tiempo real.

---

## **Autor**
- Sergio Morato

--- 

