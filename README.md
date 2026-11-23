# Veterinaria San Marcos – Frontend (React + Vite)

Tienda online, citas veterinarias y panel de administración. Consume la API REST del backend
(Spring Boot) a través del API Gateway.

## Requisitos

- Node.js **20.19+ o 22.12+** (recomendado Node 24).
- El backend iniciado en `http://localhost:8080`.

## Ejecutar

```bash
cp .env.example .env     # VITE_API_URL=http://localhost:8080/api/v1
npm install
npm run dev              # http://localhost:5173
```

Abrir siempre con `localhost` (no `127.0.0.1`) para que la cookie de sesión funcione.

## Scripts

| Comando | Qué hace |
|---|---|
| `npm run dev` | Servidor de desarrollo |
| `npm run build` | Compila para producción en `dist/` |
| `npm run test` | Pruebas con Vitest |
| `npm run lint` | ESLint |

## Integración con el backend

| Archivo | Responsabilidad |
|---|---|
| `src/services/api.js` | Cliente HTTP único: URL del Gateway, cabecera `Authorization`, renovación automática del token |
| `src/context/AuthContext.jsx` | Sesión: login, logout y recuperación al recargar la página |
| `src/context/TiendaContext.jsx` | Productos desde la API, carrito y compra (`POST /pedidos`) |
| `src/components/RutaProtegida.jsx` | Restricción de pantallas por rol |
| `src/hooks/useRecurso.js` | Carga de listas desde la API para el panel |

El access token (JWT) se mantiene solo en memoria. El refresh token viaja en una cookie HttpOnly
que administra el backend. En `localStorage` solo quedan el carrito y una marca de sesión sin datos sensibles.

## Roles

| Rol | Acceso |
|---|---|
| CLIENTE | Tienda, Mis Compras, Solicitar Cita, Mis Citas, Mi Perfil |
| VENDEDOR | Panel: Productos y Pedidos (solo lectura) |
| ADMIN | Panel completo |

## Backend

El backend (API Gateway + microservicios Spring Boot) está en su propio repositorio.
Debe estar iniciado en `http://localhost:8080` antes de abrir el frontend.

## Documentación

El manual de usuario está en `docs/`.

# Veterinaria San Marcos — Backend (Microservicios)

Backend desarrollado con **Spring Boot** bajo una arquitectura de microservicios para la plataforma web **Veterinaria San Marcos**, desplegado en **AWS EC2** mediante contenedores Docker.

---

## 🌐 Enlaces de Producción (AWS)

* **API Gateway (Base URL):** `http://13.221.202.90:8080/api/v1`
* **Documentación Interactiva (Swagger UI):** `http://13.221.202.90:8080/swagger-ui/index.html`
* **Frontend Conectado (AWS S3):** `http://veterinaria-sanmarcos-front-ev3.s3-website-us-east-1.amazonaws.com`

---

## 🏗️ Arquitectura Desplegada

* **Infraestructura:** Instancia **AWS EC2** (`m7i-flex.large`, Ubuntu) en la región `us-east-1` con grupo de seguridad habilitado en el puerto `8080`.
* **Orquestación:** **Docker** y **Docker Compose** para el despliegue unificado de los servicios.
* **Servicios:**
  * **API Gateway:** Enrutamiento centralizado y control de políticas **CORS** hacia el bucket S3 del frontend.
  * **Microservicios Spring Boot:** Gestión de autenticación/usuarios, catálogo de productos/servicios, citas médicas, pedidos y mensajes.
  * **Base de Datos:** **MySQL** contenedorizado con persistencia en volúmenes Docker.

---

## ⚙️ Variables de Entorno (`.env`)

Para ejecutar el proyecto en un entorno local o en un nuevo servidor, crea un archivo `.env` en la raíz del backend (basado en `.env.example`) con las siguientes variables principales:

```env
MYSQL_ROOT_PASSWORD=<clave_root_mysql>
MYSQL_DATABASE=veterinaria_sanmarcos
JWT_SECRET=<clave_secreta_jwt>
FRONTEND_ORIGINS=[http://veterinaria-sanmarcos-front-ev3.s3-website-us-east-1.amazonaws.com](http://veterinaria-sanmarcos-front-ev3.s3-website-us-east-1.amazonaws.com)
GATEWAY_URL=[http://13.221.202.90:8080](http://13.221.202.90:8080)
