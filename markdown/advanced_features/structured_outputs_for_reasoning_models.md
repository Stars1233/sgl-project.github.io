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
    2026-04-03 03:20:16.029 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 03:20:16] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 03:20:16.029 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 03:20:16] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 03:20:16.029 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 03:20:16] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 03:20:16.029 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 03:20:16] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 03:20:16.029 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 03:20:16] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Multi-thread loading shards:  50% Completed | 1/2 [00:01<00:01,  1.53s/it]

    Multi-thread loading shards: 100% Completed | 2/2 [00:03<00:00,  1.75s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:03<00:00,  1.71s/it]


    2026-04-03 03:20:23,814 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 03:20:23] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:07,  3.28s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:07,  3.28s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:04<01:43,  1.84s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:04<01:43,  1.84s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:05,  1.19s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:05,  1.19s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:46,  1.17it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:46,  1.17it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:34,  1.55it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:34,  1.55it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:24,  2.09it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:24,  2.09it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:18,  2.75it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:18,  2.75it/s]Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:05<00:14,  3.47it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:05<00:14,  3.47it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:05<00:11,  4.32it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:05<00:11,  4.32it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:05<00:11,  4.32it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:05<00:07,  6.03it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:05<00:07,  6.03it/s]

    Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:05<00:07,  6.03it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:06<00:05,  7.53it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:06<00:05,  7.53it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:06<00:05,  7.53it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:06<00:04,  9.15it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:06<00:04,  9.15it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:06<00:04,  9.15it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:06<00:03, 11.13it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:06<00:03, 11.13it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:06<00:03, 11.13it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:06<00:03, 11.13it/s]

    Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:06<00:02, 14.44it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:06<00:02, 14.44it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:06<00:02, 14.44it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:06<00:02, 14.44it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:06<00:02, 15.57it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:06<00:02, 15.57it/s]

    Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:06<00:02, 15.57it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:06<00:02, 15.98it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:06<00:02, 15.98it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:06<00:02, 15.98it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:06<00:01, 16.69it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:06<00:01, 16.69it/s]

    Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:06<00:01, 16.69it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:06<00:01, 16.69it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:06<00:01, 18.05it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:06<00:01, 18.05it/s]Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:06<00:01, 18.05it/s]Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:06<00:01, 18.05it/s]

    Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:07<00:01, 19.34it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:07<00:01, 19.34it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:07<00:01, 19.34it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:07<00:01, 19.34it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:07<00:01, 20.46it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:07<00:01, 20.46it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:07<00:01, 20.46it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:07<00:01, 20.46it/s]

    Compiling num tokens (num_tokens=208):  62%|██████▏   | 36/58 [00:07<00:01, 20.46it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:07<00:00, 22.85it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:07<00:00, 22.85it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:07<00:00, 22.85it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:07<00:00, 22.85it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:07<00:00, 23.40it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:07<00:00, 23.40it/s]

    Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:07<00:00, 23.40it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:07<00:00, 23.40it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:07<00:00, 24.86it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:07<00:00, 24.86it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:07<00:00, 24.86it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:07<00:00, 24.86it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:07<00:00, 25.38it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:07<00:00, 25.38it/s]

    Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:07<00:00, 25.38it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:07<00:00, 25.38it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:07<00:00, 25.38it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:07<00:00, 27.49it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:07<00:00, 27.49it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:07<00:00, 27.49it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:07<00:00, 27.49it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:07<00:00, 27.49it/s] 

    Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:07<00:00, 29.78it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:07<00:00, 29.78it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:07<00:00,  7.33it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=100.69 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=100.69 GB):   2%|▏         | 1/58 [00:00<00:41,  1.37it/s]Capturing num tokens (num_tokens=7680 avail_mem=100.36 GB):   2%|▏         | 1/58 [00:00<00:41,  1.37it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=100.36 GB):   3%|▎         | 2/58 [00:01<00:42,  1.32it/s]Capturing num tokens (num_tokens=7168 avail_mem=100.36 GB):   3%|▎         | 2/58 [00:01<00:42,  1.32it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=100.36 GB):   5%|▌         | 3/58 [00:02<00:36,  1.51it/s]Capturing num tokens (num_tokens=6656 avail_mem=100.36 GB):   5%|▌         | 3/58 [00:02<00:36,  1.51it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=100.36 GB):   7%|▋         | 4/58 [00:02<00:32,  1.67it/s]Capturing num tokens (num_tokens=6144 avail_mem=100.36 GB):   7%|▋         | 4/58 [00:02<00:32,  1.67it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=100.36 GB):   9%|▊         | 5/58 [00:02<00:27,  1.95it/s]Capturing num tokens (num_tokens=5632 avail_mem=100.36 GB):   9%|▊         | 5/58 [00:02<00:27,  1.95it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=100.36 GB):  10%|█         | 6/58 [00:05<01:00,  1.17s/it]Capturing num tokens (num_tokens=5120 avail_mem=100.03 GB):  10%|█         | 6/58 [00:05<01:00,  1.17s/it]

    Capturing num tokens (num_tokens=5120 avail_mem=100.03 GB):  12%|█▏        | 7/58 [00:05<00:45,  1.11it/s]Capturing num tokens (num_tokens=4608 avail_mem=100.35 GB):  12%|█▏        | 7/58 [00:05<00:45,  1.11it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=100.35 GB):  14%|█▍        | 8/58 [00:05<00:34,  1.44it/s]Capturing num tokens (num_tokens=4096 avail_mem=100.13 GB):  14%|█▍        | 8/58 [00:05<00:34,  1.44it/s]Capturing num tokens (num_tokens=4096 avail_mem=100.13 GB):  16%|█▌        | 9/58 [00:06<00:26,  1.86it/s]Capturing num tokens (num_tokens=3840 avail_mem=100.34 GB):  16%|█▌        | 9/58 [00:06<00:26,  1.86it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=100.34 GB):  17%|█▋        | 10/58 [00:06<00:21,  2.20it/s]Capturing num tokens (num_tokens=3584 avail_mem=99.29 GB):  17%|█▋        | 10/58 [00:06<00:21,  2.20it/s] 

    Capturing num tokens (num_tokens=3584 avail_mem=99.29 GB):  19%|█▉        | 11/58 [00:06<00:19,  2.44it/s]Capturing num tokens (num_tokens=3328 avail_mem=101.17 GB):  19%|█▉        | 11/58 [00:06<00:19,  2.44it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=101.17 GB):  21%|██        | 12/58 [00:06<00:16,  2.80it/s]Capturing num tokens (num_tokens=3072 avail_mem=100.28 GB):  21%|██        | 12/58 [00:06<00:16,  2.80it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=100.28 GB):  22%|██▏       | 13/58 [00:07<00:15,  2.81it/s]Capturing num tokens (num_tokens=2816 avail_mem=99.45 GB):  22%|██▏       | 13/58 [00:07<00:15,  2.81it/s] Capturing num tokens (num_tokens=2816 avail_mem=99.45 GB):  24%|██▍       | 14/58 [00:07<00:13,  3.26it/s]Capturing num tokens (num_tokens=2560 avail_mem=100.27 GB):  24%|██▍       | 14/58 [00:07<00:13,  3.26it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=100.27 GB):  26%|██▌       | 15/58 [00:07<00:12,  3.57it/s]Capturing num tokens (num_tokens=2304 avail_mem=99.50 GB):  26%|██▌       | 15/58 [00:07<00:12,  3.57it/s] 

    Capturing num tokens (num_tokens=2304 avail_mem=99.50 GB):  28%|██▊       | 16/58 [00:07<00:10,  3.88it/s]Capturing num tokens (num_tokens=2048 avail_mem=100.26 GB):  28%|██▊       | 16/58 [00:07<00:10,  3.88it/s]Capturing num tokens (num_tokens=2048 avail_mem=100.26 GB):  29%|██▉       | 17/58 [00:08<00:09,  4.23it/s]Capturing num tokens (num_tokens=1792 avail_mem=99.55 GB):  29%|██▉       | 17/58 [00:08<00:09,  4.23it/s] 

    Capturing num tokens (num_tokens=1792 avail_mem=99.55 GB):  31%|███       | 18/58 [00:08<00:08,  4.50it/s]Capturing num tokens (num_tokens=1536 avail_mem=99.54 GB):  31%|███       | 18/58 [00:08<00:08,  4.50it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=99.54 GB):  33%|███▎      | 19/58 [00:08<00:08,  4.63it/s]Capturing num tokens (num_tokens=1280 avail_mem=100.25 GB):  33%|███▎      | 19/58 [00:08<00:08,  4.63it/s]Capturing num tokens (num_tokens=1280 avail_mem=100.25 GB):  34%|███▍      | 20/58 [00:08<00:07,  5.08it/s]Capturing num tokens (num_tokens=1024 avail_mem=99.59 GB):  34%|███▍      | 20/58 [00:08<00:07,  5.08it/s] 

    Capturing num tokens (num_tokens=1024 avail_mem=99.59 GB):  36%|███▌      | 21/58 [00:08<00:06,  5.67it/s]Capturing num tokens (num_tokens=960 avail_mem=100.23 GB):  36%|███▌      | 21/58 [00:08<00:06,  5.67it/s]Capturing num tokens (num_tokens=960 avail_mem=100.23 GB):  38%|███▊      | 22/58 [00:08<00:05,  6.33it/s]Capturing num tokens (num_tokens=896 avail_mem=99.64 GB):  38%|███▊      | 22/58 [00:08<00:05,  6.33it/s] 

    Capturing num tokens (num_tokens=896 avail_mem=99.64 GB):  40%|███▉      | 23/58 [00:09<00:05,  6.58it/s]Capturing num tokens (num_tokens=832 avail_mem=99.64 GB):  40%|███▉      | 23/58 [00:09<00:05,  6.58it/s]Capturing num tokens (num_tokens=832 avail_mem=99.64 GB):  41%|████▏     | 24/58 [00:09<00:04,  7.25it/s]Capturing num tokens (num_tokens=768 avail_mem=100.24 GB):  41%|████▏     | 24/58 [00:09<00:04,  7.25it/s]

    Capturing num tokens (num_tokens=768 avail_mem=100.24 GB):  43%|████▎     | 25/58 [00:09<00:04,  7.46it/s]Capturing num tokens (num_tokens=704 avail_mem=99.68 GB):  43%|████▎     | 25/58 [00:09<00:04,  7.46it/s] Capturing num tokens (num_tokens=704 avail_mem=99.68 GB):  45%|████▍     | 26/58 [00:09<00:04,  7.82it/s]Capturing num tokens (num_tokens=640 avail_mem=100.25 GB):  45%|████▍     | 26/58 [00:09<00:04,  7.82it/s]

    Capturing num tokens (num_tokens=640 avail_mem=100.25 GB):  47%|████▋     | 27/58 [00:09<00:04,  7.28it/s]Capturing num tokens (num_tokens=576 avail_mem=99.72 GB):  47%|████▋     | 27/58 [00:09<00:04,  7.28it/s] Capturing num tokens (num_tokens=576 avail_mem=99.72 GB):  48%|████▊     | 28/58 [00:09<00:04,  7.47it/s]Capturing num tokens (num_tokens=512 avail_mem=99.71 GB):  48%|████▊     | 28/58 [00:09<00:04,  7.47it/s]Capturing num tokens (num_tokens=480 avail_mem=100.20 GB):  48%|████▊     | 28/58 [00:09<00:04,  7.47it/s]

    Capturing num tokens (num_tokens=480 avail_mem=100.20 GB):  52%|█████▏    | 30/58 [00:09<00:03,  8.82it/s]Capturing num tokens (num_tokens=448 avail_mem=99.76 GB):  52%|█████▏    | 30/58 [00:09<00:03,  8.82it/s] Capturing num tokens (num_tokens=448 avail_mem=99.76 GB):  53%|█████▎    | 31/58 [00:09<00:03,  8.35it/s]Capturing num tokens (num_tokens=416 avail_mem=100.17 GB):  53%|█████▎    | 31/58 [00:09<00:03,  8.35it/s]

    Capturing num tokens (num_tokens=416 avail_mem=100.17 GB):  55%|█████▌    | 32/58 [00:10<00:03,  8.60it/s]Capturing num tokens (num_tokens=384 avail_mem=99.77 GB):  55%|█████▌    | 32/58 [00:10<00:03,  8.60it/s] Capturing num tokens (num_tokens=352 avail_mem=100.18 GB):  55%|█████▌    | 32/58 [00:10<00:03,  8.60it/s]Capturing num tokens (num_tokens=352 avail_mem=100.18 GB):  59%|█████▊    | 34/58 [00:10<00:02,  9.57it/s]Capturing num tokens (num_tokens=320 avail_mem=99.79 GB):  59%|█████▊    | 34/58 [00:10<00:02,  9.57it/s] 

    Capturing num tokens (num_tokens=288 avail_mem=100.17 GB):  59%|█████▊    | 34/58 [00:10<00:02,  9.57it/s]Capturing num tokens (num_tokens=288 avail_mem=100.17 GB):  62%|██████▏   | 36/58 [00:10<00:02, 10.56it/s]Capturing num tokens (num_tokens=256 avail_mem=99.79 GB):  62%|██████▏   | 36/58 [00:10<00:02, 10.56it/s] Capturing num tokens (num_tokens=240 avail_mem=100.15 GB):  62%|██████▏   | 36/58 [00:10<00:02, 10.56it/s]

    Capturing num tokens (num_tokens=240 avail_mem=100.15 GB):  66%|██████▌   | 38/58 [00:10<00:01, 11.53it/s]Capturing num tokens (num_tokens=224 avail_mem=99.81 GB):  66%|██████▌   | 38/58 [00:10<00:01, 11.53it/s] Capturing num tokens (num_tokens=208 avail_mem=100.13 GB):  66%|██████▌   | 38/58 [00:10<00:01, 11.53it/s]Capturing num tokens (num_tokens=208 avail_mem=100.13 GB):  69%|██████▉   | 40/58 [00:10<00:01, 12.06it/s]Capturing num tokens (num_tokens=192 avail_mem=99.82 GB):  69%|██████▉   | 40/58 [00:10<00:01, 12.06it/s] 

    Capturing num tokens (num_tokens=176 avail_mem=100.12 GB):  69%|██████▉   | 40/58 [00:10<00:01, 12.06it/s]Capturing num tokens (num_tokens=176 avail_mem=100.12 GB):  72%|███████▏  | 42/58 [00:10<00:01, 12.60it/s]Capturing num tokens (num_tokens=160 avail_mem=99.84 GB):  72%|███████▏  | 42/58 [00:10<00:01, 12.60it/s] Capturing num tokens (num_tokens=144 avail_mem=100.10 GB):  72%|███████▏  | 42/58 [00:10<00:01, 12.60it/s]Capturing num tokens (num_tokens=144 avail_mem=100.10 GB):  76%|███████▌  | 44/58 [00:11<00:01, 13.06it/s]Capturing num tokens (num_tokens=128 avail_mem=99.86 GB):  76%|███████▌  | 44/58 [00:11<00:01, 13.06it/s] 

    Capturing num tokens (num_tokens=112 avail_mem=100.10 GB):  76%|███████▌  | 44/58 [00:11<00:01, 13.06it/s]Capturing num tokens (num_tokens=112 avail_mem=100.10 GB):  79%|███████▉  | 46/58 [00:11<00:00, 13.44it/s]Capturing num tokens (num_tokens=96 avail_mem=99.87 GB):  79%|███████▉  | 46/58 [00:11<00:00, 13.44it/s]  Capturing num tokens (num_tokens=80 avail_mem=100.08 GB):  79%|███████▉  | 46/58 [00:11<00:00, 13.44it/s]Capturing num tokens (num_tokens=80 avail_mem=100.08 GB):  83%|████████▎ | 48/58 [00:11<00:00, 14.49it/s]Capturing num tokens (num_tokens=64 avail_mem=100.11 GB):  83%|████████▎ | 48/58 [00:11<00:00, 14.49it/s]

    Capturing num tokens (num_tokens=48 avail_mem=100.03 GB):  83%|████████▎ | 48/58 [00:11<00:00, 14.49it/s]Capturing num tokens (num_tokens=32 avail_mem=100.07 GB):  83%|████████▎ | 48/58 [00:11<00:00, 14.49it/s]Capturing num tokens (num_tokens=32 avail_mem=100.07 GB):  88%|████████▊ | 51/58 [00:11<00:00, 16.57it/s]Capturing num tokens (num_tokens=28 avail_mem=99.93 GB):  88%|████████▊ | 51/58 [00:11<00:00, 16.57it/s] 

    Capturing num tokens (num_tokens=24 avail_mem=100.07 GB):  88%|████████▊ | 51/58 [00:11<00:00, 16.57it/s]Capturing num tokens (num_tokens=24 avail_mem=100.07 GB):  91%|█████████▏| 53/58 [00:11<00:00, 14.94it/s]Capturing num tokens (num_tokens=20 avail_mem=100.06 GB):  91%|█████████▏| 53/58 [00:11<00:00, 14.94it/s]Capturing num tokens (num_tokens=16 avail_mem=100.05 GB):  91%|█████████▏| 53/58 [00:11<00:00, 14.94it/s]Capturing num tokens (num_tokens=12 avail_mem=100.05 GB):  91%|█████████▏| 53/58 [00:11<00:00, 14.94it/s]Capturing num tokens (num_tokens=12 avail_mem=100.05 GB):  97%|█████████▋| 56/58 [00:11<00:00, 16.76it/s]Capturing num tokens (num_tokens=8 avail_mem=99.95 GB):  97%|█████████▋| 56/58 [00:11<00:00, 16.76it/s]  

    Capturing num tokens (num_tokens=4 avail_mem=99.95 GB):  97%|█████████▋| 56/58 [00:11<00:00, 16.76it/s]Capturing num tokens (num_tokens=4 avail_mem=99.95 GB): 100%|██████████| 58/58 [00:11<00:00,  4.91it/s]


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


