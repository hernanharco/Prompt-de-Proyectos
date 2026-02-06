🚀 Prompt para Windsurf: Creación del Admin Modular
```text
Instrucciones para Windsurf:

Hola. Basado en los componentes Astro que hemos creado (Navbar, Hero, Services, Gallery, Testimonials, Contact, Footer), necesito construir un Panel de Administración dinámico bajo la ruta /admin.

Objetivos Técnicos:

Arquitectura: Crea una estructura de carpetas en src/pages/admin/ donde cada componente de la landing tenga su propia vista de edición (ej: /admin/hero, /admin/services).

Interactividad: Usa un enfoque de "Edición en Vivo" donde pueda actualizar textos, nombres de iconos de Material Symbols, URLs de imágenes y esquemas de colores (clases de Tailwind).

Persistencia (MongoDB): Diseña un esquema de datos en un archivo src/lib/models/SiteConfig.ts que unifique toda la configuración de la landing. Necesito ver la estructura del Schema de Mongoose/MongoDB que soporte esta flexibilidad.

Flujo de Trabajo: > * Usa pnpm para instalar cualquier dependencia necesaria (como mongoose o un validador de formularios como zod).

Aplica los cambios directamente en el código siguiendo mi regla de .windsurfrules.

El admin debe permitir previsualizar los cambios antes de guardarlos.

Seguridad Modular: Prepara el sistema para que en el futuro podamos añadir la autenticación por cookies httpOnly que tenemos en nuestra estrategia SaaS.

Entregables específicos:

Archivo de Schema para MongoDB.

Layout del Dashboard Admin con Sidebar de navegación.

Página /admin/index.astro con un resumen del estado del sitio.

Formularios dinámicos para editar el componente Hero y Services como prioridad.

Por favor, explica el paso a paso de cómo conectarás los componentes de la landing para que lean los datos desde la base de datos en lugar de los arrays estáticos actuales.
```
