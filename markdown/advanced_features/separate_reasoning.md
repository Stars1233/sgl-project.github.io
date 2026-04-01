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


    2026-04-01 19:56:35.954 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 19:56:35] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 19:56:35.955 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 19:56:35] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 19:56:35.955 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 19:56:35] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 19:56:35.955 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 19:56:35] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 19:56:35.955 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 19:56:35] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:00<00:00,  1.05it/s]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.24s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.20s/it]


    2026-04-01 19:56:41,105 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 19:56:41] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:54,  3.06s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:54,  3.06s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:24,  1.50s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:24,  1.50s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:49,  1.10it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:49,  1.10it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:33,  1.62it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:33,  1.62it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:23,  2.21it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:23,  2.21it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.85it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.85it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.58it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.58it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.36it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.36it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.25it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.25it/s]

    Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.25it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:06,  6.91it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:06,  6.91it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:06,  6.91it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:04<00:05,  8.44it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:04<00:05,  8.44it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:04<00:05,  8.44it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:05<00:04,  9.93it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:05<00:04,  9.93it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:05<00:04,  9.93it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:03, 11.86it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:03, 11.86it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:03, 11.86it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:05<00:03, 11.86it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 15.07it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 15.07it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 15.07it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 15.07it/s]

    Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:05<00:02, 15.07it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:05<00:01, 20.31it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:05<00:01, 20.31it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:05<00:01, 20.31it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:05<00:01, 20.31it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:05<00:01, 20.31it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:05<00:01, 20.31it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:05<00:01, 26.93it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:05<00:01, 26.93it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:05<00:01, 26.93it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:05<00:01, 26.93it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:05<00:01, 26.93it/s]

    Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:05<00:01, 26.93it/s]Compiling num tokens (num_tokens=320):  50%|█████     | 29/58 [00:05<00:01, 26.93it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:05<00:00, 33.86it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:05<00:00, 33.86it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:05<00:00, 33.86it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:05<00:00, 33.86it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:05<00:00, 33.86it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:05<00:00, 33.86it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:05<00:00, 33.86it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:05<00:00, 36.83it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:05<00:00, 36.83it/s]

    Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:05<00:00, 36.83it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:05<00:00, 36.83it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:05<00:00, 36.83it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:05<00:00, 32.36it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:05<00:00, 32.36it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:05<00:00, 32.36it/s] 

    Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:05<00:00, 32.36it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:06<00:00, 32.36it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:06<00:00, 29.77it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:06<00:00, 29.77it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:06<00:00, 29.77it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:06<00:00, 29.77it/s]

    Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:06<00:00, 29.77it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:06<00:00, 28.20it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:06<00:00, 28.20it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:06<00:00, 28.20it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:06<00:00, 28.20it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:06<00:00, 28.20it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:06<00:00, 29.65it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:06<00:00, 29.65it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:06<00:00,  9.14it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=23.09 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=23.09 GB):   2%|▏         | 1/58 [00:00<00:42,  1.34it/s]Capturing num tokens (num_tokens=7680 avail_mem=23.06 GB):   2%|▏         | 1/58 [00:00<00:42,  1.34it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=23.06 GB):   3%|▎         | 2/58 [00:01<00:36,  1.55it/s]Capturing num tokens (num_tokens=7168 avail_mem=23.06 GB):   3%|▎         | 2/58 [00:01<00:36,  1.55it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=23.06 GB):   5%|▌         | 3/58 [00:01<00:32,  1.69it/s]Capturing num tokens (num_tokens=6656 avail_mem=23.06 GB):   5%|▌         | 3/58 [00:01<00:32,  1.69it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=23.06 GB):   7%|▋         | 4/58 [00:02<00:29,  1.83it/s]Capturing num tokens (num_tokens=6144 avail_mem=23.07 GB):   7%|▋         | 4/58 [00:02<00:29,  1.83it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=23.07 GB):   9%|▊         | 5/58 [00:02<00:27,  1.96it/s]Capturing num tokens (num_tokens=5632 avail_mem=23.07 GB):   9%|▊         | 5/58 [00:02<00:27,  1.96it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=23.07 GB):  10%|█         | 6/58 [00:03<00:25,  2.07it/s]Capturing num tokens (num_tokens=5120 avail_mem=23.07 GB):  10%|█         | 6/58 [00:03<00:25,  2.07it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=23.07 GB):  12%|█▏        | 7/58 [00:03<00:22,  2.32it/s]Capturing num tokens (num_tokens=4608 avail_mem=23.08 GB):  12%|█▏        | 7/58 [00:03<00:22,  2.32it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=23.08 GB):  14%|█▍        | 8/58 [00:03<00:20,  2.39it/s]Capturing num tokens (num_tokens=4096 avail_mem=23.06 GB):  14%|█▍        | 8/58 [00:03<00:20,  2.39it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=23.06 GB):  16%|█▌        | 9/58 [00:04<00:17,  2.83it/s]Capturing num tokens (num_tokens=3840 avail_mem=23.07 GB):  16%|█▌        | 9/58 [00:04<00:17,  2.83it/s]Capturing num tokens (num_tokens=3840 avail_mem=23.07 GB):  17%|█▋        | 10/58 [00:04<00:14,  3.27it/s]Capturing num tokens (num_tokens=3584 avail_mem=23.06 GB):  17%|█▋        | 10/58 [00:04<00:14,  3.27it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=23.06 GB):  19%|█▉        | 11/58 [00:04<00:14,  3.29it/s]Capturing num tokens (num_tokens=3328 avail_mem=23.06 GB):  19%|█▉        | 11/58 [00:04<00:14,  3.29it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=23.06 GB):  21%|██        | 12/58 [00:04<00:14,  3.22it/s]Capturing num tokens (num_tokens=3072 avail_mem=23.05 GB):  21%|██        | 12/58 [00:04<00:14,  3.22it/s]Capturing num tokens (num_tokens=3072 avail_mem=23.05 GB):  22%|██▏       | 13/58 [00:05<00:11,  3.80it/s]Capturing num tokens (num_tokens=2816 avail_mem=23.06 GB):  22%|██▏       | 13/58 [00:05<00:11,  3.80it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=23.06 GB):  24%|██▍       | 14/58 [00:05<00:09,  4.63it/s]Capturing num tokens (num_tokens=2560 avail_mem=23.06 GB):  24%|██▍       | 14/58 [00:05<00:09,  4.63it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=23.06 GB):  26%|██▌       | 15/58 [00:05<00:09,  4.62it/s]Capturing num tokens (num_tokens=2304 avail_mem=41.09 GB):  26%|██▌       | 15/58 [00:05<00:09,  4.62it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=41.09 GB):  28%|██▊       | 16/58 [00:05<00:08,  4.71it/s]Capturing num tokens (num_tokens=2048 avail_mem=41.09 GB):  28%|██▊       | 16/58 [00:05<00:08,  4.71it/s]Capturing num tokens (num_tokens=2048 avail_mem=41.09 GB):  29%|██▉       | 17/58 [00:05<00:07,  5.16it/s]Capturing num tokens (num_tokens=1792 avail_mem=42.10 GB):  29%|██▉       | 17/58 [00:05<00:07,  5.16it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=42.10 GB):  31%|███       | 18/58 [00:05<00:07,  5.67it/s]Capturing num tokens (num_tokens=1536 avail_mem=42.09 GB):  31%|███       | 18/58 [00:05<00:07,  5.67it/s]Capturing num tokens (num_tokens=1536 avail_mem=42.09 GB):  33%|███▎      | 19/58 [00:06<00:06,  5.88it/s]Capturing num tokens (num_tokens=1280 avail_mem=41.27 GB):  33%|███▎      | 19/58 [00:06<00:06,  5.88it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=41.27 GB):  34%|███▍      | 20/58 [00:06<00:06,  6.08it/s]Capturing num tokens (num_tokens=1024 avail_mem=41.27 GB):  34%|███▍      | 20/58 [00:06<00:06,  6.08it/s]Capturing num tokens (num_tokens=1024 avail_mem=41.27 GB):  36%|███▌      | 21/58 [00:06<00:05,  6.89it/s]Capturing num tokens (num_tokens=960 avail_mem=42.09 GB):  36%|███▌      | 21/58 [00:06<00:05,  6.89it/s] Capturing num tokens (num_tokens=896 avail_mem=41.32 GB):  36%|███▌      | 21/58 [00:06<00:05,  6.89it/s]

    Capturing num tokens (num_tokens=896 avail_mem=41.32 GB):  40%|███▉      | 23/58 [00:06<00:04,  7.60it/s]Capturing num tokens (num_tokens=832 avail_mem=41.32 GB):  40%|███▉      | 23/58 [00:06<00:04,  7.60it/s]Capturing num tokens (num_tokens=832 avail_mem=41.32 GB):  41%|████▏     | 24/58 [00:06<00:04,  7.70it/s]Capturing num tokens (num_tokens=768 avail_mem=42.20 GB):  41%|████▏     | 24/58 [00:06<00:04,  7.70it/s]

    Capturing num tokens (num_tokens=768 avail_mem=42.20 GB):  43%|████▎     | 25/58 [00:06<00:04,  8.17it/s]Capturing num tokens (num_tokens=704 avail_mem=42.08 GB):  43%|████▎     | 25/58 [00:06<00:04,  8.17it/s]Capturing num tokens (num_tokens=704 avail_mem=42.08 GB):  45%|████▍     | 26/58 [00:06<00:03,  8.38it/s]Capturing num tokens (num_tokens=640 avail_mem=41.37 GB):  45%|████▍     | 26/58 [00:06<00:03,  8.38it/s]

    Capturing num tokens (num_tokens=640 avail_mem=41.37 GB):  47%|████▋     | 27/58 [00:07<00:03,  8.03it/s]Capturing num tokens (num_tokens=576 avail_mem=41.36 GB):  47%|████▋     | 27/58 [00:07<00:03,  8.03it/s]Capturing num tokens (num_tokens=576 avail_mem=41.36 GB):  48%|████▊     | 28/58 [00:07<00:04,  7.42it/s]Capturing num tokens (num_tokens=512 avail_mem=42.07 GB):  48%|████▊     | 28/58 [00:07<00:04,  7.42it/s]

    Capturing num tokens (num_tokens=512 avail_mem=42.07 GB):  50%|█████     | 29/58 [00:07<00:04,  6.85it/s]Capturing num tokens (num_tokens=480 avail_mem=41.41 GB):  50%|█████     | 29/58 [00:07<00:04,  6.85it/s]Capturing num tokens (num_tokens=448 avail_mem=41.41 GB):  50%|█████     | 29/58 [00:07<00:04,  6.85it/s]Capturing num tokens (num_tokens=448 avail_mem=41.41 GB):  53%|█████▎    | 31/58 [00:07<00:03,  8.28it/s]Capturing num tokens (num_tokens=416 avail_mem=42.07 GB):  53%|█████▎    | 31/58 [00:07<00:03,  8.28it/s]

    Capturing num tokens (num_tokens=416 avail_mem=42.07 GB):  55%|█████▌    | 32/58 [00:07<00:03,  8.64it/s]Capturing num tokens (num_tokens=384 avail_mem=41.46 GB):  55%|█████▌    | 32/58 [00:07<00:03,  8.64it/s]Capturing num tokens (num_tokens=384 avail_mem=41.46 GB):  57%|█████▋    | 33/58 [00:07<00:02,  8.94it/s]Capturing num tokens (num_tokens=352 avail_mem=42.07 GB):  57%|█████▋    | 33/58 [00:07<00:02,  8.94it/s]Capturing num tokens (num_tokens=320 avail_mem=42.06 GB):  57%|█████▋    | 33/58 [00:07<00:02,  8.94it/s]

    Capturing num tokens (num_tokens=320 avail_mem=42.06 GB):  60%|██████    | 35/58 [00:07<00:02,  9.90it/s]Capturing num tokens (num_tokens=288 avail_mem=41.51 GB):  60%|██████    | 35/58 [00:07<00:02,  9.90it/s]Capturing num tokens (num_tokens=256 avail_mem=42.06 GB):  60%|██████    | 35/58 [00:08<00:02,  9.90it/s]Capturing num tokens (num_tokens=256 avail_mem=42.06 GB):  64%|██████▍   | 37/58 [00:08<00:01, 10.79it/s]Capturing num tokens (num_tokens=240 avail_mem=41.56 GB):  64%|██████▍   | 37/58 [00:08<00:01, 10.79it/s]

    Capturing num tokens (num_tokens=224 avail_mem=41.56 GB):  64%|██████▍   | 37/58 [00:08<00:01, 10.79it/s]Capturing num tokens (num_tokens=224 avail_mem=41.56 GB):  67%|██████▋   | 39/58 [00:08<00:01, 11.02it/s]Capturing num tokens (num_tokens=208 avail_mem=42.05 GB):  67%|██████▋   | 39/58 [00:08<00:01, 11.02it/s]Capturing num tokens (num_tokens=192 avail_mem=41.61 GB):  67%|██████▋   | 39/58 [00:08<00:01, 11.02it/s]

    Capturing num tokens (num_tokens=192 avail_mem=41.61 GB):  71%|███████   | 41/58 [00:08<00:01, 11.35it/s]Capturing num tokens (num_tokens=176 avail_mem=42.05 GB):  71%|███████   | 41/58 [00:08<00:01, 11.35it/s]Capturing num tokens (num_tokens=160 avail_mem=41.97 GB):  71%|███████   | 41/58 [00:08<00:01, 11.35it/s]Capturing num tokens (num_tokens=160 avail_mem=41.97 GB):  74%|███████▍  | 43/58 [00:08<00:01, 11.77it/s]Capturing num tokens (num_tokens=144 avail_mem=41.62 GB):  74%|███████▍  | 43/58 [00:08<00:01, 11.77it/s]

    Capturing num tokens (num_tokens=128 avail_mem=42.00 GB):  74%|███████▍  | 43/58 [00:08<00:01, 11.77it/s]Capturing num tokens (num_tokens=128 avail_mem=42.00 GB):  78%|███████▊  | 45/58 [00:08<00:01, 11.70it/s]Capturing num tokens (num_tokens=112 avail_mem=41.66 GB):  78%|███████▊  | 45/58 [00:08<00:01, 11.70it/s]Capturing num tokens (num_tokens=96 avail_mem=42.04 GB):  78%|███████▊  | 45/58 [00:08<00:01, 11.70it/s] 

    Capturing num tokens (num_tokens=96 avail_mem=42.04 GB):  81%|████████  | 47/58 [00:08<00:00, 12.20it/s]Capturing num tokens (num_tokens=80 avail_mem=41.67 GB):  81%|████████  | 47/58 [00:08<00:00, 12.20it/s]Capturing num tokens (num_tokens=64 avail_mem=42.03 GB):  81%|████████  | 47/58 [00:08<00:00, 12.20it/s]Capturing num tokens (num_tokens=64 avail_mem=42.03 GB):  84%|████████▍ | 49/58 [00:09<00:00, 12.75it/s]Capturing num tokens (num_tokens=48 avail_mem=41.70 GB):  84%|████████▍ | 49/58 [00:09<00:00, 12.75it/s]

    Capturing num tokens (num_tokens=32 avail_mem=42.03 GB):  84%|████████▍ | 49/58 [00:09<00:00, 12.75it/s]Capturing num tokens (num_tokens=32 avail_mem=42.03 GB):  88%|████████▊ | 51/58 [00:09<00:00, 13.32it/s]Capturing num tokens (num_tokens=28 avail_mem=41.72 GB):  88%|████████▊ | 51/58 [00:09<00:00, 13.32it/s]Capturing num tokens (num_tokens=24 avail_mem=42.02 GB):  88%|████████▊ | 51/58 [00:09<00:00, 13.32it/s]

    Capturing num tokens (num_tokens=24 avail_mem=42.02 GB):  91%|█████████▏| 53/58 [00:09<00:00, 13.52it/s]Capturing num tokens (num_tokens=20 avail_mem=41.74 GB):  91%|█████████▏| 53/58 [00:09<00:00, 13.52it/s]Capturing num tokens (num_tokens=16 avail_mem=42.01 GB):  91%|█████████▏| 53/58 [00:09<00:00, 13.52it/s]Capturing num tokens (num_tokens=16 avail_mem=42.01 GB):  95%|█████████▍| 55/58 [00:09<00:00, 13.98it/s]Capturing num tokens (num_tokens=12 avail_mem=42.03 GB):  95%|█████████▍| 55/58 [00:09<00:00, 13.98it/s]

    Capturing num tokens (num_tokens=8 avail_mem=42.00 GB):  95%|█████████▍| 55/58 [00:09<00:00, 13.98it/s] Capturing num tokens (num_tokens=8 avail_mem=42.00 GB):  98%|█████████▊| 57/58 [00:09<00:00, 12.73it/s]Capturing num tokens (num_tokens=4 avail_mem=42.00 GB):  98%|█████████▊| 57/58 [00:09<00:00, 12.73it/s]Capturing num tokens (num_tokens=4 avail_mem=42.00 GB): 100%|██████████| 58/58 [00:09<00:00,  5.99it/s]


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



