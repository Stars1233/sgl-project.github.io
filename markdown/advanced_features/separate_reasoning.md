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


    2026-04-02 09:20:25.393 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:20:25] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 09:20:25.393 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:20:25] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 09:20:25.393 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:20:25] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 09:20:25.393 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:20:25] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 09:20:25.393 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:20:25] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Multi-thread loading shards:  50% Completed | 1/2 [00:00<00:00,  1.11it/s]

    Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.24s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.19s/it]


    2026-04-02 09:20:33,219 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 09:20:33] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:05<05:26,  5.73s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:05<05:26,  5.73s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:06<02:52,  3.08s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:06<02:52,  3.08s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:07<01:45,  1.91s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:07<01:45,  1.91s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:07<01:10,  1.31s/it]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:07<01:10,  1.31s/it]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:08<00:50,  1.05it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:08<00:50,  1.05it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:08<00:38,  1.35it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:08<00:38,  1.35it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:08<00:30,  1.69it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:08<00:30,  1.69it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:09<00:24,  2.06it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:09<00:24,  2.06it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:09<00:19,  2.48it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:09<00:19,  2.48it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:09<00:16,  2.94it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:09<00:16,  2.94it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:09<00:13,  3.37it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:09<00:13,  3.37it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:09<00:12,  3.82it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:09<00:12,  3.82it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:10<00:10,  4.32it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:10<00:10,  4.32it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:10<00:09,  4.85it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:10<00:09,  4.85it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:10<00:07,  5.45it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:10<00:07,  5.45it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:10<00:06,  6.01it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:10<00:06,  6.01it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:10<00:06,  6.75it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:10<00:06,  6.75it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:10<00:05,  7.46it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:10<00:05,  7.46it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:10<00:05,  7.46it/s]

    Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:10<00:04,  8.97it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:10<00:04,  8.97it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:10<00:04,  8.97it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:10<00:03, 10.58it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:10<00:03, 10.58it/s]

    Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:11<00:03, 10.58it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:11<00:02, 12.01it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:11<00:02, 12.01it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:11<00:02, 12.01it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:11<00:02, 12.01it/s]

    Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:11<00:02, 14.57it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:11<00:02, 14.57it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:11<00:02, 14.57it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:11<00:02, 14.57it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:11<00:01, 17.53it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:11<00:01, 17.53it/s]Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:11<00:01, 17.53it/s]Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:11<00:01, 17.53it/s]

    Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:11<00:01, 19.12it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:11<00:01, 19.12it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:11<00:01, 19.12it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:11<00:01, 19.12it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:11<00:01, 19.12it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:11<00:00, 22.35it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:11<00:00, 22.35it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:11<00:00, 22.35it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:11<00:00, 22.35it/s]

    Compiling num tokens (num_tokens=192):  64%|██████▍   | 37/58 [00:11<00:00, 22.35it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:11<00:00, 25.53it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:11<00:00, 25.53it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:11<00:00, 25.53it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:11<00:00, 25.53it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:11<00:00, 25.53it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:11<00:00, 29.09it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:11<00:00, 29.09it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:11<00:00, 29.09it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:11<00:00, 29.09it/s]

    Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:11<00:00, 29.09it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:11<00:00, 31.48it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:11<00:00, 31.48it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:11<00:00, 31.48it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:11<00:00, 31.48it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:11<00:00, 31.48it/s]Compiling num tokens (num_tokens=20):  84%|████████▍ | 49/58 [00:12<00:00, 31.48it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:12<00:00, 35.28it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:12<00:00, 35.28it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:12<00:00, 35.28it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:12<00:00, 35.28it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:12<00:00, 35.28it/s]

    Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:12<00:00,  4.79it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=88.94 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=88.94 GB):   2%|▏         | 1/58 [00:00<00:24,  2.34it/s]Capturing num tokens (num_tokens=7680 avail_mem=87.88 GB):   2%|▏         | 1/58 [00:00<00:24,  2.34it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=87.88 GB):   3%|▎         | 2/58 [00:00<00:27,  2.00it/s]Capturing num tokens (num_tokens=7168 avail_mem=88.06 GB):   3%|▎         | 2/58 [00:00<00:27,  2.00it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=88.06 GB):   5%|▌         | 3/58 [00:01<00:28,  1.94it/s]Capturing num tokens (num_tokens=6656 avail_mem=88.12 GB):   5%|▌         | 3/58 [00:01<00:28,  1.94it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=88.12 GB):   7%|▋         | 4/58 [00:01<00:26,  2.03it/s]Capturing num tokens (num_tokens=6144 avail_mem=88.82 GB):   7%|▋         | 4/58 [00:01<00:26,  2.03it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=88.82 GB):   9%|▊         | 5/58 [00:02<00:25,  2.12it/s]Capturing num tokens (num_tokens=5632 avail_mem=88.90 GB):   9%|▊         | 5/58 [00:02<00:25,  2.12it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=88.90 GB):  10%|█         | 6/58 [00:02<00:22,  2.29it/s]Capturing num tokens (num_tokens=5120 avail_mem=88.24 GB):  10%|█         | 6/58 [00:02<00:22,  2.29it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=88.24 GB):  12%|█▏        | 7/58 [00:03<00:20,  2.48it/s]Capturing num tokens (num_tokens=4608 avail_mem=88.31 GB):  12%|█▏        | 7/58 [00:03<00:20,  2.48it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=88.31 GB):  14%|█▍        | 8/58 [00:03<00:19,  2.61it/s]Capturing num tokens (num_tokens=4096 avail_mem=88.37 GB):  14%|█▍        | 8/58 [00:03<00:19,  2.61it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=88.37 GB):  16%|█▌        | 9/58 [00:03<00:18,  2.62it/s]Capturing num tokens (num_tokens=3840 avail_mem=88.43 GB):  16%|█▌        | 9/58 [00:03<00:18,  2.62it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=88.43 GB):  17%|█▋        | 10/58 [00:04<00:18,  2.64it/s]Capturing num tokens (num_tokens=3584 avail_mem=88.93 GB):  17%|█▋        | 10/58 [00:04<00:18,  2.64it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=88.93 GB):  19%|█▉        | 11/58 [00:04<00:16,  2.83it/s]Capturing num tokens (num_tokens=3328 avail_mem=88.93 GB):  19%|█▉        | 11/58 [00:04<00:16,  2.83it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=88.93 GB):  21%|██        | 12/58 [00:04<00:14,  3.24it/s]Capturing num tokens (num_tokens=3072 avail_mem=88.52 GB):  21%|██        | 12/58 [00:04<00:14,  3.24it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=88.52 GB):  22%|██▏       | 13/58 [00:05<00:13,  3.26it/s]Capturing num tokens (num_tokens=2816 avail_mem=97.88 GB):  22%|██▏       | 13/58 [00:05<00:13,  3.26it/s]Capturing num tokens (num_tokens=2816 avail_mem=97.88 GB):  24%|██▍       | 14/58 [00:05<00:11,  3.71it/s]Capturing num tokens (num_tokens=2560 avail_mem=97.90 GB):  24%|██▍       | 14/58 [00:05<00:11,  3.71it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=97.90 GB):  26%|██▌       | 15/58 [00:05<00:10,  4.18it/s]Capturing num tokens (num_tokens=2304 avail_mem=98.26 GB):  26%|██▌       | 15/58 [00:05<00:10,  4.18it/s]Capturing num tokens (num_tokens=2304 avail_mem=98.26 GB):  28%|██▊       | 16/58 [00:05<00:09,  4.63it/s]Capturing num tokens (num_tokens=2048 avail_mem=98.26 GB):  28%|██▊       | 16/58 [00:05<00:09,  4.63it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=98.26 GB):  29%|██▉       | 17/58 [00:05<00:07,  5.13it/s]Capturing num tokens (num_tokens=1792 avail_mem=97.95 GB):  29%|██▉       | 17/58 [00:05<00:07,  5.13it/s]Capturing num tokens (num_tokens=1792 avail_mem=97.95 GB):  31%|███       | 18/58 [00:05<00:06,  5.95it/s]Capturing num tokens (num_tokens=1536 avail_mem=97.98 GB):  31%|███       | 18/58 [00:05<00:06,  5.95it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=97.98 GB):  33%|███▎      | 19/58 [00:05<00:05,  6.75it/s]Capturing num tokens (num_tokens=1280 avail_mem=98.00 GB):  33%|███▎      | 19/58 [00:05<00:05,  6.75it/s]Capturing num tokens (num_tokens=1280 avail_mem=98.00 GB):  34%|███▍      | 20/58 [00:05<00:05,  7.34it/s]Capturing num tokens (num_tokens=1024 avail_mem=98.25 GB):  34%|███▍      | 20/58 [00:05<00:05,  7.34it/s]

    Capturing num tokens (num_tokens=960 avail_mem=98.25 GB):  34%|███▍      | 20/58 [00:06<00:05,  7.34it/s] Capturing num tokens (num_tokens=960 avail_mem=98.25 GB):  38%|███▊      | 22/58 [00:06<00:03,  9.04it/s]Capturing num tokens (num_tokens=896 avail_mem=98.05 GB):  38%|███▊      | 22/58 [00:06<00:03,  9.04it/s]Capturing num tokens (num_tokens=832 avail_mem=98.07 GB):  38%|███▊      | 22/58 [00:06<00:03,  9.04it/s]

    Capturing num tokens (num_tokens=832 avail_mem=98.07 GB):  41%|████▏     | 24/58 [00:06<00:03, 10.20it/s]Capturing num tokens (num_tokens=768 avail_mem=98.23 GB):  41%|████▏     | 24/58 [00:06<00:03, 10.20it/s]Capturing num tokens (num_tokens=704 avail_mem=98.23 GB):  41%|████▏     | 24/58 [00:06<00:03, 10.20it/s]Capturing num tokens (num_tokens=704 avail_mem=98.23 GB):  45%|████▍     | 26/58 [00:06<00:02, 11.51it/s]Capturing num tokens (num_tokens=640 avail_mem=98.23 GB):  45%|████▍     | 26/58 [00:06<00:02, 11.51it/s]Capturing num tokens (num_tokens=576 avail_mem=98.22 GB):  45%|████▍     | 26/58 [00:06<00:02, 11.51it/s]

    Capturing num tokens (num_tokens=576 avail_mem=98.22 GB):  48%|████▊     | 28/58 [00:06<00:02, 12.73it/s]Capturing num tokens (num_tokens=512 avail_mem=98.21 GB):  48%|████▊     | 28/58 [00:06<00:02, 12.73it/s]Capturing num tokens (num_tokens=480 avail_mem=98.20 GB):  48%|████▊     | 28/58 [00:06<00:02, 12.73it/s]Capturing num tokens (num_tokens=480 avail_mem=98.20 GB):  52%|█████▏    | 30/58 [00:06<00:01, 14.06it/s]Capturing num tokens (num_tokens=448 avail_mem=98.22 GB):  52%|█████▏    | 30/58 [00:06<00:01, 14.06it/s]Capturing num tokens (num_tokens=416 avail_mem=98.13 GB):  52%|█████▏    | 30/58 [00:06<00:01, 14.06it/s]Capturing num tokens (num_tokens=384 avail_mem=98.18 GB):  52%|█████▏    | 30/58 [00:06<00:01, 14.06it/s]

    Capturing num tokens (num_tokens=384 avail_mem=98.18 GB):  57%|█████▋    | 33/58 [00:06<00:01, 16.55it/s]Capturing num tokens (num_tokens=352 avail_mem=98.17 GB):  57%|█████▋    | 33/58 [00:06<00:01, 16.55it/s]Capturing num tokens (num_tokens=320 avail_mem=98.16 GB):  57%|█████▋    | 33/58 [00:06<00:01, 16.55it/s]Capturing num tokens (num_tokens=288 avail_mem=98.16 GB):  57%|█████▋    | 33/58 [00:06<00:01, 16.55it/s]Capturing num tokens (num_tokens=288 avail_mem=98.16 GB):  62%|██████▏   | 36/58 [00:06<00:01, 17.88it/s]Capturing num tokens (num_tokens=256 avail_mem=98.15 GB):  62%|██████▏   | 36/58 [00:06<00:01, 17.88it/s]Capturing num tokens (num_tokens=240 avail_mem=98.14 GB):  62%|██████▏   | 36/58 [00:07<00:01, 17.88it/s]

    Capturing num tokens (num_tokens=224 avail_mem=98.13 GB):  62%|██████▏   | 36/58 [00:07<00:01, 17.88it/s]Capturing num tokens (num_tokens=224 avail_mem=98.13 GB):  67%|██████▋   | 39/58 [00:07<00:00, 19.35it/s]Capturing num tokens (num_tokens=208 avail_mem=98.12 GB):  67%|██████▋   | 39/58 [00:07<00:00, 19.35it/s]Capturing num tokens (num_tokens=192 avail_mem=98.12 GB):  67%|██████▋   | 39/58 [00:07<00:00, 19.35it/s]Capturing num tokens (num_tokens=176 avail_mem=98.11 GB):  67%|██████▋   | 39/58 [00:07<00:00, 19.35it/s]Capturing num tokens (num_tokens=176 avail_mem=98.11 GB):  72%|███████▏  | 42/58 [00:07<00:00, 21.02it/s]Capturing num tokens (num_tokens=160 avail_mem=98.11 GB):  72%|███████▏  | 42/58 [00:07<00:00, 21.02it/s]

    Capturing num tokens (num_tokens=144 avail_mem=98.09 GB):  72%|███████▏  | 42/58 [00:07<00:00, 21.02it/s]Capturing num tokens (num_tokens=128 avail_mem=102.59 GB):  72%|███████▏  | 42/58 [00:07<00:00, 21.02it/s]

    Capturing num tokens (num_tokens=128 avail_mem=102.59 GB):  78%|███████▊  | 45/58 [00:07<00:00, 16.76it/s]Capturing num tokens (num_tokens=112 avail_mem=102.58 GB):  78%|███████▊  | 45/58 [00:07<00:00, 16.76it/s]

    Capturing num tokens (num_tokens=96 avail_mem=102.55 GB):  78%|███████▊  | 45/58 [00:07<00:00, 16.76it/s] Capturing num tokens (num_tokens=96 avail_mem=102.55 GB):  81%|████████  | 47/58 [00:07<00:00, 13.37it/s]Capturing num tokens (num_tokens=80 avail_mem=102.56 GB):  81%|████████  | 47/58 [00:07<00:00, 13.37it/s]Capturing num tokens (num_tokens=64 avail_mem=102.55 GB):  81%|████████  | 47/58 [00:07<00:00, 13.37it/s]Capturing num tokens (num_tokens=48 avail_mem=102.55 GB):  81%|████████  | 47/58 [00:07<00:00, 13.37it/s]Capturing num tokens (num_tokens=48 avail_mem=102.55 GB):  86%|████████▌ | 50/58 [00:07<00:00, 16.19it/s]Capturing num tokens (num_tokens=32 avail_mem=102.54 GB):  86%|████████▌ | 50/58 [00:07<00:00, 16.19it/s]Capturing num tokens (num_tokens=28 avail_mem=102.55 GB):  86%|████████▌ | 50/58 [00:07<00:00, 16.19it/s]

    Capturing num tokens (num_tokens=24 avail_mem=102.54 GB):  86%|████████▌ | 50/58 [00:07<00:00, 16.19it/s]Capturing num tokens (num_tokens=24 avail_mem=102.54 GB):  91%|█████████▏| 53/58 [00:07<00:00, 18.85it/s]Capturing num tokens (num_tokens=20 avail_mem=102.51 GB):  91%|█████████▏| 53/58 [00:07<00:00, 18.85it/s]Capturing num tokens (num_tokens=16 avail_mem=102.51 GB):  91%|█████████▏| 53/58 [00:07<00:00, 18.85it/s]Capturing num tokens (num_tokens=12 avail_mem=102.50 GB):  91%|█████████▏| 53/58 [00:07<00:00, 18.85it/s]Capturing num tokens (num_tokens=8 avail_mem=102.49 GB):  91%|█████████▏| 53/58 [00:08<00:00, 18.85it/s] Capturing num tokens (num_tokens=8 avail_mem=102.49 GB):  98%|█████████▊| 57/58 [00:08<00:00, 22.13it/s]Capturing num tokens (num_tokens=4 avail_mem=102.51 GB):  98%|█████████▊| 57/58 [00:08<00:00, 22.13it/s]Capturing num tokens (num_tokens=4 avail_mem=102.51 GB): 100%|██████████| 58/58 [00:08<00:00,  7.18it/s]


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



