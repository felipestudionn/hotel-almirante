# Mustache - Sintaxis Completa

Referencia: http://mustache.github.io/mustache.5.html

## Variables

```mustache
{{nombre}}              <!-- Variable con escape HTML -->
{{{nombre}}}            <!-- Variable SIN escape HTML (triple llave) -->
{{&nombre}}             <!-- Variable SIN escape HTML (alternativa) -->
{{cliente.nombre}}      <!-- Propiedad anidada (dot notation) -->
{{.}}                   <!-- Iterador implícito (valor actual del contexto) -->
```

## Secciones (condicionales y bucles)

```mustache
{{#seccion}}
  Este bloque se renderiza si "seccion" es:
  - true / valor no vacío → se renderiza una vez
  - una lista no vacía → se renderiza una vez POR CADA elemento
{{/seccion}}
```

### Ejemplo con lista:
```mustache
{{#habitaciones}}
  <div>{{nombre}} - {{precio}}€</div>
{{/habitaciones}}
```

### Ejemplo condicional:
```mustache
{{#reservationStatus.isConfirmed}}
  <p>Su reserva está confirmada</p>
{{/reservationStatus.isConfirmed}}
```

## Secciones invertidas (negación)

```mustache
{{^seccion}}
  Se renderiza si "seccion" NO existe, es false, o es lista vacía
{{/seccion}}
```

### Ejemplo:
```mustache
{{^services}}
  <p>No hay servicios adicionales</p>
{{/services}}
```

## Comentarios

```mustache
{{! Esto es un comentario, no se renderiza }}
{{!
  Los comentarios pueden
  tener varias líneas
}}
```

## Parciales (incluir otras plantillas)

```mustache
{{> header}}            <!-- Incluye la plantilla "header" -->
{{>*dynamic}}           <!-- Parcial dinámica (nombre en variable) -->
```

## Bloques y herencia

```mustache
{{$titulo}}Título por defecto{{/titulo}}    <!-- Bloque con valor por defecto -->

{{<plantilla_padre}}                         <!-- Hereda de plantilla padre -->
  {{$titulo}}Título personalizado{{/titulo}} <!-- Sobreescribe el bloque -->
{{/plantilla_padre}}
```

## Delimitadores personalizados

```mustache
{{=<% %>=}}         <!-- Cambia delimitadores de {{ }} a <% %> -->
<%nombre%>          <!-- Usa el nuevo delimitador -->
<%={{ }}=%>         <!-- Vuelve a los delimitadores originales -->
```

## Reglas importantes

- **Escape automático**: `{{var}}` escapa HTML. Usar `{{{var}}}` para HTML crudo
- **Búsqueda recursiva**: si no encuentra la clave en el contexto actual, busca en el padre
- **Falsy values**: `false`, `null`, `undefined`, `0`, `""` y listas vacías NO renderizan secciones
- **Sin lógica**: no hay if/else ni for. Todo se basa en secciones y secciones invertidas
