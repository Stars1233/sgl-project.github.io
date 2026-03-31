# Structured Outputs For Reasoning Models

When working with reasoning models that use special tokens like `<think>...</think>` to denote reasoning sections, you might want to allow free-form text within these sections while still enforcing grammar constraints on the rest of the output.

SGLang provides a feature to disable grammar restrictions within reasoning sections. This is particularly useful for models that need to perform complex reasoning steps before providing a structured output.

To enable this feature, use the `--reasoning-parser` flag which decide the think_end_token, such as `</think>`, when launching the server. You can also specify the reasoning parser using the `--reasoning-parser` flag.

## Supported Models

Currently, SGLang supports the following reasoning models:
- [DeepSeek R1 series](https://huggingface.co/collections/deepseek-ai/deepseek-r1-678e1e131c0169c0bc89728d): The reasoning content is wrapped with `<think>` and `</think>` tags.
- [QwQ](https://huggingface.co/Qwen/QwQ-32B): The reasoning content is wrapped with `<think>` and `</think>` tags.


## Usage

## OpenAI Compatible API

Specify the `--grammar-backend`, `--reasoning-parser` option.


```python
import openai
import os

from sglang.test.doc_patch import launch_server_cmd
from sglang.utils import wait_for_server, print_highlight, terminate_process

os.environ["TOKENIZERS_PARALLELISM"] = "false"


server_process, port = launch_server_cmd(
    "python -m sglang.launch_server --model-path deepseek-ai/DeepSeek-R1-Distill-Qwen-7B --host 0.0.0.0 --reasoning-parser deepseek-r1 --log-level warning"
)

wait_for_server(f"http://localhost:{port}", process=server_process)
client = openai.Client(base_url=f"http://127.0.0.1:{port}/v1", api_key="None")
```

    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:173: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-03-31 13:56:22.169 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 13:56:22] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 13:56:22.169 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 13:56:22] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 13:56:22.170 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 13:56:22] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 13:56:22.170 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 13:56:22] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 13:56:22.170 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 13:56:22] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:00<00:00,  1.05it/s]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.19s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.16s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:31,  2.65s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:31,  2.65s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:14,  1.33s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:14,  1.33s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:44,  1.23it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:44,  1.23it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:30,  1.77it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:30,  1.77it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:03<00:22,  2.40it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:03<00:22,  2.40it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:03<00:17,  3.02it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:03<00:17,  3.02it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:03<00:14,  3.53it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:03<00:14,  3.53it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:12,  4.14it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:12,  4.14it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.03it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.03it/s]

    Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.03it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:06,  6.72it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:06,  6.72it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:06,  6.72it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:04<00:05,  8.24it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:04<00:05,  8.24it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:04<00:05,  8.24it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:04<00:04,  9.83it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:04<00:04,  9.83it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:04<00:04,  9.83it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:04<00:03, 11.71it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:04<00:03, 11.71it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:04<00:03, 11.71it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:04<00:03, 11.71it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:04<00:02, 14.85it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:04<00:02, 14.85it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:04<00:02, 14.85it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:04<00:02, 14.85it/s]

    Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:05<00:02, 14.85it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:05<00:01, 20.12it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:05<00:01, 20.12it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:05<00:01, 20.12it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:05<00:01, 20.12it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:05<00:01, 20.12it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:05<00:01, 20.12it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:05<00:01, 26.71it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:05<00:01, 26.71it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:05<00:01, 26.71it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:05<00:01, 26.71it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:05<00:01, 26.71it/s]

    Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:05<00:01, 26.71it/s]Compiling num tokens (num_tokens=320):  50%|█████     | 29/58 [00:05<00:01, 26.71it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:05<00:00, 33.47it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:05<00:00, 33.47it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:05<00:00, 33.47it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:05<00:00, 33.47it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:05<00:00, 33.47it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:05<00:00, 33.47it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:05<00:00, 33.47it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:05<00:00, 38.78it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:05<00:00, 38.78it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:05<00:00, 38.78it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:05<00:00, 38.78it/s]

    Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:05<00:00, 38.78it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:05<00:00, 38.78it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:05<00:00, 40.98it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:05<00:00, 40.98it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:05<00:00, 40.98it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:05<00:00, 40.98it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:05<00:00, 40.98it/s]Compiling num tokens (num_tokens=32):  79%|███████▉  | 46/58 [00:05<00:00, 40.98it/s]Compiling num tokens (num_tokens=28):  79%|███████▉  | 46/58 [00:05<00:00, 40.98it/s]Compiling num tokens (num_tokens=24):  79%|███████▉  | 46/58 [00:05<00:00, 40.98it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:05<00:00, 47.34it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:05<00:00, 47.34it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:05<00:00, 47.34it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:05<00:00, 47.34it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:05<00:00, 47.34it/s] Compiling num tokens (num_tokens=4):  91%|█████████▏| 53/58 [00:05<00:00, 47.34it/s]

    Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:05<00:00, 10.27it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=42.53 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=42.53 GB):   2%|▏         | 1/58 [00:00<00:16,  3.38it/s]Capturing num tokens (num_tokens=7680 avail_mem=42.50 GB):   2%|▏         | 1/58 [00:00<00:16,  3.38it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=42.50 GB):   3%|▎         | 2/58 [00:00<00:15,  3.57it/s]Capturing num tokens (num_tokens=7168 avail_mem=42.50 GB):   3%|▎         | 2/58 [00:00<00:15,  3.57it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=42.50 GB):   5%|▌         | 3/58 [00:00<00:14,  3.81it/s]Capturing num tokens (num_tokens=6656 avail_mem=42.50 GB):   5%|▌         | 3/58 [00:00<00:14,  3.81it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=42.50 GB):   7%|▋         | 4/58 [00:01<00:13,  4.10it/s]Capturing num tokens (num_tokens=6144 avail_mem=42.51 GB):   7%|▋         | 4/58 [00:01<00:13,  4.10it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=42.51 GB):   9%|▊         | 5/58 [00:01<00:12,  4.35it/s]Capturing num tokens (num_tokens=5632 avail_mem=42.51 GB):   9%|▊         | 5/58 [00:01<00:12,  4.35it/s]Capturing num tokens (num_tokens=5632 avail_mem=42.51 GB):  10%|█         | 6/58 [00:01<00:10,  4.73it/s]Capturing num tokens (num_tokens=5120 avail_mem=42.51 GB):  10%|█         | 6/58 [00:01<00:10,  4.73it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=42.51 GB):  12%|█▏        | 7/58 [00:01<00:09,  5.10it/s]Capturing num tokens (num_tokens=4608 avail_mem=42.52 GB):  12%|█▏        | 7/58 [00:01<00:09,  5.10it/s]Capturing num tokens (num_tokens=4608 avail_mem=42.52 GB):  14%|█▍        | 8/58 [00:01<00:08,  5.62it/s]Capturing num tokens (num_tokens=4096 avail_mem=42.52 GB):  14%|█▍        | 8/58 [00:01<00:08,  5.62it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=42.52 GB):  16%|█▌        | 9/58 [00:01<00:08,  6.00it/s]Capturing num tokens (num_tokens=3840 avail_mem=42.53 GB):  16%|█▌        | 9/58 [00:01<00:08,  6.00it/s]Capturing num tokens (num_tokens=3840 avail_mem=42.53 GB):  17%|█▋        | 10/58 [00:01<00:07,  6.50it/s]Capturing num tokens (num_tokens=3584 avail_mem=42.52 GB):  17%|█▋        | 10/58 [00:01<00:07,  6.50it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=42.52 GB):  19%|█▉        | 11/58 [00:02<00:06,  6.98it/s]Capturing num tokens (num_tokens=3328 avail_mem=42.50 GB):  19%|█▉        | 11/58 [00:02<00:06,  6.98it/s]Capturing num tokens (num_tokens=3328 avail_mem=42.50 GB):  21%|██        | 12/58 [00:02<00:06,  7.30it/s]Capturing num tokens (num_tokens=3072 avail_mem=42.50 GB):  21%|██        | 12/58 [00:02<00:06,  7.30it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=42.50 GB):  22%|██▏       | 13/58 [00:02<00:05,  7.64it/s]Capturing num tokens (num_tokens=2816 avail_mem=42.50 GB):  22%|██▏       | 13/58 [00:02<00:05,  7.64it/s]Capturing num tokens (num_tokens=2816 avail_mem=42.50 GB):  24%|██▍       | 14/58 [00:02<00:06,  6.97it/s]Capturing num tokens (num_tokens=2560 avail_mem=42.50 GB):  24%|██▍       | 14/58 [00:02<00:06,  6.97it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=42.50 GB):  26%|██▌       | 15/58 [00:02<00:06,  6.39it/s]Capturing num tokens (num_tokens=2304 avail_mem=42.50 GB):  26%|██▌       | 15/58 [00:02<00:06,  6.39it/s]Capturing num tokens (num_tokens=2304 avail_mem=42.50 GB):  28%|██▊       | 16/58 [00:02<00:06,  6.34it/s]Capturing num tokens (num_tokens=2048 avail_mem=42.50 GB):  28%|██▊       | 16/58 [00:02<00:06,  6.34it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=42.50 GB):  29%|██▉       | 17/58 [00:02<00:06,  6.59it/s]Capturing num tokens (num_tokens=1792 avail_mem=42.50 GB):  29%|██▉       | 17/58 [00:02<00:06,  6.59it/s]Capturing num tokens (num_tokens=1536 avail_mem=42.50 GB):  29%|██▉       | 17/58 [00:03<00:06,  6.59it/s]Capturing num tokens (num_tokens=1536 avail_mem=42.50 GB):  33%|███▎      | 19/58 [00:03<00:04,  9.10it/s]Capturing num tokens (num_tokens=1280 avail_mem=42.50 GB):  33%|███▎      | 19/58 [00:03<00:04,  9.10it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=42.50 GB):  34%|███▍      | 20/58 [00:03<00:04,  8.31it/s]Capturing num tokens (num_tokens=1024 avail_mem=61.61 GB):  34%|███▍      | 20/58 [00:03<00:04,  8.31it/s]Capturing num tokens (num_tokens=960 avail_mem=61.61 GB):  34%|███▍      | 20/58 [00:03<00:04,  8.31it/s] Capturing num tokens (num_tokens=896 avail_mem=61.60 GB):  34%|███▍      | 20/58 [00:03<00:04,  8.31it/s]Capturing num tokens (num_tokens=896 avail_mem=61.60 GB):  40%|███▉      | 23/58 [00:03<00:02, 12.72it/s]Capturing num tokens (num_tokens=832 avail_mem=61.60 GB):  40%|███▉      | 23/58 [00:03<00:02, 12.72it/s]Capturing num tokens (num_tokens=768 avail_mem=61.59 GB):  40%|███▉      | 23/58 [00:03<00:02, 12.72it/s]Capturing num tokens (num_tokens=704 avail_mem=61.59 GB):  40%|███▉      | 23/58 [00:03<00:02, 12.72it/s]

    Capturing num tokens (num_tokens=704 avail_mem=61.59 GB):  45%|████▍     | 26/58 [00:03<00:01, 16.53it/s]Capturing num tokens (num_tokens=640 avail_mem=61.59 GB):  45%|████▍     | 26/58 [00:03<00:01, 16.53it/s]Capturing num tokens (num_tokens=576 avail_mem=61.59 GB):  45%|████▍     | 26/58 [00:03<00:01, 16.53it/s]Capturing num tokens (num_tokens=512 avail_mem=61.58 GB):  45%|████▍     | 26/58 [00:03<00:01, 16.53it/s]Capturing num tokens (num_tokens=480 avail_mem=61.58 GB):  45%|████▍     | 26/58 [00:03<00:01, 16.53it/s]Capturing num tokens (num_tokens=480 avail_mem=61.58 GB):  52%|█████▏    | 30/58 [00:03<00:01, 21.10it/s]Capturing num tokens (num_tokens=448 avail_mem=61.58 GB):  52%|█████▏    | 30/58 [00:03<00:01, 21.10it/s]Capturing num tokens (num_tokens=416 avail_mem=61.57 GB):  52%|█████▏    | 30/58 [00:03<00:01, 21.10it/s]Capturing num tokens (num_tokens=384 avail_mem=61.57 GB):  52%|█████▏    | 30/58 [00:03<00:01, 21.10it/s]

    Capturing num tokens (num_tokens=384 avail_mem=61.57 GB):  57%|█████▋    | 33/58 [00:03<00:01, 22.18it/s]Capturing num tokens (num_tokens=352 avail_mem=61.56 GB):  57%|█████▋    | 33/58 [00:03<00:01, 22.18it/s]Capturing num tokens (num_tokens=320 avail_mem=61.56 GB):  57%|█████▋    | 33/58 [00:03<00:01, 22.18it/s]Capturing num tokens (num_tokens=288 avail_mem=61.56 GB):  57%|█████▋    | 33/58 [00:03<00:01, 22.18it/s]Capturing num tokens (num_tokens=256 avail_mem=61.55 GB):  57%|█████▋    | 33/58 [00:03<00:01, 22.18it/s]Capturing num tokens (num_tokens=256 avail_mem=61.55 GB):  64%|██████▍   | 37/58 [00:03<00:00, 26.31it/s]Capturing num tokens (num_tokens=240 avail_mem=61.55 GB):  64%|██████▍   | 37/58 [00:03<00:00, 26.31it/s]Capturing num tokens (num_tokens=224 avail_mem=61.54 GB):  64%|██████▍   | 37/58 [00:03<00:00, 26.31it/s]Capturing num tokens (num_tokens=208 avail_mem=61.54 GB):  64%|██████▍   | 37/58 [00:03<00:00, 26.31it/s]Capturing num tokens (num_tokens=192 avail_mem=61.54 GB):  64%|██████▍   | 37/58 [00:03<00:00, 26.31it/s]

    Capturing num tokens (num_tokens=192 avail_mem=61.54 GB):  71%|███████   | 41/58 [00:03<00:00, 29.67it/s]Capturing num tokens (num_tokens=176 avail_mem=61.53 GB):  71%|███████   | 41/58 [00:03<00:00, 29.67it/s]Capturing num tokens (num_tokens=160 avail_mem=61.53 GB):  71%|███████   | 41/58 [00:03<00:00, 29.67it/s]Capturing num tokens (num_tokens=144 avail_mem=61.52 GB):  71%|███████   | 41/58 [00:03<00:00, 29.67it/s]Capturing num tokens (num_tokens=128 avail_mem=61.53 GB):  71%|███████   | 41/58 [00:04<00:00, 29.67it/s]Capturing num tokens (num_tokens=128 avail_mem=61.53 GB):  78%|███████▊  | 45/58 [00:04<00:00, 32.37it/s]Capturing num tokens (num_tokens=112 avail_mem=61.53 GB):  78%|███████▊  | 45/58 [00:04<00:00, 32.37it/s]Capturing num tokens (num_tokens=96 avail_mem=61.53 GB):  78%|███████▊  | 45/58 [00:04<00:00, 32.37it/s] Capturing num tokens (num_tokens=80 avail_mem=61.52 GB):  78%|███████▊  | 45/58 [00:04<00:00, 32.37it/s]Capturing num tokens (num_tokens=64 avail_mem=61.52 GB):  78%|███████▊  | 45/58 [00:04<00:00, 32.37it/s]

    Capturing num tokens (num_tokens=48 avail_mem=61.52 GB):  78%|███████▊  | 45/58 [00:04<00:00, 32.37it/s]Capturing num tokens (num_tokens=48 avail_mem=61.52 GB):  86%|████████▌ | 50/58 [00:04<00:00, 35.02it/s]Capturing num tokens (num_tokens=32 avail_mem=61.51 GB):  86%|████████▌ | 50/58 [00:04<00:00, 35.02it/s]Capturing num tokens (num_tokens=28 avail_mem=61.51 GB):  86%|████████▌ | 50/58 [00:04<00:00, 35.02it/s]Capturing num tokens (num_tokens=24 avail_mem=61.51 GB):  86%|████████▌ | 50/58 [00:04<00:00, 35.02it/s]Capturing num tokens (num_tokens=20 avail_mem=61.50 GB):  86%|████████▌ | 50/58 [00:04<00:00, 35.02it/s]Capturing num tokens (num_tokens=16 avail_mem=61.50 GB):  86%|████████▌ | 50/58 [00:04<00:00, 35.02it/s]Capturing num tokens (num_tokens=16 avail_mem=61.50 GB):  95%|█████████▍| 55/58 [00:04<00:00, 37.13it/s]Capturing num tokens (num_tokens=12 avail_mem=61.50 GB):  95%|█████████▍| 55/58 [00:04<00:00, 37.13it/s]Capturing num tokens (num_tokens=8 avail_mem=61.49 GB):  95%|█████████▍| 55/58 [00:04<00:00, 37.13it/s] Capturing num tokens (num_tokens=4 avail_mem=61.49 GB):  95%|█████████▍| 55/58 [00:04<00:00, 37.13it/s]

    Capturing num tokens (num_tokens=4 avail_mem=61.49 GB): 100%|██████████| 58/58 [00:04<00:00, 13.30it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>


### JSON

you can directly define a JSON schema or use [Pydantic](https://docs.pydantic.dev/latest/) to define and validate the response.

**Using Pydantic**


```python
from pydantic import BaseModel, Field


# Define the schema using Pydantic
class CapitalInfo(BaseModel):
    name: str = Field(..., pattern=r"^\w+$", description="Name of the capital city")
    population: int = Field(..., description="Population of the capital city")


response = client.chat.completions.create(
    model="deepseek-ai/DeepSeek-R1-Distill-Qwen-7B",
    messages=[
        {
            "role": "assistant",
            "content": "Give me the information and population of the capital of France in the JSON format.",
        },
    ],
    temperature=0,
    max_tokens=2048,
    response_format={
        "type": "json_schema",
        "json_schema": {
            "name": "foo",
            # convert the pydantic model to json schema
            "schema": CapitalInfo.model_json_schema(),
        },
    },
)

print_highlight(
    f"reasoing_content: {response.choices[0].message.reasoning_content}\n\ncontent: {response.choices[0].message.content}"
)
```


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France and its population. I know that the capital of France is Paris, but I'm not exactly sure about the current population numbers. I remember that Paris is a very big city, but I think it's not the largest in the world. Maybe around 20 million? I'm not certain, though. I should probably double-check that. <br><br>Wait, I think I heard somewhere that Paris has a population over 21 million. Maybe 21.6 million? I'm not sure if that's accurate. I should look up the latest data to confirm. Also, I wonder if the population includes just the city proper or the entire metropolitan area. I think sometimes population counts include the broader area, so that might be a consideration. <br><br>I should make sure to present the information clearly in JSON format, as the user requested. So, the key would be "capital" with the value "Paris," and another key for "population." I'll need to include the number, maybe with a note about whether it's an approximate figure or the most recent data. <br><br>I'm a bit confused about whether the population figure I have is up to date. I think the population can change over time due to births, deaths, and migration. So, it's important to mention that the figure is approximate or based on the latest available data. <br><br>Also, I should consider the units. Is it in thousands, millions, or just a plain number? I think it's best to present it as a number without units, just the raw count. <br><br>Putting it all together, I'll structure the JSON with the keys "capital" and "population," and include the population as 21.6 million. I'll add a comment or note that the population figure is approximate. That should cover everything the user asked for.<br><br><br>content: {<br><br>"name": "Paris",<br>"population": 21600000}</strong>


**JSON Schema Directly**



```python
import json

json_schema = json.dumps(
    {
        "type": "object",
        "properties": {
            "name": {"type": "string", "pattern": "^[\\w]+$"},
            "population": {"type": "integer"},
        },
        "required": ["name", "population"],
    }
)

response = client.chat.completions.create(
    model="deepseek-ai/DeepSeek-R1-Distill-Qwen-7B",
    messages=[
        {
            "role": "assistant",
            "content": "Give me the information and population of the capital of France in the JSON format.",
        },
    ],
    temperature=0,
    max_tokens=2048,
    response_format={
        "type": "json_schema",
        "json_schema": {"name": "foo", "schema": json.loads(json_schema)},
    },
)

print_highlight(
    f"reasoing_content: {response.choices[0].message.reasoning_content}\n\ncontent: {response.choices[0].message.content}"
)
```


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France and its population. I know that the capital of France is Paris, but I'm not exactly sure about the current population numbers. I remember that Paris is a very big city, but I think it's not the largest in the world. Maybe around 20 million? I'm not certain, though. I should probably check that.<br><br>Wait, I think the population has been growing over the years. I recall reading somewhere that it's over 21 million now. Maybe around 21.6 million? I'm not sure if that's the exact number or just an estimate. I should look it up to confirm. Also, I should make sure that Paris is indeed the capital and not another city like Lyon or Marseille.<br><br>I'm pretty confident that Paris is the capital, but just to be thorough, I'll double-check. The official capital of France is Paris, so that's settled. Now, for the population, I think it's a bit tricky because population numbers can change yearly. I might find a figure that's a bit higher or lower depending on the source. Maybe I should look for the most recent data available, perhaps from 2023, to get the accurate number.<br><br>I think the population is around 21.6 million, but I'm not 100% sure. I should also consider whether this includes only the city proper or the entire metropolitan area. Sometimes, population figures can include surrounding suburbs and satellite towns. If the data I find includes the metropolitan area, that might be a different number. I need to clarify that.<br><br>Another thing to consider is that the population can vary slightly depending on the source. Some sources might use estimates, while others might have more precise data. I should look for a reliable source, like the official statistics from the French National Institute of Statistics and Economic Studies (INSEE) or another reputable organization. That way, I can be more confident in the accuracy of the number.<br><br>In summary, I'm pretty sure the capital is Paris, and the population is around 21.6 million, but I should verify this information to ensure it's correct. Once I have the exact number, I can present it in the JSON format as requested.<br><br><br>content: {<br><br>"name": "Paris",<br>"population": 21620000<br>}</strong>


### EBNF


```python
ebnf_grammar = """
root ::= city | description
city ::= "London" | "Paris" | "Berlin" | "Rome"
description ::= city " is " status
status ::= "the capital of " country
country ::= "England" | "France" | "Germany" | "Italy"
"""

response = client.chat.completions.create(
    model="deepseek-ai/DeepSeek-R1-Distill-Qwen-7B",
    messages=[
        {"role": "system", "content": "You are a helpful geography bot."},
        {
            "role": "assistant",
            "content": "Give me the information and population of the capital of France in the JSON format.",
        },
    ],
    temperature=0,
    max_tokens=2048,
    extra_body={"ebnf": ebnf_grammar},
)

print_highlight(
    f"reasoing_content: {response.choices[0].message.reasoning_content}\n\ncontent: {response.choices[0].message.content}"
)
```


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France and its population. I know that the capital of France is Paris, but I'm not exactly sure about the current population. I think it's a big city, maybe around 3 million? I remember hearing that Paris is one of the most populous cities in Europe, but I'm not certain about the exact number. Maybe I should check some sources or think about recent growth. I think the population has been increasing over the years, so perhaps it's now over 3.5 million? I'm a bit confused because sometimes I hear different numbers, so I should make sure. Maybe I can recall that Paris has a metropolitan area that's much larger, but the city proper is around 3.5 million. I think I'll go with that for now, but I'm not 100% sure. I should probably double-check this information to be accurate.<br><br><br>content: Paris is the capital of France</strong>


### Regular expression


```python
response = client.chat.completions.create(
    model="deepseek-ai/DeepSeek-R1-Distill-Qwen-7B",
    messages=[
        {"role": "assistant", "content": "What is the capital of France?"},
    ],
    temperature=0,
    max_tokens=2048,
    extra_body={"regex": "(Paris|London)"},
)

print_highlight(
    f"reasoing_content: {response.choices[0].message.reasoning_content}\n\ncontent: {response.choices[0].message.content}"
)
```


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France. Hmm, I remember learning about France in school, but I'm not 100% sure. Let me think. I know that Paris is a major city in France, and it's often referred to as the "City of Light." People go there for museums, landmarks like the Eiffel Tower, and it's a cultural hub. But is it the capital?<br><br>Wait, I think the capital is the official seat of government, right? So maybe Paris is both the capital and the most famous city. But I'm not entirely certain. I recall that some countries have their capital in a different city than their main tourist attraction. For example, I think Brazil's capital is not Rio de Janeiro, which is more famous. So maybe France is like that too.<br><br>I should try to remember any specific information. I think the French government declares Paris as the capital. Yeah, that sounds right. I also remember that the Eiffel Tower is in Paris, and it's a symbol of the country. So if Paris is the capital, then that makes sense. But I'm a bit confused because sometimes people say "the capital of France is Paris," but I want to make sure I'm not mixing it up with other countries.<br><br>Let me think about other capitals I know. For example, Germany's capital is Berlin, Italy's is Rome, Spain's is Madrid. So France's capital should be a major city in the north, maybe. Paris is in the north, so that fits. I think I've heard it said that Paris is the capital, so I'm pretty confident now. Yeah, I think I'm right.<br><br><br>content: Paris</strong>


### Structural Tag


```python
tool_get_current_weather = {
    "type": "function",
    "function": {
        "name": "get_current_weather",
        "description": "Get the current weather in a given location",
        "parameters": {
            "type": "object",
            "properties": {
                "city": {
                    "type": "string",
                    "description": "The city to find the weather for, e.g. 'San Francisco'",
                },
                "state": {
                    "type": "string",
                    "description": "the two-letter abbreviation for the state that the city is"
                    " in, e.g. 'CA' which would mean 'California'",
                },
                "unit": {
                    "type": "string",
                    "description": "The unit to fetch the temperature in",
                    "enum": ["celsius", "fahrenheit"],
                },
            },
            "required": ["city", "state", "unit"],
        },
    },
}

tool_get_current_date = {
    "type": "function",
    "function": {
        "name": "get_current_date",
        "description": "Get the current date and time for a given timezone",
        "parameters": {
            "type": "object",
            "properties": {
                "timezone": {
                    "type": "string",
                    "description": "The timezone to fetch the current date and time for, e.g. 'America/New_York'",
                }
            },
            "required": ["timezone"],
        },
    },
}

schema_get_current_weather = tool_get_current_weather["function"]["parameters"]
schema_get_current_date = tool_get_current_date["function"]["parameters"]


def get_messages():
    return [
        {
            "role": "system",
            "content": f"""
# Tool Instructions
- Always execute python code in messages that you share.
- When looking for real time information use relevant functions if available else fallback to brave_search
You have access to the following functions:
Use the function 'get_current_weather' to: Get the current weather in a given location
{tool_get_current_weather["function"]}
Use the function 'get_current_date' to: Get the current date and time for a given timezone
{tool_get_current_date["function"]}
If a you choose to call a function ONLY reply in the following format:
<{{start_tag}}={{function_name}}>{{parameters}}{{end_tag}}
where
start_tag => `<function`
parameters => a JSON dict with the function argument name as key and function argument value as value.
end_tag => `</function>`
Here is an example,
<function=example_function_name>{{"example_name": "example_value"}}</function>
Reminder:
- Function calls MUST follow the specified format
- Required parameters MUST be specified
- Only call one function at a time
- Put the entire function call reply on one line
- Always add your sources when using search results to answer the user query
You are a helpful assistant.""",
        },
        {
            "role": "assistant",
            "content": "You are in New York. Please get the current date and time, and the weather.",
        },
    ]


messages = get_messages()

response = client.chat.completions.create(
    model="deepseek-ai/DeepSeek-R1-Distill-Qwen-7B",
    messages=messages,
    response_format={
        "type": "structural_tag",
        "max_new_tokens": 2048,
        "structures": [
            {
                "begin": "<function=get_current_weather>",
                "schema": schema_get_current_weather,
                "end": "</function>",
            },
            {
                "begin": "<function=get_current_date>",
                "schema": schema_get_current_date,
                "end": "</function>",
            },
        ],
        "triggers": ["<function="],
    },
)

print_highlight(
    f"reasoing_content: {response.choices[0].message.reasoning_content}\n\ncontent: {response.choices[0].message.content}"
)
```


<strong style='color: #00008B;'>reasoing_content: Okay, so the user is in New York and wants to know the current date and time as well as the weather. I need to figure out how to get this information using the functions provided. <br><br>First, I should use the get_current_date function. The parameters required are the timezone, which in this case is 'America/New_York'. I'll structure the function call with that parameter.<br><br>Next, the weather information. Since the user is in New York, I can use the get_current_weather function. The city is 'New York', the state is 'NY', and the unit they mentioned wasn't specified, but since the function allows for 'celsius' or 'fahrenheit', I'll include 'unit': 'fahrenheit' as a common choice, though the user might prefer Celsius. I'll make sure to include all required parameters for this function.<br><br>I need to ensure that each function call is separate and follows the specified format. So, I'll write the get_current_date call first, then the get_current_weather call, each with their respective parameters.<br><br>Finally, I'll mention the units for clarity, noting that Fahrenheit is used but the user can switch if needed.<br><br><br>content: <function=get_current_date>{"timezone": "America/New_York"}</function><br><br><function=get_current_weather>{"city": "New York", "state": "NY", "unit": "fahrenheit"}</function><br><br>The current date and time in New York is [date and time], and the weather in New York is [weather information]. Fahrenheit is used for temperature units, but you can switch to Celsius if needed.</strong>


## Native API and SGLang Runtime (SRT)

> Note: For native API, as a work-around, you need to set `require_reasoning` argument to `True` to ensure the model will think before generating the structured output. It's not required for chat-completion API.

### JSON

**Using Pydantic**


```python
import requests
from pydantic import BaseModel, Field
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("deepseek-ai/DeepSeek-R1-Distill-Qwen-7B")


# Define the schema using Pydantic
class CapitalInfo(BaseModel):
    name: str = Field(..., pattern=r"^\w+$", description="Name of the capital city")
    population: int = Field(..., description="Population of the capital city")


messages = [
    {
        "role": "assistant",
        "content": "Give me the information and population of the capital of France in the JSON format.",
    },
]
text = tokenizer.apply_chat_template(
    messages, tokenize=False, add_generation_prompt=True, return_dict=False
)
# Make API request
response = requests.post(
    f"http://localhost:{port}/generate",
    json={
        "text": text,
        "require_reasoning": True,
        "sampling_params": {
            "temperature": 0,
            "max_new_tokens": 2048,
            "json_schema": json.dumps(CapitalInfo.model_json_schema()),
        },
    },
)
print(response.json())


reasoing_content = response.json()["text"].split("</think>")[0]
content = response.json()["text"].split("</think>")[1]
print_highlight(f"reasoing_content: {reasoing_content}\n\ncontent: {content}")
```

    {'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down. First, I need to identify what the capital of France is. I know that Paris is the capital, so that\'s straightforward. \n\nNext, I need to find the population. I remember that Paris is a major city, so its population is quite large. I think it\'s over 3 million, but I\'m not exactly sure of the exact number. Maybe I should double-check that. \n\nWait, I recall that the population figure can vary depending on the source and the year. The user didn\'t specify a particular year, so I should probably go with the most recent estimate. I believe the population is around 3,500,000 as of 2023. \n\nNow, I need to structure this information into a JSON format. JSON typically uses key-value pairs, so I\'ll create an object with keys like "city", "population", and maybe "country" since the user mentioned France. \n\nI should make sure the keys are in English to keep it clear. The city is Paris, the population is 3,500,000, and the country is France. I\'ll format this into a JSON object, ensuring proper syntax with commas and quotation marks. \n\nI also need to present this in a way that\'s easy to read, so I\'ll put each key on a new line. That way, the user can quickly see the information without confusion. \n\nI wonder if the user needs more details, like the exact current population or additional statistics. But since they only asked for the capital and population, I\'ll stick to that. \n\nLastly, I\'ll make sure the JSON is valid by checking the syntax. No trailing commas, proper use of braces, and correct quotation marks. That should cover everything the user needs.\n</think>{\n  "name": "Paris",\n  "population": 3500000\n}', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 13, 5512, 11, 358, 1184, 311, 10542, 1128, 279, 6722, 315, 9625, 374, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4710, 5847, 11, 358, 1184, 311, 1477, 279, 7042, 13, 358, 6099, 429, 12095, 374, 264, 3598, 3283, 11, 773, 1181, 7042, 374, 5008, 3460, 13, 358, 1744, 432, 594, 916, 220, 18, 3526, 11, 714, 358, 2776, 537, 6896, 2704, 315, 279, 4734, 1372, 13, 10696, 358, 1265, 1990, 15934, 429, 13, 4710, 14190, 11, 358, 19091, 429, 279, 7042, 7071, 646, 13289, 11649, 389, 279, 2530, 323, 279, 1042, 13, 576, 1196, 3207, 944, 13837, 264, 3953, 1042, 11, 773, 358, 1265, 4658, 728, 448, 279, 1429, 3213, 16045, 13, 358, 4411, 279, 7042, 374, 2163, 220, 18, 11, 20, 15, 15, 11, 15, 15, 15, 438, 315, 220, 17, 15, 17, 18, 13, 4710, 7039, 11, 358, 1184, 311, 5944, 419, 1995, 1119, 264, 4718, 3561, 13, 4718, 11136, 5711, 1376, 19083, 13530, 11, 773, 358, 3278, 1855, 458, 1633, 448, 6894, 1075, 330, 8926, 497, 330, 44441, 497, 323, 7196, 330, 11141, 1, 2474, 279, 1196, 9733, 9625, 13, 4710, 40, 1265, 1281, 2704, 279, 6894, 525, 304, 6364, 311, 2506, 432, 2797, 13, 576, 3283, 374, 12095, 11, 279, 7042, 374, 220, 18, 11, 20, 15, 15, 11, 15, 15, 15, 11, 323, 279, 3146, 374, 9625, 13, 358, 3278, 3561, 419, 1119, 264, 4718, 1633, 11, 22573, 6169, 19482, 448, 76602, 323, 54231, 15423, 13, 4710, 40, 1083, 1184, 311, 3042, 419, 304, 264, 1616, 429, 594, 4135, 311, 1349, 11, 773, 358, 3278, 2182, 1817, 1376, 389, 264, 501, 1555, 13, 2938, 1616, 11, 279, 1196, 646, 6157, 1490, 279, 1995, 2041, 21340, 13, 4710, 40, 5775, 421, 279, 1196, 3880, 803, 3565, 11, 1075, 279, 4734, 1482, 7042, 476, 5107, 13142, 13, 1988, 2474, 807, 1172, 4588, 369, 279, 6722, 323, 7042, 11, 358, 3278, 9214, 311, 429, 13, 4710, 80486, 11, 358, 3278, 1281, 2704, 279, 4718, 374, 2697, 553, 13295, 279, 19482, 13, 2308, 27748, 76602, 11, 6169, 990, 315, 59191, 11, 323, 4396, 54231, 15423, 13, 2938, 1265, 3421, 4297, 279, 1196, 3880, 624, 151649, 515, 220, 330, 606, 788, 330, 59604, 756, 220, 330, 44441, 788, 220, 18, 20, 15, 15, 15, 15, 15, 198, 92, 151643], 'meta_info': {'id': '9ca9d1318480483fa13af9a4347afeea', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 412, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 2.883166022016667, 'response_sent_to_client_ts': 1774965425.2940164}}



<strong style='color: #00008B;'>reasoing_content: Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down. First, I need to identify what the capital of France is. I know that Paris is the capital, so that's straightforward. <br><br>Next, I need to find the population. I remember that Paris is a major city, so its population is quite large. I think it's over 3 million, but I'm not exactly sure of the exact number. Maybe I should double-check that. <br><br>Wait, I recall that the population figure can vary depending on the source and the year. The user didn't specify a particular year, so I should probably go with the most recent estimate. I believe the population is around 3,500,000 as of 2023. <br><br>Now, I need to structure this information into a JSON format. JSON typically uses key-value pairs, so I'll create an object with keys like "city", "population", and maybe "country" since the user mentioned France. <br><br>I should make sure the keys are in English to keep it clear. The city is Paris, the population is 3,500,000, and the country is France. I'll format this into a JSON object, ensuring proper syntax with commas and quotation marks. <br><br>I also need to present this in a way that's easy to read, so I'll put each key on a new line. That way, the user can quickly see the information without confusion. <br><br>I wonder if the user needs more details, like the exact current population or additional statistics. But since they only asked for the capital and population, I'll stick to that. <br><br>Lastly, I'll make sure the JSON is valid by checking the syntax. No trailing commas, proper use of braces, and correct quotation marks. That should cover everything the user needs.<br><br><br>content: {<br>  "name": "Paris",<br>  "population": 3500000<br>}</strong>


**JSON Schema Directly**


```python
json_schema = json.dumps(
    {
        "type": "object",
        "properties": {
            "name": {"type": "string", "pattern": "^[\\w]+$"},
            "population": {"type": "integer"},
        },
        "required": ["name", "population"],
    }
)

# JSON
text = tokenizer.apply_chat_template(
    messages, tokenize=False, add_generation_prompt=True, return_dict=False
)
response = requests.post(
    f"http://localhost:{port}/generate",
    json={
        "text": text,
        "require_reasoning": True,
        "sampling_params": {
            "temperature": 0,
            "max_new_tokens": 2048,
            "json_schema": json_schema,
        },
    },
)

print_highlight(response.json())
```


<strong style='color: #00008B;'>{'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. I know that Paris is the capital, so that\'s straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I\'m not sure of the exact number. I think it\'s around 2 million, but I should verify that.\n\nI\'ll check a reliable source, maybe the official Paris Municipality website or a recent census. Let me see... Yes, according to the latest data, the population is approximately 2,174,300 as of 2023. That seems accurate.\n\nNext, I need to structure this information into a JSON format. JSON requires key-value pairs, so I\'ll create an object with keys like "city", "population", and "country". The city is Paris, the population is the number I found, and the country is France.\n\nI should make sure the JSON syntax is correct. Each key should be in quotes, and the values as well. The entire structure should be enclosed in curly braces. I\'ll format it properly to avoid any syntax errors.\n\nPutting it all together, the JSON object will have the city, population, and country. I\'ll present this to the user, making sure it\'s clear and easy to understand. I don\'t think the user needs anything more detailed, so this should suffice.\n\nI should also consider if the user might need additional information, like the area or age distribution, but since they only asked for population, I\'ll stick to that. Maybe they\'ll ask for more details later, but for now, this response should be helpful.\n</think>{\n  "name": "Paris",\n  "population": 2174300\n}', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1265, 1477, 279, 1429, 3213, 7042, 821, 13, 358, 6099, 429, 279, 7042, 315, 12095, 702, 1012, 7826, 11, 714, 358, 2776, 537, 2704, 315, 279, 4734, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 10146, 429, 382, 40, 3278, 1779, 264, 14720, 2530, 11, 7196, 279, 3946, 12095, 35703, 2719, 3910, 476, 264, 3213, 43602, 13, 6771, 752, 1490, 1112, 7414, 11, 4092, 311, 279, 5535, 821, 11, 279, 7042, 374, 13187, 220, 17, 11, 16, 22, 19, 11, 18, 15, 15, 438, 315, 220, 17, 15, 17, 18, 13, 2938, 4977, 13382, 382, 5847, 11, 358, 1184, 311, 5944, 419, 1995, 1119, 264, 4718, 3561, 13, 4718, 7460, 1376, 19083, 13530, 11, 773, 358, 3278, 1855, 458, 1633, 448, 6894, 1075, 330, 8926, 497, 330, 44441, 497, 323, 330, 11141, 3263, 576, 3283, 374, 12095, 11, 279, 7042, 374, 279, 1372, 358, 1730, 11, 323, 279, 3146, 374, 9625, 382, 40, 1265, 1281, 2704, 279, 4718, 19482, 374, 4396, 13, 8886, 1376, 1265, 387, 304, 17194, 11, 323, 279, 2750, 438, 1632, 13, 576, 4453, 5944, 1265, 387, 43810, 304, 68103, 59191, 13, 358, 3278, 3561, 432, 10277, 311, 5648, 894, 19482, 5975, 382, 97904, 432, 678, 3786, 11, 279, 4718, 1633, 686, 614, 279, 3283, 11, 7042, 11, 323, 3146, 13, 358, 3278, 3042, 419, 311, 279, 1196, 11, 3259, 2704, 432, 594, 2797, 323, 4135, 311, 3535, 13, 358, 1513, 944, 1744, 279, 1196, 3880, 4113, 803, 11682, 11, 773, 419, 1265, 76156, 382, 40, 1265, 1083, 2908, 421, 279, 1196, 2578, 1184, 5107, 1995, 11, 1075, 279, 3082, 476, 4231, 7982, 11, 714, 2474, 807, 1172, 4588, 369, 7042, 11, 358, 3278, 9214, 311, 429, 13, 10696, 807, 3278, 2548, 369, 803, 3565, 2937, 11, 714, 369, 1431, 11, 419, 2033, 1265, 387, 10950, 624, 151649, 515, 220, 330, 606, 788, 330, 59604, 756, 220, 330, 44441, 788, 220, 17, 16, 22, 19, 18, 15, 15, 198, 92, 151643], 'meta_info': {'id': '1ee2454c622a475bbc1e5f058344e1ad', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 384, 'cached_tokens': 22, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 2.9115200110245496, 'response_sent_to_client_ts': 1774965428.2144144}}</strong>


