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


    Multi-thread loading shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Multi-thread loading shards:  50% Completed | 1/2 [00:01<00:01,  1.15s/it]

    Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.34s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.31s/it]


    2026-04-04 01:39:11,490 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 01:39:11] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:56,  3.09s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:56,  3.09s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:32,  1.65s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:32,  1.65s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:00,  1.10s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:00,  1.10s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:44,  1.21it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:44,  1.21it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:34,  1.53it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:34,  1.53it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:28,  1.83it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:28,  1.83it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:23,  2.15it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:23,  2.15it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:05<00:21,  2.35it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:05<00:21,  2.35it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:06<00:17,  2.74it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:06<00:17,  2.74it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:06<00:15,  3.11it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:06<00:15,  3.11it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:06<00:13,  3.50it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:06<00:13,  3.50it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:06<00:11,  3.92it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:06<00:11,  3.92it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:06<00:10,  4.38it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:06<00:10,  4.38it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:07<00:09,  4.80it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:07<00:09,  4.80it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:07<00:08,  5.37it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:07<00:08,  5.37it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:07<00:07,  5.85it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:07<00:07,  5.85it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:07<00:06,  6.38it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:07<00:06,  6.38it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:07<00:05,  7.14it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:07<00:05,  7.14it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:07<00:05,  7.14it/s]

    Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:07<00:04,  8.60it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:07<00:04,  8.60it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:07<00:04,  8.60it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:07<00:03, 10.31it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:07<00:03, 10.31it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:07<00:03, 10.31it/s]

    Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:08<00:02, 12.07it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:08<00:02, 12.07it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:08<00:02, 12.07it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:08<00:02, 12.07it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:08<00:02, 14.87it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:08<00:02, 14.87it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:08<00:02, 14.87it/s]

    Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:08<00:02, 14.87it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:08<00:01, 17.53it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:08<00:01, 17.53it/s]Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:08<00:01, 17.53it/s]Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:08<00:01, 17.53it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:08<00:01, 19.90it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:08<00:01, 19.90it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:08<00:01, 19.90it/s]

    Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:08<00:01, 19.90it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:08<00:01, 21.92it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:08<00:01, 21.92it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:08<00:01, 21.92it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:08<00:01, 21.92it/s]Compiling num tokens (num_tokens=208):  62%|██████▏   | 36/58 [00:08<00:01, 21.92it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:08<00:00, 25.23it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:08<00:00, 25.23it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:08<00:00, 25.23it/s]

    Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:08<00:00, 25.23it/s]Compiling num tokens (num_tokens=144):  69%|██████▉   | 40/58 [00:08<00:00, 25.23it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:08<00:00, 27.28it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:08<00:00, 27.28it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:08<00:00, 27.28it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:08<00:00, 27.28it/s] 

    Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:08<00:00, 24.91it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:08<00:00, 24.91it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:08<00:00, 24.91it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:08<00:00, 24.91it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:09<00:00, 24.08it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:09<00:00, 24.08it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:09<00:00, 24.08it/s]

    Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:09<00:00, 24.08it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:09<00:00, 24.40it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:09<00:00, 24.40it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:09<00:00, 24.40it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:09<00:00, 24.40it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:09<00:00, 25.77it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:09<00:00, 25.77it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:09<00:00, 25.77it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:09<00:00,  6.24it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=44.07 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=44.07 GB):   2%|▏         | 1/58 [00:00<00:34,  1.63it/s]Capturing num tokens (num_tokens=7680 avail_mem=44.01 GB):   2%|▏         | 1/58 [00:00<00:34,  1.63it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=44.01 GB):   3%|▎         | 2/58 [00:01<00:31,  1.76it/s]Capturing num tokens (num_tokens=7168 avail_mem=44.01 GB):   3%|▎         | 2/58 [00:01<00:31,  1.76it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=44.01 GB):   5%|▌         | 3/58 [00:01<00:29,  1.84it/s]Capturing num tokens (num_tokens=6656 avail_mem=44.02 GB):   5%|▌         | 3/58 [00:01<00:29,  1.84it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=44.02 GB):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]Capturing num tokens (num_tokens=6144 avail_mem=44.03 GB):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=44.03 GB):   9%|▊         | 5/58 [00:02<00:25,  2.07it/s]Capturing num tokens (num_tokens=5632 avail_mem=43.24 GB):   9%|▊         | 5/58 [00:02<00:25,  2.07it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=43.24 GB):  10%|█         | 6/58 [00:02<00:23,  2.26it/s]Capturing num tokens (num_tokens=5120 avail_mem=43.31 GB):  10%|█         | 6/58 [00:02<00:23,  2.26it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=43.31 GB):  12%|█▏        | 7/58 [00:03<00:21,  2.33it/s]Capturing num tokens (num_tokens=4608 avail_mem=44.08 GB):  12%|█▏        | 7/58 [00:03<00:21,  2.33it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=44.08 GB):  14%|█▍        | 8/58 [00:03<00:20,  2.49it/s]Capturing num tokens (num_tokens=4096 avail_mem=44.05 GB):  14%|█▍        | 8/58 [00:03<00:20,  2.49it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=44.05 GB):  16%|█▌        | 9/58 [00:03<00:17,  2.76it/s]Capturing num tokens (num_tokens=3840 avail_mem=44.06 GB):  16%|█▌        | 9/58 [00:03<00:17,  2.76it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=44.06 GB):  17%|█▋        | 10/58 [00:04<00:15,  3.03it/s]Capturing num tokens (num_tokens=3584 avail_mem=44.05 GB):  17%|█▋        | 10/58 [00:04<00:15,  3.03it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=44.05 GB):  19%|█▉        | 11/58 [00:04<00:14,  3.33it/s]Capturing num tokens (num_tokens=3328 avail_mem=44.05 GB):  19%|█▉        | 11/58 [00:04<00:14,  3.33it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=44.05 GB):  21%|██        | 12/58 [00:04<00:12,  3.68it/s]Capturing num tokens (num_tokens=3072 avail_mem=44.05 GB):  21%|██        | 12/58 [00:04<00:12,  3.68it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=44.05 GB):  22%|██▏       | 13/58 [00:04<00:11,  3.98it/s]Capturing num tokens (num_tokens=2816 avail_mem=44.05 GB):  22%|██▏       | 13/58 [00:04<00:11,  3.98it/s]Capturing num tokens (num_tokens=2816 avail_mem=44.05 GB):  24%|██▍       | 14/58 [00:05<00:10,  4.38it/s]Capturing num tokens (num_tokens=2560 avail_mem=44.05 GB):  24%|██▍       | 14/58 [00:05<00:10,  4.38it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=44.05 GB):  26%|██▌       | 15/58 [00:05<00:08,  4.85it/s]Capturing num tokens (num_tokens=2304 avail_mem=44.05 GB):  26%|██▌       | 15/58 [00:05<00:08,  4.85it/s]Capturing num tokens (num_tokens=2304 avail_mem=44.05 GB):  28%|██▊       | 16/58 [00:05<00:08,  5.11it/s]Capturing num tokens (num_tokens=2048 avail_mem=44.05 GB):  28%|██▊       | 16/58 [00:05<00:08,  5.11it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=44.05 GB):  29%|██▉       | 17/58 [00:05<00:07,  5.72it/s]Capturing num tokens (num_tokens=1792 avail_mem=44.01 GB):  29%|██▉       | 17/58 [00:05<00:07,  5.72it/s]Capturing num tokens (num_tokens=1792 avail_mem=44.01 GB):  31%|███       | 18/58 [00:05<00:06,  6.33it/s]Capturing num tokens (num_tokens=1536 avail_mem=43.84 GB):  31%|███       | 18/58 [00:05<00:06,  6.33it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=43.84 GB):  33%|███▎      | 19/58 [00:05<00:05,  6.99it/s]Capturing num tokens (num_tokens=1280 avail_mem=43.88 GB):  33%|███▎      | 19/58 [00:05<00:05,  6.99it/s]Capturing num tokens (num_tokens=1280 avail_mem=43.88 GB):  34%|███▍      | 20/58 [00:05<00:04,  7.65it/s]Capturing num tokens (num_tokens=1024 avail_mem=44.05 GB):  34%|███▍      | 20/58 [00:05<00:04,  7.65it/s]Capturing num tokens (num_tokens=960 avail_mem=44.04 GB):  34%|███▍      | 20/58 [00:05<00:04,  7.65it/s] 

    Capturing num tokens (num_tokens=960 avail_mem=44.04 GB):  38%|███▊      | 22/58 [00:06<00:04,  8.82it/s]Capturing num tokens (num_tokens=896 avail_mem=44.03 GB):  38%|███▊      | 22/58 [00:06<00:04,  8.82it/s]Capturing num tokens (num_tokens=832 avail_mem=44.03 GB):  38%|███▊      | 22/58 [00:06<00:04,  8.82it/s]Capturing num tokens (num_tokens=832 avail_mem=44.03 GB):  41%|████▏     | 24/58 [00:06<00:03, 10.43it/s]Capturing num tokens (num_tokens=768 avail_mem=44.01 GB):  41%|████▏     | 24/58 [00:06<00:03, 10.43it/s]

    Capturing num tokens (num_tokens=704 avail_mem=44.01 GB):  41%|████▏     | 24/58 [00:06<00:03, 10.43it/s]Capturing num tokens (num_tokens=704 avail_mem=44.01 GB):  45%|████▍     | 26/58 [00:06<00:02, 12.11it/s]Capturing num tokens (num_tokens=640 avail_mem=44.00 GB):  45%|████▍     | 26/58 [00:06<00:02, 12.11it/s]Capturing num tokens (num_tokens=576 avail_mem=44.00 GB):  45%|████▍     | 26/58 [00:06<00:02, 12.11it/s]Capturing num tokens (num_tokens=576 avail_mem=44.00 GB):  48%|████▊     | 28/58 [00:06<00:02, 13.79it/s]Capturing num tokens (num_tokens=512 avail_mem=43.98 GB):  48%|████▊     | 28/58 [00:06<00:02, 13.79it/s]

    Capturing num tokens (num_tokens=480 avail_mem=43.98 GB):  48%|████▊     | 28/58 [00:06<00:02, 13.79it/s]Capturing num tokens (num_tokens=448 avail_mem=43.97 GB):  48%|████▊     | 28/58 [00:06<00:02, 13.79it/s]Capturing num tokens (num_tokens=448 avail_mem=43.97 GB):  53%|█████▎    | 31/58 [00:06<00:01, 15.85it/s]Capturing num tokens (num_tokens=416 avail_mem=43.97 GB):  53%|█████▎    | 31/58 [00:06<00:01, 15.85it/s]Capturing num tokens (num_tokens=384 avail_mem=43.96 GB):  53%|█████▎    | 31/58 [00:06<00:01, 15.85it/s]Capturing num tokens (num_tokens=352 avail_mem=43.95 GB):  53%|█████▎    | 31/58 [00:06<00:01, 15.85it/s]

    Capturing num tokens (num_tokens=352 avail_mem=43.95 GB):  59%|█████▊    | 34/58 [00:06<00:01, 18.02it/s]Capturing num tokens (num_tokens=320 avail_mem=43.94 GB):  59%|█████▊    | 34/58 [00:06<00:01, 18.02it/s]Capturing num tokens (num_tokens=288 avail_mem=43.93 GB):  59%|█████▊    | 34/58 [00:06<00:01, 18.02it/s]Capturing num tokens (num_tokens=256 avail_mem=43.92 GB):  59%|█████▊    | 34/58 [00:06<00:01, 18.02it/s]Capturing num tokens (num_tokens=256 avail_mem=43.92 GB):  64%|██████▍   | 37/58 [00:06<00:01, 20.05it/s]Capturing num tokens (num_tokens=240 avail_mem=43.92 GB):  64%|██████▍   | 37/58 [00:06<00:01, 20.05it/s]Capturing num tokens (num_tokens=224 avail_mem=43.91 GB):  64%|██████▍   | 37/58 [00:06<00:01, 20.05it/s]Capturing num tokens (num_tokens=208 avail_mem=43.91 GB):  64%|██████▍   | 37/58 [00:06<00:01, 20.05it/s]

    Capturing num tokens (num_tokens=192 avail_mem=43.90 GB):  64%|██████▍   | 37/58 [00:06<00:01, 20.05it/s]Capturing num tokens (num_tokens=192 avail_mem=43.90 GB):  71%|███████   | 41/58 [00:06<00:00, 23.40it/s]Capturing num tokens (num_tokens=176 avail_mem=43.89 GB):  71%|███████   | 41/58 [00:06<00:00, 23.40it/s]Capturing num tokens (num_tokens=160 avail_mem=43.89 GB):  71%|███████   | 41/58 [00:06<00:00, 23.40it/s]Capturing num tokens (num_tokens=144 avail_mem=43.88 GB):  71%|███████   | 41/58 [00:06<00:00, 23.40it/s]Capturing num tokens (num_tokens=144 avail_mem=43.88 GB):  76%|███████▌  | 44/58 [00:07<00:00, 24.25it/s]Capturing num tokens (num_tokens=128 avail_mem=43.88 GB):  76%|███████▌  | 44/58 [00:07<00:00, 24.25it/s]Capturing num tokens (num_tokens=112 avail_mem=43.89 GB):  76%|███████▌  | 44/58 [00:07<00:00, 24.25it/s]

    Capturing num tokens (num_tokens=96 avail_mem=43.86 GB):  76%|███████▌  | 44/58 [00:07<00:00, 24.25it/s] Capturing num tokens (num_tokens=96 avail_mem=43.86 GB):  81%|████████  | 47/58 [00:07<00:00, 23.05it/s]Capturing num tokens (num_tokens=80 avail_mem=43.87 GB):  81%|████████  | 47/58 [00:07<00:00, 23.05it/s]Capturing num tokens (num_tokens=64 avail_mem=43.86 GB):  81%|████████  | 47/58 [00:07<00:00, 23.05it/s]

    Capturing num tokens (num_tokens=48 avail_mem=43.86 GB):  81%|████████  | 47/58 [00:07<00:00, 23.05it/s]Capturing num tokens (num_tokens=48 avail_mem=43.86 GB):  86%|████████▌ | 50/58 [00:07<00:00, 19.40it/s]Capturing num tokens (num_tokens=32 avail_mem=43.84 GB):  86%|████████▌ | 50/58 [00:07<00:00, 19.40it/s]Capturing num tokens (num_tokens=28 avail_mem=43.83 GB):  86%|████████▌ | 50/58 [00:07<00:00, 19.40it/s]Capturing num tokens (num_tokens=24 avail_mem=43.84 GB):  86%|████████▌ | 50/58 [00:07<00:00, 19.40it/s]Capturing num tokens (num_tokens=24 avail_mem=43.84 GB):  91%|█████████▏| 53/58 [00:07<00:00, 21.16it/s]Capturing num tokens (num_tokens=20 avail_mem=43.84 GB):  91%|█████████▏| 53/58 [00:07<00:00, 21.16it/s]Capturing num tokens (num_tokens=16 avail_mem=43.81 GB):  91%|█████████▏| 53/58 [00:07<00:00, 21.16it/s]

    Capturing num tokens (num_tokens=12 avail_mem=43.82 GB):  91%|█████████▏| 53/58 [00:07<00:00, 21.16it/s]Capturing num tokens (num_tokens=12 avail_mem=43.82 GB):  97%|█████████▋| 56/58 [00:07<00:00, 23.02it/s]Capturing num tokens (num_tokens=8 avail_mem=43.82 GB):  97%|█████████▋| 56/58 [00:07<00:00, 23.02it/s] Capturing num tokens (num_tokens=4 avail_mem=43.81 GB):  97%|█████████▋| 56/58 [00:07<00:00, 23.02it/s]Capturing num tokens (num_tokens=4 avail_mem=43.81 GB): 100%|██████████| 58/58 [00:07<00:00,  7.59it/s]


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



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I add the two numbers together: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>Sure! Let's solve the problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Final Answer:**<br>\[<br>\boxed{4}<br>\]</strong>


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



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I perform the addition by combining the two numbers.<br><br>Finally, I arrive at the result, which is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'><br><br>**Solution:**<br><br>We are asked to find the sum of \(1\) and \(3\).<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the final answer is \(\boxed{4}\).</strong>


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



