# 🐾 Huellas con Futuro: Sistema de Catálogo Inteligente para Refugios

**Curso:** Inteligencia artificial: Generación de Prompts
**Entrega:** Proyecto Final - IA: Entretejiendo Imaginación y Algoritmos
**Autora:** Sofía Algamiz
**Nº de Comisión:** 96090

---

## Resumen

**Huellas con Futuro** es una Prueba de Concepto (POC) que aplica técnicas de *Fast Prompting* para resolver un problema concreto de los refugios de animales: la falta de tiempo y de personal especializado para redactar biografías atractivas y conseguir fotografías presentables de los animales rescatados, lo que alarga sus tiempos de adopción. La solución combina un modelo **Texto-a-Texto** (GPT-4o-mini, vía API de OpenAI) que genera en **una única llamada** el título, la biografía, el copy de redes sociales, la ficha resumen y un prompt fotográfico calibrado a los rasgos reales del animal, con un modelo **Texto-a-Imagen** (usado de forma manual, sin API, en una herramienta gratuita) que transforma ese prompt en una imagen conceptual del animal en un hogar cálido. El resultado es una notebook funcional, con interfaz interactiva, modo demo sin credenciales, protocolo de veracidad y un costo de operación de centavos de dólar por mes — evolucionando de los prompts sueltos de la Preentrega 1 a un sistema integrado y medible.

---

## 1. Introducción

### 1.1 Nombre del proyecto
**Huellas con Futuro – Sistema de Catálogo IA**, una herramienta de generación asistida de perfiles de adopción para refugios y protectoras de animales.

### 1.2 Presentación del problema a abordar
Los refugios y protectoras de animales enfrentan una sobrepoblación constante y tiempos prolongados de estancia de los animales rescatados. Esta problemática se agrava por el déficit comunicacional que sufren estas organizaciones:

* **Sobrecarga operativa:** el personal dedica casi todos sus recursos al cuidado veterinario, la alimentación y el rescate, sin tiempo ni presupuesto para contratar redactores o fotógrafos.
* **Fichas frías e impersonales:** biografías puramente descriptivas o trágicas ("Macho, 2 años, mestizo, abandonado") que no generan la conexión emocional necesaria para motivar una adopción.
* **Fotografías desfavorables:** imágenes tomadas en caniles, con mala iluminación o con animales asustados, que invisibilizan durante meses a animales de pelaje oscuro, mayores o mestizos comunes.

Es una problemática relevante porque afecta directamente el bienestar animal y la capacidad operativa de organizaciones con recursos limitados: cada día que un animal no es visibilizado correctamente es un día más de institucionalización y un lugar menos disponible para un nuevo rescate.

### 1.3 Desarrollo de la propuesta de solución
La propuesta consiste en un **Sistema Generador de Catálogos de Adopción Responsable**, alimentado por modelos de IA generativa e implementado como una Prueba de Concepto (POC) en Jupyter Notebook.

La solución se vincula al desarrollo de modelos de IA a través de dos frentes:

1. **Modelo Texto-a-Texto (LLM – GPT-4o-mini):** genera, a partir de una ficha técnica del animal, una biografía persuasiva, un copy para redes sociales y una ficha resumen, aplicando técnicas de *storytelling* ético.
2. **Modelo Texto-a-Imagen (herramienta gratuita, sin API):** a partir de un prompt fotográfico calibrado con los rasgos físicos reales del animal (generado también por el LLM), produce una imagen conceptual que lo muestra en un entorno hogareño cálido, superando la limitación visual de las fotos tomadas en el refugio.

A diferencia de la Preentrega 1 —donde cada prompt (biografía emotiva, biografía en primera persona, retrato canino, retrato felino) se ejecutaba de forma independiente—, en esta etapa los tres resultados de texto (título, biografía, copy de redes) y el prompt de imagen se generan **en una única llamada a la API**, aplicando *Fast Prompting*. Cada técnica elegida resuelve una debilidad puntual detectada en esa primera entrega:

