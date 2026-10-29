# Catálogo de herramientas y capacidades de asistentes tipo JARVIS

**Fecha de corte:** 29 de octubre de 2026.  
**Objetivo:** catalogar qué puede hacer un asistente por medio de herramientas, cómo se implementan y qué evidencia se necesita para afirmar que funciona.

## Principio clave

Un modelo de lenguaje no controla por sí solo el PC. El sistema recibe una petición, selecciona una herramienta, valida argumentos, ejecuta código autorizado, recoge el resultado y decide si necesita otro paso. Las capacidades reales dependen del modelo, las herramientas instaladas, los permisos, el sistema operativo y los servicios externos.

Decir “puede manejar el ordenador” es demasiado impreciso. Hay que indicar si abre una aplicación, opera la interfaz por coordenadas, usa una API, modifica archivos o solo explica cómo hacerlo.

## 1. Taxonomía de herramientas

| Familia | Acciones posibles | Requisitos típicos | Fallos comunes | Verificación de éxito |
|---|---|---|---|---|
| Navegador | Buscar, abrir páginas, rellenar formularios, descargar, extraer información | Playwright/CDP/extensión/API o control visual | Cambios de DOM, CAPTCHA, sesión caducada, contenido hostil | URL final, estado DOM, archivo descargado o confirmación del sitio |
| Archivos | Buscar, leer, resumir, crear, editar, mover y borrar | API del sistema, rutas permitidas y permisos | Ruta incorrecta, codificación, archivo bloqueado, edición parcial | Comprobar contenido, ruta y checksum/diff |
| Aplicaciones de escritorio | Abrir apps, activar ventanas, atajos, automatizar UI | API del SO, atajos o automatización visual | Foco incorrecto, ventanas modales, cambios de versión | Detectar ventana/proceso y verificar resultado |
| Terminal | Ejecutar herramientas de desarrollo y scripts | Lista permitida, argumentos validados, sandbox | Inyección de comandos, directorio equivocado, proceso colgado | Código de salida, logs y archivos resultantes |
| Voz | STT, VAD, wake word, TTS, streaming, interrupción | Micrófono, motor local/API, permisos | Ruido, transcripción errónea, eco, latencia | Transcripción y estado de reproducción |
| Visión/OCR | Captura de pantalla, detección de elementos, OCR, lectura de ventanas | Permisos de captura, modelo visual/OCR | Escala DPI, múltiples monitores, texto pequeño, elementos dinámicos | Comparar captura antes/después y validar el objetivo |
| Mensajería | Redactar/enviar mensajes, leer canales, notificaciones | API oficial, OAuth, permisos y consentimiento | Token vencido, límites, destinatario incorrecto | ID/estado de envío y confirmación explícita |
| Correo/calendario | Buscar, resumir, crear borradores, eventos, recordatorios | API de proveedor y OAuth | Zona horaria, duplicados, invitado equivocado | Consultar evento/borrador creado y mostrar resumen |
| Sistema operativo | Volumen, brillo, energía, ajustes, procesos | API específica del SO y permisos | Diferencias entre Windows/macOS/Linux, permisos elevados | Leer de nuevo el valor actual |
| Búsqueda web | Recuperar fuentes y responder con citas | API de búsqueda o navegación | Resultados desactualizados, SEO spam, citas no relevantes | Abrir fuente original y asociar afirmación con evidencia |
| Base de datos | Consultar, insertar, actualizar, migrar | Driver/API, esquema y credenciales | SQL incorrecto, migración parcial, datos duplicados | Transacción, consultas de comprobación y backup |
| Memoria/RAG | Guardar preferencias, indexar documentos, recuperar pasajes | SQLite/Postgres/vector DB/embeddings | Datos viejos, duplicados, baja recuperación, borrado incompleto | Mostrar fuente/fecha y probar corrección/borrado |
| Herramientas MCP | Conectar herramientas externas mediante un protocolo común | Cliente/servidor MCP, autenticación y permisos | Esquema erróneo, servidor caído, exceso de privilegios | Validar respuesta, permisos y logs |
| Tareas programadas | Recordatorios, cron, tareas repetidas | Scheduler, proceso persistente y almacenamiento | Duplicados, zona horaria, reinicio, tareas perdidas | Registro de ejecución y estado durable |
| Subagentes | Delegar investigación, código o tareas paralelas | Orquestador, cuotas, contexto y permisos | Coste multiplicado, resultados contradictorios, fugas entre contextos | Revisión central, presupuesto y salida verificable |
| Control del hogar | Luces, escenas, sensores, termostatos | Home Assistant u otra plataforma y dispositivos | Estado desincronizado, dispositivo desconectado | Leer estado real después del comando |
| Audio/medios | Música, volumen, reproductores y dispositivos | API del reproductor, sistema de audio o servicios | Sesión expirada, dispositivo equivocado, API cambiada | Estado del reproductor y dispositivo activo |
| Desarrollo | Leer repositorios, editar código, ejecutar tests, abrir PRs | Git, CLI, entorno aislado y permisos | Cambios incorrectos, tests insuficientes, secretos en logs | Diff revisado, pruebas, build y commit/PR confirmado |
| Monitorización | CPU, RAM, red, errores, salud de servicios | API del sistema/hosting y observabilidad | Métricas ausentes, datos atrasados, falso estado saludable | Consultar fuente y marca temporal |
| Automatización web empresarial | Formularios, CRM, paneles internos | APIs oficiales o RPA y autorización | Cambio de UI, doble envío, acceso no autorizado | Identificador de transacción y verificación en el sistema |

