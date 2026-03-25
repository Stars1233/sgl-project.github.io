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

    [2026-03-25 06:00:04] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.


    [2026-03-25 06:00:04] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.


    [2026-03-25 06:00:04] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [2026-03-25 06:00:08] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-25 06:00:08] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-25 06:00:08] INFO utils.py:164: NumExpr defaulting to 16 threads.
    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    [2026-03-25 06:00:10] INFO server_args.py:2246: Attention backend not specified. Use fa3 backend by default.
    [2026-03-25 06:00:10] INFO server_args.py:3535: Set soft_watchdog_timeout since in CI


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:175: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [2026-03-25 06:00:16] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-25 06:00:16] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-25 06:00:16] INFO utils.py:164: NumExpr defaulting to 16 threads.
    [2026-03-25 06:00:16] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-25 06:00:16] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-25 06:00:16] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-03-25 06:00:21.252 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 06:00:21] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 06:00:21.252 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 06:00:21] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 06:00:21.252 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 06:00:21] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 06:00:21.252 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 06:00:21] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 06:00:21.252 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 06:00:21] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:00<00:00,  1.22it/s]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.13s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.08s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:54,  3.07s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:54,  3.07s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:23,  1.50s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:23,  1.50s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:49,  1.11it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:49,  1.11it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:33,  1.61it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:33,  1.61it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:27,  1.90it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:27,  1.90it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:24,  2.15it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:24,  2.15it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:20,  2.45it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:20,  2.45it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:05<00:18,  2.77it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:05<00:18,  2.77it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:05<00:15,  3.11it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:05<00:15,  3.11it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:05<00:13,  3.49it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:05<00:13,  3.49it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:05<00:12,  3.80it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:05<00:12,  3.80it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:05<00:11,  4.17it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:05<00:11,  4.17it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:06<00:09,  4.54it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:06<00:09,  4.54it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:06<00:09,  4.80it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:06<00:09,  4.80it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:06<00:08,  4.84it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:06<00:08,  4.84it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:06<00:08,  5.08it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:06<00:08,  5.08it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:06<00:07,  5.53it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:06<00:07,  5.53it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:06<00:06,  6.30it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:06<00:06,  6.30it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:07<00:06,  6.30it/s]

    Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:07<00:04,  7.98it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:07<00:04,  7.98it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:07<00:04,  7.98it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:07<00:03,  9.86it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:07<00:03,  9.86it/s]

    Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:07<00:03,  9.86it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:07<00:03, 11.29it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:07<00:03, 11.29it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:07<00:03, 11.29it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:07<00:02, 13.22it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:07<00:02, 13.22it/s]

    Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:07<00:02, 13.22it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:07<00:02, 14.65it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:07<00:02, 14.65it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:07<00:02, 14.65it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:07<00:02, 14.65it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:07<00:01, 17.33it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:07<00:01, 17.33it/s]

    Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:07<00:01, 17.33it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:07<00:01, 17.33it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:07<00:01, 19.38it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:07<00:01, 19.38it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:07<00:01, 19.38it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:07<00:01, 19.38it/s]

    Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:07<00:01, 20.37it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:07<00:01, 20.37it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:07<00:01, 20.37it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:08<00:01, 20.37it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:08<00:00, 22.18it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:08<00:00, 22.18it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:08<00:00, 22.18it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:08<00:00, 22.18it/s]

    Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:08<00:00, 23.06it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:08<00:00, 23.06it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:08<00:00, 23.06it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:08<00:00, 23.06it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:08<00:00, 24.47it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:08<00:00, 24.47it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:08<00:00, 24.47it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:08<00:00, 24.47it/s]

    Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:08<00:00, 25.30it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:08<00:00, 25.30it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:08<00:00, 25.30it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:08<00:00, 25.30it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:08<00:00, 25.30it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:08<00:00, 27.43it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:08<00:00, 27.43it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:08<00:00, 27.43it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:08<00:00, 27.43it/s]

    Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:08<00:00, 27.43it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:08<00:00, 30.30it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:08<00:00, 30.30it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:08<00:00,  6.71it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=93.05 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=93.05 GB):   2%|▏         | 1/58 [00:00<00:23,  2.38it/s]Capturing num tokens (num_tokens=7680 avail_mem=93.01 GB):   2%|▏         | 1/58 [00:00<00:23,  2.38it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=93.01 GB):   3%|▎         | 2/58 [00:00<00:22,  2.52it/s]Capturing num tokens (num_tokens=7168 avail_mem=93.01 GB):   3%|▎         | 2/58 [00:00<00:22,  2.52it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=93.01 GB):   5%|▌         | 3/58 [00:01<00:21,  2.58it/s]Capturing num tokens (num_tokens=6656 avail_mem=93.01 GB):   5%|▌         | 3/58 [00:01<00:21,  2.58it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=93.01 GB):   7%|▋         | 4/58 [00:01<00:27,  1.99it/s]Capturing num tokens (num_tokens=6144 avail_mem=91.98 GB):   7%|▋         | 4/58 [00:01<00:27,  1.99it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=91.98 GB):   9%|▊         | 5/58 [00:02<00:26,  2.01it/s]Capturing num tokens (num_tokens=5632 avail_mem=92.17 GB):   9%|▊         | 5/58 [00:02<00:26,  2.01it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=92.17 GB):  10%|█         | 6/58 [00:02<00:23,  2.19it/s]Capturing num tokens (num_tokens=5120 avail_mem=92.99 GB):  10%|█         | 6/58 [00:02<00:23,  2.19it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=92.99 GB):  12%|█▏        | 7/58 [00:03<00:21,  2.41it/s]Capturing num tokens (num_tokens=4608 avail_mem=92.23 GB):  12%|█▏        | 7/58 [00:03<00:21,  2.41it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=92.23 GB):  14%|█▍        | 8/58 [00:03<00:19,  2.59it/s]Capturing num tokens (num_tokens=4096 avail_mem=93.01 GB):  14%|█▍        | 8/58 [00:03<00:19,  2.59it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=93.01 GB):  16%|█▌        | 9/58 [00:03<00:17,  2.80it/s]Capturing num tokens (num_tokens=3840 avail_mem=92.30 GB):  16%|█▌        | 9/58 [00:03<00:17,  2.80it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=92.30 GB):  17%|█▋        | 10/58 [00:03<00:15,  3.01it/s]Capturing num tokens (num_tokens=3584 avail_mem=92.36 GB):  17%|█▋        | 10/58 [00:03<00:15,  3.01it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=92.36 GB):  19%|█▉        | 11/58 [00:04<00:14,  3.18it/s]Capturing num tokens (num_tokens=3328 avail_mem=93.02 GB):  19%|█▉        | 11/58 [00:04<00:14,  3.18it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=93.02 GB):  21%|██        | 12/58 [00:04<00:13,  3.34it/s]Capturing num tokens (num_tokens=3072 avail_mem=92.41 GB):  21%|██        | 12/58 [00:04<00:13,  3.34it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=92.41 GB):  22%|██▏       | 13/58 [00:04<00:12,  3.63it/s]Capturing num tokens (num_tokens=2816 avail_mem=92.47 GB):  22%|██▏       | 13/58 [00:04<00:12,  3.63it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=92.47 GB):  24%|██▍       | 14/58 [00:04<00:11,  3.90it/s]Capturing num tokens (num_tokens=2560 avail_mem=93.03 GB):  24%|██▍       | 14/58 [00:04<00:11,  3.90it/s]Capturing num tokens (num_tokens=2560 avail_mem=93.03 GB):  26%|██▌       | 15/58 [00:05<00:10,  4.29it/s]Capturing num tokens (num_tokens=2304 avail_mem=92.53 GB):  26%|██▌       | 15/58 [00:05<00:10,  4.29it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=92.53 GB):  28%|██▊       | 16/58 [00:05<00:08,  4.76it/s]Capturing num tokens (num_tokens=2048 avail_mem=92.92 GB):  28%|██▊       | 16/58 [00:05<00:08,  4.76it/s]Capturing num tokens (num_tokens=2048 avail_mem=92.92 GB):  29%|██▉       | 17/58 [00:05<00:08,  5.03it/s]Capturing num tokens (num_tokens=1792 avail_mem=93.03 GB):  29%|██▉       | 17/58 [00:05<00:08,  5.03it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=93.03 GB):  31%|███       | 18/58 [00:05<00:07,  5.62it/s]Capturing num tokens (num_tokens=1536 avail_mem=92.42 GB):  31%|███       | 18/58 [00:05<00:07,  5.62it/s]Capturing num tokens (num_tokens=1536 avail_mem=92.42 GB):  33%|███▎      | 19/58 [00:05<00:06,  5.94it/s]Capturing num tokens (num_tokens=1280 avail_mem=92.58 GB):  33%|███▎      | 19/58 [00:05<00:06,  5.94it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=92.58 GB):  34%|███▍      | 20/58 [00:05<00:05,  6.51it/s]Capturing num tokens (num_tokens=1024 avail_mem=91.96 GB):  34%|███▍      | 20/58 [00:05<00:05,  6.51it/s]Capturing num tokens (num_tokens=960 avail_mem=92.14 GB):  34%|███▍      | 20/58 [00:05<00:05,  6.51it/s] 

    Capturing num tokens (num_tokens=960 avail_mem=92.14 GB):  38%|███▊      | 22/58 [00:06<00:04,  7.52it/s]Capturing num tokens (num_tokens=896 avail_mem=91.53 GB):  38%|███▊      | 22/58 [00:06<00:04,  7.52it/s]Capturing num tokens (num_tokens=832 avail_mem=91.72 GB):  38%|███▊      | 22/58 [00:06<00:04,  7.52it/s]

    Capturing num tokens (num_tokens=832 avail_mem=91.72 GB):  41%|████▏     | 24/58 [00:06<00:04,  8.28it/s]Capturing num tokens (num_tokens=768 avail_mem=91.38 GB):  41%|████▏     | 24/58 [00:06<00:04,  8.28it/s]Capturing num tokens (num_tokens=704 avail_mem=91.70 GB):  41%|████▏     | 24/58 [00:06<00:04,  8.28it/s]Capturing num tokens (num_tokens=704 avail_mem=91.70 GB):  45%|████▍     | 26/58 [00:06<00:03,  9.26it/s]Capturing num tokens (num_tokens=640 avail_mem=91.70 GB):  45%|████▍     | 26/58 [00:06<00:03,  9.26it/s]

    Capturing num tokens (num_tokens=576 avail_mem=91.40 GB):  45%|████▍     | 26/58 [00:06<00:03,  9.26it/s]Capturing num tokens (num_tokens=576 avail_mem=91.40 GB):  48%|████▊     | 28/58 [00:06<00:02, 10.29it/s]Capturing num tokens (num_tokens=512 avail_mem=91.57 GB):  48%|████▊     | 28/58 [00:06<00:02, 10.29it/s]Capturing num tokens (num_tokens=480 avail_mem=90.35 GB):  48%|████▊     | 28/58 [00:06<00:02, 10.29it/s]

    Capturing num tokens (num_tokens=480 avail_mem=90.35 GB):  52%|█████▏    | 30/58 [00:06<00:02, 11.03it/s]Capturing num tokens (num_tokens=448 avail_mem=89.14 GB):  52%|█████▏    | 30/58 [00:06<00:02, 11.03it/s]Capturing num tokens (num_tokens=416 avail_mem=89.14 GB):  52%|█████▏    | 30/58 [00:06<00:02, 11.03it/s]Capturing num tokens (num_tokens=416 avail_mem=89.14 GB):  55%|█████▌    | 32/58 [00:06<00:02, 12.27it/s]Capturing num tokens (num_tokens=384 avail_mem=88.94 GB):  55%|█████▌    | 32/58 [00:06<00:02, 12.27it/s]Capturing num tokens (num_tokens=352 avail_mem=89.13 GB):  55%|█████▌    | 32/58 [00:06<00:02, 12.27it/s]

    Capturing num tokens (num_tokens=352 avail_mem=89.13 GB):  59%|█████▊    | 34/58 [00:06<00:01, 13.05it/s]Capturing num tokens (num_tokens=320 avail_mem=89.13 GB):  59%|█████▊    | 34/58 [00:06<00:01, 13.05it/s]Capturing num tokens (num_tokens=288 avail_mem=88.98 GB):  59%|█████▊    | 34/58 [00:07<00:01, 13.05it/s]Capturing num tokens (num_tokens=288 avail_mem=88.98 GB):  62%|██████▏   | 36/58 [00:07<00:01, 14.56it/s]Capturing num tokens (num_tokens=256 avail_mem=89.12 GB):  62%|██████▏   | 36/58 [00:07<00:01, 14.56it/s]Capturing num tokens (num_tokens=240 avail_mem=89.11 GB):  62%|██████▏   | 36/58 [00:07<00:01, 14.56it/s]

    Capturing num tokens (num_tokens=224 avail_mem=89.11 GB):  62%|██████▏   | 36/58 [00:07<00:01, 14.56it/s]Capturing num tokens (num_tokens=224 avail_mem=89.11 GB):  67%|██████▋   | 39/58 [00:07<00:01, 16.65it/s]Capturing num tokens (num_tokens=208 avail_mem=88.99 GB):  67%|██████▋   | 39/58 [00:07<00:01, 16.65it/s]Capturing num tokens (num_tokens=192 avail_mem=89.09 GB):  67%|██████▋   | 39/58 [00:07<00:01, 16.65it/s]Capturing num tokens (num_tokens=176 avail_mem=89.08 GB):  67%|██████▋   | 39/58 [00:07<00:01, 16.65it/s]Capturing num tokens (num_tokens=176 avail_mem=89.08 GB):  72%|███████▏  | 42/58 [00:07<00:00, 18.35it/s]Capturing num tokens (num_tokens=160 avail_mem=89.08 GB):  72%|███████▏  | 42/58 [00:07<00:00, 18.35it/s]

    Capturing num tokens (num_tokens=144 avail_mem=89.07 GB):  72%|███████▏  | 42/58 [00:07<00:00, 18.35it/s]Capturing num tokens (num_tokens=128 avail_mem=89.08 GB):  72%|███████▏  | 42/58 [00:07<00:00, 18.35it/s]Capturing num tokens (num_tokens=128 avail_mem=89.08 GB):  78%|███████▊  | 45/58 [00:07<00:00, 19.48it/s]Capturing num tokens (num_tokens=112 avail_mem=89.02 GB):  78%|███████▊  | 45/58 [00:07<00:00, 19.48it/s]Capturing num tokens (num_tokens=96 avail_mem=89.01 GB):  78%|███████▊  | 45/58 [00:07<00:00, 19.48it/s] Capturing num tokens (num_tokens=80 avail_mem=89.05 GB):  78%|███████▊  | 45/58 [00:07<00:00, 19.48it/s]Capturing num tokens (num_tokens=80 avail_mem=89.05 GB):  83%|████████▎ | 48/58 [00:07<00:00, 21.53it/s]Capturing num tokens (num_tokens=64 avail_mem=89.04 GB):  83%|████████▎ | 48/58 [00:07<00:00, 21.53it/s]

    Capturing num tokens (num_tokens=48 avail_mem=89.04 GB):  83%|████████▎ | 48/58 [00:07<00:00, 21.53it/s]Capturing num tokens (num_tokens=32 avail_mem=89.03 GB):  83%|████████▎ | 48/58 [00:07<00:00, 21.53it/s]Capturing num tokens (num_tokens=32 avail_mem=89.03 GB):  88%|████████▊ | 51/58 [00:07<00:00, 22.51it/s]Capturing num tokens (num_tokens=28 avail_mem=89.02 GB):  88%|████████▊ | 51/58 [00:07<00:00, 22.51it/s]Capturing num tokens (num_tokens=24 avail_mem=89.02 GB):  88%|████████▊ | 51/58 [00:07<00:00, 22.51it/s]Capturing num tokens (num_tokens=20 avail_mem=89.01 GB):  88%|████████▊ | 51/58 [00:07<00:00, 22.51it/s]Capturing num tokens (num_tokens=20 avail_mem=89.01 GB):  93%|█████████▎| 54/58 [00:07<00:00, 23.60it/s]Capturing num tokens (num_tokens=16 avail_mem=89.00 GB):  93%|█████████▎| 54/58 [00:07<00:00, 23.60it/s]

    Capturing num tokens (num_tokens=12 avail_mem=88.99 GB):  93%|█████████▎| 54/58 [00:07<00:00, 23.60it/s]Capturing num tokens (num_tokens=8 avail_mem=88.98 GB):  93%|█████████▎| 54/58 [00:07<00:00, 23.60it/s] Capturing num tokens (num_tokens=8 avail_mem=88.98 GB):  98%|█████████▊| 57/58 [00:07<00:00, 24.80it/s]Capturing num tokens (num_tokens=4 avail_mem=88.98 GB):  98%|█████████▊| 57/58 [00:07<00:00, 24.80it/s]Capturing num tokens (num_tokens=4 avail_mem=88.98 GB): 100%|██████████| 58/58 [00:07<00:00,  7.28it/s]


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



