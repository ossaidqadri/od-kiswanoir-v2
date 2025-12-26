# Kiswa Noir Sections Development Guide

A section is a reusable, customizable component that can be added to any page via the Shopify theme editor. This guide covers how sections work and how to create new ones.

## Section Anatomy

Every section consists of three parts:

```liquid
<!-- HTML & Liquid markup -->
<section id="section-{{ section.id }}" class="my-section">
  <div class="container">
    <h2>{{ section.settings.title }}</h2>
    <p>{{ section.settings.description }}</p>
  </div>
</section>

<!-- CSS styling -->
{%- style -%}
  #section-{{ section.id }} {
    background-color: {{ section.settings.bg_color }};
    padding: {{ section.settings.padding }}px;
  }

  #section-{{ section.id }} h2 {
    font-size: {{ section.settings.title_size }}px;
    color: {{ section.settings.title_color }};
  }
{%- endstyle -%}

<!-- JavaScript for interactivity -->
{%- javascript -%}
  document.addEventListener('DOMContentLoaded', function() {
    const section = document.querySelector('#section-{{ section.id }}');
    // Initialize component
  });
{%- endjavascript -%}

<!-- Schema for theme editor settings -->
{% schema %}
{
  "name": "My Section",
  "target": "section",
  "settings": [
    {
      "type": "text",
      "id": "title",
      "label": "Title"
    }
  ]
}
{% endschema %}
```

## Section Structure

### 1. ID and Classes

Always use `section.id` for unique identification:

```liquid
<section id="section-{{ section.id }}" class="my-section">
  <!-- Content -->
</section>
```

This ensures:
- Unique IDs for styling multiple instances
- Scoped CSS that doesn't affect other sections
- Proper DOM targeting for JavaScript

### 2. Settings Access

Access section settings via `section.settings`:

```liquid
{{ section.settings.title }}
{{ section.settings.bg_color }}
{{ section.settings.show_image }}
```

### 3. Blocks (Sub-components)

Sections can contain blocks for repeatable content:

```liquid
{% schema %}
{
  "name": "Featured Products",
  "blocks": [
    {
      "type": "product",
      "name": "Product",
      "settings": [
        {
          "type": "product",
          "id": "product",
          "label": "Product"
        }
      ]
    }
  ]
}
{% endschema %}
```

Render blocks in your markup:

```liquid
{%- for block in section.blocks -%}
  <div class="product-item" {{ block.shopify_attributes }}>
    <h3>{{ block.settings.product.title }}</h3>
    <a href="{{ block.settings.product.url }}">View</a>
  </div>
{%- endfor -%}
```

## Schema Settings Types

The schema defines what options appear in the theme editor.

### Text Input

```json
{
  "type": "text",
  "id": "title",
  "label": "Title",
  "default": "Welcome",
  "placeholder": "Enter title"
}
```

### Number Input

```json
{
  "type": "number",
  "id": "padding",
  "label": "Padding (px)",
  "min": 0,
  "max": 100,
  "step": 5,
  "default": 20
}
```

### Color Picker

```json
{
  "type": "color",
  "id": "bg_color",
  "label": "Background Color",
  "default": "#ffffff"
}
```

### Toggle/Checkbox

```json
{
  "type": "checkbox",
  "id": "show_image",
  "label": "Show Image?",
  "default": true
}
```

### Select Dropdown

```json
{
  "type": "select",
  "id": "text_align",
  "label": "Text Alignment",
  "options": [
    {
      "value": "left",
      "label": "Left"
    },
    {
      "value": "center",
      "label": "Center"
    }
  ],
  "default": "left"
}
```

### Image Picker

```json
{
  "type": "image_picker",
  "id": "image",
  "label": "Image"
}
```

### Product Picker

```json
{
  "type": "product",
  "id": "product",
  "label": "Select Product"
}
```

### Collection Picker

```json
{
  "type": "collection",
  "id": "collection",
  "label": "Select Collection"
}
```

### Page Picker

