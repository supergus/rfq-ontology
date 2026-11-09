# Repo guide: rfq-ontology

A map of this repository for auditors (CS IT) and new contributors. Last checked against `main` on 2026-11-09.

> **Demo status / archived.** This is an **early proof-of-concept** tutorial OWL ontology for the agentic RFQ-to-quote workflow that preceded the production **CS Agent Orchestration Suite**. The GitHub repository is normally **archived**. Its contents were folded (with history) into [`pla-agents`](https://github.com/supergus/pla-agents) under `ontology/`, which is now the live copy for the PLA demo. This repo may stay archived or be deleted once the suite is in production. **Inference (flagged):** production ontology / Graph View work moved to **Leaf Tender**; the toy RFQ vocabulary here is tutorial-only and is not the suite's server graph.

**Names used here.** The **CS Agent Orchestration Suite** is the production software (`leaf-tender`, `agent-orchard`, `agent-buffet`, `suite-landing`). **CS** is the company; **CS IT** is its IT organisation.

## Purpose

Tutorial **OWL 2** ontology for a manufacturing RFQ-to-quote process: package completeness, BOM / make-vs-buy, component library, costing, and a PLA (program / tools / issues) vocabulary, plus SHACL shapes, SPARQL competency queries, a deterministic quote script, and a worked example (sample RFQ-1001).

Namespace (historical): `https://supergus.github.io/rfq-ontology/ns#` (GitHub Pages for the ns was planned, not published).

## Where it runs

| Item | Value |
|---|---|
| Hosted service | **none** (library / docs / CI only) |
| Consumed by | Historically `pla-agents` / `pla-demo`; live tree is now `pla-agents/ontology/` |
| Secrets | **none** |
| Data policy | Toy / sample individuals only |

## Folder tree

```
rfq-ontology/
├── .github/workflows/   CI (ROBOT + pytest)
├── data/                sample RFQs, reference library, sample PLA run
├── docs/                concepts, formulas, worked example, roadmap, …
├── ontology/            OWL Turtle modules
├── queries/             CQ1–CQ12 SPARQL
├── scripts/             quote.py, reason.sh, …
├── shapes/              SHACL
└── tests/               pytest
```

## Every tracked file

### Root
| File | What it is for |
|---|---|
| `.github/pull_request_template.md` | PR checklist. |
| `.github/workflows/ci.yml` | Workflow **tests**: Turtle parse, ROBOT/HermiT, SHACL, SPARQL, quote numbers; uploads `build/` artifact. |
| `.gitignore` | Ignores `build/`, `.venv/`, jars, etc. |
| `CONTRIBUTING.md` | Protected `main`, PR + Grok Bot QC. |
| `README.md` | What is in the repo, how to run checks, worked-example table, related repos. |
| `REPO-GUIDE.md` | This file. |
| `requirements.txt` | `rdflib`, `pyshacl`, `pytest`. |

### ontology/
| File | What it is for |
|---|---|
| `ontology/rfq.ttl` | Root module (`owl:imports` the others). |
| `ontology/rfq-core.ttl` | Core classes / properties. |
| `ontology/rfq-provenance.ttl` | Provenance vocabulary (plus PROV-O reuse). |
| `ontology/rfq-package.ttl` | RFQ package / completeness. |
| `ontology/rfq-bom.ttl` | BOM / make-vs-buy. |
| `ontology/rfq-library.ttl` | Component library. |
| `ontology/rfq-costing.ttl` | Costing / rates / breaks. |
| `ontology/rfq-pla.ttl` | PLA programs, tools, tool runs, issues, approval. |
| `ontology/catalog-v001.xml` | Protégé catalog. |

### data/
| File | What it is for |
|---|---|
| `data/reference.ttl` | Sample library parts, routings, work centers, rate tables. |
| `data/rfq-1001.ttl` | Complete sample RFQ (worked example BRK-100). |
| `data/rfq-1002-incomplete.ttl` | Incomplete sample RFQ (missing Incoterm / material). |
| `data/pla-run-1002.ttl` | Sample PLA run graph for competency queries. |

### shapes/ and queries/
| File | What it is for |
|---|---|
| `shapes/rfq-completeness.ttl` | SHACL closed-world completeness + BOM/match quality. |
| `shapes/pla-shapes.ttl` | SHACL for PLA state. |
| `queries/cq1-rfq-completeness.rq` … `cq12-rfq-line-matches.rq` | Twelve competency SPARQL queries. |

### scripts/
| File | What it is for |
|---|---|
| `scripts/quote.py` | Computes the worked-example quote; optional `--ttl` PROV output. |
| `scripts/run_queries.py` | Runs every competency query and prints answers. |
| `scripts/reason.sh` | ROBOT: merge, OWL 2 DL profile, HermiT, report. |
| `scripts/rfqlib.py` | Shared helpers. |

### docs/
| File | What it is for |
|---|---|
| `docs/PROJECT.md` | Goals, design, PLA, demo, CI overview. |
| `docs/concepts.md` | Domain concepts. |
| `docs/formulas.md` | Cost / price formulas. |
| `docs/worked-example.md` | Full arithmetic for RFQ-1001. |
| `docs/competency-questions.md` | CQ list and intent. |
| `docs/pla-model.md` | PLA vocabulary write-up. |
| `docs/sample-llm-prompts.md` | Example prompts for AI steps (fictional email). |
| `docs/sessions.md` | Tutorial session outline. |
| `docs/simplifications.md` | What the toy model leaves out. |
| `docs/roadmap.md` | Future items (e.g. pyLODE docs) — not built. |

### tests/
| File | What it is for |
|---|---|
| `tests/conftest.py` | Shared fixtures. |
| `tests/test_ontology.py` | Turtle parses / basic structure. |
| `tests/test_shacl.py` | pySHACL expectations. |
| `tests/test_queries.py` | Competency query answers. |
| `tests/test_quote.py` | Exact worked-example numbers. |
| `tests/test_reasoner.py` | HermiT inferences (when `REQUIRE_REASONER=1`). |
| `tests/test_pla.py` | PLA vocabulary checks. |

## Run tests

```
python -m venv .venv && . .venv/bin/activate
pip install -r requirements.txt
python -m pytest -q
# Full OWL checks need Java + ROBOT — see README / .github/workflows/ci.yml
```

No deploy. Prefer editing the live copy in `pla-agents/ontology/` unless there is a deliberate reason to touch this archived tree.

## Where to look (audit topics)

| Topic | Look here |
|---|---|
| Vocabulary definitions | `ontology/*.ttl` |
| Validation | `shapes/`, `tests/test_shacl.py` |
| Cost math | `scripts/quote.py`, `docs/formulas.md`, `docs/worked-example.md` |
| Secrets / customer data | none |

## Findings

| Kind | Where | Notes |
|---|---|---|
| Stale related-repo links in README | `README.md` still names `rfq-agents` / `rfq-demo` | Repos were renamed to `pla-agents` / `pla-demo`; archive snapshot. |
| No secret values or real customer PII found | — | Toy data only. |
