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
    2026-03-27 01:39:32.169 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 01:39:32] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 01:39:32.169 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 01:39:32] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 01:39:32.169 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 01:39:32] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 01:39:32.169 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 01:39:32] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 01:39:32.169 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 01:39:32] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:01<00:01,  1.18s/it]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.25s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.24s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:53,  3.04s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:53,  3.04s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:22,  1.48s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:22,  1.48s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:48,  1.13it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:48,  1.13it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:32,  1.65it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:32,  1.65it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:03<00:23,  2.25it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:03<00:23,  2.25it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:17,  2.91it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:17,  2.91it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.61it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.61it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.40it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.40it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.29it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.29it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.29it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:06,  7.01it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:06,  7.01it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:06,  7.01it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:04<00:05,  8.53it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:04<00:05,  8.53it/s]

    Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:04<00:05,  8.53it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:04<00:04, 10.14it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:04<00:04, 10.14it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:04<00:04, 10.14it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:03, 12.09it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:03, 12.09it/s]

    Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:03, 12.09it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:05<00:03, 12.09it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 15.41it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 15.41it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 15.41it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 15.41it/s]Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:05<00:02, 15.41it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:05<00:01, 20.63it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:05<00:01, 20.63it/s]

    Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:05<00:01, 20.63it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:05<00:01, 20.63it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:05<00:01, 20.63it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:05<00:01, 20.63it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:05<00:01, 27.21it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:05<00:01, 27.21it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:05<00:01, 27.21it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:05<00:01, 27.21it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:05<00:01, 27.21it/s]Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:05<00:01, 27.21it/s]Compiling num tokens (num_tokens=320):  50%|█████     | 29/58 [00:05<00:01, 27.21it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:05<00:00, 34.70it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:05<00:00, 34.70it/s]

    Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:05<00:00, 34.70it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:05<00:00, 34.70it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:05<00:00, 34.70it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:05<00:00, 34.70it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:05<00:00, 34.70it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:05<00:00, 39.95it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:05<00:00, 39.95it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:05<00:00, 39.95it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:05<00:00, 39.95it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:05<00:00, 39.95it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:05<00:00, 39.95it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:05<00:00, 39.95it/s] 

    Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:05<00:00, 42.98it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:05<00:00, 42.98it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:05<00:00, 42.98it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:05<00:00, 42.98it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:05<00:00, 42.98it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:05<00:00, 42.98it/s]Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:05<00:00, 42.98it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:05<00:00, 46.96it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:05<00:00, 46.96it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:05<00:00, 46.96it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:05<00:00, 46.96it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:05<00:00, 46.96it/s] Compiling num tokens (num_tokens=4):  91%|█████████▏| 53/58 [00:05<00:00, 46.96it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:05<00:00,  9.85it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=121.57 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=121.57 GB):   2%|▏         | 1/58 [00:00<00:17,  3.32it/s]Capturing num tokens (num_tokens=7680 avail_mem=106.52 GB):   2%|▏         | 1/58 [00:00<00:17,  3.32it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=106.52 GB):   3%|▎         | 2/58 [00:00<00:16,  3.44it/s]Capturing num tokens (num_tokens=7168 avail_mem=106.52 GB):   3%|▎         | 2/58 [00:00<00:16,  3.44it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=106.52 GB):   5%|▌         | 3/58 [00:00<00:18,  3.04it/s]Capturing num tokens (num_tokens=6656 avail_mem=106.53 GB):   5%|▌         | 3/58 [00:00<00:18,  3.04it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=106.53 GB):   7%|▋         | 4/58 [00:01<00:16,  3.35it/s]Capturing num tokens (num_tokens=6144 avail_mem=106.53 GB):   7%|▋         | 4/58 [00:01<00:16,  3.35it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=106.53 GB):   9%|▊         | 5/58 [00:01<00:14,  3.76it/s]Capturing num tokens (num_tokens=5632 avail_mem=106.53 GB):   9%|▊         | 5/58 [00:01<00:14,  3.76it/s]Capturing num tokens (num_tokens=5632 avail_mem=106.53 GB):  10%|█         | 6/58 [00:01<00:12,  4.27it/s]Capturing num tokens (num_tokens=5120 avail_mem=106.53 GB):  10%|█         | 6/58 [00:01<00:12,  4.27it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=106.53 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.72it/s]Capturing num tokens (num_tokens=4608 avail_mem=106.54 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.72it/s]Capturing num tokens (num_tokens=4608 avail_mem=106.54 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.29it/s]Capturing num tokens (num_tokens=4096 avail_mem=106.54 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.29it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=106.54 GB):  16%|█▌        | 9/58 [00:02<00:08,  5.85it/s]Capturing num tokens (num_tokens=3840 avail_mem=106.55 GB):  16%|█▌        | 9/58 [00:02<00:08,  5.85it/s]Capturing num tokens (num_tokens=3840 avail_mem=106.55 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.39it/s]Capturing num tokens (num_tokens=3584 avail_mem=106.54 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.39it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=106.54 GB):  19%|█▉        | 11/58 [00:02<00:06,  6.98it/s]Capturing num tokens (num_tokens=3328 avail_mem=106.54 GB):  19%|█▉        | 11/58 [00:02<00:06,  6.98it/s]Capturing num tokens (num_tokens=3328 avail_mem=106.54 GB):  21%|██        | 12/58 [00:02<00:06,  7.51it/s]Capturing num tokens (num_tokens=3072 avail_mem=106.54 GB):  21%|██        | 12/58 [00:02<00:06,  7.51it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=106.54 GB):  22%|██▏       | 13/58 [00:02<00:05,  8.12it/s]Capturing num tokens (num_tokens=2816 avail_mem=106.54 GB):  22%|██▏       | 13/58 [00:02<00:05,  8.12it/s]Capturing num tokens (num_tokens=2560 avail_mem=106.54 GB):  22%|██▏       | 13/58 [00:02<00:05,  8.12it/s]Capturing num tokens (num_tokens=2560 avail_mem=106.54 GB):  26%|██▌       | 15/58 [00:02<00:04,  9.57it/s]Capturing num tokens (num_tokens=2304 avail_mem=106.54 GB):  26%|██▌       | 15/58 [00:02<00:04,  9.57it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=106.54 GB):  26%|██▌       | 15/58 [00:02<00:04,  9.57it/s]Capturing num tokens (num_tokens=2048 avail_mem=106.54 GB):  29%|██▉       | 17/58 [00:02<00:03, 11.08it/s]Capturing num tokens (num_tokens=1792 avail_mem=106.54 GB):  29%|██▉       | 17/58 [00:02<00:03, 11.08it/s]Capturing num tokens (num_tokens=1536 avail_mem=106.54 GB):  29%|██▉       | 17/58 [00:02<00:03, 11.08it/s]Capturing num tokens (num_tokens=1536 avail_mem=106.54 GB):  33%|███▎      | 19/58 [00:02<00:03, 12.85it/s]Capturing num tokens (num_tokens=1280 avail_mem=106.54 GB):  33%|███▎      | 19/58 [00:02<00:03, 12.85it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=106.54 GB):  33%|███▎      | 19/58 [00:02<00:03, 12.85it/s]Capturing num tokens (num_tokens=960 avail_mem=106.54 GB):  33%|███▎      | 19/58 [00:02<00:03, 12.85it/s] Capturing num tokens (num_tokens=960 avail_mem=106.54 GB):  38%|███▊      | 22/58 [00:03<00:02, 16.05it/s]Capturing num tokens (num_tokens=896 avail_mem=106.53 GB):  38%|███▊      | 22/58 [00:03<00:02, 16.05it/s]Capturing num tokens (num_tokens=832 avail_mem=106.53 GB):  38%|███▊      | 22/58 [00:03<00:02, 16.05it/s]Capturing num tokens (num_tokens=768 avail_mem=106.53 GB):  38%|███▊      | 22/58 [00:03<00:02, 16.05it/s]Capturing num tokens (num_tokens=768 avail_mem=106.53 GB):  43%|████▎     | 25/58 [00:03<00:01, 19.14it/s]Capturing num tokens (num_tokens=704 avail_mem=106.52 GB):  43%|████▎     | 25/58 [00:03<00:01, 19.14it/s]

    Capturing num tokens (num_tokens=640 avail_mem=106.52 GB):  43%|████▎     | 25/58 [00:03<00:01, 19.14it/s]Capturing num tokens (num_tokens=576 avail_mem=106.52 GB):  43%|████▎     | 25/58 [00:03<00:01, 19.14it/s]Capturing num tokens (num_tokens=512 avail_mem=106.51 GB):  43%|████▎     | 25/58 [00:03<00:01, 19.14it/s]Capturing num tokens (num_tokens=512 avail_mem=106.51 GB):  50%|█████     | 29/58 [00:03<00:01, 22.95it/s]Capturing num tokens (num_tokens=480 avail_mem=106.51 GB):  50%|█████     | 29/58 [00:03<00:01, 22.95it/s]Capturing num tokens (num_tokens=448 avail_mem=106.51 GB):  50%|█████     | 29/58 [00:03<00:01, 22.95it/s]Capturing num tokens (num_tokens=416 avail_mem=106.50 GB):  50%|█████     | 29/58 [00:03<00:01, 22.95it/s]Capturing num tokens (num_tokens=384 avail_mem=106.50 GB):  50%|█████     | 29/58 [00:03<00:01, 22.95it/s]

    Capturing num tokens (num_tokens=384 avail_mem=106.50 GB):  57%|█████▋    | 33/58 [00:03<00:00, 26.39it/s]Capturing num tokens (num_tokens=352 avail_mem=106.49 GB):  57%|█████▋    | 33/58 [00:03<00:00, 26.39it/s]Capturing num tokens (num_tokens=320 avail_mem=106.49 GB):  57%|█████▋    | 33/58 [00:03<00:00, 26.39it/s]Capturing num tokens (num_tokens=288 avail_mem=106.49 GB):  57%|█████▋    | 33/58 [00:03<00:00, 26.39it/s]Capturing num tokens (num_tokens=256 avail_mem=106.48 GB):  57%|█████▋    | 33/58 [00:03<00:00, 26.39it/s]Capturing num tokens (num_tokens=256 avail_mem=106.48 GB):  64%|██████▍   | 37/58 [00:03<00:00, 29.38it/s]Capturing num tokens (num_tokens=240 avail_mem=106.48 GB):  64%|██████▍   | 37/58 [00:03<00:00, 29.38it/s]Capturing num tokens (num_tokens=224 avail_mem=106.47 GB):  64%|██████▍   | 37/58 [00:03<00:00, 29.38it/s]Capturing num tokens (num_tokens=208 avail_mem=106.47 GB):  64%|██████▍   | 37/58 [00:03<00:00, 29.38it/s]Capturing num tokens (num_tokens=192 avail_mem=106.47 GB):  64%|██████▍   | 37/58 [00:03<00:00, 29.38it/s]

    Capturing num tokens (num_tokens=192 avail_mem=106.47 GB):  71%|███████   | 41/58 [00:03<00:00, 31.83it/s]Capturing num tokens (num_tokens=176 avail_mem=106.46 GB):  71%|███████   | 41/58 [00:03<00:00, 31.83it/s]Capturing num tokens (num_tokens=160 avail_mem=106.46 GB):  71%|███████   | 41/58 [00:03<00:00, 31.83it/s]Capturing num tokens (num_tokens=144 avail_mem=106.45 GB):  71%|███████   | 41/58 [00:03<00:00, 31.83it/s]Capturing num tokens (num_tokens=128 avail_mem=106.46 GB):  71%|███████   | 41/58 [00:03<00:00, 31.83it/s]Capturing num tokens (num_tokens=128 avail_mem=106.46 GB):  78%|███████▊  | 45/58 [00:03<00:00, 33.14it/s]Capturing num tokens (num_tokens=112 avail_mem=106.40 GB):  78%|███████▊  | 45/58 [00:03<00:00, 33.14it/s]Capturing num tokens (num_tokens=96 avail_mem=106.39 GB):  78%|███████▊  | 45/58 [00:03<00:00, 33.14it/s] Capturing num tokens (num_tokens=80 avail_mem=106.39 GB):  78%|███████▊  | 45/58 [00:03<00:00, 33.14it/s]Capturing num tokens (num_tokens=64 avail_mem=106.38 GB):  78%|███████▊  | 45/58 [00:03<00:00, 33.14it/s]

    Capturing num tokens (num_tokens=64 avail_mem=106.38 GB):  84%|████████▍ | 49/58 [00:03<00:00, 34.45it/s]Capturing num tokens (num_tokens=48 avail_mem=106.38 GB):  84%|████████▍ | 49/58 [00:03<00:00, 34.45it/s]Capturing num tokens (num_tokens=32 avail_mem=106.38 GB):  84%|████████▍ | 49/58 [00:03<00:00, 34.45it/s]Capturing num tokens (num_tokens=28 avail_mem=106.38 GB):  84%|████████▍ | 49/58 [00:03<00:00, 34.45it/s]Capturing num tokens (num_tokens=24 avail_mem=106.37 GB):  84%|████████▍ | 49/58 [00:03<00:00, 34.45it/s]Capturing num tokens (num_tokens=24 avail_mem=106.37 GB):  91%|█████████▏| 53/58 [00:03<00:00, 35.16it/s]Capturing num tokens (num_tokens=20 avail_mem=106.37 GB):  91%|█████████▏| 53/58 [00:03<00:00, 35.16it/s]Capturing num tokens (num_tokens=16 avail_mem=106.37 GB):  91%|█████████▏| 53/58 [00:03<00:00, 35.16it/s]Capturing num tokens (num_tokens=12 avail_mem=106.36 GB):  91%|█████████▏| 53/58 [00:03<00:00, 35.16it/s]Capturing num tokens (num_tokens=8 avail_mem=106.36 GB):  91%|█████████▏| 53/58 [00:03<00:00, 35.16it/s] 

    Capturing num tokens (num_tokens=8 avail_mem=106.36 GB):  98%|█████████▊| 57/58 [00:04<00:00, 36.05it/s]Capturing num tokens (num_tokens=4 avail_mem=106.35 GB):  98%|█████████▊| 57/58 [00:04<00:00, 36.05it/s]Capturing num tokens (num_tokens=4 avail_mem=106.35 GB): 100%|██████████| 58/58 [00:04<00:00, 14.37it/s]


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



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I perform the addition: 1 plus 3 equals 4.<br><br>Finally, I provide the answer, which is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>**Solution:**<br><br>To find the sum of 1 and 3, follow these simple steps:<br><br>1. **Start with the number 1.**<br>2. **Add the number 3 to it.**<br><br>\[<br>1 + 3 = 4<br>\]<br><br>**Final Answer:** \(\boxed{4}\)</strong>


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



