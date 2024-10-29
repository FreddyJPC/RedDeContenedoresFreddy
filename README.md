# Práctica: Servidor de Base de Datos con pgAdmin

## Informe de conexión entre dos contenedores utilizando Docker

### 1. Tiempo invertido en la práctica
- **Duración :** 1 hora

### 2. Introducción a los conceptos utilizados

#### Docker
Docker es una herramienta que permite empaquetar aplicaciones y sus dependencias en contenedores. Estos contenedores son ligeros y ejecutan aplicaciones de forma aislada, compartiendo el núcleo del sistema operativo en lugar de crear una máquina virtual completa, lo que los hace más eficientes.

#### Componentes principales
- **Contenedor:** Entorno aislado que ejecuta aplicaciones según las instrucciones definidas en una imagen.
- **Imagen:** Plantilla que contiene las instrucciones y dependencias necesarias para crear un contenedor. Las imágenes se pueden compartir y reutilizar.

### 3. Requisitos previos

Para realizar esta práctica se requieren algunos conocimientos y configuraciones básicas:

- Familiaridad con Docker y Docker Desktop instalado en el equipo.
- Uso de la Terminal (PowerShell en Windows).
- Conocimiento básico sobre redes y puertos.
- Acceso a Internet para descargar imágenes de Docker.
- Un navegador web para acceder a la interfaz de pgAdmin.

### 4. Objetivos de la práctica

El propósito de esta práctica es:
- Crear y configurar dos contenedores (PostgreSQL y pgAdmin) utilizando Docker.
- Conectar estos contenedores en una red personalizada de Docker para que puedan comunicarse.
- Comprobar la conexión creando una base de datos y una tabla de prueba en PostgreSQL desde pgAdmin.

### 5. Material y herramientas necesarias

- Computadora con Docker Desktop instalado.
- Navegador web para acceder a pgAdmin.
- Documentación de Docker para referencia.

### 6. Procedimiento

#### Paso 1: Crear una red en Docker

Primero, configuramos una red personalizada en Docker para que ambos contenedores puedan comunicarse:

bash
docker network create --attachable my_network

Este comando crea una red llamada my_network, que es una red "adjuntable", permitiendo que otros contenedores se conecten a ella.

Paso 2: Crear el contenedor de PostgreSQL
Ejecutamos el siguiente comando para crear el contenedor de PostgreSQL, que actuará como base de datos:

docker run -d --name database --network my_network -e POSTGRES_PASSWORD=admin -p 5432:5432 postgres

Paso 3: Crear el contenedor de pgAdmin
Creamos el contenedor para pgAdmin, que servirá como interfaz gráfica para administrar la base de datos:
docker run -d --name pgadmin --network my_network -e PGADMIN_DEFAULT_EMAIL=freddypaguay20@gmail.com -e PGADMIN_DEFAULT_PASSWORD=admin -p 8089:80 dpage/pgadmin4

Paso 4: Configurar la conexión en pgAdmin
Accedemos a pgAdmin desde el navegador en http://localhost:8089.
Iniciamos sesión utilizando el correo y la contraseña configurados en el paso anterior.
En pgAdmin, seleccionamos Add New Server para configurar la conexión a PostgreSQL.
En la pestaña General, ingresamos un nombre para la conexión, como "PostgreSQL_Server".
En la pestaña Connection, configuramos los siguientes parámetros:
Host name/address: database (el nombre del contenedor de PostgreSQL).
Port: 5432.
Username: postgres.
Password: admin.


Paso 5: Verificar la conexión creando una base de datos y una tabla
Para comprobar que la conexión entre pgAdmin y PostgreSQL funciona correctamente, creamos una base de datos y una tabla de prueba:

En la interfaz de pgAdmin, expandimos el servidor que acabamos de agregar.
Hacemos clic derecho en Databases y seleccionamos Create > Database….
Asignamos un nombre a la base de datos (por ejemplo, prueba_db) y hacemos clic en Save.
Una vez creada la base de datos, la seleccionamos en el panel izquierdo.
Navegamos a Schemas > public > Tables, hacemos clic derecho en Tables y seleccionamos Create > Table….
Le damos un nombre a la tabla, como tabla_prueba, y agregamos algunas columnas (por ejemplo, una columna id de tipo entero y una columna nombre de tipo texto).
Guardamos la tabla y verificamos que se haya creado correctamente.

### 7. Resultados esperados
Al finalizar estos pasos, tenemos dos contenedores (PostgreSQL y pgAdmin) ejecutándose en la misma red local y comunicándose sin problemas. Pudimos conectarnos a PostgreSQL desde pgAdmin, crear una base de datos y añadir una tabla, confirmando que la configuración de red y los permisos están correctamente establecidos.

Esta práctica nos permite entender el proceso de configuración de contenedores en Docker y cómo conectar servicios mediante una red personalizada, brindando una experiencia local similar a la de un entorno de producción.
