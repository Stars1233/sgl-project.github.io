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

    Multi-thread loading shards:  50% Completed | 1/2 [00:01<00:01,  1.04s/it]

    Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.18s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.16s/it]


    2026-04-03 21:24:15,674 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 21:24:15] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:56,  3.09s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:56,  3.09s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:27,  1.56s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:27,  1.56s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:52,  1.05it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:52,  1.05it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:35,  1.51it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:35,  1.51it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:25,  2.06it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:25,  2.06it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:19,  2.68it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:19,  2.68it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:15,  3.34it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:15,  3.34it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:13,  3.78it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:13,  3.78it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:11,  4.32it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:11,  4.32it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:04<00:09,  4.93it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:04<00:09,  4.93it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:05<00:09,  4.87it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:05<00:09,  4.87it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:05<00:09,  4.81it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:05<00:09,  4.81it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:05<00:09,  4.90it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:05<00:09,  4.90it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:05<00:08,  5.04it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:05<00:08,  5.04it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:05<00:08,  5.35it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:05<00:08,  5.35it/s]

    Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:06<00:07,  5.74it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:06<00:07,  5.74it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:06<00:06,  6.32it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:06<00:06,  6.32it/s]

    Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:06<00:05,  6.86it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:06<00:05,  6.86it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:06<00:05,  7.45it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:06<00:05,  7.45it/s]

    Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:06<00:05,  7.45it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:06<00:04,  8.87it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:06<00:04,  8.87it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:06<00:04,  8.87it/s]

    Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:06<00:03, 10.64it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:06<00:03, 10.64it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:06<00:03, 10.64it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:06<00:02, 12.09it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:06<00:02, 12.09it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:06<00:02, 12.09it/s]

    Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:06<00:02, 13.42it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:06<00:02, 13.42it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:07<00:02, 13.42it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:07<00:01, 14.81it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:07<00:01, 14.81it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:07<00:01, 14.81it/s]

    Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:07<00:01, 14.81it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:07<00:01, 16.99it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:07<00:01, 16.99it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:07<00:01, 16.99it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:07<00:01, 16.99it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:07<00:01, 18.47it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:07<00:01, 18.47it/s]

    Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:07<00:01, 18.47it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:07<00:01, 18.47it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:07<00:01, 19.57it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:07<00:01, 19.57it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:07<00:01, 19.57it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:07<00:01, 19.57it/s]

    Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:07<00:00, 21.12it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:07<00:00, 21.12it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:07<00:00, 21.12it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:07<00:00, 21.12it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:07<00:00, 21.59it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:07<00:00, 21.59it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:07<00:00, 21.59it/s]

    Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:07<00:00, 21.59it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:07<00:00, 23.20it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:07<00:00, 23.20it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:07<00:00, 23.20it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:07<00:00, 23.20it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:07<00:00, 24.01it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:07<00:00, 24.01it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:08<00:00, 24.01it/s]

    Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:08<00:00, 24.01it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:08<00:00, 24.01it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:08<00:00, 26.69it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:08<00:00, 26.69it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:08<00:00, 26.69it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:08<00:00, 26.69it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:08<00:00, 26.69it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:08<00:00, 29.30it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:08<00:00,  7.05it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=100.26 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=100.26 GB):   2%|▏         | 1/58 [00:00<00:34,  1.63it/s]Capturing num tokens (num_tokens=7680 avail_mem=100.22 GB):   2%|▏         | 1/58 [00:00<00:34,  1.63it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=100.22 GB):   3%|▎         | 2/58 [00:01<00:30,  1.85it/s]Capturing num tokens (num_tokens=7168 avail_mem=100.18 GB):   3%|▎         | 2/58 [00:01<00:30,  1.85it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=100.18 GB):   5%|▌         | 3/58 [00:01<00:25,  2.16it/s]Capturing num tokens (num_tokens=6656 avail_mem=100.19 GB):   5%|▌         | 3/58 [00:01<00:25,  2.16it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=100.19 GB):   7%|▋         | 4/58 [00:01<00:23,  2.29it/s]Capturing num tokens (num_tokens=6144 avail_mem=100.18 GB):   7%|▋         | 4/58 [00:01<00:23,  2.29it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=100.18 GB):   9%|▊         | 5/58 [00:02<00:19,  2.68it/s]Capturing num tokens (num_tokens=5632 avail_mem=100.17 GB):   9%|▊         | 5/58 [00:02<00:19,  2.68it/s]Capturing num tokens (num_tokens=5632 avail_mem=100.17 GB):  10%|█         | 6/58 [00:02<00:16,  3.23it/s]Capturing num tokens (num_tokens=5120 avail_mem=100.17 GB):  10%|█         | 6/58 [00:02<00:16,  3.23it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=100.17 GB):  12%|█▏        | 7/58 [00:02<00:15,  3.21it/s]Capturing num tokens (num_tokens=4608 avail_mem=99.14 GB):  12%|█▏        | 7/58 [00:02<00:15,  3.21it/s] 

    Capturing num tokens (num_tokens=4608 avail_mem=99.14 GB):  14%|█▍        | 8/58 [00:02<00:15,  3.23it/s]Capturing num tokens (num_tokens=4096 avail_mem=100.15 GB):  14%|█▍        | 8/58 [00:02<00:15,  3.23it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=100.15 GB):  16%|█▌        | 9/58 [00:03<00:14,  3.35it/s]Capturing num tokens (num_tokens=3840 avail_mem=99.33 GB):  16%|█▌        | 9/58 [00:03<00:14,  3.35it/s] 

    Capturing num tokens (num_tokens=3840 avail_mem=99.33 GB):  17%|█▋        | 10/58 [00:03<00:14,  3.38it/s]Capturing num tokens (num_tokens=3584 avail_mem=100.16 GB):  17%|█▋        | 10/58 [00:03<00:14,  3.38it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=100.16 GB):  19%|█▉        | 11/58 [00:03<00:13,  3.56it/s]Capturing num tokens (num_tokens=3328 avail_mem=99.39 GB):  19%|█▉        | 11/58 [00:03<00:13,  3.56it/s] 

    Capturing num tokens (num_tokens=3328 avail_mem=99.39 GB):  21%|██        | 12/58 [00:04<00:13,  3.43it/s]Capturing num tokens (num_tokens=3072 avail_mem=100.16 GB):  21%|██        | 12/58 [00:04<00:13,  3.43it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=100.16 GB):  22%|██▏       | 13/58 [00:04<00:12,  3.52it/s]Capturing num tokens (num_tokens=2816 avail_mem=99.45 GB):  22%|██▏       | 13/58 [00:04<00:12,  3.52it/s] 

    Capturing num tokens (num_tokens=2816 avail_mem=99.45 GB):  24%|██▍       | 14/58 [00:04<00:11,  3.83it/s]Capturing num tokens (num_tokens=2560 avail_mem=100.16 GB):  24%|██▍       | 14/58 [00:04<00:11,  3.83it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=100.16 GB):  26%|██▌       | 15/58 [00:04<00:10,  4.10it/s]Capturing num tokens (num_tokens=2304 avail_mem=99.50 GB):  26%|██▌       | 15/58 [00:04<00:10,  4.10it/s] Capturing num tokens (num_tokens=2304 avail_mem=99.50 GB):  28%|██▊       | 16/58 [00:04<00:09,  4.46it/s]Capturing num tokens (num_tokens=2048 avail_mem=100.16 GB):  28%|██▊       | 16/58 [00:04<00:09,  4.46it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=100.16 GB):  29%|██▉       | 17/58 [00:05<00:08,  4.79it/s]Capturing num tokens (num_tokens=1792 avail_mem=99.52 GB):  29%|██▉       | 17/58 [00:05<00:08,  4.79it/s] Capturing num tokens (num_tokens=1792 avail_mem=99.52 GB):  31%|███       | 18/58 [00:05<00:07,  5.22it/s]Capturing num tokens (num_tokens=1536 avail_mem=100.13 GB):  31%|███       | 18/58 [00:05<00:07,  5.22it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=100.13 GB):  33%|███▎      | 19/58 [00:05<00:06,  5.70it/s]Capturing num tokens (num_tokens=1280 avail_mem=99.58 GB):  33%|███▎      | 19/58 [00:05<00:06,  5.70it/s] Capturing num tokens (num_tokens=1280 avail_mem=99.58 GB):  34%|███▍      | 20/58 [00:05<00:06,  6.20it/s]Capturing num tokens (num_tokens=1024 avail_mem=100.13 GB):  34%|███▍      | 20/58 [00:05<00:06,  6.20it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=100.13 GB):  36%|███▌      | 21/58 [00:05<00:05,  6.95it/s]Capturing num tokens (num_tokens=960 avail_mem=99.63 GB):  36%|███▌      | 21/58 [00:05<00:05,  6.95it/s]  Capturing num tokens (num_tokens=960 avail_mem=99.63 GB):  38%|███▊      | 22/58 [00:05<00:04,  7.20it/s]Capturing num tokens (num_tokens=896 avail_mem=100.13 GB):  38%|███▊      | 22/58 [00:05<00:04,  7.20it/s]

    Capturing num tokens (num_tokens=832 avail_mem=99.69 GB):  38%|███▊      | 22/58 [00:05<00:04,  7.20it/s] Capturing num tokens (num_tokens=832 avail_mem=99.69 GB):  41%|████▏     | 24/58 [00:05<00:04,  8.19it/s]Capturing num tokens (num_tokens=768 avail_mem=99.68 GB):  41%|████▏     | 24/58 [00:05<00:04,  8.19it/s]

    Capturing num tokens (num_tokens=704 avail_mem=100.12 GB):  41%|████▏     | 24/58 [00:06<00:04,  8.19it/s]Capturing num tokens (num_tokens=704 avail_mem=100.12 GB):  45%|████▍     | 26/58 [00:06<00:03,  9.02it/s]Capturing num tokens (num_tokens=640 avail_mem=99.70 GB):  45%|████▍     | 26/58 [00:06<00:03,  9.02it/s] Capturing num tokens (num_tokens=576 avail_mem=100.11 GB):  45%|████▍     | 26/58 [00:06<00:03,  9.02it/s]

    Capturing num tokens (num_tokens=576 avail_mem=100.11 GB):  48%|████▊     | 28/58 [00:06<00:03,  9.56it/s]Capturing num tokens (num_tokens=512 avail_mem=99.72 GB):  48%|████▊     | 28/58 [00:06<00:03,  9.56it/s] Capturing num tokens (num_tokens=480 avail_mem=100.10 GB):  48%|████▊     | 28/58 [00:06<00:03,  9.56it/s]Capturing num tokens (num_tokens=480 avail_mem=100.10 GB):  52%|█████▏    | 30/58 [00:06<00:02, 10.08it/s]Capturing num tokens (num_tokens=448 avail_mem=99.74 GB):  52%|█████▏    | 30/58 [00:06<00:02, 10.08it/s] 

    Capturing num tokens (num_tokens=416 avail_mem=100.10 GB):  52%|█████▏    | 30/58 [00:06<00:02, 10.08it/s]Capturing num tokens (num_tokens=416 avail_mem=100.10 GB):  55%|█████▌    | 32/58 [00:06<00:02, 10.76it/s]Capturing num tokens (num_tokens=384 avail_mem=99.77 GB):  55%|█████▌    | 32/58 [00:06<00:02, 10.76it/s] Capturing num tokens (num_tokens=352 avail_mem=100.09 GB):  55%|█████▌    | 32/58 [00:06<00:02, 10.76it/s]

    Capturing num tokens (num_tokens=352 avail_mem=100.09 GB):  59%|█████▊    | 34/58 [00:06<00:02, 11.29it/s]Capturing num tokens (num_tokens=320 avail_mem=100.09 GB):  59%|█████▊    | 34/58 [00:06<00:02, 11.29it/s]Capturing num tokens (num_tokens=288 avail_mem=99.81 GB):  59%|█████▊    | 34/58 [00:06<00:02, 11.29it/s] Capturing num tokens (num_tokens=288 avail_mem=99.81 GB):  62%|██████▏   | 36/58 [00:07<00:02, 10.88it/s]Capturing num tokens (num_tokens=256 avail_mem=100.08 GB):  62%|██████▏   | 36/58 [00:07<00:02, 10.88it/s]

    Capturing num tokens (num_tokens=240 avail_mem=99.82 GB):  62%|██████▏   | 36/58 [00:07<00:02, 10.88it/s] Capturing num tokens (num_tokens=240 avail_mem=99.82 GB):  66%|██████▌   | 38/58 [00:07<00:01, 12.10it/s]Capturing num tokens (num_tokens=224 avail_mem=100.07 GB):  66%|██████▌   | 38/58 [00:07<00:01, 12.10it/s]Capturing num tokens (num_tokens=208 avail_mem=100.07 GB):  66%|██████▌   | 38/58 [00:07<00:01, 12.10it/s]Capturing num tokens (num_tokens=208 avail_mem=100.07 GB):  69%|██████▉   | 40/58 [00:07<00:01, 13.11it/s]Capturing num tokens (num_tokens=192 avail_mem=99.87 GB):  69%|██████▉   | 40/58 [00:07<00:01, 13.11it/s] 

    Capturing num tokens (num_tokens=176 avail_mem=100.06 GB):  69%|██████▉   | 40/58 [00:07<00:01, 13.11it/s]Capturing num tokens (num_tokens=176 avail_mem=100.06 GB):  72%|███████▏  | 42/58 [00:07<00:01, 14.29it/s]Capturing num tokens (num_tokens=160 avail_mem=100.06 GB):  72%|███████▏  | 42/58 [00:07<00:01, 14.29it/s]Capturing num tokens (num_tokens=144 avail_mem=99.91 GB):  72%|███████▏  | 42/58 [00:07<00:01, 14.29it/s] Capturing num tokens (num_tokens=128 avail_mem=99.94 GB):  72%|███████▏  | 42/58 [00:07<00:01, 14.29it/s]

    Capturing num tokens (num_tokens=128 avail_mem=99.94 GB):  78%|███████▊  | 45/58 [00:07<00:00, 16.38it/s]Capturing num tokens (num_tokens=112 avail_mem=100.05 GB):  78%|███████▊  | 45/58 [00:07<00:00, 16.38it/s]Capturing num tokens (num_tokens=96 avail_mem=100.04 GB):  78%|███████▊  | 45/58 [00:07<00:00, 16.38it/s] Capturing num tokens (num_tokens=80 avail_mem=100.03 GB):  78%|███████▊  | 45/58 [00:07<00:00, 16.38it/s]Capturing num tokens (num_tokens=80 avail_mem=100.03 GB):  83%|████████▎ | 48/58 [00:07<00:00, 17.61it/s]Capturing num tokens (num_tokens=64 avail_mem=99.96 GB):  83%|████████▎ | 48/58 [00:07<00:00, 17.61it/s] Capturing num tokens (num_tokens=48 avail_mem=99.94 GB):  83%|████████▎ | 48/58 [00:07<00:00, 17.61it/s]

    Capturing num tokens (num_tokens=32 avail_mem=100.01 GB):  83%|████████▎ | 48/58 [00:07<00:00, 17.61it/s]Capturing num tokens (num_tokens=32 avail_mem=100.01 GB):  88%|████████▊ | 51/58 [00:07<00:00, 19.60it/s]Capturing num tokens (num_tokens=28 avail_mem=100.01 GB):  88%|████████▊ | 51/58 [00:07<00:00, 19.60it/s]Capturing num tokens (num_tokens=24 avail_mem=100.00 GB):  88%|████████▊ | 51/58 [00:07<00:00, 19.60it/s]Capturing num tokens (num_tokens=20 avail_mem=99.99 GB):  88%|████████▊ | 51/58 [00:07<00:00, 19.60it/s] Capturing num tokens (num_tokens=20 avail_mem=99.99 GB):  93%|█████████▎| 54/58 [00:07<00:00, 20.72it/s]Capturing num tokens (num_tokens=16 avail_mem=99.98 GB):  93%|█████████▎| 54/58 [00:07<00:00, 20.72it/s]

    Capturing num tokens (num_tokens=12 avail_mem=99.98 GB):  93%|█████████▎| 54/58 [00:07<00:00, 20.72it/s]Capturing num tokens (num_tokens=8 avail_mem=99.97 GB):  93%|█████████▎| 54/58 [00:07<00:00, 20.72it/s] Capturing num tokens (num_tokens=8 avail_mem=99.97 GB):  98%|█████████▊| 57/58 [00:08<00:00, 21.61it/s]Capturing num tokens (num_tokens=4 avail_mem=99.96 GB):  98%|█████████▊| 57/58 [00:08<00:00, 21.61it/s]Capturing num tokens (num_tokens=4 avail_mem=99.96 GB): 100%|██████████| 58/58 [00:08<00:00,  7.18it/s]


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


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France and its population. I know that the capital of France is Paris, but I'm not exactly sure about the current population numbers. I remember that Paris is a very big city, but I think it's not the largest in the world. Maybe around 20 million? I'm not certain, though. I should check if that's correct.<br><br>Wait, I think the population might have changed a bit over the years. I recall reading somewhere that Paris has grown a lot, especially with the influx of people moving there for work. But I'm not sure if it's exactly 21 million or maybe a bit more. I should look up the latest data to confirm.<br><br>I also wonder if the population figure includes just the city proper or the entire metropolitan area. Sometimes, people talk about the metro area, which can be much larger. But the question specifically asks for the population of the capital, so I think it refers to the city limits. Still, I should make sure.<br><br>Another thing to consider is that population figures can vary depending on the source. Some might cite estimates from government agencies, while others might use more recent data from censuses or surveys. I should find a reliable source to get the most accurate number.<br><br>I think the population of Paris is around 21 million, but I'm not 100% sure. Maybe I should think about other major cities in France to compare. For example, Lyon is another big city, but it's much smaller. I believe its population is around 1.2 million. That gives me a sense that Paris is significantly larger.<br><br>Also, considering the economic activities in Paris, like the fashion industry and the entertainment sector, it makes sense that it's the capital. The concentration of businesses and people there would contribute to a larger population. But again, I'm not certain about the exact number.<br><br>I should also think about how population figures can change over time due to births, deaths, and migration. If there's been a lot of immigration into Paris, the population would be higher. Conversely, if there's been a lot of people leaving, it might be lower. I don't have data on that, so I'll stick with the general estimate I have.<br><br>In summary, I'm pretty confident that Paris is the capital of France, and its population is around 21 million. However, to be precise, I should verify this information to ensure accuracy.<br><br><br>content: {<br><br>"name": "Paris",<br>"population": 21420000<br>}</strong>


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


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France and its population. I know that the capital of France is Paris, but I'm not exactly sure about the current population numbers. I remember that Paris is a very big city, but I think it's not the largest in the world. Maybe around 20 million? I'm not certain, though. I should check if that's correct.<br><br>Wait, I think the population might have changed a bit over the years. I recall reading somewhere that Paris has grown a lot, especially with the influx of people moving there for work. But I'm not sure if it's exactly 21 million or maybe a bit more. I should look up the latest data to confirm.<br><br>I also wonder if the population figure includes just the city proper or the entire metropolitan area. Sometimes, people talk about the metro area, which can be much larger. But the question specifically asks for the population of the capital, so I think it refers to the city limits. Still, I should make sure.<br><br>Another thing to consider is that population figures can vary depending on the source. Some might cite estimates from government agencies, while others might use more recent data from censuses or surveys. I should find a reliable source to get the most accurate number.<br><br>I think the population of Paris is around 21 million, but I'm not 100% sure. Maybe I should think about other major cities in France to compare. For example, Lyon is another big city, but it's much smaller. I believe its population is around 1.2 million. That gives me a sense that Paris is significantly larger.<br><br>Also, considering the economic activities in Paris, like the fashion industry and the entertainment sector, it makes sense that it's the capital. The concentration of businesses and people there would contribute to a larger population. But again, I'm not certain about the exact number.<br><br>I should also think about how population figures can change over time due to births, deaths, and migration. If there's been a lot of immigration into Paris, the population would be higher. Conversely, if there's been a lot of people leaving, it might be lower. I don't have data on that, so I'll stick with the general estimate I have.<br><br>In summary, I'm pretty confident that Paris is the capital of France, and its population is around 21 million. However, to be precise, I should verify this information to ensure accuracy.<br><br><br>content: {<br><br>"name": "Paris",<br>"population": 21420000<br>}</strong>


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


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France and its population. I know that the capital of France is Paris, but I'm not entirely sure about the population number. I think it's a big city, so maybe around 3 million? But I'm not certain. I should probably double-check that. Maybe I can recall that Paris is one of the largest cities in Europe, so 3.5 million sounds about right. I don't think it's more than that because I've heard it's a major tourist attraction but not the largest in the world. So, I'll go with Paris having a population of approximately 3.5 million.<br><br><br>content: Paris is the capital of France</strong>


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


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France. Hmm, I remember learning a bit about France in school, but I'm not 100% sure. Let me think. I know that Paris is a major city in France, and it's often referred to as the "City of Light" because of the famous Eiffel Tower. But is it the capital? I think so, but I'm not entirely certain. <br><br>Wait, I also recall that there's another city called Lyon. Isn't that the capital? No, I think I'm mixing things up. Lyon is definitely a significant city in France, known for its gastronomy and being the second-largest city. But I'm pretty sure Paris is the capital. <br><br>Let me try to remember any other capitals I know. London is the capital of the UK, Rome is Italy, Beijing is China, and so on. So, for France, Paris seems to be the one. I think I've heard it referred to as the capital in various contexts, like government buildings, official events, and so on. <br><br>I also remember that the Eiffel Tower is in Paris, and it's a symbol of the country. The French flag has a blue background with white and yellow, and I believe the white part is shaped like the Eiffel Tower, which is in Paris. That makes me more confident that Paris is the capital. <br><br>Another way to think about it is the political aspect. The President of France is based in Paris, right? So that would mean Paris is where the government is located, making it the capital. <br><br>I guess I'm pretty sure now. I don't think I've heard of Lyon being the capital. Maybe I confused it with another country? No, I'm pretty sure it's Paris. So, after all that thinking, I'm confident that the capital of France is Paris.<br><br><br>content: Paris</strong>


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


