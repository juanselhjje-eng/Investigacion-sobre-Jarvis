# Plan de investigación global: asistentes tipo JARVIS

Fecha de corte de esta fase: 29 de octubre de 2026.

## Objetivo

Crear un mapa amplio y basado en evidencia de asistentes personales de IA que se presentan como Jarvis o cumplen una función similar, en proyectos open source, productos comerciales, tutoriales, videos, comunidades técnicas y redes sociales públicas. Documentar capacidades, arquitectura, límites, preguntas frecuentes, errores, solicitudes de mejora, seguridad, privacidad, costes y diferencias entre demostración y funcionamiento comprobado.

No existe una forma realista de revisar literalmente cada publicación de internet. La investigación debe ser amplia, rastreable y explícita sobre muestras y vacíos; nunca se presentará como una revisión universal completa.

## Categorías de fuentes

1. Repositorios: código, README, licencias, releases, commits, tests, issues, pull requests y discusiones.
2. Videos y tutoriales: YouTube y otras plataformas con contenido público accesible; separar demostración, explicación de código, comentarios y seguimiento de versiones.
3. Redes sociales: publicaciones públicas, respuestas, comentarios accesibles y solicitudes repetidas. No asumir acceso a contenido privado.
4. Comunidades: foros técnicos, Reddit, discusiones de proveedores y reportes de instalación.
5. Productos comerciales: web oficial, documentación, precios, privacidad y límites declarados.
6. Investigación técnica: artículos, benchmarks, documentación de modelos, audio en tiempo real, agentes, memoria, herramientas y seguridad.

## Taxonomía técnica

- Entrada: palabra de activación, pulsar para hablar, micrófono continuo, texto, atajos, cámara, capturas, OCR y eventos del sistema.
- Voz: VAD, STT, TTS, streaming, interrupción, latencia, idioma, ruido y selección de dispositivo.
- Cerebro: modelo local o API, enrutamiento, fallback, contexto, límites, coste y degradación.
- Herramientas: navegador, aplicaciones, archivos, terminal, correo, calendario, mensajería, sistema operativo, domótica, APIs y conectores.
- Memoria: sesión, persistente, semántica/vectorial, episódica, preferencias, corrección, olvido, sincronización y recuperación.
- Agentes: planificación, subagentes, colas, tareas programadas, ejecución larga, reintentos, cancelación y recuperación tras reinicio.
- Visión: pantalla, múltiples monitores, ventanas, cámara, OCR, coordenadas y validación del efecto.
- Interfaz: HUD, bandeja, overlay, historial, estado de tareas, registros, permisos, diagnóstico y accesibilidad.
- Seguridad: prompt injection, contenido web no confiable, shell, permisos, confirmaciones, secretos, plugins, sandbox y borrado.
- Operación: instalación, dependencias, actualizaciones, consumo, red, disponibilidad, costes, compatibilidad y desinstalación.
- Experiencia real: éxito verificado, errores silenciosos, recuperación, confianza y control humano.

## Registro de cada hallazgo

Registrar ID, fecha, proyecto, URL, tipo de fuente, resumen fiel, estado (anunciado, visible en código, reportado, reproducido, resuelto, pendiente o no verificable), versión, SO, pasos de reproducción, impacto, solución sugerida, estado de solución, confianza y límites de la evidencia.

## Matriz de cobertura

| Área | Estado | Siguiente trabajo |
|---|---|---|
| Mapa de repositorios y familias | En ampliación | Añadidos OpenClaw, Hermes, Goose, Jan, Home Assistant Assist, OpenVoiceOS, Rhasspy y Willow; seguir inventario por categoría |
| Serie MARK | Parcial | Mantener como caso de estudio, no como representación total |
| Producto de Danny el Arquitecto | Parcial | Documentar claims públicos sin inferir backend privado |
| YouTube y tutoriales | Inicial | Hay un video registrado; revisar series completas y comentarios accesibles |
| Instagram y otras redes | Inicial | Registrar solo publicaciones públicas accesibles |
| Reddit y foros | Inicial | Clasificar quejas, soluciones y peticiones |
| Preguntas frecuentes | Inicial | FAQ de instalación, voz, modelos, memoria, coste y seguridad |
| Errores de instalación | Parcial | Añadidos issues recientes de OpenJarvis; ampliar con versiones y soluciones |
| Seguridad y privacidad | Ampliada documentalmente | Auditoría de OpenClaw, Hermes, Goose y Jan; faltan pruebas prácticas y verificar el estado actual de cada issue |
| Benchmarks y pruebas prácticas | Pendiente | Se definió protocolo común de instalación, voz, memoria, herramientas, seguridad y resistencia; falta ejecutarlo en equipos controlados |
| Comparativa global | Inicial | Comparar asistentes visibles y prototipos; falta muestreo amplio y pruebas |
| Por qué algunos proyectos no se vuelven famosos | Inicial | Síntesis de encuestas, documentación y casos de éxito; falta estudio longitudinal por proyecto |
| Progreso y vacíos | En curso | Actualizar por categorías cubiertas; no dar porcentaje mundial sin denominador defendible |

## Porcentaje

El porcentaje se calculará a partir de una matriz publicada, con pesos por categoría y ejemplos de fuentes revisadas. Se distinguirá cobertura documental, diversidad de proyectos/plataformas, cobertura de problemas comunitarios y verificación práctica. No se afirmará un porcentaje de “todo internet”.

