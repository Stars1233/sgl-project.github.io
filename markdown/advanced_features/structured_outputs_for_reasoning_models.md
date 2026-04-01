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
    2026-04-01 01:44:31.062 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 01:44:31] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 01:44:31.062 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 01:44:31] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 01:44:31.062 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 01:44:31] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 01:44:31.062 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 01:44:31] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 01:44:31.062 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 01:44:31] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:00<00:00,  1.03it/s]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.23s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.20s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:53,  3.04s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:53,  3.04s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:40,  1.79s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:40,  1.79s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:04,  1.17s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:04,  1.17s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:47,  1.13it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:47,  1.13it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:39,  1.34it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:39,  1.34it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:33,  1.58it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:33,  1.58it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:06<00:26,  1.92it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:06<00:26,  1.92it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:06<00:21,  2.29it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:06<00:21,  2.29it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:06<00:16,  3.01it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:06<00:16,  3.01it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:06<00:16,  3.01it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:06<00:10,  4.55it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:06<00:10,  4.55it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:06<00:10,  4.55it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:06<00:07,  6.10it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:06<00:07,  6.10it/s]

    Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:06<00:07,  6.10it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:06<00:05,  7.72it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:06<00:05,  7.72it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:06<00:05,  7.72it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:06<00:04,  9.65it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:06<00:04,  9.65it/s]

    Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:07<00:04,  9.65it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:07<00:04,  9.65it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:07<00:02, 12.81it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:07<00:02, 12.81it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:07<00:02, 12.81it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:07<00:02, 12.81it/s]Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:07<00:02, 12.81it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:07<00:01, 17.92it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:07<00:01, 17.92it/s]

    Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:07<00:01, 17.92it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:07<00:01, 17.92it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:07<00:01, 17.92it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:07<00:01, 17.92it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:07<00:01, 24.53it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:07<00:01, 24.53it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:07<00:01, 24.53it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:07<00:01, 24.53it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:07<00:01, 24.53it/s]Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:07<00:01, 24.53it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:07<00:00, 30.55it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:07<00:00, 30.55it/s]

    Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:07<00:00, 30.55it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:07<00:00, 30.55it/s]Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:07<00:00, 30.55it/s]Compiling num tokens (num_tokens=224):  59%|█████▊    | 34/58 [00:07<00:00, 30.55it/s]Compiling num tokens (num_tokens=208):  59%|█████▊    | 34/58 [00:07<00:00, 30.55it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:07<00:00, 36.96it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:07<00:00, 36.96it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:07<00:00, 36.96it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:07<00:00, 36.96it/s]Compiling num tokens (num_tokens=144):  69%|██████▉   | 40/58 [00:07<00:00, 36.96it/s]Compiling num tokens (num_tokens=128):  69%|██████▉   | 40/58 [00:07<00:00, 36.96it/s]Compiling num tokens (num_tokens=112):  69%|██████▉   | 40/58 [00:07<00:00, 36.96it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:07<00:00, 41.91it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:07<00:00, 41.91it/s] 

    Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:07<00:00, 41.91it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:07<00:00, 41.91it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:07<00:00, 41.91it/s]Compiling num tokens (num_tokens=32):  79%|███████▉  | 46/58 [00:07<00:00, 41.91it/s]Compiling num tokens (num_tokens=28):  79%|███████▉  | 46/58 [00:07<00:00, 41.91it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:07<00:00, 46.41it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:07<00:00, 46.41it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:07<00:00, 46.41it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:07<00:00, 46.41it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:07<00:00, 46.41it/s]Compiling num tokens (num_tokens=8):  90%|████████▉ | 52/58 [00:07<00:00, 46.41it/s] Compiling num tokens (num_tokens=4):  90%|████████▉ | 52/58 [00:07<00:00, 46.41it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:07<00:00,  7.40it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=51.91 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=51.91 GB):   2%|▏         | 1/58 [00:00<00:38,  1.50it/s]Capturing num tokens (num_tokens=7680 avail_mem=51.88 GB):   2%|▏         | 1/58 [00:00<00:38,  1.50it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=51.88 GB):   3%|▎         | 2/58 [00:00<00:24,  2.26it/s]Capturing num tokens (num_tokens=7168 avail_mem=51.88 GB):   3%|▎         | 2/58 [00:00<00:24,  2.26it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=51.88 GB):   5%|▌         | 3/58 [00:01<00:19,  2.85it/s]Capturing num tokens (num_tokens=6656 avail_mem=51.88 GB):   5%|▌         | 3/58 [00:01<00:19,  2.85it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=51.88 GB):   7%|▋         | 4/58 [00:01<00:16,  3.37it/s]Capturing num tokens (num_tokens=6144 avail_mem=51.89 GB):   7%|▋         | 4/58 [00:01<00:16,  3.37it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=51.89 GB):   9%|▊         | 5/58 [00:01<00:13,  3.80it/s]Capturing num tokens (num_tokens=5632 avail_mem=51.89 GB):   9%|▊         | 5/58 [00:01<00:13,  3.80it/s]Capturing num tokens (num_tokens=5632 avail_mem=51.89 GB):  10%|█         | 6/58 [00:01<00:12,  4.29it/s]Capturing num tokens (num_tokens=5120 avail_mem=51.89 GB):  10%|█         | 6/58 [00:01<00:12,  4.29it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=51.89 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.76it/s]Capturing num tokens (num_tokens=4608 avail_mem=51.90 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.76it/s]Capturing num tokens (num_tokens=4608 avail_mem=51.90 GB):  14%|█▍        | 8/58 [00:02<00:09,  5.32it/s]Capturing num tokens (num_tokens=4096 avail_mem=51.90 GB):  14%|█▍        | 8/58 [00:02<00:09,  5.32it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=51.90 GB):  16%|█▌        | 9/58 [00:02<00:08,  5.84it/s]Capturing num tokens (num_tokens=3840 avail_mem=51.91 GB):  16%|█▌        | 9/58 [00:02<00:08,  5.84it/s]Capturing num tokens (num_tokens=3840 avail_mem=51.91 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.39it/s]Capturing num tokens (num_tokens=3584 avail_mem=51.90 GB):  17%|█▋        | 10/58 [00:02<00:07,  6.39it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=51.90 GB):  19%|█▉        | 11/58 [00:02<00:06,  6.98it/s]Capturing num tokens (num_tokens=3328 avail_mem=51.90 GB):  19%|█▉        | 11/58 [00:02<00:06,  6.98it/s]Capturing num tokens (num_tokens=3328 avail_mem=51.90 GB):  21%|██        | 12/58 [00:02<00:06,  7.62it/s]Capturing num tokens (num_tokens=3072 avail_mem=51.90 GB):  21%|██        | 12/58 [00:02<00:06,  7.62it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=51.90 GB):  21%|██        | 12/58 [00:02<00:06,  7.62it/s]Capturing num tokens (num_tokens=2816 avail_mem=51.90 GB):  24%|██▍       | 14/58 [00:02<00:04,  8.81it/s]Capturing num tokens (num_tokens=2560 avail_mem=51.90 GB):  24%|██▍       | 14/58 [00:02<00:04,  8.81it/s]Capturing num tokens (num_tokens=2304 avail_mem=51.90 GB):  24%|██▍       | 14/58 [00:02<00:04,  8.81it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=51.90 GB):  28%|██▊       | 16/58 [00:02<00:04, 10.12it/s]Capturing num tokens (num_tokens=2048 avail_mem=51.90 GB):  28%|██▊       | 16/58 [00:02<00:04, 10.12it/s]Capturing num tokens (num_tokens=1792 avail_mem=51.90 GB):  28%|██▊       | 16/58 [00:02<00:04, 10.12it/s]Capturing num tokens (num_tokens=1792 avail_mem=51.90 GB):  31%|███       | 18/58 [00:03<00:03, 11.73it/s]Capturing num tokens (num_tokens=1536 avail_mem=51.90 GB):  31%|███       | 18/58 [00:03<00:03, 11.73it/s]Capturing num tokens (num_tokens=1280 avail_mem=51.90 GB):  31%|███       | 18/58 [00:03<00:03, 11.73it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=51.90 GB):  31%|███       | 18/58 [00:03<00:03, 11.73it/s]Capturing num tokens (num_tokens=1024 avail_mem=51.90 GB):  36%|███▌      | 21/58 [00:03<00:02, 14.71it/s]Capturing num tokens (num_tokens=960 avail_mem=51.90 GB):  36%|███▌      | 21/58 [00:03<00:02, 14.71it/s] Capturing num tokens (num_tokens=896 avail_mem=51.89 GB):  36%|███▌      | 21/58 [00:03<00:02, 14.71it/s]Capturing num tokens (num_tokens=832 avail_mem=51.89 GB):  36%|███▌      | 21/58 [00:03<00:02, 14.71it/s]Capturing num tokens (num_tokens=832 avail_mem=51.89 GB):  41%|████▏     | 24/58 [00:03<00:01, 17.74it/s]Capturing num tokens (num_tokens=768 avail_mem=51.88 GB):  41%|████▏     | 24/58 [00:03<00:01, 17.74it/s]Capturing num tokens (num_tokens=704 avail_mem=51.88 GB):  41%|████▏     | 24/58 [00:03<00:01, 17.74it/s]

    Capturing num tokens (num_tokens=640 avail_mem=51.88 GB):  41%|████▏     | 24/58 [00:03<00:01, 17.74it/s]Capturing num tokens (num_tokens=576 avail_mem=51.87 GB):  41%|████▏     | 24/58 [00:03<00:01, 17.74it/s]Capturing num tokens (num_tokens=576 avail_mem=51.87 GB):  48%|████▊     | 28/58 [00:03<00:01, 21.28it/s]Capturing num tokens (num_tokens=512 avail_mem=51.77 GB):  48%|████▊     | 28/58 [00:03<00:01, 21.28it/s]Capturing num tokens (num_tokens=480 avail_mem=51.63 GB):  48%|████▊     | 28/58 [00:03<00:01, 21.28it/s]Capturing num tokens (num_tokens=448 avail_mem=51.49 GB):  48%|████▊     | 28/58 [00:03<00:01, 21.28it/s]Capturing num tokens (num_tokens=448 avail_mem=51.49 GB):  53%|█████▎    | 31/58 [00:03<00:01, 23.20it/s]Capturing num tokens (num_tokens=416 avail_mem=51.37 GB):  53%|█████▎    | 31/58 [00:03<00:01, 23.20it/s]

    Capturing num tokens (num_tokens=384 avail_mem=51.25 GB):  53%|█████▎    | 31/58 [00:03<00:01, 23.20it/s]Capturing num tokens (num_tokens=352 avail_mem=51.11 GB):  53%|█████▎    | 31/58 [00:03<00:01, 23.20it/s]Capturing num tokens (num_tokens=320 avail_mem=50.99 GB):  53%|█████▎    | 31/58 [00:03<00:01, 23.20it/s]Capturing num tokens (num_tokens=320 avail_mem=50.99 GB):  60%|██████    | 35/58 [00:03<00:00, 26.08it/s]Capturing num tokens (num_tokens=288 avail_mem=50.87 GB):  60%|██████    | 35/58 [00:03<00:00, 26.08it/s]Capturing num tokens (num_tokens=256 avail_mem=50.75 GB):  60%|██████    | 35/58 [00:03<00:00, 26.08it/s]Capturing num tokens (num_tokens=240 avail_mem=50.62 GB):  60%|██████    | 35/58 [00:03<00:00, 26.08it/s]Capturing num tokens (num_tokens=224 avail_mem=50.53 GB):  60%|██████    | 35/58 [00:03<00:00, 26.08it/s]

    Capturing num tokens (num_tokens=224 avail_mem=50.53 GB):  67%|██████▋   | 39/58 [00:03<00:00, 28.90it/s]Capturing num tokens (num_tokens=208 avail_mem=50.52 GB):  67%|██████▋   | 39/58 [00:03<00:00, 28.90it/s]Capturing num tokens (num_tokens=192 avail_mem=50.52 GB):  67%|██████▋   | 39/58 [00:03<00:00, 28.90it/s]Capturing num tokens (num_tokens=176 avail_mem=50.51 GB):  67%|██████▋   | 39/58 [00:03<00:00, 28.90it/s]Capturing num tokens (num_tokens=160 avail_mem=50.51 GB):  67%|██████▋   | 39/58 [00:03<00:00, 28.90it/s]Capturing num tokens (num_tokens=160 avail_mem=50.51 GB):  74%|███████▍  | 43/58 [00:03<00:00, 31.24it/s]Capturing num tokens (num_tokens=144 avail_mem=50.48 GB):  74%|███████▍  | 43/58 [00:03<00:00, 31.24it/s]Capturing num tokens (num_tokens=128 avail_mem=50.49 GB):  74%|███████▍  | 43/58 [00:03<00:00, 31.24it/s]Capturing num tokens (num_tokens=112 avail_mem=50.40 GB):  74%|███████▍  | 43/58 [00:03<00:00, 31.24it/s]Capturing num tokens (num_tokens=96 avail_mem=50.39 GB):  74%|███████▍  | 43/58 [00:03<00:00, 31.24it/s] 

    Capturing num tokens (num_tokens=96 avail_mem=50.39 GB):  81%|████████  | 47/58 [00:03<00:00, 33.10it/s]Capturing num tokens (num_tokens=80 avail_mem=50.39 GB):  81%|████████  | 47/58 [00:03<00:00, 33.10it/s]Capturing num tokens (num_tokens=64 avail_mem=50.35 GB):  81%|████████  | 47/58 [00:03<00:00, 33.10it/s]Capturing num tokens (num_tokens=48 avail_mem=49.80 GB):  81%|████████  | 47/58 [00:04<00:00, 33.10it/s]Capturing num tokens (num_tokens=32 avail_mem=48.56 GB):  81%|████████  | 47/58 [00:04<00:00, 33.10it/s]Capturing num tokens (num_tokens=32 avail_mem=48.56 GB):  88%|████████▊ | 51/58 [00:04<00:00, 33.30it/s]Capturing num tokens (num_tokens=28 avail_mem=47.95 GB):  88%|████████▊ | 51/58 [00:04<00:00, 33.30it/s]Capturing num tokens (num_tokens=24 avail_mem=47.95 GB):  88%|████████▊ | 51/58 [00:04<00:00, 33.30it/s]Capturing num tokens (num_tokens=20 avail_mem=47.95 GB):  88%|████████▊ | 51/58 [00:04<00:00, 33.30it/s]

    Capturing num tokens (num_tokens=16 avail_mem=47.94 GB):  88%|████████▊ | 51/58 [00:04<00:00, 33.30it/s]Capturing num tokens (num_tokens=16 avail_mem=47.94 GB):  95%|█████████▍| 55/58 [00:04<00:00, 29.62it/s]Capturing num tokens (num_tokens=12 avail_mem=47.94 GB):  95%|█████████▍| 55/58 [00:04<00:00, 29.62it/s]Capturing num tokens (num_tokens=8 avail_mem=47.93 GB):  95%|█████████▍| 55/58 [00:04<00:00, 29.62it/s] Capturing num tokens (num_tokens=4 avail_mem=47.93 GB):  95%|█████████▍| 55/58 [00:04<00:00, 29.62it/s]

    Capturing num tokens (num_tokens=4 avail_mem=47.93 GB): 100%|██████████| 58/58 [00:04<00:00, 13.08it/s]


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


