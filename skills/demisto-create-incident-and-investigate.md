---
name: Create an incident and add investigation entries
description: Open a new security incident in Demisto (Cortex XSOAR), search for it, and post war-room entries and evidence during the investigation.
api: openapi/demisto-openapi-original.json
operations:
- createIncident
- searchIncidents
- investigationAddEntryHandler
- saveEvidence
---

# Create an incident and investigate (Demisto / Cortex XSOAR)

Use this skill to open a security incident and drive its investigation through the Demisto REST API.

## Authentication
Send your API key in the `Authorization` header on every request. On XSOAR 8+/XSIAM also send `x-xdr-auth-id` (API-Key-ID) and `X-XSRF-TOKEN`. See `authentication/demisto-authentication.yml`. The base URL is your XSOAR server / tenant.

## Steps
1. **Create the incident** — `POST /incident` (`createIncident`) with a `CreateIncidentRequest` body (name, type, severity, labels, custom fields). The response returns the created incident including its `id` and `investigationId`.
2. **Find it again** — `POST /incidents/search` (`searchIncidents`) with a filter object; use `page`/`size` for pagination and read `total` from the response (see `conventions/demisto-conventions.yml`).
3. **Add a war-room entry** — `POST /entry` (`investigationAddEntryHandler`) referencing the `investigationId` to post notes, command output, or context into the investigation timeline.
4. **Record evidence** — `POST /evidence` (`saveEvidence`) to attach an entry to the incident's evidence board for later reporting.

## Rules
- Writes are not idempotent — do not blindly retry `createIncident`; search first to avoid duplicates.
- Errors return a JSON body with an HTTP status (see `errors/demisto-problem-types.yml`); `412` is the generic precondition failure.
