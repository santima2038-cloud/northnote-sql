# Datos

Base de datos de **Northnote**, una app de notas y planificación por suscripción. Son 9 tablas exportadas en CSV, una por archivo. Los datos son sintéticos: simulan 10.000 clientes entre enero 2024 y septiembre 2026.

Los archivos se conservan tal como fueron exportados, con sus errores originales. La limpieza se hace en SQL (ver [`../sql/`](../sql/)).

| Archivo | Una fila es | Filas |
|---|---|---|
| `raw/countries.csv` | Un país | 19 |
| `raw/acquisition_channels.csv` | Un canal de adquisición | 6 |
| `raw/plans.csv` | Un plan de suscripción | 6 |
| `raw/coupons.csv` | Un cupón de descuento | 2 |
| `raw/customers.csv` | Un cliente registrado | 10.208 |
| `raw/subscriptions.csv` | Una suscripción de un cliente | 10.214 |
| `raw/invoices.csv` | Una factura (el cobro de un periodo) | 19.286 |
| `raw/payment_attempts.csv` | Un intento de cobro de una factura | 22.593 |
| `raw/events.csv` | Una acción en la app o en el sitio web | 286.022 |

---

## `countries.csv`

| Columna | Descripción |
|---|---|
| `country_id` | Identificador del país |
| `country_code` | Código ISO de dos letras |
| `country_name` | Nombre del país |

## `acquisition_channels.csv`

| Columna | Descripción |
|---|---|
| `channel_id` | Identificador del canal |
| `channel_code` | Código del canal |
| `channel_name` | Nombre del canal |

## `plans.csv`

| Columna | Descripción |
|---|---|
| `plan_id` | Identificador del plan |
| `plan_code` | Código del plan |
| `plan_name` | Nombre comercial: Starter, Pro o Team |
| `billing_cycle` | Ciclo de cobro: `monthly` o `annual` |
| `price` | Precio de lista en USD |

## `coupons.csv`

| Columna | Descripción |
|---|---|
| `coupon_id` | Identificador del cupón |
| `coupon_code` | Código que usa el cliente |
| `discount_percent` | Porcentaje de descuento |
| `discounted_invoices` | Número de facturas a las que se aplica el descuento |

Los cupones solo se aplican a planes mensuales.

## `customers.csv`

| Columna | Descripción |
|---|---|
| `customer_id` | Identificador del cliente |
| `full_name` | Nombre completo |
| `email` | Email de registro |
| `country_id` | País del cliente |
| `channel_id` | Canal por el que llegó |
| `signup_date` | Fecha de registro |

## `subscriptions.csv`

La prueba gratuita de 14 días es solo para quien se suscribe por primera vez. Un cliente que cancela y vuelve tiene una suscripción nueva, sin prueba.

| Columna | Descripción |
|---|---|
| `subscription_id` | Identificador de la suscripción |
| `customer_id` | Cliente dueño de la suscripción |
| `plan_id` | Plan con el que empezó la suscripción |
| `coupon_id` | Cupón aplicado, si tuvo |
| `status` | Estado actual: `trialing`, `trial_expired`, `active` o `canceled` |
| `created_at` | Fecha de inicio de la suscripción |
| `trial_end` | Fin de la prueba gratuita. Vacío si no tuvo prueba |
| `converted_at` | Fecha del primer pago. Vacío si nunca pagó |
| `canceled_at` | Fecha de la solicitud de cancelación, o del último cobro fallido |
| `access_ends_at` | Fecha en que el cliente pierde el acceso |
| `cancel_reason` | Motivo de cancelación. `payment_failed` indica que no se pudo cobrar |

## `invoices.csv`

| Columna | Descripción |
|---|---|
| `invoice_id` | Identificador de la factura |
| `subscription_id` | Suscripción a la que corresponde |
| `plan_id` | Plan facturado en ese periodo |
| `period_start` | Inicio del periodo facturado |
| `period_end` | Fin del periodo facturado (fecha de renovación) |
| `amount` | Monto antes del descuento, en USD |
| `discount_amount` | Descuento aplicado, en USD |
| `status` | `paid`, `failed` (se agotaron los reintentos) o `pending` (reintentos en curso) |

## `payment_attempts.csv`

Si un cobro falla, el sistema reintenta hasta 3 veces.

| Columna | Descripción |
|---|---|
| `attempt_id` | Identificador del intento |
| `invoice_id` | Factura que se intentó cobrar |
| `attempt_number` | Número de intento (1 a 4) |
| `attempted_at` | Fecha del intento |
| `result` | `succeeded` o `failed` |
| `failure_reason` | Código de error si el cobro falló |

## `events.csv`

Cubre los primeros 45 días de cada usuario desde su registro y las visitas a la página de precios de personas que todavía no se habían registrado. Antes del registro solo hay `anonymous_id`; el evento `signup_completed` trae `anonymous_id` y `customer_id`.

| Columna | Descripción |
|---|---|
| `event_id` | Identificador del evento |
| `event_time` | Fecha y hora del evento, en UTC |
| `event_name` | Tipo de acción (ver tabla de eventos) |
| `customer_id` | Cliente que hizo la acción |
| `anonymous_id` | Identificador del visitante antes de registrarse |
| `session_id` | Sesión de uso a la que pertenece el evento |
| `platform` | Plataforma: web, iOS, Android o escritorio |
| `app_version` | Versión de la app |

| Evento | Descripción |
|---|---|
| `pricing_page_viewed` | Visita a la página de precios |
| `signup_completed` | Registro completado |
| `app_opened` | Apertura de la app (inicio de sesión de uso) |
| `note_created` | Creación de una nota |
| `search_used` | Uso del buscador |
| `template_used` | Uso de una plantilla |
| `note_shared` | Nota compartida |
| `reminder_set` | Recordatorio programado |
| `calendar_connected` | Calendario conectado |
| `workspace_invite_sent` | Invitación enviada a un espacio de equipo |
| `export_pdf` | Nota exportada a PDF |
| `session_ended` | Cierre de la sesión de uso |
