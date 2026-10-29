# FatihMakes — auditoría inicial de Mark LVI

**Estado:** primera pasada; no es auditoría completa ni prueba de ejecución.  
**Fuente primaria principal:** https://github.com/FatihMakes/Mark-LVI

## 1. Qué es

El README presenta MARK LVI como un asistente de escritorio multiplataforma, escrito en Python, con voz en tiempo real, visión, control del ordenador, memoria, interfaz gráfica y un sistema extensible por acciones y plugins. El README también declara uso personal y no comercial en versiones de la serie; se debe leer el archivo de licencia exacto de la versión que se quiera usar antes de reutilizar código.

**Clasificación:** afirmación del creador/documentación. No se ha probado de forma independiente cada capacidad.

## 2. Arquitectura que se puede observar

En `main.py` aparecen estas piezas:

- Python como proceso principal.
- `asyncio` y tareas concurrentes para mantener la sesión y manejar eventos.
- `sounddevice` y `numpy` para captura y procesamiento de audio.
- Cliente `google.genai` y tipos de Gemini.
- Interfaz `JarvisUI` importada desde `ui.py`.
- Módulo de memoria con carga, actualización, resumen de sesión y búsqueda.
- Descubrimiento de acciones mediante `core.action_loader`.
- Descubrimiento de plugins mediante `core.plugin_loader`.
- Captura de pantalla/cámara, monitor de sistema, comportamiento proactivo, búsquedas web, confirmación de acciones, deshacer, dispositivos de audio, detección de palabra de activación y protección contra eco.

El código declara que las acciones con archivos se auto-describen mediante un diccionario `TOOL` y un handler en archivos separados. Esto reduce la necesidad de editar el núcleo cada vez que se añade una habilidad.

El proceso configura el modelo en una constante inicial y comenta que la selección/fallback de modelos vive en `core/gemini.py`. También declara un límite de espera para turnos que no responden, porque una conexión puede quedar abierta pero dejar de entregar respuestas.

## 3. Capacidades que anuncia el README

### Voz y conversación
- Conversación de voz en tiempo real con Gemini Live.
- Alternativa configurable de reconocimiento de voz + modelo de lenguaje + voz.
- Push-to-talk, palabra de activación, detección del eco propio y selector de micrófono/altavoz.
- Síntesis local con Piper y reconocimiento local con Whisper, según la documentación.
- Continuidad de sesión, cambio de voz y selección de modelo alternativo cuando un proveedor falla o se agota una cuota.

### Interfaz
- HUD de escritorio con avatar animado, sincronización labial, forma de onda, selector de voz y temas.
- Panel para mostrar contenido, noticias y resultados.
- Controles de configuración y memoria.

### Acciones digitales
- Abrir y cerrar aplicaciones.
- Navegar y actuar en el navegador.
- Procesar archivos locales.
- Buscar en la web y consultar el tiempo.
- Enviar mensajes mediante integraciones anunciadas.
- Controlar ventanas, volumen, brillo, Wi-Fi y funciones de energía.
- Recordatorios del sistema y arranque automático.
- Monitoreo de hardware y alertas.
- Capturas de pantalla y cámara, si están habilitadas.
- Tareas de varios pasos en modo agente.
- Panel remoto desde el teléfono mediante emparejamiento QR.

### Memoria
- Memoria persistente local, resúmenes de sesión y recuperación por búsqueda.
- Panel para inspeccionar y eliminar recuerdos.
- El README dice que la memoria no tiene un límite fijo y que el contexto recupera lo relevante bajo demanda. Esto no significa que el modelo pueda recordar literalmente todo: la calidad depende de lo que se guarde, indexe, recupere y quepa en contexto.

### Extensión
- Acciones y plugins auto-descubiertos desde carpetas dedicadas.
- Proveedores de modelos configurables mediante un protocolo compatible con `/v1/chat/completions`, según el README.
- Instalación opcional de componentes de voz para evitar descargar dependencias grandes si no se usan.

## 4. Instalación y plataformas

