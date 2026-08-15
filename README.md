# 🐾 Huellas con Futuro: Sistema de Catálogo Inteligente para Refugios

**Curso:** Inteligencia artificial: Generación de Prompts
**Entrega:** #2 - Fast Prompting en Acción: Desentrañando la Magia
**Autora:** Sofía Algamiz
**Nº de Comisión:** 96090

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
2. **Modelo Texto-a-Imagen (Nightcafe / Leonardo AI / DALL-E 3):** a partir de un prompt fotográfico calibrado con los rasgos físicos reales del animal (generado también por el LLM), produce una imagen conceptual que lo muestra en un entorno hogareño cálido, superando la limitación visual de las fotos tomadas en el refugio.

A diferencia de la Preentrega 1 —donde cada prompt (biografía emotiva, biografía en primera persona, retrato canino, retrato felino) se ejecutaba de forma independiente—, en esta etapa los tres resultados de texto (título, biografía, copy de redes) y el prompt de imagen se generan **en una única llamada a la API**, aplicando *Fast Prompting* para reducir tiempo de respuesta y costo por animal procesado.

### 1.4 Justificación de la viabilidad del proyecto

| Dimensión | Análisis de viabilidad |
|---|---|
| **Viabilidad técnica** | Alta. No requiere infraestructura de servidor ni programación compleja: se ejecuta en una notebook con la API de OpenAI (texto) y plataformas gratuitas o freemium (imagen). La complejidad recae en el diseño y calibración del prompt maestro. |
| **Tiempo y recursos** | Óptimo. El desarrollo, testeo e iteración de los prompts entra dentro de las semanas asignadas al curso (ver cronograma en la sección 3). Los insumos son solo fichas de prueba y acceso web/API a los modelos. |
| **Costos** | Muy bajo. Ver detalle de costos en la sección 3.3 — del orden de centavos de dólar por mes para un refugio con decenas de ingresos mensuales. |
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
* **Analizar si las técnicas de Fast Prompting mejoran la propuesta de la Preentrega 1**: la respuesta es sí — se pasa de 2 prompts T2T + 2 prompts T2I ejecutados manualmente y por separado, a un único prompt maestro (few-shot + JSON forzado) que genera los cuatro resultados en una sola consulta, reduciendo tiempo de carga por animal y consumo de tokens.

---

## 3. Metodología

### 3.1 Cronograma de trabajo e iteraciones
El proyecto se estructura en 3 etapas:

| Etapa | Actividad | Casos de prueba | Entregable / hito |
|---|---|:---:|---|
| **1. Calibración** | Diseño del prompt maestro con salida JSON forzada. Pruebas de temperatura y de límite de tokens. | 5 casos sintéticos | Prompt base validado en 1 sola llamada. |
| **2. Casos límite** | Pruebas con animales de perfil más difícil (adultos mayores, con condiciones médicas o de conducta especial). | 10 casos reales de refugio | Ajuste de restricciones y guardrails éticos del prompt. |
| **3. Integración T2I e interfaz** | Evaluación de fidelidad visual en herramientas de generación de imagen y armado de la interfaz interactiva. | 5 pruebas cruzadas | Notebook funcional con formulario interactivo. |

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
* **Modelo Texto-a-Texto (OpenAI `gpt-4o-mini`):** ≈450 tokens de input + ≈400 tokens de output (JSON) por perfil → **menos de USD 0,001 por animal**. Un refugio con 50 ingresos mensuales gastaría menos de USD 0,05 al mes.
* **Modelo Texto-a-Imagen:**
  * *Nightcafe / Leonardo AI (plan gratuito):* entre 5 y 150 créditos gratuitos diarios, suficientes para cubrir el catálogo mensual de un refugio sin costo.
  * *DALL-E 3 (API de OpenAI):* USD 0,04 por imagen estándar (1024×1024). Se deja como alternativa opcional de mayor calidad, ya que dejó de tener nivel gratuito (ver nota del curso).
* **Consultas a la API por animal procesado:** 1 sola llamada al modelo de texto (few-shot + JSON forzado). La generación de imagen se hace de forma manual en la herramienta gratuita elegida, copiando el prompt T2I que produce la notebook — así se evita cualquier costo de API de imagen en la POC.

---

## 4. Herramientas y técnicas de prompting utilizadas

1. **System Instructions (rol experto):** se le asigna al modelo el rol de redactor publicitario ético especializado en bienestar animal, para fijar el marco de comportamiento antes de cualquier instrucción del usuario.
2. **Few-Shot Prompting:** se provee un ejemplo completo de `input → output JSON` para fijar tono, estructura y nivel de detalle sin ambigüedad, reduciendo la necesidad de reintentos.
3. **Structured Output (JSON forzado):** mediante `response_format={"type": "json_object"}` se obtiene una respuesta parseable directamente en Python, sin post-procesamiento de texto libre.
4. **Fast Prompting (ejecución en una sola pasada):** se unifican 3 tareas (biografía, copy de redes y prompt de imagen) en una única consulta a la API, en vez de 3-4 llamadas independientes como en la Preentrega 1. Esto reduce la latencia total y el consumo de tokens por animal procesado.

Se eligieron estas técnicas —y no, por ejemplo, *Chain-of-Thought* extenso o múltiples llamadas encadenadas— porque la tarea es de generación directa (no requiere razonamiento paso a paso visible) y el objetivo explícito del proyecto es minimizar el número de consultas a la API por motivos de costo y tiempo de respuesta.

---

## 5. Implementación

La implementación completa está en [`fast_prompting_poc.ipynb`](./fast_prompting_poc.ipynb). Resumen de las piezas clave:

* **Prompt maestro (`SYSTEM_PROMPT`):** define el rol, las reglas éticas y el esquema JSON de salida exacto que debe respetar el modelo.
* **Ejemplo few-shot (`FEW_SHOT_EXAMPLE_INPUT` / `FEW_SHOT_EXAMPLE_OUTPUT`):** fija el estilo esperado mostrando un caso resuelto de punta a punta.
* **Función `generar_perfil_adopcion()`:** arma los mensajes (system + few-shot + input real), hace **una única llamada** a `gpt-4o-mini` con `response_format={"type": "json_object"}`, y devuelve el JSON ya parseado. Si la API no está disponible (sin `OPENAI_API_KEY` configurada), cae a un modo demo offline que genera una salida de ejemplo con los mismos datos, para poder mostrar el funcionamiento sin necesidad de credenciales.
* **Interfaz interactiva (`ipywidgets`):** formulario con nombre, especie, edad, tamaño, rasgos, personalidad y salud del animal, más un botón que dispara `generar_perfil_adopcion()` y muestra el resultado formateado (biografía, copy de redes, ficha resumen y prompt de imagen).

### Cómo ejecutarlo
```bash
pip install -r requirements.txt
export OPENAI_API_KEY="tu-api-key"   # opcional: sin key, corre en modo demo
jupyter notebook fast_prompting_poc.ipynb
```

---

## 6. Estructura del repositorio
* `fast_prompting_poc.ipynb` — Jupyter Notebook con la implementación de la POC, interfaz interactiva y casos de prueba.
* `README.md` — esta documentación: propuesta, metodología y justificación.
* `requirements.txt` — dependencias del proyecto (`openai`, `ipywidgets`).
* `.gitignore` — excluye archivos de entorno local (por ejemplo, `.env` con la API key) para que nunca se suban credenciales al repositorio.
