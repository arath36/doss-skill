# Doss Navigation and UI Guide

This document explains how to navigate the Doss app and interact with its UI components using browser automation tools.

## Getting Connected

1. Get browser tab context: `mcp__Claude_in_Chrome__tabs_context_mcp` (with `createIfEmpty: true`)
2. Navigate to the app: `mcp__Claude_in_Chrome__navigate` to `https://app.doss.com`
3. Take a screenshot to verify you're logged in: `mcp__Claude_in_Chrome__computer` with `action: screenshot`

If you see a login page instead of the app, ask the user to log in first.

## URL Structure

All URLs are under `https://app.doss.com/`:

| Pattern | Example | What it shows |
|---|---|---|
| `/` | `app.doss.com/` | Home page (Status Overview Hub dashboard) |
| `/table/{uuid}?tab=tableTabs&view={uuid}` | Long UUID-based URL | A table with a specific saved view |
| `/record/{uuid}` | `app.doss.com/record/809f4688-...` | A single record's detail page |
| `/forms` | `app.doss.com/forms` | Forms section |
| `/dossbot/chat` | `app.doss.com/dossbot/chat` | Dossbot AI chat |
| `/dossbot/chat/new` | `app.doss.com/dossbot/chat/new` | New Dossbot conversation |

Table and view UUIDs are not predictable — navigate via the sidebar instead of constructing URLs directly.

## Page Layout

### Sidebar (Left, ~240px wide)

The sidebar is always visible and contains:

**Top section (utilities):**
- **Home** — Top-left, returns to dashboard
- **Sidebar collapse button** — Icon next to Home
- **Search** (Cmd+K) — Opens global search
- **Notifications** — Bell icon with unread badge count
- **Dossbot** — AI chat assistant

**Main navigation (tables):**
- Status Overview Hub
- Orders
- Freight Quoting Center
- Shipments
- Invoices
- Customers
- Samples
- Product Catalog
- Parent Customers

**Bottom section:**
- Forms
- User avatar and name (e.g., "Joy Grendual")
- Help icon (?)

Click any sidebar item to navigate to that section. The active section is highlighted.

### Header Bar (Top)

When viewing a table:
- **Table name** — Left side (e.g., "Orders")
- **Views** tab — Next to table name
- **Activity** button — Top-right, opens comment panel
- **View Exports** button — Top-right (on some pages)

When viewing a record:
- **Record identifier** — Center (e.g., "50228549")
- **Activity** button — Top-right
- **Three-dot menu** — Top-right

### Main Content Area

**Table view** has three regions:
1. **View selector** (top) — Dropdown showing current view name, plus "+" to create new view
2. **Toolbar** (right of view selector) — Sort, Filter, Search (magnifying glass), and three-dot menu
3. **Table grid** — Column headers and rows of data

**Record detail view** has two panels:
1. **Left panel** — Field values displayed vertically (field name above, value below)
2. **Right panel** — Subtable tabs (e.g., "Order Line Items", "Shipments")

## Interacting with Tables

### Switching Views

Each table has a **Views panel** on the left side of the table area. The available views are listed vertically. Click a view name to switch to it.

The current view is also shown in a dropdown at the top of the table. You can click this dropdown to switch views.

### Using Sort and Filter

- **Sort** — Click "Sort" or the sort icon (↑↓) in the toolbar. Adds/modifies sort criteria.
- **Filter** — Click "Filter" in the toolbar. Shows active filters and allows adding new ones.
- **Search** — Click the magnifying glass icon in the toolbar to search within the current table view.

### Selecting Records

- Click a row's **checkbox** (leftmost column) to select it
- Click the **expand arrow** (">") in a row to expand inline subtables
- Click the **primary field value** (e.g., the PO number) to open the full record detail page

### Pagination

Tables show pagination info at the bottom-right: "{count} records" and "X-Y of Z" with prev/next arrows.

## Interacting with Records

### Opening a Record

From a table view, click on the primary identifier cell (the main text in the first data column, NOT the checkbox or expand arrow). This navigates to `/record/{uuid}`.

### Record Detail Layout

**Left panel fields** are displayed as:
```
Field Label
[Field Value]
```

Field types you'll encounter:
- **Text fields** — Editable text box
- **Date fields** — Date with calendar icon
- **Dropdown/Select** — Value with chevron (▼)
- **Checkbox** — Checkbox with label
- **Formula fields** — Value with "f" icon (read-only, calculated)
- **Linked records** — Value with magnifying glass icon (🔍), clickable to navigate to linked record
- **Lookup fields** — Value with magnifying glass icon, pulled from a linked record

**Right panel subtables** are displayed as tabs. Click a tab name to switch between subtables (e.g., "Order Line Items" vs "Shipments").

### Navigating Back

- Click the table name in the sidebar to return to the table view
- Use the browser back button
- Click "Home" in the sidebar to return to the dashboard

## Global Search (Cmd+K)

Press Cmd+K or click "Search" in the sidebar to open global search. Type a query to search across all records in all tables. Results show the record type and matching field values.

## Activity Panel

Click the "Activity" button (top-right, with sparkle icon ✦) to open the activity sidebar. This shows:
- **Comment input** — "Add comment (Cmd+Enter)" at the top
- **Comment thread** — Previous comments with author, timestamp, and reply threads
- **Reply input** — At the bottom of each thread

The Activity panel appears as a right sidebar overlay on the current page.

## Notifications

Click the bell icon with badge count in the sidebar to view notifications. The badge shows unread count (e.g., "77").

## Tips for Browser Automation

- Use `left_click` on sidebar items to navigate between sections
- Use `screenshot` after each navigation to verify the page loaded
- Table data may still be loading after navigation — look for "Loading..." text or empty rows
- The Views panel on the left side of tables is separate from the main sidebar
- When scrolling in record detail, the left panel (fields) scrolls independently from the right panel (subtables)
- Use `scroll` with coordinates inside the left panel to see more fields on a record
- The "Save" button (bottom-right, Cmd+Enter) appears on record detail pages — avoid clicking it to prevent accidental edits
- "+ Add Record" appears at the bottom of tables — avoid clicking it