<strong style='color: #00008B;'>reasoing_content: Alright, so the user is in New York and wants the current date and time along with the weather. I need to figure out how to get this information using the provided functions. <br><br>First, I should use the get_current_date function. The parameters required are the timezone, which in this case is 'America/New_York'. I'll structure the function call with that parameter.<br><br>Next, I need the weather information. For that, I'll use the get_current_weather function. The city is New York, the state is NY, and I'll default to Celsius for the unit since it's a common request without specifying otherwise.<br><br>I should make sure each function call is separate, as per the instructions, so I won't combine them into one. I'll write each function call on its own line, specifying the parameters correctly within JSON format.<br><br>Finally, I'll present the responses clearly, using the specified format with start and end tags for each function call. I'll also add sources to indicate where the information came from, using the functions as the basis.<br><br><br>content: <function=get_current_date>{"timezone": "America/New_York"}</function><br><function=get_current_weather>{"city": "New York", "state": "NY", "unit": "celsius"}</function><br><br>Sources: <br>- get_current_date function call<br>- get_current_weather function call</strong>


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

    {'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down. First, I need to identify what the capital of France is. I know that Paris is the capital, so that\'s the starting point.\n\nNext, I need to find the population of Paris. I remember that Paris is a major city with a large population, but I\'m not exactly sure of the current number. I think it\'s around 2 million, but I should double-check that. Maybe I can recall that it\'s approximately 2,150,000 as of recent estimates.\n\nNow, the user wants this information in JSON format. JSON stands for JavaScript Object Notation, which is a way to structure data. I need to create a JSON object that includes the key "capital" with the value "Paris" and another key "population" with the number I just thought of.\n\nI should make sure the JSON syntax is correct. That means using double quotes for keys and string values, and commas appropriately between key-value pairs. Also, the numbers should be in quotes if they\'re strings, but population is a number, so it should be without quotes.\n\nPutting it all together, the JSON object should look like this: {"capital": "Paris", "population": 2150000}. I should present this clearly so the user can easily understand and use the information.\n\nI wonder if the user needs more details, like the population figure\'s source or the exact year it was recorded. But since they didn\'t ask for that, I\'ll stick to the information requested. Maybe they just need a straightforward data structure for a program or a report.\n\nAlso, considering the user\'s request, they might be a student working on a project, or perhaps someone developing an app that requires capital cities and their populations. Either way, providing accurate and concise data is key.\n\nI should ensure that the JSON is correctly formatted to avoid any errors when the user tries to use it. No markdown, just plain JSON. That way, it\'s easy to copy and paste into their code or document.\n\nIn summary, I identified the capital, found the population number, structured it into a JSON object, and made sure the syntax is correct. I didn\'t overcomplicate it, just provided the necessary information in the requested format.\n</think>{"name": "Paris", "population": 2150000}', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 13, 5512, 11, 358, 1184, 311, 10542, 1128, 279, 6722, 315, 9625, 374, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 279, 5916, 1459, 382, 5847, 11, 358, 1184, 311, 1477, 279, 7042, 315, 12095, 13, 358, 6099, 429, 12095, 374, 264, 3598, 3283, 448, 264, 3460, 7042, 11, 714, 358, 2776, 537, 6896, 2704, 315, 279, 1482, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 1990, 15934, 429, 13, 10696, 358, 646, 19091, 429, 432, 594, 13187, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 438, 315, 3213, 17530, 382, 7039, 11, 279, 1196, 6801, 419, 1995, 304, 4718, 3561, 13, 4718, 13352, 369, 12914, 3002, 2806, 367, 11, 892, 374, 264, 1616, 311, 5944, 821, 13, 358, 1184, 311, 1855, 264, 4718, 1633, 429, 5646, 279, 1376, 330, 65063, 1, 448, 279, 897, 330, 59604, 1, 323, 2441, 1376, 330, 44441, 1, 448, 279, 1372, 358, 1101, 3381, 315, 382, 40, 1265, 1281, 2704, 279, 4718, 19482, 374, 4396, 13, 2938, 3363, 1667, 1990, 17194, 369, 6894, 323, 914, 2750, 11, 323, 76602, 34901, 1948, 1376, 19083, 13530, 13, 7281, 11, 279, 5109, 1265, 387, 304, 17194, 421, 807, 2299, 9069, 11, 714, 7042, 374, 264, 1372, 11, 773, 432, 1265, 387, 2041, 17194, 382, 97904, 432, 678, 3786, 11, 279, 4718, 1633, 1265, 1401, 1075, 419, 25, 5212, 65063, 788, 330, 59604, 497, 330, 44441, 788, 220, 17, 16, 20, 15, 15, 15, 15, 7810, 358, 1265, 3042, 419, 9355, 773, 279, 1196, 646, 6707, 3535, 323, 990, 279, 1995, 382, 40, 5775, 421, 279, 1196, 3880, 803, 3565, 11, 1075, 279, 7042, 7071, 594, 2530, 476, 279, 4734, 1042, 432, 572, 12433, 13, 1988, 2474, 807, 3207, 944, 2548, 369, 429, 11, 358, 3278, 9214, 311, 279, 1995, 11223, 13, 10696, 807, 1101, 1184, 264, 30339, 821, 5944, 369, 264, 2025, 476, 264, 1895, 382, 13394, 11, 12831, 279, 1196, 594, 1681, 11, 807, 2578, 387, 264, 5458, 3238, 389, 264, 2390, 11, 476, 8365, 4325, 11220, 458, 906, 429, 7460, 6722, 9720, 323, 862, 21910, 13, 20988, 1616, 11, 8241, 13382, 323, 63594, 821, 374, 1376, 382, 40, 1265, 5978, 429, 279, 4718, 374, 12440, 23126, 311, 5648, 894, 5975, 979, 279, 1196, 16297, 311, 990, 432, 13, 2308, 50494, 11, 1101, 14396, 4718, 13, 2938, 1616, 11, 432, 594, 4135, 311, 2975, 323, 24937, 1119, 862, 2038, 476, 2197, 382, 641, 12126, 11, 358, 10820, 279, 6722, 11, 1730, 279, 7042, 1372, 11, 32930, 432, 1119, 264, 4718, 1633, 11, 323, 1865, 2704, 279, 19482, 374, 4396, 13, 358, 3207, 944, 916, 5689, 48795, 432, 11, 1101, 3897, 279, 5871, 1995, 304, 279, 11223, 3561, 624, 151649, 4913, 606, 788, 330, 59604, 497, 330, 44441, 788, 220, 17, 16, 20, 15, 15, 15, 15, 92, 151643], 'meta_info': {'id': 'a836da8d6f7c4697b194aaa9cc8ed031', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 500, 'cached_tokens': 1, 'cached_tokens_details': {'device': 1, 'host': 0}, 'dp_rank': None, 'e2e_latency': 5.377470181323588, 'response_sent_to_client_ts': 1775251506.8374677}}



