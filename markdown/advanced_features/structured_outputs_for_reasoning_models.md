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


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:173: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-03-31 21:09:27.862 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:09:27] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:09:27.862 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:09:27] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:09:27.862 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:09:27] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:09:27.862 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:09:27] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:09:27.863 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:09:27] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:01<00:01,  1.12s/it]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.45s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.40s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:42,  2.85s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:42,  2.85s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:22,  1.48s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:22,  1.48s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:49,  1.12it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:49,  1.12it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:33,  1.64it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:33,  1.64it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:03<00:23,  2.23it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:03<00:23,  2.23it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.87it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.87it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.59it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.59it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.37it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.37it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.24it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.24it/s]

    Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.24it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:06,  6.90it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:06,  6.90it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:06,  6.90it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:04<00:05,  7.95it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:04<00:05,  7.95it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:04<00:05,  7.37it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:04<00:05,  7.37it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:05<00:05,  7.20it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:05<00:05,  7.20it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:05<00:05,  7.27it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:05<00:05,  7.27it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:05,  7.48it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:05,  7.48it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:05<00:05,  7.89it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:05<00:05,  7.89it/s]

    Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:05<00:04,  8.37it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:05<00:04,  8.37it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:05<00:04,  8.37it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:05<00:03,  9.76it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:05<00:03,  9.76it/s] 

    Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:05<00:03,  9.76it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:05<00:03, 11.31it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:05<00:03, 11.31it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:05<00:03, 11.31it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:05<00:02, 13.01it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:05<00:02, 13.01it/s]

    Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:06<00:02, 13.01it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:06<00:02, 14.51it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:06<00:02, 14.51it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:06<00:02, 14.51it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:06<00:01, 15.78it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:06<00:01, 15.78it/s]

    Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:06<00:01, 15.78it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:06<00:01, 15.78it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:06<00:01, 17.60it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:06<00:01, 17.60it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:06<00:01, 17.60it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:06<00:01, 17.60it/s]

    Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:06<00:01, 18.96it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:06<00:01, 18.96it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:06<00:01, 18.96it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:06<00:01, 18.96it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:06<00:00, 20.12it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:06<00:00, 20.12it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:06<00:00, 20.12it/s]

    Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:06<00:00, 20.12it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:06<00:00, 21.50it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:06<00:00, 21.50it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:06<00:00, 21.50it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:06<00:00, 21.50it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:06<00:00, 22.84it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:06<00:00, 22.84it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:06<00:00, 22.84it/s]

    Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:06<00:00, 22.84it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:06<00:00, 24.35it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:06<00:00, 24.35it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:07<00:00, 24.35it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:07<00:00, 24.35it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:07<00:00, 24.37it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:07<00:00, 24.37it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:07<00:00, 24.37it/s]

    Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:07<00:00, 24.37it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:07<00:00, 25.21it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:07<00:00, 25.21it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:07<00:00, 25.21it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:07<00:00, 25.21it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:07<00:00, 25.21it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:07<00:00, 28.38it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:07<00:00, 28.38it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:07<00:00,  7.92it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=20.96 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=20.96 GB):   2%|▏         | 1/58 [00:00<00:37,  1.51it/s]Capturing num tokens (num_tokens=7680 avail_mem=20.93 GB):   2%|▏         | 1/58 [00:00<00:37,  1.51it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=20.93 GB):   3%|▎         | 2/58 [00:01<00:32,  1.75it/s]Capturing num tokens (num_tokens=7168 avail_mem=20.93 GB):   3%|▎         | 2/58 [00:01<00:32,  1.75it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=20.93 GB):   5%|▌         | 3/58 [00:01<00:28,  1.90it/s]Capturing num tokens (num_tokens=6656 avail_mem=20.93 GB):   5%|▌         | 3/58 [00:01<00:28,  1.90it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=20.93 GB):   7%|▋         | 4/58 [00:02<00:30,  1.75it/s]Capturing num tokens (num_tokens=6144 avail_mem=20.93 GB):   7%|▋         | 4/58 [00:02<00:30,  1.75it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=20.93 GB):   9%|▊         | 5/58 [00:02<00:26,  2.02it/s]Capturing num tokens (num_tokens=5632 avail_mem=20.93 GB):   9%|▊         | 5/58 [00:02<00:26,  2.02it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=20.93 GB):  10%|█         | 6/58 [00:02<00:22,  2.29it/s]Capturing num tokens (num_tokens=5120 avail_mem=44.15 GB):  10%|█         | 6/58 [00:02<00:22,  2.29it/s]Capturing num tokens (num_tokens=5120 avail_mem=44.15 GB):  12%|█▏        | 7/58 [00:03<00:18,  2.81it/s]Capturing num tokens (num_tokens=4608 avail_mem=42.97 GB):  12%|█▏        | 7/58 [00:03<00:18,  2.81it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=42.97 GB):  14%|█▍        | 8/58 [00:03<00:17,  2.84it/s]Capturing num tokens (num_tokens=4096 avail_mem=42.97 GB):  14%|█▍        | 8/58 [00:03<00:17,  2.84it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=42.97 GB):  16%|█▌        | 9/58 [00:03<00:15,  3.15it/s]Capturing num tokens (num_tokens=3840 avail_mem=43.15 GB):  16%|█▌        | 9/58 [00:03<00:15,  3.15it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=43.15 GB):  17%|█▋        | 10/58 [00:04<00:15,  3.17it/s]Capturing num tokens (num_tokens=3584 avail_mem=43.14 GB):  17%|█▋        | 10/58 [00:04<00:15,  3.17it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=43.14 GB):  19%|█▉        | 11/58 [00:04<00:14,  3.32it/s]Capturing num tokens (num_tokens=3328 avail_mem=43.21 GB):  19%|█▉        | 11/58 [00:04<00:14,  3.32it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=43.21 GB):  21%|██        | 12/58 [00:04<00:14,  3.13it/s]Capturing num tokens (num_tokens=3072 avail_mem=44.13 GB):  21%|██        | 12/58 [00:04<00:14,  3.13it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=44.13 GB):  22%|██▏       | 13/58 [00:04<00:13,  3.43it/s]Capturing num tokens (num_tokens=2816 avail_mem=43.28 GB):  22%|██▏       | 13/58 [00:04<00:13,  3.43it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=43.28 GB):  24%|██▍       | 14/58 [00:05<00:11,  3.79it/s]Capturing num tokens (num_tokens=2560 avail_mem=43.35 GB):  24%|██▍       | 14/58 [00:05<00:11,  3.79it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=43.35 GB):  26%|██▌       | 15/58 [00:05<00:10,  4.00it/s]Capturing num tokens (num_tokens=2304 avail_mem=43.35 GB):  26%|██▌       | 15/58 [00:05<00:10,  4.00it/s]Capturing num tokens (num_tokens=2304 avail_mem=43.35 GB):  28%|██▊       | 16/58 [00:05<00:09,  4.42it/s]Capturing num tokens (num_tokens=2048 avail_mem=43.41 GB):  28%|██▊       | 16/58 [00:05<00:09,  4.42it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=43.41 GB):  29%|██▉       | 17/58 [00:05<00:08,  4.78it/s]Capturing num tokens (num_tokens=1792 avail_mem=44.15 GB):  29%|██▉       | 17/58 [00:05<00:08,  4.78it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=44.15 GB):  31%|███       | 18/58 [00:05<00:08,  4.62it/s]Capturing num tokens (num_tokens=1536 avail_mem=43.85 GB):  31%|███       | 18/58 [00:05<00:08,  4.62it/s]Capturing num tokens (num_tokens=1536 avail_mem=43.85 GB):  33%|███▎      | 19/58 [00:06<00:07,  5.33it/s]Capturing num tokens (num_tokens=1280 avail_mem=43.55 GB):  33%|███▎      | 19/58 [00:06<00:07,  5.33it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=43.55 GB):  34%|███▍      | 20/58 [00:06<00:06,  5.90it/s]Capturing num tokens (num_tokens=1024 avail_mem=44.15 GB):  34%|███▍      | 20/58 [00:06<00:06,  5.90it/s]Capturing num tokens (num_tokens=1024 avail_mem=44.15 GB):  36%|███▌      | 21/58 [00:06<00:05,  6.62it/s]Capturing num tokens (num_tokens=960 avail_mem=43.62 GB):  36%|███▌      | 21/58 [00:06<00:05,  6.62it/s] 

    Capturing num tokens (num_tokens=960 avail_mem=43.62 GB):  38%|███▊      | 22/58 [00:06<00:04,  7.35it/s]Capturing num tokens (num_tokens=896 avail_mem=44.14 GB):  38%|███▊      | 22/58 [00:06<00:04,  7.35it/s]Capturing num tokens (num_tokens=896 avail_mem=44.14 GB):  40%|███▉      | 23/58 [00:06<00:04,  7.84it/s]Capturing num tokens (num_tokens=832 avail_mem=43.65 GB):  40%|███▉      | 23/58 [00:06<00:04,  7.84it/s]

    Capturing num tokens (num_tokens=768 avail_mem=44.13 GB):  40%|███▉      | 23/58 [00:06<00:04,  7.84it/s]Capturing num tokens (num_tokens=768 avail_mem=44.13 GB):  43%|████▎     | 25/58 [00:06<00:03,  8.53it/s]Capturing num tokens (num_tokens=704 avail_mem=43.67 GB):  43%|████▎     | 25/58 [00:06<00:03,  8.53it/s]Capturing num tokens (num_tokens=640 avail_mem=44.10 GB):  43%|████▎     | 25/58 [00:06<00:03,  8.53it/s]

    Capturing num tokens (num_tokens=640 avail_mem=44.10 GB):  47%|████▋     | 27/58 [00:06<00:03,  9.14it/s]Capturing num tokens (num_tokens=576 avail_mem=43.69 GB):  47%|████▋     | 27/58 [00:06<00:03,  9.14it/s]Capturing num tokens (num_tokens=512 avail_mem=43.72 GB):  47%|████▋     | 27/58 [00:06<00:03,  9.14it/s]Capturing num tokens (num_tokens=512 avail_mem=43.72 GB):  50%|█████     | 29/58 [00:07<00:02,  9.84it/s]Capturing num tokens (num_tokens=480 avail_mem=44.12 GB):  50%|█████     | 29/58 [00:07<00:02,  9.84it/s]

    Capturing num tokens (num_tokens=448 avail_mem=43.75 GB):  50%|█████     | 29/58 [00:07<00:02,  9.84it/s]Capturing num tokens (num_tokens=448 avail_mem=43.75 GB):  53%|█████▎    | 31/58 [00:07<00:02, 10.71it/s]Capturing num tokens (num_tokens=416 avail_mem=43.78 GB):  53%|█████▎    | 31/58 [00:07<00:02, 10.71it/s]Capturing num tokens (num_tokens=384 avail_mem=44.11 GB):  53%|█████▎    | 31/58 [00:07<00:02, 10.71it/s]

    Capturing num tokens (num_tokens=384 avail_mem=44.11 GB):  57%|█████▋    | 33/58 [00:07<00:02, 10.80it/s]Capturing num tokens (num_tokens=352 avail_mem=43.80 GB):  57%|█████▋    | 33/58 [00:07<00:02, 10.80it/s]Capturing num tokens (num_tokens=320 avail_mem=44.10 GB):  57%|█████▋    | 33/58 [00:07<00:02, 10.80it/s]Capturing num tokens (num_tokens=320 avail_mem=44.10 GB):  60%|██████    | 35/58 [00:07<00:01, 11.98it/s]Capturing num tokens (num_tokens=288 avail_mem=43.83 GB):  60%|██████    | 35/58 [00:07<00:01, 11.98it/s]Capturing num tokens (num_tokens=256 avail_mem=44.09 GB):  60%|██████    | 35/58 [00:07<00:01, 11.98it/s]

    Capturing num tokens (num_tokens=256 avail_mem=44.09 GB):  64%|██████▍   | 37/58 [00:07<00:01, 13.00it/s]Capturing num tokens (num_tokens=240 avail_mem=44.08 GB):  64%|██████▍   | 37/58 [00:07<00:01, 13.00it/s]Capturing num tokens (num_tokens=224 avail_mem=44.08 GB):  64%|██████▍   | 37/58 [00:07<00:01, 13.00it/s]Capturing num tokens (num_tokens=224 avail_mem=44.08 GB):  67%|██████▋   | 39/58 [00:07<00:01, 13.74it/s]Capturing num tokens (num_tokens=208 avail_mem=43.95 GB):  67%|██████▋   | 39/58 [00:07<00:01, 13.74it/s]

    Capturing num tokens (num_tokens=192 avail_mem=44.07 GB):  67%|██████▋   | 39/58 [00:07<00:01, 13.74it/s]Capturing num tokens (num_tokens=192 avail_mem=44.07 GB):  71%|███████   | 41/58 [00:07<00:01, 13.77it/s]Capturing num tokens (num_tokens=176 avail_mem=44.07 GB):  71%|███████   | 41/58 [00:07<00:01, 13.77it/s]

    Capturing num tokens (num_tokens=160 avail_mem=43.94 GB):  71%|███████   | 41/58 [00:08<00:01, 13.77it/s]Capturing num tokens (num_tokens=160 avail_mem=43.94 GB):  74%|███████▍  | 43/58 [00:08<00:01, 12.76it/s]Capturing num tokens (num_tokens=144 avail_mem=44.05 GB):  74%|███████▍  | 43/58 [00:08<00:01, 12.76it/s]Capturing num tokens (num_tokens=128 avail_mem=44.06 GB):  74%|███████▍  | 43/58 [00:08<00:01, 12.76it/s]Capturing num tokens (num_tokens=128 avail_mem=44.06 GB):  78%|███████▊  | 45/58 [00:08<00:00, 13.98it/s]Capturing num tokens (num_tokens=112 avail_mem=44.06 GB):  78%|███████▊  | 45/58 [00:08<00:00, 13.98it/s]Capturing num tokens (num_tokens=96 avail_mem=44.05 GB):  78%|███████▊  | 45/58 [00:08<00:00, 13.98it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=43.94 GB):  78%|███████▊  | 45/58 [00:08<00:00, 13.98it/s]Capturing num tokens (num_tokens=80 avail_mem=43.94 GB):  83%|████████▎ | 48/58 [00:08<00:00, 16.74it/s]Capturing num tokens (num_tokens=64 avail_mem=43.94 GB):  83%|████████▎ | 48/58 [00:08<00:00, 16.74it/s]Capturing num tokens (num_tokens=48 avail_mem=44.03 GB):  83%|████████▎ | 48/58 [00:08<00:00, 16.74it/s]Capturing num tokens (num_tokens=48 avail_mem=44.03 GB):  86%|████████▌ | 50/58 [00:08<00:00, 17.08it/s]Capturing num tokens (num_tokens=32 avail_mem=44.02 GB):  86%|████████▌ | 50/58 [00:08<00:00, 17.08it/s]Capturing num tokens (num_tokens=28 avail_mem=44.01 GB):  86%|████████▌ | 50/58 [00:08<00:00, 17.08it/s]

    Capturing num tokens (num_tokens=24 avail_mem=44.01 GB):  86%|████████▌ | 50/58 [00:08<00:00, 17.08it/s]Capturing num tokens (num_tokens=24 avail_mem=44.01 GB):  91%|█████████▏| 53/58 [00:08<00:00, 19.16it/s]Capturing num tokens (num_tokens=20 avail_mem=44.00 GB):  91%|█████████▏| 53/58 [00:08<00:00, 19.16it/s]Capturing num tokens (num_tokens=16 avail_mem=44.01 GB):  91%|█████████▏| 53/58 [00:08<00:00, 19.16it/s]Capturing num tokens (num_tokens=12 avail_mem=43.95 GB):  91%|█████████▏| 53/58 [00:08<00:00, 19.16it/s]Capturing num tokens (num_tokens=12 avail_mem=43.95 GB):  97%|█████████▋| 56/58 [00:08<00:00, 21.54it/s]Capturing num tokens (num_tokens=8 avail_mem=43.94 GB):  97%|█████████▋| 56/58 [00:08<00:00, 21.54it/s] Capturing num tokens (num_tokens=4 avail_mem=43.97 GB):  97%|█████████▋| 56/58 [00:08<00:00, 21.54it/s]

    Capturing num tokens (num_tokens=4 avail_mem=43.97 GB): 100%|██████████| 58/58 [00:08<00:00,  6.63it/s]


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


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France and its population. I know that the capital of France is Paris, but I'm not exactly sure about the current population numbers. I remember that Paris is a very big city, but I think it's not the largest in the world. Maybe around 20 million? I'm not certain, though. I should probably double-check that. <br><br>Wait, I think I heard somewhere that Paris has a population over 21 million. Maybe 21.6 million? I'm not sure if that's accurate. I should look up the latest data to confirm. Also, I wonder if the population includes just the city proper or the entire metropolitan area. I think sometimes population counts include the broader area, so that might be a consideration. <br><br>I should make sure to present the information clearly in JSON format, as the user requested. So, the key would be "capital" with the value "Paris," and another key for "population." I'll need to include the number, maybe with a note about whether it's an approximate figure or the most recent data. <br><br>I'm a bit confused about whether the population figure I have is up to date. I think the population can change over time due to births, deaths, and migration. So, it's important to mention that the figure is approximate or based on the latest available data. <br><br>Also, I should consider the units. Is it in thousands, millions, or just a plain number? I think it's best to present it as a number without units, just the raw count. <br><br>Putting it all together, I'll structure the JSON with the keys "capital" and "population," and include the population as 21.6 million. I'll add a comment or note that the population figure is approximate. That should cover everything the user asked for.<br><br><br>content: {<br><br>"name": "Paris",<br>"population": 21600000}</strong>


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


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France and its population. I know that the capital of France is Paris, but I'm not exactly sure about the current population numbers. I remember that Paris is a very big city, but I think it's not the largest in the world. Maybe around 20 million? I'm not certain, though. I should probably double-check that. <br><br>Wait, I think I heard somewhere that Paris has a population over 21 million. Maybe 21.6 million? I'm not sure if that's accurate. I should look up the latest data to confirm. Also, I wonder if the population includes just the city proper or the entire metropolitan area. I think sometimes population counts include the broader area, so that might be a consideration. <br><br>I should make sure to present the information clearly in JSON format, as the user requested. So, the key would be "capital" with the value "Paris," and another key for "population." I'll need to include the number, maybe with a note about whether it's an approximate figure or the most recent data. <br><br>I'm a bit confused about whether the population figure I have is up to date. I think the population can change over time due to births, deaths, and migration. So, it's important to mention that the figure is approximate or based on the latest available data. <br><br>Also, I should consider the units. Is it in thousands, millions, or just a plain number? I think it's best to present it as a number without units, just the raw count. <br><br>Putting it all together, I'll structure the JSON with the keys "capital" and "population," and include the population as 21.6 million. I'll add a comment or note that the population figure is approximate. That should cover everything the user asked for.<br><br><br>content: {<br><br>"name": "Paris",<br>"population": 21600000}</strong>


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


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France and its population. I know that the capital of France is Paris, but I'm not exactly sure about the current population. I think it's a big city, maybe around 3 million? I remember hearing that Paris is one of the most populous cities in Europe, but I'm not certain about the exact number. Maybe I should check some sources or think about recent growth. I think the population has been increasing over the years, so perhaps it's now over 3.5 million? I'm a bit confused because sometimes I hear different numbers, so I should make sure. Maybe I can recall that Paris has a metropolitan area that's much larger, but the city proper is around 3.5 million. I think I'll go with that for now, but I'm not 100% sure. I should probably double-check this information to be accurate.<br><br><br>content: Paris is the capital of France</strong>


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


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France. Hmm, I remember learning about France in school, but I'm not 100% sure. Let me think. I know that Paris is a major city in France, and it's often referred to as the "City of Light." People go there for museums, landmarks like the Eiffel Tower, and it's a cultural hub. But is it the capital?<br><br>Wait, I think the capital is the official seat of government, right? So maybe Paris is both the capital and the most famous city. But I'm not entirely certain. I recall that some countries have their capital in a different city than their main tourist attraction. For example, I think Brazil's capital is not Rio de Janeiro, which is more famous. So maybe France is like that too.<br><br>I should try to remember any specific information. I think the French government declares Paris as the capital. Yeah, that sounds right. I also remember that the Eiffel Tower is in Paris, and it's a symbol of the country. So if Paris is the capital, then that makes sense. But I'm a bit confused because sometimes people say "the capital of France is Paris," but I want to make sure I'm not mixing it up with other countries.<br><br>Let me think about other capitals I know. For example, Germany's capital is Berlin, Italy's is Rome, Spain's is Madrid. So France's capital should be a major city in the north, maybe. Paris is in the north, so that fits. I think I've heard it said that Paris is the capital, so I'm pretty confident now. Yeah, I think I'm right.<br><br><br>content: Paris</strong>


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


