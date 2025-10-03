# Markets Config Troubleshooting Guide

This guide helps debug issues with Prebuy markets configuration not being available or working correctly.

## Quick Diagnostic Steps

### 1. Use the Debug Snippet

Replace your current Prebuy integration with the debug version temporarily:

```liquid
<!-- Replace this line: -->
{% render 'prebuy-selling-plan', product: product %}

<!-- With this debug version: -->
{% render 'prebuy-selling-plan-debug', product: product, show_debug: true %}
```

The debug version will show comprehensive information about:
- Markets metafield availability and structure
- Current market detection
- Campaign matching process
- Inventory validation
- Step-by-step processing logs

### 2. Check Browser Console

Open Developer Tools → Console and look for:
- `🌍 Prebuy Markets Debug` group with detailed logs
- Any error messages or warnings

### 3. Verify Markets Metafield Structure

The expected structure for `shop.metafields.prebuy.markets` is:

```json
[
  {
    "id": "93117382984",
    "locationPriorities": [
      {
        "locationId": "12345678",
        "priority": 1,
        "label": "US Warehouse"
      },
      {
        "locationId": "87654321",
        "priority": 2,
        "label": "EU Warehouse"
      }
    ]
  }
]
```

## Common Issues and Solutions

### Issue 1: Markets Metafield Not Found

**Symptoms:**
- Debug shows: "shop.metafields.prebuy.markets exists: NO"
- No market configuration detected

**Causes & Solutions:**

1. **Metafield not created:**
   - Create the metafield in Shopify Admin
   - Navigate to: Settings → Metafields → Shop
   - Add metafield with namespace: `prebuy`, key: `markets`
   - Type: JSON

2. **Wrong metafield definition:**
   - Ensure metafield definition matches exactly:
     - Namespace: `prebuy`
     - Key: `markets`
     - Type: JSON
     - Owner: Shop

3. **Permissions issue:**
   - Ensure the app/integration has access to read shop metafields
   - Check Shopify app permissions

### Issue 2: Markets Metafield Exists but Value is Empty

**Symptoms:**
- Debug shows: "shop.metafields.prebuy.markets exists: YES"
- Debug shows: "shop.metafields.prebuy.markets.value exists: NO"

**Causes & Solutions:**

1. **Empty metafield value:**
   - Check the metafield value in Shopify Admin
   - Ensure it contains valid JSON data
   - Set the value to your markets configuration array

2. **Invalid JSON format:**
   - Validate your JSON using a JSON validator
   - Common issues: missing quotes, trailing commas, wrong brackets

3. **Metafield not saved:**
   - Ensure you clicked "Save" after setting the metafield value
   - Check if there were any validation errors during save

### Issue 3: Market ID Mismatch

**Symptoms:**
- Debug shows markets array but "NO MARKET CONFIG FOUND FOR CURRENT MARKET"
- Current market ID doesn't match any market in the array

**Causes & Solutions:**

1. **Wrong market ID format:**
   - Current market: `gid://shopify/Market/93117382984`
   - Metafield should use numeric format: `"93117382984"`
   - **Fix:** Remove the GID prefix, use only the numeric part

2. **Incorrect market ID:**
   - Check your actual market ID in Shopify Admin
   - Navigate to: Settings → Markets
   - Compare with your metafield configuration

3. **String vs numeric mismatch:**
   - Ensure market IDs in metafield are strings: `"93117382984"`
   - Not numbers: `93117382984`

### Issue 4: Location Priorities Structure Issues

**Symptoms:**
- Market config found but no campaigns match
- Debug shows "Missing locationId or locationPriorities"

**Causes & Solutions:**

1. **Missing locationPriorities array:**
   ```json
   // ❌ Wrong - missing locationPriorities
   {
     "id": "93117382984"
   }
   
   // ✅ Correct
   {
     "id": "93117382984",
     "locationPriorities": [...]
   }
   ```

