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

    Multi-thread loading shards:  50% Completed | 1/2 [00:00<00:00,  1.31it/s]

    Multi-thread loading shards: 100% Completed | 2/2 [00:01<00:00,  1.04s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:01<00:00,  1.00it/s]


    2026-04-04 05:28:28,899 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 05:28:28] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:57,  3.11s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:57,  3.11s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:39,  1.77s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:39,  1.77s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:03,  1.15s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:03,  1.15s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:45,  1.19it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:45,  1.19it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:34,  1.52it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:34,  1.52it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:27,  1.86it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:27,  1.86it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:22,  2.29it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:22,  2.29it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:05<00:18,  2.66it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:05<00:18,  2.66it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:06<00:17,  2.83it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:06<00:17,  2.83it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:06<00:15,  3.08it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:06<00:15,  3.08it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:06<00:14,  3.33it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:06<00:14,  3.33it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:06<00:12,  3.63it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:06<00:12,  3.63it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:07<00:11,  4.01it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:07<00:11,  4.01it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:07<00:10,  4.36it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:07<00:10,  4.36it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:07<00:09,  4.77it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:07<00:09,  4.77it/s]

    Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:07<00:07,  5.33it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:07<00:07,  5.33it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:07<00:07,  5.77it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:07<00:07,  5.77it/s]

    Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:07<00:06,  6.53it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:07<00:06,  6.53it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:07<00:06,  6.53it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:07<00:04,  8.25it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:07<00:04,  8.25it/s]

    Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:08<00:04,  8.25it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:08<00:03,  9.86it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:08<00:03,  9.86it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:08<00:03,  9.86it/s]

    Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:08<00:03,  9.93it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:08<00:03,  9.93it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:08<00:03,  9.93it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:08<00:02, 10.69it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:08<00:02, 10.69it/s]

    Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:08<00:02, 10.69it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:08<00:02, 11.25it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:08<00:02, 11.25it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:08<00:02, 11.25it/s]

    Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:08<00:02, 12.44it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:08<00:02, 12.44it/s]Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:08<00:02, 12.44it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:08<00:01, 13.93it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:08<00:01, 13.93it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:08<00:01, 13.93it/s]

    Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:08<00:01, 13.93it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:08<00:01, 16.04it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:08<00:01, 16.04it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:09<00:01, 16.04it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:09<00:01, 15.95it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:09<00:01, 15.95it/s]

    Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:09<00:01, 15.95it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:09<00:01, 15.95it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:09<00:01, 17.00it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:09<00:01, 17.00it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:09<00:01, 17.00it/s]

    Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:09<00:00, 16.88it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:09<00:00, 16.88it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:09<00:00, 16.88it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:09<00:00, 17.15it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:09<00:00, 17.15it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:09<00:00, 17.15it/s]

    Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:09<00:00, 17.85it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:09<00:00, 17.85it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:09<00:00, 17.85it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:09<00:00, 17.50it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:09<00:00, 17.50it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:09<00:00, 17.50it/s]

    Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:09<00:00, 17.88it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:09<00:00, 17.88it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:09<00:00, 17.88it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:09<00:00, 17.88it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:09<00:00, 18.36it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:09<00:00, 18.36it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:10<00:00, 18.36it/s]

    Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:10<00:00, 18.36it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:10<00:00, 19.50it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:10<00:00, 19.50it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:10<00:00, 19.50it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:10<00:00,  5.69it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=84.40 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=84.40 GB):   2%|▏         | 1/58 [00:00<00:53,  1.07it/s]Capturing num tokens (num_tokens=7680 avail_mem=84.44 GB):   2%|▏         | 1/58 [00:00<00:53,  1.07it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=84.44 GB):   3%|▎         | 2/58 [00:01<00:44,  1.25it/s]Capturing num tokens (num_tokens=7168 avail_mem=84.57 GB):   3%|▎         | 2/58 [00:01<00:44,  1.25it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=84.57 GB):   5%|▌         | 3/58 [00:02<00:39,  1.40it/s]Capturing num tokens (num_tokens=6656 avail_mem=84.65 GB):   5%|▌         | 3/58 [00:02<00:39,  1.40it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=84.65 GB):   7%|▋         | 4/58 [00:02<00:33,  1.62it/s]Capturing num tokens (num_tokens=6144 avail_mem=85.18 GB):   7%|▋         | 4/58 [00:02<00:33,  1.62it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=85.18 GB):   9%|▊         | 5/58 [00:03<00:31,  1.66it/s]Capturing num tokens (num_tokens=5632 avail_mem=84.75 GB):   9%|▊         | 5/58 [00:03<00:31,  1.66it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=84.75 GB):  10%|█         | 6/58 [00:03<00:27,  1.90it/s]Capturing num tokens (num_tokens=5120 avail_mem=85.18 GB):  10%|█         | 6/58 [00:03<00:27,  1.90it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=85.18 GB):  12%|█▏        | 7/58 [00:04<00:24,  2.08it/s]Capturing num tokens (num_tokens=4608 avail_mem=84.15 GB):  12%|█▏        | 7/58 [00:04<00:24,  2.08it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=84.15 GB):  14%|█▍        | 8/58 [00:04<00:23,  2.15it/s]Capturing num tokens (num_tokens=4096 avail_mem=84.86 GB):  14%|█▍        | 8/58 [00:04<00:23,  2.15it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=84.86 GB):  16%|█▌        | 9/58 [00:04<00:20,  2.37it/s]Capturing num tokens (num_tokens=3840 avail_mem=84.34 GB):  16%|█▌        | 9/58 [00:04<00:20,  2.37it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=84.34 GB):  17%|█▋        | 10/58 [00:05<00:19,  2.45it/s]Capturing num tokens (num_tokens=3584 avail_mem=84.96 GB):  17%|█▋        | 10/58 [00:05<00:19,  2.45it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=84.96 GB):  19%|█▉        | 11/58 [00:05<00:17,  2.74it/s]Capturing num tokens (num_tokens=3328 avail_mem=84.39 GB):  19%|█▉        | 11/58 [00:05<00:17,  2.74it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=84.39 GB):  21%|██        | 12/58 [00:05<00:16,  2.85it/s]Capturing num tokens (num_tokens=3072 avail_mem=84.39 GB):  21%|██        | 12/58 [00:05<00:16,  2.85it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=84.39 GB):  22%|██▏       | 13/58 [00:06<00:14,  3.10it/s]Capturing num tokens (num_tokens=2816 avail_mem=84.44 GB):  22%|██▏       | 13/58 [00:06<00:14,  3.10it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=84.44 GB):  24%|██▍       | 14/58 [00:06<00:13,  3.25it/s]Capturing num tokens (num_tokens=2560 avail_mem=84.44 GB):  24%|██▍       | 14/58 [00:06<00:13,  3.25it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=84.44 GB):  26%|██▌       | 15/58 [00:06<00:11,  3.59it/s]Capturing num tokens (num_tokens=2304 avail_mem=85.14 GB):  26%|██▌       | 15/58 [00:06<00:11,  3.59it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=85.14 GB):  28%|██▊       | 16/58 [00:06<00:11,  3.77it/s]Capturing num tokens (num_tokens=2048 avail_mem=84.40 GB):  28%|██▊       | 16/58 [00:06<00:11,  3.77it/s]Capturing num tokens (num_tokens=2048 avail_mem=84.40 GB):  29%|██▉       | 17/58 [00:06<00:09,  4.18it/s]Capturing num tokens (num_tokens=1792 avail_mem=85.14 GB):  29%|██▉       | 17/58 [00:06<00:09,  4.18it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=85.14 GB):  31%|███       | 18/58 [00:07<00:08,  4.51it/s]Capturing num tokens (num_tokens=1536 avail_mem=84.52 GB):  31%|███       | 18/58 [00:07<00:08,  4.51it/s]Capturing num tokens (num_tokens=1536 avail_mem=84.52 GB):  33%|███▎      | 19/58 [00:07<00:07,  4.93it/s]Capturing num tokens (num_tokens=1280 avail_mem=85.08 GB):  33%|███▎      | 19/58 [00:07<00:07,  4.93it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=85.08 GB):  34%|███▍      | 20/58 [00:07<00:07,  5.37it/s]Capturing num tokens (num_tokens=1024 avail_mem=84.58 GB):  34%|███▍      | 20/58 [00:07<00:07,  5.37it/s]Capturing num tokens (num_tokens=1024 avail_mem=84.58 GB):  36%|███▌      | 21/58 [00:07<00:06,  5.80it/s]Capturing num tokens (num_tokens=960 avail_mem=85.12 GB):  36%|███▌      | 21/58 [00:07<00:06,  5.80it/s] 

    Capturing num tokens (num_tokens=960 avail_mem=85.12 GB):  38%|███▊      | 22/58 [00:07<00:05,  6.36it/s]Capturing num tokens (num_tokens=896 avail_mem=84.62 GB):  38%|███▊      | 22/58 [00:07<00:05,  6.36it/s]Capturing num tokens (num_tokens=896 avail_mem=84.62 GB):  40%|███▉      | 23/58 [00:07<00:05,  5.94it/s]Capturing num tokens (num_tokens=832 avail_mem=85.11 GB):  40%|███▉      | 23/58 [00:07<00:05,  5.94it/s]

    Capturing num tokens (num_tokens=832 avail_mem=85.11 GB):  41%|████▏     | 24/58 [00:07<00:05,  6.67it/s]Capturing num tokens (num_tokens=768 avail_mem=84.66 GB):  41%|████▏     | 24/58 [00:07<00:05,  6.67it/s]Capturing num tokens (num_tokens=768 avail_mem=84.66 GB):  43%|████▎     | 25/58 [00:08<00:04,  7.04it/s]Capturing num tokens (num_tokens=704 avail_mem=85.10 GB):  43%|████▎     | 25/58 [00:08<00:04,  7.04it/s]

    Capturing num tokens (num_tokens=640 avail_mem=84.67 GB):  43%|████▎     | 25/58 [00:08<00:04,  7.04it/s]Capturing num tokens (num_tokens=640 avail_mem=84.67 GB):  47%|████▋     | 27/58 [00:08<00:03,  7.87it/s]Capturing num tokens (num_tokens=576 avail_mem=84.67 GB):  47%|████▋     | 27/58 [00:08<00:03,  7.87it/s]

    Capturing num tokens (num_tokens=512 avail_mem=85.07 GB):  47%|████▋     | 27/58 [00:08<00:03,  7.87it/s]Capturing num tokens (num_tokens=512 avail_mem=85.07 GB):  50%|█████     | 29/58 [00:08<00:03,  8.85it/s]Capturing num tokens (num_tokens=480 avail_mem=84.68 GB):  50%|█████     | 29/58 [00:08<00:03,  8.85it/s]Capturing num tokens (num_tokens=448 avail_mem=85.05 GB):  50%|█████     | 29/58 [00:08<00:03,  8.85it/s]

    Capturing num tokens (num_tokens=448 avail_mem=85.05 GB):  53%|█████▎    | 31/58 [00:08<00:02,  9.44it/s]Capturing num tokens (num_tokens=416 avail_mem=84.70 GB):  53%|█████▎    | 31/58 [00:08<00:02,  9.44it/s]Capturing num tokens (num_tokens=384 avail_mem=85.06 GB):  53%|█████▎    | 31/58 [00:08<00:02,  9.44it/s]Capturing num tokens (num_tokens=384 avail_mem=85.06 GB):  57%|█████▋    | 33/58 [00:08<00:02,  9.76it/s]Capturing num tokens (num_tokens=352 avail_mem=84.71 GB):  57%|█████▋    | 33/58 [00:08<00:02,  9.76it/s]

    Capturing num tokens (num_tokens=320 avail_mem=84.96 GB):  57%|█████▋    | 33/58 [00:08<00:02,  9.76it/s]Capturing num tokens (num_tokens=320 avail_mem=84.96 GB):  60%|██████    | 35/58 [00:09<00:02, 10.42it/s]Capturing num tokens (num_tokens=288 avail_mem=85.02 GB):  60%|██████    | 35/58 [00:09<00:02, 10.42it/s]Capturing num tokens (num_tokens=256 avail_mem=84.73 GB):  60%|██████    | 35/58 [00:09<00:02, 10.42it/s]

    Capturing num tokens (num_tokens=256 avail_mem=84.73 GB):  64%|██████▍   | 37/58 [00:09<00:01, 11.31it/s]Capturing num tokens (num_tokens=240 avail_mem=84.99 GB):  64%|██████▍   | 37/58 [00:09<00:01, 11.31it/s]Capturing num tokens (num_tokens=224 avail_mem=84.76 GB):  64%|██████▍   | 37/58 [00:09<00:01, 11.31it/s]Capturing num tokens (num_tokens=224 avail_mem=84.76 GB):  67%|██████▋   | 39/58 [00:09<00:01, 12.09it/s]Capturing num tokens (num_tokens=208 avail_mem=84.98 GB):  67%|██████▋   | 39/58 [00:09<00:01, 12.09it/s]

    Capturing num tokens (num_tokens=192 avail_mem=84.77 GB):  67%|██████▋   | 39/58 [00:09<00:01, 12.09it/s]Capturing num tokens (num_tokens=192 avail_mem=84.77 GB):  71%|███████   | 41/58 [00:09<00:01, 13.14it/s]Capturing num tokens (num_tokens=176 avail_mem=84.97 GB):  71%|███████   | 41/58 [00:09<00:01, 13.14it/s]Capturing num tokens (num_tokens=160 avail_mem=84.84 GB):  71%|███████   | 41/58 [00:09<00:01, 13.14it/s]Capturing num tokens (num_tokens=160 avail_mem=84.84 GB):  74%|███████▍  | 43/58 [00:09<00:01, 14.39it/s]Capturing num tokens (num_tokens=144 avail_mem=84.93 GB):  74%|███████▍  | 43/58 [00:09<00:01, 14.39it/s]

    Capturing num tokens (num_tokens=128 avail_mem=84.98 GB):  74%|███████▍  | 43/58 [00:09<00:01, 14.39it/s]Capturing num tokens (num_tokens=128 avail_mem=84.98 GB):  78%|███████▊  | 45/58 [00:09<00:00, 14.57it/s]Capturing num tokens (num_tokens=112 avail_mem=84.97 GB):  78%|███████▊  | 45/58 [00:09<00:00, 14.57it/s]Capturing num tokens (num_tokens=96 avail_mem=84.85 GB):  78%|███████▊  | 45/58 [00:09<00:00, 14.57it/s] Capturing num tokens (num_tokens=80 avail_mem=84.84 GB):  78%|███████▊  | 45/58 [00:09<00:00, 14.57it/s]Capturing num tokens (num_tokens=80 avail_mem=84.84 GB):  83%|████████▎ | 48/58 [00:09<00:00, 16.85it/s]Capturing num tokens (num_tokens=64 avail_mem=84.94 GB):  83%|████████▎ | 48/58 [00:09<00:00, 16.85it/s]

    Capturing num tokens (num_tokens=48 avail_mem=84.94 GB):  83%|████████▎ | 48/58 [00:09<00:00, 16.85it/s]Capturing num tokens (num_tokens=32 avail_mem=84.93 GB):  83%|████████▎ | 48/58 [00:09<00:00, 16.85it/s]Capturing num tokens (num_tokens=32 avail_mem=84.93 GB):  88%|████████▊ | 51/58 [00:09<00:00, 18.24it/s]Capturing num tokens (num_tokens=28 avail_mem=84.93 GB):  88%|████████▊ | 51/58 [00:09<00:00, 18.24it/s]Capturing num tokens (num_tokens=24 avail_mem=84.92 GB):  88%|████████▊ | 51/58 [00:10<00:00, 18.24it/s]Capturing num tokens (num_tokens=20 avail_mem=84.86 GB):  88%|████████▊ | 51/58 [00:10<00:00, 18.24it/s]

    Capturing num tokens (num_tokens=20 avail_mem=84.86 GB):  93%|█████████▎| 54/58 [00:10<00:00, 20.09it/s]Capturing num tokens (num_tokens=16 avail_mem=84.85 GB):  93%|█████████▎| 54/58 [00:10<00:00, 20.09it/s]Capturing num tokens (num_tokens=12 avail_mem=84.85 GB):  93%|█████████▎| 54/58 [00:10<00:00, 20.09it/s]Capturing num tokens (num_tokens=8 avail_mem=84.89 GB):  93%|█████████▎| 54/58 [00:10<00:00, 20.09it/s] Capturing num tokens (num_tokens=8 avail_mem=84.89 GB):  98%|█████████▊| 57/58 [00:10<00:00, 21.36it/s]Capturing num tokens (num_tokens=4 avail_mem=84.88 GB):  98%|█████████▊| 57/58 [00:10<00:00, 21.36it/s]Capturing num tokens (num_tokens=4 avail_mem=84.88 GB): 100%|██████████| 58/58 [00:10<00:00,  5.65it/s]


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