<strong style='color: #00008B;'>First, I identify the two numbers in the addition problem: 1 and 3.<br><br>Next, I add the first number to the second number: 1 plus 3 equals 4.<br><br>Therefore, the sum of 1 and 3 is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>Sure! Let's solve the addition problem step by step.<br><br>**Problem:**  <br>What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**  <br>   The two numbers are **1** and **3**.<br><br>2. **Add the numbers together:**  <br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Final Answer:**  <br>\[<br>\boxed{4}<br>\]</strong>


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



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I perform the addition: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'><br><br>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>**Final Answer:** \boxed{4}</strong>


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



<strong style='color: #00008B;'>First, I need to identify the two numbers in the problem: 1 and 3.<br><br>Next, I'll add these two numbers together.<br><br>After performing the addition, the result is 4.</strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'><br><br>**Solution:**<br><br>We are asked to find the sum of \(1\) and \(3\).<br><br>\[<br>1 + 3 = 4<br>\]<br><br>**Answer:** \(\boxed{4}\)</strong>


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



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I'll add the two numbers together: 1 plus 3.<br><br>Finally, the result of this addition is 4.<br></think><br><br>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the final answer is \(\boxed{4}\).</strong>


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



<strong style='color: #00008B;'>First, I recognize that the user is asking for the sum of 1 and 3.<br><br>To solve this, I add the two numbers together.<br><br>1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></think><br><br>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the final answer is \(\boxed{4}\).</strong>


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:328: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      return await dependant.call(**values)



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>First, I recognize that the user is asking for the sum of 1 and 3.<br><br>To solve this, I add the two numbers together.<br><br>1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></strong>



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


    2026-04-02 09:21:22.611 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:21:22] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 09:21:22.611 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:21:22] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 09:21:22.611 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:21:22] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 09:21:22.611 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:21:22] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 09:21:22.611 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:21:22] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Multi-thread loading shards:  50% Completed | 1/2 [00:00<00:00,  1.05it/s]

    Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.23s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.19s/it]


    2026-04-02 09:21:29,464 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 09:21:29] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:58,  3.13s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:58,  3.13s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:38,  1.76s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:38,  1.76s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:01,  1.12s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:01,  1.12s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:42,  1.26it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:42,  1.26it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:31,  1.68it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:31,  1.68it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:24,  2.15it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:24,  2.15it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:18,  2.69it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:18,  2.69it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:05<00:15,  3.28it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:05<00:15,  3.28it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:05<00:12,  3.91it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:05<00:12,  3.91it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:05<00:10,  4.59it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:05<00:10,  4.59it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:05<00:10,  4.59it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:05<00:07,  6.36it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:05<00:07,  6.36it/s]

    Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:05<00:07,  6.36it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:05<00:05,  8.11it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:05<00:05,  8.11it/s]Compiling num tokens (num_tokens=2304):  24%|██▍       | 14/58 [00:06<00:05,  8.11it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:06<00:04,  9.94it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:06<00:04,  9.94it/s]

    Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:06<00:04,  9.94it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:06<00:03, 10.31it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:06<00:03, 10.31it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:06<00:03, 10.31it/s]

    Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:06<00:03, 10.52it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:06<00:03, 10.52it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:06<00:03, 10.52it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:06<00:02, 12.23it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:06<00:02, 12.23it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:06<00:02, 12.23it/s]

    Compiling num tokens (num_tokens=768):  38%|███▊      | 22/58 [00:06<00:02, 12.23it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:06<00:02, 15.02it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:06<00:02, 15.02it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:06<00:02, 15.02it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:06<00:01, 15.88it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:06<00:01, 15.88it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:06<00:01, 15.88it/s]

    Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:06<00:01, 15.88it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:06<00:01, 17.48it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:06<00:01, 17.48it/s]Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:07<00:01, 17.48it/s]Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:07<00:01, 17.48it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:07<00:01, 19.89it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:07<00:01, 19.89it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:07<00:01, 19.89it/s]

    Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:07<00:01, 19.89it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:07<00:01, 21.51it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:07<00:01, 21.51it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:07<00:01, 21.51it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:07<00:01, 21.51it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:07<00:00, 22.78it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:07<00:00, 22.78it/s]

    Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:07<00:00, 22.78it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:07<00:00, 22.78it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:07<00:00, 21.50it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:07<00:00, 21.50it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:07<00:00, 21.50it/s]

    Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:07<00:00, 21.50it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:07<00:00, 20.51it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:07<00:00, 20.51it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:07<00:00, 20.51it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:07<00:00, 20.51it/s]

    Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:07<00:00, 19.20it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:07<00:00, 19.20it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:07<00:00, 19.20it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:07<00:00, 18.97it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:07<00:00, 18.97it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:07<00:00, 18.97it/s]

    Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:08<00:00, 18.97it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:08<00:00, 19.58it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:08<00:00, 19.58it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:08<00:00, 19.58it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:08<00:00, 19.58it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:08<00:00, 20.46it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:08<00:00, 20.46it/s] 

    Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:08<00:00, 20.46it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:08<00:00,  7.02it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=84.47 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=84.47 GB):   2%|▏         | 1/58 [00:00<00:50,  1.13it/s]Capturing num tokens (num_tokens=7680 avail_mem=84.57 GB):   2%|▏         | 1/58 [00:00<00:50,  1.13it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=84.57 GB):   3%|▎         | 2/58 [00:01<00:46,  1.21it/s]Capturing num tokens (num_tokens=7168 avail_mem=84.94 GB):   3%|▎         | 2/58 [00:01<00:46,  1.21it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=84.94 GB):   5%|▌         | 3/58 [00:02<00:41,  1.33it/s]Capturing num tokens (num_tokens=6656 avail_mem=84.71 GB):   5%|▌         | 3/58 [00:02<00:41,  1.33it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=84.71 GB):   7%|▋         | 4/58 [00:02<00:37,  1.42it/s]Capturing num tokens (num_tokens=6144 avail_mem=85.18 GB):   7%|▋         | 4/58 [00:02<00:37,  1.42it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=85.18 GB):   9%|▊         | 5/58 [00:03<00:34,  1.53it/s]Capturing num tokens (num_tokens=5632 avail_mem=84.82 GB):   9%|▊         | 5/58 [00:03<00:34,  1.53it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=84.82 GB):  10%|█         | 6/58 [00:04<00:31,  1.68it/s]Capturing num tokens (num_tokens=5120 avail_mem=85.19 GB):  10%|█         | 6/58 [00:04<00:31,  1.68it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=85.19 GB):  12%|█▏        | 7/58 [00:04<00:28,  1.82it/s]Capturing num tokens (num_tokens=4608 avail_mem=84.93 GB):  12%|█▏        | 7/58 [00:04<00:28,  1.82it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=84.93 GB):  14%|█▍        | 8/58 [00:04<00:24,  2.03it/s]Capturing num tokens (num_tokens=4096 avail_mem=85.19 GB):  14%|█▍        | 8/58 [00:04<00:24,  2.03it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=85.19 GB):  16%|█▌        | 9/58 [00:05<00:21,  2.24it/s]Capturing num tokens (num_tokens=3840 avail_mem=85.19 GB):  16%|█▌        | 9/58 [00:05<00:21,  2.24it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=85.19 GB):  17%|█▋        | 10/58 [00:05<00:18,  2.53it/s]Capturing num tokens (num_tokens=3584 avail_mem=85.06 GB):  17%|█▋        | 10/58 [00:05<00:18,  2.53it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=85.06 GB):  19%|█▉        | 11/58 [00:05<00:16,  2.85it/s]Capturing num tokens (num_tokens=3328 avail_mem=85.18 GB):  19%|█▉        | 11/58 [00:05<00:16,  2.85it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=85.18 GB):  21%|██        | 12/58 [00:05<00:14,  3.25it/s]Capturing num tokens (num_tokens=3072 avail_mem=85.17 GB):  21%|██        | 12/58 [00:05<00:14,  3.25it/s]Capturing num tokens (num_tokens=3072 avail_mem=85.17 GB):  22%|██▏       | 13/58 [00:06<00:12,  3.68it/s]Capturing num tokens (num_tokens=2816 avail_mem=85.17 GB):  22%|██▏       | 13/58 [00:06<00:12,  3.68it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=85.17 GB):  24%|██▍       | 14/58 [00:06<00:10,  4.29it/s]Capturing num tokens (num_tokens=2560 avail_mem=85.16 GB):  24%|██▍       | 14/58 [00:06<00:10,  4.29it/s]Capturing num tokens (num_tokens=2560 avail_mem=85.16 GB):  26%|██▌       | 15/58 [00:06<00:08,  4.83it/s]Capturing num tokens (num_tokens=2304 avail_mem=85.15 GB):  26%|██▌       | 15/58 [00:06<00:08,  4.83it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=85.15 GB):  28%|██▊       | 16/58 [00:06<00:07,  5.51it/s]Capturing num tokens (num_tokens=2048 avail_mem=85.14 GB):  28%|██▊       | 16/58 [00:06<00:07,  5.51it/s]Capturing num tokens (num_tokens=2048 avail_mem=85.14 GB):  29%|██▉       | 17/58 [00:06<00:06,  6.21it/s]Capturing num tokens (num_tokens=1792 avail_mem=85.14 GB):  29%|██▉       | 17/58 [00:06<00:06,  6.21it/s]Capturing num tokens (num_tokens=1536 avail_mem=85.13 GB):  29%|██▉       | 17/58 [00:06<00:06,  6.21it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=85.13 GB):  33%|███▎      | 19/58 [00:06<00:04,  8.10it/s]Capturing num tokens (num_tokens=1280 avail_mem=85.13 GB):  33%|███▎      | 19/58 [00:06<00:04,  8.10it/s]Capturing num tokens (num_tokens=1024 avail_mem=85.12 GB):  33%|███▎      | 19/58 [00:06<00:04,  8.10it/s]Capturing num tokens (num_tokens=1024 avail_mem=85.12 GB):  36%|███▌      | 21/58 [00:06<00:03, 10.29it/s]Capturing num tokens (num_tokens=960 avail_mem=85.11 GB):  36%|███▌      | 21/58 [00:06<00:03, 10.29it/s] Capturing num tokens (num_tokens=896 avail_mem=85.10 GB):  36%|███▌      | 21/58 [00:06<00:03, 10.29it/s]

    Capturing num tokens (num_tokens=896 avail_mem=85.10 GB):  40%|███▉      | 23/58 [00:07<00:03, 11.02it/s]Capturing num tokens (num_tokens=832 avail_mem=84.06 GB):  40%|███▉      | 23/58 [00:07<00:03, 11.02it/s]Capturing num tokens (num_tokens=768 avail_mem=84.05 GB):  40%|███▉      | 23/58 [00:07<00:03, 11.02it/s]

    Capturing num tokens (num_tokens=768 avail_mem=84.05 GB):  43%|████▎     | 25/58 [00:07<00:03,  9.17it/s]Capturing num tokens (num_tokens=704 avail_mem=84.05 GB):  43%|████▎     | 25/58 [00:07<00:03,  9.17it/s]Capturing num tokens (num_tokens=640 avail_mem=85.06 GB):  43%|████▎     | 25/58 [00:07<00:03,  9.17it/s]Capturing num tokens (num_tokens=640 avail_mem=85.06 GB):  47%|████▋     | 27/58 [00:07<00:03,  9.51it/s]Capturing num tokens (num_tokens=576 avail_mem=84.23 GB):  47%|████▋     | 27/58 [00:07<00:03,  9.51it/s]

    Capturing num tokens (num_tokens=512 avail_mem=84.22 GB):  47%|████▋     | 27/58 [00:07<00:03,  9.51it/s]Capturing num tokens (num_tokens=512 avail_mem=84.22 GB):  50%|█████     | 29/58 [00:07<00:03,  8.93it/s]Capturing num tokens (num_tokens=480 avail_mem=84.21 GB):  50%|█████     | 29/58 [00:07<00:03,  8.93it/s]

    Capturing num tokens (num_tokens=448 avail_mem=85.03 GB):  50%|█████     | 29/58 [00:07<00:03,  8.93it/s]Capturing num tokens (num_tokens=448 avail_mem=85.03 GB):  53%|█████▎    | 31/58 [00:07<00:02,  9.36it/s]Capturing num tokens (num_tokens=416 avail_mem=85.02 GB):  53%|█████▎    | 31/58 [00:07<00:02,  9.36it/s]Capturing num tokens (num_tokens=416 avail_mem=85.02 GB):  55%|█████▌    | 32/58 [00:08<00:02,  9.37it/s]Capturing num tokens (num_tokens=384 avail_mem=84.25 GB):  55%|█████▌    | 32/58 [00:08<00:02,  9.37it/s]

    Capturing num tokens (num_tokens=384 avail_mem=84.25 GB):  57%|█████▋    | 33/58 [00:08<00:02,  9.10it/s]Capturing num tokens (num_tokens=352 avail_mem=84.24 GB):  57%|█████▋    | 33/58 [00:08<00:02,  9.10it/s]Capturing num tokens (num_tokens=352 avail_mem=84.24 GB):  59%|█████▊    | 34/58 [00:08<00:02,  9.22it/s]Capturing num tokens (num_tokens=320 avail_mem=85.01 GB):  59%|█████▊    | 34/58 [00:08<00:02,  9.22it/s]

    Capturing num tokens (num_tokens=288 avail_mem=84.92 GB):  59%|█████▊    | 34/58 [00:08<00:02,  9.22it/s]Capturing num tokens (num_tokens=288 avail_mem=84.92 GB):  62%|██████▏   | 36/58 [00:08<00:02,  9.28it/s]Capturing num tokens (num_tokens=256 avail_mem=84.25 GB):  62%|██████▏   | 36/58 [00:08<00:02,  9.28it/s]

    Capturing num tokens (num_tokens=256 avail_mem=84.25 GB):  64%|██████▍   | 37/58 [00:08<00:02,  9.12it/s]Capturing num tokens (num_tokens=240 avail_mem=84.27 GB):  64%|██████▍   | 37/58 [00:08<00:02,  9.12it/s]Capturing num tokens (num_tokens=224 avail_mem=84.97 GB):  64%|██████▍   | 37/58 [00:08<00:02,  9.12it/s]Capturing num tokens (num_tokens=224 avail_mem=84.97 GB):  67%|██████▋   | 39/58 [00:08<00:01,  9.70it/s]Capturing num tokens (num_tokens=208 avail_mem=84.31 GB):  67%|██████▋   | 39/58 [00:08<00:01,  9.70it/s]

    Capturing num tokens (num_tokens=208 avail_mem=84.31 GB):  69%|██████▉   | 40/58 [00:08<00:01,  9.35it/s]Capturing num tokens (num_tokens=192 avail_mem=84.30 GB):  69%|██████▉   | 40/58 [00:08<00:01,  9.35it/s]Capturing num tokens (num_tokens=176 avail_mem=85.01 GB):  69%|██████▉   | 40/58 [00:09<00:01,  9.35it/s]Capturing num tokens (num_tokens=176 avail_mem=85.01 GB):  72%|███████▏  | 42/58 [00:09<00:01, 10.70it/s]Capturing num tokens (num_tokens=160 avail_mem=84.96 GB):  72%|███████▏  | 42/58 [00:09<00:01, 10.70it/s]

    Capturing num tokens (num_tokens=144 avail_mem=84.35 GB):  72%|███████▏  | 42/58 [00:09<00:01, 10.70it/s]Capturing num tokens (num_tokens=144 avail_mem=84.35 GB):  76%|███████▌  | 44/58 [00:09<00:01, 10.65it/s]Capturing num tokens (num_tokens=128 avail_mem=84.36 GB):  76%|███████▌  | 44/58 [00:09<00:01, 10.65it/s]Capturing num tokens (num_tokens=112 avail_mem=84.96 GB):  76%|███████▌  | 44/58 [00:09<00:01, 10.65it/s]

    Capturing num tokens (num_tokens=112 avail_mem=84.96 GB):  79%|███████▉  | 46/58 [00:09<00:01, 11.16it/s]Capturing num tokens (num_tokens=96 avail_mem=84.41 GB):  79%|███████▉  | 46/58 [00:09<00:01, 11.16it/s] Capturing num tokens (num_tokens=80 avail_mem=85.00 GB):  79%|███████▉  | 46/58 [00:09<00:01, 11.16it/s]Capturing num tokens (num_tokens=80 avail_mem=85.00 GB):  83%|████████▎ | 48/58 [00:09<00:00, 11.65it/s]Capturing num tokens (num_tokens=64 avail_mem=84.95 GB):  83%|████████▎ | 48/58 [00:09<00:00, 11.65it/s]

    Capturing num tokens (num_tokens=48 avail_mem=84.46 GB):  83%|████████▎ | 48/58 [00:09<00:00, 11.65it/s]Capturing num tokens (num_tokens=48 avail_mem=84.46 GB):  86%|████████▌ | 50/58 [00:09<00:00, 11.83it/s]Capturing num tokens (num_tokens=32 avail_mem=84.95 GB):  86%|████████▌ | 50/58 [00:09<00:00, 11.83it/s]Capturing num tokens (num_tokens=28 avail_mem=84.51 GB):  86%|████████▌ | 50/58 [00:09<00:00, 11.83it/s]

    Capturing num tokens (num_tokens=28 avail_mem=84.51 GB):  90%|████████▉ | 52/58 [00:09<00:00, 12.09it/s]Capturing num tokens (num_tokens=24 avail_mem=84.51 GB):  90%|████████▉ | 52/58 [00:09<00:00, 12.09it/s]Capturing num tokens (num_tokens=20 avail_mem=84.94 GB):  90%|████████▉ | 52/58 [00:10<00:00, 12.09it/s]Capturing num tokens (num_tokens=20 avail_mem=84.94 GB):  93%|█████████▎| 54/58 [00:10<00:00, 12.35it/s]Capturing num tokens (num_tokens=16 avail_mem=84.52 GB):  93%|█████████▎| 54/58 [00:10<00:00, 12.35it/s]

    Capturing num tokens (num_tokens=12 avail_mem=84.94 GB):  93%|█████████▎| 54/58 [00:10<00:00, 12.35it/s]Capturing num tokens (num_tokens=12 avail_mem=84.94 GB):  97%|█████████▋| 56/58 [00:10<00:00, 12.84it/s]Capturing num tokens (num_tokens=8 avail_mem=84.55 GB):  97%|█████████▋| 56/58 [00:10<00:00, 12.84it/s] Capturing num tokens (num_tokens=4 avail_mem=84.93 GB):  97%|█████████▋| 56/58 [00:10<00:00, 12.84it/s]Capturing num tokens (num_tokens=4 avail_mem=84.93 GB): 100%|██████████| 58/58 [00:10<00:00, 13.57it/s]Capturing num tokens (num_tokens=4 avail_mem=84.93 GB): 100%|██████████| 58/58 [00:10<00:00,  5.60it/s]



