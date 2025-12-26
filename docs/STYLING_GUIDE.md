# Styling Architecture Guide for Kiswa Noir

This guide covers the CSS and Tailwind styling patterns used in Kiswa Noir theme.

## Overview

Kiswa Noir uses a **utility-first approach** with Tailwind CSS 3.x as the primary styling framework. Custom CSS is used sparingly for complex layouts and animations.

### Tech Stack
- **Framework**: Tailwind CSS 3.x
- **Compiled Output**: `assets/tailwind.css` (80KB minified)
- **Custom CSS**: Minimal custom styles in section `<style>` tags
- **Approach**: Mobile-first, responsive design

## Tailwind CSS Fundamentals

### Utility Classes

Tailwind provides utility classes for common styling needs:

```html
<!-- Spacing -->
<div class="m-4">Margin 1rem</div>
<div class="p-6">Padding 1.5rem</div>
<div class="mt-8">Margin-top 2rem</div>
<div class="px-4 py-2">Padding X 1rem, Y 0.5rem</div>

<!-- Display & Layout -->
<div class="flex">Flexbox</div>
<div class="grid grid-cols-3">3-column grid</div>
<div class="block md:inline-block">Display block, inline on tablet+</div>
<div class="hidden lg:block">Hidden, visible on desktop</div>

<!-- Typography -->
<h1 class="text-4xl font-bold">Large bold text</h1>
<p class="text-sm text-gray-600">Small gray text</p>
<span class="uppercase tracking-wider">Uppercase with letter spacing</span>
<p class="line-clamp-2">Max 2 lines with ellipsis</p>

<!-- Colors -->
<div class="bg-white text-black">White background, black text</div>
<div class="bg-black text-white">Black background, white text</div>
<div class="text-blue-600 hover:text-blue-800">Blue text, darker on hover</div>
<div class="border-2 border-gray-300">Gray border</div>

<!-- Sizing -->
<div class="w-full">Full width</div>
<div class="w-1/2 md:w-2/3">50% width, 66% on tablet+</div>
<div class="h-64">256px height</div>
<div class="min-h-screen">Full viewport height minimum</div>

<!-- Effects -->
<div class="opacity-75 hover:opacity-100">75% opacity, full on hover</div>
<div class="shadow-md hover:shadow-lg">Medium shadow, larger on hover</div>
<div class="rounded-lg">Rounded corners</div>
<div class="transition duration-300 ease-in-out">Smooth transitions</div>
```

## Responsive Breakpoints

Kiswa Noir uses mobile-first responsive design:

```html
<!-- Default (mobile) -->
<div class="text-sm">

<!-- Tablet and up (768px) -->
<div class="md:text-base">

<!-- Desktop and up (1024px) -->
<div class="lg:text-lg">

<!-- Large desktop and up (1280px) -->
<div class="xl:text-xl">
```

### Common Breakpoints

| Breakpoint | Size | CSS |
|-----------|------|-----|
| sm | 640px | `sm:` |
| md | 768px | `md:` |
| lg | 1024px | `lg:` |
| xl | 1280px | `xl:` |
| 2xl | 1536px | `2xl:` |

## Kiswa Noir Color System

### Brand Colors

```html
<!-- Primary (Black) -->
<div class="text-black bg-black">Primary brand color</div>

<!-- Secondary (White) -->
<div class="text-white bg-white">Secondary/background</div>

<!-- Neutral grays -->
<div class="text-gray-50">Almost white</div>
<div class="text-gray-900">Almost black</div>

<!-- Accent colors (fashion-forward) -->
<div class="text-red-600">Accent red</div>
<div class="text-gray-600">Neutral gray</div>
```

### Using Color System

```liquid
<!-- In Liquid templates -->
<div style="color: {{ section.settings.text_color }}">
  Customizable text color
</div>

<div class="text-black dark:text-white">
  Black text, white in dark mode
</div>
```

## Layout Patterns

### Flexbox Layouts

```html
<!-- Horizontal layout -->
<div class="flex gap-4">
  <div class="flex-1">Item 1</div>
  <div class="flex-1">Item 2</div>
  <div class="flex-1">Item 3</div>
</div>

<!-- Centered content -->
<div class="flex items-center justify-center min-h-screen">
  <div>Centered content</div>
</div>

<!-- Space between -->
<div class="flex justify-between items-center">
  <h1>Title</h1>
  <button>Action</button>
</div>

<!-- Vertical stack (column is default in mobile) -->
<div class="flex flex-col md:flex-row gap-6">
  <aside class="w-full md:w-64">Sidebar</aside>
  <main class="flex-1">Content</main>
</div>
```

