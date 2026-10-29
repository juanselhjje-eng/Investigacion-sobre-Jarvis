# Arquitectura propuesta para M.I.L.O. basada en la primera investigación

Este documento propone patrones técnicos, no afirma que ya estén implementados.

## Principios

1. **Acción real, resultado comprobado.** El modelo propone una herramienta; el ejecutor la valida y devuelve evidencia observable.
2. **Permisos mínimos.** Cada herramienta solo recibe acceso a los recursos necesarios.
3. **Confirmación independiente.** El modelo no puede aprobar por sí mismo una operación que exige confirmación humana.
4. **Memoria editable.** El usuario puede ver, corregir y eliminar recuerdos.
5. **Proveedores intercambiables.** Conversación, transcripción y voz deben poder configurarse por separado.
6. **Errores visibles.** No ocultar fallos ni convertir un timeout en un falso éxito.
7. **Tareas persistentes.** Los planes sobreviven al reinicio cuando se guardan explícitamente y permiten reanudar desde el último paso válido.
8. **Pruebas reproducibles.** Cada herramienta debe tener pruebas positivas, negativas y de recuperación.

## Componentes

- Interfaz: escritorio, texto, voz, estado de tareas y ajustes.
- Orquestador: interpreta objetivos, crea planes limitados y gestiona estados.
- Proveedor de modelos: adaptadores para APIs y modelos locales.
- Audio: captura, detección de actividad, STT, TTS, interrupción y selección de dispositivos.
- Percepción: capturas bajo permiso explícito, OCR/visión y contexto de ventanas.
- Registro de herramientas: esquema de parámetros, permisos, handler, timeout y política de confirmación.
- Ejecutor: ejecuta una herramienta por vez, registra el resultado y comprueba el efecto.
- Memoria: hechos del usuario, proyectos, episodios y preferencias con origen y fecha.
- Planificador: tareas, dependencias, reintentos limitados y cancelación.
- Seguridad: validación de entradas, protección de secretos, aislamiento de plugins y contenido remoto no confiable.
- Observabilidad: logs locales redactados, métricas de latencia, trazas de herramienta y exportación de diagnósticos.

## Formato de resultado de una herramienta

Cada herramienta debe devolver un estado explícito:
- `done`: el efecto se observó y se verificó.
- `unverified`: la petición se envió, pero no se pudo comprobar el efecto.
- `failed`: la acción falló, con error útil.
- `needs_confirmation`: falta autorización explícita.
- `cancelled`: el usuario canceló.

El modelo no debe convertir `unverified` en “listo”.

## Seguridad por niveles

- Nivel 0: leer o explicar información proporcionada.
- Nivel 1: preparar cambios sin ejecutarlos.
- Nivel 2: ejecutar acciones reversibles y de bajo impacto autorizadas previamente.
- Nivel 3: solicitar confirmación para enviar mensajes, borrar datos, instalar software, modificar configuraciones importantes o publicar contenido.
- Nivel 4: bloquear acciones no permitidas por la política del sistema.

No almacenar claves API en el repositorio. Usar variables de entorno o almacén de secretos del sistema operativo; excluir archivos de configuración sensibles del control de versiones.

## Plan de implementación sugerido

1. Estabilizar conversación, configuración y logs.
2. Implementar registro de herramientas y resultados verificables.
3. Añadir memoria persistente con panel de revisión.
4. Añadir control de ventanas y navegador con permisos.
5. Añadir visión con consentimiento y selección de pantalla.
6. Añadir planificación multi-paso con reanudación y cancelación.
7. Añadir proveedores alternativos y modo local.
8. Añadir plugins aislados, pruebas y diagnóstico.
9. Añadir panel de equipo/subagentes solo después de que las tareas individuales sean fiables.

## Métricas mínimas

- Tasa de éxito verificado por herramienta.
- Falsos positivos de “completado”.
- Latencia de primera respuesta de voz.
- Errores de reconocimiento y activación.
- Reintentos por proveedor.
- Porcentaje de tareas reanudadas correctamente.
- Tasa de recuperación de memoria relevante.
- Incidentes de permisos o filtración de secretos.
