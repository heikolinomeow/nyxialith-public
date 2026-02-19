# Changelog
All notable changes to this project will be documented in this file.
The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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
