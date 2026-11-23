# Guion de exposición · Virus del Nilo Occidental en Chicago

| Parte | Expositor | Diapositivas | Contenido |
|---|---|---|---|
| 1 | Salvador | 1 a 16 | Contexto, preguntas, datos, limpieza, métricas y exploración |
| 2 | Integrante 2 | 17 a 25 | Preguntas 1 a 6: dónde y cuándo circula el virus |
| 3 | Integrante 3 | 26 a 39 | Preguntas 7 a 10, matriz de correlación y conclusiones |

Cada parte dura entre 5 y 6 minutos. El número entre corchetes es la diapositiva.

## Parte 1 · Salvador

### [1] Portada

Buenas, somos [nombres]. Nuestro proyecto trata sobre el virus del Nilo Occidental en Chicago y usa los datos de vigilancia de mosquitos para saber dónde y cuándo actuar contra el mosquito.

### [2] Imagen

(Pasar rápido.) Empiezo por el contexto y los datos.

### [3] Contexto

Este virus lo transmiten mosquitos del género *Culex*. En Chicago lo vigila el Departamento de Salud Pública, el CDPH, y cada temporada tiene que decidir tres cosas: dónde fumigar, según qué zonas concentran mosquitos infectados; dónde poner trampas, para que la vigilancia cubra las zonas de riesgo; y cuándo alertar a la población, según qué señales anticipan el pico de infección.

### [4] Preguntas de negocio

Para ayudar con esas decisiones planteamos 10 preguntas. Cinco son sobre cuándo circula el virus: la P2, P4, P6, P7 y P8. La P3 trata sobre los vectores, es decir, las especies de mosquito. La P1 y la P5 son sobre dónde actuar y con qué recursos, y la P9 y la P10 sobre el clima. Mis compañeros las responden más adelante; yo les cuento con qué datos trabajamos y cómo los preparamos.

### [5] Descripción del dataset

Usamos dos fuentes. La principal son los resultados de laboratorio de los mosquitos que captura el CDPH entre 2007 y 2026, publicados en el portal de datos abiertos de Chicago. Cada semana, de mayo a octubre, se revisan trampas en las 77 áreas comunitarias de la ciudad.

Los mosquitos no se analizan uno por uno. Se juntan en lotes de hasta 50 de la misma especie, y el lote sale positivo si al menos uno tiene el virus. Todo lo que viene después se mide en lotes.

La NOAA, por su parte, nos da el clima diario de dos estaciones: O'Hare, al noroeste, y Midway, al suroeste. A cada trampa le asignamos la estación más cercana y la semana de captura, y con eso unimos los dos datasets.

### [6] Columnas del dataset de mosquitos

El dataset de mosquitos tiene 45.481 registros y 19 columnas. Hay columnas de tiempo, como el año y la semana epidemiológica, y columnas de ubicación, como el área comunitaria, el distrito, el código postal y las coordenadas. También están la especie, el tipo de trampa, el resultado y el número de mosquitos del lote.

El 11,8 % de los registros no tiene resultado. Son trampas que se revisaron y no capturaron nada.

### [7] Columnas del dataset de clima

El de clima tiene 14.512 registros diarios y 9 columnas con temperatura, lluvia, nieve y viento. La temperatura media venía completamente vacía, así que la calculamos como el promedio entre la máxima y la mínima.

### [8] Limpieza

La limpieza tuvo nueve pasos. Quitamos los duplicados exactos y los TEST ID repetidos, y separamos las trampas sin captura: no sirven para medir infección, pero sí cuentan para la abundancia. Después quitamos los lotes con 0 mosquitos, convertimos las fechas, corregimos fechas de análisis fuera de rango, dejamos solo las trampas GRAVID y agrupamos las especies raras en "OTRAS".

Además, la latitud y la longitud venían intercambiadas en el dataset original. Las corregimos con la columna LOCATION.

### [9] Porcentaje de ruido

El 15,95 % de los registros es ruido, sobre todo trampas sin captura y trampas que no son GRAVID. Después de limpiar quedan 38.225 lotes para medir infección.

### [10] Métricas nuevas

Con los datos limpios creamos varias métricas a partir de la literatura. La principal es la MIR, la tasa mínima de infección: cuántos de cada 1000 mosquitos analizados tenían el virus, como mínimo. Se calcula como 1000 por los lotes positivos, dividido para los mosquitos analizados. Le dicen mínima porque un lote positivo puede tener más de un mosquito infectado y aun así cuenta una sola vez.

