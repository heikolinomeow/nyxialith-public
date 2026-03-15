# Changelog
All notable changes to this project will be documented in this file.
The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [v0.2.0] - 2026-03-15

### Added

- **Persistent Chat Sessions (SQLite Backend)**:
  - **Database Integration**: Migrated conversation history from ephemeral frontend state to a robust SQLite persistence layer (`sessions` & `session_messages`).
  - **Context-Aware Memory**: Implemented automatic session creation and message ingestion in the backend `RunManager`.
  - **Session Management**: Full lifecycle support for listing, selecting, and continuing prior conversations from the Consult sidebar.
  - **API Correction**: Standardized Axum route path parameters for consistent session message retrieval.

- **Consult UX: Logic Exchange Paradigm**:
  - **Visual Exchange Grouping**: Redesigned message rendering into "Exchanges" (User Input + Assistant Response) linked by vertical Logic Threads (dashed connectors).
  - **Temporal Masking**: Added a dynamic top-fade mask to the chat history to focus the operator's attention on the active conversation exchange.
  - **Command-Line Aesthetics**: Prefixed user prompts with `>>` and integrated high-fidelity timestamps for every message.
  - **Paragraph & Line-Height Polish**: Refined `terminalRenderer` logic to handle multi-paragraph responses with natural spacing and improved legibility.
  - **Stateful Interaction**: Added a global "Nyxialith OS" system prompt and improved auto-scrolling behavior for long responses.

- **Nyxialith OS: Active Session & Interface Overhaul**:
  - **Brand Initiation**: Formal introduction of Nyxialith OS. Updated window titles, metadata, and generated fresh application icons (`.ico`, `.icns`, `.png`) from core assets.
  - **Active Session Management**: Transitioned from a placeholder sidebar to a fully functional persistence layer.
    - Supports saving, listing, and loading chat sessions from the SQLite registry.
    - Integrated "New Chat" workflow with automatic session creation.
  - **Exchange-Based Chat UX**:
    - Implemented "Exchanges": Messages are visually paired with a "Logic Link" (dashed thread) connecting user prompts to responses.
    - Added high-precision timestamps for every message.
  - **High-Fidelity Interaction**:
    - **Directive Shift**: Standardized on `SHIFT + ENTER` for sending to accommodate native multiline input.
    - **Sliding Memory Window**: Implemented a 20-message buffer for context-aware inference.
    - **Logic Formatting**: Improved Markdown rendering with specific support for paragraphs and line breaks.

- **UI Redesign: Bottom Navigation Paradigm**:
  - **Global Layout Shift**: Replaced the legacy left sidebar with a streamlined Windows-style bottom navigation bar.
  - **Icon-Centric UX**: Navigational elements (Home, Consult, Admin) now prioritize symbolic icons for a cleaner, high-density interface. Dashboard and Models have been migrated to the Admin console.
  - **Brand-Driven Home Experience**: New default landing page featuring Nyxia branding with randomized welcome messaging and pulsing presence animations.
  - **Tiered Admin Experience**: Unified technical controls into a specialized Admin view.
    - Features a dedicated internal sidebar for Dashboard, Models, Logs, and Settings.
    - Refactored `switchView` logic to handle automatic routing between the main bottom nav and admin subviews.
  - **Consult UI Restructure**: Radically simplified the chat interface for deep focus.
    - Integrated a sessions sidebar and centered the main conversation flow.
    - Compact Top Bar displaying the active model status.
    - Auto-resizing text input limited to 80% viewport height.
  - **Responsive Layout Stability**: Migrated to a pure Flexbox architecture (`flex: 1`) to ensure views dynamically fill the viewport without overlapping navigation elements.

- **Empty-State Refinement**:
  - Intelligent status dot and label system that dims and shifts styles when no model is active.
  - Added Mono-type styling (JetBrains Mono) for technical standby states.
  - Updated status dot logic to grey-out during healthy idle states.