<strong style='color: #00008B;'>reasoing_content: Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down. First, I need to identify what the capital of France is. I know that Paris is the capital, so that's the starting point.<br><br>Next, I need to find the population of Paris. I remember that Paris is a major city with a large population, but I'm not exactly sure of the current number. I think it's around 2 million, but I should double-check that. Maybe I can recall that it's approximately 2,150,000 as of recent estimates.<br><br>Now, the user wants this information in JSON format. JSON stands for JavaScript Object Notation, which is a way to structure data. I need to create a JSON object that includes the key "capital" with the value "Paris" and another key "population" with the number I just thought of.<br><br>I should make sure the JSON syntax is correct. That means using double quotes for keys and string values, and commas appropriately between key-value pairs. Also, the numbers should be in quotes if they're strings, but population is a number, so it should be without quotes.<br><br>Putting it all together, the JSON object should look like this: {"capital": "Paris", "population": 2150000}. I should present this clearly so the user can easily understand and use the information.<br><br>I wonder if the user needs more details, like the population figure's source or the exact year it was recorded. But since they didn't ask for that, I'll stick to the information requested. Maybe they just need a straightforward data structure for a program or a report.<br><br>Also, considering the user's request, they might be a student working on a project, or perhaps someone developing an app that requires capital cities and their populations. Either way, providing accurate and concise data is key.<br><br>I should ensure that the JSON is correctly formatted to avoid any errors when the user tries to use it. No markdown, just plain JSON. That way, it's easy to copy and paste into their code or document.<br><br>In summary, I identified the capital, found the population number, structured it into a JSON object, and made sure the syntax is correct. I didn't overcomplicate it, just provided the necessary information in the requested format.<br><br><br>content: {"name": "Paris", "population": 2150000}</strong>


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


