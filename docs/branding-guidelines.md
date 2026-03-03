# Hotel Almirante - Guía de Identidad Corporativa

Fuente: Manual de Identidad Corporativa (Paula Alenda - Diseño & Ilustración)

---

## 1. La Marca

Logotipo principal horizontal:
- **HOTEL** (tracking amplio, peso ligero)
- **ALMIRANTE** (tracking amplio, mayor tamaño)
- **PLAYA SAN JUAN** (tracking amplio, peso ligero, menor tamaño)

## 2. Isotipo

Dos variantes del isotipo con la "a" estilizada (serif):

- **Isotipo A**: "a" + "ALMIRANTE" debajo (versión compacta)
- **Isotipo B**: "a" + "HOTEL ALMIRANTE" + "PLAYA SAN JUAN" debajo (versión completa)

## 3. Gama Cromática

### Color principal
| Color | CMYK | RGB | HEX |
|-------|------|-----|-----|
| **Black** | C:0 M:0 Y:0 K:100 | R:22 G:22 B:21 | `#161615` |

### Colores complementarios
| Color | Pantone | CMYK | RGB | HEX |
|-------|---------|------|-----|-----|
| **Verde azulado oscuro** | 7721C | C:93 M:36 Y:54 K:34 | R:0 G:92 B:93 | `#005C5D` |
| **Azul petróleo** | 3145C | C:100 M:24 Y:35 K:14 | R:0 G:117 B:141 | `#00758D` |
| **Dorado/Mostaza** | 117C | C:6 M:18 Y:86 K:31 | R:185 G:158 B:39 | `#B99E27` |

## 4. Tipografías

### Catamaran Thin
- Uso: texto de cuerpo, subtítulos, textos ligeros
- Peso: Thin
- Google Fonts: `Catamaran:wght@100`
- Incluye mayúsculas, minúsculas y números

### Trajan Pro
- Uso: títulos, logotipo "ALMIRANTE", textos destacados
- Peso: Regular
- Solo mayúsculas y versalitas (no tiene minúsculas convencionales)

## 5. Versiones del Logo

| Versión | Uso recomendado |
|---------|----------------|
| **Positivo** (negro sobre blanco) | Uso general, web, emails fondo claro |
| **Negativo** (blanco sobre negro/oscuro) | Fondos oscuros, banners, cabeceras |

## 6. Recursos Complementarios / Estampados

Patrones geométricos en dos paletas:
- **Azul petróleo**: rayas horizontales, chevron, retícula
- **Dorado/Mostaza**: rayas horizontales, chevron, retícula

Uso: fondos decorativos, separadores, elementos gráficos complementarios.

---

## Aplicación en WitBooking

### Para emails (estilos inline)
```css
/* Color principal de texto */
color: #161615;

/* Tipografía principal (fallback web-safe para Catamaran) */
font-family: 'Catamaran', 'Helvetica Neue', Helvetica, Arial, sans-serif;

/* Tipografía títulos (fallback web-safe para Trajan Pro) */
font-family: 'Trajan Pro', 'Times New Roman', Georgia, serif;

/* Colores de acento */
/* Verde azulado: */ color: #005C5D;
/* Azul petróleo: */ color: #00758D;
/* Dorado:        */ color: #B99E27;
```

### Para motor de reservas
- **Color primario** (botones, acción): `#161615` o `#005C5D`
- **Color secundario** (cabecera, formulario): `#00758D`
- **Logo web**: Marca completa horizontal (PNG transparente)
- **Logo móvil**: Isotipo A compacto (PNG transparente)
- **Logo email**: Versión sobre fondo blanco (JPG/PNG)
- **Favicon**: Isotipo "a" solo