<strong style='color: #00008B;'>Certainly! Let's solve the addition problem step by step.<br><br>**Problem:**  <br>What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**  <br>   We have the numbers 1 and 3.<br><br>2. **Add the numbers together:**  <br>   \[<br>   1 + 3 = 4<br>   \]<br><br>3. **Final Answer:**  <br>   \[<br>   \boxed{4}<br>   \]<br><br>So, \(1 + 3 = \boxed{4}\).</strong>


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



<strong style='color: #00008B;'>To solve the problem of adding 1 and 3, I start by identifying the two numbers involved.<br><br>Next, I perform the addition operation by combining these two numbers.<br><br>Finally, I calculate the sum to find the result of 1 plus 3.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'><br><br>**Solution:**<br><br>To find the sum of 1 and 3, follow these steps:<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Perform the addition:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>3. **Present the final answer:**<br>   \[<br>   \boxed{4}<br>   \]<br><br>**Answer:** \(\boxed{4}\)</strong>


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



<strong style='color: #00008B;'>I need to add the numbers 1 and 3 together.<br><br>First, I'll identify the two numbers: 1 and 3.<br><br>Next, I'll perform the addition: 1 plus 3 equals 4.<br><br>Therefore, the sum of 1 and 3 is 4.</strong>



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



<strong style='color: #00008B;'>First, I recognize that the user is asking for the sum of 1 and 3.<br><br>To find the result, I add the two numbers together: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></think><br><br>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>**Final Answer:** \boxed{4}</strong>


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



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I add the numbers together: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></think><br><br>Certainly! Let's solve the problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Final Answer:**<br>\[<br>\boxed{4}<br>\]</strong>


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:328: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      return await dependant.call(**values)



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I add the numbers together: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>Certainly! Let's solve the problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Final Answer:**<br>\[<br>\boxed{4}<br>\]</strong>



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

    [2026-03-25 06:01:12] INFO server_args.py:2246: Attention backend not specified. Use fa3 backend by default.


    [2026-03-25 06:01:12] INFO server_args.py:3535: Set soft_watchdog_timeout since in CI


    [2026-03-25 06:01:12] INFO engine.py:177: server_args=ServerArgs(model_path='deepseek-ai/DeepSeek-R1-Distill-Qwen-7B', tokenizer_path='deepseek-ai/DeepSeek-R1-Distill-Qwen-7B', tokenizer_mode='auto', tokenizer_worker_num=1, skip_tokenizer_init=False, load_format='auto', model_loader_extra_config='{}', trust_remote_code=False, context_length=None, is_embedding=False, enable_multimodal=None, revision=None, model_impl='auto', host='127.0.0.1', port=30000, fastapi_root_path='', grpc_mode=False, skip_server_warmup=False, warmups=None, nccl_port=None, checkpoint_engine_wait_weights_before_ready=False, ssl_keyfile=None, ssl_certfile=None, ssl_ca_certs=None, ssl_keyfile_password=None, enable_ssl_refresh=False, dtype='auto', quantization=None, quantization_param_path=None, kv_cache_dtype='auto', enable_fp32_lm_head=False, modelopt_quant=None, modelopt_checkpoint_restore_path=None, modelopt_checkpoint_save_path=None, modelopt_export_path=None, quantize_and_serve=False, rl_quant_profile=None, mem_fraction_static=0.903, max_running_requests=128, max_queued_requests=None, max_total_tokens=20480, chunked_prefill_size=8192, enable_dynamic_chunking=False, max_prefill_tokens=16384, prefill_max_requests=None, schedule_policy='fcfs', enable_priority_scheduling=False, disable_priority_preemption=False, default_priority_value=None, abort_on_priority_when_disabled=False, schedule_low_priority_values_first=False, priority_scheduling_preemption_threshold=10, schedule_conservativeness=1.0, page_size=1, swa_full_tokens_ratio=0.8, disable_hybrid_swa_memory=False, radix_eviction_policy='lru', enable_prefill_delayer=False, prefill_delayer_max_delay_passes=30, prefill_delayer_token_usage_low_watermark=None, prefill_delayer_forward_passes_buckets=None, prefill_delayer_wait_seconds_buckets=None, device='cuda', tp_size=1, pp_size=1, pp_max_micro_batch_size=None, pp_async_batch_depth=0, stream_interval=1, incremental_streaming_output=False, enable_streaming_session=False, random_seed=641391710, constrained_json_whitespace_pattern=None, constrained_json_disable_any_whitespace=False, watchdog_timeout=300, soft_watchdog_timeout=300, dist_timeout=None, download_dir=None, model_checksum=None, base_gpu_id=0, gpu_id_step=1, sleep_on_idle=False, use_ray=False, custom_sigquit_handler=None, log_level='error', log_level_http=None, log_requests=False, log_requests_level=2, log_requests_format='text', log_requests_target=None, uvicorn_access_log_exclude_prefixes=[], crash_dump_folder=None, show_time_cost=False, enable_metrics=False, enable_metrics_for_all_schedulers=False, tokenizer_metrics_custom_labels_header='x-custom-labels', tokenizer_metrics_allowed_custom_labels=None, extra_metric_labels=None, bucket_time_to_first_token=None, bucket_inter_token_latency=None, bucket_e2e_request_latency=None, collect_tokens_histogram=False, prompt_tokens_buckets=None, generation_tokens_buckets=None, gc_warning_threshold_secs=0.0, decode_log_interval=40, enable_request_time_stats_logging=False, kv_events_config=None, enable_trace=False, otlp_traces_endpoint='localhost:4317', export_metrics_to_file=False, export_metrics_to_file_dir=None, api_key=None, admin_api_key=None, served_model_name='deepseek-ai/DeepSeek-R1-Distill-Qwen-7B', weight_version='default', chat_template=None, hf_chat_template_name=None, completion_template=None, file_storage_path='sglang_storage', enable_cache_report=False, reasoning_parser=None, tool_call_parser=None, tool_server=None, sampling_defaults='model', dp_size=1, load_balance_method='round_robin', attn_cp_size=1, moe_dp_size=1, dist_init_addr=None, nnodes=1, node_rank=0, json_model_override_args='{}', preferred_sampling_params=None, enable_lora=None, enable_lora_overlap_loading=None, max_lora_rank=None, lora_target_modules=None, lora_paths=None, max_loaded_loras=None, max_loras_per_batch=8, lora_eviction_policy='lru', lora_backend='csgmv', max_lora_chunk_size=16, attention_backend='fa3', decode_attention_backend=None, prefill_attention_backend=None, sampling_backend='flashinfer', grammar_backend='xgrammar', mm_attention_backend=None, fp8_gemm_runner_backend='auto', fp4_gemm_runner_backend='auto', nsa_prefill_backend=None, nsa_decode_backend=None, disable_flashinfer_autotune=False, mamba_backend='triton', speculative_algorithm=None, speculative_draft_model_path=None, speculative_draft_model_revision=None, speculative_draft_load_format=None, speculative_num_steps=None, speculative_eagle_topk=None, speculative_num_draft_tokens=None, speculative_accept_threshold_single=1.0, speculative_accept_threshold_acc=1.0, speculative_token_map=None, speculative_attention_mode='prefill', speculative_draft_attention_backend=None, speculative_moe_runner_backend='auto', speculative_moe_a2a_backend=None, speculative_draft_model_quantization=None, speculative_ngram_min_match_window_size=1, speculative_ngram_max_match_window_size=12, speculative_ngram_min_bfs_breadth=1, speculative_ngram_max_bfs_breadth=10, speculative_ngram_match_type='BFS', speculative_ngram_max_trie_depth=18, speculative_ngram_capacity=10000000, enable_multi_layer_eagle=False, ep_size=1, moe_a2a_backend='none', moe_runner_backend='auto', flashinfer_mxfp4_moe_precision='default', enable_flashinfer_allreduce_fusion=False, enable_aiter_allreduce_fusion=False, deepep_mode='auto', ep_num_redundant_experts=0, ep_dispatch_algorithm=None, init_expert_location='trivial', enable_eplb=False, eplb_algorithm='auto', eplb_rebalance_num_iterations=1000, eplb_rebalance_layers_per_chunk=None, eplb_min_rebalancing_utilization_threshold=1.0, expert_distribution_recorder_mode=None, expert_distribution_recorder_buffer_size=1000, enable_expert_distribution_metrics=False, deepep_config=None, moe_dense_tp_size=None, elastic_ep_backend=None, enable_elastic_expert_backup=False, mooncake_ib_device=None, max_mamba_cache_size=None, mamba_ssm_dtype=None, mamba_full_memory_ratio=0.9, mamba_scheduler_strategy='no_buffer', mamba_track_interval=256, linear_attn_backend='triton', linear_attn_decode_backend=None, linear_attn_prefill_backend=None, enable_hierarchical_cache=False, hicache_ratio=2.0, hicache_size=0, hicache_write_policy='write_through', hicache_io_backend='kernel', hicache_mem_layout='layer_first', disable_hicache_numa_detect=False, hicache_storage_backend=None, hicache_storage_prefetch_policy='best_effort', hicache_storage_backend_extra_config=None, enable_hisparse=False, hisparse_config=None, enable_lmcache=False, kt_weight_path=None, kt_method=None, kt_cpuinfer=None, kt_threadpool_count=None, kt_num_gpu_experts=None, kt_max_deferred_experts_per_token=None, dllm_algorithm=None, dllm_algorithm_config=None, enable_double_sparsity=False, ds_channel_config_path=None, ds_heavy_channel_num=32, ds_heavy_token_num=256, ds_heavy_channel_type='qk', ds_sparse_decode_threshold=4096, cpu_offload_gb=0, offload_group_size=-1, offload_num_in_group=1, offload_prefetch_step=1, offload_mode='cpu', multi_item_scoring_delimiter=None, disable_radix_cache=False, cuda_graph_max_bs=4, cuda_graph_bs=[1, 2, 4, 8, 12, 16, 24, 32, 40, 48, 56, 64, 72, 80, 88, 96, 104, 112, 120, 128, 136, 144, 152, 160, 168, 176, 184, 192, 200, 208, 216, 224, 232, 240, 248, 256], disable_cuda_graph=True, disable_cuda_graph_padding=False, enable_profile_cuda_graph=False, enable_cudagraph_gc=False, enable_layerwise_nvtx_marker=False, enable_nccl_nvls=False, enable_symm_mem=False, disable_flashinfer_cutlass_moe_fp4_allgather=False, enable_tokenizer_batch_encode=False, disable_tokenizer_batch_decode=False, disable_outlines_disk_cache=False, disable_custom_all_reduce=False, enable_mscclpp=False, enable_torch_symm_mem=False, pre_warm_nccl=False, disable_overlap_schedule=False, enable_mixed_chunk=False, enable_dp_attention=False, enable_dp_lm_head=False, enable_two_batch_overlap=False, enable_single_batch_overlap=False, tbo_token_distribution_threshold=0.48, enable_torch_compile=False, disable_piecewise_cuda_graph=False, enforce_piecewise_cuda_graph=False, enable_torch_compile_debug_mode=False, torch_compile_max_bs=32, piecewise_cuda_graph_max_tokens=8192, piecewise_cuda_graph_tokens=[4, 8, 12, 16, 20, 24, 28, 32, 48, 64, 80, 96, 112, 128, 144, 160, 176, 192, 208, 224, 240, 256, 288, 320, 352, 384, 416, 448, 480, 512, 576, 640, 704, 768, 832, 896, 960, 1024, 1280, 1536, 1792, 2048, 2304, 2560, 2816, 3072, 3328, 3584, 3840, 4096, 4608, 5120, 5632, 6144, 6656, 7168, 7680, 8192], piecewise_cuda_graph_compiler='eager', torchao_config='', enable_nan_detection=False, enable_p2p_check=False, triton_attention_reduce_in_fp32=False, triton_attention_num_kv_splits=8, triton_attention_split_tile_size=None, num_continuous_decode_steps=1, delete_ckpt_after_loading=False, enable_memory_saver=False, enable_weights_cpu_backup=False, enable_draft_weights_cpu_backup=False, allow_auto_truncate=False, enable_custom_logit_processor=False, flashinfer_mla_disable_ragged=False, disable_shared_experts_fusion=False, disable_chunked_prefix_cache=False, disable_fast_image_processor=False, keep_mm_feature_on_device=False, enable_return_hidden_states=False, enable_return_routed_experts=False, scheduler_recv_interval=1, numa_node=None, enable_deterministic_inference=False, rl_on_policy_target=None, enable_attn_tp_input_scattered=False, enable_nsa_prefill_context_parallel=False, nsa_prefill_cp_mode='round-robin-split', enable_fused_qk_norm_rope=False, enable_precise_embedding_interpolation=False, enable_fused_moe_sum_all_reduce=False, enable_prefill_context_parallel=False, prefill_cp_mode='in-seq-split', enable_dynamic_batch_tokenizer=False, dynamic_batch_tokenizer_batch_size=32, dynamic_batch_tokenizer_batch_timeout=0.002, debug_tensor_dump_output_folder=None, debug_tensor_dump_layers=None, debug_tensor_dump_input_file=None, debug_tensor_dump_inject=False, disaggregation_mode='null', disaggregation_transfer_backend='mooncake', disaggregation_bootstrap_port=8998, disaggregation_ib_device=None, disaggregation_decode_enable_offload_kvcache=False, num_reserved_decode_tokens=512, disaggregation_decode_polling_interval=1, encoder_only=False, language_only=False, encoder_transfer_backend='zmq_to_scheduler', encoder_urls=[], enable_adaptive_dispatch_to_encoder=False, custom_weight_loader=[], weight_loader_disable_mmap=False, remote_instance_weight_loader_seed_instance_ip=None, remote_instance_weight_loader_seed_instance_service_port=None, remote_instance_weight_loader_send_weights_group_ports=None, remote_instance_weight_loader_backend='nccl', remote_instance_weight_loader_start_seed_via_transfer_engine=False, modelexpress_config=None, enable_pdmux=False, pdmux_config_path=None, sm_group_num=8, mm_max_concurrent_calls=32, mm_per_request_timeout=10.0, enable_broadcast_mm_inputs_process=False, enable_prefix_mm_cache=False, mm_enable_dp_encoder=False, mm_process_config={}, limit_mm_data_per_request=None, enable_mm_global_cache=False, decrypted_config_file=None, decrypted_draft_config_file=None, forward_hooks=None)


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-03-25 06:01:22.276 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 06:01:22] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 06:01:22.276 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 06:01:22] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 06:01:22.276 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 06:01:22] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 06:01:22.276 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 06:01:22] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 06:01:22.276 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 06:01:22] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:00<00:00,  1.21it/s]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.14s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.09s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:53,  3.04s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:53,  3.04s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:23,  1.49s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:23,  1.49s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:49,  1.12it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:49,  1.12it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:33,  1.63it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:33,  1.63it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:03<00:23,  2.23it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:03<00:23,  2.23it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.86it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.86it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.60it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.60it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:12,  3.90it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:12,  3.90it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:12,  3.99it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:12,  3.99it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:04<00:11,  4.18it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:04<00:11,  4.18it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:05<00:12,  3.85it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:05<00:12,  3.85it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:05<00:12,  3.72it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:05<00:12,  3.72it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:05<00:11,  3.81it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:05<00:11,  3.81it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:06<00:11,  3.86it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:06<00:11,  3.86it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:06<00:10,  4.11it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:06<00:10,  4.11it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:06<00:09,  4.41it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:06<00:09,  4.41it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:06<00:08,  4.68it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:06<00:08,  4.68it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:06<00:07,  5.03it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:06<00:07,  5.03it/s]

    Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:06<00:06,  5.64it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:06<00:06,  5.64it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:07<00:06,  6.05it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:07<00:06,  6.05it/s]

    Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:07<00:05,  6.63it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:07<00:05,  6.63it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:07<00:05,  6.63it/s]

    Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:07<00:04,  7.82it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:07<00:04,  7.82it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:07<00:04,  7.82it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:07<00:03,  8.91it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:07<00:03,  8.91it/s]

    Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:07<00:03,  8.91it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:07<00:03,  9.98it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:07<00:03,  9.98it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:07<00:03,  9.98it/s]

    Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:07<00:02, 11.10it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:07<00:02, 11.10it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:07<00:02, 11.10it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:07<00:02, 12.38it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:07<00:02, 12.38it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:08<00:02, 12.38it/s]

    Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:08<00:01, 13.04it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:08<00:01, 13.04it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:08<00:01, 13.04it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:08<00:01, 13.91it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:08<00:01, 13.91it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:08<00:01, 13.91it/s]

    Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:08<00:01, 14.48it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:08<00:01, 14.48it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:08<00:01, 14.48it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:08<00:01, 15.59it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:08<00:01, 15.59it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:08<00:01, 15.59it/s]

    Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:08<00:01, 16.27it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:08<00:01, 16.27it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:08<00:01, 16.27it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:08<00:00, 16.70it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:08<00:00, 16.70it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:08<00:00, 16.70it/s]

    Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:08<00:00, 16.98it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:08<00:00, 16.98it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:08<00:00, 16.98it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:08<00:00, 16.98it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:08<00:00, 17.70it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:08<00:00, 17.70it/s]

    Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:08<00:00, 17.70it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:09<00:00, 18.01it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:09<00:00, 18.01it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:09<00:00, 18.01it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:09<00:00, 18.01it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:09<00:00, 18.82it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:09<00:00, 18.82it/s]

    Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:09<00:00, 18.82it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:09<00:00, 18.82it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:09<00:00, 19.78it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:09<00:00, 19.78it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:09<00:00, 19.78it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:09<00:00,  6.18it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=85.22 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=85.22 GB):   2%|▏         | 1/58 [00:00<00:53,  1.06it/s]Capturing num tokens (num_tokens=7680 avail_mem=85.19 GB):   2%|▏         | 1/58 [00:00<00:53,  1.06it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=85.19 GB):   3%|▎         | 2/58 [00:01<00:45,  1.22it/s]Capturing num tokens (num_tokens=7168 avail_mem=85.19 GB):   3%|▎         | 2/58 [00:01<00:45,  1.22it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=85.19 GB):   5%|▌         | 3/58 [00:02<00:38,  1.42it/s]Capturing num tokens (num_tokens=6656 avail_mem=85.19 GB):   5%|▌         | 3/58 [00:02<00:38,  1.42it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=85.19 GB):   7%|▋         | 4/58 [00:02<00:31,  1.70it/s]Capturing num tokens (num_tokens=6144 avail_mem=85.19 GB):   7%|▋         | 4/58 [00:02<00:31,  1.70it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=85.19 GB):   9%|▊         | 5/58 [00:03<00:26,  1.99it/s]Capturing num tokens (num_tokens=5632 avail_mem=85.19 GB):   9%|▊         | 5/58 [00:03<00:26,  1.99it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=85.19 GB):  10%|█         | 6/58 [00:03<00:22,  2.31it/s]Capturing num tokens (num_tokens=5120 avail_mem=85.19 GB):  10%|█         | 6/58 [00:03<00:22,  2.31it/s]Capturing num tokens (num_tokens=5120 avail_mem=85.19 GB):  12%|█▏        | 7/58 [00:03<00:17,  2.89it/s]Capturing num tokens (num_tokens=4608 avail_mem=85.19 GB):  12%|█▏        | 7/58 [00:03<00:17,  2.89it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=85.19 GB):  14%|█▍        | 8/58 [00:03<00:15,  3.33it/s]Capturing num tokens (num_tokens=4096 avail_mem=82.97 GB):  14%|█▍        | 8/58 [00:03<00:15,  3.33it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=82.97 GB):  16%|█▌        | 9/58 [00:04<00:17,  2.79it/s]Capturing num tokens (num_tokens=3840 avail_mem=84.14 GB):  16%|█▌        | 9/58 [00:04<00:17,  2.79it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=84.14 GB):  17%|█▋        | 10/58 [00:04<00:17,  2.74it/s]Capturing num tokens (num_tokens=3584 avail_mem=84.15 GB):  17%|█▋        | 10/58 [00:04<00:17,  2.74it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=84.15 GB):  19%|█▉        | 11/58 [00:04<00:18,  2.61it/s]Capturing num tokens (num_tokens=3328 avail_mem=84.37 GB):  19%|█▉        | 11/58 [00:04<00:18,  2.61it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=84.37 GB):  21%|██        | 12/58 [00:05<00:17,  2.60it/s]Capturing num tokens (num_tokens=3072 avail_mem=84.22 GB):  21%|██        | 12/58 [00:05<00:17,  2.60it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=84.22 GB):  22%|██▏       | 13/58 [00:05<00:16,  2.70it/s]Capturing num tokens (num_tokens=2816 avail_mem=84.38 GB):  22%|██▏       | 13/58 [00:05<00:16,  2.70it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=84.38 GB):  24%|██▍       | 14/58 [00:06<00:16,  2.73it/s]Capturing num tokens (num_tokens=2560 avail_mem=83.52 GB):  24%|██▍       | 14/58 [00:06<00:16,  2.73it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=83.52 GB):  26%|██▌       | 15/58 [00:06<00:14,  2.89it/s]Capturing num tokens (num_tokens=2304 avail_mem=84.58 GB):  26%|██▌       | 15/58 [00:06<00:14,  2.89it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=84.58 GB):  28%|██▊       | 16/58 [00:06<00:14,  2.93it/s]Capturing num tokens (num_tokens=2048 avail_mem=83.65 GB):  28%|██▊       | 16/58 [00:06<00:14,  2.93it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=83.65 GB):  29%|██▉       | 17/58 [00:06<00:12,  3.17it/s]Capturing num tokens (num_tokens=1792 avail_mem=84.43 GB):  29%|██▉       | 17/58 [00:06<00:12,  3.17it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=84.43 GB):  31%|███       | 18/58 [00:07<00:12,  3.30it/s]Capturing num tokens (num_tokens=1536 avail_mem=84.54 GB):  31%|███       | 18/58 [00:07<00:12,  3.30it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=84.54 GB):  33%|███▎      | 19/58 [00:07<00:11,  3.53it/s]Capturing num tokens (num_tokens=1280 avail_mem=84.50 GB):  33%|███▎      | 19/58 [00:07<00:11,  3.53it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=84.50 GB):  34%|███▍      | 20/58 [00:07<00:09,  3.84it/s]Capturing num tokens (num_tokens=1024 avail_mem=84.56 GB):  34%|███▍      | 20/58 [00:07<00:09,  3.84it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=84.56 GB):  36%|███▌      | 21/58 [00:07<00:08,  4.13it/s]Capturing num tokens (num_tokens=960 avail_mem=83.96 GB):  36%|███▌      | 21/58 [00:07<00:08,  4.13it/s] Capturing num tokens (num_tokens=960 avail_mem=83.96 GB):  38%|███▊      | 22/58 [00:08<00:08,  4.37it/s]Capturing num tokens (num_tokens=896 avail_mem=85.17 GB):  38%|███▊      | 22/58 [00:08<00:08,  4.37it/s]

    Capturing num tokens (num_tokens=896 avail_mem=85.17 GB):  40%|███▉      | 23/58 [00:08<00:07,  4.65it/s]Capturing num tokens (num_tokens=832 avail_mem=84.09 GB):  40%|███▉      | 23/58 [00:08<00:07,  4.65it/s]Capturing num tokens (num_tokens=832 avail_mem=84.09 GB):  41%|████▏     | 24/58 [00:08<00:06,  4.90it/s]Capturing num tokens (num_tokens=768 avail_mem=85.16 GB):  41%|████▏     | 24/58 [00:08<00:06,  4.90it/s]

    Capturing num tokens (num_tokens=768 avail_mem=85.16 GB):  43%|████▎     | 25/58 [00:08<00:06,  5.20it/s]Capturing num tokens (num_tokens=704 avail_mem=84.17 GB):  43%|████▎     | 25/58 [00:08<00:06,  5.20it/s]Capturing num tokens (num_tokens=704 avail_mem=84.17 GB):  45%|████▍     | 26/58 [00:08<00:05,  5.36it/s]Capturing num tokens (num_tokens=640 avail_mem=84.66 GB):  45%|████▍     | 26/58 [00:08<00:05,  5.36it/s]

    Capturing num tokens (num_tokens=640 avail_mem=84.66 GB):  47%|████▋     | 27/58 [00:08<00:05,  5.76it/s]Capturing num tokens (num_tokens=576 avail_mem=84.69 GB):  47%|████▋     | 27/58 [00:08<00:05,  5.76it/s]Capturing num tokens (num_tokens=576 avail_mem=84.69 GB):  48%|████▊     | 28/58 [00:09<00:04,  6.18it/s]Capturing num tokens (num_tokens=512 avail_mem=84.71 GB):  48%|████▊     | 28/58 [00:09<00:04,  6.18it/s]

    Capturing num tokens (num_tokens=512 avail_mem=84.71 GB):  50%|█████     | 29/58 [00:09<00:04,  6.23it/s]Capturing num tokens (num_tokens=480 avail_mem=85.16 GB):  50%|█████     | 29/58 [00:09<00:04,  6.23it/s]Capturing num tokens (num_tokens=480 avail_mem=85.16 GB):  52%|█████▏    | 30/58 [00:09<00:04,  6.60it/s]Capturing num tokens (num_tokens=448 avail_mem=84.34 GB):  52%|█████▏    | 30/58 [00:09<00:04,  6.60it/s]

    Capturing num tokens (num_tokens=448 avail_mem=84.34 GB):  53%|█████▎    | 31/58 [00:09<00:04,  6.69it/s]Capturing num tokens (num_tokens=416 avail_mem=85.15 GB):  53%|█████▎    | 31/58 [00:09<00:04,  6.69it/s]Capturing num tokens (num_tokens=416 avail_mem=85.15 GB):  55%|█████▌    | 32/58 [00:09<00:03,  6.80it/s]Capturing num tokens (num_tokens=384 avail_mem=84.39 GB):  55%|█████▌    | 32/58 [00:09<00:03,  6.80it/s]

    Capturing num tokens (num_tokens=384 avail_mem=84.39 GB):  57%|█████▋    | 33/58 [00:09<00:03,  7.33it/s]Capturing num tokens (num_tokens=352 avail_mem=85.13 GB):  57%|█████▋    | 33/58 [00:09<00:03,  7.33it/s]Capturing num tokens (num_tokens=352 avail_mem=85.13 GB):  59%|█████▊    | 34/58 [00:09<00:03,  7.38it/s]Capturing num tokens (num_tokens=320 avail_mem=84.77 GB):  59%|█████▊    | 34/58 [00:09<00:03,  7.38it/s]

    Capturing num tokens (num_tokens=320 avail_mem=84.77 GB):  60%|██████    | 35/58 [00:09<00:02,  7.78it/s]Capturing num tokens (num_tokens=288 avail_mem=84.83 GB):  60%|██████    | 35/58 [00:09<00:02,  7.78it/s]Capturing num tokens (num_tokens=256 avail_mem=84.79 GB):  60%|██████    | 35/58 [00:10<00:02,  7.78it/s]

    Capturing num tokens (num_tokens=256 avail_mem=84.79 GB):  64%|██████▍   | 37/58 [00:10<00:02,  8.19it/s]Capturing num tokens (num_tokens=240 avail_mem=85.12 GB):  64%|██████▍   | 37/58 [00:10<00:02,  8.19it/s]Capturing num tokens (num_tokens=224 avail_mem=84.58 GB):  64%|██████▍   | 37/58 [00:10<00:02,  8.19it/s]Capturing num tokens (num_tokens=224 avail_mem=84.58 GB):  67%|██████▋   | 39/58 [00:10<00:02,  9.04it/s]Capturing num tokens (num_tokens=208 avail_mem=85.11 GB):  67%|██████▋   | 39/58 [00:10<00:02,  9.04it/s]

    Capturing num tokens (num_tokens=192 avail_mem=84.83 GB):  67%|██████▋   | 39/58 [00:10<00:02,  9.04it/s]Capturing num tokens (num_tokens=192 avail_mem=84.83 GB):  71%|███████   | 41/58 [00:10<00:01,  9.51it/s]Capturing num tokens (num_tokens=176 avail_mem=85.11 GB):  71%|███████   | 41/58 [00:10<00:01,  9.51it/s]Capturing num tokens (num_tokens=160 avail_mem=84.70 GB):  71%|███████   | 41/58 [00:10<00:01,  9.51it/s]

    Capturing num tokens (num_tokens=160 avail_mem=84.70 GB):  74%|███████▍  | 43/58 [00:10<00:01, 10.54it/s]Capturing num tokens (num_tokens=144 avail_mem=85.09 GB):  74%|███████▍  | 43/58 [00:10<00:01, 10.54it/s]Capturing num tokens (num_tokens=128 avail_mem=84.87 GB):  74%|███████▍  | 43/58 [00:10<00:01, 10.54it/s]Capturing num tokens (num_tokens=128 avail_mem=84.87 GB):  78%|███████▊  | 45/58 [00:10<00:01, 11.32it/s]Capturing num tokens (num_tokens=112 avail_mem=85.00 GB):  78%|███████▊  | 45/58 [00:10<00:01, 11.32it/s]

    Capturing num tokens (num_tokens=96 avail_mem=84.78 GB):  78%|███████▊  | 45/58 [00:10<00:01, 11.32it/s] Capturing num tokens (num_tokens=96 avail_mem=84.78 GB):  81%|████████  | 47/58 [00:10<00:00, 12.19it/s]Capturing num tokens (num_tokens=80 avail_mem=84.80 GB):  81%|████████  | 47/58 [00:10<00:00, 12.19it/s]Capturing num tokens (num_tokens=64 avail_mem=85.07 GB):  81%|████████  | 47/58 [00:11<00:00, 12.19it/s]

    Capturing num tokens (num_tokens=64 avail_mem=85.07 GB):  84%|████████▍ | 49/58 [00:11<00:00, 12.69it/s]Capturing num tokens (num_tokens=48 avail_mem=84.96 GB):  84%|████████▍ | 49/58 [00:11<00:00, 12.69it/s]Capturing num tokens (num_tokens=32 avail_mem=84.94 GB):  84%|████████▍ | 49/58 [00:11<00:00, 12.69it/s]Capturing num tokens (num_tokens=32 avail_mem=84.94 GB):  88%|████████▊ | 51/58 [00:11<00:00, 13.93it/s]Capturing num tokens (num_tokens=28 avail_mem=85.04 GB):  88%|████████▊ | 51/58 [00:11<00:00, 13.93it/s]Capturing num tokens (num_tokens=24 avail_mem=85.03 GB):  88%|████████▊ | 51/58 [00:11<00:00, 13.93it/s]

    Capturing num tokens (num_tokens=24 avail_mem=85.03 GB):  91%|█████████▏| 53/58 [00:11<00:00, 14.39it/s]Capturing num tokens (num_tokens=20 avail_mem=85.02 GB):  91%|█████████▏| 53/58 [00:11<00:00, 14.39it/s]Capturing num tokens (num_tokens=16 avail_mem=85.02 GB):  91%|█████████▏| 53/58 [00:11<00:00, 14.39it/s]Capturing num tokens (num_tokens=16 avail_mem=85.02 GB):  95%|█████████▍| 55/58 [00:11<00:00, 15.05it/s]Capturing num tokens (num_tokens=12 avail_mem=84.92 GB):  95%|█████████▍| 55/58 [00:11<00:00, 15.05it/s]Capturing num tokens (num_tokens=8 avail_mem=84.99 GB):  95%|█████████▍| 55/58 [00:11<00:00, 15.05it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=84.98 GB):  95%|█████████▍| 55/58 [00:11<00:00, 15.05it/s]Capturing num tokens (num_tokens=4 avail_mem=84.98 GB): 100%|██████████| 58/58 [00:11<00:00, 16.49it/s]Capturing num tokens (num_tokens=4 avail_mem=84.98 GB): 100%|██████████| 58/58 [00:11<00:00,  4.98it/s]



<strong style='color: #00008B;'>==== Original Output ====</strong>



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of the numbers 1 and 3.<br><br>Next, I add the two numbers together: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></think><br><br>Sure! Let's solve the problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Start with the number 1.**<br>2. **Add the number 3 to it.**<br>   <br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:** \(\boxed{4}\)</strong>



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of the numbers 1 and 3.<br><br>Next, I add the two numbers together: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>Sure! Let's solve the problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Start with the number 1.**<br>2. **Add the number 3 to it.**<br>   <br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:** \(\boxed{4}\)</strong>



```python
llm.shutdown()
```

## Supporting New Reasoning Model Schemas

For future reasoning models, you can implement the reasoning parser as a subclass of `BaseReasoningFormatDetector` in `python/sglang/srt/reasoning_parser.py` and specify the reasoning parser for new reasoning model schemas accordingly.