| Debilidad de la Preentrega 1 | Técnica de Fast Prompting que la resuelve |
|---|---|
| 4 prompts sueltos, ejecutados y copiados a mano uno por uno | **Fast Prompting en una sola pasada**: un único prompt maestro devuelve los 4 resultados juntos |
| Salida en texto libre, había que copiar y pegar cada parte a mano | **JSON forzado** (`response_format={"type": "json_object"}`): la salida ya viene parseable en variables de Python, sin post-procesamiento manual |
| El tono y la estructura variaban según cómo se redactara el prompt cada vez | **Few-Shot Prompting**: un ejemplo `input → output` fija el estilo y el nivel de detalle de forma consistente |
| No había un rol ni límites éticos explícitos para el modelo | **System Instructions**: fija el rol de redactor ético desde el inicio de la conversación, antes de cualquier input del usuario |

### 1.4 Justificación de la viabilidad del proyecto

| Dimensión | Análisis de viabilidad |
|---|---|
| **Viabilidad técnica** | Alta. No requiere infraestructura de servidor ni programación compleja: se ejecuta en una notebook con la API de OpenAI (texto) y una herramienta gratuita de generación de imagen, usada de forma manual (sin API). La complejidad recae en el diseño y calibración del prompt maestro. |
| **Tiempo y recursos** | Óptimo. El desarrollo, testeo e iteración de los prompts entra dentro de las semanas asignadas al curso (ver cronograma en la sección 3). Los insumos son solo fichas de prueba y acceso web/API a los modelos. |
| **Costos** | Muy bajo. Ver detalle de costos en la sección 3.3 — del orden de centavos de dólar por mes para un refugio con decenas de ingresos mensuales, y $0 en generación de imagen al no usar la API de DALL-E. |
| **Disponibilidad de datos** | Garantizada. Se trabaja con casos reales aportados por refugios locales o con perfiles sintéticos construidos sobre patrones comunes de rescates urbanos. |
| **Impacto y transferencia** | Directo. Una vez construido el prompt maestro, cualquier voluntario del refugio puede reutilizarlo completando variables de entrada simples, sin conocimientos previos de IA. |

**Conclusión de viabilidad:** el proyecto aborda un problema real, fracciona la solución en componentes simples, combina modelos Texto-Texto y Texto-Imagen, y es 100% realizable con los recursos disponibles durante la cursada.

---

## 2. Objetivos

### 2.1 Objetivo general
Desarrollar una Prueba de Concepto (POC) en Python/Jupyter Notebook que implemente técnicas de *Fast Prompting* para generar perfiles de adopción multimodales (texto persuasivo + prompt de imagen calibrado) con alta eficiencia de tokens y costo computacional mínimo.

### 2.2 Objetivos específicos
* Implementar *Few-Shot Prompting* y salida estructurada (JSON) para resolver la generación completa del perfil en **una única llamada a la API**, optimizando el costo por animal.
* Diseñar un **protocolo de validación ética y de veracidad** que evite alucinaciones sobre el temperamento del animal y asegure que la imagen generada respete su morfología real (color, tamaño, orejas, marcas distintivas).
* Crear una interfaz interactiva dentro del notebook que permita a voluntarios del refugio cargar datos sin escribir ni modificar código.
* Medir y mostrar explícitamente el consumo de tokens de cada consulta, para evidenciar la optimización buscada.
* **Analizar si las técnicas de Fast Prompting mejoran la propuesta de la Preentrega 1**: la respuesta es sí — se pasa de 2 prompts T2T + 2 prompts T2I ejecutados manualmente y por separado, a un único prompt maestro (few-shot + JSON forzado) que genera los cuatro resultados en una sola consulta, reduciendo tiempo de carga por animal y consumo de tokens.

---

## 3. Metodología

### 3.1 Cronograma de trabajo e iteraciones
El proyecto se estructuró en 3 etapas:

| Etapa | Actividad | Casos de prueba | Entregable / hito |
|---|---|:---:|---|
| **1. Calibración** | Diseño del prompt maestro con salida JSON forzada. Pruebas de temperatura y de límite de tokens. | 5 casos sintéticos | Prompt base validado en 1 sola llamada. |
| **2. Casos límite** | Pruebas con animales de perfil más difícil (adultos mayores, con condiciones médicas o de conducta especial). | 10 casos reales de refugio | Ajuste de restricciones y guardrails éticos del prompt. |
| **3. Integración T2I e interfaz** | Evaluación de fidelidad visual en la herramienta de generación de imagen y armado de la interfaz interactiva. | 5 pruebas cruzadas | Notebook funcional con formulario interactivo, imágenes reales incluidas. |

### 3.2 Protocolo de validación: veracidad del contenido y fidelidad visual
Para no engañar al adoptante ni distorsionar la realidad del animal rescatado, se establecen los siguientes controles:

**Fidelidad del texto (guardrails éticos)**
* *Sin omisión médica:* si la ficha indica medicación crónica o necesidades especiales (por ejemplo, paseo con bozal), el prompt obliga a incluirlo de forma clara y amigable en la "ficha rápida" — nunca se omite.
* *Sin promesas absolutas no verificadas:* se instruye al modelo a no usar frases como "nunca ladra" o "se lleva bien con todos" salvo que estén confirmadas explícitamente en el input.
* *Revisión humana antes de publicar:* el protocolo establece que un voluntario revisa el texto generado contra la ficha original antes de publicarlo — el modelo asiste la redacción, no reemplaza la verificación humana.

**Fidelidad visual (restricciones del prompt Texto-a-Imagen)**
* *Inyección obligatoria de rasgos distintivos:* el prompt T2I no describe un animal genérico; extrae del input el color de manto, tipo de orejas, manchas o cicatrices y tamaño exacto, y los incluye explícitamente en el prompt de imagen.
* *Transparencia con el adoptante:* toda imagen generada se publica junto con la leyenda "Representación artística del potencial en hogar de [Nombre]", dejando claro que es una recreación conceptual y no una fotografía real del animal.
* *Chequeo de coherencia:* antes de publicar, se compara la imagen generada contra la ficha (tamaño, color, rasgos) para descartar resultados que distorsionen la apariencia real.

