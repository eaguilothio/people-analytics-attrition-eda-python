# People Analytics — Análisis de Rotación de Talento
¿Qué factores explican que un empleado abandone la empresa?

---

## ¿Cuál era el problema de negocio?

El área de Recursos Humanos de ABC Corporation había identificado un patrón en los registros históricos: la rotación no afectaba de forma homogénea a toda la organización.

Los datos apuntaban a que la fuga se concentraba en determinados departamentos, con impacto directo en la estabilidad de los equipos y en la relación con los clientes.

El análisis se estructuró en torno a una pregunta central: **¿qué factores explican que un empleado abandone la empresa?**

---

## ¿Qué datos usé y de dónde salieron?

Dataset público de Kaggle — [IBM HR Analytics Employee Attrition & Performance](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset).

**1.470 empleados · 35 variables originales**

De las 35 variables originales se seleccionaron 8, organizadas en tres dimensiones:

| Dimensión | Variables |
|---|---|
| Estructura organizativa | `department` `jobrole` |
| Condiciones laborales | `overtime` `monthlyincome` `jobsatisfaction` |
| Ciclo de vida | `yearsatcompany` |
| Objetivo | `attrition` |

---

## ¿Qué herramienta usé y por qué?

**Python**, ejecutado en un Jupyter Notebook.

- Permite automatizar el procesamiento y análisis de grandes volúmenes de datos, superando las limitaciones de herramientas como Excel en rendimiento.
- Facilita la exploración rápida de información y la generación de insights accionables, acelerando la toma de decisiones basada en datos.

---

## El proceso analítico de principio a fin

### BLOQUE 1 — Exploración y limpieza

---

#### Paso 1 — Selección de variables

De las 35 variables originales se trabajó con 8. Menos variables bien justificadas generan más claridad que muchas variables mal elegidas.

---

#### Paso 2 — Estandarización

Columnas y valores categóricos unificados a minúsculas sin espacios. Sin este paso, Python trataría `"Sales"`, `"sales"` y `"SALES"` como tres departamentos distintos.

---

#### Paso 3 — Duplicados

Cada fila representa un empleado único. Se verificó que no existieran registros duplicados que pudieran inflar las tasas de rotación.

---

#### Paso 4 — Nulos

- `monthlyincome` — se imputa con la mediana por `jobrole`, ya que el salario varía estructuralmente entre roles.
- `jobsatisfaction`, `department`, `overtime` — porcentaje de nulos bajo (<5%). Se imputan con la moda para no perder registros.

---

### BLOQUE 2 — Análisis

---

#### Paso 5 — Tasa global de attrition

Antes de buscar causas, hay que dimensionar el problema.

Resultado: **16,1%** — ligeramente por encima del benchmark del sector tecnológico (~15%). Un porcentaje que no es de alarma, pero que puede ocultar concentraciones muy localizadas.

---

#### Paso 6 — Correlación con attrition

Se analizó qué variables se asocian con la fuga y en qué dirección.

`overtime` es la variable con mayor correlación positiva con `attrition`. Por el contrario, `jobsatisfaction`, `yearsatcompany` y `monthlyincome` muestran correlación negativa: a mayor satisfacción, antigüedad o salario, menor riesgo de fuga.

---

#### Paso 7 — Segmentación por departamento y rol

Las tasas globales ocultan la distribución real del riesgo. El análisis por segmento revela dónde se concentra la fuga.

---

## Resultados

- El departamento con mayor tasa de rotación es Sales (20.3%).
- El rol más vulnerable es Sales Representative (39.8%).
- Existe una relación positiva entre overtime y attrition, lo que sugiere que las horas extra podrían estar asociadas a mayor probabilidad de abandono.
- Variables como job satisfaction, years at company y monthly income muestran una relación negativa con la fuga, aunque de intensidad débil.
- La fuga suele ser un fenómeno multifactorial. Variables con menos relación con la variable de interés como salario, satisfacción o antigüedad no siempre explican la salida de forma individual, pero en conjunto pueden aumentar la probabilidad de que un empleado abandone la empresa.

---

## Acciones recomendadas

1. No tomar decisiones basadas únicamente en la tasa global — el 16.1% de attrition general oculta segmentos donde la fuga es especialmente relevante.
2. Revisar la política de horas extra en los departamentos críticos.

    Introducir:
    - compensación adicional clara
    - redistribución de la carga de trabajo

3. Los empleados con más años en la empresa o mayor ingreso presentan menor riesgo de fuga.

    Valorar:
    - ajustes salariales en departamentos críticos
    - refuerzo de la supervisión y acompañamiento del equipo junior

---

## Limitaciones

El dataset no incluye variable de fecha. Si los registros corresponden a períodos distintos, comparar la tasa global con un benchmark anual concreto sería metodológicamente incorrecto. Se asume que todos los registros pertenecen a un mismo período, pero no se puede verificar.

---

## Archivos del repositorio

```
dataset/ABC_Corp.csv       → dataset original
ABC_Corp.ipynb             → notebook con el análisis completo
README.md                  → documento del proceso
```
