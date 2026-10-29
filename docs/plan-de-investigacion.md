# Plan de investigación y porcentaje de cobertura

Fecha de corte inicial: 29 de octubre de 2026.

## Qué significa el porcentaje

El porcentaje representa qué parte de una lista de áreas técnicas está documentada con fuentes, no qué tan cerca está M.I.L.O. de ser un asistente perfecto. Una función anunciada no equivale a una función probada. Se mantendrá separado el nivel de documentación del nivel de verificación práctica.

## Matriz de cobertura inicial

| Área | Estado inicial | Evidencia o trabajo pendiente |
|---|---|---|
| Funciones anunciadas de Mark LVI | Parcial | README primario consultado; falta comprobar módulos |
| Arquitectura del proceso principal | Parcial | Imports, sesión Gemini, audio, acciones y memoria visibles en main.py |
| Entrada y salida de audio | Parcial | Código y reportes de micrófono; falta prueba local |
| Memoria persistente | Parcial | README describe memoria local y búsqueda; falta auditar almacenamiento y recuperación |
| Control del sistema operativo | Parcial | Capacidades anunciadas; falta auditar cada acción |
| Visión y capturas de pantalla/cámara | Parcial | Capacidades anunciadas; falta revisar permisos y módulo completo |
| Plugins y acciones | Parcial | Cargadores auto-descubiertos descritos en main.py; falta revisar contratos |
| Confirmación y deshacer | Parcial | Se anuncian; módulos disponibles para auditoría posterior |
| Seguridad e inyección de instrucciones | Parcial | README afirma frontera de contenido no confiable; falta verificar implementación |
| Proveedores locales y externos | Parcial | README y setup consultados; falta revisar adaptadores |
| Instalación y dependencias | Parcial | setup.py revisado; falta requirements completo y pruebas por SO |
| Incidencias de usuarios | Parcial | Consultadas varias issues recientes y antiguas |
| Ideas y solicitudes de la comunidad | Parcial | Revisadas algunas incidencias y propuestas |
| Versiones antiguas de FatihMakes | Pendiente | Comparar Mark-X, Mark-X.1, Mark-XXXIX-OR y otras versiones |
| Producto de Danny el Arquitecto | Parcial | Web, Mission Control y legal consultados |
| Código fuente de Danny | No verificado | No se localizó en esta pasada un repositorio fuente oficial público; no asumir que el producto web y sus demostraciones prueban todas las funciones |
| Pruebas reales reproducibles | Pendiente | No se ha ejecutado el software en el equipo del usuario |
| Comparación final y plan de implementación | Pendiente | Se elaborará tras completar la auditoría |

**Estimación actual: 35%.** Se usa una cifra conservadora porque hay mucha información de marketing y README, pero aún no se ha inspeccionado cada módulo ni ejecutado las aplicaciones.

## Método

1. Priorizar repositorios, código, documentación oficial, incidencias y páginas legales.
2. Para cada capacidad, registrar fuente, qué dice exactamente, qué se puede inferir y qué falta probar.
3. Separar errores observados en incidencias de defectos confirmados por reproducción.
4. Proponer arreglos concretos con coste, riesgo y prioridad.
5. No copiar código de forma que infrinja licencias. El objetivo es aprender patrones y construir una implementación propia compatible con las licencias de las dependencias.
6. Mantener una lista de cambios con fecha y commit.

## Criterio del 80%

Se notificará al usuario cuando el documento alcance al menos 80% de las áreas definidas y exista cobertura de las dos líneas de investigación. Si el código de Danny no es público, el área de implementación interna se marcará como no verificable, no se inventará una arquitectura.

## Fuentes consultadas en la fase inicial

- https://github.com/FatihMakes/Mark-LVI
- https://github.com/FatihMakes/Mark-LVI/blob/main/main.py
- https://github.com/FatihMakes/Mark-LVI/blob/main/setup.py
- https://github.com/FatihMakes/Mark-LVI/issues/160
- https://github.com/FatihMakes/Mark-LVI/issues/162
- https://github.com/FatihMakes/Mark-LVI/issues/154
- https://github.com/FatihMakes/Mark-LVI/issues/159
- https://github.com/FatihMakes/Mark-XXXIX-OR/issues/47
- https://www.dannyelarquitecto.com/jarvis
- https://dannyelarquitecto.com/mission-control/
- https://www.dannyelarquitecto.com/legal
