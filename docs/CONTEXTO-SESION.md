# Hotel Almirante - Contexto Completo del Proyecto

**Fecha:** 3 marzo 2026
**Proyecto:** Personalización WitBooking + potencial hub de comunicación digital
**Repositorio:** https://github.com/felipestudionn/hotel-almirante.git
**Directorio local:** `/Users/felipemartinez/HA`

---

## 1. QUÉ ES ESTE PROYECTO

Personalización completa del motor de reservas WitBooking para el Hotel Almirante (Playa de San Juan, Alicante). Incluye:
- Plantillas de email (confirmación, pre-estancia, post-estancia)
- Personalización visual del motor de reservas (logos, colores, banners, display)
- Mensajes web dentro del flujo de reserva

Se habló de ampliar el proyecto a **hub de comunicación digital** incluyendo redes sociales (calendario editorial, copies, estrategia), pero queda pendiente de decisión.

---

## 2. ESTADO ACTUAL

### Lo que está hecho:
- ✅ Repo GitHub creado y conectado
- ✅ Estructura de carpetas del proyecto
- ✅ Documentación técnica de WitBooking recopilada y organizada en Markdown
- ✅ Manual de identidad corporativa analizado y documentado
- ✅ Briefing de trabajo analizado y documentado
- ✅ Assets de branding (logos en múltiples formatos) subidos al repo
- ✅ Todo commiteado y pusheado (3 commits en main)

### Lo que está pendiente:
- ⏳ **Referencias visuales** - Esperando a que pasen las referencias de diseño para las plantillas
- ⏳ **Redimensionar logos** a las 5 versiones que pide WitBooking (300x80, 200x200, 250x70, 300x80 negativo, 400x100)
- ⏳ **Crear plantilla HTML** del email de confirmación con etiquetas Mustache
- ⏳ **Crear banner Display** (1200x300 px)
- ⏳ **Configurar colores** en la extranet de WitBooking
- ⏳ Decisión sobre integrar redes sociales en este proyecto

---

## 3. ESTRUCTURA DEL REPOSITORIO

```
HA/
├── .gitignore                    # Excluye .DS_Store
├── README.md                     # Overview del proyecto
├── assets/
│   ├── branding/                 # Logos originales (PNG, JPG, TIF, PDF, ZIP)
│   │   ├── HOTEL ALMIRANTE-_Marca.png           # Marca completa horizontal
│   │   ├── HOTEL ALMIRANTE-_Isotipo.png         # "a" + Hotel Almirante + Playa San Juan
│   │   ├── HOTEL ALMIRANTE-_Isotipo B.png       # "a" + Almirante (compacto)
│   │   ├── TRANS logos hotel almirante BN_*.png  # Versiones fondo transparente
│   │   ├── Almirante negro_fondoblanco (002).jpg # Versión fondo blanco
│   │   └── ... (+ versiones TIF y ZIP)
│   └── images/                   # Imágenes para plantillas (vacío)
├── docs/
│   ├── HOTEL ALMIRANTE-manual de identidad corporativa (1).pdf  # Manual original
│   ├── briefing_witbooking_hotel_almirante.docx                 # Briefing original
│   ├── branding-guidelines.md    # Guía de identidad (Markdown)
│   ├── briefing-witbooking.md    # Briefing completo (Markdown)
│   ├── witbooking-tags.md        # Etiquetas Mustache para emails
│   ├── witbooking-motor.md       # Referencia del motor de reservas
│   └── mustache-sintaxis.md      # Sintaxis completa de Mustache
└── templates/
    ├── emails/
    │   ├── confirmacion/         # (vacío - pendiente de referencias)
    │   ├── pre-estancia/         # (vacío)
    │   └── post-estancia/        # (vacío)
    └── motor-reservas/           # (vacío)
```

---

## 4. IDENTIDAD DE MARCA - RESUMEN RÁPIDO

### Colores
| Uso | HEX | Nombre |
|---|---|---|
| Texto principal | `#161615` | Negro corporativo |
| Botones/enlaces principal | `#005C5D` | Verde azulado (PANTONE 7721C) |
| Secundario/hover | `#00758D` | Azul petróleo (PANTONE 3145C) |
| Acento/CTAs | `#B99E27` | Dorado (PANTONE 117C) |
| Texto secundario | `#666666` | Gris medio |
| Fondo | `#FFFFFF` | Blanco |
| Fondo bloques | `#F5F5F5` | Gris claro |

### Tipografías
- **Catamaran Thin** → cuerpo de texto (alternativas: Montserrat Light, Raleway Light)
- **Trajan Pro** → títulos y display (alternativas: Playfair Display, Cormorant Garamond)

### Logos disponibles
- Marca completa horizontal: "HOTEL ALMIRANTE - PLAYA SAN JUAN"
- Isotipo completo: "a" estilizada + nombre completo (vertical)
- Isotipo compacto: "a" estilizada + "ALMIRANTE" (vertical)
- Todas las versiones en fondo transparente y BN

---

## 5. SISTEMA WITBOOKING - LO QUE SABEMOS

### Motor de plantillas
- Usa **Mustache** (logic-less templates)
- Plantillas en **HTML con estilos inline**
- Máximo **64 KB** por plantilla
- 3 tipos de email: confirmación, pre-estancia, post-estancia

