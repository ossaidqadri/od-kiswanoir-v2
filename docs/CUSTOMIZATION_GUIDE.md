# Theme Customization & Settings Guide for Kiswa Noir

This guide covers how to customize the Kiswa Noir theme through the Shopify theme editor and configuration files.

## Theme Customization Overview

The Kiswa Noir theme is designed to be highly customizable through:

1. **Shopify Theme Editor** - Visual settings without code
2. **Configuration Files** - Direct JSON/Liquid changes
3. **Section Settings** - Per-section customization
4. **Template Configuration** - Page-level customization

## Accessing Theme Editor

### Shopify Admin

```
Shopify Admin > Online Store > Themes > Kiswa Noir > Customize
```

Or directly:

```
https://[store].myshopify.com/admin/themes/[theme-id]/editor
```

### Shopify CLI (Local Development)

```bash
# Install Shopify CLI
npm install -g @shopify/cli

# Serve theme locally
shopify theme serve

# Open in browser
https://127.0.0.1:9292
```

## Settings Schema Configuration

### settings_schema.json Structure

Controls what options appear in the theme editor:

```json
[
  {
    "name": "Colors",
    "settings": [
      {
        "type": "header",
        "content": "Primary Colors"
      },
      {
        "type": "color",
        "id": "primary_color",
        "label": "Primary Color",
        "default": "#000000"
      },
      {
        "type": "color",
        "id": "secondary_color",
        "label": "Secondary Color",
        "default": "#ffffff"
      }
    ]
  },
  {
    "name": "Typography",
    "settings": [
      {
        "type": "header",
        "content": "Fonts"
      },
      {
        "type": "font_picker",
        "id": "body_font",
        "label": "Body Font",
        "default": "open_sans"
      }
    ]
  }
]
```

## Common Customization Scenarios

### 1. Changing Colors

**Via Theme Editor:**

1. Open theme editor
2. Navigate to "Colors" or "Theme settings"
3. Click color picker
4. Select new color
5. Save

**Via Code (settings_data.json):**

```json
{
  "current": {
    "colors": {
      "primary_color": "#1a1a1a",
      "secondary_color": "#ffffff",
      "accent_color": "#e74c3c",
      "text_color": "#333333",
      "background_color": "#ffffff"
    }
  }
}
```

**In Liquid Templates:**

```liquid
<div style="color: {{ settings.text_color }}">
  Text with custom color
</div>

<button style="background-color: {{ settings.primary_color }}">
  Custom button
</button>
```

### 2. Customizing Header

**Access in Theme Editor:**

```
Theme Editor > Header > Customize
```

**Available Settings:**

- Logo/image
- Logo width
- Background color
- Text color
- Navigation style
- Cart visibility
- Search functionality
- Mobile menu style

**Configuring in Code:**

```json
{
  "current": {
    "header_logo": {
      "src": "cdn://[image-id].png",
      "width": 150
    },
    "header_bg_color": "#ffffff",
    "header_text_color": "#000000",
    "show_cart": true,
    "show_search": true
  }
}
```

### 3. Footer Customization

**Available Options:**

- Footer text/copyright
- Footer links
- Social media icons
- Newsletter signup
- Payment icons
- Contact information

**In sections/footer.liquid:**

```liquid
<footer id="section-{{ section.id }}" class="bg-black text-white">
  <div class="container">
    <!-- Footer content -->
    <p>{{ section.settings.copyright_text }}</p>

    {% if section.settings.show_newsletter %}
      <!-- Newsletter form -->
    {% endif %}
  </div>
</footer>

{% schema %}
{
  "name": "Footer",
  "settings": [
    {
      "type": "text",
      "id": "copyright_text",
      "label": "Copyright Text",
      "default": "© 2024 Kiswa Noir. All rights reserved."
    },
    {
      "type": "checkbox",
      "id": "show_newsletter",
      "label": "Show Newsletter Signup",
      "default": true
    }
  ]
}
{% endschema %}
```

### 4. Homepage Customization

**Via Theme Editor:**

1. Open theme editor
2. Click "Homepage"
3. Add/remove/reorder sections
4. Customize each section's settings