<strong style='color: #00008B;'>==== Original Output ====</strong>



<strong style='color: #00008B;'>I need to calculate the sum of 1 and 3.<br><br>First, I'll identify the two numbers involved in the addition.<br><br>Next, I'll add the two numbers together.<br><br>Finally, the result of adding 1 and 3 is 4.<br></think><br><br>To solve the addition problem \(1 + 3\), follow these simple steps:<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>3. **Present the final answer:**<br>   \[<br>   \boxed{4}<br>   \]<br><br>So, the result of \(1 + 3\) is \(\boxed{4}\).</strong>



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>I need to calculate the sum of 1 and 3.<br><br>First, I'll identify the two numbers involved in the addition.<br><br>Next, I'll add the two numbers together.<br><br>Finally, the result of adding 1 and 3 is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>To solve the addition problem \(1 + 3\), follow these simple steps:<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>3. **Present the final answer:**<br>   \[<br>   \boxed{4}<br>   \]<br><br>So, the result of \(1 + 3\) is \(\boxed{4}\).</strong>



```python
llm.shutdown()
```

## Supporting New Reasoning Model Schemas

For future reasoning models, you can implement the reasoning parser as a subclass of `BaseReasoningFormatDetector` in `python/sglang/srt/reasoning_parser.py` and specify the reasoning parser for new reasoning model schemas accordingly.