<strong style='color: #00008B;'>reasoing_content: Okay, so the user is asking for the current date and time in New York and the weather there. I need to figure out how to get this information using the functions provided.<br><br>First, I'll look at the functions available. There's get_current_weather and get_current_date. I need both pieces of information, so I'll have to use both functions.<br><br>Starting with the date, I'll use get_current_date. The function requires a timezone parameter. Since the user is in New York, I'll specify 'America/New_York' as the timezone. I'll structure this as a JSON object within the function call.<br><br>Next, for the weather, I'll use get_current_weather. This function needs a city, state, and unit. The city is New York, but I need the two-letter state abbreviation. New York is NY, so state is 'NY'. The unit should be Fahrenheit since the user didn't specify, but I'll include it as 'fahrenheit' in the parameters.<br><br>I should make sure each function call is separate and follows the required format. I'll construct each function call with the appropriate parameters, ensuring that all required fields are included and correctly formatted as JSON within the function.<br><br>Putting it all together, I'll send two separate function calls: one for the date with the timezone and another for the weather with city, state, and unit. This way, the user gets both the current date and time in New York and the weather details they're asking for.<br><br><br>content: <function=get_current_date>{"timezone": "America/New_York"}</function><br><function=get_current_weather>{"city": "New York", "state": "NY", "unit": "fahrenheit"}</function></strong>


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

    {'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. I know that Paris is the capital, so that\'s straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I\'m not sure of the exact number. I think it\'s around 2 million, but I should verify that.\n\nWait, maybe I should check the latest statistics. I recall that in recent years, Paris has seen a slight increase. Let me think... I believe the population is approximately 2,150,000 as of 2023. That seems about right, but I\'m not 100% certain. I should make sure to present this information accurately.\n\nNext, I need to structure this into a JSON format. JSON requires key-value pairs, so I\'ll create an object with keys like "city", "population", and maybe "country" for context. The city is Paris, the population is 2,150,000, and the country is France.\n\nI should also consider if the user might need more details, like the exact year of the population figure. Including that could be helpful, so I\'ll add "year": 2023. That way, the user knows the data is up to date.\n\nPutting it all together, the JSON should look clean and well-structured. I\'ll make sure the syntax is correct, with proper commas and quotation marks. No markdown, just plain JSON.\n\nI think that\'s all. The user probably just needs the information quickly, so keeping it concise is key. I\'ll present the JSON without any extra fluff.\n</think>{\n\n"name": "Paris",\n"population": 21500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1265, 1477, 279, 1429, 3213, 7042, 821, 13, 358, 6099, 429, 279, 7042, 315, 12095, 702, 1012, 7826, 11, 714, 358, 2776, 537, 2704, 315, 279, 4734, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 10146, 429, 382, 14190, 11, 7196, 358, 1265, 1779, 279, 5535, 13142, 13, 358, 19091, 429, 304, 3213, 1635, 11, 12095, 702, 3884, 264, 8112, 5263, 13, 6771, 752, 1744, 1112, 358, 4411, 279, 7042, 374, 13187, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 438, 315, 220, 17, 15, 17, 18, 13, 2938, 4977, 911, 1290, 11, 714, 358, 2776, 537, 220, 16, 15, 15, 4, 3654, 13, 358, 1265, 1281, 2704, 311, 3042, 419, 1995, 29257, 382, 5847, 11, 358, 1184, 311, 5944, 419, 1119, 264, 4718, 3561, 13, 4718, 7460, 1376, 19083, 13530, 11, 773, 358, 3278, 1855, 458, 1633, 448, 6894, 1075, 330, 8926, 497, 330, 44441, 497, 323, 7196, 330, 11141, 1, 369, 2266, 13, 576, 3283, 374, 12095, 11, 279, 7042, 374, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 11, 323, 279, 3146, 374, 9625, 382, 40, 1265, 1083, 2908, 421, 279, 1196, 2578, 1184, 803, 3565, 11, 1075, 279, 4734, 1042, 315, 279, 7042, 7071, 13, 55121, 429, 1410, 387, 10950, 11, 773, 358, 3278, 912, 330, 3157, 788, 220, 17, 15, 17, 18, 13, 2938, 1616, 11, 279, 1196, 8788, 279, 821, 374, 705, 311, 2400, 382, 97904, 432, 678, 3786, 11, 279, 4718, 1265, 1401, 4240, 323, 1632, 12, 51143, 13, 358, 3278, 1281, 2704, 279, 19482, 374, 4396, 11, 448, 6169, 76602, 323, 54231, 15423, 13, 2308, 50494, 11, 1101, 14396, 4718, 382, 40, 1744, 429, 594, 678, 13, 576, 1196, 4658, 1101, 3880, 279, 1995, 6157, 11, 773, 10282, 432, 63594, 374, 1376, 13, 358, 3278, 3042, 279, 4718, 2041, 894, 4960, 1320, 1362, 624, 151649, 4257, 1, 606, 788, 330, 59604, 756, 1, 44441, 788, 220, 17, 16, 20, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15], 'meta_info': {'id': 'a23e4f91e89346b19a58e258cd577679', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': {'device': 1, 'host': 0}, 'dp_rank': None, 'e2e_latency': 20.276404540985823, 'response_sent_to_client_ts': 1775280577.8784761}}



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


