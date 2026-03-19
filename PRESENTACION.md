# Presentación: Predicción de Consumo Eléctrico con Deep Learning (LSTM)

> **Duración estimada:** ~5 minutos  
> **Formato:** Lista de diapositivas lista para copiar a PowerPoint, Canva o Google Slides  
> **Audiencia:** Exposición académica (profesor / tribunal)

---

## Diapositiva 1: Portada

**Título:**  
Predicción de Consumo Eléctrico con Redes Neuronales LSTM

**Subtítulo:**  
Proyecto de Deep Learning — Series Temporales

**Contenido visual sugerido:**  
Logo de la institución, imagen de una red neuronal o gráfico de consumo eléctrico, nombres de los autores y fecha.

**🎤 Notas del expositor:**  
> "Buenas tardes. En esta presentación les voy a explicar un proyecto de Deep Learning en el que construí un modelo de redes neuronales LSTM para predecir el consumo eléctrico de clientes industriales. El objetivo es demostrar cómo la inteligencia artificial puede anticipar la demanda de energía a partir de datos históricos."

---

## Diapositiva 2: Problema y Contexto

**Título:**  
¿Cuál es el problema?

**Contenido (bullets):**
- La demanda eléctrica varía de forma compleja a lo largo del tiempo.
- Predecir el consumo permite mejorar la gestión de la red y reducir costos.
- Los métodos estadísticos clásicos tienen dificultades con patrones no lineales.
- **Solución propuesta:** usar una red neuronal LSTM, diseñada para aprender dependencias temporales largas.

**Contenido visual sugerido:**  
Gráfico de una serie temporal de consumo eléctrico con picos y valles.

**🎤 Notas del expositor:**  
> "El problema central es la predicción de consumo eléctrico. La demanda tiene patrones que dependen de la hora del día, el día de la semana y la temporada del año. Los métodos clásicos como ARIMA no capturan bien estas relaciones no lineales, por eso recurrimos a Deep Learning."

---

## Diapositiva 3: Dataset

**Título:**  
El Dataset — UCI Electricity Load Diagrams 2011-2014

**Contenido (bullets):**
- **Fuente:** UCI Machine Learning Repository
- **Período:** 2011–2014 (4 años de datos)
- **Frecuencia:** Mediciones cada **15 minutos**
- **Cobertura:** Múltiples clientes industriales y residenciales (370 series)
- **Tamaño:** ~140.000 filas × 370 columnas
- Se seleccionó el cliente con mayor consumo promedio para el modelado

**Contenido visual sugerido:**  
Tabla con las primeras filas del dataset; esquema de fechas y clientes.

**🎤 Notas del expositor:**  
> "El dataset proviene del repositorio UCI y contiene mediciones de consumo eléctrico cada 15 minutos para 370 clientes a lo largo de 4 años. Para simplificar el modelado, seleccioné al cliente con el mayor consumo promedio como serie objetivo."

---

## Diapositiva 4: Análisis Exploratorio (EDA)

**Título:**  
Explorando los Datos

**Contenido (bullets):**
- Visualización del consumo agregado de todos los clientes (serie total)
- Inspección de series individuales: diferentes magnitudes y patrones
- Reestructuración temporal: de 15 minutos → **promedio horario** para reducir ruido
- Identificación de valores faltantes (corregidos con `ffill/bfill`)
- Verificación de estacionalidad y tendencia

**Contenido visual sugerido:**  
- Gráfico 1: Consumo eléctrico agregado 2011-2014  
- Gráfico 2: Tres series individuales superpuestas  
- Gráfico 3: Serie horaria del cliente seleccionado

**🎤 Notas del expositor:**  
> "En el análisis exploratorio visualicé la serie completa y series individuales para entender la distribución del consumo. Luego resamplé a frecuencia horaria para trabajar con datos más suaves y sin tanto ruido de la granularidad de 15 minutos."

---

## Diapositiva 5: Preprocesamiento

**Título:**  
Preparando los Datos para el Modelo

**Contenido (bullets):**
- **Imputación:** valores faltantes con propagación hacia adelante y hacia atrás
- **Normalización:** MinMaxScaler → escala [0, 1] (necesaria para redes neuronales)
- **Ventanas temporales (lookback = 168 h):**  
  - La red recibe las **últimas 168 horas** (7 días) como contexto  
  - Predice la **hora siguiente**
- **Resultado:** matrices X (secuencias) e y (etiquetas)

**Contenido visual sugerido:**  
Diagrama de ventana deslizante mostrando cómo se construyen X e y.

```
|--- 168 horas de contexto ---|  →  hora 169 (predicción)
[h1, h2, ..., h168]             →  y
      [h2, h3, ..., h169]       →  y
```

**🎤 Notas del expositor:**  
> "Antes de entrenar el modelo, normalicé la serie entre 0 y 1 para que la red aprenda de forma estable. Luego construí ventanas deslizantes de 168 horas —equivalente a una semana— como entrada. Cada ventana produce una predicción para la hora siguiente."

