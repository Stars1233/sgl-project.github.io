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


    2026-03-30 08:47:05.142 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:47:05] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 08:47:05.142 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:47:05] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 08:47:05.142 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:47:05] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 08:47:05.142 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:47:05] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 08:47:05.142 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:47:05] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:00<00:00,  1.04it/s]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.19s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.15s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:48,  2.96s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:48,  2.96s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:21,  1.46s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:21,  1.46s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:49,  1.11it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:49,  1.11it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:38,  1.41it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:38,  1.41it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:30,  1.72it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:30,  1.72it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:25,  2.00it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:25,  2.00it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:21,  2.32it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:21,  2.32it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:05<00:19,  2.54it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:05<00:19,  2.54it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:05<00:17,  2.80it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:05<00:17,  2.80it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:05<00:14,  3.21it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:05<00:14,  3.21it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:06<00:13,  3.55it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:06<00:13,  3.55it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:06<00:11,  4.00it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:06<00:11,  4.00it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:06<00:10,  4.45it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:06<00:10,  4.45it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:06<00:09,  4.80it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:06<00:09,  4.80it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:06<00:08,  5.34it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:06<00:08,  5.34it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:06<00:07,  5.82it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:06<00:07,  5.82it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:06<00:06,  6.40it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:06<00:06,  6.40it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:07<00:06,  6.40it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:07<00:05,  7.68it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:07<00:05,  7.68it/s]

    Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:07<00:05,  7.68it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:07<00:04,  8.92it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:07<00:04,  8.92it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:07<00:04,  8.92it/s]

    Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:07<00:03, 10.76it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:07<00:03, 10.76it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:07<00:03, 10.76it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:07<00:02, 12.25it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:07<00:02, 12.25it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:07<00:02, 12.25it/s]

    Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:07<00:02, 12.25it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:07<00:02, 14.60it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:07<00:02, 14.60it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:07<00:02, 14.60it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:07<00:02, 14.60it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:07<00:01, 17.14it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:07<00:01, 17.14it/s]

    Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:07<00:01, 17.14it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:07<00:01, 17.14it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:07<00:01, 18.54it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:07<00:01, 18.54it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:07<00:01, 18.54it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:08<00:01, 18.54it/s]

    Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:08<00:01, 20.02it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:08<00:01, 20.02it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:08<00:01, 20.02it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:08<00:01, 20.02it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:08<00:00, 22.33it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:08<00:00, 22.33it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:08<00:00, 22.33it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:08<00:00, 22.33it/s]

    Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:08<00:00, 22.93it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:08<00:00, 22.93it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:08<00:00, 22.93it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:08<00:00, 22.93it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:08<00:00, 23.41it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:08<00:00, 23.41it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:08<00:00, 23.41it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:08<00:00, 23.41it/s]

    Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:08<00:00, 23.41it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:08<00:00, 26.13it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:08<00:00, 26.13it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:08<00:00, 26.13it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:08<00:00, 26.13it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:08<00:00, 26.13it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:08<00:00, 26.13it/s]Compiling num tokens (num_tokens=16):  95%|█████████▍| 55/58 [00:08<00:00, 32.18it/s]Compiling num tokens (num_tokens=12):  95%|█████████▍| 55/58 [00:08<00:00, 32.18it/s]Compiling num tokens (num_tokens=8):  95%|█████████▍| 55/58 [00:08<00:00, 32.18it/s] Compiling num tokens (num_tokens=4):  95%|█████████▍| 55/58 [00:08<00:00, 32.18it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:08<00:00,  6.67it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=25.00 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=25.00 GB):   2%|▏         | 1/58 [00:00<00:42,  1.34it/s]Capturing num tokens (num_tokens=7680 avail_mem=23.78 GB):   2%|▏         | 1/58 [00:00<00:42,  1.34it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=23.78 GB):   3%|▎         | 2/58 [00:01<00:43,  1.30it/s]Capturing num tokens (num_tokens=7168 avail_mem=44.01 GB):   3%|▎         | 2/58 [00:01<00:43,  1.30it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=44.01 GB):   5%|▌         | 3/58 [00:02<00:36,  1.50it/s]Capturing num tokens (num_tokens=6656 avail_mem=43.09 GB):   5%|▌         | 3/58 [00:02<00:36,  1.50it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=43.09 GB):   7%|▋         | 4/58 [00:02<00:31,  1.72it/s]Capturing num tokens (num_tokens=6144 avail_mem=43.16 GB):   7%|▋         | 4/58 [00:02<00:31,  1.72it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=43.16 GB):   9%|▊         | 5/58 [00:02<00:27,  1.89it/s]Capturing num tokens (num_tokens=5632 avail_mem=43.24 GB):   9%|▊         | 5/58 [00:02<00:27,  1.89it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=43.24 GB):  10%|█         | 6/58 [00:03<00:24,  2.15it/s]Capturing num tokens (num_tokens=5120 avail_mem=43.31 GB):  10%|█         | 6/58 [00:03<00:24,  2.15it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=43.31 GB):  12%|█▏        | 7/58 [00:03<00:21,  2.35it/s]Capturing num tokens (num_tokens=4608 avail_mem=43.38 GB):  12%|█▏        | 7/58 [00:03<00:21,  2.35it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=43.38 GB):  14%|█▍        | 8/58 [00:03<00:19,  2.60it/s]Capturing num tokens (num_tokens=4096 avail_mem=43.45 GB):  14%|█▍        | 8/58 [00:03<00:19,  2.60it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=43.45 GB):  16%|█▌        | 9/58 [00:04<00:17,  2.86it/s]Capturing num tokens (num_tokens=3840 avail_mem=43.53 GB):  16%|█▌        | 9/58 [00:04<00:17,  2.86it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=43.53 GB):  17%|█▋        | 10/58 [00:04<00:15,  3.14it/s]Capturing num tokens (num_tokens=3584 avail_mem=43.56 GB):  17%|█▋        | 10/58 [00:04<00:15,  3.14it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=43.56 GB):  19%|█▉        | 11/58 [00:04<00:15,  3.03it/s]Capturing num tokens (num_tokens=3328 avail_mem=43.59 GB):  19%|█▉        | 11/58 [00:04<00:15,  3.03it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=43.59 GB):  21%|██        | 12/58 [00:05<00:13,  3.38it/s]Capturing num tokens (num_tokens=3072 avail_mem=44.10 GB):  21%|██        | 12/58 [00:05<00:13,  3.38it/s]Capturing num tokens (num_tokens=3072 avail_mem=44.10 GB):  22%|██▏       | 13/58 [00:05<00:11,  3.79it/s]Capturing num tokens (num_tokens=2816 avail_mem=43.66 GB):  22%|██▏       | 13/58 [00:05<00:11,  3.79it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=43.66 GB):  24%|██▍       | 14/58 [00:05<00:10,  4.20it/s]Capturing num tokens (num_tokens=2560 avail_mem=43.69 GB):  24%|██▍       | 14/58 [00:05<00:10,  4.20it/s]Capturing num tokens (num_tokens=2560 avail_mem=43.69 GB):  26%|██▌       | 15/58 [00:05<00:09,  4.72it/s]Capturing num tokens (num_tokens=2304 avail_mem=43.72 GB):  26%|██▌       | 15/58 [00:05<00:09,  4.72it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=43.72 GB):  28%|██▊       | 16/58 [00:05<00:08,  5.24it/s]Capturing num tokens (num_tokens=2048 avail_mem=43.75 GB):  28%|██▊       | 16/58 [00:05<00:08,  5.24it/s]Capturing num tokens (num_tokens=2048 avail_mem=43.75 GB):  29%|██▉       | 17/58 [00:05<00:07,  5.84it/s]Capturing num tokens (num_tokens=1792 avail_mem=43.79 GB):  29%|██▉       | 17/58 [00:05<00:07,  5.84it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=43.79 GB):  31%|███       | 18/58 [00:05<00:06,  6.46it/s]Capturing num tokens (num_tokens=1536 avail_mem=43.81 GB):  31%|███       | 18/58 [00:05<00:06,  6.46it/s]Capturing num tokens (num_tokens=1280 avail_mem=43.84 GB):  31%|███       | 18/58 [00:06<00:06,  6.46it/s]Capturing num tokens (num_tokens=1280 avail_mem=43.84 GB):  34%|███▍      | 20/58 [00:06<00:04,  8.09it/s]Capturing num tokens (num_tokens=1024 avail_mem=43.88 GB):  34%|███▍      | 20/58 [00:06<00:04,  8.09it/s]

    Capturing num tokens (num_tokens=960 avail_mem=43.89 GB):  34%|███▍      | 20/58 [00:06<00:04,  8.09it/s] Capturing num tokens (num_tokens=960 avail_mem=43.89 GB):  38%|███▊      | 22/58 [00:06<00:03,  9.85it/s]Capturing num tokens (num_tokens=896 avail_mem=43.91 GB):  38%|███▊      | 22/58 [00:06<00:03,  9.85it/s]

    Capturing num tokens (num_tokens=832 avail_mem=44.03 GB):  38%|███▊      | 22/58 [00:06<00:03,  9.85it/s]Capturing num tokens (num_tokens=832 avail_mem=44.03 GB):  41%|████▏     | 24/58 [00:06<00:03,  8.60it/s]Capturing num tokens (num_tokens=768 avail_mem=44.01 GB):  41%|████▏     | 24/58 [00:06<00:03,  8.60it/s]Capturing num tokens (num_tokens=704 avail_mem=43.91 GB):  41%|████▏     | 24/58 [00:06<00:03,  8.60it/s]

    Capturing num tokens (num_tokens=704 avail_mem=43.91 GB):  45%|████▍     | 26/58 [00:06<00:03, 10.04it/s]Capturing num tokens (num_tokens=640 avail_mem=43.91 GB):  45%|████▍     | 26/58 [00:06<00:03, 10.04it/s]Capturing num tokens (num_tokens=576 avail_mem=43.91 GB):  45%|████▍     | 26/58 [00:06<00:03, 10.04it/s]Capturing num tokens (num_tokens=576 avail_mem=43.91 GB):  48%|████▊     | 28/58 [00:06<00:02, 11.84it/s]Capturing num tokens (num_tokens=512 avail_mem=44.00 GB):  48%|████▊     | 28/58 [00:06<00:02, 11.84it/s]Capturing num tokens (num_tokens=480 avail_mem=43.98 GB):  48%|████▊     | 28/58 [00:06<00:02, 11.84it/s]

    Capturing num tokens (num_tokens=448 avail_mem=43.98 GB):  48%|████▊     | 28/58 [00:06<00:02, 11.84it/s]Capturing num tokens (num_tokens=448 avail_mem=43.98 GB):  53%|█████▎    | 31/58 [00:06<00:01, 14.19it/s]Capturing num tokens (num_tokens=416 avail_mem=43.97 GB):  53%|█████▎    | 31/58 [00:06<00:01, 14.19it/s]Capturing num tokens (num_tokens=384 avail_mem=43.92 GB):  53%|█████▎    | 31/58 [00:06<00:01, 14.19it/s]Capturing num tokens (num_tokens=384 avail_mem=43.92 GB):  57%|█████▋    | 33/58 [00:07<00:01, 14.95it/s]Capturing num tokens (num_tokens=352 avail_mem=43.95 GB):  57%|█████▋    | 33/58 [00:07<00:01, 14.95it/s]

    Capturing num tokens (num_tokens=320 avail_mem=43.94 GB):  57%|█████▋    | 33/58 [00:07<00:01, 14.95it/s]Capturing num tokens (num_tokens=288 avail_mem=43.93 GB):  57%|█████▋    | 33/58 [00:07<00:01, 14.95it/s]Capturing num tokens (num_tokens=288 avail_mem=43.93 GB):  62%|██████▏   | 36/58 [00:07<00:01, 17.40it/s]Capturing num tokens (num_tokens=256 avail_mem=43.92 GB):  62%|██████▏   | 36/58 [00:07<00:01, 17.40it/s]Capturing num tokens (num_tokens=240 avail_mem=43.92 GB):  62%|██████▏   | 36/58 [00:07<00:01, 17.40it/s]Capturing num tokens (num_tokens=224 avail_mem=43.91 GB):  62%|██████▏   | 36/58 [00:07<00:01, 17.40it/s]Capturing num tokens (num_tokens=224 avail_mem=43.91 GB):  67%|██████▋   | 39/58 [00:07<00:00, 19.77it/s]Capturing num tokens (num_tokens=208 avail_mem=43.90 GB):  67%|██████▋   | 39/58 [00:07<00:00, 19.77it/s]

    Capturing num tokens (num_tokens=192 avail_mem=43.90 GB):  67%|██████▋   | 39/58 [00:07<00:00, 19.77it/s]Capturing num tokens (num_tokens=176 avail_mem=43.89 GB):  67%|██████▋   | 39/58 [00:07<00:00, 19.77it/s]Capturing num tokens (num_tokens=176 avail_mem=43.89 GB):  72%|███████▏  | 42/58 [00:07<00:00, 21.68it/s]Capturing num tokens (num_tokens=160 avail_mem=43.88 GB):  72%|███████▏  | 42/58 [00:07<00:00, 21.68it/s]Capturing num tokens (num_tokens=144 avail_mem=43.89 GB):  72%|███████▏  | 42/58 [00:07<00:00, 21.68it/s]

    Capturing num tokens (num_tokens=128 avail_mem=43.89 GB):  72%|███████▏  | 42/58 [00:07<00:00, 21.68it/s]Capturing num tokens (num_tokens=128 avail_mem=43.89 GB):  78%|███████▊  | 45/58 [00:07<00:00, 20.19it/s]Capturing num tokens (num_tokens=112 avail_mem=43.89 GB):  78%|███████▊  | 45/58 [00:07<00:00, 20.19it/s]Capturing num tokens (num_tokens=96 avail_mem=43.88 GB):  78%|███████▊  | 45/58 [00:07<00:00, 20.19it/s] Capturing num tokens (num_tokens=80 avail_mem=43.87 GB):  78%|███████▊  | 45/58 [00:07<00:00, 20.19it/s]Capturing num tokens (num_tokens=80 avail_mem=43.87 GB):  83%|████████▎ | 48/58 [00:07<00:00, 21.93it/s]Capturing num tokens (num_tokens=64 avail_mem=43.86 GB):  83%|████████▎ | 48/58 [00:07<00:00, 21.93it/s]Capturing num tokens (num_tokens=48 avail_mem=43.86 GB):  83%|████████▎ | 48/58 [00:07<00:00, 21.93it/s]

    Capturing num tokens (num_tokens=32 avail_mem=43.85 GB):  83%|████████▎ | 48/58 [00:07<00:00, 21.93it/s]Capturing num tokens (num_tokens=32 avail_mem=43.85 GB):  88%|████████▊ | 51/58 [00:07<00:00, 23.71it/s]Capturing num tokens (num_tokens=28 avail_mem=43.84 GB):  88%|████████▊ | 51/58 [00:07<00:00, 23.71it/s]Capturing num tokens (num_tokens=24 avail_mem=43.84 GB):  88%|████████▊ | 51/58 [00:07<00:00, 23.71it/s]Capturing num tokens (num_tokens=20 avail_mem=43.83 GB):  88%|████████▊ | 51/58 [00:07<00:00, 23.71it/s]Capturing num tokens (num_tokens=20 avail_mem=43.83 GB):  93%|█████████▎| 54/58 [00:07<00:00, 25.28it/s]Capturing num tokens (num_tokens=16 avail_mem=43.82 GB):  93%|█████████▎| 54/58 [00:07<00:00, 25.28it/s]Capturing num tokens (num_tokens=12 avail_mem=43.81 GB):  93%|█████████▎| 54/58 [00:07<00:00, 25.28it/s]

    Capturing num tokens (num_tokens=8 avail_mem=43.81 GB):  93%|█████████▎| 54/58 [00:07<00:00, 25.28it/s] Capturing num tokens (num_tokens=8 avail_mem=43.81 GB):  98%|█████████▊| 57/58 [00:08<00:00, 23.98it/s]Capturing num tokens (num_tokens=4 avail_mem=43.80 GB):  98%|█████████▊| 57/58 [00:08<00:00, 23.98it/s]Capturing num tokens (num_tokens=4 avail_mem=43.80 GB): 100%|██████████| 58/58 [00:08<00:00,  7.19it/s]


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


