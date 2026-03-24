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

    [2026-03-24 04:35:34] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.


    [2026-03-24 04:35:34] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.


    [2026-03-24 04:35:34] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [2026-03-24 04:35:39] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-24 04:35:39] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-24 04:35:39] INFO utils.py:164: NumExpr defaulting to 16 threads.
    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    [2026-03-24 04:35:41] INFO server_args.py:2240: Attention backend not specified. Use fa3 backend by default.
    [2026-03-24 04:35:41] INFO server_args.py:3523: Set soft_watchdog_timeout since in CI


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:175: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [2026-03-24 04:35:46] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-24 04:35:46] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-24 04:35:46] INFO utils.py:164: NumExpr defaulting to 16 threads.
    [2026-03-24 04:35:46] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-24 04:35:46] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-24 04:35:46] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-03-24 04:35:52.080 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 04:35:52] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 04:35:52.080 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 04:35:52] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 04:35:52.080 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 04:35:52] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 04:35:52.080 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 04:35:52] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 04:35:52.080 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 04:35:52] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:01<00:01,  1.00s/it]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.29s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.24s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:05,  3.25s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:05,  3.25s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:28,  1.58s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:28,  1.58s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:52,  1.05it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:52,  1.05it/s]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:35,  1.54it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:35,  1.54it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:24,  2.12it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:24,  2.12it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.75it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.75it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.48it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.48it/s]Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.27it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.27it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.15it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.15it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.15it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:06,  6.84it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:06,  6.84it/s]

    Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:06,  6.84it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:05<00:05,  8.35it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:05<00:05,  8.35it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:05<00:05,  8.35it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:05<00:04,  9.91it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:05<00:04,  9.91it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:05<00:04,  9.91it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:03, 11.84it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:03, 11.84it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:03, 11.84it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:05<00:03, 11.84it/s]

    Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 15.13it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 15.13it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 15.13it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 15.13it/s]Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:05<00:02, 15.13it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:05<00:01, 20.28it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:05<00:01, 20.28it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:05<00:01, 20.28it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:05<00:01, 20.28it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:05<00:01, 20.28it/s]

    Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:05<00:01, 20.28it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:05<00:01, 26.81it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:05<00:01, 26.81it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:05<00:01, 26.81it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:05<00:01, 26.81it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:05<00:01, 26.81it/s]Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:05<00:01, 26.81it/s]Compiling num tokens (num_tokens=320):  50%|█████     | 29/58 [00:05<00:01, 26.81it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:05<00:00, 33.95it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:05<00:00, 33.95it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:05<00:00, 33.95it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:05<00:00, 33.95it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:05<00:00, 33.95it/s]

    Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:05<00:00, 33.95it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:05<00:00, 38.06it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:05<00:00, 38.06it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:05<00:00, 38.06it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:05<00:00, 38.06it/s]Compiling num tokens (num_tokens=144):  69%|██████▉   | 40/58 [00:05<00:00, 38.06it/s]Compiling num tokens (num_tokens=128):  69%|██████▉   | 40/58 [00:06<00:00, 38.06it/s]

    Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:06<00:00, 32.64it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:06<00:00, 32.64it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:06<00:00, 32.64it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:06<00:00, 32.64it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:06<00:00, 32.64it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:06<00:00, 31.11it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:06<00:00, 31.11it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:06<00:00, 31.11it/s]

    Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:06<00:00, 31.11it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:06<00:00, 31.11it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:06<00:00, 30.84it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:06<00:00, 30.84it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:06<00:00, 30.84it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:06<00:00, 30.84it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:06<00:00, 30.84it/s] 

    Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:06<00:00, 30.08it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:06<00:00, 30.08it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:06<00:00,  8.89it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=100.39 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=100.39 GB):   2%|▏         | 1/58 [00:00<00:43,  1.31it/s]Capturing num tokens (num_tokens=7680 avail_mem=100.36 GB):   2%|▏         | 1/58 [00:00<00:43,  1.31it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=100.36 GB):   3%|▎         | 2/58 [00:01<00:37,  1.49it/s]Capturing num tokens (num_tokens=7168 avail_mem=100.35 GB):   3%|▎         | 2/58 [00:01<00:37,  1.49it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=100.35 GB):   5%|▌         | 3/58 [00:01<00:33,  1.62it/s]Capturing num tokens (num_tokens=6656 avail_mem=100.36 GB):   5%|▌         | 3/58 [00:01<00:33,  1.62it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=100.36 GB):   7%|▋         | 4/58 [00:02<00:29,  1.80it/s]Capturing num tokens (num_tokens=6144 avail_mem=100.36 GB):   7%|▋         | 4/58 [00:02<00:29,  1.80it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=100.36 GB):   9%|▊         | 5/58 [00:02<00:28,  1.85it/s]Capturing num tokens (num_tokens=5632 avail_mem=100.33 GB):   9%|▊         | 5/58 [00:02<00:28,  1.85it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=100.33 GB):  10%|█         | 6/58 [00:03<00:26,  1.96it/s]Capturing num tokens (num_tokens=5120 avail_mem=100.33 GB):  10%|█         | 6/58 [00:03<00:26,  1.96it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=100.33 GB):  12%|█▏        | 7/58 [00:03<00:24,  2.09it/s]Capturing num tokens (num_tokens=4608 avail_mem=100.33 GB):  12%|█▏        | 7/58 [00:03<00:24,  2.09it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=100.33 GB):  14%|█▍        | 8/58 [00:04<00:21,  2.30it/s]Capturing num tokens (num_tokens=4096 avail_mem=100.33 GB):  14%|█▍        | 8/58 [00:04<00:21,  2.30it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=100.33 GB):  16%|█▌        | 9/58 [00:04<00:19,  2.57it/s]Capturing num tokens (num_tokens=3840 avail_mem=100.33 GB):  16%|█▌        | 9/58 [00:04<00:19,  2.57it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=100.33 GB):  17%|█▋        | 10/58 [00:04<00:16,  2.84it/s]Capturing num tokens (num_tokens=3584 avail_mem=100.32 GB):  17%|█▋        | 10/58 [00:04<00:16,  2.84it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=100.32 GB):  19%|█▉        | 11/58 [00:04<00:14,  3.20it/s]Capturing num tokens (num_tokens=3328 avail_mem=100.31 GB):  19%|█▉        | 11/58 [00:04<00:14,  3.20it/s]Capturing num tokens (num_tokens=3328 avail_mem=100.31 GB):  21%|██        | 12/58 [00:05<00:12,  3.68it/s]Capturing num tokens (num_tokens=3072 avail_mem=100.30 GB):  21%|██        | 12/58 [00:05<00:12,  3.68it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=100.30 GB):  22%|██▏       | 13/58 [00:05<00:10,  4.19it/s]Capturing num tokens (num_tokens=2816 avail_mem=100.27 GB):  22%|██▏       | 13/58 [00:05<00:10,  4.19it/s]Capturing num tokens (num_tokens=2816 avail_mem=100.27 GB):  24%|██▍       | 14/58 [00:05<00:09,  4.82it/s]Capturing num tokens (num_tokens=2560 avail_mem=100.28 GB):  24%|██▍       | 14/58 [00:05<00:09,  4.82it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=100.28 GB):  26%|██▌       | 15/58 [00:05<00:07,  5.42it/s]Capturing num tokens (num_tokens=2304 avail_mem=100.27 GB):  26%|██▌       | 15/58 [00:05<00:07,  5.42it/s]Capturing num tokens (num_tokens=2304 avail_mem=100.27 GB):  28%|██▊       | 16/58 [00:05<00:06,  6.07it/s]Capturing num tokens (num_tokens=2048 avail_mem=100.27 GB):  28%|██▊       | 16/58 [00:05<00:06,  6.07it/s]Capturing num tokens (num_tokens=1792 avail_mem=100.26 GB):  28%|██▊       | 16/58 [00:05<00:06,  6.07it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=100.26 GB):  31%|███       | 18/58 [00:05<00:04,  8.09it/s]Capturing num tokens (num_tokens=1536 avail_mem=100.27 GB):  31%|███       | 18/58 [00:05<00:04,  8.09it/s]Capturing num tokens (num_tokens=1280 avail_mem=100.27 GB):  31%|███       | 18/58 [00:05<00:04,  8.09it/s]Capturing num tokens (num_tokens=1280 avail_mem=100.27 GB):  34%|███▍      | 20/58 [00:05<00:03, 10.06it/s]Capturing num tokens (num_tokens=1024 avail_mem=100.26 GB):  34%|███▍      | 20/58 [00:05<00:03, 10.06it/s]Capturing num tokens (num_tokens=960 avail_mem=100.23 GB):  34%|███▍      | 20/58 [00:05<00:03, 10.06it/s] 

    Capturing num tokens (num_tokens=960 avail_mem=100.23 GB):  38%|███▊      | 22/58 [00:05<00:02, 12.22it/s]Capturing num tokens (num_tokens=896 avail_mem=100.25 GB):  38%|███▊      | 22/58 [00:05<00:02, 12.22it/s]Capturing num tokens (num_tokens=832 avail_mem=99.21 GB):  38%|███▊      | 22/58 [00:06<00:02, 12.22it/s] Capturing num tokens (num_tokens=832 avail_mem=99.21 GB):  41%|████▏     | 24/58 [00:06<00:02, 11.42it/s]Capturing num tokens (num_tokens=768 avail_mem=99.20 GB):  41%|████▏     | 24/58 [00:06<00:02, 11.42it/s]

    Capturing num tokens (num_tokens=704 avail_mem=99.19 GB):  41%|████▏     | 24/58 [00:06<00:02, 11.42it/s]Capturing num tokens (num_tokens=704 avail_mem=99.19 GB):  45%|████▍     | 26/58 [00:06<00:03, 10.27it/s]Capturing num tokens (num_tokens=640 avail_mem=100.19 GB):  45%|████▍     | 26/58 [00:06<00:03, 10.27it/s]

    Capturing num tokens (num_tokens=576 avail_mem=100.18 GB):  45%|████▍     | 26/58 [00:06<00:03, 10.27it/s]Capturing num tokens (num_tokens=576 avail_mem=100.18 GB):  48%|████▊     | 28/58 [00:06<00:02, 10.14it/s]Capturing num tokens (num_tokens=512 avail_mem=99.35 GB):  48%|████▊     | 28/58 [00:06<00:02, 10.14it/s] 

    Capturing num tokens (num_tokens=480 avail_mem=99.34 GB):  48%|████▊     | 28/58 [00:06<00:02, 10.14it/s]Capturing num tokens (num_tokens=480 avail_mem=99.34 GB):  52%|█████▏    | 30/58 [00:06<00:02,  9.48it/s]Capturing num tokens (num_tokens=448 avail_mem=99.32 GB):  52%|█████▏    | 30/58 [00:06<00:02,  9.48it/s]

    Capturing num tokens (num_tokens=416 avail_mem=100.15 GB):  52%|█████▏    | 30/58 [00:06<00:02,  9.48it/s]Capturing num tokens (num_tokens=416 avail_mem=100.15 GB):  55%|█████▌    | 32/58 [00:07<00:02, 10.03it/s]Capturing num tokens (num_tokens=384 avail_mem=100.01 GB):  55%|█████▌    | 32/58 [00:07<00:02, 10.03it/s]Capturing num tokens (num_tokens=352 avail_mem=99.37 GB):  55%|█████▌    | 32/58 [00:07<00:02, 10.03it/s] 

    Capturing num tokens (num_tokens=352 avail_mem=99.37 GB):  59%|█████▊    | 34/58 [00:07<00:02,  9.48it/s]Capturing num tokens (num_tokens=320 avail_mem=99.36 GB):  59%|█████▊    | 34/58 [00:07<00:02,  9.48it/s]Capturing num tokens (num_tokens=288 avail_mem=100.12 GB):  59%|█████▊    | 34/58 [00:07<00:02,  9.48it/s]Capturing num tokens (num_tokens=288 avail_mem=100.12 GB):  62%|██████▏   | 36/58 [00:07<00:02,  9.91it/s]Capturing num tokens (num_tokens=256 avail_mem=99.40 GB):  62%|██████▏   | 36/58 [00:07<00:02,  9.91it/s] 

    Capturing num tokens (num_tokens=240 avail_mem=99.40 GB):  62%|██████▏   | 36/58 [00:07<00:02,  9.91it/s]Capturing num tokens (num_tokens=240 avail_mem=99.40 GB):  66%|██████▌   | 38/58 [00:07<00:02,  9.31it/s]Capturing num tokens (num_tokens=224 avail_mem=99.49 GB):  66%|██████▌   | 38/58 [00:07<00:02,  9.31it/s]Capturing num tokens (num_tokens=208 avail_mem=100.09 GB):  66%|██████▌   | 38/58 [00:07<00:02,  9.31it/s]

    Capturing num tokens (num_tokens=208 avail_mem=100.09 GB):  69%|██████▉   | 40/58 [00:07<00:01,  9.79it/s]Capturing num tokens (num_tokens=192 avail_mem=99.43 GB):  69%|██████▉   | 40/58 [00:07<00:01,  9.79it/s] Capturing num tokens (num_tokens=176 avail_mem=99.43 GB):  69%|██████▉   | 40/58 [00:07<00:01,  9.79it/s]Capturing num tokens (num_tokens=176 avail_mem=99.43 GB):  72%|███████▏  | 42/58 [00:08<00:01,  9.99it/s]Capturing num tokens (num_tokens=160 avail_mem=100.08 GB):  72%|███████▏  | 42/58 [00:08<00:01,  9.99it/s]

    Capturing num tokens (num_tokens=144 avail_mem=99.47 GB):  72%|███████▏  | 42/58 [00:08<00:01,  9.99it/s] Capturing num tokens (num_tokens=144 avail_mem=99.47 GB):  76%|███████▌  | 44/58 [00:08<00:01, 10.31it/s]Capturing num tokens (num_tokens=128 avail_mem=99.48 GB):  76%|███████▌  | 44/58 [00:08<00:01, 10.31it/s]

    Capturing num tokens (num_tokens=112 avail_mem=100.09 GB):  76%|███████▌  | 44/58 [00:08<00:01, 10.31it/s]Capturing num tokens (num_tokens=112 avail_mem=100.09 GB):  79%|███████▉  | 46/58 [00:08<00:01, 10.09it/s]Capturing num tokens (num_tokens=96 avail_mem=99.53 GB):  79%|███████▉  | 46/58 [00:08<00:01, 10.09it/s]  Capturing num tokens (num_tokens=80 avail_mem=100.08 GB):  79%|███████▉  | 46/58 [00:08<00:01, 10.09it/s]

    Capturing num tokens (num_tokens=80 avail_mem=100.08 GB):  83%|████████▎ | 48/58 [00:08<00:00, 10.59it/s]Capturing num tokens (num_tokens=64 avail_mem=99.58 GB):  83%|████████▎ | 48/58 [00:08<00:00, 10.59it/s] Capturing num tokens (num_tokens=48 avail_mem=99.58 GB):  83%|████████▎ | 48/58 [00:08<00:00, 10.59it/s]Capturing num tokens (num_tokens=48 avail_mem=99.58 GB):  86%|████████▌ | 50/58 [00:08<00:00, 11.02it/s]Capturing num tokens (num_tokens=32 avail_mem=100.07 GB):  86%|████████▌ | 50/58 [00:08<00:00, 11.02it/s]

    Capturing num tokens (num_tokens=28 avail_mem=99.63 GB):  86%|████████▌ | 50/58 [00:08<00:00, 11.02it/s] Capturing num tokens (num_tokens=28 avail_mem=99.63 GB):  90%|████████▉ | 52/58 [00:08<00:00, 11.42it/s]Capturing num tokens (num_tokens=24 avail_mem=100.10 GB):  90%|████████▉ | 52/58 [00:08<00:00, 11.42it/s]Capturing num tokens (num_tokens=20 avail_mem=100.06 GB):  90%|████████▉ | 52/58 [00:09<00:00, 11.42it/s]

    Capturing num tokens (num_tokens=20 avail_mem=100.06 GB):  93%|█████████▎| 54/58 [00:09<00:00, 12.11it/s]Capturing num tokens (num_tokens=16 avail_mem=99.65 GB):  93%|█████████▎| 54/58 [00:09<00:00, 12.11it/s] Capturing num tokens (num_tokens=12 avail_mem=100.06 GB):  93%|█████████▎| 54/58 [00:09<00:00, 12.11it/s]Capturing num tokens (num_tokens=12 avail_mem=100.06 GB):  97%|█████████▋| 56/58 [00:09<00:00, 12.67it/s]Capturing num tokens (num_tokens=8 avail_mem=99.67 GB):  97%|█████████▋| 56/58 [00:09<00:00, 12.67it/s]  

    Capturing num tokens (num_tokens=4 avail_mem=100.06 GB):  97%|█████████▋| 56/58 [00:09<00:00, 12.67it/s]Capturing num tokens (num_tokens=4 avail_mem=100.06 GB): 100%|██████████| 58/58 [00:09<00:00, 13.41it/s]Capturing num tokens (num_tokens=4 avail_mem=100.06 GB): 100%|██████████| 58/58 [00:09<00:00,  6.20it/s]


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


