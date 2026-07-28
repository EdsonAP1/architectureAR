# README

# Proyecto

Este proyecto implementa una arquitectura moderna basada en la separación entre **Frontend** y **Backend**, permitiendo un despliegue eficiente y escalable.

* **Frontend:** desplegado en **Cloudflare Pages**, encargado de la interfaz de usuario.
* **Backend:** desplegado en un **Servidor Casero** mediante **Docker Compose**.
* **Base de datos:** PostgreSQL.
* **Comunicación segura:** Cloudflare Tunnel.
* **API:** desarrollada en Go.

---

# Arquitectura del Sistema

```
               ┌────────────────────────────────────────────────────────┐
               │                    CLIENTE (Navegador)                 │
               └───────────────────────────┬────────────────────────────┘
                                           │
                    ┌──────────────────────┴──────────────────────┐
                    ▼                                             ▼
       [ Peticiones Estáticas (HTML/JS) ]             [ Peticiones API (JSON) ]
                    │                                             │
                    ▼                                             ▼
   ┌─────────────────────────────────┐           ┌─────────────────────────────────┐
   │         NUBE / CLOUD            │           │         SERVIDOR CASERO         │
   │  Cloudflare Pages               │           │  (Docker Compose)               │
   │  - React (Vite)                 │           │                                 │
   │  - Assets / UI                  │           │  ┌───────────────────────────┐  │
   └─────────────────────────────────┘           │  │ Cloudflare Tunnel         │  │
                                                 │  └─────────────┬─────────────┘  │
                                                 │                ▼                │
                                                 │  ┌───────────────────────────┐  │
                                                 │  │ Backend (Go API)          │  │
                                                 │  └─────────────┬─────────────┘  │
                                                 │                ▼                │
                                                 │  ┌───────────────────────────┐  │
                                                 │  │ Base de Datos (PostgreSQL)│  │
                                                 │  └───────────────────────────┘  │
                                                 └─────────────────────────────────┘
```

---

# Descripción de la Arquitectura

El sistema está dividido en dos componentes principales:

## Frontend

El cliente accede desde un navegador web. Los archivos estáticos (HTML, CSS, JavaScript e imágenes) son servidos por **Cloudflare Pages**, donde se encuentra desplegada la aplicación desarrollada con **React** y **Vite**.

## Backend

Las solicitudes a la API son dirigidas al servidor casero mediante **Cloudflare Tunnel**, el cual expone de forma segura el servicio sin necesidad de abrir puertos públicos.

Dentro del servidor se ejecutan los contenedores administrados por **Docker Compose**, donde se encuentran:

* Backend desarrollado en Go.
* Base de datos PostgreSQL.

El Backend procesa las solicitudes, aplica la lógica del negocio y realiza las operaciones correspondientes sobre la base de datos.

---

# Flujo de Funcionamiento

1. El usuario accede a la aplicación desde su navegador.
2. Cloudflare Pages entrega el Frontend (React).
3. El Frontend consume la API mediante solicitudes HTTP en formato JSON.
4. Cloudflare Tunnel redirige las solicitudes hacia el servidor casero.
5. El Backend en Go procesa la petición.
6. PostgreSQL almacena o recupera la información solicitada.
7. El Backend responde con datos en formato JSON.
8. El Frontend actualiza la interfaz del usuario.

---

# Tecnologías Utilizadas

| Componente       | Tecnología        |
| ---------------- | ----------------- |
| Frontend         | React + Vite      |
| Backend          | Go                |
| Base de Datos    | PostgreSQL        |
| Contenedores     | Docker            |
| Orquestación     | Docker Compose    |
| Hosting Frontend | Cloudflare Pages  |
| Acceso Seguro    | Cloudflare Tunnel |

---

# Estructura del Proyecto

```
proyecto/
│
├── frontend/                     <-- DESPLIEGUE EN CLOUDFLARE PAGES (Sin Docker)
│   ├── src/                      <-- Código fuente en React (Componentes, Views, Hooks)
│   ├── public/                   <-- Favicon, imágenes estáticas
│   ├── index.html                <-- Punto de entrada HTML
│   ├── package.json              <-- Dependencias de Node (Vite, React, Tailwind, Axios)
│   └── vite.config.js            <-- Configuración de compilación con Vite
│
├── backend/                      <-- DESPLIEGUE EN SERVIDOR CASERO (Dentro de Docker)
│   ├── cmd/
│   │   └── api/
│   │       └── main.go           <-- Punto de entrada de la aplicación Go
│   ├── internal/                 <-- Lógica del negocio (Handlers, Routers, Models)
│   ├── go.mod                    <-- Módulo y dependencias de Go
│   ├── go.sum
│   └── Dockerfile                <-- Construcción de la imagen liviana en Go (Multi-stage)
│
├── database/                     <-- SCRIPTS SQL DE LA BASE DE DATOS
│   └── init.sql                  <-- Script de creación de tablas e inserciones iniciales
│
├── .gitignore                    <-- Ignorar node_modules, .env, binarios de Go
├── .env.example                  <-- Plantilla de variables de entorno
└── docker-compose.yml            <-- Orquestador Docker para tu Servidor Casero
```

---

# Descripción de Directorios

## frontend

Contiene toda la aplicación cliente desarrollada con React. Este directorio se despliega directamente en Cloudflare Pages y no requiere Docker.

## backend

Implementa la API REST desarrollada en Go. Se ejecuta dentro de un contenedor Docker y contiene toda la lógica del negocio.

## database

Incluye los scripts SQL necesarios para crear la estructura inicial de la base de datos e insertar información base.

## docker-compose.yml

Archivo encargado de orquestar los servicios del servidor casero, incluyendo el Backend y PostgreSQL.

## .env.example

Plantilla con las variables de entorno requeridas para la configuración del proyecto.

---

# Despliegue

## Frontend

* Compilación mediante Vite.
* Publicación automática en Cloudflare Pages.

## Backend

* Construcción mediante Docker.
* Ejecución utilizando Docker Compose.
* Exposición del servicio mediante Cloudflare Tunnel.

---

# Ventajas de la Arquitectura

* Separación clara entre Frontend y Backend.
* Escalabilidad de cada componente de forma independiente.
* Mayor seguridad gracias a Cloudflare Tunnel.
* Despliegue rápido y automatizado.
* Arquitectura modular y fácil de mantener.
* Uso eficiente de contenedores Docker.
* Base de datos centralizada mediante PostgreSQL.

---

# Licencia

Este proyecto puede distribuirse y modificarse según los términos definidos por su propietario o la licencia correspondiente.