<strong style='color: #00008B;'>reasoing_content: Okay, so the user is in New York and wants the current date and time along with the weather. I need to figure out how to get this information using the functions provided.<br><br>First, I should use the get_current_date function. The required parameter is timezone, which is 'America/New_York'. I'll structure the function call with that parameter.<br><br>Next, for the weather, I'll use get_current_weather. The parameters needed are city, state, and unit. The city is 'New York', the state is 'NY', and the unit should be 'fahrenheit' since the user didn't specify otherwise.<br><br>I need to make sure each function call is separate, as per the instructions. So I'll call get_current_date first with the timezone, then get_current_weather with the specified parameters.<br><br>I should also remember to include the sources in the response. Using the functions means the data comes from there, so I'll note that in the reminder section.<br><br>Putting it all together, I'll format each function call correctly, ensuring the parameters are in JSON format within the function syntax. That should cover everything the user is asking for.<br><br><br>content: <function=get_current_date>{"timezone": "America/New_York"}</function><br><function=get_current_weather>{"city": "New York", "state": "NY", "unit": "fahrenheit"}</function><br><br>Reminder: The data was obtained using the get_current_date and get_current_weather functions.</strong>


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

    {'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. I know that Paris is the capital, so that\'s straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I\'m not sure of the exact number. I think it\'s around 2 million, but I should verify that.\n\nWait, maybe I should check the latest statistics. I recall that in recent years, Paris has seen a slight increase. Let me think... I believe the population is approximately 2,150,000 as of 2023. That seems about right, but I\'m not 100% certain. I should make sure to present this information accurately.\n\nNext, I need to structure this into a JSON format. JSON requires key-value pairs, so I\'ll create an object with keys like "city", "population", and maybe "country" for context. The city is Paris, the population is 2,150,000, and the country is France.\n\nI should also consider if the user might need more details, like the exact year of the population figure. Including that could be helpful, so I\'ll add "year": 2023. That way, the user knows the data is up to date.\n\nPutting it all together, the JSON should look clean and well-structured. I\'ll make sure the syntax is correct, with proper commas and quotation marks. No markdown, just plain JSON.\n\nI think that\'s all. The user probably just needs the information quickly, so keeping it concise is key. I\'ll present the JSON without any extra fluff.\n</think>{\n\n"name": "Paris",\n"population": 21500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1265, 1477, 279, 1429, 3213, 7042, 821, 13, 358, 6099, 429, 279, 7042, 315, 12095, 702, 1012, 7826, 11, 714, 358, 2776, 537, 2704, 315, 279, 4734, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 10146, 429, 382, 14190, 11, 7196, 358, 1265, 1779, 279, 5535, 13142, 13, 358, 19091, 429, 304, 3213, 1635, 11, 12095, 702, 3884, 264, 8112, 5263, 13, 6771, 752, 1744, 1112, 358, 4411, 279, 7042, 374, 13187, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 438, 315, 220, 17, 15, 17, 18, 13, 2938, 4977, 911, 1290, 11, 714, 358, 2776, 537, 220, 16, 15, 15, 4, 3654, 13, 358, 1265, 1281, 2704, 311, 3042, 419, 1995, 29257, 382, 5847, 11, 358, 1184, 311, 5944, 419, 1119, 264, 4718, 3561, 13, 4718, 7460, 1376, 19083, 13530, 11, 773, 358, 3278, 1855, 458, 1633, 448, 6894, 1075, 330, 8926, 497, 330, 44441, 497, 323, 7196, 330, 11141, 1, 369, 2266, 13, 576, 3283, 374, 12095, 11, 279, 7042, 374, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 11, 323, 279, 3146, 374, 9625, 382, 40, 1265, 1083, 2908, 421, 279, 1196, 2578, 1184, 803, 3565, 11, 1075, 279, 4734, 1042, 315, 279, 7042, 7071, 13, 55121, 429, 1410, 387, 10950, 11, 773, 358, 3278, 912, 330, 3157, 788, 220, 17, 15, 17, 18, 13, 2938, 1616, 11, 279, 1196, 8788, 279, 821, 374, 705, 311, 2400, 382, 97904, 432, 678, 3786, 11, 279, 4718, 1265, 1401, 4240, 323, 1632, 12, 51143, 13, 358, 3278, 1281, 2704, 279, 19482, 374, 4396, 11, 448, 6169, 76602, 323, 54231, 15423, 13, 2308, 50494, 11, 1101, 14396, 4718, 382, 40, 1744, 429, 594, 678, 13, 576, 1196, 4658, 1101, 3880, 279, 1995, 6157, 11, 773, 10282, 432, 63594, 374, 1376, 13, 358, 3278, 3042, 279, 4718, 2041, 894, 4960, 1320, 1362, 624, 151649, 4257, 1, 606, 788, 330, 59604, 756, 1, 44441, 788, 220, 17, 16, 20, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15], 'meta_info': {'id': 'b6cdbc06716c4e31bee7b359c21ebc0b', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': {'device': 1, 'host': 0}, 'dp_rank': None, 'e2e_latency': 24.393073289189488, 'response_sent_to_client_ts': 1775186496.6645987}}



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


