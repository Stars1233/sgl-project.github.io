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
    2026-04-02 06:55:12.525 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:55:12] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 06:55:12.525 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:55:12] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 06:55:12.525 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:55:12] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 06:55:12.525 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:55:12] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 06:55:12.525 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:55:12] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Multi-thread loading shards:  50% Completed | 1/2 [00:01<00:01,  1.07s/it]

    Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.18s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.16s/it]


    2026-04-02 06:55:20,163 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 06:55:20] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:54,  3.05s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:54,  3.05s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:23,  1.49s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:23,  1.49s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:49,  1.12it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:49,  1.12it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:33,  1.63it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:33,  1.63it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:03<00:23,  2.23it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:03<00:23,  2.23it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.88it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.88it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.62it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.62it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.41it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.41it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.29it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.29it/s]

    Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.29it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:06,  6.97it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:06,  6.97it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:06,  6.97it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:04<00:05,  8.46it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:04<00:05,  8.46it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:04<00:05,  8.46it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:04<00:04, 10.01it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:04<00:04, 10.01it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:05<00:04, 10.01it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:03, 11.95it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:03, 11.95it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:03, 11.95it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:05<00:03, 11.95it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 15.24it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 15.24it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 15.24it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 15.24it/s]

    Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:05<00:02, 15.24it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:05<00:01, 20.41it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:05<00:01, 20.41it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:05<00:01, 20.41it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:05<00:01, 20.41it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:05<00:01, 20.41it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:05<00:01, 20.41it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:05<00:01, 26.97it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:05<00:01, 26.97it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:05<00:01, 26.97it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:05<00:01, 26.97it/s]

    Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:05<00:01, 26.97it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:05<00:00, 29.33it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:05<00:00, 29.33it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:05<00:00, 29.33it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:05<00:00, 29.33it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:05<00:00, 29.33it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:05<00:00, 29.33it/s]Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:05<00:00, 29.33it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:05<00:00, 36.71it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:05<00:00, 36.71it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:05<00:00, 36.71it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:05<00:00, 36.71it/s]

    Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:05<00:00, 36.71it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:05<00:00, 36.71it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:05<00:00, 38.65it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:05<00:00, 38.65it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:05<00:00, 38.65it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:05<00:00, 38.65it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:05<00:00, 38.65it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:05<00:00, 38.65it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:05<00:00, 38.65it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:05<00:00, 42.60it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:05<00:00, 42.60it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:05<00:00, 42.60it/s]

    Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:05<00:00, 42.60it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:05<00:00, 42.60it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:05<00:00, 42.60it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:05<00:00, 42.60it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:05<00:00, 42.60it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:05<00:00, 42.60it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:05<00:00, 51.77it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:05<00:00,  9.71it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=104.61 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=104.61 GB):   2%|▏         | 1/58 [00:00<00:16,  3.48it/s]Capturing num tokens (num_tokens=7680 avail_mem=104.58 GB):   2%|▏         | 1/58 [00:00<00:16,  3.48it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=104.58 GB):   3%|▎         | 2/58 [00:00<00:15,  3.59it/s]Capturing num tokens (num_tokens=7168 avail_mem=104.58 GB):   3%|▎         | 2/58 [00:00<00:15,  3.59it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=104.58 GB):   5%|▌         | 3/58 [00:00<00:14,  3.81it/s]Capturing num tokens (num_tokens=6656 avail_mem=104.59 GB):   5%|▌         | 3/58 [00:00<00:14,  3.81it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=104.59 GB):   7%|▋         | 4/58 [00:01<00:13,  4.09it/s]Capturing num tokens (num_tokens=6144 avail_mem=104.59 GB):   7%|▋         | 4/58 [00:01<00:13,  4.09it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=104.59 GB):   9%|▊         | 5/58 [00:01<00:12,  4.34it/s]Capturing num tokens (num_tokens=5632 avail_mem=104.59 GB):   9%|▊         | 5/58 [00:01<00:12,  4.34it/s]Capturing num tokens (num_tokens=5632 avail_mem=104.59 GB):  10%|█         | 6/58 [00:01<00:11,  4.72it/s]Capturing num tokens (num_tokens=5120 avail_mem=104.59 GB):  10%|█         | 6/58 [00:01<00:11,  4.72it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=104.59 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.66it/s]Capturing num tokens (num_tokens=4608 avail_mem=104.60 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.66it/s]Capturing num tokens (num_tokens=4608 avail_mem=104.60 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.17it/s]Capturing num tokens (num_tokens=4096 avail_mem=104.60 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.17it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=104.60 GB):  16%|█▌        | 9/58 [00:01<00:08,  5.73it/s]Capturing num tokens (num_tokens=3840 avail_mem=104.61 GB):  16%|█▌        | 9/58 [00:01<00:08,  5.73it/s]Capturing num tokens (num_tokens=3840 avail_mem=104.61 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.25it/s]Capturing num tokens (num_tokens=3584 avail_mem=104.61 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.25it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=104.61 GB):  19%|█▉        | 11/58 [00:02<00:06,  6.76it/s]Capturing num tokens (num_tokens=3328 avail_mem=104.61 GB):  19%|█▉        | 11/58 [00:02<00:06,  6.76it/s]Capturing num tokens (num_tokens=3328 avail_mem=104.61 GB):  21%|██        | 12/58 [00:02<00:06,  7.42it/s]Capturing num tokens (num_tokens=3072 avail_mem=104.60 GB):  21%|██        | 12/58 [00:02<00:06,  7.42it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=104.61 GB):  21%|██        | 12/58 [00:02<00:06,  7.42it/s]Capturing num tokens (num_tokens=2816 avail_mem=104.61 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.64it/s]Capturing num tokens (num_tokens=2560 avail_mem=104.61 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.64it/s]Capturing num tokens (num_tokens=2304 avail_mem=104.61 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.64it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=104.61 GB):  28%|██▊       | 16/58 [00:02<00:04, 10.00it/s]Capturing num tokens (num_tokens=2048 avail_mem=104.60 GB):  28%|██▊       | 16/58 [00:02<00:04, 10.00it/s]Capturing num tokens (num_tokens=1792 avail_mem=104.61 GB):  28%|██▊       | 16/58 [00:02<00:04, 10.00it/s]Capturing num tokens (num_tokens=1792 avail_mem=104.61 GB):  31%|███       | 18/58 [00:02<00:03, 11.11it/s]Capturing num tokens (num_tokens=1536 avail_mem=104.60 GB):  31%|███       | 18/58 [00:02<00:03, 11.11it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=104.60 GB):  31%|███       | 18/58 [00:02<00:03, 11.11it/s]Capturing num tokens (num_tokens=1280 avail_mem=104.60 GB):  34%|███▍      | 20/58 [00:02<00:03,  9.95it/s]Capturing num tokens (num_tokens=1024 avail_mem=104.60 GB):  34%|███▍      | 20/58 [00:02<00:03,  9.95it/s]

    Capturing num tokens (num_tokens=960 avail_mem=104.60 GB):  34%|███▍      | 20/58 [00:03<00:03,  9.95it/s] Capturing num tokens (num_tokens=960 avail_mem=104.60 GB):  38%|███▊      | 22/58 [00:03<00:03,  9.62it/s]Capturing num tokens (num_tokens=896 avail_mem=104.59 GB):  38%|███▊      | 22/58 [00:03<00:03,  9.62it/s]Capturing num tokens (num_tokens=832 avail_mem=104.59 GB):  38%|███▊      | 22/58 [00:03<00:03,  9.62it/s]

    Capturing num tokens (num_tokens=832 avail_mem=104.59 GB):  41%|████▏     | 24/58 [00:03<00:03, 10.60it/s]Capturing num tokens (num_tokens=768 avail_mem=104.59 GB):  41%|████▏     | 24/58 [00:03<00:03, 10.60it/s]Capturing num tokens (num_tokens=704 avail_mem=104.58 GB):  41%|████▏     | 24/58 [00:03<00:03, 10.60it/s]

    Capturing num tokens (num_tokens=704 avail_mem=104.58 GB):  45%|████▍     | 26/58 [00:03<00:03,  9.53it/s]Capturing num tokens (num_tokens=640 avail_mem=104.56 GB):  45%|████▍     | 26/58 [00:03<00:03,  9.53it/s]Capturing num tokens (num_tokens=576 avail_mem=104.56 GB):  45%|████▍     | 26/58 [00:03<00:03,  9.53it/s]Capturing num tokens (num_tokens=576 avail_mem=104.56 GB):  48%|████▊     | 28/58 [00:03<00:02, 10.75it/s]Capturing num tokens (num_tokens=512 avail_mem=104.55 GB):  48%|████▊     | 28/58 [00:03<00:02, 10.75it/s]Capturing num tokens (num_tokens=480 avail_mem=104.55 GB):  48%|████▊     | 28/58 [00:03<00:02, 10.75it/s]Capturing num tokens (num_tokens=448 avail_mem=104.55 GB):  48%|████▊     | 28/58 [00:03<00:02, 10.75it/s]

    Capturing num tokens (num_tokens=416 avail_mem=104.54 GB):  48%|████▊     | 28/58 [00:03<00:02, 10.75it/s]Capturing num tokens (num_tokens=416 avail_mem=104.54 GB):  55%|█████▌    | 32/58 [00:03<00:01, 15.85it/s]Capturing num tokens (num_tokens=384 avail_mem=104.54 GB):  55%|█████▌    | 32/58 [00:03<00:01, 15.85it/s]Capturing num tokens (num_tokens=352 avail_mem=104.53 GB):  55%|█████▌    | 32/58 [00:03<00:01, 15.85it/s]Capturing num tokens (num_tokens=320 avail_mem=104.51 GB):  55%|█████▌    | 32/58 [00:03<00:01, 15.85it/s]Capturing num tokens (num_tokens=288 avail_mem=104.49 GB):  55%|█████▌    | 32/58 [00:03<00:01, 15.85it/s]Capturing num tokens (num_tokens=288 avail_mem=104.49 GB):  62%|██████▏   | 36/58 [00:03<00:01, 20.00it/s]Capturing num tokens (num_tokens=256 avail_mem=104.45 GB):  62%|██████▏   | 36/58 [00:03<00:01, 20.00it/s]Capturing num tokens (num_tokens=240 avail_mem=104.43 GB):  62%|██████▏   | 36/58 [00:04<00:01, 20.00it/s]

    Capturing num tokens (num_tokens=224 avail_mem=104.42 GB):  62%|██████▏   | 36/58 [00:04<00:01, 20.00it/s]Capturing num tokens (num_tokens=208 avail_mem=104.41 GB):  62%|██████▏   | 36/58 [00:04<00:01, 20.00it/s]Capturing num tokens (num_tokens=208 avail_mem=104.41 GB):  69%|██████▉   | 40/58 [00:04<00:00, 23.77it/s]Capturing num tokens (num_tokens=192 avail_mem=104.41 GB):  69%|██████▉   | 40/58 [00:04<00:00, 23.77it/s]Capturing num tokens (num_tokens=176 avail_mem=104.41 GB):  69%|██████▉   | 40/58 [00:04<00:00, 23.77it/s]Capturing num tokens (num_tokens=160 avail_mem=104.40 GB):  69%|██████▉   | 40/58 [00:04<00:00, 23.77it/s]Capturing num tokens (num_tokens=160 avail_mem=104.40 GB):  74%|███████▍  | 43/58 [00:04<00:00, 23.47it/s]Capturing num tokens (num_tokens=144 avail_mem=104.40 GB):  74%|███████▍  | 43/58 [00:04<00:00, 23.47it/s]

    Capturing num tokens (num_tokens=128 avail_mem=104.41 GB):  74%|███████▍  | 43/58 [00:04<00:00, 23.47it/s]Capturing num tokens (num_tokens=112 avail_mem=104.41 GB):  74%|███████▍  | 43/58 [00:04<00:00, 23.47it/s]Capturing num tokens (num_tokens=112 avail_mem=104.41 GB):  79%|███████▉  | 46/58 [00:04<00:00, 24.18it/s]Capturing num tokens (num_tokens=96 avail_mem=103.90 GB):  79%|███████▉  | 46/58 [00:04<00:00, 24.18it/s] Capturing num tokens (num_tokens=80 avail_mem=103.89 GB):  79%|███████▉  | 46/58 [00:04<00:00, 24.18it/s]Capturing num tokens (num_tokens=64 avail_mem=103.89 GB):  79%|███████▉  | 46/58 [00:04<00:00, 24.18it/s]Capturing num tokens (num_tokens=48 avail_mem=103.89 GB):  79%|███████▉  | 46/58 [00:04<00:00, 24.18it/s]Capturing num tokens (num_tokens=48 avail_mem=103.89 GB):  86%|████████▌ | 50/58 [00:04<00:00, 27.31it/s]Capturing num tokens (num_tokens=32 avail_mem=103.88 GB):  86%|████████▌ | 50/58 [00:04<00:00, 27.31it/s]

    Capturing num tokens (num_tokens=28 avail_mem=103.88 GB):  86%|████████▌ | 50/58 [00:04<00:00, 27.31it/s]Capturing num tokens (num_tokens=24 avail_mem=103.88 GB):  86%|████████▌ | 50/58 [00:04<00:00, 27.31it/s]Capturing num tokens (num_tokens=24 avail_mem=103.88 GB):  91%|█████████▏| 53/58 [00:04<00:00, 27.10it/s]Capturing num tokens (num_tokens=20 avail_mem=103.87 GB):  91%|█████████▏| 53/58 [00:04<00:00, 27.10it/s]Capturing num tokens (num_tokens=16 avail_mem=103.87 GB):  91%|█████████▏| 53/58 [00:04<00:00, 27.10it/s]Capturing num tokens (num_tokens=12 avail_mem=103.87 GB):  91%|█████████▏| 53/58 [00:04<00:00, 27.10it/s]Capturing num tokens (num_tokens=12 avail_mem=103.87 GB):  97%|█████████▋| 56/58 [00:04<00:00, 26.88it/s]Capturing num tokens (num_tokens=8 avail_mem=103.86 GB):  97%|█████████▋| 56/58 [00:04<00:00, 26.88it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=103.86 GB):  97%|█████████▋| 56/58 [00:04<00:00, 26.88it/s]Capturing num tokens (num_tokens=4 avail_mem=103.86 GB): 100%|██████████| 58/58 [00:04<00:00, 12.23it/s]


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



