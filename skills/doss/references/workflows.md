# Doss Workflows

This document describes common business processes and how they map to actions in Doss.

## The Order Lifecycle

An order flows through these statuses (visible in the **Overview Status** field and as saved views):

```
Pending Ack → ON HOLD / Kitting Needed → Wait to Ship Next Month → Sent For Quotes → Processing → Shipped/Picked Up → Delivered → Closed
```

Not every order goes through every status. The typical path depends on the fulfillment warehouse and shipment mode.

### Status Definitions

| Status | Meaning |
|---|---|
| **Pending Ack** | Order received, awaiting acknowledgment |
| **ON HOLD** | Order paused (e.g., under MOQ, awaiting approval) |
| **Kitting Needed** | Order requires kitting/assembly before shipping |
| **Wait to Ship Next Month** | Approved but scheduled for future shipment |
| **Pending Submission** | Ready to submit to fulfillment but not yet sent |
| **Sent For Quotes** | Freight quotes have been requested |
| **Processing** | Actively being picked/packed at the warehouse |
| **Shipped/Picked Up** | Left the warehouse |
| **Delivered** | Confirmed delivered to customer |
| **Closed** | Fully completed (shipped, invoiced, done) |

### Where to Check Order Status

1. **Quick overview**: Home page → Status Overview Hub shows orders filtered by current status
2. **All orders**: Orders table → use the saved view matching the status you want
3. **Specific order**: Search by PO number (Cmd+K or table search icon)

---

## Freight Quoting Workflow

When an order needs freight, it gets a record in the **Freight Quoting Center**.

```
Order created → Quote requested (checkbox + ship date filled) → Quote accepted → Order moves to Processing
```

### Key Fields to Check

- **Quote Status**: "requested" means waiting for a quote, "accepted" means quote received and approved
- **Requested Ship Date**: Must be filled before requesting quotes
- **Request Quotes ?**: Checkbox indicating the freight form was submitted

### Finding Orders Needing Quotes

Go to Freight Quoting Center → use the **"Sent for Quotes"** view to see orders with active quote requests, or **"Pending Submission"** for orders that still need to be submitted.

---

## Shipment Tracking

Each order can have one or more shipments. Shipments track the physical movement of goods.

### Shipment Statuses

| Status | Meaning |
|---|---|
| **Processing** | Being prepared at the warehouse |
| **Shipped / Picked Up** | In transit or picked up by carrier |
| **Delivered** | Arrived at destination |
| **Received** | Customer confirmed receipt |
| **Closed** | Shipment fully closed out |

### Finding Shipment Information

1. **From an order**: Open the order record → click "Shipments" tab in the right panel
2. **From the Shipments table**: Navigate to Shipments → filter by status view
3. **By tracking number**: Use global search (Cmd+K) or the Shipments table search

### Key Shipment Fields

- **Internal Ref #** — Warehouse reference (format varies: "LAX1-106376", "SAT1-106289", "MEZ-14-26-FLON")
- **Tracking Link** — External URL for carrier tracking (Estes Express, ShipBob, etc.)
- **Ship Date** — When the shipment left the warehouse
- **Issue Date** — When the shipment record was created

---

## Invoicing Workflow

After an order ships, it needs to be invoiced.

```
Order shipped → Invoice record created (status: "Ready to Invoice") → Invoice generated and sent (status: "Invoiced")
```

### Invoice Views

| View | What it shows |
|---|---|
| **Ready to Invoice [SPS]** | Orders fulfilled via SPS-connected retailers (UNFI, etc.) needing invoicing |
| **Ready to Invoice [Non-S...]** | Non-SPS orders needing invoicing |
| **Invoiced** | All completed invoices |
| **Invoiced this Month** | Invoices generated in the current month |
| **UNFI Invoices** | Invoices specifically for UNFI orders |

### Finding Invoice Status for an Order

Search by PO number in the Invoices table, or check the **Invoice Status** field: "Ready to Invoice" means it still needs to be invoiced, "Invoiced" means complete.

