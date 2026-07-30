# Naman Adep: GPU / HPC Portfolio

> GPU infrastructure and performance engineering: AMD Instinct / ROCm, NVIDIA CUDA, Kubernetes GPU Operator, LLM inference. Measured on real hardware.

---

## Quick navigation

| Area | Jump to |
|------|---------|
| AMD Instinct / ROCm | [AMD GPU Work](#-amd-instinct--rocm) |
| NVIDIA H200 | [NVIDIA GPU Work](#-nvidia-h200-nvl--sxm) |
| Observability & Monitoring | [Observability](#-observability--monitoring) |
| Scheduling & Cluster Ops | [Cluster Ops](#-scheduling--cluster-operations) |
| LLM Inference & AI | [LLM / AI](#-llm-inference--ai-on-hpc) |
| User Guides | [Guides](#-user-guides--visual-documentation) |

---

## AMD Instinct / ROCm

### [mi300x-amd-rocm-validation](https://github.com/namanadep/mi300x-amd-rocm-validation)
**Full production-readiness validation suite for 8× AMD Instinct MI300X GPUs.**

Run March 19, 2026, on a bare 8-GPU node. Covers ROCm stack bring-up, RCCL collective bandwidth, 1.29 TB memory stress, peak-load thermal, and the AMD GPU Prometheus exporter.

| Metric | Result |
|--------|--------|
| Peak FP32 TFLOPS (single GPU, 8192×8192) | **~94.5 TFLOPS** |
| RCCL AllReduce bus bandwidth (8 GPUs, 1 GB) | **~3,468 GB/s** |
| Total VRAM stressed (8 GPUs) | **~1.29 TB** |
| Peak junction temp | 86°C (0 throttle events) |
| Peak power | 750 W / GPU |

Stack: ROCm 6.4.1 · RCCL 2.22.3 · PyTorch 2.4.1+rocm6.0 · Ubuntu 24.04.2

---

### [rag-amd-mi300x-benchmark](https://github.com/namanadep/rag-amd-mi300x-benchmark)
**Head-to-head benchmark: Vectorless RAG (BM25) vs Vector RAG (dense embeddings) on a single AMD Instinct MI300X VF.**

Measures retrieval latency, VRAM overhead, index build time, and ROUGE-L answer quality on the same corpus and LLM.

| Metric | BM25 | Dense | Delta |
|--------|------|-------|-------|
| Index build | **8.3 ms** | 2,847 ms | BM25 340× faster |
| Retrieval p50 | **1.8 ms** | 47.3 ms | BM25 26× faster |
| VRAM overhead | **0 MB** | ~130 MB | — |
| ROUGE-L quality | 0.3241 | **0.3587** | Dense +10% |

Hardware: MI300X VF · 191.69 GiB HBM3 · ROCm 6.4.1 · Qwen3-32B via vLLM

---

### [amd-rocm-gpu-ops](https://github.com/namanadep/amd-rocm-gpu-ops)
**Operational health checks and Kubernetes GPU Operator runbook for AMD Instinct / ROCm nodes.**

- `scripts/amd-gpu-health-check.sh`: ROCm + K8s pass/fail checks with BOM output
- `runbooks/rocm-k8s-validation-runbook.md`: step-by-step from first boot to validated inference
- `results/sample-output.md`: sanitised real run output

---

### [amd-enterprise-ai-platform](https://github.com/namanadep/amd-enterprise-ai-platform)
**Field guide for deploying AMD Enterprise AI Suite on bare metal (March 2026).**

Bloom CLI → RKE2 → ClusterForge → AIRM pipeline documented end-to-end, including real failures: `ndots:5` DNS, Argo CD CRD sync, Canal CNI image pull, GPU Operator device plugin issues.

Stack: RKE2 v1.34.1 · AMD GPU Operator v1.4.1 · K8s `amd.com/gpu: 1`

---

### [amd-ai-workbench-ops](https://github.com/namanadep/amd-ai-workbench-ops)
**AMD AI Workbench operations: install checklist, user guide, model deployment runbook.**

---

### [amd-ai-workbench-user-guide](https://github.com/namanadep/amd-ai-workbench-user-guide)
Screenshot-based visual guide for AMD AI Workbench: API keys, chat/comparison modes, fine-tuning, model catalog, ComfyUI / MLflow / JupyterLab / VS Code workspaces.

---

## NVIDIA H200 NVL / SXM

### [h200-gpu-benchmark-suite](https://github.com/namanadep/h200-gpu-benchmark-suite)
**Reproducible FP32 GEMM and HBM bandwidth benchmarks on NVIDIA H200 (NVL and SXM variants).**

Measured on real production nodes in India, March–April 2026.

| SKU | FP32 TFLOPS | HBM BW | Peak Power |
|-----|-------------|--------|------------|
| H200 NVL | **~46.5 TFLOPS** | — | 548 W |
| H200 SXM | **~51 TFLOPS** | **2,725 GB/s (91%)** | 600 W |
| FP16 (Tensor Cores) | **728,286 GFLOPS** | — | 14.38× vs FP32 |

CLI tool with configs, Docker support, and CSV result output.

---

### [multi-node-nccl-p2p-benchmarks](https://github.com/namanadep/multi-node-nccl-p2p-benchmarks)
**Measured GPU-to-GPU collective bandwidth on a real 2-node × 8 H200 cluster.**

Includes intra-node NVLink (~781 GB/s per GPU pair), cross-node TCP AllReduce (~2.28 GB/s), and a documented RoCE v2 failure investigation with root-cause analysis (switch PFC/ECN/DCQCN misconfiguration, not the host NICs).

| Path | Bandwidth | Status |
|------|-----------|--------|
| NVLink intra-node (any pair) | **~781 GB/s** | PASS |
| TCP cross-node AllReduce (16 GPUs) | **~2.28 GB/s** | PASS |
| RoCE v2 cross-node | — | FAIL → root cause documented |

---

### [mig-performance-lab](https://github.com/namanadep/mig-performance-lab)
**Benchmarks NVIDIA MIG profile tradeoffs (isolation, memory, and throughput) on A100/H100-class GPUs.**

CLI commands (`mig-lab status`, `mig-lab describe-profiles`) for non-destructive topology analysis. Structured configs and repeatable nvidia-smi snapshots plus optional PyTorch micro-loads.

---

### [isaac-sim-pow](https://github.com/namanadep/isaac-sim-pow)
**NVIDIA Isaac Sim 6.0.0: proof of work on bare H200 NVL VM (pip install, SSH-only).**

Documents full install, headless `SimulationApp` lifecycle, WebRTC streaming TCP signaling confirmed. Root-causes the WebRTC UDP boundary over SSH and documents three concrete unblock paths (Tailscale, DNAT, Chisel).

---

### [cuda-pytorch-optimization-benchmarks](https://github.com/namanadep/cuda-pytorch-optimization-benchmarks)
**CUDA kernel fusion and PyTorch optimization benchmarks**, covering mixed precision, memory layout, and compute-bound vs memory-bound profiling patterns.

---

## Observability & Monitoring

### [gpu-obs-quickstart](https://github.com/namanadep/gpu-obs-quickstart)
**End-to-end GPU + host observability stack on a Linux machine with Docker.**

`DCGM exporter → Prometheus (recording & alert rules) → Alertmanager → Grafana` + `node_exporter` for CPU, memory, disk, and network. Dashboards and datasources are provisioned as code, not recreated by hand in the UI.

| Service | Port | Role |
|---------|------|------|
| dcgm-exporter | internal :9400 | GPU metrics |
| prometheus | **9091** | Scrape + alert rules |
| alertmanager | **9093** | Alert routing |
| grafana | **3002** | Dashboards |
| node-exporter | **9100** | Host metrics |

Validated on H200 NVL. Pairs with `isaac-sim-pow` for Physical AI workload monitoring.

---

### [hpc-gpu-observability-lab](https://github.com/namanadep/hpc-gpu-observability-lab)
**Dual-vendor (NVIDIA + AMD) GPU observability lab**: DCGM and amd-metrics-exporter on the same Prometheus / Grafana stack. Demonstrates drop-in compatibility between DCGM exporter and AMD GPU exporter (both on :9400).

---

### [llm-inference-observability](https://github.com/namanadep/llm-inference-observability)
**Inference performance harness for LLM servers (Ollama, vLLM) with structured result tracking.**

Real Ollama run: **704 tokens/s** on H200 NVL. AMD results for Qwen3-32B on MI300X VF: TTFT p50 62 ms, 296 tok/s @ conc=8, zero OOM, with full SLO table.

Records tokens/s, latency percentiles, batch size, and GPU telemetry into timestamped CSV/JSONL for regression tracking.

---

### [grafana-monitoring-user-guide](https://github.com/namanadep/grafana-monitoring-user-guide)
Visual walkthrough of Grafana dashboards for GPU monitoring: compute, memory, thermal, power, PCIe, ECC errors, plus alerting and notification policy setup. 13 screenshots.

---

## Scheduling & Cluster Operations

### [slurm-job-analyzer](https://github.com/namanadep/slurm-job-analyzer)
**Published PyPI package: parse Slurm `sacct` exports to identify GPU waste and queue analytics.**

```bash
pip install slurm-job-analyzer
slurm-analyze gpu-waste --input jobs.tsv --idle-threshold 20
```

Identifies EFFICIENT / UNDERUTILIZED / IDLE\_WASTE jobs across partitions. Works with NVIDIA and AMD clusters without cluster access: just a sacct TSV export.

---

### [slurm-interactive-demo](https://github.com/namanadep/slurm-interactive-demo)
**Fully browser-based interactive SLURM scheduler simulator**: partitions, queues, GRES, drain, and backfill. No cluster needed. Built with HTML/CSS/JS with a `TUTORIALS.md` guide.

---

### [kubernetes-gpu-cluster-user-guide](https://github.com/namanadep/kubernetes-gpu-cluster-user-guide)
Step-by-step visual guide for Kubernetes GPU cluster setup: node inspection, NVIDIA GPU Operator, Node Feature Discovery (NFD). 18+ screenshots.

---

### [kubernetes-pytorch-gpu-user-guide](https://github.com/namanadep/kubernetes-pytorch-gpu-user-guide)
PyTorch GPU pods on Kubernetes: deployment, port forwarding, benchmarking. 11 screenshots.

---

### [kubernetes-tensorflow-gpu-user-guide](https://github.com/namanadep/kubernetes-tensorflow-gpu-user-guide)
TensorFlow GPU pods on Kubernetes: Jupyter notebooks, Keras training, GPU verification. 14 screenshots.

---

### [storage-ai-fio-benchmarks](https://github.com/namanadep/storage-ai-fio-benchmarks)
**FIO storage benchmarks for AI workloads**: sequential read/write, random I/O, and mixed patterns relevant to checkpoint I/O and dataset loading on HPC clusters.

---

## LLM Inference & AI on HPC

### [indic-parler-tts-hpc](https://github.com/namanadep/indic-parler-tts-hpc)
**GPU-optimised inference scripts for AI4Bharat Indic Parler-TTS on HPC clusters.**

Hindi, Telugu, Marathi, Gujarati. Measured: ~40 short clips in 3.3 min on H200 NVL. Includes long-form chunked generation, Slurm job templates, and shared filesystem awareness.

---

### [indian-art-sd-exploration](https://github.com/namanadep/indian-art-sd-exploration)
**Stable Diffusion 1.5 fine-tuned for Indian art styles on H200 NVL**: Madhubani, Warli, Mughal miniatures, Tanjore, Pattachitra, rangoli. 50 images from curated prompts at ~1.2 s/image. Includes `HPC_PROOF_OF_WORK.md` and Slurm job templates.

---

### [esg_buddy](https://github.com/namanadep/esg_buddy)
**Full-stack agentic AI app for ESG compliance verification** (BRSR, GRI, SASB, TCFD).

PDF → semantic chunking → vector embeddings → chain-of-thought LLM → self-reflection → rule-based validation.

Stack: FastAPI · ChromaDB · OpenAI · React 18 · Tailwind CSS · Framer Motion · Vite

---

## User Guides & Visual Documentation

Screenshot-based guides from real production systems. All MIT licensed.

| Guide | Coverage | Screenshots |
|-------|----------|-------------|
| [nvidia-mig-gpu-partitioning-user-guide](https://github.com/namanadep/nvidia-mig-gpu-partitioning-user-guide) | MIG mode enable, 1G–7G profiles, PyTorch on isolated instances | 33 |
| [llm-fine-tuning-user-guide](https://github.com/namanadep/llm-fine-tuning-user-guide) | GPU verify → baseline eval → training → checkpoints → fine-tuned eval | 9 |
| [grafana-monitoring-user-guide](https://github.com/namanadep/grafana-monitoring-user-guide) | GPU dashboards, alerting, notification policies | 13 |
| [amd-ai-resource-manager-user-guide](https://github.com/namanadep/amd-ai-resource-manager-user-guide) | AMD AIRM dashboard, GPU utilization, project management, RBAC, S3 | 38 |
| [amd-ai-workbench-user-guide](https://github.com/namanadep/amd-ai-workbench-user-guide) | AMD AI Workbench: API keys, fine-tuning, ComfyUI, JupyterLab, VS Code | — |
| [ai-video-analysis-user-guide](https://github.com/namanadep/ai-video-analysis-user-guide) | FFmpeg + Ollama: scene detection, object detection, OCR, action recognition | 24 |
| [ai-image-generation-user-guide](https://github.com/namanadep/ai-image-generation-user-guide) | Stable Diffusion XL CLI: single & batch generation, GPU monitoring | 16 |
| [ai-image-analysis-user-guide](https://github.com/namanadep/ai-image-analysis-user-guide) | AI-based image analysis workflows | — |
| [ai-book-translation-user-guide](https://github.com/namanadep/ai-book-translation-user-guide) | AI book translation pipeline | — |
| [kubernetes-gpu-cluster-user-guide](https://github.com/namanadep/kubernetes-gpu-cluster-user-guide) | K8s GPU cluster, NVIDIA GPU Operator, NFD | 18 |
| [kubernetes-pytorch-gpu-user-guide](https://github.com/namanadep/kubernetes-pytorch-gpu-user-guide) | PyTorch GPU pods on K8s | 11 |
| [kubernetes-tensorflow-gpu-user-guide](https://github.com/namanadep/kubernetes-tensorflow-gpu-user-guide) | TensorFlow GPU pods on K8s, Keras | 14 |

---

## Explainers & Education

### [nvidia-explainers](https://github.com/namanadep/nvidia-explainers)
**Interactive HTML explainers for NVIDIA AI infrastructure**: DPU, DOCA, Slurm, PCIe, storage tuning, hardware validation, and GPU data center concepts.

---

## About

**Naman Adep**: GPU infrastructure and performance engineering.

Hands-on experience commissioning and operating NVIDIA H200 NVL/SXM and AMD Instinct MI300X clusters. Work spans acceptance testing, multi-GPU networking (NCCL/RCCL, NVLink, XGMI, RoCE v2), Kubernetes GPU Operator, observability (DCGM, amd-metrics-exporter, Prometheus, Grafana), and LLM inference (Ollama, vLLM, ROCm).

[![GitHub](https://img.shields.io/badge/GitHub-namanadep-181717?logo=github)](https://github.com/namanadep)
