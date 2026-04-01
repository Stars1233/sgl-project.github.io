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
    2026-04-01 06:18:20.536 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 06:18:20] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 06:18:20.536 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 06:18:20] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 06:18:20.536 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 06:18:20] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 06:18:20.536 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 06:18:20] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 06:18:20.536 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 06:18:20] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:00<00:00,  1.23it/s]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.12s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.07s/it]


    2026-04-01 06:18:25,469 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 06:18:25] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:54,  3.05s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:54,  3.05s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:35,  1.70s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:35,  1.70s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<00:59,  1.08s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<00:59,  1.08s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:40,  1.32it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:40,  1.32it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:29,  1.78it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:29,  1.78it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:22,  2.27it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:22,  2.27it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:18,  2.82it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:18,  2.82it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:05<00:14,  3.41it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:05<00:14,  3.41it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:05<00:11,  4.27it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:05<00:11,  4.27it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:05<00:11,  4.27it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:05<00:07,  5.98it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:05<00:07,  5.98it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:05<00:07,  5.98it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:05<00:06,  7.06it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:05<00:06,  7.06it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:05<00:06,  6.85it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:05<00:06,  6.85it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:05<00:06,  6.92it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:05<00:06,  6.92it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:06<00:05,  7.51it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:06<00:05,  7.51it/s]Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:06<00:05,  7.51it/s]

    Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:06<00:04,  8.77it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:06<00:04,  8.77it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:06<00:04,  8.77it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:06<00:03,  9.56it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:06<00:03,  9.56it/s]

    Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:06<00:03,  9.56it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:06<00:03, 10.92it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:06<00:03, 10.92it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:06<00:03, 10.92it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:06<00:02, 12.64it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:06<00:02, 12.64it/s]

    Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:06<00:02, 12.64it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:06<00:02, 12.64it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:06<00:02, 14.90it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:06<00:02, 14.90it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:06<00:02, 14.90it/s]

    Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:06<00:01, 16.05it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:06<00:01, 16.05it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:06<00:01, 16.05it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:06<00:01, 16.05it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:07<00:01, 18.71it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:07<00:01, 18.71it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:07<00:01, 18.71it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:07<00:01, 18.71it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:07<00:01, 18.71it/s]

    Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:07<00:00, 23.03it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:07<00:00, 23.03it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:07<00:00, 23.03it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:07<00:00, 23.03it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:07<00:00, 21.50it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:07<00:00, 21.50it/s]

    Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:07<00:00, 21.50it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:07<00:00, 21.50it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:07<00:00, 19.88it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:07<00:00, 19.88it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:07<00:00, 19.88it/s]

    Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:07<00:00, 19.88it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:07<00:00, 19.32it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:07<00:00, 19.32it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:07<00:00, 19.32it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:07<00:00, 19.32it/s]

    Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:07<00:00, 19.20it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:07<00:00, 19.20it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:07<00:00, 19.20it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:07<00:00, 18.97it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:07<00:00, 18.97it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:07<00:00, 18.97it/s]

    Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:08<00:00, 18.97it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:08<00:00, 19.04it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:08<00:00, 19.04it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:08<00:00, 19.04it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:08<00:00, 19.04it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:08<00:00, 20.38it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:08<00:00, 20.38it/s] 

    Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:08<00:00, 20.38it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:08<00:00,  7.03it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=85.19 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=85.19 GB):   2%|▏         | 1/58 [00:00<00:52,  1.09it/s]Capturing num tokens (num_tokens=7680 avail_mem=85.17 GB):   2%|▏         | 1/58 [00:00<00:52,  1.09it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=85.17 GB):   3%|▎         | 2/58 [00:01<00:46,  1.20it/s]Capturing num tokens (num_tokens=7168 avail_mem=84.68 GB):   3%|▎         | 2/58 [00:01<00:46,  1.20it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=84.68 GB):   5%|▌         | 3/58 [00:02<00:41,  1.32it/s]Capturing num tokens (num_tokens=6656 avail_mem=85.18 GB):   5%|▌         | 3/58 [00:02<00:41,  1.32it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=85.18 GB):   7%|▋         | 4/58 [00:02<00:38,  1.41it/s]Capturing num tokens (num_tokens=6144 avail_mem=85.18 GB):   7%|▋         | 4/58 [00:02<00:38,  1.41it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=85.18 GB):   9%|▊         | 5/58 [00:03<00:34,  1.52it/s]Capturing num tokens (num_tokens=5632 avail_mem=84.85 GB):   9%|▊         | 5/58 [00:03<00:34,  1.52it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=84.85 GB):  10%|█         | 6/58 [00:04<00:31,  1.67it/s]Capturing num tokens (num_tokens=5120 avail_mem=85.18 GB):  10%|█         | 6/58 [00:04<00:31,  1.67it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=85.18 GB):  12%|█▏        | 7/58 [00:04<00:27,  1.83it/s]Capturing num tokens (num_tokens=4608 avail_mem=85.19 GB):  12%|█▏        | 7/58 [00:04<00:27,  1.83it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=85.19 GB):  14%|█▍        | 8/58 [00:04<00:24,  2.03it/s]Capturing num tokens (num_tokens=4096 avail_mem=85.02 GB):  14%|█▍        | 8/58 [00:04<00:24,  2.03it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=85.02 GB):  16%|█▌        | 9/58 [00:05<00:21,  2.28it/s]Capturing num tokens (num_tokens=3840 avail_mem=85.07 GB):  16%|█▌        | 9/58 [00:05<00:21,  2.28it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=85.07 GB):  17%|█▋        | 10/58 [00:05<00:18,  2.59it/s]Capturing num tokens (num_tokens=3584 avail_mem=85.18 GB):  17%|█▋        | 10/58 [00:05<00:18,  2.59it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=85.18 GB):  19%|█▉        | 11/58 [00:05<00:16,  2.89it/s]Capturing num tokens (num_tokens=3328 avail_mem=85.18 GB):  19%|█▉        | 11/58 [00:05<00:16,  2.89it/s]Capturing num tokens (num_tokens=3328 avail_mem=85.18 GB):  21%|██        | 12/58 [00:05<00:13,  3.32it/s]Capturing num tokens (num_tokens=3072 avail_mem=85.17 GB):  21%|██        | 12/58 [00:05<00:13,  3.32it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=85.17 GB):  22%|██▏       | 13/58 [00:06<00:12,  3.61it/s]Capturing num tokens (num_tokens=2816 avail_mem=85.16 GB):  22%|██▏       | 13/58 [00:06<00:12,  3.61it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=85.16 GB):  24%|██▍       | 14/58 [00:06<00:11,  3.67it/s]Capturing num tokens (num_tokens=2560 avail_mem=85.10 GB):  24%|██▍       | 14/58 [00:06<00:11,  3.67it/s]Capturing num tokens (num_tokens=2560 avail_mem=85.10 GB):  26%|██▌       | 15/58 [00:06<00:09,  4.40it/s]Capturing num tokens (num_tokens=2304 avail_mem=85.15 GB):  26%|██▌       | 15/58 [00:06<00:09,  4.40it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=85.15 GB):  28%|██▊       | 16/58 [00:06<00:08,  5.24it/s]Capturing num tokens (num_tokens=2048 avail_mem=85.14 GB):  28%|██▊       | 16/58 [00:06<00:08,  5.24it/s]Capturing num tokens (num_tokens=1792 avail_mem=85.13 GB):  28%|██▊       | 16/58 [00:06<00:08,  5.24it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=85.13 GB):  31%|███       | 18/58 [00:06<00:06,  6.40it/s]Capturing num tokens (num_tokens=1536 avail_mem=84.09 GB):  31%|███       | 18/58 [00:06<00:06,  6.40it/s]Capturing num tokens (num_tokens=1536 avail_mem=84.09 GB):  33%|███▎      | 19/58 [00:07<00:06,  6.17it/s]Capturing num tokens (num_tokens=1280 avail_mem=84.09 GB):  33%|███▎      | 19/58 [00:07<00:06,  6.17it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=84.09 GB):  34%|███▍      | 20/58 [00:07<00:05,  6.42it/s]Capturing num tokens (num_tokens=1024 avail_mem=85.74 GB):  34%|███▍      | 20/58 [00:07<00:05,  6.42it/s]Capturing num tokens (num_tokens=1024 avail_mem=85.74 GB):  36%|███▌      | 21/58 [00:07<00:05,  6.87it/s]Capturing num tokens (num_tokens=960 avail_mem=85.07 GB):  36%|███▌      | 21/58 [00:07<00:05,  6.87it/s] 

    Capturing num tokens (num_tokens=960 avail_mem=85.07 GB):  38%|███▊      | 22/58 [00:07<00:05,  7.08it/s]Capturing num tokens (num_tokens=896 avail_mem=84.25 GB):  38%|███▊      | 22/58 [00:07<00:05,  7.08it/s]Capturing num tokens (num_tokens=896 avail_mem=84.25 GB):  40%|███▉      | 23/58 [00:07<00:04,  7.07it/s]Capturing num tokens (num_tokens=832 avail_mem=84.24 GB):  40%|███▉      | 23/58 [00:07<00:04,  7.07it/s]

    Capturing num tokens (num_tokens=832 avail_mem=84.24 GB):  41%|████▏     | 24/58 [00:07<00:04,  7.08it/s]Capturing num tokens (num_tokens=768 avail_mem=84.23 GB):  41%|████▏     | 24/58 [00:07<00:04,  7.08it/s]Capturing num tokens (num_tokens=704 avail_mem=85.06 GB):  41%|████▏     | 24/58 [00:07<00:04,  7.08it/s]Capturing num tokens (num_tokens=704 avail_mem=85.06 GB):  45%|████▍     | 26/58 [00:07<00:03,  8.25it/s]Capturing num tokens (num_tokens=640 avail_mem=84.29 GB):  45%|████▍     | 26/58 [00:07<00:03,  8.25it/s]

    Capturing num tokens (num_tokens=640 avail_mem=84.29 GB):  47%|████▋     | 27/58 [00:08<00:03,  8.11it/s]Capturing num tokens (num_tokens=576 avail_mem=84.28 GB):  47%|████▋     | 27/58 [00:08<00:03,  8.11it/s]Capturing num tokens (num_tokens=576 avail_mem=84.28 GB):  48%|████▊     | 28/58 [00:08<00:03,  7.99it/s]Capturing num tokens (num_tokens=512 avail_mem=84.27 GB):  48%|████▊     | 28/58 [00:08<00:03,  7.99it/s]

    Capturing num tokens (num_tokens=512 avail_mem=84.27 GB):  50%|█████     | 29/58 [00:08<00:03,  8.36it/s]Capturing num tokens (num_tokens=480 avail_mem=85.03 GB):  50%|█████     | 29/58 [00:08<00:03,  8.36it/s]Capturing num tokens (num_tokens=480 avail_mem=85.03 GB):  52%|█████▏    | 30/58 [00:08<00:03,  8.41it/s]Capturing num tokens (num_tokens=448 avail_mem=84.32 GB):  52%|█████▏    | 30/58 [00:08<00:03,  8.41it/s]

    Capturing num tokens (num_tokens=448 avail_mem=84.32 GB):  53%|█████▎    | 31/58 [00:08<00:03,  8.31it/s]Capturing num tokens (num_tokens=416 avail_mem=84.31 GB):  53%|█████▎    | 31/58 [00:08<00:03,  8.31it/s]Capturing num tokens (num_tokens=384 avail_mem=85.02 GB):  53%|█████▎    | 31/58 [00:08<00:03,  8.31it/s]

    Capturing num tokens (num_tokens=384 avail_mem=85.02 GB):  57%|█████▋    | 33/58 [00:08<00:03,  8.32it/s]Capturing num tokens (num_tokens=352 avail_mem=84.99 GB):  57%|█████▋    | 33/58 [00:08<00:03,  8.32it/s]Capturing num tokens (num_tokens=352 avail_mem=84.99 GB):  59%|█████▊    | 34/58 [00:08<00:03,  7.31it/s]Capturing num tokens (num_tokens=320 avail_mem=84.34 GB):  59%|█████▊    | 34/58 [00:08<00:03,  7.31it/s]

    Capturing num tokens (num_tokens=288 avail_mem=85.00 GB):  59%|█████▊    | 34/58 [00:09<00:03,  7.31it/s]Capturing num tokens (num_tokens=288 avail_mem=85.00 GB):  62%|██████▏   | 36/58 [00:09<00:02,  8.51it/s]Capturing num tokens (num_tokens=256 avail_mem=84.39 GB):  62%|██████▏   | 36/58 [00:09<00:02,  8.51it/s]

    Capturing num tokens (num_tokens=256 avail_mem=84.39 GB):  64%|██████▍   | 37/58 [00:09<00:02,  8.57it/s]Capturing num tokens (num_tokens=240 avail_mem=84.38 GB):  64%|██████▍   | 37/58 [00:09<00:02,  8.57it/s]Capturing num tokens (num_tokens=224 avail_mem=84.98 GB):  64%|██████▍   | 37/58 [00:09<00:02,  8.57it/s]Capturing num tokens (num_tokens=224 avail_mem=84.98 GB):  67%|██████▋   | 39/58 [00:09<00:01,  9.73it/s]Capturing num tokens (num_tokens=208 avail_mem=84.42 GB):  67%|██████▋   | 39/58 [00:09<00:01,  9.73it/s]

    Capturing num tokens (num_tokens=208 avail_mem=84.42 GB):  69%|██████▉   | 40/58 [00:09<00:02,  7.91it/s]Capturing num tokens (num_tokens=192 avail_mem=84.97 GB):  69%|██████▉   | 40/58 [00:09<00:02,  7.91it/s]Capturing num tokens (num_tokens=176 avail_mem=84.47 GB):  69%|██████▉   | 40/58 [00:09<00:02,  7.91it/s]Capturing num tokens (num_tokens=176 avail_mem=84.47 GB):  72%|███████▏  | 42/58 [00:09<00:01,  8.92it/s]Capturing num tokens (num_tokens=160 avail_mem=84.47 GB):  72%|███████▏  | 42/58 [00:09<00:01,  8.92it/s]

    Capturing num tokens (num_tokens=144 avail_mem=84.96 GB):  72%|███████▏  | 42/58 [00:09<00:01,  8.92it/s]Capturing num tokens (num_tokens=144 avail_mem=84.96 GB):  76%|███████▌  | 44/58 [00:09<00:01,  9.69it/s]Capturing num tokens (num_tokens=128 avail_mem=84.53 GB):  76%|███████▌  | 44/58 [00:09<00:01,  9.69it/s]Capturing num tokens (num_tokens=112 avail_mem=84.97 GB):  76%|███████▌  | 44/58 [00:10<00:01,  9.69it/s]

    Capturing num tokens (num_tokens=112 avail_mem=84.97 GB):  79%|███████▉  | 46/58 [00:10<00:01, 10.83it/s]Capturing num tokens (num_tokens=96 avail_mem=84.55 GB):  79%|███████▉  | 46/58 [00:10<00:01, 10.83it/s] Capturing num tokens (num_tokens=80 avail_mem=84.99 GB):  79%|███████▉  | 46/58 [00:10<00:01, 10.83it/s]Capturing num tokens (num_tokens=80 avail_mem=84.99 GB):  83%|████████▎ | 48/58 [00:10<00:00, 11.69it/s]Capturing num tokens (num_tokens=64 avail_mem=84.96 GB):  83%|████████▎ | 48/58 [00:10<00:00, 11.69it/s]

    Capturing num tokens (num_tokens=48 avail_mem=84.57 GB):  83%|████████▎ | 48/58 [00:10<00:00, 11.69it/s]Capturing num tokens (num_tokens=48 avail_mem=84.57 GB):  86%|████████▌ | 50/58 [00:10<00:00, 12.22it/s]Capturing num tokens (num_tokens=32 avail_mem=84.95 GB):  86%|████████▌ | 50/58 [00:10<00:00, 12.22it/s]Capturing num tokens (num_tokens=28 avail_mem=84.59 GB):  86%|████████▌ | 50/58 [00:10<00:00, 12.22it/s]

    Capturing num tokens (num_tokens=28 avail_mem=84.59 GB):  90%|████████▉ | 52/58 [00:10<00:00, 12.76it/s]Capturing num tokens (num_tokens=24 avail_mem=84.95 GB):  90%|████████▉ | 52/58 [00:10<00:00, 12.76it/s]Capturing num tokens (num_tokens=20 avail_mem=84.61 GB):  90%|████████▉ | 52/58 [00:10<00:00, 12.76it/s]Capturing num tokens (num_tokens=20 avail_mem=84.61 GB):  93%|█████████▎| 54/58 [00:10<00:00, 13.26it/s]Capturing num tokens (num_tokens=16 avail_mem=84.94 GB):  93%|█████████▎| 54/58 [00:10<00:00, 13.26it/s]Capturing num tokens (num_tokens=12 avail_mem=84.93 GB):  93%|█████████▎| 54/58 [00:10<00:00, 13.26it/s]

    Capturing num tokens (num_tokens=12 avail_mem=84.93 GB):  97%|█████████▋| 56/58 [00:10<00:00, 13.81it/s]Capturing num tokens (num_tokens=8 avail_mem=84.98 GB):  97%|█████████▋| 56/58 [00:10<00:00, 13.81it/s] Capturing num tokens (num_tokens=4 avail_mem=84.93 GB):  97%|█████████▋| 56/58 [00:10<00:00, 13.81it/s]Capturing num tokens (num_tokens=4 avail_mem=84.93 GB): 100%|██████████| 58/58 [00:10<00:00, 14.56it/s]Capturing num tokens (num_tokens=4 avail_mem=84.93 GB): 100%|██████████| 58/58 [00:10<00:00,  5.32it/s]


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


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France and its population. I know that the capital of France is Paris, but I'm not exactly sure about the current population numbers. I remember that Paris is a very big city, but I think it's not the largest in the world. Maybe around 20 million? I'm not certain, though. I should probably check that.<br><br>Wait, I think I heard somewhere that Paris has a population over 21 million. Maybe 21.6 million? I'm not sure if that's accurate. I should look up the latest data to confirm. Let me think, where can I find reliable information? Maybe the official government website or a reputable news source. <br><br>I recall that France's population is around 40 million, so Paris being a major city would have a significant portion of that. If the total population is about 40 million, and Paris is the largest city, it's plausible that it's around 21.6 million. I think I've seen that number before, but I'm not 100% sure. <br><br>Also, I should consider if the population figure includes just the city proper or the metropolitan area. Sometimes, population counts can include surrounding suburbs and satellite towns. But I think in this case, the user is asking for the population of the capital, which is Paris, so it's probably just the city limits. <br><br>I should also think about how populations can change over time. Demographics can fluctuate due to births, deaths, and migration. So the number might not be exact and could vary slightly from year to year. <br><br>To sum up, I'm pretty confident that Paris is the capital of France and that its population is approximately 21.6 million. But to be thorough, I should verify this information to ensure accuracy.<br><br><br>content: {<br><br>"name": "Paris",<br>"population": 21620000<br>}</strong>


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


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France and its population. I know that the capital of France is Paris, but I'm not exactly sure about the current population numbers. I remember that Paris is a very big city, but I think it's not the largest in the world. Maybe around 20 million? I'm not certain, though. I should probably check that.<br><br>Wait, I think I heard somewhere that Paris has a population over 21 million. Maybe 21.6 million? I'm not sure if that's accurate. I should look up the latest data to confirm. Let me think, where can I find reliable information? Maybe the official government website or a reputable news source. <br><br>I recall that France's population is around 40 million, so Paris being a major city would have a significant portion of that. If the total population is about 40 million, and Paris is the largest city, it's plausible that it's around 21.6 million. I think I've seen that number before, but I'm not 100% sure. <br><br>Also, I should consider if the population figure includes just the city proper or the metropolitan area. Sometimes, population counts can include surrounding suburbs and satellite towns. But I think in this case, the user is asking for the population of the capital, which is Paris, so it's probably just the city limits. <br><br>I should also think about how populations can change over time. Demographics can fluctuate due to births, deaths, and migration. So the number might not be exact and could vary slightly from year to year. <br><br>To sum up, I'm pretty confident that Paris is the capital of France and that its population is approximately 21.6 million. But to be thorough, I should verify this information to ensure accuracy.<br><br><br>content: {<br><br>"name": "Paris",<br>"population": 21620000<br>}</strong>


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


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France and its population. I know that the capital of France is Paris, but I'm not exactly sure about the current population. I think it's a big city, maybe around 3 million? But I'm not certain. I should probably check some reliable sources to confirm this. Maybe I can look up recent population data or news articles that mention Paris's population. I remember hearing that Paris is one of the most populous cities in the world, but I'm not sure if it's over 3 million or not. I should also consider factors like urbanization and migration that might affect the population numbers. Maybe the population has grown a bit since the last census. I think the most recent data might be from 2020 or 2021. I should make sure the number I provide is accurate and up-to-date. Also, I should present this information in a clear and concise way, maybe in JSON format as the user requested. I should double-check the population figure to ensure it's correct before finalizing the answer.<br><br><br>content: Rome is the capital of France</strong>


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


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France. Hmm, I remember learning a bit about France in school, but I'm not 100% sure. Let me think. I know that Paris is a major city in France, and it's often referred to as the "City of Light" because of the famous Eiffel Tower. But is it the capital? I think so, but I'm not entirely certain. <br><br>Wait, I also recall that there's another city called Lyon. Isn't that the capital? No, I think I'm mixing things up. Lyon is definitely a significant city in France, known for its gastronomy and being the second-largest city. But I'm pretty sure Paris is the capital. <br><br>Let me try to remember any other capitals I know. For example, Germany's capital is Berlin, Italy's is Rome, Spain's is Madrid, and so on. So, following that pattern, France's capital should be Paris. I think I heard it a lot in history classes, especially when talking about the French Revolution and Napoleon. Those events happened in Paris, which probably helped it become the capital.<br><br>I also remember that the Eiffel Tower is in Paris, and it's a symbol of the country. The tower was built in the 19th century, and it's a tourist attraction. So, if Paris has such a famous landmark, it's likely the capital. <br><br>Another way to think about it is the political aspect. The President of France is based in Paris, right? So that makes sense. The government quarters, like the Palace of Versailles, are in Paris. That would mean Paris is where the country's government is located, making it the capital.<br><br>I guess I'm pretty confident now. I don't think I've heard of any other city being the capital of France. Lyon is more of a regional capital or something. Maybe it's the regional capital for certain areas, but not the national one. <br><br>So, putting it all together, Paris is the capital of France because it's the most significant political, cultural, and symbolic center of the country. It's where major landmarks like the Eiffel Tower and government buildings are located, and it's the birthplace of many important historical events and figures.<br><br><br>content: Paris</strong>


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


