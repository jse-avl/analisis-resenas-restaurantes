# Plataforma de Análisis de Reseñas de Restaurantes — Panamá
# Restaurant Review Analytics Platform — Panama

---

## Español

Dashboard interactivo + pipeline ETL que extrae reseñas reales de Google Maps, analiza su sentimiento por aspecto (comida, servicio, precio) usando LLM, y agrupa restaurantes por desempeño mediante clustering no supervisado.

**Grupo 5:** Christian Duraty, Yireikis Abrego, Jorge Izarra, Jose Avila

### Stack Tecnológico

| Componente | Tecnología |
|-----------|-----------|
| Scraping | Playwright (Chromium headless) + Selenium (fallback) |
| Procesamiento | Python 3.13 + Pandas, NumPy |
| Sentimiento | LLM (Claude/OpenAI) con respaldo keyword-based |
| Clustering | Scikit-learn (KMeans + StandardScaler) |
| Dashboard | Streamlit + Matplotlib |

### Pipeline

| Paso | Script | Descripción |
|------|--------|-------------|
| 0 | `scraper_playwright.py` | Extrae reseñas de Google Maps vía Playwright |
| 1 | `descargar_datos.py` | Descarga real o fallback híbrido (Selenium + CSV) |
| 2 | `limpiar_datos.py` | Limpieza, deduplicación, clasificación por rating |
| 3 | `analizar_sentimiento.py` | Análisis de sentimiento por aspecto con LLM o keyword |
| 4 | `clustering.py` | KMeans sobre rating, popularidad y % positivo |
| 5 | `app.py` | Dashboard Streamlit con 4 pestañas |

**Origen:** 444 reseñas reales de 5 restaurantes panameños extraídas de Google Maps con Playwright.

### Instalación y Ejecución

```bash
pip install -r requirements.txt
playwright install chromium
python run_pipeline.py   # Pipeline completo
python -m streamlit run app.py  # Dashboard
```

### Dashboard — 4 Pestañas

1. **Resumen General** — Métricas globales, distribución de ratings, tabla de restaurantes con filtros
2. **Análisis por Restaurante** — Rating, sentimiento general y desglose por aspecto (comida/servicio/precio)
3. **Clustering** — Gráfico de dispersión (rating vs % positivo), tablas por cluster, estadísticas
4. **Recomendaciones** — Top 5 según scoring personalizado (rating + sentimiento + popularidad)

### Análisis de Sentimiento por Aspecto

Extrae sentimiento para **comida** (calidad, sabor), **servicio** (atención, rapidez) y **precio** (relación calidad-precio). Modalidad LLM (Claude/OpenAI) con alta precisión; fallback keyword-based con +50 términos por aspecto.

### Clustering

KMeans con 4 clusters sobre 3 features normalizadas: rating promedio (peso 0.4), % reseñas positivas (peso 0.6) y número de reseñas (peso 0.2). Etiquetas: Excelente → Bueno → Regular → Malo.

### Configuración

```env
ANTHROPIC_API_KEY=tu_clave_aqui
# OPENAI_API_KEY=tu_clave_aqui
```

---

## English

Interactive dashboard + ETL pipeline that extracts real Google Maps restaurant reviews, analyzes aspect-based sentiment (food, service, price) using LLM, and groups restaurants by performance via unsupervised clustering.

**Group 5:** Christian Duraty, Yireikis Abrego, Jorge Izarra, Jose Avila

### Tech Stack

| Component | Technology |
|-----------|-----------|
| Scraping | Playwright (Chromium headless) + Selenium (fallback) |
| Processing | Python 3.13 + Pandas, NumPy |
| Sentiment | LLM (Claude/OpenAI) with keyword-based fallback |
| Clustering | Scikit-learn (KMeans + StandardScaler) |
| Dashboard | Streamlit + Matplotlib |

### Pipeline

| Step | Script | Description |
|------|--------|-------------|
| 0 | `scraper_playwright.py` | Extracts reviews from Google Maps via Playwright |
| 1 | `descargar_datos.py` | Downloads real reviews or hybrid fallback (Selenium + CSV) |
| 2 | `limpiar_datos.py` | Cleaning, deduplication, rating classification |
| 3 | `analizar_sentimiento.py` | Aspect-based sentiment analysis with LLM or keyword |
| 4 | `clustering.py` | KMeans on rating, popularity, and % positive |
| 5 | `app.py` | Streamlit dashboard with 4 tabs |

**Data source:** 444 real reviews from 5 Panamanian restaurants scraped from Google Maps with Playwright.

### Installation & Usage

```bash
pip install -r requirements.txt
playwright install chromium
python run_pipeline.py   # Full pipeline
python -m streamlit run app.py  # Dashboard
```

### Dashboard — 4 Tabs

1. **Overview** — Global metrics, rating distribution, restaurant table with filters
2. **Per-Restaurant Analysis** — Rating, overall sentiment, aspect breakdown (food/service/price)
3. **Clustering** — Scatter plot (rating vs % positive), cluster tables, statistics
4. **Recommendations** — Top 5 based on custom scoring (rating + sentiment + popularity)

### Aspect-Based Sentiment Analysis

Extracts sentiment for **food** (quality, taste), **service** (attentiveness, speed), and **price** (value for money). LLM mode (Claude/OpenAI) for high accuracy; keyword-based fallback with 50+ terms per aspect.

### Clustering

KMeans with 4 clusters on 3 normalized features: average rating (weight 0.4), % positive reviews (weight 0.6), and review count (weight 0.2). Labels: Excellent → Good → Average → Poor.

### Configuration

```env
ANTHROPIC_API_KEY=your_key_here
# OPENAI_API_KEY=your_key_here
```

### Project Structure

```
├── scraper_playwright.py      # Playwright extraction
├── descargar_datos.py          # Step 1: download/merge (2 sources)
├── limpiar_datos.py            # Step 2: cleaning & transformation
├── analizar_sentimiento.py     # Step 3: aspect sentiment (LLM + keyword)
├── clustering.py               # Step 4: KMeans clustering
├── app.py                      # Streamlit dashboard
├── run_pipeline.py             # Pipeline orchestrator
├── requirements.txt            # Dependencies
├── .env                        # API keys (ANTHROPIC/OPENAI)
└── data/
    ├── reviews_real.csv        # Real reviews (scraper output)
    ├── reviews.csv             # Processed reviews (step 1)
    ├── reviews_clean.csv       # Clean reviews (step 2)
    ├── reviews_con_sentimiento.csv  # With sentiment analysis (step 3)
    ├── clusters.csv            # Restaurant clusters (step 4)
    └── restaurantes_panama.csv # Backup dataset (2nd source)
```
