# E-Commerce Flask + Supabase

E-commerce completo listo para producción con Python Flask, Supabase (PostgreSQL + Auth + Storage) y despliegue automatizado con GitHub Actions.

## 🎨 Paleta de Colores

- **Primario**: `#1C3FAA` (Azul profundo)
- **Acento**: `#FFB020` (Naranja dorado)
- **Éxito**: `#10B981` (Verde)
- **Error**: `#EF4444` (Rojo)
- **Neutros**: Escala de grises (`#F9FAFB` a `#111827`)

## 🚀 Características

### MVP Completo
- ✅ Catálogo con categorías, subcategorías, marcas y variaciones
- ✅ Búsqueda, filtros y ordenamientos
- ✅ Carrito persistente y wishlist
- ✅ Checkout completo (datos, envío, confirmación)
- ✅ Autenticación con Supabase Auth (email/password)
- ✅ Roles: Administrador, Gestor, Cliente
- ✅ Panel Admin completo (CRUD productos, pedidos, reportes)
- ✅ Pagos en modo sandbox (preparado para integración real)
- ✅ Envíos por departamento/municipio (Guatemala) + pickup
- ✅ Integración WhatsApp con mensaje prellenado
- ✅ Páginas institucionales (Nosotros, Contacto, Términos, etc.)
- ✅ Localización español (Guatemala), moneda GTQ (Q)

### Gestión de Productos (Solo Administrador)
- Alta/edición completa de productos
- Múltiples imágenes con Supabase Storage
- Variaciones (talla/color), SKU único
- Estados: borrador, pendiente, publicado, oculto
- Validaciones estrictas
- Historial de cambios (auditoría)
- Vista previa antes de publicar

## 📋 Requisitos Previos

