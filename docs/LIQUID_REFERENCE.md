# Liquid Template Reference for Kiswa Noir

Liquid is Shopify's template language. This guide covers the Liquid syntax used throughout the theme.

## Basic Syntax

### Comments

```liquid
{# This is a comment and won't appear in output #}
```

### Output Variables

```liquid
{{ variable }}
{{ object.property }}
{{ product.title }}
```

### Filters

Filters modify variables:

```liquid
{{ 'text' | upcase }}          {# TEXT #}
{{ 'text' | capitalize }}      {# Text #}
{{ 10.4 | ceil }}              {# 11 #}
{{ 24.4 | floor }}             {# 24 #}
{{ 'hello world' | split: ' ' }}{# ["hello", "world"] #}
```

### Tags

Control logic:

```liquid
{% if condition %}
  output
{% elsif other_condition %}
  other output
{% else %}
  default output
{% endif %}
```

## Control Structures

### If/Elsif/Else

```liquid
{% if product.available %}
  <p>In stock!</p>
{% elsif product.on_sale %}
  <p>On sale!</p>
{% else %}
  <p>Out of stock</p>
{% endif %}
```

### Unless (Opposite of If)

```liquid
{% unless product.available %}
  <p>Out of stock</p>
{% endunless %}
```

### Case/When

```liquid
{% case product.type %}
  {% when 'Shirt' %}
    <p>This is a shirt</p>
  {% when 'Shoes' %}
    <p>These are shoes</p>
  {% else %}
    <p>Other product</p>
{% endcase %}
```

### For Loops

```liquid
{% for product in collection.products %}
  <div>{{ product.title }}</div>
{% endfor %}
```

Loop with counter:

```liquid
{% for product in collection.products %}
  <div>{{ forloop.index }}: {{ product.title }}</div>
{% endfor %}
```

Loop variables available:
- `forloop.index` - Current iteration (1-indexed)
- `forloop.index0` - Current iteration (0-indexed)
- `forloop.first` - True if first iteration
- `forloop.last` - True if last iteration
- `forloop.length` - Total iterations
- `forloop.rindex` - Count backwards from end

### Limit and Offset

```liquid
{% for product in collection.products limit: 4 offset: 0 %}
  {{ product.title }}
{% endfor %}
```

### Reversed

```liquid
{% for product in collection.products reversed %}
  {{ product.title }}
{% endfor %}
```

## Include Snippets

Include reusable partials:

```liquid
{% include 'snippet-name' %}
```

With variables:

```liquid
{% include 'product-card' with product: item %}
```

Multiple variables:

```liquid
{% include 'button' with text: 'Click me', color: 'red' %}
```

Variable scope:

```liquid
{% include 'snippet' with my_var as product %}
{# Inside snippet, use: {{ product }} #}
```

## Common Filters

### String Filters

```liquid
{{ 'hello' | upcase }}         {# HELLO #}
{{ 'HELLO' | downcase }}       {# hello #}
{{ 'hello' | capitalize }}     {# Hello #}
{{ 'hello world' | first }}    {# h #}
{{ 'hello world' | last }}     {# d #}
{{ 'hello world' | size }}     {# 11 #}
{{ 'hello world' | split: ' ' }}{# ["hello", "world"] #}
{{ 'a,b,c' | join: ' ' }}     {# a b c #}
{{ 'hello' | replace: 'l', 'L' }}{# heLLo #}
{{ 'hello' | remove: 'l' }}    {# heo #}
{{ ' hello ' | strip }}        {# hello #}
{{ ' hello ' | lstrip }}       {# hello (right space remains) #}
```

### Math Filters

```liquid
{{ 4.6 | ceil }}               {# 5 #}
{{ 4.6 | floor }}              {# 4 #}
{{ 4.6 | round }}              {# 5 #}
{{ 4.6 | round: 1 }}           {# 4.6 #}
{{ 5 | plus: 3 }}              {# 8 #}
{{ 5 | minus: 3 }}             {# 2 #}
{{ 5 | times: 3 }}             {# 15 #}
{{ 15 | divided_by: 3 }}       {# 5 #}
{{ 15 | modulo: 4 }}           {# 3 #}
{{ -5 | abs }}                 {# 5 #}
{{ -5 | abs | ceil }}          {# 5 #}
```

### Date Filters

```liquid
{{ 'now' | date: '%Y-%m-%d' }} {# 2024-01-15 #}
{{ 'now' | date: '%B %d, %Y' }}{# January 15, 2024 #}
{{ product.created_at | date: '%Y-%m-%d %H:%M' }}
```

### Array Filters