<strong style='color: #00008B;'>reasoing_content: Okay, so the user is in New York and wants to know the current date and time, as well as the weather. They mentioned using functions to get this information. Let me break this down step by step.<br><br>First, I need to figure out which functions to use. The user mentioned two functions: get_current_weather and get_current_date. I should use both of them.<br><br>Starting with the date. The get_current_date function requires a timezone parameter. The user is in New York, so the timezone should be 'America/New_York'. I'll structure the function call as <function=date>{{"timezone": "America/New_York"}}</function>.<br><br>Next, for the weather. The get_current_weather function needs a city, state, and unit. The city is New York, the state is NY, and the unit is probably Fahrenheit since that's a common request. So the parameters would be {'city': 'New York', 'state': 'NY', 'unit': 'fahrenheit'}. I'll call this function as <function=weather>{parameters}</function>.<br><br>I should make sure to include both function calls in the response, each on their own line with the correct syntax. Also, I'll add sources at the end to indicate where the information came from, using the respective function names as references.<br><br>Putting it all together, I'll structure the response with each function call on a separate line, followed by a summary of the results and the sources. That way, the user gets a clear and organized answer.<br><br><br>content: <function=get_current_date>{  <br>  "timezone": "America/New_York"  <br>}</function>  <br><function=get_current_weather>{  <br>  "city": "New York",  <br>  "state": "NY",  <br>  "unit": "fahrenheit"  <br>}</function>  <br><br>Sources:  <br>- Date: get_current_date  <br>- Weather: get_current_weather</strong>


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

    {'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down. First, I need to identify what the capital of France is. I know that Paris is the capital, so that\'s straightforward. \n\nNext, I need to find the population. I remember that Paris is a major city, so its population is quite large. I think it\'s over 3 million, but I\'m not exactly sure of the exact number. Maybe I should double-check that. \n\nWait, I recall that the population figure can vary depending on the source and the year. The user didn\'t specify a particular year, so I should probably go with the most recent estimate. I believe the population is around 3,500,000 as of 2023. \n\nNow, I need to structure this information into a JSON format. JSON typically uses key-value pairs, so I\'ll create an object with keys like "city", "population", and maybe "country" since the user mentioned France. \n\nI should make sure the keys are in English to keep it clear. The city is Paris, the population is 3,500,000, and the country is France. I\'ll format this into a JSON object, ensuring proper syntax with commas and quotation marks. \n\nI also need to present this in a way that\'s easy to read, so I\'ll put each key on a new line. That way, the user can quickly see the information without confusion. \n\nI wonder if the user needs more details, like the exact current population or additional statistics. But since they only asked for the capital and population, I\'ll stick to that. \n\nLastly, I\'ll make sure the JSON is valid by checking the syntax. No trailing commas, proper use of braces, and correct quotation marks. That should cover everything the user needs.\n</think>{\n  "name": "Paris",\n  "population": 3500000\n}', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 13, 5512, 11, 358, 1184, 311, 10542, 1128, 279, 6722, 315, 9625, 374, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4710, 5847, 11, 358, 1184, 311, 1477, 279, 7042, 13, 358, 6099, 429, 12095, 374, 264, 3598, 3283, 11, 773, 1181, 7042, 374, 5008, 3460, 13, 358, 1744, 432, 594, 916, 220, 18, 3526, 11, 714, 358, 2776, 537, 6896, 2704, 315, 279, 4734, 1372, 13, 10696, 358, 1265, 1990, 15934, 429, 13, 4710, 14190, 11, 358, 19091, 429, 279, 7042, 7071, 646, 13289, 11649, 389, 279, 2530, 323, 279, 1042, 13, 576, 1196, 3207, 944, 13837, 264, 3953, 1042, 11, 773, 358, 1265, 4658, 728, 448, 279, 1429, 3213, 16045, 13, 358, 4411, 279, 7042, 374, 2163, 220, 18, 11, 20, 15, 15, 11, 15, 15, 15, 438, 315, 220, 17, 15, 17, 18, 13, 4710, 7039, 11, 358, 1184, 311, 5944, 419, 1995, 1119, 264, 4718, 3561, 13, 4718, 11136, 5711, 1376, 19083, 13530, 11, 773, 358, 3278, 1855, 458, 1633, 448, 6894, 1075, 330, 8926, 497, 330, 44441, 497, 323, 7196, 330, 11141, 1, 2474, 279, 1196, 9733, 9625, 13, 4710, 40, 1265, 1281, 2704, 279, 6894, 525, 304, 6364, 311, 2506, 432, 2797, 13, 576, 3283, 374, 12095, 11, 279, 7042, 374, 220, 18, 11, 20, 15, 15, 11, 15, 15, 15, 11, 323, 279, 3146, 374, 9625, 13, 358, 3278, 3561, 419, 1119, 264, 4718, 1633, 11, 22573, 6169, 19482, 448, 76602, 323, 54231, 15423, 13, 4710, 40, 1083, 1184, 311, 3042, 419, 304, 264, 1616, 429, 594, 4135, 311, 1349, 11, 773, 358, 3278, 2182, 1817, 1376, 389, 264, 501, 1555, 13, 2938, 1616, 11, 279, 1196, 646, 6157, 1490, 279, 1995, 2041, 21340, 13, 4710, 40, 5775, 421, 279, 1196, 3880, 803, 3565, 11, 1075, 279, 4734, 1482, 7042, 476, 5107, 13142, 13, 1988, 2474, 807, 1172, 4588, 369, 279, 6722, 323, 7042, 11, 358, 3278, 9214, 311, 429, 13, 4710, 80486, 11, 358, 3278, 1281, 2704, 279, 4718, 374, 2697, 553, 13295, 279, 19482, 13, 2308, 27748, 76602, 11, 6169, 990, 315, 59191, 11, 323, 4396, 54231, 15423, 13, 2938, 1265, 3421, 4297, 279, 1196, 3880, 624, 151649, 515, 220, 330, 606, 788, 330, 59604, 756, 220, 330, 44441, 788, 220, 18, 20, 15, 15, 15, 15, 15, 198, 92, 151643], 'meta_info': {'id': 'f92f8796e40c4817a21aae6d87e5c2fa', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 412, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 3.127250402001664, 'response_sent_to_client_ts': 1774991418.5014656}}



