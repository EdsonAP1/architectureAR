
               ┌────────────────────────────────────────────────────────┐
               │                    CLIENTE (Navegador)                  │
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
   └─────────────────────────────────┘           │  │ Cloudflare Tunnel        │   │
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


# ARCH


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
