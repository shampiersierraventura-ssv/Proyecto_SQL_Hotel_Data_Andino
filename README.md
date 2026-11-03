# 🏨 Proyecto SQL | Análisis de Cancelaciones Hoteleras — DataAndino

![SQL](https://img.shields.io/badge/SQL-Analytics-336791?style=flat-square)
![Databricks](https://img.shields.io/badge/Databricks-SQL-FF3621?style=flat-square)
![Estado](https://img.shields.io/badge/Proyecto-Académico-informational?style=flat-square)

## 📌 Descripción del proyecto

Este proyecto aborda un caso de análisis de datos para **DataAndino**, un holding hotelero que busca comprender el comportamiento de las cancelaciones de reservas y estimar su impacto económico. A través de consultas **SQL en Databricks**, se exploran las características de las reservas, se calculan costos asociados a las cancelaciones y se contrastan hipótesis sobre los factores relacionados con el riesgo de cancelación.

El enfoque combina **exploración y transformación de datos, análisis financiero y generación de información para la toma de decisiones comerciales**.

## 🎯 Objetivo general

Analizar las cancelaciones de reservas hoteleras mediante SQL para cuantificar las pérdidas económicas asociadas y reconocer patrones que contribuyan a mejorar la gestión de reservas y la negociación con agencias intermediarias.

### Objetivos específicos

- Explorar, limpiar y transformar los registros de reservas mediante consultas SQL.
- Identificar la distribución de reservas por canal, categoría de precio y tipo de hotel.
- Estimar los costos de intermediación correspondientes a reservas canceladas.
- Cuantificar el costo operativo asociado a cancelaciones realizadas con menos de tres días de anticipación.
- Aplicar operaciones `JOIN` para integrar información complementaria de países, vendedores y facturas, en los ejercicios que suponen estas tablas.
- Evaluar asociaciones entre cancelaciones y composición familiar, solicitudes especiales, modificaciones de reserva y precio.

## 💼 Contexto del negocio

DataAndino enfrenta dos posibles fuentes de pérdidas relacionadas con las cancelaciones:

| Concepto | Supuesto del caso | Impacto |
|---|---|---|
| Comisión de intermediación | USD 1.50 por reserva generada mediante el canal `TA/TO` | Se paga incluso si la reserva se cancela. |
| Cancelación tardía | USD 120 por reserva cancelada con menos de 3 días de anticipación | Costo operativo hundido estimado por Finanzas. |

El análisis busca dimensionar estas pérdidas y aportar evidencia para **revisar las condiciones comerciales con las agencias** y fortalecer la gestión del riesgo de cancelación.

## 🗃️ Fuente de datos

**Dataset:** [SQL — Proyecto 1: Análisis de Cancelaciones Hoteleras (Kaggle)](https://www.kaggle.com/datasets/datacertlaboratoria/sql-proyecto-1-analisis-de-cancelaciones-hoteleras)

El caso utiliza como tabla principal `tb_reservas`, que contiene información sobre características de las reservas, fechas, canales de distribución, tarifas y cancelaciones.

**Variables relevantes:**

| Variable | Descripción |
|---|---|
| `hotel` | Tipo de hotel (`City Hotel` o `Resort Hotel`). |
| `is_canceled` | Indicador de cancelación: 1 = cancelada; 0 = no cancelada. |
| `lead_time` | Días entre la creación de la reserva y la llegada. |
| `arrival_date_year`, `arrival_date_month`, `arrival_date_day_of_month` | Componentes de la fecha de llegada. |
| `distribution_channel` | Canal de distribución de la reserva. |
| `reservation_status_date` | Fecha de actualización del estado de la reserva. |
| `adr` | Tarifa diaria promedio (Average Daily Rate). |
| `children`, `babies` | Número de niños y bebés. |
| `total_of_special_requests` | Número de solicitudes especiales. |
| `booking_changes` | Número de modificaciones de la reserva. |
| `country` | Código de país asociado a la reserva. |

> **Nota:** Algunos ejercicios suponen columnas adicionales (`arrival_date`, `id_reserva`, `id_vendedor`) o tablas complementarias (`tb_paises`, `tb_vendedores`, `tb_facturas`). Su disponibilidad debe verificarse o prepararse antes de ejecutar las consultas correspondientes.

## 🛠️ Tecnologías y técnicas

- **Databricks SQL:** ejecución de consultas y análisis de datos.
- **SQL:** consultas, transformaciones y agregaciones.
- **Manipulación de datos:** `CREATE`, `INSERT`, `UPDATE`, `DELETE`, `CAST`, `CONCAT` y `CASE WHEN`.
- **Análisis descriptivo:** `COUNT`, `SUM`, `AVG`, `GROUP BY` y filtros.
- **Funciones de fechas y matemáticas:** `DATE_SUB`, diferencias de fechas y `CEIL`.
- **Integración de datos:** `LEFT JOIN`, `INNER JOIN`, `FULL OUTER JOIN` y `RIGHT JOIN`.

## 🔎 Desarrollo del análisis

El proyecto comprende **20 ejercicios**, organizados en tres etapas:

### 1. Limpieza, exploración y transformación de datos (ejercicios 1–10)

Se realizan operaciones de creación y modificación de tablas de prueba, estandarización de fechas, filtrado de canales, agregación de métricas y clasificación de reservas por rango tarifario. También se calculan fechas de reserva y meses aproximados de anticipación.

**Propósito:** comprender la estructura de la información y preparar variables útiles para el análisis.

### 2. Impacto financiero e integración de datos (ejercicios 11–16)

Se estiman dos componentes de pérdida económica:

**Costo de marketing asociado a reservas canceladas:**

```text
Pérdida por intermediación (USD)
= Reservas canceladas del canal TA/TO × 1.50
```

**Costo operativo por cancelaciones tardías:**

```text
Pérdida operativa (USD)
= Cancelaciones con menos de 3 días de anticipación × 120
```

Además, se plantean cruces entre reservas y tablas complementarias para analizar países, asignación de vendedores y registros de facturación.

**Propósito:** traducir las cancelaciones en métricas financieras y ampliar el análisis mediante relaciones entre tablas.

### 3. Validación de hipótesis de cancelación (ejercicios 17–20)

| Hipótesis | Segmentación o métrica | Pregunta de negocio |
|---|---|---|
| Composición familiar | Reservas con hijos vs. sin hijos | ¿Qué grupo presenta una mayor tasa de cancelación? |
| Solicitudes especiales | Cantidad de solicitudes | ¿La tasa de cancelación cambia según las solicitudes? |
| Modificaciones | Sin cambios, 1–3 cambios y más de 3 cambios | ¿Modificar una reserva se relaciona con menor riesgo de cancelación? |
| Precio | Tarifa promedio de reservas canceladas por tipo de hotel | ¿Qué tipo de hotel presenta una tarifa promedio de cancelación más alta? |

La tasa de cancelación se calcula como:

\[
\text{Tasa de cancelación (\%)} = \frac{\text{Reservas canceladas}}{\text{Total de reservas}} \times 100
\]

En SQL, cuando `is_canceled` contiene únicamente valores 0 y 1, esta tasa puede obtenerse mediante `AVG(is_canceled) * 100`.

**Propósito:** identificar patrones descriptivos de cancelación y generar evidencia para la gestión comercial. Estas comparaciones no implican, por sí solas, relaciones causales.

## 📈 Resultados esperados

El desarrollo de las consultas permitirá obtener:

- Indicadores de volumen y tasa de cancelaciones.
- Estimaciones del costo de comisiones pagadas por reservas canceladas.
- Estimaciones del costo operativo de cancelaciones tardías.
- Comparaciones de cancelaciones por características de la reserva.
- Hallazgos que orienten la revisión de contratos de intermediación y las políticas de reservas.

> Los valores finales y las conclusiones específicas deben incorporarse una vez ejecutadas y validadas las consultas SQL. Este README describe el alcance del proyecto, no resultados ya comprobados.

## 🚀 Ejecución

1. Obtener los datos desde la fuente de Kaggle.
2. Cargar el conjunto de datos en un entorno compatible con **Databricks SQL** y preparar la tabla `tb_reservas`.
3. Verificar los tipos de datos, las columnas disponibles y los supuestos de cada ejercicio.
4. Ejecutar las consultas SQL correspondientes a las tres etapas del proyecto.
5. Revisar los resultados y documentar una conclusión de negocio breve por ejercicio (máximo tres líneas, conforme a las instrucciones).

**Consideración:** las operaciones de modificación y eliminación deben realizarse sobre `tb_reservas_prueba`, no sobre la tabla original.

## 📚 Referencias

- [Dataset de cancelaciones hoteleras — Kaggle](https://www.kaggle.com/datasets/datacertlaboratoria/sql-proyecto-1-analisis-de-cancelaciones-hoteleras)
- [Guía del proyecto: Análisis de Cancelaciones en Hotel DataAndino usando SQL-Databricks](https://drive.google.com/file/d/13J96HvUK6ZXrligQPi97mm84ZMSZsIJ8/view?usp=sharing)
- [Documentación oficial de Databricks SQL](https://docs.databricks.com/en/sql/index.html)

---

**Proyecto académico de análisis de datos con SQL y Databricks.**
