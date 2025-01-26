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

### 💡 ¿Cómo funciona todo junto?

**Spring Boot**: Gestiona la lógica del **CRUD** (controladores, servicios, repositorios).

**Hibernate**: Se encarga de la conexión y manejo de datos con **PostgreSQL**.

**Docker**: Levanta **PostgreSQL** mediante `docker-compose.yml`.

**Maven**: Compila el proyecto y gestiona las dependencias.

---

## 📂 Estructura de Controllers, Entities y Repositories

### 1. Controllers

Esta carpeta contiene las clases que definen los endpoints de la API. Son responsables de manejar las solicitudes HTTP (GET, POST, PUT, DELETE, etc.) y delegar la lógica al repositorio o al servicio correspondiente.

- `ProductoController`: Define los endpoints para interactuar con los productos:
    - Obtener todos los productos.
    - Obtener un producto por ID.
    - Crear un nuevo producto.
    - Actualizar un producto existente.
    - Eliminar un producto por ID.

### 2. Entities 
Aquí se definen las clases que representan las tablas de la base de datos. Estas clases son los modelos del proyecto.

- `Producto`: Representa la tabla de productos en la base de datos, con atributos como:
    - `id`: Identificador único del producto.
    - `nombre`: Nombre del producto.
    - `precio`: Precio del producto.

### 3. Repositories
Esta carpeta contiene las interfaces que extienden de JpaRepository o CrudRepository, que proporcionan métodos básicos para interactuar con la base de datos sin escribir consultas SQL manuales.

- ProductoRepository: Es una interfaz que permite:
    - Guardar, actualizar y eliminar productos.
    - Buscar productos por ID o todos los registros.

---

## Explicancion del Codigo

### 📋 ProductoController

```java
@RestController
@RequestMapping("/productos")
public class ProductoController {
```
Define un controlador REST que responde a las solicitudes enviadas a `/productos`.

### Metodos

### 1. Obtener todos los productos
```java
@GetMapping
public List<Producto> obtenerProductos() {
    return productoRepository.findAll();
}
```
Devuelve una lista de todos los productos desde la base de datos.

### 2. Obtener producto por ID:
```java
@GetMapping("/{id}")
public Producto obtenerProductoPorId(@PathVariable Long id) {
    return productoRepository.findById(id)
        .orElseThrow(() -> new RuntimeException("No se encontró el producto con el id: " + id));
}
```
Busca un producto por su ID.
Lanza un error si el producto no existe.

### 3. Crear un producto:
```java
@PostMapping
public Producto crearProducto(@RequestBody Producto producto) {
    return productoRepository.save(producto);
}
```
Crea un nuevo producto y lo guarda en la base de datos.

### 4. Actualizar un producto:
```java
@PutMapping("/{id}")
public Producto actualizarProducto(@PathVariable Long id, @RequestBody Producto detallesProducto) {
    Producto producto = productoRepository.findById(id)
        .orElseThrow(() -> new RuntimeException("No se encontró el producto con el ID: " + id));

    producto.setNombre(detallesProducto.getNombre());
    producto.setPrecio(detallesProducto.getPrecio());

    return productoRepository.save(producto);
}
```
Actualiza el nombre y precio de un producto existente.

### 5. Eliminar un producto:
```java
@DeleteMapping("/{id}")
public String borrarProducto(@PathVariable Long id) {
    Producto producto = productoRepository.findById(id)
        .orElseThrow(() -> new RuntimeException("No se encontró el producto con el id: " + id));

    productoRepository.delete(producto);
    return "El producto con el ID: " + id + " fue eliminado correctamente.";
}
```
Borra un producto de la base de datos y devuelve un mensaje de confirmación.

---

# 📋 ProductoRepository
```java
public interface ProductoRepository extends JpaRepository<Producto, Long> {
}
```
Extiende de JpaRepository, lo que permite utilizar métodos ya implementados como:
- `findAll()`: Buscar todos los registros.
- `findById(Long id)`: Buscar un registro por su ID.
- `save(Producto producto)`: Guardar o actualizar un producto.
- `delete(Producto producto)`: Eliminar un producto.

---

# 📋 ApirestApplication
```java
@SpringBootApplication
public class ApirestApplication {
    public static void main(String[] args) {
        SpringApplication.run(ApirestApplication.class, args);
    }
}
```
Punto de entrada del proyecto.
Usa `SpringApplication.run` para iniciar la aplicación.

---

# 📝 Resumen

- **Controllers**: Manejan las solicitudes HTTP y llaman al repositorio.
- **Entities**: Representan las tablas de la base de datos.
- **Repositories**: Proveen métodos predefinidos para interactuar con la base de datos.