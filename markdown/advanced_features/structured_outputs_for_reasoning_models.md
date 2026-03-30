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
    2026-03-30 21:39:47.807 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 21:39:47] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 21:39:47.807 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 21:39:47] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 21:39:47.807 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 21:39:47] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 21:39:47.807 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 21:39:47] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 21:39:47.807 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 21:39:47] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:01<00:01,  1.04s/it]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.46s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.39s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:53,  3.05s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:53,  3.05s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:23,  1.49s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:23,  1.49s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:49,  1.11it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:49,  1.11it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:33,  1.62it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:33,  1.62it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:03<00:23,  2.22it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:03<00:23,  2.22it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.86it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.86it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.58it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.58it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.33it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.33it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.15it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.15it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:04<00:08,  5.95it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:04<00:08,  5.95it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:04<00:08,  5.95it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:04<00:05,  7.71it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:04<00:05,  7.71it/s]

    Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:04<00:05,  7.71it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:04<00:04,  9.29it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:04<00:04,  9.29it/s]Compiling num tokens (num_tokens=2304):  24%|██▍       | 14/58 [00:05<00:04,  9.29it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:05<00:03, 11.00it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:05<00:03, 11.00it/s]

    Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:05<00:03, 11.00it/s]Compiling num tokens (num_tokens=1536):  28%|██▊       | 16/58 [00:05<00:03, 11.00it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:05<00:02, 14.04it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:05<00:02, 14.04it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:05<00:02, 14.04it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:05<00:02, 14.04it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:05<00:02, 14.04it/s]

    Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:05<00:01, 18.75it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:05<00:01, 18.75it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:05<00:01, 18.75it/s]Compiling num tokens (num_tokens=704):  40%|███▉      | 23/58 [00:05<00:01, 18.75it/s]Compiling num tokens (num_tokens=640):  40%|███▉      | 23/58 [00:05<00:01, 18.75it/s]Compiling num tokens (num_tokens=576):  40%|███▉      | 23/58 [00:05<00:01, 18.75it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:05<00:01, 25.12it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:05<00:01, 25.12it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:05<00:01, 25.12it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:05<00:01, 25.12it/s]Compiling num tokens (num_tokens=416):  48%|████▊     | 28/58 [00:05<00:01, 25.12it/s]Compiling num tokens (num_tokens=384):  48%|████▊     | 28/58 [00:05<00:01, 25.12it/s]

    Compiling num tokens (num_tokens=352):  48%|████▊     | 28/58 [00:05<00:01, 25.12it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:05<00:00, 32.59it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:05<00:00, 32.59it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:05<00:00, 32.59it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:05<00:00, 32.59it/s]Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:05<00:00, 32.59it/s]Compiling num tokens (num_tokens=224):  59%|█████▊    | 34/58 [00:05<00:00, 32.59it/s]Compiling num tokens (num_tokens=208):  59%|█████▊    | 34/58 [00:05<00:00, 32.59it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:05<00:00, 38.35it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:05<00:00, 38.35it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:05<00:00, 38.35it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:05<00:00, 38.35it/s]Compiling num tokens (num_tokens=144):  69%|██████▉   | 40/58 [00:05<00:00, 38.35it/s]

    Compiling num tokens (num_tokens=128):  69%|██████▉   | 40/58 [00:05<00:00, 38.35it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:05<00:00, 41.24it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:05<00:00, 41.24it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:05<00:00, 41.24it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:05<00:00, 41.24it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:05<00:00, 41.24it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:05<00:00, 41.24it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:05<00:00, 41.24it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:05<00:00, 45.32it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:05<00:00, 45.32it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:05<00:00, 45.32it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:05<00:00, 45.32it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:05<00:00, 45.32it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:05<00:00, 45.32it/s]

    Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:05<00:00, 45.32it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:05<00:00, 45.32it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:05<00:00,  9.70it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=87.37 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=87.37 GB):   2%|▏         | 1/58 [00:00<00:16,  3.37it/s]Capturing num tokens (num_tokens=7680 avail_mem=87.34 GB):   2%|▏         | 1/58 [00:00<00:16,  3.37it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=87.34 GB):   3%|▎         | 2/58 [00:00<00:15,  3.57it/s]Capturing num tokens (num_tokens=7168 avail_mem=87.34 GB):   3%|▎         | 2/58 [00:00<00:15,  3.57it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=87.34 GB):   5%|▌         | 3/58 [00:00<00:14,  3.81it/s]Capturing num tokens (num_tokens=6656 avail_mem=87.35 GB):   5%|▌         | 3/58 [00:00<00:14,  3.81it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=87.35 GB):   7%|▋         | 4/58 [00:01<00:13,  4.11it/s]Capturing num tokens (num_tokens=6144 avail_mem=87.35 GB):   7%|▋         | 4/58 [00:01<00:13,  4.11it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=87.35 GB):   9%|▊         | 5/58 [00:01<00:12,  4.36it/s]Capturing num tokens (num_tokens=5632 avail_mem=87.35 GB):   9%|▊         | 5/58 [00:01<00:12,  4.36it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=87.35 GB):  10%|█         | 6/58 [00:01<00:13,  3.86it/s]Capturing num tokens (num_tokens=5120 avail_mem=86.20 GB):  10%|█         | 6/58 [00:01<00:13,  3.86it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=86.20 GB):  12%|█▏        | 7/58 [00:01<00:15,  3.37it/s]Capturing num tokens (num_tokens=4608 avail_mem=86.20 GB):  12%|█▏        | 7/58 [00:01<00:15,  3.37it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=86.20 GB):  14%|█▍        | 8/58 [00:02<00:15,  3.30it/s]Capturing num tokens (num_tokens=4096 avail_mem=86.21 GB):  14%|█▍        | 8/58 [00:02<00:15,  3.30it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=86.21 GB):  16%|█▌        | 9/58 [00:02<00:14,  3.28it/s]Capturing num tokens (num_tokens=3840 avail_mem=86.21 GB):  16%|█▌        | 9/58 [00:02<00:14,  3.28it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=86.21 GB):  17%|█▋        | 10/58 [00:02<00:14,  3.32it/s]Capturing num tokens (num_tokens=3584 avail_mem=86.21 GB):  17%|█▋        | 10/58 [00:02<00:14,  3.32it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=86.21 GB):  19%|█▉        | 11/58 [00:03<00:13,  3.42it/s]Capturing num tokens (num_tokens=3328 avail_mem=86.21 GB):  19%|█▉        | 11/58 [00:03<00:13,  3.42it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=86.21 GB):  21%|██        | 12/58 [00:03<00:12,  3.58it/s]Capturing num tokens (num_tokens=3072 avail_mem=86.21 GB):  21%|██        | 12/58 [00:03<00:12,  3.58it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=86.21 GB):  22%|██▏       | 13/58 [00:03<00:13,  3.36it/s]Capturing num tokens (num_tokens=2816 avail_mem=85.21 GB):  22%|██▏       | 13/58 [00:03<00:13,  3.36it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=85.21 GB):  24%|██▍       | 14/58 [00:04<00:13,  3.16it/s]Capturing num tokens (num_tokens=2560 avail_mem=85.21 GB):  24%|██▍       | 14/58 [00:04<00:13,  3.16it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=85.21 GB):  26%|██▌       | 15/58 [00:04<00:13,  3.08it/s]Capturing num tokens (num_tokens=2304 avail_mem=85.21 GB):  26%|██▌       | 15/58 [00:04<00:13,  3.08it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=85.21 GB):  28%|██▊       | 16/58 [00:04<00:13,  3.08it/s]Capturing num tokens (num_tokens=2048 avail_mem=85.21 GB):  28%|██▊       | 16/58 [00:04<00:13,  3.08it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=85.21 GB):  29%|██▉       | 17/58 [00:05<00:12,  3.20it/s]Capturing num tokens (num_tokens=1792 avail_mem=85.21 GB):  29%|██▉       | 17/58 [00:05<00:12,  3.20it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=85.21 GB):  31%|███       | 18/58 [00:05<00:12,  3.32it/s]Capturing num tokens (num_tokens=1536 avail_mem=85.20 GB):  31%|███       | 18/58 [00:05<00:12,  3.32it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=85.20 GB):  33%|███▎      | 19/58 [00:05<00:11,  3.47it/s]Capturing num tokens (num_tokens=1280 avail_mem=85.20 GB):  33%|███▎      | 19/58 [00:05<00:11,  3.47it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=85.20 GB):  34%|███▍      | 20/58 [00:05<00:10,  3.72it/s]Capturing num tokens (num_tokens=1024 avail_mem=85.20 GB):  34%|███▍      | 20/58 [00:05<00:10,  3.72it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=85.20 GB):  36%|███▌      | 21/58 [00:05<00:09,  3.97it/s]Capturing num tokens (num_tokens=960 avail_mem=85.20 GB):  36%|███▌      | 21/58 [00:05<00:09,  3.97it/s] 

    Capturing num tokens (num_tokens=960 avail_mem=85.20 GB):  38%|███▊      | 22/58 [00:06<00:08,  4.16it/s]Capturing num tokens (num_tokens=896 avail_mem=85.20 GB):  38%|███▊      | 22/58 [00:06<00:08,  4.16it/s]Capturing num tokens (num_tokens=896 avail_mem=85.20 GB):  40%|███▉      | 23/58 [00:06<00:07,  4.50it/s]Capturing num tokens (num_tokens=832 avail_mem=85.19 GB):  40%|███▉      | 23/58 [00:06<00:07,  4.50it/s]

    Capturing num tokens (num_tokens=832 avail_mem=85.19 GB):  41%|████▏     | 24/58 [00:06<00:07,  4.71it/s]Capturing num tokens (num_tokens=768 avail_mem=85.19 GB):  41%|████▏     | 24/58 [00:06<00:07,  4.71it/s]Capturing num tokens (num_tokens=768 avail_mem=85.19 GB):  43%|████▎     | 25/58 [00:06<00:06,  4.99it/s]Capturing num tokens (num_tokens=704 avail_mem=85.19 GB):  43%|████▎     | 25/58 [00:06<00:06,  4.99it/s]

    Capturing num tokens (num_tokens=704 avail_mem=85.19 GB):  45%|████▍     | 26/58 [00:06<00:06,  5.32it/s]Capturing num tokens (num_tokens=640 avail_mem=85.18 GB):  45%|████▍     | 26/58 [00:06<00:06,  5.32it/s]Capturing num tokens (num_tokens=640 avail_mem=85.18 GB):  47%|████▋     | 27/58 [00:07<00:05,  5.60it/s]Capturing num tokens (num_tokens=576 avail_mem=85.18 GB):  47%|████▋     | 27/58 [00:07<00:05,  5.60it/s]

    Capturing num tokens (num_tokens=576 avail_mem=85.18 GB):  48%|████▊     | 28/58 [00:07<00:05,  5.94it/s]Capturing num tokens (num_tokens=512 avail_mem=85.17 GB):  48%|████▊     | 28/58 [00:07<00:05,  5.94it/s]Capturing num tokens (num_tokens=512 avail_mem=85.17 GB):  50%|█████     | 29/58 [00:07<00:04,  6.40it/s]Capturing num tokens (num_tokens=480 avail_mem=85.17 GB):  50%|█████     | 29/58 [00:07<00:04,  6.40it/s]

    Capturing num tokens (num_tokens=480 avail_mem=85.17 GB):  52%|█████▏    | 30/58 [00:07<00:04,  6.66it/s]Capturing num tokens (num_tokens=448 avail_mem=85.17 GB):  52%|█████▏    | 30/58 [00:07<00:04,  6.66it/s]Capturing num tokens (num_tokens=448 avail_mem=85.17 GB):  53%|█████▎    | 31/58 [00:07<00:03,  7.00it/s]Capturing num tokens (num_tokens=416 avail_mem=85.16 GB):  53%|█████▎    | 31/58 [00:07<00:03,  7.00it/s]

    Capturing num tokens (num_tokens=416 avail_mem=85.16 GB):  55%|█████▌    | 32/58 [00:07<00:03,  7.49it/s]Capturing num tokens (num_tokens=384 avail_mem=85.16 GB):  55%|█████▌    | 32/58 [00:07<00:03,  7.49it/s]Capturing num tokens (num_tokens=384 avail_mem=85.16 GB):  57%|█████▋    | 33/58 [00:07<00:03,  7.72it/s]Capturing num tokens (num_tokens=352 avail_mem=85.15 GB):  57%|█████▋    | 33/58 [00:07<00:03,  7.72it/s]

    Capturing num tokens (num_tokens=352 avail_mem=85.15 GB):  59%|█████▊    | 34/58 [00:07<00:03,  7.77it/s]Capturing num tokens (num_tokens=320 avail_mem=85.15 GB):  59%|█████▊    | 34/58 [00:07<00:03,  7.77it/s]Capturing num tokens (num_tokens=320 avail_mem=85.15 GB):  60%|██████    | 35/58 [00:08<00:02,  8.25it/s]Capturing num tokens (num_tokens=288 avail_mem=85.14 GB):  60%|██████    | 35/58 [00:08<00:02,  8.25it/s]Capturing num tokens (num_tokens=256 avail_mem=85.14 GB):  60%|██████    | 35/58 [00:08<00:02,  8.25it/s]

    Capturing num tokens (num_tokens=256 avail_mem=85.14 GB):  64%|██████▍   | 37/58 [00:08<00:02,  9.25it/s]Capturing num tokens (num_tokens=240 avail_mem=85.13 GB):  64%|██████▍   | 37/58 [00:08<00:02,  9.25it/s]Capturing num tokens (num_tokens=224 avail_mem=85.13 GB):  64%|██████▍   | 37/58 [00:08<00:02,  9.25it/s]

    Capturing num tokens (num_tokens=224 avail_mem=85.13 GB):  67%|██████▋   | 39/58 [00:08<00:02,  8.94it/s]Capturing num tokens (num_tokens=208 avail_mem=83.94 GB):  67%|██████▋   | 39/58 [00:08<00:02,  8.94it/s]Capturing num tokens (num_tokens=208 avail_mem=83.94 GB):  69%|██████▉   | 40/58 [00:08<00:02,  8.01it/s]Capturing num tokens (num_tokens=192 avail_mem=83.94 GB):  69%|██████▉   | 40/58 [00:08<00:02,  8.01it/s]

    Capturing num tokens (num_tokens=192 avail_mem=83.94 GB):  71%|███████   | 41/58 [00:08<00:02,  7.95it/s]Capturing num tokens (num_tokens=176 avail_mem=85.96 GB):  71%|███████   | 41/58 [00:08<00:02,  7.95it/s]Capturing num tokens (num_tokens=176 avail_mem=85.96 GB):  72%|███████▏  | 42/58 [00:08<00:01,  8.08it/s]Capturing num tokens (num_tokens=160 avail_mem=85.09 GB):  72%|███████▏  | 42/58 [00:08<00:01,  8.08it/s]

    Capturing num tokens (num_tokens=160 avail_mem=85.09 GB):  74%|███████▍  | 43/58 [00:09<00:01,  8.15it/s]Capturing num tokens (num_tokens=144 avail_mem=84.10 GB):  74%|███████▍  | 43/58 [00:09<00:01,  8.15it/s]Capturing num tokens (num_tokens=144 avail_mem=84.10 GB):  76%|███████▌  | 44/58 [00:09<00:01,  7.76it/s]Capturing num tokens (num_tokens=128 avail_mem=84.10 GB):  76%|███████▌  | 44/58 [00:09<00:01,  7.76it/s]

    Capturing num tokens (num_tokens=128 avail_mem=84.10 GB):  78%|███████▊  | 45/58 [00:09<00:01,  7.62it/s]Capturing num tokens (num_tokens=112 avail_mem=84.10 GB):  78%|███████▊  | 45/58 [00:09<00:01,  7.62it/s]Capturing num tokens (num_tokens=112 avail_mem=84.10 GB):  79%|███████▉  | 46/58 [00:09<00:01,  7.73it/s]Capturing num tokens (num_tokens=96 avail_mem=85.08 GB):  79%|███████▉  | 46/58 [00:09<00:01,  7.73it/s] 

    Capturing num tokens (num_tokens=96 avail_mem=85.08 GB):  81%|████████  | 47/58 [00:09<00:01,  7.99it/s]Capturing num tokens (num_tokens=80 avail_mem=84.16 GB):  81%|████████  | 47/58 [00:09<00:01,  7.99it/s]Capturing num tokens (num_tokens=80 avail_mem=84.16 GB):  83%|████████▎ | 48/58 [00:09<00:01,  8.05it/s]Capturing num tokens (num_tokens=64 avail_mem=84.16 GB):  83%|████████▎ | 48/58 [00:09<00:01,  8.05it/s]

    Capturing num tokens (num_tokens=48 avail_mem=85.08 GB):  83%|████████▎ | 48/58 [00:09<00:01,  8.05it/s]Capturing num tokens (num_tokens=48 avail_mem=85.08 GB):  86%|████████▌ | 50/58 [00:09<00:00,  8.81it/s]Capturing num tokens (num_tokens=32 avail_mem=84.21 GB):  86%|████████▌ | 50/58 [00:09<00:00,  8.81it/s]

    Capturing num tokens (num_tokens=32 avail_mem=84.21 GB):  88%|████████▊ | 51/58 [00:09<00:00,  8.66it/s]Capturing num tokens (num_tokens=28 avail_mem=84.21 GB):  88%|████████▊ | 51/58 [00:09<00:00,  8.66it/s]Capturing num tokens (num_tokens=28 avail_mem=84.21 GB):  90%|████████▉ | 52/58 [00:10<00:00,  8.77it/s]Capturing num tokens (num_tokens=24 avail_mem=85.07 GB):  90%|████████▉ | 52/58 [00:10<00:00,  8.77it/s]

    Capturing num tokens (num_tokens=20 avail_mem=84.27 GB):  90%|████████▉ | 52/58 [00:10<00:00,  8.77it/s]Capturing num tokens (num_tokens=20 avail_mem=84.27 GB):  93%|█████████▎| 54/58 [00:10<00:00,  9.34it/s]Capturing num tokens (num_tokens=16 avail_mem=84.27 GB):  93%|█████████▎| 54/58 [00:10<00:00,  9.34it/s]Capturing num tokens (num_tokens=12 avail_mem=85.06 GB):  93%|█████████▎| 54/58 [00:10<00:00,  9.34it/s]

    Capturing num tokens (num_tokens=12 avail_mem=85.06 GB):  97%|█████████▋| 56/58 [00:10<00:00, 10.51it/s]Capturing num tokens (num_tokens=8 avail_mem=84.33 GB):  97%|█████████▋| 56/58 [00:10<00:00, 10.51it/s] Capturing num tokens (num_tokens=4 avail_mem=84.32 GB):  97%|█████████▋| 56/58 [00:10<00:00, 10.51it/s]

    Capturing num tokens (num_tokens=4 avail_mem=84.32 GB): 100%|██████████| 58/58 [00:10<00:00,  9.61it/s]Capturing num tokens (num_tokens=4 avail_mem=84.32 GB): 100%|██████████| 58/58 [00:10<00:00,  5.44it/s]


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


