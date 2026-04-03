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
    2026-04-03 09:16:32.813 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:16:32] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 09:16:32.813 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:16:32] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 09:16:32.813 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:16:32] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 09:16:32.813 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:16:32] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 09:16:32.814 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:16:32] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Multi-thread loading shards:  50% Completed | 1/2 [00:04<00:04,  4.09s/it]

    Multi-thread loading shards: 100% Completed | 2/2 [00:07<00:00,  3.84s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:07<00:00,  3.88s/it]


    2026-04-03 09:16:46,710 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 09:16:46] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:59,  3.14s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:59,  3.14s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:04<01:42,  1.83s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:04<01:42,  1.83s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:08,  1.24s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:08,  1.24s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:05<00:50,  1.07it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:05<00:50,  1.07it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:38,  1.36it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:38,  1.36it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:31,  1.65it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:31,  1.65it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:06<00:25,  1.98it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:06<00:25,  1.98it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:06<00:21,  2.30it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:06<00:21,  2.30it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:06<00:18,  2.68it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:06<00:18,  2.68it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:06<00:15,  3.11it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:06<00:15,  3.11it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:07<00:13,  3.54it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:07<00:13,  3.54it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:07<00:11,  3.86it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:07<00:11,  3.86it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:07<00:10,  4.19it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:07<00:10,  4.19it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:07<00:09,  4.51it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:07<00:09,  4.51it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:07<00:08,  4.89it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:07<00:08,  4.89it/s]

    Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:07<00:07,  5.33it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:07<00:07,  5.33it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:08<00:06,  5.91it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:08<00:06,  5.91it/s]

    Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:08<00:05,  6.71it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:08<00:05,  6.71it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:08<00:05,  7.33it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:08<00:05,  7.33it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:08<00:05,  7.33it/s]

    Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:08<00:04,  9.16it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:08<00:04,  9.16it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:08<00:04,  9.16it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:08<00:03, 10.94it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:08<00:03, 10.94it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:08<00:03, 10.94it/s]

    Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:08<00:02, 13.03it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:08<00:02, 13.03it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:08<00:02, 13.03it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:08<00:02, 13.03it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:08<00:01, 15.69it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:08<00:01, 15.69it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:08<00:01, 15.69it/s]

    Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:08<00:01, 15.69it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:08<00:01, 18.69it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:08<00:01, 18.69it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:08<00:01, 18.69it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:08<00:01, 18.69it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:08<00:01, 18.69it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:09<00:01, 22.16it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:09<00:01, 22.16it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:09<00:01, 22.16it/s]

    Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:09<00:01, 22.16it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:09<00:01, 22.16it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:09<00:00, 25.93it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:09<00:00, 25.93it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:09<00:00, 25.93it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:09<00:00, 25.93it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:09<00:00, 25.93it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:09<00:00, 28.37it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:09<00:00, 28.37it/s]

    Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:09<00:00, 28.37it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:09<00:00, 28.37it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:09<00:00, 24.44it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:09<00:00, 24.44it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:09<00:00, 24.44it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:09<00:00, 24.44it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:09<00:00, 24.44it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:09<00:00, 27.81it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:09<00:00, 27.81it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:09<00:00, 27.81it/s]

    Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:09<00:00, 27.81it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:09<00:00, 27.81it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:09<00:00, 27.81it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:09<00:00, 27.81it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:09<00:00, 35.92it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:09<00:00, 35.92it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:09<00:00, 35.92it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:09<00:00,  6.02it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=100.29 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=100.29 GB):   2%|▏         | 1/58 [00:00<00:45,  1.25it/s]Capturing num tokens (num_tokens=7680 avail_mem=100.21 GB):   2%|▏         | 1/58 [00:00<00:45,  1.25it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=100.21 GB):   3%|▎         | 2/58 [00:01<00:37,  1.50it/s]Capturing num tokens (num_tokens=7168 avail_mem=100.22 GB):   3%|▎         | 2/58 [00:01<00:37,  1.50it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=100.22 GB):   5%|▌         | 3/58 [00:01<00:31,  1.72it/s]Capturing num tokens (num_tokens=6656 avail_mem=99.44 GB):   5%|▌         | 3/58 [00:01<00:31,  1.72it/s] 

    Capturing num tokens (num_tokens=6656 avail_mem=99.44 GB):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]Capturing num tokens (num_tokens=6144 avail_mem=99.50 GB):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=99.50 GB):   9%|▊         | 5/58 [00:02<00:26,  1.99it/s]Capturing num tokens (num_tokens=5632 avail_mem=99.55 GB):   9%|▊         | 5/58 [00:02<00:26,  1.99it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=99.55 GB):  10%|█         | 6/58 [00:03<00:24,  2.17it/s]Capturing num tokens (num_tokens=5120 avail_mem=100.20 GB):  10%|█         | 6/58 [00:03<00:24,  2.17it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=100.20 GB):  12%|█▏        | 7/58 [00:03<00:23,  2.21it/s]Capturing num tokens (num_tokens=4608 avail_mem=100.20 GB):  12%|█▏        | 7/58 [00:03<00:23,  2.21it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=100.20 GB):  14%|█▍        | 8/58 [00:03<00:20,  2.44it/s]Capturing num tokens (num_tokens=4096 avail_mem=99.65 GB):  14%|█▍        | 8/58 [00:03<00:20,  2.44it/s] 

    Capturing num tokens (num_tokens=4096 avail_mem=99.65 GB):  16%|█▌        | 9/58 [00:04<00:17,  2.72it/s]Capturing num tokens (num_tokens=3840 avail_mem=99.70 GB):  16%|█▌        | 9/58 [00:04<00:17,  2.72it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=99.70 GB):  17%|█▋        | 10/58 [00:04<00:16,  2.98it/s]Capturing num tokens (num_tokens=3584 avail_mem=99.75 GB):  17%|█▋        | 10/58 [00:04<00:16,  2.98it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=99.75 GB):  19%|█▉        | 11/58 [00:04<00:14,  3.26it/s]Capturing num tokens (num_tokens=3328 avail_mem=99.77 GB):  19%|█▉        | 11/58 [00:04<00:14,  3.26it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=99.77 GB):  21%|██        | 12/58 [00:04<00:12,  3.54it/s]Capturing num tokens (num_tokens=3072 avail_mem=100.18 GB):  21%|██        | 12/58 [00:04<00:12,  3.54it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=100.18 GB):  22%|██▏       | 13/58 [00:05<00:11,  3.86it/s]Capturing num tokens (num_tokens=2816 avail_mem=100.18 GB):  22%|██▏       | 13/58 [00:05<00:11,  3.86it/s]Capturing num tokens (num_tokens=2816 avail_mem=100.18 GB):  24%|██▍       | 14/58 [00:05<00:10,  4.30it/s]Capturing num tokens (num_tokens=2560 avail_mem=99.82 GB):  24%|██▍       | 14/58 [00:05<00:10,  4.30it/s] 

    Capturing num tokens (num_tokens=2560 avail_mem=99.82 GB):  26%|██▌       | 15/58 [00:05<00:08,  4.80it/s]Capturing num tokens (num_tokens=2304 avail_mem=99.85 GB):  26%|██▌       | 15/58 [00:05<00:08,  4.80it/s]Capturing num tokens (num_tokens=2304 avail_mem=99.85 GB):  28%|██▊       | 16/58 [00:05<00:07,  5.29it/s]Capturing num tokens (num_tokens=2048 avail_mem=100.12 GB):  28%|██▊       | 16/58 [00:05<00:07,  5.29it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=100.12 GB):  29%|██▉       | 17/58 [00:05<00:07,  5.72it/s]Capturing num tokens (num_tokens=1792 avail_mem=100.18 GB):  29%|██▉       | 17/58 [00:05<00:07,  5.72it/s]Capturing num tokens (num_tokens=1792 avail_mem=100.18 GB):  31%|███       | 18/58 [00:05<00:06,  6.25it/s]Capturing num tokens (num_tokens=1536 avail_mem=100.17 GB):  31%|███       | 18/58 [00:05<00:06,  6.25it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=100.17 GB):  33%|███▎      | 19/58 [00:05<00:05,  7.03it/s]Capturing num tokens (num_tokens=1280 avail_mem=99.93 GB):  33%|███▎      | 19/58 [00:05<00:05,  7.03it/s] Capturing num tokens (num_tokens=1024 avail_mem=100.17 GB):  33%|███▎      | 19/58 [00:06<00:05,  7.03it/s]Capturing num tokens (num_tokens=1024 avail_mem=100.17 GB):  36%|███▌      | 21/58 [00:06<00:04,  8.57it/s]Capturing num tokens (num_tokens=960 avail_mem=100.17 GB):  36%|███▌      | 21/58 [00:06<00:04,  8.57it/s] 

    Capturing num tokens (num_tokens=896 avail_mem=99.97 GB):  36%|███▌      | 21/58 [00:06<00:04,  8.57it/s] Capturing num tokens (num_tokens=896 avail_mem=99.97 GB):  40%|███▉      | 23/58 [00:06<00:03, 10.25it/s]Capturing num tokens (num_tokens=832 avail_mem=100.00 GB):  40%|███▉      | 23/58 [00:06<00:03, 10.25it/s]Capturing num tokens (num_tokens=768 avail_mem=100.16 GB):  40%|███▉      | 23/58 [00:06<00:03, 10.25it/s]Capturing num tokens (num_tokens=768 avail_mem=100.16 GB):  43%|████▎     | 25/58 [00:06<00:02, 11.22it/s]Capturing num tokens (num_tokens=704 avail_mem=100.15 GB):  43%|████▎     | 25/58 [00:06<00:02, 11.22it/s]

    Capturing num tokens (num_tokens=640 avail_mem=100.15 GB):  43%|████▎     | 25/58 [00:06<00:02, 11.22it/s]Capturing num tokens (num_tokens=640 avail_mem=100.15 GB):  47%|████▋     | 27/58 [00:06<00:02, 12.66it/s]Capturing num tokens (num_tokens=576 avail_mem=100.03 GB):  47%|████▋     | 27/58 [00:06<00:02, 12.66it/s]Capturing num tokens (num_tokens=512 avail_mem=100.05 GB):  47%|████▋     | 27/58 [00:06<00:02, 12.66it/s]Capturing num tokens (num_tokens=512 avail_mem=100.05 GB):  50%|█████     | 29/58 [00:06<00:02, 14.26it/s]Capturing num tokens (num_tokens=480 avail_mem=100.13 GB):  50%|█████     | 29/58 [00:06<00:02, 14.26it/s]

    Capturing num tokens (num_tokens=448 avail_mem=100.12 GB):  50%|█████     | 29/58 [00:06<00:02, 14.26it/s]Capturing num tokens (num_tokens=448 avail_mem=100.12 GB):  53%|█████▎    | 31/58 [00:06<00:01, 15.46it/s]Capturing num tokens (num_tokens=416 avail_mem=100.11 GB):  53%|█████▎    | 31/58 [00:06<00:01, 15.46it/s]Capturing num tokens (num_tokens=384 avail_mem=100.11 GB):  53%|█████▎    | 31/58 [00:06<00:01, 15.46it/s]Capturing num tokens (num_tokens=352 avail_mem=100.10 GB):  53%|█████▎    | 31/58 [00:06<00:01, 15.46it/s]Capturing num tokens (num_tokens=352 avail_mem=100.10 GB):  59%|█████▊    | 34/58 [00:06<00:01, 17.59it/s]Capturing num tokens (num_tokens=320 avail_mem=100.04 GB):  59%|█████▊    | 34/58 [00:06<00:01, 17.59it/s]

    Capturing num tokens (num_tokens=288 avail_mem=100.08 GB):  59%|█████▊    | 34/58 [00:06<00:01, 17.59it/s]Capturing num tokens (num_tokens=256 avail_mem=100.07 GB):  59%|█████▊    | 34/58 [00:06<00:01, 17.59it/s]Capturing num tokens (num_tokens=256 avail_mem=100.07 GB):  64%|██████▍   | 37/58 [00:06<00:01, 19.42it/s]Capturing num tokens (num_tokens=240 avail_mem=100.06 GB):  64%|██████▍   | 37/58 [00:06<00:01, 19.42it/s]Capturing num tokens (num_tokens=224 avail_mem=100.06 GB):  64%|██████▍   | 37/58 [00:07<00:01, 19.42it/s]Capturing num tokens (num_tokens=208 avail_mem=100.05 GB):  64%|██████▍   | 37/58 [00:07<00:01, 19.42it/s]

    Capturing num tokens (num_tokens=208 avail_mem=100.05 GB):  69%|██████▉   | 40/58 [00:07<00:00, 20.82it/s]Capturing num tokens (num_tokens=192 avail_mem=100.05 GB):  69%|██████▉   | 40/58 [00:07<00:00, 20.82it/s]Capturing num tokens (num_tokens=176 avail_mem=100.00 GB):  69%|██████▉   | 40/58 [00:07<00:00, 20.82it/s]Capturing num tokens (num_tokens=160 avail_mem=100.03 GB):  69%|██████▉   | 40/58 [00:07<00:00, 20.82it/s]Capturing num tokens (num_tokens=160 avail_mem=100.03 GB):  74%|███████▍  | 43/58 [00:07<00:00, 23.05it/s]Capturing num tokens (num_tokens=144 avail_mem=100.02 GB):  74%|███████▍  | 43/58 [00:07<00:00, 23.05it/s]Capturing num tokens (num_tokens=128 avail_mem=100.03 GB):  74%|███████▍  | 43/58 [00:07<00:00, 23.05it/s]Capturing num tokens (num_tokens=112 avail_mem=100.02 GB):  74%|███████▍  | 43/58 [00:07<00:00, 23.05it/s]

    Capturing num tokens (num_tokens=112 avail_mem=100.02 GB):  79%|███████▉  | 46/58 [00:07<00:00, 22.31it/s]Capturing num tokens (num_tokens=96 avail_mem=100.01 GB):  79%|███████▉  | 46/58 [00:07<00:00, 22.31it/s] Capturing num tokens (num_tokens=80 avail_mem=99.96 GB):  79%|███████▉  | 46/58 [00:07<00:00, 22.31it/s] Capturing num tokens (num_tokens=64 avail_mem=99.96 GB):  79%|███████▉  | 46/58 [00:07<00:00, 22.31it/s]Capturing num tokens (num_tokens=64 avail_mem=99.96 GB):  84%|████████▍ | 49/58 [00:07<00:00, 22.32it/s]Capturing num tokens (num_tokens=48 avail_mem=99.95 GB):  84%|████████▍ | 49/58 [00:07<00:00, 22.32it/s]Capturing num tokens (num_tokens=32 avail_mem=99.94 GB):  84%|████████▍ | 49/58 [00:07<00:00, 22.32it/s]

    Capturing num tokens (num_tokens=28 avail_mem=99.94 GB):  84%|████████▍ | 49/58 [00:07<00:00, 22.32it/s]Capturing num tokens (num_tokens=28 avail_mem=99.94 GB):  90%|████████▉ | 52/58 [00:07<00:00, 24.03it/s]Capturing num tokens (num_tokens=24 avail_mem=99.93 GB):  90%|████████▉ | 52/58 [00:07<00:00, 24.03it/s]Capturing num tokens (num_tokens=20 avail_mem=99.93 GB):  90%|████████▉ | 52/58 [00:07<00:00, 24.03it/s]Capturing num tokens (num_tokens=16 avail_mem=99.93 GB):  90%|████████▉ | 52/58 [00:07<00:00, 24.03it/s]Capturing num tokens (num_tokens=12 avail_mem=99.90 GB):  90%|████████▉ | 52/58 [00:07<00:00, 24.03it/s]Capturing num tokens (num_tokens=12 avail_mem=99.90 GB):  97%|█████████▋| 56/58 [00:07<00:00, 26.31it/s]Capturing num tokens (num_tokens=8 avail_mem=99.89 GB):  97%|█████████▋| 56/58 [00:07<00:00, 26.31it/s] Capturing num tokens (num_tokens=4 avail_mem=99.88 GB):  97%|█████████▋| 56/58 [00:07<00:00, 26.31it/s]

    Capturing num tokens (num_tokens=4 avail_mem=99.88 GB): 100%|██████████| 58/58 [00:07<00:00,  7.44it/s]


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


