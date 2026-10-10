# PULSO Functional Web Prototype — Implementation Plan

## Scope and outcome

Build a responsive Phase 2 clickable prototype for PULSO, an offline-first community information and coordination layer for Marinduque. The prototype will demonstrate the credible end-to-end chain **Observe → Report → Store Offline → Synchronize → Verify → Visualize → Respond** using mock data and browser persistence. It will not claim to issue official warnings, certify hazards, dispatch emergency services, or replace PAGASA, PHIVOLCS, GeoRiskPH, NDRRMC, or LGU systems.

The first release is client-side and mock-data driven so the core workflow can be tested with intended users before server integrations, authentication, live agency feeds, or a production spatial database are introduced.

## Product surfaces

- **Home**: selected municipality/barangay, official advisory, local information freshness, prominent incident-report CTA, quick links, and persistent connectivity/sync state.
- **Report**: accessible incident form with the locally reviewed report categories, approximate location, description, observation time, optional evidence placeholder, urgency, immediate-danger flag, contact method, privacy consent, validation, and an offline-safe receipt.
- **Map**: a non-authoritative island/facility view with a text-based list fallback. Facilities carry type, area, status, verification date, source, and visibility labels. The map is planning context only and is not an evacuation order.
- **Preparedness**: plain-language before/during/after guidance, emergency contacts, and official-source links.
- **Updates**: official advisory feed with issuer, original title/source, issue time, affected area, retrieval time, stale/superseded labels, and a separately labeled PULSO summary.
- **Operations**: official-view prototype with report queue, filters, report detail, verification/assignment controls, internal notes, and a status timeline.
- **Situation report**: draft summary assembled from mock report data for review/export, without implying official submission.

A visible role switcher will let reviewers preview **Resident**, **Reporter**, and **Official** views. Main navigation stays simple: Home, Map, Report, Preparedness, Updates; Official adds Operations and Reports.

## Interaction and data behavior

- Use `localStorage` for the first prototype’s report outbox and preferences, with a small abstraction so IndexedDB can replace it when the pilot requires larger queues or attachments.
- Generate a client-side reference ID for every saved report.
- A prototype connectivity toggle controls `online`/`offline` behavior without depending on the host network. Offline submission stores a report as `Queued offline`, updates the pending count, and shows a receipt. Online submission simulates `Received`.
- **Sync now** transitions queued items to `Received` and then exposes them to the official queue as `Under verification`; include last sync time and failed/retry affordances in the status panel.
- Keep verification status separate from urgency. Preserve a status timeline: Draft, Queued offline, Received, Under verification, Verified, Assigned, Action in progress, Resolved, Closed, Rejected or duplicate.
- Provide client-side filtering by status, urgency, category, and area in Operations. Verification and assignment controls update the local mock record and append an event to the timeline.
- Use source and freshness labels throughout. Mark all seeded records as **Prototype / demo data** and show clear privacy and operational disclaimers.

## Implementation approach

- Use a small Vite + TypeScript + React application with local component state and a thin persistence module. Keep UI modules feature-oriented rather than putting the entire app in one file.
- Use CSS variables and responsive CSS, with no dependency on live map tiles for the first prototype. The map view will use the accurate Marinduque silhouette as a vector illustration, facility markers as illustrative points, and a list view as the reliable fallback.
- Use the provided brand board as the visual reference. Reuse the verified Marinduque outline already available in the workspace; do not generate a false geographic silhouette.
- Create the PWA manifest and a lightweight service worker that caches the app shell and static mock data. Keep the application-owned PWA contract explicit.
- Provide `/manus-routes.json` for `/`, `/map`, `/report`, `/preparedness`, `/updates`, `/operations`, and `/situation-report`.
- Start the Preview listener on the initialized project port, bound to `0.0.0.0`. Add a root-relative API-free app so the preview and eventual static deployment stay portable.

## Design system

### Design movement

**Editorial civic technology**: a restrained, Swiss-influenced information system softened by island warmth and community signal. The product should feel credible enough for local officials and legible enough for a reporter using a low-end Android phone.

### Core principles

1. **Signal over spectacle** — crisis information is calm, direct, and timestamped.
2. **Place before abstraction** — the accurate Marinduque silhouette and local geography remain visible anchors.
3. **Human verification** — states, roles, evidence, and provenance are explicit.
4. **Offline dignity** — the interface explains what is cached, queued, syncing, stale, or confirmed.

### Color philosophy

