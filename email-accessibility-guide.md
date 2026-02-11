# Email Accessibility & Dark Mode Guide

## Overview
This guide provides practical instructions for adding ARIA roles and dark mode support to our existing table-based email HTML. 

---

## ARIA Roles for Semantic Structure

### Why Use ARIA in Email HTML?

We don't use semantic tags like `<h1>`, `<p>`, or `<ul>`, so ARIA roles provide another way to communicate document structure to screen readers.

### Heading Roles

Add `role="heading"` and `aria-level` to `<td>` elements that function as headings.

**Current code:**
```html
<td class="h1">
  Welcome to Our Study
</td>
```

**Enhanced with ARIA:**
```html
<td class="h1" role="heading" aria-level="1">
  Welcome to Our Study
</td>
```

**Heading Level Guidelines:**
- `aria-level="1"` - Main title (use once per email, typically for `.h1`)
- `aria-level="2"` - Major sections (typically for `.h2`)
- `aria-level="3"` - Subsections (typically for `.h3`)
- `aria-level="4"` - Minor headings (typically for `.h4`)
- `aria-level="5"` - Small headings (typically for `.h5`)

**Important:** Maintain logical hierarchy. Don't skip levels (e.g., don't go from `aria-level="1"` to `aria-level="4"`).

---

### Paragraph Roles

Add `role="paragraph"` to `<td>` elements containing body text.

**Current code:**
```html
<td class="p">
  This study examines the effects of the treatment on patients with HSDD.
</td>
```

**Enhanced with ARIA:**
```html
<td class="p" role="paragraph">
  This study examines the effects of the treatment on patients with HSDD.
</td>
```

---

### List Roles

Our lists are table-based. Add `role="list"` to the container table and `role="listitem"` to each item row.

**Current code:**
```html
<table class="ul" border="0" cellpadding="0" cellspacing="0">
  <tr>
    <td class="li" valign="top">•</td>
    <td valign="top">First benefit of the treatment</td>
  </tr>
  <tr>
    <td class="li" valign="top">•</td>
    <td valign="top">Second benefit of the treatment</td>
  </tr>
</table>
```

**Enhanced with ARIA:**
```html
<table class="ul" role="list" border="0" cellpadding="0" cellspacing="0">
  <tr role="listitem">
    <td class="li" valign="top" aria-hidden="true">•</td>
    <td valign="top">First benefit of the treatment</td>
  </tr>
  <tr role="listitem">
    <td class="li" valign="top" aria-hidden="true">•</td>
    <td valign="top">Second benefit of the treatment</td>
  </tr>
</table>
```

**Note:** `aria-hidden="true"` on the bullet prevents screen readers from announcing "bullet" before each item.

---

### Landmark Roles

Landmark roles help users navigate to major sections of the email.

**Common landmarks for email:**

```html
<!-- Main content area -->
<td class="content" role="article">
  <!-- Your main email content -->
</td>

<!-- Header/banner (if applicable) -->
<td class="header" role="banner">
  <!-- Logo, preheader -->
</td>

<!-- Footer -->
<td class="footer" role="contentinfo">
  <!-- Legal text, unsubscribe, address -->
</td>
```

**Avoid:** Don't use `role="main"` or `role="navigation"` - these are better suited for web pages than emails.

---

## Image Accessibility

### Decision Tree: Alt Text vs. ARIA Label

Use this workflow when handling images:

1. **Is the image decorative (purely visual, no information)?**
   - YES → Use `alt=""` (empty alt text)
   - NO → Continue to step 2

2. **Did the client provide alt text?**
   - YES → Continue to step 3
   - NO → Write descriptive alt text yourself

3. **Is the client's alt text appropriate?**
   - Appropriate = describes the image's purpose/content
   - Inappropriate = too vague ("image"), decorative image with alt text, or misleading
   - If appropriate → Use it
   - If inappropriate → Revise or remove it

### Examples

#### Decorative Images
Images used only for visual design (spacers, decorative borders, etc.) should have empty alt text.

```html
<img src="decorative-border.png" alt="" width="600" height="20"/>
```

#### Meaningful Images with Alt Text
Product images, charts, photos that convey information need descriptive alt text.

```html
<!-- Good alt text: describes what's shown -->
<img src="product-bottle.png" alt="VYLEESI pre-filled autoinjector pen" width="300" height="400"/>

<!-- Good alt text: describes the data -->
<img src="efficacy-chart.png" alt="Bar chart showing 60% of patients experienced improvement" width="500" height="300"/>
```