<strong style='color: #00008B;'>reasoing_content: Okay, so the user is in New York and wants to know the current date and time along with the weather. I need to figure out how to get this information using the functions provided. <br><br>First, I should use the 'get_current_date' function. The parameters required are the timezone. Since the user is in New York, the timezone would be 'America/New_York'. I'll structure the function call with timezone set to that value.<br><br>Next, I need the weather information. For that, I'll use the 'get_current_weather' function. The parameters needed are the city, state, and unit. The city is 'New York', the state is 'NY', and the unit should be 'fahrenheit' since the user didn't specify otherwise.<br><br>I have to make sure each function call is separate and follows the correct format. So I'll first call get_current_date with the timezone parameter, then call get_current_weather with city, state, and unit as parameters. <br><br>I should also remember to include the sources in the response, so I'll mention the functions used and their documentation links. <br><br>Putting it all together, I'll format the response as two separate function calls, each on its own line, with the appropriate parameters and tags.<br><br><br>content: <function=get_current_date>{"timezone": "America/New_York)}</function> <function=get_current_weather>{"  <br>  }</function></strong>


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

    {'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down. First, I need to identify what the capital of France is. I know that Paris is the capital, so that\'s straightforward. \n\nNext, I need to find the population. I remember that Paris is a major city, so its population is quite large. I think it\'s over 3 million, but I\'m not exactly sure of the exact number. Maybe I should double-check that. \n\nWait, I recall that the population figure can vary depending on the source and the year. The user didn\'t specify a particular year, so I should probably go with the most recent estimate. I believe the population is around 3,500,000 as of 2023. \n\nNow, I need to structure this information into a JSON format. JSON typically uses key-value pairs, so I\'ll create an object with keys like "city", "population", and maybe "country" since the user mentioned France. \n\nI should make sure the keys are in English to keep it clear. The city is Paris, the population is 3,500,000, and the country is France. I\'ll format this into a JSON object, ensuring proper syntax with commas and quotation marks. \n\nI also need to present this in a way that\'s easy to read, so I\'ll put each key on a new line. That way, the user can quickly see the information without confusion. \n\nI wonder if the user needs more details, like the exact current population or additional statistics. But since they only asked for the capital and population, I\'ll stick to that. \n\nLastly, I\'ll make sure the JSON is valid by checking the syntax. No trailing commas, proper use of braces, and correct quotation marks. That should cover everything the user needs.\n</think>{\n  "name": "Paris",\n  "population": 3500000\n}', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 13, 5512, 11, 358, 1184, 311, 10542, 1128, 279, 6722, 315, 9625, 374, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4710, 5847, 11, 358, 1184, 311, 1477, 279, 7042, 13, 358, 6099, 429, 12095, 374, 264, 3598, 3283, 11, 773, 1181, 7042, 374, 5008, 3460, 13, 358, 1744, 432, 594, 916, 220, 18, 3526, 11, 714, 358, 2776, 537, 6896, 2704, 315, 279, 4734, 1372, 13, 10696, 358, 1265, 1990, 15934, 429, 13, 4710, 14190, 11, 358, 19091, 429, 279, 7042, 7071, 646, 13289, 11649, 389, 279, 2530, 323, 279, 1042, 13, 576, 1196, 3207, 944, 13837, 264, 3953, 1042, 11, 773, 358, 1265, 4658, 728, 448, 279, 1429, 3213, 16045, 13, 358, 4411, 279, 7042, 374, 2163, 220, 18, 11, 20, 15, 15, 11, 15, 15, 15, 438, 315, 220, 17, 15, 17, 18, 13, 4710, 7039, 11, 358, 1184, 311, 5944, 419, 1995, 1119, 264, 4718, 3561, 13, 4718, 11136, 5711, 1376, 19083, 13530, 11, 773, 358, 3278, 1855, 458, 1633, 448, 6894, 1075, 330, 8926, 497, 330, 44441, 497, 323, 7196, 330, 11141, 1, 2474, 279, 1196, 9733, 9625, 13, 4710, 40, 1265, 1281, 2704, 279, 6894, 525, 304, 6364, 311, 2506, 432, 2797, 13, 576, 3283, 374, 12095, 11, 279, 7042, 374, 220, 18, 11, 20, 15, 15, 11, 15, 15, 15, 11, 323, 279, 3146, 374, 9625, 13, 358, 3278, 3561, 419, 1119, 264, 4718, 1633, 11, 22573, 6169, 19482, 448, 76602, 323, 54231, 15423, 13, 4710, 40, 1083, 1184, 311, 3042, 419, 304, 264, 1616, 429, 594, 4135, 311, 1349, 11, 773, 358, 3278, 2182, 1817, 1376, 389, 264, 501, 1555, 13, 2938, 1616, 11, 279, 1196, 646, 6157, 1490, 279, 1995, 2041, 21340, 13, 4710, 40, 5775, 421, 279, 1196, 3880, 803, 3565, 11, 1075, 279, 4734, 1482, 7042, 476, 5107, 13142, 13, 1988, 2474, 807, 1172, 4588, 369, 279, 6722, 323, 7042, 11, 358, 3278, 9214, 311, 429, 13, 4710, 80486, 11, 358, 3278, 1281, 2704, 279, 4718, 374, 2697, 553, 13295, 279, 19482, 13, 2308, 27748, 76602, 11, 6169, 990, 315, 59191, 11, 323, 4396, 54231, 15423, 13, 2938, 1265, 3421, 4297, 279, 1196, 3880, 624, 151649, 515, 220, 330, 606, 788, 330, 59604, 756, 220, 330, 44441, 788, 220, 18, 20, 15, 15, 15, 15, 15, 198, 92, 151643], 'meta_info': {'id': '25d11712f6e8437b804a4c35786fa7fc', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 412, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 2.9501306950114667, 'response_sent_to_client_ts': 1775007914.1430292}}



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