<strong style='color: #00008B;'>{'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. I know that Paris is the capital, so that\'s straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I\'m not exactly sure of the current number. I think it\'s around 2 million, but I should verify that.\n\nWait, maybe I should check some reliable sources to confirm the population. I recall that the population figures can vary depending on the source and the year. For example, the 2020 census might have a slightly different number than the 2021 estimate. I think the population was around 2,165,000 in 2021, but I\'m not 100% certain. I should make sure to use the most accurate and up-to-date information.\n\nAlso, the user wants the information in JSON format. JSON is a data interchange format, so I\'ll need to structure the data accordingly. I should include the city name, population, and maybe the year of the data. It\'s important to present the information clearly and accurately, so I\'ll double-check the numbers to avoid any mistakes.\n\nI should also consider if there are any other relevant details the user might find useful, like the area of the city or some key facts about it. But since the user specifically asked for population, I\'ll focus on that. Maybe adding a note about the population figure being approximate would be helpful, just in case.\n\nPutting it all together, I\'ll structure the JSON with the city name, population, and the year. I\'ll make sure the syntax is correct, using quotation marks and commas appropriately. I\'ll also keep the language clear and straightforward so that the user can easily understand the information.\n\nFinally, I\'ll review the JSON to ensure there are no errors and that the data is accurate. This way, the user gets a reliable and well-formatted response to their query.\n</think>{"name": "Paris", "population": 2165000}', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1265, 1477, 279, 1429, 3213, 7042, 821, 13, 358, 6099, 429, 279, 7042, 315, 12095, 702, 1012, 7826, 11, 714, 358, 2776, 537, 6896, 2704, 315, 279, 1482, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 10146, 429, 382, 14190, 11, 7196, 358, 1265, 1779, 1045, 14720, 8173, 311, 7683, 279, 7042, 13, 358, 19091, 429, 279, 7042, 12396, 646, 13289, 11649, 389, 279, 2530, 323, 279, 1042, 13, 1752, 3110, 11, 279, 220, 17, 15, 17, 15, 43602, 2578, 614, 264, 10078, 2155, 1372, 1091, 279, 220, 17, 15, 17, 16, 16045, 13, 358, 1744, 279, 7042, 572, 2163, 220, 17, 11, 16, 21, 20, 11, 15, 15, 15, 304, 220, 17, 15, 17, 16, 11, 714, 358, 2776, 537, 220, 16, 15, 15, 4, 3654, 13, 358, 1265, 1281, 2704, 311, 990, 279, 1429, 13382, 323, 705, 4686, 18413, 1995, 382, 13394, 11, 279, 1196, 6801, 279, 1995, 304, 4718, 3561, 13, 4718, 374, 264, 821, 51263, 3561, 11, 773, 358, 3278, 1184, 311, 5944, 279, 821, 27079, 13, 358, 1265, 2924, 279, 3283, 829, 11, 7042, 11, 323, 7196, 279, 1042, 315, 279, 821, 13, 1084, 594, 2989, 311, 3042, 279, 1995, 9355, 323, 29257, 11, 773, 358, 3278, 1990, 15934, 279, 5109, 311, 5648, 894, 20643, 382, 40, 1265, 1083, 2908, 421, 1052, 525, 894, 1008, 9760, 3565, 279, 1196, 2578, 1477, 5390, 11, 1075, 279, 3082, 315, 279, 3283, 476, 1045, 1376, 13064, 911, 432, 13, 1988, 2474, 279, 1196, 11689, 4588, 369, 7042, 11, 358, 3278, 5244, 389, 429, 13, 10696, 7842, 264, 5185, 911, 279, 7042, 7071, 1660, 44868, 1035, 387, 10950, 11, 1101, 304, 1142, 382, 97904, 432, 678, 3786, 11, 358, 3278, 5944, 279, 4718, 448, 279, 3283, 829, 11, 7042, 11, 323, 279, 1042, 13, 358, 3278, 1281, 2704, 279, 19482, 374, 4396, 11, 1667, 54231, 15423, 323, 76602, 34901, 13, 358, 3278, 1083, 2506, 279, 4128, 2797, 323, 30339, 773, 429, 279, 1196, 646, 6707, 3535, 279, 1995, 382, 23949, 11, 358, 3278, 3395, 279, 4718, 311, 5978, 1052, 525, 902, 5975, 323, 429, 279, 821, 374, 13382, 13, 1096, 1616, 11, 279, 1196, 5221, 264, 14720, 323, 1632, 8460, 12127, 2033, 311, 862, 3239, 624, 151649, 4913, 606, 788, 330, 59604, 497, 330, 44441, 788, 220, 17, 16, 21, 20, 15, 15, 15, 92, 151643], 'meta_info': {'id': '3dc17cc015ab4cf3a54dc372140fec48', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 447, 'cached_tokens': 22, 'cached_tokens_details': {'device': 22, 'host': 0}, 'dp_rank': None, 'e2e_latency': 6.064145969692618, 'response_sent_to_client_ts': 1775186502.7418125}}</strong>


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

    [{'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '11cd7fb8ffb041b183b2c99206309b5c', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': {'device': 10, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.14977630181238055, 'response_sent_to_client_ts': 1775186502.948503}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '6311f555ba3e4a5cbfe9b27d279b222d', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': {'device': 10, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.14966298267245293, 'response_sent_to_client_ts': 1775186502.9485157}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': 'fdf5b4aa892042e186c678f1c5fe934d', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': {'device': 10, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.14961900794878602, 'response_sent_to_client_ts': 1775186502.948521}}]


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

    {'text': ' France, and the \n\\( n \\)  \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\(', 'output_ids': [9625, 11, 323, 279, 220, 198, 44292, 308, 1124, 8, 220, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767], 'meta_info': {'id': '846e4f44a412460a90ef75e68174f7a6', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 6, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': {'device': 1, 'host': 0}, 'dp_rank': None, 'e2e_latency': 19.62658472592011, 'response_sent_to_client_ts': 1775186522.5831897}}


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


<strong style='color: #00008B;'>{'text': 'Alright, so the user is asking for the information and population of the capital of France in JSON format. Okay, let\'s break this down. \n\nFirst, I need to figure out what exactly they\'re asking for. The capital of France is Paris, so that\'s clear. Now, they want a JSON format, which is a data structure used to store information. I know JSON stands for JavaScript Object Notation, but in this context, it\'s just a way to present the data clearly.\n\nSo, I should gather the necessary details about Paris. The population is a key piece of information. I remember that as of recent estimates, Paris has a population around 2 million. But I should check that to make sure it\'s up to date. Maybe the latest data puts it slightly higher, like 2,150,000 or something. Also, it\'s important to specify that this is an approximate figure because population figures can change over time.\n\nNext, the location of the capital. Paris is located in Île de凡ny, which is one of the more populated islands in the city. It\'s situated in the northern part of the Seine River, so that geographical detail might be useful for someone looking up more information about the area.\n\nI need to present this information in a clear JSON structure. Typically, JSON uses key-value pairs. So, I\'ll set up a main object with keys like "capital" and "population". Under "capital", I\'ll include both the name and the location. For "population", I\'ll provide the number along with a note explaining that it\'s approximate.\n\nI should also consider the user\'s intent. They might be looking for a structured data format to use in another application or for a report where having the data organized neatly would be beneficial. By providing the JSON format, they get a standardized way to access the information, which is often useful for APIs or automated data processing.\n\nAdditionally, I might think about including more data points, like the administrative region or some key landmarks, but the user specifically asked for population and information about the capital. So sticking to the essentials is probably best here.\n\nFinally, I\'ll make sure the JSON is properly formatted and that there are no syntax errors. Using quotation marks around the keys and values, and commas to separate the items. Proper indentation can make it more readable, so I\'ll structure it that way.\n</think>\n\nHere is the information about the capital of France in JSON format:\n\n```json\n{\n  "capital": {\n    "name": "Paris",\n    "location": "Île de la Cité, Nord-Est de la France",\n    "region": "Seine River, northern France"\n  },\n  "population": {\n    "number": 2150000,\n    "note": "An estimate as of the latest available data"\n  }\n}\n```', 'output_ids': [71486, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 35439, 11, 1077, 594, 1438, 419, 1495, 13, 4710, 5338, 11, 358, 1184, 311, 7071, 700, 1128, 6896, 807, 2299, 10161, 369, 13, 576, 6722, 315, 9625, 374, 12095, 11, 773, 429, 594, 2797, 13, 4695, 11, 807, 1366, 264, 4718, 3561, 11, 892, 374, 264, 821, 5944, 1483, 311, 3553, 1995, 13, 358, 1414, 4718, 13352, 369, 12914, 3002, 2806, 367, 11, 714, 304, 419, 2266, 11, 432, 594, 1101, 264, 1616, 311, 3042, 279, 821, 9355, 382, 4416, 11, 358, 1265, 9567, 279, 5871, 3565, 911, 12095, 13, 576, 7042, 374, 264, 1376, 6573, 315, 1995, 13, 358, 6099, 429, 438, 315, 3213, 17530, 11, 12095, 702, 264, 7042, 2163, 220, 17, 3526, 13, 1988, 358, 1265, 1779, 429, 311, 1281, 2704, 432, 594, 705, 311, 2400, 13, 10696, 279, 5535, 821, 9521, 432, 10078, 5080, 11, 1075, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 476, 2494, 13, 7281, 11, 432, 594, 2989, 311, 13837, 429, 419, 374, 458, 44868, 7071, 1576, 7042, 12396, 646, 2297, 916, 882, 382, 5847, 11, 279, 3728, 315, 279, 6722, 13, 12095, 374, 7407, 304, 59108, 273, 409, 100216, 3834, 11, 892, 374, 825, 315, 279, 803, 34359, 29000, 304, 279, 3283, 13, 1084, 594, 30083, 304, 279, 18172, 949, 315, 279, 1345, 482, 10948, 11, 773, 429, 52901, 7716, 2578, 387, 5390, 369, 4325, 3330, 705, 803, 1995, 911, 279, 3082, 382, 40, 1184, 311, 3042, 419, 1995, 304, 264, 2797, 4718, 5944, 13, 45302, 11, 4718, 5711, 1376, 19083, 13530, 13, 2055, 11, 358, 3278, 738, 705, 264, 1887, 1633, 448, 6894, 1075, 330, 65063, 1, 323, 330, 44441, 3263, 9449, 330, 65063, 497, 358, 3278, 2924, 2176, 279, 829, 323, 279, 3728, 13, 1752, 330, 44441, 497, 358, 3278, 3410, 279, 1372, 3156, 448, 264, 5185, 25021, 429, 432, 594, 44868, 382, 40, 1265, 1083, 2908, 279, 1196, 594, 7385, 13, 2379, 2578, 387, 3330, 369, 264, 32930, 821, 3561, 311, 990, 304, 2441, 3766, 476, 369, 264, 1895, 1380, 3432, 279, 821, 16645, 62166, 1035, 387, 23699, 13, 3216, 8241, 279, 4718, 3561, 11, 807, 633, 264, 50014, 1616, 311, 2615, 279, 1995, 11, 892, 374, 3545, 5390, 369, 33356, 476, 27498, 821, 8692, 382, 49574, 11, 358, 2578, 1744, 911, 2670, 803, 821, 3501, 11, 1075, 279, 22707, 5537, 476, 1045, 1376, 59924, 11, 714, 279, 1196, 11689, 4588, 369, 7042, 323, 1995, 911, 279, 6722, 13, 2055, 36972, 311, 279, 58786, 374, 4658, 1850, 1588, 382, 23949, 11, 358, 3278, 1281, 2704, 279, 4718, 374, 10277, 23126, 323, 429, 1052, 525, 902, 19482, 5975, 13, 12091, 54231, 15423, 2163, 279, 6894, 323, 2750, 11, 323, 76602, 311, 8651, 279, 3589, 13, 64558, 69592, 646, 1281, 432, 803, 33798, 11, 773, 358, 3278, 5944, 432, 429, 1616, 624, 151649, 271, 8420, 374, 279, 1995, 911, 279, 6722, 315, 9625, 304, 4718, 3561, 1447, 73594, 2236, 198, 515, 220, 330, 65063, 788, 341, 262, 330, 606, 788, 330, 59604, 756, 262, 330, 2527, 788, 330, 71807, 273, 409, 1187, 356, 12815, 11, 29181, 13431, 267, 409, 1187, 9625, 756, 262, 330, 3943, 788, 330, 1514, 482, 10948, 11, 18172, 9625, 698, 220, 1153, 220, 330, 44441, 788, 341, 262, 330, 4082, 788, 220, 17, 16, 20, 15, 15, 15, 15, 345, 262, 330, 9974, 788, 330, 2082, 16045, 438, 315, 279, 5535, 2500, 821, 698, 220, 456, 532, 73594, 151643], 'meta_info': {'id': '709b494aded3411a882e1a36722a6f75', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 586, 'cached_tokens': 22, 'cached_tokens_details': {'device': 22, 'host': 0}, 'dp_rank': None, 'e2e_latency': 5.627363855019212, 'response_sent_to_client_ts': 1775186528.2216775}}</strong>



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


    2026-04-03 03:22:21.382 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 03:22:21] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 03:22:21.382 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 03:22:21] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 03:22:21.382 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 03:22:21] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 03:22:21.382 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 03:22:21] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 03:22:21.383 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 03:22:21] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Multi-thread loading shards:  50% Completed | 1/2 [00:01<00:01,  1.21s/it]

    Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.26s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.25s/it]


    2026-04-03 03:22:28,312 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 03:22:28] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:06<06:26,  6.79s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:06<06:26,  6.79s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:07<02:49,  3.03s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:07<02:49,  3.03s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:07<01:35,  1.73s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:07<01:35,  1.73s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:07<01:00,  1.12s/it]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:07<01:00,  1.12s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:07<00:41,  1.29it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:07<00:41,  1.29it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:07<00:29,  1.78it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:07<00:29,  1.78it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:08<00:21,  2.37it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:08<00:21,  2.37it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:08<00:16,  3.05it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:08<00:16,  3.05it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:08<00:12,  3.85it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:08<00:12,  3.85it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:08<00:10,  4.74it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:08<00:10,  4.74it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:08<00:10,  4.74it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:08<00:07,  6.51it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:08<00:07,  6.51it/s]

    Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:08<00:07,  6.51it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:08<00:05,  8.24it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:08<00:05,  8.24it/s]Compiling num tokens (num_tokens=2304):  24%|██▍       | 14/58 [00:08<00:05,  8.24it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:08<00:04,  9.94it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:08<00:04,  9.94it/s]

    Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:08<00:04,  9.94it/s]Compiling num tokens (num_tokens=1536):  28%|██▊       | 16/58 [00:08<00:04,  9.94it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:08<00:03, 12.94it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:08<00:03, 12.94it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:08<00:03, 12.94it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:09<00:03, 12.94it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:09<00:03, 12.94it/s]

    Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:09<00:01, 17.62it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:09<00:01, 17.62it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:09<00:01, 17.62it/s]Compiling num tokens (num_tokens=704):  40%|███▉      | 23/58 [00:09<00:01, 17.62it/s]Compiling num tokens (num_tokens=640):  40%|███▉      | 23/58 [00:09<00:01, 17.62it/s]Compiling num tokens (num_tokens=576):  40%|███▉      | 23/58 [00:09<00:01, 17.62it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:09<00:01, 23.83it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:09<00:01, 23.83it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:09<00:01, 23.83it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:09<00:01, 23.83it/s]Compiling num tokens (num_tokens=416):  48%|████▊     | 28/58 [00:09<00:01, 23.83it/s]Compiling num tokens (num_tokens=384):  48%|████▊     | 28/58 [00:09<00:01, 23.83it/s]

    Compiling num tokens (num_tokens=352):  48%|████▊     | 28/58 [00:09<00:01, 23.83it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:09<00:00, 31.11it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:09<00:00, 31.11it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:09<00:00, 31.11it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:09<00:00, 31.11it/s]Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:09<00:00, 31.11it/s]Compiling num tokens (num_tokens=224):  59%|█████▊    | 34/58 [00:09<00:00, 31.11it/s]Compiling num tokens (num_tokens=208):  59%|█████▊    | 34/58 [00:09<00:00, 31.11it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:09<00:00, 36.91it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:09<00:00, 36.91it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:09<00:00, 36.91it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:09<00:00, 36.91it/s]Compiling num tokens (num_tokens=144):  69%|██████▉   | 40/58 [00:09<00:00, 36.91it/s]

    Compiling num tokens (num_tokens=128):  69%|██████▉   | 40/58 [00:09<00:00, 36.91it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:09<00:00, 39.83it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:09<00:00, 39.83it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:09<00:00, 39.83it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:09<00:00, 39.83it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:09<00:00, 39.83it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:09<00:00, 39.83it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:09<00:00, 39.83it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:09<00:00, 43.76it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:09<00:00, 43.76it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:09<00:00, 43.76it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:09<00:00, 43.76it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:09<00:00, 43.76it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:09<00:00, 43.76it/s]

    Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:09<00:00, 43.76it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:09<00:00, 43.76it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:09<00:00,  5.96it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=75.55 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=75.55 GB):   2%|▏         | 1/58 [00:00<00:19,  2.86it/s]Capturing num tokens (num_tokens=7680 avail_mem=74.23 GB):   2%|▏         | 1/58 [00:00<00:19,  2.86it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=74.23 GB):   3%|▎         | 2/58 [00:00<00:19,  2.87it/s]Capturing num tokens (num_tokens=7168 avail_mem=73.98 GB):   3%|▎         | 2/58 [00:00<00:19,  2.87it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=73.98 GB):   5%|▌         | 3/58 [00:01<00:23,  2.35it/s]Capturing num tokens (num_tokens=6656 avail_mem=73.99 GB):   5%|▌         | 3/58 [00:01<00:23,  2.35it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=73.99 GB):   7%|▋         | 4/58 [00:01<00:23,  2.31it/s]Capturing num tokens (num_tokens=6144 avail_mem=73.99 GB):   7%|▋         | 4/58 [00:01<00:23,  2.31it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=73.99 GB):   9%|▊         | 5/58 [00:02<00:22,  2.33it/s]Capturing num tokens (num_tokens=5632 avail_mem=74.00 GB):   9%|▊         | 5/58 [00:02<00:22,  2.33it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=74.00 GB):  10%|█         | 6/58 [00:02<00:21,  2.43it/s]Capturing num tokens (num_tokens=5120 avail_mem=74.00 GB):  10%|█         | 6/58 [00:02<00:21,  2.43it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=74.00 GB):  12%|█▏        | 7/58 [00:02<00:20,  2.55it/s]Capturing num tokens (num_tokens=4608 avail_mem=74.00 GB):  12%|█▏        | 7/58 [00:02<00:20,  2.55it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=74.00 GB):  14%|█▍        | 8/58 [00:03<00:19,  2.56it/s]Capturing num tokens (num_tokens=4096 avail_mem=74.01 GB):  14%|█▍        | 8/58 [00:03<00:19,  2.56it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=74.01 GB):  16%|█▌        | 9/58 [00:03<00:17,  2.79it/s]Capturing num tokens (num_tokens=3840 avail_mem=74.28 GB):  16%|█▌        | 9/58 [00:03<00:17,  2.79it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=74.28 GB):  17%|█▋        | 10/58 [00:03<00:15,  3.05it/s]Capturing num tokens (num_tokens=3584 avail_mem=74.49 GB):  17%|█▋        | 10/58 [00:03<00:15,  3.05it/s]Capturing num tokens (num_tokens=3584 avail_mem=74.49 GB):  19%|█▉        | 11/58 [00:03<00:12,  3.71it/s]Capturing num tokens (num_tokens=3328 avail_mem=74.46 GB):  19%|█▉        | 11/58 [00:03<00:12,  3.71it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=74.46 GB):  21%|██        | 12/58 [00:04<00:10,  4.45it/s]Capturing num tokens (num_tokens=3072 avail_mem=74.45 GB):  21%|██        | 12/58 [00:04<00:10,  4.45it/s]Capturing num tokens (num_tokens=3072 avail_mem=74.45 GB):  22%|██▏       | 13/58 [00:04<00:08,  5.33it/s]Capturing num tokens (num_tokens=2816 avail_mem=74.45 GB):  22%|██▏       | 13/58 [00:04<00:08,  5.33it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=74.45 GB):  24%|██▍       | 14/58 [00:04<00:07,  5.86it/s]Capturing num tokens (num_tokens=2560 avail_mem=74.45 GB):  24%|██▍       | 14/58 [00:04<00:07,  5.86it/s]Capturing num tokens (num_tokens=2560 avail_mem=74.45 GB):  26%|██▌       | 15/58 [00:04<00:06,  6.58it/s]Capturing num tokens (num_tokens=2304 avail_mem=73.94 GB):  26%|██▌       | 15/58 [00:04<00:06,  6.58it/s]Capturing num tokens (num_tokens=2048 avail_mem=73.94 GB):  26%|██▌       | 15/58 [00:04<00:06,  6.58it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=73.94 GB):  29%|██▉       | 17/58 [00:04<00:04,  8.57it/s]Capturing num tokens (num_tokens=1792 avail_mem=73.94 GB):  29%|██▉       | 17/58 [00:04<00:04,  8.57it/s]Capturing num tokens (num_tokens=1536 avail_mem=73.94 GB):  29%|██▉       | 17/58 [00:04<00:04,  8.57it/s]Capturing num tokens (num_tokens=1536 avail_mem=73.94 GB):  33%|███▎      | 19/58 [00:04<00:03, 10.26it/s]Capturing num tokens (num_tokens=1280 avail_mem=73.94 GB):  33%|███▎      | 19/58 [00:04<00:03, 10.26it/s]Capturing num tokens (num_tokens=1024 avail_mem=73.94 GB):  33%|███▎      | 19/58 [00:04<00:03, 10.26it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=73.94 GB):  36%|███▌      | 21/58 [00:04<00:03, 11.64it/s]Capturing num tokens (num_tokens=960 avail_mem=73.94 GB):  36%|███▌      | 21/58 [00:04<00:03, 11.64it/s] Capturing num tokens (num_tokens=896 avail_mem=105.93 GB):  36%|███▌      | 21/58 [00:04<00:03, 11.64it/s]

    Capturing num tokens (num_tokens=896 avail_mem=105.93 GB):  40%|███▉      | 23/58 [00:04<00:03, 10.59it/s]Capturing num tokens (num_tokens=832 avail_mem=105.93 GB):  40%|███▉      | 23/58 [00:04<00:03, 10.59it/s]Capturing num tokens (num_tokens=768 avail_mem=105.93 GB):  40%|███▉      | 23/58 [00:05<00:03, 10.59it/s]Capturing num tokens (num_tokens=704 avail_mem=105.92 GB):  40%|███▉      | 23/58 [00:05<00:03, 10.59it/s]Capturing num tokens (num_tokens=704 avail_mem=105.92 GB):  45%|████▍     | 26/58 [00:05<00:02, 14.29it/s]Capturing num tokens (num_tokens=640 avail_mem=105.92 GB):  45%|████▍     | 26/58 [00:05<00:02, 14.29it/s]Capturing num tokens (num_tokens=576 avail_mem=105.92 GB):  45%|████▍     | 26/58 [00:05<00:02, 14.29it/s]Capturing num tokens (num_tokens=512 avail_mem=105.91 GB):  45%|████▍     | 26/58 [00:05<00:02, 14.29it/s]

    Capturing num tokens (num_tokens=512 avail_mem=105.91 GB):  50%|█████     | 29/58 [00:05<00:01, 17.57it/s]Capturing num tokens (num_tokens=480 avail_mem=105.91 GB):  50%|█████     | 29/58 [00:05<00:01, 17.57it/s]Capturing num tokens (num_tokens=448 avail_mem=105.91 GB):  50%|█████     | 29/58 [00:05<00:01, 17.57it/s]Capturing num tokens (num_tokens=416 avail_mem=105.90 GB):  50%|█████     | 29/58 [00:05<00:01, 17.57it/s]Capturing num tokens (num_tokens=416 avail_mem=105.90 GB):  55%|█████▌    | 32/58 [00:05<00:01, 18.98it/s]Capturing num tokens (num_tokens=384 avail_mem=105.90 GB):  55%|█████▌    | 32/58 [00:05<00:01, 18.98it/s]Capturing num tokens (num_tokens=352 avail_mem=105.89 GB):  55%|█████▌    | 32/58 [00:05<00:01, 18.98it/s]

    Capturing num tokens (num_tokens=320 avail_mem=105.89 GB):  55%|█████▌    | 32/58 [00:05<00:01, 18.98it/s]Capturing num tokens (num_tokens=288 avail_mem=105.89 GB):  55%|█████▌    | 32/58 [00:05<00:01, 18.98it/s]Capturing num tokens (num_tokens=288 avail_mem=105.89 GB):  62%|██████▏   | 36/58 [00:05<00:00, 22.71it/s]Capturing num tokens (num_tokens=256 avail_mem=105.88 GB):  62%|██████▏   | 36/58 [00:05<00:00, 22.71it/s]Capturing num tokens (num_tokens=240 avail_mem=105.88 GB):  62%|██████▏   | 36/58 [00:05<00:00, 22.71it/s]Capturing num tokens (num_tokens=224 avail_mem=105.87 GB):  62%|██████▏   | 36/58 [00:05<00:00, 22.71it/s]Capturing num tokens (num_tokens=208 avail_mem=105.87 GB):  62%|██████▏   | 36/58 [00:05<00:00, 22.71it/s]Capturing num tokens (num_tokens=208 avail_mem=105.87 GB):  69%|██████▉   | 40/58 [00:05<00:00, 25.44it/s]Capturing num tokens (num_tokens=192 avail_mem=105.87 GB):  69%|██████▉   | 40/58 [00:05<00:00, 25.44it/s]

    Capturing num tokens (num_tokens=176 avail_mem=105.86 GB):  69%|██████▉   | 40/58 [00:05<00:00, 25.44it/s]Capturing num tokens (num_tokens=160 avail_mem=105.86 GB):  69%|██████▉   | 40/58 [00:05<00:00, 25.44it/s]Capturing num tokens (num_tokens=144 avail_mem=105.86 GB):  69%|██████▉   | 40/58 [00:05<00:00, 25.44it/s]Capturing num tokens (num_tokens=144 avail_mem=105.86 GB):  76%|███████▌  | 44/58 [00:05<00:00, 28.06it/s]Capturing num tokens (num_tokens=128 avail_mem=105.87 GB):  76%|███████▌  | 44/58 [00:05<00:00, 28.06it/s]Capturing num tokens (num_tokens=112 avail_mem=105.86 GB):  76%|███████▌  | 44/58 [00:05<00:00, 28.06it/s]Capturing num tokens (num_tokens=96 avail_mem=105.86 GB):  76%|███████▌  | 44/58 [00:05<00:00, 28.06it/s] Capturing num tokens (num_tokens=80 avail_mem=105.85 GB):  76%|███████▌  | 44/58 [00:05<00:00, 28.06it/s]

    Capturing num tokens (num_tokens=80 avail_mem=105.85 GB):  83%|████████▎ | 48/58 [00:05<00:00, 28.40it/s]Capturing num tokens (num_tokens=64 avail_mem=105.85 GB):  83%|████████▎ | 48/58 [00:05<00:00, 28.40it/s]Capturing num tokens (num_tokens=48 avail_mem=105.85 GB):  83%|████████▎ | 48/58 [00:05<00:00, 28.40it/s]Capturing num tokens (num_tokens=32 avail_mem=105.84 GB):  83%|████████▎ | 48/58 [00:05<00:00, 28.40it/s]Capturing num tokens (num_tokens=32 avail_mem=105.84 GB):  88%|████████▊ | 51/58 [00:05<00:00, 27.94it/s]Capturing num tokens (num_tokens=28 avail_mem=105.84 GB):  88%|████████▊ | 51/58 [00:05<00:00, 27.94it/s]Capturing num tokens (num_tokens=24 avail_mem=105.84 GB):  88%|████████▊ | 51/58 [00:05<00:00, 27.94it/s]Capturing num tokens (num_tokens=20 avail_mem=105.83 GB):  88%|████████▊ | 51/58 [00:06<00:00, 27.94it/s]

    Capturing num tokens (num_tokens=20 avail_mem=105.83 GB):  93%|█████████▎| 54/58 [00:06<00:00, 28.12it/s]Capturing num tokens (num_tokens=16 avail_mem=105.83 GB):  93%|█████████▎| 54/58 [00:06<00:00, 28.12it/s]Capturing num tokens (num_tokens=12 avail_mem=105.83 GB):  93%|█████████▎| 54/58 [00:06<00:00, 28.12it/s]Capturing num tokens (num_tokens=8 avail_mem=105.82 GB):  93%|█████████▎| 54/58 [00:06<00:00, 28.12it/s] Capturing num tokens (num_tokens=8 avail_mem=105.82 GB):  98%|█████████▊| 57/58 [00:06<00:00, 26.22it/s]Capturing num tokens (num_tokens=4 avail_mem=105.82 GB):  98%|█████████▊| 57/58 [00:06<00:00, 26.22it/s]Capturing num tokens (num_tokens=4 avail_mem=105.82 GB): 100%|██████████| 58/58 [00:06<00:00,  9.31it/s]


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
    
    Generated text: Okay, the user is asking for the information and population of the capital of France. I know the capital is Paris, so I should start by confirming that.
    
    Next, I need to provide the population. I recall that Paris is the most populous city in France and one of the largest in Europe. The population has been increasing, so I should mention that it's in the hundreds of millions.
    
    I should also include the country's population if the user is interested in that context. The current data as of 2023 estimates Paris at around 21 million people and France at about 40 million.
    
    It's good to present the information clearly, maybe in a structured format like a JSON, as the user requested. I'll make sure to format it correctly with the country and capital names and their respective populations.
    
    I should keep it concise but informative, so the user gets all the necessary details without too much fluff.
    </think>
    
    Here is the information about the capital of France in JSON format:
    
    ```json
    {
      "country": "France",
      "capital": "Paris",
      "population": {
        "country": 40401892,
        "city": 2164908
      }
    }
    ```
    
    This JSON object contains:
    - The country name: "France"
    - The capital city: "Paris"
    - Population statistics:
      - Total population of France: 40,401,892 (as of 2021)
      - Population of Paris: 2,164,908 (as of 2021)



```python
llm.shutdown()
```
