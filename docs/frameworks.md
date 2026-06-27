# Core Frameworks
The core frameworks are selected based on the SRS for the Remote Soundboard system. When a framework is required by one or more requirements, the specific relevant requirement(s) are listed.

## Core frameworks
- **FastAPI** — Backbone of the system. Serves the controller API and the WebSocket channel for commands, status updates, and sound-effect file sync (FR28–FR31, IR9), and runs as the local HTTP/WebSocket server inside the audio host. Used on both ends.
- **Pydantic** — Ships with FastAPI. Models the application-level protocol messages (IR9), settings, and the API-key / Spotify configuration (FR22, SR2).
- **LangGraph** — Implements the required multi-agent AI workflow (FR18): parse the natural-language description, locate the requested quote or sound effect within the supplied audio file, extract and validate the fragment, return it for preview. The core AI component.
- **LangChain** — Tool and integration layer beneath LangGraph (LLM wrappers, audio-processing tools, output parsing).
- **Mistral API** — The agent's model. Mistral is a European provider, satisfying BR11 / FR21; each user supplies their own key (FR22, DC9).
- **MCP (Model Context Protocol)** — Exposes the agent's tools (audio analysis/segmentation, extraction/validation) as standardized MCP servers.
- **Langsmith** — Tracing for the agentic workflow plus token/cost accounting, directly supporting the €0.50 per-extraction AI cost cap (PR4).
- **PostgreSQL + SQL** — Persistence of sound-effect metadata, color codes, groups, and configuration across sessions (FR14, UR10, UR11–12). Hosted on Azure Database for PostgreSQL.
- **Docker** — Containerizes the backend for deployment to Azure.
- **pytest** — Automated unit, integration, and end-to-end tests (QAR1–QAR3).
- **pip-audit** — Dependency vulnerability scanner (PyPA, OSV / PyPI Advisory Database). Audits resolved dependencies for known vulnerabilities. Free and open source with no account required, keeping within budget (BR13, DC7). Run via uv against an exported requirements list.
- **Azure AI Foundry** — Azure AI surface: deploy/serve the model from an EU region (BR11) and host the agentic backend.
- **uv** — Python package and project manager. Manages the virtual environment, dependencies, and lockfile (`uv.lock`). Replaces pip/venv. Used locally for development and inside Docker for reproducible builds (`uv sync --frozen --no-dev`).
- **Git + GitHub Actions** — Version control and CI for build, test, and deploy to Azure.

## Required integrations
- **Online royalty-free sound libraries** — Search for and download sound effects directly to the controller for the sound effect retrieval feature (FR15–FR16, IR10). Must be royalty-free and accessible at no cost (ESR8, BR13, DC7).
- **Spotify Web API** — Read and set Spotify playback volume from the audio host, including automatic volume reduction during sound-effect playback (FR23–FR27, IR8). Requires an active Spotify Premium subscription on the laptop (DC4, ESR2).
- **Google OAuth 2.0 (Google Sign-In)** — Sole authentication mechanism for the soundboard remote controller; required before any soundboard functionality is accessible, with a persistent session (FR35–FR36, SR3, DC8).