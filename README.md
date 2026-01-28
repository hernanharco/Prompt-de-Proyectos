# Prompt-de-Proyectos
🚀 Prompt Maestro para futuros Backends SaaS Modulares
Copia y guarda este prompt. Está diseñado para que cualquier IA entienda tu Principio Rector y las restricciones técnicas de tu stack.

Prompt: "Actúa como un experto en arquitectura de software. Necesito construir un nuevo módulo/backend siguiendo una infraestructura SaaS Modular.

Stack Técnico: > - Framework: FastAPI (Python 3.12+).

DB: PostgreSQL (Neon) con SQLAlchemy 2.0 y asyncpg.

Estructura: Basada en carpetas por dominio (app/modules/nombre_modulo).

Tooling: Optimizado para Linux y pnpm para el frontend acompañante.

Reglas Estrictas:

Multi-tenant: El diseño debe permitir separar datos por tenant_id.

Seguridad: Autenticación basada en cookies httpOnly, configuración dinámica de CORS y manejo de SSL explícito para asyncpg (usando ssl.create_default_context).

Modularidad: El módulo debe ser independiente, con su propio router.py, service.py, models.py y schemas.py.

Eficiencia: Menos infraestructura, más valor. Prioriza rapidez de iteración y despliegue continuo.

Tarea: [Describe aquí el nuevo módulo, ej: 'Sistema de gestión de inventario para talleres']. Genera la estructura de archivos y el código base asegurando la conexión asíncrona robusta."
