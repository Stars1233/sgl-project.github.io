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


    2026-04-01 20:58:51.671 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 20:58:51] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 20:58:51.671 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 20:58:51] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 20:58:51.671 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 20:58:51] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 20:58:51.671 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 20:58:51] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 20:58:51.671 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 20:58:51] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:01<00:01,  1.16s/it]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.44s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.40s/it]


    2026-04-01 20:58:57,291 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 20:58:57] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:49,  2.98s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:49,  2.98s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:39,  1.77s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:39,  1.77s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:05,  1.20s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:05,  1.20s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:05<00:52,  1.04it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:05<00:52,  1.04it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:42,  1.24it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:42,  1.24it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:06<00:36,  1.42it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:06<00:36,  1.42it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:06<00:31,  1.62it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:06<00:31,  1.62it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:06<00:27,  1.81it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:06<00:27,  1.81it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:07<00:23,  2.05it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:07<00:23,  2.05it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:07<00:21,  2.28it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:07<00:21,  2.28it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:07<00:18,  2.52it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:07<00:18,  2.52it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:08<00:16,  2.76it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:08<00:16,  2.76it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:08<00:14,  3.05it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:08<00:14,  3.05it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:08<00:13,  3.30it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:08<00:13,  3.30it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:08<00:11,  3.67it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:08<00:11,  3.67it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:09<00:10,  4.04it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:09<00:10,  4.04it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:09<00:09,  4.50it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:09<00:09,  4.50it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:09<00:08,  4.97it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:09<00:08,  4.97it/s]

    Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:09<00:06,  5.62it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:09<00:06,  5.62it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:09<00:06,  6.13it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:09<00:06,  6.13it/s]

    Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:09<00:05,  6.75it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:09<00:05,  6.75it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:09<00:05,  6.75it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:09<00:04,  8.62it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:09<00:04,  8.62it/s]

    Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:09<00:04,  8.62it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:10<00:03, 10.12it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:10<00:03, 10.12it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:10<00:03, 10.12it/s]

    Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:10<00:02, 11.19it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:10<00:02, 11.19it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:10<00:02, 11.19it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:10<00:02, 12.21it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:10<00:02, 12.21it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:10<00:02, 12.21it/s]

    Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:10<00:02, 13.45it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:10<00:02, 13.45it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:10<00:02, 13.45it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:10<00:01, 14.02it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:10<00:01, 14.02it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:10<00:01, 14.02it/s]

    Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:10<00:01, 15.21it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:10<00:01, 15.21it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:10<00:01, 15.21it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:10<00:01, 15.66it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:10<00:01, 15.66it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:10<00:01, 15.66it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:10<00:01, 15.66it/s]

    Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:10<00:01, 17.76it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:10<00:01, 17.76it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:10<00:01, 17.76it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:11<00:00, 17.94it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:11<00:00, 17.94it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:11<00:00, 17.94it/s]

    Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:11<00:00, 17.94it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:11<00:00, 18.98it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:11<00:00, 18.98it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:11<00:00, 18.98it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:11<00:00, 18.98it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:11<00:00, 20.02it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:11<00:00, 20.02it/s]

    Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:11<00:00, 20.02it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:11<00:00, 20.02it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:11<00:00, 21.82it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:11<00:00, 21.82it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:11<00:00, 21.82it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:11<00:00, 21.82it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:11<00:00, 23.13it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:11<00:00, 23.13it/s]

    Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:11<00:00, 23.13it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:11<00:00, 23.13it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:11<00:00, 23.13it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:11<00:00, 26.54it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:11<00:00,  4.97it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=23.40 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=23.40 GB):   2%|▏         | 1/58 [00:00<00:42,  1.35it/s]Capturing num tokens (num_tokens=7680 avail_mem=23.53 GB):   2%|▏         | 1/58 [00:00<00:42,  1.35it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=23.53 GB):   3%|▎         | 2/58 [00:01<00:40,  1.40it/s]Capturing num tokens (num_tokens=7168 avail_mem=22.57 GB):   3%|▎         | 2/58 [00:01<00:40,  1.40it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=22.57 GB):   5%|▌         | 3/58 [00:02<00:41,  1.33it/s]Capturing num tokens (num_tokens=6656 avail_mem=23.64 GB):   5%|▌         | 3/58 [00:02<00:41,  1.33it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=23.64 GB):   7%|▋         | 4/58 [00:02<00:39,  1.37it/s]Capturing num tokens (num_tokens=6144 avail_mem=22.90 GB):   7%|▋         | 4/58 [00:02<00:39,  1.37it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=22.90 GB):   9%|▊         | 5/58 [00:03<00:36,  1.46it/s]Capturing num tokens (num_tokens=5632 avail_mem=23.03 GB):   9%|▊         | 5/58 [00:03<00:36,  1.46it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=23.03 GB):  10%|█         | 6/58 [00:04<00:33,  1.54it/s]Capturing num tokens (num_tokens=5120 avail_mem=23.86 GB):  10%|█         | 6/58 [00:04<00:33,  1.54it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=23.86 GB):  12%|█▏        | 7/58 [00:04<00:31,  1.63it/s]Capturing num tokens (num_tokens=4608 avail_mem=23.23 GB):  12%|█▏        | 7/58 [00:04<00:31,  1.63it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=23.23 GB):  14%|█▍        | 8/58 [00:05<00:28,  1.76it/s]Capturing num tokens (num_tokens=4096 avail_mem=23.36 GB):  14%|█▍        | 8/58 [00:05<00:28,  1.76it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=23.36 GB):  16%|█▌        | 9/58 [00:05<00:25,  1.90it/s]Capturing num tokens (num_tokens=3840 avail_mem=24.06 GB):  16%|█▌        | 9/58 [00:05<00:25,  1.90it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=24.06 GB):  17%|█▋        | 10/58 [00:05<00:23,  2.03it/s]Capturing num tokens (num_tokens=3584 avail_mem=23.48 GB):  17%|█▋        | 10/58 [00:05<00:23,  2.03it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=23.48 GB):  19%|█▉        | 11/58 [00:06<00:21,  2.21it/s]Capturing num tokens (num_tokens=3328 avail_mem=23.58 GB):  19%|█▉        | 11/58 [00:06<00:21,  2.21it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=23.58 GB):  21%|██        | 12/58 [00:06<00:19,  2.42it/s]Capturing num tokens (num_tokens=3072 avail_mem=24.16 GB):  21%|██        | 12/58 [00:06<00:19,  2.42it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=24.16 GB):  22%|██▏       | 13/58 [00:06<00:17,  2.60it/s]Capturing num tokens (num_tokens=2816 avail_mem=24.20 GB):  22%|██▏       | 13/58 [00:06<00:17,  2.60it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=24.20 GB):  24%|██▍       | 14/58 [00:07<00:15,  2.80it/s]Capturing num tokens (num_tokens=2560 avail_mem=23.79 GB):  24%|██▍       | 14/58 [00:07<00:15,  2.80it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=23.79 GB):  26%|██▌       | 15/58 [00:07<00:13,  3.12it/s]Capturing num tokens (num_tokens=2304 avail_mem=23.85 GB):  26%|██▌       | 15/58 [00:07<00:13,  3.12it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=23.85 GB):  28%|██▊       | 16/58 [00:07<00:12,  3.39it/s]Capturing num tokens (num_tokens=2048 avail_mem=24.30 GB):  28%|██▊       | 16/58 [00:07<00:12,  3.39it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=24.30 GB):  29%|██▉       | 17/58 [00:07<00:11,  3.66it/s]Capturing num tokens (num_tokens=1792 avail_mem=23.95 GB):  29%|██▉       | 17/58 [00:07<00:11,  3.66it/s]Capturing num tokens (num_tokens=1792 avail_mem=23.95 GB):  31%|███       | 18/58 [00:08<00:09,  4.11it/s]Capturing num tokens (num_tokens=1536 avail_mem=23.99 GB):  31%|███       | 18/58 [00:08<00:09,  4.11it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=23.99 GB):  33%|███▎      | 19/58 [00:08<00:08,  4.48it/s]Capturing num tokens (num_tokens=1280 avail_mem=24.56 GB):  33%|███▎      | 19/58 [00:08<00:08,  4.48it/s]Capturing num tokens (num_tokens=1280 avail_mem=24.56 GB):  34%|███▍      | 20/58 [00:08<00:07,  4.96it/s]Capturing num tokens (num_tokens=1024 avail_mem=24.23 GB):  34%|███▍      | 20/58 [00:08<00:07,  4.96it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=24.23 GB):  36%|███▌      | 21/58 [00:08<00:06,  5.75it/s]Capturing num tokens (num_tokens=960 avail_mem=24.55 GB):  36%|███▌      | 21/58 [00:08<00:06,  5.75it/s] Capturing num tokens (num_tokens=960 avail_mem=24.55 GB):  38%|███▊      | 22/58 [00:08<00:06,  5.68it/s]Capturing num tokens (num_tokens=896 avail_mem=24.54 GB):  38%|███▊      | 22/58 [00:08<00:06,  5.68it/s]

    Capturing num tokens (num_tokens=896 avail_mem=24.54 GB):  40%|███▉      | 23/58 [00:08<00:05,  6.34it/s]Capturing num tokens (num_tokens=832 avail_mem=24.26 GB):  40%|███▉      | 23/58 [00:08<00:05,  6.34it/s]Capturing num tokens (num_tokens=832 avail_mem=24.26 GB):  41%|████▏     | 24/58 [00:08<00:04,  7.08it/s]Capturing num tokens (num_tokens=768 avail_mem=24.52 GB):  41%|████▏     | 24/58 [00:08<00:04,  7.08it/s]

    Capturing num tokens (num_tokens=768 avail_mem=24.52 GB):  43%|████▎     | 25/58 [00:09<00:04,  7.38it/s]Capturing num tokens (num_tokens=704 avail_mem=24.52 GB):  43%|████▎     | 25/58 [00:09<00:04,  7.38it/s]Capturing num tokens (num_tokens=704 avail_mem=24.52 GB):  45%|████▍     | 26/58 [00:09<00:04,  8.00it/s]Capturing num tokens (num_tokens=640 avail_mem=24.29 GB):  45%|████▍     | 26/58 [00:09<00:04,  8.00it/s]Capturing num tokens (num_tokens=576 avail_mem=24.31 GB):  45%|████▍     | 26/58 [00:09<00:04,  8.00it/s]

    Capturing num tokens (num_tokens=576 avail_mem=24.31 GB):  48%|████▊     | 28/58 [00:09<00:03,  9.30it/s]Capturing num tokens (num_tokens=512 avail_mem=24.49 GB):  48%|████▊     | 28/58 [00:09<00:03,  9.30it/s]Capturing num tokens (num_tokens=480 avail_mem=24.49 GB):  48%|████▊     | 28/58 [00:09<00:03,  9.30it/s]Capturing num tokens (num_tokens=480 avail_mem=24.49 GB):  52%|█████▏    | 30/58 [00:09<00:02, 10.14it/s]Capturing num tokens (num_tokens=448 avail_mem=24.34 GB):  52%|█████▏    | 30/58 [00:09<00:02, 10.14it/s]

    Capturing num tokens (num_tokens=416 avail_mem=24.46 GB):  52%|█████▏    | 30/58 [00:09<00:02, 10.14it/s]Capturing num tokens (num_tokens=416 avail_mem=24.46 GB):  55%|█████▌    | 32/58 [00:09<00:02, 11.53it/s]Capturing num tokens (num_tokens=384 avail_mem=24.46 GB):  55%|█████▌    | 32/58 [00:09<00:02, 11.53it/s]Capturing num tokens (num_tokens=352 avail_mem=24.45 GB):  55%|█████▌    | 32/58 [00:09<00:02, 11.53it/s]

    Capturing num tokens (num_tokens=352 avail_mem=24.45 GB):  59%|█████▊    | 34/58 [00:09<00:01, 12.45it/s]Capturing num tokens (num_tokens=320 avail_mem=24.44 GB):  59%|█████▊    | 34/58 [00:09<00:01, 12.45it/s]Capturing num tokens (num_tokens=288 avail_mem=24.34 GB):  59%|█████▊    | 34/58 [00:09<00:01, 12.45it/s]Capturing num tokens (num_tokens=288 avail_mem=24.34 GB):  62%|██████▏   | 36/58 [00:09<00:01, 13.84it/s]Capturing num tokens (num_tokens=256 avail_mem=24.33 GB):  62%|██████▏   | 36/58 [00:09<00:01, 13.84it/s]Capturing num tokens (num_tokens=240 avail_mem=24.40 GB):  62%|██████▏   | 36/58 [00:09<00:01, 13.84it/s]

    Capturing num tokens (num_tokens=240 avail_mem=24.40 GB):  66%|██████▌   | 38/58 [00:10<00:01, 14.81it/s]Capturing num tokens (num_tokens=224 avail_mem=24.40 GB):  66%|██████▌   | 38/58 [00:10<00:01, 14.81it/s]Capturing num tokens (num_tokens=208 avail_mem=24.38 GB):  66%|██████▌   | 38/58 [00:10<00:01, 14.81it/s]Capturing num tokens (num_tokens=208 avail_mem=24.38 GB):  69%|██████▉   | 40/58 [00:10<00:01, 15.66it/s]Capturing num tokens (num_tokens=192 avail_mem=24.37 GB):  69%|██████▉   | 40/58 [00:10<00:01, 15.66it/s]Capturing num tokens (num_tokens=176 avail_mem=24.36 GB):  69%|██████▉   | 40/58 [00:10<00:01, 15.66it/s]

    Capturing num tokens (num_tokens=176 avail_mem=24.36 GB):  72%|███████▏  | 42/58 [00:10<00:00, 16.47it/s]Capturing num tokens (num_tokens=160 avail_mem=24.37 GB):  72%|███████▏  | 42/58 [00:10<00:00, 16.47it/s]Capturing num tokens (num_tokens=144 avail_mem=24.31 GB):  72%|███████▏  | 42/58 [00:10<00:00, 16.47it/s]Capturing num tokens (num_tokens=128 avail_mem=24.35 GB):  72%|███████▏  | 42/58 [00:10<00:00, 16.47it/s]Capturing num tokens (num_tokens=128 avail_mem=24.35 GB):  78%|███████▊  | 45/58 [00:10<00:00, 17.72it/s]Capturing num tokens (num_tokens=112 avail_mem=24.34 GB):  78%|███████▊  | 45/58 [00:10<00:00, 17.72it/s]Capturing num tokens (num_tokens=96 avail_mem=24.33 GB):  78%|███████▊  | 45/58 [00:10<00:00, 17.72it/s] 

    Capturing num tokens (num_tokens=96 avail_mem=24.33 GB):  81%|████████  | 47/58 [00:10<00:00, 18.27it/s]Capturing num tokens (num_tokens=80 avail_mem=24.32 GB):  81%|████████  | 47/58 [00:10<00:00, 18.27it/s]Capturing num tokens (num_tokens=64 avail_mem=24.31 GB):  81%|████████  | 47/58 [00:10<00:00, 18.27it/s]Capturing num tokens (num_tokens=48 avail_mem=24.30 GB):  81%|████████  | 47/58 [00:10<00:00, 18.27it/s]Capturing num tokens (num_tokens=48 avail_mem=24.30 GB):  86%|████████▌ | 50/58 [00:10<00:00, 19.04it/s]Capturing num tokens (num_tokens=32 avail_mem=24.29 GB):  86%|████████▌ | 50/58 [00:10<00:00, 19.04it/s]Capturing num tokens (num_tokens=28 avail_mem=24.28 GB):  86%|████████▌ | 50/58 [00:10<00:00, 19.04it/s]

    Capturing num tokens (num_tokens=24 avail_mem=24.27 GB):  86%|████████▌ | 50/58 [00:10<00:00, 19.04it/s]Capturing num tokens (num_tokens=24 avail_mem=24.27 GB):  91%|█████████▏| 53/58 [00:10<00:00, 19.67it/s]Capturing num tokens (num_tokens=20 avail_mem=24.25 GB):  91%|█████████▏| 53/58 [00:10<00:00, 19.67it/s]Capturing num tokens (num_tokens=16 avail_mem=24.23 GB):  91%|█████████▏| 53/58 [00:10<00:00, 19.67it/s]Capturing num tokens (num_tokens=12 avail_mem=24.23 GB):  91%|█████████▏| 53/58 [00:10<00:00, 19.67it/s]Capturing num tokens (num_tokens=12 avail_mem=24.23 GB):  97%|█████████▋| 56/58 [00:10<00:00, 20.39it/s]Capturing num tokens (num_tokens=8 avail_mem=24.22 GB):  97%|█████████▋| 56/58 [00:10<00:00, 20.39it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=24.21 GB):  97%|█████████▋| 56/58 [00:10<00:00, 20.39it/s]Capturing num tokens (num_tokens=4 avail_mem=24.21 GB): 100%|██████████| 58/58 [00:11<00:00,  5.26it/s]


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



