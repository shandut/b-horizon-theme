# Shopify Theme Development Learnings & Best Practices

## Table of Contents
1. [Common Theme Customization Patterns](#common-theme-customization-patterns)
2. [Technical Implementation Guidelines](#technical-implementation-guidelines)
3. [Shopify Liquid Gotchas & Solutions](#shopify-liquid-gotchas--solutions)
4. [Accessibility & Best Practices](#accessibility--best-practices)
5. [Git & Deployment Workflow](#git--deployment-workflow)
6. [Common User Requests & Solutions](#common-user-requests--solutions)
7. [Performance Optimization Tips](#performance-optimization-tips)
8. [Testing & Validation](#testing--validation)

---

## Common Theme Customization Patterns

### 1. Variant Picker Customizations

**Pattern:** Users often want to customize how product variants are displayed.

**Common Requests:**
- Change "Color" to "Colour" for AU/UK markets
- Display variant images as thumbnails
- Different styling for color vs size options
- Text case transformations (UPPERCASE, lowercase, Capitalize)

**Implementation Strategy:**
```liquid
{% liquid
  # Check option type and apply different styling
  assign option_name_lower = product_option.name | downcase
  assign is_color_option = false
  if option_name_lower == 'color' or option_name_lower == 'colour'
    assign is_color_option = true
    assign variant_style = 'color_thumbnails'
  endif
%}
```

**Key Learnings:**
- Always check option names case-insensitively
- Separate logic for different option types (color, size, etc.)
- Use conditional CSS classes for flexible styling
- Consider international spelling variations

### 2. Promotional Sections

**Pattern:** Hero banners with discount information are extremely common.

**Successful Implementation Example:**
```liquid
<section class="promo-banner">
  <div class="promo-content">
    <span class="subtitle">{{ section.settings.subtitle }}</span>
    <div class="discount">
      <span class="percentage">{{ section.settings.discount_percentage }}</span>
      <span class="text">{{ section.settings.discount_text }}</span>
    </div>
  </div>
</section>
```

**Schema Best Practices:**
- Always include customizable colors (background, text)
- Provide padding/margin controls
- Include optional background images
- Add link options for entire banner or specific CTAs
- Support both desktop and mobile layouts

### 3. Product Information Blocks

**Pattern:** Enhanced product information displays beyond basic Shopify defaults.

**Common Enhancements:**
- Multi-promo badges based on product tags
- Afterpay/payment installment calculators
- Enhanced product titles with vendor display
- Dynamic pricing displays

**Example Multi-Promo Badge Logic:**
```liquid
{% for i in (1..10) %}
  assign tag_key = 'product_tag_' | append: i
  assign promo_tag = block.settings[tag_key]
  
  if product.tags contains promo_tag
    # Display corresponding promo
  endif
{% endfor %}
```

---

## Technical Implementation Guidelines

### 1. Liquid Money Filters

**Critical Learning:** Shopify prices are stored in cents!

```liquid
# WRONG - divides cents by 4
assign installment_price = product.price | divided_by: 4

# CORRECT - converts to dollars first
assign installment_price = product.price | divided_by: 4
assign installment_price_formatted = installment_price | money
```

### 2. Dynamic Settings with Loops

**Pattern:** Creating multiple similar settings dynamically.

```liquid
{% schema %}
{
  "settings": [
    {% for i in (1..10) %}
      {
        "type": "text",
        "id": "item_{{ i }}",
        "label": "Item {{ i }}"
      }{% unless forloop.last %},{% endunless %}
    {% endfor %}
  ]
}
{% endschema %}
```

### 3. Image Dimensions & Responsive Images

**Key Specifications:**
```liquid
# For specific dimensions (e.g., 572x715)
{{ image | image_url: width: 572, height: 715 }}

# With responsive srcset
assign widths = '572, 744, 1144, 1430'
{{ image | image_url: width: 1430 | image_tag: widths: widths }}
```

### 4. Conditional Rendering Based on Settings

**Pattern:** Show/hide elements based on multiple conditions.

```liquid
{% if section.settings.show_element and product.available %}
  {% unless product.tags contains 'hide-element' %}
    # Render element
  {% endunless %}
{% endif %}
```

---

## Shopify Liquid Gotchas & Solutions

### 1. The Extra Brackets Issue

**Problem:** When building arrays or strings dynamically, extra brackets appear.

**Wrong Approach:**
```liquid
assign items = '' | split: ','
assign items = items | push: 'item1'
# Results in ["item1"] being displayed
```

**Correct Approach:**
```liquid
assign items_string = ''
if condition
  assign items_string = items_string | append: 'item1'
endif
assign items = items_string | split: '||'
```

### 2. Case-Sensitive String Comparisons

**Problem:** Liquid string comparisons are case-sensitive.

**Solution:** Always use `downcase` for comparisons:
```liquid
{% if menu_item.title | downcase == 'sale' %}
  # Apply special styling
{% endif %}
```

### 3. Schema Validation Issues

**Common Mistakes:**
- Trailing commas in JSON
- Missing required fields (type, id, label)
- Invalid setting types
- Incorrectly formatted visible_if conditions

**Correct Schema Example:**
```json
{
  "type": "text",
  "id": "setting_id",
  "label": "Setting Label",
  "default": "Default Value",
  "visible_if": { "other_setting": true }
}
```

---

## Accessibility & Best Practices

### 1. Modal/Dialog Implementation

**Always use native `<dialog>` element:**
```html
<dialog id="modal-{{ block.id }}" class="modal">
  <button onclick="this.closest('dialog').close()">Close</button>
  <!-- Content -->
</dialog>

<button onclick="document.getElementById('modal-{{ block.id }}').showModal()">
  Open Modal
</button>
```

### 2. Color Contrast Requirements

**Key Values to Remember:**
- Text: 4.5:1 minimum contrast ratio
- Large text: 3:1 minimum contrast ratio
- UI components: 3:1 minimum contrast ratio
- Use colors like #dc3545 (red) for sufficient contrast

### 3. ARIA Labels for Dynamic Content

```liquid
<button aria-label="{{ product.title | escape }} - Add to cart">
  Add to Cart
</button>
```

---

## Git & Deployment Workflow

### 1. Commit Message Standards

**Good Examples:**
```bash
git commit -m "Add Afterpay payment calculator to product page

- Calculate 4 installments based on product price
- Include modal for payment information
- Add customizable thresholds and styling
- Support dynamic variant price updates"
```

### 2. Handling Shopify CLI Sync Issues

**Common Issues & Solutions:**
1. **Non-fast-forward errors:** Always pull with rebase
   ```bash
   git pull --rebase && git push origin main
   ```

2. **File conflicts:** Shopify Theme Editor changes
   - Pull changes from Shopify first
   - Merge carefully, preserving both sets of changes
   - Push back to maintain sync

3. **Authentication issues:** Re-authenticate with Shopify CLI
   ```bash
   shopify theme dev --store=store-name.myshopify.com
   ```

---

## Common User Requests & Solutions

### 1. "Make the Sale text red in navigation"

**Solution Pattern:**
```liquid
{% assign is_sale_item = false %}
{% if link.title | downcase == 'sale' and section.settings.highlight_sale %}
  assign is_sale_item = true
{% endif %}

<a class="nav-link{% if is_sale_item %} nav-link--sale{% endif %}"
   {% if is_sale_item %}
     style="color: {{ section.settings.sale_color }};"
   {% endif %}>
  {{ link.title }}
</a>
```

### 2. "Add social media icons"

**Key Implementation Points:**
- Use SVG icons for scalability
- Provide individual URL inputs for each platform
- Include hover states
- Consider using Font Awesome or custom SVGs
- Always include aria-labels

### 3. "Show different content based on product tags"

**Flexible Pattern:**
```liquid
{% for tag in product.tags %}
  {% case tag %}
    {% when '2for40' %}
      <span class="badge">2 for $40</span>
    {% when '50off' %}
      <span class="badge">50% OFF</span>
  {% endcase %}
{% endfor %}
```

---

## Performance Optimization Tips

### 1. Image Loading Strategy

```liquid
# First image - eager load
loading: 'eager', fetchpriority: 'high'

# Other images - lazy load
loading: 'lazy', fetchpriority: 'auto'
```

### 2. Responsive Image Sizes

**Calculate appropriate sizes attribute:**
```liquid
# Mobile first approach
sizes="(min-width: 1200px) 572px, 
       (min-width: 750px) calc(50vw - 2rem), 
       100vw"
```

### 3. Minimize Liquid Processing

**Use capture and assign efficiently:**
```liquid
{% capture expensive_operation %}
  # Complex logic here
{% endcapture %}
{% assign result = expensive_operation | strip %}
```

---

## Testing & Validation

### 1. Essential Testing Checklist

- [ ] Test all variant combinations
- [ ] Verify mobile responsiveness
- [ ] Check accessibility (keyboard navigation, screen readers)
- [ ] Test with long product names/descriptions
- [ ] Verify all currency formats
- [ ] Test with products without images
- [ ] Check sold-out product behavior
- [ ] Verify translation keys work

### 2. Common Validation Errors

**Schema Validation:**
```bash
# Always validate JSON schema
shopify theme check
```

**Liquid Syntax:**
- Unclosed tags
- Missing `endfor`, `endif`, `endunless`
- Incorrect filter syntax
- Missing required parameters in snippets

### 3. Browser Testing Requirements

**Minimum browsers to test:**
- Chrome (latest)
- Safari (latest)
- Firefox (latest)
- Mobile Safari (iOS)
- Chrome Mobile (Android)

---

## Quick Reference: Useful Liquid Patterns

### String Manipulation
```liquid
# Remove whitespace
{{ variable | strip }}

# Replace text
{{ text | replace: 'old', 'new' }}

# Truncate with ellipsis
{{ text | truncate: 50 }}

# URL handle
{{ text | handle }}
```

### Array Operations
```liquid
# Create array from string
assign items = 'item1,item2,item3' | split: ','

# Add to array (creates new array)
assign items = items | push: 'item4'

# Check if contains
{% if items contains 'item2' %}

# Get first/last
{{ items | first }}
{{ items | last }}
```

### Money Formatting
```liquid
# Standard money format
{{ price | money }}

# Without currency symbol
{{ price | money_without_currency }}

# With currency code
{{ price | money_with_currency }}
```

---

## Final Tips for AI Agents and Developers

1. **Always read existing code first** - Understanding the current implementation prevents breaking changes

2. **Use semantic HTML** - Shopify themes should be accessible by default

3. **Respect theme architecture** - Follow existing patterns for consistency
   - Sections for page layout
   - Blocks for reusable components
   - Snippets for shared code
   - Assets for static files

4. **Test incrementally** - Make small changes and test frequently

5. **Document your changes** - Update comments and documentation as you go

6. **Consider international users** - Support multiple currencies, languages, and regional preferences

7. **Mobile-first development** - Most Shopify traffic is mobile

8. **Use Shopify's built-in features** - Don't reinvent what Shopify already provides

9. **Keep performance in mind** - Every millisecond counts for conversion

10. **Always provide fallbacks** - Handle edge cases gracefully

---

## Resources & References

- [Shopify Liquid Documentation](https://shopify.dev/docs/themes/liquid/reference)
- [Theme Architecture](https://shopify.dev/docs/themes/architecture)
- [Shopify CLI Commands](https://shopify.dev/docs/themes/tools/cli)
- [WCAG 2.2 Guidelines](https://www.w3.org/WAI/WCAG22/quickref/)
- [Shopify Theme Check](https://shopify.dev/docs/themes/tools/theme-check)

---

*Last Updated: November 2024*
*Based on real-world theme development for shoe-theme-horizonz project*


