# Temas sociales y ambientales con datasets reales de competiciones de Kaggle

## Criterio de admisión

Se revisaron la consigna de Proyecto 1P y los dos ejemplos proporcionados. Los documentos se usan como material de referencia para evaluar el proyecto. Se conserva la preferencia del estudiante por preguntas concretas y técnicas introductorias, aunque algún ejemplo emplee modelos más avanzados.

Cada opción incluida tiene: una competición identificable de Kaggle; origen real documentado; fechas; categorías; medidas continuas; ubicación; diez preguntas resolubles con cruces y gráficos; oportunidades de limpieza; métricas nuevas con referencias identificadas; posibilidad de faceted plots y heatmaps. No se exige participar en el ranking ni reproducir el objetivo predictivo de la competición.

El requisito espacial admite lugares o sectores, además de coordenadas, tal como está redactado en la consigna. Se indica exactamente qué ubicación aporta cada opción. No se presenta una distancia como si fuese una coordenada, ni una duración relativa como si fuese una fecha.

**Alcance de la verificación:** se comprobaron documentación pública, diccionarios y referencias. No se descargaron los CSV de estas competiciones. La calidad de cada fila y el porcentaje final de ruido se determinan en el proyecto; no se dan por calculados aquí. Los conteos citados son documentados por trabajos que analizaron las bases, no conteos propios.

No se incluyen propuestas de deforestación o desplazamiento: en esta búsqueda no se pudo verificar una competición que reúna todos los requisitos y el alcance introductorio solicitado. Esto no demuestra que no exista alguna.

## 1. Vigilancia ambiental del virus del Nilo Occidental

