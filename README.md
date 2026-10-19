# Northnote — Growth & Product Analytics con PostgreSQL

Análisis de **Northnote**, una app de notas y planificación por suscripción. El proyecto construye la base de datos en PostgreSQL a partir de 9 tablas exportadas, detecta y corrige sus problemas de calidad y responde preguntas de negocio sobre adquisición, conversión, retención, ingresos y uso del producto.

**Herramientas:** PostgreSQL · pgAdmin · SQL (JOINs, agregaciones, subconsultas, CTEs) · _[herramienta del dashboard]_

> Los datos son sintéticos: simulan el comportamiento de 10.000 clientes entre enero 2024 y septiembre 2026, con los errores típicos de una exportación real.

---

## Contexto del negocio

Northnote ofrece 14 días de prueba gratis. Al terminar la prueba, la persona decide si paga por uno de tres planes, con cobro mensual o anual:

| Plan | Mensual (USD) | Anual (USD) |
|---|---|---|
| Starter | 9 | 90 |
| Pro | 19 | 190 |
| Team | 49 | 490 |

Los clientes pueden cambiar de plan, cancelar o volver tiempo después. Algunos entran con cupones (`WELCOME50`: 50 % en 3 facturas; `SAVE20`: 20 % en 6). Si un cobro falla, el sistema reintenta; si todos los intentos fallan, la suscripción se cancela.

**Pregunta principal:** _[escribe aquí la pregunta que guía tu análisis]_

---

## Datos

9 tablas: catálogos (países, canales, planes, cupones), clientes, suscripciones, facturas, intentos de pago y eventos de uso del producto. El diccionario completo está en [`data/README.md`](data/README.md).

---

## Modelo de datos

_[imagen del diagrama ER]_

_[qué representa una fila en cada tabla, cuáles son las llaves primarias y foráneas, y la cardinalidad de cada relación]_

---

## Calidad de datos

| Tabla | Problema | Filas afectadas | Qué decidí |
|---|---|---|---|
| _[tabla]_ | _[problema]_ | _[n]_ | _[corrección o exclusión]_ |

Las correcciones están en [`sql/02_limpieza.sql`](sql/02_limpieza.sql).

---

## Análisis

Las consultas están en [`sql/03_analisis.sql`](sql/03_analisis.sql).

**Growth**
- ¿Cuánto se cobró cada mes?
- ¿Qué plan genera más ingresos y cuál tiene más clientes pagando?
- ¿Qué porcentaje de las pruebas gratis termina en pago, según el canal?
- ¿Cuánto ingreso deja en promedio un cliente según el canal por el que llegó?
- ¿Por qué cancela la gente y qué parte de las cancelaciones se debe a pagos fallidos?

**Product**
- ¿Qué funciones se usan más y en qué plataforma?
- En sus primeros 7 días, ¿qué hacen distinto quienes terminan pagando?
- ¿Quienes conectan el calendario convierten más? ¿Eso prueba causalidad?

---

## Hallazgos

1. _[hallazgo con número]_
2. _[hallazgo con número]_
3. _[hallazgo con número]_

---

## Dashboard

_[link al dashboard]_

_[captura]_

---

## Qué haría después

_[próximos pasos o preguntas que quedaron abiertas]_

---

## Estructura del repositorio

```
northnote-sql/
├── README.md
├── data/
│   ├── README.md         ← diccionario de datos
│   └── raw/              ← los 9 CSV originales
├── diagrama/             ← diagrama ER
├── sql/
│   ├── 01_tablas.sql     ← creación de tablas
│   ├── 02_limpieza.sql   ← corrección de errores
│   └── 03_analisis.sql   ← consultas de negocio
└── dashboard/            ← capturas
```