---

## Customer and Account Management

Doss uses a two-level customer hierarchy:

### Parent Customers → Customers (Locations)

- **Parent Customer** = the company (e.g., "Imperfect Foods", "Jack's Stir Brew Coffee")
- **Customer** = a specific ship-to location (e.g., "Imperfect Foods CHI", "Jack's Stir Brew Coffee - 200 Park Ave")

### Customer Types

The Customers table isn't just for sales customers. It also stores:
- **Sales Customer** — Ship-to locations for orders
- **Warehouse** — Fulfillment centers (D2, ShipBob, etc.)
- **Freight Broker** — Logistics partners
- **Supplier** — Vendors/suppliers

Use the saved views to filter by type: "Active Sales Customers", "Warehouses", "Freight Brokers", "Suppliers".

### Looking Up a Customer

1. **By name**: Customers table → search for the location name
2. **By parent**: Parent Customers table → find the parent account → check "Linked Locations"
3. **From an order**: Open an order record → look at "Customer Name" field → click it to navigate to the customer

### Payment Terms

Payment terms are set at the **Parent Customer** level:
- **Default Payment Terms** — e.g., "NET30"
- **Payment Terms Days** — Numeric (e.g., 30)

---

## Sample Orders

Sample orders are handled separately from regular orders and are typically fulfilled through **ShipBob**.

### Sample Workflow

```
Sample created → Approve Order (checkbox) → Processing → Shipped → Delivered
```

### Sample Views

Monthly views (June 2025 through February 2026) let you see samples by time period. Status views (On Hold, Processing, Ready to Submit Order, Shipped, Delivered, Delivery Issues) show current state.

---

## Product Catalog Lookups

### SKU Naming Convention

Products follow this naming pattern: `Bar-{flavor}-{pack}ea/{case}cs`

| Code | Product |
|---|---|
| MV | Mixed Variety |
| PB | Peanut Butter |
| PC | Peanut Chocolate |
| HC | Hot Chocolate |
| AB | Almond Butter |
| MB | Maple Blueberry |
| HZ | Hazelnut |

Pack sizes: 4ea (4-pack), 12ea (12-pack)
Case sizes: 6cs (6 per case), 8cs (8 per case)

Example: `Bar-MV-12ea/8cs` = Mixed Variety, 12-pack bars, 8 packs per case (master case).

### Product Types

Views in the Product Catalog help filter by type:
- **Master Cases (12 Pack)** — Standard 12-bar retail packs
- **Master Cases (4 Pack)** — Smaller 4-bar packs
- **Caddies** — Display units
- **Pallets** — Pallet-level SKUs
- **Sample Products (SWAG...)** — Sample/promotional items

### Finding UPC/GTIN Codes

Each product has barcodes for different retail partners:
- **[SPS UNFI/Costco] Product Label** — Label code for UNFI and Costco
- **[SPS SuperValue] UPC/GTIN** — UPC barcode
- **[SPS KeHE/Target/Costco] ConsumerPackage** — Consumer-facing barcode

---

## Common Lookup Patterns

### "What's the status of PO {number}?"

1. Press Cmd+K or go to Orders
2. Search for the PO number
3. Check the **Overview Status** field
4. For shipping details, click the "Shipments" tab on the order record

### "Which orders ship next month?"

Go to Orders → click **"Wait to Ship Next Month"** view.

### "What orders are on hold?"

Go to Orders → click **"ON HOLD"** view. Or check the Home dashboard which shows the Status Overview Hub filtered to "UNDER MOQ (ON HOLD)".

### "How much did we invoice this month?"

Go to Invoices → click **"Invoiced this Month"** view → look at the Invoice Amount ($) column.

### "What are the active products?"

Go to Product Catalog → click **"Active"** view.

### "Who are our warehouse partners?"

Go to Customers → click **"Warehouses"** view.

### "What samples went out recently?"

Go to Samples → click the most recent month view (e.g., "February 2026") or use "Shipped" view.
