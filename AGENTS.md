# AdornZ Landscaping — Agent Guidelines

## Project Overview

This is a **single-page, static website** for AdornZ Landscaping, a premium landscaping business targeting villa owners in Kerala. Built with **vanilla HTML + CSS only** (no frameworks, no build tools). The design follows a **modern minimal, earthy natural theme**.

---

## CSS Best Practices

### 1. Design System & Variables

All design tokens live in `:root` inside `styles.css`. **Always use CSS custom properties** — never hard-code raw color/font/spacing values.

```css
/* ✅ Correct */
color: var(--deep-green);
font-family: var(--font-heading);
border-radius: var(--radius);
transition: all var(--transition);

/* ❌ Wrong */
color: #1f3d2b;
font-family: 'Playfair Display', serif;
border-radius: 12px;
transition: all 0.35s ease;
```

**Existing tokens to use:**

| Token                | Value                               | Usage                          |
|----------------------|-------------------------------------|--------------------------------|
| `--deep-green`       | `#1f3d2b`                           | Primary brand color, headings  |
| `--sage`             | `#7a9c8a`                           | Accent, labels, muted elements |
| `--beige`            | `#f5f2eb`                           | Main background                |
| `--dark-grey`        | `#222`                              | Body text                      |
| `--white`            | `#ffffff`                           | White surfaces                 |
| `--off-white`        | `#faf9f6`                           | Alternate section background   |
| `--sage-light`       | `#d4e2da`                           | Tags, light accents            |
| `--sage-muted`       | `#a7bfb2`                           | Subtle accents                 |
| `--border-light`     | `rgba(31, 61, 43, 0.1)`             | Card/section borders           |
| `--font-heading`     | `'Playfair Display', Georgia, …`    | Headings, brand text           |
| `--font-body`        | `'Inter', -apple-system, …`         | Body copy, labels, buttons     |
| `--container`        | `1200px`                            | Max-width for `.container`     |
| `--section-pad`      | `100px 0`                           | Standard section padding       |
| `--radius`           | `12px`                              | Cards, images                  |
| `--radius-sm`        | `8px`                               | Smaller elements               |
| `--transition`       | `0.35s cubic-bezier(…)`             | All transitions/animations     |

> **When adding new tokens**, add them to `:root` with a descriptive `--kebab-case` name and document what they're for.

---

### 2. File Organization & Section Structure

The stylesheet is organized into clearly labeled sections using block comment headers. **Always maintain this structure** when adding new sections:

```css
/* ========================================
   SECTION NAME (ALL CAPS)
   ======================================== */
```

Minor sub-groups within a section use lighter comments:

```css
/* ---------- Sub-group Name ---------- */
```

**Current section order:**
1. Google Fonts import
2. CSS Variables (`:root`)
3. Reset
4. Utility classes (`.container`, `.section-label`, `.section-title`, `.section-subtitle`)
5. Button styles (`.btn`, `.btn-primary`, `.btn-secondary`, `.btn-outline`, `.btn-whatsapp`)
6. Navigation
7. Hero Section
8. About Section
9. Services Section
10. Projects Section
11. Why Us Section
12. CTA Section
13. Footer
14. Floating WhatsApp Button
15. Responsive — Tablet (`@media max-width: 1024px`)
16. Responsive — Mobile (`@media max-width: 768px`)
17. Responsive — Small phones (`@media max-width: 480px`)

> **New sections** should be added **before** the responsive media queries and **after** the last content section.

---

### 3. Naming Conventions

Follow a **BEM-lite** pattern: section → element naming with hyphens.

```css
/* Section-level class */
.services { }

/* Child elements use section prefix */
.services-header { }
.services-grid { }
.service-card { }
.service-icon { }

/* Element states or modifiers */
.project-item.large { }
.navbar.scrolled { }
.nav-links.active { }
```

**Rules:**
- Use **lowercase, hyphen-separated** class names (no camelCase, no underscores).
- Prefix child classes with the **singular or plural section name** (e.g., `.footer-col`, `.why-us-card`).
- Use standard HTML tags (`h1`–`h4`, `p`, `a`, `img`) scoped under a class for element-level styling.
- **Never use IDs** for styling. IDs are only for JavaScript hooks and anchor targets.

