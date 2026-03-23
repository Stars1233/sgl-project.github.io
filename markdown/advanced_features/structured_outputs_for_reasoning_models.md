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

    [2026-03-23 19:37:51] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.


    [2026-03-23 19:37:51] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.


    [2026-03-23 19:37:51] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [2026-03-23 19:37:56] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-23 19:37:56] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-23 19:37:56] INFO utils.py:164: NumExpr defaulting to 16 threads.
    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    [2026-03-23 19:37:58] INFO server_args.py:2240: Attention backend not specified. Use fa3 backend by default.
    [2026-03-23 19:37:58] INFO server_args.py:3523: Set soft_watchdog_timeout since in CI


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:175: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [2026-03-23 19:38:03] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-23 19:38:03] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-23 19:38:03] INFO utils.py:164: NumExpr defaulting to 16 threads.
    [2026-03-23 19:38:03] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-23 19:38:03] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-23 19:38:03] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-03-23 19:38:08.921 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-23 19:38:08] Persistent cache disabled, using in-memory JIT cache
    2026-03-23 19:38:08.921 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-23 19:38:08] Persistent cache disabled, using in-memory JIT cache
    2026-03-23 19:38:08.921 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-23 19:38:08] Persistent cache disabled, using in-memory JIT cache
    2026-03-23 19:38:08.921 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-23 19:38:08] Persistent cache disabled, using in-memory JIT cache
    2026-03-23 19:38:08.921 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-23 19:38:08] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:00<00:00,  1.25it/s]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.06s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.02s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:07,  3.30s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:07,  3.30s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:32,  1.65s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:32,  1.65s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<00:55,  1.01s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<00:55,  1.01s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:37,  1.44it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:37,  1.44it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:27,  1.95it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:27,  1.95it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:20,  2.55it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:20,  2.55it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:15,  3.26it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:15,  3.26it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:12,  4.05it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:12,  4.05it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:05<00:10,  4.53it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:05<00:10,  4.53it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:05<00:09,  4.98it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:05<00:09,  4.98it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:05<00:08,  5.46it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:05<00:08,  5.46it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:05<00:07,  6.09it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:05<00:07,  6.09it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:05<00:06,  6.76it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:05<00:06,  6.76it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:05<00:05,  7.41it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:05<00:05,  7.41it/s]Compiling num tokens (num_tokens=2304):  24%|██▍       | 14/58 [00:05<00:05,  7.41it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:05<00:04,  9.22it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:05<00:04,  9.22it/s]

    Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:05<00:04,  9.22it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:05<00:03, 10.79it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:05<00:03, 10.79it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:05<00:03, 10.79it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:06<00:02, 12.79it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:06<00:02, 12.79it/s]

    Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:06<00:02, 12.79it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:06<00:02, 12.79it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:06<00:02, 16.60it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:06<00:02, 16.60it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:06<00:02, 16.60it/s]Compiling num tokens (num_tokens=704):  40%|███▉      | 23/58 [00:06<00:02, 16.60it/s]Compiling num tokens (num_tokens=640):  40%|███▉      | 23/58 [00:06<00:02, 16.60it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:06<00:01, 21.40it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:06<00:01, 21.40it/s]

    Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:06<00:01, 21.40it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:06<00:01, 21.40it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:06<00:01, 21.40it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:06<00:01, 26.00it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:06<00:01, 26.00it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:06<00:01, 26.00it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:06<00:01, 26.00it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:06<00:01, 26.00it/s]Compiling num tokens (num_tokens=288):  53%|█████▎    | 31/58 [00:06<00:01, 26.00it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:06<00:00, 31.70it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:06<00:00, 31.70it/s]

    Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:06<00:00, 31.70it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:06<00:00, 31.70it/s]Compiling num tokens (num_tokens=208):  62%|██████▏   | 36/58 [00:06<00:00, 31.70it/s]Compiling num tokens (num_tokens=192):  62%|██████▏   | 36/58 [00:06<00:00, 31.70it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:06<00:00, 35.78it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:06<00:00, 35.78it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:06<00:00, 35.78it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:06<00:00, 35.78it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:06<00:00, 35.78it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:06<00:00, 35.78it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:06<00:00, 38.60it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:06<00:00, 38.60it/s] 

    Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:06<00:00, 38.60it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:06<00:00, 38.60it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:06<00:00, 38.60it/s]Compiling num tokens (num_tokens=32):  79%|███████▉  | 46/58 [00:06<00:00, 38.60it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:06<00:00, 41.49it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:06<00:00, 41.49it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:06<00:00, 41.49it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:06<00:00, 41.49it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:06<00:00, 41.49it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:06<00:00, 41.49it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:06<00:00, 41.49it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:06<00:00, 41.49it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:06<00:00, 49.26it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:06<00:00,  8.39it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=101.21 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=101.21 GB):   2%|▏         | 1/58 [00:00<00:30,  1.85it/s]Capturing num tokens (num_tokens=7680 avail_mem=100.17 GB):   2%|▏         | 1/58 [00:00<00:30,  1.85it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=100.17 GB):   3%|▎         | 2/58 [00:01<00:31,  1.78it/s]Capturing num tokens (num_tokens=7168 avail_mem=100.16 GB):   3%|▎         | 2/58 [00:01<00:31,  1.78it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=100.16 GB):   5%|▌         | 3/58 [00:01<00:29,  1.86it/s]Capturing num tokens (num_tokens=6656 avail_mem=100.17 GB):   5%|▌         | 3/58 [00:01<00:29,  1.86it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=100.17 GB):   7%|▋         | 4/58 [00:02<00:27,  1.97it/s]Capturing num tokens (num_tokens=6144 avail_mem=100.17 GB):   7%|▋         | 4/58 [00:02<00:27,  1.97it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=100.17 GB):   9%|▊         | 5/58 [00:02<00:25,  2.06it/s]Capturing num tokens (num_tokens=5632 avail_mem=100.18 GB):   9%|▊         | 5/58 [00:02<00:25,  2.06it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=100.18 GB):  10%|█         | 6/58 [00:02<00:23,  2.21it/s]Capturing num tokens (num_tokens=5120 avail_mem=100.18 GB):  10%|█         | 6/58 [00:02<00:23,  2.21it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=100.18 GB):  12%|█▏        | 7/58 [00:03<00:21,  2.36it/s]Capturing num tokens (num_tokens=4608 avail_mem=100.18 GB):  12%|█▏        | 7/58 [00:03<00:21,  2.36it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=100.18 GB):  14%|█▍        | 8/58 [00:03<00:24,  2.00it/s]Capturing num tokens (num_tokens=4096 avail_mem=100.15 GB):  14%|█▍        | 8/58 [00:03<00:24,  2.00it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=100.15 GB):  16%|█▌        | 9/58 [00:04<00:21,  2.30it/s]Capturing num tokens (num_tokens=3840 avail_mem=100.15 GB):  16%|█▌        | 9/58 [00:04<00:21,  2.30it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=100.15 GB):  17%|█▋        | 10/58 [00:04<00:18,  2.59it/s]Capturing num tokens (num_tokens=3584 avail_mem=100.15 GB):  17%|█▋        | 10/58 [00:04<00:18,  2.59it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=100.15 GB):  19%|█▉        | 11/58 [00:04<00:16,  2.91it/s]Capturing num tokens (num_tokens=3328 avail_mem=100.15 GB):  19%|█▉        | 11/58 [00:04<00:16,  2.91it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=100.15 GB):  21%|██        | 12/58 [00:04<00:13,  3.31it/s]Capturing num tokens (num_tokens=3072 avail_mem=100.15 GB):  21%|██        | 12/58 [00:04<00:13,  3.31it/s]Capturing num tokens (num_tokens=3072 avail_mem=100.15 GB):  22%|██▏       | 13/58 [00:05<00:11,  3.77it/s]Capturing num tokens (num_tokens=2816 avail_mem=100.15 GB):  22%|██▏       | 13/58 [00:05<00:11,  3.77it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=100.15 GB):  24%|██▍       | 14/58 [00:05<00:10,  4.27it/s]Capturing num tokens (num_tokens=2560 avail_mem=100.15 GB):  24%|██▍       | 14/58 [00:05<00:10,  4.27it/s]Capturing num tokens (num_tokens=2560 avail_mem=100.15 GB):  26%|██▌       | 15/58 [00:05<00:08,  5.01it/s]Capturing num tokens (num_tokens=2304 avail_mem=100.15 GB):  26%|██▌       | 15/58 [00:05<00:08,  5.01it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=100.15 GB):  28%|██▊       | 16/58 [00:05<00:07,  5.72it/s]Capturing num tokens (num_tokens=2048 avail_mem=100.15 GB):  28%|██▊       | 16/58 [00:05<00:07,  5.72it/s]Capturing num tokens (num_tokens=1792 avail_mem=100.15 GB):  28%|██▊       | 16/58 [00:05<00:07,  5.72it/s]Capturing num tokens (num_tokens=1792 avail_mem=100.15 GB):  31%|███       | 18/58 [00:05<00:05,  7.42it/s]Capturing num tokens (num_tokens=1536 avail_mem=100.14 GB):  31%|███       | 18/58 [00:05<00:05,  7.42it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=100.14 GB):  31%|███       | 18/58 [00:05<00:05,  7.42it/s]Capturing num tokens (num_tokens=1280 avail_mem=100.14 GB):  34%|███▍      | 20/58 [00:05<00:04,  9.16it/s]Capturing num tokens (num_tokens=1024 avail_mem=100.14 GB):  34%|███▍      | 20/58 [00:05<00:04,  9.16it/s]Capturing num tokens (num_tokens=960 avail_mem=100.14 GB):  34%|███▍      | 20/58 [00:05<00:04,  9.16it/s] Capturing num tokens (num_tokens=960 avail_mem=100.14 GB):  38%|███▊      | 22/58 [00:06<00:03, 10.73it/s]Capturing num tokens (num_tokens=896 avail_mem=100.13 GB):  38%|███▊      | 22/58 [00:06<00:03, 10.73it/s]

    Capturing num tokens (num_tokens=832 avail_mem=100.13 GB):  38%|███▊      | 22/58 [00:06<00:03, 10.73it/s]Capturing num tokens (num_tokens=768 avail_mem=100.12 GB):  38%|███▊      | 22/58 [00:06<00:03, 10.73it/s]Capturing num tokens (num_tokens=768 avail_mem=100.12 GB):  43%|████▎     | 25/58 [00:06<00:02, 14.06it/s]Capturing num tokens (num_tokens=704 avail_mem=100.12 GB):  43%|████▎     | 25/58 [00:06<00:02, 14.06it/s]Capturing num tokens (num_tokens=640 avail_mem=100.12 GB):  43%|████▎     | 25/58 [00:06<00:02, 14.06it/s]Capturing num tokens (num_tokens=576 avail_mem=100.11 GB):  43%|████▎     | 25/58 [00:06<00:02, 14.06it/s]Capturing num tokens (num_tokens=576 avail_mem=100.11 GB):  48%|████▊     | 28/58 [00:06<00:01, 17.65it/s]Capturing num tokens (num_tokens=512 avail_mem=100.11 GB):  48%|████▊     | 28/58 [00:06<00:01, 17.65it/s]

    Capturing num tokens (num_tokens=480 avail_mem=100.11 GB):  48%|████▊     | 28/58 [00:06<00:01, 17.65it/s]Capturing num tokens (num_tokens=480 avail_mem=100.11 GB):  52%|█████▏    | 30/58 [00:06<00:01, 17.23it/s]Capturing num tokens (num_tokens=448 avail_mem=99.07 GB):  52%|█████▏    | 30/58 [00:06<00:01, 17.23it/s] 

    Capturing num tokens (num_tokens=416 avail_mem=99.07 GB):  52%|█████▏    | 30/58 [00:06<00:01, 17.23it/s]Capturing num tokens (num_tokens=416 avail_mem=99.07 GB):  55%|█████▌    | 32/58 [00:06<00:01, 13.61it/s]Capturing num tokens (num_tokens=384 avail_mem=100.41 GB):  55%|█████▌    | 32/58 [00:06<00:01, 13.61it/s]Capturing num tokens (num_tokens=352 avail_mem=100.06 GB):  55%|█████▌    | 32/58 [00:06<00:01, 13.61it/s]

    Capturing num tokens (num_tokens=352 avail_mem=100.06 GB):  59%|█████▊    | 34/58 [00:06<00:01, 13.48it/s]Capturing num tokens (num_tokens=320 avail_mem=99.24 GB):  59%|█████▊    | 34/58 [00:06<00:01, 13.48it/s] Capturing num tokens (num_tokens=288 avail_mem=99.23 GB):  59%|█████▊    | 34/58 [00:06<00:01, 13.48it/s]

    Capturing num tokens (num_tokens=288 avail_mem=99.23 GB):  62%|██████▏   | 36/58 [00:06<00:01, 12.08it/s]Capturing num tokens (num_tokens=256 avail_mem=99.23 GB):  62%|██████▏   | 36/58 [00:06<00:01, 12.08it/s]Capturing num tokens (num_tokens=240 avail_mem=100.05 GB):  62%|██████▏   | 36/58 [00:07<00:01, 12.08it/s]Capturing num tokens (num_tokens=240 avail_mem=100.05 GB):  66%|██████▌   | 38/58 [00:07<00:01, 12.49it/s]Capturing num tokens (num_tokens=224 avail_mem=100.05 GB):  66%|██████▌   | 38/58 [00:07<00:01, 12.49it/s]

    Capturing num tokens (num_tokens=208 avail_mem=99.27 GB):  66%|██████▌   | 38/58 [00:07<00:01, 12.49it/s] Capturing num tokens (num_tokens=208 avail_mem=99.27 GB):  69%|██████▉   | 40/58 [00:07<00:01, 12.26it/s]Capturing num tokens (num_tokens=192 avail_mem=99.27 GB):  69%|██████▉   | 40/58 [00:07<00:01, 12.26it/s]

    Capturing num tokens (num_tokens=176 avail_mem=99.27 GB):  69%|██████▉   | 40/58 [00:07<00:01, 12.26it/s]Capturing num tokens (num_tokens=176 avail_mem=99.27 GB):  72%|███████▏  | 42/58 [00:07<00:01, 11.84it/s]Capturing num tokens (num_tokens=160 avail_mem=100.04 GB):  72%|███████▏  | 42/58 [00:07<00:01, 11.84it/s]Capturing num tokens (num_tokens=144 avail_mem=99.32 GB):  72%|███████▏  | 42/58 [00:07<00:01, 11.84it/s] 

    Capturing num tokens (num_tokens=144 avail_mem=99.32 GB):  76%|███████▌  | 44/58 [00:07<00:01, 11.53it/s]Capturing num tokens (num_tokens=128 avail_mem=99.33 GB):  76%|███████▌  | 44/58 [00:07<00:01, 11.53it/s]Capturing num tokens (num_tokens=112 avail_mem=99.33 GB):  76%|███████▌  | 44/58 [00:07<00:01, 11.53it/s]Capturing num tokens (num_tokens=112 avail_mem=99.33 GB):  79%|███████▉  | 46/58 [00:07<00:01, 11.28it/s]Capturing num tokens (num_tokens=96 avail_mem=100.03 GB):  79%|███████▉  | 46/58 [00:07<00:01, 11.28it/s]

    Capturing num tokens (num_tokens=80 avail_mem=99.37 GB):  79%|███████▉  | 46/58 [00:07<00:01, 11.28it/s] Capturing num tokens (num_tokens=80 avail_mem=99.37 GB):  83%|████████▎ | 48/58 [00:08<00:00, 10.99it/s]Capturing num tokens (num_tokens=64 avail_mem=99.37 GB):  83%|████████▎ | 48/58 [00:08<00:00, 10.99it/s]Capturing num tokens (num_tokens=48 avail_mem=100.03 GB):  83%|████████▎ | 48/58 [00:08<00:00, 10.99it/s]

    Capturing num tokens (num_tokens=48 avail_mem=100.03 GB):  86%|████████▌ | 50/58 [00:08<00:00, 11.70it/s]Capturing num tokens (num_tokens=32 avail_mem=99.42 GB):  86%|████████▌ | 50/58 [00:08<00:00, 11.70it/s] Capturing num tokens (num_tokens=28 avail_mem=99.42 GB):  86%|████████▌ | 50/58 [00:08<00:00, 11.70it/s]Capturing num tokens (num_tokens=28 avail_mem=99.42 GB):  90%|████████▉ | 52/58 [00:08<00:00, 11.56it/s]Capturing num tokens (num_tokens=24 avail_mem=100.01 GB):  90%|████████▉ | 52/58 [00:08<00:00, 11.56it/s]

    Capturing num tokens (num_tokens=20 avail_mem=100.00 GB):  90%|████████▉ | 52/58 [00:08<00:00, 11.56it/s]Capturing num tokens (num_tokens=20 avail_mem=100.00 GB):  93%|█████████▎| 54/58 [00:08<00:00, 12.05it/s]Capturing num tokens (num_tokens=16 avail_mem=99.45 GB):  93%|█████████▎| 54/58 [00:08<00:00, 12.05it/s] Capturing num tokens (num_tokens=12 avail_mem=99.49 GB):  93%|█████████▎| 54/58 [00:08<00:00, 12.05it/s]

    Capturing num tokens (num_tokens=12 avail_mem=99.49 GB):  97%|█████████▋| 56/58 [00:08<00:00, 10.46it/s]Capturing num tokens (num_tokens=8 avail_mem=102.62 GB):  97%|█████████▋| 56/58 [00:08<00:00, 10.46it/s]Capturing num tokens (num_tokens=4 avail_mem=102.12 GB):  97%|█████████▋| 56/58 [00:08<00:00, 10.46it/s]Capturing num tokens (num_tokens=4 avail_mem=102.12 GB): 100%|██████████| 58/58 [00:08<00:00, 10.80it/s]Capturing num tokens (num_tokens=4 avail_mem=102.12 GB): 100%|██████████| 58/58 [00:08<00:00,  6.52it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:116: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
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


<strong style='color: #00008B;'>reasoing_content: Okay, the user is asking for the current date and time in New York and the weather there. I need to figure out how to get this using the provided functions. <br><br>First, I'll use the get_current_date function. The required parameter is timezone, which should be 'America/New_York'. I'll structure the function call with that parameter.<br><br>Next, for the weather, I'll use get_current_weather. The city is New York, the state is NY, and the unit is probably Fahrenheit since that's a common request. So the parameters will be city: 'New York', state: 'NY', unit: 'fahrenheit'.<br><br>I need to make sure each function call is separate and follows the required format. I'll write each function call on its own line, specifying the parameters correctly. <br><br>Also, I should remember to include the source for each function, so I'll add {source: 'get_current_date'} and {source: 'get_current_weather'} at the end of each line. <br><br>Putting it all together, I'll send two separate function calls to the user, each with their parameters and sources. That should cover both the date/time and the weather information they're asking for.<br><br><br>content: <function=get_current_date>{  <br>  "timezone": "America/New_York"  <br>}</function>  <br>Source: {source: 'get_current_date'}  <br><br><function=get_current_weather>{"city": "New York", "state": "NY", "unit": "fahrenheit"}</function>  <br>Source: {source: 'get_current_weather'}</strong>


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

    {'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. I know that Paris is the capital, so that\'s straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I\'m not sure of the exact number. I think it\'s around 2 million, but I should verify that.\n\nI\'ll check a reliable source, maybe the official Paris Municipality website or a recent census. Let me see, according to the 2020 census, Paris had a population of about 2,174,300. That seems accurate. I should make sure to include this number in the JSON.\n\nNext, I need to structure this information into a JSON format. The user wants a JSON, so I\'ll create an object with a "name" field for the city, a "population" field, and maybe a "description" for more context. The description can mention that Paris is the capital and a major city in France.\n\nI should also consider the format. The JSON should be properly formatted with keys and values, and each key should be a string. The population number should be an integer since it\'s a count of people.\n\nPutting it all together, the JSON will have the city name, population, and a brief description. I\'ll make sure the syntax is correct, with commas and brackets in the right places to avoid errors.\n\nFinally, I\'ll present the JSON to the user, keeping it simple and clear. I don\'t need to add extra information unless the user asks for it, so I\'ll stick to the basics they requested.\n</think>{\n\n"name": "Paris",\n"population": 2174300000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1265, 1477, 279, 1429, 3213, 7042, 821, 13, 358, 6099, 429, 279, 7042, 315, 12095, 702, 1012, 7826, 11, 714, 358, 2776, 537, 2704, 315, 279, 4734, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 10146, 429, 382, 40, 3278, 1779, 264, 14720, 2530, 11, 7196, 279, 3946, 12095, 35703, 2719, 3910, 476, 264, 3213, 43602, 13, 6771, 752, 1490, 11, 4092, 311, 279, 220, 17, 15, 17, 15, 43602, 11, 12095, 1030, 264, 7042, 315, 911, 220, 17, 11, 16, 22, 19, 11, 18, 15, 15, 13, 2938, 4977, 13382, 13, 358, 1265, 1281, 2704, 311, 2924, 419, 1372, 304, 279, 4718, 382, 5847, 11, 358, 1184, 311, 5944, 419, 1995, 1119, 264, 4718, 3561, 13, 576, 1196, 6801, 264, 4718, 11, 773, 358, 3278, 1855, 458, 1633, 448, 264, 330, 606, 1, 2070, 369, 279, 3283, 11, 264, 330, 44441, 1, 2070, 11, 323, 7196, 264, 330, 4684, 1, 369, 803, 2266, 13, 576, 4008, 646, 6286, 429, 12095, 374, 279, 6722, 323, 264, 3598, 3283, 304, 9625, 382, 40, 1265, 1083, 2908, 279, 3561, 13, 576, 4718, 1265, 387, 10277, 23126, 448, 6894, 323, 2750, 11, 323, 1817, 1376, 1265, 387, 264, 914, 13, 576, 7042, 1372, 1265, 387, 458, 7546, 2474, 432, 594, 264, 1760, 315, 1251, 382, 97904, 432, 678, 3786, 11, 279, 4718, 686, 614, 279, 3283, 829, 11, 7042, 11, 323, 264, 9814, 4008, 13, 358, 3278, 1281, 2704, 279, 19482, 374, 4396, 11, 448, 76602, 323, 38929, 304, 279, 1290, 7482, 311, 5648, 5975, 382, 23949, 11, 358, 3278, 3042, 279, 4718, 311, 279, 1196, 11, 10282, 432, 4285, 323, 2797, 13, 358, 1513, 944, 1184, 311, 912, 4960, 1995, 7241, 279, 1196, 17064, 369, 432, 11, 773, 358, 3278, 9214, 311, 279, 31774, 807, 11223, 624, 151649, 4257, 1, 606, 788, 330, 59604, 756, 1, 44441, 788, 220, 17, 16, 22, 19, 18, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15], 'meta_info': {'id': '16ca40ec76ef4d8a864a1f299c261720', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 21.946826566942036, 'response_sent_to_client_ts': 1774294757.7175763}}



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


<strong style='color: #00008B;'>{'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down. First, I need to identify what the capital of France is. I know that Paris is the capital, so that\'s the starting point.\n\nNext, I need to find the population of Paris. I remember that Paris is a major city with a large population, but I\'m not exactly sure of the current number. I think it\'s around 2 million, but I should double-check that. Maybe I can recall that it\'s approximately 2,150,000 as of recent estimates.\n\nNow, the user wants this information in JSON format. JSON stands for JavaScript Object Notation, which is a way to structure data. I need to create a JSON object that includes the key "capital" with the value "Paris" and another key "population" with the number I just thought of.\n\nI should make sure the JSON syntax is correct. That means using double quotes for keys and string values, and commas appropriately between key-value pairs. Also, the numbers should be in quotes if they\'re strings, but population is a number, so it should be without quotes.\n\nPutting it all together, the JSON object should look like this: {"capital": "Paris", "population": 2150000}. I should present this clearly so the user can easily understand and use the information.\n\nI wonder if the user needs more details, like the population figure\'s source or the exact year it was recorded. But since they didn\'t ask for that, I\'ll stick to the information requested. Maybe they just need a straightforward data structure for a program or a report.\n\nAlso, considering the user\'s request, they might be a student working on a project, or perhaps someone developing an app that requires geographical data. Either way, providing the information in a structured format like JSON would be helpful for their purposes.\n\nI should make sure there are no typos in the JSON to avoid any issues when the user tries to use it. Double-checking the spelling of "capital" and "population" is important. Also, ensuring that the commas are correctly placed between the key-value pairs.\n\nIn summary, the user needs the capital of France and its population in JSON. I\'ve identified Paris as the capital and approximately 2,150,000 as the population. Structuring this into a JSON object with appropriate keys and correct syntax should meet their needs.\n</think>{\n\n"name": "Paris",\n"population": 2150000}', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 13, 5512, 11, 358, 1184, 311, 10542, 1128, 279, 6722, 315, 9625, 374, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 279, 5916, 1459, 382, 5847, 11, 358, 1184, 311, 1477, 279, 7042, 315, 12095, 13, 358, 6099, 429, 12095, 374, 264, 3598, 3283, 448, 264, 3460, 7042, 11, 714, 358, 2776, 537, 6896, 2704, 315, 279, 1482, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 1990, 15934, 429, 13, 10696, 358, 646, 19091, 429, 432, 594, 13187, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 438, 315, 3213, 17530, 382, 7039, 11, 279, 1196, 6801, 419, 1995, 304, 4718, 3561, 13, 4718, 13352, 369, 12914, 3002, 2806, 367, 11, 892, 374, 264, 1616, 311, 5944, 821, 13, 358, 1184, 311, 1855, 264, 4718, 1633, 429, 5646, 279, 1376, 330, 65063, 1, 448, 279, 897, 330, 59604, 1, 323, 2441, 1376, 330, 44441, 1, 448, 279, 1372, 358, 1101, 3381, 315, 382, 40, 1265, 1281, 2704, 279, 4718, 19482, 374, 4396, 13, 2938, 3363, 1667, 1990, 17194, 369, 6894, 323, 914, 2750, 11, 323, 76602, 34901, 1948, 1376, 19083, 13530, 13, 7281, 11, 279, 5109, 1265, 387, 304, 17194, 421, 807, 2299, 9069, 11, 714, 7042, 374, 264, 1372, 11, 773, 432, 1265, 387, 2041, 17194, 382, 97904, 432, 678, 3786, 11, 279, 4718, 1633, 1265, 1401, 1075, 419, 25, 5212, 65063, 788, 330, 59604, 497, 330, 44441, 788, 220, 17, 16, 20, 15, 15, 15, 15, 7810, 358, 1265, 3042, 419, 9355, 773, 279, 1196, 646, 6707, 3535, 323, 990, 279, 1995, 382, 40, 5775, 421, 279, 1196, 3880, 803, 3565, 11, 1075, 279, 7042, 7071, 594, 2530, 476, 279, 4734, 1042, 432, 572, 12433, 13, 1988, 2474, 807, 3207, 944, 2548, 369, 429, 11, 358, 3278, 9214, 311, 279, 1995, 11223, 13, 10696, 807, 1101, 1184, 264, 30339, 821, 5944, 369, 264, 2025, 476, 264, 1895, 382, 13394, 11, 12831, 279, 1196, 594, 1681, 11, 807, 2578, 387, 264, 5458, 3238, 389, 264, 2390, 11, 476, 8365, 4325, 11220, 458, 906, 429, 7460, 52901, 821, 13, 20988, 1616, 11, 8241, 279, 1995, 304, 264, 32930, 3561, 1075, 4718, 1035, 387, 10950, 369, 862, 9895, 382, 40, 1265, 1281, 2704, 1052, 525, 902, 13580, 966, 304, 279, 4718, 311, 5648, 894, 4714, 979, 279, 1196, 16297, 311, 990, 432, 13, 7093, 15934, 287, 279, 42429, 315, 330, 65063, 1, 323, 330, 44441, 1, 374, 2989, 13, 7281, 11, 22573, 429, 279, 76602, 525, 12440, 9099, 1948, 279, 1376, 19083, 13530, 382, 641, 12126, 11, 279, 1196, 3880, 279, 6722, 315, 9625, 323, 1181, 7042, 304, 4718, 13, 358, 3003, 10820, 12095, 438, 279, 6722, 323, 13187, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 438, 279, 7042, 13, 16139, 1677, 419, 1119, 264, 4718, 1633, 448, 8311, 6894, 323, 4396, 19482, 1265, 3367, 862, 3880, 624, 151649, 4257, 1, 606, 788, 330, 59604, 756, 1, 44441, 788, 220, 17, 16, 20, 15, 15, 15, 15, 92, 151643], 'meta_info': {'id': 'b5834edacebf432fb27ebba0773921f4', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 528, 'cached_tokens': 22, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 5.705374368000776, 'response_sent_to_client_ts': 1774294763.4342685}}</strong>


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

    [{'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '7f9295d131214371aa682e208dbd1503', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.12720993207767606, 'response_sent_to_client_ts': 1774294763.5916536}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '9a9e446e52ca4b91b2c4d95cd28961ab', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.12714327173307538, 'response_sent_to_client_ts': 1774294763.5916674}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': 'd167226c7d01426388c21eafa75ffa89', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.12708612717688084, 'response_sent_to_client_ts': 1774294763.5916727}}]


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

    {'text': ' France, and the \n\\( n \\)  \\( m \\) \\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\)', 'output_ids': [9625, 11, 323, 279, 220, 198, 44292, 308, 1124, 8, 220, 17767, 296, 1124, 8, 17767, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8], 'meta_info': {'id': '09b7b451b42a413099c9b3dd086091ab', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 6, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 18.932407641783357, 'response_sent_to_client_ts': 1774294782.5321481}}


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


