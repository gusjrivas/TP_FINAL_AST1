# Informe de Análisis de Series Temporales
## Generación Eléctrica Mensual de Argentina (2015-2023)

**Curso:** Análisis de Series de Tiempo (02MIA2026) — Especialización en Inteligencia Artificial, UBA FIUBA
**Docente:** Camilo Argoty
**Alumnos:** Gustavo Rivas, Carlos Rivas, Fermín Rodríguez
**Notebook de referencia:** [`TP_Final_Generacion_Electrica_Argentina_v1.ipynb`](TP_Final_Generacion_Electrica_Argentina_v1.ipynb)

---

## 0. Qué pide la consigna y dónde se resuelve

La consigna (`Modelo_TP_final_AST_MIA_3Co202.pdf`) pide dos entregables, cada uno con un peso del 50 % de la nota:

### 1) Código en Python comentado y reproducible (50 %)

Debe incluir:

| Requisito de la consigna | Dónde se resuelve en el notebook `v1` |
|---|---|
| a. Limpieza y preparación de los datos | Secciones 2 y 3 (carga del dataset de Kaggle, filtrado por Argentina, verificación de nulos/outliers/frecuencia) |
| b. Creación de modelos de series de tiempo (mínimo 3 tipos) | Sección 6: se construyeron **4 modelos** — ARIMA, SARIMA, Holt-Winters (aditivo y multiplicativo) y Prophet |
| c. Generación de pronósticos por modelo, evaluación y comparación | Secciones 6.5 y 8: pronósticos sobre el conjunto de prueba (2023) + pronóstico operativo a futuro (2024) con métricas MAE/RMSE/MAPE |

### 2) Informe de análisis (50 %)

Debe incluir los puntos **a) a e)** detallados a continuación. Este documento (`INFORME.md`) desarrolla cada uno de esos puntos, reutilizando los gráficos, tablas y resultados generados por el notebook `v1`:

