# src

Home of the **deliverable** — the reusable artifact (Claude Skill or Project) that drives the [Step 1–4 workflow](../README.md#workflow) — plus any helper scripts it needs. Right now it holds the example-data generator.

| File | Purpose |
|---|---|
| `gen_data.py` | Regenerates the synthetic *example* study in `../data/synthetic-study/` (fixed seed → reproducible). Needs `openpyxl` and `reportlab`. |

The first CoFest task is choosing the artifact's form (Skill vs. Project — see the README's [Deliverable](../README.md#the-deliverable) section). Likely pieces, whatever the form:

- **Instructions / prompt** that walk an LLM through Steps 1–4 against arbitrary inputs.
- **Artifact parsing helpers** — pull text/tables/headers from XLSX, CSV, data dictionaries, and PDFs to feed the model.
- **CEDAR MCP orchestration** — author/validate (`cedar-artifact-mcp`), term lookup (`bioportal-term-mcp`), repository push/pull (`cedar-rest-mcp`), view (`cedar-cee-mcp`).
- **Canopy submission** — bootstrap a study from the Step 1 instance and attach files (Step 4).