<strong style='color: #00008B;'>reasoing_content: Okay, so the user is in New York and wants the current date and time along with the weather. I need to figure out how to get this information using the provided functions.<br><br>First, I should use the 'get_current_date' function. The function requires a timezone parameter. Since the user is in New York, I'll set the timezone to 'America/New_York'. That should give me the correct date and time.<br><br>Next, I need the weather information. The 'get_current_weather' function is perfect for this. I'll need the city and state. The city is 'New York', and the state is 'NY'. I also have to specify the unit. The user didn't mention it, but since weather data is often in Celsius, I'll default to 'celsius'.<br><br>Now, I'll structure the function calls. I'll start with get_current_date, include the timezone parameter with its value. Then, I'll call get_current_weather with city, state, and unit as parameters. I'll make sure to format each function call correctly, using the specified JSON structure within the function tags.<br><br>I should also remember to include the sources in the response. For the date, it's from the get_current_date function, and for the weather, it's from get_current_weather. That way, the user knows where the information came from.<br><br>Putting it all together, I'll write each function call on separate lines, making sure the syntax is correct. I'll start each with '<function=' followed by the function name and the parameters in JSON, then close with '</function>'. <br><br>I think that covers everything the user asked for. They wanted both the date/time and weather, so breaking it down into two separate function calls makes sense and keeps it organized.<br><br><br>content: <function=get_current_date>{"timezone": "America/New_York"}</function>  <br><function=get_current_weather>{"city": "New York", "state": "NY", "unit": "celsius"}</function></strong>


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

    {'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down. First, I need to identify what the capital of France is. I know that Paris is the capital, so that\'s straightforward. \n\nNext, I need to find the population. I remember that Paris is a major city, so its population is quite large. I think it\'s over 3 million, but I\'m not exactly sure of the exact number. Maybe I should double-check that. \n\nWait, I recall that the population figure can vary depending on the source and the year. The user didn\'t specify a particular year, so I should probably go with the most recent estimate. I believe the population is around 3,500,000 as of 2023. \n\nNow, I need to structure this information into a JSON format. JSON typically uses key-value pairs, so I\'ll create an object with keys like "city", "population", and maybe "country" since the user mentioned France. \n\nI should make sure the keys are in English to keep it clear. The city is Paris, the population is 3,500,000, and the country is France. I\'ll format this into a JSON object, ensuring proper syntax with commas and quotation marks. \n\nI also need to present this in a way that\'s easy to read, so I\'ll put each key on a new line. That way, the user can quickly see the information without confusion. \n\nI wonder if the user needs more details, like the exact current population or additional statistics. But since they only asked for the capital and population, I\'ll stick to that. \n\nLastly, I\'ll make sure the JSON is valid by checking the syntax. No trailing commas, proper use of braces, and correct quotation marks. That should cover everything the user needs.\n</think>{\n  "name": "Paris",\n  "population": 3500000\n}', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 13, 5512, 11, 358, 1184, 311, 10542, 1128, 279, 6722, 315, 9625, 374, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4710, 5847, 11, 358, 1184, 311, 1477, 279, 7042, 13, 358, 6099, 429, 12095, 374, 264, 3598, 3283, 11, 773, 1181, 7042, 374, 5008, 3460, 13, 358, 1744, 432, 594, 916, 220, 18, 3526, 11, 714, 358, 2776, 537, 6896, 2704, 315, 279, 4734, 1372, 13, 10696, 358, 1265, 1990, 15934, 429, 13, 4710, 14190, 11, 358, 19091, 429, 279, 7042, 7071, 646, 13289, 11649, 389, 279, 2530, 323, 279, 1042, 13, 576, 1196, 3207, 944, 13837, 264, 3953, 1042, 11, 773, 358, 1265, 4658, 728, 448, 279, 1429, 3213, 16045, 13, 358, 4411, 279, 7042, 374, 2163, 220, 18, 11, 20, 15, 15, 11, 15, 15, 15, 438, 315, 220, 17, 15, 17, 18, 13, 4710, 7039, 11, 358, 1184, 311, 5944, 419, 1995, 1119, 264, 4718, 3561, 13, 4718, 11136, 5711, 1376, 19083, 13530, 11, 773, 358, 3278, 1855, 458, 1633, 448, 6894, 1075, 330, 8926, 497, 330, 44441, 497, 323, 7196, 330, 11141, 1, 2474, 279, 1196, 9733, 9625, 13, 4710, 40, 1265, 1281, 2704, 279, 6894, 525, 304, 6364, 311, 2506, 432, 2797, 13, 576, 3283, 374, 12095, 11, 279, 7042, 374, 220, 18, 11, 20, 15, 15, 11, 15, 15, 15, 11, 323, 279, 3146, 374, 9625, 13, 358, 3278, 3561, 419, 1119, 264, 4718, 1633, 11, 22573, 6169, 19482, 448, 76602, 323, 54231, 15423, 13, 4710, 40, 1083, 1184, 311, 3042, 419, 304, 264, 1616, 429, 594, 4135, 311, 1349, 11, 773, 358, 3278, 2182, 1817, 1376, 389, 264, 501, 1555, 13, 2938, 1616, 11, 279, 1196, 646, 6157, 1490, 279, 1995, 2041, 21340, 13, 4710, 40, 5775, 421, 279, 1196, 3880, 803, 3565, 11, 1075, 279, 4734, 1482, 7042, 476, 5107, 13142, 13, 1988, 2474, 807, 1172, 4588, 369, 279, 6722, 323, 7042, 11, 358, 3278, 9214, 311, 429, 13, 4710, 80486, 11, 358, 3278, 1281, 2704, 279, 4718, 374, 2697, 553, 13295, 279, 19482, 13, 2308, 27748, 76602, 11, 6169, 990, 315, 59191, 11, 323, 4396, 54231, 15423, 13, 2938, 1265, 3421, 4297, 279, 1196, 3880, 624, 151649, 515, 220, 330, 606, 788, 330, 59604, 756, 220, 330, 44441, 788, 220, 18, 20, 15, 15, 15, 15, 15, 198, 92, 151643], 'meta_info': {'id': '0ef7a9c4c02a41f4982b3ac566605535', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 412, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 3.116937111946754, 'response_sent_to_client_ts': 1774860475.8201873}}



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


