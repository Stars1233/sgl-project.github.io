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

    Multi-thread loading shards:  50% Completed | 1/2 [00:00<00:00,  1.33it/s]

    Multi-thread loading shards: 100% Completed | 2/2 [00:01<00:00,  1.03s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:01<00:00,  1.01it/s]


    2026-04-04 05:27:38,475 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 05:27:38] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:55,  3.07s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:55,  3.07s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:23,  1.49s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:23,  1.49s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:49,  1.11it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:49,  1.11it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:33,  1.63it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:33,  1.63it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:03<00:23,  2.23it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:03<00:23,  2.23it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.86it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.86it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.60it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.60it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.39it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.39it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.28it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.28it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.28it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:06,  6.97it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:06,  6.97it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:06,  6.97it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:04<00:05,  8.46it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:04<00:05,  8.46it/s]

    Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:04<00:05,  8.46it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:04<00:04, 10.04it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:04<00:04, 10.04it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:05<00:04, 10.04it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:03, 11.96it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:03, 11.96it/s]

    Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:03, 11.96it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:05<00:03, 11.96it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 15.19it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 15.19it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 15.19it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 15.19it/s]Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:05<00:02, 15.19it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:05<00:01, 20.17it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:05<00:01, 20.17it/s]

    Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:05<00:01, 20.17it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:05<00:01, 20.17it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:05<00:01, 20.17it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:05<00:01, 20.17it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:05<00:01, 26.75it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:05<00:01, 26.75it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:05<00:01, 26.75it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:05<00:01, 26.75it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:05<00:01, 26.75it/s]Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:05<00:01, 26.75it/s]

    Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:05<00:00, 29.86it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:05<00:00, 29.86it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:05<00:00, 29.86it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:05<00:00, 29.86it/s]Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:05<00:00, 29.86it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:05<00:00, 27.57it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:05<00:00, 27.57it/s]

    Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:05<00:00, 27.57it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:05<00:00, 27.57it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:05<00:00, 26.75it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:05<00:00, 26.75it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:05<00:00, 26.75it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:05<00:00, 26.75it/s]

    Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:06<00:00, 24.16it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:06<00:00, 24.16it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:06<00:00, 24.16it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:06<00:00, 24.16it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:06<00:00, 23.21it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:06<00:00, 23.21it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:06<00:00, 23.21it/s]

    Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:06<00:00, 23.21it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:06<00:00, 22.37it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:06<00:00, 22.37it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:06<00:00, 22.37it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:06<00:00, 22.37it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:06<00:00, 22.71it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:06<00:00, 22.71it/s]

    Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:06<00:00, 22.71it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:06<00:00, 22.71it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:06<00:00, 23.00it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:06<00:00, 23.00it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:06<00:00, 23.00it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:06<00:00,  8.75it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=93.05 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=93.05 GB):   2%|▏         | 1/58 [00:00<00:36,  1.54it/s]Capturing num tokens (num_tokens=7680 avail_mem=93.02 GB):   2%|▏         | 1/58 [00:00<00:36,  1.54it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=93.02 GB):   3%|▎         | 2/58 [00:01<00:33,  1.68it/s]Capturing num tokens (num_tokens=7168 avail_mem=93.02 GB):   3%|▎         | 2/58 [00:01<00:33,  1.68it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=93.02 GB):   5%|▌         | 3/58 [00:01<00:30,  1.80it/s]Capturing num tokens (num_tokens=6656 avail_mem=93.02 GB):   5%|▌         | 3/58 [00:01<00:30,  1.80it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=93.02 GB):   7%|▋         | 4/58 [00:02<00:27,  1.93it/s]Capturing num tokens (num_tokens=6144 avail_mem=93.02 GB):   7%|▋         | 4/58 [00:02<00:27,  1.93it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=93.02 GB):   9%|▊         | 5/58 [00:02<00:25,  2.06it/s]Capturing num tokens (num_tokens=5632 avail_mem=93.03 GB):   9%|▊         | 5/58 [00:02<00:25,  2.06it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=93.03 GB):  10%|█         | 6/58 [00:02<00:23,  2.24it/s]Capturing num tokens (num_tokens=5120 avail_mem=93.03 GB):  10%|█         | 6/58 [00:02<00:23,  2.24it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=93.03 GB):  12%|█▏        | 7/58 [00:03<00:20,  2.46it/s]Capturing num tokens (num_tokens=4608 avail_mem=92.99 GB):  12%|█▏        | 7/58 [00:03<00:20,  2.46it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=92.99 GB):  14%|█▍        | 8/58 [00:03<00:17,  2.78it/s]Capturing num tokens (num_tokens=4096 avail_mem=92.55 GB):  14%|█▍        | 8/58 [00:03<00:17,  2.78it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=92.55 GB):  16%|█▌        | 9/58 [00:03<00:15,  3.18it/s]Capturing num tokens (num_tokens=3840 avail_mem=92.16 GB):  16%|█▌        | 9/58 [00:03<00:15,  3.18it/s]Capturing num tokens (num_tokens=3840 avail_mem=92.16 GB):  17%|█▋        | 10/58 [00:03<00:13,  3.60it/s]Capturing num tokens (num_tokens=3584 avail_mem=91.74 GB):  17%|█▋        | 10/58 [00:03<00:13,  3.60it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=91.74 GB):  19%|█▉        | 11/58 [00:04<00:11,  4.14it/s]Capturing num tokens (num_tokens=3328 avail_mem=91.73 GB):  19%|█▉        | 11/58 [00:04<00:11,  4.14it/s]Capturing num tokens (num_tokens=3328 avail_mem=91.73 GB):  21%|██        | 12/58 [00:04<00:09,  4.67it/s]Capturing num tokens (num_tokens=3072 avail_mem=91.70 GB):  21%|██        | 12/58 [00:04<00:09,  4.67it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=91.70 GB):  22%|██▏       | 13/58 [00:04<00:08,  5.14it/s]Capturing num tokens (num_tokens=2816 avail_mem=90.88 GB):  22%|██▏       | 13/58 [00:04<00:08,  5.14it/s]Capturing num tokens (num_tokens=2816 avail_mem=90.88 GB):  24%|██▍       | 14/58 [00:04<00:07,  5.94it/s]Capturing num tokens (num_tokens=2560 avail_mem=89.19 GB):  24%|██▍       | 14/58 [00:04<00:07,  5.94it/s]Capturing num tokens (num_tokens=2304 avail_mem=89.19 GB):  24%|██▍       | 14/58 [00:04<00:07,  5.94it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=89.19 GB):  28%|██▊       | 16/58 [00:04<00:05,  7.99it/s]Capturing num tokens (num_tokens=2048 avail_mem=89.19 GB):  28%|██▊       | 16/58 [00:04<00:05,  7.99it/s]Capturing num tokens (num_tokens=1792 avail_mem=89.19 GB):  28%|██▊       | 16/58 [00:04<00:05,  7.99it/s]Capturing num tokens (num_tokens=1792 avail_mem=89.19 GB):  31%|███       | 18/58 [00:04<00:03, 10.04it/s]Capturing num tokens (num_tokens=1536 avail_mem=89.18 GB):  31%|███       | 18/58 [00:04<00:03, 10.04it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=88.15 GB):  31%|███       | 18/58 [00:04<00:03, 10.04it/s]Capturing num tokens (num_tokens=1280 avail_mem=88.15 GB):  34%|███▍      | 20/58 [00:05<00:04,  8.84it/s]Capturing num tokens (num_tokens=1024 avail_mem=88.15 GB):  34%|███▍      | 20/58 [00:05<00:04,  8.84it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=88.15 GB):  36%|███▌      | 21/58 [00:05<00:04,  8.87it/s]Capturing num tokens (num_tokens=960 avail_mem=89.81 GB):  36%|███▌      | 21/58 [00:05<00:04,  8.87it/s] Capturing num tokens (num_tokens=960 avail_mem=89.81 GB):  38%|███▊      | 22/58 [00:05<00:03,  9.07it/s]Capturing num tokens (num_tokens=896 avail_mem=89.14 GB):  38%|███▊      | 22/58 [00:05<00:03,  9.07it/s]

    Capturing num tokens (num_tokens=896 avail_mem=89.14 GB):  40%|███▉      | 23/58 [00:05<00:03,  9.10it/s]Capturing num tokens (num_tokens=832 avail_mem=88.32 GB):  40%|███▉      | 23/58 [00:05<00:03,  9.10it/s]Capturing num tokens (num_tokens=832 avail_mem=88.32 GB):  41%|████▏     | 24/58 [00:05<00:03,  8.70it/s]Capturing num tokens (num_tokens=768 avail_mem=88.32 GB):  41%|████▏     | 24/58 [00:05<00:03,  8.70it/s]

    Capturing num tokens (num_tokens=768 avail_mem=88.32 GB):  43%|████▎     | 25/58 [00:05<00:03,  8.53it/s]Capturing num tokens (num_tokens=704 avail_mem=88.31 GB):  43%|████▎     | 25/58 [00:05<00:03,  8.53it/s]Capturing num tokens (num_tokens=640 avail_mem=89.13 GB):  43%|████▎     | 25/58 [00:05<00:03,  8.53it/s]Capturing num tokens (num_tokens=640 avail_mem=89.13 GB):  47%|████▋     | 27/58 [00:05<00:03,  9.76it/s]Capturing num tokens (num_tokens=576 avail_mem=88.37 GB):  47%|████▋     | 27/58 [00:05<00:03,  9.76it/s]

    Capturing num tokens (num_tokens=576 avail_mem=88.37 GB):  48%|████▊     | 28/58 [00:05<00:03,  9.42it/s]Capturing num tokens (num_tokens=512 avail_mem=88.36 GB):  48%|████▊     | 28/58 [00:05<00:03,  9.42it/s]Capturing num tokens (num_tokens=512 avail_mem=88.36 GB):  50%|█████     | 29/58 [00:06<00:03,  9.34it/s]Capturing num tokens (num_tokens=480 avail_mem=88.36 GB):  50%|█████     | 29/58 [00:06<00:03,  9.34it/s]

    Capturing num tokens (num_tokens=448 avail_mem=89.12 GB):  50%|█████     | 29/58 [00:06<00:03,  9.34it/s]Capturing num tokens (num_tokens=448 avail_mem=89.12 GB):  53%|█████▎    | 31/58 [00:06<00:02,  9.74it/s]Capturing num tokens (num_tokens=416 avail_mem=88.41 GB):  53%|█████▎    | 31/58 [00:06<00:02,  9.74it/s]

    Capturing num tokens (num_tokens=416 avail_mem=88.41 GB):  55%|█████▌    | 32/58 [00:06<00:02,  9.60it/s]Capturing num tokens (num_tokens=384 avail_mem=88.41 GB):  55%|█████▌    | 32/58 [00:06<00:02,  9.60it/s]Capturing num tokens (num_tokens=352 avail_mem=89.12 GB):  55%|█████▌    | 32/58 [00:06<00:02,  9.60it/s]Capturing num tokens (num_tokens=352 avail_mem=89.12 GB):  59%|█████▊    | 34/58 [00:06<00:02,  9.96it/s]Capturing num tokens (num_tokens=320 avail_mem=89.11 GB):  59%|█████▊    | 34/58 [00:06<00:02,  9.96it/s]

    Capturing num tokens (num_tokens=288 avail_mem=88.45 GB):  59%|█████▊    | 34/58 [00:06<00:02,  9.96it/s]Capturing num tokens (num_tokens=288 avail_mem=88.45 GB):  62%|██████▏   | 36/58 [00:06<00:02, 10.10it/s]Capturing num tokens (num_tokens=256 avail_mem=88.45 GB):  62%|██████▏   | 36/58 [00:06<00:02, 10.10it/s]Capturing num tokens (num_tokens=240 avail_mem=89.10 GB):  62%|██████▏   | 36/58 [00:06<00:02, 10.10it/s]

    Capturing num tokens (num_tokens=240 avail_mem=89.10 GB):  66%|██████▌   | 38/58 [00:06<00:01, 10.54it/s]Capturing num tokens (num_tokens=224 avail_mem=88.50 GB):  66%|██████▌   | 38/58 [00:06<00:01, 10.54it/s]Capturing num tokens (num_tokens=208 avail_mem=88.49 GB):  66%|██████▌   | 38/58 [00:07<00:01, 10.54it/s]Capturing num tokens (num_tokens=208 avail_mem=88.49 GB):  69%|██████▉   | 40/58 [00:07<00:01, 10.62it/s]Capturing num tokens (num_tokens=192 avail_mem=89.10 GB):  69%|██████▉   | 40/58 [00:07<00:01, 10.62it/s]

    Capturing num tokens (num_tokens=176 avail_mem=88.55 GB):  69%|██████▉   | 40/58 [00:07<00:01, 10.62it/s]Capturing num tokens (num_tokens=176 avail_mem=88.55 GB):  72%|███████▏  | 42/58 [00:07<00:01, 10.77it/s]Capturing num tokens (num_tokens=160 avail_mem=89.14 GB):  72%|███████▏  | 42/58 [00:07<00:01, 10.77it/s]Capturing num tokens (num_tokens=144 avail_mem=89.09 GB):  72%|███████▏  | 42/58 [00:07<00:01, 10.77it/s]

    Capturing num tokens (num_tokens=144 avail_mem=89.09 GB):  76%|███████▌  | 44/58 [00:07<00:01, 11.29it/s]Capturing num tokens (num_tokens=128 avail_mem=88.61 GB):  76%|███████▌  | 44/58 [00:07<00:01, 11.29it/s]Capturing num tokens (num_tokens=112 avail_mem=89.10 GB):  76%|███████▌  | 44/58 [00:07<00:01, 11.29it/s]Capturing num tokens (num_tokens=112 avail_mem=89.10 GB):  79%|███████▉  | 46/58 [00:07<00:01, 11.99it/s]Capturing num tokens (num_tokens=96 avail_mem=88.66 GB):  79%|███████▉  | 46/58 [00:07<00:01, 11.99it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=88.65 GB):  79%|███████▉  | 46/58 [00:07<00:01, 11.99it/s]Capturing num tokens (num_tokens=80 avail_mem=88.65 GB):  83%|████████▎ | 48/58 [00:07<00:00, 12.19it/s]Capturing num tokens (num_tokens=64 avail_mem=89.09 GB):  83%|████████▎ | 48/58 [00:07<00:00, 12.19it/s]Capturing num tokens (num_tokens=48 avail_mem=88.67 GB):  83%|████████▎ | 48/58 [00:07<00:00, 12.19it/s]

    Capturing num tokens (num_tokens=48 avail_mem=88.67 GB):  86%|████████▌ | 50/58 [00:07<00:00, 12.35it/s]Capturing num tokens (num_tokens=32 avail_mem=89.08 GB):  86%|████████▌ | 50/58 [00:07<00:00, 12.35it/s]Capturing num tokens (num_tokens=28 avail_mem=88.70 GB):  86%|████████▌ | 50/58 [00:07<00:00, 12.35it/s]Capturing num tokens (num_tokens=28 avail_mem=88.70 GB):  90%|████████▉ | 52/58 [00:08<00:00, 12.67it/s]Capturing num tokens (num_tokens=24 avail_mem=89.14 GB):  90%|████████▉ | 52/58 [00:08<00:00, 12.67it/s]Capturing num tokens (num_tokens=20 avail_mem=89.07 GB):  90%|████████▉ | 52/58 [00:08<00:00, 12.67it/s]

    Capturing num tokens (num_tokens=20 avail_mem=89.07 GB):  93%|█████████▎| 54/58 [00:08<00:00, 13.23it/s]Capturing num tokens (num_tokens=16 avail_mem=88.71 GB):  93%|█████████▎| 54/58 [00:08<00:00, 13.23it/s]Capturing num tokens (num_tokens=12 avail_mem=89.07 GB):  93%|█████████▎| 54/58 [00:08<00:00, 13.23it/s]Capturing num tokens (num_tokens=12 avail_mem=89.07 GB):  97%|█████████▋| 56/58 [00:08<00:00, 13.17it/s]Capturing num tokens (num_tokens=8 avail_mem=88.73 GB):  97%|█████████▋| 56/58 [00:08<00:00, 13.17it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=89.06 GB):  97%|█████████▋| 56/58 [00:08<00:00, 13.17it/s]Capturing num tokens (num_tokens=4 avail_mem=89.06 GB): 100%|██████████| 58/58 [00:08<00:00, 11.91it/s]Capturing num tokens (num_tokens=4 avail_mem=89.06 GB): 100%|██████████| 58/58 [00:08<00:00,  6.79it/s]


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



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>I start by identifying the two numbers involved in the addition: 1 and 3.<br><br>Next, I perform the addition operation by adding these two numbers together.<br><br>Finally, I calculate the result to find that 1 plus 3 equals 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>Certainly! Let's solve the addition problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Perform the addition:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:** \(\boxed{4}\)</strong>


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



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I add the two numbers together: 1 plus 3 equals 4.<br><br>Finally, I conclude that the answer is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'><br><br>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>**Answer:** \(\boxed{4}\)</strong>


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



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I add the two numbers together: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.</strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'><br><br>**Solution:**<br><br>We need to find the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>**Answer:** \boxed{4}</strong>


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



