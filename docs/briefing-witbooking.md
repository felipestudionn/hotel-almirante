# Briefing de Personalización WitBooking - Hotel Almirante

Playa de San Juan - Alicante - Costa Blanca | Febrero 2026

---

## 1. Colores para el Motor de Reservas

| Elemento del motor | Color HEX | Referencia |
|---|---|---|
| Color principal (botones, enlaces) | `#005C5D` | PANTONE 7721C |
| Color secundario (acentos, hover) | `#00758D` | PANTONE 3145C |
| Color de acento (destacados, CTAs) | `#B99E27` | PANTONE 117C |
| Texto principal | `#161615` | Negro corporativo |
| Texto secundario | `#666666` | Gris medio |
| Fondo general | `#FFFFFF` | Blanco |
| Fondo bloques info | `#F5F5F5` | Gris claro |

## 2. Logos Necesarios para WitBooking

| Versión | Dimensiones | Formato | Uso |
|---|---|---|---|
| Logo principal | 300 x 80 px | PNG transp. | Cabecera del motor (header) |
| Logo cuadrado | 200 x 200 px | PNG transp. | Favicon / icono pestaña |
| Logo email | 250 x 70 px | PNG transp. | Cabecera email confirmación |
| Logo negativo | 300 x 80 px | PNG transp. | Fondos oscuros (footer, banners) |
| Logo con tagline | 400 x 100 px | PNG transp. | Display / banners promocionales |

**Especificaciones:**
- PNG con fondo transparente (obligatorio)
- Mínimo 72 ppi, recomendado 144 ppi (retina)
- Peso máximo: 100 KB por archivo
- Subida en: **Configuración > Personalización > Logo**

### Tipografías alternativas web-safe
- Catamaran Thin → **Montserrat Light** o **Raleway Light**
- Trajan Pro → **Playfair Display** o **Cormorant Garamond**

## 3. Email de Confirmación de Reserva

### Asunto
`Tu reserva en Hotel Almirante está confirmada`

### Preheader
`El Mediterráneo te espera. Aquí tienes los detalles de tu estancia.`

### Estructura

1. **Header**: Logo Hotel Almirante
2. **Titular**: "Tu reserva está confirmada"
3. **Saludo**: Hola {{customer.name}},
4. **Intro**: Gracias por elegir Hotel Almirante. El Mediterráneo te espera.
5. **Bloque reserva**:
   - N.º de reserva: {{roomStayId}}
   - Check-in: {{startDate}} · desde las 14:00
   - Check-out: {{endDate}} · hasta las 12:00
   - Habitación: {{accommodationInfo.name}}
   - Régimen: {{mealPlan}}
   - Total: {{totalAmount}} {{currency}}
6. **CTA**: "Gestionar mi reserva"
7. **Info práctica**:
   - Dirección: Playa de San Juan, Alicante
   - Cómo llegar: A 10 min del centro · 20 min del aeropuerto
   - Parking: Disponible para huéspedes
   - Contacto: reservas@hotelalmirante.com · 965 225 661
8. **Footer**:
   - Hotel Almirante · Playa de San Juan · Alicante
   - "El único hotel con acceso directo a la playa"
   - @hotel_almirante · hotelalmirante.com

### Mapeo de etiquetas briefing → WitBooking Mustache

| Briefing | Etiqueta Mustache |
|---|---|
| {nombre_huésped} | `{{customer.name}}` |
| {numero_reserva} | `{{roomStayId}}` |
| {fecha_checkin} | `{{startDate}}` |
| {fecha_checkout} | `{{endDate}}` |
| {tipo_habitacion} | `{{accommodationInfo.name}}` |
| {regimen} | `{{mealPlan}}` |
| {precio_total} | `{{totalAmount}}` |

## 4. Mensaje Display (Banner Motor de Reservas)

### Opciones de mensaje

**Opción A - Mejor precio garantizado**
> Reserva directa · Mejor precio garantizado · Sin intermediarios
>
> "Reservando aquí te garantizamos el mejor precio disponible. Directo, sin comisiones."

**Opción B - Ventajas exclusivas**
> Solo en nuestra web: late check-out · welcome drink · mejor tarifa
>
> "Reserva directo y disfruta de ventajas que no encontrarás en ningún otro sitio."

**Opción C - Experiencia**
> El único hotel con acceso directo a Playa de San Juan
>
> 6 km de costa · Piscina · Jardines · Restaurante frente al mar

### Especificaciones del banner gráfico

| Propiedad | Valor |
|---|---|
| Dimensiones | 1200 x 300 px (ratio 4:1, responsive) |
| Formato | JPG optimizado (<150 KB) o PNG |
| Colores fondo | `#005C5D` o `#00758D` |
| Color texto titular | `#FFFFFF` (blanco) |
| Color subtítulo | `#B99E27` (dorado) |
| Imagen de fondo | Vista mar/playa con overlay oscuro 40% |
| Logo | Versión negativo (blanco) esquina superior derecha |

### Implementación
- **Ruta**: Configuración > Personalización de apariencia > Gestor Display
- Se puede integrar como imagen o HTML con estilos inline
- Mensajes contextuales: Configuración > Mensajes web / Mensajes destacados

## 5. Checklist de Entregables

- [ ] Logo principal (300x80 px) - PNG transparente
- [ ] Logo cuadrado (200x200 px) - PNG transparente
- [ ] Logo email (250x70 px) - PNG transparente
- [ ] Logo negativo (300x80 px) - PNG transparente
- [ ] Logo con tagline (400x100 px) - PNG transparente
- [ ] Banner Display (1200x300 px) - JPG/PNG
- [ ] Plantilla HTML email confirmación
- [ ] Configuración colores en motor
- [ ] Subida logos a extranet WitBooking
- [ ] Configuración email en extranet
- [ ] Configuración Display en extranet
