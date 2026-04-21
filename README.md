# SAP PP – Production Order Management Simulator

> A faithful browser-based replica of the SAP ECC / R3 GUI for SAP Production Planning (PP), covering the complete Production Order lifecycle from creation to technical completion.

---

## Overview

This simulator is a single self-contained HTML file that replicates the look, feel, and workflow of the SAP ECC graphical user interface. It is designed for students and trainees studying the **SAP PP (Production Planning)** module, allowing them to practise key transactions without access to a live SAP system.

The simulator faithfully reproduces:
- The SAP title bar, menu bar, toolbar, and status bar
- Yellow input fields, monospace fonts, and blue section headers
- Tab strips, modal dialogs (F4 search help, save confirmations, TECO warnings)
- Real-time status badges (`CRTD`, `REL`, `CONF`, `TECO`)
- A live stock overview that updates as goods movements are posted

---

## How to Run

No installation, server, or internet connection is required.

1. Download the file: `SAP_PP_Production_Order_Simulator.html`
2. Open it in any modern web browser (Chrome, Firefox, Edge, Safari)
3. The simulator loads instantly — all logic runs locally in the browser

---

## Transaction Codes Covered

| TCode | Full Name | Purpose |
|-------|-----------|---------|
| **CO01** | Create Production Order | Manually create and release a production order |
| **MD16** | Display Planned Orders | Convert MRP-generated planned orders into production orders |
| **CO02** | Change Production Order | Modify order quantity, dates, and apply TECO |
| **CO15** | Confirm Production Order | Post yield quantity and trigger automatic goods movements |
| **COOIS** | Production Order List | View all active orders and their statuses |
| **MB52** | Warehouse Stock Overview | Monitor real-time stock levels for all materials |

---

## Navigation

You can navigate between transactions in three ways:

- **Easy Access Screen** — Click a transaction card (CO01, MD16, CO02, CO15) or use the left-hand navigation tree
- **Menu Bar** — Use the `Logistics` menu to jump to any screen
- **TCode Field** — Type a transaction code into the toolbar field (e.g. `CO01`) and press Enter or click ▶

---

## Full Workflow Guide

Follow these steps in order to simulate a complete production order lifecycle.

---

### Step 1 — Create a Production Order (CO01)

1. From the home screen, click the **CO01** card or navigate via `Logistics → CO01`
2. In the **Initial Screen**, enter:
   - **Material**: click  to open the F4 material search and select a finished good (e.g. `E1AH205/S5R16QTS`)
   - **Production Plant**: `SM01` (pre-filled)
   - **Order Type**: `PP01` (default)
3. Click **✔ Enter** — the system copies BOM and Routing data into the order header
4. On the **Order Header** screen:
   - Set **Total Qty** (e.g. `100`)
   - The system auto-fills start/end dates and scheduling type (`Current date`)
5. Click ** Release Order** to authorize shop floor execution — status changes to `REL`
6. Click ** Avail. Check** to verify component stock availability
7. Click ** Save** — the system assigns an order number (e.g. `1000001`) and displays a confirmation

---

### Step 2 — Convert Planned Orders (MD16)

> Use this step to convert MRP-generated planned orders instead of creating manually via CO01.

1. Navigate to **MD16**
2. Select **MRP Controller** as the filter type and click **✔ Enter**
3. Enter filter parameters:
   - **Plant**: `SM01`
   - **MRP Controller**: `010` (SFG EWH)
   - **End Selection Date**: desired cutoff date
4. The system lists all open planned orders
5. Select one or more rows using the checkboxes
6. Click **🔄 Convert to Production Order** — selected planned orders are converted and removed from MRP

---

### Step 3 — Modify a Production Order (CO02)

1. Navigate to **CO02**
2. Enter the **Production Order** number (or click 🔍 to search from the order list)
3. Click **✔ Enter** to load the order
4. On the **General** tab, update the **Total Qty** or other fields as needed
5. Click ** Save** — the system confirms the update with a success message

---

### Step 4 — Confirm a Production Order (CO15)

1. Navigate to **CO15**
2. Enter the **Production Order** number and click **✔ Enter**
3. On the **Confirmation Entry** screen:
   - Enter **Yield** quantity (actual quantity produced)
   - Select **Confirmation Type**: `Partial Confirmation` or `Final Confirmation`
   - Check the **Posting Date**
4. Click ** Goods Movements** to preview all automatic postings:
   - **Movement Type 101** (GR) — finished goods added to unrestricted stock
   - **Movement Type 261** (GI) — raw material components consumed from stock
5. Click ** Save** — all goods movements post simultaneously and stock levels update in real time

