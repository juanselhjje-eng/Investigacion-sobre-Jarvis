# Evolución de la serie MARK de FatihMakes

Esta comparación reconstruye la evolución a partir de los README y árboles públicos consultados. No significa que se hayan ejecutado todas las versiones.

## 1. Mark X / X.1: asistente local por intención y acción

Fuentes:
- https://github.com/FatihMakes/Mark-X
- https://github.com/FatihMakes/Mark-X.1
- https://github.com/FatihMakes/Jarvis-Mark-X
- Tutorial enlazado en README: https://youtu.be/w_AGre6-9TM

El README de Mark X.1 describe un asistente modular de escritorio hecho en Python. Las funciones declaradas son abrir aplicaciones, enviar mensajes por WhatsApp, buscar en la web, consultar el tiempo, mantener memoria temporal y de largo plazo, y conversar con memoria. El árbol muestra carpetas `actions`, `core`, `memory`, más `main.py`, `llm.py`, `speech_to_text.py`, `tts.py` y `ui.py`.

**Arquitectura deducible del árbol:** un núcleo coordina STT, LLM, TTS, interfaz y acciones; la capacidad de control está programada como acciones específicas, no como control universal mágico.

**Limitaciones documentadas o previsibles:**
- El README ofrece una lista pequeña de capacidades; no documenta el alcance de control de pantalla, navegación visual ni planificación multi-paso.
- El sistema requiere que el usuario programe las acciones que puede ejecutar; el propio README advierte que solo hace lo que se codifica.
- El README declara uso personal/educativo y prohíbe uso comercial, venta y presentarlo como trabajo propio. Revisar licencia y avisos de la versión concreta antes de reutilizar.
- La afirmación “100% local” debe revisarse junto con la configuración del modelo y cualquier API: no basta con que el código corra en el PC para demostrar que toda inferencia se hace localmente.

## 2. Mark XXXIX-OR: voz en tiempo real, visión y tareas

Fuente: https://github.com/FatihMakes/Mark-XXXIX-OR

El README declara voz en tiempo real, control de sistema, abrir aplicaciones, gestionar archivos, ejecutar comandos de terminal, tareas complejas de varios pasos, visión de pantalla/webcam, memoria persistente e interacción híbrida por voz y teclado. Añade carga directa de archivos, UI redimensionable, mejoras multiplataforma y llamadas de modelos auxiliares mediante OpenRouter mientras Gemini Live mantiene la voz.

### Dependencias visibles

El `requirements.txt` consultado incluye, entre otras, `sounddevice`, `google-genai`, `google-generativeai`, `pillow`, `requests`, `beautifulsoup4`, `duckduckgo-search`, `playwright`, `pyautogui`, `pyperclip`, `opencv-python`, `numpy`, `mss`, `psutil`, `send2trash`, `youtube-transcript-api`, `pywinauto` y bibliotecas específicas de Windows.

**Riesgo de instalación observado en el diseño:** la lista mezcla paquetes multiplataforma y dependencias Windows. En incidencias públicas hay usuarios que reportan que `pypiwin32` falla en Ubuntu, que falta `sounddevice`, y que no funciona el micrófono. El Mark LVI posterior mejora el uso de marcadores de plataforma y el diagnóstico de instalación; esto es un indicio de evolución del proyecto, no prueba de que todos los problemas anteriores se hayan eliminado.

### Incidencias y sugerencias de la comunidad

Fuentes:
- https://github.com/FatihMakes/Mark-XXXIX-OR/issues
- https://github.com/FatihMakes/Mark-XXXIX-OR/pulls

