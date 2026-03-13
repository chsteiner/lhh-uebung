# Requirements – MoMA Collection Research Interface

A static single-page web application (`index.html`) for exploring the MoMA Artworks.csv dataset. Built with vanilla JavaScript and CSS — no frameworks, no build step, no backend.

## Constraints

- **Single file**: all HTML, CSS, and JS live in one `index.html`
- **Data loading**: reads `Artworks.csv` from the same directory at runtime (fetch + client-side parsing)
- **No dependencies**: no npm, no CDN libraries — pure vanilla JS and CSS
- **Performance target**: usable on 160k rows. Paginated table and debounced inputs to stay responsive
- **Static hosting**: works when served from any HTTP static file server (e.g. `python -m http.server`, VS Code Live Server)

## User stories

### US-1: Load and parse the dataset

- CSV is fetched and parsed client-side, handling quoted fields and the UTF-8 BOM
- A loading indicator is shown during parsing
- Total row count is displayed after loading

### US-2: Browse artworks in a paginated table

- Table shows: Title, Artist, Date, Medium, Classification, Nationality, DateAcquired
- Pagination with configurable page size (25 / 50 / 100)
- Click a column header to sort ascending/descending
- Current page, total pages, and total matching rows displayed
- Title links to the MoMA URL if available; plain text otherwise

### US-3: Full-text search

- Single search input, searches across Title, Artist, and Medium
- Debounced (300ms) to avoid lag on 160k rows
- Result count updates in real time
- Clearing the search restores the full dataset

### US-4: Filter by facets

- Dropdown filters for: Classification, Department, Nationality (top 30 + "Other"), Gender (Male / Female / Mixed / Unknown)
- Filters are combinable (AND logic)
- An "All" option resets each filter
- Parsing and display rules for Nationality/Gender: see data handling table below

### US-5: Filter by date range

- Two number inputs: "From year" and "To year"
- Parses the free-form Date field by extracting the first 4-digit year found
- Works in combination with facet filters and search
- Artworks with no parseable date are excluded when a date range is active

### US-6: Artwork detail view

- Click a row to open a modal showing all non-empty fields for that artwork
- Links to the MoMA collection page if URL exists
- Closable via button or Escape key

### US-7: Summary statistics dashboard — deferred to v2

- Planned: total count, top 5 nationalities/classifications/mediums (CSS-only bar charts), acquisition timeline by decade (1929–2029)

## Data handling notes

| Issue | Strategy |
|---|---|
| BeginDate/EndDate = 0 | Treat as null, display as "Unknown" |
| Free-form Date field | Extract first 4-digit number as the sortable/filterable year |
| Multi-artist Nationality/Gender | Split on `) (`; strip parentheses for display. Filter uses contains-match so `(male) (female)` matches both Male and Female |
| Gender normalization | 4 filter options: Male (contains `male`), Female (contains `female`), Mixed (contains both), Unknown (empty) |
| 22k unique Medium strings | Show raw values in table and detail view; no normalization in v1 |
| Missing URLs/ImageURLs | Title as plain text when URL absent; image thumbnail in detail view hidden when ImageURL absent or request fails |
| Columns <1% fill | Circumference, Length, Weight, Seat Height excluded from UI entirely; Duration (sec.) shown in detail view only |
