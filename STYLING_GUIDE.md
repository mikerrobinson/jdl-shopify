# Styling Guide: Horizon CSS + Tailwind CSS

This theme uses a **dual system approach** combining Shopify's Horizon theme CSS with Tailwind CSS.

## Quick Decision Guide

**Use Horizon CSS classes when:**
- Working with existing components (buttons, cards, forms, navigation)
- Need complex interactions (carousels, galleries, drawers)
- Component already exists and works well

**Use Tailwind CSS when:**
- Building new custom sections from scratch
- Need quick layout utilities (flex, grid, spacing)
- Prototyping new designs
- Creating one-off custom styling

## System Architecture

### How Settings Flow to Styles

```
Shopify Admin UI
  ↓ (user changes colors, fonts, spacing)
Settings stored in settings_data.json
  ↓
Liquid snippets generate CSS variables
  - theme-styles-variables.liquid (typography, spacing, layout)
  - color-schemes.liquid (color schemes)
  ↓
CSS variables available to both systems
  ↓
┌─────────────────────┬─────────────────────┐
│   Horizon CSS       │   Tailwind CSS      │
│   (base.css)        │   (app.css)         │
└─────────────────────┴─────────────────────┘
```

**Key Principle**: Both systems read from the same CSS variables, so **all styles remain editable in Shopify admin**.

## Tailwind Configuration

### Theme-Aware Utilities

All Tailwind utilities prefixed with `theme-*` automatically use values from Shopify admin settings:

#### Colors
```html
<div class="bg-theme-bg text-theme-fg">
  <h1 class="text-theme-heading">Heading</h1>
  <button class="bg-theme-primary hover:bg-theme-primary-hover">Click</button>
</div>
```

**Available colors:**
- `theme-bg` - Background color
- `theme-fg` - Foreground/text color
- `theme-heading` - Heading color
- `theme-primary` - Primary brand color
- `theme-primary-hover` - Primary hover state
- `theme-border` - Border color
- `theme-shadow` - Shadow color

#### Typography
```html
<h1 class="font-theme-heading">Uses heading font from admin</h1>
<p class="font-theme-body">Uses body font from admin</p>
<span class="font-theme-accent">Uses accent font from admin</span>
```

**Available fonts:**
- `font-theme-body`
- `font-theme-subheading`
- `font-theme-heading`
- `font-theme-accent`

#### Spacing
```html
<div class="p-theme-md m-theme-lg">
  <div class="space-y-theme-sm">
    <!-- Uses spacing scale from admin -->
  </div>
</div>
```

**Available spacing:**
- `theme-3xs`, `theme-2xs`, `theme-xs` (extra small)
- `theme-sm`, `theme-md`, `theme-lg` (small to large)
- `theme-xl`, `theme-2xl`, `theme-3xl` (extra large)

### Using CSS Variables Directly

For any CSS variable not mapped to Tailwind, use arbitrary values:

```html
<!-- Use any CSS variable with arbitrary value syntax -->
<div class="text-[color:var(--color-primary)]">
  Custom color
</div>

<div class="p-[var(--padding-md)]">
  Custom padding
</div>

<div class="rounded-[var(--style-border-radius-buttons-primary)]">
  Custom radius
</div>
```

## Practical Examples

### Example 1: Custom Section with Tailwind Layout + Horizon Components

```liquid
<!-- /sections/custom-hero.liquid -->
<div class="color-{{ section.settings.color_scheme }}">
  <div class="flex flex-col md:flex-row items-center gap-theme-lg p-theme-xl">

    <!-- Left column: Use Tailwind for layout -->
    <div class="w-full md:w-1/2 space-y-theme-md">
      <h1 class="font-theme-heading text-4xl text-theme-heading">
        {{ section.settings.heading }}
      </h1>
      <p class="font-theme-body text-theme-fg">
        {{ section.settings.text }}
      </p>

      <!-- Use existing Horizon button component -->
      {% render 'button',
        style: 'primary',
        text: section.settings.button_text,
        url: section.settings.button_url
      %}
    </div>

    <!-- Right column: Use Tailwind for layout -->
    <div class="w-full md:w-1/2">
      <img src="{{ section.settings.image | img_url: '800x' }}"
           class="w-full h-auto rounded-lg">
    </div>

  </div>
</div>
```

### Example 2: Grid Layout with Horizon Cards

```liquid
<!-- Use Tailwind for grid, Horizon for card styling -->
<div class="grid grid-cols-1 md:grid-cols-3 gap-theme-md p-theme-lg">
  {% for product in collection.products %}
    {% render 'product-card', product: product %}
  {% endfor %}
</div>
```