### 3.3 Análisis de costos y límites de acceso a las herramientas
* **Modelo Texto-a-Texto (OpenAI `gpt-4o-mini`):** ≈450 tokens de input + ≈400 tokens de output (JSON) por perfil → **menos de USD 0,001 por animal**. Un refugio con 50 ingresos mensuales gastaría menos de USD 0,05 al mes. La notebook muestra el conteo real de tokens de cada consulta (ver sección 5).
* **Modelo Texto-a-Imagen:** siguiendo la recomendación de la cátedra (DALL-E dejó de tener nivel gratuito), **no se usa la API de generación de imagen**. El prompt T2I que produce el LLM se escribe manualmente en **[Pollinations.ai](https://pollinations.ai)**, una herramienta gratuita y sin registro que genera la imagen a partir del texto — sin costo, sin cuenta y sin límite diario de créditos, a diferencia de Nightcafe/Leonardo AI (créditos gratuitos limitados) o DALL-E 3 vía API (USD 0,04 por imagen).
* **Consultas a la API por animal procesado:** 1 sola llamada al modelo de texto (few-shot + JSON forzado). La imagen se genera aparte, de forma manual, sin ningún costo de API.

### 3.4 Métricas de éxito
Para evaluar si la solución cumple su objetivo en un refugio real, se proponen las siguientes métricas de seguimiento:

* **Tiempo promedio de redacción por animal:** de ~15-20 minutos manuales a menos de 1 minuto con la POC (carga del formulario + 1 llamada a la API).
* **Costo mensual de generación de contenido:** menor a USD 0,05 para un refugio con 50 ingresos/mes (ver sección 3.3).
* **Tasa de adopción de animales con perfil generado por la POC** vs. animales con ficha tradicional, medida en el tiempo que tardan en ser adoptados desde su publicación.
* **Consultas de API por animal:** debe mantenerse en 1 (evita que el costo escale si se agregan más funcionalidades).

---

## 4. Herramientas y técnicas de prompting utilizadas

1. **System Instructions (rol experto):** se le asigna al modelo el rol de redactor publicitario ético especializado en bienestar animal, para fijar el marco de comportamiento antes de cualquier instrucción del usuario.
2. **Few-Shot Prompting:** se provee un ejemplo completo de `input → output JSON` para fijar tono, estructura y nivel de detalle sin ambigüedad, reduciendo la necesidad de reintentos.
3. **Structured Output (JSON forzado):** mediante `response_format={"type": "json_object"}` se obtiene una respuesta parseable directamente en Python, sin post-procesamiento de texto libre.
4. **Fast Prompting (ejecución en una sola pasada):** se unifican 3 tareas (biografía, copy de redes y prompt de imagen) en una única consulta a la API, en vez de 3-4 llamadas independientes como en la Preentrega 1. Esto reduce la latencia total y el consumo de tokens por animal procesado.

Se eligieron estas técnicas —y no, por ejemplo, *Chain-of-Thought* extenso o múltiples llamadas encadenadas— porque la tarea es de generación directa (no requiere razonamiento paso a paso visible) y el objetivo explícito del proyecto es minimizar el número de consultas a la API por motivos de costo y tiempo de respuesta.

Para el modelo **Texto-a-Imagen** se usó **prompting descriptivo estructurado** (sujeto + rasgos distintivos + ambientación + especificaciones fotográficas de lente/luz), escrito manualmente en Pollinations.ai, sin llamadas a ninguna API de pago.

---

## 5. Implementación

La implementación completa está en [`fast_prompting_poc.ipynb`](./fast_prompting_poc.ipynb). Resumen de las piezas clave:

* **Prompt maestro (`SYSTEM_PROMPT`):** define el rol, las reglas éticas y el esquema JSON de salida exacto que debe respetar el modelo.
* **Ejemplo few-shot (`FEW_SHOT_EXAMPLE_INPUT` / `FEW_SHOT_EXAMPLE_OUTPUT`):** fija el estilo esperado mostrando un caso resuelto de punta a punta (el animal "Milo").
* **Función `generar_perfil_adopcion()`:** arma los mensajes (system + few-shot + input real), hace **una única llamada** a `gpt-4o-mini` con `response_format={"type": "json_object"}`, y devuelve el JSON ya parseado **junto con el conteo real de tokens consumidos** (`prompt_tokens` / `completion_tokens` / `total_tokens`). Si la API no está disponible (sin `OPENAI_API_KEY` configurada), cae a un modo demo offline que genera una salida de ejemplo con los mismos datos, para poder mostrar el funcionamiento sin necesidad de credenciales.
* **Interfaz interactiva (`ipywidgets`):** formulario con nombre, especie, edad, tamaño, rasgos, personalidad y salud del animal, más un botón que dispara `generar_perfil_adopcion()` y muestra el resultado formateado (biografía, copy de redes, ficha resumen, prompt de imagen y tokens consumidos).

### Prompts Texto-a-Imagen y su resultado real

Como se indicó en la sección 3.3, no se llama a ninguna API de imagen: el prompt T2I generado por el LLM se escribió manualmente en [Pollinations.ai](https://pollinations.ai) y estas son las imágenes obtenidas:

**Prompt (perro "Milo"):**
```
A professional studio photo of a large mixed-breed dog, short glossy jet-black coat with a
distinct white patch on chest, gentle expressive brown eyes, floppy ears, relaxed lying posture
on a soft woolen rug in a sunlit living room. Warm ambient lighting, indoor plants in blurred
background, 85mm portrait lens, f/2.0, photorealistic, 8k resolution.
```
![Retrato conceptual de Milo, perro mestizo negro con pecho blanco, generado por IA](./imagenes/milo_retrato.jpg)

**Prompt (gato, ficha "Roco" adaptada a felino):**
```
Editorial lifestyle photography of a fluffy black and white tuxedo cat resting peacefully on a
knit cream-colored blanket on a window sill. Soft morning sunlight filtering through, highlighting
the green clarity of the cat's eyes and the texture of its fur. Peaceful, cozy, homey vibe, warm
color palette, sharp focus on the cat's face, shallow depth of field, hyper-realistic, 8k resolution.
```
![Retrato conceptual de un gato tuxedo blanco y negro, generado por IA](./imagenes/gato_retrato.jpg)

*Representación artística generada por IA — no es una fotografía real de un animal del refugio.*

### Cómo ejecutarlo
```bash
pip install -r requirements.txt
export OPENAI_API_KEY="tu-api-key"   # opcional: sin key, corre en modo demo
jupyter notebook fast_prompting_poc.ipynb
```

---

## 6. Resultados

Al ejecutar la notebook (con o sin `OPENAI_API_KEY`), la POC entrega, a partir de un formulario de 7 campos, un perfil completo de adopción en **una sola llamada a la API**:

* **Título de catálogo** (gancho, máximo 6 palabras).
* **Biografía narrativa** de 2-3 párrafos, sin promesas no verificadas y sin omitir condiciones de salud.
* **Copy para redes sociales** con hashtags, listo para publicar.
* **Ficha resumen** estructurada (edad, tamaño, compatibilidad, salud).
* **Prompt T2I calibrado** a los rasgos reales del animal, listo para pegar en una herramienta gratuita de imagen.
* **Conteo de tokens** de la consulta (o aviso de modo demo si no hay API key).

Los dos prompts T2I probados en la sección 5 (perro y gato) muestran que el modelo de imagen respeta los rasgos distintivos indicados en el prompt (color de manto, marcas, entorno hogareño cálido), cumpliendo el objetivo de generar una representación digna y coherente con la ficha del animal — no un animal genérico.

**¿Se logra la solución esperada?** Sí: la POC reduce un proceso manual de redacción y búsqueda de fotos (~15-20 minutos por animal) a menos de un minuto de carga de formulario más una consulta a la API, manteniendo el costo por debajo de los 5 centavos de dólar mensuales para un refugio de tamaño mediano, sin necesidad de conocimientos técnicos por parte de quien lo usa.

---

## 7. Conclusiones

* El proyecto cumple los objetivos planteados: se identificó una problemática real y acotada (comunicación de adopciones en refugios), se diseñó una solución basada en modelos de IA generativa, y se implementó una POC funcional que la resuelve con una única llamada a la API de texto y sin costo en la generación de imagen.
* Las técnicas de *Fast Prompting* (System Instructions + Few-Shot + JSON forzado + ejecución en una pasada) demostraron ser más eficientes que la ejecución manual de prompts independientes de la Preentrega 1, tanto en tiempo como en consumo de tokens — algo que ahora la propia notebook evidencia con el conteo de tokens por consulta.
* El protocolo de validación ética y de fidelidad visual es, junto con el modo demo offline, el diferencial más importante del proyecto: permite confiar en que el contenido generado no engañe al adoptante ni distorsione la realidad del animal.
* Como líneas de mejora futuras quedan: automatizar la comparación entre la ficha original y el texto generado (chequeo automático de omisiones), y medir en un caso real la tasa de adopción antes/después de usar la POC, tal como se propone en las métricas de éxito (sección 3.4).

---

## 8. Referencias

* Documentación oficial de la API de OpenAI (Chat Completions, JSON mode): https://platform.openai.com/docs
* Pollinations.ai — generación de imágenes gratuita y sin registro: https://pollinations.ai
* Nightcafe Studio — alternativa freemium de generación de imágenes: https://nightcafe.studio
* Contenidos y material recomendado del curso *Inteligencia artificial: Generación de Prompts*.
* Preentrega 1 y Entrega 2 de este mismo proyecto (historial de iteración disponible en los commits de este repositorio).

---

## 9. Estructura del repositorio
* `fast_prompting_poc.ipynb` — Jupyter Notebook con la implementación de la POC, interfaz interactiva, conteo de tokens y prompts/imágenes T2I.
* `imagenes/` — imágenes reales generadas a partir de los prompts T2I (`milo_retrato.jpg`, `gato_retrato.jpg`).
* `README.md` — esta documentación: resumen, propuesta, metodología, resultados y conclusiones.
* `requirements.txt` — dependencias del proyecto (`openai`, `ipywidgets`).
* `.gitignore` — excluye archivos de entorno local (por ejemplo, `.env` con la API key) para que nunca se suban credenciales al repositorio.
