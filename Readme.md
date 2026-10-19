# Northnote — Analytics con PostgreSQL

Análisis de **Northnote**, una app de notas y planificación por suscripción. El proyecto construye la base de datos en PostgreSQL a partir de 9 tablas exportadas, detecta y corrige sus problemas de calidad y responde preguntas de negocio sobre adquisición, conversión, retención, ingresos y uso del producto.

**Pregunta principal:** ¿qué diferencia a los clientes que pagan y se quedan de los que abandonan, y qué puede hacer Northnote para retener a más?

**Herramientas:** PostgreSQL

> Los datos son sintéticos: simulan el comportamiento de 10.000 clientes entre enero de 2024 y septiembre de 2026, con los errores típicos de una exportación real.

---

## Contexto del Negocio

Northnote ofrece 14 días de prueba gratis. Al terminar la prueba, la persona decide si paga por uno de tres planes, con cobro mensual o anual:

| Plan | Mensual (USD) | Anual (USD) |
|---|---|---|
| Starter | 9 | 90 |
| Pro | 19 | 190 |
| Team | 49 | 490 |

Los clientes pueden cambiar de plan, cancelar o volver tiempo después. Algunos entran con cupones (`WELCOME50`: 50 % en las primeras 3 facturas; `SAVE20`: 20 % en las primeras 6). Si un cobro falla, el sistema reintenta hasta tres veces; si todos los intentos fallan, la suscripción se cancela.

---

## Datos

La base tiene 9 tablas:

| Tipo | Tablas |
|---|---|
| Catálogos | `Countries`, `Acquisition_Channels`, `Plans`, `Coupons` |
| Clientes y Suscripciones | `Customers`, `Subscriptions` |
| Cobros | `Invoices`, `Payment_Attempts` |
| Uso del Producto | `Events` |

El glosario completo de columnas está en [`Data/Readme.md`](Data/Readme.md).

---

## Preguntas de Negocio

- ¿Cuánto se cobró cada mes?
- ¿Qué plan genera más ingresos y cuál tiene más clientes pagando?
- ¿Qué porcentaje de las pruebas gratis termina en pago, según el canal?
- ¿Cuánto ingreso deja en promedio un cliente según el canal por el que llegó?
- ¿Por qué cancela la gente y qué parte de las cancelaciones se debe a pagos fallidos?

- ¿Qué funciones se usan más y en qué plataforma?
- En sus primeros 7 días, ¿qué hacen distinto quienes terminan pagando?
- ¿Quienes conectan el calendario convierten más? ¿Eso prueba causalidad?