<strong style='color: #00008B;'>reasoing_content: Okay, so the user is asking for the current date and time in New York and the weather there. I need to figure out how to get this information using the functions provided. <br><br>First, I should use the get_current_date function. It requires a timezone parameter. The user mentioned New York, so I need to figure out the correct timezone abbreviation. I think it's 'America/New_York', so I'll include that in the parameters.<br><br>Next, for the weather, I should use get_current_weather. The function needs a city, state, and unit. The city is New York, but I need the state abbreviation. New York is NY, so state will be 'NY'. The unit should be in Fahrenheit since the user didn't specify, so I'll set unit to 'fahrenheit'.<br><br>I need to make sure each function call is separate and follows the required format. So I'll call get_current_date first with the timezone parameter, then get_current_weather with city, state, and unit parameters. <br><br>I should also mention the sources in the response, so I'll add a note about using the US气象局 and time.gov for the information. <br><br>Putting it all together, I'll structure the response with each function call on separate lines and include the sources at the end.<br><br><br>content: <function=get_current_date>{"timezone": "America/New_York"}</function>  <br><function=get_current_weather>{"city": "New York", "state": "NY", "unit": "fahrenheit"}</function>  <br><br>Sources:  <br>- Current date and time: time.gov  <br>- Current weather: US气象局</strong>


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

    {'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. I know that Paris is the capital, so that\'s straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I\'m not sure of the exact number. I think it\'s around 2 million, but I should verify that.\n\nI\'ll check a reliable source, maybe the official Paris Municipality website or a recent census. Let me see, according to the 2020 census, Paris had a population of about 2,174,300. That seems accurate. I should make sure to include this number in the JSON.\n\nNext, I need to structure this information into a JSON format. The user wants a JSON, so I\'ll create an object with a "name" field for the city, a "population" field, and maybe a "description" for more context. The description can mention that Paris is the capital and a major city in France.\n\nI should also consider the format. The JSON should be properly formatted with keys and values, and each key should be a string. The population number should be an integer since it\'s a count of people.\n\nPutting it all together, the JSON will have the city name, population, and a brief description. I\'ll make sure the syntax is correct, with commas and brackets in the right places to avoid errors.\n\nFinally, I\'ll present the JSON to the user, keeping it simple and clear. I don\'t need to add extra information unless the user asks for it, so I\'ll stick to the basics they requested.\n</think>{\n\n"name": "Paris",\n"population": 2174300000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1265, 1477, 279, 1429, 3213, 7042, 821, 13, 358, 6099, 429, 279, 7042, 315, 12095, 702, 1012, 7826, 11, 714, 358, 2776, 537, 2704, 315, 279, 4734, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 10146, 429, 382, 40, 3278, 1779, 264, 14720, 2530, 11, 7196, 279, 3946, 12095, 35703, 2719, 3910, 476, 264, 3213, 43602, 13, 6771, 752, 1490, 11, 4092, 311, 279, 220, 17, 15, 17, 15, 43602, 11, 12095, 1030, 264, 7042, 315, 911, 220, 17, 11, 16, 22, 19, 11, 18, 15, 15, 13, 2938, 4977, 13382, 13, 358, 1265, 1281, 2704, 311, 2924, 419, 1372, 304, 279, 4718, 382, 5847, 11, 358, 1184, 311, 5944, 419, 1995, 1119, 264, 4718, 3561, 13, 576, 1196, 6801, 264, 4718, 11, 773, 358, 3278, 1855, 458, 1633, 448, 264, 330, 606, 1, 2070, 369, 279, 3283, 11, 264, 330, 44441, 1, 2070, 11, 323, 7196, 264, 330, 4684, 1, 369, 803, 2266, 13, 576, 4008, 646, 6286, 429, 12095, 374, 279, 6722, 323, 264, 3598, 3283, 304, 9625, 382, 40, 1265, 1083, 2908, 279, 3561, 13, 576, 4718, 1265, 387, 10277, 23126, 448, 6894, 323, 2750, 11, 323, 1817, 1376, 1265, 387, 264, 914, 13, 576, 7042, 1372, 1265, 387, 458, 7546, 2474, 432, 594, 264, 1760, 315, 1251, 382, 97904, 432, 678, 3786, 11, 279, 4718, 686, 614, 279, 3283, 829, 11, 7042, 11, 323, 264, 9814, 4008, 13, 358, 3278, 1281, 2704, 279, 19482, 374, 4396, 11, 448, 76602, 323, 38929, 304, 279, 1290, 7482, 311, 5648, 5975, 382, 23949, 11, 358, 3278, 3042, 279, 4718, 311, 279, 1196, 11, 10282, 432, 4285, 323, 2797, 13, 358, 1513, 944, 1184, 311, 912, 4960, 1995, 7241, 279, 1196, 17064, 369, 432, 11, 773, 358, 3278, 9214, 311, 279, 31774, 807, 11223, 624, 151649, 4257, 1, 606, 788, 330, 59604, 756, 1, 44441, 788, 220, 17, 16, 22, 19, 18, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15], 'meta_info': {'id': '50305a4eea7f4ffb8359be99a65d65bc', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 23.349081987980753, 'response_sent_to_client_ts': 1774327022.752554}}



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


<strong style='color: #00008B;'>{'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down. First, I need to identify what the capital of France is. I know that Paris is the capital, so that\'s the starting point.\n\nNext, I need to find the population of Paris. I remember that Paris is a major city with a large population, but I\'m not exactly sure of the current number. I think it\'s around 2 million, but I should double-check that. Maybe I can recall that it\'s approximately 2,150,000 as of recent estimates.\n\nNow, the user wants this information in JSON format. JSON stands for JavaScript Object Notation, which is a way to structure data. I need to create a JSON object that includes the key "capital" with the value "Paris" and another key "population" with the number I just thought of.\n\nI should make sure the JSON syntax is correct. That means using double quotes for keys and string values, and commas appropriately between key-value pairs. Also, the numbers should be in quotes if they\'re strings, but population is a number, so it should be without quotes.\n\nPutting it all together, the JSON object should look like this: {"capital": "Paris", "population": 2150000}. I should present this clearly so the user can easily understand and use the information.\n\nI wonder if the user needs more details, like the population figure\'s source or the exact year it was recorded. But since they didn\'t ask for that, I\'ll stick to the information requested. Maybe they just need a straightforward data structure for a program or a report.\n\nAlso, considering the user\'s request, they might be a student working on a project, or perhaps someone developing an app that requires geographical data. Either way, providing the information in a structured format like JSON would be helpful for their purposes.\n\nI should make sure there are no typos in the JSON to avoid any issues when the user tries to use it. Double-checking the spelling of "capital" and "population" is important. Also, ensuring that the commas are correctly placed between the key-value pairs.\n\nIn summary, the user needs the capital of France and its population in JSON. I\'ve identified Paris as the capital and approximately 2,150,000 as the population. Structuring this into a JSON object with appropriate keys and correct syntax should meet their needs.\n</think>{\n\n"name": "Paris",\n"population": 2150000}', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 13, 5512, 11, 358, 1184, 311, 10542, 1128, 279, 6722, 315, 9625, 374, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 279, 5916, 1459, 382, 5847, 11, 358, 1184, 311, 1477, 279, 7042, 315, 12095, 13, 358, 6099, 429, 12095, 374, 264, 3598, 3283, 448, 264, 3460, 7042, 11, 714, 358, 2776, 537, 6896, 2704, 315, 279, 1482, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 1990, 15934, 429, 13, 10696, 358, 646, 19091, 429, 432, 594, 13187, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 438, 315, 3213, 17530, 382, 7039, 11, 279, 1196, 6801, 419, 1995, 304, 4718, 3561, 13, 4718, 13352, 369, 12914, 3002, 2806, 367, 11, 892, 374, 264, 1616, 311, 5944, 821, 13, 358, 1184, 311, 1855, 264, 4718, 1633, 429, 5646, 279, 1376, 330, 65063, 1, 448, 279, 897, 330, 59604, 1, 323, 2441, 1376, 330, 44441, 1, 448, 279, 1372, 358, 1101, 3381, 315, 382, 40, 1265, 1281, 2704, 279, 4718, 19482, 374, 4396, 13, 2938, 3363, 1667, 1990, 17194, 369, 6894, 323, 914, 2750, 11, 323, 76602, 34901, 1948, 1376, 19083, 13530, 13, 7281, 11, 279, 5109, 1265, 387, 304, 17194, 421, 807, 2299, 9069, 11, 714, 7042, 374, 264, 1372, 11, 773, 432, 1265, 387, 2041, 17194, 382, 97904, 432, 678, 3786, 11, 279, 4718, 1633, 1265, 1401, 1075, 419, 25, 5212, 65063, 788, 330, 59604, 497, 330, 44441, 788, 220, 17, 16, 20, 15, 15, 15, 15, 7810, 358, 1265, 3042, 419, 9355, 773, 279, 1196, 646, 6707, 3535, 323, 990, 279, 1995, 382, 40, 5775, 421, 279, 1196, 3880, 803, 3565, 11, 1075, 279, 7042, 7071, 594, 2530, 476, 279, 4734, 1042, 432, 572, 12433, 13, 1988, 2474, 807, 3207, 944, 2548, 369, 429, 11, 358, 3278, 9214, 311, 279, 1995, 11223, 13, 10696, 807, 1101, 1184, 264, 30339, 821, 5944, 369, 264, 2025, 476, 264, 1895, 382, 13394, 11, 12831, 279, 1196, 594, 1681, 11, 807, 2578, 387, 264, 5458, 3238, 389, 264, 2390, 11, 476, 8365, 4325, 11220, 458, 906, 429, 7460, 52901, 821, 13, 20988, 1616, 11, 8241, 279, 1995, 304, 264, 32930, 3561, 1075, 4718, 1035, 387, 10950, 369, 862, 9895, 382, 40, 1265, 1281, 2704, 1052, 525, 902, 13580, 966, 304, 279, 4718, 311, 5648, 894, 4714, 979, 279, 1196, 16297, 311, 990, 432, 13, 7093, 15934, 287, 279, 42429, 315, 330, 65063, 1, 323, 330, 44441, 1, 374, 2989, 13, 7281, 11, 22573, 429, 279, 76602, 525, 12440, 9099, 1948, 279, 1376, 19083, 13530, 382, 641, 12126, 11, 279, 1196, 3880, 279, 6722, 315, 9625, 323, 1181, 7042, 304, 4718, 13, 358, 3003, 10820, 12095, 438, 279, 6722, 323, 13187, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 438, 279, 7042, 13, 16139, 1677, 419, 1119, 264, 4718, 1633, 448, 8311, 6894, 323, 4396, 19482, 1265, 3367, 862, 3880, 624, 151649, 4257, 1, 606, 788, 330, 59604, 756, 1, 44441, 788, 220, 17, 16, 20, 15, 15, 15, 15, 92, 151643], 'meta_info': {'id': 'c7b7b41c00f3464eb64760c8e9701b32', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 528, 'cached_tokens': 22, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 5.83818327030167, 'response_sent_to_client_ts': 1774327028.6057806}}</strong>


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

    [{'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '21129d3aa7274444959c9adb4c8fb7f4', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.11490689078345895, 'response_sent_to_client_ts': 1774327028.7583416}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '6b0fe8bf028c469fb602bb5da38b920d', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.1148174786940217, 'response_sent_to_client_ts': 1774327028.7583559}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': 'ac12ce0400b6478c951fe7749b7e728f', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.11477246088907123, 'response_sent_to_client_ts': 1774327028.7583616}}]


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

    {'text': ' France, and the \n\\( n \\)  \\( m \\) \\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\)', 'output_ids': [9625, 11, 323, 279, 220, 198, 44292, 308, 1124, 8, 220, 17767, 296, 1124, 8, 17767, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8], 'meta_info': {'id': '03e1cf01e4c94b05a65c8a298c098353', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 6, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 18.961569098290056, 'response_sent_to_client_ts': 1774327047.7278404}}


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