<strong style='color: #00008B;'>reasoing_content: Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down. First, I need to identify what the capital of France is. I know that Paris is the capital, so that's straightforward. <br><br>Next, I need to find the population. I remember that Paris is a major city, so its population is quite large. I think it's over 3 million, but I'm not exactly sure of the exact number. Maybe I should double-check that. <br><br>Wait, I recall that the population figure can vary depending on the source and the year. The user didn't specify a particular year, so I should probably go with the most recent estimate. I believe the population is around 3,500,000 as of 2023. <br><br>Now, I need to structure this information into a JSON format. JSON typically uses key-value pairs, so I'll create an object with keys like "city", "population", and maybe "country" since the user mentioned France. <br><br>I should make sure the keys are in English to keep it clear. The city is Paris, the population is 3,500,000, and the country is France. I'll format this into a JSON object, ensuring proper syntax with commas and quotation marks. <br><br>I also need to present this in a way that's easy to read, so I'll put each key on a new line. That way, the user can quickly see the information without confusion. <br><br>I wonder if the user needs more details, like the exact current population or additional statistics. But since they only asked for the capital and population, I'll stick to that. <br><br>Lastly, I'll make sure the JSON is valid by checking the syntax. No trailing commas, proper use of braces, and correct quotation marks. That should cover everything the user needs.<br><br><br>content: {<br>  "name": "Paris",<br>  "population": 3500000<br>}</strong>


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


