# Kiswa Noir Theme Architecture

## Overview

Kiswa Noir is a modern, minimalist Shopify theme built with Liquid templating, Tailwind CSS, and vanilla JavaScript. It emphasizes clean design, accessibility, and performance with support for 30+ languages.

**Brand Philosophy**: "Minimalism - Accessible design by producing less & building better"

## Project Structure

```
kiswa/
├── layout/                 # HTML template wrappers
│   ├── theme.liquid       # Main layout for all pages
│   └── password.liquid    # Password-protected store layout
│
├── sections/              # Reusable component modules
│   ├── header.liquid      # Primary navigation & branding
│   ├── footer.liquid      # Footer with links & social
│   ├── hero-*.liquid      # Hero banner variations
│   ├── featured-*.liquid  # Product showcase sections
│   └── ...                # 21 total section files
│
├── snippets/              # Reusable partial templates
│   ├── theme-head.liquid  # Meta tags, stylesheets, fonts
│   ├── menu.liquid        # Menu rendering utility
│   ├── cart-*.liquid      # Cart components
│   ├── icons.liquid       # SVG icon snippets
│   └── ...                # 17 total snippet files
│
├── templates/             # Page type layouts
│   ├── index.json         # Homepage
│   ├── product.json       # Product detail pages
│   ├── collection.json    # Category listings
│   ├── cart.json          # Shopping cart
│   ├── blog.json          # Blog listing
│   ├── article.json       # Individual blog posts
│   ├── page.json          # Static pages
│   ├── search.json        # Search results
│   ├── password.json      # Password protection page
│   ├── 404.json           # Error page
│   └── ...                # 13 total template files
│
├── assets/                # Compiled CSS & JavaScript
│   ├── tailwind.css       # Compiled Tailwind (80KB)
│   ├── customer.css       # Customer account styling (10KB)
│   ├── global.js          # Core utilities & logic (38KB)
│   ├── cart.js            # Cart Web Components
│   ├── cart-notification.js
│   └── swiper.min.js      # Image carousel library
│
├── config/                # Theme configuration
│   ├── settings_schema.json    # Customization options
│   └── settings_data.json      # Current theme settings
│
├── locales/               # Multi-language translations
│   ├── en.default.json    # English (base language)
│   ├── es.json, fr.json   # Spanish, French, etc.
│   ├── *.schema.json      # Translatable field definitions
│   └── ...                # 30 languages + 30 schema files
│
└── Root files
    ├── favicon.svg        # Browser tab icon
    ├── logo.svg           # Brand logo (Great Vibes font)
    └── .gitignore         # Git ignore rules
```

**Total**: 145+ files, 48 Liquid templates

## Core Concepts

### 1. Sections (Reusable Components)

Sections are self-contained Liquid modules that can be added/configured via the Shopify theme editor. Each section includes:

- **HTML/Liquid markup** - Component structure
- **CSS styling** - Scoped styles (Tailwind + custom)
- **JavaScript** - Interactive behavior
- **Schema** - Editor settings and configuration

**Key Sections**:
- `header.liquid` - Navigation, logo, cart, menu
- `footer.liquid` - Links, social, copyright
- `hero-collage.liquid` - Multi-image gallery
- `featured-collection.liquid` - Product grid
- `brand-message.liquid` - Text + CTA section

### 2. Snippets (Reusable Partials)

Smaller template fragments that are included in sections or layouts:

- `theme-head.liquid` - Meta tags, stylesheets, preloads
- `menu.liquid` - Navigation menu rendering
- `cart-items.liquid` - Shopping cart item display
- `icons.liquid` - SVG icon library

### 3. Templates (Page Types)

JSON-based page configurations that define which sections appear on each page type:

- **index.json** - Homepage with hero + brand message
- **product.json** - Product detail + recommended items
- **collection.json** - Category with product grid
- **cart.json** - Shopping cart interface

### 4. Liquid Template Engine

