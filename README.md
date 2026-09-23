# LogiFlow — Sistema de gestión logística

Aplicación backend para una empresa de logística que centraliza la gestión de **clientes, flota de vehículos, choferes, envíos y facturación**. Expone una API REST con CRUD completo para cada entidad e incluye vistas web renderizadas en el servidor, autenticación de usuarios y control de acceso por roles.

Proyecto desarrollado en equipo para la materia Backend de la Tecnicatura Superior en Desarrollo de Software (IFTS N°29).

---

## Funcionalidades

- **CRUD completo** (GET, POST, PUT, PATCH, DELETE) para clientes, vehículos, choferes, envíos y facturas.
- **Autenticación con sesiones**: login y logout, con sesiones persistidas en MongoDB (`express-session` + `connect-mongo`).
- **Contraseñas encriptadas** con bcrypt antes de guardarse en la base de datos.
- **Control de acceso por roles**: cada rol tiene permisos definidos por sección y por acción (ver, crear, editar, eliminar), verificados mediante middleware.
- **Vistas web con Pug**: login, dashboard, envíos, flota, choferes, clientes y facturas.
- **Colecciones de Postman** incluidas para probar la API.

## Roles y permisos

| Rol | Acceso |
|---|---|
| **Administrador** | Acceso total a todas las secciones y acciones. |
| **Facturista** | Gestiona facturas (ver, crear, editar) y consulta envíos y clientes. |
| **Chofer** | Consulta los envíos. |

## Tecnologías

- **Node.js** y **Express 5**
- **MongoDB** con **Mongoose**
- **express-session** y **connect-mongo** (manejo de sesiones)
- **bcrypt** (encriptación de contraseñas)
- **Pug** (motor de vistas)
- **Postman** (pruebas de la API)

## Estructura del proyecto

```
├── controllers/          # Lógica de cada entidad y de autenticación
├── middleware/auth.js    # Verificación de sesión, roles y permisos
├── models/               # Esquemas de Mongoose (incluye Usuario con roles)
├── routes/               # Definición de endpoints por entidad
├── src/                  # Vistas Pug
├── public/               # Estilos y scripts del frontend
├── Postman Collections/  # Colecciones para probar la API
└── index.js              # Punto de entrada: servidor, sesiones y rutas
```

## Instalación y ejecución

1. Clonar el repositorio:

```bash
git clone https://github.com/luisimuller/LogiFlow-CRUD-Backend.git
cd LogiFlow-CRUD-Backend
```

2. Instalar las dependencias:

```bash
npm install
```

3. Configurar la conexión a MongoDB: en `index.js`, reemplazar la cadena de conexión en `mongoose.connect(...)` y en `mongoUrl` (configuración de sesiones) por la de tu propia base de datos, local o en MongoDB Atlas.

4. Iniciar el servidor:

```bash
npm run dev     # con recarga automática (nodemon)
# o
npm start
```

5. Abrir **http://localhost:3000** en el navegador.

## Usuarios de prueba

Al iniciar, el sistema crea automáticamente tres usuarios de ejemplo, uno por rol:

| Usuario | Contraseña | Rol |
|---|---|---|
| admin | admin123 | Administrador |
| facturista1 | facturista123 | Facturista |
| chofer1 | chofer123 | Chofer |

## Endpoints principales

Todas las rutas (excepto `/login`) requieren una sesión iniciada. Para probar la API desde Postman, primero hacer `POST /login` con usuario y contraseña; la cookie de sesión se reutiliza en las siguientes peticiones.

### Autenticación

| Método | Ruta | Descripción |
|---|---|---|
| GET | `/login` | Formulario de inicio de sesión |
| POST | `/login` | Iniciar sesión |
| GET | `/logout` | Cerrar sesión |

### Envíos

| Método | Ruta | Descripción |
|---|---|---|
| GET | `/envios/api` | Listar envíos |
| GET | `/envios/api/:id` | Obtener un envío |
| POST | `/envios/api/agregar` | Crear un envío |
| PUT | `/envios/api/:id` | Actualizar un envío completo |
| PATCH | `/envios/api/:id` | Actualizar un envío parcialmente |
| DELETE | `/envios/api/:id` | Eliminar un envío |

Ejemplo de body para crear un envío:

```json
{
  "idCliente": 1,
  "idVehiculo": 2,
  "idChofer": 1,
  "origen": "La Plata",
  "destino": "Mar del Plata",
  "fechaEnvio": "2024-02-01",
  "estado": "pendiente",
  "costo": 2000
}
```

### Facturas

| Método | Ruta | Descripción |
|---|---|---|
| GET | `/facturas/api` | Listar facturas |
| POST | `/facturas/api/agregar` | Crear una factura |
| PUT | `/facturas/api/:id` | Actualizar una factura completa |
| PATCH | `/facturas/api/:id` | Actualizar una factura parcialmente |
| DELETE | `/facturas/api/:id` | Eliminar una factura |

### Clientes, vehículos y choferes

Siguen el mismo patrón, con la base `/clientes`, `/vehiculos` y `/chofer`:

| Método | Ruta | Descripción |
|---|---|---|
| GET | `/{recurso}` | Listar |
| GET | `/{recurso}/:id` | Obtener por ID |
| POST | `/{recurso}/agregar` | Crear |
| PUT | `/{recurso}/:id` | Actualizar completo |
| PATCH | `/{recurso}/:id` | Actualizar parcialmente |
| DELETE | `/{recurso}/:id` | Eliminar |

## Modelo de datos

- **Cliente**: nombre, apellido, razón social, dirección, teléfono, mail.
- **Vehículo**: patente, tipo (camión, furgón, moto, etc.), capacidad, estado (activo, en mantenimiento, dado de baja).
- **Chofer**: nombre, apellido, DNI, licencia, teléfono.
- **Envío**: cliente, vehículo, chofer, origen, destino, fecha, estado y costo.
- **Factura**: envío asociado, fecha, monto y método de pago.
- **Usuario**: usuario, contraseña encriptada, nombre, email y rol.

## Equipo

- Luisina Müller
- Nicolás Chiovetta
- Adrián Leroy
- Leonel Donnet
- Hernán Burgos
