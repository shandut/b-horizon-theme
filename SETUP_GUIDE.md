# Shopify Horizon Theme Development Setup Guide

## ✅ Installation Complete!

The Shopify Horizon theme has been successfully cloned to your workspace. This is Shopify's flagship theme that showcases the latest Liquid Storefronts features.

## 📁 Theme Structure

Your theme contains the following directories:

- **`assets/`** - JavaScript, CSS files, and SVG icons
- **`blocks/`** - Reusable theme blocks (87 Liquid files)
- **`config/`** - Theme settings and configuration
- **`layout/`** - Main layout templates
- **`locales/`** - Internationalization files (50+ languages)
- **`sections/`** - Theme sections for the customizer
- **`snippets/`** - Reusable Liquid code snippets (111 files)
- **`templates/`** - Page templates

## 🚀 Next Steps

### 1. Connect to a Shopify Store

To start developing, you need to connect this theme to a Shopify development store:

```bash
# Login to your Shopify Partner account
shopify auth login

# List your stores
shopify store list

# Connect to a specific store
shopify theme dev --store your-store.myshopify.com
```

### 2. Start Development Server

Once connected to a store, start the development server:

```bash
# Start the development server
shopify theme dev

# Or with specific options
shopify theme dev --store your-store.myshopify.com --live-reload hot-reload
```

This will:
- Start a local development server (usually on http://127.0.0.1:9292)
- Enable hot reload for instant updates
- Sync your local changes with the development theme

### 3. Push Theme to Store

To create a new theme on your store:

```bash
# Push as a new unpublished theme
shopify theme push --unpublished

# Or push to a specific theme
shopify theme push --theme [theme-id]
```

### 4. Pull Latest Store Data

To sync with an existing theme:

```bash
# Pull from the live theme
shopify theme pull

# Pull from a specific theme
shopify theme pull --theme [theme-id]
```

## 🛠️ Development Tools

### Theme Check (Already Installed)

Validate your theme code:

```bash
# Run theme check
shopify theme check

# Auto-fix issues where possible
shopify theme check --auto-correct
```

### Useful Commands

```bash
# List themes on your store
shopify theme list

# Open theme in Shopify Admin
shopify theme open

# Open theme editor
shopify theme open --editor

# Package theme as a zip
shopify theme package
```

## ⚠️ Important Notes

1. **Not for Theme Store**: Horizon-based themes cannot be submitted to the Shopify Theme Store. Use the [Skeleton Theme](https://github.com/Shopify/skeleton-theme) for Theme Store submissions.

2. **Stay Updated**: To pull latest Horizon updates:
   ```bash
   git remote add upstream https://github.com/Shopify/horizon.git
   git fetch upstream
   git pull upstream main
   ```

3. **Development Store Required**: You need a Shopify development store to test the theme. Create one free at [partners.shopify.com](https://partners.shopify.com).

## 📚 Resources

- [Shopify Theme Documentation](https://shopify.dev/docs/themes)
- [Liquid Reference](https://shopify.dev/docs/api/liquid)
- [Theme Architecture](https://shopify.dev/docs/themes/architecture)
- [Shopify CLI Documentation](https://shopify.dev/docs/themes/tools/cli)
- [Theme Check Documentation](https://shopify.dev/docs/themes/tools/theme-check)

## 🎨 Theme Philosophy

Horizon follows these principles:
- **Web-native**: Leverages modern web standards
- **Lean & Fast**: Performance-first approach
- **Server-rendered**: HTML rendered by Shopify using Liquid
- **Progressive Enhancement**: Works across all browsers

## 🔧 Customization Tips

1. **Theme Settings**: Edit `config/settings_schema.json` to add custom theme settings
2. **Sections**: Create new sections in the `sections/` directory
3. **Styles**: Main CSS is in `assets/base.css`
4. **JavaScript**: Component-based JS files in `assets/`
5. **Translations**: Add/edit translations in `locales/`

---

Happy developing! 🚀 Your Horizon theme is ready for customization.
