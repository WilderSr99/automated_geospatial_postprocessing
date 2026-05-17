# Proyecto de Postprocesamiento Geoespacial de Viviendas Detectadas

## Descripción del problema

Los modelos de inteligencia artificial pueden detectar viviendas en imágenes satelitales y entregar el resultado como polígonos en una capa geoespacial. Sin embargo, esos polígonos no siempre tienen contexto territorial, centro poblado asociado ni código UBIGEO.

Este proyecto organiza un flujo de trabajo en Python para automatizar el postprocesamiento geoespacial de esas viviendas detectadas.

## Objetivo del programa

Asignar viviendas detectadas a centros poblados cercanos, calcular centroides, agrupar por UBIGEO, generar métricas territoriales y exportar resultados geográficos y tabulares.

## Entradas

Los archivos de entrada:

- `viviendas_detectadas.gpkg`: polígonos de viviendas detectadas.
- `centros_poblados.gpkg`: puntos o polígonos de centros poblados con campo `UBIGEO`.
- `area_interes.gpkg`: polígono opcional para recortar el área de trabajo.

Las rutas se configuran en segun la ruta local

## Proceso

El programa principal realiza los siguientes pasos:

1. Carga las viviendas detectadas.
2. Carga los centros poblados.
3. Carga el área de interés si existe.
4. Valida que las capas tengan CRS.
5. Reproyecta las capas a un CRS métrico.
6. Recorta viviendas dentro del área de interés.
7. Calcula centroides de las viviendas.
8. Asigna cada vivienda al centro poblado más cercano con `sjoin_nearest`.
9. Agrupa viviendas por `UBIGEO`.
10. Calcula el total de viviendas por `UBIGEO`.
11. Genera Árboles de Expansión Mínima por grupo cuando hay dos o más viviendas.
12. Genera buffers simples por grupo.
13. Exporta resultados en GeoPackage y CSV.

## Salidas

Los resultados :

- `resultados_postproceso.gpkg`
  - `viviendas_asignadas`
  - `centroides_asignados`
  - `mst_por_ubigeo`
  - `buffers_por_ubigeo`
- `resumen_viviendas_por_ubigeo.csv`

## Instrucciones de ejecución

Crear y activar un entorno virtual:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Instalar dependencias:

```powershell
pip install -r requirements.txt
```

Ejecutar el programa:

```
En la carpeta de notebook
celda por celda
```

## Estructuras de programación usadas

El proyecto demuestra conceptos de Fundamentos de Programación I:

- Secuencias de instrucciones en `main.py`.
- Condicionales `if` para validar archivos, CRS, geometrías y cantidad de viviendas.
- Bucles `for` para recorrer viviendas, centroides y grupos por UBIGEO.
- Listas para almacenar geometrías, líneas, coordenadas y atributos.
- Diccionarios para resumir métricas por UBIGEO.
- Funciones reutilizables organizadas por módulo.
- Cadenas de texto para mensajes claros en terminal.
- Manejo básico de archivos de entrada y salida.
- Manejo básico de errores con `try`, `except`, `FileNotFoundError` y `ValueError`.

## Estructura del proyecto

```text
proyecto_postproceso_geoespacial/
|
├── notebooks/
│   ├── Flujo_delimitacion.ipynb
│   
|
├── output/
|   ├── Poligonos de viviendas antes de asignacion UBIGEO.png
|   ├── Poligonos de viviendas despues de asignacion UBIGEO.png
|   ├── Puntos_georeferenciado.png
|   ├── VORONOI.png
|
├── requirements.txt
└── README.md
```
