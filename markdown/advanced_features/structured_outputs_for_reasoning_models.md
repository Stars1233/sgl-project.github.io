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


    2026-04-02 00:15:37.128 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 00:15:37] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 00:15:37.128 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 00:15:37] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 00:15:37.128 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 00:15:37] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 00:15:37.128 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 00:15:37] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 00:15:37.128 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 00:15:37] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:00<00:00,  1.44it/s]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:01<00:00,  1.09it/s]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:01<00:00,  1.13it/s]


    2026-04-02 00:15:41,451 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 00:15:41] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:53,  3.04s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:53,  3.04s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:23,  1.48s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:23,  1.48s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:49,  1.12it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:49,  1.12it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:33,  1.63it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:33,  1.63it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:03<00:23,  2.22it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:03<00:23,  2.22it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.86it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.86it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.60it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.60it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.39it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.39it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.23it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.23it/s]

    Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.23it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:06,  6.83it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:06,  6.83it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:06,  6.83it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:04<00:05,  8.35it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:04<00:05,  8.35it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:04<00:05,  8.35it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:04<00:04,  9.93it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:04<00:04,  9.93it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:05<00:04,  9.93it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:03, 11.84it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:03, 11.84it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:03, 11.84it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:05<00:03, 11.84it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 15.06it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 15.06it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 15.06it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:05<00:02, 15.06it/s]

    Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:05<00:02, 15.06it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:05<00:01, 19.91it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:05<00:01, 19.91it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:05<00:01, 19.91it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:05<00:01, 19.91it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:05<00:01, 19.09it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:05<00:01, 19.09it/s]

    Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:05<00:01, 19.09it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:05<00:01, 19.09it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:05<00:01, 19.64it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:05<00:01, 19.64it/s]Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:05<00:01, 19.64it/s]Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:05<00:01, 19.64it/s]

    Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:05<00:01, 20.54it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:05<00:01, 20.54it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:05<00:01, 20.54it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:05<00:01, 20.54it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:05<00:01, 21.29it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:05<00:01, 21.29it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:05<00:01, 21.29it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:05<00:01, 21.29it/s]

    Compiling num tokens (num_tokens=208):  62%|██████▏   | 36/58 [00:05<00:01, 21.29it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:06<00:00, 23.46it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:06<00:00, 23.46it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:06<00:00, 23.46it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:06<00:00, 23.46it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:06<00:00, 23.82it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:06<00:00, 23.82it/s]

    Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:06<00:00, 23.82it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:06<00:00, 23.82it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:06<00:00, 24.82it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:06<00:00, 24.82it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:06<00:00, 24.82it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:06<00:00, 24.82it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:06<00:00, 25.33it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:06<00:00, 25.33it/s]

    Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:06<00:00, 25.33it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:06<00:00, 25.33it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:06<00:00, 25.33it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:06<00:00, 26.87it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:06<00:00, 26.87it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:06<00:00, 26.87it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:06<00:00, 26.87it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:06<00:00, 26.87it/s] 

    Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:06<00:00, 29.30it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:06<00:00, 29.30it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:06<00:00,  8.72it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=86.22 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=86.22 GB):   2%|▏         | 1/58 [00:00<00:37,  1.53it/s]Capturing num tokens (num_tokens=7680 avail_mem=86.19 GB):   2%|▏         | 1/58 [00:00<00:37,  1.53it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=86.19 GB):   3%|▎         | 2/58 [00:01<00:33,  1.67it/s]Capturing num tokens (num_tokens=7168 avail_mem=86.19 GB):   3%|▎         | 2/58 [00:01<00:33,  1.67it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=86.19 GB):   5%|▌         | 3/58 [00:01<00:29,  1.83it/s]Capturing num tokens (num_tokens=6656 avail_mem=86.19 GB):   5%|▌         | 3/58 [00:01<00:29,  1.83it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=86.19 GB):   7%|▋         | 4/58 [00:02<00:31,  1.72it/s]Capturing num tokens (num_tokens=6144 avail_mem=85.19 GB):   7%|▋         | 4/58 [00:02<00:31,  1.72it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=85.19 GB):   9%|▊         | 5/58 [00:02<00:31,  1.70it/s]Capturing num tokens (num_tokens=5632 avail_mem=85.20 GB):   9%|▊         | 5/58 [00:02<00:31,  1.70it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=85.20 GB):  10%|█         | 6/58 [00:03<00:28,  1.83it/s]Capturing num tokens (num_tokens=5120 avail_mem=85.20 GB):  10%|█         | 6/58 [00:03<00:28,  1.83it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=85.20 GB):  12%|█▏        | 7/58 [00:03<00:25,  1.98it/s]Capturing num tokens (num_tokens=4608 avail_mem=85.20 GB):  12%|█▏        | 7/58 [00:03<00:25,  1.98it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=85.20 GB):  14%|█▍        | 8/58 [00:04<00:23,  2.16it/s]Capturing num tokens (num_tokens=4096 avail_mem=85.20 GB):  14%|█▍        | 8/58 [00:04<00:23,  2.16it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=85.20 GB):  16%|█▌        | 9/58 [00:04<00:20,  2.40it/s]Capturing num tokens (num_tokens=3840 avail_mem=85.20 GB):  16%|█▌        | 9/58 [00:04<00:20,  2.40it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=85.20 GB):  17%|█▋        | 10/58 [00:04<00:19,  2.46it/s]Capturing num tokens (num_tokens=3584 avail_mem=84.01 GB):  17%|█▋        | 10/58 [00:04<00:19,  2.46it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=84.01 GB):  19%|█▉        | 11/58 [00:05<00:19,  2.43it/s]Capturing num tokens (num_tokens=3328 avail_mem=85.17 GB):  19%|█▉        | 11/58 [00:05<00:19,  2.43it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=85.17 GB):  21%|██        | 12/58 [00:05<00:18,  2.52it/s]Capturing num tokens (num_tokens=3072 avail_mem=84.18 GB):  21%|██        | 12/58 [00:05<00:18,  2.52it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=84.18 GB):  22%|██▏       | 13/58 [00:06<00:17,  2.53it/s]Capturing num tokens (num_tokens=2816 avail_mem=85.18 GB):  22%|██▏       | 13/58 [00:06<00:17,  2.53it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=85.18 GB):  24%|██▍       | 14/58 [00:06<00:16,  2.66it/s]Capturing num tokens (num_tokens=2560 avail_mem=84.25 GB):  24%|██▍       | 14/58 [00:06<00:16,  2.66it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=84.25 GB):  26%|██▌       | 15/58 [00:06<00:15,  2.75it/s]Capturing num tokens (num_tokens=2304 avail_mem=85.18 GB):  26%|██▌       | 15/58 [00:06<00:15,  2.75it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=85.18 GB):  28%|██▊       | 16/58 [00:07<00:14,  2.90it/s]Capturing num tokens (num_tokens=2048 avail_mem=84.32 GB):  28%|██▊       | 16/58 [00:07<00:14,  2.90it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=84.32 GB):  29%|██▉       | 17/58 [00:07<00:13,  3.08it/s]Capturing num tokens (num_tokens=1792 avail_mem=85.18 GB):  29%|██▉       | 17/58 [00:07<00:13,  3.08it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=85.18 GB):  31%|███       | 18/58 [00:07<00:12,  3.30it/s]Capturing num tokens (num_tokens=1536 avail_mem=84.39 GB):  31%|███       | 18/58 [00:07<00:12,  3.30it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=84.39 GB):  33%|███▎      | 19/58 [00:07<00:11,  3.49it/s]Capturing num tokens (num_tokens=1280 avail_mem=85.18 GB):  33%|███▎      | 19/58 [00:07<00:11,  3.49it/s]Capturing num tokens (num_tokens=1280 avail_mem=85.18 GB):  34%|███▍      | 20/58 [00:08<00:09,  3.89it/s]Capturing num tokens (num_tokens=1024 avail_mem=84.45 GB):  34%|███▍      | 20/58 [00:08<00:09,  3.89it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=84.45 GB):  36%|███▌      | 21/58 [00:08<00:08,  4.14it/s]Capturing num tokens (num_tokens=960 avail_mem=84.45 GB):  36%|███▌      | 21/58 [00:08<00:08,  4.14it/s] Capturing num tokens (num_tokens=960 avail_mem=84.45 GB):  38%|███▊      | 22/58 [00:08<00:07,  4.75it/s]Capturing num tokens (num_tokens=896 avail_mem=84.51 GB):  38%|███▊      | 22/58 [00:08<00:07,  4.75it/s]

    Capturing num tokens (num_tokens=896 avail_mem=84.51 GB):  40%|███▉      | 23/58 [00:08<00:07,  4.95it/s]Capturing num tokens (num_tokens=832 avail_mem=84.51 GB):  40%|███▉      | 23/58 [00:08<00:07,  4.95it/s]Capturing num tokens (num_tokens=832 avail_mem=84.51 GB):  41%|████▏     | 24/58 [00:08<00:06,  5.61it/s]Capturing num tokens (num_tokens=768 avail_mem=85.17 GB):  41%|████▏     | 24/58 [00:08<00:06,  5.61it/s]

    Capturing num tokens (num_tokens=768 avail_mem=85.17 GB):  43%|████▎     | 25/58 [00:08<00:05,  5.77it/s]Capturing num tokens (num_tokens=704 avail_mem=84.57 GB):  43%|████▎     | 25/58 [00:08<00:05,  5.77it/s]Capturing num tokens (num_tokens=640 avail_mem=85.17 GB):  43%|████▎     | 25/58 [00:08<00:05,  5.77it/s]

    Capturing num tokens (num_tokens=640 avail_mem=85.17 GB):  47%|████▋     | 27/58 [00:09<00:04,  6.75it/s]Capturing num tokens (num_tokens=576 avail_mem=84.64 GB):  47%|████▋     | 27/58 [00:09<00:04,  6.75it/s]Capturing num tokens (num_tokens=512 avail_mem=84.80 GB):  47%|████▋     | 27/58 [00:09<00:04,  6.75it/s]

    Capturing num tokens (num_tokens=512 avail_mem=84.80 GB):  50%|█████     | 29/58 [00:09<00:03,  7.49it/s]Capturing num tokens (num_tokens=480 avail_mem=84.66 GB):  50%|█████     | 29/58 [00:09<00:03,  7.49it/s]Capturing num tokens (num_tokens=448 avail_mem=85.15 GB):  50%|█████     | 29/58 [00:09<00:03,  7.49it/s]

    Capturing num tokens (num_tokens=448 avail_mem=85.15 GB):  53%|█████▎    | 31/58 [00:09<00:03,  8.20it/s]Capturing num tokens (num_tokens=416 avail_mem=85.20 GB):  53%|█████▎    | 31/58 [00:09<00:03,  8.20it/s]Capturing num tokens (num_tokens=384 avail_mem=84.71 GB):  53%|█████▎    | 31/58 [00:09<00:03,  8.20it/s]Capturing num tokens (num_tokens=384 avail_mem=84.71 GB):  57%|█████▋    | 33/58 [00:09<00:02,  9.26it/s]Capturing num tokens (num_tokens=352 avail_mem=85.14 GB):  57%|█████▋    | 33/58 [00:09<00:02,  9.26it/s]

    Capturing num tokens (num_tokens=320 avail_mem=84.73 GB):  57%|█████▋    | 33/58 [00:09<00:02,  9.26it/s]Capturing num tokens (num_tokens=320 avail_mem=84.73 GB):  60%|██████    | 35/58 [00:09<00:02, 10.40it/s]Capturing num tokens (num_tokens=288 avail_mem=85.13 GB):  60%|██████    | 35/58 [00:09<00:02, 10.40it/s]Capturing num tokens (num_tokens=256 avail_mem=84.76 GB):  60%|██████    | 35/58 [00:09<00:02, 10.40it/s]

    Capturing num tokens (num_tokens=256 avail_mem=84.76 GB):  64%|██████▍   | 37/58 [00:09<00:01, 11.69it/s]Capturing num tokens (num_tokens=240 avail_mem=85.12 GB):  64%|██████▍   | 37/58 [00:09<00:01, 11.69it/s]Capturing num tokens (num_tokens=224 avail_mem=83.75 GB):  64%|██████▍   | 37/58 [00:10<00:01, 11.69it/s]

    Capturing num tokens (num_tokens=224 avail_mem=83.75 GB):  67%|██████▋   | 39/58 [00:10<00:01, 10.04it/s]Capturing num tokens (num_tokens=208 avail_mem=84.08 GB):  67%|██████▋   | 39/58 [00:10<00:01, 10.04it/s]Capturing num tokens (num_tokens=192 avail_mem=84.08 GB):  67%|██████▋   | 39/58 [00:10<00:01, 10.04it/s]

    Capturing num tokens (num_tokens=192 avail_mem=84.08 GB):  71%|███████   | 41/58 [00:10<00:01,  9.01it/s]Capturing num tokens (num_tokens=176 avail_mem=84.81 GB):  71%|███████   | 41/58 [00:10<00:01,  9.01it/s]Capturing num tokens (num_tokens=160 avail_mem=85.07 GB):  71%|███████   | 41/58 [00:10<00:01,  9.01it/s]

    Capturing num tokens (num_tokens=160 avail_mem=85.07 GB):  74%|███████▍  | 43/58 [00:10<00:01,  9.01it/s]Capturing num tokens (num_tokens=144 avail_mem=84.25 GB):  74%|███████▍  | 43/58 [00:10<00:01,  9.01it/s]Capturing num tokens (num_tokens=144 avail_mem=84.25 GB):  76%|███████▌  | 44/58 [00:10<00:01,  8.58it/s]Capturing num tokens (num_tokens=128 avail_mem=84.05 GB):  76%|███████▌  | 44/58 [00:10<00:01,  8.58it/s]

    Capturing num tokens (num_tokens=112 avail_mem=85.17 GB):  76%|███████▌  | 44/58 [00:10<00:01,  8.58it/s]Capturing num tokens (num_tokens=112 avail_mem=85.17 GB):  79%|███████▉  | 46/58 [00:10<00:01,  9.19it/s]Capturing num tokens (num_tokens=96 avail_mem=85.07 GB):  79%|███████▉  | 46/58 [00:10<00:01,  9.19it/s] Capturing num tokens (num_tokens=80 avail_mem=84.87 GB):  79%|███████▉  | 46/58 [00:11<00:01,  9.19it/s]

    Capturing num tokens (num_tokens=80 avail_mem=84.87 GB):  83%|████████▎ | 48/58 [00:11<00:01,  9.27it/s]Capturing num tokens (num_tokens=64 avail_mem=84.29 GB):  83%|████████▎ | 48/58 [00:11<00:01,  9.27it/s]Capturing num tokens (num_tokens=64 avail_mem=84.29 GB):  84%|████████▍ | 49/58 [00:11<00:01,  8.99it/s]Capturing num tokens (num_tokens=48 avail_mem=84.29 GB):  84%|████████▍ | 49/58 [00:11<00:01,  8.99it/s]

    Capturing num tokens (num_tokens=32 avail_mem=84.94 GB):  84%|████████▍ | 49/58 [00:11<00:01,  8.99it/s]Capturing num tokens (num_tokens=32 avail_mem=84.94 GB):  88%|████████▊ | 51/58 [00:11<00:00,  9.42it/s]Capturing num tokens (num_tokens=28 avail_mem=84.33 GB):  88%|████████▊ | 51/58 [00:11<00:00,  9.42it/s]

    Capturing num tokens (num_tokens=28 avail_mem=84.33 GB):  90%|████████▉ | 52/58 [00:11<00:00,  9.13it/s]Capturing num tokens (num_tokens=24 avail_mem=84.32 GB):  90%|████████▉ | 52/58 [00:11<00:00,  9.13it/s]Capturing num tokens (num_tokens=24 avail_mem=84.32 GB):  91%|█████████▏| 53/58 [00:11<00:00,  8.91it/s]Capturing num tokens (num_tokens=20 avail_mem=84.32 GB):  91%|█████████▏| 53/58 [00:11<00:00,  8.91it/s]

    Capturing num tokens (num_tokens=16 avail_mem=85.02 GB):  91%|█████████▏| 53/58 [00:11<00:00,  8.91it/s]Capturing num tokens (num_tokens=16 avail_mem=85.02 GB):  95%|█████████▍| 55/58 [00:11<00:00,  9.08it/s]Capturing num tokens (num_tokens=12 avail_mem=84.36 GB):  95%|█████████▍| 55/58 [00:11<00:00,  9.08it/s]

    Capturing num tokens (num_tokens=12 avail_mem=84.36 GB):  97%|█████████▋| 56/58 [00:12<00:00,  8.98it/s]Capturing num tokens (num_tokens=8 avail_mem=84.35 GB):  97%|█████████▋| 56/58 [00:12<00:00,  8.98it/s] Capturing num tokens (num_tokens=8 avail_mem=84.35 GB):  98%|█████████▊| 57/58 [00:12<00:00,  8.22it/s]Capturing num tokens (num_tokens=4 avail_mem=85.01 GB):  98%|█████████▊| 57/58 [00:12<00:00,  8.22it/s]

    Capturing num tokens (num_tokens=4 avail_mem=85.01 GB): 100%|██████████| 58/58 [00:12<00:00,  4.70it/s]


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