Reportes públicos consultados:
- “VOICE COMMANDS NOT WORKING” (#40): el usuario puede chatear, pero no usar comandos de voz.
- “No soundevice module” (#42): falta el paquete de audio.
- “I having problem with pypiwin32 ... Ubuntu” (#41): la dependencia Windows impide instalación en Linux.
- “Can’t chat / i can't chat with it” y reportes de errores PyQt6 o Google GenAI.
- Solicitud de proveedor configurable compatible con OpenAI (#47): propone configurar base URL y clave para proveedores como Groq, DeepSeek, Mistral, OpenAI, Ollama o LM Studio, manteniendo Gemini Live como voz. La propuesta coincide con el rumbo que luego se documenta en Mark LVI, aunque debe comprobarse qué versión implementó cada parte.

Estos reportes son evidencia de problemas de usuarios en esa versión/entorno, no una tasa estadística de fallos.

### Mejoras recomendadas para M.I.L.O.

1. Instalar dependencias según sistema operativo y separar extras opcionales.
2. Probar audio antes de abrir la sesión conversacional.
3. No dar por completada una acción de escritorio solo porque se envió un clic o comando.
4. Evitar comandos de terminal construidos con texto libre; validar ejecutables y parámetros.
5. Separar modelo de voz y modelos auxiliares, con fallback y timeout.
6. Añadir diagnósticos y pruebas automatizadas de instalación.

## 3. Mark LVI (56): modularidad, proveedores, resultados verificables y seguridad

Fuente: https://github.com/FatihMakes/Mark-LVI

Mark LVI mantiene la idea de asistente de escritorio, pero el README muestra un sistema mucho más dividido en módulos:
- `main.py`: ciclo principal, sesión Gemini Live, audio, despacho de herramientas.
- `ui.py`: interfaz PyQt6 y HUD.
- `actions/`: funciones que controlan el ordenador.
- `plugins/`: capacidades extra, como correo, mensajería o herramientas especializadas.
- `providers/`: motores, LLM, STT y TTS alternativos.
- `memory/`: configuración y memoria persistente.
- `core/`: cargadores, confirmación, deshacer, dispositivos de audio, eco, wake word y utilidades.

### Avances destacados frente a versiones anteriores

- Proveedores intercambiables y opción de modelos locales.
- Separación de la conversación en tiempo real y llamadas auxiliares.
- Fallback por cuotas, errores y timeouts.
- Acciones auto-descriptivas y plugins detectados automáticamente.
- Memoria local con recuperación bajo demanda, en lugar de enviar todo el archivo a cada sesión.
- Confirmación humana para algunas acciones irreversibles y deshacer para ciertas acciones reversibles.
- Resultados `done`, `unverified` y `failed` para evitar afirmar que una acción ocurrió sin comprobarlo.
- Carga diferida de bibliotecas pesadas.
- Manejo más explícito de límites de plataforma, como Wayland y permisos de accesibilidad de macOS.

### Costes y limitaciones

- El modo local puede ser más lento que el flujo de voz integrado; el README estima 1,5–4 segundos para una cadena STT → LLM → TTS en hardware ordinario, frente a 0,3–0,6 segundos para la ruta Live según las mediciones del autor.
- Las APIs alojadas envían datos al proveedor elegido; “local” solo se aplica si todas las partes relevantes se ejecutan localmente.
- Controlar ventanas y aplicaciones depende de permisos, drivers, servidor gráfico y estado real del sistema.
- Los plugins Python no quedan aislados solo por estar en una carpeta: se ejecutan con los permisos del proceso si se importan.
- La memoria local sigue necesitando políticas de privacidad, borrado, respaldo y manejo de corrupción.
- La licencia actual consultada de Mark LVI es CC BY-NC 4.0, no una licencia que autorice uso comercial por defecto.

## 4. Trayectoria resumida

| Etapa | Enfoque declarado | Principal aprendizaje para M.I.L.O. |
|---|---|---|
| Mark X / X.1 | Voz, intención → acción, apps, mensajes, web, clima y memoria | Empezar con acciones concretas y contratos simples |
| Mark XXXIX-OR | Voz Live, visión, archivos, tareas multi-paso y OpenRouter | Separar conversación de operaciones auxiliares y ampliar capacidades de forma modular |
| Mark LVI | Proveedores configurables, modo local, recuperación de memoria, resultados verificados, confirmación y deshacer | Priorizar fiabilidad, privacidad explícita, fallos recuperables y no mentir sobre resultados |

## 5. Qué no se puede concluir todavía

- No se han ejecutado las versiones en una matriz de sistemas operativos.
- Las afirmaciones de velocidad publicadas por el creador no son un benchmark independiente.
- Las incidencias públicas no prueban el estado del defecto en el último commit.
- El historial completo de cada versión y cada vídeo aún requiere revisión sistemática.
- No se debe copiar ni distribuir el código de estos proyectos sin respetar su licencia y atribución.