**Homepage Configuration (templates/index.json):**

```json
{
  "sections": {
    "hero_collage": {
      "type": "hero-collage",
      "settings": {
        "title": "Welcome",
        "image_1": "cdn://[image-id-1].png",
        "image_2": "cdn://[image-id-2].png"
      }
    },
    "featured_collection": {
      "type": "featured-collection",
      "settings": {
        "collection": "frontpage",
        "title": "Featured Products"
      }
    },
    "brand_message": {
      "type": "brand-message",
      "settings": {
        "text": "Minimalism - Accessible design by producing less & building better",
        "cta_text": "Learn More",
        "cta_url": "/about"
      }
    }
  },
  "order": ["hero_collage", "featured_collection", "brand_message"]
}
```

### 5. Product Page Customization

**Available Options:**

- Product gallery layout
- Product information display
- Related products section
- Customer reviews
- Size guide
- Variants display

**In sections/product.liquid:**

```liquid
<section id="product-section">
  <div class="product-gallery">
    <!-- Product images #}
  </div>

  <div class="product-info">
    <h1>{{ product.title }}</h1>
    <p class="price">{{ product.price | money }}</p>

    {% if section.settings.show_description %}
      <div>{{ product.description }}</div>
    {% endif %}

    <form>
      <!-- Variant selector -->
    </form>

    {% if section.settings.show_related %}
      <!-- Related products -->
    {% endif %}
  </div>
</section>

{% schema %}
{
  "name": "Product",
  "settings": [
    {
      "type": "checkbox",
      "id": "show_description",
      "label": "Show Product Description",
      "default": true
    },
    {
      "type": "checkbox",
      "id": "show_related",
      "label": "Show Related Products",
      "default": true
    }
  ]
}
{% endschema %}
```

## Section-Specific Settings

### Adding New Section Settings

1. **Update schema:**

```liquid
{% schema %}
{
  "name": "My Section",
  "settings": [
    {
      "type": "text",
      "id": "heading",
      "label": "Heading",
      "default": "Section Heading"
    },
    {
      "type": "color",
      "id": "bg_color",
      "label": "Background Color",
      "default": "#ffffff"
    }
  ]
}
{% endschema %}
```

2. **Use in template:**

```liquid
<section id="section-{{ section.id }}" style="background-color: {{ section.settings.bg_color }}">
  <h2>{{ section.settings.heading }}</h2>
</section>
```

3. **Change in theme editor:**
- Navigate to section
- Adjust settings
- See live preview

## Customizing Store Information

### Store Settings

```liquid
{# Access store settings in templates #}
{{ shop.name }}                    {# Store name #}
{{ shop.email }}                   {# Contact email #}
{{ shop.phone }}                   {# Phone number #}
{{ shop.address }}                 {# Address #}
{{ shop.currency }}                {# Currency code #}
{{ shop.money_format }}            {# Price format #}
{{ shop.primary_locale }}          {# Main language #}
```

### Using in Templates

```liquid
<footer>
  <h4>{{ shop.name }}</h4>
  <p>{{ shop.address.address1 }}</p>
  <p>{{ shop.address.city }}, {{ shop.address.province }}</p>
  <a href="tel:{{ shop.phone }}">{{ shop.phone }}</a>
  <a href="mailto:{{ shop.email }}">{{ shop.email }}</a>
</footer>
```

## Brand Customization

### Logo Configuration

**In sections/header.liquid:**

```liquid
{%- if section.settings.logo -%}
  <img
    src="{{ section.settings.logo | image_url: width: section.settings.logo_width }}"
    alt="{{ shop.name }}"
    class="logo"
    width="{{ section.settings.logo_width }}"
    height="auto"
  >
{%- else -%}
  <h1 class="store-name">{{ shop.name }}</h1>
{%- endif -%}

{% schema %}
{
  "name": "Header",
  "settings": [
    {
      "type": "image_picker",
      "id": "logo",
      "label": "Logo"
    },
    {
      "type": "range",
      "id": "logo_width",
      "min": 50,
      "max": 300,
      "step": 10,
      "unit": "px",
      "label": "Logo Width",
      "default": 150
    }
  ]
}
{% endschema %}
```