#### Background Images with ARIA
When using background images that convey meaning (via CSS or table backgrounds), use a container with `role="img"` and `aria-label`.

```html
<td role="img" aria-label="Doctor consulting with patient" style="background-image: url('consultation.jpg'); width: 600px; height: 400px;">
  <!-- Any text overlay goes here -->
</td>
```

### When Clients Don't Provide Alt Text

If a client doesn't provide alt text for a meaningful image:

1. **Best practice:** Request alt text from the client
2. **If unavailable:** Write it yourself based on the image content and context
3. **Document your decision:** Note in your workflow that you authored the alt text

### When Clients Provide Bad Alt Text

Common issues and fixes:

**Too vague:**
```html
<!-- Client provides: -->
<img src="doctor.jpg" alt="image" width="400" height="300"/>

<!-- Fix it: -->
<img src="doctor.jpg" alt="Healthcare provider discussing treatment options with patient" width="400" height="300"/>
```

**Decorative image with alt text:**
```html
<!-- Client provides: -->
<img src="pink-gradient.png" alt="pink gradient background" width="600" height="100"/>

<!-- Fix it: -->
<img src="pink-gradient.png" alt="" width="600" height="100"/>
```

**Redundant information:**
```html
<!-- If there's a heading that says "Meet Our Panel" above the image: -->

<!-- Client provides: -->
<img src="panel.jpg" alt="Meet our panel of experts" width="500" height="400"/>

<!-- Fix it (don't repeat the heading): -->
<img src="panel.jpg" alt="Four medical professionals seated at a conference table" width="500" height="400"/>
```

---

## Dark Mode Support

### Why Dark Mode Matters
Many email clients now support dark mode (Apple Mail, Outlook, Gmail). Without proper CSS, your carefully chosen colors may invert unexpectedly, causing readability issues.

### Basic Dark Mode CSS

Add this to your `<style>` block:

```css
/* Dark Mode Media Query */
@media (prefers-color-scheme: dark) {
  
  /* Prevent background color inversion */
  .white-background { background-color: #ffffff !important; }
  .black-background { background-color: #000000 !important; }
  
  /* Prevent text color inversion */
  .black { color: #120D0E !important; }
  .white { color: #ffffff !important; }
  
  /* Ensure links remain visible */
  a { color: #64B5F6 !important; }  /* Lighter blue for dark backgrounds */
  
  /* Preserve brand colors */
  .pink { color: #F0ABC1 !important; }
  .dk-julep { color: #32AB94 !important; }
}
```

### Specific Dark Mode Fixes

#### Preserve Logo/Image Visibility
Logos with transparent backgrounds may disappear in dark mode. Add a white background container:

```css
@media (prefers-color-scheme: dark) {
  .logo-container {
    background-color: #ffffff !important;
    padding: 10px !important;
  }
}
```

#### Force Light Mode for Entire Email
If your email design doesn't work well in dark mode, force light mode:

```css
@media (prefers-color-scheme: dark) {
  /* Force entire email to light mode */
  body, table, td {
    background-color: #ffffff !important;
    color: #000000 !important;
  }
}
```

**Note:** This is a last resort. It's better to design for both modes.

#### Handle Bordered Elements
Borders can become invisible in dark mode if they match the background:

```css
.border-element {
  border: 1px solid #cccccc;
}

@media (prefers-color-scheme: dark) {
  .border-element {
    border-color: #666666 !important;
  }
}
```

### Dark Mode Color Palette Recommendations

When choosing colors that work in both modes:

**Text:**
- Light mode: Dark gray (#120D0E) on white (#ffffff)
- Dark mode: Light gray (#E0E0E0) on dark (#121212)

**Links:**
- Light mode: Blue (#1c75bc)
- Dark mode: Light blue (#64B5F6)

**Backgrounds:**
- Avoid pure black (#000000) in dark mode; use dark gray (#121212) for less eye strain
- Keep brand colors consistent where possible

### Testing Dark Mode

Test your emails in:
- Apple Mail (iOS and macOS)
- Gmail app (iOS and Android)
- Outlook app (iOS and Android)

Check that:
- Text is readable
- Images are visible
- Buttons/CTAs are clear
- Brand colors are preserved

---

## Email Client-Specific ARIA Support

### ARIA Support by Email Client

| Client | ARIA Support | Notes |
|--------|-------------|-------|
| Apple Mail | ✅ Good | Full support for roles and labels |
| Gmail | ⚠️ Partial | Strips some ARIA; test thoroughly |
| Outlook | ⚠️ Limited | Desktop version has better support than web |
| Yahoo Mail | ⚠️ Partial | Basic role support |
| Thunderbird | ✅ Good | Full support |

**Recommendation:** Always add ARIA even if support is partial. It provides progressive enhancement and doesn't break email rendering.

---

## Practical Examples from Your Codebase

### Example 1: Hero Section with Heading

**Before:**
```html
<tr>
  <td class="content-1">
    <table role="presentation" border="0" cellpadding="0" cellspacing="0" width="100%">
      <tr>
        <td class="h1 pb25">
          Join Us for an Expert Panel Discussion
        </td>
      </tr>
    </table>
  </td>
</tr>
```

**After (with ARIA):**
```html
<tr>
  <td class="content-1" role="article">
    <table role="presentation" border="0" cellpadding="0" cellspacing="0" width="100%">
      <tr>
        <td class="h1 pb25" role="heading" aria-level="1">
          Join Us for an Expert Panel Discussion
        </td>
      </tr>
    </table>
  </td>
</tr>
```

---

### Example 2: Body Text with Subheading

**Before:**
```html
<tr>
  <td class="h3 pb15">
    Event Details
  </td>
</tr>
<tr>
  <td class="p">
    Join us virtually on March 15th for an in-depth discussion about HSDD treatment options.
  </td>
</tr>
```

**After (with ARIA):**
```html
<tr>
  <td class="h3 pb15" role="heading" aria-level="2">
    Event Details
  </td>
</tr>
<tr>
  <td class="p" role="paragraph">
    Join us virtually on March 15th for an in-depth discussion about HSDD treatment options.
  </td>
</tr>
```

---

### Example 3: Panelist Bio with Image

**Before:**
```html
<tr>
  <td class="pt15" valign="top" width="45%">
    <table role="presentation" border="0" cellpadding="0" cellspacing="0" width="100%">
      <tr>
        <td class="image">
          <img src="{{asset.morse-2x.url}}" alt="{{asset.morse-2x.alt}}" width="200" height="200"/>
        </td>
      </tr>
    </table>
  </td>
  <td valign="middle" width="55%">
    <table role="presentation" border="0" cellpadding="0" cellspacing="0" width="100%">
      <tr>
        <td class="pb10">
          <strong>Emily Morse, PhD</strong>
        </td>
      </tr>
      <tr>
        <td>
          Sex Educator, <em>Sex With Emily</em>
        </td>
      </tr>
    </table>
  </td>
</tr>
```

**After (with ARIA and good alt text):**
```html
<tr>
  <td class="pt15" valign="top" width="45%">
    <table role="presentation" border="0" cellpadding="0" cellspacing="0" width="100%">
      <tr>
        <td class="image">
          <img src="{{asset.morse-2x.url}}" alt="Headshot of Emily Morse" width="200" height="200"/>
        </td>
      </tr>
    </table>
  </td>
  <td valign="middle" width="55%">
    <table role="presentation" border="0" cellpadding="0" cellspacing="0" width="100%">
      <tr>
        <td class="pb10" role="heading" aria-level="3">
          <strong>Emily Morse, PhD</strong>
        </td>
      </tr>
      <tr>
        <td role="paragraph">
          Sex Educator, <em>Sex With Emily</em>
        </td>
      </tr>
    </table>
  </td>
</tr>
```

**Alt text note:** Instead of generic "{{asset.morse-2x.alt}}", use "Headshot of Emily Morse" - this is more useful for screen reader users.

---

### Example 4: Footer with Legal Links

**Before:**
```html
<tr>
  <td class="footer">
    <table role="presentation" border="0" cellpadding="0" cellspacing="0" width="100%">
      <tr>
        <td class="legal" align="center">
          <a href="https://cosettepharma.com/terms-of-use/">Terms &amp; Conditions</a>
          <span class="link-divider">|</span>
          <a href="https://cosettepharma.com/privacy-policy/">Privacy Policy</a>
          <span class="link-divider">|</span>
          <a href="{{build.unsubscribeLink}}">Unsubscribe</a>
        </td>
      </tr>
    </table>
  </td>
</tr>
```

**After (with ARIA landmark):**
```html
<tr>
  <td class="footer" role="contentinfo">
    <table role="presentation" border="0" cellpadding="0" cellspacing="0" width="100%">
      <tr>
        <td class="legal" align="center" role="paragraph">
          <a href="https://cosettepharma.com/terms-of-use/">Terms &amp; Conditions</a>
          <span class="link-divider" aria-hidden="true">|</span>
          <a href="https://cosettepharma.com/privacy-policy/">Privacy Policy</a>
          <span class="link-divider" aria-hidden="true">|</span>
          <a href="{{build.unsubscribeLink}}">Unsubscribe</a>
        </td>
      </tr>
    </table>
  </td>
</tr>
```

**Note:** `aria-hidden="true"` on the dividers prevents screen readers from announcing "vertical bar" between each link.

---

## What NOT to Use

### Avoid aria-labelledby

**Don't use:** `aria-labelledby`

**Why:** It creates relationships between elements that can break if IDs aren't unique or if content is dynamically generated. It adds unnecessary complexity to email HTML.

**Instead:** Use descriptive text content directly in elements.

---

### Avoid Conflicting Attributes

Never mix alt text with aria-label on the same image. Screen readers will read both or choose one unpredictably.

**Bad:**
```html
<img src="cat.jpg" alt="cute cat napping" aria-label="sleepy dog" width="300" height="200"/>
```

**Good (choose one):**
```html
<!-- Option 1: Standard image -->
<img src="cat.jpg" alt="cute cat napping" width="300" height="200"/>

<!-- Option 2: Background image container -->
<td role="img" aria-label="cute cat napping" style="background-image: url('cat.jpg'); width: 300px; height: 200px;"></td>
```

---

## Accessibility Checklist

Use this checklist before finalizing emails:

- [ ] All `.h1`, `.h2`, `.h3`, `.h4`, `.h5` elements have `role="heading"` and appropriate `aria-level`
- [ ] All `.p` elements have `role="paragraph"`
- [ ] All list tables have `role="list"` and list items have `role="listitem"`
- [ ] Main content area has `role="article"`
- [ ] Footer has `role="contentinfo"`
- [ ] All meaningful images have descriptive alt text
- [ ] All decorative images have `alt=""`
- [ ] Background images with meaning use `role="img"` and `aria-label`
- [ ] No conflicting alt text and aria-label on same element
- [ ] Decorative characters (bullets, dividers) have `aria-hidden="true"`
- [ ] Dark mode CSS preserves readability
- [ ] Links are visible in both light and dark mode
- [ ] Brand colors are preserved in dark mode

---

## Additional Resources

### Section 508 Guidelines
[https://www.section508.gov/create/email-messages/](https://www.section508.gov/create/email-messages/)

### ARIA in Email
[https://www.goodemailcode.com/email-accessibility/aria-landmarks-in-html-email.html](https://www.goodemailcode.com/email-accessibility/aria-landmarks-in-html-email.html)

### ARIA Roles Reference
[https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles)

### ARIA Authoring Practices
[https://www.w3.org/WAI/ARIA/apg/practices/read-me-first/](https://www.w3.org/WAI/ARIA/apg/practices/read-me-first/)

### Dark Mode for Email
[https://www.litmus.com/blog/the-ultimate-guide-to-dark-mode-for-email-marketers](https://www.litmus.com/blog/the-ultimate-guide-to-dark-mode-for-email-marketers)

---

## Getting Started

### Quick Wins
Start with these easy implementations:

1. **Add heading roles** - Takes 30 seconds per email, huge accessibility impact
2. **Fix decorative images** - Use `alt=""` for spacers and borders
3. **Add footer landmark** - One `role="contentinfo"` on your footer `<td>`

### Medium Effort
Once comfortable with basics:

1. **Add paragraph roles** - More time-consuming but improves document structure
2. **Implement list roles** - Requires updating your list table pattern
3. **Add dark mode CSS** - Test thoroughly across clients

### Advanced
For comprehensive accessibility:

1. **Audit all alt text** - Review client-provided alt text for quality
2. **Add article landmarks** - Structure main content areas
3. **Implement comprehensive dark mode** - Fine-tune all colors and elements

---

## Questions?

If you have questions about implementing ARIA or dark mode, consult:
- W3C ARIA documentation (linked above)
- Litmus email accessibility resources
- Email on Acid accessibility testing tools

Remember: Any accessibility improvement is better than none. Start small and iterate.
