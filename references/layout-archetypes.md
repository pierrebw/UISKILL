# Layout archetypes and source-image index

Choose the reference whose information architecture most closely matches the requested product. Do not average all nine images into one interface.

| Image | Primary use | Defining structure |
|---|---|---|
| `01-inbox-detail.jpeg` | Inbox, notifications, CRM, issue/task detail | Persistent sidebar + grouped list + fixed detail inspector; quiet black surfaces and strong text hierarchy |
| `02-analytics-overview.jpeg` | Competitive analytics, BI, product metrics | Sidebar + filter bar + KPI strip + paired analytics panels + dense heatmap table; green-forward status accents |
| `03-task-table.jpeg` | Project management, admin tables, work queues | Sidebar + project header tabs + filters + compact full-width table; status pills and avatars |
| `04-engineering-console.jpeg` | IDE, ML platform, code/data tooling | Narrow icon rail + module sidebar + top tabs + editor canvas; monospace type and amber technical accent |
| `05-operations-map.jpeg` | Security, monitoring, incident response | Sidebar + KPI strip + central dark map + right live feed + bottom media strip; semantic incident colors |
| `06-fleet-map.jpeg` | Logistics, dispatch, fleet tracking | Icon rail + fleet list/stats + dominant route map; bright blue route as the single focal accent |
| `07-usage-analytics.jpeg` | Billing, token usage, infrastructure metrics | Wide full-canvas dashboard + compact filters + KPI cards + large chart + dense data table |
| `08-editor-workspace.jpeg` | Writing, knowledge, documents | Sidebar + narrow context tabs + generous editor column; black canvas, subdued chrome, focused content |
| `09-light-monitoring.jpeg` | Light-mode monitoring only | Sidebar + toolbar + edge-to-edge bordered chart grid; orange action/series accent and almost no shadow |

## Selection rules

- Use images 01 or 03 when the core task is scanning and acting on records.
- Use images 02 or 07 when the core task is comparing metrics and trends.
- Use image 05 or 06 when geography or movement is the dominant object.
- Use image 04 when code, files, tests, or technical pipelines are central.
- Use image 08 when the content itself needs visual priority over navigation.
- Use image 09 only for a requested or inherited light theme.
- Use a secondary reference only for a missing pattern, such as borrowing the detail pane from 01 for a table based on 03.

## Composition rules

- Keep persistent global navigation on the left.
- Place scope controls and filters above the data they affect.
- Let the dominant work object occupy the largest contiguous region.
- Keep detail or event context in a fixed side pane when cross-referencing matters.
- Use edge-to-edge tables and charts within bounded regions; avoid wrapping each row or series in a separate card.
- Crop or scroll dense content rather than shrinking type below the specified range.

## Presentation-frame rule

Images 02, 04, 05, 06, and 08 include an external colorful or textured backdrop around the app. That backdrop is not part of the interface. The app may receive a rounded outer shell for a promotional mockup, but production screens should use the in-app surfaces only.
