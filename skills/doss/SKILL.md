---
name: doss
description: Navigate and read data from the Doss ERP platform (app.doss.com) — an operations cloud for managing orders, shipments, invoices, freight quoting, customers, product catalog, and samples. Use this skill whenever the user asks about Doss, wants to look up orders or shipments, check invoice status, find customer information, review product catalog data, or perform any task involving the Doss web application. Also trigger when the user mentions PO numbers, shipment tracking, freight quotes, fulfillment warehouses, or any supply chain operations that would involve Doss.
---

# Doss ERP Skill

Doss is an Adaptive ERP and Operations Cloud at **app.doss.com**. It manages the full order-to-delivery lifecycle for a consumer packaged goods (CPG) company: receiving orders, quoting freight, shipping product, and invoicing customers.

## Before You Start

You need browser access to use Doss. Use the Claude-in-Chrome MCP tools (`mcp__Claude_in_Chrome__*`). Start by getting tab context, then navigate to `https://app.doss.com`.

**Important**: This is live production data. Do not create, edit, or delete records unless the user explicitly asks you to and confirms the action. Default to read-only operations — viewing tables, filtering views, reading record details, and searching.

## What Doss Contains

Doss has 9 data tables accessible from the left sidebar, plus utility features:

| Section | What it tracks | Record count (approx) |
|---|---|---|
| **Status Overview Hub** | Dashboard combining shipment and order views with filters | — |
| **Orders** | Purchase orders from customers | ~2900 |
| **Freight Quoting Center** | Freight quote requests tied to orders | ~700 |
| **Shipments** | Physical shipments against orders | varies |
| **Invoices** | Invoices generated from shipped orders | ~2600 |
| **Customers** | Ship-to locations (also includes warehouses, freight brokers, suppliers) | ~285 |
| **Samples** | Sample orders (typically via ShipBob) | ~170 |
| **Product Catalog** | SKU master data (bars, cases, caddies, pallets) | ~85 |
| **Parent Customers** | Parent accounts that own multiple customer locations | ~96 |

Additional features:
- **Dossbot** (`/dossbot/chat`) — Built-in AI chat assistant
- **Search** (Cmd+K) — Global search across all tables
- **Notifications** — Bell icon in sidebar with unread count
- **Activity** — Comment/reply panel on record pages (top-right)
- **Forms** (`/forms`) — Form builder (currently empty)

## How to Use This Skill

Read the reference files based on what you need:

- **`references/data-model.md`** — When you need to understand tables, fields, relationships, or find specific data. Read this to know which table to look in and what columns to expect.

- **`references/navigation-and-ui.md`** — When you need to navigate the app, use filters, switch views, open records, or understand the URL structure. Read this for step-by-step browser interaction patterns.

- **`references/workflows.md`** — When the user asks about business processes like "what's the status of this order" or "which orders are ready to invoice." Read this to understand the order lifecycle and common operational tasks.

## Quick Reference: Common Tasks

**Find an order by PO number**: Go to Orders > use Search (magnifying glass icon) or Cmd+K > type the PO number.

**Check shipment status**: Go to Shipments > use the view filter (Processing, Shipped/Picked Up, Delivered, etc.).

**Look up a customer**: Go to Customers > search by location name. Use "Active Sales Customers" view to filter to active ones.

**Check invoice status**: Go to Invoices > use views like "Ready to Invoice [SPS]", "Invoiced", or "Invoiced this Month".

**Find product info**: Go to Product Catalog > search by Vendor Item # (e.g., "Bar-MV-12ea/8cs").
