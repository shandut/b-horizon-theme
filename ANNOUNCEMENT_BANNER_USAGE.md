# Announcement Banner Section Usage Guide

## Overview
A customizable, collapsible announcement banner that can be placed at the top of your homepage or any page. Perfect for holiday announcements, delivery notices, sales, or important updates.

## Features
- ✅ **Dismissible**: Users can close the banner with an X button
- ✅ **Session persistence**: Once dismissed, stays hidden for the session
- ✅ **Auto-dismiss option**: Can automatically hide after a set time
- ✅ **Multiple icon options**: Warning, info, shipping, or gift icons
- ✅ **Fully customizable**: Colors, text, links, and spacing
- ✅ **Mobile responsive**: Optimized layout for all screen sizes
- ✅ **Accessible**: Proper ARIA labels and keyboard navigation
- ✅ **Smooth animations**: Slide down on load, slide up on dismiss

## How to Add to Your Homepage

### Option 1: Through Shopify Admin (Recommended)
1. Go to your Shopify Admin
2. Navigate to **Online Store > Themes**
3. Click **Customize** on your theme
4. On the homepage, click **Add section**
5. Look for **"Announcement Banner"** in the section list
6. Configure the settings as needed
7. Save your changes

### Option 2: Add to templates/index.json
Add this to your homepage template:

```json
{
  "sections": {
    "announcement_banner": {
      "type": "announcement-banner-collapsible",
      "settings": {
        "enable_banner": true,
        "title": "Christmas delivery",
        "message": "Order now to make sure your gifts arrive in time for Christmas!",
        "link_text": "Find out more",
        "link_url": "/pages/shipping-information",
        "show_icon": true,
        "icon_type": "warning",
        "dismissible": true,
        "background_color": "#FFF4E6",
        "text_color": "#333333",
        "link_color": "#0066CC",
        "icon_color": "#FF6B00"
      }
    }
  },
  "order": [
    "announcement_banner",
    // ... other sections
  ]
}
```

## Customization Options

### Basic Settings
- **Enable banner**: Toggle the banner on/off
- **Title**: Bold text before the message (optional)
- **Message**: Main announcement text
- **Link text**: Call-to-action link text
- **Link URL**: Where the link goes

### Icon Options
- **Warning triangle**: ⚠️ For urgent notices
- **Information circle**: ℹ️ For general info
- **Shipping truck**: 🚚 For delivery updates
- **Gift box**: 🎁 For holiday/gift messages

### Behavior Settings
- **Dismissible**: Allow users to close the banner
- **Auto-dismiss**: Automatically hide after X seconds
- **Auto-dismiss delay**: 5-30 seconds

### Styling
- **Background color**: Banner background
- **Text color**: Main text color
- **Link color**: CTA link color
- **Icon color**: Icon/warning symbol color
- **Padding**: Top and bottom spacing

## Example Configurations

### Holiday Shipping Notice (Like Bunnings)
```json
{
  "title": "Christmas delivery",
  "message": "Order now to make sure your gifts arrive in time for Christmas!",
  "link_text": "Find out more",
  "icon_type": "warning",
  "background_color": "#FFF4E6",
  "icon_color": "#FF6B00"
}
```

### Sale Announcement
```json
{
  "title": "SALE NOW ON",
  "message": "Up to 50% off selected items.",
  "link_text": "Shop sale",
  "icon_type": "gift",
  "background_color": "#FFE5E5",
  "icon_color": "#FF0000"
}
```

### Store Update
```json
{
  "title": "Store hours",
  "message": "We're open extended hours for the holidays.",
  "link_text": "View hours",
  "icon_type": "info",
  "background_color": "#E5F3FF",
  "icon_color": "#0066CC"
}
```

### Free Shipping
```json
{
  "title": "Free delivery",
  "message": "On all orders over $50.",
  "link_text": "Learn more",
  "icon_type": "shipping",
  "background_color": "#E5FFE5",
  "icon_color": "#00AA00"
}
```

## Technical Details

### Session Storage
The banner uses `sessionStorage` to remember when a user dismisses it. This means:
- Banner stays hidden for the current browser session
- Reappears when user opens a new browser window
- Perfect for temporary announcements

### Performance
- Lightweight: ~3KB total (HTML, CSS, JS)
- No external dependencies
- Vanilla JavaScript (no jQuery required)
- CSS animations for smooth transitions
- Lazy-loaded icons (inline SVG)

### Accessibility
- Proper ARIA labels on close button
- Keyboard navigable
- Screen reader friendly
- High contrast colors by default
- Respects user motion preferences

## Troubleshooting

### Banner not appearing?
1. Check that `enable_banner` is set to `true`
2. Clear browser session storage
3. Check browser console for errors

### Banner keeps reappearing?
- This is normal after closing the browser
- Use localStorage instead of sessionStorage for permanent dismissal

### Style conflicts?
- The section uses scoped CSS with unique class names
- Check for global CSS that might override colors

## Best Practices

1. **Keep messages short**: 1-2 lines maximum
2. **Use clear CTAs**: "Find out more", "Shop now", "Learn more"
3. **Choose appropriate colors**: Ensure good contrast (4.5:1 ratio)
4. **Don't overuse**: One announcement at a time
5. **Update regularly**: Keep content fresh and relevant
6. **Test on mobile**: Ensure message fits on small screens

## Support
This section follows Shopify theme best practices and is compatible with:
- All modern browsers
- Shopify Online Store 2.0
- Mobile devices
- Screen readers
- Keyboard navigation
