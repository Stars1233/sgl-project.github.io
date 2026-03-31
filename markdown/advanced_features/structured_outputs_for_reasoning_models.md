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


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:176: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-03-31 03:57:36.400 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 03:57:36] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 03:57:36.400 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 03:57:36] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 03:57:36.400 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 03:57:36] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 03:57:36.401 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 03:57:36] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 03:57:36.401 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 03:57:36] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:01<00:01,  1.02s/it]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.41s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.35s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:56,  3.09s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:56,  3.09s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:24,  1.51s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:24,  1.51s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:49,  1.10it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:49,  1.10it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:33,  1.61it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:33,  1.61it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:24,  2.20it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:24,  2.20it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.84it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.84it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.58it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.58it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.30it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.30it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.17it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.17it/s]

    Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.17it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:06,  6.84it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:06,  6.84it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:06,  6.84it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:04<00:05,  8.27it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:04<00:05,  8.27it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:04<00:05,  8.27it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:05<00:04,  9.76it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:05<00:04,  9.76it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:05<00:04,  9.76it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:03, 11.62it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:03, 11.62it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:03, 11.62it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:05<00:03, 11.62it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 14.77it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 14.77it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 14.77it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 14.77it/s]

    Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:05<00:02, 14.77it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:05<00:01, 19.84it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:05<00:01, 19.84it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:05<00:01, 19.84it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:05<00:01, 19.84it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:05<00:01, 19.84it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:05<00:01, 19.84it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:05<00:01, 26.16it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:05<00:01, 26.16it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:05<00:01, 26.16it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:05<00:01, 26.16it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:05<00:01, 26.16it/s]

    Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:05<00:01, 26.16it/s]Compiling num tokens (num_tokens=320):  50%|█████     | 29/58 [00:05<00:01, 26.16it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:05<00:00, 33.24it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:05<00:00, 33.24it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:05<00:00, 33.24it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:05<00:00, 33.24it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:05<00:00, 33.24it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:05<00:00, 33.24it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:05<00:00, 33.24it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:05<00:00, 38.79it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:05<00:00, 38.79it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:05<00:00, 38.79it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:05<00:00, 38.79it/s]

    Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:05<00:00, 38.79it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:05<00:00, 38.79it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:05<00:00, 38.79it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:05<00:00, 42.19it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:05<00:00, 42.19it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:05<00:00, 42.19it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:05<00:00, 42.19it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:05<00:00, 42.19it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:05<00:00, 42.19it/s]Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:05<00:00, 42.19it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:05<00:00, 46.44it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:05<00:00, 46.44it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:05<00:00, 46.44it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:05<00:00, 46.44it/s]

    Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:06<00:00, 46.44it/s] Compiling num tokens (num_tokens=4):  91%|█████████▏| 53/58 [00:06<00:00, 46.44it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:06<00:00,  9.63it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=90.17 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=90.17 GB):   2%|▏         | 1/58 [00:00<00:17,  3.25it/s]Capturing num tokens (num_tokens=7680 avail_mem=90.13 GB):   2%|▏         | 1/58 [00:00<00:17,  3.25it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=90.13 GB):   3%|▎         | 2/58 [00:00<00:16,  3.49it/s]Capturing num tokens (num_tokens=7168 avail_mem=90.13 GB):   3%|▎         | 2/58 [00:00<00:16,  3.49it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=90.13 GB):   5%|▌         | 3/58 [00:00<00:14,  3.74it/s]Capturing num tokens (num_tokens=6656 avail_mem=90.13 GB):   5%|▌         | 3/58 [00:00<00:14,  3.74it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=90.13 GB):   7%|▋         | 4/58 [00:01<00:13,  4.03it/s]Capturing num tokens (num_tokens=6144 avail_mem=90.14 GB):   7%|▋         | 4/58 [00:01<00:13,  4.03it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=90.14 GB):   9%|▊         | 5/58 [00:01<00:12,  4.30it/s]Capturing num tokens (num_tokens=5632 avail_mem=90.14 GB):   9%|▊         | 5/58 [00:01<00:12,  4.30it/s]Capturing num tokens (num_tokens=5632 avail_mem=90.14 GB):  10%|█         | 6/58 [00:01<00:11,  4.69it/s]Capturing num tokens (num_tokens=5120 avail_mem=90.14 GB):  10%|█         | 6/58 [00:01<00:11,  4.69it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=90.14 GB):  12%|█▏        | 7/58 [00:01<00:10,  5.01it/s]Capturing num tokens (num_tokens=4608 avail_mem=87.36 GB):  12%|█▏        | 7/58 [00:01<00:10,  5.01it/s]Capturing num tokens (num_tokens=4608 avail_mem=87.36 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.46it/s]Capturing num tokens (num_tokens=4096 avail_mem=87.36 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.46it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=87.36 GB):  16%|█▌        | 9/58 [00:01<00:08,  5.84it/s]Capturing num tokens (num_tokens=3840 avail_mem=87.37 GB):  16%|█▌        | 9/58 [00:01<00:08,  5.84it/s]Capturing num tokens (num_tokens=3840 avail_mem=87.37 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.35it/s]Capturing num tokens (num_tokens=3584 avail_mem=87.36 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.35it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=87.36 GB):  19%|█▉        | 11/58 [00:02<00:06,  6.91it/s]Capturing num tokens (num_tokens=3328 avail_mem=87.36 GB):  19%|█▉        | 11/58 [00:02<00:06,  6.91it/s]Capturing num tokens (num_tokens=3328 avail_mem=87.36 GB):  21%|██        | 12/58 [00:02<00:06,  7.55it/s]Capturing num tokens (num_tokens=3072 avail_mem=87.36 GB):  21%|██        | 12/58 [00:02<00:06,  7.55it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=87.36 GB):  21%|██        | 12/58 [00:02<00:06,  7.55it/s]Capturing num tokens (num_tokens=2816 avail_mem=87.36 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.76it/s]Capturing num tokens (num_tokens=2560 avail_mem=87.36 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.76it/s]Capturing num tokens (num_tokens=2304 avail_mem=87.36 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.76it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=87.36 GB):  28%|██▊       | 16/58 [00:02<00:04, 10.09it/s]Capturing num tokens (num_tokens=2048 avail_mem=87.36 GB):  28%|██▊       | 16/58 [00:02<00:04, 10.09it/s]Capturing num tokens (num_tokens=1792 avail_mem=87.36 GB):  28%|██▊       | 16/58 [00:02<00:04, 10.09it/s]Capturing num tokens (num_tokens=1792 avail_mem=87.36 GB):  31%|███       | 18/58 [00:02<00:03, 11.66it/s]Capturing num tokens (num_tokens=1536 avail_mem=87.36 GB):  31%|███       | 18/58 [00:02<00:03, 11.66it/s]Capturing num tokens (num_tokens=1280 avail_mem=87.36 GB):  31%|███       | 18/58 [00:02<00:03, 11.66it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=87.36 GB):  31%|███       | 18/58 [00:02<00:03, 11.66it/s]Capturing num tokens (num_tokens=1024 avail_mem=87.36 GB):  36%|███▌      | 21/58 [00:02<00:02, 14.60it/s]Capturing num tokens (num_tokens=960 avail_mem=87.36 GB):  36%|███▌      | 21/58 [00:02<00:02, 14.60it/s] Capturing num tokens (num_tokens=896 avail_mem=87.35 GB):  36%|███▌      | 21/58 [00:02<00:02, 14.60it/s]Capturing num tokens (num_tokens=832 avail_mem=87.35 GB):  36%|███▌      | 21/58 [00:02<00:02, 14.60it/s]Capturing num tokens (num_tokens=832 avail_mem=87.35 GB):  41%|████▏     | 24/58 [00:02<00:01, 17.53it/s]Capturing num tokens (num_tokens=768 avail_mem=87.35 GB):  41%|████▏     | 24/58 [00:02<00:01, 17.53it/s]Capturing num tokens (num_tokens=704 avail_mem=87.34 GB):  41%|████▏     | 24/58 [00:02<00:01, 17.53it/s]

    Capturing num tokens (num_tokens=640 avail_mem=87.34 GB):  41%|████▏     | 24/58 [00:03<00:01, 17.53it/s]Capturing num tokens (num_tokens=640 avail_mem=87.34 GB):  47%|████▋     | 27/58 [00:03<00:01, 20.50it/s]Capturing num tokens (num_tokens=576 avail_mem=87.34 GB):  47%|████▋     | 27/58 [00:03<00:01, 20.50it/s]Capturing num tokens (num_tokens=512 avail_mem=87.33 GB):  47%|████▋     | 27/58 [00:03<00:01, 20.50it/s]Capturing num tokens (num_tokens=480 avail_mem=87.33 GB):  47%|████▋     | 27/58 [00:03<00:01, 20.50it/s]Capturing num tokens (num_tokens=448 avail_mem=87.33 GB):  47%|████▋     | 27/58 [00:03<00:01, 20.50it/s]Capturing num tokens (num_tokens=448 avail_mem=87.33 GB):  53%|█████▎    | 31/58 [00:03<00:01, 24.15it/s]Capturing num tokens (num_tokens=416 avail_mem=87.32 GB):  53%|█████▎    | 31/58 [00:03<00:01, 24.15it/s]Capturing num tokens (num_tokens=384 avail_mem=87.32 GB):  53%|█████▎    | 31/58 [00:03<00:01, 24.15it/s]

    Capturing num tokens (num_tokens=352 avail_mem=87.31 GB):  53%|█████▎    | 31/58 [00:03<00:01, 24.15it/s]Capturing num tokens (num_tokens=320 avail_mem=87.31 GB):  53%|█████▎    | 31/58 [00:03<00:01, 24.15it/s]Capturing num tokens (num_tokens=320 avail_mem=87.31 GB):  60%|██████    | 35/58 [00:03<00:00, 27.07it/s]Capturing num tokens (num_tokens=288 avail_mem=87.31 GB):  60%|██████    | 35/58 [00:03<00:00, 27.07it/s]Capturing num tokens (num_tokens=256 avail_mem=87.30 GB):  60%|██████    | 35/58 [00:03<00:00, 27.07it/s]Capturing num tokens (num_tokens=240 avail_mem=87.30 GB):  60%|██████    | 35/58 [00:03<00:00, 27.07it/s]Capturing num tokens (num_tokens=224 avail_mem=87.29 GB):  60%|██████    | 35/58 [00:03<00:00, 27.07it/s]Capturing num tokens (num_tokens=224 avail_mem=87.29 GB):  67%|██████▋   | 39/58 [00:03<00:00, 28.30it/s]Capturing num tokens (num_tokens=208 avail_mem=87.29 GB):  67%|██████▋   | 39/58 [00:03<00:00, 28.30it/s]

    Capturing num tokens (num_tokens=192 avail_mem=87.11 GB):  67%|██████▋   | 39/58 [00:03<00:00, 28.30it/s]Capturing num tokens (num_tokens=176 avail_mem=86.13 GB):  67%|██████▋   | 39/58 [00:03<00:00, 28.30it/s]

    Capturing num tokens (num_tokens=176 avail_mem=86.13 GB):  72%|███████▏  | 42/58 [00:03<00:00, 18.67it/s]Capturing num tokens (num_tokens=160 avail_mem=86.12 GB):  72%|███████▏  | 42/58 [00:03<00:00, 18.67it/s]Capturing num tokens (num_tokens=144 avail_mem=86.12 GB):  72%|███████▏  | 42/58 [00:03<00:00, 18.67it/s]Capturing num tokens (num_tokens=128 avail_mem=86.13 GB):  72%|███████▏  | 42/58 [00:03<00:00, 18.67it/s]

    Capturing num tokens (num_tokens=128 avail_mem=86.13 GB):  78%|███████▊  | 45/58 [00:04<00:00, 15.48it/s]Capturing num tokens (num_tokens=112 avail_mem=86.13 GB):  78%|███████▊  | 45/58 [00:04<00:00, 15.48it/s]Capturing num tokens (num_tokens=96 avail_mem=86.12 GB):  78%|███████▊  | 45/58 [00:04<00:00, 15.48it/s] Capturing num tokens (num_tokens=96 avail_mem=86.12 GB):  81%|████████  | 47/58 [00:04<00:00, 14.04it/s]Capturing num tokens (num_tokens=80 avail_mem=86.11 GB):  81%|████████  | 47/58 [00:04<00:00, 14.04it/s]

    Capturing num tokens (num_tokens=64 avail_mem=86.11 GB):  81%|████████  | 47/58 [00:04<00:00, 14.04it/s]Capturing num tokens (num_tokens=64 avail_mem=86.11 GB):  84%|████████▍ | 49/58 [00:04<00:00, 13.09it/s]Capturing num tokens (num_tokens=48 avail_mem=86.11 GB):  84%|████████▍ | 49/58 [00:04<00:00, 13.09it/s]Capturing num tokens (num_tokens=32 avail_mem=86.10 GB):  84%|████████▍ | 49/58 [00:04<00:00, 13.09it/s]

    Capturing num tokens (num_tokens=32 avail_mem=86.10 GB):  88%|████████▊ | 51/58 [00:04<00:00, 12.47it/s]Capturing num tokens (num_tokens=28 avail_mem=86.10 GB):  88%|████████▊ | 51/58 [00:04<00:00, 12.47it/s]Capturing num tokens (num_tokens=24 avail_mem=86.10 GB):  88%|████████▊ | 51/58 [00:04<00:00, 12.47it/s]Capturing num tokens (num_tokens=24 avail_mem=86.10 GB):  91%|█████████▏| 53/58 [00:04<00:00, 11.77it/s]Capturing num tokens (num_tokens=20 avail_mem=86.10 GB):  91%|█████████▏| 53/58 [00:04<00:00, 11.77it/s]

    Capturing num tokens (num_tokens=16 avail_mem=86.09 GB):  91%|█████████▏| 53/58 [00:04<00:00, 11.77it/s]Capturing num tokens (num_tokens=16 avail_mem=86.09 GB):  95%|█████████▍| 55/58 [00:04<00:00, 11.77it/s]Capturing num tokens (num_tokens=12 avail_mem=86.09 GB):  95%|█████████▍| 55/58 [00:04<00:00, 11.77it/s]Capturing num tokens (num_tokens=8 avail_mem=86.09 GB):  95%|█████████▍| 55/58 [00:05<00:00, 11.77it/s] 

    Capturing num tokens (num_tokens=8 avail_mem=86.09 GB):  98%|█████████▊| 57/58 [00:05<00:00, 11.47it/s]Capturing num tokens (num_tokens=4 avail_mem=86.08 GB):  98%|█████████▊| 57/58 [00:05<00:00, 11.47it/s]Capturing num tokens (num_tokens=4 avail_mem=86.08 GB): 100%|██████████| 58/58 [00:05<00:00, 10.99it/s]


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


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France and its population. I know that the capital of France is Paris, but I'm not exactly sure about the current population numbers. I remember that Paris is a very big city, but I think it's not the largest in the world. Maybe around 20 million? I'm not certain, though. I should check if that's correct.<br><br>Wait, I think the population might have changed a bit over the years. I recall reading somewhere that Paris has grown a lot, especially with the influx of people moving there for work. But I'm not sure if it's exactly 21 million or maybe a bit more. I should look up the latest data to confirm.<br><br>I also wonder if the population figure includes just the city proper or the entire metropolitan area. Sometimes, people talk about the metro area, which can be much larger. But I think the question is specifically about the capital, so probably just the city limits. <br><br>Another thing to consider is that population numbers can vary depending on the source. Some might cite estimates from government agencies, while others might use more recent surveys. I should make sure to get the most accurate and up-to-date information. Maybe I can refer to official statistics or recent reports from organizations like the United Nations or the World Bank.<br><br>I also recall that Paris has a diverse population, with people from all over the world. That might affect the overall population count, but I don't think it changes the fact that Paris is the capital. <br><br>To sum up, I'm pretty confident that Paris is the capital of France, but I'm a bit unsure about the exact population. I'll look up the latest data to confirm whether it's 21 million or another number. Once I have that, I can present the information in the JSON format as requested.<br><br><br>content: {<br><br>"name": "Paris",<br>"population": 21545000<br>}</strong>


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


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France and its population. I know that the capital of France is Paris, but I'm not exactly sure about the current population numbers. I remember that Paris is a very big city, but I think it's not the largest in the world. Maybe around 20 million? I'm not certain, though. I should check if that's correct.<br><br>Wait, I think the population might have changed a bit over the years. I recall reading somewhere that Paris has grown a lot, especially with the influx of people moving there for work. But I'm not sure if it's exactly 21 million or maybe a bit more. I should look up the latest data to confirm.<br><br>I also wonder if the population figure includes just the city proper or the entire metropolitan area. Sometimes, people talk about the metro area, which can be much larger. But I think the question is specifically about the capital, so probably just the city limits. <br><br>Another thing to consider is that population numbers can vary depending on the source. Some might cite estimates from government agencies, while others might use more recent surveys. I should make sure to get the most accurate and up-to-date information. Maybe I can refer to official statistics or recent reports from organizations like the United Nations or the World Bank.<br><br>I also recall that Paris has a diverse population, with people from all over the world. That might affect the overall population count, but I don't think it changes the fact that Paris is the capital. <br><br>To sum up, I'm pretty confident that Paris is the capital of France, but I'm a bit unsure about the exact population. I'll look up the latest data to confirm whether it's 21 million or another number. Once I have that, I can present the information in the JSON format as requested.<br><br><br>content: {<br><br>"name": "Paris",<br>"population": 21545000<br>}</strong>


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


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France and its population. I know that the capital of France is Paris, but I'm not entirely sure about the population number. I think it's a big city, so maybe around 3 million? But I'm not certain. I should probably double-check that. Maybe I can recall that Paris is one of the largest cities in Europe, so 3.5 million sounds about right. I don't think it's more than that because I've heard it's a major tourist attraction but not the largest in the world. So, I'll go with Paris having a population of approximately 3.5 million.<br><br><br>content: London is the capital of France</strong>


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