### EBNF


```python
response = requests.post(
    f"http://localhost:{port}/generate",
    json={
        "text": "Give me the information of the capital of France.",
        "require_reasoning": True,
        "sampling_params": {
            "max_new_tokens": 2048,
            "temperature": 0,
            "n": 3,
            "ebnf": (
                "root ::= city | description\n"
                'city ::= "London" | "Paris" | "Berlin" | "Rome"\n'
                'description ::= city " is " status\n'
                'status ::= "the capital of " country\n'
                'country ::= "England" | "France" | "Germany" | "Italy"'
            ),
        },
        "stream": False,
        "return_logprob": False,
    },
)

print(response.json())
```

    [{'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': 'a4d44b93934c4b258bd3000b9ed0839f', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.087595489108935, 'response_sent_to_client_ts': 1774965428.3276343}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '2ca59f396c1d44728da6b850105b2a11', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.08752390509471297, 'response_sent_to_client_ts': 1774965428.32764}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '1d0201d0dadd4b6e989fd3d763d12f98', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.08747453591786325, 'response_sent_to_client_ts': 1774965428.327643}}]


### Regular expression


```python
response = requests.post(
    f"http://localhost:{port}/generate",
    json={
        "text": "Paris is the capital of",
        "require_reasoning": True,
        "sampling_params": {
            "temperature": 0,
            "max_new_tokens": 2048,
            "regex": "(France|England)",
        },
    },
)
print(response.json())
```

    {'text': ' France, and the \n\\( n \\)  \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\(', 'output_ids': [9625, 11, 323, 279, 220, 198, 44292, 308, 1124, 8, 220, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767], 'meta_info': {'id': 'cb16c39470314d918fd8074c6ef935a5', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 6, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 15.472753859008662, 'response_sent_to_client_ts': 1774965443.8061337}}


