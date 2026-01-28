# para ejecutar el FastAPI
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000

##🧩 FASE 1 – Análisis del diseño (aquí SÍ pegas el HTML)

👉 Conversación A (análisis)
Aquí yo soy tu analista de dominio.

Prompt que usas:

Actúa como analista de producto y backend.

Analiza el siguiente HTML (vista ya aprobada por el cliente).

Tareas:
1. Identificar entidades del dominio
2. Identificar estados y acciones
3. Proponer el contrato API mínimo necesario
4. No generar código
5. No asumir lógica no visible en la UI

HTML:
[PEGAS EL HTML]

🎯 Output:

entidades

campos

endpoints

flujos

Este output es texto estructurado, no código.