Shopify's template language used in all .liquid files:

```liquid
{# Comments #}
{{ variable }}              {# Output variable #}
{{ variable | filter }}     {# Apply filter #}
{% if condition %}...{% endif %}
{% for item in collection %}...{% endfor %}
{% include 'snippet-name' %}
```

### 5. Web Components Pattern

Modern JavaScript modules for interactive features:

```javascript
// components/cart-items.js
class CartItems extends HTMLElement {
  connectedCallback() {
    this.addEventListener('click', this.handleClick);
  }
}
customElements.define('cart-items', CartItems);
```

## Data Flow

### 1. Homepage Load

```
theme.liquid (main layout)
  ├── theme-head.liquid (meta, CSS, fonts)
  ├── header.liquid (navigation)
  ├── index.json sections
  │   ├── hero-collage
  │   ├── brand-message
  │   ├── featured-collection
  │   └── recommended-products
  ├── footer.liquid
  └── global.js (initialization)
```

### 2. Product Page Load

```
theme.liquid
  ├── theme-head.liquid
  ├── header.liquid
  ├── product.json sections
  │   ├── product-details
  │   ├── product-images
  │   └── recommended-products
  ├── footer.liquid
  └── global.js + cart.js
```

### 3. Cart Interaction

```
Cart Button Click
  ├── global.js handles cart API call
  ├── Updates cart state
  ├── cart-notification.js shows toast
  ├── cart-drawer.js renders items (Web Component)
  └── Updates cart count in header
```

## Technology Stack

| Layer | Technology | Usage |
|-------|-----------|-------|
| **Templating** | Shopify Liquid | All .liquid files |
| **Styling** | Tailwind CSS 3.x | Main styling framework |
| **CSS** | Custom CSS | Tailwind + custom classes |
| **JavaScript** | ES6+ Vanilla JS | No frameworks/jQuery |
| **Web Components** | Native Custom Elements API | cart.js, cart-notification.js |
| **Fonts** | Google Fonts (Great Vibes) | Logo/branding typography |
| **Icons** | Custom SVG | Inline icons.liquid |
| **Carousels** | Swiper.js | Image galleries |
| **Effects** | Kursor.js | Custom cursor |
| **UI Library** | Flowbite | Modals, dropdowns |

## Performance Optimizations

### 1. Font Loading

```liquid
{# theme-head.liquid #}
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preload" href="..." as="font" type="font/woff2">
```

### 2. Image Optimization

- Lazy loading with `loading="lazy"`
- Multiple image sizes for responsive design
- Image preloading for critical paths

### 3. Asset Optimization

- CSS minified to 80KB (Tailwind)
- JavaScript split into modules (4-38KB)
- SVG sprites for icons
- Async/defer loading for non-critical scripts

### 4. Caching

- Browser caching via Shopify CDN
- Service Worker for offline support (optional)

## Configuration System

### settings_schema.json

Defines customizable theme options:

```json
[
  {
    "name": "Header",
    "settings": [
      {
        "type": "color",
        "id": "header_bg_color",
        "label": "Background Color"
      }
    ]
  }
]
```

### settings_data.json

Current active configuration values:

```json
{
  "colors": {
    "bg_color": "#ffffff",
    "text_color": "#000000"
  }
}
```

Access in Liquid:

```liquid
{{ settings.colors.bg_color }}
```

## Internationalization (i18n)

### File Structure

```
locales/
├── en.default.json       # English base translations
├── es.json               # Spanish
├── fr.json               # French
└── en.schema.json        # English field definitions
```

### Usage in Templates

```liquid
{# Translate using t filter #}
{{ 'general.404.title' | t }}

{# Translate with variables #}
{{ 'cart.item_count' | t: count: cart.item_count }}
```

### Translation File Structure

```json
{
  "general": {
    "404": {
      "title": "Page Not Found"
    },
    "pagination": {
      "label": "Pagination"
    }
  },
  "cart": {
    "empty": "Your cart is empty"
  }
}
```