- Python 3.12+
- Node.js 18+ (para TailwindCSS)
- Cuenta en [Supabase](https://supabase.com)
- Cuenta en [Render](https://render.com) o [Railway](https://railway.app)
- Git y GitHub

## 🛠️ Setup Local

### 1. Clonar el repositorio

```bash
git clone https://github.com/tu-usuario/ecommerce-flask-supabase.git
cd ecommerce-flask-supabase
```

### 2. Crear proyecto en Supabase

1. Ve a [supabase.com](https://supabase.com) y crea un nuevo proyecto
2. Anota las credenciales:
   - `SUPABASE_URL`: URL de tu proyecto
   - `SUPABASE_ANON_KEY`: Clave anónima (pública)
   - `SUPABASE_SERVICE_ROLE_KEY`: Clave de servicio (privada, solo server)
   - `DATABASE_URL`: Connection string de PostgreSQL

### 3. Configurar variables de entorno

```bash
cp .env.example .env
```

Edita `.env` con tus credenciales:

```env
# Flask
FLASK_ENV=development
FLASK_SECRET_KEY=tu-clave-secreta-muy-segura
FLASK_DEBUG=True

# Supabase
SUPABASE_URL=https://tu-proyecto.supabase.co
SUPABASE_ANON_KEY=tu-anon-key
SUPABASE_SERVICE_ROLE_KEY=tu-service-role-key
DATABASE_URL=postgresql://postgres:[PASSWORD]@db.tu-proyecto.supabase.co:5432/postgres

# Email (simulado en desarrollo)
MAIL_SERVER=localhost
MAIL_PORT=1025
MAIL_USERNAME=
MAIL_PASSWORD=
MAIL_USE_TLS=False
MAIL_DEFAULT_SENDER=noreply@tutienda.com

# WhatsApp
WHATSAPP_PHONE=+50212345678

# Pagos (sandbox)
PAYMENT_MODE=sandbox
```

### 4. Instalar dependencias

```bash
# Python
python -m venv venv
source venv/bin/activate  # En Windows: venv\Scripts\activate
pip install -r requirements.txt

# Node.js (TailwindCSS)
npm install
```

### 5. Provisionar base de datos

Ejecuta las migraciones en Supabase:

```bash
# Opción 1: Usando el script de Python
python manage.py provision

# Opción 2: Manualmente en Supabase SQL Editor
# Ejecuta en orden los archivos de supabase/migrations/
```

Esto creará:
- ✅ Esquema completo de tablas
- ✅ Políticas RLS
- ✅ Datos de ejemplo (categorías, marcas, productos, usuarios)
- ✅ Bucket de Storage para imágenes

### 6. Crear usuario administrador

```bash
python manage.py create-admin --email admin@tutienda.com --password Admin123!
```

### 7. Compilar assets

```bash
npm run build
```

### 8. Ejecutar en desarrollo

```bash
# Opción 1: Usando Make
make dev

# Opción 2: Directamente
flask run

# Opción 3: Con hot-reload de Tailwind
npm run dev  # En una terminal
flask run    # En otra terminal
```

La aplicación estará disponible en `http://localhost:5000`

## 🧪 Tests

```bash
# Todos los tests
make test

# Solo unitarios
pytest tests/unit -v

# Solo integración
pytest tests/integration -v

# Con cobertura
pytest --cov=app --cov-report=html

# E2E con Playwright
pytest tests/e2e -v
```

## 📦 Comandos Make

```bash
make dev          # Ejecutar en desarrollo
make test         # Ejecutar tests
make seed         # Cargar datos de ejemplo
make deploy       # Desplegar (via GitHub Actions)
make lint         # Linter (flake8/ruff)
make format       # Formatear código (black)
make clean        # Limpiar archivos temporales
```

## 🚢 Despliegue

### Configuración en Render/Railway

#### Opción 1: Render

1. Conecta tu repositorio de GitHub
2. Crea un nuevo **Web Service**
3. Configura:
   - **Build Command**: `pip install -r requirements.txt && npm install && npm run build`
   - **Start Command**: `gunicorn "app:create_app()" --bind 0.0.0.0:$PORT`
4. Agrega las variables de entorno desde `.env.example`
5. Despliega

#### Opción 2: Railway

1. Conecta tu repositorio de GitHub
2. Crea un nuevo proyecto
3. Railway detectará automáticamente el `Procfile`
4. Agrega las variables de entorno
5. Despliega

### GitHub Actions (Automático)

El despliegue se ejecuta automáticamente en cada push a `main`:

1. **CI**: Lint → Tests → Build
2. **Provision DB**: Aplica migraciones (manual o automático)
3. **Deploy**: Despliega a Render/Railway

#### Configurar Secrets en GitHub

Ve a `Settings > Secrets and variables > Actions` y agrega:

```
SUPABASE_URL
SUPABASE_ANON_KEY
SUPABASE_SERVICE_ROLE_KEY
DATABASE_URL
FLASK_SECRET_KEY
RENDER_API_KEY (o RAILWAY_TOKEN)
RENDER_SERVICE_ID (o RAILWAY_PROJECT_ID)
```

## 📁 Estructura del Proyecto

```
ecommerce-flask-supabase/
├── app/
│   ├── blueprints/
│   │   ├── admin/          # Panel de administración
│   │   ├── auth/           # Autenticación
│   │   ├── cart/           # Carrito de compras
│   │   ├── catalog/        # Catálogo y productos
│   │   ├── checkout/       # Proceso de compra
│   │   ├── main/           # Páginas principales
│   │   └── user/           # Perfil de usuario
│   ├── services/
│   │   ├── supabase.py     # Cliente Supabase
│   │   ├── auth.py         # Servicio de autenticación
│   │   ├── products.py     # Servicio de productos
│   │   ├── cart.py         # Servicio de carrito
│   │   ├── orders.py       # Servicio de pedidos
│   │   └── storage.py      # Servicio de Storage
│   ├── models/             # Modelos SQLAlchemy
│   ├── forms/              # Formularios Flask-WTF
│   ├── templates/          # Templates Jinja2
│   ├── static/
│   │   ├── css/            # CSS compilado
│   │   ├── js/             # JavaScript + HTMX
│   │   └── images/         # Imágenes estáticas
│   ├── utils/              # Utilidades
│   └── __init__.py         # Factory de aplicación
├── supabase/
│   └── migrations/
│       ├── 00_schema.sql   # Esquema de base de datos
│       ├── 01_rls.sql      # Políticas RLS
│       ├── 02_seed.sql     # Datos de ejemplo
│       └── 03_storage.sql  # Configuración Storage
├── tests/
│   ├── unit/               # Tests unitarios
│   ├── integration/        # Tests de integración
│   └── e2e/                # Tests end-to-end
├── .github/
│   └── workflows/
│       ├── ci.yml          # CI/CD principal
│       ├── deploy.yml      # Despliegue
│       └── provision-db.yml # Provisión de DB
├── infra/
│   ├── Procfile            # Para Render/Railway
│   └── render.yaml         # Configuración Render
├── manage.py               # CLI de gestión
├── requirements.txt        # Dependencias Python
├── package.json            # Dependencias Node.js
├── tailwind.config.js      # Configuración Tailwind
├── postcss.config.js       # PostCSS
├── pytest.ini              # Configuración pytest
├── .env.example            # Ejemplo de variables
└── README.md
```

## 👥 Usuarios de Prueba

Después de ejecutar el seed, tendrás estos usuarios:

| Email | Password | Rol |
|-------|----------|-----|
| admin@tutienda.com | Admin123! | Administrador |
| gestor@tutienda.com | Gestor123! | Gestor |
| cliente@tutienda.com | Cliente123! | Cliente |

## 🔒 Seguridad

- ✅ RLS (Row Level Security) en todas las tablas
- ✅ Validación server-side y client-side
- ✅ CSRF protection en formularios
- ✅ Rate limiting en endpoints críticos
- ✅ Sanitización de inputs
- ✅ Service Role Key solo en servidor
- ✅ Políticas de contraseñas fuertes
- ✅ Bloqueo tras intentos fallidos

## 📊 Reportes y Analytics

El panel de administración incluye:
- Ventas por rango de fechas
- Top productos más vendidos
- Top categorías
- Gráficos de tendencias
- Inventario bajo stock
- Pedidos por estado

## 🎨 Personalización de Branding

### Cambiar colores

Edita `tailwind.config.js`:

```js
module.exports = {
  theme: {
    extend: {
      colors: {
        primary: '#1C3FAA',    // Tu color primario
        accent: '#FFB020',     // Tu color de acento
        // ...
      }
    }
  }
}
```

Luego recompila:

```bash
npm run build
```

### Cambiar logo

Reemplaza los archivos en `app/static/images/`:
- `logo.svg` - Logo principal
- `logo-white.svg` - Logo para fondos oscuros
- `favicon.ico` - Favicon

## 📮 Colección Postman

Importa `postman_collection.json` en Postman para probar todos los endpoints.

## 🐛 Troubleshooting

### Error de conexión a Supabase

Verifica que `SUPABASE_URL` y las keys sean correctas. Prueba la conexión:

```bash
python manage.py test-connection
```

### Imágenes no se cargan

Verifica las políticas del bucket `products` en Supabase Storage:
- Lectura pública habilitada
- Subida solo para usuarios autenticados con rol Admin

### Migraciones fallan

Ejecuta las migraciones manualmente desde el SQL Editor de Supabase en orden:
1. `00_schema.sql`
2. `01_rls.sql`
3. `02_seed.sql`
4. `03_storage.sql`

## 📝 Licencia

MIT License - Ver `LICENSE` para más detalles.

## 🤝 Contribuir

1. Fork el proyecto
2. Crea una rama (`git checkout -b feature/nueva-funcionalidad`)
3. Commit tus cambios (`git commit -am 'Agrega nueva funcionalidad'`)
4. Push a la rama (`git push origin feature/nueva-funcionalidad`)
5. Abre un Pull Request

## 📧 Soporte

Para preguntas o soporte, abre un issue en GitHub.

---

**Desarrollado con ❤️ en Guatemala 🇬🇹**