#### Native CSS Nesting
Native CSS nesting is **encouraged** for better grouping of related styles and improved readability. While the existing codebase uses flat selectors, new components or sections should leverage nesting for cleaner structure.

```css
/* ✅ Encouraged for new development */
.service-card {
  padding: 40px;
  
  & h3 {
    margin-bottom: 10px;
  }
  
  &:hover {
    transform: translateY(-6px);
    
    &::before {
      transform: scaleX(1);
    }
  }
  
  & .service-icon {
    background: var(--sage-light);
  }
}
```

---

### 4. Responsive Design

This project uses a **desktop-first** approach with three breakpoints:

| Breakpoint    | Max-width  | Target              |
|---------------|------------|----------------------|
| Tablet        | `1024px`   | iPads, small laptops |
| Mobile        | `768px`    | Phones (landscape)   |
| Small phones  | `480px`    | Small phones         |

**Rules:**
- Write base styles for desktop; override in the media queries.
- Keep all responsive overrides **inside the existing media query blocks** at the bottom of the file — don't scatter them throughout.
- Use `flex-direction: column` for mobile stacking (the established pattern).
- Use `clamp()` for fluid typography where appropriate (see `.section-title`, `.hero-content h1`).

```css
/* ✅ Fluid type — preferred */
font-size: clamp(2rem, 4vw, 2.75rem);

/* ✅ Mobile override — in the correct media query block */
@media (max-width: 768px) {
  .new-section .container {
    flex-direction: column;
  }
}
```

---

### 5. Layout Patterns

The site uses **Flexbox** consistently (no CSS Grid currently). Maintain this:

```css
/* Standard section layout: side-by-side with gap */
.section .container {
  display: flex;
  align-items: center;
  gap: 80px; /* desktop */
}

/* Card grids */
.cards-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 24px;
  justify-content: center;
}

/* Centered header */
.section-header {
  text-align: center;
  margin-bottom: 60px;
}
```

- Always use `.container` (90% width, `max-width: var(--container)`, centered) to wrap section content.
- Cards use `flex: 1 1 <basis>` with `max-width` for sizing.

---

### 6. Typography

**Two font families only:**
- **Playfair Display** (`--font-heading`) → All headings (`h1`–`h4`), brand name, card titles.
- **Inter** (`--font-body`) → Body text, buttons, labels, navigation, meta text.

**Hierarchy:**

| Element            | Font family       | Size pattern                  | Weight |
|--------------------|-------------------|-------------------------------|--------|
| Hero `h1`          | `--font-heading`  | `clamp(2.5rem, 6vw, 4rem)`   | 600    |
| Section `h2`       | `--font-heading`  | `clamp(2rem, 4vw, 2.75rem)`  | 600    |
| Card `h3`          | `--font-heading`  | `1.1rem`–`1.25rem`           | 600    |
| Section label      | `--font-body`     | `0.75rem`, uppercase, spaced | 600    |
| Body text          | `--font-body`     | `0.88rem`–`1.05rem`          | 400    |
| Small/meta text    | `--font-body`     | `0.72rem`–`0.85rem`          | 500    |

> Always set `line-height` explicitly. Body text uses `1.7`–`1.85`, headings use `1.15`–`1.25`.

---

### 7. Color Usage

Colors alternate between sections to create visual rhythm:

| Section     | Background        | Text color     |
|-------------|-------------------|----------------|
| Hero        | `--deep-green`    | `--white`      |
| About       | `--beige`         | `--dark-grey`  |
| Services    | `--off-white`     | `--dark-grey`  |
| Projects    | `--beige`         | `--dark-grey`  |
| Why Us      | `--deep-green`    | `--white`      |
| CTA         | `--beige`         | `--dark-grey`  |
| Footer      | `--deep-green`    | `white @ 70%`  |

- For white text on dark backgrounds, use `rgba(255, 255, 255, <opacity>)` for hierarchy (0.85 for primary, 0.6–0.65 for secondary, 0.4–0.55 for muted).
- Muted body text on light backgrounds uses `#555` or `#777`.
- **Never introduce new brand colors** without updating `:root`.

---

### 8. Transitions & Animations

- Use the shared `--transition` variable (`0.35s cubic-bezier(0.25, 0.46, 0.45, 0.94)`) for all hover/state transitions.
- Common hover patterns:
  - **Cards**: `transform: translateY(-4px to -6px)` + `box-shadow` increase.
  - **Buttons**: `translateY(-2px)` + `box-shadow`.
  - **Images**: `transform: scale(1.03–1.06)` with `overflow: hidden` on the container.
  - **Links**: underline via `::after` pseudo-element width animation.
