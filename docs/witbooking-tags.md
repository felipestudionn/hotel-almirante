# Etiquetas WitBooking (Mustache)

## Datos de la reserva
- `{{roomStayId}}` - ID de la reserva
- `{{reservationStatus}}` - Estado de la reserva
- `{{startDate}}` - Fecha de entrada
- `{{endDate}}` - Fecha de salida
- `{{creationDate}}` - Fecha de creación
- `{{stayNights}}` - Número de noches

## Datos del cliente
- `{{customer.name}}` - Nombre del huésped
- `{{customer.email}}` - Email del huésped
- `{{customer.phoneNumber}}` - Teléfono del huésped
- `{{customer.tier}}` - Nivel de fidelización

## Datos del hotel
- `{{property.name}}` - Nombre del hotel
- `{{property.address}}` - Dirección
- `{{property.phoneNumber}}` - Teléfono del hotel

## Habitación y servicios
- `{{accommodationInfo.name}}` - Tipo de habitación
- `{{services}}` - Array de servicios extra (transfers, etc.)
- `{{mealPlan}}` - Régimen alimenticio

## Imágenes
- `{{accommodationInfo.media}}` - Array de imágenes con `url`, `title`, `description`

## Importes
- `{{baseAmount}}` - Importe base
- `{{totalAmount}}` - Importe total
- `{{currency}}` - Moneda
- `{{guaranteeAmount}}` - Importe de garantía
- `{{pendingAmount}}` - Importe pendiente
- `{{taxations}}` - Array de impuestos (ticker, name, type, typology, amounts)

## Condicionales
- `{{#reservationStatus.isCanceled}}...{{/reservationStatus.isCanceled}}` - Contenido si cancelada
- `{{#reservationStatus.isConfirmed}}...{{/reservationStatus.isConfirmed}}` - Contenido si confirmada

## Políticas
- `{{conditionInfo.cancellation}}` - Política de cancelación (HTML)
- `{{conditionInfo.checkIn}}` - Info de check-in (HTML)
- `{{conditionInfo.checkOut}}` - Info de check-out (HTML)
