# Investigación técnica para construir M.I.L.O.

Investigación viva sobre asistentes digitales que pueden conversar, percibir información del equipo y ejecutar tareas: la serie MARK de FatihMakes y el JARVIS de Danny el Arquitecto.

## Estado de la investigación

**Progreso estimado de la fase inicial: 80% — todavía no alcanza el umbral del 80%.**

Este porcentaje es una estimación de cobertura del plan de investigación, no una medida científica. Ya se revisaron la documentación pública de Mark LVI, el núcleo, cargadores de acciones/plugins, memoria, confirmación, deshacer, proveedores, automatización de navegador, visión, archivos, mensajería y dependencias, reportes públicos de usuarios y las páginas públicas del producto JARVIS de Danny. La cobertura documental pública ha alcanzado aproximadamente el 80% del plan definido. No equivale a una auditoría de ejecución: faltan pruebas reproducibles en un equipo real, revisión completa de vídeos, y acceso al backend/código interno de Danny, que no se ha verificado como público.

## Documentos

- [Plan, alcance y método de verificación](docs/plan-de-investigacion.md)
- [FatihMakes: Mark LVI, arquitectura, funciones y errores reportados](docs/fatihmakes-mark-lvi.md)
- [Evolución de Mark X a Mark LVI](docs/evolucion-serie-mark.md)
- [Danny el Arquitecto: producto JARVIS, Mission Control y límites de la evidencia](docs/danny-el-arquitecto.md)
- [Diseño propuesto para M.I.L.O.](docs/arquitectura-propuesta-milo.md)
- [Comparativa y prioridades de implementación](docs/comparativa-y-prioridades.md)

## Regla de evidencia

Cada afirmación importante debe clasificarse como:
- **Verificado en código/documentación:** aparece en una fuente primaria consultada.
- **Reportado por usuarios:** aparece en una incidencia pública; no demuestra que le ocurra a todos.
- **Afirmación del creador:** se anuncia en una web o README, pero no se ha probado independientemente.
- **Inferencia/recomendación:** conclusión técnica propuesta para M.I.L.O.

## Fuentes principales

- FatihMakes, Mark LVI: https://github.com/FatihMakes/Mark-LVI
- Código principal: https://github.com/FatihMakes/Mark-LVI/blob/main/main.py
- Instalador: https://github.com/FatihMakes/Mark-LVI/blob/main/setup.py
- Incidencias Mark LVI: https://github.com/FatihMakes/Mark-LVI/issues
- JARVIS de Danny: https://www.dannyelarquitecto.com/jarvis
- Mission Control: https://dannyelarquitecto.com/mission-control/
- Avisos legales y privacidad: https://www.dannyelarquitecto.com/legal

## Próximo criterio de avance

No se marcará el 80% hasta cubrir, como mínimo: arquitectura, entrada/salida de voz, modelos y proveedores, memoria, percepción visual, control del sistema, navegación web, plugins, planificación multi-paso, interfaz, persistencia, privacidad, seguridad, instalación, dependencias, errores de usuarios, licencia y sugerencias de la comunidad; además, se revisarán las fuentes primarias disponibles de ambos proyectos y se señalarán explícitamente las áreas que no puedan verificarse.
