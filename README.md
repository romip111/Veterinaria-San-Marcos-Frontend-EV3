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

# Veterinaria San Marcos — Frontend Web(S3)

Aplicación web desarrollada en **React + Vite** para la gestión y tienda online de **Veterinaria San Marcos**, desplegada como sitio web estático en **AWS S3** y conectada a los microservicios en **AWS EC2**.

---

## 🌐 Enlaces de Producción (AWS)

* **Aplicación Web (AWS S3):** `http://veterinaria-sanmarcos-front-ev3.s3-website-us-east-1.amazonaws.com`
* **Backend API / Swagger (AWS EC2):** `http://13.221.202.90:8080/swagger-ui/index.html`

> **Nota de acceso:** La aplicación utiliza protocolo `HTTP`. Si el navegador redirige automáticamente a `HTTPS`, ingresar explícitamente usando `http://`.

---

## 🏗️ Arquitectura Desplegada

* **Alojamiento:** **AWS S3** (`us-east-1`) configurado con *Static Website Hosting* y política pública de lectura (`s3:GetObject`).
* **Enrutamiento SPA:** Configurado con `index.html` como documento de índice y de error para soportar navegación directa con React Router.
* **Comunicación:** Consumo de API REST a través del API Gateway en EC2 (`http://13.221.202.90:8080/api/v1`).

---

## 🔑 Credenciales de Prueba

El sistema cuenta con roles preconfigurados para validar los distintos flujos de la plataforma:

| Rol | Contraseña de Prueba | Acceso |
| :--- | :--- | :--- |
| **Administrador** | `Admin1234` | Dashboard, gestión de productos, usuarios, pedidos, citas y mensajes |
| **Vendedor** | `Vende1234` | Gestión de ventas, pedidos y atención |
| **Cliente** | `Cliente123` | Catálogo, carrito de compras, solicitud de citas e historial |

---

## ⚙️ Variables de Entorno

Crear un archivo `.env.production` en la raíz del proyecto antes de compilar para producción:

```env
VITE_API_URL=[http://13.221.202.90:8080/api/v1](http://13.221.202.90:8080/api/v1)

🛠️ Instalación, Compilación y Despliegue en S3
1. Instalar dependencias:

npm install

---
2. Ejecutar en modo desarrollo local:

npm run dev

---

3. Compilar archivos estáticos para producción (genera la carpeta dist/):

npm run build

---





