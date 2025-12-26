# Internationalization (i18n) & Translation Guide for Kiswa Noir

This guide covers how to manage and create translations for the Kiswa Noir theme across 30+ languages.

## Overview

Kiswa Noir supports **30+ languages** through a comprehensive translation system:

```
locales/
├── en.default.json       (Base language - English)
├── es.json               (Spanish)
├── fr.json               (French)
├── de.json               (German)
├── it.json               (Italian)
├── pt.json               (Portuguese)
├── ja.json               (Japanese)
├── zh.json               (Simplified Chinese)
├── ... (20+ more languages)
│
├── en.schema.json        (Translatable section settings)
├── es.schema.json
├── ... (schema files for all languages)
```

## Translation File Structure

### Base Language File (en.default.json)

```json
{
  "general": {
    "404": {
      "title": "Page Not Found",
      "message": "The page you requested could not be found",
      "back_button": "Go Back Home"
    },
    "pagination": {
      "label": "Pagination",
      "next": "Next",
      "previous": "Previous"
    },
    "shop_now": "Shop Now",
    "learn_more": "Learn More",
    "add_to_cart": "Add to Cart",
    "remove_from_cart": "Remove",
    "quantity": "Quantity",
    "price": "Price",
    "contact": {
      "email": "Email",
      "phone": "Phone",
      "address": "Address",
      "hours": "Hours"
    }
  },
  "sections": {
    "header": {
      "menu": "Menu",
      "search": "Search",
      "cart": "Cart"
    },
    "footer": {
      "newsletter_title": "Subscribe to Our Newsletter",
      "subscribe": "Subscribe",
      "follow_us": "Follow Us",
      "copyright": "All rights reserved"
    },
    "featured_collection": {
      "title": "Featured Collection",
      "view_all": "View All"
    },
    "hero_banner": {
      "title": "Welcome to Kiswa Noir",
      "subtitle": "Minimalism - Accessible design by producing less & building better"
    }
  },
  "cart": {
    "title": "Shopping Cart",
    "empty": "Your cart is empty",
    "item_count": "{{ count }} item in cart",
    "item_count_plural": "{{ count }} items in cart",
    "subtotal": "Subtotal",
    "shipping": "Shipping",
    "tax": "Tax",
    "total": "Total",
    "continue_shopping": "Continue Shopping",
    "checkout": "Checkout",
    "update": "Update Cart"
  },
  "product": {
    "title": "Product",
    "price": "Price",
    "compare_at": "Compare at",
    "in_stock": "In Stock",
    "out_of_stock": "Out of Stock",
    "quantity": "Quantity",
    "variant": "Variant",
    "color": "Color",
    "size": "Size",
    "description": "Description",
    "reviews": "Reviews",
    "share": "Share"
  },
  "customer": {
    "login": "Log In",
    "logout": "Log Out",
    "register": "Create Account",
    "account": "Account",
    "addresses": "Addresses",
    "orders": "Orders",
    "email": "Email",
    "password": "Password",
    "first_name": "First Name",
    "last_name": "Last Name"
  }
}
```

## Using Translations in Templates

### Basic Translation

```liquid
{# Translate simple strings #}
<h1>{{ 'general.shop_now' | t }}</h1>
<p>{{ 'sections.header.menu' | t }}</p>
<button>{{ 'sections.featured_collection.view_all' | t }}</button>
```

### Translations with Variables

```liquid
{# Translate with dynamic values #}
<p>{{ 'cart.item_count' | t: count: cart.item_count }}</p>

{# Output: "5 items in cart" if count is 5 #}

{# Translate with product name #}
<h2>{{ 'product.title' | t: name: product.title }}</h2>
```

### Pluralization

```liquid
{# Automatic pluralization based on count #}
<p>{{ 'cart.item_count' | t: count: cart.item_count }}</p>

{# JSON defines singular and plural forms #}
{
  "item_count": "{{ count }} item in cart",
  "item_count_plural": "{{ count }} items in cart"
}
```

### Conditional Translations

```liquid
{% if product.available %}
  {# Use translation if product is available #}
  <p>{{ 'product.in_stock' | t }}</p>
{% else %}
  {# Use different translation if out of stock #}
  <p>{{ 'product.out_of_stock' | t }}</p>
{% endif %}
```

### Nested Translations

```liquid
{# Access nested translation keys #}
{{ 'general.contact.email' | t }}      {# Email #}
{{ 'general.contact.phone' | t }}      {# Phone #}
{{ 'general.contact.address' | t }}    {# Address #}
```

## Schema Translations

### Schema File Structure

Sections can have translatable settings through `.schema.json` files:

```json
{# en.schema.json #}
{
  "sections": {
    "hero_banner": {
      "name": "Hero Banner",
      "settings": {
        "title": {
          "content": "Title",
          "label": "Title"
        },
        "description": {
          "content": "Description",
          "label": "Description"
        },
        "button_text": {
          "content": "Button Text",
          "label": "Button Text"
        }
      }
    }
  }
}
```

### Using Schema Translations

```liquid
{% schema %}
{
  "name": "t:sections.hero_banner.name",
  "settings": [
    {
      "type": "text",
      "id": "title",
      "label": "t:sections.hero_banner.settings.title.label"
    }
  ]
}
{% endschema %}
```

## Complete Translation Example

### Setting Up a New Translation

**Step 1: Create the language file**

```json
{# locales/es.json (Spanish) #}
{
  "general": {
    "shop_now": "Comprar Ahora",
    "learn_more": "Aprende Mas",
    "add_to_cart": "Agregar al Carrito"
  },
  "sections": {
    "header": {
      "menu": "Menu",
      "search": "Buscar",
      "cart": "Carrito"
    }
  },
  "cart": {
    "title": "Carrito de Compras",
    "empty": "Tu carrito esta vacio",
    "total": "Total"
  }
}
```

**Step 2: Create the schema file**

```json
{# locales/es.schema.json #}
{
  "sections": {
    "header": {
      "name": "Encabezado"
    },
    "hero_banner": {
      "name": "Banner Hero",
      "settings": {
        "title": {
          "label": "Titulo"
        },
        "description": {
          "label": "Descripcion"
        }
      }
    }
  }
}
```

**Step 3: Use in templates**

```liquid
<h1>{{ 'sections.hero_banner.settings.title.label' | t }}</h1>
<p>{{ 'general.shop_now' | t }}</p>
```

## Supported Languages

### Current Languages (30+)

```
English (en.default.json)
Spanish (es.json)
French (fr.json)
German (de.json)
Italian (it.json)
Portuguese (pt.json)
Russian (ru.json)
Chinese Simplified (zh.json)
Chinese Traditional (zh-Hans.json)
Japanese (ja.json)
Korean (ko.json)
Turkish (tr.json)
Polish (pl.json)
Dutch (nl.json)
Swedish (sv.json)
Norwegian (no.json)
Danish (da.json)
Finnish (fi.json)
Greek (el.json)
Czech (cs.json)
Hungarian (hu.json)
Romanian (ro.json)
Bulgarian (bg.json)
Croatian (hr.json)
Lithuanian (lt.json)
Latvian (lv.json)
Estonian (et.json)
Slovak (sk.json)
Slovenian (sl.json)
Vietnamese (vi.json)
Thai (th.json)
Indonesian (id.json)
```

## Best Practices

### 1. Consistent Key Naming

```json
{
  "sections": {
    "header": {
      "menu": "Menu",
      "menu_label": "Toggle Menu",
      "menu_close": "Close Menu"
    }
  }
}
```

### 2. Logical Organization

```json
{
  "general": {           {# General/global strings #}
    "404": { ... },
    "contact": { ... }
  },
  "sections": {          {# Section-specific strings #}
    "header": { ... },
    "footer": { ... }
  },
  "cart": { ... },       {# Feature-specific strings #}
  "product": { ... },
  "customer": { ... }
}
```

### 3. Pluralization Pattern

```json
{
  "items": "{{ count }} item",
  "items_plural": "{{ count }} items"
}
```

Or with explicit singular/plural:

```json
{
  "count_one": "1 item",
  "count_other": "{{ count }} items"
}
```

### 4. Translatable HTML

Keep HTML out of translations:

```liquid
{# Good #}
<h1>{{ 'sections.hero.title' | t }}</h1>

{# Avoid #}
{# Don't put HTML in translation #}
{{ 'sections.hero.title_html' | t }}
{# Where title_html contains "<strong>Bold Title</strong>" #}
```

### 5. Context for Translators

Add comments for context:

```json
{
  "_comment": "Generic action buttons",
  "general": {
    "shop_now": "Shop Now",
    "learn_more": "Learn More"
  },

  "_section_header_comment": "Main header/navigation section",
  "sections": {
    "header": {
      "menu": "Menu",
      "cart": "Cart"
    }
  }
}
```

### 6. Date Formats

Include locale-specific formats:

```json
{
  "formats": {
    "date_short": "MM/DD/YYYY",
    "date_long": "MMMM DD, YYYY",
    "time_short": "HH:mm",
    "time_long": "HH:mm:ss"
  }
}
```

### 7. Currency Display

Handle currency-specific formatting:

```liquid
{{ product.price | money }}  {# Uses store's currency #}

{# Custom formatting for different locales #}
{% if shop.currency == 'USD' %}
  ${{ product.price | divided_by: 100.0 }}
{% elsif shop.currency == 'EUR' %}
  €{{ product.price | divided_by: 100.0 }}
{% endif %}
```

## Common Patterns

### Product Page Translations

```liquid
<div class="product">
  <h1>{{ product.title }}</h1>

  {% if product.available %}
    <p>{{ 'product.in_stock' | t }}</p>
  {% else %}
    <p>{{ 'product.out_of_stock' | t }}</p>
  {% endif %}

  <span class="price">{{ product.price | money }}</span>

  <button>{{ 'general.add_to_cart' | t }}</button>
</div>
```

### Cart Notifications

```liquid
{# In cart-notification.js #}
class CartNotification extends HTMLElement {
  show(message) {
    this.innerHTML = message;
    this.classList.add('show');
  }
}

{# In global.js #}
cart.showNotification('{{ "general.added_to_cart" | t }}');
```

### Error Messages

```liquid
{% if product == blank %}
  <p>{{ 'product.not_found' | t }}</p>
{% endif %}

{% if customer == blank %}
  <p>{{ 'customer.login_required' | t }}</p>
{% endif %}
```

### Section Settings

```liquid
{% schema %}
{
  "name": "t:sections.featured_collection.name",
  "settings": [
    {
      "type": "collection",
      "id": "collection",
      "label": "t:sections.featured_collection.settings.collection.label"
    },
    {
      "type": "text",
      "id": "title",
      "label": "t:sections.featured_collection.settings.title.label",
      "default": "t:sections.featured_collection.settings.title.default"
    }
  ]
}
{% endschema %}
```

## Maintenance

### Managing Large Translation Files

Keep translations organized:

```json
{
  "_metadata": {
    "language": "en",
    "language_name": "English",
    "last_updated": "2024-01-15",
    "completeness": "100%"
  },
  "general": { ... },
  "sections": { ... }
}
```

### Tracking Translation Status

```json
{
  "_status": {
    "total_keys": 250,
    "translated": 248,
    "pending": 2
  }
}
```

### Finding Untranslated Strings

```liquid
{# Development helper - show untranslated keys #}
{% if settings.show_translation_keys %}
  {{ 'missing.key' | t }}  {# Will show: missing.key #}
{% endif %}
```

## Tools & Integrations

### Shopify Translation Apps

Popular translation management tools for Shopify:

- **Langify** - Multi-language support
- **Weglot** - Automatic translations
- **Translate.com** - Translation management
- **Localizer** - Language switcher

### Manual Translation Workflow

1. Export `en.default.json` as reference
2. Create new language file: `locales/[lang].json`
3. Translate all keys
4. Create `locales/[lang].schema.json`
5. Test in theme editor
6. Deploy

## Testing Translations

### Manual Testing

```liquid
{# Test specific language #}
{% if request.locale.iso_code == 'es' %}
  <!-- Test Spanish content -->
{% endif %}

{# Debug translations #}
{{ 'general.shop_now' | t }}
{{ 'sections.header.menu' | t }}
```

### Language Switcher Implementation

```liquid
{# Add language selector to footer #}
<select id="language-select">
  {% for locale in shop.published_locales %}
    <option
      value="{{ locale.iso_code }}"
      {% if locale.iso_code == request.locale.iso_code %}selected{% endif %}
    >
      {{ locale.endonym_name }}
    </option>
  {% endfor %}
</select>

{%- javascript -%}
  document.getElementById('language-select').addEventListener('change', (e) => {
    window.location = '/' + e.target.value + window.location.pathname;
  });
{%- endjavascript -%}
```

## Adding a New Language

### Step-by-Step Process

1. **Create translation file**

```bash
cp locales/en.default.json locales/[lang].json
```

2. **Translate all keys**

```json
{
  "general": {
    "shop_now": "[Translation here]",
    ...
  }
}
```

3. **Create schema file**

```bash
cp locales/en.schema.json locales/[lang].schema.json
```

4. **Translate schema**

```json
{
  "sections": {
    "header": {
      "name": "[Translation here]"
    }
  }
}
```

5. **Test in theme editor**

- Load theme in Shopify Admin
- Switch language in store settings
- Verify all text translates correctly
- Check mobile responsiveness

6. **Enable in Shopify**

- Go to Shopify Admin > Settings > Languages
- Enable new language
- Set as available language option

## Resources

- [Shopify Translation Documentation](https://shopify.dev/themes/internationalization)
- [Liquid t Filter Reference](https://shopify.dev/api/liquid/filters#t)
- [ISO 639-1 Language Codes](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)
- [Unicode CLDR](http://cldr.unicode.org) - Language/locale data

