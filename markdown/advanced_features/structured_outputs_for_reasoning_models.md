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


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:172: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    Multi-thread loading shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Multi-thread loading shards:  50% Completed | 1/2 [00:00<00:00,  1.09it/s]

    Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.25s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.20s/it]


    2026-04-04 08:58:21,340 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 08:58:21] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:56,  3.09s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:56,  3.09s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:24,  1.50s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:24,  1.50s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:49,  1.10it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:49,  1.10it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:33,  1.61it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:33,  1.61it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:24,  2.20it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:24,  2.20it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.83it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.83it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.56it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.56it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.32it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.32it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.19it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.19it/s]

    Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.19it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:06,  6.87it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:06,  6.87it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:06,  6.87it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:04<00:05,  8.30it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:04<00:05,  8.30it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:04<00:05,  8.30it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:05<00:04,  9.77it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:05<00:04,  9.77it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:05<00:04,  9.77it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:03, 11.73it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:03, 11.73it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:03, 11.73it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:05<00:03, 11.73it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 14.96it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 14.96it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 14.96it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 14.96it/s]

    Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:05<00:02, 14.96it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:05<00:01, 20.00it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:05<00:01, 20.00it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:05<00:01, 20.00it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:05<00:01, 20.00it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:05<00:01, 19.51it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:05<00:01, 19.51it/s]

    Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:05<00:01, 19.51it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:05<00:01, 19.51it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:05<00:01, 19.94it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:05<00:01, 19.94it/s]Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:05<00:01, 19.94it/s]Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:05<00:01, 19.94it/s]

    Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:05<00:01, 20.32it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:05<00:01, 20.32it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:05<00:01, 20.32it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:05<00:01, 20.32it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:05<00:01, 21.13it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:05<00:01, 21.13it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:06<00:01, 21.13it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:06<00:01, 21.13it/s]

    Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:06<00:00, 23.10it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:06<00:00, 23.10it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:06<00:00, 23.10it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:06<00:00, 23.10it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:06<00:00, 23.44it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:06<00:00, 23.44it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:06<00:00, 23.44it/s]

    Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:06<00:00, 23.44it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:06<00:00, 24.10it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:06<00:00, 24.10it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:06<00:00, 24.10it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:06<00:00, 24.10it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:06<00:00, 24.58it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:06<00:00, 24.58it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:06<00:00, 24.58it/s]

    Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:06<00:00, 24.58it/s]Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:06<00:00, 24.58it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:06<00:00, 26.96it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:06<00:00, 26.96it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:06<00:00, 26.96it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:06<00:00, 26.96it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:06<00:00, 26.96it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:06<00:00, 28.46it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:06<00:00, 28.46it/s] 

    Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:06<00:00, 28.46it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:06<00:00,  8.64it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=86.22 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=86.22 GB):   2%|▏         | 1/58 [00:00<00:36,  1.56it/s]Capturing num tokens (num_tokens=7680 avail_mem=86.19 GB):   2%|▏         | 1/58 [00:00<00:36,  1.56it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=86.19 GB):   3%|▎         | 2/58 [00:01<00:32,  1.70it/s]Capturing num tokens (num_tokens=7168 avail_mem=86.19 GB):   3%|▎         | 2/58 [00:01<00:32,  1.70it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=86.19 GB):   5%|▌         | 3/58 [00:01<00:29,  1.86it/s]Capturing num tokens (num_tokens=6656 avail_mem=86.19 GB):   5%|▌         | 3/58 [00:01<00:29,  1.86it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=86.19 GB):   7%|▋         | 4/58 [00:02<00:27,  1.98it/s]Capturing num tokens (num_tokens=6144 avail_mem=85.19 GB):   7%|▋         | 4/58 [00:02<00:27,  1.98it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=85.19 GB):   9%|▊         | 5/58 [00:02<00:28,  1.87it/s]Capturing num tokens (num_tokens=5632 avail_mem=85.20 GB):   9%|▊         | 5/58 [00:02<00:28,  1.87it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=85.20 GB):  10%|█         | 6/58 [00:03<00:26,  1.95it/s]Capturing num tokens (num_tokens=5120 avail_mem=85.20 GB):  10%|█         | 6/58 [00:03<00:26,  1.95it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=85.20 GB):  12%|█▏        | 7/58 [00:03<00:24,  2.05it/s]Capturing num tokens (num_tokens=4608 avail_mem=85.20 GB):  12%|█▏        | 7/58 [00:03<00:24,  2.05it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=85.20 GB):  14%|█▍        | 8/58 [00:03<00:22,  2.21it/s]Capturing num tokens (num_tokens=4096 avail_mem=85.20 GB):  14%|█▍        | 8/58 [00:03<00:22,  2.21it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=85.20 GB):  16%|█▌        | 9/58 [00:04<00:19,  2.47it/s]Capturing num tokens (num_tokens=3840 avail_mem=85.20 GB):  16%|█▌        | 9/58 [00:04<00:19,  2.47it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=85.20 GB):  17%|█▋        | 10/58 [00:04<00:19,  2.47it/s]Capturing num tokens (num_tokens=3584 avail_mem=84.01 GB):  17%|█▋        | 10/58 [00:04<00:19,  2.47it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=84.01 GB):  19%|█▉        | 11/58 [00:05<00:19,  2.45it/s]Capturing num tokens (num_tokens=3328 avail_mem=85.17 GB):  19%|█▉        | 11/58 [00:05<00:19,  2.45it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=85.17 GB):  21%|██        | 12/58 [00:05<00:18,  2.53it/s]Capturing num tokens (num_tokens=3072 avail_mem=84.18 GB):  21%|██        | 12/58 [00:05<00:18,  2.53it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=84.18 GB):  22%|██▏       | 13/58 [00:05<00:17,  2.54it/s]Capturing num tokens (num_tokens=2816 avail_mem=85.17 GB):  22%|██▏       | 13/58 [00:05<00:17,  2.54it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=85.17 GB):  24%|██▍       | 14/58 [00:06<00:16,  2.65it/s]Capturing num tokens (num_tokens=2560 avail_mem=84.25 GB):  24%|██▍       | 14/58 [00:06<00:16,  2.65it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=84.25 GB):  26%|██▌       | 15/58 [00:06<00:15,  2.75it/s]Capturing num tokens (num_tokens=2304 avail_mem=85.18 GB):  26%|██▌       | 15/58 [00:06<00:15,  2.75it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=85.18 GB):  28%|██▊       | 16/58 [00:06<00:14,  2.92it/s]Capturing num tokens (num_tokens=2048 avail_mem=84.32 GB):  28%|██▊       | 16/58 [00:06<00:14,  2.92it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=84.32 GB):  29%|██▉       | 17/58 [00:07<00:13,  3.11it/s]Capturing num tokens (num_tokens=1792 avail_mem=85.18 GB):  29%|██▉       | 17/58 [00:07<00:13,  3.11it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=85.18 GB):  31%|███       | 18/58 [00:07<00:12,  3.29it/s]Capturing num tokens (num_tokens=1536 avail_mem=84.39 GB):  31%|███       | 18/58 [00:07<00:12,  3.29it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=84.39 GB):  33%|███▎      | 19/58 [00:07<00:11,  3.50it/s]Capturing num tokens (num_tokens=1280 avail_mem=85.18 GB):  33%|███▎      | 19/58 [00:07<00:11,  3.50it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=85.18 GB):  34%|███▍      | 20/58 [00:07<00:09,  3.84it/s]Capturing num tokens (num_tokens=1024 avail_mem=84.45 GB):  34%|███▍      | 20/58 [00:07<00:09,  3.84it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=84.45 GB):  36%|███▌      | 21/58 [00:08<00:09,  4.08it/s]Capturing num tokens (num_tokens=960 avail_mem=84.66 GB):  36%|███▌      | 21/58 [00:08<00:09,  4.08it/s] Capturing num tokens (num_tokens=960 avail_mem=84.66 GB):  38%|███▊      | 22/58 [00:08<00:08,  4.49it/s]Capturing num tokens (num_tokens=896 avail_mem=84.51 GB):  38%|███▊      | 22/58 [00:08<00:08,  4.49it/s]

    Capturing num tokens (num_tokens=896 avail_mem=84.51 GB):  40%|███▉      | 23/58 [00:08<00:07,  4.68it/s]Capturing num tokens (num_tokens=832 avail_mem=84.51 GB):  40%|███▉      | 23/58 [00:08<00:07,  4.68it/s]Capturing num tokens (num_tokens=832 avail_mem=84.51 GB):  41%|████▏     | 24/58 [00:08<00:06,  5.25it/s]Capturing num tokens (num_tokens=768 avail_mem=84.58 GB):  41%|████▏     | 24/58 [00:08<00:06,  5.25it/s]

    Capturing num tokens (num_tokens=768 avail_mem=84.58 GB):  43%|████▎     | 25/58 [00:08<00:06,  5.29it/s]Capturing num tokens (num_tokens=704 avail_mem=84.57 GB):  43%|████▎     | 25/58 [00:08<00:06,  5.29it/s]Capturing num tokens (num_tokens=704 avail_mem=84.57 GB):  45%|████▍     | 26/58 [00:08<00:05,  6.02it/s]Capturing num tokens (num_tokens=640 avail_mem=84.64 GB):  45%|████▍     | 26/58 [00:08<00:05,  6.02it/s]

    Capturing num tokens (num_tokens=640 avail_mem=84.64 GB):  47%|████▋     | 27/58 [00:08<00:04,  6.29it/s]Capturing num tokens (num_tokens=576 avail_mem=85.17 GB):  47%|████▋     | 27/58 [00:08<00:04,  6.29it/s]Capturing num tokens (num_tokens=576 avail_mem=85.17 GB):  48%|████▊     | 28/58 [00:09<00:04,  7.02it/s]Capturing num tokens (num_tokens=512 avail_mem=84.67 GB):  48%|████▊     | 28/58 [00:09<00:04,  7.02it/s]

    Capturing num tokens (num_tokens=512 avail_mem=84.67 GB):  50%|█████     | 29/58 [00:09<00:03,  7.30it/s]Capturing num tokens (num_tokens=480 avail_mem=85.16 GB):  50%|█████     | 29/58 [00:09<00:03,  7.30it/s]Capturing num tokens (num_tokens=448 avail_mem=84.69 GB):  50%|█████     | 29/58 [00:09<00:03,  7.30it/s]

    Capturing num tokens (num_tokens=448 avail_mem=84.69 GB):  53%|█████▎    | 31/58 [00:09<00:03,  8.31it/s]Capturing num tokens (num_tokens=416 avail_mem=85.15 GB):  53%|█████▎    | 31/58 [00:09<00:03,  8.31it/s]Capturing num tokens (num_tokens=384 avail_mem=84.71 GB):  53%|█████▎    | 31/58 [00:09<00:03,  8.31it/s]Capturing num tokens (num_tokens=384 avail_mem=84.71 GB):  57%|█████▋    | 33/58 [00:09<00:02,  9.11it/s]Capturing num tokens (num_tokens=352 avail_mem=85.14 GB):  57%|█████▋    | 33/58 [00:09<00:02,  9.11it/s]

    Capturing num tokens (num_tokens=352 avail_mem=85.14 GB):  59%|█████▊    | 34/58 [00:09<00:02,  9.14it/s]Capturing num tokens (num_tokens=320 avail_mem=84.73 GB):  59%|█████▊    | 34/58 [00:09<00:02,  9.14it/s]Capturing num tokens (num_tokens=288 avail_mem=85.13 GB):  59%|█████▊    | 34/58 [00:09<00:02,  9.14it/s]Capturing num tokens (num_tokens=288 avail_mem=85.13 GB):  62%|██████▏   | 36/58 [00:09<00:02, 10.62it/s]Capturing num tokens (num_tokens=256 avail_mem=85.13 GB):  62%|██████▏   | 36/58 [00:09<00:02, 10.62it/s]Capturing num tokens (num_tokens=240 avail_mem=84.79 GB):  62%|██████▏   | 36/58 [00:09<00:02, 10.62it/s]

    Capturing num tokens (num_tokens=240 avail_mem=84.79 GB):  66%|██████▌   | 38/58 [00:09<00:01, 12.30it/s]Capturing num tokens (num_tokens=224 avail_mem=85.12 GB):  66%|██████▌   | 38/58 [00:09<00:01, 12.30it/s]Capturing num tokens (num_tokens=208 avail_mem=83.78 GB):  66%|██████▌   | 38/58 [00:10<00:01, 12.30it/s]Capturing num tokens (num_tokens=208 avail_mem=83.78 GB):  69%|██████▉   | 40/58 [00:10<00:01, 11.42it/s]Capturing num tokens (num_tokens=192 avail_mem=84.08 GB):  69%|██████▉   | 40/58 [00:10<00:01, 11.42it/s]

    Capturing num tokens (num_tokens=176 avail_mem=84.08 GB):  69%|██████▉   | 40/58 [00:10<00:01, 11.42it/s]Capturing num tokens (num_tokens=176 avail_mem=84.08 GB):  72%|███████▏  | 42/58 [00:10<00:01,  9.52it/s]Capturing num tokens (num_tokens=160 avail_mem=85.50 GB):  72%|███████▏  | 42/58 [00:10<00:01,  9.52it/s]

    Capturing num tokens (num_tokens=144 avail_mem=85.06 GB):  72%|███████▏  | 42/58 [00:10<00:01,  9.52it/s]Capturing num tokens (num_tokens=144 avail_mem=85.06 GB):  76%|███████▌  | 44/58 [00:10<00:01,  9.74it/s]Capturing num tokens (num_tokens=128 avail_mem=84.26 GB):  76%|███████▌  | 44/58 [00:10<00:01,  9.74it/s]

    Capturing num tokens (num_tokens=112 avail_mem=84.25 GB):  76%|███████▌  | 44/58 [00:10<00:01,  9.74it/s]Capturing num tokens (num_tokens=112 avail_mem=84.25 GB):  79%|███████▉  | 46/58 [00:10<00:01,  8.96it/s]Capturing num tokens (num_tokens=96 avail_mem=84.10 GB):  79%|███████▉  | 46/58 [00:10<00:01,  8.96it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=84.94 GB):  79%|███████▉  | 46/58 [00:10<00:01,  8.96it/s]Capturing num tokens (num_tokens=80 avail_mem=84.94 GB):  83%|████████▎ | 48/58 [00:11<00:01,  9.49it/s]Capturing num tokens (num_tokens=64 avail_mem=85.05 GB):  83%|████████▎ | 48/58 [00:11<00:01,  9.49it/s]Capturing num tokens (num_tokens=48 avail_mem=84.28 GB):  83%|████████▎ | 48/58 [00:11<00:01,  9.49it/s]

    Capturing num tokens (num_tokens=48 avail_mem=84.28 GB):  86%|████████▌ | 50/58 [00:11<00:00,  9.42it/s]Capturing num tokens (num_tokens=32 avail_mem=84.27 GB):  86%|████████▌ | 50/58 [00:11<00:00,  9.42it/s]Capturing num tokens (num_tokens=32 avail_mem=84.27 GB):  88%|████████▊ | 51/58 [00:11<00:00,  9.21it/s]Capturing num tokens (num_tokens=28 avail_mem=84.27 GB):  88%|████████▊ | 51/58 [00:11<00:00,  9.21it/s]

    Capturing num tokens (num_tokens=28 avail_mem=84.27 GB):  90%|████████▉ | 52/58 [00:11<00:00,  9.16it/s]Capturing num tokens (num_tokens=24 avail_mem=85.03 GB):  90%|████████▉ | 52/58 [00:11<00:00,  9.16it/s]Capturing num tokens (num_tokens=24 avail_mem=85.03 GB):  91%|█████████▏| 53/58 [00:11<00:00,  9.19it/s]Capturing num tokens (num_tokens=20 avail_mem=84.32 GB):  91%|█████████▏| 53/58 [00:11<00:00,  9.19it/s]

    Capturing num tokens (num_tokens=20 avail_mem=84.32 GB):  93%|█████████▎| 54/58 [00:11<00:00,  9.18it/s]Capturing num tokens (num_tokens=16 avail_mem=84.23 GB):  93%|█████████▎| 54/58 [00:11<00:00,  9.18it/s]Capturing num tokens (num_tokens=12 avail_mem=85.02 GB):  93%|█████████▎| 54/58 [00:11<00:00,  9.18it/s]Capturing num tokens (num_tokens=12 avail_mem=85.02 GB):  97%|█████████▋| 56/58 [00:11<00:00,  9.68it/s]Capturing num tokens (num_tokens=8 avail_mem=85.01 GB):  97%|█████████▋| 56/58 [00:11<00:00,  9.68it/s] 

    Capturing num tokens (num_tokens=8 avail_mem=85.01 GB):  98%|█████████▊| 57/58 [00:12<00:00,  9.51it/s]Capturing num tokens (num_tokens=4 avail_mem=84.35 GB):  98%|█████████▊| 57/58 [00:12<00:00,  9.51it/s]Capturing num tokens (num_tokens=4 avail_mem=84.35 GB): 100%|██████████| 58/58 [00:12<00:00,  9.22it/s]Capturing num tokens (num_tokens=4 avail_mem=84.35 GB): 100%|██████████| 58/58 [00:12<00:00,  4.77it/s]


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