| Punto de la consigna | Sección de este informe |
|---|---|
| a. Planteamiento de la pregunta de investigación | [1. Pregunta de Investigación](#1-planteamiento-de-la-pregunta-de-investigación) |
| b. Descripción de los datos (origen y tipo de cada atributo) | [2. Descripción de los Datos](#2-descripción-de-los-datos) |
| c. Descripción de los modelos (características + gráficas de la serie original y de los datos simulados) | [3. Descripción de los Modelos](#3-descripción-de-los-modelos) |
| d. Pruebas sobre los modelos (resultados de pruebas, evaluaciones y validaciones) | [4. Pruebas sobre los Modelos](#4-pruebas-sobre-los-modelos) |
| e. Conclusiones (sobre el fenómeno y los modelos, respondiendo la pregunta de investigación, lecciones aprendidas) | [5. Conclusiones](#5-conclusiones) |

---

## 1. Planteamiento de la Pregunta de Investigación

**Pregunta principal:** ¿Cuál modelo de series de tiempo (ARIMA, SARIMA, Holt-Winters o Prophet) proporciona las predicciones más precisas para la generación eléctrica mensual de Argentina, y qué papel juega la fuerte estacionalidad anual (picos de verano e invierno) en el rendimiento de cada modelo?

**Objetivos específicos:**

1. Identificar y caracterizar los patrones temporales (tendencia y estacionalidad) de la generación eléctrica argentina.
2. Evaluar la capacidad predictiva de ARIMA, SARIMA, Holt-Winters (aditivo y multiplicativo) y Prophet, justificando los órdenes de los modelos con pruebas formales de estacionariedad.
3. Determinar el modelo más adecuado para pronósticos de generación a corto plazo y emitir un pronóstico operativo para 2024.

---

## 2. Descripción de los Datos

### 2.1 Origen del dataset

- **Fuente:** [Electricity Production Dataset](https://www.kaggle.com/datasets/sazidthe1/global-electricity-production) (Kaggle, autor *sazidthe1*).
- **Cobertura original:** producción eléctrica mensual de **48 países**, entre 2010 y 2023.
- **Filtros aplicados:** `Country == "Argentina"` y `parameter == "Net Electricity Production"` (para evitar duplicar registros por tipo de combustible/fuente).
- **Cobertura efectiva de la serie de Argentina:** **108 meses, de enero de 2015 a diciembre de 2023** (los meses previos a 2015 no están reportados para el país).

### 2.2 Atributos del dataset original

| Columna | Tipo de dato | Descripción |
|---|---|---|
| `country_name` | texto (categórico) | Nombre del país (se filtra por "Argentina") |
| `date` | fecha (datetime) | Fecha del registro mensual |
| `parameter` | texto (categórico) | Tipo de medición (se usa "Net Electricity Production") |
| `product` | texto (categórico) | Fuente/producto de generación |
| `value` | numérico (float) | Valor de producción eléctrica |
| `unit` | texto (categórico) | Unidad de medida (GWh) |

Tras el filtrado y la agrupación mensual, la serie final utilizada es **univariada**:

| Columna | Tipo de dato | Descripción |
|---|---|---|
| `date` (índice) | datetime, frecuencia mensual (`MS`) | Mes de referencia, de 2015-01 a 2023-12 |
| `GenGWh` | float | Generación eléctrica neta mensual de Argentina, en GWh |

### 2.3 Calidad de los datos

- **Valores faltantes:** 0
- **Meses faltantes en la serie:** ninguno (serie mensual continua y completa)
- **Valores atípicos (método IQR):** 0 detectados
- **Estadísticas descriptivas de `GenGWh`:**

| Estadístico | Valor (GWh) |
|---|---|
| count | 108 |
| mean | 33.811,41 |
| std | 3.058,10 |
| min | 25.655,53 |
| 25% | 31.449,74 |
| 50% (mediana) | 33.508,27 |
| 75% | 35.880,90 |
| max | 40.882,60 |

### 2.4 Visualización inicial de la serie

![Serie original de generación eléctrica](informe_assets/13_serie_original.png)

**Observaciones:**

- La serie oscila en torno a un nivel medio de **~33.800 GWh/mes** (~405.000 GWh/año), **sin tendencia monotónica significativa** de largo plazo (correlación de Spearman entre tiempo y nivel: ρ ≈ -0,11; p ≈ 0,26).
- Sin embargo, se observa un **descenso marcado en los últimos años**: -11,6 % en 2022 y -19,3 % en 2023 (ver tabla de crecimiento anual más abajo). Este quiebre afecta el desempeño de todos los modelos sobre el conjunto de prueba (2023).
- Se distingue una **estacionalidad anual marcada**, coherente con un país del hemisferio sur (picos de generación en verano por aire acondicionado y en invierno por calefacción).
- Se identifican caídas puntuales asociables a eventos concretos, como el inicio de la pandemia en 2020.

### 2.5 Tasas de crecimiento

![Tasas de crecimiento mensual](informe_assets/16_tasas_crecimiento.png)

| Año | Crecimiento anual (GWh) | Tasa anual (%) |
|---|---|---|
| 2015 | +23,16 | +0,07 % |
| 2016 | -1.121,35 | -2,93 % |
| 2017 | -2.109,06 | -5,38 % |
| 2018 | -5.403,38 | -14,06 % |
| 2019 | +299,03 | +0,87 % |
| 2020 | +771,11 | +2,10 % |
| 2021 | -179,06 | -0,46 % |
| 2022 | -4.760,54 | **-11,64 %** |
| 2023 | -7.497,55 | **-19,26 %** |

---

## 3. Descripción de los Modelos

### 3.1 Análisis de componentes de la serie (previo a modelar)

Antes de seleccionar los modelos, se descompuso la serie en tendencia, estacionalidad y residuos, comparando los esquemas **aditivo** y **multiplicativo**:

| Descomposición aditiva | Descomposición multiplicativa |
|---|---|
| ![Descomposición aditiva](informe_assets/19_descomposicion_aditiva.png) | ![Descomposición multiplicativa](informe_assets/21_descomposicion_multiplicativa.png) |

- El desvío estándar de los residuos del modelo **aditivo** (≈ 1.548 GWh) resultó marginalmente menor que el equivalente del modelo **multiplicativo** (≈ 1.576 GWh), por lo que ninguno de los dos esquemas puede descartarse de antemano. Por eso se exploran ambas variantes también en Holt-Winters.

**Patrón estacional bimodal** (medias mensuales con desvío estándar):

![Patrón estacional medio por mes](informe_assets/22_patron_estacional.png)

Se confirma un patrón **bimodal**: un pico de **verano** (enero/febrero, por aire acondicionado) y un pico de **invierno** (junio/julio, por calefacción), con valles en los meses de transición (abril-mayo y septiembre-octubre).

**Tendencia con medias móviles:**

![Medias móviles](informe_assets/24_medias_moviles.png)

**Análisis de residuos de la descomposición:**

| Residuos en el tiempo / distribución / Q-Q / ACF |
|---|
| ![Residuos de la descomposición](informe_assets/26_residuos_descomposicion.png) |
| ![ACF de los residuos](informe_assets/28_acf_residuos_descomposicion.png) |

### 3.2 Pruebas de estacionariedad (justificación de los órdenes `d` y `D`)

Se aplicaron las pruebas **ADF** (H₀: la serie tiene raíz unitaria → no estacionaria) y **KPSS** (H₀: la serie es estacionaria) sobre la serie original y sobre versiones diferenciadas:

| Versión de la serie | ADF stat | ADF p-valor | Conclusión ADF | KPSS stat | KPSS p-valor | Conclusión KPSS |
|---|---|---|---|---|---|---|
| Serie original | -1,555 | 0,5064 | NO estacionaria | 0,136 | 0,100 | Estacionaria |
| 1 diferencia regular (`d=1`) | -4,235 | 0,0006 | **Estacionaria** | 0,032 | 0,100 | Estacionaria |
| 1 diferencia estacional (`D=1`, lag 12) | -2,508 | 0,1134 | NO estacionaria | 0,194 | 0,100 | Estacionaria |
| `d=1` + `D=1` | -4,141 | 0,0008 | **Estacionaria** | 0,106 | 0,100 | Estacionaria |

**Conclusión:** la diferenciación regular (`d=1`) es la que más aporta a estabilizar el nivel de la serie, mientras que la diferenciación estacional (`D=1`, lag 12) es necesaria para eliminar el patrón anual recurrente. Esto justifica empíricamente el uso de **`d=1` y `D=1`** en el modelo SARIMA `(1,1,1)(1,1,1,12)`.

### 3.3 Modelos seleccionados

Se seleccionaron **cuatro modelos** (cumpliendo y superando el mínimo de 3 que pide la consigna), en función de la estructura de tendencia + estacionalidad bimodal observada:

| Modelo | Rol | Componente estacional | Órdenes |
|---|---|---|---|
| **ARIMA** | Línea base / "piso" de comparación (sin componente estacional) | No | (1,1,1) |
| **SARIMA** | Modelo estacional principal | Sí (s=12) | (1,1,1)(1,1,1,12) |
| **Holt-Winters (aditivo)** | Suavizado exponencial con estacionalidad aditiva | Sí | tendencia aditiva, estacionalidad aditiva, período 12 |
| **Holt-Winters (multiplicativo)** | Suavizado exponencial con estacionalidad multiplicativa | Sí | tendencia aditiva, estacionalidad multiplicativa, período 12 |
| **Prophet** | Modelo aditivo (Meta), comparativo, estacionalidad por términos de Fourier | Sí (anual) | `yearly_seasonality=True`, sin estacionalidad semanal/diaria (serie mensual) |

**División entrenamiento/prueba:** se dejó el **último año completo (2023, 12 meses)** como conjunto de prueba, y los **96 meses restantes (2015-2022)** como entrenamiento.

#### ARIMA(1,1,1) — modelo base

- `p=1`: dependencia con el valor inmediatamente anterior.
- `d=1`: una diferenciación para manejar la tendencia no estacionaria.
- `q=1`: término de media móvil para shocks de corto plazo.
- **Hipótesis:** al no tener componente estacional, se espera que sea el modelo con peor desempeño frente a la fuerte estacionalidad bimodal de la serie.

![Pronóstico ARIMA](informe_assets/37_forecast_arima.png)

#### SARIMA(1,1,1)(1,1,1,12)

- Extiende ARIMA agregando componentes estacionales `(P,D,Q,s) = (1,1,1,12)`, justificados por las pruebas ADF/KPSS de la sección 3.2.

![Pronóstico SARIMA](informe_assets/39_forecast_sarima.png)

#### Holt-Winters (aditivo y multiplicativo)

- Especialmente efectivo para series con tendencia y estacionalidad bien definidas.
- Se prueba primero la variante **aditiva** y luego la **multiplicativa**, dado que la descomposición sugirió que la amplitud estacional podría crecer levemente con el nivel de la serie.

| Holt-Winters aditivo | Holt-Winters multiplicativo |
|---|---|
| ![HW aditivo](informe_assets/41_forecast_hw_aditivo.png) | ![HW multiplicativo](informe_assets/43_forecast_hw_multiplicativo.png) |

#### Prophet

- Descompone la serie en tendencia + estacionalidad + efectos de calendario de forma automática, sin necesidad de elegir órdenes `(p,d,q)`.
- Se incorpora como cuarto modelo comparativo por su buen desempeño esperado en series con tendencia y estacionalidad anual marcada.
- **Hipótesis:** se espera que sea competitivo con SARIMA al capturar la misma estacionalidad anual, pero por una vía no paramétrica (Fourier).

![Pronóstico Prophet](informe_assets/45_forecast_prophet.png)

---

## 4. Pruebas sobre los Modelos

### 4.1 Comparación de métricas de error (conjunto de prueba = 2023, 12 meses)

![Comparación de pronósticos de todos los modelos](informe_assets/47_comparacion_modelos.png)

| Modelo | MAE | RMSE | MAPE (%) |
|---|---|---|---|
| **SARIMA (1,1,1)(1,1,1,12)** | 2.507,79 | 2.991,00 | **8,15 %** |
| **Holt-Winters (multiplicativo)** | 2.779,49 | 3.123,55 | **9,02 %** |
| **Holt-Winters (aditivo)** | 2.872,08 | 3.203,34 | **9,32 %** |
| **Prophet** | 3.517,25 | 3.881,71 | **11,61 %** |
| **ARIMA (1,1,1)** | 3.877,02 | 4.617,09 | **13,01 %** |

*(Tabla ordenada de menor a mayor MAPE — corresponde a la ejecución de referencia del notebook `v1`; puede variar levemente entre corridas por la optimización de máxima verosimilitud.)*

### 4.2 Visualización del mejor modelo (SARIMA) vs. datos reales

![SARIMA vs datos reales](informe_assets/51_sarima_vs_real.png)

![Comparativa detallada real vs. predicho (SARIMA)](informe_assets/58_real_vs_predicho_sarima.png)

### 4.3 Diagnóstico de residuos del modelo SARIMA

**ACF y PACF de los residuos:**

![ACF y PACF de residuos SARIMA](informe_assets/53_acf_pacf_residuos_sarima.png)

**Test de Ljung-Box** (H₀: los residuos son ruido blanco, sin autocorrelación):

| Lag | Estadístico LB | p-valor |
|---|---|---|
| 6 | 1,684 | 0,946 |
| 12 | 17,706 | 0,125 |
| 18 | 20,554 | 0,302 |
| 24 | 20,989 | 0,639 |

**Resultado:** todos los p-valores son > 0,05, por lo que **no se rechaza la hipótesis de ruido blanco**: el modelo SARIMA capturó adecuadamente la estructura temporal de la serie.

### 4.4 Extensión 1 — Selección automática de órdenes SARIMA (grid search por AIC)

Se realizó una búsqueda sistemática sobre combinaciones de `(p,d,q)` y `(P,D,Q,12)`, eligiendo la de menor AIC (equivalente a lo que hace `pmdarima.auto_arima`, mostrando aquí el procedimiento de forma transparente):

| Configuración SARIMA | AIC |
|---|---|
| **(1,1,1)(0,1,1,12)** ← mejor por AIC | 1244,08 |
| (1,1,1)(1,1,1,12) ← elegido manualmente | 1245,97 |
| (0,1,1)(0,1,1,12) | 1248,39 |
| (0,1,1)(1,1,1,12) | 1250,38 |
| (1,0,1)(0,1,1,12) | 1261,68 |

| | MAE | RMSE | MAPE |
|---|---|---|---|
| Mejor-AIC `(1,1,1)(0,1,1,12)` | 2.923,98 | 3.314,50 | 9,47 % |
| Manual `(1,1,1)(1,1,1,12)` | 2.507,78 | 2.990,99 | **8,15 %** |

**Conclusión:** el modelo de menor AIC (mejor ajuste *in-sample*) **no** resultó ser el mejor en el conjunto de prueba (*out-of-sample*). Por eso se mantiene el SARIMA `(1,1,1)(1,1,1,12)` elegido manualmente como modelo final.

### 4.5 Extensión 2 — Modelo de volatilidad GARCH(1,1)

La serie de tasas de crecimiento mensual presenta *clustering* de volatilidad (períodos de alta y baja variabilidad), por lo que se modeló la **varianza condicional** con un GARCH(1,1) sobre la distribución t de Student:

![Volatilidad condicional GARCH(1,1)](informe_assets/63_garch_volatilidad.png)

| Parámetro | Coeficiente | p-valor | Interpretación |
|---|---|---|---|
| ω (omega) | 48,053 | 0,010 | Significativo |
| α₁ (ARCH) | 0,073 | 0,473 | No significativo |
| β₁ (GARCH) | 0,436 | 0,030 | Significativo — persistencia de volatilidad |
| ν (grados de libertad t) | 88,92 | 0,781 | No significativo |

**Conclusión:** existe **persistencia de la volatilidad** (β ≈ 0,44, significativo), pero el efecto de los shocks recientes es débil (α ≈ 0,07, no significativo). El GARCH aporta intervalos de incertidumbre más realistas sin alterar el pronóstico puntual de SARIMA.

### 4.6 Pronóstico operativo a futuro (2024)

Se reentrenó el modelo SARIMA ganador sobre **toda la serie disponible (2015-2023)** y se proyectaron los **12 meses de 2024**, con su intervalo de confianza del 95 %:

![Pronóstico de generación eléctrica 2024](informe_assets/65_pronostico_2024.png)

| Mes 2024 | Pronóstico (GWh) | IC inferior 95% | IC superior 95% |
|---|---|---|---|
| Enero | 35.332,0 | 31.161,3 | 39.502,7 |
| Febrero | 29.569,7 | 24.766,7 | 34.372,7 |
| Marzo | 33.996,8 | 28.936,8 | 39.056,9 |
| Abril | 24.556,7 | 19.346,0 | 29.767,4 |
| Mayo | 28.336,8 | 23.013,4 | 33.660,2 |
| Junio | 32.932,5 | 27.511,9 | 38.353,1 |
| Julio | 32.692,5 | 27.181,9 | 38.203,0 |
| Agosto | 30.219,4 | 24.622,6 | 35.816,2 |
| Septiembre | 27.073,2 | 21.392,5 | 32.753,8 |
| Octubre | 26.035,5 | 20.272,6 | 31.798,4 |
| Noviembre | 28.586,3 | 22.742,5 | 34.430,2 |
| Diciembre | 30.695,5 | 24.772,0 | 36.619,1 |

El pronóstico reproduce el patrón bimodal característico verano/invierno (picos en enero y junio-julio).

---

## 5. Conclusiones

1. **Respuesta a la pregunta de investigación:** el modelo **SARIMA(1,1,1)(1,1,1,12)** resultó el más preciso (**MAPE 8,15 %**), confirmando la hipótesis central del trabajo: **la estacionalidad anual es el factor determinante** del pronóstico de generación eléctrica argentina. Incorporar el componente estacional redujo el error de un 13,01 % (ARIMA, sin estacionalidad) a un 8,15 % (SARIMA).

2. **Ranking de modelos:** los **Holt-Winters** se ubicaron en un nivel intermedio (~9 %), por encima de SARIMA pero muy por delante del ARIMA simple, que —al no modelar el ciclo de 12 meses— genera un pronóstico casi plano que ignora la naturaleza bimodal de la demanda argentina. **Prophet** (MAPE 11,61 %) también superó a ARIMA al capturar la estacionalidad anual mediante términos de Fourier, pero no alcanzó a SARIMA ni a Holt-Winters; esto es esperable dado el tamaño reducido de la muestra (108 observaciones) y la ausencia de variables como temperatura o feriados que potencien a Prophet.

3. **Aditivo vs. multiplicativo:** la variante **multiplicativa** de Holt-Winters superó levemente a la aditiva (9,02 % vs. 9,32 %), consistente con una dependencia muy moderada entre el nivel de la serie y la amplitud de su estacionalidad (en la descomposición, sin embargo, el esquema aditivo había sido marginalmente mejor — la diferencia entre ambos enfoques es pequeña).

4. **Limitación principal:** la fuerte contracción de 2022-2023 introduce un quiebre de nivel que ningún modelo lineal anticipa por completo; esto explica buena parte del error residual sobre el conjunto de prueba (2023) y sugiere, como trabajo futuro, incorporar **variables exógenas** (actividad económica, temperatura) vía SARIMAX.

5. **Selección automática vs. manual:** la búsqueda por AIC seleccionó `SARIMA(1,1,1)(0,1,1,12)` (AIC 1244,08), levemente por debajo del modelo manual (AIC 1245,97). Sin embargo, en el conjunto de prueba el modelo manual predijo mejor (MAPE 8,15 % vs. 9,47 %): un buen recordatorio de que el menor AIC (mejor ajuste *in-sample*) no garantiza mejor capacidad predictiva *out-of-sample*.

6. **Volatilidad (GARCH):** se detectó persistencia de la volatilidad (β ≈ 0,44, significativo) pero un efecto débil de shocks recientes (α ≈ 0,07, no significativo), lo que permite construir intervalos de incertidumbre más realistas sin alterar el pronóstico puntual de SARIMA.

7. **Lecciones aprendidas / dificultades:**
   - El dataset de Kaggle es **multi-país y multi-producto**, por lo que fue necesario un filtrado cuidadoso (`Country == "Argentina"` y `parameter == "Net Electricity Production"`) para evitar duplicar valores y obtener una serie mensual univariada y continua.
   - La elección entre descomposición **aditiva y multiplicativa** no era evidente a priori; se resolvió comparando cuantitativamente la dispersión de los residuos de ambas.
   - Los órdenes de diferenciación de SARIMA no se fijaron "a ojo": se justificaron formalmente con pruebas **ADF/KPSS**.
   - El **menor AIC no implica mejor pronóstico out-of-sample**, lo que refuerza la importancia de validar siempre sobre el conjunto de prueba y no solo con criterios de ajuste in-sample.
   - La fuerte caída de la generación en 2022-2023 es la principal fuente de error de todos los modelos sobre el conjunto de prueba, y es un fenómeno que ningún modelo univariado puede anticipar sin información exógena.