## Preguntas centrales

1. ¿Qué proyectos se ejecutan y cuáles son prototipos o demos?
2. ¿Qué parte usa APIs y qué parte funciona sin internet?
3. ¿Cómo se verifica que una acción ocurrió?
4. ¿Qué sucede si falla voz, modelo, red, memoria o herramienta?
5. ¿Cómo se corrigen errores y recuerdos incorrectos?
6. ¿Qué requiere aprobación humana?
7. ¿Qué límites reporta la comunidad repetidamente?
8. ¿Qué sugerencias se implementan realmente?
9. ¿Cómo se comparan coste, latencia, privacidad, consumo y éxito?
10. ¿Qué hace útil a un asistente más allá de una interfaz futurista?

## Fuentes iniciales

- https://github.com/topics/jarvis-ai
- https://github.com/topics/jarvis-assistant
- https://github.com/open-jarvis/OpenJarvis
- https://github.com/leon-ai/leon
- https://github.com/leon-ai/leon/issues
- https://github.com/OpenVoiceOS/ovos-core/issues
- https://github.com/FatihMakes/Mark-LVI
- https://github.com/FatihMakes/Mark-LVI/issues
- https://www.youtube.com/watch?v=qw16ascMH4Y
- https://www.dannyelarquitecto.com/jarvis
- https://dannyelarquitecto.com/mission-control/


## Registro de esta actualización — 29 de octubre de 2026

- Se incorporó un análisis de barreras de adopción con fuentes de Pew Research Center, YouGov, ICO, GitHub y un snapshot de agentes open source.
- Se distinguió entre popularidad en GitHub y usuarios activos.
- Se amplió el mapa con OpenClaw, Hermes Agent, Goose y Jan.
- Se registraron issues de OpenJarvis como ejemplos recientes, manteniendo la etiqueta de reportes individuales.
- La cobertura mundial sigue sin porcentaje: el conjunto examinado aún no es una muestra estadística representativa y no se han reproducido las funciones en hardware real.


## Registro de esta actualización — 29 de octubre de 2026 (segunda ampliación)

- Se añadió [Auditoría profunda de seguridad y fiabilidad](auditoria-profunda-seguridad-fiabilidad.md), con documentación primaria de OpenClaw, Hermes Agent, Goose y Jan.
- Se documentaron riesgos de prompt injection, límites efectivos de herramientas, aprobación de MCP, separación de perfiles y crecimiento de memoria en procesos largos.
- Se incorporó un protocolo común de pruebas reproducibles; ninguna de estas pruebas se declara ejecutada en esta fase.
- Los issues se tratan como reportes fechados y sujetos a cambios, no como prueba de fallos universales o necesariamente vigentes.


## Registro de esta actualización — 29 de octubre de 2026 (tercera ampliación)

- Se añadió [Asistentes de voz, domótica y local-first](asistentes-de-voz-domotica-y-local-first.md).
- El mapa ahora separa agentes generales de escritorio, pipelines de voz/domótica y satélites de voz; no se evaluarán como si fueran el mismo producto.
- Se incorporó Rhasspy como caso de ciclo de vida y mantenimiento, porque su repositorio principal figura archivado; se documentó también la transición declarada de Wyoming Satellite.
- Se registraron los claims de rendimiento de Willow como afirmaciones del proyecto, no como resultados independientes.
- No se ejecutaron instalaciones ni pruebas físicas de voz en esta fase.


## Registro de esta actualización — 29 de octubre de 2026 (cuarta ampliación)

- Se añadió [Modelos gratuitos frente a pago y coste real](modelos-gratuitos-vs-pago-coste-real.md), con fuentes oficiales de Google, OpenRouter, Ollama, OpenAI y Anthropic. Incluye cuotas, datos, requisitos locales, costes ocultos, cálculo de tokens y arquitecturas gratuitas/híbridas/de pago.
- Se añadió [Investigación de UI, HUD y paneles de control](ui-hud-y-paneles-de-control.md), diferenciando chat, overlay, HUD, estado de voz, memoria visible y Mission Control; incluye consideraciones de Electron y Tauri.
- Se añadió [Catálogo de herramientas y capacidades](catalogo-herramientas-y-capacidades.md), que organiza las capacidades por familia, requisitos, fallos y verificación de éxito.
- Las cuotas y precios se registraron como instantánea documental, no como promesa de permanencia; se deben revisar antes de implementar.
- No se ejecutaron benchmarks comparativos de modelos ni pruebas de interfaz; las recomendaciones se basan en documentación oficial y criterios técnicos.


## Registro de esta actualización — 29 de octubre de 2026 (criterios para menores)

- Se estableció una regla transversal: cada alternativa debe incluir qué hace, ventajas, desventajas, coste total, privacidad, seguridad, facilidad, evidencia y recomendación específica para menores.
- Se añadieron evaluaciones orientadas a principiantes y menores a los documentos de modelos, UI/HUD y herramientas.
- La recomendación debe priorizar privacidad, permisos mínimos, control humano, coste predecible y aprendizaje; no se debe asumir que gratis, local o popular equivale automáticamente a seguro.
- Se incorporaron fuentes de UNICEF, UNESCO y FTC; la referencia FTC/COPPA es estadounidense y no debe presentarse como ley colombiana.