### Favicon Customization

**In snippets/theme-head.liquid:**

```liquid
<link rel="icon" type="image/svg+xml" href="{{ 'favicon.svg' | asset_url }}">
<link rel="apple-touch-icon" href="{{ 'apple-touch-icon.png' | asset_url }}">
```

### Social Media Links

**Store Settings > Online Store Settings > Social Media**

Access in templates:

```liquid
{%- if shop.social_media_accounts -%}
  <ul class="social-links">
    {%- for account in shop.social_media_accounts -%}
      <li>
        <a href="{{ account.link }}" title="{{ account.platform | capitalize }}">
          {%- include 'social-icon' with icon: account.platform -%}
        </a>
      </li>
    {%- endfor -%}
  </ul>
{%- endif -%}
```

## Advanced Customization

### Custom CSS

**Method 1: In Section Styles**

```liquid
{%- style -%}
  #section-{{ section.id }} {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  }

  #section-{{ section.id }} h2 {
    font-size: clamp(24px, 5vw, 48px);
    letter-spacing: 2px;
  }

  @media (max-width: 768px) {
    #section-{{ section.id }} {
      padding: 20px;
    }
  }
{%- endstyle -%}
```

**Method 2: In Assets**

Add custom CSS to `assets/custom.css` and include in `snippets/theme-head.liquid`:

```liquid
<link rel="stylesheet" href="{{ 'custom.css' | asset_url }}">
```

### Custom JavaScript

**In Section**

```liquid
{%- javascript -%}
  document.addEventListener('DOMContentLoaded', function() {
    const section = document.querySelector('#section-{{ section.id }}');
    // Custom functionality
  });
{%- endjavascript -%}
```

**In Assets**

Add to `assets/custom.js` and include in `snippets/theme-head.liquid`:

```liquid
<script src="{{ 'custom.js' | asset_url }}" defer></script>
```

## Theme Settings Best Practices

### 1. Logical Organization

Group related settings:

```json
[
  {
    "name": "Colors",
    "settings": [ ... ]
  },
  {
    "name": "Typography",
    "settings": [ ... ]
  },
  {
    "name": "Layout",
    "settings": [ ... ]
  }
]
```

### 2. Clear Defaults

Always provide sensible defaults:

```json
{
  "type": "color",
  "id": "primary_color",
  "label": "Primary Color",
  "default": "#000000"
}
```

### 3. Helpful Labels

Use clear, descriptive labels:

```json
{
  "type": "number",
  "id": "padding",
  "label": "Section Padding (pixels)",
  "min": 0,
  "max": 100,
  "step": 5,
  "default": 40
}
```

### 4. Limit Settings

Keep settings focused and minimal:

```json
{# Good - Essential settings only #}
{
  "type": "color",
  "id": "bg_color",
  "label": "Background"
}

{# Avoid - Too many micro-settings #}
{
  "type": "number",
  "id": "padding_top",
  "label": "Top Padding"
},
{
  "type": "number",
  "id": "padding_right",
  "label": "Right Padding"
},
{
  "type": "number",
  "id": "padding_bottom",
  "label": "Bottom Padding"
},
{
  "type": "number",
  "id": "padding_left",
  "label": "Left Padding"
}
```

### 5. Use Presets

Provide preset configurations:

```json
{
  "presets": [
    {
      "name": "Light Theme",
      "settings": {
        "bg_color": "#ffffff",
        "text_color": "#000000"
      }
    },
    {
      "name": "Dark Theme",
      "settings": {
        "bg_color": "#000000",
        "text_color": "#ffffff"
      }
    }
  ]
}
```

## Mobile Customization

### Mobile-Specific Settings