El archivo `setup.py`:
- Identifica Windows, macOS o Linux.
- Indica Python 3.11 como mínimo y 3.13 como versión máxima probada, aunque permite continuar con versiones más nuevas mostrando una advertencia.
- Instala dependencias de `requirements.txt`.
- Intenta instalar navegadores de Playwright; si falla, advierte que la automatización del navegador no funcionará, pero permite continuar.
- Comprueba que exista `core/face_model.obj`.
- Muestra notas específicas para herramientas nativas de cada sistema operativo.

**Riesgo/limitación:** una dependencia nativa o un navegador puede instalarse de forma distinta según el sistema. El instalador tolera ciertos fallos para que la aplicación siga abriendo, lo que puede dejar funciones deshabilitadas si el usuario no lee los avisos. Recomendación para M.I.L.O.: al inicio, ejecutar un diagnóstico que muestre claramente qué módulos están listos, cuáles fallaron y qué pasos solucionan cada fallo.

## 5. Problemas reportados por usuarios

Estos son reportes públicos, no prueba de que el fallo ocurra a todos los usuarios ni de que siga presente en la versión más reciente.

### Micrófono, palabra de activación y suspensión
- [Issue #162 — Mic issues](https://github.com/FatihMakes/Mark-LVI/issues/162): un usuario informa que el micrófono pasa la prueba de configuración, pero la activación por voz falla con frecuencia.
- [Issue #134 — Activation Issue](https://github.com/FatihMakes/Mark-LVI/issues/134): se informa que, tras varias sesiones, el agente entra en reposo y no vuelve a despertar.
- [Issue #145 — Wont respond sometimes](https://github.com/FatihMakes/Mark-LVI/issues/145): reporte de suspensión automática y falta de respuesta.
- [Issue #160 — Jarvis looping start](https://github.com/FatihMakes/Mark-LVI/issues/160): registro de error con `sounddevice.PortAudioError` al abrir el flujo de entrada; el usuario describe un ciclo de despertar/pensar/dormir y problemas de entrada de audio.

**Causas posibles, aún no confirmadas:** selección incorrecta de dispositivo, permisos, dispositivo ocupado, frecuencia de muestreo no soportada, backend PortAudio/driver o una excepción en tareas concurrentes que reinicia el ciclo.

**Cómo investigarlo/arreglarlo en M.I.L.O.:**
1. Enumerar dispositivos de audio con nombre, índice y frecuencias admitidas.
2. Probar el micrófono con un flujo mínimo antes de iniciar el agente.
3. Separar estado de escucha, reposo, reconexión y error en una máquina de estados explícita.
4. No reiniciar silenciosamente después de una excepción; mostrar el error y guardar un registro con fecha.
5. Añadir recuperación con backoff y botón para reiniciar solo el subsistema de audio.
6. Probar desconectar/reconectar micrófonos y cambios de dispositivo mientras la app está abierta.

### Pantallas múltiples
- [Issue #154 — Multiple Monitor viewing](https://github.com/FatihMakes/Mark-LVI/issues/154): un usuario solicita que la visión pueda ver los otros monitores; indica que la versión que usa solo ve uno.

**Mejora para M.I.L.O.:** enumerar monitores, permitir elegir pantalla y mostrar una vista previa de la captura; pedir permiso antes de enviar capturas a un proveedor remoto. No asumir que una captura de una pantalla representa todo el escritorio.

### Selección de modelo y dependencias
- [Issue #146 — Can´t change model](https://github.com/FatihMakes/Mark-LVI/issues/146): reporte de que no se puede seleccionar el modelo desde el menú.
- [Issue #132 — sounddevice missing](https://github.com/FatihMakes/Mark-LVI/issues/132): reporte de `ModuleNotFoundError: No module named 'sounddevice'`.
- [Issue #149 — Wake word setup failed](https://github.com/FatihMakes/Mark-LVI/issues/149): reporte de conflicto de dependencias al instalar el detector de palabra de activación.
- [Issue #150 — download issue](https://github.com/FatihMakes/Mark-LVI/issues/150): reporte de descarga ZIP.

**Mejoras para M.I.L.O.:** pruebas de configuración automatizadas, versión y entorno virtual documentados, instalación opcional por módulos, selector de modelos que valide el proveedor antes de guardar, y errores accionables que incluyan el comando exacto de reparación.

## 6. Solicitudes e ideas de la comunidad

- [Issue #159 — Helm Improvement Proposals](https://github.com/FatihMakes/Mark-LVI/issues/159) propone soporte para clientes MCP, subagentes, embeddings para búsqueda semántica, seguimiento de tareas multi-paso, enrutamiento de modelos por tipo de tarea, trazas de ejecución y descripciones de herramientas más claras. Es una propuesta publicada, no evidencia de que todas esas funciones estén implementadas.
- [Issue #154](https://github.com/FatihMakes/Mark-LVI/issues/154) pide soporte para varios monitores.
- [Issue #151](https://github.com/FatihMakes/Mark-LVI/issues/151) pregunta por licencia comercial interna. Es un recordatorio de que la licencia debe aclararse antes de usar el proyecto en una actividad comercial.

Una issue puede estar abierta, cerrada o desactualizada; el estado actual debe revisarse antes de dar por resuelto un problema.

## 7. Seguridad: fortalezas anunciadas y cosas que aún deben auditarse

El README anuncia:
- Puerta de confirmación para acciones irreversibles.
- Capacidad de deshacer determinadas acciones.
- Resultados de herramientas con estados `done`, `unverified` y `failed`.
- Protección frente a instrucciones maliciosas contenidas en páginas web o documentos.
- Descripciones dinámicas de habilidades y límites para reducir afirmaciones falsas.

Son buenos patrones de diseño, pero el README por sí solo no prueba que la frontera de seguridad sea completa.

Auditoría pendiente:
- ¿Las herramientas validan argumentos en código, además de confiar en el modelo?
- ¿Las acciones peligrosas requieren confirmación fuera del texto del modelo?
- ¿Puede un plugin leer claves, memoria o archivos ajenos a su función?
- ¿La navegación web trata todo el contenido remoto como datos no confiables?
- ¿El registro oculta claves API, contraseñas y datos personales?
- ¿Deshacer funciona también tras un cierre inesperado?
- ¿Qué acciones no son reversibles y cómo se informa al usuario?
- ¿Se limita la ejecución de comandos, rutas y dominios?

## 8. Límites y riesgos de la propuesta

- La voz en tiempo real de Gemini requiere conectividad y está sujeta a cuotas, latencia y cambios del proveedor.
- Las opciones locales reducen dependencia de la nube, pero pueden consumir CPU/RAM y responder más lentamente según el hardware.
- Un asistente con permisos para manejar archivos, navegador, mensajería y sistema operativo puede causar daños por errores o instrucciones mal interpretadas.
- El control de escritorio varía por sistema operativo, permisos de accesibilidad y entorno gráfico.
- La memoria persistente puede guardar información incorrecta o sensible; necesita revisión, eliminación y límites de retención.
- El término “autónomo” no implica que todas las tareas se ejecuten sin supervisión ni que siempre se completen.
- No se han ejecutado pruebas independientes de las capacidades anunciadas en esta investigación.

## 9. Qué tomar como patrón para M.I.L.O.

Prioridad alta:
1. Separar núcleo conversacional, proveedores, herramientas, memoria y UI.
2. Definir un contrato común para herramientas y respuestas verificables.
3. Tener estados explícitos para audio, tareas, errores y reconexión.
4. Usar confirmación independiente del modelo y una política de permisos por herramienta.
5. Registrar cada acción con parámetros saneados, resultado observado y opción de deshacer cuando sea posible.
6. Diagnóstico inicial de dispositivos, claves, proveedores y permisos.
7. Pruebas de regresión para cada acción.
8. Plugins con permisos mínimos y límites claros.

Prioridad media:
- Enrutamiento de modelos por tarea.
- Búsqueda híbrida por palabras clave y embeddings.
- Subagentes con presupuestos de tiempo/tokens y salida validada.
- Soporte de varios monitores.
- Métricas de latencia y trazas locales.

## 10. Fuentes primarias revisadas

- README: https://github.com/FatihMakes/Mark-LVI
- Núcleo: https://github.com/FatihMakes/Mark-LVI/blob/main/main.py
- Instalación: https://github.com/FatihMakes/Mark-LVI/blob/main/setup.py
- Issues: enlaces integrados en cada sección.
