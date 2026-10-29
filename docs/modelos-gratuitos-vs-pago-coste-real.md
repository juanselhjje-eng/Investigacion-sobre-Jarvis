# Modelos gratuitos frente a modelos de pago: coste real de un asistente

**Fecha de corte:** 29 de octubre de 2026.  
**Tipo de investigación:** comparación documental de páginas oficiales de proveedores y plataformas. Los precios, cuotas y modelos cambian; verificar antes de implementar.

## Resumen ejecutivo

No existe una respuesta universal de “gratis es peor” o “pago es mejor”. La decisión depende de la tarea, la frecuencia, el privacidad requerida, el hardware y cuánto importa la continuidad.

- **Gratis en la nube:** útil para aprender, prototipar y uso ocasional; límites de cuota, disponibilidad, modelos cambiantes y condiciones de datos pueden ser importantes.
- **Modelo local con pesos abiertos:** sin cobro por token ni envío obligatorio a una API remota, pero usa electricidad, RAM/VRAM, almacenamiento y tiempo de configuración. “Gratis” no significa coste total cero.
- **API de pago por uso:** permite escoger modelos y pagar según tokens/herramientas; buena para medir y controlar gasto si se ponen límites, pero los agentes pueden multiplicar llamadas.
- **Suscripción de chat:** paga acceso a una interfaz/servicio y límites incluidos; no implica automáticamente que una API de desarrollador esté incluida.
- **Híbrido:** suele ser el diseño más práctico: modelo barato/local para tareas simples, modelo mejor para tareas difíciles, y límites duros de gasto.

## 1. Qué significa realmente “gratis”

| Modalidad | Qué puede costar $0 | Costes o límites que permanecen |
|---|---|---|
| Chat gratuito en una web | Uso dentro de los límites del plan | Mensajes limitados, acceso a modelos restringido, cambios de cuota y funciones no disponibles |
| API gratuita | Algunas llamadas/modelos dentro de cuota | Rate limits, disponibilidad variable, políticas de datos y modelos no aptos para producción |
| Modelo local | Inferencia sin tarifa por token | PC, electricidad, RAM/VRAM, disco, refrigeración, configuración y velocidad |
| Código open source | Licencia permite usar el código bajo sus términos | Hosting, APIs, servicios de voz, mantenimiento y licencias de pesos/modelos pueden costar |
| Crédito promocional | Prueba temporal | Puede agotarse; no es una fuente permanente |
| Plan de pago mensual | Acceso incluido en el plan | Límites, uso sujeto a políticas y costes extra de API/hosting no incluidos |

## 2. Ejemplos oficiales revisados

### Gemini API

La página oficial diferencia Free y Paid: el nivel gratuito da acceso limitado a determinados modelos y tokens sin coste, pero la página indica que el contenido del nivel gratuito puede usarse para mejorar productos. El nivel de pago tiene límites más altos y declara que el contenido no se usa para mejorar productos. No todos los modelos están disponibles en el nivel gratuito.

Para un ejemplo concreto, la página consultada mostraba Gemini 3.8 Flash con tarifa gratuita disponible y un precio estándar publicado para el nivel de pago; las tarifas cambian en fechas futuras indicadas en la propia página. No se debe extrapolar esa tarifa a otros modelos ni asumir que todas las herramientas son gratuitas.

Fuente oficial: https://ai.google.dev/gemini-api/docs/pricing

### OpenRouter

La colección oficial muestra modelos con precio de tokens $0, incluidos modelos con sufijo `:free`. La página de límites indica 20 solicitudes por minuto y 50 al día para variantes gratuitas; puede subir a 1.000 solicitudes al día tras comprar al menos 10 USD en créditos. Los intentos fallidos también pueden contar. Modelos sin sufijo `:free` pueden tener reglas diferentes, y las cuotas del proveedor pueden ser más estrictas.

Fuentes:
- https://openrouter.ai/collections/free-models
- https://openrouter.ai/docs/limits
- https://openrouter.ai/pricing

**Interpretación:** un modelo gratuito puede ser muy útil, pero una cuota de 50 solicitudes al día puede agotarse rápidamente si el agente realiza múltiples llamadas por tarea, reintenta o divide el trabajo en subagentes. No se recomienda diseñar un producto estable dependiendo de una cuota gratuita que puede cambiar.

### Ollama: local y nube

Ollama permite ejecutar modelos localmente sin suscripción; sus modelos en la nube usan créditos y precios propios. La documentación de la biblioteca ofrece ejemplos de requisitos de RAM: 7B alrededor de 8 GB, 13B alrededor de 16 GB y 70B alrededor de 64 GB para la familia indicada; son reglas orientativas, no garantía para todos los modelos. Cuantización, contexto, GPU y otras aplicaciones modifican los requisitos.

