# JavaScript Architecture Guide for Kiswa Noir

This guide covers the JavaScript patterns and Web Components used in Kiswa Noir theme.

## Architecture Overview

Kiswa Noir uses:

- **Vanilla JavaScript (ES6+)** - No jQuery or frameworks
- **Web Components** - Custom HTML elements for interactive features
- **Modular Pattern** - Functions organized by feature
- **Minimal Dependencies** - Swiper.js for carousels, Flowbite for UI

## File Structure

```
assets/
├── global.js                  # Core utilities & initialization (38KB)
├── cart.js                    # Cart Web Components (4.7KB)
├── cart-notification.js       # Toast notifications
├── theme-editor.js            # Shopify theme editor integration
└── swiper.min.js             # Carousel library
```

## Global.js Core Features

The main global.js file contains:

### 1. Initialization

```javascript
document.addEventListener('DOMContentLoaded', function() {
  // Initialize all components
  initCart();
  initMenu();
  initModals();
  setupFocusTraps();
});
```

### 2. Utilities

Common utility functions:

```javascript
// Get elements safely
const getElement = (selector) => document.querySelector(selector);
const getElements = (selector) => document.querySelectorAll(selector);

// Event delegation
const delegate = (parent, event, selector, callback) => {
  parent.addEventListener(event, (e) => {
    if (e.target.matches(selector)) {
      callback.call(e.target, e);
    }
  });
};

// Classes
const addClass = (el, className) => el.classList.add(className);
const removeClass = (el, className) => el.classList.remove(className);
const toggleClass = (el, className) => el.classList.toggle(className);
const hasClass = (el, className) => el.classList.contains(className);
```

### 3. Cart Management

```javascript
class Cart {
  async addItem(variantId, quantity = 1) {
    const response = await fetch('/cart/add.js', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        items: [{ id: variantId, quantity }]
      })
    });

    const cart = await response.json();
    this.updateUI(cart);
    this.showNotification(`Added to cart`);
    return cart;
  }

  async updateQuantity(lineIndex, quantity) {
    const response = await fetch('/cart/change.js', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        line: lineIndex,
        quantity
      })
    });

    const cart = await response.json();
    this.updateUI(cart);
    return cart;
  }

  async removeItem(lineIndex) {
    return this.updateQuantity(lineIndex, 0);
  }

  updateUI(cart) {
    // Update cart count
    const cartCount = document.querySelector('[data-cart-count]');
    if (cartCount) {
      cartCount.textContent = cart.item_count;
    }

    // Update cart total
    const cartTotal = document.querySelector('[data-cart-total]');
    if (cartTotal) {
      cartTotal.textContent = this.formatMoney(cart.total_price);
    }

    // Update cart items
    const cartItems = document.querySelector('cart-items');
    if (cartItems) {
      cartItems.update(cart);
    }
  }

  formatMoney(cents) {
    return '$' + (cents / 100).toFixed(2);
  }

  showNotification(message) {
    const notification = document.createElement('cart-notification');
    notification.setAttribute('message', message);
    document.body.appendChild(notification);
  }
}

const cart = new Cart();
```

### 4. Focus Trap

Trap focus within modals:

```javascript
class FocusTrap {
  constructor(element) {
    this.element = element;
    this.focusableElements = this.element.querySelectorAll(
      'a, button, input, select, textarea, [tabindex]'
    );
    this.firstElement = this.focusableElements[0];
    this.lastElement = this.focusableElements[
      this.focusableElements.length - 1
    ];

    this.handleKeyDown = this.handleKeyDown.bind(this);
  }

  activate() {
    this.element.addEventListener('keydown', this.handleKeyDown);
    this.firstElement.focus();
  }

  deactivate() {
    this.element.removeEventListener('keydown', this.handleKeyDown);
  }

  handleKeyDown(e) {
    if (e.key !== 'Tab') return;

    if (e.shiftKey) {
      if (document.activeElement === this.firstElement) {
        this.lastElement.focus();
        e.preventDefault();
      }
    } else {
      if (document.activeElement === this.lastElement) {
        this.firstElement.focus();
        e.preventDefault();
      }
    }
  }
}
```

## Web Components

Custom HTML elements for cart interaction:

### 1. CartItems Component

Renders and manages cart items:

```javascript
class CartItems extends HTMLElement {
  connectedCallback() {
    this.addEventListener('change', this.onChange.bind(this));
    this.addEventListener('click', this.onClick.bind(this));
  }

  onChange(e) {
    if (e.target.matches('[data-quantity]')) {
      const lineIndex = parseInt(e.target.dataset.lineIndex);
      const quantity = parseInt(e.target.value);
      cart.updateQuantity(lineIndex, quantity);
    }
  }

  onClick(e) {
    if (e.target.matches('[data-remove]')) {
      const lineIndex = parseInt(e.target.dataset.lineIndex);
      cart.removeItem(lineIndex);
    }
  }

  update(cartData) {
    this.innerHTML = this.renderItems(cartData.items);
  }

  renderItems(items) {
    return items
      .map(
        (item, index) => `
      <div class="cart-item">
        <img src="${item.image.src}" alt="${item.title}">
        <div>
          <h4>${item.title}</h4>
          <p>$${(item.price / 100).toFixed(2)}</p>
          <input
            type="number"
            value="${item.quantity}"
            min="1"
            data-quantity
            data-line-index="${index}"
          >
          <button data-remove data-line-index="${index}">Remove</button>
        </div>
      </div>
    `
      )
      .join('');
  }
}

customElements.define('cart-items', CartItems);
```

