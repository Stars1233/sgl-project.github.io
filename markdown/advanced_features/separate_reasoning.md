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


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:172: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-04-03 09:41:24.391 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:41:24] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 09:41:24.391 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:41:24] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 09:41:24.391 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:41:24] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 09:41:24.391 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:41:24] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 09:41:24.392 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:41:24] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Multi-thread loading shards:  50% Completed | 1/2 [00:01<00:01,  1.21s/it]

    Multi-thread loading shards: 100% Completed | 2/2 [00:03<00:00,  1.60s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:03<00:00,  1.54s/it]


    2026-04-03 09:41:33,492 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 09:41:33] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:46,  2.93s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:46,  2.93s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:32,  1.64s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:32,  1.64s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:02,  1.14s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:02,  1.14s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:47,  1.14it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:47,  1.14it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:37,  1.41it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:37,  1.41it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:30,  1.69it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:30,  1.69it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:26,  1.96it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:26,  1.96it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:06<00:24,  2.07it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:06<00:24,  2.07it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:06<00:21,  2.29it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:06<00:21,  2.29it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:06<00:18,  2.64it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:06<00:18,  2.64it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:07<00:16,  2.82it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:07<00:16,  2.82it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:07<00:15,  2.96it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:07<00:15,  2.96it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:07<00:13,  3.22it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:07<00:13,  3.22it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:07<00:12,  3.43it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:07<00:12,  3.43it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:08<00:11,  3.72it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:08<00:11,  3.72it/s]

    Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:08<00:10,  4.02it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:08<00:10,  4.02it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:08<00:09,  4.51it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:08<00:09,  4.51it/s]

    Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:08<00:08,  4.91it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:08<00:08,  4.91it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:08<00:07,  5.40it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:08<00:07,  5.40it/s]

    Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:08<00:06,  5.80it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:08<00:06,  5.80it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:09<00:05,  6.60it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:09<00:05,  6.60it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:09<00:05,  6.60it/s]

    Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:09<00:04,  8.20it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:09<00:04,  8.20it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:09<00:04,  8.20it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:09<00:03,  9.13it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:09<00:03,  9.13it/s]

    Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:09<00:03,  9.13it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:09<00:03, 10.18it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:09<00:03, 10.18it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:09<00:03, 10.18it/s]

    Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:09<00:02, 11.24it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:09<00:02, 11.24it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:09<00:02, 11.24it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:09<00:02, 11.24it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:09<00:01, 13.51it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:09<00:01, 13.51it/s]

    Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:09<00:01, 13.51it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:09<00:01, 14.11it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:09<00:01, 14.11it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:10<00:01, 14.11it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:10<00:01, 14.28it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:10<00:01, 14.28it/s]

    Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:10<00:01, 14.28it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:10<00:01, 14.28it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:10<00:01, 16.87it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:10<00:01, 16.87it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:10<00:01, 16.87it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:10<00:00, 17.36it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:10<00:00, 17.36it/s]

    Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:10<00:00, 17.36it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:10<00:00, 17.24it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:10<00:00, 17.24it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:10<00:00, 17.24it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:10<00:00, 17.36it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:10<00:00, 17.36it/s]

    Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:10<00:00, 17.36it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:10<00:00, 17.36it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:10<00:00, 18.64it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:10<00:00, 18.64it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:10<00:00, 18.64it/s]

    Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:10<00:00, 18.20it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:10<00:00, 18.20it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:10<00:00, 18.20it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:10<00:00, 18.20it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:10<00:00, 19.18it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:10<00:00, 19.18it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:11<00:00, 19.18it/s]

    Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:11<00:00, 19.18it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:11<00:00, 19.18it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:11<00:00, 23.52it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:11<00:00, 23.52it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:11<00:00,  5.21it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=24.68 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=24.68 GB):   2%|▏         | 1/58 [00:00<00:39,  1.46it/s]Capturing num tokens (num_tokens=7680 avail_mem=24.64 GB):   2%|▏         | 1/58 [00:00<00:39,  1.46it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=24.64 GB):   3%|▎         | 2/58 [00:01<00:34,  1.64it/s]Capturing num tokens (num_tokens=7168 avail_mem=24.58 GB):   3%|▎         | 2/58 [00:01<00:34,  1.64it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=24.58 GB):   5%|▌         | 3/58 [00:01<00:34,  1.59it/s]Capturing num tokens (num_tokens=6656 avail_mem=23.35 GB):   5%|▌         | 3/58 [00:01<00:34,  1.59it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=23.35 GB):   7%|▋         | 4/58 [00:02<00:32,  1.69it/s]Capturing num tokens (num_tokens=6144 avail_mem=23.79 GB):   7%|▋         | 4/58 [00:02<00:32,  1.69it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=23.79 GB):   9%|▊         | 5/58 [00:02<00:30,  1.74it/s]Capturing num tokens (num_tokens=5632 avail_mem=24.48 GB):   9%|▊         | 5/58 [00:02<00:30,  1.74it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=24.48 GB):  10%|█         | 6/58 [00:03<00:27,  1.89it/s]Capturing num tokens (num_tokens=5120 avail_mem=23.85 GB):  10%|█         | 6/58 [00:03<00:27,  1.89it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=23.85 GB):  12%|█▏        | 7/58 [00:03<00:25,  2.01it/s]Capturing num tokens (num_tokens=4608 avail_mem=23.90 GB):  12%|█▏        | 7/58 [00:03<00:25,  2.01it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=23.90 GB):  14%|█▍        | 8/58 [00:04<00:23,  2.17it/s]Capturing num tokens (num_tokens=4096 avail_mem=24.62 GB):  14%|█▍        | 8/58 [00:04<00:23,  2.17it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=24.62 GB):  16%|█▌        | 9/58 [00:04<00:20,  2.36it/s]Capturing num tokens (num_tokens=3840 avail_mem=23.96 GB):  16%|█▌        | 9/58 [00:04<00:20,  2.36it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=23.96 GB):  17%|█▋        | 10/58 [00:04<00:18,  2.59it/s]Capturing num tokens (num_tokens=3584 avail_mem=23.96 GB):  17%|█▋        | 10/58 [00:04<00:18,  2.59it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=23.96 GB):  19%|█▉        | 11/58 [00:05<00:16,  2.81it/s]Capturing num tokens (num_tokens=3328 avail_mem=24.61 GB):  19%|█▉        | 11/58 [00:05<00:16,  2.81it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=24.61 GB):  21%|██        | 12/58 [00:05<00:15,  3.03it/s]Capturing num tokens (num_tokens=3072 avail_mem=24.05 GB):  21%|██        | 12/58 [00:05<00:15,  3.03it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=24.05 GB):  22%|██▏       | 13/58 [00:05<00:14,  3.19it/s]Capturing num tokens (num_tokens=2816 avail_mem=24.10 GB):  22%|██▏       | 13/58 [00:05<00:14,  3.19it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=24.10 GB):  24%|██▍       | 14/58 [00:06<00:15,  2.87it/s]Capturing num tokens (num_tokens=2560 avail_mem=24.15 GB):  24%|██▍       | 14/58 [00:06<00:15,  2.87it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=24.15 GB):  26%|██▌       | 15/58 [00:06<00:13,  3.27it/s]Capturing num tokens (num_tokens=2304 avail_mem=24.59 GB):  26%|██▌       | 15/58 [00:06<00:13,  3.27it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=24.59 GB):  28%|██▊       | 16/58 [00:06<00:11,  3.63it/s]Capturing num tokens (num_tokens=2048 avail_mem=24.59 GB):  28%|██▊       | 16/58 [00:06<00:11,  3.63it/s]Capturing num tokens (num_tokens=2048 avail_mem=24.59 GB):  29%|██▉       | 17/58 [00:06<00:10,  4.01it/s]Capturing num tokens (num_tokens=1792 avail_mem=24.20 GB):  29%|██▉       | 17/58 [00:06<00:10,  4.01it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=24.20 GB):  31%|███       | 18/58 [00:06<00:08,  4.51it/s]Capturing num tokens (num_tokens=1536 avail_mem=24.23 GB):  31%|███       | 18/58 [00:06<00:08,  4.51it/s]Capturing num tokens (num_tokens=1536 avail_mem=24.23 GB):  33%|███▎      | 19/58 [00:07<00:07,  4.94it/s]Capturing num tokens (num_tokens=1280 avail_mem=24.58 GB):  33%|███▎      | 19/58 [00:07<00:07,  4.94it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=24.58 GB):  34%|███▍      | 20/58 [00:07<00:06,  5.61it/s]Capturing num tokens (num_tokens=1024 avail_mem=24.24 GB):  34%|███▍      | 20/58 [00:07<00:06,  5.61it/s]Capturing num tokens (num_tokens=1024 avail_mem=24.24 GB):  36%|███▌      | 21/58 [00:07<00:05,  6.41it/s]Capturing num tokens (num_tokens=960 avail_mem=24.56 GB):  36%|███▌      | 21/58 [00:07<00:05,  6.41it/s] 

    Capturing num tokens (num_tokens=960 avail_mem=24.56 GB):  38%|███▊      | 22/58 [00:07<00:05,  6.76it/s]Capturing num tokens (num_tokens=896 avail_mem=24.34 GB):  38%|███▊      | 22/58 [00:07<00:05,  6.76it/s]Capturing num tokens (num_tokens=896 avail_mem=24.34 GB):  40%|███▉      | 23/58 [00:07<00:05,  6.42it/s]Capturing num tokens (num_tokens=832 avail_mem=24.28 GB):  40%|███▉      | 23/58 [00:07<00:05,  6.42it/s]

    Capturing num tokens (num_tokens=832 avail_mem=24.28 GB):  41%|████▏     | 24/58 [00:07<00:04,  7.19it/s]Capturing num tokens (num_tokens=768 avail_mem=24.54 GB):  41%|████▏     | 24/58 [00:07<00:04,  7.19it/s]Capturing num tokens (num_tokens=768 avail_mem=24.54 GB):  43%|████▎     | 25/58 [00:07<00:04,  7.71it/s]Capturing num tokens (num_tokens=704 avail_mem=24.54 GB):  43%|████▎     | 25/58 [00:07<00:04,  7.71it/s]Capturing num tokens (num_tokens=640 avail_mem=24.31 GB):  43%|████▎     | 25/58 [00:07<00:04,  7.71it/s]

    Capturing num tokens (num_tokens=640 avail_mem=24.31 GB):  47%|████▋     | 27/58 [00:07<00:03,  9.50it/s]Capturing num tokens (num_tokens=576 avail_mem=24.50 GB):  47%|████▋     | 27/58 [00:07<00:03,  9.50it/s]Capturing num tokens (num_tokens=576 avail_mem=24.50 GB):  48%|████▊     | 28/58 [00:08<00:03,  9.43it/s]Capturing num tokens (num_tokens=512 avail_mem=24.51 GB):  48%|████▊     | 28/58 [00:08<00:03,  9.43it/s]Capturing num tokens (num_tokens=480 avail_mem=24.46 GB):  48%|████▊     | 28/58 [00:08<00:03,  9.43it/s]

    Capturing num tokens (num_tokens=480 avail_mem=24.46 GB):  52%|█████▏    | 30/58 [00:08<00:02, 10.67it/s]Capturing num tokens (num_tokens=448 avail_mem=24.50 GB):  52%|█████▏    | 30/58 [00:08<00:02, 10.67it/s]Capturing num tokens (num_tokens=416 avail_mem=24.35 GB):  52%|█████▏    | 30/58 [00:08<00:02, 10.67it/s]

    Capturing num tokens (num_tokens=416 avail_mem=24.35 GB):  55%|█████▌    | 32/58 [00:08<00:04,  6.24it/s]Capturing num tokens (num_tokens=384 avail_mem=24.48 GB):  55%|█████▌    | 32/58 [00:08<00:04,  6.24it/s]Capturing num tokens (num_tokens=352 avail_mem=24.48 GB):  55%|█████▌    | 32/58 [00:08<00:04,  6.24it/s]Capturing num tokens (num_tokens=352 avail_mem=24.48 GB):  59%|█████▊    | 34/58 [00:08<00:03,  7.44it/s]Capturing num tokens (num_tokens=320 avail_mem=24.37 GB):  59%|█████▊    | 34/58 [00:08<00:03,  7.44it/s]

    Capturing num tokens (num_tokens=288 avail_mem=24.46 GB):  59%|█████▊    | 34/58 [00:08<00:03,  7.44it/s]Capturing num tokens (num_tokens=288 avail_mem=24.46 GB):  62%|██████▏   | 36/58 [00:09<00:02,  8.78it/s]Capturing num tokens (num_tokens=256 avail_mem=24.45 GB):  62%|██████▏   | 36/58 [00:09<00:02,  8.78it/s]

    Capturing num tokens (num_tokens=240 avail_mem=24.45 GB):  62%|██████▏   | 36/58 [00:09<00:02,  8.78it/s]Capturing num tokens (num_tokens=240 avail_mem=24.45 GB):  66%|██████▌   | 38/58 [00:09<00:02,  8.30it/s]Capturing num tokens (num_tokens=224 avail_mem=24.44 GB):  66%|██████▌   | 38/58 [00:09<00:02,  8.30it/s]Capturing num tokens (num_tokens=208 avail_mem=24.43 GB):  66%|██████▌   | 38/58 [00:09<00:02,  8.30it/s]Capturing num tokens (num_tokens=208 avail_mem=24.43 GB):  69%|██████▉   | 40/58 [00:09<00:01,  9.92it/s]Capturing num tokens (num_tokens=192 avail_mem=24.42 GB):  69%|██████▉   | 40/58 [00:09<00:01,  9.92it/s]

    Capturing num tokens (num_tokens=176 avail_mem=24.42 GB):  69%|██████▉   | 40/58 [00:09<00:01,  9.92it/s]Capturing num tokens (num_tokens=176 avail_mem=24.42 GB):  72%|███████▏  | 42/58 [00:09<00:01, 11.56it/s]Capturing num tokens (num_tokens=160 avail_mem=24.41 GB):  72%|███████▏  | 42/58 [00:09<00:01, 11.56it/s]Capturing num tokens (num_tokens=144 avail_mem=24.40 GB):  72%|███████▏  | 42/58 [00:09<00:01, 11.56it/s]Capturing num tokens (num_tokens=144 avail_mem=24.40 GB):  76%|███████▌  | 44/58 [00:09<00:01, 12.77it/s]Capturing num tokens (num_tokens=128 avail_mem=24.40 GB):  76%|███████▌  | 44/58 [00:09<00:01, 12.77it/s]

    Capturing num tokens (num_tokens=112 avail_mem=24.40 GB):  76%|███████▌  | 44/58 [00:09<00:01, 12.77it/s]Capturing num tokens (num_tokens=96 avail_mem=24.39 GB):  76%|███████▌  | 44/58 [00:09<00:01, 12.77it/s] Capturing num tokens (num_tokens=96 avail_mem=24.39 GB):  81%|████████  | 47/58 [00:09<00:00, 15.76it/s]Capturing num tokens (num_tokens=80 avail_mem=24.38 GB):  81%|████████  | 47/58 [00:09<00:00, 15.76it/s]Capturing num tokens (num_tokens=64 avail_mem=24.36 GB):  81%|████████  | 47/58 [00:09<00:00, 15.76it/s]Capturing num tokens (num_tokens=48 avail_mem=24.37 GB):  81%|████████  | 47/58 [00:09<00:00, 15.76it/s]Capturing num tokens (num_tokens=48 avail_mem=24.37 GB):  86%|████████▌ | 50/58 [00:09<00:00, 18.71it/s]Capturing num tokens (num_tokens=32 avail_mem=24.36 GB):  86%|████████▌ | 50/58 [00:09<00:00, 18.71it/s]

    Capturing num tokens (num_tokens=28 avail_mem=24.36 GB):  86%|████████▌ | 50/58 [00:09<00:00, 18.71it/s]Capturing num tokens (num_tokens=24 avail_mem=24.35 GB):  86%|████████▌ | 50/58 [00:09<00:00, 18.71it/s]Capturing num tokens (num_tokens=24 avail_mem=24.35 GB):  91%|█████████▏| 53/58 [00:10<00:00, 21.04it/s]Capturing num tokens (num_tokens=20 avail_mem=24.34 GB):  91%|█████████▏| 53/58 [00:10<00:00, 21.04it/s]Capturing num tokens (num_tokens=16 avail_mem=24.33 GB):  91%|█████████▏| 53/58 [00:10<00:00, 21.04it/s]Capturing num tokens (num_tokens=12 avail_mem=24.31 GB):  91%|█████████▏| 53/58 [00:10<00:00, 21.04it/s]Capturing num tokens (num_tokens=8 avail_mem=24.32 GB):  91%|█████████▏| 53/58 [00:10<00:00, 21.04it/s] 

    Capturing num tokens (num_tokens=8 avail_mem=24.32 GB):  98%|█████████▊| 57/58 [00:10<00:00, 23.82it/s]Capturing num tokens (num_tokens=4 avail_mem=24.31 GB):  98%|█████████▊| 57/58 [00:10<00:00, 23.82it/s]Capturing num tokens (num_tokens=4 avail_mem=24.31 GB): 100%|██████████| 58/58 [00:10<00:00,  5.69it/s]


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



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of the numbers 1 and 3.<br><br>Next, I add the two numbers together: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>**Solution:**<br><br>We are asked to find the sum of the numbers 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the final answer is \(\boxed{4}\).</strong>


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



