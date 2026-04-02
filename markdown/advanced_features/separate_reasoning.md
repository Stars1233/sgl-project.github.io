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


    2026-04-02 01:51:25.322 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 01:51:25] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 01:51:25.322 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 01:51:25] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 01:51:25.322 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 01:51:25] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 01:51:25.322 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 01:51:25] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 01:51:25.322 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 01:51:25] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:01<00:01,  1.58s/it]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:03<00:00,  1.62s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:03<00:00,  1.61s/it]


    2026-04-02 01:51:31,317 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 01:51:31] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:07,  3.29s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:07,  3.29s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:28,  1.58s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:28,  1.58s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:52,  1.06it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:52,  1.06it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:34,  1.55it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:34,  1.55it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:24,  2.14it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:24,  2.14it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.77it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.77it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.51it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.51it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.29it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.29it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.18it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.18it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.18it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:06,  6.87it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:06,  6.87it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:06,  6.87it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:05<00:05,  8.39it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:05<00:05,  8.39it/s]

    Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:05<00:05,  8.39it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:05<00:04,  9.98it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:05<00:04,  9.98it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:05<00:04,  9.98it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:03, 11.91it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:03, 11.91it/s]

    Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:03, 11.91it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:05<00:03, 11.91it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 15.17it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 15.17it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 15.17it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 15.17it/s]Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:05<00:02, 15.17it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:05<00:01, 20.27it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:05<00:01, 20.27it/s]

    Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:05<00:01, 20.27it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:05<00:01, 20.27it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:05<00:01, 20.27it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:05<00:01, 20.27it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:05<00:01, 26.78it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:05<00:01, 26.78it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:05<00:01, 26.78it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:05<00:01, 26.78it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:05<00:01, 26.78it/s]Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:05<00:01, 26.78it/s]Compiling num tokens (num_tokens=320):  50%|█████     | 29/58 [00:05<00:01, 26.78it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:05<00:00, 34.17it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:05<00:00, 34.17it/s]

    Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:05<00:00, 34.17it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:05<00:00, 34.17it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:05<00:00, 34.17it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:05<00:00, 34.17it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:05<00:00, 34.17it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:05<00:00, 39.52it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:05<00:00, 39.52it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:05<00:00, 39.52it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:05<00:00, 39.52it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:05<00:00, 39.52it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:05<00:00, 39.52it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:05<00:00, 39.52it/s] 

    Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:05<00:00, 42.67it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:05<00:00, 42.67it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:06<00:00, 42.67it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:06<00:00, 42.67it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:06<00:00, 42.67it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:06<00:00, 42.67it/s]Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:06<00:00, 42.67it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:06<00:00, 46.75it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:06<00:00, 46.75it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:06<00:00, 46.75it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:06<00:00, 46.75it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:06<00:00, 46.75it/s] Compiling num tokens (num_tokens=4):  91%|█████████▏| 53/58 [00:06<00:00, 46.75it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:06<00:00,  9.42it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=122.24 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=122.24 GB):   2%|▏         | 1/58 [00:00<00:20,  2.84it/s]Capturing num tokens (num_tokens=7680 avail_mem=122.19 GB):   2%|▏         | 1/58 [00:00<00:20,  2.84it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=122.19 GB):   3%|▎         | 2/58 [00:00<00:18,  3.11it/s]Capturing num tokens (num_tokens=7168 avail_mem=121.54 GB):   3%|▎         | 2/58 [00:00<00:18,  3.11it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=121.54 GB):   5%|▌         | 3/58 [00:00<00:15,  3.49it/s]Capturing num tokens (num_tokens=6656 avail_mem=121.54 GB):   5%|▌         | 3/58 [00:00<00:15,  3.49it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=121.54 GB):   7%|▋         | 4/58 [00:01<00:13,  3.87it/s]Capturing num tokens (num_tokens=6144 avail_mem=121.55 GB):   7%|▋         | 4/58 [00:01<00:13,  3.87it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=121.55 GB):   9%|▊         | 5/58 [00:01<00:12,  4.18it/s]Capturing num tokens (num_tokens=5632 avail_mem=121.55 GB):   9%|▊         | 5/58 [00:01<00:12,  4.18it/s]Capturing num tokens (num_tokens=5632 avail_mem=121.55 GB):  10%|█         | 6/58 [00:01<00:11,  4.57it/s]Capturing num tokens (num_tokens=5120 avail_mem=121.55 GB):  10%|█         | 6/58 [00:01<00:11,  4.57it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=121.55 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.66it/s]Capturing num tokens (num_tokens=4608 avail_mem=121.56 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.66it/s]Capturing num tokens (num_tokens=4608 avail_mem=121.56 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.02it/s]Capturing num tokens (num_tokens=4096 avail_mem=121.56 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.02it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=121.56 GB):  16%|█▌        | 9/58 [00:02<00:09,  4.95it/s]Capturing num tokens (num_tokens=3840 avail_mem=121.57 GB):  16%|█▌        | 9/58 [00:02<00:09,  4.95it/s]Capturing num tokens (num_tokens=3840 avail_mem=121.57 GB):  17%|█▋        | 10/58 [00:02<00:08,  5.59it/s]Capturing num tokens (num_tokens=3584 avail_mem=121.56 GB):  17%|█▋        | 10/58 [00:02<00:08,  5.59it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=121.56 GB):  19%|█▉        | 11/58 [00:02<00:07,  6.27it/s]Capturing num tokens (num_tokens=3328 avail_mem=121.56 GB):  19%|█▉        | 11/58 [00:02<00:07,  6.27it/s]Capturing num tokens (num_tokens=3328 avail_mem=121.56 GB):  21%|██        | 12/58 [00:02<00:06,  7.00it/s]Capturing num tokens (num_tokens=3072 avail_mem=121.56 GB):  21%|██        | 12/58 [00:02<00:06,  7.00it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=121.56 GB):  21%|██        | 12/58 [00:02<00:06,  7.00it/s]Capturing num tokens (num_tokens=2816 avail_mem=121.56 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.29it/s]Capturing num tokens (num_tokens=2560 avail_mem=121.56 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.29it/s]Capturing num tokens (num_tokens=2304 avail_mem=121.56 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.29it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=121.56 GB):  28%|██▊       | 16/58 [00:02<00:04,  9.70it/s]Capturing num tokens (num_tokens=2048 avail_mem=121.56 GB):  28%|██▊       | 16/58 [00:02<00:04,  9.70it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.49 GB):  28%|██▊       | 16/58 [00:02<00:04,  9.70it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.49 GB):  31%|███       | 18/58 [00:02<00:03, 11.17it/s]Capturing num tokens (num_tokens=1536 avail_mem=115.18 GB):  31%|███       | 18/58 [00:02<00:03, 11.17it/s]Capturing num tokens (num_tokens=1280 avail_mem=107.22 GB):  31%|███       | 18/58 [00:02<00:03, 11.17it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=107.22 GB):  34%|███▍      | 20/58 [00:03<00:02, 13.07it/s]Capturing num tokens (num_tokens=1024 avail_mem=107.22 GB):  34%|███▍      | 20/58 [00:03<00:02, 13.07it/s]Capturing num tokens (num_tokens=960 avail_mem=107.22 GB):  34%|███▍      | 20/58 [00:03<00:02, 13.07it/s] Capturing num tokens (num_tokens=896 avail_mem=107.22 GB):  34%|███▍      | 20/58 [00:03<00:02, 13.07it/s]Capturing num tokens (num_tokens=896 avail_mem=107.22 GB):  40%|███▉      | 23/58 [00:03<00:02, 16.38it/s]Capturing num tokens (num_tokens=832 avail_mem=107.21 GB):  40%|███▉      | 23/58 [00:03<00:02, 16.38it/s]Capturing num tokens (num_tokens=768 avail_mem=107.21 GB):  40%|███▉      | 23/58 [00:03<00:02, 16.38it/s]Capturing num tokens (num_tokens=704 avail_mem=107.20 GB):  40%|███▉      | 23/58 [00:03<00:02, 16.38it/s]

    Capturing num tokens (num_tokens=704 avail_mem=107.20 GB):  40%|███▉      | 23/58 [00:13<00:02, 16.38it/s]

    Capturing num tokens (num_tokens=704 avail_mem=107.20 GB):  45%|████▍     | 26/58 [00:31<01:55,  3.62s/it]Capturing num tokens (num_tokens=640 avail_mem=106.66 GB):  45%|████▍     | 26/58 [00:31<01:55,  3.62s/it]Capturing num tokens (num_tokens=576 avail_mem=106.02 GB):  45%|████▍     | 26/58 [00:31<01:55,  3.62s/it]Capturing num tokens (num_tokens=576 avail_mem=106.02 GB):  48%|████▊     | 28/58 [00:31<01:20,  2.69s/it]Capturing num tokens (num_tokens=512 avail_mem=105.91 GB):  48%|████▊     | 28/58 [00:31<01:20,  2.69s/it]

    Capturing num tokens (num_tokens=480 avail_mem=105.91 GB):  48%|████▊     | 28/58 [00:31<01:20,  2.69s/it]Capturing num tokens (num_tokens=480 avail_mem=105.91 GB):  52%|█████▏    | 30/58 [00:32<00:55,  1.99s/it]Capturing num tokens (num_tokens=448 avail_mem=105.91 GB):  52%|█████▏    | 30/58 [00:32<00:55,  1.99s/it]

    Capturing num tokens (num_tokens=416 avail_mem=105.91 GB):  52%|█████▏    | 30/58 [00:32<00:55,  1.99s/it]

    Capturing num tokens (num_tokens=416 avail_mem=105.91 GB):  55%|█████▌    | 32/58 [00:32<00:39,  1.53s/it]Capturing num tokens (num_tokens=384 avail_mem=105.90 GB):  55%|█████▌    | 32/58 [00:32<00:39,  1.53s/it]Capturing num tokens (num_tokens=352 avail_mem=105.89 GB):  55%|█████▌    | 32/58 [00:32<00:39,  1.53s/it]Capturing num tokens (num_tokens=352 avail_mem=105.89 GB):  59%|█████▊    | 34/58 [00:33<00:26,  1.12s/it]

    Capturing num tokens (num_tokens=320 avail_mem=105.89 GB):  59%|█████▊    | 34/58 [00:33<00:26,  1.12s/it]Capturing num tokens (num_tokens=288 avail_mem=105.89 GB):  59%|█████▊    | 34/58 [00:33<00:26,  1.12s/it]Capturing num tokens (num_tokens=288 avail_mem=105.89 GB):  62%|██████▏   | 36/58 [00:33<00:17,  1.23it/s]Capturing num tokens (num_tokens=256 avail_mem=105.88 GB):  62%|██████▏   | 36/58 [00:33<00:17,  1.23it/s]

    Capturing num tokens (num_tokens=240 avail_mem=105.88 GB):  62%|██████▏   | 36/58 [00:33<00:17,  1.23it/s]Capturing num tokens (num_tokens=240 avail_mem=105.88 GB):  66%|██████▌   | 38/58 [00:33<00:11,  1.67it/s]Capturing num tokens (num_tokens=224 avail_mem=105.87 GB):  66%|██████▌   | 38/58 [00:33<00:11,  1.67it/s]

    Capturing num tokens (num_tokens=208 avail_mem=105.87 GB):  66%|██████▌   | 38/58 [00:35<00:11,  1.67it/s]Capturing num tokens (num_tokens=208 avail_mem=105.87 GB):  69%|██████▉   | 40/58 [00:35<00:12,  1.44it/s]Capturing num tokens (num_tokens=192 avail_mem=105.87 GB):  69%|██████▉   | 40/58 [00:35<00:12,  1.44it/s]Capturing num tokens (num_tokens=176 avail_mem=105.86 GB):  69%|██████▉   | 40/58 [00:35<00:12,  1.44it/s]Capturing num tokens (num_tokens=160 avail_mem=105.86 GB):  69%|██████▉   | 40/58 [00:35<00:12,  1.44it/s]Capturing num tokens (num_tokens=160 avail_mem=105.86 GB):  74%|███████▍  | 43/58 [00:35<00:06,  2.26it/s]Capturing num tokens (num_tokens=144 avail_mem=105.86 GB):  74%|███████▍  | 43/58 [00:35<00:06,  2.26it/s]

    Capturing num tokens (num_tokens=128 avail_mem=105.87 GB):  74%|███████▍  | 43/58 [00:35<00:06,  2.26it/s]Capturing num tokens (num_tokens=128 avail_mem=105.87 GB):  78%|███████▊  | 45/58 [00:35<00:04,  2.93it/s]Capturing num tokens (num_tokens=112 avail_mem=105.86 GB):  78%|███████▊  | 45/58 [00:35<00:04,  2.93it/s]Capturing num tokens (num_tokens=96 avail_mem=105.86 GB):  78%|███████▊  | 45/58 [00:35<00:04,  2.93it/s] 

    Capturing num tokens (num_tokens=96 avail_mem=105.86 GB):  81%|████████  | 47/58 [00:35<00:03,  3.66it/s]Capturing num tokens (num_tokens=80 avail_mem=105.85 GB):  81%|████████  | 47/58 [00:35<00:03,  3.66it/s]Capturing num tokens (num_tokens=64 avail_mem=105.85 GB):  81%|████████  | 47/58 [00:35<00:03,  3.66it/s]Capturing num tokens (num_tokens=64 avail_mem=105.85 GB):  84%|████████▍ | 49/58 [00:35<00:01,  4.58it/s]Capturing num tokens (num_tokens=48 avail_mem=105.85 GB):  84%|████████▍ | 49/58 [00:35<00:01,  4.58it/s]

    Capturing num tokens (num_tokens=32 avail_mem=105.84 GB):  84%|████████▍ | 49/58 [00:35<00:01,  4.58it/s]Capturing num tokens (num_tokens=32 avail_mem=105.84 GB):  88%|████████▊ | 51/58 [00:35<00:01,  5.87it/s]Capturing num tokens (num_tokens=28 avail_mem=105.84 GB):  88%|████████▊ | 51/58 [00:35<00:01,  5.87it/s]Capturing num tokens (num_tokens=24 avail_mem=105.84 GB):  88%|████████▊ | 51/58 [00:35<00:01,  5.87it/s]Capturing num tokens (num_tokens=20 avail_mem=105.83 GB):  88%|████████▊ | 51/58 [00:35<00:01,  5.87it/s]Capturing num tokens (num_tokens=20 avail_mem=105.83 GB):  93%|█████████▎| 54/58 [00:36<00:00,  8.05it/s]Capturing num tokens (num_tokens=16 avail_mem=105.83 GB):  93%|█████████▎| 54/58 [00:36<00:00,  8.05it/s]

    Capturing num tokens (num_tokens=12 avail_mem=105.83 GB):  93%|█████████▎| 54/58 [00:36<00:00,  8.05it/s]Capturing num tokens (num_tokens=12 avail_mem=105.83 GB):  97%|█████████▋| 56/58 [00:36<00:00,  9.25it/s]Capturing num tokens (num_tokens=8 avail_mem=105.82 GB):  97%|█████████▋| 56/58 [00:36<00:00,  9.25it/s] Capturing num tokens (num_tokens=4 avail_mem=105.82 GB):  97%|█████████▋| 56/58 [00:36<00:00,  9.25it/s]Capturing num tokens (num_tokens=4 avail_mem=105.82 GB): 100%|██████████| 58/58 [00:36<00:00, 10.72it/s]Capturing num tokens (num_tokens=4 avail_mem=105.82 GB): 100%|██████████| 58/58 [00:36<00:00,  1.60it/s]


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



