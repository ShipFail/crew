# Crew

> **Mount the Crew. Ship the work.**

`crew` is the **Ship.Fail distribution** of AI co-founders for **PromptWareOS**.

* **PromptWareOS** (`shipfail/promptware`) provides the *boot protocol*, *kernel primitives*, and *system-call-like tools*.
* **Crew** (`shipfail/crew`) provides the *userland*: **agents (personas)**, **skills**, and **tool contracts**.
* **Any project repo** (e.g. `shipfail/thoth`) provides the *worktree* and a single canonical **boot file** (usually `AGENTS.md`).

This repo is intended to be **mounted as `$HOME`** by PromptWareOS conventions so every Ship.Fail project can load the same co-founders **from the cloud**.

---

## The three-repo model

```
  shipfail/promptware     shipfail/crew            shipfail/<project>
  (kernel + boot)         (distro/userland)        (app/worktree)
        │                      │                        │
        └──── boot protocol ────┴── mount as HOME ───────┘
                              
            Project boot file (AGENTS.md) tells the agent to:
            1) mount /work  (the project repo)
            2) mount HOME   (this repo: crew)
            3) init agent   (a persona from crew)
```

---

## What lives here

* **Agents**: each co-founder is a versioned, documented persona.
* **Skills**: reusable procedures and operating rules.
* **Tools**: contracts for tool usage (intent, args, outputs, constraints).
* **Profiles**: project overlays (glossary, conventions, boundaries) for maximum extensibility.

---

## Quickstart

### 1) Add a boot file to your project repo

Create `AGENTS.md` in your project repo (example: `shipfail/thoth`).

```md
# PromptWareOS Boot

BOOTLOADER:
- Load PromptWareOS boot protocol: https://github.com/shipfail/promptware
- Mount the project worktree as: /work
- Mount Ship.Fail Crew as HOME: os://crew

MOUNT:
- os://crew = https://github.com/shipfail/crew@v0.1.0

INIT:
- Activate agent: os://crew/agents/cartographer/agent.md
- Apply profile overlay (optional): os://crew/profiles/thoth/profile.md
```

> **Pin by default** (`@v0.1.0` or `@<commit-sha>`). If you want the “edge channel,” mount `@main`.

### 2) Make tool-specific entrypoints point to `AGENTS.md`

For maximum compatibility, keep a single canonical boot file and make others forward to it:

* `CLAUDE.md`
* `GEMINI.md`
* `CODEX.md`

Example:

```md
This repo boots via **AGENTS.md**.

Please read `AGENTS.md` and follow it as the canonical bootloader.
```

---

## Repo layout

```text
crew/
  README.md

  agents/
    <agent-name>/
      agent.md
      skills.yaml        # optional: agent skill selection
      tools.yaml         # optional: agent tool selection
      prompts/           # optional: reusable blocks/snippets

  skills/
    <skill-name>/
      skill.md
      examples/          # optional

  tools/
    <tool-name>/
      tool.md            # contract: intent, args, I/O, constraints

  profiles/
    <project-name>/
      profile.md         # project overlay: conventions, glossary, boundaries
      mounts.md          # optional: recommended mounts

  dist/
    manifest.yaml        # distro index: what exists, defaults
    versions.md          # changelog / release notes
```

---

## Naming conventions

Consistency is the whole point of a distro.

### Agents

* Use **noun-based personas**: `cartographer`, `blacksmith`, `librarian`, `auditor`, `pilot`.
* Each agent must have **one** canonical `agent.md`.

**Path contract:**

* `os://crew/agents/<agent-name>/agent.md`

### Skills

* Use **verb-first** names: `write_prd`, `review_code`, `triage_issues`, `design_api`.

**Path contract:**

* `os://crew/skills/<skill-name>/skill.md`

### Tools

* Tool documents describe **contracts**, not implementation.
* Prefer `os_*` for OS primitives and `tool_*` for external integrations.

**Path contract:**

* `os://crew/tools/<tool-name>/tool.md`

---

## Versioning and releases

Crew is a **distribution**. Treat it like one.

* Use tags like `v0.1.0`, `v0.1.1`, …
* Keep a concise changelog at `dist/versions.md`.
* Projects should mount **pinned** versions for deterministic behavior.

Recommended policy:

* **Stable**: projects pin tags.
* **Edge**: experimental projects mount `@main`.

---

## Contributing

### Add a new agent

1. Create `agents/<agent-name>/agent.md`.
2. Optionally add `skills.yaml` and `tools.yaml` to declare dependencies.
3. Add the agent to `dist/manifest.yaml`.

Agent checklist:

* Clear mission + boundaries
* Input/output expectations
* Default workflow (plan → execute → self-review)
* Failure modes and what to do next

### Add a new skill

1. Create `skills/<skill-name>/skill.md`.
2. Keep it **small and composable**.
3. Add at least one minimal example if the skill is non-obvious.

### Add a project profile

1. Create `profiles/<project-name>/profile.md`.
2. Include glossary + conventions + “do/don’t.”
3. Keep it strictly **project-specific**.

---

## Design principles

* **Convention-only**: tools read boot files; this repo stays implementation-agnostic.
* **Determinism first**: pin versions; float only when you mean to.
* **Sharp roles**: agents should be focused, not generic blobs.
* **Small skills**: skills should feel like Unix commands—composable and testable.

---

## FAQ

### Is this public?

For now it can be internal; structure is designed so parts can be open-sourced later.

### Why a separate repo?

So agents and skills can be upgraded once and reused everywhere without copy/paste.

### Can one project use multiple agents?

MVP starts with one. Future versions can support **teams** (multiple active personas).

---

## Roadmap

* [ ] `dist/manifest.yaml` and a minimal “distro index”
* [ ] `cartographer` + `blacksmith` starter agents
* [ ] Core skill set: clarify → plan → read → change → self-review
* [ ] Lightweight regression prompts (behavior checks)

---

## License

**Public Prompt License - Apache Variant (PPL-A)**

This project is licensed under the **Public Prompt License - Apache Variant (PPL-A)**. This license extends the Apache License 2.0 to explicitly cover "Prompt Source" while maintaining full Apache 2.0 compatibility for code and tools.

See [LICENSE](LICENSE) for the full text.

---

## Links

* PromptWareOS kernel: [https://github.com/shipfail/promptware](https://github.com/shipfail/promptware)
* Ship.Fail: [https://ship.fail](https://ship.fail)
