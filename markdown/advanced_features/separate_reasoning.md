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
    2026-04-03 15:37:22.649 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 15:37:22] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 15:37:22.650 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 15:37:22] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 15:37:22.650 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 15:37:22] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 15:37:22.650 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 15:37:22] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 15:37:22.650 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 15:37:22] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Multi-thread loading shards:  50% Completed | 1/2 [00:01<00:01,  1.04s/it]

    Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.17s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.15s/it]


    2026-04-03 15:37:27,674 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 15:37:27] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:50,  2.99s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:50,  2.99s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:21,  1.46s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:21,  1.46s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:48,  1.14it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:48,  1.14it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:32,  1.66it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:32,  1.66it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:03<00:23,  2.27it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:03<00:23,  2.27it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:17,  2.92it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:17,  2.92it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:13,  3.68it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:13,  3.68it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.44it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.44it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.28it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.28it/s]

    Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.28it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:06,  6.98it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:06,  6.98it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:06,  6.98it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:04<00:05,  8.44it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:04<00:05,  8.44it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:04<00:05,  8.44it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:04<00:04, 10.02it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:04<00:04, 10.02it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:04<00:04, 10.02it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:04<00:03, 11.95it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:04<00:03, 11.95it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:03, 11.95it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:05<00:03, 11.95it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 15.24it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 15.24it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 15.24it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 15.24it/s]

    Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:05<00:02, 15.24it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:05<00:01, 20.33it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:05<00:01, 20.33it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:05<00:01, 20.33it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:05<00:01, 20.33it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:05<00:01, 20.33it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:05<00:01, 20.33it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:05<00:01, 26.84it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:05<00:01, 26.84it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:05<00:01, 26.84it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:05<00:01, 26.84it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:05<00:01, 26.84it/s]

    Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:05<00:01, 26.84it/s]Compiling num tokens (num_tokens=320):  50%|█████     | 29/58 [00:05<00:01, 26.84it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:05<00:00, 34.10it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:05<00:00, 34.10it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:05<00:00, 34.10it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:05<00:00, 34.10it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:05<00:00, 34.10it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:05<00:00, 34.10it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:05<00:00, 34.10it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:05<00:00, 39.29it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:05<00:00, 39.29it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:05<00:00, 39.29it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:05<00:00, 39.29it/s]

    Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:08<00:00, 39.29it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:08<00:00, 39.29it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:09<00:02,  4.32it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:09<00:02,  4.32it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:09<00:02,  4.32it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:09<00:02,  4.32it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:09<00:02,  4.32it/s]Compiling num tokens (num_tokens=32):  79%|███████▉  | 46/58 [00:09<00:02,  4.32it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:09<00:01,  6.00it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:09<00:01,  6.00it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:09<00:01,  6.00it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:09<00:01,  6.00it/s]

    Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:09<00:01,  6.00it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:09<00:01,  6.00it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:09<00:01,  6.00it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:09<00:00,  8.69it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:09<00:00,  8.69it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:09<00:00,  6.29it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=92.40 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=92.40 GB):   2%|▏         | 1/58 [00:00<00:20,  2.78it/s]Capturing num tokens (num_tokens=7680 avail_mem=92.37 GB):   2%|▏         | 1/58 [00:00<00:20,  2.78it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=92.37 GB):   3%|▎         | 2/58 [00:00<00:20,  2.77it/s]Capturing num tokens (num_tokens=7168 avail_mem=101.43 GB):   3%|▎         | 2/58 [00:00<00:20,  2.77it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=101.43 GB):   5%|▌         | 3/58 [00:00<00:17,  3.11it/s]Capturing num tokens (num_tokens=6656 avail_mem=101.44 GB):   5%|▌         | 3/58 [00:00<00:17,  3.11it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=101.44 GB):   7%|▋         | 4/58 [00:01<00:15,  3.59it/s]Capturing num tokens (num_tokens=6144 avail_mem=101.44 GB):   7%|▋         | 4/58 [00:01<00:15,  3.59it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=101.44 GB):   9%|▊         | 5/58 [00:01<00:13,  3.98it/s]Capturing num tokens (num_tokens=5632 avail_mem=101.44 GB):   9%|▊         | 5/58 [00:01<00:13,  3.98it/s]Capturing num tokens (num_tokens=5632 avail_mem=101.44 GB):  10%|█         | 6/58 [00:01<00:11,  4.45it/s]Capturing num tokens (num_tokens=5120 avail_mem=101.44 GB):  10%|█         | 6/58 [00:01<00:11,  4.45it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=101.44 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.89it/s]Capturing num tokens (num_tokens=4608 avail_mem=101.45 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.89it/s]Capturing num tokens (num_tokens=4608 avail_mem=101.45 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.44it/s]Capturing num tokens (num_tokens=4096 avail_mem=101.45 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.44it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=101.45 GB):  16%|█▌        | 9/58 [00:02<00:08,  5.97it/s]Capturing num tokens (num_tokens=3840 avail_mem=101.46 GB):  16%|█▌        | 9/58 [00:02<00:08,  5.97it/s]Capturing num tokens (num_tokens=3840 avail_mem=101.46 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.49it/s]Capturing num tokens (num_tokens=3584 avail_mem=101.46 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.49it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=101.46 GB):  19%|█▉        | 11/58 [00:02<00:06,  7.06it/s]Capturing num tokens (num_tokens=3328 avail_mem=101.46 GB):  19%|█▉        | 11/58 [00:02<00:06,  7.06it/s]Capturing num tokens (num_tokens=3328 avail_mem=101.46 GB):  21%|██        | 12/58 [00:02<00:05,  7.67it/s]Capturing num tokens (num_tokens=3072 avail_mem=101.45 GB):  21%|██        | 12/58 [00:02<00:05,  7.67it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=101.45 GB):  22%|██▏       | 13/58 [00:02<00:05,  8.18it/s]Capturing num tokens (num_tokens=2816 avail_mem=101.46 GB):  22%|██▏       | 13/58 [00:02<00:05,  8.18it/s]Capturing num tokens (num_tokens=2560 avail_mem=101.46 GB):  22%|██▏       | 13/58 [00:02<00:05,  8.18it/s]Capturing num tokens (num_tokens=2560 avail_mem=101.46 GB):  26%|██▌       | 15/58 [00:02<00:04,  9.60it/s]Capturing num tokens (num_tokens=2304 avail_mem=101.46 GB):  26%|██▌       | 15/58 [00:02<00:04,  9.60it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=101.45 GB):  26%|██▌       | 15/58 [00:02<00:04,  9.60it/s]Capturing num tokens (num_tokens=2048 avail_mem=101.45 GB):  29%|██▉       | 17/58 [00:02<00:04, 10.16it/s]Capturing num tokens (num_tokens=1792 avail_mem=101.46 GB):  29%|██▉       | 17/58 [00:02<00:04, 10.16it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=101.45 GB):  29%|██▉       | 17/58 [00:02<00:04, 10.16it/s]Capturing num tokens (num_tokens=1536 avail_mem=101.45 GB):  33%|███▎      | 19/58 [00:03<00:04,  8.53it/s]Capturing num tokens (num_tokens=1280 avail_mem=105.94 GB):  33%|███▎      | 19/58 [00:03<00:04,  8.53it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=105.94 GB):  33%|███▎      | 19/58 [00:03<00:04,  8.53it/s]Capturing num tokens (num_tokens=1024 avail_mem=105.94 GB):  36%|███▌      | 21/58 [00:03<00:03, 10.40it/s]Capturing num tokens (num_tokens=960 avail_mem=105.94 GB):  36%|███▌      | 21/58 [00:03<00:03, 10.40it/s] Capturing num tokens (num_tokens=896 avail_mem=105.93 GB):  36%|███▌      | 21/58 [00:03<00:03, 10.40it/s]Capturing num tokens (num_tokens=832 avail_mem=105.93 GB):  36%|███▌      | 21/58 [00:03<00:03, 10.40it/s]Capturing num tokens (num_tokens=832 avail_mem=105.93 GB):  41%|████▏     | 24/58 [00:03<00:02, 13.97it/s]Capturing num tokens (num_tokens=768 avail_mem=105.93 GB):  41%|████▏     | 24/58 [00:03<00:02, 13.97it/s]

    Capturing num tokens (num_tokens=704 avail_mem=104.77 GB):  41%|████▏     | 24/58 [00:03<00:02, 13.97it/s]Capturing num tokens (num_tokens=704 avail_mem=104.77 GB):  45%|████▍     | 26/58 [00:03<00:02, 12.44it/s]Capturing num tokens (num_tokens=640 avail_mem=104.76 GB):  45%|████▍     | 26/58 [00:03<00:02, 12.44it/s]

    Capturing num tokens (num_tokens=576 avail_mem=104.76 GB):  45%|████▍     | 26/58 [00:03<00:02, 12.44it/s]Capturing num tokens (num_tokens=576 avail_mem=104.76 GB):  48%|████▊     | 28/58 [00:03<00:02, 10.94it/s]Capturing num tokens (num_tokens=512 avail_mem=104.75 GB):  48%|████▊     | 28/58 [00:03<00:02, 10.94it/s]

    Capturing num tokens (num_tokens=480 avail_mem=104.75 GB):  48%|████▊     | 28/58 [00:03<00:02, 10.94it/s]Capturing num tokens (num_tokens=480 avail_mem=104.75 GB):  52%|█████▏    | 30/58 [00:03<00:02, 10.76it/s]Capturing num tokens (num_tokens=448 avail_mem=104.75 GB):  52%|█████▏    | 30/58 [00:03<00:02, 10.76it/s]

    Capturing num tokens (num_tokens=416 avail_mem=104.75 GB):  52%|█████▏    | 30/58 [00:04<00:02, 10.76it/s]Capturing num tokens (num_tokens=416 avail_mem=104.75 GB):  55%|█████▌    | 32/58 [00:04<00:02, 10.33it/s]Capturing num tokens (num_tokens=384 avail_mem=104.74 GB):  55%|█████▌    | 32/58 [00:04<00:02, 10.33it/s]

    Capturing num tokens (num_tokens=352 avail_mem=104.74 GB):  55%|█████▌    | 32/58 [00:04<00:02, 10.33it/s]Capturing num tokens (num_tokens=352 avail_mem=104.74 GB):  59%|█████▊    | 34/58 [00:04<00:02, 10.19it/s]Capturing num tokens (num_tokens=320 avail_mem=104.73 GB):  59%|█████▊    | 34/58 [00:04<00:02, 10.19it/s]Capturing num tokens (num_tokens=288 avail_mem=104.73 GB):  59%|█████▊    | 34/58 [00:04<00:02, 10.19it/s]

    Capturing num tokens (num_tokens=288 avail_mem=104.73 GB):  62%|██████▏   | 36/58 [00:04<00:02, 10.16it/s]Capturing num tokens (num_tokens=256 avail_mem=104.72 GB):  62%|██████▏   | 36/58 [00:04<00:02, 10.16it/s]Capturing num tokens (num_tokens=240 avail_mem=104.72 GB):  62%|██████▏   | 36/58 [00:04<00:02, 10.16it/s]Capturing num tokens (num_tokens=240 avail_mem=104.72 GB):  66%|██████▌   | 38/58 [00:04<00:01, 10.25it/s]Capturing num tokens (num_tokens=224 avail_mem=104.72 GB):  66%|██████▌   | 38/58 [00:04<00:01, 10.25it/s]

    Capturing num tokens (num_tokens=208 avail_mem=104.71 GB):  66%|██████▌   | 38/58 [00:04<00:01, 10.25it/s]Capturing num tokens (num_tokens=208 avail_mem=104.71 GB):  69%|██████▉   | 40/58 [00:04<00:01, 10.35it/s]Capturing num tokens (num_tokens=192 avail_mem=104.71 GB):  69%|██████▉   | 40/58 [00:04<00:01, 10.35it/s]Capturing num tokens (num_tokens=176 avail_mem=104.71 GB):  69%|██████▉   | 40/58 [00:05<00:01, 10.35it/s]

    Capturing num tokens (num_tokens=176 avail_mem=104.71 GB):  72%|███████▏  | 42/58 [00:05<00:01, 10.23it/s]Capturing num tokens (num_tokens=160 avail_mem=104.70 GB):  72%|███████▏  | 42/58 [00:05<00:01, 10.23it/s]Capturing num tokens (num_tokens=144 avail_mem=104.70 GB):  72%|███████▏  | 42/58 [00:05<00:01, 10.23it/s]Capturing num tokens (num_tokens=144 avail_mem=104.70 GB):  76%|███████▌  | 44/58 [00:05<00:01, 10.42it/s]Capturing num tokens (num_tokens=128 avail_mem=104.71 GB):  76%|███████▌  | 44/58 [00:05<00:01, 10.42it/s]

    Capturing num tokens (num_tokens=112 avail_mem=104.70 GB):  76%|███████▌  | 44/58 [00:05<00:01, 10.42it/s]Capturing num tokens (num_tokens=112 avail_mem=104.70 GB):  79%|███████▉  | 46/58 [00:05<00:01, 10.55it/s]Capturing num tokens (num_tokens=96 avail_mem=104.70 GB):  79%|███████▉  | 46/58 [00:05<00:01, 10.55it/s] Capturing num tokens (num_tokens=80 avail_mem=104.70 GB):  79%|███████▉  | 46/58 [00:05<00:01, 10.55it/s]

    Capturing num tokens (num_tokens=80 avail_mem=104.70 GB):  83%|████████▎ | 48/58 [00:05<00:00, 10.45it/s]Capturing num tokens (num_tokens=64 avail_mem=104.69 GB):  83%|████████▎ | 48/58 [00:05<00:00, 10.45it/s]Capturing num tokens (num_tokens=48 avail_mem=104.69 GB):  83%|████████▎ | 48/58 [00:05<00:00, 10.45it/s]

    Capturing num tokens (num_tokens=48 avail_mem=104.69 GB):  86%|████████▌ | 50/58 [00:06<00:00,  8.49it/s]Capturing num tokens (num_tokens=32 avail_mem=104.69 GB):  86%|████████▌ | 50/58 [00:06<00:00,  8.49it/s]Capturing num tokens (num_tokens=28 avail_mem=104.69 GB):  86%|████████▌ | 50/58 [00:06<00:00,  8.49it/s]Capturing num tokens (num_tokens=28 avail_mem=104.69 GB):  90%|████████▉ | 52/58 [00:06<00:00,  9.60it/s]Capturing num tokens (num_tokens=24 avail_mem=104.68 GB):  90%|████████▉ | 52/58 [00:06<00:00,  9.60it/s]Capturing num tokens (num_tokens=20 avail_mem=104.68 GB):  90%|████████▉ | 52/58 [00:06<00:00,  9.60it/s]

    Capturing num tokens (num_tokens=20 avail_mem=104.68 GB):  93%|█████████▎| 54/58 [00:06<00:00, 11.27it/s]Capturing num tokens (num_tokens=16 avail_mem=104.67 GB):  93%|█████████▎| 54/58 [00:06<00:00, 11.27it/s]Capturing num tokens (num_tokens=12 avail_mem=104.67 GB):  93%|█████████▎| 54/58 [00:06<00:00, 11.27it/s]Capturing num tokens (num_tokens=8 avail_mem=104.67 GB):  93%|█████████▎| 54/58 [00:06<00:00, 11.27it/s] Capturing num tokens (num_tokens=8 avail_mem=104.67 GB):  98%|█████████▊| 57/58 [00:06<00:00, 13.86it/s]Capturing num tokens (num_tokens=4 avail_mem=104.66 GB):  98%|█████████▊| 57/58 [00:06<00:00, 13.86it/s]Capturing num tokens (num_tokens=4 avail_mem=104.66 GB): 100%|██████████| 58/58 [00:06<00:00,  8.93it/s]


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



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I perform the addition: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>Sure! Let's solve the problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Start with the first number:**  <br>   \(1\)<br><br>2. **Add the second number:**  <br>   \(1 + 3\)<br><br>3. **Calculate the sum:**  <br>   \(1 + 3 = 4\)<br><br>**Final Answer:**  <br>\(\boxed{4}\)</strong>


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



