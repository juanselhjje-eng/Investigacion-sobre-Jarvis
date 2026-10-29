

## Comparación inicial: qué conviene aprender de cada proyecto

| Tema | FatihMakes MARK | Danny el Arquitecto JARVIS | Decisión recomendada para M.I.L.O. |
|---|---|---|---|
| Ejecución en PC | Código Python público para automatización local | Producto web con experiencia en navegador y copiloto web anunciados | Construir núcleo local para acciones del sistema; separar funciones web |
| Voz | Gemini Live y cadena alternativa STT/LLM/TTS | Voz integrada anunciada; implementación exacta no auditada | Abstraer STT/TTS y mostrar proveedor/privacidad |
| Visión | Captura de pantalla y cámara en el código consultado | No confirmada como control de escritorio local desde las páginas consultadas | Implementar captura explícita, con permiso y selección de monitor |
| Memoria | Archivo JSON local y recuperación bajo demanda en el código consultado | Memoria entre sesiones y semántica anunciadas; backend interno no verificado | Memoria editable, fuentes, fecha, borrado y búsqueda híbrida |
| Agentes | Planificación multi-paso anunciada; propuesta comunitaria para subagentes | Mission Control anuncia salas, tareas y equipo de agentes | Primero tareas individuales trazables; luego subagentes |
| Seguridad | Confirmación desde UI, deshacer limitado y estados verificables anunciados/codificados | Aprobación humana descrita en guía; implementación no auditada | Confirmación fuera del modelo, permisos mínimos y resultados verificables |
| Extensibilidad | Acciones/plugins Python auto-descubiertos | Integraciones del producto no verificables sin código | Plugins con permisos y revisión; no ejecutar plugins desconocidos sin control |
| Persistencia | Archivos locales; deshacer no persistente en el módulo revisado | Sincronización entre equipos anunciada | Guardar tareas y resultados en almacenamiento explícito con política de datos |
| Transparencia | Código, issues y licencia consultables | Páginas públicas y avisos legales; backend no inspeccionado | Documentar qué se probó y qué solo se anuncia |

## Prioridades de implementación para M.I.L.O.

**P0 — fiabilidad y seguridad**
- Estados de tarea persistentes y resultado verificado.
- Registro de acciones con timeout, error y evidencia.
- Confirmación independiente para acciones sensibles.
- Control de permisos y protección de claves.
- Pruebas de audio y diagnóstico de dependencias.

**P1 — capacidades útiles**
- Voz interrumpible, proveedor intercambiable y modo local.
- Memoria editable con búsqueda por palabras y semántica.
- Navegador, archivos y visión con límites de seguridad.
- Deshacer para acciones reversibles y backups para archivos.

**P2 — autonomía coordinada**
- Planificador multi-paso con dependencias, cancelación y reintentos limitados.
- Subagentes especializados con contexto y presupuesto acotados.
- Panel de tareas y salas solo cuando el backend refleje estados reales.
- Métricas de latencia, tasa de éxito y coste por tarea.

**Conclusión:** no copiar el diseño visual como si fuera el sistema. La parte más valiosa para M.I.L.O. es combinar las herramientas locales y el registro explícito de resultados de MARK con la organización de tareas y supervisión de Mission Control, sin presentar demos o porcentajes no comprobados como ejecución real.
