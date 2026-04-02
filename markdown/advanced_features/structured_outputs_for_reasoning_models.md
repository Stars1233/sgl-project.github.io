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
    2026-04-02 06:51:30.953 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:51:30] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 06:51:30.953 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:51:30] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 06:51:30.953 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:51:30] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 06:51:30.953 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:51:30] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 06:51:30.953 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:51:30] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Multi-thread loading shards:  50% Completed | 1/2 [00:01<00:01,  1.21s/it]

    Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.31s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.30s/it]


    2026-04-02 06:51:38,122 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 06:51:38] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:01,  3.19s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:01,  3.19s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:30,  1.61s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:30,  1.61s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:53,  1.04it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:53,  1.04it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:35,  1.53it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:35,  1.53it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:25,  2.10it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:25,  2.10it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:19,  2.71it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:19,  2.71it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.44it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.44it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.22it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.22it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.10it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.10it/s]

    Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.10it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:06,  6.80it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:06,  6.80it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:05<00:06,  6.80it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:05<00:05,  8.33it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:05<00:05,  8.33it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:05<00:05,  8.33it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:05<00:04,  9.94it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:05<00:04,  9.94it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:05<00:04,  9.94it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:03, 11.88it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:03, 11.88it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:03, 11.88it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:05<00:03, 11.88it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 15.19it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 15.19it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 15.19it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 15.19it/s]

    Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:05<00:02, 15.19it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:05<00:01, 20.36it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:05<00:01, 20.36it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:05<00:01, 20.36it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:05<00:01, 20.36it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:05<00:01, 20.36it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:05<00:01, 22.62it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:05<00:01, 22.62it/s]

    Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:05<00:01, 22.62it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:05<00:01, 22.62it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:05<00:01, 23.24it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:05<00:01, 23.24it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:05<00:01, 23.24it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:05<00:01, 23.24it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:05<00:00, 24.80it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:05<00:00, 24.80it/s]

    Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:05<00:00, 24.80it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:05<00:00, 24.80it/s]Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:06<00:00, 24.80it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:06<00:00, 27.91it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:06<00:00, 27.91it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:06<00:00, 27.91it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:06<00:00, 27.91it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:06<00:00, 27.91it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:06<00:00, 27.91it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:06<00:00, 32.20it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:06<00:00, 32.20it/s]

    Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:06<00:00, 32.20it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:06<00:00, 32.20it/s]Compiling num tokens (num_tokens=96):  74%|███████▍  | 43/58 [00:06<00:00, 32.20it/s] Compiling num tokens (num_tokens=80):  74%|███████▍  | 43/58 [00:06<00:00, 32.20it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:06<00:00, 35.61it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:06<00:00, 35.61it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:06<00:00, 35.61it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:06<00:00, 35.61it/s]Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:06<00:00, 35.61it/s]Compiling num tokens (num_tokens=24):  83%|████████▎ | 48/58 [00:06<00:00, 35.61it/s]Compiling num tokens (num_tokens=20):  83%|████████▎ | 48/58 [00:06<00:00, 35.61it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:06<00:00, 41.21it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:06<00:00, 41.21it/s]

    Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:06<00:00, 41.21it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:06<00:00, 41.21it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:06<00:00, 41.21it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:06<00:00,  9.00it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=101.38 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=101.38 GB):   2%|▏         | 1/58 [00:00<00:16,  3.46it/s]Capturing num tokens (num_tokens=7680 avail_mem=101.35 GB):   2%|▏         | 1/58 [00:00<00:16,  3.46it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=101.35 GB):   3%|▎         | 2/58 [00:00<00:20,  2.77it/s]Capturing num tokens (num_tokens=7168 avail_mem=100.32 GB):   3%|▎         | 2/58 [00:00<00:20,  2.77it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=100.32 GB):   5%|▌         | 3/58 [00:01<00:26,  2.07it/s]Capturing num tokens (num_tokens=6656 avail_mem=100.32 GB):   5%|▌         | 3/58 [00:01<00:26,  2.07it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=100.32 GB):   7%|▋         | 4/58 [00:01<00:27,  1.99it/s]Capturing num tokens (num_tokens=6144 avail_mem=100.32 GB):   7%|▋         | 4/58 [00:01<00:27,  1.99it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=100.32 GB):   9%|▊         | 5/58 [00:02<00:27,  1.94it/s]Capturing num tokens (num_tokens=5632 avail_mem=100.31 GB):   9%|▊         | 5/58 [00:02<00:27,  1.94it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=100.31 GB):  10%|█         | 6/58 [00:02<00:25,  2.05it/s]Capturing num tokens (num_tokens=5120 avail_mem=100.30 GB):  10%|█         | 6/58 [00:02<00:25,  2.05it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=100.30 GB):  12%|█▏        | 7/58 [00:03<00:23,  2.18it/s]Capturing num tokens (num_tokens=4608 avail_mem=100.30 GB):  12%|█▏        | 7/58 [00:03<00:23,  2.18it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=100.30 GB):  14%|█▍        | 8/58 [00:03<00:21,  2.35it/s]Capturing num tokens (num_tokens=4096 avail_mem=100.28 GB):  14%|█▍        | 8/58 [00:03<00:21,  2.35it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=100.28 GB):  16%|█▌        | 9/58 [00:03<00:19,  2.54it/s]Capturing num tokens (num_tokens=3840 avail_mem=100.29 GB):  16%|█▌        | 9/58 [00:03<00:19,  2.54it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=100.29 GB):  17%|█▋        | 10/58 [00:04<00:17,  2.71it/s]Capturing num tokens (num_tokens=3584 avail_mem=100.28 GB):  17%|█▋        | 10/58 [00:04<00:17,  2.71it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=100.28 GB):  19%|█▉        | 11/58 [00:04<00:16,  2.93it/s]Capturing num tokens (num_tokens=3328 avail_mem=100.27 GB):  19%|█▉        | 11/58 [00:04<00:16,  2.93it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=100.27 GB):  21%|██        | 12/58 [00:04<00:14,  3.14it/s]Capturing num tokens (num_tokens=3072 avail_mem=100.28 GB):  21%|██        | 12/58 [00:04<00:14,  3.14it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=100.28 GB):  22%|██▏       | 13/58 [00:05<00:13,  3.34it/s]Capturing num tokens (num_tokens=2816 avail_mem=100.27 GB):  22%|██▏       | 13/58 [00:05<00:13,  3.34it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=100.27 GB):  24%|██▍       | 14/58 [00:05<00:12,  3.62it/s]Capturing num tokens (num_tokens=2560 avail_mem=100.27 GB):  24%|██▍       | 14/58 [00:05<00:12,  3.62it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=100.27 GB):  26%|██▌       | 15/58 [00:05<00:10,  3.93it/s]Capturing num tokens (num_tokens=2304 avail_mem=100.26 GB):  26%|██▌       | 15/58 [00:05<00:10,  3.93it/s]Capturing num tokens (num_tokens=2304 avail_mem=100.26 GB):  28%|██▊       | 16/58 [00:05<00:09,  4.24it/s]Capturing num tokens (num_tokens=2048 avail_mem=100.26 GB):  28%|██▊       | 16/58 [00:05<00:09,  4.24it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=100.26 GB):  29%|██▉       | 17/58 [00:05<00:08,  4.68it/s]Capturing num tokens (num_tokens=1792 avail_mem=100.23 GB):  29%|██▉       | 17/58 [00:05<00:08,  4.68it/s]Capturing num tokens (num_tokens=1792 avail_mem=100.23 GB):  31%|███       | 18/58 [00:05<00:07,  5.21it/s]Capturing num tokens (num_tokens=1536 avail_mem=100.24 GB):  31%|███       | 18/58 [00:05<00:07,  5.21it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=100.24 GB):  33%|███▎      | 19/58 [00:06<00:06,  5.82it/s]Capturing num tokens (num_tokens=1280 avail_mem=100.24 GB):  33%|███▎      | 19/58 [00:06<00:06,  5.82it/s]Capturing num tokens (num_tokens=1024 avail_mem=100.23 GB):  33%|███▎      | 19/58 [00:06<00:06,  5.82it/s]Capturing num tokens (num_tokens=1024 avail_mem=100.23 GB):  36%|███▌      | 21/58 [00:06<00:04,  7.51it/s]Capturing num tokens (num_tokens=960 avail_mem=100.23 GB):  36%|███▌      | 21/58 [00:06<00:04,  7.51it/s] 

    Capturing num tokens (num_tokens=896 avail_mem=100.22 GB):  36%|███▌      | 21/58 [00:06<00:04,  7.51it/s]Capturing num tokens (num_tokens=896 avail_mem=100.22 GB):  40%|███▉      | 23/58 [00:06<00:03,  9.26it/s]Capturing num tokens (num_tokens=832 avail_mem=100.21 GB):  40%|███▉      | 23/58 [00:06<00:03,  9.26it/s]Capturing num tokens (num_tokens=768 avail_mem=100.18 GB):  40%|███▉      | 23/58 [00:06<00:03,  9.26it/s]Capturing num tokens (num_tokens=768 avail_mem=100.18 GB):  43%|████▎     | 25/58 [00:06<00:03, 10.95it/s]Capturing num tokens (num_tokens=704 avail_mem=100.19 GB):  43%|████▎     | 25/58 [00:06<00:03, 10.95it/s]

    Capturing num tokens (num_tokens=640 avail_mem=100.18 GB):  43%|████▎     | 25/58 [00:06<00:03, 10.95it/s]Capturing num tokens (num_tokens=640 avail_mem=100.18 GB):  47%|████▋     | 27/58 [00:06<00:02, 12.13it/s]Capturing num tokens (num_tokens=576 avail_mem=100.17 GB):  47%|████▋     | 27/58 [00:06<00:02, 12.13it/s]Capturing num tokens (num_tokens=512 avail_mem=100.16 GB):  47%|████▋     | 27/58 [00:06<00:02, 12.13it/s]Capturing num tokens (num_tokens=512 avail_mem=100.16 GB):  50%|█████     | 29/58 [00:06<00:02, 12.98it/s]Capturing num tokens (num_tokens=480 avail_mem=100.15 GB):  50%|█████     | 29/58 [00:06<00:02, 12.98it/s]

    Capturing num tokens (num_tokens=448 avail_mem=100.15 GB):  50%|█████     | 29/58 [00:06<00:02, 12.98it/s]Capturing num tokens (num_tokens=416 avail_mem=100.14 GB):  50%|█████     | 29/58 [00:06<00:02, 12.98it/s]Capturing num tokens (num_tokens=416 avail_mem=100.14 GB):  55%|█████▌    | 32/58 [00:06<00:01, 16.02it/s]Capturing num tokens (num_tokens=384 avail_mem=100.13 GB):  55%|█████▌    | 32/58 [00:06<00:01, 16.02it/s]Capturing num tokens (num_tokens=352 avail_mem=100.13 GB):  55%|█████▌    | 32/58 [00:07<00:01, 16.02it/s]

    Capturing num tokens (num_tokens=352 avail_mem=100.13 GB):  59%|█████▊    | 34/58 [00:07<00:01, 14.33it/s]Capturing num tokens (num_tokens=320 avail_mem=100.12 GB):  59%|█████▊    | 34/58 [00:07<00:01, 14.33it/s]Capturing num tokens (num_tokens=288 avail_mem=100.12 GB):  59%|█████▊    | 34/58 [00:07<00:01, 14.33it/s]

    Capturing num tokens (num_tokens=288 avail_mem=100.12 GB):  62%|██████▏   | 36/58 [00:07<00:02,  9.95it/s]Capturing num tokens (num_tokens=256 avail_mem=99.08 GB):  62%|██████▏   | 36/58 [00:07<00:02,  9.95it/s] Capturing num tokens (num_tokens=240 avail_mem=99.08 GB):  62%|██████▏   | 36/58 [00:07<00:02,  9.95it/s]

    Capturing num tokens (num_tokens=240 avail_mem=99.08 GB):  66%|██████▌   | 38/58 [00:07<00:02,  9.90it/s]Capturing num tokens (num_tokens=224 avail_mem=100.14 GB):  66%|██████▌   | 38/58 [00:07<00:02,  9.90it/s]Capturing num tokens (num_tokens=208 avail_mem=100.07 GB):  66%|██████▌   | 38/58 [00:07<00:02,  9.90it/s]Capturing num tokens (num_tokens=208 avail_mem=100.07 GB):  69%|██████▉   | 40/58 [00:07<00:01, 10.88it/s]Capturing num tokens (num_tokens=192 avail_mem=99.25 GB):  69%|██████▉   | 40/58 [00:07<00:01, 10.88it/s] 

    Capturing num tokens (num_tokens=176 avail_mem=99.25 GB):  69%|██████▉   | 40/58 [00:07<00:01, 10.88it/s]Capturing num tokens (num_tokens=176 avail_mem=99.25 GB):  72%|███████▏  | 42/58 [00:07<00:01, 10.63it/s]Capturing num tokens (num_tokens=160 avail_mem=99.25 GB):  72%|███████▏  | 42/58 [00:07<00:01, 10.63it/s]Capturing num tokens (num_tokens=144 avail_mem=100.07 GB):  72%|███████▏  | 42/58 [00:08<00:01, 10.63it/s]

    Capturing num tokens (num_tokens=144 avail_mem=100.07 GB):  76%|███████▌  | 44/58 [00:08<00:01, 11.34it/s]Capturing num tokens (num_tokens=128 avail_mem=100.07 GB):  76%|███████▌  | 44/58 [00:08<00:01, 11.34it/s]Capturing num tokens (num_tokens=112 avail_mem=99.31 GB):  76%|███████▌  | 44/58 [00:08<00:01, 11.34it/s] Capturing num tokens (num_tokens=112 avail_mem=99.31 GB):  79%|███████▉  | 46/58 [00:08<00:01, 11.75it/s]Capturing num tokens (num_tokens=96 avail_mem=99.30 GB):  79%|███████▉  | 46/58 [00:08<00:01, 11.75it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=99.29 GB):  79%|███████▉  | 46/58 [00:08<00:01, 11.75it/s]Capturing num tokens (num_tokens=80 avail_mem=99.29 GB):  83%|████████▎ | 48/58 [00:08<00:00, 11.15it/s]Capturing num tokens (num_tokens=64 avail_mem=100.06 GB):  83%|████████▎ | 48/58 [00:08<00:00, 11.15it/s]Capturing num tokens (num_tokens=48 avail_mem=99.35 GB):  83%|████████▎ | 48/58 [00:08<00:00, 11.15it/s] 

    Capturing num tokens (num_tokens=48 avail_mem=99.35 GB):  86%|████████▌ | 50/58 [00:08<00:00, 11.02it/s]Capturing num tokens (num_tokens=32 avail_mem=99.34 GB):  86%|████████▌ | 50/58 [00:08<00:00, 11.02it/s]Capturing num tokens (num_tokens=28 avail_mem=100.06 GB):  86%|████████▌ | 50/58 [00:08<00:00, 11.02it/s]Capturing num tokens (num_tokens=28 avail_mem=100.06 GB):  90%|████████▉ | 52/58 [00:08<00:00, 11.47it/s]Capturing num tokens (num_tokens=24 avail_mem=99.40 GB):  90%|████████▉ | 52/58 [00:08<00:00, 11.47it/s] 

    Capturing num tokens (num_tokens=20 avail_mem=99.39 GB):  90%|████████▉ | 52/58 [00:08<00:00, 11.47it/s]Capturing num tokens (num_tokens=20 avail_mem=99.39 GB):  93%|█████████▎| 54/58 [00:09<00:00, 11.22it/s]Capturing num tokens (num_tokens=16 avail_mem=100.15 GB):  93%|█████████▎| 54/58 [00:09<00:00, 11.22it/s]Capturing num tokens (num_tokens=12 avail_mem=100.04 GB):  93%|█████████▎| 54/58 [00:09<00:00, 11.22it/s]

    Capturing num tokens (num_tokens=12 avail_mem=100.04 GB):  97%|█████████▋| 56/58 [00:09<00:00, 11.56it/s]Capturing num tokens (num_tokens=8 avail_mem=99.44 GB):  97%|█████████▋| 56/58 [00:09<00:00, 11.56it/s]  Capturing num tokens (num_tokens=4 avail_mem=99.48 GB):  97%|█████████▋| 56/58 [00:09<00:00, 11.56it/s]

    Capturing num tokens (num_tokens=4 avail_mem=99.48 GB): 100%|██████████| 58/58 [00:09<00:00, 10.54it/s]Capturing num tokens (num_tokens=4 avail_mem=99.48 GB): 100%|██████████| 58/58 [00:09<00:00,  6.17it/s]


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