```liquid
{% schema %}
{
  "name": "Section",
  "settings": [
    {
      "type": "range",
      "id": "padding_desktop",
      "label": "Padding (Desktop)",
      "min": 0,
      "max": 100,
      "step": 5,
      "default": 40
    },
    {
      "type": "range",
      "id": "padding_mobile",
      "label": "Padding (Mobile)",
      "min": 0,
      "max": 50,
      "step": 5,
      "default": 20
    }
  ]
}
{% endschema %}

{%- style -%}
  #section-{{ section.id }} {
    padding: {{ section.settings.padding_desktop }}px;
  }

  @media (max-width: 768px) {
    #section-{{ section.id }} {
      padding: {{ section.settings.padding_mobile }}px;
    }
  }
{%- endstyle -%}
```

## Customization Examples

### Example 1: Custom Hero Banner

```liquid
<section id="section-{{ section.id }}" class="hero-banner">
  {% if section.settings.background_image %}
    <img
      src="{{ section.settings.background_image | image_url: width: 1600 }}"
      alt="Hero"
      class="hero-bg"
    >
  {% endif %}

  <div class="hero-content">
    <h1>{{ section.settings.title }}</h1>
    <p>{{ section.settings.subtitle }}</p>

    {% if section.settings.button_text %}
      <a href="{{ section.settings.button_url }}" class="btn">
        {{ section.settings.button_text }}
      </a>
    {% endif %}
  </div>
</section>

{%- style -%}
  #section-{{ section.id }} {
    min-height: {{ section.settings.height }}px;
    background-color: {{ section.settings.overlay_color }};
    display: flex;
    align-items: center;
    justify-content: center;
    color: {{ section.settings.text_color }};
    text-align: center;
  }

  #section-{{ section.id }} .hero-bg {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
    opacity: {{ section.settings.image_opacity }};
  }

  #section-{{ section.id }} h1 {
    font-size: {{ section.settings.title_size }}px;
  }
{%- endstyle -%}

{% schema %}
{
  "name": "Custom Hero",
  "settings": [
    {
      "type": "image_picker",
      "id": "background_image",
      "label": "Background Image"
    },
    {
      "type": "text",
      "id": "title",
      "label": "Title"
    },
    {
      "type": "textarea",
      "id": "subtitle",
      "label": "Subtitle"
    },
    {
      "type": "url",
      "id": "button_url",
      "label": "Button URL"
    },
    {
      "type": "text",
      "id": "button_text",
      "label": "Button Text"
    },
    {
      "type": "range",
      "id": "height",
      "min": 200,
      "max": 800,
      "step": 50,
      "unit": "px",
      "label": "Height",
      "default": 500
    },
    {
      "type": "color",
      "id": "text_color",
      "label": "Text Color"
    }
  ]
}
{% endschema %}
```

### Example 2: Product Filter Customization

```liquid
<div class="product-filters">
  {% if section.settings.show_sort %}
    <select id="sort-by">
      <option value="newest">Newest</option>
      <option value="price-low-to-high">Price: Low to High</option>
      <option value="price-high-to-low">Price: High to Low</option>
      <option value="best-selling">Best Selling</option>
    </select>
  {% endif %}

  {% if section.settings.show_filters %}
    <aside class="filters">
      <!-- Filter options -->
    </aside>
  {% endif %}
</div>

{% schema %}
{
  "name": "Product Filters",
  "settings": [
    {
      "type": "checkbox",
      "id": "show_sort",
      "label": "Show Sort Options",
      "default": true
    },
    {
      "type": "checkbox",
      "id": "show_filters",
      "label": "Show Filters",
      "default": true
    }
  ]
}
{% endschema %}
```

## Troubleshooting

### Settings Not Appearing

1. Check `settings_schema.json` is valid JSON
2. Verify section file has proper `{% schema %}` block
3. Reload theme editor
4. Check browser console for errors

### Changes Not Saving

1. Ensure JSON is properly formatted
2. Check for duplicate setting IDs
3. Verify file permissions
4. Clear browser cache

### Preview Not Updating

1. Save changes in editor
2. Refresh browser tab
3. Check network tab for errors
4. Verify CSS is being applied

## Resources

- [Shopify Settings Schema Documentation](https://shopify.dev/themes/architecture/settings)
- [Theme Editor Guide](https://shopify.dev/themes/tools/theme-editor)
- [Liquid Reference](https://shopify.dev/api/liquid)

