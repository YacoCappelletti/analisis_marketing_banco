# Análisis de Campaña de Marketing Bancario

Este repositorio contiene el análisis detallado de una campaña de marketing realizada por una institución financiera portuguesa con el objetivo de promover depósitos a plazo. El estudio combina técnicas avanzadas de análisis exploratorio de datos (EDA), modelado predictivo y recomendaciones estratégicas basadas en evidencia para optimizar la eficacia de futuras campañas de adquisición de clientes.

El análisis se basa en un conjunto de datos que registra las interacciones con clientes durante una campaña telefónica, incluyendo información demográfica, comportamiento previo, indicadores macroeconómicos y resultados de la campaña. El enfoque metodológico prioriza la interpretación de resultados desde una perspectiva de negocio, con el fin de identificar segmentos de alto valor, factores críticos de conversión y oportunidades de mejora operativa.

## Introducción del Dataset

El dataset contiene 41,188 registros correspondientes a contactos realizados durante una campaña de marketing bancario. Cada registro representa un cliente potencial y contiene 21 variables que describen características personales, financieras, de contacto y contexto económico al momento de la interacción.

La variable objetivo, `y`, indica si el cliente suscribió un depósito a plazo tras la campaña (valores: *yes* / *no*). La tasa de conversión global es del 11.27%, un valor representativo dentro del sector bancario para campañas de este tipo.

Las variables incluyen:
- **Características demográficas**: edad, estado civil, nivel educativo, ocupación.
- **Productos financieros**: préstamos personales, hipotecas, morosidad.
- **Interacción en campaña**: duración de la llamada, número de contactos, canal utilizado.
- **Historial previo**: resultado de campañas anteriores, días desde último contacto.
- **Indicadores macroeconómicos**: tasa de interés Euribor a 3 meses, tasa de variación del empleo, índice de precios al consumidor, entre otros.

## Variables Clave Analizadas

El análisis exploratorio identificó varias variables con alto impacto en la decisión del cliente:

- **`duration`**: Duración del último contacto. Los clientes que convierten tienen una mediana de duración de 500 segundos, frente a 200 segundos en no convertidores.
- **`euribor3m`**: Tasa de interés a 3 meses. Valores bajos (<a 1%) se asocian a mayores tasas de conversión.
- **`nr.employed`** y **`emp.var.rate`**: Indicadores de empleo. Un entorno de empleo estable y alto número de empleados favorece la suscripción.
- **`poutcome`**: Resultado de campañas previas. Clientes con éxito previo tienen una tasa de conversión del 65.1%.
- **`contact`**: Canal de contacto. El uso de celular presenta una tasa de conversión del 14.7%, frente al 5.2% del teléfono fijo.
- **`month`**: Mes de contacto. Marzo, septiembre y diciembre registran tasas de conversión superiores al 44%, mientras que mayo-julio presentan tasas inferiores al 10%.

## Principales Hallazgos

1. **Perfil de cliente con alta propensión a convertir**:
   - Jubilados (25.2% de conversión).
   - Estudiantes (31.4% de conversión).
   - Personas con educación universitaria (13.7% de conversión).
   - Solteros (14.0% de conversión), frente a casados (10.2%).

2. **Impacto del contexto económico**:
   - Las condiciones macroeconómicas son los predictores más fuertes de conversión. Bajas tasas de interés y alto empleo incrementan significativamente la disposición a invertir.

3. **Patrones temporales relevantes**:
   - Existe una marcada estacionalidad: los meses de marzo, septiembre y diciembre presentan tasas de conversión 4 a 5 veces superiores al promedio.
   - El jueves es el día de la semana con mayor efectividad (12.1% de conversión).

4. **Efectividad del contacto**:
   - Duraciones superiores a 200 segundos están fuertemente asociadas a éxito.
   - Más de 3 contactos por cliente reduce la probabilidad de conversión, sugiriendo fatiga.

5. **Calidad de datos**:
   - Se identificó un 20.87% de valores faltantes en la variable `default` y 4.2% en `education`, lo que requiere estrategias de imputación robustas.
   - Alta correlación entre variables macroeconómicas (hasta 0.97), lo que sugiere riesgo de multicolinealidad.

## Recomendaciones Estratégicas

### Segmentación de Clientes
- Priorizar segmentos con alta tasa de conversión: jubilados, estudiantes y titulados universitarios.
- Evitar contactos con clientes jóvenes en empleos inestables o con deuda personal activa.

### Optimización Temporal
- Concentrar recursos en los meses de mayor efectividad: marzo, septiembre y diciembre.
- Programar el mayor volumen de contactos los jueves.
- Reducir esfuerzos en mayo-julio, donde la alta actividad no se traduce en conversiones.

### Gestión de Contacto
- Fomentar interacciones de larga duración (> 200 segundos).
- Limitar el número de contactos por cliente a 3-5 intentos por campaña.
- Priorizar el canal celular sobre el teléfono fijo.

### Uso de Datos y Modelos
- Monitorear indicadores macroeconómicos como termómetro para activar o desactivar campañas.
- Implementar estrategias de imputación avanzada para variables con datos faltantes.


## Modelado Predictivo

Se entrenaron y evaluaron dos modelos predictivos: **Random Forest** y **Regresión Logística**, con enfoque en maximizar el *recall* para capturar la mayor cantidad posible de clientes potenciales, minimizando falsos negativos.

### Preparación de Datos
- División temporal: entrenamiento (mar-jul), prueba (ago-dic).
- Exclusión de la variable `duration` para evitar *data leakage*.
- One-hot encoding para variables categóricas y escalado para numéricas.
- Manejo de valores faltantes mediante imputación por mediana.

### Resultados de Evaluación

| Modelo               | Accuracy | Precision | Recall  | F1-Score | ROC-AUC |
|----------------------|----------|-----------|---------|----------|---------|
| Random Forest        | 0.816    | 0.426     | 0.709   | 0.532    | 0.783   |
| Regresión Logística  | 0.795    | 0.393     | 0.714   | 0.507    | 0.797   |

Ambos modelos superan el umbral de discriminación aceptable (AUC > 0.7). La Regresión Logística presenta un ROC-AUC ligeramente superior, mientras que Random Forest ofrece mejor equilibrio entre precisión y exhaustividad.

### Importancia de Variables

- **Random Forest**: Destaca variables macroeconómicas como las más influyentes (`euribor3m`, `emp.var.rate`, `nr.employed`), lo que refuerza la dependencia del entorno económico.
- **Regresión Logística**: Confirma el impacto de `emp.var.rate` y `euribor3m`, y resalta el efecto negativo del canal `telephone`, reforzando la recomendación de priorizar celular.


## Potencial de Implementación

Los modelos desarrollados permiten priorizar clientes con mayor probabilidad de conversión, optimizando el uso de recursos y reduciendo costos operativos. La curva de lift muestra que, con el 10% de la población contactada, se puede capturar hasta el 39% de las conversiones, multiplicando por 6 la eficiencia frente a una selección aleatoria.

La combinación de ambos modelos ofrece un enfoque híbrido:
- **Random Forest** para identificación temprana de casos de alto potencial.
- **Regresión Logística** para despliegue estable en campañas de mayor escala.


## Conclusión

Este análisis demuestra que la efectividad de una campaña de marketing bancario no depende únicamente del volumen de contactos, sino de una estrategia informada por datos que considere el perfil del cliente, el contexto económico y el timing operativo. La integración de modelos predictivos con recomendaciones accionables permite transformar datos en decisiones estratégicas, maximizando el retorno de inversión y fortaleciendo la capacidad de respuesta del banco frente a condiciones cambiantes del mercado.