```json
{
  "type": "page",
  "id": "page",
  "label": "Select Page"
}
```

### Rich Text Editor

```json
{
  "type": "richtext",
  "id": "description",
  "label": "Description",
  "default": "<p>Default text</p>"
}
```

### URL Input

```json
{
  "type": "url",
  "id": "link_url",
  "label": "Link URL"
}
```

### Settings Groups

Organize settings with the `info` type:

```json
{
  "type": "header",
  "content": "Colors"
},
{
  "type": "color",
  "id": "primary_color",
  "label": "Primary"
}
```

## Complete Section Example

Here's a complete "Hero Banner" section:

```liquid
{%- style -%}
  #section-{{ section.id }} {
    position: relative;
    min-height: {{ section.settings.height }}px;
    background-color: {{ section.settings.overlay_color }};
    overflow: hidden;
  }

  #section-{{ section.id }} .hero-image {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
    opacity: {{ section.settings.image_opacity }};
  }

  #section-{{ section.id }} .hero-content {
    position: relative;
    z-index: 2;
    display: flex;
    flex-direction: column;
    justify-content: {{ section.settings.vertical_align }};
    align-items: {{ section.settings.horizontal_align }};
    height: 100%;
    padding: {{ section.settings.padding }}px;
    color: {{ section.settings.text_color }};
    text-align: {{ section.settings.text_align }};
  }

  #section-{{ section.id }} h1 {
    font-size: {{ section.settings.title_size }}px;
    font-weight: {{ section.settings.title_weight }};
    margin-bottom: 1rem;
    line-height: 1.2;
  }

  #section-{{ section.id }} .hero-button {
    display: inline-block;
    background-color: {{ section.settings.button_color }};
    color: {{ section.settings.button_text_color }};
    padding: 12px 28px;
    text-decoration: none;
    border-radius: {{ section.settings.button_radius }}px;
    transition: all 0.3s ease;
  }

  #section-{{ section.id }} .hero-button:hover {
    opacity: 0.8;
    transform: translateY(-2px);
  }

  @media (max-width: 768px) {
    #section-{{ section.id }} {
      min-height: {{ section.settings.height_mobile }}px;
    }

    #section-{{ section.id }} h1 {
      font-size: {{ section.settings.title_size_mobile }}px;
    }

    #section-{{ section.id }} .hero-content {
      padding: {{ section.settings.padding_mobile }}px;
    }
  }
{%- endstyle -%}

<section id="section-{{ section.id }}">
  {%- if section.settings.image -%}
    {{
      section.settings.image
      | image_url: width: 1600
      | image_tag: class: "hero-image", alt: section.settings.image.alt
    }}
  {%- endif -%}

  <div class="hero-content">
    {%- if section.settings.title -%}
      <h1>{{ section.settings.title }}</h1>
    {%- endif -%}

    {%- if section.settings.description -%}
      <p class="mb-6">{{ section.settings.description }}</p>
    {%- endif -%}

    {%- if section.settings.button_text and section.settings.button_url -%}
      <a href="{{ section.settings.button_url }}" class="hero-button">
        {{ section.settings.button_text }}
      </a>
    {%- endif -%}
  </div>
</section>

{%- javascript -%}
  // Add any interactive behavior here
  (function() {
    const section = document.querySelector('#section-{{ section.id }}');
    if (section) {
      // Component logic
    }
  })();
{%- endjavascript -%}

{% schema %}
{
  "name": "Hero Banner",
  "target": "section",
  "settings": [
    {
      "type": "header",
      "content": "Content"
    },
    {
      "type": "text",
      "id": "title",
      "label": "Title",
      "default": "Welcome to Kiswa Noir"
    },
    {
      "type": "textarea",
      "id": "description",
      "label": "Description",
      "default": "Your shop description"
    },
    {
      "type": "image_picker",
      "id": "image",
      "label": "Background Image"
    },
    {
      "type": "header",
      "content": "Button"
    },
    {
      "type": "text",
      "id": "button_text",
      "label": "Button Text",
      "default": "Shop Now"
    },
    {
      "type": "url",
      "id": "button_url",
      "label": "Button URL"
    },
    {
      "type": "header",
      "content": "Layout"
    },
    {
      "type": "number",
      "id": "height",
      "label": "Height (Desktop, px)",
      "min": 200,
      "max": 1000,
      "step": 50,
      "default": 600
    },
    {
      "type": "number",
      "id": "height_mobile",
      "label": "Height (Mobile, px)",
      "min": 200,
      "max": 600,
      "step": 50,
      "default": 400
    },
    {
      "type": "select",
      "id": "vertical_align",
      "label": "Vertical Alignment",
      "options": [
        { "value": "flex-start", "label": "Top" },
        { "value": "center", "label": "Center" },
        { "value": "flex-end", "label": "Bottom" }
      ],
      "default": "center"
    },
    {
      "type": "select",
      "id": "horizontal_align",
      "label": "Horizontal Alignment",
      "options": [
        { "value": "flex-start", "label": "Left" },
        { "value": "center", "label": "Center" },
        { "value": "flex-end", "label": "Right" }
      ],
      "default": "center"
    },
    {
      "type": "select",
      "id": "text_align",
      "label": "Text Alignment",
      "options": [
        { "value": "left", "label": "Left" },
        { "value": "center", "label": "Center" },
        { "value": "right", "label": "Right" }
      ],
      "default": "center"
    },
    {
      "type": "number",
      "id": "padding",
      "label": "Padding (Desktop, px)",
      "min": 0,
      "max": 100,
      "step": 5,
      "default": 40
    },
    {
      "type": "number",
      "id": "padding_mobile",
      "label": "Padding (Mobile, px)",
      "min": 0,
      "max": 100,
      "step": 5,
      "default": 20
    },
    {
      "type": "header",
      "content": "Colors"
    },
    {
      "type": "color",
      "id": "overlay_color",
      "label": "Overlay Color",
      "default": "rgba(0, 0, 0, 0.3)"
    },
    {
      "type": "range",
      "id": "image_opacity",
      "min": 0,
      "max": 1,
      "step": 0.1,
      "label": "Image Opacity",
      "default": 1
    },
    {
      "type": "color",
      "id": "text_color",
      "label": "Text Color",
      "default": "#ffffff"
    },
    {
      "type": "number",
      "id": "title_size",
      "label": "Title Size (Desktop, px)",
      "min": 20,
      "max": 100,
      "step": 5,
      "default": 56
    },
    {
      "type": "number",
      "id": "title_size_mobile",
      "label": "Title Size (Mobile, px)",
      "min": 20,
      "max": 60,
      "step": 5,
      "default": 32
    },
    {
      "type": "select",
      "id": "title_weight",
      "label": "Title Weight",
      "options": [
        { "value": "400", "label": "Regular" },
        { "value": "600", "label": "Semibold" },
        { "value": "700", "label": "Bold" },
        { "value": "800", "label": "Extrabold" }
      ],
      "default": "700"
    },
    {
      "type": "color",
      "id": "button_color",
      "label": "Button Color",
      "default": "#000000"
    },
    {
      "type": "color",
      "id": "button_text_color",
      "label": "Button Text Color",
      "default": "#ffffff"
    },
    {
      "type": "number",
      "id": "button_radius",
      "label": "Button Border Radius (px)",
      "min": 0,
      "max": 50,
      "step": 5,
      "default": 4
    }
  ],
  "presets": [
    {
      "name": "Hero Banner",
      "settings": {
        "title": "Welcome to Kiswa Noir",
        "description": "Minimalism - Accessible design by producing less & building better"
      }
    }
  ]
}
{% endschema %}
```