Usage in HTML:

```html
<cart-items data-cart-items></cart-items>
```

### 2. CartNotification Component

Toast notifications:

```javascript
class CartNotification extends HTMLElement {
  connectedCallback() {
    this.render();
    this.show();
  }

  render() {
    const message = this.getAttribute('message') || 'Added to cart';
    this.innerHTML = `
      <div class="notification">
        <p>${message}</p>
        <button aria-label="Close">×</button>
      </div>
    `;

    this.querySelector('button').addEventListener('click', () => {
      this.remove();
    });
  }

  show() {
    this.classList.add('show');
    setTimeout(() => this.remove(), 3000);
  }
}

customElements.define('cart-notification', CartNotification);
```

### 3. CartRemoveButton Component

Handles individual item removal:

```javascript
class CartRemoveButton extends HTMLElement {
  connectedCallback() {
    this.addEventListener('click', this.handleClick.bind(this));
  }

  handleClick(e) {
    e.preventDefault();
    const lineIndex = parseInt(this.dataset.lineIndex);
    cart.removeItem(lineIndex);
  }
}

customElements.define('cart-remove-button', CartRemoveButton);
```

## Menu/Navigation

Toggle menu visibility:

```javascript
class MenuToggle {
  constructor() {
    this.button = document.querySelector('[data-menu-toggle]');
    this.menu = document.querySelector('[data-menu]');

    if (this.button) {
      this.button.addEventListener('click', () => this.toggle());
      document.addEventListener('click', (e) => this.handleClickOutside(e));
    }
  }

  toggle() {
    this.menu.classList.toggle('hidden');
    this.button.setAttribute('aria-expanded',
      this.menu.classList.contains('hidden') ? 'false' : 'true'
    );
  }

  handleClickOutside(e) {
    if (!this.menu.contains(e.target) && !this.button.contains(e.target)) {
      this.menu.classList.add('hidden');
    }
  }
}

new MenuToggle();
```

## Modal Handling

```javascript
class Modal {
  constructor(selector) {
    this.modal = document.querySelector(selector);
    this.openButtons = document.querySelectorAll(`[data-modal="${selector}"]`);
    this.closeButtons = this.modal.querySelectorAll('[data-modal-close]');

    this.openButtons.forEach(btn => {
      btn.addEventListener('click', () => this.open());
    });

    this.closeButtons.forEach(btn => {
      btn.addEventListener('click', () => this.close());
    });

    document.addEventListener('keydown', (e) => {
      if (e.key === 'Escape') this.close();
    });
  }

  open() {
    this.modal.classList.remove('hidden');
    document.body.style.overflow = 'hidden';
  }

  close() {
    this.modal.classList.add('hidden');
    document.body.style.overflow = 'auto';
  }
}

new Modal('[data-modal-id="newsletter"]');
```

## Event Delegation

Handle events efficiently:

```javascript
// Delegate click events to parent
document.addEventListener('click', (e) => {
  if (e.target.matches('[data-product-add]')) {
    const variantId = e.target.dataset.variantId;
    cart.addItem(variantId);
  }

  if (e.target.matches('[data-size-select]')) {
    const size = e.target.value;
    console.log('Size selected:', size);
  }
});

// Delegate change events
document.addEventListener('change', (e) => {
  if (e.target.matches('[data-quantity]')) {
    console.log('Quantity changed to:', e.target.value);
  }
});
```

## API Interaction Patterns

### Fetch with Error Handling

```javascript
async function fetchCart() {
  try {
    const response = await fetch('/cart.json');
    if (!response.ok) throw new Error('Network error');
    return await response.json();
  } catch (error) {
    console.error('Failed to fetch cart:', error);
    return null;
  }
}

// Usage
const cartData = await fetchCart();
if (cartData) {
  console.log(cartData.items);
}
```

### Cart API Endpoints

```javascript
// Get cart
GET /cart.json

// Add to cart
POST /cart/add.js
Body: { items: [{ id: 12345, quantity: 1 }] }

// Update cart
POST /cart/change.js
Body: { line: 1, quantity: 2 }

// Remove from cart
POST /cart/change.js
Body: { line: 1, quantity: 0 }

// Get checkout URL
{{ cart.checkout_url }}
```

## Theme Editor Integration

Handle theme editor changes:

```javascript
if (window.Shopify && window.Shopify.designMode) {
  // We're in theme editor
  document.addEventListener('shopify:section:load', (e) => {
    // Section was added
    initSection(e.detail.sectionId);
  });

  document.addEventListener('shopify:section:unload', (e) => {
    // Section was removed
    cleanupSection(e.detail.sectionId);
  });

  document.addEventListener('shopify:section:select', (e) => {
    // Section was selected
    selectSection(e.detail.sectionId);
  });
}
```

