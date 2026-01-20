# JDL Shopify Theme

A custom Shopify theme for [jdlindustries.com](https://jdlindustries.com) based on Shopify's Horizon theme, enhanced with Tailwind CSS for rapid UI development and customization.

## About This Theme

This theme builds upon Shopify's Horizon theme foundation, which provides a clean, modern design with excellent performance characteristics. The addition of Tailwind CSS enables rapid prototyping and custom styling without the need to write custom CSS from scratch.

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

## Using Tailwind CSS

Add Tailwind utility classes directly to your Liquid templates:

```liquid
<div class="container mx-auto px-4">
  <h1 class="text-3xl font-bold text-gray-900">Hello World</h1>
  <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
    Click Me
  </button>
</div>
```

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