### Grid Layouts

```html
<!-- Responsive grid -->
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
  <div>Product 1</div>
  <div>Product 2</div>
  <div>Product 3</div>
  <div>Product 4</div>
</div>

<!-- Auto-fit grid -->
<div class="grid gap-4 grid-cols-[repeat(auto-fit,minmax(200px,1fr))]">
  <div>Item</div>
  <div>Item</div>
</div>

<!-- Two column equal width -->
<div class="grid grid-cols-2 gap-8">
  <div>Left</div>
  <div>Right</div>
</div>
```

### Container Queries

```html
<!-- Contain width for responsive sizing -->
<div class="max-w-4xl mx-auto px-4">
  <h1 class="text-3xl md:text-5xl">Title</h1>
  <p class="text-base md:text-lg text-gray-600">Description</p>
</div>
```

## Typography

### Font Sizes

```html
<h1 class="text-4xl md:text-5xl lg:text-6xl font-bold">
  Large heading
</h1>

<h2 class="text-2xl md:text-3xl font-semibold">
  Section heading
</h2>

<h3 class="text-xl font-semibold">
  Subsection
</h3>

<p class="text-base md:text-lg">
  Body text
</p>

<small class="text-sm text-gray-600">
  Small text
</small>
```

### Font Weights

```html
<p class="font-light">Thin text (300)</p>
<p class="font-normal">Normal text (400)</p>
<p class="font-semibold">Semibold text (600)</p>
<p class="font-bold">Bold text (700)</p>
<p class="font-extrabold">Very bold text (800)</p>
```

### Line Height

```html
<p class="leading-tight">Tighter line spacing (1.25)</p>
<p class="leading-normal">Normal line spacing (1.5)</p>
<p class="leading-relaxed">More relaxed (1.625)</p>
<p class="leading-loose">Very relaxed (2)</p>
```

### Text Styles

```html
<!-- Color -->
<p class="text-black">Black text</p>
<p class="text-gray-600">Gray text</p>
<p class="text-white">White text</p>

<!-- Alignment -->
<p class="text-left">Left aligned</p>
<p class="text-center">Centered</p>
<p class="text-right">Right aligned</p>
<p class="text-justify">Justified</p>

<!-- Transform -->
<p class="uppercase">UPPERCASE TEXT</p>
<p class="lowercase">lowercase text</p>
<p class="capitalize">Capitalize Each Word</p>

<!-- Decoration -->
<p class="underline">Underlined text</p>
<p class="line-through">Struck through</p>
<p class="no-underline">No underline</p>

<!-- Tracking (letter spacing) -->
<p class="tracking-tight">Tight spacing</p>
<p class="tracking-normal">Normal spacing</p>
<p class="tracking-wide">Wide spacing</p>
<p class="tracking-widest">Extra wide spacing</p>
```

## Components

### Buttons

```html
<!-- Primary button -->
<button class="px-6 py-2 bg-black text-white rounded hover:bg-gray-800 transition">
  Click me
</button>

<!-- Secondary button -->
<button class="px-6 py-2 bg-white text-black border border-black rounded hover:bg-gray-100 transition">
  Secondary
</button>

<!-- Ghost button -->
<button class="px-6 py-2 text-black underline hover:no-underline transition">
  Ghost
</button>

<!-- Button with icon -->
<button class="flex items-center gap-2 px-4 py-2 bg-black text-white rounded">
  <svg class="w-5 h-5">...</svg>
  <span>Button</span>
</button>

<!-- Disabled button -->
<button disabled class="px-6 py-2 bg-gray-300 text-gray-500 cursor-not-allowed">
  Disabled
</button>

<!-- Size variants -->
<button class="px-3 py-1 text-sm">Small</button>
<button class="px-4 py-2 text-base">Medium</button>
<button class="px-6 py-3 text-lg">Large</button>
```

### Cards