---

### Step 5 — Technical Completion / TECO (CO02)

> Apply TECO to formally close the order after all production is complete.

1. Navigate to **CO02** and load the relevant order
2. From the menu bar: `Functions → Restrict Processing →  Complete Technically`
   - Alternatively, click the ** TECO** button in the sub-toolbar
3. A confirmation dialog appears listing the consequences of TECO:
   - No further goods movements can be posted
   - Order is removed from all MRP runs
   - Component reservations are deleted
   - Controlling can perform cost variance analysis
4. Click **✔ Complete Technically** to confirm — order status changes to `TECO`
5. Click ** Save**

---

## Order Status Lifecycle

```
CRTD  →  REL  →  CNF (partial)  →  CNF (final)  →  TECO
```

| Status | Meaning |
|--------|---------|
| `CRTD` | Created — order saved but not yet released |
| `REL` | Released — authorized for shop floor execution |
| `CONF` | Partially confirmed — some yield quantity posted |
| `TECO` | Technically Complete — order closed, excluded from MRP |

---

## Materials Available in the Simulator

| Material Code | Description | Unit | Type |
|---------------|-------------|------|------|
| `E1AH205/S5R16QTS` | 205/55 R16 91H QUATRAC 5 | EA | FERT (Finished Good) |
| `3605202` | BLU R 50 H Tyre | PZ | FERT (Finished Good) |
| `025000220100` | Semi-Finished Goods A | NR | HALB (Semi-Finished) |
| `035002090001` | Raw Material Component X | NR | ROH (Raw Material) |
| `FG-CTRL-001` | Control Panel Board | EA | FERT (Finished Good) |

Each finished good has associated BOM components (raw materials) that are automatically consumed during CO15 confirmation via Movement Type 261.

---

## Additional Screens

### COOIS — Production Order List
- Access via `Logistics → COOIS` or the home screen card
- Displays all created orders with order number, material, quantity, delivered quantity, status, and creation date
- Includes quick-action buttons to jump directly into CO02 or CO15 for any listed order

### MB52 — Warehouse Stock Overview
- Access via `Logistics → MB52` or the home screen card
- Shows real-time unrestricted stock for all materials at Plant SM01
- Stock cards update dynamically every time a CO15 confirmation is saved
- Cards display in red when stock is zero or critically low

---

## Key SAP PP Concepts Demonstrated

| Concept | Where Demonstrated |
|---------|-------------------|
| BOM auto-copy on order creation | CO01 – Order Header screen |
| MRP Planned Order conversion | MD16 – Convert workflow |
| Material Availability Check | CO01 / CO02 – Avail. Check button |
| Simultaneous GR + GI posting | CO15 – Goods Movements screen |
| Backflushing logic | CO15 – automatic component issue |
| TECO consequences | CO02 – TECO confirmation dialog |
| Real-time stock update | MB52 – after CO15 save |
| Order status transitions | All screens – status badge |

---

## Technical Details

| Property | Value |
|----------|-------|
| File type | Single HTML file (self-contained) |
| Dependencies | None — no external libraries, no CDN calls |
| Internet required | No |
| Data persistence | In-memory only (resets on page refresh) |
| Browser support | Chrome 90+, Firefox 88+, Edge 90+, Safari 14+ |
| Screen resolution | Best at 1280×800 or higher |
| SAP style reference | SAP ECC 6.0 / R3 GUI (Enjoy theme) |

---

## Project Information

| Field | Details |
|-------|---------|
| Module | SAP Production Planning (SAP PP) |
| Topic | Production Order Management |
| Transactions | CO01, MD16, CO02, CO15, COOIS, MB52 |
| Plant | SM01 – AVBV Enschede Mfg. |
| Client | 100 |

---

## Glossary

| Term | Definition |
|------|-----------|
| **BOM** | Bill of Materials — list of components needed to manufacture a product |
| **Routing** | Sequence of operations and work centers for production |
| **MRP** | Material Requirements Planning — automatic demand/supply planning engine |
| **Planned Order** | MRP-generated demand signal, must be converted before execution |
| **Production Order** | Firm manufacturing instruction with BOM, routing, and cost data |
| **GR** | Goods Receipt — stock increase (Movement Type 101) |
| **GI** | Goods Issue — stock reduction (Movement Type 261) |
| **TECO** | Technical Completion — final closure status of a production order |
| **Backflushing** | Automatic goods issue of components during confirmation |
| **FERT** | SAP material type for Finished Goods |
| **HALB** | SAP material type for Semi-Finished Goods |
| **ROH** | SAP material type for Raw Materials |
