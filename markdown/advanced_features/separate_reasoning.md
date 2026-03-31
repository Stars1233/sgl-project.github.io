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


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:173: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-03-31 21:07:43.911 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:07:43] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:07:43.911 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:07:43] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:07:43.911 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:07:43] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:07:43.912 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:07:43] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:07:43.912 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:07:43] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:01<00:01,  1.16s/it]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.51s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.46s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:08,  3.30s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:08,  3.30s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:04<01:47,  1.92s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:04<01:47,  1.92s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:08,  1.24s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:08,  1.24s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:05<00:50,  1.07it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:05<00:50,  1.07it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:38,  1.37it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:38,  1.37it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:31,  1.67it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:31,  1.67it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:06<00:25,  2.01it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:06<00:25,  2.01it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:06<00:21,  2.35it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:06<00:21,  2.35it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:06<00:18,  2.72it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:06<00:18,  2.72it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:06<00:15,  3.12it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:06<00:15,  3.12it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:07<00:13,  3.50it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:07<00:13,  3.50it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:07<00:11,  3.96it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:07<00:11,  3.96it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:07<00:10,  4.39it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:07<00:10,  4.39it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:07<00:09,  4.75it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:07<00:09,  4.75it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:07<00:08,  5.28it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:07<00:08,  5.28it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:07<00:07,  5.87it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:07<00:07,  5.87it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:07<00:06,  6.57it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:07<00:06,  6.57it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:08<00:05,  7.16it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:08<00:05,  7.16it/s]

    Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:08<00:05,  7.16it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:08<00:04,  8.69it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:08<00:04,  8.69it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:08<00:04,  8.69it/s] 

    Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:08<00:03, 10.29it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:08<00:03, 10.29it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:08<00:03, 10.29it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:08<00:02, 12.01it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:08<00:02, 12.01it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:08<00:02, 12.01it/s]

    Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:08<00:02, 12.01it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:08<00:02, 14.90it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:08<00:02, 14.90it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:08<00:02, 14.90it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:08<00:02, 14.90it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:08<00:01, 17.39it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:08<00:01, 17.39it/s]

    Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:08<00:01, 17.39it/s]Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:08<00:01, 17.39it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:08<00:01, 19.76it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:08<00:01, 19.76it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:08<00:01, 19.76it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:08<00:01, 19.76it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:09<00:01, 21.96it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:09<00:01, 21.96it/s]

    Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:09<00:01, 21.96it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:09<00:01, 21.96it/s]Compiling num tokens (num_tokens=208):  62%|██████▏   | 36/58 [00:09<00:01, 21.96it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:09<00:00, 24.89it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:09<00:00, 24.89it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:09<00:00, 24.89it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:09<00:00, 24.89it/s]

    Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:09<00:00, 24.36it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:09<00:00, 24.36it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:09<00:00, 24.36it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:09<00:00, 24.36it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:09<00:00, 23.57it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:09<00:00, 23.57it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:09<00:00, 23.57it/s]

    Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:09<00:00, 23.57it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:09<00:00, 22.61it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:09<00:00, 22.61it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:09<00:00, 22.61it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:09<00:00, 22.61it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:09<00:00, 23.18it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:09<00:00, 23.18it/s]

    Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:09<00:00, 23.18it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:09<00:00, 23.18it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:09<00:00, 23.18it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:09<00:00, 26.97it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:09<00:00, 26.97it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:09<00:00, 26.97it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:09<00:00,  5.92it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=42.14 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=42.14 GB):   2%|▏         | 1/58 [00:00<00:25,  2.25it/s]Capturing num tokens (num_tokens=7680 avail_mem=41.07 GB):   2%|▏         | 1/58 [00:00<00:25,  2.25it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=41.07 GB):   3%|▎         | 2/58 [00:00<00:27,  2.00it/s]Capturing num tokens (num_tokens=7168 avail_mem=41.25 GB):   3%|▎         | 2/58 [00:00<00:27,  2.00it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=41.25 GB):   5%|▌         | 3/58 [00:01<00:27,  1.97it/s]Capturing num tokens (num_tokens=6656 avail_mem=42.08 GB):   5%|▌         | 3/58 [00:01<00:27,  1.97it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=42.08 GB):   7%|▋         | 4/58 [00:02<00:28,  1.93it/s]Capturing num tokens (num_tokens=6144 avail_mem=42.09 GB):   7%|▋         | 4/58 [00:02<00:28,  1.93it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=42.09 GB):   9%|▊         | 5/58 [00:02<00:27,  1.95it/s]Capturing num tokens (num_tokens=5632 avail_mem=41.38 GB):   9%|▊         | 5/58 [00:02<00:27,  1.95it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=41.38 GB):  10%|█         | 6/58 [00:02<00:24,  2.14it/s]Capturing num tokens (num_tokens=5120 avail_mem=41.44 GB):  10%|█         | 6/58 [00:02<00:24,  2.14it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=41.44 GB):  12%|█▏        | 7/58 [00:03<00:23,  2.16it/s]Capturing num tokens (num_tokens=4608 avail_mem=41.50 GB):  12%|█▏        | 7/58 [00:03<00:23,  2.16it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=41.50 GB):  14%|█▍        | 8/58 [00:03<00:20,  2.43it/s]Capturing num tokens (num_tokens=4096 avail_mem=41.57 GB):  14%|█▍        | 8/58 [00:03<00:20,  2.43it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=41.57 GB):  16%|█▌        | 9/58 [00:03<00:17,  2.80it/s]Capturing num tokens (num_tokens=3840 avail_mem=42.12 GB):  16%|█▌        | 9/58 [00:03<00:17,  2.80it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=42.12 GB):  17%|█▋        | 10/58 [00:04<00:16,  2.93it/s]Capturing num tokens (num_tokens=3584 avail_mem=42.12 GB):  17%|█▋        | 10/58 [00:04<00:16,  2.93it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=42.12 GB):  19%|█▉        | 11/58 [00:04<00:14,  3.29it/s]Capturing num tokens (num_tokens=3328 avail_mem=41.68 GB):  19%|█▉        | 11/58 [00:04<00:14,  3.29it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=41.68 GB):  21%|██        | 12/58 [00:04<00:12,  3.62it/s]Capturing num tokens (num_tokens=3072 avail_mem=41.71 GB):  21%|██        | 12/58 [00:04<00:12,  3.62it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=41.71 GB):  22%|██▏       | 13/58 [00:04<00:11,  3.93it/s]Capturing num tokens (num_tokens=2816 avail_mem=42.06 GB):  22%|██▏       | 13/58 [00:04<00:11,  3.93it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=42.06 GB):  24%|██▍       | 14/58 [00:05<00:10,  4.18it/s]Capturing num tokens (num_tokens=2560 avail_mem=42.13 GB):  24%|██▍       | 14/58 [00:05<00:10,  4.18it/s]Capturing num tokens (num_tokens=2560 avail_mem=42.13 GB):  26%|██▌       | 15/58 [00:05<00:09,  4.50it/s]Capturing num tokens (num_tokens=2304 avail_mem=41.77 GB):  26%|██▌       | 15/58 [00:05<00:09,  4.50it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=41.77 GB):  28%|██▊       | 16/58 [00:05<00:08,  5.13it/s]Capturing num tokens (num_tokens=2048 avail_mem=41.79 GB):  28%|██▊       | 16/58 [00:05<00:08,  5.13it/s]Capturing num tokens (num_tokens=2048 avail_mem=41.79 GB):  29%|██▉       | 17/58 [00:05<00:07,  5.67it/s]Capturing num tokens (num_tokens=1792 avail_mem=42.12 GB):  29%|██▉       | 17/58 [00:05<00:07,  5.67it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=42.12 GB):  31%|███       | 18/58 [00:05<00:06,  5.99it/s]Capturing num tokens (num_tokens=1536 avail_mem=42.12 GB):  31%|███       | 18/58 [00:05<00:06,  5.99it/s]Capturing num tokens (num_tokens=1536 avail_mem=42.12 GB):  33%|███▎      | 19/58 [00:05<00:06,  6.29it/s]Capturing num tokens (num_tokens=1280 avail_mem=41.85 GB):  33%|███▎      | 19/58 [00:05<00:06,  6.29it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=41.85 GB):  34%|███▍      | 20/58 [00:05<00:06,  6.28it/s]Capturing num tokens (num_tokens=1024 avail_mem=41.93 GB):  34%|███▍      | 20/58 [00:06<00:06,  6.28it/s]Capturing num tokens (num_tokens=1024 avail_mem=41.93 GB):  36%|███▌      | 21/58 [00:06<00:06,  5.86it/s]

    Capturing num tokens (num_tokens=960 avail_mem=42.12 GB):  36%|███▌      | 21/58 [00:06<00:06,  5.86it/s] Capturing num tokens (num_tokens=960 avail_mem=42.12 GB):  38%|███▊      | 22/58 [00:06<00:05,  6.63it/s]Capturing num tokens (num_tokens=896 avail_mem=42.11 GB):  38%|███▊      | 22/58 [00:06<00:05,  6.63it/s]Capturing num tokens (num_tokens=832 avail_mem=41.91 GB):  38%|███▊      | 22/58 [00:06<00:05,  6.63it/s]

    Capturing num tokens (num_tokens=832 avail_mem=41.91 GB):  41%|████▏     | 24/58 [00:06<00:03,  8.65it/s]Capturing num tokens (num_tokens=768 avail_mem=42.10 GB):  41%|████▏     | 24/58 [00:06<00:03,  8.65it/s]Capturing num tokens (num_tokens=704 avail_mem=42.10 GB):  41%|████▏     | 24/58 [00:06<00:03,  8.65it/s]Capturing num tokens (num_tokens=704 avail_mem=42.10 GB):  45%|████▍     | 26/58 [00:06<00:03, 10.29it/s]Capturing num tokens (num_tokens=640 avail_mem=41.96 GB):  45%|████▍     | 26/58 [00:06<00:03, 10.29it/s]Capturing num tokens (num_tokens=576 avail_mem=42.09 GB):  45%|████▍     | 26/58 [00:06<00:03, 10.29it/s]

    Capturing num tokens (num_tokens=576 avail_mem=42.09 GB):  48%|████▊     | 28/58 [00:06<00:02, 12.27it/s]Capturing num tokens (num_tokens=512 avail_mem=42.08 GB):  48%|████▊     | 28/58 [00:06<00:02, 12.27it/s]Capturing num tokens (num_tokens=480 avail_mem=42.08 GB):  48%|████▊     | 28/58 [00:06<00:02, 12.27it/s]Capturing num tokens (num_tokens=448 avail_mem=42.07 GB):  48%|████▊     | 28/58 [00:06<00:02, 12.27it/s]Capturing num tokens (num_tokens=448 avail_mem=42.07 GB):  53%|█████▎    | 31/58 [00:06<00:01, 14.52it/s]Capturing num tokens (num_tokens=416 avail_mem=42.07 GB):  53%|█████▎    | 31/58 [00:06<00:01, 14.52it/s]

    Capturing num tokens (num_tokens=384 avail_mem=42.06 GB):  53%|█████▎    | 31/58 [00:06<00:01, 14.52it/s]Capturing num tokens (num_tokens=384 avail_mem=42.06 GB):  57%|█████▋    | 33/58 [00:06<00:01, 15.51it/s]Capturing num tokens (num_tokens=352 avail_mem=42.05 GB):  57%|█████▋    | 33/58 [00:06<00:01, 15.51it/s]Capturing num tokens (num_tokens=320 avail_mem=41.99 GB):  57%|█████▋    | 33/58 [00:06<00:01, 15.51it/s]Capturing num tokens (num_tokens=288 avail_mem=42.04 GB):  57%|█████▋    | 33/58 [00:06<00:01, 15.51it/s]Capturing num tokens (num_tokens=288 avail_mem=42.04 GB):  62%|██████▏   | 36/58 [00:07<00:01, 17.83it/s]Capturing num tokens (num_tokens=256 avail_mem=42.03 GB):  62%|██████▏   | 36/58 [00:07<00:01, 17.83it/s]

    Capturing num tokens (num_tokens=240 avail_mem=42.02 GB):  62%|██████▏   | 36/58 [00:07<00:01, 17.83it/s]Capturing num tokens (num_tokens=224 avail_mem=42.02 GB):  62%|██████▏   | 36/58 [00:07<00:01, 17.83it/s]Capturing num tokens (num_tokens=224 avail_mem=42.02 GB):  67%|██████▋   | 39/58 [00:07<00:00, 19.76it/s]Capturing num tokens (num_tokens=208 avail_mem=41.96 GB):  67%|██████▋   | 39/58 [00:07<00:00, 19.76it/s]Capturing num tokens (num_tokens=192 avail_mem=42.00 GB):  67%|██████▋   | 39/58 [00:07<00:00, 19.76it/s]Capturing num tokens (num_tokens=176 avail_mem=42.00 GB):  67%|██████▋   | 39/58 [00:07<00:00, 19.76it/s]

    Capturing num tokens (num_tokens=176 avail_mem=42.00 GB):  72%|███████▏  | 42/58 [00:07<00:00, 20.96it/s]Capturing num tokens (num_tokens=160 avail_mem=41.99 GB):  72%|███████▏  | 42/58 [00:07<00:00, 20.96it/s]Capturing num tokens (num_tokens=144 avail_mem=41.98 GB):  72%|███████▏  | 42/58 [00:07<00:00, 20.96it/s]Capturing num tokens (num_tokens=128 avail_mem=41.99 GB):  72%|███████▏  | 42/58 [00:07<00:00, 20.96it/s]Capturing num tokens (num_tokens=128 avail_mem=41.99 GB):  78%|███████▊  | 45/58 [00:07<00:00, 20.53it/s]Capturing num tokens (num_tokens=112 avail_mem=41.98 GB):  78%|███████▊  | 45/58 [00:07<00:00, 20.53it/s]Capturing num tokens (num_tokens=96 avail_mem=41.97 GB):  78%|███████▊  | 45/58 [00:07<00:00, 20.53it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=41.96 GB):  78%|███████▊  | 45/58 [00:07<00:00, 20.53it/s]Capturing num tokens (num_tokens=80 avail_mem=41.96 GB):  83%|████████▎ | 48/58 [00:07<00:00, 20.63it/s]Capturing num tokens (num_tokens=64 avail_mem=41.95 GB):  83%|████████▎ | 48/58 [00:07<00:00, 20.63it/s]Capturing num tokens (num_tokens=48 avail_mem=41.95 GB):  83%|████████▎ | 48/58 [00:07<00:00, 20.63it/s]

    Capturing num tokens (num_tokens=32 avail_mem=41.94 GB):  83%|████████▎ | 48/58 [00:07<00:00, 20.63it/s]Capturing num tokens (num_tokens=32 avail_mem=41.94 GB):  88%|████████▊ | 51/58 [00:07<00:00, 17.87it/s]Capturing num tokens (num_tokens=28 avail_mem=41.93 GB):  88%|████████▊ | 51/58 [00:07<00:00, 17.87it/s]Capturing num tokens (num_tokens=24 avail_mem=41.93 GB):  88%|████████▊ | 51/58 [00:07<00:00, 17.87it/s]Capturing num tokens (num_tokens=20 avail_mem=41.92 GB):  88%|████████▊ | 51/58 [00:07<00:00, 17.87it/s]Capturing num tokens (num_tokens=20 avail_mem=41.92 GB):  93%|█████████▎| 54/58 [00:07<00:00, 20.16it/s]

    Capturing num tokens (num_tokens=16 avail_mem=41.91 GB):  93%|█████████▎| 54/58 [00:08<00:00, 20.16it/s]Capturing num tokens (num_tokens=12 avail_mem=41.90 GB):  93%|█████████▎| 54/58 [00:08<00:00, 20.16it/s]Capturing num tokens (num_tokens=8 avail_mem=41.90 GB):  93%|█████████▎| 54/58 [00:08<00:00, 20.16it/s] Capturing num tokens (num_tokens=8 avail_mem=41.90 GB):  98%|█████████▊| 57/58 [00:08<00:00, 17.50it/s]Capturing num tokens (num_tokens=4 avail_mem=41.91 GB):  98%|█████████▊| 57/58 [00:08<00:00, 17.50it/s]Capturing num tokens (num_tokens=4 avail_mem=41.91 GB): 100%|██████████| 58/58 [00:08<00:00,  7.10it/s]


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