```html
<!-- Basic card -->
<div class="bg-white rounded-lg shadow-md p-6">
  <h3 class="text-xl font-semibold mb-4">Card Title</h3>
  <p class="text-gray-600">Card content goes here</p>
</div>

<!-- Product card -->
<div class="bg-white rounded-lg overflow-hidden shadow hover:shadow-lg transition">
  <div class="aspect-square bg-gray-100">
    <img src="image.jpg" alt="Product" class="w-full h-full object-cover">
  </div>
  <div class="p-4">
    <h3 class="font-semibold">Product Name</h3>
    <p class="text-gray-600">$99.00</p>
    <button class="mt-4 w-full bg-black text-white py-2 rounded">
      Add to Cart
    </button>
  </div>
</div>

<!-- Card with border -->
<div class="border border-gray-200 rounded-lg p-6">
  <h3 class="font-semibold mb-2">Bordered Card</h3>
  <p class="text-gray-600">Content with border</p>
</div>

<!-- Gradient card -->
<div class="bg-gradient-to-r from-gray-900 to-black text-white rounded-lg p-6">
  <h3 class="font-semibold">Gradient Card</h3>
</div>
```

### Forms

```html
<!-- Text input -->
<input
  type="text"
  placeholder="Enter text"
  class="w-full px-4 py-2 border border-gray-300 rounded focus:outline-none focus:border-black transition"
>

<!-- Select dropdown -->
<select class="w-full px-4 py-2 border border-gray-300 rounded focus:outline-none focus:border-black">
  <option>Option 1</option>
  <option>Option 2</option>
</select>

<!-- Textarea -->
<textarea
  placeholder="Enter message"
  class="w-full px-4 py-2 border border-gray-300 rounded focus:outline-none focus:border-black transition"
  rows="4"
></textarea>

<!-- Checkbox -->
<input type="checkbox" class="w-4 h-4 rounded border-gray-300">

<!-- Radio -->
<input type="radio" class="w-4 h-4 border-gray-300">

<!-- Form group -->
<div class="mb-4">
  <label class="block text-sm font-semibold mb-2">Label</label>
  <input type="text" class="w-full px-4 py-2 border border-gray-300 rounded">
</div>
```

## Spacing System

```html
<!-- Margin: m[t|r|b|l]-[size] -->
<div class="mt-4 mr-6 mb-8 ml-2">Margin on all sides</div>

<!-- Padding: p[t|r|b|l]-[size] -->
<div class="pt-4 pr-6 pb-8 pl-2">Padding on all sides</div>

<!-- Gap (flexbox/grid) -->
<div class="flex gap-4">Flex gap</div>
<div class="grid gap-6">Grid gap</div>

<!-- Spacing values -->
0 = 0px
1 = 0.25rem (4px)
2 = 0.5rem (8px)
3 = 0.75rem (12px)
4 = 1rem (16px)
6 = 1.5rem (24px)
8 = 2rem (32px)
12 = 3rem (48px)
16 = 4rem (64px)
```

## Hover & Interaction States

```html
<!-- Hover -->
<button class="bg-black text-white hover:bg-gray-800">
  Hover button
</button>

<!-- Focus -->
<input class="border-2 border-gray-300 focus:border-black focus:outline-none">

<!-- Active -->
<button class="bg-white text-black active:bg-gray-100">
  Active button
</button>

<!-- Disabled -->
<button disabled class="bg-gray-300 text-gray-500 cursor-not-allowed">
  Disabled
</button>

<!-- Group states (hover parent) -->
<div class="group">
  <div class="group-hover:text-red-600">
    This changes color when parent is hovered
  </div>
</div>

<!-- Focus within -->
<div class="focus-within:ring-2 focus-within:ring-black">
  <input type="text">
</div>
```

## Animations

```html
<!-- Transition -->
<div class="transition duration-300 ease-in-out hover:opacity-50">
  Smooth transition
</div>

<!-- Fade in -->
<div class="animate-fadeIn">Fade in animation</div>

<!-- Slide in -->
<div class="transform translate-x-0 transition duration-500">
  Slide in
</div>

<!-- Custom animation -->
<style>
  @keyframes slideUp {
    from {
      opacity: 0;
      transform: translateY(10px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  .animate-slideUp {
    animation: slideUp 0.3s ease-out;
  }
</style>
<div class="animate-slideUp">Slides up</div>
```

## Kiswa Noir Specific Classes

### Custom Utility Classes

```css
/* In tailwind.css or section styles */

.btn-primary {
  @apply px-6 py-2 bg-black text-white rounded transition hover:bg-gray-800;
}

.section-spacing {
  @apply py-12 md:py-20 lg:py-32;
}

.container-max {
  @apply max-w-6xl mx-auto px-4;
}

.text-gradient {
  @apply bg-gradient-to-r from-gray-900 to-black bg-clip-text text-transparent;
}

.card {
  @apply bg-white rounded-lg shadow-md p-6 transition hover:shadow-lg;
}

.badge {
  @apply inline-block px-3 py-1 bg-black text-white text-xs font-semibold rounded-full;
}
```

