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
    2026-03-30 17:15:22.311 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 17:15:22] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 17:15:22.311 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 17:15:22] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 17:15:22.311 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 17:15:22] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 17:15:22.311 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 17:15:22] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 17:15:22.311 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 17:15:22] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:01<00:01,  1.13s/it]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.22s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.21s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:51,  3.02s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:51,  3.02s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:26,  1.54s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:26,  1.54s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:51,  1.06it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:51,  1.06it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:35,  1.53it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:35,  1.53it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:25,  2.05it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:25,  2.05it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:19,  2.64it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:19,  2.64it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:15,  3.37it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:15,  3.37it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:12,  4.04it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:12,  4.04it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:10,  4.48it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:10,  4.48it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:04<00:09,  5.02it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:04<00:09,  5.02it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:05<00:08,  5.56it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:05<00:08,  5.56it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:05<00:07,  6.15it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:05<00:07,  6.15it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:05<00:06,  6.81it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:05<00:06,  6.81it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:05<00:05,  7.51it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:05<00:05,  7.51it/s]Compiling num tokens (num_tokens=2304):  24%|██▍       | 14/58 [00:05<00:05,  7.51it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:05<00:04,  9.08it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:05<00:04,  9.08it/s]

    Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:05<00:04,  9.08it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:05<00:03, 10.88it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:05<00:03, 10.88it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:05<00:03, 10.88it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:05<00:02, 12.76it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:05<00:02, 12.76it/s]

    Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:05<00:02, 12.76it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:05<00:02, 14.30it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:05<00:02, 14.30it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:05<00:02, 14.30it/s]

    Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:06<00:02, 14.25it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:06<00:02, 14.25it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:06<00:02, 14.25it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:06<00:02, 14.86it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:06<00:02, 14.86it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:06<00:02, 14.86it/s]

    Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:06<00:01, 15.45it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:06<00:01, 15.45it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:06<00:01, 15.45it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:06<00:01, 15.45it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:06<00:01, 17.20it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:06<00:01, 17.20it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:06<00:01, 17.20it/s]

    Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:06<00:01, 17.20it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:06<00:01, 18.53it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:06<00:01, 18.53it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:06<00:01, 18.53it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:06<00:01, 18.53it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:06<00:01, 19.51it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:06<00:01, 19.51it/s]

    Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:06<00:01, 19.51it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:06<00:01, 19.51it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:06<00:00, 21.19it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:06<00:00, 21.19it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:06<00:00, 21.19it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:06<00:00, 21.19it/s]

    Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:06<00:00, 21.85it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:06<00:00, 21.85it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:06<00:00, 21.85it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:07<00:00, 21.85it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:07<00:00, 23.28it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:07<00:00, 23.28it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:07<00:00, 23.28it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:07<00:00, 23.28it/s]

    Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:07<00:00, 23.91it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:07<00:00, 23.91it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:07<00:00, 23.91it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:07<00:00, 23.91it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:07<00:00, 25.42it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:07<00:00, 25.42it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:07<00:00, 25.42it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:07<00:00, 25.42it/s]

    Compiling num tokens (num_tokens=16):  95%|█████████▍| 55/58 [00:07<00:00, 26.44it/s]Compiling num tokens (num_tokens=12):  95%|█████████▍| 55/58 [00:07<00:00, 26.44it/s]Compiling num tokens (num_tokens=8):  95%|█████████▍| 55/58 [00:07<00:00, 26.44it/s] Compiling num tokens (num_tokens=4):  95%|█████████▍| 55/58 [00:07<00:00, 26.44it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:07<00:00,  7.78it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=100.28 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=100.28 GB):   2%|▏         | 1/58 [00:00<00:38,  1.50it/s]Capturing num tokens (num_tokens=7680 avail_mem=100.23 GB):   2%|▏         | 1/58 [00:00<00:38,  1.50it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=100.23 GB):   3%|▎         | 2/58 [00:01<00:35,  1.60it/s]Capturing num tokens (num_tokens=7168 avail_mem=100.22 GB):   3%|▎         | 2/58 [00:01<00:35,  1.60it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=100.22 GB):   5%|▌         | 3/58 [00:01<00:31,  1.73it/s]Capturing num tokens (num_tokens=6656 avail_mem=100.22 GB):   5%|▌         | 3/58 [00:01<00:31,  1.73it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=100.22 GB):   7%|▋         | 4/58 [00:02<00:28,  1.86it/s]Capturing num tokens (num_tokens=6144 avail_mem=100.21 GB):   7%|▋         | 4/58 [00:02<00:28,  1.86it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=100.21 GB):   9%|▊         | 5/58 [00:02<00:26,  2.02it/s]Capturing num tokens (num_tokens=5632 avail_mem=100.20 GB):   9%|▊         | 5/58 [00:02<00:26,  2.02it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=100.20 GB):  10%|█         | 6/58 [00:03<00:23,  2.26it/s]Capturing num tokens (num_tokens=5120 avail_mem=100.19 GB):  10%|█         | 6/58 [00:03<00:23,  2.26it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=100.19 GB):  12%|█▏        | 7/58 [00:03<00:19,  2.56it/s]Capturing num tokens (num_tokens=4608 avail_mem=100.19 GB):  12%|█▏        | 7/58 [00:03<00:19,  2.56it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=100.19 GB):  14%|█▍        | 8/58 [00:03<00:16,  2.95it/s]Capturing num tokens (num_tokens=4096 avail_mem=100.19 GB):  14%|█▍        | 8/58 [00:03<00:16,  2.95it/s]Capturing num tokens (num_tokens=4096 avail_mem=100.19 GB):  16%|█▌        | 9/58 [00:03<00:14,  3.43it/s]Capturing num tokens (num_tokens=3840 avail_mem=100.19 GB):  16%|█▌        | 9/58 [00:03<00:14,  3.43it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=100.19 GB):  17%|█▋        | 10/58 [00:03<00:12,  3.91it/s]Capturing num tokens (num_tokens=3584 avail_mem=100.18 GB):  17%|█▋        | 10/58 [00:03<00:12,  3.91it/s]Capturing num tokens (num_tokens=3584 avail_mem=100.18 GB):  19%|█▉        | 11/58 [00:04<00:10,  4.42it/s]Capturing num tokens (num_tokens=3328 avail_mem=100.18 GB):  19%|█▉        | 11/58 [00:04<00:10,  4.42it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=100.18 GB):  21%|██        | 12/58 [00:04<00:08,  5.29it/s]Capturing num tokens (num_tokens=3072 avail_mem=100.18 GB):  21%|██        | 12/58 [00:04<00:08,  5.29it/s]Capturing num tokens (num_tokens=3072 avail_mem=100.18 GB):  22%|██▏       | 13/58 [00:04<00:07,  6.09it/s]Capturing num tokens (num_tokens=2816 avail_mem=100.18 GB):  22%|██▏       | 13/58 [00:04<00:07,  6.09it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=100.18 GB):  22%|██▏       | 13/58 [00:04<00:07,  6.09it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=100.18 GB):  26%|██▌       | 15/58 [00:04<00:08,  5.29it/s]Capturing num tokens (num_tokens=2304 avail_mem=99.15 GB):  26%|██▌       | 15/58 [00:04<00:08,  5.29it/s] 

    Capturing num tokens (num_tokens=2304 avail_mem=99.15 GB):  28%|██▊       | 16/58 [00:04<00:08,  5.18it/s]Capturing num tokens (num_tokens=2048 avail_mem=99.15 GB):  28%|██▊       | 16/58 [00:04<00:08,  5.18it/s]Capturing num tokens (num_tokens=2048 avail_mem=99.15 GB):  29%|██▉       | 17/58 [00:05<00:07,  5.49it/s]Capturing num tokens (num_tokens=1792 avail_mem=100.15 GB):  29%|██▉       | 17/58 [00:05<00:07,  5.49it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=100.15 GB):  31%|███       | 18/58 [00:05<00:06,  5.94it/s]Capturing num tokens (num_tokens=1536 avail_mem=99.94 GB):  31%|███       | 18/58 [00:05<00:06,  5.94it/s] Capturing num tokens (num_tokens=1536 avail_mem=99.94 GB):  33%|███▎      | 19/58 [00:05<00:06,  5.94it/s]Capturing num tokens (num_tokens=1280 avail_mem=99.33 GB):  33%|███▎      | 19/58 [00:05<00:06,  5.94it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=99.33 GB):  34%|███▍      | 20/58 [00:05<00:06,  6.17it/s]Capturing num tokens (num_tokens=1024 avail_mem=99.33 GB):  34%|███▍      | 20/58 [00:05<00:06,  6.17it/s]Capturing num tokens (num_tokens=960 avail_mem=100.15 GB):  34%|███▍      | 20/58 [00:05<00:06,  6.17it/s]Capturing num tokens (num_tokens=960 avail_mem=100.15 GB):  38%|███▊      | 22/58 [00:05<00:04,  7.60it/s]Capturing num tokens (num_tokens=896 avail_mem=99.38 GB):  38%|███▊      | 22/58 [00:05<00:04,  7.60it/s] 

    Capturing num tokens (num_tokens=896 avail_mem=99.38 GB):  40%|███▉      | 23/58 [00:05<00:04,  7.61it/s]Capturing num tokens (num_tokens=832 avail_mem=99.37 GB):  40%|███▉      | 23/58 [00:05<00:04,  7.61it/s]Capturing num tokens (num_tokens=832 avail_mem=99.37 GB):  41%|████▏     | 24/58 [00:05<00:04,  7.68it/s]Capturing num tokens (num_tokens=768 avail_mem=99.37 GB):  41%|████▏     | 24/58 [00:05<00:04,  7.68it/s]

    Capturing num tokens (num_tokens=768 avail_mem=99.37 GB):  43%|████▎     | 25/58 [00:06<00:04,  8.01it/s]Capturing num tokens (num_tokens=704 avail_mem=100.13 GB):  43%|████▎     | 25/58 [00:06<00:04,  8.01it/s]Capturing num tokens (num_tokens=704 avail_mem=100.13 GB):  45%|████▍     | 26/58 [00:06<00:03,  8.07it/s]Capturing num tokens (num_tokens=640 avail_mem=99.42 GB):  45%|████▍     | 26/58 [00:06<00:03,  8.07it/s] 

    Capturing num tokens (num_tokens=640 avail_mem=99.42 GB):  47%|████▋     | 27/58 [00:06<00:03,  8.19it/s]Capturing num tokens (num_tokens=576 avail_mem=99.42 GB):  47%|████▋     | 27/58 [00:06<00:03,  8.19it/s]Capturing num tokens (num_tokens=576 avail_mem=99.42 GB):  48%|████▊     | 28/58 [00:06<00:03,  8.38it/s]Capturing num tokens (num_tokens=512 avail_mem=100.12 GB):  48%|████▊     | 28/58 [00:06<00:03,  8.38it/s]

    Capturing num tokens (num_tokens=512 avail_mem=100.12 GB):  50%|█████     | 29/58 [00:06<00:03,  8.79it/s]Capturing num tokens (num_tokens=480 avail_mem=99.46 GB):  50%|█████     | 29/58 [00:06<00:03,  8.79it/s] Capturing num tokens (num_tokens=480 avail_mem=99.46 GB):  52%|█████▏    | 30/58 [00:06<00:03,  8.85it/s]Capturing num tokens (num_tokens=448 avail_mem=99.46 GB):  52%|█████▏    | 30/58 [00:06<00:03,  8.85it/s]

    Capturing num tokens (num_tokens=416 avail_mem=100.12 GB):  52%|█████▏    | 30/58 [00:06<00:03,  8.85it/s]Capturing num tokens (num_tokens=416 avail_mem=100.12 GB):  55%|█████▌    | 32/58 [00:06<00:02,  9.55it/s]Capturing num tokens (num_tokens=384 avail_mem=99.51 GB):  55%|█████▌    | 32/58 [00:06<00:02,  9.55it/s] 

    Capturing num tokens (num_tokens=384 avail_mem=99.51 GB):  57%|█████▋    | 33/58 [00:06<00:02,  9.11it/s]Capturing num tokens (num_tokens=352 avail_mem=99.47 GB):  57%|█████▋    | 33/58 [00:06<00:02,  9.11it/s]Capturing num tokens (num_tokens=320 avail_mem=100.08 GB):  57%|█████▋    | 33/58 [00:07<00:02,  9.11it/s]

    Capturing num tokens (num_tokens=320 avail_mem=100.08 GB):  60%|██████    | 35/58 [00:07<00:02,  8.35it/s]Capturing num tokens (num_tokens=288 avail_mem=99.53 GB):  60%|██████    | 35/58 [00:07<00:02,  8.35it/s] Capturing num tokens (num_tokens=256 avail_mem=100.11 GB):  60%|██████    | 35/58 [00:07<00:02,  8.35it/s]Capturing num tokens (num_tokens=256 avail_mem=100.11 GB):  64%|██████▍   | 37/58 [00:07<00:02,  9.67it/s]Capturing num tokens (num_tokens=240 avail_mem=100.07 GB):  64%|██████▍   | 37/58 [00:07<00:02,  9.67it/s]

    Capturing num tokens (num_tokens=224 avail_mem=99.57 GB):  64%|██████▍   | 37/58 [00:07<00:02,  9.67it/s] Capturing num tokens (num_tokens=224 avail_mem=99.57 GB):  67%|██████▋   | 39/58 [00:07<00:01, 10.34it/s]Capturing num tokens (num_tokens=208 avail_mem=100.06 GB):  67%|██████▋   | 39/58 [00:07<00:01, 10.34it/s]Capturing num tokens (num_tokens=192 avail_mem=99.62 GB):  67%|██████▋   | 39/58 [00:07<00:01, 10.34it/s] 

    Capturing num tokens (num_tokens=192 avail_mem=99.62 GB):  71%|███████   | 41/58 [00:07<00:01, 10.94it/s]Capturing num tokens (num_tokens=176 avail_mem=99.62 GB):  71%|███████   | 41/58 [00:07<00:01, 10.94it/s]Capturing num tokens (num_tokens=160 avail_mem=100.06 GB):  71%|███████   | 41/58 [00:07<00:01, 10.94it/s]Capturing num tokens (num_tokens=160 avail_mem=100.06 GB):  74%|███████▍  | 43/58 [00:07<00:01, 11.46it/s]Capturing num tokens (num_tokens=144 avail_mem=99.64 GB):  74%|███████▍  | 43/58 [00:07<00:01, 11.46it/s] 

    Capturing num tokens (num_tokens=128 avail_mem=100.06 GB):  74%|███████▍  | 43/58 [00:07<00:01, 11.46it/s]Capturing num tokens (num_tokens=128 avail_mem=100.06 GB):  78%|███████▊  | 45/58 [00:07<00:01, 12.23it/s]Capturing num tokens (num_tokens=112 avail_mem=99.68 GB):  78%|███████▊  | 45/58 [00:07<00:01, 12.23it/s] Capturing num tokens (num_tokens=96 avail_mem=100.06 GB):  78%|███████▊  | 45/58 [00:08<00:01, 12.23it/s]

    Capturing num tokens (num_tokens=96 avail_mem=100.06 GB):  81%|████████  | 47/58 [00:08<00:00, 12.84it/s]Capturing num tokens (num_tokens=80 avail_mem=99.69 GB):  81%|████████  | 47/58 [00:08<00:00, 12.84it/s] Capturing num tokens (num_tokens=64 avail_mem=99.69 GB):  81%|████████  | 47/58 [00:08<00:00, 12.84it/s]Capturing num tokens (num_tokens=64 avail_mem=99.69 GB):  84%|████████▍ | 49/58 [00:08<00:00, 13.24it/s]Capturing num tokens (num_tokens=48 avail_mem=100.04 GB):  84%|████████▍ | 49/58 [00:08<00:00, 13.24it/s]

    Capturing num tokens (num_tokens=32 avail_mem=99.71 GB):  84%|████████▍ | 49/58 [00:08<00:00, 13.24it/s] Capturing num tokens (num_tokens=32 avail_mem=99.71 GB):  88%|████████▊ | 51/58 [00:08<00:00, 13.47it/s]Capturing num tokens (num_tokens=28 avail_mem=100.04 GB):  88%|████████▊ | 51/58 [00:08<00:00, 13.47it/s]Capturing num tokens (num_tokens=24 avail_mem=99.73 GB):  88%|████████▊ | 51/58 [00:08<00:00, 13.47it/s] Capturing num tokens (num_tokens=24 avail_mem=99.73 GB):  91%|█████████▏| 53/58 [00:08<00:00, 13.92it/s]Capturing num tokens (num_tokens=20 avail_mem=99.96 GB):  91%|█████████▏| 53/58 [00:08<00:00, 13.92it/s]

    Capturing num tokens (num_tokens=16 avail_mem=100.03 GB):  91%|█████████▏| 53/58 [00:08<00:00, 13.92it/s]Capturing num tokens (num_tokens=16 avail_mem=100.03 GB):  95%|█████████▍| 55/58 [00:08<00:00, 13.10it/s]Capturing num tokens (num_tokens=12 avail_mem=99.77 GB):  95%|█████████▍| 55/58 [00:08<00:00, 13.10it/s] Capturing num tokens (num_tokens=8 avail_mem=100.02 GB):  95%|█████████▍| 55/58 [00:08<00:00, 13.10it/s]

    Capturing num tokens (num_tokens=8 avail_mem=100.02 GB):  98%|█████████▊| 57/58 [00:08<00:00, 14.11it/s]Capturing num tokens (num_tokens=4 avail_mem=100.02 GB):  98%|█████████▊| 57/58 [00:08<00:00, 14.11it/s]Capturing num tokens (num_tokens=4 avail_mem=100.02 GB): 100%|██████████| 58/58 [00:08<00:00,  6.54it/s]


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


