# 📊 People Analytics — Análisis de Rotación de Talento
**ABC Corporation · EDA Project · Python**

---

## 🧠 Contexto

A primera vista, la rotación de empleados en ABC Corporation no representa una señal de alarma. Sin embargo, las métricas globales no siempre reflejan lo que ocurre dentro de cada área de la empresa. Recursos Humanos detectó indicios de que la salida de empleados podría estar concentrándose en determinados puestos críticos, generando un impacto desproporcionado sobre la operación.

Este análisis nace para identificar dónde se produce realmente la fuga de talento, medir su magnitud y comprender qué variables están asociadas a ella. A través del análisis exploratorio de datos, el proyecto busca aportar una base objetiva para diseñar estrategias de retención más efectivas.

---

## 📁 Estructura del proyecto

```
├── EDA_attrition_ABC_Corp.ipynb   # Notebook principal
├── datasets/
│   └── ABC_Corp.csv               # Dataset de empleados
└── README.md
```

---

## 🗂️ Variables analizadas

| Variable | Pregunta de negocio |
|---|---|
| `department`, `jobrole` | ¿Dónde se concentra el problema? |
| `monthlyincome` | ¿El salario influye en la decisión de irse? |
| `yearsatcompany` | ¿Los empleados más nuevos son más vulnerables? |
| `jobsatisfaction` | ¿La satisfacción laboral protege contra la fuga? |
| `overtime` | ¿Las horas extra empujan a la salida? |
| `attrition` | Variable objetivo — ¿quién se fue? |

---

## ⚙️ Decisiones técnicas

### Selección de variables
De todas las columnas disponibles, se seleccionaron **8 variables** con criterio de negocio: cada una responde a una pregunta concreta de RRHH, evitando incluir variables sin interpretabilidad práctica.

### Limpieza de datos
- **Estandarización:** columnas y valores convertidos a `snake_case` para evitar errores por mayúsculas o espacios.
- **Duplicados:** eliminados por `employeenumber` (ID único del empleado), conservando el primer registro.
- **Nulos:** imputación diferenciada por tipo de variable:
  - `monthlyincome` → mediana por `jobrole` (respeta diferencias salariales entre puestos)
  - `jobsatisfaction`, `department`, `overtime` → moda (valor más frecuente)
- **Validación de rangos:** verificación de que salarios, antigüedad y satisfacción tienen valores lógicos.

### Análisis 
El análisis se estructuró en dos grandes fases:

**Fase 1 — Descripción base**
Antes de buscar causas, era necesario entender quiénes son los empleados y cómo está organizada la empresa. Esta fase no cruza ninguna variable con `attrition` todavía — solo construye el mapa de la realidad organizativa: cómo se distribuye la plantilla por departamento y puesto, cuál es el perfil económico y laboral típico (salario mediano, antigüedad, satisfacción), y cuál es la tasa global de rotación. Este último dato es el punto de partida del problema y la referencia contra la que se leerán todos los segmentos posteriores.

**Fase 2 — Determinar qué factores se asocian con la fuga**

Esta fase se divide en tres bloques que se complementan:

- **Factores numéricos (heatmap de correlación):** se codificó `attrition` y `overtime` como variables binarias (0/1) y se calculó la correlación de Pearson de cada variable numérica con `attrition`. El heatmap permite ver de un vistazo la dirección (positiva o negativa) e intensidad de cada relación. Esto establece una jerarquía de variables: cuáles merecen investigación y cuáles son básicamente ruido.

- **Factores categóricos (crosstabs):** se calcularon tablas de contingencia normalizadas por fila para obtener la tasa de fuga dentro de cada grupo. Primero la dimensión actitudinal — niveles de satisfacción laboral — y luego la estructural — departamento y puesto. Normalizar por fila es clave: permite comparar grupos de distinto tamaño en igualdad de condiciones, sin que un departamento grande distorsione los porcentajes.

- **Cruce departamento/puesto × horas extra:** el heatmap señaló `overtime` como el factor numérico con mayor asociación con `attrition`. Este tercer bloque investiga si ese desgaste se concentra precisamente en los segmentos que ya identificamos como críticos, o si es un problema transversal. El cruce confirma que `sales` acumula tanto la mayor tasa de fuga como el mayor porcentaje de empleados con horas extra — lo que apunta a una causa estructural, no aleatoria.

---

## 💡 Hallazgos principales

- **Tasa global de attrition: 16.1%** — el número global oculta segmentos críticos.
- **Departamento más vulnerable:** `sales` (20.3% de fuga).
- **Puesto más vulnerable:** `sales_representative` (39.8% de fuga).
- **Satisfacción:** empleados con nivel 1 tienen el doble de fuga que los de nivel 4 (22.6% vs 11.2%).
- **Horas extra:** correlación de 0.25 con attrition; `sales` concentra tanto el mayor overtime (28.6%) como la mayor fuga.