<strong style='color: #00008B;'>{'text': 'Okay, so the user is asking for the information and population of the capital of France in JSON format. Let me break this down.\n\nFirst, I need to identify the capital of France. I know that Paris is the capital, so that\'s straightforward. Now, I should find the most recent population data. I remember that the population of Paris has been growing, but I\'m not sure of the exact number. I think it\'s around 2 million, but I should verify that.\n\nI\'ll check a reliable source, maybe the official Paris Municipality website or a recent census. Let me see... Yes, according to the latest data, the population is approximately 2,174,300 as of 2023. That seems accurate.\n\nNext, I need to structure this information into a JSON format. JSON requires key-value pairs, so I\'ll create an object with keys like "city", "population", and "country". The city is Paris, the population is the number I found, and the country is France.\n\nI should make sure the JSON syntax is correct. Each key should be in quotes, and the values as well. The entire structure should be enclosed in curly braces. I\'ll format it properly to avoid any syntax errors.\n\nPutting it all together, the JSON object will have the city, population, and country. I\'ll present this to the user, making sure it\'s clear and easy to understand. I don\'t think the user needs anything more detailed, so this should suffice.\n\nI should also consider if the user might need additional information, like the area or age distribution, but since they only asked for population, I\'ll stick to that. Maybe they\'ll ask for more details later, but for now, this response should be helpful.\n</think>{\n  "name": "Paris",\n  "population": 2174300\n}', 'output_ids': [32313, 11, 773, 279, 1196, 374, 10161, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 6771, 752, 1438, 419, 1495, 382, 5338, 11, 358, 1184, 311, 10542, 279, 6722, 315, 9625, 13, 358, 1414, 429, 12095, 374, 279, 6722, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1265, 1477, 279, 1429, 3213, 7042, 821, 13, 358, 6099, 429, 279, 7042, 315, 12095, 702, 1012, 7826, 11, 714, 358, 2776, 537, 2704, 315, 279, 4734, 1372, 13, 358, 1744, 432, 594, 2163, 220, 17, 3526, 11, 714, 358, 1265, 10146, 429, 382, 40, 3278, 1779, 264, 14720, 2530, 11, 7196, 279, 3946, 12095, 35703, 2719, 3910, 476, 264, 3213, 43602, 13, 6771, 752, 1490, 1112, 7414, 11, 4092, 311, 279, 5535, 821, 11, 279, 7042, 374, 13187, 220, 17, 11, 16, 22, 19, 11, 18, 15, 15, 438, 315, 220, 17, 15, 17, 18, 13, 2938, 4977, 13382, 382, 5847, 11, 358, 1184, 311, 5944, 419, 1995, 1119, 264, 4718, 3561, 13, 4718, 7460, 1376, 19083, 13530, 11, 773, 358, 3278, 1855, 458, 1633, 448, 6894, 1075, 330, 8926, 497, 330, 44441, 497, 323, 330, 11141, 3263, 576, 3283, 374, 12095, 11, 279, 7042, 374, 279, 1372, 358, 1730, 11, 323, 279, 3146, 374, 9625, 382, 40, 1265, 1281, 2704, 279, 4718, 19482, 374, 4396, 13, 8886, 1376, 1265, 387, 304, 17194, 11, 323, 279, 2750, 438, 1632, 13, 576, 4453, 5944, 1265, 387, 43810, 304, 68103, 59191, 13, 358, 3278, 3561, 432, 10277, 311, 5648, 894, 19482, 5975, 382, 97904, 432, 678, 3786, 11, 279, 4718, 1633, 686, 614, 279, 3283, 11, 7042, 11, 323, 3146, 13, 358, 3278, 3042, 419, 311, 279, 1196, 11, 3259, 2704, 432, 594, 2797, 323, 4135, 311, 3535, 13, 358, 1513, 944, 1744, 279, 1196, 3880, 4113, 803, 11682, 11, 773, 419, 1265, 76156, 382, 40, 1265, 1083, 2908, 421, 279, 1196, 2578, 1184, 5107, 1995, 11, 1075, 279, 3082, 476, 4231, 7982, 11, 714, 2474, 807, 1172, 4588, 369, 7042, 11, 358, 3278, 9214, 311, 429, 13, 10696, 807, 3278, 2548, 369, 803, 3565, 2937, 11, 714, 369, 1431, 11, 419, 2033, 1265, 387, 10950, 624, 151649, 515, 220, 330, 606, 788, 330, 59604, 756, 220, 330, 44441, 788, 220, 17, 16, 22, 19, 18, 15, 15, 198, 92, 151643], 'meta_info': {'id': '957cd7541e964040a9051f94ecfd7eec', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 384, 'cached_tokens': 22, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 3.461340468027629, 'response_sent_to_client_ts': 1775007917.6170692}}</strong>


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

    [{'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '3b59157630a44210ad9c5433c2aa733c', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.15223284997045994, 'response_sent_to_client_ts': 1775007917.812824}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': 'ef1cdfa405e2402aa3453c6d859273a0', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.15214358898811042, 'response_sent_to_client_ts': 1775007917.8128393}}, {'text': 'Berlin is the capital of France', 'output_ids': [3430, 81, 742, 77, 374, 279, 6722, 315, 9625, 151643], 'meta_info': {'id': '4b57e92ba076485b905123f97c086f89', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 11, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 10, 'cached_tokens': 10, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.152104840031825, 'response_sent_to_client_ts': 1775007917.8128445}}]


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

    {'text': ' France, and the \n\\( n \\)  \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\( l \\) \\( m \\) \\( k \\) \\(', 'output_ids': [9625, 11, 323, 279, 220, 198, 44292, 308, 1124, 8, 220, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767, 326, 1124, 8, 17767, 296, 1124, 8, 17767, 595, 1124, 8, 17767], 'meta_info': {'id': 'f0b6532cde234a78af6af21a305f3d2c', 'finish_reason': {'type': 'length', 'length': 2048}, 'prompt_tokens': 6, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 2048, 'cached_tokens': 1, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 20.626259958022274, 'response_sent_to_client_ts': 1775007938.4463048}}


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


