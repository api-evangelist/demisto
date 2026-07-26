---
name: Manage threat indicators
description: Create, search, and edit threat-intelligence indicators (IOCs) in Demisto (Cortex XSOAR).
api: openapi/demisto-openapi-original.json
operations:
- indicatorsCreate
- indicatorsSearch
- indicatorsEdit
---

# Manage threat indicators (Demisto / Cortex XSOAR)

Use this skill to manage threat-intelligence indicators (IOCs) through the Demisto REST API.

## Authentication
Send your API key in the `Authorization` header (plus `x-xdr-auth-id` / `X-XSRF-TOKEN` on XSOAR 8+). See `authentication/demisto-authentication.yml`.

## Steps
1. **Create an indicator** — `POST /indicator/create` (`indicatorsCreate`) with the indicator value, type (ip, domain, file hash, url), and reputation/score.
2. **Search indicators** — `POST /indicators/search` (`indicatorsSearch`) with a query filter; page through results with `page`/`size` and read `total`.
3. **Edit an indicator** — `POST /indicator/edit` (`indicatorsEdit`) to update reputation, comments, or custom fields on an existing indicator.

## Rules
- Indicator reputation feeds automation/playbooks — set the DBotScore deliberately.
- Search before create to avoid duplicating an existing IOC; the API does not deduplicate for you.
- Errors return an HTTP status with a JSON body (see `errors/demisto-problem-types.yml`).
