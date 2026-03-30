# TOA vLLM / LLM-D Hackathon

**Date:** April 25, 2026 | **Location:** The Open Accelerator, Fortpoint, Boston

Materials for running a one-day hackathon focused on LLM inference with [vLLM](https://github.com/vllm-project/vllm) and [llm-d](https://github.com/llm-d/llm-d). Includes pre-configured GPU environments on NVIDIA Brev, setup scripts, benchmarking tools, and an attendee guide.

## What's in this repo

```
attendee-guide.md                        # Attendee-facing environment guide
hackathon-gpu-cost-estimate.xlsx         # GPU instance cost estimates by tier
launchable-configs/
  tier1-app-builder/                     # 1x L40S — RAG, apps, BYOP
    setup.sh                             #   Brev setup script (vLLM, LangChain, ChromaDB, Gradio)
    BREV_CONFIG.md                       #   Brev console configuration steps
  tier2-performance/                     # 2x A100 80GB — quantization, spec decode, benchmarking
    setup.sh                             #   Brev setup script (8B + 70B models, profiling tools)
    BREV_CONFIG.md                       #   Brev console configuration steps
  tier3-deep-tech/                       # 4x A100 80GB — distributed inference, llm-d, K8s
    setup.sh                             #   Brev setup script (full llm-d stack, kind, k9s)
    BREV_CONFIG.md                       #   Brev console configuration steps
```

## Tiers at a glance

| Tier | GPU | Models | Tracks |
|------|-----|--------|--------|
| **1 — App Builder** | 1x L40S (48 GB) | Llama 3.1 8B | RAG, BYOP, Eval |
| **2 — Performance** | 2x A100 80 GB | 8B + 70B | Quantization, Speculative Decode, Eval |
| **3 — Deep Tech** | 4x A100 80 GB | 8B + 70B | Distributed Inference, llm-d, K8s |

## How to use

### 1. Create Brev Launchables

For each tier you plan to offer:

1. Open the [NVIDIA Brev Console](https://brev.nvidia.com/).
2. Follow the step-by-step instructions in the tier's `BREV_CONFIG.md`.
3. Paste the contents of the tier's `setup.sh` as the setup script.
4. Publish the Launchable with link sharing enabled.

Each Launchable will provision a GPU instance with models pre-downloaded, tools installed, and starter scripts ready to go.

### 2. Share with attendees

Distribute the Launchable links to attendees along with the [Attendee Guide](attendee-guide.md). The guide covers:

- How to connect to their environment (Jupyter, SSH, API)
- What's pre-installed per tier
- Quick-start recipes for each of the 6 hackathon tracks
- Troubleshooting common issues

### 3. Estimate costs

Open `hackathon-gpu-cost-estimate.xlsx` to review and adjust GPU instance costs based on expected team count and session duration.

## Hackathon tracks

1. **Lean Inference Challenge** — Quantize models and optimize throughput
2. **RAG on Open Inference** — Build retrieval-augmented generation apps on vLLM
3. **Speculative Futures** — Speed up 70B inference with speculative decoding
4. **Inference at Scale** — Deploy llm-d on Kubernetes with disaggregated serving
5. **BYOP (Build Your Own Product)** — Ship a product powered by local LLM inference
6. **Performance Tuning & Evaluation** — Benchmark and evaluate with guidellm and lm-eval

## Links

- [vLLM Documentation](https://docs.vllm.ai/)
- [llm-d Documentation](https://llm-d.ai/docs/)
- [NVIDIA Brev Console](https://brev.nvidia.com/)
- [Brev Launchables Docs](https://docs.nvidia.com/brev/concepts/launchables)