<strong style='color: #00008B;'>{'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. I know that Paris is the capital, so that\'s straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I\'m not sure of the exact number. I think it\'s around 2 million, but I should verify that.\n\nI\'ll check a reliable source, maybe the official Paris Municipality website or a recent census. Let me see... Yes, according to the latest data, the population is approximately 2,174,300 as of 2023. That seems accurate.\n\nNext, I need to structure this information into a JSON format. JSON requires key-value pairs, so I\'ll create an object with keys like "city", "population", and "country". The city is Paris, the population is the number I found, and the country is France.\n\nI should make sure the JSON syntax is correct. Each key should be in quotes, and the values as well. The entire structure should be enclosed in curly braces. I\'ll format it properly to avoid any syntax errors.\n\nPutting it all together, the JSON object will have the city, population, and country. I\'ll present this to the user, making sure it\'s clear and easy to understand. I don\'t think the user needs anything more detailed, so this should suffice.\n\nI should also consider if the user might need additional information, like the area or age distribution, but since they only asked for population, I\'ll stick to that. Maybe they\'ll ask for more details later, but for now, this response should be helpful.\n</think>{\n  "name": "Paris",\n  "population": 2174300\n}', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1265, 1477, 279, 1429, 3213, 7042, 821, 13, 358, 6099, 429, 279, 7042, 315, 12095, 702, 1012, 7826, 11, 714, 358, 2776, 537, 2704, 315, 279, 4734, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 10146, 429, 382, 40, 3278, 1779, 264, 14720, 2530, 11, 7196, 279, 3946, 12095, 35703, 2719, 3910, 476, 264, 3213, 43602, 13, 6771, 752, 1490, 1112, 7414, 11, 4092, 311, 279, 5535, 821, 11, 279, 7042, 374, 13187, 220, 17, 11, 16, 22, 19, 11, 18, 15, 15, 438, 315, 220, 17, 15, 17, 18, 13, 2938, 4977, 13382, 382, 5847, 11, 358, 1184, 311, 5944, 419, 1995, 1119, 264, 4718, 3561, 13, 4718, 7460, 1376, 19083, 13530, 11, 773, 358, 3278, 1855, 458, 1633, 448, 6894, 1075, 330, 8926, 497, 330, 44441, 497, 323, 330, 11141, 3263, 576, 3283, 374, 12095, 11, 279, 7042, 374, 279, 1372, 358, 1730, 11, 323, 279, 3146, 374, 9625, 382, 40, 1265, 1281, 2704, 279, 4718, 19482, 374, 4396, 13, 8886, 1376, 1265, 387, 304, 17194, 11, 323, 279, 2750, 438, 1632, 13, 576, 4453, 5944, 1265, 387, 43810, 304, 68103, 59191, 13, 358, 3278, 3561, 432, 10277, 311, 5648, 894, 19482, 5975, 382, 97904, 432, 678, 3786, 11, 279, 4718, 1633, 686, 614, 279, 3283, 11, 7042, 11, 323, 3146, 13, 358, 3278, 3042, 419, 311, 279, 1196, 11, 3259, 2704, 432, 594, 2797, 323, 4135, 311, 3535, 13, 358, 1513, 944, 1744, 279, 1196, 3880, 4113, 803, 11682, 11, 773, 419, 1265, 76156, 382, 40, 1265, 1083, 2908, 421, 279, 1196, 2578, 1184, 5107, 1995, 11, 1075, 279, 3082, 476, 4231, 7982, 11, 714, 2474, 807, 1172, 4588, 369, 7042, 11, 358, 3278, 9214, 311, 429, 13, 10696, 807, 3278, 2548, 369, 803, 3565, 2937, 11, 714, 369, 1431, 11, 419, 2033, 1265, 387, 10950, 624, 151649, 515, 220, 330, 606, 788, 330, 59604, 756, 220, 330, 44441, 788, 220, 17, 16, 22, 19, 18, 15, 15, 198, 92, 151643], 'meta_info': {'id': '61add30983fa4af5b5c1a578eabebc9d', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 384, 'cached_tokens': 22, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 3.2889006909681484, 'response_sent_to_client_ts': 1774991421.7997055}}</strong>


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

    [{'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '8d6fb17e7779471aa4bdca80a8133bce', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.14898287504911423, 'response_sent_to_client_ts': 1774991421.9950478}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': 'b98c82f9d2ca4712823c573e6075b144', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.14890074101276696, 'response_sent_to_client_ts': 1774991421.9950602}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '2525b38b77b243c69bbb577e831c1578', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.14886646100785583, 'response_sent_to_client_ts': 1774991421.995065}}]


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

    {'text': ' France, and the \n\\( n \\)  \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\(', 'output_ids': [9625, 11, 323, 279, 220, 198, 44292, 308, 1124, 8, 220, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767], 'meta_info': {'id': '7dc18963f2034fe5b860df4800a23d46', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 6, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 30.485458386014216, 'response_sent_to_client_ts': 1774991452.4870396}}


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