<strong style='color: #00008B;'>reasoing_content: Okay, so the user is asking for the current date and time in New York and the weather there. I need to figure out how to respond using the allowed functions.<br><br>First, I should use the get_current_date function. The function requires a timezone parameter. Since the user is in New York, I'll use 'America/New_York' as the value. I'll format it like <function=current_function>{parameters}</function>, replacing current_function with get_current_date and parameters with the timezone.<br><br>Next, for the weather, I'll use get_current_weather. It needs city, state, and unit. The city is New York, the state is NY, and I'll choose Fahrenheit since the user didn't specify otherwise. So the parameters object will have these three keys with their respective values.<br><br>I have to make sure each function call is on its own line and properly formatted. Also, I need to include the sources in the response. For the date, the source is the function's documentation, and for the weather, it's the same function's description.<br><br>Putting it all together, I'll structure the response with each function call on a separate line, adding the necessary JSON parameters, and include the sources at the end.<br><br><br>content: <function=get_current_date>{  <br>  "timezone": "America/New_York"  <br>}</function>  <br><function=get_current_weather>{  <br>  "city": "New York",  <br>  "state": "NY",  <br>  "unit": "fahrenheit"  <br>}</function>  <br>Sources:  <br>- get_current_date documentation: https://docs.example.com/get_current_date  <br>- get_current_weather documentation: https://docs.example.com/get_current_weather</strong>


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

    {'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. I know that Paris is the capital, so that\'s straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I\'m not sure of the exact number. I think it\'s around 2 million, but I should verify that.\n\nWait, maybe I should check the latest statistics. I recall that in recent years, Paris has seen a slight increase. Let me think... I believe the population is approximately 2,150,000 as of 2023. That seems about right, but I\'m not 100% certain. I should make sure to present this information accurately.\n\nNext, I need to structure this into a JSON format. JSON requires key-value pairs, so I\'ll create an object with keys like "city", "population", and maybe "country" for context. The city is Paris, the population is 2,150,000, and the country is France.\n\nI should also consider if the user might need more details, like the exact year of the population figure. Including that could be helpful, so I\'ll add "year": 2023. That way, the user knows the data is up to date.\n\nPutting it all together, the JSON should look clean and well-structured. I\'ll make sure the syntax is correct, with proper commas and quotation marks. No markdown, just plain JSON.\n\nI think that\'s all. The user probably just needs the information quickly, so keeping it concise is key. I\'ll present the JSON without any extra fluff.\n</think>{\n\n"name": "Paris",\n"population": 21500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1265, 1477, 279, 1429, 3213, 7042, 821, 13, 358, 6099, 429, 279, 7042, 315, 12095, 702, 1012, 7826, 11, 714, 358, 2776, 537, 2704, 315, 279, 4734, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 10146, 429, 382, 14190, 11, 7196, 358, 1265, 1779, 279, 5535, 13142, 13, 358, 19091, 429, 304, 3213, 1635, 11, 12095, 702, 3884, 264, 8112, 5263, 13, 6771, 752, 1744, 1112, 358, 4411, 279, 7042, 374, 13187, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 438, 315, 220, 17, 15, 17, 18, 13, 2938, 4977, 911, 1290, 11, 714, 358, 2776, 537, 220, 16, 15, 15, 4, 3654, 13, 358, 1265, 1281, 2704, 311, 3042, 419, 1995, 29257, 382, 5847, 11, 358, 1184, 311, 5944, 419, 1119, 264, 4718, 3561, 13, 4718, 7460, 1376, 19083, 13530, 11, 773, 358, 3278, 1855, 458, 1633, 448, 6894, 1075, 330, 8926, 497, 330, 44441, 497, 323, 7196, 330, 11141, 1, 369, 2266, 13, 576, 3283, 374, 12095, 11, 279, 7042, 374, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 11, 323, 279, 3146, 374, 9625, 382, 40, 1265, 1083, 2908, 421, 279, 1196, 2578, 1184, 803, 3565, 11, 1075, 279, 4734, 1042, 315, 279, 7042, 7071, 13, 55121, 429, 1410, 387, 10950, 11, 773, 358, 3278, 912, 330, 3157, 788, 220, 17, 15, 17, 18, 13, 2938, 1616, 11, 279, 1196, 8788, 279, 821, 374, 705, 311, 2400, 382, 97904, 432, 678, 3786, 11, 279, 4718, 1265, 1401, 4240, 323, 1632, 12, 51143, 13, 358, 3278, 1281, 2704, 279, 19482, 374, 4396, 11, 448, 6169, 76602, 323, 54231, 15423, 13, 2308, 50494, 11, 1101, 14396, 4718, 382, 40, 1744, 429, 594, 678, 13, 576, 1196, 4658, 1101, 3880, 279, 1995, 6157, 11, 773, 10282, 432, 63594, 374, 1376, 13, 358, 3278, 3042, 279, 4718, 2041, 894, 4960, 1320, 1362, 624, 151649, 4257, 1, 606, 788, 330, 59604, 756, 1, 44441, 788, 220, 17, 16, 20, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15], 'meta_info': {'id': '76946d08bbfc4749b533a488a8096d78', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': {'device': 1, 'host': 0}, 'dp_rank': None, 'e2e_latency': 19.127202042844146, 'response_sent_to_client_ts': 1775293167.1378512}}



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