## Blocks Example

Sections with repeating content use blocks:

```liquid
{%- for block in section.blocks -%}
  <div class="feature-item" {{ block.shopify_attributes }}>
    <h3>{{ block.settings.title }}</h3>
    <p>{{ block.settings.description }}</p>
  </div>
{%- endfor -%}

{% schema %}
{
  "name": "Features",
  "blocks": [
    {
      "type": "feature",
      "name": "Feature",
      "settings": [
        {
          "type": "text",
          "id": "title",
          "label": "Title"
        },
        {
          "type": "textarea",
          "id": "description",
          "label": "Description"
        }
      ]
    }
  ]
}
{% endschema %}
```

## Best Practices

### 1. Always Use section.id

```liquid
{# Correct #}
<section id="section-{{ section.id }}">

{# Incorrect - will conflict with other sections #}
<section id="my-section">
```

### 2. Style Scoping

Use section-specific selectors in `<style>` tags:

```liquid
{%- style -%}
  #section-{{ section.id }} h2 { color: red; }
  #section-{{ section.id }} .btn { padding: 10px; }
{%- endstyle -%}
```

### 3. Keep Schemas Simple

Only expose settings that users should customize:

```json
{
  "type": "text",
  "id": "title",
  "label": "Title",
  "default": "Section Title"
}
```

