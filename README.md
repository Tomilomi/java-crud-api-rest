## **API** **REST** con **Spring Boot**, **Hibernate** y **PostgreSQL**

> Este proyecto es una API REST construida con **Spring Boot**, **Hibernate** y **PostgreSQL**, con soporte para **Docker**. A continuación, algunas anotaciones.


# Estructura del Proyecto `java-crud-api-rest`

## 📂 Archivos y Carpetas

### 1. `.mvn/wrapper`
Contiene archivos para usar **Maven Wrapper**, lo que permite ejecutar Maven sin instalarlo globalmente:
- `mvnw` y `mvnw.cmd`: Scripts para ejecutar Maven en **Linux/Mac** o **Windows**.
- `wrapper/`: Archivos de configuración específicos de Maven Wrapper.

### 2. `src`
Carpeta principal del código fuente del proyecto:
- **`main/java`**: Contiene el código Java (controladores, servicios, repositorios, etc.).
- **`main/resources`**: Configuraciones del proyecto, como `application.properties` o `application.yml`.
- **`test/java`**: Código para pruebas unitarias o de integración.

### 3. `.env.template`
Plantilla para las variables de entorno. Ejemplo de lo que puede incluir:
```plaintext
DB_HOST=localhost
DB_PORT=5432
DB_NAME=mi_base_de_datos
DB_USER=usuario
DB_PASSWORD=contraseña
```
### 4. 📄 `.gitattributes`

Configuración para Git. Por ejemplo:

- Cómo manejar saltos de línea (LF o CRLF).
- Qué archivos considerar como binarios.
- Opciones para conflictos de fusión.

### 5. 📄 `.gitignore`

Lista de archivos y carpetas que Git debe ignorar. Ejemplo:
```
# Archivos de compilación
/target/
*.class

# Variables de entorno
.env

# Archivos del sistema operativo
.DS_Store
```
### 6. 📄 `docker-compose.yml`
Define servicios de docker para el proyecto. Ejemplo de lo que podria contener:
- **PostgreSQL**: Base de atos configurada con usuario, contraseña y volumen persistente.
- **Red**: Comunicacion entre la API y la base de datos

### 7. 📄 `pom.xml`
Archivo de configuración de Maven:

- **Dependencias**: Define bibliotecas necesarias como **Spring Boot**, **Hibernate** y **PostgreSQL**.
- **Configuración**: Especifica el empaquetado (.jar), versión de Java y plugins.

### 8. 📄 `mvnw` y `mvnw.cmd`
Scripts para ejecutar Maven sin instalarlo globalmente:

- `mvnw`: Usar en Linux/Mac.
- `mvnw.cmd`: Usar en Windows.
---
### 💡 ¿Cómo funciona todo junto?

**Spring Boot**: Gestiona la lógica del **CRUD** (controladores, servicios, repositorios).

**Hibernate**: Se encarga de la conexión y manejo de datos con **PostgreSQL**.

**Docker**: Levanta **PostgreSQL** mediante `docker-compose.yml`.

**Maven**: Compila el proyecto y gestiona las dependencias.