<strong style='color: #00008B;'>{'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. I know that Paris is the capital, so that\'s straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I\'m not exactly sure of the current number. I think it\'s around 2 million, but I should verify that.\n\nWait, maybe I should check some reliable sources to confirm the population. I recall that the population figures can vary depending on the source and the year. For example, the 2020 census might have a slightly different number than the 2021 estimate. I think the population was around 2,165,000 in 2021, but I\'m not 100% certain. I should make sure to use the most accurate and up-to-date information.\n\nAlso, the user wants the information in JSON format. JSON is a data interchange format, so I\'ll need to structure the data accordingly. I should include the city name, population, and maybe the year of the data. It\'s important to present the information clearly and accurately, so I\'ll double-check the numbers to avoid any mistakes.\n\nI should also consider if there are any other relevant details the user might find useful, like the area of the city or some key facts about it. But since the user specifically asked for population, I\'ll focus on that. Maybe adding a note about the population figure being approximate would be helpful, just in case.\n\nPutting it all together, I\'ll structure the JSON with the city name, population, and the year. I\'ll make sure the syntax is correct, using quotation marks and commas appropriately. I\'ll also keep the language clear and straightforward so that the user can easily understand the information.\n\nFinally, I\'ll review the JSON to ensure there are no errors and that the data is accurate. This way, the user gets a reliable and well-formatted response to their query.\n</think>{"name": "Paris", "population": 2165000}', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1265, 1477, 279, 1429, 3213, 7042, 821, 13, 358, 6099, 429, 279, 7042, 315, 12095, 702, 1012, 7826, 11, 714, 358, 2776, 537, 6896, 2704, 315, 279, 1482, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 10146, 429, 382, 14190, 11, 7196, 358, 1265, 1779, 1045, 14720, 8173, 311, 7683, 279, 7042, 13, 358, 19091, 429, 279, 7042, 12396, 646, 13289, 11649, 389, 279, 2530, 323, 279, 1042, 13, 1752, 3110, 11, 279, 220, 17, 15, 17, 15, 43602, 2578, 614, 264, 10078, 2155, 1372, 1091, 279, 220, 17, 15, 17, 16, 16045, 13, 358, 1744, 279, 7042, 572, 2163, 220, 17, 11, 16, 21, 20, 11, 15, 15, 15, 304, 220, 17, 15, 17, 16, 11, 714, 358, 2776, 537, 220, 16, 15, 15, 4, 3654, 13, 358, 1265, 1281, 2704, 311, 990, 279, 1429, 13382, 323, 705, 4686, 18413, 1995, 382, 13394, 11, 279, 1196, 6801, 279, 1995, 304, 4718, 3561, 13, 4718, 374, 264, 821, 51263, 3561, 11, 773, 358, 3278, 1184, 311, 5944, 279, 821, 27079, 13, 358, 1265, 2924, 279, 3283, 829, 11, 7042, 11, 323, 7196, 279, 1042, 315, 279, 821, 13, 1084, 594, 2989, 311, 3042, 279, 1995, 9355, 323, 29257, 11, 773, 358, 3278, 1990, 15934, 279, 5109, 311, 5648, 894, 20643, 382, 40, 1265, 1083, 2908, 421, 1052, 525, 894, 1008, 9760, 3565, 279, 1196, 2578, 1477, 5390, 11, 1075, 279, 3082, 315, 279, 3283, 476, 1045, 1376, 13064, 911, 432, 13, 1988, 2474, 279, 1196, 11689, 4588, 369, 7042, 11, 358, 3278, 5244, 389, 429, 13, 10696, 7842, 264, 5185, 911, 279, 7042, 7071, 1660, 44868, 1035, 387, 10950, 11, 1101, 304, 1142, 382, 97904, 432, 678, 3786, 11, 358, 3278, 5944, 279, 4718, 448, 279, 3283, 829, 11, 7042, 11, 323, 279, 1042, 13, 358, 3278, 1281, 2704, 279, 19482, 374, 4396, 11, 1667, 54231, 15423, 323, 76602, 34901, 13, 358, 3278, 1083, 2506, 279, 4128, 2797, 323, 30339, 773, 429, 279, 1196, 646, 6707, 3535, 279, 1995, 382, 23949, 11, 358, 3278, 3395, 279, 4718, 311, 5978, 1052, 525, 902, 5975, 323, 429, 279, 821, 374, 13382, 13, 1096, 1616, 11, 279, 1196, 5221, 264, 14720, 323, 1632, 8460, 12127, 2033, 311, 862, 3239, 624, 151649, 4913, 606, 788, 330, 59604, 497, 330, 44441, 788, 220, 17, 16, 21, 20, 15, 15, 15, 92, 151643], 'meta_info': {'id': 'dbdcafe20dbb4ee9892b5139a955339c', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 447, 'cached_tokens': 22, 'cached_tokens_details': {'device': 22, 'host': 0}, 'dp_rank': None, 'e2e_latency': 4.553588312584907, 'response_sent_to_client_ts': 1775293171.704066}}</strong>


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

    [{'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': 'd4f3361589a24cd9ab7331d485e7761b', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': {'device': 10, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.15756718069314957, 'response_sent_to_client_ts': 1775293171.907263}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': 'ff90135225534813a56136c85c96913d', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': {'device': 10, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.15748848393559456, 'response_sent_to_client_ts': 1775293171.907277}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '6586a118bc0b4259bf68c6b762cbff28', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': {'device': 10, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.15728416107594967, 'response_sent_to_client_ts': 1775293171.9072819}}]


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

    {'text': ' France, and the \n\\( n \\)  \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\(', 'output_ids': [9625, 11, 323, 279, 220, 198, 44292, 308, 1124, 8, 220, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767], 'meta_info': {'id': 'c7541563c4844c39b295ed3433b93e25', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 6, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': {'device': 1, 'host': 0}, 'dp_rank': None, 'e2e_latency': 19.65728255501017, 'response_sent_to_client_ts': 1775293191.5747893}}


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


<strong style='color: #00008B;'>{'text': 'Alright, the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. That\'s straightforward: Paris.\n\nNext, I should collect the relevant data. The population figure is essential, but I also need other key information like the area, administrative divisions, and notable landmarks.\n\nFor the population, I remember that Paris has a large population, but it\'s not exact. The figure I have is around 2.1 million people as of 2023. I should mention that the number can vary slightly each year.\n\nThen, the area. Paris is known for itsÎle de île (the islands), which make up a significant part of the city. I think there are 13 main islands, so I\'ll include that.\n\nAdministrative divisions are next. Paris is a grand CdI (Commune d\' administrator je�数), and it\'s the most populous in France. It has several arrondissements, or districts. I know these are numbered and there are 20 of them, including(validation typo).\n\nLandmarks are important too. The Eiffel Tower is iconic, and the Arc de Triomphe is another famous spot. Including these will make the information more comprehensive.\n\nI should structure this information in a JSON format. The keys should be clear and organized. Maybe start with "City" and "Population", then "Area", "Administrative_Divisions", and "Landmarks".\n\nI need to make sure the JSON syntax is correct to avoid errors. Each key should be in quotes, and the values appropriately formatted, especially the population with commas.\n\nPutting it all together, I\'ll present the JSON with each piece of information. I\'ll also add a brief description to explain each part, making it clear for the user.\n\nFinally, I\'ll wrap it up with a friendly note asking if they need more details. That should cover everything the user asked for and potentially address any hidden needs they might have.\n</think>\n\nCertainly! Here is the information and population of the capital of France, Paris, in JSON format:\n\n```json\n{\n  "City": "Paris",\n  "Population": "Approximately 2,150,000 (2023 estimate)",\n  "Area": "105.5 square kilometers",\n  "Administrative_Divisions": {\n    "Type": "Grand CdI (Commune d\' administration je数目)",\n    "Population": "2,143,743 in 2022",\n    "Number_of_Arrondissements": "20 Arrondissements",\n    "Main_Arrondissement": "巴黎市 (巴黎市, Arrondissement 1)"\n  },\n  "Landmarks": {\n    "Eiffel_Tower": true,\n    "Arc_de_Triomphe": true,\n    "Notre-Dame": true\n  }\n}\n```\n\nThis JSON includes the population, area, administrative divisions, and notable landmarks of Paris. Let me know if you need more details!', 'output_ids': [71486, 11, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 2938, 594, 30339, 25, 12095, 382, 5847, 11, 358, 1265, 6530, 279, 9760, 821, 13, 576, 7042, 7071, 374, 7565, 11, 714, 358, 1083, 1184, 1008, 1376, 1995, 1075, 279, 3082, 11, 22707, 36501, 11, 323, 27190, 59924, 382, 2461, 279, 7042, 11, 358, 6099, 429, 12095, 702, 264, 3460, 7042, 11, 714, 432, 594, 537, 4734, 13, 576, 7071, 358, 614, 374, 2163, 220, 17, 13, 16, 3526, 1251, 438, 315, 220, 17, 15, 17, 18, 13, 358, 1265, 6286, 429, 279, 1372, 646, 13289, 10078, 1817, 1042, 382, 12209, 11, 279, 3082, 13, 12095, 374, 3881, 369, 1181, 71807, 273, 409, 14364, 273, 320, 1782, 29000, 701, 892, 1281, 705, 264, 5089, 949, 315, 279, 3283, 13, 358, 1744, 1052, 525, 220, 16, 18, 1887, 29000, 11, 773, 358, 3278, 2924, 429, 382, 61263, 1388, 36501, 525, 1790, 13, 12095, 374, 264, 6662, 83990, 40, 320, 17977, 2886, 294, 6, 28093, 4759, 27260, 701, 323, 432, 594, 279, 1429, 94451, 304, 9625, 13, 1084, 702, 3807, 2890, 2111, 15398, 1368, 11, 476, 26438, 13, 358, 1414, 1493, 525, 48826, 323, 1052, 525, 220, 17, 15, 315, 1105, 11, 2670, 79413, 85105, 3593, 41957, 15544, 525, 2989, 2238, 13, 576, 468, 3092, 301, 21938, 374, 26277, 11, 323, 279, 19689, 409, 12359, 14435, 383, 374, 2441, 11245, 7702, 13, 55121, 1493, 686, 1281, 279, 1995, 803, 15817, 382, 40, 1265, 5944, 419, 1995, 304, 264, 4718, 3561, 13, 576, 6894, 1265, 387, 2797, 323, 16645, 13, 10696, 1191, 448, 330, 12730, 1, 323, 330, 53371, 497, 1221, 330, 8726, 497, 330, 61263, 1388, 1557, 344, 6805, 497, 323, 330, 41957, 15544, 11436, 40, 1184, 311, 1281, 2704, 279, 4718, 19482, 374, 4396, 311, 5648, 5975, 13, 8886, 1376, 1265, 387, 304, 17194, 11, 323, 279, 2750, 34901, 23126, 11, 5310, 279, 7042, 448, 76602, 382, 97904, 432, 678, 3786, 11, 358, 3278, 3042, 279, 4718, 448, 1817, 6573, 315, 1995, 13, 358, 3278, 1083, 912, 264, 9814, 4008, 311, 10339, 1817, 949, 11, 3259, 432, 2797, 369, 279, 1196, 382, 23949, 11, 358, 3278, 15061, 432, 705, 448, 264, 11657, 5185, 10161, 421, 807, 1184, 803, 3565, 13, 2938, 1265, 3421, 4297, 279, 1196, 4588, 369, 323, 13581, 2621, 894, 8177, 3880, 807, 2578, 614, 624, 151649, 271, 95456, 0, 5692, 374, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 11, 12095, 11, 304, 4718, 3561, 1447, 73594, 2236, 198, 515, 220, 330, 12730, 788, 330, 59604, 756, 220, 330, 53371, 788, 330, 69520, 7108, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 320, 17, 15, 17, 18, 16045, 15752, 220, 330, 8726, 788, 330, 16, 15, 20, 13, 20, 9334, 40568, 756, 220, 330, 61263, 1388, 1557, 344, 6805, 788, 341, 262, 330, 929, 788, 330, 40151, 83990, 40, 320, 17977, 2886, 294, 6, 8567, 4759, 114940, 15752, 262, 330, 53371, 788, 330, 17, 11, 16, 19, 18, 11, 22, 19, 18, 304, 220, 17, 15, 17, 17, 756, 262, 330, 2833, 3575, 1566, 634, 2111, 15398, 1368, 788, 330, 17, 15, 18418, 2111, 15398, 1368, 756, 262, 330, 6202, 1566, 634, 2111, 49653, 788, 330, 106004, 22697, 320, 106004, 22697, 11, 18418, 2111, 49653, 220, 16, 12954, 220, 1153, 220, 330, 41957, 15544, 788, 341, 262, 330, 36, 3092, 301, 1139, 1202, 788, 830, 345, 262, 330, 36809, 2259, 1139, 461, 14435, 383, 788, 830, 345, 262, 330, 2623, 265, 9420, 373, 788, 830, 198, 220, 456, 532, 13874, 19324, 1986, 4718, 5646, 279, 7042, 11, 3082, 11, 22707, 36501, 11, 323, 27190, 59924, 315, 12095, 13, 6771, 752, 1414, 421, 498, 1184, 803, 3565, 0, 151643], 'meta_info': {'id': '66f5920612444a97831b04ad79686aa4', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 640, 'cached_tokens': 22, 'cached_tokens_details': {'device': 22, 'host': 0}, 'dp_rank': None, 'e2e_latency': 5.924084579572082, 'response_sent_to_client_ts': 1775293197.509561}}</strong>



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


    Multi-thread loading shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Multi-thread loading shards:  50% Completed | 1/2 [00:00<00:00,  1.00it/s]

    Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.32s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.27s/it]


    2026-04-04 09:00:15,262 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 09:00:15] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:50,  3.00s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:50,  3.00s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:25,  1.52s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:25,  1.52s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:51,  1.07it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:51,  1.07it/s]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:34,  1.56it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:34,  1.56it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:24,  2.15it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:24,  2.15it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.77it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.77it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.51it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.51it/s]Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.28it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.28it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.16it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.16it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.16it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:06,  6.84it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:06,  6.84it/s]

    Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:06,  6.84it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:04<00:05,  8.35it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:04<00:05,  8.35it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:04<00:05,  8.35it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:05<00:04,  9.88it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:05<00:04,  9.88it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:05<00:04,  9.88it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:03, 11.81it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:03, 11.81it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:03, 11.81it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:05<00:03, 11.81it/s]

    Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 15.09it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 15.09it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 15.09it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 15.09it/s]Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:05<00:02, 15.09it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:05<00:01, 20.22it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:05<00:01, 20.22it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:05<00:01, 20.22it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:05<00:01, 20.22it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:05<00:01, 20.22it/s]

    Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:05<00:01, 24.74it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:05<00:01, 24.74it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:05<00:01, 24.74it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:05<00:01, 24.74it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:05<00:01, 25.27it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:05<00:01, 25.27it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:05<00:01, 25.27it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:05<00:01, 25.27it/s]

    Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:05<00:00, 25.97it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:05<00:00, 25.97it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:05<00:00, 25.97it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:05<00:00, 25.97it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:05<00:00, 26.30it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:05<00:00, 26.30it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:05<00:00, 26.30it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:05<00:00, 26.30it/s]

    Compiling num tokens (num_tokens=192):  64%|██████▍   | 37/58 [00:05<00:00, 26.30it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:05<00:00, 28.08it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:05<00:00, 28.08it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:06<00:00, 28.08it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:06<00:00, 28.08it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:06<00:00, 28.50it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:06<00:00, 28.50it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:06<00:00, 28.50it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:06<00:00, 28.50it/s] 

    Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:06<00:00, 28.50it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:06<00:00, 29.68it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:06<00:00, 29.68it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:06<00:00, 29.68it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:06<00:00, 29.68it/s]Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:06<00:00, 29.68it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:06<00:00, 31.84it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:06<00:00, 31.84it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:06<00:00, 31.84it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:06<00:00, 31.84it/s]

    Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:06<00:00, 31.84it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:06<00:00, 33.84it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:06<00:00, 33.84it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:06<00:00, 33.84it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:06<00:00,  9.00it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=104.59 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=104.59 GB):   2%|▏         | 1/58 [00:00<00:17,  3.19it/s]Capturing num tokens (num_tokens=7680 avail_mem=122.21 GB):   2%|▏         | 1/58 [00:00<00:17,  3.19it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=122.21 GB):   3%|▎         | 2/58 [00:00<00:18,  2.98it/s]Capturing num tokens (num_tokens=7168 avail_mem=122.21 GB):   3%|▎         | 2/58 [00:00<00:18,  2.98it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=122.21 GB):   5%|▌         | 3/58 [00:00<00:16,  3.42it/s]Capturing num tokens (num_tokens=6656 avail_mem=122.22 GB):   5%|▌         | 3/58 [00:00<00:16,  3.42it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=122.22 GB):   7%|▋         | 4/58 [00:01<00:14,  3.82it/s]Capturing num tokens (num_tokens=6144 avail_mem=122.22 GB):   7%|▋         | 4/58 [00:01<00:14,  3.82it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=122.22 GB):   9%|▊         | 5/58 [00:01<00:12,  4.14it/s]Capturing num tokens (num_tokens=5632 avail_mem=122.22 GB):   9%|▊         | 5/58 [00:01<00:12,  4.14it/s]Capturing num tokens (num_tokens=5632 avail_mem=122.22 GB):  10%|█         | 6/58 [00:01<00:11,  4.57it/s]Capturing num tokens (num_tokens=5120 avail_mem=122.22 GB):  10%|█         | 6/58 [00:01<00:11,  4.57it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=122.22 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.97it/s]Capturing num tokens (num_tokens=4608 avail_mem=122.23 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.97it/s]Capturing num tokens (num_tokens=4608 avail_mem=122.23 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.49it/s]Capturing num tokens (num_tokens=4096 avail_mem=122.23 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.49it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=122.23 GB):  16%|█▌        | 9/58 [00:01<00:08,  6.01it/s]Capturing num tokens (num_tokens=3840 avail_mem=122.24 GB):  16%|█▌        | 9/58 [00:01<00:08,  6.01it/s]Capturing num tokens (num_tokens=3840 avail_mem=122.24 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.48it/s]Capturing num tokens (num_tokens=3584 avail_mem=122.24 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.48it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=122.24 GB):  19%|█▉        | 11/58 [00:02<00:06,  7.03it/s]Capturing num tokens (num_tokens=3328 avail_mem=122.24 GB):  19%|█▉        | 11/58 [00:02<00:06,  7.03it/s]Capturing num tokens (num_tokens=3328 avail_mem=122.24 GB):  21%|██        | 12/58 [00:02<00:06,  7.61it/s]Capturing num tokens (num_tokens=3072 avail_mem=122.23 GB):  21%|██        | 12/58 [00:02<00:06,  7.61it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=122.23 GB):  22%|██▏       | 13/58 [00:02<00:07,  6.42it/s]Capturing num tokens (num_tokens=2816 avail_mem=122.24 GB):  22%|██▏       | 13/58 [00:02<00:07,  6.42it/s]Capturing num tokens (num_tokens=2560 avail_mem=122.24 GB):  22%|██▏       | 13/58 [00:02<00:07,  6.42it/s]Capturing num tokens (num_tokens=2560 avail_mem=122.24 GB):  26%|██▌       | 15/58 [00:02<00:05,  8.19it/s]Capturing num tokens (num_tokens=2304 avail_mem=122.24 GB):  26%|██▌       | 15/58 [00:02<00:05,  8.19it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=122.23 GB):  26%|██▌       | 15/58 [00:02<00:05,  8.19it/s]Capturing num tokens (num_tokens=2048 avail_mem=122.23 GB):  29%|██▉       | 17/58 [00:02<00:04,  9.90it/s]Capturing num tokens (num_tokens=1792 avail_mem=122.24 GB):  29%|██▉       | 17/58 [00:02<00:04,  9.90it/s]Capturing num tokens (num_tokens=1536 avail_mem=122.23 GB):  29%|██▉       | 17/58 [00:02<00:04,  9.90it/s]Capturing num tokens (num_tokens=1536 avail_mem=122.23 GB):  33%|███▎      | 19/58 [00:02<00:03, 11.83it/s]Capturing num tokens (num_tokens=1280 avail_mem=122.23 GB):  33%|███▎      | 19/58 [00:02<00:03, 11.83it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=122.23 GB):  33%|███▎      | 19/58 [00:02<00:03, 11.83it/s]Capturing num tokens (num_tokens=960 avail_mem=122.23 GB):  33%|███▎      | 19/58 [00:03<00:03, 11.83it/s] Capturing num tokens (num_tokens=960 avail_mem=122.23 GB):  38%|███▊      | 22/58 [00:03<00:02, 15.22it/s]Capturing num tokens (num_tokens=896 avail_mem=122.22 GB):  38%|███▊      | 22/58 [00:03<00:02, 15.22it/s]Capturing num tokens (num_tokens=832 avail_mem=122.22 GB):  38%|███▊      | 22/58 [00:03<00:02, 15.22it/s]Capturing num tokens (num_tokens=768 avail_mem=122.22 GB):  38%|███▊      | 22/58 [00:03<00:02, 15.22it/s]Capturing num tokens (num_tokens=768 avail_mem=122.22 GB):  43%|████▎     | 25/58 [00:03<00:01, 18.41it/s]Capturing num tokens (num_tokens=704 avail_mem=122.21 GB):  43%|████▎     | 25/58 [00:03<00:01, 18.41it/s]

    Capturing num tokens (num_tokens=640 avail_mem=122.21 GB):  43%|████▎     | 25/58 [00:03<00:01, 18.41it/s]Capturing num tokens (num_tokens=576 avail_mem=122.21 GB):  43%|████▎     | 25/58 [00:03<00:01, 18.41it/s]Capturing num tokens (num_tokens=512 avail_mem=122.20 GB):  43%|████▎     | 25/58 [00:03<00:01, 18.41it/s]Capturing num tokens (num_tokens=512 avail_mem=122.20 GB):  50%|█████     | 29/58 [00:03<00:01, 22.34it/s]Capturing num tokens (num_tokens=480 avail_mem=122.20 GB):  50%|█████     | 29/58 [00:03<00:01, 22.34it/s]Capturing num tokens (num_tokens=448 avail_mem=122.20 GB):  50%|█████     | 29/58 [00:03<00:01, 22.34it/s]Capturing num tokens (num_tokens=416 avail_mem=122.19 GB):  50%|█████     | 29/58 [00:03<00:01, 22.34it/s]Capturing num tokens (num_tokens=384 avail_mem=122.19 GB):  50%|█████     | 29/58 [00:03<00:01, 22.34it/s]

    Capturing num tokens (num_tokens=384 avail_mem=122.19 GB):  57%|█████▋    | 33/58 [00:03<00:00, 25.89it/s]Capturing num tokens (num_tokens=352 avail_mem=122.18 GB):  57%|█████▋    | 33/58 [00:03<00:00, 25.89it/s]Capturing num tokens (num_tokens=320 avail_mem=122.18 GB):  57%|█████▋    | 33/58 [00:03<00:00, 25.89it/s]Capturing num tokens (num_tokens=288 avail_mem=122.18 GB):  57%|█████▋    | 33/58 [00:03<00:00, 25.89it/s]Capturing num tokens (num_tokens=256 avail_mem=122.17 GB):  57%|█████▋    | 33/58 [00:03<00:00, 25.89it/s]Capturing num tokens (num_tokens=256 avail_mem=122.17 GB):  64%|██████▍   | 37/58 [00:03<00:00, 29.08it/s]Capturing num tokens (num_tokens=240 avail_mem=122.17 GB):  64%|██████▍   | 37/58 [00:03<00:00, 29.08it/s]Capturing num tokens (num_tokens=224 avail_mem=122.17 GB):  64%|██████▍   | 37/58 [00:03<00:00, 29.08it/s]Capturing num tokens (num_tokens=208 avail_mem=122.16 GB):  64%|██████▍   | 37/58 [00:03<00:00, 29.08it/s]Capturing num tokens (num_tokens=192 avail_mem=122.16 GB):  64%|██████▍   | 37/58 [00:03<00:00, 29.08it/s]

    Capturing num tokens (num_tokens=192 avail_mem=122.16 GB):  71%|███████   | 41/58 [00:03<00:00, 31.70it/s]Capturing num tokens (num_tokens=176 avail_mem=122.15 GB):  71%|███████   | 41/58 [00:03<00:00, 31.70it/s]Capturing num tokens (num_tokens=160 avail_mem=122.15 GB):  71%|███████   | 41/58 [00:03<00:00, 31.70it/s]Capturing num tokens (num_tokens=144 avail_mem=122.15 GB):  71%|███████   | 41/58 [00:03<00:00, 31.70it/s]Capturing num tokens (num_tokens=128 avail_mem=122.16 GB):  71%|███████   | 41/58 [00:03<00:00, 31.70it/s]Capturing num tokens (num_tokens=128 avail_mem=122.16 GB):  78%|███████▊  | 45/58 [00:03<00:00, 33.58it/s]Capturing num tokens (num_tokens=112 avail_mem=122.15 GB):  78%|███████▊  | 45/58 [00:03<00:00, 33.58it/s]Capturing num tokens (num_tokens=96 avail_mem=122.15 GB):  78%|███████▊  | 45/58 [00:03<00:00, 33.58it/s] Capturing num tokens (num_tokens=80 avail_mem=122.14 GB):  78%|███████▊  | 45/58 [00:03<00:00, 33.58it/s]Capturing num tokens (num_tokens=64 avail_mem=122.14 GB):  78%|███████▊  | 45/58 [00:03<00:00, 33.58it/s]

    Capturing num tokens (num_tokens=64 avail_mem=122.14 GB):  84%|████████▍ | 49/58 [00:03<00:00, 34.35it/s]Capturing num tokens (num_tokens=48 avail_mem=122.14 GB):  84%|████████▍ | 49/58 [00:03<00:00, 34.35it/s]Capturing num tokens (num_tokens=32 avail_mem=122.13 GB):  84%|████████▍ | 49/58 [00:03<00:00, 34.35it/s]Capturing num tokens (num_tokens=28 avail_mem=122.13 GB):  84%|████████▍ | 49/58 [00:03<00:00, 34.35it/s]Capturing num tokens (num_tokens=24 avail_mem=122.13 GB):  84%|████████▍ | 49/58 [00:03<00:00, 34.35it/s]Capturing num tokens (num_tokens=24 avail_mem=122.13 GB):  91%|█████████▏| 53/58 [00:03<00:00, 35.54it/s]Capturing num tokens (num_tokens=20 avail_mem=122.12 GB):  91%|█████████▏| 53/58 [00:03<00:00, 35.54it/s]Capturing num tokens (num_tokens=16 avail_mem=122.12 GB):  91%|█████████▏| 53/58 [00:03<00:00, 35.54it/s]Capturing num tokens (num_tokens=12 avail_mem=122.12 GB):  91%|█████████▏| 53/58 [00:03<00:00, 35.54it/s]Capturing num tokens (num_tokens=8 avail_mem=122.11 GB):  91%|█████████▏| 53/58 [00:03<00:00, 35.54it/s] 

    Capturing num tokens (num_tokens=8 avail_mem=122.11 GB):  98%|█████████▊| 57/58 [00:04<00:00, 36.49it/s]Capturing num tokens (num_tokens=4 avail_mem=122.11 GB):  98%|█████████▊| 57/58 [00:04<00:00, 36.49it/s]Capturing num tokens (num_tokens=4 avail_mem=122.11 GB): 100%|██████████| 58/58 [00:04<00:00, 14.33it/s]


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
    
    Generated text: Alright, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down. 
    
    First, I need to identify the capital of France, which I know is Paris. That's straightforward. Next, the user wants the population data. I should verify the current population number. From what I recall, Paris has a population around 2 million. I think I saw a recent estimate saying approximately 2,150,000 people. 
    
    Now, the user specified the format should be JSON. JSON requires a well-structured data format with key-value pairs. So, I'll need to create a JSON object with keys like "city", "population". I'll make sure the population number is included as a numerical value, not a string, so it's clear and can be easily used if needed.
    
    I should also consider the response structure. Maybe include additional information about the city, like its geographical location or key landmarks, to make the JSON more informative. But the user only asked for the capital and its population, so I should keep it concise unless they ask for more details.
    
    I'll format the JSON properly, using double quotes for strings and making sure commas are correctly placed, especially after the last item in the object. No trailing commas to avoid syntax errors.
    
    Finally, I'll present the JSON in a clear manner, ensuring it's easy to read and understand. I'll double-check the population number for accuracy to provide the most up-to-date and reliable information.
    </think>
    
    Here is the information and population of the capital of France in JSON format:
    
    ```json
    {
      "city": "Paris",
      "population": 2150000
    }
    ```



```python
llm.shutdown()
```