### Responsive Container

```html
<div class="container-max">
  <h1>Max width content</h1>
</div>
```

### Hero Section

```html
<section class="relative h-screen md:h-[600px] bg-cover bg-center" style="background-image: url(...)">
  <div class="absolute inset-0 bg-black/50"></div>
  <div class="relative h-full flex items-center justify-center">
    <div class="text-center text-white">
      <h1 class="text-5xl md:text-7xl font-bold mb-4">Title</h1>
      <p class="text-xl md:text-2xl mb-8">Subtitle</p>
      <button class="btn-primary">Action</button>
    </div>
  </div>
</section>
```

## Best Practices

### 1. Use Utility Classes First

```html
<!-- Good -->
<div class="flex items-center justify-between p-4 bg-white">
  <h1 class="text-2xl font-bold">Title</h1>
  <button class="px-4 py-2 bg-black text-white rounded">Action</button>
</div>

<!-- Avoid -->
<div class="header">
  <h1>Title</h1>
  <button>Action</button>
</div>
```

### 2. Responsive Design Mobile-First

```html
<!-- Good -->
<div class="w-full md:w-1/2 lg:w-1/3">
  Mobile full width, grows smaller on larger screens
</div>

<!-- Avoid -->
<div class="w-1/3 sm:w-1/2 md:w-full">
  Shrinks on smaller screens
</div>
```

### 3. Component Composition

```html
<!-- Good - Combine utilities for components -->
<button class="px-6 py-2 bg-black text-white rounded hover:bg-gray-800 transition">
  Primary Button
</button>

<!-- Better - Use @apply for reuse -->
<!-- See custom classes above -->
<button class="btn-primary">Primary Button</button>
```

### 4. Avoid Magic Numbers

```html
<!-- Good -->
<div class="gap-4"><!-- 16px gap --></div>

<!-- Avoid -->
<div style="gap: 15px"><!-- Non-standard spacing --></div>
```

### 5. Use CSS Variables for Theming

```css
:root {
  --primary-color: #000000;
  --secondary-color: #ffffff;
  --text-color: #000000;
  --border-color: #cccccc;
}

.btn {
  background-color: var(--primary-color);
  color: var(--secondary-color);
}
```

### 6. Dark Mode Support

```html
<!-- Add dark: prefix for dark mode -->
<div class="bg-white dark:bg-gray-900 text-black dark:text-white">
  Adapts to dark mode
</div>
```

### 7. Optimize Images

```html
<!-- Proper image sizing -->
<img
  src="image.jpg"
  srcset="image-sm.jpg 600w, image-lg.jpg 1200w"
  sizes="(max-width: 768px) 100vw, 50vw"
  alt="Description"
  class="w-full h-auto"
>
```

### 8. Accessibility

```html
<!-- Good contrast -->
<p class="text-gray-900 bg-white">High contrast (AAA)</p>

<!-- Focus states -->
<button class="focus:outline-none focus:ring-2 focus:ring-black">
  Focusable button
</button>

<!-- Semantic HTML -->
<nav role="navigation">Navigation</nav>
<main role="main">Main content</main>
```

## Debugging

### Display Utilities

```html
<!-- Debug spacing -->
<div class="border-2 border-red-500 p-4">
  Padding shown with red border
</div>

<!-- Debug grid -->
<div class="grid grid-cols-3 gap-4 bg-gray-100">
  <div class="bg-blue-100">1</div>
  <div class="bg-blue-200">2</div>
  <div class="bg-blue-300">3</div>
</div>

<!-- Debug flexbox -->
<div class="flex gap-4 bg-yellow-100">
  <div class="bg-yellow-300">Item 1</div>
  <div class="bg-yellow-400">Item 2</div>
</div>
```

## Resources

- [Tailwind CSS Docs](https://tailwindcss.com/docs)
- [Tailwind UI Components](https://tailwindui.com)
- [CSS-Tricks Flexbox Guide](https://css-tricks.com/snippets/css/a-guide-to-flexbox)
- [CSS-Tricks Grid Guide](https://css-tricks.com/snippets/css/complete-guide-grid)

