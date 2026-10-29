# Auditoría profunda: seguridad, fiabilidad y fallos operativos en agentes personales

**Fecha de revisión:** 29 de octubre de 2026.  
**Tipo de documento:** revisión documental de fuentes públicas. No es una auditoría de seguridad independiente ni una reproducción local de los fallos.

## Conclusión principal

Los asistentes más capaces ya no son solo una voz que abre aplicaciones: algunos leen archivos, usan navegador y terminal, conectan mensajería, ejecutan tareas programadas y llaman herramientas externas. Eso aumenta su utilidad, pero también amplía la superficie de error. La diferencia entre una demo y un agente fiable se ve en los permisos efectivos, la gestión de contenido no confiable, la recuperación tras errores, el uso de memoria a largo plazo y el comportamiento durante muchas horas de ejecución.

No debe confundirse:
- **Capacidad anunciada** con capacidad probada en un entorno independiente.
- **Reporte de issue** con vulnerabilidad confirmada o defecto universal.
- **Reporte cerrado** con solución demostrada en todas las versiones.
- **Modo local** con privacidad absoluta: conectores, proveedores remotos y telemetría pueden cambiar el flujo de datos.
- **Confirmación en pantalla** con verificación de que la acción final tuvo el efecto esperado.

## 1. OpenClaw: permisos, prompt injection y ecosistema de extensiones

### Lo que documenta el propio proyecto

La documentación oficial divide la seguridad en controles de acceso a canales, autenticación del gateway, perfiles de herramientas, aislamiento, permisos de ejecución, control del navegador y tratamiento de contenido externo. Recomienda mantener las herramientas de alto riesgo restringidas y no asumir que una conversación privada elimina los riesgos: el agente puede leer páginas, correos, archivos o resultados web que contengan instrucciones hostiles.

Fuentes:
- https://docs.openclaw.ai/security
- https://docs.openclaw.ai/gateway/security/prompt-injection
- https://openclaw.ai/security

### Dato importante del panel público de seguridad

La página de seguridad revisada el 9 de septiembre de 2026 indicaba 1.799 reportes recibidos desde enero de 2026; 58% figuraban como cerrados sin aviso de seguridad por ser inválidos, duplicados o por diseño; 722 correcciones publicadas y 39 con CVE, con 14 críticas confirmadas corregidas y divulgadas. Son cifras declaradas por el proyecto y pueden cambiar. **Un reporte recibido no equivale a una vulnerabilidad confirmada.** La página también excluye de su alcance algunas superficies de terceros, como skills de ClawHub.

La distribución de correcciones citada por el proyecto muestra que el riesgo no se limita al modelo: gateway/autenticación/alcances, ejecución/sandbox/aprobaciones, conectores de chat, navegador, plugins, archivos, MCP y tráfico de red aparecen como áreas distintas.

### Riesgo técnico clave: prompt injection

Una página o documento leído por el agente puede intentar que ignore instrucciones, revele datos o ejecute una acción. El modelo no debe ser la única barrera. Controles más robustos incluyen:
- permisos mínimos por agente y herramienta;
- sandbox activado para tareas que ejecuten código;
- aprobación humana para acciones destructivas o externas;
- allowlists de canales, usuarios y comandos;
- secretos fuera del contexto del modelo y de archivos accesibles sin necesidad;
- separar lectura/resumen de contenido no confiable de la ejecución privilegiada;
- comprobar el resultado real de la acción antes de declarar éxito.

La documentación también advierte que las defensas de los modelos no resuelven el problema por completo y que modelos más pequeños o antiguos pueden ser más susceptibles. Las cifras de benchmarks que aparecen en la documentación deben interpretarse con su metodología y escenario exactos, no como garantía universal.

## 2. Hermes Agent: memoria, límites de herramientas y estabilidad de procesos

### Controles declarados por la documentación

La documentación de seguridad enumera autorización de usuarios, aprobación de comandos peligrosos, seguridad de escritura de archivos, aislamiento de contenedores, filtrado de credenciales para MCP, análisis de archivos de contexto, aislamiento entre sesiones y validación de rutas.