<strong style='color: #00008B;'>reasoing_content: Alright, let's figure out how to help the user. They're in New York and want the current date and time, along with the weather. <br><br>First, I need to determine which functions to use. The user mentioned they're in New York, so I should get the current date and time for their timezone. Looking at the available functions, 'get_current_date' is perfect for this. It requires a timezone parameter. Since New York is in the 'America/New_York' timezone, I'll set that as the argument.<br><br>Next, I need to get the weather. The user is in New York, so I should specify the city and state. New York's state is NY. The function 'get_current_weather' requires city, state, and unit. I'll choose Fahrenheit since the user didn't specify, but it's a common unit people use.<br><br>I should structure the response correctly, using the specified format. Each function call will be on a separate line, starting with <function= followed by the function name and parameters in a JSON object. I'll also add sources to the answer, citing each function's documentation to ensure credibility.<br><br>Wait, the user mentioned to always execute Python code in messages. But since this is a text response, I just need to provide the formatted function calls with the necessary parameters. I should make sure each parameter is correctly formatted and that the JSON syntax is accurate to avoid errors.<br><br>Double-checking the parameters: for 'get_current_date', the timezone is 'America/New_York'. For 'get_current_weather', city is 'New York', state is 'NY', and unit is 'fahrenheit'. <br><br>I think that's all. Time to put it together in the specified format.<br><br><br>content: <function=get_current_date>{"timezone": "America/New_York"}</function>  <br><function=get_current_weather>{"city": "New York", "state": "NY", "unit": "fahrenheit"}</function></strong>


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

    {'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. I know that Paris is the capital, so that\'s straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I\'m not sure of the exact number. I think it\'s around 2 million, but I should verify that.\n\nI\'ll check a reliable source, maybe the official Paris Municipality website or a recent census. Let me see, according to the 2020 census, Paris had a population of about 2,174,300. That seems accurate. I should make sure to include this number in the JSON.\n\nNext, I need to structure this information into a JSON format. The user wants a JSON, so I\'ll create an object with a "name" field for the city, a "population" field, and maybe a "description" for more context. The description can mention that Paris is the capital and a major city in France.\n\nI should also consider the format. The JSON should be properly formatted with keys and values, and each key should be a string. The population number should be an integer since it\'s a count of people.\n\nPutting it all together, the JSON will have the city name, population, and a brief description. I\'ll make sure the syntax is correct, with commas and brackets in the right places to avoid errors.\n\nFinally, I\'ll present the JSON to the user, keeping it simple and clear. I don\'t need to add extra information unless the user asks for it, so I\'ll stick to the basics they requested.\n</think>{\n\n"name": "Paris",\n"population": 2174300000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1265, 1477, 279, 1429, 3213, 7042, 821, 13, 358, 6099, 429, 279, 7042, 315, 12095, 702, 1012, 7826, 11, 714, 358, 2776, 537, 2704, 315, 279, 4734, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 10146, 429, 382, 40, 3278, 1779, 264, 14720, 2530, 11, 7196, 279, 3946, 12095, 35703, 2719, 3910, 476, 264, 3213, 43602, 13, 6771, 752, 1490, 11, 4092, 311, 279, 220, 17, 15, 17, 15, 43602, 11, 12095, 1030, 264, 7042, 315, 911, 220, 17, 11, 16, 22, 19, 11, 18, 15, 15, 13, 2938, 4977, 13382, 13, 358, 1265, 1281, 2704, 311, 2924, 419, 1372, 304, 279, 4718, 382, 5847, 11, 358, 1184, 311, 5944, 419, 1995, 1119, 264, 4718, 3561, 13, 576, 1196, 6801, 264, 4718, 11, 773, 358, 3278, 1855, 458, 1633, 448, 264, 330, 606, 1, 2070, 369, 279, 3283, 11, 264, 330, 44441, 1, 2070, 11, 323, 7196, 264, 330, 4684, 1, 369, 803, 2266, 13, 576, 4008, 646, 6286, 429, 12095, 374, 279, 6722, 323, 264, 3598, 3283, 304, 9625, 382, 40, 1265, 1083, 2908, 279, 3561, 13, 576, 4718, 1265, 387, 10277, 23126, 448, 6894, 323, 2750, 11, 323, 1817, 1376, 1265, 387, 264, 914, 13, 576, 7042, 1372, 1265, 387, 458, 7546, 2474, 432, 594, 264, 1760, 315, 1251, 382, 97904, 432, 678, 3786, 11, 279, 4718, 686, 614, 279, 3283, 829, 11, 7042, 11, 323, 264, 9814, 4008, 13, 358, 3278, 1281, 2704, 279, 19482, 374, 4396, 11, 448, 76602, 323, 38929, 304, 279, 1290, 7482, 311, 5648, 5975, 382, 23949, 11, 358, 3278, 3042, 279, 4718, 311, 279, 1196, 11, 10282, 432, 4285, 323, 2797, 13, 358, 1513, 944, 1184, 311, 912, 4960, 1995, 7241, 279, 1196, 17064, 369, 432, 11, 773, 358, 3278, 9214, 311, 279, 31774, 807, 11223, 624, 151649, 4257, 1, 606, 788, 330, 59604, 756, 1, 44441, 788, 220, 17, 16, 22, 19, 18, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15, 15], 'meta_info': {'id': 'b26b7221d0454fd49940239b5693a6d8', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 22.790579149033874, 'response_sent_to_client_ts': 1774890993.375557}}



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


<strong style='color: #00008B;'>{'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down. First, I need to identify what the capital of France is. I know that Paris is the capital, so that\'s the starting point.\n\nNext, I need to find the population of Paris. I remember that Paris is a major city with a large population, but I\'m not exactly sure of the current number. I think it\'s around 2 million, but I should double-check that. Maybe I can recall that it\'s approximately 2,150,000 as of recent estimates.\n\nNow, the user wants this information in JSON format. JSON stands for JavaScript Object Notation, which is a way to structure data. I need to create a JSON object that includes the key "capital" with the value "Paris" and another key "population" with the number I just thought of.\n\nI should make sure the JSON syntax is correct. That means using double quotes for keys and string values, and commas appropriately between key-value pairs. Also, the numbers should be in quotes if they\'re strings, but population is a number, so it should be without quotes.\n\nPutting it all together, the JSON object should look like this: {"capital": "Paris", "population": 2150000}. I should present this clearly so the user can easily understand and use the information.\n\nI wonder if the user needs more details, like the population figure\'s source or the exact year it was recorded. But since they didn\'t ask for that, I\'ll stick to the information requested. Maybe they just need a straightforward data structure for a program or a report.\n\nAlso, considering the user\'s request, they might be a student working on a project, or perhaps someone developing an app that requires geographical data. Either way, providing the information in a structured format like JSON would be helpful for their purposes.\n\nI should make sure there are no typos in the JSON to avoid any issues when the user tries to use it. Double-checking the spelling of "capital" and "population" is important. Also, ensuring that the commas are correctly placed between the key-value pairs.\n\nIn summary, the user needs the capital of France and its population in JSON. I\'ve identified Paris as the capital and approximately 2,150,000 as the population. Structuring this into a JSON object with appropriate keys and correct syntax should meet their needs.\n</think>{\n\n"name": "Paris",\n"population": 2150000}', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 13, 5512, 11, 358, 1184, 311, 10542, 1128, 279, 6722, 315, 9625, 374, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 279, 5916, 1459, 382, 5847, 11, 358, 1184, 311, 1477, 279, 7042, 315, 12095, 13, 358, 6099, 429, 12095, 374, 264, 3598, 3283, 448, 264, 3460, 7042, 11, 714, 358, 2776, 537, 6896, 2704, 315, 279, 1482, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 1990, 15934, 429, 13, 10696, 358, 646, 19091, 429, 432, 594, 13187, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 438, 315, 3213, 17530, 382, 7039, 11, 279, 1196, 6801, 419, 1995, 304, 4718, 3561, 13, 4718, 13352, 369, 12914, 3002, 2806, 367, 11, 892, 374, 264, 1616, 311, 5944, 821, 13, 358, 1184, 311, 1855, 264, 4718, 1633, 429, 5646, 279, 1376, 330, 65063, 1, 448, 279, 897, 330, 59604, 1, 323, 2441, 1376, 330, 44441, 1, 448, 279, 1372, 358, 1101, 3381, 315, 382, 40, 1265, 1281, 2704, 279, 4718, 19482, 374, 4396, 13, 2938, 3363, 1667, 1990, 17194, 369, 6894, 323, 914, 2750, 11, 323, 76602, 34901, 1948, 1376, 19083, 13530, 13, 7281, 11, 279, 5109, 1265, 387, 304, 17194, 421, 807, 2299, 9069, 11, 714, 7042, 374, 264, 1372, 11, 773, 432, 1265, 387, 2041, 17194, 382, 97904, 432, 678, 3786, 11, 279, 4718, 1633, 1265, 1401, 1075, 419, 25, 5212, 65063, 788, 330, 59604, 497, 330, 44441, 788, 220, 17, 16, 20, 15, 15, 15, 15, 7810, 358, 1265, 3042, 419, 9355, 773, 279, 1196, 646, 6707, 3535, 323, 990, 279, 1995, 382, 40, 5775, 421, 279, 1196, 3880, 803, 3565, 11, 1075, 279, 7042, 7071, 594, 2530, 476, 279, 4734, 1042, 432, 572, 12433, 13, 1988, 2474, 807, 3207, 944, 2548, 369, 429, 11, 358, 3278, 9214, 311, 279, 1995, 11223, 13, 10696, 807, 1101, 1184, 264, 30339, 821, 5944, 369, 264, 2025, 476, 264, 1895, 382, 13394, 11, 12831, 279, 1196, 594, 1681, 11, 807, 2578, 387, 264, 5458, 3238, 389, 264, 2390, 11, 476, 8365, 4325, 11220, 458, 906, 429, 7460, 52901, 821, 13, 20988, 1616, 11, 8241, 279, 1995, 304, 264, 32930, 3561, 1075, 4718, 1035, 387, 10950, 369, 862, 9895, 382, 40, 1265, 1281, 2704, 1052, 525, 902, 13580, 966, 304, 279, 4718, 311, 5648, 894, 4714, 979, 279, 1196, 16297, 311, 990, 432, 13, 7093, 15934, 287, 279, 42429, 315, 330, 65063, 1, 323, 330, 44441, 1, 374, 2989, 13, 7281, 11, 22573, 429, 279, 76602, 525, 12440, 9099, 1948, 279, 1376, 19083, 13530, 382, 641, 12126, 11, 279, 1196, 3880, 279, 6722, 315, 9625, 323, 1181, 7042, 304, 4718, 13, 358, 3003, 10820, 12095, 438, 279, 6722, 323, 13187, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 438, 279, 7042, 13, 16139, 1677, 419, 1119, 264, 4718, 1633, 448, 8311, 6894, 323, 4396, 19482, 1265, 3367, 862, 3880, 624, 151649, 4257, 1, 606, 788, 330, 59604, 756, 1, 44441, 788, 220, 17, 16, 20, 15, 15, 15, 15, 92, 151643], 'meta_info': {'id': '69626285a6324880b5b4ab8ae35fd295', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 528, 'cached_tokens': 22, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 5.491843243129551, 'response_sent_to_client_ts': 1774890998.8780205}}</strong>


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

    [{'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '88fac1372c13417aba43bf6388f23a7a', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.11116861179471016, 'response_sent_to_client_ts': 1774890999.0274098}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': 'bdb8feca5e97461e9e4f61a8d4186d57', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.11109429225325584, 'response_sent_to_client_ts': 1774890999.0274217}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '75ee61cda4cc459a8f81e18863bdff78', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.1110337721183896, 'response_sent_to_client_ts': 1774890999.027436}}]


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

    {'text': ' France, and the \n\\( n \\)  \\( m \\) \\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\) \n\\( n \\) \\( m \\)', 'output_ids': [9625, 11, 323, 279, 220, 198, 44292, 308, 1124, 8, 220, 17767, 296, 1124, 8, 17767, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8, 715, 44292, 308, 1124, 8, 17767, 296, 1124, 8], 'meta_info': {'id': '69b05167f244421584e9ed66ced5cd86', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 6, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 18.364659222774208, 'response_sent_to_client_ts': 1774891017.3994715}}


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


