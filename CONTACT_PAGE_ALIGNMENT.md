# Contact Page Alignment Guide

## ✅ Implementation Complete

All contact information blocks are now **perfectly left-aligned** with "Dubai, UAE" as the vertical anchor point.

## What Was Changed

### 1. Created Contact CSS Template
**File**: `assets/css/templates/contact.css`

This CSS file overrides the default right-alignment behavior and ensures all contact blocks are left-aligned on both mobile and desktop.

### 2. Updated Contact HTML
**File**: `contact.html`

Added the contact CSS template to the page:
```html
<link href="assets/css/templates/contact.css" rel="stylesheet" />
```

## Alignment Details

### Desktop (≥ 768px)
- **Staff Block**: Left-aligned
- **Address Block**: Left-aligned  
- **Contact Info**: Left-aligned
- **All Text**: Left-aligned
- **Anchor Point**: "Dubai, UAE"

### Mobile (< 768px)
- **All Blocks**: Left-aligned (consistent with desktop)
- **All Text**: Left-aligned

## CSS Implementation

The CSS uses `!important` flags to override the existing styles from `build.min.css`:

```css
.contact-inner-wrapper {
  align-items: flex-start !important;  /* Force left alignment */
  flex-direction: column !important;   /* Stack vertically */
}

.box--staff,
.box--address {
  align-self: flex-start !important;   /* Each block starts at left */
  text-align: left !important;         /* Text aligned left */
}
```

## Blocks Affected

1. **Staff Listings** (`.box--staff`)
   - Department headings
   - Member names
   - Email addresses

2. **Address Information** (`.box--address`)
   - Street address (if present)
   - **"Dubai, UAE"** ← Main alignment anchor
   - Phone number (T:)
   - Email (E:)
   - Social media links (Vimeo, Instagram)

3. **All Text Elements**
   - Headers (`<h2>`)
   - Paragraphs (`<p>`)
   - Links (`<a>`)
   - Lists (`<ul>`, `<li>`)

## Visual Result

Before (Right-aligned on desktop):
```
                                    Production
                               email@example.com
                               email@example.com
                               
                                   Dubai, UAE
                              T: +971 XX XXX XXXX
                         E: contact@dubaifilmmaker.com
```

After (Left-aligned):
```
Production
email@example.com
email@example.com

Dubai, UAE
T: +971 XX XXX XXXX
E: contact@dubaifilmmaker.com
```

## Testing

### How to Verify
1. Open `contact.html` in browser
2. Check desktop view (width ≥ 768px)
3. Verify all blocks are flush left
4. Check mobile view (width < 768px)
5. Verify consistent left alignment

### Expected Console
No console logs - this is pure CSS styling.

## Files Modified

1. ✅ **Created**: `assets/css/templates/contact.css` - New CSS file
2. ✅ **Updated**: `contact.html` - Added CSS link

## Compatibility

- ✅ Mobile (< 768px)
- ✅ Tablet (≥ 768px)
- ✅ Desktop (≥ 1024px)
- ✅ Large Desktop (≥ 1440px)

Works across all screen sizes with consistent left alignment.

## Future Updates

If you need to adjust the alignment:

### Change Alignment Point
Edit `assets/css/templates/contact.css`:

```css
/* For center alignment */
.contact-inner-wrapper {
  align-items: center !important;
}

/* For right alignment */
.contact-inner-wrapper {
  align-items: flex-end !important;
}
```

### Add Spacing Between Blocks
```css
.box--staff {
  margin-bottom: 3rem;
}

.box--address {
  margin-top: 2rem;
}
```

### Adjust Text Alignment Only
```css
/* Keep blocks positioned as is, but change text alignment */
.box--staff *,
.box--address * {
  text-align: center; /* or 'right' */
}
```

## Notes

- The CSS uses `!important` to override existing `build.min.css` styles
- "Dubai, UAE" serves as the visual anchor for vertical alignment
- All blocks share the same left edge
- No JavaScript required - pure CSS solution
- Works with dynamically loaded content from `page-renderer.js`

## Related Files

- `assets/js/page-renderer.js` - Renders contact content dynamically
- `data/contact.json` - Contact data source (if exists)
- `assets/dist/build.min.css` - Main stylesheet (overridden by contact.css)
