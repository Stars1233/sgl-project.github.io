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
    2026-04-01 09:42:09.460 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 09:42:09] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 09:42:09.460 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 09:42:09] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 09:42:09.460 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 09:42:09] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 09:42:09.460 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 09:42:09] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 09:42:09.460 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 09:42:09] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:01<00:01,  1.30s/it]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:03<00:00,  1.84s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:03<00:00,  1.76s/it]


    2026-04-01 09:42:15,503 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 09:42:15] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:03,  3.21s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:03,  3.21s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:27,  1.56s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:27,  1.56s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:51,  1.07it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:51,  1.07it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:34,  1.57it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:34,  1.57it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:24,  2.15it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:24,  2.15it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.79it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.79it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.47it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.47it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.26it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.26it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.14it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.14it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:04<00:08,  5.87it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:04<00:08,  5.87it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:07,  6.22it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:07,  6.22it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:05<00:06,  6.60it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:05<00:06,  6.60it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:05<00:06,  7.03it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:05<00:06,  7.03it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:05<00:05,  7.53it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:05<00:05,  7.53it/s]Compiling num tokens (num_tokens=2304):  24%|██▍       | 14/58 [00:05<00:05,  7.53it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:05<00:04,  8.80it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:05<00:04,  8.80it/s]

    Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:05<00:04,  8.80it/s]Compiling num tokens (num_tokens=1536):  28%|██▊       | 16/58 [00:05<00:04,  8.80it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:05<00:03, 12.52it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:05<00:03, 12.52it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:05<00:03, 12.52it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:05<00:03, 12.52it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:05<00:02, 16.52it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:05<00:02, 16.52it/s]

    Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:05<00:02, 16.52it/s]Compiling num tokens (num_tokens=768):  38%|███▊      | 22/58 [00:05<00:02, 16.52it/s]Compiling num tokens (num_tokens=704):  38%|███▊      | 22/58 [00:05<00:02, 16.52it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:05<00:01, 21.90it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:05<00:01, 21.90it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:05<00:01, 21.90it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:05<00:01, 21.90it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:05<00:01, 21.90it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:05<00:01, 21.90it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:05<00:00, 28.51it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:05<00:00, 28.51it/s]

    Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:05<00:00, 28.51it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:05<00:00, 28.51it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:05<00:00, 28.51it/s]Compiling num tokens (num_tokens=288):  53%|█████▎    | 31/58 [00:06<00:00, 28.51it/s]Compiling num tokens (num_tokens=256):  53%|█████▎    | 31/58 [00:06<00:00, 28.51it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:06<00:00, 34.78it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:06<00:00, 34.78it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:06<00:00, 34.78it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:06<00:00, 34.78it/s]Compiling num tokens (num_tokens=192):  64%|██████▍   | 37/58 [00:06<00:00, 34.78it/s]Compiling num tokens (num_tokens=176):  64%|██████▍   | 37/58 [00:06<00:00, 34.78it/s]

    Compiling num tokens (num_tokens=160):  64%|██████▍   | 37/58 [00:06<00:00, 34.78it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:06<00:00, 39.22it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:06<00:00, 39.22it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:06<00:00, 39.22it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:06<00:00, 39.22it/s]Compiling num tokens (num_tokens=96):  74%|███████▍  | 43/58 [00:06<00:00, 39.22it/s] Compiling num tokens (num_tokens=80):  74%|███████▍  | 43/58 [00:06<00:00, 39.22it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:06<00:00, 41.27it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:06<00:00, 41.27it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:06<00:00, 41.27it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:06<00:00, 41.27it/s]Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:06<00:00, 41.27it/s]Compiling num tokens (num_tokens=24):  83%|████████▎ | 48/58 [00:06<00:00, 41.27it/s]

    Compiling num tokens (num_tokens=20):  83%|████████▎ | 48/58 [00:06<00:00, 41.27it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:06<00:00, 46.21it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:06<00:00, 46.21it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:06<00:00, 46.21it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:06<00:00, 46.21it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:06<00:00, 46.21it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:06<00:00,  9.03it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=94.05 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=94.05 GB):   2%|▏         | 1/58 [00:00<00:17,  3.30it/s]Capturing num tokens (num_tokens=7680 avail_mem=94.02 GB):   2%|▏         | 1/58 [00:00<00:17,  3.30it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=94.02 GB):   3%|▎         | 2/58 [00:00<00:15,  3.51it/s]Capturing num tokens (num_tokens=7168 avail_mem=94.02 GB):   3%|▎         | 2/58 [00:00<00:15,  3.51it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=94.02 GB):   5%|▌         | 3/58 [00:00<00:14,  3.72it/s]Capturing num tokens (num_tokens=6656 avail_mem=94.02 GB):   5%|▌         | 3/58 [00:00<00:14,  3.72it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=94.02 GB):   7%|▋         | 4/58 [00:01<00:13,  4.00it/s]Capturing num tokens (num_tokens=6144 avail_mem=94.02 GB):   7%|▋         | 4/58 [00:01<00:13,  4.00it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=94.02 GB):   9%|▊         | 5/58 [00:01<00:15,  3.34it/s]Capturing num tokens (num_tokens=5632 avail_mem=93.03 GB):   9%|▊         | 5/58 [00:01<00:15,  3.34it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=93.03 GB):  10%|█         | 6/58 [00:01<00:17,  3.01it/s]Capturing num tokens (num_tokens=5120 avail_mem=93.03 GB):  10%|█         | 6/58 [00:01<00:17,  3.01it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=93.03 GB):  12%|█▏        | 7/58 [00:02<00:17,  2.91it/s]Capturing num tokens (num_tokens=4608 avail_mem=92.94 GB):  12%|█▏        | 7/58 [00:02<00:17,  2.91it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=92.94 GB):  14%|█▍        | 8/58 [00:02<00:17,  2.92it/s]Capturing num tokens (num_tokens=4096 avail_mem=92.59 GB):  14%|█▍        | 8/58 [00:02<00:17,  2.92it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=92.59 GB):  16%|█▌        | 9/58 [00:02<00:16,  2.96it/s]Capturing num tokens (num_tokens=3840 avail_mem=92.12 GB):  16%|█▌        | 9/58 [00:02<00:16,  2.96it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=92.12 GB):  17%|█▋        | 10/58 [00:03<00:15,  3.04it/s]Capturing num tokens (num_tokens=3584 avail_mem=91.74 GB):  17%|█▋        | 10/58 [00:03<00:15,  3.04it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=91.74 GB):  19%|█▉        | 11/58 [00:03<00:14,  3.19it/s]Capturing num tokens (num_tokens=3328 avail_mem=91.73 GB):  19%|█▉        | 11/58 [00:03<00:14,  3.19it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=91.73 GB):  21%|██        | 12/58 [00:03<00:13,  3.32it/s]Capturing num tokens (num_tokens=3072 avail_mem=90.20 GB):  21%|██        | 12/58 [00:03<00:13,  3.32it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=90.20 GB):  22%|██▏       | 13/58 [00:03<00:12,  3.50it/s]Capturing num tokens (num_tokens=2816 avail_mem=89.19 GB):  22%|██▏       | 13/58 [00:03<00:12,  3.50it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=89.19 GB):  24%|██▍       | 14/58 [00:04<00:11,  3.74it/s]Capturing num tokens (num_tokens=2560 avail_mem=89.19 GB):  24%|██▍       | 14/58 [00:04<00:11,  3.74it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=89.19 GB):  26%|██▌       | 15/58 [00:04<00:10,  4.02it/s]Capturing num tokens (num_tokens=2304 avail_mem=89.19 GB):  26%|██▌       | 15/58 [00:04<00:10,  4.02it/s]Capturing num tokens (num_tokens=2304 avail_mem=89.19 GB):  28%|██▊       | 16/58 [00:04<00:09,  4.31it/s]Capturing num tokens (num_tokens=2048 avail_mem=89.19 GB):  28%|██▊       | 16/58 [00:04<00:09,  4.31it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=89.19 GB):  29%|██▉       | 17/58 [00:04<00:08,  4.63it/s]Capturing num tokens (num_tokens=1792 avail_mem=89.19 GB):  29%|██▉       | 17/58 [00:04<00:08,  4.63it/s]Capturing num tokens (num_tokens=1792 avail_mem=89.19 GB):  31%|███       | 18/58 [00:04<00:08,  4.96it/s]Capturing num tokens (num_tokens=1536 avail_mem=89.19 GB):  31%|███       | 18/58 [00:04<00:08,  4.96it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=89.19 GB):  33%|███▎      | 19/58 [00:05<00:07,  5.23it/s]Capturing num tokens (num_tokens=1280 avail_mem=89.19 GB):  33%|███▎      | 19/58 [00:05<00:07,  5.23it/s]Capturing num tokens (num_tokens=1280 avail_mem=89.19 GB):  34%|███▍      | 20/58 [00:05<00:06,  5.77it/s]Capturing num tokens (num_tokens=1024 avail_mem=89.19 GB):  34%|███▍      | 20/58 [00:05<00:06,  5.77it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=89.19 GB):  36%|███▌      | 21/58 [00:05<00:05,  6.28it/s]Capturing num tokens (num_tokens=960 avail_mem=89.19 GB):  36%|███▌      | 21/58 [00:05<00:05,  6.28it/s] Capturing num tokens (num_tokens=960 avail_mem=89.19 GB):  38%|███▊      | 22/58 [00:05<00:05,  6.98it/s]Capturing num tokens (num_tokens=896 avail_mem=89.18 GB):  38%|███▊      | 22/58 [00:05<00:05,  6.98it/s]

    Capturing num tokens (num_tokens=896 avail_mem=89.18 GB):  40%|███▉      | 23/58 [00:05<00:04,  7.49it/s]Capturing num tokens (num_tokens=832 avail_mem=89.18 GB):  40%|███▉      | 23/58 [00:05<00:04,  7.49it/s]Capturing num tokens (num_tokens=768 avail_mem=89.17 GB):  40%|███▉      | 23/58 [00:05<00:04,  7.49it/s]Capturing num tokens (num_tokens=768 avail_mem=89.17 GB):  43%|████▎     | 25/58 [00:05<00:03,  8.79it/s]Capturing num tokens (num_tokens=704 avail_mem=89.17 GB):  43%|████▎     | 25/58 [00:05<00:03,  8.79it/s]

    Capturing num tokens (num_tokens=640 avail_mem=89.17 GB):  43%|████▎     | 25/58 [00:05<00:03,  8.79it/s]Capturing num tokens (num_tokens=640 avail_mem=89.17 GB):  47%|████▋     | 27/58 [00:05<00:02, 10.67it/s]Capturing num tokens (num_tokens=576 avail_mem=89.16 GB):  47%|████▋     | 27/58 [00:05<00:02, 10.67it/s]Capturing num tokens (num_tokens=512 avail_mem=89.16 GB):  47%|████▋     | 27/58 [00:05<00:02, 10.67it/s]Capturing num tokens (num_tokens=512 avail_mem=89.16 GB):  50%|█████     | 29/58 [00:06<00:02, 12.39it/s]Capturing num tokens (num_tokens=480 avail_mem=89.16 GB):  50%|█████     | 29/58 [00:06<00:02, 12.39it/s]

    Capturing num tokens (num_tokens=448 avail_mem=89.15 GB):  50%|█████     | 29/58 [00:06<00:02, 12.39it/s]Capturing num tokens (num_tokens=416 avail_mem=89.15 GB):  50%|█████     | 29/58 [00:06<00:02, 12.39it/s]Capturing num tokens (num_tokens=416 avail_mem=89.15 GB):  55%|█████▌    | 32/58 [00:06<00:01, 15.49it/s]Capturing num tokens (num_tokens=384 avail_mem=89.15 GB):  55%|█████▌    | 32/58 [00:06<00:01, 15.49it/s]Capturing num tokens (num_tokens=352 avail_mem=89.14 GB):  55%|█████▌    | 32/58 [00:06<00:01, 15.49it/s]Capturing num tokens (num_tokens=320 avail_mem=89.13 GB):  55%|█████▌    | 32/58 [00:06<00:01, 15.49it/s]

    Capturing num tokens (num_tokens=320 avail_mem=89.13 GB):  60%|██████    | 35/58 [00:06<00:01, 18.23it/s]Capturing num tokens (num_tokens=288 avail_mem=89.13 GB):  60%|██████    | 35/58 [00:06<00:01, 18.23it/s]Capturing num tokens (num_tokens=256 avail_mem=89.12 GB):  60%|██████    | 35/58 [00:06<00:01, 18.23it/s]Capturing num tokens (num_tokens=240 avail_mem=89.12 GB):  60%|██████    | 35/58 [00:06<00:01, 18.23it/s]Capturing num tokens (num_tokens=240 avail_mem=89.12 GB):  66%|██████▌   | 38/58 [00:06<00:01, 17.48it/s]Capturing num tokens (num_tokens=224 avail_mem=89.12 GB):  66%|██████▌   | 38/58 [00:06<00:01, 17.48it/s]

    Capturing num tokens (num_tokens=208 avail_mem=89.11 GB):  66%|██████▌   | 38/58 [00:06<00:01, 17.48it/s]Capturing num tokens (num_tokens=192 avail_mem=89.11 GB):  66%|██████▌   | 38/58 [00:06<00:01, 17.48it/s]Capturing num tokens (num_tokens=192 avail_mem=89.11 GB):  71%|███████   | 41/58 [00:06<00:00, 19.46it/s]Capturing num tokens (num_tokens=176 avail_mem=89.11 GB):  71%|███████   | 41/58 [00:06<00:00, 19.46it/s]Capturing num tokens (num_tokens=160 avail_mem=89.10 GB):  71%|███████   | 41/58 [00:06<00:00, 19.46it/s]Capturing num tokens (num_tokens=144 avail_mem=89.10 GB):  71%|███████   | 41/58 [00:06<00:00, 19.46it/s]Capturing num tokens (num_tokens=128 avail_mem=89.11 GB):  71%|███████   | 41/58 [00:06<00:00, 19.46it/s]Capturing num tokens (num_tokens=128 avail_mem=89.11 GB):  78%|███████▊  | 45/58 [00:06<00:00, 23.96it/s]Capturing num tokens (num_tokens=112 avail_mem=89.11 GB):  78%|███████▊  | 45/58 [00:06<00:00, 23.96it/s]

    Capturing num tokens (num_tokens=96 avail_mem=89.10 GB):  78%|███████▊  | 45/58 [00:06<00:00, 23.96it/s] Capturing num tokens (num_tokens=80 avail_mem=89.09 GB):  78%|███████▊  | 45/58 [00:06<00:00, 23.96it/s]Capturing num tokens (num_tokens=64 avail_mem=89.09 GB):  78%|███████▊  | 45/58 [00:06<00:00, 23.96it/s]Capturing num tokens (num_tokens=64 avail_mem=89.09 GB):  84%|████████▍ | 49/58 [00:06<00:00, 27.27it/s]Capturing num tokens (num_tokens=48 avail_mem=89.09 GB):  84%|████████▍ | 49/58 [00:06<00:00, 27.27it/s]Capturing num tokens (num_tokens=32 avail_mem=89.08 GB):  84%|████████▍ | 49/58 [00:06<00:00, 27.27it/s]

    Capturing num tokens (num_tokens=28 avail_mem=88.05 GB):  84%|████████▍ | 49/58 [00:06<00:00, 27.27it/s]Capturing num tokens (num_tokens=28 avail_mem=88.05 GB):  90%|████████▉ | 52/58 [00:07<00:00, 21.39it/s]Capturing num tokens (num_tokens=24 avail_mem=88.05 GB):  90%|████████▉ | 52/58 [00:07<00:00, 21.39it/s]

    Capturing num tokens (num_tokens=20 avail_mem=88.04 GB):  90%|████████▉ | 52/58 [00:07<00:00, 21.39it/s]Capturing num tokens (num_tokens=16 avail_mem=89.04 GB):  90%|████████▉ | 52/58 [00:07<00:00, 21.39it/s]Capturing num tokens (num_tokens=16 avail_mem=89.04 GB):  95%|█████████▍| 55/58 [00:07<00:00, 17.49it/s]Capturing num tokens (num_tokens=12 avail_mem=89.04 GB):  95%|█████████▍| 55/58 [00:07<00:00, 17.49it/s]

    Capturing num tokens (num_tokens=8 avail_mem=88.21 GB):  95%|█████████▍| 55/58 [00:07<00:00, 17.49it/s] Capturing num tokens (num_tokens=4 avail_mem=88.21 GB):  95%|█████████▍| 55/58 [00:07<00:00, 17.49it/s]

    Capturing num tokens (num_tokens=4 avail_mem=88.21 GB): 100%|██████████| 58/58 [00:07<00:00, 13.89it/s]Capturing num tokens (num_tokens=4 avail_mem=88.21 GB): 100%|██████████| 58/58 [00:07<00:00,  7.66it/s]


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



