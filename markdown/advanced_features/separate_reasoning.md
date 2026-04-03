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
    2026-04-03 08:12:33.441 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 08:12:33] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 08:12:33.441 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 08:12:33] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 08:12:33.441 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 08:12:33] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 08:12:33.441 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 08:12:33] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 08:12:33.441 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 08:12:33] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Multi-thread loading shards:  50% Completed | 1/2 [00:00<00:00,  1.01it/s]

    Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.22s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.19s/it]


    2026-04-03 08:12:42,164 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 08:12:42] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:55,  3.09s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:55,  3.09s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:24,  1.51s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:24,  1.51s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:50,  1.10it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:50,  1.10it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:33,  1.61it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:33,  1.61it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:24,  2.20it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:24,  2.20it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.84it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.84it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:15,  3.37it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:15,  3.37it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:14,  3.43it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:14,  3.43it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:14,  3.41it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:14,  3.41it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:05<00:14,  3.27it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:05<00:14,  3.27it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:05<00:14,  3.27it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:05<00:14,  3.27it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:05<00:13,  3.32it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:05<00:13,  3.32it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:06<00:12,  3.47it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:06<00:12,  3.47it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:06<00:12,  3.61it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:06<00:12,  3.61it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:06<00:11,  3.82it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:06<00:11,  3.82it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:06<00:10,  4.14it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:06<00:10,  4.14it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:06<00:09,  4.46it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:06<00:09,  4.46it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:07<00:08,  4.97it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:07<00:08,  4.97it/s]

    Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:07<00:07,  5.36it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:07<00:07,  5.36it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:07<00:06,  5.76it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:07<00:06,  5.76it/s]

    Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:07<00:05,  6.31it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:07<00:05,  6.31it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:07<00:05,  6.31it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:07<00:04,  7.91it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:07<00:04,  7.91it/s]

    Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:07<00:04,  7.91it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:07<00:03,  8.76it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:07<00:03,  8.76it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:07<00:03,  8.76it/s]

    Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:08<00:03,  9.93it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:08<00:03,  9.93it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:08<00:03,  9.93it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:08<00:02, 10.88it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:08<00:02, 10.88it/s]

    Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:08<00:02, 10.88it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:08<00:02, 11.96it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:08<00:02, 11.96it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:08<00:02, 11.96it/s]

    Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:08<00:02, 11.99it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:08<00:02, 11.99it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:08<00:02, 11.99it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:08<00:01, 12.87it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:08<00:01, 12.87it/s]

    Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:08<00:01, 12.87it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:08<00:01, 13.24it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:08<00:01, 13.24it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:08<00:01, 13.24it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:08<00:01, 14.56it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:08<00:01, 14.56it/s]

    Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:08<00:01, 14.56it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:09<00:01, 15.03it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:09<00:01, 15.03it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:09<00:01, 15.03it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:09<00:00, 15.40it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:09<00:00, 15.40it/s]

    Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:09<00:00, 15.40it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:09<00:00, 15.76it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:09<00:00, 15.76it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:09<00:00, 15.76it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:09<00:00, 16.24it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:09<00:00, 16.24it/s]

    Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:09<00:00, 16.24it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:09<00:00, 16.09it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:09<00:00, 16.09it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:09<00:00, 16.09it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:09<00:00, 16.09it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:09<00:00, 16.09it/s]

    Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:09<00:00, 16.09it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:09<00:00, 16.78it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:09<00:00, 16.78it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:09<00:00, 16.78it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:09<00:00, 16.78it/s]

    Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:09<00:00, 18.23it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:09<00:00, 18.23it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:09<00:00, 18.23it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:09<00:00,  5.83it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=24.59 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=24.59 GB):   2%|▏         | 1/58 [00:00<00:50,  1.12it/s]Capturing num tokens (num_tokens=7680 avail_mem=24.56 GB):   2%|▏         | 1/58 [00:00<00:50,  1.12it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=24.56 GB):   3%|▎         | 2/58 [00:01<00:42,  1.33it/s]Capturing num tokens (num_tokens=7168 avail_mem=24.56 GB):   3%|▎         | 2/58 [00:01<00:42,  1.33it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=24.56 GB):   5%|▌         | 3/58 [00:02<00:36,  1.50it/s]Capturing num tokens (num_tokens=6656 avail_mem=24.56 GB):   5%|▌         | 3/58 [00:02<00:36,  1.50it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=24.56 GB):   7%|▋         | 4/58 [00:02<00:30,  1.77it/s]Capturing num tokens (num_tokens=6144 avail_mem=24.57 GB):   7%|▋         | 4/58 [00:02<00:30,  1.77it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=24.57 GB):   9%|▊         | 5/58 [00:02<00:26,  2.02it/s]Capturing num tokens (num_tokens=5632 avail_mem=23.37 GB):   9%|▊         | 5/58 [00:02<00:26,  2.02it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=23.37 GB):  10%|█         | 6/58 [00:03<00:24,  2.09it/s]Capturing num tokens (num_tokens=5120 avail_mem=24.53 GB):  10%|█         | 6/58 [00:03<00:24,  2.09it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=24.53 GB):  12%|█▏        | 7/58 [00:03<00:23,  2.18it/s]Capturing num tokens (num_tokens=4608 avail_mem=22.52 GB):  12%|█▏        | 7/58 [00:03<00:23,  2.18it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=22.52 GB):  14%|█▍        | 8/58 [00:04<00:23,  2.09it/s]Capturing num tokens (num_tokens=4096 avail_mem=22.59 GB):  14%|█▍        | 8/58 [00:04<00:23,  2.09it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=22.59 GB):  16%|█▌        | 9/58 [00:04<00:22,  2.15it/s]Capturing num tokens (num_tokens=3840 avail_mem=23.70 GB):  16%|█▌        | 9/58 [00:04<00:22,  2.15it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=23.70 GB):  17%|█▋        | 10/58 [00:05<00:22,  2.15it/s]Capturing num tokens (num_tokens=3584 avail_mem=22.84 GB):  17%|█▋        | 10/58 [00:05<00:22,  2.15it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=22.84 GB):  19%|█▉        | 11/58 [00:05<00:20,  2.27it/s]Capturing num tokens (num_tokens=3328 avail_mem=23.74 GB):  19%|█▉        | 11/58 [00:05<00:20,  2.27it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=23.74 GB):  21%|██        | 12/58 [00:05<00:19,  2.37it/s]Capturing num tokens (num_tokens=3072 avail_mem=23.76 GB):  21%|██        | 12/58 [00:05<00:19,  2.37it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=23.76 GB):  22%|██▏       | 13/58 [00:06<00:18,  2.44it/s]Capturing num tokens (num_tokens=2816 avail_mem=23.81 GB):  22%|██▏       | 13/58 [00:06<00:18,  2.44it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=23.81 GB):  24%|██▍       | 14/58 [00:06<00:16,  2.62it/s]Capturing num tokens (num_tokens=2560 avail_mem=23.16 GB):  24%|██▍       | 14/58 [00:06<00:16,  2.62it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=23.16 GB):  26%|██▌       | 15/58 [00:06<00:15,  2.75it/s]Capturing num tokens (num_tokens=2304 avail_mem=24.55 GB):  26%|██▌       | 15/58 [00:06<00:15,  2.75it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=24.55 GB):  28%|██▊       | 16/58 [00:07<00:14,  2.92it/s]Capturing num tokens (num_tokens=2048 avail_mem=23.29 GB):  28%|██▊       | 16/58 [00:07<00:14,  2.92it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=23.29 GB):  29%|██▉       | 17/58 [00:07<00:12,  3.16it/s]Capturing num tokens (num_tokens=1792 avail_mem=23.36 GB):  29%|██▉       | 17/58 [00:07<00:12,  3.16it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=23.36 GB):  31%|███       | 18/58 [00:07<00:11,  3.45it/s]Capturing num tokens (num_tokens=1536 avail_mem=23.94 GB):  31%|███       | 18/58 [00:07<00:11,  3.45it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=23.94 GB):  33%|███▎      | 19/58 [00:08<00:11,  3.54it/s]Capturing num tokens (num_tokens=1280 avail_mem=23.45 GB):  33%|███▎      | 19/58 [00:08<00:11,  3.54it/s]Capturing num tokens (num_tokens=1280 avail_mem=23.45 GB):  34%|███▍      | 20/58 [00:08<00:09,  4.03it/s]Capturing num tokens (num_tokens=1024 avail_mem=24.09 GB):  34%|███▍      | 20/58 [00:08<00:09,  4.03it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=24.09 GB):  36%|███▌      | 21/58 [00:08<00:08,  4.21it/s]Capturing num tokens (num_tokens=960 avail_mem=24.00 GB):  36%|███▌      | 21/58 [00:08<00:08,  4.21it/s] Capturing num tokens (num_tokens=960 avail_mem=24.00 GB):  38%|███▊      | 22/58 [00:08<00:07,  4.60it/s]Capturing num tokens (num_tokens=896 avail_mem=24.12 GB):  38%|███▊      | 22/58 [00:08<00:07,  4.60it/s]

    Capturing num tokens (num_tokens=896 avail_mem=24.12 GB):  40%|███▉      | 23/58 [00:08<00:07,  4.95it/s]Capturing num tokens (num_tokens=832 avail_mem=24.05 GB):  40%|███▉      | 23/58 [00:08<00:07,  4.95it/s]Capturing num tokens (num_tokens=832 avail_mem=24.05 GB):  41%|████▏     | 24/58 [00:08<00:06,  5.00it/s]Capturing num tokens (num_tokens=768 avail_mem=24.55 GB):  41%|████▏     | 24/58 [00:08<00:06,  5.00it/s]

    Capturing num tokens (num_tokens=768 avail_mem=24.55 GB):  43%|████▎     | 25/58 [00:09<00:06,  5.44it/s]Capturing num tokens (num_tokens=704 avail_mem=23.74 GB):  43%|████▎     | 25/58 [00:09<00:06,  5.44it/s]Capturing num tokens (num_tokens=704 avail_mem=23.74 GB):  45%|████▍     | 26/58 [00:09<00:05,  5.73it/s]Capturing num tokens (num_tokens=640 avail_mem=24.54 GB):  45%|████▍     | 26/58 [00:09<00:05,  5.73it/s]

    Capturing num tokens (num_tokens=640 avail_mem=24.54 GB):  47%|████▋     | 27/58 [00:09<00:05,  6.18it/s]Capturing num tokens (num_tokens=576 avail_mem=24.13 GB):  47%|████▋     | 27/58 [00:09<00:05,  6.18it/s]Capturing num tokens (num_tokens=576 avail_mem=24.13 GB):  48%|████▊     | 28/58 [00:09<00:04,  6.13it/s]Capturing num tokens (num_tokens=512 avail_mem=24.23 GB):  48%|████▊     | 28/58 [00:09<00:04,  6.13it/s]

    Capturing num tokens (num_tokens=512 avail_mem=24.23 GB):  50%|█████     | 29/58 [00:09<00:04,  6.35it/s]Capturing num tokens (num_tokens=480 avail_mem=24.14 GB):  50%|█████     | 29/58 [00:09<00:04,  6.35it/s]Capturing num tokens (num_tokens=480 avail_mem=24.14 GB):  52%|█████▏    | 30/58 [00:09<00:04,  6.48it/s]Capturing num tokens (num_tokens=448 avail_mem=24.53 GB):  52%|█████▏    | 30/58 [00:09<00:04,  6.48it/s]

    Capturing num tokens (num_tokens=448 avail_mem=24.53 GB):  53%|█████▎    | 31/58 [00:09<00:04,  6.59it/s]Capturing num tokens (num_tokens=416 avail_mem=24.16 GB):  53%|█████▎    | 31/58 [00:09<00:04,  6.59it/s]Capturing num tokens (num_tokens=416 avail_mem=24.16 GB):  55%|█████▌    | 32/58 [00:10<00:03,  7.33it/s]Capturing num tokens (num_tokens=384 avail_mem=24.31 GB):  55%|█████▌    | 32/58 [00:10<00:03,  7.33it/s]

    Capturing num tokens (num_tokens=384 avail_mem=24.31 GB):  57%|█████▋    | 33/58 [00:10<00:03,  7.78it/s]Capturing num tokens (num_tokens=352 avail_mem=24.18 GB):  57%|█████▋    | 33/58 [00:10<00:03,  7.78it/s]Capturing num tokens (num_tokens=352 avail_mem=24.18 GB):  59%|█████▊    | 34/58 [00:10<00:02,  8.11it/s]Capturing num tokens (num_tokens=320 avail_mem=24.51 GB):  59%|█████▊    | 34/58 [00:10<00:02,  8.11it/s]

    Capturing num tokens (num_tokens=320 avail_mem=24.51 GB):  60%|██████    | 35/58 [00:10<00:02,  8.38it/s]Capturing num tokens (num_tokens=288 avail_mem=24.19 GB):  60%|██████    | 35/58 [00:10<00:02,  8.38it/s]Capturing num tokens (num_tokens=256 avail_mem=24.49 GB):  60%|██████    | 35/58 [00:10<00:02,  8.38it/s]

    Capturing num tokens (num_tokens=256 avail_mem=24.49 GB):  64%|██████▍   | 37/58 [00:10<00:02,  8.95it/s]Capturing num tokens (num_tokens=240 avail_mem=24.48 GB):  64%|██████▍   | 37/58 [00:10<00:02,  8.95it/s]Capturing num tokens (num_tokens=224 avail_mem=24.23 GB):  64%|██████▍   | 37/58 [00:10<00:02,  8.95it/s]

    Capturing num tokens (num_tokens=224 avail_mem=24.23 GB):  67%|██████▋   | 39/58 [00:10<00:02,  8.86it/s]Capturing num tokens (num_tokens=208 avail_mem=24.40 GB):  67%|██████▋   | 39/58 [00:10<00:02,  8.86it/s]Capturing num tokens (num_tokens=192 avail_mem=24.46 GB):  67%|██████▋   | 39/58 [00:10<00:02,  8.86it/s]Capturing num tokens (num_tokens=192 avail_mem=24.46 GB):  71%|███████   | 41/58 [00:11<00:01,  9.26it/s]Capturing num tokens (num_tokens=176 avail_mem=24.40 GB):  71%|███████   | 41/58 [00:11<00:01,  9.26it/s]

    Capturing num tokens (num_tokens=160 avail_mem=24.36 GB):  71%|███████   | 41/58 [00:11<00:01,  9.26it/s]Capturing num tokens (num_tokens=160 avail_mem=24.36 GB):  74%|███████▍  | 43/58 [00:11<00:01, 10.40it/s]Capturing num tokens (num_tokens=144 avail_mem=24.44 GB):  74%|███████▍  | 43/58 [00:11<00:01, 10.40it/s]Capturing num tokens (num_tokens=128 avail_mem=24.28 GB):  74%|███████▍  | 43/58 [00:11<00:01, 10.40it/s]

    Capturing num tokens (num_tokens=128 avail_mem=24.28 GB):  78%|███████▊  | 45/58 [00:11<00:01, 11.67it/s]Capturing num tokens (num_tokens=112 avail_mem=24.44 GB):  78%|███████▊  | 45/58 [00:11<00:01, 11.67it/s]Capturing num tokens (num_tokens=96 avail_mem=24.44 GB):  78%|███████▊  | 45/58 [00:11<00:01, 11.67it/s] Capturing num tokens (num_tokens=96 avail_mem=24.44 GB):  81%|████████  | 47/58 [00:11<00:00, 12.26it/s]Capturing num tokens (num_tokens=80 avail_mem=24.42 GB):  81%|████████  | 47/58 [00:11<00:00, 12.26it/s]Capturing num tokens (num_tokens=64 avail_mem=24.30 GB):  81%|████████  | 47/58 [00:11<00:00, 12.26it/s]

    Capturing num tokens (num_tokens=64 avail_mem=24.30 GB):  84%|████████▍ | 49/58 [00:11<00:00, 13.66it/s]Capturing num tokens (num_tokens=48 avail_mem=24.40 GB):  84%|████████▍ | 49/58 [00:11<00:00, 13.66it/s]Capturing num tokens (num_tokens=32 avail_mem=24.39 GB):  84%|████████▍ | 49/58 [00:11<00:00, 13.66it/s]Capturing num tokens (num_tokens=32 avail_mem=24.39 GB):  88%|████████▊ | 51/58 [00:11<00:00, 14.16it/s]Capturing num tokens (num_tokens=28 avail_mem=24.36 GB):  88%|████████▊ | 51/58 [00:11<00:00, 14.16it/s]Capturing num tokens (num_tokens=24 avail_mem=24.30 GB):  88%|████████▊ | 51/58 [00:11<00:00, 14.16it/s]

    Capturing num tokens (num_tokens=24 avail_mem=24.30 GB):  91%|█████████▏| 53/58 [00:11<00:00, 15.39it/s]Capturing num tokens (num_tokens=20 avail_mem=24.36 GB):  91%|█████████▏| 53/58 [00:11<00:00, 15.39it/s]Capturing num tokens (num_tokens=16 avail_mem=24.36 GB):  91%|█████████▏| 53/58 [00:11<00:00, 15.39it/s]Capturing num tokens (num_tokens=16 avail_mem=24.36 GB):  95%|█████████▍| 55/58 [00:11<00:00, 15.31it/s]Capturing num tokens (num_tokens=12 avail_mem=24.33 GB):  95%|█████████▍| 55/58 [00:11<00:00, 15.31it/s]Capturing num tokens (num_tokens=8 avail_mem=24.28 GB):  95%|█████████▍| 55/58 [00:11<00:00, 15.31it/s] 

    Capturing num tokens (num_tokens=8 avail_mem=24.28 GB):  98%|█████████▊| 57/58 [00:12<00:00, 15.68it/s]Capturing num tokens (num_tokens=4 avail_mem=24.26 GB):  98%|█████████▊| 57/58 [00:12<00:00, 15.68it/s]Capturing num tokens (num_tokens=4 avail_mem=24.26 GB): 100%|██████████| 58/58 [00:12<00:00,  4.81it/s]


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