## 2. Niveles de autonomía

1. **Nivel 0 — responder:** solo genera texto.
2. **Nivel 1 — sugerir:** explica los pasos, pero el usuario los ejecuta.
3. **Nivel 2 — acción puntual:** ejecuta una herramienta tras una petición clara.
4. **Nivel 3 — flujo de varios pasos:** planifica y encadena herramientas, con límites y verificaciones.
5. **Nivel 4 — tareas persistentes:** agenda, espera eventos, recupera estado y notifica resultados.
6. **Nivel 5 — coordinación de agentes:** divide trabajo y delega a subagentes con permisos separados.

Un nivel superior no significa mejor calidad. Aumenta la complejidad y los riesgos de duplicación, gasto, errores en cadena y acciones no deseadas. La autonomía debe ajustarse al riesgo de cada herramienta, no habilitarse globalmente.

## 3. Controles mínimos para cada herramienta

Cada herramienta debe declarar:
- nombre y propósito;
- esquema de parámetros validado;
- permisos y rutas permitidas;
- si puede cambiar datos externos;
- nivel de riesgo;
- timeout y límite de tamaño;
- número máximo de reintentos;
- mecanismo de cancelación;
- registro de entrada/salida con secretos redactados;
- condición de éxito verificable;
- estrategia de recuperación y posible reversión.

Las acciones se deben clasificar:
- **Lectura:** buscar, consultar, resumir.
- **Cambio reversible:** crear borrador, modificar archivo de prueba, cambiar ajuste recuperable.
- **Acción externa:** enviar mensaje, publicar, crear evento compartido.
- **Acción destructiva o sensible:** borrar, ejecutar código con privilegios, cambiar seguridad, transferir dinero o modificar datos críticos.

Las dos últimas categorías requieren controles más estrictos y, en muchos casos, aprobación humana explícita.

## 4. APIs frente a control visual

### API directa
Ventajas: contratos más estables, respuestas estructuradas, menor fragilidad que coordenadas, mejores registros.  
Limitaciones: requiere acceso oficial, autenticación, cuotas y permisos; no todas las apps tienen API.

### Automatización del navegador
Ventajas: trabaja con interfaces web reales y puede cubrir flujos que no ofrecen API pública.  
Limitaciones: cambia el DOM, necesita sesiones, puede leer contenido malicioso y no siempre detecta un éxito real.

### Control visual del escritorio
Ventajas: puede operar programas sin API mediante captura y entrada de teclado/ratón.  
Limitaciones: sensible a DPI, posición de ventanas, latencia, diálogos y múltiples monitores; debe validar visualmente después de cada paso importante.

