# sistema-inteligencia-electoral-manzanillo.
Limpieza, consolidación y análisis de datos electorales y sociodemográficos de Manzanillo, Colima (2012-2024)".
# Sistema de Inteligencia Electoral - Manzanillo (2012-2024)

## Descripción
Este proyecto implementa un flujo de trabajo completo para la limpieza, estandarización, enriquecimiento y consolidación de resultados electorales a nivel casilla para todas las elecciones locales y federales en Manzanillo, Colima, durante el periodo 2012-2024.

El objetivo final es la creación de una base de datos maestra unificada y un GeoPackage, que sirvan como insumos para el análisis geoespacial y político-electoral, cruzando los resultados de las votaciones con datos demográficos del Censo 2020.

## Estructura del Proyecto
El repositorio está organizado en una estructura modular para separar los datos crudos, el código, los datos procesados y los resultados finales.

```
/
├── 01_datos_brutos/        # Datos originales (ignorados por Git)
│   ├── Ayuntamiento/
│   ├── Diputado_Local/
│   ├── ... etc.
├── 02_datos_limpios/         # Archivos CSV limpios por año (ignorados por Git)
├── 03_base_consolidada/    # Archivos CSV consolidados por tipo de elección (ignorados por Git)
├── 04_base_maestra/        # La base de datos maestra final (ignorada por Git)
├── 05_censo/               # Archivos del censo y geometrías (ignorados por Git)
├── 06_geopackage/          # El GeoPackage final (ignorado por Git)
├── scripts/
│   └── procesador_electoral.py # Script principal para automatizar todo el proceso
├── .gitignore              # Especifica los archivos y carpetas a ignorar
└── README.md               # Este archivo de documentación
```

## Metodología
El proceso se divide en las siguientes fases, automatizadas por el script principal:

1.  **Limpieza Individual:** Cada archivo CSV de resultados es procesado para corregir inconsistencias de formato, nombres de columna y tipos de dato.
2.  **Enriquecimiento Analítico:** A cada registro (casilla) se le añaden métricas clave como:
    * **Bloques Políticos:** Agregación de votos en `morena_coalitions` y `non_morena_coalitions`.
    * **Análisis de Competencia:** Identificación del ganador, segundo lugar y margen de victoria por casilla.
3.  **Consolidación:** Los archivos limpios se consolidan por tipo de elección y, finalmente, en una única base de datos maestra.
4.  **Unión Geo-Censal:** La base de datos final se une con las geometrías de las secciones electorales y los datos del Censo 2020 para generar el producto final.

## Diccionario de Variables (Columnas Analíticas Principales)
La base de datos final contiene las siguientes columnas calculadas, además de los votos crudos de cada elección.

| Nombre de la Columna      | Tipo de Dato | Descripción                                                                                               |
| ------------------------- | ------------ | --------------------------------------------------------------------------------------------------------- |
| **IDENTIFICADORES** |              |                                                                                                           |
| `seccion`                 | `int`      | Identificador numérico único de la sección electoral. Llave principal para unir con datos censales/geo. |
| `casilla`                 | `object`     | Identificador de la casilla (ej. 'B1', 'C1', 'C2').                                                         |
| `anio`                    | `int`      | Año de la elección.                                                                                       |
| `tipo_eleccion`           | `object`     | Tipo de elección (Ayuntamiento, Gobernador, Presidente, etc.).                                            |
| `lista_nominal`           | `int`      | Número total de ciudadanos registrados para votar en esa casilla/sección.                                    |
| **MÉTRICAS DE COMPETENCIA** | | |
| `ganador_casilla_partido` | `object`     | Nombre del partido o coalición que obtuvo más votos en esa casilla.                                       |
| `ganador_casilla_votos`   | `int`      | Número de votos obtenidos por el ganador en esa casilla.                                                  |
| `segundo_lugar_partido`   | `object`     | Nombre del competidor que obtuvo el segundo lugar en votos.                                               |
| `segundo_lugar_votos`     | `int`      | Número de votos obtenidos por el segundo lugar.                                                           |
| `margen_victoria`         | `int`      | Diferencia de votos entre el primer y el segundo lugar.                                                   |
| **MÉTRICAS DE BLOQUES** | | |
| `morena_coalitions`       | `int`      | Suma total de votos para Morena y sus aliados. Para años pre-Morena, es un proxy de izquierda. |
| `non_morena_coalitions`   | `int`      | Suma total de votos para todos los demás competidores (bloque opositor).                               |
| `voto_morena_solo`        | `int`      | Votos emitidos únicamente por el partido Morena. Es 0 si Morena no compitió.                  |
| `voto_morena_aliados`     | `int`      | Suma de votos para los aliados de Morena y para las combinaciones de su coalición.                        |