<strong style='color: #00008B;'>{'text': 'Okay, so I need to figure out how to respond to the user\'s request. They asked for the information and population of the capital of France in JSON format. Let me break this down step by step.\n\nFirst, I should identify the capital of France. I know that Paris is the capital, no question about it. That\'s straightforward. \n\nNext, the population part is a bit trickier. The population of a city can change over time, so I need to ensure I\'m getting the most accurate and recent data. I remember that populations can fluctuate due to birth rates, death rates, and migration. But I\'m not sure about the exact current population number. \n\nI think the best way is to look up the latest statistics. I suppose I could search online for the most recent population data for Paris. But to make sure I\'m up-to-date, I might need to check a reliable source, like a recent census or a reputable government website. \n\nWait, I\'m a bit concerned about the exact figure. I know that Paris is a major city, so its population must be in the hundreds of thousands. I think I\'ve heard it\'s around 12 million, but I\'m not entirely sure. Perhaps I should confirm this by checking official sources or recent news articles. \n\nI should also consider whether the user expects just a simple number or more detailed information. The user mentioned "information" and "population," so maybe they\'re looking for more than just a number. Maybe some context or details about Paris\'s demographics. \n\nIncluding additional details could be helpful. For example, Paris has a diverse population with a wide range of ages and ethnic groups. It\'s also known for being a cultural and economic hub, which might influence the population dynamics. \n\nI need to structure this in JSON format, as per the user\'s request. JSON requires key-value pairs, so I\'ll need to have keys like "city", "population", and perhaps others if more details are needed. Maybe "year" if they want to know if the data is up to date. \n\nWait, the user didn\'t specify a time frame. So perhaps just provide the population as of the most recent data available. Let me search again to confirm the number. Upon checking, I see that as of 2023, Paris\' population is approximately 12,561,000 people. \n\nSo, putting it all together, I\'ll create a JSON object with the city and population fields. I should also add a comment mentioning that the population figure is approximate and up to date for accuracy. \n\nOne thing I\'m worried about is whether I\'m using the correct units—kilometers for area, for example, but since the user asked for population, that\'s not an issue here. \n\nI should double-check the numbers again to make sure I\'m providing accurate information. Let me verify the population from a reliable source, like the Paris Municipal Government\'s website or a recent census. Yes, it does say about 12.5 million. \n\nAlright, I think I\'m ready to put it all together. The JSON should be clear and concise, with relevant information about the capital and its population. Adding the comment will assure the user that the data is current.\n</think>\n\nHere is the JSON information about the capital of France, which is Paris, including its population:\n\n```json\n{\n  "capital": "Paris",\n  "population": 12561000,\n  "year": 2023,\n  "comment": "Population figure is approximate and up to date as of [2023]"\n}\n```\n\nThis JSON includes the capital city, its population, and a year indicating the data\'s freshness. The comment reassures the user that the population figure is current.', 'output_ids': [32313, 11, 773, 358, 1184, 311, 7071, 700, 1246, 311, 5889, 311, 279, 1196, 594, 1681, 13, 2379, 4588, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 3019, 553, 3019, 382, 5338, 11, 358, 1265, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 902, 3405, 911, 432, 13, 2938, 594, 30339, 13, 4710, 5847, 11, 279, 7042, 949, 374, 264, 2699, 14068, 1268, 13, 576, 7042, 315, 264, 3283, 646, 2297, 916, 882, 11, 773, 358, 1184, 311, 5978, 358, 2776, 3709, 279, 1429, 13382, 323, 3213, 821, 13, 358, 6099, 429, 21910, 646, 38288, 6292, 4152, 311, 7194, 7813, 11, 4545, 7813, 11, 323, 11906, 13, 1988, 358, 2776, 537, 2704, 911, 279, 4734, 1482, 7042, 1372, 13, 4710, 40, 1744, 279, 1850, 1616, 374, 311, 1401, 705, 279, 5535, 13142, 13, 358, 22477, 358, 1410, 2711, 2860, 369, 279, 1429, 3213, 7042, 821, 369, 12095, 13, 1988, 311, 1281, 2704, 358, 2776, 705, 4686, 18413, 11, 358, 2578, 1184, 311, 1779, 264, 14720, 2530, 11, 1075, 264, 3213, 43602, 476, 264, 55840, 3033, 3910, 13, 4710, 14190, 11, 358, 2776, 264, 2699, 11658, 911, 279, 4734, 7071, 13, 358, 1414, 429, 12095, 374, 264, 3598, 3283, 11, 773, 1181, 7042, 1969, 387, 304, 279, 11499, 315, 9037, 13, 358, 1744, 358, 3003, 6617, 432, 594, 2163, 220, 16, 17, 3526, 11, 714, 358, 2776, 537, 11368, 2704, 13, 18765, 358, 1265, 7683, 419, 553, 13295, 3946, 8173, 476, 3213, 3669, 9709, 13, 4710, 40, 1265, 1083, 2908, 3425, 279, 1196, 24297, 1101, 264, 4285, 1372, 476, 803, 11682, 1995, 13, 576, 1196, 9733, 330, 25069, 1, 323, 330, 44441, 1335, 773, 7196, 807, 2299, 3330, 369, 803, 1091, 1101, 264, 1372, 13, 10696, 1045, 2266, 476, 3565, 911, 12095, 594, 62234, 13, 4710, 83449, 5107, 3565, 1410, 387, 10950, 13, 1752, 3110, 11, 12095, 702, 264, 16807, 7042, 448, 264, 6884, 2088, 315, 16639, 323, 21551, 5203, 13, 1084, 594, 1083, 3881, 369, 1660, 264, 12752, 323, 6955, 18719, 11, 892, 2578, 10173, 279, 7042, 29195, 13, 4710, 40, 1184, 311, 5944, 419, 304, 4718, 3561, 11, 438, 817, 279, 1196, 594, 1681, 13, 4718, 7460, 1376, 19083, 13530, 11, 773, 358, 3278, 1184, 311, 614, 6894, 1075, 330, 8926, 497, 330, 44441, 497, 323, 8365, 3800, 421, 803, 3565, 525, 4362, 13, 10696, 330, 3157, 1, 421, 807, 1366, 311, 1414, 421, 279, 821, 374, 705, 311, 2400, 13, 4710, 14190, 11, 279, 1196, 3207, 944, 13837, 264, 882, 4034, 13, 2055, 8365, 1101, 3410, 279, 7042, 438, 315, 279, 1429, 3213, 821, 2500, 13, 6771, 752, 2711, 1549, 311, 7683, 279, 1372, 13, 29438, 13295, 11, 358, 1490, 429, 438, 315, 220, 17, 15, 17, 18, 11, 12095, 6, 7042, 374, 13187, 220, 16, 17, 11, 20, 21, 16, 11, 15, 15, 15, 1251, 13, 4710, 4416, 11, 10687, 432, 678, 3786, 11, 358, 3278, 1855, 264, 4718, 1633, 448, 279, 3283, 323, 7042, 5043, 13, 358, 1265, 1083, 912, 264, 3980, 44291, 429, 279, 7042, 7071, 374, 44868, 323, 705, 311, 2400, 369, 13403, 13, 4710, 3966, 3166, 358, 2776, 17811, 911, 374, 3425, 358, 2776, 1667, 279, 4396, 8153, 2293, 85526, 32404, 369, 3082, 11, 369, 3110, 11, 714, 2474, 279, 1196, 4588, 369, 7042, 11, 429, 594, 537, 458, 4265, 1588, 13, 4710, 40, 1265, 1990, 15934, 279, 5109, 1549, 311, 1281, 2704, 358, 2776, 8241, 13382, 1995, 13, 6771, 752, 10146, 279, 7042, 504, 264, 14720, 2530, 11, 1075, 279, 12095, 44240, 10212, 594, 3910, 476, 264, 3213, 43602, 13, 7414, 11, 432, 1558, 1977, 911, 220, 16, 17, 13, 20, 3526, 13, 4710, 71486, 11, 358, 1744, 358, 2776, 5527, 311, 2182, 432, 678, 3786, 13, 576, 4718, 1265, 387, 2797, 323, 63594, 11, 448, 9760, 1995, 911, 279, 6722, 323, 1181, 7042, 13, 30370, 279, 3980, 686, 34915, 279, 1196, 429, 279, 821, 374, 1482, 624, 151649, 271, 8420, 374, 279, 4718, 1995, 911, 279, 6722, 315, 9625, 11, 892, 374, 12095, 11, 2670, 1181, 7042, 1447, 73594, 2236, 198, 515, 220, 330, 65063, 788, 330, 59604, 756, 220, 330, 44441, 788, 220, 16, 17, 20, 21, 16, 15, 15, 15, 345, 220, 330, 3157, 788, 220, 17, 15, 17, 18, 345, 220, 330, 6182, 788, 330, 53371, 7071, 374, 44868, 323, 705, 311, 2400, 438, 315, 508, 17, 15, 17, 18, 38445, 532, 13874, 19324, 1986, 4718, 5646, 279, 6722, 3283, 11, 1181, 7042, 11, 323, 264, 1042, 18860, 279, 821, 594, 98216, 13, 576, 3980, 31734, 1413, 279, 1196, 429, 279, 7042, 7071, 374, 1482, 13, 151643], 'meta_info': {'id': '625368ff89534564b31b6982c8b948d4', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 776, 'cached_tokens': 22, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 7.239298238884658, 'response_sent_to_client_ts': 1774294789.785229}}</strong>



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

    [2026-03-23 19:39:52] INFO server_args.py:2240: Attention backend not specified. Use fa3 backend by default.


    [2026-03-23 19:39:52] INFO server_args.py:3523: Set soft_watchdog_timeout since in CI


    [2026-03-23 19:39:52] INFO engine.py:177: server_args=ServerArgs(model_path='deepseek-ai/DeepSeek-R1-Distill-Qwen-7B', tokenizer_path='deepseek-ai/DeepSeek-R1-Distill-Qwen-7B', tokenizer_mode='auto', tokenizer_worker_num=1, skip_tokenizer_init=False, load_format='auto', model_loader_extra_config='{}', trust_remote_code=False, context_length=None, is_embedding=False, enable_multimodal=None, revision=None, model_impl='auto', host='127.0.0.1', port=30000, fastapi_root_path='', grpc_mode=False, skip_server_warmup=False, warmups=None, nccl_port=None, checkpoint_engine_wait_weights_before_ready=False, ssl_keyfile=None, ssl_certfile=None, ssl_ca_certs=None, ssl_keyfile_password=None, enable_ssl_refresh=False, dtype='auto', quantization=None, quantization_param_path=None, kv_cache_dtype='auto', enable_fp32_lm_head=False, modelopt_quant=None, modelopt_checkpoint_restore_path=None, modelopt_checkpoint_save_path=None, modelopt_export_path=None, quantize_and_serve=False, rl_quant_profile=None, mem_fraction_static=0.903, max_running_requests=128, max_queued_requests=None, max_total_tokens=20480, chunked_prefill_size=8192, enable_dynamic_chunking=False, max_prefill_tokens=16384, prefill_max_requests=None, schedule_policy='fcfs', enable_priority_scheduling=False, disable_priority_preemption=False, default_priority_value=None, abort_on_priority_when_disabled=False, schedule_low_priority_values_first=False, priority_scheduling_preemption_threshold=10, schedule_conservativeness=1.0, page_size=1, swa_full_tokens_ratio=0.8, disable_hybrid_swa_memory=False, radix_eviction_policy='lru', enable_prefill_delayer=False, prefill_delayer_max_delay_passes=30, prefill_delayer_token_usage_low_watermark=None, prefill_delayer_forward_passes_buckets=None, prefill_delayer_wait_seconds_buckets=None, device='cuda', tp_size=1, pp_size=1, pp_max_micro_batch_size=None, pp_async_batch_depth=0, stream_interval=1, incremental_streaming_output=False, enable_streaming_session=False, random_seed=823510704, constrained_json_whitespace_pattern=None, constrained_json_disable_any_whitespace=False, watchdog_timeout=300, soft_watchdog_timeout=300, dist_timeout=None, download_dir=None, model_checksum=None, base_gpu_id=0, gpu_id_step=1, sleep_on_idle=False, use_ray=False, custom_sigquit_handler=None, log_level='error', log_level_http=None, log_requests=False, log_requests_level=2, log_requests_format='text', log_requests_target=None, uvicorn_access_log_exclude_prefixes=[], crash_dump_folder=None, show_time_cost=False, enable_metrics=False, enable_metrics_for_all_schedulers=False, tokenizer_metrics_custom_labels_header='x-custom-labels', tokenizer_metrics_allowed_custom_labels=None, extra_metric_labels=None, bucket_time_to_first_token=None, bucket_inter_token_latency=None, bucket_e2e_request_latency=None, collect_tokens_histogram=False, prompt_tokens_buckets=None, generation_tokens_buckets=None, gc_warning_threshold_secs=0.0, decode_log_interval=40, enable_request_time_stats_logging=False, kv_events_config=None, enable_trace=False, otlp_traces_endpoint='localhost:4317', export_metrics_to_file=False, export_metrics_to_file_dir=None, api_key=None, admin_api_key=None, served_model_name='deepseek-ai/DeepSeek-R1-Distill-Qwen-7B', weight_version='default', chat_template=None, hf_chat_template_name=None, completion_template=None, file_storage_path='sglang_storage', enable_cache_report=False, reasoning_parser='deepseek-r1', tool_call_parser=None, tool_server=None, sampling_defaults='model', dp_size=1, load_balance_method='round_robin', attn_cp_size=1, moe_dp_size=1, dist_init_addr=None, nnodes=1, node_rank=0, json_model_override_args='{}', preferred_sampling_params=None, enable_lora=None, enable_lora_overlap_loading=None, max_lora_rank=None, lora_target_modules=None, lora_paths=None, max_loaded_loras=None, max_loras_per_batch=8, lora_eviction_policy='lru', lora_backend='csgmv', max_lora_chunk_size=16, attention_backend='fa3', decode_attention_backend=None, prefill_attention_backend=None, sampling_backend='flashinfer', grammar_backend='xgrammar', mm_attention_backend=None, fp8_gemm_runner_backend='auto', fp4_gemm_runner_backend='auto', nsa_prefill_backend=None, nsa_decode_backend=None, disable_flashinfer_autotune=False, mamba_backend='triton', speculative_algorithm=None, speculative_draft_model_path=None, speculative_draft_model_revision=None, speculative_draft_load_format=None, speculative_num_steps=None, speculative_eagle_topk=None, speculative_num_draft_tokens=None, speculative_accept_threshold_single=1.0, speculative_accept_threshold_acc=1.0, speculative_token_map=None, speculative_attention_mode='prefill', speculative_draft_attention_backend=None, speculative_moe_runner_backend='auto', speculative_moe_a2a_backend=None, speculative_draft_model_quantization=None, speculative_ngram_min_match_window_size=1, speculative_ngram_max_match_window_size=12, speculative_ngram_min_bfs_breadth=1, speculative_ngram_max_bfs_breadth=10, speculative_ngram_match_type='BFS', speculative_ngram_max_trie_depth=18, speculative_ngram_capacity=10000000, enable_multi_layer_eagle=False, ep_size=1, moe_a2a_backend='none', moe_runner_backend='auto', flashinfer_mxfp4_moe_precision='default', enable_flashinfer_allreduce_fusion=False, enable_aiter_allreduce_fusion=False, deepep_mode='auto', ep_num_redundant_experts=0, ep_dispatch_algorithm=None, init_expert_location='trivial', enable_eplb=False, eplb_algorithm='auto', eplb_rebalance_num_iterations=1000, eplb_rebalance_layers_per_chunk=None, eplb_min_rebalancing_utilization_threshold=1.0, expert_distribution_recorder_mode=None, expert_distribution_recorder_buffer_size=1000, enable_expert_distribution_metrics=False, deepep_config=None, moe_dense_tp_size=None, elastic_ep_backend=None, enable_elastic_expert_backup=False, mooncake_ib_device=None, max_mamba_cache_size=None, mamba_ssm_dtype=None, mamba_full_memory_ratio=0.9, mamba_scheduler_strategy='no_buffer', mamba_track_interval=256, linear_attn_backend='triton', linear_attn_decode_backend=None, linear_attn_prefill_backend=None, enable_hierarchical_cache=False, hicache_ratio=2.0, hicache_size=0, hicache_write_policy='write_through', hicache_io_backend='kernel', hicache_mem_layout='layer_first', disable_hicache_numa_detect=False, hicache_storage_backend=None, hicache_storage_prefetch_policy='best_effort', hicache_storage_backend_extra_config=None, enable_hisparse=False, hisparse_config=None, enable_lmcache=False, kt_weight_path=None, kt_method=None, kt_cpuinfer=None, kt_threadpool_count=None, kt_num_gpu_experts=None, kt_max_deferred_experts_per_token=None, dllm_algorithm=None, dllm_algorithm_config=None, enable_double_sparsity=False, ds_channel_config_path=None, ds_heavy_channel_num=32, ds_heavy_token_num=256, ds_heavy_channel_type='qk', ds_sparse_decode_threshold=4096, cpu_offload_gb=0, offload_group_size=-1, offload_num_in_group=1, offload_prefetch_step=1, offload_mode='cpu', multi_item_scoring_delimiter=None, disable_radix_cache=False, cuda_graph_max_bs=4, cuda_graph_bs=[1, 2, 4, 8, 12, 16, 24, 32, 40, 48, 56, 64, 72, 80, 88, 96, 104, 112, 120, 128, 136, 144, 152, 160, 168, 176, 184, 192, 200, 208, 216, 224, 232, 240, 248, 256], disable_cuda_graph=True, disable_cuda_graph_padding=False, enable_profile_cuda_graph=False, enable_cudagraph_gc=False, enable_layerwise_nvtx_marker=False, enable_nccl_nvls=False, enable_symm_mem=False, disable_flashinfer_cutlass_moe_fp4_allgather=False, enable_tokenizer_batch_encode=False, disable_tokenizer_batch_decode=False, disable_outlines_disk_cache=False, disable_custom_all_reduce=False, enable_mscclpp=False, enable_torch_symm_mem=False, pre_warm_nccl=False, disable_overlap_schedule=False, enable_mixed_chunk=False, enable_dp_attention=False, enable_dp_lm_head=False, enable_two_batch_overlap=False, enable_single_batch_overlap=False, tbo_token_distribution_threshold=0.48, enable_torch_compile=False, disable_piecewise_cuda_graph=False, enforce_piecewise_cuda_graph=False, enable_torch_compile_debug_mode=False, torch_compile_max_bs=32, piecewise_cuda_graph_max_tokens=8192, piecewise_cuda_graph_tokens=[4, 8, 12, 16, 20, 24, 28, 32, 48, 64, 80, 96, 112, 128, 144, 160, 176, 192, 208, 224, 240, 256, 288, 320, 352, 384, 416, 448, 480, 512, 576, 640, 704, 768, 832, 896, 960, 1024, 1280, 1536, 1792, 2048, 2304, 2560, 2816, 3072, 3328, 3584, 3840, 4096, 4608, 5120, 5632, 6144, 6656, 7168, 7680, 8192], piecewise_cuda_graph_compiler='eager', torchao_config='', enable_nan_detection=False, enable_p2p_check=False, triton_attention_reduce_in_fp32=False, triton_attention_num_kv_splits=8, triton_attention_split_tile_size=None, num_continuous_decode_steps=1, delete_ckpt_after_loading=False, enable_memory_saver=False, enable_weights_cpu_backup=False, enable_draft_weights_cpu_backup=False, allow_auto_truncate=False, enable_custom_logit_processor=False, flashinfer_mla_disable_ragged=False, disable_shared_experts_fusion=False, disable_chunked_prefix_cache=False, disable_fast_image_processor=False, keep_mm_feature_on_device=False, enable_return_hidden_states=False, enable_return_routed_experts=False, scheduler_recv_interval=1, numa_node=None, enable_deterministic_inference=False, rl_on_policy_target=None, enable_attn_tp_input_scattered=False, enable_nsa_prefill_context_parallel=False, nsa_prefill_cp_mode='round-robin-split', enable_fused_qk_norm_rope=False, enable_precise_embedding_interpolation=False, enable_fused_moe_sum_all_reduce=False, enable_prefill_context_parallel=False, prefill_cp_mode='in-seq-split', enable_dynamic_batch_tokenizer=False, dynamic_batch_tokenizer_batch_size=32, dynamic_batch_tokenizer_batch_timeout=0.002, debug_tensor_dump_output_folder=None, debug_tensor_dump_layers=None, debug_tensor_dump_input_file=None, debug_tensor_dump_inject=False, disaggregation_mode='null', disaggregation_transfer_backend='mooncake', disaggregation_bootstrap_port=8998, disaggregation_ib_device=None, disaggregation_decode_enable_offload_kvcache=False, num_reserved_decode_tokens=512, disaggregation_decode_polling_interval=1, encoder_only=False, language_only=False, encoder_transfer_backend='zmq_to_scheduler', encoder_urls=[], enable_adaptive_dispatch_to_encoder=False, custom_weight_loader=[], weight_loader_disable_mmap=False, remote_instance_weight_loader_seed_instance_ip=None, remote_instance_weight_loader_seed_instance_service_port=None, remote_instance_weight_loader_send_weights_group_ports=None, remote_instance_weight_loader_backend='nccl', remote_instance_weight_loader_start_seed_via_transfer_engine=False, modelexpress_config=None, enable_pdmux=False, pdmux_config_path=None, sm_group_num=8, mm_max_concurrent_calls=32, mm_per_request_timeout=10.0, enable_broadcast_mm_inputs_process=False, enable_prefix_mm_cache=False, mm_enable_dp_encoder=False, mm_process_config={}, limit_mm_data_per_request=None, enable_mm_global_cache=False, decrypted_config_file=None, decrypted_draft_config_file=None, forward_hooks=None)


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-03-23 19:40:02.372 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-23 19:40:02] Persistent cache disabled, using in-memory JIT cache
    2026-03-23 19:40:02.373 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-23 19:40:02] Persistent cache disabled, using in-memory JIT cache
    2026-03-23 19:40:02.373 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-23 19:40:02] Persistent cache disabled, using in-memory JIT cache
    2026-03-23 19:40:02.373 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-23 19:40:02] Persistent cache disabled, using in-memory JIT cache
    2026-03-23 19:40:02.373 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-23 19:40:02] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:00<00:00,  1.14it/s]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.16s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.12s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:04,  3.23s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:04,  3.23s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:32,  1.65s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:32,  1.65s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<00:56,  1.02s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<00:56,  1.02s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:38,  1.39it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:38,  1.39it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:28,  1.86it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:28,  1.86it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:22,  2.35it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:22,  2.35it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:17,  2.91it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:17,  2.91it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:05<00:14,  3.52it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:05<00:14,  3.52it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:05<00:11,  4.17it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:05<00:11,  4.17it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:05<00:09,  4.92it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:05<00:09,  4.92it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:05<00:08,  5.67it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:05<00:08,  5.67it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:05<00:07,  6.47it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:05<00:07,  6.47it/s]Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:05<00:07,  6.47it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:05<00:05,  7.94it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:05<00:05,  7.94it/s]

    Compiling num tokens (num_tokens=2304):  24%|██▍       | 14/58 [00:05<00:05,  7.94it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:05<00:04,  9.42it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:05<00:04,  9.42it/s]Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:05<00:04,  9.42it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:06<00:03, 11.04it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:06<00:03, 11.04it/s]

    Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:06<00:03, 11.04it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:06<00:03, 11.04it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:06<00:02, 14.01it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:06<00:02, 14.01it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:06<00:02, 14.01it/s]Compiling num tokens (num_tokens=832):  36%|███▌      | 21/58 [00:06<00:02, 14.01it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:06<00:01, 17.50it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:06<00:01, 17.50it/s]

    Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:06<00:01, 17.50it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:06<00:01, 17.50it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:06<00:01, 17.50it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:06<00:01, 22.11it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:06<00:01, 22.11it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:06<00:01, 22.11it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:06<00:01, 22.11it/s]Compiling num tokens (num_tokens=416):  48%|████▊     | 28/58 [00:06<00:01, 22.11it/s]Compiling num tokens (num_tokens=384):  48%|████▊     | 28/58 [00:06<00:01, 22.11it/s]

    Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:06<00:00, 27.41it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:06<00:00, 27.41it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:06<00:00, 27.41it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:06<00:00, 27.41it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:06<00:00, 27.41it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:06<00:00, 27.41it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:06<00:00, 31.84it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:06<00:00, 31.84it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:06<00:00, 31.84it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:06<00:00, 31.84it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:06<00:00, 31.84it/s]

    Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:06<00:00, 31.84it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:06<00:00, 35.32it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:06<00:00, 35.32it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:06<00:00, 35.32it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:06<00:00, 35.32it/s]Compiling num tokens (num_tokens=96):  74%|███████▍  | 43/58 [00:06<00:00, 35.32it/s] Compiling num tokens (num_tokens=80):  74%|███████▍  | 43/58 [00:06<00:00, 35.32it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:06<00:00, 38.78it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:06<00:00, 38.78it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:06<00:00, 38.78it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:06<00:00, 38.78it/s]Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:06<00:00, 38.78it/s]Compiling num tokens (num_tokens=24):  83%|████████▎ | 48/58 [00:06<00:00, 38.78it/s]

    Compiling num tokens (num_tokens=20):  83%|████████▎ | 48/58 [00:06<00:00, 38.78it/s]Compiling num tokens (num_tokens=16):  83%|████████▎ | 48/58 [00:06<00:00, 38.78it/s]Compiling num tokens (num_tokens=16):  95%|█████████▍| 55/58 [00:06<00:00, 45.68it/s]Compiling num tokens (num_tokens=12):  95%|█████████▍| 55/58 [00:06<00:00, 45.68it/s]Compiling num tokens (num_tokens=8):  95%|█████████▍| 55/58 [00:06<00:00, 45.68it/s] Compiling num tokens (num_tokens=4):  95%|█████████▍| 55/58 [00:06<00:00, 45.68it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:06<00:00,  8.32it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=102.57 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=102.57 GB):   2%|▏         | 1/58 [00:00<00:16,  3.49it/s]Capturing num tokens (num_tokens=7680 avail_mem=102.54 GB):   2%|▏         | 1/58 [00:00<00:16,  3.49it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=102.54 GB):   3%|▎         | 2/58 [00:00<00:15,  3.61it/s]Capturing num tokens (num_tokens=7168 avail_mem=102.54 GB):   3%|▎         | 2/58 [00:00<00:15,  3.61it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=102.54 GB):   5%|▌         | 3/58 [00:01<00:19,  2.77it/s]Capturing num tokens (num_tokens=6656 avail_mem=102.54 GB):   5%|▌         | 3/58 [00:01<00:19,  2.77it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=102.54 GB):   7%|▋         | 4/58 [00:01<00:16,  3.28it/s]Capturing num tokens (num_tokens=6144 avail_mem=102.55 GB):   7%|▋         | 4/58 [00:01<00:16,  3.28it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=102.55 GB):   9%|▊         | 5/58 [00:01<00:14,  3.72it/s]Capturing num tokens (num_tokens=5632 avail_mem=102.55 GB):   9%|▊         | 5/58 [00:01<00:14,  3.72it/s]Capturing num tokens (num_tokens=5632 avail_mem=102.55 GB):  10%|█         | 6/58 [00:01<00:12,  4.19it/s]Capturing num tokens (num_tokens=5120 avail_mem=102.55 GB):  10%|█         | 6/58 [00:01<00:12,  4.19it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=102.55 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.67it/s]Capturing num tokens (num_tokens=4608 avail_mem=102.56 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.67it/s]Capturing num tokens (num_tokens=4608 avail_mem=102.56 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.24it/s]Capturing num tokens (num_tokens=4096 avail_mem=102.56 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.24it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=102.56 GB):  16%|█▌        | 9/58 [00:02<00:08,  5.79it/s]Capturing num tokens (num_tokens=3840 avail_mem=102.56 GB):  16%|█▌        | 9/58 [00:02<00:08,  5.79it/s]Capturing num tokens (num_tokens=3840 avail_mem=102.56 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.31it/s]Capturing num tokens (num_tokens=3584 avail_mem=102.56 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.31it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=102.56 GB):  19%|█▉        | 11/58 [00:02<00:08,  5.53it/s]Capturing num tokens (num_tokens=3328 avail_mem=102.23 GB):  19%|█▉        | 11/58 [00:02<00:08,  5.53it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=102.23 GB):  21%|██        | 12/58 [00:02<00:09,  5.11it/s]Capturing num tokens (num_tokens=3072 avail_mem=102.23 GB):  21%|██        | 12/58 [00:02<00:09,  5.11it/s]Capturing num tokens (num_tokens=3072 avail_mem=102.23 GB):  22%|██▏       | 13/58 [00:02<00:08,  5.37it/s]Capturing num tokens (num_tokens=2816 avail_mem=102.23 GB):  22%|██▏       | 13/58 [00:02<00:08,  5.37it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=102.23 GB):  24%|██▍       | 14/58 [00:02<00:07,  5.96it/s]Capturing num tokens (num_tokens=2560 avail_mem=102.23 GB):  24%|██▍       | 14/58 [00:02<00:07,  5.96it/s]Capturing num tokens (num_tokens=2560 avail_mem=102.23 GB):  26%|██▌       | 15/58 [00:03<00:06,  6.79it/s]Capturing num tokens (num_tokens=2304 avail_mem=102.23 GB):  26%|██▌       | 15/58 [00:03<00:06,  6.79it/s]Capturing num tokens (num_tokens=2048 avail_mem=102.23 GB):  26%|██▌       | 15/58 [00:03<00:06,  6.79it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=102.23 GB):  29%|██▉       | 17/58 [00:03<00:04,  8.41it/s]Capturing num tokens (num_tokens=1792 avail_mem=102.23 GB):  29%|██▉       | 17/58 [00:03<00:04,  8.41it/s]Capturing num tokens (num_tokens=1536 avail_mem=102.22 GB):  29%|██▉       | 17/58 [00:03<00:04,  8.41it/s]Capturing num tokens (num_tokens=1536 avail_mem=102.22 GB):  33%|███▎      | 19/58 [00:03<00:03, 10.17it/s]Capturing num tokens (num_tokens=1280 avail_mem=102.22 GB):  33%|███▎      | 19/58 [00:03<00:03, 10.17it/s]Capturing num tokens (num_tokens=1024 avail_mem=102.22 GB):  33%|███▎      | 19/58 [00:03<00:03, 10.17it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=102.22 GB):  36%|███▌      | 21/58 [00:03<00:03, 12.26it/s]Capturing num tokens (num_tokens=960 avail_mem=102.22 GB):  36%|███▌      | 21/58 [00:03<00:03, 12.26it/s] Capturing num tokens (num_tokens=896 avail_mem=102.21 GB):  36%|███▌      | 21/58 [00:03<00:03, 12.26it/s]Capturing num tokens (num_tokens=832 avail_mem=102.21 GB):  36%|███▌      | 21/58 [00:03<00:03, 12.26it/s]Capturing num tokens (num_tokens=832 avail_mem=102.21 GB):  41%|████▏     | 24/58 [00:03<00:02, 15.12it/s]Capturing num tokens (num_tokens=768 avail_mem=102.21 GB):  41%|████▏     | 24/58 [00:03<00:02, 15.12it/s]Capturing num tokens (num_tokens=704 avail_mem=102.20 GB):  41%|████▏     | 24/58 [00:03<00:02, 15.12it/s]

    Capturing num tokens (num_tokens=640 avail_mem=102.20 GB):  41%|████▏     | 24/58 [00:03<00:02, 15.12it/s]Capturing num tokens (num_tokens=640 avail_mem=102.20 GB):  47%|████▋     | 27/58 [00:03<00:01, 17.51it/s]Capturing num tokens (num_tokens=576 avail_mem=102.20 GB):  47%|████▋     | 27/58 [00:03<00:01, 17.51it/s]Capturing num tokens (num_tokens=512 avail_mem=102.19 GB):  47%|████▋     | 27/58 [00:03<00:01, 17.51it/s]Capturing num tokens (num_tokens=480 avail_mem=102.19 GB):  47%|████▋     | 27/58 [00:03<00:01, 17.51it/s]Capturing num tokens (num_tokens=480 avail_mem=102.19 GB):  52%|█████▏    | 30/58 [00:03<00:01, 19.89it/s]Capturing num tokens (num_tokens=448 avail_mem=102.19 GB):  52%|█████▏    | 30/58 [00:03<00:01, 19.89it/s]Capturing num tokens (num_tokens=416 avail_mem=102.18 GB):  52%|█████▏    | 30/58 [00:03<00:01, 19.89it/s]

    Capturing num tokens (num_tokens=384 avail_mem=102.18 GB):  52%|█████▏    | 30/58 [00:03<00:01, 19.89it/s]Capturing num tokens (num_tokens=352 avail_mem=102.17 GB):  52%|█████▏    | 30/58 [00:03<00:01, 19.89it/s]Capturing num tokens (num_tokens=352 avail_mem=102.17 GB):  59%|█████▊    | 34/58 [00:03<00:00, 24.10it/s]Capturing num tokens (num_tokens=320 avail_mem=102.17 GB):  59%|█████▊    | 34/58 [00:03<00:00, 24.10it/s]Capturing num tokens (num_tokens=288 avail_mem=102.17 GB):  59%|█████▊    | 34/58 [00:03<00:00, 24.10it/s]Capturing num tokens (num_tokens=256 avail_mem=102.16 GB):  59%|█████▊    | 34/58 [00:03<00:00, 24.10it/s]Capturing num tokens (num_tokens=240 avail_mem=102.16 GB):  59%|█████▊    | 34/58 [00:04<00:00, 24.10it/s]Capturing num tokens (num_tokens=240 avail_mem=102.16 GB):  66%|██████▌   | 38/58 [00:04<00:00, 27.81it/s]Capturing num tokens (num_tokens=224 avail_mem=102.16 GB):  66%|██████▌   | 38/58 [00:04<00:00, 27.81it/s]Capturing num tokens (num_tokens=208 avail_mem=102.15 GB):  66%|██████▌   | 38/58 [00:04<00:00, 27.81it/s]

    Capturing num tokens (num_tokens=192 avail_mem=102.15 GB):  66%|██████▌   | 38/58 [00:04<00:00, 27.81it/s]Capturing num tokens (num_tokens=176 avail_mem=102.14 GB):  66%|██████▌   | 38/58 [00:04<00:00, 27.81it/s]Capturing num tokens (num_tokens=176 avail_mem=102.14 GB):  72%|███████▏  | 42/58 [00:04<00:00, 30.71it/s]Capturing num tokens (num_tokens=160 avail_mem=102.14 GB):  72%|███████▏  | 42/58 [00:04<00:00, 30.71it/s]Capturing num tokens (num_tokens=144 avail_mem=102.14 GB):  72%|███████▏  | 42/58 [00:04<00:00, 30.71it/s]Capturing num tokens (num_tokens=128 avail_mem=102.15 GB):  72%|███████▏  | 42/58 [00:04<00:00, 30.71it/s]Capturing num tokens (num_tokens=112 avail_mem=102.05 GB):  72%|███████▏  | 42/58 [00:04<00:00, 30.71it/s]Capturing num tokens (num_tokens=112 avail_mem=102.05 GB):  79%|███████▉  | 46/58 [00:04<00:00, 29.83it/s]Capturing num tokens (num_tokens=96 avail_mem=102.11 GB):  79%|███████▉  | 46/58 [00:04<00:00, 29.83it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=102.10 GB):  79%|███████▉  | 46/58 [00:04<00:00, 29.83it/s]Capturing num tokens (num_tokens=64 avail_mem=102.10 GB):  79%|███████▉  | 46/58 [00:04<00:00, 29.83it/s]Capturing num tokens (num_tokens=48 avail_mem=101.87 GB):  79%|███████▉  | 46/58 [00:04<00:00, 29.83it/s]Capturing num tokens (num_tokens=48 avail_mem=101.87 GB):  86%|████████▌ | 50/58 [00:04<00:00, 25.37it/s]Capturing num tokens (num_tokens=32 avail_mem=101.88 GB):  86%|████████▌ | 50/58 [00:04<00:00, 25.37it/s]Capturing num tokens (num_tokens=28 avail_mem=101.89 GB):  86%|████████▌ | 50/58 [00:04<00:00, 25.37it/s]

    Capturing num tokens (num_tokens=24 avail_mem=101.91 GB):  86%|████████▌ | 50/58 [00:04<00:00, 25.37it/s]Capturing num tokens (num_tokens=24 avail_mem=101.91 GB):  91%|█████████▏| 53/58 [00:04<00:00, 24.10it/s]Capturing num tokens (num_tokens=20 avail_mem=101.91 GB):  91%|█████████▏| 53/58 [00:04<00:00, 24.10it/s]Capturing num tokens (num_tokens=16 avail_mem=101.92 GB):  91%|█████████▏| 53/58 [00:04<00:00, 24.10it/s]Capturing num tokens (num_tokens=12 avail_mem=102.06 GB):  91%|█████████▏| 53/58 [00:04<00:00, 24.10it/s]Capturing num tokens (num_tokens=12 avail_mem=102.06 GB):  97%|█████████▋| 56/58 [00:04<00:00, 24.30it/s]Capturing num tokens (num_tokens=8 avail_mem=102.05 GB):  97%|█████████▋| 56/58 [00:04<00:00, 24.30it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=102.05 GB):  97%|█████████▋| 56/58 [00:04<00:00, 24.30it/s]Capturing num tokens (num_tokens=4 avail_mem=102.05 GB): 100%|██████████| 58/58 [00:04<00:00, 11.98it/s]


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
    
    Generated text: Alright, so the user asked me to provide the information and population of the capital of France in JSON format. First, I need to identify the capital of France, which I know is Paris. 
    
    Next, I should gather accurate data about Paris's population. I remember that Paris has a large population, but I'm not exactly sure of the current number. I think it's around 2 million, but I should confirm that. Maybe I'll check some reliable sources or memory banks to ensure the figure is up to date. 
    
    Wait, the user didn't specify a year, so I'll go with the most recent estimate. I believe as of 2023, Paris has a population of approximately 2,177,000. That seems about right, but I should double-check to make sure I'm not off by a few hundred thousand.
    
    Now, structuring this information into a JSON format. I know JSON requires key-value pairs, so I'll use "capital" as the key for the city and "population" for the number. It's important to format the numbers correctly, using commas and proper syntax to avoid any errors.
    
    Putting it all together, I'll create a JSON object with the keys and values as mentioned. This should provide the user with a clear and concise answer in the format they requested. 
    
    I don't think the user needs any additional information beyond the population of the capital. They might be looking for quick data, perhaps for a comparison or a quick fact check. So, keeping it simple and accurate is key here.
    </think>
    
    ```json
    {
      "capital": "Paris",
      "population": 2177000
    }
    ```



```python
llm.shutdown()
```
