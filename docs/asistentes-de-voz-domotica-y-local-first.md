# Asistentes de voz local-first, domótica y satélites de voz

**Fecha de revisión:** 29 de octubre de 2026.  
**Propósito:** ampliar el estudio más allá de los agentes de escritorio y comparar sistemas cuyo centro es la voz, la automatización del hogar o el funcionamiento sin nube.

## Hallazgo principal

“Un asistente tipo JARVIS” no es una sola categoría técnica. Existen al menos tres familias diferentes:

1. **Agente general de escritorio:** trabaja con archivos, navegador, terminal, tareas largas y varios conectores.
2. **Asistente de voz/domótica:** transforma voz en intenciones y controla entidades del hogar; suele priorizar rapidez, fiabilidad de comandos y acciones concretas.
3. **Satélite de voz:** dispositivo con micrófono, altavoz y detección de palabra de activación que delega el procesamiento a un servidor o pipeline.

Comparar estas familias solo por “inteligencia” es engañoso. Se deben medir latencia, fiabilidad de comandos, alcance de acciones, funcionamiento sin internet, mantenimiento, hardware, privacidad y facilidad para añadir capacidades.

## 1. Home Assistant Assist: pipeline de voz componible

La documentación oficial describe un pipeline con etapas separadas:
- detección de palabra de activación;
- voz a texto (STT);
- reconocimiento de intención o agente conversacional;
- texto a voz (TTS).

Cada etapa puede ejecutarse en un dispositivo, en Home Assistant o en la nube, según los componentes seleccionados. Las etapas separadas ayudan a diagnosticar si falla la detección, la transcripción, la interpretación o la respuesta hablada.

Fuentes:
- https://www.home-assistant.io/voice_control/voice_remote_local_assistant
- https://contributors.home-assistant.io/architecture/platform/voice/
- https://contributors.home-assistant.io/core/internals/assist-pipelines/
- https://www.home-assistant.io/integrations/wyoming

### Modo completamente local

La guía oficial explica cómo configurar STT local (Whisper o Speech-to-Phrase), TTS local con Piper y la integración Wyoming. Eso puede mantener el audio dentro de la red doméstica, siempre que el usuario no configure componentes remotos adicionales.

No todos los motores locales tienen la misma capacidad. La documentación advierte que Speech-to-Phrase es de conjunto cerrado: puede ser muy rápido para comandos domésticos compatibles, pero no cubre automáticamente peticiones abiertas como cualquier conversación o todas las variantes de listas y temporizadores.

### Implicaciones técnicas

- Una cadena de voz puede fallar en una etapa sin que el resto esté roto.
- Los mensajes de error y los eventos de cada etapa son más útiles que una única respuesta “no entendí”.
- El streaming continuo de audio puede consumir CPU y red; la documentación recomienda detección local de actividad de voz para evitar enviar audio innecesariamente.
- “Local” depende de la configuración real de cada etapa, no del nombre del producto.

## 2. OpenVoiceOS (OVOS): arquitectura de servicios y habilidades

La documentación técnica de OVOS presenta una arquitectura de componentes:
- messagebus WebSocket para eventos;
- ovos-core como orquestador de intenciones y habilidades;
- listener para wake word y STT;
- servicio de audio/TTS;
- GUI y abstracción de hardware;
- scheduler y carga/descarga de habilidades.

Fuentes:
- https://github.com/OpenVoiceOS/ovos-core/blob/dev/docs/architecture.md
- https://openvoiceos.github.io/beta-technical-manual/core/

La ventaja de esta separación es que cada componente tiene un contrato y puede reemplazarse o depurarse de forma independiente. La contrapartida es operativa: múltiples procesos, servicios, plugins y eventos requieren configuración y diagnóstico. La arquitectura también documenta estados como started, alive, ready y stopping, lo que permite investigar problemas de arranque en lugar de tratarlos como un fallo misterioso del asistente completo.

La documentación de madurez es una declaración del proyecto; no sustituye pruebas de instalación, actualizaciones y recuperación.

## 3. Rhasspy: lección de un proyecto de voz offline y su ciclo de vida

El repositorio oficial de Rhasspy se presenta como un asistente offline, multilingüe y extensible, integrado con Home Assistant, Node-RED y otros sistemas. Describe servicios independientes coordinados mediante mensajes MQTT, además de API HTTP y WebSocket.

Fuente: https://github.com/rhasspy/rhasspy

**Estado relevante:** el repositorio principal figura archivado desde el 6 de octubre de 2025. Eso significa que ese repositorio ya no acepta cambios en su estado archivado; no implica que todos los componentes relacionados o las instalaciones existentes hayan dejado de funcionar.

El repositorio de wyoming-satellite también indica que fue archivado el 27 de enero de 2026 y que se reemplazó por Linux Voice Assistant con protocolo ESPHome:
- https://github.com/rhasspy/wyoming-satellite

