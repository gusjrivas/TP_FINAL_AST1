---
marp: true
theme: default
paginate: true
header: "![logo](logo-fiuba.png)"
footer: "Generación Eléctrica Mensual de Argentina (2015-2023)  ·  Análisis de Series de Tiempo — 02MIA2026"
title: "Análisis de Series de Tiempo Aplicado a la Generación Eléctrica Mensual de Argentina (2015–2023)"
style: |
  /* Paleta del informe: navy, blue, teal, accent (mismos colores que el PPTX) */
  :root {
    --navy: #1F3864;
    --blue: #2E75B6;
    --teal: #2E86AB;
    --accent: #E8A83C;
    --border: #C9D4E3;
    --gray: #595959;
    --light-bg: #F2F6FB;
    --header-bg: #ECF2FB;
    --alt-bg: #E3ECF7;
    --page-bg: #FCFDFE;
  }
  section {
    width: 1280px;
    height: 720px;
    padding: 126px 48px 58px 48px;
    background: var(--page-bg);
    font-family: "Avenir Next", "Helvetica Neue", Arial, sans-serif;
    font-size: 20.5px;
    line-height: 1.42;
    color: #333333;
    justify-content: flex-start;
  }
  /* banda superior con filete de acento a la izquierda y regla teal abajo */
  section::before {
    content: "";
    position: absolute;
    top: 0; left: 0;
    width: 100%; height: 108px;
    background: linear-gradient(90deg, var(--accent) 0 13px, var(--header-bg) 13px);
    border-bottom: 2px solid var(--teal);
  }
  /* kicker (h2) + título (h1) dentro de la banda */
  section h2 {
    position: absolute; top: 15px; left: 48px; margin: 0; padding: 0;
    font-size: 19px; font-weight: 700; color: var(--teal);
  }
  section h1 {
    position: absolute; top: 43px; left: 48px; margin: 0; padding: 0;
    font-size: 33px; font-weight: 700; color: var(--navy);
  }
  /* logo institucional arriba a la derecha */
  header {
    position: absolute; top: 26px; right: 42px; left: auto;
    margin: 0; padding: 0; z-index: 2;
  }
  header img { height: 54px; }
  /* footer con línea fina separadora + número de página */
  footer {
    position: absolute; left: 0; right: 0; bottom: 0; margin: 0;
    padding: 7px 100px 10px 48px;
    border-top: 1px solid var(--border);
    font-size: 13.5px; color: var(--gray); background: none;
  }
  section::after {
    position: absolute; right: 42px; bottom: 10px; padding: 0; margin: 0;
    font-size: 13.5px; color: var(--gray); background: none;
  }
  ul, ol { margin: 0.15em 0 0.5em; padding-left: 1.25em; }
  li { margin: 0.22em 0; }
  ul ul { font-size: 0.93em; color: #444444; margin: 0.1em 0; }
  li::marker { color: var(--blue); }
  strong { color: var(--navy); }
  p { margin: 0.45em 0; }
  table { margin: 0.45em auto; font-size: 0.92em; border-collapse: collapse; }
  th { background: var(--blue); color: #ffffff; border: 1px solid var(--blue); padding: 5px 16px; }
  td { border: 1px solid var(--border); padding: 5px 16px; background: #ffffff; }
  tbody tr:nth-child(even) td { background: var(--alt-bg); }
  blockquote {
    margin: 0.55em 0; padding: 12px 20px;
    border-left: 6px solid var(--accent);
    background: var(--light-bg); border-radius: 0 8px 8px 0;
    font-size: 0.97em; color: #333333;
  }
  blockquote p { margin: 0.25em 0; }
  /* layouts de dos columnas */
  .cols { display: flex; gap: 26px; align-items: center; }
  .cols > div { flex: 1; min-width: 0; }
  .cols > div.grow { flex: 1.55; }
  .cols img { max-width: 100%; height: auto; }
  .cols div > p:has(img) { text-align: center; margin: 0.2em 0; }
  .cols.top { align-items: flex-start; }
  .caption { font-size: 0.88em; color: var(--gray); }
  section.compact { font-size: 18.5px; }
  section.compact table { font-size: 0.9em; }
  section.compact td, section.compact th { padding: 4px 12px; }
  /* ---------- portada ---------- */
  section.title {
    padding: 0;
    background: linear-gradient(90deg, var(--navy) 0 413px, var(--page-bg) 413px);
  }
  section.title::before { display: none; }
  section.title .c1 {
    position: absolute; top: -125px; left: -125px;
    width: 307px; height: 307px; border-radius: 50%; background: var(--teal);
  }
  section.title .c2 {
    position: absolute; top: 538px; left: 250px;
    width: 250px; height: 250px; border-radius: 50%; background: var(--accent);
  }
  section.title .logo-plate {
    position: absolute; top: 52px; left: 52px;
    background: #ffffff; border-radius: 12px; padding: 13px 22px;
  }
  section.title .logo-plate img { height: 66px; display: block; }
  section.title .side-tag {
    position: absolute; bottom: 56px; left: 52px;
    color: #ffffff; font-size: 21px; font-weight: 700; line-height: 1.3;
  }
  section.title .side-tag span {
    display: block; margin-top: 8px;
    color: #CFD9EC; font-size: 17.5px; font-weight: 400;
  }
  section.title .main-title { position: absolute; top: 92px; left: 465px; right: 50px; }
  section.title .main-title .pre { color: var(--teal); font-size: 28px; margin: 0; }
  section.title .main-title .big {
    color: var(--navy); font-size: 44px; font-weight: 700; line-height: 1.18; margin: 6px 0 0;
  }
  section.title .main-title .years { color: var(--accent); font-size: 26px; font-weight: 700; margin: 12px 0 0; }
  section.title .info-card {
    position: absolute; top: 418px; left: 465px; right: 55px;
    background: #ffffff; border: 1px solid var(--border); border-radius: 12px;
    padding: 24px 34px; font-size: 20.5px; line-height: 1.75;
  }
  section.title .info-card b { color: var(--teal); }
  /* ---------- cierre ---------- */
  section.closing { padding: 0; background: var(--page-bg); }
  section.closing::before { display: none; }
  section.closing .c1 {
    position: absolute; top: -154px; left: -154px;
    width: 384px; height: 384px; border-radius: 50%; background: var(--navy);
  }
  section.closing .c2 {
    position: absolute; right: -134px; bottom: -134px;
    width: 384px; height: 384px; border-radius: 50%; background: var(--teal);
  }
  section.closing .rule {
    position: absolute; top: 341px; left: 0; width: 100%;
    height: 3px; background: var(--accent);
  }
  section.closing .logo-plate {
    position: absolute; top: 52px; left: 50%; transform: translateX(-50%);
    background: #ffffff; border: 1px solid var(--border); border-radius: 12px;
    padding: 12px 26px;
  }
  section.closing .logo-plate img { height: 60px; display: block; }
  section.closing .thanks { position: absolute; top: 372px; left: 0; width: 100%; text-align: center; }
  section.closing .thanks .big { font-size: 60px; font-weight: 700; color: var(--navy); margin: 0; }
  section.closing .thanks .sub { font-size: 30px; color: var(--teal); margin: 12px 0 0; }
  section.closing .thanks .names { font-size: 22px; color: var(--gray); margin: 30px 0 0; }
  section.closing .thanks .course { font-size: 16px; color: var(--gray); margin: 8px 0 0; }
---

<!-- _class: title -->
<!-- _paginate: false -->
<!-- _header: "" -->
<!-- _footer: "" -->

<div class="c1"></div>
<div class="c2"></div>
<div class="logo-plate">

![logo FIUBA](logo-fiuba.png)

</div>
<div class="side-tag">Especialización en<br>Inteligencia Artificial<span>Facultad de Ingeniería — UBA</span></div>
<div class="main-title">
<p class="pre">Análisis de Series de Tiempo Aplicado a la</p>
<p class="big">Generación Eléctrica Mensual de Argentina</p>
<p class="years">(2015 – 2023)</p>
</div>
<div class="info-card">
<b>Curso:</b> Análisis de Series de Tiempo I (02MIA2026)<br>
<b>Programa:</b> Especialización en Inteligencia Artificial — UBA, FIUBA<br>
<b>Docente:</b> Camilo Argoty<br>
<b>Alumnos:</b> Gustavo Rivas · Carlos Rivas · Fermín Rodríguez
</div>

<!--
Buenas tardes. Vamos a presentar nuestro trabajo final de Análisis de Series de Tiempo,
donde aplicamos distintas técnicas de modelado y pronóstico sobre la generación eléctrica
mensual de Argentina entre 2015 y 2023.
Somos Gustavo Rivas, Carlos Rivas y Fermín Rodríguez, y el trabajo fue dirigido por el
profesor Camilo Argoty en el marco de la Especialización en Inteligencia Artificial de la UBA.
A lo largo de la presentación vamos a mostrar cómo analizamos los datos, qué modelos probamos
y cuál resultó más preciso para predecir la generación eléctrica.
-->

---

## Trabajo Final

# Agenda

1. De qué se trata el trabajo y qué queremos responder
2. Los datos: de dónde vienen y cómo son
3. Qué patrones encontramos (tendencia, estacionalidad)
4. Los modelos que probamos: ARIMA, SARIMA, Holt-Winters y Prophet
5. Cómo comparamos los modelos y cuál ganó
6. Validación: ¿el modelo ganador está bien armado?
7. Un extra: medir la volatilidad con GARCH
8. Pronóstico para 2024 y conclusiones

<!--
Así está organizada la presentación: primero contamos qué pregunta nos motivó y de dónde
sacamos los datos. Después mostramos qué patrones encontramos en la serie -tendencia y
estacionalidad-, y presentamos los cuatro modelos que construimos.
Luego viene la parte central: comparamos el desempeño de los modelos, validamos que el
ganador esté bien especificado, y agregamos dos extensiones -selección automática de
órdenes y un modelo de volatilidad GARCH-. Cerramos con el pronóstico para 2024 y las
conclusiones generales.
-->

---

## 1. Pregunta de Investigación

# ¿Qué queremos responder?

> **¿Qué modelo predice mejor la generación eléctrica mensual de Argentina: ARIMA, SARIMA, Holt-Winters o Prophet? ¿Y qué tan importante es la fuerte estacionalidad anual —con picos en verano e invierno— para el resultado?**

En otras palabras, queremos lograr tres cosas:

- **Entender cómo se mueve la generación eléctrica**: ¿tiene tendencia? ¿se repite un patrón cada año?
- **Probar cuatro modelos distintos** y comparar qué tan bien predicen, apoyándonos en pruebas estadísticas formales para justificar cada decisión (no "a ojo").
- **Elegir el mejor modelo** y usarlo para proyectar la generación eléctrica de 2024, con un rango de confianza.

<!--
La pregunta central del trabajo es bastante simple de entender: queremos saber cuál de
estos cuatro modelos predice mejor la generación eléctrica mes a mes en Argentina, y
sobre todo, cuánto importa la estacionalidad -es decir, esos picos que se repiten todos
los veranos e inviernos-.
Para responderla nos propusimos tres cosas: primero entender los patrones de la serie
(tendencia y estacionalidad), después poner a competir los cuatro modelos con pruebas
estadísticas que justifiquen cada decisión, y finalmente usar el ganador para proyectar
2024 con un margen de confianza.
-->

---

## 1. Marco Teórico

# Dos formas distintas de "mirar" una serie

- **Familia 1 — ARIMA / SARIMA:**
  - Necesitan que la serie sea "estable" (estacionaria): sin tendencias raras ni cambios de comportamiento en el tiempo
  - Si no lo es, se la transforma (diferenciando) hasta que lo sea
  - Aprenden la dependencia entre un mes y los anteriores
- **Familia 2 — Holt-Winters / Prophet:**
  - No exigen esa estabilidad previa
  - Separan directamente la serie en "nivel", "tendencia" y "estacionalidad", y los van actualizando mes a mes

> **Nuestra hipótesis**
> Como la generación eléctrica argentina tiene un patrón estacional muy marcado, esperamos que los modelos que lo capturan explícitamente (SARIMA, Holt-Winters, Prophet) le ganen por bastante a un modelo que no lo tiene en cuenta (ARIMA).
> Además, notamos que algunos meses son más "volátiles" que otros, así que sumamos un modelo extra (GARCH) para medir esa variabilidad.

<!--
Para entender por qué elegimos estos cuatro modelos, conviene pensarlos en dos grandes
familias. La primera son ARIMA y SARIMA, que vienen de la estadística clásica: necesitan
que la serie sea "estable" en el tiempo, y si no lo es, se la transforma diferenciándola.
La segunda familia es Holt-Winters y Prophet, que no exigen esa estabilidad previa: separan
directamente la serie en nivel, tendencia y estacionalidad, y van actualizando esos
componentes a medida que llegan nuevos datos.
Nuestra hipótesis de partida es simple: como la demanda eléctrica argentina tiene un
patrón estacional muy marcado -veranos e inviernos-, esperamos que los modelos que lo
capturan directamente le ganen claramente a ARIMA, que no lo modela. Y como notamos que
hay meses más volátiles que otros, agregamos GARCH como un análisis extra para medir esa
variabilidad.
-->

---

## 2. Descripción de los Datos

# De dónde vienen nuestros datos

- **Dataset público de Kaggle**: "Electricity Production Dataset" (autor: sazidthe1)
  - Trae datos mensuales de generación eléctrica de 48 países, entre 2010 y 2023
  - Nos quedamos solo con **Argentina** y con la generación neta total ("Net Electricity Production")
  - Resultado: una serie de **108 meses**, de enero 2015 a diciembre 2023
- Después de filtrar, la serie con la que trabajamos es muy simple:

| Columna | Qué significa |
|---|---|
| **Fecha (mes)** | Un dato por mes, de enero de 2015 a diciembre de 2023 |
| **Generación (GWh)** | Cuánta electricidad se generó en Argentina ese mes, en gigavatios-hora |

<!--
Los datos vienen de un dataset público de Kaggle que tiene la generación eléctrica
mensual de 48 países. De ahí nos quedamos solo con Argentina, y dentro de los distintos
indicadores que trae, usamos la generación neta total -para no mezclar ni duplicar
valores por tipo de fuente de energía-.
El resultado es una serie muy simple de leer: un valor de generación eléctrica, en
gigavatios-hora, por cada uno de los 108 meses entre enero de 2015 y diciembre de 2023.
Esa es la serie univariada sobre la que trabajamos todo el análisis.
-->

---

## 2.3 Calidad de los Datos

# ¿Los datos están "limpios"?

<div class="cols top">
<div>

- **No hay valores faltantes**
- **No falta ningún mes**: la serie está completa y es continua
- **No detectamos valores atípicos** (outliers)
- **Conclusión**: no hicieron falta correcciones, la serie está lista para analizarse tal cual.

</div>
<div>

| Estadístico | Valor (GWh) |
|---|---|
| Cantidad de meses | 108 |
| Promedio | 33.811 |
| Desvío estándar | 3.058 |
| Mínimo | 25.656 |
| Percentil 25 | 31.450 |
| Mediana | 33.508 |
| Percentil 75 / Máximo | 35.881 / 40.883 |

</div>
</div>

> La variación entre meses representa solo **~9% del promedio**: es decir, la serie no "salta" de forma errática, gran parte de ese movimiento se explica por la época del año (estacionalidad).

<!--
Antes de modelar, lo primero es chequear que los datos estén bien. Y en este caso la
buena noticia es que la serie viene muy limpia: no hay meses faltantes, no hay valores
nulos, y al aplicar el método estándar de detección de outliers no encontramos ninguno.
Mirando las estadísticas descriptivas, la generación promedio es de unos 33.800 GWh por
mes, con una variación de alrededor del 9% respecto a ese promedio. Esa variación
relativamente moderada es una primera pista de que el comportamiento de la serie está
dominado por un patrón que se repite -la estacionalidad- y no por saltos erráticos.
-->

---

## 2.4 Primer Vistazo a los Datos

# Así se ve la serie completa

<div class="cols">
<div class="grow">

![Serie original de generación eléctrica](informe_assets/13_serie_original.png)

</div>
<div>

- **Nivel promedio**: ~33.800 GWh por mes
- No hay una **tendencia** clara de crecimiento o caída en los 9 años
- Pero sí hay una **baja marcada en los últimos años**: -11,6% en 2022 y -19,3% en 2023
- Se repite todos los años el mismo patrón: **sube en verano e invierno**
- Se ven caídas puntuales en momentos puntuales (ej. **pandemia 2020**)

</div>
</div>

<!--
Este es el primer gráfico que armamos, y ya cuenta gran parte de la historia. Se ve que
la serie oscila siempre alrededor de un nivel promedio de unos 33.800 GWh por mes, sin
una tendencia clara de crecimiento o caída a lo largo de los nueve años -de hecho hicimos
una prueba estadística (correlación de Spearman) que confirma que no hay una tendencia
significativa-.
Lo que sí se nota a simple vista son dos cosas: primero, un patrón que se repite todos
los años, con subidas en verano e invierno; y segundo, una caída bastante marcada en los
últimos dos años -2022 y 2023-, que más adelante vamos a ver que le complica la vida a
todos los modelos al momento de evaluar.
-->

---

## 2.5 Tasas de Crecimiento

# ¿Cuánto creció (o cayó) cada año?

<div class="cols">
<div class="grow">

![Tasas de crecimiento anuales](informe_assets/16_tasas_crecimiento.png)

</div>
<div>

| Año | Variación |
|---|---|
| 2015 | +0,07 % |
| 2016 | -2,93 % |
| 2017 | -5,38 % |
| 2018 | -14,06 % |
| 2019 | +0,87 % |
| 2020 | +2,10 % |
| 2021 | -0,46 % |
| **2022** | **-11,64 %** |
| **2023** | **-19,26 %** |

</div>
</div>

<!--
Esta tabla muestra cuánto varió la generación eléctrica de un año a otro. Se ve que en
la mayoría de los años los cambios son moderados, para arriba o para abajo. Pero en los
dos últimos años -2022 y 2023, resaltados en naranja- la caída es mucho más fuerte:
-11,6% y -19,3%.
Esto es importante porque justamente esos dos años -sobre todo 2023- son los que vamos a
usar para evaluar qué tan bien predicen los modelos. Como ningún modelo "sabe" de
antemano que viene esa caída tan fuerte, todos van a tener cierto error en ese tramo.
Además, esta variación mes a mes no es siempre igual: hay períodos más tranquilos y
otros más movidos, algo que retomamos más adelante con el modelo GARCH.
-->

---

## 3.1 Tendencia, Estacionalidad y Residuo

# Separando la serie en sus "piezas"

<div class="cols">
<div>

![h:330](informe_assets/19_descomposicion_aditiva.png)

</div>
<div>

![h:330](informe_assets/21_descomposicion_multiplicativa.png)

</div>
</div>

Toda serie se puede pensar como **Tendencia + Estacionalidad + Residuo** (esquema "aditivo", izquierda) o como **Tendencia × Estacionalidad × Residuo** ("multiplicativo", derecha).

En nuestro caso, ambos esquemas dejan un residuo muy parecido (~1.550 GWh), así que probamos las dos variantes más adelante con Holt-Winters.

<!--
Para entender mejor la serie, la "desarmamos" en tres piezas: una tendencia de largo
plazo, un patrón estacional que se repite cada año, y un residuo -lo que queda sin
explicar-. Hay dos formas de combinar esas piezas: sumándolas (esquema aditivo, a la
izquierda) o multiplicándolas (esquema multiplicativo, a la derecha).
La diferencia práctica es si el tamaño de los picos estacionales se mantiene constante
en GWh (aditivo) o si crece en proporción al nivel de la serie (multiplicativo). En
nuestro caso, ambos esquemas dejan residuos muy similares, por lo que no descartamos
ninguno de entrada: más adelante probamos las dos variantes con Holt-Winters y dejamos
que los resultados decidan.
-->

---

## 3.1 Estacionalidad y Tendencia

# El patrón que se repite cada año

<div class="cols">
<div>

![h:330](informe_assets/22_patron_estacional.png)

</div>
<div>

![h:330](informe_assets/24_medias_moviles.png)

</div>
</div>

**Izquierda**: el promedio de generación por mes muestra dos picos —verano (enero/febrero, por el aire acondicionado) e invierno (junio/julio, por calefacción)— con valles en las épocas de transición.

**Derecha**: suavizando la serie con un promedio móvil de 12 meses, se ve un nivel estable entre 2015 y 2021, y la caída sostenida de 2022-2023.

<!--
Este gráfico de la izquierda es clave para entender todo el trabajo: muestra el promedio
de generación para cada mes del año, y se ve clarísimo un patrón "bimodal", con dos
picos -uno en verano, por el uso de aire acondicionado, y otro en invierno, por la
calefacción-, con valles en otoño y primavera.
Este es justamente el patrón que un modelo sin componente estacional, como ARIMA simple,
no puede captar. A la derecha, suavizando la serie con un promedio de 12 meses, se ve el
nivel general: bastante estable hasta 2021, y con la caída sostenida que ya mencionamos
en 2022-2023.
-->

---

## 3.1 Diagnóstico de los Residuos

# ¿Quedó algo sin explicar?

<div class="cols">
<div class="grow">

![h:360](informe_assets/26_residuos_descomposicion.png)

</div>
<div>

![h:175](informe_assets/28_acf_residuos_descomposicion.png)

| Chequeo | Resultado | p-valor |
|---|---|---|
| ¿Son normales? | **Sí** | 0,184 |
| ¿Son estables? | **Sí** | 0,000 |

</div>
</div>

> Lo que sobra después de sacar tendencia y estacionalidad se comporta como "ruido": no tiene patrones escondidos. **Buena señal.**

<!--
Una vez que separamos tendencia y estacionalidad, queda un "resto" -el residuo-. La
pregunta es: ¿ese resto tiene algún patrón escondido que se nos pasó, o es simplemente
ruido sin estructura?
Hicimos dos chequeos: uno de normalidad, que confirma que los residuos se distribuyen
de forma normal; y uno de estacionariedad sobre esos residuos, que confirma que son
estables en el tiempo. En el gráfico de autocorrelación de la derecha tampoco se ven
patrones llamativos. Conclusión: la descomposición en tendencia + estacionalidad explica
bien la serie, y lo que queda es efectivamente "ruido".
-->

---

## 3.2 Pruebas de Estacionariedad

# ¿La serie es "estable"? (ADF y KPSS)

- Para usar ARIMA/SARIMA, la serie tiene que ser "estable" en el tiempo (estacionaria)
- Usamos **dos pruebas que se complementan**: si las dos coinciden, la conclusión es más confiable

| Versión de la serie | Prueba 1 (ADF) | Prueba 2 (KPSS) | ¿Es estable? |
|---|---|---|---|
| Serie original | No pasa | Pasa | No del todo |
| Con 1 diferencia simple (d=1) | Pasa | Pasa | Sí |
| Con 1 diferencia estacional (D=1) | No pasa | Pasa | No del todo |
| **Con ambas diferencias (d=1 y D=1)** | **Pasa** | **Pasa** | **Sí, la mejor opción** |

> **Conclusión**: hace falta aplicar las dos diferenciaciones —una simple y una estacional— para que la serie quede "estable". Por eso el SARIMA que construimos usa **d=1 y D=1**.

<!--
Antes de armar el ARIMA y el SARIMA, tenemos que chequear si la serie es "estable" en el
tiempo -en la jerga, "estacionaria"-. Para eso usamos dos pruebas estadísticas distintas
que se complementan: si ambas coinciden en la misma conclusión, podemos confiar más en
el resultado.
La tabla resume lo que encontramos: la serie original no pasa del todo la primera prueba.
Aplicando una diferenciación simple -comparar cada mes con el anterior- mejora bastante.
Aplicando solo la diferenciación estacional -comparar cada mes con el mismo mes del año
anterior- no alcanza. Pero combinando las dos diferenciaciones, la serie queda estable
según ambas pruebas. Por eso el SARIMA que armamos usa esas dos diferenciaciones, lo que
en la jerga técnica se anota como d=1 y D=1.
-->

---

## 3.3 Resumen de Modelos

# Los 4 modelos que vamos a comparar

| Modelo | ¿Qué lo distingue? | ¿Tiene en cuenta la estacionalidad? |
|---|---|---|
| **ARIMA** | El más simple: solo mira los valores recientes. Nuestro "punto de partida" | No |
| **SARIMA** | Como ARIMA, pero agrega el ciclo de 12 meses | Sí |
| **Holt-Winters (aditivo)** | Suaviza nivel, tendencia y estacionalidad como cantidades fijas | Sí |
| **Holt-Winters (multiplicativo)** | Igual, pero la estacionalidad es un % del nivel | Sí |
| **Prophet** | Modelo de Meta/Facebook, ajusta tendencia y estacionalidad automáticamente | Sí (anual) |

> **Para que la comparación sea justa**: entrenamos todos los modelos con los datos de **2015-2022**, y los evaluamos prediciendo **2023** (12 meses que ningún modelo vio antes).

<!--
Acá tenemos los cuatro modelos que vamos a comparar. ARIMA es el más simple y nuestro
punto de partida -no tiene en cuenta la estacionalidad-. SARIMA es básicamente ARIMA más
el ciclo de 12 meses. Holt-Winters tiene dos versiones, aditiva y multiplicativa, que se
diferencian en cómo tratan el tamaño de la estacionalidad. Y Prophet es el modelo de Meta,
que ajusta automáticamente tendencia y estacionalidad sin que tengamos que elegir tantos
parámetros a mano.
Para que la comparación sea justa, a todos los entrenamos con los datos de 2015 a 2022, y
los evaluamos viendo qué tan bien predicen 2023 -que ninguno vio durante el entrenamiento-.
-->

---

## 3.3 Descripción de Modelos

# Modelo 1: ARIMA — el punto de partida

<div class="cols">
<div class="grow">

![Pronóstico ARIMA](informe_assets/37_forecast_arima.png)

</div>
<div>

- Mira solo el **mes anterior** y los **errores recientes** para predecir el siguiente
- No sabe nada de "verano" o "invierno": **no tiene componente estacional**
- Por eso esperamos que sea el que **peor prediga** frente a un patrón tan estacional
- Sirve como **"piso"**: si otro modelo no le gana a este, no vale la pena usarlo

</div>
</div>

<!--
Empezamos con ARIMA, el modelo más simple de los cuatro. Básicamente mira el valor del
mes anterior y los errores recientes para proyectar el siguiente mes, pero no tiene
ningún concepto de "ciclo anual": no sabe que enero se parece a otros eneros.
Por eso esperamos que sea el que peor desempeño tenga, justamente porque nuestra serie
tiene una estacionalidad tan marcada. Lo incluimos como punto de referencia: si los demás
modelos no le ganan claramente a ARIMA, no estaría justificado usar algo más complejo.
En el gráfico se puede ver que su pronóstico para 2023 es bastante "plano", sin replicar
los picos de verano e invierno.
-->

---

## 3.3 Descripción de Modelos

# Modelo 2: SARIMA — ARIMA + estacionalidad

<div class="cols">
<div class="grow">

![Pronóstico SARIMA](informe_assets/39_forecast_sarima.png)

</div>
<div>

- Es ARIMA, pero además **compara cada mes con el mismo mes del año anterior**
- Las dos diferenciaciones (**d=1 y D=1**) que vimos antes están incorporadas acá
- Es el modelo **"candidato natural"** para una serie tan estacional como la nuestra
- Esperamos que sea **de los mejores**, ya que ataca directamente el patrón anual

</div>
</div>

<!--
SARIMA es, en esencia, ARIMA potenciado con un componente estacional. Además de mirar el
mes anterior, compara cada mes con el mismo mes del año pasado -por ejemplo, este enero
con el enero anterior-. Las dos diferenciaciones que justificamos con las pruebas ADF y
KPSS están incorporadas en este modelo.
Por construcción, este es el candidato más lógico para una serie con un patrón estacional
tan fuerte como el nuestro, y en el gráfico ya se nota: el pronóstico para 2023 sigue la
forma de los picos de verano e invierno, a diferencia del pronóstico plano de ARIMA.
-->

---

## 3.3 Descripción de Modelos

# Modelo 3: Holt-Winters — suavizado inteligente

<div class="cols">
<div>

![Holt-Winters aditivo](informe_assets/41_forecast_hw_aditivo.png)

</div>
<div>

![Holt-Winters multiplicativo](informe_assets/43_forecast_hw_multiplicativo.png)

</div>
</div>

- Separa la serie en 3 "piezas" —**nivel, tendencia y estacionalidad**— y las va actualizando mes a mes a medida que llegan nuevos datos
- **Versión aditiva** (izquierda): la estacionalidad suma/resta siempre la misma cantidad de GWh
- **Versión multiplicativa** (derecha): la estacionalidad es un porcentaje del nivel actual

<!--
Holt-Winters funciona distinto: en lugar de buscar fórmulas matemáticas complejas, separa
la serie en tres piezas -nivel, tendencia y estacionalidad- y las va "suavizando" y
actualizando mes a mes, dándole más o menos peso a los datos recientes.
Probamos dos versiones: la aditiva, donde el efecto estacional siempre suma o resta la
misma cantidad de GWh; y la multiplicativa, donde ese efecto es un porcentaje del nivel
actual. Como vimos en la descomposición, ambas variantes eran candidatas razonables, así
que las probamos a las dos y dejamos que la comparación final decida cuál funciona mejor.
-->

---

## 3.3 Descripción de Modelos

# Modelo 4: Prophet — el de Meta/Facebook

<div class="cols">
<div class="grow">

![Pronóstico Prophet](informe_assets/45_forecast_prophet.png)

</div>
<div>

- Divide la serie en **tendencia + estacionalidad**, igual que los anteriores, pero ajusta todo **automáticamente**
- Usa una técnica matemática (**series de Fourier**) para representar la estacionalidad anual
- No necesitamos elegir tantos parámetros a mano ni verificar estacionariedad
- Lo incluimos como cuarto modelo para tener un punto de comparación **"moderno"** y de uso muy extendido en la industria

</div>
</div>

<!--
El cuarto modelo es Prophet, desarrollado por Meta y muy usado en la industria para
pronósticos de negocio. También separa la serie en tendencia y estacionalidad, pero lo
hace de forma automática, usando una técnica matemática llamada series de Fourier para
representar el ciclo anual.
La ventaja es que no tenemos que elegir tantos parámetros a mano ni preocuparnos por la
estacionariedad. Lo incluimos como un cuarto modelo de referencia, muy usado en la
práctica, para ver cómo se compara contra los enfoques más "clásicos" como SARIMA y
Holt-Winters.
-->

---

## 4.1 Comparación de Modelos (2023)

# ¡El momento de la verdad! ¿Quién ganó?

<div class="cols">
<div class="grow">

![Comparación de modelos](informe_assets/47_comparacion_modelos.png)

</div>
<div>

| Modelo | Error promedio (MAPE) |
|---|---|
| **SARIMA** | **8,15 %** |
| Holt-Winters (multiplicativo) | 9,02 % |
| Holt-Winters (aditivo) | 9,32 % |
| Prophet | 11,61 % |
| ARIMA | 13,01 % |

<p class="caption">El MAPE es el error promedio en porcentaje: "en promedio, el modelo se equivoca un X% del valor real". Cuanto más bajo, mejor.</p>

</div>
</div>

> **Ganó SARIMA con 8,15% de error.** La diferencia con ARIMA (13,01%) muestra cuánto aporta modelar la estacionalidad.

<!--
Este es el resultado clave de todo el trabajo. Para comparar los modelos usamos el MAPE,
que es el error promedio expresado en porcentaje: si el MAPE es 8%, en promedio el
modelo se equivoca un 8% del valor real. Es una métrica fácil de comunicar porque no
depende de la unidad -GWh-, sino que es relativa.
El ganador fue SARIMA, con un error de 8,15%. Le siguen las dos versiones de
Holt-Winters, alrededor del 9%, después Prophet con 11,6%, y por último ARIMA con 13%.
Este resultado confirma nuestra hipótesis inicial: el modelo que mejor capta el patrón
estacional -SARIMA- es el que predice mejor, y la diferencia con ARIMA -que no lo tiene
en cuenta- es de casi 5 puntos porcentuales. Usamos también otras dos métricas, MAE y
RMSE, y el orden de los modelos es el mismo en las tres, lo que le da más solidez a esta
conclusión.
-->

---

## 4.2 SARIMA vs. la Realidad

# Mirando de cerca al ganador: SARIMA

<div class="cols">
<div>

![SARIMA vs valores reales](informe_assets/51_sarima_vs_real.png)

</div>
<div>

![Real vs predicho SARIMA](informe_assets/58_real_vs_predicho_sarima.png)

</div>
</div>

> La línea de predicción de SARIMA **sigue de cerca a la línea real**, incluyendo los picos estacionales. Las mayores diferencias aparecen en los meses donde la caída de 2023 fue más fuerte.

<!--
Acá podemos ver en detalle qué tan bien predijo SARIMA comparado con lo que realmente
pasó en 2023. La línea de predicción sigue de cerca a la línea real, incluyendo la forma
de los picos de verano e invierno -algo que, como vimos, ARIMA no lograba-.
Las mayores diferencias entre predicción y realidad aparecen en los meses donde la
caída de generación de 2023 fue más pronunciada, justamente porque el modelo se entrenó
con datos hasta 2022 y no podía "saber" que venía esa baja tan marcada.
-->

---

## 4.3 Validación de Residuos

# ¿SARIMA está bien armado?

<div class="cols">
<div class="grow">

![ACF y PACF de los residuos de SARIMA](informe_assets/53_acf_pacf_residuos_sarima.png)

</div>
<div>

| Horizonte | ¿Hay patrones sin explicar? | p-valor |
|---|---|---|
| 6 meses | No | 0,946 |
| 12 meses | No | 0,125 |
| 18 meses | No | 0,302 |
| 24 meses | No | 0,639 |

</div>
</div>

> Esta prueba (**Ljung-Box**) chequea si quedó algún patrón sin explicar en lo que el modelo no pudo predecir. En todos los horizontes —incluso a 12 y 24 meses, los "ciclos" anuales— la respuesta es "no": **SARIMA capturó bien tanto el corto plazo como el ciclo anual.**

<!--
Que SARIMA haya tenido el menor error no es suficiente: también queremos confirmar que
está "bien armado", es decir, que no dejó información sin aprovechar. Para eso miramos
los residuos -la diferencia entre lo predicho y lo real- y les hacemos una prueba
llamada Ljung-Box, que chequea si queda algún patrón escondido.
La tabla muestra que, para todos los horizontes que probamos -incluso a 12 y 24 meses,
que son justamente los "ciclos" anuales-, la respuesta es que no quedan patrones sin
explicar. Esto confirma que SARIMA no solo predijo mejor, sino que está correctamente
especificado: no hay una versión "más simple que se nos escapó" que pudiera mejorar el
resultado.
-->

---

<!-- _class: compact -->

## 4.4 Extensión 1: Selección Automática

# ¿Y si lo elegía la computadora?

<div class="cols top">
<div class="grow">

| Configuración SARIMA | Puntaje (AIC, menor = mejor ajuste) |
|---|---|
| **(1,1,1)(0,1,1,12)  ← elegido por la PC** | **1244,08** |
| **(1,1,1)(1,1,1,12)  ← el nuestro** | **1245,97** |
| (0,1,1)(0,1,1,12) | 1248,39 |
| (0,1,1)(1,1,1,12) | 1250,38 |
| (1,0,1)(0,1,1,12) | 1261,68 |

</div>
<div>

| Configuración | Error (MAPE) prediciendo 2023 |
|---|---|
| Elegida por la PC | 9,47 % |
| **La nuestra (manual)** | **8,15 %** |

</div>
</div>

Probamos también una **búsqueda automática** que recorre muchas combinaciones y elige la de mejor "puntaje" interno (AIC).

> La diferencia de puntaje es mínima, pero a la hora de predecir 2023 de verdad, **nuestra configuración elegida "a mano" predijo mejor** (8,15% vs. 9,47%). **Moraleja**: un buen puntaje interno no garantiza el mejor pronóstico real, por eso siempre hay que validar contra datos reales.

<!--
Como extensión, probamos qué pasaría si dejábamos que una búsqueda automática eligiera
la configuración de SARIMA, en lugar de elegirla nosotros. Esa búsqueda recorre muchas
combinaciones posibles y elige la que tiene mejor "puntaje" interno, llamado AIC, que
mide qué tan bien se ajusta el modelo a los datos de entrenamiento penalizando la
complejidad.
La configuración elegida automáticamente tuvo un puntaje apenas mejor que la nuestra.
Pero cuando las hicimos competir prediciendo 2023 -los datos reales que nos interesan-,
nuestra configuración elegida a mano predijo mejor: 8,15% de error contra 9,47%. La
moraleja es importante: un buen puntaje "interno" no garantiza el mejor pronóstico en la
práctica, por eso es clave validar siempre contra datos reales que el modelo no vio.
-->

---

<!-- _class: compact -->

## 4.5 Extensión 2: Volatilidad (GARCH)

# Bonus: ¿qué tan "nerviosa" es la serie?

<div class="cols">
<div class="grow">

![h:330](informe_assets/63_garch_volatilidad.png)

</div>
<div>

| Parámetro | ¿Qué mide? | ¿Es relevante? |
|---|---|---|
| **ω** | Nivel "normal" de volatilidad | Sí |
| **α** (shocks) | Reacción a una sorpresa puntual | No tanto |
| **β** (memoria) | Cuánto dura un período movido | Sí |

</div>
</div>

Todos los modelos anteriores predicen el valor promedio. **GARCH va un paso más allá**: predice qué tan "ancho" debería ser el margen de error mes a mes.

Encontramos que **los períodos movidos tienden a continuar un tiempo** (hay "memoria"), pero un shock puntual no dispara por sí solo un período de alta volatilidad.

> **En la práctica**: permite dar intervalos de confianza más realistas, más anchos cuando "está movido" y más angostos cuando está tranquilo.

<!--
Como bonus, agregamos un análisis de volatilidad con un modelo llamado GARCH. La idea es
distinta a todo lo anterior: en lugar de predecir el valor promedio, predice qué tan
"ancho" debería ser el margen de error en cada momento -es decir, mide qué tan "nerviosa"
está la serie-.
Encontramos que hay cierta "memoria" en la volatilidad: si un mes es movido, es más
probable que el siguiente también lo sea. Pero un shock puntual aislado no necesariamente
dispara por sí solo un período de alta volatilidad. En la práctica, esto nos permite dar
intervalos de confianza más realistas: más anchos en los períodos movidos, y más angostos
en los tranquilos, en lugar de un margen de error fijo para todos los meses.
-->

---

## 4.6 Pronóstico Operativo

# ¿Y qué esperamos para 2024?

<div class="cols">
<div class="grow">

![Pronóstico 2024 con SARIMA](informe_assets/65_pronostico_2024.png)

</div>
<div>

| Mes | Pronóstico | Mínimo esperado | Máximo esperado |
|---|---|---|---|
| Enero | 35.332 | 31.161 | 39.503 |
| Febrero | 29.570 | 24.767 | 34.373 |
| Marzo | 33.997 | 28.937 | 39.057 |
| Junio | 32.933 | 27.512 | 38.353 |
| Julio | 32.693 | 27.182 | 38.203 |
| Diciembre | 30.696 | 24.772 | 36.619 |

<p class="caption">Valores en GWh. Tabla resumida (6 de 12 meses) — el detalle completo está en el informe.</p>

</div>
</div>

<!--
Por último, usamos SARIMA -nuestro modelo ganador- reentrenado con todos los datos
disponibles, de 2015 a 2023, para proyectar los 12 meses de 2024. El gráfico muestra esa
proyección junto con una banda de confianza del 95%.
Como era de esperar, el pronóstico reproduce el mismo patrón bimodal que vimos en toda la
serie: picos en enero y en junio-julio. La tabla muestra, para algunos meses, el valor
esperado y el rango -mínimo y máximo- dentro del cual esperamos que caiga el valor real
con un 95% de confianza. Esta es la salida que un equipo de planificación energética
podría usar como insumo para sus decisiones.
-->

---

## 5. Conclusiones

# Lo que aprendimos (parte 1)

- **Respondiendo la pregunta inicial**: SARIMA fue el mejor (8,15% de error), y la estacionalidad anual resultó ser **el factor más importante** para predecir bien.
- El orden final fue: **SARIMA**, después **Holt-Winters** (~9%), después **Prophet** (11,6%) y por último **ARIMA** (13%) — confirmando que cuanto mejor se modela la estacionalidad, mejor se predice.
- Entre las dos versiones de Holt-Winters, **la multiplicativa fue apenas mejor** (9,02% vs. 9,32%) — una diferencia pequeña, esperable según lo que ya habíamos visto en la descomposición.
- La **fuerte caída de 2022-2023** fue el principal "dolor de cabeza" para todos los modelos: ninguno podía anticipar ese quiebre sin información externa (ej. actividad económica).

<!--
Para cerrar, repasemos las conclusiones principales. La pregunta que nos hicimos al
principio queda respondida: SARIMA fue el modelo más preciso, con un error de 8,15%, y
esto confirma que la estacionalidad anual es el factor más determinante para predecir
bien esta serie.
El orden final -SARIMA, Holt-Winters, Prophet y ARIMA- tiene una lectura clara: cuanto
mejor modela un método la estacionalidad, mejor predice. Entre las dos versiones de
Holt-Winters la diferencia fue mínima, algo que ya habíamos anticipado en la
descomposición. Y un punto importante: la fuerte caída de generación en 2022-2023 fue el
principal desafío para todos los modelos, porque ninguno podía "adivinar" ese cambio sin
información externa, como por ejemplo el nivel de actividad económica del país.
-->

---

## 5. Conclusiones y Aprendizajes

# Lo que aprendimos (parte 2)

- El **"piloto automático"** (selección por AIC) **no siempre gana**: nuestra configuración elegida a mano predijo mejor en datos reales (8,15% vs. 9,47%).
- **GARCH** mostró que la volatilidad "tiene memoria": los períodos movidos tienden a continuar, lo que permite dar márgenes de error más realistas.
- Validamos que SARIMA no solo ganó en precisión, sino que **está bien especificado**: no deja patrones sin explicar en sus errores.
- **Aprendizaje general**: ningún indicador por sí solo (ni el puntaje interno, ni el error de predicción, ni las pruebas de residuos) alcanza — hay que mirar **los tres juntos** para elegir con confianza.

<!--
Algunas reflexiones finales. Primero, vimos que el "piloto automático" de selección de
modelos no siempre acierta: la configuración elegida a mano predijo mejor en datos
reales que la elegida automáticamente por puntaje interno.
Segundo, el análisis de volatilidad con GARCH mostró que los períodos "movidos" tienden a
durar un tiempo, lo que en la práctica permite dar márgenes de error más realistas y no
un número fijo para todos los meses.
Tercero, no solo nos quedamos con que SARIMA tuvo el menor error: también confirmamos que
está bien especificado, sin patrones sin explicar en sus errores.
Y como aprendizaje general del trabajo: ningún indicador por sí solo es suficiente. Hay
que combinar el ajuste interno del modelo, el error prediciendo datos reales, y el
diagnóstico de los residuos para elegir un modelo con confianza. Con esto cerramos la
presentación, ¡gracias!
-->

---

<!-- _class: closing -->
<!-- _paginate: false -->
<!-- _header: "" -->
<!-- _footer: "" -->

<div class="c1"></div>
<div class="c2"></div>
<div class="rule"></div>
<div class="logo-plate">

![logo FIUBA](logo-fiuba.png)

</div>
<div class="thanks">
<p class="big">Muchas gracias</p>
<p class="sub">Preguntas y discusión</p>
<p class="names">Gustavo Rivas · Carlos Rivas · Fermín Rodríguez</p>
<p class="course">Análisis de Series de Tiempo (02MIA2026) — Especialización en IA, UBA FIUBA</p>
</div>

<!--
Muchas gracias por la atención. Quedamos abiertos a preguntas sobre cualquiera de los
modelos, los resultados o las extensiones que presentamos.
-->
