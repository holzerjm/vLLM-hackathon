# Hackathon Project Suggestions

One project per proficiency level, each mapped to a pre-configured Brev tier with ready-to-use tooling.

---

## Beginner: "Ask My Docs" — RAG-Powered Q&A App

**Track:** RAG on Open Inference / BYOP  
**Tier:** Tier 1 — App Builder

### What You'll Build

A Retrieval-Augmented Generation app that lets users upload documents (PDFs, markdown, text) and ask natural-language questions about them. The app ingests docs into ChromaDB, retrieves relevant chunks, and sends them as context to Llama 3.1 8B running on vLLM — all behind a Gradio UI.

### Why This Project

- Uses the pre-built FastAPI scaffold and test client already on the Tier 1 image
- LangChain, ChromaDB, and Gradio are pre-installed — no dependency wrangling
- Only requires Python and basic REST API knowledge
- Produces a visible, demo-able product in a few hours

### Recommended Configuration

| Setting | Value |
|---|---|
| **Brev Instance** | Tier 1 — App Builder |
| **GPU** | 1× L40S (48 GB) |
| **Model** | `/models/llama-3.1-8b-instruct` |
| **vLLM flags** | `--max-model-len 8192 --gpu-memory-utilization 0.85 --dtype auto` |
| **Embedding model** | `bge-small-en-v1.5` (pre-downloaded) |
| **Frontend** | Gradio (pre-installed) |

### Getting Started

```bash
# 1. Start the vLLM server
bash /workspace/scripts/start_vllm_server.sh

# 2. Verify it's running
python3 /workspace/scripts/test_client.py

# 3. Build from the app scaffold
cd /workspace/app-scaffold
# Edit main.py to add ChromaDB ingestion + retrieval chain
```

### Key Files

- `launchable-configs/tier1-app-builder/setup.sh` — see what's pre-installed
- `attendee-guide.md` — Track 2 (RAG) quick recipe

---

## Intermediate: "Speed Demon" — Speculative Decoding Benchmark Showdown

**Track:** Speculative Futures + Performance Tuning  
**Tier:** Tier 2 — Performance

### What You'll Build

A comparative benchmark suite that measures Llama 3.1 70B inference **with and without** speculative decoding (using the 8B model as draft). You'll tune draft token counts, measure acceptance rates, and produce a visual dashboard of latency vs. throughput trade-offs across different workload profiles (short chat, long-form generation, code completion).

### Why This Project

- Requires understanding the draft/verify cycle of speculative decoding
- Involves meaningful parameter tuning and quantitative analysis
- Benchmarking scripts and profiling tools are pre-installed
- Produces charts and data you can present to judges
- Stretch goal: layer in AWQ/GPTQ quantization for further speedups

### Recommended Configuration

| Setting | Value |
|---|---|
| **Brev Instance** | Tier 2 — Performance |
| **GPU** | 2× A100 (80 GB) |
| **Target model** | `/models/llama-3.1-70b-instruct` with `--tensor-parallel-size 2` |
| **Draft model** | `/models/llama-3.1-8b-instruct` |
| **vLLM flags** | `--tensor-parallel-size 2 --speculative-model /models/llama-3.1-8b-instruct --num-speculative-tokens 5` |
| **Benchmarking** | `guidellm`, `bench_throughput.sh` |
| **Profiling** | `nvtop`, `py-spy` |

### Getting Started

```bash
# 1. Run the baseline (no speculation) — record throughput & latency
bash /workspace/scripts/bench_throughput.sh

# 2. Launch with speculative decoding
bash /workspace/scripts/speculative_decoding.sh

# 3. Re-run benchmarks and compare
# 4. Vary --num-speculative-tokens (3, 5, 7) and plot the results
```

### Key Files

- `launchable-configs/tier2-performance/setup.sh` — see `speculative_decoding.sh` and `bench_throughput.sh`
- `attendee-guide.md` — Track 3 (Speculative Futures) and Track 6 (Performance Tuning) recipes

---

## Advanced: "Infinite Scale" — Disaggregated Prefill/Decode with llm-d on Kubernetes

**Track:** Inference at Scale  
**Tier:** Tier 3 — Deep Tech

### What You'll Build

A fully disaggregated inference deployment on Kubernetes where **prefill and decode phases run on separate GPU pools**, connected via KV-cache-aware routing. You'll deploy llm-d with Helm, configure autoscaling policies based on Prometheus metrics (queue depth, time-to-first-token, tokens-per-second), and demonstrate that the system can dynamically shift GPU allocation between prefill and decode pools under varying load.

### Why This Project

- Requires deep understanding of the prefill/decode split and KV-cache transfer
- Real systems-level work: Kubernetes orchestration, networking, scheduling, observability
- Mirrors production-grade inference infrastructure at companies serving LLMs at scale
- Stretch goal: implement custom routing policies or multi-model serving

### Recommended Configuration

| Setting | Value |
|---|---|
| **Brev Instance** | Tier 3 — Deep Tech |
| **GPU** | 4× A100 (80 GB) |
| **Model** | Llama 3.1 70B distributed across prefill + decode pools |
| **Orchestration** | kind cluster → llm-d Helm chart |
| **Helm values** | `values-70b-distributed.yaml` (prefill/decode split) |
| **Monitoring** | Prometheus + k9s |
| **Load testing** | `guidellm` against K8s ingress |

### Getting Started

```bash
# 1. Start the local Kubernetes cluster
bash /workspace/scripts/start_kind_cluster.sh

# 2. Deploy llm-d with the distributed config
bash /workspace/scripts/deploy_llm_d.sh

# 3. Monitor with k9s
k9s

# 4. Load-test and observe scaling behavior
guidellm --target http://<ingress-ip>/v1 --rate sweep
```

### Key Files

- `launchable-configs/tier3-deep-tech/setup.sh` — full llm-d + K8s stack setup
- `attendee-guide.md` — Track 4 (Inference at Scale) recipe

---

## Quick Reference

| Level | Project | Tier | GPU | Model | Key Challenge |
|---|---|---|---|---|---|
| Beginner | Ask My Docs | 1 | 1× L40S | 8B | Build a RAG app with Gradio + ChromaDB |
| Intermediate | Speed Demon | 2 | 2× A100 | 70B + 8B draft | Tune & benchmark speculative decoding |
| Advanced | Infinite Scale | 3 | 4× A100 | 70B distributed | Disaggregated prefill/decode on K8s |
