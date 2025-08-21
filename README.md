# sistema-inteligencia-electoral-manzanillo.
Limpieza, consolidación y análisis de datos electorales y sociodemográficos de Manzanillo, Colima (2012-2024)".

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