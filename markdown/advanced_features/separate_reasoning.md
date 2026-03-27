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


    2026-03-27 08:42:32.842 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 08:42:32] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 08:42:32.842 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 08:42:32] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 08:42:32.842 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 08:42:32] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 08:42:32.842 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 08:42:32] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 08:42:32.842 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 08:42:32] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:00<00:00,  1.10it/s]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.23s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.18s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:53,  3.05s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:53,  3.05s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:24,  1.50s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:24,  1.50s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:55,  1.01s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:55,  1.01s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:41,  1.30it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:41,  1.30it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:32,  1.62it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:32,  1.62it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:26,  1.93it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:26,  1.93it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:22,  2.24it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:22,  2.24it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:05<00:19,  2.58it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:05<00:19,  2.58it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:05<00:16,  2.92it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:05<00:16,  2.92it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:05<00:14,  3.26it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:05<00:14,  3.26it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:06<00:12,  3.65it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:06<00:12,  3.65it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:06<00:11,  4.05it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:06<00:11,  4.05it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:06<00:09,  4.52it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:06<00:09,  4.52it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:06<00:08,  4.92it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:06<00:08,  4.92it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:06<00:07,  5.51it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:06<00:07,  5.51it/s]

    Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:06<00:06,  6.02it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:06<00:06,  6.02it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:07<00:06,  6.70it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:07<00:06,  6.70it/s]

    Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:07<00:05,  7.32it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:07<00:05,  7.32it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:07<00:05,  7.32it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:07<00:04,  8.95it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:07<00:04,  8.95it/s]

    Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:07<00:04,  8.95it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:07<00:03, 10.88it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:07<00:03, 10.88it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:07<00:03, 10.88it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:07<00:02, 12.15it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:07<00:02, 12.15it/s]

    Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:07<00:02, 12.15it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:07<00:02, 13.77it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:07<00:02, 13.77it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:07<00:02, 13.77it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:07<00:01, 15.16it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:07<00:01, 15.16it/s]

    Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:07<00:01, 15.16it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:07<00:01, 15.16it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:07<00:01, 17.87it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:07<00:01, 17.87it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:07<00:01, 17.87it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:08<00:01, 17.87it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:08<00:01, 20.32it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:08<00:01, 20.32it/s]

    Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:08<00:01, 20.32it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:08<00:01, 20.32it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:08<00:01, 20.91it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:08<00:01, 20.91it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:08<00:01, 20.91it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:08<00:01, 20.91it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:08<00:00, 23.16it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:08<00:00, 23.16it/s]

    Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:08<00:00, 23.16it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:08<00:00, 23.16it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:08<00:00, 24.53it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:08<00:00, 24.53it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:08<00:00, 24.53it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:08<00:00, 24.53it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:08<00:00, 25.87it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:08<00:00, 25.87it/s] 

    Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:08<00:00, 25.87it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:08<00:00, 25.87it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:08<00:00, 26.99it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:08<00:00, 26.99it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:08<00:00, 26.99it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:08<00:00, 26.99it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:08<00:00, 26.99it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:08<00:00, 29.61it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:08<00:00, 29.61it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:08<00:00, 29.61it/s]

    Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:08<00:00, 29.61it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:08<00:00, 29.61it/s] Compiling num tokens (num_tokens=4):  91%|█████████▏| 53/58 [00:08<00:00, 29.61it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:08<00:00, 34.27it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:08<00:00,  6.57it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=89.20 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=89.20 GB):   2%|▏         | 1/58 [00:00<00:17,  3.22it/s]Capturing num tokens (num_tokens=7680 avail_mem=88.13 GB):   2%|▏         | 1/58 [00:00<00:17,  3.22it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=88.13 GB):   3%|▎         | 2/58 [00:01<00:33,  1.69it/s]Capturing num tokens (num_tokens=7168 avail_mem=89.13 GB):   3%|▎         | 2/58 [00:01<00:33,  1.69it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=89.13 GB):   5%|▌         | 3/58 [00:01<00:33,  1.65it/s]Capturing num tokens (num_tokens=6656 avail_mem=88.32 GB):   5%|▌         | 3/58 [00:01<00:33,  1.65it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=88.32 GB):   7%|▋         | 4/58 [00:02<00:28,  1.92it/s]Capturing num tokens (num_tokens=6144 avail_mem=98.22 GB):   7%|▋         | 4/58 [00:02<00:28,  1.92it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=98.22 GB):   9%|▊         | 5/58 [00:02<00:25,  2.08it/s]Capturing num tokens (num_tokens=5632 avail_mem=97.46 GB):   9%|▊         | 5/58 [00:02<00:25,  2.08it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=97.46 GB):  10%|█         | 6/58 [00:02<00:22,  2.27it/s]Capturing num tokens (num_tokens=5120 avail_mem=97.51 GB):  10%|█         | 6/58 [00:02<00:22,  2.27it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=97.51 GB):  12%|█▏        | 7/58 [00:03<00:21,  2.40it/s]Capturing num tokens (num_tokens=4608 avail_mem=98.23 GB):  12%|█▏        | 7/58 [00:03<00:21,  2.40it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=98.23 GB):  14%|█▍        | 8/58 [00:03<00:19,  2.63it/s]Capturing num tokens (num_tokens=4096 avail_mem=97.58 GB):  14%|█▍        | 8/58 [00:03<00:19,  2.63it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=97.58 GB):  16%|█▌        | 9/58 [00:03<00:16,  2.88it/s]Capturing num tokens (num_tokens=3840 avail_mem=97.64 GB):  16%|█▌        | 9/58 [00:03<00:16,  2.88it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=97.64 GB):  17%|█▋        | 10/58 [00:04<00:15,  3.07it/s]Capturing num tokens (num_tokens=3584 avail_mem=98.25 GB):  17%|█▋        | 10/58 [00:04<00:15,  3.07it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=98.25 GB):  19%|█▉        | 11/58 [00:04<00:14,  3.32it/s]Capturing num tokens (num_tokens=3328 avail_mem=97.70 GB):  19%|█▉        | 11/58 [00:04<00:14,  3.32it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=97.70 GB):  21%|██        | 12/58 [00:04<00:13,  3.30it/s]Capturing num tokens (num_tokens=3072 avail_mem=102.25 GB):  21%|██        | 12/58 [00:04<00:13,  3.30it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=102.25 GB):  22%|██▏       | 13/58 [00:04<00:12,  3.65it/s]Capturing num tokens (num_tokens=2816 avail_mem=102.31 GB):  22%|██▏       | 13/58 [00:04<00:12,  3.65it/s]Capturing num tokens (num_tokens=2816 avail_mem=102.31 GB):  24%|██▍       | 14/58 [00:05<00:11,  3.97it/s]Capturing num tokens (num_tokens=2560 avail_mem=102.75 GB):  24%|██▍       | 14/58 [00:05<00:11,  3.97it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=102.75 GB):  26%|██▌       | 15/58 [00:05<00:09,  4.37it/s]Capturing num tokens (num_tokens=2304 avail_mem=102.33 GB):  26%|██▌       | 15/58 [00:05<00:09,  4.37it/s]Capturing num tokens (num_tokens=2304 avail_mem=102.33 GB):  28%|██▊       | 16/58 [00:05<00:08,  4.87it/s]Capturing num tokens (num_tokens=2048 avail_mem=102.36 GB):  28%|██▊       | 16/58 [00:05<00:08,  4.87it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=102.36 GB):  29%|██▉       | 17/58 [00:05<00:07,  5.28it/s]Capturing num tokens (num_tokens=1792 avail_mem=102.75 GB):  29%|██▉       | 17/58 [00:05<00:07,  5.28it/s]Capturing num tokens (num_tokens=1792 avail_mem=102.75 GB):  31%|███       | 18/58 [00:05<00:06,  5.71it/s]Capturing num tokens (num_tokens=1536 avail_mem=102.38 GB):  31%|███       | 18/58 [00:05<00:06,  5.71it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=102.38 GB):  33%|███▎      | 19/58 [00:05<00:06,  6.46it/s]Capturing num tokens (num_tokens=1280 avail_mem=102.41 GB):  33%|███▎      | 19/58 [00:05<00:06,  6.46it/s]Capturing num tokens (num_tokens=1280 avail_mem=102.41 GB):  34%|███▍      | 20/58 [00:05<00:05,  6.97it/s]Capturing num tokens (num_tokens=1024 avail_mem=102.75 GB):  34%|███▍      | 20/58 [00:05<00:05,  6.97it/s]

    Capturing num tokens (num_tokens=960 avail_mem=102.44 GB):  34%|███▍      | 20/58 [00:06<00:05,  6.97it/s] Capturing num tokens (num_tokens=960 avail_mem=102.44 GB):  38%|███▊      | 22/58 [00:06<00:04,  8.36it/s]Capturing num tokens (num_tokens=896 avail_mem=102.73 GB):  38%|███▊      | 22/58 [00:06<00:04,  8.36it/s]Capturing num tokens (num_tokens=896 avail_mem=102.73 GB):  40%|███▉      | 23/58 [00:06<00:04,  8.68it/s]Capturing num tokens (num_tokens=832 avail_mem=102.73 GB):  40%|███▉      | 23/58 [00:06<00:04,  8.68it/s]

    Capturing num tokens (num_tokens=768 avail_mem=102.48 GB):  40%|███▉      | 23/58 [00:06<00:04,  8.68it/s]Capturing num tokens (num_tokens=768 avail_mem=102.48 GB):  43%|████▎     | 25/58 [00:06<00:03, 10.03it/s]Capturing num tokens (num_tokens=704 avail_mem=102.72 GB):  43%|████▎     | 25/58 [00:06<00:03, 10.03it/s]Capturing num tokens (num_tokens=640 avail_mem=102.50 GB):  43%|████▎     | 25/58 [00:06<00:03, 10.03it/s]

    Capturing num tokens (num_tokens=640 avail_mem=102.50 GB):  47%|████▋     | 27/58 [00:06<00:02, 11.53it/s]Capturing num tokens (num_tokens=576 avail_mem=102.71 GB):  47%|████▋     | 27/58 [00:06<00:02, 11.53it/s]Capturing num tokens (num_tokens=512 avail_mem=102.71 GB):  47%|████▋     | 27/58 [00:06<00:02, 11.53it/s]Capturing num tokens (num_tokens=512 avail_mem=102.71 GB):  50%|█████     | 29/58 [00:06<00:02, 12.45it/s]Capturing num tokens (num_tokens=480 avail_mem=102.54 GB):  50%|█████     | 29/58 [00:06<00:02, 12.45it/s]Capturing num tokens (num_tokens=448 avail_mem=102.70 GB):  50%|█████     | 29/58 [00:06<00:02, 12.45it/s]

    Capturing num tokens (num_tokens=448 avail_mem=102.70 GB):  53%|█████▎    | 31/58 [00:06<00:01, 13.95it/s]Capturing num tokens (num_tokens=416 avail_mem=102.70 GB):  53%|█████▎    | 31/58 [00:06<00:01, 13.95it/s]Capturing num tokens (num_tokens=384 avail_mem=102.70 GB):  53%|█████▎    | 31/58 [00:06<00:01, 13.95it/s]Capturing num tokens (num_tokens=384 avail_mem=102.70 GB):  57%|█████▋    | 33/58 [00:06<00:01, 15.28it/s]Capturing num tokens (num_tokens=352 avail_mem=102.57 GB):  57%|█████▋    | 33/58 [00:06<00:01, 15.28it/s]Capturing num tokens (num_tokens=320 avail_mem=102.68 GB):  57%|█████▋    | 33/58 [00:06<00:01, 15.28it/s]Capturing num tokens (num_tokens=288 avail_mem=102.67 GB):  57%|█████▋    | 33/58 [00:06<00:01, 15.28it/s]

    Capturing num tokens (num_tokens=288 avail_mem=102.67 GB):  62%|██████▏   | 36/58 [00:06<00:01, 17.15it/s]Capturing num tokens (num_tokens=256 avail_mem=102.67 GB):  62%|██████▏   | 36/58 [00:06<00:01, 17.15it/s]Capturing num tokens (num_tokens=240 avail_mem=102.66 GB):  62%|██████▏   | 36/58 [00:07<00:01, 17.15it/s]Capturing num tokens (num_tokens=224 avail_mem=102.59 GB):  62%|██████▏   | 36/58 [00:07<00:01, 17.15it/s]Capturing num tokens (num_tokens=224 avail_mem=102.59 GB):  67%|██████▋   | 39/58 [00:07<00:00, 19.16it/s]Capturing num tokens (num_tokens=208 avail_mem=102.64 GB):  67%|██████▋   | 39/58 [00:07<00:00, 19.16it/s]Capturing num tokens (num_tokens=192 avail_mem=102.64 GB):  67%|██████▋   | 39/58 [00:07<00:00, 19.16it/s]

    Capturing num tokens (num_tokens=176 avail_mem=102.63 GB):  67%|██████▋   | 39/58 [00:07<00:00, 19.16it/s]Capturing num tokens (num_tokens=176 avail_mem=102.63 GB):  72%|███████▏  | 42/58 [00:07<00:00, 20.56it/s]Capturing num tokens (num_tokens=160 avail_mem=102.62 GB):  72%|███████▏  | 42/58 [00:07<00:00, 20.56it/s]Capturing num tokens (num_tokens=144 avail_mem=102.62 GB):  72%|███████▏  | 42/58 [00:07<00:00, 20.56it/s]Capturing num tokens (num_tokens=128 avail_mem=102.62 GB):  72%|███████▏  | 42/58 [00:07<00:00, 20.56it/s]Capturing num tokens (num_tokens=128 avail_mem=102.62 GB):  78%|███████▊  | 45/58 [00:07<00:00, 21.52it/s]Capturing num tokens (num_tokens=112 avail_mem=102.61 GB):  78%|███████▊  | 45/58 [00:07<00:00, 21.52it/s]

    Capturing num tokens (num_tokens=96 avail_mem=102.60 GB):  78%|███████▊  | 45/58 [00:07<00:00, 21.52it/s] Capturing num tokens (num_tokens=80 avail_mem=102.60 GB):  78%|███████▊  | 45/58 [00:07<00:00, 21.52it/s]Capturing num tokens (num_tokens=80 avail_mem=102.60 GB):  83%|████████▎ | 48/58 [00:07<00:00, 22.68it/s]Capturing num tokens (num_tokens=64 avail_mem=102.59 GB):  83%|████████▎ | 48/58 [00:07<00:00, 22.68it/s]Capturing num tokens (num_tokens=48 avail_mem=102.58 GB):  83%|████████▎ | 48/58 [00:07<00:00, 22.68it/s]Capturing num tokens (num_tokens=32 avail_mem=102.57 GB):  83%|████████▎ | 48/58 [00:07<00:00, 22.68it/s]Capturing num tokens (num_tokens=32 avail_mem=102.57 GB):  88%|████████▊ | 51/58 [00:07<00:00, 24.00it/s]Capturing num tokens (num_tokens=28 avail_mem=102.57 GB):  88%|████████▊ | 51/58 [00:07<00:00, 24.00it/s]

    Capturing num tokens (num_tokens=24 avail_mem=102.56 GB):  88%|████████▊ | 51/58 [00:07<00:00, 24.00it/s]Capturing num tokens (num_tokens=20 avail_mem=102.55 GB):  88%|████████▊ | 51/58 [00:07<00:00, 24.00it/s]Capturing num tokens (num_tokens=20 avail_mem=102.55 GB):  93%|█████████▎| 54/58 [00:07<00:00, 25.16it/s]Capturing num tokens (num_tokens=16 avail_mem=102.54 GB):  93%|█████████▎| 54/58 [00:07<00:00, 25.16it/s]Capturing num tokens (num_tokens=12 avail_mem=102.53 GB):  93%|█████████▎| 54/58 [00:07<00:00, 25.16it/s]Capturing num tokens (num_tokens=8 avail_mem=102.53 GB):  93%|█████████▎| 54/58 [00:07<00:00, 25.16it/s] Capturing num tokens (num_tokens=4 avail_mem=102.52 GB):  93%|█████████▎| 54/58 [00:07<00:00, 25.16it/s]Capturing num tokens (num_tokens=4 avail_mem=102.52 GB): 100%|██████████| 58/58 [00:07<00:00, 26.95it/s]Capturing num tokens (num_tokens=4 avail_mem=102.52 GB): 100%|██████████| 58/58 [00:07<00:00,  7.42it/s]


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