<strong style='color: #00008B;'>**Solution:**<br><br>We need to calculate the sum of the numbers 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the final answer is \(\boxed{4}\).</strong>


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



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I add the two numbers together: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'><br><br>**Solution:**<br><br>To find the sum of 1 and 3, follow these simple steps:<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>3. **Write the final answer:**<br>   \[<br>   \boxed{4}<br>   \]<br><br>**Answer:** \(\boxed{4}\)</strong>


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



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I add the two numbers together to find the total.<br><br>Finally, I arrive at the result, which is 4.</strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'><br><br>Sure! Let's solve the problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:** \(\boxed{4}\)</strong>


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



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I add the two numbers together: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></think><br><br>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>3. **Final Answer:**<br>   \[<br>   \boxed{4}<br>   \]</strong>


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



<strong style='color: #00008B;'>To find the sum of 1 and 3, I start by identifying the two numbers involved.<br><br>Next, I add the two numbers together to get the total.<br><br>Finally, I conclude that the sum of 1 and 3 is 4.<br></think><br><br>**Solution:**<br><br>We want to find the sum of 1 and 3.<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the two numbers:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>3. **Final Answer:**<br>   \[<br>   \boxed{4}<br>   \]</strong>


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:328: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      return await dependant.call(**values)



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>To find the sum of 1 and 3, I start by identifying the two numbers involved.<br><br>Next, I add the two numbers together to get the total.<br><br>Finally, I conclude that the sum of 1 and 3 is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>**Solution:**<br><br>We want to find the sum of 1 and 3.<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the two numbers:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>3. **Final Answer:**<br>   \[<br>   \boxed{4}<br>   \]</strong>



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


    2026-04-01 09:43:00.772 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 09:43:00] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 09:43:00.772 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 09:43:00] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 09:43:00.772 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 09:43:00] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 09:43:00.772 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 09:43:00] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 09:43:00.772 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 09:43:00] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:00<00:00,  1.34it/s]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.05s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.00s/it]


    2026-04-01 09:43:05,311 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 09:43:05] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:58,  3.14s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:58,  3.14s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:40,  1.80s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:40,  1.80s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:04,  1.17s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:04,  1.17s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:46,  1.16it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:46,  1.16it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:35,  1.49it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:35,  1.49it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:28,  1.80it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:28,  1.80it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:23,  2.14it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:23,  2.14it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:06<00:20,  2.49it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:06<00:20,  2.49it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:06<00:16,  2.89it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:06<00:16,  2.89it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:06<00:14,  3.33it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:06<00:14,  3.33it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:06<00:12,  3.72it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:06<00:12,  3.72it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:06<00:10,  4.18it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:06<00:10,  4.18it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:06<00:09,  4.64it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:06<00:09,  4.64it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:07<00:08,  5.13it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:07<00:08,  5.13it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:07<00:07,  5.71it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:07<00:07,  5.71it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:07<00:06,  6.30it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:07<00:06,  6.30it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:07<00:05,  7.03it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:07<00:05,  7.03it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:07<00:05,  7.03it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:07<00:04,  8.60it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:07<00:04,  8.60it/s]

    Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:07<00:04,  8.60it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:07<00:03, 10.59it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:07<00:03, 10.59it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:07<00:03, 10.59it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:07<00:02, 12.51it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:07<00:02, 12.51it/s]

    Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:07<00:02, 12.51it/s]Compiling num tokens (num_tokens=704):  40%|███▉      | 23/58 [00:07<00:02, 12.51it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:07<00:02, 15.97it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:07<00:02, 15.97it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:08<00:02, 15.97it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:08<00:02, 15.97it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:08<00:01, 18.93it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:08<00:01, 18.93it/s]

    Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:08<00:01, 18.93it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:08<00:01, 18.93it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:08<00:01, 18.93it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:08<00:01, 22.74it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:08<00:01, 22.74it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:08<00:01, 22.74it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:08<00:01, 22.74it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:08<00:01, 22.74it/s]

    Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:08<00:00, 26.03it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:08<00:00, 26.03it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:08<00:00, 26.03it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:08<00:00, 26.03it/s]Compiling num tokens (num_tokens=192):  64%|██████▍   | 37/58 [00:08<00:00, 26.03it/s]Compiling num tokens (num_tokens=176):  64%|██████▍   | 37/58 [00:08<00:00, 26.03it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:08<00:00, 30.37it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:08<00:00, 30.37it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:08<00:00, 30.37it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:08<00:00, 30.37it/s]Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:08<00:00, 30.37it/s]

    Compiling num tokens (num_tokens=96):  72%|███████▏  | 42/58 [00:08<00:00, 30.37it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:08<00:00, 33.17it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:08<00:00, 33.17it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:08<00:00, 33.17it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:08<00:00, 33.17it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:08<00:00, 33.17it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:08<00:00, 33.17it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:08<00:00, 37.10it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:08<00:00, 37.10it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:08<00:00, 37.10it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:08<00:00, 37.10it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:08<00:00, 37.10it/s]

    Compiling num tokens (num_tokens=8):  90%|████████▉ | 52/58 [00:08<00:00, 37.10it/s] Compiling num tokens (num_tokens=4):  90%|████████▉ | 52/58 [00:08<00:00, 37.10it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:08<00:00,  6.59it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=85.21 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=85.21 GB):   2%|▏         | 1/58 [00:00<00:51,  1.10it/s]Capturing num tokens (num_tokens=7680 avail_mem=85.15 GB):   2%|▏         | 1/58 [00:00<00:51,  1.10it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=85.15 GB):   3%|▎         | 2/58 [00:01<00:47,  1.17it/s]Capturing num tokens (num_tokens=7168 avail_mem=84.16 GB):   3%|▎         | 2/58 [00:01<00:47,  1.17it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=84.16 GB):   5%|▌         | 3/58 [00:02<00:44,  1.24it/s]Capturing num tokens (num_tokens=6656 avail_mem=84.23 GB):   5%|▌         | 3/58 [00:02<00:44,  1.24it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=84.23 GB):   7%|▋         | 4/58 [00:03<00:40,  1.32it/s]Capturing num tokens (num_tokens=6144 avail_mem=84.30 GB):   7%|▋         | 4/58 [00:03<00:40,  1.32it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=84.30 GB):   9%|▊         | 5/58 [00:03<00:38,  1.39it/s]Capturing num tokens (num_tokens=5632 avail_mem=84.38 GB):   9%|▊         | 5/58 [00:03<00:38,  1.39it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=84.38 GB):  10%|█         | 6/58 [00:04<00:34,  1.51it/s]Capturing num tokens (num_tokens=5120 avail_mem=84.45 GB):  10%|█         | 6/58 [00:04<00:34,  1.51it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=84.45 GB):  12%|█▏        | 7/58 [00:04<00:31,  1.62it/s]Capturing num tokens (num_tokens=4608 avail_mem=84.52 GB):  12%|█▏        | 7/58 [00:04<00:31,  1.62it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=84.52 GB):  14%|█▍        | 8/58 [00:05<00:28,  1.76it/s]Capturing num tokens (num_tokens=4096 avail_mem=84.59 GB):  14%|█▍        | 8/58 [00:05<00:28,  1.76it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=84.59 GB):  16%|█▌        | 9/58 [00:05<00:25,  1.92it/s]Capturing num tokens (num_tokens=3840 avail_mem=84.67 GB):  16%|█▌        | 9/58 [00:05<00:25,  1.92it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=84.67 GB):  17%|█▋        | 10/58 [00:06<00:23,  2.07it/s]Capturing num tokens (num_tokens=3584 avail_mem=84.70 GB):  17%|█▋        | 10/58 [00:06<00:23,  2.07it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=84.70 GB):  19%|█▉        | 11/58 [00:06<00:22,  2.10it/s]Capturing num tokens (num_tokens=3328 avail_mem=84.73 GB):  19%|█▉        | 11/58 [00:06<00:22,  2.10it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=84.73 GB):  21%|██        | 12/58 [00:06<00:19,  2.34it/s]Capturing num tokens (num_tokens=3072 avail_mem=84.76 GB):  21%|██        | 12/58 [00:06<00:19,  2.34it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=84.76 GB):  22%|██▏       | 13/58 [00:07<00:17,  2.61it/s]Capturing num tokens (num_tokens=2816 avail_mem=84.80 GB):  22%|██▏       | 13/58 [00:07<00:17,  2.61it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=84.80 GB):  24%|██▍       | 14/58 [00:07<00:14,  2.96it/s]Capturing num tokens (num_tokens=2560 avail_mem=84.83 GB):  24%|██▍       | 14/58 [00:07<00:14,  2.96it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=84.83 GB):  26%|██▌       | 15/58 [00:07<00:12,  3.33it/s]Capturing num tokens (num_tokens=2304 avail_mem=84.86 GB):  26%|██▌       | 15/58 [00:07<00:12,  3.33it/s]Capturing num tokens (num_tokens=2304 avail_mem=84.86 GB):  28%|██▊       | 16/58 [00:07<00:11,  3.80it/s]Capturing num tokens (num_tokens=2048 avail_mem=84.89 GB):  28%|██▊       | 16/58 [00:07<00:11,  3.80it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=84.89 GB):  29%|██▉       | 17/58 [00:08<00:09,  4.33it/s]Capturing num tokens (num_tokens=1792 avail_mem=84.93 GB):  29%|██▉       | 17/58 [00:08<00:09,  4.33it/s]Capturing num tokens (num_tokens=1792 avail_mem=84.93 GB):  31%|███       | 18/58 [00:08<00:08,  4.93it/s]Capturing num tokens (num_tokens=1536 avail_mem=84.95 GB):  31%|███       | 18/58 [00:08<00:08,  4.93it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=84.95 GB):  33%|███▎      | 19/58 [00:08<00:06,  5.68it/s]Capturing num tokens (num_tokens=1280 avail_mem=84.98 GB):  33%|███▎      | 19/58 [00:08<00:06,  5.68it/s]Capturing num tokens (num_tokens=1280 avail_mem=84.98 GB):  34%|███▍      | 20/58 [00:08<00:05,  6.38it/s]Capturing num tokens (num_tokens=1024 avail_mem=85.01 GB):  34%|███▍      | 20/58 [00:08<00:05,  6.38it/s]Capturing num tokens (num_tokens=960 avail_mem=85.03 GB):  34%|███▍      | 20/58 [00:08<00:05,  6.38it/s] 

    Capturing num tokens (num_tokens=960 avail_mem=85.03 GB):  38%|███▊      | 22/58 [00:08<00:04,  8.54it/s]Capturing num tokens (num_tokens=896 avail_mem=85.05 GB):  38%|███▊      | 22/58 [00:08<00:04,  8.54it/s]Capturing num tokens (num_tokens=832 avail_mem=85.05 GB):  38%|███▊      | 22/58 [00:08<00:04,  8.54it/s]Capturing num tokens (num_tokens=832 avail_mem=85.05 GB):  41%|████▏     | 24/58 [00:08<00:03, 10.38it/s]Capturing num tokens (num_tokens=768 avail_mem=85.04 GB):  41%|████▏     | 24/58 [00:08<00:03, 10.38it/s]Capturing num tokens (num_tokens=704 avail_mem=85.04 GB):  41%|████▏     | 24/58 [00:08<00:03, 10.38it/s]

    Capturing num tokens (num_tokens=704 avail_mem=85.04 GB):  45%|████▍     | 26/58 [00:08<00:02, 10.90it/s]Capturing num tokens (num_tokens=640 avail_mem=84.01 GB):  45%|████▍     | 26/58 [00:08<00:02, 10.90it/s]Capturing num tokens (num_tokens=576 avail_mem=84.01 GB):  45%|████▍     | 26/58 [00:08<00:02, 10.90it/s]

    Capturing num tokens (num_tokens=576 avail_mem=84.01 GB):  48%|████▊     | 28/58 [00:09<00:03,  9.21it/s]Capturing num tokens (num_tokens=512 avail_mem=84.01 GB):  48%|████▊     | 28/58 [00:09<00:03,  9.21it/s]Capturing num tokens (num_tokens=480 avail_mem=85.03 GB):  48%|████▊     | 28/58 [00:09<00:03,  9.21it/s]Capturing num tokens (num_tokens=480 avail_mem=85.03 GB):  52%|█████▏    | 30/58 [00:09<00:02,  9.48it/s]Capturing num tokens (num_tokens=448 avail_mem=84.21 GB):  52%|█████▏    | 30/58 [00:09<00:02,  9.48it/s]

    Capturing num tokens (num_tokens=416 avail_mem=84.20 GB):  52%|█████▏    | 30/58 [00:09<00:02,  9.48it/s]Capturing num tokens (num_tokens=416 avail_mem=84.20 GB):  55%|█████▌    | 32/58 [00:09<00:02,  8.89it/s]Capturing num tokens (num_tokens=384 avail_mem=84.20 GB):  55%|█████▌    | 32/58 [00:09<00:02,  8.89it/s]

    Capturing num tokens (num_tokens=384 avail_mem=84.20 GB):  57%|█████▋    | 33/58 [00:09<00:02,  8.87it/s]Capturing num tokens (num_tokens=352 avail_mem=85.02 GB):  57%|█████▋    | 33/58 [00:09<00:02,  8.87it/s]Capturing num tokens (num_tokens=320 avail_mem=85.01 GB):  57%|█████▋    | 33/58 [00:09<00:02,  8.87it/s]

    Capturing num tokens (num_tokens=320 avail_mem=85.01 GB):  60%|██████    | 35/58 [00:10<00:03,  7.35it/s]Capturing num tokens (num_tokens=288 avail_mem=84.26 GB):  60%|██████    | 35/58 [00:10<00:03,  7.35it/s]Capturing num tokens (num_tokens=288 avail_mem=84.26 GB):  62%|██████▏   | 36/58 [00:10<00:02,  7.54it/s]Capturing num tokens (num_tokens=256 avail_mem=84.27 GB):  62%|██████▏   | 36/58 [00:10<00:02,  7.54it/s]

    Capturing num tokens (num_tokens=256 avail_mem=84.27 GB):  64%|██████▍   | 37/58 [00:10<00:02,  7.95it/s]Capturing num tokens (num_tokens=240 avail_mem=85.06 GB):  64%|██████▍   | 37/58 [00:10<00:02,  7.95it/s]Capturing num tokens (num_tokens=240 avail_mem=85.06 GB):  66%|██████▌   | 38/58 [00:10<00:02,  8.23it/s]Capturing num tokens (num_tokens=224 avail_mem=85.03 GB):  66%|██████▌   | 38/58 [00:10<00:02,  8.23it/s]

    Capturing num tokens (num_tokens=224 avail_mem=85.03 GB):  67%|██████▋   | 39/58 [00:10<00:02,  8.39it/s]Capturing num tokens (num_tokens=208 avail_mem=84.31 GB):  67%|██████▋   | 39/58 [00:10<00:02,  8.39it/s]Capturing num tokens (num_tokens=208 avail_mem=84.31 GB):  69%|██████▉   | 40/58 [00:10<00:02,  8.46it/s]Capturing num tokens (num_tokens=192 avail_mem=84.30 GB):  69%|██████▉   | 40/58 [00:10<00:02,  8.46it/s]

    Capturing num tokens (num_tokens=176 avail_mem=85.01 GB):  69%|██████▉   | 40/58 [00:10<00:02,  8.46it/s]Capturing num tokens (num_tokens=176 avail_mem=85.01 GB):  72%|███████▏  | 42/58 [00:10<00:01,  9.33it/s]Capturing num tokens (num_tokens=160 avail_mem=84.34 GB):  72%|███████▏  | 42/58 [00:10<00:01,  9.33it/s]

    Capturing num tokens (num_tokens=160 avail_mem=84.34 GB):  74%|███████▍  | 43/58 [00:10<00:01,  9.25it/s]Capturing num tokens (num_tokens=144 avail_mem=84.34 GB):  74%|███████▍  | 43/58 [00:10<00:01,  9.25it/s]Capturing num tokens (num_tokens=144 avail_mem=84.34 GB):  76%|███████▌  | 44/58 [00:10<00:01,  9.27it/s]Capturing num tokens (num_tokens=128 avail_mem=85.02 GB):  76%|███████▌  | 44/58 [00:10<00:01,  9.27it/s]Capturing num tokens (num_tokens=112 avail_mem=84.41 GB):  76%|███████▌  | 44/58 [00:11<00:01,  9.27it/s]

    Capturing num tokens (num_tokens=112 avail_mem=84.41 GB):  79%|███████▉  | 46/58 [00:11<00:01,  9.83it/s]Capturing num tokens (num_tokens=96 avail_mem=84.40 GB):  79%|███████▉  | 46/58 [00:11<00:01,  9.83it/s] Capturing num tokens (num_tokens=80 avail_mem=85.00 GB):  79%|███████▉  | 46/58 [00:11<00:01,  9.83it/s]Capturing num tokens (num_tokens=80 avail_mem=85.00 GB):  83%|████████▎ | 48/58 [00:11<00:00, 10.62it/s]Capturing num tokens (num_tokens=64 avail_mem=84.42 GB):  83%|████████▎ | 48/58 [00:11<00:00, 10.62it/s]

    Capturing num tokens (num_tokens=48 avail_mem=84.42 GB):  83%|████████▎ | 48/58 [00:11<00:00, 10.62it/s]Capturing num tokens (num_tokens=48 avail_mem=84.42 GB):  86%|████████▌ | 50/58 [00:11<00:00, 10.32it/s]Capturing num tokens (num_tokens=32 avail_mem=84.98 GB):  86%|████████▌ | 50/58 [00:11<00:00, 10.32it/s]Capturing num tokens (num_tokens=28 avail_mem=84.46 GB):  86%|████████▌ | 50/58 [00:11<00:00, 10.32it/s]

    Capturing num tokens (num_tokens=28 avail_mem=84.46 GB):  90%|████████▉ | 52/58 [00:11<00:00, 10.67it/s]Capturing num tokens (num_tokens=24 avail_mem=84.45 GB):  90%|████████▉ | 52/58 [00:11<00:00, 10.67it/s]Capturing num tokens (num_tokens=20 avail_mem=84.96 GB):  90%|████████▉ | 52/58 [00:11<00:00, 10.67it/s]Capturing num tokens (num_tokens=20 avail_mem=84.96 GB):  93%|█████████▎| 54/58 [00:11<00:00, 10.77it/s]Capturing num tokens (num_tokens=16 avail_mem=84.91 GB):  93%|█████████▎| 54/58 [00:11<00:00, 10.77it/s]

    Capturing num tokens (num_tokens=12 avail_mem=84.51 GB):  93%|█████████▎| 54/58 [00:11<00:00, 10.77it/s]Capturing num tokens (num_tokens=12 avail_mem=84.51 GB):  97%|█████████▋| 56/58 [00:12<00:00, 10.82it/s]Capturing num tokens (num_tokens=8 avail_mem=84.95 GB):  97%|█████████▋| 56/58 [00:12<00:00, 10.82it/s] Capturing num tokens (num_tokens=4 avail_mem=84.52 GB):  97%|█████████▋| 56/58 [00:12<00:00, 10.82it/s]

    Capturing num tokens (num_tokens=4 avail_mem=84.52 GB): 100%|██████████| 58/58 [00:12<00:00, 10.96it/s]Capturing num tokens (num_tokens=4 avail_mem=84.52 GB): 100%|██████████| 58/58 [00:12<00:00,  4.74it/s]



<strong style='color: #00008B;'>==== Original Output ====</strong>



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of the numbers 1 and 3.<br><br>I will start by identifying the two numbers involved in the addition.<br><br>Next, I'll add the two numbers together to find the total.<br><br>Finally, I'll present the result as the answer to the problem.<br></think><br><br>Sure! Let's solve the problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:** \(\boxed{4}\)</strong>



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of the numbers 1 and 3.<br><br>I will start by identifying the two numbers involved in the addition.<br><br>Next, I'll add the two numbers together to find the total.<br><br>Finally, I'll present the result as the answer to the problem.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>Sure! Let's solve the problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:** \(\boxed{4}\)</strong>



```python
llm.shutdown()
```

## Supporting New Reasoning Model Schemas

For future reasoning models, you can implement the reasoning parser as a subclass of `BaseReasoningFormatDetector` in `python/sglang/srt/reasoning_parser.py` and specify the reasoning parser for new reasoning model schemas accordingly.
