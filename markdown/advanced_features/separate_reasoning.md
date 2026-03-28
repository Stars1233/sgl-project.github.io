# Reasoning Parser

SGLang supports parsing reasoning content out from "normal" content for reasoning models such as [DeepSeek R1](https://huggingface.co/deepseek-ai/DeepSeek-R1).

## Supported Models & Parsers

| Model  |  Reasoning tags      | Parser | Notes |
|---------|-----------------------------|------------------|-------|
| [DeepSeek‑R1 series](https://huggingface.co/collections/deepseek-ai/deepseek-r1-678e1e131c0169c0bc89728d) | `<think>` … `</think>` | `deepseek-r1` | Supports all variants (R1, R1-0528, R1-Distill) |
| [DeepSeek‑V3 series](https://huggingface.co/deepseek-ai/DeepSeek-V3.1) | `<think>` … `</think>` | `deepseek-v3` | Including [DeepSeek‑V3.2](https://huggingface.co/deepseek-ai/DeepSeek-V3.2-Exp). Supports `thinking` parameter |
| [Standard Qwen3 models](https://huggingface.co/collections/Qwen/qwen3-67dd247413f0e2e4f653967f) | `<think>` … `</think>` | `qwen3` | Supports `enable_thinking` parameter |
| [Qwen3-Thinking models](https://huggingface.co/Qwen/Qwen3-235B-A22B-Thinking-2507) | `<think>` … `</think>` | `qwen3` or `qwen3-thinking` | Always generates thinking content |
| [Kimi K2 Thinking](https://huggingface.co/moonshotai/Kimi-K2-Thinking) | `◁think▷` … `◁/think▷` | `kimi_k2` | Uses special thinking delimiters. Also requires `--tool-call-parser kimi_k2` for tool use. |
| [GPT OSS](https://huggingface.co/openai/gpt-oss-120b) | `<\|channel\|>analysis<\|message\|>` … `<\|end\|>` | `gpt-oss` | N/A |
### Model-Specific Behaviors

**DeepSeek-R1 Family:**
- DeepSeek-R1: No `<think>` start tag, jumps directly to thinking content
- DeepSeek-R1-0528: Generates both `<think>` start and `</think>` end tags
- Both are handled by the same `deepseek-r1` parser

**DeepSeek-V3 Family:**
- DeepSeek-V3.1/V3.2: Hybrid model supporting both thinking and non-thinking modes, use the `deepseek-v3` parser and `thinking` parameter (NOTE: not `enable_thinking`)

**Qwen3 Family:**
- Standard Qwen3 (e.g., Qwen3-2507): Use `qwen3` parser, supports `enable_thinking` in chat templates
- Qwen3-Thinking (e.g., Qwen3-235B-A22B-Thinking-2507): Use `qwen3` or `qwen3-thinking` parser, always thinks

**Kimi K2:**
- Kimi K2 Thinking: Uses special `◁think▷` and `◁/think▷` tags. For agentic tool use, also specify `--tool-call-parser kimi_k2`.

**GPT OSS:**
- GPT OSS: Uses special `<|channel|>analysis<|message|>` and `<|end|>` tags

## Usage

### Launching the Server

Specify the `--reasoning-parser` option.


```python
import requests
from openai import OpenAI
from sglang.test.doc_patch import launch_server_cmd
from sglang.utils import wait_for_server, print_highlight, terminate_process

server_process, port = launch_server_cmd(
    "python3 -m sglang.launch_server --model-path deepseek-ai/DeepSeek-R1-Distill-Qwen-7B --host 0.0.0.0 --reasoning-parser deepseek-r1 --log-level warning"
)

wait_for_server(f"http://localhost:{port}", process=server_process)
```

    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:175: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [2026-03-28 05:20:33] numactl command not found, skipping NUMA node configuration for GPU. Install numactl (e.g., apt-get install numactl) to enable automatic NUMA binding.


    [2026-03-28 05:20:39] numactl command not found, skipping NUMA node configuration for GPU. Install numactl (e.g., apt-get install numactl) to enable automatic NUMA binding.


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-03-28 05:20:42.947 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 05:20:42] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 05:20:42.947 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 05:20:42] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 05:20:42.947 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 05:20:42] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 05:20:42.947 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 05:20:42] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 05:20:42.948 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 05:20:42] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:01<00:01,  1.09s/it]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.36s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.32s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:47,  2.93s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:47,  2.93s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:42,  1.83s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:42,  1.83s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:05,  1.19s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:05,  1.19s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:47,  1.13it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:47,  1.13it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:36,  1.45it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:36,  1.45it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:29,  1.75it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:29,  1.75it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:24,  2.09it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:24,  2.09it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:06<00:20,  2.42it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:06<00:20,  2.42it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:06<00:17,  2.79it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:06<00:17,  2.79it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:06<00:15,  3.14it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:06<00:15,  3.14it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:06<00:13,  3.49it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:06<00:13,  3.49it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:06<00:11,  3.84it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:06<00:11,  3.84it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:07<00:10,  4.22it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:07<00:10,  4.22it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:07<00:09,  4.67it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:07<00:09,  4.67it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:07<00:08,  5.13it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:07<00:08,  5.13it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:07<00:07,  5.68it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:07<00:07,  5.68it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:07<00:06,  6.27it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:07<00:06,  6.27it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:07<00:05,  6.91it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:07<00:05,  6.91it/s]

    Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:07<00:05,  6.91it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:07<00:04,  8.53it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:07<00:04,  8.53it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:08<00:04,  8.53it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:08<00:03, 10.78it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:08<00:03, 10.78it/s]

    Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:08<00:03, 10.78it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:08<00:02, 12.64it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:08<00:02, 12.64it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:08<00:02, 12.64it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:08<00:02, 12.64it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:08<00:02, 15.25it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:08<00:02, 15.25it/s]

    Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:08<00:02, 15.25it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:08<00:02, 15.25it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:08<00:01, 18.01it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:08<00:01, 18.01it/s]Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:08<00:01, 18.01it/s]Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:08<00:01, 18.01it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:08<00:01, 20.50it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:08<00:01, 20.50it/s]

    Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:08<00:01, 20.50it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:08<00:01, 20.50it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:08<00:00, 22.74it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:08<00:00, 22.74it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:08<00:00, 22.74it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:08<00:00, 22.74it/s]Compiling num tokens (num_tokens=208):  62%|██████▏   | 36/58 [00:08<00:00, 22.74it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:08<00:00, 26.90it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:08<00:00, 26.90it/s]

    Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:08<00:00, 26.90it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:08<00:00, 26.90it/s]Compiling num tokens (num_tokens=144):  69%|██████▉   | 40/58 [00:08<00:00, 26.90it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:08<00:00, 29.65it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:08<00:00, 29.65it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:08<00:00, 29.65it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:08<00:00, 29.65it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:08<00:00, 29.65it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:09<00:00, 32.01it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:09<00:00, 32.01it/s]

    Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:09<00:00, 32.01it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:09<00:00, 32.01it/s]Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:09<00:00, 32.01it/s]Compiling num tokens (num_tokens=24):  83%|████████▎ | 48/58 [00:09<00:00, 32.01it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:09<00:00, 35.72it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:09<00:00, 35.72it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:09<00:00, 35.72it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:09<00:00, 35.72it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:09<00:00, 35.72it/s] Compiling num tokens (num_tokens=4):  91%|█████████▏| 53/58 [00:09<00:00, 35.72it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:09<00:00,  6.29it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=30.52 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=30.52 GB):   2%|▏         | 1/58 [00:00<00:34,  1.67it/s]Capturing num tokens (num_tokens=7680 avail_mem=30.45 GB):   2%|▏         | 1/58 [00:00<00:34,  1.67it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=30.45 GB):   3%|▎         | 2/58 [00:01<00:32,  1.73it/s]Capturing num tokens (num_tokens=7168 avail_mem=30.45 GB):   3%|▎         | 2/58 [00:01<00:32,  1.73it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=30.45 GB):   5%|▌         | 3/58 [00:01<00:30,  1.81it/s]Capturing num tokens (num_tokens=6656 avail_mem=30.46 GB):   5%|▌         | 3/58 [00:01<00:30,  1.81it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=30.46 GB):   7%|▋         | 4/58 [00:02<00:31,  1.69it/s]Capturing num tokens (num_tokens=6144 avail_mem=30.47 GB):   7%|▋         | 4/58 [00:02<00:31,  1.69it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=30.47 GB):   9%|▊         | 5/58 [00:02<00:28,  1.86it/s]Capturing num tokens (num_tokens=5632 avail_mem=39.54 GB):   9%|▊         | 5/58 [00:02<00:28,  1.86it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=39.54 GB):  10%|█         | 6/58 [00:03<00:24,  2.09it/s]Capturing num tokens (num_tokens=5120 avail_mem=39.54 GB):  10%|█         | 6/58 [00:03<00:24,  2.09it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=39.54 GB):  12%|█▏        | 7/58 [00:03<00:22,  2.30it/s]Capturing num tokens (num_tokens=4608 avail_mem=39.55 GB):  12%|█▏        | 7/58 [00:03<00:22,  2.30it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=39.55 GB):  14%|█▍        | 8/58 [00:03<00:19,  2.57it/s]Capturing num tokens (num_tokens=4096 avail_mem=39.56 GB):  14%|█▍        | 8/58 [00:03<00:19,  2.57it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=39.56 GB):  16%|█▌        | 9/58 [00:04<00:17,  2.86it/s]Capturing num tokens (num_tokens=3840 avail_mem=39.57 GB):  16%|█▌        | 9/58 [00:04<00:17,  2.86it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=39.57 GB):  17%|█▋        | 10/58 [00:04<00:15,  3.11it/s]Capturing num tokens (num_tokens=3584 avail_mem=39.56 GB):  17%|█▋        | 10/58 [00:04<00:15,  3.11it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=39.56 GB):  19%|█▉        | 11/58 [00:04<00:13,  3.40it/s]Capturing num tokens (num_tokens=3328 avail_mem=39.56 GB):  19%|█▉        | 11/58 [00:04<00:13,  3.40it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=39.56 GB):  21%|██        | 12/58 [00:04<00:12,  3.73it/s]Capturing num tokens (num_tokens=3072 avail_mem=39.56 GB):  21%|██        | 12/58 [00:04<00:12,  3.73it/s]Capturing num tokens (num_tokens=3072 avail_mem=39.56 GB):  22%|██▏       | 13/58 [00:04<00:11,  4.06it/s]Capturing num tokens (num_tokens=2816 avail_mem=39.56 GB):  22%|██▏       | 13/58 [00:04<00:11,  4.06it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=39.56 GB):  24%|██▍       | 14/58 [00:05<00:09,  4.49it/s]Capturing num tokens (num_tokens=2560 avail_mem=44.05 GB):  24%|██▍       | 14/58 [00:05<00:09,  4.49it/s]Capturing num tokens (num_tokens=2560 avail_mem=44.05 GB):  26%|██▌       | 15/58 [00:05<00:08,  4.97it/s]Capturing num tokens (num_tokens=2304 avail_mem=44.06 GB):  26%|██▌       | 15/58 [00:05<00:08,  4.97it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=44.06 GB):  28%|██▊       | 16/58 [00:05<00:07,  5.47it/s]Capturing num tokens (num_tokens=2048 avail_mem=44.06 GB):  28%|██▊       | 16/58 [00:05<00:07,  5.47it/s]Capturing num tokens (num_tokens=2048 avail_mem=44.06 GB):  29%|██▉       | 17/58 [00:05<00:06,  6.10it/s]Capturing num tokens (num_tokens=1792 avail_mem=44.06 GB):  29%|██▉       | 17/58 [00:05<00:06,  6.10it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=44.06 GB):  31%|███       | 18/58 [00:05<00:05,  6.78it/s]Capturing num tokens (num_tokens=1536 avail_mem=44.05 GB):  31%|███       | 18/58 [00:05<00:05,  6.78it/s]Capturing num tokens (num_tokens=1280 avail_mem=44.05 GB):  31%|███       | 18/58 [00:05<00:05,  6.78it/s]Capturing num tokens (num_tokens=1280 avail_mem=44.05 GB):  34%|███▍      | 20/58 [00:05<00:04,  8.28it/s]Capturing num tokens (num_tokens=1024 avail_mem=44.05 GB):  34%|███▍      | 20/58 [00:05<00:04,  8.28it/s]

    Capturing num tokens (num_tokens=960 avail_mem=44.04 GB):  34%|███▍      | 20/58 [00:05<00:04,  8.28it/s] Capturing num tokens (num_tokens=960 avail_mem=44.04 GB):  38%|███▊      | 22/58 [00:05<00:03, 10.04it/s]Capturing num tokens (num_tokens=896 avail_mem=44.03 GB):  38%|███▊      | 22/58 [00:05<00:03, 10.04it/s]Capturing num tokens (num_tokens=832 avail_mem=44.03 GB):  38%|███▊      | 22/58 [00:06<00:03, 10.04it/s]Capturing num tokens (num_tokens=832 avail_mem=44.03 GB):  41%|████▏     | 24/58 [00:06<00:02, 11.46it/s]Capturing num tokens (num_tokens=768 avail_mem=44.02 GB):  41%|████▏     | 24/58 [00:06<00:02, 11.46it/s]

    Capturing num tokens (num_tokens=704 avail_mem=44.01 GB):  41%|████▏     | 24/58 [00:06<00:02, 11.46it/s]Capturing num tokens (num_tokens=704 avail_mem=44.01 GB):  45%|████▍     | 26/58 [00:06<00:02, 12.91it/s]Capturing num tokens (num_tokens=640 avail_mem=44.00 GB):  45%|████▍     | 26/58 [00:06<00:02, 12.91it/s]Capturing num tokens (num_tokens=576 avail_mem=44.00 GB):  45%|████▍     | 26/58 [00:06<00:02, 12.91it/s]Capturing num tokens (num_tokens=576 avail_mem=44.00 GB):  48%|████▊     | 28/58 [00:06<00:02, 14.37it/s]Capturing num tokens (num_tokens=512 avail_mem=43.99 GB):  48%|████▊     | 28/58 [00:06<00:02, 14.37it/s]

    Capturing num tokens (num_tokens=480 avail_mem=43.94 GB):  48%|████▊     | 28/58 [00:06<00:02, 14.37it/s]Capturing num tokens (num_tokens=448 avail_mem=43.92 GB):  48%|████▊     | 28/58 [00:06<00:02, 14.37it/s]Capturing num tokens (num_tokens=448 avail_mem=43.92 GB):  53%|█████▎    | 31/58 [00:06<00:01, 14.01it/s]Capturing num tokens (num_tokens=416 avail_mem=43.94 GB):  53%|█████▎    | 31/58 [00:06<00:01, 14.01it/s]

    Capturing num tokens (num_tokens=384 avail_mem=43.96 GB):  53%|█████▎    | 31/58 [00:06<00:01, 14.01it/s]Capturing num tokens (num_tokens=384 avail_mem=43.96 GB):  57%|█████▋    | 33/58 [00:06<00:01, 14.74it/s]Capturing num tokens (num_tokens=352 avail_mem=43.95 GB):  57%|█████▋    | 33/58 [00:06<00:01, 14.74it/s]Capturing num tokens (num_tokens=320 avail_mem=43.94 GB):  57%|█████▋    | 33/58 [00:06<00:01, 14.74it/s]Capturing num tokens (num_tokens=288 avail_mem=43.94 GB):  57%|█████▋    | 33/58 [00:06<00:01, 14.74it/s]Capturing num tokens (num_tokens=288 avail_mem=43.94 GB):  62%|██████▏   | 36/58 [00:06<00:01, 17.33it/s]Capturing num tokens (num_tokens=256 avail_mem=43.93 GB):  62%|██████▏   | 36/58 [00:06<00:01, 17.33it/s]

    Capturing num tokens (num_tokens=240 avail_mem=43.92 GB):  62%|██████▏   | 36/58 [00:06<00:01, 17.33it/s]Capturing num tokens (num_tokens=224 avail_mem=43.91 GB):  62%|██████▏   | 36/58 [00:06<00:01, 17.33it/s]Capturing num tokens (num_tokens=224 avail_mem=43.91 GB):  67%|██████▋   | 39/58 [00:06<00:00, 19.75it/s]Capturing num tokens (num_tokens=208 avail_mem=43.91 GB):  67%|██████▋   | 39/58 [00:06<00:00, 19.75it/s]Capturing num tokens (num_tokens=192 avail_mem=43.90 GB):  67%|██████▋   | 39/58 [00:06<00:00, 19.75it/s]Capturing num tokens (num_tokens=176 avail_mem=43.89 GB):  67%|██████▋   | 39/58 [00:06<00:00, 19.75it/s]Capturing num tokens (num_tokens=176 avail_mem=43.89 GB):  72%|███████▏  | 42/58 [00:06<00:00, 21.84it/s]Capturing num tokens (num_tokens=160 avail_mem=43.89 GB):  72%|███████▏  | 42/58 [00:06<00:00, 21.84it/s]

    Capturing num tokens (num_tokens=144 avail_mem=43.88 GB):  72%|███████▏  | 42/58 [00:07<00:00, 21.84it/s]Capturing num tokens (num_tokens=128 avail_mem=43.90 GB):  72%|███████▏  | 42/58 [00:07<00:00, 21.84it/s]Capturing num tokens (num_tokens=128 avail_mem=43.90 GB):  78%|███████▊  | 45/58 [00:07<00:00, 23.54it/s]Capturing num tokens (num_tokens=112 avail_mem=43.89 GB):  78%|███████▊  | 45/58 [00:07<00:00, 23.54it/s]Capturing num tokens (num_tokens=96 avail_mem=43.88 GB):  78%|███████▊  | 45/58 [00:07<00:00, 23.54it/s] Capturing num tokens (num_tokens=80 avail_mem=43.87 GB):  78%|███████▊  | 45/58 [00:07<00:00, 23.54it/s]Capturing num tokens (num_tokens=80 avail_mem=43.87 GB):  83%|████████▎ | 48/58 [00:07<00:00, 24.83it/s]Capturing num tokens (num_tokens=64 avail_mem=43.86 GB):  83%|████████▎ | 48/58 [00:07<00:00, 24.83it/s]

    Capturing num tokens (num_tokens=48 avail_mem=43.86 GB):  83%|████████▎ | 48/58 [00:07<00:00, 24.83it/s]Capturing num tokens (num_tokens=32 avail_mem=43.85 GB):  83%|████████▎ | 48/58 [00:07<00:00, 24.83it/s]Capturing num tokens (num_tokens=32 avail_mem=43.85 GB):  88%|████████▊ | 51/58 [00:07<00:00, 26.06it/s]Capturing num tokens (num_tokens=28 avail_mem=43.85 GB):  88%|████████▊ | 51/58 [00:07<00:00, 26.06it/s]Capturing num tokens (num_tokens=24 avail_mem=43.84 GB):  88%|████████▊ | 51/58 [00:07<00:00, 26.06it/s]Capturing num tokens (num_tokens=20 avail_mem=43.83 GB):  88%|████████▊ | 51/58 [00:07<00:00, 26.06it/s]Capturing num tokens (num_tokens=16 avail_mem=43.83 GB):  88%|████████▊ | 51/58 [00:07<00:00, 26.06it/s]

    Capturing num tokens (num_tokens=16 avail_mem=43.83 GB):  95%|█████████▍| 55/58 [00:07<00:00, 27.38it/s]Capturing num tokens (num_tokens=12 avail_mem=43.82 GB):  95%|█████████▍| 55/58 [00:07<00:00, 27.38it/s]Capturing num tokens (num_tokens=8 avail_mem=43.81 GB):  95%|█████████▍| 55/58 [00:07<00:00, 27.38it/s] Capturing num tokens (num_tokens=4 avail_mem=43.81 GB):  95%|█████████▍| 55/58 [00:07<00:00, 27.38it/s]Capturing num tokens (num_tokens=4 avail_mem=43.81 GB): 100%|██████████| 58/58 [00:07<00:00,  7.71it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>


Note that `--reasoning-parser` defines the parser used to interpret responses.

### OpenAI Compatible API

Using the OpenAI compatible API, the contract follows the [DeepSeek API design](https://api-docs.deepseek.com/guides/reasoning_model) established with the release of DeepSeek-R1:

- `reasoning_content`: The content of the CoT.
- `content`: The content of the final answer.


```python
# Initialize OpenAI-like client
client = OpenAI(api_key="None", base_url=f"http://0.0.0.0:{port}/v1")
model_name = client.models.list().data[0].id

messages = [
    {
        "role": "user",
        "content": "What is 1+3?",
    }
]
```

#### Non-Streaming Request


```python
response_non_stream = client.chat.completions.create(
    model=model_name,
    messages=messages,
    temperature=0.6,
    top_p=0.95,
    stream=False,  # Non-streaming
    extra_body={"separate_reasoning": True},
)
print_highlight("==== Reasoning ====")
print_highlight(response_non_stream.choices[0].message.reasoning_content)

print_highlight("==== Text ====")
print_highlight(response_non_stream.choices[0].message.content)
```


<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>I need to add the numbers 1 and 3 together.<br><br>First, I recognize that 1 is a single unit.<br><br>Next, I add 3 more units to the initial 1.<br><br>Combining these, the total sum is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>Sure! Let's solve the addition problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Start with the first number:**<br>   \[<br>   1<br>   \]<br><br>2. **Add the second number to it:**<br>   \[<br>   1 + 3<br>   \]<br><br>3. **Combine the two numbers to find the sum:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:** \(\boxed{4}\)</strong>


#### Streaming Request


```python
response_stream = client.chat.completions.create(
    model=model_name,
    messages=messages,
    temperature=0.6,
    top_p=0.95,
    stream=True,  # Non-streaming
    extra_body={"separate_reasoning": True},
)

reasoning_content = ""
content = ""
for chunk in response_stream:
    if chunk.choices[0].delta.content:
        content += chunk.choices[0].delta.content
    if chunk.choices[0].delta.reasoning_content:
        reasoning_content += chunk.choices[0].delta.reasoning_content

print_highlight("==== Reasoning ====")
print_highlight(reasoning_content)

print_highlight("==== Text ====")
print_highlight(content)
```


<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>To solve the problem of adding 1 and 3, I start by identifying the two numbers involved.<br><br>Next, I perform the addition operation by combining these two numbers.<br><br>Finally, I calculate the sum to determine the result.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'><br><br>**Solution:**<br><br>To find the sum of 1 and 3, follow these simple steps:<br><br>1. **Start with the first number:**<br>   <br>   \[<br>   1<br>   \]<br><br>2. **Add the second number:**<br>   <br>   \[<br>   1 + 3<br>   \]<br><br>3. **Calculate the total:**<br>   <br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Final Answer:**<br><br>\[<br>\boxed{4}<br>\]</strong>


Optionally, you can buffer the reasoning content to the last reasoning chunk (or the first chunk after the reasoning content).


```python
response_stream = client.chat.completions.create(
    model=model_name,
    messages=messages,
    temperature=0.6,
    top_p=0.95,
    stream=True,  # Non-streaming
    extra_body={"separate_reasoning": True, "stream_reasoning": False},
)

reasoning_content = ""
content = ""
for chunk in response_stream:
    if chunk.choices[0].delta.content:
        content += chunk.choices[0].delta.content
    if chunk.choices[0].delta.reasoning_content:
        reasoning_content += chunk.choices[0].delta.reasoning_content

print_highlight("==== Reasoning ====")
print_highlight(reasoning_content)

print_highlight("==== Text ====")
print_highlight(content)
```


<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I'll add the two numbers together: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.</strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'><br><br>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the final answer is \(\boxed{4}\).</strong>


The reasoning separation is enable by default when specify . 
**To disable it, set the `separate_reasoning` option to `False` in request.**


```python
response_non_stream = client.chat.completions.create(
    model=model_name,
    messages=messages,
    temperature=0.6,
    top_p=0.95,
    stream=False,  # Non-streaming
    extra_body={"separate_reasoning": False},
)

print_highlight("==== Original Output ====")
print_highlight(response_non_stream.choices[0].message.content)
```


<strong style='color: #00008B;'>==== Original Output ====</strong>



<strong style='color: #00008B;'>First, I need to identify the two numbers in the problem: 1 and 3.<br><br>Next, I'll add these two numbers together.<br><br>Adding 1 and 3 gives me 4.<br><br>Therefore, the sum of 1 and 3 is 4.<br></think><br><br>Certainly! Let's solve the addition problem step by step.<br><br>**Problem:**<br>\[ 1 + 3 \]<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>3. **Final Answer:**<br>   \[<br>   \boxed{4}<br>   \]<br><br>**Answer:** \(\boxed{4}\)</strong>


### SGLang Native API 


```python
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("deepseek-ai/DeepSeek-R1-Distill-Qwen-7B")
input = tokenizer.apply_chat_template(
    messages, tokenize=False, add_generation_prompt=True, return_dict=False
)

gen_url = f"http://localhost:{port}/generate"
gen_data = {
    "text": input,
    "sampling_params": {
        "skip_special_tokens": False,
        "max_new_tokens": 1024,
        "temperature": 0.6,
        "top_p": 0.95,
    },
}
gen_response = requests.post(gen_url, json=gen_data).json()["text"]

print_highlight("==== Original Output ====")
print_highlight(gen_response)

parse_url = f"http://localhost:{port}/separate_reasoning"
separate_reasoning_data = {
    "text": gen_response,
    "reasoning_parser": "deepseek-r1",
}
separate_reasoning_response_json = requests.post(
    parse_url, json=separate_reasoning_data
).json()
print_highlight("==== Reasoning ====")
print_highlight(separate_reasoning_response_json["reasoning_text"])
print_highlight("==== Text ====")
print_highlight(separate_reasoning_response_json["text"])
```


<strong style='color: #00008B;'>==== Original Output ====</strong>



<strong style='color: #00008B;'>I recognize that the problem asks for the sum of 1 and 3.<br><br>I can start by identifying the numbers involved, which are 1 and 3.<br><br>Next, I'll add these two numbers together.<br><br>After performing the addition, I'll determine the total.<br><br>Finally, I'll present the result as the solution to the problem.<br></think><br><br>Sure! Let's solve the problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**  <br>   We have the numbers 1 and 3.<br><br>2. **Add the numbers together:**  <br>   \[<br>   1 + 3 = 4<br>   \]<br><br>3. **Present the final answer:**  <br>   \[<br>   \boxed{4}<br>   \]<br><br>**Answer:** \(\boxed{4}\)</strong>


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:328: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      return await dependant.call(**values)



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>I recognize that the problem asks for the sum of 1 and 3.<br><br>I can start by identifying the numbers involved, which are 1 and 3.<br><br>Next, I'll add these two numbers together.<br><br>After performing the addition, I'll determine the total.<br><br>Finally, I'll present the result as the solution to the problem.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>Sure! Let's solve the problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**  <br>   We have the numbers 1 and 3.<br><br>2. **Add the numbers together:**  <br>   \[<br>   1 + 3 = 4<br>   \]<br><br>3. **Present the final answer:**  <br>   \[<br>   \boxed{4}<br>   \]<br><br>**Answer:** \(\boxed{4}\)</strong>



```python
terminate_process(server_process)
```

### Offline Engine API


```python
import sglang as sgl
from sglang.srt.parser.reasoning_parser import ReasoningParser
from sglang.utils import print_highlight

llm = sgl.Engine(model_path="deepseek-ai/DeepSeek-R1-Distill-Qwen-7B")
tokenizer = AutoTokenizer.from_pretrained("deepseek-ai/DeepSeek-R1-Distill-Qwen-7B")
input = tokenizer.apply_chat_template(
    messages, tokenize=False, add_generation_prompt=True, return_dict=False
)
sampling_params = {
    "max_new_tokens": 1024,
    "skip_special_tokens": False,
    "temperature": 0.6,
    "top_p": 0.95,
}
result = llm.generate(prompt=input, sampling_params=sampling_params)

generated_text = result["text"]  # Assume there is only one prompt

print_highlight("==== Original Output ====")
print_highlight(generated_text)

parser = ReasoningParser("deepseek-r1")
reasoning_text, text = parser.parse_non_stream(generated_text)
print_highlight("==== Reasoning ====")
print_highlight(reasoning_text)
print_highlight("==== Text ====")
print_highlight(text)
```

    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-03-28 05:21:35.849 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 05:21:35] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 05:21:35.849 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 05:21:35] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 05:21:35.849 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 05:21:35] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 05:21:35.849 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 05:21:35] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 05:21:35.849 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 05:21:35] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:01<00:01,  1.15s/it]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.50s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.45s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:58,  3.13s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:58,  3.13s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:04<01:41,  1.82s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:04<01:41,  1.82s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:05,  1.19s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:05,  1.19s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:47,  1.14it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:47,  1.14it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:36,  1.46it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:36,  1.46it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:29,  1.77it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:29,  1.77it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:24,  2.10it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:24,  2.10it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:06<00:20,  2.44it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:06<00:20,  2.44it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:06<00:17,  2.81it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:06<00:17,  2.81it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:06<00:14,  3.21it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:06<00:14,  3.21it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:06<00:13,  3.58it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:06<00:13,  3.58it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:06<00:11,  4.00it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:06<00:11,  4.00it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:07<00:10,  4.41it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:07<00:10,  4.41it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:07<00:09,  4.79it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:07<00:09,  4.79it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:07<00:08,  5.30it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:07<00:08,  5.30it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:07<00:07,  5.86it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:07<00:07,  5.86it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:07<00:06,  6.45it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:07<00:06,  6.45it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:07<00:05,  7.18it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:07<00:05,  7.18it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:07<00:05,  7.18it/s]

    Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:07<00:04,  8.40it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:07<00:04,  8.40it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:08<00:04,  8.40it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:08<00:03, 10.17it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:08<00:03, 10.17it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:08<00:03, 10.17it/s]

    Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:08<00:02, 11.64it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:08<00:02, 11.64it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:08<00:02, 11.64it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:08<00:02, 13.21it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:08<00:02, 13.21it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:08<00:02, 13.21it/s]

    Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:08<00:02, 14.81it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:08<00:02, 14.81it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:08<00:02, 14.81it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:08<00:02, 14.81it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:08<00:01, 17.09it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:08<00:01, 17.09it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:08<00:01, 17.09it/s]

    Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:08<00:01, 17.51it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:08<00:01, 17.51it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:08<00:01, 17.51it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:08<00:01, 17.51it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:08<00:01, 19.20it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:08<00:01, 19.20it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:08<00:01, 19.20it/s]

    Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:08<00:01, 19.20it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:08<00:00, 20.41it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:08<00:00, 20.41it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:08<00:00, 20.41it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:09<00:00, 20.41it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:09<00:00, 21.15it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:09<00:00, 21.15it/s]

    Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:09<00:00, 21.15it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:09<00:00, 21.15it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:09<00:00, 21.91it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:09<00:00, 21.91it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:09<00:00, 21.91it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:09<00:00, 21.91it/s]

    Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:09<00:00, 22.50it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:09<00:00, 22.50it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:09<00:00, 22.50it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:09<00:00, 22.50it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:09<00:00, 23.49it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:09<00:00, 23.49it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:09<00:00, 23.49it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:09<00:00, 23.49it/s]

    Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:09<00:00, 23.49it/s]Compiling num tokens (num_tokens=16):  95%|█████████▍| 55/58 [00:09<00:00, 25.88it/s]Compiling num tokens (num_tokens=12):  95%|█████████▍| 55/58 [00:09<00:00, 25.88it/s]Compiling num tokens (num_tokens=8):  95%|█████████▍| 55/58 [00:09<00:00, 25.88it/s] Compiling num tokens (num_tokens=4):  95%|█████████▍| 55/58 [00:09<00:00, 25.88it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:09<00:00,  6.01it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=43.27 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=43.27 GB):   2%|▏         | 1/58 [00:00<00:37,  1.51it/s]Capturing num tokens (num_tokens=7680 avail_mem=43.24 GB):   2%|▏         | 1/58 [00:00<00:37,  1.51it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=43.24 GB):   3%|▎         | 2/58 [00:01<00:35,  1.57it/s]Capturing num tokens (num_tokens=7168 avail_mem=43.24 GB):   3%|▎         | 2/58 [00:01<00:35,  1.57it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=43.24 GB):   5%|▌         | 3/58 [00:01<00:31,  1.72it/s]Capturing num tokens (num_tokens=6656 avail_mem=43.24 GB):   5%|▌         | 3/58 [00:01<00:31,  1.72it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=43.24 GB):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]Capturing num tokens (num_tokens=6144 avail_mem=43.25 GB):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=43.25 GB):   9%|▊         | 5/58 [00:02<00:26,  1.98it/s]Capturing num tokens (num_tokens=5632 avail_mem=43.25 GB):   9%|▊         | 5/58 [00:02<00:26,  1.98it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=43.25 GB):  10%|█         | 6/58 [00:03<00:24,  2.17it/s]Capturing num tokens (num_tokens=5120 avail_mem=43.25 GB):  10%|█         | 6/58 [00:03<00:24,  2.17it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=43.25 GB):  12%|█▏        | 7/58 [00:03<00:22,  2.31it/s]Capturing num tokens (num_tokens=4608 avail_mem=43.26 GB):  12%|█▏        | 7/58 [00:03<00:22,  2.31it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=43.26 GB):  14%|█▍        | 8/58 [00:03<00:18,  2.70it/s]Capturing num tokens (num_tokens=4096 avail_mem=43.26 GB):  14%|█▍        | 8/58 [00:03<00:18,  2.70it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=43.26 GB):  16%|█▌        | 9/58 [00:04<00:17,  2.85it/s]Capturing num tokens (num_tokens=3840 avail_mem=43.26 GB):  16%|█▌        | 9/58 [00:04<00:17,  2.85it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=43.26 GB):  17%|█▋        | 10/58 [00:04<00:16,  2.98it/s]Capturing num tokens (num_tokens=3584 avail_mem=43.26 GB):  17%|█▋        | 10/58 [00:04<00:16,  2.98it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=43.26 GB):  19%|█▉        | 11/58 [00:04<00:14,  3.15it/s]Capturing num tokens (num_tokens=3328 avail_mem=43.26 GB):  19%|█▉        | 11/58 [00:04<00:14,  3.15it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=43.26 GB):  21%|██        | 12/58 [00:04<00:13,  3.34it/s]Capturing num tokens (num_tokens=3072 avail_mem=43.26 GB):  21%|██        | 12/58 [00:04<00:13,  3.34it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=43.26 GB):  22%|██▏       | 13/58 [00:05<00:12,  3.49it/s]Capturing num tokens (num_tokens=2816 avail_mem=43.26 GB):  22%|██▏       | 13/58 [00:05<00:12,  3.49it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=43.26 GB):  24%|██▍       | 14/58 [00:05<00:11,  3.69it/s]Capturing num tokens (num_tokens=2560 avail_mem=43.26 GB):  24%|██▍       | 14/58 [00:05<00:11,  3.69it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=43.26 GB):  26%|██▌       | 15/58 [00:05<00:12,  3.52it/s]Capturing num tokens (num_tokens=2304 avail_mem=43.26 GB):  26%|██▌       | 15/58 [00:05<00:12,  3.52it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=43.26 GB):  28%|██▊       | 16/58 [00:05<00:10,  3.85it/s]Capturing num tokens (num_tokens=2048 avail_mem=43.26 GB):  28%|██▊       | 16/58 [00:05<00:10,  3.85it/s]Capturing num tokens (num_tokens=2048 avail_mem=43.26 GB):  29%|██▉       | 17/58 [00:06<00:09,  4.22it/s]Capturing num tokens (num_tokens=1792 avail_mem=43.26 GB):  29%|██▉       | 17/58 [00:06<00:09,  4.22it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=43.26 GB):  31%|███       | 18/58 [00:06<00:08,  4.57it/s]Capturing num tokens (num_tokens=1536 avail_mem=43.26 GB):  31%|███       | 18/58 [00:06<00:08,  4.57it/s]Capturing num tokens (num_tokens=1536 avail_mem=43.26 GB):  33%|███▎      | 19/58 [00:06<00:07,  4.94it/s]Capturing num tokens (num_tokens=1280 avail_mem=43.26 GB):  33%|███▎      | 19/58 [00:06<00:07,  4.94it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=43.26 GB):  34%|███▍      | 20/58 [00:06<00:07,  5.39it/s]Capturing num tokens (num_tokens=1024 avail_mem=43.26 GB):  34%|███▍      | 20/58 [00:06<00:07,  5.39it/s]Capturing num tokens (num_tokens=1024 avail_mem=43.26 GB):  36%|███▌      | 21/58 [00:06<00:06,  5.89it/s]Capturing num tokens (num_tokens=960 avail_mem=43.25 GB):  36%|███▌      | 21/58 [00:06<00:06,  5.89it/s] 

    Capturing num tokens (num_tokens=960 avail_mem=43.25 GB):  38%|███▊      | 22/58 [00:06<00:05,  6.28it/s]Capturing num tokens (num_tokens=896 avail_mem=43.25 GB):  38%|███▊      | 22/58 [00:06<00:05,  6.28it/s]Capturing num tokens (num_tokens=896 avail_mem=43.25 GB):  40%|███▉      | 23/58 [00:06<00:05,  6.61it/s]Capturing num tokens (num_tokens=832 avail_mem=43.25 GB):  40%|███▉      | 23/58 [00:06<00:05,  6.61it/s]

    Capturing num tokens (num_tokens=832 avail_mem=43.25 GB):  41%|████▏     | 24/58 [00:07<00:04,  6.90it/s]Capturing num tokens (num_tokens=768 avail_mem=43.24 GB):  41%|████▏     | 24/58 [00:07<00:04,  6.90it/s]

    Capturing num tokens (num_tokens=768 avail_mem=43.24 GB):  43%|████▎     | 25/58 [00:07<00:05,  6.16it/s]Capturing num tokens (num_tokens=704 avail_mem=43.24 GB):  43%|████▎     | 25/58 [00:07<00:05,  6.16it/s]Capturing num tokens (num_tokens=704 avail_mem=43.24 GB):  45%|████▍     | 26/58 [00:07<00:05,  6.16it/s]Capturing num tokens (num_tokens=640 avail_mem=43.24 GB):  45%|████▍     | 26/58 [00:07<00:05,  6.16it/s]

    Capturing num tokens (num_tokens=640 avail_mem=43.24 GB):  47%|████▋     | 27/58 [00:07<00:04,  6.67it/s]Capturing num tokens (num_tokens=576 avail_mem=43.23 GB):  47%|████▋     | 27/58 [00:07<00:04,  6.67it/s]Capturing num tokens (num_tokens=576 avail_mem=43.23 GB):  48%|████▊     | 28/58 [00:07<00:04,  7.08it/s]Capturing num tokens (num_tokens=512 avail_mem=43.23 GB):  48%|████▊     | 28/58 [00:07<00:04,  7.08it/s]

    Capturing num tokens (num_tokens=512 avail_mem=43.23 GB):  50%|█████     | 29/58 [00:07<00:03,  7.60it/s]Capturing num tokens (num_tokens=480 avail_mem=43.23 GB):  50%|█████     | 29/58 [00:07<00:03,  7.60it/s]Capturing num tokens (num_tokens=480 avail_mem=43.23 GB):  52%|█████▏    | 30/58 [00:07<00:03,  7.95it/s]Capturing num tokens (num_tokens=448 avail_mem=43.22 GB):  52%|█████▏    | 30/58 [00:07<00:03,  7.95it/s]

    Capturing num tokens (num_tokens=448 avail_mem=43.22 GB):  53%|█████▎    | 31/58 [00:08<00:03,  8.03it/s]Capturing num tokens (num_tokens=416 avail_mem=43.22 GB):  53%|█████▎    | 31/58 [00:08<00:03,  8.03it/s]Capturing num tokens (num_tokens=416 avail_mem=43.22 GB):  55%|█████▌    | 32/58 [00:08<00:03,  8.38it/s]Capturing num tokens (num_tokens=384 avail_mem=43.22 GB):  55%|█████▌    | 32/58 [00:08<00:03,  8.38it/s]

    Capturing num tokens (num_tokens=384 avail_mem=43.22 GB):  57%|█████▋    | 33/58 [00:08<00:02,  8.59it/s]Capturing num tokens (num_tokens=352 avail_mem=43.21 GB):  57%|█████▋    | 33/58 [00:08<00:02,  8.59it/s]Capturing num tokens (num_tokens=352 avail_mem=43.21 GB):  59%|█████▊    | 34/58 [00:08<00:02,  8.79it/s]Capturing num tokens (num_tokens=320 avail_mem=43.21 GB):  59%|█████▊    | 34/58 [00:08<00:02,  8.79it/s]

    Capturing num tokens (num_tokens=320 avail_mem=43.21 GB):  60%|██████    | 35/58 [00:08<00:02,  8.01it/s]Capturing num tokens (num_tokens=288 avail_mem=43.19 GB):  60%|██████    | 35/58 [00:08<00:02,  8.01it/s]Capturing num tokens (num_tokens=288 avail_mem=43.19 GB):  62%|██████▏   | 36/58 [00:08<00:02,  7.43it/s]Capturing num tokens (num_tokens=256 avail_mem=43.18 GB):  62%|██████▏   | 36/58 [00:08<00:02,  7.43it/s]

    Capturing num tokens (num_tokens=256 avail_mem=43.18 GB):  64%|██████▍   | 37/58 [00:08<00:02,  7.42it/s]Capturing num tokens (num_tokens=240 avail_mem=42.68 GB):  64%|██████▍   | 37/58 [00:08<00:02,  7.42it/s]Capturing num tokens (num_tokens=224 avail_mem=42.68 GB):  64%|██████▍   | 37/58 [00:08<00:02,  7.42it/s]Capturing num tokens (num_tokens=208 avail_mem=42.55 GB):  64%|██████▍   | 37/58 [00:08<00:02,  7.42it/s]Capturing num tokens (num_tokens=208 avail_mem=42.55 GB):  69%|██████▉   | 40/58 [00:08<00:01, 12.42it/s]Capturing num tokens (num_tokens=192 avail_mem=42.51 GB):  69%|██████▉   | 40/58 [00:08<00:01, 12.42it/s]Capturing num tokens (num_tokens=176 avail_mem=42.51 GB):  69%|██████▉   | 40/58 [00:08<00:01, 12.42it/s]Capturing num tokens (num_tokens=160 avail_mem=42.51 GB):  69%|██████▉   | 40/58 [00:08<00:01, 12.42it/s]Capturing num tokens (num_tokens=144 avail_mem=42.50 GB):  69%|██████▉   | 40/58 [00:08<00:01, 12.42it/s]

    Capturing num tokens (num_tokens=144 avail_mem=42.50 GB):  76%|███████▌  | 44/58 [00:09<00:00, 15.27it/s]Capturing num tokens (num_tokens=128 avail_mem=60.86 GB):  76%|███████▌  | 44/58 [00:09<00:00, 15.27it/s]Capturing num tokens (num_tokens=112 avail_mem=60.86 GB):  76%|███████▌  | 44/58 [00:09<00:00, 15.27it/s]Capturing num tokens (num_tokens=96 avail_mem=60.85 GB):  76%|███████▌  | 44/58 [00:09<00:00, 15.27it/s] Capturing num tokens (num_tokens=80 avail_mem=60.85 GB):  76%|███████▌  | 44/58 [00:09<00:00, 15.27it/s]Capturing num tokens (num_tokens=64 avail_mem=60.85 GB):  76%|███████▌  | 44/58 [00:09<00:00, 15.27it/s]Capturing num tokens (num_tokens=64 avail_mem=60.85 GB):  84%|████████▍ | 49/58 [00:09<00:00, 21.91it/s]Capturing num tokens (num_tokens=48 avail_mem=60.84 GB):  84%|████████▍ | 49/58 [00:09<00:00, 21.91it/s]Capturing num tokens (num_tokens=32 avail_mem=60.84 GB):  84%|████████▍ | 49/58 [00:09<00:00, 21.91it/s]Capturing num tokens (num_tokens=28 avail_mem=60.84 GB):  84%|████████▍ | 49/58 [00:09<00:00, 21.91it/s]Capturing num tokens (num_tokens=24 avail_mem=60.83 GB):  84%|████████▍ | 49/58 [00:09<00:00, 21.91it/s]

    Capturing num tokens (num_tokens=20 avail_mem=60.83 GB):  84%|████████▍ | 49/58 [00:09<00:00, 21.91it/s]Capturing num tokens (num_tokens=20 avail_mem=60.83 GB):  93%|█████████▎| 54/58 [00:09<00:00, 27.33it/s]Capturing num tokens (num_tokens=16 avail_mem=60.83 GB):  93%|█████████▎| 54/58 [00:09<00:00, 27.33it/s]Capturing num tokens (num_tokens=12 avail_mem=60.82 GB):  93%|█████████▎| 54/58 [00:09<00:00, 27.33it/s]Capturing num tokens (num_tokens=8 avail_mem=60.82 GB):  93%|█████████▎| 54/58 [00:09<00:00, 27.33it/s] Capturing num tokens (num_tokens=4 avail_mem=60.82 GB):  93%|█████████▎| 54/58 [00:09<00:00, 27.33it/s]Capturing num tokens (num_tokens=4 avail_mem=60.82 GB): 100%|██████████| 58/58 [00:09<00:00,  6.15it/s]



<strong style='color: #00008B;'>==== Original Output ====</strong>



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I'll add the two numbers together: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></think><br><br>**Solution:**<br><br>We are asked to find the sum of \(1\) and \(3\).<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the final answer is \(\boxed{4}\).</strong>



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I'll add the two numbers together: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>**Solution:**<br><br>We are asked to find the sum of \(1\) and \(3\).<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the final answer is \(\boxed{4}\).</strong>



```python
llm.shutdown()
```

## Supporting New Reasoning Model Schemas

For future reasoning models, you can implement the reasoning parser as a subclass of `BaseReasoningFormatDetector` in `python/sglang/srt/reasoning_parser.py` and specify the reasoning parser for new reasoning model schemas accordingly.
