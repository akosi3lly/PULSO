# PULSO Community Intelligence

A Phase 2 functional prototype for an offline-first community information and coordination layer for Marinduque, Philippines.

## Run

```bash
pnpm install
pnpm dev
```

The Vite app listens on port `3000` and serves the responsive Preview.

## Prototype behavior

The app is mock-data driven and intentionally does not issue official warnings, certify hazards, dispatch emergency services, or replace PAGASA, PHIVOLCS, GeoRiskPH, NDRRMC, or LGU systems. Reports are persisted in `localStorage` for the first prototype: toggle the connectivity control to **Offline**, complete the report form, save a queued report, toggle back online, and use **Sync now**. Switch the role preview to **Official** to open the operations queue, verify or assign the report, add an internal note, and prepare a situation-report draft.

The map uses the accurate Marinduque silhouette as a vector anchor, while facility and incident records are illustrative demo data. The list view is the low-bandwidth fallback.

## Routes

`/`, `/map`, `/report`, `/preparedness`, `/updates`, `/operations`, and `/situation-report` are declared in `public/manus-routes.json`.