<strong style='color: #00008B;'>reasoing_content: Okay, I need to figure out how to respond to the user's request. They mentioned being in New York and want the current date and time, along with the weather. <br><br>First, I should determine which functions to use. The user mentioned two functions: get_current_weather and get_current_date. <br><br>For the date and time, I'll use get_current_date. The parameters required are the timezone. Since the user is in New York, I'll set the timezone parameter to 'America/New_York'. <br><br>Next, for the weather, I'll use get_current_weather. The parameters needed are city, state, and unit. The city is New York, the state is NY, and the unit should be Fahrenheit since the user didn't specify otherwise. <br><br>I need to structure each function call correctly. Each should start with <function=, followed by the function name and parameters in a JSON object, and end with </function>. <br><br>I'll make sure to include the sources in the response, indicating where each function's information comes from. <br><br>Putting it all together, I'll send two separate function calls: one for the date/time and another for the weather. This way, the user gets both pieces of information clearly.<br><br><br>content: <function=get_current_date>{"timezone": "America/New_York)}</function> <br> <br> <function=get_current_weather>{" <br>  <br>  }</function></strong>


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

    {'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. I know that Paris is the capital, so that\'s straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I\'m not sure of the exact number. I think it\'s around 2 million, but I should verify that.\n\nI\'ll check a reliable source, maybe the official Paris Municipality website or a recent census. Let me see, according to the 2020 census, Paris had a population of about 2,174,300. That seems accurate. I should make sure to include this number in the JSON.\n\nNext, I need to structure this information into a JSON format. The user wants a JSON, so I\'ll create an object with a "name" field for the city, a "population" field, and maybe a "description" for more context. The description can mention that Paris is the capital and a major city in France.\n\nI should also consider the format. The JSON should be properly formatted with keys and values, and each key should be a string. The population number should be an integer since it\'s a count of people.\n\nPutting it all together, the JSON will have the city name, population, and a brief description. I\'ll make sure the syntax is correct, with commas and brackets in the right places to avoid errors.\n\nFinally, I\'ll present the JSON to the user, keeping it simple and clear. I don\'t need to add extra information unless the user asks for it, so I\'ll stick to the basics they requested.\n</think>{\n\n"name": "Paris",\n"population": 2174300000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1265, 1477, 279, 1429, 3213, 7042, 821, 13, 358, 6099, 429, 279, 7042, 315, 12095, 702, 1012, 7826, 11, 714, 358, 2776, 537, 2704, 315, 279, 4734, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 10146, 429, 382, 40, 3278, 1779, 264, 14720, 2530, 11, 7196, 279, 3946, 12095, 35703, 2719, 3910, 476, 264, 3213, 43602, 13, 6771, 752, 1490, 11, 4092, 311, 279, 220, 17, 15, 17, 15, 43602, 11, 12095, 1030, 264, 7042, 315, 911, 220, 17, 11, 16, 22, 19, 11, 18, 15, 15, 13, 2938, 4977, 13382, 13, 358, 1265, 1281, 2704, 311, 2924, 419, 1372, 304, 279, 4718, 382, 5847, 11, 358, 1184, 311, 5944, 419, 1995, 1119, 264, 4718, 3561, 13, 576, 1196, 6801, 264, 4718, 11, 773, 358, 3278, 1855, 458, 1633, 448, 264, 330, 606, 1, 2070, 369, 279, 3283, 11, 264, 330, 44441, 1, 2070, 11, 323, 7196, 264, 330, 4684, 1, 369, 803, 2266, 13, 576, 4008, 646, 6286, 429, 12095, 374, 279, 6722, 323, 264, 3598, 3283, 304, 9625, 382, 40, 1265, 1083, 2908, 279, 3561, 13, 576, 4718, 1265, 387, 10277, 23126, 448, 6894, 323, 2750, 11, 323, 1817, 1376, 1265, 387, 264, 914, 13, 576, 7042, 1372, 1265, 387, 458, 7546, 2474, 432, 594, 264, 1760, 315, 1251, 382, 97904, 432, 678, 3786, 11, 279, 4718, 686, 614, 279, 3283, 829, 11, 7042, 11, 323, 264, 9814, 4008, 13, 358, 3278, 1281, 2704, 279, 19482, 374, 4396, 11, 448, 76602, 323, 38929, 304, 279, 1290, 7482, 311, 5648, 5975, 382, 23949, 11, 358, 3278, 3042, 279, 4718, 311, 279, 1196, 11, 10282, 432, 4285, 323, 2797, 13, 358, 1513, 944, 1184, 311, 912, 4960, 1995, 7241, 279, 1196, 17064, 369, 432, 11, 773, 358, 3278, 9214, 311, 279, 31774, 807, 11223, 624, 151649, 4257, 1, 606, 788, 330, 59604, 756, 1, 44441, 788, 220, 17, 16, 22, 19, 18, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15], 'meta_info': {'id': '8acf660cf1d243ffba3626ce620c672f', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 18.47640419891104, 'response_sent_to_client_ts': 1774929519.007955}}



<strong style='color: #00008B;'>reasoing_content: Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.<br><br>First, I need to identify the capital of France. I know that Paris is the capital, so that's straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I'm not sure of the exact number. I think it's around 2 million, but I should verify that.<br><br>I'll check a reliable source, maybe the official Paris Municipality website or a recent census. Let me see, according to the 2020 census, Paris had a population of about 2,174,300. That seems accurate. I should make sure to include this number in the JSON.<br><br>Next, I need to structure this information into a JSON format. The user wants a JSON, so I'll create an object with a "name" field for the city, a "population" field, and maybe a "description" for more context. The description can mention that Paris is the capital and a major city in France.<br><br>I should also consider the format. The JSON should be properly formatted with keys and values, and each key should be a string. The population number should be an integer since it's a count of people.<br><br>Putting it all together, the JSON will have the city name, population, and a brief description. I'll make sure the syntax is correct, with commas and brackets in the right places to avoid errors.<br><br>Finally, I'll present the JSON to the user, keeping it simple and clear. I don't need to add extra information unless the user asks for it, so I'll stick to the basics they requested.<br><br><br>content: {<br><br>"name": "Paris",<br>"population": 2174300000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000</strong>


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


<strong style='color: #00008B;'>{'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down. First, I need to identify what the capital of France is. I know that Paris is the capital, so that\'s the starting point.\n\nNext, I need to find the population of Paris. I remember that Paris is a major city with a large population, but I\'m not exactly sure of the current number. I think it\'s around 2 million, but I should double-check that. Maybe I can recall that it\'s approximately 2,150,000 as of recent estimates.\n\nNow, the user wants this information in JSON format. JSON stands for JavaScript Object Notation, which is a way to structure data. I need to create a JSON object that includes the key "capital" with the value "Paris" and another key "population" with the number I just thought of.\n\nI should make sure the JSON syntax is correct. That means using double quotes for keys and string values, and commas appropriately between key-value pairs. Also, the numbers should be in quotes if they\'re strings, but population is a number, so it should be without quotes.\n\nPutting it all together, the JSON object should look like this: {"capital": "Paris", "population": 2150000}. I should present this clearly so the user can easily understand and use the information.\n\nI wonder if the user needs more details, like the population figure\'s source or the exact year it was recorded. But since they didn\'t ask for that, I\'ll stick to the information requested. Maybe they just need a straightforward data structure for a program or a report.\n\nAlso, considering the user\'s request, they might be a student working on a project, or perhaps someone developing an app that requires geographical data. Either way, providing the information in a structured format like JSON would be helpful for their purposes.\n\nI should make sure there are no typos in the JSON to avoid any issues when the user tries to use it. Double-checking the spelling of "capital" and "population" is important. Also, ensuring that the commas are correctly placed between the key-value pairs.\n\nIn summary, the user needs the capital of France and its population in JSON. I\'ve identified Paris as the capital and approximately 2,150,000 as the population. Structuring this into a JSON object with appropriate keys and correct syntax should meet their needs.\n</think>{\n\n"name": "Paris",\n"population": 2150000}', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 13, 5512, 11, 358, 1184, 311, 10542, 1128, 279, 6722, 315, 9625, 374, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 279, 5916, 1459, 382, 5847, 11, 358, 1184, 311, 1477, 279, 7042, 315, 12095, 13, 358, 6099, 429, 12095, 374, 264, 3598, 3283, 448, 264, 3460, 7042, 11, 714, 358, 2776, 537, 6896, 2704, 315, 279, 1482, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 1990, 15934, 429, 13, 10696, 358, 646, 19091, 429, 432, 594, 13187, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 438, 315, 3213, 17530, 382, 7039, 11, 279, 1196, 6801, 419, 1995, 304, 4718, 3561, 13, 4718, 13352, 369, 12914, 3002, 2806, 367, 11, 892, 374, 264, 1616, 311, 5944, 821, 13, 358, 1184, 311, 1855, 264, 4718, 1633, 429, 5646, 279, 1376, 330, 65063, 1, 448, 279, 897, 330, 59604, 1, 323, 2441, 1376, 330, 44441, 1, 448, 279, 1372, 358, 1101, 3381, 315, 382, 40, 1265, 1281, 2704, 279, 4718, 19482, 374, 4396, 13, 2938, 3363, 1667, 1990, 17194, 369, 6894, 323, 914, 2750, 11, 323, 76602, 34901, 1948, 1376, 19083, 13530, 13, 7281, 11, 279, 5109, 1265, 387, 304, 17194, 421, 807, 2299, 9069, 11, 714, 7042, 374, 264, 1372, 11, 773, 432, 1265, 387, 2041, 17194, 382, 97904, 432, 678, 3786, 11, 279, 4718, 1633, 1265, 1401, 1075, 419, 25, 5212, 65063, 788, 330, 59604, 497, 330, 44441, 788, 220, 17, 16, 20, 15, 15, 15, 15, 7810, 358, 1265, 3042, 419, 9355, 773, 279, 1196, 646, 6707, 3535, 323, 990, 279, 1995, 382, 40, 5775, 421, 279, 1196, 3880, 803, 3565, 11, 1075, 279, 7042, 7071, 594, 2530, 476, 279, 4734, 1042, 432, 572, 12433, 13, 1988, 2474, 807, 3207, 944, 2548, 369, 429, 11, 358, 3278, 9214, 311, 279, 1995, 11223, 13, 10696, 807, 1101, 1184, 264, 30339, 821, 5944, 369, 264, 2025, 476, 264, 1895, 382, 13394, 11, 12831, 279, 1196, 594, 1681, 11, 807, 2578, 387, 264, 5458, 3238, 389, 264, 2390, 11, 476, 8365, 4325, 11220, 458, 906, 429, 7460, 52901, 821, 13, 20988, 1616, 11, 8241, 279, 1995, 304, 264, 32930, 3561, 1075, 4718, 1035, 387, 10950, 369, 862, 9895, 382, 40, 1265, 1281, 2704, 1052, 525, 902, 13580, 966, 304, 279, 4718, 311, 5648, 894, 4714, 979, 279, 1196, 16297, 311, 990, 432, 13, 7093, 15934, 287, 279, 42429, 315, 330, 65063, 1, 323, 330, 44441, 1, 374, 2989, 13, 7281, 11, 22573, 429, 279, 76602, 525, 12440, 9099, 1948, 279, 1376, 19083, 13530, 382, 641, 12126, 11, 279, 1196, 3880, 279, 6722, 315, 9625, 323, 1181, 7042, 304, 4718, 13, 358, 3003, 10820, 12095, 438, 279, 6722, 323, 13187, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 438, 279, 7042, 13, 16139, 1677, 419, 1119, 264, 4718, 1633, 448, 8311, 6894, 323, 4396, 19482, 1265, 3367, 862, 3880, 624, 151649, 4257, 1, 606, 788, 330, 59604, 756, 1, 44441, 788, 220, 17, 16, 20, 15, 15, 15, 15, 92, 151643], 'meta_info': {'id': 'bf4a3dc410264b9f982176ccc3ae8acd', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 528, 'cached_tokens': 22, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 4.672242330852896, 'response_sent_to_client_ts': 1774929523.6909688}}</strong>


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

    [{'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '1c72d83e51384668bb827696a707ee33', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.10528055112808943, 'response_sent_to_client_ts': 1774929523.818943}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': 'b23ff1d11d204bb9b5e52cd064edce93', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.1052235160022974, 'response_sent_to_client_ts': 1774929523.8189561}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': 'f9a5d8d74acd46de8b51d4318e94d5b7', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.10517850890755653, 'response_sent_to_client_ts': 1774929523.8189604}}]


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

    {'text': ' France, and the \n\\( n \\)  \\( m \\) \\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\)', 'output_ids': [9625, 11, 323, 279, 220, 198, 44292, 308, 1124, 8, 220, 17767, 296, 1124, 8, 17767, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8], 'meta_info': {'id': 'f50040d326be4d33a8ac9acb41314112', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 6, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 20.576345436275005, 'response_sent_to_client_ts': 1774929544.4021726}}


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


<strong style='color: #00008B;'>{'text': 'Alright, the user is asking for the capital of France along with its population, and they specifically want it in JSON format. \n\nI know that the capital of France is Paris. I should make sure that\'s clear. \n\nNext, the population. Paris is a big city, so I expect a large number. I remember it\'s over 3.5 million, but I should double-check that. I\'ll say approximately 3,700,000 in 2023, but I\'ll note that it\'s subject to change slightly due to ongoing development and population shifts.\n\nNow, to present this in JSON. I need to structure it with a key for "capital" and a key for "population." Using double quotes inside the JSON for readability is important, but I also remember that JSON allows for either single or double quotes, so I\'ll stick with double quotes for consistency and clarity.\n\nPutting it all together, I should make sure the JSON syntax is correct—proper commas, colons, and brackets. No trailing commas to avoid errors. \n\nFinally, I\'ll offer to provide more details if needed, showing that I\'m open to further assistance. That should cover everything the user asked for and make them feel supported if they have more questions later.\n</think>\n\nHere is the information about the capital of France in JSON format:\n\n```json\n{\n  "capital": "Paris",\n  "population": "Approximately 3,700,000 (in 2023)"\n}\n```', 'output_ids': [71486, 11, 279, 1196, 374, 10161, 369, 279, 6722, 315, 9625, 3156, 448, 1181, 7042, 11, 323, 807, 11689, 1366, 432, 304, 4718, 3561, 13, 4710, 40, 1414, 429, 279, 6722, 315, 9625, 374, 12095, 13, 358, 1265, 1281, 2704, 429, 594, 2797, 13, 4710, 5847, 11, 279, 7042, 13, 12095, 374, 264, 2409, 3283, 11, 773, 358, 1720, 264, 3460, 1372, 13, 358, 6099, 432, 594, 916, 220, 18, 13, 20, 3526, 11, 714, 358, 1265, 1990, 15934, 429, 13, 358, 3278, 1977, 13187, 220, 18, 11, 22, 15, 15, 11, 15, 15, 15, 304, 220, 17, 15, 17, 18, 11, 714, 358, 3278, 5185, 429, 432, 594, 3832, 311, 2297, 10078, 4152, 311, 14195, 4401, 323, 7042, 28635, 382, 7039, 11, 311, 3042, 419, 304, 4718, 13, 358, 1184, 311, 5944, 432, 448, 264, 1376, 369, 330, 65063, 1, 323, 264, 1376, 369, 330, 44441, 1189, 12091, 1990, 17194, 4766, 279, 4718, 369, 91494, 374, 2989, 11, 714, 358, 1083, 6099, 429, 4718, 6147, 369, 2987, 3175, 476, 1990, 17194, 11, 773, 358, 3278, 9214, 448, 1990, 17194, 369, 28137, 323, 31273, 382, 97904, 432, 678, 3786, 11, 358, 1265, 1281, 2704, 279, 4718, 19482, 374, 4396, 2293, 80668, 76602, 11, 1375, 2382, 11, 323, 38929, 13, 2308, 27748, 76602, 311, 5648, 5975, 13, 4710, 23949, 11, 358, 3278, 3010, 311, 3410, 803, 3565, 421, 4362, 11, 9027, 429, 358, 2776, 1787, 311, 4623, 12994, 13, 2938, 1265, 3421, 4297, 279, 1196, 4588, 369, 323, 1281, 1105, 2666, 7248, 421, 807, 614, 803, 4755, 2937, 624, 151649, 271, 8420, 374, 279, 1995, 911, 279, 6722, 315, 9625, 304, 4718, 3561, 1447, 73594, 2236, 198, 515, 220, 330, 65063, 788, 330, 59604, 756, 220, 330, 44441, 788, 330, 69520, 7108, 220, 18, 11, 22, 15, 15, 11, 15, 15, 15, 320, 258, 220, 17, 15, 17, 18, 12954, 532, 73594, 151643], 'meta_info': {'id': '09a0323b5e0f48338cfa9e2dd880e4d2', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 312, 'cached_tokens': 22, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 2.941055051982403, 'response_sent_to_client_ts': 1774929547.3527524}}</strong>



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


    2026-03-31 03:59:20.973 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 03:59:20] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 03:59:20.973 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 03:59:20] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 03:59:20.973 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 03:59:20] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 03:59:20.974 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 03:59:20] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 03:59:20.974 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 03:59:20] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:01<00:01,  1.05s/it]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.44s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.38s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:00,  3.17s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:00,  3.17s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:25,  1.53s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:25,  1.53s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:50,  1.08it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:50,  1.08it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:33,  1.59it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:33,  1.59it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:24,  2.18it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:24,  2.18it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.81it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.81it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.55it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.55it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.32it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.32it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.20it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.20it/s]

    Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.20it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:06,  6.84it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:06,  6.84it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:06,  6.84it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:04<00:05,  8.32it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:04<00:05,  8.32it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:05<00:05,  8.32it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:05<00:04,  9.90it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:05<00:04,  9.90it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:05<00:04,  9.90it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:03, 11.85it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:03, 11.85it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:03, 11.85it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:05<00:03, 11.85it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 15.11it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 15.11it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 15.11it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 15.11it/s]

    Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:05<00:02, 15.11it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:05<00:01, 19.81it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:05<00:01, 19.81it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:05<00:01, 19.81it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:05<00:01, 19.81it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:05<00:01, 19.81it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:05<00:01, 19.81it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:05<00:01, 25.57it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:05<00:01, 25.57it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:05<00:01, 25.57it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:05<00:01, 25.57it/s]

    Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:05<00:01, 25.57it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:05<00:00, 28.57it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:05<00:00, 28.57it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:05<00:00, 28.57it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:05<00:00, 28.57it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:05<00:00, 28.57it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:05<00:00, 28.57it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:05<00:00, 33.05it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:05<00:00, 33.05it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:05<00:00, 33.05it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:05<00:00, 33.05it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:05<00:00, 33.05it/s]

    Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:05<00:00, 33.05it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:05<00:00, 33.05it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:05<00:00, 38.23it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:05<00:00, 38.23it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:05<00:00, 38.23it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:05<00:00, 38.23it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:05<00:00, 38.23it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:05<00:00, 38.23it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:06<00:00, 38.23it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:06<00:00, 42.48it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:06<00:00, 42.48it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:06<00:00, 42.48it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:06<00:00, 42.48it/s]

    Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:06<00:00, 42.48it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:06<00:00, 42.48it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:06<00:00, 42.48it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:06<00:00, 42.48it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:06<00:00, 42.48it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:06<00:00, 51.37it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:06<00:00,  9.46it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=88.15 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=88.15 GB):   2%|▏         | 1/58 [00:00<00:16,  3.44it/s]Capturing num tokens (num_tokens=7680 avail_mem=86.84 GB):   2%|▏         | 1/58 [00:00<00:16,  3.44it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=86.84 GB):   3%|▎         | 2/58 [00:00<00:16,  3.49it/s]Capturing num tokens (num_tokens=7168 avail_mem=86.83 GB):   3%|▎         | 2/58 [00:00<00:16,  3.49it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=86.83 GB):   5%|▌         | 3/58 [00:00<00:14,  3.77it/s]Capturing num tokens (num_tokens=6656 avail_mem=86.84 GB):   5%|▌         | 3/58 [00:00<00:14,  3.77it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=86.84 GB):   7%|▋         | 4/58 [00:01<00:13,  4.09it/s]Capturing num tokens (num_tokens=6144 avail_mem=86.84 GB):   7%|▋         | 4/58 [00:01<00:13,  4.09it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=86.84 GB):   9%|▊         | 5/58 [00:01<00:12,  4.36it/s]Capturing num tokens (num_tokens=5632 avail_mem=86.84 GB):   9%|▊         | 5/58 [00:01<00:12,  4.36it/s]Capturing num tokens (num_tokens=5632 avail_mem=86.84 GB):  10%|█         | 6/58 [00:01<00:11,  4.72it/s]Capturing num tokens (num_tokens=5120 avail_mem=86.84 GB):  10%|█         | 6/58 [00:01<00:11,  4.72it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=86.84 GB):  12%|█▏        | 7/58 [00:01<00:10,  5.09it/s]Capturing num tokens (num_tokens=4608 avail_mem=86.85 GB):  12%|█▏        | 7/58 [00:01<00:10,  5.09it/s]Capturing num tokens (num_tokens=4608 avail_mem=86.85 GB):  14%|█▍        | 8/58 [00:01<00:08,  5.60it/s]Capturing num tokens (num_tokens=4096 avail_mem=86.85 GB):  14%|█▍        | 8/58 [00:01<00:08,  5.60it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=86.85 GB):  16%|█▌        | 9/58 [00:01<00:08,  6.09it/s]Capturing num tokens (num_tokens=3840 avail_mem=86.86 GB):  16%|█▌        | 9/58 [00:01<00:08,  6.09it/s]Capturing num tokens (num_tokens=3840 avail_mem=86.86 GB):  17%|█▋        | 10/58 [00:01<00:07,  6.58it/s]Capturing num tokens (num_tokens=3584 avail_mem=86.85 GB):  17%|█▋        | 10/58 [00:01<00:07,  6.58it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=86.85 GB):  19%|█▉        | 11/58 [00:02<00:06,  7.07it/s]Capturing num tokens (num_tokens=3328 avail_mem=86.85 GB):  19%|█▉        | 11/58 [00:02<00:06,  7.07it/s]Capturing num tokens (num_tokens=3328 avail_mem=86.85 GB):  21%|██        | 12/58 [00:02<00:05,  7.69it/s]Capturing num tokens (num_tokens=3072 avail_mem=86.85 GB):  21%|██        | 12/58 [00:02<00:05,  7.69it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=86.85 GB):  21%|██        | 12/58 [00:02<00:05,  7.69it/s]Capturing num tokens (num_tokens=2816 avail_mem=86.85 GB):  24%|██▍       | 14/58 [00:02<00:04,  8.84it/s]Capturing num tokens (num_tokens=2560 avail_mem=86.85 GB):  24%|██▍       | 14/58 [00:02<00:04,  8.84it/s]Capturing num tokens (num_tokens=2304 avail_mem=86.85 GB):  24%|██▍       | 14/58 [00:02<00:04,  8.84it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=86.85 GB):  28%|██▊       | 16/58 [00:02<00:04, 10.18it/s]Capturing num tokens (num_tokens=2048 avail_mem=86.85 GB):  28%|██▊       | 16/58 [00:02<00:04, 10.18it/s]Capturing num tokens (num_tokens=1792 avail_mem=86.85 GB):  28%|██▊       | 16/58 [00:02<00:04, 10.18it/s]Capturing num tokens (num_tokens=1792 avail_mem=86.85 GB):  31%|███       | 18/58 [00:02<00:03, 11.72it/s]Capturing num tokens (num_tokens=1536 avail_mem=86.85 GB):  31%|███       | 18/58 [00:02<00:03, 11.72it/s]Capturing num tokens (num_tokens=1280 avail_mem=86.85 GB):  31%|███       | 18/58 [00:02<00:03, 11.72it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=86.85 GB):  31%|███       | 18/58 [00:02<00:03, 11.72it/s]Capturing num tokens (num_tokens=1024 avail_mem=86.85 GB):  36%|███▌      | 21/58 [00:02<00:02, 14.65it/s]Capturing num tokens (num_tokens=960 avail_mem=86.85 GB):  36%|███▌      | 21/58 [00:02<00:02, 14.65it/s] Capturing num tokens (num_tokens=896 avail_mem=86.82 GB):  36%|███▌      | 21/58 [00:02<00:02, 14.65it/s]Capturing num tokens (num_tokens=832 avail_mem=86.82 GB):  36%|███▌      | 21/58 [00:02<00:02, 14.65it/s]Capturing num tokens (num_tokens=832 avail_mem=86.82 GB):  41%|████▏     | 24/58 [00:02<00:02, 16.69it/s]Capturing num tokens (num_tokens=768 avail_mem=86.81 GB):  41%|████▏     | 24/58 [00:02<00:02, 16.69it/s]

    Capturing num tokens (num_tokens=704 avail_mem=86.81 GB):  41%|████▏     | 24/58 [00:02<00:02, 16.69it/s]Capturing num tokens (num_tokens=704 avail_mem=86.81 GB):  45%|████▍     | 26/58 [00:03<00:01, 16.93it/s]Capturing num tokens (num_tokens=640 avail_mem=86.81 GB):  45%|████▍     | 26/58 [00:03<00:01, 16.93it/s]Capturing num tokens (num_tokens=576 avail_mem=86.80 GB):  45%|████▍     | 26/58 [00:03<00:01, 16.93it/s]Capturing num tokens (num_tokens=576 avail_mem=86.80 GB):  48%|████▊     | 28/58 [00:03<00:01, 17.29it/s]Capturing num tokens (num_tokens=512 avail_mem=86.80 GB):  48%|████▊     | 28/58 [00:03<00:01, 17.29it/s]

    Capturing num tokens (num_tokens=480 avail_mem=86.80 GB):  48%|████▊     | 28/58 [00:03<00:01, 17.29it/s]Capturing num tokens (num_tokens=448 avail_mem=86.79 GB):  48%|████▊     | 28/58 [00:03<00:01, 17.29it/s]Capturing num tokens (num_tokens=448 avail_mem=86.79 GB):  53%|█████▎    | 31/58 [00:03<00:01, 18.77it/s]Capturing num tokens (num_tokens=416 avail_mem=86.79 GB):  53%|█████▎    | 31/58 [00:03<00:01, 18.77it/s]Capturing num tokens (num_tokens=384 avail_mem=86.79 GB):  53%|█████▎    | 31/58 [00:03<00:01, 18.77it/s]Capturing num tokens (num_tokens=352 avail_mem=86.78 GB):  53%|█████▎    | 31/58 [00:03<00:01, 18.77it/s]

    Capturing num tokens (num_tokens=352 avail_mem=86.78 GB):  59%|█████▊    | 34/58 [00:03<00:01, 19.90it/s]Capturing num tokens (num_tokens=320 avail_mem=86.78 GB):  59%|█████▊    | 34/58 [00:03<00:01, 19.90it/s]Capturing num tokens (num_tokens=288 avail_mem=86.77 GB):  59%|█████▊    | 34/58 [00:03<00:01, 19.90it/s]Capturing num tokens (num_tokens=256 avail_mem=86.77 GB):  59%|█████▊    | 34/58 [00:03<00:01, 19.90it/s]Capturing num tokens (num_tokens=256 avail_mem=86.77 GB):  64%|██████▍   | 37/58 [00:03<00:00, 21.31it/s]Capturing num tokens (num_tokens=240 avail_mem=85.61 GB):  64%|██████▍   | 37/58 [00:03<00:00, 21.31it/s]

    Capturing num tokens (num_tokens=224 avail_mem=85.61 GB):  64%|██████▍   | 37/58 [00:03<00:00, 21.31it/s]Capturing num tokens (num_tokens=208 avail_mem=85.60 GB):  64%|██████▍   | 37/58 [00:03<00:00, 21.31it/s]

    Capturing num tokens (num_tokens=208 avail_mem=85.60 GB):  69%|██████▉   | 40/58 [00:03<00:01, 15.25it/s]Capturing num tokens (num_tokens=192 avail_mem=104.71 GB):  69%|██████▉   | 40/58 [00:03<00:01, 15.25it/s]

    Capturing num tokens (num_tokens=176 avail_mem=104.71 GB):  69%|██████▉   | 40/58 [00:04<00:01, 15.25it/s]Capturing num tokens (num_tokens=176 avail_mem=104.71 GB):  72%|███████▏  | 42/58 [00:04<00:01, 11.48it/s]Capturing num tokens (num_tokens=160 avail_mem=104.70 GB):  72%|███████▏  | 42/58 [00:04<00:01, 11.48it/s]Capturing num tokens (num_tokens=144 avail_mem=104.70 GB):  72%|███████▏  | 42/58 [00:04<00:01, 11.48it/s]

    Capturing num tokens (num_tokens=144 avail_mem=104.70 GB):  76%|███████▌  | 44/58 [00:04<00:01, 11.11it/s]Capturing num tokens (num_tokens=128 avail_mem=104.71 GB):  76%|███████▌  | 44/58 [00:04<00:01, 11.11it/s]Capturing num tokens (num_tokens=112 avail_mem=104.71 GB):  76%|███████▌  | 44/58 [00:04<00:01, 11.11it/s]Capturing num tokens (num_tokens=112 avail_mem=104.71 GB):  79%|███████▉  | 46/58 [00:04<00:01, 11.07it/s]Capturing num tokens (num_tokens=96 avail_mem=104.70 GB):  79%|███████▉  | 46/58 [00:04<00:01, 11.07it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=104.70 GB):  79%|███████▉  | 46/58 [00:04<00:01, 11.07it/s]Capturing num tokens (num_tokens=80 avail_mem=104.70 GB):  83%|████████▎ | 48/58 [00:04<00:00, 11.07it/s]Capturing num tokens (num_tokens=64 avail_mem=104.69 GB):  83%|████████▎ | 48/58 [00:04<00:00, 11.07it/s]Capturing num tokens (num_tokens=48 avail_mem=104.69 GB):  83%|████████▎ | 48/58 [00:04<00:00, 11.07it/s]

    Capturing num tokens (num_tokens=48 avail_mem=104.69 GB):  86%|████████▌ | 50/58 [00:04<00:00, 10.89it/s]Capturing num tokens (num_tokens=32 avail_mem=104.69 GB):  86%|████████▌ | 50/58 [00:04<00:00, 10.89it/s]Capturing num tokens (num_tokens=28 avail_mem=104.69 GB):  86%|████████▌ | 50/58 [00:05<00:00, 10.89it/s]Capturing num tokens (num_tokens=28 avail_mem=104.69 GB):  90%|████████▉ | 52/58 [00:05<00:00, 10.84it/s]Capturing num tokens (num_tokens=24 avail_mem=104.68 GB):  90%|████████▉ | 52/58 [00:05<00:00, 10.84it/s]

    Capturing num tokens (num_tokens=20 avail_mem=104.68 GB):  90%|████████▉ | 52/58 [00:05<00:00, 10.84it/s]Capturing num tokens (num_tokens=20 avail_mem=104.68 GB):  93%|█████████▎| 54/58 [00:05<00:00, 10.94it/s]Capturing num tokens (num_tokens=16 avail_mem=104.67 GB):  93%|█████████▎| 54/58 [00:05<00:00, 10.94it/s]Capturing num tokens (num_tokens=12 avail_mem=104.67 GB):  93%|█████████▎| 54/58 [00:05<00:00, 10.94it/s]

    Capturing num tokens (num_tokens=12 avail_mem=104.67 GB):  97%|█████████▋| 56/58 [00:05<00:00, 10.75it/s]Capturing num tokens (num_tokens=8 avail_mem=104.67 GB):  97%|█████████▋| 56/58 [00:05<00:00, 10.75it/s] Capturing num tokens (num_tokens=4 avail_mem=104.66 GB):  97%|█████████▋| 56/58 [00:05<00:00, 10.75it/s]Capturing num tokens (num_tokens=4 avail_mem=104.66 GB): 100%|██████████| 58/58 [00:05<00:00, 10.89it/s]Capturing num tokens (num_tokens=4 avail_mem=104.66 GB): 100%|██████████| 58/58 [00:05<00:00, 10.23it/s]


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
    Generated text: Berlin is the capital of Italy


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
    
    Generated text: Okay, so the user is asking for the capital of France and its population in JSON format. Let me break this down. First, I know that the capital of France is Paris. That's pretty straightforward. Now, regarding the population, I remember that Paris is one of the largest cities in the world, but I'm not exactly sure of the current number. I think it's over 3 million people, but it might have grown a bit. 
    
    I should check if the population is up to date. I recall that major cities like Paris have been growing steadily over the years. Maybe around 3.5 million? I'm not 100% certain, so it's better to provide an approximate figure. Also, I should consider whether to include the exact number or a rounded figure. Since population data can vary, giving an approximate value might be more helpful.
    
    Now, the user specifically wants the information in JSON format. JSON is a data interchange format, so I'll need to structure it with keys for the capital and population. The capital will be a string, "Paris", and the population will be a number. I should make sure to represent the number correctly, perhaps with commas for readability, like 3,500,000.
    
    I should also note that population figures can change due to various factors such as births, deaths, and migration. It's good practice to mention that the number might vary slightly depending on the source or the year it's recorded. That way, the user is aware that the figure isn't static and might change over time.
    
    Putting it all together, I'll create a JSON object with the keys "capital" and "population". I'll fill in "Paris" for the capital and approximately 3,500,000 for the population, along with the note about the population figure changing. This should meet the user's request effectively and provide them with the necessary information in the correct format.
    </think>
    
    ```json
    {
      "capital": "Paris",
      "population": 3500000,
      "note": "Population figures may vary slightly depending on the source or year."
    }
    ```



```python
llm.shutdown()
```