2. **Wrong locationPriorities structure:**
   ```json
   // ❌ Wrong
   "locationPriorities": {
     "12345678": 1
   }
   
   // ✅ Correct
   "locationPriorities": [
     {
       "locationId": "12345678",
       "priority": 1,
       "label": "US Warehouse"
     }
   ]
   ```

3. **Missing required fields:**
   - Each location priority must have: `locationId`, `priority`
   - `label` is optional but recommended

### Issue 5: Campaign Location ID Mismatch

**Symptoms:**
- Market config found but "NO CAMPAIGN SELECTED"
- Debug shows location matches attempted but failed

**Causes & Solutions:**

1. **Location ID format mismatch:**
   - Campaign locationId: `"12345678"`
   - Market locationId: `"12345678"`
   - Ensure both use the same format (string)

2. **Wrong location IDs:**
   - Verify location IDs in Shopify Admin
   - Navigate to: Settings → Locations
   - Compare with your campaign metafields

3. **Missing campaigns:**
   - Check variant metafields: `variant.metafields.prebuy.campaigns`
   - Ensure campaigns array contains valid location IDs

### Issue 6: Inventory Validation Blocking Preorders

**Symptoms:**
- Everything configured correctly but preorders not showing
- Debug shows "Has stock (>0): false"

**Causes & Solutions:**

1. **Variant out of stock:**
   - Check variant inventory in Shopify Admin
   - Prebuy only shows preorders for variants with positive inventory
   - **Note:** This is intentional behavior for inventory validation

2. **Inventory tracking issues:**
   - Ensure inventory tracking is enabled for the variant
   - Check inventory levels across all locations

## Step-by-Step Verification

### 1. Verify Metafield Creation
```
1. Go to Shopify Admin → Settings → Metafields
2. Click "Shop" tab
3. Look for "prebuy.markets" metafield
4. If not found, create it:
   - Namespace: prebuy
   - Key: markets
   - Type: JSON
```

### 2. Verify Metafield Content
```
1. Open the prebuy.markets metafield
2. Check the JSON structure matches the expected format
3. Validate JSON syntax
4. Ensure market IDs are strings, not numbers
```

### 3. Verify Current Market ID
```
1. Add debug snippet to your product page
2. Check "Current Market" section in debug output
3. Note the numeric market ID
4. Ensure this ID exists in your markets metafield
```

### 4. Verify Location Matching
```
1. Check "Campaign Matching" section in debug output
2. Verify campaign location IDs match market location IDs
3. Ensure location priorities are properly configured
```

### 5. Verify Inventory
```
1. Check "Inventory Status" section in debug output
2. Ensure variant has positive inventory
3. If needed, adjust inventory or modify validation logic
```

## Testing Checklist

- [ ] Markets metafield exists and has valid JSON
- [ ] Current market ID matches an entry in markets array
- [ ] Market has locationPriorities array with correct structure
- [ ] Campaign location IDs match market location IDs
- [ ] Variant has positive inventory
- [ ] Browser console shows no errors
- [ ] Debug output shows "Is market compatible: true"

## Advanced Debugging

### Console Commands

You can run these commands in the browser console for additional debugging:

```javascript
// Check variant data
console.log('Variant Data:', JSON.parse(document.querySelector('.prebuy-variant-data').textContent));

// Check selling plan inputs
console.log('Selling Plan Inputs:', document.querySelectorAll('.prebuy-selling-plan'));

// Check current market
console.log('Current Market:', window.Shopify?.locale?.market || 'Not available');
```

### Network Tab Debugging

1. Open Developer Tools → Network tab
2. Reload the product page
3. Look for any failed requests related to metafields
4. Check for CORS or permission errors

## Getting Help

If you're still experiencing issues after following this guide:

1. **Include debug output** from the debug snippet
2. **Specify your Shopify setup:**
   - Theme name/version
   - Markets configuration
   - Number of locations
3. **Describe the expected vs actual behavior**
4. **Include any console errors or warnings**

The debug snippet provides comprehensive logging that should help identify the specific issue with your markets configuration.
