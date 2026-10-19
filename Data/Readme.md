# Datos

Base de datos de **Northnote**, una app de notas y planificación por suscripción. Son 9 tablas exportadas en CSV, una por archivo. Los datos son sintéticos: simulan 10.000 clientes entre enero de 2024 y septiembre de 2026.

Los archivos se conservan tal como fueron exportados, con sus errores originales. La limpieza se hace en SQL.

| Archivo | Una Fila Es | Filas |
|---|---|---|
| `Raw/Countries.csv` | Un País | 19 |
| `Raw/Acquisition_Channels.csv` | Un Canal de Adquisición | 6 |
| `Raw/Plans.csv` | Un Plan de Suscripción | 6 |
| `Raw/Coupons.csv` | Un Cupón de Descuento | 2 |
| `Raw/Customers.csv` | Un Cliente Registrado | 10.208 |
| `Raw/Subscriptions.csv` | Una Suscripción de un Cliente | 10.214 |
| `Raw/Invoices.csv` | Una Factura (el Cobro de un Periodo) | 19.286 |
| `Raw/Payment_Attempts.csv` | Un Intento de Cobro de una Factura | 22.593 |
| `Raw/Events.csv` | Una Acción en la App o en el Sitio Web | 286.022 |

---

## `Countries.csv`

| Columna | Descripción |
|---|---|
| `Country_ID` | Identificador del país |
| `Country_Code` | Código ISO de dos letras |
| `Country_Name` | Nombre del país |

## `Acquisition_Channels.csv`

| Columna | Descripción |
|---|---|
| `Channel_ID` | Identificador del canal |
| `Channel_Code` | Código del canal |
| `Channel_Name` | Nombre del canal |

## `Plans.csv`

| Columna | Descripción |
|---|---|
| `Plan_ID` | Identificador del plan |
| `Plan_Code` | Código del plan |
| `Plan_Name` | Nombre comercial: Starter, Pro o Team |
| `Billing_Cycle` | Ciclo de cobro: `monthly` o `annual` |
| `Price` | Precio de lista en USD |

## `Coupons.csv`

| Columna | Descripción |
|---|---|
| `Coupon_ID` | Identificador del cupón |
| `Coupon_Code` | Código que usa el cliente |
| `Discount_Percent` | Porcentaje de descuento |
| `Discounted_Invoices` | Número de facturas a las que se aplica el descuento |

Los cupones solo se aplican a planes mensuales.

## `Customers.csv`

| Columna | Descripción |
|---|---|
| `Customer_ID` | Identificador del cliente |
| `Full_Name` | Nombre completo |
| `Email` | Email de registro |
| `Country_ID` | País del cliente |
| `Channel_ID` | Canal por el que llegó |
| `Signup_Date` | Fecha de registro |

## `Subscriptions.csv`

La prueba gratuita de 14 días es solo para quien se suscribe por primera vez. Un cliente que cancela y vuelve tiene una suscripción nueva, sin prueba.

| Columna | Descripción |
|---|---|
| `Subscription_ID` | Identificador de la suscripción |
| `Customer_ID` | Cliente dueño de la suscripción |
| `Plan_ID` | Plan con el que empezó la suscripción |
| `Coupon_ID` | Cupón aplicado. Vacío si no usó cupón |
| `Status` | Estado actual: `trialing`, `trial_expired`, `active` o `canceled` |
| `Created_At` | Fecha de inicio de la suscripción |
| `Trial_End` | Fin de la prueba gratuita. Vacío si no tuvo prueba |
| `Converted_At` | Fecha del primer pago. Vacío si nunca pagó |
| `Canceled_At` | Fecha de la solicitud de cancelación, o del último cobro fallido |
| `Access_Ends_At` | Fecha en que el cliente pierde el acceso |
| `Cancel_Reason` | Motivo de cancelación. `payment_failed` indica que no se pudo cobrar |

## `Invoices.csv`

| Columna | Descripción |
|---|---|
| `Invoice_ID` | Identificador de la factura |
| `Subscription_ID` | Suscripción a la que corresponde |
| `Plan_ID` | Plan facturado en ese periodo |
| `Period_Start` | Inicio del periodo facturado |
| `Period_End` | Fin del periodo facturado (fecha de renovación) |
| `Amount` | Monto antes del descuento, en USD |
| `Discount_Amount` | Descuento aplicado, en USD |
| `Status` | `paid`, `failed` (se agotaron los reintentos) o `pending` (reintentos en curso) |

## `Payment_Attempts.csv`

Si un cobro falla, el sistema reintenta hasta 3 veces.

| Columna | Descripción |
|---|---|
| `Attempt_ID` | Identificador del intento |
| `Invoice_ID` | Factura que se intentó cobrar |
| `Attempt_Number` | Número de intento (1 a 4) |
| `Attempted_At` | Fecha del intento |
| `Result` | `succeeded` o `failed` |
| `Failure_Reason` | Código de error si el cobro falló |

## `Events.csv`

Cubre los primeros 45 días de cada usuario desde su registro y las visitas a la página de precios de personas que todavía no se habían registrado. Antes del registro solo hay `Anonymous_ID`; el evento `signup_completed` trae `Anonymous_ID` y `Customer_ID`.

| Columna | Descripción |
|---|---|
| `Event_ID` | Identificador del evento |
| `Event_Time` | Fecha y hora del evento, en UTC |
| `Event_Name` | Tipo de acción (ver tabla de eventos) |
| `Customer_ID` | Cliente que hizo la acción |
| `Anonymous_ID` | Identificador del visitante antes de registrarse |
| `Session_ID` | Sesión de uso a la que pertenece el evento |
| `Platform` | Plataforma: web, iOS, Android o escritorio |
| `App_Version` | Versión de la app |

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
