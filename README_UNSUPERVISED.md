# Informe de Aprendizaje No Supervisado: Segmentación de Clientes de Crédito

## 1. Descripción de la técnica utilizada
Se seleccionó el algoritmo **K-Means Clustering** (Opción 1 del enunciado). 

### Justificación:
El objetivo del proyecto es el *Scoring Crediticio*. K-Means permite agrupar a los solicitantes en segmentos homogéneos basándose en su comportamiento financiero y demográfico (Ingresos, Monto del crédito, Edad, Antigüedad laboral). 
Esta técnica fue elegida porque permite:
1.  Identificar **perfiles de riesgo latentes** que no son obvios a simple vista.
2.  Validar si ciertos grupos demográficos tienen tasas de morosidad (`TARGET`) significativamente más altas que el promedio.

## 2. Instrucciones de ejecución
El análisis se encuentra en el script adjunto. Para ejecutarlo:
1.  Asegúrese de tener las librerías instaladas: `pandas`, `numpy`, `sklearn`, `matplotlib`, `seaborn`.
2.  Asegúrese de que el archivo `application_.parquet` se encuentre en la ruta `/content/` o actualice la ruta en la línea 15 del script.
3.  Ejecute el script completo. Se imprimirá en consola la tabla de perfiles y se generará un gráfico de dispersión (PCA).

## 3. Análisis e interpretación de resultados
Se aplicó K-Means con `k=4` clusters sobre datos estandarizados. 
*Nota: Los resultados varían según la ejecución, pero típicamente se observa:*

* **Cluster de "Bajo Riesgo":** Suele caracterizarse por personas con mayor antigüedad laboral, mayores ingresos y edad avanzada. Su tasa de mora (`TARGET mean`) tiende a ser la más baja.
* **Cluster de "Alto Riesgo":** A menudo agrupa a clientes más jóvenes (`DAYS_BIRTH` con valores absolutos menores), con menor antigüedad laboral y créditos pequeños pero frecuentes. Este grupo suele presentar la tasa de `Default_Rate` más alta.

La visualización mediante PCA (Análisis de Componentes Principales) muestra la separación espacial de estos grupos, confirmando que los clientes tienen características estructuralmente distintas.

## 4. Discusión: ¿Incorporar al proyecto final?
**SÍ, se recomienda incorporar esta técnica.**

### Razones:
1.  **Ingeniería de Características (Feature Engineering):** El `Cluster ID` asignado a cada cliente puede utilizarse como una nueva variable categórica (input) para el modelo supervisado de clasificación (XGBoost/Random Forest). Saber a qué "perfil" pertenece el cliente agrega información valiosa que puede mejorar el AUC-ROC.
2.  **Estrategia de Negocio:** Independiente del modelo predictivo, los clusters permiten al banco definir estrategias diferenciadas (ej. ofrecer tasas más bajas al "Cluster Seguro" o pedir más garantías al "Cluster Riesgoso").