Fuente: https://hermes-agent.nousresearch.com/docs/user-guide/security

### Reportes públicos representativos

| Reporte | Qué describió la persona que lo abrió | Por qué importa | Estado de evidencia en esta investigación |
|---|---|---|---|
| https://github.com/NousResearch/hermes-agent/issues/49386 | Herramientas de proveedores de memoria aparecían después del filtrado pese a configurar `disabled_toolsets=["memory"]` | Una restricción declarada debe aplicarse a todas las rutas de ensamblaje de herramientas | Reporte de seguridad; no se afirma aquí que siga presente en la versión actual |
| https://github.com/NousResearch/hermes-agent/issues/80527 | El proceso del dashboard creció hasta varios GB durante sesiones con herramientas y fue terminado por el sistema | Un proceso de larga duración puede afectar a todos los clientes conectados cuando agota memoria | Reporte de usuario con medidas descritas; sin reproducción independiente aquí |
| https://github.com/NousResearch/hermes-agent/issues/103777 | Un dashboard continuó intentando leer logs de un perfil borrado y acumuló memoria hasta degradarse | El ciclo de vida de perfiles y los errores repetidos deben manejarse sin bucles de crecimiento | Issue abierto según la página consultada; puede cambiar |
| https://github.com/NousResearch/hermes-agent/issues/70684 | Una sesión CLI prolongada acumuló memoria durante resultados grandes de herramientas y compresión de contexto | Los resultados de navegador, terminal y archivos pueden ser grandes; la limpieza de contexto no garantiza liberar memoria del proceso | Reporte/análisis de la persona que abrió el issue, no benchmark independiente |
| https://github.com/NousResearch/hermes-agent/issues/83494 | Un usuario reportó acciones incorrectas/destructivas después de fallos de herramientas pese a disponer de documentación y memoria | El agente debe detenerse, verificar y pedir ayuda después de fallos repetidos, no improvisar indefinidamente | Relato de incidente del usuario; no generalizable a todos los despliegues |

Estos ejemplos no demuestran que Hermes sea inseguro en todos los usos. Demuestran por qué una evaluación seria debe revisar las versiones afectadas, el estado de cada issue, la corrección posterior y si existen pruebas de regresión.

### Pruebas que se deberían exigir

1. Confirmar que una herramienta desactivada no reaparece por proveedores, plugins o subagentes.
2. Ejecutar sesiones largas con resultados de herramientas grandes y medir RAM de forma periódica.
3. Borrar perfiles durante la ejecución y comprobar que no hay bucles ni acceso residual a sus datos.
4. Forzar fallos de herramienta y verificar que el agente detiene la cadena cuando la acción ya no es segura.
5. Verificar que la memoria de un perfil no se mezcla con otro y que la opción de desactivarla se aplica a todas las rutas.

## 3. Goose: problemas de instalación, secretos y extensiones

### Lo que declara el proyecto

Goose ofrece aplicación de escritorio, CLI y API; conectores por MCP; recetas; subagentes; controles de permisos, detección de prompt injection y modo sandbox. La documentación anuncia soporte para múltiples proveedores y sistemas operativos.

Fuentes:
- https://goose-docs.ai/
- https://goose-docs.ai/docs/troubleshooting/known-issues/
- https://github.com/aaif-goose/goose/

### Problemas documentados

La guía oficial de problemas conocidos explica que el guardado de credenciales puede fallar cuando no existe un keyring/DBus disponible, especialmente en entornos headless o contenedores. También documenta fallos de extensiones MCP en redes sin acceso a internet, porque pueden necesitar descargar runtimes o dependencias.

Un issue de Goose describió un crash al cargar sesiones con proveedores personalizados y extensiones MCP en versiones 1.42 a 1.46, mientras 1.40 y 1.41 funcionaban según el reporte:
- https://github.com/aaif-goose/goose/issues/11370

La lección no es que toda instalación falle, sino que una matriz de compatibilidad debe probar cada combinación de versión, proveedor, extensiones, sistema operativo y conectividad. La autenticación y el arranque de extensiones deben diagnosticarse por separado del modelo.

