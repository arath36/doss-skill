# Doss Data Model

This document describes every table in Doss, its fields, and how tables relate to each other.

## Entity Relationship Overview

```
Parent Customers (1) ──→ (many) Customers (ship-to locations)
Product Catalog (SKUs) ──→ referenced by Order Line Items
Orders (1) ──→ (many) Order Line Items
Orders (1) ──→ (many) Shipments
Orders (1) ──→ (many) Invoices
Orders (1) ──→ (1) Freight Quoting Center record
Samples ──→ standalone (fulfilled via ShipBob)
```

The central entity is **Orders**. Most other tables link back to an order via PO number or a direct relationship.

---

## Orders

The primary table. Each record is a purchase order from a customer.

**Key fields:**
- **PO** — Purchase order number (e.g., "50228549"). The primary identifier.
- **Status Overview LINK** — Formatted string: `PO {number} / {warehouse} / {customer}` (e.g., "PO 50228549 / D2 / Foodcase International"). Links to the Status Overview Hub.
- **Fulfillment Warehouse** — Dropdown. Values include "D2", "ShipBob", and others. Indicates which warehouse fulfills the order.
- **Total Sales Amount** — Formula field (currency). Calculated from line items.
- **Due Date [From Status Overview]** — Lookup field. The order's due/delivery date.
- **Due Date** — Date field.
- **Case Count** — Formula field (numeric).
- **Order ID** — Text field. Secondary identifier.
- **Order Approved ?** — Checkbox. Whether the order has been approved.
- **Overview Status** — Lookup field. Values: "Wait to Ship Next Month", "ON HOLD", "Kitting Needed", "Pending Ack", "Pending Submission", "Sent For Quotes", "Processing", "Shipped/Picked Up", "Delivered", "Closed".
- **Order Date** — Date field.
- **Order Link** — Text/URL field.
- **Customer Name** — Linked record to Customers table (e.g., "Foodcase International B.V.").
- **Sales Channel** — Text field.
- **Order Total** — Formula field (currency).
- **Discount Code** — Text field.
- **Bar Count** — Formula field (numeric).
- **Price / Bar** — Formula field (currency).
- **Ship To Destination** — Text field (full address).
- **Shipment Mode** — Dropdown. Values include "Pickup".
- **Ship Speed** — Text/dropdown field.

**Subtables (visible in record detail):**
- **Order Line Items** tab — Vendor Item #, Product Catalog Link, Ship Qty, Order Qty, [D2] Product Description Link, [D2] Item Number Link
- **Shipments** tab — Links to related shipment records

**Saved views:**
All Records, Wait to Ship Next Month, ON HOLD, Kitting Needed, Pending Ack, Pending Submission for..., Sent For Quotes, Processing, Shipped/Picked Up, Delivered, Closed, Open Orders, Filtered View (DO NOT T...), Frosted Strawberry Ord..., View (15)

---

## Freight Quoting Center

Tracks freight quote requests. Each record corresponds to an order that needs a freight quote.

**Key fields:**
- **Status Overview Link** — Formatted PO string (same pattern as Orders).
- **Quote Status** — Text/tag field. Values: "requested", "accepted".
- **Requested Ship Date ** [Complete before Request]** — Date field.
- **Request Quotes ? (Did you fill out Request)** — Checkbox.

**Saved views:**
All Records, Wait to Ship Next Month, Pending Submission for..., Sent for Quotes, Processing, Shipped / Picked Up, Analysis View

---

## Shipments

Tracks physical shipments. Multiple shipments can exist per order.

**Key fields:**
- **Status Overview Link** — Formatted PO string.
- **Internal Ref #** — Text field (e.g., "LAX1-106376", "SAT1-106289"). Warehouse-specific reference.
- **Ship Date** — Date field.
- **Linked PO** — Linked record to Orders (by PO number).
- **Order ID** — Numeric field.
- **Order URL** — URL field (some link to external systems like web.shipbob.com).
- **Overview Status** — Text field. Values: "Processing", "Shipped / Picked Up", "Delivered", "Received", "Closed".
- **Issue Date** — Date field.
- **Due Date** — Date field.
- **Shipping Status** — Text field.
- **Tracking Link** — URL field.
- **Tracking #** — Text field.
- **# of Shipments** — Numeric field.

**Saved views:**
All Records, Processing, Shipped / Picked Up, Delivered, Received, Closed, All Records Copy

---

## Invoices

Tracks invoices generated from shipped orders.

