# Changelog
All notable changes to this project will be documented in this file.
The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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
