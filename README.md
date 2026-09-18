# Codex build handoff: DC Operations CLI

Build the application described below. This is a self-contained implementation brief based on our discussion. Produce actual source files, configuration examples, and documentation. Carry the work through implementation; do not stop after a plan or create only a Markdown document containing pseudocode.

## 1. Product and environment

Build one practical CLI for data-center engineers to inspect equipment, improve documentation, record maintenance, verify connections, and watch alerts for a specific cabinet.

The environment uses dcTrack today and plans to migrate to NetBox. Jira owns change tickets and approval workflows. Another team normally runs Ansible daily and may retain collection results in Git/Stash. Grafana is present, and Nagstamon already connects to the monitoring setup. The actual alert backend and API configuration still need identification. Webhooks are not available initially.

Engineers work in a trading environment. Reuse existing observations before introducing new device access. The tool must not restart services, install agents, change server or switch configuration, reboot equipment, or operate PDU outlets as part of inspection. Logging into a server with read-only credentials is not a guarantee of zero operational impact.

The interface is a CLI. No web dashboard is required. History persists after the CLI closes; cabinet polling runs only while the engineer runs watch. Automated reminders and unattended collection are later features.

## 2. Coding style and implementation approach

Use Python and the readable, disciplined style we discussed in connection with Jane Street: clear types, explicit data flow, small understandable functions, and predictable behavior. This is a readability preference, not a requirement to use OCaml or a claim that an official Jane Street Python style guide exists.

- Prefer ordinary functions, descriptive names, simple control flow, and small modules with clear responsibilities.
- Use type annotations, dataclasses, explicit result states, and explicit optional values where they clarify the domain.
- Validate external input once at the boundary. Keep comparison rules independent of HTTP, CLI formatting, and database operations.
- Use explicit units and timezone-aware timestamps. Avoid ambiguous numbers and booleans whose meaning depends on hidden context.
- Catch specific errors. Explain missing prerequisites. Do not swallow failures or present unknown data as success.
- Keep comments about reasons, assumptions, and limits; avoid comments that repeat the code.
- Avoid clever one-liners, speculative abstractions, generic workflow engines, unnecessary inheritance, sprawling configuration, and duplicated logic.
- Do not introduce microservices, queues, Kubernetes, an agent framework, or a large dependency stack without a demonstrated need.
- Make small coherent changes. Inspect existing files before changing them. Preserve unrelated work.
- Use a short dependency-based implementation plan. Do not repeatedly restart planning or make endless review passes.

Start with Python 3.11+, argparse, and a small set of focused modules. Plain readable output and structured JSON are enough; a terminal UI framework is optional, not a reason to delay the core workflows.

Use SQLite on local disk for a bounded pilot or one serialized service process. Do not share the SQLite file over NFS or treat it as a multi-process team database. An authenticated internal service, such as FastAPI, owns credentials and authorization for team use and live documentation writes. The CLI is its client. Use the same domain functions in local mode and service mode. Introduce PostgreSQL or background jobs only when actual deployment requirements justify them.

Keep source files authoritative. A Markdown source bundle, if included, is a generated companion and must stay consistent with the actual files.

## 3. Ownership and evidence rules

Keep these three kinds of information separate:

1. Expected: approved infrastructure records and intended connections from dcTrack, later NetBox.
2. Observed: what a collector or monitoring source reported at a particular time.
3. Performed: physical work reported by an engineer or established by a suitable device event.

Imports must never silently turn an observed connection into approved documentation. Jira owns approval and workflow; it must not become a second rack and hardware database. The tool owns detailed maintenance history and evidence. Git/Stash holds source, collection artifacts where the team uses it, and explicitly published reports; it is not the operational database.

Use stable internal device/component IDs, external system IDs, and explicit alias mappings. Do not identify a device solely by IP address, hostname, or a loosely matched serial. Detect collisions and ambiguous matches. Preserve history when a device is renamed, moved, or mapped to NetBox later.

Every observation needs its source, source run ID, actual observation time, import time, and per-section collection status. Distinguish complete, failed, and not collected. Preserve source event times separately from tool recording times. Freshness limits must be configurable.

Missing data is unknown. A failed collection is not a removal. An omitted slot is not an empty slot. A daily snapshot can bound a change between observations; it cannot establish the exact physical removal time.

## 4. Inventory, imports, and collectors

Import approved inventory/documentation exports and finalized daily collection artifacts. Define and document a versioned normalized input format. Implement schema validation, stable device mapping, duplicate-run protection, and retention of collection provenance. Conflicting content under the same source/run ID must not silently replace history.