<strong style='color: #00008B;'>{'text': 'Alright, the user asked for the population of the capital of France. First, I need to figure out what the capital is. From what I know, the capital of France is Paris, so that\'s the city we\'re focusing on.\n\nNow, I need to find the current population of Paris. I should check reliable sources like the latest census or recent estimates. I recall that Paris is one of the largest cities in the world, so it\'s likely to have a population well over 30 million.\n\nI think the most recent data would be from 2021 or 2022, as those are usually up to date. So, I\'ll look up the exact figure. From my knowledge, Paris has a population of approximately 3,760,000 people in 2022.\n\nIn the response, I should structure this information clearly in JSON format. The user specifically asked for JSON, so it\'s important to format it correctly. I\'ll create a JSON object with the key "capital" and its value as Paris, followed by another object with "population" and its corresponding number.\n\nI should make sure the numbers are accurate and present them in a readable format. Also, I should keep the response concise and straightforward since the user didn\'t specify needing any additional information or explanations beyond the data.\n\nSo, to summarize, I need to provide a JSON response with the capital name as Paris and its population as 3,760,000, formatted correctly without any extra fluff.\n</think>\n\n```json\n{\n  "capital": "Paris",\n  "population": 3760000\n}\n```', 'output_ids': [71486, 11, 279, 1196, 4588, 369, 279, 7042, 315, 279, 6722, 315, 9625, 13, 5512, 11, 358, 1184, 311, 7071, 700, 1128, 279, 6722, 374, 13, 5542, 1128, 358, 1414, 11, 279, 6722, 315, 9625, 374, 12095, 11, 773, 429, 594, 279, 3283, 582, 2299, 21080, 389, 382, 7039, 11, 358, 1184, 311, 1477, 279, 1482, 7042, 315, 12095, 13, 358, 1265, 1779, 14720, 8173, 1075, 279, 5535, 43602, 476, 3213, 17530, 13, 358, 19091, 429, 12095, 374, 825, 315, 279, 7772, 9720, 304, 279, 1879, 11, 773, 432, 594, 4363, 311, 614, 264, 7042, 1632, 916, 220, 18, 15, 3526, 382, 40, 1744, 279, 1429, 3213, 821, 1035, 387, 504, 220, 17, 15, 17, 16, 476, 220, 17, 15, 17, 17, 11, 438, 1846, 525, 5990, 705, 311, 2400, 13, 2055, 11, 358, 3278, 1401, 705, 279, 4734, 7071, 13, 5542, 847, 6540, 11, 12095, 702, 264, 7042, 315, 13187, 220, 18, 11, 22, 21, 15, 11, 15, 15, 15, 1251, 304, 220, 17, 15, 17, 17, 382, 641, 279, 2033, 11, 358, 1265, 5944, 419, 1995, 9355, 304, 4718, 3561, 13, 576, 1196, 11689, 4588, 369, 4718, 11, 773, 432, 594, 2989, 311, 3561, 432, 12440, 13, 358, 3278, 1855, 264, 4718, 1633, 448, 279, 1376, 330, 65063, 1, 323, 1181, 897, 438, 12095, 11, 8110, 553, 2441, 1633, 448, 330, 44441, 1, 323, 1181, 12159, 1372, 382, 40, 1265, 1281, 2704, 279, 5109, 525, 13382, 323, 3042, 1105, 304, 264, 33798, 3561, 13, 7281, 11, 358, 1265, 2506, 279, 2033, 63594, 323, 30339, 2474, 279, 1196, 3207, 944, 13837, 32821, 894, 5107, 1995, 476, 40841, 7797, 279, 821, 382, 4416, 11, 311, 62079, 11, 358, 1184, 311, 3410, 264, 4718, 2033, 448, 279, 6722, 829, 438, 12095, 323, 1181, 7042, 438, 220, 18, 11, 22, 21, 15, 11, 15, 15, 15, 11, 23126, 12440, 2041, 894, 4960, 1320, 1362, 624, 151649, 271, 73594, 2236, 198, 515, 220, 330, 65063, 788, 330, 59604, 756, 220, 330, 44441, 788, 220, 18, 22, 21, 15, 15, 15, 15, 198, 532, 73594, 151643], 'meta_info': {'id': '19958a1c808d4f8fab809441d2c6d2a4', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 342, 'cached_tokens': 22, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 3.2102518579922616, 'response_sent_to_client_ts': 1774327050.9480927}}</strong>



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

    [2026-03-24 04:37:33] INFO server_args.py:2240: Attention backend not specified. Use fa3 backend by default.


    [2026-03-24 04:37:33] INFO server_args.py:3523: Set soft_watchdog_timeout since in CI


    [2026-03-24 04:37:33] INFO engine.py:177: server_args=ServerArgs(model_path='deepseek-ai/DeepSeek-R1-Distill-Qwen-7B', tokenizer_path='deepseek-ai/DeepSeek-R1-Distill-Qwen-7B', tokenizer_mode='auto', tokenizer_worker_num=1, skip_tokenizer_init=False, load_format='auto', model_loader_extra_config='{}', trust_remote_code=False, context_length=None, is_embedding=False, enable_multimodal=None, revision=None, model_impl='auto', host='127.0.0.1', port=30000, fastapi_root_path='', grpc_mode=False, skip_server_warmup=False, warmups=None, nccl_port=None, checkpoint_engine_wait_weights_before_ready=False, ssl_keyfile=None, ssl_certfile=None, ssl_ca_certs=None, ssl_keyfile_password=None, enable_ssl_refresh=False, dtype='auto', quantization=None, quantization_param_path=None, kv_cache_dtype='auto', enable_fp32_lm_head=False, modelopt_quant=None, modelopt_checkpoint_restore_path=None, modelopt_checkpoint_save_path=None, modelopt_export_path=None, quantize_and_serve=False, rl_quant_profile=None, mem_fraction_static=0.903, max_running_requests=128, max_queued_requests=None, max_total_tokens=20480, chunked_prefill_size=8192, enable_dynamic_chunking=False, max_prefill_tokens=16384, prefill_max_requests=None, schedule_policy='fcfs', enable_priority_scheduling=False, disable_priority_preemption=False, default_priority_value=None, abort_on_priority_when_disabled=False, schedule_low_priority_values_first=False, priority_scheduling_preemption_threshold=10, schedule_conservativeness=1.0, page_size=1, swa_full_tokens_ratio=0.8, disable_hybrid_swa_memory=False, radix_eviction_policy='lru', enable_prefill_delayer=False, prefill_delayer_max_delay_passes=30, prefill_delayer_token_usage_low_watermark=None, prefill_delayer_forward_passes_buckets=None, prefill_delayer_wait_seconds_buckets=None, device='cuda', tp_size=1, pp_size=1, pp_max_micro_batch_size=None, pp_async_batch_depth=0, stream_interval=1, incremental_streaming_output=False, enable_streaming_session=False, random_seed=710366271, constrained_json_whitespace_pattern=None, constrained_json_disable_any_whitespace=False, watchdog_timeout=300, soft_watchdog_timeout=300, dist_timeout=None, download_dir=None, model_checksum=None, base_gpu_id=0, gpu_id_step=1, sleep_on_idle=False, use_ray=False, custom_sigquit_handler=None, log_level='error', log_level_http=None, log_requests=False, log_requests_level=2, log_requests_format='text', log_requests_target=None, uvicorn_access_log_exclude_prefixes=[], crash_dump_folder=None, show_time_cost=False, enable_metrics=False, enable_metrics_for_all_schedulers=False, tokenizer_metrics_custom_labels_header='x-custom-labels', tokenizer_metrics_allowed_custom_labels=None, extra_metric_labels=None, bucket_time_to_first_token=None, bucket_inter_token_latency=None, bucket_e2e_request_latency=None, collect_tokens_histogram=False, prompt_tokens_buckets=None, generation_tokens_buckets=None, gc_warning_threshold_secs=0.0, decode_log_interval=40, enable_request_time_stats_logging=False, kv_events_config=None, enable_trace=False, otlp_traces_endpoint='localhost:4317', export_metrics_to_file=False, export_metrics_to_file_dir=None, api_key=None, admin_api_key=None, served_model_name='deepseek-ai/DeepSeek-R1-Distill-Qwen-7B', weight_version='default', chat_template=None, hf_chat_template_name=None, completion_template=None, file_storage_path='sglang_storage', enable_cache_report=False, reasoning_parser='deepseek-r1', tool_call_parser=None, tool_server=None, sampling_defaults='model', dp_size=1, load_balance_method='round_robin', attn_cp_size=1, moe_dp_size=1, dist_init_addr=None, nnodes=1, node_rank=0, json_model_override_args='{}', preferred_sampling_params=None, enable_lora=None, enable_lora_overlap_loading=None, max_lora_rank=None, lora_target_modules=None, lora_paths=None, max_loaded_loras=None, max_loras_per_batch=8, lora_eviction_policy='lru', lora_backend='csgmv', max_lora_chunk_size=16, attention_backend='fa3', decode_attention_backend=None, prefill_attention_backend=None, sampling_backend='flashinfer', grammar_backend='xgrammar', mm_attention_backend=None, fp8_gemm_runner_backend='auto', fp4_gemm_runner_backend='auto', nsa_prefill_backend=None, nsa_decode_backend=None, disable_flashinfer_autotune=False, mamba_backend='triton', speculative_algorithm=None, speculative_draft_model_path=None, speculative_draft_model_revision=None, speculative_draft_load_format=None, speculative_num_steps=None, speculative_eagle_topk=None, speculative_num_draft_tokens=None, speculative_accept_threshold_single=1.0, speculative_accept_threshold_acc=1.0, speculative_token_map=None, speculative_attention_mode='prefill', speculative_draft_attention_backend=None, speculative_moe_runner_backend='auto', speculative_moe_a2a_backend=None, speculative_draft_model_quantization=None, speculative_ngram_min_match_window_size=1, speculative_ngram_max_match_window_size=12, speculative_ngram_min_bfs_breadth=1, speculative_ngram_max_bfs_breadth=10, speculative_ngram_match_type='BFS', speculative_ngram_max_trie_depth=18, speculative_ngram_capacity=10000000, enable_multi_layer_eagle=False, ep_size=1, moe_a2a_backend='none', moe_runner_backend='auto', flashinfer_mxfp4_moe_precision='default', enable_flashinfer_allreduce_fusion=False, enable_aiter_allreduce_fusion=False, deepep_mode='auto', ep_num_redundant_experts=0, ep_dispatch_algorithm=None, init_expert_location='trivial', enable_eplb=False, eplb_algorithm='auto', eplb_rebalance_num_iterations=1000, eplb_rebalance_layers_per_chunk=None, eplb_min_rebalancing_utilization_threshold=1.0, expert_distribution_recorder_mode=None, expert_distribution_recorder_buffer_size=1000, enable_expert_distribution_metrics=False, deepep_config=None, moe_dense_tp_size=None, elastic_ep_backend=None, enable_elastic_expert_backup=False, mooncake_ib_device=None, max_mamba_cache_size=None, mamba_ssm_dtype=None, mamba_full_memory_ratio=0.9, mamba_scheduler_strategy='no_buffer', mamba_track_interval=256, linear_attn_backend='triton', linear_attn_decode_backend=None, linear_attn_prefill_backend=None, enable_hierarchical_cache=False, hicache_ratio=2.0, hicache_size=0, hicache_write_policy='write_through', hicache_io_backend='kernel', hicache_mem_layout='layer_first', disable_hicache_numa_detect=False, hicache_storage_backend=None, hicache_storage_prefetch_policy='best_effort', hicache_storage_backend_extra_config=None, enable_hisparse=False, hisparse_config=None, enable_lmcache=False, kt_weight_path=None, kt_method=None, kt_cpuinfer=None, kt_threadpool_count=None, kt_num_gpu_experts=None, kt_max_deferred_experts_per_token=None, dllm_algorithm=None, dllm_algorithm_config=None, enable_double_sparsity=False, ds_channel_config_path=None, ds_heavy_channel_num=32, ds_heavy_token_num=256, ds_heavy_channel_type='qk', ds_sparse_decode_threshold=4096, cpu_offload_gb=0, offload_group_size=-1, offload_num_in_group=1, offload_prefetch_step=1, offload_mode='cpu', multi_item_scoring_delimiter=None, disable_radix_cache=False, cuda_graph_max_bs=4, cuda_graph_bs=[1, 2, 4, 8, 12, 16, 24, 32, 40, 48, 56, 64, 72, 80, 88, 96, 104, 112, 120, 128, 136, 144, 152, 160, 168, 176, 184, 192, 200, 208, 216, 224, 232, 240, 248, 256], disable_cuda_graph=True, disable_cuda_graph_padding=False, enable_profile_cuda_graph=False, enable_cudagraph_gc=False, enable_layerwise_nvtx_marker=False, enable_nccl_nvls=False, enable_symm_mem=False, disable_flashinfer_cutlass_moe_fp4_allgather=False, enable_tokenizer_batch_encode=False, disable_tokenizer_batch_decode=False, disable_outlines_disk_cache=False, disable_custom_all_reduce=False, enable_mscclpp=False, enable_torch_symm_mem=False, pre_warm_nccl=False, disable_overlap_schedule=False, enable_mixed_chunk=False, enable_dp_attention=False, enable_dp_lm_head=False, enable_two_batch_overlap=False, enable_single_batch_overlap=False, tbo_token_distribution_threshold=0.48, enable_torch_compile=False, disable_piecewise_cuda_graph=False, enforce_piecewise_cuda_graph=False, enable_torch_compile_debug_mode=False, torch_compile_max_bs=32, piecewise_cuda_graph_max_tokens=8192, piecewise_cuda_graph_tokens=[4, 8, 12, 16, 20, 24, 28, 32, 48, 64, 80, 96, 112, 128, 144, 160, 176, 192, 208, 224, 240, 256, 288, 320, 352, 384, 416, 448, 480, 512, 576, 640, 704, 768, 832, 896, 960, 1024, 1280, 1536, 1792, 2048, 2304, 2560, 2816, 3072, 3328, 3584, 3840, 4096, 4608, 5120, 5632, 6144, 6656, 7168, 7680, 8192], piecewise_cuda_graph_compiler='eager', torchao_config='', enable_nan_detection=False, enable_p2p_check=False, triton_attention_reduce_in_fp32=False, triton_attention_num_kv_splits=8, triton_attention_split_tile_size=None, num_continuous_decode_steps=1, delete_ckpt_after_loading=False, enable_memory_saver=False, enable_weights_cpu_backup=False, enable_draft_weights_cpu_backup=False, allow_auto_truncate=False, enable_custom_logit_processor=False, flashinfer_mla_disable_ragged=False, disable_shared_experts_fusion=False, disable_chunked_prefix_cache=False, disable_fast_image_processor=False, keep_mm_feature_on_device=False, enable_return_hidden_states=False, enable_return_routed_experts=False, scheduler_recv_interval=1, numa_node=None, enable_deterministic_inference=False, rl_on_policy_target=None, enable_attn_tp_input_scattered=False, enable_nsa_prefill_context_parallel=False, nsa_prefill_cp_mode='round-robin-split', enable_fused_qk_norm_rope=False, enable_precise_embedding_interpolation=False, enable_fused_moe_sum_all_reduce=False, enable_prefill_context_parallel=False, prefill_cp_mode='in-seq-split', enable_dynamic_batch_tokenizer=False, dynamic_batch_tokenizer_batch_size=32, dynamic_batch_tokenizer_batch_timeout=0.002, debug_tensor_dump_output_folder=None, debug_tensor_dump_layers=None, debug_tensor_dump_input_file=None, debug_tensor_dump_inject=False, disaggregation_mode='null', disaggregation_transfer_backend='mooncake', disaggregation_bootstrap_port=8998, disaggregation_ib_device=None, disaggregation_decode_enable_offload_kvcache=False, num_reserved_decode_tokens=512, disaggregation_decode_polling_interval=1, encoder_only=False, language_only=False, encoder_transfer_backend='zmq_to_scheduler', encoder_urls=[], enable_adaptive_dispatch_to_encoder=False, custom_weight_loader=[], weight_loader_disable_mmap=False, remote_instance_weight_loader_seed_instance_ip=None, remote_instance_weight_loader_seed_instance_service_port=None, remote_instance_weight_loader_send_weights_group_ports=None, remote_instance_weight_loader_backend='nccl', remote_instance_weight_loader_start_seed_via_transfer_engine=False, modelexpress_config=None, enable_pdmux=False, pdmux_config_path=None, sm_group_num=8, mm_max_concurrent_calls=32, mm_per_request_timeout=10.0, enable_broadcast_mm_inputs_process=False, enable_prefix_mm_cache=False, mm_enable_dp_encoder=False, mm_process_config={}, limit_mm_data_per_request=None, enable_mm_global_cache=False, decrypted_config_file=None, decrypted_draft_config_file=None, forward_hooks=None)


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-03-24 04:37:43.699 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 04:37:43] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 04:37:43.699 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 04:37:43] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 04:37:43.699 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 04:37:43] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 04:37:43.699 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 04:37:43] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 04:37:43.699 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 04:37:43] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:00<00:00,  1.06it/s]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.33s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.27s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:07,  3.28s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:07,  3.28s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:28,  1.58s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:28,  1.58s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:51,  1.06it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:51,  1.06it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:34,  1.56it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:34,  1.56it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:24,  2.15it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:24,  2.15it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.78it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.78it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.53it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.53it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.31it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.31it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.19it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.19it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.19it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:06,  6.90it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:06,  6.90it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:06,  6.90it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:05<00:05,  8.41it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:05<00:05,  8.41it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:05<00:05,  8.19it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:05<00:05,  8.19it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:05<00:05,  7.87it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:05<00:05,  7.87it/s]

    Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:05<00:05,  7.86it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:05<00:05,  7.86it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:05,  8.01it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:05,  8.01it/s]

    Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:05<00:04,  8.39it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:05<00:04,  8.39it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:05<00:04,  8.39it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:04,  9.34it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:04,  9.34it/s]

    Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:04,  9.34it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:05<00:03, 11.07it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:05<00:03, 11.07it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:06<00:03, 11.07it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:06<00:02, 12.15it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:06<00:02, 12.15it/s]

    Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:06<00:02, 12.15it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:06<00:02, 13.99it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:06<00:02, 13.99it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:06<00:02, 13.99it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:06<00:01, 15.13it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:06<00:01, 15.13it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:06<00:01, 15.13it/s]

    Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:06<00:01, 15.13it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:06<00:01, 17.42it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:06<00:01, 17.42it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:06<00:01, 17.42it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:06<00:01, 17.42it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:06<00:01, 19.10it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:06<00:01, 19.10it/s]

    Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:06<00:01, 19.10it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:06<00:01, 19.10it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:06<00:01, 20.13it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:06<00:01, 20.13it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:06<00:01, 20.13it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:06<00:01, 20.13it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:06<00:00, 22.10it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:06<00:00, 22.10it/s]

    Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:06<00:00, 22.10it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:06<00:00, 22.10it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:06<00:00, 22.79it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:06<00:00, 22.79it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:06<00:00, 22.79it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:07<00:00, 22.79it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:07<00:00, 24.06it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:07<00:00, 24.06it/s] 

    Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:07<00:00, 24.06it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:07<00:00, 24.06it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:07<00:00, 25.23it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:07<00:00, 25.23it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:07<00:00, 25.23it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:07<00:00, 25.23it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:07<00:00, 25.23it/s]

    Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:07<00:00, 26.94it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:07<00:00, 26.94it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:07<00:00, 26.94it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:07<00:00, 26.94it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:07<00:00, 26.94it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:07<00:00, 29.22it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:07<00:00, 29.22it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:07<00:00,  7.80it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=104.80 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=104.80 GB):   2%|▏         | 1/58 [00:00<00:34,  1.67it/s]Capturing num tokens (num_tokens=7680 avail_mem=104.77 GB):   2%|▏         | 1/58 [00:00<00:34,  1.67it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=104.77 GB):   3%|▎         | 2/58 [00:01<00:29,  1.92it/s]Capturing num tokens (num_tokens=7168 avail_mem=104.77 GB):   3%|▎         | 2/58 [00:01<00:29,  1.92it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=104.77 GB):   5%|▌         | 3/58 [00:01<00:25,  2.17it/s]Capturing num tokens (num_tokens=6656 avail_mem=104.77 GB):   5%|▌         | 3/58 [00:01<00:25,  2.17it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=104.77 GB):   7%|▋         | 4/58 [00:01<00:21,  2.54it/s]Capturing num tokens (num_tokens=6144 avail_mem=104.77 GB):   7%|▋         | 4/58 [00:01<00:21,  2.54it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=104.77 GB):   9%|▊         | 5/58 [00:02<00:18,  2.85it/s]Capturing num tokens (num_tokens=5632 avail_mem=104.77 GB):   9%|▊         | 5/58 [00:02<00:18,  2.85it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=104.77 GB):  10%|█         | 6/58 [00:02<00:16,  3.24it/s]Capturing num tokens (num_tokens=5120 avail_mem=104.77 GB):  10%|█         | 6/58 [00:02<00:16,  3.24it/s]Capturing num tokens (num_tokens=5120 avail_mem=104.77 GB):  12%|█▏        | 7/58 [00:02<00:13,  3.83it/s]Capturing num tokens (num_tokens=4608 avail_mem=104.78 GB):  12%|█▏        | 7/58 [00:02<00:13,  3.83it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=104.78 GB):  14%|█▍        | 8/58 [00:02<00:11,  4.46it/s]Capturing num tokens (num_tokens=4096 avail_mem=104.78 GB):  14%|█▍        | 8/58 [00:02<00:11,  4.46it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=104.78 GB):  16%|█▌        | 9/58 [00:02<00:11,  4.18it/s]Capturing num tokens (num_tokens=3840 avail_mem=103.60 GB):  16%|█▌        | 9/58 [00:02<00:11,  4.18it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=103.60 GB):  17%|█▋        | 10/58 [00:03<00:11,  4.06it/s]Capturing num tokens (num_tokens=3584 avail_mem=104.75 GB):  17%|█▋        | 10/58 [00:03<00:11,  4.06it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=104.75 GB):  19%|█▉        | 11/58 [00:03<00:11,  4.03it/s]Capturing num tokens (num_tokens=3328 avail_mem=103.77 GB):  19%|█▉        | 11/58 [00:03<00:11,  4.03it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=103.77 GB):  21%|██        | 12/58 [00:03<00:11,  4.04it/s]Capturing num tokens (num_tokens=3072 avail_mem=104.75 GB):  21%|██        | 12/58 [00:03<00:11,  4.04it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=104.75 GB):  22%|██▏       | 13/58 [00:03<00:11,  4.08it/s]Capturing num tokens (num_tokens=2816 avail_mem=103.83 GB):  22%|██▏       | 13/58 [00:03<00:11,  4.08it/s]Capturing num tokens (num_tokens=2816 avail_mem=103.83 GB):  24%|██▍       | 14/58 [00:04<00:10,  4.33it/s]Capturing num tokens (num_tokens=2560 avail_mem=103.90 GB):  24%|██▍       | 14/58 [00:04<00:10,  4.33it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=103.90 GB):  26%|██▌       | 15/58 [00:04<00:09,  4.42it/s]Capturing num tokens (num_tokens=2304 avail_mem=103.90 GB):  26%|██▌       | 15/58 [00:04<00:09,  4.42it/s]Capturing num tokens (num_tokens=2304 avail_mem=103.90 GB):  28%|██▊       | 16/58 [00:04<00:08,  4.81it/s]Capturing num tokens (num_tokens=2048 avail_mem=103.97 GB):  28%|██▊       | 16/58 [00:04<00:08,  4.81it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=103.97 GB):  29%|██▉       | 17/58 [00:04<00:08,  4.95it/s]Capturing num tokens (num_tokens=1792 avail_mem=103.97 GB):  29%|██▉       | 17/58 [00:04<00:08,  4.95it/s]Capturing num tokens (num_tokens=1792 avail_mem=103.97 GB):  31%|███       | 18/58 [00:04<00:07,  5.55it/s]Capturing num tokens (num_tokens=1536 avail_mem=104.03 GB):  31%|███       | 18/58 [00:04<00:07,  5.55it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=104.03 GB):  33%|███▎      | 19/58 [00:04<00:06,  5.81it/s]Capturing num tokens (num_tokens=1280 avail_mem=104.84 GB):  33%|███▎      | 19/58 [00:04<00:06,  5.81it/s]Capturing num tokens (num_tokens=1024 avail_mem=104.76 GB):  33%|███▎      | 19/58 [00:04<00:06,  5.81it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=104.76 GB):  36%|███▌      | 21/58 [00:05<00:06,  6.04it/s]Capturing num tokens (num_tokens=960 avail_mem=104.76 GB):  36%|███▌      | 21/58 [00:05<00:06,  6.04it/s] Capturing num tokens (num_tokens=960 avail_mem=104.76 GB):  38%|███▊      | 22/58 [00:05<00:05,  6.56it/s]Capturing num tokens (num_tokens=896 avail_mem=104.16 GB):  38%|███▊      | 22/58 [00:05<00:05,  6.56it/s]

    Capturing num tokens (num_tokens=832 avail_mem=104.76 GB):  38%|███▊      | 22/58 [00:05<00:05,  6.56it/s]Capturing num tokens (num_tokens=832 avail_mem=104.76 GB):  41%|████▏     | 24/58 [00:05<00:04,  7.56it/s]Capturing num tokens (num_tokens=768 avail_mem=104.23 GB):  41%|████▏     | 24/58 [00:05<00:04,  7.56it/s]Capturing num tokens (num_tokens=704 avail_mem=104.75 GB):  41%|████▏     | 24/58 [00:05<00:04,  7.56it/s]

    Capturing num tokens (num_tokens=704 avail_mem=104.75 GB):  45%|████▍     | 26/58 [00:05<00:03,  8.51it/s]Capturing num tokens (num_tokens=640 avail_mem=104.26 GB):  45%|████▍     | 26/58 [00:05<00:03,  8.51it/s]Capturing num tokens (num_tokens=576 avail_mem=104.75 GB):  45%|████▍     | 26/58 [00:05<00:03,  8.51it/s]Capturing num tokens (num_tokens=576 avail_mem=104.75 GB):  48%|████▊     | 28/58 [00:05<00:03,  9.47it/s]Capturing num tokens (num_tokens=512 avail_mem=104.28 GB):  48%|████▊     | 28/58 [00:05<00:03,  9.47it/s]

    Capturing num tokens (num_tokens=480 avail_mem=104.74 GB):  48%|████▊     | 28/58 [00:05<00:03,  9.47it/s]Capturing num tokens (num_tokens=480 avail_mem=104.74 GB):  52%|█████▏    | 30/58 [00:06<00:02, 10.39it/s]Capturing num tokens (num_tokens=448 avail_mem=104.30 GB):  52%|█████▏    | 30/58 [00:06<00:02, 10.39it/s]Capturing num tokens (num_tokens=416 avail_mem=104.73 GB):  52%|█████▏    | 30/58 [00:06<00:02, 10.39it/s]

    Capturing num tokens (num_tokens=416 avail_mem=104.73 GB):  55%|█████▌    | 32/58 [00:06<00:02, 11.02it/s]Capturing num tokens (num_tokens=384 avail_mem=104.33 GB):  55%|█████▌    | 32/58 [00:06<00:02, 11.02it/s]Capturing num tokens (num_tokens=352 avail_mem=104.72 GB):  55%|█████▌    | 32/58 [00:06<00:02, 11.02it/s]Capturing num tokens (num_tokens=352 avail_mem=104.72 GB):  59%|█████▊    | 34/58 [00:06<00:02, 11.78it/s]Capturing num tokens (num_tokens=320 avail_mem=104.76 GB):  59%|█████▊    | 34/58 [00:06<00:02, 11.78it/s]Capturing num tokens (num_tokens=288 avail_mem=104.38 GB):  59%|█████▊    | 34/58 [00:06<00:02, 11.78it/s]

    Capturing num tokens (num_tokens=288 avail_mem=104.38 GB):  62%|██████▏   | 36/58 [00:06<00:01, 12.90it/s]Capturing num tokens (num_tokens=256 avail_mem=104.71 GB):  62%|██████▏   | 36/58 [00:06<00:01, 12.90it/s]Capturing num tokens (num_tokens=240 avail_mem=104.41 GB):  62%|██████▏   | 36/58 [00:06<00:01, 12.90it/s]Capturing num tokens (num_tokens=240 avail_mem=104.41 GB):  66%|██████▌   | 38/58 [00:06<00:01, 14.04it/s]Capturing num tokens (num_tokens=224 avail_mem=104.71 GB):  66%|██████▌   | 38/58 [00:06<00:01, 14.04it/s]Capturing num tokens (num_tokens=208 avail_mem=104.70 GB):  66%|██████▌   | 38/58 [00:06<00:01, 14.04it/s]

    Capturing num tokens (num_tokens=208 avail_mem=104.70 GB):  69%|██████▉   | 40/58 [00:06<00:01, 15.31it/s]Capturing num tokens (num_tokens=192 avail_mem=104.70 GB):  69%|██████▉   | 40/58 [00:06<00:01, 15.31it/s]Capturing num tokens (num_tokens=176 avail_mem=104.69 GB):  69%|██████▉   | 40/58 [00:06<00:01, 15.31it/s]Capturing num tokens (num_tokens=176 avail_mem=104.69 GB):  72%|███████▏  | 42/58 [00:06<00:01, 15.55it/s]Capturing num tokens (num_tokens=160 avail_mem=104.48 GB):  72%|███████▏  | 42/58 [00:06<00:01, 15.55it/s]Capturing num tokens (num_tokens=144 avail_mem=104.51 GB):  72%|███████▏  | 42/58 [00:06<00:01, 15.55it/s]

    Capturing num tokens (num_tokens=144 avail_mem=104.51 GB):  76%|███████▌  | 44/58 [00:06<00:00, 16.33it/s]Capturing num tokens (num_tokens=128 avail_mem=104.54 GB):  76%|███████▌  | 44/58 [00:06<00:00, 16.33it/s]Capturing num tokens (num_tokens=112 avail_mem=104.56 GB):  76%|███████▌  | 44/58 [00:06<00:00, 16.33it/s]Capturing num tokens (num_tokens=112 avail_mem=104.56 GB):  79%|███████▉  | 46/58 [00:06<00:00, 17.18it/s]Capturing num tokens (num_tokens=96 avail_mem=104.56 GB):  79%|███████▉  | 46/58 [00:06<00:00, 17.18it/s] Capturing num tokens (num_tokens=80 avail_mem=104.56 GB):  79%|███████▉  | 46/58 [00:07<00:00, 17.18it/s]

    Capturing num tokens (num_tokens=80 avail_mem=104.56 GB):  83%|████████▎ | 48/58 [00:07<00:00, 17.61it/s]Capturing num tokens (num_tokens=64 avail_mem=104.66 GB):  83%|████████▎ | 48/58 [00:07<00:00, 17.61it/s]Capturing num tokens (num_tokens=48 avail_mem=104.65 GB):  83%|████████▎ | 48/58 [00:07<00:00, 17.61it/s]Capturing num tokens (num_tokens=32 avail_mem=104.65 GB):  83%|████████▎ | 48/58 [00:07<00:00, 17.61it/s]Capturing num tokens (num_tokens=32 avail_mem=104.65 GB):  88%|████████▊ | 51/58 [00:07<00:00, 19.35it/s]Capturing num tokens (num_tokens=28 avail_mem=104.64 GB):  88%|████████▊ | 51/58 [00:07<00:00, 19.35it/s]Capturing num tokens (num_tokens=24 avail_mem=104.63 GB):  88%|████████▊ | 51/58 [00:07<00:00, 19.35it/s]

    Capturing num tokens (num_tokens=20 avail_mem=104.62 GB):  88%|████████▊ | 51/58 [00:07<00:00, 19.35it/s]Capturing num tokens (num_tokens=20 avail_mem=104.62 GB):  93%|█████████▎| 54/58 [00:07<00:00, 21.11it/s]Capturing num tokens (num_tokens=16 avail_mem=104.62 GB):  93%|█████████▎| 54/58 [00:07<00:00, 21.11it/s]Capturing num tokens (num_tokens=12 avail_mem=104.61 GB):  93%|█████████▎| 54/58 [00:07<00:00, 21.11it/s]Capturing num tokens (num_tokens=8 avail_mem=104.60 GB):  93%|█████████▎| 54/58 [00:07<00:00, 21.11it/s] Capturing num tokens (num_tokens=8 avail_mem=104.60 GB):  98%|█████████▊| 57/58 [00:07<00:00, 22.61it/s]Capturing num tokens (num_tokens=4 avail_mem=104.59 GB):  98%|█████████▊| 57/58 [00:07<00:00, 22.61it/s]Capturing num tokens (num_tokens=4 avail_mem=104.59 GB): 100%|██████████| 58/58 [00:07<00:00,  7.73it/s]


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
    Generated text: Rome is the capital of Italy
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
    
    Generated text: Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down. First, I need to figure out what exactly they're looking for. They want a JSON response, which is a data structure that's commonly used in web applications. JSON stands for JavaScript Object Notation, so it's a way to represent data as plain text.
    
    Now, the capital of France is Paris. That's straightforward. Next, I should include the population. I remember that population numbers can change over time, so I should look for the most recent data. I think as of 2023, Paris has a population around 2.2 million. But I should verify that to make sure it's accurate. Maybe I can recall that Paris is one of the most populous cities in Europe, so that number seems plausible.
    
    I also need to structure this information properly. The user didn't specify what exactly they want in the JSON, but from the example they provided earlier, it seems they expect a key-value pair with "capital" and "population". So I'll include both pieces of information under those keys.
    
    I should make sure the JSON syntax is correct. The keys should be in quotes, and the values as well. The entire structure should be valid JSON, so I'll use double quotes around the keys and ensure there are commas where necessary. Also, the JSON shouldn't have any trailing commas to prevent syntax errors.
    
    Another thing to consider is whether the population number is up to date. Since I'm providing a general answer, I can note that the population might vary slightly depending on the source and the year. But for the sake of simplicity and the user's request, I'll stick with the current number I have.
    
    I should also think about any additional information that might be useful. Maybe including the estimated population growth rate or the city's economic status could add more value, but the user didn't ask for that, so I'll stick to what's requested.
    
    Lastly, I need to present this information clearly and concisely. Using a JSON format with proper indentation makes it easier to read. I'll format it neatly so that the user can easily copy and use the data without issues.
    
    Putting it all together, I'll create a JSON object with "capital" set to "Paris" and "population" set to 2200000. I'll make sure the JSON is correctly formatted and ready for the user to use as needed.
    </think>
    
    ```json
    {
      "capital": "Paris",
      "population": 2200000
    }
    ```



```python
llm.shutdown()
```
