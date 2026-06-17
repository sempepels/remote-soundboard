# Core Frameworks

The core frameworks are selected based on the SRS for the Remote Soundboard system. When the a framework is required by one or more requirements, the specific relevant requirements(s) are listed.

## Core frameworks

- **FastAPI** — Backbone of the system. Serves the controller API and the WebSocket channel for commands, status updates, and sound-effect file sync (FR28–FR31, IR9), and runs as the local HTTP/WebSocket server inside the audio host. Used on both ends.
- **Pydantic** — Ships with FastAPI. Models the application-level protocol messages (IR9), settings, and the API-key / Spotify configuration (FR22, SR2).
- **LangGraph** — Implements the required multi-agent AI workflow (FR16): parse the natural-language request, search the internet for a matching sound, fetch and validate the audio, return it for preview. The core AI component.
- **LangChain** — Tool and integration layer beneath LangGraph (LLM wrappers, web-search tool, output parsing).
- **Mistral API** — The agent's model. Mistral is a European provider, satisfying BR11 / FR21; each user supplies their own key (FR22, DC9).
- **MCP (Model Context Protocol)** — Exposes the agent's tools (internet search, audio download/validation) as standardized MCP servers.
- **Langsmith** — Tracing for the agentic workflow plus token/cost accounting, directly supporting the €0.50 per-search AI cost cap (PR4).
- **PostgreSQL + SQL** — Persistence of sound-effect metadata, colour codes, groups, and configuration across sessions (FR14, UR10, UR11–12). Hosted on Azure Database for PostgreSQL.
- **Docker** — Containerizes the backend for deployment to Azure.
- **pytest** — Automated unit, integration, and end-to-end tests (QAR1–QAR3).
- **Azure AI Foundry** — Azure AI surface: deploy/serve the model from an EU region (BR11) and host the agentic backend.
- **uv** — Python package and project manager. Manages the virtual environment, dependencies, and lockfile (`uv.lock`). Replaces pip/venv. Used locally for development and inside Docker for reproducible builds (`uv sync --frozen --no-dev`).
- **Git + GitHub Actions** — Version control and CI for build, test, and deploy to Azure.

## Required integrations

- **Spotify Web API** — Read and set Spotify playback volume from the audio host, including automatic volume reduction during sound-effect playback (FR23–FR27, IR8). Requires an active Spotify Premium subscription on the laptop (DC4, ESR2).
- **Google OAuth 2.0 (Google Sign-In)** — Sole authentication mechanism for the soundboard remote controller; required before any soundboard functionality is accessible, with a persistent session (FR35–FR36, SR3, DC8).