La abundancia vectorial sale de la guía del CDC de 2013. De Karki et al. (2020) y Ruiz et al. (2010) tomamos la MIR, la temperatura y la lluvia de 1 a 4 semanas antes, los positivos acumulados en la temporada y la temperatura de enero.

### [11] Glosario

Estos términos aparecen en toda la presentación. Un lote son los mosquitos de una trampa que se analizan juntos, y una trampa-semana es una trampa revisada durante una semana, haya capturado algo o no. La abundancia vectorial es el número de mosquitos capturados dividido para las trampas-semana. Los positivos acumulados cuentan los lotes positivos desde el inicio de la temporada y vuelven a cero cada año. El rezago es el valor de semanas anteriores, que usamos para ver si anticipa la infección actual. Y la ρ de Spearman mide qué tan juntas suben dos medidas, de −1 a 1: cerca de 0 no hay relación, y cerca de 1 suben juntas.

### [12] Exploración · tamaño del lote

Pasamos a la exploración. Solo el 9,9 % de los lotes sale positivo, 3.800 de 38.225. Los positivos tienen una mediana de 25 mosquitos y los negativos de 5, porque mientras más mosquitos hay en un lote, más probable es que alguno tenga el virus.

### [13] Exploración · lotes por año

Entre años cambia mucho tanto el muestreo como el porcentaje de positivos. El año con menos lotes fue 2022, con 991. En 2016 salió positivo el 23,7 % de los lotes, el máximo, y en 2009 apenas el 0,9 %.

### [14] Exploración · lotes por mes

La temporada va de junio a septiembre, y mayo y octubre casi no tienen datos. En agosto sale positivo el 18 % de los lotes, el doble que en julio.

### [15] Exploración · especies y zonas

El 45 % de los lotes son mixtos de *pipiens* y *restuans*, las dos especies principales. La zona más muestreada es el Extremo norte. En el Centro, en cambio, hay solo 99 lotes en 20 años.

### [16] Exploración · clima

En julio y agosto la temperatura media ronda los 24 °C, y Midway es más cálido que O'Hare todos los meses. De mayo a julio llueven unos 115 mm al mes.

Ahora [Integrante 2] les muestra dónde y cuándo circula el virus.

## Parte 2 · Integrante 2

### [17] Sección: preguntas 1 a 6

Gracias. En esta parte respondemos las preguntas 1 a 6, sobre dónde y cuándo circula el virus.

### [18] Imagen

(Pasar rápido.)

### [19] P1 · Clasificación ABC

La primera pregunta es qué porcentaje de distritos concentra la mayoría de los positivos. Ordenamos los distritos de más a menos positivos, como en un Pareto, y los separamos en clases A, B y C.

La clase A son 18 distritos, el 40 % del total, y juntos tienen el 79,6 % de los lotes positivos. La clase C es el 35,6 % de los distritos y tiene apenas el 5,4 %. Por eso el presupuesto de fumigación debería ir primero a los distritos de la clase A.

### [20] P2 · Estacionalidad

La segunda pregunta es en qué semanas se concentra la infección y si el pico cambia de un año a otro. Si promediamos todos los años, la MIR empieza a subir en la semana 27 y llega al máximo en la 33, a mediados de agosto.

### [21] P2 · Estacionalidad (cont.)

Año por año pasa lo mismo. El pico mediano también cae en la semana 33, y en 20 años nunca salió del rango entre la semana 30 y la 36. El más alto fue de 42 por 1000, en la semana 33 de 2025. Para el CDPH, eso significa tener la fumigación en marcha antes de la semana 30.

### [22] P3 · Especies

La tercera pregunta compara las especies. *Culex pipiens* es la que más se infecta, con una MIR de 11,3 por 1000. Los lotes mixtos aportan muchos más mosquitos: 9,8 por trampa-semana, contra 2,0 de *pipiens* sola. *Culex territans* casi no se infecta, con una MIR de 2,1.

### [23] P4 · Abundancia e infección

La cuarta pregunta es si el pico de mosquitos y el de infección caen en las mismas semanas. No: la infección llega una mediana de 3 semanas después del pico de abundancia, y así pasó en 19 de los 20 años. En la misma semana, la relación entre abundancia y MIR es débil, ρ = 0,26. Cuando se ve el pico de mosquitos, quedan unas 3 semanas para actuar.

### [24] P5 · Zonas de la ciudad

