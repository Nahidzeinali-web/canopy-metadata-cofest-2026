# Runbook — driving the workflow (Steps 1–4)

Step-by-step companion to the [workflow in the README](../README.md#workflow). The goal of CoFest is to build a **reusable artifact (Claude Skill or Project)** that performs these steps on *any* input; this runbook is both the manual procedure and the spec that artifact must automate. Fill in commands as it matures.

## Prerequisites

- A CEDAR account + API key — <https://cedar.metadatacenter.org/>
- A BioPortal account + API key — <https://bioportal.bioontology.org/>
- The CEDAR MCP servers installed and registered with your LLM client:
  - [`cedar-artifact-mcp`](https://github.com/metadatacenter/cedar-artifact-mcp) (author/validate templates & instances)
  - [`bioportal-term-mcp`](https://github.com/metadatacenter/bioportal-term-mcp) (ontology lookups)
  - `cedar-rest-mcp` (push/pull to CEDAR) — *in development*
  - `cedar-cee-mcp` (view/render templates) — *in development*

```bash
export CEDAR_API_KEY=...        # do NOT commit
export BIOPORTAL_API_KEY=...
```

## Step 0 — Inputs
Use the bundled synthetic study in [`../data/synthetic-study/`](../data/synthetic-study/): a dataset spreadsheet, a protocol PDF, and an SOP PDF. (Or bring your own XLSX + PDFs — the artifact must work generically.)

## Step 1 — Fill the Canopy Study instance
Pull the live Canopy Study template from its well-known CEDAR location (`cedar-rest-mcp`). Infer field values from the protocol PDF + spreadsheet, resolve controlled terms with `bioportal-term-mcp`, and produce a valid `study-metadata.json` (`cedar-artifact-mcp`). **Keep this instance — it bootstraps the Canopy study in Step 4.**

## Step 2 — Create a domain-specific template
Design a flat, ontology-controlled template for the dataset (see [`../templates/domain-specific-template.yaml`](../templates/domain-specific-template.yaml) for the target shape). Author it with `cedar-artifact-mcp`, resolve controlled terms with `bioportal-term-mcp`, upload with `cedar-rest-mcp`, and verify with `cedar-cee-mcp` or the CEDAR UI.

## Step 3 — Fill the domain-specific instance
Pull the template authored in Step 2. Infer values from the artifacts, produce a valid `domain-specific-metadata.json`, and upload it to CEDAR.

## Step 4 — Create the study in Canopy
Create a new study in Canopy, bootstrapping it from the **Step 1** `study-metadata.json`, with files attached:
`study-metadata.json` (pre-fills study fields), `domain-specific-template.json` + `domain-specific-metadata.json` (added as a new category and rendered).
Staging: <https://staging.canopyplatform.org/> · Production (coming): <https://canopy.stanford.edu/>

## Definition of done
The reusable artifact takes the example study from raw files to a registered Canopy submission with no manual CEDAR work — both instances validate against their templates, controlled values resolve to real ontology term IDs — and the same artifact runs against a *different* set of input artifacts.
