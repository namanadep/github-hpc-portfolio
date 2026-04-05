# GitHub HPC portfolio — AMD Instinct / ROCm first

This index orders repositories by relevance to **AMD GPU software, performance,
and AI systems** roles. Consolidate and pin in this order.

---

## Pins (GitHub UI — profile → Customize your pins)

Pin exactly these six, in order:

| Pin | Repository | One-line signal |
|-----|------------|-----------------|
| 1 | [mi300x-amd-rocm-validation](https://github.com/namanadep/mi300x-amd-rocm-validation) | ROCm perf depth — rocprof, RCCL, HBM stress, single-VF before/after |
| 2 | [amd-enterprise-ai-platform](https://github.com/namanadep/amd-enterprise-ai-platform) | Full AMD Enterprise AI Suite deployment on K8s — every failure documented |
| 3 | [llm-inference-observability](https://github.com/namanadep/llm-inference-observability) | Qwen3-32B on MI300X VF — SLO table, VRAM, throughput sweep, ROCm case study |
| 4 | [amd-rocm-gpu-ops](https://github.com/namanadep/amd-rocm-gpu-ops) | Ops health check script + K8s GPU Operator runbook |
| 5 | [multi-node-nccl-p2p-benchmarks](https://github.com/namanadep/multi-node-nccl-p2p-benchmarks) | H200 NCCL + RoCE investigation + RCCL-on-Instinct companion doc |
| 6 | [amd-ai-workbench-ops](https://github.com/namanadep/amd-ai-workbench-ops) | AI Workbench install, user guide, model deployment |

**Unpin or demote:** `h200-gpu-benchmark-suite`, `mig-performance-lab`,
`cuda-pytorch-optimization-benchmarks` — fine as background breadth,
weak as AMD primary signal.

---

## Tier 1 — AMD Instinct / ROCm (primary narrative)

### `mi300x-amd-rocm-validation`
ROCm performance and production-readiness validation.

- **8-GPU node:** RCCL all-reduce bandwidth, 1.29 TB HBM stress, thermal at 750 W/GPU, amdgpu Prometheus exporter
- **Single VF:** `docs/single-vf-rocprof-analysis.md` — torch.profiler trace, rocBLAS kernel identification, FP32→FP16 4.20× speedup, roofline context, VF vs bare-metal caveats

### `amd-enterprise-ai-platform`
Field guide for AMD Enterprise AI Suite on bare metal.

- Bloom CLI → RKE2 → ClusterForge → AIRM — documented March 2026 deployment
- Documented failures: `ndots:5` DNS, Argo CD CRD sync, Canal CNI image, GPU Operator device plugin
- K8s: RKE2 v1.34.1, AMD GPU Operator v1.4.1, `amd.com/gpu: 1`

### `llm-inference-observability`
Inference performance harness — now includes AMD ROCm results.

- **AMD:** Qwen3-32B on MI300X VF — TTFT p50 62 ms, 296 tok/s @ conc=8, zero OOM, SLO table — `results/mi300x-vf-qwen3-32b-rocm641.md`
- **Case study:** `docs/amd-rocm-case-study.md` — hypothesis → measurement → findings → what failed
- **NVIDIA:** Ollama 704 tok/s on H200 NVL (existing)

### `amd-rocm-gpu-ops`  ← **NEW**
Operational tooling for AMD Instinct + K8s nodes.

- `scripts/amd-gpu-health-check.sh` — ROCm + K8s pass/fail checks, BOM output
- `runbooks/rocm-k8s-validation-runbook.md` — step-by-step from first boot to validated inference
- `results/sample-output.md` — sanitized real run output

### `amd-ai-workbench-ops`
AMD AI Workbench operations — install checklist, user guide, model deployment.

---

## Tier 2 — Multi-GPU / scaling (AMD + NVIDIA)

### `multi-node-nccl-p2p-benchmarks`
Real 2-node × 8 H200 cluster NCCL measurements + RoCE failure investigation.

- **NEW:** `docs/RCCL-on-instinct.md` — RCCL vs NCCL API parity, XGMI vs NVLink, VF constraints, single-rank init on 1 GPU, what to measure with 2+ GPUs

---

## Tier 3 — NVIDIA breadth (supporting evidence)

| Repo | Content |
|------|---------|
| `h200-gpu-benchmark-suite` | Single-node H200 GEMM + HBM saturation |
| `cuda-pytorch-optimization-benchmarks` | CUDA kernel fusion, PyTorch optimisation |
| `hpc-gpu-observability-lab` | DCGM/Prometheus dual-vendor observability |
| `mig-performance-lab` | MIG profiles, isolation vs throughput |
| `slurm-job-analyzer` | `sacct` queue analytics |
| `storage-ai-fio-benchmarks` | FIO storage benchmarks for AI workloads |

These are breadth. If AMD is the primary employer target, do not lead with these.

---

## Profile bio (use in GitHub About + LinkedIn headline)

> GPU infrastructure and performance engineering — AMD Instinct / ROCm, NVIDIA CUDA,
> Kubernetes GPU operator, LLM inference. I measure things on real hardware.

---

## What to do next (from feedback)

- [ ] Pin the 6 repos above in GitHub UI (cannot be set via API)
- [ ] Push `amd-rocm-gpu-ops` as new public repo
- [ ] Add `docs/single-vf-rocprof-analysis.md` to `mi300x-amd-rocm-validation`
- [ ] Add `results/mi300x-vf-qwen3-32b-rocm641.md` + `docs/amd-rocm-case-study.md` to `llm-inference-observability`
- [ ] Add `docs/RCCL-on-instinct.md` to `multi-node-nccl-p2p-benchmarks`
- [ ] Update `namanadep/namanadep` profile README
- [ ] Pick one upstream project (vLLM ROCm / PyTorch ROCm / docs) and open first PR