### Structural Tag


```python
text = tokenizer.apply_chat_template(
    messages, tokenize=False, add_generation_prompt=True, return_dict=False
)
payload = {
    "text": text,
    "require_reasoning": True,
    "sampling_params": {
        "max_new_tokens": 2048,
        "structural_tag": json.dumps(
            {
                "type": "structural_tag",
                "structures": [
                    {
                        "begin": "<function=get_current_weather>",
                        "schema": schema_get_current_weather,
                        "end": "</function>",
                    },
                    {
                        "begin": "<function=get_current_date>",
                        "schema": schema_get_current_date,
                        "end": "</function>",
                    },
                ],
                "triggers": ["<function="],
            }
        ),
    },
}


# Send POST request to the API endpoint
response = requests.post(f"http://localhost:{port}/generate", json=payload)
print_highlight(response.json())
```


<strong style='color: #00008B;'>{'text': 'Okay, so the user asked for the information and population of the capital of France in JSON format. Hmm, first I need to identify what the capital is. I know that Paris is the capital of France, so that\'s a given. \n\nNow, when it comes to population, I should consider that the figure might have changed over the years. I remember that in recent years, Paris has had population estimates around nearly 3.5 million people. I think the latest figure is from 2022, but I should double-check that to be sure. \n\nWait, maybe there are different sources that provide slightly different numbers. One source might cite 8,622,000 while another says 2,822,700. That seems a bit confusing. Oh, right, I think the issue is distinguishing between residents and registered inhabitants. Sometimes, people living in communes are counted differently than the city proper. \n\nSo, to cover all bases, I should present both numbers: one for the city proper and another for the urban area, which typically includes larger commuting zones. That way, the user has a comprehensive view. \n\nI\'ll structure the JSON with a "capital" key containing the city name, and then two keys: "population" with a main figure and "Population notes" explaining the numbers. That way, it\'s clear and informative. \n\nAlso, it\'s good to keep the response friendly and let them know I can provide more details if needed. That adds value and reassures them that I\'m here to help further if there are any more questions. \n\nPutting it all together, I\'ll format it in JSON, make sure the syntax is correct, and present the information clearly. Yeah, that should do it.\n</think>\n\nHere is the information about the capital of France in JSON format:\n\n```json\n{\n  "capital": "Paris",\n  "population": {\n    "city_proper": 8622000,\n    "urban_area": 2822700\n  },\n  "Population_notes": "The city proper population is a city-wide estimate, while the urban area includes larger commute zones."\n}\n```\n\nThis JSON includes the population of Paris, distinguishing between the city proper and the urban area. The population figures are approximate and may vary slightly depending on the source. Let me know if you need more details!', 'output_ids': [32313, 11, 773, 279, 1196, 4588, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 88190, 11, 1156, 358, 1184, 311, 10542, 1128, 279, 6722, 374, 13, 358, 1414, 429, 12095, 374, 279, 6722, 315, 9625, 11, 773, 429, 594, 264, 2661, 13, 4710, 7039, 11, 979, 432, 4041, 311, 7042, 11, 358, 1265, 2908, 429, 279, 7071, 2578, 614, 5497, 916, 279, 1635, 13, 358, 6099, 429, 304, 3213, 1635, 11, 12095, 702, 1030, 7042, 17530, 2163, 7009, 220, 18, 13, 20, 3526, 1251, 13, 358, 1744, 279, 5535, 7071, 374, 504, 220, 17, 15, 17, 17, 11, 714, 358, 1265, 1990, 15934, 429, 311, 387, 2704, 13, 4710, 14190, 11, 7196, 1052, 525, 2155, 8173, 429, 3410, 10078, 2155, 5109, 13, 3776, 2530, 2578, 38296, 220, 23, 11, 21, 17, 17, 11, 15, 15, 15, 1393, 2441, 2727, 220, 17, 11, 23, 17, 17, 11, 22, 15, 15, 13, 2938, 4977, 264, 2699, 30615, 13, 8670, 11, 1290, 11, 358, 1744, 279, 4265, 374, 84955, 1948, 10826, 323, 9681, 39671, 13, 17688, 11, 1251, 5382, 304, 1063, 8531, 525, 29994, 21303, 1091, 279, 3283, 6169, 13, 4710, 4416, 11, 311, 3421, 678, 23092, 11, 358, 1265, 3042, 2176, 5109, 25, 825, 369, 279, 3283, 6169, 323, 2441, 369, 279, 15662, 3082, 11, 892, 11136, 5646, 8131, 93850, 25395, 13, 2938, 1616, 11, 279, 1196, 702, 264, 15817, 1651, 13, 4710, 40, 3278, 5944, 279, 4718, 448, 264, 330, 65063, 1, 1376, 8482, 279, 3283, 829, 11, 323, 1221, 1378, 6894, 25, 330, 44441, 1, 448, 264, 1887, 7071, 323, 330, 53371, 8388, 1, 25021, 279, 5109, 13, 2938, 1616, 11, 432, 594, 2797, 323, 38219, 13, 4710, 13394, 11, 432, 594, 1661, 311, 2506, 279, 2033, 11657, 323, 1077, 1105, 1414, 358, 646, 3410, 803, 3565, 421, 4362, 13, 2938, 11367, 897, 323, 31734, 1413, 1105, 429, 358, 2776, 1588, 311, 1492, 4623, 421, 1052, 525, 894, 803, 4755, 13, 4710, 97904, 432, 678, 3786, 11, 358, 3278, 3561, 432, 304, 4718, 11, 1281, 2704, 279, 19482, 374, 4396, 11, 323, 3042, 279, 1995, 9355, 13, 21607, 11, 429, 1265, 653, 432, 624, 151649, 271, 8420, 374, 279, 1995, 911, 279, 6722, 315, 9625, 304, 4718, 3561, 1447, 73594, 2236, 198, 515, 220, 330, 65063, 788, 330, 59604, 756, 220, 330, 44441, 788, 341, 262, 330, 8926, 2540, 712, 788, 220, 23, 21, 17, 17, 15, 15, 15, 345, 262, 330, 59059, 15030, 788, 220, 17, 23, 17, 17, 22, 15, 15, 198, 220, 1153, 220, 330, 53371, 45138, 788, 330, 785, 3283, 6169, 7042, 374, 264, 3283, 24472, 16045, 11, 1393, 279, 15662, 3082, 5646, 8131, 58163, 25395, 10040, 532, 13874, 19324, 1986, 4718, 5646, 279, 7042, 315, 12095, 11, 84955, 1948, 279, 3283, 6169, 323, 279, 15662, 3082, 13, 576, 7042, 12396, 525, 44868, 323, 1231, 13289, 10078, 11649, 389, 279, 2530, 13, 6771, 752, 1414, 421, 498, 1184, 803, 3565, 0, 151643], 'meta_info': {'id': '27d1853746e44e29a8092765aebb70ab', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 492, 'cached_tokens': 22, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 3.698511947062798, 'response_sent_to_client_ts': 1774965447.514134}}</strong>



