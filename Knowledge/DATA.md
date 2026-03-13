# Artworks.csv – MoMA Collection Dataset

Source: [github.com/MuseumofModernArt/collection](https://github.com/MuseumofModernArt/collection)

## Overview

160,269 rows × 30 columns, UTF-8 with BOM, standard quoted CSV.

## Columns

| Group | Column | Type | Fill % | Description |
|---|---|---|---|---|
| Identity | Title | text | 99.98 | Name of the artwork |
| | Date | text | 98.7 | Free-form: year (`1953`), range (`1976-77`), approximate (`c. 1920`), or empty |
| | Medium | text | 94.3 | Materials and technique, e.g. `Gelatin silver print`, `Oil on canvas`. 22,827 unique values — near-duplicates common |
| | Dimensions | text | 94.5 | Physical size in imperial and metric, free-form. May not agree with structured dimension columns |
| | Classification | text | 99.99 | Category: Photograph, Print, Drawing, Design, Architecture, Painting, etc. (1 row missing) |
| | Department | text | 100 | Curatorial department: Drawings & Prints, Architecture & Design, Photography, etc. |
| | ObjectID | int | 100 | Unique ID, range 2–507,396 |
| | AccessionNumber | text | 100 | Unique catalog number |
| Artist | Artist | text | 99.2 | Artist name(s), comma-separated for multi-artist works |
| | ConstituentID | int | 99.2 | Artist ID(s), range 1–141,100 |
| | ArtistBio | text | 95.8 | e.g. `(Austrian, 1841–1918)` |
| | Nationality | text | 99.2 | e.g. `(French)`. Multi-artist: `(American) (French)` — split on `) (` to parse |
| | BeginDate | int | 99.2 | Birth year. `0` = unknown |
| | EndDate | int | 99.2 | Death year. `0` = living or unknown |
| | Gender | text | 99.2 | `(male)`, `(female)`, or repeated for multi-artist works |
| Acquisition | CreditLine | text | 99.2 | How acquired (gift, purchase, etc.) |
| | DateAcquired | date | 96.6 | ISO `YYYY-MM-DD`, range 1929–2026 |
| | Cataloged | text | 100 | `Y` (101,598) / `N` (58,671). `Y` count = URL fill count — cataloged ↔ has MoMA URL |
| | OnView | text | 0.6 | Gallery location if displayed, e.g. `MoMA, Floor 4, 417`. 99.4% empty |
| Links | URL | text | 63.4 | MoMA collection page |
| | ImageURL | text | 58.1 | Artwork image |
| Dimensions | Height (cm) | float | 80.8 | Range 0–9,140 |
| | Width (cm) | float | 80.2 | Range 0–9,144 |
| | Depth (cm) | float | 11.6 | 3D objects |
| | Diameter (cm) | float | 0.9 | Circular objects |
| | Duration (sec.) | float | 1.2 | Film/video/time-based media |
| | Circumference, Length, Weight, Seat Height | float/— | <1 | All <1% filled; Seat Height entirely empty |

## Data quality issues

1. **Sentinel zeros in BeginDate/EndDate**: `0` instead of null for unknown/living artists. Must filter before computing statistics.
2. **Free-form Date field**: years, ranges, approximations, compound entries — not directly parseable. Extract first 4-digit number for a usable year.
3. **Medium near-duplicates**: 22,827 unique strings. `Lithograph` vs. `Lithograph, printed in color` vs. `Lithograph, printed in black`. Needs normalization for grouping.
4. **Multi-artist encoding**: Nationality, Gender, ConstituentID repeat parenthesized values per artist (e.g. `(male) (female)`). Parse by splitting on `) (`.
5. **Dimensions text vs. structured fields**: `Dimensions` column is human-readable; separate numeric columns hold parsed values — but they don't always agree or both exist.

## Notable patterns

- **Photography and prints dominate**: Gelatin silver print (17,056), lithograph (8,762), pencil on paper (7,317) far outnumber oil on canvas (1,069, rank 12). The collection skews heavily toward works on paper and photographic media.
- **American-heavy**: ~52% of nationality mentions are American (83,138), then French (24,684) and German (10,978). Counts are per-mention — multi-artist works count each nationality separately.
- **2,014 artworks (1.3%)** have no date at all.
