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


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-03-27 23:10:21.871 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 23:10:21] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 23:10:21.871 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 23:10:21] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 23:10:21.871 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 23:10:21] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 23:10:21.871 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 23:10:21] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 23:10:21.871 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 23:10:21] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:01<00:01,  1.03s/it]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.41s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.35s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:01,  3.18s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:01,  3.18s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:04<01:48,  1.94s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:04<01:48,  1.94s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:08,  1.25s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:08,  1.25s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:05<00:49,  1.10it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:05<00:49,  1.10it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:35,  1.50it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:35,  1.50it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:29,  1.78it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:29,  1.78it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:24,  2.08it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:24,  2.08it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:06<00:20,  2.47it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:06<00:20,  2.47it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:06<00:17,  2.87it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:06<00:17,  2.87it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:06<00:15,  3.08it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:06<00:15,  3.08it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:07<00:14,  3.16it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:07<00:14,  3.16it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:07<00:14,  3.24it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:07<00:14,  3.24it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:07<00:12,  3.49it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:07<00:12,  3.49it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:07<00:11,  3.89it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:07<00:11,  3.89it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:07<00:10,  4.07it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:07<00:10,  4.07it/s]

    Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:08<00:09,  4.31it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:08<00:09,  4.31it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:08<00:08,  4.77it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:08<00:08,  4.77it/s]

    Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:08<00:07,  5.50it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:08<00:07,  5.50it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:08<00:06,  5.98it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:08<00:06,  5.98it/s]

    Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:08<00:05,  6.52it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:08<00:05,  6.52it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:08<00:05,  6.52it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:08<00:04,  8.00it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:08<00:04,  8.00it/s]

    Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:08<00:04,  8.00it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:09<00:03,  9.19it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:09<00:03,  9.19it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:09<00:03,  9.19it/s]

    Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:09<00:03,  9.72it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:09<00:03,  9.72it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:09<00:03,  9.72it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:09<00:02, 10.66it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:09<00:02, 10.66it/s]

    Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:09<00:02, 10.66it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:09<00:02, 12.24it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:09<00:02, 12.24it/s]Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:09<00:02, 12.24it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:09<00:01, 13.03it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:09<00:01, 13.03it/s]

    Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:09<00:01, 13.03it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:09<00:01, 13.52it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:09<00:01, 13.52it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:09<00:01, 13.52it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:09<00:01, 13.92it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:09<00:01, 13.92it/s]

    Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:09<00:01, 13.92it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:09<00:01, 15.26it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:09<00:01, 15.26it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:10<00:01, 15.26it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:10<00:01, 16.00it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:10<00:01, 16.00it/s]

    Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:10<00:01, 16.00it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:10<00:00, 16.47it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:10<00:00, 16.47it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:10<00:00, 16.47it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:10<00:00, 16.95it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:10<00:00, 16.95it/s]

    Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:10<00:00, 16.95it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:10<00:00, 17.63it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:10<00:00, 17.63it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:10<00:00, 17.63it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:10<00:00, 16.65it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:10<00:00, 16.65it/s]

    Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:10<00:00, 16.65it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:10<00:00, 16.65it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:10<00:00, 18.58it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:10<00:00, 18.58it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:10<00:00, 18.58it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:10<00:00, 18.60it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:10<00:00, 18.60it/s]

    Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:10<00:00, 18.60it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:10<00:00, 18.60it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:10<00:00, 19.51it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:10<00:00, 19.51it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:10<00:00, 19.51it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:11<00:00,  5.27it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=24.14 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=24.14 GB):   2%|▏         | 1/58 [00:00<00:46,  1.24it/s]Capturing num tokens (num_tokens=7680 avail_mem=24.10 GB):   2%|▏         | 1/58 [00:00<00:46,  1.24it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=24.10 GB):   3%|▎         | 2/58 [00:01<00:40,  1.38it/s]Capturing num tokens (num_tokens=7168 avail_mem=24.28 GB):   3%|▎         | 2/58 [00:01<00:40,  1.38it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=24.28 GB):   5%|▌         | 3/58 [00:02<00:36,  1.49it/s]Capturing num tokens (num_tokens=6656 avail_mem=24.18 GB):   5%|▌         | 3/58 [00:02<00:36,  1.49it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=24.18 GB):   7%|▋         | 4/58 [00:02<00:31,  1.72it/s]Capturing num tokens (num_tokens=6144 avail_mem=24.54 GB):   7%|▋         | 4/58 [00:02<00:31,  1.72it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=24.54 GB):   9%|▊         | 5/58 [00:02<00:28,  1.85it/s]Capturing num tokens (num_tokens=5632 avail_mem=24.54 GB):   9%|▊         | 5/58 [00:02<00:28,  1.85it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=24.54 GB):  10%|█         | 6/58 [00:03<00:24,  2.13it/s]Capturing num tokens (num_tokens=5120 avail_mem=24.53 GB):  10%|█         | 6/58 [00:03<00:24,  2.13it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=24.53 GB):  12%|█▏        | 7/58 [00:03<00:24,  2.10it/s]Capturing num tokens (num_tokens=4608 avail_mem=24.52 GB):  12%|█▏        | 7/58 [00:03<00:24,  2.10it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=24.52 GB):  14%|█▍        | 8/58 [00:04<00:21,  2.35it/s]Capturing num tokens (num_tokens=4096 avail_mem=24.47 GB):  14%|█▍        | 8/58 [00:04<00:21,  2.35it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=24.47 GB):  16%|█▌        | 9/58 [00:04<00:17,  2.76it/s]Capturing num tokens (num_tokens=3840 avail_mem=24.40 GB):  16%|█▌        | 9/58 [00:04<00:17,  2.76it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=24.40 GB):  17%|█▋        | 10/58 [00:04<00:15,  3.16it/s]Capturing num tokens (num_tokens=3584 avail_mem=24.49 GB):  17%|█▋        | 10/58 [00:04<00:15,  3.16it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=24.49 GB):  19%|█▉        | 11/58 [00:04<00:13,  3.53it/s]Capturing num tokens (num_tokens=3328 avail_mem=24.48 GB):  19%|█▉        | 11/58 [00:04<00:13,  3.53it/s]Capturing num tokens (num_tokens=3328 avail_mem=24.48 GB):  21%|██        | 12/58 [00:04<00:11,  4.02it/s]Capturing num tokens (num_tokens=3072 avail_mem=24.44 GB):  21%|██        | 12/58 [00:04<00:11,  4.02it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=24.44 GB):  22%|██▏       | 13/58 [00:05<00:10,  4.36it/s]Capturing num tokens (num_tokens=2816 avail_mem=24.45 GB):  22%|██▏       | 13/58 [00:05<00:10,  4.36it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=24.45 GB):  24%|██▍       | 14/58 [00:05<00:11,  3.88it/s]Capturing num tokens (num_tokens=2560 avail_mem=24.43 GB):  24%|██▍       | 14/58 [00:05<00:11,  3.88it/s]Capturing num tokens (num_tokens=2560 avail_mem=24.43 GB):  26%|██▌       | 15/58 [00:05<00:09,  4.45it/s]Capturing num tokens (num_tokens=2304 avail_mem=24.43 GB):  26%|██▌       | 15/58 [00:05<00:09,  4.45it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=24.43 GB):  28%|██▊       | 16/58 [00:05<00:08,  5.14it/s]Capturing num tokens (num_tokens=2048 avail_mem=24.42 GB):  28%|██▊       | 16/58 [00:05<00:08,  5.14it/s]Capturing num tokens (num_tokens=2048 avail_mem=24.42 GB):  29%|██▉       | 17/58 [00:05<00:06,  6.00it/s]Capturing num tokens (num_tokens=1792 avail_mem=24.37 GB):  29%|██▉       | 17/58 [00:05<00:06,  6.00it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=24.37 GB):  31%|███       | 18/58 [00:05<00:05,  6.68it/s]Capturing num tokens (num_tokens=1536 avail_mem=24.40 GB):  31%|███       | 18/58 [00:05<00:05,  6.68it/s]Capturing num tokens (num_tokens=1280 avail_mem=24.37 GB):  31%|███       | 18/58 [00:06<00:05,  6.68it/s]Capturing num tokens (num_tokens=1280 avail_mem=24.37 GB):  34%|███▍      | 20/58 [00:06<00:04,  8.30it/s]Capturing num tokens (num_tokens=1024 avail_mem=24.38 GB):  34%|███▍      | 20/58 [00:06<00:04,  8.30it/s]

    Capturing num tokens (num_tokens=960 avail_mem=24.36 GB):  34%|███▍      | 20/58 [00:06<00:04,  8.30it/s] Capturing num tokens (num_tokens=960 avail_mem=24.36 GB):  38%|███▊      | 22/58 [00:06<00:03,  9.80it/s]Capturing num tokens (num_tokens=896 avail_mem=24.35 GB):  38%|███▊      | 22/58 [00:06<00:03,  9.80it/s]Capturing num tokens (num_tokens=832 avail_mem=24.34 GB):  38%|███▊      | 22/58 [00:06<00:03,  9.80it/s]

    Capturing num tokens (num_tokens=832 avail_mem=24.34 GB):  41%|████▏     | 24/58 [00:06<00:03, 10.98it/s]Capturing num tokens (num_tokens=768 avail_mem=24.32 GB):  41%|████▏     | 24/58 [00:06<00:03, 10.98it/s]Capturing num tokens (num_tokens=704 avail_mem=24.31 GB):  41%|████▏     | 24/58 [00:06<00:03, 10.98it/s]Capturing num tokens (num_tokens=704 avail_mem=24.31 GB):  45%|████▍     | 26/58 [00:06<00:02, 12.83it/s]Capturing num tokens (num_tokens=640 avail_mem=24.30 GB):  45%|████▍     | 26/58 [00:06<00:02, 12.83it/s]Capturing num tokens (num_tokens=576 avail_mem=24.29 GB):  45%|████▍     | 26/58 [00:06<00:02, 12.83it/s]

    Capturing num tokens (num_tokens=576 avail_mem=24.29 GB):  48%|████▊     | 28/58 [00:06<00:02, 14.21it/s]Capturing num tokens (num_tokens=512 avail_mem=24.30 GB):  48%|████▊     | 28/58 [00:06<00:02, 14.21it/s]Capturing num tokens (num_tokens=480 avail_mem=24.29 GB):  48%|████▊     | 28/58 [00:06<00:02, 14.21it/s]Capturing num tokens (num_tokens=448 avail_mem=24.29 GB):  48%|████▊     | 28/58 [00:06<00:02, 14.21it/s]Capturing num tokens (num_tokens=448 avail_mem=24.29 GB):  53%|█████▎    | 31/58 [00:06<00:01, 16.40it/s]Capturing num tokens (num_tokens=416 avail_mem=24.28 GB):  53%|█████▎    | 31/58 [00:06<00:01, 16.40it/s]Capturing num tokens (num_tokens=384 avail_mem=24.27 GB):  53%|█████▎    | 31/58 [00:06<00:01, 16.40it/s]

    Capturing num tokens (num_tokens=352 avail_mem=24.26 GB):  53%|█████▎    | 31/58 [00:06<00:01, 16.40it/s]Capturing num tokens (num_tokens=352 avail_mem=24.26 GB):  59%|█████▊    | 34/58 [00:06<00:01, 17.97it/s]Capturing num tokens (num_tokens=320 avail_mem=24.25 GB):  59%|█████▊    | 34/58 [00:06<00:01, 17.97it/s]Capturing num tokens (num_tokens=288 avail_mem=24.24 GB):  59%|█████▊    | 34/58 [00:06<00:01, 17.97it/s]Capturing num tokens (num_tokens=256 avail_mem=24.23 GB):  59%|█████▊    | 34/58 [00:06<00:01, 17.97it/s]Capturing num tokens (num_tokens=256 avail_mem=24.23 GB):  64%|██████▍   | 37/58 [00:07<00:01, 19.03it/s]Capturing num tokens (num_tokens=240 avail_mem=24.22 GB):  64%|██████▍   | 37/58 [00:07<00:01, 19.03it/s]

    Capturing num tokens (num_tokens=224 avail_mem=24.21 GB):  64%|██████▍   | 37/58 [00:07<00:01, 19.03it/s]Capturing num tokens (num_tokens=224 avail_mem=24.21 GB):  67%|██████▋   | 39/58 [00:07<00:01, 15.96it/s]Capturing num tokens (num_tokens=208 avail_mem=24.21 GB):  67%|██████▋   | 39/58 [00:07<00:01, 15.96it/s]Capturing num tokens (num_tokens=192 avail_mem=24.20 GB):  67%|██████▋   | 39/58 [00:07<00:01, 15.96it/s]Capturing num tokens (num_tokens=176 avail_mem=24.20 GB):  67%|██████▋   | 39/58 [00:07<00:01, 15.96it/s]Capturing num tokens (num_tokens=160 avail_mem=24.20 GB):  67%|██████▋   | 39/58 [00:07<00:01, 15.96it/s]Capturing num tokens (num_tokens=160 avail_mem=24.20 GB):  74%|███████▍  | 43/58 [00:07<00:00, 20.94it/s]Capturing num tokens (num_tokens=144 avail_mem=24.19 GB):  74%|███████▍  | 43/58 [00:07<00:00, 20.94it/s]Capturing num tokens (num_tokens=128 avail_mem=24.20 GB):  74%|███████▍  | 43/58 [00:07<00:00, 20.94it/s]

    Capturing num tokens (num_tokens=112 avail_mem=24.20 GB):  74%|███████▍  | 43/58 [00:07<00:00, 20.94it/s]Capturing num tokens (num_tokens=96 avail_mem=24.19 GB):  74%|███████▍  | 43/58 [00:07<00:00, 20.94it/s] Capturing num tokens (num_tokens=96 avail_mem=24.19 GB):  81%|████████  | 47/58 [00:07<00:00, 25.25it/s]Capturing num tokens (num_tokens=80 avail_mem=24.19 GB):  81%|████████  | 47/58 [00:07<00:00, 25.25it/s]Capturing num tokens (num_tokens=64 avail_mem=24.19 GB):  81%|████████  | 47/58 [00:07<00:00, 25.25it/s]Capturing num tokens (num_tokens=48 avail_mem=24.18 GB):  81%|████████  | 47/58 [00:07<00:00, 25.25it/s]Capturing num tokens (num_tokens=32 avail_mem=24.18 GB):  81%|████████  | 47/58 [00:07<00:00, 25.25it/s]Capturing num tokens (num_tokens=32 avail_mem=24.18 GB):  88%|████████▊ | 51/58 [00:07<00:00, 28.49it/s]Capturing num tokens (num_tokens=28 avail_mem=24.18 GB):  88%|████████▊ | 51/58 [00:07<00:00, 28.49it/s]Capturing num tokens (num_tokens=24 avail_mem=24.17 GB):  88%|████████▊ | 51/58 [00:07<00:00, 28.49it/s]

    Capturing num tokens (num_tokens=20 avail_mem=24.17 GB):  88%|████████▊ | 51/58 [00:07<00:00, 28.49it/s]Capturing num tokens (num_tokens=16 avail_mem=24.17 GB):  88%|████████▊ | 51/58 [00:07<00:00, 28.49it/s]Capturing num tokens (num_tokens=12 avail_mem=24.16 GB):  88%|████████▊ | 51/58 [00:07<00:00, 28.49it/s]Capturing num tokens (num_tokens=12 avail_mem=24.16 GB):  97%|█████████▋| 56/58 [00:07<00:00, 32.01it/s]Capturing num tokens (num_tokens=8 avail_mem=24.16 GB):  97%|█████████▋| 56/58 [00:07<00:00, 32.01it/s] Capturing num tokens (num_tokens=4 avail_mem=24.16 GB):  97%|█████████▋| 56/58 [00:07<00:00, 32.01it/s]Capturing num tokens (num_tokens=4 avail_mem=24.16 GB): 100%|██████████| 58/58 [00:07<00:00,  7.51it/s]


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