```liquid
{{ array | first }}            {# First item #}
{{ array | last }}             {# Last item #}
{{ array | size }}             {# Array length #}
{{ array | join: ', ' }}       {# Item1, Item2, Item3 #}
{{ array | map: 'title' }}     {# Extract property from each #}
{{ array | where: 'available' }}{# Filter array items #}
{{ array | sort }}             {# Sort alphabetically #}
{{ array | uniq }}             {# Remove duplicates #}
```

### Money Filters

```liquid
{{ product.price | money }}    {# $19.99 #}
{{ product.price | money_with_currency }}  {# $19.99 USD #}
{{ product.price | money_without_currency }}{# 19.99 #}
```

### URL/Image Filters

```liquid
{{ 'hello world' | url_encode }} {# hello%20world #}
{{ 'hello world' | url_decode }} {# hello world #}
{{ product.featured_image | image_url: width: 500 }}
{{ product.featured_image | image_tag: alt: product.title }}
{{ 'myimage.jpg' | img_url }}
{{ 'myimage.jpg' | img_url: '200x' }}
```

### Translation Filter

```liquid
{{ 'general.404.title' | t }}
{{ 'cart.quantity' | t: count: 5 }}
```

### Other Common Filters

```liquid
{{ value | default: 'fallback' }} {# Use if value is nil/false #}
{{ 'text' | slice: 0, 4 }}       {# substr(0, 4) #}
{{ 'hello' | append: ' world' }} {# hello world #}
{{ 'hello' | prepend: 'say ' }}  {# say hello #}
```

## Shopify Objects

### Product Object

```liquid
{{ product.title }}
{{ product.description }}
{{ product.price }}
{{ product.available }}
{{ product.handle }}
{{ product.url }}
{{ product.collections }}
{{ product.featured_image }}
{{ product.images }}
{{ product.variants }}
{{ product.created_at }}
{{ product.vendor }}
{{ product.type }}
{{ product.tags }}
```

### Collection Object

```liquid
{{ collection.title }}
{{ collection.description }}
{{ collection.handle }}
{{ collection.url }}
{{ collection.image }}
{{ collection.products }}
{{ collection.products_count }}
{{ collection.all_products_count }}
```

### Cart Object

```liquid
{{ cart.item_count }}
{{ cart.total_price }}
{{ cart.items }}
{{ cart.checkout_url }}
```

### Customer Object

```liquid
{{ customer.first_name }}
{{ customer.last_name }}
{{ customer.email }}
{{ customer.orders_count }}
{{ customer.total_spent }}
{{ customer.addresses }}
```

### Settings Object

```liquid
{{ settings.footer_color }}
{{ settings.logo_image }}
{{ settings.show_newsletter }}
```

### Section Object

```liquid
{{ section.id }}
{{ section.settings.title }}
{{ section.blocks }}
```

### Theme Object

```liquid
{{ theme.name }}
{{ theme.id }}
{{ theme.store }}
```

## Kisswa Noir Common Patterns

### Translation Pattern

All user-facing text should be translatable:

```liquid
{# Good #}
<h1>{{ 'sections.featured_collection.title' | t }}</h1>
<button>{{ 'general.shop_now' | t }}</button>

{# Avoid #}
<h1>Featured Collection</h1>
<button>Shop Now</button>
```

### Product Grid

```liquid
<div class="grid grid-cols-2 md:grid-cols-3 gap-4">
  {%- for product in collection.products -%}
    <a href="{{ product.url }}" class="product-card">
      {%- if product.featured_image -%}
        {{
          product.featured_image
          | image_url: width: 500
          | image_tag:
            alt: product.title,
            loading: 'lazy'
        }}
      {%- endif -%}
      <h3>{{ product.title }}</h3>
      <p class="price">{{ product.price | money }}</p>
    </a>
  {%- endfor -%}
</div>
```

### Section with Settings

```liquid
<section id="section-{{ section.id }}" class="py-12">
  <h2>{{ section.settings.title }}</h2>
  <p style="color: {{ section.settings.text_color }};">
    {{ section.settings.description }}
  </p>
</section>

{%- style -%}
  #section-{{ section.id }} {
    background-color: {{ section.settings.bg_color }};
  }
{%- endstyle -%}
```

### Conditional Rendering

```liquid
{%- if product.available -%}
  <button>Add to Cart</button>
{%- elsif product.on_sale -%}
  <span>On Sale!</span>
{%- else -%}
  <button disabled>Out of Stock</button>
{%- endif -%}
```

### Loop with Break/Continue

```liquid
{%- for product in collection.products -%}
  {%- if forloop.index > 3 -%}
    {%- break -%}
  {%- endif -%}

  {{ product.title }}
{%- endfor -%}
```

### Image Optimization

