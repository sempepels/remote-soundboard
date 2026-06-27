# CLAUDE.md
Guidance for working in this repository. Read this before making any changes.

## Project description
The **Remote Soundboard system** is a custom system for the carnival group **CV de Trikvögel** (Limburg, NL), built with **WAVE Entertainment**.
During carnival parades the DJ from WAVE Entertainment walks behind the carnival wagon and controls the music via Spotify on a laptop wired to the wagon's speakers.
This Spotify setup lacks a soundboard, which is featured by the Remote Soundboard system.
With this soundboard short themed sound effects (e.g. a sheep sound, Limburgs-dialect phrases) can be triggered remotely while walking.
Beyond manual triggering, the system uses an agentic AI workflow: the DJ can provide a long audio file and describe a quote or sound effect in natural language; a multi-agent pipeline will then locate and extract the matching fragment as a ready-to-use sound effect.

## Status
**Planning / pre-implementation**
The repo currently contains documentation only, there is no application code yet.

## Architecture
The architecture of the Remote Soundboard system consists of two synchronized components that talk over the local network (typically the smartphone's cellular hotspot):
- **Soundboard remote controller** — This instance runs on the DJ's smartphone (Android/iOS) and on desktop (Windows/macOS). Triggers and manages sound effects, hosts the agentic sound processing features, handles Google sign-in. The desktop instance is mainly used for preparing/organizing sound effects before a parade.
- **Soundboard audio host** — This is a desktop app on the laptop wired to the speakers (Windows/macOS). Receives commands, plays sound effects through the OS default audio device, and controls Spotify volume. Runs a local HTTP/WebSocket server.

Controller and host communicate over a documented application-level protocol via HTTP/WebSocket (IR9, FR28).
The host must reject commands from unauthenticated sources (FR31, SR1).

## Core features
The Remote Soundboard system has the following core features:
- **Remote soundboard control.** - The DJ triggers, stops, and adjusts the volume of sound effects on the audio host from the controller, with a visual indication of what is playing (UR1–UR4, FR1–FR7). The main screen is a grid of labeled, color-coded buttons (IR1, FR8). Buttons and groups are color-coded for quick recognition during a parade (UR11–UR12, FR32–FR34, NFR2, NFR4).
- **Sound effect management.** - The DJ adds effects by importing a file or recording from the device microphone, and can rename or delete them (UR5–UR9, FR9–FR12). Effects and metadata persist across sessions (UR10, FR14) and sync to the host on any change (FR13).
- **Sound effect retrieval.** - The DJ searches online royalty-free sound libraries with a text query, previews results, and downloads matches straight to the soundboard (UR13–UR14, FR15–FR16, IR10, ESR8).
- **Agentic sound extraction.** - The DJ supplies a longer audio file and describes a quote/effect in natural language; a multi-agent AI workflow locates and extracts the fragment, notifies the DJ, and lets them preview and accept or discard it (UR15–UR17, FR17–FR22, IR11, ESR5–ESR7).
- **Spotify integration.** - The host controls the laptop's Spotify playback volume via the Spotify Web API, automatically ducking the music while an effect plays and restoring it afterward; the DJ can enable/disable ducking and configure the reduction level (UR18–UR20, FR23–FR27, IR8, DC4, ESR1–ESR2).
- **Connection, authentication, and cross-platform.** - Controller and host establish a persistent, auto-reconnecting connection and surface connection status; the host rejects unauthenticated commands (UR21–UR22, FR28–FR31, SR1). The DJ signs in with Google before accessing the soundboard, with a persisted session (UR23, FR35–FR36, SR3).

## Hard constraints
The Remote Soundboard system has the following hard constraints, check any change against them:
- **EU AI services.** - Use European-developed/hosted AI wherever feasible (BR11, DC3). Mistral is the chosen LLM for this reason — do not swap in a non-EU provider without flagging the conflict.
- **No shared/bundled AI key.** - Each user configures their own key (DC9, FR22). Never commit a key or ship a default one.
- **Cost caps.** - A single agentic sound extraction request must cost ≤ €0.50 in AI API calls (BR5, PR4); total operational cost ≤ €100/yr (BR13, PR5). Avoid designs that imply unbudgeted paid services (DC7).
- **Latency.** - Trigger-to-audio ≤ 1000 ms (BR4, PR1); UI ack of play/stop ≤ 200 ms (PR2).
- **Cross-platform.** - Controller: Android, iOS, Windows, macOS (DC1). Host: Windows, macOS (DC2).
- **Secrets.** - API keys/credentials stored securely on-device, never shown in UI or sent in plaintext (SR2).
- **Non-technical users.** - Operable without technical expertise; user-facing text and errors in plain, non-technical language (BR6, NFR1, NFR3).
- **Audio format.** - Host must support at least MP3 (FR7).

## Documentation
You can find the documentation for the Remote Soundboard system in the `docs/` folder.
The following documentation is written for the project:
- [docs/system_requirements_specification.md](docs/system_requirements_specification.md) - This file contains a complete description of the system, along with its complete set of requirements. 
- [docs/frameworks.md](docs/frameworks.md) - This file contains the selected frameworks and tech stack for the project.

When the code differs from the documentation, treat it as a bug.

## Agents
## Agent setup
For this project, the **lead agent** (from the main Claude Code session) acts as both the **software engineer** and the **agent orchestrator**.
This agent plans and implements changes directly in the project repo, and delegates specialized work to specific subagents.
The following subagents are available for this project:
- **regulatory-compliance-agent** - This agent is read-only and performs compliance assessments and audits, and answers compliance-related questions.
- **security-agent** - This agent is read-only and performs threat modeling and security audits, and answers security-related questions.
- **environment-configuration-agent** - This agent performs all environment, dependency and lockfile related tasks, and answers environment-related questions.
- **implementation-review-agent** - This agent is read-only and performs architecture and code reviews, outputting only a report of its findings.
- **testing-agent** - This agent writes, runs and reviews the tests for the Remote Soundboard system, and answers testing-related questions.

The lead agent obtains reports from all the subagents and is responsible for the final output.

### Default workflow
When developing a new feature, the lead agent follows the following default workflow (adapt as the task requires):
1. **Plan.** - The lead agent plans the design for the new feature.
2. **Assess the plan.** - In parallel, the lead agent asks the:
   - **regulatory-compliance-agent** to perform a compliance assessment of the plan.
   - **security-agent** to perform threat modeling for the plan.
   - **implementation-review-agent** to review the design plan.
3. **Revise the plan.** - The lead agent reviews the reports and adjusts the plan to address their findings.
4. **Prepare the environment.** - If needed, the lead agent invokes the **environment-configuration-agent** to prepare the environment for the plan.
5. **Implement.** - The lead agent implements the plan.
6. **Audit the code.** - In parallel, the lead agent asks the:
   - **regulatory-compliance-agent** to perform a compliance audit of the implementation.
   - **security-agent** to perform a security audit of the implementation.
   - **implementation-review-agent** to review the code.
7. **Revise the implementation.** - The lead agent reviews the reports and adjusts the implementation to address their findings.
8. **Test.** - The lead agent asks the **testing-agent** to write and perform unit, integration and end-to-end tests for the feature.

## Conventions
### General
- **Writing language.** - The writing language for this project is American English.
- **Traceability.** - Requirements are traceable by ID (BR/UR/FR/NFR/IR/PR/SR/DC/ESR/QAR/DR). When referring to a requirement, reference the relevant requirement IDs for traceability.
- **Documentation sync.** - Keep the documentation files in the `docs/` folder as well as `CLAUDE.md` in sync with the current implementation.

### Coding
- **Language & version.** - Backend is Python. Target the version pinned in `pyproject.toml`; do not use syntax newer than that.
- **Environment configuration** - The `uv` virtual environment is only configured through the environment-configuration-agent, this includes installing dependencies. Any other agent (including the lead agent) is not allowed to configure the environment. When environment configuration is necessary, the lead agent delegates this task to the environment-configuration-agent.
- **Style & formatting.** - Follow PEP 8. Use type hints on all public functions, methods, and module-level constants. Keep formatting tool-driven (don't hand-format) and don't reformat unrelated code in a change.
- **Naming.** - `snake_case` for functions, methods, variables, and modules; `PascalCase` for classes and Pydantic models; `UPPER_SNAKE_CASE` for constants. Names are descriptive and in American English.
- **Data models & validation.** - Model all protocol messages (IR9), settings, and configuration as **Pydantic** models — validate at the boundary rather than passing around raw dicts. Reuse one shared set of protocol models across controller and host so both ends stay in sync.
- **API & async.** - Define HTTP/WebSocket endpoints with **FastAPI**. Use `async def` for I/O-bound paths (network, audio host commands, AI calls); never block the event loop with synchronous I/O or CPU-heavy work — offload that to a worker/thread. Respect the latency caps (PR1, PR2) on the trigger and play/stop paths.
- **Agentic workflow.** - Implement the multi-agent extraction flow with **LangGraph/LangChain** (FR18) and expose agent tools as **MCP** servers. Route all LLM calls through the configured **Mistral** provider — do not introduce a non-EU model provider (BR11, DC3). Keep **Langsmith** tracing/cost accounting on the extraction path so the €0.50 per-request cap (PR4) stays measurable.
- **Secrets & config.** - Never hard-code or commit API keys, credentials, or a default/shared key (DC9, FR22, SR2). Read secrets and per-user config from secure on-device storage / environment, never from source, and never log them.
- **Errors & user-facing text.** - Raise specific exceptions and handle them at the boundary. User-facing messages and errors are in plain, non-technical language (NFR1, NFR3); keep technical detail in logs only.
- **Persistence.** - Access PostgreSQL through parameterized queries / the chosen data layer — never build SQL with string interpolation. Keep schema changes captured as migrations.
- **Traceability.** - When code implements a requirement, reference the relevant requirement ID(s) in a comment or docstring, consistent with the General conventions.
- **Tests.** - New behavior ships with **pytest** tests (unit/integration/e2e per QAR1–QAR3); test authoring and runs are the **testing-agent**'s responsibility. Don't mark a feature done until its tests pass.
- **Comments.** - Comment the *why*, not the *what*. Match the comment density and idiom of surrounding code.