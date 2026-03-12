# 🔍 Domain URL Finder

Herramienta en Python para obtener todas las URLs públicas de un dominio usando múltiples fuentes: `robots.txt`, `sitemap.xml`, Wayback Machine y Web Crawling. El resultado se exporta a un archivo **CSV**.

---

## 📋 Requisitos

- Python 3.10+
- pip

---

## ⚙️ Instalación

### 1. Clonar el repositorio

```bash
git clone https://github.com/tu-usuario/domain-url-finder.git
cd domain-url-finder
```

### 2. Crear y activar el entorno virtual

```bash
# Crear
python -m venv venv

# Activar en Linux/macOS
source venv/bin/activate

# Activar en Windows
venv\Scripts\activate
```

### 3. Instalar dependencias

```bash
pip install -r requirements.txt
```

### 4. Configurar el archivo `.env`

```bash
cp .env.example .env
```

Editar `.env` y definir al menos `TARGET_DOMAIN`:

```env
TARGET_DOMAIN=example.com
```

---

## 🚀 Uso

### Opción A — usando `.env` (recomendado)

```bash
python domain_url_finder.py
```

### Opción B — usando argumentos CLI

```bash
python domain_url_finder.py --domain example.com
```

Los argumentos CLI tienen **prioridad** sobre los valores del `.env`.

---

## 🔧 Parámetros

### Variables del `.env`

| Variable | Descripción | Default |
|---|---|---|
| `TARGET_DOMAIN` | Dominio a escanear *(requerido)* | — |
| `OUTPUT_FILE` | Nombre del archivo CSV de salida | `<domain>_urls.csv` |
| `SOURCES` | Fuentes separadas por coma | `robots,sitemap,wayback,crawl` |
| `SCAN_DEPTH` | Profundidad máxima del crawl | `2` |
| `MAX_PAGES` | Máximo de páginas a rastrear | `500` |
| `WAYBACK_LIMIT` | Límite de resultados de Wayback Machine | `5000` |

### Argumentos CLI

| Argumento | Descripción |
|---|---|
| `--domain` | Dominio objetivo (sobreescribe `TARGET_DOMAIN`) |
| `--output` | Archivo CSV de salida (sobreescribe `OUTPUT_FILE`) |
| `--sources` | Fuentes: `robots` `sitemap` `wayback` `crawl` |
| `--depth` | Profundidad del crawl |
| `--max-pages` | Máximo de páginas del crawl |
| `--wayback-limit` | Límite de URLs del Wayback Machine |

---

## 📂 Estructura del CSV generado

| Columna | Descripción |
|---|---|
| `url` | URL completa |
| `scheme` | Protocolo (`http` / `https`) |
| `subdomain` | Host completo (ej: `www.example.com`) |
| `path` | Ruta (ej: `/blog/articulo-1`) |
| `sources` | Fuentes que encontraron la URL, separadas por `\|` |
| `found_at` | Timestamp UTC de la ejecución |

Ejemplo de contenido:

```
url,scheme,subdomain,path,sources,found_at
https://example.com/about,https,example.com,/about,crawl|sitemap,2026-03-12 18:00:00 UTC
https://example.com/blog,https,example.com,/blog,wayback,2026-03-12 18:00:00 UTC
```

---

## 🔎 Fuentes de descubrimiento

| Fuente | Descripción |
|---|---|
| `robots` | Extrae rutas declaradas en `robots.txt` |
| `sitemap` | Parsea `sitemap.xml` e índices de sitemaps anidados |
| `wayback` | Consulta el CDX API de Wayback Machine (URLs históricas) |
| `crawl` | Rastreo BFS siguiendo enlaces `<a href>` en páginas HTML |

---

## 💡 Ejemplos

```bash
# Solo crawl y sitemap, mayor profundidad
python domain_url_finder.py --domain example.com --sources crawl sitemap --depth 3

# Solo Wayback Machine con más resultados
python domain_url_finder.py --domain example.com --sources wayback --wayback-limit 10000

# Guardar en archivo específico
python domain_url_finder.py --domain example.com --output reporte.csv
```

---

## 📁 Estructura del proyecto

```
domain-url-finder/
├── domain_url_finder.py   # Script principal
├── .env                   # Variables de entorno (no subir a git)
├── .env.example           # Plantilla de configuración
├── .gitignore
├── requirements.txt
└── README.md
```