<strong style='color: #00008B;'>First, I need to identify the two numbers in the problem, which are 1 and 3.<br><br>Next, I'll add these two numbers together.<br><br>Adding 1 and 3 gives me a total of 4.<br><br>So, the final answer is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'><br><br>Sure! Let's solve the addition problem step by step.<br><br>**Problem:**  <br>What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Final Answer:**<br>\[<br>\boxed{4}<br>\]</strong>


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



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of the numbers 1 and 3.<br><br>Next, I add the two numbers together to find the total.<br><br>Finally, I conclude that the result of 1 plus 3 is 4.<br></think><br><br>Sure! Let's solve the problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:**<br>\[<br>\boxed{4}<br>\]</strong>


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



<strong style='color: #00008B;'>I need to calculate the sum of 1 and 3.<br><br>First, I recognize that addition involves combining two numbers to find their total.<br><br>Adding 1 and 3 together gives me 4.<br><br>Therefore, the final answer is 4.<br></think><br><br>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the final answer is \(\boxed{4}\).</strong>



<strong style='color: #00008B;'>==== Reasoning ====</strong>


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:328: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      return await dependant.call(**values)



<strong style='color: #00008B;'>I need to calculate the sum of 1 and 3.<br><br>First, I recognize that addition involves combining two numbers to find their total.<br><br>Adding 1 and 3 together gives me 4.<br><br>Therefore, the final answer is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the final answer is \(\boxed{4}\).</strong>



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


    2026-03-27 01:40:16.781 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 01:40:16] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 01:40:16.781 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 01:40:16] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 01:40:16.781 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 01:40:16] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 01:40:16.781 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 01:40:16] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 01:40:16.781 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 01:40:16] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:01<00:01,  1.15s/it]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.22s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.21s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:56,  3.09s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:56,  3.09s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:23,  1.50s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:23,  1.50s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:49,  1.11it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:49,  1.11it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:33,  1.63it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:33,  1.63it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:23,  2.21it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:23,  2.21it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.80it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.80it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.53it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.53it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.32it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.32it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.21it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.21it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.21it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:06,  6.93it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:06,  6.93it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:06,  6.93it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:04<00:05,  8.46it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:04<00:05,  8.46it/s]

    Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:04<00:05,  8.46it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:05<00:04, 10.09it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:05<00:04, 10.09it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:05<00:04, 10.09it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:03, 12.03it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:03, 12.03it/s]

    Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:03, 12.03it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:05<00:03, 12.03it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 15.34it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 15.34it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 15.34it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 15.34it/s]Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:05<00:02, 15.34it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:05<00:01, 20.51it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:05<00:01, 20.51it/s]

    Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:05<00:01, 20.51it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:05<00:01, 20.51it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:05<00:01, 20.51it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:05<00:01, 20.51it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:05<00:01, 27.14it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:05<00:01, 27.14it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:05<00:01, 27.14it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:05<00:01, 27.14it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:05<00:01, 27.14it/s]Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:05<00:01, 27.14it/s]Compiling num tokens (num_tokens=320):  50%|█████     | 29/58 [00:05<00:01, 27.14it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:05<00:00, 34.66it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:05<00:00, 34.66it/s]

    Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:05<00:00, 34.66it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:05<00:00, 34.66it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:05<00:00, 34.66it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:05<00:00, 34.66it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:05<00:00, 34.66it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:05<00:00, 40.03it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:05<00:00, 40.03it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:05<00:00, 40.03it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:05<00:00, 40.03it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:05<00:00, 40.03it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:05<00:00, 40.03it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:05<00:00, 40.03it/s] 

    Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:05<00:00, 43.18it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:05<00:00, 43.18it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:05<00:00, 43.18it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:05<00:00, 43.18it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:05<00:00, 43.18it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:05<00:00, 43.18it/s]Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:05<00:00, 43.18it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:05<00:00, 47.25it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:05<00:00, 47.25it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:05<00:00, 47.25it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:05<00:00, 47.25it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:05<00:00, 47.25it/s] Compiling num tokens (num_tokens=4):  91%|█████████▏| 53/58 [00:05<00:00, 47.25it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:05<00:00,  9.73it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=103.19 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=103.19 GB):   2%|▏         | 1/58 [00:00<00:16,  3.54it/s]Capturing num tokens (num_tokens=7680 avail_mem=103.16 GB):   2%|▏         | 1/58 [00:00<00:16,  3.54it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=103.16 GB):   3%|▎         | 2/58 [00:00<00:15,  3.65it/s]Capturing num tokens (num_tokens=7168 avail_mem=103.16 GB):   3%|▎         | 2/58 [00:00<00:15,  3.65it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=103.16 GB):   5%|▌         | 3/58 [00:00<00:14,  3.87it/s]Capturing num tokens (num_tokens=6656 avail_mem=103.16 GB):   5%|▌         | 3/58 [00:00<00:14,  3.87it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=103.16 GB):   7%|▋         | 4/58 [00:01<00:12,  4.17it/s]Capturing num tokens (num_tokens=6144 avail_mem=103.17 GB):   7%|▋         | 4/58 [00:01<00:12,  4.17it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=103.17 GB):   9%|▊         | 5/58 [00:01<00:11,  4.43it/s]Capturing num tokens (num_tokens=5632 avail_mem=103.17 GB):   9%|▊         | 5/58 [00:01<00:11,  4.43it/s]Capturing num tokens (num_tokens=5632 avail_mem=103.17 GB):  10%|█         | 6/58 [00:01<00:10,  4.81it/s]Capturing num tokens (num_tokens=5120 avail_mem=103.17 GB):  10%|█         | 6/58 [00:01<00:10,  4.81it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=103.17 GB):  12%|█▏        | 7/58 [00:01<00:09,  5.19it/s]Capturing num tokens (num_tokens=4608 avail_mem=103.18 GB):  12%|█▏        | 7/58 [00:01<00:09,  5.19it/s]Capturing num tokens (num_tokens=4608 avail_mem=103.18 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.16it/s]Capturing num tokens (num_tokens=4096 avail_mem=103.18 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.16it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=103.18 GB):  16%|█▌        | 9/58 [00:01<00:09,  5.04it/s]Capturing num tokens (num_tokens=3840 avail_mem=103.19 GB):  16%|█▌        | 9/58 [00:01<00:09,  5.04it/s]Capturing num tokens (num_tokens=3840 avail_mem=103.19 GB):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Capturing num tokens (num_tokens=3584 avail_mem=103.18 GB):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=103.18 GB):  19%|█▉        | 11/58 [00:02<00:07,  6.32it/s]Capturing num tokens (num_tokens=3328 avail_mem=103.18 GB):  19%|█▉        | 11/58 [00:02<00:07,  6.32it/s]Capturing num tokens (num_tokens=3328 avail_mem=103.18 GB):  21%|██        | 12/58 [00:02<00:06,  6.76it/s]Capturing num tokens (num_tokens=3072 avail_mem=103.18 GB):  21%|██        | 12/58 [00:02<00:06,  6.76it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=103.18 GB):  22%|██▏       | 13/58 [00:02<00:07,  5.86it/s]Capturing num tokens (num_tokens=2816 avail_mem=103.18 GB):  22%|██▏       | 13/58 [00:02<00:07,  5.86it/s]Capturing num tokens (num_tokens=2560 avail_mem=103.18 GB):  22%|██▏       | 13/58 [00:02<00:07,  5.86it/s]Capturing num tokens (num_tokens=2560 avail_mem=103.18 GB):  26%|██▌       | 15/58 [00:02<00:05,  7.69it/s]Capturing num tokens (num_tokens=2304 avail_mem=103.18 GB):  26%|██▌       | 15/58 [00:02<00:05,  7.69it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=103.18 GB):  26%|██▌       | 15/58 [00:02<00:05,  7.69it/s]Capturing num tokens (num_tokens=2048 avail_mem=103.18 GB):  29%|██▉       | 17/58 [00:02<00:04,  9.40it/s]Capturing num tokens (num_tokens=1792 avail_mem=103.18 GB):  29%|██▉       | 17/58 [00:02<00:04,  9.40it/s]Capturing num tokens (num_tokens=1536 avail_mem=103.17 GB):  29%|██▉       | 17/58 [00:02<00:04,  9.40it/s]Capturing num tokens (num_tokens=1536 avail_mem=103.17 GB):  33%|███▎      | 19/58 [00:02<00:03, 11.19it/s]Capturing num tokens (num_tokens=1280 avail_mem=103.14 GB):  33%|███▎      | 19/58 [00:02<00:03, 11.19it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=103.14 GB):  33%|███▎      | 19/58 [00:03<00:03, 11.19it/s]Capturing num tokens (num_tokens=960 avail_mem=103.14 GB):  33%|███▎      | 19/58 [00:03<00:03, 11.19it/s] Capturing num tokens (num_tokens=960 avail_mem=103.14 GB):  38%|███▊      | 22/58 [00:03<00:02, 13.82it/s]Capturing num tokens (num_tokens=896 avail_mem=103.13 GB):  38%|███▊      | 22/58 [00:03<00:02, 13.82it/s]Capturing num tokens (num_tokens=832 avail_mem=103.13 GB):  38%|███▊      | 22/58 [00:03<00:02, 13.82it/s]

    Capturing num tokens (num_tokens=832 avail_mem=103.13 GB):  41%|████▏     | 24/58 [00:03<00:02, 14.72it/s]Capturing num tokens (num_tokens=768 avail_mem=103.13 GB):  41%|████▏     | 24/58 [00:03<00:02, 14.72it/s]Capturing num tokens (num_tokens=704 avail_mem=103.12 GB):  41%|████▏     | 24/58 [00:03<00:02, 14.72it/s]Capturing num tokens (num_tokens=640 avail_mem=103.12 GB):  41%|████▏     | 24/58 [00:03<00:02, 14.72it/s]Capturing num tokens (num_tokens=640 avail_mem=103.12 GB):  47%|████▋     | 27/58 [00:03<00:01, 18.16it/s]Capturing num tokens (num_tokens=576 avail_mem=103.12 GB):  47%|████▋     | 27/58 [00:03<00:01, 18.16it/s]Capturing num tokens (num_tokens=512 avail_mem=103.11 GB):  47%|████▋     | 27/58 [00:03<00:01, 18.16it/s]Capturing num tokens (num_tokens=480 avail_mem=103.11 GB):  47%|████▋     | 27/58 [00:03<00:01, 18.16it/s]Capturing num tokens (num_tokens=448 avail_mem=103.11 GB):  47%|████▋     | 27/58 [00:03<00:01, 18.16it/s]

    Capturing num tokens (num_tokens=448 avail_mem=103.11 GB):  53%|█████▎    | 31/58 [00:03<00:01, 22.20it/s]Capturing num tokens (num_tokens=416 avail_mem=103.10 GB):  53%|█████▎    | 31/58 [00:03<00:01, 22.20it/s]Capturing num tokens (num_tokens=384 avail_mem=103.10 GB):  53%|█████▎    | 31/58 [00:03<00:01, 22.20it/s]Capturing num tokens (num_tokens=352 avail_mem=103.09 GB):  53%|█████▎    | 31/58 [00:03<00:01, 22.20it/s]Capturing num tokens (num_tokens=320 avail_mem=103.09 GB):  53%|█████▎    | 31/58 [00:03<00:01, 22.20it/s]Capturing num tokens (num_tokens=320 avail_mem=103.09 GB):  60%|██████    | 35/58 [00:03<00:00, 25.14it/s]Capturing num tokens (num_tokens=288 avail_mem=103.09 GB):  60%|██████    | 35/58 [00:03<00:00, 25.14it/s]Capturing num tokens (num_tokens=256 avail_mem=103.08 GB):  60%|██████    | 35/58 [00:03<00:00, 25.14it/s]Capturing num tokens (num_tokens=240 avail_mem=103.08 GB):  60%|██████    | 35/58 [00:03<00:00, 25.14it/s]

    Capturing num tokens (num_tokens=224 avail_mem=103.08 GB):  60%|██████    | 35/58 [00:03<00:00, 25.14it/s]Capturing num tokens (num_tokens=224 avail_mem=103.08 GB):  67%|██████▋   | 39/58 [00:03<00:00, 28.28it/s]Capturing num tokens (num_tokens=208 avail_mem=103.07 GB):  67%|██████▋   | 39/58 [00:03<00:00, 28.28it/s]Capturing num tokens (num_tokens=192 avail_mem=103.07 GB):  67%|██████▋   | 39/58 [00:03<00:00, 28.28it/s]Capturing num tokens (num_tokens=176 avail_mem=103.06 GB):  67%|██████▋   | 39/58 [00:03<00:00, 28.28it/s]Capturing num tokens (num_tokens=160 avail_mem=103.06 GB):  67%|██████▋   | 39/58 [00:03<00:00, 28.28it/s]Capturing num tokens (num_tokens=160 avail_mem=103.06 GB):  74%|███████▍  | 43/58 [00:03<00:00, 30.57it/s]Capturing num tokens (num_tokens=144 avail_mem=103.06 GB):  74%|███████▍  | 43/58 [00:03<00:00, 30.57it/s]Capturing num tokens (num_tokens=128 avail_mem=103.07 GB):  74%|███████▍  | 43/58 [00:03<00:00, 30.57it/s]Capturing num tokens (num_tokens=112 avail_mem=103.06 GB):  74%|███████▍  | 43/58 [00:03<00:00, 30.57it/s]

    Capturing num tokens (num_tokens=96 avail_mem=103.06 GB):  74%|███████▍  | 43/58 [00:03<00:00, 30.57it/s] Capturing num tokens (num_tokens=96 avail_mem=103.06 GB):  81%|████████  | 47/58 [00:03<00:00, 32.06it/s]Capturing num tokens (num_tokens=80 avail_mem=103.05 GB):  81%|████████  | 47/58 [00:03<00:00, 32.06it/s]Capturing num tokens (num_tokens=64 avail_mem=103.05 GB):  81%|████████  | 47/58 [00:03<00:00, 32.06it/s]Capturing num tokens (num_tokens=48 avail_mem=103.05 GB):  81%|████████  | 47/58 [00:03<00:00, 32.06it/s]Capturing num tokens (num_tokens=32 avail_mem=103.04 GB):  81%|████████  | 47/58 [00:03<00:00, 32.06it/s]Capturing num tokens (num_tokens=32 avail_mem=103.04 GB):  88%|████████▊ | 51/58 [00:04<00:00, 33.36it/s]Capturing num tokens (num_tokens=28 avail_mem=103.04 GB):  88%|████████▊ | 51/58 [00:04<00:00, 33.36it/s]Capturing num tokens (num_tokens=24 avail_mem=103.04 GB):  88%|████████▊ | 51/58 [00:04<00:00, 33.36it/s]Capturing num tokens (num_tokens=20 avail_mem=103.03 GB):  88%|████████▊ | 51/58 [00:04<00:00, 33.36it/s]

    Capturing num tokens (num_tokens=16 avail_mem=103.03 GB):  88%|████████▊ | 51/58 [00:04<00:00, 33.36it/s]Capturing num tokens (num_tokens=16 avail_mem=103.03 GB):  95%|█████████▍| 55/58 [00:04<00:00, 33.68it/s]Capturing num tokens (num_tokens=12 avail_mem=103.03 GB):  95%|█████████▍| 55/58 [00:04<00:00, 33.68it/s]Capturing num tokens (num_tokens=8 avail_mem=103.02 GB):  95%|█████████▍| 55/58 [00:04<00:00, 33.68it/s] Capturing num tokens (num_tokens=4 avail_mem=103.02 GB):  95%|█████████▍| 55/58 [00:04<00:00, 33.68it/s]Capturing num tokens (num_tokens=4 avail_mem=103.02 GB): 100%|██████████| 58/58 [00:04<00:00, 13.76it/s]



<strong style='color: #00008B;'>==== Original Output ====</strong>



<strong style='color: #00008B;'>I need to calculate the sum of 1 and 3. <br><br>First, I'll add the two numbers together.<br><br>1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></think><br><br>**Solution:**<br><br>We need to calculate the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the final answer is \(\boxed{4}\).</strong>



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>I need to calculate the sum of 1 and 3. <br><br>First, I'll add the two numbers together.<br><br>1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>**Solution:**<br><br>We need to calculate the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the final answer is \(\boxed{4}\).</strong>



```python
llm.shutdown()
```

## Supporting New Reasoning Model Schemas

For future reasoning models, you can implement the reasoning parser as a subclass of `BaseReasoningFormatDetector` in `python/sglang/srt/reasoning_parser.py` and specify the reasoning parser for new reasoning model schemas accordingly.
