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
    2026-03-31 06:17:56.454 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 06:17:56] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 06:17:56.454 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 06:17:56] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 06:17:56.454 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 06:17:56] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 06:17:56.454 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 06:17:56] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 06:17:56.454 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 06:17:56] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:00<00:00,  1.06it/s]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.32s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.26s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:50,  2.99s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:50,  2.99s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:38,  1.76s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:38,  1.76s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:03,  1.15s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:03,  1.15s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:46,  1.17it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:46,  1.17it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:35,  1.50it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:35,  1.50it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:28,  1.80it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:28,  1.80it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:23,  2.14it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:23,  2.14it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:05<00:20,  2.47it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:05<00:20,  2.47it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:06<00:17,  2.80it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:06<00:17,  2.80it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:06<00:15,  3.07it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:06<00:15,  3.07it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:06<00:13,  3.48it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:06<00:13,  3.48it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:06<00:11,  3.99it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:06<00:11,  3.99it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:06<00:10,  4.46it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:06<00:10,  4.46it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:07<00:08,  4.98it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:07<00:08,  4.98it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:07<00:07,  5.58it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:07<00:07,  5.58it/s]

    Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:07<00:07,  5.58it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:07<00:05,  7.05it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:07<00:05,  7.05it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:07<00:05,  7.05it/s]

    Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:07<00:04,  8.59it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:07<00:04,  8.59it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:07<00:04,  8.59it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:07<00:03, 10.32it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:07<00:03, 10.32it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:07<00:03, 10.32it/s]

    Compiling num tokens (num_tokens=832):  36%|███▌      | 21/58 [00:07<00:03, 10.32it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:07<00:02, 13.51it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:07<00:02, 13.51it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:07<00:02, 13.51it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:07<00:02, 13.51it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:07<00:01, 16.75it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:07<00:01, 16.75it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:08<00:01, 16.75it/s]

    Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:08<00:01, 16.75it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:08<00:01, 16.75it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:08<00:01, 20.96it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:08<00:01, 20.96it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:08<00:01, 20.96it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:08<00:01, 20.96it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:08<00:01, 20.96it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:08<00:00, 24.06it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:08<00:00, 24.06it/s]

    Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:08<00:00, 24.06it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:08<00:00, 24.06it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:08<00:00, 24.06it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:08<00:00, 27.67it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:08<00:00, 27.67it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:08<00:00, 27.67it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:08<00:00, 27.67it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:08<00:00, 27.67it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:08<00:00, 30.00it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:08<00:00, 30.00it/s]

    Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:08<00:00, 30.00it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:08<00:00, 30.00it/s]Compiling num tokens (num_tokens=96):  74%|███████▍  | 43/58 [00:08<00:00, 30.00it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:08<00:00, 32.11it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:08<00:00, 32.11it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:08<00:00, 32.11it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:08<00:00, 32.11it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:08<00:00, 32.11it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:08<00:00, 32.11it/s]Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:08<00:00, 32.11it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:08<00:00, 39.49it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:08<00:00, 39.49it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:08<00:00, 39.49it/s]

    Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:08<00:00, 39.49it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:08<00:00, 39.49it/s] Compiling num tokens (num_tokens=4):  91%|█████████▏| 53/58 [00:08<00:00, 39.49it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:08<00:00,  6.66it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=19.81 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=19.81 GB):   2%|▏         | 1/58 [00:00<00:35,  1.60it/s]Capturing num tokens (num_tokens=7680 avail_mem=19.95 GB):   2%|▏         | 1/58 [00:00<00:35,  1.60it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=19.95 GB):   3%|▎         | 2/58 [00:01<00:34,  1.60it/s]Capturing num tokens (num_tokens=7168 avail_mem=20.01 GB):   3%|▎         | 2/58 [00:01<00:34,  1.60it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=20.01 GB):   5%|▌         | 3/58 [00:01<00:33,  1.64it/s]Capturing num tokens (num_tokens=6656 avail_mem=20.90 GB):   5%|▌         | 3/58 [00:01<00:33,  1.64it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=20.90 GB):   7%|▋         | 4/58 [00:02<00:31,  1.72it/s]Capturing num tokens (num_tokens=6144 avail_mem=20.91 GB):   7%|▋         | 4/58 [00:02<00:31,  1.72it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=20.91 GB):   9%|▊         | 5/58 [00:02<00:28,  1.88it/s]Capturing num tokens (num_tokens=5632 avail_mem=20.91 GB):   9%|▊         | 5/58 [00:02<00:28,  1.88it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=20.91 GB):  10%|█         | 6/58 [00:03<00:27,  1.92it/s]Capturing num tokens (num_tokens=5120 avail_mem=20.91 GB):  10%|█         | 6/58 [00:03<00:27,  1.92it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=20.91 GB):  12%|█▏        | 7/58 [00:03<00:26,  1.91it/s]Capturing num tokens (num_tokens=4608 avail_mem=20.40 GB):  12%|█▏        | 7/58 [00:03<00:26,  1.91it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=20.40 GB):  14%|█▍        | 8/58 [00:04<00:24,  2.02it/s]Capturing num tokens (num_tokens=4096 avail_mem=20.57 GB):  14%|█▍        | 8/58 [00:04<00:24,  2.02it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=20.57 GB):  16%|█▌        | 9/58 [00:04<00:21,  2.33it/s]Capturing num tokens (num_tokens=3840 avail_mem=20.47 GB):  16%|█▌        | 9/58 [00:04<00:21,  2.33it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=20.47 GB):  17%|█▋        | 10/58 [00:04<00:19,  2.46it/s]Capturing num tokens (num_tokens=3584 avail_mem=43.72 GB):  17%|█▋        | 10/58 [00:04<00:19,  2.46it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=43.72 GB):  19%|█▉        | 11/58 [00:05<00:16,  2.86it/s]Capturing num tokens (num_tokens=3328 avail_mem=43.75 GB):  19%|█▉        | 11/58 [00:05<00:16,  2.86it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=43.75 GB):  21%|██        | 12/58 [00:05<00:14,  3.28it/s]Capturing num tokens (num_tokens=3072 avail_mem=44.15 GB):  21%|██        | 12/58 [00:05<00:14,  3.28it/s]Capturing num tokens (num_tokens=3072 avail_mem=44.15 GB):  22%|██▏       | 13/58 [00:05<00:12,  3.67it/s]Capturing num tokens (num_tokens=2816 avail_mem=44.19 GB):  22%|██▏       | 13/58 [00:05<00:12,  3.67it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=44.19 GB):  24%|██▍       | 14/58 [00:05<00:10,  4.08it/s]Capturing num tokens (num_tokens=2560 avail_mem=43.92 GB):  24%|██▍       | 14/58 [00:05<00:10,  4.08it/s]Capturing num tokens (num_tokens=2560 avail_mem=43.92 GB):  26%|██▌       | 15/58 [00:05<00:09,  4.67it/s]Capturing num tokens (num_tokens=2304 avail_mem=43.92 GB):  26%|██▌       | 15/58 [00:05<00:09,  4.67it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=43.92 GB):  28%|██▊       | 16/58 [00:06<00:08,  5.06it/s]Capturing num tokens (num_tokens=2048 avail_mem=44.15 GB):  28%|██▊       | 16/58 [00:06<00:08,  5.06it/s]Capturing num tokens (num_tokens=2048 avail_mem=44.15 GB):  29%|██▉       | 17/58 [00:06<00:07,  5.82it/s]Capturing num tokens (num_tokens=1792 avail_mem=44.15 GB):  29%|██▉       | 17/58 [00:06<00:07,  5.82it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=44.15 GB):  31%|███       | 18/58 [00:06<00:06,  6.40it/s]Capturing num tokens (num_tokens=1536 avail_mem=44.15 GB):  31%|███       | 18/58 [00:06<00:06,  6.40it/s]Capturing num tokens (num_tokens=1280 avail_mem=44.14 GB):  31%|███       | 18/58 [00:06<00:06,  6.40it/s]Capturing num tokens (num_tokens=1280 avail_mem=44.14 GB):  34%|███▍      | 20/58 [00:06<00:04,  8.15it/s]Capturing num tokens (num_tokens=1024 avail_mem=44.02 GB):  34%|███▍      | 20/58 [00:06<00:04,  8.15it/s]

    Capturing num tokens (num_tokens=960 avail_mem=44.06 GB):  34%|███▍      | 20/58 [00:06<00:04,  8.15it/s] Capturing num tokens (num_tokens=960 avail_mem=44.06 GB):  38%|███▊      | 22/58 [00:06<00:04,  8.60it/s]Capturing num tokens (num_tokens=896 avail_mem=44.05 GB):  38%|███▊      | 22/58 [00:06<00:04,  8.60it/s]Capturing num tokens (num_tokens=832 avail_mem=44.02 GB):  38%|███▊      | 22/58 [00:06<00:04,  8.60it/s]Capturing num tokens (num_tokens=832 avail_mem=44.02 GB):  41%|████▏     | 24/58 [00:06<00:03, 10.09it/s]Capturing num tokens (num_tokens=768 avail_mem=44.02 GB):  41%|████▏     | 24/58 [00:06<00:03, 10.09it/s]

    Capturing num tokens (num_tokens=704 avail_mem=44.08 GB):  41%|████▏     | 24/58 [00:06<00:03, 10.09it/s]Capturing num tokens (num_tokens=704 avail_mem=44.08 GB):  45%|████▍     | 26/58 [00:06<00:02, 11.22it/s]Capturing num tokens (num_tokens=640 avail_mem=44.09 GB):  45%|████▍     | 26/58 [00:06<00:02, 11.22it/s]

    Capturing num tokens (num_tokens=576 avail_mem=44.08 GB):  45%|████▍     | 26/58 [00:07<00:02, 11.22it/s]Capturing num tokens (num_tokens=576 avail_mem=44.08 GB):  48%|████▊     | 28/58 [00:07<00:02, 10.48it/s]Capturing num tokens (num_tokens=512 avail_mem=44.08 GB):  48%|████▊     | 28/58 [00:07<00:02, 10.48it/s]Capturing num tokens (num_tokens=480 avail_mem=44.07 GB):  48%|████▊     | 28/58 [00:07<00:02, 10.48it/s]Capturing num tokens (num_tokens=448 avail_mem=44.07 GB):  48%|████▊     | 28/58 [00:07<00:02, 10.48it/s]Capturing num tokens (num_tokens=448 avail_mem=44.07 GB):  53%|█████▎    | 31/58 [00:07<00:02, 13.24it/s]Capturing num tokens (num_tokens=416 avail_mem=44.06 GB):  53%|█████▎    | 31/58 [00:07<00:02, 13.24it/s]

    Capturing num tokens (num_tokens=384 avail_mem=44.05 GB):  53%|█████▎    | 31/58 [00:07<00:02, 13.24it/s]Capturing num tokens (num_tokens=352 avail_mem=44.04 GB):  53%|█████▎    | 31/58 [00:07<00:02, 13.24it/s]Capturing num tokens (num_tokens=352 avail_mem=44.04 GB):  59%|█████▊    | 34/58 [00:07<00:01, 15.79it/s]Capturing num tokens (num_tokens=320 avail_mem=44.03 GB):  59%|█████▊    | 34/58 [00:07<00:01, 15.79it/s]Capturing num tokens (num_tokens=288 avail_mem=44.03 GB):  59%|█████▊    | 34/58 [00:07<00:01, 15.79it/s]Capturing num tokens (num_tokens=256 avail_mem=44.02 GB):  59%|█████▊    | 34/58 [00:07<00:01, 15.79it/s]

    Capturing num tokens (num_tokens=256 avail_mem=44.02 GB):  64%|██████▍   | 37/58 [00:07<00:01, 17.24it/s]Capturing num tokens (num_tokens=240 avail_mem=44.01 GB):  64%|██████▍   | 37/58 [00:07<00:01, 17.24it/s]Capturing num tokens (num_tokens=224 avail_mem=44.00 GB):  64%|██████▍   | 37/58 [00:07<00:01, 17.24it/s]Capturing num tokens (num_tokens=208 avail_mem=43.99 GB):  64%|██████▍   | 37/58 [00:07<00:01, 17.24it/s]Capturing num tokens (num_tokens=208 avail_mem=43.99 GB):  69%|██████▉   | 40/58 [00:07<00:00, 19.55it/s]Capturing num tokens (num_tokens=192 avail_mem=43.99 GB):  69%|██████▉   | 40/58 [00:07<00:00, 19.55it/s]Capturing num tokens (num_tokens=176 avail_mem=43.99 GB):  69%|██████▉   | 40/58 [00:07<00:00, 19.55it/s]Capturing num tokens (num_tokens=160 avail_mem=43.99 GB):  69%|██████▉   | 40/58 [00:07<00:00, 19.55it/s]

    Capturing num tokens (num_tokens=160 avail_mem=43.99 GB):  74%|███████▍  | 43/58 [00:07<00:00, 21.30it/s]Capturing num tokens (num_tokens=144 avail_mem=43.98 GB):  74%|███████▍  | 43/58 [00:07<00:00, 21.30it/s]Capturing num tokens (num_tokens=128 avail_mem=43.98 GB):  74%|███████▍  | 43/58 [00:07<00:00, 21.30it/s]Capturing num tokens (num_tokens=112 avail_mem=43.98 GB):  74%|███████▍  | 43/58 [00:07<00:00, 21.30it/s]Capturing num tokens (num_tokens=112 avail_mem=43.98 GB):  79%|███████▉  | 46/58 [00:07<00:00, 22.69it/s]Capturing num tokens (num_tokens=96 avail_mem=43.97 GB):  79%|███████▉  | 46/58 [00:07<00:00, 22.69it/s] Capturing num tokens (num_tokens=80 avail_mem=43.96 GB):  79%|███████▉  | 46/58 [00:07<00:00, 22.69it/s]

    Capturing num tokens (num_tokens=64 avail_mem=43.95 GB):  79%|███████▉  | 46/58 [00:07<00:00, 22.69it/s]Capturing num tokens (num_tokens=64 avail_mem=43.95 GB):  84%|████████▍ | 49/58 [00:08<00:00, 21.72it/s]Capturing num tokens (num_tokens=48 avail_mem=43.95 GB):  84%|████████▍ | 49/58 [00:08<00:00, 21.72it/s]Capturing num tokens (num_tokens=32 avail_mem=43.94 GB):  84%|████████▍ | 49/58 [00:08<00:00, 21.72it/s]Capturing num tokens (num_tokens=28 avail_mem=43.93 GB):  84%|████████▍ | 49/58 [00:08<00:00, 21.72it/s]Capturing num tokens (num_tokens=28 avail_mem=43.93 GB):  90%|████████▉ | 52/58 [00:08<00:00, 22.04it/s]Capturing num tokens (num_tokens=24 avail_mem=43.93 GB):  90%|████████▉ | 52/58 [00:08<00:00, 22.04it/s]

    Capturing num tokens (num_tokens=20 avail_mem=43.92 GB):  90%|████████▉ | 52/58 [00:08<00:00, 22.04it/s]Capturing num tokens (num_tokens=16 avail_mem=43.91 GB):  90%|████████▉ | 52/58 [00:08<00:00, 22.04it/s]Capturing num tokens (num_tokens=16 avail_mem=43.91 GB):  95%|█████████▍| 55/58 [00:08<00:00, 22.34it/s]Capturing num tokens (num_tokens=12 avail_mem=43.90 GB):  95%|█████████▍| 55/58 [00:08<00:00, 22.34it/s]Capturing num tokens (num_tokens=8 avail_mem=43.90 GB):  95%|█████████▍| 55/58 [00:08<00:00, 22.34it/s] Capturing num tokens (num_tokens=4 avail_mem=43.90 GB):  95%|█████████▍| 55/58 [00:08<00:00, 22.34it/s]Capturing num tokens (num_tokens=4 avail_mem=43.90 GB): 100%|██████████| 58/58 [00:08<00:00,  6.93it/s]


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