<strong style='color: #00008B;'>First, I need to identify the two numbers involved in the addition: 1 and 3.<br><br>Next, I'll add these two numbers together: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>Certainly! Let's solve the addition problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Final Answer:**<br>\[<br>\boxed{4}<br>\]</strong>


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



<strong style='color: #00008B;'>I need to add the numbers 1 and 3.<br><br>First, I recognize that 1 is a single unit.<br><br>Then, I add another 3 units to it.<br><br>Combining these, I get a total of 4.<br><br>So, the sum of 1 and 3 is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'><br><br>Sure! Let's solve the addition problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Start with the number 1.**<br>2. **Add 3 to it.**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:** \(\boxed{4}\)</strong>


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



<strong style='color: #00008B;'>First, I need to add the numbers 1 and 3 together.<br><br>Adding 1 and 3 gives me 4.<br><br>Therefore, the sum of 1 and 3 is 4.</strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'><br><br>**Solution:**<br><br>We are asked to find the sum of \(1\) and \(3\).<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the final answer is \(\boxed{4}\).</strong>


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



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I add the two numbers together: 1 plus 3 equals 4.<br><br>Finally, I present the result as the answer to the problem.<br></think><br><br>**Solution:**<br><br>We need to find the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the answer is \(\boxed{4}\).</strong>


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



