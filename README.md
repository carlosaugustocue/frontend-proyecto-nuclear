# Sistema de Gestión Veterinaria - Frontend

Frontend del Sistema de Gestión Veterinaria desarrollado con Vue 3, Vite y Vuetify.

## 🚀 Características

- **Vue 3** - Framework progresivo de JavaScript
- **Vite** - Build tool y dev server ultra rápido
- **Vuetify 3** - Librería de componentes Material Design
- **Pinia** - Gestión de estado moderno
- **Vue Router** - Enrutamiento
- **Axios** - Cliente HTTP
- **Autenticación JWT** - Seguridad con tokens

## 📋 Módulos Implementados

1. **Autenticación** - Login con JWT, validación de sesiones
2. **Dashboard** - Panel de control con estadísticas
3. **Citas** - CRUD completo de citas médicas
4. **Pacientes** - Gestión de mascotas
5. **Clientes** - Gestión de propietarios
6. **Facturas** - Facturación y pagos
7. **Inventario** - Gestión de productos
8. **Usuarios** - Gestión de usuarios y permisos

## 🛠️ Instalación

```bash
# Instalar dependencias
npm install

# Ejecutar servidor de desarrollo
npm run dev

# Build para producción
npm run build

# Preview de build
npm run preview

# Linting y formateo
npm run lint
```

## 🔧 Configuración

### Variables de Entorno

Crea un archivo `.env.local` en la raíz del proyecto:

```env
# URL del backend API
# En desarrollo, se usa el proxy de Vite (/api)
# En producción, debe apuntar a la URL completa del backend
VITE_API_URL=https://proyecto-nuclear-veterinaria-production.up.railway.app
```

**Nota:** En desarrollo local, el proxy de Vite redirige `/api` a `http://localhost:8080`, por lo que no necesitas configurar `VITE_API_URL` para desarrollo.

### Desarrollo Local

1. Asegúrate de que el backend esté corriendo en `http://localhost:8080`
2. Ejecuta `npm run dev`
3. El frontend estará disponible en `http://localhost:3000`

## 🌐 Conexión al Backend

### Desarrollo

El proxy de Vite redirige las llamadas a `/api` hacia `http://localhost:8080`:

```javascript
proxy: {
  '/api': {
    target: 'http://localhost:8080',
    changeOrigin: true,
  },
}
```

### Producción

En producción, el frontend usa la variable de entorno `VITE_API_URL` para conectarse al backend. Si no está configurada, intentará usar `/api` (útil si el frontend y backend están en el mismo dominio).

## 📁 Estructura del Proyecto

```
frontend/
├── src/
│   ├── main.js                 # Punto de entrada
│   ├── App.vue                 # Componente raíz
│   ├── plugins/
│   │   └── vuetify.js          # Configuración de Vuetify
│   ├── router/
│   │   └── index.js            # Rutas y guards
│   ├── stores/
│   │   ├── authStore.js        # Store de autenticación (Pinia)
│   │   ├── citasStore.js       # Store de citas
│   │   ├── clientesStore.js    # Store de clientes
│   │   └── ...                 # Otros stores
│   ├── composables/
│   │   ├── useApi.js           # Composable para API HTTP
│   │   ├── useNotification.js  # Notificaciones
│   │   └── useReferenceData.js # Datos de referencia
│   └── views/
│       ├── LoginView.vue       # Vista de login
│       ├── DashboardView.vue   # Dashboard principal
│       ├── CitasView.vue       # Listado de citas
│       ├── PacientesView.vue   # Listado de pacientes
│       ├── ClientesView.vue    # Listado de clientes
│       ├── FacturasView.vue    # Listado de facturas
│       └── ...                 # Otras vistas
├── index.html                  # HTML principal
├── vite.config.js              # Configuración de Vite
├── package.json                # Dependencias
├── Dockerfile                  # Docker para Railway
├── railway.json                # Configuración Railway
├── netlify.toml                # Configuración Netlify
└── README.md                   # Este archivo
```

## 🔐 Configuración de Autenticación

El frontend utiliza JWT (JSON Web Tokens) para autenticación:

1. El usuario se autentica en `/login`
2. El backend retorna un token JWT
3. El token se almacena en `localStorage`
4. Se envía en cada request en el header `Authorization: Bearer <token>`
5. Si el token expira, el usuario es redirigido a login

## 📱 Componentes principales

### Composable useApi

Proporciona métodos para hacer requests HTTP con autenticación automática:

```javascript
import { useApi } from '@/composables/useApi'

const { get, post, put, patch, delete: deleteRequest } = useApi()

const response = await get('/v1/citas')
```

### Store de Autenticación (Pinia)

Gestiona el estado de autenticación:

```javascript
import { useAuthStore } from '@/stores/authStore'

const authStore = useAuthStore()
await authStore.login(email, password)
authStore.logout()
```

## 🚀 Despliegue

### Netlify (Recomendado)

1. Conecta tu repositorio en [netlify.com](https://netlify.com)
2. Configura:
   - **Build command**: `npm ci && npm run build`
   - **Publish directory**: `dist`
3. Agrega variable de entorno:
   ```
   VITE_API_URL=https://proyecto-nuclear-veterinaria-production.up.railway.app
   ```

### Railway

1. Crea un nuevo servicio en Railway
2. Conecta tu repositorio
3. Railway detectará el `Dockerfile` automáticamente
4. Agrega variable de entorno:
   ```
   VITE_API_URL=https://proyecto-nuclear-veterinaria-production.up.railway.app
   ```

### Vercel

1. Conecta tu repositorio en [vercel.com](https://vercel.com)
2. Configura:
   - **Framework Preset**: Vite
   - **Build Command**: `npm run build`
   - **Output Directory**: `dist`
3. Agrega variable de entorno:
   ```
   VITE_API_URL=https://proyecto-nuclear-veterinaria-production.up.railway.app
   ```

## 🐛 Troubleshooting

### El frontend no se conecta al backend

- Verifica que el servidor Spring Boot esté corriendo en `http://localhost:8080` (desarrollo)
- Verifica que `VITE_API_URL` esté configurada correctamente (producción)
- Comprueba la configuración del proxy en `vite.config.js`
- Revisa la consola del navegador para errores CORS

### Token expirado

- El token JWT se valida automáticamente
- Si está expirado, se redirige a login
- Los datos se guardan en `localStorage` bajo la clave `token`

### Errores de CORS

- Asegúrate de que el backend tenga CORS habilitado
- Verifica `CORS_ALLOWED_ORIGINS` en `application-prod.properties` del backend
- Asegúrate de que la URL del frontend esté en la lista de orígenes permitidos

## 📚 Recursos

- [Vue 3 Docs](https://vuejs.org/)
- [Vite Docs](https://vitejs.dev/)
- [Vuetify Documentation](https://vuetifyjs.com/)
- [Pinia Documentation](https://pinia.vuejs.org/)
- [Vue Router Documentation](https://router.vuejs.org/)

## 📄 Licencia

Este proyecto es parte del Sistema de Gestión Veterinaria.
