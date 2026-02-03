# 1. El Prompt Maestro
Copia y guarda este prompt. Está diseñado para que cualquier IA genere código compatible con tu estructura de app/core/settings.py, app/db/session.py, etc.

Prompt sugerido: 

```text
"Actúa como un desarrollador experto en FastAPI y SQLAlchemy. Analiza la lógica de la interfaz proporcionada y genera exclusivamente la infraestructura de backend siguiendo una arquitectura modular.

Entregables por cada dominio (services y business_hours):

Model (SQLAlchemy): En app/models/[dominio].py. Usa la clase Base de app.models.base. Para horarios, implementa una relación para manejar múltiples slots de tiempo (turno partido).

Schemas (Pydantic): En app/schemas/[dominio].py. Crea esquemas para Crear, Leer y Actualizar.

API Router: En app/api/v1/endpoints/[dominio].py.

Infraestructura de Despliegue: * Crea un Dockerfile optimizado para FastAPI.

Crea un setup.sh que reconozca entornos. Ejemplo: ./setup.sh production para configurar producción y ./setup.sh development para desarrollo.

Reglas Técnicas:

Gestión de paquetes: Usa siempre pnpm para ejecutar scripts o tareas relacionadas.

Rutas: Usa rutas absolutas (ej: from app.db.session import get_db).

Seguridad: Implementa validaciones robustas y manejo de tipos.

Didáctica: El código debe ser limpio y estar comentado paso a paso para un desarrollador Junior, explicando la lógica de la base de datos y los endpoints.

Lógica de la Interfaz a analizar (Referencia de datos): [PEGA AQUÍ EL CÓDIGO REACT/HTML QUE ME PASASTE]"
```