- Keep animations subtle and purposeful — no flashy/distracting effects.
- Use `@keyframes` only for looping animations (e.g., `scrollPulse`, `whatsappPulse`).

---

### 9. Buttons

Extend the existing `.btn` base class with modifier classes:

| Class            | Style                          | Use case               |
|------------------|--------------------------------|------------------------|
| `.btn-primary`   | Deep green bg, white text      | Main CTAs              |
| `.btn-secondary` | Transparent, white border      | Hero secondary action  |
| `.btn-outline`   | Transparent, deep green border | Light bg secondary CTA |
| `.btn-whatsapp`  | `#25D366` green bg             | WhatsApp actions       |

- All buttons are **pill-shaped** (`border-radius: 50px`).
- All buttons use `inline-flex` with `align-items: center` and `gap: 8px` for icon + text.
- **Never create one-off button styles** — add a new modifier class to `.btn` if needed.

---

### 10. Images

- All images use `max-width: 100%; height: auto; display: block` (set globally in the reset).
- Sized images (`object-fit: cover`) specify an explicit `height` (e.g., `360px`, `480px`) and reduce it in responsive breakpoints.
- Wrap images in a container with `overflow: hidden; border-radius: var(--radius)` for rounded corners and zoom effects.
- Always include meaningful `alt` text for accessibility.

---

### 11. Spacing Conventions

- Sections use `padding: var(--section-pad)` (`100px 0` desktop, `64px 0` mobile).
- Section headers have `margin-bottom: 60px`.
- Card internal padding: `40px 32px` (desktop), reduce on mobile.
- Standard gaps: `80px` (large sections), `40px` (grids), `24px` (cards), `16px` (buttons/inline).
- Use consistent spacing — don't introduce arbitrary margin/padding values.

---

### 12. Avoid These Anti-Patterns

| ❌ Don't                                    | ✅ Do                                          |
|---------------------------------------------|------------------------------------------------|
| Use `!important` (except mobile nav hacks)  | Write more specific selectors                  |
| Hard-code colors/fonts                       | Use CSS variables from `:root`                 |
| Use CSS Grid (to keep consistency)           | Use Flexbox for all layouts                    |
| Nest media queries inside selectors          | Keep all responsive styles at the bottom        |
| Create inline styles in HTML                 | Use classes in the stylesheet                  |
| Use pixel values for fluid typography        | Use `clamp()` for responsive font sizes        |
| Add vendor prefixes manually                 | Only add for known needs (e.g., `-webkit-backdrop-filter`) |
| Use generic class names like `.card`, `.box` | Prefix with section name: `.service-card`      |

---

### 13. Accessibility

- Maintain sufficient color contrast (the current palette passes WCAG AA).
- Use `aria-label` on icon-only interactive elements (see hamburger, WhatsApp float).
- Ensure all interactive elements have visible focus states (add `:focus-visible` if creating new interactive elements).
- Keep `scroll-behavior: smooth` on `html`.
- Use semantic elements (`<nav>`, `<section>`, `<footer>`, `<main>`) in HTML alongside CSS styling.

---

### 14. Adding New Sections

When adding a new section to the page, follow this checklist:

1. **HTML**: Use a `<section>` with an `id` and class name. Wrap contents in `.container`.
2. **CSS**: Add section styles between the last content section and the responsive block.
3. **Section headers**: Reuse `.section-label`, `.section-title`, `.section-subtitle` utility classes.
4. **Background**: Alternate between `--beige` and `--off-white` (or `--deep-green` for dark sections).
5. **Responsive**: Add mobile overrides inside the existing `@media (max-width: 768px)` block.
6. **Navigation**: Add a nav link in the `<nav>` if the section is anchor-linked.

---

## Technology Stack

- **HTML5** — semantic, accessible markup
- **Vanilla CSS** — no preprocessors, no Tailwind, no frameworks
- **Vanilla JS** — minimal, inline `<script>` for nav scroll/toggle only
- **Google Fonts** — Playfair Display + Inter (imported via `@import` in CSS)
- **No build tools** — open `index.html` directly or serve via a local HTTP server
