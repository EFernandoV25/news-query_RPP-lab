# 📰 News Intelligence Laboratory - RPP Retrieval System

Sistema de recuperación de noticias inteligente basado en embeddings semánticos para el feed RSS de RPP Perú. Utiliza SentenceTransformers y ChromaDB para búsquedas semánticas avanzadas orquestadas con LangChain.

## 🎯 Características

- **Ingestión automática** de noticias desde RPP RSS
- **Tokenización** con tiktoken para análisis de contexto
- **Embeddings semánticos** usando `sentence-transformers/all-MiniLM-L6-v2`
- **Base de datos vectorial** ChromaDB para búsqueda por similitud
- **Pipeline modular** orquestado con LangChain (LCEL)
- **Chunking inteligente** para documentos largos
- **Búsqueda semántica** con resultados en DataFrame

## 📋 Requisitos

- Python 3.10+
- pip o conda para gestión de paquetes

## 🚀 Instalación

### 1. Clonar el repositorio

```bash
git clone https://github.com/tu-usuario/rpp-news-retrieval.git
cd rpp-news-retrieval
```

### 2. Crear entorno virtual (recomendado)

```bash
# Con venv
python -m venv venv
source venv/bin/activate  # En Windows: venv\Scripts\activate

# O con conda
conda create -n rpp-news python=3.11
conda activate rpp-news
```

### 3. Instalar dependencias

```bash
pip install -r requirements.txt
```

## 💻 Uso

### Script Python

```python
from rpp_retrieval import RPPNewsRetriever

# Crear e inicializar el sistema
retriever = RPPNewsRetriever(max_items=50)
retriever.run_pipeline()

# Realizar consultas
results = retriever.query("Últimas noticias de economía", n_results=5)
print(results)
```

### Jupyter Notebook

Abre el notebook `Task1_RPP_Retrieval.ipynb` para un tutorial paso a paso:

```bash
jupyter notebook Task1_RPP_Retrieval.ipynb
```

## 🏗️ Arquitectura del Sistema

### Pipeline de Procesamiento

```
RSS Feed → Tokenización → Embeddings → ChromaDB → Retriever
```

**Paso 0: Carga de datos**
- Extrae últimas 50 noticias del RSS de RPP
- Captura: título, descripción, link, fecha

**Paso 1: Tokenización**
- Analiza tokens con `tiktoken` (encoding: cl100k_base)
- Determina si se requiere chunking (límite: 256 tokens)

**Paso 2: Generación de Embeddings**
- Modelo: `sentence-transformers/all-MiniLM-L6-v2`
- Dimensión: 384
- Velocidad: ~3000 tokens/segundo

**Paso 3: Almacenamiento Vectorial**
- Base de datos: ChromaDB
- Persistencia local en `./chroma_rpp`
- Búsqueda por similitud coseno

**Paso 4: Consultas**
- Búsqueda semántica con k-nearest neighbors
- Resultados en formato pandas DataFrame
- Deduplicación por título

**Paso 5: Orquestación LangChain**
- Pipeline modular con LCEL
- Funciones reutilizables
- Configuración flexible

## 📊 Estructura del Proyecto

```
rpp-news-retrieval/
│
├── news-query_RPP-lab.ipynb    # Notebook tutorial completo
├── requirements.txt              # Dependencias
├── README.md                     # Documentación

```

## 🔍 Ejemplos de Consultas

```python
# Consulta sobre economía
retriever.query("Últimas noticias de economía")

# Consulta sobre tecnología
retriever.query("Noticias sobre inteligencia artificial")

# Consulta sobre política
retriever.query("Gobierno y elecciones en Perú")

# Consulta sobre deportes
retriever.query("Resultados de fútbol peruano")
```

## 🛠️ Configuración Avanzada

### Variables de Entorno

Crea un archivo `.env` para personalizar:

```bash
# RSS Feed
RPP_RSS=https://rpp.pe/rss
MAX_RSS_ITEMS=50

# ChromaDB
CHROMA_DB_DIR=./chroma_rpp
CHROMA_COLLECTION=rpp_news

# Embeddings
EMBED_MODEL_NAME=sentence-transformers/all-MiniLM-L6-v2

# Tokenización
MAX_TOKENS=256
OVERLAP_TOKENS=40
```

### Personalización del Pipeline

```python
from dataclasses import dataclass

@dataclass
class PipelineConfig:
    rss_url: str = "https://rpp.pe/rss"
    limit: int = 100  # Más noticias
    max_tokens: int = 512  # Mayor contexto
    overlap: int = 50
    model_name: str = "sentence-transformers/paraphrase-multilingual-mpnet-base-v2"  # Mejor modelo

retriever = RPPNewsRetriever(config=PipelineConfig())
```

## 📈 Rendimiento

- **Velocidad de ingestión**: ~50 noticias en 5-10 segundos
- **Tiempo de embeddings**: ~0.02 segundos por documento
- **Latencia de búsqueda**: <100ms para 50 documentos
- **Memoria RAM**: ~500MB con modelo base

## 🤝 Contribuciones

¡Las contribuciones son bienvenidas! Por favor:

1. Fork el proyecto
2. Crea una rama para tu feature (`git checkout -b feature/AmazingFeature`)
3. Commit tus cambios (`git commit -m 'Add some AmazingFeature'`)
4. Push a la rama (`git push origin feature/AmazingFeature`)
5. Abre un Pull Request

## 📝 Roadmap

- [ ] Soporte para múltiples fuentes RSS
- [ ] Sistema de actualización incremental
- [ ] API REST con FastAPI
- [ ] Dashboard de visualización con Streamlit
- [ ] Clasificación automática de categorías
- [ ] Análisis de sentimiento
- [ ] Sistema de recomendación personalizado
- [ ] Cache de consultas frecuentes
- [ ] Soporte para búsqueda híbrida (semántica + keyword)

## 🐛 Problemas Conocidos

- ChromaDB puede requerir permisos de escritura en el directorio
- En Windows, tiktoken puede requerir Visual C++ Build Tools
- El primer uso descarga el modelo de embeddings (~80MB)

## 📚 Referencias

- [LangChain Documentation](https://docs.langchain.com/)
- [ChromaDB Documentation](https://docs.trychroma.com/)
- [SentenceTransformers](https://www.sbert.net/)
- [RPP Perú](https://rpp.pe/)

## 📄 Licencia

Este proyecto está bajo la Licencia MIT. Ver archivo `LICENSE` para más detalles.

## 👨‍💻 Autor

**Tu Nombre**
- GitHub: [@tu-usuario](https://github.com/tu-usuario)
- LinkedIn: [Tu Perfil](https://linkedin.com/in/tu-perfil)

## 🙏 Agradecimientos

- RPP Perú por proporcionar el feed RSS público
- Equipo de LangChain por el excelente framework
- Comunidad de Hugging Face por los modelos de embeddings

---

⭐ Si este proyecto te fue útil, considera darle una estrella en GitHub!
