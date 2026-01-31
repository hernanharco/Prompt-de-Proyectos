Prompt Optimizado (Copia y pega esto)
Actúa como un Senior Frontend Engineer experto en Next.js 15+ (App Router) y TypeScript.

Tarea: Generar un módulo funcional basado en un HTML/Imagen de API, aplicando Clean Architecture para un SaaS modular.

Arquitectura Obligatoria (Flujo de 3 capas):

Capa de Entrada (src/app/page.tsx): Debe ser un Server Component. Su única función es importar y renderizar el componente de vista. No debe tener 'use client'.

Capa de Presentación (src/components/[Nombre].tsx): Debe ser un Client Component ('use client'). REGLA ESTRICTA: No puede tener useEffect, ni manejar fetch, ni lógica compleja. Solo debe obtener datos y funciones desestructurando el Custom Hook y mapearlos al JSX con Tailwind CSS.

Capa de Lógica (src/hooks/use[Nombre].ts): Un Custom Hook que gestione el estado (useState), los efectos (useEffect), las llamadas a la API (usando la ruta /backend/...) y el tipado de TypeScript.

Instrucciones de estilo:

Usa Tailwind CSS para que el diseño sea idéntico al HTML/Imagen proporcionado.

Usa la configuración de API centralizada en @/config/api.

Implementa manejo de estados de carga (loading) y error.

Entregables:

src/hooks/use[Nombre].ts (La inteligencia).

src/components/[Nombre].tsx (La piel).

src/app/page.tsx (El contenedor).

Nota para Junior: Explica brevemente el "contrato" (las props/retornos) que creaste entre el Hook y el Componente para que entienda cómo se pasan la información.
