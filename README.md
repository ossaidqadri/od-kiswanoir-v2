# Kiswa Noir - Shopify Theme Documentation

Welcome to the Kiswa Noir theme documentation. This is a modern, minimalist Shopify theme built with Liquid, Tailwind CSS, and vanilla JavaScript.

**Brand Philosophy**: "Minimalism - Accessible design by producing less & building better"

## Quick Start

### 1. Prerequisites

- Shopify store (development or production)
- Shopify CLI installed: `npm install -g @shopify/cli`
- Node.js 16+ (for asset compilation)
- Git (for version control)

### 2. Installation

#### Via Shopify Admin

1. Go to **Online Store > Themes**
2. Click **Add theme**
3. Upload or link the Kiswa Noir theme
4. Click **Customize** to configure

#### Via Shopify CLI (Local Development)

```bash
# Clone the theme repository
git clone https://github.com/yourusername/kiswa-noir.git
cd kiswa-noir

# Log in to your Shopify store
shopify login --store my-store.myshopify.com

# Serve theme locally
shopify theme serve

# Theme available at: https://127.0.0.1:9292
```

### 3. Initial Configuration

1. Open theme editor in Shopify Admin
2. Configure header with logo
3. Set brand colors and typography
4. Add social media links
5. Configure footer content
6. Customize homepage sections

## Project Structure

```
kiswa/
├── docs/                      # Documentation files
│   ├── ARCHITECTURE.md        # Overall system design
│   ├── SECTIONS_GUIDE.md      # Section development
│   ├── LIQUID_REFERENCE.md    # Liquid syntax reference
│   ├── JAVASCRIPT_GUIDE.md    # JavaScript patterns
│   ├── STYLING_GUIDE.md       # CSS/Tailwind patterns
│   ├── I18N_GUIDE.md          # Translation guide
│   ├── CUSTOMIZATION_GUIDE.md # Theme editor guide
│   └── README.md              # This file
│
├── layout/                    # HTML template wrappers
│   ├── theme.liquid           # Main layout
│   └── password.liquid        # Password protection
│
├── sections/                  # Reusable components
│   ├── header.liquid
│   ├── footer.liquid
│   ├── hero-collage.liquid
│   ├── featured-collection.liquid
│   └── ... (21 total)
│
├── snippets/                  # Reusable partials
│   ├── theme-head.liquid
│   ├── menu.liquid
│   ├── icons.liquid
│   └── ... (17 total)
│
├── templates/                 # Page type layouts
│   ├── index.json             # Homepage
│   ├── product.json           # Product detail
│   ├── collection.json        # Category
│   ├── cart.json              # Shopping cart
│   └── ... (13 total)
│
├── assets/                    # CSS & JavaScript
│   ├── tailwind.css           # Main stylesheet
│   ├── customer.css           # Customer pages
│   ├── global.js              # Core JavaScript
│   ├── cart.js                # Cart components
│   └── swiper.min.js          # Carousel library
│
├── config/                    # Configuration
│   ├── settings_schema.json   # Theme editor settings
│   └── settings_data.json     # Current settings
│
├── locales/                   # Multi-language support
│   ├── en.default.json        # English (base)
│   ├── es.json, fr.json       # Other languages
│   └── *.schema.json          # Translatable settings
│
└── Root files
    ├── favicon.svg
    ├── logo.svg
    └── .gitignore
```

## Technology Stack

| Layer | Technology | Version |
|-------|-----------|---------|
| **Templating** | Shopify Liquid | Latest |
| **Styling** | Tailwind CSS | 3.x |
| **JavaScript** | Vanilla ES6+ | No frameworks |
| **Web Components** | Native Custom Elements | - |
| **Carousels** | Swiper.js | Latest |
| **Fonts** | Google Fonts | Great Vibes |

## Key Features

- **30+ Language Support** - Comprehensive i18n system
- **Responsive Design** - Mobile-first approach
- **Accessibility** - WCAG 2.1 AA compliance
- **Performance** - Optimized assets, lazy loading
- **Customizable** - Extensive theme editor settings
- **Web Components** - Modern interactive patterns
- **SEO Optimized** - Proper meta tags, structured data
- **Cart Management** - AJAX add-to-cart functionality

## Documentation Guide

### For Theme Customization
- Start with: **[CUSTOMIZATION_GUIDE.md](docs/CUSTOMIZATION_GUIDE.md)**
- Learn how to use the theme editor
- Configure colors, fonts, header/footer
- Add/remove/reorder homepage sections

### For Development

1. **Architecture Overview**: [ARCHITECTURE.md](docs/ARCHITECTURE.md)
   - Project structure and organization
   - Technology stack overview
   - Core concepts and data flow

2. **Liquid Templates**: [LIQUID_REFERENCE.md](docs/LIQUID_REFERENCE.md)
   - Liquid syntax reference
   - Shopify objects and filters
   - Common patterns

3. **Building Sections**: [SECTIONS_GUIDE.md](docs/SECTIONS_GUIDE.md)
   - Section anatomy
   - Schema settings
   - Blocks and repeating content
   - Best practices with examples

4. **Styling**: [STYLING_GUIDE.md](docs/STYLING_GUIDE.md)
   - Tailwind CSS utility classes
   - Layout patterns
   - Component styles
   - Responsive design

5. **JavaScript**: [JAVASCRIPT_GUIDE.md](docs/JAVASCRIPT_GUIDE.md)
   - Web Components patterns
   - Cart management
   - Event handling
   - Performance optimization

6. **Translations**: [I18N_GUIDE.md](docs/I18N_GUIDE.md)
   - Translation file structure
   - Using translations in templates
   - Supporting new languages
   - Schema translations

## Common Tasks

### Add a New Section