<strong style='color: #00008B;'>First, I identify the numbers to be added, which are 1 and 3.<br><br>Next, I perform the addition operation by combining these two numbers.<br><br>Finally, I calculate the sum to find that 1 plus 3 equals 4.<br></think><br><br>Sure! Let's solve the addition problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Perform the addition:**<br>   \[<br>   1 + 3<br>   \]<br><br>3. **Calculate the sum:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:**<br>\[<br>\boxed{4}<br>\]</strong>


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:328: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      return await dependant.call(**values)



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>First, I identify the numbers to be added, which are 1 and 3.<br><br>Next, I perform the addition operation by combining these two numbers.<br><br>Finally, I calculate the sum to find that 1 plus 3 equals 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>Sure! Let's solve the addition problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Perform the addition:**<br>   \[<br>   1 + 3<br>   \]<br><br>3. **Calculate the sum:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:**<br>\[<br>\boxed{4}<br>\]</strong>



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


    2026-04-02 01:53:18.339 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 01:53:18] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 01:53:18.339 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 01:53:18] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 01:53:18.339 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 01:53:18] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 01:53:18.339 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 01:53:18] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 01:53:18.339 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 01:53:18] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:01<00:01,  1.23s/it]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.31s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.30s/it]


    2026-04-02 01:53:23,688 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 01:53:23] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:55,  3.07s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:55,  3.07s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:23,  1.49s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:23,  1.49s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:49,  1.11it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:49,  1.11it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:33,  1.63it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:33,  1.63it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:03<00:23,  2.23it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:03<00:23,  2.23it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.88it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.88it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.58it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.58it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.37it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.37it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.23it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.23it/s]

    Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.23it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:06,  6.87it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:06,  6.87it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:06,  6.87it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:04<00:05,  8.36it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:04<00:05,  8.36it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:04<00:05,  8.36it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:04<00:04,  9.95it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:04<00:04,  9.95it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:05<00:04,  9.95it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:03, 11.81it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:03, 11.81it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:03, 11.81it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:05<00:03, 11.81it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 14.91it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 14.91it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 14.91it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 14.91it/s]

    Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:05<00:02, 14.91it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:05<00:01, 19.98it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:05<00:01, 19.98it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:05<00:01, 19.98it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:05<00:01, 19.98it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:05<00:01, 19.98it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:05<00:01, 19.98it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:05<00:01, 26.52it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:05<00:01, 26.52it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:05<00:01, 26.52it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:05<00:01, 26.52it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:05<00:01, 26.52it/s]

    Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:05<00:01, 26.52it/s]Compiling num tokens (num_tokens=320):  50%|█████     | 29/58 [00:05<00:01, 26.52it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:05<00:00, 34.01it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:05<00:00, 34.01it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:05<00:00, 34.01it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:05<00:00, 34.01it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:05<00:00, 34.01it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:05<00:00, 34.01it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:05<00:00, 34.01it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:05<00:00, 39.42it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:05<00:00, 39.42it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:05<00:00, 39.42it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:05<00:00, 39.42it/s]

    Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:05<00:00, 39.42it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:05<00:00, 39.42it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:05<00:00, 41.81it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:05<00:00, 41.81it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:05<00:00, 41.81it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:05<00:00, 41.81it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:05<00:00, 41.81it/s]Compiling num tokens (num_tokens=32):  79%|███████▉  | 46/58 [00:05<00:00, 41.81it/s]Compiling num tokens (num_tokens=28):  79%|███████▉  | 46/58 [00:05<00:00, 41.81it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:05<00:00, 45.36it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:05<00:00, 45.36it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:05<00:00, 45.36it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:05<00:00, 45.36it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:05<00:00, 45.36it/s]

    Compiling num tokens (num_tokens=8):  90%|████████▉ | 52/58 [00:05<00:00, 45.36it/s] Compiling num tokens (num_tokens=4):  90%|████████▉ | 52/58 [00:05<00:00, 45.36it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:05<00:00,  9.72it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.37 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=118.37 GB):   2%|▏         | 1/58 [00:00<00:24,  2.36it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.34 GB):   2%|▏         | 1/58 [00:00<00:24,  2.36it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=118.34 GB):   3%|▎         | 2/58 [00:01<00:31,  1.79it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.34 GB):   3%|▎         | 2/58 [00:01<00:31,  1.79it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=118.34 GB):   5%|▌         | 3/58 [00:01<00:24,  2.24it/s]Capturing num tokens (num_tokens=6656 avail_mem=117.79 GB):   5%|▌         | 3/58 [00:01<00:24,  2.24it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=117.79 GB):   7%|▋         | 4/58 [00:01<00:23,  2.30it/s]Capturing num tokens (num_tokens=6144 avail_mem=117.80 GB):   7%|▋         | 4/58 [00:01<00:23,  2.30it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=117.80 GB):   9%|▊         | 5/58 [00:02<00:21,  2.43it/s]Capturing num tokens (num_tokens=5632 avail_mem=117.80 GB):   9%|▊         | 5/58 [00:02<00:21,  2.43it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=117.80 GB):  10%|█         | 6/58 [00:02<00:18,  2.80it/s]Capturing num tokens (num_tokens=5120 avail_mem=117.80 GB):  10%|█         | 6/58 [00:02<00:18,  2.80it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=117.80 GB):  12%|█▏        | 7/58 [00:02<00:15,  3.24it/s]Capturing num tokens (num_tokens=4608 avail_mem=117.80 GB):  12%|█▏        | 7/58 [00:02<00:15,  3.24it/s]Capturing num tokens (num_tokens=4608 avail_mem=117.80 GB):  14%|█▍        | 8/58 [00:02<00:13,  3.77it/s]Capturing num tokens (num_tokens=4096 avail_mem=117.81 GB):  14%|█▍        | 8/58 [00:02<00:13,  3.77it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=117.81 GB):  16%|█▌        | 9/58 [00:03<00:11,  4.11it/s]Capturing num tokens (num_tokens=3840 avail_mem=117.81 GB):  16%|█▌        | 9/58 [00:03<00:11,  4.11it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=117.81 GB):  17%|█▋        | 10/58 [00:03<00:12,  3.97it/s]Capturing num tokens (num_tokens=3584 avail_mem=117.81 GB):  17%|█▋        | 10/58 [00:03<00:12,  3.97it/s]Capturing num tokens (num_tokens=3584 avail_mem=117.81 GB):  19%|█▉        | 11/58 [00:03<00:10,  4.43it/s]Capturing num tokens (num_tokens=3328 avail_mem=117.23 GB):  19%|█▉        | 11/58 [00:03<00:10,  4.43it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=117.23 GB):  21%|██        | 12/58 [00:03<00:10,  4.54it/s]Capturing num tokens (num_tokens=3072 avail_mem=117.78 GB):  21%|██        | 12/58 [00:03<00:10,  4.54it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=117.78 GB):  22%|██▏       | 13/58 [00:03<00:09,  4.52it/s]Capturing num tokens (num_tokens=2816 avail_mem=117.78 GB):  22%|██▏       | 13/58 [00:03<00:09,  4.52it/s]Capturing num tokens (num_tokens=2816 avail_mem=117.78 GB):  24%|██▍       | 14/58 [00:04<00:09,  4.88it/s]Capturing num tokens (num_tokens=2560 avail_mem=117.79 GB):  24%|██▍       | 14/58 [00:04<00:09,  4.88it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=117.79 GB):  26%|██▌       | 15/58 [00:04<00:07,  5.49it/s]Capturing num tokens (num_tokens=2304 avail_mem=117.51 GB):  26%|██▌       | 15/58 [00:04<00:07,  5.49it/s]Capturing num tokens (num_tokens=2304 avail_mem=117.51 GB):  28%|██▊       | 16/58 [00:04<00:06,  6.09it/s]Capturing num tokens (num_tokens=2048 avail_mem=117.79 GB):  28%|██▊       | 16/58 [00:04<00:06,  6.09it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=117.79 GB):  29%|██▉       | 17/58 [00:04<00:06,  6.74it/s]Capturing num tokens (num_tokens=1792 avail_mem=117.79 GB):  29%|██▉       | 17/58 [00:04<00:06,  6.74it/s]Capturing num tokens (num_tokens=1792 avail_mem=117.79 GB):  31%|███       | 18/58 [00:04<00:05,  7.46it/s]Capturing num tokens (num_tokens=1536 avail_mem=117.78 GB):  31%|███       | 18/58 [00:04<00:05,  7.46it/s]Capturing num tokens (num_tokens=1280 avail_mem=117.66 GB):  31%|███       | 18/58 [00:04<00:05,  7.46it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=117.66 GB):  34%|███▍      | 20/58 [00:04<00:04,  9.32it/s]Capturing num tokens (num_tokens=1024 avail_mem=117.78 GB):  34%|███▍      | 20/58 [00:04<00:04,  9.32it/s]Capturing num tokens (num_tokens=960 avail_mem=117.78 GB):  34%|███▍      | 20/58 [00:04<00:04,  9.32it/s] Capturing num tokens (num_tokens=960 avail_mem=117.78 GB):  38%|███▊      | 22/58 [00:04<00:03, 11.13it/s]Capturing num tokens (num_tokens=896 avail_mem=117.71 GB):  38%|███▊      | 22/58 [00:04<00:03, 11.13it/s]Capturing num tokens (num_tokens=832 avail_mem=117.76 GB):  38%|███▊      | 22/58 [00:04<00:03, 11.13it/s]

    Capturing num tokens (num_tokens=832 avail_mem=117.76 GB):  41%|████▏     | 24/58 [00:04<00:02, 13.05it/s]Capturing num tokens (num_tokens=768 avail_mem=117.75 GB):  41%|████▏     | 24/58 [00:04<00:02, 13.05it/s]Capturing num tokens (num_tokens=704 avail_mem=117.75 GB):  41%|████▏     | 24/58 [00:04<00:02, 13.05it/s]Capturing num tokens (num_tokens=640 avail_mem=117.74 GB):  41%|████▏     | 24/58 [00:04<00:02, 13.05it/s]Capturing num tokens (num_tokens=640 avail_mem=117.74 GB):  47%|████▋     | 27/58 [00:05<00:01, 15.65it/s]Capturing num tokens (num_tokens=576 avail_mem=117.71 GB):  47%|████▋     | 27/58 [00:05<00:01, 15.65it/s]Capturing num tokens (num_tokens=512 avail_mem=117.72 GB):  47%|████▋     | 27/58 [00:05<00:01, 15.65it/s]

    Capturing num tokens (num_tokens=480 avail_mem=117.72 GB):  47%|████▋     | 27/58 [00:05<00:01, 15.65it/s]Capturing num tokens (num_tokens=480 avail_mem=117.72 GB):  52%|█████▏    | 30/58 [00:05<00:01, 18.28it/s]Capturing num tokens (num_tokens=448 avail_mem=117.69 GB):  52%|█████▏    | 30/58 [00:05<00:01, 18.28it/s]Capturing num tokens (num_tokens=416 avail_mem=117.70 GB):  52%|█████▏    | 30/58 [00:05<00:01, 18.28it/s]Capturing num tokens (num_tokens=384 avail_mem=117.70 GB):  52%|█████▏    | 30/58 [00:05<00:01, 18.28it/s]Capturing num tokens (num_tokens=384 avail_mem=117.70 GB):  57%|█████▋    | 33/58 [00:05<00:01, 20.83it/s]Capturing num tokens (num_tokens=352 avail_mem=117.70 GB):  57%|█████▋    | 33/58 [00:05<00:01, 20.83it/s]Capturing num tokens (num_tokens=320 avail_mem=117.70 GB):  57%|█████▋    | 33/58 [00:05<00:01, 20.83it/s]

    Capturing num tokens (num_tokens=288 avail_mem=117.69 GB):  57%|█████▋    | 33/58 [00:05<00:01, 20.83it/s]Capturing num tokens (num_tokens=256 avail_mem=117.68 GB):  57%|█████▋    | 33/58 [00:05<00:01, 20.83it/s]Capturing num tokens (num_tokens=256 avail_mem=117.68 GB):  64%|██████▍   | 37/58 [00:05<00:00, 24.04it/s]Capturing num tokens (num_tokens=240 avail_mem=117.68 GB):  64%|██████▍   | 37/58 [00:05<00:00, 24.04it/s]Capturing num tokens (num_tokens=224 avail_mem=117.67 GB):  64%|██████▍   | 37/58 [00:05<00:00, 24.04it/s]Capturing num tokens (num_tokens=208 avail_mem=117.66 GB):  64%|██████▍   | 37/58 [00:05<00:00, 24.04it/s]Capturing num tokens (num_tokens=192 avail_mem=117.65 GB):  64%|██████▍   | 37/58 [00:05<00:00, 24.04it/s]Capturing num tokens (num_tokens=192 avail_mem=117.65 GB):  71%|███████   | 41/58 [00:05<00:00, 26.49it/s]Capturing num tokens (num_tokens=176 avail_mem=117.65 GB):  71%|███████   | 41/58 [00:05<00:00, 26.49it/s]

    Capturing num tokens (num_tokens=160 avail_mem=117.64 GB):  71%|███████   | 41/58 [00:05<00:00, 26.49it/s]Capturing num tokens (num_tokens=144 avail_mem=117.63 GB):  71%|███████   | 41/58 [00:05<00:00, 26.49it/s]Capturing num tokens (num_tokens=128 avail_mem=117.64 GB):  71%|███████   | 41/58 [00:05<00:00, 26.49it/s]Capturing num tokens (num_tokens=128 avail_mem=117.64 GB):  78%|███████▊  | 45/58 [00:05<00:00, 27.76it/s]Capturing num tokens (num_tokens=112 avail_mem=117.63 GB):  78%|███████▊  | 45/58 [00:05<00:00, 27.76it/s]Capturing num tokens (num_tokens=96 avail_mem=117.62 GB):  78%|███████▊  | 45/58 [00:05<00:00, 27.76it/s] Capturing num tokens (num_tokens=80 avail_mem=117.62 GB):  78%|███████▊  | 45/58 [00:05<00:00, 27.76it/s]Capturing num tokens (num_tokens=64 avail_mem=117.62 GB):  78%|███████▊  | 45/58 [00:05<00:00, 27.76it/s]

    Capturing num tokens (num_tokens=64 avail_mem=117.62 GB):  84%|████████▍ | 49/58 [00:05<00:00, 30.08it/s]Capturing num tokens (num_tokens=48 avail_mem=117.61 GB):  84%|████████▍ | 49/58 [00:05<00:00, 30.08it/s]Capturing num tokens (num_tokens=32 avail_mem=117.61 GB):  84%|████████▍ | 49/58 [00:05<00:00, 30.08it/s]Capturing num tokens (num_tokens=28 avail_mem=117.61 GB):  84%|████████▍ | 49/58 [00:05<00:00, 30.08it/s]Capturing num tokens (num_tokens=24 avail_mem=117.60 GB):  84%|████████▍ | 49/58 [00:05<00:00, 30.08it/s]Capturing num tokens (num_tokens=24 avail_mem=117.60 GB):  91%|█████████▏| 53/58 [00:05<00:00, 32.28it/s]Capturing num tokens (num_tokens=20 avail_mem=117.60 GB):  91%|█████████▏| 53/58 [00:05<00:00, 32.28it/s]Capturing num tokens (num_tokens=16 avail_mem=117.60 GB):  91%|█████████▏| 53/58 [00:05<00:00, 32.28it/s]Capturing num tokens (num_tokens=12 avail_mem=117.59 GB):  91%|█████████▏| 53/58 [00:05<00:00, 32.28it/s]Capturing num tokens (num_tokens=8 avail_mem=117.59 GB):  91%|█████████▏| 53/58 [00:05<00:00, 32.28it/s] 

    Capturing num tokens (num_tokens=8 avail_mem=117.59 GB):  98%|█████████▊| 57/58 [00:05<00:00, 34.05it/s]Capturing num tokens (num_tokens=4 avail_mem=117.58 GB):  98%|█████████▊| 57/58 [00:05<00:00, 34.05it/s]Capturing num tokens (num_tokens=4 avail_mem=117.58 GB): 100%|██████████| 58/58 [00:05<00:00,  9.69it/s]



<strong style='color: #00008B;'>==== Original Output ====</strong>



<strong style='color: #00008B;'>First, I recognize that I need to calculate the sum of 1 and 3.<br><br>Next, I add the two numbers together: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></think><br><br>Sure! Let's solve the problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Final Answer:**<br>\[<br>\boxed{4}<br>\]</strong>



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>First, I recognize that I need to calculate the sum of 1 and 3.<br><br>Next, I add the two numbers together: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>Sure! Let's solve the problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Final Answer:**<br>\[<br>\boxed{4}<br>\]</strong>



```python
llm.shutdown()
```

## Supporting New Reasoning Model Schemas

For future reasoning models, you can implement the reasoning parser as a subclass of `BaseReasoningFormatDetector` in `python/sglang/srt/reasoning_parser.py` and specify the reasoning parser for new reasoning model schemas accordingly.
