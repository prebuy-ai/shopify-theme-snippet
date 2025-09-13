# Prebuy Selling Plan Integration - Installation Guide

This guide will walk you through integrating the Prebuy selling plan functionality into any Shopify theme.

## Prerequisites

Before installing, ensure you have:
- Access to your Shopify theme code editor
- Basic understanding of Liquid templating
- Prebuy app installed and configured in your Shopify store
- Selling plans created in your Prebuy app

## Installation Steps

### Step 1: Upload the Snippet

1. In your Shopify admin, go to **Online Store > Themes**
2. Click **Actions > Edit code** on your active theme
3. In the **Snippets** folder, click **Add a new snippet**
4. Name it `prebuy-selling-plan` (without the .liquid extension)
5. Copy the contents of `prebuy-selling-plan.liquid` from this repository
6. Paste it into the new snippet and **Save**

### Step 2: Integrate into Product Forms

You need to add the Prebuy snippet to your product forms. The most common locations are:

#### Option A: Buy Buttons Snippet (Recommended)
Most themes have a `buy-buttons.liquid` snippet that handles the product form:

1. Open `snippets/buy-buttons.liquid` (or similar file like `product-form.liquid`)
2. Find the line with the variant ID input: `<input type="hidden" name="id" value="...">`
3. **Immediately after** that line, add:
   ```liquid
   {% render 'prebuy-selling-plan', product: product %}
   ```

#### Option B: Product Template/Section
If your theme doesn't use a buy-buttons snippet:

1. Open your main product template (usually `sections/main-product.liquid` or `templates/product.liquid`)
2. Find the product form (`<form action="{{ routes.cart_add_url }}"...>`)
3. Locate the variant ID input and add the render line after it

### Step 3: Add Product Page Text (Optional)

To display Prebuy messaging on the product page:

1. In the same file where you added the snippet, find a good location for the text (usually after the buy button)
2. Add this line:
   ```liquid
   {% render 'prebuy-selling-plan', product: product, show_text: true %}
   ```

### Step 4: Product Cards (Optional)

For collection pages with quick-buy functionality:

1. Open your product card snippet (usually `snippets/card-product.liquid` or `snippets/product-card.liquid`)
2. Find any product forms in the card
3. Add the render line after the variant ID input:
   ```liquid
   {% render 'prebuy-selling-plan', product: card_product %}
   ```

## Theme-Specific Integration Examples

### Dawn Theme
```liquid
<!-- In snippets/buy-buttons.liquid -->
<input type="hidden" name="id" value="{{ product.selected_or_first_available_variant.id }}">
{% render 'prebuy-selling-plan', product: product %}
```

### Debut Theme
```liquid
<!-- In snippets/product-form.liquid -->
<select name="id" class="product-form__variants">...</select>
{% render 'prebuy-selling-plan', product: product %}
```

### Brooklyn Theme
```liquid
<!-- In sections/product-template.liquid -->
<input type="hidden" name="id" value="{{ product.selected_or_first_available_variant.id }}">
{% render 'prebuy-selling-plan', product: product %}
```

## Verification

After installation, verify the integration:

1. **Check Browser Console**: Open developer tools on a product page and look for Prebuy debug messages starting with 🌍 or 🔄
2. **Inspect Form**: Right-click the buy button and inspect the form. You should see:
   - `<input class="prebuy-selling-plan" ...>`
   - `<input class="prebuy-location-property" ...>`
3. **Test Variant Changes**: Change product variants and check that the console logs show variant updates
4. **Verify Selling Plan**: In the browser's Network tab, when adding to cart, check that the `selling_plan` parameter is included

## Troubleshooting

### No Debug Messages in Console
- Check that the snippet is properly included in your product form
- Ensure you're viewing a product with Prebuy campaigns configured
- Verify the product has variants with metafield data

### Selling Plan Not Applied
- Check that the hidden input `name="selling_plan"` exists and has a value
- Verify that Shopify Markets is configured correctly if using multiple markets
- Ensure the selling plan IDs in your metafields match those in Shopify

### Variant Changes Not Working
- The snippet includes multiple fallback methods for variant change detection
- Most modern themes use a pubsub system that should work automatically
- Check console for any JavaScript errors that might interfere

### Markets Not Working
- Verify your `shop.metafields.prebuy.markets` contains the correct market-to-location mapping
- Check that market IDs are in numeric format (e.g., "93117382984")
- Ensure location priorities are set correctly with priority numbers

## Support

If you encounter issues:

1. Check the browser console for Prebuy debug messages
2. Verify your metafield structure matches the expected format
3. Test with a simple product first before complex variants
4. Ensure your theme's variant selector is working properly

## Advanced Configuration

### Custom Styling
The snippet outputs these CSS classes you can style:
- `.prebuy-product-text` - Container for product page text
- `.prebuy-text` - The actual text content

### Custom JavaScript Integration
The snippet exposes `window.prebuySellingPlanInitialized` to prevent conflicts with custom implementations.

### Metafield Structure Reference

**Variant Metafields** (`variant.metafields.prebuy.campaigns`):
```json
[
  {
    "locationId": "12345678",
    "sellingPlanId": "gid://shopify/SellingPlan/987654321",
    "productPageText": "Pre-order now for delivery in 2 weeks"
  }
]
```

**Shop Metafields** (`shop.metafields.prebuy.markets`):
```json
[
  {
    "id": "93117382984",
    "locationPriorities": [
      {
        "locationId": "12345678",
        "priority": 1,
        "label": "US Warehouse"
      }
    ]
  }
]
```