```liquid
{{
  product.featured_image
  | image_url: width: 1200
  | image_tag:
    alt: product.title,
    loading: 'lazy',
    sizes: '(max-width: 768px) 100vw, 50vw',
    widths: '300, 500, 800, 1200'
}}
```

### Money Display

```liquid
<p class="price">{{ product.price | money }}</p>
<p class="compare">{{ product.compare_at_price | money }}</p>
<p class="sale-price" style="color: red;">
  Sale: {{ product.price | money }}
</p>
```

### Cart Handling

```liquid
{% if cart.item_count > 0 %}
  <p>Items in cart: {{ cart.item_count }}</p>
  <p>Total: {{ cart.total_price | money }}</p>
  <a href="{{ cart.url }}">View Cart</a>
{% else %}
  <p>Your cart is empty</p>
{% endif %}
```

### Collection Loop

```liquid
{%- for collection in shop.collections limit: 4 -%}
  <a href="{{ collection.url }}">
    {{ collection.title }}
  </a>
{%- endfor -%}
```

### Responsive Image

```liquid
{%- if section.settings.image -%}
  {{
    section.settings.image
    | image_url: width: 800
    | image_tag:
      class: 'w-full h-auto',
      alt: section.settings.image.alt,
      loading: 'lazy'
  }}
{%- endif -%}
```

## Best Practices

### 1. Use Semantic Variables

```liquid
{# Good #}
{% for product in collection.products %}
  {{ product.title }}
{% endfor %}

{# Avoid #}
{% for item in collection.products %}
  {{ item.title }}
{% endfor %}
```

### 2. Filter Early, Display Last

```liquid
{# Good #}
{% assign featured = collection.products | where: 'tags' contains: 'featured' %}
{% for product in featured %}
  {{ product.title }}
{% endfor %}

{# Avoid #}
{% for product in collection.products %}
  {% if product.tags contains 'featured' %}
    {{ product.title }}
  {% endif %}
{% endfor %}
```

### 3. Use Assign for Complex Values

```liquid
{# Good #}
{% assign product_url = product.url %}
<a href="{{ product_url }}">View</a>

{# Avoid #}
<a href="{{ product.url }}">View</a>
```

### 4. Cache When Possible

```liquid
{% paginate collection.products by 12 %}
  ...
{% endpaginate %}
```

### 5. Always Include Alt Text

```liquid
{{
  product.featured_image
  | image_tag:
    alt: product.title,
    loading: 'lazy'
}}
```

### 6. Use Translated Strings

```liquid
{# Good #}
<h1>{{ 'sections.hero.title' | t }}</h1>

{# Avoid #}
<h1>Welcome</h1>
```

### 7. Optimize Image URLs

```liquid
{# Good - Specify width for optimization #}
{{ product.featured_image | image_url: width: 500 }}

{# Avoid - No optimization #}
{{ product.featured_image.src }}
```

### 8. Use DRY Principles

```liquid
{# Good - Reuse with snippet #}
{% include 'product-card' with product: item %}

{# Avoid - Repeat HTML multiple times #}
<div class="card">...</div>
<div class="card">...</div>
<div class="card">...</div>
```

## Common Errors

### 1. Undefined Variables

```liquid
{# Error: section doesn't exist in this context #}
{{ section.settings.title }}

{# Solution: Check context or provide default #}
{{ section.settings.title | default: 'No title' }}
```

### 2. Nil/Empty Values

```liquid
{# Error: Can't access title on nil #}
{{ product.title }}

{# Solution: Check existence first #}
{% if product %}
  {{ product.title }}
{% endif %}
```

### 3. Type Mismatch

```liquid
{# Error: Can't iterate string #}
{% for item in 'hello' %}

{# Solution: Ensure you're iterating array #}
{% for item in array %}
```

### 4. Filter Order

```liquid
{# Wrong order #}
{{ product.price | split: ',' | money }}

{# Correct - filters are applied left to right #}
{{ 'price,name' | split: ',' | first }}
```

## Debug Techniques

### Output Variables

```liquid
{# Print to page #}
<pre>{{ section | json }}</pre>
```

### Conditional Debug

```liquid
{% if settings.debug_mode %}
  <div class="debug">
    {{ collection.title }}
  </div>
{% endif %}
```

### Browser Console

```liquid
{%- javascript -%}
  console.log('Product:', {{ product | json }});
{%- endjavascript -%}
```

## Resources

- [Shopify Liquid Reference](https://shopify.dev/api/liquid)
- [Liquid Cheat Sheet](https://cheat.kapeli.com/cheatsheets/Shopify_Liquid.docset/Contents/Resources/Documents/index)
- [Theme Development Docs](https://shopify.dev/themes)