Do not assume the other team's raw Ansible format. Keep its conversion in a small adapter; supply representative example inputs. Support partial runs without inferring missing hardware from failed sections. Record late evidence according to its observation time, not arrival time.

The tool should retain and show these details when evidence exists:

- Devices: manufacturer, model, serial/service tag, owner, site, cabinet, rack position, aliases, and inventory-system ID.
- Optics: physical cage/port, vendor, part number, serial, form factor, media, supported speed, wavelength, and optical readings with units and timestamps.
- NICs: card identity, physical slot, model, part/serial, firmware, physical ports, and available PCI/OS mappings.
- NIC/interfaces: permanent versus operational MAC, interface aliases, configured versus operating speed, VLAN configuration, MTU, bonds/port channels, breakout/lane relationships, and neighbor evidence.
- DIMMs: physical slot, serial, part number, manufacturer, capacity, and rated/configured speed.
- SSDs/HDDs: physical bay, serial, model, capacity, firmware, controller relationship, and available health evidence.
- Other replaceable hardware, such as PSUs, when the source supplies reliable identity and location.

Do not claim every device exposes all fields. Preserve unknowns and conflicting sources. Do not confuse physical disks with RAID volumes or a physical NIC port with a bond, VLAN interface, or virtual function.

Direct collectors are optional later adapters, enabled only when approved and needed. Server OS LLDP may contain detail that iDRAC/Redfish does not expose. Reuse collected OS LLDP where available; do not assume Redfish supplies equivalent topology. If direct access is later enabled, use approved read-only operations, restricted accounts, verified hosts/certificates, timeouts, and bounded concurrency. Never install or enable LLDP during a read.

## 5. Wrong-cable and connection verification

Compare the approved endpoint with observed LLDP neighbors and reliable interface mappings. Use evidence from both ends where available, without claiming that one-sided evidence is bilateral verification.

Resolve the chain from server identity to physical NIC card/port, OS interface, switch identity, and switch port. Handle bonds, port channels, breakouts, aliases, and patching explicitly. LLDP reports a neighbor relationship; it does not necessarily reveal every passive patch-panel segment.

Return clear findings:

- PASS: reliable endpoints agree as of the observation time.
- MISMATCH: reliable evidence disagrees; investigate cabling, mapping, or outdated documentation.
- UNDOCUMENTED: a reliable observed endpoint has no approved baseline.
- NOT VERIFIED: evidence is stale, missing, ambiguous, incomplete, or unsupported.

Do not classify no LLDP neighbor as proof of a wrong cable. Do not automatically recable, change a switch port, or accept the observed connection as correct. Where supplied, show link state, operating speed, errors, MAC/VLAN evidence, and optical measurements alongside the connection result. Use documented requirements or vendor thresholds; do not invent intended VLANs or acceptable optical levels.

## 6. Hardware changes and maintenance history

Support install, remove, replace, reseat, move, clean, inspect/test, RMA/disposition, and amendment records. This must cover optics, NIC cards, DIMMs/RAM, SSDs, and other identified replaceable components.

Record device, cabinet context, physical slot/port, component type, old/new identity, actor, Jira ticket/work session, reason, actual occurrence time, tool recording time, supporting evidence, and removed-part destination where relevant. If identity is unknown, require an explanation rather than a fabricated serial.

Examples the system must support:

- An engineer reseats an optic and records the time and reason even though the serial stays the same.
- An optic is removed, later replaced, and both actions remain in history.
- A Dell server receives a new NIC in a specific PCIe slot, retaining old/new serials and the ticket.
- A DIMM changes serial in the same slot between two inventories.
- An SSD is removed and the source explicitly reports the bay empty afterward.

Different serials in a comparable physical slot are replacement evidence. Identical serials do not prove that no reseat occurred. Exact insert/remove events require a suitable device event or a technician record. Do not promise universal switch optic-event support.

Retain corrections as linked amendments; do not erase the original event. Track moves and disposition without merging components on weak identity evidence. Ambiguous serials remain unresolved. A moved component must not lose its earlier installation history.

## 7. Work sessions and verification

Provide this workflow:

1. Start a device or cabinet work session with a Jira ticket reference.
2. Capture the available baseline and its freshness/completeness.
3. Optionally start a cabinet watch linked to that session.
4. Record actual physical actions.
5. Import or obtain approved post-work evidence.
6. Compare relevant before/after state and record verification results.
7. Prepare and apply separately reviewed documentation updates.
8. Export or explicitly publish the work report.

Keep implementation completion, verification, and documentation completion as separate states. A pre-work snapshot cannot verify post-work hardware. Completing a session with incomplete verification requires an explanation and must retain the incomplete finding. Do not automatically close or approve the Jira ticket.