<strong style='color: #00008B;'>{'text': 'Alright, so the user just asked for the information and population of the capital of France in JSON format. Hmm, okay, first off, I should figure out what exactly they\'re asking for. They want a JSON response, which is a data structure commonly used for data interchange. So I need to make sure I collect all the relevant information correctly.\n\nThe capital of France is Paris, so that\'s straightforward. Now, I need to find out the latest population figures. I remember that population statistics can change, so I should verify the current numbers. From what I recall, as of recent years, Paris has been growing, but it\'s a big city, so there might be some estimates involved.\n\nI\'ll look up the official sources for the most accurate data. The latest figure, I think, is around 2.2 million people. I should double-check the sources to ensure reliability, maybe the official website of the INSEE department or recent census reports. That would add credibility to the response.\n\nIncluding some additional details could be helpful, like the area of the capital and its administrative region. Paris\'s administrative region is Île-de-France metropolitan, which covers more than just Paris itself but includes surrounding areas. Knowing the metropolitan area population might give a broader context.\n\nThe geographical coordinates are also important for any mapping applications. So I\'ll include the latitude and longitude of Paris. Demographics-wise, I should note some common age groups to give a snapshot of the population structure.\n\nI also wonder about the time zone of Paris. It\'s in Central European Time, but not sure if the user needs that. Maybe including it could be nice, but it\'s optional unless specified.\n\nLooking at the structure, the JSON should be clean and well-formatted. Using proper syntax with keys and commas is essential to avoid errors. I should organize it clearly, maybe under a "capital" key with various sub-keys like "name," "population," etc.\n\nWait, did I get all the numbers right? Let me confirm the population figure again. A quick search shows that as of recent data, Paris has a population of approximately 2,155,000. Okay, so I can adjust the number slightly for accuracy.\n\nAlso, I should consider presenting the population as an approximate figure since exact numbers can vary between sources and years. Including units (like "thousand") can make it clearer for the reader.\n\nI\'m thinking about the response structure. Should I group some details together? Maybe have a "general" section with city name, area description, coordinates, and administrative region. Then, under "demographics," include population, age groups, and language stats.\n\nAdding an "about" key under "general" might help, providing a brief overview of Paris\'s significance. This way, the JSON is comprehensive and useful for someone looking for detailed information.\n\nI should make sure not to make any errors in the JSON syntax. Missing commas or incorrect nesting could cause issues when someone tries to parse the data. Testing would be a good idea, but since I\'m just drafting this, I have to be extra careful.\n\nFinally, I\'ll compile all these points into a structured JSON format, ensuring clarity and accuracy. The user likely needs this information for research, a project, or maybe just out of curiosity about Paris\'s demographics. Providing precise and up-to-date data is crucial to meet their needs effectively.\n</think>\n\nHere is the information and population of the capital of France (Paris) in JSON format:\n\n```json\n{\n  "capital": {\n    "name": "Paris",\n    "general": {\n      "capital": "Yes",\n      "region": "Île-de-France metropolitan area",\n      "coordinates": {\n        "latitude": 48.8566,\n        "longitude": 2.3522\n      },\n      "area": "Greater Paris area is about 1275 square kilometers",\n      "administrativeRegion": " Île-de-France, Île-de-France_fix"\n    },\n    "population": 2176001,\n    "year": 2023,\n    "demographics": {\n      "ageGroups": {\n        "under5": 198400,\n        "5-14": 450000,\n        "15-24": 410000,\n        "25-34": 390000,\n        "35-44": 390000,\n        "45-54": 350000,\n        "55-64": 300000,\n        "65+": 176600\n      },\n      "origin": "()].rox"\n    },\n    "about": "Paris is the capital city of France and a major center of art, culture, and government. It is located in the Île-de-France region."\n  }\n}\n```\n\nThis JSON includes the population of Paris (approximately 2,176,001 as of 2023), along with general information and demographics about the city.', 'output_ids': [71486, 11, 773, 279, 1196, 1101, 4588, 369, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 304, 4718, 3561, 13, 88190, 11, 16910, 11, 1156, 1007, 11, 358, 1265, 7071, 700, 1128, 6896, 807, 2299, 10161, 369, 13, 2379, 1366, 264, 4718, 2033, 11, 892, 374, 264, 821, 5944, 16626, 1483, 369, 821, 51263, 13, 2055, 358, 1184, 311, 1281, 2704, 358, 6530, 678, 279, 9760, 1995, 12440, 382, 785, 6722, 315, 9625, 374, 12095, 11, 773, 429, 594, 30339, 13, 4695, 11, 358, 1184, 311, 1477, 700, 279, 5535, 7042, 12396, 13, 358, 6099, 429, 7042, 13142, 646, 2297, 11, 773, 358, 1265, 10146, 279, 1482, 5109, 13, 5542, 1128, 358, 19091, 11, 438, 315, 3213, 1635, 11, 12095, 702, 1012, 7826, 11, 714, 432, 594, 264, 2409, 3283, 11, 773, 1052, 2578, 387, 1045, 17530, 6398, 382, 40, 3278, 1401, 705, 279, 3946, 8173, 369, 279, 1429, 13382, 821, 13, 576, 5535, 7071, 11, 358, 1744, 11, 374, 2163, 220, 17, 13, 17, 3526, 1251, 13, 358, 1265, 1990, 15934, 279, 8173, 311, 5978, 30538, 11, 7196, 279, 3946, 3910, 315, 279, 1964, 48740, 9292, 476, 3213, 43602, 6682, 13, 2938, 1035, 912, 37669, 311, 279, 2033, 382, 83449, 1045, 5107, 3565, 1410, 387, 10950, 11, 1075, 279, 3082, 315, 279, 6722, 323, 1181, 22707, 5537, 13, 12095, 594, 22707, 5537, 374, 59108, 273, 6810, 7276, 34106, 57406, 11, 892, 14521, 803, 1091, 1101, 12095, 5086, 714, 5646, 14590, 5671, 13, 57633, 279, 57406, 3082, 7042, 2578, 2968, 264, 26829, 2266, 382, 785, 52901, 13934, 525, 1083, 2989, 369, 894, 12731, 8357, 13, 2055, 358, 3278, 2924, 279, 20849, 323, 20515, 315, 12095, 13, 4724, 44145, 44439, 11, 358, 1265, 5185, 1045, 4185, 4231, 5203, 311, 2968, 264, 16295, 315, 279, 7042, 5944, 382, 40, 1083, 5775, 911, 279, 882, 10143, 315, 12095, 13, 1084, 594, 304, 10684, 7513, 4120, 11, 714, 537, 2704, 421, 279, 1196, 3880, 429, 13, 10696, 2670, 432, 1410, 387, 6419, 11, 714, 432, 594, 10101, 7241, 5189, 382, 22464, 518, 279, 5944, 11, 279, 4718, 1265, 387, 4240, 323, 1632, 8460, 12127, 13, 12091, 6169, 19482, 448, 6894, 323, 76602, 374, 7565, 311, 5648, 5975, 13, 358, 1265, 30235, 432, 9355, 11, 7196, 1212, 264, 330, 65063, 1, 1376, 448, 5257, 1186, 76109, 1075, 330, 606, 1335, 330, 44441, 1335, 4992, 382, 14190, 11, 1521, 358, 633, 678, 279, 5109, 1290, 30, 6771, 752, 7683, 279, 7042, 7071, 1549, 13, 362, 3974, 2711, 4933, 429, 438, 315, 3213, 821, 11, 12095, 702, 264, 7042, 315, 13187, 220, 17, 11, 16, 20, 20, 11, 15, 15, 15, 13, 35439, 11, 773, 358, 646, 7500, 279, 1372, 10078, 369, 13403, 382, 13394, 11, 358, 1265, 2908, 31544, 279, 7042, 438, 458, 44868, 7071, 2474, 4734, 5109, 646, 13289, 1948, 8173, 323, 1635, 13, 55121, 8153, 320, 4803, 330, 339, 51849, 899, 646, 1281, 432, 48379, 369, 279, 6604, 382, 40, 2776, 7274, 911, 279, 2033, 5944, 13, 12260, 358, 1874, 1045, 3565, 3786, 30, 10696, 614, 264, 330, 24595, 1, 3772, 448, 3283, 829, 11, 3082, 4008, 11, 13934, 11, 323, 22707, 5537, 13, 5005, 11, 1212, 330, 27431, 44145, 1335, 2924, 7042, 11, 4231, 5203, 11, 323, 4128, 10472, 382, 32308, 458, 330, 9096, 1, 1376, 1212, 330, 24595, 1, 2578, 1492, 11, 8241, 264, 9814, 23251, 315, 12095, 594, 25361, 13, 1096, 1616, 11, 279, 4718, 374, 15817, 323, 5390, 369, 4325, 3330, 369, 11682, 1995, 382, 40, 1265, 1281, 2704, 537, 311, 1281, 894, 5975, 304, 279, 4718, 19482, 13, 35264, 76602, 476, 15114, 66710, 1410, 5240, 4714, 979, 4325, 16297, 311, 4715, 279, 821, 13, 26768, 1035, 387, 264, 1661, 4522, 11, 714, 2474, 358, 2776, 1101, 57900, 419, 11, 358, 614, 311, 387, 4960, 16585, 382, 23949, 11, 358, 3278, 19192, 678, 1493, 3501, 1119, 264, 32930, 4718, 3561, 11, 22573, 31273, 323, 13403, 13, 576, 1196, 4363, 3880, 419, 1995, 369, 3412, 11, 264, 2390, 11, 476, 7196, 1101, 700, 315, 40228, 911, 12095, 594, 62234, 13, 80100, 23560, 323, 705, 4686, 18413, 821, 374, 16587, 311, 3367, 862, 3880, 13444, 624, 151649, 271, 8420, 374, 279, 1995, 323, 7042, 315, 279, 6722, 315, 9625, 320, 59604, 8, 304, 4718, 3561, 1447, 73594, 2236, 198, 515, 220, 330, 65063, 788, 341, 262, 330, 606, 788, 330, 59604, 756, 262, 330, 24595, 788, 341, 414, 330, 65063, 788, 330, 9454, 756, 414, 330, 3943, 788, 330, 71807, 273, 6810, 7276, 34106, 57406, 3082, 756, 414, 330, 34739, 788, 341, 286, 330, 23718, 788, 220, 19, 23, 13, 23, 20, 21, 21, 345, 286, 330, 25446, 788, 220, 17, 13, 18, 20, 17, 17, 198, 414, 1153, 414, 330, 4798, 788, 330, 41366, 12095, 3082, 374, 911, 220, 16, 17, 22, 20, 9334, 40568, 756, 414, 330, 68849, 1388, 14091, 788, 330, 59108, 273, 6810, 7276, 34106, 11, 59108, 273, 6810, 7276, 34106, 36060, 698, 262, 1153, 262, 330, 44441, 788, 220, 17, 16, 22, 21, 15, 15, 16, 345, 262, 330, 3157, 788, 220, 17, 15, 17, 18, 345, 262, 330, 27431, 44145, 788, 341, 414, 330, 424, 22173, 788, 341, 286, 330, 7995, 20, 788, 220, 16, 24, 23, 19, 15, 15, 345, 286, 330, 20, 12, 16, 19, 788, 220, 19, 20, 15, 15, 15, 15, 345, 286, 330, 16, 20, 12, 17, 19, 788, 220, 19, 16, 15, 15, 15, 15, 345, 286, 330, 17, 20, 12, 18, 19, 788, 220, 18, 24, 15, 15, 15, 15, 345, 286, 330, 18, 20, 12, 19, 19, 788, 220, 18, 24, 15, 15, 15, 15, 345, 286, 330, 19, 20, 12, 20, 19, 788, 220, 18, 20, 15, 15, 15, 15, 345, 286, 330, 20, 20, 12, 21, 19, 788, 220, 18, 15, 15, 15, 15, 15, 345, 286, 330, 21, 20, 80479, 220, 16, 22, 21, 21, 15, 15, 198, 414, 1153, 414, 330, 8611, 788, 330, 57590, 54789, 698, 262, 1153, 262, 330, 9096, 788, 330, 59604, 374, 279, 6722, 3283, 315, 9625, 323, 264, 3598, 4126, 315, 1947, 11, 7674, 11, 323, 3033, 13, 1084, 374, 7407, 304, 279, 59108, 273, 6810, 7276, 34106, 5537, 10040, 220, 456, 532, 13874, 19324, 1986, 4718, 5646, 279, 7042, 315, 12095, 320, 96736, 220, 17, 11, 16, 22, 21, 11, 15, 15, 16, 438, 315, 220, 17, 15, 17, 18, 701, 3156, 448, 4586, 1995, 323, 62234, 911, 279, 3283, 13, 151643], 'meta_info': {'id': '2da714f33a17412384d47ffac76c4e7d', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 23, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 1065, 'cached_tokens': 22, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 10.388806462986395, 'response_sent_to_client_ts': 1775007948.843917}}</strong>



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


    2026-04-01 01:46:00.724 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 01:46:00] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 01:46:00.724 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 01:46:00] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 01:46:00.724 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 01:46:00] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 01:46:00.724 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 01:46:00] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 01:46:00.724 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 01:46:00] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:01<00:01,  1.17s/it]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.51s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.46s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:55,  3.08s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:55,  3.08s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:24,  1.51s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:24,  1.51s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:50,  1.10it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:50,  1.10it/s]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:33,  1.61it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:33,  1.61it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:24,  2.20it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:24,  2.20it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.84it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.84it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.57it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.57it/s]Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.34it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.34it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.21it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.21it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.21it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:07,  6.32it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:07,  6.32it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:05<00:08,  5.74it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:05<00:08,  5.74it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:05<00:08,  5.17it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:05<00:08,  5.17it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:05<00:09,  4.79it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:05<00:09,  4.79it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:05<00:09,  4.75it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:05<00:09,  4.75it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:05<00:08,  4.91it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:05<00:08,  4.91it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:06<00:07,  5.19it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:06<00:07,  5.19it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:06<00:07,  5.66it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:06<00:07,  5.66it/s]

    Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:06<00:06,  6.06it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:06<00:06,  6.06it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:06<00:05,  6.52it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:06<00:05,  6.52it/s]

    Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:06<00:05,  7.09it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:06<00:05,  7.09it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:06<00:05,  7.09it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:06<00:03,  9.03it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:06<00:03,  9.03it/s]

    Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:06<00:03,  9.03it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:06<00:03, 10.05it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:06<00:03, 10.05it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:06<00:03, 10.05it/s]

    Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:07<00:02, 11.41it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:07<00:02, 11.41it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:07<00:02, 11.41it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:07<00:02, 13.15it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:07<00:02, 13.15it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:07<00:02, 13.15it/s]

    Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:07<00:01, 14.58it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:07<00:01, 14.58it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:07<00:01, 14.58it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:07<00:01, 14.82it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:07<00:01, 14.82it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:07<00:01, 14.82it/s]

    Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:07<00:01, 14.82it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:07<00:01, 16.83it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:07<00:01, 16.83it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:07<00:01, 16.83it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:07<00:01, 16.83it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:07<00:01, 18.53it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:07<00:01, 18.53it/s]

    Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:07<00:01, 18.53it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:07<00:01, 18.53it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:07<00:00, 19.75it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:07<00:00, 19.75it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:07<00:00, 19.75it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:07<00:00, 19.76it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:07<00:00, 19.76it/s]

    Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:07<00:00, 19.76it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:07<00:00, 19.76it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:08<00:00, 21.03it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:08<00:00, 21.03it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:08<00:00, 21.03it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:08<00:00, 21.03it/s]

    Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:08<00:00, 21.61it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:08<00:00, 21.61it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:08<00:00, 21.61it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:08<00:00, 21.61it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:08<00:00, 22.56it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:08<00:00, 22.56it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:08<00:00, 22.56it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:08<00:00, 22.56it/s]

    Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:08<00:00, 22.56it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:08<00:00, 25.65it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:08<00:00, 25.65it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:08<00:00,  6.89it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=38.60 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=38.60 GB):   2%|▏         | 1/58 [00:00<00:48,  1.16it/s]Capturing num tokens (num_tokens=7680 avail_mem=38.17 GB):   2%|▏         | 1/58 [00:00<00:48,  1.16it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=38.17 GB):   3%|▎         | 2/58 [00:01<00:39,  1.42it/s]Capturing num tokens (num_tokens=7168 avail_mem=38.57 GB):   3%|▎         | 2/58 [00:01<00:39,  1.42it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=38.57 GB):   5%|▌         | 3/58 [00:01<00:34,  1.60it/s]Capturing num tokens (num_tokens=6656 avail_mem=38.29 GB):   5%|▌         | 3/58 [00:01<00:34,  1.60it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=38.29 GB):   7%|▋         | 4/58 [00:02<00:29,  1.86it/s]Capturing num tokens (num_tokens=6144 avail_mem=38.57 GB):   7%|▋         | 4/58 [00:02<00:29,  1.86it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=38.57 GB):   9%|▊         | 5/58 [00:02<00:25,  2.11it/s]Capturing num tokens (num_tokens=5632 avail_mem=38.56 GB):   9%|▊         | 5/58 [00:02<00:25,  2.11it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=38.56 GB):  10%|█         | 6/58 [00:03<00:21,  2.40it/s]Capturing num tokens (num_tokens=5120 avail_mem=38.56 GB):  10%|█         | 6/58 [00:03<00:21,  2.40it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=38.56 GB):  12%|█▏        | 7/58 [00:03<00:18,  2.82it/s]Capturing num tokens (num_tokens=4608 avail_mem=38.50 GB):  12%|█▏        | 7/58 [00:03<00:18,  2.82it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=38.50 GB):  14%|█▍        | 8/58 [00:03<00:19,  2.60it/s]Capturing num tokens (num_tokens=4096 avail_mem=37.47 GB):  14%|█▍        | 8/58 [00:03<00:19,  2.60it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=37.47 GB):  16%|█▌        | 9/58 [00:03<00:16,  3.02it/s]Capturing num tokens (num_tokens=3840 avail_mem=38.49 GB):  16%|█▌        | 9/58 [00:03<00:16,  3.02it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=38.49 GB):  17%|█▋        | 10/58 [00:04<00:16,  2.88it/s]Capturing num tokens (num_tokens=3584 avail_mem=37.50 GB):  17%|█▋        | 10/58 [00:04<00:16,  2.88it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=37.50 GB):  19%|█▉        | 11/58 [00:04<00:15,  3.01it/s]Capturing num tokens (num_tokens=3328 avail_mem=37.57 GB):  19%|█▉        | 11/58 [00:04<00:15,  3.01it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=37.57 GB):  21%|██        | 12/58 [00:04<00:14,  3.14it/s]Capturing num tokens (num_tokens=3072 avail_mem=37.66 GB):  21%|██        | 12/58 [00:04<00:14,  3.14it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=37.66 GB):  22%|██▏       | 13/58 [00:05<00:13,  3.33it/s]Capturing num tokens (num_tokens=2816 avail_mem=37.63 GB):  22%|██▏       | 13/58 [00:05<00:13,  3.33it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=37.63 GB):  24%|██▍       | 14/58 [00:05<00:12,  3.56it/s]Capturing num tokens (num_tokens=2560 avail_mem=38.49 GB):  24%|██▍       | 14/58 [00:05<00:12,  3.56it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=38.49 GB):  26%|██▌       | 15/58 [00:05<00:11,  3.84it/s]Capturing num tokens (num_tokens=2304 avail_mem=37.69 GB):  26%|██▌       | 15/58 [00:05<00:11,  3.84it/s]Capturing num tokens (num_tokens=2304 avail_mem=37.69 GB):  28%|██▊       | 16/58 [00:05<00:10,  4.15it/s]Capturing num tokens (num_tokens=2048 avail_mem=38.48 GB):  28%|██▊       | 16/58 [00:05<00:10,  4.15it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=38.48 GB):  29%|██▉       | 17/58 [00:06<00:09,  4.19it/s]Capturing num tokens (num_tokens=1792 avail_mem=37.76 GB):  29%|██▉       | 17/58 [00:06<00:09,  4.19it/s]Capturing num tokens (num_tokens=1792 avail_mem=37.76 GB):  31%|███       | 18/58 [00:06<00:08,  4.74it/s]Capturing num tokens (num_tokens=1536 avail_mem=37.81 GB):  31%|███       | 18/58 [00:06<00:08,  4.74it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=37.81 GB):  33%|███▎      | 19/58 [00:06<00:07,  4.92it/s]Capturing num tokens (num_tokens=1280 avail_mem=37.81 GB):  33%|███▎      | 19/58 [00:06<00:07,  4.92it/s]Capturing num tokens (num_tokens=1280 avail_mem=37.81 GB):  34%|███▍      | 20/58 [00:06<00:06,  5.68it/s]Capturing num tokens (num_tokens=1024 avail_mem=37.88 GB):  34%|███▍      | 20/58 [00:06<00:06,  5.68it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=37.88 GB):  36%|███▌      | 21/58 [00:06<00:06,  6.08it/s]Capturing num tokens (num_tokens=960 avail_mem=38.47 GB):  36%|███▌      | 21/58 [00:06<00:06,  6.08it/s] Capturing num tokens (num_tokens=896 avail_mem=37.93 GB):  36%|███▌      | 21/58 [00:06<00:06,  6.08it/s]

    Capturing num tokens (num_tokens=896 avail_mem=37.93 GB):  40%|███▉      | 23/58 [00:06<00:04,  7.15it/s]Capturing num tokens (num_tokens=832 avail_mem=38.46 GB):  40%|███▉      | 23/58 [00:06<00:04,  7.15it/s]Capturing num tokens (num_tokens=768 avail_mem=37.95 GB):  40%|███▉      | 23/58 [00:06<00:04,  7.15it/s]

    Capturing num tokens (num_tokens=768 avail_mem=37.95 GB):  43%|████▎     | 25/58 [00:07<00:04,  7.91it/s]Capturing num tokens (num_tokens=704 avail_mem=38.43 GB):  43%|████▎     | 25/58 [00:07<00:04,  7.91it/s]Capturing num tokens (num_tokens=640 avail_mem=37.97 GB):  43%|████▎     | 25/58 [00:07<00:04,  7.91it/s]Capturing num tokens (num_tokens=640 avail_mem=37.97 GB):  47%|████▋     | 27/58 [00:07<00:03,  8.64it/s]Capturing num tokens (num_tokens=576 avail_mem=38.42 GB):  47%|████▋     | 27/58 [00:07<00:03,  8.64it/s]

    Capturing num tokens (num_tokens=576 avail_mem=38.42 GB):  48%|████▊     | 28/58 [00:07<00:03,  8.69it/s]Capturing num tokens (num_tokens=512 avail_mem=37.98 GB):  48%|████▊     | 28/58 [00:07<00:03,  8.69it/s]Capturing num tokens (num_tokens=480 avail_mem=38.40 GB):  48%|████▊     | 28/58 [00:07<00:03,  8.69it/s]Capturing num tokens (num_tokens=480 avail_mem=38.40 GB):  52%|█████▏    | 30/58 [00:07<00:02,  9.40it/s]Capturing num tokens (num_tokens=448 avail_mem=38.00 GB):  52%|█████▏    | 30/58 [00:07<00:02,  9.40it/s]

    Capturing num tokens (num_tokens=416 avail_mem=38.32 GB):  52%|█████▏    | 30/58 [00:07<00:02,  9.40it/s]Capturing num tokens (num_tokens=416 avail_mem=38.32 GB):  55%|█████▌    | 32/58 [00:07<00:02, 10.21it/s]Capturing num tokens (num_tokens=384 avail_mem=38.08 GB):  55%|█████▌    | 32/58 [00:07<00:02, 10.21it/s]

    Capturing num tokens (num_tokens=352 avail_mem=38.35 GB):  55%|█████▌    | 32/58 [00:07<00:02, 10.21it/s]

    Capturing num tokens (num_tokens=352 avail_mem=38.35 GB):  59%|█████▊    | 34/58 [00:08<00:03,  6.72it/s]Capturing num tokens (num_tokens=320 avail_mem=38.36 GB):  59%|█████▊    | 34/58 [00:08<00:03,  6.72it/s]Capturing num tokens (num_tokens=320 avail_mem=38.36 GB):  60%|██████    | 35/58 [00:08<00:03,  7.09it/s]Capturing num tokens (num_tokens=288 avail_mem=38.36 GB):  60%|██████    | 35/58 [00:08<00:03,  7.09it/s]Capturing num tokens (num_tokens=256 avail_mem=38.08 GB):  60%|██████    | 35/58 [00:08<00:03,  7.09it/s]

    Capturing num tokens (num_tokens=256 avail_mem=38.08 GB):  64%|██████▍   | 37/58 [00:08<00:02,  8.93it/s]Capturing num tokens (num_tokens=240 avail_mem=38.30 GB):  64%|██████▍   | 37/58 [00:08<00:02,  8.93it/s]Capturing num tokens (num_tokens=224 avail_mem=38.13 GB):  64%|██████▍   | 37/58 [00:08<00:02,  8.93it/s]Capturing num tokens (num_tokens=224 avail_mem=38.13 GB):  67%|██████▋   | 39/58 [00:08<00:01, 10.61it/s]Capturing num tokens (num_tokens=208 avail_mem=38.33 GB):  67%|██████▋   | 39/58 [00:08<00:01, 10.61it/s]Capturing num tokens (num_tokens=192 avail_mem=38.34 GB):  67%|██████▋   | 39/58 [00:08<00:01, 10.61it/s]

    Capturing num tokens (num_tokens=176 avail_mem=38.20 GB):  67%|██████▋   | 39/58 [00:08<00:01, 10.61it/s]Capturing num tokens (num_tokens=176 avail_mem=38.20 GB):  72%|███████▏  | 42/58 [00:08<00:01, 13.19it/s]Capturing num tokens (num_tokens=160 avail_mem=38.21 GB):  72%|███████▏  | 42/58 [00:08<00:01, 13.19it/s]Capturing num tokens (num_tokens=144 avail_mem=38.21 GB):  72%|███████▏  | 42/58 [00:08<00:01, 13.19it/s]

    Capturing num tokens (num_tokens=144 avail_mem=38.21 GB):  76%|███████▌  | 44/58 [00:08<00:01, 12.69it/s]Capturing num tokens (num_tokens=128 avail_mem=38.32 GB):  76%|███████▌  | 44/58 [00:08<00:01, 12.69it/s]Capturing num tokens (num_tokens=112 avail_mem=38.32 GB):  76%|███████▌  | 44/58 [00:08<00:01, 12.69it/s]Capturing num tokens (num_tokens=112 avail_mem=38.32 GB):  79%|███████▉  | 46/58 [00:08<00:00, 14.15it/s]Capturing num tokens (num_tokens=96 avail_mem=38.31 GB):  79%|███████▉  | 46/58 [00:08<00:00, 14.15it/s] Capturing num tokens (num_tokens=80 avail_mem=38.30 GB):  79%|███████▉  | 46/58 [00:09<00:00, 14.15it/s]Capturing num tokens (num_tokens=64 avail_mem=38.19 GB):  79%|███████▉  | 46/58 [00:09<00:00, 14.15it/s]

    Capturing num tokens (num_tokens=64 avail_mem=38.19 GB):  84%|████████▍ | 49/58 [00:09<00:00, 16.63it/s]Capturing num tokens (num_tokens=48 avail_mem=38.19 GB):  84%|████████▍ | 49/58 [00:09<00:00, 16.63it/s]Capturing num tokens (num_tokens=32 avail_mem=38.19 GB):  84%|████████▍ | 49/58 [00:09<00:00, 16.63it/s]Capturing num tokens (num_tokens=28 avail_mem=38.28 GB):  84%|████████▍ | 49/58 [00:09<00:00, 16.63it/s]Capturing num tokens (num_tokens=28 avail_mem=38.28 GB):  90%|████████▉ | 52/58 [00:09<00:00, 18.29it/s]Capturing num tokens (num_tokens=24 avail_mem=38.27 GB):  90%|████████▉ | 52/58 [00:09<00:00, 18.29it/s]Capturing num tokens (num_tokens=20 avail_mem=38.26 GB):  90%|████████▉ | 52/58 [00:09<00:00, 18.29it/s]

    Capturing num tokens (num_tokens=16 avail_mem=38.26 GB):  90%|████████▉ | 52/58 [00:09<00:00, 18.29it/s]Capturing num tokens (num_tokens=16 avail_mem=38.26 GB):  95%|█████████▍| 55/58 [00:09<00:00, 19.91it/s]Capturing num tokens (num_tokens=12 avail_mem=38.25 GB):  95%|█████████▍| 55/58 [00:09<00:00, 19.91it/s]Capturing num tokens (num_tokens=8 avail_mem=38.24 GB):  95%|█████████▍| 55/58 [00:09<00:00, 19.91it/s] Capturing num tokens (num_tokens=4 avail_mem=38.24 GB):  95%|█████████▍| 55/58 [00:09<00:00, 19.91it/s]Capturing num tokens (num_tokens=4 avail_mem=38.24 GB): 100%|██████████| 58/58 [00:09<00:00, 21.74it/s]Capturing num tokens (num_tokens=4 avail_mem=38.24 GB): 100%|██████████| 58/58 [00:09<00:00,  6.11it/s]


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
    
    Generated text: Okay, the user is asking for the information and population of the capital of France in JSON format. So, I need to figure out the correct data. 
    
    First, the capital of France is definitely Paris. I'm pretty sure about that. Now, I need the population. I remember that Paris is one of the most populous cities in the world, but I'm not exactly sure about the current number. 
    
    I think the population has been growing over the years. I recall that in recent years, it was around 2 million. Maybe 2020 or 2021? But I should double-check that. 
    
    Wait, I think I heard that Paris's population is approximately 2.1 million in 2021. That sounds about right. I should confirm that the source is reliable. I believe it's based on recent estimates from organizations like INSEE, which is the French National Institute of Statistics and Information of the Economy. 
    
    So, putting it all together, the JSON structure should include the key "capital" with the name "Paris" and the "population" key with the number 2100000. That should meet the user's request accurately.
    </think>
    
    ```json
    {
      "capital": "Paris",
      "population": 2100000
    }
    ```



```python
llm.shutdown()
```