### Example 3: Custom Component Using Both Systems

```liquid
<!-- Custom announcement bar -->
<div class="bg-theme-primary text-white">
  <div class="flex items-center justify-between max-w-7xl mx-auto px-theme-md py-theme-sm">

    <span class="font-theme-body text-sm">
      {{ settings.announcement_text }}
    </span>

    <!-- Use Horizon's existing button styling -->
    <button class="button button--secondary button--small">
      Shop Now
    </button>

  </div>
</div>
```

### Example 4: Responsive Spacing

```liquid
<!-- Mobile: small spacing, Desktop: large spacing -->
<section class="p-theme-sm md:p-theme-xl">
  <div class="space-y-theme-xs md:space-y-theme-lg">
    <h2>Content here</h2>
    <p>More content</p>
  </div>
</section>
```

## Common Horizon Components to Reuse

These components are already built and work great - use them instead of rebuilding:

### Buttons
```liquid
{% render 'button',
  style: 'primary',        # or 'secondary'
  text: 'Click me',
  url: '/products',
  size: 'large'            # optional
%}
```

### Product Cards
```liquid
{% render 'product-card',
  product: product,
  show_vendor: true,
  show_quick_add: true
%}
```

### Collection Cards
```liquid
{% render 'collection-card',
  collection: collection,
  columns: 3
%}
```

### Icon Components
```liquid
{% render 'icon', icon: 'cart', size: 'medium' %}
{% render 'icon', icon: 'search', size: 'large' %}
```

## Best Practices

### DO:
- Use Tailwind for layout (flex, grid, spacing, positioning)
- Use Horizon components for interactive elements
- Use `theme-*` utilities to respect admin settings
- Combine both systems in the same component

### DON'T:
- Don't rebuild existing Horizon components with Tailwind
- Don't use hard-coded colors (use `theme-*` colors instead)
- Don't use hard-coded spacing (use `theme-*` spacing instead)
- Don't migrate complex components (carousel, drawer, navigation)

## Testing Your Styles

### Verify Admin Settings Work

1. Make a change in Shopify admin (e.g., change primary color)
2. Both Horizon and Tailwind styles should update automatically
3. No code changes should be needed

### Test Color Schemes

```liquid
<!-- Your custom section should work with any color scheme -->
<div class="color-{{ section.settings.color_scheme }}">
  <div class="bg-theme-bg text-theme-fg p-theme-md">
    <!-- Content inherits colors from color scheme -->
  </div>
</div>
```

### Test Responsive Behavior

Use Tailwind's responsive prefixes with theme values:
```html
<div class="p-theme-sm md:p-theme-md lg:p-theme-lg">
  Spacing scales with viewport
</div>
```

## Available CSS Variables

If you need to access variables not mapped to Tailwind, here are the most useful ones:

### Colors
```css
--color-background-rgb
--color-foreground-rgb
--color-foreground-heading-rgb
--color-primary-rgb
--color-primary-hover-rgb
--color-primary-button-background-rgb
--color-secondary-button-background-rgb
--color-border-rgb
--color-shadow-rgb
```

### Typography
```css
--font-body--family
--font-heading--family
--font-h1--size
--font-h2--size
--font-paragraph--size
```

### Spacing
```css
--margin-{3xs,2xs,xs,sm,md,lg,xl,2xl,3xl,4xl,5xl,6xl}
--padding-{3xs,2xs,xs,sm,md,lg,xl,2xl,3xl}
--gap-{3xs,2xs,xs,sm,md,lg,xl,2xl,3xl}
```

### Borders & Radius
```css
--style-border-radius-buttons-primary
--style-border-radius-buttons-secondary
--style-border-radius-inputs
--style-border-radius-cards
--style-border-radius-pill
```

### Animation
```css
--animation-speed-fast
--animation-speed-normal
--animation-speed-medium
--animation-speed-slow
```

## Getting Help

- For Horizon components: Check existing snippets in `/snippets/`
- For Tailwind utilities: See `tailwind.config.js` for configured values
- For CSS variables: See `snippets/theme-styles-variables.liquid` and `snippets/color-schemes.liquid`

## Summary

This dual system gives you the best of both worlds:
- **Horizon CSS**: Proven, complex components that work out of the box
- **Tailwind CSS**: Rapid development for layouts and custom sections
- **CSS Variables**: Both systems stay editable in Shopify admin

When in doubt, use what already exists. Build new things with Tailwind.