### Etiquetas Mustache principales
```mustache
{{customer.name}}              <!-- Nombre huésped -->
{{customer.email}}             <!-- Email huésped -->
{{roomStayId}}                 <!-- N.º reserva -->
{{startDate}} / {{endDate}}    <!-- Fechas entrada/salida -->
{{stayNights}}                 <!-- Noches -->
{{accommodationInfo.name}}     <!-- Tipo habitación -->
{{mealPlan}}                   <!-- Régimen -->
{{totalAmount}} {{currency}}   <!-- Precio total -->
{{baseAmount}}                 <!-- Precio base -->
{{pendingAmount}}              <!-- Pendiente de pago -->
{{guaranteeAmount}}            <!-- Garantía -->

{{! Condicionales }}
{{#reservationStatus.isConfirmed}}...{{/reservationStatus.isConfirmed}}
{{#reservationStatus.isCanceled}}...{{/reservationStatus.isCanceled}}

{{! Listas }}
{{#services}}{{.}}{{/services}}
{{#taxations}}...{{/taxations}}

{{! Políticas (devuelven HTML, usar triple llave) }}
{{{conditionInfo.cancellation}}}
{{{conditionInfo.checkIn}}}
{{{conditionInfo.checkOut}}}

{{! Hotel }}
{{property.name}}
{{property.address}}
{{property.phoneNumber}}
```

### Motor de reservas - Personalización
- **Display**: Pestañas con vídeos, imágenes, banners, tours virtuales (Configuración → Display)
- **Banners HTML**: Con clases `class="full"`, `class="center fr-fic fr-dii"`, responsive
- **Logo**: 4 tipos (principal, móvil, email, favicon) en Configuración → Apariencia → Logo
- **Tags**: Etiquetas de habitaciones (Configuración → Gestión de etiquetas)
- **Mensajes Web**: 6 posiciones en el flujo (Comunicación → Mensajes Web)

### Herramientas de validación
- Mustache: https://pismute.github.io/Try-Dashbars/
- Test emails: https://putsmail.com/tests/new
- Minificar HTML: https://codebeautify.org/minify-html

---

## 6. BRIEFING DEL EMAIL DE CONFIRMACIÓN

### Estructura definida
1. **Header** → Logo
2. **Titular** → "Tu reserva está confirmada"
3. **Saludo** → "Hola {{customer.name}},"
4. **Intro** → "Gracias por elegir Hotel Almirante. El Mediterráneo te espera."
5. **Bloque reserva** → Datos dinámicos (n.º reserva, fechas, habitación, régimen, total)
6. **CTA** → "Gestionar mi reserva"
7. **Info práctica** → Dirección, cómo llegar, parking, contacto
8. **Footer** → Datos del hotel, claim, RRSS

### Copy
- **Asunto**: "Tu reserva en Hotel Almirante está confirmada"
- **Preheader**: "El Mediterráneo te espera. Aquí tienes los detalles de tu estancia."
- **Contacto**: reservas@hotelalmirante.com · 965 225 661
- **Claim**: "El único hotel con acceso directo a la playa"
- **Dirección**: Playa de San Juan, Alicante (10 min centro, 20 min aeropuerto)

---

## 7. BRIEFING DEL DISPLAY / BANNER

### 3 opciones de mensaje (pendiente de elegir):
- **A**: "Reserva directa · Mejor precio garantizado · Sin intermediarios"
- **B**: "Solo en nuestra web: late check-out · welcome drink · mejor tarifa"
- **C**: "El único hotel con acceso directo a Playa de San Juan" + servicios

### Specs del banner
- 1200 x 300 px, ratio 4:1, responsive
- Fondo: `#005C5D` o `#00758D` con foto mar/playa + overlay 40%
- Texto: blanco (`#FFFFFF`) + dorado (`#B99E27`)
- Logo negativo (blanco) esquina superior derecha
- JPG < 150 KB

---

## 8. CONVERSACIÓN SOBRE REDES SOCIALES

Se planteó ampliar este proyecto para incluir gestión de redes sociales del hotel. Conclusiones:

### Lo que Claude Code puede hacer en RRSS:
- Generar copies para posts (captions, hashtags, CTAs)
- Crear calendarios editoriales
- Adaptar mensajes a diferentes plataformas
- Proponer pilares de contenido y estrategia
- Crear guías de tono y voz
- Plantillas HTML para newsletters
- Briefs para creación de contenido visual

### Lo que NO puede hacer:
- Publicar directamente en redes
- Generar imágenes/vídeos
- Analizar métricas en tiempo real

### Modelo de trabajo recomendado:
| Qué | Dónde |
|---|---|
| Generar contenido (copies, calendarios, plantillas) | Claude Code |
| Consultar y gestionar el día a día | Notion (tiene integración) |
| Branding, plantillas WitBooking, código | GitHub (este repo) |
| Iteraciones rápidas | Claude Code o Claude.ai |

**Decisión pendiente** de si se integra en este mismo proyecto o se mantiene separado.

---

## 9. PRÓXIMOS PASOS

1. **Recibir referencias visuales** para las plantillas
2. **Redimensionar logos** a las 5 versiones WitBooking
3. **Crear plantilla HTML** del email de confirmación con Mustache
4. **Crear banner Display** con una de las 3 opciones de mensaje
5. **Configurar colores** en la extranet
6. Decidir sobre redes sociales

---

## 10. DATOS DE CONTACTO DEL HOTEL

- **Hotel Almirante** · Playa de San Juan · Alicante · Costa Blanca
- **Web**: hotelalmirante.com
- **Email**: reservas@hotelalmirante.com
- **Teléfono**: 965 225 661
- **Instagram**: @hotel_almirante
- **Claim**: "El único hotel con acceso directo a la playa"
- 6 km de costa · A 10 min del centro de Alicante · 20 min del aeropuerto
