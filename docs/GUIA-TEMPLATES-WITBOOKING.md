# Guia Completa: Templates de Email para WitBooking - Hotel Almirante

> Documento de referencia para crear y mantener plantillas de email personalizadas
> para el motor de reservas WitBooking del Hotel Almirante de Cartagena.
>
> Basado en las lecciones aprendidas durante el desarrollo de los templates
> de confirmacion de reserva (marzo 2026).

---

## Indice

1. [Estructura del proyecto](#1-estructura-del-proyecto)
2. [Arquitectura del template HTML](#2-arquitectura-del-template-html)
3. [Seccion Pocardy (CRITICO)](#3-seccion-pocardy-critico---lecciones-aprendidas)
4. [Media queries](#4-media-queries)
5. [Codificacion de caracteres (CRITICO)](#5-codificacion-de-caracteres-critico)
6. [Hosting de imagenes](#6-hosting-de-imagenes)
7. [Tags Mustache de WitBooking](#7-tags-mustache-de-witbooking)
8. [Links del proyecto](#8-links-del-proyecto)
9. [Proceso para crear nueva variante](#9-proceso-para-crear-nueva-variante)
10. [Comando sed para convertir caracteres](#10-comando-sed-para-convertir-caracteres)
11. [Limites de WitBooking](#11-limites-de-witbooking)

---

## 1. Estructura del proyecto

### Arbol de directorios

```
HA/
├── assets/
│   └── branding/                  # Logos e imagenes originales
│       ├── HOTEL ALMIRANTE-_Marca.png
│       ├── HeaderHA.png
│       ├── pocardy-restaurante..jpg
│       ├── RESTAURANTE POCARDY-_Marca copia.png
│       └── TRANS logos hotel almirante BN_Marca copia.png
├── docs/
│   ├── branding-guidelines.md     # Guia de marca
│   ├── witbooking-tags.md         # Referencia de tags Mustache
│   ├── mustache-sintaxis.md       # Sintaxis Mustache general
│   ├── email-pms-real.md          # Ejemplo de email real del PMS
│   └── GUIA-TEMPLATES-WITBOOKING.md  # ESTE DOCUMENTO
└── templates/
    └── emails/
        └── confirmacion/
            ├── template-confirmacion-ES.html   # Template español
            ├── template-confirmacion-EN.html   # Template ingles
            ├── template-confirmacion-FR.html   # Template frances
            ├── preview-confirmacion.html       # Preview con datos de prueba
            ├── preview-para-enviar.html        # Preview para pruebas de envio
            ├── preview-para-gmail.html         # Preview optimizado para Gmail
            └── template-predefinido-witbooking.html  # Template original de WitBooking
```

### Convencion de nombres

- **Templates finales:** `template-{tipo}-{IDIOMA}.html`
  - Ejemplos: `template-confirmacion-ES.html`, `template-cancelacion-EN.html`
- **Previews:** `preview-{tipo}.html` (con datos de prueba hardcodeados)
- **Idiomas:** ES (español), EN (ingles), FR (frances)
- **Tipos de email posibles:** confirmacion, cancelacion, modificacion, pre-llegada

---

## 2. Arquitectura del template HTML

### Estructura base obligatoria

Todo template debe comenzar asi:

```html
<html>
<head>
<meta charset="UTF-8">
<style>
  /* Estilos globales y media queries aqui */
</style>
</head>
<body style="margin: 0; padding: 0; background-color: #F5F5F5;">
  <!-- Contenido del email -->
</body>
</html>
```

**Importante:** NO usar `<!DOCTYPE html>` ya que algunos clientes de email lo procesan
de forma inconsistente. Comenzar directamente con `<html>`.

### Layout con tablas

Todo el layout se construye con tablas (`<table role="presentation">`). NUNCA usar
`<div>` para estructura. Los `<div>` solo se usan dentro de celdas para contenido menor.

```html
<!-- Contenedor principal centrado -->
<table role="presentation" width="100%" cellspacing="0" cellpadding="0" border="0">
<tr>
<td align="center" style="padding: 20px 10px;">

  <!-- Email container con ancho maximo -->
  <table role="presentation" class="email-container"
         width="600" cellspacing="0" cellpadding="0" border="0"
         style="max-width: 600px; width: 100%; background-color: #ffffff;">
    <!-- Secciones del email aqui -->
  </table>

</td>
</tr>
</table>
```

### Dimensiones clave

| Elemento | Valor |
|----------|-------|
| Ancho maximo del email | 600px |
| Background exterior | #F5F5F5 |
| Background del email | #ffffff |
| Padding lateral desktop | 30px - 40px |
| Padding lateral mobile | 10px - 15px |

### Colores de marca

| Color | Hex | Uso |
|-------|-----|-----|
| Negro | `#161615` | Texto principal, fondos oscuros, header |
| Verde | `#005C5D` | Acento secundario |
| Azul | `#00758D` | Links, botones |
| Dorado | `#B99E27` | Lineas decorativas, acentos |
| Gris fondo | `#F5F5F5` | Background exterior del email |
| Gris texto | `#666666` | Texto secundario |

### Tipografias

```css
/* Cuerpo de texto */
font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;

/* Titulos decorativos (tipo de habitacion, nombre Pocardy, etc.) */
font-family: Georgia, 'Times New Roman', serif;
```

### Secciones del email (orden)

1. **Header** - Logo del hotel centrado sobre fondo negro
2. **Foto header** - Imagen del hotel a ancho completo
3. **Titulo principal** - Icono + texto de confirmacion
4. **Datos de la reserva** - Tabla con detalles de la estancia
5. **Resumen economico** - Precios, pagos, pendiente
6. **Politica de cancelacion** - Condicional, viene en HTML del PMS
7. **Boton de accion** - "Gestionar reserva" o similar
8. **Separador dorado** - Linea de 50px en color dorado
9. **Seccion Pocardy** - Promocion del restaurante (2 columnas)
10. **Footer** - Logo blanco + redes sociales + legal

---

## 3. Seccion Pocardy (CRITICO - Lecciones aprendidas)

Esta seccion fue la mas problematica del desarrollo. Aqui se documentan
las soluciones finales despues de multiples iteraciones.

### El problema

Se necesita una seccion con 2 columnas: foto del restaurante a la izquierda
y contenido (logo + texto + boton) a la derecha, sobre fondo negro.

### Lo que NO funciona

> **NUNCA usar `display:block` en media queries para convertir columnas en filas.**
> Mobile Safari ignora completamente `display:block !important` en elementos `<td>`.
> Los `<td>` mantienen su comportamiento de celda de tabla sin importar el CSS.

Intentos fallidos:
- `display: block !important` en `<td>` - ignorado por Mobile Safari
- `width: 100% !important` en `<td>` - no colapsa columnas en Mobile Safari
- Usar `<div>` con `display: inline-block` - inconsistente entre clientes

### La solucion final: mantener 2 columnas siempre

En vez de intentar convertir a 1 columna en mobile, **mantener siempre el layout
de 2 columnas** y ajustar proporciones:

- **Desktop:** foto 46% / contenido 54%
- **Mobile:** foto 40% / contenido 60%

### Estructura HTML de Pocardy

```html
<!-- SECCION POCARDY -->
<tr>
<td style="padding: 0 20px 30px 20px;" class="pocardy-wrapper">
<table role="presentation" width="100%" cellspacing="0" cellpadding="0" border="0"
       style="background-color: #161615; border-radius: 6px; overflow: hidden;">
<tr>
  <!-- COLUMNA FOTO -->
  <td width="46%" valign="top" class="pocardy-foto"
      style="width: 46%; padding: 0; margin: 0; line-height: 0; font-size: 0;">
    <img src="URL_FOTO_POCARDY"
         alt="Restaurante Pocardy"
         width="276"
         style="display: block; width: 100%; height: auto; max-height: 280px; object-fit: cover; object-position: center top;"
    />
  </td>

  <!-- COLUMNA CONTENIDO -->
  <td width="54%" valign="middle" class="pocardy-content"
      style="padding: 25px 20px; text-align: center;">
    <!-- Logo Pocardy -->
    <img src="URL_LOGO_POCARDY_BLANCO" alt="Restaurante Pocardy"
         width="160" style="display: block; margin: 0 auto 12px auto; width: 160px; max-width: 100%;" />

    <!-- Texto descriptivo -->
    <p style="font-family: Georgia, 'Times New Roman', serif; font-size: 11px; ...">
      Texto promocional...
    </p>

    <!-- Boton de reserva -->
    <a href="https://pocardy.com/#reserva"
       style="display: inline-block; font-family: 'Helvetica Neue', ...; font-size: 9px; ...">
      RESERVAR MESA
    </a>
  </td>
</tr>
</table>
</td>
</tr>
```

### Reglas criticas para la foto de Pocardy

1. **Inline style de la imagen:**
   ```
   height: auto; max-height: 280px;
   ```
   NUNCA poner `height: 100%` en el inline style. Los clientes de email que no
   soportan media queries (como Outlook) calcularian una altura enorme (3x).

2. **En el media query** se aplica `position: absolute` con `height: 100%` para
   que la foto llene toda la celda. Esto SOLO se activa en clientes que soportan
   media queries (iOS Mail, Gmail app, etc.).

3. La celda padre (`.pocardy-foto`) debe tener `position: relative` en el media query.

---

## 4. Media queries

### Breakpoint unico

```css
@media only screen and (max-width: 620px) {
  /* ... */
}
```

Se usa 620px porque el email tiene 600px de ancho + 10px de padding a cada lado.

### Media query completo

```css
@media only screen and (max-width: 620px) {
  /* Contenedor principal */
  .email-container {
    width: 100% !important;
    min-width: 0 !important;
  }

  /* Eliminar padding lateral del body */
  .email-padding {
    padding: 0 !important;
  }

  /* Seccion Pocardy - contenedor */
  .pocardy-wrapper {
    padding: 0 10px !important;
  }

  /* Pocardy - celda de la foto */
  .pocardy-foto {
    width: 40% !important;
    position: relative !important;
  }

  /* Pocardy - imagen: position absolute para llenar celda */
  .pocardy-foto img {
    position: absolute !important;
    top: 0 !important;
    left: 0 !important;
    width: 100% !important;
    height: 100% !important;
    object-fit: cover !important;
    object-position: center top !important;
  }

  /* Pocardy - contenido: reducir padding */
  .pocardy-content {
    padding: 10px 10px !important;
  }

  /* Pocardy - logo mas pequeño */
  .pocardy-content img {
    max-width: 110px !important;
  }

  /* Pocardy - texto mas pequeño */
  .pocardy-content p {
    font-size: 9px !important;
  }

  /* Pocardy - boton mas pequeño */
  .pocardy-content a {
    font-size: 8px !important;
    letter-spacing: 1px !important;
  }
}
```

### Clientes de email y soporte de media queries

| Cliente | Soporta media queries | Notas |
|---------|----------------------|-------|
| Apple Mail (iOS/macOS) | Si | Referencia principal |
| Gmail App (iOS/Android) | Si | |
| Gmail Web | Parcial | Soporta media queries pero con prefijo |
| Outlook 365 Web | Si | |
| Outlook Desktop (Windows) | NO | Usa Word como motor de renderizado |
| Yahoo Mail | Si | |

**Regla:** El template debe verse aceptable SIN media queries (para Outlook).
Los media queries solo MEJORAN la experiencia en clientes que los soportan.

---

## 5. Codificacion de caracteres (CRITICO)

### Por que es necesario

WitBooking procesa los templates y en algunos contextos **ignora la declaracion
`<meta charset="UTF-8">`**. Ademas, muchos clientes de email (especialmente los
de escritorio) no manejan UTF-8 correctamente.

**Solucion:** Convertir TODOS los caracteres especiales a entidades HTML antes
de pegar el template en WitBooking.

### Tabla completa de conversiones

#### Español

| Caracter | Entidad HTML | Descripcion |
|----------|-------------|-------------|
| a con acento | `&aacute;` | a minuscula con acento agudo |
| e con acento | `&eacute;` | e minuscula con acento agudo |
| i con acento | `&iacute;` | i minuscula con acento agudo |
| o con acento | `&oacute;` | o minuscula con acento agudo |
| u con acento | `&uacute;` | u minuscula con acento agudo |
| enie | `&ntilde;` | ene con tilde |
| u con dieresis | `&uuml;` | u con dieresis |
| A con acento | `&Aacute;` | A mayuscula con acento agudo |
| E con acento | `&Eacute;` | E mayuscula con acento agudo |
| I con acento | `&Iacute;` | I mayuscula con acento agudo |
| O con acento | `&Oacute;` | O mayuscula con acento agudo |
| U con acento | `&Uacute;` | U mayuscula con acento agudo |
| Enie mayuscula | `&Ntilde;` | N mayuscula con tilde |
| apertura interrogacion | `&iquest;` | Signo de interrogacion invertido |
| apertura exclamacion | `&iexcl;` | Signo de exclamacion invertido |

#### Frances

| Caracter | Entidad HTML | Descripcion |
|----------|-------------|-------------|
| a grave | `&agrave;` | a con acento grave |
| e grave | `&egrave;` | e con acento grave |
| e circunflejo | `&ecirc;` | e con circunflejo |
| i circunflejo | `&icirc;` | i con circunflejo |
| o circunflejo | `&ocirc;` | o con circunflejo |
| u grave | `&ugrave;` | u con acento grave |
| u circunflejo | `&ucirc;` | u con circunflejo |
| c cedilla | `&ccedil;` | c con cedilla |

#### Simbolos

| Caracter | Entidad HTML | Descripcion |
|----------|-------------|-------------|
| guion medio | `&ndash;` | En dash |
| guion largo | `&mdash;` | Em dash |
| punto medio | `&middot;` | Middle dot (separadores) |
| euro | `&euro;` | Signo de euro |
| grados | `&deg;` | Signo de grados |

### Regla critica sobre comillas

> **NUNCA convertir las comillas que forman parte de atributos HTML.**
>
> Las comillas simples (`'`) y dobles (`"`) dentro de `style="..."`, `href="..."`,
> `alt="..."`, etc. NO se deben tocar. Solo se convierten los caracteres
> en el **contenido visible** del email (texto entre tags).

### Verificacion post-conversion

Despues de convertir, verificar que:
1. Todos los `style="..."` siguen intactos
2. Todos los `href="..."` siguen intactos
3. Los tags Mustache `{{...}}` no fueron alterados
4. No quedan caracteres UTF-8 en el contenido visible (buscar con grep)

---

## 6. Hosting de imagenes

### Servicio: imgbb.com

Se usa imgbb.com para hospedar las imagenes del template.

> **IMPORTANTE:** Usar imgbb CON CUENTA registrada. Las imagenes subidas sin
> cuenta (como invitado) se eliminan automaticamente despues de un tiempo.

### URL correcta vs incorrecta

```
CORRECTO (URL directa a la imagen):
https://i.ibb.co/m5Pm1Q1L/HOTEL-ALMIRANTE-Marca.png

INCORRECTO (URL del visor de imgbb):
https://ibb.co/m5Pm1Q1L
```

Siempre usar la URL que comienza con `https://i.ibb.co/` que apunta directamente
al archivo de imagen.

### Imagenes del proyecto

| # | Imagen | Archivo original | URL imgbb |
|---|--------|-----------------|-----------|
| 1 | Logo hotel (header) | `HOTEL ALMIRANTE-_Marca.png` | `https://i.ibb.co/m5Pm1Q1L/HOTEL-ALMIRANTE-Marca.png` |
| 2 | Foto header | `HeaderHA.png` | `https://i.ibb.co/YTfCVPC4/Header-HA.png` |
| 3 | Foto Pocardy | `pocardy-restaurante..jpg` | `https://i.ibb.co/C5RZd855/pocardy-restaurante.jpg` |
| 4 | Logo Pocardy (blanco) | `RESTAURANTE POCARDY-_Marca copia.png` | `https://i.ibb.co/8DzpB4gW/RESTAURANTE-POCARDY-Marca-copia.png` |
| 5 | Logo footer (blanco) | `TRANS logos hotel almirante BN_Marca copia.png` | `https://i.ibb.co/7x4LwrYm/TRANS-logos-hotel-almirante-BN-Marca-copia.png` |

### Archivos originales

Los archivos originales de las imagenes estan en:
```
/Users/felipemartinez/HA/assets/branding/
```

---

## 7. Tags Mustache de WitBooking

WitBooking usa el sistema de templates Mustache para inyectar los datos de la reserva.

### Sintaxis basica

| Sintaxis | Funcion | Ejemplo |
|----------|---------|---------|
| `{{variable}}` | Variable simple (texto escapado) | `{{customer.name}}` |
| `{{{variable}}}` | Variable HTML (sin escapar) | `{{{conditionInfo.cancellation}}}` |
| `{{#section}}...{{/section}}` | Seccion condicional | `{{#reservationStatus.isConfirmed}}...{{/reservationStatus.isConfirmed}}` |

### Tags principales por categoria

#### Datos del huesped

| Tag | Descripcion |
|-----|-------------|
| `{{customer.name}}` | Nombre completo del huesped |

#### Datos de la reserva

| Tag | Descripcion |
|-----|-------------|
| `{{roomStayId}}` | Numero de localizador / codigo de reserva |
| `{{startDate}}` | Fecha de llegada (check-in) |
| `{{endDate}}` | Fecha de salida (check-out) |
| `{{stayNights}}` | Numero de noches |
| `{{accommodationInfo.name}}` | Nombre/tipo de la habitacion |
| `{{numberOfOccupants}}` | Numero de huespedes |
| `{{mealPlan}}` | Regimen alimenticio (AD, MP, PC, SA) |

#### Datos economicos

| Tag | Descripcion |
|-----|-------------|
| `{{totalAmount}}` | Importe total de la reserva |
| `{{currency}}` | Moneda (EUR, USD, etc.) |
| `{{guaranteeAmount}}` | Pago a cuenta / cargo anticipado |
| `{{pendingAmount}}` | Importe pendiente de pago |

#### Contenido HTML

| Tag | Descripcion |
|-----|-------------|
| `{{{conditionInfo.cancellation}}}` | Politica de cancelacion (viene en HTML) |

**Nota:** Usar triple llave `{{{ }}}` para que el HTML no sea escapado.

#### URLs y acciones

| Tag | Descripcion |
|-----|-------------|
| `{{cancelUrl}}` | URL para gestionar/cancelar la reserva |

#### Secciones condicionales

```html
<!-- Solo se muestra si la reserva esta confirmada -->
{{#reservationStatus.isConfirmed}}
  <p>Su reserva ha sido confirmada.</p>
{{/reservationStatus.isConfirmed}}

<!-- Solo se muestra si la reserva esta cancelada -->
{{#reservationStatus.isCanceled}}
  <p>Su reserva ha sido cancelada.</p>
{{/reservationStatus.isCanceled}}
```

### Uso en el template

Ejemplo de como se usan los tags en contexto:

```html
<td style="font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; font-size: 14px; color: #333333;">
  <strong>Localizador:</strong> {{roomStayId}}<br>
  <strong>Llegada:</strong> {{startDate}}<br>
  <strong>Salida:</strong> {{endDate}}<br>
  <strong>Noches:</strong> {{stayNights}}<br>
  <strong>Habitaci&oacute;n:</strong> {{accommodationInfo.name}}<br>
  <strong>Hu&eacute;spedes:</strong> {{numberOfOccupants}}<br>
  <strong>R&eacute;gimen:</strong> {{mealPlan}}
</td>
```

**Nota:** Los tags Mustache no necesitan conversion de caracteres. Se dejan tal cual.

---

## 8. Links del proyecto

### URLs principales

| Destino | URL |
|---------|-----|
| Web del hotel | `https://www.hotelalmirante.com` |
| Instagram del hotel | `https://www.instagram.com/hotel_almirante/` |
| Reservas Pocardy | `https://pocardy.com/#reserva` |
| Politica de privacidad | `https://www.hotelalmirante.com/politica-de-privacidad/` |

### Uso en templates

Todos los links deben tener `target="_blank"` para abrir en nueva ventana:

```html
<a href="https://www.hotelalmirante.com" target="_blank"
   style="color: #00758D; text-decoration: underline;">
  www.hotelalmirante.com
</a>
```

### Verificacion de links

Antes de entregar un template, verificar que:
- NO quedan `href="#"` como placeholder
- Todos los links son URLs absolutas (empiezan con `https://`)
- El link de `{{cancelUrl}}` se mantiene como tag Mustache

---

## 9. Proceso para crear nueva variante

### Paso a paso

#### 1. Copiar template base

Usar el template ES de confirmacion como punto de partida:

```bash
cp templates/emails/confirmacion/template-confirmacion-ES.html \
   templates/emails/{nuevo-tipo}/template-{nuevo-tipo}-ES.html
```

#### 2. Modificar contenido y secciones

Segun el tipo de email:
- **Cancelacion:** Cambiar icono, titulo, eliminar boton de gestionar, añadir mensaje de cancelacion
- **Modificacion:** Cambiar titulo, mostrar datos antiguos vs nuevos
- **Pre-llegada:** Añadir informacion practica (direccion, parking, check-in)

#### 3. Crear versiones en otros idiomas

```bash
cp template-{tipo}-ES.html template-{tipo}-EN.html
cp template-{tipo}-ES.html template-{tipo}-FR.html
```

Traducir TODO el contenido visible (no tocar los tags Mustache ni los atributos HTML).

#### 4. Convertir caracteres especiales

Ejecutar el script sed (ver seccion 10) sobre los tres archivos.

#### 5. Lista de verificacion final

- [ ] No quedan `href="#"` placeholder
- [ ] Todos los caracteres especiales estan como entidades HTML
- [ ] Los tags Mustache `{{...}}` estan intactos
- [ ] Los atributos `style="..."` no fueron alterados por el sed
- [ ] Las URLs de imagenes apuntan a imgbb (no locales)
- [ ] El archivo pesa menos de 64KB
- [ ] Las secciones condicionales Mustache son correctas para este tipo de email
- [ ] Los tres idiomas (ES, EN, FR) estan completos

#### 6. Verificar tamaño

```bash
ls -la templates/emails/{tipo}/template-{tipo}-*.html
# Cada archivo debe ser < 64KB (65536 bytes)
# Los templates actuales pesan ~24KB cada uno
```

#### 7. Copiar y pegar en WitBooking

```bash
# Copiar template al portapapeles
cat templates/emails/{tipo}/template-{tipo}-ES.html | pbcopy
```

Luego:
1. Ir al panel de WitBooking
2. Ir a la configuracion de plantillas de email
3. Seleccionar el idioma correspondiente
4. Pegar el contenido completo en el editor de plantilla custom
5. Guardar
6. Repetir para EN y FR

---

## 10. Comando sed para convertir caracteres

### Script completo

Ejecutar desde el directorio donde estan los templates:

```bash
for f in template-*.html; do
  # Vocales minusculas con acento (espanol)
  sed -i '' 's/á/\&aacute;/g' "$f"
  sed -i '' 's/é/\&eacute;/g' "$f"
  sed -i '' 's/í/\&iacute;/g' "$f"
  sed -i '' 's/ó/\&oacute;/g' "$f"
  sed -i '' 's/ú/\&uacute;/g' "$f"
  sed -i '' 's/ñ/\&ntilde;/g' "$f"
  sed -i '' 's/ü/\&uuml;/g' "$f"

  # Vocales mayusculas con acento
  sed -i '' 's/Á/\&Aacute;/g' "$f"
  sed -i '' 's/É/\&Eacute;/g' "$f"
  sed -i '' 's/Í/\&Iacute;/g' "$f"
  sed -i '' 's/Ó/\&Oacute;/g' "$f"
  sed -i '' 's/Ú/\&Uacute;/g' "$f"
  sed -i '' 's/Ñ/\&Ntilde;/g' "$f"

  # Signos de puntuacion invertidos
  sed -i '' 's/¿/\&iquest;/g' "$f"
  sed -i '' 's/¡/\&iexcl;/g' "$f"

  # Guiones y puntos
  sed -i '' 's/–/\&ndash;/g' "$f"
  sed -i '' 's/—/\&mdash;/g' "$f"
  sed -i '' 's/·/\&middot;/g' "$f"

  # Simbolos
  sed -i '' 's/€/\&euro;/g' "$f"
  sed -i '' 's/°/\&deg;/g' "$f"

  # Caracteres franceses
  sed -i '' 's/à/\&agrave;/g' "$f"
  sed -i '' 's/è/\&egrave;/g' "$f"
  sed -i '' 's/ê/\&ecirc;/g' "$f"
  sed -i '' 's/î/\&icirc;/g' "$f"
  sed -i '' 's/ô/\&ocirc;/g' "$f"
  sed -i '' 's/ù/\&ugrave;/g' "$f"
  sed -i '' 's/û/\&ucirc;/g' "$f"
  sed -i '' 's/ç/\&ccedil;/g' "$f"
done
```

### Advertencia critica

> **NUNCA ejecutar conversion sobre comillas.**
>
> Las comillas simples (`'`), dobles (`"`), tipograficas (`'`, `'`, `"`, `"`)
> que aparezcan dentro de atributos HTML como `style="..."` o
> `font-family: 'Helvetica Neue'` NO deben ser convertidas.
>
> El script sed de arriba esta diseñado para NO tocar comillas.
> Si se necesita convertir comillas tipograficas en el texto visible,
> hacerlo manualmente con mucho cuidado de no alterar los atributos.

### Verificacion post-sed

```bash
# Verificar que no quedan caracteres UTF-8 en el contenido
# (este grep deberia devolver 0 resultados si todo fue convertido)
grep -P '[áéíóúñüÁÉÍÓÚÑàèêîôùûç¿¡–—·€°]' template-*.html
```

---

## 11. Limites de WitBooking

### Limites tecnicos

| Parametro | Limite | Notas |
|-----------|--------|-------|
| Tamaño maximo del template | 64 KB | Los actuales pesan ~24KB |
| Soporte `<style>` en `<head>` | Si | Funciona correctamente |
| Soporte media queries | Si* | *Depende del cliente de email final |
| Soporte Mustache triple llave | Si | Para contenido HTML como politica de cancelacion |
| Numero de idiomas | 3 | ES, EN, FR (configurable por idioma) |

### Comportamiento del motor de templates

- WitBooking procesa el template con Mustache antes de enviarlo
- Los tags `{{variable}}` se reemplazan con datos de la reserva
- Las secciones `{{#section}}...{{/section}}` se evaluan como condicionales
- El HTML resultante se envia tal cual al cliente de email
- WitBooking **no modifica** el CSS ni la estructura HTML

### Cosas que WitBooking NO hace

- No minifica el HTML
- No inline los estilos del `<style>` (hay que confiar en el soporte del cliente)
- No optimiza imagenes
- No añade tracking pixels (o si los añade, son transparentes)
- No modifica los links (excepto posiblemente `{{cancelUrl}}`)

---

## Apendice A: Estructura HTML de referencia rapida

```
<html>
<head>
  <meta charset="UTF-8">
  <style>
    @media only screen and (max-width: 620px) { ... }
  </style>
</head>
<body bgcolor="#F5F5F5">
  <table width="100%">           <!-- Wrapper exterior -->
    <tr><td align="center">
      <table class="email-container" width="600">  <!-- Email 600px -->

        <!-- 1. HEADER: Logo sobre negro -->
        <tr><td bgcolor="#161615">...</td></tr>

        <!-- 2. FOTO HEADER -->
        <tr><td><img src="HeaderHA.png" /></td></tr>

        <!-- 3. TITULO -->
        <tr><td>Confirmacion / Cancelacion / etc.</td></tr>

        <!-- 4. DATOS RESERVA -->
        <tr><td>
          {{roomStayId}}, {{startDate}}, etc.
        </td></tr>

        <!-- 5. RESUMEN ECONOMICO -->
        <tr><td>
          {{totalAmount}} {{currency}}
        </td></tr>

        <!-- 6. POLITICA CANCELACION (condicional) -->
        <tr><td>
          {{{conditionInfo.cancellation}}}
        </td></tr>

        <!-- 7. BOTON ACCION -->
        <tr><td>
          <a href="{{cancelUrl}}">GESTIONAR RESERVA</a>
        </td></tr>

        <!-- 8. SEPARADOR DORADO -->
        <tr><td>
          <div style="border-top: 2px solid #B99E27; width: 50px;"></div>
        </td></tr>

        <!-- 9. SECCION POCARDY (2 columnas) -->
        <tr><td>
          <table bgcolor="#161615">
            <tr>
              <td width="46%" class="pocardy-foto">foto</td>
              <td width="54%" class="pocardy-content">contenido</td>
            </tr>
          </table>
        </td></tr>

        <!-- 10. FOOTER -->
        <tr><td bgcolor="#161615">
          logo blanco + redes + legal
        </td></tr>

      </table>
    </td></tr>
  </table>
</body>
</html>
```

---

## Apendice B: Datos de prueba para previews

Para crear archivos de preview con datos hardcodeados:

```
Nombre: Juan P&eacute;rez Garc&iacute;a
Localizador: WB-2026-001234
Llegada: 15/07/2026
Salida: 18/07/2026
Noches: 3
Habitacion: Suite Almirante con vistas al mar
Huespedes: 2
Regimen: Alojamiento y Desayuno
Total: 450,00 EUR
Pago a cuenta: 450,00 EUR
Pendiente: 0,00 EUR
```

---

*Documento creado en marzo 2026. Actualizar segun se descubran nuevas
particularidades del sistema WitBooking o de los clientes de email.*