<strong style='color: #00008B;'>reasoing_content: Okay, so I need to figure out the capital of France and its population. I know that the capital of France is Paris, but I'm not exactly sure about the current population numbers. I remember that Paris is a very big city, but I think it's not the largest in the world. Maybe around 20 million? I'm not certain, though. I should probably check that.<br><br>Wait, I think the population has been growing over the years. I recall reading somewhere that it's over 21 million now. Maybe around 21.6 million? I'm not sure if that's the exact number or just an estimate. I should look it up to confirm. Also, I should make sure that Paris is indeed the capital and not another city like Lyon or Marseille.<br><br>I'm pretty confident that Paris is the capital, but just to be thorough, I'll double-check. The official capital of France is Paris, so that's settled. Now, for the population, I think it's a bit tricky because population numbers can change yearly. I might find a figure that's a bit higher or lower depending on the source. Maybe I should look for the most recent data available, perhaps from 2023, to get the accurate number.<br><br>I think the population is around 21.6 million, but I'm not 100% sure. I should also consider whether this includes only the city proper or the entire metropolitan area. Sometimes, population figures can include surrounding suburbs and satellite towns. If the data I find includes the metropolitan area, that might be a different number. I need to clarify that.<br><br>Another thing to consider is that the population can vary slightly depending on the source. Some sources might use estimates, while others might have more precise data. I should look for a reliable source, like the official statistics from the French National Institute of Statistics and Economic Studies (INSEE) or another reputable organization. That way, I can be more confident in the accuracy of the number.<br><br>In summary, I'm pretty sure the capital is Paris, and the population is around 21.6 million, but I should verify this information to ensure it's correct. Once I have the exact number, I can present it in the JSON format as requested.<br><br><br>content: {<br><br>"name": "Paris",<br>"population": 21620000<br>}</strong>


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