<strong style='color: #00008B;'>To solve the problem of adding 1 and 3, I start by identifying the two numbers involved.<br><br>Next, I perform the addition operation by combining these two numbers.<br><br>Finally, I calculate the sum to find the result of 1 plus 3.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>**Solution:**<br><br>We are asked to find the sum of \(1\) and \(3\).<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Perform the addition:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Final Answer:**<br>\[<br>\boxed{4}<br>\]</strong>


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



<strong style='color: #00008B;'>I need to calculate the sum of 1 and 3.<br><br>Adding these two numbers together gives 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'><br><br>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>\[<br>1 + 3 = \boxed{4}<br>\]</strong>


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



<strong style='color: #00008B;'><br><br>**Solution:**<br><br>To find the sum of \(1\) and \(3\), follow these simple steps:<br><br>1. **Start with the first number:**<br>   \[<br>   1<br>   \]<br><br>2. **Add the second number:**<br>   \[<br>   1 + 3<br>   \]<br><br>3. **Calculate the sum:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Final Answer:**<br>\[<br>\boxed{4}<br>\]</strong>


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



<strong style='color: #00008B;'>To solve the problem of adding 1 and 3, I start by identifying the two numbers involved.<br><br>Next, I add these numbers together to find their sum.<br><br>Finally, I conclude that the result of 1 plus 3 is 4.<br></think><br><br>Sure! Let's solve the addition problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Start with the first number:**<br>   \[<br>   1<br>   \]<br><br>2. **Add the second number to it:**<br>   \[<br>   1 + 3<br>   \]<br><br>3. **Calculate the sum:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:** \(\boxed{4}\)</strong>


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



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I add the two numbers together: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></think><br><br>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the final answer is \(\boxed{4}\).</strong>


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:328: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      return await dependant.call(**values)



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of 1 and 3.<br><br>Next, I add the two numbers together: 1 plus 3 equals 4.<br><br>Therefore, the final answer is 4.<br></strong>



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


    2026-04-01 20:59:52.492 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 20:59:52] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 20:59:52.492 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 20:59:52] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 20:59:52.492 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 20:59:52] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 20:59:52.492 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 20:59:52] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 20:59:52.492 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 20:59:52] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:01<00:01,  1.20s/it]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.55s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.50s/it]


    2026-04-01 20:59:58,390 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 20:59:58] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:52,  3.02s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:52,  3.02s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:39,  1.79s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:39,  1.79s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:04,  1.17s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:04,  1.17s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:46,  1.16it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:46,  1.16it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:35,  1.48it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:35,  1.48it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:29,  1.78it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:29,  1.78it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:24,  2.11it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:24,  2.11it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:06<00:20,  2.45it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:06<00:20,  2.45it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:06<00:17,  2.82it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:06<00:17,  2.82it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:06<00:14,  3.25it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:06<00:14,  3.25it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:06<00:12,  3.62it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:06<00:12,  3.62it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:06<00:12,  3.66it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:06<00:12,  3.66it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:07<00:12,  3.69it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:07<00:12,  3.69it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:07<00:11,  3.78it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:07<00:11,  3.78it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:07<00:10,  3.99it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:07<00:10,  3.99it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:07<00:09,  4.25it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:07<00:09,  4.25it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:08<00:08,  4.59it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:08<00:08,  4.59it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:08<00:08,  4.96it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:08<00:08,  4.96it/s]

    Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:08<00:07,  5.45it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:08<00:07,  5.45it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:08<00:06,  5.87it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:08<00:06,  5.87it/s]

    Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:08<00:05,  6.43it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:08<00:05,  6.43it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:08<00:05,  6.43it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:08<00:04,  7.82it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:08<00:04,  7.82it/s]

    Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:08<00:04,  7.82it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:08<00:03,  8.85it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:08<00:03,  8.85it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:09<00:03,  8.85it/s]

    Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:09<00:03,  9.85it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:09<00:03,  9.85it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:09<00:03,  9.85it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:09<00:02, 10.81it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:09<00:02, 10.81it/s]

    Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:09<00:02, 10.81it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:09<00:02, 11.83it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:09<00:02, 11.83it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:09<00:02, 11.83it/s]

    Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:09<00:02, 12.33it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:09<00:02, 12.33it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:09<00:02, 12.33it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:09<00:01, 13.10it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:09<00:01, 13.10it/s]

    Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:09<00:01, 13.10it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:09<00:01, 13.44it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:09<00:01, 13.44it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:09<00:01, 13.44it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:09<00:01, 14.79it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:09<00:01, 14.79it/s]

    Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:10<00:01, 14.79it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:10<00:01, 15.21it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:10<00:01, 15.21it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:10<00:01, 15.21it/s]

    Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:10<00:01, 14.64it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:10<00:01, 14.64it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:10<00:01, 14.64it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:10<00:00, 15.13it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:10<00:00, 15.13it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:10<00:00, 15.13it/s] 

    Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:10<00:00, 15.97it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:10<00:00, 15.97it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:10<00:00, 15.97it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:10<00:00, 16.02it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:10<00:00, 16.02it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:10<00:00, 16.02it/s]

    Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:10<00:00, 15.46it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:10<00:00, 15.46it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:10<00:00, 15.46it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:10<00:00, 15.46it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:10<00:00, 17.35it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:10<00:00, 17.35it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:10<00:00, 17.35it/s]

    Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:10<00:00, 17.88it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:10<00:00, 17.88it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:11<00:00, 17.88it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:11<00:00,  5.25it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=23.79 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=23.79 GB):   2%|▏         | 1/58 [00:01<01:02,  1.10s/it]Capturing num tokens (num_tokens=7680 avail_mem=23.76 GB):   2%|▏         | 1/58 [00:01<01:02,  1.10s/it]

    Capturing num tokens (num_tokens=7680 avail_mem=23.76 GB):   3%|▎         | 2/58 [00:01<00:52,  1.06it/s]Capturing num tokens (num_tokens=7168 avail_mem=23.76 GB):   3%|▎         | 2/58 [00:01<00:52,  1.06it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=23.76 GB):   5%|▌         | 3/58 [00:02<00:46,  1.19it/s]Capturing num tokens (num_tokens=6656 avail_mem=23.76 GB):   5%|▌         | 3/58 [00:02<00:46,  1.19it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=23.76 GB):   7%|▋         | 4/58 [00:03<00:40,  1.33it/s]Capturing num tokens (num_tokens=6144 avail_mem=23.77 GB):   7%|▋         | 4/58 [00:03<00:40,  1.33it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=23.77 GB):   9%|▊         | 5/58 [00:03<00:35,  1.49it/s]Capturing num tokens (num_tokens=5632 avail_mem=23.77 GB):   9%|▊         | 5/58 [00:03<00:35,  1.49it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=23.77 GB):  10%|█         | 6/58 [00:04<00:31,  1.66it/s]Capturing num tokens (num_tokens=5120 avail_mem=23.77 GB):  10%|█         | 6/58 [00:04<00:31,  1.66it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=23.77 GB):  12%|█▏        | 7/58 [00:04<00:28,  1.81it/s]Capturing num tokens (num_tokens=4608 avail_mem=23.77 GB):  12%|█▏        | 7/58 [00:04<00:28,  1.81it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=23.77 GB):  14%|█▍        | 8/58 [00:05<00:24,  2.00it/s]Capturing num tokens (num_tokens=4096 avail_mem=23.77 GB):  14%|█▍        | 8/58 [00:05<00:24,  2.00it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=23.77 GB):  16%|█▌        | 9/58 [00:05<00:21,  2.24it/s]Capturing num tokens (num_tokens=3840 avail_mem=22.74 GB):  16%|█▌        | 9/58 [00:05<00:21,  2.24it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=22.74 GB):  17%|█▋        | 10/58 [00:05<00:22,  2.17it/s]Capturing num tokens (num_tokens=3584 avail_mem=22.74 GB):  17%|█▋        | 10/58 [00:05<00:22,  2.17it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=22.74 GB):  19%|█▉        | 11/58 [00:06<00:21,  2.18it/s]Capturing num tokens (num_tokens=3328 avail_mem=23.74 GB):  19%|█▉        | 11/58 [00:06<00:21,  2.18it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=23.74 GB):  21%|██        | 12/58 [00:06<00:20,  2.28it/s]Capturing num tokens (num_tokens=3072 avail_mem=22.92 GB):  21%|██        | 12/58 [00:06<00:20,  2.28it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=22.92 GB):  22%|██▏       | 13/58 [00:07<00:19,  2.32it/s]Capturing num tokens (num_tokens=2816 avail_mem=22.92 GB):  22%|██▏       | 13/58 [00:07<00:19,  2.32it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=22.92 GB):  24%|██▍       | 14/58 [00:07<00:17,  2.52it/s]Capturing num tokens (num_tokens=2560 avail_mem=23.75 GB):  24%|██▍       | 14/58 [00:07<00:17,  2.52it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=23.75 GB):  26%|██▌       | 15/58 [00:07<00:16,  2.66it/s]Capturing num tokens (num_tokens=2304 avail_mem=22.98 GB):  26%|██▌       | 15/58 [00:07<00:16,  2.66it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=22.98 GB):  28%|██▊       | 16/58 [00:08<00:15,  2.78it/s]Capturing num tokens (num_tokens=2048 avail_mem=23.75 GB):  28%|██▊       | 16/58 [00:08<00:15,  2.78it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=23.75 GB):  29%|██▉       | 17/58 [00:08<00:13,  2.96it/s]Capturing num tokens (num_tokens=1792 avail_mem=23.04 GB):  29%|██▉       | 17/58 [00:08<00:13,  2.96it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=23.04 GB):  31%|███       | 18/58 [00:08<00:13,  3.06it/s]Capturing num tokens (num_tokens=1536 avail_mem=23.04 GB):  31%|███       | 18/58 [00:08<00:13,  3.06it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=23.04 GB):  33%|███▎      | 19/58 [00:08<00:11,  3.26it/s]Capturing num tokens (num_tokens=1280 avail_mem=23.75 GB):  33%|███▎      | 19/58 [00:08<00:11,  3.26it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=23.75 GB):  34%|███▍      | 20/58 [00:09<00:11,  3.44it/s]Capturing num tokens (num_tokens=1024 avail_mem=23.09 GB):  34%|███▍      | 20/58 [00:09<00:11,  3.44it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=23.09 GB):  36%|███▌      | 21/58 [00:09<00:10,  3.65it/s]Capturing num tokens (num_tokens=960 avail_mem=23.75 GB):  36%|███▌      | 21/58 [00:09<00:10,  3.65it/s] Capturing num tokens (num_tokens=960 avail_mem=23.75 GB):  38%|███▊      | 22/58 [00:09<00:09,  3.97it/s]Capturing num tokens (num_tokens=896 avail_mem=23.15 GB):  38%|███▊      | 22/58 [00:09<00:09,  3.97it/s]

    Capturing num tokens (num_tokens=896 avail_mem=23.15 GB):  40%|███▉      | 23/58 [00:09<00:08,  4.06it/s]Capturing num tokens (num_tokens=832 avail_mem=23.14 GB):  40%|███▉      | 23/58 [00:09<00:08,  4.06it/s]

    Capturing num tokens (num_tokens=832 avail_mem=23.14 GB):  41%|████▏     | 24/58 [00:10<00:08,  4.23it/s]Capturing num tokens (num_tokens=768 avail_mem=23.74 GB):  41%|████▏     | 24/58 [00:10<00:08,  4.23it/s]

    Capturing num tokens (num_tokens=768 avail_mem=23.74 GB):  43%|████▎     | 25/58 [00:10<00:07,  4.39it/s]Capturing num tokens (num_tokens=704 avail_mem=23.19 GB):  43%|████▎     | 25/58 [00:10<00:07,  4.39it/s]

    Capturing num tokens (num_tokens=704 avail_mem=23.19 GB):  45%|████▍     | 26/58 [00:10<00:07,  4.49it/s]Capturing num tokens (num_tokens=640 avail_mem=23.74 GB):  45%|████▍     | 26/58 [00:10<00:07,  4.49it/s]Capturing num tokens (num_tokens=640 avail_mem=23.74 GB):  47%|████▋     | 27/58 [00:10<00:06,  4.83it/s]Capturing num tokens (num_tokens=576 avail_mem=23.24 GB):  47%|████▋     | 27/58 [00:10<00:06,  4.83it/s]

    Capturing num tokens (num_tokens=576 avail_mem=23.24 GB):  48%|████▊     | 28/58 [00:10<00:06,  4.73it/s]Capturing num tokens (num_tokens=512 avail_mem=23.24 GB):  48%|████▊     | 28/58 [00:10<00:06,  4.73it/s]Capturing num tokens (num_tokens=512 avail_mem=23.24 GB):  50%|█████     | 29/58 [00:11<00:05,  5.03it/s]Capturing num tokens (num_tokens=480 avail_mem=23.73 GB):  50%|█████     | 29/58 [00:11<00:05,  5.03it/s]

    Capturing num tokens (num_tokens=480 avail_mem=23.73 GB):  52%|█████▏    | 30/58 [00:11<00:05,  5.05it/s]Capturing num tokens (num_tokens=448 avail_mem=23.29 GB):  52%|█████▏    | 30/58 [00:11<00:05,  5.05it/s]Capturing num tokens (num_tokens=448 avail_mem=23.29 GB):  53%|█████▎    | 31/58 [00:11<00:05,  5.04it/s]Capturing num tokens (num_tokens=416 avail_mem=23.73 GB):  53%|█████▎    | 31/58 [00:11<00:05,  5.04it/s]

    Capturing num tokens (num_tokens=416 avail_mem=23.73 GB):  55%|█████▌    | 32/58 [00:11<00:05,  5.11it/s]Capturing num tokens (num_tokens=384 avail_mem=23.31 GB):  55%|█████▌    | 32/58 [00:11<00:05,  5.11it/s]Capturing num tokens (num_tokens=384 avail_mem=23.31 GB):  57%|█████▋    | 33/58 [00:11<00:04,  5.25it/s]Capturing num tokens (num_tokens=352 avail_mem=23.72 GB):  57%|█████▋    | 33/58 [00:11<00:04,  5.25it/s]

    Capturing num tokens (num_tokens=352 avail_mem=23.72 GB):  59%|█████▊    | 34/58 [00:12<00:04,  5.30it/s]Capturing num tokens (num_tokens=320 avail_mem=23.33 GB):  59%|█████▊    | 34/58 [00:12<00:04,  5.30it/s]Capturing num tokens (num_tokens=320 avail_mem=23.33 GB):  60%|██████    | 35/58 [00:12<00:04,  5.57it/s]Capturing num tokens (num_tokens=288 avail_mem=23.72 GB):  60%|██████    | 35/58 [00:12<00:04,  5.57it/s]

    Capturing num tokens (num_tokens=288 avail_mem=23.72 GB):  62%|██████▏   | 36/58 [00:12<00:03,  5.66it/s]Capturing num tokens (num_tokens=256 avail_mem=23.35 GB):  62%|██████▏   | 36/58 [00:12<00:03,  5.66it/s]Capturing num tokens (num_tokens=256 avail_mem=23.35 GB):  64%|██████▍   | 37/58 [00:12<00:03,  5.90it/s]Capturing num tokens (num_tokens=240 avail_mem=23.71 GB):  64%|██████▍   | 37/58 [00:12<00:03,  5.90it/s]

    Capturing num tokens (num_tokens=240 avail_mem=23.71 GB):  66%|██████▌   | 38/58 [00:12<00:03,  6.05it/s]Capturing num tokens (num_tokens=224 avail_mem=23.37 GB):  66%|██████▌   | 38/58 [00:12<00:03,  6.05it/s]Capturing num tokens (num_tokens=224 avail_mem=23.37 GB):  67%|██████▋   | 39/58 [00:12<00:03,  6.15it/s]Capturing num tokens (num_tokens=208 avail_mem=23.70 GB):  67%|██████▋   | 39/58 [00:12<00:03,  6.15it/s]

    Capturing num tokens (num_tokens=208 avail_mem=23.70 GB):  69%|██████▉   | 40/58 [00:13<00:02,  6.24it/s]Capturing num tokens (num_tokens=192 avail_mem=23.39 GB):  69%|██████▉   | 40/58 [00:13<00:02,  6.24it/s]Capturing num tokens (num_tokens=192 avail_mem=23.39 GB):  71%|███████   | 41/58 [00:13<00:02,  6.44it/s]Capturing num tokens (num_tokens=176 avail_mem=23.69 GB):  71%|███████   | 41/58 [00:13<00:02,  6.44it/s]

    Capturing num tokens (num_tokens=176 avail_mem=23.69 GB):  72%|███████▏  | 42/58 [00:13<00:02,  6.39it/s]Capturing num tokens (num_tokens=160 avail_mem=23.41 GB):  72%|███████▏  | 42/58 [00:13<00:02,  6.39it/s]Capturing num tokens (num_tokens=160 avail_mem=23.41 GB):  74%|███████▍  | 43/58 [00:13<00:02,  6.04it/s]Capturing num tokens (num_tokens=144 avail_mem=23.68 GB):  74%|███████▍  | 43/58 [00:13<00:02,  6.04it/s]

    Capturing num tokens (num_tokens=144 avail_mem=23.68 GB):  76%|███████▌  | 44/58 [00:13<00:02,  6.02it/s]Capturing num tokens (num_tokens=128 avail_mem=23.70 GB):  76%|███████▌  | 44/58 [00:13<00:02,  6.02it/s]Capturing num tokens (num_tokens=128 avail_mem=23.70 GB):  78%|███████▊  | 45/58 [00:13<00:02,  6.47it/s]Capturing num tokens (num_tokens=112 avail_mem=23.47 GB):  78%|███████▊  | 45/58 [00:13<00:02,  6.47it/s]

    Capturing num tokens (num_tokens=112 avail_mem=23.47 GB):  79%|███████▉  | 46/58 [00:13<00:01,  6.61it/s]Capturing num tokens (num_tokens=96 avail_mem=23.68 GB):  79%|███████▉  | 46/58 [00:13<00:01,  6.61it/s] Capturing num tokens (num_tokens=96 avail_mem=23.68 GB):  81%|████████  | 47/58 [00:14<00:01,  6.48it/s]Capturing num tokens (num_tokens=80 avail_mem=23.68 GB):  81%|████████  | 47/58 [00:14<00:01,  6.48it/s]

    Capturing num tokens (num_tokens=80 avail_mem=23.68 GB):  83%|████████▎ | 48/58 [00:14<00:01,  6.82it/s]Capturing num tokens (num_tokens=64 avail_mem=23.51 GB):  83%|████████▎ | 48/58 [00:14<00:01,  6.82it/s]Capturing num tokens (num_tokens=64 avail_mem=23.51 GB):  84%|████████▍ | 49/58 [00:14<00:01,  7.08it/s]Capturing num tokens (num_tokens=48 avail_mem=23.67 GB):  84%|████████▍ | 49/58 [00:14<00:01,  7.08it/s]

    Capturing num tokens (num_tokens=48 avail_mem=23.67 GB):  86%|████████▌ | 50/58 [00:14<00:01,  7.19it/s]Capturing num tokens (num_tokens=32 avail_mem=23.67 GB):  86%|████████▌ | 50/58 [00:14<00:01,  7.19it/s]Capturing num tokens (num_tokens=32 avail_mem=23.67 GB):  88%|████████▊ | 51/58 [00:14<00:00,  7.39it/s]Capturing num tokens (num_tokens=28 avail_mem=23.67 GB):  88%|████████▊ | 51/58 [00:14<00:00,  7.39it/s]

    Capturing num tokens (num_tokens=28 avail_mem=23.67 GB):  90%|████████▉ | 52/58 [00:14<00:00,  7.86it/s]Capturing num tokens (num_tokens=24 avail_mem=23.66 GB):  90%|████████▉ | 52/58 [00:14<00:00,  7.86it/s]Capturing num tokens (num_tokens=24 avail_mem=23.66 GB):  91%|█████████▏| 53/58 [00:14<00:00,  7.80it/s]Capturing num tokens (num_tokens=20 avail_mem=23.65 GB):  91%|█████████▏| 53/58 [00:14<00:00,  7.80it/s]

    Capturing num tokens (num_tokens=20 avail_mem=23.65 GB):  93%|█████████▎| 54/58 [00:14<00:00,  7.81it/s]Capturing num tokens (num_tokens=16 avail_mem=23.65 GB):  93%|█████████▎| 54/58 [00:14<00:00,  7.81it/s]Capturing num tokens (num_tokens=16 avail_mem=23.65 GB):  95%|█████████▍| 55/58 [00:15<00:00,  7.75it/s]Capturing num tokens (num_tokens=12 avail_mem=23.64 GB):  95%|█████████▍| 55/58 [00:15<00:00,  7.75it/s]

    Capturing num tokens (num_tokens=12 avail_mem=23.64 GB):  97%|█████████▋| 56/58 [00:15<00:00,  8.06it/s]Capturing num tokens (num_tokens=8 avail_mem=23.55 GB):  97%|█████████▋| 56/58 [00:15<00:00,  8.06it/s] Capturing num tokens (num_tokens=8 avail_mem=23.55 GB):  98%|█████████▊| 57/58 [00:15<00:00,  8.23it/s]Capturing num tokens (num_tokens=4 avail_mem=23.62 GB):  98%|█████████▊| 57/58 [00:15<00:00,  8.23it/s]

    Capturing num tokens (num_tokens=4 avail_mem=23.62 GB): 100%|██████████| 58/58 [00:15<00:00,  8.18it/s]Capturing num tokens (num_tokens=4 avail_mem=23.62 GB): 100%|██████████| 58/58 [00:15<00:00,  3.75it/s]



<strong style='color: #00008B;'>==== Original Output ====</strong>



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of the numbers 1 and 3.<br><br>To find the total, I add the two numbers together: 1 plus 3.<br><br>After performing the addition, the result is 4.<br></think><br><br>**Solution:**<br><br>We are asked to find the sum of \(1\) and \(3\).<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the final answer is \(\boxed{4}\).</strong>



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of the numbers 1 and 3.<br><br>To find the total, I add the two numbers together: 1 plus 3.<br><br>After performing the addition, the result is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>**Solution:**<br><br>We are asked to find the sum of \(1\) and \(3\).<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the final answer is \(\boxed{4}\).</strong>



```python
llm.shutdown()
```

## Supporting New Reasoning Model Schemas

For future reasoning models, you can implement the reasoning parser as a subclass of `BaseReasoningFormatDetector` in `python/sglang/srt/reasoning_parser.py` and specify the reasoning parser for new reasoning model schemas accordingly.