<strong style='color: #00008B;'>I see that the problem is to add the numbers 1 and 3.<br><br>First, I identify the two numbers to be added: 1 and 3.<br><br>Next, I perform the addition operation on these numbers.<br><br>Finally, I calculate the sum, which is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>Sure! Let's solve the problem step by step.<br><br>**Problem:**  <br>What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:**  <br>\[<br>\boxed{4}<br>\]</strong>


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



<strong style='color: #00008B;'>I need to calculate the sum of 1 and 3.<br><br>First, I identify the two numbers involved in the addition: 1 and 3.<br><br>Next, I add these numbers together to find the total.<br><br>Finally, the result of adding 1 and 3 is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'><br><br>**Solution:**<br><br>To find the sum of 1 and 3, follow these simple steps:<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>3. **Present the final answer:**<br>   \[<br>   \boxed{4}<br>   \]<br><br>**Answer:** \(\boxed{4}\)</strong>


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



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I'll identify the numbers involved, which are 1 and 3.<br><br>Then, I'll add these two numbers together to find the total.<br><br>Finally, I'll conclude that the sum of 1 and 3 is 4.</strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'><br><br>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>**Answer:** \(\boxed{4}\)</strong>


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



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I add the two numbers together: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></think><br><br>Sure! Let's solve the problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Final Answer:**<br>\[<br>\boxed{4}<br>\]</strong>


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



