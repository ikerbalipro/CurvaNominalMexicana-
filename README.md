# La curva nominal mexicana

Análisis y pronóstico de la curva de rendimientos nominal de México, a partir de las tasas mensuales de subasta primaria de CETES (28, 91, 182 y 364 días) y MBONOS (3, 5, 10, 20 y 30 años), de octubre de 2006 a septiembre de 2026.

**Autor:** Iker Bali Elizalde · octubre de 2026

## Preguntas

1. ¿Hay estructura en la forma en que se mueven juntos los nueve plazos?
2. ¿Se puede pronosticar la curva a uno y a cuatro trimestres (3 y 12 meses)?
3. ¿Esa estructura mejora los pronósticos?

## Resultados principales

- **Hay estructura.** Las nueve tasas tienen correlaciones de 0.73 o más, y tres componentes principales explican el 99.5% de la varianza (94.3%, 4.6% y 0.7%). Representan el **nivel** (correlación de 1.00 con el promedio de las tasas), la **pendiente** (0.55 con 30 años − 28 días) y la **curvatura** (0.74).
- **Sí se puede pronosticar.** El mejor modelo, Holt con tendencia amortiguada, reduce el RMSE frente a la caminata aleatoria en 13% a 3 meses (Diebold-Mariano, p = 0.006) y en 10% a 12 meses (p = 0.097). La mejora se concentra en los CETES; en los MBONOS ningún modelo le gana al baseline.
- **La estructura no mejora el pronóstico.** Pronosticar los 3 componentes y reconstruir la curva da un error estadísticamente igual a pronosticar las 9 series por separado (p entre 0.24 y 0.98).

## Contenido

| Archivo | Descripción |
|---|---|
| `curva_nominal.ipynb` | Notebook completo, con una sección por paso del análisis |
| `requirements.txt` | Dependencias de Python |

Estructura del notebook:

1. Conversión del formato largo al formato ancho (una fila por mes, una columna por plazo)
2. Análisis exploratorio: series de tiempo, valores faltantes y matriz de correlación
3. Imputación: comparación de MICE, KNN e interpolación lineal
4. Análisis de componentes principales y su interpretación (nivel, pendiente y curvatura)
5. Enfoque individual (9 series) frente a enfoque colectivo (3 componentes)
6. Modelos: Holt amortiguado, ARIMA y Ridge directo; baseline de caminata aleatoria y VAR como referencia
7. Backtest con ventana expandible desde septiembre de 2014 (RMSE, MAE y prueba Diebold-Mariano)
8. Resultados por plazo y horizonte

## Datos

Los datos **no se incluyen** en este repositorio. Son las tasas promedio mensuales anualizadas de las subastas primarias de valores gubernamentales que publica el Banco de México en su [Sistema de Información Económica (SIE)](https://www.banxico.org.mx/SieInternet/).

Para correr el notebook, guarda en la misma carpeta un archivo `banxico_auction_rates_monthly.csv` en formato largo con estas columnas:

```
DATE,INSTRUMENT,INDICATOR,VALUE_PCT
2006-10-01,"CETEs, 28 days",Annualized monthly average rate,7.05
2006-10-01,"CETEs, 182 days",Annualized monthly average rate,7.42
```

Los valores de `INSTRUMENT` esperados son: `CETEs, 28 days`, `CETEs, 91 days`, `CETEs, 182 days`, `CETEs, 364 days`, `MBONOs, 3 years`, `MBONOs, 5 years`, `MBONOs, 10 years`, `MBONOs, 20 years` y `MBONOs, 30 years`.

## Cómo correrlo

```bash
python -m venv .venv
source .venv/bin/activate        # En Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook curva_nominal.ipynb
```

El backtest se paraleliza con `joblib` y el notebook completo tarda alrededor de 8 minutos.

## Limitaciones y siguientes pasos

- Las tasas son de subastas mensuales, no cotizaciones simultáneas de mercado, y los plazos largos tienen hasta 30% de meses sin dato.
- El problema se trató como univariado: faltan variables exógenas (tasa objetivo de Banxico, inflación, tasas de EE. UU.), ingeniería de características y probar modelos de deep learning.
- Falta un análisis más económico: entender la curva desde el punto de vista financiero y no solo matemático.
