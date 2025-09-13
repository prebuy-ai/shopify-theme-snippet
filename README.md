# Prebuy Selling Plan Integration Snippet

A universal Shopify Liquid snippet that integrates Prebuy selling plans with any Shopify theme, including full support for Shopify Markets and multi-location setups.

## 🚀 Features

- **Universal Theme Compatibility**: Works with any Shopify theme without modification
- **Shopify Markets Support**: Automatically handles multiple markets and locations
- **Priority-Based Location Matching**: Selects the best campaign based on location priorities
- **Dynamic Variant Updates**: Automatically updates selling plans when customers change variants
- **Product Page Text Display**: Optional messaging display for customers
- **Debug-Friendly**: Comprehensive console logging for troubleshooting
- **Lightweight**: No external dependencies, pure Liquid and JavaScript

## 📋 What's Included

```
PREBUY-THEME-SNIPPET/
├── prebuy-selling-plan.liquid      # Main integration snippet
├── examples/
│   ├── buy-buttons-integration.liquid    # Example buy-buttons integration
│   └── product-card-integration.liquid   # Example product card integration
├── INSTALLATION.md                 # Detailed installation guide
└── README.md                      # This file
```

## 🛠️ Quick Start

1. **Upload the snippet** to your theme's `snippets/` folder
2. **Add to your product form** after the variant ID input:
   ```liquid
   {% render 'prebuy-selling-plan', product: product %}
   ```
3. **Optional: Add product page text** after your buy button:
   ```liquid
   {% render 'prebuy-selling-plan', product: product, show_text: true %}
   ```

See [INSTALLATION.md](INSTALLATION.md) for detailed instructions.

## 🔧 How It Works

### Metafield Structure

The snippet expects specific metafield structures:

**Variant Campaigns** (`variant.metafields.prebuy.campaigns`):
```json
[
  {
    "locationId": "12345678",
    "sellingPlanId": "gid://shopify/SellingPlan/987654321", 
    "productPageText": "Pre-order now for delivery in 2 weeks"
  }
]
```

**Market Configuration** (`shop.metafields.prebuy.markets`):
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

### Location Matching Logic

1. **Market Detection**: Identifies the current Shopify market
2. **Campaign Filtering**: Finds all campaigns with matching location IDs
3. **Priority Selection**: Chooses the campaign with the lowest priority number (1 = highest priority)
4. **Dynamic Updates**: Recalculates when variants change

## 🌍 Shopify Markets Support

The snippet fully supports Shopify Markets by:

- Detecting the current market automatically
- Matching campaigns to market-specific locations
- Prioritizing campaigns based on location preferences
- Handling market switching dynamically

## 📊 Debug Information

The snippet provides comprehensive debugging through browser console logs:

- **🌍 Market Info**: Current market, location priorities, campaign structure
- **🔄 Variant Changes**: Detailed logging when variants change
- **Campaign Matching**: Shows which campaigns matched and why

## 🎯 Usage Examples

### Basic Product Form Integration
```liquid
<form action="{{ routes.cart_add_url }}" method="post">
  <input type="hidden" name="id" value="{{ product.selected_or_first_available_variant.id }}">
  {% render 'prebuy-selling-plan', product: product %}
  <button type="submit">Add to Cart</button>
</form>
```

### With Product Page Text
```liquid
<form action="{{ routes.cart_add_url }}" method="post">
  <input type="hidden" name="id" value="{{ product.selected_or_first_available_variant.id }}">
  {% render 'prebuy-selling-plan', product: product %}
  <button type="submit">Add to Cart</button>
</form>

{% render 'prebuy-selling-plan', product: product, show_text: true %}
```

### Collection Page Quick Buy
```liquid
<form action="{{ routes.cart_add_url }}" method="post" class="product-card-form">
  <input type="hidden" name="id" value="{{ card_product.selected_or_first_available_variant.id }}">
  {% render 'prebuy-selling-plan', product: card_product %}
  <button type="submit">Quick Buy</button>
</form>
```

## 🔍 Verification Checklist

After installation, verify:

- [ ] Browser console shows Prebuy debug messages (🌍 and 🔄 emojis)
- [ ] Hidden inputs with classes `prebuy-selling-plan` and `prebuy-location-property` exist
- [ ] Variant changes trigger console logs
- [ ] Selling plan parameter appears in cart requests
- [ ] Product page text displays when configured

## 🚨 Troubleshooting

### Common Issues

**No debug messages**: Check snippet placement in product form
**Selling plan not applied**: Verify metafield structure and selling plan IDs
**Variant changes not detected**: Multiple fallback methods included, check for JS errors
**Markets not working**: Verify market ID format and location mapping

See [INSTALLATION.md](INSTALLATION.md) for detailed troubleshooting.

## 📱 Theme Compatibility

Tested and compatible with:
- **Dawn** (Shopify's reference theme)
- **Debut** (Legacy Shopify theme)
- **Brooklyn** (Popular third-party theme)
- **Impulse** (Out of the Box themes)
- **Turbo** (Out of the Box themes)
- **Most custom themes**

The snippet uses multiple detection methods to ensure compatibility across different theme architectures.

## 🔄 Version History

### v1.0.0
- Initial release with Markets support
- Priority-based location matching
- Universal theme compatibility
- Comprehensive debugging

## 📞 Support

For integration support:
1. Check browser console for debug messages
2. Verify metafield structure matches examples
3. Test with simple products first
4. Review [INSTALLATION.md](INSTALLATION.md) troubleshooting section

## 📄 License

This snippet is provided as-is for integration with the Prebuy app. Modify as needed for your specific use case.

---

**Ready to integrate?** Start with [INSTALLATION.md](INSTALLATION.md) for step-by-step instructions.