<strong style='color: #00008B;'>reasoing_content: Okay, so the user is in New York and wants the current date and time along with the weather. I need to figure out how to get this information using the provided functions. <br><br>First, I should use the 'get_current_date' function. The parameters required are the timezone, which in this case is 'America/New_York'. I'll structure the function call with that parameter.<br><br>Next, I'll need the weather information. For that, I'll use the 'get_current_weather' function. The parameters required here are city, state, and unit. The city is 'New York', the state is 'NY', and the unit should be 'fahrenheit' since the user didn't specify, but it's a common request.<br><br>I should make sure to format each function call correctly, using the specified JSON structure within the tags. I'll also add the sources where I got the functions from, which are the provided function descriptions.<br><br>Putting it all together, I'll send two separate function calls: one for the date and time, and another for the weather. Each will have the necessary parameters included. I'll make sure to follow the format strictly, keeping each function call on its own line and properly tagging them.<br><br><br>content: <function=get_current_date>{"timezone": "America/New_York"}</function>  <br><function=get_current_weather>{"city": "New York", "state": "NY", "unit": "fahrenheit"}</function>  <br><br>Source:  <br>- Function calls follow the specified format  <br>- Required parameters are specified  <br>- Only one function at a time  <br>- Function calls are on separate lines</strong>


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

    {'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. I know that Paris is the capital, so that\'s straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I\'m not sure of the exact number. I think it\'s around 2 million, but I should verify that.\n\nWait, maybe I should check the latest statistics. I recall that in recent years, Paris has seen a slight increase. Let me think... I believe the population is approximately 2,150,000 as of 2023. That seems about right, but I\'m not 100% certain. I should make sure to present this information accurately.\n\nNext, I need to structure this into a JSON format. JSON requires key-value pairs, so I\'ll create an object with keys like "city", "population", and maybe "country" for context. The city is Paris, the population is 2,150,000, and the country is France.\n\nI should also consider if the user might need more details, like the exact year of the population figure. Including that could be helpful, so I\'ll add "year": 2023. That way, the user knows the data is up to date.\n\nPutting it all together, the JSON should look clean and well-structured. I\'ll make sure the syntax is correct, with proper commas and quotation marks. No markdown, just plain JSON.\n\nI think that\'s all. The user probably just needs the information quickly, so keeping it concise is key. I\'ll present the JSON without any extra fluff.\n</think>{\n\n"name": "Paris",\n"population": 21500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1265, 1477, 279, 1429, 3213, 7042, 821, 13, 358, 6099, 429, 279, 7042, 315, 12095, 702, 1012, 7826, 11, 714, 358, 2776, 537, 2704, 315, 279, 4734, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 10146, 429, 382, 14190, 11, 7196, 358, 1265, 1779, 279, 5535, 13142, 13, 358, 19091, 429, 304, 3213, 1635, 11, 12095, 702, 3884, 264, 8112, 5263, 13, 6771, 752, 1744, 1112, 358, 4411, 279, 7042, 374, 13187, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 438, 315, 220, 17, 15, 17, 18, 13, 2938, 4977, 911, 1290, 11, 714, 358, 2776, 537, 220, 16, 15, 15, 4, 3654, 13, 358, 1265, 1281, 2704, 311, 3042, 419, 1995, 29257, 382, 5847, 11, 358, 1184, 311, 5944, 419, 1119, 264, 4718, 3561, 13, 4718, 7460, 1376, 19083, 13530, 11, 773, 358, 3278, 1855, 458, 1633, 448, 6894, 1075, 330, 8926, 497, 330, 44441, 497, 323, 7196, 330, 11141, 1, 369, 2266, 13, 576, 3283, 374, 12095, 11, 279, 7042, 374, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 11, 323, 279, 3146, 374, 9625, 382, 40, 1265, 1083, 2908, 421, 279, 1196, 2578, 1184, 803, 3565, 11, 1075, 279, 4734, 1042, 315, 279, 7042, 7071, 13, 55121, 429, 1410, 387, 10950, 11, 773, 358, 3278, 912, 330, 3157, 788, 220, 17, 15, 17, 18, 13, 2938, 1616, 11, 279, 1196, 8788, 279, 821, 374, 705, 311, 2400, 382, 97904, 432, 678, 3786, 11, 279, 4718, 1265, 1401, 4240, 323, 1632, 12, 51143, 13, 358, 3278, 1281, 2704, 279, 19482, 374, 4396, 11, 448, 6169, 76602, 323, 54231, 15423, 13, 2308, 50494, 11, 1101, 14396, 4718, 382, 40, 1744, 429, 594, 678, 13, 576, 1196, 4658, 1101, 3880, 279, 1995, 6157, 11, 773, 10282, 432, 63594, 374, 1376, 13, 358, 3278, 3042, 279, 4718, 2041, 894, 4960, 1320, 1362, 624, 151649, 4257, 1, 606, 788, 330, 59604, 756, 1, 44441, 788, 220, 17, 16, 20, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15], 'meta_info': {'id': '121fa2512e6347d2a19e0c13f06fe4f2', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': {'device': 1, 'host': 0}, 'dp_rank': None, 'e2e_latency': 25.417219154071063, 'response_sent_to_client_ts': 1775112768.5244055}}



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


