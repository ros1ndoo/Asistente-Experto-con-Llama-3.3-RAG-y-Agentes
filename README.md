# Entrega de Proyecto Final: IA Generativa - Asistente Experto con Llama 3.3, RAG y Agentes

**Alumno (UUID):** 57fdfdd2-0056-4bdb-8c56-da5a86b58304

## Descripción del Proyecto y Dominio Elegido
El presente repositorio contiene la entrega del proyecto final del módulo de IA Generativa. Se ha desarrollado un agente de inteligencia artificial experto en **Visualización de Datos y Comunicación Efectiva**. 

El objetivo principal de este agente es ayudar a los usuarios a preparar presentaciones de alto impacto, brindando asesoría sobre principios de diseño gráfico, reducción de carga cognitiva ("chartjunk"), estructuración de narrativas (Storytelling) y oratoria, todo ello fundamentado en una base de conocimiento propia.

## Tecnologías Implementadas
Cumpliendo con los requisitos de la rúbrica, el stack tecnológico incluye:
- **LLM:** Llama 3.3 70B (`llama-3.3-70b-versatile`), accedido a través de la infraestructura de Groq mediante API compatible con el protocolo OpenAI.
- **Embeddings:** Sentence-Transformers (`paraphrase-multilingual-MiniLM-L12-v2`), ejecutados localmente para generar representaciones vectoriales multilingües.
- **Base de Datos Vectorial:** ChromaDB.
- **Framework del Agente y Memoria:** LangGraph y LangChain.
- **Entorno de Ejecución:** Jupyter Notebook.

## Estructura del Repositorio
- `data/`: Contiene los 2 documentos de texto base (equivalentes a ~20 páginas) que conforman la base de conocimiento sobre el dominio elegido, redactados con base en el libro "Storytelling con Datos" y un resumen de "Hable como en TED" de Carmine Gallo.
- `agent_notebook.ipynb`: Jupyter Notebook principal (MVP) que contiene el pipeline de datos, la inicialización de ChromaDB, la definición del grafo de LangGraph, las pruebas de memoria y la celda interactiva.
- `requirements.txt`: Listado de dependencias para replicar el entorno.
- `.env`: Archivo de configuración para la API Key de Groq.

## Instrucciones de Ejecución para Evaluación

1.  **Preparación del Entorno:**
    ```bash
    # Se recomienda el uso de un entorno virtual
    python -m venv venv
    venv\\Scripts\\activate  # Windows
    pip install -r requirements.txt
    ```

2.  **Configuración de API Key:**
    Asegúrese de que el archivo `.env` en la raíz del proyecto contiene la clave de Groq:
    ```env
    GROQ_API_KEY=tu_api_key_de_groq
    ```

3.  **Ejecución del Notebook:**
    Abra `agent_notebook.ipynb` en Jupyter Notebook y ejecute todas las celdas secuencialmente. 
    - La primera ejecución descargará automáticamente el modelo de embeddings (~500MB). Las ejecuciones posteriores usarán la caché local.
    - Las celdas intermedias demostrarán 5 ejemplos predefinidos donde se evidencia el funcionamiento de la memoria y la extracción de la base de conocimiento (RAG).
    - La última celda desplegará un bucle interactivo para conversar con el agente en tiempo real.

## Justificación del System Prompt

El diseño del *System Prompt* se documenta y justifica a continuación para cumplir con los criterios de evaluación:

1.  **Rol Definido:** Se establece el rol de "experto consultor en Visualización de Datos y Comunicación Efectiva" para guiar el tono y la autoridad de las respuestas.
2.  **Enrutamiento Forzado a la Herramienta (RAG):** Se instruye al agente a invocar explícitamente la herramienta `busqueda_visualizacion_datos` al tratar los temas clave. Esto garantiza que las respuestas estén fundamentadas en los documentos indexados en ChromaDB y mitiga las alucinaciones.
3.  **Manejo de la Incertidumbre (Transparencia):** Se incluye la regla de admitir explícitamente si una información no se encuentra en la base de conocimiento.
4.  **Estructura Pedagógica:** Se le solicita aplicar la "regla del 3" en sus explicaciones, asegurando que las respuestas largas sean fáciles de leer y asimilar por el usuario final.

## Justificación de la Arquitectura del LLM

Se utiliza **Llama 3.3 70B** ejecutado a través de la infraestructura de **Groq**, que provee inferencia de alta velocidad mediante sus chips LPU (Language Processing Unit). La API de Groq es compatible con el protocolo OpenAI, lo que permite una integración transparente con LangChain manteniendo todas las funcionalidades requeridas (tool calling, streaming, memoria conversacional).

Para los embeddings, se utiliza **Sentence-Transformers** (`paraphrase-multilingual-MiniLM-L12-v2`) ejecutado localmente, lo que elimina dependencias de APIs externas para la vectorización, garantiza reproducibilidad y ofrece soporte nativo multilingüe para el contenido en español.
