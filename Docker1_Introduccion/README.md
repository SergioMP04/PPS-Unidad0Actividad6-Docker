# **Informe: Creación y Gestión de un Contenedor Nginx en Docker**

## **Introducción**

Este informe describe el procedimiento para crear y gestionar un contenedor Docker basado en la imagen oficial de nginx. Además, se realizan las siguientes tareas:

- Asignar un nombre al contenedor.

- Configurar el mapeo de puertos para acceder al servidor web desde el host.
- Verificar el acceso al servidor web mediante un navegador.
- Listar las imágenes locales disponibles.
- Detener y eliminar el contenedor.

---

## **Procedimiento**

### **1. Creación del contenedor**

Se creó un contenedor llamado `servidor_web` en modo demonio utilizando la imagen oficial de nginx, con el puerto `8181` del host mapeado al puerto `80` del contenedor.

#### **Comando utilizado**

```bash
docker run -d --name servidor_web -p 8181:80 nginx
```

<p align="center">
  <img src="imagenes\Descarga_NGINX.png" alt="Conf">
</p>

#### **Comprobación de la ejecución del contenedor**

Para verificar que el contenedor está en ejecución, se utilizó el comando:

```bash
docker ps
```

<p align="center">
  <img src="imagenes\Creacion_Contenedor.png" alt="Conf">
</p>

#### **Resultado**

Se confirma que el contenedor `servidor_web` está corriendo correctamente y tiene el puerto `8181` mapeado.

---

### **2. Acceso al servidor web**

Para confirmar que el servidor nginx está funcionando correctamente:
1. Se accedió a la dirección `http://0.0.0.0:8181` desde un navegador web (donde `<IP_LOCAL>` es la dirección IP de la máquina host).
2. La página de inicio de nginx ("Welcome to nginx!") se mostró correctamente.

#### **Pantallazo del navegador**

<p align="center">
  <img src="imagenes\Contenedor_Firefox.png" alt="Conf">
</p>

---

### **3. Visualización de imágenes locales**

Se listaron las imágenes disponibles en el registro local de Docker con el comando:

```bash
docker images
```

#### **Resultado**

La imagen oficial de nginx aparece entre las disponibles.

<p align="center">
  <img src="imagenes\Docker_Imagenes.png" alt="Conf">
</p>

---

### **4. Eliminación del contenedor**

Para detener y eliminar el contenedor, se ejecutaron los siguientes comandos:

#### **1. Detener el contenedor**

```bash
docker stop servidor_web
```

#### **2. Eliminar el contenedor**

```bash
docker rm servidor_web
```

- En caso de que el contenedor se encuentre corriendo habilitaremos la opción.

```bash
docker rm -f servidor_web
```

#### **Comprobación**

El contenedor fue eliminado exitosamente y ya no aparece en la lista de contenedores.

<p align="center">
  <img src="imagenes\Docker_Rm.png" alt="Conf">
</p>

---

## **Autor**

- Sergio Morato

---