<strong style='color: #00008B;'>reasoing_content: Alright, I need to figure out how to respond to the user's query. They're in New York and want the current date and time along with the weather. Let me break this down step by step.<br><br>First, the user mentioned they're in New York, so I should consider the timezone. New York is in the Eastern Time Zone, which is 'America/New_York'. I remember that the user can use the 'get_current_date' function for this. I'll need to call this function with the timezone parameter set to 'America/New_York'. That should give me the current date and time in their local time.<br><br>Next, for the weather. The user is in New York, so I should get the weather for that specific location. Looking at the available functions, 'get_current_weather' is the right one. It requires a city and state. I know the city is New York, and the state abbreviation is NY. So I'll call 'get_current_weather' with city: 'New York' and state: 'NY'. This should fetch the current weather conditions there.<br><br>I should make sure to call each function separately, as per the instructions, so the user gets both pieces of information. I don't need to combine them into a single response unless the user specifically asks for it, which they don't here.<br><br>Also, I should format the function calls correctly. For 'get_current_date', the parameters object should have timezone: 'America/New_York'. And for 'get_current_weather', it should have city and state as specified. I'll make sure to structure each function call on a single line as required.<br><br>I think that's all. I'll prepare the responses for each function call, making sure to include the parameters in JSON format within each function's call.<br><br><br>content: <function=get_current_date>{"timezone": "America/New_York"}</function><br><function=get_current_weather>{"city": "New York", "state": "NY", "unit": "celsius"}</function></strong>


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

    {'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down. First, I need to identify what the capital of France is. I know that Paris is the capital, so that\'s straightforward. \n\nNext, I need to find the population. I remember that Paris is a major city, so its population is quite large. I think it\'s over 3 million, but I\'m not exactly sure of the exact number. Maybe I should double-check that. \n\nWait, I recall that the population figure can vary depending on the source and the year. The user didn\'t specify a particular year, so I should probably go with the most recent estimate. I believe the population is around 3,500,000 as of 2023. \n\nNow, I need to structure this information into a JSON format. JSON typically uses key-value pairs, so I\'ll create an object with keys like "city", "population", and maybe "country" since the user mentioned France. \n\nI should make sure the keys are in English to keep it clear. The city is Paris, the population is 3,500,000, and the country is France. I\'ll format this into a JSON object, ensuring proper syntax with commas and quotation marks. \n\nI also need to present this in a way that\'s easy to read, so I\'ll put each key on a new line. That way, the user can quickly see the information without confusion. \n\nI wonder if the user needs more details, like the exact current population or additional statistics. But since they only asked for the capital and population, I\'ll stick to that. \n\nLastly, I\'ll make sure the JSON is valid by checking the syntax. No trailing commas, proper use of braces, and correct quotation marks. That should cover everything the user needs.\n</think>{\n  "name": "Paris",\n  "population": 3500000\n}', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 13, 5512, 11, 358, 1184, 311, 10542, 1128, 279, 6722, 315, 9625, 374, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4710, 5847, 11, 358, 1184, 311, 1477, 279, 7042, 13, 358, 6099, 429, 12095, 374, 264, 3598, 3283, 11, 773, 1181, 7042, 374, 5008, 3460, 13, 358, 1744, 432, 594, 916, 220, 18, 3526, 11, 714, 358, 2776, 537, 6896, 2704, 315, 279, 4734, 1372, 13, 10696, 358, 1265, 1990, 15934, 429, 13, 4710, 14190, 11, 358, 19091, 429, 279, 7042, 7071, 646, 13289, 11649, 389, 279, 2530, 323, 279, 1042, 13, 576, 1196, 3207, 944, 13837, 264, 3953, 1042, 11, 773, 358, 1265, 4658, 728, 448, 279, 1429, 3213, 16045, 13, 358, 4411, 279, 7042, 374, 2163, 220, 18, 11, 20, 15, 15, 11, 15, 15, 15, 438, 315, 220, 17, 15, 17, 18, 13, 4710, 7039, 11, 358, 1184, 311, 5944, 419, 1995, 1119, 264, 4718, 3561, 13, 4718, 11136, 5711, 1376, 19083, 13530, 11, 773, 358, 3278, 1855, 458, 1633, 448, 6894, 1075, 330, 8926, 497, 330, 44441, 497, 323, 7196, 330, 11141, 1, 2474, 279, 1196, 9733, 9625, 13, 4710, 40, 1265, 1281, 2704, 279, 6894, 525, 304, 6364, 311, 2506, 432, 2797, 13, 576, 3283, 374, 12095, 11, 279, 7042, 374, 220, 18, 11, 20, 15, 15, 11, 15, 15, 15, 11, 323, 279, 3146, 374, 9625, 13, 358, 3278, 3561, 419, 1119, 264, 4718, 1633, 11, 22573, 6169, 19482, 448, 76602, 323, 54231, 15423, 13, 4710, 40, 1083, 1184, 311, 3042, 419, 304, 264, 1616, 429, 594, 4135, 311, 1349, 11, 773, 358, 3278, 2182, 1817, 1376, 389, 264, 501, 1555, 13, 2938, 1616, 11, 279, 1196, 646, 6157, 1490, 279, 1995, 2041, 21340, 13, 4710, 40, 5775, 421, 279, 1196, 3880, 803, 3565, 11, 1075, 279, 4734, 1482, 7042, 476, 5107, 13142, 13, 1988, 2474, 807, 1172, 4588, 369, 279, 6722, 323, 7042, 11, 358, 3278, 9214, 311, 429, 13, 4710, 80486, 11, 358, 3278, 1281, 2704, 279, 4718, 374, 2697, 553, 13295, 279, 19482, 13, 2308, 27748, 76602, 11, 6169, 990, 315, 59191, 11, 323, 4396, 54231, 15423, 13, 2938, 1265, 3421, 4297, 279, 1196, 3880, 624, 151649, 515, 220, 330, 606, 788, 330, 59604, 756, 220, 330, 44441, 788, 220, 18, 20, 15, 15, 15, 15, 15, 198, 92, 151643], 'meta_info': {'id': 'b625119da11c419598867b75d9073ae7', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 412, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 2.8518962480593473, 'response_sent_to_client_ts': 1774937926.7938519}}



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