La lección para cualquier asistente es que la calidad técnica no basta: hay que evaluar quién mantiene el proyecto, si hay una ruta de migración, si los componentes críticos siguen recibiendo parches y si los usuarios pueden exportar su configuración.

## 4. Willow: asistente orientado a hardware dedicado

Willow se describe como una plataforma abierta y autoalojada para hardware ESP32-S3-BOX, con integración para Home Assistant, openHAB y APIs REST. El proyecto anuncia detección de palabra de activación, VAD, mejoras de audio y dos caminos: reconocimiento de comandos en el propio dispositivo o procesamiento mediante Willow Inference Server.

Fuente: https://heywillow.io/

El sitio del proyecto anuncia cifras de latencia, tasa de fallos y coste de hardware. En esta investigación esas cifras se clasifican como **afirmaciones del proyecto**, no como mediciones independientes. Para verificarlas haría falta conocer el hardware exacto, la distancia al micrófono, ruido, idiomas, conjunto de comandos, número de pruebas, criterios de fallo y entorno de red.

Willow ilustra una estrategia distinta a un agente de PC: limitar el hardware y el flujo de comandos puede mejorar la previsibilidad y el tiempo de respuesta, pero no equivale a tener un agente general que pueda resolver cualquier tarea de escritorio.

## 5. Comparación por objetivo

| Sistema | Objetivo central | ¿Puede funcionar localmente? | Interfaz principal | Principal complejidad | Evidencia consultada |
|---|---|---|---|---|---|
| Home Assistant Assist | Control por voz y automatización del hogar | Sí, con pipeline local configurado | Voz, app y dispositivos satélite | Coordinar STT, intenciones, TTS e integraciones | Documentación técnica oficial |
| OpenVoiceOS | Plataforma de asistente de voz modular | Depende de los componentes elegidos; arquitectura orientada a servicios | Voz, skills y messagebus | Operación de varios servicios, plugins y eventos | Documentación de arquitectura |
| Rhasspy | Comandos de voz offline y personalizables | Sí, según los servicios configurados | Voz y web UI/API | Definición de intenciones y mantenimiento del ecosistema | README; repositorio principal archivado |
| Willow | Dispositivo de voz dedicado y rápido para hogar | Sí para comandos locales compatibles; también ofrece servidor opcional | Hardware ESP32-S3-BOX | Hardware específico, calidad de audio y límites de comandos | Claims del sitio oficial, no reproducidos |
| OpenClaw/Hermes/Goose/Jan | Agentes generales con herramientas y tareas | Varía; algunos componentes/proveedores son remotos | App, CLI, canales de chat o navegador | Seguridad de herramientas, contexto, memoria y ejecución prolongada | Documentación e issues, ver auditoría de seguridad |

## 6. Pruebas comunes que permiten comparar sin sesgo

No se deben comparar un asistente doméstico y un agente general con una única tarea. Para voz/domótica:
1. Comando compatible con una intención definida.
2. Variación lingüística natural del mismo comando.
3. Habla a distancia y con ruido.
4. Interrupción, timeout y dispositivo de audio cambiado.
5. Pérdida de red con componentes locales.
6. Acción ejecutada y verificación del estado real del dispositivo.

Para agente de escritorio:
1. Abrir/leer un archivo de prueba.
2. Modificar un archivo de prueba con confirmación.
3. Navegar una web no confiable sin obedecer instrucciones incrustadas.
4. Fallo de herramienta y detención segura.
5. Reinicio durante tarea y recuperación.
6. Memoria corregida, borrada y aislada entre perfiles.

Registrar tasa de éxito, falsos positivos, tiempo total, consumo, datos enviados fuera del equipo, número de pasos manuales y recuperación tras error. No usar solo la velocidad del primer resultado.

## 7. Fuentes y límites

- Home Assistant local voice assistant: https://www.home-assistant.io/voice_control/voice_remote_local_assistant
- Home Assistant voice architecture: https://contributors.home-assistant.io/architecture/platform/voice/
- Assist pipeline API: https://contributors.home-assistant.io/core/internals/assist-pipelines/
- Wyoming: https://www.home-assistant.io/integrations/wyoming
- OpenVoiceOS architecture: https://github.com/OpenVoiceOS/ovos-core/blob/dev/docs/architecture.md
- OVOS technical manual: https://openvoiceos.github.io/beta-technical-manual/core/
- Rhasspy: https://github.com/rhasspy/rhasspy
- Wyoming Satellite deprecación: https://github.com/rhasspy/wyoming-satellite
- Willow: https://heywillow.io/

Esta fase es documental. No se instalaron estos sistemas ni se reprodujeron sus benchmarks. Las afirmaciones de privacidad, rendimiento y fiabilidad se deben contrastar con configuración real, versión y pruebas independientes.