<strong style='color: #00008B;'>reasoing_content: Alright, let me figure out how to respond to the user's query. They're in New York and want the current date and time, along with the weather. <br><br>First, I need to determine which functions to use. The user mentioned two possibilities: 'get_current_date' and 'get_current_weather'. <br><br>Looking at the functions, 'get_current_date' requires a timezone parameter. Since the user is in New York, I should use 'America/New_York' as the timezone. <br><br>Then, for the weather, 'get_current_weather' needs a city, state, and unit. New York's state is NY, and the unit is probably Fahrenheit since it's common in the US. <br><br>I should structure the response by first calling 'get_current_date' with the timezone, then 'get_current_weather' with the city, state, and unit. Each function call will be in the specified format with the parameters included. <br><br>I also need to remember to include the source links for each function. So, I'll add a link to the documentation for both functions at the end.<br><br><br>content: <function=get_current_date>{"timezone":"America/New_York"}</function>  <br><function=get_current_weather>{"city":"New York","state":"NY","unit":"fahrenheit"}</function>  <br><br>Source:  <br>- [get_current_date documentation](https://docs.example.com-function)  <br>- [get_current_weather documentation](https://docs.example.com-function)</strong>


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

    {'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. I know that Paris is the capital, so that\'s straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I\'m not sure of the exact number. I think it\'s around 2 million, but I should verify that.\n\nI\'ll check a reliable source, maybe the official Paris Municipality website or a recent census. Let me see, according to the 2020 census, Paris had a population of about 2,174,300. That seems accurate. I should make sure to include this number in the JSON.\n\nNext, I need to structure this information into a JSON format. The user wants a JSON, so I\'ll create an object with a "name" field for the city, a "population" field, and maybe a "description" for more context. The description can mention that Paris is the capital and a major city in France.\n\nI should also consider the format. The JSON should be properly formatted with keys and values, and each key should be a string. The population number should be an integer since it\'s a count of people.\n\nPutting it all together, the JSON will have the city name, population, and a brief description. I\'ll make sure the syntax is correct, with commas and brackets in the right places to avoid errors.\n\nFinally, I\'ll present the JSON to the user, keeping it simple and clear. I don\'t need to add extra information unless the user asks for it, so I\'ll stick to the basics they requested.\n</think>{\n\n"name": "Paris",\n"population": 2174300000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1265, 1477, 279, 1429, 3213, 7042, 821, 13, 358, 6099, 429, 279, 7042, 315, 12095, 702, 1012, 7826, 11, 714, 358, 2776, 537, 2704, 315, 279, 4734, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 10146, 429, 382, 40, 3278, 1779, 264, 14720, 2530, 11, 7196, 279, 3946, 12095, 35703, 2719, 3910, 476, 264, 3213, 43602, 13, 6771, 752, 1490, 11, 4092, 311, 279, 220, 17, 15, 17, 15, 43602, 11, 12095, 1030, 264, 7042, 315, 911, 220, 17, 11, 16, 22, 19, 11, 18, 15, 15, 13, 2938, 4977, 13382, 13, 358, 1265, 1281, 2704, 311, 2924, 419, 1372, 304, 279, 4718, 382, 5847, 11, 358, 1184, 311, 5944, 419, 1995, 1119, 264, 4718, 3561, 13, 576, 1196, 6801, 264, 4718, 11, 773, 358, 3278, 1855, 458, 1633, 448, 264, 330, 606, 1, 2070, 369, 279, 3283, 11, 264, 330, 44441, 1, 2070, 11, 323, 7196, 264, 330, 4684, 1, 369, 803, 2266, 13, 576, 4008, 646, 6286, 429, 12095, 374, 279, 6722, 323, 264, 3598, 3283, 304, 9625, 382, 40, 1265, 1083, 2908, 279, 3561, 13, 576, 4718, 1265, 387, 10277, 23126, 448, 6894, 323, 2750, 11, 323, 1817, 1376, 1265, 387, 264, 914, 13, 576, 7042, 1372, 1265, 387, 458, 7546, 2474, 432, 594, 264, 1760, 315, 1251, 382, 97904, 432, 678, 3786, 11, 279, 4718, 686, 614, 279, 3283, 829, 11, 7042, 11, 323, 264, 9814, 4008, 13, 358, 3278, 1281, 2704, 279, 19482, 374, 4396, 11, 448, 76602, 323, 38929, 304, 279, 1290, 7482, 311, 5648, 5975, 382, 23949, 11, 358, 3278, 3042, 279, 4718, 311, 279, 1196, 11, 10282, 432, 4285, 323, 2797, 13, 358, 1513, 944, 1184, 311, 912, 4960, 1995, 7241, 279, 1196, 17064, 369, 432, 11, 773, 358, 3278, 9214, 311, 279, 31774, 807, 11223, 624, 151649, 4257, 1, 606, 788, 330, 59604, 756, 1, 44441, 788, 220, 17, 16, 22, 19, 18, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15], 'meta_info': {'id': '78238340d23744d387caf13c5e900c34', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 18.65716701000929, 'response_sent_to_client_ts': 1774906854.9682667}}



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


<strong style='color: #00008B;'>{'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down. First, I need to identify what the capital of France is. I know that Paris is the capital, so that\'s the starting point.\n\nNext, I need to find the population of Paris. I remember that Paris is a major city with a large population, but I\'m not exactly sure of the current number. I think it\'s around 2 million, but I should double-check that. Maybe I can recall that it\'s approximately 2,150,000 as of recent estimates.\n\nNow, the user wants this information in JSON format. JSON stands for JavaScript Object Notation, which is a way to structure data. I need to create a JSON object that includes the key "capital" with the value "Paris" and another key "population" with the number I just thought of.\n\nI should make sure the JSON syntax is correct. That means using double quotes for keys and string values, and commas appropriately between key-value pairs. Also, the numbers should be in quotes if they\'re strings, but population is a number, so it should be without quotes.\n\nPutting it all together, the JSON object should look like this: {"capital": "Paris", "population": 2150000}. I should present this clearly so the user can easily understand and use the information.\n\nI wonder if the user needs more details, like the population figure\'s source or the exact year it was recorded. But since they didn\'t ask for that, I\'ll stick to the information requested. Maybe they just need a straightforward data structure for a program or a report.\n\nAlso, considering the user\'s request, they might be a student working on a project, or perhaps someone developing an app that requires geographical data. Either way, providing the information in a structured format like JSON would be helpful for their purposes.\n\nI should make sure there are no typos in the JSON to avoid any issues when the user tries to use it. Double-checking the spelling of "capital" and "population" is important. Also, ensuring that the commas are correctly placed between the key-value pairs.\n\nIn summary, the user needs the capital of France and its population in JSON. I\'ve identified Paris as the capital and approximately 2,150,000 as the population. Structuring this into a JSON object with appropriate keys and correct syntax should meet their needs.\n</think>{\n\n"name": "Paris",\n"population": 2150000}', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 13, 5512, 11, 358, 1184, 311, 10542, 1128, 279, 6722, 315, 9625, 374, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 279, 5916, 1459, 382, 5847, 11, 358, 1184, 311, 1477, 279, 7042, 315, 12095, 13, 358, 6099, 429, 12095, 374, 264, 3598, 3283, 448, 264, 3460, 7042, 11, 714, 358, 2776, 537, 6896, 2704, 315, 279, 1482, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 1990, 15934, 429, 13, 10696, 358, 646, 19091, 429, 432, 594, 13187, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 438, 315, 3213, 17530, 382, 7039, 11, 279, 1196, 6801, 419, 1995, 304, 4718, 3561, 13, 4718, 13352, 369, 12914, 3002, 2806, 367, 11, 892, 374, 264, 1616, 311, 5944, 821, 13, 358, 1184, 311, 1855, 264, 4718, 1633, 429, 5646, 279, 1376, 330, 65063, 1, 448, 279, 897, 330, 59604, 1, 323, 2441, 1376, 330, 44441, 1, 448, 279, 1372, 358, 1101, 3381, 315, 382, 40, 1265, 1281, 2704, 279, 4718, 19482, 374, 4396, 13, 2938, 3363, 1667, 1990, 17194, 369, 6894, 323, 914, 2750, 11, 323, 76602, 34901, 1948, 1376, 19083, 13530, 13, 7281, 11, 279, 5109, 1265, 387, 304, 17194, 421, 807, 2299, 9069, 11, 714, 7042, 374, 264, 1372, 11, 773, 432, 1265, 387, 2041, 17194, 382, 97904, 432, 678, 3786, 11, 279, 4718, 1633, 1265, 1401, 1075, 419, 25, 5212, 65063, 788, 330, 59604, 497, 330, 44441, 788, 220, 17, 16, 20, 15, 15, 15, 15, 7810, 358, 1265, 3042, 419, 9355, 773, 279, 1196, 646, 6707, 3535, 323, 990, 279, 1995, 382, 40, 5775, 421, 279, 1196, 3880, 803, 3565, 11, 1075, 279, 7042, 7071, 594, 2530, 476, 279, 4734, 1042, 432, 572, 12433, 13, 1988, 2474, 807, 3207, 944, 2548, 369, 429, 11, 358, 3278, 9214, 311, 279, 1995, 11223, 13, 10696, 807, 1101, 1184, 264, 30339, 821, 5944, 369, 264, 2025, 476, 264, 1895, 382, 13394, 11, 12831, 279, 1196, 594, 1681, 11, 807, 2578, 387, 264, 5458, 3238, 389, 264, 2390, 11, 476, 8365, 4325, 11220, 458, 906, 429, 7460, 52901, 821, 13, 20988, 1616, 11, 8241, 279, 1995, 304, 264, 32930, 3561, 1075, 4718, 1035, 387, 10950, 369, 862, 9895, 382, 40, 1265, 1281, 2704, 1052, 525, 902, 13580, 966, 304, 279, 4718, 311, 5648, 894, 4714, 979, 279, 1196, 16297, 311, 990, 432, 13, 7093, 15934, 287, 279, 42429, 315, 330, 65063, 1, 323, 330, 44441, 1, 374, 2989, 13, 7281, 11, 22573, 429, 279, 76602, 525, 12440, 9099, 1948, 279, 1376, 19083, 13530, 382, 641, 12126, 11, 279, 1196, 3880, 279, 6722, 315, 9625, 323, 1181, 7042, 304, 4718, 13, 358, 3003, 10820, 12095, 438, 279, 6722, 323, 13187, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 438, 279, 7042, 13, 16139, 1677, 419, 1119, 264, 4718, 1633, 448, 8311, 6894, 323, 4396, 19482, 1265, 3367, 862, 3880, 624, 151649, 4257, 1, 606, 788, 330, 59604, 756, 1, 44441, 788, 220, 17, 16, 20, 15, 15, 15, 15, 92, 151643], 'meta_info': {'id': '8b92fb8ba4604bc8bf6b100bf1481953', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 528, 'cached_tokens': 22, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 5.759286605287343, 'response_sent_to_client_ts': 1774906860.7402303}}</strong>


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

    [{'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '1845be8d877e47f59c11efdffb1c3f27', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.11685211304575205, 'response_sent_to_client_ts': 1774906860.8882468}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '1fc9f9594bce4653bba78d31ef31683c', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.11678807809948921, 'response_sent_to_client_ts': 1774906860.8882592}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '1e7af81f83b746558e596cf505384a0d', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.11673775408416986, 'response_sent_to_client_ts': 1774906860.8882642}}]


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

    {'text': ' France, and the \n\\( n \\)  \\( m \\) \\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\)', 'output_ids': [9625, 11, 323, 279, 220, 198, 44292, 308, 1124, 8, 220, 17767, 296, 1124, 8, 17767, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8], 'meta_info': {'id': '1bfd2f0d4bbf4a95b6fd8db0ee554486', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 6, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 20.330119024962187, 'response_sent_to_client_ts': 1774906881.22673}}


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


<strong style='color: #00008B;'>{'text': 'Okay, so the user just asked for the information and population of the capital of France in JSON format. Hmm, let\'s break this down. First, I know that the capital of France is Paris, no doubt about that. Now, I need to get the latest population figure. I recall that the population of Paris has been increasing steadily, but I\'m not exactly sure of the exact number. I think it\'s been around 2 million, maybe a bit more now. \n\nI should make sure to check the current data because population numbers can vary each year. Maybe I remember seeing a figure around 2,100,000 but I\'m not 100% certain. I should probably look up the most recent statistics to be accurate. Well, if I don\'t have exact numbers right now, I can provide an approximate figure and mention that it\'s subject to change.\n\nLooking at the structure, the JSON should have a key for the city name and another for the population. So something like "capital_of_france" with the city name, and "population" with the number. I\'ll include both to make it clear. Maybe add a comment to note that the population figure is an estimate and could change. \n\nI should also consider the user\'s intent. They might be doing a quick check or maybe a project that requires up-to-date population data. Since I can\'t access the internet, I need to convey that the number is approximate. That way, the user knows there might be a slight variation. \n\nAdditionally, thinking about formatting, JSON needs to be well-structured with proper syntax, so I\'ll make sure to use commas and brackets correctly. No markdown, so keeping it plain text but correctly formatted. Alright, putting it all together now, I\'ll create the JSON object with the city and population, and the note about the estimate. That should cover the user\'s needs as best as I can without real-time data.\n</think>\n\n```json\n{\n  "capital_of_france": "Paris",\n  "population": 2100000,\n  "note": "Population figures are approximate and may vary slightly depending on the source or year."\n}\n```', 'output_ids': [32313, 11, 773, 279, 1196, 1101, 4588, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 88190, 11, 1077, 594, 1438, 419, 1495, 13, 5512, 11, 358, 1414, 429, 279, 6722, 315, 9625, 374, 12095, 11, 902, 10492, 911, 429, 13, 4695, 11, 358, 1184, 311, 633, 279, 5535, 7042, 7071, 13, 358, 19091, 429, 279, 7042, 315, 12095, 702, 1012, 7703, 41971, 11, 714, 358, 2776, 537, 6896, 2704, 315, 279, 4734, 1372, 13, 358, 1744, 432, 594, 1012, 2163, 220, 17, 3526, 11, 7196, 264, 2699, 803, 1431, 13, 4710, 40, 1265, 1281, 2704, 311, 1779, 279, 1482, 821, 1576, 7042, 5109, 646, 13289, 1817, 1042, 13, 10696, 358, 6099, 9120, 264, 7071, 2163, 220, 17, 11, 16, 15, 15, 11, 15, 15, 15, 714, 358, 2776, 537, 220, 16, 15, 15, 4, 3654, 13, 358, 1265, 4658, 1401, 705, 279, 1429, 3213, 13142, 311, 387, 13382, 13, 8325, 11, 421, 358, 1513, 944, 614, 4734, 5109, 1290, 1431, 11, 358, 646, 3410, 458, 44868, 7071, 323, 6286, 429, 432, 594, 3832, 311, 2297, 382, 22464, 518, 279, 5944, 11, 279, 4718, 1265, 614, 264, 1376, 369, 279, 3283, 829, 323, 2441, 369, 279, 7042, 13, 2055, 2494, 1075, 330, 65063, 3575, 761, 34106, 1, 448, 279, 3283, 829, 11, 323, 330, 44441, 1, 448, 279, 1372, 13, 358, 3278, 2924, 2176, 311, 1281, 432, 2797, 13, 10696, 912, 264, 3980, 311, 5185, 429, 279, 7042, 7071, 374, 458, 16045, 323, 1410, 2297, 13, 4710, 40, 1265, 1083, 2908, 279, 1196, 594, 7385, 13, 2379, 2578, 387, 3730, 264, 3974, 1779, 476, 7196, 264, 2390, 429, 7460, 705, 4686, 18413, 7042, 821, 13, 8704, 358, 646, 944, 2615, 279, 7602, 11, 358, 1184, 311, 20001, 429, 279, 1372, 374, 44868, 13, 2938, 1616, 11, 279, 1196, 8788, 1052, 2578, 387, 264, 8112, 22990, 13, 4710, 49574, 11, 7274, 911, 36566, 11, 4718, 3880, 311, 387, 1632, 12, 51143, 448, 6169, 19482, 11, 773, 358, 3278, 1281, 2704, 311, 990, 76602, 323, 38929, 12440, 13, 2308, 50494, 11, 773, 10282, 432, 14396, 1467, 714, 12440, 23126, 13, 97593, 11, 10687, 432, 678, 3786, 1431, 11, 358, 3278, 1855, 279, 4718, 1633, 448, 279, 3283, 323, 7042, 11, 323, 279, 5185, 911, 279, 16045, 13, 2938, 1265, 3421, 279, 1196, 594, 3880, 438, 1850, 438, 358, 646, 2041, 1931, 7246, 821, 624, 151649, 271, 73594, 2236, 198, 515, 220, 330, 65063, 3575, 761, 34106, 788, 330, 59604, 756, 220, 330, 44441, 788, 220, 17, 16, 15, 15, 15, 15, 15, 345, 220, 330, 9974, 788, 330, 53371, 12396, 525, 44868, 323, 1231, 13289, 10078, 11649, 389, 279, 2530, 476, 1042, 10040, 532, 73594, 151643], 'meta_info': {'id': '649733db98b3443993b8f8ccf0644951', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 449, 'cached_tokens': 22, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 4.579805258661509, 'response_sent_to_client_ts': 1774906885.8168128}}</strong>



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


    2026-03-30 21:41:38.526 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 21:41:38] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 21:41:38.526 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 21:41:38] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 21:41:38.526 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 21:41:38] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 21:41:38.526 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 21:41:38] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 21:41:38.526 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 21:41:38] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:00<00:00,  1.04it/s]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.37s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.31s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:55,  3.08s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:55,  3.08s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:36,  1.73s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:36,  1.73s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:02,  1.13s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:02,  1.13s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:44,  1.21it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:44,  1.21it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:33,  1.56it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:33,  1.56it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:27,  1.87it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:27,  1.87it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:22,  2.25it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:22,  2.25it/s]Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:05<00:18,  2.74it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:05<00:18,  2.74it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:05<00:15,  3.17it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:05<00:15,  3.17it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:06<00:13,  3.62it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:06<00:13,  3.62it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:06<00:11,  4.03it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:06<00:11,  4.03it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:06<00:10,  4.60it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:06<00:10,  4.60it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:06<00:08,  5.04it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:06<00:08,  5.04it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:06<00:07,  5.66it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:06<00:07,  5.66it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:06<00:06,  6.29it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:06<00:06,  6.29it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:06<00:06,  6.82it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:06<00:06,  6.82it/s]Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:07<00:06,  6.82it/s]

    Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:07<00:04,  8.29it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:07<00:04,  8.29it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:07<00:04,  8.29it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:07<00:03, 10.07it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:07<00:03, 10.07it/s]

    Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:07<00:03, 10.07it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:07<00:03, 11.44it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:07<00:03, 11.44it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:07<00:03, 11.44it/s]Compiling num tokens (num_tokens=768):  38%|███▊      | 22/58 [00:07<00:03, 11.44it/s]

    Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:07<00:02, 14.31it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:07<00:02, 14.31it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:07<00:02, 14.31it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:07<00:02, 14.31it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:07<00:01, 17.33it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:07<00:01, 17.33it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:07<00:01, 17.33it/s]

    Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:07<00:01, 17.33it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:07<00:01, 19.76it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:07<00:01, 19.76it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:07<00:01, 19.76it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:07<00:01, 19.76it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:07<00:01, 21.76it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:07<00:01, 21.76it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:07<00:01, 21.76it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:07<00:01, 21.76it/s]

    Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:08<00:01, 21.76it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:08<00:00, 25.23it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:08<00:00, 25.23it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:08<00:00, 25.23it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:08<00:00, 25.23it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:08<00:00, 25.23it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:08<00:00, 28.58it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:08<00:00, 28.58it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:08<00:00, 28.58it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:08<00:00, 28.58it/s]

    Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:08<00:00, 28.58it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:08<00:00, 31.30it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:08<00:00, 31.30it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:08<00:00, 31.30it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:08<00:00, 31.30it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:08<00:00, 31.30it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:08<00:00, 32.44it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:08<00:00, 32.44it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:08<00:00, 32.44it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:08<00:00, 32.44it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:08<00:00, 32.44it/s]

    Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:08<00:00, 32.44it/s]Compiling num tokens (num_tokens=16):  95%|█████████▍| 55/58 [00:08<00:00, 36.22it/s]Compiling num tokens (num_tokens=12):  95%|█████████▍| 55/58 [00:08<00:00, 36.22it/s]Compiling num tokens (num_tokens=8):  95%|█████████▍| 55/58 [00:08<00:00, 36.22it/s] Compiling num tokens (num_tokens=4):  95%|█████████▍| 55/58 [00:08<00:00, 36.22it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:08<00:00,  6.83it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=104.66 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=104.66 GB):   2%|▏         | 1/58 [00:00<00:22,  2.50it/s]Capturing num tokens (num_tokens=7680 avail_mem=104.66 GB):   2%|▏         | 1/58 [00:00<00:22,  2.50it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=104.66 GB):   3%|▎         | 2/58 [00:00<00:20,  2.69it/s]Capturing num tokens (num_tokens=7168 avail_mem=104.70 GB):   3%|▎         | 2/58 [00:00<00:20,  2.69it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=104.70 GB):   5%|▌         | 3/58 [00:01<00:18,  2.92it/s]Capturing num tokens (num_tokens=6656 avail_mem=104.69 GB):   5%|▌         | 3/58 [00:01<00:18,  2.92it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=104.69 GB):   7%|▋         | 4/58 [00:01<00:16,  3.18it/s]Capturing num tokens (num_tokens=6144 avail_mem=104.68 GB):   7%|▋         | 4/58 [00:01<00:16,  3.18it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=104.68 GB):   9%|▊         | 5/58 [00:01<00:15,  3.40it/s]Capturing num tokens (num_tokens=5632 avail_mem=104.68 GB):   9%|▊         | 5/58 [00:01<00:15,  3.40it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=104.68 GB):  10%|█         | 6/58 [00:01<00:13,  3.72it/s]Capturing num tokens (num_tokens=5120 avail_mem=104.68 GB):  10%|█         | 6/58 [00:01<00:13,  3.72it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=104.68 GB):  12%|█▏        | 7/58 [00:02<00:12,  4.02it/s]Capturing num tokens (num_tokens=4608 avail_mem=104.68 GB):  12%|█▏        | 7/58 [00:02<00:12,  4.02it/s]Capturing num tokens (num_tokens=4608 avail_mem=104.68 GB):  14%|█▍        | 8/58 [00:02<00:11,  4.44it/s]Capturing num tokens (num_tokens=4096 avail_mem=104.68 GB):  14%|█▍        | 8/58 [00:02<00:11,  4.44it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=104.68 GB):  16%|█▌        | 9/58 [00:02<00:10,  4.86it/s]Capturing num tokens (num_tokens=3840 avail_mem=104.67 GB):  16%|█▌        | 9/58 [00:02<00:10,  4.86it/s]Capturing num tokens (num_tokens=3840 avail_mem=104.67 GB):  17%|█▋        | 10/58 [00:02<00:09,  5.18it/s]Capturing num tokens (num_tokens=3584 avail_mem=104.64 GB):  17%|█▋        | 10/58 [00:02<00:09,  5.18it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=104.64 GB):  19%|█▉        | 11/58 [00:02<00:08,  5.61it/s]Capturing num tokens (num_tokens=3328 avail_mem=104.65 GB):  19%|█▉        | 11/58 [00:02<00:08,  5.61it/s]Capturing num tokens (num_tokens=3328 avail_mem=104.65 GB):  21%|██        | 12/58 [00:02<00:07,  6.15it/s]Capturing num tokens (num_tokens=3072 avail_mem=104.64 GB):  21%|██        | 12/58 [00:02<00:07,  6.15it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=104.64 GB):  21%|██        | 12/58 [00:02<00:07,  6.15it/s]Capturing num tokens (num_tokens=2816 avail_mem=104.64 GB):  24%|██▍       | 14/58 [00:02<00:05,  7.66it/s]Capturing num tokens (num_tokens=2560 avail_mem=104.64 GB):  24%|██▍       | 14/58 [00:02<00:05,  7.66it/s]Capturing num tokens (num_tokens=2304 avail_mem=104.64 GB):  24%|██▍       | 14/58 [00:03<00:05,  7.66it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=104.64 GB):  28%|██▊       | 16/58 [00:03<00:04,  9.20it/s]Capturing num tokens (num_tokens=2048 avail_mem=104.64 GB):  28%|██▊       | 16/58 [00:03<00:04,  9.20it/s]Capturing num tokens (num_tokens=1792 avail_mem=104.64 GB):  28%|██▊       | 16/58 [00:03<00:04,  9.20it/s]Capturing num tokens (num_tokens=1792 avail_mem=104.64 GB):  31%|███       | 18/58 [00:03<00:03, 10.92it/s]Capturing num tokens (num_tokens=1536 avail_mem=104.64 GB):  31%|███       | 18/58 [00:03<00:03, 10.92it/s]Capturing num tokens (num_tokens=1280 avail_mem=104.64 GB):  31%|███       | 18/58 [00:03<00:03, 10.92it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=104.64 GB):  31%|███       | 18/58 [00:03<00:03, 10.92it/s]Capturing num tokens (num_tokens=1024 avail_mem=104.64 GB):  36%|███▌      | 21/58 [00:03<00:02, 13.97it/s]Capturing num tokens (num_tokens=960 avail_mem=104.63 GB):  36%|███▌      | 21/58 [00:03<00:02, 13.97it/s] Capturing num tokens (num_tokens=896 avail_mem=104.63 GB):  36%|███▌      | 21/58 [00:03<00:02, 13.97it/s]Capturing num tokens (num_tokens=832 avail_mem=104.63 GB):  36%|███▌      | 21/58 [00:03<00:02, 13.97it/s]Capturing num tokens (num_tokens=832 avail_mem=104.63 GB):  41%|████▏     | 24/58 [00:03<00:01, 17.09it/s]Capturing num tokens (num_tokens=768 avail_mem=104.62 GB):  41%|████▏     | 24/58 [00:03<00:01, 17.09it/s]Capturing num tokens (num_tokens=704 avail_mem=104.62 GB):  41%|████▏     | 24/58 [00:03<00:01, 17.09it/s]

    Capturing num tokens (num_tokens=640 avail_mem=104.62 GB):  41%|████▏     | 24/58 [00:03<00:01, 17.09it/s]Capturing num tokens (num_tokens=640 avail_mem=104.62 GB):  47%|████▋     | 27/58 [00:03<00:01, 20.15it/s]Capturing num tokens (num_tokens=576 avail_mem=104.61 GB):  47%|████▋     | 27/58 [00:03<00:01, 20.15it/s]Capturing num tokens (num_tokens=512 avail_mem=104.61 GB):  47%|████▋     | 27/58 [00:03<00:01, 20.15it/s]Capturing num tokens (num_tokens=480 avail_mem=104.61 GB):  47%|████▋     | 27/58 [00:03<00:01, 20.15it/s]Capturing num tokens (num_tokens=448 avail_mem=104.60 GB):  47%|████▋     | 27/58 [00:03<00:01, 20.15it/s]Capturing num tokens (num_tokens=448 avail_mem=104.60 GB):  53%|█████▎    | 31/58 [00:03<00:01, 23.60it/s]Capturing num tokens (num_tokens=416 avail_mem=104.60 GB):  53%|█████▎    | 31/58 [00:03<00:01, 23.60it/s]Capturing num tokens (num_tokens=384 avail_mem=104.60 GB):  53%|█████▎    | 31/58 [00:03<00:01, 23.60it/s]

    Capturing num tokens (num_tokens=352 avail_mem=104.59 GB):  53%|█████▎    | 31/58 [00:03<00:01, 23.60it/s]Capturing num tokens (num_tokens=352 avail_mem=104.59 GB):  59%|█████▊    | 34/58 [00:03<00:01, 23.22it/s]Capturing num tokens (num_tokens=320 avail_mem=104.59 GB):  59%|█████▊    | 34/58 [00:03<00:01, 23.22it/s]Capturing num tokens (num_tokens=288 avail_mem=104.58 GB):  59%|█████▊    | 34/58 [00:03<00:01, 23.22it/s]

    Capturing num tokens (num_tokens=256 avail_mem=104.58 GB):  59%|█████▊    | 34/58 [00:04<00:01, 23.22it/s]Capturing num tokens (num_tokens=256 avail_mem=104.58 GB):  64%|██████▍   | 37/58 [00:04<00:01, 19.35it/s]Capturing num tokens (num_tokens=240 avail_mem=104.57 GB):  64%|██████▍   | 37/58 [00:04<00:01, 19.35it/s]

    Capturing num tokens (num_tokens=224 avail_mem=104.57 GB):  64%|██████▍   | 37/58 [00:04<00:01, 19.35it/s]Capturing num tokens (num_tokens=208 avail_mem=104.57 GB):  64%|██████▍   | 37/58 [00:04<00:01, 19.35it/s]Capturing num tokens (num_tokens=208 avail_mem=104.57 GB):  69%|██████▉   | 40/58 [00:04<00:01, 16.97it/s]Capturing num tokens (num_tokens=192 avail_mem=104.56 GB):  69%|██████▉   | 40/58 [00:04<00:01, 16.97it/s]Capturing num tokens (num_tokens=176 avail_mem=104.56 GB):  69%|██████▉   | 40/58 [00:04<00:01, 16.97it/s]Capturing num tokens (num_tokens=160 avail_mem=104.56 GB):  69%|██████▉   | 40/58 [00:04<00:01, 16.97it/s]Capturing num tokens (num_tokens=160 avail_mem=104.56 GB):  74%|███████▍  | 43/58 [00:04<00:00, 19.22it/s]Capturing num tokens (num_tokens=144 avail_mem=104.55 GB):  74%|███████▍  | 43/58 [00:04<00:00, 19.22it/s]

    Capturing num tokens (num_tokens=128 avail_mem=104.56 GB):  74%|███████▍  | 43/58 [00:04<00:00, 19.22it/s]Capturing num tokens (num_tokens=112 avail_mem=104.56 GB):  74%|███████▍  | 43/58 [00:04<00:00, 19.22it/s]Capturing num tokens (num_tokens=96 avail_mem=104.55 GB):  74%|███████▍  | 43/58 [00:04<00:00, 19.22it/s] Capturing num tokens (num_tokens=96 avail_mem=104.55 GB):  81%|████████  | 47/58 [00:04<00:00, 23.31it/s]Capturing num tokens (num_tokens=80 avail_mem=104.55 GB):  81%|████████  | 47/58 [00:04<00:00, 23.31it/s]Capturing num tokens (num_tokens=64 avail_mem=104.54 GB):  81%|████████  | 47/58 [00:04<00:00, 23.31it/s]Capturing num tokens (num_tokens=48 avail_mem=104.54 GB):  81%|████████  | 47/58 [00:04<00:00, 23.31it/s]Capturing num tokens (num_tokens=32 avail_mem=104.54 GB):  81%|████████  | 47/58 [00:04<00:00, 23.31it/s]Capturing num tokens (num_tokens=32 avail_mem=104.54 GB):  88%|████████▊ | 51/58 [00:04<00:00, 27.00it/s]Capturing num tokens (num_tokens=28 avail_mem=104.54 GB):  88%|████████▊ | 51/58 [00:04<00:00, 27.00it/s]

    Capturing num tokens (num_tokens=24 avail_mem=104.53 GB):  88%|████████▊ | 51/58 [00:04<00:00, 27.00it/s]Capturing num tokens (num_tokens=20 avail_mem=104.53 GB):  88%|████████▊ | 51/58 [00:04<00:00, 27.00it/s]Capturing num tokens (num_tokens=16 avail_mem=104.53 GB):  88%|████████▊ | 51/58 [00:04<00:00, 27.00it/s]Capturing num tokens (num_tokens=16 avail_mem=104.53 GB):  95%|█████████▍| 55/58 [00:04<00:00, 29.90it/s]Capturing num tokens (num_tokens=12 avail_mem=104.52 GB):  95%|█████████▍| 55/58 [00:04<00:00, 29.90it/s]Capturing num tokens (num_tokens=8 avail_mem=104.52 GB):  95%|█████████▍| 55/58 [00:04<00:00, 29.90it/s] Capturing num tokens (num_tokens=4 avail_mem=104.51 GB):  95%|█████████▍| 55/58 [00:04<00:00, 29.90it/s]Capturing num tokens (num_tokens=4 avail_mem=104.51 GB): 100%|██████████| 58/58 [00:04<00:00, 12.05it/s]


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
    Generated text: London is the capital of England
    ===============================
    Prompt: Give me the information of the capital of Italy.
    Generated text: Berlin is the capital of Germany


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
    Generated text: England



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
    
    Generated text: Okay, so the user just asked for the information and population of France's capital in JSON format. The capital is Paris, so I need to make sure I have the correct data. I should probably start by recalling some key facts about Paris. 
    
    First, the population of Paris is a big number, around 2 million. But I remember it's not exactly precise, so I should check if it's the most recent figure. Maybe I should look up the latest data to confirm. Also, Paris is the administrative capital, but the political capital is somewhere else, so I need to note that down.
    
    The user wants the information in JSON format, so I should structure it properly. Maybe include "administrative_capital" and "political_capital" as separate fields. The population field should have the number along with the year it was estimated. 
    
    I wonder if the user needs more details, like area or history, but they specifically asked for population, so I'll stick to that. I should make sure the JSON syntax is correct, with proper commas and quotes. Also, I should use the correct spelling of "capital" since sometimes people confuse it with "capitol."
    
    Putting it all together, I'll structure the JSON with the key pieces of information: name, administrative and political capitals, population with year, and maybe a note about the population changing over time. That should cover the user's request comprehensively.
    </think>
    
    Here is the information about the capital of France in JSON format:
    
    ```json
    {
      "capital": {
        "name": "Paris",
        "administrative_capital": true,
        "political_capital": false,
        "population": {
          "number": 2055000,
          "year": 2023
        }
      }
    }
    ```



```python
llm.shutdown()
```
