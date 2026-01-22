# JDL Shopify Theme

A custom Shopify theme for [jdlindustries.com](https://jdlindustries.com) based on Shopify's Horizon theme, enhanced with Tailwind CSS for rapid UI development and customization.

## About This Theme

This theme builds upon Shopify's Horizon theme foundation, which provides a clean, modern design with excellent performance characteristics. We've added Tailwind CSS to enable rapid prototyping and custom styling for new sections and layouts.

### Dual System Approach

This theme uses **both Horizon CSS and Tailwind CSS** together:

- **Horizon CSS** (`assets/base.css`) - Use for existing components like buttons, cards, forms, navigation, carousels, and galleries
- **Tailwind CSS** (`assets/app.css`) - Use for custom layouts, spacing utilities, and new sections you build from scratch

**Key Feature**: Both systems respect Shopify admin theme settings - colors, fonts, and spacing defined in the admin automatically work with both Horizon classes and Tailwind utilities.

📖 **See [STYLING_GUIDE.md](STYLING_GUIDE.md) for complete documentation on when to use each system.**

## Development Setup

### Prerequisites
- Node.js (v14 or higher)
- Shopify CLI
- npm or yarn

### Installation

1. Install dependencies:
```bash
npm install
```

2. Build Tailwind CSS:
```bash
npm run build:css
```

## Development Workflow

### Working with Shopify CLI

To develop locally with Shopify CLI and Tailwind CSS, you'll need to run two processes:

1. **Start Tailwind CSS watch mode** (in one terminal):
```bash
npm run dev
```
This watches your Liquid files and rebuilds `assets/app.css` whenever you use Tailwind classes.

2. **Start Shopify CLI** (in another terminal):
```bash
shopify theme dev
```
This starts the local development server and syncs your theme with Shopify.

### Build Commands

- `npm run build:css` - Build Tailwind CSS for production (minified)
- `npm run watch:css` - Watch for changes and rebuild automatically
- `npm run dev` - Alias for watch:css

## Using the Dual System

### Quick Examples

**Use Tailwind for layout, Horizon for components:**

```liquid
<!-- Custom section with Tailwind layout -->
<div class="flex flex-col md:flex-row gap-theme-lg p-theme-xl">
  <div class="w-full md:w-1/2">
    <h2 class="font-theme-heading text-theme-heading">{{ section.settings.title }}</h2>
    <p class="font-theme-body text-theme-fg mt-theme-sm">{{ section.settings.text }}</p>

    <!-- Use existing Horizon button component -->
    {% render 'button', style: 'primary', text: 'Shop Now' %}
  </div>
  <div class="w-full md:w-1/2">
    <img src="{{ section.settings.image | img_url: '800x' }}" class="w-full h-auto rounded-lg">
  </div>
</div>
```

**Theme-aware utilities automatically use admin settings:**

```liquid
<div class="bg-theme-bg text-theme-fg p-theme-md border border-theme-border">
  <h3 class="font-theme-heading text-theme-heading">Styled with admin colors</h3>
  <button class="bg-theme-primary hover:bg-theme-primary-hover text-white px-6 py-3 rounded">
    Primary Color Button
  </button>
</div>
```

### Available Theme Utilities

- **Colors**: `bg-theme-bg`, `text-theme-fg`, `text-theme-heading`, `bg-theme-primary`, `border-theme-border`
- **Typography**: `font-theme-body`, `font-theme-heading`, `font-theme-subheading`, `font-theme-accent`
- **Spacing**: `p-theme-sm`, `m-theme-md`, `gap-theme-lg` (3xs, 2xs, xs, sm, md, lg, xl, 2xl, 3xl)

The build process automatically scans your templates and only includes the CSS classes you actually use, keeping the file size small.

### Template Scanning

The Tailwind config is set up to scan:
- `layout/**/*.liquid`
- `sections/**/*.liquid`
- `snippets/**/*.liquid`
- `templates/**/*.liquid`
- `templates/**/*.json`
- `blocks/**/*.liquid`

## File Structure

- `src/styles.css` - Source CSS file with Tailwind directives
- `assets/app.css` - Built CSS file (generated, committed to repo)
- `tailwind.config.js` - Tailwind configuration
- `.shopifyignore` - Files to exclude from Shopify uploads

## Deployment

Before deploying, make sure to build the CSS:

```bash
npm run build:css
shopify theme push
```

The built `assets/app.css` file is committed to the repository and will be deployed with your theme.