<strong style='color: #00008B;'>reasoing_content: Okay, so the user is in New York and wants the current date and time along with the weather. I need to figure out how to get both pieces of information using the functions provided.<br><br>First, I should use the get_current_date function. The parameters required are the timezone. Since the user is in New York, I'll set the timezone parameter to 'America/New_York'. That should give me the correct date and time.<br><br>Next, for the weather, I'll use the get_current_weather function. The city is New York, the state is NY, and I'll choose Fahrenheit since the user didn't specify otherwise. So the parameters will be city: 'New York', state: 'NY', unit: 'fahrenheit'.<br><br>I need to make sure each function call is separate and follows the required format. I'll start with the date function, then the weather function. I'll also include the sources in the response to show where the information came from.<br><br><br>content: <function=get_current_date>{"timezone": "America/New_York"}</function>  <br><function=get_current_weather>{"city": "New York", "state": "NY", "unit": "fahrenheit"}</function>  <br><br>Sources:  <br>- Current date and time obtained using get_current_date function  <br>- Current weather obtained using get_current_weather function</strong>


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

    {'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. I know that Paris is the capital, so that\'s straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I\'m not sure of the exact number. I think it\'s around 2 million, but I should verify that.\n\nWait, maybe I should check the latest statistics. I recall that in recent years, Paris has seen a slight increase. Let me think... I believe the population is approximately 2,150,000 as of 2023. That seems about right, but I\'m not 100% certain. I should make sure to present this information accurately.\n\nNext, I need to structure this into a JSON format. JSON requires key-value pairs, so I\'ll create an object with keys like "city", "population", and maybe "country" for context. The city is Paris, the population is 2,150,000, and the country is France.\n\nI should also consider if the user might need more details, like the exact year of the population figure. Including that could be helpful, so I\'ll add "year": 2023. That way, the user knows the data is up to date.\n\nPutting it all together, the JSON should look clean and well-structured. I\'ll make sure the syntax is correct, with proper commas and quotation marks. No markdown, just plain JSON.\n\nI think that\'s all. The user probably just needs the information quickly, so keeping it concise is key. I\'ll present the JSON without any extra fluff.\n</think>{\n\n"name": "Paris",\n"population": 21500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1265, 1477, 279, 1429, 3213, 7042, 821, 13, 358, 6099, 429, 279, 7042, 315, 12095, 702, 1012, 7826, 11, 714, 358, 2776, 537, 2704, 315, 279, 4734, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 10146, 429, 382, 14190, 11, 7196, 358, 1265, 1779, 279, 5535, 13142, 13, 358, 19091, 429, 304, 3213, 1635, 11, 12095, 702, 3884, 264, 8112, 5263, 13, 6771, 752, 1744, 1112, 358, 4411, 279, 7042, 374, 13187, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 438, 315, 220, 17, 15, 17, 18, 13, 2938, 4977, 911, 1290, 11, 714, 358, 2776, 537, 220, 16, 15, 15, 4, 3654, 13, 358, 1265, 1281, 2704, 311, 3042, 419, 1995, 29257, 382, 5847, 11, 358, 1184, 311, 5944, 419, 1119, 264, 4718, 3561, 13, 4718, 7460, 1376, 19083, 13530, 11, 773, 358, 3278, 1855, 458, 1633, 448, 6894, 1075, 330, 8926, 497, 330, 44441, 497, 323, 7196, 330, 11141, 1, 369, 2266, 13, 576, 3283, 374, 12095, 11, 279, 7042, 374, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 11, 323, 279, 3146, 374, 9625, 382, 40, 1265, 1083, 2908, 421, 279, 1196, 2578, 1184, 803, 3565, 11, 1075, 279, 4734, 1042, 315, 279, 7042, 7071, 13, 55121, 429, 1410, 387, 10950, 11, 773, 358, 3278, 912, 330, 3157, 788, 220, 17, 15, 17, 18, 13, 2938, 1616, 11, 279, 1196, 8788, 279, 821, 374, 705, 311, 2400, 382, 97904, 432, 678, 3786, 11, 279, 4718, 1265, 1401, 4240, 323, 1632, 12, 51143, 13, 358, 3278, 1281, 2704, 279, 19482, 374, 4396, 11, 448, 6169, 76602, 323, 54231, 15423, 13, 2308, 50494, 11, 1101, 14396, 4718, 382, 40, 1744, 429, 594, 678, 13, 576, 1196, 4658, 1101, 3880, 279, 1995, 6157, 11, 773, 10282, 432, 63594, 374, 1376, 13, 358, 3278, 3042, 279, 4718, 2041, 894, 4960, 1320, 1362, 624, 151649, 4257, 1, 606, 788, 330, 59604, 756, 1, 44441, 788, 220, 17, 16, 20, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15], 'meta_info': {'id': 'adc833b297b14b219370236c9109bfeb', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': {'device': 1, 'host': 0}, 'dp_rank': None, 'e2e_latency': 25.5034450548701, 'response_sent_to_client_ts': 1775207886.9230475}}



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


