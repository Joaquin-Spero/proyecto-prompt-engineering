# Automatización de Análisis y Creación de Contenido para Streaming

**Alumno:** Joaquin SPero
**Curso:** Inteligencia artificial: Generación de Prompts
**Comisión:** 95920

## Resumen
Armé este proyecto para automatizar el análisis de métricas y la creación de recursos gráficos para mis canales de Twitch y YouTube. Utilizo modelos de lenguaje para procesar logs de chat, herramientas de análisis de datos con IA para estadísticas de transmisiones, y modelos de generación de imágenes para crear miniaturas.

## Introducción
**Nombre del proyecto:** Automatización de Análisis y Creación de Contenido para Streaming.
**Presentación del problema a abordar:** Como creador de contenido, invierto horas analizando qué momentos del stream funcionaron mejor basándome en el chat y las métricas de retención, para luego adaptar ese contenido a YouTube. Es un proceso manual, repetitivo y lento.
**Desarrollo de la propuesta de solución:** Mi solución se vincula directamente al uso de modelos de IA. Utilizo la API de Gemini (como alternativa a OpenAI por su capa gratuita) y la Librería Pandas IA para procesar datos tabulares y texto no estructurado. Además, integro un modelo texto a imagen para la generación rápida de miniaturas basándome en los títulos generados por la IA de texto.
**Justificación de la viabilidad del proyecto:** El proyecto es técnica y económicamente viable. Utilizo Python, la API gratuita de Gemini (gemini-3.8-flash) y PandasAI ejecutados localmente. Para las imágenes, opté por usar Bing Image Creator, adjuntando el resultado en el repositorio. No requiere hardware de alta gama y el consumo de tokens es nulo gracias a la capa gratuita.

## Objetivos
* Reducir el tiempo de post-producción y análisis de mis streams.
* Automatizar el análisis de los momentos clave extrayendo insights del chat.
* Obtener datos sobre mi audiencia mediante consultas en lenguaje natural al CSV.
* Generar assets gráficos para las miniaturas mediante prompts optimizados.

## Metodología
Llevo a cabo el proyecto mediante una Prueba de Concepto (POC) en una Jupyter Notebook. Para lograr los objetivos implementé tres procedimientos clave:
1. **Procesamiento de logs:** Inyecto el log del chat a la API para extraer un resumen y posibles títulos.
2. **Análisis de datos:** Cargo las estadísticas en un DataFrame y utilizo PandasAI para hacerle consultas directas.
3. **Generación de imágenes:** Aplico prompts directamente en una herramienta online y renderizo la imagen resultante.

## Evaluación de costos y optimización de API
El código está optimizado al máximo para reducir el consumo. En toda la ejecución se realizan únicamente 2 consultas a la API: una al modelo texto a texto para analizar el log, y otra interna de PandasAI para el CSV. Al centralizar los pedidos y usar la capa gratuita de Gemini, el costo monetario de implementación es cero, haciendo el proyecto 100% rentable.

## Herramientas y tecnologías
Elegí estas técnicas de prompting para optimizar el contexto y maximizar la precisión:
* **Few shoot prompting:** Le doy a la API ejemplos exactos de cómo quiero que formatee el resumen del chat y los títulos, asegurando que mantenga mi estilo.
* **Zero prompt shooting:** Consulto el CSV directamente sin ejemplos previos, ya que el modelo interpreta perfectamente la estructura de datos tabulares.
* **Prompt iterativo y Prompting dirigido:** Apliqué iteraciones sucesivas ajustando la iluminación, la paleta de colores y el estilo hasta conseguir la miniatura ideal.

## Implementación
Desarrollé el código utilizando `google-genai` y armé un parche de la clase `GoogleGemini` de PandasAI para soportar el modelo `gemini-3.8-flash`. 

**Código de procesamiento de texto (Few shoot prompting):**
```python
import google.genai as genai

client = genai.Client(api_key="TU_API_KEY_GEMINI")
prompt = """
Sos un asistente experto en marketing de YouTube. Lee el siguiente log de chat de Twitch y generá 1 título clickbait y un breve resumen.
Ejemplo:
Chat: " user: me muero, user2: épico"
Salida: Título: ¡EL MEJOR MOMENTO DEL STREAM! - Resumen: La audiencia reaccionó con mucha emoción a una jugada.

Chat a analizar: [CONTENIDO_DEL_CHAT]
"""
response = client.models.generate_content(model='gemini-3.8-flash', contents=prompt)
```

**Código de análisis de métricas (Zero prompt shooting con Libreria Pandas IA):**
```python
import pandas as pd
from pandasai import SmartDataframe

df = pd.read_csv('stream_metrics.csv')
llm = GoogleGeminiFix(api_key="TU_API_KEY_GEMINI", model="gemini-3.8-flash")
df_smart = SmartDataframe(df, config={"llm": llm})
respuesta = df_smart.chat("¿Cuál fue el día con el pico máximo de espectadores y cuántos ingresos generó?")
```

**Prompt Texto a Imagen utilizado en Bing Image Creator:**
> "Una miniatura para YouTube de un stream de gaming, colores neón violeta y azul. En el centro un trofeo brillante con el logo de Twitch. Estilo render 3D de alta calidad, iluminación dramática."

**Salida (Imagen de resultado):**
![Miniatura generada](miniatura.png)

## Resultados
La implementación de la POC funciona correctamente y llega a la solución esperada. Pude procesar el archivo de chat y obtener un título clickbait. Con PandasAI, interactué con el CSV de métricas y obtuve el dato exacto del pico de espectadores directamente en lenguaje natural. La generación de la imagen me dio un recurso final integrado.

## Conclusiones
Validé que automatizar estas tareas con IA reduce drásticamente mi tiempo de trabajo offline. Se lograron todos los objetivos propuestos manteniendo los costos a cero. Comprobé que el uso de Few shoot prompting es fundamental para estructurar la salida en un solo intento, haciendo que el script no ejecute consultas innecesarias a la API.

## Referencias
* Documentación oficial de la API de Google Gemini (google-genai).
* Documentación oficial de PandasAI.
* Microsoft Bing Image Creator.