<strong style='color: #00008B;'>{'text': 'Alright, so I need to figure out the information and population of the capital of France in JSON format. Let me start by recalling what the capital is. I think Paris is the capital of France. That\'s right, Paris is the political, commercial, and cultural center, so it makes sense that it\'s the capital.\n\nNow, I need to find the population. I remember reading somewhere that Paris is one of the most populous cities in the world. I think the population is around 2 million, but I\'m not entirely sure. I should double-check that. Maybe it\'s higher now? I think it\'s actually a bit over 2 million, perhaps 2,150,000 or something like that.\n\nLet me consider some factors that might affect the population. Paris has a large city area, so it includes more than just the administrative center. The metropolitan area is bigger. I think the metropolitan area population can be around 11 million, but I\'m not certain. However, the question specifically asks for the capital, which I believe refers to the city proper.\n\nWait, sometimes people get confused between the city and the metropolitan area. If I\'m unclear, maybe I should just provide both. But since the user asked for the capital, I should stick to the city. So, population around 2,155,000 for the city and perhaps 565,000 for the arrondissement, which is essentially the city limits.\n\nI should structure this into a JSON format. JSON typically uses keys, so I\'ll create a key like "capital" with the name, "population" with the number. I\'ll format the numbers correctly, using commas as thousand separators. So the population would be "2,155,000".\n\nPutting it all together, the JSON should look neat with proper syntax. Let me write that out. Make sure the quotes around the keys and the string values are correct. Also, commas are important to separate the key-value pairs. I don\'t want any syntax errors, so I\'ll be cautious with the braces and brackets.\n\nDouble-checking, Paris is indeed the capital, the population is roughly 2.155 million, and the JSON structure seems correct. I think that\'s all I need for this query.\n</think>\n\n```json\n{\n  "capital": "Paris",\n  "population": "2,155,000"\n}\n```', 'output_ids': [71486, 11, 773, 358, 1184, 311, 7071, 700, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1191, 553, 88646, 1128, 279, 6722, 374, 13, 358, 1744, 12095, 374, 279, 6722, 315, 9625, 13, 2938, 594, 1290, 11, 12095, 374, 279, 4948, 11, 8353, 11, 323, 12752, 4126, 11, 773, 432, 3643, 5530, 429, 432, 594, 279, 6722, 382, 7039, 11, 358, 1184, 311, 1477, 279, 7042, 13, 358, 6099, 5290, 14696, 429, 12095, 374, 825, 315, 279, 1429, 94451, 9720, 304, 279, 1879, 13, 358, 1744, 279, 7042, 374, 2163, 220, 17, 3526, 11, 714, 358, 2776, 537, 11368, 2704, 13, 358, 1265, 1990, 15934, 429, 13, 10696, 432, 594, 5080, 1431, 30, 358, 1744, 432, 594, 3520, 264, 2699, 916, 220, 17, 3526, 11, 8365, 220, 17, 11, 16, 20, 15, 11, 15, 15, 15, 476, 2494, 1075, 429, 382, 10061, 752, 2908, 1045, 9363, 429, 2578, 7802, 279, 7042, 13, 12095, 702, 264, 3460, 3283, 3082, 11, 773, 432, 5646, 803, 1091, 1101, 279, 22707, 4126, 13, 576, 57406, 3082, 374, 11243, 13, 358, 1744, 279, 57406, 3082, 7042, 646, 387, 2163, 220, 16, 16, 3526, 11, 714, 358, 2776, 537, 3654, 13, 4354, 11, 279, 3405, 11689, 17064, 369, 279, 6722, 11, 892, 358, 4411, 19257, 311, 279, 3283, 6169, 382, 14190, 11, 7025, 1251, 633, 21815, 1948, 279, 3283, 323, 279, 57406, 3082, 13, 1416, 358, 2776, 24416, 11, 7196, 358, 1265, 1101, 3410, 2176, 13, 1988, 2474, 279, 1196, 4588, 369, 279, 6722, 11, 358, 1265, 9214, 311, 279, 3283, 13, 2055, 11, 7042, 2163, 220, 17, 11, 16, 20, 20, 11, 15, 15, 15, 369, 279, 3283, 323, 8365, 220, 20, 21, 20, 11, 15, 15, 15, 369, 279, 2890, 2111, 49653, 11, 892, 374, 15791, 279, 3283, 13388, 382, 40, 1265, 5944, 419, 1119, 264, 4718, 3561, 13, 4718, 11136, 5711, 6894, 11, 773, 358, 3278, 1855, 264, 1376, 1075, 330, 65063, 1, 448, 279, 829, 11, 330, 44441, 1, 448, 279, 1372, 13, 358, 3278, 3561, 279, 5109, 12440, 11, 1667, 76602, 438, 16183, 82502, 13, 2055, 279, 7042, 1035, 387, 330, 17, 11, 16, 20, 20, 11, 15, 15, 15, 11436, 97904, 432, 678, 3786, 11, 279, 4718, 1265, 1401, 28485, 448, 6169, 19482, 13, 6771, 752, 3270, 429, 700, 13, 7405, 2704, 279, 17194, 2163, 279, 6894, 323, 279, 914, 2750, 525, 4396, 13, 7281, 11, 76602, 525, 2989, 311, 8651, 279, 1376, 19083, 13530, 13, 358, 1513, 944, 1366, 894, 19482, 5975, 11, 773, 358, 3278, 387, 45778, 448, 279, 59191, 323, 38929, 382, 7378, 15934, 287, 11, 12095, 374, 12824, 279, 6722, 11, 279, 7042, 374, 17267, 220, 17, 13, 16, 20, 20, 3526, 11, 323, 279, 4718, 5944, 4977, 4396, 13, 358, 1744, 429, 594, 678, 358, 1184, 369, 419, 3239, 624, 151649, 271, 73594, 2236, 198, 515, 220, 330, 65063, 788, 330, 59604, 756, 220, 330, 44441, 788, 330, 17, 11, 16, 20, 20, 11, 15, 15, 15, 698, 532, 73594, 151643], 'meta_info': {'id': '9eb0fffcb6654237a67ac34f5134d54c', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 503, 'cached_tokens': 22, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 4.47851969092153, 'response_sent_to_client_ts': 1774991456.974564}}</strong>



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


    2026-03-31 21:11:08.677 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:11:08] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:11:08.677 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:11:08] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:11:08.677 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:11:08] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:11:08.677 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:11:08] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:11:08.677 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:11:08] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:01<00:01,  1.15s/it]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.49s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.44s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:51,  3.00s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:51,  3.00s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:22,  1.48s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:22,  1.48s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:49,  1.12it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:49,  1.12it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:33,  1.63it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:33,  1.63it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:25,  2.05it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:25,  2.05it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:22,  2.28it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:22,  2.28it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:20,  2.54it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:20,  2.54it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:17,  2.79it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:17,  2.79it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:05<00:15,  3.08it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:05<00:15,  3.08it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:05<00:13,  3.43it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:05<00:13,  3.43it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:05<00:12,  3.73it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:05<00:12,  3.73it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:05<00:11,  4.11it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:05<00:11,  4.11it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:06<00:11,  4.09it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:06<00:11,  4.09it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:06<00:10,  4.30it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:06<00:10,  4.30it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:06<00:08,  4.93it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:06<00:08,  4.93it/s]

    Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:06<00:07,  5.49it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:06<00:07,  5.49it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:06<00:06,  6.18it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:06<00:06,  6.18it/s]

    Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:06<00:05,  6.88it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:06<00:05,  6.88it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:06<00:05,  7.58it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:06<00:05,  7.58it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:06<00:05,  7.58it/s]

    Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:07<00:04,  9.10it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:07<00:04,  9.10it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:07<00:04,  9.10it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:07<00:03, 10.87it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:07<00:03, 10.87it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:07<00:03, 10.87it/s]

    Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:07<00:02, 12.55it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:07<00:02, 12.55it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:07<00:02, 12.55it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:07<00:02, 14.10it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:07<00:02, 14.10it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:07<00:02, 14.10it/s]

    Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:07<00:01, 15.53it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:07<00:01, 15.53it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:07<00:01, 15.53it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:07<00:01, 15.53it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:07<00:01, 17.01it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:07<00:01, 17.01it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:07<00:01, 17.01it/s]

    Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:07<00:01, 17.01it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:07<00:01, 18.86it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:07<00:01, 18.86it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:07<00:01, 18.86it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:07<00:01, 18.86it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:07<00:00, 20.23it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:07<00:00, 20.23it/s]

    Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:07<00:00, 20.23it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:08<00:00, 20.23it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:08<00:00, 21.49it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:08<00:00, 21.49it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:08<00:00, 21.49it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:08<00:00, 21.49it/s]

    Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:08<00:00, 22.60it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:08<00:00, 22.60it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:08<00:00, 22.60it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:08<00:00, 22.60it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:08<00:00, 24.46it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:08<00:00, 24.46it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:08<00:00, 24.46it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:08<00:00, 24.46it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:08<00:00, 24.46it/s]

    Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:08<00:00, 26.21it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:08<00:00, 26.21it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:08<00:00, 26.21it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:08<00:00, 26.21it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:08<00:00, 26.21it/s]Compiling num tokens (num_tokens=16):  95%|█████████▍| 55/58 [00:08<00:00, 28.21it/s]Compiling num tokens (num_tokens=12):  95%|█████████▍| 55/58 [00:08<00:00, 28.21it/s]Compiling num tokens (num_tokens=8):  95%|█████████▍| 55/58 [00:08<00:00, 28.21it/s] Compiling num tokens (num_tokens=4):  95%|█████████▍| 55/58 [00:08<00:00, 28.21it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:08<00:00,  6.76it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=34.47 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=34.47 GB):   2%|▏         | 1/58 [00:00<00:43,  1.32it/s]Capturing num tokens (num_tokens=7680 avail_mem=34.43 GB):   2%|▏         | 1/58 [00:00<00:43,  1.32it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=34.43 GB):   3%|▎         | 2/58 [00:01<00:30,  1.83it/s]Capturing num tokens (num_tokens=7168 avail_mem=34.43 GB):   3%|▎         | 2/58 [00:01<00:30,  1.83it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=34.43 GB):   5%|▌         | 3/58 [00:01<00:25,  2.19it/s]Capturing num tokens (num_tokens=6656 avail_mem=33.25 GB):   5%|▌         | 3/58 [00:01<00:25,  2.19it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=33.25 GB):   7%|▋         | 4/58 [00:01<00:23,  2.26it/s]Capturing num tokens (num_tokens=6144 avail_mem=34.41 GB):   7%|▋         | 4/58 [00:01<00:23,  2.26it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=34.41 GB):   9%|▊         | 5/58 [00:02<00:23,  2.25it/s]Capturing num tokens (num_tokens=5632 avail_mem=34.41 GB):   9%|▊         | 5/58 [00:02<00:23,  2.25it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=34.41 GB):  10%|█         | 6/58 [00:02<00:22,  2.36it/s]Capturing num tokens (num_tokens=5120 avail_mem=33.49 GB):  10%|█         | 6/58 [00:02<00:22,  2.36it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=33.49 GB):  12%|█▏        | 7/58 [00:03<00:20,  2.48it/s]Capturing num tokens (num_tokens=4608 avail_mem=33.57 GB):  12%|█▏        | 7/58 [00:03<00:20,  2.48it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=33.57 GB):  14%|█▍        | 8/58 [00:03<00:18,  2.68it/s]Capturing num tokens (num_tokens=4096 avail_mem=33.64 GB):  14%|█▍        | 8/58 [00:03<00:18,  2.68it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=33.64 GB):  16%|█▌        | 9/58 [00:03<00:17,  2.86it/s]Capturing num tokens (num_tokens=3840 avail_mem=34.30 GB):  16%|█▌        | 9/58 [00:03<00:17,  2.86it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=34.30 GB):  17%|█▋        | 10/58 [00:04<00:15,  3.00it/s]Capturing num tokens (num_tokens=3584 avail_mem=33.87 GB):  17%|█▋        | 10/58 [00:04<00:15,  3.00it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=33.87 GB):  19%|█▉        | 11/58 [00:04<00:14,  3.21it/s]Capturing num tokens (num_tokens=3328 avail_mem=32.78 GB):  19%|█▉        | 11/58 [00:04<00:14,  3.21it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=32.78 GB):  21%|██        | 12/58 [00:04<00:13,  3.47it/s]Capturing num tokens (num_tokens=3072 avail_mem=32.54 GB):  21%|██        | 12/58 [00:04<00:13,  3.47it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=32.54 GB):  22%|██▏       | 13/58 [00:04<00:12,  3.74it/s]Capturing num tokens (num_tokens=2816 avail_mem=32.59 GB):  22%|██▏       | 13/58 [00:04<00:12,  3.74it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=32.59 GB):  24%|██▍       | 14/58 [00:04<00:10,  4.05it/s]Capturing num tokens (num_tokens=2560 avail_mem=30.92 GB):  24%|██▍       | 14/58 [00:04<00:10,  4.05it/s]Capturing num tokens (num_tokens=2560 avail_mem=30.92 GB):  26%|██▌       | 15/58 [00:05<00:09,  4.35it/s]Capturing num tokens (num_tokens=2304 avail_mem=30.60 GB):  26%|██▌       | 15/58 [00:05<00:09,  4.35it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=30.60 GB):  28%|██▊       | 16/58 [00:05<00:08,  4.70it/s]Capturing num tokens (num_tokens=2048 avail_mem=30.14 GB):  28%|██▊       | 16/58 [00:05<00:08,  4.70it/s]Capturing num tokens (num_tokens=2048 avail_mem=30.14 GB):  29%|██▉       | 17/58 [00:05<00:08,  4.91it/s]Capturing num tokens (num_tokens=1792 avail_mem=30.55 GB):  29%|██▉       | 17/58 [00:05<00:08,  4.91it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=30.55 GB):  31%|███       | 18/58 [00:05<00:07,  5.20it/s]Capturing num tokens (num_tokens=1536 avail_mem=30.60 GB):  31%|███       | 18/58 [00:05<00:07,  5.20it/s]Capturing num tokens (num_tokens=1536 avail_mem=30.60 GB):  33%|███▎      | 19/58 [00:05<00:06,  5.59it/s]Capturing num tokens (num_tokens=1280 avail_mem=30.20 GB):  33%|███▎      | 19/58 [00:05<00:06,  5.59it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=30.23 GB):  33%|███▎      | 19/58 [00:05<00:06,  5.59it/s]Capturing num tokens (num_tokens=1024 avail_mem=30.23 GB):  36%|███▌      | 21/58 [00:05<00:05,  7.16it/s]Capturing num tokens (num_tokens=960 avail_mem=30.59 GB):  36%|███▌      | 21/58 [00:05<00:05,  7.16it/s] Capturing num tokens (num_tokens=896 avail_mem=30.59 GB):  36%|███▌      | 21/58 [00:06<00:05,  7.16it/s]

    Capturing num tokens (num_tokens=896 avail_mem=30.59 GB):  40%|███▉      | 23/58 [00:06<00:04,  8.37it/s]Capturing num tokens (num_tokens=832 avail_mem=30.29 GB):  40%|███▉      | 23/58 [00:06<00:04,  8.37it/s]Capturing num tokens (num_tokens=768 avail_mem=30.58 GB):  40%|███▉      | 23/58 [00:06<00:04,  8.37it/s]Capturing num tokens (num_tokens=768 avail_mem=30.58 GB):  43%|████▎     | 25/58 [00:06<00:03,  9.54it/s]Capturing num tokens (num_tokens=704 avail_mem=30.58 GB):  43%|████▎     | 25/58 [00:06<00:03,  9.54it/s]

    Capturing num tokens (num_tokens=640 avail_mem=30.41 GB):  43%|████▎     | 25/58 [00:06<00:03,  9.54it/s]Capturing num tokens (num_tokens=640 avail_mem=30.41 GB):  47%|████▋     | 27/58 [00:06<00:02, 10.90it/s]Capturing num tokens (num_tokens=576 avail_mem=30.36 GB):  47%|████▋     | 27/58 [00:06<00:02, 10.90it/s]Capturing num tokens (num_tokens=512 avail_mem=30.53 GB):  47%|████▋     | 27/58 [00:06<00:02, 10.90it/s]Capturing num tokens (num_tokens=512 avail_mem=30.53 GB):  50%|█████     | 29/58 [00:06<00:02, 11.98it/s]Capturing num tokens (num_tokens=480 avail_mem=30.56 GB):  50%|█████     | 29/58 [00:06<00:02, 11.98it/s]

    Capturing num tokens (num_tokens=448 avail_mem=30.55 GB):  50%|█████     | 29/58 [00:06<00:02, 11.98it/s]Capturing num tokens (num_tokens=448 avail_mem=30.55 GB):  53%|█████▎    | 31/58 [00:06<00:02, 12.90it/s]Capturing num tokens (num_tokens=416 avail_mem=30.55 GB):  53%|█████▎    | 31/58 [00:06<00:02, 12.90it/s]Capturing num tokens (num_tokens=384 avail_mem=30.54 GB):  53%|█████▎    | 31/58 [00:06<00:02, 12.90it/s]

    Capturing num tokens (num_tokens=384 avail_mem=30.54 GB):  57%|█████▋    | 33/58 [00:06<00:01, 13.21it/s]Capturing num tokens (num_tokens=352 avail_mem=30.53 GB):  57%|█████▋    | 33/58 [00:06<00:01, 13.21it/s]Capturing num tokens (num_tokens=320 avail_mem=30.52 GB):  57%|█████▋    | 33/58 [00:06<00:01, 13.21it/s]Capturing num tokens (num_tokens=320 avail_mem=30.52 GB):  60%|██████    | 35/58 [00:06<00:01, 14.71it/s]Capturing num tokens (num_tokens=288 avail_mem=30.52 GB):  60%|██████    | 35/58 [00:06<00:01, 14.71it/s]Capturing num tokens (num_tokens=256 avail_mem=30.51 GB):  60%|██████    | 35/58 [00:07<00:01, 14.71it/s]

    Capturing num tokens (num_tokens=256 avail_mem=30.51 GB):  64%|██████▍   | 37/58 [00:07<00:01, 10.88it/s]Capturing num tokens (num_tokens=240 avail_mem=30.50 GB):  64%|██████▍   | 37/58 [00:07<00:01, 10.88it/s]Capturing num tokens (num_tokens=224 avail_mem=30.50 GB):  64%|██████▍   | 37/58 [00:07<00:01, 10.88it/s]Capturing num tokens (num_tokens=208 avail_mem=30.49 GB):  64%|██████▍   | 37/58 [00:07<00:01, 10.88it/s]Capturing num tokens (num_tokens=208 avail_mem=30.49 GB):  69%|██████▉   | 40/58 [00:07<00:01, 13.77it/s]Capturing num tokens (num_tokens=192 avail_mem=30.48 GB):  69%|██████▉   | 40/58 [00:07<00:01, 13.77it/s]Capturing num tokens (num_tokens=176 avail_mem=30.48 GB):  69%|██████▉   | 40/58 [00:07<00:01, 13.77it/s]

    Capturing num tokens (num_tokens=160 avail_mem=30.47 GB):  69%|██████▉   | 40/58 [00:07<00:01, 13.77it/s]Capturing num tokens (num_tokens=160 avail_mem=30.47 GB):  74%|███████▍  | 43/58 [00:07<00:00, 16.37it/s]Capturing num tokens (num_tokens=144 avail_mem=30.46 GB):  74%|███████▍  | 43/58 [00:07<00:00, 16.37it/s]Capturing num tokens (num_tokens=128 avail_mem=30.46 GB):  74%|███████▍  | 43/58 [00:07<00:00, 16.37it/s]Capturing num tokens (num_tokens=112 avail_mem=30.46 GB):  74%|███████▍  | 43/58 [00:07<00:00, 16.37it/s]Capturing num tokens (num_tokens=112 avail_mem=30.46 GB):  79%|███████▉  | 46/58 [00:07<00:00, 18.76it/s]Capturing num tokens (num_tokens=96 avail_mem=30.45 GB):  79%|███████▉  | 46/58 [00:07<00:00, 18.76it/s] Capturing num tokens (num_tokens=80 avail_mem=30.44 GB):  79%|███████▉  | 46/58 [00:07<00:00, 18.76it/s]

    Capturing num tokens (num_tokens=64 avail_mem=30.43 GB):  79%|███████▉  | 46/58 [00:07<00:00, 18.76it/s]Capturing num tokens (num_tokens=64 avail_mem=30.43 GB):  84%|████████▍ | 49/58 [00:07<00:00, 20.98it/s]Capturing num tokens (num_tokens=48 avail_mem=30.43 GB):  84%|████████▍ | 49/58 [00:07<00:00, 20.98it/s]Capturing num tokens (num_tokens=32 avail_mem=30.42 GB):  84%|████████▍ | 49/58 [00:07<00:00, 20.98it/s]Capturing num tokens (num_tokens=28 avail_mem=30.43 GB):  84%|████████▍ | 49/58 [00:07<00:00, 20.98it/s]Capturing num tokens (num_tokens=28 avail_mem=30.43 GB):  90%|████████▉ | 52/58 [00:07<00:00, 22.96it/s]Capturing num tokens (num_tokens=24 avail_mem=30.41 GB):  90%|████████▉ | 52/58 [00:07<00:00, 22.96it/s]Capturing num tokens (num_tokens=20 avail_mem=30.42 GB):  90%|████████▉ | 52/58 [00:07<00:00, 22.96it/s]

    Capturing num tokens (num_tokens=16 avail_mem=30.39 GB):  90%|████████▉ | 52/58 [00:07<00:00, 22.96it/s]Capturing num tokens (num_tokens=16 avail_mem=30.39 GB):  95%|█████████▍| 55/58 [00:07<00:00, 24.55it/s]Capturing num tokens (num_tokens=12 avail_mem=30.38 GB):  95%|█████████▍| 55/58 [00:07<00:00, 24.55it/s]Capturing num tokens (num_tokens=8 avail_mem=30.39 GB):  95%|█████████▍| 55/58 [00:07<00:00, 24.55it/s] Capturing num tokens (num_tokens=4 avail_mem=30.38 GB):  95%|█████████▍| 55/58 [00:07<00:00, 24.55it/s]Capturing num tokens (num_tokens=4 avail_mem=30.38 GB): 100%|██████████| 58/58 [00:08<00:00,  7.22it/s]


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
    Generated text: Berlin is the capital of Germany
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
    
    Generated text: Alright, so I just got a request from a user asking for the information and population of the capital of France in JSON format. Hmm, let me think about how to approach this.
    
    First, I need to identify the capital of France. I'm pretty sure it's Paris, but I'll double-check just to be sure. Yep, Paris is definitely the capital. Now, I should find the latest population data for Paris. I know population numbers can change yearly, so I need the most recent figure.
    
    I'll look up the current population. I remember reading somewhere that Paris has been growing steadily. I think it's around 2 million people. Wait, let me verify that. Oh, yes, the official estimate is about 2,173,000 as of 2023. That sounds right.
    
    Now, the user wants this information in JSON format. JSON is a data format that's easy to read and write, so I'll structure it accordingly. I should include fields like the city name, population, and maybe a year if they want context, but the user didn't specify, so I'll just include the population number.
    
    Putting it all together, I'll create a JSON object with the key "capital" and a nested object containing "name" and "population". That should meet the user's requirements.
    
    I should also make sure the JSON is properly formatted and valid. Let me write it out: { "capital": { "name": "Paris", "population": 2173000 } }. Yep, that looks correct.
    
    I wonder if the user is a student working on a project or maybe a developer needing data in JSON. Either way, providing a clear and concise response is key. I hope this helps them with whatever they're working on!
    </think>
    
    Here is the information and population of the capital of France in JSON format:
    
    ```json
    {
      "capital": {
        "name": "Paris",
        "population": 2173000
      }
    }
    ```



```python
llm.shutdown()
```