<strong style='color: #00008B;'>To solve the problem 1 plus 3, I start by identifying the two numbers involved, which are 1 and 3.<br><br>Next, I add these two numbers together: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>To solve the problem \(1 + 3\), follow these simple steps:<br><br>1. **Identify the numbers to add:**  <br>   - First number: \(1\)  <br>   - Second number: \(3\)<br><br>2. **Add the numbers together:**  <br>   \[<br>   1 + 3 = 4<br>   \]<br><br>3. **Present the final answer:**  <br>   \[<br>   \boxed{4}<br>   \]</strong>


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



<strong style='color: #00008B;'>First, I need to identify the two numbers in the problem, which are 1 and 3.<br><br>Next, I will add these two numbers together to find the sum.<br><br>Finally, I will calculate the total by adding 1 and 3, resulting in 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'><br><br>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the final answer is \(\boxed{4}\).</strong>


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



<strong style='color: #00008B;'>I need to add the numbers 1 and 3.<br><br>First, I'll identify the two numbers to be added: 1 and 3.<br><br>Next, I'll perform the addition: 1 plus 3 equals 4.<br><br>Therefore, the sum of 1 and 3 is 4.</strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'><br><br>**Solution:**<br><br>We need to find the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>**Answer:** \(\boxed{4}\)</strong>


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