<strong style='color: #00008B;'>First, I need to identify the two numbers in the problem, which are 1 and 3.<br><br>Next, I will add these two numbers together.<br><br>Adding 1 and 3 gives a sum of 4.<br><br>Therefore, the final answer is 4.<br></think><br><br>**Solution:**<br><br>We need to find the sum of the numbers 1 and 3.<br><br>1. **Identify the numbers:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Final Answer:**<br>\[<br>\boxed{4}<br>\]</strong>


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



<strong style='color: #00008B;'>First, I need to identify the two numbers in the problem: 1 and 3.<br><br>Next, I'll add these two numbers together.<br><br>Finally, the sum of 1 and 3 is 4.<br></think><br><br>Sure! Let's solve the problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:** \(\boxed{4}\)</strong>


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:328: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      return await dependant.call(**values)



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>First, I need to identify the two numbers in the problem: 1 and 3.<br><br>Next, I'll add these two numbers together.<br><br>Finally, the sum of 1 and 3 is 4.<br></strong>



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


    Multi-thread loading shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Multi-thread loading shards:  50% Completed | 1/2 [00:00<00:00,  1.33it/s]

    Multi-thread loading shards: 100% Completed | 2/2 [00:01<00:00,  1.04s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:01<00:00,  1.01it/s]


    2026-04-04 05:28:27,900 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 05:28:27] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:53,  3.04s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:53,  3.04s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:23,  1.48s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:23,  1.48s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:49,  1.12it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:49,  1.12it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:32,  1.64it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:32,  1.64it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:03<00:23,  2.24it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:03<00:23,  2.24it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.82it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.82it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:16,  3.00it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:16,  3.00it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:15,  3.19it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:15,  3.19it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:14,  3.48it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:14,  3.48it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:05<00:12,  3.87it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:05<00:12,  3.87it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:05<00:11,  4.13it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:05<00:11,  4.13it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:05<00:10,  4.49it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:05<00:10,  4.49it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:05<00:09,  4.82it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:05<00:09,  4.82it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:05<00:08,  5.29it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:05<00:08,  5.29it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:05<00:07,  5.74it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:05<00:07,  5.74it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:06<00:06,  6.35it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:06<00:06,  6.35it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:06<00:05,  6.85it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:06<00:05,  6.85it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:06<00:05,  7.43it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:06<00:05,  7.43it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:06<00:05,  7.43it/s]

    Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:06<00:04,  9.03it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:06<00:04,  9.03it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:06<00:04,  9.03it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:06<00:03, 10.89it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:06<00:03, 10.89it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:06<00:03, 10.89it/s]

    Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:06<00:02, 12.09it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:06<00:02, 12.09it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:06<00:02, 12.09it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:06<00:02, 13.01it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:06<00:02, 13.01it/s]

    Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:06<00:02, 13.01it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:07<00:02, 12.83it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:07<00:02, 12.83it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:07<00:02, 12.83it/s]

    Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:07<00:02, 13.53it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:07<00:02, 13.53it/s]Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:07<00:02, 13.53it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:07<00:01, 14.02it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:07<00:01, 14.02it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:07<00:01, 14.02it/s]

    Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:07<00:01, 14.60it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:07<00:01, 14.60it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:07<00:01, 14.60it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:07<00:01, 14.84it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:07<00:01, 14.84it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:07<00:01, 14.84it/s]

    Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:07<00:01, 15.39it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:07<00:01, 15.39it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:07<00:01, 15.39it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:07<00:01, 16.04it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:07<00:01, 16.04it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:07<00:01, 16.04it/s]

    Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:07<00:00, 16.60it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:07<00:00, 16.60it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:07<00:00, 16.60it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:07<00:00, 16.75it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:07<00:00, 16.75it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:08<00:00, 16.75it/s]

    Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:08<00:00, 16.75it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:08<00:00, 17.86it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:08<00:00, 17.86it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:08<00:00, 17.86it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:08<00:00, 17.75it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:08<00:00, 17.75it/s]

    Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:08<00:00, 17.75it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:08<00:00, 18.10it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:08<00:00, 18.10it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:08<00:00, 18.10it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:08<00:00, 18.10it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:08<00:00, 19.11it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:08<00:00, 19.11it/s]

    Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:08<00:00, 19.11it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:08<00:00, 19.11it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:08<00:00, 20.84it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:08<00:00, 20.84it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:08<00:00,  6.69it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=85.22 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=85.22 GB):   2%|▏         | 1/58 [00:00<00:47,  1.20it/s]Capturing num tokens (num_tokens=7680 avail_mem=85.19 GB):   2%|▏         | 1/58 [00:00<00:47,  1.20it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=85.19 GB):   3%|▎         | 2/58 [00:01<00:43,  1.28it/s]Capturing num tokens (num_tokens=7168 avail_mem=85.18 GB):   3%|▎         | 2/58 [00:01<00:43,  1.28it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=85.18 GB):   5%|▌         | 3/58 [00:02<00:37,  1.45it/s]Capturing num tokens (num_tokens=6656 avail_mem=85.18 GB):   5%|▌         | 3/58 [00:02<00:37,  1.45it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=85.18 GB):   7%|▋         | 4/58 [00:02<00:31,  1.69it/s]Capturing num tokens (num_tokens=6144 avail_mem=85.19 GB):   7%|▋         | 4/58 [00:02<00:31,  1.69it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=85.19 GB):   9%|▊         | 5/58 [00:03<00:32,  1.65it/s]Capturing num tokens (num_tokens=5632 avail_mem=84.00 GB):   9%|▊         | 5/58 [00:03<00:32,  1.65it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=84.00 GB):  10%|█         | 6/58 [00:03<00:29,  1.76it/s]Capturing num tokens (num_tokens=5120 avail_mem=84.17 GB):  10%|█         | 6/58 [00:03<00:29,  1.76it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=84.17 GB):  12%|█▏        | 7/58 [00:04<00:27,  1.82it/s]Capturing num tokens (num_tokens=4608 avail_mem=85.17 GB):  12%|█▏        | 7/58 [00:04<00:27,  1.82it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=85.17 GB):  14%|█▍        | 8/58 [00:04<00:25,  1.97it/s]Capturing num tokens (num_tokens=4096 avail_mem=84.25 GB):  14%|█▍        | 8/58 [00:04<00:25,  1.97it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=84.25 GB):  16%|█▌        | 9/58 [00:05<00:22,  2.17it/s]Capturing num tokens (num_tokens=3840 avail_mem=84.32 GB):  16%|█▌        | 9/58 [00:05<00:22,  2.17it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=84.32 GB):  17%|█▋        | 10/58 [00:05<00:20,  2.38it/s]Capturing num tokens (num_tokens=3584 avail_mem=85.18 GB):  17%|█▋        | 10/58 [00:05<00:20,  2.38it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=85.18 GB):  19%|█▉        | 11/58 [00:05<00:18,  2.56it/s]Capturing num tokens (num_tokens=3328 avail_mem=84.39 GB):  19%|█▉        | 11/58 [00:05<00:18,  2.56it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=84.39 GB):  21%|██        | 12/58 [00:06<00:18,  2.55it/s]Capturing num tokens (num_tokens=3072 avail_mem=84.45 GB):  21%|██        | 12/58 [00:06<00:18,  2.55it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=84.45 GB):  22%|██▏       | 13/58 [00:06<00:16,  2.77it/s]Capturing num tokens (num_tokens=2816 avail_mem=84.52 GB):  22%|██▏       | 13/58 [00:06<00:16,  2.77it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=84.52 GB):  24%|██▍       | 14/58 [00:06<00:15,  2.86it/s]Capturing num tokens (num_tokens=2560 avail_mem=84.15 GB):  24%|██▍       | 14/58 [00:06<00:15,  2.86it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=84.15 GB):  26%|██▌       | 15/58 [00:06<00:14,  2.92it/s]Capturing num tokens (num_tokens=2304 avail_mem=84.16 GB):  26%|██▌       | 15/58 [00:07<00:14,  2.92it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=84.16 GB):  28%|██▊       | 16/58 [00:07<00:12,  3.23it/s]Capturing num tokens (num_tokens=2048 avail_mem=84.63 GB):  28%|██▊       | 16/58 [00:07<00:12,  3.23it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=84.63 GB):  29%|██▉       | 17/58 [00:07<00:11,  3.49it/s]Capturing num tokens (num_tokens=1792 avail_mem=83.84 GB):  29%|██▉       | 17/58 [00:07<00:11,  3.49it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=83.84 GB):  31%|███       | 18/58 [00:07<00:11,  3.52it/s]Capturing num tokens (num_tokens=1536 avail_mem=84.33 GB):  31%|███       | 18/58 [00:07<00:11,  3.52it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=84.33 GB):  33%|███▎      | 19/58 [00:07<00:10,  3.81it/s]Capturing num tokens (num_tokens=1280 avail_mem=84.69 GB):  33%|███▎      | 19/58 [00:07<00:10,  3.81it/s]Capturing num tokens (num_tokens=1280 avail_mem=84.69 GB):  34%|███▍      | 20/58 [00:08<00:08,  4.37it/s]Capturing num tokens (num_tokens=1024 avail_mem=83.96 GB):  34%|███▍      | 20/58 [00:08<00:08,  4.37it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=83.96 GB):  36%|███▌      | 21/58 [00:08<00:08,  4.46it/s]Capturing num tokens (num_tokens=960 avail_mem=84.39 GB):  36%|███▌      | 21/58 [00:08<00:08,  4.46it/s] Capturing num tokens (num_tokens=960 avail_mem=84.39 GB):  38%|███▊      | 22/58 [00:08<00:07,  4.66it/s]Capturing num tokens (num_tokens=896 avail_mem=84.76 GB):  38%|███▊      | 22/58 [00:08<00:07,  4.66it/s]

    Capturing num tokens (num_tokens=896 avail_mem=84.76 GB):  40%|███▉      | 23/58 [00:08<00:06,  5.10it/s]Capturing num tokens (num_tokens=832 avail_mem=84.44 GB):  40%|███▉      | 23/58 [00:08<00:06,  5.10it/s]Capturing num tokens (num_tokens=832 avail_mem=84.44 GB):  41%|████▏     | 24/58 [00:08<00:06,  5.10it/s]Capturing num tokens (num_tokens=768 avail_mem=84.44 GB):  41%|████▏     | 24/58 [00:08<00:06,  5.10it/s]

    Capturing num tokens (num_tokens=768 avail_mem=84.44 GB):  43%|████▎     | 25/58 [00:09<00:06,  5.30it/s]Capturing num tokens (num_tokens=704 avail_mem=84.82 GB):  43%|████▎     | 25/58 [00:09<00:06,  5.30it/s]Capturing num tokens (num_tokens=704 avail_mem=84.82 GB):  45%|████▍     | 26/58 [00:09<00:05,  5.88it/s]Capturing num tokens (num_tokens=640 avail_mem=84.19 GB):  45%|████▍     | 26/58 [00:09<00:05,  5.88it/s]

    Capturing num tokens (num_tokens=640 avail_mem=84.19 GB):  47%|████▋     | 27/58 [00:09<00:05,  5.80it/s]Capturing num tokens (num_tokens=576 avail_mem=84.49 GB):  47%|████▋     | 27/58 [00:09<00:05,  5.80it/s]Capturing num tokens (num_tokens=576 avail_mem=84.49 GB):  48%|████▊     | 28/58 [00:09<00:05,  5.93it/s]Capturing num tokens (num_tokens=512 avail_mem=85.14 GB):  48%|████▊     | 28/58 [00:09<00:05,  5.93it/s]

    Capturing num tokens (num_tokens=512 avail_mem=85.14 GB):  50%|█████     | 29/58 [00:09<00:04,  6.68it/s]Capturing num tokens (num_tokens=480 avail_mem=84.30 GB):  50%|█████     | 29/58 [00:09<00:04,  6.68it/s]Capturing num tokens (num_tokens=480 avail_mem=84.30 GB):  52%|█████▏    | 30/58 [00:09<00:04,  6.78it/s]Capturing num tokens (num_tokens=448 avail_mem=84.52 GB):  52%|█████▏    | 30/58 [00:09<00:04,  6.78it/s]

    Capturing num tokens (num_tokens=448 avail_mem=84.52 GB):  53%|█████▎    | 31/58 [00:09<00:03,  7.02it/s]Capturing num tokens (num_tokens=416 avail_mem=85.13 GB):  53%|█████▎    | 31/58 [00:09<00:03,  7.02it/s]Capturing num tokens (num_tokens=416 avail_mem=85.13 GB):  55%|█████▌    | 32/58 [00:09<00:03,  7.48it/s]Capturing num tokens (num_tokens=384 avail_mem=84.58 GB):  55%|█████▌    | 32/58 [00:09<00:03,  7.48it/s]

    Capturing num tokens (num_tokens=384 avail_mem=84.58 GB):  57%|█████▋    | 33/58 [00:10<00:03,  7.36it/s]Capturing num tokens (num_tokens=352 avail_mem=84.57 GB):  57%|█████▋    | 33/58 [00:10<00:03,  7.36it/s]Capturing num tokens (num_tokens=352 avail_mem=84.57 GB):  59%|█████▊    | 34/58 [00:10<00:03,  7.86it/s]Capturing num tokens (num_tokens=320 avail_mem=85.11 GB):  59%|█████▊    | 34/58 [00:10<00:03,  7.86it/s]

    Capturing num tokens (num_tokens=320 avail_mem=85.11 GB):  60%|██████    | 35/58 [00:10<00:02,  8.33it/s]Capturing num tokens (num_tokens=288 avail_mem=84.50 GB):  60%|██████    | 35/58 [00:10<00:02,  8.33it/s]Capturing num tokens (num_tokens=288 avail_mem=84.50 GB):  62%|██████▏   | 36/58 [00:10<00:03,  6.98it/s]Capturing num tokens (num_tokens=256 avail_mem=84.99 GB):  62%|██████▏   | 36/58 [00:10<00:03,  6.98it/s]

    Capturing num tokens (num_tokens=240 avail_mem=84.56 GB):  62%|██████▏   | 36/58 [00:10<00:03,  6.98it/s]Capturing num tokens (num_tokens=240 avail_mem=84.56 GB):  66%|██████▌   | 38/58 [00:10<00:02,  8.10it/s]Capturing num tokens (num_tokens=224 avail_mem=85.10 GB):  66%|██████▌   | 38/58 [00:10<00:02,  8.10it/s]Capturing num tokens (num_tokens=208 avail_mem=84.67 GB):  66%|██████▌   | 38/58 [00:10<00:02,  8.10it/s]

    Capturing num tokens (num_tokens=208 avail_mem=84.67 GB):  69%|██████▉   | 40/58 [00:10<00:02,  8.73it/s]Capturing num tokens (num_tokens=192 avail_mem=84.67 GB):  69%|██████▉   | 40/58 [00:10<00:02,  8.73it/s]Capturing num tokens (num_tokens=176 avail_mem=85.07 GB):  69%|██████▉   | 40/58 [00:11<00:02,  8.73it/s]Capturing num tokens (num_tokens=176 avail_mem=85.07 GB):  72%|███████▏  | 42/58 [00:11<00:01,  9.38it/s]Capturing num tokens (num_tokens=160 avail_mem=84.69 GB):  72%|███████▏  | 42/58 [00:11<00:01,  9.38it/s]

    Capturing num tokens (num_tokens=144 avail_mem=85.06 GB):  72%|███████▏  | 42/58 [00:11<00:01,  9.38it/s]Capturing num tokens (num_tokens=144 avail_mem=85.06 GB):  76%|███████▌  | 44/58 [00:11<00:01,  9.82it/s]Capturing num tokens (num_tokens=128 avail_mem=84.70 GB):  76%|███████▌  | 44/58 [00:11<00:01,  9.82it/s]Capturing num tokens (num_tokens=112 avail_mem=85.06 GB):  76%|███████▌  | 44/58 [00:11<00:01,  9.82it/s]

    Capturing num tokens (num_tokens=112 avail_mem=85.06 GB):  79%|███████▉  | 46/58 [00:11<00:01, 10.53it/s]Capturing num tokens (num_tokens=96 avail_mem=84.72 GB):  79%|███████▉  | 46/58 [00:11<00:01, 10.53it/s] Capturing num tokens (num_tokens=80 avail_mem=85.04 GB):  79%|███████▉  | 46/58 [00:11<00:01, 10.53it/s]Capturing num tokens (num_tokens=80 avail_mem=85.04 GB):  83%|████████▎ | 48/58 [00:11<00:00, 10.55it/s]Capturing num tokens (num_tokens=64 avail_mem=84.73 GB):  83%|████████▎ | 48/58 [00:11<00:00, 10.55it/s]

    Capturing num tokens (num_tokens=48 avail_mem=85.02 GB):  83%|████████▎ | 48/58 [00:11<00:00, 10.55it/s]Capturing num tokens (num_tokens=48 avail_mem=85.02 GB):  86%|████████▌ | 50/58 [00:11<00:00, 11.26it/s]Capturing num tokens (num_tokens=32 avail_mem=85.06 GB):  86%|████████▌ | 50/58 [00:11<00:00, 11.26it/s]Capturing num tokens (num_tokens=28 avail_mem=85.01 GB):  86%|████████▌ | 50/58 [00:11<00:00, 11.26it/s]

    Capturing num tokens (num_tokens=28 avail_mem=85.01 GB):  90%|████████▉ | 52/58 [00:11<00:00, 12.14it/s]Capturing num tokens (num_tokens=24 avail_mem=85.01 GB):  90%|████████▉ | 52/58 [00:11<00:00, 12.14it/s]Capturing num tokens (num_tokens=20 avail_mem=84.94 GB):  90%|████████▉ | 52/58 [00:11<00:00, 12.14it/s]Capturing num tokens (num_tokens=20 avail_mem=84.94 GB):  93%|█████████▎| 54/58 [00:12<00:00, 13.16it/s]Capturing num tokens (num_tokens=16 avail_mem=84.99 GB):  93%|█████████▎| 54/58 [00:12<00:00, 13.16it/s]Capturing num tokens (num_tokens=12 avail_mem=84.79 GB):  93%|█████████▎| 54/58 [00:12<00:00, 13.16it/s]

    Capturing num tokens (num_tokens=12 avail_mem=84.79 GB):  97%|█████████▋| 56/58 [00:12<00:00, 14.11it/s]Capturing num tokens (num_tokens=8 avail_mem=84.97 GB):  97%|█████████▋| 56/58 [00:12<00:00, 14.11it/s] Capturing num tokens (num_tokens=4 avail_mem=84.98 GB):  97%|█████████▋| 56/58 [00:12<00:00, 14.11it/s]Capturing num tokens (num_tokens=4 avail_mem=84.98 GB): 100%|██████████| 58/58 [00:12<00:00, 14.44it/s]Capturing num tokens (num_tokens=4 avail_mem=84.98 GB): 100%|██████████| 58/58 [00:12<00:00,  4.71it/s]



<strong style='color: #00008B;'>==== Original Output ====</strong>



<strong style='color: #00008B;'>First, I recognize that I need to calculate the sum of 1 and 3.<br><br>Adding these two numbers together gives me 4.<br></think><br><br>**Solution:**<br><br>We need to calculate the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>**Answer:** \(\boxed{4}\)</strong>



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>First, I recognize that I need to calculate the sum of 1 and 3.<br><br>Adding these two numbers together gives me 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>**Solution:**<br><br>We need to calculate the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>**Answer:** \(\boxed{4}\)</strong>



```python
llm.shutdown()
```

## Supporting New Reasoning Model Schemas

For future reasoning models, you can implement the reasoning parser as a subclass of `BaseReasoningFormatDetector` in `python/sglang/srt/reasoning_parser.py` and specify the reasoning parser for new reasoning model schemas accordingly.
