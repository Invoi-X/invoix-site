# Invoix.Xyz — Product Whitepaper

**Version 1.0.0 | February 2026**

---

## Table of Contents

1. [Executive Summary](#executive-summary)
2. [Problem Statement](#problem-statement)
3. [Product Overview](#product-overview)
4. [Core Features](#core-features)
   - [Invoice Creation](#invoice-creation)
   - [Templates](#templates)
   - [PDF Export](#pdf-export)
   - [Client Management](#client-management)
   - [Dashboard & Layouts](#dashboard--layouts)
   - [Analytics & Charts](#analytics--charts)
   - [Email Integration](#email-integration)
   - [Recurring Invoices](#recurring-invoices)
   - [Keyboard Shortcuts](#keyboard-shortcuts)
5. [Customisation](#customisation)
   - [Themes & Appearance](#themes--appearance)
   - [Branding](#branding)
   - [Invoice Numbering](#invoice-numbering)
   - [Regional Settings](#regional-settings)
6. [Currencies & Tax](#currencies--tax)
7. [Data & Privacy](#data--privacy)
   - [Local-First Architecture](#local-first-architecture)
   - [Backup & Restore](#backup--restore)
   - [Storage](#storage)
8. [Technical Architecture](#technical-architecture)
   - [Technology Stack](#technology-stack)
   - [Project Structure](#project-structure)
   - [State Management](#state-management)
   - [Electron Desktop App](#electron-desktop-app)
9. [Onboarding Experience](#onboarding-experience)
10. [Accessibility & Code Quality](#accessibility--code-quality)
11. [Pricing Model](#pricing-model)
12. [Roadmap](#roadmap)

---

## Executive Summary

Invoix.Xyz is a professional invoice generator built for freelancers and small businesses who want beautiful, functional invoices without the complexity of enterprise software or the limitations of free tools. It runs as a native Windows desktop application — fully offline, fully private, with zero recurring costs after purchase.

The app ships with 6 professionally designed invoice templates, 7 currency options, a full analytics dashboard with 5 chart types, Gmail and Outlook email integration, and a deeply customisable theme system with 4 colour palettes and light/dark modes.

Invoix.Xyz is not a SaaS product. It's software you own. Your data stays on your machine. There are no accounts to create, no servers to depend on, and no monthly fees.

---

## Problem Statement

Freelancers and small business owners face a recurring friction point: invoicing. The options available today fall into three categories, none of which are ideal:

**1. Enterprise platforms (FreshBooks, QuickBooks, Xero)**
Powerful but expensive. Monthly subscriptions of £15–50+/month add up quickly, and most features go unused by solo operators. Data lives on someone else's servers.

**2. Free tools (Wave, PayPal invoicing)**
Functional but limited. Templates are generic, customisation is minimal, and the free tier often comes with ads, data harvesting, or upsell pressure.

**3. Manual methods (Word, Excel, Google Docs)**
Maximum control, zero automation. No calculations, no status tracking, no analytics, no PDF consistency. Every invoice is built from scratch.

Invoix.Xyz fills the gap: a one-time purchase desktop app that offers professional-grade invoicing with full control over data, branding, and workflow — at a fraction of the cost of subscription software.

---

## Product Overview

| Attribute | Detail |
|---|---|
| **Product** | Invoix.Xyz |
| **Version** | 1.0.0 |
| **Platform** | Windows (desktop application) |
| **Architecture** | Electron + React + TypeScript |
| **Data Storage** | Local filesystem (no cloud dependency) |
| **Pricing** | One-time purchase |
| **Internet Required** | Only for email sending; all other features work offline |

---

## Core Features

### Invoice Creation

The invoice editor is the heart of the application. It provides a structured form with real-time preview, supporting every field a professional invoice needs:

- **Business & client information** — Name, address, email, phone, tax ID. Client details auto-fill from saved clients.
- **Line items** — Unlimited rows. Each item has description, quantity, rate, and a taxable toggle. Supports negative values for credits and refunds.
- **Discount** — Percentage or flat amount, with an optional reason field to explain the discount to the client.
- **Tax** — Configurable rate applied only to items marked as taxable. Tax is calculated after proportional discount allocation.
- **Shipping** — Optional flat fee added after tax.
- **Notes, terms, payment instructions** — Free-text fields included on the final invoice. Payment instructions can be configured per currency.
- **PO / Reference number** — Optional field for client purchase order tracking.
- **Status tracking** — Draft → Sent → Paid, with automatic Overdue detection based on due date.

**Calculation engine:**

```
Subtotal = Σ (quantity × rate) for all line items
Taxable Amount = Σ (quantity × rate) for taxable items only
Discount = percentage of subtotal OR flat amount
Tax = taxRate × (taxable amount after proportional discount)
Total = Subtotal − Discount + Tax + Shipping
```

All values are rounded to 2 decimal places. The engine handles edge cases including negative line items, zero-quantity rows, and 100% discounts.

---

### Templates

Invoix.Xyz ships with 6 invoice templates, each designed for a different aesthetic:

| Template | Style | Character |
|---|---|---|
| **Modern** | Gradient headers, bold accent colours | Contemporary and energetic |
| **Classic** | Table borders, formal structure, dark header row | Traditional and corporate |
| **Minimal** | Generous whitespace, thin lines, light typography | Clean and understated |
| **Professional** | Accent-coloured top bar, structured grid layout | Corporate-ready |
| **Elegant** | Centred headers, refined borders, italic flourishes | Sophisticated and polished |
| **Wild** | Randomised gradients, neon accents, bold geometry | Expressive and unique |

The **Wild** template is unique: it generates a deterministic colour scheme per invoice using a seeded random number generator based on the invoice ID. The same invoice always looks the same, but every invoice looks different. Colours span gradients, borders, header backgrounds, and accent elements — producing invoices that are genuinely one-of-a-kind.

All templates support:
- Company logo display
- Custom accent colour (except Wild, which auto-generates)
- Full line item table with taxable indicators
- Discount, tax, and shipping breakdown
- Notes, terms, and payment instructions
- Optimised A4 layout for PDF export

---

### PDF Export

Invoices export to pixel-perfect A4 PDF files with one click. The PDF is generated client-side using `@react-pdf/renderer` — no server round-trip, no internet required.

- **Format:** A4 (210mm × 297mm)
- **Filename:** `{invoiceNumber}.pdf` (e.g., `INV-20260214-001.pdf`)
- **Contents:** Full invoice with logo, all line items, calculations, notes, and terms
- **Template fidelity:** Each template has dedicated PDF stylesheets that match the on-screen preview exactly

PDFs are generated in-memory and saved via the system file dialog. No temporary files are created.

---

### Client Management

The client management system lets users save and reuse client details across invoices:

- **Create, edit, delete** clients with name, email, phone, company, address, and notes
- **Auto-fill** — Select a saved client when creating an invoice and all fields populate instantly
- **Search** — Filter clients by name, email, or company
- **Grid layout** with quick-action buttons (Edit, Delete, Use in New Invoice)
- **Timestamps** — Created and last updated dates tracked per client

---

### Dashboard & Layouts

The dashboard is the main view after launch. It displays all invoices with filtering, sorting, and search. Five layout options let users choose the view that fits their workflow:

| Layout | Description |
|---|---|
| **Classic** | Card grid with status badges, the default view |
| **Compact Cards** | Denser grid with smaller cards, showing more invoices per screen |
| **Side Panel** | List on the left, invoice preview panel on the right |
| **Minimal** | Clean list view with subtle dividers, no card borders |
| **Kanban** | Column-based board organised by status (Draft, Sent, Paid, Overdue) |

**Dashboard controls:**
- **Search** by invoice number, client name, or amount
- **Status filter** — All, Draft, Sent, Paid, Overdue (multi-select)
- **Sort** by date, amount, client, or invoice number (ascending/descending)
- **Card size** toggle — Default, Compact, Dense
- **Layout switcher** — Quick toggle between all 5 views

The **Kanban layout** supports collapsible columns and reordering of invoices within columns. It provides a visual pipeline of invoice status at a glance.

---

### Analytics & Charts

The analytics panel provides a visual overview of business performance. It sits at the top of the dashboard and adapts to the selected theme and mode.

**Five chart types:**

| Chart | Use Case |
|---|---|
| **Bar** | Revenue per period — clear comparison across months |
| **Line** | Revenue trend over time — spotting growth or decline |
| **Area** | Filled version of line chart — visual impact for presentations |
| **Composed** | Bar (revenue) + line (invoice count) overlay — dual metrics |
| **Scatter** | Dot distribution — identifying outliers and patterns |

**Three timeframes:**
- Daily (last 30 days)
- Weekly (last 12 weeks)
- Monthly (last 12 months)

**Metrics displayed:**
- Total revenue (paid invoices)
- Outstanding amount (sent + draft + overdue)
- Overdue count
- Total invoice count
- Revenue trend with configurable chart type and timeframe

Multi-currency invoices are handled gracefully — the analytics display totals per currency (e.g., "$12,450 / £3,200").

---

### Email Integration

Invoix.Xyz integrates with Gmail and Outlook for one-click invoice delivery:

- **OAuth 2.0 authentication** — Secure connection, no passwords stored locally
- **One-click send** — From the invoice preview page, click Send and the PDF is attached and delivered
- **Customisable email templates** — Edit the subject line and body with 8 template variables:

| Variable | Resolves To |
|---|---|
| `{invoiceNumber}` | e.g., INV-20260214-001 |
| `{clientName}` | Client's name |
| `{companyName}` | Your business name |
| `{total}` | Total amount with currency symbol |
| `{invoiceDate}` | Formatted invoice date |
| `{dueDate}` | Formatted due date |
| `{paymentInstructions}` | Your payment details |
| `{clientEmail}` | Client's email address |

- **Preferred provider** — When both Gmail and Outlook are connected, choose a default
- **Connection status** — Visual indicators show which providers are linked
- **Token management** — Re-authentication prompts if a token expires

Email is the only feature that requires an internet connection. All other functionality works fully offline.

---

### Recurring Invoices

Invoices can be marked as recurring with a configurable interval:

- **Weekly**
- **Bi-weekly**
- **Monthly**
- **Quarterly**
- **Yearly**

When a recurring invoice is due, the app auto-generates a new draft with updated dates. The original invoice is linked as the parent, creating a chain of recurring billing history. An optional end date limits the recurrence window.

---

### Keyboard Shortcuts

Power users can navigate the app without touching the mouse:

| Shortcut | Action | Context |
|---|---|---|
| `Ctrl+N` | Create new invoice | Global |
| `Ctrl+S` | Save invoice and return to dashboard | Invoice editor |
| `Ctrl+P` | Preview current invoice | Invoice editor |
| `Ctrl+/` | Show keyboard shortcuts reference | Global |

Shortcuts are disabled when focus is inside an input field to prevent conflicts. A reference modal (triggered by `Ctrl+/`) lists all available shortcuts with context hints.

---

## Customisation

### Themes & Appearance

Invoix.Xyz offers a comprehensive theming system:

**Light/Dark mode** — A single toggle switches the entire application between light and dark palettes. The mode persists across sessions and applies before the first frame renders (no flash of incorrect theme).

**Four colour themes:**

| Theme | Palette | Character |
|---|---|---|
| **Amethyst** | Purple / indigo gradients | Default — creative and modern |
| **Ocean** | Cyan / teal gradients | Calm and professional |
| **Sunset** | Orange / rose gradients | Warm and energetic |
| **Emerald** | Green / teal gradients | Fresh and natural |

Themes affect every element: navigation, buttons, cards, charts, invoice accent colours, and even the desktop app icon (which changes to match the selected theme).

The theme system is implemented via CSS custom properties applied to the `<html>` element, ensuring instant, flicker-free transitions.

---

### Branding

- **Company logo** — Upload PNG, JPG, or SVG (max 2MB). Displayed in the invoice header and PDF export.
- **Accent colour** — Custom hex colour applied to invoice headers, borders, and highlights. Persists as the default for new invoices.
- **Business details** — Company name, address, email, phone, and tax ID displayed on every invoice.

---

### Invoice Numbering

Two numbering formats:

| Format | Pattern | Example |
|---|---|---|
| **Date-based** | `{PREFIX}-YYYYMMDD-XXXX` | INV-20260214-0041 |
| **Sequential** | `{PREFIX}-{NEXT}` | INV-0041 |

The prefix is configurable (default: "INV"). The next number auto-increments and can be manually adjusted.

---

### Regional Settings

**Date formats:**
- MMM dd, yyyy (Feb 14, 2026)
- MM/dd/yyyy (02/14/2026)
- dd/MM/yyyy (14/02/2026)
- yyyy-MM-dd (2026-02-14)
- dd MMM yyyy (14 Feb 2026)

**Payment terms presets:**
- Due on Receipt (0 days)
- Net 7, 15, 30, 45, 60, 90
- Custom (manual due date)

**Tax rate presets:**
- US Sales Tax (7.12%)
- UK VAT (20%)
- EU VAT (21%)
- Canada GST (5%)
- Australia GST (10%)
- India GST (18%)
- Custom (manual percentage)

**Late fee configuration:**
- None
- Percentage-based (applied to overdue amount)
- Flat amount

---

## Currencies & Tax

### Supported Currencies

| Code | Symbol | Currency |
|---|---|---|
| USD | $ | US Dollar |
| EUR | € | Euro |
| GBP | £ | British Pound |
| CAD | C$ | Canadian Dollar |
| AUD | A$ | Australian Dollar |
| JPY | ¥ | Japanese Yen |
| INR | ₹ | Indian Rupee |

Currency formatting respects regional conventions via `Intl.NumberFormat`. Each currency can have its own payment instructions (e.g., different bank details for USD vs GBP transfers).

### Tax Handling

- Tax is applied only to line items marked as "taxable"
- When a discount is applied, it is proportionally allocated across taxable items before tax calculation
- The tax rate is configurable per invoice (default set in settings)
- Tax presets cover major jurisdictions but any custom rate is supported

---

## Data & Privacy

### Local-First Architecture

Invoix.Xyz is built on a local-first principle. All data — invoices, clients, settings, preferences — is stored on the user's machine. There is no cloud database, no user accounts, no telemetry, and no analytics collection.

This means:
- **No internet required** for core functionality
- **No vendor lock-in** — your data is always accessible
- **No subscription dependency** — the app works forever, even if the company disappears
- **Complete privacy** — financial data never leaves your device

### Backup & Restore

The export/import system provides full data portability:

**Export:**
- Format: Human-readable JSON
- Contents: All invoices, clients, and settings
- Filename: `invoice-backup-YYYY-MM-DD.json`
- Metadata includes app version and export timestamp

**Import:**
- Validates structure and required fields before importing
- Maximum file size: 50MB
- Shows preview of data (invoice count, client count) before confirmation
- Full data replacement (import overwrites existing data)

**Data structure:**
```json
{
  "version": "1.0.0",
  "exportDate": "2026-02-14T12:00:00.000Z",
  "invoices": [...],
  "clients": [...],
  "settings": {...}
}
```

### Storage

- **Location:** `%APPDATA%\Invoix.Xyz\invoix-data.json` (Windows)
- **Format:** JSON (human-readable)
- **Write strategy:** Atomic (temp file + rename) to prevent corruption on crash
- **Persistence:** Zustand state synced to filesystem via custom Electron storage adapter

---

## Technical Architecture

### Technology Stack

| Layer | Technology | Version |
|---|---|---|
| **Runtime** | Electron | 34.2.0 |
| **Framework** | React | 19.2.4 |
| **Language** | TypeScript | 5.9.3 |
| **Build Tool** | Vite | 7.3.1 |
| **Styling** | Tailwind CSS | 3.4.19 |
| **Animations** | Framer Motion | 12.34.0 |
| **State** | Zustand | 5.0.11 |
| **PDF** | @react-pdf/renderer | 4.3.2 |
| **Charts** | Recharts | 3.7.0 |
| **Dates** | date-fns | 4.1.0 |
| **Icons** | Lucide React | 0.563.0 |
| **Notifications** | React Hot Toast | 2.6.0 |
| **Routing** | React Router | 7.13.0 |
| **Packaging** | Electron Builder | 25.1.8 |
| **Auto-Updates** | Electron Updater | 6.7.3 |

### Project Structure

```
src/
  components/
    invoice/         — Form sections (BusinessInfo, ClientInfo, LineItems, etc.)
    preview/         — 6 invoice template renderers
    dashboard/       — Layouts, cards, analytics, Kanban board
    shared/          — Reusable UI (Button, Input, Select, Modal, etc.)
  stores/            — Zustand stores (invoice, client, settings, theme, dashboard, email)
  services/          — Email API integration (Gmail, Outlook OAuth)
  utils/             — Calculations, formatters, PDF export, backup, seed data
  hooks/             — Custom hooks (keyboard shortcuts, calculations, filtered invoices)
  pages/             — Route pages (Dashboard, Editor, Preview, Clients, Settings)
  types/             — TypeScript interfaces (Invoice, Client, LineItem, etc.)
  lib/               — Storage abstraction (Electron IPC vs localStorage)
  App.tsx            — Router setup (HashRouter for Electron compatibility)
  main.tsx           — React entry point

electron/
  main.cjs           — Electron main process (window management, IPC handlers)
  preload.cjs        — Preload script (secure bridge between renderer and main)

public/build/
  icon.ico           — Default app icon
  icons/             — Themed icons (amethyst.ico, ocean.ico, sunset.ico, emerald.ico)
```

### State Management

Zustand stores manage all application state with persistence:

| Store | Manages |
|---|---|
| **Invoice Store** | CRUD operations, invoice list, active invoice |
| **Client Store** | Client list, CRUD operations |
| **Settings Store** | Business info, defaults, preferences |
| **Theme Store** | Light/dark mode, colour theme |
| **Dashboard Store** | Active layout, sort/filter preferences, card size |
| **Email Template Store** | Custom email subject and body |

All stores persist to the Electron filesystem via a custom storage adapter that bridges Zustand's `persist` middleware with Electron's IPC-based file I/O. In development (browser), it falls back to `localStorage`.

### Electron Desktop App

**Window configuration:**
- Default size: 1400 × 900px
- Minimum size: 1024 × 700px
- Custom app icon (changes with selected colour theme)

**IPC handlers:**
| Channel | Purpose |
|---|---|
| `store:get` | Read key from JSON data store |
| `store:set` | Write key to JSON data store |
| `store:delete` | Remove key from data store |
| `store:has` | Check key existence |
| `store:getPath` | Return absolute path to data file |
| `app:setThemeIcon` | Update app icon based on colour theme |

**Build output:**
- Windows NSIS installer (`Invoix.Xyz Setup 1.0.0.exe`)
- User-customisable install directory
- Desktop and Start Menu shortcuts

**Auto-updater:**
- Configured via `electron-updater` with GitHub Releases as the update source
- Checks for updates 3 seconds after launch (when enabled)
- User-controllable toggle in Settings
- Dialog prompts for download and restart

---

## Onboarding Experience

First-time users are greeted with a 5-step onboarding wizard:

| Step | Content |
|---|---|
| **0 — Welcome** | Brand introduction, "Private & local" and "No account needed" badges |
| **1 — Your Business** | Company name, address, email, phone, tax ID |
| **2 — Logo & Branding** | Logo upload + accent colour picker with live preview |
| **3 — Invoice Defaults** | Currency, default tax rate, payment terms |
| **4 — All Set** | Summary of configured items, "Show on startup" toggle |

The wizard features animated transitions (Framer Motion), clickable progress dots, and a skip button on every step. All data persists immediately — there's no risk of losing progress. The wizard respects the user's preference and won't reappear unless requested.

---

## Accessibility & Code Quality

### Accessibility

- All ~68 non-submit buttons have explicit `type="button"` (including the shared Button component)
- All ~25 icon-only buttons have descriptive `aria-label` attributes
- Keyboard navigation support (Tab, Enter, Escape)
- Semantic HTML throughout
- Screen reader compatible

### Code Quality

- **TypeScript strict mode** — No `any` types in production code; all replaced with proper typed interfaces (`CurrencyCode`, `DiscountType`, `InvoiceStatus`, etc.)
- **No console output** — All `console.log`, `console.warn`, and `console.debug` statements removed from production code
- **No dead code** — All TODO/FIXME comments resolved, unused functions and imports removed
- **Clean builds** — Both TypeScript compiler and Vite bundler pass with zero warnings
- **MutationObserver guard** — SSR-safe with `typeof document !== 'undefined'` check
- **Atomic file writes** — Electron storage uses temp file + rename to prevent data corruption

---

## Pricing Model

Invoix.Xyz uses a straightforward pricing structure:

| Tier | Price | Features |
|---|---|---|
| **Free** | £0 forever | 5 invoices/month, all 6 templates, PDF export, Invoix.Xyz watermark |
| **Desktop** | £29.99 one-time | Unlimited invoices, no watermark, email integration, analytics, free updates forever |
| **Cloud Pro** | £4.99/month | Everything in Desktop + cloud storage, cross-device sync, automatic backups, priority support, all future updates |

The Desktop tier is the primary offering — a one-time purchase with no recurring costs. The Cloud Pro tier is planned for users who need cross-device access.

---

## Roadmap

### Delivered (v1.0.0)

- Full invoice CRUD with line items, taxes, discounts, shipping
- 6 invoice templates (Modern, Classic, Minimal, Professional, Elegant, Wild)
- PDF export (A4, pixel-perfect)
- Client management with auto-fill
- Gmail + Outlook email integration
- 5 dashboard layouts including Kanban
- 5 chart types with 3 timeframes
- 4 colour themes + light/dark mode
- Keyboard shortcuts
- Onboarding wizard
- Data export/import backup system
- Logo upload and branding
- Recurring invoice scheduling
- Negative line items (credits/refunds)
- Discount reason field
- Late fee configuration
- Per-currency payment instructions
- Windows desktop app with custom icon

### Planned

- macOS and Linux builds
- Cloud sync (Cloud Pro tier)
- Multi-device access via web app
- Receipt/expense tracking
- Client portal (share invoice link)
- Payment gateway integration (Stripe, PayPal)
- Multi-language support
- Custom template builder
- Batch invoice operations
- Tax report generation

---

## Summary

Invoix.Xyz is professional invoicing software that respects its users. It's fast, private, offline-capable, and beautifully designed. It doesn't ask for a monthly subscription to do something as fundamental as sending an invoice.

For freelancers and small businesses, it offers everything the enterprise tools provide — templates, analytics, email integration, client management — without the complexity, the recurring cost, or the data concerns.

One purchase. Your data. Your invoices. Done.

---

*Invoix.Xyz v1.0.0 — Built in Austin, TX*
*https://invoix.xyz*