## 4. Jan: privacidad declarada, límites de MCP y madurez del agente

### Separación importante: local frente a nube

Jan permite modelos locales y proveedores en la nube. La documentación aclara que un modelo en la nube recibe los mensajes necesarios para generar respuestas; por tanto, “usar Jan” no implica que todos los datos permanezcan en el dispositivo.

Fuentes:
- https://docs.jan.ai/desktop/quickstart
- https://docs.jan.ai/desktop/privacy
- https://docs.jan.ai/desktop/api-server

Jan declara que las analíticas son opcionales y describe qué datos dice recopilar cuando el usuario las habilita. Eso es una política declarada por el producto, no una auditoría externa del tráfico de red.

### MCP: permiso por llamada y modo de aprobación total

La documentación explica que las llamadas MCP pueden exponer datos o ejecutar acciones no deseadas. De forma predeterminada, el usuario puede revisar argumentos y aprobar o denegar llamadas; existe también una opción para aprobar todas las herramientas MCP. Activar ese modo global cambia materialmente el riesgo.

Fuentes:
- https://docs.jan.ai/desktop/mcp
- https://docs.jan.ai/desktop/integrations/mcp-servers

### Desktop y CLI no comparten necesariamente todo

La documentación de Jan Agent indica que es una vista previa y que algunos almacenes de memoria y espacios de trabajo del agente de escritorio y del agente CLI son independientes por defecto. También distingue la ejecución con aprobación de la ejecución autoaprobada, que amplía el riesgo si se le da acceso a archivos o comandos.

Fuente: https://docs.jan.ai/agent

### Ejemplos recientes del rastreador de incidencias

Una página de issues consultada mostraba reportes recientes relacionados con mensajes de error del servidor API local, firma del desinstalador de Windows, rutas de modelos en respuestas API, cambios de hosts confiables que no se guardan y búsqueda de documentos que cae a una búsqueda lineal cuando no carga sqlite-vec:
- https://github.com/janhq/jan/issues

La lista de issues es dinámica y algunos reportes pueden ser corregidos después de esta fecha. Debe registrarse versión y estado antes de tratar cualquier problema como vigente.

## 5. Matriz comparativa de controles

| Control | OpenClaw | Hermes Agent | Goose | Jan |
|---|---|---|---|---|
| Permisos/confirmación | Perfiles, allowlists y aprobaciones documentados | Aprobación de comandos peligrosos y autorización | Permisos de herramientas y sandbox declarados | Aprobación de herramientas MCP; existe modo de aprobar todas |
| Aislamiento | Sandbox y perfiles por agente documentados | Docker/Modal y aislamiento de sesiones documentados | Modo sandbox documentado | Cowork puede usar sandbox; Jan Agent tiene modos de aprobación |
| Riesgo de contenido externo | Prompt injection tratado explícitamente en docs | Escaneo de contexto y controles declarados | Detección de prompt injection declarada | Documentación MCP advierte sobre prompt injection |
| Evidencia pública de fallos consultada | Panel de seguridad con reportes, correcciones y CVE | Issues de filtrado de herramientas y crecimiento de memoria | Issues de crash; guía de problemas de keyring/extensiones | Issues de servidor local, instalación y búsqueda |
| Lo que falta verificar | Configuración efectiva y versión instalada | Estado final de cada issue y pruebas de regresión | Reproducción por SO/proveedor/extensiones | Tráfico de red, estado de issues y diferencias entre Desktop/CLI |

La tabla compara documentación disponible, no una puntuación absoluta de seguridad.

## 6. Protocolo reproducible que falta ejecutar

No se ha ejecutado aún un banco de pruebas práctico común. Para una futura comparación se debe registrar cada prueba con versión exacta, hardware, sistema operativo, configuración, modelo, pasos, logs y resultado.

### A. Instalación limpia
- Instalar en una máquina o entorno limpio siguiendo solo la documentación oficial.
- Registrar pasos manuales, dependencias faltantes, tiempo, errores y consumo.
- Desinstalar y comprobar si quedan procesos, servicios o datos.