<strong style='color: #00008B;'>reasoing_content: Okay, so the user is in New York and wants the current date and time along with the weather. Let me figure out how to approach this.<br><br>First, I need to call the get_current_date function. The user mentioned the timezone is New York, so I'll set the timezone parameter to 'America/New_York'. That should give me the correct date and time.<br><br>Next, I should also get the weather using get_current_weather. The city is New York, and the unit should be Fahrenheit since that's a common preference, but the function allows for both. I'll include both city and unit in the parameters.<br><br>I need to make sure each function call is separate and follows the required format. So, I'll first call get_current_date with the timezone and then get_current_weather with the city and unit. I'll structure each function call on its own line as per the instructions.<br><br>I should also remember to add the sources from the functions used, so I'll mention the current date from get_current_date and the weather from get_current_weather.<br><br>Putting it all together, I'll format each function call correctly with the parameters in JSON and add the necessary sources at the end.<br><br><br>content: <function=get_current_date>{"timezone": "America/New_York"}</function><br><function=get_current_weather>{"city": "New York", "state": "NY", "unit": "fahrenheit"}</function><br><br>Sources:<br>- Current date and time from get_current_date<br>- Weather data from get_current_weather</strong>


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

    {'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. I know that Paris is the capital, so that\'s straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I\'m not sure of the exact number. I think it\'s around 2 million, but I should verify that.\n\nWait, maybe I should check the latest statistics. I recall that in recent years, Paris has seen a slight increase. Let me think... I believe the population is approximately 2,150,000 as of 2023. That seems about right, but I\'m not 100% certain. I should make sure to present this information accurately.\n\nNext, I need to structure this into a JSON format. JSON requires key-value pairs, so I\'ll create an object with keys like "city", "population", and maybe "country" for context. The city is Paris, the population is 2,150,000, and the country is France.\n\nI should also consider if the user might need more details, like the exact year of the population figure. Including that could be helpful, so I\'ll add "year": 2023. That way, the user knows the data is up to date.\n\nPutting it all together, the JSON should look clean and well-structured. I\'ll make sure the syntax is correct, with proper commas and quotation marks. No markdown, just plain JSON.\n\nI think that\'s all. The user probably just needs the information quickly, so keeping it concise is key. I\'ll present the JSON without any extra fluff.\n</think>{\n\n"name": "Paris",\n"population": 21500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1265, 1477, 279, 1429, 3213, 7042, 821, 13, 358, 6099, 429, 279, 7042, 315, 12095, 702, 1012, 7826, 11, 714, 358, 2776, 537, 2704, 315, 279, 4734, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 10146, 429, 382, 14190, 11, 7196, 358, 1265, 1779, 279, 5535, 13142, 13, 358, 19091, 429, 304, 3213, 1635, 11, 12095, 702, 3884, 264, 8112, 5263, 13, 6771, 752, 1744, 1112, 358, 4411, 279, 7042, 374, 13187, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 438, 315, 220, 17, 15, 17, 18, 13, 2938, 4977, 911, 1290, 11, 714, 358, 2776, 537, 220, 16, 15, 15, 4, 3654, 13, 358, 1265, 1281, 2704, 311, 3042, 419, 1995, 29257, 382, 5847, 11, 358, 1184, 311, 5944, 419, 1119, 264, 4718, 3561, 13, 4718, 7460, 1376, 19083, 13530, 11, 773, 358, 3278, 1855, 458, 1633, 448, 6894, 1075, 330, 8926, 497, 330, 44441, 497, 323, 7196, 330, 11141, 1, 369, 2266, 13, 576, 3283, 374, 12095, 11, 279, 7042, 374, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 11, 323, 279, 3146, 374, 9625, 382, 40, 1265, 1083, 2908, 421, 279, 1196, 2578, 1184, 803, 3565, 11, 1075, 279, 4734, 1042, 315, 279, 7042, 7071, 13, 55121, 429, 1410, 387, 10950, 11, 773, 358, 3278, 912, 330, 3157, 788, 220, 17, 15, 17, 18, 13, 2938, 1616, 11, 279, 1196, 8788, 279, 821, 374, 705, 311, 2400, 382, 97904, 432, 678, 3786, 11, 279, 4718, 1265, 1401, 4240, 323, 1632, 12, 51143, 13, 358, 3278, 1281, 2704, 279, 19482, 374, 4396, 11, 448, 6169, 76602, 323, 54231, 15423, 13, 2308, 50494, 11, 1101, 14396, 4718, 382, 40, 1744, 429, 594, 678, 13, 576, 1196, 4658, 1101, 3880, 279, 1995, 6157, 11, 773, 10282, 432, 63594, 374, 1376, 13, 358, 3278, 3042, 279, 4718, 2041, 894, 4960, 1320, 1362, 624, 151649, 4257, 1, 606, 788, 330, 59604, 756, 1, 44441, 788, 220, 17, 16, 20, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15], 'meta_info': {'id': 'd613f22d0b894209a7e9798cd53fc8c0', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 19.786647586151958, 'response_sent_to_client_ts': 1775024372.2926648}}



<strong style='color: #00008B;'>reasoing_content: Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.<br><br>First, I need to identify the capital of France. I know that Paris is the capital, so that's straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I'm not sure of the exact number. I think it's around 2 million, but I should verify that.<br><br>Wait, maybe I should check the latest statistics. I recall that in recent years, Paris has seen a slight increase. Let me think... I believe the population is approximately 2,150,000 as of 2023. That seems about right, but I'm not 100% certain. I should make sure to present this information accurately.<br><br>Next, I need to structure this into a JSON format. JSON requires key-value pairs, so I'll create an object with keys like "city", "population", and maybe "country" for context. The city is Paris, the population is 2,150,000, and the country is France.<br><br>I should also consider if the user might need more details, like the exact year of the population figure. Including that could be helpful, so I'll add "year": 2023. That way, the user knows the data is up to date.<br><br>Putting it all together, the JSON should look clean and well-structured. I'll make sure the syntax is correct, with proper commas and quotation marks. No markdown, just plain JSON.<br><br>I think that's all. The user probably just needs the information quickly, so keeping it concise is key. I'll present the JSON without any extra fluff.<br><br><br>content: {<br><br>"name": "Paris",<br>"population": 21500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000</strong>


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


<strong style='color: #00008B;'>{'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. I know that Paris is the capital, so that\'s straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I\'m not exactly sure of the current number. I think it\'s around 2 million, but I should verify that.\n\nWait, maybe I should check some reliable sources to confirm the population. I recall that the population figures can vary depending on the source and the year. For example, the 2020 census might have a slightly different number than the 2021 estimate. I think the population was around 2,165,000 in 2021, but I\'m not 100% certain. I should make sure to use the most accurate and up-to-date information.\n\nAlso, the user wants the information in JSON format. JSON is a data interchange format, so I\'ll need to structure the data accordingly. I should include the city name, population, and maybe the year of the data. It\'s important to present the information clearly and accurately, so I\'ll double-check the numbers to avoid any mistakes.\n\nI should also consider if there are any other relevant details the user might find useful, like the area of the city or some key facts about it. But since the user specifically asked for population, I\'ll focus on that. Maybe adding a note about the population figure being approximate would be helpful, just in case.\n\nPutting it all together, I\'ll structure the JSON with the city name, population, and the year. I\'ll make sure the syntax is correct, using quotation marks and commas appropriately. I\'ll also keep the language clear and straightforward so that the user can easily understand the information.\n\nFinally, I\'ll review the JSON to ensure there are no errors and that the data is accurate. This way, the user gets a reliable and well-formatted response to their query.\n</think>{"name": "Paris", "population": 2165000}', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1265, 1477, 279, 1429, 3213, 7042, 821, 13, 358, 6099, 429, 279, 7042, 315, 12095, 702, 1012, 7826, 11, 714, 358, 2776, 537, 6896, 2704, 315, 279, 1482, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 10146, 429, 382, 14190, 11, 7196, 358, 1265, 1779, 1045, 14720, 8173, 311, 7683, 279, 7042, 13, 358, 19091, 429, 279, 7042, 12396, 646, 13289, 11649, 389, 279, 2530, 323, 279, 1042, 13, 1752, 3110, 11, 279, 220, 17, 15, 17, 15, 43602, 2578, 614, 264, 10078, 2155, 1372, 1091, 279, 220, 17, 15, 17, 16, 16045, 13, 358, 1744, 279, 7042, 572, 2163, 220, 17, 11, 16, 21, 20, 11, 15, 15, 15, 304, 220, 17, 15, 17, 16, 11, 714, 358, 2776, 537, 220, 16, 15, 15, 4, 3654, 13, 358, 1265, 1281, 2704, 311, 990, 279, 1429, 13382, 323, 705, 4686, 18413, 1995, 382, 13394, 11, 279, 1196, 6801, 279, 1995, 304, 4718, 3561, 13, 4718, 374, 264, 821, 51263, 3561, 11, 773, 358, 3278, 1184, 311, 5944, 279, 821, 27079, 13, 358, 1265, 2924, 279, 3283, 829, 11, 7042, 11, 323, 7196, 279, 1042, 315, 279, 821, 13, 1084, 594, 2989, 311, 3042, 279, 1995, 9355, 323, 29257, 11, 773, 358, 3278, 1990, 15934, 279, 5109, 311, 5648, 894, 20643, 382, 40, 1265, 1083, 2908, 421, 1052, 525, 894, 1008, 9760, 3565, 279, 1196, 2578, 1477, 5390, 11, 1075, 279, 3082, 315, 279, 3283, 476, 1045, 1376, 13064, 911, 432, 13, 1988, 2474, 279, 1196, 11689, 4588, 369, 7042, 11, 358, 3278, 5244, 389, 429, 13, 10696, 7842, 264, 5185, 911, 279, 7042, 7071, 1660, 44868, 1035, 387, 10950, 11, 1101, 304, 1142, 382, 97904, 432, 678, 3786, 11, 358, 3278, 5944, 279, 4718, 448, 279, 3283, 829, 11, 7042, 11, 323, 279, 1042, 13, 358, 3278, 1281, 2704, 279, 19482, 374, 4396, 11, 1667, 54231, 15423, 323, 76602, 34901, 13, 358, 3278, 1083, 2506, 279, 4128, 2797, 323, 30339, 773, 429, 279, 1196, 646, 6707, 3535, 279, 1995, 382, 23949, 11, 358, 3278, 3395, 279, 4718, 311, 5978, 1052, 525, 902, 5975, 323, 429, 279, 821, 374, 13382, 13, 1096, 1616, 11, 279, 1196, 5221, 264, 14720, 323, 1632, 8460, 12127, 2033, 311, 862, 3239, 624, 151649, 4913, 606, 788, 330, 59604, 497, 330, 44441, 788, 220, 17, 16, 21, 20, 15, 15, 15, 92, 151643], 'meta_info': {'id': '2e2df1ba466f4b4ba55f99026b5c7af4', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 447, 'cached_tokens': 22, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 4.826124049723148, 'response_sent_to_client_ts': 1775024377.1352737}}</strong>


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

    [{'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '3a4fa6418bfb47a3bbb6e50c720dbd68', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.130088793579489, 'response_sent_to_client_ts': 1775024377.3108838}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '80bb43c33d634417a0c35531ceefb5d8', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.1299440087750554, 'response_sent_to_client_ts': 1775024377.3108947}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '3ecdd1088f81408bbc2d469fc8df108a', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.1298882057890296, 'response_sent_to_client_ts': 1775024377.3108993}}]


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

    {'text': ' France, and the \n\\( n \\)  \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\(', 'output_ids': [9625, 11, 323, 279, 220, 198, 44292, 308, 1124, 8, 220, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767], 'meta_info': {'id': '6bd9706dadbf4363ac30f0a5ccd7f2a0', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 6, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 19.65288472501561, 'response_sent_to_client_ts': 1775024396.9714959}}


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


<strong style='color: #00008B;'>{'text': 'Alright, so the user has asked for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to figure out which city is the capital of France. I know Paris is the capital, so that\'s the starting point.\n\nNext, I need to find the current population of Paris. Population numbers can change yearly, so I should make sure the data is up-to-date. As of recent estimates, Paris has a population of around 2,177,000 residents.\n\nNow, the user wants this information in JSON format. JSON stands for JavaScript Object Notation, which is a lightweight data-interchange format that\'s easy for humans to read and write and easy for machines to parse and generate.\n\nI should structure the JSON with a key-value pair. The main key here would likely be "capital" with the value being another object containing population and the city name as a subkey.\n\nSo the structure would be something like:\n{\n  "capital": {\n    "population": 2177000,\n    "city": "Paris"\n  }\n}\n\nI need to make sure the numbers are accurate. Maybe I should cross-reference the population with a reliable source to ensure accuracy. Population figures can vary, so having the most recent data is crucial.\n\nI should also consider if the user needs just the population number or wants additional details. But since they specifically mentioned population, I\'ll stick to that.\n\nIs there anything else the user might need? They asked for JSON, so that\'s the format. I don\'t see any other requirements mentioned, so I should provide just this data without extra information.\n\nI think that covers everything. The response should be clear and concise, providing the required data in the requested format.\n</think>\n\n```json\n{\n  "capital": {\n    "population": 2177000,\n    "city": "Paris"\n  }\n}\n```', 'output_ids': [71486, 11, 773, 279, 1196, 702, 4588, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 7071, 700, 892, 3283, 374, 279, 6722, 315, 9625, 13, 358, 1414, 12095, 374, 279, 6722, 11, 773, 429, 594, 279, 5916, 1459, 382, 5847, 11, 358, 1184, 311, 1477, 279, 1482, 7042, 315, 12095, 13, 39529, 5109, 646, 2297, 44270, 11, 773, 358, 1265, 1281, 2704, 279, 821, 374, 705, 4686, 18413, 13, 1634, 315, 3213, 17530, 11, 12095, 702, 264, 7042, 315, 2163, 220, 17, 11, 16, 22, 22, 11, 15, 15, 15, 10826, 382, 7039, 11, 279, 1196, 6801, 419, 1995, 304, 4718, 3561, 13, 4718, 13352, 369, 12914, 3002, 2806, 367, 11, 892, 374, 264, 29144, 821, 44894, 3373, 3561, 429, 594, 4135, 369, 12677, 311, 1349, 323, 3270, 323, 4135, 369, 12645, 311, 4715, 323, 6923, 382, 40, 1265, 5944, 279, 4718, 448, 264, 1376, 19083, 6716, 13, 576, 1887, 1376, 1588, 1035, 4363, 387, 330, 65063, 1, 448, 279, 897, 1660, 2441, 1633, 8482, 7042, 323, 279, 3283, 829, 438, 264, 1186, 792, 382, 4416, 279, 5944, 1035, 387, 2494, 1075, 510, 515, 220, 330, 65063, 788, 341, 262, 330, 44441, 788, 220, 17, 16, 22, 22, 15, 15, 15, 345, 262, 330, 8926, 788, 330, 59604, 698, 220, 456, 630, 40, 1184, 311, 1281, 2704, 279, 5109, 525, 13382, 13, 10696, 358, 1265, 5312, 72623, 279, 7042, 448, 264, 14720, 2530, 311, 5978, 13403, 13, 39529, 12396, 646, 13289, 11, 773, 3432, 279, 1429, 3213, 821, 374, 16587, 382, 40, 1265, 1083, 2908, 421, 279, 1196, 3880, 1101, 279, 7042, 1372, 476, 6801, 5107, 3565, 13, 1988, 2474, 807, 11689, 9733, 7042, 11, 358, 3278, 9214, 311, 429, 382, 3872, 1052, 4113, 770, 279, 1196, 2578, 1184, 30, 2379, 4588, 369, 4718, 11, 773, 429, 594, 279, 3561, 13, 358, 1513, 944, 1490, 894, 1008, 8502, 9733, 11, 773, 358, 1265, 3410, 1101, 419, 821, 2041, 4960, 1995, 382, 40, 1744, 429, 14521, 4297, 13, 576, 2033, 1265, 387, 2797, 323, 63594, 11, 8241, 279, 2567, 821, 304, 279, 11223, 3561, 624, 151649, 271, 73594, 2236, 198, 515, 220, 330, 65063, 788, 341, 262, 330, 44441, 788, 220, 17, 16, 22, 22, 15, 15, 15, 345, 262, 330, 8926, 788, 330, 59604, 698, 220, 456, 532, 73594, 151643], 'meta_info': {'id': '75f0c65a81b7449c88a2cc628e2fb4b5', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 397, 'cached_tokens': 22, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 3.7102352157235146, 'response_sent_to_client_ts': 1775024400.6919289}}</strong>



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


    2026-04-01 06:20:13.233 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 06:20:13] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 06:20:13.233 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 06:20:13] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 06:20:13.234 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 06:20:13] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 06:20:13.234 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 06:20:13] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 06:20:13.234 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 06:20:13] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:00<00:00,  1.14it/s]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.17s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.13s/it]


    2026-04-01 06:20:18,332 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 06:20:18] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:53,  3.05s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:53,  3.05s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:26,  1.54s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:26,  1.54s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:51,  1.06it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:51,  1.06it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:35,  1.52it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:35,  1.52it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:26,  2.03it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:26,  2.03it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:20,  2.57it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:20,  2.57it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:15,  3.20it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:15,  3.20it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:13,  3.83it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:13,  3.83it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:10,  4.69it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:10,  4.69it/s]

    Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:10,  4.69it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:07,  6.40it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:07,  6.40it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:05<00:07,  6.40it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:05<00:05,  7.94it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:05<00:05,  7.94it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:05<00:05,  7.94it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:05<00:04,  9.58it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:05<00:04,  9.58it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:05<00:04,  9.58it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:03, 11.52it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:03, 11.52it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:03, 11.52it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:05<00:03, 11.52it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 14.80it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 14.80it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 14.80it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 14.80it/s]

    Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:05<00:02, 14.80it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:05<00:01, 19.90it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:05<00:01, 19.90it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:05<00:01, 19.90it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:05<00:01, 19.90it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:05<00:01, 19.90it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:05<00:01, 19.90it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:05<00:01, 26.38it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:05<00:01, 26.38it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:05<00:01, 26.38it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:05<00:01, 26.38it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:05<00:01, 26.38it/s]

    Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:05<00:01, 26.38it/s]Compiling num tokens (num_tokens=320):  50%|█████     | 29/58 [00:05<00:01, 26.38it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:05<00:00, 33.76it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:05<00:00, 33.76it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:05<00:00, 33.76it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:05<00:00, 33.76it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:05<00:00, 33.76it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:05<00:00, 33.76it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:05<00:00, 33.76it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:05<00:00, 39.07it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:05<00:00, 39.07it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:05<00:00, 39.07it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:05<00:00, 39.07it/s]

    Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:06<00:00, 39.07it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:06<00:00, 39.07it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:06<00:00, 40.25it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:06<00:00, 40.25it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:06<00:00, 40.25it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:06<00:00, 40.25it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:06<00:00, 40.25it/s]Compiling num tokens (num_tokens=32):  79%|███████▉  | 46/58 [00:06<00:00, 40.25it/s]Compiling num tokens (num_tokens=28):  79%|███████▉  | 46/58 [00:06<00:00, 40.25it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:06<00:00, 44.49it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:06<00:00, 44.49it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:06<00:00, 44.49it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:06<00:00, 44.49it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:06<00:00, 44.49it/s]

    Compiling num tokens (num_tokens=8):  90%|████████▉ | 52/58 [00:06<00:00, 44.49it/s] Compiling num tokens (num_tokens=4):  90%|████████▉ | 52/58 [00:06<00:00, 44.49it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:06<00:00,  9.26it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=104.59 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=104.59 GB):   2%|▏         | 1/58 [00:00<00:33,  1.72it/s]Capturing num tokens (num_tokens=7680 avail_mem=104.56 GB):   2%|▏         | 1/58 [00:00<00:33,  1.72it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=104.56 GB):   3%|▎         | 2/58 [00:01<00:30,  1.81it/s]Capturing num tokens (num_tokens=7168 avail_mem=104.56 GB):   3%|▎         | 2/58 [00:01<00:30,  1.81it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=104.56 GB):   5%|▌         | 3/58 [00:01<00:27,  2.02it/s]Capturing num tokens (num_tokens=6656 avail_mem=104.57 GB):   5%|▌         | 3/58 [00:01<00:27,  2.02it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=104.57 GB):   7%|▋         | 4/58 [00:01<00:21,  2.55it/s]Capturing num tokens (num_tokens=6144 avail_mem=122.22 GB):   7%|▋         | 4/58 [00:01<00:21,  2.55it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=122.22 GB):   9%|▊         | 5/58 [00:02<00:19,  2.74it/s]Capturing num tokens (num_tokens=5632 avail_mem=122.22 GB):   9%|▊         | 5/58 [00:02<00:19,  2.74it/s]Capturing num tokens (num_tokens=5632 avail_mem=122.22 GB):  10%|█         | 6/58 [00:02<00:15,  3.33it/s]Capturing num tokens (num_tokens=5120 avail_mem=122.22 GB):  10%|█         | 6/58 [00:02<00:15,  3.33it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=122.22 GB):  12%|█▏        | 7/58 [00:02<00:13,  3.90it/s]Capturing num tokens (num_tokens=4608 avail_mem=122.23 GB):  12%|█▏        | 7/58 [00:02<00:13,  3.90it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=122.23 GB):  14%|█▍        | 8/58 [00:02<00:11,  4.17it/s]Capturing num tokens (num_tokens=4096 avail_mem=122.23 GB):  14%|█▍        | 8/58 [00:02<00:11,  4.17it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=122.23 GB):  16%|█▌        | 9/58 [00:02<00:11,  4.37it/s]Capturing num tokens (num_tokens=3840 avail_mem=122.24 GB):  16%|█▌        | 9/58 [00:02<00:11,  4.37it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=122.24 GB):  17%|█▋        | 10/58 [00:03<00:11,  4.34it/s]Capturing num tokens (num_tokens=3584 avail_mem=122.24 GB):  17%|█▋        | 10/58 [00:03<00:11,  4.34it/s]Capturing num tokens (num_tokens=3584 avail_mem=122.24 GB):  19%|█▉        | 11/58 [00:03<00:09,  5.09it/s]Capturing num tokens (num_tokens=3328 avail_mem=122.24 GB):  19%|█▉        | 11/58 [00:03<00:09,  5.09it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=122.24 GB):  21%|██        | 12/58 [00:03<00:07,  5.95it/s]Capturing num tokens (num_tokens=3072 avail_mem=122.23 GB):  21%|██        | 12/58 [00:03<00:07,  5.95it/s]Capturing num tokens (num_tokens=2816 avail_mem=122.24 GB):  21%|██        | 12/58 [00:03<00:07,  5.95it/s]Capturing num tokens (num_tokens=2816 avail_mem=122.24 GB):  24%|██▍       | 14/58 [00:03<00:05,  7.50it/s]Capturing num tokens (num_tokens=2560 avail_mem=122.24 GB):  24%|██▍       | 14/58 [00:03<00:05,  7.50it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=122.24 GB):  24%|██▍       | 14/58 [00:03<00:05,  7.50it/s]Capturing num tokens (num_tokens=2304 avail_mem=122.24 GB):  28%|██▊       | 16/58 [00:03<00:04,  9.08it/s]Capturing num tokens (num_tokens=2048 avail_mem=122.23 GB):  28%|██▊       | 16/58 [00:03<00:04,  9.08it/s]Capturing num tokens (num_tokens=1792 avail_mem=122.24 GB):  28%|██▊       | 16/58 [00:03<00:04,  9.08it/s]Capturing num tokens (num_tokens=1792 avail_mem=122.24 GB):  31%|███       | 18/58 [00:03<00:03, 10.83it/s]Capturing num tokens (num_tokens=1536 avail_mem=122.23 GB):  31%|███       | 18/58 [00:03<00:03, 10.83it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=122.23 GB):  31%|███       | 18/58 [00:03<00:03, 10.83it/s]Capturing num tokens (num_tokens=1280 avail_mem=122.23 GB):  34%|███▍      | 20/58 [00:03<00:03, 10.06it/s]Capturing num tokens (num_tokens=1024 avail_mem=122.23 GB):  34%|███▍      | 20/58 [00:03<00:03, 10.06it/s]Capturing num tokens (num_tokens=960 avail_mem=122.23 GB):  34%|███▍      | 20/58 [00:04<00:03, 10.06it/s] 

    Capturing num tokens (num_tokens=960 avail_mem=122.23 GB):  38%|███▊      | 22/58 [00:04<00:03, 11.93it/s]Capturing num tokens (num_tokens=896 avail_mem=122.22 GB):  38%|███▊      | 22/58 [00:04<00:03, 11.93it/s]Capturing num tokens (num_tokens=832 avail_mem=122.22 GB):  38%|███▊      | 22/58 [00:04<00:03, 11.93it/s]Capturing num tokens (num_tokens=768 avail_mem=122.22 GB):  38%|███▊      | 22/58 [00:04<00:03, 11.93it/s]Capturing num tokens (num_tokens=768 avail_mem=122.22 GB):  43%|████▎     | 25/58 [00:04<00:02, 15.42it/s]Capturing num tokens (num_tokens=704 avail_mem=122.21 GB):  43%|████▎     | 25/58 [00:04<00:02, 15.42it/s]Capturing num tokens (num_tokens=640 avail_mem=122.21 GB):  43%|████▎     | 25/58 [00:04<00:02, 15.42it/s]Capturing num tokens (num_tokens=576 avail_mem=122.21 GB):  43%|████▎     | 25/58 [00:04<00:02, 15.42it/s]

    Capturing num tokens (num_tokens=576 avail_mem=122.21 GB):  48%|████▊     | 28/58 [00:04<00:01, 17.65it/s]Capturing num tokens (num_tokens=512 avail_mem=122.20 GB):  48%|████▊     | 28/58 [00:04<00:01, 17.65it/s]Capturing num tokens (num_tokens=480 avail_mem=122.20 GB):  48%|████▊     | 28/58 [00:04<00:01, 17.65it/s]Capturing num tokens (num_tokens=448 avail_mem=122.20 GB):  48%|████▊     | 28/58 [00:04<00:01, 17.65it/s]Capturing num tokens (num_tokens=416 avail_mem=122.19 GB):  48%|████▊     | 28/58 [00:04<00:01, 17.65it/s]Capturing num tokens (num_tokens=416 avail_mem=122.19 GB):  55%|█████▌    | 32/58 [00:04<00:01, 22.02it/s]Capturing num tokens (num_tokens=384 avail_mem=122.19 GB):  55%|█████▌    | 32/58 [00:04<00:01, 22.02it/s]Capturing num tokens (num_tokens=352 avail_mem=122.18 GB):  55%|█████▌    | 32/58 [00:04<00:01, 22.02it/s]Capturing num tokens (num_tokens=320 avail_mem=122.18 GB):  55%|█████▌    | 32/58 [00:04<00:01, 22.02it/s]Capturing num tokens (num_tokens=288 avail_mem=122.18 GB):  55%|█████▌    | 32/58 [00:04<00:01, 22.02it/s]

    Capturing num tokens (num_tokens=288 avail_mem=122.18 GB):  62%|██████▏   | 36/58 [00:04<00:00, 25.91it/s]Capturing num tokens (num_tokens=256 avail_mem=122.17 GB):  62%|██████▏   | 36/58 [00:04<00:00, 25.91it/s]Capturing num tokens (num_tokens=240 avail_mem=122.17 GB):  62%|██████▏   | 36/58 [00:04<00:00, 25.91it/s]Capturing num tokens (num_tokens=224 avail_mem=122.17 GB):  62%|██████▏   | 36/58 [00:04<00:00, 25.91it/s]Capturing num tokens (num_tokens=208 avail_mem=122.16 GB):  62%|██████▏   | 36/58 [00:04<00:00, 25.91it/s]Capturing num tokens (num_tokens=208 avail_mem=122.16 GB):  69%|██████▉   | 40/58 [00:04<00:00, 29.25it/s]Capturing num tokens (num_tokens=192 avail_mem=122.16 GB):  69%|██████▉   | 40/58 [00:04<00:00, 29.25it/s]Capturing num tokens (num_tokens=176 avail_mem=122.15 GB):  69%|██████▉   | 40/58 [00:04<00:00, 29.25it/s]Capturing num tokens (num_tokens=160 avail_mem=122.15 GB):  69%|██████▉   | 40/58 [00:04<00:00, 29.25it/s]Capturing num tokens (num_tokens=144 avail_mem=122.15 GB):  69%|██████▉   | 40/58 [00:04<00:00, 29.25it/s]

    Capturing num tokens (num_tokens=144 avail_mem=122.15 GB):  76%|███████▌  | 44/58 [00:04<00:00, 30.47it/s]Capturing num tokens (num_tokens=128 avail_mem=122.16 GB):  76%|███████▌  | 44/58 [00:04<00:00, 30.47it/s]Capturing num tokens (num_tokens=112 avail_mem=122.15 GB):  76%|███████▌  | 44/58 [00:04<00:00, 30.47it/s]Capturing num tokens (num_tokens=96 avail_mem=122.15 GB):  76%|███████▌  | 44/58 [00:04<00:00, 30.47it/s] Capturing num tokens (num_tokens=80 avail_mem=122.14 GB):  76%|███████▌  | 44/58 [00:04<00:00, 30.47it/s]

    Capturing num tokens (num_tokens=80 avail_mem=122.14 GB):  83%|████████▎ | 48/58 [00:04<00:00, 25.05it/s]Capturing num tokens (num_tokens=64 avail_mem=122.14 GB):  83%|████████▎ | 48/58 [00:04<00:00, 25.05it/s]Capturing num tokens (num_tokens=48 avail_mem=122.14 GB):  83%|████████▎ | 48/58 [00:05<00:00, 25.05it/s]Capturing num tokens (num_tokens=32 avail_mem=122.13 GB):  83%|████████▎ | 48/58 [00:05<00:00, 25.05it/s]Capturing num tokens (num_tokens=28 avail_mem=122.13 GB):  83%|████████▎ | 48/58 [00:05<00:00, 25.05it/s]Capturing num tokens (num_tokens=28 avail_mem=122.13 GB):  90%|████████▉ | 52/58 [00:05<00:00, 28.00it/s]Capturing num tokens (num_tokens=24 avail_mem=122.13 GB):  90%|████████▉ | 52/58 [00:05<00:00, 28.00it/s]Capturing num tokens (num_tokens=20 avail_mem=122.12 GB):  90%|████████▉ | 52/58 [00:05<00:00, 28.00it/s]Capturing num tokens (num_tokens=16 avail_mem=122.12 GB):  90%|████████▉ | 52/58 [00:05<00:00, 28.00it/s]Capturing num tokens (num_tokens=12 avail_mem=122.12 GB):  90%|████████▉ | 52/58 [00:05<00:00, 28.00it/s]

    Capturing num tokens (num_tokens=12 avail_mem=122.12 GB):  97%|█████████▋| 56/58 [00:05<00:00, 30.45it/s]Capturing num tokens (num_tokens=8 avail_mem=122.11 GB):  97%|█████████▋| 56/58 [00:05<00:00, 30.45it/s] Capturing num tokens (num_tokens=4 avail_mem=122.11 GB):  97%|█████████▋| 56/58 [00:05<00:00, 30.45it/s]Capturing num tokens (num_tokens=4 avail_mem=122.11 GB): 100%|██████████| 58/58 [00:05<00:00, 11.00it/s]


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
        "population": 138000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
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
    Generated text: Berlin is the capital of Germany
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
    
    Generated text: Alright, so I just got this query from a user asking for the information and population of the capital of France in JSON format. Let me try to break this down.
    
    First, I know that the capital of France is Paris. That's pretty straightforward. Now, the user wants this information in JSON. I remember that JSON stands for JavaScript Object Notation, and it's a way to structure data in an organized manner. So, I need to create a JSON object that includes Paris and its population.
    
    Wait, the user specifically mentioned the population. I should check what the current population of Paris is. From what I recall, the population of Paris is over 3 million. Let me see... I think the latest estimate is around 3,500,000. But I should make sure that's up to date. Maybe I should look it up to confirm. Oh, okay, double-checking a reliable source, yes, as of 2023, the population is approximately 3,500,000. That seems about right.
    
    Now, structuring the JSON. The user didn't specify the exact format, but a typical way is to have an object with a "name" and "population" key. So, I'll include "city": "Paris" and "population": 3500000. It's important to note that population figures can vary slightly depending on the source and the year, so I should mention that in the response.
    
    Also, considering the user's request, they might just want a simple data structure. They didn't specify any particular formatting beyond JSON, so I'll stick to a basic structure without any additional fields unless necessary.
    
    Wait, should I include the units for population? Probably not necessary here since it's a number. Just the number is sufficient.
    
    Putting it all together, the JSON should look like this:
    
    {
      "city": "Paris",
      "population": 3500000
    }
    
    That's concise and meets the user's requirements. I think this should satisfy their needs. They might be using this data for a project or a presentation, so accuracy is key. Since I'm providing an estimate, I should also add a note that population figures may vary.
    
    Hmm, should I mention the current year or the latest data source? Maybe it's better not to overcomplicate it unless the user asks for it. Keeping it simple is probably best.
    
    Yes, I think that covers everything the user asked for. I'll present the JSON and add the population note as a helpful aside.
    </think>
    
    Here is the information and population of the capital of France in JSON format:
    
    ```json
    {
      "city": "Paris",
      "population": 3500000
    }
    ```
    
    The population of Paris is approximately 3.5 million.



```python
llm.shutdown()
```
