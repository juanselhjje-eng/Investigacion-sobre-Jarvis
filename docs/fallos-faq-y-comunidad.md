# Errores, límites, preguntas frecuentes y sugerencias de la comunidad

Síntesis inicial. Estos son patrones de investigación y ejemplos visibles en fuentes públicas, no estadísticas globales ni afirmaciones de que todos los asistentes tengan estos fallos.

## Reportes y solicitudes vistos en fuentes consultadas

| Categoría | Ejemplo | Interpretación correcta | Qué investigar |
|---|---|---|---|
| Instalación/dependencias | Issues de Leon sobre ZIP corrupto, puente Python ausente, compilación de llama.cpp y requisitos de GLIBC | Reportes concretos vinculados a versiones/entornos | SO, versión, logs, dependencias y solución |
| Conexión/arranque | Issues de Leon sobre servidor TCP y de OpenVoiceOS sobre posible carrera al iniciar | Problemas reportados; verificar vigencia | Logs, puertos, procesos y orden de servicios |
| Voz/atajos | Peticiones sobre push-to-talk, wake word y selección de dispositivos en proyectos de asistentes | Voz depende de hardware, SO y permisos | Micrófono, VAD, ruido, latencia e interrupción |
| Memoria | Solicitudes en Leon sobre memoria externa, importancia y olvido gradual | Guardar datos no garantiza recuperar lo correcto | Recuperación, duplicados, fuentes, caducidad y borrado |
| Modelos/proveedores | Solicitudes para Ollama, backends compatibles y búsqueda sin clave | Portabilidad no elimina diferencias entre APIs | Contratos, timeouts, fallback, coste y rate limits |
| Multiplataforma | Solicitudes de Windows/iOS y errores de bibliotecas por SO | Una función en un sistema no implica paridad en otro | Matriz de compatibilidad, CI y pruebas reales |
| Integraciones | Peticiones de búsqueda, memoria MCP y conectores | Cada integración añade permisos y puntos de fallo | OAuth, tokens, límites, revocación y errores |

Fuentes:
- https://github.com/leon-ai/leon/issues
- https://github.com/OpenVoiceOS/ovos-core/issues
- https://github.com/akshayaggarwal99/jarvis-ai-assistant/issues
- https://github.com/Avinashb722/jarvis-ai-assistant/issues

## FAQ que debe responder la investigación

### Instalación
- ¿Qué sistema operativo, versión de lenguaje y dependencias se necesitan?
- ¿Qué hardware requiere voz, visión o modelo local?
- ¿Cómo se diagnostica una instalación que se cierra o no arranca?
- ¿Cómo se actualiza y desinstala sin dejar servicios residuales?

### Modelos y coste
- ¿Necesita API key y qué datos envía al proveedor?
- ¿Funciona sin internet?
- ¿Qué ocurre si se agota la cuota, falla la red o el proveedor devuelve 429?
- ¿Cómo se comparan coste, latencia, calidad y RAM/VRAM?
- ¿Qué significa “gratis”: software, modelo, API, cómputo o alojamiento?

### Voz y percepción
- ¿Funciona con el micrófono elegido y en varios idiomas?
- ¿Se puede interrumpir mientras habla?
- ¿Cómo evita activaciones falsas y errores de transcripción?
- ¿Qué ocurre con varios monitores o al cambiar de dispositivo de audio?
- ¿La cámara/captura requiere permiso explícito?

### Memoria
- ¿Qué se guarda y dónde?
- ¿Cómo se revisa, corrige, exporta y elimina un recuerdo?
- ¿Distingue hechos, inferencias y datos temporales?
- ¿Cómo evita guardar secretos o repetir información vieja?
- ¿Puede mostrar de qué fuente recuperó un dato?

### Automatización
- ¿Ejecuta acciones reales o solo abre enlaces?
- ¿Comprueba que una acción terminó?
- ¿Qué ocurre si la tarea se interrumpe a mitad?
- ¿Puede cancelar, reintentar con límites y continuar tras reiniciar?
- ¿Qué acciones requieren aprobación humana?

### Privacidad y seguridad
- ¿Dónde se guardan claves y registros?
- ¿Puede una página maliciosa inducir al agente a revelar datos o actuar?
- ¿Los plugins tienen acceso total o permisos limitados?
- ¿Cómo se revocan integraciones y se borran datos?
- ¿La telemetría es opcional y requiere consentimiento?

## Límites estructurales a medir

1. Percepción imperfecta: transcripción, OCR y visión pueden confundir elementos.
2. Razonamiento probabilístico: el modelo puede interpretar mal el objetivo o inventar detalles.
3. Herramientas frágiles: interfaces, páginas y APIs cambian.
4. Memoria incompleta: datos guardados pueden no recuperarse, contradecirse o quedar obsoletos.
5. Autonomía acotada: planes largos acumulan errores; reintentos pueden duplicar acciones.
6. Latencia/disponibilidad: voz puede depender de red, CPU/GPU y dispositivos.
7. Privacidad: cada conector puede transmitir datos distintos; “local-first” debe verificarse.
8. Seguridad de ejecución: terminal, navegador y plugins pueden causar efectos externos.
9. Falsa sensación de éxito: decir “hecho” no demuestra que un archivo, correo o ajuste cambió.
10. Coste y mantenimiento: modelos, voz y procesos persistentes consumen recursos y necesitan actualizaciones.

## Líneas de mejora a verificar en solicitudes reales

- Más proveedores de modelos y modo local.
- Memoria externa, puntuación de importancia y olvido controlado.
- Compatibilidad entre sistemas operativos.
- Diagnóstico de dependencias más claro.
- Mejoras de wake word, push-to-talk y selección de dispositivos.
- Más herramientas y estándares de integración.
- Trazas, evaluación y fiabilidad de herramientas.
- Control visible de permisos, cancelación y estado.
- Documentación y pruebas reproducibles.

## Convertir un reporte en conocimiento útil

1. Abrir el reporte original y leer el contexto.
2. Registrar versión, fecha, sistema operativo y pasos.
3. Buscar duplicados y soluciones posteriores.
4. Clasificar como pregunta, solicitud, error reproducible o reporte no confirmado.
5. No inferir frecuencia a partir de un solo comentario.
6. Registrar si el mantenedor confirmó la solución.