<strong style='color: #00008B;'>First, I identify the two numbers that need to be added: 1 and 3.<br><br>Next, I perform the addition operation by combining these two numbers.<br><br>Finally, I calculate the sum to determine the result.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>Sure! Let's solve the addition step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Perform the addition:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>3. **Final Answer:**<br>   \[<br>   \boxed{4}<br>   \]</strong>


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



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I perform the addition: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'><br><br>**Solution:**<br><br>We need to calculate the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the final answer is \(\boxed{4}\).</strong>


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



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of the numbers 1 and 3.<br><br>Next, I add the two numbers together: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.</strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'><br><br>**Solution:**<br><br>We are asked to find the sum of the numbers 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the final answer is \(\boxed{4}\).</strong>


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



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I add the two numbers together: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></think><br><br>Sure! Let's solve the problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**  <br>   We have two numbers, 1 and 3.<br><br>2. **Perform the addition:**  <br>   \[<br>   1 + 3 = 4<br>   \]<br><br>3. **Write the final answer:**  <br>   \[<br>   \boxed{4}<br>   \]<br><br>**Answer:** \(\boxed{4}\)</strong>


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



<strong style='color: #00008B;'>First, I identify the two numbers in the problem, which are 1 and 3.<br><br>Next, I add these two numbers together.<br><br>Finally, I calculate that 1 plus 3 equals 4.<br></think><br><br>Sure! Let's solve the problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:** \(\boxed{4}\)</strong>


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:328: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      return await dependant.call(**values)



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>First, I identify the two numbers in the problem, which are 1 and 3.<br><br>Next, I add these two numbers together.<br><br>Finally, I calculate that 1 plus 3 equals 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>Sure! Let's solve the problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:** \(\boxed{4}\)</strong>



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


    2026-03-27 23:11:18.631 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 23:11:18] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 23:11:18.631 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 23:11:18] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 23:11:18.631 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 23:11:18] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 23:11:18.631 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 23:11:18] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 23:11:18.631 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 23:11:18] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:01<00:01,  1.03s/it]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.24s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.21s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:01,  3.18s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:01,  3.18s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:04<01:43,  1.85s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:04<01:43,  1.85s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:09,  1.26s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:09,  1.26s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:05<00:53,  1.00it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:05<00:53,  1.00it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:44,  1.20it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:44,  1.20it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:06<00:37,  1.39it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:06<00:37,  1.39it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:06<00:31,  1.60it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:06<00:31,  1.60it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:07<00:27,  1.79it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:07<00:27,  1.79it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:07<00:24,  2.03it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:07<00:24,  2.03it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:07<00:21,  2.26it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:07<00:21,  2.26it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:08<00:18,  2.48it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:08<00:18,  2.48it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:08<00:16,  2.74it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:08<00:16,  2.74it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:08<00:15,  2.99it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:08<00:15,  2.99it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:08<00:13,  3.26it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:08<00:13,  3.26it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:09<00:12,  3.57it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:09<00:12,  3.57it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:09<00:10,  3.96it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:09<00:10,  3.96it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:09<00:09,  4.34it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:09<00:09,  4.34it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:09<00:08,  4.75it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:09<00:08,  4.75it/s]

    Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:09<00:07,  5.25it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:09<00:07,  5.25it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:09<00:06,  5.71it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:09<00:06,  5.71it/s]

    Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:10<00:05,  6.45it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:10<00:05,  6.45it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:10<00:05,  6.45it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:10<00:04,  7.79it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:10<00:04,  7.79it/s]

    Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:10<00:04,  7.79it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:10<00:03,  8.82it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:10<00:03,  8.82it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:10<00:03,  8.82it/s]

    Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:10<00:03,  9.87it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:10<00:03,  9.87it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:10<00:03,  9.87it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:10<00:02, 10.78it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:10<00:02, 10.78it/s]

    Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:10<00:02, 10.78it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:10<00:02, 12.07it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:10<00:02, 12.07it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:10<00:02, 12.07it/s]

    Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:11<00:02, 12.11it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:11<00:02, 12.11it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:11<00:02, 12.11it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:11<00:01, 13.30it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:11<00:01, 13.30it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:11<00:01, 13.30it/s]

    Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:11<00:01, 13.86it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:11<00:01, 13.86it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:11<00:01, 13.86it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:11<00:01, 15.10it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:11<00:01, 15.10it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:11<00:01, 15.10it/s]

    Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:11<00:01, 15.69it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:11<00:01, 15.69it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:11<00:01, 15.69it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:11<00:00, 16.12it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:11<00:00, 16.12it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:11<00:00, 16.12it/s]

    Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:11<00:00, 16.81it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:11<00:00, 16.81it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:11<00:00, 16.81it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:11<00:00, 17.06it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:11<00:00, 17.06it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:11<00:00, 17.06it/s]

    Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:11<00:00, 17.04it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:11<00:00, 17.04it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:12<00:00, 17.04it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:12<00:00, 17.23it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:12<00:00, 17.23it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:12<00:00, 17.23it/s]

    Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:12<00:00, 17.23it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:12<00:00, 19.50it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:12<00:00, 19.50it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:12<00:00, 19.50it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:12<00:00, 19.50it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:12<00:00, 20.61it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:12<00:00, 20.61it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:12<00:00,  4.69it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=23.79 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=23.79 GB):   2%|▏         | 1/58 [00:00<00:47,  1.21it/s]Capturing num tokens (num_tokens=7680 avail_mem=23.75 GB):   2%|▏         | 1/58 [00:00<00:47,  1.21it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=23.75 GB):   3%|▎         | 2/58 [00:01<00:41,  1.36it/s]Capturing num tokens (num_tokens=7168 avail_mem=23.75 GB):   3%|▎         | 2/58 [00:01<00:41,  1.36it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=23.75 GB):   5%|▌         | 3/58 [00:02<00:37,  1.46it/s]Capturing num tokens (num_tokens=6656 avail_mem=22.72 GB):   5%|▌         | 3/58 [00:02<00:37,  1.46it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=22.72 GB):   7%|▋         | 4/58 [00:02<00:36,  1.46it/s]Capturing num tokens (num_tokens=6144 avail_mem=23.73 GB):   7%|▋         | 4/58 [00:02<00:36,  1.46it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=23.73 GB):   9%|▊         | 5/58 [00:03<00:35,  1.48it/s]Capturing num tokens (num_tokens=5632 avail_mem=22.91 GB):   9%|▊         | 5/58 [00:03<00:35,  1.48it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=22.91 GB):  10%|█         | 6/58 [00:04<00:32,  1.59it/s]Capturing num tokens (num_tokens=5120 avail_mem=23.74 GB):  10%|█         | 6/58 [00:04<00:32,  1.59it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=23.74 GB):  12%|█▏        | 7/58 [00:04<00:30,  1.68it/s]Capturing num tokens (num_tokens=4608 avail_mem=22.98 GB):  12%|█▏        | 7/58 [00:04<00:30,  1.68it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=22.98 GB):  14%|█▍        | 8/58 [00:05<00:28,  1.78it/s]Capturing num tokens (num_tokens=4096 avail_mem=23.04 GB):  14%|█▍        | 8/58 [00:05<00:28,  1.78it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=23.04 GB):  16%|█▌        | 9/58 [00:05<00:26,  1.87it/s]Capturing num tokens (num_tokens=3840 avail_mem=23.76 GB):  16%|█▌        | 9/58 [00:05<00:26,  1.87it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=23.76 GB):  17%|█▋        | 10/58 [00:05<00:23,  2.01it/s]Capturing num tokens (num_tokens=3584 avail_mem=23.10 GB):  17%|█▋        | 10/58 [00:05<00:23,  2.01it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=23.10 GB):  19%|█▉        | 11/58 [00:06<00:22,  2.10it/s]Capturing num tokens (num_tokens=3328 avail_mem=23.76 GB):  19%|█▉        | 11/58 [00:06<00:22,  2.10it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=23.76 GB):  21%|██        | 12/58 [00:06<00:20,  2.21it/s]Capturing num tokens (num_tokens=3072 avail_mem=23.15 GB):  21%|██        | 12/58 [00:06<00:20,  2.21it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=23.15 GB):  22%|██▏       | 13/58 [00:07<00:19,  2.36it/s]Capturing num tokens (num_tokens=2816 avail_mem=23.22 GB):  22%|██▏       | 13/58 [00:07<00:19,  2.36it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=23.22 GB):  24%|██▍       | 14/58 [00:07<00:17,  2.50it/s]Capturing num tokens (num_tokens=2560 avail_mem=23.77 GB):  24%|██▍       | 14/58 [00:07<00:17,  2.50it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=23.77 GB):  26%|██▌       | 15/58 [00:07<00:16,  2.67it/s]Capturing num tokens (num_tokens=2304 avail_mem=23.27 GB):  26%|██▌       | 15/58 [00:07<00:16,  2.67it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=23.27 GB):  28%|██▊       | 16/58 [00:08<00:14,  2.90it/s]Capturing num tokens (num_tokens=2048 avail_mem=23.33 GB):  28%|██▊       | 16/58 [00:08<00:14,  2.90it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=23.33 GB):  29%|██▉       | 17/58 [00:08<00:13,  3.05it/s]Capturing num tokens (num_tokens=1792 avail_mem=23.78 GB):  29%|██▉       | 17/58 [00:08<00:13,  3.05it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=23.78 GB):  31%|███       | 18/58 [00:08<00:12,  3.26it/s]Capturing num tokens (num_tokens=1536 avail_mem=23.35 GB):  31%|███       | 18/58 [00:08<00:12,  3.26it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=23.35 GB):  33%|███▎      | 19/58 [00:08<00:11,  3.47it/s]Capturing num tokens (num_tokens=1280 avail_mem=23.77 GB):  33%|███▎      | 19/58 [00:08<00:11,  3.47it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=23.77 GB):  34%|███▍      | 20/58 [00:09<00:10,  3.69it/s]Capturing num tokens (num_tokens=1024 avail_mem=23.39 GB):  34%|███▍      | 20/58 [00:09<00:10,  3.69it/s]Capturing num tokens (num_tokens=1024 avail_mem=23.39 GB):  36%|███▌      | 21/58 [00:09<00:09,  4.10it/s]Capturing num tokens (num_tokens=960 avail_mem=23.76 GB):  36%|███▌      | 21/58 [00:09<00:09,  4.10it/s] 

    Capturing num tokens (num_tokens=960 avail_mem=23.76 GB):  38%|███▊      | 22/58 [00:09<00:08,  4.18it/s]Capturing num tokens (num_tokens=896 avail_mem=23.76 GB):  38%|███▊      | 22/58 [00:09<00:08,  4.18it/s]Capturing num tokens (num_tokens=896 avail_mem=23.76 GB):  40%|███▉      | 23/58 [00:09<00:07,  4.56it/s]Capturing num tokens (num_tokens=832 avail_mem=23.43 GB):  40%|███▉      | 23/58 [00:09<00:07,  4.56it/s]

    Capturing num tokens (num_tokens=832 avail_mem=23.43 GB):  41%|████▏     | 24/58 [00:09<00:07,  4.62it/s]Capturing num tokens (num_tokens=768 avail_mem=23.76 GB):  41%|████▏     | 24/58 [00:09<00:07,  4.62it/s]Capturing num tokens (num_tokens=768 avail_mem=23.76 GB):  43%|████▎     | 25/58 [00:10<00:06,  4.86it/s]Capturing num tokens (num_tokens=704 avail_mem=23.45 GB):  43%|████▎     | 25/58 [00:10<00:06,  4.86it/s]

    Capturing num tokens (num_tokens=704 avail_mem=23.45 GB):  45%|████▍     | 26/58 [00:10<00:06,  5.13it/s]Capturing num tokens (num_tokens=640 avail_mem=23.75 GB):  45%|████▍     | 26/58 [00:10<00:06,  5.13it/s]Capturing num tokens (num_tokens=640 avail_mem=23.75 GB):  47%|████▋     | 27/58 [00:10<00:05,  5.22it/s]Capturing num tokens (num_tokens=576 avail_mem=23.74 GB):  47%|████▋     | 27/58 [00:10<00:05,  5.22it/s]

    Capturing num tokens (num_tokens=576 avail_mem=23.74 GB):  48%|████▊     | 28/58 [00:10<00:05,  5.55it/s]Capturing num tokens (num_tokens=512 avail_mem=23.49 GB):  48%|████▊     | 28/58 [00:10<00:05,  5.55it/s]Capturing num tokens (num_tokens=512 avail_mem=23.49 GB):  50%|█████     | 29/58 [00:10<00:04,  5.81it/s]Capturing num tokens (num_tokens=480 avail_mem=23.74 GB):  50%|█████     | 29/58 [00:10<00:04,  5.81it/s]

    Capturing num tokens (num_tokens=480 avail_mem=23.74 GB):  52%|█████▏    | 30/58 [00:10<00:04,  5.92it/s]Capturing num tokens (num_tokens=448 avail_mem=23.51 GB):  52%|█████▏    | 30/58 [00:10<00:04,  5.92it/s]Capturing num tokens (num_tokens=448 avail_mem=23.51 GB):  53%|█████▎    | 31/58 [00:11<00:04,  5.94it/s]Capturing num tokens (num_tokens=416 avail_mem=23.53 GB):  53%|█████▎    | 31/58 [00:11<00:04,  5.94it/s]

    Capturing num tokens (num_tokens=416 avail_mem=23.53 GB):  55%|█████▌    | 32/58 [00:11<00:04,  6.15it/s]Capturing num tokens (num_tokens=384 avail_mem=23.72 GB):  55%|█████▌    | 32/58 [00:11<00:04,  6.15it/s]Capturing num tokens (num_tokens=384 avail_mem=23.72 GB):  57%|█████▋    | 33/58 [00:11<00:03,  6.26it/s]Capturing num tokens (num_tokens=352 avail_mem=23.72 GB):  57%|█████▋    | 33/58 [00:11<00:03,  6.26it/s]

    Capturing num tokens (num_tokens=352 avail_mem=23.72 GB):  59%|█████▊    | 34/58 [00:11<00:03,  6.60it/s]Capturing num tokens (num_tokens=320 avail_mem=23.57 GB):  59%|█████▊    | 34/58 [00:11<00:03,  6.60it/s]Capturing num tokens (num_tokens=320 avail_mem=23.57 GB):  60%|██████    | 35/58 [00:11<00:03,  7.03it/s]Capturing num tokens (num_tokens=288 avail_mem=23.71 GB):  60%|██████    | 35/58 [00:11<00:03,  7.03it/s]

    Capturing num tokens (num_tokens=288 avail_mem=23.71 GB):  62%|██████▏   | 36/58 [00:11<00:03,  7.17it/s]Capturing num tokens (num_tokens=256 avail_mem=23.70 GB):  62%|██████▏   | 36/58 [00:11<00:03,  7.17it/s]Capturing num tokens (num_tokens=256 avail_mem=23.70 GB):  64%|██████▍   | 37/58 [00:11<00:02,  7.39it/s]Capturing num tokens (num_tokens=240 avail_mem=23.70 GB):  64%|██████▍   | 37/58 [00:11<00:02,  7.39it/s]

    Capturing num tokens (num_tokens=240 avail_mem=23.70 GB):  66%|██████▌   | 38/58 [00:11<00:02,  7.52it/s]Capturing num tokens (num_tokens=224 avail_mem=23.69 GB):  66%|██████▌   | 38/58 [00:11<00:02,  7.52it/s]Capturing num tokens (num_tokens=224 avail_mem=23.69 GB):  67%|██████▋   | 39/58 [00:12<00:02,  7.74it/s]Capturing num tokens (num_tokens=208 avail_mem=23.59 GB):  67%|██████▋   | 39/58 [00:12<00:02,  7.74it/s]

    Capturing num tokens (num_tokens=208 avail_mem=23.59 GB):  69%|██████▉   | 40/58 [00:12<00:02,  7.85it/s]Capturing num tokens (num_tokens=192 avail_mem=23.68 GB):  69%|██████▉   | 40/58 [00:12<00:02,  7.85it/s]Capturing num tokens (num_tokens=192 avail_mem=23.68 GB):  71%|███████   | 41/58 [00:12<00:02,  7.89it/s]Capturing num tokens (num_tokens=176 avail_mem=23.67 GB):  71%|███████   | 41/58 [00:12<00:02,  7.89it/s]

    Capturing num tokens (num_tokens=176 avail_mem=23.67 GB):  72%|███████▏  | 42/58 [00:12<00:02,  7.91it/s]Capturing num tokens (num_tokens=160 avail_mem=23.66 GB):  72%|███████▏  | 42/58 [00:12<00:02,  7.91it/s]Capturing num tokens (num_tokens=160 avail_mem=23.66 GB):  74%|███████▍  | 43/58 [00:12<00:01,  8.01it/s]Capturing num tokens (num_tokens=144 avail_mem=23.65 GB):  74%|███████▍  | 43/58 [00:12<00:01,  8.01it/s]

    Capturing num tokens (num_tokens=144 avail_mem=23.65 GB):  76%|███████▌  | 44/58 [00:12<00:01,  8.13it/s]Capturing num tokens (num_tokens=128 avail_mem=23.61 GB):  76%|███████▌  | 44/58 [00:12<00:01,  8.13it/s]Capturing num tokens (num_tokens=112 avail_mem=23.63 GB):  76%|███████▌  | 44/58 [00:12<00:01,  8.13it/s]Capturing num tokens (num_tokens=112 avail_mem=23.63 GB):  79%|███████▉  | 46/58 [00:12<00:01, 10.75it/s]Capturing num tokens (num_tokens=96 avail_mem=23.64 GB):  79%|███████▉  | 46/58 [00:12<00:01, 10.75it/s] Capturing num tokens (num_tokens=80 avail_mem=23.63 GB):  79%|███████▉  | 46/58 [00:12<00:01, 10.75it/s]

    Capturing num tokens (num_tokens=80 avail_mem=23.63 GB):  83%|████████▎ | 48/58 [00:12<00:00, 10.82it/s]Capturing num tokens (num_tokens=64 avail_mem=41.98 GB):  83%|████████▎ | 48/58 [00:12<00:00, 10.82it/s]Capturing num tokens (num_tokens=48 avail_mem=42.00 GB):  83%|████████▎ | 48/58 [00:13<00:00, 10.82it/s]Capturing num tokens (num_tokens=32 avail_mem=41.94 GB):  83%|████████▎ | 48/58 [00:13<00:00, 10.82it/s]Capturing num tokens (num_tokens=32 avail_mem=41.94 GB):  88%|████████▊ | 51/58 [00:13<00:00, 14.92it/s]Capturing num tokens (num_tokens=28 avail_mem=41.96 GB):  88%|████████▊ | 51/58 [00:13<00:00, 14.92it/s]Capturing num tokens (num_tokens=24 avail_mem=41.96 GB):  88%|████████▊ | 51/58 [00:13<00:00, 14.92it/s]Capturing num tokens (num_tokens=20 avail_mem=41.95 GB):  88%|████████▊ | 51/58 [00:13<00:00, 14.92it/s]

    Capturing num tokens (num_tokens=20 avail_mem=41.95 GB):  93%|█████████▎| 54/58 [00:13<00:00, 17.99it/s]Capturing num tokens (num_tokens=16 avail_mem=41.94 GB):  93%|█████████▎| 54/58 [00:13<00:00, 17.99it/s]Capturing num tokens (num_tokens=12 avail_mem=41.93 GB):  93%|█████████▎| 54/58 [00:13<00:00, 17.99it/s]Capturing num tokens (num_tokens=8 avail_mem=41.93 GB):  93%|█████████▎| 54/58 [00:13<00:00, 17.99it/s] Capturing num tokens (num_tokens=8 avail_mem=41.93 GB):  98%|█████████▊| 57/58 [00:13<00:00, 20.64it/s]Capturing num tokens (num_tokens=4 avail_mem=41.90 GB):  98%|█████████▊| 57/58 [00:13<00:00, 20.64it/s]Capturing num tokens (num_tokens=4 avail_mem=41.90 GB): 100%|██████████| 58/58 [00:13<00:00,  4.34it/s]



<strong style='color: #00008B;'>==== Original Output ====</strong>



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of the numbers 1 and 3.<br><br>Next, I add the two numbers together: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></think><br><br>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>**Answer:** \boxed{4}</strong>



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of the numbers 1 and 3.<br><br>Next, I add the two numbers together: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>**Answer:** \boxed{4}</strong>



```python
llm.shutdown()
```

## Supporting New Reasoning Model Schemas

For future reasoning models, you can implement the reasoning parser as a subclass of `BaseReasoningFormatDetector` in `python/sglang/srt/reasoning_parser.py` and specify the reasoning parser for new reasoning model schemas accordingly.