Allow truthful action logging even when Jira authorization cannot currently be verified. Mark that condition and block external documentation writes until fresh validation succeeds. A ticket check for documentation is not permission to perform physical work outside the organization's maintenance window.

## 8. Documentation audit and controlled writes

Audit missing, stale, conflicting, unsupported, and unverified information. Show evidence, scope, and the concrete fields needing attention. Do not infer documentation completeness merely because a report was exported or one field changed.

Every dcTrack mutation made by this tool requires a valid Jira change ticket. The rule includes field updates, connection updates, component records, notes, and report references. Read-only inspection and unlinked cabinet watch do not need a ticket.

The write workflow must:

1. Build a stored field-level proposal showing old values, proposed values, target IDs, evidence, ticket, author, and destination version.
2. Require explicit review of that exact proposal.
3. Authenticate the actor at the service; never trust a client-supplied actor name or role for production authority.
4. Revalidate the actual Jira workflow, approval, permitted status, actor, asset/cabinet scope, and applicable documentation deadline immediately before writing.
5. Re-read affected destination records and detect changes since the preview.
6. Apply only supported, allowlisted operations using the installed API's verified concurrency mechanism.
7. Read back the result and retain a durable operation record and audit trail.

No ticket, wrong scope, missing approval, stale policy, expired authorization, failed Jira lookup, or unsupported concurrency means no write. Documentation may be allowed after physical implementation under a separately defined deadline; do not invent the company's policy.

Record an operation intent before a mutation. Preserve partial and uncertain outcomes. A timeout or failed read-back after a possible write must not become a confirmed failure or success. Reconcile before retrying; avoid duplicate notes and repeated changes. Explain how an unresolved attempt is investigated and how any new attempt is authorized.

Keep live write credentials in the trusted service. Local file mode is for a pilot, not an authorization boundary. Enforcing the ticket rule across dcTrack also requires appropriate dcTrack permissions; the CLI cannot prevent separate authorized users from bypassing it through another client.

Do not invent dcTrack endpoints, payloads, fields, ETags, approval fields, or NetBox capabilities. Start with supported scalar fields when necessary, but explicitly identify topology/component operations that remain unimplemented. An HTTP PATCH request existing in code is not proof of a working dcTrack integration. Unsupported operations must fail with a useful explanation, not silently disappear from the scope.

## 9. Cabinet watch through existing monitoring

Implement an on-demand watch for a selected cabinet. It polls the same underlying alert source used by the existing Nagstamon/Grafana setup. No webhook requirement. Identify that source before implementing its specific adapter; do not scrape the GUI or assume Nagstamon exposes an API or that a Grafana rule list is a firing-alert feed.

Map exact device identities and relevant host/service/interface labels to the cabinet. Include connected upstream ports when mapped, even if the switch is elsewhere. Show unmapped devices and uncertain coverage. Do not filter only by cabinet text in alert summaries.

Display relevant active, changed, resolved, suppressed, and unknown alert states, severity, affected device/interface, source time when available, observation time, last successful refresh, and monitoring-source health. Retain changes and gaps in session history. An empty response is healthy only when completeness and source health are established. Disappearance from an alert list is not proof of recovery.

Use configurable bounded polling. Ctrl-C ends collection and records the stop when possible. An outage or interrupted session must not appear continuously healthy. Polling can miss short flaps between refreshes; do not promise a complete device event log. Static file examples must be visibly identified as examples.

## 10. Reports, Git/Stash, Jira, and overdue work

Generate useful Markdown and JSON reports containing scope, ticket, actor, baseline, actions, component identities, timestamps, before/after findings, watch events/gaps, documentation proposals, applied operations, and remaining work. Present a readable summary with supporting evidence, rather than only an opaque raw-data dump.

Publish only to explicitly requested and configured destinations. Support a dedicated Git/Stash report checkout, Jira summaries or references, and supported dcTrack notes/references through the same write policy. Preserve stable report/operation references and prevent duplicate publications after uncertain responses. Do not put credentials or unnecessary raw operational data into reports.

Provide an on-demand list of documentation outstanding more than 24 hours after implementation completion. Tie outstanding status to required actions/fields and evidence of completion. If a first version can only list review candidates, label that limitation; do not claim it proves work is undocumented. Automatic team reminders, Slack/email delivery, escalation and scheduling are deferred. Nothing sends messages while the CLI is stopped in the initial version.

## 11. CLI experience

Use a consistent command name such as `dcops`; avoid a collision with the existing Unix `dc` command. Keep commands understandable and include help, useful errors, JSON output, and stable exit behavior. Distinguish success, findings needing review, policy blocks, and incomplete/unknown results.

