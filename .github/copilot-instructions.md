# Copilot Instructions for Berlin Savers

## Project Overview
**Berlin Savers** is a single-file, client-side web app showcasing the cheapest verified services in Berlin across 10 categories (gyms, restaurants, mobile plans, etc.). The entire app is embedded in `text-to-html-converter (3).txt` as a self-contained HTML file with inline JavaScript, Tailwind CSS, and hardcoded deal/blog data.

### Architecture at a Glance
- **No backend**: Data is hardcoded in the JavaScript `dealsData` and `blogsData` objects
- **Single-page app (SPA)**: Hash-based routing (`#listing/gyms`, `#detail/gyms/g1`, etc.)
- **Mobile-first responsive design**: Fixed bottom nav for mobile, desktop sidebar for larger screens
- **Styling**: Tailwind CSS with custom color theme (Orange `#FF8C00`, Light Blue `#00BFFF`)

## Critical Patterns & Conventions

### 1. Data Structure
All deals are organized by category in the `dealsData` object. Each deal has:
```javascript
{
  id: 'g1',                    // Unique ID within category
  name: 'Service Name',        // Display name
  price: '€14.90/Month',       // Main selling point
  why: 'Short value prop',     // One-line hook
  logo: 'https://...',         // 60x60 placeholder image
  rating: 4.2,                 // User rating (0-5, supports decimals)
  description: 'Long...',      // Full explanation with "VERIFIED DEAL:" prefix
  websiteLink: 'https://...',  // Official URL (null if N/A)
  mapsQuery: 'Service Name...' // Google Maps search query (null if N/A)
}
```

### 2. Routing Convention
Hash routes determine which view renders:
- `#home` or blank → `renderHome()` (category index)
- `#listing/{category}` → `renderListing(categoryKey)` (deal list)
- `#detail/{category}/{dealId}` → `renderDetail(categoryKey, dealId)` (detail page)
- `#blogs` → `renderBlogs()` (article index)
- `#blog/{blogId}` → `renderBlogDetail(blogId)` (article detail)
- `#featured` → `renderFeaturedDeals()` (highlighted deals)

The `router()` function parses hash changes and delegates to render functions. **Always use hash-based navigation**—never reload the page.

### 3. Adding New Deals
1. Add a new object to the appropriate `dealsData[category]` array
2. Ensure the `id` is unique within that category (e.g., 'g6' for the 6th gym)
3. Use the exact property names shown above (case-sensitive)
4. If adding a new category, create a new key in `dealsData` with an empty array, then add deals

**Example:**
```javascript
gyms: [
  // existing deals...
  { id: 'g6', name: 'New Gym', price: '€13.50/Month', why: '...', /* ... */ }
]
```

### 4. Color Theme & CSS Classes
- Primary CTA color: `primary` (`#FF8C00`, orange) — use for "Visit" buttons, price highlights
- Secondary/accent: `logo-blue` (`#00BFFF`, light blue) — use for active nav, informational buttons
- Tailwind extends: `text-primary`, `bg-primary-dark`, `bg-logo-blue`, `text-star-color` (gold `#FFD700`)
- Hover states: Buttons use `.hover:bg-primary-dark` and `.hover:scale-105` transforms

### 5. Rendering Helpers
- `getTitleFromKey(key)` — Converts 'driving_schools' → 'Driving Schools'
- `renderRating(rating)` — Returns SVG star display with half-star support
- `updateSEO(title, description)` — Updates page title and OG meta tags for sharing
- Deal cards use `.deal-card-hover` class for transform animations on hover

### 6. Key UI Components
- **Sidebar (desktop)**: Updated by `renderSidebar(currentRoute)`, shows categories + featured section
- **Mobile bottom nav**: Fixed buttons with `.nav-btn` class, updated by router
- **Deal cards**: Flex layout with logo, title, description, price, CTA button
- **Text truncation**: `.deal-description` limits to 3 lines with ellipsis (using `-webkit-line-clamp: 3`)

## Development Workflow

### Making Changes
1. **Edit the data**: Modify `dealsData` or `blogsData` objects to add/remove/update entries
2. **Test routing**: Use browser dev tools to navigate via hash and verify no JS errors
3. **Mobile testing**: Check both bottom nav (mobile) and sidebar (desktop) reflect current route
4. **SEO updates**: Call `updateSEO()` in render functions to ensure title/meta tags match content

### Adding New Sections/Views
1. Create a new `render{ViewName}()` function following the naming pattern
2. Call `updateSEO()` and `renderSidebar(currentRoute)` inside it
3. Add a case in the `router()` switch statement
4. Add a navigation link in `renderSidebar()` or mobile nav if needed

### Debugging
- All navigation uses `window.location.hash` — inspect the URL hash to trace routing
- Event listeners use delegation on `#content` element and fixed nav — check for missing `data-route` attributes
- Rating rendering uses SVG — verify `rating` values are numbers between 0-5
- Images use Placeholder.co CDN — ensure logo URLs follow the format `https://placehold.co/60x60/{bg}/{text}?text={label}`

## Common Tasks

### Bulk Update Deal Prices
Update the `price` property in each deal object. The price displays in 3+ places (home grid, listing card, detail view), so ensure consistency.

### Change Color Scheme
1. Update Tailwind config in the `<script>` tag (colors object)
2. Update CSS comments that reference color names for clarity
3. Test all buttons, backgrounds, and text colors for sufficient contrast

### Add New Blog Article
1. Push new object to `blogsData` array with `id`, `title`, `summary`, `date`, `content` (HTML)
2. Ensure `content` includes `<h3>` headers and `<p>` tags for structure
3. The `blog/{id}` route will auto-render if ID exists

### Responsive Design Tweaks
- Mobile: Uses Tailwind responsive prefixes (`md:`, `sm:`, `lg:`) — test at 375px, 768px, 1024px widths
- Sidebar only shows on `md` and up (`hidden md:block`)
- Bottom nav hides on desktop (`md:hidden`)
- Grid adjusts: `grid-cols-1 sm:grid-cols-2 lg:grid-cols-3`

## File Locations & References
- **Single source of truth**: `text-to-html-converter (3).txt` (contains entire app)
- **Data location**: Lines ~350–600 (`const dealsData = {...}`, `const blogsData = [...]`)
- **Render functions**: Lines ~620–1100
- **Router & event listeners**: Lines ~1100–1250
- **Tailwind config**: Lines ~20–50

## Important Notes
- **No build process**: File is served as-is; all CSS/JS is inline
- **No database**: Changes must be made to hardcoded objects; consider migrating to JSON for scaling
- **Open Graph/SEO**: Dynamic meta tags update on navigation; test with Facebook/Twitter link previews
- **External links**: Always use `target="_blank"` for external sites; use hash routes for internal navigation
- **Accessibility**: Star ratings use SVG with accessible color contrast; text uses semantic HTML where possible

