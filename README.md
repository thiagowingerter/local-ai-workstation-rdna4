# Local AI Engineering Workstation — AMD RDNA4 & Multi-Agent Architecture

> **Production-grade local AI infrastructure** built on Windows 11 & AMD Radeon RX 9070 XT (16 GB VRAM), orchestrating dense multimodal 27B LLMs, CPU-offloaded FIM/Embeddings, autonomous coding agents (Cline/Continue), and AI-assisted binary decompilation (`GhidraMCP` + `LLM4Decompile` + `decomp.me`).

---

## 1. Architecture Overview (Release v0.1.0)

This project documents the iterative engineering of an air-gapped-capable, zero-token-cost local AI workstation designed for autonomous software engineering, multimodal QA, and binary reverse engineering.

### Core Engineering Principles
1. **Single OpenAI-Compatible Gateway (`127.0.0.1:8080`):** Powered by `llama-swap` and `llama.cpp` (Vulkan backend), dynamically loading and swapping specialized models (`Qwen3.8-27B` for engineering/vision and `LLM4Decompile` for binary analysis) via PCIe Gen4 NVMe storage.
2. **Zero VRAM Contention for IDE Background Tasks:** Code autocomplete (`qwen2.5-coder:1.5b-base`) and repository embeddings (`nomic-embed-text`) run strictly on CPU (`Ryzen 7 7700`) via Ollama (`num_gpu 0`), preserving 100% of the 16 GB VRAM for the primary reasoning model.
3. **Model Context Protocol (MCP) First:** All external capabilities (local web search via `SearXNG`, static binary analysis via `GhidraMCP`, sandboxed code execution via `Open Terminal`, and hybrid memory via `Qdrant` + `SQLite WAL`) are decoupled as modular MCP servers.

---

## 2. Reference Hardware & Validated Telemetry (Baseline F0)

### Host Specifications
* **CPU:** AMD Ryzen™ 7 7700 (8 Cores / 16 Threads, Zen 4 AM5, +0.15 GHz Boost Offset)
* **Motherboard:** Asus Prime X670E Pro WIFI
* **GPU:** XFX Swift AMD Radeon™ RX 9070 XT 16 GB (RDNA 4 / `gfx1201`)
  * *Telemetry Tuning:* `-35 mV` Voltage Offset | `+10%` Power Limit | `2614 MHz` Max Frequency | AMD SmartAccess Memory (ReBAR) `ON`
* **System RAM:** 32 GB DDR5 Dual-Channel (5600 MHz)
* **Storage:** 2 TB NVMe PCIe Gen4 (1 TB Samsung 980 PRO + 1 TB XPG S70)
* **OS & Drivers:** Windows 11 Pro 64-bit | AMD Adrenalin Edition 26.10.2 | Docker Desktop 4.99.0 (WSL2 capped at 8 GB RAM)

### Real-World Benchmarks (`llama.cpp` v0.6.0 — Vulkan Backend)

| Metric | Measured Value | Technical Notes |
| :--- | :--- | :--- |
| **Primary Model** | `Qwen3.8-27B` (Dense Multimodal + Vision `mmproj-F16`) | Hybrid architecture (1/4 Full Attention + 3/4 Gated DeltaNet) |
| **Quantization** | `IQ3_M` (11.72 GB weights) | 100% GPU offload (`-ngl 99`), Vision projector kept on CPU (`--no-mmproj-offload`) |
| **Context Window** | `32,768 tokens` | Flash Attention (`-fa on`) + 8-bit KV Cache (`-ctk q8_0 -ctv q8_0`) |
| **Token Generation (`tg`)** | **36.8 tokens/s** | Stable across long multi-file coding sessions |
| **Prompt Processing (`pp`)** | **~870 tokens/s** | ~15,000 tokens ingested in ~17 seconds |
| **Dedicated VRAM Usage** | **13.3 GB** (`llama.exe`) | ~15.5 GB total system VRAM footprint with Windows 11 |

---

## 3. Hardware Sizing Guide (Recommendations Across Configurations)

Based on empirical memory-bandwidth and KV-cache profiling, the table below provides recommended configurations for smaller and larger hardware setups:

| Hardware Tier | VRAM / System RAM | Recommended Primary Model & Quant | FIM & Embeddings Strategy | Context & KV Flags | Target Workload |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Tier 1: Entry** | **8 GB VRAM** / 16 GB RAM | Dense `7B–8B` (`Qwen2.5-Coder-7B` or `Llama-3.1-8B`) @ `Q4_K_M` (~4.8 GB) | CPU FIM (`1.5B`) + CPU `nomic-embed-text` | `-c 16384 -fa on -ctk q8_0 -ctv q8_0` | Fast IDE chat, single-file edits, local RAG (`@codebase`). |
| **Tier 2: Mid-Range** | **12 GB VRAM** / 32 GB RAM | Dense `12B–14B` (`Qwen2.5-Coder-14B`) @ `Q4_K_M` (~8.5 GB) + `LLM4Decompile-6.7B` | CPU FIM (`1.5B`) + CPU `nomic-embed-text` | `-c 16384` to `32768` (`-fa on`, KV `q8_0`) | Multi-file refactoring and static binary decompilation. |
| **Tier 3: Sweet Spot (Ref)** | **16 GB VRAM** / 32 GB DDR5 | **Dense `27B` (`Qwen3.8-27B` @ `IQ3_M` / `IQ4_XS`)** or **MoE `35B-A3B` (`Q4_K_M`)** | CPU FIM (`1.5B`) + CPU `nomic-embed-text` | **`-c 32768 -fa on -ctk q8_0 -ctv q8_0 --no-reasoning-preserve`** | Full Plan/Act autonomous coding (`Cline`), Vision QA, and `GhidraMCP` + `LLM4Decompile-9B`. |
| **Tier 4: High-RAM / Pro** | **24 GB+ VRAM** or **64–96 GB DDR5** | **MoE `80B-A3B` (`Qwen3-Coder-Next` @ `Q4_K_M` w/ `--n-cpu-moe`)** or Dense `32B–70B` | Dedicated GPU/CPU split (`7B` FIM) | `-c 65536 -fa on -ctk q8_0 -ctv q8_0` | Frontier-grade repository architecture and parallel multi-agent execution. |

---

## 4. Release Roadmap

- [x] **v0.1.0 (Current):** Core Vulkan Inference Engine (`llama.cpp` + `llama-swap`), CPU-offloaded FIM/Embeddings (`Ollama`), and IDE Autonomous Agent (`Cline` + `Continue`) validated with `pytest`.
- [ ] **v0.2.0 (In Progress):** Agent Tooling & Reverse Engineering Stack — WSL2 memory governance (`.wslconfig`), local web search (`SearXNG` Docker + Web MCP), sandboxed `Open Terminal`, and Decompilation Lab (`GhidraMCP` + `LLM4Decompile` + `decomp.me`).
- [ ] **v0.3.0 (Planned):** Polyglot Cognitive Memory (`trinity-mcp v2` combining `Qdrant` Hybrid Search, `SQLite` Bitemporal Event Graph, and Offline Sleep Consolidation).
- [ ] **v0.4.0 (Planned):** Interactive `Local AI Hardware & VRAM Sizing Calculator` web application (HTML + Alpine.js on GitHub Pages) & 2.5D Visual Pipeline (`comfy-mcp`).