- **Early Verification**: Implemented immediate model registry scanning on UI initialization to ensure UI state parity with daemon runtime.

- **Animated Home Arrival**: New randomized brand greetings and refined branding animations on the Home landing page.

## [v0.1.1] - 2026-02-19

- **Advanced Sampling**: Implemented a comprehensive sampler chain including Repetition Penalty, Top-P, and Temperature to prevent loops and improve output quality.
- **Chat Templates**: Integrated `llama_chat_apply_template` to use model-native conversation formatting (GGUF-defined).
- **Streaming Reliability**: Added UTF-8 byte buffering to the inference stream to fix emoji rendering ("") issues.
- **CLI Enhancements**:
  - `nyx chat`: Standardized interactive chat command.
  - `nyx models list-remote`: Refined UI with hardware requirements, model sizes, and curated descriptions.
- **Configuration**: Added `max_queue` to `DaemonArgs` to control the backlog size (default: 32).
- **Registry Enhancements**: Added schema migration for `family_id` and `version` columns in `models` table to support versioning.
- **Sidecar Readiness**:
  - **Structured Logging**: Added optional JSON log output (`--log-json`) for machine parsing.
  - **Graceful Shutdown**: Enabled signal handling (`SIGTERM`/`CTRL_C`) for clean daemon termination.
  - **Custom Exit Codes**: Daemon now returns non-zero exit codes on failure.
  - **Doctor JSON**: `nyx doctor --json` for structured environment pre-validation.
- **Network & Security**:
  - **Host Binding**: Configurable bind address. Now requires `--allow-insecure-host` for non-loopback bindings.
  - **CORS**: Implemented permissive CORS for local UI development.
  - **Rate Limiting**: Added API rate limiting (default 10 req/s) via `--rate-limit-per-sec`.
- **Stability & Reliability**:
  - **Process Locking**: Implemented PID-based `nyx-app.lock` to prevent multiple daemon instances.
  - **App Directory**: Standardized application data directory to `Nyxialith` across platforms.
- **Active Resource Management (OOM Guard)**:
  - Added `min_free_ram_mb` argument to proactively refuse heavy operations if system memory is low.
- **Telemetry API**:
  - Added `GET /v1/system/telemetry` to retrieve a history of resource usage snapshots.
- **Database Resilience**:
  - Enabled **WAL Mode** for SQLite to improve concurrency and crash safety.
  - Added `PRAGMA integrity_check` on every daemon startup.
- **Deep Cancellation**:
  - Inference runtime now checks for cancellation signals during prompt processing (pre-fill phase).
- **Hardening Polish**:
  - Exempted `/v1/system/health` from authentication to allow readiness probes.
  - Improved `nyx doctor` with precise hardware detection and error reporting.

### Changed
- **Architecture**: Refactored `RunManager` to support state isolation and FIFO queueing.
- **Diagnostics**: Relaxed strict loopback validation to a warning to support broader network configurations.
- **CLI**: Standardized daemon subcommand and improved error reporting.

## [v0.1.0] - 2026-02-14
### Added
- **Core Runtime**: Native `llama.cpp` integration (v9 compatible) for high-performance inference on CPU/GPU.
- **Daemon Service**: Background service for model management and inference requests.
- **CLI**: `nyx` command-line tool for managing authentication, models, and runs.
- **Windows Support**: Full compatibility with Windows 10/11 x64.
- **Runtime Stability**: Built-in KV cache handling (`memory_seq_rm`) ensuring correct context clearing between sequential runs.
### Security
- **Authentication**: Confirmed token-gated access with loopback-only binding.
- **Access Control**: Token masking and unauthorized access blocking verified.
### Limitations
- **Concurrency**: The scheduler processes **one request at a time** (`max_queue=0`). Parallel requests return `429 Too Many Requests`.
- **Build Requirements**: Building from source requires `cmake` and recursive submodule cloning.
