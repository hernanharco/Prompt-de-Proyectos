# 1. El Prompt Maestro
Copia y guarda este prompt. Está diseñado para que cualquier IA genere código compatible con tu estructura de app/core/settings.py, app/db/session.py, etc.

Prompt sugerido: 

```text
"Actúa como un desarrollador experto en FastAPI y SQLAlchemy. Analiza el siguiente HTML de
una interfaz y genera la infraestructura de backend siguiendo una arquitectura modular.

Entregables:

Model (SQLAlchemy): En app/models/[dominio].py. Usa la clase Base de app.models.base.

Schemas (Pydantic): En app/schemas/[dominio].py. Crea esquemas para Crear, Leer y Actualizar.

API Router: En app/api/v1/endpoints/[dominio].py.

Reglas Técnicas:

Usa rutas absolutas (ej: from app.db.session import get_db).

Implementa seguridad (ej: password debe ser un string que luego se hashee).

El código debe ser limpio y estar comentado para un desarrollador Junior.

HTML a analizar: [PEGA TU HTML AQUÍ]"
```
