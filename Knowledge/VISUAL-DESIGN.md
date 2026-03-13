# Visual Design – MoMA Collection Research Interface

## Design direction

Functional data tool with museum-quality typography. The interface should feel like a well-designed catalog index — dense, legible, quietly authoritative. Not decorative. The data is the content.

## Color scheme

All text colors meet WCAG AA contrast on their respective backgrounds.

| Role | Value | Usage |
|---|---|---|
| Background | `#fafafa` | Page background |
| Surface | `#ffffff` | Table rows, modal, filter panel |
| Border | `#e0e0e0` | Table lines, dividers, input borders |
| Text primary | `#1a1a1a` | Body text, table cells |
| Text secondary | `#545454` | Labels, metadata, empty states (7.5:1 on white, 6.9:1 on #fafafa) |
| Accent | `#0057b8` | Links, active sort indicator, focus rings |
| Accent hover | `#003d82` | Link hover, button hover |
| Row hover | `#e8eff7` | Table row hover state (used instead of zebra striping) |
| Filter active | `#e8f0fe` | Selected dropdown highlight |
| Disabled | `#8c8c8c` | Disabled pagination buttons (3.5:1 on white — meets WCAG for UI components) |

Minimal palette. No MoMA red — this is a research tool, not branding.

## Typography

- **Font stack**: `-apple-system, BlinkMacSystemFont, "Segoe UI", Arial, sans-serif` — true system fonts, no loading
- **Table body**: 13px, line-height 1.4
- **Table header**: 12px, uppercase, letter-spacing 0.05em, text-secondary color, font-weight 600
- **Page title**: 20px, font-weight 700
- **Filter labels**: 11px, uppercase, letter-spacing 0.04em
- **Detail modal**: 14px body, 11px field labels

Small type throughout. Research interfaces reward density over whitespace.

## Layout

```
┌──────────────────────────────────────────────────────────┐
│  MoMA Collection Explorer          [search............]  │
├────────────┬─────────────────────────────────────────────┤
│ Filters    │  Showing 160,269 artworks                   │
│            │                                             │
│ Classif. ▼ │  Title       Artist    Date  Medium  ...    │
│ Dept.    ▼ │  ──────────────────────────────────────     │
│ Nation.  ▼ │  row                                        │
│ Gender   ▼ │  row                                        │
│            │  row                                        │
│ Year range │  row                                        │
│ [    ]-[  ]│  row                                        │
│            │  ◀ 1 2 ... 6411 ▶          25 | 50 | 100   │
└────────────┴─────────────────────────────────────────────┘
```

- **Sidebar**: fixed 220px width, left side. Filter dropdowns stacked vertically with labels above each.
- **Main area**: fills remaining width. Result count top-left, table below, pagination bottom.
- **Search bar**: top-right of header, ~300px wide.
- **Header**: single row, 48px height. Title left, search right. No logo.

## Table design

- No zebra striping — rely on horizontal borders and hover for row distinction
- Horizontal lines only (`#e0e0e0`), no vertical borders
- Column headers sticky on scroll
- Sort indicator: `▲` / `▼` appended to active column header
- Title column: left-aligned, max-width 300px, truncated with ellipsis. Link styled as accent color, no underline (underline on hover)
- Date column: right-aligned, 70px fixed width
- Compact row height: 32px

## Pagination

- Layout: `◀ Prev` and `Next ▶` buttons with page numbers between them
- Truncation: show first page, last page, current page, and 1 neighbor on each side. Gaps rendered as `…`. E.g.: `1 … 4 5 6 … 6411`
- Current page: bold text, accent-colored underline
- Disabled prev/next: `#8c8c8c` text, `cursor: default`

## Detail modal

- Overlay: `rgba(0, 0, 0, 0.4)` backdrop
- Modal: white, max-width 640px, centered, 24px padding, 4px border-radius
- **Scroll**: max-height `80vh`, `overflow-y: auto` on the modal body (below the close button)
- Image thumbnail (if available): max-height 300px, top of modal, centered, subtle border
- Fields rendered as a two-column definition list: label left (secondary color, 11px uppercase), value right
- Close button: top-right `×`, 32px hit target, sticky within modal
- MoMA link: accent-colored button at bottom

## Filter panel

- Each filter: label (11px uppercase) + native `<select>` dropdown, full sidebar width
- Year range: two `<input type="number">` side by side with "–" separator
- Spacing: 16px between filter groups
- No custom dropdown styling — native selects for reliability and accessibility

## Interactive states

- Focus rings: 2px `#0057b8` outline on all interactive elements
- Table row hover: background shifts to `#e8eff7`, cursor pointer — consistent on every row (no zebra striping ambiguity)
- Disabled pagination: `#8c8c8c` text, `cursor: default`
- Loading state: centered spinner (CSS-only, border animation) with "Loading artworks..." text below