---

## Diapositiva 6: División de Datos

**Título:**  
Train / Test Split

**Contenido (bullets):**
- **80 %** de los datos → entrenamiento
- **20 %** → evaluación (test)
- La división es **cronológica** (no aleatoria) para respetar el orden temporal
- Tensor de entrada: `(muestras, 168, 1)` — una característica por paso de tiempo

**Contenido visual sugerido:**  
Barra horizontal dividida 80 % azul / 20 % naranja con fechas aproximadas.

**🎤 Notas del expositor:**  
> "Es importante que la división sea cronológica y no aleatoria, porque mezclar fechas futuras con pasadas en el entrenamiento generaría fuga de información y métricas artificialmente buenas."

---

## Diapositiva 7: ¿Qué es una LSTM?

**Título:**  
LSTM — La Memoria de la Red Neuronal

**Contenido (bullets):**
- **LSTM** = Long Short-Term Memory (Memoria a Corto y Largo Plazo)
- Es un tipo especial de red neuronal recurrente (RNN)
- Incorpora **compuertas** que controlan qué información recordar y qué olvidar:
  - 🔒 Compuerta de olvido (*forget gate*)
  - 📥 Compuerta de entrada (*input gate*)
  - 📤 Compuerta de salida (*output gate*)
- Ideal para series temporales con dependencias largas (ej.: patrones semanales)

**Contenido visual sugerido:**  
Diagrama simplificado de una celda LSTM con sus tres compuertas etiquetadas.

**🎤 Notas del expositor:**  
> "Una LSTM es una red neuronal con memoria. A diferencia de una red densa común, puede 'recordar' lo que ocurrió hace muchos pasos de tiempo y 'olvidar' lo que no es relevante. Esto la hace perfecta para el consumo eléctrico, que tiene patrones diarios y semanales."

---

## Diapositiva 8: Arquitectura del Modelo

**Título:**  
Arquitectura LSTM Utilizada

**Contenido (bullets):**

| Capa | Detalle |
|------|---------|
| LSTM (64 unidades) | Retorna secuencias completas |
| Dropout 20 % | Regularización — evita sobreajuste |
| LSTM (32 unidades) | Retorna solo el último estado |
| Dropout 20 % | Regularización |
| Dense (16, ReLU) | Capa intermedia |
| Dense (1) | Salida: consumo de la próxima hora |

- **Optimizador:** Adam  
- **Función de pérdida:** MSE (Error Cuadrático Medio)

**Contenido visual sugerido:**  
Diagrama vertical de capas (tipo Keras summary).

**🎤 Notas del expositor:**  
> "La arquitectura tiene dos capas LSTM apiladas, con Dropout entre ellas para evitar sobreajuste. Las capas densas al final conectan la representación aprendida con la predicción numérica."

---

## Diapositiva 9: Entrenamiento

**Título:**  
Entrenando el Modelo

**Contenido (bullets):**
- Hasta **30 épocas** de entrenamiento
- **Batch size:** 64 muestras por actualización
- **EarlyStopping:** detiene el entrenamiento si la pérdida de validación no mejora en 8 épocas consecutivas → evita sobreajuste y ahorra tiempo
- Se restauran los **mejores pesos** automáticamente

**Contenido visual sugerido:**  
Gráfico de curvas de pérdida (Train Loss vs. Validation Loss por época) con línea vertical en la época de parada anticipada.

**🎤 Notas del expositor:**  
> "Usé EarlyStopping para detener el entrenamiento automáticamente cuando el modelo deja de mejorar. Esto evita que sobreajuste al conjunto de entrenamiento y garantiza que guardemos los mejores pesos encontrados."

---

## Diapositiva 10: Evaluación del Modelo

**Título:**  
Métricas de Desempeño

**Contenido (bullets):**
- **RMSE** (Root Mean Squared Error): penaliza errores grandes — muy sensible a picos
- **MAE** (Mean Absolute Error): error promedio absoluto — más interpretable

> Las predicciones se **desnormalizan** antes de calcular las métricas (escala original de kWh)

**Contenido visual sugerido:**  
Tabla con valores de RMSE y MAE; gráfico de barras comparando ambas métricas.

**🎤 Notas del expositor:**  
> "Después de desnormalizar las predicciones, calculé RMSE y MAE en las unidades originales del dataset. El MAE nos dice cuánto se equivoca el modelo en promedio, mientras el RMSE castiga más los errores grandes."

---

## Diapositiva 11: Resultados — Real vs. Predicho

**Título:**  
¿Qué tan bien predice el modelo?

**Contenido (bullets):**
- El modelo sigue fielmente la tendencia general del consumo
- Captura correctamente los **picos diarios** y los **valles nocturnos**
- Pequeñas desviaciones en transiciones abruptas (esperado en series reales)
- El gráfico muestra los últimos meses de datos (conjunto de test)

**Contenido visual sugerido:**  
Gráfico de líneas: consumo **Real** (azul) vs. **Predicho** (naranja) sobre el período de test — tomar directamente del notebook.