<strong style='color: #00008B;'>{'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down. First, I need to identify what the capital of France is. I know that Paris is the capital, so that\'s the starting point.\n\nNext, I need to find the population of Paris. I remember that Paris is a major city with a large population, but I\'m not exactly sure of the current number. I think it\'s around 2 million, but I should double-check that. Maybe I can recall that it\'s approximately 2,150,000 as of recent estimates.\n\nNow, the user wants this information in JSON format. JSON stands for JavaScript Object Notation, which is a way to structure data. I need to create a JSON object that includes the key "capital" with the value "Paris" and another key "population" with the number I just thought of.\n\nI should make sure the JSON syntax is correct. That means using double quotes for keys and string values, and commas appropriately between key-value pairs. Also, the numbers should be in quotes if they\'re strings, but population is a number, so it should be without quotes.\n\nPutting it all together, the JSON object should look like this: {"capital": "Paris", "population": 2150000}. I should present this clearly so the user can easily understand and use the information.\n\nI wonder if the user needs more details, like the population figure\'s source or the exact year it was recorded. But since they didn\'t ask for that, I\'ll stick to the information requested. Maybe they just need a straightforward data structure for a program or a report.\n\nAlso, considering the user\'s request, they might be a student working on a project, or perhaps someone developing an app that requires capital cities and their populations. Either way, providing accurate and concise data is key.\n\nI should ensure that the JSON is correctly formatted to avoid any errors when the user tries to use it. No markdown, just plain JSON. That way, it\'s easy to copy and paste into their code or document.\n\nIn summary, I identified the capital, found the population number, structured it into a JSON object, and made sure the syntax is correct. I didn\'t overcomplicate it, just provided the necessary information in the requested format.\n</think>{"name": "Paris", "population": 2150000}', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 13, 5512, 11, 358, 1184, 311, 10542, 1128, 279, 6722, 315, 9625, 374, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 279, 5916, 1459, 382, 5847, 11, 358, 1184, 311, 1477, 279, 7042, 315, 12095, 13, 358, 6099, 429, 12095, 374, 264, 3598, 3283, 448, 264, 3460, 7042, 11, 714, 358, 2776, 537, 6896, 2704, 315, 279, 1482, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 1990, 15934, 429, 13, 10696, 358, 646, 19091, 429, 432, 594, 13187, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 438, 315, 3213, 17530, 382, 7039, 11, 279, 1196, 6801, 419, 1995, 304, 4718, 3561, 13, 4718, 13352, 369, 12914, 3002, 2806, 367, 11, 892, 374, 264, 1616, 311, 5944, 821, 13, 358, 1184, 311, 1855, 264, 4718, 1633, 429, 5646, 279, 1376, 330, 65063, 1, 448, 279, 897, 330, 59604, 1, 323, 2441, 1376, 330, 44441, 1, 448, 279, 1372, 358, 1101, 3381, 315, 382, 40, 1265, 1281, 2704, 279, 4718, 19482, 374, 4396, 13, 2938, 3363, 1667, 1990, 17194, 369, 6894, 323, 914, 2750, 11, 323, 76602, 34901, 1948, 1376, 19083, 13530, 13, 7281, 11, 279, 5109, 1265, 387, 304, 17194, 421, 807, 2299, 9069, 11, 714, 7042, 374, 264, 1372, 11, 773, 432, 1265, 387, 2041, 17194, 382, 97904, 432, 678, 3786, 11, 279, 4718, 1633, 1265, 1401, 1075, 419, 25, 5212, 65063, 788, 330, 59604, 497, 330, 44441, 788, 220, 17, 16, 20, 15, 15, 15, 15, 7810, 358, 1265, 3042, 419, 9355, 773, 279, 1196, 646, 6707, 3535, 323, 990, 279, 1995, 382, 40, 5775, 421, 279, 1196, 3880, 803, 3565, 11, 1075, 279, 7042, 7071, 594, 2530, 476, 279, 4734, 1042, 432, 572, 12433, 13, 1988, 2474, 807, 3207, 944, 2548, 369, 429, 11, 358, 3278, 9214, 311, 279, 1995, 11223, 13, 10696, 807, 1101, 1184, 264, 30339, 821, 5944, 369, 264, 2025, 476, 264, 1895, 382, 13394, 11, 12831, 279, 1196, 594, 1681, 11, 807, 2578, 387, 264, 5458, 3238, 389, 264, 2390, 11, 476, 8365, 4325, 11220, 458, 906, 429, 7460, 6722, 9720, 323, 862, 21910, 13, 20988, 1616, 11, 8241, 13382, 323, 63594, 821, 374, 1376, 382, 40, 1265, 5978, 429, 279, 4718, 374, 12440, 23126, 311, 5648, 894, 5975, 979, 279, 1196, 16297, 311, 990, 432, 13, 2308, 50494, 11, 1101, 14396, 4718, 13, 2938, 1616, 11, 432, 594, 4135, 311, 2975, 323, 24937, 1119, 862, 2038, 476, 2197, 382, 641, 12126, 11, 358, 10820, 279, 6722, 11, 1730, 279, 7042, 1372, 11, 32930, 432, 1119, 264, 4718, 1633, 11, 323, 1865, 2704, 279, 19482, 374, 4396, 13, 358, 3207, 944, 916, 5689, 48795, 432, 11, 1101, 3897, 279, 5871, 1995, 304, 279, 11223, 3561, 624, 151649, 4913, 606, 788, 330, 59604, 497, 330, 44441, 788, 220, 17, 16, 20, 15, 15, 15, 15, 92, 151643], 'meta_info': {'id': '3053818840d14ff6b7fe587bfb1b22b2', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 500, 'cached_tokens': 22, 'cached_tokens_details': {'device': 22, 'host': 0}, 'dp_rank': None, 'e2e_latency': 5.436690540984273, 'response_sent_to_client_ts': 1775251512.2868125}}</strong>


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

    [{'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '4b8c8510760043eba50d7c7116f3567a', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': {'device': 10, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.11936649540439248, 'response_sent_to_client_ts': 1775251512.4334493}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '5898b7d489e545feaac6e2fe2e416b41', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': {'device': 10, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.1192792053334415, 'response_sent_to_client_ts': 1775251512.4334638}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '5cf47ae990eb46f39219cde99d78f4ee', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': {'device': 10, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.11922616697847843, 'response_sent_to_client_ts': 1775251512.433469}}]


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

    {'text': ' France, and the \n\\( n \\)  \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\(', 'output_ids': [9625, 11, 323, 279, 220, 198, 44292, 308, 1124, 8, 220, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767], 'meta_info': {'id': '6d7ea6b6b9534b42a5388167e0632a30', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 6, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': {'device': 1, 'host': 0}, 'dp_rank': None, 'e2e_latency': 21.412451576907188, 'response_sent_to_client_ts': 1775251533.8539848}}


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


<strong style='color: #00008B;'>{'text': 'Alright, so the user is asking for the information and population of the capital of France in JSON format. I know the capital is Paris, so that\'s the first piece to get right. They probably want the population data, so I should look up the latest numbers. I remember that Paris has a population over 30 million, maybe around 3,700,000 or so, but I\'m not entirely sure. I should check a recent source to confirm. Also, the user wants this in JSON format, which means I\'ll need to structure it properly with key-value pairs. Maybe include country, capital, population, and a note about the population figure. I wonder if they need more details, like area or number of inhabitants, but since they specified the capital and population, I\'ll stick to those. I shouldn\'t assume they want just the population; maybe the total inhabitants are also useful. I\'ll make sure the JSON is properly formatted so that it doesn\'t break when they use it. Alright, I think that\'s covered. I\'ll prepare the response accordingly.\n</think>\n\nHere is the information and population of the capital of France in JSON format:\n\n```json\n{\n  "capital": "Paris",\n  "country": "France",\n  "population": 3700000,\n  "total_inhabitants": "Over 30 million"\n}\n```\n\nThis JSON object provides the following details:\n- **Capital**: Paris\n- **Country**: France\n- **Population**: Approximately 3,700,000 (based on the latest estimates)\n- **Total Inhabitants**: Over 30 million (as of recent data)\n\nNote: Population figures can vary slightly depending on the source and the year of publication. For the most accurate and up-to-date information, please refer to official government statistics or recent publications.', 'output_ids': [71486, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 358, 1414, 279, 6722, 374, 12095, 11, 773, 429, 594, 279, 1156, 6573, 311, 633, 1290, 13, 2379, 4658, 1366, 279, 7042, 821, 11, 773, 358, 1265, 1401, 705, 279, 5535, 5109, 13, 358, 6099, 429, 12095, 702, 264, 7042, 916, 220, 18, 15, 3526, 11, 7196, 2163, 220, 18, 11, 22, 15, 15, 11, 15, 15, 15, 476, 773, 11, 714, 358, 2776, 537, 11368, 2704, 13, 358, 1265, 1779, 264, 3213, 2530, 311, 7683, 13, 7281, 11, 279, 1196, 6801, 419, 304, 4718, 3561, 11, 892, 3363, 358, 3278, 1184, 311, 5944, 432, 10277, 448, 1376, 19083, 13530, 13, 10696, 2924, 3146, 11, 6722, 11, 7042, 11, 323, 264, 5185, 911, 279, 7042, 7071, 13, 358, 5775, 421, 807, 1184, 803, 3565, 11, 1075, 3082, 476, 1372, 315, 39671, 11, 714, 2474, 807, 5189, 279, 6722, 323, 7042, 11, 358, 3278, 9214, 311, 1846, 13, 358, 13133, 944, 9658, 807, 1366, 1101, 279, 7042, 26, 7196, 279, 2790, 39671, 525, 1083, 5390, 13, 358, 3278, 1281, 2704, 279, 4718, 374, 10277, 23126, 773, 429, 432, 3171, 944, 1438, 979, 807, 990, 432, 13, 97593, 11, 358, 1744, 429, 594, 9761, 13, 358, 3278, 10549, 279, 2033, 27079, 624, 151649, 271, 8420, 374, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 1447, 73594, 2236, 198, 515, 220, 330, 65063, 788, 330, 59604, 756, 220, 330, 11141, 788, 330, 49000, 756, 220, 330, 44441, 788, 220, 18, 22, 15, 15, 15, 15, 15, 345, 220, 330, 5035, 1243, 96702, 1783, 788, 330, 1918, 220, 18, 15, 3526, 698, 532, 13874, 19324, 1986, 4718, 1633, 5707, 279, 2701, 3565, 510, 12, 3070, 63593, 95518, 12095, 198, 12, 3070, 16408, 95518, 9625, 198, 12, 3070, 53371, 95518, 78804, 220, 18, 11, 22, 15, 15, 11, 15, 15, 15, 320, 29939, 389, 279, 5535, 17530, 340, 12, 3070, 7595, 758, 96702, 1783, 95518, 6065, 220, 18, 15, 3526, 320, 300, 315, 3213, 821, 692, 9112, 25, 39529, 12396, 646, 13289, 10078, 11649, 389, 279, 2530, 323, 279, 1042, 315, 16599, 13, 1752, 279, 1429, 13382, 323, 705, 4686, 18413, 1995, 11, 4486, 8300, 311, 3946, 3033, 13142, 476, 3213, 27985, 13, 151643], 'meta_info': {'id': 'a01adcb6d690438c843c7b53987a0cb2', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 384, 'cached_tokens': 22, 'cached_tokens_details': {'device': 22, 'host': 0}, 'dp_rank': None, 'e2e_latency': 3.7473198669031262, 'response_sent_to_client_ts': 1775251537.611905}}</strong>



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

    Multi-thread loading shards:  50% Completed | 1/2 [00:01<00:01,  1.25s/it]

    Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.32s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.31s/it]


    2026-04-03 21:25:55,483 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 21:25:55] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:47,  2.94s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:47,  2.94s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:24,  1.51s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:24,  1.51s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:51,  1.06it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:51,  1.06it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:36,  1.50it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:36,  1.50it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:26,  1.98it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:26,  1.98it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:21,  2.46it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:21,  2.46it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:16,  3.01it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:16,  3.01it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:14,  3.56it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:14,  3.56it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:11,  4.19it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:11,  4.19it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:05<00:10,  4.78it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:05<00:10,  4.78it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:05<00:08,  5.42it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:05<00:08,  5.42it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:05<00:07,  6.11it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:05<00:07,  6.11it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:05<00:06,  6.82it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:05<00:06,  6.82it/s]

    Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:05<00:06,  6.82it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:05<00:05,  8.20it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:05<00:05,  8.20it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:05<00:05,  8.20it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:04,  9.71it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:04,  9.71it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:04,  9.71it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:05<00:03, 11.45it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:05<00:03, 11.45it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:05<00:03, 11.45it/s]

    Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:05<00:03, 11.45it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:05<00:02, 14.66it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:05<00:02, 14.66it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:06<00:02, 14.66it/s]Compiling num tokens (num_tokens=768):  38%|███▊      | 22/58 [00:06<00:02, 14.66it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:06<00:01, 17.94it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:06<00:01, 17.94it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:06<00:01, 17.94it/s]

    Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:06<00:01, 17.94it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:06<00:01, 17.94it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:06<00:01, 23.26it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:06<00:01, 23.26it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:06<00:01, 23.26it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:06<00:01, 23.26it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:06<00:01, 23.26it/s]Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:06<00:01, 23.26it/s]Compiling num tokens (num_tokens=320):  50%|█████     | 29/58 [00:06<00:01, 23.26it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:06<00:00, 31.74it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:06<00:00, 31.74it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:06<00:00, 31.74it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:06<00:00, 31.74it/s]

    Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:06<00:00, 31.74it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:06<00:00, 31.74it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:06<00:00, 36.32it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:06<00:00, 36.32it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:06<00:00, 36.32it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:06<00:00, 36.32it/s]Compiling num tokens (num_tokens=144):  69%|██████▉   | 40/58 [00:06<00:00, 36.32it/s]Compiling num tokens (num_tokens=128):  69%|██████▉   | 40/58 [00:06<00:00, 36.32it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:06<00:00, 38.50it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:06<00:00, 38.50it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:06<00:00, 38.50it/s] 

    Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:06<00:00, 38.50it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:06<00:00, 38.50it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:06<00:00, 38.50it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:06<00:00, 40.26it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:06<00:00, 40.26it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:06<00:00, 40.26it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:06<00:00, 40.26it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:06<00:00, 40.26it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:06<00:00, 40.26it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:06<00:00, 40.26it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:06<00:00, 40.26it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:06<00:00, 48.21it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:06<00:00, 48.21it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:06<00:00,  8.61it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=75.24 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=75.24 GB):   2%|▏         | 1/58 [00:00<00:25,  2.23it/s]Capturing num tokens (num_tokens=7680 avail_mem=107.20 GB):   2%|▏         | 1/58 [00:00<00:25,  2.23it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=107.20 GB):   3%|▎         | 2/58 [00:00<00:19,  2.89it/s]Capturing num tokens (num_tokens=7168 avail_mem=107.20 GB):   3%|▎         | 2/58 [00:00<00:19,  2.89it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=107.20 GB):   5%|▌         | 3/58 [00:00<00:16,  3.31it/s]Capturing num tokens (num_tokens=6656 avail_mem=107.21 GB):   5%|▌         | 3/58 [00:00<00:16,  3.31it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=107.21 GB):   7%|▋         | 4/58 [00:01<00:14,  3.70it/s]Capturing num tokens (num_tokens=6144 avail_mem=107.21 GB):   7%|▋         | 4/58 [00:01<00:14,  3.70it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=107.21 GB):   9%|▊         | 5/58 [00:01<00:13,  4.04it/s]Capturing num tokens (num_tokens=5632 avail_mem=107.22 GB):   9%|▊         | 5/58 [00:01<00:13,  4.04it/s]Capturing num tokens (num_tokens=5632 avail_mem=107.22 GB):  10%|█         | 6/58 [00:01<00:11,  4.48it/s]Capturing num tokens (num_tokens=5120 avail_mem=107.22 GB):  10%|█         | 6/58 [00:01<00:11,  4.48it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=107.22 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.86it/s]Capturing num tokens (num_tokens=4608 avail_mem=107.22 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.86it/s]Capturing num tokens (num_tokens=4608 avail_mem=107.22 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.23it/s]Capturing num tokens (num_tokens=4096 avail_mem=107.23 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.23it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=107.23 GB):  16%|█▌        | 9/58 [00:02<00:10,  4.85it/s]Capturing num tokens (num_tokens=3840 avail_mem=107.23 GB):  16%|█▌        | 9/58 [00:02<00:10,  4.85it/s]Capturing num tokens (num_tokens=3840 avail_mem=107.23 GB):  17%|█▋        | 10/58 [00:02<00:08,  5.46it/s]Capturing num tokens (num_tokens=3584 avail_mem=107.23 GB):  17%|█▋        | 10/58 [00:02<00:08,  5.46it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=107.23 GB):  19%|█▉        | 11/58 [00:02<00:07,  6.13it/s]Capturing num tokens (num_tokens=3328 avail_mem=107.23 GB):  19%|█▉        | 11/58 [00:02<00:07,  6.13it/s]Capturing num tokens (num_tokens=3328 avail_mem=107.23 GB):  21%|██        | 12/58 [00:02<00:06,  6.84it/s]Capturing num tokens (num_tokens=3072 avail_mem=107.23 GB):  21%|██        | 12/58 [00:02<00:06,  6.84it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=107.23 GB):  22%|██▏       | 13/58 [00:02<00:05,  7.52it/s]Capturing num tokens (num_tokens=2816 avail_mem=107.23 GB):  22%|██▏       | 13/58 [00:02<00:05,  7.52it/s]Capturing num tokens (num_tokens=2560 avail_mem=107.23 GB):  22%|██▏       | 13/58 [00:02<00:05,  7.52it/s]Capturing num tokens (num_tokens=2560 avail_mem=107.23 GB):  26%|██▌       | 15/58 [00:02<00:04,  9.05it/s]Capturing num tokens (num_tokens=2304 avail_mem=107.23 GB):  26%|██▌       | 15/58 [00:02<00:04,  9.05it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=107.27 GB):  26%|██▌       | 15/58 [00:02<00:04,  9.05it/s]Capturing num tokens (num_tokens=2048 avail_mem=107.27 GB):  29%|██▉       | 17/58 [00:02<00:03, 10.55it/s]Capturing num tokens (num_tokens=1792 avail_mem=106.04 GB):  29%|██▉       | 17/58 [00:02<00:03, 10.55it/s]Capturing num tokens (num_tokens=1536 avail_mem=105.94 GB):  29%|██▉       | 17/58 [00:02<00:03, 10.55it/s]Capturing num tokens (num_tokens=1536 avail_mem=105.94 GB):  33%|███▎      | 19/58 [00:03<00:03, 12.31it/s]Capturing num tokens (num_tokens=1280 avail_mem=105.95 GB):  33%|███▎      | 19/58 [00:03<00:03, 12.31it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=105.95 GB):  33%|███▎      | 19/58 [00:03<00:03, 12.31it/s]Capturing num tokens (num_tokens=960 avail_mem=105.94 GB):  33%|███▎      | 19/58 [00:03<00:03, 12.31it/s] Capturing num tokens (num_tokens=960 avail_mem=105.94 GB):  38%|███▊      | 22/58 [00:03<00:02, 15.51it/s]Capturing num tokens (num_tokens=896 avail_mem=105.94 GB):  38%|███▊      | 22/58 [00:03<00:02, 15.51it/s]Capturing num tokens (num_tokens=832 avail_mem=105.94 GB):  38%|███▊      | 22/58 [00:03<00:02, 15.51it/s]Capturing num tokens (num_tokens=768 avail_mem=105.93 GB):  38%|███▊      | 22/58 [00:03<00:02, 15.51it/s]Capturing num tokens (num_tokens=768 avail_mem=105.93 GB):  43%|████▎     | 25/58 [00:03<00:01, 18.64it/s]Capturing num tokens (num_tokens=704 avail_mem=105.93 GB):  43%|████▎     | 25/58 [00:03<00:01, 18.64it/s]

    Capturing num tokens (num_tokens=640 avail_mem=105.92 GB):  43%|████▎     | 25/58 [00:03<00:01, 18.64it/s]Capturing num tokens (num_tokens=576 avail_mem=105.92 GB):  43%|████▎     | 25/58 [00:03<00:01, 18.64it/s]Capturing num tokens (num_tokens=512 avail_mem=105.91 GB):  43%|████▎     | 25/58 [00:03<00:01, 18.64it/s]Capturing num tokens (num_tokens=512 avail_mem=105.91 GB):  50%|█████     | 29/58 [00:03<00:01, 22.48it/s]Capturing num tokens (num_tokens=480 avail_mem=105.91 GB):  50%|█████     | 29/58 [00:03<00:01, 22.48it/s]Capturing num tokens (num_tokens=448 avail_mem=105.91 GB):  50%|█████     | 29/58 [00:03<00:01, 22.48it/s]Capturing num tokens (num_tokens=416 avail_mem=105.91 GB):  50%|█████     | 29/58 [00:03<00:01, 22.48it/s]Capturing num tokens (num_tokens=384 avail_mem=105.90 GB):  50%|█████     | 29/58 [00:03<00:01, 22.48it/s]

    Capturing num tokens (num_tokens=384 avail_mem=105.90 GB):  57%|█████▋    | 33/58 [00:03<00:00, 25.90it/s]Capturing num tokens (num_tokens=352 avail_mem=105.89 GB):  57%|█████▋    | 33/58 [00:03<00:00, 25.90it/s]Capturing num tokens (num_tokens=320 avail_mem=105.89 GB):  57%|█████▋    | 33/58 [00:03<00:00, 25.90it/s]Capturing num tokens (num_tokens=288 avail_mem=105.89 GB):  57%|█████▋    | 33/58 [00:03<00:00, 25.90it/s]Capturing num tokens (num_tokens=256 avail_mem=105.88 GB):  57%|█████▋    | 33/58 [00:03<00:00, 25.90it/s]Capturing num tokens (num_tokens=256 avail_mem=105.88 GB):  64%|██████▍   | 37/58 [00:03<00:00, 28.83it/s]Capturing num tokens (num_tokens=240 avail_mem=105.88 GB):  64%|██████▍   | 37/58 [00:03<00:00, 28.83it/s]Capturing num tokens (num_tokens=224 avail_mem=105.87 GB):  64%|██████▍   | 37/58 [00:03<00:00, 28.83it/s]Capturing num tokens (num_tokens=208 avail_mem=105.87 GB):  64%|██████▍   | 37/58 [00:03<00:00, 28.83it/s]Capturing num tokens (num_tokens=192 avail_mem=105.87 GB):  64%|██████▍   | 37/58 [00:03<00:00, 28.83it/s]

    Capturing num tokens (num_tokens=192 avail_mem=105.87 GB):  71%|███████   | 41/58 [00:03<00:00, 31.28it/s]Capturing num tokens (num_tokens=176 avail_mem=105.86 GB):  71%|███████   | 41/58 [00:03<00:00, 31.28it/s]Capturing num tokens (num_tokens=160 avail_mem=105.86 GB):  71%|███████   | 41/58 [00:03<00:00, 31.28it/s]Capturing num tokens (num_tokens=144 avail_mem=105.86 GB):  71%|███████   | 41/58 [00:03<00:00, 31.28it/s]Capturing num tokens (num_tokens=128 avail_mem=105.87 GB):  71%|███████   | 41/58 [00:03<00:00, 31.28it/s]Capturing num tokens (num_tokens=128 avail_mem=105.87 GB):  78%|███████▊  | 45/58 [00:03<00:00, 32.94it/s]Capturing num tokens (num_tokens=112 avail_mem=105.86 GB):  78%|███████▊  | 45/58 [00:03<00:00, 32.94it/s]Capturing num tokens (num_tokens=96 avail_mem=105.86 GB):  78%|███████▊  | 45/58 [00:03<00:00, 32.94it/s] Capturing num tokens (num_tokens=80 avail_mem=105.85 GB):  78%|███████▊  | 45/58 [00:03<00:00, 32.94it/s]Capturing num tokens (num_tokens=64 avail_mem=105.85 GB):  78%|███████▊  | 45/58 [00:03<00:00, 32.94it/s]

    Capturing num tokens (num_tokens=64 avail_mem=105.85 GB):  84%|████████▍ | 49/58 [00:03<00:00, 34.11it/s]Capturing num tokens (num_tokens=48 avail_mem=105.85 GB):  84%|████████▍ | 49/58 [00:03<00:00, 34.11it/s]Capturing num tokens (num_tokens=32 avail_mem=105.84 GB):  84%|████████▍ | 49/58 [00:03<00:00, 34.11it/s]Capturing num tokens (num_tokens=28 avail_mem=105.84 GB):  84%|████████▍ | 49/58 [00:03<00:00, 34.11it/s]Capturing num tokens (num_tokens=24 avail_mem=105.84 GB):  84%|████████▍ | 49/58 [00:04<00:00, 34.11it/s]Capturing num tokens (num_tokens=24 avail_mem=105.84 GB):  91%|█████████▏| 53/58 [00:04<00:00, 35.10it/s]Capturing num tokens (num_tokens=20 avail_mem=105.83 GB):  91%|█████████▏| 53/58 [00:04<00:00, 35.10it/s]Capturing num tokens (num_tokens=16 avail_mem=105.83 GB):  91%|█████████▏| 53/58 [00:04<00:00, 35.10it/s]Capturing num tokens (num_tokens=12 avail_mem=105.83 GB):  91%|█████████▏| 53/58 [00:04<00:00, 35.10it/s]Capturing num tokens (num_tokens=8 avail_mem=105.82 GB):  91%|█████████▏| 53/58 [00:04<00:00, 35.10it/s] 

    Capturing num tokens (num_tokens=8 avail_mem=105.82 GB):  98%|█████████▊| 57/58 [00:04<00:00, 35.76it/s]Capturing num tokens (num_tokens=4 avail_mem=105.82 GB):  98%|█████████▊| 57/58 [00:04<00:00, 35.76it/s]Capturing num tokens (num_tokens=4 avail_mem=105.82 GB): 100%|██████████| 58/58 [00:04<00:00, 13.86it/s]


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
    
    Generated text: Alright, so the user asked for the information and population of the capital of France in JSON format. Hmm, I need to figure out how to approach this. First, I should confirm what the capital of France is. Well, I know it's Paris, but maybe I should double-check to make sure. Yeah, Paris is definitely the capital.
    
    Next, I need to find the population of Paris. I remember reading that it's a big city, so the population must be in the hundreds of millions. Wait, no, wait. Wait, Paris is one of the largest cities in the world. I think it's around 20-something million people. Let me recall the exact figure. I believe it's approximately 21,653,000 as of 2023. I should probably verify that to be accurate.
    
    So, the user wants this information in JSON format. JSON stands for JavaScript Object Notation, which is a way to structure data. I need to create a JSON object that includes the city name, its population, and maybe some additional details if applicable. The user specifically mentioned population, so I should include that.
    
    Wait, the initial response I had was just the JSON with city and population. Maybe the user expects more details. Should I include other information like the area or some interesting facts about Paris? Hmm, the user didn't specify, but they did ask for the population, so sticking to that is probably best unless they ask for more.
    
    I should also consider the format. The JSON object should have keys like "city" and "population". The population number should be a large number, so I'll represent it as a number in JSON, not as a string. That makes sense.
    
    Putting it all together, I'll structure the JSON with the city name as a string and the population as a number. I think that's all the user needs. They might be using this data for a project or a presentation, so accuracy is important. Maybe I should mention the source of the data, but the user didn't ask for it, so perhaps it's better to keep it simple.
    
    Wait, should I write 21653000 as a string or a number? In JSON, numbers are written without quotes, so it should be 21653000 as a number. That's correct.
    
    So the final JSON object would be something like:
    {
      "city": "Paris",
      "population": 21653000
    }
    
    I think that's clear and concise. I hope that's exactly what the user was looking for.
    </think>
    
    Here is the information and population of the capital of France in JSON format:
    
    ```json
    {
      "city": "Paris",
      "population": 21653000
    }
    ```



```python
llm.shutdown()
```
