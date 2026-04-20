
---
title: Knowledge System and AI Workflow for Trading System
type: adr
status: accepted
tags: [ai, codex/claude, architecture, knowledge-system, trading-system]
created: 2026-04-18
---

# ADR 0001 — Knowledge System and AI Workflow

## 🧠 Context

The trading system being developed is:

- A **personal, high-quality trading system**
- Focused on **stocks, ETFs, and options**
- Designed for **long-term, swing, and selective short-term trading**
- Built using **Python and modern development practices**
- Intended to incorporate **AI/LLM assistance for development and reasoning**

The system will evolve over time and requires:

- A **persistent memory layer**
- A way to **accumulate, refine, and organize knowledge**
- A structure that supports **progressive understanding**, not just retrieval

Traditional approaches like RAG (Retrieval-Augmented Generation) introduce:

- Complexity (embeddings, vector DBs)
- Opaque behavior
- Weak long-term knowledge synthesis

We instead want a system that:

> Builds knowledge incrementally, transparently, and persistently

Inspired by :contentReference[oaicite:0]{index=0}’s “LLM Wiki” concept.

---

## ✅ Decision

### 1. Separate Knowledge Base from Project

A dedicated knowledge base will exist outside the trading system repo:

```text
C:\Users\bosto\knowledge-base\
````

Within it:

```text
trading-system/
  adr/
  knowledge/
```

This knowledge base is:

* Used by **Codex CLI / Claude CLI**
* NOT part of runtime application logic
* A **persistent cognitive layer**, not an execution layer

---

### 2. File-Based Knowledge System (Markdown First)

We adopt a filesystem-based approach using:

* Markdown files
* YAML frontmatter
* Obsidian-compatible structure

Example structure:

```text
knowledge/
  raw/
  processed/
  index/
  entities/
  topics/
  outputs/
```

#### Folder Roles

* `raw/` → unstructured notes, ideas, imports
* `processed/` → cleaned, structured markdown
* `entities/` → canonical pages (tickers, concepts, systems)
* `topics/` → synthesized knowledge (e.g., “swing trading rules”)
* `index/` → navigation, summaries, maps of content
* `outputs/` → generated answers (ephemeral)

---

### 3. Codex/Claude as Knowledge Manager

Codex CLI / Claude CLI are responsible for:

* Reading and processing raw inputs
* Creating structured markdown pages
* Maintaining cross-references (`[[links]]`)
* Updating summaries and indexes
* Detecting contradictions and inconsistencies
* Evolving the structure over time

**Key principle:**

> The LLM writes and maintains the knowledge base

The human (user) is responsible for:

* sourcing information
* asking questions
* guiding exploration

---

### 4. LLM Wiki Model (Core Philosophy)

This system follows the “LLM Wiki” model:

* Knowledge is **compiled once**, not re-derived per query
* The system builds a **persistent, interlinked wiki**
* Each new input:

  * updates existing pages
  * strengthens or challenges prior understanding
  * adds cross-links
  * refines summaries

**Important properties:**

* Persistent
* Compounding
* Transparent
* Inspectable

---

### 5. No Vector DB / RAG (for now)

We intentionally avoid:

* Embeddings
* Vector databases
* Retrieval pipelines

Instead we rely on:

* File structure
* Markdown linking
* LLM reasoning over files

**Rationale:**

* Simpler system
* Fully inspectable
* Easier iteration
* Better alignment with early-stage development

---

### 6. Obsidian-Compatible Design

The knowledge base is designed to work with:

* Obsidian-style markdown
* `[[wikilinks]]`
* YAML frontmatter
* Graph-based navigation

**Workflow:**

* Obsidian (or VS Code) = **IDE for knowledge**
* LLM = **programmer/editor of knowledge**
* Knowledge base = **codebase**

---

### 7. Separation of Concerns

| Layer            | Role                              |
| ---------------- | --------------------------------- |
| Codex/Claude CLI | Knowledge + development assistant |
| Knowledge Base   | Persistent memory (files)         |
| Git Worktrees    | Task isolation                    |
| Docker           | Runtime isolation                 |
| Trading System   | Application logic                 |

---

### 8. Incremental Evolution

We start minimal:

```text
knowledge/raw/
```

Structure evolves through:

* interaction with Codex/Claude
* repeated refinement
* natural emergence of entities and topics

---

### 9. Progressive Disclosure Design

All knowledge should support:

* layered detail
* summaries → deeper sections → references

This ensures:

* readability
* scalability
* usability over time

---

## 🔄 Workflow

1. Add notes to `knowledge/raw/`
2. Ask Codex/Claude to process and organize
3. LLM creates/updates structured markdown
4. LLM updates cross-links and indexes
5. Use LLM to query, refine, and evolve knowledge

Example prompt:

```text
Process the notes in knowledge/raw/trading-ideas.md and integrate them into the knowledge base.
Update relevant entity and topic pages, and create new ones if needed.
```

---

## ⚖️ Consequences

### ✅ Pros

* Simple and transparent
* Fully inspectable
* No external dependencies
* Compounding knowledge over time
* Strong alignment with LLM capabilities
* Git-friendly and versionable

### ❌ Cons

* Requires active prompting
* Structure may evolve unpredictably
* No automatic semantic retrieval
* Depends on LLM discipline/quality

---

## 🔮 Future Considerations

* Introduce light indexing or metadata search
* Add automated workflows for ingestion
* Integrate with trading system (read-only at first)
* Multi-agent workflows (Codex + Claude collaboration)
* Potential later introduction of RAG if needed

---

## 🔗 Related Notes

* [[trading-system-overview]]
* [[knowledge-system-design]]
* [[ai-assisted-development-workflow]]

---

## 💡 Key Insight

> The knowledge base is not documentation — it is a living system.

It should:

* evolve
* self-correct
* become more structured over time

The LLM is not just answering questions —
it is **maintaining and improving the system itself**.

---

```

---

## 🔧 How You Should Use This

Drop this into:

```

C:\Users\bosto\knowledge-base\trading-system\adr\0001-knowledge-system.md

```

Then immediately start using it like this:

- “Update ADR 0001 based on this new idea…”
- “Refactor the knowledge structure to separate entities and topics better”
- “Create initial index pages based on current content”

👉 That’s how it becomes a *living system*, not a static doc.

---

If you want next, the highest-leverage follow-up is:
👉 creating the first **`index/` + `entities/` + `topics/` seed structure**

That’s where the wiki actually starts becoming powerful.
```