<strong style='color: #00008B;'>{'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. I know that Paris is the capital, so that\'s straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I\'m not sure of the exact number. I think it\'s around 2 million, but I should verify that.\n\nI\'ll check a reliable source, maybe the official Paris Municipality website or a recent census. Let me see... Yes, according to the latest data, the population is approximately 2,174,300 as of 2023. That seems accurate.\n\nNext, I need to structure this information into a JSON format. JSON requires key-value pairs, so I\'ll create an object with keys like "city", "population", and "country". The city is Paris, the population is the number I found, and the country is France.\n\nI should make sure the JSON syntax is correct. Each key should be in quotes, and the values as well. The entire structure should be enclosed in curly braces. I\'ll format it properly to avoid any syntax errors.\n\nPutting it all together, the JSON object will have the city, population, and country. I\'ll present this to the user, making sure it\'s clear and easy to understand. I don\'t think the user needs anything more detailed, so this should suffice.\n\nI should also consider if the user might need additional information, like the area or age distribution, but since they only asked for population, I\'ll stick to that. Maybe they\'ll ask for more details later, but for now, this response should be helpful.\n</think>{\n  "name": "Paris",\n  "population": 2174300\n}', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1265, 1477, 279, 1429, 3213, 7042, 821, 13, 358, 6099, 429, 279, 7042, 315, 12095, 702, 1012, 7826, 11, 714, 358, 2776, 537, 2704, 315, 279, 4734, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 10146, 429, 382, 40, 3278, 1779, 264, 14720, 2530, 11, 7196, 279, 3946, 12095, 35703, 2719, 3910, 476, 264, 3213, 43602, 13, 6771, 752, 1490, 1112, 7414, 11, 4092, 311, 279, 5535, 821, 11, 279, 7042, 374, 13187, 220, 17, 11, 16, 22, 19, 11, 18, 15, 15, 438, 315, 220, 17, 15, 17, 18, 13, 2938, 4977, 13382, 382, 5847, 11, 358, 1184, 311, 5944, 419, 1995, 1119, 264, 4718, 3561, 13, 4718, 7460, 1376, 19083, 13530, 11, 773, 358, 3278, 1855, 458, 1633, 448, 6894, 1075, 330, 8926, 497, 330, 44441, 497, 323, 330, 11141, 3263, 576, 3283, 374, 12095, 11, 279, 7042, 374, 279, 1372, 358, 1730, 11, 323, 279, 3146, 374, 9625, 382, 40, 1265, 1281, 2704, 279, 4718, 19482, 374, 4396, 13, 8886, 1376, 1265, 387, 304, 17194, 11, 323, 279, 2750, 438, 1632, 13, 576, 4453, 5944, 1265, 387, 43810, 304, 68103, 59191, 13, 358, 3278, 3561, 432, 10277, 311, 5648, 894, 19482, 5975, 382, 97904, 432, 678, 3786, 11, 279, 4718, 1633, 686, 614, 279, 3283, 11, 7042, 11, 323, 3146, 13, 358, 3278, 3042, 419, 311, 279, 1196, 11, 3259, 2704, 432, 594, 2797, 323, 4135, 311, 3535, 13, 358, 1513, 944, 1744, 279, 1196, 3880, 4113, 803, 11682, 11, 773, 419, 1265, 76156, 382, 40, 1265, 1083, 2908, 421, 279, 1196, 2578, 1184, 5107, 1995, 11, 1075, 279, 3082, 476, 4231, 7982, 11, 714, 2474, 807, 1172, 4588, 369, 7042, 11, 358, 3278, 9214, 311, 429, 13, 10696, 807, 3278, 2548, 369, 803, 3565, 2937, 11, 714, 369, 1431, 11, 419, 2033, 1265, 387, 10950, 624, 151649, 515, 220, 330, 606, 788, 330, 59604, 756, 220, 330, 44441, 788, 220, 17, 16, 22, 19, 18, 15, 15, 198, 92, 151643], 'meta_info': {'id': '0b7a619d9ea5468e8b8307249934414b', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 384, 'cached_tokens': 22, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 2.6439352720044553, 'response_sent_to_client_ts': 1774860478.4741366}}</strong>


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

    [{'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': 'c16bfae36b4048e79bea0ec7ae6c8c60', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.08154780196491629, 'response_sent_to_client_ts': 1774860478.5822203}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '820d39336a7140e9b7ccd79c10d511a0', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.08148227003403008, 'response_sent_to_client_ts': 1774860478.5822325}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '2bc4ade3a1d34d40b4a09afb56b7167c', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.08144564903341234, 'response_sent_to_client_ts': 1774860478.582237}}]


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

    {'text': ' France, and the \n\\( n \\)  \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\(', 'output_ids': [9625, 11, 323, 279, 220, 198, 44292, 308, 1124, 8, 220, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767], 'meta_info': {'id': '0a47f1b7235944b289fb38d94c0b90e0', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 6, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 19.696984826005064, 'response_sent_to_client_ts': 1774860498.2869716}}


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


