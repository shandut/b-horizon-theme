# Bunnings-Style Header Section Usage Guide

## Overview
A custom Shopify header section that matches the Bunnings Warehouse design with a green background, prominent search bar, and clean navigation elements.

## Features
- ✅ **Bunnings green color scheme** (#0d5257 background)
- ✅ **Left-aligned logo** with customizable size
- ✅ **Center search bar** with predictive search functionality
- ✅ **Right-aligned actions** (Sign In, Lists, Cart)
- ✅ **Cart count badge** with dynamic updates
- ✅ **Predictive search** with product and collection results
- ✅ **Popular searches** (optional)
- ✅ **Mobile responsive** design
- ✅ **Accessible** with proper ARIA labels

## How to Add to Your Store

### Option 1: Through Shopify Admin (Recommended)
1. Go to your Shopify Admin
2. Navigate to **Online Store > Themes**
3. Click **Customize** on your theme
4. In the header area, click **Add section**
5. Look for **"Bunnings Header"** in the section list
6. Configure the settings as needed
7. Save your changes

### Option 2: Replace Default Header
1. In the theme customizer, remove or hide the default header section
2. Add the Bunnings Header section in its place
3. Configure all settings to match your brand

## Configuration Options

### Logo Settings
- **Logo image**: Upload your store logo (SVG recommended)
- **Logo width**: 50-250px (default: 150px)
- **Logo height**: 20-100px (default: 60px)

### Search Settings
- **Search placeholder**: Custom placeholder text
- **Show popular searches**: Toggle popular search links
- **Popular searches**: Up to 5 popular search terms

### Action Links
- **Show Lists**: Toggle the Lists link
- **Lists text**: Customize the Lists label
- **Lists URL**: Link to your lists/wishlist page

### Color Customization
- **Background color**: Header background (default: #0d5257)
- **Text color**: Text and icon color (default: white)
- **Hover color**: Hover state background
- **Search background**: Search bar background (default: white)
- **Search text color**: Search input text color
- **Search border color**: Search bar border

### Layout
- **Header height**: 60-120px (default: 80px)

## Default Bunnings Configuration

```json
{
  "background_color": "#0d5257",
  "text_color": "#ffffff",
  "hover_color": "rgba(255, 255, 255, 0.1)",
  "search_background": "#ffffff",
  "search_text_color": "#333333",
  "search_placeholder": "What can we help you find today?",
  "show_lists": true,
  "lists_text": "Lists",
  "header_height": 80
}
```

## Search Functionality

### Predictive Search Features
- **Product results**: Shows products with images and prices
- **Collection results**: Shows matching collections
- **Debounced input**: 300ms delay for performance
- **Minimum 2 characters**: Prevents unnecessary API calls
- **Escape key**: Closes search results
- **Click outside**: Closes search results

### Search API Integration
The header uses Shopify's native search API:
- Endpoint: `/search/suggest.json`
- Resources: Products, Collections, Pages
- Limit: 5 results per type

## Mobile Behavior

On screens smaller than 750px:
- Logo width reduces automatically
- Search bar can be hidden (implement toggle if needed)
- Action text labels are hidden (icons only)
- Cart count badge repositions

## Cart Integration

### Dynamic Cart Updates
- Cart count updates without page refresh
- Listen for `cart:updated` events
- AJAX cart compatibility
- Proper ARIA labels for accessibility

### Cart Count Badge
- Shows when items > 0
- Orange background (#ff6b00)
- Positioned absolutely over cart icon
- Updates dynamically

## Accessibility Features

- **Semantic HTML**: Proper header structure
- **ARIA labels**: All interactive elements labeled
- **Keyboard navigation**: Full keyboard support
- **Screen reader friendly**: Proper announcements
- **Focus indicators**: Clear visual focus states
- **Color contrast**: Meets WCAG 2.2 standards

## Customization Examples

### Hardware Store Theme
```liquid
{
  "background_color": "#2c5530",
  "search_placeholder": "Search for tools, materials, and more...",
  "show_popular_searches": true,
  "popular_search_1": "power tools",
  "popular_search_2": "paint",
  "popular_search_3": "timber",
  "popular_search_4": "garden",
  "popular_search_5": "lighting"
}
```

### Home & Garden Theme
```liquid
{
  "background_color": "#1a4d2e",
  "search_placeholder": "Find everything for your home & garden",
  "lists_text": "Projects",
  "lists_url": "/pages/diy-projects"
}
```

### Trade Supplies Theme
```liquid
{
  "background_color": "#003d4d",
  "search_placeholder": "Search trade supplies and equipment",
  "show_lists": true,
  "lists_text": "Quote List"
}
```

## Advanced Customization

### Adding Mobile Menu Toggle
```javascript
// Add to the script section
function toggleMobileSearch() {
  const searchSection = document.querySelector('.bunnings-header__search');
  searchSection.classList.toggle('mobile-visible');
}
```

### Custom Popular Searches Style
```css
.bunnings-search__popular {
  background: rgba(255, 255, 255, 0.1);
  padding: 0.5rem 1rem;
  border-radius: 20px;
}
```

### Enhanced Cart Drawer Integration
```javascript
// Integrate with cart drawer instead of cart page
document.querySelector('.bunnings-header__cart-link').addEventListener('click', function(e) {
  e.preventDefault();
  // Open cart drawer
  document.querySelector('#cart-drawer').showModal();
});
```

## Troubleshooting

### Search not working?
1. Check that Shopify search is enabled in your store
2. Verify the search API endpoint is accessible
3. Check browser console for JavaScript errors

### Logo not displaying?
1. Ensure image is uploaded to Shopify Files
2. Check image format (PNG, JPG, SVG supported)
3. Verify logo width/height settings

### Cart count not updating?
1. Ensure cart.js API is accessible
2. Check for JavaScript errors
3. Verify `cart:updated` events are firing

## Performance Tips

1. **Use SVG logos** for better scaling
2. **Optimize images** before uploading
3. **Minimize custom CSS** additions
4. **Use native Shopify APIs** for search
5. **Implement lazy loading** for search results

## Browser Support

- Chrome (latest)
- Safari (latest)
- Firefox (latest)
- Edge (latest)
- Mobile Safari (iOS 12+)
- Chrome Mobile (Android)

## Best Practices

1. **Keep search prominent** - It's a key navigation tool
2. **Maintain contrast** - Ensure text is readable
3. **Test on mobile** - Most traffic is mobile
4. **Use clear CTAs** - Make actions obvious
5. **Optimize for speed** - Header loads on every page
6. **Follow accessibility** - Support all users

## Support

This section follows Shopify theme best practices and is compatible with:
- Shopify Online Store 2.0
- All modern browsers
- Screen readers
- Keyboard navigation
- Touch devices

---

*Based on Bunnings Warehouse design patterns for Shopify themes*
