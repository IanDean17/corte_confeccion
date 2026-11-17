# Johanna Corte y Confección

Sitio web para el taller de corte y confección de Johanna: arreglos, ajustes y confección a medida en Santiago.

## Funciones
- Barra de navegación con la marca y menú adaptable a celular.
- Sección **Sobre nosotros** y servicios.
- **Carrusel** de trabajos realizados (flechas, puntos, deslizamiento y avance automático).
- **Precios de referencia** con plazos de entrega.
- **Chatbot** que cotiza con precios estimados y plazos, calcula totales por cantidad (uniformes, cortinas) y responde sobre horarios, pagos, telas y despacho.
- **Agenda** con calendario de disponibilidad: días y horas ocupadas se bloquean para evitar choques; pide correo y envía confirmación automática.
- Pie de página con redes sociales, horario y contacto.
- Paleta pastel intensa, modo claro/oscuro y diseño adaptable a cualquier resolución.

## Tecnologías
HTML, CSS y JavaScript puro, sin dependencias. Reservas compartidas con **Supabase** y correos con **EmailJS** (ver [CONFIGURAR-RESERVAS.md](CONFIGURAR-RESERVAS.md)); sin configurar, funciona en modo demostración.

## Cómo verlo
Abre `index.html` o sirve la carpeta:
```bash
python -m http.server 5173
```

Desarrollado por [Ian Navarrete](https://github.com/IanDean17).
