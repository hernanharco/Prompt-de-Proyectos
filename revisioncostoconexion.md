Contexto: Mi base de datos Neon (Serverless) está consumiendo demasiadas Compute Units (está en 28.2/100 CU-hrs) porque no logra entrar en estado de suspensión (Idle). Sospecho que el Frontend (Astro + Svelte) está realizando peticiones constantes (Polling) o manteniendo conexiones activas que no permiten que la DB se apague tras los 5 minutos de inactividad.

Tarea: Revisa todo el directorio de frontend/ y busca patrones de "Auto-refresh" o "Live Sync". Específicamente:

TanStack Query / Svelte Query: Busca configuraciones globales o locales. Asegúrate de que refetchOnWindowFocus, refetchOnMount y refetchInterval estén desactivados o configurados con tiempos muy largos (mínimo 15-30 minutos).

Native JS: Busca cualquier setInterval o setTimeout que realice llamadas a la API de forma cíclica.

WebSockets: Verifica si hay implementaciones de Socket.io o WebSockets nativos que mantengan el túnel abierto constantemente.

Astro HMR: Confirma que el Hot Module Replacement de desarrollo no esté afectando la conexión en producción (aunque esto es menos probable).

Objetivo: Modifica el código para que las peticiones solo ocurran por acción explícita del usuario (clic en refrescar) o que el intervalo de refresco sea de al menos 15 minutos. Aplica el principio de "Menos infraestructura, más valor": prioriza que la DB pueda dormir.
