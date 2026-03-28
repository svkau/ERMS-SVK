# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

ERMS-SVK is an XML Schema Definition (XSD) project defining Svenska kyrkans (Church of Sweden) extensions to the ERMS (Electronic Records Management System) standard. The schema extends ERMS v3 from DILCIS (https://citserms.dilcis.eu/schema/ERMS_v3.xsd).

The schema is **not** standalone — it must always be used together with a profile schema that defines a specific ERMS adaptation.

## Repository Structure

- `ERMS-SVK-element.xsd` — the single schema file defining all SVK-specific element extensions
- Target namespace: `https://earkiv.svenskakyrkan.se/schemas/ERMS-SVK-element/v2`
- Current version: 2.0

## Schema Architecture

The XSD imports `erms:` namespace elements (agent, appendix, dates) from DILCIS ERMS v3 and defines these top-level SVK extension elements:

| Element | Purpose |
|---|---|
| `initiative` | Whether a matter was initiated externally or internally (`externt`/`eget`) |
| `relatedObjects` | Links to related projects or real estate objects |
| `svkNotes` | Notes with typed categories (arkivanteckning, intern anteckning, etc.) |
| `contractInfo` | Contract metadata including values, currency, agreement type |
| `auditLogEvents` | Audit trail of user actions with timestamps |
| `workflows` | Approval workflows with recipients and statuses |
| `boardMeetingInformation` | Board/committee meeting handling records |
| `svkAppendix` | Extended appendix type wrapping ERMS `appendix` with file versioning and signature info |

The shared `agentsType` complex type wraps one or more `erms:agent` elements and is reused across svkNotes, workflows, and workflowRecipients.

## Validation

To validate the XSD itself against the XML Schema specification, use `xmllint`:
```bash
xmllint --schema http://www.w3.org/2001/XMLSchema.xsd ERMS-SVK-element.xsd
```

To validate an XML instance against this schema (combined with a profile schema), use:
```bash
xmllint --schema <profile-schema.xsd> <instance.xml> --noout
```