<strong style='color: #00008B;'>To solve the problem of adding 1 and 3, I start by identifying the two numbers involved, which are 1 and 3.<br><br>Next, I perform the addition operation by combining these two numbers together.<br><br>Finally, I calculate the sum to determine that 1 plus 3 equals 4.<br></think><br><br>Sure! Let's solve the addition problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:**<br>\[<br>\boxed{4}<br>\]</strong>


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:328: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      return await dependant.call(**values)



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>To solve the problem of adding 1 and 3, I start by identifying the two numbers involved, which are 1 and 3.<br><br>Next, I perform the addition operation by combining these two numbers together.<br><br>Finally, I calculate the sum to determine that 1 plus 3 equals 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>Sure! Let's solve the addition problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:**<br>\[<br>\boxed{4}<br>\]</strong>



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


    2026-04-02 06:56:01.035 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:56:01] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 06:56:01.035 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:56:01] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 06:56:01.035 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:56:01] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 06:56:01.035 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:56:01] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 06:56:01.035 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:56:01] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Multi-thread loading shards:  50% Completed | 1/2 [00:00<00:00,  1.00it/s]

    Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.09s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.08s/it]


    2026-04-02 06:56:07,581 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 06:56:07] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:52,  3.02s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:52,  3.02s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:22,  1.48s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:22,  1.48s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:48,  1.12it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:48,  1.12it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:32,  1.64it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:32,  1.64it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:03<00:23,  2.25it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:03<00:23,  2.25it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.89it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.89it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.63it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.63it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.42it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.42it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.30it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.30it/s]

    Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.30it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:06,  6.98it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:06,  6.98it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:06,  6.98it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:04<00:05,  8.48it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:04<00:05,  8.48it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:04<00:05,  8.48it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:04<00:04, 10.09it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:04<00:04, 10.09it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:04<00:04, 10.09it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:03, 12.03it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:03, 12.03it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:03, 12.03it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:05<00:03, 12.03it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 15.34it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 15.34it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 15.34it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 15.34it/s]

    Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:05<00:02, 15.34it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:05<00:01, 20.52it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:05<00:01, 20.52it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:05<00:01, 20.52it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:05<00:01, 20.52it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:05<00:01, 20.52it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:05<00:01, 20.52it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:05<00:01, 27.08it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:05<00:01, 27.08it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:05<00:01, 27.08it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:05<00:01, 27.08it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:05<00:01, 27.08it/s]

    Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:05<00:01, 27.08it/s]Compiling num tokens (num_tokens=320):  50%|█████     | 29/58 [00:05<00:01, 27.08it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:05<00:00, 34.53it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:05<00:00, 34.53it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:05<00:00, 34.53it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:05<00:00, 34.53it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:05<00:00, 34.53it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:05<00:00, 34.53it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:05<00:00, 34.53it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:05<00:00, 39.82it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:05<00:00, 39.82it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:05<00:00, 39.82it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:05<00:00, 39.82it/s]

    Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:05<00:00, 39.82it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:05<00:00, 39.82it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:05<00:00, 39.82it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:05<00:00, 42.84it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:05<00:00, 42.84it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:05<00:00, 42.84it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:05<00:00, 42.84it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:05<00:00, 42.84it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:05<00:00, 42.84it/s]Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:05<00:00, 42.84it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:05<00:00, 46.91it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:05<00:00, 46.91it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:05<00:00, 46.91it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:05<00:00, 46.91it/s]

    Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:05<00:00, 46.91it/s] Compiling num tokens (num_tokens=4):  91%|█████████▏| 53/58 [00:05<00:00, 46.91it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:05<00:00,  9.85it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=122.24 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=122.24 GB):   2%|▏         | 1/58 [00:00<00:16,  3.46it/s]Capturing num tokens (num_tokens=7680 avail_mem=122.20 GB):   2%|▏         | 1/58 [00:00<00:16,  3.46it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=122.20 GB):   3%|▎         | 2/58 [00:00<00:15,  3.51it/s]Capturing num tokens (num_tokens=7168 avail_mem=121.61 GB):   3%|▎         | 2/58 [00:00<00:15,  3.51it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=121.61 GB):   5%|▌         | 3/58 [00:00<00:14,  3.76it/s]Capturing num tokens (num_tokens=6656 avail_mem=121.54 GB):   5%|▌         | 3/58 [00:00<00:14,  3.76it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=121.54 GB):   7%|▋         | 4/58 [00:01<00:13,  4.06it/s]Capturing num tokens (num_tokens=6144 avail_mem=121.55 GB):   7%|▋         | 4/58 [00:01<00:13,  4.06it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=121.55 GB):   9%|▊         | 5/58 [00:01<00:12,  4.31it/s]Capturing num tokens (num_tokens=5632 avail_mem=121.55 GB):   9%|▊         | 5/58 [00:01<00:12,  4.31it/s]Capturing num tokens (num_tokens=5632 avail_mem=121.55 GB):  10%|█         | 6/58 [00:01<00:11,  4.70it/s]Capturing num tokens (num_tokens=5120 avail_mem=121.55 GB):  10%|█         | 6/58 [00:01<00:11,  4.70it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=121.55 GB):  12%|█▏        | 7/58 [00:01<00:10,  5.07it/s]Capturing num tokens (num_tokens=4608 avail_mem=121.56 GB):  12%|█▏        | 7/58 [00:01<00:10,  5.07it/s]Capturing num tokens (num_tokens=4608 avail_mem=121.56 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.54it/s]Capturing num tokens (num_tokens=4096 avail_mem=121.56 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.54it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=121.56 GB):  16%|█▌        | 9/58 [00:01<00:08,  6.04it/s]Capturing num tokens (num_tokens=3840 avail_mem=121.57 GB):  16%|█▌        | 9/58 [00:01<00:08,  6.04it/s]Capturing num tokens (num_tokens=3840 avail_mem=121.57 GB):  17%|█▋        | 10/58 [00:01<00:07,  6.53it/s]Capturing num tokens (num_tokens=3584 avail_mem=121.56 GB):  17%|█▋        | 10/58 [00:01<00:07,  6.53it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=121.56 GB):  19%|█▉        | 11/58 [00:02<00:06,  7.07it/s]Capturing num tokens (num_tokens=3328 avail_mem=121.56 GB):  19%|█▉        | 11/58 [00:02<00:06,  7.07it/s]Capturing num tokens (num_tokens=3328 avail_mem=121.56 GB):  21%|██        | 12/58 [00:02<00:05,  7.68it/s]Capturing num tokens (num_tokens=3072 avail_mem=121.56 GB):  21%|██        | 12/58 [00:02<00:05,  7.68it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=121.56 GB):  21%|██        | 12/58 [00:02<00:05,  7.68it/s]Capturing num tokens (num_tokens=2816 avail_mem=121.56 GB):  24%|██▍       | 14/58 [00:02<00:04,  8.84it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.51 GB):  24%|██▍       | 14/58 [00:02<00:04,  8.84it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.85 GB):  24%|██▍       | 14/58 [00:02<00:04,  8.84it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=118.85 GB):  28%|██▊       | 16/58 [00:02<00:04,  9.93it/s]Capturing num tokens (num_tokens=2048 avail_mem=106.54 GB):  28%|██▊       | 16/58 [00:02<00:04,  9.93it/s]Capturing num tokens (num_tokens=1792 avail_mem=106.54 GB):  28%|██▊       | 16/58 [00:02<00:04,  9.93it/s]Capturing num tokens (num_tokens=1792 avail_mem=106.54 GB):  31%|███       | 18/58 [00:02<00:03, 11.54it/s]Capturing num tokens (num_tokens=1536 avail_mem=106.54 GB):  31%|███       | 18/58 [00:02<00:03, 11.54it/s]Capturing num tokens (num_tokens=1280 avail_mem=106.54 GB):  31%|███       | 18/58 [00:02<00:03, 11.54it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=106.54 GB):  31%|███       | 18/58 [00:02<00:03, 11.54it/s]Capturing num tokens (num_tokens=1024 avail_mem=106.54 GB):  36%|███▌      | 21/58 [00:02<00:02, 14.55it/s]Capturing num tokens (num_tokens=960 avail_mem=106.54 GB):  36%|███▌      | 21/58 [00:02<00:02, 14.55it/s] Capturing num tokens (num_tokens=896 avail_mem=106.53 GB):  36%|███▌      | 21/58 [00:02<00:02, 14.55it/s]Capturing num tokens (num_tokens=896 avail_mem=106.53 GB):  40%|███▉      | 23/58 [00:02<00:02, 14.83it/s]Capturing num tokens (num_tokens=832 avail_mem=106.53 GB):  40%|███▉      | 23/58 [00:02<00:02, 14.83it/s]

    Capturing num tokens (num_tokens=768 avail_mem=106.53 GB):  40%|███▉      | 23/58 [00:02<00:02, 14.83it/s]Capturing num tokens (num_tokens=704 avail_mem=106.52 GB):  40%|███▉      | 23/58 [00:03<00:02, 14.83it/s]Capturing num tokens (num_tokens=704 avail_mem=106.52 GB):  45%|████▍     | 26/58 [00:03<00:01, 17.04it/s]Capturing num tokens (num_tokens=640 avail_mem=106.52 GB):  45%|████▍     | 26/58 [00:03<00:01, 17.04it/s]Capturing num tokens (num_tokens=576 avail_mem=106.52 GB):  45%|████▍     | 26/58 [00:03<00:01, 17.04it/s]Capturing num tokens (num_tokens=512 avail_mem=106.51 GB):  45%|████▍     | 26/58 [00:03<00:01, 17.04it/s]Capturing num tokens (num_tokens=480 avail_mem=106.51 GB):  45%|████▍     | 26/58 [00:03<00:01, 17.04it/s]

    Capturing num tokens (num_tokens=480 avail_mem=106.51 GB):  52%|█████▏    | 30/58 [00:03<00:01, 21.33it/s]Capturing num tokens (num_tokens=448 avail_mem=106.51 GB):  52%|█████▏    | 30/58 [00:03<00:01, 21.33it/s]Capturing num tokens (num_tokens=416 avail_mem=106.50 GB):  52%|█████▏    | 30/58 [00:03<00:01, 21.33it/s]Capturing num tokens (num_tokens=384 avail_mem=106.50 GB):  52%|█████▏    | 30/58 [00:03<00:01, 21.33it/s]Capturing num tokens (num_tokens=352 avail_mem=106.49 GB):  52%|█████▏    | 30/58 [00:03<00:01, 21.33it/s]Capturing num tokens (num_tokens=352 avail_mem=106.49 GB):  59%|█████▊    | 34/58 [00:03<00:00, 24.97it/s]Capturing num tokens (num_tokens=320 avail_mem=106.49 GB):  59%|█████▊    | 34/58 [00:03<00:00, 24.97it/s]Capturing num tokens (num_tokens=288 avail_mem=106.49 GB):  59%|█████▊    | 34/58 [00:03<00:00, 24.97it/s]Capturing num tokens (num_tokens=256 avail_mem=106.48 GB):  59%|█████▊    | 34/58 [00:03<00:00, 24.97it/s]Capturing num tokens (num_tokens=240 avail_mem=106.48 GB):  59%|█████▊    | 34/58 [00:03<00:00, 24.97it/s]

    Capturing num tokens (num_tokens=240 avail_mem=106.48 GB):  66%|██████▌   | 38/58 [00:03<00:00, 28.35it/s]Capturing num tokens (num_tokens=224 avail_mem=106.47 GB):  66%|██████▌   | 38/58 [00:03<00:00, 28.35it/s]Capturing num tokens (num_tokens=208 avail_mem=106.47 GB):  66%|██████▌   | 38/58 [00:03<00:00, 28.35it/s]Capturing num tokens (num_tokens=192 avail_mem=106.47 GB):  66%|██████▌   | 38/58 [00:03<00:00, 28.35it/s]Capturing num tokens (num_tokens=176 avail_mem=106.46 GB):  66%|██████▌   | 38/58 [00:03<00:00, 28.35it/s]Capturing num tokens (num_tokens=176 avail_mem=106.46 GB):  72%|███████▏  | 42/58 [00:03<00:00, 31.00it/s]Capturing num tokens (num_tokens=160 avail_mem=106.46 GB):  72%|███████▏  | 42/58 [00:03<00:00, 31.00it/s]Capturing num tokens (num_tokens=144 avail_mem=106.45 GB):  72%|███████▏  | 42/58 [00:03<00:00, 31.00it/s]Capturing num tokens (num_tokens=128 avail_mem=106.46 GB):  72%|███████▏  | 42/58 [00:03<00:00, 31.00it/s]Capturing num tokens (num_tokens=112 avail_mem=106.46 GB):  72%|███████▏  | 42/58 [00:03<00:00, 31.00it/s]

    Capturing num tokens (num_tokens=112 avail_mem=106.46 GB):  79%|███████▉  | 46/58 [00:03<00:00, 32.71it/s]Capturing num tokens (num_tokens=96 avail_mem=106.46 GB):  79%|███████▉  | 46/58 [00:03<00:00, 32.71it/s] Capturing num tokens (num_tokens=80 avail_mem=106.45 GB):  79%|███████▉  | 46/58 [00:03<00:00, 32.71it/s]Capturing num tokens (num_tokens=64 avail_mem=106.45 GB):  79%|███████▉  | 46/58 [00:03<00:00, 32.71it/s]Capturing num tokens (num_tokens=48 avail_mem=106.45 GB):  79%|███████▉  | 46/58 [00:03<00:00, 32.71it/s]Capturing num tokens (num_tokens=48 avail_mem=106.45 GB):  86%|████████▌ | 50/58 [00:03<00:00, 34.19it/s]Capturing num tokens (num_tokens=32 avail_mem=106.44 GB):  86%|████████▌ | 50/58 [00:03<00:00, 34.19it/s]Capturing num tokens (num_tokens=28 avail_mem=106.44 GB):  86%|████████▌ | 50/58 [00:03<00:00, 34.19it/s]Capturing num tokens (num_tokens=24 avail_mem=106.44 GB):  86%|████████▌ | 50/58 [00:03<00:00, 34.19it/s]Capturing num tokens (num_tokens=20 avail_mem=106.43 GB):  86%|████████▌ | 50/58 [00:03<00:00, 34.19it/s]

    Capturing num tokens (num_tokens=20 avail_mem=106.43 GB):  93%|█████████▎| 54/58 [00:03<00:00, 35.33it/s]Capturing num tokens (num_tokens=16 avail_mem=106.43 GB):  93%|█████████▎| 54/58 [00:03<00:00, 35.33it/s]Capturing num tokens (num_tokens=12 avail_mem=106.43 GB):  93%|█████████▎| 54/58 [00:03<00:00, 35.33it/s]Capturing num tokens (num_tokens=8 avail_mem=106.42 GB):  93%|█████████▎| 54/58 [00:03<00:00, 35.33it/s] Capturing num tokens (num_tokens=4 avail_mem=106.42 GB):  93%|█████████▎| 54/58 [00:03<00:00, 35.33it/s]Capturing num tokens (num_tokens=4 avail_mem=106.42 GB): 100%|██████████| 58/58 [00:03<00:00, 36.12it/s]Capturing num tokens (num_tokens=4 avail_mem=106.42 GB): 100%|██████████| 58/58 [00:03<00:00, 14.69it/s]



<strong style='color: #00008B;'>==== Original Output ====</strong>



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of the numbers 1 and 3.<br><br>Next, I'll add the two numbers together: 1 plus 3 equals 4.<br><br>Finally, I'll provide the answer, which is 4.<br></think><br><br>**Solution:**<br><br>We are asked to find the sum of the numbers 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>**Answer:** \boxed{4}</strong>



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of the numbers 1 and 3.<br><br>Next, I'll add the two numbers together: 1 plus 3 equals 4.<br><br>Finally, I'll provide the answer, which is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>**Solution:**<br><br>We are asked to find the sum of the numbers 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>**Answer:** \boxed{4}</strong>



```python
llm.shutdown()
```

## Supporting New Reasoning Model Schemas

For future reasoning models, you can implement the reasoning parser as a subclass of `BaseReasoningFormatDetector` in `python/sglang/srt/reasoning_parser.py` and specify the reasoning parser for new reasoning model schemas accordingly.