**🎤 Notas del expositor:**  
> "Este gráfico es el resultado más importante: la curva naranja —predicción— sigue de cerca a la curva azul —valores reales—. El modelo aprendió correctamente los patrones de consumo diario y semanal."

---

## Diapositiva 12: Predicción Futura (Multi-step)

**Título:**  
Mirando al Futuro — Predicción Multi-paso

**Contenido (bullets):**
- El modelo genera **72 pasos futuros** (~3 días hacia adelante)
- Estrategia **auto-regresiva:** cada predicción se convierte en entrada para la siguiente
- La incertidumbre aumenta con el horizonte de predicción (efecto de acumulación de error)

**Contenido visual sugerido:**  
Gráfico con la serie histórica reciente seguida de los 72 pasos predichos marcados en color diferente y/o zona sombreada de incertidumbre.

**🎤 Notas del expositor:**  
> "Además de evaluar en datos pasados, usé el modelo para proyectar el consumo 72 horas hacia el futuro. Para eso, cada predicción se retroalimenta como entrada para el siguiente paso. A mayor horizonte, mayor acumulación del error."

---

## Diapositiva 13: Conclusiones

**Título:**  
Conclusiones

**Contenido (bullets):**
- ✅ Las redes LSTM son **efectivas para series temporales** con patrones complejos
- ✅ El preprocesamiento (normalización + ventanas) es **crítico** para el éxito del modelo
- ✅ EarlyStopping mejora la **generalización** sin esfuerzo manual
- ✅ RMSE y MAE confirman que el modelo aprende patrones reales, no ruido
- 📚 **Aprendizajes clave del proyecto:**
  - Manejo de datasets de series temporales grandes
  - Implementación de LSTM con Keras/TensorFlow
  - Técnicas de regularización (Dropout, EarlyStopping)
  - Evaluación correcta preservando el orden temporal
- 🔭 **Posibles mejoras futuras:**
  - Incluir variables exógenas (temperatura, festivos)
  - Explorar arquitecturas CNN-LSTM o Transformer
  - Aplicar predicción multi-variate (varios clientes simultáneos)

**🎤 Notas del expositor:**  
> "En resumen, el proyecto demostró que una arquitectura LSTM bien configurada puede capturar los patrones de consumo eléctrico con buena precisión. Lo más importante que aprendí es que el éxito del modelo depende tanto del preprocesamiento como de la arquitectura. Como pasos futuros, sería interesante incluir variables externas como la temperatura o ampliar el modelo para predecir varios clientes al mismo tiempo."

---

## Diapositiva 14: ¡Gracias! / Preguntas

**Título:**  
¡Muchas Gracias!

**Contenido:**
- Repositorio del proyecto: [github.com/11vicente/ActividadDeepLearning](https://github.com/11vicente/ActividadDeepLearning)
- Dataset: UCI Electricity Load Diagrams 2011-2014
- Tecnologías: Python · TensorFlow/Keras · Pandas · Scikit-learn · Matplotlib

**Contenido visual sugerido:**  
Imagen de fondo de una ciudad iluminada de noche o red neuronal. QR code al repositorio.

**🎤 Notas del expositor:**  
> "Eso es todo. Quedo a disposición para cualquier pregunta sobre el modelo, los datos o los resultados."

---

## 📋 Guía de Tiempo (5 minutos)

| Diapositiva | Tema | Tiempo sugerido |
|------------|------|----------------|
| 1 | Portada | 0:15 |
| 2 | Problema y Contexto | 0:25 |
| 3 | Dataset | 0:25 |
| 4 | EDA | 0:25 |
| 5 | Preprocesamiento | 0:30 |
| 6 | Train/Test Split | 0:15 |
| 7 | ¿Qué es LSTM? | 0:30 |
| 8 | Arquitectura | 0:20 |
| 9 | Entrenamiento | 0:20 |
| 10 | Evaluación | 0:20 |
| 11 | Real vs. Predicho | 0:20 |
| 12 | Predicción Futura | 0:15 |
| 13 | Conclusiones | 0:30 |
| 14 | Cierre / Preguntas | 0:10 |
| **Total** | | **~4:50** |

> ⏱ Se dejan ~10 segundos de margen para transiciones entre diapositivas y mantener la presentación dentro del límite de 5 minutos.

---

## 🎨 Sugerencias de Diseño

- **Paleta de colores:** Azul oscuro (#1A237E) + Naranja (#FF6F00) + Blanco  
- **Tipografía:** Montserrat o Roboto (títulos en negrita, contenido en regular)  
- **Fondo:** Oscuro con texto claro para mayor impacto visual  
- **Íconos:** ⚡ electricidad, 🧠 inteligencia artificial, 📈 predicción  
- **Gráficos:** Exportar directamente desde el notebook (`plt.savefig(...)`)  
- **Máximo 5 bullets por diapositiva** — no sobrecargar con texto  