<strong style='color: #00008B;'>{'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. I know that Paris is the capital, so that\'s straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I\'m not exactly sure of the current number. I think it\'s around 2 million, but I should verify that.\n\nWait, maybe I should check some reliable sources to confirm the population. I recall that the population figures can vary depending on the source and the year. For example, the 2020 census might have a slightly different number than the 2021 estimate. I think the population was around 2,165,000 in 2021, but I\'m not 100% certain. I should make sure to use the most accurate and up-to-date information.\n\nAlso, the user wants the information in JSON format. JSON is a data interchange format, so I\'ll need to structure the data accordingly. I should include the city name, population, and maybe the year of the data. It\'s important to present the information clearly and accurately, so I\'ll double-check the numbers to avoid any mistakes.\n\nI should also consider if there are any other relevant details the user might find useful, like the area of the city or some key facts about it. But since the user specifically asked for population, I\'ll focus on that. Maybe adding a note about the population figure being approximate would be helpful, just in case.\n\nPutting it all together, I\'ll structure the JSON with the city name, population, and the year. I\'ll make sure the syntax is correct, using quotation marks and commas appropriately. I\'ll also keep the language clear and straightforward so that the user can easily understand the information.\n\nFinally, I\'ll review the JSON to ensure there are no errors and that the data is accurate. This way, the user gets a reliable and well-formatted response to their query.\n</think>{"name": "Paris", "population": 2165000}', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1265, 1477, 279, 1429, 3213, 7042, 821, 13, 358, 6099, 429, 279, 7042, 315, 12095, 702, 1012, 7826, 11, 714, 358, 2776, 537, 6896, 2704, 315, 279, 1482, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 10146, 429, 382, 14190, 11, 7196, 358, 1265, 1779, 1045, 14720, 8173, 311, 7683, 279, 7042, 13, 358, 19091, 429, 279, 7042, 12396, 646, 13289, 11649, 389, 279, 2530, 323, 279, 1042, 13, 1752, 3110, 11, 279, 220, 17, 15, 17, 15, 43602, 2578, 614, 264, 10078, 2155, 1372, 1091, 279, 220, 17, 15, 17, 16, 16045, 13, 358, 1744, 279, 7042, 572, 2163, 220, 17, 11, 16, 21, 20, 11, 15, 15, 15, 304, 220, 17, 15, 17, 16, 11, 714, 358, 2776, 537, 220, 16, 15, 15, 4, 3654, 13, 358, 1265, 1281, 2704, 311, 990, 279, 1429, 13382, 323, 705, 4686, 18413, 1995, 382, 13394, 11, 279, 1196, 6801, 279, 1995, 304, 4718, 3561, 13, 4718, 374, 264, 821, 51263, 3561, 11, 773, 358, 3278, 1184, 311, 5944, 279, 821, 27079, 13, 358, 1265, 2924, 279, 3283, 829, 11, 7042, 11, 323, 7196, 279, 1042, 315, 279, 821, 13, 1084, 594, 2989, 311, 3042, 279, 1995, 9355, 323, 29257, 11, 773, 358, 3278, 1990, 15934, 279, 5109, 311, 5648, 894, 20643, 382, 40, 1265, 1083, 2908, 421, 1052, 525, 894, 1008, 9760, 3565, 279, 1196, 2578, 1477, 5390, 11, 1075, 279, 3082, 315, 279, 3283, 476, 1045, 1376, 13064, 911, 432, 13, 1988, 2474, 279, 1196, 11689, 4588, 369, 7042, 11, 358, 3278, 5244, 389, 429, 13, 10696, 7842, 264, 5185, 911, 279, 7042, 7071, 1660, 44868, 1035, 387, 10950, 11, 1101, 304, 1142, 382, 97904, 432, 678, 3786, 11, 358, 3278, 5944, 279, 4718, 448, 279, 3283, 829, 11, 7042, 11, 323, 279, 1042, 13, 358, 3278, 1281, 2704, 279, 19482, 374, 4396, 11, 1667, 54231, 15423, 323, 76602, 34901, 13, 358, 3278, 1083, 2506, 279, 4128, 2797, 323, 30339, 773, 429, 279, 1196, 646, 6707, 3535, 279, 1995, 382, 23949, 11, 358, 3278, 3395, 279, 4718, 311, 5978, 1052, 525, 902, 5975, 323, 429, 279, 821, 374, 13382, 13, 1096, 1616, 11, 279, 1196, 5221, 264, 14720, 323, 1632, 8460, 12127, 2033, 311, 862, 3239, 624, 151649, 4913, 606, 788, 330, 59604, 497, 330, 44441, 788, 220, 17, 16, 21, 20, 15, 15, 15, 92, 151643], 'meta_info': {'id': '21fb29cdf2e24849810ee13e24bf4810', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 447, 'cached_tokens': 22, 'cached_tokens_details': {'device': 22, 'host': 0}, 'dp_rank': None, 'e2e_latency': 5.004937909077853, 'response_sent_to_client_ts': 1775112773.5393734}}</strong>


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

    [{'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '8245e6d4d54f48abad58d6211236d0d1', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': {'device': 10, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.13459703791886568, 'response_sent_to_client_ts': 1775112773.7172165}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': 'd7e2aec7b01f40b1b2b405d8a0f5feae', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': {'device': 10, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.13452930701896548, 'response_sent_to_client_ts': 1775112773.71723}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '4fb2d37c5fa44f4787698822f73d7d34', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': {'device': 10, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.13448725501075387, 'response_sent_to_client_ts': 1775112773.7172346}}]


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

    {'text': ' France, and the \n\\( n \\)  \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\(', 'output_ids': [9625, 11, 323, 279, 220, 198, 44292, 308, 1124, 8, 220, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767], 'meta_info': {'id': 'cd0dfe21f49a4c09bf4895901ecdb9fd', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 6, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': {'device': 1, 'host': 0}, 'dp_rank': None, 'e2e_latency': 19.460097449831665, 'response_sent_to_client_ts': 1775112793.1854565}}


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


<strong style='color: #00008B;'>{'text': 'Okay, so I need to find out the information and population of France\'s capital city, which is Paris. Alright, let\'s start by recalling what I know about Paris. I know it\'s the biggest city in France, and the capital. But I\'m not exactly sure about its population. Wait, I think it\'s over 3 million people, but I might be off. Maybe it\'s around 2.1 or something like that? I\'m not certain. \n\nI should probably check the exact population figure. I remember hearing that populations can change over time due to births, deaths, and migration. Europe is undergoing something called an "ageing population," so that might affect Paris\'s population. Also, does Paris have any special status? I think it\'s a comune, which might mean its population isn\'t counted in the same way as larger cities. That could be part of the reason its population is reported differently. \n\nWhat\'s the current year? The information in my memory might be outdated. If it\'s from, say, 2023, but now it\'s 2024, the numbers might have changed a bit. Also, is there any specific demographic trend in Paris? Are there more young people or is it getting pricier for families to live there? These factors can influence the population growth. \n\nWait, I should also consider how data is collected. Population figures can vary depending on the source—government databases, United Nations reports, or recent surveys. It might help to have the exact numbers from a reputable source. Since I\'m just trying to remember, I think the approximate population is around 2.1 to 2.2 million. \n\nIf I were presenting this information, I\'d note that the capital\'s population includes certain characteristics, like its status as a city of cultural significance, which might make it more populous or attracting tourists and expatriates. But the actual figure is probably somewhere close to 2.12 million as of recent estimates. \n\nAlso, considering natural increase, which is the difference between births and deaths, Paris might have a positive or slightly negative natural increase depending on the year. But on average, I think it\'s manageable. \n\nTo summarize, Paris, the capital of France, is home to approximately 2.12 million people, taking into account all factors that might affect its population size. It\'s a significant city, so I guess the numbers are a bit higher than average for a comune, but not as high as other megacities in Europe or elsewhere in the world. \n\nI think that\'s a solid understanding for this context. The population figure is crucial, so I should make sure to include it accurately in the JSON format.\n</think>\n\n```json\n{\n  "capital": "Paris",\n  "population": 2120000\n}\n```', 'output_ids': [32313, 11, 773, 358, 1184, 311, 1477, 700, 279, 1995, 323, 7042, 315, 9625, 594, 6722, 3283, 11, 892, 374, 12095, 13, 97593, 11, 1077, 594, 1191, 553, 88646, 1128, 358, 1414, 911, 12095, 13, 358, 1414, 432, 594, 279, 8538, 3283, 304, 9625, 11, 323, 279, 6722, 13, 1988, 358, 2776, 537, 6896, 2704, 911, 1181, 7042, 13, 13824, 11, 358, 1744, 432, 594, 916, 220, 18, 3526, 1251, 11, 714, 358, 2578, 387, 1007, 13, 10696, 432, 594, 2163, 220, 17, 13, 16, 476, 2494, 1075, 429, 30, 358, 2776, 537, 3654, 13, 4710, 40, 1265, 4658, 1779, 279, 4734, 7042, 7071, 13, 358, 6099, 10778, 429, 21910, 646, 2297, 916, 882, 4152, 311, 65232, 11, 16375, 11, 323, 11906, 13, 4505, 374, 46297, 2494, 2598, 458, 330, 424, 287, 7042, 1335, 773, 429, 2578, 7802, 12095, 594, 7042, 13, 7281, 11, 1558, 12095, 614, 894, 3281, 2639, 30, 358, 1744, 432, 594, 264, 469, 2886, 11, 892, 2578, 3076, 1181, 7042, 4436, 944, 29994, 304, 279, 1852, 1616, 438, 8131, 9720, 13, 2938, 1410, 387, 949, 315, 279, 2874, 1181, 7042, 374, 4961, 21303, 13, 4710, 3838, 594, 279, 1482, 1042, 30, 576, 1995, 304, 847, 4938, 2578, 387, 40526, 13, 1416, 432, 594, 504, 11, 1977, 11, 220, 17, 15, 17, 18, 11, 714, 1431, 432, 594, 220, 17, 15, 17, 19, 11, 279, 5109, 2578, 614, 5497, 264, 2699, 13, 7281, 11, 374, 1052, 894, 3151, 37362, 9149, 304, 12095, 30, 8713, 1052, 803, 3908, 1251, 476, 374, 432, 3709, 548, 292, 1268, 369, 8521, 311, 3887, 1052, 30, 4220, 9363, 646, 10173, 279, 7042, 6513, 13, 4710, 14190, 11, 358, 1265, 1083, 2908, 1246, 821, 374, 14548, 13, 39529, 12396, 646, 13289, 11649, 389, 279, 2530, 2293, 86763, 31806, 11, 3639, 19140, 6682, 11, 476, 3213, 31213, 13, 1084, 2578, 1492, 311, 614, 279, 4734, 5109, 504, 264, 55840, 2530, 13, 8704, 358, 2776, 1101, 4460, 311, 6099, 11, 358, 1744, 279, 44868, 7042, 374, 2163, 220, 17, 13, 16, 311, 220, 17, 13, 17, 3526, 13, 4710, 2679, 358, 1033, 31544, 419, 1995, 11, 358, 4172, 5185, 429, 279, 6722, 594, 7042, 5646, 3654, 17452, 11, 1075, 1181, 2639, 438, 264, 3283, 315, 12752, 25361, 11, 892, 2578, 1281, 432, 803, 94451, 476, 50547, 31653, 323, 1343, 2361, 973, 13, 1988, 279, 5042, 7071, 374, 4658, 14696, 3265, 311, 220, 17, 13, 16, 17, 3526, 438, 315, 3213, 17530, 13, 4710, 13394, 11, 12831, 5810, 5263, 11, 892, 374, 279, 6672, 1948, 65232, 323, 16375, 11, 12095, 2578, 614, 264, 6785, 476, 10078, 8225, 5810, 5263, 11649, 389, 279, 1042, 13, 1988, 389, 5461, 11, 358, 1744, 432, 594, 70028, 13, 4710, 1249, 62079, 11, 12095, 11, 279, 6722, 315, 9625, 11, 374, 2114, 311, 13187, 220, 17, 13, 16, 17, 3526, 1251, 11, 4633, 1119, 2692, 678, 9363, 429, 2578, 7802, 1181, 7042, 1379, 13, 1084, 594, 264, 5089, 3283, 11, 773, 358, 7942, 279, 5109, 525, 264, 2699, 5080, 1091, 5461, 369, 264, 469, 2886, 11, 714, 537, 438, 1550, 438, 1008, 18740, 580, 1361, 304, 4505, 476, 17920, 304, 279, 1879, 13, 4710, 40, 1744, 429, 594, 264, 6437, 8660, 369, 419, 2266, 13, 576, 7042, 7071, 374, 16587, 11, 773, 358, 1265, 1281, 2704, 311, 2924, 432, 29257, 304, 279, 4718, 3561, 624, 151649, 271, 73594, 2236, 198, 515, 220, 330, 65063, 788, 330, 59604, 756, 220, 330, 44441, 788, 220, 17, 16, 17, 15, 15, 15, 15, 198, 532, 73594, 151643], 'meta_info': {'id': 'de2947087fb246458a9f0f8dbba7a126', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 583, 'cached_tokens': 22, 'cached_tokens_details': {'device': 22, 'host': 0}, 'dp_rank': None, 'e2e_latency': 5.49054196709767, 'response_sent_to_client_ts': 1775112798.685491}}</strong>



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


    2026-04-02 06:53:31.369 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:53:31] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 06:53:31.369 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:53:31] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 06:53:31.369 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:53:31] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 06:53:31.369 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:53:31] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 06:53:31.369 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:53:31] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Multi-thread loading shards:  50% Completed | 1/2 [00:01<00:01,  1.19s/it]

    Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.31s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.29s/it]


    2026-04-02 06:53:39,355 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 06:53:39] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:00,  3.17s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:00,  3.17s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:04<01:40,  1.80s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:04<01:40,  1.80s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:03,  1.15s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:03,  1.15s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:45,  1.20it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:45,  1.20it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:33,  1.58it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:33,  1.58it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:26,  1.99it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:26,  1.99it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:20,  2.48it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:20,  2.48it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:05<00:16,  3.04it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:05<00:16,  3.04it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:05<00:13,  3.68it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:05<00:13,  3.68it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:05<00:10,  4.54it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:05<00:10,  4.54it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:06<00:09,  5.22it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:06<00:09,  5.22it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:06<00:07,  5.92it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:06<00:07,  5.92it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:06<00:06,  6.69it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:06<00:06,  6.69it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:06<00:06,  6.69it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:06<00:04,  8.67it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:06<00:04,  8.67it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:06<00:04,  8.67it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:06<00:03, 11.09it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:06<00:03, 11.09it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:06<00:03, 11.09it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:06<00:03, 11.09it/s]

    Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:06<00:02, 14.83it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:06<00:02, 14.83it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:06<00:02, 14.83it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:06<00:02, 14.83it/s]Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:06<00:02, 14.83it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:06<00:01, 19.18it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:06<00:01, 19.18it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:06<00:01, 19.18it/s]

    Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:06<00:01, 18.96it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:06<00:01, 18.96it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:06<00:01, 18.96it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:06<00:01, 18.94it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:06<00:01, 18.94it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:07<00:01, 18.94it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:07<00:01, 18.94it/s]

    Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:07<00:01, 20.98it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:07<00:01, 20.98it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:07<00:01, 20.98it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:07<00:01, 20.98it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:07<00:01, 20.98it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:07<00:00, 25.01it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:07<00:00, 25.01it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:07<00:00, 25.01it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:07<00:00, 25.01it/s]

    Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:07<00:00, 25.88it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:07<00:00, 25.88it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:07<00:00, 25.88it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:07<00:00, 25.88it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:07<00:00, 25.63it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:07<00:00, 25.63it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:07<00:00, 25.63it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:07<00:00, 25.63it/s]

    Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:07<00:00, 26.34it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:07<00:00, 26.34it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:07<00:00, 26.34it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:07<00:00, 26.34it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:07<00:00, 26.34it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:07<00:00, 27.66it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:07<00:00, 27.66it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:07<00:00, 27.66it/s]

    Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:07<00:00, 27.66it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:07<00:00, 28.20it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:07<00:00, 28.20it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:07<00:00, 28.20it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:07<00:00, 28.20it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:07<00:00, 28.20it/s]Compiling num tokens (num_tokens=16):  95%|█████████▍| 55/58 [00:07<00:00, 31.31it/s]Compiling num tokens (num_tokens=12):  95%|█████████▍| 55/58 [00:07<00:00, 31.31it/s]Compiling num tokens (num_tokens=8):  95%|█████████▍| 55/58 [00:07<00:00, 31.31it/s] Compiling num tokens (num_tokens=4):  95%|█████████▍| 55/58 [00:07<00:00, 31.31it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:07<00:00,  7.33it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=104.05 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=104.05 GB):   2%|▏         | 1/58 [00:00<00:31,  1.79it/s]Capturing num tokens (num_tokens=7680 avail_mem=104.75 GB):   2%|▏         | 1/58 [00:00<00:31,  1.79it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=104.75 GB):   3%|▎         | 2/58 [00:01<00:28,  1.94it/s]Capturing num tokens (num_tokens=7168 avail_mem=104.23 GB):   3%|▎         | 2/58 [00:01<00:28,  1.94it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=104.23 GB):   5%|▌         | 3/58 [00:01<00:26,  2.10it/s]Capturing num tokens (num_tokens=6656 avail_mem=104.76 GB):   5%|▌         | 3/58 [00:01<00:26,  2.10it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=104.76 GB):   7%|▋         | 4/58 [00:01<00:24,  2.21it/s]Capturing num tokens (num_tokens=6144 avail_mem=104.33 GB):   7%|▋         | 4/58 [00:01<00:24,  2.21it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=104.33 GB):   9%|▊         | 5/58 [00:02<00:21,  2.44it/s]Capturing num tokens (num_tokens=5632 avail_mem=104.40 GB):   9%|▊         | 5/58 [00:02<00:21,  2.44it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=104.40 GB):  10%|█         | 6/58 [00:02<00:19,  2.65it/s]Capturing num tokens (num_tokens=5120 avail_mem=104.77 GB):  10%|█         | 6/58 [00:02<00:19,  2.65it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=104.77 GB):  12%|█▏        | 7/58 [00:02<00:17,  2.92it/s]Capturing num tokens (num_tokens=4608 avail_mem=104.51 GB):  12%|█▏        | 7/58 [00:02<00:17,  2.92it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=104.51 GB):  14%|█▍        | 8/58 [00:03<00:15,  3.31it/s]Capturing num tokens (num_tokens=4096 avail_mem=104.77 GB):  14%|█▍        | 8/58 [00:03<00:15,  3.31it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=104.77 GB):  16%|█▌        | 9/58 [00:03<00:13,  3.65it/s]Capturing num tokens (num_tokens=3840 avail_mem=104.78 GB):  16%|█▌        | 9/58 [00:03<00:13,  3.65it/s]Capturing num tokens (num_tokens=3840 avail_mem=104.78 GB):  17%|█▋        | 10/58 [00:03<00:11,  4.11it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=104.64 GB):  17%|█▋        | 10/58 [00:03<00:11,  4.11it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=104.64 GB):  19%|█▉        | 11/58 [00:03<00:12,  3.77it/s]Capturing num tokens (num_tokens=3328 avail_mem=104.76 GB):  19%|█▉        | 11/58 [00:03<00:12,  3.77it/s]Capturing num tokens (num_tokens=3328 avail_mem=104.76 GB):  21%|██        | 12/58 [00:03<00:10,  4.28it/s]Capturing num tokens (num_tokens=3072 avail_mem=104.75 GB):  21%|██        | 12/58 [00:03<00:10,  4.28it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=104.75 GB):  22%|██▏       | 13/58 [00:04<00:09,  4.82it/s]Capturing num tokens (num_tokens=2816 avail_mem=104.77 GB):  22%|██▏       | 13/58 [00:04<00:09,  4.82it/s]Capturing num tokens (num_tokens=2816 avail_mem=104.77 GB):  24%|██▍       | 14/58 [00:04<00:07,  5.56it/s]Capturing num tokens (num_tokens=2560 avail_mem=104.74 GB):  24%|██▍       | 14/58 [00:04<00:07,  5.56it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=104.74 GB):  26%|██▌       | 15/58 [00:04<00:07,  6.01it/s]Capturing num tokens (num_tokens=2304 avail_mem=104.69 GB):  26%|██▌       | 15/58 [00:04<00:07,  6.01it/s]Capturing num tokens (num_tokens=2304 avail_mem=104.69 GB):  28%|██▊       | 16/58 [00:04<00:06,  6.81it/s]Capturing num tokens (num_tokens=2048 avail_mem=104.73 GB):  28%|██▊       | 16/58 [00:04<00:06,  6.81it/s]Capturing num tokens (num_tokens=1792 avail_mem=104.69 GB):  28%|██▊       | 16/58 [00:04<00:06,  6.81it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=104.69 GB):  31%|███       | 18/58 [00:04<00:04,  8.33it/s]Capturing num tokens (num_tokens=1536 avail_mem=104.71 GB):  31%|███       | 18/58 [00:04<00:04,  8.33it/s]Capturing num tokens (num_tokens=1280 avail_mem=104.71 GB):  31%|███       | 18/58 [00:04<00:04,  8.33it/s]Capturing num tokens (num_tokens=1280 avail_mem=104.71 GB):  34%|███▍      | 20/58 [00:04<00:03, 10.04it/s]Capturing num tokens (num_tokens=1024 avail_mem=104.69 GB):  34%|███▍      | 20/58 [00:04<00:03, 10.04it/s]Capturing num tokens (num_tokens=960 avail_mem=104.70 GB):  34%|███▍      | 20/58 [00:04<00:03, 10.04it/s] 

    Capturing num tokens (num_tokens=960 avail_mem=104.70 GB):  38%|███▊      | 22/58 [00:04<00:03, 11.15it/s]Capturing num tokens (num_tokens=896 avail_mem=104.69 GB):  38%|███▊      | 22/58 [00:04<00:03, 11.15it/s]Capturing num tokens (num_tokens=832 avail_mem=104.68 GB):  38%|███▊      | 22/58 [00:04<00:03, 11.15it/s]Capturing num tokens (num_tokens=832 avail_mem=104.68 GB):  41%|████▏     | 24/58 [00:04<00:02, 13.10it/s]Capturing num tokens (num_tokens=768 avail_mem=104.68 GB):  41%|████▏     | 24/58 [00:04<00:02, 13.10it/s]Capturing num tokens (num_tokens=704 avail_mem=104.67 GB):  41%|████▏     | 24/58 [00:04<00:02, 13.10it/s]Capturing num tokens (num_tokens=640 avail_mem=104.66 GB):  41%|████▏     | 24/58 [00:05<00:02, 13.10it/s]

    Capturing num tokens (num_tokens=640 avail_mem=104.66 GB):  47%|████▋     | 27/58 [00:05<00:01, 15.83it/s]Capturing num tokens (num_tokens=576 avail_mem=104.67 GB):  47%|████▋     | 27/58 [00:05<00:01, 15.83it/s]Capturing num tokens (num_tokens=512 avail_mem=104.66 GB):  47%|████▋     | 27/58 [00:05<00:01, 15.83it/s]Capturing num tokens (num_tokens=480 avail_mem=104.65 GB):  47%|████▋     | 27/58 [00:05<00:01, 15.83it/s]Capturing num tokens (num_tokens=480 avail_mem=104.65 GB):  52%|█████▏    | 30/58 [00:05<00:01, 18.41it/s]Capturing num tokens (num_tokens=448 avail_mem=104.65 GB):  52%|█████▏    | 30/58 [00:05<00:01, 18.41it/s]Capturing num tokens (num_tokens=416 avail_mem=104.64 GB):  52%|█████▏    | 30/58 [00:05<00:01, 18.41it/s]Capturing num tokens (num_tokens=384 avail_mem=104.63 GB):  52%|█████▏    | 30/58 [00:05<00:01, 18.41it/s]

    Capturing num tokens (num_tokens=384 avail_mem=104.63 GB):  57%|█████▋    | 33/58 [00:05<00:01, 20.70it/s]Capturing num tokens (num_tokens=352 avail_mem=104.62 GB):  57%|█████▋    | 33/58 [00:05<00:01, 20.70it/s]Capturing num tokens (num_tokens=320 avail_mem=104.62 GB):  57%|█████▋    | 33/58 [00:05<00:01, 20.70it/s]Capturing num tokens (num_tokens=288 avail_mem=104.59 GB):  57%|█████▋    | 33/58 [00:05<00:01, 20.70it/s]Capturing num tokens (num_tokens=256 avail_mem=104.58 GB):  57%|█████▋    | 33/58 [00:05<00:01, 20.70it/s]Capturing num tokens (num_tokens=256 avail_mem=104.58 GB):  64%|██████▍   | 37/58 [00:05<00:00, 22.05it/s]Capturing num tokens (num_tokens=240 avail_mem=104.57 GB):  64%|██████▍   | 37/58 [00:05<00:00, 22.05it/s]Capturing num tokens (num_tokens=224 avail_mem=104.59 GB):  64%|██████▍   | 37/58 [00:05<00:00, 22.05it/s]

    Capturing num tokens (num_tokens=208 avail_mem=104.58 GB):  64%|██████▍   | 37/58 [00:05<00:00, 22.05it/s]Capturing num tokens (num_tokens=208 avail_mem=104.58 GB):  69%|██████▉   | 40/58 [00:05<00:00, 23.89it/s]Capturing num tokens (num_tokens=192 avail_mem=104.57 GB):  69%|██████▉   | 40/58 [00:05<00:00, 23.89it/s]Capturing num tokens (num_tokens=176 avail_mem=104.57 GB):  69%|██████▉   | 40/58 [00:05<00:00, 23.89it/s]Capturing num tokens (num_tokens=160 avail_mem=104.56 GB):  69%|██████▉   | 40/58 [00:05<00:00, 23.89it/s]Capturing num tokens (num_tokens=144 avail_mem=104.55 GB):  69%|██████▉   | 40/58 [00:05<00:00, 23.89it/s]Capturing num tokens (num_tokens=144 avail_mem=104.55 GB):  76%|███████▌  | 44/58 [00:05<00:00, 26.68it/s]Capturing num tokens (num_tokens=128 avail_mem=104.56 GB):  76%|███████▌  | 44/58 [00:05<00:00, 26.68it/s]Capturing num tokens (num_tokens=112 avail_mem=104.56 GB):  76%|███████▌  | 44/58 [00:05<00:00, 26.68it/s]

    Capturing num tokens (num_tokens=96 avail_mem=104.55 GB):  76%|███████▌  | 44/58 [00:05<00:00, 26.68it/s] Capturing num tokens (num_tokens=80 avail_mem=104.55 GB):  76%|███████▌  | 44/58 [00:05<00:00, 26.68it/s]Capturing num tokens (num_tokens=80 avail_mem=104.55 GB):  83%|████████▎ | 48/58 [00:05<00:00, 29.70it/s]Capturing num tokens (num_tokens=64 avail_mem=104.55 GB):  83%|████████▎ | 48/58 [00:05<00:00, 29.70it/s]Capturing num tokens (num_tokens=48 avail_mem=104.54 GB):  83%|████████▎ | 48/58 [00:05<00:00, 29.70it/s]Capturing num tokens (num_tokens=32 avail_mem=104.54 GB):  83%|████████▎ | 48/58 [00:05<00:00, 29.70it/s]Capturing num tokens (num_tokens=28 avail_mem=104.54 GB):  83%|████████▎ | 48/58 [00:05<00:00, 29.70it/s]Capturing num tokens (num_tokens=28 avail_mem=104.54 GB):  90%|████████▉ | 52/58 [00:05<00:00, 32.05it/s]Capturing num tokens (num_tokens=24 avail_mem=104.53 GB):  90%|████████▉ | 52/58 [00:05<00:00, 32.05it/s]

    Capturing num tokens (num_tokens=20 avail_mem=104.53 GB):  90%|████████▉ | 52/58 [00:05<00:00, 32.05it/s]Capturing num tokens (num_tokens=16 avail_mem=104.53 GB):  90%|████████▉ | 52/58 [00:06<00:00, 32.05it/s]Capturing num tokens (num_tokens=12 avail_mem=104.52 GB):  90%|████████▉ | 52/58 [00:06<00:00, 32.05it/s]Capturing num tokens (num_tokens=12 avail_mem=104.52 GB):  97%|█████████▋| 56/58 [00:06<00:00, 28.94it/s]Capturing num tokens (num_tokens=8 avail_mem=104.52 GB):  97%|█████████▋| 56/58 [00:06<00:00, 28.94it/s] Capturing num tokens (num_tokens=4 avail_mem=104.52 GB):  97%|█████████▋| 56/58 [00:06<00:00, 28.94it/s]Capturing num tokens (num_tokens=4 avail_mem=104.52 GB): 100%|██████████| 58/58 [00:06<00:00,  9.42it/s]


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
    Generated text: Paris is the capital of France
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
    
    Generated text: Okay, I need to figure out how to respond to the user's query. They asked for the information and population of the capital of France in JSON format. First, I should determine what the capital of France is. I know that Paris is the capital of France, so that's straightforward.
    
    Next, I need to find the most recent population data for Paris. I remember that the population figures can change each year, so I should look for the latest available data. From what I recall, the population was around 2 million in recent years. I should verify that to make sure it's accurate.
    
    Then, I need to structure this information into a JSON format. JSON typically uses key-value pairs, so I'll create an object with a "capital" key and its value as "Paris", and a "population" key with the respective number. I'll make sure the JSON syntax is correct, using quotation marks and commas appropriately.
    
    I should also consider if there are any nuances the user might be looking for. They might want the population in a specific format or maybe a note if the number is approximate. Since I don't have exact current data, providing an approximate figure with a note that it's an estimate might be helpful.
    
    Finally, I'll present the JSON in a clear and concise manner, ensuring that it's easy for the user to understand and use if needed.
    </think>
    
    Certainly! Here is the information and population of the capital of France in JSON format:
    
    ```json
    {
      "capital": "Paris",
      "population": 2070000
    }
    ```
    
    This JSON object contains the following information:
    - The capital of France: Paris
    - The population of Paris: approximately 2,070,000 people (as of the latest available data)
    
    Note: The population figure is an estimate and may vary depending on the source and the year of the data.



```python
llm.shutdown()
```
