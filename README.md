# 🎬 Simulación real: ChatGPT → Windsurf

## ACTO 0 – Estado inicial

Tú tienes:

El HTML aprobado por el cliente

Tu manual

Un proyecto vacío en Windsurf (o repo recién creado)

No hay backend aún. No hay FastAPI. Solo intención.
____________
## ACTO 1 – Conversación A en ChatGPT

(Análisis del HTML)

👉 Qué haces tú

1. Abres una conversación nueva en ChatGPT

2. Pegas solo esto:
```text
Actúa como analista de producto y backend.

Analiza el siguiente HTML (vista ya aprobada por el cliente).

Tareas:
1. Identificar entidades del dominio
2. Identificar estados y acciones
3. Proponer el contrato API mínimo necesario
4. No generar código
5. No asumir lógica no visible en la UI
```
HTML:
[PEGAS EL HTML]
_________
🧠 Qué hace ChatGPT
Analiza la UI y responde con algo parecido a:
```text
ENTIDADES
- Appointment
- Customer
- Service
- User (staff/admin)

APPOINTMENT
- id
- customer_id
- service_id
- start_time
- end_time
- status
- price
- notes

ESTADOS
- pending
- confirmed
- cancelled

ENDPOINTS
- GET /appointments?date=
- POST /appointments
- PATCH /appointments/{id}/confirm
- PATCH /appointments/{id}/cancel
```
👉 Esto NO es código.
👉 Lo copias y lo guardas.

🎯 Fin del Acto 1
____
## 🧱 ACTO 2 – Conversación B en ChatGPT
(Scaffolding)

👉 Qué haces tú

1. Abres otra conversación nueva

2. Pegas:
```text
Usa estrictamente las siguientes reglas:

Actúa como un Lead Software Engineer experto en FastAPI, arquitecturas multi-tenant y sistemas backend orientados a producto.

Objetivo:
Construir un backend modular y escalable siguiendo el principio:
“Menos infraestructura, más valor entregado”.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ARQUITECTURA OBLIGATORIA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Estructura estricta e innegociable:

app/
├── main.py
├── core/
│   ├── config.py
│   ├── security.py
│   └── session.py
├── db/
│   ├── models.py
│   └── router.py

No crees carpetas adicionales fuera de esta estructura.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
STACK
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- Python 3.12+
- FastAPI
- SQLAlchemy 2.0 async
- asyncpg
- Pydantic v2

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
MULTI-TENANCY (CRÍTICO)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- Todas las tablas heredan de una base con tenant_id obligatorio.
- Toda query debe filtrar por tenant_id.
- tenant_id se obtiene del contexto de request.
- Está prohibido ejecutar queries sin aislamiento de tenant.
- Middleware tenant:
  - Extrae tenant del token o headers.
  - Coloca tenant_id en request.state.tenant_id.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DB & SSL (NEON)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
-Crear engine con create_async_engine usando:

connect_args={"ssl": ssl_context}


ssl_context con:

ssl.create_default_context()
ssl_context.check_hostname = False
ssl_context.verify_mode = ssl.CERT_NONE


La URL de conexión se carga desde .env (DATABASE_URL)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
COOKIES & CONFIG via .env
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
.env define:

COOKIE_SECURE=false
COOKIE_SAMESITE=lax
DATABASE_URL=...
PORT=8000
CORS_ORIGINS=http://localhost:3000,https://vercel.app


El backend debe leer estas variables automáticamente y aplicarlas.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SEGURIDAD & CORS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- Auth con cookies httpOnly
- CORS dinámico (localhost + Vercel)
- credentials=True

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
OBSERVABILIDAD
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- Endpoint GET /health

  - Ejecuta SELECT 1

   - Mide latencia DB

- Logging básico con tenant_id por request

- Mostrar al iniciar: URL docs + URL health

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
REGLAS GENERALES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- Código async y tipado
- Sin lógica de negocio en routers
- Sin texto explicativo innecesario
- Prioriza claridad y mantenibilidad


Usa este contrato de dominio ya validado:
[PEGAS EL OUTPUT DE FASE 1]
Usa estrictamente la arquitectura y reglas ya definidas.

Genera un nuevo módulo backend siguiendo el patrón estándar del proyecto.

Input:
- Nombre del módulo
- Entidad principal
- Campos
- Operaciones

Output:
- Router del módulo
- Modelos SQLAlchemy con tenant_id
- Schemas Pydantic
- Lógica necesaria
- Actualización de app/db/models.py
- Actualización de app/db/router.py
- Filtro tenant obligatorio en todas las queries

No inventes infraestructura nueva.
No omitas el filtro tenant_id.

Usa OVERLAY – SaaS B2B (Base)

- El sistema es multi-tenant por organización (tenant)
- Todo dato pertenece a una organización
- Los usuarios pertenecen a una organización
- Roles soportados:
  - owner
  - admin
  - staff
- El owner puede gestionar usuarios
- El sistema debe permitir auditoría futura (created_at, updated_at)
- El diseño debe ser compatible con billing futuro
- No asumir lógica enterprise innecesaria

Genera el prompt de scaffolding definitivo para crear el backend en Windsurf.
No generes código todavía.
```
Extra:

Cuando se arranca con:

uvicorn app.main:app --reload --host 0.0.0.0 --port 8000


Debe mostrar la dirección IP de la app, URL de /docs y /health.
_____

## ⚙️ ACTO 3 – Windsurf

(Creación real del proyecto)

👉 Qué haces tú en Windsurf

Abres Windsurf

Abres el proyecto vacío

En el chat de Windsurf pegas solo esto:

```text
[PEGAS EL PROMPT DE SCAFFOLDING GENERADO EN EL ACTO 2]
```
⏳ Esperas…
_____
🤖 Qué hace Windsurf

- Crea la estructura app/

- Genera:

  - main.py

  - core/config.py

  - db/models.py

  - db/router.py

- Prepara async engine

- Configura multi-tenant

- No inventa endpoints

🎯 Proyecto base creado
_____________
ACTO 4 – Input mínimo (crear un módulo)

👉 Qué haces tú

En el mismo chat de Windsurf, pegas ahora:
```text
MÓDULO: Gestión de Citas

ENTIDAD PRINCIPAL: Appointment

CAMPOS:
- service_id
- customer_id
- start_time
- end_time
- duration_minutes
- price
- notes
- status

OPERACIONES:
- Listar citas por fecha
- Crear cita
- Confirmar cita
- Cancelar cita

Todas las operaciones filtradas por tenant_id (obligatorio)
```
_________
🤖 Qué hace Windsurf

Crea:

modelos SQLAlchemy con tenant_id

schemas Pydantic

router del módulo

Registra el router

Filtra todas las queries por tenant_id

🎯 Módulo funcional
________
ACTO 5 – Resultado final

Tu repo ahora tiene:
```text
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
```
Ejectuas
# para ejecutar el FastAPI
```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```
Abres:

/docs

/health

Todo vivo 🔥



