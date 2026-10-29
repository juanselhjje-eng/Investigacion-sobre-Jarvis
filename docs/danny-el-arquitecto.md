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

## 5. Errores, limitaciones y posibles soluciones para replicar la idea en un asistente propio

### Riesgo: confundir interfaz con ejecución
**Riesgo:** un tablero puede mostrar agentes, tareas y porcentajes sin demostrar que una tarea se completó correctamente.

**Solución para un asistente propio:** cada tarea debe tener ID, objetivo, estado, pasos, herramienta invocada, resultado observado, artefactos generados, fecha, error y evidencia. El progreso debe calcularse desde pasos completados, no generarse libremente por el modelo.

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

**Solución:** implementar patrones observables y documentados, no intentar adivinar la implementación interna. Diseñar un asistente propio con interfaces propias y componentes intercambiables.

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


## 7. Observaciones concretas de la interfaz pública de Mission Control

La página pública presenta varios módulos, entre ellos JARVIS Core, Comunicaciones, Observatorio, Laboratorio y Motores, además de salas de estrategia, investigación, contenido, automatización y analítica. También presenta paneles para agentes, tareas, alertas, bitácora y contenido.

La misma página aclara explícitamente que algunas partes siguen en preparación:
- La bandeja que clasificaría mensajes de redes sociales todavía no está conectada a las cuentas; la página dice que no mostrará mensajes inventados mientras no esté conectada.
- La zona de contenido describe una primera fase local/demo para convertir una idea en hook, caption, carrusel o story sin API externa.
- La sección para gestionar llaves y consumo de modelos aparece como pendiente de preparación.

Estas notas son relevantes porque muestran que el producto combina módulos disponibles con otros que se están preparando. No se debe asumir que toda sección visible equivale a una integración terminada. Algunos valores de telemetría y estados que aparecen en la interfaz son elementos de la presentación; sin acceso a la instrumentación interna no se puede confirmar que midan recursos o tareas reales.

Fuente: https://dannyelarquitecto.com/mission-control/

## 8. Arquitectura probable a alto nivel, sin atribuir detalles internos

A partir de las páginas públicas solo se puede proponer un mapa conceptual, no confirmar la implementación:

1. **Cliente web:** interfaz de conversación, voz, ajustes y Mission Control.
2. **Proveedores de IA:** la página legal menciona Gemini y Groq, con BYOK en determinadas rutas.
3. **Memoria y sincronización:** las páginas de producto anuncian memoria entre sesiones y dispositivos; el aviso legal menciona Supabase para el formulario de interés de El Método, no demuestra por sí solo que toda la memoria de JARVIS use exactamente el mismo esquema.
4. **Mission Control:** interfaz para salas, tareas, agentes y aprobaciones; la guía dice que las decisiones clave quedan bajo supervisión del usuario.
5. **Servicios de voz y contenido:** la política legal enumera proveedores y usos específicos; hay que distinguir qué datos recibe cada proveedor.

No se debe convertir este mapa conceptual en una afirmación sobre frameworks, colas, bases de datos, workers o arquitectura del servidor, porque el código fuente interno no ha sido auditado.

## 9. Lista de comprobación para una auditoría independiente de Danny

- [ ] Probar conversación de texto y voz en una cuenta de prueba sin comprar planes.
- [ ] Determinar si la voz es streaming o síntesis posterior y medir latencia con una metodología repetible.
- [ ] Comprobar qué ocurre al cerrar la pestaña durante una tarea.
- [ ] Crear una tarea y comprobar si el progreso refleja pasos reales y resultados guardados.
- [ ] Revisar si las aprobaciones bloquean realmente la ejecución de la acción.
- [ ] Inspeccionar en el navegador el almacenamiento local de la llave BYOK y confirmar que no se envía en rutas no necesarias.
- [ ] Revisar tráfico de red para identificar qué contenido se envía a cada proveedor, sin registrar claves ni datos privados.
- [ ] Verificar qué memoria se sincroniza, cómo se borra y cómo se exporta.
- [ ] Confirmar que las integraciones de redes sociales están conectadas antes de confiar en la bandeja.
- [ ] Identificar qué módulos son demo, están en preparación o dependen de una suscripción.

Esta lista no se ha ejecutado. No se han creado cuentas ni realizado compras como parte de la investigación.