```python
terminate_process(server_process)
```

## Offline Engine API


```python
import sglang as sgl

llm = sgl.Engine(
    model_path="deepseek-ai/DeepSeek-R1-Distill-Qwen-7B",
    reasoning_parser="deepseek-r1",
    grammar_backend="xgrammar",
)
```

    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-03-31 13:57:40.250 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 13:57:40] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 13:57:40.250 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 13:57:40] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 13:57:40.250 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 13:57:40] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 13:57:40.250 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 13:57:40] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 13:57:40.251 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 13:57:40] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:00<00:00,  1.08it/s]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.18s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.15s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:44,  2.89s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:44,  2.89s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:21,  1.46s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:21,  1.46s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:49,  1.11it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:49,  1.11it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:33,  1.60it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:33,  1.60it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:03<00:24,  2.15it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:03<00:24,  2.15it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:19,  2.72it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:19,  2.72it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:15,  3.36it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:15,  3.36it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:14,  3.49it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:14,  3.49it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:13,  3.71it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:13,  3.71it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:05<00:12,  3.96it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:05<00:12,  3.96it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:05<00:10,  4.35it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:05<00:10,  4.35it/s]

    Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:05<00:10,  4.35it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:05<00:07,  6.26it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:05<00:07,  6.26it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:05<00:07,  6.26it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:05<00:05,  8.11it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:05<00:05,  8.11it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:05<00:05,  8.11it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:04, 10.20it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:04, 10.20it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:04, 10.20it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:05<00:04, 10.20it/s]

    Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 13.52it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 13.52it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 13.52it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 13.52it/s]Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:05<00:02, 13.52it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:05<00:01, 18.83it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:05<00:01, 18.83it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:05<00:01, 18.83it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:05<00:01, 18.83it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:05<00:01, 18.83it/s]

    Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:05<00:01, 23.56it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:05<00:01, 23.56it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:05<00:01, 23.56it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:05<00:01, 23.56it/s]Compiling num tokens (num_tokens=416):  48%|████▊     | 28/58 [00:05<00:01, 23.56it/s]Compiling num tokens (num_tokens=384):  48%|████▊     | 28/58 [00:06<00:01, 23.56it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:06<00:00, 30.06it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:06<00:00, 30.06it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:06<00:00, 30.06it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:06<00:00, 30.06it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:06<00:00, 30.06it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:06<00:00, 30.06it/s]Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:06<00:00, 30.06it/s]

    Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:06<00:00, 37.26it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:06<00:00, 37.26it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:06<00:00, 37.26it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:06<00:00, 37.26it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:06<00:00, 37.26it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:06<00:00, 37.26it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:06<00:00, 37.26it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:06<00:00, 42.09it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:06<00:00, 42.09it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:06<00:00, 42.09it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:06<00:00, 42.09it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:06<00:00, 42.09it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:06<00:00, 42.09it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:06<00:00, 42.09it/s]

    Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:06<00:00, 46.91it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:06<00:00, 46.91it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:06<00:00, 46.91it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:06<00:00, 46.91it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:06<00:00, 46.91it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:06<00:00, 46.91it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:06<00:00, 46.91it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:06<00:00, 46.91it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:06<00:00,  9.01it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=61.62 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=61.62 GB):   2%|▏         | 1/58 [00:00<00:16,  3.41it/s]Capturing num tokens (num_tokens=7680 avail_mem=61.59 GB):   2%|▏         | 1/58 [00:00<00:16,  3.41it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=61.59 GB):   3%|▎         | 2/58 [00:00<00:19,  2.87it/s]Capturing num tokens (num_tokens=7168 avail_mem=61.59 GB):   3%|▎         | 2/58 [00:00<00:19,  2.87it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=61.59 GB):   5%|▌         | 3/58 [00:00<00:16,  3.26it/s]Capturing num tokens (num_tokens=6656 avail_mem=61.59 GB):   5%|▌         | 3/58 [00:00<00:16,  3.26it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=61.59 GB):   7%|▋         | 4/58 [00:01<00:16,  3.19it/s]Capturing num tokens (num_tokens=6144 avail_mem=61.60 GB):   7%|▋         | 4/58 [00:01<00:16,  3.19it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=61.60 GB):   9%|▊         | 5/58 [00:01<00:17,  3.03it/s]Capturing num tokens (num_tokens=5632 avail_mem=61.60 GB):   9%|▊         | 5/58 [00:01<00:17,  3.03it/s]Capturing num tokens (num_tokens=5632 avail_mem=61.60 GB):  10%|█         | 6/58 [00:01<00:14,  3.53it/s]Capturing num tokens (num_tokens=5120 avail_mem=61.60 GB):  10%|█         | 6/58 [00:01<00:14,  3.53it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=61.60 GB):  12%|█▏        | 7/58 [00:01<00:12,  4.07it/s]Capturing num tokens (num_tokens=4608 avail_mem=61.61 GB):  12%|█▏        | 7/58 [00:01<00:12,  4.07it/s]Capturing num tokens (num_tokens=4608 avail_mem=61.61 GB):  14%|█▍        | 8/58 [00:02<00:10,  4.69it/s]Capturing num tokens (num_tokens=4096 avail_mem=61.61 GB):  14%|█▍        | 8/58 [00:02<00:10,  4.69it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=61.61 GB):  16%|█▌        | 9/58 [00:02<00:09,  5.33it/s]Capturing num tokens (num_tokens=3840 avail_mem=61.62 GB):  16%|█▌        | 9/58 [00:02<00:09,  5.33it/s]Capturing num tokens (num_tokens=3840 avail_mem=61.62 GB):  17%|█▋        | 10/58 [00:02<00:08,  5.94it/s]Capturing num tokens (num_tokens=3584 avail_mem=61.61 GB):  17%|█▋        | 10/58 [00:02<00:08,  5.94it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=61.61 GB):  19%|█▉        | 11/58 [00:02<00:07,  6.60it/s]Capturing num tokens (num_tokens=3328 avail_mem=61.61 GB):  19%|█▉        | 11/58 [00:02<00:07,  6.60it/s]Capturing num tokens (num_tokens=3328 avail_mem=61.61 GB):  21%|██        | 12/58 [00:02<00:06,  7.31it/s]Capturing num tokens (num_tokens=3072 avail_mem=61.61 GB):  21%|██        | 12/58 [00:02<00:06,  7.31it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=61.61 GB):  21%|██        | 12/58 [00:02<00:06,  7.31it/s]Capturing num tokens (num_tokens=2816 avail_mem=61.61 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.60it/s]Capturing num tokens (num_tokens=2560 avail_mem=61.61 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.60it/s]Capturing num tokens (num_tokens=2304 avail_mem=61.61 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.60it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=61.61 GB):  28%|██▊       | 16/58 [00:02<00:04, 10.01it/s]Capturing num tokens (num_tokens=2048 avail_mem=61.61 GB):  28%|██▊       | 16/58 [00:02<00:04, 10.01it/s]Capturing num tokens (num_tokens=1792 avail_mem=61.61 GB):  28%|██▊       | 16/58 [00:03<00:04, 10.01it/s]Capturing num tokens (num_tokens=1792 avail_mem=61.61 GB):  31%|███       | 18/58 [00:03<00:03, 11.44it/s]Capturing num tokens (num_tokens=1536 avail_mem=61.61 GB):  31%|███       | 18/58 [00:03<00:03, 11.44it/s]Capturing num tokens (num_tokens=1280 avail_mem=61.61 GB):  31%|███       | 18/58 [00:03<00:03, 11.44it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=61.61 GB):  34%|███▍      | 20/58 [00:03<00:02, 13.32it/s]Capturing num tokens (num_tokens=1024 avail_mem=61.61 GB):  34%|███▍      | 20/58 [00:03<00:02, 13.32it/s]Capturing num tokens (num_tokens=960 avail_mem=61.61 GB):  34%|███▍      | 20/58 [00:03<00:02, 13.32it/s] Capturing num tokens (num_tokens=896 avail_mem=61.60 GB):  34%|███▍      | 20/58 [00:03<00:02, 13.32it/s]Capturing num tokens (num_tokens=896 avail_mem=61.60 GB):  40%|███▉      | 23/58 [00:03<00:02, 16.57it/s]Capturing num tokens (num_tokens=832 avail_mem=61.60 GB):  40%|███▉      | 23/58 [00:03<00:02, 16.57it/s]Capturing num tokens (num_tokens=768 avail_mem=61.59 GB):  40%|███▉      | 23/58 [00:03<00:02, 16.57it/s]Capturing num tokens (num_tokens=704 avail_mem=61.59 GB):  40%|███▉      | 23/58 [00:03<00:02, 16.57it/s]

    Capturing num tokens (num_tokens=704 avail_mem=61.59 GB):  45%|████▍     | 26/58 [00:03<00:01, 19.68it/s]Capturing num tokens (num_tokens=640 avail_mem=61.59 GB):  45%|████▍     | 26/58 [00:03<00:01, 19.68it/s]Capturing num tokens (num_tokens=576 avail_mem=61.59 GB):  45%|████▍     | 26/58 [00:03<00:01, 19.68it/s]Capturing num tokens (num_tokens=512 avail_mem=61.58 GB):  45%|████▍     | 26/58 [00:03<00:01, 19.68it/s]Capturing num tokens (num_tokens=480 avail_mem=61.58 GB):  45%|████▍     | 26/58 [00:03<00:01, 19.68it/s]Capturing num tokens (num_tokens=480 avail_mem=61.58 GB):  52%|█████▏    | 30/58 [00:03<00:01, 23.52it/s]Capturing num tokens (num_tokens=448 avail_mem=61.58 GB):  52%|█████▏    | 30/58 [00:03<00:01, 23.52it/s]Capturing num tokens (num_tokens=416 avail_mem=61.57 GB):  52%|█████▏    | 30/58 [00:03<00:01, 23.52it/s]Capturing num tokens (num_tokens=384 avail_mem=61.57 GB):  52%|█████▏    | 30/58 [00:03<00:01, 23.52it/s]

    Capturing num tokens (num_tokens=352 avail_mem=61.56 GB):  52%|█████▏    | 30/58 [00:03<00:01, 23.52it/s]Capturing num tokens (num_tokens=352 avail_mem=61.56 GB):  59%|█████▊    | 34/58 [00:03<00:00, 26.20it/s]Capturing num tokens (num_tokens=320 avail_mem=61.56 GB):  59%|█████▊    | 34/58 [00:03<00:00, 26.20it/s]Capturing num tokens (num_tokens=288 avail_mem=61.56 GB):  59%|█████▊    | 34/58 [00:03<00:00, 26.20it/s]Capturing num tokens (num_tokens=256 avail_mem=61.55 GB):  59%|█████▊    | 34/58 [00:03<00:00, 26.20it/s]Capturing num tokens (num_tokens=240 avail_mem=61.55 GB):  59%|█████▊    | 34/58 [00:03<00:00, 26.20it/s]Capturing num tokens (num_tokens=240 avail_mem=61.55 GB):  66%|██████▌   | 38/58 [00:03<00:00, 29.53it/s]Capturing num tokens (num_tokens=224 avail_mem=61.54 GB):  66%|██████▌   | 38/58 [00:03<00:00, 29.53it/s]Capturing num tokens (num_tokens=208 avail_mem=61.54 GB):  66%|██████▌   | 38/58 [00:03<00:00, 29.53it/s]Capturing num tokens (num_tokens=192 avail_mem=61.54 GB):  66%|██████▌   | 38/58 [00:03<00:00, 29.53it/s]

    Capturing num tokens (num_tokens=176 avail_mem=61.53 GB):  66%|██████▌   | 38/58 [00:03<00:00, 29.53it/s]Capturing num tokens (num_tokens=160 avail_mem=61.53 GB):  66%|██████▌   | 38/58 [00:03<00:00, 29.53it/s]Capturing num tokens (num_tokens=160 avail_mem=61.53 GB):  74%|███████▍  | 43/58 [00:03<00:00, 33.22it/s]Capturing num tokens (num_tokens=144 avail_mem=61.52 GB):  74%|███████▍  | 43/58 [00:03<00:00, 33.22it/s]Capturing num tokens (num_tokens=128 avail_mem=61.53 GB):  74%|███████▍  | 43/58 [00:03<00:00, 33.22it/s]Capturing num tokens (num_tokens=112 avail_mem=61.53 GB):  74%|███████▍  | 43/58 [00:03<00:00, 33.22it/s]Capturing num tokens (num_tokens=96 avail_mem=61.53 GB):  74%|███████▍  | 43/58 [00:03<00:00, 33.22it/s] Capturing num tokens (num_tokens=80 avail_mem=61.52 GB):  74%|███████▍  | 43/58 [00:03<00:00, 33.22it/s]Capturing num tokens (num_tokens=80 avail_mem=61.52 GB):  83%|████████▎ | 48/58 [00:03<00:00, 35.76it/s]Capturing num tokens (num_tokens=64 avail_mem=61.52 GB):  83%|████████▎ | 48/58 [00:03<00:00, 35.76it/s]Capturing num tokens (num_tokens=48 avail_mem=61.52 GB):  83%|████████▎ | 48/58 [00:04<00:00, 35.76it/s]

    Capturing num tokens (num_tokens=32 avail_mem=61.51 GB):  83%|████████▎ | 48/58 [00:04<00:00, 35.76it/s]Capturing num tokens (num_tokens=28 avail_mem=61.51 GB):  83%|████████▎ | 48/58 [00:04<00:00, 35.76it/s]Capturing num tokens (num_tokens=24 avail_mem=61.51 GB):  83%|████████▎ | 48/58 [00:04<00:00, 35.76it/s]Capturing num tokens (num_tokens=24 avail_mem=61.51 GB):  91%|█████████▏| 53/58 [00:04<00:00, 37.37it/s]Capturing num tokens (num_tokens=20 avail_mem=61.50 GB):  91%|█████████▏| 53/58 [00:04<00:00, 37.37it/s]Capturing num tokens (num_tokens=16 avail_mem=61.50 GB):  91%|█████████▏| 53/58 [00:04<00:00, 37.37it/s]Capturing num tokens (num_tokens=12 avail_mem=61.50 GB):  91%|█████████▏| 53/58 [00:04<00:00, 37.37it/s]Capturing num tokens (num_tokens=8 avail_mem=61.49 GB):  91%|█████████▏| 53/58 [00:04<00:00, 37.37it/s] Capturing num tokens (num_tokens=4 avail_mem=61.49 GB):  91%|█████████▏| 53/58 [00:04<00:00, 37.37it/s]Capturing num tokens (num_tokens=4 avail_mem=61.49 GB): 100%|██████████| 58/58 [00:04<00:00, 39.13it/s]Capturing num tokens (num_tokens=4 avail_mem=61.49 GB): 100%|██████████| 58/58 [00:04<00:00, 13.74it/s]


