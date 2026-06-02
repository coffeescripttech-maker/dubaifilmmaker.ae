# Navigation Visibility Control Guide

## Overview
You can now control which navigation links appear in the header menu through `config.json`. This works on **both mobile and desktop** views.

## Current Settings

### config.json
```json
{
  "features": {
    "navigation": {
      "worksPage": {
        "enabled": true     ← Works link is VISIBLE
      },
      "aboutPage": {
        "enabled": false    ← About link is HIDDEN
      },
      "contactPage": {
        "enabled": true     ← Contact link is VISIBLE
      }
    }
  }
}
```

### Result
Navigation menu shows only:
- ✅ **Homepage**
- ✅ **Works**
- ❌ **About** (hidden)
- ✅ **Contact**

## How It Works

### 1. Config Setting
When you set `"enabled": false`, the system:
1. Reads the config from `config.json`
2. Applies CSS class to `<html>` element (e.g., `hide-nav-about`)
3. CSS rule hides the corresponding `<li>` element

### 2. CSS Classes Applied
```css
html.hide-nav-about .header__subnav li[data-slug="about"] {
  display: none !important;
}
```

### 3. HTML Structure
The navigation items have `data-slug` attributes:
```html
<li data-slug="works">
  <a href="works">Works</a>
</li>
<li data-slug="about">
  <a href="about.html">About</a>
</li>
<li data-slug="contact">
  <a href="contact">Contact</a>
</li>
```

## Testing

### Quick Visual Test
1. Open `index.html` in browser
2. Look at navigation menu (top right on desktop, menu button on mobile)
3. Should see: **Homepage | Works | Contact** (no About)

### Console Test
Open browser console (F12) and run:
```javascript
setTimeout(() => {
  const nav = window.siteConfig?.features?.navigation;
  console.log('About enabled in config?', nav?.aboutPage?.enabled);
  
  const aboutLi = document.querySelector('.header__subnav li[data-slug="about"]');
  if (aboutLi) {
    const style = window.getComputedStyle(aboutLi);
    console.log('About link display:', style.display);
    console.log('Is hidden?', style.display === 'none');
  }
}, 2000);
```

Expected output:
```
About enabled in config? false
About link display: none
Is hidden? true
```

### Using Test Page
1. Open `TEST_MOBILE_CONFIG.html`
2. Look at **"4. Navigation Visibility"** section
3. Should show:
   - ✅ Works Page: ENABLED
   - ❌ About Page: DISABLED
   - ✅ Contact Page: ENABLED

## Changing Settings

### Show About Page Again
Edit `config.json`:
```json
"aboutPage": {
  "enabled": true    ← Change false to true
}
```

### Hide Works Page
```json
"worksPage": {
  "enabled": false   ← Change true to false
}
```

### Hide All Secondary Pages
```json
"navigation": {
  "worksPage": { "enabled": false },
  "aboutPage": { "enabled": false },
  "contactPage": { "enabled": false }
}
```
Result: Only Homepage link visible

## Common Use Cases

### 1. Coming Soon Mode
Hide pages that aren't ready:
```json
{
  "worksPage": { "enabled": false },
  "aboutPage": { "enabled": false },
  "contactPage": { "enabled": true }  // Keep contact for inquiries
}
```

### 2. Portfolio Mode
Show only work:
```json
{
  "worksPage": { "enabled": true },
  "aboutPage": { "enabled": false },
  "contactPage": { "enabled": false }
}
```

### 3. Full Site
Show everything:
```json
{
  "worksPage": { "enabled": true },
  "aboutPage": { "enabled": true },
  "contactPage": { "enabled": true }
}
```

## Important Notes

### ✅ Works On
- Desktop header navigation
- Mobile menu
- Both light and dark themes

### ❌ Doesn't Prevent
- Direct URL access (users can still type `/about.html`)
- Search engine indexing
- Linked references from other pages

### 🔒 To Fully Hide a Page
If you want to completely prevent access:
1. Hide from navigation (config.json)
2. Delete or rename the page file
3. Or add password protection

## Troubleshooting

### Link still visible?
1. **Hard refresh** (Ctrl+Shift+R or Cmd+Shift+R)
2. **Check config syntax** - ensure valid JSON (no trailing commas)
3. **Check console** - should see: `🔗 About page hidden from navigation`
4. **Inspect HTML** - `<html>` should have `hide-nav-about` class
5. **Check computed styles**:
   ```javascript
   const li = document.querySelector('li[data-slug="about"]');
   console.log(window.getComputedStyle(li).display); // Should be "none"
   ```

### Changes not applying?
1. Clear browser cache
2. Check that `window.siteConfig` is loaded:
   ```javascript
   console.log(window.siteConfig?.features?.navigation);
   ```
3. Verify script execution order:
   - `site-config.js` must load first
   - Mobile navigation script runs after config loads

## Files Modified

1. ✅ `config.json` - Set `aboutPage.enabled: false`
2. ✅ `index.html` - Added CSS rules and config reader
3. ✅ `TEST_MOBILE_CONFIG.html` - Added navigation visibility check
4. ✅ `QUICK_TEST.txt` - Added navigation test commands

## Next Steps

1. **Test on both mobile and desktop**
2. **Check navigation menu** - About should be missing
3. **Try toggling settings** - Change enabled values and refresh
4. **Use test tools** if issues occur

The navigation visibility system is fully functional and controlled by config! 🎉
