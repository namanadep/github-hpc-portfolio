# GitHub HPC portfolio — publish kit

This folder contains **ready-to-publish** repositories and a **profile README** aligned with:

- Your **Medium_Ready_Articles** series (H200 benchmarks, NVLink/NCCL, storage, OOM, NVIDIA career content).
- The portfolio feedback: **HPC-first pins**, engineering layout (CLI, configs, tests, results), reproducible benchmarks, and a clear **GPU infrastructure / performance engineering** narrative.

## What to do on GitHub

1. **Create a profile README** (special repo `your-username/your-username`): copy `profile/README.md` into that repo’s `README.md`. Adjust links and metrics to match what you are comfortable sharing publicly.

2. **Create six new public repositories** (names below). Copy each subfolder’s contents to the repo root (not the subfolder name nested twice).

3. **Pin these six** on your profile (order is a suggestion):

   | Pin # | Repository | One-line signal |
   |-------|------------|-----------------|
   | 1 | `h200-gpu-benchmark-suite` | Single-node GEMM + HBM saturation, methodology, tables |
   | 2 | `multi-node-nccl-p2p-benchmarks` | P2P / interconnect measurement and CSV export |
   | 3 | `hpc-gpu-observability-lab` | DCGM/Prometheus-style telemetry + dashboards |
   | 4 | `mig-performance-lab` | MIG profiles, isolation vs throughput tradeoffs |
   | 5 | `llm-inference-observability` | Token throughput, batch/TP sweep, structured runs |
   | 6 | `slurm-job-analyzer` | Scheduler / queue analytics from `sacct` exports |

4. **Fix trust issues**: Unpin or archive repos that 404 or are abandoned. Replace any blog/README link that pointed at `w4_regression_demo` with a live repo or remove the link.

5. **Cross-post**: For each repo, add a short LinkedIn post + Medium/dev.to article pointing to the README (matches your `00_PUBLISHING_SCHEDULE.md` LinkedIn workflow).

## Article ↔ repo mapping

| Medium article (workspace) | Related repo |
|----------------------------|--------------|
| `01_H200_Benchmark_Complete_Guide.md` | `h200-gpu-benchmark-suite` |
| `03_NVLink_How_GPUs_Talk.md`, `10_Distributed_Training_NCCL_Guide.md` | `multi-node-nccl-p2p-benchmarks` |
| `12_GPU_Memory_OOM_Guide.md` | `llm-inference-observability` (memory hooks section) |
| `05_Storage_VAST_vs_NetApp.md` | Optional future `storage-fio-ai-workloads` (not in this kit) |

## Positioning line (use in bio + profile README)

**GPU infrastructure and performance engineering** — reproducible benchmarking, multi-GPU communication, observability (DCGM/Prometheus), and scheduler-aware analysis on large NVIDIA GPU systems (e.g. Hopper/H200-class).

## Folder layout

```
github_hpc_portfolio/
├── README.md                 ← this file
├── profile/README.md         ← GitHub profile README source
├── h200-gpu-benchmark-suite/
├── multi-node-nccl-p2p-benchmarks/
├── hpc-gpu-observability-lab/
├── mig-performance-lab/
├── llm-inference-observability/
└── slurm-job-analyzer/
```

Each project includes `README.md`, `pyproject.toml`, `src/`, `configs/`, `tests/`, `results/` (samples), and `Dockerfile` or compose where relevant.
