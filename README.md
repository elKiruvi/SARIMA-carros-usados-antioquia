# Modelo SARIMA para la Predicción de Búsqueda de Carros Usados en Antioquia

## Descripción del Proyecto
Este estudio analiza el interés del consumidor en el mercado de vehículos de segunda mano en el departamento de Antioquia (Colombia). Utilizando datos mensuales extraídos de Google Trends desde enero de 2006 como indicador adelantado de la demanda regional, se desarrolló un modelo econométrico SARIMA. El objetivo principal es comprender los patrones temporales subyacentes, como tendencias y estacionalidades, y pronosticar futuras fluctuaciones en el interés de búsqueda. 

El modelo busca transformar la huella digital del público en inteligencia de mercado accionable, permitiendo a los actores del sector automotriz anticipar ciclos de demanda, planificar campañas de marketing y optimizar la gestión de inventario.

## Datos
Los datos utilizados corresponden a una serie de tiempo univariada con frecuencia mensual.
* **Periodo:** Enero de 2006 hasta Octubre de 2025 (incluyendo periodo de pronóstico).
* **Variables:**
  * `Fecha`: Índice temporal (formato datetime).
  * `Busquedas`: Conteo discreto del volumen de búsquedas (enteros).
* **Estadísticas Descriptivas:** La serie presenta una media de 46.39 y una mediana de 48.50, indicando una distribución relativamente simétrica. Sin embargo, cuenta con una desviación estándar de 17.88, lo que denota una volatilidad significativa a lo largo del tiempo.

## Metodología

### 1. Análisis Exploratorio y Estacionariedad
* **Tendencia y Varianza:** La serie original resultó no estacionaria en media y varianza. Presentó un crecimiento hasta 2010, estabilidad hasta 2017, y una tendencia decreciente acentuada desde 2021.
* **Prueba de Dickey-Fuller Aumentada (ADF):** El test estadístico confirmó la presencia de una raíz unitaria (p-valor = 0.83).
* **Diferenciación:** Se aplicó una primera diferencia regular (d=1) que logró estacionar la serie en media (p-valor = 0.0009), y se analizó una diferencia estacional (d=12) para capturar la periodicidad anual.

### 2. Modelado
El análisis de los correlogramas (ACF y PACF) evidenció picos significativos en los rezagos estacionales y regulares. Con base en esta información, se entrenó un modelo mediante la librería `statsmodels`:
* **Especificación:** SARIMA(1, 1, 1) x (1, 1, 1, 12)
* **Datos de prueba:** Se reservó el último 12% de la serie para evaluar la capacidad del modelo de extrapolar la tendencia decreciente reciente.

## Resultados y Desempeño
El modelo capturó exitosamente tanto la tendencia histórica como los patrones estacionales recurrentes. 

* **Métricas de Evaluación:**
  * **RMSE (Raíz del Error Cuadrático Medio):** 3.50
  * **MAE (Error Absoluto Medio):** 2.80
* **Análisis de Negocio:** Las métricas indican una desviación mínima respecto a los valores reales. El modelo pronostica una continuación en la tendencia decreciente de búsquedas directas en Google para los próximos 3 años, lo cual se atribuye a la migración de los consumidores hacia plataformas especializadas (Marketplace, Mercado Libre, TucarroYa). 
* **Estacionalidad:** Se confirmaron picos de interés a principios de año (motivados por cambio de modelo y promociones de concesionarios) y contracciones entre marzo-abril y agosto-septiembre.

## Limitaciones y Trabajo Futuro
A pesar del buen desempeño métrico, el análisis identificó áreas de mejora técnica:
1. **Heterocedasticidad:** El modelo SARIMA asume varianza constante. Se propone aplicar transformaciones Box-Cox o logarítmicas en futuras iteraciones para estabilizar la varianza.
2. **Quiebres Estructurales:** Shocks externos, como la pandemia de COVID-19 en 2020, afectaron la serie. Se sugiere el uso de variables de intervención (dummies) para aislar estos efectos.
3. **Modelos Multivariados:** Evolucionar hacia un modelo SARIMAX para incluir variables exógenas macroeconómicas (precio de la gasolina, TRM, indicadores económicos regionales).
4. **Algoritmos Alternativos:** Comparar los resultados actuales con modelos robustos ante múltiples estacionalidades como Prophet, o redes neuronales recurrentes (LSTM) para capturar no linealidades complejas.

## Requisitos y Tecnologías
El proyecto fue desarrollado en Python. Las principales librerías utilizadas son:
* `pandas` y `numpy` para la manipulación de datos.
* `matplotlib` y `seaborn` para el análisis visual.
* `statsmodels` para pruebas de hipótesis (ADF), correlogramas y modelado SARIMA.
* `scikit-learn` para el cálculo de métricas de evaluación (RMSE, MAE).

## Instrucciones de Ejecución
1. Clonar el repositorio.
2. Instalar las dependencias listadas (se recomienda el uso de un entorno virtual).
3. Ejecutar el notebook proporcionado para reproducir el análisis exploratorio, el entrenamiento del modelo y los pronósticos a 36 meses.