**Key fields:**
- **Status Overview Link** — Formatted PO string.
- **Invoice #** — Text field (e.g., "10001779287", "MEZ-14-26-FLON").
- **Invoice / BOL Date** — Date field.
- **Linked PO** — Text field (PO number).
- **Invoice Amount ($)** — Currency field.
- **Invoice Status** — Text field. Values: "Invoiced", "Ready to Invoice".
- **Overview Status** — Text field. Values: "Delivered", "Shipped / Picked Up", "Closed".

**Saved views:**
All Records, Ready to Invoice [SPS], Ready to Invoice [Non-S...], Invoiced, Invoiced this Month, UNFI Invoices

---

## Customers

Ship-to locations. This table contains not just sales customers but also warehouses, freight brokers, and suppliers — differentiated by the "Location Type" field.

**Key fields:**
- **Location Name** — Text field. The specific location name (e.g., "Imperfect Foods CHI", "Jack's Stir Brew Coffee - 200 Park").
- **Parent Account** — Linked record to Parent Customers (e.g., "Imperfect Foods", "Jack's Stir Brew Coffee").
- **Location Type** — Text field. Values: "Sales Customer", "Warehouse", "Freight Broker", "Supplier".
- **Status** — Text field. Values: "Active", "Inactive".
- **Shipping Address** — Text field (full address).
- **Street Address** — Text field.
- **OPTIONAL Street Address** — Text field (suite, unit, etc.).

**Saved views:**
All Records, Active Sales Customers, Warehouses, Freight Brokers, Suppliers

---

## Samples

Tracks sample orders, typically fulfilled through ShipBob.

**Key fields:**
- **PO / Order ID / DC** — Text field (e.g., "SAMPLE-589 / 302343535 / Attn. Rodrigo Zuloaga").
- **Approve Order** — Checkbox.
- **Fulfillment Warehouse** — Text field. Typically "ShipBob".
- **Order ID** — Numeric field.
- **Ship Date Complete for D2 Orders** — Date field.
- **Shipping Speed Class** — Text field. Values: "Expedited (2-4 days)", "Standard (Ground)".

**Saved views:**
All Samples Orders, On Hold, Processing, Ready to Submit Order, Shipped, Delivered, Delivery Issues, plus monthly views (June 2025 through February 2026)

---

## Product Catalog

The SKU master table. Contains all products and their identifiers across different retail partners.

**Key fields:**
- **Vendor Item #** — Text field. The primary SKU identifier (e.g., "Bar-MV-12ea/8cs", "Bar-PB-4ea/6cs").
- **Status** — Text field. Values: "Active", "Inactive".
- **Type** — Text field. Values: "Master" (and likely others).
- **[SPS UNFI/Costco] Product Label** — Text field. SKU label for UNFI/Costco (e.g., "BAR-MV-12E").
- **[SPS SuperValue] UPC/GTIN** — Text field. UPC code (e.g., "085004658215").
- **[SPS KeHE/Target/Costco] ConsumerPackage** — Text field. Another barcode format.

**Saved views:**
All Records, Active, Caddies, Master Cases (12 Pack), Master Cases (4 Pack), Sample Products (SWAG...), Pallets

**SKU naming convention:** `Bar-{flavor code}-{pack size}/{case size}`. Flavor codes observed: MV (Mixed Variety), PB (Peanut Butter), PC (Peanut Chocolate?), HC (Hot Chocolate), AB, MB (Maple Blueberry), HZ (Hazelnut).

---

## Parent Customers

Parent accounts that own one or more customer locations.

**Key fields:**
- **Linked Account** — Text field. The parent company name (e.g., "5 Squares Inc.", "Amazon", "Imperfect Foods").
- **Type** — Text field. Values: "Wholesale", "Warehouse", "Amazon".
- **Default Payment Terms** — Text field. Values: "NET30".
- **Freight Terms** — Text field.
- **Linked Locations** — Linked records to Customers table.
- **Payment Terms Days** — Formula field (numeric, e.g., 30, 0).

**Saved views:**
All Records

---

## Status Overview Hub (Home Page)

Not a standalone table — it's a dashboard page that embeds filtered views from other tables. The Home page at `app.doss.com/` shows:

1. **Status Overview Hub** — A view of shipments/orders filtered by "UNDER MOQ (ON HOLD)" with columns: PO/Order ID/DC, Delivery Date, Due Date, Overview Status, Linked PO #, Tracking Link, Tracking #, # of Shipments, Fulfillment columns.

2. **Orders** — An embedded view of the Orders table with a dropdown filter for status (e.g., "ON HOLD").

Both sections have their own sort, filter, and search controls.
