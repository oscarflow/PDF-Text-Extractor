# Email Accessibility & Dark Mode Guide


## ARIA Roles for Semantic Structure

### Why Use ARIA in Email HTML?

We don't use semantic tags so ARIA roles provide another way to communicate document structure to screen readers.

### Heading Roles

Add `role="heading"` and `aria-level` to `<td>` elements that function as headings.

**Example ARIA:**
```html
<td class="h1" role="heading" aria-level="1">
  Header
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

**Example:**
```html
<td class="p" role="paragraph">
  This study examines the effects of the treatment on patients with HSDD.
</td>
```

---

### List Roles

Add `role="list"` to the container table and `role="listitem"` to each item row.

**Example:**
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


#### Background Images with ARIA
When using background images that convey meaning (via CSS or table backgrounds), use a container with `role="img"` and `aria-label`.

```html
<td role="img" aria-label="Doctor consulting with patient" style="background-image: url('consultation.jpg'); width: 600px; height: 400px;">
  <!-- Any text overlay goes here -->
</td>
```

## Dark Mode Support

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