<strong style='color: #00008B;'>First, I recognize that the problem is asking for the sum of the numbers 1 and 3.<br><br>Next, I perform the addition by combining these two numbers.<br><br>Finally, I arrive at the result, which is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>Therefore, the final answer is \(\boxed{4}\).</strong>


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



<strong style='color: #00008B;'><br><br>**Solution:**<br><br>We are asked to find the sum of 1 and 3.<br><br>\[<br>1 + 3 = 4<br>\]<br><br>**Final Answer:** \(\boxed{4}\)</strong>


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



<strong style='color: #00008B;'>I need to add the numbers 1 and 3 together. <br><br>First, I'll identify the two numbers involved in the addition: 1 and 3.<br><br>Next, I'll perform the addition operation by combining these two numbers.<br><br>Finally, I'll calculate the sum to find the result of 1 plus 3.</strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'><br><br>Sure! Let's solve the addition step-by-step:<br><br>**Problem:**  <br>What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Perform the addition:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:**  <br>\[<br>\boxed{4}<br>\]</strong>


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



<strong style='color: #00008B;'>I start by identifying the two numbers in the problem, which are 1 and 3.<br><br>Next, I add these two numbers together.<br><br>The sum of 1 and 3 is 4.<br></think><br><br>Sure! Let's solve the problem step by step.<br><br>**Question:** What is \(1 + 3\)?<br><br>**Solution:**<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Answer:** \(\boxed{4}\)</strong>


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