<strong style='color: #00008B;'>{'text': 'Okay, so the user asked for the information and population of the capital of France in JSON format. Yeah, that makes sense. First, I need to figure out what the capital is. Let me think... is it Paris? I\'m pretty sure that\'s correct.\n\nAlright, so the next step is to gather the population data. I know that populations can change over time, so I should probably provide the most recent estimate. I think as of 2023, the population is around 2.16 million. Hmm, but I\'m not 100% sure. Maybe I should double-check that number to make it accurate.\n\nNow, putting this into JSON format, I remember that JSON uses key-value pairs. So the keys would be things like "city," "population," "region," and "country." The values would be the specific data: Paris as the city, 2164000 as the population, and so on. I should probably structure it clearly, maybe using proper formatting with indentation for readability.\n\nWait, I also recall that Paris is the second largest city in France, right? So maybe including the rank could be helpful. And the region is Ile-de-France, which is a large region in the north part of the country. The country itself is, of course, France. Adding that gives a bit more context.\n\nI should make sure the JSON syntax is correct – commas, colons, brackets, and so on. No trailing commas or missing braces, that could cause errors. Also, the numbers should be accurate and match up with reputable sources. If I\'m unsure about the population, it\'s best to confirm from a reliable database or a recent census report.\n\nAdditionally, thinking about the user\'s potential needs, they might just be looking for a quick fact check. So providing precise and up-to-date information is crucial. I should avoid any assumptions and stick to the most current data available. \n\nHmm, also, sometimes population numbers can vary based on the source – like government vs. estimate from other organizations. So it\'s important to note the source or the year of the data if possible. But maybe that\'s beyond what\'s needed here. The user didn\'t specify, just asking for the data, so perhaps just numbers are enough.\n\nAlright, putting it all together: city name, population number, region, and country, all in proper JSON format. Double-checking to ensure no typos and that the data makes sense. I think that covers everything the user is asking for.\n</think>\n\nHere is the information and population of the capital of France in JSON format:\n\n```json\n{\n  "city": "Paris",\n  "region": "Ile-de-France",\n  "country": "France",\n  "population": 2164000\n}\n```', 'output_ids': [32313, 11, 773, 279, 1196, 4588, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 21607, 11, 429, 3643, 5530, 13, 5512, 11, 358, 1184, 311, 7071, 700, 1128, 279, 6722, 374, 13, 6771, 752, 1744, 1112, 374, 432, 12095, 30, 358, 2776, 5020, 2704, 429, 594, 4396, 382, 71486, 11, 773, 279, 1790, 3019, 374, 311, 9567, 279, 7042, 821, 13, 358, 1414, 429, 21910, 646, 2297, 916, 882, 11, 773, 358, 1265, 4658, 3410, 279, 1429, 3213, 16045, 13, 358, 1744, 438, 315, 220, 17, 15, 17, 18, 11, 279, 7042, 374, 2163, 220, 17, 13, 16, 21, 3526, 13, 88190, 11, 714, 358, 2776, 537, 220, 16, 15, 15, 4, 2704, 13, 10696, 358, 1265, 1990, 15934, 429, 1372, 311, 1281, 432, 13382, 382, 7039, 11, 10687, 419, 1119, 4718, 3561, 11, 358, 6099, 429, 4718, 5711, 1376, 19083, 13530, 13, 2055, 279, 6894, 1035, 387, 2513, 1075, 330, 8926, 1335, 330, 44441, 1335, 330, 3943, 1335, 323, 330, 11141, 1189, 576, 2750, 1035, 387, 279, 3151, 821, 25, 12095, 438, 279, 3283, 11, 220, 17, 16, 21, 19, 15, 15, 15, 438, 279, 7042, 11, 323, 773, 389, 13, 358, 1265, 4658, 5944, 432, 9355, 11, 7196, 1667, 6169, 36566, 448, 69592, 369, 91494, 382, 14190, 11, 358, 1083, 19091, 429, 12095, 374, 279, 2086, 7772, 3283, 304, 9625, 11, 1290, 30, 2055, 7196, 2670, 279, 7077, 1410, 387, 10950, 13, 1597, 279, 5537, 374, 358, 273, 6810, 7276, 34106, 11, 892, 374, 264, 3460, 5537, 304, 279, 10200, 949, 315, 279, 3146, 13, 576, 3146, 5086, 374, 11, 315, 3308, 11, 9625, 13, 30370, 429, 6696, 264, 2699, 803, 2266, 382, 40, 1265, 1281, 2704, 279, 4718, 19482, 374, 4396, 1365, 76602, 11, 1375, 2382, 11, 38929, 11, 323, 773, 389, 13, 2308, 27748, 76602, 476, 7402, 59191, 11, 429, 1410, 5240, 5975, 13, 7281, 11, 279, 5109, 1265, 387, 13382, 323, 2432, 705, 448, 55840, 8173, 13, 1416, 358, 2776, 42903, 911, 279, 7042, 11, 432, 594, 1850, 311, 7683, 504, 264, 14720, 4625, 476, 264, 3213, 43602, 1895, 382, 49574, 11, 7274, 911, 279, 1196, 594, 4650, 3880, 11, 807, 2578, 1101, 387, 3330, 369, 264, 3974, 2097, 1779, 13, 2055, 8241, 23560, 323, 705, 4686, 18413, 1995, 374, 16587, 13, 358, 1265, 5648, 894, 31846, 323, 9214, 311, 279, 1429, 1482, 821, 2500, 13, 4710, 80022, 11, 1083, 11, 7025, 7042, 5109, 646, 13289, 3118, 389, 279, 2530, 1365, 1075, 3033, 6165, 13, 16045, 504, 1008, 11104, 13, 2055, 432, 594, 2989, 311, 5185, 279, 2530, 476, 279, 1042, 315, 279, 821, 421, 3204, 13, 1988, 7196, 429, 594, 7797, 1128, 594, 4362, 1588, 13, 576, 1196, 3207, 944, 13837, 11, 1101, 10161, 369, 279, 821, 11, 773, 8365, 1101, 5109, 525, 3322, 382, 71486, 11, 10687, 432, 678, 3786, 25, 3283, 829, 11, 7042, 1372, 11, 5537, 11, 323, 3146, 11, 678, 304, 6169, 4718, 3561, 13, 7093, 15934, 287, 311, 5978, 902, 13580, 966, 323, 429, 279, 821, 3643, 5530, 13, 358, 1744, 429, 14521, 4297, 279, 1196, 374, 10161, 369, 624, 151649, 271, 8420, 374, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 1447, 73594, 2236, 198, 515, 220, 330, 8926, 788, 330, 59604, 756, 220, 330, 3943, 788, 330, 40, 273, 6810, 7276, 34106, 756, 220, 330, 11141, 788, 330, 49000, 756, 220, 330, 44441, 788, 220, 17, 16, 21, 19, 15, 15, 15, 198, 532, 73594, 151643], 'meta_info': {'id': 'c6c7a5a5735e43e78a3e46af4e850ca0', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 579, 'cached_tokens': 22, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 4.642327616922557, 'response_sent_to_client_ts': 1774860502.940119}}</strong>



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


    2026-03-30 08:48:35.690 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:48:35] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 08:48:35.690 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:48:35] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 08:48:35.690 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:48:35] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 08:48:35.690 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:48:35] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 08:48:35.690 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:48:35] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:00<00:00,  1.03it/s]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.21s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.18s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:03,  3.22s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:03,  3.22s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:27,  1.57s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:27,  1.57s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:51,  1.06it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:51,  1.06it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:34,  1.56it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:34,  1.56it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:24,  2.14it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:24,  2.14it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.76it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.76it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.50it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.50it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.28it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.28it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.15it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.15it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:04<00:07,  6.05it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:04<00:07,  6.05it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:04<00:07,  6.05it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:04<00:05,  7.80it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:04<00:05,  7.80it/s]

    Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:05<00:05,  7.80it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:05<00:04,  9.37it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:05<00:04,  9.37it/s]Compiling num tokens (num_tokens=2304):  24%|██▍       | 14/58 [00:05<00:04,  9.37it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:05<00:03, 11.06it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:05<00:03, 11.06it/s]

    Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:05<00:03, 11.06it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:05<00:03, 12.98it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:05<00:03, 12.98it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:05<00:03, 12.98it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:05<00:03, 12.98it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:05<00:02, 16.48it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:05<00:02, 16.48it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:05<00:02, 16.48it/s]

    Compiling num tokens (num_tokens=832):  36%|███▌      | 21/58 [00:05<00:02, 16.48it/s]Compiling num tokens (num_tokens=768):  36%|███▌      | 21/58 [00:05<00:02, 16.48it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:05<00:01, 21.94it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:05<00:01, 21.94it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:05<00:01, 21.94it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:05<00:01, 21.94it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:05<00:01, 21.94it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:05<00:01, 21.94it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:05<00:00, 28.83it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:05<00:00, 28.83it/s]Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:05<00:00, 28.83it/s]

    Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:05<00:00, 28.83it/s]Compiling num tokens (num_tokens=352):  52%|█████▏    | 30/58 [00:05<00:00, 28.83it/s]Compiling num tokens (num_tokens=320):  52%|█████▏    | 30/58 [00:05<00:00, 28.83it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:05<00:00, 34.18it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:05<00:00, 34.18it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:05<00:00, 34.18it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:05<00:00, 34.18it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:05<00:00, 34.18it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:05<00:00, 34.18it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:05<00:00, 34.18it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:05<00:00, 39.89it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:05<00:00, 39.89it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:05<00:00, 39.89it/s]

    Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:05<00:00, 39.89it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:05<00:00, 39.89it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:05<00:00, 39.89it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:05<00:00, 39.89it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:05<00:00, 44.14it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:05<00:00, 44.14it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:06<00:00, 44.14it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:06<00:00, 44.14it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:06<00:00, 44.14it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:06<00:00, 44.14it/s]Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:06<00:00, 44.14it/s]Compiling num tokens (num_tokens=20):  81%|████████  | 47/58 [00:06<00:00, 44.14it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:06<00:00, 50.23it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:06<00:00, 50.23it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:06<00:00, 50.23it/s]

    Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:06<00:00, 50.23it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:06<00:00, 50.23it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:06<00:00,  9.46it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=61.62 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=61.62 GB):   2%|▏         | 1/58 [00:00<00:23,  2.40it/s]Capturing num tokens (num_tokens=7680 avail_mem=61.59 GB):   2%|▏         | 1/58 [00:00<00:23,  2.40it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=61.59 GB):   3%|▎         | 2/58 [00:00<00:20,  2.71it/s]Capturing num tokens (num_tokens=7168 avail_mem=61.57 GB):   3%|▎         | 2/58 [00:00<00:20,  2.71it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=61.57 GB):   5%|▌         | 3/58 [00:01<00:20,  2.72it/s]Capturing num tokens (num_tokens=6656 avail_mem=61.58 GB):   5%|▌         | 3/58 [00:01<00:20,  2.72it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=61.58 GB):   7%|▋         | 4/58 [00:01<00:17,  3.11it/s]Capturing num tokens (num_tokens=6144 avail_mem=61.09 GB):   7%|▋         | 4/58 [00:01<00:17,  3.11it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=61.09 GB):   9%|▊         | 5/58 [00:01<00:14,  3.57it/s]Capturing num tokens (num_tokens=5632 avail_mem=60.93 GB):   9%|▊         | 5/58 [00:01<00:14,  3.57it/s]Capturing num tokens (num_tokens=5632 avail_mem=60.93 GB):  10%|█         | 6/58 [00:01<00:12,  4.08it/s]Capturing num tokens (num_tokens=5120 avail_mem=60.93 GB):  10%|█         | 6/58 [00:01<00:12,  4.08it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=60.93 GB):  12%|█▏        | 7/58 [00:01<00:11,  4.57it/s]Capturing num tokens (num_tokens=4608 avail_mem=60.94 GB):  12%|█▏        | 7/58 [00:01<00:11,  4.57it/s]Capturing num tokens (num_tokens=4608 avail_mem=60.94 GB):  14%|█▍        | 8/58 [00:02<00:09,  5.16it/s]Capturing num tokens (num_tokens=4096 avail_mem=60.94 GB):  14%|█▍        | 8/58 [00:02<00:09,  5.16it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=60.94 GB):  16%|█▌        | 9/58 [00:02<00:08,  5.74it/s]Capturing num tokens (num_tokens=3840 avail_mem=60.94 GB):  16%|█▌        | 9/58 [00:02<00:08,  5.74it/s]Capturing num tokens (num_tokens=3840 avail_mem=60.94 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.29it/s]Capturing num tokens (num_tokens=3584 avail_mem=60.94 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.29it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=60.94 GB):  19%|█▉        | 11/58 [00:02<00:06,  6.83it/s]Capturing num tokens (num_tokens=3328 avail_mem=60.94 GB):  19%|█▉        | 11/58 [00:02<00:06,  6.83it/s]Capturing num tokens (num_tokens=3328 avail_mem=60.94 GB):  21%|██        | 12/58 [00:02<00:06,  7.47it/s]Capturing num tokens (num_tokens=3072 avail_mem=60.94 GB):  21%|██        | 12/58 [00:02<00:06,  7.47it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=60.94 GB):  21%|██        | 12/58 [00:02<00:06,  7.47it/s]Capturing num tokens (num_tokens=2816 avail_mem=60.94 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.56it/s]Capturing num tokens (num_tokens=2560 avail_mem=60.94 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.56it/s]Capturing num tokens (num_tokens=2304 avail_mem=60.94 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.56it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=60.94 GB):  28%|██▊       | 16/58 [00:02<00:04,  9.87it/s]Capturing num tokens (num_tokens=2048 avail_mem=60.94 GB):  28%|██▊       | 16/58 [00:02<00:04,  9.87it/s]Capturing num tokens (num_tokens=1792 avail_mem=60.94 GB):  28%|██▊       | 16/58 [00:02<00:04,  9.87it/s]Capturing num tokens (num_tokens=1792 avail_mem=60.94 GB):  31%|███       | 18/58 [00:03<00:03, 10.76it/s]Capturing num tokens (num_tokens=1536 avail_mem=60.94 GB):  31%|███       | 18/58 [00:03<00:03, 10.76it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=60.94 GB):  31%|███       | 18/58 [00:03<00:03, 10.76it/s]Capturing num tokens (num_tokens=1280 avail_mem=60.94 GB):  34%|███▍      | 20/58 [00:03<00:02, 12.69it/s]Capturing num tokens (num_tokens=1024 avail_mem=60.94 GB):  34%|███▍      | 20/58 [00:03<00:02, 12.69it/s]Capturing num tokens (num_tokens=960 avail_mem=60.93 GB):  34%|███▍      | 20/58 [00:03<00:02, 12.69it/s] Capturing num tokens (num_tokens=896 avail_mem=60.93 GB):  34%|███▍      | 20/58 [00:03<00:02, 12.69it/s]Capturing num tokens (num_tokens=896 avail_mem=60.93 GB):  40%|███▉      | 23/58 [00:03<00:02, 15.53it/s]Capturing num tokens (num_tokens=832 avail_mem=60.93 GB):  40%|███▉      | 23/58 [00:03<00:02, 15.53it/s]

    Capturing num tokens (num_tokens=768 avail_mem=60.92 GB):  40%|███▉      | 23/58 [00:03<00:02, 15.53it/s]Capturing num tokens (num_tokens=704 avail_mem=60.92 GB):  40%|███▉      | 23/58 [00:03<00:02, 15.53it/s]Capturing num tokens (num_tokens=704 avail_mem=60.92 GB):  45%|████▍     | 26/58 [00:03<00:01, 18.72it/s]Capturing num tokens (num_tokens=640 avail_mem=60.92 GB):  45%|████▍     | 26/58 [00:03<00:01, 18.72it/s]Capturing num tokens (num_tokens=576 avail_mem=60.91 GB):  45%|████▍     | 26/58 [00:03<00:01, 18.72it/s]Capturing num tokens (num_tokens=512 avail_mem=60.91 GB):  45%|████▍     | 26/58 [00:03<00:01, 18.72it/s]Capturing num tokens (num_tokens=480 avail_mem=60.91 GB):  45%|████▍     | 26/58 [00:03<00:01, 18.72it/s]Capturing num tokens (num_tokens=480 avail_mem=60.91 GB):  52%|█████▏    | 30/58 [00:03<00:01, 22.82it/s]Capturing num tokens (num_tokens=448 avail_mem=60.90 GB):  52%|█████▏    | 30/58 [00:03<00:01, 22.82it/s]

    Capturing num tokens (num_tokens=416 avail_mem=60.90 GB):  52%|█████▏    | 30/58 [00:03<00:01, 22.82it/s]Capturing num tokens (num_tokens=384 avail_mem=60.90 GB):  52%|█████▏    | 30/58 [00:03<00:01, 22.82it/s]Capturing num tokens (num_tokens=352 avail_mem=60.89 GB):  52%|█████▏    | 30/58 [00:03<00:01, 22.82it/s]Capturing num tokens (num_tokens=352 avail_mem=60.89 GB):  59%|█████▊    | 34/58 [00:03<00:00, 26.05it/s]Capturing num tokens (num_tokens=320 avail_mem=60.89 GB):  59%|█████▊    | 34/58 [00:03<00:00, 26.05it/s]Capturing num tokens (num_tokens=288 avail_mem=59.83 GB):  59%|█████▊    | 34/58 [00:03<00:00, 26.05it/s]Capturing num tokens (num_tokens=256 avail_mem=59.82 GB):  59%|█████▊    | 34/58 [00:03<00:00, 26.05it/s]Capturing num tokens (num_tokens=240 avail_mem=59.82 GB):  59%|█████▊    | 34/58 [00:03<00:00, 26.05it/s]Capturing num tokens (num_tokens=240 avail_mem=59.82 GB):  66%|██████▌   | 38/58 [00:03<00:00, 29.50it/s]Capturing num tokens (num_tokens=224 avail_mem=59.82 GB):  66%|██████▌   | 38/58 [00:03<00:00, 29.50it/s]

    Capturing num tokens (num_tokens=208 avail_mem=59.81 GB):  66%|██████▌   | 38/58 [00:03<00:00, 29.50it/s]Capturing num tokens (num_tokens=192 avail_mem=50.89 GB):  66%|██████▌   | 38/58 [00:03<00:00, 29.50it/s]Capturing num tokens (num_tokens=176 avail_mem=45.84 GB):  66%|██████▌   | 38/58 [00:03<00:00, 29.50it/s]Capturing num tokens (num_tokens=176 avail_mem=45.84 GB):  72%|███████▏  | 42/58 [00:03<00:00, 31.94it/s]Capturing num tokens (num_tokens=160 avail_mem=45.84 GB):  72%|███████▏  | 42/58 [00:03<00:00, 31.94it/s]Capturing num tokens (num_tokens=144 avail_mem=45.83 GB):  72%|███████▏  | 42/58 [00:03<00:00, 31.94it/s]Capturing num tokens (num_tokens=128 avail_mem=45.84 GB):  72%|███████▏  | 42/58 [00:03<00:00, 31.94it/s]Capturing num tokens (num_tokens=112 avail_mem=45.84 GB):  72%|███████▏  | 42/58 [00:03<00:00, 31.94it/s]Capturing num tokens (num_tokens=112 avail_mem=45.84 GB):  79%|███████▉  | 46/58 [00:03<00:00, 34.06it/s]Capturing num tokens (num_tokens=96 avail_mem=45.83 GB):  79%|███████▉  | 46/58 [00:03<00:00, 34.06it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=45.83 GB):  79%|███████▉  | 46/58 [00:03<00:00, 34.06it/s]Capturing num tokens (num_tokens=64 avail_mem=45.83 GB):  79%|███████▉  | 46/58 [00:03<00:00, 34.06it/s]Capturing num tokens (num_tokens=48 avail_mem=45.83 GB):  79%|███████▉  | 46/58 [00:04<00:00, 34.06it/s]Capturing num tokens (num_tokens=48 avail_mem=45.83 GB):  86%|████████▌ | 50/58 [00:04<00:00, 33.87it/s]Capturing num tokens (num_tokens=32 avail_mem=45.82 GB):  86%|████████▌ | 50/58 [00:04<00:00, 33.87it/s]

    Capturing num tokens (num_tokens=28 avail_mem=45.82 GB):  86%|████████▌ | 50/58 [00:04<00:00, 33.87it/s]Capturing num tokens (num_tokens=24 avail_mem=45.82 GB):  86%|████████▌ | 50/58 [00:04<00:00, 33.87it/s]Capturing num tokens (num_tokens=20 avail_mem=45.81 GB):  86%|████████▌ | 50/58 [00:04<00:00, 33.87it/s]Capturing num tokens (num_tokens=20 avail_mem=45.81 GB):  93%|█████████▎| 54/58 [00:04<00:00, 24.69it/s]Capturing num tokens (num_tokens=16 avail_mem=45.81 GB):  93%|█████████▎| 54/58 [00:04<00:00, 24.69it/s]

    Capturing num tokens (num_tokens=12 avail_mem=45.80 GB):  93%|█████████▎| 54/58 [00:04<00:00, 24.69it/s]Capturing num tokens (num_tokens=8 avail_mem=45.80 GB):  93%|█████████▎| 54/58 [00:04<00:00, 24.69it/s] Capturing num tokens (num_tokens=8 avail_mem=45.80 GB):  98%|█████████▊| 57/58 [00:04<00:00, 21.24it/s]Capturing num tokens (num_tokens=4 avail_mem=45.80 GB):  98%|█████████▊| 57/58 [00:04<00:00, 21.24it/s]Capturing num tokens (num_tokens=4 avail_mem=45.80 GB): 100%|██████████| 58/58 [00:04<00:00, 12.76it/s]


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
    Generated text: Rome is the capital of Italy
    ===============================
    Prompt: Give me the information of the capital of Germany.
    Generated text: Berlin is the capital of Germany
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
    
    Generated text: Alright, so the user is asking for the information and population of the capital of France in JSON format. Hmm, first, I need to figure out who the user is and why they're asking this. Maybe they're a student working on a project, or perhaps they're just curious about the data. Either way, they need a structured response.
    
    Okay, the capital of France is definitely Paris. That part is straightforward. Now, I should provide the population figure. I remember that Paris is one of the most populous cities globally, but I'm not sure about the exact current number. I think it's around 2 million, but I should double-check that. Wait, the user might be looking for an accurate figure, so maybe I should confirm the latest data. From what I recall, as of 2023, Paris had a population of approximately 2,150,000. But I should make sure that's up to date. Alternatively, I could mention that the figure can vary slightly depending on the source.
    
    Now, the user specifically asked for JSON format. JSON stands for JavaScript Object Notation, which is a lightweight data-interchange format that's easy for humans to read and write, and easy for machines to parse and generate. So I'll need to structure the information into a JSON object with appropriate keys. The user might be using this data in a program or for a report, so clarity and structure are important.
    
    Let me think about the fields to include. The basic information would be the name of the city, the country, the population figure, and maybe the region it's in. That way, the data is comprehensive but not overly complicated. I should present this in a clear, straightforward JSON format without any extra fluff.
    
    Wait, should I include more detailed information, like the mayor's name or significant landmarks? The user didn't specify, so I'll stick to the essentials to keep it concise. They just asked for population, so focusing on that is best.
    
    Also, I should ensure the JSON syntax is correct. Using double quotes for strings, commas appropriately between key-value pairs, and making sure the overall structure is valid. I don't want any syntax errors that could cause issues when the user tries to use the data.
    
    Putting it all together, the JSON object will have keys like "City", "Country", "Population", and "Region". The population number should be included with its unit, probably as a string with the number and 's' for singular/plural consistency. Wait, actually, in JSON, numbers are typically in their raw form, so maybe just the number without the unit. But the user might expect the unit for clarity, so I'll include the number with an 's' for plural form.
    
    Let me write that out: 
    
    {
      "City": "Paris",
      "Country": "France",
      "Population": 2150000 + "s",
      "Region": "Ile-de-France"
    }
    
    Hmm, that looks correct. I should make sure that the keys are consistent and descriptive. The user can then easily access each piece of information by referencing the JSON keys.
    
    I should also consider if the population number is an estimate or exact. Since I'm using a figure from 2023, it's probably an estimate, so adding a note in the explanation might be helpful, but the user didn't ask for that. They just want the data in JSON, so I'll stick to providing the number as it is.
    
    Finally, I'll present the JSON neatly, maybe with line breaks for readability. That way, the user can copy it directly without issues. I think that covers everything the user needs. I should double-check everything before sending it to make sure there are no errors.
    </think>
    
    Here is the information and population of the capital of France in JSON format:
    
    ```json
    {
      "City": "Paris",
      "Country": "France",
      "Population": 2150000 + "s",
      "Region": "Ile-de-France"
    }
    ```



```python
llm.shutdown()
```