<strong style='color: #00008B;'>First, I need to add the numbers 1 and 3.<br><br>I'll start with the number 1.<br><br>Then, I'll add 3 to it.<br><br>After performing the addition, the result is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>Sure! Let's solve the addition step by step.<br><br>**Problem:**  <br>What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Start with the first number:**  <br>   \(1\)<br><br>2. **Add the second number:**  <br>   \(1 + 3\)<br><br>3. **Calculate the sum:**  <br>   \(1 + 3 = 4\)<br><br>**Answer:**  <br>\(\boxed{4}\)</strong>


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



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I perform the addition of these two numbers.<br><br>Finally, I arrive at the conclusion that 1 plus 3 equals 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'><br><br>**Solution:**<br><br>To find the sum of 1 and 3, follow these simple steps:<br><br>1. **Start with the number 1.**<br>2. **Add the number 3 to it.**<br>3. **Calculate the total.**<br><br>\[<br>1 + 3 = \boxed{4}<br>\]<br><br>**Final Answer:** \(\boxed{4}\)</strong>


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



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I add the two numbers together: 1 plus 3 equals 4.<br><br>Finally, I state the result clearly.</strong>



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



<strong style='color: #00008B;'>To solve the problem of adding 1 and 3, I start by identifying the two numbers involved in the addition.<br><br>Next, I perform the addition by combining these two numbers together.<br><br>Finally, I calculate the sum to determine the result of the addition.<br></think><br><br>Sure! Let's solve the addition problem step by step.<br><br>**Problem:**  <br>What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:**  <br>\(\boxed{4}\)</strong>


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



