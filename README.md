# Product Pricing API

API que permite consultar el precio aplicable de un producto según su fecha, ID de producto e ID de marca.  
Construido con enfoque **API First**, usando **Java 21**, **Spring Boot 3**, **WebFlux**, **H2 con R2DBC**, y **OpenAPI 3.0**.

---

## 🧰 Tecnologías

- Java 21
- Spring Boot 3
- Spring WebFlux
- H2 + R2DBC
- OpenAPI 3.0 (API First)
- Gradle

---

## 🚀 Cómo ejecutar

```bash
./gradlew clean build
./gradlew bootRun
```

La API quedará corriendo en:

```
http://localhost:8080
```

Consola de H2 (modo Web):

```
http://localhost:8080/h2-console
```

- **JDBC URL:** `r2dbc:h2:mem:///testdb;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE`
- **User:** `capitole`
- **Password:** *(vacío)*

---

## 📄 Documentación OpenAPI

La especificación se encuentra en:

```
src/main/resources/product-pricing-api.yaml
```

Puedes visualizarla con [Swagger Editor](https://editor.swagger.io) o generar código con:

```bash
./gradlew openApiGenerate
```

---

## 🛠 Estructura del Proyecto

### Arquitectura

Se aplica **Arquitectura Limpia / Hexagonal**:

- **Dominio**: lógica de negocio, modelo `Price`
- **Aplicación**: servicios de aplicación y lógica de transformación
- **Infraestructura**: adaptadores para la base de datos (H2 con R2DBC)
- **Entrypoint (API)**: implementación expuesta a través de controladores WebFlux generados desde OpenAPI

```
src
├── main
│   ├── java
│   │   └── com.capitole.product_pricing_service
│   │       ├── domain        # Modelo de dominio (Price) y lógica de negocio
│   │       ├── application   # Servicios de aplicación
│   │       ├── infrastructure# Adaptadores R2DBC para H2
│   │       ├── api           # Interfaces generadas desde OpenAPI
│   │       └── handler       # Adaptadores primarios: implementación REST
│   └── resources
│       ├── application.yml
│       └── product-pricing-api.yaml
└── test
    └── ... # pruebas unitarias y de integración
```

---


## 📦 API Endpoint

```
GET /v1/capitole/product/pricing/applicable
```

### Parámetros:

| Nombre       | Tipo     | Requerido | Descripción                      |
|--------------|----------|-----------|----------------------------------|
| `date`       | string   | ✅        | Fecha ISO 8601                   |
| `product_id` | integer  | ✅        | ID del producto                  |
| `brand_id`   | integer  | ✅        | ID de la marca                   |

### Respuestas:

- `200 OK`: Precio encontrado
- `400 Bad Request`: Parámetros inválidos
- `404 Not Found`: No se encontró precio aplicable
- `500 Internal Server Error`: Error inesperado

---