<strong style='color: #00008B;'>I see that the user is asking for the sum of 1 and 3.<br><br>First, I'll identify the two numbers involved in the addition.<br><br>Next, I'll add them together to get the total.<br><br>Finally, I'll provide the final answer clearly.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'><br><br>To solve the addition problem \(1 + 3\), follow these simple steps:<br><br>1. **Start with the first number:**  <br>   \(1\)<br><br>2. **Add the second number:**  <br>   \(1 + 3\)<br><br>3. **Calculate the sum:**  <br>   \(1 + 3 = 4\)<br><br>So, the final answer is:<br><br>\[<br>\boxed{4}<br>\]</strong>


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



<strong style='color: #00008B;'><br><br>**Solution:**<br><br>We need to calculate the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the final answer is \(\boxed{4}\).</strong>


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



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I add the two numbers together: 1 plus 3 equals 4.<br><br>Finally, I conclude that the result of 1 plus 3 is 4.<br></think><br><br>**Solution:**<br><br>We need to find the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the result is \(\boxed{4}\).</strong>


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



<strong style='color: #00008B;'>I need to calculate the sum of 1 and 3.<br><br>Adding these two numbers together gives me 4.<br><br>So, the final answer is 4.<br></think><br><br>Sure, let's solve the problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Start with the number 1.**<br>2. **Add the number 3 to it.**<br>   <br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:** \(\boxed{4}\)</strong>


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:328: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      return await dependant.call(**values)



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>I need to calculate the sum of 1 and 3.<br><br>Adding these two numbers together gives me 4.<br><br>So, the final answer is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>Sure, let's solve the problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Start with the number 1.**<br>2. **Add the number 3 to it.**<br>   <br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:** \(\boxed{4}\)</strong>



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


    2026-04-03 15:38:13.862 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 15:38:13] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 15:38:13.862 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 15:38:13] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 15:38:13.862 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 15:38:13] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 15:38:13.862 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 15:38:13] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 15:38:13.862 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 15:38:13] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Multi-thread loading shards:  50% Completed | 1/2 [00:01<00:01,  1.18s/it]

    Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.31s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.29s/it]


    2026-04-03 15:38:19,269 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 15:38:19] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:00,  3.17s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:00,  3.17s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:28,  1.58s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:28,  1.58s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:52,  1.06it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:52,  1.06it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:34,  1.55it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:34,  1.55it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:27,  1.93it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:27,  1.93it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:22,  2.30it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:22,  2.30it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:18,  2.72it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:18,  2.72it/s]Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:05<00:15,  3.18it/s]

    Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:05<00:15,  3.18it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:05<00:13,  3.64it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:05<00:13,  3.64it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:05<00:12,  3.78it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:05<00:12,  3.78it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:05<00:11,  4.01it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:05<00:11,  4.01it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:05<00:10,  4.53it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:05<00:10,  4.53it/s]

    Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:05<00:10,  4.53it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:05<00:06,  6.49it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:05<00:06,  6.49it/s]Compiling num tokens (num_tokens=2304):  24%|██▍       | 14/58 [00:06<00:06,  6.49it/s]

    Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:06<00:05,  8.35it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:06<00:05,  8.35it/s]Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:06<00:05,  8.35it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:06<00:03, 10.49it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:06<00:03, 10.49it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:06<00:03, 10.49it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:06<00:03, 10.49it/s]

    Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:06<00:02, 14.22it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:06<00:02, 14.22it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:06<00:02, 14.22it/s]Compiling num tokens (num_tokens=832):  36%|███▌      | 21/58 [00:06<00:02, 14.22it/s]Compiling num tokens (num_tokens=768):  36%|███▌      | 21/58 [00:06<00:02, 14.22it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:06<00:01, 19.50it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:06<00:01, 19.50it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:06<00:01, 19.50it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:06<00:01, 19.50it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:06<00:01, 19.50it/s]

    Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:06<00:01, 24.33it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:06<00:01, 24.33it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:06<00:01, 24.33it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:06<00:01, 24.33it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:06<00:01, 24.33it/s]Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:06<00:01, 24.33it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:06<00:00, 30.25it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:06<00:00, 30.25it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:06<00:00, 30.25it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:06<00:00, 30.25it/s]Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:06<00:00, 30.25it/s]Compiling num tokens (num_tokens=224):  59%|█████▊    | 34/58 [00:06<00:00, 30.25it/s]

    Compiling num tokens (num_tokens=208):  59%|█████▊    | 34/58 [00:06<00:00, 30.25it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:06<00:00, 36.43it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:06<00:00, 36.43it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:06<00:00, 36.43it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:06<00:00, 36.43it/s]Compiling num tokens (num_tokens=144):  69%|██████▉   | 40/58 [00:06<00:00, 36.43it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:06<00:00, 31.38it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:06<00:00, 31.38it/s]

    Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:06<00:00, 31.38it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:07<00:00, 31.38it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:07<00:00, 31.38it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:07<00:00, 29.83it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:07<00:00, 29.83it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:07<00:00, 29.83it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:07<00:00, 29.83it/s]Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:07<00:00, 29.83it/s]

    Compiling num tokens (num_tokens=24):  83%|████████▎ | 48/58 [00:07<00:00, 29.83it/s]Compiling num tokens (num_tokens=20):  83%|████████▎ | 48/58 [00:07<00:00, 29.83it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:07<00:00, 36.10it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:07<00:00, 36.10it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:07<00:00, 36.10it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:07<00:00, 36.10it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:07<00:00, 36.10it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:07<00:00,  8.00it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=117.72 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=117.72 GB):   2%|▏         | 1/58 [00:00<00:16,  3.45it/s]Capturing num tokens (num_tokens=7680 avail_mem=117.69 GB):   2%|▏         | 1/58 [00:00<00:16,  3.45it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=117.69 GB):   3%|▎         | 2/58 [00:00<00:16,  3.38it/s]Capturing num tokens (num_tokens=7168 avail_mem=117.69 GB):   3%|▎         | 2/58 [00:00<00:16,  3.38it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=117.69 GB):   5%|▌         | 3/58 [00:00<00:15,  3.66it/s]Capturing num tokens (num_tokens=6656 avail_mem=117.69 GB):   5%|▌         | 3/58 [00:00<00:15,  3.66it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=117.69 GB):   7%|▋         | 4/58 [00:01<00:13,  3.98it/s]Capturing num tokens (num_tokens=6144 avail_mem=117.70 GB):   7%|▋         | 4/58 [00:01<00:13,  3.98it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=117.70 GB):   9%|▊         | 5/58 [00:01<00:12,  4.26it/s]Capturing num tokens (num_tokens=5632 avail_mem=117.70 GB):   9%|▊         | 5/58 [00:01<00:12,  4.26it/s]Capturing num tokens (num_tokens=5632 avail_mem=117.70 GB):  10%|█         | 6/58 [00:01<00:11,  4.66it/s]Capturing num tokens (num_tokens=5120 avail_mem=117.70 GB):  10%|█         | 6/58 [00:01<00:11,  4.66it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=117.70 GB):  12%|█▏        | 7/58 [00:01<00:10,  5.04it/s]Capturing num tokens (num_tokens=4608 avail_mem=117.70 GB):  12%|█▏        | 7/58 [00:01<00:10,  5.04it/s]Capturing num tokens (num_tokens=4608 avail_mem=117.70 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.55it/s]Capturing num tokens (num_tokens=4096 avail_mem=117.71 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.55it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=117.71 GB):  16%|█▌        | 9/58 [00:01<00:08,  6.05it/s]Capturing num tokens (num_tokens=3840 avail_mem=117.71 GB):  16%|█▌        | 9/58 [00:01<00:08,  6.05it/s]Capturing num tokens (num_tokens=3840 avail_mem=117.71 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.52it/s]Capturing num tokens (num_tokens=3584 avail_mem=117.71 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.52it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=117.71 GB):  19%|█▉        | 11/58 [00:02<00:06,  7.04it/s]Capturing num tokens (num_tokens=3328 avail_mem=117.71 GB):  19%|█▉        | 11/58 [00:02<00:06,  7.04it/s]Capturing num tokens (num_tokens=3328 avail_mem=117.71 GB):  21%|██        | 12/58 [00:02<00:06,  7.63it/s]Capturing num tokens (num_tokens=3072 avail_mem=117.71 GB):  21%|██        | 12/58 [00:02<00:06,  7.63it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=117.71 GB):  22%|██▏       | 13/58 [00:02<00:05,  8.16it/s]Capturing num tokens (num_tokens=2816 avail_mem=117.71 GB):  22%|██▏       | 13/58 [00:02<00:05,  8.16it/s]Capturing num tokens (num_tokens=2560 avail_mem=117.71 GB):  22%|██▏       | 13/58 [00:02<00:05,  8.16it/s]Capturing num tokens (num_tokens=2560 avail_mem=117.71 GB):  26%|██▌       | 15/58 [00:02<00:04,  9.50it/s]Capturing num tokens (num_tokens=2304 avail_mem=117.71 GB):  26%|██▌       | 15/58 [00:02<00:04,  9.50it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=117.71 GB):  26%|██▌       | 15/58 [00:02<00:04,  9.50it/s]Capturing num tokens (num_tokens=2048 avail_mem=117.71 GB):  29%|██▉       | 17/58 [00:02<00:03, 11.02it/s]Capturing num tokens (num_tokens=1792 avail_mem=117.71 GB):  29%|██▉       | 17/58 [00:02<00:03, 11.02it/s]Capturing num tokens (num_tokens=1536 avail_mem=117.70 GB):  29%|██▉       | 17/58 [00:02<00:03, 11.02it/s]Capturing num tokens (num_tokens=1536 avail_mem=117.70 GB):  33%|███▎      | 19/58 [00:02<00:03, 12.71it/s]Capturing num tokens (num_tokens=1280 avail_mem=117.71 GB):  33%|███▎      | 19/58 [00:02<00:03, 12.71it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=117.71 GB):  33%|███▎      | 19/58 [00:02<00:03, 12.71it/s]Capturing num tokens (num_tokens=960 avail_mem=117.70 GB):  33%|███▎      | 19/58 [00:02<00:03, 12.71it/s] Capturing num tokens (num_tokens=960 avail_mem=117.70 GB):  38%|███▊      | 22/58 [00:02<00:02, 15.94it/s]Capturing num tokens (num_tokens=896 avail_mem=117.70 GB):  38%|███▊      | 22/58 [00:02<00:02, 15.94it/s]Capturing num tokens (num_tokens=832 avail_mem=117.70 GB):  38%|███▊      | 22/58 [00:02<00:02, 15.94it/s]Capturing num tokens (num_tokens=768 avail_mem=117.69 GB):  38%|███▊      | 22/58 [00:02<00:02, 15.94it/s]Capturing num tokens (num_tokens=768 avail_mem=117.69 GB):  43%|████▎     | 25/58 [00:02<00:01, 18.95it/s]Capturing num tokens (num_tokens=704 avail_mem=117.69 GB):  43%|████▎     | 25/58 [00:02<00:01, 18.95it/s]

    Capturing num tokens (num_tokens=640 avail_mem=117.69 GB):  43%|████▎     | 25/58 [00:03<00:01, 18.95it/s]Capturing num tokens (num_tokens=576 avail_mem=117.68 GB):  43%|████▎     | 25/58 [00:03<00:01, 18.95it/s]Capturing num tokens (num_tokens=576 avail_mem=117.68 GB):  48%|████▊     | 28/58 [00:03<00:01, 21.06it/s]Capturing num tokens (num_tokens=512 avail_mem=117.68 GB):  48%|████▊     | 28/58 [00:03<00:01, 21.06it/s]Capturing num tokens (num_tokens=480 avail_mem=117.67 GB):  48%|████▊     | 28/58 [00:03<00:01, 21.06it/s]Capturing num tokens (num_tokens=448 avail_mem=117.67 GB):  48%|████▊     | 28/58 [00:03<00:01, 21.06it/s]Capturing num tokens (num_tokens=416 avail_mem=117.67 GB):  48%|████▊     | 28/58 [00:03<00:01, 21.06it/s]

    Capturing num tokens (num_tokens=416 avail_mem=117.67 GB):  55%|█████▌    | 32/58 [00:03<00:01, 24.48it/s]Capturing num tokens (num_tokens=384 avail_mem=117.66 GB):  55%|█████▌    | 32/58 [00:03<00:01, 24.48it/s]Capturing num tokens (num_tokens=352 avail_mem=117.66 GB):  55%|█████▌    | 32/58 [00:03<00:01, 24.48it/s]Capturing num tokens (num_tokens=320 avail_mem=117.65 GB):  55%|█████▌    | 32/58 [00:03<00:01, 24.48it/s]Capturing num tokens (num_tokens=288 avail_mem=117.65 GB):  55%|█████▌    | 32/58 [00:03<00:01, 24.48it/s]Capturing num tokens (num_tokens=288 avail_mem=117.65 GB):  62%|██████▏   | 36/58 [00:03<00:00, 27.45it/s]Capturing num tokens (num_tokens=256 avail_mem=117.65 GB):  62%|██████▏   | 36/58 [00:03<00:00, 27.45it/s]Capturing num tokens (num_tokens=240 avail_mem=117.64 GB):  62%|██████▏   | 36/58 [00:03<00:00, 27.45it/s]Capturing num tokens (num_tokens=224 avail_mem=117.64 GB):  62%|██████▏   | 36/58 [00:03<00:00, 27.45it/s]

    Capturing num tokens (num_tokens=208 avail_mem=117.63 GB):  62%|██████▏   | 36/58 [00:03<00:00, 27.45it/s]Capturing num tokens (num_tokens=208 avail_mem=117.63 GB):  69%|██████▉   | 40/58 [00:03<00:00, 29.63it/s]Capturing num tokens (num_tokens=192 avail_mem=117.63 GB):  69%|██████▉   | 40/58 [00:03<00:00, 29.63it/s]Capturing num tokens (num_tokens=176 avail_mem=117.63 GB):  69%|██████▉   | 40/58 [00:03<00:00, 29.63it/s]Capturing num tokens (num_tokens=160 avail_mem=117.63 GB):  69%|██████▉   | 40/58 [00:03<00:00, 29.63it/s]Capturing num tokens (num_tokens=144 avail_mem=117.62 GB):  69%|██████▉   | 40/58 [00:03<00:00, 29.63it/s]Capturing num tokens (num_tokens=144 avail_mem=117.62 GB):  76%|███████▌  | 44/58 [00:03<00:00, 31.48it/s]Capturing num tokens (num_tokens=128 avail_mem=117.63 GB):  76%|███████▌  | 44/58 [00:03<00:00, 31.48it/s]Capturing num tokens (num_tokens=112 avail_mem=117.63 GB):  76%|███████▌  | 44/58 [00:03<00:00, 31.48it/s]Capturing num tokens (num_tokens=96 avail_mem=117.62 GB):  76%|███████▌  | 44/58 [00:03<00:00, 31.48it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=117.62 GB):  76%|███████▌  | 44/58 [00:03<00:00, 31.48it/s]Capturing num tokens (num_tokens=80 avail_mem=117.62 GB):  83%|████████▎ | 48/58 [00:03<00:00, 32.53it/s]Capturing num tokens (num_tokens=64 avail_mem=117.62 GB):  83%|████████▎ | 48/58 [00:03<00:00, 32.53it/s]Capturing num tokens (num_tokens=48 avail_mem=117.61 GB):  83%|████████▎ | 48/58 [00:03<00:00, 32.53it/s]Capturing num tokens (num_tokens=32 avail_mem=117.61 GB):  83%|████████▎ | 48/58 [00:03<00:00, 32.53it/s]Capturing num tokens (num_tokens=28 avail_mem=117.61 GB):  83%|████████▎ | 48/58 [00:03<00:00, 32.53it/s]Capturing num tokens (num_tokens=28 avail_mem=117.61 GB):  90%|████████▉ | 52/58 [00:03<00:00, 33.71it/s]Capturing num tokens (num_tokens=24 avail_mem=117.60 GB):  90%|████████▉ | 52/58 [00:03<00:00, 33.71it/s]Capturing num tokens (num_tokens=20 avail_mem=117.60 GB):  90%|████████▉ | 52/58 [00:03<00:00, 33.71it/s]Capturing num tokens (num_tokens=16 avail_mem=117.60 GB):  90%|████████▉ | 52/58 [00:03<00:00, 33.71it/s]

    Capturing num tokens (num_tokens=12 avail_mem=117.59 GB):  90%|████████▉ | 52/58 [00:03<00:00, 33.71it/s]Capturing num tokens (num_tokens=12 avail_mem=117.59 GB):  97%|█████████▋| 56/58 [00:03<00:00, 34.43it/s]Capturing num tokens (num_tokens=8 avail_mem=117.59 GB):  97%|█████████▋| 56/58 [00:03<00:00, 34.43it/s] Capturing num tokens (num_tokens=4 avail_mem=117.58 GB):  97%|█████████▋| 56/58 [00:03<00:00, 34.43it/s]Capturing num tokens (num_tokens=4 avail_mem=117.58 GB): 100%|██████████| 58/58 [00:03<00:00, 14.65it/s]



<strong style='color: #00008B;'>==== Original Output ====</strong>



<strong style='color: #00008B;'>I need to add the numbers 1 and 3 together. <br><br>First, I'll identify the two numbers involved in the addition: 1 and 3.<br><br>Next, I'll perform the addition operation by summing these two numbers.<br><br>Finally, the result of adding 1 and 3 is 4.<br></think><br><br>Sure! Let's solve the addition problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Perform the addition:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:**<br>\[<br>\boxed{4}<br>\]</strong>



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>I need to add the numbers 1 and 3 together. <br><br>First, I'll identify the two numbers involved in the addition: 1 and 3.<br><br>Next, I'll perform the addition operation by summing these two numbers.<br><br>Finally, the result of adding 1 and 3 is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>Sure! Let's solve the addition problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Perform the addition:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:**<br>\[<br>\boxed{4}<br>\]</strong>



```python
llm.shutdown()
```

## Supporting New Reasoning Model Schemas

For future reasoning models, you can implement the reasoning parser as a subclass of `BaseReasoningFormatDetector` in `python/sglang/srt/reasoning_parser.py` and specify the reasoning parser for new reasoning model schemas accordingly.
