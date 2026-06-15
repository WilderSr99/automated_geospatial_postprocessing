# Proyecto de Postprocesamiento Geoespacial de Viviendas Detectadas

![Python](https://img.shields.io/badge/Python-3.9%2B-blue?logo=python&logoColor=white)
![GeoPandas](https://img.shields.io/badge/GeoPandas-0.12%2B-139C5A?logo=geopandas&logoColor=white)
![Shapely](https://img.shields.io/badge/Shapely-2.0%2B-white?logo=python&logoColor=black)
![NetworkX](https://img.shields.io/badge/NetworkX-3.0%2B-orange?logo=networkx&logoColor=white)

Este proyecto organiza un flujo de trabajo automatizado en Python para el postprocesamiento, enriquecimiento territorial y estructuración de datos de viviendas detectadas mediante modelos de Inteligencia Artificial en imágenes satelitales. El pipeline resuelve la falta de contexto político-administrativo de las detecciones brutas, vinculándolas con la nomenclatura oficial y generando métricas de proximidad geográfica.

---

## 📌 Descripción del Problema y Objetivo

Los modelos de segmentación e IA son altamente eficientes localizando geometrías de viviendas individuales, pero suelen entregar polígonos aislados en formato bruto. Estos elementos carecen de atributos clave para la gestión pública y el análisis sociodemográfico, tales como el **centro poblado asociado**, el contexto territorial y el código **UBIGEO** (Código de Ubicación Geográfica Oficial en el Perú).

**Objetivo del Programa:** Automatizar la asignación de viviendas detectadas a sus centros poblados correspondientes por criterios de mínima distancia, calcular sus centroides, agrupar los resultados por UBIGEO, generar subproductos geográficos avanzados (como Árboles de Expansión Mínima y áreas de influencia) y exportar los reportes analíticos tanto en formato espacial como tabular.

---

## 🗺️ Arquitectura de Datos (Entradas y Salidas)

El script interactúa con formatos de almacenamiento estándar en sistemas de información geográfica (SIG):

### Capas de Entrada (`Inputs`)
* `viviendas_detectadas.gpkg`: Capa vectorial nativa (Polígonos) producto de las inferencias del modelo de IA.
* `centros_poblados.gpkg`: Capa de referencia institucional (Puntos o Polígonos) que contiene el atributo político-administrativo `UBIGEO`.
* `area_interes.gpkg` *(Opcional)*: Polígono de máscara para delimitar espacialmente la zona de estudio o bounding box de interés.

> ⚙️ **Nota de Configuración:** Las rutas de estos archivos se configuran de manera relativa o absoluta según el entorno local en los scripts o notebooks correspondientes.

### Capas de Salida (`Outputs`)
* **`resultados_postproceso.gpkg`** (Base de datos espacial GeoPackage con múltiples capas):
    * `viviendas_asignadas`: Polígonos de viviendas con los atributos heredados de su centro poblado y UBIGEO respectivo.
    * `centroides_asignados`: Puntos geométricos calculados en base a la masa de las viviendas para agilidad de procesamiento y visualización tabular.
    * `mst_por_ubigeo`: Capa de líneas que representa el **Árbol de Expansión Mínima** (Minimum Spanning Tree) que conecta los centroides, útil para evaluar conectividad y densidad lineal.
    * `buffers_por_ubigeo`: Polígonos envolventes que representan el área de influencia simple de las viviendas por grupo geográfico.
* **`resumen_viviendas_por_ubigeo.csv`**: Reporte analítico tabular que consolida las sumatorias volumétricas de viviendas encontradas por zona censal o distrito.

---

## ⚙️ Fases del Pipeline Geoespacial

El proceso central está estructurado bajo las siguientes etapas algorítmicas secuenciales:

```text
[Ingesta de Capas] ──> [Validación/Repreyección CRS] ──> [Recorte AOI]
                                                             │
[Exportación de Datos] <── [Métricas (MST / Buffers)] <── [SJoin Nearest]

```

# Crear el entorno virtual
python -m venv .venv

# Activar el entorno virtual (En Windows via PowerShell)
.\.venv\Scripts\Activate.ps1

```text

pip install --upgrade pip
pip install -r requirements.txt

```
