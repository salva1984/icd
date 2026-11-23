# Vigilancia del Virus del Nilo Occidental en Chicago

Proyecto 1P de Introducción a la Ciencia de Datos (PAO 2026-II). El análisis está en [`proyecto_wnv.ipynb`](proyecto_wnv.ipynb).

## Datos

| Archivo | Contenido | Fuente |
|---|---|---|
| `export.csv` | Resultados de laboratorio de los lotes (pools) de mosquitos capturados por el Departamento de Salud Pública de Chicago, 2007 a 2026 | [West Nile Virus (WNV) Mosquito Test Results](https://data.cityofchicago.org/d/jqe8-8r6s), Portal de Datos Abiertos de Chicago |
| `datos/clima_noaa_chicago.csv` | Clima diario de las estaciones O'Hare (`USW00094846`) y Midway (`USW00014819`) | [NOAA GHCN-Daily](https://www.ncei.noaa.gov/access/services/data/v1?dataset=daily-summaries&stations=USW00094846,USW00014819&startDate=2007-01-01&endDate=2026-11-20&dataTypes=TMAX,TMIN,TAVG,PRCP,SNOW,AWND&format=csv&units=metric&includeStationName=true) |

Los demás archivos de `datos/` los exporta el notebook al final.

## Features creadas

El dataset solo dice, para cada lote, cuántos mosquitos tenía y si salió positivo. Con eso no se puede comparar el riesgo entre semanas, zonas o años, ni anticiparlo. Las siete features siguientes vienen de una guía del CDC y de dos estudios hechos con datos de Chicago.

| # | Feature | Cómo se calcula | Fuente |
|---|---|---|---|
| 1 | `MIR` | 1000 × lotes positivos / mosquitos analizados | CDC (2013), pp. 17 y 18; Karki et al. (2020) |
| 2 | `MIR_PREV_1` a `MIR_PREV_4` | MIR de 1 a 4 semanas antes | Karki et al. (2020) |
| 3 | `POS_ACUM` | Lotes positivos acumulados desde el inicio de la temporada | Karki et al. (2020) |
| 4 | `ABUNDANCIA` | Mosquitos capturados / trampa-semanas, contando las trampas revisadas sin captura | CDC (2013), p. 17; metadatos del portal de Chicago |
| 5 | `TEMP_PREV_1` a `TEMP_PREV_4` | Temperatura media semanal de 1 a 4 semanas antes | Karki et al. (2020); Ruiz et al. (2010) |
| 6 | `LLUVIA_PREV_1` a `LLUVIA_PREV_4` | Lluvia total semanal (mm) de 1 a 4 semanas antes | Karki et al. (2020); Ruiz et al. (2010) |
| 7 | `TEMP_ENERO` | Temperatura media de enero del año de la temporada | Karki et al. (2020); Ruiz et al. (2010) |

### Por qué se eligió cada una

**1. MIR (tasa mínima de infección).** Contar lotes positivos no basta, porque un año en que se analizan más mosquitos tendrá más positivos aunque la infección sea la misma. La MIR divide por los mosquitos analizados y da una tasa comparable entre semanas, zonas y años. Es la medida que describe el CDC (2013) para estimar la infección a partir de lotes. Es "mínima" porque supone un solo mosquito infectado por lote positivo, así que subestima la infección cuando esta es alta; el CDC recomienda en ese caso la estimación por máxima verosimilitud. Karki et al. (2020) también usan la MIR en sus modelos para Chicago.

**2. MIR de semanas previas.** Karki et al. (2020) incluyeron la MIR de 1 a 4 semanas antes y encontraron que aumenta la probabilidad de casos humanos en Chicago, con la relación más fuerte a una semana. La usamos para ver si la infección de hace unas semanas anticipa la de ahora y sirve como alerta temprana.

**3. Positivos acumulados en la temporada.** En Karki et al. (2020), los lotes positivos acumulados hasta la semana 31 explicaron el 93 % de la variación de los casos humanos anuales. Por eso el notebook usa la semana 31 como punto de corte para estimar a mitad de temporada qué tan intenso será el año.

**4. Abundancia vectorial.** La MIR dice qué proporción de mosquitos está infectada, pero no cuántos hay. Una zona con MIR alta y pocos mosquitos no representa el mismo riesgo que una con MIR alta y muchos. El CDC (2013) define la abundancia como mosquitos por noche de trampa. Según los metadatos del portal de Chicago, desde 2024 el dataset incluye las trampas revisadas sin hembras *Culex*, y esas trampa-semanas entran con 0 mosquitos para no inflar la abundancia. Cada trampa se revisa una vez por semana (el 99,6 % de las trampa-semanas tiene una sola fecha de recolección), así que se usa la trampa-semana en lugar de la noche de trampa.

**5. Temperatura de semanas previas.** Ruiz et al. (2010) encontraron que la temperatura es el factor climático que mejor predice la infección de los mosquitos en el noreste de Illinois. Su efecto llega con retraso, porque el calor acelera el desarrollo del mosquito y la multiplicación del virus dentro de él. Seguimos a Karki et al. (2020): la temperatura semanal es el promedio de las medias diarias y se prueban rezagos de 1 a 4 semanas. NOAA no publica la temperatura media (`TAVG`) de estas estaciones, así que se reconstruye como (TMAX + TMIN) / 2.

**6. Lluvia de semanas previas.** En la literatura el efecto de la lluvia no es claro. Karki et al. (2020) encontraron una asociación negativa con los casos humanos, y Ruiz et al. (2010) vieron que su efecto cambia de un año a otro. La incluimos para comprobar si en estos datos ayuda a anticipar la infección. Igual que en Karki et al. (2020), la lluvia semanal es la suma de la lluvia diaria y se prueban rezagos de 1 a 4 semanas.

**7. Invierno cálido.** Ruiz et al. (2010) proponen que con un invierno suave podrían sobrevivir más mosquitos adultos hasta la primavera. Karki et al. (2020) usan la temperatura media de enero como indicador del invierno, y aquí se calcula igual, promediando las dos estaciones.

A cada trampa se le asigna el clima de la estación más cercana (O'Hare o Midway).

## Referencias

- Centers for Disease Control and Prevention. (2013). *West Nile virus in the United States: Guidelines for surveillance, prevention, and control* (4.ª rev.). https://stacks.cdc.gov/view/cdc/22064
- Karki, S., Brown, W. M., Uelmen, J., Ruiz, M. O., & Smith, R. L. (2020). The drivers of West Nile virus human illness in the Chicago, Illinois, USA area: Fine scale dynamic effects of weather, mosquito infection, social, and biological conditions. *PLoS ONE, 15*(5), e0227160. https://pmc.ncbi.nlm.nih.gov/articles/PMC7241786
- Ruiz, M. O., Chaves, L. F., Hamer, G. L., Sun, T., Brown, W. M., Walker, E. D., Haramis, L., Goldberg, T. L., & Kitron, U. D. (2010). Local impact of temperature and precipitation on West Nile virus infection in *Culex* species mosquitoes in northeast Illinois, USA. *Parasites & Vectors, 3*, 19. https://pmc.ncbi.nlm.nih.gov/articles/PMC2856545
- City of Chicago. (2024). *West Nile Virus (WNV) Mosquito Test Results* [metadatos del dataset]. https://data.cityofchicago.org/d/jqe8-8r6s