### B. Voz y percepción
- Medir latencia desde la voz hasta el primer audio de respuesta.
- Probar ruido, acentos, cambio de micrófono, interrupción y activaciones falsas.
- Verificar qué audio se envía a proveedores externos.

### C. Memoria
- Guardar un hecho, corregirlo, marcarlo como temporal y pedir su eliminación.
- Preguntar después de reiniciar y desde otra sesión.
- Verificar si recupera el dato actualizado, cita la fuente y respeta el borrado.
- Probar aislamiento entre perfiles.

### D. Herramientas y recuperación
- Dar una tarea inocua que cree o modifique un archivo de prueba.
- Provocar un fallo controlado y comprobar que informa del fallo en vez de decir “listo”.
- Interrumpir una tarea y verificar cancelación, reintento y duplicación de acciones.
- Probar confirmaciones y permisos mínimos.

### E. Seguridad
- Usar únicamente archivos de prueba con instrucciones adversariales no peligrosas que pidan ignorar el objetivo o leer un archivo señuelo.
- Comprobar que el agente trata el contenido como no confiable y no lee archivos fuera del alcance permitido.
- Verificar que una herramienta desactivada realmente no se puede invocar por otra ruta.
- No usar secretos reales ni ejecutar pruebas destructivas.

### F. Resistencia prolongada
- Ejecutar un guion fijo de tareas de 2–8 horas en condiciones controladas.
- Medir RAM, CPU, tokens, fallos, latencia, número de herramientas y recuperación tras reinicio.
- Repetir al menos tres veces y comparar medianas; registrar variación, no solo el mejor resultado.

## 7. Preguntas que siguen abiertas

1. ¿Cuántos reportes se resolvieron y en qué versión exacta?
2. ¿Qué regresiones aparecen tras actualizar?
3. ¿Qué controles de seguridad están activados por defecto y cuáles requieren configuración manual?
4. ¿Qué datos salen del dispositivo con cada proveedor y con cada extensión?
5. ¿Se puede exportar y borrar completamente memoria, logs, cachés, índices y trazas?
6. ¿Qué ocurre cuando un subagente hereda herramientas o memoria de otro perfil?
7. ¿Los benchmarks miden tareas reales de extremo a extremo o solo llamadas aisladas?
8. ¿Qué tasa de éxito se obtiene en tareas repetidas, no en una sola demostración?

## Fuentes primarias consultadas

- OpenClaw Security: https://openclaw.ai/security
- OpenClaw security docs: https://docs.openclaw.ai/security
- OpenClaw prompt injection: https://docs.openclaw.ai/gateway/security/prompt-injection
- Hermes security: https://hermes-agent.nousresearch.com/docs/user-guide/security
- Hermes issue #49386: https://github.com/NousResearch/hermes-agent/issues/49386
- Hermes issue #80527: https://github.com/NousResearch/hermes-agent/issues/80527
- Hermes issue #103777: https://github.com/NousResearch/hermes-agent/issues/103777
- Hermes issue #70684: https://github.com/NousResearch/hermes-agent/issues/70684
- Hermes issue #83494: https://github.com/NousResearch/hermes-agent/issues/83494
- Goose known issues: https://goose-docs.ai/docs/troubleshooting/known-issues/
- Goose issue #11370: https://github.com/aaif-goose/goose/issues/11370
- Jan privacy: https://docs.jan.ai/desktop/privacy
- Jan MCP: https://docs.jan.ai/desktop/mcp
- Jan Agent overview: https://docs.jan.ai/agent
- Jan issues: https://github.com/janhq/jan/issues

## Límites de esta fase

- Se revisaron páginas públicas accesibles; no se probó el software localmente.
- No se afirma que todos los issues sigan abiertos o afecten a la última versión.
- Las cifras y descripciones de seguridad de los proyectos son declaraciones de sus mantenedores.
- La muestra se concentra en cuatro proyectos modernos; no representa todos los asistentes de internet.
- No se inspeccionaron redes privadas, cuentas cerradas ni todo el contenido de YouTube/Instagram.
