# Proyecto Final (Python + Power BI): 75 Temporadas de Fórmula 1 (1950–2024)

**Autor:** Pedro  
**Fecha:** 14/08/2025

## 1. Objetivo
Analizar 75 temporadas de Fórmula 1 (1950–2024) y construir un dashboard operativo en Power BI, partiendo de un EDA riguroso en Python.

## 2. Estructura del repositorio
```
data/
  raw/
  processed/
notebooks/
  F1_EDA_v2.ipynb
src/
  io.py
  clean.py
  features.py
reports/
  figures/
bi/
  F1WC_1950-2024_Dashboard_v4.pbix
```

## 3. Entregables
- **EDA en Python:** `notebooks/F1_EDA_v2.ipynb`  
- **Dashboard Power BI:** `bi/F1WC_1950-2024_Dashboard_v4.pbix`  
- **Informe** (este README resume pasos y hallazgos).

## 4. Requisitos
- Python 3.10+
- Librerías: `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`, `statsmodels` (opcional), `pyarrow` (opcional)
- Power BI Desktop (agosto 2024 o superior recomendado)

## 5. Pasos para reproducir
1. Crear entorno:
   ```bash
   python -m venv .venv
   source .venv/bin/activate  # Windows: .venv\Scripts\activate
   pip install -U pandas numpy matplotlib seaborn scipy statsmodels pyarrow
   ```
2. Preparar datos: copiar fuentes a `data/raw/` y ejecutar el cuaderno `notebooks/F1_EDA_v2.ipynb` hasta generar intermedios en `data/processed/`.
3. Abrir `bi/F1WC_1950-2024_Dashboard_v4.pbix` y actualizar orígenes a `data/processed/` si es necesario.
4. Validar el modelo (relaciones y cardinalidades) y revisar medidas DAX clave (KPIs, ordenación por Mes Nº, etc.).

## 6. Metodología
- **Limpieza y transformación:** tipificación, eliminación de duplicados, imputación, normalización de claves, derivados (era, posiciones ganadas, %DNF).

- **EDA:** distribuciones, tendencias, correlaciones; visualizaciones con `matplotlib`/`seaborn`.

- **Análisis estadístico:** correlaciones, ANOVA/Kruskal, regresiones simples/múltiples donde aplique.

- **BI:** modelo en estrella, medidas DAX, segmentaciones sincronizadas, bookmarks y KPIs.

## 7. Medidas DAX (ejemplos)
```DAX
Total Carreras = DISTINCTCOUNT( races[raceId] )

Pilotos Únicos = DISTINCTCOUNT( drivers[driverId] )

Constructores Únicos = DISTINCTCOUNT( constructors[constructorId] )

Puntos Totales = SUM( results[points] )

Podios = CALCULATE( COUNTROWS(results), results[position] <= 3 )

DNF % = DIVIDE(
    CALCULATE(COUNTROWS(results), status[status] <> "Finished"),
    COUNTROWS(results)
)

Posiciones Ganadas = SUMX( results, results[grid] - results[position] )

Media Pit Stop (s) = AVERAGE( pit_stops[duration_s] )

Mes = FORMAT( Calendar[Date], "mmmm" )
Mes Nº = MONTH( Calendar[Date] )  -- usar para ordenar 'Mes'
```

## 8. Buenas prácticas adoptadas
- Desactivar **Auto Date/Time** en Power BI.
- Usar claves enteras y **relaciones 1:\*** explícitas.
- Columnas de orden para texto temporal (Mes vs Mes Nº).
- Evitar columnas calculadas pesadas; preferir **medidas**.
- Validaciones de calidad de datos automatizadas en Python.

## 9. Resultados (resumen ilustrativo)
- Incremento del número de carreras por temporada en décadas recientes.

- Correlación negativa moderada entre posición de salida y final.

- Efecto de paradas en boxes dependiente de era/circuito.

- Concentración de victorias por eras según cambios reglamentarios.

## 10. Trabajo futuro
- Integrar telemetría y meteorología.

- Modelos predictivos de probabilidad de podio o puntos.

- Automatizar pipelines con orquestación (p.ej., GitHub Actions).

## 11. Licencia
Uso académico y divulgativo.
