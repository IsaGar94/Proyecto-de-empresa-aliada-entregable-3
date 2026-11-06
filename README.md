# Proyecto Empresa Aliada – Entregable 3
## Análisis de agrupamiento (Clustering) para la marca VANISH

## Descripción del proyecto

En este entregable se desarrolla un análisis de agrupamiento (*clustering*) para los registros de la marca **VANISH**, con el propósito de identificar grupos de observaciones que compartan características similares en sus variables de ventas.

Para realizar el análisis se integran las tablas de información del proyecto, se filtran los registros correspondientes a VANISH y se aplica el algoritmo **K-Means**. También se utiliza el método del codo para explorar el número de clústeres y se generan visualizaciones que permiten revisar la distribución y las características de los grupos obtenidos.

## Objetivo

Aplicar una técnica de aprendizaje no supervisado para segmentar los registros de la marca VANISH a partir de variables relacionadas con sus ventas, y explorar las diferencias entre los clústeres resultantes.

## Archivos de entrada

- `DIM_CALENDAR (2).xlsx`
- `DIM_CATEGORY (2).csv`
- `FACT_SALES (1).csv`
- `DIM_SEGMENT (1).xlsx`
- `DIM_PRODUCT (1).xlsx`

## Herramientas y bibliotecas

- **Python:** procesamiento y análisis.
- **Pandas:** lectura, limpieza, integración y transformación de datos.
- **NumPy:** operaciones numéricas.
- **Matplotlib** y **Seaborn:** visualización de datos.
- **Scikit-learn:** `StandardScaler` para estandarizar variables y `KMeans` para el agrupamiento.

## Desarrollo del análisis

### 1. Carga y exploración de los datos

Se cargan las cinco tablas de entrada y se revisan los primeros registros, los nombres de las columnas, los valores nulos y los registros duplicados.

### 2. Integración de las tablas

Se construye una base de datos mediante las siguientes uniones:

1. `FACT_SALES` con `DIM_PRODUCT`, utilizando `ITEM_CODE` e `ITEM`.
2. El resultado con `DIM_CATEGORY`, utilizando `CATEGORY` e `ID_CATEGORY`.
3. El resultado con `DIM_SEGMENT`, utilizando `CATEGORY`, `ATTR1`, `ATTR2`, `ATTR3` y `FORMAT`.
4. Finalmente, se integra `DIM_CALENDAR` mediante el campo `WEEK`.

Después de las uniones, se eliminan espacios al inicio y al final de los valores de las columnas de tipo texto.

### 3. Selección de la marca VANISH

Se revisan las marcas disponibles y se filtran los registros cuya columna `BRAND` corresponde a `VANISH`. También se consulta la cantidad de registros obtenidos y se revisan los valores nulos del subconjunto seleccionado.

### 4. Selección de variables

Para el análisis se utilizan:

- `TOTAL_UNIT_SALES`: ventas totales en unidades.
- `TOTAL_VALUE_SALES`: ventas totales en valor.
- `TOTAL_UNIT_AVG_WEEKLY_SALES`: promedio semanal de ventas en unidades.

### 5. Estandarización de variables

Se aplica `StandardScaler` a las tres variables seleccionadas. La estandarización permite llevarlas a una escala comparable antes de aplicar K-Means, evitando que las diferencias en sus magnitudes dominen el cálculo de distancias.

### 6. Método del codo

Se calcula la inercia del modelo K-Means para valores de `k` desde 1 hasta 10. Después, se genera la gráfica del método del codo para observar cómo disminuye la inercia al aumentar el número de clústeres.

En el código se establece `k_optimo = 3`, por lo que el modelo final se configura para generar tres clústeres.

### 7. Aplicación de K-Means

Se entrena K-Means con tres clústeres y `random_state=42`. Las etiquetas generadas por el modelo se agregan a la base de resultados en una nueva columna llamada `CLUSTER`.

### 8. Análisis de los clústeres

Se realizan consultas para conocer la composición y características de los grupos:

- Conteo de registros por clúster.
- Promedio de `TOTAL_UNIT_SALES`, `TOTAL_VALUE_SALES` y `TOTAL_UNIT_AVG_WEEKLY_SALES` por clúster.
- Distribución de los clústeres por `REGION`.
- Distribución de los clústeres por `SEGMENT`.
- Distribución de los clústeres por `CATEGORY`.

Estas consultas permiten comparar el tamaño y el comportamiento promedio de los grupos, así como explorar su relación con otras características de los registros.

## Visualizaciones generadas

El código genera las siguientes gráficas:

1. **Gráfica del método del codo:** muestra la inercia para distintos valores de `k` y apoya la selección del número de clústeres.
2. **Gráfica de dispersión de clústeres:** representa `TOTAL_UNIT_SALES` frente a `TOTAL_VALUE_SALES`, diferenciando los registros según el clúster asignado.
3. **Gráfica de cantidad de registros por clúster:** muestra cuántos registros pertenecen a cada grupo.
4. **Gráfica de ventas promedio por clúster:** compara el promedio de `TOTAL_VALUE_SALES` entre los grupos.

Las gráficas se muestran en el entorno de ejecución mediante `plt.show()`.

## Archivo de salida

Al finalizar el análisis, se exporta la base de resultados a:

- `Resultados_Clustering_Vanish_EJ.csv`

El archivo contiene los registros utilizados en el análisis y la columna `CLUSTER` con el grupo asignado por K-Means.

## Estructura sugerida del proyecto

```text
Proyecto Empresa Aliada Entregable 3/
│
├── DIM_CALENDAR (2).xlsx
├── DIM_CATEGORY (2).csv
├── FACT_SALES (1).csv
├── DIM_SEGMENT (1).xlsx
├── DIM_PRODUCT (1).xlsx
│
├── Codigo.ipynb
│
└── Resultados_Clustering_Vanish_EJ.csv
```

## Consideraciones

- El análisis de clustering se realiza únicamente sobre los registros de la marca VANISH.
- Las variables utilizadas para formar los clústeres son las tres variables de ventas indicadas en este documento.
- El código evalúa la inercia para valores de `k` entre 1 y 10, pero establece directamente tres clústeres para el modelo final.
- Para confirmar que tres es la elección más adecuada, es necesario revisar la gráfica del codo y valorar si ese punto representa una disminución importante de la inercia seguida de una reducción más gradual.
- Las gráficas se muestran en el notebook, pero el código compartido no incluye instrucciones `savefig()` para guardarlas como archivos de imagen.

## Conclusión

En este entregable se integra la información de las tablas del proyecto y se aplica K-Means a los registros de la marca VANISH, utilizando tres variables relacionadas con las ventas y estandarizándolas previamente.

El análisis permite asignar cada registro a uno de tres clústeres y explorar sus diferencias mediante estadísticas descriptivas, tablas de distribución y gráficas. Los resultados se exportan a un archivo CSV para facilitar su consulta y utilización posterior.

La interpretación de los perfiles de cada clúster debe realizarse a partir de los promedios y distribuciones obtenidos durante la ejecución del notebook.

***Nota***: La gráfica del método del codo y las visualizaciones de los clusters se muestran al ejecutar el notebook. En el código compartido no se incluye una instrucción savefig() para guardar estas gráficas como archivos de imagen.