Cover these command families; exact spelling may be refined consistently:

- `dcops doctor`
- `dcops load-documents FILE`
- `dcops import FILE`
- `dcops inspect DEVICE` and `dcops inspect CABINET --cabinet`
- `dcops history DEVICE` and `dcops history SERIAL --serial`
- `dcops work start DEVICE --ticket CHG-1234`
- `dcops work start CABINET --cabinet --ticket CHG-1234`
- `dcops work log WORK_ID` with guided entry or `--entry FILE`
- `dcops work verify WORK_ID`
- `dcops work complete WORK_ID --reason TEXT`
- `dcops docs audit DEVICE_OR_CABINET`
- `dcops docs diff DEVICE --ticket CHG-1234`
- `dcops docs apply PLAN_ID --confirm`
- `dcops docs reconcile PLAN_ID`
- `dcops docs overdue`
- `dcops watch CABINET --work WORK_ID --interval 30`
- `dcops report export WORK_ID --output REPORT.md`
- `dcops report publish WORK_ID --to DESTINATION --confirm`

A confirmation must refer to a concrete stored proposal or publication, not approve future unrelated changes. Read-only commands do not need repeated confirmation. Logs should help engineers understand what happened without exposing secrets or dumping upstream responses indiscriminately.

## 12. Persistence, adapters, and migration

Keep separate records for approved documentation, collection runs, observations, component identity and placement history, work sessions, immutable action/amendment events, watch sessions, change proposals, write attempts, publications, and audits. Use transactions and uniqueness constraints where needed; do not build a separate subsystem for each table.

Keep inventory, Jira, artifact normalization, and monitoring integrations behind small explicit interfaces. Preserve TLS verification, configured origins, timeouts, input limits, and credential separation. Never disable certificate checking or follow redirects with credentials to arbitrary hosts to make an integration work.

Use dcTrack first. Preserve a narrow path to a later NetBox adapter using the same internal identities, evidence, history and engineer commands. Avoid dual authorities or automatic bidirectional sync. NetBox migration and vendor-specific mapping require deliberate work; a generic adapter is not a completed migration.

## 13. Deliverables and diagrams

Deliver:

- Actual modular Python source and packaging.
- Configuration templates with placeholders and writes disabled by default.
- Representative normalized inventory/LLDP, partial-collection, monitoring, Jira, NIC replacement, DIMM/SSD change, and optic reseat examples.
- A root README with an overview, project layout, installation, commands, and honest implementation status.
- Detailed architecture, data contracts, collector/import guidance, connector mapping instructions, maintenance workflows, failure handling, and future NetBox migration notes.
- Clear numbered diagrams with short labels and meaningful decision branches. Retain the relationships and uncertainty branches; do not replace a decision graph with a vague list.
- SVG or PNG diagrams embedded in Markdown so they display on mobile. Mermaid source may accompany them, but raw Mermaid must not be the only diagram. An HTML terminal-style view is a useful optional companion.
- A concise progress/gap record identifying completed behavior, unverified behavior, and environment-dependent integration work.

Use a practical module layout: common domain types, persistence, comparisons, workflow operations, connectors, CLI, optional authenticated service, and report publication. Choose module boundaries for clarity, not a predetermined file count. Keep implementation details out of routine engineer prompts unless they help an operational decision.

## 14. Validation boundary and implementation order

For the initial writing phase, do not execute application tests, contact real systems, or deploy. The user requested code writing without tests. Review source for consistency, document the checks skipped, and do not claim runtime, integration, safety, or production validation. Do not spend this phase building a large test harness. Deployment validation is a later separately authorized phase.

Implement in this order:

1. Domain contracts, persistence and normalized imports.
2. Device/cabinet inspection, documentation audit and comparison findings.
3. Component history, maintenance actions and work-session verification.
4. Documentation proposals, Jira policy and conditional write/reconciliation behavior.
5. Cabinet-scoped monitoring and durable event/gap history.
6. Reports, explicit publication and on-demand outstanding-documentation review.
7. Clear packaging, configuration and documentation.

Complete all useful work that does not depend on missing company access. When an installed API contract or artifact format is unknown, isolate that adapter, state the exact information needed, and continue with independent features. Do not ask repeatedly about routine choices already specified here. Do not silently reduce the whole product to a file-demo CLI or declare a placeholder connector complete.

Finish by stating what code was produced, which workflows are implemented, what was only source-reviewed, and the precise remaining integration gaps. Completion of the writing phase does not mean the tool is production-ready.


---

# Existing UISKILL documentation

# Build Precision Dashboard UI

