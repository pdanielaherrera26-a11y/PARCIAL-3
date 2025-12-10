# RAG: Sistema Inteligente de Búsqueda de Recetas

## Acerca del Proyecto

Este proyecto implementa un sistema RAG (Retrieval-Augmented Generation) diseñado para buscar y generar explicaciones detalladas de recetas culinarias. El sistema combina técnicas avanzadas de recuperación de información con modelos de lenguaje natural para proporcionar respuestas precisas y contextualizadas sobre preparación de alimentos.

Desarrollado como parte del Parcial 3 de la asignatura Inteligencia Artificial Aplicada a la Economía (HE2), este RAG demuestra cómo la inteligencia artificial puede facilitar el acceso a conocimiento culinario de manera intuitiva y en el idioma del usuario.

## Integrantes del Equipo

- **Laura Sofía Vera** - 201123145
- **Juan Camilo Arrieta** - 202121839
- **Juan Pablo Camacho** - 202110977
- **Daniela Herrera** - 202113704

## Arquitectura del Sistema

Nuestro RAG implementa una arquitectura de dos etapas que combina recuperación eficiente con generación inteligente:

### Etapa 1: Encoder (Recuperación)

La primera etapa se encarga de encontrar las recetas más relevantes en nuestra base de datos vectorial. Utilizamos el modelo `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`, un modelo SBERT multilingüe optimizado que transforma texto en vectores de 384 dimensiones. Este modelo fue seleccionado específicamente por su capacidad para entender el significado semántico de textos cortos como recetas, y por su soporte multilingüe que permite procesar consultas tanto en español como en inglés.

Los pasos de esta etapa incluyen la segmentación del texto en chunks de 512 caracteres con un solapamiento de 150 caracteres para preservar el contexto, la vectorización mediante batching de 128 documentos simultáneos para optimizar el rendimiento en GPU, y el almacenamiento en una base de datos FAISS que permite búsquedas de similitud ultra-rápidas mediante aproximación de vecinos más cercanos.

### Etapa 2: Decoder (Generación)

Una vez recuperadas las recetas relevantes, la segunda etapa genera una respuesta coherente y detallada. Empleamos Mistral-7B-Instruct-v0.2 a través de la API de Hugging Face, un modelo con 7 mil millones de parámetros especialmente entrenado para seguir instrucciones con precisión.

El proceso de generación incluye la construcción de un prompt que combina las recetas recuperadas con instrucciones específicas para traducción y explicación, la llamada a la API en la nube que evita limitaciones de memoria local, y la generación de respuestas estructuradas que incluyen título de la receta, lista de ingredientes traducidos al español con equivalencias claras, y pasos detallados de preparación.

## Tecnologías Utilizadas

El proyecto hace uso de un conjunto robusto de tecnologías especializadas. Para el manejo de datos trabajamos con Hugging Face Datasets para cargar el corpus de recetas, y LangChain para la segmentación inteligente de texto. En cuanto a modelos de inteligencia artificial, empleamos Sentence Transformers para los embeddings multilingües y Mistral-7B para la generación de texto. Para almacenamiento y búsqueda utilizamos FAISS como base de datos vectorial optimizada, mientras que PyTorch proporciona el soporte para aceleración con GPU.

## Dataset

Trabajamos con el dataset `corbt/all-recipes` disponible en Hugging Face, que contiene miles de recetas en inglés de diversas culturas culinarias. Para este proyecto académico, optimizamos el procesamiento utilizando un subconjunto de 2000 recetas, lo que permite tiempos de respuesta rápidos manteniendo una amplia variedad de opciones culinarias.

## Instalación y Uso

Para utilizar este proyecto, primero debes clonar el repositorio y abrir el notebook en Google Colab para aprovechar las GPUs gratuitas. Asegúrate de configurar tu token de Hugging Face en la variable `HF_TOKEN` del notebook, que puedes obtener gratuitamente en la configuración de tu cuenta de Hugging Face. Luego ejecuta las celdas secuencialmente comenzando por la carga del dataset, seguida por la vectorización que puede tomar algunos minutos en la primera ejecución, y finalmente las consultas de prueba.

## Ejemplos de Uso

El sistema puede responder a diversas consultas culinarias. Por ejemplo, si preguntas "¿Cómo hago un Mac and Cheese clásico?", el sistema recuperará recetas similares de macarrones con queso y generará una explicación completa con ingredientes traducidos y pasos detallados. Si consultas "Necesito una receta de brownies con cacao", obtendrás múltiples opciones de brownies con las proporciones exactas y técnicas de preparación. Para consultas como "Tengo pollo y brócoli, ¿qué cocino?", el sistema buscará recetas que combinen ambos ingredientes y sugerirá preparaciones completas.

## Características Destacadas

Nuestro sistema ofrece recuperación semántica avanzada que entiende la intención de la consulta más allá de palabras clave simples. Proporciona traducción inteligente al español con equivalencias culturales apropiadas, como convertir "stick of butter" en "barra de mantequilla" o "cocoa" en "cacao en polvo". La arquitectura optimizada para GPU permite respuestas en segundos incluso con miles de recetas. El sistema mantiene contexto completo de las recetas gracias al solapamiento entre chunks, y utiliza prompts especializados que evitan traducciones incorrectas comunes en contextos culinarios.

## Limitaciones Conocidas

Es importante entender que el sistema tiene algunas limitaciones que identificamos durante el desarrollo. Las consultas subjetivas como "la mejor receta" o "recomiéndame algo que me guste" pueden producir resultados menos precisos, ya que el sistema no aprende preferencias personales del usuario. El rendimiento depende significativamente del tamaño del corpus, por lo que con datasets pequeños las opciones son más limitadas. El sistema no puede validar disponibilidad de ingredientes en tu cocina ni adaptarse a restricciones alimentarias sin que las especifiques en la consulta. Tampoco verifica combinaciones de sabores en tiempo real, sino que se basa en recetas existentes probadas.

## Estructura del Proyecto

El repositorio está organizado de manera clara. El archivo `Final_IA.ipynb` contiene el notebook principal con todo el código funcional, mientras que `Final. - IA.pdf` incluye la documentación completa del proyecto explicando la arquitectura y decisiones de diseño. El directorio `recetas_faiss_index/` almacena la base de datos vectorial pre-procesada, y este `README.md` proporciona la documentación general del proyecto.

## Posibles Mejoras Futuras

Vemos varias direcciones prometedoras para extender este trabajo. Se podría implementar un sistema de feedback que aprenda de las preferencias del usuario a lo largo del tiempo, expandir el dataset para incluir recetas específicamente colombianas o de otras regiones latinoamericanas, agregar filtros por restricciones alimentarias como vegano, sin gluten o bajo en sodio, implementar un sistema de recomendación basado en ingredientes disponibles mediante visión computacional, y añadir validación nutricional automática de las recetas generadas.

## Referencias

Este proyecto se construyó sobre herramientas y recursos de código abierto. El dataset proviene de Hugging Face Datasets disponible en `corbt/all-recipes`. Utilizamos el modelo de embeddings de Sentence Transformers documentado en su página oficial, y Mistral-7B-Instruct para generación de texto. La implementación de búsqueda vectorial se realizó con FAISS de Facebook Research, y LangChain proporcionó las herramientas para procesamiento de texto.

## Licencia

Este proyecto fue desarrollado con fines académicos para la Universidad de los Andes, Colombia, durante el curso de Inteligencia Artificial Aplicada a la Economía.

---

**Fecha de Entrega**: Diciembre 11, 2025  
**Institución**: Universidad de los Andes  
**Curso**: HE2 - Inteligencia Artificial Aplicada a la Economía