La quinta pregunta es qué zonas tienen más infección. El Noroeste tiene la MIR anual mediana más alta, 8,8, y el Extremo sureste la más baja, 5,1. El Centro no aparece porque no tiene mosquitos suficientes, lo que coincide con lo poco que se muestrea ahí.

### [25] P6 · Tendencia anual

La sexta pregunta es si la infección está aumentando con los años, y sí está aumentando: la correlación entre el año y la MIR anual es ρ = 0,52. La MIR promedio pasó de 6,2 en 2007-2016 a 9,8 en 2017-2026, un 57 % más, y el año más alto fue 2025, con 14,9.

Ahora [Integrante 3] les explica qué señales avisan antes de que llegue el pico.

## Parte 3 · Integrante 3

### [26] Sección: preguntas 7 a 10

Gracias. En esta última parte vemos las señales de alerta temprana y el clima.

### [27] Imagen

(Pasar rápido.)

### [28] P7 · Alerta temprana

La séptima pregunta usa un indicador de Karki et al. (2020): los positivos acumulados hasta la semana 31. Para esa semana ya ocurrió, en mediana, el 29 % de los positivos del año, y los años más intensos, 2012, 2016 y 2025, ya se separan del resto.

### [29] P7 · Alerta temprana (cont.)

Los años con más positivos hasta la semana 31 también tienen más positivos después, con ρ = 0,47. De ahí sale una regla: si en la semana 31 hay más de 39 positivos, que es la mediana histórica, hay que reforzar el control el resto de la temporada.

### [30] P8 · MIR previa

La octava pregunta es si la MIR de semanas anteriores anticipa la de la semana actual. Con una semana de rezago la relación es fuerte, ρ = 0,79, y con cuatro semanas baja a 0,31.

### [31] P8 · MIR previa (cont.)

Parte de esa correlación puede venir de la propia temporada, porque la infección sube y baja cada año en las mismas semanas. Por eso la separamos por fases, y se mantiene en todas: 0,59 en la fase temprana, 0,79 en el pico y 0,49 en la fase tardía. Mirar la MIR de la semana pasada sirve durante toda la temporada.

### [32] P9 · Temperatura y lluvia

La novena pregunta busca con cuántas semanas de anticipación el clima se relaciona con la infección. La temperatura de la misma semana tiene poca relación, ρ = 0,25, pero la de 3 a 4 semanas antes llega a ρ ≈ 0,60. La lluvia no muestra relación en ningún rezago.

### [33] P9 · Temperatura y lluvia (cont.)

Separada por fases, la relación con la temperatura baja a alrededor de 0,3 en cada una. Parte del efecto viene de que toda la temporada es calurosa, aunque dentro de cada fase la relación sigue siendo positiva.

### [34] P10 · Invierno

La última pregunta es si un enero más cálido anticipa una temporada más intensa. La temperatura de enero se relaciona con los positivos a la semana 31, con ρ = 0,47, y con la MIR anual, con 0,44. Por ejemplo, en 2012 enero tuvo −0,8 °C y para la semana 31 ya había 336 positivos. En 2014 enero tuvo −8,6 °C y hubo solo 23.

### [35] Matriz de correlación

En la matriz se ve qué medidas semanales van más de la mano con la infección. Primero está la MIR de la semana anterior, con 0,79, y después la temperatura de 3 semanas antes, con 0,60. La lluvia queda cerca de 0.

### [36] Conclusión 1 · Cuándo actuar

Con cinco señales armamos un calendario de alerta. En enero se revisa si el invierno fue cálido. Durante la temporada, el calor avisa con 3 semanas de anticipación, y el pico de mosquitos llega 3 semanas antes que el de infección. Cada semana se mira la MIR de la semana anterior, y en la semana 31, si hay más de 39 positivos, se refuerza el control. Con estas señales hay tiempo para fumigar antes del pico.

### [37] Conclusión 2 · Dónde actuar

La segunda conclusión es que los recursos deben concentrarse en pocas zonas. El 40 % de los distritos, la clase A, tiene el 80 % de los positivos, mientras que el 36 %, la clase C, tiene solo el 5 %. Por zonas, el Noroeste tiene la MIR más alta, 8,8, y el Extremo sureste la más baja, 5,1.

### [38] Imagen

(Pasar rápido.) Y la infección viene subiendo: un 57 % más en la última década.

### [39] Cierre

Gracias por su atención. ¿Tienen alguna pregunta?
