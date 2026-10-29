# Mapa global inicial de asistentes tipo JARVIS

Inventario de partida, no ranking ni censo completo. Los proyectos con el nombre Jarvis no necesariamente comparten código ni capacidades.

| Proyecto/familia | Tipo | Qué permite estudiar | Límite de evidencia |
|---|---|---|---|
| FatihMakes MARK | Serie de asistentes de escritorio Python | Voz, control local, navegador, memoria, visión y acciones según la versión | Las capacidades varían por versión; README no sustituye pruebas |
| OpenJarvis | Framework local-first de Stanford SAIL/Hazy Research | Modelos locales, motores intercambiables, agentes, herramientas, memoria, tareas y métricas de energía/coste | Claims y métricas requieren revisar metodología y benchmarks |
| Leon | Asistente personal open source | Herramientas, contexto, memoria y ejecución agentic | README oficial indica que Leon 2.0 está en Developer Preview y parte de documentación antigua describe la arquitectura anterior |
| OpenVoiceOS | Plataforma abierta de asistente de voz | Arquitectura y reportes sobre servicio y arranque | Issues no representan a todos los usuarios |
| JARVIS de Danny el Arquitecto | Producto anunciado con interfaz web y Mission Control | Copiloto, memoria, voz y panel de agentes/tareas según páginas públicas | Backend oficial no verificado públicamente en esta pasada |
| JARVIS 3.0 en YouTube | Tutorial/demo | Descripción anuncia voz en tiempo real, wake word, memoria y control de PC | Descripción promocional; falta revisar el código completo y reproducir |
| Tema jarvis-ai en GitHub | Ecosistema heterogéneo | Cientos de repositorios con distintos lenguajes, objetivos y madurez | Tags sirven para descubrir, no para evaluar calidad |

## Fuentes primarias

- https://github.com/topics/jarvis-ai
- https://github.com/topics/jarvis-assistant
- https://github.com/open-jarvis/OpenJarvis
- https://github.com/open-jarvis/OpenJarvis/tree/main/docs
- https://github.com/leon-ai/leon
- https://github.com/leon-ai/leon/issues
- https://github.com/OpenVoiceOS/ovos-core/issues
- https://github.com/FatihMakes/Mark-LVI
- https://www.youtube.com/watch?v=qw16ascMH4Y
- https://www.dannyelarquitecto.com/jarvis
- https://dannyelarquitecto.com/mission-control/

## Primeros hallazgos

### OpenJarvis

La documentación describe una arquitectura modular con modelos, motor de inferencia, agentes, herramientas y memoria, además de un bucle de aprendizaje basado en trazas. Ofrece presets para investigación, tareas programadas, código y resúmenes. Es una referencia útil para estudiar asistentes local-first y mediciones de coste/energía. Las cifras de rendimiento publicadas son afirmaciones del proyecto y deben contrastarse con la metodología y benchmarks.

### Leon

El README indica que Leon 2.0 está en Developer Preview y que parte de la documentación antigua describe la arquitectura previa. Sus issues incluyen solicitudes de memoria externa, puntuación y caducidad de recuerdos, motores de búsqueda, modelos compatibles con OpenAI, así como reportes de descarga, puentes ausentes y conexión. Son reportes y peticiones concretas, no defectos universales.

### OpenVoiceOS

Las incidencias incluyen, entre otras, una posible carrera durante el arranque. Hay que leer el hilo completo y comprobar el estado actual antes de declarar el problema vigente.

### Tutoriales

Un tutorial de septiembre de 2026 anuncia voz en tiempo real, memoria persistente, wake word y control de PC. Eso documenta qué se presenta al público, no que cada función sea robusta o reproducible.

## Vacíos

Aún no se inspeccionaron todos los repositorios del tema GitHub ni todos los videos y comentarios. Instagram y otras redes pueden restringir contenido. No se realizó una prueba práctica conjunta en hardware equivalente y no se infieren sistemas internos a partir de clips promocionales.


## Nuevos candidatos añadidos en la segunda búsqueda

| Proyecto | Enfoque declarado | Preguntas para la auditoría |
|---|---|---|
| [psycoks/Jarvis](https://github.com/psycoks/Jarvis) | Asistente Windows local-first, wake word y STT/TTS locales, memoria, navegador visible, visión mediante Gemini y registros de auditoría | Confirmar qué componentes son locales y qué contenido sale a Gemini; probar instalación, permisos y fiabilidad del navegador |
| [RizN91/jarvis](https://github.com/RizN91/jarvis) | Asistente de voz/dictado para Windows con pulsar para hablar, datos locales y clave guardada en Credential Manager según su README | Medir consumo, revisar los límites del dictado y confirmar las afirmaciones de privacidad en el código |
| [OpenJarvis](https://github.com/open-jarvis/OpenJarvis) | Framework local-first con motores intercambiables, agentes, herramientas y métricas | Revisar benchmarks, compatibilidad real, requisitos y issues de instalación |
| [Leon](https://github.com/leon-ai/leon) | Asistente modular con herramientas, contexto, memoria y ejecución de agentes | Separar Developer Preview de la arquitectura legacy y clasificar issues por versión |
| [JARVIS 3.0 (YouTube)](https://www.youtube.com/watch?v=qw16ascMH4Y) | Tutorial de voz, memoria, wake word y control del PC | Localizar y auditar el código que el video enlaza; comparar lo mostrado con el código disponible |

Las capacidades de la tabla provienen de las descripciones públicas de cada proyecto; todavía no se consideran resultados de pruebas independientes.

## Tamaño del ecosistema

Las páginas de temas de GitHub mostraban alrededor de 212–213 repositorios con la etiqueta jarvis-ai en las búsquedas realizadas. El número cambia con el tiempo y los filtros; no representa todos los asistentes existentes, ni confirma que todos sean proyectos funcionales o mantenidos.

Fuentes de descubrimiento:
- https://github.com/topics/jarvis-ai
- https://github.com/topics/jarvis-assistant
- https://github.com/topics/jarvis