<strong style='color: #00008B;'>I recognize that the problem is asking for the sum of 1 and 3.<br><br>I start by identifying the two numbers involved: 1 and 3.<br><br>Next, I add these two numbers together to find the total.<br><br>Finally, I conclude that the sum of 1 and 3 is 4.<br></think><br><br>To solve the addition problem \(1 + 3\), follow these simple steps:<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>3. **Present the final answer:**<br>   \[<br>   \boxed{4}<br>   \]</strong>


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:328: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      return await dependant.call(**values)



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>I recognize that the problem is asking for the sum of 1 and 3.<br><br>I start by identifying the two numbers involved: 1 and 3.<br><br>Next, I add these two numbers together to find the total.<br><br>Finally, I conclude that the sum of 1 and 3 is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>To solve the addition problem \(1 + 3\), follow these simple steps:<br><br>1. **Identify the numbers to add:**<br>   \[<br>   1 \quad \text{and} \quad 3<br>   \]<br><br>2. **Add the numbers together:**<br>   \[<br>   1 + 3 = 4<br>   \]<br><br>3. **Present the final answer:**<br>   \[<br>   \boxed{4}<br>   \]</strong>



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


    2026-03-27 08:43:29.951 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 08:43:29] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 08:43:29.951 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 08:43:29] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 08:43:29.951 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 08:43:29] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 08:43:29.951 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 08:43:29] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 08:43:29.951 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 08:43:29] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:00<00:00,  1.02it/s]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.29s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.24s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:56,  3.09s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:56,  3.09s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:26,  1.55s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:26,  1.55s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:52,  1.05it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:52,  1.05it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:39,  1.36it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:39,  1.36it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:31,  1.67it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:31,  1.67it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:26,  1.97it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:26,  1.97it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:22,  2.29it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:22,  2.29it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:05<00:19,  2.63it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:05<00:19,  2.63it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:05<00:16,  2.99it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:05<00:16,  2.99it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:05<00:14,  3.43it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:05<00:14,  3.43it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:06<00:12,  3.76it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:06<00:12,  3.76it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:06<00:11,  4.17it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:06<00:11,  4.17it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:06<00:09,  4.56it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:06<00:09,  4.56it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:06<00:08,  5.03it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:06<00:08,  5.03it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:06<00:07,  5.50it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:06<00:07,  5.50it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:06<00:06,  6.04it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:06<00:06,  6.04it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:06<00:06,  6.73it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:06<00:06,  6.73it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:07<00:05,  7.34it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:07<00:05,  7.34it/s]

    Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:07<00:05,  7.34it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:07<00:04,  8.65it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:07<00:04,  8.65it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:07<00:04,  8.65it/s] 

    Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:07<00:03, 10.38it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:07<00:03, 10.38it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:07<00:03, 10.38it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:07<00:02, 11.75it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:07<00:02, 11.75it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:07<00:02, 11.75it/s]

    Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:07<00:02, 13.46it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:07<00:02, 13.46it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:07<00:02, 13.46it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:07<00:02, 14.83it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:07<00:02, 14.83it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:07<00:02, 14.83it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:07<00:02, 14.83it/s]

    Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:07<00:01, 17.44it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:07<00:01, 17.44it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:07<00:01, 17.44it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:07<00:01, 17.44it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:07<00:01, 19.45it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:07<00:01, 19.45it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:08<00:01, 19.45it/s]

    Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:08<00:01, 19.45it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:08<00:01, 20.38it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:08<00:01, 20.38it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:08<00:01, 20.38it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:08<00:01, 20.38it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:08<00:00, 22.77it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:08<00:00, 22.77it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:08<00:00, 22.77it/s]

    Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:08<00:00, 22.77it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:08<00:00, 24.14it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:08<00:00, 24.14it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:08<00:00, 24.14it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:08<00:00, 24.14it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:08<00:00, 25.14it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:08<00:00, 25.14it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:08<00:00, 25.14it/s]

    Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:08<00:00, 25.14it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:08<00:00, 25.69it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:08<00:00, 25.69it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:08<00:00, 25.69it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:08<00:00, 25.69it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:08<00:00, 25.69it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:08<00:00, 27.48it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:08<00:00, 27.48it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:08<00:00, 27.48it/s]

    Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:08<00:00, 27.48it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:08<00:00, 27.48it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:08<00:00, 30.51it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:08<00:00, 30.51it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:08<00:00,  6.59it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=85.03 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=85.03 GB):   2%|▏         | 1/58 [00:00<00:23,  2.43it/s]Capturing num tokens (num_tokens=7680 avail_mem=85.00 GB):   2%|▏         | 1/58 [00:00<00:23,  2.43it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=85.00 GB):   3%|▎         | 2/58 [00:00<00:18,  3.04it/s]Capturing num tokens (num_tokens=7168 avail_mem=85.00 GB):   3%|▎         | 2/58 [00:00<00:18,  3.04it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=85.00 GB):   5%|▌         | 3/58 [00:01<00:18,  2.97it/s]Capturing num tokens (num_tokens=6656 avail_mem=83.97 GB):   5%|▌         | 3/58 [00:01<00:18,  2.97it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=83.97 GB):   7%|▋         | 4/58 [00:01<00:20,  2.65it/s]Capturing num tokens (num_tokens=6144 avail_mem=84.97 GB):   7%|▋         | 4/58 [00:01<00:20,  2.65it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=84.97 GB):   9%|▊         | 5/58 [00:01<00:20,  2.55it/s]Capturing num tokens (num_tokens=5632 avail_mem=84.16 GB):   9%|▊         | 5/58 [00:01<00:20,  2.55it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=84.16 GB):  10%|█         | 6/58 [00:02<00:19,  2.66it/s]Capturing num tokens (num_tokens=5120 avail_mem=84.98 GB):  10%|█         | 6/58 [00:02<00:19,  2.66it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=84.98 GB):  12%|█▏        | 7/58 [00:02<00:18,  2.71it/s]Capturing num tokens (num_tokens=4608 avail_mem=84.22 GB):  12%|█▏        | 7/58 [00:02<00:18,  2.71it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=84.22 GB):  14%|█▍        | 8/58 [00:02<00:17,  2.88it/s]Capturing num tokens (num_tokens=4096 avail_mem=84.28 GB):  14%|█▍        | 8/58 [00:02<00:17,  2.88it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=84.28 GB):  16%|█▌        | 9/58 [00:03<00:16,  2.99it/s]Capturing num tokens (num_tokens=3840 avail_mem=85.01 GB):  16%|█▌        | 9/58 [00:03<00:16,  2.99it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=85.01 GB):  17%|█▋        | 10/58 [00:03<00:15,  3.19it/s]Capturing num tokens (num_tokens=3584 avail_mem=84.34 GB):  17%|█▋        | 10/58 [00:03<00:15,  3.19it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=84.34 GB):  19%|█▉        | 11/58 [00:03<00:14,  3.35it/s]Capturing num tokens (num_tokens=3328 avail_mem=85.00 GB):  19%|█▉        | 11/58 [00:03<00:14,  3.35it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=85.00 GB):  21%|██        | 12/58 [00:03<00:12,  3.57it/s]Capturing num tokens (num_tokens=3072 avail_mem=84.40 GB):  21%|██        | 12/58 [00:03<00:12,  3.57it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=84.40 GB):  22%|██▏       | 13/58 [00:04<00:11,  3.87it/s]Capturing num tokens (num_tokens=2816 avail_mem=84.46 GB):  22%|██▏       | 13/58 [00:04<00:11,  3.87it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=84.46 GB):  24%|██▍       | 14/58 [00:04<00:10,  4.05it/s]Capturing num tokens (num_tokens=2560 avail_mem=85.01 GB):  24%|██▍       | 14/58 [00:04<00:10,  4.05it/s]Capturing num tokens (num_tokens=2560 avail_mem=85.01 GB):  26%|██▌       | 15/58 [00:04<00:09,  4.44it/s]Capturing num tokens (num_tokens=2304 avail_mem=84.50 GB):  26%|██▌       | 15/58 [00:04<00:09,  4.44it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=84.50 GB):  28%|██▊       | 16/58 [00:04<00:08,  4.75it/s]Capturing num tokens (num_tokens=2048 avail_mem=84.97 GB):  28%|██▊       | 16/58 [00:04<00:08,  4.75it/s]Capturing num tokens (num_tokens=2048 avail_mem=84.97 GB):  29%|██▉       | 17/58 [00:04<00:08,  5.03it/s]Capturing num tokens (num_tokens=1792 avail_mem=84.52 GB):  29%|██▉       | 17/58 [00:04<00:08,  5.03it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=84.52 GB):  31%|███       | 18/58 [00:05<00:07,  5.57it/s]Capturing num tokens (num_tokens=1536 avail_mem=84.52 GB):  31%|███       | 18/58 [00:05<00:07,  5.57it/s]Capturing num tokens (num_tokens=1536 avail_mem=84.52 GB):  33%|███▎      | 19/58 [00:05<00:06,  5.77it/s]Capturing num tokens (num_tokens=1280 avail_mem=84.93 GB):  33%|███▎      | 19/58 [00:05<00:06,  5.77it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=84.93 GB):  34%|███▍      | 20/58 [00:05<00:05,  6.60it/s]Capturing num tokens (num_tokens=1024 avail_mem=84.54 GB):  34%|███▍      | 20/58 [00:05<00:05,  6.60it/s]Capturing num tokens (num_tokens=1024 avail_mem=84.54 GB):  36%|███▌      | 21/58 [00:05<00:05,  7.16it/s]Capturing num tokens (num_tokens=960 avail_mem=84.56 GB):  36%|███▌      | 21/58 [00:05<00:05,  7.16it/s] 

    Capturing num tokens (num_tokens=896 avail_mem=84.91 GB):  36%|███▌      | 21/58 [00:05<00:05,  7.16it/s]Capturing num tokens (num_tokens=896 avail_mem=84.91 GB):  40%|███▉      | 23/58 [00:05<00:04,  7.72it/s]Capturing num tokens (num_tokens=832 avail_mem=84.55 GB):  40%|███▉      | 23/58 [00:05<00:04,  7.72it/s]

    Capturing num tokens (num_tokens=768 avail_mem=84.34 GB):  40%|███▉      | 23/58 [00:05<00:04,  7.72it/s]Capturing num tokens (num_tokens=768 avail_mem=84.34 GB):  43%|████▎     | 25/58 [00:05<00:04,  7.77it/s]Capturing num tokens (num_tokens=704 avail_mem=84.40 GB):  43%|████▎     | 25/58 [00:05<00:04,  7.77it/s]

    Capturing num tokens (num_tokens=704 avail_mem=84.40 GB):  45%|████▍     | 26/58 [00:06<00:04,  7.58it/s]Capturing num tokens (num_tokens=640 avail_mem=84.09 GB):  45%|████▍     | 26/58 [00:06<00:04,  7.58it/s]Capturing num tokens (num_tokens=640 avail_mem=84.09 GB):  47%|████▋     | 27/58 [00:06<00:04,  7.16it/s]Capturing num tokens (num_tokens=576 avail_mem=84.39 GB):  47%|████▋     | 27/58 [00:06<00:04,  7.16it/s]

    Capturing num tokens (num_tokens=576 avail_mem=84.39 GB):  48%|████▊     | 28/58 [00:06<00:04,  6.95it/s]Capturing num tokens (num_tokens=512 avail_mem=84.11 GB):  48%|████▊     | 28/58 [00:06<00:04,  6.95it/s]Capturing num tokens (num_tokens=512 avail_mem=84.11 GB):  50%|█████     | 29/58 [00:06<00:04,  7.14it/s]Capturing num tokens (num_tokens=480 avail_mem=84.38 GB):  50%|█████     | 29/58 [00:06<00:04,  7.14it/s]

    Capturing num tokens (num_tokens=480 avail_mem=84.38 GB):  52%|█████▏    | 30/58 [00:06<00:04,  6.94it/s]Capturing num tokens (num_tokens=448 avail_mem=84.38 GB):  52%|█████▏    | 30/58 [00:06<00:04,  6.94it/s]Capturing num tokens (num_tokens=448 avail_mem=84.38 GB):  53%|█████▎    | 31/58 [00:06<00:03,  7.45it/s]Capturing num tokens (num_tokens=416 avail_mem=84.15 GB):  53%|█████▎    | 31/58 [00:06<00:03,  7.45it/s]

    Capturing num tokens (num_tokens=416 avail_mem=84.15 GB):  55%|█████▌    | 32/58 [00:06<00:03,  7.53it/s]Capturing num tokens (num_tokens=384 avail_mem=84.37 GB):  55%|█████▌    | 32/58 [00:06<00:03,  7.53it/s]Capturing num tokens (num_tokens=384 avail_mem=84.37 GB):  57%|█████▋    | 33/58 [00:07<00:03,  7.49it/s]Capturing num tokens (num_tokens=352 avail_mem=84.37 GB):  57%|█████▋    | 33/58 [00:07<00:03,  7.49it/s]

    Capturing num tokens (num_tokens=352 avail_mem=84.37 GB):  59%|█████▊    | 34/58 [00:07<00:02,  8.08it/s]Capturing num tokens (num_tokens=320 avail_mem=84.19 GB):  59%|█████▊    | 34/58 [00:07<00:02,  8.08it/s]Capturing num tokens (num_tokens=320 avail_mem=84.19 GB):  60%|██████    | 35/58 [00:07<00:02,  8.36it/s]Capturing num tokens (num_tokens=288 avail_mem=84.36 GB):  60%|██████    | 35/58 [00:07<00:02,  8.36it/s]

    Capturing num tokens (num_tokens=288 avail_mem=84.36 GB):  62%|██████▏   | 36/58 [00:07<00:02,  8.44it/s]Capturing num tokens (num_tokens=256 avail_mem=84.35 GB):  62%|██████▏   | 36/58 [00:07<00:02,  8.44it/s]Capturing num tokens (num_tokens=240 avail_mem=84.23 GB):  62%|██████▏   | 36/58 [00:07<00:02,  8.44it/s]Capturing num tokens (num_tokens=240 avail_mem=84.23 GB):  66%|██████▌   | 38/58 [00:07<00:02,  9.66it/s]Capturing num tokens (num_tokens=224 avail_mem=84.33 GB):  66%|██████▌   | 38/58 [00:07<00:02,  9.66it/s]

    Capturing num tokens (num_tokens=208 avail_mem=84.33 GB):  66%|██████▌   | 38/58 [00:07<00:02,  9.66it/s]Capturing num tokens (num_tokens=208 avail_mem=84.33 GB):  69%|██████▉   | 40/58 [00:07<00:01,  9.83it/s]Capturing num tokens (num_tokens=192 avail_mem=84.33 GB):  69%|██████▉   | 40/58 [00:07<00:01,  9.83it/s]Capturing num tokens (num_tokens=176 avail_mem=84.32 GB):  69%|██████▉   | 40/58 [00:07<00:01,  9.83it/s]

    Capturing num tokens (num_tokens=176 avail_mem=84.32 GB):  72%|███████▏  | 42/58 [00:07<00:01, 10.58it/s]Capturing num tokens (num_tokens=160 avail_mem=84.26 GB):  72%|███████▏  | 42/58 [00:07<00:01, 10.58it/s]Capturing num tokens (num_tokens=144 avail_mem=84.30 GB):  72%|███████▏  | 42/58 [00:07<00:01, 10.58it/s]Capturing num tokens (num_tokens=144 avail_mem=84.30 GB):  76%|███████▌  | 44/58 [00:08<00:01, 10.71it/s]Capturing num tokens (num_tokens=128 avail_mem=84.31 GB):  76%|███████▌  | 44/58 [00:08<00:01, 10.71it/s]

    Capturing num tokens (num_tokens=112 avail_mem=84.31 GB):  76%|███████▌  | 44/58 [00:08<00:01, 10.71it/s]Capturing num tokens (num_tokens=112 avail_mem=84.31 GB):  79%|███████▉  | 46/58 [00:08<00:01, 10.84it/s]Capturing num tokens (num_tokens=96 avail_mem=84.30 GB):  79%|███████▉  | 46/58 [00:08<00:01, 10.84it/s] Capturing num tokens (num_tokens=80 avail_mem=84.24 GB):  79%|███████▉  | 46/58 [00:08<00:01, 10.84it/s]

    Capturing num tokens (num_tokens=80 avail_mem=84.24 GB):  83%|████████▎ | 48/58 [00:08<00:00, 11.67it/s]Capturing num tokens (num_tokens=64 avail_mem=84.28 GB):  83%|████████▎ | 48/58 [00:08<00:00, 11.67it/s]Capturing num tokens (num_tokens=48 avail_mem=84.27 GB):  83%|████████▎ | 48/58 [00:08<00:00, 11.67it/s]Capturing num tokens (num_tokens=48 avail_mem=84.27 GB):  86%|████████▌ | 50/58 [00:08<00:00, 11.78it/s]Capturing num tokens (num_tokens=32 avail_mem=84.27 GB):  86%|████████▌ | 50/58 [00:08<00:00, 11.78it/s]

    Capturing num tokens (num_tokens=28 avail_mem=84.26 GB):  86%|████████▌ | 50/58 [00:08<00:00, 11.78it/s]Capturing num tokens (num_tokens=28 avail_mem=84.26 GB):  90%|████████▉ | 52/58 [00:08<00:00, 11.91it/s]Capturing num tokens (num_tokens=24 avail_mem=84.26 GB):  90%|████████▉ | 52/58 [00:08<00:00, 11.91it/s]Capturing num tokens (num_tokens=20 avail_mem=84.25 GB):  90%|████████▉ | 52/58 [00:08<00:00, 11.91it/s]

    Capturing num tokens (num_tokens=20 avail_mem=84.25 GB):  93%|█████████▎| 54/58 [00:08<00:00, 12.68it/s]Capturing num tokens (num_tokens=16 avail_mem=84.22 GB):  93%|█████████▎| 54/58 [00:08<00:00, 12.68it/s]Capturing num tokens (num_tokens=12 avail_mem=84.23 GB):  93%|█████████▎| 54/58 [00:08<00:00, 12.68it/s]Capturing num tokens (num_tokens=12 avail_mem=84.23 GB):  97%|█████████▋| 56/58 [00:09<00:00, 12.90it/s]Capturing num tokens (num_tokens=8 avail_mem=84.23 GB):  97%|█████████▋| 56/58 [00:09<00:00, 12.90it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=84.22 GB):  97%|█████████▋| 56/58 [00:09<00:00, 12.90it/s]Capturing num tokens (num_tokens=4 avail_mem=84.22 GB): 100%|██████████| 58/58 [00:09<00:00, 13.18it/s]Capturing num tokens (num_tokens=4 avail_mem=84.22 GB): 100%|██████████| 58/58 [00:09<00:00,  6.34it/s]