A Codex UI-building skill for producing dense, reference-faithful enterprise dashboards without drifting into generic SaaS styling.

The bundled visual language is best described as **precision dark enterprise dashboard UI**, also known as **dark SaaS operations UI** or **control-room UI**.

## Style characteristics

- Near-black layered surfaces
- Hairline borders and restrained shadows
- Dense desktop-first layouts
- Compact typography and controls
- Persistent sidebars, split panes, inspectors, tables, maps, and charts
- One product accent with sparse semantic status colors
- Precise alignment and realistic operational data
- Light mode only when requested or inherited from an existing product

This is not glassmorphism, cyberpunk, or a soft consumer-dashboard aesthetic. The skill rejects neon glow, decorative gradients, oversized cards, excessive pills, giant headings, broad empty spaces, and unrelated visual motifs.

## Samples

| Analytics dashboard | Crypto admin dashboard | Data-quality dashboard |
|---|---|---|
| [![Analytics dashboard](samples/01-analytics-dashboard.jpeg)](samples/01-analytics-dashboard.jpeg) | [![Crypto admin dashboard](samples/02-crypto-admin-dashboard.jpeg)](samples/02-crypto-admin-dashboard.jpeg) | [![Data-quality dashboard](samples/03-data-quality-dashboard.jpeg)](samples/03-data-quality-dashboard.jpeg) |

| Sales CRM table | Logistics schedule | Three-pane inbox |
|---|---|---|
| [![Sales CRM table](samples/04-sales-crm-table.jpeg)](samples/04-sales-crm-table.jpeg) | [![Logistics schedule](samples/05-logistics-schedule-dashboard.jpeg)](samples/05-logistics-schedule-dashboard.jpeg) | [![Three-pane inbox](samples/06-three-pane-inbox.jpeg)](samples/06-three-pane-inbox.jpeg) |

| Engineering console | Token usage | Editor workspace |
|---|---|---|
| [![Engineering console](samples/07-engineering-console.jpeg)](samples/07-engineering-console.jpeg) | [![Token usage dashboard](samples/08-token-usage-dashboard.jpeg)](samples/08-token-usage-dashboard.jpeg) | [![Editor workspace](samples/09-editor-workspace.jpeg)](samples/09-editor-workspace.jpeg) |

## What the skill does

The skill requires Codex to:

1. Inspect the bundled inspiration images before designing.
2. Choose one primary reference based on the product's information architecture.
3. Use no more than two supporting references for missing patterns.
4. Preserve the project's framework, behavior, routes, and data flow.
5. Build the macro layout before styling individual components.
6. Apply a shared token system instead of scattered one-off CSS.
7. Pass both code validation and rendered screenshot comparison before claiming completion.

The colorful or textured backgrounds surrounding some reference applications are treated as presentation framing—not as part of the product UI.

## Included layout archetypes

| Archetype | Suitable products |
|---|---|
| Three-pane workspace | Inbox, CRM, issue tracking, task detail |
| Analytics dashboard | BI, usage, billing, product metrics |
| Dense task table | Project management, admin, work queues |
| Engineering console | IDE, ML platform, pipeline tooling |
| Operations map | Monitoring, security, incident response |
| Fleet map | Logistics, dispatch, route tracking |
| Editor workspace | Writing, documents, knowledge tools |
| Light monitoring | Light-mode observability and tracing |

## Usage

Invoke the skill directly:

```text
Use @build-precision-dashboard-ui to build a desktop network-operations dashboard.
```

It can also repair an existing interface:

```text
Use @build-precision-dashboard-ui to restyle this dashboard and fix its visual drift without breaking existing behavior.
```

## Installation

The repository root is the complete skill folder. Clone it into a skills location or import the repository as a personal skill:

```bash
git clone https://github.com/pierrebw/UISKILL.git build-precision-dashboard-ui
```

The required `SKILL.md` and UI metadata are already included.

## Repository structure

```text
UISKILL/
├── SKILL.md
├── agents/
│   └── openai.yaml
├── assets/
│   ├── inspiration/
│   │   └── 01–09 reference images
│   └── precision-ui-tokens.css
├── samples/
│   └── 01–09 example interfaces
└── references/
    ├── layout-archetypes.md
    ├── style-system.md
    └── visual-qa.md
```

## Fidelity gates

Completion requires two independent gates:

- **Code gate:** typecheck, lint, tests, build, interaction checks, overflow, focus, and accessibility.
- **Visual gate:** render at the target viewport, capture a screenshot, compare against the primary reference, and repair mismatches in topology, proportion, density, surfaces, typography, controls, and data visualization.

A successful build alone is not considered proof that the interface matches the inspiration.
