# Motor de Reservas WitBooking - Referencia

## Gestor Display
Permite crear pestañas de información con:
- Videos
- Imágenes / Banners
- Tours virtuales
- Texto

Se configura en **Configuración → Display**.

## Banners HTML
Código base para banners:
```html
<p class="full">
  <img class="center fr-fic fr-dii" src="URL-IMAGEN" border="0" alt="banner" width="100%" height="auto">
</p>
```
- Se insertan desde "Vista de código" (icono tres puntos)
- Se puede aplicar a nivel **Establecimiento** o **Alojamiento** (por ticker)
- Hay que introducir el código en todos los idiomas activos
- Se pueden programar fechas de visualización

## Imágenes con HTML
```html
<img src="URL-IMAGEN" width="100%">
```
- La imagen debe estar alojada en servidor propio o servicio como imgbb.com
- Se insertan desde editores con "Vista de código"

## Logo y Apariencia
Ubicación: **Configuración → Apariencia → Logo**

4 tipos de logo:
1. **Logo** - Principal, se muestra en toda la interfaz
2. **Logo móvil** - Solo en dispositivos móviles
3. **Logo email** - Para emails (recomendado si el logo es blanco)
4. **Favicon** - Icono de pestaña del navegador

Colores personalizables:
- **Color primario** - Botones de acción, desplegables, texto destacado
- **Color secundario** - Fondo de cabecera y formulario de reserva

## Tags / Etiquetas
Etiquetas para destacar características de habitaciones/paquetes:
- Ejemplos: "Vista al mar", "Mascota permitida", "Desayuno incluido"
- Se configuran en **Configuración → Gestión de etiquetas**
- Se pueden importar por Excel (columnas: ticker_tag, name_es, action)
- Máximo 255 caracteres por nombre

## Mensajes Web
Avisos en el frontal del motor. Se configuran en **Comunicación → Mensajes Web**.

Posiciones disponibles:
1. Sobre la lista de alojamientos (paso 1)
2. Bajo "datos personales" (paso 2)
3. Bajo "forma de pago" (paso 2)
4. Modal en selección de alojamientos (paso 1)
5. Modal en checkout (paso 2)
6. Pantalla de confirmación de reserva

Opciones:
- Protección por código promocional
- Período de visualización configurable
- Mostrar solo cuando no hay disponibilidad
