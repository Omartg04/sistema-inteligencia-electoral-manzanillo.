# Sistema de Inteligencia Electoral - Manzanillo (2012-2024)

## Descripción
Este proyecto implementa un flujo de trabajo completo para la limpieza, estandarización, enriquecimiento y consolidación de resultados electorales a nivel casilla para todas las elecciones locales y federales en Manzanillo, Colima, durante el periodo 2012-2024.

El objetivo final es la creación de una base de datos maestra unificada y un GeoPackage, que sirvan como insumos para el análisis geoespacial y político-electoral, cruzando los resultados de las votaciones con datos demográficos del Censo 2020.

## Estructura del Proyecto
El repositorio está organizado en una estructura modular para separar los datos crudos, el código, los datos procesados y los resultados finales. Las bases se encuentran disponibles en: https://drive.google.com/drive/folders/14KyNSXuqfq38MGKCkIEkhvwje1wFJKAP?usp=sharing

```
/
├── 01_datos_brutos/        # Datos originales (ignorados por Git)
│   ├── Ayuntamiento/
│   ├── ... etc.
├── 02_datos_limpios/         # Archivos CSV limpios por año (ignorados por Git)
├── 03_base_consolidada/    # CSV consolidados por tipo de elección (ignorados por Git)
├── 04_base_maestra/        # La base de datos maestra final (ignorada por Git)
├── 05_censo/               # Archivos del censo y geometrías (ignorados por Git)
├── 05_analisis_agregado/   # Base de datos electoral agregada por sección (ignorada por Git)
├── 06_geopackage/          # El GeoPackage final (ignorado por Git)
├── scripts/
│   └── procesador_electoral.py # Script principal para automatizar todo el proceso
│   └── ... (notebooks de análisis)
├── .gitignore              # Especifica los archivos y carpetas a ignorar
└── README.md               # Este archivo de documentación
```

## Metodología
El proceso se divide en las siguientes fases, automatizadas por los scripts y notebooks:

1.  **Limpieza Individual:** Cada archivo CSV de resultados es procesado para corregir inconsistencias de formato, nombres de columna y tipos de dato.
2.  **Enriquecimiento Analítico:** A cada registro (casilla) se le añaden métricas clave como:
    * **Bloques Políticos:** Agregación de votos en `morena_coalitions` y `non_morena_coalitions`.
    * **Análisis de Competencia:** Identificación del ganador, segundo lugar y margen de victoria por casilla.
3.  **Consolidación:** Los archivos limpios se consolidan por tipo de elección y, finalmente, en una única **Base de Datos Maestra** que contiene el detalle de cada votación.
4.  **Agregación por Sección:** La Base de Datos Maestra es procesada para crear una **Base de Datos Agregada**, que resume el comportamiento electoral histórico (2012-2024) en una sola fila por cada sección electoral.
5.  **Unión Geo-Censal:** La base de datos agregada se une con las geometrías de las secciones electorales y los datos del Censo 2020 para generar el producto final: un **GeoPackage** listo para el análisis en sistemas de información geográfica (GIS).

## Diccionario de Variables (Columnas Analíticas Principales)
La **Base de Datos Maestra** y la **Base Agregada** contienen las siguientes columnas calculadas, además de los votos crudos de cada elección.

| Nombre de la Columna      | Nivel de Detalle | Descripción                                                                                               |
| ------------------------- | ---------------- | --------------------------------------------------------------------------------------------------------- |
| **IDENTIFICADORES** |                  |                                                                                                           |
| `seccion`                 | Ambos            | Identificador numérico único de la sección electoral. Llave principal para unir con datos censales/geo. |
| `casilla`                 | Maestra          | Identificador de la casilla (ej. 'B1', 'C1', 'C2').                                                         |
| `anio`                    | Maestra          | Año de la elección.                                                                                       |
| `tipo_eleccion`           | Maestra          | Tipo de elección (Ayuntamiento, Gobernador, Presidente, etc.).                                            |
| `lista_nominal`           | Maestra          | Número total de ciudadanos registrados para votar en esa casilla/sección.                                    |
| **MÉTRICAS DE COMPETENCIA** | | |
| `ganador_casilla_partido` | Maestra          | Nombre del partido o coalición que obtuvo más votos en esa casilla.                                       |
| `partido_dominante`       | Agregada         | El partido o coalición que ha ganado más veces en esa sección a lo largo del periodo 2012-2024.             |
| `margen_victoria`         | Maestra          | Diferencia de votos entre el primer y el segundo lugar en una casilla específica.                         |
| **MÉTRICAS DE BLOQUES** | | |
| `morena_coalitions`       | Maestra          | Suma de votos para Morena y sus aliados en una elección específica. Para años pre-Morena, es un proxy. |
| `votos_morena_acumulados` | Agregada         | Suma total de votos para el bloque de Morena en esa sección a lo largo de 12 años.                      |
| `pct_voto_morena`         | Agregada         | Porcentaje histórico del voto para el bloque de Morena en esa sección.                                     |
| `tasa_participacion_promedio` | Agregada | El promedio de participación ciudadana en esa sección a través