<strong style='color: #00008B;'>First, I identify the two numbers in the problem: 1 and 3.<br><br>Next, I add these two numbers together.<br><br>Finally, I calculate the sum to find that 1 plus 3 equals 4.<br></think><br><br>Sure! Let's solve the addition problem step by step.<br><br>**Problem:**  <br>What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**  <br>   We have the numbers 1 and 3.<br><br>2. **Add the numbers together:**  <br>   \[<br>   1 + 3 = 4<br>   \]<br><br>3. **Final Answer:**  <br>   \[<br>   \boxed{4}<br>   \]<br><br>So, \(1 + 3\) equals \(\boxed{4}\).</strong>


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:328: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      return await dependant.call(**values)



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>First, I identify the two numbers in the problem: 1 and 3.<br><br>Next, I add these two numbers together.<br><br>Finally, I calculate the sum to find that 1 plus 3 equals 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>Sure! Let's solve the addition problem step by step.<br><br>**Problem:**  <br>What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**  <br>   We have the numbers 1 and 3.<br><br>2. **Add the numbers together:**  <br>   \[<br>   1 + 3 = 4<br>   \]<br><br>3. **Final Answer:**  <br>   \[<br>   \boxed{4}<br>   \]<br><br>So, \(1 + 3\) equals \(\boxed{4}\).</strong>



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


    2026-04-03 08:13:34.352 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 08:13:34] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 08:13:34.352 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 08:13:34] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 08:13:34.352 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 08:13:34] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 08:13:34.352 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 08:13:34] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 08:13:34.352 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 08:13:34] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Multi-thread loading shards:  50% Completed | 1/2 [00:01<00:01,  1.03s/it]

    Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.36s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.31s/it]


    2026-04-03 08:13:43,403 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 08:13:43] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:03,  3.21s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:03,  3.21s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:04<01:45,  1.88s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:04<01:45,  1.88s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:07,  1.22s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:07,  1.22s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:48,  1.11it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:48,  1.11it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:37,  1.43it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:37,  1.43it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:30,  1.72it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:30,  1.72it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:24,  2.05it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:24,  2.05it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:06<00:20,  2.38it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:06<00:20,  2.38it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:06<00:17,  2.75it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:06<00:17,  2.75it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:06<00:15,  3.15it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:06<00:15,  3.15it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:06<00:13,  3.52it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:06<00:13,  3.52it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:07<00:11,  3.92it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:07<00:11,  3.92it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:07<00:10,  4.33it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:07<00:10,  4.33it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:07<00:10,  4.28it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:07<00:10,  4.28it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:07<00:09,  4.36it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:07<00:09,  4.36it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:07<00:09,  4.54it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:07<00:09,  4.54it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:08<00:08,  4.78it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:08<00:08,  4.78it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:08<00:07,  5.11it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:08<00:07,  5.11it/s]

    Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:08<00:07,  5.55it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:08<00:07,  5.55it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:08<00:06,  5.99it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:08<00:06,  5.99it/s]

    Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:08<00:05,  6.63it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:08<00:05,  6.63it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:08<00:05,  6.63it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:08<00:04,  8.26it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:08<00:04,  8.26it/s]

    Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:08<00:04,  8.26it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:09<00:03,  9.30it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:09<00:03,  9.30it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:09<00:03,  9.30it/s]

    Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:09<00:03, 10.29it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:09<00:03, 10.29it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:09<00:03, 10.29it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:09<00:02, 11.19it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:09<00:02, 11.19it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:09<00:02, 11.19it/s]

    Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:09<00:02, 12.59it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:09<00:02, 12.59it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:09<00:02, 12.59it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:09<00:01, 12.64it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:09<00:01, 12.64it/s]

    Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:09<00:01, 12.64it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:09<00:01, 13.55it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:09<00:01, 13.55it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:09<00:01, 13.55it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:09<00:01, 14.06it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:09<00:01, 14.06it/s]

    Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:09<00:01, 14.06it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:09<00:01, 14.06it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:10<00:01, 15.89it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:10<00:01, 15.89it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:10<00:01, 15.89it/s]

    Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:10<00:00, 16.09it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:10<00:00, 16.09it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:10<00:00, 16.09it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:10<00:00, 16.20it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:10<00:00, 16.20it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:10<00:00, 16.20it/s]

    Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:10<00:00, 16.20it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:10<00:00, 17.23it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:10<00:00, 17.23it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:10<00:00, 17.23it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:10<00:00, 16.90it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:10<00:00, 16.90it/s]

    Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:10<00:00, 16.90it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:10<00:00, 16.73it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:10<00:00, 16.73it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:10<00:00, 16.73it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:10<00:00, 16.73it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:10<00:00, 18.51it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:10<00:00, 18.51it/s]

    Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:10<00:00, 18.51it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:10<00:00, 18.51it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:10<00:00, 19.32it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:10<00:00, 19.32it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:10<00:00,  5.28it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=23.79 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=23.79 GB):   2%|▏         | 1/58 [00:00<00:55,  1.03it/s]Capturing num tokens (num_tokens=7680 avail_mem=23.76 GB):   2%|▏         | 1/58 [00:00<00:55,  1.03it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=23.76 GB):   3%|▎         | 2/58 [00:01<00:48,  1.15it/s]Capturing num tokens (num_tokens=7168 avail_mem=23.76 GB):   3%|▎         | 2/58 [00:01<00:48,  1.15it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=23.76 GB):   5%|▌         | 3/58 [00:02<00:45,  1.22it/s]Capturing num tokens (num_tokens=6656 avail_mem=23.76 GB):   5%|▌         | 3/58 [00:02<00:45,  1.22it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=23.76 GB):   7%|▋         | 4/58 [00:03<00:42,  1.28it/s]Capturing num tokens (num_tokens=6144 avail_mem=23.77 GB):   7%|▋         | 4/58 [00:03<00:42,  1.28it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=23.77 GB):   9%|▊         | 5/58 [00:03<00:37,  1.40it/s]Capturing num tokens (num_tokens=5632 avail_mem=23.77 GB):   9%|▊         | 5/58 [00:03<00:37,  1.40it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=23.77 GB):  10%|█         | 6/58 [00:04<00:32,  1.58it/s]Capturing num tokens (num_tokens=5120 avail_mem=23.77 GB):  10%|█         | 6/58 [00:04<00:32,  1.58it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=23.77 GB):  12%|█▏        | 7/58 [00:04<00:29,  1.75it/s]Capturing num tokens (num_tokens=4608 avail_mem=23.77 GB):  12%|█▏        | 7/58 [00:04<00:29,  1.75it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=23.77 GB):  14%|█▍        | 8/58 [00:05<00:25,  1.95it/s]Capturing num tokens (num_tokens=4096 avail_mem=23.77 GB):  14%|█▍        | 8/58 [00:05<00:25,  1.95it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=23.77 GB):  16%|█▌        | 9/58 [00:05<00:22,  2.20it/s]Capturing num tokens (num_tokens=3840 avail_mem=23.78 GB):  16%|█▌        | 9/58 [00:05<00:22,  2.20it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=23.78 GB):  17%|█▋        | 10/58 [00:05<00:20,  2.32it/s]Capturing num tokens (num_tokens=3584 avail_mem=22.74 GB):  17%|█▋        | 10/58 [00:05<00:20,  2.32it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=22.74 GB):  19%|█▉        | 11/58 [00:06<00:20,  2.31it/s]Capturing num tokens (num_tokens=3328 avail_mem=22.74 GB):  19%|█▉        | 11/58 [00:06<00:20,  2.31it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=22.74 GB):  21%|██        | 12/58 [00:06<00:18,  2.44it/s]Capturing num tokens (num_tokens=3072 avail_mem=23.74 GB):  21%|██        | 12/58 [00:06<00:18,  2.44it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=23.74 GB):  22%|██▏       | 13/58 [00:07<00:18,  2.47it/s]Capturing num tokens (num_tokens=2816 avail_mem=22.92 GB):  22%|██▏       | 13/58 [00:07<00:18,  2.47it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=22.92 GB):  24%|██▍       | 14/58 [00:07<00:17,  2.57it/s]Capturing num tokens (num_tokens=2560 avail_mem=23.75 GB):  24%|██▍       | 14/58 [00:07<00:17,  2.57it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=23.75 GB):  26%|██▌       | 15/58 [00:07<00:15,  2.77it/s]Capturing num tokens (num_tokens=2304 avail_mem=22.98 GB):  26%|██▌       | 15/58 [00:07<00:15,  2.77it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=22.98 GB):  28%|██▊       | 16/58 [00:08<00:14,  2.84it/s]Capturing num tokens (num_tokens=2048 avail_mem=22.98 GB):  28%|██▊       | 16/58 [00:08<00:14,  2.84it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=22.98 GB):  29%|██▉       | 17/58 [00:08<00:13,  2.97it/s]Capturing num tokens (num_tokens=1792 avail_mem=23.75 GB):  29%|██▉       | 17/58 [00:08<00:13,  2.97it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=23.75 GB):  31%|███       | 18/58 [00:08<00:12,  3.11it/s]Capturing num tokens (num_tokens=1536 avail_mem=23.04 GB):  31%|███       | 18/58 [00:08<00:12,  3.11it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=23.04 GB):  33%|███▎      | 19/58 [00:08<00:12,  3.25it/s]Capturing num tokens (num_tokens=1280 avail_mem=23.76 GB):  33%|███▎      | 19/58 [00:08<00:12,  3.25it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=23.76 GB):  34%|███▍      | 20/58 [00:09<00:10,  3.54it/s]Capturing num tokens (num_tokens=1024 avail_mem=23.09 GB):  34%|███▍      | 20/58 [00:09<00:10,  3.54it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=23.09 GB):  36%|███▌      | 21/58 [00:09<00:09,  3.72it/s]Capturing num tokens (num_tokens=960 avail_mem=23.09 GB):  36%|███▌      | 21/58 [00:09<00:09,  3.72it/s] 

    Capturing num tokens (num_tokens=960 avail_mem=23.09 GB):  38%|███▊      | 22/58 [00:09<00:09,  3.93it/s]Capturing num tokens (num_tokens=896 avail_mem=23.75 GB):  38%|███▊      | 22/58 [00:09<00:09,  3.93it/s]

    Capturing num tokens (num_tokens=896 avail_mem=23.75 GB):  40%|███▉      | 23/58 [00:09<00:08,  4.10it/s]Capturing num tokens (num_tokens=832 avail_mem=23.14 GB):  40%|███▉      | 23/58 [00:09<00:08,  4.10it/s]

    Capturing num tokens (num_tokens=832 avail_mem=23.14 GB):  41%|████▏     | 24/58 [00:10<00:08,  4.02it/s]Capturing num tokens (num_tokens=768 avail_mem=23.14 GB):  41%|████▏     | 24/58 [00:10<00:08,  4.02it/s]Capturing num tokens (num_tokens=768 avail_mem=23.14 GB):  43%|████▎     | 25/58 [00:10<00:07,  4.33it/s]Capturing num tokens (num_tokens=704 avail_mem=23.19 GB):  43%|████▎     | 25/58 [00:10<00:07,  4.33it/s]

    Capturing num tokens (num_tokens=704 avail_mem=23.19 GB):  45%|████▍     | 26/58 [00:10<00:07,  4.39it/s]Capturing num tokens (num_tokens=640 avail_mem=23.19 GB):  45%|████▍     | 26/58 [00:10<00:07,  4.39it/s]Capturing num tokens (num_tokens=640 avail_mem=23.19 GB):  47%|████▋     | 27/58 [00:10<00:06,  4.64it/s]Capturing num tokens (num_tokens=576 avail_mem=23.74 GB):  47%|████▋     | 27/58 [00:10<00:06,  4.64it/s]

    Capturing num tokens (num_tokens=576 avail_mem=23.74 GB):  48%|████▊     | 28/58 [00:10<00:06,  4.78it/s]Capturing num tokens (num_tokens=512 avail_mem=23.24 GB):  48%|████▊     | 28/58 [00:10<00:06,  4.78it/s]Capturing num tokens (num_tokens=512 avail_mem=23.24 GB):  50%|█████     | 29/58 [00:11<00:05,  4.94it/s]Capturing num tokens (num_tokens=480 avail_mem=23.73 GB):  50%|█████     | 29/58 [00:11<00:05,  4.94it/s]

    Capturing num tokens (num_tokens=480 avail_mem=23.73 GB):  52%|█████▏    | 30/58 [00:11<00:05,  5.32it/s]Capturing num tokens (num_tokens=448 avail_mem=23.29 GB):  52%|█████▏    | 30/58 [00:11<00:05,  5.32it/s]

    Capturing num tokens (num_tokens=448 avail_mem=23.29 GB):  53%|█████▎    | 31/58 [00:11<00:05,  5.08it/s]Capturing num tokens (num_tokens=416 avail_mem=23.73 GB):  53%|█████▎    | 31/58 [00:11<00:05,  5.08it/s]Capturing num tokens (num_tokens=416 avail_mem=23.73 GB):  55%|█████▌    | 32/58 [00:11<00:04,  5.47it/s]Capturing num tokens (num_tokens=384 avail_mem=23.31 GB):  55%|█████▌    | 32/58 [00:11<00:04,  5.47it/s]

    Capturing num tokens (num_tokens=384 avail_mem=23.31 GB):  57%|█████▋    | 33/58 [00:11<00:04,  5.33it/s]Capturing num tokens (num_tokens=352 avail_mem=23.31 GB):  57%|█████▋    | 33/58 [00:11<00:04,  5.33it/s]Capturing num tokens (num_tokens=352 avail_mem=23.31 GB):  59%|█████▊    | 34/58 [00:11<00:04,  5.76it/s]Capturing num tokens (num_tokens=320 avail_mem=23.72 GB):  59%|█████▊    | 34/58 [00:11<00:04,  5.76it/s]

    Capturing num tokens (num_tokens=320 avail_mem=23.72 GB):  60%|██████    | 35/58 [00:12<00:04,  5.64it/s]Capturing num tokens (num_tokens=288 avail_mem=23.33 GB):  60%|██████    | 35/58 [00:12<00:04,  5.64it/s]Capturing num tokens (num_tokens=288 avail_mem=23.33 GB):  62%|██████▏   | 36/58 [00:12<00:03,  5.91it/s]Capturing num tokens (num_tokens=256 avail_mem=23.71 GB):  62%|██████▏   | 36/58 [00:12<00:03,  5.91it/s]

    Capturing num tokens (num_tokens=256 avail_mem=23.71 GB):  64%|██████▍   | 37/58 [00:12<00:03,  5.88it/s]Capturing num tokens (num_tokens=240 avail_mem=23.35 GB):  64%|██████▍   | 37/58 [00:12<00:03,  5.88it/s]Capturing num tokens (num_tokens=240 avail_mem=23.35 GB):  66%|██████▌   | 38/58 [00:12<00:03,  6.52it/s]Capturing num tokens (num_tokens=224 avail_mem=23.70 GB):  66%|██████▌   | 38/58 [00:12<00:03,  6.52it/s]

    Capturing num tokens (num_tokens=224 avail_mem=23.70 GB):  67%|██████▋   | 39/58 [00:12<00:02,  6.56it/s]Capturing num tokens (num_tokens=208 avail_mem=23.37 GB):  67%|██████▋   | 39/58 [00:12<00:02,  6.56it/s]Capturing num tokens (num_tokens=208 avail_mem=23.37 GB):  69%|██████▉   | 40/58 [00:12<00:02,  6.87it/s]Capturing num tokens (num_tokens=192 avail_mem=23.70 GB):  69%|██████▉   | 40/58 [00:12<00:02,  6.87it/s]

    Capturing num tokens (num_tokens=192 avail_mem=23.70 GB):  71%|███████   | 41/58 [00:12<00:02,  6.63it/s]Capturing num tokens (num_tokens=176 avail_mem=23.39 GB):  71%|███████   | 41/58 [00:12<00:02,  6.63it/s]Capturing num tokens (num_tokens=176 avail_mem=23.39 GB):  72%|███████▏  | 42/58 [00:13<00:02,  6.85it/s]Capturing num tokens (num_tokens=160 avail_mem=23.69 GB):  72%|███████▏  | 42/58 [00:13<00:02,  6.85it/s]

    Capturing num tokens (num_tokens=160 avail_mem=23.69 GB):  74%|███████▍  | 43/58 [00:13<00:02,  6.58it/s]Capturing num tokens (num_tokens=144 avail_mem=23.68 GB):  74%|███████▍  | 43/58 [00:13<00:02,  6.58it/s]Capturing num tokens (num_tokens=144 avail_mem=23.68 GB):  76%|███████▌  | 44/58 [00:13<00:02,  6.95it/s]Capturing num tokens (num_tokens=128 avail_mem=23.44 GB):  76%|███████▌  | 44/58 [00:13<00:02,  6.95it/s]

    Capturing num tokens (num_tokens=128 avail_mem=23.44 GB):  78%|███████▊  | 45/58 [00:13<00:01,  6.78it/s]Capturing num tokens (num_tokens=112 avail_mem=23.69 GB):  78%|███████▊  | 45/58 [00:13<00:01,  6.78it/s]Capturing num tokens (num_tokens=112 avail_mem=23.69 GB):  79%|███████▉  | 46/58 [00:13<00:01,  6.91it/s]Capturing num tokens (num_tokens=96 avail_mem=23.46 GB):  79%|███████▉  | 46/58 [00:13<00:01,  6.91it/s] 

    Capturing num tokens (num_tokens=96 avail_mem=23.46 GB):  81%|████████  | 47/58 [00:13<00:01,  6.39it/s]Capturing num tokens (num_tokens=80 avail_mem=23.68 GB):  81%|████████  | 47/58 [00:13<00:01,  6.39it/s]

    Capturing num tokens (num_tokens=80 avail_mem=23.68 GB):  83%|████████▎ | 48/58 [00:14<00:02,  3.84it/s]Capturing num tokens (num_tokens=64 avail_mem=23.67 GB):  83%|████████▎ | 48/58 [00:14<00:02,  3.84it/s]Capturing num tokens (num_tokens=64 avail_mem=23.67 GB):  84%|████████▍ | 49/58 [00:14<00:02,  4.45it/s]Capturing num tokens (num_tokens=48 avail_mem=23.67 GB):  84%|████████▍ | 49/58 [00:14<00:02,  4.45it/s]

    Capturing num tokens (num_tokens=48 avail_mem=23.67 GB):  86%|████████▌ | 50/58 [00:14<00:01,  5.17it/s]Capturing num tokens (num_tokens=32 avail_mem=23.53 GB):  86%|████████▌ | 50/58 [00:14<00:01,  5.17it/s]Capturing num tokens (num_tokens=32 avail_mem=23.53 GB):  88%|████████▊ | 51/58 [00:14<00:01,  5.81it/s]Capturing num tokens (num_tokens=28 avail_mem=23.67 GB):  88%|████████▊ | 51/58 [00:14<00:01,  5.81it/s]

    Capturing num tokens (num_tokens=28 avail_mem=23.67 GB):  90%|████████▉ | 52/58 [00:14<00:00,  6.31it/s]Capturing num tokens (num_tokens=24 avail_mem=23.66 GB):  90%|████████▉ | 52/58 [00:14<00:00,  6.31it/s]Capturing num tokens (num_tokens=24 avail_mem=23.66 GB):  91%|█████████▏| 53/58 [00:15<00:00,  6.57it/s]Capturing num tokens (num_tokens=20 avail_mem=23.65 GB):  91%|█████████▏| 53/58 [00:15<00:00,  6.57it/s]

    Capturing num tokens (num_tokens=20 avail_mem=23.65 GB):  93%|█████████▎| 54/58 [00:15<00:00,  6.66it/s]Capturing num tokens (num_tokens=16 avail_mem=23.54 GB):  93%|█████████▎| 54/58 [00:15<00:00,  6.66it/s]Capturing num tokens (num_tokens=16 avail_mem=23.54 GB):  95%|█████████▍| 55/58 [00:15<00:00,  7.20it/s]Capturing num tokens (num_tokens=12 avail_mem=23.55 GB):  95%|█████████▍| 55/58 [00:15<00:00,  7.20it/s]

    Capturing num tokens (num_tokens=12 avail_mem=23.55 GB):  97%|█████████▋| 56/58 [00:15<00:00,  7.35it/s]Capturing num tokens (num_tokens=8 avail_mem=23.63 GB):  97%|█████████▋| 56/58 [00:15<00:00,  7.35it/s] Capturing num tokens (num_tokens=8 avail_mem=23.63 GB):  98%|█████████▊| 57/58 [00:15<00:00,  7.53it/s]Capturing num tokens (num_tokens=4 avail_mem=23.63 GB):  98%|█████████▊| 57/58 [00:15<00:00,  7.53it/s]

    Capturing num tokens (num_tokens=4 avail_mem=23.63 GB): 100%|██████████| 58/58 [00:15<00:00,  7.62it/s]Capturing num tokens (num_tokens=4 avail_mem=23.63 GB): 100%|██████████| 58/58 [00:15<00:00,  3.71it/s]



<strong style='color: #00008B;'>==== Original Output ====</strong>



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of the numbers 1 and 3.<br><br>I start by identifying the two numbers to be added: 1 and 3.<br><br>Next, I perform the addition: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></think><br><br>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Perform the addition:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Final Answer:**<br>\[<br>\boxed{4}<br>\]</strong>



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of the numbers 1 and 3.<br><br>I start by identifying the two numbers to be added: 1 and 3.<br><br>Next, I perform the addition: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Perform the addition:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Final Answer:**<br>\[<br>\boxed{4}<br>\]</strong>



```python
llm.shutdown()
```

## Supporting New Reasoning Model Schemas

For future reasoning models, you can implement the reasoning parser as a subclass of `BaseReasoningFormatDetector` in `python/sglang/srt/reasoning_parser.py` and specify the reasoning parser for new reasoning model schemas accordingly.