<strong style='color: #00008B;'>==== Original Output ====</strong>



<strong style='color: #00008B;'>First, I recognize that the user is asking for the sum of 1 and 3.<br><br>To solve this, I will add the two numbers together.<br><br>Adding 1 and 3 gives me a total of 4.<br><br>Therefore, the final answer is 4.<br></think><br><br>**Solution:**<br><br>To find the sum of 1 and 3, follow these simple steps:<br><br>1. **Start with the first number:**<br>   <br>   \[<br>   1<br>   \]<br><br>2. **Add the second number to the first:**<br>   <br>   \[<br>   1 + 3<br>   \]<br><br>3. **Calculate the sum:**<br>   <br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Final Answer:** \(\boxed{4}\)</strong>



<strong style='color: #00008B;'>==== Reasoning ====</strong>



<strong style='color: #00008B;'>First, I recognize that the user is asking for the sum of 1 and 3.<br><br>To solve this, I will add the two numbers together.<br><br>Adding 1 and 3 gives me a total of 4.<br><br>Therefore, the final answer is 4.<br></strong>



<strong style='color: #00008B;'>==== Text ====</strong>



<strong style='color: #00008B;'>**Solution:**<br><br>To find the sum of 1 and 3, follow these simple steps:<br><br>1. **Start with the first number:**<br>   <br>   \[<br>   1<br>   \]<br><br>2. **Add the second number to the first:**<br>   <br>   \[<br>   1 + 3<br>   \]<br><br>3. **Calculate the sum:**<br>   <br>   \[<br>   1 + 3 = 4<br>   \]<br><br>**Final Answer:** \(\boxed{4}\)</strong>



```python
llm.shutdown()
```

## Supporting New Reasoning Model Schemas

For future reasoning models, you can implement the reasoning parser as a subclass of `BaseReasoningFormatDetector` in `python/sglang/srt/reasoning_parser.py` and specify the reasoning parser for new reasoning model schemas accordingly.