<strong style='color: #00008B;'>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>1. **Start with the number 1.**<br>2. **Add 3 to it.**<br>   <br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Final Answer:** \(\boxed{4}\)</strong>


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



<strong style='color: #00008B;'>First, I need to identify the two numbers in the addition problem: 1 and 3.<br><br>Next, I'll add these two numbers together.<br><br>Finally, I'll calculate the sum to find the result.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'><br><br>Sure! Let's solve the addition problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:**<br>\[<br>\boxed{4}<br>\]</strong>


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



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of the numbers 1 and 3.<br><br>Next, I perform the addition operation by combining these two numbers.<br><br>Finally, I calculate that 1 plus 3 equals 4.<br></think><br><br>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>1. **Identify the numbers to add:**  <br>   - First number: 1<br>   - Second number: 3<br><br>2. **Perform the addition:**  <br>   \[<br>   1 + 3 = 4<br>   \]<br><br>3. **Final Answer:**  <br>   \[<br>   \boxed{4}<br>   \]</strong>


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



<strong style='color: #00008B;'>First, I identify the two numbers in the problem, which are 1 and 3.<br><br>Next, I add these two numbers together to find the sum.<br><br>Finally, I calculate that 1 plus 3 equals 4.<br></think><br><br>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the answer is \(\boxed{4}\).</strong>


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:328: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      return await dependant.call(**values)



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>First, I identify the two numbers in the problem, which are 1 and 3.<br><br>Next, I add these two numbers together to find the sum.<br><br>Finally, I calculate that 1 plus 3 equals 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the answer is \(\boxed{4}\).</strong>



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


    2026-03-31 21:08:34.770 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:08:34] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:08:34.770 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:08:34] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:08:34.770 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:08:34] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:08:34.770 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:08:34] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:08:34.770 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:08:34] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:01<00:01,  1.13s/it]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.46s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.41s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:55,  3.09s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:55,  3.09s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:24,  1.52s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:24,  1.52s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:50,  1.09it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:50,  1.09it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:33,  1.60it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:33,  1.60it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:24,  2.19it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:24,  2.19it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.83it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.83it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.55it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.55it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.32it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.32it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.19it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.19it/s]

    Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.19it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:06,  6.83it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:06,  6.83it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:06,  6.83it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:04<00:05,  8.31it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:04<00:05,  8.31it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:04<00:05,  8.31it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:05<00:04,  9.83it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:05<00:04,  9.83it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:05<00:04,  9.83it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:03, 11.69it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:03, 11.69it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:03, 11.69it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:05<00:03, 11.69it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 14.87it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 14.87it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 14.87it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 14.87it/s]

    Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:05<00:02, 14.87it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:05<00:01, 20.02it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:05<00:01, 20.02it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:05<00:01, 20.02it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:05<00:01, 20.02it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:05<00:01, 20.02it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:05<00:01, 20.02it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:05<00:01, 26.63it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:05<00:01, 26.63it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:05<00:01, 26.63it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:05<00:01, 26.63it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:05<00:01, 26.63it/s]

    Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:05<00:01, 26.63it/s]Compiling num tokens (num_tokens=320):  50%|█████     | 29/58 [00:05<00:01, 26.63it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:05<00:00, 33.47it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:05<00:00, 33.47it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:05<00:00, 33.47it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:05<00:00, 33.47it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:05<00:00, 33.47it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:05<00:00, 33.47it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:05<00:00, 33.47it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:05<00:00, 39.24it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:05<00:00, 39.24it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:05<00:00, 39.24it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:05<00:00, 39.24it/s]

    Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:05<00:00, 39.24it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:05<00:00, 39.24it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:05<00:00, 39.24it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:05<00:00, 42.25it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:05<00:00, 42.25it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:05<00:00, 42.25it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:05<00:00, 42.25it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:05<00:00, 42.25it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:05<00:00, 42.25it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:05<00:00, 44.12it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:05<00:00, 44.12it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:05<00:00, 44.12it/s]

    Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:05<00:00, 44.12it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:06<00:00, 44.12it/s]Compiling num tokens (num_tokens=8):  90%|████████▉ | 52/58 [00:06<00:00, 44.12it/s] Compiling num tokens (num_tokens=4):  90%|████████▉ | 52/58 [00:06<00:00, 44.12it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:06<00:00,  9.60it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=42.14 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=42.14 GB):   2%|▏         | 1/58 [00:00<00:44,  1.28it/s]Capturing num tokens (num_tokens=7680 avail_mem=42.11 GB):   2%|▏         | 1/58 [00:00<00:44,  1.28it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=42.11 GB):   3%|▎         | 2/58 [00:01<00:37,  1.51it/s]Capturing num tokens (num_tokens=7168 avail_mem=42.11 GB):   3%|▎         | 2/58 [00:01<00:37,  1.51it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=42.11 GB):   5%|▌         | 3/58 [00:01<00:33,  1.63it/s]Capturing num tokens (num_tokens=6656 avail_mem=42.11 GB):   5%|▌         | 3/58 [00:01<00:33,  1.63it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=42.11 GB):   7%|▋         | 4/58 [00:02<00:31,  1.73it/s]Capturing num tokens (num_tokens=6144 avail_mem=42.10 GB):   7%|▋         | 4/58 [00:02<00:31,  1.73it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=42.10 GB):   9%|▊         | 5/58 [00:02<00:29,  1.82it/s]Capturing num tokens (num_tokens=5632 avail_mem=41.51 GB):   9%|▊         | 5/58 [00:02<00:29,  1.82it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=41.51 GB):  10%|█         | 6/58 [00:03<00:26,  1.99it/s]Capturing num tokens (num_tokens=5120 avail_mem=41.45 GB):  10%|█         | 6/58 [00:03<00:26,  1.99it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=41.45 GB):  12%|█▏        | 7/58 [00:03<00:24,  2.11it/s]Capturing num tokens (num_tokens=4608 avail_mem=41.46 GB):  12%|█▏        | 7/58 [00:03<00:24,  2.11it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=41.46 GB):  14%|█▍        | 8/58 [00:04<00:21,  2.34it/s]Capturing num tokens (num_tokens=4096 avail_mem=41.46 GB):  14%|█▍        | 8/58 [00:04<00:21,  2.34it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=41.46 GB):  16%|█▌        | 9/58 [00:04<00:19,  2.57it/s]Capturing num tokens (num_tokens=3840 avail_mem=41.46 GB):  16%|█▌        | 9/58 [00:04<00:19,  2.57it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=41.46 GB):  17%|█▋        | 10/58 [00:04<00:17,  2.80it/s]Capturing num tokens (num_tokens=3584 avail_mem=41.46 GB):  17%|█▋        | 10/58 [00:04<00:17,  2.80it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=41.46 GB):  19%|█▉        | 11/58 [00:04<00:15,  3.08it/s]Capturing num tokens (num_tokens=3328 avail_mem=41.46 GB):  19%|█▉        | 11/58 [00:04<00:15,  3.08it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=41.46 GB):  21%|██        | 12/58 [00:05<00:13,  3.43it/s]Capturing num tokens (num_tokens=3072 avail_mem=26.44 GB):  21%|██        | 12/58 [00:05<00:13,  3.43it/s]Capturing num tokens (num_tokens=3072 avail_mem=26.44 GB):  22%|██▏       | 13/58 [00:05<00:11,  3.87it/s]Capturing num tokens (num_tokens=2816 avail_mem=26.44 GB):  22%|██▏       | 13/58 [00:05<00:11,  3.87it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=26.44 GB):  24%|██▍       | 14/58 [00:05<00:10,  4.34it/s]Capturing num tokens (num_tokens=2560 avail_mem=26.44 GB):  24%|██▍       | 14/58 [00:05<00:10,  4.34it/s]Capturing num tokens (num_tokens=2560 avail_mem=26.44 GB):  26%|██▌       | 15/58 [00:05<00:09,  4.66it/s]Capturing num tokens (num_tokens=2304 avail_mem=26.44 GB):  26%|██▌       | 15/58 [00:05<00:09,  4.66it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=26.44 GB):  28%|██▊       | 16/58 [00:05<00:08,  5.17it/s]Capturing num tokens (num_tokens=2048 avail_mem=26.44 GB):  28%|██▊       | 16/58 [00:05<00:08,  5.17it/s]Capturing num tokens (num_tokens=1792 avail_mem=26.44 GB):  28%|██▊       | 16/58 [00:05<00:08,  5.17it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=26.44 GB):  31%|███       | 18/58 [00:06<00:06,  6.65it/s]Capturing num tokens (num_tokens=1536 avail_mem=26.43 GB):  31%|███       | 18/58 [00:06<00:06,  6.65it/s]Capturing num tokens (num_tokens=1280 avail_mem=26.43 GB):  31%|███       | 18/58 [00:06<00:06,  6.65it/s]Capturing num tokens (num_tokens=1280 avail_mem=26.43 GB):  34%|███▍      | 20/58 [00:06<00:04,  8.66it/s]Capturing num tokens (num_tokens=1024 avail_mem=26.43 GB):  34%|███▍      | 20/58 [00:06<00:04,  8.66it/s]Capturing num tokens (num_tokens=960 avail_mem=26.43 GB):  34%|███▍      | 20/58 [00:06<00:04,  8.66it/s] 

    Capturing num tokens (num_tokens=896 avail_mem=26.42 GB):  34%|███▍      | 20/58 [00:06<00:04,  8.66it/s]Capturing num tokens (num_tokens=896 avail_mem=26.42 GB):  40%|███▉      | 23/58 [00:06<00:02, 12.44it/s]Capturing num tokens (num_tokens=832 avail_mem=26.42 GB):  40%|███▉      | 23/58 [00:06<00:02, 12.44it/s]Capturing num tokens (num_tokens=768 avail_mem=26.42 GB):  40%|███▉      | 23/58 [00:06<00:02, 12.44it/s]Capturing num tokens (num_tokens=704 avail_mem=26.41 GB):  40%|███▉      | 23/58 [00:06<00:02, 12.44it/s]Capturing num tokens (num_tokens=704 avail_mem=26.41 GB):  45%|████▍     | 26/58 [00:06<00:02, 15.95it/s]Capturing num tokens (num_tokens=640 avail_mem=26.41 GB):  45%|████▍     | 26/58 [00:06<00:02, 15.95it/s]Capturing num tokens (num_tokens=576 avail_mem=26.41 GB):  45%|████▍     | 26/58 [00:06<00:02, 15.95it/s]

    Capturing num tokens (num_tokens=512 avail_mem=26.40 GB):  45%|████▍     | 26/58 [00:06<00:02, 15.95it/s]Capturing num tokens (num_tokens=480 avail_mem=26.40 GB):  45%|████▍     | 26/58 [00:06<00:02, 15.95it/s]Capturing num tokens (num_tokens=480 avail_mem=26.40 GB):  52%|█████▏    | 30/58 [00:06<00:01, 20.31it/s]Capturing num tokens (num_tokens=448 avail_mem=26.40 GB):  52%|█████▏    | 30/58 [00:06<00:01, 20.31it/s]Capturing num tokens (num_tokens=416 avail_mem=25.36 GB):  52%|█████▏    | 30/58 [00:06<00:01, 20.31it/s]

    Capturing num tokens (num_tokens=384 avail_mem=25.36 GB):  52%|█████▏    | 30/58 [00:06<00:01, 20.31it/s]Capturing num tokens (num_tokens=384 avail_mem=25.36 GB):  57%|█████▋    | 33/58 [00:06<00:01, 14.97it/s]Capturing num tokens (num_tokens=352 avail_mem=25.35 GB):  57%|█████▋    | 33/58 [00:06<00:01, 14.97it/s]

    Capturing num tokens (num_tokens=320 avail_mem=26.35 GB):  57%|█████▋    | 33/58 [00:06<00:01, 14.97it/s]Capturing num tokens (num_tokens=320 avail_mem=26.35 GB):  60%|██████    | 35/58 [00:06<00:01, 13.98it/s]Capturing num tokens (num_tokens=288 avail_mem=26.35 GB):  60%|██████    | 35/58 [00:06<00:01, 13.98it/s]Capturing num tokens (num_tokens=256 avail_mem=25.52 GB):  60%|██████    | 35/58 [00:07<00:01, 13.98it/s]

    Capturing num tokens (num_tokens=256 avail_mem=25.52 GB):  64%|██████▍   | 37/58 [00:07<00:01, 11.79it/s]Capturing num tokens (num_tokens=240 avail_mem=25.52 GB):  64%|██████▍   | 37/58 [00:07<00:01, 11.79it/s]Capturing num tokens (num_tokens=224 avail_mem=26.34 GB):  64%|██████▍   | 37/58 [00:07<00:01, 11.79it/s]Capturing num tokens (num_tokens=224 avail_mem=26.34 GB):  67%|██████▋   | 39/58 [00:07<00:01, 11.76it/s]Capturing num tokens (num_tokens=208 avail_mem=26.33 GB):  67%|██████▋   | 39/58 [00:07<00:01, 11.76it/s]

    Capturing num tokens (num_tokens=192 avail_mem=25.57 GB):  67%|██████▋   | 39/58 [00:07<00:01, 11.76it/s]Capturing num tokens (num_tokens=192 avail_mem=25.57 GB):  71%|███████   | 41/58 [00:07<00:01,  9.01it/s]Capturing num tokens (num_tokens=176 avail_mem=25.62 GB):  71%|███████   | 41/58 [00:07<00:01,  9.01it/s]

    Capturing num tokens (num_tokens=160 avail_mem=26.26 GB):  71%|███████   | 41/58 [00:07<00:01,  9.01it/s]Capturing num tokens (num_tokens=160 avail_mem=26.26 GB):  74%|███████▍  | 43/58 [00:08<00:01,  8.84it/s]Capturing num tokens (num_tokens=144 avail_mem=22.57 GB):  74%|███████▍  | 43/58 [00:08<00:01,  8.84it/s]

    Capturing num tokens (num_tokens=128 avail_mem=22.58 GB):  74%|███████▍  | 43/58 [00:08<00:01,  8.84it/s]Capturing num tokens (num_tokens=128 avail_mem=22.58 GB):  78%|███████▊  | 45/58 [00:08<00:01,  8.49it/s]Capturing num tokens (num_tokens=112 avail_mem=20.51 GB):  78%|███████▊  | 45/58 [00:08<00:01,  8.49it/s]

    Capturing num tokens (num_tokens=112 avail_mem=20.51 GB):  79%|███████▉  | 46/58 [00:08<00:01,  8.37it/s]Capturing num tokens (num_tokens=96 avail_mem=19.85 GB):  79%|███████▉  | 46/58 [00:08<00:01,  8.37it/s] Capturing num tokens (num_tokens=96 avail_mem=19.85 GB):  81%|████████  | 47/58 [00:08<00:01,  8.46it/s]Capturing num tokens (num_tokens=80 avail_mem=19.84 GB):  81%|████████  | 47/58 [00:08<00:01,  8.46it/s]

    Capturing num tokens (num_tokens=64 avail_mem=20.50 GB):  81%|████████  | 47/58 [00:08<00:01,  8.46it/s]Capturing num tokens (num_tokens=64 avail_mem=20.50 GB):  84%|████████▍ | 49/58 [00:08<00:00,  9.02it/s]Capturing num tokens (num_tokens=48 avail_mem=20.50 GB):  84%|████████▍ | 49/58 [00:08<00:00,  9.02it/s]Capturing num tokens (num_tokens=32 avail_mem=19.89 GB):  84%|████████▍ | 49/58 [00:08<00:00,  9.02it/s]

    Capturing num tokens (num_tokens=32 avail_mem=19.89 GB):  88%|████████▊ | 51/58 [00:08<00:00,  9.49it/s]Capturing num tokens (num_tokens=28 avail_mem=19.89 GB):  88%|████████▊ | 51/58 [00:08<00:00,  9.49it/s]Capturing num tokens (num_tokens=24 avail_mem=20.49 GB):  88%|████████▊ | 51/58 [00:08<00:00,  9.49it/s]Capturing num tokens (num_tokens=24 avail_mem=20.49 GB):  91%|█████████▏| 53/58 [00:09<00:00, 10.46it/s]Capturing num tokens (num_tokens=20 avail_mem=19.94 GB):  91%|█████████▏| 53/58 [00:09<00:00, 10.46it/s]

    Capturing num tokens (num_tokens=16 avail_mem=19.94 GB):  91%|█████████▏| 53/58 [00:09<00:00, 10.46it/s]Capturing num tokens (num_tokens=16 avail_mem=19.94 GB):  95%|█████████▍| 55/58 [00:09<00:00, 10.78it/s]Capturing num tokens (num_tokens=12 avail_mem=20.48 GB):  95%|█████████▍| 55/58 [00:09<00:00, 10.78it/s]Capturing num tokens (num_tokens=8 avail_mem=19.99 GB):  95%|█████████▍| 55/58 [00:09<00:00, 10.78it/s] 

    Capturing num tokens (num_tokens=8 avail_mem=19.99 GB):  98%|█████████▊| 57/58 [00:09<00:00, 11.08it/s]Capturing num tokens (num_tokens=4 avail_mem=20.56 GB):  98%|█████████▊| 57/58 [00:09<00:00, 11.08it/s]Capturing num tokens (num_tokens=4 avail_mem=20.56 GB): 100%|██████████| 58/58 [00:09<00:00,  6.14it/s]