### Híbrido
Usar API cuando exista, DOM para sitios web y visión/ratón solo para casos que lo requieran. Es una estrategia técnica razonable, pero debe confirmarse por tarea y aplicación.

## 5. Memoria y recuperación

La memoria no es solo guardar una conversación. Conviene separar:
- memoria de sesión;
- preferencias explícitas;
- hechos con fecha y fuente;
- memoria episódica de tareas realizadas;
- índices de documentos;
- cachés temporales;
- trazas de herramientas.

Debe haber mecanismos para corregir contradicciones, expirar datos temporales, mostrar de dónde salió un recuerdo y borrar todos los derivados. Un dato antiguo no debe prevalecer automáticamente sobre una corrección reciente.

## 6. Cómo comprobar si una capacidad es real

Para cada afirmación de un proyecto, registrar:
1. enlace a la implementación o documentación;
2. versión exacta;
3. requisitos del sistema;
4. pasos para ejecutar la función;
5. resultado esperado;
6. resultado observado y logs;
7. límites y fallos;
8. estado de la evidencia: anunciada, código visible, issue reportado, reproducida o no verificable.

Ejemplo: “envía correo” solo se considera verificado si se prueba con una cuenta de ensayo, se confirma el mensaje enviado y se revisa el destinatario. Que el modelo genere una frase diciendo “enviado” no es evidencia.

## 7. Protocolo de pruebas

- Preparar archivos, cuentas y páginas de prueba.
- Ejecutar la misma tarea varias veces.
- Introducir fallos de red y herramientas de forma controlada.
- Verificar cancelación y evitar duplicados.
- Medir éxito, latencia, coste, pasos manuales y acciones equivocadas.
- Revisar los permisos reales y los datos enviados fuera del equipo.
- Probar que las restricciones de herramientas se aplican a todos los agentes y rutas.
- Repetir después de cada actualización.

## 8. Fuentes de referencia

- OpenClaw: https://docs.openclaw.ai/
- Hermes Agent: https://hermes-agent.nousresearch.com/docs/user-guide/features/overview
- Goose: https://goose-docs.ai/
- Jan Agent: https://docs.jan.ai/agent
- OpenJarvis: https://github.com/open-jarvis/OpenJarvis
- Home Assistant Assist: https://www.home-assistant.io/voice_control/voice_remote_local_assistant
- MCP specification: https://modelcontextprotocol.io/
- Electron security: https://www.electronjs.org/docs/latest/tutorial/security
- Tauri security: https://tauri.app/es/security/

## Límites

Este catálogo clasifica capacidades posibles y controles de diseño; no significa que todos los asistentes citados implementen todas las familias de herramientas. La disponibilidad concreta debe verificarse por versión, configuración, licencia, sistema operativo y conectores habilitados.


## 9. Ventajas, desventajas y prioridades para un menor de edad

