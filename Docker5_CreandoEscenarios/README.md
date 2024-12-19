## Informe: Despliegue de Prestashop y Nextcloud utilizando Docker-Compose

---

### **1. Despliegue de Prestashop**

#### **Objetivo**  
Desplegar una tienda virtual utilizando Prestashop y configurando una base de datos con las especificaciones indicadas.

---

#### **Paso a Paso**

1. **Descargar el archivo `docker-compose.yml` de Bitnami**  
   - Accede a la URL oficial de Bitnami Prestashop en Docker Hub:  
     [https://hub.docker.com/r/bitnami/prestashop/](https://hub.docker.com/r/bitnami/prestashop/)
   - Descarga el archivo `docker-compose.yml` usando:
     ```bash
     curl -O https://raw.githubusercontent.com/bitnami/containers/main/bitnami/prestashop/docker-compose.yml
     ```

2. **Modificar el archivo `docker-compose.yml`**  
   - Abre el archivo con un editor de texto como `nano` o `vim`:
     ```bash
     nano docker-compose.yml
     ```
   - Modifica las siguientes variables de entorno en ambos servicios (`prestashop` y `mariadb`):

     **Base de datos y usuario:**  
     ```yaml
     - MARIADB_DATABASE=mitienda
     - MARIADB_USER=pepe
     - MARIADB_PASSWORD=pepe
     ```

     **Host para acceder desde una máquina virtual (si aplica):**  
     ```yaml
     - PRESTASHOP_HOST=<IP_DE_LA_MAQUINA>
     ```

3. **Levantar el escenario con Docker Compose**  
   - Guarda el archivo modificado y ejecuta:
     ```bash
     docker-compose up -d
     ```

4. **Verificar que los contenedores están funcionando**  
   - Comprueba el estado de los contenedores:
     ```bash
     docker-compose ps
     ```

5. **Acceder a Prestashop desde el navegador**  
   - Abre un navegador e introduce la dirección IP y puerto donde se ejecuta Prestashop:  
     ```http
     http://<IP_LOCAL>:8080
     ```

6. **Repetir el ejercicio si es necesario**  
   - Para eliminar volúmenes y reiniciar el escenario, ejecuta:
     ```bash
     docker-compose down -v
     ```

---

#### **Pantallazos a entregar**  
1. Pantallazo del archivo `docker-compose.yml` modificado.  
2. Pantallazo de los contenedores funcionando (`docker-compose ps`).  
3. Pantallazo del acceso desde el navegador a la tienda virtual Prestashop.

---

### **2. Despliegue de Nextcloud**

#### **Objetivo**  
Desplegar la aplicación Nextcloud junto con una base de datos (MariaDB o PostgreSQL) utilizando Docker Compose.

---

#### **Paso a Paso**

1. **Instalar Docker Compose**  
   - Si no está instalado, descárgalo con el siguiente comando:
     ```bash
     sudo apt update
     sudo apt install -y docker-compose
     ```

2. **Crear el archivo `docker-compose.yml`**  
   - Dentro de un directorio, crea un archivo con el siguiente contenido:
     ```yaml
     version: '3.8'

     services:
       db:
         image: mariadb:10.5
         container_name: nextcloud_db
         restart: always
         environment:
           MARIADB_ROOT_PASSWORD: rootpassword
           MARIADB_DATABASE: nextcloud
           MARIADB_USER: nextclouduser
           MARIADB_PASSWORD: nextcloudpassword
         volumes:
           - db_data:/var/lib/mysql
         networks:
           - nextcloud_network

       app:
         image: nextcloud
         container_name: nextcloud_app
         restart: always
         ports:
           - "8080:80"
         environment:
           NEXTCLOUD_ADMIN_USER: admin
           NEXTCLOUD_ADMIN_PASSWORD: adminpassword
           NEXTCLOUD_DATABASE_HOST: db
           NEXTCLOUD_DATABASE_USER: nextclouduser
           NEXTCLOUD_DATABASE_PASSWORD: nextcloudpassword
           NEXTCLOUD_DATABASE_NAME: nextcloud
         volumes:
           - nextcloud_data:/var/www/html
         depends_on:
           - db
         networks:
           - nextcloud_network

     volumes:
       db_data:
       nextcloud_data:

     networks:
       nextcloud_network:
         driver: bridge
     ```

3. **Levantar el escenario con Docker Compose**  
   - Guarda el archivo y ejecuta:
     ```bash
     docker-compose up -d
     ```

4. **Verificar los contenedores**  
   - Comprueba los contenedores activos:
     ```bash
     docker-compose ps
     ```

5. **Acceder a la aplicación Nextcloud**  
   - Abre un navegador y accede a la aplicación:  
     ```http
     http://<IP_LOCAL>:8080
     ```

6. **Comprobar almacenamiento y red**  
   - Verifica los volúmenes creados:
     ```bash
     docker volume ls
     ```
   - Comprueba la red bridge creada:
     ```bash
     docker network ls
     ```

7. **Eliminar el escenario**  
   - Para eliminar los contenedores y volúmenes:
     ```bash
     docker-compose down -v
     ```

---

#### **Pantallazos a entregar**  
1. Pantallazo del archivo `docker-compose.yml`.  
2. Pantallazo de los contenedores funcionando (`docker-compose ps`).  
3. Pantallazo del acceso a la aplicación Nextcloud desde el navegador.

---

### **Conclusión**  
Se han desplegado correctamente las aplicaciones Prestashop y Nextcloud utilizando Docker Compose, configurando las variables de entorno necesarias y garantizando la persistencia de datos. El proceso permite automatizar el despliegue, facilitando la gestión de aplicaciones complejas con múltiples servicios.