Fuentes:
- https://ollama.com/pricing
- https://ollama.com/library/llama2

### API de pago

Las páginas oficiales de OpenAI y Anthropic publican tarifas por millón de tokens, con precios distintos para entrada, salida, caché y procesamiento por lotes. Por ejemplo, la tabla de OpenAI muestra diferencias grandes entre modelos y modalidades; Anthropic documenta precios por modelo y un descuento del 50% en Batch API para procesamiento asíncrono. Estas páginas son mejores que usar una tabla de terceros como fuente definitiva.

Fuentes:
- https://developers.openai.com/api/docs/pricing
- https://platform.claude.com/docs/en/about-claude/pricing

**Nota:** las suscripciones de chat y las API son productos de facturación distintos. No asumir que pagar una suscripción mensual habilita llamadas ilimitadas desde un programa externo.

## 3. ¿Cuándo conviene gratis y cuándo pago?

| Necesidad | Opción inicial | Motivo | Cuándo cambiar |
|---|---|---|---|
| Aprender y hacer pruebas | Modelo gratuito de API o modelo local pequeño | Minimiza el gasto inicial | Si las cuotas bloquean pruebas o las respuestas no bastan |
| Comandos sencillos y clasificación | Modelo pequeño/local o API barata | Tareas acotadas no siempre necesitan el modelo más capaz | Si se mide una tasa alta de error |
| Investigación larga y razonamiento difícil | Modelo de mayor capacidad con presupuesto | Puede resolver mejor tareas complejas, aunque debe probarse | Mantenerlo solo si mejora resultados medidos |
| Voz en tiempo real | Pipeline local o servicio con streaming | Latencia, interrupciones y coste por audio importan | Comparar latencia y reconocimiento en el entorno real |
| Uso constante durante todo el día | Local/híbrido o API con límites y métricas | El coste acumulado y la disponibilidad se vuelven importantes | Si el hardware local es lento o el coste API resulta menor |
| Datos privados | Modelo local o proveedor con condiciones adecuadas | Evita o reduce transferencias remotas | Revisar conectores y telemetría; local no garantiza que todo permanezca local |
| Agente que usa navegador, archivos y terminal | Modelo que soporte bien herramientas, más permisos restringidos | La calidad de tool-calling importa tanto como la prosa | Probar tareas completas y recuperación de errores |
| Producto para terceros | Presupuesto de operación y proveedor con condiciones claras | Debe haber continuidad, soporte y control de coste | No basarse únicamente en cuotas gratuitas |

## 4. Cómo calcular el coste de API

El coste aproximado de una solicitud de texto es:

`coste = (tokens_entrada / 1.000.000 × precio_entrada) + (tokens_salida / 1.000.000 × precio_salida)`

A esto se suman, cuando corresponda, audio, imágenes/vídeo, búsqueda, almacenamiento de caché, ejecución de herramientas y comisiones de plataforma.

### Ejemplo de cálculo hipotético

Si una tarea procesa 20.000 tokens de entrada y genera 4.000 de salida, con tarifas ilustrativas de 0,50 USD por millón de entrada y 2 USD por millón de salida:

- Entrada: 20.000 / 1.000.000 × 0,50 = 0,01 USD.
- Salida: 4.000 / 1.000.000 × 2 = 0,008 USD.
- Total de tokens: 0,018 USD por tarea, antes de herramientas y otros cargos.

Este ejemplo es matemático, no una cotización de un modelo específico.

### Por qué los agentes pueden salir más caros que un chat

Una tarea con herramientas puede producir una secuencia: plan → llamada a herramienta → resultado → nuevo razonamiento → otra herramienta → verificación. Cada vuelta vuelve a consumir tokens. Un bucle de reintentos o un subagente que repite el contexto puede multiplicar el consumo.

Controles recomendados:
- máximo de pasos por tarea;
- máximo de llamadas y tiempo de ejecución;
- presupuesto mensual y alertas;
- límite de salida y tamaño de contexto;
- caché de instrucciones estables cuando el proveedor lo soporte;
- modelo pequeño para clasificación y tareas sencillas;
- escalado a un modelo caro solo cuando sea necesario;
- registro de coste por tarea, herramienta y modelo;
- detener reintentos repetidos y pedir confirmación cuando el objetivo no avanza.

## 5. Lo que se necesita para un sistema de coste cero

### A. Capa de modelo
- Un modelo local con licencia compatible, o una API con nivel gratuito.
- Un selector de modelos que soporte fallbacks sin repetir llamadas indefinidamente.
- Un registro de proveedor, modelo, cuota, fecha de revisión y condiciones de datos.
- Para local: comprobar RAM/VRAM, espacio en disco, sistema operativo y rendimiento real.

