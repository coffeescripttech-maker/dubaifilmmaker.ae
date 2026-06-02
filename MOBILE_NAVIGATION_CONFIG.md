# Mobile Navigation Configuration Guide

## Overview
The mobile navigation behavior can now be fully controlled through `config.json`. This allows you to easily toggle features without editing HTML/CSS.

## Configuration Options

### Location: `config.json`

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
    },
    "homeBoxLinks": {
      "mobileViewProject": {
        "enabled": false,
        "description": "Enable/disable 'view project' button on mobile in box--home"
      }
    },
    "mobileNavigation": {
      "swipeGestures": {
        "enabled": true,
        "description": "Enable/disable swipe left/right gestures on mobile",
        "swipeThreshold": 50,
        "swipeThresholdDescription": "Minimum swipe distance in pixels (default: 50)"
      },
      "navigationArrows": {
        "enabled": false,
        "description": "Show/hide navigation arrows (prev/next) on mobile"
      }
    }
  }
}
```

## Feature Descriptions

### 1. **Navigation Links** (`navigation.worksPage|aboutPage|contactPage.enabled`)
- **Default**: `true` (visible)
- **When `true`**: Link appears in navigation menu (both mobile & desktop)
- **When `false`**: Link is hidden from navigation menu (both mobile & desktop)
- **Applies to**: Both mobile and desktop views

### 2. **View Project Button** (`mobileViewProject.enabled`)
- **Default**: `false` (hidden)
- **When `true`**: Shows a "view project" button below the video on mobile
- **When `false`**: Users tap the video directly to open projects

### 2. **Swipe Gestures** (`swipeGestures.enabled`)
- **Default**: `true` (enabled)
- **When `true`**: Users can swipe left/right to navigate between projects
- **When `false`**: Swipe gestures are disabled
- **Swipe Threshold**: Controls sensitivity (lower = easier to trigger)
  - Default: `50` pixels
  - Recommended range: `30-80` pixels

### 3. **Navigation Arrows** (`navigationArrows.enabled`)
- **Default**: `false` (hidden on mobile)
- **When `true`**: Shows prev/next arrow buttons on mobile
- **When `false`**: Arrows are hidden on mobile (always visible on desktop)

## Recommended Configurations

### Current Setup (About Page Hidden) ✅
```json
{
  "navigation": {
    "worksPage": { "enabled": true },
    "aboutPage": { "enabled": false },
    "contactPage": { "enabled": true }
  },
  "mobileViewProject": { "enabled": false },
  "swipeGestures": { "enabled": true, "swipeThreshold": 50 },
  "navigationArrows": { "enabled": true }
}
```
**Result**: About page hidden, Works + Contact visible, swipe gestures + arrows on mobile

### All Pages Visible
```json
{
  "navigation": {
    "worksPage": { "enabled": true },
    "aboutPage": { "enabled": true },
    "contactPage": { "enabled": true }
  }
}
```
**Result**: All navigation links visible

### Minimal Navigation (Works Only)
```json
{
  "navigation": {
    "worksPage": { "enabled": true },
    "aboutPage": { "enabled": false },
    "contactPage": { "enabled": false }
  }
}
```
**Result**: Only Works link visible

## How It Works

1. **Config Loading**: The script reads `config.json` via `window.siteConfig`
2. **CSS Classes**: Applies classes to `<html>` element based on config:
   - `mobile-swipe-enabled` - Enables touch-action CSS for swipes
   - `mobile-arrows-enabled` - Shows arrow buttons on mobile
   - `mobile-view-project-enabled` - Shows "view project" button
3. **Feature Initialization**: Only active features are initialized

## Testing Changes

1. Edit `config.json`
2. Save the file
3. Refresh the page (hard refresh: Ctrl+Shift+R)
4. Check console for confirmation:
   ```
   ✓ Mobile swipe gestures enabled
   ✓ Mobile navigation arrows disabled
   ✓ Mobile view project button disabled
   ```

## Desktop Behavior

**Note**: Desktop always shows:
- Navigation arrows (prev/next)
- No "view project" button
- No swipe gestures (not applicable)

Mobile config options **only affect mobile devices** (screen width < 768px).

## Troubleshooting

### Changes not applying?
1. **Hard refresh the page** (Ctrl+Shift+R or Cmd+Shift+R)
2. **Check browser console** for errors or warnings
3. **Verify config.json is valid JSON** (no trailing commas, proper syntax)
4. **Use the test page**: Open `TEST_MOBILE_CONFIG.html` in your browser to see:
   - Whether config is loading
   - Which CSS classes are applied
   - Current mobile detection status
   - Live config values

### Config shows enabled but nothing happens?
1. **Check if you're on mobile**: Open browser DevTools (F12) → Toggle device toolbar
2. **Verify screen width**: Must be < 768px for mobile features
3. **Check console logs**: Should see messages like:
   ```
   ✅ Mobile navigation arrows enabled - added .mobile-arrows-enabled class
   ```
4. **Inspect HTML element**: Right-click page → Inspect → Check `<html>` tag for classes:
   - `.mobile-swipe-enabled`
   - `.mobile-arrows-enabled`
   - `.mobile-view-project-enabled`

### Arrows still not visible on mobile?
1. Open DevTools Console (F12)
2. Type: `document.querySelector('.box--home__buttons-mobile')`
3. If it returns `null`, the element doesn't exist yet
4. If it exists, check computed styles:
   ```javascript
   const el = document.querySelector('.box--home__buttons-mobile');
   console.log(window.getComputedStyle(el).display);
   // Should show "flex" if enabled, "none" if disabled
   ```

### Swipe gestures too sensitive?
- Increase `swipeThreshold` (e.g., `70` or `80`)

### Swipe gestures not working?
- Check that `swipeGestures.enabled` is `true`
- Verify you're on a touch device
- Check console for initialization messages

## Future Enhancements

Potential additions:
- `swipeAnimation`: Enable/disable visual feedback during swipe
- `autoplayEnabled`: Control video autoplay on mobile
- `vibrationFeedback`: Haptic feedback on swipe (iOS)