1. Create `sections/my-section.liquid`
2. Add HTML, CSS (in `<style>` tag), and JavaScript
3. Define schema with editor settings
4. Add to page template JSON

See [SECTIONS_GUIDE.md](docs/SECTIONS_GUIDE.md) for complete example.

### Customize Header

1. Open theme editor
2. Click on header section
3. Adjust:
   - Logo/image
   - Colors
   - Navigation items
   - Search/cart visibility

See [CUSTOMIZATION_GUIDE.md](docs/CUSTOMIZATION_GUIDE.md#customizing-header).

### Add New Translation

1. Update `locales/en.default.json` with new key
2. Add translation to all language files
3. Use in templates: `{{ 'key.path' | t }}`

See [I18N_GUIDE.md](docs/I18N_GUIDE.md#adding-a-new-language).

### Deploy Theme Updates

```bash
# With Shopify CLI
shopify theme push

# Or push to specific environment
shopify theme push --store my-store.myshopify.com

# Deploy live (requires confirmation)
shopify theme publish
```

### Local Development Setup

```bash
# Install dependencies
npm install

# Watch for CSS changes
npm run watch

# Build CSS
npm run build

# Start local server
shopify theme serve
```

## Shopify Resources

### Key APIs & Documentation
- [Shopify Theme Development](https://shopify.dev/themes)
- [Liquid Reference](https://shopify.dev/api/liquid)
- [Shopify CLI Docs](https://shopify.dev/themes/tools/cli)
- [Theme Architecture](https://shopify.dev/themes/architecture)

### External Libraries
- [Tailwind CSS](https://tailwindcss.com)
- [Swiper.js](https://swiperjs.com)
- [Flowbite](https://flowbite.com)

## Development Best Practices

### 1. Always Read Before Coding

- Check existing implementations
- Review similar patterns
- Understand the architecture

### 2. Follow Project Conventions

- Use existing component patterns
- Maintain consistent naming
- Follow folder structure
- Use established utilities

### 3. Test Across Devices

- Test on mobile (320px+)
- Test on tablet (768px+)
- Test on desktop (1024px+)
- Test in theme editor

### 4. Optimize Performance

- Compress images
- Minify CSS/JS
- Use lazy loading
- Preload critical assets
- Avoid render-blocking resources

### 5. Ensure Accessibility

- Include alt text on images
- Use semantic HTML
- Provide ARIA labels
- Test keyboard navigation
- Maintain color contrast

### 6. Translate All Text

- Add keys to translation files
- Use `{{ 'key' | t }}` filter
- Support all 30+ languages
- Test in different languages

### 7. Version Control

```bash
# Commit changes with descriptive messages
git add .
git commit -m "Add new feature description"

# Push to repository
git push origin feature-branch

# Create pull request for review
```

## Troubleshooting

### Issue: Changes not appearing in theme editor

**Solution:**
1. Save your changes
2. Refresh the theme editor (Cmd+R / Ctrl+R)
3. Hard refresh browser (Cmd+Shift+R / Ctrl+Shift+R)
4. Clear browser cache

### Issue: Styling not applying

**Solution:**
1. Check CSS file is being loaded (Network tab)
2. Verify CSS specificity (use section ID)
3. Check for conflicting styles
4. Use `!important` only as last resort

### Issue: JavaScript errors in console

**Solution:**
1. Check browser console for error messages
2. Verify syntax is correct
3. Check if libraries are loaded
4. Test in different browser

### Issue: Image not displaying

**Solution:**
1. Verify image URL is correct
2. Check image alt text is set
3. Verify width/height attributes
4. Check image file format supported

## Performance Tips

### Asset Optimization

```bash
# Minify CSS
npx csso assets/tailwind.css -o assets/tailwind.css

# Minify JavaScript
npx terser assets/global.js -o assets/global.js

# Compress images
# Use ImageOptim, TinyPNG, or similar tool
```

### Lazy Loading

```liquid
<img src="{{ image }}" loading="lazy" alt="Description">
```

### Defer Non-Critical Scripts

```liquid
<script defer src="{{ 'non-critical.js' | asset_url }}"></script>
```

### Preload Critical Assets

```liquid
<link rel="preload" href="{{ 'great-vibes.woff2' | asset_url }}" as="font" type="font/woff2" crossorigin>
```

## Support & Contribution

### Getting Help

- Check documentation first
- Review similar implementations
- Test in development environment
- Check browser console for errors

### Contributing

1. Fork the repository
2. Create a feature branch
3. Make changes following best practices
4. Test thoroughly
5. Submit pull request with description

## License

This theme is the intellectual property of Kiswa Noir. Use and modification is limited to authorized parties.

## Credits

**Built by**: Other Dev
**Website**: [otherdev.com](https://otherdev.com)
**Brand**: Kiswa Noir
**Contact**: WhatsApp +923111769243
**Social**: [Instagram @kiswanoir85](https://instagram.com/kiswanoir85)

---

## Table of Contents (Quick Links)

- [Architecture Guide](docs/ARCHITECTURE.md) - System design and structure
- [Sections Guide](docs/SECTIONS_GUIDE.md) - Building custom sections
- [Liquid Reference](docs/LIQUID_REFERENCE.md) - Template language guide
- [JavaScript Guide](docs/JAVASCRIPT_GUIDE.md) - JS patterns and Web Components
- [Styling Guide](docs/STYLING_GUIDE.md) - CSS and Tailwind patterns
- [i18n Guide](docs/I18N_GUIDE.md) - Translation and localization
- [Customization Guide](docs/CUSTOMIZATION_GUIDE.md) - Theme editor settings

---

**Last Updated**: January 2024
**Version**: 1.0.0
**Status**: Production Ready

