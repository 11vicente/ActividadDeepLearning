# Prediccion de Consumo Electrico con Deep Learning (LSTM)

Proyecto academico de series temporales en Python para analizar y predecir el consumo electrico usando el dataset **UCI Electricity Load Diagrams 2011-2014** y un modelo de red neuronal **LSTM**.

## Objetivo

Desarrollar un flujo completo de trabajo de Machine Learning y Deep Learning para:

- Explorar patrones temporales del consumo electrico.
- Preprocesar datos de series temporales (limpieza, remuestreo, normalizacion).
- Entrenar un modelo LSTM para prediccion.
- Evaluar el rendimiento con metricas de error.
- Realizar prediccion futura a multiples pasos.

## Dataset

- **Nombre**: Electricity Load Diagrams 2011-2014 (UCI)
- **Fuente**: https://archive.ics.uci.edu/ml/machine-learning-databases/00321/LD2011_2014.txt.zip
- **Frecuencia original**: cada 15 minutos
- **Contenido**: consumo electrico para multiples clientes

## Estructura del proyecto

- `prediccion_consumo_lstm.ipynb`: notebook principal con todo el pipeline academico.

## Contenido del notebook

El cuaderno incluye las siguientes secciones:

1. Introduccion a series temporales, prediccion electrica, RNN y LSTM.
2. Importacion de librerias.
3. Carga y descripcion del dataset.
4. EDA (visualizacion de series, patrones diarios/semanales, estadisticas).
5. Preprocesamiento de datos.
6. Creacion de ventanas temporales (lookback).
7. Division temporal en entrenamiento y prueba.
8. Construccion y compilacion del modelo LSTM.
9. Entrenamiento con EarlyStopping.
10. Evaluacion con RMSE y MAE.
11. Predicciones y comparacion real vs predicho.
12. Prediccion futura multi-step.
13. Conclusiones y mejoras propuestas.

## Requisitos

Se recomienda Python 3.10-3.12 para compatibilidad completa con TensorFlow.

Dependencias principales:

- numpy
- pandas
- matplotlib
- seaborn
- scikit-learn
- statsmodels
- tensorflow (o keras + torch como alternativa)

## Instalacion rapida

```bash
pip install numpy pandas matplotlib seaborn scikit-learn statsmodels tensorflow
```

Si TensorFlow no esta disponible en tu version de Python, usa:

```bash
pip install numpy pandas matplotlib seaborn scikit-learn statsmodels keras torch
```

## Ejecucion

1. Abre `prediccion_consumo_lstm.ipynb` en Jupyter Notebook o Google Colab.
2. Ejecuta las celdas en orden desde la primera hasta la ultima.
3. Verifica metricas RMSE y MAE, y revisa las graficas de prediccion.

## Notas de compatibilidad

- En versiones recientes de pandas, la frecuencia horaria debe escribirse como `h` (minuscula), no `H`.
- El notebook ya fue ajustado para esta compatibilidad.

## Resultados esperados

- Curvas de entrenamiento y validacion.
- Comparacion visual entre consumo real y predicho.
- Pronostico de consumo futuro para los siguientes pasos temporales.

## Mejoras futuras

- Incluir variables exogenas (clima, festivos, calendario).
- Ajustar hiperparametros (lookback, capas, neuronas, batch size).
- Probar modelos hibridos CNN + LSTM.
- Comparar con GRU y arquitecturas basadas en Transformers.

## Autor

Proyecto desarrollado para actividad academica de Deep Learning.