<strong style='color: #00008B;'>reasoing_content: Okay, so the user is in New York and wants the current date and time along with the weather. Let me figure out how to approach this.<br><br>First, I need to determine which functions to use. The user mentioned they're in New York, so I should use 'get_current_date' and 'get_current_weather'. <br><br>For the date, I'll call 'get_current_date' with the timezone set to 'America/New_York'. That should give me the current date and time in their local time zone.<br><br>Next, for the weather, I'll call 'get_current_weather' with the city as 'New York' and the unit as 'fahrenheit' since the user didn't specify, but Fahrenheit is commonly used in the US.<br><br>I should structure the response by first showing the date and time, then the weather information. I'll make sure to include the units and any relevant details like the temperature and conditions.<br><br>I need to remember to format each function call correctly, using the specified syntax with start and end tags. Also, I'll add sources where I got the information, but since these are built-in functions, I don't need external data.<br><br>Putting it all together, I'll present the date and time first, followed by the weather details, each in their own function call blocks. That should cover everything the user asked for.<br><br><br>content: <function=get_current_date>{"timezone": "America/New_York"}</function><br><function=get_current_weather>{"city": "New York", "state": "NY", "unit": "fahrenheit"}</function></strong>


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

    {'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. I know that Paris is the capital, so that\'s straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I\'m not sure of the exact number. I think it\'s around 2 million, but I should verify that.\n\nWait, maybe I should check the latest statistics. I recall that in recent years, Paris has seen a slight increase. Let me think... I believe the population is approximately 2,150,000 as of 2023. That seems about right, but I\'m not 100% certain. I should make sure to present this information accurately.\n\nNext, I need to structure this into a JSON format. JSON requires key-value pairs, so I\'ll create an object with keys like "city", "population", and maybe "country" for context. The city is Paris, the population is 2,150,000, and the country is France.\n\nI should also consider if the user might need more details, like the exact year of the population figure. Including that could be helpful, so I\'ll add "year": 2023. That way, the user knows the data is up to date.\n\nPutting it all together, the JSON should look clean and well-structured. I\'ll make sure the syntax is correct, with proper commas and quotation marks. No markdown, just plain JSON.\n\nI think that\'s all. The user probably just needs the information quickly, so keeping it concise is key. I\'ll present the JSON without any extra fluff.\n</think>{\n\n"name": "Paris",\n"population": 21500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1265, 1477, 279, 1429, 3213, 7042, 821, 13, 358, 6099, 429, 279, 7042, 315, 12095, 702, 1012, 7826, 11, 714, 358, 2776, 537, 2704, 315, 279, 4734, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 10146, 429, 382, 14190, 11, 7196, 358, 1265, 1779, 279, 5535, 13142, 13, 358, 19091, 429, 304, 3213, 1635, 11, 12095, 702, 3884, 264, 8112, 5263, 13, 6771, 752, 1744, 1112, 358, 4411, 279, 7042, 374, 13187, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 438, 315, 220, 17, 15, 17, 18, 13, 2938, 4977, 911, 1290, 11, 714, 358, 2776, 537, 220, 16, 15, 15, 4, 3654, 13, 358, 1265, 1281, 2704, 311, 3042, 419, 1995, 29257, 382, 5847, 11, 358, 1184, 311, 5944, 419, 1119, 264, 4718, 3561, 13, 4718, 7460, 1376, 19083, 13530, 11, 773, 358, 3278, 1855, 458, 1633, 448, 6894, 1075, 330, 8926, 497, 330, 44441, 497, 323, 7196, 330, 11141, 1, 369, 2266, 13, 576, 3283, 374, 12095, 11, 279, 7042, 374, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 11, 323, 279, 3146, 374, 9625, 382, 40, 1265, 1083, 2908, 421, 279, 1196, 2578, 1184, 803, 3565, 11, 1075, 279, 4734, 1042, 315, 279, 7042, 7071, 13, 55121, 429, 1410, 387, 10950, 11, 773, 358, 3278, 912, 330, 3157, 788, 220, 17, 15, 17, 18, 13, 2938, 1616, 11, 279, 1196, 8788, 279, 821, 374, 705, 311, 2400, 382, 97904, 432, 678, 3786, 11, 279, 4718, 1265, 1401, 4240, 323, 1632, 12, 51143, 13, 358, 3278, 1281, 2704, 279, 19482, 374, 4396, 11, 448, 6169, 76602, 323, 54231, 15423, 13, 2308, 50494, 11, 1101, 14396, 4718, 382, 40, 1744, 429, 594, 678, 13, 576, 1196, 4658, 1101, 3880, 279, 1995, 6157, 11, 773, 10282, 432, 63594, 374, 1376, 13, 358, 3278, 3042, 279, 4718, 2041, 894, 4960, 1320, 1362, 624, 151649, 4257, 1, 606, 788, 330, 59604, 756, 1, 44441, 788, 220, 17, 16, 20, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15], 'meta_info': {'id': '2def78311a5547898ce93b379cc2dc5f', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 18.59752351278439, 'response_sent_to_client_ts': 1775089006.9314747}}



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


<strong style='color: #00008B;'>{'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. I know that Paris is the capital, so that\'s straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I\'m not exactly sure of the current number. I think it\'s around 2 million, but I should verify that.\n\nWait, maybe I should check some reliable sources to confirm the population. I recall that the population figures can vary depending on the source and the year. For example, the 2020 census might have a slightly different number than the 2021 estimate. I think the population was around 2,165,000 in 2021, but I\'m not 100% certain. I should make sure to use the most accurate and up-to-date information.\n\nAlso, the user wants the information in JSON format. JSON is a data interchange format, so I\'ll need to structure the data accordingly. I should include the city name, population, and maybe the year of the data. It\'s important to present the information clearly and accurately, so I\'ll double-check the numbers to avoid any mistakes.\n\nI should also consider if there are any other relevant details the user might find useful, like the area of the city or some key facts about it. But since the user specifically asked for population, I\'ll focus on that. Maybe adding a note about the population figure being approximate would be helpful, just in case.\n\nPutting it all together, I\'ll structure the JSON with the city name, population, and the year. I\'ll make sure the syntax is correct, using quotation marks and commas appropriately. I\'ll also keep the language clear and straightforward so that the user can easily understand the information.\n\nFinally, I\'ll review the JSON to ensure there are no errors and that the data is accurate. This way, the user gets a reliable and well-formatted response to their query.\n</think>{"name": "Paris", "population": 2165000}', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1265, 1477, 279, 1429, 3213, 7042, 821, 13, 358, 6099, 429, 279, 7042, 315, 12095, 702, 1012, 7826, 11, 714, 358, 2776, 537, 6896, 2704, 315, 279, 1482, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 10146, 429, 382, 14190, 11, 7196, 358, 1265, 1779, 1045, 14720, 8173, 311, 7683, 279, 7042, 13, 358, 19091, 429, 279, 7042, 12396, 646, 13289, 11649, 389, 279, 2530, 323, 279, 1042, 13, 1752, 3110, 11, 279, 220, 17, 15, 17, 15, 43602, 2578, 614, 264, 10078, 2155, 1372, 1091, 279, 220, 17, 15, 17, 16, 16045, 13, 358, 1744, 279, 7042, 572, 2163, 220, 17, 11, 16, 21, 20, 11, 15, 15, 15, 304, 220, 17, 15, 17, 16, 11, 714, 358, 2776, 537, 220, 16, 15, 15, 4, 3654, 13, 358, 1265, 1281, 2704, 311, 990, 279, 1429, 13382, 323, 705, 4686, 18413, 1995, 382, 13394, 11, 279, 1196, 6801, 279, 1995, 304, 4718, 3561, 13, 4718, 374, 264, 821, 51263, 3561, 11, 773, 358, 3278, 1184, 311, 5944, 279, 821, 27079, 13, 358, 1265, 2924, 279, 3283, 829, 11, 7042, 11, 323, 7196, 279, 1042, 315, 279, 821, 13, 1084, 594, 2989, 311, 3042, 279, 1995, 9355, 323, 29257, 11, 773, 358, 3278, 1990, 15934, 279, 5109, 311, 5648, 894, 20643, 382, 40, 1265, 1083, 2908, 421, 1052, 525, 894, 1008, 9760, 3565, 279, 1196, 2578, 1477, 5390, 11, 1075, 279, 3082, 315, 279, 3283, 476, 1045, 1376, 13064, 911, 432, 13, 1988, 2474, 279, 1196, 11689, 4588, 369, 7042, 11, 358, 3278, 5244, 389, 429, 13, 10696, 7842, 264, 5185, 911, 279, 7042, 7071, 1660, 44868, 1035, 387, 10950, 11, 1101, 304, 1142, 382, 97904, 432, 678, 3786, 11, 358, 3278, 5944, 279, 4718, 448, 279, 3283, 829, 11, 7042, 11, 323, 279, 1042, 13, 358, 3278, 1281, 2704, 279, 19482, 374, 4396, 11, 1667, 54231, 15423, 323, 76602, 34901, 13, 358, 3278, 1083, 2506, 279, 4128, 2797, 323, 30339, 773, 429, 279, 1196, 646, 6707, 3535, 279, 1995, 382, 23949, 11, 358, 3278, 3395, 279, 4718, 311, 5978, 1052, 525, 902, 5975, 323, 429, 279, 821, 374, 13382, 13, 1096, 1616, 11, 279, 1196, 5221, 264, 14720, 323, 1632, 8460, 12127, 2033, 311, 862, 3239, 624, 151649, 4913, 606, 788, 330, 59604, 497, 330, 44441, 788, 220, 17, 16, 21, 20, 15, 15, 15, 92, 151643], 'meta_info': {'id': 'd5804e024ea9495b913c57c8bbb7b9a2', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 447, 'cached_tokens': 22, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 5.036251584999263, 'response_sent_to_client_ts': 1775089011.97886}}</strong>


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

    [{'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '375dca39dbbd42c3ae31dd2d51c0224a', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.11627476988360286, 'response_sent_to_client_ts': 1775089012.1353686}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '1834a9ca79c64ce080d00a06478644c9', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.11619696533307433, 'response_sent_to_client_ts': 1775089012.1353803}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '430137d4b28a42fe810a6b1660bd9028', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.11614290392026305, 'response_sent_to_client_ts': 1775089012.135385}}]


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

    {'text': ' France, and the \n\\( n \\)  \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\(', 'output_ids': [9625, 11, 323, 279, 220, 198, 44292, 308, 1124, 8, 220, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767], 'meta_info': {'id': 'd4b5b13695704b88a600a4862983f2b0', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 6, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 20.067994282580912, 'response_sent_to_client_ts': 1775089032.2110918}}


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


<strong style='color: #00008B;'>{'text': 'Okay, the user is asking for the information and population of the capital of France in JSON format. So first, I need to figure out where the capital of France is. I know it\'s Paris, so I should start by confirming that.\n\nNext, they want population data. I remember that Paris is a major city with a large population, but I\'m not sure about the exact numbers. I think the population is over 3 million, maybe around 3.5 million as of the latest estimate, but I\'m not 100% sure.\n\nI also need to consider the format. They specified JSON, which is a data interchange format, so I\'ll structure it accordingly. I should include the basic info like country, city, population, and any additional details they might find useful, like the administrative division (Arrondissement)?\n\nWait, do I include that? The user didn\'t specify, but including more details could be helpful. I should make it clear in the response that there might be discrepancies in population numbers due to varying sources and time periods.\n\nI should also present the information neatly, perhaps with more readable line breaks. But since the user wants JSON, that\'s the structure they need.\n\nAnother point: population estimates can change over time. It\'s important to note that the figure I provide is approximate and should be updated if more precise data is available. Maybe I should mention that it\'s around 3,500,000 as of recent records.\n\nPutting it all together, I\'ll structure the JSON with country, capital city, population, and a note about the estimate. Then, I\'ll present the JSON in a code block so it\'s clear and easy to read.\n\nI should also double-check the population numbers to ensure accuracy. If I\'m unsure, it\'s better to admit that the data might vary. I recall that Paris\'s population in recent years has been maintained at around 3.5 million, so that should be acceptable.\n\nFinally, I\'ll make sure the JSON is correctly formatted, with proper commas and brackets, so there are no syntax errors. That way, the user can easily copy and use the data.\n</think>\n\nHere is the information and population of the capital of France in JSON format:\n\n```json\n{\n  "country": "France",\n  "capital": "Paris",\n  "population": 3500000,\n  "notes": "Population figures are approximate and may vary depending on the source and time period."\n}\n```', 'output_ids': [32313, 11, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 2055, 1156, 11, 358, 1184, 311, 7071, 700, 1380, 279, 6722, 315, 9625, 374, 13, 358, 1414, 432, 594, 12095, 11, 773, 358, 1265, 1191, 553, 48996, 429, 382, 5847, 11, 807, 1366, 7042, 821, 13, 358, 6099, 429, 12095, 374, 264, 3598, 3283, 448, 264, 3460, 7042, 11, 714, 358, 2776, 537, 2704, 911, 279, 4734, 5109, 13, 358, 1744, 279, 7042, 374, 916, 220, 18, 3526, 11, 7196, 2163, 220, 18, 13, 20, 3526, 438, 315, 279, 5535, 16045, 11, 714, 358, 2776, 537, 220, 16, 15, 15, 4, 2704, 382, 40, 1083, 1184, 311, 2908, 279, 3561, 13, 2379, 5189, 4718, 11, 892, 374, 264, 821, 51263, 3561, 11, 773, 358, 3278, 5944, 432, 27079, 13, 358, 1265, 2924, 279, 6770, 3546, 1075, 3146, 11, 3283, 11, 7042, 11, 323, 894, 5107, 3565, 807, 2578, 1477, 5390, 11, 1075, 279, 22707, 12804, 320, 8838, 2111, 49653, 73530, 14190, 11, 653, 358, 2924, 429, 30, 576, 1196, 3207, 944, 13837, 11, 714, 2670, 803, 3565, 1410, 387, 10950, 13, 358, 1265, 1281, 432, 2797, 304, 279, 2033, 429, 1052, 2578, 387, 90267, 304, 7042, 5109, 4152, 311, 28765, 8173, 323, 882, 18346, 382, 40, 1265, 1083, 3042, 279, 1995, 62166, 11, 8365, 448, 803, 33798, 1555, 18303, 13, 1988, 2474, 279, 1196, 6801, 4718, 11, 429, 594, 279, 5944, 807, 1184, 382, 14037, 1459, 25, 7042, 17530, 646, 2297, 916, 882, 13, 1084, 594, 2989, 311, 5185, 429, 279, 7071, 358, 3410, 374, 44868, 323, 1265, 387, 6049, 421, 803, 23560, 821, 374, 2500, 13, 10696, 358, 1265, 6286, 429, 432, 594, 2163, 220, 18, 11, 20, 15, 15, 11, 15, 15, 15, 438, 315, 3213, 7424, 382, 97904, 432, 678, 3786, 11, 358, 3278, 5944, 279, 4718, 448, 3146, 11, 6722, 3283, 11, 7042, 11, 323, 264, 5185, 911, 279, 16045, 13, 5005, 11, 358, 3278, 3042, 279, 4718, 304, 264, 2038, 2504, 773, 432, 594, 2797, 323, 4135, 311, 1349, 382, 40, 1265, 1083, 1990, 15934, 279, 7042, 5109, 311, 5978, 13403, 13, 1416, 358, 2776, 42903, 11, 432, 594, 2664, 311, 16698, 429, 279, 821, 2578, 13289, 13, 358, 19091, 429, 12095, 594, 7042, 304, 3213, 1635, 702, 1012, 18401, 518, 2163, 220, 18, 13, 20, 3526, 11, 773, 429, 1265, 387, 21555, 382, 23949, 11, 358, 3278, 1281, 2704, 279, 4718, 374, 12440, 23126, 11, 448, 6169, 76602, 323, 38929, 11, 773, 1052, 525, 902, 19482, 5975, 13, 2938, 1616, 11, 279, 1196, 646, 6707, 2975, 323, 990, 279, 821, 624, 151649, 271, 8420, 374, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 1447, 73594, 2236, 198, 515, 220, 330, 11141, 788, 330, 49000, 756, 220, 330, 65063, 788, 330, 59604, 756, 220, 330, 44441, 788, 220, 18, 20, 15, 15, 15, 15, 15, 345, 220, 330, 18286, 788, 330, 53371, 12396, 525, 44868, 323, 1231, 13289, 11649, 389, 279, 2530, 323, 882, 4168, 10040, 532, 73594, 151643], 'meta_info': {'id': '2f5a3e8d7c9e40d499903e8ed9f11b74', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 508, 'cached_tokens': 22, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 4.966439655981958, 'response_sent_to_client_ts': 1775089037.1893134}}</strong>



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


    2026-04-02 00:17:30.237 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 00:17:30] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 00:17:30.237 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 00:17:30] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 00:17:30.237 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 00:17:30] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 00:17:30.237 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 00:17:30] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 00:17:30.237 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 00:17:30] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:00<00:00,  1.43it/s]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:01<00:00,  1.09it/s]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:01<00:00,  1.13it/s]


    2026-04-02 00:17:34,541 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 00:17:34] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:56,  3.09s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:56,  3.09s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:38,  1.76s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:38,  1.76s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:02,  1.13s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:02,  1.13s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:44,  1.21it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:44,  1.21it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:33,  1.57it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:33,  1.57it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:26,  1.94it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:26,  1.94it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:21,  2.36it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:21,  2.36it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:05<00:17,  2.78it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:05<00:17,  2.78it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:05<00:15,  3.20it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:05<00:15,  3.20it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:06<00:12,  3.71it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:06<00:12,  3.71it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:06<00:10,  4.35it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:06<00:10,  4.35it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:06<00:09,  4.73it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:06<00:09,  4.73it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:06<00:08,  5.36it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:06<00:08,  5.36it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:06<00:07,  6.01it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:06<00:07,  6.01it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:06<00:06,  6.68it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:06<00:06,  6.68it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:06<00:06,  6.68it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:06<00:04,  8.23it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:06<00:04,  8.23it/s]

    Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:07<00:04,  8.23it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:07<00:03, 10.14it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:07<00:03, 10.14it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:07<00:03, 10.14it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:07<00:03, 11.73it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:07<00:03, 11.73it/s] 

    Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:07<00:03, 11.73it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:07<00:02, 13.65it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:07<00:02, 13.65it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:07<00:02, 13.65it/s]Compiling num tokens (num_tokens=704):  40%|███▉      | 23/58 [00:07<00:02, 13.65it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:07<00:01, 17.25it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:07<00:01, 17.25it/s]

    Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:07<00:01, 17.25it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:07<00:01, 17.25it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:07<00:01, 20.20it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:07<00:01, 20.20it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:07<00:01, 20.20it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:07<00:01, 20.20it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:07<00:01, 20.20it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:07<00:00, 25.03it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:07<00:00, 25.03it/s]

    Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:07<00:00, 25.03it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:07<00:00, 25.03it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:07<00:00, 25.03it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:07<00:00, 27.62it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:07<00:00, 27.62it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:07<00:00, 27.62it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:07<00:00, 27.62it/s]Compiling num tokens (num_tokens=192):  64%|██████▍   | 37/58 [00:07<00:00, 27.62it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:07<00:00, 30.26it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:07<00:00, 30.26it/s]

    Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:07<00:00, 30.26it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:07<00:00, 30.26it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:07<00:00, 30.26it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:07<00:00, 32.27it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:07<00:00, 32.27it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:07<00:00, 32.27it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:07<00:00, 32.27it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:08<00:00, 32.27it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:08<00:00, 32.27it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:08<00:00, 36.15it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:08<00:00, 36.15it/s]

    Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:08<00:00, 36.15it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:08<00:00, 36.15it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:08<00:00, 36.15it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:08<00:00, 36.15it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:08<00:00, 36.15it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:08<00:00, 42.17it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:08<00:00, 42.17it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:08<00:00, 42.17it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:08<00:00,  7.10it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=104.67 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=104.67 GB):   2%|▏         | 1/58 [00:00<00:20,  2.75it/s]Capturing num tokens (num_tokens=7680 avail_mem=104.64 GB):   2%|▏         | 1/58 [00:00<00:20,  2.75it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=104.64 GB):   3%|▎         | 2/58 [00:00<00:19,  2.93it/s]Capturing num tokens (num_tokens=7168 avail_mem=104.63 GB):   3%|▎         | 2/58 [00:00<00:19,  2.93it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=104.63 GB):   5%|▌         | 3/58 [00:00<00:17,  3.15it/s]Capturing num tokens (num_tokens=6656 avail_mem=104.62 GB):   5%|▌         | 3/58 [00:00<00:17,  3.15it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=104.62 GB):   7%|▋         | 4/58 [00:01<00:14,  3.61it/s]Capturing num tokens (num_tokens=6144 avail_mem=104.63 GB):   7%|▋         | 4/58 [00:01<00:14,  3.61it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=104.63 GB):   9%|▊         | 5/58 [00:01<00:13,  3.98it/s]Capturing num tokens (num_tokens=5632 avail_mem=104.63 GB):   9%|▊         | 5/58 [00:01<00:13,  3.98it/s]Capturing num tokens (num_tokens=5632 avail_mem=104.63 GB):  10%|█         | 6/58 [00:01<00:11,  4.45it/s]Capturing num tokens (num_tokens=5120 avail_mem=104.63 GB):  10%|█         | 6/58 [00:01<00:11,  4.45it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=104.63 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.88it/s]Capturing num tokens (num_tokens=4608 avail_mem=104.64 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.88it/s]Capturing num tokens (num_tokens=4608 avail_mem=104.64 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.43it/s]Capturing num tokens (num_tokens=4096 avail_mem=104.64 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.43it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=104.64 GB):  16%|█▌        | 9/58 [00:02<00:08,  5.73it/s]Capturing num tokens (num_tokens=3840 avail_mem=104.63 GB):  16%|█▌        | 9/58 [00:02<00:08,  5.73it/s]Capturing num tokens (num_tokens=3840 avail_mem=104.63 GB):  17%|█▋        | 10/58 [00:02<00:08,  5.86it/s]Capturing num tokens (num_tokens=3584 avail_mem=104.62 GB):  17%|█▋        | 10/58 [00:02<00:08,  5.86it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=104.62 GB):  19%|█▉        | 11/58 [00:02<00:07,  6.21it/s]Capturing num tokens (num_tokens=3328 avail_mem=104.13 GB):  19%|█▉        | 11/58 [00:02<00:07,  6.21it/s]Capturing num tokens (num_tokens=3328 avail_mem=104.13 GB):  21%|██        | 12/58 [00:02<00:06,  6.93it/s]Capturing num tokens (num_tokens=3072 avail_mem=103.97 GB):  21%|██        | 12/58 [00:02<00:06,  6.93it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=103.97 GB):  21%|██        | 12/58 [00:02<00:06,  6.93it/s]Capturing num tokens (num_tokens=2816 avail_mem=103.97 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.31it/s]Capturing num tokens (num_tokens=2560 avail_mem=103.97 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.31it/s]Capturing num tokens (num_tokens=2304 avail_mem=103.97 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.31it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=103.97 GB):  28%|██▊       | 16/58 [00:02<00:04,  9.55it/s]Capturing num tokens (num_tokens=2048 avail_mem=103.91 GB):  28%|██▊       | 16/58 [00:02<00:04,  9.55it/s]Capturing num tokens (num_tokens=2048 avail_mem=103.91 GB):  29%|██▉       | 17/58 [00:02<00:04,  8.99it/s]Capturing num tokens (num_tokens=1792 avail_mem=103.91 GB):  29%|██▉       | 17/58 [00:02<00:04,  8.99it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=103.91 GB):  31%|███       | 18/58 [00:03<00:04,  8.74it/s]Capturing num tokens (num_tokens=1536 avail_mem=103.91 GB):  31%|███       | 18/58 [00:03<00:04,  8.74it/s]Capturing num tokens (num_tokens=1536 avail_mem=103.91 GB):  33%|███▎      | 19/58 [00:03<00:04,  8.76it/s]Capturing num tokens (num_tokens=1280 avail_mem=103.91 GB):  33%|███▎      | 19/58 [00:03<00:04,  8.76it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=103.91 GB):  33%|███▎      | 19/58 [00:03<00:04,  8.76it/s]Capturing num tokens (num_tokens=1024 avail_mem=103.91 GB):  36%|███▌      | 21/58 [00:03<00:03,  9.52it/s]Capturing num tokens (num_tokens=960 avail_mem=103.91 GB):  36%|███▌      | 21/58 [00:03<00:03,  9.52it/s] Capturing num tokens (num_tokens=896 avail_mem=103.90 GB):  36%|███▌      | 21/58 [00:03<00:03,  9.52it/s]

    Capturing num tokens (num_tokens=896 avail_mem=103.90 GB):  40%|███▉      | 23/58 [00:03<00:03, 10.26it/s]Capturing num tokens (num_tokens=832 avail_mem=103.90 GB):  40%|███▉      | 23/58 [00:03<00:03, 10.26it/s]Capturing num tokens (num_tokens=768 avail_mem=103.89 GB):  40%|███▉      | 23/58 [00:03<00:03, 10.26it/s]Capturing num tokens (num_tokens=768 avail_mem=103.89 GB):  43%|████▎     | 25/58 [00:03<00:02, 11.04it/s]Capturing num tokens (num_tokens=704 avail_mem=103.89 GB):  43%|████▎     | 25/58 [00:03<00:02, 11.04it/s]

    Capturing num tokens (num_tokens=640 avail_mem=103.89 GB):  43%|████▎     | 25/58 [00:03<00:02, 11.04it/s]Capturing num tokens (num_tokens=640 avail_mem=103.89 GB):  47%|████▋     | 27/58 [00:03<00:02, 11.84it/s]Capturing num tokens (num_tokens=576 avail_mem=103.88 GB):  47%|████▋     | 27/58 [00:03<00:02, 11.84it/s]Capturing num tokens (num_tokens=512 avail_mem=103.88 GB):  47%|████▋     | 27/58 [00:03<00:02, 11.84it/s]

    Capturing num tokens (num_tokens=512 avail_mem=103.88 GB):  50%|█████     | 29/58 [00:03<00:02, 12.71it/s]Capturing num tokens (num_tokens=480 avail_mem=103.88 GB):  50%|█████     | 29/58 [00:03<00:02, 12.71it/s]Capturing num tokens (num_tokens=448 avail_mem=103.87 GB):  50%|█████     | 29/58 [00:04<00:02, 12.71it/s]Capturing num tokens (num_tokens=448 avail_mem=103.87 GB):  53%|█████▎    | 31/58 [00:04<00:01, 13.75it/s]Capturing num tokens (num_tokens=416 avail_mem=103.87 GB):  53%|█████▎    | 31/58 [00:04<00:01, 13.75it/s]Capturing num tokens (num_tokens=384 avail_mem=103.87 GB):  53%|█████▎    | 31/58 [00:04<00:01, 13.75it/s]

    Capturing num tokens (num_tokens=384 avail_mem=103.87 GB):  57%|█████▋    | 33/58 [00:04<00:01, 15.10it/s]Capturing num tokens (num_tokens=352 avail_mem=103.86 GB):  57%|█████▋    | 33/58 [00:04<00:01, 15.10it/s]Capturing num tokens (num_tokens=320 avail_mem=103.86 GB):  57%|█████▋    | 33/58 [00:04<00:01, 15.10it/s]Capturing num tokens (num_tokens=288 avail_mem=103.85 GB):  57%|█████▋    | 33/58 [00:04<00:01, 15.10it/s]Capturing num tokens (num_tokens=256 avail_mem=103.85 GB):  57%|█████▋    | 33/58 [00:04<00:01, 15.10it/s]Capturing num tokens (num_tokens=256 avail_mem=103.85 GB):  64%|██████▍   | 37/58 [00:04<00:01, 20.92it/s]Capturing num tokens (num_tokens=240 avail_mem=103.85 GB):  64%|██████▍   | 37/58 [00:04<00:01, 20.92it/s]Capturing num tokens (num_tokens=224 avail_mem=103.84 GB):  64%|██████▍   | 37/58 [00:04<00:01, 20.92it/s]Capturing num tokens (num_tokens=208 avail_mem=103.84 GB):  64%|██████▍   | 37/58 [00:04<00:01, 20.92it/s]

    Capturing num tokens (num_tokens=208 avail_mem=103.84 GB):  69%|██████▉   | 40/58 [00:04<00:00, 21.88it/s]Capturing num tokens (num_tokens=192 avail_mem=103.84 GB):  69%|██████▉   | 40/58 [00:04<00:00, 21.88it/s]Capturing num tokens (num_tokens=176 avail_mem=120.43 GB):  69%|██████▉   | 40/58 [00:04<00:00, 21.88it/s]Capturing num tokens (num_tokens=160 avail_mem=120.42 GB):  69%|██████▉   | 40/58 [00:04<00:00, 21.88it/s]

    Capturing num tokens (num_tokens=160 avail_mem=120.42 GB):  74%|███████▍  | 43/58 [00:04<00:00, 18.12it/s]Capturing num tokens (num_tokens=144 avail_mem=116.73 GB):  74%|███████▍  | 43/58 [00:04<00:00, 18.12it/s]Capturing num tokens (num_tokens=128 avail_mem=106.46 GB):  74%|███████▍  | 43/58 [00:04<00:00, 18.12it/s]Capturing num tokens (num_tokens=112 avail_mem=106.46 GB):  74%|███████▍  | 43/58 [00:04<00:00, 18.12it/s]Capturing num tokens (num_tokens=112 avail_mem=106.46 GB):  79%|███████▉  | 46/58 [00:04<00:00, 20.57it/s]Capturing num tokens (num_tokens=96 avail_mem=106.46 GB):  79%|███████▉  | 46/58 [00:04<00:00, 20.57it/s] Capturing num tokens (num_tokens=80 avail_mem=106.45 GB):  79%|███████▉  | 46/58 [00:04<00:00, 20.57it/s]Capturing num tokens (num_tokens=64 avail_mem=106.45 GB):  79%|███████▉  | 46/58 [00:04<00:00, 20.57it/s]Capturing num tokens (num_tokens=48 avail_mem=106.45 GB):  79%|███████▉  | 46/58 [00:04<00:00, 20.57it/s]

    Capturing num tokens (num_tokens=48 avail_mem=106.45 GB):  86%|████████▌ | 50/58 [00:04<00:00, 24.87it/s]Capturing num tokens (num_tokens=32 avail_mem=106.44 GB):  86%|████████▌ | 50/58 [00:04<00:00, 24.87it/s]Capturing num tokens (num_tokens=28 avail_mem=106.44 GB):  86%|████████▌ | 50/58 [00:04<00:00, 24.87it/s]Capturing num tokens (num_tokens=24 avail_mem=106.44 GB):  86%|████████▌ | 50/58 [00:04<00:00, 24.87it/s]Capturing num tokens (num_tokens=20 avail_mem=106.43 GB):  86%|████████▌ | 50/58 [00:04<00:00, 24.87it/s]Capturing num tokens (num_tokens=20 avail_mem=106.43 GB):  93%|█████████▎| 54/58 [00:04<00:00, 28.13it/s]Capturing num tokens (num_tokens=16 avail_mem=106.43 GB):  93%|█████████▎| 54/58 [00:04<00:00, 28.13it/s]Capturing num tokens (num_tokens=12 avail_mem=106.43 GB):  93%|█████████▎| 54/58 [00:04<00:00, 28.13it/s]Capturing num tokens (num_tokens=8 avail_mem=106.42 GB):  93%|█████████▎| 54/58 [00:04<00:00, 28.13it/s] Capturing num tokens (num_tokens=4 avail_mem=106.42 GB):  93%|█████████▎| 54/58 [00:05<00:00, 28.13it/s]

    Capturing num tokens (num_tokens=4 avail_mem=106.42 GB): 100%|██████████| 58/58 [00:05<00:00, 30.75it/s]Capturing num tokens (num_tokens=4 avail_mem=106.42 GB): 100%|██████████| 58/58 [00:05<00:00, 11.49it/s]


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
    
    Generated text: Alright, so the user is asking for the information and population of the capital of France in JSON format. First, I need to identify the capital of France, which is Paris. 
    
    Next, I should gather accurate data about Paris's population. I recall that as of the latest estimates, Paris has a population around 2 million people. However, I should check if there's the most recent data available. Maybe the current population is slightly different, so it's better to verify.
    
    I also need to include other relevant information about Paris to make the JSON comprehensive. That would include its administrative region, which is Île-de-France, its prefecture, which is Île-de-France, and its metropolitan area population. The metropolitan area is often larger because it includes surrounding regions. I believe the metropolitan area population is about 11 million.
    
    Additionally, Paris is known for its landmarks like the Eiffel Tower, so including that could add value to the JSON. I should structure all this information neatly in a JSON format, making sure the keys are descriptive and the values are accurate.
    
    I should also note that the population figure might change over time due to factors like immigration, births, and deaths. Therefore, it's important to mention that the data is approximate and refers to a specific year, like 2023, to provide context.
    
    Putting it all together, I'll create a JSON object with the country name, capital, region, prefecture, metropolitan area population, population figure, landmarks, and a note about the population data. This should give the user a clear and concise answer in the desired format.
    </think>
    
    Here is the information about the capital of France (Paris) in JSON format:
    
    ```json
    {
      "country": "France",
      "capital": {
        "name": "Paris",
        "administrativeRegion": "Île-de-France",
        "prefecture": "Île-de-France",
        "metropolitanAreaPopulation": 11000000,
        "population": 2082000  // As of the latest estimate (2023)
      },
      "landmarks": [
        "Eiffel Tower"
      ],
      "notes": "Population figures are approximate and may vary slightly depending on the source and year."
    }
    ```



```python
llm.shutdown()
```
