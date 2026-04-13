---
term: KV Cache (Key-Value Cache)
sphere: software
source: Transformer architecture — Vaswani et al., "Attention Is All You Need", 2017
tldr: The stored intermediate attention computation states (keys and values) that allow a transformer to process new tokens without recomputing all prior context — the dominant VRAM consumer at inference scale.
---

## Definition

In transformer inference, each token in a sequence attends to all previous tokens via the attention mechanism. Without caching, generating each new token would require recomputing attention over the entire context — O(n²) cost.

The KV cache stores the Key and Value matrices computed for all previous tokens in memory (VRAM). When generating a new token, the model retrieves cached K and V matrices and only computes the new token's Query — O(n) incremental cost.

## Memory impact

KV cache VRAM consumption scales with:
- Context length (tokens)
- Batch size (parallel users)
- Model size (number of attention layers × hidden dimension)
- Precision (FP16, BF16, FP8, INT4)

**Example — Llama 3.1 70B, BF16, 128K context, batch=1:**
```
KV cache = 2 × layers × heads × head_dim × context_len × bytes_per_element
         ≈ 2 × 80 × 64 × 128 × 131,072 × 2 bytes
         ≈ ~34 GB
```
Model weights alone: 140 GB. Total VRAM needed: ~174 GB — requires 3× H100 or 1× H200.

## KV cache and agentic workloads

Agentic inference (8-hour autonomous sessions, 200K–1M token contexts) makes KV cache the dominant infrastructure challenge:

- **Size**: a 1M token context with Llama 3.1 70B uses ~260 GB of KV cache — more than the model weights themselves
- **Persistence**: agentic sessions must persist KV cache between tool calls and sub-tasks (unlike stateless REST inference)
- **Eviction strategy**: when VRAM fills, KV cache must be offloaded to CPU DRAM or storage — creating latency spikes unless storage is fast

**Emerging solutions:**
- **Prefix caching**: cache KV states for common system prompts (reused across users)
- **KV cache offloading**: move cold cache entries to CPU DRAM or NVMe SSD
- **Quantised KV**: INT8/INT4 KV cache — 2–4× memory reduction with minimal quality loss
- **Streaming LLM**: sliding window approaches for infinite context without growing cache

## Why it matters for ICN

The agentic storage play depends on KV cache economics:
1. Enterprises running long-context agentic sessions need fast storage adjacent to compute for KV cache offload
2. IC's high-performance S3 (Nitro product) is a natural KV cache tier — fast enough for offload, cheap at scale
3. As context windows grow to 1M+ tokens, KV cache storage requirements dwarf model weights — making storage the primary operational cost

The "Context is Gold" positioning is partly about KV cache: the context stored in IC is what makes the model *useful for that specific enterprise* — it embodies their workflow, their data, their interaction history.

## Key sources

- Pope et al., "Efficiently Scaling Transformer Inference" (Google, 2022)
- vLLM documentation — PagedAttention (KV cache memory management)
- Anthropic: Claude context window management documentation
