# Pronóstico del nivel del arroyo Mburicaó

> [!WARNING]
> **Producto experimental de investigación** (tesis de maestría, FIUNA).
> **No es una alerta oficial.** No usar para decisiones de seguridad.

Este repositorio **no tiene código**. Tiene un único archivo de datos que un programa
actualiza **automáticamente cada 10 minutos**:

**`forecast.json`**, en la rama [`data`](../../tree/data):
<https://raw.githubusercontent.com/fedemoranf-alt/mburicao-pronostico/data/forecast.json>

Lo lee la página del proyecto **Cuencas Urbanas Mburicaó** para mostrar el pronóstico del
nivel del arroyo a 10, 20 y 30 minutos:
<https://mburicaocastai.github.io/v1/es/monitoreo-tiempo-real/>

## Cómo funciona

1. Un sensor en el arroyo mide el nivel del agua cada 5 minutos.
2. Cada 10 minutos, un programa en un servidor de la Facultad de Ingeniería (FIUNA) lee
   esas mediciones y calcula el nivel esperado para los próximos 30 minutos, con un rango
   de incertidumbre.
3. Guarda el resultado en su base de datos y **publica una copia acá**, en `forecast.json`.
4. La página web descarga este archivo y dibuja el gráfico.

## Qué hay en el archivo

| Campo | Qué significa |
|---|---|
| `actualizado` | Cuándo se publicó (hora universal, UTC) |
| `ultimo_dato` | Hora de la última medición usada (hora de Paraguay, UTC-3) |
| `estado` | `ok` = pronóstico del modelo · `fallback_persistence` = faltaban datos: se muestra el último nivel medido, sin rango |
| `observado` | Nivel medido cada 10 minutos en las últimas 3 horas, en metros |
| `pronostico` | Nivel esperado a 10, 20 y 30 minutos (`nivel`), su rango probable del 90 % (`min_90`, `max_90`) y la probabilidad de superar el umbral de referencia (`prob_umbral`) |
| `umbral` | Nivel de referencia estadístico (superado solo el 5 % del tiempo en 2025). **No** es un umbral oficial de alerta |

El rango del 90 % contiene el nivel real 9 de cada 10 veces **en promedio a lo largo del
tiempo**; no es una garantía para cada pronóstico individual.

## Por qué la rama `data` tiene un solo commit

Cada publicación **reemplaza** a la anterior en vez de sumarse al historial. Guardar un
commit cada 10 minutos serían más de 50.000 por año sin ningún valor: el historial completo
de pronósticos ya está en la base de datos del servidor.

**No edites la rama `data` a mano:** se reemplaza entera en la próxima publicación, en
menos de 10 minutos.

## Autoría

Federico Morán — tesis de Maestría en Ciencias de la Inteligencia Artificial, Facultad de
Ingeniería, Universidad Nacional de Asunción (FIUNA).
