# Memoria Técnica: Eventual

## Ingeniería Web - Curso 2025/26
**Universidad de Málaga**
**Autor:** Jesús Repiso Rio

---

## Despliegue de la Aplicación

La aplicación se encuentra desplegada y es accesible públicamente en las siguientes direcciones:

- **Frontend (Vercel):** [https://prueba-examen-web.vercel.app/](https://prueba-examen-web.vercel.app/)
- **Backend (Render):** [https://eventual-backend.onrender.com](https://eventual-backend.onrender.com)

## Tecnologías Utilizadas

El proyecto ha sido desarrollado utilizando el stack MERN y diversas herramientas modernas:

- **Frontend:** React + Vite, React Router, Axios, CSS Modules, React-Leaflet (Mapas), @react-oauth/google.
- **Backend:** Node.js, Express, Mongoose.
- **Base de Datos:** MongoDB Atlas.
- **APIs Externas:**
    - Cloudinary (Gestión de imágenes).
    - Google Identity Services (Autenticación OAuth).
    - Nominatim OpenStreetMap (Geocoding y Mapas).
- **Infraestructura:** Vercel (Frontend), Render (Backend).

## Instrucciones de Instalación y Despliegue

Debido a medidas de seguridad, los archivos de configuración sensibles y las dependencias (`node_modules`) han sido excluidos del repositorio mediante `.gitignore`. A continuación se detallan los pasos para poner en marcha el proyecto tanto en local como en la nube.

### Ejecución en Local

#### Paso 1: Clonado del Repositorio
```bash
git clone <URL_DEL_REPO>
cd pruebaExamenWeb
```

#### Paso 2: Instalación de Dependencias
Es necesario instalar las dependencias tanto para el servidor (backend) como para el cliente (frontend).

```bash
# Instalar dependencias del backend
cd backend
npm install

# Instalar dependencias del frontend
cd ../frontend
npm install
```

#### Paso 3: Configuración de Variables de Entorno
Se deben crear manualmente los archivos `.env` en las carpetas correspondientes.

**Frontend (`frontend/.env`):**
```bash
VITE_CLOUDINARY_CLOUD_NAME=<TU_CLOUD_NAME>
VITE_CLOUDINARY_UPLOAD_PRESET=<TU_UPLOAD_PRESET>
# URL del backend local (por defecto puerto 5000 o 3000)
VITE_API_URL=http://localhost:5000
```

**Backend (`backend/.env`):**
```bash
MONGO_URI=<TU_MONGO_URI>
PORT=5000
```

#### Paso 4: Ejecución
Para iniciar la aplicación en modo desarrollo:

```bash
# Terminal 1: Backend
cd backend
npm start

# Terminal 2: Frontend
cd frontend
npm run dev
```

### Despliegue en Nube
El despliegue se ha realizado conectando el repositorio de GitHub a los servicios de hosting:
- **Vercel (Frontend):** Se importó el proyecto seleccionando la carpeta `frontend`. En la configuración del proyecto en Vercel, se añadieron las variables de entorno definidas anteriormente.
- **Render (Backend):** Se creó un Web Service conectado al repositorio, con el directorio raíz en `backend`. Se configuraron las variables de entorno (`MONGO_URI`) en el panel de control de Render.

## Funcionalidad Implementada

### Búsqueda y Mapa
La aplicación permite filtrar eventos basándose en la ubicación. El algoritmo calcula la distancia euclidiana entre la dirección buscada y los eventos disponibles, mostrando solo aquellos que se encuentran a una distancia menor a 0.2 unidades. Los resultados se visualizan en un mapa interactivo con marcadores.

### Geocoding Automático
Al crear o editar un evento, el usuario introduce una dirección textual. El sistema utiliza la API de Nominatim para convertir esta dirección en coordenadas (Latitud/Longitud) de forma transparente, permitiendo su posterior posicionamiento en el mapa.

### Gestión de Logs y Seguridad
Se ha implementado un sistema de autenticación mediante Google OAuth. Cada vez que un usuario inicia sesión, se genera un registro persistente en la colección `AccessLog` de MongoDB, almacenando:
- Timestamp del acceso.
- Email del usuario.
- Token de sesión (fragmento).

### Gestión de Imágenes
Las imágenes de los eventos se suben directamente desde el navegador del cliente a Cloudinary, optimizando el ancho de banda del servidor. La URL segura devuelta por Cloudinary es la que se almacena en la base de datos.

### Restricciones
Se han implementado controles de autorización en el frontend y backend. Las operaciones de edición y borrado de eventos están protegidas y restringidas exclusivamente al usuario organizador que creó el evento (verificado mediante email).
