# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

ops is a GitHub Pages-hosted equipment maintenance management system. It consists of:
- **Admin Panel**: `index.html` — warehouse, workorders, devices, vendors, dashboard
- **Public Query**: `query.html` — public work order status lookup
- **Vendor Portal**: `vendor.html` — vendor login and work order management

## Repository Structure

```
├── index.html       # Admin panel
├── query.html       # Public query interface
├── vendor.html      # Vendor self-service portal
├── devices.js       # Site/org mappings and device data
├── workorders.json  # Sample work order data
└── .gitignore
```

## Development

No build system — files are deployed directly to GitHub Pages. Open `index.html`, `query.html`, or `vendor.html` directly in a browser to preview.

## Architecture

- **Frontend**: Vanilla HTML/CSS/JS with Bootstrap 5, Chart.js 4, and SheetJS (xlsx) for Excel parsing
- **Data Storage**: LocalStorage for persistence; GitHub Gist for cloud sync
- **Modules in index.html**:
  - 工单管理 (Workorders) — CRUD operations
  - 数据看板 (Dashboard) — Chart.js visualizations
  - 运维商 (Vendors) — Maintenance company management
  - 仓储管理 (Warehouse) — Excel import, outbound tracking, storage fee calculation
  - 网点设备 (Devices) — Site device inventory

## Key Data Structures

- `whRecords` array: `{id, name, category, qty, remark, inDate, status, outDate}` — Warehouse records
- `WH_PRICE_MAP`: Storage fee pricing per category (`现金自助设备`: 45, `自助设备`: 20, `大型设备`: 45, `其它设备`: 20)

## Storage Fee Calculation Rule

Equipment stored ≥15 days in a month incurs storage fee:
- Inbound date ≤ 15th → fee starts from current month
- Inbound date > 15th → fee starts from next month
- Outbound date ≤ 15th → no fee from next month
- Outbound date > 15th → fee charged for current month

## GitHub Sync

Uses GitHub Gist (`gist_id: 9ae11536cf7304baed38841a47c39385`) with personal access token stored in localStorage (`GITHUB_TOKEN_VAL`). Push directly to `main` branch — GitHub Pages auto-deploys.
