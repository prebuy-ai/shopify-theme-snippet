# Prebuy Integration - Quick Reference Card

## 🚀 Installation (5 minutes)

1. **Upload snippet**: Copy `prebuy-selling-plan.liquid` to `snippets/` folder
2. **Add to product form**: Place after variant ID input
   ```liquid
   {% render 'prebuy-selling-plan', product: product %}
   ```
3. **Optional: Enable preorder button text**:
   ```liquid
   {% render 'prebuy-selling-plan', product: product, show_button_text: true %}
   ```
4. **Test**: Check browser console for 🌍 debug messages

## 📝 Common Integration Points

### Buy Buttons Snippet
```liquid
<!-- In snippets/buy-buttons.liquid -->
<input type="hidden" name="id" value="{{ product.selected_or_first_available_variant.id }}">
{% render 'prebuy-selling-plan', product: product %}
```

### Product Template
```liquid
<!-- In sections/main-product.liquid -->
<form action="{{ routes.cart_add_url }}" method="post">
  <input type="hidden" name="id" value="{{ variant.id }}">
  {% render 'prebuy-selling-plan', product: product %}
  <button type="submit">Add to Cart</button>
</form>
```

### Product Page Text
```liquid
<!-- After buy button -->
{% render 'prebuy-selling-plan', product: product, show_text: true %}
```

### Preorder Button Text (Optional)
```liquid
<!-- Enable automatic "Add to Cart" → "Preorder Now" text changes -->
{% render 'prebuy-selling-plan', product: product, show_button_text: true %}

<button type="submit">Add to Cart</button>
<!-- Automatically becomes "Preorder Now" for preorder variants -->
```

## 🔍 Verification Steps

1. **Console Check**: Look for 🌍 and 🔄 emoji messages
2. **Form Inspect**: Find `<input class="prebuy-selling-plan">`
3. **Variant Test**: Change variants, check console updates
4. **Cart Test**: Add to cart, verify selling plan in network requests

## 🚨 Quick Fixes

| Issue | Solution |
|-------|----------|
| No debug messages | Check snippet is in product form |
| Selling plan not applied | Verify metafield structure |
| Variants not updating | Check for JavaScript errors |
| Markets not working | Verify market ID format |

## 📊 Expected Output

**Form Elements Added:**
```html
<input type="hidden" name="selling_plan" value="123456789" class="prebuy-selling-plan" data-buy-button-text="Pre-order Now">
<input type="hidden" name="properties[_locationId]" value="location123" class="prebuy-location-property">
```

**Button Attributes Added:**
```html
<button type="submit" data-original-text="Add to Cart" data-prebuy-button-active="true">Pre-order Now</button>
```

**Console Messages:**
```
🌍 Prebuy Markets Info (Priority-based Structure): {...}
🔄 Prebuy Variant Change (Priority-based Structure): {...}
```

## 🎯 Need Help?

1. Check [INSTALLATION.md](INSTALLATION.md) for detailed steps
2. Review browser console for error messages
3. Test with a simple product first
4. Verify Prebuy app configuration in Shopify admin
