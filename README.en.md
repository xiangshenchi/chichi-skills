<div align="center">

**English** · [中文](./README.md)

# 🛠️ Shawn Skills

#### AI Skills I use daily in development — from code analysis and thesis docs to test reports and knowledge management

[![License](https://img.shields.io/badge/License-MIT-3B82F6?style=for-the-badge)](./skill-creator/LICENSE.txt)
[![Skills](https://img.shields.io/badge/Skills-7-10B981?style=for-the-badge)](#-skills)
[![AgentSkills](https://img.shields.io/badge/AgentSkills-Standard-8B5CF6?style=for-the-badge)](https://agentskills.io)

![Claude Code](https://img.shields.io/badge/Claude_Code-Skill-D97706?style=flat-square&logo=anthropic&logoColor=white)
![Codex](https://img.shields.io/badge/Codex-Skill-10B981?style=flat-square&logo=openai&logoColor=white)

</div>

A collection of battle-tested AI Skills I've built and used across real projects — nothing fancy, just practical tools that actually save time.

Every Skill here is a structured instruction set that AI agents can load directly, following the [Agent Skills](https://agentskills.io) open standard. Compatible with Claude Code and Codex.

---

## 📋 Catalog

| Name | One-liner |
|---|---|
| 🧪 [**repo-test-analyst**](#-repo-test-analyst) | Auto-analyze your project repo and generate comprehensive test reports & code |
| 🎯 [**yolo-report-analyst**](#-yolo-report-analyst) | Analyze YOLO training results and generate professional reports in one click |
| 📐 [**code-diagram-analyst**](#-code-diagram-analyst) | Analyze code structure and auto-generate software engineering diagrams |
| 📖 [**thesis-doc-agent**](#-thesis-doc-agent) | Read your project code and generate graduation thesis‑ready software engineering docs |
| ✍️ [**skill-article-writer**](#-skill-article-writer) | Fetch platform rules from a URL and auto-generate compliant Skill intro articles |
| 🔧 [**skill-creator**](#-skill-creator) | A guide for creating high-quality, well-structured Agent Skills |
| 🗣️ [**code-explainer**](#-code-explainer) | A collection of code-explanation skill packages |

---

## ✨ Skills

<table>
<tr><td>

### 🧪 repo-test-analyst

> *"Drop your project in — get test reports, test code, and security scans all in one go."*

Hand your project repo (zip, folder, or GitHub link) to the agent and it runs a full test analysis — from directory exploration and tech-stack identification to strategy selection, test execution, and report generation. Fully automatic.

**What it does**

- Auto‑detects language, framework, and architecture type (Python / JS / TS / Java / Go / Rust…)
- Picks the optimal test strategy (unit, integration, E2E, security, performance…)
- Runs static analysis, security scans, and dependency audits
- Outputs test code + structured Markdown reports

**How to trigger**

```
analyze this project / repo       # full analysis
generate a test report            # report output
what's wrong with this project    # problem check
check code quality                # code quality
```

**📦 Includes**: CHANGELOG.md · CONTRIBUTING.md · README.md · SKILL.md · references/ · social_articles/

→ [SKILL.md](./repo-test-analyst/SKILL.md) · [README](./repo-test-analyst/README.md)

</td></tr>
</table>

<table>
<tr><td>

### 🎯 yolo-report-analyst

> *"YOLO training done? Try generating a report in one sentence."*

Automatically parse YOLO training outputs (logs, CSV, YAML, images, etc.) and generate a complete professional report covering configuration, performance evaluation, training analysis, issue diagnosis, and improvement suggestions.

**What it does**

- Parses `results.csv`, `args.yaml`, training logs, confusion matrices, PR curves, and more
- Auto‑identifies the best epoch, detects overfitting signals, analyzes loss convergence
- Per‑class performance evaluation to pinpoint weak categories
- Generates targeted improvement suggestions based on metrics
- Supports both Markdown and Word document output
- Produces usable reports even with incomplete files

**How to trigger**

```
analyze my training results       # training analysis
how did this training go          # performance review
generate a training report        # report output
summarize the results             # quick summary
```

**📦 Includes**: CHANGELOG.md · CONTRIBUTING.md · README.md · README_EN.md · SKILL.md · example/ · references/ · social_articles/

→ [SKILL.md](./yolo-report-analyst/SKILL.md) · [README](./yolo-report-analyst/README.md)

</td></tr>
</table>

<table>
<tr><td>

### 📐 code-diagram-analyst

> *"One sentence turns your code structure into clear architecture diagrams."*

Analyze project code and automatically generate the right software engineering diagrams — rendered with Mermaid right in the conversation.

**What it does**

- **Class Diagram** — class definitions, inheritance, interfaces
- **ER Diagram** — database models, ORM schemas
- **Sequence Diagram** — API calls, message flows
- **Architecture Diagram** — multi‑layer architecture, microservice topology
- **Component Diagram** — module dependencies
- **Flowchart** — business logic, state machines
- **State Diagram** — state enums and transitions
- **Dependency Graph** — package and module references
- **Data Flow Diagram** — data processing pipelines

**How to trigger**

```
generate a class/architecture/ER/sequence diagram   # specify diagram type
analyze project structure / code relationships      # structural analysis
visualize system architecture / module dependencies # architecture viz
generate UML diagrams                               # UML
```

**📦 Includes**: SKILL.md · references/mermaid-templates.md · references/language-patterns.md

→ [SKILL.md](./code-diagram-analyst/SKILL.md) · [Mermaid Templates](./code-diagram-analyst/references/mermaid-templates.md)

</td></tr>
</table>

<table>
<tr><td>

### 📖 thesis-doc-agent

> *"A thesis writer's best friend — code done, docs handled."*

A Skill for Claude Code and compatible AI coding agents. Given a project codebase or conversation history, it analyzes the project across multiple software engineering dimensions and produces graduation-thesis‑ready documentation with academic rigor.

**What it does**

- **Requirements Analysis** — functional/non‑functional requirements, use‑case modeling
- **Technical Architecture** — system layering, tech choices, architecture decisions
- **Database Design** — ER diagrams, table schemas, relationship mapping
- **Module Design** — core module breakdown and interface definitions
- **Test Plan** — testing strategy, test case design
- **Project Summary** — technical highlights and future improvements

**Core Principles**

- Mandatory code traceability — every code reference includes exact file paths
- Mermaid diagrams preferred for explaining complex structures
- Precise language, clear logic, meeting academic writing standards

**How to trigger**

```
generate graduation thesis documentation           # full suite
analyze my project / code / architecture            # project analysis
write requirements / technical / design docs        # specific doc type
generate thesis documentation                       # English
```

**📦 Includes**: CHANGELOG.md · CONTRIBUTING.md · README.md · README.en.md · SKILL.md · example/ · references/ · social_articles/

→ [SKILL.md](./thesis-doc-agent/SKILL.md) · [README](./thesis-doc-agent/README.md)

</td></tr>
</table>

<table>
<tr><td>

### ✍️ skill-article-writer

> *"Give it a Skill link and the platform rules — it writes the article for you."*

Given a platform or competition rules page URL, it fetches the rules, combines them with your Skill info, and generates an intro or submission article that fully complies with the platform's requirements.

**What it does**

- Auto‑fetches and parses platform submission rules
- Adapts article structure and style to match the rules
- Generates content based on the Skill's core features
- Supports multiple platform format requirements

**How to trigger**

```
write a competition submission article              # contest entry
write an article following this site's rules        # rule adaptation
write a Skill intro article                         # promotion
follow the requirements in this link                # by-request output
```

→ [SKILL.md](./skill-article-writer/SKILL.md)

</td></tr>
</table>

<table>
<tr><td>

### 🔧 skill-creator

> *"Want to write your own Skill but don't know where to start? This guide is your starting point."*

A complete guide for creating high-quality, well-structured Agent Skills. Covers core principles, file structure conventions, reference resources, and best practices.

**What it does**

- Defines core Skill principles — conciseness, completeness, reusability
- Provides a standard SKILL.md template and file structure
- Includes reference resource examples (output templates, workflows, etc.)
- Cross-platform compatibility guidance (Claude Code, Codex, OpenCode, etc.)

**📦 Includes**: LICENSE.txt · SKILL.md · references/ · scripts/

→ [SKILL.md](./skill-creator/SKILL.md) · [LICENSE](./skill-creator/LICENSE.txt)

</td></tr>
</table>

<table>
<tr><td>

### 🗣️ code-explainer

> *"Code you don't understand? Let it explain it to you."*

A collection of code-explanation skill packages for different scenarios. Enables agents to explain code clearly and structurally — from small snippets to full projects.

**Packages**

| Package | Description |
|---|---|
| `code-explainer.zip` | Basic code explanation skill |
| `code-explainer-plus.zip` | Enhanced code explanation |
| `code-explainer-thesis.zip` | Thesis‑focused code explanation |
| `project-analyzer.zip` | Project‑level analysis |
| `project-compass.zip` | Project navigation guide |
| `repo-narrator.zip` | Full repo walkthrough |

→ Located in [./code-explainer/](./code-explainer/)

</td></tr>
</table>

---

## 🌟 About This Project

This repository is a collection of AI Skills I've built and refined through real‑world development. Every Skill here has been proven in actual projects and genuinely saves time.

It also contains the **[LLM Wiki](./LLM-wiki.md)** philosophy document — a pattern for having LLMs build and maintain your personal knowledge base. If the Skills above are "tools," the LLM Wiki is the "methodology": it enables agents not just to answer questions, but to **build an accumulating knowledge system** that grows richer over time.

These Skills were all used in my own projects before being open‑sourced. If you find them helpful, a ⭐ is much appreciated. Questions or suggestions? Feel free to open an Issue.

---

## 📦 Installation

In a Skill‑compatible Agent (Claude Code, Codex, etc.), just say:

```
install this skill: https://github.com/xiangshenchi/chichi-skills/tree/main/<skill-name>
```

Or manually place the Skill directory into your Agent's Skills configuration path.

---

<div align="center">

[MIT License](./skill-creator/LICENSE.txt) · Free to use / modify / redistribute

</div>
