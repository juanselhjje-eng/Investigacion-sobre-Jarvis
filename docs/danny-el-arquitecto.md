# Danny el Arquitecto — investigación pública inicial de JARVIS

**Estado:** investigación del producto público, no auditoría de su implementación interna.  
**Fuentes principales:** web oficial, Mission Control y avisos legales.

## 1. Qué se puede afirmar

La web oficial presenta JARVIS como un asistente de IA con experiencia visual de escritorio, distintas versiones y capacidades que dependen del plan. La página anuncia:
- Versión gratuita para probar conversación y voz desde navegador.
- Plus con copiloto en páginas web, memoria que persiste entre sesiones y sincronización entre dispositivos.
- Pro con funciones avanzadas, voz de mayor calidad, memoria semántica, Mission Control y un equipo de agentes.
- Una experiencia recomendada en ordenador; la propia página indica que la versión móvil sigue evolucionando.

Fuente: https://www.dannyelarquitecto.com/jarvis

Estas son funciones anunciadas por el creador. No deben tratarse como verificación independiente de cómo están implementadas ni de que cada función tenga el mismo alcance que un agente local de escritorio.

## 2. Mission Control

La guía pública de Mission Control describe un panel para organizar agentes y tareas en salas, ver estado/progreso, enviar tareas, atender mensajes y supervisar el flujo de trabajo.

La guía diferencia responsabilidades:
- El usuario actúa como operador y valida decisiones clave.
- JARVIS supervisa, filtra y sugiere.
- Los agentes ejecutan tareas asignadas.
- La guía afirma que JARVIS no actúa por el usuario sin su visto bueno.

También menciona vistas de equipo y táctica, bandeja, contenido, agentes, tareas, salas y alertas.

Fuente: https://dannyelarquitecto.com/mission-control/

**Observación:** el sitio muestra una interfaz con ejemplos de estado, nombres de agentes y porcentajes de progreso. Una demostración visual no prueba por sí sola que cada tarea se ejecute en segundo plano, que el progreso se derive de resultados reales o que exista un motor multiagente completo. Eso requiere pruebas reproducibles o acceso al código y a la documentación técnica.

## 3. Modelo, llaves y datos

La página legal afirma que JARVIS usa modelos generativos como Groq y Google Gemini mediante la llave personal del usuario (BYOK). La misma página explica que la llave se guarda en el navegador del usuario y viaja cifrada por HTTPS en las solicitudes; también describe servicios relacionados con voz, tipografías, vídeo y almacenamiento de algunos datos de registro.

Fuente: https://www.dannyelarquitecto.com/legal

Esto es una declaración de la propia web, no una auditoría de seguridad. Para validar la afirmación haría falta observar el tráfico, revisar el código cliente y servidor, conocer la configuración de almacenamiento y verificar que ninguna llave se registre o se exponga accidentalmente.

La página legal también reconoce que los modelos pueden generar información inexacta, incompleta o sesgada, y recomienda verificar los resultados importantes.

## 4. Límites y preguntas sin resolver

No se encontró en esta pasada un repositorio de código fuente oficial público que permita inspeccionar el núcleo de JARVIS de Danny. Por eso, estas cuestiones siguen **sin verificar**:

- Qué modelo exacto se usa en cada función y cómo se cambia.
- Cómo se implementa la memoria semántica, cómo se indexa y cómo se elimina.
- Si Mission Control usa agentes independientes, llamadas secuenciales a modelos o una mezcla.
- Si las tareas siguen ejecutándose al cerrar la pestaña o desconectar el dispositivo.
- Cómo se guardan estados, resultados, reintentos y fallos.
- Qué permisos tienen los agentes y cómo se validan acciones peligrosas.
- Qué sistema de cola, base de datos, autenticación y despliegue sostiene la aplicación.
- Cómo se comprueba el progreso que se muestra.
- Qué funciones son reales, cuáles están en demostración y cuáles son simulaciones o contenido local.
- Cómo se manejan inyección de instrucciones desde páginas web, filtración de datos y fallos de proveedor.

No es correcto declarar que son defectos sin acceder al código; son preguntas de auditoría.

## 5. Errores, limitaciones y posibles soluciones para replicar la idea en M.I.L.O.

### Riesgo: confundir interfaz con ejecución
**Riesgo:** un tablero puede mostrar agentes, tareas y porcentajes sin demostrar que una tarea se completó correctamente.

**Solución para M.I.L.O.:** cada tarea debe tener ID, objetivo, estado, pasos, herramienta invocada, resultado observado, artefactos generados, fecha, error y evidencia. El progreso debe calcularse desde pasos completados, no generarse libremente por el modelo.

### Riesgo: memoria poco transparente
**Riesgo:** una “memoria profunda” no garantiza exactitud, eliminación completa ni que se recupere el dato correcto.

**Solución:** registro consultable de recuerdos con origen, fecha, confianza, edición y eliminación; búsqueda híbrida; límites de retención; separar memoria de usuario, memoria de proyecto y contexto temporal.

### Riesgo: proveedor remoto y privacidad
**Riesgo:** BYOK no implica procesamiento totalmente local. Los prompts y los archivos seleccionados pueden enviarse a la API elegida.

**Solución:** indicador visible de proveedor y destino de datos, opción local, minimización de contexto, redacción de secretos y consentimiento antes de compartir capturas o documentos.

### Riesgo: autonomía ambigua
**Riesgo:** un agente puede ejecutar acciones que no corresponden a la intención original.

**Solución:** niveles de autonomía (consultar, preparar, ejecutar acciones reversibles, acciones sensibles con confirmación), lista permitida de herramientas y validación posterior del resultado.

### Riesgo: dependencia de producto cerrado
**Riesgo:** sin código o documentación interna suficiente, no se puede reproducir con precisión el sistema ni comprobar sus límites.

**Solución:** implementar patrones observables y documentados, no intentar adivinar la implementación interna. Diseñar M.I.L.O. con interfaces propias y componentes intercambiables.

## 6. Qué investigar después

1. Recorrer las páginas públicas enlazadas desde JARVIS, las preguntas frecuentes y la documentación de cada versión.
2. Verificar diferencias funcionales entre Free, Plus y Pro, con fecha de consulta.
3. Buscar documentación pública sobre integraciones, memoria, agentes, sincronización y controles.
4. Examinar el sitio desde navegador y registrar qué controles funcionan y qué acciones requieren cuenta; no usar cuentas ni realizar compras.
5. Revisar términos, privacidad, condiciones de uso y límites de licencia.
6. Si se publica código oficial, auditarlo módulo por módulo antes de actualizar el estado de verificación.

## Fuentes

- Producto JARVIS: https://www.dannyelarquitecto.com/jarvis
- Mission Control: https://dannyelarquitecto.com/mission-control/
- Avisos legales: https://www.dannyelarquitecto.com/legal
- Página principal del creador: https://dannyelarquitecto.com/