### JSON

**Using Pydantic**


```python
import json
from pydantic import BaseModel, Field

prompts = [
    "Give me the information of the capital of China in the JSON format.",
    "Give me the information of the capital of France in the JSON format.",
    "Give me the information of the capital of Ireland in the JSON format.",
]


# Define the schema using Pydantic
class CapitalInfo(BaseModel):
    name: str = Field(..., pattern=r"^\w+$", description="Name of the capital city")
    population: int = Field(..., description="Population of the capital city")


sampling_params = {
    "temperature": 0,
    "top_p": 0.95,
    "max_new_tokens": 2048,
    "json_schema": json.dumps(CapitalInfo.model_json_schema()),
}

outputs = llm.generate(prompts, sampling_params)
for prompt, output in zip(prompts, outputs):
    print("===============================")
    print(f"Prompt: {prompt}\nGenerated text: {output['text']}")
```

    ===============================
    Prompt: Give me the information of the capital of China in the JSON format.
    Generated text: {
      "name": "Beijing",
      "population": 316000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
    ===============================
    Prompt: Give me the information of the capital of France in the JSON format.
    Generated text: {
      "name": "Paris",
      "population": 2154000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
    ===============================
    Prompt: Give me the information of the capital of Ireland in the JSON format.
    Generated text: {
      "name": "Ireland",
      "population": 500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000


**JSON Schema Directly**


```python
prompts = [
    "Give me the information of the capital of China in the JSON format.",
    "Give me the information of the capital of France in the JSON format.",
    "Give me the information of the capital of Ireland in the JSON format.",
]

json_schema = json.dumps(
    {
        "type": "object",
        "properties": {
            "name": {"type": "string", "pattern": "^[\\w]+$"},
            "population": {"type": "integer"},
        },
        "required": ["name", "population"],
    }
)

sampling_params = {"temperature": 0, "max_new_tokens": 2048, "json_schema": json_schema}

outputs = llm.generate(prompts, sampling_params)
for prompt, output in zip(prompts, outputs):
    print("===============================")
    print(f"Prompt: {prompt}\nGenerated text: {output['text']}")
```

    ===============================
    Prompt: Give me the information of the capital of China in the JSON format.
    Generated text: {
      "name": "Beijing",
      "population": 316000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
    ===============================
    Prompt: Give me the information of the capital of France in the JSON format.
    Generated text: {
      "name": "Paris",
      "population": 2154000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
    ===============================
    Prompt: Give me the information of the capital of Ireland in the JSON format.
    Generated text: {
      "name": "Ireland",
      "population": 500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000


### EBNF



```python
prompts = [
    "Give me the information of the capital of France.",
    "Give me the information of the capital of Germany.",
    "Give me the information of the capital of Italy.",
]

sampling_params = {
    "temperature": 0.8,
    "top_p": 0.95,
    "ebnf": (
        "root ::= city | description\n"
        'city ::= "London" | "Paris" | "Berlin" | "Rome"\n'
        'description ::= city " is " status\n'
        'status ::= "the capital of " country\n'
        'country ::= "England" | "France" | "Germany" | "Italy"'
    ),
}

outputs = llm.generate(prompts, sampling_params)
for prompt, output in zip(prompts, outputs):
    print("===============================")
    print(f"Prompt: {prompt}\nGenerated text: {output['text']}")
```

    ===============================
    Prompt: Give me the information of the capital of France.
    Generated text: Berlin is the capital of France
    ===============================
    Prompt: Give me the information of the capital of Germany.
    Generated text: London is the capital of England
    ===============================
    Prompt: Give me the information of the capital of Italy.
    Generated text: Paris is the capital of France


### Regular expression


```python
prompts = [
    "Please provide information about London as a major global city:",
    "Please provide information about Paris as a major global city:",
]

sampling_params = {"temperature": 0.8, "top_p": 0.95, "regex": "(France|England)"}

outputs = llm.generate(prompts, sampling_params)
for prompt, output in zip(prompts, outputs):
    print("===============================")
    print(f"Prompt: {prompt}\nGenerated text: {output['text']}")
```

    ===============================
    Prompt: Please provide information about London as a major global city:
    Generated text: France
    ===============================
    Prompt: Please provide information about Paris as a major global city:
    Generated text: France



```python
text = tokenizer.apply_chat_template(
    messages, tokenize=False, add_generation_prompt=True, return_dict=False
)
prompts = [text]


sampling_params = {
    "temperature": 0.8,
    "top_p": 0.95,
    "max_new_tokens": 2048,
    "structural_tag": json.dumps(
        {
            "type": "structural_tag",
            "structures": [
                {
                    "begin": "<function=get_current_weather>",
                    "schema": schema_get_current_weather,
                    "end": "</function>",
                },
                {
                    "begin": "<function=get_current_date>",
                    "schema": schema_get_current_date,
                    "end": "</function>",
                },
            ],
            "triggers": ["<function="],
        }
    ),
}


# Send POST request to the API endpoint
outputs = llm.generate(prompts, sampling_params)
for prompt, output in zip(prompts, outputs):
    print("===============================")
    print(f"Prompt: {prompt}\nGenerated text: {output['text']}")
```

    ===============================
    Prompt: <｜begin▁of▁sentence｜><｜Assistant｜>Give me the information and population of the capital of France in the JSON format.<｜end▁of▁sentence｜><｜Assistant｜><think>
    
    Generated text: Alright, so I just got this query from a user asking for the information and population of the capital of France in JSON format. Hmm, okay, first, I need to make sure I understand exactly what they're asking for. The capital of France is definitely Paris, right? So I should start by confirming that.
    
    Now, the user wants this information in JSON format. JSON stands for JavaScript Object Notation, and it's a way to structure data that's easy for both humans and machines to read. So I'll need to create a JSON object. The structure should probably include key-value pairs. They mentioned "information and population," so I should think about what specific data points are relevant.
    
    Population is straightforward, but I should look up the latest estimate. I remember that as of recent years, Paris has been growing, but it's also a busy city with a lot of factors affecting population growth. Let me check the current population figure. I think it's around 2.2 million, but I should verify that to be accurate.
    
    Next, information about the capital. What exactly is needed? Probably the administrative region, maybe the location coordinates, and some notable landmarks. Paris is located in the Île-de-France region, right? So I'll include that. For the location, I can provide the latitude and longitude coordinates to pinpoint it on a map. Coordinates like 48.8566° N, 2.3522° E should do the trick.
    
    Then, including some key landmarks would add more information. The Eiffel Tower, the Louvre Museum, Notre-Dame, and the Arc de Triomphe are must-mentionables. Including these will give the user a better idea of what Paris is like.
    
    I should also consider the date format for population data. Since populations can change yearly, I'll note that it's an estimate for the year 2023. That way, the user is aware it's approximate and might change in the future.
    
    Putting it all together, the JSON structure would have an "Info" key with a subkey "capital" containing the region name. Then, an "Population" key with a subkey "current" and "year" to clarify the estimate. Under "Population," I'll include the number and the year it's for. Under "Information," the "location" key will have "coordinates" as a list of the longitude and latitude.
    
    Let me double-check all the data to ensure accuracy. The administrative region is indeed Île-de-France, the coordinates are correct, and the landmarks are accurate. The population figure for 2023 is roughly 2.2 million, but I should confirm if it's more up-to-date. Maybe in 2022 it was slightly different, but 2023 seems reasonable.
    
    I should also make sure the JSON syntax is correct. Commas should be placed properly between the keys, and the string values should be enclosed in quotes. No trailing commas inside the arrays or objects, as that can cause errors.
    
    Finally, I'll present the JSON in a clear and organized manner. Maybe break it into two main sections for "Info" and "Population" to make it easy to read. Each key-value pair will be on a new line for clarity.
    
    So, putting it all together, the response should be concise, accurate, and formatted correctly in JSON. This way, the user gets exactly what they asked for without any unnecessary information. I think that covers everything the user needs. Hopefully, this helps them out!
    </think>
    
    Here is the information and population of the capital of France (Paris) in JSON format:
    
    ```json
    {
      "Info": {
        "capital": "Île-de-France",
        "location": {
          "coordinates": {
            "longitude": 2.3522,
            "latitude": 48.8566
          }
        }
      },
      "Population": {
        "current": 2220000,
        "year": 2023
      }
    }
    ```
    
    This JSON includes the population of Paris (estimated as of 2023) and some basic information about the city, such as its administrative region and geographic location.



```python
llm.shutdown()
```