| Familia de herramientas | Ventajas | Desventajas o riesgos | Prioridad recomendada para aprender |
|---|---|---|---|
| Búsqueda web con fuentes | Enseña investigación y permite comprobar afirmaciones | Desinformación, páginas falsas, instrucciones maliciosas y fuentes desactualizadas | Alta: empezar por lectura, comparación de fuentes y citas verificables |
| Lectura y resumen de archivos de prueba | Útil para estudiar, resumir y organizar | Puede subir archivos privados a servicios remotos o resumir mal | Alta: usar copias no sensibles, indicar si el archivo se procesa localmente o en la nube |
| Creación de borradores | Ayuda a preparar textos, correos o tareas sin enviarlos | Puede inventar datos o producir contenido que el usuario no comprende | Alta: mantener en borrador y revisar antes de usar; para tareas escolares, usarlo para aprender y comprobar, no sustituir el trabajo propio |
| Voz con botón | Interacción accesible y práctica | Transcripción errónea, audio enviado a la nube o activación accidental | Media-alta: push-to-talk, indicador de micrófono y sin grabación persistente por defecto |
| Control visual de aplicaciones | Puede automatizar tareas sin API | Clic equivocado, foco de ventana incorrecto y fragilidad ante cambios de interfaz | Media: probar primero en aplicaciones y cuentas de ensayo, con pasos visibles y cancelación |
| Automatización de navegador | Repite formularios y tareas web | Prompt injection, sesiones abiertas, formularios enviados por error y cambios de interfaz | Media-baja al principio: solo sitios conocidos y tareas reversibles; aprobación antes de enviar o publicar |
| Sistema de archivos | Organiza y crea archivos de forma útil | Borrado o sobrescritura accidental, acceso a carpetas privadas | Media: limitar a una carpeta de pruebas, hacer copias y bloquear borrado permanente sin aprobación |
| Terminal y ejecución de código | Muy útil para aprender programación y pruebas | Puede ejecutar órdenes destructivas, instalar malware o exponer secretos si tiene permisos excesivos | Baja para automatización autónoma inicial: usar un entorno aislado, lista de comandos permitidos y supervisión adulta cuando haya instalación o cambios del sistema |
| Correo, mensajería y redes | Ahorra tiempo redactando y organizando comunicaciones | Puede enviar a la persona equivocada, filtrar datos o publicar sin querer | Solo borradores al principio; envío/publicación requiere confirmación que muestre destinatario, contenido y adjuntos |
| Calendario y recordatorios | Organiza tareas y estudio | Horarios equivocados, invitaciones a terceros y datos personales en la nube | Media: empezar con recordatorios locales y revisar zona horaria y sincronización |
| Memoria persistente | Evita repetir contexto y permite personalizar | Puede conservar datos sensibles, recordar algo incorrecto o no borrar todas las copias | Media: guardar solo preferencias no sensibles, con fuente, fecha, corrección y borrado accesible |
| Base de datos | Enseña estructura, persistencia y aplicaciones reales | Exposición de credenciales, pérdida de datos y costes de hosting | Media para proyectos de aprendizaje: usar datos ficticios, copias de seguridad y credenciales limitadas |
| MCP y conectores | Facilitan integrar herramientas de varios proveedores | Un servidor malicioso o con demasiados permisos puede acceder a datos o ejecutar acciones | Baja al principio: instalar solo conectores necesarios y conocidos, revisar permisos y probar con datos ficticios |
| Subagentes | Permiten dividir una investigación grande | Multiplican coste, errores, accesos y dificultad de supervisión | Baja al principio: primero dominar un solo agente; después limitar tareas, permisos, presupuesto y contexto de cada subagente |
| Tareas programadas | Automatizan rutinas y avisos | Acciones repetidas, procesos que siguen activos y notificaciones no deseadas | Media: recordatorios simples primero; tareas externas con límite de frecuencia y botón de apagado |
| Domótica y dispositivos físicos | Aporta accesibilidad y automatización doméstica | Puede afectar seguridad del hogar o a otras personas | Baja sin supervisión: comenzar con simulaciones o luces de prueba; no permitir acciones de riesgo ni cambios que afecten a otros sin autorización |
| Monitorización del PC | Ayuda a entender rendimiento y errores | Puede registrar actividad privada o exponer telemetría | Media: medir solo lo necesario y no recolectar contenido personal de otros usuarios del equipo |

### Escala de riesgo recomendada

- **Verde — lectura y práctica:** búsqueda, preguntas, resúmenes de material no sensible y archivos de prueba.
- **Amarillo — revisión antes de usar:** editar archivos, automatizar el navegador, instalar dependencias, usar memoria persistente o conectores.
- **Rojo — no dejar autónomo:** enviar mensajes, publicar, borrar datos, comprar, cambiar seguridad del sistema, ejecutar comandos privilegiados o controlar dispositivos que puedan afectar a personas. Requiere autorización y confirmación explícita; algunas acciones no deben habilitarse en absoluto.

### Recomendación general

Para un menor que está aprendiendo, el mejor orden es: **leer → proponer → crear borradores → actuar en un entorno de prueba → automatizar tareas reversibles → integrar cuentas reales solo cuando sea necesario**. No dar al agente acceso completo al PC, correo, contraseñas o archivos personales solo para que parezca más avanzado. La potencia debe crecer después de comprobar la fiabilidad, no antes.

Referencia de protección infantil y privacidad: https://www.unicef.org/innocenti/reports/policy-guidance-ai-children
