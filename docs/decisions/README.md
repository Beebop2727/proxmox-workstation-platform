---
type: decision
decision_id: ADR-001
status: accepted
date: 2026-08-27
scope: documentation
tags:
  - knowledge-management
  - obsidian
  - local-ai
  - retrieval
---

# Decision records and the project research brain

## Decision summary

This repository will grow from a build record into a structured technical knowledge base for the Proxmox workstation platform.

Markdown files in Git will remain the canonical source. GitHub will provide readable public documentation and change history, while Obsidian can be used as a local interface for browsing, linking, and maintaining the same files. In the future, a local AI assistant may index the sanitized Markdown corpus to provide fast, source-linked answers about the platform.

## Context

The project already contains useful setup notes, progress records, troubleshooting discoveries, and lessons learned. Without a consistent structure, that knowledge becomes harder to find as the repository grows. Important context can also disappear into terminal history, browser tabs, chat conversations, or memory.

The documentation should support three kinds of reader:

1. A person reading the project on GitHub.
2. The project owner working through the notes in Obsidian.
3. A future local retrieval system answering questions from the repository.

## Decision

The repository will use plain Markdown and predictable metadata to record research, decisions, procedures, incidents, and dated progress.

The planned documentation areas are:

| Path | Purpose | Authority |
|---|---|---|
| `docs/decisions/` | Architectural and operational choices, including rejected alternatives | Accepted decisions |
| `docs/research/` | Questions, evidence, experiments, sources, and provisional conclusions | Not authoritative until tested |
| `docs/runbooks/` | Repeatable procedures with verification and rollback steps | Operational guidance when marked tested |
| `docs/incidents/` | Symptoms, diagnosis, resolution, and prevention | Evidence from real failures |
| `docs/progress/` | Dated snapshots of the build | Historical record |
| `docs/sources/` | Curated source notes and links | Reference material |

These folders can be added when their first useful document is created. Empty structure will not be added merely for appearance.

## Documentation conventions

Each substantial note should make its reliability and context obvious. Where relevant, include:

- A concise outcome or summary near the top.
- `type`, `status`, and date properties in YAML front matter.
- The hardware, software version, and environment to which it applies.
- A distinction between observed facts, inferences, and decisions.
- Commands in fenced code blocks with placeholders instead of secrets.
- Verification steps that demonstrate whether a procedure worked.
- Rollback or recovery steps for changes that can fail.
- Problems encountered, including approaches that did not work.
- Links to primary sources and related repository documents.
- A `last_verified` date for operational instructions.

File names should be descriptive and stable. A document should normally cover one main subject so that people and retrieval tools can identify its purpose without interpreting an entire folder.

## Obsidian usage

The repository can be opened directly as an Obsidian vault because its source documents are ordinary Markdown files. Obsidian is an interface, not a required storage format.

Useful Obsidian features may include:

- Properties for status, subject, platform version, and verification dates.
- Backlinks for connecting systems, failures, research, and procedures.
- Bases for views such as active research, untested runbooks, and stale documentation.
- Templates for consistent decisions, runbooks, research notes, and incident reports.

Standard relative Markdown links should be preferred where practical so documents remain navigable on GitHub and outside Obsidian.

## Future local-AI use

The initial retrieval design is intentionally simple:

```text
Sanitized repository clone
          |
          v
Markdown parser and heading-aware chunker
          |
          v
Local search or embedding index
          |
          v
Local language model
          |
          v
Answer with links to the source file and heading
```

A future assistant should:

- Index Markdown text and its front-matter properties.
- Chunk documents along heading boundaries rather than arbitrary character counts.
- Retain the repository path, heading, status, and verification date with every chunk.
- Cite the source document and heading in each technical answer.
- Prefer tested runbooks and accepted decisions over provisional research.
- Surface conflicting or outdated notes instead of silently choosing one.
- Say when the repository does not contain enough evidence to answer.

The first version does not require a vector database. Markdown search and structured metadata may be sufficient while the corpus remains small. More complex retrieval should be introduced only after real queries show a need for it.

## Public/private boundary

This is a public repository and therefore only contains a sanitized, publishable knowledge corpus.

Never commit:

- Passwords, API tokens, cookies, private keys, or recovery codes.
- WireGuard private keys or complete live VPN configurations.
- Personal data or identifying account details.
- Secret values copied into screenshots, logs, command output, or configuration files.
- Sensitive network or host details that are unnecessary to explain the design.

Secrets belong in a password manager. Private operational notes should live in a separate private vault or repository. Public examples should use unmistakable placeholders such as `<PBS_HOST>` and `<API_TOKEN>`.

A local AI index must preserve this boundary: indexing a private vault is a separate decision and must not cause private material to be copied into this public repository.

## Consequences

### Benefits

- Research becomes reusable project knowledge rather than disposable notes.
- Decisions retain their reasoning and rejected alternatives.
- Procedures become easier to repeat, audit, and restore from.
- Git provides a reviewable history of how understanding changes.
- The Markdown corpus remains portable and suitable for future local retrieval.

### Costs and risks

- Notes require occasional review to avoid becoming stale.
- Metadata and templates add a small amount of writing overhead.
- AI answers can still be wrong if source notes are wrong, contradictory, or outdated.
- A public knowledge base requires deliberate sanitization before every commit.

## Initial implementation

The next useful documents should be created as the work occurs:

1. A research note comparing Proxmox off-site backup approaches using Filen.
2. A decision record selecting the first backup experiment.
3. A tested runbook for backup, verification, and full restoration.
4. An incident note for any failed upload, verification, or restore attempt.
5. A small local retrieval prototype once enough trusted notes exist to evaluate it.

## Review trigger

Review this decision when one of the following happens:

- The repository becomes difficult to navigate with its current structure.
- Obsidian-specific features reduce GitHub readability.
- A local AI prototype is ready to ingest the documentation.
- Private operational notes need to be linked with the public corpus.
- Documentation maintenance becomes too burdensome to sustain.