<strong style='color: #00008B;'>To solve the problem of adding 1 and 3, I start by identifying the numbers involved.<br><br>Next, I perform the addition operation by combining the two numbers.<br><br>Finally, I calculate the sum to find the result.</strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'><br><br>**Solution:**<br><br>To find the sum of 1 and 3, follow these simple steps:<br><br>1. **Identify the numbers to add:**<br>   <br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Perform the addition:**<br>   <br>   \[<br>   1 + 3 = 4<br>   \]<br><br>3. **Write the final answer:**<br>   <br>   \[<br>   \boxed{4}<br>   \]</strong>


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



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I add the two numbers together: 1 plus 3 equals 4.<br><br>Finally, I conclude that the result of 1 + 3 is 4.<br></think><br><br>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>1. Start with the number **1**.<br>2. Add the number **3** to it.<br>3. The sum is **4**.<br><br>\[<br>1 + 3 = \boxed{4}<br>\]<br><br>**Answer:** \(\boxed{4}\)</strong>


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



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I perform the addition: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></think><br><br>Sure! Let's solve the problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Perform the addition:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:** \(\boxed{4}\)</strong>


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:328: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      return await dependant.call(**values)



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I perform the addition: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>Sure! Let's solve the problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Perform the addition:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:** \(\boxed{4}\)</strong>



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


    Multi-thread loading shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Multi-thread loading shards:  50% Completed | 1/2 [00:01<00:01,  1.06s/it]

    Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.36s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.31s/it]


    2026-04-04 01:40:05,635 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 01:40:05] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:54,  3.06s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:54,  3.06s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:40,  1.80s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:40,  1.80s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:04,  1.18s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:04,  1.18s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:46,  1.15it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:46,  1.15it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:36,  1.47it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:36,  1.47it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:29,  1.77it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:29,  1.77it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:24,  2.11it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:24,  2.11it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:06<00:20,  2.45it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:06<00:20,  2.45it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:06<00:17,  2.84it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:06<00:17,  2.84it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:06<00:14,  3.24it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:06<00:14,  3.24it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:06<00:13,  3.58it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:06<00:13,  3.58it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:06<00:11,  4.00it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:06<00:11,  4.00it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:07<00:10,  4.44it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:07<00:10,  4.44it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:07<00:09,  4.82it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:07<00:09,  4.82it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:07<00:08,  5.33it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:07<00:08,  5.33it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:07<00:07,  5.85it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:07<00:07,  5.85it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:07<00:06,  6.46it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:07<00:06,  6.46it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:07<00:05,  7.12it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:07<00:05,  7.12it/s]

    Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:07<00:05,  7.12it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:07<00:04,  8.38it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:07<00:04,  8.38it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:07<00:04,  8.38it/s] 

    Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:08<00:03, 10.02it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:08<00:03, 10.02it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:08<00:03, 10.02it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:08<00:02, 11.48it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:08<00:02, 11.48it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:08<00:02, 11.48it/s]

    Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:08<00:02, 13.04it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:08<00:02, 13.04it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:08<00:02, 13.04it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:08<00:02, 14.58it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:08<00:02, 14.58it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:08<00:02, 14.58it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:08<00:02, 14.58it/s]

    Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:08<00:01, 16.89it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:08<00:01, 16.89it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:08<00:01, 16.89it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:08<00:01, 17.21it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:08<00:01, 17.21it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:08<00:01, 17.21it/s]

    Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:08<00:01, 17.21it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:08<00:01, 18.67it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:08<00:01, 18.67it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:08<00:01, 18.67it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:08<00:01, 18.67it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:08<00:00, 20.71it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:08<00:00, 20.71it/s]

    Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:08<00:00, 20.71it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:08<00:00, 20.71it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:09<00:00, 21.72it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:09<00:00, 21.72it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:09<00:00, 21.72it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:09<00:00, 21.72it/s]

    Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:09<00:00, 22.45it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:09<00:00, 22.45it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:09<00:00, 22.45it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:09<00:00, 22.45it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:09<00:00, 23.10it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:09<00:00, 23.10it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:09<00:00, 23.10it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:09<00:00, 23.10it/s]

    Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:09<00:00, 23.95it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:09<00:00, 23.95it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:09<00:00, 23.95it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:09<00:00, 23.95it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:09<00:00, 23.95it/s]Compiling num tokens (num_tokens=16):  95%|█████████▍| 55/58 [00:09<00:00, 26.49it/s]Compiling num tokens (num_tokens=12):  95%|█████████▍| 55/58 [00:09<00:00, 26.49it/s]Compiling num tokens (num_tokens=8):  95%|█████████▍| 55/58 [00:09<00:00, 26.49it/s] Compiling num tokens (num_tokens=4):  95%|█████████▍| 55/58 [00:09<00:00, 26.49it/s]

    Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:09<00:00,  6.05it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=27.58 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=27.58 GB):   2%|▏         | 1/58 [00:00<00:35,  1.60it/s]Capturing num tokens (num_tokens=7680 avail_mem=27.55 GB):   2%|▏         | 1/58 [00:00<00:35,  1.60it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=27.55 GB):   3%|▎         | 2/58 [00:01<00:33,  1.66it/s]Capturing num tokens (num_tokens=7168 avail_mem=27.55 GB):   3%|▎         | 2/58 [00:01<00:33,  1.66it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=27.55 GB):   5%|▌         | 3/58 [00:01<00:31,  1.76it/s]Capturing num tokens (num_tokens=6656 avail_mem=27.55 GB):   5%|▌         | 3/58 [00:01<00:31,  1.76it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=27.55 GB):   7%|▋         | 4/58 [00:02<00:28,  1.88it/s]Capturing num tokens (num_tokens=6144 avail_mem=27.56 GB):   7%|▋         | 4/58 [00:02<00:28,  1.88it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=27.56 GB):   9%|▊         | 5/58 [00:02<00:26,  1.97it/s]Capturing num tokens (num_tokens=5632 avail_mem=27.49 GB):   9%|▊         | 5/58 [00:02<00:26,  1.97it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=27.49 GB):  10%|█         | 6/58 [00:03<00:24,  2.11it/s]Capturing num tokens (num_tokens=5120 avail_mem=24.52 GB):  10%|█         | 6/58 [00:03<00:24,  2.11it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=24.52 GB):  12%|█▏        | 7/58 [00:03<00:23,  2.18it/s]Capturing num tokens (num_tokens=4608 avail_mem=21.74 GB):  12%|█▏        | 7/58 [00:03<00:23,  2.18it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=21.74 GB):  14%|█▍        | 8/58 [00:03<00:20,  2.39it/s]Capturing num tokens (num_tokens=4096 avail_mem=21.74 GB):  14%|█▍        | 8/58 [00:03<00:20,  2.39it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=21.74 GB):  16%|█▌        | 9/58 [00:04<00:19,  2.56it/s]Capturing num tokens (num_tokens=3840 avail_mem=21.74 GB):  16%|█▌        | 9/58 [00:04<00:19,  2.56it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=21.74 GB):  17%|█▋        | 10/58 [00:04<00:21,  2.26it/s]Capturing num tokens (num_tokens=3584 avail_mem=21.74 GB):  17%|█▋        | 10/58 [00:04<00:21,  2.26it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=21.74 GB):  19%|█▉        | 11/58 [00:05<00:18,  2.54it/s]Capturing num tokens (num_tokens=3328 avail_mem=21.74 GB):  19%|█▉        | 11/58 [00:05<00:18,  2.54it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=21.74 GB):  21%|██        | 12/58 [00:05<00:16,  2.83it/s]Capturing num tokens (num_tokens=3072 avail_mem=21.74 GB):  21%|██        | 12/58 [00:05<00:16,  2.83it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=21.74 GB):  22%|██▏       | 13/58 [00:05<00:14,  3.04it/s]Capturing num tokens (num_tokens=2816 avail_mem=21.74 GB):  22%|██▏       | 13/58 [00:05<00:14,  3.04it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=21.74 GB):  24%|██▍       | 14/58 [00:05<00:13,  3.27it/s]Capturing num tokens (num_tokens=2560 avail_mem=21.74 GB):  24%|██▍       | 14/58 [00:05<00:13,  3.27it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=21.74 GB):  26%|██▌       | 15/58 [00:06<00:11,  3.60it/s]Capturing num tokens (num_tokens=2304 avail_mem=21.74 GB):  26%|██▌       | 15/58 [00:06<00:11,  3.60it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=21.74 GB):  28%|██▊       | 16/58 [00:06<00:10,  3.91it/s]Capturing num tokens (num_tokens=2048 avail_mem=21.74 GB):  28%|██▊       | 16/58 [00:06<00:10,  3.91it/s]Capturing num tokens (num_tokens=2048 avail_mem=21.74 GB):  29%|██▉       | 17/58 [00:06<00:09,  4.25it/s]Capturing num tokens (num_tokens=1792 avail_mem=21.74 GB):  29%|██▉       | 17/58 [00:06<00:09,  4.25it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=21.74 GB):  31%|███       | 18/58 [00:06<00:08,  4.51it/s]Capturing num tokens (num_tokens=1536 avail_mem=21.74 GB):  31%|███       | 18/58 [00:06<00:08,  4.51it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=21.74 GB):  33%|███▎      | 19/58 [00:06<00:08,  4.64it/s]Capturing num tokens (num_tokens=1280 avail_mem=21.74 GB):  33%|███▎      | 19/58 [00:06<00:08,  4.64it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=21.74 GB):  34%|███▍      | 20/58 [00:07<00:08,  4.69it/s]Capturing num tokens (num_tokens=1024 avail_mem=21.74 GB):  34%|███▍      | 20/58 [00:07<00:08,  4.69it/s]Capturing num tokens (num_tokens=1024 avail_mem=21.74 GB):  36%|███▌      | 21/58 [00:07<00:07,  5.06it/s]Capturing num tokens (num_tokens=960 avail_mem=21.73 GB):  36%|███▌      | 21/58 [00:07<00:07,  5.06it/s] 

    Capturing num tokens (num_tokens=960 avail_mem=21.73 GB):  38%|███▊      | 22/58 [00:07<00:06,  5.60it/s]Capturing num tokens (num_tokens=896 avail_mem=21.73 GB):  38%|███▊      | 22/58 [00:07<00:06,  5.60it/s]Capturing num tokens (num_tokens=896 avail_mem=21.73 GB):  40%|███▉      | 23/58 [00:07<00:05,  6.11it/s]Capturing num tokens (num_tokens=832 avail_mem=21.73 GB):  40%|███▉      | 23/58 [00:07<00:05,  6.11it/s]

    Capturing num tokens (num_tokens=832 avail_mem=21.73 GB):  41%|████▏     | 24/58 [00:07<00:05,  6.52it/s]Capturing num tokens (num_tokens=768 avail_mem=21.72 GB):  41%|████▏     | 24/58 [00:07<00:05,  6.52it/s]Capturing num tokens (num_tokens=768 avail_mem=21.72 GB):  43%|████▎     | 25/58 [00:07<00:04,  6.88it/s]Capturing num tokens (num_tokens=704 avail_mem=21.72 GB):  43%|████▎     | 25/58 [00:07<00:04,  6.88it/s]

    Capturing num tokens (num_tokens=704 avail_mem=21.72 GB):  45%|████▍     | 26/58 [00:07<00:04,  7.26it/s]Capturing num tokens (num_tokens=640 avail_mem=21.72 GB):  45%|████▍     | 26/58 [00:07<00:04,  7.26it/s]Capturing num tokens (num_tokens=640 avail_mem=21.72 GB):  47%|████▋     | 27/58 [00:07<00:04,  7.50it/s]Capturing num tokens (num_tokens=576 avail_mem=21.71 GB):  47%|████▋     | 27/58 [00:07<00:04,  7.50it/s]

    Capturing num tokens (num_tokens=576 avail_mem=21.71 GB):  48%|████▊     | 28/58 [00:08<00:03,  7.73it/s]Capturing num tokens (num_tokens=512 avail_mem=21.70 GB):  48%|████▊     | 28/58 [00:08<00:03,  7.73it/s]Capturing num tokens (num_tokens=512 avail_mem=21.70 GB):  50%|█████     | 29/58 [00:08<00:03,  8.07it/s]Capturing num tokens (num_tokens=480 avail_mem=21.70 GB):  50%|█████     | 29/58 [00:08<00:03,  8.07it/s]

    Capturing num tokens (num_tokens=480 avail_mem=21.70 GB):  52%|█████▏    | 30/58 [00:08<00:03,  8.31it/s]Capturing num tokens (num_tokens=448 avail_mem=21.70 GB):  52%|█████▏    | 30/58 [00:08<00:03,  8.31it/s]Capturing num tokens (num_tokens=448 avail_mem=21.70 GB):  53%|█████▎    | 31/58 [00:08<00:03,  8.24it/s]Capturing num tokens (num_tokens=416 avail_mem=21.66 GB):  53%|█████▎    | 31/58 [00:08<00:03,  8.24it/s]

    Capturing num tokens (num_tokens=416 avail_mem=21.66 GB):  55%|█████▌    | 32/58 [00:08<00:03,  8.46it/s]Capturing num tokens (num_tokens=384 avail_mem=21.66 GB):  55%|█████▌    | 32/58 [00:08<00:03,  8.46it/s]Capturing num tokens (num_tokens=384 avail_mem=21.66 GB):  57%|█████▋    | 33/58 [00:08<00:02,  8.60it/s]Capturing num tokens (num_tokens=352 avail_mem=21.65 GB):  57%|█████▋    | 33/58 [00:08<00:02,  8.60it/s]

    Capturing num tokens (num_tokens=352 avail_mem=21.65 GB):  59%|█████▊    | 34/58 [00:08<00:03,  7.80it/s]Capturing num tokens (num_tokens=320 avail_mem=21.65 GB):  59%|█████▊    | 34/58 [00:08<00:03,  7.80it/s]Capturing num tokens (num_tokens=320 avail_mem=21.65 GB):  60%|██████    | 35/58 [00:08<00:03,  7.26it/s]Capturing num tokens (num_tokens=288 avail_mem=21.65 GB):  60%|██████    | 35/58 [00:08<00:03,  7.26it/s]

    Capturing num tokens (num_tokens=288 avail_mem=21.65 GB):  62%|██████▏   | 36/58 [00:09<00:02,  7.81it/s]Capturing num tokens (num_tokens=256 avail_mem=21.64 GB):  62%|██████▏   | 36/58 [00:09<00:02,  7.81it/s]Capturing num tokens (num_tokens=256 avail_mem=21.64 GB):  64%|██████▍   | 37/58 [00:09<00:02,  8.31it/s]Capturing num tokens (num_tokens=240 avail_mem=21.64 GB):  64%|██████▍   | 37/58 [00:09<00:02,  8.31it/s]

    Capturing num tokens (num_tokens=224 avail_mem=21.64 GB):  64%|██████▍   | 37/58 [00:09<00:02,  8.31it/s]Capturing num tokens (num_tokens=224 avail_mem=21.64 GB):  67%|██████▋   | 39/58 [00:09<00:02,  8.99it/s]Capturing num tokens (num_tokens=208 avail_mem=21.63 GB):  67%|██████▋   | 39/58 [00:09<00:02,  8.99it/s]

    Capturing num tokens (num_tokens=208 avail_mem=21.63 GB):  69%|██████▉   | 40/58 [00:09<00:01,  9.19it/s]Capturing num tokens (num_tokens=192 avail_mem=21.63 GB):  69%|██████▉   | 40/58 [00:09<00:01,  9.19it/s]Capturing num tokens (num_tokens=192 avail_mem=21.63 GB):  71%|███████   | 41/58 [00:09<00:01,  9.31it/s]Capturing num tokens (num_tokens=176 avail_mem=21.62 GB):  71%|███████   | 41/58 [00:09<00:01,  9.31it/s]

    Capturing num tokens (num_tokens=160 avail_mem=21.62 GB):  71%|███████   | 41/58 [00:09<00:01,  9.31it/s]Capturing num tokens (num_tokens=160 avail_mem=21.62 GB):  74%|███████▍  | 43/58 [00:09<00:01,  9.62it/s]Capturing num tokens (num_tokens=144 avail_mem=21.62 GB):  74%|███████▍  | 43/58 [00:09<00:01,  9.62it/s]

    Capturing num tokens (num_tokens=144 avail_mem=21.62 GB):  76%|███████▌  | 44/58 [00:09<00:01,  9.64it/s]Capturing num tokens (num_tokens=128 avail_mem=21.63 GB):  76%|███████▌  | 44/58 [00:09<00:01,  9.64it/s]Capturing num tokens (num_tokens=128 avail_mem=21.63 GB):  78%|███████▊  | 45/58 [00:09<00:01,  9.53it/s]Capturing num tokens (num_tokens=112 avail_mem=21.62 GB):  78%|███████▊  | 45/58 [00:10<00:01,  9.53it/s]

    Capturing num tokens (num_tokens=112 avail_mem=21.62 GB):  79%|███████▉  | 46/58 [00:10<00:01,  8.29it/s]Capturing num tokens (num_tokens=96 avail_mem=21.62 GB):  79%|███████▉  | 46/58 [00:10<00:01,  8.29it/s] Capturing num tokens (num_tokens=80 avail_mem=21.61 GB):  79%|███████▉  | 46/58 [00:10<00:01,  8.29it/s]Capturing num tokens (num_tokens=80 avail_mem=21.61 GB):  83%|████████▎ | 48/58 [00:10<00:01,  9.00it/s]Capturing num tokens (num_tokens=64 avail_mem=21.61 GB):  83%|████████▎ | 48/58 [00:10<00:01,  9.00it/s]

    Capturing num tokens (num_tokens=48 avail_mem=21.61 GB):  83%|████████▎ | 48/58 [00:10<00:01,  9.00it/s]Capturing num tokens (num_tokens=48 avail_mem=21.61 GB):  86%|████████▌ | 50/58 [00:10<00:00,  9.37it/s]Capturing num tokens (num_tokens=32 avail_mem=21.60 GB):  86%|████████▌ | 50/58 [00:10<00:00,  9.37it/s]Capturing num tokens (num_tokens=28 avail_mem=21.60 GB):  86%|████████▌ | 50/58 [00:10<00:00,  9.37it/s]

    Capturing num tokens (num_tokens=28 avail_mem=21.60 GB):  90%|████████▉ | 52/58 [00:10<00:00,  9.65it/s]Capturing num tokens (num_tokens=24 avail_mem=21.60 GB):  90%|████████▉ | 52/58 [00:10<00:00,  9.65it/s]Capturing num tokens (num_tokens=20 avail_mem=21.59 GB):  90%|████████▉ | 52/58 [00:10<00:00,  9.65it/s]Capturing num tokens (num_tokens=20 avail_mem=21.59 GB):  93%|█████████▎| 54/58 [00:10<00:00,  9.91it/s]Capturing num tokens (num_tokens=16 avail_mem=21.59 GB):  93%|█████████▎| 54/58 [00:10<00:00,  9.91it/s]

    Capturing num tokens (num_tokens=12 avail_mem=21.59 GB):  93%|█████████▎| 54/58 [00:11<00:00,  9.91it/s]Capturing num tokens (num_tokens=12 avail_mem=21.59 GB):  97%|█████████▋| 56/58 [00:11<00:00,  9.97it/s]Capturing num tokens (num_tokens=8 avail_mem=21.58 GB):  97%|█████████▋| 56/58 [00:11<00:00,  9.97it/s] Capturing num tokens (num_tokens=4 avail_mem=21.58 GB):  97%|█████████▋| 56/58 [00:11<00:00,  9.97it/s]

    Capturing num tokens (num_tokens=4 avail_mem=21.58 GB): 100%|██████████| 58/58 [00:11<00:00, 10.12it/s]Capturing num tokens (num_tokens=4 avail_mem=21.58 GB): 100%|██████████| 58/58 [00:11<00:00,  5.13it/s]



<strong style='color: #00008B;'>==== Original Output ====</strong>



<strong style='color: #00008B;'>To solve the problem of adding 1 and 3, I start by identifying the two numbers involved.<br><br>Next, I perform the addition of these two numbers to find the sum.<br><br>Finally, I conclude that the result of 1 plus 3 is 4.<br></think><br><br>To solve the problem \(1 + 3\), follow these easy steps:<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>3. **Present the final answer:**<br>   \[<br>   \boxed{4}<br>   \]</strong>



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>To solve the problem of adding 1 and 3, I start by identifying the two numbers involved.<br><br>Next, I perform the addition of these two numbers to find the sum.<br><br>Finally, I conclude that the result of 1 plus 3 is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>To solve the problem \(1 + 3\), follow these easy steps:<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>3. **Present the final answer:**<br>   \[<br>   \boxed{4}<br>   \]</strong>



```python
llm.shutdown()
```

## Supporting New Reasoning Model Schemas

For future reasoning models, you can implement the reasoning parser as a subclass of `BaseReasoningFormatDetector` in `python/sglang/srt/reasoning_parser.py` and specify the reasoning parser for new reasoning model schemas accordingly.
