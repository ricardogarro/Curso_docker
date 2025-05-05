# Curso de Docker – Capítulo 1: Introducción a Docker

## 🎯 Objetivos del capítulo

- Entender qué es Docker y por qué es útil
- Diferenciar entre contenedores y máquinas virtuales
- Instalar Docker en tu sistema operativo
- Ejecutar tu primer contenedor

---

## 🐳 ¿Qué es Docker?

Docker es una plataforma que permite crear, ejecutar y gestionar **contenedores**. Un contenedor es un entorno aislado que incluye todo lo necesario para ejecutar una aplicación: código, dependencias, sistema de archivos, etc.

> ✅ **Ventajas clave:**
> - Portabilidad: corre igual en cualquier sistema
> - Ligereza: consume menos recursos que una VM
> - Aislamiento: cada app vive en su propio espacio
> - Reproducibilidad: entornos de desarrollo idénticos

---

## 🖥️ Contenedores vs Máquinas Virtuales

| Característica         | Contenedores       | Máquinas Virtuales |
|------------------------|--------------------|---------------------|
| Uso de recursos        | Bajo               | Alto                |
| Tiempo de inicio       | Milisegundos       | Minutos             |
| Sistema operativo base | Comparte el host   | OS completo         |
| Tamaño                 | Ligero (MB)        | Pesado (GB)         |
| Ideal para             | Microservicios     | Aplicaciones monolíticas o legacy |

---

## 🛠️ Instalación de Docker

### 🔵 Windows o macOS

- Ir a: [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)
- Descargar e instalar Docker Desktop
- Requiere cuenta gratuita de Docker

### 🟢 Linux (ej: Ubuntu)

```bash
sudo apt update
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker $USER
```

Luego cerrar sesión y volver a entrar.

---

## ▶️ Tu primer contenedor

Ejecutá este comando para correr un contenedor que imprime "Hello from Docker":

```bash
docker run hello-world
```

> Esto descarga una imagen de prueba y muestra un mensaje confirmando que Docker funciona correctamente.

---

## 📦 Conceptos clave

- **Imagen:** plantilla de solo lectura para crear contenedores (como un "snapshot" de una app).
- **Contenedor:** instancia en ejecución de una imagen.
- **Docker Hub:** repositorio público de imágenes (como GitHub pero para Docker).

---

## 📚 Tareas sugeridas