### 4. Provide Defaults

```json
{
  "type": "color",
  "id": "bg_color",
  "label": "Background",
  "default": "#ffffff"
}
```

### 5. Use Presets

Help users get started with section variations:

```json
"presets": [
  {
    "name": "With Image",
    "settings": {
      "show_image": true
    }
  }
]
```

### 6. Mobile Responsiveness

Always consider mobile in your CSS:

```css
@media (max-width: 768px) {
  #section-{{ section.id }} {
    padding: 20px;
  }

  #section-{{ section.id }} h2 {
    font-size: 24px;
  }
}
```

### 7. Accessibility

Include proper ARIA labels and semantic HTML:

```liquid
<section id="section-{{ section.id }}" role="region" aria-label="{{ section.settings.title }}">
  <h2>{{ section.settings.title }}</h2>
  <nav aria-label="Main navigation">
    {%- include 'menu' -%}
  </nav>
</section>
```

### 8. Image Optimization

Always optimize images:

```liquid
{{
  product.featured_image
  | image_url: width: 500
  | image_tag:
    alt: product.title,
    loading: "lazy",
    sizes: "(max-width: 768px) 100vw, 50vw",
    widths: "300, 500, 800"
}}
```

### 9. Internationalization

Translate all user-facing text:

```liquid
<h2>{{ 'sections.featured_collection.title' | t }}</h2>
<button>{{ 'general.shop_now' | t }}</button>
```

### 10. Test Editor Experience

Always test sections in the theme editor to ensure:
- All settings work correctly
- Preview updates properly
- Controls are intuitive
- Defaults make sense

## Common Patterns

### Conditional Settings

```liquid
{%- if section.settings.show_description -%}
  <p>{{ section.settings.description }}</p>
{%- endif -%}
```

### Dynamic Classes

```liquid
<div class="product-grid {% if section.settings.columns_mobile == 1 %}grid-cols-1{% elsif section.settings.columns_mobile == 2 %}grid-cols-2{% endif %}">
```

### Include Snippets

```liquid
{%- include 'product-card' with product: block.settings.product -%}
```

### Product Loops

```liquid
{%- for product in section.settings.collection.products -%}
  <div>{{ product.title }}</div>
{%- endfor -%}
```

## Existing Sections in Kiswa Noir

Refer to these sections as examples:

- `header.liquid` - Navigation with dropdown
- `footer.liquid` - Links and social icons
- `hero-collage.liquid` - Multi-image gallery
- `featured-collection.liquid` - Product grid
- `brand-message.liquid` - Text + CTA
- `recommended-products.liquid` - Related items

## Resources

- [Shopify Sections Documentation](https://shopify.dev/themes/architecture/sections)
- [Schema Types Reference](https://shopify.dev/themes/architecture/settings/input-settings)
- [Theme Best Practices](https://shopify.dev/themes/best-practices)

