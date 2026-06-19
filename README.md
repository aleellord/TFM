# OPTIMIZACIÓN DE CARTERAS BASADA EN LA PREDICCIÓN DE RETORNOS MEDIANTE MODELOS DE SERIES TEMPORALES Y APRENDIZAJE AUTOMÁTICO - Alejandra Llord

El objetivo es estudiar y comparar modelos de series temporales y de aprendizaje automático para predecir los retornos diarios y semanales de un conjunto de fondos cotizados (ETFs). Posteriormente, estas predicciones se utilizan para construir una cartera optimizada según el modelo de Markowitz.

---

## Estructura de carpetas

```
TFM/
├── 1.Preprocesado/                     # Descarga y preparación de datos
├── 2.Modelo_Series_Temporales/         # Modelos ARIMA
├── 3.Modelo_ML/                        # Modelos de machine learning
├── 4.Markowitz/                        # Backtest de la cartera
└── Datos_csv/                          # Archivos CSV generados
```

---

## 1. Preprocesado

| Notebook | Qué hace |
|----------|----------|
| `01_Extraccion_datos_iniciales.ipynb` | Descarga precios ajustados y volumen de Yahoo Finance para 63 ETFs |
| `02_analisis_inicial.ipynb` | Análisis exploratorio: distribuciones, correlaciones y filtro por liquidez |
| `03_Creación_df_diarios_ML.ipynb` | Construye el dataset diario con retornos, volumen y variables de calendario |
| `04_Creación_df_semanales_ML.ipynb` | Construye el dataset semanal con versiones de 0 a 4 lags de retornos pasados |

---

## 2. Modelos de series temporales

| Notebook | Qué hace |
|----------|----------|
| `00_preprocesado_estacionalidad.ipynb` | Tests ADF y KPSS para comprobar estacionariedad |
| `00_Estudio_parametros_ARIMA.ipynb` | Explora parámetros (p, d, q) con gráficos y métricas: AIC, BIC y test de Ljung-Box en un ETF representativo |
| `Modelo_diario/01_datos_series_temporales_grupal_diario.ipynb` | ARIMA walk-forward en todos los ETFs a escala diaria |
| `Modelo_semanal/01_datos_series_temporales_grupal_semanal.ipynb` | ARIMA walk-forward a escala semanal |

---

## 3. Modelos de machine learning

| Notebook | Qué hace |
|----------|----------|
| `Modelos_diarios/02_Comparacion_modelos_diarios.ipynb` | Entrena Ridge, RF, GBR y LR en walk-forward diario; guarda resultados en Google Sheets |
| `Modelos_diarios/03_Análisis_resultados_modelos_diarios.ipynb` | Lee Sheets y analiza resultados diarios con tablas y gráficos |
| `Modelos_semanales/02_Comparacion_modelos_semanal.ipynb` | Entrena Ridge, RF, GBR y LR en walk-forward semanal ; guarda resultados en Google Sheets|
| `Modelos_semanales/03_Análisis_resultados_modelos_semanales.ipynb` | Análisis de resultados semanales |
| `Modelos_semanales/04_Predicciones_modelo_Ridge_0lags.ipynb` | Genera predicciones individuales por semana con el modelo ganador |

---

## 4. Backtest Markowitz

| Notebook | Qué hace |
|----------|----------|
| `01_backtest_markowitz_ARIMA.ipynb` | Cartera Markowitz semanal con predicciones ARIMA |


---

## Datos

- **Fuente:** Yahoo Finance (enero 2021 – febrero 2026)
- **ETFs:** 63 iniciales → 58 tras el filtro de estacionariedad
- **Grupos de activos:** mercado estadounidense, factores y estilos, sectores económicos, mercados internacionales, renta fija, materias primas

---

## Métricas de evaluación

- **RMSE** (error cuadrático medio): penaliza los errores grandes
- **MAE** (error absoluto medio): mide el error promedio sin penalizar más los grandes
- **Baseline:** predice el retorno del periodo anterior
- **Zeros:** predice siempre cero (referencia exigente porque los retornos oscilan en torno a cero)
- **Kappa de Cohen:** mide si el modelo acierta la dirección del mercado (sube/baja)


---

## Orden de ejecución recomendado

```
1.Preprocesado/
  → 01_Extraccion_datos_iniciales.ipynb
  → 02_analisis_inicial.ipynb
  → 03_Creación_df_diarios_ML.ipynb
  → 04_Creación_df_semanales_ML.ipynb

2.Modelo_Series_Temporales/
  → 00_preprocesado_estacionalidad.ipynb
  → 00_Estudio_parametros_ARIMA.ipynb
  → Modelo_diario/01_datos_series_temporales_grupal_diario.ipynb
  → Modelo_semanal/01_datos_series_temporales_grupal_semanal_*.ipynb

3.Modelo_ML/
  → Modelos_*/02_Comparacion_*.ipynb
  → Modelos_*/03_Análisis_*.ipynb
  → Modelos_semanales/04_Predicciones_*.ipynb

4.Markowitz/
  → 01_backtest_markowitz_*.ipynb
  → 02_backtest_markowitz_*.ipynb
```

---

## Requisitos

```
pandas, numpy, matplotlib, seaborn
statsmodels          # ARIMA, tests ADF y KPSS
scikit-learn         # modelos ML, TimeSeriesSplit, RandomizedSearchCV
PyPortfolioOpt       # optimización Markowitz
cvxpy                # solver de optimización cuadrática
gspread, google-auth # integración con Google Sheets
yfinance             # descarga de datos desde Yahoo Finance
```
