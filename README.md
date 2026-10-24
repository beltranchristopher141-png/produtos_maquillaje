# Belleza Rosa — sistema de tienda de maquillaje

Proyecto académico full-stack basado en la guía de Express, API REST, React/Vite, Axios, CRUD, JOIN, variables de entorno y despliegue.

## Estructura

- `backend/`: API REST con Express, CORS y MySQL. CRUD de empleados, productos y ventas.
- `frontend/`: React + Vite + Axios; panel para administrar inventario, empleados y ventas.
- `database/maquillaje.sql`: crea la base y las tablas relacionadas e inserta datos de ejemplo.

## Requisitos

Node.js LTS, MySQL (por ejemplo, MySQL Workbench o XAMPP) y Git. La web se puede abrir localmente sin publicar nada.

## 1. Preparar MySQL

1. Abre MySQL Workbench o phpMyAdmin.
2. Ejecuta todo el contenido de `database/maquillaje.sql`.
3. Esto crea la base `maquillaje`, las tablas `empleados`, `productos`, `ventas` y algunos registros de ejemplo.

## 2. Preparar el backend

Abre una terminal en `Proyecto_Maquillaje/backend` y ejecuta:

```bash
npm install
```

Copia `.env.example` a un archivo llamado `.env` en esa misma carpeta. Actualiza `DB_USER`, `DB_PASSWORD`, `DB_HOST` y `DB_PORT` según tu MySQL. No subas `.env` a GitHub.

Inicia la API:

```bash
npm run dev
```

Prueba `http://localhost:3000/` en el navegador. Debe aparecer un mensaje de API funcionando. Mantén esa terminal abierta.

## 3. Preparar el frontend

Abre OTRA terminal en `Proyecto_Maquillaje/frontend` y ejecuta:

```bash
npm install
npm run dev
```

Abre la dirección que Vite muestre, normalmente `http://localhost:5173`. El archivo `src/api.js` usa `http://localhost:3000` por defecto y centraliza las llamadas a Express.

## 4. Cómo se conecta todo

1. React muestra formularios y tablas en el navegador.
2. Axios (`frontend/src/api.js`) manda solicitudes HTTP a la API Express.
3. Express (`backend/index.js`) valida los datos y ejecuta consultas MySQL.
4. MySQL almacena empleados, productos y ventas.
5. La ruta `GET /ventas` usa `JOIN` para mostrar el empleado, producto, cantidad, precio y total en una sola tabla.

Ambas terminales deben estar activas para probarlo localmente.

## 5. Publicar el frontend

GitHub almacena el código, pero **GitHub Pages por sí solo no ejecuta el backend Node ni aloja la base MySQL**. Publica el frontend en Vercel y el backend en Render; usa una base MySQL remota compatible (por ejemplo Aiven).

### GitHub

1. Crea un repositorio para el backend y otro para el frontend (o sube la carpeta raíz completa a un solo repositorio).
2. No incluyas `.env`, contraseñas ni datos privados. Los `.gitignore` ya excluyen `.env` y `node_modules`.
3. Desde la terminal de cada proyecto, inicializa Git y sube los archivos, por ejemplo:

```bash
git init
git add .
git commit -m "Proyecto tienda de maquillaje"
git branch -M main
git remote add origin https://github.com/TU-USUARIO/NOMBRE-REPOSITORIO.git
git push -u origin main
```

Reemplaza la URL de ejemplo por la URL real del repositorio creado. Si Git ya está inicializado, no repitas `git init` ni `git remote add`.

### Backend en Render

1. Crea una base MySQL remota y ejecuta el SQL adaptado a esa base. Guarda host, usuario, contraseña, base y puerto.
2. Sube `backend/` a un repositorio de GitHub.
3. En Render, crea un **Web Service** conectado al repositorio. Si el repositorio contiene solamente backend, usa raíz `.`; si está en un monorepo, configura Root Directory como `backend`.
4. Build command: `npm install`. Start command: `npm start`.
5. En Environment Variables de Render configura `DB_HOST`, `DB_USER`, `DB_PASSWORD`, `DB_NAME`, `DB_PORT` y `DB_SSL` si tu proveedor requiere SSL. Configura `PORT` solo si el servicio lo necesita; Express usa el puerto proporcionado por Render.
6. Cuando termine el despliegue, abre `https://TU-API.onrender.com/`. Guarda esa URL.

### Frontend en Vercel

1. Sube `frontend/` a GitHub.
2. En Vercel importa el repositorio. Si es monorepo, elige Root Directory `frontend`.
3. Build command: `npm run build`; Output Directory: `dist`.
4. Añade la variable de entorno `VITE_API_URL` con la URL real del backend de Render, sin una ruta final (ejemplo: `https://tu-api.onrender.com`).
5. Haz Deploy. Vercel generará el enlace público de tu página.
6. Si cambias `VITE_API_URL`, vuelve a desplegar. Verifica también que el backend permita CORS y que su base de datos sea accesible desde la nube.

## Importante

- No publiques contraseñas ni el archivo `.env`.
- El frontend publicado no podrá usar `localhost:3000`; en Vercel debes poner la URL pública del backend.
- Para el despliegue real, las tablas deben existir en la base MySQL remota y contener los datos que necesites.
- Para una práctica escolar, prueba primero todo localmente antes de publicar.