<strong style='color: #00008B;'>{'text': 'Okay, so the user just asked for the information and population of the capital of France in JSON format. Hmm, let me break this down. \n\nFirst, I need to identify what the capital of France is. That\'s easy, it\'s Paris. Now, the user wants both information—probably meaning some basic facts—and the population. I should make sure the data is accurate. \n\nI remember that Paris has a population around 2 million, but I should double-check that. Let me think, recent estimates say about 2,155,000 as of 2023. Oh, and it\'s not just people; it\'s also the capital, so I should include the nickname, "La Ville zawやり" because I think that\'s French for "The Parisian City." \n\nDual citizenship is important too. Paris has a mix of French and international residents, making it a global hub. I should mention roughly how many international residents there are. I think it\'s about 250,000. \n\nPutting this into JSON, I need to structure it properly. Maybe use a "country" object with properties like "geographical_moment" and "official_name," just to give context. Then, the "capital" section can include population stats and details. \n\nI should make sure to present it clearly without any markdown, since the user mentioned avoiding that. Wait, but in JSON, spaces are okay as long as the syntax is correct. I\'ll structure the JSON with correct keys and nested objects where necessary.\n\nLet me double-check all the numbers to be precise. Population growth can vary, so I\'ll stick with the latest estimate. Also, the nickname is specific to Paris, so that\'s accurate. \n\nOkay, I think I have everything. Time to format the JSON correctly. Make sure the keys are consistent and the nesting is right. That should cover all the user\'s requirements concisely.\n</think>\n\nHere is the information about the capital of France (Paris) in the requested JSON format:\n\n```json\n{\n  "country": {\n    "geographical_moment": "contemporary",\n    "official_name": "France"\n  },\n  "capital": {\n    "name": "Paris",\n    "population": 2155000,\n    "population statistically defined": 2169000,\n    "dual_citizenship": true,\n    "nickname": "La Ville zaw Holly"\n    "international_residents": 250000\n  }\n}\n```', 'output_ids': [32313, 11, 773, 279, 1196, 1101, 4588, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 88190, 11, 1077, 752, 1438, 419, 1495, 13, 4710, 5338, 11, 358, 1184, 311, 10542, 1128, 279, 6722, 315, 9625, 374, 13, 2938, 594, 4135, 11, 432, 594, 12095, 13, 4695, 11, 279, 1196, 6801, 2176, 1995, 2293, 54440, 7290, 1045, 6770, 13064, 16804, 279, 7042, 13, 358, 1265, 1281, 2704, 279, 821, 374, 13382, 13, 4710, 40, 6099, 429, 12095, 702, 264, 7042, 2163, 220, 17, 3526, 11, 714, 358, 1265, 1990, 15934, 429, 13, 6771, 752, 1744, 11, 3213, 17530, 1977, 911, 220, 17, 11, 16, 20, 20, 11, 15, 15, 15, 438, 315, 220, 17, 15, 17, 18, 13, 8670, 11, 323, 432, 594, 537, 1101, 1251, 26, 432, 594, 1083, 279, 6722, 11, 773, 358, 1265, 2924, 279, 29399, 11, 330, 8747, 89130, 64007, 130205, 1, 1576, 358, 1744, 429, 594, 8585, 369, 330, 785, 12095, 1103, 4311, 1189, 4710, 85074, 32679, 374, 2989, 2238, 13, 12095, 702, 264, 6514, 315, 8585, 323, 6489, 10826, 11, 3259, 432, 264, 3644, 18719, 13, 358, 1265, 6286, 17267, 1246, 1657, 6489, 10826, 1052, 525, 13, 358, 1744, 432, 594, 911, 220, 17, 20, 15, 11, 15, 15, 15, 13, 4710, 97904, 419, 1119, 4718, 11, 358, 1184, 311, 5944, 432, 10277, 13, 10696, 990, 264, 330, 11141, 1, 1633, 448, 5888, 1075, 330, 709, 31177, 717, 12913, 1, 323, 330, 32812, 1269, 1335, 1101, 311, 2968, 2266, 13, 5005, 11, 279, 330, 65063, 1, 3772, 646, 2924, 7042, 10472, 323, 3565, 13, 4710, 40, 1265, 1281, 2704, 311, 3042, 432, 9355, 2041, 894, 50494, 11, 2474, 279, 1196, 9733, 30426, 429, 13, 13824, 11, 714, 304, 4718, 11, 12621, 525, 16910, 438, 1293, 438, 279, 19482, 374, 4396, 13, 358, 3278, 5944, 279, 4718, 448, 4396, 6894, 323, 24034, 6171, 1380, 5871, 382, 10061, 752, 1990, 15934, 678, 279, 5109, 311, 387, 23560, 13, 39529, 6513, 646, 13289, 11, 773, 358, 3278, 9214, 448, 279, 5535, 16045, 13, 7281, 11, 279, 29399, 374, 3151, 311, 12095, 11, 773, 429, 594, 13382, 13, 4710, 32313, 11, 358, 1744, 358, 614, 4297, 13, 4120, 311, 3561, 279, 4718, 12440, 13, 7405, 2704, 279, 6894, 525, 12966, 323, 279, 66710, 374, 1290, 13, 2938, 1265, 3421, 678, 279, 1196, 594, 8502, 3529, 285, 974, 624, 151649, 271, 8420, 374, 279, 1995, 911, 279, 6722, 315, 9625, 320, 59604, 8, 304, 279, 11223, 4718, 3561, 1447, 73594, 2236, 198, 515, 220, 330, 11141, 788, 341, 262, 330, 709, 31177, 717, 12913, 788, 330, 772, 13766, 756, 262, 330, 32812, 1269, 788, 330, 49000, 698, 220, 1153, 220, 330, 65063, 788, 341, 262, 330, 606, 788, 330, 59604, 756, 262, 330, 44441, 788, 220, 17, 16, 20, 20, 15, 15, 15, 345, 262, 330, 44441, 46852, 4512, 788, 220, 17, 16, 21, 24, 15, 15, 15, 345, 262, 330, 67, 928, 666, 275, 28960, 2151, 788, 830, 345, 262, 330, 39413, 788, 330, 8747, 89130, 64007, 52325, 698, 262, 330, 97597, 4918, 6880, 788, 220, 17, 20, 15, 15, 15, 15, 198, 220, 456, 532, 73594, 151643], 'meta_info': {'id': '3661a4cefa244729869782f4030f5170', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 525, 'cached_tokens': 22, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 4.812205683905631, 'response_sent_to_client_ts': 1774891022.2214866}}</strong>



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


    2026-03-30 17:17:14.609 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 17:17:14] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 17:17:14.609 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 17:17:14] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 17:17:14.610 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 17:17:14] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 17:17:14.610 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 17:17:14] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 17:17:14.610 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 17:17:14] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:01<00:01,  1.17s/it]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.28s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.27s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:55,  3.07s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:55,  3.07s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:34,  1.68s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:34,  1.68s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<00:57,  1.05s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<00:57,  1.05s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:39,  1.37it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:39,  1.37it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:28,  1.84it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:28,  1.84it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:22,  2.32it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:22,  2.32it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:17,  2.91it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:17,  2.91it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:05<00:13,  3.66it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:05<00:13,  3.66it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:05<00:10,  4.52it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:05<00:10,  4.52it/s]

    Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:05<00:10,  4.52it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:05<00:08,  5.65it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:05<00:08,  5.65it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:05<00:08,  5.57it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:05<00:08,  5.57it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:05<00:07,  5.81it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:05<00:07,  5.81it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:05<00:06,  6.29it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:05<00:06,  6.29it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:05<00:06,  6.72it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:05<00:06,  6.72it/s]

    Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:06<00:06,  6.96it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:06<00:06,  6.96it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:06<00:05,  7.33it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:06<00:05,  7.33it/s]

    Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:06<00:05,  7.33it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:06<00:04,  8.71it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:06<00:04,  8.71it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:06<00:04,  8.71it/s]

    Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:06<00:03,  9.85it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:06<00:03,  9.85it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:06<00:03,  9.85it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:06<00:03, 11.23it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:06<00:03, 11.23it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:06<00:03, 11.23it/s]

    Compiling num tokens (num_tokens=704):  40%|███▉      | 23/58 [00:06<00:03, 11.23it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:06<00:02, 14.45it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:06<00:02, 14.45it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:06<00:02, 14.45it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:06<00:01, 15.52it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:06<00:01, 15.52it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:06<00:01, 15.52it/s]

    Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:07<00:01, 15.52it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:07<00:01, 17.58it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:07<00:01, 17.58it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:07<00:01, 17.58it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:07<00:01, 17.58it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:07<00:01, 17.58it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:07<00:01, 21.23it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:07<00:01, 21.23it/s]

    Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:07<00:01, 21.23it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:07<00:01, 21.23it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:07<00:00, 22.04it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:07<00:00, 22.04it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:07<00:00, 22.04it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:07<00:00, 22.04it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:07<00:00, 22.04it/s]

    Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:07<00:00, 25.04it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:07<00:00, 25.04it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:07<00:00, 25.04it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:07<00:00, 25.04it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:07<00:00, 25.23it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:07<00:00, 25.23it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:07<00:00, 25.23it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:07<00:00, 25.23it/s]

    Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:07<00:00, 25.23it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:07<00:00, 27.54it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:07<00:00, 27.54it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:07<00:00, 27.54it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:07<00:00, 27.54it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:07<00:00, 27.54it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:07<00:00, 28.36it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:07<00:00, 28.36it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:07<00:00, 28.36it/s]

    Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:07<00:00, 28.36it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:07<00:00, 28.36it/s] Compiling num tokens (num_tokens=4):  91%|█████████▏| 53/58 [00:07<00:00, 28.36it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:07<00:00,  7.33it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=104.29 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=104.29 GB):   2%|▏         | 1/58 [00:00<00:30,  1.86it/s]Capturing num tokens (num_tokens=7680 avail_mem=104.37 GB):   2%|▏         | 1/58 [00:00<00:30,  1.86it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=104.37 GB):   3%|▎         | 2/58 [00:01<00:28,  1.95it/s]Capturing num tokens (num_tokens=7168 avail_mem=104.39 GB):   3%|▎         | 2/58 [00:01<00:28,  1.95it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=104.39 GB):   5%|▌         | 3/58 [00:01<00:26,  2.11it/s]Capturing num tokens (num_tokens=6656 avail_mem=104.46 GB):   5%|▌         | 3/58 [00:01<00:26,  2.11it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=104.46 GB):   7%|▋         | 4/58 [00:01<00:22,  2.36it/s]Capturing num tokens (num_tokens=6144 avail_mem=104.76 GB):   7%|▋         | 4/58 [00:01<00:22,  2.36it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=104.76 GB):   9%|▊         | 5/58 [00:02<00:20,  2.63it/s]Capturing num tokens (num_tokens=5632 avail_mem=104.76 GB):   9%|▊         | 5/58 [00:02<00:20,  2.63it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=104.76 GB):  10%|█         | 6/58 [00:02<00:17,  2.89it/s]Capturing num tokens (num_tokens=5120 avail_mem=104.75 GB):  10%|█         | 6/58 [00:02<00:17,  2.89it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=104.75 GB):  12%|█▏        | 7/58 [00:02<00:15,  3.24it/s]Capturing num tokens (num_tokens=4608 avail_mem=104.75 GB):  12%|█▏        | 7/58 [00:02<00:15,  3.24it/s]Capturing num tokens (num_tokens=4608 avail_mem=104.75 GB):  14%|█▍        | 8/58 [00:02<00:13,  3.66it/s]Capturing num tokens (num_tokens=4096 avail_mem=104.75 GB):  14%|█▍        | 8/58 [00:02<00:13,  3.66it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=104.75 GB):  16%|█▌        | 9/58 [00:02<00:11,  4.09it/s]Capturing num tokens (num_tokens=3840 avail_mem=104.74 GB):  16%|█▌        | 9/58 [00:02<00:11,  4.09it/s]Capturing num tokens (num_tokens=3840 avail_mem=104.74 GB):  17%|█▋        | 10/58 [00:03<00:10,  4.50it/s]Capturing num tokens (num_tokens=3584 avail_mem=104.74 GB):  17%|█▋        | 10/58 [00:03<00:10,  4.50it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=104.74 GB):  19%|█▉        | 11/58 [00:03<00:09,  4.95it/s]Capturing num tokens (num_tokens=3328 avail_mem=104.73 GB):  19%|█▉        | 11/58 [00:03<00:09,  4.95it/s]Capturing num tokens (num_tokens=3328 avail_mem=104.73 GB):  21%|██        | 12/58 [00:03<00:08,  5.45it/s]Capturing num tokens (num_tokens=3072 avail_mem=104.72 GB):  21%|██        | 12/58 [00:03<00:08,  5.45it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=104.72 GB):  22%|██▏       | 13/58 [00:03<00:07,  5.92it/s]Capturing num tokens (num_tokens=2816 avail_mem=104.71 GB):  22%|██▏       | 13/58 [00:03<00:07,  5.92it/s]Capturing num tokens (num_tokens=2816 avail_mem=104.71 GB):  24%|██▍       | 14/58 [00:03<00:06,  6.53it/s]Capturing num tokens (num_tokens=2560 avail_mem=104.71 GB):  24%|██▍       | 14/58 [00:03<00:06,  6.53it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=104.71 GB):  26%|██▌       | 15/58 [00:03<00:05,  7.18it/s]Capturing num tokens (num_tokens=2304 avail_mem=104.70 GB):  26%|██▌       | 15/58 [00:03<00:05,  7.18it/s]Capturing num tokens (num_tokens=2048 avail_mem=104.69 GB):  26%|██▌       | 15/58 [00:03<00:05,  7.18it/s]Capturing num tokens (num_tokens=2048 avail_mem=104.69 GB):  29%|██▉       | 17/58 [00:04<00:04,  8.59it/s]Capturing num tokens (num_tokens=1792 avail_mem=104.67 GB):  29%|██▉       | 17/58 [00:04<00:04,  8.59it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=104.69 GB):  29%|██▉       | 17/58 [00:04<00:04,  8.59it/s]Capturing num tokens (num_tokens=1536 avail_mem=104.69 GB):  33%|███▎      | 19/58 [00:04<00:03, 10.08it/s]Capturing num tokens (num_tokens=1280 avail_mem=104.69 GB):  33%|███▎      | 19/58 [00:04<00:03, 10.08it/s]Capturing num tokens (num_tokens=1024 avail_mem=104.66 GB):  33%|███▎      | 19/58 [00:04<00:03, 10.08it/s]Capturing num tokens (num_tokens=1024 avail_mem=104.66 GB):  36%|███▌      | 21/58 [00:04<00:03, 11.81it/s]Capturing num tokens (num_tokens=960 avail_mem=104.67 GB):  36%|███▌      | 21/58 [00:04<00:03, 11.81it/s] 

    Capturing num tokens (num_tokens=896 avail_mem=104.67 GB):  36%|███▌      | 21/58 [00:04<00:03, 11.81it/s]Capturing num tokens (num_tokens=896 avail_mem=104.67 GB):  40%|███▉      | 23/58 [00:04<00:02, 13.67it/s]Capturing num tokens (num_tokens=832 avail_mem=104.66 GB):  40%|███▉      | 23/58 [00:04<00:02, 13.67it/s]Capturing num tokens (num_tokens=768 avail_mem=104.65 GB):  40%|███▉      | 23/58 [00:04<00:02, 13.67it/s]Capturing num tokens (num_tokens=704 avail_mem=104.64 GB):  40%|███▉      | 23/58 [00:04<00:02, 13.67it/s]Capturing num tokens (num_tokens=704 avail_mem=104.64 GB):  45%|████▍     | 26/58 [00:04<00:01, 16.27it/s]Capturing num tokens (num_tokens=640 avail_mem=104.64 GB):  45%|████▍     | 26/58 [00:04<00:01, 16.27it/s]

    Capturing num tokens (num_tokens=576 avail_mem=104.63 GB):  45%|████▍     | 26/58 [00:04<00:01, 16.27it/s]Capturing num tokens (num_tokens=512 avail_mem=104.62 GB):  45%|████▍     | 26/58 [00:04<00:01, 16.27it/s]Capturing num tokens (num_tokens=512 avail_mem=104.62 GB):  50%|█████     | 29/58 [00:04<00:01, 18.78it/s]Capturing num tokens (num_tokens=480 avail_mem=104.62 GB):  50%|█████     | 29/58 [00:04<00:01, 18.78it/s]Capturing num tokens (num_tokens=448 avail_mem=104.61 GB):  50%|█████     | 29/58 [00:04<00:01, 18.78it/s]Capturing num tokens (num_tokens=416 avail_mem=104.61 GB):  50%|█████     | 29/58 [00:04<00:01, 18.78it/s]Capturing num tokens (num_tokens=416 avail_mem=104.61 GB):  55%|█████▌    | 32/58 [00:04<00:01, 21.28it/s]Capturing num tokens (num_tokens=384 avail_mem=104.60 GB):  55%|█████▌    | 32/58 [00:04<00:01, 21.28it/s]

    Capturing num tokens (num_tokens=352 avail_mem=104.59 GB):  55%|█████▌    | 32/58 [00:04<00:01, 21.28it/s]Capturing num tokens (num_tokens=320 avail_mem=104.59 GB):  55%|█████▌    | 32/58 [00:04<00:01, 21.28it/s]Capturing num tokens (num_tokens=288 avail_mem=104.58 GB):  55%|█████▌    | 32/58 [00:04<00:01, 21.28it/s]Capturing num tokens (num_tokens=288 avail_mem=104.58 GB):  62%|██████▏   | 36/58 [00:04<00:00, 25.63it/s]Capturing num tokens (num_tokens=256 avail_mem=104.58 GB):  62%|██████▏   | 36/58 [00:04<00:00, 25.63it/s]Capturing num tokens (num_tokens=240 avail_mem=104.57 GB):  62%|██████▏   | 36/58 [00:04<00:00, 25.63it/s]Capturing num tokens (num_tokens=224 avail_mem=104.57 GB):  62%|██████▏   | 36/58 [00:04<00:00, 25.63it/s]Capturing num tokens (num_tokens=208 avail_mem=104.57 GB):  62%|██████▏   | 36/58 [00:04<00:00, 25.63it/s]Capturing num tokens (num_tokens=208 avail_mem=104.57 GB):  69%|██████▉   | 40/58 [00:04<00:00, 29.12it/s]Capturing num tokens (num_tokens=192 avail_mem=104.56 GB):  69%|██████▉   | 40/58 [00:04<00:00, 29.12it/s]

    Capturing num tokens (num_tokens=176 avail_mem=104.56 GB):  69%|██████▉   | 40/58 [00:04<00:00, 29.12it/s]Capturing num tokens (num_tokens=160 avail_mem=104.56 GB):  69%|██████▉   | 40/58 [00:04<00:00, 29.12it/s]Capturing num tokens (num_tokens=144 avail_mem=104.55 GB):  69%|██████▉   | 40/58 [00:05<00:00, 29.12it/s]Capturing num tokens (num_tokens=144 avail_mem=104.55 GB):  76%|███████▌  | 44/58 [00:05<00:00, 31.78it/s]Capturing num tokens (num_tokens=128 avail_mem=104.56 GB):  76%|███████▌  | 44/58 [00:05<00:00, 31.78it/s]Capturing num tokens (num_tokens=112 avail_mem=104.56 GB):  76%|███████▌  | 44/58 [00:05<00:00, 31.78it/s]Capturing num tokens (num_tokens=96 avail_mem=104.55 GB):  76%|███████▌  | 44/58 [00:05<00:00, 31.78it/s] Capturing num tokens (num_tokens=80 avail_mem=104.55 GB):  76%|███████▌  | 44/58 [00:05<00:00, 31.78it/s]Capturing num tokens (num_tokens=80 avail_mem=104.55 GB):  83%|████████▎ | 48/58 [00:05<00:00, 33.34it/s]Capturing num tokens (num_tokens=64 avail_mem=104.55 GB):  83%|████████▎ | 48/58 [00:05<00:00, 33.34it/s]

    Capturing num tokens (num_tokens=48 avail_mem=104.54 GB):  83%|████████▎ | 48/58 [00:05<00:00, 33.34it/s]Capturing num tokens (num_tokens=32 avail_mem=104.54 GB):  83%|████████▎ | 48/58 [00:05<00:00, 33.34it/s]Capturing num tokens (num_tokens=28 avail_mem=104.54 GB):  83%|████████▎ | 48/58 [00:05<00:00, 33.34it/s]Capturing num tokens (num_tokens=28 avail_mem=104.54 GB):  90%|████████▉ | 52/58 [00:05<00:00, 34.97it/s]Capturing num tokens (num_tokens=24 avail_mem=104.53 GB):  90%|████████▉ | 52/58 [00:05<00:00, 34.97it/s]Capturing num tokens (num_tokens=20 avail_mem=104.53 GB):  90%|████████▉ | 52/58 [00:05<00:00, 34.97it/s]Capturing num tokens (num_tokens=16 avail_mem=104.53 GB):  90%|████████▉ | 52/58 [00:05<00:00, 34.97it/s]Capturing num tokens (num_tokens=12 avail_mem=104.52 GB):  90%|████████▉ | 52/58 [00:05<00:00, 34.97it/s]Capturing num tokens (num_tokens=12 avail_mem=104.52 GB):  97%|█████████▋| 56/58 [00:05<00:00, 36.10it/s]Capturing num tokens (num_tokens=8 avail_mem=104.52 GB):  97%|█████████▋| 56/58 [00:05<00:00, 36.10it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=104.52 GB):  97%|█████████▋| 56/58 [00:05<00:00, 36.10it/s]Capturing num tokens (num_tokens=4 avail_mem=104.52 GB): 100%|██████████| 58/58 [00:05<00:00, 10.71it/s]


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
    
    Generated text: Okay, so I need to figure out the information and population of the capital of France, which is Paris, and present it in JSON format. Let me start by recalling what I know about Paris. I know it's the largest city in France and the capital. The population is a big number, but I'm not exactly sure of the exact figure. I think it's around 2 million, but maybe a bit more or less?
    
    I remember reading somewhere that the population of Paris is over two million. Maybe I should check that. Wait, I think it's actually around 2,100,000. That seems about right. But I should confirm that. Also, I should think about the key facts—capital of France, population figure, and maybe the area or some other relevant information.
    
    Wait, the user specifically asked for the population of the capital, so just Paris, not the whole metropolitan area. I think the metropolitan area is much larger, but the capital city itself is around 2.1 million. So, that's the number I should use.
    
    Now, how to format this in JSON. JSON requires key-value pairs. So I'll need to structure it with "Capital" as a key and its corresponding value, and "Population" as another key with its value.
    
    Putting it all together, I'll write a JSON object where "Capital" has the value "Paris" and "Population" has the value 2100000. That should cover the user's request accurately.
    </think>
    
    The JSON representation of the information and population of Paris, France's capital, is as follows:
    
    ```json
    {
      "Capital": "Paris",
      "Population": 2100000
    }
    ```



```python
llm.shutdown()
```
