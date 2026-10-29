# Comparativa técnica global: patrones y decisiones de diseño

Esta comparativa describe patrones a estudiar; no es un ranking ni prueba que una capacidad funcione en todas las implementaciones.

## Enfoques

| Enfoque | Ventajas potenciales | Límites y riesgos |
|---|---|---|
| Scripts de voz con acciones fijas | Simples, previsibles y fáciles de depurar | No generalizan; cada función requiere programación explícita |
| LLM con herramientas | Comprende peticiones variadas y elige herramientas | Puede elegir mal, inventar resultados o repetir acciones |
| Local-first | Mayor control de datos y posible funcionamiento sin red | Requiere hardware suficiente y puede aumentar latencia |
| APIs remotas | Modelos potentes sin instalar pesos localmente | Depende de red, cuota, precio y proveedor |
| Proveedores modulares | Cambiar STT/TTS/LLM de forma independiente | Adaptadores requieren mantenimiento |
| Memoria persistente/RAG | Recupera hechos y documentos entre sesiones | Puede recuperar datos viejos, contradictorios o sensibles |
| Automatización de escritorio | Opera aplicaciones y archivos reales | Frágil ante cambios de interfaz, permisos y ventanas |
| Agente de navegador | Útil para investigación y flujos web | Prompt injection, cambios de página, sesiones y acciones irreversibles |
| Multiagente | Reparte tareas y paraleliza análisis | Coordinación, coste, conflictos y propagación de errores |
| Panel de control | Hace visibles tareas, aprobaciones y estados | Puede simular actividad si no está conectado a un backend real |

## Arquitectura funcional a evaluar

1. Interfaz: voz, texto, atajos, estado y accesibilidad.
2. Entrada/percepción: STT, VAD, capturas, OCR y contexto de ventana con consentimiento.
3. Orquestación: convierte objetivo en plan, mantiene estados y permite cancelar.
4. Modelo: proveedor local/remoto, selección, límites y fallback.
5. Herramientas: contratos tipados, permisos, timeouts y validación.
6. Ejecutor: ejecuta acciones aisladas y recoge evidencia del resultado.
7. Memoria: preferencias, episodios y documentos con fuentes y controles de borrado.
8. Seguridad: confirmaciones independientes, permisos mínimos, secretos y aislamiento.
9. Observabilidad: registros, métricas, reproducción de errores y diagnósticos.
10. Evaluación: tareas reales, casos negativos, fallos inyectados y medición de éxito.

## Cómo hacer un asistente menos convencional

No basta con un HUD, voz sintética, palabra de activación o animación. Las diferencias deben ser comportamientos medibles:

- Autonomía con límites: planear varios pasos y pedir permiso para efectos sensibles.
- Memoria con procedencia: mostrar fuente, fecha y permitir corregir o borrar.
- Aprendizaje de correcciones: convertir correcciones explícitas en preferencias revisables.
- Recuperación de fallos: detectar error, explicar, intentar alternativa segura y conservar estado.
- Interacción continua: interrumpir voz, cambiar de canal y conservar contexto.
- Control observable: mostrar herramientas, permisos y cambios.
- Adaptación al equipo: medir hardware, latencia y coste y elegir local/nube según necesidad.
- Evaluación real: tasa de éxito verificado, errores, latencia y coste.
- Privacidad por defecto: conectores opt-in, secretos protegidos y borrado comprobable.
- Acciones reversibles: previsualizar cambios, crear respaldos y deshacer cuando sea viable.

## Estados de resultado

- done: efecto observado y comprobado.
- unverified: se intentó, pero no se pudo comprobar.
- failed: error identificado.
- needs_confirmation: requiere autorización.
- cancelled: el usuario canceló.

Una respuesta del modelo no es evidencia. Para acciones de escritura, envío o cambios del sistema, comprobar el estado final por una vía independiente cuando sea posible.

## Fuentes técnicas

- https://github.com/open-jarvis/OpenJarvis
- https://github.com/leon-ai/leon
- https://github.com/OpenVoiceOS/ovos-core
- https://github.com/topics/jarvis-ai
- https://github.com/topics/jarvis-assistant
- https://github.com/FatihMakes/Mark-LVI
- https://github.com/leon-ai/leon/issues