**Competición:** [West Nile Virus Prediction](https://www.kaggle.com/competitions/predict-west-nile-virus/data).

**Tema:** diferencias territoriales y temporales en la detección del virus en muestras de mosquitos, para orientar la revisión de la cobertura de vigilancia.

La fuente describe muestreos reales de Chicago y datos meteorológicos de NOAA. Se trabajaría con `train.csv` y `weather.csv`. Los registros etiquetados corresponden a 2007, 2009, 2011 y 2013. Un [notebook público de análisis](https://cocalc.com/github/eeshawn11/DSI33-Shawn/blob/main/Project4/code/Part_1_WNV_Cleaning.ipynb) documenta 10.506 registros principales y 2.944 meteorológicos.

| Tipo requerido | Evidencia en los archivos |
|---|---|
| Temporal | Date en muestreos y meteorología |
| Categórico | Species, Trap y WnvPresent; Station en meteorología |
| Continuo | Temperatura, velocidad del viento y precipitación en weather.csv |
| Espacial | Latitude, Longitude y direcciones aproximadas de trampas |

Las coordenadas son geocodificaciones aproximadas, no posiciones GPS exactas. La unidad principal es una muestra o lote de mosquitos, no una persona enferma. Las ubicaciones no se muestrean de forma uniforme y solo se registran capturas de especies encontradas.

### Diez preguntas y cómo responderlas

| N.º | Pregunta para la gestión de vigilancia | Análisis introductorio |
|---|---|---|
| 1 | ¿Qué sitios presentan detecciones repetidas en distintos períodos y merecen revisar la continuidad del seguimiento? | Cruzar trampa, año y mes; comparar muestras, positivos y proporción de lotes positivos con heatmap y curvas. |
| 2 | ¿En qué meses se concentra la detección y se repite el patrón entre los años disponibles? | Comparar perfiles mensuales por año y especie, mostrando cantidad analizada y positividad en facetas. |
| 3 | ¿Las especies con más mosquitos analizados también concentran las detecciones positivas? | Comparar volumen analizado, lotes positivos y métricas normalizadas por especie y período. Usar barras y puntos. |
| 4 | ¿Qué sitios parecen destacar por detección debido a un mayor esfuerzo de muestreo y cuáles mantienen valores altos al normalizar? | Contrastar el orden por positivos con el orden por porcentaje de lotes positivos y MIR, reportando los denominadores. |
| 5 | ¿Qué combinaciones de sitio y especie mantienen señales de detección entre años? | Construir tablas sitio–especie–año; comparar continuidad, positividad y cobertura, evitando grupos con muy pocas muestras. |
| 6 | ¿Cómo varía la detección en semanas con diferentes condiciones de temperatura? | Unir el clima por fecha tras resolver las dos estaciones; comparar temperatura, especie, mes y positividad con boxplots y facetas. No inferir causalidad. |
| 7 | ¿Las condiciones de lluvia anteriores coinciden con diferencias en la detección posterior? | Calcular lluvia acumulada de siete días en el calendario meteorológico y compararla con detección, separando año y especie. |
| 8 | ¿Los sitios con detección reiterada tienen cobertura suficiente a lo largo de la temporada? | Contrastar meses observados, número de muestras y detecciones mediante matriz de cobertura y líneas. Ausencia de registro no significa resultado negativo. |
| 9 | ¿Qué zonas combinan detección recurrente y seguimiento intermitente? | Definir agrupaciones espaciales transparentes, comparar cobertura y métricas de positividad; mostrar un heatmap y un gráfico de puntos. No llamarlas barrios oficiales sin un archivo de límites. |
| 10 | ¿Cambian las zonas señaladas al usar positivos absolutos, proporción de lotes positivos o MIR? | Comparar las tres vistas, sus denominadores y su estabilidad entre períodos. Identificar diferencias de interpretación para el informe de vigilancia. |

### Métricas nuevas y bibliografía

- **Porcentaje de lotes positivos:** 100 × lotes positivos / lotes analizados. Es una variable agregada por sitio, especie y período; no aparece calculada en cada fila.
- **MIR por 1.000 mosquitos:** 1.000 × lotes positivos / mosquitos analizados, por especie y período. Supone un mosquito infectado por lote positivo; es una aproximación, especialmente limitada cuando la infección es alta.
- **Lluvia previa acumulada:** suma de precipitación en los siete días anteriores. Es una extensión exploratoria de la relación entre condiciones ambientales y vigilancia; el lapso concreto debe justificarse, sin presentarlo como un óptimo demostrado.

Las dos primeras métricas están definidas en las [guías de vigilancia de CDC](https://www.cdc.gov/west-nile-virus/php/surveillance-and-control-guidelines/index.html). Ese documento también explica que la proporción de lotes positivos tiene comparabilidad limitada cuando varía el tamaño de los lotes. La MIR se implementa con sumas y divisiones, sin estimación avanzada. El proyecto debe mostrar tamaños muestrales y revisar si ambas métricas ofrecen mensajes consistentes.

### Limpieza y alcance

Leer fechas y unidades; interpretar códigos meteorológicos de faltantes o trazas usando el PDF NOAA incluido en la competición; verificar coordenadas y conteos. La competición divide capturas mayores de 50 mosquitos en varias filas: varias filas con igual fecha, trampa y especie no son automáticamente duplicados.

Weather tiene dos estaciones. Para una primera versión puede elegirse una estación fija y documentarla como referencia meteorológica de la ciudad; no representa el microclima exacto de cada trampa. Unir sin tratar esa duplicidad multiplicaría registros. Calcular acumulados sobre el calendario meteorológico antes de unirlos a muestreos.

No usar fumigación para afirmar que un tratamiento funcionó: demostrar ese efecto requeriría otro diseño. El alcance aquí es describir muestras observadas, no estimar incidencia humana ni indicar intervenciones sanitarias concretas.

**Dificultad:** moderada y manejable. Facetas por especie/año y heatmaps sitio–mes cumplen el apartado visual.

## 2. Diferencias territoriales en el costo de vivienda y acceso a servicios

**Competición:** [Sberbank Russian Housing Market](https://www.kaggle.com/competitions/sberbank-russian-housing-market/data).

**Tema:** comparar viviendas semejantes para describir diferencias de precio y proximidad a servicios entre distritos de Moscú.

Kaggle describe transacciones individuales de agosto de 2011 a junio de 2015 en entrenamiento. El [informe de un análisis académico](https://www.imo.universite-paris-saclay.fr/~yannig.goude/Materials/ProjetMLF/Rapports2020/Report_Machine_Learning_Project_Gomtsyan_Marival.pdf) documenta 30.471 observaciones. Para empezar basta con train.csv y el diccionario; se seleccionan unas pocas columnas en vez de analizar todas.

| Tipo requerido | Evidencia en el diccionario |
|---|---|
| Temporal | timestamp: fecha de transacción |
| Categórico | product_type, material, state, sub_area |
| Continuo | price_doc, full_sq, life_sq y distancias a servicios |
| Espacial | sub_area: nombre del distrito; proximidades a metro, áreas verdes y otros lugares |

El [diccionario distribuido con la base, en una copia pública](https://github.com/datitran/Krimskrams/blob/master/Kaggle/Sberbank%20Russian%20Housing%20Market/data_dictionary.txt) permite comprobar el significado de campos. La ubicación se aporta por distrito y relaciones espaciales; no se promete disponer de latitud y longitud de cada vivienda.

### Diez preguntas y cómo responderlas

| N.º | Pregunta para analizar vivienda y territorio | Análisis introductorio |
|---|---|---|
| 1 | ¿Qué distritos combinan precios por m² relativamente menores y proximidad al metro para viviendas de tamaño semejante? | Crear precio por m² y estratos de superficie; comparar precio, distancia y distribución por distrito y período. |
| 2 | ¿La diferencia de precio entre distritos se mantiene al separar viviendas por tamaño y condición? | Comparar boxplots de precio por m² dentro de estratos y contrastar con la comparación sin estratos. |
| 3 | ¿Qué distritos muestran aumentos persistentes del precio por m² entre períodos comparables? | Agrupar por trimestre y distrito, controlar la composición por tamaño y mostrar series junto con cantidades de transacciones. |
| 4 | ¿Las viviendas más económicas de cada distrito también se encuentran más alejadas del transporte público? | Comparar grupos de precio por m², rangos de tamaño y distancias; usar facetas por distrito. |
| 5 | ¿Cómo cambia la combinación de precio y proximidad a zonas verdes entre distritos? | Comparar viviendas de tamaño similar, distancias a zonas verdes y precio por m² con puntos y distribuciones. |
| 6 | ¿Las viviendas de mayor antigüedad combinan precios menores y diferencias de condición según el distrito? | Calcular edad a la fecha de transacción; cruzar edad, condición y precio por m² en facetas. |
| 7 | ¿Qué distritos presentan una dispersión amplia de precios incluso entre viviendas comparables? | Obtener mediana y cuartiles dentro de estratos de superficie/condición y contrastar períodos con boxplots. |
| 8 | ¿La cercanía al metro se asocia del mismo modo con el precio en distintos distritos? | Comparar rangos de distancia, superficie y condición; usar dispersión y facetas. La asociación no demuestra que el metro cause la diferencia. |
| 9 | ¿Cambian las diferencias territoriales entre tipos de compra registrados? | Separar product_type, controlar superficie y trimestre, y comparar distribuciones por distrito. No inferir ingresos ni situación social individual del comprador. |
| 10 | ¿Qué diferencias entre distritos se conservan después de retirar únicamente registros confirmados como inconsistentes? | Comparar las tablas y gráficos de precio, superficie y proximidad antes y después de reglas de limpieza documentadas. |

### Métricas nuevas y bibliografía

- **Precio por m²:** price_doc / full_sq, con superficie positiva y válida.
- **Edad del inmueble al transar:** año de timestamp menos build_year, con validaciones temporales.
- **Índice descriptivo relativo:** mediana del precio por m² del distrito y estrato / mediana de una referencia comparable × 100. Se presenta como comparación de la muestra, no como índice oficial de precios ajustado por calidad.

[Scatigna, Szemere y Tsatsaronis (2014), BIS](https://www.bis.org/publications/qr-201409/residential-property-price-statistics-across-globe) discuten precio por m² y estratificación por características para comparar inmuebles. El [Handbook on Residential Property Price Indices, OECD et al. (2013)](https://www.oecd.org/en/publications/handbook-on-residential-property-price-indices_9789264197183-en.html) aporta el marco para justificar comparaciones temporales y advertir sobre cambios de composición. La edad es una característica derivada para esa estratificación; no se atribuye al manual una fórmula que este no haya definido expresamente.

### Limpieza y alcance

Revisar superficies cero o negativas, relaciones inconsistentes entre superficie habitable y total, plantas/pisos, años de construcción y nulos. Las transacciones se identifican por id; una misma vivienda puede tener transacciones distintas, por lo que no se elimina por repetición aparente del inmueble. No quitar precios extremos sin verificar su significado.

Trabajar con aproximadamente 12–15 columnas y cuatro a seis distritos con suficientes datos. Usar heatmap distrito–trimestre y boxplots con facetas por tamaño o condición. No se necesita predecir precios ni ajustar una regresión hedónica.

**Límite de interpretación social:** el dataset sirve para desigualdades de precios y proximidad territorial. Por sí solo no mide capacidad de pago de hogares, desplazamiento, desalojos, disponibilidad de vivienda social ni causalidad de gentrificación. No se proponen preguntas sobre esas variables ausentes.

**Dificultad:** moderada, con conceptos cercanos y limpieza suficiente para el curso.

## 3. Integración de energía solar y perfiles territoriales de consumo

**Competición:** [Enefit – Predict Energy Behavior of Prosumers](https://www.kaggle.com/competitions/predict-energy-behavior-of-prosumers/data).

**Tema:** describir cuándo coinciden o se separan los perfiles de consumo y producción solar en grupos territoriales de clientes, para orientar análisis de flexibilidad energética.

La documentación identifica clientes reales de Estonia y segmentos agregados por condado, tipo de cliente y contrato. No son perfiles de hogares identificables. La [descripción del organizador de la competición](https://www.linkedin.com/pulse/machine-learning-time-series-forecasting-insights-from-scellier-ndjbf) confirma que el problema se basó en consumo y producción fotovoltaica de clientes agrupados territorialmente.

| Tipo requerido | Evidencia en archivos de la competición |
|---|---|
| Temporal | datetime en entrenamiento y date en client.csv |
| Categórico | is_business, product_type, is_consumption |
| Continuo | target y installed_capacity; variables meteorológicas si se incorporan |
| Espacial | county y county_id_to_name_map.json; coordenadas meteorológicas en archivos de clima y su correspondencia territorial |

Las coordenadas del clima identifican estaciones o puntos meteorológicos, no viviendas. Los condados sí son ubicaciones del segmento de energía. No se traslada una coordenada de estación a los clientes como si fuera su dirección.

Para empezar bastan train.csv, client.csv y el catálogo de condados. El clima, los precios y la API de evaluación predictiva son extensiones opcionales. La escala de la base obliga a seleccionar columnas y un período acotado, pero no exige modelos avanzados.

### Diez preguntas y cómo responderlas

| N.º | Pregunta para integración de renovables | Análisis introductorio |
|---|---|---|
| 1 | ¿En qué horas la producción registrada coincide menos con el consumo del mismo segmento? | Emparejar producción y consumo por segmento y hora; comparar curvas y diferencias con facetas por condado. |
| 2 | ¿Cómo cambia esa coincidencia entre estaciones del año? | Crear mes/estación y contrastar perfiles horarios y balance de energía con heatmaps. |
| 3 | ¿Los segmentos residenciales y empresariales presentan horarios distintos de mayor consumo? | Separar is_business y condado; comparar perfiles normalizados y dispersión por hora. |
| 4 | ¿Qué condados presentan mayor producción por unidad de capacidad instalada en períodos comparables? | Unir capacidad por segmento y fecha; calcular producción normalizada y revisar cobertura y distribución, no solo totales. |
| 5 | ¿Qué segmentos mantienen una diferencia positiva entre consumo y producción durante más horas? | Contar horas con ambos datos válidos, comparar frecuencia y magnitud de la diferencia por mes y segmento. |
| 6 | ¿Qué diferencias de perfiles aparecen entre días laborables y fines de semana? | Crear tipo de día, comparar horarios dentro del mismo condado, cliente y contrato, y usar facetas. |
| 7 | ¿Los segmentos que más producen en términos absolutos también destacan tras considerar su capacidad instalada? | Contrastar producción total, capacidad y producción normalizada por período mediante puntos y tablas. |
| 8 | ¿Qué segmentos tienen perfiles de consumo más variables y en qué meses ocurre? | Comparar cuartiles, cambios de días equivalentes y perfiles horarios por segmento; controlar cobertura y escala. |
| 9 | ¿Cambian los perfiles observados entre tipos de contrato dentro de un mismo condado y clase de cliente? | Separar contrato y comparar curvas normalizadas y distribuciones, sin atribuir al contrato un efecto causal. |
| 10 | ¿Cómo cambia la comparación de producción entre segmentos cuando se consideran cambios de capacidad y número de puntos de suministro? | Usar client.csv para contrastar resultados totales, por capacidad y por punto, documentando denominadores y datos faltantes. |

### Métricas nuevas y bibliografía

- **Producción específica:** energía producida del segmento / capacidad instalada correspondiente, en períodos y unidades compatibles.
- **Diferencia horaria registrada:** consumo menos producción del mismo segmento y hora. No interpretarla como autoconsumo físico medido de cada instalación.
- **Frecuencia de diferencia positiva:** horas válidas con consumo superior a producción / horas con ambas medidas × 100.

[Marion et al. (2005), Performance Parameters for Grid-Connected PV Systems, NREL/CP-520-37358](https://research-hub.nlr.gov/en/publications/performance-parameters-for-grid-connected-pv-systems-3/) y el [manual de recurso solar IEA PVPS (2021)](https://iea-pvps.org/wp-content/uploads/2021/06/IEA_PVPS_T16_Solar_Res_Handbook_20211028.pdf) justifican normalizar energía por potencia nominal para comparar sistemas. Se adapta ese principio a segmentos, sin confundirlo con una eficiencia de conversión. El estudio [Measuring the Resilience of Renewable Energy](https://assets-eu.researchsquare.com/files/rs-7296905/v1_covered_97327764-1cfa-4af3-a292-340062612839.pdf), disponible como preprint, describe expresamente el emparejamiento de consumo y producción y diferencias positivas con este dataset. El indicador de frecuencia propuesto resume esos balances; no se atribuye su fórmula exacta al artículo.

### Limpieza y alcance

Revisar fechas y zona horaria EET/EEST, horario de verano, nulos, identificadores y capacidad positiva. Validar las claves del emparejamiento y de la unión con clientes: segmento y fecha correspondientes, considerando el significado de data_block_id. Una unión basada solo en condado multiplicaría las filas y sería incorrecta.

Seleccionar algunos condados y varios meses con buena cobertura. Para una versión inicial, trabajar con perfiles diarios/horarios, sin clima ni precios. No hace falta entrenar una red neuronal ni usar la API de competición para responder las preguntas descriptivas.

**Dificultad:** moderada; requiere más cuidado con uniones y horarios que las otras opciones. No permite medir pobreza energética ni ahorros de una familia: no contiene sus ingresos ni facturas individuales.

## Comprobación común para el proyecto

| Requisito | Cómo se concreta en las tres opciones |
|---|---|
| Diez preguntas que requieren varias técnicas | Cada sección incluye diez cruces con procedimientos; no se limitan a una media o un total. |
| Limpieza y transformación | Hay reglas y errores potenciales específicos; el diagnóstico real se ejecutará en el notebook. |
| Porcentaje de ruido | Contar filas no utilizables con reglas justificadas y dividir entre filas originales; separar filas corregidas de excluidas. No contar dos veces una misma fila por varias causas. |
| Features con bibliografía | Se identifican métricas derivadas y fuentes concretas, distinguiendo definiciones publicadas de adaptaciones propuestas. |
| Medidas estadísticas | Medianas, cuartiles, proporciones, variación y comparación dentro de estratos. |
| Faceted plots | Por especie/año; distrito/tamaño; condado/tipo de cliente, respectivamente. |
| Heatmaps | Sitio–mes; distrito–trimestre; hora–mes, respectivamente. |
| Entregables | Se pueden entregar notebooks documentados, archivos usados y diapositivas con preguntas, gráficos y conclusiones. Aquí se prepara la selección; esos entregables todavía no están realizados. |

No hay una exigencia de ruido mínimo: si el diagnóstico no identifica registros no utilizables, se documentan las comprobaciones y el porcentaje obtenido. No se eliminan observaciones válidas para fabricar una etapa de limpieza.

## Elección recomendada

**Primera elección: West Nile Virus.** Es la mejor combinación entre impacto social directo, ubicación con coordenadas, tamaño manejable y métricas sencillas con bibliografía explícita. Hay que respetar la unidad de muestreo y no convertir sus resultados en incidencia humana.

**Segunda elección: vivienda y territorio.** Es apropiada si interesa desigualdad urbana y se quiere un tema intuitivo; el análisis se limita a precios y proximidad a servicios en las transacciones observadas.

**Tercera elección: Enefit.** Es la alternativa ambiental para transición energética; se adapta al curso mediante filtros y agrupaciones, aunque necesita uniones temporales más cuidadosas.