### B. Voz
- STT local, por ejemplo Whisper u otro motor compatible, o cuota gratuita de un servicio.
- TTS local, por ejemplo Piper, o una alternativa gratuita con límites.
- Detección de voz/palabra de activación si se necesita manos libres.
- Pruebas de latencia, ruido, idioma e interrupciones.

### C. Herramientas
- Búsqueda web con cuota disponible o un motor local; el scraping debe respetar términos y robots.
- Automatización de navegador local y con permisos acotados.
- Acciones de archivos y aplicaciones con confirmación para cambios importantes.
- Integraciones externas que no exijan suscripciones de pago.
- Un plan alternativo si la API se cae o la cuota se agota.

### D. Almacenamiento
- SQLite o archivos locales para historial y configuración.
- Base vectorial local solo si realmente se necesita búsqueda semántica.
- Copias de seguridad, exportación y borrado de memoria.
- No guardar claves dentro del historial ni del prompt.

### E. Interfaz
- Web local o aplicación de escritorio con framework gratuito.
- Iconos, tipografías y recursos con licencias compatibles.
- Animaciones ligeras para no afectar al rendimiento.

### F. Ejecución y distribución
- Ejecutar el asistente en el PC del usuario evita pagar un servidor de IA, pero el PC debe estar encendido.
- Hosting gratuito puede dormir, imponer cuotas o no permitir procesos persistentes.
- Una aplicación local no necesita dominio público si solo se usa en ese PC.
- Distribuir la aplicación a otras personas añade soporte, actualizaciones, firma, instalador y obligaciones de privacidad.

### G. Costes ocultos que no desaparecen
Electricidad, hardware, Internet, tiempo de mantenimiento, almacenamiento, dominios, hosting, copias de seguridad, herramientas de voz, servicios de búsqueda, firma de software y soporte.

## 6. Arquitectura gratuita recomendada para una prueba personal

1. Interfaz local en navegador o escritorio.
2. Modelo local pequeño para clasificación y comandos sencillos.
3. Proveedor gratuito como alternativa opcional, con límites explícitos.
4. STT/TTS local para reducir dependencia de servicios externos.
5. Herramientas con lista permitida, no acceso ilimitado al sistema.
6. Memoria local con controles para ver, corregir y borrar.
7. Registro de fallos, latencia y consumo.
8. Límite de pasos, tiempo y solicitudes.
9. Modo degradado sin Internet.
10. Pruebas con archivos de ensayo antes de permitir acciones reales.

## 7. Arquitectura de pago controlado

1. Proveedor de API con precio público y límites de gasto.
2. Modelo barato por defecto y modelo potente solo para tareas difíciles.
3. API key guardada en variable de entorno o gestor de secretos, nunca en el frontend ni en Git.
4. Presupuesto máximo, alertas y mecanismo de corte.
5. Métricas de tokens, herramientas, errores y coste por flujo.
6. Política de datos verificada para cada proveedor.
7. Proveedor alternativo y manejo de 429/timeout sin bucles.
8. Pruebas de regresión cuando cambie el modelo.
9. Presupuesto separado para voz, búsqueda, almacenamiento y hosting.
10. Un límite de gasto configurado antes de exponer el agente a usuarios.

## 8. Recomendación basada en evidencia

Para experimentar, empieza con gratuito/local. Para tareas donde la precisión importa, compara en un conjunto fijo de tareas entre modelo gratuito, modelo local y modelo de pago. Mide:
- porcentaje de tareas completadas correctamente;
- errores peligrosos o acciones equivocadas;
- latencia mediana y percentil 95;
- coste por tarea completada, no solo por llamada;
- cantidad de intervención humana;
- continuidad cuando falla el proveedor;
- tratamiento de datos y facilidad para borrar información.

El modelo más barato no es automáticamente el más económico si necesita cinco intentos y mucho tiempo humano. El más caro tampoco es automáticamente el mejor si una tarea sencilla puede resolverse con un modelo pequeño.

## Fuentes oficiales

- Gemini API pricing: https://ai.google.dev/gemini-api/docs/pricing
- OpenRouter free models: https://openrouter.ai/collections/free-models
- OpenRouter pricing and limits: https://openrouter.ai/pricing
- OpenRouter limits: https://openrouter.ai/docs/limits
- Ollama pricing: https://ollama.com/pricing
- Ollama model memory example: https://ollama.com/library/llama2
- OpenAI API pricing: https://developers.openai.com/api/docs/pricing
- Anthropic API pricing: https://platform.claude.com/docs/en/about-claude/pricing

## Límites

No se hizo un benchmark común de calidad entre modelos. Los precios y cuotas se deben volver a revisar al implementar. El uso local no elimina coste eléctrico ni restricciones de licencia de cada modelo. La investigación no considera ningún proveedor “ilimitado” sin evidencia contractual.
