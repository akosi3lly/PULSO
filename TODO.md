# PULSO Prototype ToDo

## Responsive PWA shell and identity system
- Implement a responsive PULSO application shell with deep marine blue `#12324A`, signal coral `#E76658`, soft sand `#F6F1E8`, muted leaf `#849B73`, Manrope/DM Sans typography, accurate Marinduque silhouette mark, clear desktop rail, mobile navigation, accessible focus states, PWA metadata, app-shell caching, and `/manus-routes.json` entries for `/`, `/map`, `/report`, `/preparedness`, `/updates`, `/operations`, and `/situation-report`.

## Resident home, advisories, updates, and preparedness
- Home must show selected municipality/barangay, an official advisory with issuer/title/source/issue time/affected area, local information freshness, a prominent Report an incident action, quick links, and persistent online/offline/sync state; if there is no active warning, do not show a misleading all-clear.
- Updates must distinguish official information from a clearly labeled PULSO plain-language summary, preserve source links, show timestamps/retrieval time, and label stale/superseded data.
- Preparedness must provide before/during/after guidance, emergency contacts, reporting guidance, official-source links, and a clear statement that PULSO maps are not evacuation orders.

## Offline incident reporting workflow
- Provide report categories for flooding/rising water, blocked road/bridge damage, landslide, fire, damaged house/facility, power/telecom outage, water/sanitation, medical/welfare, missing/stranded person, agriculture damage, boat/gear/fisheries damage, and other local concern.
- The form must collect category, approximate location, short description, observation time, optional evidence, urgency, immediate-danger state, reporter role/account, optional contact method, and privacy notice/consent; validate required fields and collect only response-needed information.
- When offline, save a report locally with a client-side unique ID, show `Queued offline`, maintain a pending count, allow safe deletion of unsent drafts, and show a receipt; when online, simulate submission as `Received`.
- Provide `Sync now`, last successful sync time, retry/failure state, and idempotent local behavior; synchronization must not create duplicate reports.

## Community map and facility directory
- Provide a map-like view and a text-based list fallback with evacuation centers, health facilities, water points, and demo community reports; each record must show source/date or last verification, operating status, and public/restricted visibility label.
- Label hazard/context information as planning context and never present PULSO data as official hazard assessment or the map as an evacuation order.

## Official operations and situation report
- Provide a role-preview switcher for Resident, Reporter, and Official; Official exposes Operations/Reports views while respecting function and geographic-scope intent in the prototype.
- Operations must provide list/map-like views, filters by date/area/category/status/urgency/verification, report detail, event history, human verification and rejection/duplicate controls, assignment to an office/team, internal notes, related evidence, escalation/closure fields, and an approved-data export affordance.
- Keep verification status separate from urgency and preserve the original report; status labels include Draft, Queued offline, Received, Under verification, Verified, Assigned, Action in progress, Resolved, Closed, Rejected or duplicate.
- Provide a situation-report draft that summarizes mock reports for review/export and does not imply official submission.

## Safety and source boundaries
- Use mock/demo data only; clearly state that PULSO complements and does not replace PAGASA, PHIVOLCS, GeoRiskPH, NDRRMC, or LGU systems, and that submitting a report does not dispatch emergency services.
- Do not integrate live agency feeds or expose protected API tokens in browser code; preserve source, issue/retrieval timestamps, data age, intended use, and limitations.