<strong style='color: #00008B;'>{'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. I know that Paris is the capital, so that\'s straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I\'m not sure of the exact number. I think it\'s around 2 million, but I should verify that.\n\nWait, maybe I should check the latest statistics. I recall that in recent years, Paris has seen a slight increase. Let me think... I believe the population is approximately 2,150,000 as of 2023. That seems about right, but I\'m not 100% certain. I should make sure to present this information accurately.\n\nNext, I need to structure this into a JSON format. JSON requires key-value pairs, so I\'ll create an object with keys like "city", "population", and maybe "country" for context. The city is Paris, the population is 2,150,000, and the country is France.\n\nI should also consider if the user might need more details, like the exact year of the population figure. Including that could be helpful, so I\'ll add "year": 2023. That way, the user knows the data is up to date.\n\nPutting it all together, the JSON should look clean and well-structured. I\'ll make sure the syntax is correct, with proper commas and quotation marks. No markdown, just plain JSON.\n\nI think that\'s all. The user probably just needs the information quickly, so keeping it concise is key. I\'ll present the JSON without any extra fluff.\n</think>{\n\n"name": "Paris",\n"population": 21500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1265, 1477, 279, 1429, 3213, 7042, 821, 13, 358, 6099, 429, 279, 7042, 315, 12095, 702, 1012, 7826, 11, 714, 358, 2776, 537, 2704, 315, 279, 4734, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 10146, 429, 382, 14190, 11, 7196, 358, 1265, 1779, 279, 5535, 13142, 13, 358, 19091, 429, 304, 3213, 1635, 11, 12095, 702, 3884, 264, 8112, 5263, 13, 6771, 752, 1744, 1112, 358, 4411, 279, 7042, 374, 13187, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 438, 315, 220, 17, 15, 17, 18, 13, 2938, 4977, 911, 1290, 11, 714, 358, 2776, 537, 220, 16, 15, 15, 4, 3654, 13, 358, 1265, 1281, 2704, 311, 3042, 419, 1995, 29257, 382, 5847, 11, 358, 1184, 311, 5944, 419, 1119, 264, 4718, 3561, 13, 4718, 7460, 1376, 19083, 13530, 11, 773, 358, 3278, 1855, 458, 1633, 448, 6894, 1075, 330, 8926, 497, 330, 44441, 497, 323, 7196, 330, 11141, 1, 369, 2266, 13, 576, 3283, 374, 12095, 11, 279, 7042, 374, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 11, 323, 279, 3146, 374, 9625, 382, 40, 1265, 1083, 2908, 421, 279, 1196, 2578, 1184, 803, 3565, 11, 1075, 279, 4734, 1042, 315, 279, 7042, 7071, 13, 55121, 429, 1410, 387, 10950, 11, 773, 358, 3278, 912, 330, 3157, 788, 220, 17, 15, 17, 18, 13, 2938, 1616, 11, 279, 1196, 8788, 279, 821, 374, 705, 311, 2400, 382, 97904, 432, 678, 3786, 11, 279, 4718, 1265, 1401, 4240, 323, 1632, 12, 51143, 13, 358, 3278, 1281, 2704, 279, 19482, 374, 4396, 11, 448, 6169, 76602, 323, 54231, 15423, 13, 2308, 50494, 11, 1101, 14396, 4718, 382, 40, 1744, 429, 594, 678, 13, 576, 1196, 4658, 1101, 3880, 279, 1995, 6157, 11, 773, 10282, 432, 63594, 374, 1376, 13, 358, 3278, 3042, 279, 4718, 2041, 894, 4960, 1320, 1362, 624, 151649, 4257, 1, 606, 788, 330, 59604, 756, 1, 44441, 788, 220, 17, 16, 20, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15], 'meta_info': {'id': 'c9d140ada4fa4de388bea27c34f44693', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 22, 'cached_tokens_details': {'device': 22, 'host': 0}, 'dp_rank': None, 'e2e_latency': 21.294948489870876, 'response_sent_to_client_ts': 1775207908.2368124}}</strong>


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

    [{'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': 'a490061665e94b4e805e9d109d3bbf12', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': {'device': 10, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.1222357489168644, 'response_sent_to_client_ts': 1775207908.3925595}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': 'd914fed94ded4185b56ff4b6aaadd260', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': {'device': 10, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.12215317692607641, 'response_sent_to_client_ts': 1775207908.3925717}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '299e13689da94efeb18c93a0a5666cc0', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': {'device': 10, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.122064424213022, 'response_sent_to_client_ts': 1775207908.392577}}]


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

    {'text': ' France, and the \n\\( n \\)  \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\(', 'output_ids': [9625, 11, 323, 279, 220, 198, 44292, 308, 1124, 8, 220, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767], 'meta_info': {'id': 'd3e4825d53894e4c9c688e995588c647', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 6, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': {'device': 1, 'host': 0}, 'dp_rank': None, 'e2e_latency': 19.691196190658957, 'response_sent_to_client_ts': 1775207928.094559}}


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


<strong style='color: #00008B;'>{'text': 'Okay, the user is asking for a JSON format of the information and population of Paris, the capital of France. I need to make sure I include all the required details.\n\nFirst, I should identify the key pieces of information: city name, country name, population, and maybe some additional relevant data. For the population, I should look for the most recent estimate. I know Paris has over 3 million people, but I need the exact figure as of 2023.\n\nWait, the 2020 data says the population was 3,599,507. Should I use that? It\'s the latest available figure before recent events like the pandemic. Maybe I should also mention it as an approximate figure to cover possible fluctuations.\n\nIncluding the metropolitan area population would be helpful for the user, showing that it\'s a bit higher. That way, they have a clearer picture of the area\'s size.\n\nI should format all this information into a JSON structure, making sure the keys are clear and the data is accurate. I\'ll write this as a response to their query, providing the JSON data neatly.\n</think>\n\nHere is the information and population of the capital of France (Paris) in JSON format:\n\n```json\n{\n  "city": "Paris",\n  "country": "France",\n  "population": 3599507,\n  "metropolitan_area": 12661716,\n  "description": "The capital of France, Paris, is a major city located in northern France, known for its historical significance, landmarks, and status as one of the world\'s most cosmopolitan cities."\n}\n```', 'output_ids': [32313, 11, 279, 1196, 374, 10161, 369, 264, 4718, 3561, 315, 279, 1995, 323, 7042, 315, 12095, 11, 279, 6722, 315, 9625, 13, 358, 1184, 311, 1281, 2704, 358, 2924, 678, 279, 2567, 3565, 382, 5338, 11, 358, 1265, 10542, 279, 1376, 9666, 315, 1995, 25, 3283, 829, 11, 3146, 829, 11, 7042, 11, 323, 7196, 1045, 5107, 9760, 821, 13, 1752, 279, 7042, 11, 358, 1265, 1401, 369, 279, 1429, 3213, 16045, 13, 358, 1414, 12095, 702, 916, 220, 18, 3526, 1251, 11, 714, 358, 1184, 279, 4734, 7071, 438, 315, 220, 17, 15, 17, 18, 382, 14190, 11, 279, 220, 17, 15, 17, 15, 821, 2727, 279, 7042, 572, 220, 18, 11, 20, 24, 24, 11, 20, 15, 22, 13, 12260, 358, 990, 429, 30, 1084, 594, 279, 5535, 2500, 7071, 1573, 3213, 4357, 1075, 279, 27422, 13, 10696, 358, 1265, 1083, 6286, 432, 438, 458, 44868, 7071, 311, 3421, 3204, 64549, 382, 83449, 279, 57406, 3082, 7042, 1035, 387, 10950, 369, 279, 1196, 11, 9027, 429, 432, 594, 264, 2699, 5080, 13, 2938, 1616, 11, 807, 614, 264, 48379, 6802, 315, 279, 3082, 594, 1379, 382, 40, 1265, 3561, 678, 419, 1995, 1119, 264, 4718, 5944, 11, 3259, 2704, 279, 6894, 525, 2797, 323, 279, 821, 374, 13382, 13, 358, 3278, 3270, 419, 438, 264, 2033, 311, 862, 3239, 11, 8241, 279, 4718, 821, 62166, 624, 151649, 271, 8420, 374, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 320, 59604, 8, 304, 4718, 3561, 1447, 73594, 2236, 198, 515, 220, 330, 8926, 788, 330, 59604, 756, 220, 330, 11141, 788, 330, 49000, 756, 220, 330, 44441, 788, 220, 18, 20, 24, 24, 20, 15, 22, 345, 220, 330, 4059, 30511, 15030, 788, 220, 16, 17, 21, 21, 16, 22, 16, 21, 345, 220, 330, 4684, 788, 330, 785, 6722, 315, 9625, 11, 12095, 11, 374, 264, 3598, 3283, 7407, 304, 18172, 9625, 11, 3881, 369, 1181, 13656, 25361, 11, 59924, 11, 323, 2639, 438, 825, 315, 279, 1879, 594, 1429, 55654, 84909, 9720, 10040, 532, 73594, 151643], 'meta_info': {'id': '05202a8960cc4f5a93cafde147d2e5bc', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 341, 'cached_tokens': 22, 'cached_tokens_details': {'device': 22, 'host': 0}, 'dp_rank': None, 'e2e_latency': 3.413496260996908, 'response_sent_to_client_ts': 1775207931.5353355}}</strong>



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


    2026-04-03 09:19:04.467 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:19:04] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 09:19:04.467 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:19:04] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 09:19:04.467 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:19:04] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 09:19:04.467 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:19:04] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 09:19:04.467 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:19:04] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Multi-thread loading shards:  50% Completed | 1/2 [00:01<00:01,  1.12s/it]

    Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.25s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.23s/it]


    2026-04-03 09:19:12,527 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 09:19:12] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:04<04:02,  4.25s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:04<04:02,  4.25s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:04<01:50,  1.98s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:04<01:50,  1.98s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:03,  1.16s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:03,  1.16s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:05<00:41,  1.29it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:05<00:41,  1.29it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:29,  1.82it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:29,  1.82it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:21,  2.41it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:21,  2.41it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:16,  3.11it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:16,  3.11it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:05<00:12,  3.87it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:05<00:12,  3.87it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:05<00:10,  4.74it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:05<00:10,  4.74it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:05<00:09,  5.18it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:05<00:09,  5.18it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:05<00:09,  5.18it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:05<00:06,  6.98it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:05<00:06,  6.98it/s]

    Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:06<00:06,  6.98it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:06<00:05,  8.67it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:06<00:05,  8.67it/s]Compiling num tokens (num_tokens=2304):  24%|██▍       | 14/58 [00:06<00:05,  8.67it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:06<00:04, 10.48it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:06<00:04, 10.48it/s]

    Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:06<00:04, 10.48it/s]Compiling num tokens (num_tokens=1536):  28%|██▊       | 16/58 [00:06<00:04, 10.48it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:06<00:02, 13.54it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:06<00:02, 13.54it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:06<00:02, 13.54it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:06<00:02, 13.54it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:06<00:02, 13.54it/s]

    Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:06<00:01, 18.25it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:06<00:01, 18.25it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:06<00:01, 18.25it/s]Compiling num tokens (num_tokens=704):  40%|███▉      | 23/58 [00:06<00:01, 18.25it/s]Compiling num tokens (num_tokens=640):  40%|███▉      | 23/58 [00:06<00:01, 18.25it/s]Compiling num tokens (num_tokens=576):  40%|███▉      | 23/58 [00:06<00:01, 18.25it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:06<00:01, 24.71it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:06<00:01, 24.71it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:06<00:01, 24.71it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:06<00:01, 24.71it/s]Compiling num tokens (num_tokens=416):  48%|████▊     | 28/58 [00:06<00:01, 24.71it/s]Compiling num tokens (num_tokens=384):  48%|████▊     | 28/58 [00:06<00:01, 24.71it/s]

    Compiling num tokens (num_tokens=352):  48%|████▊     | 28/58 [00:06<00:01, 24.71it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:06<00:00, 32.10it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:06<00:00, 32.10it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:06<00:00, 32.10it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:06<00:00, 32.10it/s]Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:06<00:00, 32.10it/s]Compiling num tokens (num_tokens=224):  59%|█████▊    | 34/58 [00:06<00:00, 32.10it/s]Compiling num tokens (num_tokens=208):  59%|█████▊    | 34/58 [00:06<00:00, 32.10it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:06<00:00, 37.84it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:06<00:00, 37.84it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:06<00:00, 37.84it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:06<00:00, 37.84it/s]Compiling num tokens (num_tokens=144):  69%|██████▉   | 40/58 [00:06<00:00, 37.84it/s]

    Compiling num tokens (num_tokens=128):  69%|██████▉   | 40/58 [00:06<00:00, 37.84it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:06<00:00, 39.80it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:06<00:00, 39.80it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:07<00:00, 39.80it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:07<00:00, 39.80it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:07<00:00, 39.80it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:07<00:00, 39.80it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:07<00:00, 39.80it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:07<00:00, 44.12it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:07<00:00, 44.12it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:07<00:00, 44.12it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:07<00:00, 44.12it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:07<00:00, 44.12it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:07<00:00, 44.12it/s]

    Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:07<00:00, 44.12it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:07<00:00, 44.12it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:07<00:00,  8.07it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=103.94 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=103.94 GB):   2%|▏         | 1/58 [00:00<00:19,  2.94it/s]Capturing num tokens (num_tokens=7680 avail_mem=103.91 GB):   2%|▏         | 1/58 [00:00<00:19,  2.94it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=103.91 GB):   3%|▎         | 2/58 [00:00<00:16,  3.35it/s]Capturing num tokens (num_tokens=7168 avail_mem=103.91 GB):   3%|▎         | 2/58 [00:00<00:16,  3.35it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=103.91 GB):   5%|▌         | 3/58 [00:00<00:15,  3.56it/s]Capturing num tokens (num_tokens=6656 avail_mem=103.91 GB):   5%|▌         | 3/58 [00:00<00:15,  3.56it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=103.91 GB):   7%|▋         | 4/58 [00:01<00:16,  3.20it/s]Capturing num tokens (num_tokens=6144 avail_mem=103.61 GB):   7%|▋         | 4/58 [00:01<00:16,  3.20it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=103.61 GB):   9%|▊         | 5/58 [00:01<00:16,  3.12it/s]Capturing num tokens (num_tokens=5632 avail_mem=102.94 GB):   9%|▊         | 5/58 [00:01<00:16,  3.12it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=102.94 GB):  10%|█         | 6/58 [00:01<00:14,  3.52it/s]Capturing num tokens (num_tokens=5120 avail_mem=102.94 GB):  10%|█         | 6/58 [00:01<00:14,  3.52it/s]Capturing num tokens (num_tokens=5120 avail_mem=102.94 GB):  12%|█▏        | 7/58 [00:01<00:12,  4.05it/s]Capturing num tokens (num_tokens=4608 avail_mem=102.52 GB):  12%|█▏        | 7/58 [00:01<00:12,  4.05it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=102.52 GB):  14%|█▍        | 8/58 [00:02<00:10,  4.68it/s]Capturing num tokens (num_tokens=4096 avail_mem=102.53 GB):  14%|█▍        | 8/58 [00:02<00:10,  4.68it/s]Capturing num tokens (num_tokens=4096 avail_mem=102.53 GB):  16%|█▌        | 9/58 [00:02<00:09,  5.32it/s]Capturing num tokens (num_tokens=3840 avail_mem=102.53 GB):  16%|█▌        | 9/58 [00:02<00:09,  5.32it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=102.53 GB):  17%|█▋        | 10/58 [00:02<00:08,  5.74it/s]Capturing num tokens (num_tokens=3584 avail_mem=102.28 GB):  17%|█▋        | 10/58 [00:02<00:08,  5.74it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=102.28 GB):  19%|█▉        | 11/58 [00:02<00:09,  4.85it/s]Capturing num tokens (num_tokens=3328 avail_mem=102.26 GB):  19%|█▉        | 11/58 [00:02<00:09,  4.85it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=102.26 GB):  21%|██        | 12/58 [00:02<00:10,  4.23it/s]Capturing num tokens (num_tokens=3072 avail_mem=102.26 GB):  21%|██        | 12/58 [00:02<00:10,  4.23it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=102.26 GB):  22%|██▏       | 13/58 [00:03<00:12,  3.63it/s]Capturing num tokens (num_tokens=2816 avail_mem=102.26 GB):  22%|██▏       | 13/58 [00:03<00:12,  3.63it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=102.26 GB):  24%|██▍       | 14/58 [00:03<00:12,  3.43it/s]Capturing num tokens (num_tokens=2560 avail_mem=102.26 GB):  24%|██▍       | 14/58 [00:03<00:12,  3.43it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=102.26 GB):  26%|██▌       | 15/58 [00:03<00:12,  3.35it/s]Capturing num tokens (num_tokens=2304 avail_mem=102.26 GB):  26%|██▌       | 15/58 [00:03<00:12,  3.35it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=102.26 GB):  28%|██▊       | 16/58 [00:04<00:12,  3.33it/s]Capturing num tokens (num_tokens=2048 avail_mem=102.26 GB):  28%|██▊       | 16/58 [00:04<00:12,  3.33it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=102.26 GB):  29%|██▉       | 17/58 [00:04<00:11,  3.49it/s]Capturing num tokens (num_tokens=1792 avail_mem=102.26 GB):  29%|██▉       | 17/58 [00:04<00:11,  3.49it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=102.26 GB):  31%|███       | 18/58 [00:04<00:12,  3.25it/s]Capturing num tokens (num_tokens=1536 avail_mem=102.25 GB):  31%|███       | 18/58 [00:04<00:12,  3.25it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=102.25 GB):  33%|███▎      | 19/58 [00:05<00:11,  3.45it/s]Capturing num tokens (num_tokens=1280 avail_mem=102.25 GB):  33%|███▎      | 19/58 [00:05<00:11,  3.45it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=102.25 GB):  34%|███▍      | 20/58 [00:05<00:10,  3.67it/s]Capturing num tokens (num_tokens=1024 avail_mem=102.25 GB):  34%|███▍      | 20/58 [00:05<00:10,  3.67it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=102.25 GB):  36%|███▌      | 21/58 [00:05<00:09,  3.97it/s]Capturing num tokens (num_tokens=960 avail_mem=102.25 GB):  36%|███▌      | 21/58 [00:05<00:09,  3.97it/s] Capturing num tokens (num_tokens=960 avail_mem=102.25 GB):  38%|███▊      | 22/58 [00:05<00:08,  4.33it/s]Capturing num tokens (num_tokens=896 avail_mem=102.24 GB):  38%|███▊      | 22/58 [00:05<00:08,  4.33it/s]

    Capturing num tokens (num_tokens=896 avail_mem=102.24 GB):  40%|███▉      | 23/58 [00:05<00:07,  4.46it/s]Capturing num tokens (num_tokens=832 avail_mem=102.24 GB):  40%|███▉      | 23/58 [00:05<00:07,  4.46it/s]Capturing num tokens (num_tokens=832 avail_mem=102.24 GB):  41%|████▏     | 24/58 [00:06<00:06,  5.30it/s]Capturing num tokens (num_tokens=768 avail_mem=102.49 GB):  41%|████▏     | 24/58 [00:06<00:06,  5.30it/s]Capturing num tokens (num_tokens=704 avail_mem=102.48 GB):  41%|████▏     | 24/58 [00:06<00:06,  5.30it/s]

    Capturing num tokens (num_tokens=704 avail_mem=102.48 GB):  45%|████▍     | 26/58 [00:06<00:04,  7.33it/s]Capturing num tokens (num_tokens=640 avail_mem=102.48 GB):  45%|████▍     | 26/58 [00:06<00:04,  7.33it/s]Capturing num tokens (num_tokens=576 avail_mem=102.48 GB):  45%|████▍     | 26/58 [00:06<00:04,  7.33it/s]Capturing num tokens (num_tokens=576 avail_mem=102.48 GB):  48%|████▊     | 28/58 [00:06<00:03,  9.21it/s]Capturing num tokens (num_tokens=512 avail_mem=102.47 GB):  48%|████▊     | 28/58 [00:06<00:03,  9.21it/s]Capturing num tokens (num_tokens=480 avail_mem=102.47 GB):  48%|████▊     | 28/58 [00:06<00:03,  9.21it/s]

    Capturing num tokens (num_tokens=480 avail_mem=102.47 GB):  52%|█████▏    | 30/58 [00:06<00:02, 11.21it/s]Capturing num tokens (num_tokens=448 avail_mem=102.47 GB):  52%|█████▏    | 30/58 [00:06<00:02, 11.21it/s]Capturing num tokens (num_tokens=416 avail_mem=102.46 GB):  52%|█████▏    | 30/58 [00:06<00:02, 11.21it/s]Capturing num tokens (num_tokens=384 avail_mem=102.46 GB):  52%|█████▏    | 30/58 [00:06<00:02, 11.21it/s]Capturing num tokens (num_tokens=384 avail_mem=102.46 GB):  57%|█████▋    | 33/58 [00:06<00:01, 14.89it/s]Capturing num tokens (num_tokens=352 avail_mem=102.45 GB):  57%|█████▋    | 33/58 [00:06<00:01, 14.89it/s]Capturing num tokens (num_tokens=320 avail_mem=102.45 GB):  57%|█████▋    | 33/58 [00:06<00:01, 14.89it/s]Capturing num tokens (num_tokens=288 avail_mem=102.45 GB):  57%|█████▋    | 33/58 [00:06<00:01, 14.89it/s]Capturing num tokens (num_tokens=256 avail_mem=102.44 GB):  57%|█████▋    | 33/58 [00:06<00:01, 14.89it/s]

    Capturing num tokens (num_tokens=256 avail_mem=102.44 GB):  64%|██████▍   | 37/58 [00:06<00:01, 20.40it/s]Capturing num tokens (num_tokens=240 avail_mem=102.44 GB):  64%|██████▍   | 37/58 [00:06<00:01, 20.40it/s]Capturing num tokens (num_tokens=224 avail_mem=102.44 GB):  64%|██████▍   | 37/58 [00:06<00:01, 20.40it/s]Capturing num tokens (num_tokens=208 avail_mem=102.43 GB):  64%|██████▍   | 37/58 [00:06<00:01, 20.40it/s]Capturing num tokens (num_tokens=192 avail_mem=102.43 GB):  64%|██████▍   | 37/58 [00:06<00:01, 20.40it/s]Capturing num tokens (num_tokens=192 avail_mem=102.43 GB):  71%|███████   | 41/58 [00:06<00:00, 24.14it/s]Capturing num tokens (num_tokens=176 avail_mem=102.42 GB):  71%|███████   | 41/58 [00:06<00:00, 24.14it/s]Capturing num tokens (num_tokens=160 avail_mem=102.42 GB):  71%|███████   | 41/58 [00:06<00:00, 24.14it/s]Capturing num tokens (num_tokens=144 avail_mem=102.41 GB):  71%|███████   | 41/58 [00:06<00:00, 24.14it/s]

    Capturing num tokens (num_tokens=128 avail_mem=102.42 GB):  71%|███████   | 41/58 [00:06<00:00, 24.14it/s]Capturing num tokens (num_tokens=128 avail_mem=102.42 GB):  78%|███████▊  | 45/58 [00:06<00:00, 26.51it/s]Capturing num tokens (num_tokens=112 avail_mem=102.42 GB):  78%|███████▊  | 45/58 [00:06<00:00, 26.51it/s]Capturing num tokens (num_tokens=96 avail_mem=102.42 GB):  78%|███████▊  | 45/58 [00:06<00:00, 26.51it/s] Capturing num tokens (num_tokens=80 avail_mem=102.41 GB):  78%|███████▊  | 45/58 [00:06<00:00, 26.51it/s]Capturing num tokens (num_tokens=64 avail_mem=102.41 GB):  78%|███████▊  | 45/58 [00:07<00:00, 26.51it/s]Capturing num tokens (num_tokens=64 avail_mem=102.41 GB):  84%|████████▍ | 49/58 [00:07<00:00, 29.24it/s]Capturing num tokens (num_tokens=48 avail_mem=102.41 GB):  84%|████████▍ | 49/58 [00:07<00:00, 29.24it/s]Capturing num tokens (num_tokens=32 avail_mem=102.40 GB):  84%|████████▍ | 49/58 [00:07<00:00, 29.24it/s]Capturing num tokens (num_tokens=28 avail_mem=102.40 GB):  84%|████████▍ | 49/58 [00:07<00:00, 29.24it/s]

    Capturing num tokens (num_tokens=24 avail_mem=102.40 GB):  84%|████████▍ | 49/58 [00:07<00:00, 29.24it/s]Capturing num tokens (num_tokens=24 avail_mem=102.40 GB):  91%|█████████▏| 53/58 [00:07<00:00, 31.11it/s]Capturing num tokens (num_tokens=20 avail_mem=102.39 GB):  91%|█████████▏| 53/58 [00:07<00:00, 31.11it/s]Capturing num tokens (num_tokens=16 avail_mem=102.39 GB):  91%|█████████▏| 53/58 [00:07<00:00, 31.11it/s]Capturing num tokens (num_tokens=12 avail_mem=102.38 GB):  91%|█████████▏| 53/58 [00:07<00:00, 31.11it/s]Capturing num tokens (num_tokens=8 avail_mem=102.38 GB):  91%|█████████▏| 53/58 [00:07<00:00, 31.11it/s] Capturing num tokens (num_tokens=8 avail_mem=102.38 GB):  98%|█████████▊| 57/58 [00:07<00:00, 32.14it/s]Capturing num tokens (num_tokens=4 avail_mem=102.38 GB):  98%|█████████▊| 57/58 [00:07<00:00, 32.14it/s]Capturing num tokens (num_tokens=4 avail_mem=102.38 GB): 100%|██████████| 58/58 [00:07<00:00,  7.96it/s]


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
    Generated text: England
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
    
    Generated text: Alright, the user is asking for the information and population of the capital of France. I know that the capital is Paris. They specifically want this data in JSON format. I should make sure to include the population, but I also remember that population numbers can change over time. The current estimate is around 2.16 million, but it's probably better to note that the number might vary. I should present the information clearly, maybe with a population figure and a note about it changing. I'll format it neatly in JSON so it's easy to read and use for whatever they're planning.
    </think>
    
    Here is the information and population of the capital of France (Paris) in JSON format:
    
    ```json
    {
      "capital": "Paris",
      "population": 2161000
    }
    ```
    
    Note: The population figure may vary slightly depending on the source and year.



```python
llm.shutdown()
```
