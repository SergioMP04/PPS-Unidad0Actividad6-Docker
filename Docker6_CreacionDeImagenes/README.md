### **Informe de Actividades: Creación de Imágenes y Contenedores Docker**

---

## **Actividad 1: Creación de una Imagen a partir de un Contenedor**

### **1. Objetivo**

Crear una imagen personalizada a partir de un contenedor de una imagen base, instalar herramientas de red y subirla a Docker Hub.

---

### **2. Pasos Realizados**

#### **Paso 1: Arrancar un Contenedor**

1. Utilizamos la imagen base `ubuntu` para arrancar un contenedor en modo interactivo.

   ```bash
   docker run -it --name contenedor_red ubuntu bash
   ```

   **Pantallazo**: Creación del contenedor.

<p align="center">
  <img src="imagenes\CContenedor.png" alt="Conf">
</p>
---

#### **Paso 2: Instalar Paquetes de Herramientas de Red**

2. Dentro del contenedor, instalamos los paquetes `inetutils-ping`, `iproute2` y `dnsutils` con `apt`:

   ```bash
   apt update
   apt install -y inetutils-ping iproute2 dnsutils
   ```

   **Verificación**: Ejecutar comandos como `ping`, `ip a`, y `nslookup` para confirmar su instalación.

---

#### **Paso 3: Crear una Imagen a partir del Contenedor**
3. Salimos del contenedor y creamos una imagen utilizando `docker commit`:
   ```bash
   docker commit contenedor_red <tu_usuario_docker_hub>/comandos_redes
   ```

   **Pantallazo**: Comando utilizado para crear la imagen.
<p align="center">
  <img src="imagenes\DockerCommit.png" alt="Conf">
</p>
---

#### **Paso 4: Subir la Imagen a Docker Hub**
4. Autenticarse en Docker Hub si no lo has hecho:

   ```bash
   docker login
   ```

5. Subir la imagen al registro:

   ```bash
   docker push sergiomoratovalle/comandos_redes
   ```

   **Pantallazo**: Imagen subida a Docker Hub.
<p align="center">
  <img src="imagenes\Login.png" alt="Conf">
   <img src="imagenes\DockerPush.png" alt="Conf">
</p>
---

#### **Paso 5: Descargar y Probar la Imagen**

6. En otro ordenador (o borrando la imagen localmente), descargamos la imagen desde Docker Hub:

   ```bash
   docker rm contenedor_red
   docker pull <tu_usuario_docker_hub>/comandos_redes
   ```

7. Creamos un contenedor a partir de la imagen descargada:

   ```bash
   docker run -it --name nuevo_contenedor_red sergiomoratovalle/comandos_redes bash
   ```

   **Pantallazo**: Descarga de la imagen y creación del nuevo contenedor.

<p align="center">
  <img src="imagenes\DockerPull2.png" alt="Conf">
</p>

---

## **Actividad 2: Creación de una Imagen a partir de un Dockerfile**

### **1. Objetivo**
Crear una imagen con un servidor web que sirva una página estática utilizando un `Dockerfile` y subirla a Docker Hub.

---

### **2. Pasos Realizados**

#### **Paso 1: Crear el Archivo `index.html`**
1. Creamos una página web estática:
   ```html
   <!DOCTYPE html>
   <html lang="es">
   <head>
       <meta charset="UTF-8">
       <title>Servidor Web Docker</title>
   </head>
   <body>
       <h1>¡Hola Mundo desde Docker!</h1>
   </body>
   </html>
   ```
   **Guardar como**: `index.html`.

---

#### **Paso 2: Crear el Dockerfile**
2. Creamos un archivo llamado `Dockerfile` en el mismo directorio:
   ```dockerfile
   # Utiliza una imagen base de Debian
   FROM debian:latest

   # Instala el servidor web Apache
   RUN apt update && apt install -y apache2

   # Copia el archivo index.html en el directorio raíz del servidor web
   COPY index.html /var/www/html/index.html

   # Expone el puerto 80
   EXPOSE 80

   # Comando para iniciar el servidor Apache
   CMD ["apachectl", "-D", "FOREGROUND"]
   ```
   **Pantallazo**: Contenido del archivo `Dockerfile`.

---

#### **Paso 3: Crear la Imagen Docker**
3. Creamos la imagen utilizando el siguiente comando:
   ```bash
   docker build -t <tu_usuario_docker_hub>/mi_servidor_web .
   ```
   **Pantallazo**: Comando que crea la imagen.

---

#### **Paso 4: Subir la Imagen a Docker Hub**
4. Subimos la imagen al registro:
   ```bash
   docker push <tu_usuario_docker_hub>/mi_servidor_web
   ```
   **Pantallazo**: Imagen subida a Docker Hub.

---

#### **Paso 5: Descargar y Probar la Imagen**
5. En otro ordenador (o borrando la imagen localmente), descargamos la imagen:
   ```bash
   docker pull <tu_usuario_docker_hub>/mi_servidor_web
   ```
6. Creamos un contenedor y probamos el servidor web:
   ```bash
   docker run -d -p 8080:80 --name servidor_web <tu_usuario_docker_hub>/mi_servidor_web
   ```
7. Accedemos desde el navegador a `http://localhost:8080` (o usando tu IP local).

   **Pantallazo**: Página servida correctamente desde el contenedor.

---

## **Entrega Final**
Se deberán entregar **los siguientes pantallazos**coma

### **Actividad 1: Creación de Imagen desde un Contenedor**
1. Creación del contenedor.
2. Comando que crea la nueva imagen.
3. Imagen subida a Docker Hub.
4. Descarga de la imagen y creación del contenedor en otro equipo.

### **Actividad 2: Creación de Imagen con Dockerfile**
1. Contenido del archivo `Dockerfile`.
2. Comando que crea la imagen.
3. Imagen subida a Docker Hub.
4. Descarga de la imagen y ejecución del servidor web.

---

### **Conclusión**
En este informe se ha demostrado la capacidad de crear imágenes personalizadas Docker a partir de contenedores y utilizando un Dockerfile. Se realizaron pruebas de funcionamiento en local y se integraron con Docker Hub para compartir las imágenes.