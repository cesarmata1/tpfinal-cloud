# tpfinal-cloud — Vue 3 + Docker + Azure DevOps

Trabajo práctico final de Cloud: una aplicación **Vue 3 + Vite** que muestra el autor, la fecha y el **identificador de build** con el que se generó, desplegada como contenedor con **Nginx** y automatizada con **Azure DevOps** y **GitHub Actions**.

## ✨ Qué incluye
- **Frontend Vue 3** (`src/App.vue`): muestra el `VITE_BUILD_ID` inyectado en tiempo de compilación, para comprobar qué versión está desplegada.
- **Dockerfile multi-stage**
  1. `node:20-alpine` → instala dependencias y ejecuta `npm run build` (recibe `BUILD_ID` como argumento).
  2. `nginx:alpine` → sirve los estáticos de `dist/` en el puerto 80.
- **Azure Pipelines** (`.azure/pipelines/`)
  - Se lanza con push a `develop` y usa el grupo de variables `tpfinal-vars`.
  - Usa una **plantilla reutilizable** (`templates/_build-front.yml`): instala Node 20, `npm ci`, build con el commit como `VITE_BUILD_ID` y publica `dist` como artefacto `front-dist`.
- **GitHub Actions** (`.github/workflows/deploy-container.yml`)
  - Con push a `main`: construye la imagen, la sube a **Docker Hub** etiquetada con el SHA del commit y la despliega en **Azure Web App**.

## 🏗️ Arquitectura
```
develop ──► Azure Pipelines ──► npm ci + build ──► artefacto front-dist
main    ──► GitHub Actions  ──► docker build ──► Docker Hub ──► Azure Web App (Nginx)
```

## ▶️ Ejecutar en local
```bash
npm install
npm run dev          # http://localhost:5173
```
Con Docker:
```bash
docker build --build-arg BUILD_ID=local -t tpfinal-cloud .
docker run -p 8080:80 tpfinal-cloud   # http://localhost:8080
```

## 🛠️ Tecnologías
Vue 3 · Vite · Docker · Nginx · Azure DevOps Pipelines · GitHub Actions · Docker Hub · Azure App Service
