# CLAUDE.md

Guidance for working in this repository. Read this before making changes.

## What this project is

**Remote Soundboard** is a custom system for the carnival group **CV de Trikvögel** (Limburg, NL),
built with **WAVE Entertainment**. During carnival parades a DJ walks behind a wagon and controls
music via Spotify on a laptop wired to the wagon's speakers. Spotify has no soundboard, so this
system adds one: short themed sound effects (e.g. a sheep sound, Limburgs-dialect phrases) triggered
remotely while walking.

The full specification lives in [documentation/system_requirements_specification.md](documentation/system_requirements_specification.md).
The chosen stack and its rationale live in [documentation/frameworks.md](documentation/frameworks.md).
**These two documents are the source of truth.** When code and docs disagree, treat it as a bug to
flag, not a license to drift.

## Status

Planning / pre-implementation. The repo currently contains documentation only — there is no
application code yet. The `.gitignore` is Python-oriented, consistent with a FastAPI backend.

## Architecture

Two synchronized components that talk over the local network (typically the smartphone's cellular hotspot):

- **Soundboard remote controller** — runs on the DJ's smartphone (Android/iOS) and on desktop
  (Windows/macOS). Triggers and manages sound effects, hosts the agentic sound-retrieval UI, handles
  Google sign-in. Desktop is mainly for preparing/organising sound effects before a parade.
- **Soundboard audio host** — desktop app on the laptop wired to the speakers (Windows/macOS).
  Receives commands, plays sound effects through the OS default audio device, and controls Spotify
  volume. Runs a local HTTP/WebSocket server.

Controller and host communicate over a documented application-level protocol via HTTP/WebSocket (IR9, FR28).
The host must reject commands from unauthenticated sources (FR31, SR1).

## Technology stack

From [documentation/frameworks.md](documentation/frameworks.md):

- **FastAPI** — controller API + WebSocket channel and the host's local server. Both ends.
- **Pydantic** — protocol messages, settings, API-key/Spotify config.
- **LangGraph** — the multi-agent AI sound-retrieval workflow (parse → search → fetch → validate → preview).
- **LangChain** — LLM wrappers, web-search tool, output parsing beneath LangGraph.
- **Mistral API** — the agent's LLM. Chosen as a **European** provider to satisfy BR11/FR21.
  Each user supplies their **own** API key (FR22, DC9) — never bundle a shared key.
- **MCP** — exposes agent tools (internet search, audio download/validation) as MCP servers.
- **LangSmith** — tracing + token/cost accounting (supports the €0.50/search cap, PR4).
- **PostgreSQL + SQL** — persistence of sound-effect metadata, colours, groups, config
  (FR14, UR10–12). Hosted on Azure Database for PostgreSQL.
- **Docker** — containerizes the backend for Azure deployment.
- **pytest** — unit, integration, and end-to-end tests (QAR1–QAR3).
- **Azure AI Foundry** — deploys/serves the model from an EU region and hosts the agentic backend.
- **uv** — Python package and project manager. The `uv.lock` lockfile is committed to git.
- **Git + GitHub Actions** — version control and CI (build/test/deploy to Azure).

Integrations: **Spotify Web API** (volume control + ducking, FR23–FR27; needs Spotify Premium on the
laptop) and **Google OAuth 2.0** (the only auth mechanism, FR35–FR36, SR3).

## Hard constraints — do not violate

These come straight from the requirements and budget. Check any change against them:

- **EU AI services.** Use European-developed/hosted AI wherever feasible (BR11, DC3). Mistral is the
  chosen LLM for this reason — do not swap in a non-EU provider without flagging the conflict.
- **No shared/bundled AI key.** Each user configures their own key (DC9, FR22). Never commit a key
  or ship a default one.
- **Cost caps.** A single agentic sound retrieval search must cost ≤ €0.50 in AI API calls (BR5, PR4); total operational cost ≤ €100/yr (BR13, PR5).
  Avoid designs that imply unbudgeted paid services (DC7).
- **Latency.** Trigger-to-audio ≤ 1000 ms (BR4, PR1); UI ack of play/stop ≤ 200 ms (PR2).
- **Cross-platform.** Controller: Android, iOS, Windows, macOS (DC1). Host: Windows, macOS (DC2).
- **Secrets.** API keys/credentials stored securely on-device, never shown in UI or sent in plaintext (SR2).
- **Non-technical users.** Operable without technical expertise; user-facing text and errors in
  plain, non-technical language (BR6, NFR1, NFR3).
- **Audio format.** Host must support at least MP3 (FR7).

## Conventions

- Requirements are traceable by ID (BR/UR/FR/NFR/IR/PR/SR/DC/ESR/QAR/DR). When implementing a
  feature, reference the relevant requirement IDs in commits/PRs so the work stays traceable to the SRS.
- Keep [documentation/frameworks.md](documentation/frameworks.md) in sync when the stack changes.