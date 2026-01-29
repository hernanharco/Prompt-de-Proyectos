Manual Completo del Proyecto Windsurf – FastAPI Multi-Tenant (2026, Actualizado y Definitivo, Impecable)

🏁 ACTO 0 – Estado inicial

Tienes:

HTML aprobado por el cliente

Manual original

Proyecto vacío en Windsurf o repo recién creado

Sin backend todavía, solo intención

Mejoras añadidas:

⚠️ Activar entorno virtual manualmente antes de uvicorn
Preparado para hot reload y Pydantic v2
Auditoría y multi-tenant reforzados
JWT seguro con expiración y roles
Logging con tenant_id y request_id
Configuración Pydantic v2 con validación de asignación
Observabilidad mejorada (latencia DB y HTTP opcional)

🧱 ACTO 1 – Conversación A en ChatGPT (Análisis del HTML)

Qué hacer:

Abrir una conversación nueva en ChatGPT

Pegar:

Actúa como analista de producto y backend. Analiza el siguiente HTML (vista aprobada).  
Tareas:
1. Identificar entidades del dominio
2. Identificar estados y acciones
3. Proponer el contrato API mínimo necesario
4. No generar código
5. No asumir lógica no visible en la UI


HTML: [PEGAS EL HTML]

Salida esperada:

ENTIDADES

Appointment

Customer

Service

User (staff/admin)

APPOINTMENT

id

customer_id

service_id

start_time

end_time

status

price

notes

ESTADOS

pending

confirmed

cancelled

ENDPOINTS

GET /appointments?date=

POST /appointments

PATCH /appointments/{id}/confirm

PATCH /appointments/{id}/cancel

Guardar este output; no es código.

🧱 ACTO 2 – Conversación B en ChatGPT (Scaffolding)

Prompt maestro:

Actúa como Lead Software Engineer experto en FastAPI, arquitecturas multi-tenant y sistemas backend orientados a producto.


Objetivo: backend modular y escalable. “Menos infraestructura, más valor entregado”.

ARQUITECTURA:

app/
├── main.py
├── core/
│   ├── config.py
│   ├── security.py
│   └── session.py
├── db/
│   ├── models.py
│   └── router.py


No crear carpetas adicionales.

STACK:

Python 3.12+

FastAPI

SQLAlchemy 2.0 async

asyncpg

Pydantic v2

PyJWT (JWT support)

MULTI-TENANCY:

Todas tablas con tenant_id obligatorio

Toda query filtra por tenant_id

tenant_id desde request.state.tenant_id

Middleware tenant: extrae tenant de token o headers

DB & SSL:

create_async_engine con connect_args={"ssl": ssl_context}

ssl_context: ssl.create_default_context(), check_hostname=False, verify_mode=ssl.CERT_NONE

URL desde .env DATABASE_URL

CONFIG & COOKIES (.env):

COOKIE_SECURE=false
COOKIE_SAMESITE=lax
DATABASE_URL=...
PORT=8000
CORS_ORIGINS=http://localhost:3000,https://vercel.app


⚡ Mejoras Pydantic v2: extra = "ignore", validate_assignment = True

SEGURIDAD & CORS:

Auth con cookies httpOnly

CORS dinámico

credentials=True

JWT con expiración corta (ej: 30 minutos)

Roles SaaS: owner, admin, staff

OBSERVABILIDAD:

GET /health: SELECT 1 + latencia DB

Logging con tenant_id y request_id

Mostrar al iniciar URL /docs y /health

REGLAS:

Código async y tipado

Sin lógica de negocio en routers

Auditoría futura: created_at, updated_at

Compatible con billing

MEJORAS POR Pydantic v2 Y HOT RELOAD:

En schemas: regex → pattern en Field

En models: __table_args__ = {"extend_existing": True}

Evitar imports circulares router → service → models

Crear primero models.py → schemas.py → router.py

En Settings: extra = "ignore" y validate_assignment = True

JWT: instalar PyJWT y usar en security.py

Metadata global SQLAlchemy para evitar conflictos

Logs seguros: no imprimir tokens completos

⚠️ Activar entorno virtual manual antes de uvicorn

🧱 ACTO 3 – Windsurf (Creación del proyecto)

Qué hacer:

Abrir Windsurf con proyecto vacío
Pegar prompt de Acto 2
Esperar

Resultado:

Estructura app/ creada

Async engine configurado

Multi-tenant implementado

Healthcheck, logging y CORS listos

No endpoints de dominio aún

🎯 Proyecto base listo

🧱 ACTO 4 – Crear un módulo mínimo

Qué hacer:

MÓDULO: Gestión de Citas
ENTIDAD PRINCIPAL: Appointment

CAMPOS:

service_id

customer_id

start_time

end_time

duration_minutes

price

notes

status

tenant_id

created_at / updated_at

OPERACIONES:

Listar citas por fecha

Crear cita

Confirmar cita

Cancelar cita

⚠️ Todas filtradas por tenant_id
⚠️ Importación absoluta
⚠️ Crear primero models.py → schemas.py → router.py
⚠️ En models: __table_args__ = {"extend_existing": True}
⚠️ En schemas: Field(..., pattern="^(pending|confirmed|cancelled)$")

Qué hace Windsurf:

Modelos SQLAlchemy con tenant_id y extend_existing

Schemas Pydantic con pattern

Router registrado

Queries filtradas por tenant_id

created_at / updated_at con func.now()

🎯 Módulo funcional listo

🧱 ACTO 5 – Resultado final del repo

app/
├── main.py
├── core/
│   ├── config.py
│   ├── security.py
│   └── session.py
├── db/
│   ├── models.py
│   └── router.py
├── appointments/
│   ├── models.py
│   ├── schemas.py
│   ├── service.py
│   └── router.py


🚀 Cómo ejecutar FastAPI

# 1. Crear y activar entorno virtual
python3 -m venv venv
source venv/bin/activate

# 2. Instalar dependencias
pip install -r requirements.txt

# 3. Ejecutar servidor
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000


Abrir /docs y /health
Hot reload funciona sin conflictos con SQLAlchemy, Pydantic v2 y JWT

📦 requirements.txt recomendado

fastapi>=0.107
uvicorn[standard]>=0.23
sqlalchemy[asyncio]>=2.0
asyncpg>=0.27
pydantic>=2.5
python-dotenv>=1.0
PyJWT>=2.8


Mantener versiones mínimas para compatibilidad

Instalar pre-commit hooks opcionales: black, isort, flake8, bandit

⚡ Tips importantes

Multi-tenant obligatorio: todas queries filtradas por tenant_id

Hot reload: usar __table_args__ = {"extend_existing": True} en todos los modelos

Metadata global SQLAlchemy para evitar conflictos

Pydantic v2: Field(..., pattern="...") en lugar de regex

Pydantic Settings: extra = "ignore", validate_assignment = True

JWT: expiración corta, roles SaaS, instalar PyJWT

Logs seguros: tenant_id y request_id; nunca tokens completos

Imports circulares: romper ciclo router → service → models usando imports locales

Auditoría: todos los modelos con created_at / updated_at

Roles SaaS: owner, admin, staff

Código async y tipado

Observabilidad: medir latencia DB y HTTP en /health

Pre-commit hooks para calidad y seguridad

✅ Este manual ahora es definitivo, actualizado e impecable, listo para:

Subir a GitHub

Ejecutar sin errores de SQLAlchemy hot reload, Pydantic v2, JWT, multi-tenant, Settings, logging seguro