<strong style='color: #00008B;'>I need to calculate the sum of 1 and 3.<br><br>First, I add 1 and 3 together.<br><br>The result is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'><br><br>Sure, let's solve the problem step by step:<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Start with the first number:**<br>   \[<br>   1<br>   \]<br><br>2. **Add the second number:**<br>   \[<br>   1 + 3<br>   \]<br><br>3. **Calculate the sum:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Final Answer:**<br>\[<br>\boxed{4}<br>\]</strong>


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



<strong style='color: #00008B;'>I need to calculate the sum of 1 and 3.<br><br>First, I'll identify the numbers involved: 1 and 3.<br><br>Next, I'll add these two numbers together.<br><br>1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.</strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'><br><br>Sure! Let's solve the addition step by step.<br><br>**Problem:**  <br>What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:**  <br>\[<br>\boxed{4}<br>\]</strong>


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



<strong style='color: #00008B;'>First, I recognize that the user is asking for the sum of 1 and 3.<br><br>I need to add these two numbers together.<br><br>Adding 1 and 3 gives a total of 4.<br><br>Therefore, the final answer is 4.<br></think><br><br>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>**Answer:** \(\boxed{4}\)</strong>


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



<strong style='color: #00008B;'>To solve the problem 1 plus 3, I start by identifying the two numbers involved, which are 1 and 3.<br><br>Next, I perform the addition operation by adding these two numbers together.<br><br>Finally, the sum of 1 and 3 is 4.<br></think><br><br>**Solution:**<br><br>We are asked to find the sum of \(1\) and \(3\).<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the answer is \(\boxed{4}\).</strong>


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:328: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      return await dependant.call(**values)



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>To solve the problem 1 plus 3, I start by identifying the two numbers involved, which are 1 and 3.<br><br>Next, I perform the addition operation by adding these two numbers together.<br><br>Finally, the sum of 1 and 3 is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>**Solution:**<br><br>We are asked to find the sum of \(1\) and \(3\).<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the answer is \(\boxed{4}\).</strong>



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


    2026-04-03 09:42:22.908 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:42:22] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 09:42:22.908 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:42:22] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 09:42:22.908 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:42:22] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 09:42:22.908 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:42:22] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 09:42:22.908 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:42:22] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Multi-thread loading shards:  50% Completed | 1/2 [00:01<00:01,  1.23s/it]

    Multi-thread loading shards: 100% Completed | 2/2 [00:03<00:00,  1.57s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:03<00:00,  1.52s/it]


    2026-04-03 09:42:32,109 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 09:42:32] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:53,  3.05s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:53,  3.05s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:40,  1.80s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:40,  1.80s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:01,  1.12s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:01,  1.12s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:44,  1.21it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:44,  1.21it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:34,  1.53it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:34,  1.53it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:27,  1.86it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:27,  1.86it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:22,  2.25it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:22,  2.25it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:05<00:19,  2.51it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:05<00:19,  2.51it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:06<00:18,  2.62it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:06<00:18,  2.62it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:06<00:16,  2.89it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:06<00:16,  2.89it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:06<00:14,  3.16it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:06<00:14,  3.16it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:07<00:14,  3.28it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:07<00:14,  3.28it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:07<00:12,  3.59it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:07<00:12,  3.59it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:07<00:11,  3.72it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:07<00:11,  3.72it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:07<00:10,  4.12it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:07<00:10,  4.12it/s]

    Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:07<00:09,  4.42it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:07<00:09,  4.42it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:07<00:08,  4.94it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:07<00:08,  4.94it/s]

    Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:08<00:07,  5.35it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:08<00:07,  5.35it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:08<00:06,  5.75it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:08<00:06,  5.75it/s]

    Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:08<00:06,  5.75it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:08<00:05,  7.01it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:08<00:05,  7.01it/s] 

    Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:08<00:05,  7.01it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:08<00:04,  8.27it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:08<00:04,  8.27it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:08<00:04,  8.27it/s]

    Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:08<00:03,  9.41it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:08<00:03,  9.41it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:08<00:03,  9.41it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:08<00:02, 10.67it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:08<00:02, 10.67it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:09<00:02, 10.67it/s]

    Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:09<00:02, 12.09it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:09<00:02, 12.09it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:09<00:02, 12.09it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:09<00:02, 12.73it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:09<00:02, 12.73it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:09<00:02, 12.73it/s]

    Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:09<00:01, 14.07it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:09<00:01, 14.07it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:09<00:01, 14.07it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:09<00:01, 14.34it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:09<00:01, 14.34it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:09<00:01, 14.34it/s]

    Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:09<00:01, 15.57it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:09<00:01, 15.57it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:09<00:01, 15.57it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:09<00:01, 15.57it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:09<00:01, 16.95it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:09<00:01, 16.95it/s]

    Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:09<00:01, 16.95it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:09<00:01, 16.95it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:09<00:00, 18.26it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:09<00:00, 18.26it/s]

    Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:09<00:00, 18.26it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:10<00:00, 16.37it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:10<00:00, 16.37it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:10<00:00, 16.37it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:10<00:00, 16.37it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:10<00:00, 17.62it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:10<00:00, 17.62it/s]

    Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:10<00:00, 17.62it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:10<00:00, 17.62it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:10<00:00, 18.93it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:10<00:00, 18.93it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:10<00:00, 18.93it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:10<00:00, 19.01it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:10<00:00, 19.01it/s]

    Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:10<00:00, 19.01it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:10<00:00, 19.01it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:10<00:00, 21.60it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:10<00:00, 21.60it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:10<00:00, 21.60it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:10<00:00,  5.48it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=24.66 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=24.66 GB):   2%|▏         | 1/58 [00:00<00:40,  1.40it/s]Capturing num tokens (num_tokens=7680 avail_mem=24.62 GB):   2%|▏         | 1/58 [00:00<00:40,  1.40it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=24.62 GB):   3%|▎         | 2/58 [00:01<00:34,  1.62it/s]Capturing num tokens (num_tokens=7168 avail_mem=24.61 GB):   3%|▎         | 2/58 [00:01<00:34,  1.62it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=24.61 GB):   5%|▌         | 3/58 [00:01<00:29,  1.87it/s]Capturing num tokens (num_tokens=6656 avail_mem=24.60 GB):   5%|▌         | 3/58 [00:01<00:29,  1.87it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=24.60 GB):   7%|▋         | 4/58 [00:02<00:25,  2.16it/s]Capturing num tokens (num_tokens=6144 avail_mem=24.58 GB):   7%|▋         | 4/58 [00:02<00:25,  2.16it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=24.58 GB):   9%|▊         | 5/58 [00:02<00:23,  2.24it/s]Capturing num tokens (num_tokens=5632 avail_mem=24.58 GB):   9%|▊         | 5/58 [00:02<00:23,  2.24it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=24.58 GB):  10%|█         | 6/58 [00:02<00:19,  2.69it/s]Capturing num tokens (num_tokens=5120 avail_mem=24.57 GB):  10%|█         | 6/58 [00:02<00:19,  2.69it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=24.57 GB):  12%|█▏        | 7/58 [00:03<00:18,  2.74it/s]Capturing num tokens (num_tokens=4608 avail_mem=23.55 GB):  12%|█▏        | 7/58 [00:03<00:18,  2.74it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=23.55 GB):  14%|█▍        | 8/58 [00:03<00:18,  2.77it/s]Capturing num tokens (num_tokens=4096 avail_mem=24.52 GB):  14%|█▍        | 8/58 [00:03<00:18,  2.77it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=24.52 GB):  16%|█▌        | 9/58 [00:03<00:16,  2.91it/s]Capturing num tokens (num_tokens=3840 avail_mem=23.72 GB):  16%|█▌        | 9/58 [00:03<00:16,  2.91it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=23.72 GB):  17%|█▋        | 10/58 [00:04<00:16,  2.97it/s]Capturing num tokens (num_tokens=3584 avail_mem=24.54 GB):  17%|█▋        | 10/58 [00:04<00:16,  2.97it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=24.54 GB):  19%|█▉        | 11/58 [00:04<00:14,  3.21it/s]Capturing num tokens (num_tokens=3328 avail_mem=23.76 GB):  19%|█▉        | 11/58 [00:04<00:14,  3.21it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=23.76 GB):  21%|██        | 12/58 [00:04<00:14,  3.15it/s]Capturing num tokens (num_tokens=3072 avail_mem=24.53 GB):  21%|██        | 12/58 [00:04<00:14,  3.15it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=24.53 GB):  22%|██▏       | 13/58 [00:04<00:13,  3.23it/s]Capturing num tokens (num_tokens=2816 avail_mem=23.80 GB):  22%|██▏       | 13/58 [00:04<00:13,  3.23it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=23.80 GB):  24%|██▍       | 14/58 [00:05<00:12,  3.52it/s]Capturing num tokens (num_tokens=2560 avail_mem=24.52 GB):  24%|██▍       | 14/58 [00:05<00:12,  3.52it/s]Capturing num tokens (num_tokens=2560 avail_mem=24.52 GB):  26%|██▌       | 15/58 [00:05<00:11,  3.91it/s]Capturing num tokens (num_tokens=2304 avail_mem=23.86 GB):  26%|██▌       | 15/58 [00:05<00:11,  3.91it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=23.86 GB):  28%|██▊       | 16/58 [00:05<00:09,  4.21it/s]Capturing num tokens (num_tokens=2048 avail_mem=24.52 GB):  28%|██▊       | 16/58 [00:05<00:09,  4.21it/s]Capturing num tokens (num_tokens=2048 avail_mem=24.52 GB):  29%|██▉       | 17/58 [00:05<00:08,  4.82it/s]Capturing num tokens (num_tokens=1792 avail_mem=24.45 GB):  29%|██▉       | 17/58 [00:05<00:08,  4.82it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=24.45 GB):  31%|███       | 18/58 [00:05<00:07,  5.07it/s]Capturing num tokens (num_tokens=1536 avail_mem=23.91 GB):  31%|███       | 18/58 [00:05<00:07,  5.07it/s]Capturing num tokens (num_tokens=1536 avail_mem=23.91 GB):  33%|███▎      | 19/58 [00:05<00:07,  5.53it/s]Capturing num tokens (num_tokens=1280 avail_mem=24.52 GB):  33%|███▎      | 19/58 [00:05<00:07,  5.53it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=24.52 GB):  34%|███▍      | 20/58 [00:06<00:06,  5.96it/s]Capturing num tokens (num_tokens=1024 avail_mem=23.97 GB):  34%|███▍      | 20/58 [00:06<00:06,  5.96it/s]Capturing num tokens (num_tokens=1024 avail_mem=23.97 GB):  36%|███▌      | 21/58 [00:06<00:05,  6.58it/s]Capturing num tokens (num_tokens=960 avail_mem=24.52 GB):  36%|███▌      | 21/58 [00:06<00:05,  6.58it/s] 

    Capturing num tokens (num_tokens=960 avail_mem=24.52 GB):  38%|███▊      | 22/58 [00:06<00:06,  5.92it/s]Capturing num tokens (num_tokens=896 avail_mem=23.97 GB):  38%|███▊      | 22/58 [00:06<00:06,  5.92it/s]

    Capturing num tokens (num_tokens=896 avail_mem=23.97 GB):  40%|███▉      | 23/58 [00:06<00:06,  5.35it/s]Capturing num tokens (num_tokens=832 avail_mem=24.46 GB):  40%|███▉      | 23/58 [00:06<00:06,  5.35it/s]Capturing num tokens (num_tokens=832 avail_mem=24.46 GB):  41%|████▏     | 24/58 [00:06<00:06,  5.27it/s]Capturing num tokens (num_tokens=768 avail_mem=24.02 GB):  41%|████▏     | 24/58 [00:06<00:06,  5.27it/s]

    Capturing num tokens (num_tokens=768 avail_mem=24.02 GB):  43%|████▎     | 25/58 [00:07<00:06,  4.96it/s]Capturing num tokens (num_tokens=704 avail_mem=24.46 GB):  43%|████▎     | 25/58 [00:07<00:06,  4.96it/s]Capturing num tokens (num_tokens=704 avail_mem=24.46 GB):  45%|████▍     | 26/58 [00:07<00:06,  5.02it/s]Capturing num tokens (num_tokens=640 avail_mem=24.04 GB):  45%|████▍     | 26/58 [00:07<00:06,  5.02it/s]

    Capturing num tokens (num_tokens=640 avail_mem=24.04 GB):  47%|████▋     | 27/58 [00:07<00:06,  4.91it/s]Capturing num tokens (num_tokens=576 avail_mem=24.45 GB):  47%|████▋     | 27/58 [00:07<00:06,  4.91it/s]Capturing num tokens (num_tokens=576 avail_mem=24.45 GB):  48%|████▊     | 28/58 [00:07<00:05,  5.36it/s]Capturing num tokens (num_tokens=512 avail_mem=24.06 GB):  48%|████▊     | 28/58 [00:07<00:05,  5.36it/s]

    Capturing num tokens (num_tokens=480 avail_mem=24.44 GB):  48%|████▊     | 28/58 [00:07<00:05,  5.36it/s]Capturing num tokens (num_tokens=480 avail_mem=24.44 GB):  52%|█████▏    | 30/58 [00:07<00:03,  7.26it/s]Capturing num tokens (num_tokens=448 avail_mem=41.73 GB):  52%|█████▏    | 30/58 [00:07<00:03,  7.26it/s]

    Capturing num tokens (num_tokens=448 avail_mem=41.73 GB):  53%|█████▎    | 31/58 [00:07<00:03,  7.02it/s]Capturing num tokens (num_tokens=416 avail_mem=42.08 GB):  53%|█████▎    | 31/58 [00:07<00:03,  7.02it/s]Capturing num tokens (num_tokens=416 avail_mem=42.08 GB):  55%|█████▌    | 32/58 [00:08<00:03,  7.53it/s]Capturing num tokens (num_tokens=384 avail_mem=41.75 GB):  55%|█████▌    | 32/58 [00:08<00:03,  7.53it/s]Capturing num tokens (num_tokens=352 avail_mem=42.07 GB):  55%|█████▌    | 32/58 [00:08<00:03,  7.53it/s]

    Capturing num tokens (num_tokens=352 avail_mem=42.07 GB):  59%|█████▊    | 34/58 [00:08<00:02,  9.38it/s]Capturing num tokens (num_tokens=320 avail_mem=42.07 GB):  59%|█████▊    | 34/58 [00:08<00:02,  9.38it/s]Capturing num tokens (num_tokens=288 avail_mem=41.79 GB):  59%|█████▊    | 34/58 [00:08<00:02,  9.38it/s]Capturing num tokens (num_tokens=288 avail_mem=41.79 GB):  62%|██████▏   | 36/58 [00:08<00:02, 10.87it/s]Capturing num tokens (num_tokens=256 avail_mem=41.79 GB):  62%|██████▏   | 36/58 [00:08<00:02, 10.87it/s]Capturing num tokens (num_tokens=240 avail_mem=42.06 GB):  62%|██████▏   | 36/58 [00:08<00:02, 10.87it/s]

    Capturing num tokens (num_tokens=240 avail_mem=42.06 GB):  66%|██████▌   | 38/58 [00:08<00:01, 11.89it/s]Capturing num tokens (num_tokens=224 avail_mem=42.06 GB):  66%|██████▌   | 38/58 [00:08<00:01, 11.89it/s]Capturing num tokens (num_tokens=208 avail_mem=41.83 GB):  66%|██████▌   | 38/58 [00:08<00:01, 11.89it/s]Capturing num tokens (num_tokens=208 avail_mem=41.83 GB):  69%|██████▉   | 40/58 [00:08<00:01, 13.37it/s]Capturing num tokens (num_tokens=192 avail_mem=42.05 GB):  69%|██████▉   | 40/58 [00:08<00:01, 13.37it/s]Capturing num tokens (num_tokens=176 avail_mem=41.85 GB):  69%|██████▉   | 40/58 [00:08<00:01, 13.37it/s]

    Capturing num tokens (num_tokens=176 avail_mem=41.85 GB):  72%|███████▏  | 42/58 [00:08<00:01, 14.74it/s]Capturing num tokens (num_tokens=160 avail_mem=42.04 GB):  72%|███████▏  | 42/58 [00:08<00:01, 14.74it/s]Capturing num tokens (num_tokens=144 avail_mem=42.03 GB):  72%|███████▏  | 42/58 [00:08<00:01, 14.74it/s]Capturing num tokens (num_tokens=144 avail_mem=42.03 GB):  76%|███████▌  | 44/58 [00:08<00:00, 15.12it/s]Capturing num tokens (num_tokens=128 avail_mem=42.05 GB):  76%|███████▌  | 44/58 [00:08<00:00, 15.12it/s]Capturing num tokens (num_tokens=112 avail_mem=41.93 GB):  76%|███████▌  | 44/58 [00:08<00:00, 15.12it/s]

    Capturing num tokens (num_tokens=96 avail_mem=42.03 GB):  76%|███████▌  | 44/58 [00:08<00:00, 15.12it/s] Capturing num tokens (num_tokens=96 avail_mem=42.03 GB):  81%|████████  | 47/58 [00:08<00:00, 17.21it/s]Capturing num tokens (num_tokens=80 avail_mem=42.02 GB):  81%|████████  | 47/58 [00:08<00:00, 17.21it/s]Capturing num tokens (num_tokens=64 avail_mem=42.02 GB):  81%|████████  | 47/58 [00:09<00:00, 17.21it/s]Capturing num tokens (num_tokens=64 avail_mem=42.02 GB):  84%|████████▍ | 49/58 [00:09<00:00, 17.76it/s]Capturing num tokens (num_tokens=48 avail_mem=41.92 GB):  84%|████████▍ | 49/58 [00:09<00:00, 17.76it/s]Capturing num tokens (num_tokens=32 avail_mem=41.99 GB):  84%|████████▍ | 49/58 [00:09<00:00, 17.76it/s]

    Capturing num tokens (num_tokens=28 avail_mem=42.00 GB):  84%|████████▍ | 49/58 [00:09<00:00, 17.76it/s]Capturing num tokens (num_tokens=28 avail_mem=42.00 GB):  90%|████████▉ | 52/58 [00:09<00:00, 19.39it/s]Capturing num tokens (num_tokens=24 avail_mem=41.99 GB):  90%|████████▉ | 52/58 [00:09<00:00, 19.39it/s]Capturing num tokens (num_tokens=20 avail_mem=41.98 GB):  90%|████████▉ | 52/58 [00:09<00:00, 19.39it/s]Capturing num tokens (num_tokens=16 avail_mem=41.98 GB):  90%|████████▉ | 52/58 [00:09<00:00, 19.39it/s]Capturing num tokens (num_tokens=16 avail_mem=41.98 GB):  95%|█████████▍| 55/58 [00:09<00:00, 20.55it/s]Capturing num tokens (num_tokens=12 avail_mem=41.97 GB):  95%|█████████▍| 55/58 [00:09<00:00, 20.55it/s]

    Capturing num tokens (num_tokens=8 avail_mem=41.91 GB):  95%|█████████▍| 55/58 [00:09<00:00, 20.55it/s] Capturing num tokens (num_tokens=4 avail_mem=41.95 GB):  95%|█████████▍| 55/58 [00:09<00:00, 20.55it/s]Capturing num tokens (num_tokens=4 avail_mem=41.95 GB): 100%|██████████| 58/58 [00:09<00:00, 22.15it/s]Capturing num tokens (num_tokens=4 avail_mem=41.95 GB): 100%|██████████| 58/58 [00:09<00:00,  6.14it/s]



<strong style='color: #00008B;'>==== Original Output ====</strong>



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I add the two numbers together: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></think><br><br>**Solution:**<br><br>We are asked to find the sum of \(1\) and \(3\).<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the final answer is \(\boxed{4}\).</strong>



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I add the two numbers together: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>**Solution:**<br><br>We are asked to find the sum of \(1\) and \(3\).<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the final answer is \(\boxed{4}\).</strong>



```python
llm.shutdown()
```

## Supporting New Reasoning Model Schemas

For future reasoning models, you can implement the reasoning parser as a subclass of `BaseReasoningFormatDetector` in `python/sglang/srt/reasoning_parser.py` and specify the reasoning parser for new reasoning model schemas accordingly.