<strong style='color: #00008B;'>First, I need to identify the two numbers in the problem, which are 1 and 3.<br><br>Next, I'll add these two numbers together.<br><br>After performing the addition, the result is 4.<br></think><br><br>Sure! Let's solve the problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:** \(\boxed{4}\)</strong>


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



<strong style='color: #00008B;'>First, I need to identify the two numbers in the problem, which are 1 and 3.<br><br>Next, I'll add these two numbers together.<br><br>Adding 1 and 3 gives a sum of 4.<br><br>Therefore, the final answer is 4.<br></think><br><br>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>1. **Identify the numbers to add:**<br>   <br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   <br>   \[<br>   1 + 3 = 4<br>   \]<br><br>3. **Final Answer:**<br>   <br>   \[<br>   \boxed{4}<br>   \]</strong>


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:328: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      return await dependant.call(**values)



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>First, I need to identify the two numbers in the problem, which are 1 and 3.<br><br>Next, I'll add these two numbers together.<br><br>Adding 1 and 3 gives a sum of 4.<br><br>Therefore, the final answer is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>1. **Identify the numbers to add:**<br>   <br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   <br>   \[<br>   1 + 3 = 4<br>   \]<br><br>3. **Final Answer:**<br>   <br>   \[<br>   \boxed{4}<br>   \]</strong>



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


    2026-04-01 19:57:23.850 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 19:57:23] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 19:57:23.850 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 19:57:23] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 19:57:23.850 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 19:57:23] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 19:57:23.850 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 19:57:23] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 19:57:23.850 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 19:57:23] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:00<00:00,  1.04it/s]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.19s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.15s/it]


    2026-04-01 19:57:28,628 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 19:57:28] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:41,  2.83s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:41,  2.83s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:18,  1.41s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:18,  1.41s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:46,  1.17it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:46,  1.17it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:31,  1.69it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:31,  1.69it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:03<00:23,  2.30it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:03<00:23,  2.30it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:03<00:17,  2.91it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:03<00:17,  2.91it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:13,  3.70it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:13,  3.70it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.48it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.48it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.37it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.37it/s]

    Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.37it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:06,  7.02it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:06,  7.02it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:06,  7.02it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:04<00:05,  8.53it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:04<00:05,  8.53it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:04<00:05,  8.53it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:04<00:04, 10.03it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:04<00:04, 10.03it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:04<00:04, 10.03it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:04<00:03, 11.92it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:04<00:03, 11.92it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:04<00:03, 11.92it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:04<00:03, 11.92it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 15.08it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 15.08it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 15.08it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 15.08it/s]

    Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:05<00:02, 15.08it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:05<00:01, 20.26it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:05<00:01, 20.26it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:05<00:01, 20.26it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:05<00:01, 20.26it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:05<00:01, 20.26it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:05<00:01, 20.26it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:05<00:01, 26.83it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:05<00:01, 26.83it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:05<00:01, 26.83it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:05<00:01, 26.83it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:05<00:01, 26.83it/s]

    Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:05<00:01, 26.83it/s]Compiling num tokens (num_tokens=320):  50%|█████     | 29/58 [00:05<00:01, 26.83it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:05<00:00, 33.62it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:05<00:00, 33.62it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:05<00:00, 33.62it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:05<00:00, 33.62it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:05<00:00, 33.62it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:05<00:00, 33.62it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:05<00:00, 33.62it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:05<00:00, 39.49it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:05<00:00, 39.49it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:05<00:00, 39.49it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:05<00:00, 39.49it/s]

    Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:05<00:00, 39.49it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:05<00:00, 39.49it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:05<00:00, 39.49it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:05<00:00, 43.88it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:05<00:00, 43.88it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:05<00:00, 43.88it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:05<00:00, 43.88it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:05<00:00, 43.88it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:05<00:00, 43.88it/s]Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:05<00:00, 43.88it/s]Compiling num tokens (num_tokens=20):  81%|████████  | 47/58 [00:05<00:00, 43.88it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:05<00:00, 50.50it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:05<00:00, 50.50it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:05<00:00, 50.50it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:05<00:00, 50.50it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:05<00:00, 50.50it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:05<00:00, 10.15it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=43.14 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=43.14 GB):   2%|▏         | 1/58 [00:00<00:17,  3.21it/s]Capturing num tokens (num_tokens=7680 avail_mem=43.11 GB):   2%|▏         | 1/58 [00:00<00:17,  3.21it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=43.11 GB):   3%|▎         | 2/58 [00:00<00:15,  3.51it/s]Capturing num tokens (num_tokens=7168 avail_mem=43.11 GB):   3%|▎         | 2/58 [00:00<00:15,  3.51it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=43.11 GB):   5%|▌         | 3/58 [00:01<00:22,  2.49it/s]Capturing num tokens (num_tokens=6656 avail_mem=42.11 GB):   5%|▌         | 3/58 [00:01<00:22,  2.49it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=42.11 GB):   7%|▋         | 4/58 [00:01<00:24,  2.18it/s]Capturing num tokens (num_tokens=6144 avail_mem=42.12 GB):   7%|▋         | 4/58 [00:01<00:24,  2.18it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=42.12 GB):   9%|▊         | 5/58 [00:02<00:24,  2.14it/s]Capturing num tokens (num_tokens=5632 avail_mem=42.12 GB):   9%|▊         | 5/58 [00:02<00:24,  2.14it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=42.12 GB):  10%|█         | 6/58 [00:02<00:22,  2.27it/s]Capturing num tokens (num_tokens=5120 avail_mem=42.12 GB):  10%|█         | 6/58 [00:02<00:22,  2.27it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=42.12 GB):  12%|█▏        | 7/58 [00:02<00:21,  2.38it/s]Capturing num tokens (num_tokens=4608 avail_mem=42.13 GB):  12%|█▏        | 7/58 [00:02<00:21,  2.38it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=42.13 GB):  14%|█▍        | 8/58 [00:03<00:19,  2.55it/s]Capturing num tokens (num_tokens=4096 avail_mem=42.13 GB):  14%|█▍        | 8/58 [00:03<00:19,  2.55it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=42.13 GB):  16%|█▌        | 9/58 [00:03<00:17,  2.75it/s]Capturing num tokens (num_tokens=3840 avail_mem=42.14 GB):  16%|█▌        | 9/58 [00:03<00:17,  2.75it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=42.14 GB):  17%|█▋        | 10/58 [00:03<00:16,  2.91it/s]Capturing num tokens (num_tokens=3584 avail_mem=42.13 GB):  17%|█▋        | 10/58 [00:03<00:16,  2.91it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=42.13 GB):  19%|█▉        | 11/58 [00:04<00:15,  3.07it/s]Capturing num tokens (num_tokens=3328 avail_mem=42.13 GB):  19%|█▉        | 11/58 [00:04<00:15,  3.07it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=42.13 GB):  21%|██        | 12/58 [00:04<00:14,  3.26it/s]Capturing num tokens (num_tokens=3072 avail_mem=42.13 GB):  21%|██        | 12/58 [00:04<00:14,  3.26it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=42.13 GB):  22%|██▏       | 13/58 [00:04<00:12,  3.51it/s]Capturing num tokens (num_tokens=2816 avail_mem=42.13 GB):  22%|██▏       | 13/58 [00:04<00:12,  3.51it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=42.13 GB):  24%|██▍       | 14/58 [00:04<00:11,  3.83it/s]Capturing num tokens (num_tokens=2560 avail_mem=42.13 GB):  24%|██▍       | 14/58 [00:04<00:11,  3.83it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=42.13 GB):  26%|██▌       | 15/58 [00:05<00:10,  4.11it/s]Capturing num tokens (num_tokens=2304 avail_mem=42.13 GB):  26%|██▌       | 15/58 [00:05<00:10,  4.11it/s]Capturing num tokens (num_tokens=2304 avail_mem=42.13 GB):  28%|██▊       | 16/58 [00:05<00:09,  4.57it/s]Capturing num tokens (num_tokens=2048 avail_mem=42.13 GB):  28%|██▊       | 16/58 [00:05<00:09,  4.57it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=42.13 GB):  29%|██▉       | 17/58 [00:05<00:08,  5.10it/s]Capturing num tokens (num_tokens=1792 avail_mem=42.13 GB):  29%|██▉       | 17/58 [00:05<00:08,  5.10it/s]Capturing num tokens (num_tokens=1792 avail_mem=42.13 GB):  31%|███       | 18/58 [00:05<00:07,  5.66it/s]Capturing num tokens (num_tokens=1536 avail_mem=42.13 GB):  31%|███       | 18/58 [00:05<00:07,  5.66it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=42.13 GB):  33%|███▎      | 19/58 [00:05<00:06,  6.29it/s]Capturing num tokens (num_tokens=1280 avail_mem=42.13 GB):  33%|███▎      | 19/58 [00:05<00:06,  6.29it/s]Capturing num tokens (num_tokens=1024 avail_mem=42.13 GB):  33%|███▎      | 19/58 [00:05<00:06,  6.29it/s]Capturing num tokens (num_tokens=1024 avail_mem=42.13 GB):  36%|███▌      | 21/58 [00:05<00:04,  8.08it/s]Capturing num tokens (num_tokens=960 avail_mem=42.13 GB):  36%|███▌      | 21/58 [00:05<00:04,  8.08it/s] 

    Capturing num tokens (num_tokens=896 avail_mem=42.12 GB):  36%|███▌      | 21/58 [00:05<00:04,  8.08it/s]Capturing num tokens (num_tokens=896 avail_mem=42.12 GB):  40%|███▉      | 23/58 [00:05<00:03,  9.89it/s]Capturing num tokens (num_tokens=832 avail_mem=42.12 GB):  40%|███▉      | 23/58 [00:05<00:03,  9.89it/s]Capturing num tokens (num_tokens=768 avail_mem=42.11 GB):  40%|███▉      | 23/58 [00:05<00:03,  9.89it/s]Capturing num tokens (num_tokens=768 avail_mem=42.11 GB):  43%|████▎     | 25/58 [00:06<00:02, 11.76it/s]Capturing num tokens (num_tokens=704 avail_mem=42.11 GB):  43%|████▎     | 25/58 [00:06<00:02, 11.76it/s]

    Capturing num tokens (num_tokens=640 avail_mem=42.10 GB):  43%|████▎     | 25/58 [00:06<00:02, 11.76it/s]Capturing num tokens (num_tokens=640 avail_mem=42.10 GB):  47%|████▋     | 27/58 [00:06<00:02, 13.68it/s]Capturing num tokens (num_tokens=576 avail_mem=42.10 GB):  47%|████▋     | 27/58 [00:06<00:02, 13.68it/s]Capturing num tokens (num_tokens=512 avail_mem=42.09 GB):  47%|████▋     | 27/58 [00:06<00:02, 13.68it/s]Capturing num tokens (num_tokens=480 avail_mem=42.09 GB):  47%|████▋     | 27/58 [00:06<00:02, 13.68it/s]Capturing num tokens (num_tokens=480 avail_mem=42.09 GB):  52%|█████▏    | 30/58 [00:06<00:01, 15.91it/s]Capturing num tokens (num_tokens=448 avail_mem=42.09 GB):  52%|█████▏    | 30/58 [00:06<00:01, 15.91it/s]

    Capturing num tokens (num_tokens=416 avail_mem=42.08 GB):  52%|█████▏    | 30/58 [00:06<00:01, 15.91it/s]Capturing num tokens (num_tokens=384 avail_mem=42.08 GB):  52%|█████▏    | 30/58 [00:06<00:01, 15.91it/s]Capturing num tokens (num_tokens=384 avail_mem=42.08 GB):  57%|█████▋    | 33/58 [00:06<00:01, 18.27it/s]Capturing num tokens (num_tokens=352 avail_mem=42.08 GB):  57%|█████▋    | 33/58 [00:06<00:01, 18.27it/s]Capturing num tokens (num_tokens=320 avail_mem=42.07 GB):  57%|█████▋    | 33/58 [00:06<00:01, 18.27it/s]Capturing num tokens (num_tokens=288 avail_mem=42.07 GB):  57%|█████▋    | 33/58 [00:06<00:01, 18.27it/s]Capturing num tokens (num_tokens=256 avail_mem=42.06 GB):  57%|█████▋    | 33/58 [00:06<00:01, 18.27it/s]Capturing num tokens (num_tokens=256 avail_mem=42.06 GB):  64%|██████▍   | 37/58 [00:06<00:00, 23.34it/s]Capturing num tokens (num_tokens=240 avail_mem=42.06 GB):  64%|██████▍   | 37/58 [00:06<00:00, 23.34it/s]Capturing num tokens (num_tokens=224 avail_mem=42.06 GB):  64%|██████▍   | 37/58 [00:06<00:00, 23.34it/s]

    Capturing num tokens (num_tokens=208 avail_mem=42.05 GB):  64%|██████▍   | 37/58 [00:06<00:00, 23.34it/s]Capturing num tokens (num_tokens=192 avail_mem=42.05 GB):  64%|██████▍   | 37/58 [00:06<00:00, 23.34it/s]Capturing num tokens (num_tokens=176 avail_mem=42.05 GB):  64%|██████▍   | 37/58 [00:06<00:00, 23.34it/s]Capturing num tokens (num_tokens=176 avail_mem=42.05 GB):  72%|███████▏  | 42/58 [00:06<00:00, 28.58it/s]Capturing num tokens (num_tokens=160 avail_mem=42.04 GB):  72%|███████▏  | 42/58 [00:06<00:00, 28.58it/s]Capturing num tokens (num_tokens=144 avail_mem=42.04 GB):  72%|███████▏  | 42/58 [00:06<00:00, 28.58it/s]Capturing num tokens (num_tokens=128 avail_mem=42.05 GB):  72%|███████▏  | 42/58 [00:06<00:00, 28.58it/s]Capturing num tokens (num_tokens=112 avail_mem=42.05 GB):  72%|███████▏  | 42/58 [00:06<00:00, 28.58it/s]Capturing num tokens (num_tokens=96 avail_mem=42.04 GB):  72%|███████▏  | 42/58 [00:06<00:00, 28.58it/s] Capturing num tokens (num_tokens=96 avail_mem=42.04 GB):  81%|████████  | 47/58 [00:06<00:00, 32.08it/s]Capturing num tokens (num_tokens=80 avail_mem=42.03 GB):  81%|████████  | 47/58 [00:06<00:00, 32.08it/s]

    Capturing num tokens (num_tokens=64 avail_mem=41.00 GB):  81%|████████  | 47/58 [00:06<00:00, 32.08it/s]Capturing num tokens (num_tokens=48 avail_mem=40.98 GB):  81%|████████  | 47/58 [00:06<00:00, 32.08it/s]

    Capturing num tokens (num_tokens=32 avail_mem=41.16 GB):  81%|████████  | 47/58 [00:07<00:00, 32.08it/s]Capturing num tokens (num_tokens=32 avail_mem=41.16 GB):  88%|████████▊ | 51/58 [00:07<00:00, 20.77it/s]Capturing num tokens (num_tokens=28 avail_mem=42.63 GB):  88%|████████▊ | 51/58 [00:07<00:00, 20.77it/s]Capturing num tokens (num_tokens=24 avail_mem=41.48 GB):  88%|████████▊ | 51/58 [00:07<00:00, 20.77it/s]Capturing num tokens (num_tokens=20 avail_mem=41.31 GB):  88%|████████▊ | 51/58 [00:07<00:00, 20.77it/s]

    Capturing num tokens (num_tokens=20 avail_mem=41.31 GB):  93%|█████████▎| 54/58 [00:07<00:00, 18.00it/s]Capturing num tokens (num_tokens=16 avail_mem=40.49 GB):  93%|█████████▎| 54/58 [00:07<00:00, 18.00it/s]Capturing num tokens (num_tokens=12 avail_mem=40.48 GB):  93%|█████████▎| 54/58 [00:07<00:00, 18.00it/s]Capturing num tokens (num_tokens=8 avail_mem=41.43 GB):  93%|█████████▎| 54/58 [00:07<00:00, 18.00it/s] 

    Capturing num tokens (num_tokens=8 avail_mem=41.43 GB):  98%|█████████▊| 57/58 [00:07<00:00, 15.74it/s]Capturing num tokens (num_tokens=4 avail_mem=41.30 GB):  98%|█████████▊| 57/58 [00:07<00:00, 15.74it/s]Capturing num tokens (num_tokens=4 avail_mem=41.30 GB): 100%|██████████| 58/58 [00:07<00:00,  7.60it/s]



<strong style='color: #00008B;'>==== Original Output ====</strong>



<strong style='color: #00008B;'>First, I need to identify the two numbers in the problem, which are 1 and 3.<br><br>Next, I'll add these two numbers together.<br><br>1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></think><br><br>**Solution:**<br><br>We are asked to find the sum of **1** and **3**.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the final answer is \(\boxed{4}\).</strong>



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>First, I need to identify the two numbers in the problem, which are 1 and 3.<br><br>Next, I'll add these two numbers together.<br><br>1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>**Solution:**<br><br>We are asked to find the sum of **1** and **3**.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the final answer is \(\boxed{4}\).</strong>



```python
llm.shutdown()
```

## Supporting New Reasoning Model Schemas

For future reasoning models, you can implement the reasoning parser as a subclass of `BaseReasoningFormatDetector` in `python/sglang/srt/reasoning_parser.py` and specify the reasoning parser for new reasoning model schemas accordingly.