<strong style='color: #00008B;'>==== Original Output ====</strong>



<strong style='color: #00008B;'>I need to add the numbers 1 and 3. <br><br>First, I'll identify the two numbers involved in the addition.<br><br>Next, I'll perform the addition by summing these two numbers.<br><br>Finally, I'll state the result as the final answer.<br></think><br><br>Sure! Let's solve the addition step by step.<br><br>**Problem:**  <br>What is \(1 + 3\)?<br><br>**Solution:**  <br>1. **Identify the numbers to add:**  <br>   \(1\) and \(3\).<br><br>2. **Add the numbers:**  <br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:**  <br>\[<br>\boxed{4}<br>\]</strong>



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>I need to add the numbers 1 and 3. <br><br>First, I'll identify the two numbers involved in the addition.<br><br>Next, I'll perform the addition by summing these two numbers.<br><br>Finally, I'll state the result as the final answer.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>Sure! Let's solve the addition step by step.<br><br>**Problem:**  <br>What is \(1 + 3\)?<br><br>**Solution:**  <br>1. **Identify the numbers to add:**  <br>   \(1\) and \(3\).<br><br>2. **Add the numbers:**  <br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:**  <br>\[<br>\boxed{4}<br>\]</strong>



```python
llm.shutdown()
```

## Supporting New Reasoning Model Schemas

For future reasoning models, you can implement the reasoning parser as a subclass of `BaseReasoningFormatDetector` in `python/sglang/srt/reasoning_parser.py` and specify the reasoning parser for new reasoning model schemas accordingly.
