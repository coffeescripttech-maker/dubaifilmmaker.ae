# Navigation Configuration Guide

## Overview
Control which navigation links appear in the header menu through `config.json`. Works on both mobile and desktop.

## Configuration Location

**File**: `config.json`

```json
{
  "features": {
    "navigation": {
      "worksPage": {
        "enabled": true,
        "description": "Enable/disable Works navigation link"
      },
      "aboutPage": {
        "enabled": false,
        "description": "Enable/disable About navigation link"
      },
      "contactPage": {
        "enabled": true,
        "description": "Enable/disable Contact navigation link"
      }
    }
  }
}
```

## Navigation Options

### 1. Works Page (`worksPage.enabled`)
- **Default**: `true` (visible)
- **When `true`**: "Works" link appears in navigation menu
- **When `false`**: "Works" link is hidden from header

### 2. About Page (`aboutPage.enabled`)
- **Default**: `false` (hidden)
- **When `true`**: "About" link appears in navigation menu
- **When `false`**: "About" link is completely hidden from header (mobile & desktop)

**Current**: Set to `false` - About page is hidden

### 3. Contact Page (`contactPage.enabled`)
- **Default**: `true` (visible)
- **When `true`**: "Contact" link appears in navigation menu
- **When `false`**: "Contact" link is hidden from header

## How It Works

1. **Config Loading**: `site-config.js` reads `config.json` on page load
2. **DOM Manipulation**: Finds `<li data-slug="about">` (or works/contact)
3. **CSS Display**: Sets `display: none` to hide the entire list item
4. **Link Disabling**: Also disables the `<a>` tag inside for extra safety

This approach:
- ✅ Works on mobile & desktop (responsive)
- ✅ Hides the entire menu item (not just disables)
- ✅ Prevents clicks completely
- ✅ No visual gap in navigation

## Current Configuration (Active)

```json
{
  "navigation": {
    "worksPage": { "enabled": true },   // ✅ Visible
    "aboutPage": { "enabled": false },  // ❌ Hidden (as requested)
    "contactPage": { "enabled": true }  // ✅ Visible
  }
}
```

**Result**: Navigation shows only "Homepage", "Works", and "Contact"

## Testing

### Option 1: Visual Inspection
1. Open `index.html` or `contact.html`
2. Check header navigation
3. About link should NOT appear

### Option 2: Use Test Page
1. Open `TEST_NAVIGATION_CONFIG.html`
2. Shows mock navigation with real config
3. Displays visibility status for each link
4. Shows config values

### Option 3: Browser Console
Open Console (F12) and run:

```javascript
// Check if config is loaded
console.log(window.siteConfig?.features?.navigation);

// Check About page status
const aboutNav = document.querySelector('li[data-slug="about"]');
console.log('About nav element:', aboutNav);
console.log('Display:', aboutNav ? window.getComputedStyle(aboutNav).display : 'NOT FOUND');

// Expected: "none"
```

## Changing Navigation Visibility

### To Show About Page Again:
```json
"aboutPage": {
  "enabled": true
}
```

### To Hide Works Page:
```json
"worksPage": {
  "enabled": false
}
```

### To Hide All Navigation (Minimal Mode):
```json
"worksPage": { "enabled": false },
"aboutPage": { "enabled": false },
"contactPage": { "enabled": false }
```

## Important Notes

### URLs Still Work
Hiding a navigation link doesn't prevent direct URL access:
- `https://yoursite.com/about` still works
- `https://yoursite.com/works` still works
- Only the navigation menu changes

### Both Mobile & Desktop
The same config applies to:
- Desktop header menu
- Mobile hamburger menu
- All screen sizes

### No Code Editing Required
Just edit `config.json` and refresh - no HTML/CSS changes needed!

## Troubleshooting

### Link still visible?
1. Hard refresh (Ctrl+Shift+R or Cmd+Shift+R)
2. Check `config.json` syntax (no trailing commas)
3. Open Console - look for:
   ```
   ✓ about navigation item hidden (mobile & desktop)
   ```

### Link missing but should be visible?
1. Check `config.json` - is `enabled: true`?
2. Open Console - should see:
   ```
   ✓ about navigation item visible (mobile & desktop)
   ```

### Config not applying?
1. Check `window.siteConfig` in console
2. Verify `site-config.js` is loaded
3. Use `TEST_NAVIGATION_CONFIG.html` to diagnose

## Files Involved

1. **`config.json`** - Configuration source
2. **`assets/js/site-config.js`** - Reads config and applies navigation visibility
3. **`index.html`** - Homepage navigation
4. **`contact.html`** - Contact page navigation
5. **`about.html`** - About page navigation (if exists)
6. **`works.html`** - Works page navigation (if exists)

All pages share the same navigation config!

## Example Console Output

When config is working correctly:

```
✓ Site config loaded: {features: {...}}
✓ about navigation item hidden (mobile & desktop)
✓ works navigation item visible (mobile & desktop)
✓ contact navigation item visible (mobile & desktop)
```

## Related Config Options

See also:
- **`MOBILE_NAVIGATION_CONFIG.md`** - Mobile swipe gestures & arrows
- **`CONFIG_FIX_SUMMARY.md`** - Config system fixes
- **`TEST_MOBILE_CONFIG.html`** - Mobile features test page
- **`TEST_NAVIGATION_CONFIG.html`** - Navigation visibility test page