## Performance Patterns

### Debouncing

```javascript
function debounce(func, delay) {
  let timeoutId;
  return function(...args) {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => func(...args), delay);
  };
}

// Usage
const handleResize = debounce(() => {
  console.log('Window resized');
}, 300);

window.addEventListener('resize', handleResize);
```

### Throttling

```javascript
function throttle(func, limit) {
  let inThrottle;
  return function(...args) {
    if (!inThrottle) {
      func(...args);
      inThrottle = true;
      setTimeout(() => (inThrottle = false), limit);
    }
  };
}

// Usage
const handleScroll = throttle(() => {
  console.log('Scrolled');
}, 1000);

window.addEventListener('scroll', handleScroll);
```

### Lazy Loading

```javascript
// Use Intersection Observer for lazy loading
const images = document.querySelectorAll('[data-lazy]');

const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const img = entry.target;
      img.src = img.dataset.src;
      img.classList.remove('lazy');
      observer.unobserve(img);
    }
  });
});

images.forEach(img => observer.observe(img));
```

## Accessibility Patterns

### Keyboard Navigation

```javascript
// Enable arrow key navigation
document.addEventListener('keydown', (e) => {
  if (e.key === 'ArrowLeft') {
    // Previous item
  } else if (e.key === 'ArrowRight') {
    // Next item
  } else if (e.key === 'Enter') {
    // Select item
  }
});
```

### ARIA Attributes

```javascript
// Update ARIA states
const button = document.querySelector('[data-menu-toggle]');
button.setAttribute('aria-expanded', 'false');
button.setAttribute('aria-label', 'Toggle navigation menu');

// Announce changes to screen readers
const announcement = document.createElement('div');
announcement.setAttribute('role', 'status');
announcement.setAttribute('aria-live', 'polite');
announcement.textContent = 'Item added to cart';
document.body.appendChild(announcement);
```

## Common Patterns

### Conditional Script Loading

```javascript
// Only load if needed
if (document.querySelector('[data-carousel]')) {
  loadScript('/assets/swiper.min.js');
}

function loadScript(src) {
  const script = document.createElement('script');
  script.src = src;
  script.async = true;
  document.head.appendChild(script);
}
```

### Store Component State

```javascript
class ComponentState {
  constructor() {
    this.state = JSON.parse(localStorage.getItem('appState')) || {};
  }

  setState(key, value) {
    this.state[key] = value;
    localStorage.setItem('appState', JSON.stringify(this.state));
  }

  getState(key) {
    return this.state[key];
  }
}

const state = new ComponentState();
state.setState('theme', 'dark');
console.log(state.getState('theme')); // 'dark'
```

## Best Practices

### 1. Use Semantic Selectors

```javascript
// Good
const addButton = document.querySelector('[data-add-to-cart]');

// Avoid
const btn = document.querySelector('.btn-primary');
```

### 2. Event Delegation

```javascript
// Good - Single listener
document.addEventListener('click', (e) => {
  if (e.target.matches('[data-delete]')) {
    handleDelete(e.target);
  }
});

// Avoid - Multiple listeners
document.querySelectorAll('[data-delete]').forEach(btn => {
  btn.addEventListener('click', handleDelete);
});
```

### 3. Error Handling

```javascript
// Good
try {
  const response = await fetch('/api/data');
  const data = await response.json();
} catch (error) {
  console.error('Error:', error);
  showUserMessage('Something went wrong');
}

// Avoid
const response = await fetch('/api/data');
const data = await response.json();
```

### 4. Async/Await Over Callbacks

```javascript
// Good
async function loadData() {
  const response = await fetch('/api/data');
  const data = await response.json();
  return data;
}

// Avoid
function loadData(callback) {
  fetch('/api/data')
    .then(r => r.json())
    .then(callback);
}
```

### 5. Clean Up Event Listeners

```javascript
// Good
class Component {
  constructor() {
    this.handleClick = this.handleClick.bind(this);
    this.element.addEventListener('click', this.handleClick);
  }

  destroy() {
    this.element.removeEventListener('click', this.handleClick);
  }
}

// Avoid - Memory leak
element.addEventListener('click', () => {
  // Anonymous function - can't remove
});
```

## Debugging

### Console Logging

```javascript
// Development logging
if (!window.PRODUCTION) {
  console.log('Component initialized');
}

// Better: Use a logger
const logger = {
  log: (...args) => {
    if (!window.PRODUCTION) console.log(...args);
  }
};
```

### Performance Monitoring

```javascript
// Measure performance
const start = performance.now();
doSomething();
const end = performance.now();
console.log(`Execution took ${end - start}ms`);
```

## Resources

- [MDN Web Docs - JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
- [Web Components Guide](https://developer.mozilla.org/en-US/docs/Web/Web_Components)
- [Shopify Theme Scripting](https://shopify.dev/themes/architecture/assets/script-tags)
- [Cart API Docs](https://shopify.dev/api/ajax/reference/cart)

