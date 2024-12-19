# **Informe de Ejercicios Docker**

---

## **Ejercicio 1: Trabajar con redes Docker**

### **1. Creación de redes Docker**
1. **Red1**  
   Crear una red con los siguientes parámetros:  
   ```bash
   docker network create \
     --driver bridge \
     --subnet=172.28.0.0/16 \
     --gateway=172.28.0.1 \
     red1
   ```

<p align="center">
  <img src="imagenes\RED1.png" alt="Conf">
</p>
2. **Red2**  
   Crear una red donde Docker asigne automáticamente los valores:

   ```bash
   docker network create --driver bridge red2
   ```

<p align="center">
  <img src="imagenes\RED2.png" alt="Conf">
</p>

---

### **2. Configuración de los contenedores**

1. **Contenedor u1**
   Crear el contenedor conectado a `red1`:

   ```bash
   docker run -dit \
     --name u1 \
     --hostname host1 \
     --network red1 \
     --ip 172.28.0.10 \
     ubuntu:20.04
   ```

<p align="center">
  <img src="imagenes\u1.png" alt="Conf">
</p>

   Instalar la herramienta `ping`:

   ```bash
   docker exec -it u1 bash
   apt update && apt install inetutils-ping -y
   apt update && apt install -y iproute2
   exit
   ```

2. **Contenedor u2**

   Crear el contenedor conectado a `red2`:

   ```bash
   docker run -dit \
     --name u2 \
     --hostname host2 \
     --network red2 \
     ubuntu:20.04
   ```

<p align="center">
  <img src="imagenes\u2.png" alt="Conf">
</p>

Instalar la herramienta `ping`:  

   ```bash
   docker exec -it u2 bash
   apt update && apt install inetutils-ping -y
   exit
   ```

---

### **3. Pruebas de conectividad**

1. **Ver configuración de red**:
   - Para `u1`:  
  
     ```bash
     docker exec u1 ip addr
     ```

   - Para `u2`:  

     ```bash
     docker exec u2 ip addr
     ```

   **Pantallazo 1 y 2**: Configuración de red de `u1` y `u2`.

2. **Probar conectividad inicial entre contenedores**:  
   - Desde `u1`:  

     ```bash
     docker exec u1 ping <IP_de_u2>
     docker exec u1 ping host2
     ```

   **Pantallazo 3**: Prueba de conectividad fallida.

3. **Conectar `u1` a la red2**:  

   ```bash
   docker network connect red2 u1
   ```

   - Desde `u1`, hacer ping a `u2`:
  
     ```bash
     docker exec u1 ping <IP_de_u2>
     docker exec u1 ping host2
     ```

   **Pantallazo 4**: Prueba de conectividad exitosa.

---

### **Pantallazos entregados**

1. Configuración de red del contenedor `u1`.

<p align="center">
  <img src="imagenes\u1_RED.png" alt="Conf">
</p>

1. Configuración de red del contenedor `u2`.

<p align="center">
  <img src="imagenes\u2_RED.png" alt="Conf">
</p>

1. Imposibilidad de hacer ping entre `u1` y `u2`.

<p align="center">
  <img src="imagenes\Imposibilidad_Conc.png" alt="Conf">
</p>

2. Conexión exitosa tras conectar `u1` a `red2`.
<p align="center">
  <img src="imagenes\RED_Puente.png" alt="Conf">
</p>

---

## **Ejercicio 2: Despliegue de Nextcloud + MariaDB**

### **1. Creación de red**

Crear una red bridge para conectar los contenedores:  

```bash
docker network create --driver=bridge nextcloud-network
```

---

### **2. Configuración de contenedor MariaDB**

1. Crear un volumen para persistencia:

   ```bash
   docker volume create mariadb_data
   ```

<p align="center">
  <img src="imagenes\RED_Nextcloud.png" alt="Conf">
</p>

2. Crear el contenedor MariaDB:
  
   ```bash
   docker run -dit \
     --name mariadb \
     --network nextcloud-network \
     -e MYSQL_ROOT_PASSWORD=root \
     -e MYSQL_DATABASE=nextcloud \
     -e MYSQL_USER=nextclouduser \
     -e MYSQL_PASSWORD=nextcloudpass \
     -v mariadb_data:/var/lib/mysql \
     mariadb:10.5
   ```

**Pantallazo 5**: Comando para crear el contenedor MariaDB.

<p align="center">
  <img src="imagenes\Mariadb.png" alt="Conf">
</p>

---

### **3. Configuración de contenedor Nextcloud**

1. Crear un volumen para persistencia:

   ```bash
   docker volume create nextcloud_data
   ```

2. Crear el contenedor Nextcloud:  

   ```bash
   docker run -dit \
     --name nextcloud \
     --network nextcloud-network \
     -e MYSQL_HOST=mariadb \
     -e MYSQL_DATABASE=nextcloud \
     -e MYSQL_USER=nextclouduser \
     -e MYSQL_PASSWORD=nextcloudpass \
     -v nextcloud_data:/var/www/html \
     -p 8080:80 \
     nextcloud
   ```

**Pantallazo 6**: Comando para crear el contenedor Nextcloud.

<p align="center">
  <img src="imagenes\SNextCloud.png" alt="Conf">
</p>

---

### **4. Acceso a la aplicación**

Abrir un navegador web y acceder a:  

```bash
http://<IP_LOCAL>:8080
```

**Pantallazo 7**: Acceso a la interfaz de Nextcloud en el navegador.

<p align="center">
  <img src="imagenes\INextCloud.png" alt="Conf">
</p>

---

### **Pantallazos entregados**

5. Comando para crear el contenedor MariaDB.

6. Comando para crear el contenedor Nextcloud.  

7. Acceso a la aplicación desde el navegador web.

---

## **Conclusión**

Los ejercicios resaltan la versatilidad de Docker para gestionar redes y desplegar aplicaciones con persistencia, demostrando su utilidad para crear entornos eficientes y escalables en desarrollo y producción.

---

## **Autor**

- Sergio Morato

---