<strong style='color: #00008B;'>{'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. I know that Paris is the capital, so that\'s straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I\'m not sure of the exact number. I think it\'s around 2 million, but I should verify that.\n\nI\'ll check a reliable source, maybe the official Paris Municipality website or a recent census. Let me see... Yes, according to the latest data, the population is approximately 2,174,300 as of 2023. That seems accurate.\n\nNext, I need to structure this information into a JSON format. JSON requires key-value pairs, so I\'ll create an object with keys like "city", "population", and "country". The city is Paris, the population is the number I found, and the country is France.\n\nI should make sure the JSON syntax is correct. Each key should be in quotes, and the values as well. The entire structure should be enclosed in curly braces. I\'ll format it properly to avoid any syntax errors.\n\nPutting it all together, the JSON object will have the city, population, and country. I\'ll present this to the user, making sure it\'s clear and easy to understand. I don\'t think the user needs anything more detailed, so this should suffice.\n\nI should also consider if the user might need additional information, like the area or age distribution, but since they only asked for population, I\'ll stick to that. Maybe they\'ll ask for more details later, but for now, this response should be helpful.\n</think>{\n  "name": "Paris",\n  "population": 2174300\n}', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1265, 1477, 279, 1429, 3213, 7042, 821, 13, 358, 6099, 429, 279, 7042, 315, 12095, 702, 1012, 7826, 11, 714, 358, 2776, 537, 2704, 315, 279, 4734, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 10146, 429, 382, 40, 3278, 1779, 264, 14720, 2530, 11, 7196, 279, 3946, 12095, 35703, 2719, 3910, 476, 264, 3213, 43602, 13, 6771, 752, 1490, 1112, 7414, 11, 4092, 311, 279, 5535, 821, 11, 279, 7042, 374, 13187, 220, 17, 11, 16, 22, 19, 11, 18, 15, 15, 438, 315, 220, 17, 15, 17, 18, 13, 2938, 4977, 13382, 382, 5847, 11, 358, 1184, 311, 5944, 419, 1995, 1119, 264, 4718, 3561, 13, 4718, 7460, 1376, 19083, 13530, 11, 773, 358, 3278, 1855, 458, 1633, 448, 6894, 1075, 330, 8926, 497, 330, 44441, 497, 323, 330, 11141, 3263, 576, 3283, 374, 12095, 11, 279, 7042, 374, 279, 1372, 358, 1730, 11, 323, 279, 3146, 374, 9625, 382, 40, 1265, 1281, 2704, 279, 4718, 19482, 374, 4396, 13, 8886, 1376, 1265, 387, 304, 17194, 11, 323, 279, 2750, 438, 1632, 13, 576, 4453, 5944, 1265, 387, 43810, 304, 68103, 59191, 13, 358, 3278, 3561, 432, 10277, 311, 5648, 894, 19482, 5975, 382, 97904, 432, 678, 3786, 11, 279, 4718, 1633, 686, 614, 279, 3283, 11, 7042, 11, 323, 3146, 13, 358, 3278, 3042, 419, 311, 279, 1196, 11, 3259, 2704, 432, 594, 2797, 323, 4135, 311, 3535, 13, 358, 1513, 944, 1744, 279, 1196, 3880, 4113, 803, 11682, 11, 773, 419, 1265, 76156, 382, 40, 1265, 1083, 2908, 421, 279, 1196, 2578, 1184, 5107, 1995, 11, 1075, 279, 3082, 476, 4231, 7982, 11, 714, 2474, 807, 1172, 4588, 369, 7042, 11, 358, 3278, 9214, 311, 429, 13, 10696, 807, 3278, 2548, 369, 803, 3565, 2937, 11, 714, 369, 1431, 11, 419, 2033, 1265, 387, 10950, 624, 151649, 515, 220, 330, 606, 788, 330, 59604, 756, 220, 330, 44441, 788, 220, 17, 16, 22, 19, 18, 15, 15, 198, 92, 151643], 'meta_info': {'id': '82ba8ae513154a6ab147b884f86ca556', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 384, 'cached_tokens': 22, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 2.6647875129710883, 'response_sent_to_client_ts': 1774937929.4728103}}</strong>


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

    [{'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '827be985b05e47df862747c586b077df', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.08362369704991579, 'response_sent_to_client_ts': 1774937929.581648}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': 'e130d7d599724f1597a7c18c077b24fa', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.0835656119743362, 'response_sent_to_client_ts': 1774937929.5816624}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': 'e52e758959214eab95217b5f38f6896c', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.08352929807733744, 'response_sent_to_client_ts': 1774937929.5816674}}]


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

    {'text': ' France, and the \n\\( n \\)  \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\(', 'output_ids': [9625, 11, 323, 279, 220, 198, 44292, 308, 1124, 8, 220, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767], 'meta_info': {'id': '9014f8d259ff4350a87a61e6990404d1', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 6, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 31.24633284599986, 'response_sent_to_client_ts': 1774937960.8350456}}


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


<strong style='color: #00008B;'>{'text': 'Okay, so I need to find the capital of France and provide its information and population in JSON format. Let me start by recalling what I know about France. I know that Paris is the capital. But I don\'t remember the exact population number. I think it\'s a big city, maybe around 2 million? Wait, no, I think it\'s more than that. I\'m not entirely sure about the figure, so I might have to check that.\n\nSpeaking of Paris, I remember that it\'s known for landmarks like the Eiffel Tower and the Louvre Museum. Those are famous attractions, but maybe not exactly relevant to the population. The population probably includes the city proper, but sometimes population figures can refer to the metropolitan area. I should clarify whether it\'s just Paris or the broader metropolitan area. I think in this context, it\'s referring to the city itself, so just Paris.\n\nI wonder if it\'s better to represent the population as approximate or exact. Since I might not know the exact current population, an approximate number is okay. Let me try to recall some key points about Paris. I think it\'s been growing steadily over the years. Maybe around 2.2 million? That sounds familiar from previous knowledge snippets I\'ve come across.\n\nPutting it together, the JSON should have a "name" field with "Paris", a "country" field with "France", and a "population" field with the approximate number, maybe 2,200,000. I should make sure the keys are in camelCase and the value is a number, encapsulated in quotes.\n\nWait, I should double-check the population figure. If I recall correctly, recent estimates put Paris\'s population around 2.2 million. But maybe it\'s slightly different now. I\'m not 100% sure, but I think that\'s a solid approximation based on what I remember.\n\nSo, summarizing, the JSON structure would be an object with the three key fields: name, country, and population. The population is a numerical value, so in JSON, it\'ll be a number with quotes. The name and country are strings.\n\nLet me make sure I format the JSON correctly. It should be like:\n\n{\n  "name": "Paris",\n  "country": "France",\n  "population": "2200000"\n}\n\nI think that\'s correct. I should avoid any formatting errors, like missing commas or quotes. Each key-value pair should be separated by a comma, and the entire structure should be properly enclosed in curly braces.\n\nYeah, that makes sense. I\'ve got the capital, country, and an approximate population based on what I remember. If I had access to current data, I could get a more precise number, but for now, 2.2 million seems reasonable.\n</think>\n\nHere is the JSON information about the capital of France, including its name, country, and population:\n\n```json\n{\n  "name": "Paris",\n  "country": "France",\n  "population": "2200000"\n}\n```\n\nThis JSON structure provides the requested details with the population given as an approximate figure.', 'output_ids': [32313, 11, 773, 358, 1184, 311, 1477, 279, 6722, 315, 9625, 323, 3410, 1181, 1995, 323, 7042, 304, 4718, 3561, 13, 6771, 752, 1191, 553, 88646, 1128, 358, 1414, 911, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 13, 1988, 358, 1513, 944, 6099, 279, 4734, 7042, 1372, 13, 358, 1744, 432, 594, 264, 2409, 3283, 11, 7196, 2163, 220, 17, 3526, 30, 13824, 11, 902, 11, 358, 1744, 432, 594, 803, 1091, 429, 13, 358, 2776, 537, 11368, 2704, 911, 279, 7071, 11, 773, 358, 2578, 614, 311, 1779, 429, 382, 32017, 315, 12095, 11, 358, 6099, 429, 432, 594, 3881, 369, 59924, 1075, 279, 468, 3092, 301, 21938, 323, 279, 9729, 48506, 16328, 13, 12969, 525, 11245, 38491, 11, 714, 7196, 537, 6896, 9760, 311, 279, 7042, 13, 576, 7042, 4658, 5646, 279, 3283, 6169, 11, 714, 7025, 7042, 12396, 646, 8300, 311, 279, 57406, 3082, 13, 358, 1265, 37163, 3425, 432, 594, 1101, 12095, 476, 279, 26829, 57406, 3082, 13, 358, 1744, 304, 419, 2266, 11, 432, 594, 22023, 311, 279, 3283, 5086, 11, 773, 1101, 12095, 382, 40, 5775, 421, 432, 594, 2664, 311, 4009, 279, 7042, 438, 44868, 476, 4734, 13, 8704, 358, 2578, 537, 1414, 279, 4734, 1482, 7042, 11, 458, 44868, 1372, 374, 16910, 13, 6771, 752, 1430, 311, 19091, 1045, 1376, 3501, 911, 12095, 13, 358, 1744, 432, 594, 1012, 7826, 41971, 916, 279, 1635, 13, 10696, 2163, 220, 17, 13, 17, 3526, 30, 2938, 10362, 11285, 504, 3681, 6540, 68642, 358, 3003, 2525, 3941, 382, 97904, 432, 3786, 11, 279, 4718, 1265, 614, 264, 330, 606, 1, 2070, 448, 330, 59604, 497, 264, 330, 11141, 1, 2070, 448, 330, 49000, 497, 323, 264, 330, 44441, 1, 2070, 448, 279, 44868, 1372, 11, 7196, 220, 17, 11, 17, 15, 15, 11, 15, 15, 15, 13, 358, 1265, 1281, 2704, 279, 6894, 525, 304, 49152, 4207, 323, 279, 897, 374, 264, 1372, 11, 42569, 7757, 304, 17194, 382, 14190, 11, 358, 1265, 1990, 15934, 279, 7042, 7071, 13, 1416, 358, 19091, 12440, 11, 3213, 17530, 2182, 12095, 594, 7042, 2163, 220, 17, 13, 17, 3526, 13, 1988, 7196, 432, 594, 10078, 2155, 1431, 13, 358, 2776, 537, 220, 16, 15, 15, 4, 2704, 11, 714, 358, 1744, 429, 594, 264, 6437, 56204, 3118, 389, 1128, 358, 6099, 382, 4416, 11, 28285, 4849, 11, 279, 4718, 5944, 1035, 387, 458, 1633, 448, 279, 2326, 1376, 5043, 25, 829, 11, 3146, 11, 323, 7042, 13, 576, 7042, 374, 264, 34776, 897, 11, 773, 304, 4718, 11, 432, 3278, 387, 264, 1372, 448, 17194, 13, 576, 829, 323, 3146, 525, 9069, 382, 10061, 752, 1281, 2704, 358, 3561, 279, 4718, 12440, 13, 1084, 1265, 387, 1075, 1447, 515, 220, 330, 606, 788, 330, 59604, 756, 220, 330, 11141, 788, 330, 49000, 756, 220, 330, 44441, 788, 330, 17, 17, 15, 15, 15, 15, 15, 698, 630, 40, 1744, 429, 594, 4396, 13, 358, 1265, 5648, 894, 36566, 5975, 11, 1075, 7402, 76602, 476, 17194, 13, 8886, 1376, 19083, 6716, 1265, 387, 18663, 553, 264, 31683, 11, 323, 279, 4453, 5944, 1265, 387, 10277, 43810, 304, 68103, 59191, 382, 23336, 11, 429, 3643, 5530, 13, 358, 3003, 2684, 279, 6722, 11, 3146, 11, 323, 458, 44868, 7042, 3118, 389, 1128, 358, 6099, 13, 1416, 358, 1030, 2615, 311, 1482, 821, 11, 358, 1410, 633, 264, 803, 23560, 1372, 11, 714, 369, 1431, 11, 220, 17, 13, 17, 3526, 4977, 13276, 624, 151649, 271, 8420, 374, 279, 4718, 1995, 911, 279, 6722, 315, 9625, 11, 2670, 1181, 829, 11, 3146, 11, 323, 7042, 1447, 73594, 2236, 198, 515, 220, 330, 606, 788, 330, 59604, 756, 220, 330, 11141, 788, 330, 49000, 756, 220, 330, 44441, 788, 330, 17, 17, 15, 15, 15, 15, 15, 698, 532, 13874, 19324, 1986, 4718, 5944, 5707, 279, 11223, 3565, 448, 279, 7042, 2661, 438, 458, 44868, 7071, 13, 151643], 'meta_info': {'id': '4e02b8d10a124d84b29ebdab1cc24a69', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 648, 'cached_tokens': 22, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 5.444410845055245, 'response_sent_to_client_ts': 1774937966.29135}}</strong>



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


    2026-03-31 06:19:38.781 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 06:19:38] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 06:19:38.781 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 06:19:38] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 06:19:38.781 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 06:19:38] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 06:19:38.781 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 06:19:38] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 06:19:38.781 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 06:19:38] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:01<00:01,  1.17s/it]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.54s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.49s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:04,  3.24s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:04,  3.24s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:04<01:45,  1.88s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:04<01:45,  1.88s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:06,  1.22s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:06,  1.22s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:48,  1.12it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:48,  1.12it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:36,  1.44it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:36,  1.44it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:30,  1.73it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:30,  1.73it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:24,  2.05it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:24,  2.05it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:06<00:20,  2.39it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:06<00:20,  2.39it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:06<00:17,  2.75it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:06<00:17,  2.75it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:06<00:15,  3.10it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:06<00:15,  3.10it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:07<00:14,  3.17it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:07<00:14,  3.17it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:07<00:13,  3.30it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:07<00:13,  3.30it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:07<00:12,  3.48it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:07<00:12,  3.48it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:07<00:11,  3.71it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:07<00:11,  3.71it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:07<00:10,  4.00it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:07<00:10,  4.00it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:08<00:09,  4.35it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:08<00:09,  4.35it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:08<00:07,  5.14it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:08<00:07,  5.14it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:08<00:07,  5.14it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:08<00:05,  6.96it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:08<00:05,  6.96it/s]

    Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:08<00:05,  6.96it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:08<00:04,  8.91it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:08<00:04,  8.91it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:08<00:04,  8.91it/s]Compiling num tokens (num_tokens=832):  36%|███▌      | 21/58 [00:08<00:04,  8.91it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:08<00:02, 12.49it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:08<00:02, 12.49it/s]

    Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:08<00:02, 12.49it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:08<00:02, 12.49it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:08<00:02, 15.48it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:08<00:02, 15.48it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:08<00:02, 15.48it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:08<00:02, 15.48it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:08<00:01, 18.54it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:08<00:01, 18.54it/s]

    Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:08<00:01, 18.54it/s]Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:08<00:01, 18.54it/s]Compiling num tokens (num_tokens=352):  52%|█████▏    | 30/58 [00:09<00:01, 18.54it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:09<00:01, 23.12it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:09<00:01, 23.12it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:09<00:01, 23.12it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:09<00:01, 23.12it/s]Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:09<00:01, 23.12it/s]

    Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:09<00:00, 26.38it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:09<00:00, 26.38it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:09<00:00, 26.38it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:09<00:00, 26.38it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:09<00:00, 26.38it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:09<00:00, 28.95it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:09<00:00, 28.95it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:09<00:00, 28.95it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:09<00:00, 28.95it/s]Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:09<00:00, 28.95it/s]

    Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:09<00:00, 31.08it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:09<00:00, 31.08it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:09<00:00, 31.08it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:09<00:00, 31.08it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:09<00:00, 31.08it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:09<00:00, 33.35it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:09<00:00, 33.35it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:09<00:00, 33.35it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:09<00:00, 33.35it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:09<00:00, 33.35it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:09<00:00, 33.35it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:09<00:00, 33.35it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:09<00:00, 33.35it/s] 

    Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:09<00:00, 42.99it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:09<00:00, 42.99it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:09<00:00,  6.04it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=34.46 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=34.46 GB):   2%|▏         | 1/58 [00:00<00:36,  1.55it/s]Capturing num tokens (num_tokens=7680 avail_mem=34.40 GB):   2%|▏         | 1/58 [00:00<00:36,  1.55it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=34.40 GB):   3%|▎         | 2/58 [00:01<00:41,  1.33it/s]Capturing num tokens (num_tokens=7168 avail_mem=34.40 GB):   3%|▎         | 2/58 [00:01<00:41,  1.33it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=34.40 GB):   5%|▌         | 3/58 [00:02<00:41,  1.31it/s]Capturing num tokens (num_tokens=6656 avail_mem=34.41 GB):   5%|▌         | 3/58 [00:02<00:41,  1.31it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=34.41 GB):   7%|▋         | 4/58 [00:02<00:35,  1.51it/s]Capturing num tokens (num_tokens=6144 avail_mem=34.42 GB):   7%|▋         | 4/58 [00:02<00:35,  1.51it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=34.42 GB):   9%|▊         | 5/58 [00:03<00:30,  1.73it/s]Capturing num tokens (num_tokens=5632 avail_mem=34.42 GB):   9%|▊         | 5/58 [00:03<00:30,  1.73it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=34.42 GB):  10%|█         | 6/58 [00:03<00:26,  1.98it/s]Capturing num tokens (num_tokens=5120 avail_mem=34.42 GB):  10%|█         | 6/58 [00:03<00:26,  1.98it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=34.42 GB):  12%|█▏        | 7/58 [00:03<00:23,  2.20it/s]Capturing num tokens (num_tokens=4608 avail_mem=34.43 GB):  12%|█▏        | 7/58 [00:03<00:23,  2.20it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=34.43 GB):  14%|█▍        | 8/58 [00:04<00:20,  2.49it/s]Capturing num tokens (num_tokens=4096 avail_mem=34.44 GB):  14%|█▍        | 8/58 [00:04<00:20,  2.49it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=34.44 GB):  16%|█▌        | 9/58 [00:04<00:17,  2.78it/s]Capturing num tokens (num_tokens=3840 avail_mem=34.45 GB):  16%|█▌        | 9/58 [00:04<00:17,  2.78it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=34.45 GB):  17%|█▋        | 10/58 [00:04<00:17,  2.81it/s]Capturing num tokens (num_tokens=3584 avail_mem=34.45 GB):  17%|█▋        | 10/58 [00:04<00:17,  2.81it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=34.45 GB):  19%|█▉        | 11/58 [00:05<00:14,  3.14it/s]Capturing num tokens (num_tokens=3328 avail_mem=34.45 GB):  19%|█▉        | 11/58 [00:05<00:14,  3.14it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=34.45 GB):  21%|██        | 12/58 [00:05<00:13,  3.48it/s]Capturing num tokens (num_tokens=3072 avail_mem=34.19 GB):  21%|██        | 12/58 [00:05<00:13,  3.48it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=34.19 GB):  22%|██▏       | 13/58 [00:05<00:11,  3.80it/s]Capturing num tokens (num_tokens=2816 avail_mem=33.90 GB):  22%|██▏       | 13/58 [00:05<00:11,  3.80it/s]Capturing num tokens (num_tokens=2816 avail_mem=33.90 GB):  24%|██▍       | 14/58 [00:05<00:10,  4.19it/s]Capturing num tokens (num_tokens=2560 avail_mem=33.74 GB):  24%|██▍       | 14/58 [00:05<00:10,  4.19it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=33.74 GB):  26%|██▌       | 15/58 [00:05<00:09,  4.64it/s]Capturing num tokens (num_tokens=2304 avail_mem=33.45 GB):  26%|██▌       | 15/58 [00:05<00:09,  4.64it/s]Capturing num tokens (num_tokens=2304 avail_mem=33.45 GB):  28%|██▊       | 16/58 [00:05<00:08,  5.13it/s]Capturing num tokens (num_tokens=2048 avail_mem=33.13 GB):  28%|██▊       | 16/58 [00:05<00:08,  5.13it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=33.13 GB):  29%|██▉       | 17/58 [00:06<00:07,  5.77it/s]Capturing num tokens (num_tokens=1792 avail_mem=32.90 GB):  29%|██▉       | 17/58 [00:06<00:07,  5.77it/s]Capturing num tokens (num_tokens=1792 avail_mem=32.90 GB):  31%|███       | 18/58 [00:06<00:06,  6.32it/s]Capturing num tokens (num_tokens=1536 avail_mem=32.92 GB):  31%|███       | 18/58 [00:06<00:06,  6.32it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=32.92 GB):  33%|███▎      | 19/58 [00:06<00:05,  6.88it/s]Capturing num tokens (num_tokens=1280 avail_mem=32.84 GB):  33%|███▎      | 19/58 [00:06<00:05,  6.88it/s]Capturing num tokens (num_tokens=1024 avail_mem=32.37 GB):  33%|███▎      | 19/58 [00:06<00:05,  6.88it/s]Capturing num tokens (num_tokens=1024 avail_mem=32.37 GB):  36%|███▌      | 21/58 [00:06<00:04,  8.36it/s]Capturing num tokens (num_tokens=960 avail_mem=30.46 GB):  36%|███▌      | 21/58 [00:06<00:04,  8.36it/s] 

    Capturing num tokens (num_tokens=896 avail_mem=30.46 GB):  36%|███▌      | 21/58 [00:06<00:04,  8.36it/s]Capturing num tokens (num_tokens=896 avail_mem=30.46 GB):  40%|███▉      | 23/58 [00:06<00:03,  9.57it/s]Capturing num tokens (num_tokens=832 avail_mem=30.57 GB):  40%|███▉      | 23/58 [00:06<00:03,  9.57it/s]Capturing num tokens (num_tokens=768 avail_mem=30.56 GB):  40%|███▉      | 23/58 [00:06<00:03,  9.57it/s]

    Capturing num tokens (num_tokens=768 avail_mem=30.56 GB):  43%|████▎     | 25/58 [00:06<00:03,  9.79it/s]Capturing num tokens (num_tokens=704 avail_mem=30.55 GB):  43%|████▎     | 25/58 [00:06<00:03,  9.79it/s]Capturing num tokens (num_tokens=640 avail_mem=30.55 GB):  43%|████▎     | 25/58 [00:06<00:03,  9.79it/s]Capturing num tokens (num_tokens=640 avail_mem=30.55 GB):  47%|████▋     | 27/58 [00:07<00:03, 10.26it/s]Capturing num tokens (num_tokens=576 avail_mem=30.54 GB):  47%|████▋     | 27/58 [00:07<00:03, 10.26it/s]

    Capturing num tokens (num_tokens=512 avail_mem=30.48 GB):  47%|████▋     | 27/58 [00:07<00:03, 10.26it/s]Capturing num tokens (num_tokens=512 avail_mem=30.48 GB):  50%|█████     | 29/58 [00:07<00:03,  8.32it/s]Capturing num tokens (num_tokens=480 avail_mem=30.51 GB):  50%|█████     | 29/58 [00:07<00:03,  8.32it/s]

    Capturing num tokens (num_tokens=480 avail_mem=30.51 GB):  52%|█████▏    | 30/58 [00:07<00:03,  8.15it/s]Capturing num tokens (num_tokens=448 avail_mem=30.49 GB):  52%|█████▏    | 30/58 [00:07<00:03,  8.15it/s]Capturing num tokens (num_tokens=448 avail_mem=30.49 GB):  53%|█████▎    | 31/58 [00:07<00:03,  7.59it/s]Capturing num tokens (num_tokens=416 avail_mem=30.51 GB):  53%|█████▎    | 31/58 [00:07<00:03,  7.59it/s]

    Capturing num tokens (num_tokens=384 avail_mem=30.50 GB):  53%|█████▎    | 31/58 [00:07<00:03,  7.59it/s]Capturing num tokens (num_tokens=384 avail_mem=30.50 GB):  57%|█████▋    | 33/58 [00:07<00:02,  8.52it/s]Capturing num tokens (num_tokens=352 avail_mem=30.47 GB):  57%|█████▋    | 33/58 [00:07<00:02,  8.52it/s]Capturing num tokens (num_tokens=320 avail_mem=30.46 GB):  57%|█████▋    | 33/58 [00:07<00:02,  8.52it/s]

    Capturing num tokens (num_tokens=320 avail_mem=30.46 GB):  60%|██████    | 35/58 [00:07<00:02, 10.45it/s]Capturing num tokens (num_tokens=288 avail_mem=30.46 GB):  60%|██████    | 35/58 [00:07<00:02, 10.45it/s]Capturing num tokens (num_tokens=256 avail_mem=30.49 GB):  60%|██████    | 35/58 [00:08<00:02, 10.45it/s]Capturing num tokens (num_tokens=256 avail_mem=30.49 GB):  64%|██████▍   | 37/58 [00:08<00:01, 12.19it/s]Capturing num tokens (num_tokens=240 avail_mem=30.44 GB):  64%|██████▍   | 37/58 [00:08<00:01, 12.19it/s]Capturing num tokens (num_tokens=224 avail_mem=30.43 GB):  64%|██████▍   | 37/58 [00:08<00:01, 12.19it/s]

    Capturing num tokens (num_tokens=224 avail_mem=30.43 GB):  67%|██████▋   | 39/58 [00:08<00:01, 12.56it/s]Capturing num tokens (num_tokens=208 avail_mem=30.44 GB):  67%|██████▋   | 39/58 [00:08<00:01, 12.56it/s]Capturing num tokens (num_tokens=192 avail_mem=30.43 GB):  67%|██████▋   | 39/58 [00:08<00:01, 12.56it/s]Capturing num tokens (num_tokens=176 avail_mem=30.45 GB):  67%|██████▋   | 39/58 [00:08<00:01, 12.56it/s]Capturing num tokens (num_tokens=176 avail_mem=30.45 GB):  72%|███████▏  | 42/58 [00:08<00:01, 15.43it/s]Capturing num tokens (num_tokens=160 avail_mem=30.42 GB):  72%|███████▏  | 42/58 [00:08<00:01, 15.43it/s]Capturing num tokens (num_tokens=144 avail_mem=30.43 GB):  72%|███████▏  | 42/58 [00:08<00:01, 15.43it/s]

    Capturing num tokens (num_tokens=144 avail_mem=30.43 GB):  76%|███████▌  | 44/58 [00:08<00:00, 16.15it/s]Capturing num tokens (num_tokens=128 avail_mem=30.41 GB):  76%|███████▌  | 44/58 [00:08<00:00, 16.15it/s]Capturing num tokens (num_tokens=112 avail_mem=30.42 GB):  76%|███████▌  | 44/58 [00:08<00:00, 16.15it/s]Capturing num tokens (num_tokens=96 avail_mem=30.40 GB):  76%|███████▌  | 44/58 [00:08<00:00, 16.15it/s] Capturing num tokens (num_tokens=96 avail_mem=30.40 GB):  81%|████████  | 47/58 [00:08<00:00, 17.72it/s]Capturing num tokens (num_tokens=80 avail_mem=30.41 GB):  81%|████████  | 47/58 [00:08<00:00, 17.72it/s]Capturing num tokens (num_tokens=64 avail_mem=30.40 GB):  81%|████████  | 47/58 [00:08<00:00, 17.72it/s]

    Capturing num tokens (num_tokens=48 avail_mem=30.40 GB):  81%|████████  | 47/58 [00:08<00:00, 17.72it/s]Capturing num tokens (num_tokens=48 avail_mem=30.40 GB):  86%|████████▌ | 50/58 [00:08<00:00, 20.44it/s]Capturing num tokens (num_tokens=32 avail_mem=30.39 GB):  86%|████████▌ | 50/58 [00:08<00:00, 20.44it/s]Capturing num tokens (num_tokens=28 avail_mem=30.39 GB):  86%|████████▌ | 50/58 [00:08<00:00, 20.44it/s]Capturing num tokens (num_tokens=24 avail_mem=30.38 GB):  86%|████████▌ | 50/58 [00:08<00:00, 20.44it/s]Capturing num tokens (num_tokens=20 avail_mem=30.37 GB):  86%|████████▌ | 50/58 [00:08<00:00, 20.44it/s]Capturing num tokens (num_tokens=20 avail_mem=30.37 GB):  93%|█████████▎| 54/58 [00:08<00:00, 23.67it/s]Capturing num tokens (num_tokens=16 avail_mem=30.36 GB):  93%|█████████▎| 54/58 [00:08<00:00, 23.67it/s]Capturing num tokens (num_tokens=12 avail_mem=30.36 GB):  93%|█████████▎| 54/58 [00:08<00:00, 23.67it/s]

    Capturing num tokens (num_tokens=8 avail_mem=30.35 GB):  93%|█████████▎| 54/58 [00:08<00:00, 23.67it/s] Capturing num tokens (num_tokens=4 avail_mem=30.34 GB):  93%|█████████▎| 54/58 [00:08<00:00, 23.67it/s]Capturing num tokens (num_tokens=4 avail_mem=30.34 GB): 100%|██████████| 58/58 [00:08<00:00, 26.86it/s]Capturing num tokens (num_tokens=4 avail_mem=30.34 GB): 100%|██████████| 58/58 [00:08<00:00,  6.48it/s]


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
    
    Generated text: Alright, the user is asking for the information and population of France's capital in JSON format. I know that the capital is Paris. I should confirm that first to make sure I'm providing accurate information.
    
    Next, I need to recall or look up the population of Paris. I believe the latest estimate is around 2.1 million people. It's important to note that populations can change over time, so I should mention that the number might vary slightly depending on the source and the year.
    
    I should structure the JSON with a proper format. Starting with the key "capital" pointing to "Paris" makes sense. Then, the "population" key can have the number and a note explaining the variation. This way, the information is clear and concise.
    
    Also, I should consider if the user needs more details or sources. But since they specifically asked for JSON, I'll stick to that format without adding extra information unless they ask for it.
    
    Finally, I'll present the JSON in a clean, readable way to ensure it's easy for the user to understand and use if needed.
    </think>
    
    ```json
    {
      "capital": "Paris",
      "population": {
        "number": 2146000,
        "note": "Population may vary slightly depending on the source and year."
      }
    }
    ```



```python
llm.shutdown()
```
