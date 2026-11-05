# Predicción de fuga de clientes — Interconnect

Proyecto final del bootcamp de Ciencia de Datos de TripleTen.

## Problema de negocio

Interconnect, un operador de telecomunicaciones, quiere anticipar qué clientes van a cancelar su contrato para ofrecerles promociones y planes especiales antes de que se vayan. El objetivo es construir un modelo de clasificación con un **ROC-AUC mínimo de 0,75**.

## Datos

Cuatro archivos con información de 7.043 clientes:

- **contract**: tipo de contrato, método de pago, cargos mensuales y totales, fechas de inicio y fin.
- **personal**: datos demográficos (género, adulto mayor, pareja, dependientes).
- **internet**: servicios de internet contratados (tipo de conexión, seguridad, soporte, streaming).
- **phone**: servicio de telefonía y líneas múltiples.

La variable objetivo (fuga) se construyó a partir de la fecha de finalización del contrato.

## Metodología

1. **Análisis exploratorio**: revisión de estructura, valores nulos, duplicados y tipos de datos en los cuatro archivos.
2. **Preprocesamiento**: unión de las tablas, creación de la variable de antigüedad, tratamiento de valores faltantes y análisis de las variables categóricas frente a la fuga.
3. **Verificación de *data leakage***: exclusión de variables que revelan el resultado.
4. **Modelado**: partición estratificada 3:1, codificación One-Hot y escalado con `StandardScaler`. Se comparan un modelo base (Dummy), regresión logística, bosque aleatorio y LightGBM.
5. **Manejo del desbalance de clases** con ajuste de pesos y **optimización de hiperparámetros** con `RandomizedSearchCV`.
6. **Selección del umbral de decisión** para priorizar la detección de clientes en riesgo (recall).
7. **Evaluación en el conjunto de prueba** e interpretación de la importancia de las variables.

## Resultados

Modelo elegido: **LightGBM** con ajuste de pesos para la clase minoritaria y umbral de decisión de 0,36.

| Métrica (prueba) | Umbral 0,36 (recomendado) | Umbral 0,50 |
|---|---|---|
| ROC-AUC | **0,91** | 0,91 |
| Recall | **0,89** | 0,81 |
| Precisión | 0,56 | 0,64 |

- **ROC-AUC de 0,91**, muy por encima del criterio de 0,75 y coherente con el valor de validación cruzada (0,90).
- De cada 100 clientes que van a cancelar, el modelo detecta 89 a tiempo para ofrecerles una promoción.
- Si se contacta a la mitad de los clientes, el modelo permite alcanzar cerca del 92 % de los que realmente se van, frente al 50 % de una selección aleatoria.

## Conclusiones para el negocio

- Las variables más influyentes son la **antigüedad del cliente**, el **tipo de contrato** y los **cargos facturados**.
- El perfil de mayor riesgo corresponde a clientes nuevos, con contrato mensual, pago con cheque electrónico, internet de fibra óptica y cargos mensuales altos. Una acción posible es ofrecerles planes anuales con descuento.
- Con un presupuesto de retención limitado, el umbral puede subirse a 0,50 o contactarse solo a una parte de los clientes marcados, y aun así se cubre una proporción importante de los casos de riesgo.
- El modelo debe reentrenarse periódicamente, ya que los datos cubren una ventana corta (octubre 2019 – febrero 2020).

## Herramientas

Python · pandas · NumPy · Matplotlib · seaborn · SciPy · scikit-learn · LightGBM

## Archivos

- [`interconnect_prediccion_fuga_clientes.ipynb`](interconnect_prediccion_fuga_clientes.ipynb): notebook completo con código, gráficos y análisis.
- `requirements.txt`: librerías utilizadas.

Los datos fueron proporcionados por TripleTen y no se incluyen en el repositorio. El notebook conserva las salidas de la ejecución original.

---

**Iván Ricardo Navarro Espinosa** — Ingeniero Químico, MSc · [GitHub](https://github.com/ivanrnavarro) · ivanrnavarro@gmail.com
Más proyectos del bootcamp en [tripleten-ciencia-de-datos](https://github.com/ivanrnavarro/tripleten-ciencia-de-datos).