<strong style='color: #00008B;'>{'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. I know that Paris is the capital, so that\'s straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I\'m not sure of the exact number. I think it\'s around 2 million, but I should verify that.\n\nWait, maybe I should check the latest statistics. I recall that in recent years, Paris has seen a slight increase. Let me think... I believe the population is approximately 2,150,000 as of 2023. That seems about right, but I\'m not 100% certain. I should make sure to present this information accurately.\n\nNext, I need to structure this into a JSON format. JSON requires key-value pairs, so I\'ll create an object with keys like "city", "population", and maybe "country" for context. The city is Paris, the population is 2,150,000, and the country is France.\n\nI should also consider if the user might need more details, like the exact year of the population figure. Including that could be helpful, so I\'ll add "year": 2023. That way, the user knows the data is up to date.\n\nPutting it all together, the JSON should look clean and well-structured. I\'ll make sure the syntax is correct, with proper commas and quotation marks. No markdown, just plain JSON.\n\nI think that\'s all. The user probably just needs the information quickly, so keeping it concise is key. I\'ll present the JSON without any extra fluff.\n</think>{\n\n"name": "Paris",\n"population": 21500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1265, 1477, 279, 1429, 3213, 7042, 821, 13, 358, 6099, 429, 279, 7042, 315, 12095, 702, 1012, 7826, 11, 714, 358, 2776, 537, 2704, 315, 279, 4734, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 10146, 429, 382, 14190, 11, 7196, 358, 1265, 1779, 279, 5535, 13142, 13, 358, 19091, 429, 304, 3213, 1635, 11, 12095, 702, 3884, 264, 8112, 5263, 13, 6771, 752, 1744, 1112, 358, 4411, 279, 7042, 374, 13187, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 438, 315, 220, 17, 15, 17, 18, 13, 2938, 4977, 911, 1290, 11, 714, 358, 2776, 537, 220, 16, 15, 15, 4, 3654, 13, 358, 1265, 1281, 2704, 311, 3042, 419, 1995, 29257, 382, 5847, 11, 358, 1184, 311, 5944, 419, 1119, 264, 4718, 3561, 13, 4718, 7460, 1376, 19083, 13530, 11, 773, 358, 3278, 1855, 458, 1633, 448, 6894, 1075, 330, 8926, 497, 330, 44441, 497, 323, 7196, 330, 11141, 1, 369, 2266, 13, 576, 3283, 374, 12095, 11, 279, 7042, 374, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 11, 323, 279, 3146, 374, 9625, 382, 40, 1265, 1083, 2908, 421, 279, 1196, 2578, 1184, 803, 3565, 11, 1075, 279, 4734, 1042, 315, 279, 7042, 7071, 13, 55121, 429, 1410, 387, 10950, 11, 773, 358, 3278, 912, 330, 3157, 788, 220, 17, 15, 17, 18, 13, 2938, 1616, 11, 279, 1196, 8788, 279, 821, 374, 705, 311, 2400, 382, 97904, 432, 678, 3786, 11, 279, 4718, 1265, 1401, 4240, 323, 1632, 12, 51143, 13, 358, 3278, 1281, 2704, 279, 19482, 374, 4396, 11, 448, 6169, 76602, 323, 54231, 15423, 13, 2308, 50494, 11, 1101, 14396, 4718, 382, 40, 1744, 429, 594, 678, 13, 576, 1196, 4658, 1101, 3880, 279, 1995, 6157, 11, 773, 10282, 432, 63594, 374, 1376, 13, 358, 3278, 3042, 279, 4718, 2041, 894, 4960, 1320, 1362, 624, 151649, 4257, 1, 606, 788, 330, 59604, 756, 1, 44441, 788, 220, 17, 16, 20, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15], 'meta_info': {'id': '70ecd6d00f454548887148d60f435f81', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 22, 'cached_tokens_details': {'device': 22, 'host': 0}, 'dp_rank': None, 'e2e_latency': 20.172508040443063, 'response_sent_to_client_ts': 1775280598.0628057}}</strong>


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

    [{'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': 'e008ca4fc75840a6bc00600d39defcf6', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': {'device': 10, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.11501683201640844, 'response_sent_to_client_ts': 1775280598.2110217}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '40302968f06b49a4b25253e337524b67', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': {'device': 10, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.11493218597024679, 'response_sent_to_client_ts': 1775280598.2110343}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': 'ffa4903276394f5bba13919f29b592ca', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': {'device': 10, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.11486075026914477, 'response_sent_to_client_ts': 1775280598.211039}}]


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

    {'text': ' France, and the \n\\( n \\)  \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\(', 'output_ids': [9625, 11, 323, 279, 220, 198, 44292, 308, 1124, 8, 220, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767], 'meta_info': {'id': '861e1ba260714816b015401c51c9946c', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 6, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': {'device': 1, 'host': 0}, 'dp_rank': None, 'e2e_latency': 18.87785977125168, 'response_sent_to_client_ts': 1775280617.0973337}}


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


<strong style='color: #00008B;'>{'text': 'Okay, so the user asked for the information and population of the capital of France in JSON format. Hmm, first thing I need to figure that out. I know that the capital of France is Paris. Now, I need to find the most recent population figure. I recall that population numbers can change, especially in a big city like Paris. Let me see, the last time I checked, Paris had around 2 million people, but that might be a bit outdated. Wait, I think in 2017, Paris had about 2,165,000 people, including everyone from grandchildren to the elderly. But I should make sure that\'s the current figure. Maybe I can check a reliable source to confirm. \n\nWait, I should also consider including the metropolitan area because sometimes that includes surrounding suburbs and that can push the population higher. But the user asked for the capital, so Paris proper, not the metropolitan area. To be safe, I\'ll provide both. I also remember that Paris is a global city, so the population might be growing a bit each year, maybe around 30,000 more. But since the exact current number is important, I should specify that as of 2023, it\'s approximately 2,165,000, perhaps fluctuating a bit. \n\nI should structure the answer in JSON, as the user requested. That means a key-value pair with "capital" as "Paris" and "population" as the number. Maybe I should make it clear that the population is approximate. Also, adding a note about it being approximate might be helpful. \n\nLet me put it all together. The JSON should have the capital and population. I\'ll write it out, ensuring correct syntax with quotes and commas. Also, the note explaining that the population is approximate and subject to change. That way, the user knows that the figure might not be exact and should check the latest data if needed. Yep, that should cover it.\n</think>\n\nHere is the information and population of the capital of France in JSON format:\n\n```json\n{\n  "capital": "Paris",\n  "population": 2165000,\n  "note": "The population figure is approximate and may vary slightly depending on the source and year."\n}\n```', 'output_ids': [32313, 11, 773, 279, 1196, 4588, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 88190, 11, 1156, 3166, 358, 1184, 311, 7071, 429, 700, 13, 358, 1414, 429, 279, 6722, 315, 9625, 374, 12095, 13, 4695, 11, 358, 1184, 311, 1477, 279, 1429, 3213, 7042, 7071, 13, 358, 19091, 429, 7042, 5109, 646, 2297, 11, 5310, 304, 264, 2409, 3283, 1075, 12095, 13, 6771, 752, 1490, 11, 279, 1537, 882, 358, 10067, 11, 12095, 1030, 2163, 220, 17, 3526, 1251, 11, 714, 429, 2578, 387, 264, 2699, 40526, 13, 13824, 11, 358, 1744, 304, 220, 17, 15, 16, 22, 11, 12095, 1030, 911, 220, 17, 11, 16, 21, 20, 11, 15, 15, 15, 1251, 11, 2670, 5019, 504, 49751, 311, 279, 28820, 13, 1988, 358, 1265, 1281, 2704, 429, 594, 279, 1482, 7071, 13, 10696, 358, 646, 1779, 264, 14720, 2530, 311, 7683, 13, 4710, 14190, 11, 358, 1265, 1083, 2908, 2670, 279, 57406, 3082, 1576, 7025, 429, 5646, 14590, 46913, 323, 429, 646, 4484, 279, 7042, 5080, 13, 1988, 279, 1196, 4588, 369, 279, 6722, 11, 773, 12095, 6169, 11, 537, 279, 57406, 3082, 13, 2014, 387, 6092, 11, 358, 3278, 3410, 2176, 13, 358, 1083, 6099, 429, 12095, 374, 264, 3644, 3283, 11, 773, 279, 7042, 2578, 387, 7826, 264, 2699, 1817, 1042, 11, 7196, 2163, 220, 18, 15, 11, 15, 15, 15, 803, 13, 1988, 2474, 279, 4734, 1482, 1372, 374, 2989, 11, 358, 1265, 13837, 429, 438, 315, 220, 17, 15, 17, 18, 11, 432, 594, 13187, 220, 17, 11, 16, 21, 20, 11, 15, 15, 15, 11, 8365, 38288, 53543, 264, 2699, 13, 4710, 40, 1265, 5944, 279, 4226, 304, 4718, 11, 438, 279, 1196, 11223, 13, 2938, 3363, 264, 1376, 19083, 6716, 448, 330, 65063, 1, 438, 330, 59604, 1, 323, 330, 44441, 1, 438, 279, 1372, 13, 10696, 358, 1265, 1281, 432, 2797, 429, 279, 7042, 374, 44868, 13, 7281, 11, 7842, 264, 5185, 911, 432, 1660, 44868, 2578, 387, 10950, 13, 4710, 10061, 752, 2182, 432, 678, 3786, 13, 576, 4718, 1265, 614, 279, 6722, 323, 7042, 13, 358, 3278, 3270, 432, 700, 11, 22573, 4396, 19482, 448, 17194, 323, 76602, 13, 7281, 11, 279, 5185, 25021, 429, 279, 7042, 374, 44868, 323, 3832, 311, 2297, 13, 2938, 1616, 11, 279, 1196, 8788, 429, 279, 7071, 2578, 537, 387, 4734, 323, 1265, 1779, 279, 5535, 821, 421, 4362, 13, 84194, 11, 429, 1265, 3421, 432, 624, 151649, 271, 8420, 374, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 1447, 73594, 2236, 198, 515, 220, 330, 65063, 788, 330, 59604, 756, 220, 330, 44441, 788, 220, 17, 16, 21, 20, 15, 15, 15, 345, 220, 330, 9974, 788, 330, 785, 7042, 7071, 374, 44868, 323, 1231, 13289, 10078, 11649, 389, 279, 2530, 323, 1042, 10040, 532, 73594, 151643], 'meta_info': {'id': '5fd82c0cfeb94329ba88a44fddc8a53a', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 475, 'cached_tokens': 22, 'cached_tokens_details': {'device': 22, 'host': 0}, 'dp_rank': None, 'e2e_latency': 5.3849088489077985, 'response_sent_to_client_ts': 1775280622.4923382}}</strong>



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

    Multi-thread loading shards:  50% Completed | 1/2 [00:00<00:00,  1.33it/s]

    Multi-thread loading shards: 100% Completed | 2/2 [00:01<00:00,  1.03s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:01<00:00,  1.01it/s]


    2026-04-04 05:30:40,234 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 05:30:40] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:55,  3.07s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:55,  3.07s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:23,  1.49s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:23,  1.49s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:49,  1.11it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:49,  1.11it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:33,  1.63it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:33,  1.63it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:03<00:23,  2.23it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:03<00:23,  2.23it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.87it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.87it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.61it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.61it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.40it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.40it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.28it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.28it/s]

    Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.28it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:06,  6.97it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:06,  6.97it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:06,  6.97it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:04<00:05,  8.46it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:04<00:05,  8.46it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:04<00:05,  8.46it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:04<00:04, 10.08it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:04<00:04, 10.08it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:05<00:04, 10.08it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:03, 12.02it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:03, 12.02it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:03, 12.02it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:05<00:03, 12.02it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 15.32it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 15.32it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 15.32it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 15.32it/s]

    Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:05<00:02, 15.32it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:05<00:01, 20.40it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:05<00:01, 20.40it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:05<00:01, 20.40it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:05<00:01, 20.40it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:05<00:01, 20.40it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:05<00:01, 20.40it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:05<00:01, 26.98it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:05<00:01, 26.98it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:05<00:01, 26.98it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:05<00:01, 26.98it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:05<00:01, 26.98it/s]

    Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:05<00:01, 26.98it/s]Compiling num tokens (num_tokens=320):  50%|█████     | 29/58 [00:05<00:01, 26.98it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:05<00:00, 34.48it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:05<00:00, 34.48it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:05<00:00, 34.48it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:05<00:00, 34.48it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:05<00:00, 34.48it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:05<00:00, 34.48it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:05<00:00, 34.48it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:05<00:00, 39.98it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:05<00:00, 39.98it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:05<00:00, 39.98it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:05<00:00, 39.98it/s]

    Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:05<00:00, 39.98it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:05<00:00, 39.98it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:05<00:00, 39.98it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:05<00:00, 43.27it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:05<00:00, 43.27it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:05<00:00, 43.27it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:05<00:00, 43.27it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:05<00:00, 43.27it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:05<00:00, 43.27it/s]Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:05<00:00, 43.27it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:05<00:00, 47.51it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:05<00:00, 47.51it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:05<00:00, 47.51it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:05<00:00, 47.51it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:05<00:00, 47.51it/s] 

    Compiling num tokens (num_tokens=4):  91%|█████████▏| 53/58 [00:05<00:00, 47.51it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:05<00:00,  9.79it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=100.67 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=100.67 GB):   2%|▏         | 1/58 [00:00<00:19,  2.95it/s]Capturing num tokens (num_tokens=7680 avail_mem=99.96 GB):   2%|▏         | 1/58 [00:00<00:19,  2.95it/s] 

    Capturing num tokens (num_tokens=7680 avail_mem=99.96 GB):   3%|▎         | 2/58 [00:00<00:19,  2.92it/s]Capturing num tokens (num_tokens=7168 avail_mem=99.96 GB):   3%|▎         | 2/58 [00:00<00:19,  2.92it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=99.96 GB):   5%|▌         | 3/58 [00:01<00:22,  2.47it/s]Capturing num tokens (num_tokens=6656 avail_mem=99.97 GB):   5%|▌         | 3/58 [00:01<00:22,  2.47it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=99.97 GB):   7%|▋         | 4/58 [00:01<00:18,  2.89it/s]Capturing num tokens (num_tokens=6144 avail_mem=99.99 GB):   7%|▋         | 4/58 [00:01<00:18,  2.89it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=99.99 GB):   9%|▊         | 5/58 [00:01<00:15,  3.40it/s]Capturing num tokens (num_tokens=5632 avail_mem=99.99 GB):   9%|▊         | 5/58 [00:01<00:15,  3.40it/s]Capturing num tokens (num_tokens=5632 avail_mem=99.99 GB):  10%|█         | 6/58 [00:01<00:13,  3.94it/s]Capturing num tokens (num_tokens=5120 avail_mem=99.99 GB):  10%|█         | 6/58 [00:01<00:13,  3.94it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=99.99 GB):  12%|█▏        | 7/58 [00:01<00:11,  4.46it/s]Capturing num tokens (num_tokens=4608 avail_mem=100.00 GB):  12%|█▏        | 7/58 [00:01<00:11,  4.46it/s]Capturing num tokens (num_tokens=4608 avail_mem=100.00 GB):  14%|█▍        | 8/58 [00:02<00:09,  5.02it/s]Capturing num tokens (num_tokens=4096 avail_mem=94.69 GB):  14%|█▍        | 8/58 [00:02<00:09,  5.02it/s] 

    Capturing num tokens (num_tokens=4096 avail_mem=94.69 GB):  16%|█▌        | 9/58 [00:02<00:08,  5.57it/s]Capturing num tokens (num_tokens=3840 avail_mem=85.67 GB):  16%|█▌        | 9/58 [00:02<00:08,  5.57it/s]Capturing num tokens (num_tokens=3840 avail_mem=85.67 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.13it/s]Capturing num tokens (num_tokens=3584 avail_mem=85.67 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.13it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=85.67 GB):  19%|█▉        | 11/58 [00:02<00:07,  6.70it/s]Capturing num tokens (num_tokens=3328 avail_mem=85.67 GB):  19%|█▉        | 11/58 [00:02<00:07,  6.70it/s]Capturing num tokens (num_tokens=3328 avail_mem=85.67 GB):  21%|██        | 12/58 [00:02<00:06,  7.21it/s]Capturing num tokens (num_tokens=3072 avail_mem=85.63 GB):  21%|██        | 12/58 [00:02<00:06,  7.21it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=85.63 GB):  22%|██▏       | 13/58 [00:02<00:05,  7.61it/s]Capturing num tokens (num_tokens=2816 avail_mem=85.63 GB):  22%|██▏       | 13/58 [00:02<00:05,  7.61it/s]Capturing num tokens (num_tokens=2816 avail_mem=85.63 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.17it/s]Capturing num tokens (num_tokens=2560 avail_mem=85.63 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.17it/s]Capturing num tokens (num_tokens=2304 avail_mem=85.63 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.17it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=85.63 GB):  28%|██▊       | 16/58 [00:03<00:04,  9.07it/s]Capturing num tokens (num_tokens=2048 avail_mem=85.63 GB):  28%|██▊       | 16/58 [00:03<00:04,  9.07it/s]Capturing num tokens (num_tokens=2048 avail_mem=85.63 GB):  29%|██▉       | 17/58 [00:03<00:04,  8.26it/s]Capturing num tokens (num_tokens=1792 avail_mem=85.63 GB):  29%|██▉       | 17/58 [00:03<00:04,  8.26it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=85.63 GB):  31%|███       | 18/58 [00:03<00:04,  8.07it/s]Capturing num tokens (num_tokens=1536 avail_mem=85.62 GB):  31%|███       | 18/58 [00:03<00:04,  8.07it/s]Capturing num tokens (num_tokens=1536 avail_mem=85.62 GB):  33%|███▎      | 19/58 [00:03<00:04,  8.10it/s]Capturing num tokens (num_tokens=1280 avail_mem=85.62 GB):  33%|███▎      | 19/58 [00:03<00:04,  8.10it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=85.62 GB):  34%|███▍      | 20/58 [00:03<00:04,  8.53it/s]Capturing num tokens (num_tokens=1024 avail_mem=85.62 GB):  34%|███▍      | 20/58 [00:03<00:04,  8.53it/s]Capturing num tokens (num_tokens=960 avail_mem=85.62 GB):  34%|███▍      | 20/58 [00:03<00:04,  8.53it/s] Capturing num tokens (num_tokens=960 avail_mem=85.62 GB):  38%|███▊      | 22/58 [00:03<00:03, 10.74it/s]Capturing num tokens (num_tokens=896 avail_mem=85.62 GB):  38%|███▊      | 22/58 [00:03<00:03, 10.74it/s]Capturing num tokens (num_tokens=832 avail_mem=85.61 GB):  38%|███▊      | 22/58 [00:03<00:03, 10.74it/s]Capturing num tokens (num_tokens=768 avail_mem=85.61 GB):  38%|███▊      | 22/58 [00:03<00:03, 10.74it/s]

    Capturing num tokens (num_tokens=768 avail_mem=85.61 GB):  43%|████▎     | 25/58 [00:03<00:02, 15.03it/s]Capturing num tokens (num_tokens=704 avail_mem=85.60 GB):  43%|████▎     | 25/58 [00:03<00:02, 15.03it/s]Capturing num tokens (num_tokens=640 avail_mem=85.60 GB):  43%|████▎     | 25/58 [00:03<00:02, 15.03it/s]Capturing num tokens (num_tokens=576 avail_mem=85.60 GB):  43%|████▎     | 25/58 [00:03<00:02, 15.03it/s]Capturing num tokens (num_tokens=576 avail_mem=85.60 GB):  48%|████▊     | 28/58 [00:03<00:01, 18.41it/s]Capturing num tokens (num_tokens=512 avail_mem=85.59 GB):  48%|████▊     | 28/58 [00:03<00:01, 18.41it/s]Capturing num tokens (num_tokens=480 avail_mem=85.59 GB):  48%|████▊     | 28/58 [00:03<00:01, 18.41it/s]Capturing num tokens (num_tokens=448 avail_mem=85.59 GB):  48%|████▊     | 28/58 [00:03<00:01, 18.41it/s]Capturing num tokens (num_tokens=416 avail_mem=85.59 GB):  48%|████▊     | 28/58 [00:03<00:01, 18.41it/s]

    Capturing num tokens (num_tokens=416 avail_mem=85.59 GB):  55%|█████▌    | 32/58 [00:03<00:01, 23.22it/s]Capturing num tokens (num_tokens=384 avail_mem=85.58 GB):  55%|█████▌    | 32/58 [00:03<00:01, 23.22it/s]Capturing num tokens (num_tokens=352 avail_mem=85.58 GB):  55%|█████▌    | 32/58 [00:04<00:01, 23.22it/s]Capturing num tokens (num_tokens=320 avail_mem=85.57 GB):  55%|█████▌    | 32/58 [00:04<00:01, 23.22it/s]Capturing num tokens (num_tokens=288 avail_mem=85.57 GB):  55%|█████▌    | 32/58 [00:04<00:01, 23.22it/s]Capturing num tokens (num_tokens=288 avail_mem=85.57 GB):  62%|██████▏   | 36/58 [00:04<00:00, 27.05it/s]Capturing num tokens (num_tokens=256 avail_mem=85.56 GB):  62%|██████▏   | 36/58 [00:04<00:00, 27.05it/s]Capturing num tokens (num_tokens=240 avail_mem=85.56 GB):  62%|██████▏   | 36/58 [00:04<00:00, 27.05it/s]Capturing num tokens (num_tokens=224 avail_mem=85.56 GB):  62%|██████▏   | 36/58 [00:04<00:00, 27.05it/s]

    Capturing num tokens (num_tokens=224 avail_mem=85.56 GB):  67%|██████▋   | 39/58 [00:04<00:00, 20.82it/s]Capturing num tokens (num_tokens=208 avail_mem=107.15 GB):  67%|██████▋   | 39/58 [00:04<00:00, 20.82it/s]Capturing num tokens (num_tokens=192 avail_mem=107.15 GB):  67%|██████▋   | 39/58 [00:04<00:00, 20.82it/s]Capturing num tokens (num_tokens=176 avail_mem=107.15 GB):  67%|██████▋   | 39/58 [00:04<00:00, 20.82it/s]Capturing num tokens (num_tokens=160 avail_mem=107.14 GB):  67%|██████▋   | 39/58 [00:04<00:00, 20.82it/s]Capturing num tokens (num_tokens=160 avail_mem=107.14 GB):  74%|███████▍  | 43/58 [00:04<00:00, 24.97it/s]Capturing num tokens (num_tokens=144 avail_mem=107.14 GB):  74%|███████▍  | 43/58 [00:04<00:00, 24.97it/s]Capturing num tokens (num_tokens=128 avail_mem=107.15 GB):  74%|███████▍  | 43/58 [00:04<00:00, 24.97it/s]Capturing num tokens (num_tokens=112 avail_mem=107.15 GB):  74%|███████▍  | 43/58 [00:04<00:00, 24.97it/s]Capturing num tokens (num_tokens=96 avail_mem=107.14 GB):  74%|███████▍  | 43/58 [00:04<00:00, 24.97it/s] 

    Capturing num tokens (num_tokens=96 avail_mem=107.14 GB):  81%|████████  | 47/58 [00:04<00:00, 28.18it/s]Capturing num tokens (num_tokens=80 avail_mem=107.13 GB):  81%|████████  | 47/58 [00:04<00:00, 28.18it/s]Capturing num tokens (num_tokens=64 avail_mem=107.13 GB):  81%|████████  | 47/58 [00:04<00:00, 28.18it/s]Capturing num tokens (num_tokens=48 avail_mem=107.13 GB):  81%|████████  | 47/58 [00:04<00:00, 28.18it/s]Capturing num tokens (num_tokens=32 avail_mem=107.12 GB):  81%|████████  | 47/58 [00:04<00:00, 28.18it/s]Capturing num tokens (num_tokens=32 avail_mem=107.12 GB):  88%|████████▊ | 51/58 [00:04<00:00, 30.97it/s]Capturing num tokens (num_tokens=28 avail_mem=107.12 GB):  88%|████████▊ | 51/58 [00:04<00:00, 30.97it/s]Capturing num tokens (num_tokens=24 avail_mem=107.12 GB):  88%|████████▊ | 51/58 [00:04<00:00, 30.97it/s]Capturing num tokens (num_tokens=20 avail_mem=107.12 GB):  88%|████████▊ | 51/58 [00:04<00:00, 30.97it/s]Capturing num tokens (num_tokens=16 avail_mem=107.11 GB):  88%|████████▊ | 51/58 [00:04<00:00, 30.97it/s]

    Capturing num tokens (num_tokens=16 avail_mem=107.11 GB):  95%|█████████▍| 55/58 [00:04<00:00, 32.98it/s]Capturing num tokens (num_tokens=12 avail_mem=107.11 GB):  95%|█████████▍| 55/58 [00:04<00:00, 32.98it/s]Capturing num tokens (num_tokens=8 avail_mem=107.11 GB):  95%|█████████▍| 55/58 [00:04<00:00, 32.98it/s] Capturing num tokens (num_tokens=4 avail_mem=107.10 GB):  95%|█████████▍| 55/58 [00:04<00:00, 32.98it/s]Capturing num tokens (num_tokens=4 avail_mem=107.10 GB): 100%|██████████| 58/58 [00:04<00:00, 12.05it/s]


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
    Generated text: Paris is the capital of France
    ===============================
    Prompt: Give me the information of the capital of Germany.
    Generated text: Berlin is the capital of Germany
    ===============================
    Prompt: Give me the information of the capital of Italy.
    Generated text: London is the capital of England


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
    
    Generated text: Okay, so I need to find the information and population of the capital of France. Let me start by thinking about what the capital of France is. I remember that Paris is the capital city. So, the capital is Paris.
    
    Next, I need to figure out the population of Paris. I think Paris is the most populous city in France, but I'm not exactly sure about the exact number. I remember hearing that it's around 2 million people, but I'm not 100% certain. Maybe I should double-check that. Let me think about recent years. I believe it's been increasing because Paris is a major city. So, perhaps it's around 2.1 million now.
    
    Now, I need to present this information in JSON format. JSON stands for JavaScript Object Notation, and it's a way to structure data. The format usually involves key-value pairs. So, I'll need to create an object with a key for the capital and another for the population.
    
    Let me outline what the JSON should look like. It should have a "capital" key with the value "Paris" and a "population" key with the number 2100000. I should make sure the commas and quotes are correct because JSON syntax is very particular.
    
    Wait, is the population an exact number or an approximate? I think it's an approximate value, so I'll include it as a number without quotes. So, the JSON would be:
    
    {
      "capital": "Paris",
      "population": 2100000
    }
    
    I should also check if there are any other details needed, but since the user only asked for the capital and its population, I think this covers it. I'll make sure the JSON is properly formatted with the correct syntax, using quotes around strings and commas between key-value pairs.
    </think>
    
    The JSON format containing the information about the capital of France and its population is as follows:
    
    ```json
    {
      "capital": "Paris",
      "population": 2100000
    }
    ```



```python
llm.shutdown()
```