- `#12324A` deep marine blue is the ownable PULSO color: trust, sea, civic reliability, and readable contrast.
- `#E76658` signal coral is reserved for pulse points, urgent attention, and primary action emphasis.
- `#F6F1E8` soft sand provides warmth and calm, avoiding sterile institutional white.
- `#849B73` muted leaf is used sparingly for livelihood, land, and verified-positive states.
- Additional status colors are subdued and never neon; urgency is not conflated with verification.

### Layout paradigm

Use **anchored information rails** rather than centered dashboard cards: a persistent left identity/navigation rail on desktop, a compact bottom navigation on mobile, and asymmetric content blocks that let the primary signal or report action sit in a clear lane. Long content uses sections and progressive disclosure; operations uses a split queue/detail workspace.

### Signature elements

- Accurate Marinduque silhouette with a single pulse-route line in the brand mark and empty-state illustrations.
- Thin coral signal rule / ring used to mark active attention and provenance without alarmist red.
- Small uppercase metadata rows with source, timestamp, and freshness as first-class information.

### Interaction philosophy

Interactions should make system state understandable: explicit toggles, confirmation receipts, non-destructive status changes, offline queue counts, and progressive disclosure for official details. No hidden auto-submit, no silent merges, and no interaction that implies emergency dispatch.

### Animation

Use restrained motion: 160–220ms fades and short horizontal transitions for route changes, a gentle pulse on the connectivity/sync indicator while syncing, and a single coral signal dot animation for new queued reports. Respect `prefers-reduced-motion`; disable decorative motion while offline or during urgent states.

### Typography system

Use **Manrope** for wordmark-like headings and interface titles, with **DM Sans** for body copy, metadata, and forms. Headings are compact and confident; body text uses 1.5–1.65 line height; metadata is uppercase with controlled letter spacing. Avoid all-caps paragraphs.

### Brand essence

**Positioning:** The trusted local information pulse that helps Marinduque communities turn observations into coordinated action.

**Personality:** Grounded, vigilant, humane.

### Brand voice

Headlines and CTAs are plain-language, calm, and action-oriented. Avoid generic “Welcome” copy.

- Example headline: “See what needs attention in your area.”
- Example CTA: “Record an observation — even without signal.”

### Wordmark and logo

Use the verified Marinduque silhouette with the pulse-route and a custom-feeling uppercase PULSO lockup. Keep the mark compact, legible, and meaningful at favicon scale; do not use a generic heart or map-pin. The mobile mark uses silhouette + pulse only.

### Signature brand color

**Deep marine blue `#12324A`** is the signature PULSO color.

## Project structure

```text
pulsoapp/
├── app.config.ts              # project logo metadata
├── package.json               # pinned toolchain and scripts
├── pnpm-workspace.yaml        # reviewed lifecycle build permissions
├── index.html                 # app shell metadata and manifest link
├── public/
│   ├── manifest.webmanifest   # application-owned PWA manifest
│   ├── sw.js                  # app-shell caching service worker
│   ├── manus-routes.json      # route manifest required by Webdev
│   └── icons/                 # compact PULSO mark assets
├── src/
│   ├── main.tsx               # React bootstrap and service-worker registration
│   ├── App.tsx                # routing, shell, role/view state
│   ├── styles.css             # tokens, responsive layout, component styles
│   ├── data/mockData.ts       # advisories, facilities, reports, guides, contacts
│   ├── lib/storage.ts         # local report outbox and settings persistence
│   ├── lib/reportWorkflow.ts  # IDs, status transitions, timeline helpers
│   ├── components/            # BrandMark, Shell, StatusPill, SourceStamp, etc.
│   └── views/                 # Home, Map, Report, Preparedness, Updates, Operations, SituationReport
└── README.md                  # local run and prototype behavior notes
```

## Build and delivery workflow

1. Create the project source, dependency policy, PWA metadata, route manifest, and initial mock-data model.
2. Implement the responsive shell and identity system first, then the resident/reporting flow, then official operations views.
3. Register host-managed TypeScript diagnostics before the first application-code batch and resolve actionable diagnostics as edits land.
4. Start the Vite development server on the configured runtime port and verify `/`, `/manus-routes.json`, and the main flows against the managed Preview.
5. Run typecheck/build checks, inspect the rendered Preview at desktop and mobile sizes, and correct any confirmed issues.
6. Commit the intended project files to the managed canonical `main` branch so the version is checkpointed. Do not publish unless explicitly requested; the delivery is the current Preview URL unless a successful publication is later confirmed.
