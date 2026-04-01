# Patio La Esperanza — Sitio Web

Sitio web estático para **Patio La Esperanza**, empresa de logística y almacenaje de contenedores ubicada en Ciudad Lázaro Cárdenas, Michoacán.

## Tecnologías

- HTML5 / CSS3 / JavaScript vanilla
- Sin frameworks, sin build system, sin dependencias npm
- Google Fonts: Montserrat + Open Sans
- Font Awesome 6.5 (iconos)

## Estructura de archivos

```
web_patio_la_esperanza/
├── index.html              # Página única con 4 secciones ancla
├── assets/
│   ├── css/
│   │   └── style.css       # Todos los estilos + variables + media queries
│   ├── js/
│   │   └── main.js         # Navbar scroll, hamburger, active link, formulario, fade-in
│   └── img/                # Imágenes y logos
└── README.md
```

## Secciones

| Ancla | Sección |
|-------|---------|
| `#inicio` | Hero con estadísticas (7,000 m², 5 rampas, 45T, 2 bodegas) |
| `#nosotros` | Quiénes somos, Misión, Visión, Valores, galería |
| `#servicios` | 9 tarjetas de servicios + galería de instalaciones |
| `#contacto` | Info de contacto, formulario y mapa Google |

## Ejecutar localmente

```bash
python3 -m http.server 8080
# Abrir http://localhost:8080
```

O abrir `index.html` directamente en el navegador.

## Design tokens

Definidos en `:root` al inicio de `style.css`:

| Variable | Valor | Uso |
|----------|-------|-----|
| `--primary` | `#1a5276` | Azul marino — navbar, encabezados, tarjetas |
| `--accent` | `#e67e22` | Naranja — CTAs, hovers, separadores |
| `--font-head` | Montserrat | Títulos y etiquetas |
| `--font-body` | Open Sans | Texto general |

## Responsive

| Breakpoint | Cambios principales |
|------------|---------------------|
| `≤ 992px` | Servicios en 2 col, nosotros en 1 col, stats en 2 col |
| `≤ 768px` | Menú hamburger, botones hero apilados, contact en 1 col |
| `≤ 576px` | Servicios en 1 col, galerías en 1 col, mapa reducido |
| `≤ 400px` | Stats más compactos, valores en 2 col |

## SEO

- Meta description, keywords, canonical, robots
- Open Graph y Twitter Card
- Schema.org `LocalBusiness` JSON-LD
- `aria-label` en secciones, `role="contentinfo"` en footer

## Contacto de la empresa

- **Dirección:** Autopista Morelia-Lázaro Cárdenas No. 135, Isla del Cayacal, C.P. 60950, Lázaro Cárdenas, Mich.
- **Teléfono:** 753 537 7838
- **Email:** ventas@patiolaesperanza.com.mx