1. Instalar Docker en tu equipo.
2. Ejecutar el contenedor `hello-world`.
3. Buscar una imagen en [https://hub.docker.com](https://hub.docker.com), por ejemplo: `nginx`, `mysql`, `php`.



# Curso de Docker – Capítulo 2: Imágenes y Contenedores

## 🎯 Objetivos del capítulo

- Aprender a usar los comandos básicos de Docker (`run`, `ps`, `stop`, `rm`)
- Diferenciar entre imagen y contenedor
- Crear tu propia imagen con un `Dockerfile`

---

## 📦 ¿Qué es una imagen?

Una **imagen** de Docker es una plantilla inmutable que contiene todo lo necesario para ejecutar una aplicación: sistema de archivos, dependencias, librerías, código fuente y configuraciones.

> 📘 Se puede pensar en una imagen como el blueprint para crear contenedores.

---

## 📦 ¿Qué es un contenedor?

Un **contenedor** es una instancia **en ejecución** de una imagen. Es aislado, reproducible y puede ser creado, pausado o eliminado fácilmente.

> 🔁 Podés tener múltiples contenedores corriendo desde la misma imagen.

---

## 🧪 Comandos esenciales

### ▶️ `docker run`

```bash
docker run hello-world
```

- Descarga la imagen (si no está) y crea un contenedor.
- `--rm`: elimina el contenedor al finalizar.
- `-it`: modo interactivo con consola.
- `--name`: asigna un nombre personalizado al contenedor.
- `-p`: expone puertos (host:contenedor).

**Ejemplo:**
```bash
docker run --name mi-nginx -p 8080:80 -d nginx
```

---

### 📋 `docker ps`

```bash
docker ps
```

- Muestra los contenedores activos.

```bash
docker ps -a
```

- Muestra todos los contenedores (incluyendo detenidos).

---

### ⏹️ `docker stop` y `docker rm`

```bash
docker stop mi-nginx     # Detiene el contenedor
docker rm mi-nginx       # Elimina el contenedor
```

También podés eliminar todos los detenidos:

```bash
docker container prune
```

---

### 🔍 `docker images`

```bash
docker images
```

- Lista las imágenes descargadas localmente.

---

## 🛠️ Creando tu propia imagen con un Dockerfile

1. **Crear archivo `Dockerfile`:**

```Dockerfile
# Usa una imagen base oficial
FROM php:8.2-cli

# Copia los archivos del proyecto
COPY . /app

# Establece el directorio de trabajo
WORKDIR /app

# Comando por defecto
CMD ["php", "index.php"]
```

2. **Crear archivo de ejemplo `index.php`:**

```php
<?php
echo "Hola desde mi contenedor PHP!";
```

3. **Construir la imagen:**

```bash
docker build -t mi-app-php .
```

4. **Ejecutar el contenedor:**

```bash
docker run mi-app-php
```

---

## 📚 Tareas sugeridas

1. Ejecutá un contenedor interactivo con Ubuntu:

```bash
docker run -it ubuntu bash
```

2. Creá una imagen personalizada usando `Dockerfile`.
3. Usá `docker ps`, `stop`, `rm` y `images` para explorar el ciclo de vida de tus contenedores.

---

# Curso de Docker – Capítulo 3: Volúmenes y Persistencia de Datos

## 🎯 Objetivos del capítulo

- Comprender por qué los datos de los contenedores no persisten por defecto
- Aprender a usar volúmenes de Docker
- Montar volúmenes desde el host
- Aplicar buenas prácticas para contenedores con base de datos

---

## 💾 ¿Qué pasa con los datos en Docker?

Por defecto, todo lo que ocurre dentro de un contenedor **se pierde al detenerlo o eliminarlo**, porque los contenedores son efímeros.

> Si instalás algo o guardás archivos dentro del contenedor sin un volumen, **desaparecen al reiniciarlo**.

---

## 📦 ¿Qué es un volumen?

Un **volumen** es un directorio persistente gestionado por Docker. Puede:

- Guardar datos fuera del ciclo de vida del contenedor
- Compartirse entre varios contenedores
- Estar en el host o en otro contenedor

---

## 🧪 Usar volúmenes en la práctica

### 1. Crear un volumen

```bash
docker volume create datos-db
```

### 2. Usar el volumen al ejecutar un contenedor

```bash
docker run -d \
  --name mysql \
  -e MYSQL_ROOT_PASSWORD=1234 \
  -v datos-db:/var/lib/mysql \
  mysql:8
```

> Esto asegura que la base de datos se guarde incluso si borrás el contenedor.

---

## 🗂️ Montaje de volumen desde el host (bind mount)

```bash
docker run -v /ruta/local:/ruta/contenedor imagen
```

Ejemplo:

```bash
docker run -v $(pwd):/app php:8.2-cli php /app/index.php
```

> Esto permite editar archivos localmente y que los cambios se vean en el contenedor.

---

## 📌 Buenas prácticas

- Usar volúmenes para bases de datos, logs y archivos subidos por usuarios
- Evitar escribir en el contenedor si no es necesario
- Usar `docker volume ls` y `docker volume prune` para gestionarlos

---

## 📚 Tareas sugeridas

1. Crear un contenedor MySQL con volumen.
2. Reiniciar el contenedor y verificar que los datos persisten.
3. Crear un contenedor PHP que use un volumen montado desde tu carpeta local.

---

# Curso de Docker – Capítulo 4: Redes en Docker

## 🎯 Objetivos del capítulo

- Entender cómo Docker gestiona redes internas
- Conectar contenedores entre sí usando nombres
- Crear redes personalizadas
- Aplicar una red interna en un stack Laravel + MySQL

---

## 🌐 ¿Qué es una red en Docker?

Docker crea una red virtual interna para que los contenedores se comuniquen. Hay varios tipos:

| Tipo         | Descripción                            |
|--------------|----------------------------------------|
| `bridge`     | Red local por defecto (modo aislado)   |
| `host`       | Comparte la red del host               |
| `none`       | Sin acceso a red                       |
| `overlay`    | Para múltiples hosts (Swarm)           |

---

## 🧱 Crear una red personalizada

```bash
docker network create mi-red
```

### Conectar dos contenedores a esa red:

```bash
docker run -d --name mysql --network mi-red -e MYSQL_ROOT_PASSWORD=1234 mysql:8

docker run -it --rm --name cliente --network mi-red alpine sh
```

Desde `cliente`, podés hacer ping a `mysql`:

```sh
ping mysql
```


---

## 📚 Tareas sugeridas

1. Crear una red personalizada y conectar dos contenedores.
2. Comprobar comunicación usando ping o MySQL CLI.
3. Usar una red interna en `docker-compose.yml` 

---



# Curso de Docker – Capítulo 5: Docker Compose

## 🎯 Objetivos del capítulo

- Entender qué es Docker Compose y por qué es útil
- Aprender a escribir un archivo `docker-compose.yml`
- Levantar múltiples servicios con un solo comando
- Administrar aplicaciones complejas fácilmente

---

## 📦 ¿Qué es Docker Compose?

**Docker Compose** es una herramienta oficial que permite definir y administrar múltiples contenedores Docker usando un solo archivo (`docker-compose.yml`).

> 🧠 En vez de ejecutar múltiples `docker run`, usás un solo archivo para describir toda tu aplicación.

---

## 📄 Sintaxis básica de `docker-compose.yml`

```yaml
version: '3.9'

services:
  nombre-del-servicio:
    image: nombre-de-la-imagen
    ports:
      - "puerto_host:puerto_contenedor"
    volumes:
      - ruta_host:ruta_contenedor
    environment:
      - VARIABLE=valor
```

---

## 🛠️ Ejemplo: Laravel + MySQL

```yaml
version: '3.9'

services:
  mysql:
    image: mysql:8
    container_name: mysql
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: laravel
    volumes:
      - mysql-data:/var/lib/mysql
    networks:
      - red-laravel

  app:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: laravel
    ports:
      - "8000:80"
    volumes:
      - .:/var/www/html
    depends_on:
      - mysql
    networks:
      - red-laravel

volumes:
  mysql-data:

networks:
  red-laravel:
```

---

## 🚀 Comandos esenciales

### Levantar los servicios

```bash
docker-compose up -d
```

> Ejecuta todos los contenedores en segundo plano.

### Ver contenedores activos del `compose`

```bash
docker-compose ps
```

### Ver logs de todos los servicios

```bash
docker-compose logs -f
```

### Detener y eliminar los contenedores

```bash
docker-compose down
```

> También elimina la red y los volúmenes anónimos si no se indican explícitamente.

---

## 📌 Ventajas de Docker Compose

- Gestión simple de múltiples servicios
- Definiciones claras y reutilizables
- Soporte para redes, volúmenes, variables de entorno y dependencias
- Perfecto para desarrollo local y testing de microservicios

---

## 📚 Tareas sugeridas

1. Crear un archivo `docker-compose.yml` con PHP y MySQL.
2. Agregar un servicio como `phpmyadmin` para administrar la DB.
3. Levantar todos los servicios con `docker-compose up`.

---


# Curso de Docker – Capítulo 6: Desarrollo y Debugging con Docker

## 🎯 Objetivos del capítulo

- Aprender a inspeccionar contenedores en ejecución
- Conectarse a un contenedor para depurar desde dentro
- Usar logs y ejecutar comandos dentro de contenedores
- Configurar hot reload para desarrollo local

---

## 🧪 1. Ejecutar contenedores en modo interactivo

Podés iniciar un contenedor y entrar a su terminal directamente:

```bash
docker run -it ubuntu bash
```

> Esto te da una **terminal Linux dentro del contenedor**, ideal para probar comandos o herramientas.

---

## 🔍 2. Ver contenedores activos

```bash
docker ps
```

Para ver todos (incluyendo detenidos):

```bash
docker ps -a
```

---

## 🛠️ 3. Conectarse a un contenedor existente

Si ya está corriendo, podés "entrar" a él:

```bash
docker exec -it nombre_o_id bash
```

Ejemplo:

```bash
docker exec -it laravel-app bash
```

> Si la imagen no tiene `bash`, podés usar `sh`:

```bash
docker exec -it node-app sh
```

---

## 📜 4. Ver logs del contenedor

```bash
docker logs nombre_o_id
```

Con seguimiento en tiempo real:

```bash
docker logs -f nombre_o_id
```

---

## 🧰 5. Inspeccionar contenedor

Ver toda la información del contenedor:

```bash
docker inspect nombre_o_id
```

Ver solo su IP:

```bash
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' contenedor
```

---

## 🪄 6. Hot reload para desarrollo

En desarrollo es común usar **volúmenes** para reflejar los cambios de archivos locales dentro del contenedor:

```yaml
services:
  app:
    image: node:18
    volumes:
      - .:/app
    working_dir: /app
    command: npm start
    ports:
      - "3000:3000"
```

> Esto permite que si cambiás el código local, el contenedor lo vea inmediatamente. Usá herramientas como `nodemon` o `artisan serve --watch`.

---

## 🛡️ 7. Eliminar contenedores de forma segura

```bash
docker stop nombre
docker rm nombre
```

Eliminar todos los detenidos:

```bash
docker container prune
```

---

## 📚 Tareas sugeridas

1. Ejecutar un contenedor Ubuntu y explorar su terminal.
2. Usar `docker exec` para conectarte a un contenedor en ejecución.
3. Ver los logs de un servicio como `mysql`, `node`, o `php-fpm`.
4. Configurar un volumen para desarrollo con hot reload.

---

## 📌 En el próximo capítulo...

> **Capítulo 7: Docker en Producción**
> - Optimizar imágenes con Alpine y multi-stage builds
> - Configurar variables de entorno seguras
> - Crear imágenes pequeñas, seguras y listas para deploy