## Styling Architecture

### Tailwind CSS Integration

Primary styling framework using utility classes:

```html
<div class="flex items-center justify-between bg-white p-4">
  <h1 class="text-2xl font-bold">Title</h1>
  <button class="bg-black text-white px-4 py-2">Click</button>
</div>
```

### Custom CSS Classes

Tailwind + custom styles in theme-head.liquid:

```css
/* Custom patterns */
.btn-primary {
  @apply px-6 py-2 bg-black text-white rounded transition;
}

.section-spacing {
  @apply py-12 md:py-20;
}
```

### Responsive Design

Mobile-first approach with breakpoints:

```html
<!-- Mobile (default) -->
<div class="text-sm">

<!-- Tablet and up -->
<div class="md:text-base">

<!-- Desktop and up -->
<div class="lg:text-lg">
```

## Accessibility Standards

### ARIA Labels

```liquid
<button aria-label="{{ 'cart.toggle' | t }}">
  {{ 'cart.label' | t }}
</button>
```

### Semantic HTML

```liquid
<nav>
  {%- include 'menu' -%}
</nav>

<main role="main">
  {%- section 'header' -%}
</main>
```

### Focus Management

Global focus utilities in global.js for keyboard navigation.

## Development Workflow

### 1. Local Development

- Use Shopify CLI (`shopify theme serve`)
- Edit Liquid/CSS/JS files
- Changes auto-reload in browser
- Test across devices

### 2. Creating a New Section

1. Create `sections/my-section.liquid`
2. Add HTML structure
3. Add CSS (scoped to section)
4. Add schema for editor
5. Reference in template JSON

### 3. Adding Translations

1. Add key to `locales/en.default.json`
2. Update all language files
3. Use `{{ 'key.path' | t }}` in templates

### 4. Deployment

- Push to Shopify theme repository
- Use Shopify Admin to deploy
- Or use theme CLI: `shopify theme push`

## Common Patterns

### Section with Settings

```liquid
{%- style -%}
  #section-{{ section.id }} {
    background-color: {{ section.settings.bg_color }};
  }
{%- endstyle -%}

<section id="section-{{ section.id }}">
  {{ section.settings.title }}
</section>

{% schema %}
{
  "name": "My Section",
  "settings": [
    {
      "type": "color",
      "id": "bg_color",
      "label": "Background"
    }
  ]
}
{% endschema %}
```

### Web Component

```javascript
class MyComponent extends HTMLElement {
  connectedCallback() {
    this.setup();
  }

  setup() {
    // Initialize component
  }

  handleClick(e) {
    // Handle click
  }
}

customElements.define('my-component', MyComponent);
```

### Product Loop

```liquid
{%- for product in collection.products -%}
  <div class="product-card">
    <a href="{{ product.url }}">
      <img src="{{ product.featured_image | image_url: width: 500 }}" alt="{{ product.title }}">
    </a>
    <h3>{{ product.title }}</h3>
    <p>{{ product.price | money }}</p>
  </div>
{%- endfor -%}
```

## Best Practices

1. **Use sections** for all page content (editor-friendly)
2. **Use snippets** for reusable partials (DRY)
3. **Prefer Tailwind** over custom CSS
4. **Always include translations** for user-facing text
5. **Use Web Components** for interactive features
6. **Test accessibility** with keyboard navigation
7. **Optimize images** with srcset and lazy loading
8. **Cache static assets** appropriately
9. **Keep schema simple** for better UX
10. **Document complex logic** with comments

## Resources

- [Shopify Theme Development](https://shopify.dev/themes)
- [Liquid Reference](https://shopify.dev/api/liquid)
- [Tailwind CSS Docs](https://tailwindcss.com/docs)
- [Web Components Guide](https://developer.mozilla.org/en-US/docs/Web/Web_Components)

