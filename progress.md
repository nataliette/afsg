# Progress — AF Gym Equipment Tracker

## Current State
Single-page application (SPA) for discovering gym equipment across Singapore locations. Fully functional with landing page, search, and responsive mobile design.

## Completed Features

### Landing Page
- ✅ Entry point (shows when no URL params)
- ✅ H1 tagline: "Find equipment across Singapore gyms"
- ✅ 2-column layout (desktop): gym list + muscle group tags
- ✅ Stacks to 1 column on mobile
- ✅ Clickable gym names and tags for navigation

### Gym View
- ✅ Equipment table per gym with category, brand, equipment, muscle groups
- ✅ Status badge (complete/partial) top-right of gym name
- ✅ Tag-based filtering (click any tag to search)
- ✅ Mobile table conversion to card grid

### Search & Tags
- ✅ Primary tags (chest, back, shoulders, arms, legs, core, cardio, calisthenics)
- ✅ Secondary tags (quads, hamstrings, biceps, etc.)
- ✅ Search input with autosuggest dropdown on focus
- ✅ Tag suggestions based on input (e.g., "quad" → quads)
- ✅ Autocomplete with 8 results max
- ✅ Visual distinction: primary (dark blue), secondary (light blue)

### Header & Navigation
- ✅ Desktop: Title + search bar in same row, responsive width
- ✅ Mobile: Centered title + hamburger menu (left) + search icon (right)
- ✅ Mobile search expands into absolute-positioned field (no content shift)
- ✅ Search field has clear button (X) and submit button (magnifying glass)
- ✅ Escape key collapses mobile search
- ✅ Click outside search closes suggestions panel

### Mobile Menu
- ✅ Fixed sidebar slide-in animation (snappy easing, 0.25s)
- ✅ Semi-transparent overlay backdrop
- ✅ Gyms list with status dots + initials
- ✅ Close button (×) in header
- ✅ Auto-closes on gym selection

### Styling & Design System
- ✅ CSS extracted to SCSS with design system
- ✅ Variables for colors, spacing (4px scale), typography, transitions
- ✅ Organized into 5 partials: _variables, _base, _components, _layout, _responsive
- ✅ Compiled to external `styles.css`
- ✅ Responsive breakpoint at 768px

### Data & Routing
- ✅ Equipment data from `equipment.json` (20KB)
- ✅ Mock fallback for preview environments
- ✅ URL-based routing: `?gym=xx` and `?tag=xx`
- ✅ Back button support (popstate event)
- ✅ GYM_INITIALS mapping (9 gyms: Cecil Street, Grandtral Complex, Harbourfront HF, Labrador View, New Queensway, Pasir Panjang, Tanjong Pagar, Telok Blangah, Wheelock Place)

## Architecture
- **Single file HTML structure** + external SCSS/CSS
- **Vanilla JavaScript**: Event listeners, DOM manipulation, CSS state toggles
- **No framework dependencies**
- **Client-side only**: No backend, static data
- **Graceful degradation**: Works in preview with mock data

## File Structure
```
afsg/
├── index.html              (main app, ~1,320 lines)
├── equipment.json          (20KB gym + equipment data)
├── styles.css              (compiled output)
├── styles.scss             (main SCSS import)
├── _variables.scss         (design tokens)
├── _base.scss              (element resets)
├── _components.scss        (buttons, tags, chips, etc.)
├── _layout.scss            (header, sidebar, content)
├── _responsive.scss        (@media queries)
├── CLAUDE.md               (this workflow guide)
└── progress.md             (this progress file)
```

## Known Limitations
- No backend; all data is static/client-side
- Equipment data is mock in preview (real data from equipment.json on actual deploy)
- No form submissions or user accounts
- No persistent state (refreshes reset view)

## Next Steps (Future Work)
- Performance audits (bundle size, render optimization)
- Enhanced search (fuzzy matching, multi-tag filters)
- Equipment images/icons per category
- User favorites or bookmarks
- Export/share equipment lists
