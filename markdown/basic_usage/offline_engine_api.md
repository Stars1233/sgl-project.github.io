# Offline Engine API

SGLang provides a direct inference engine without the need for an HTTP server, especially for use cases where additional HTTP server adds unnecessary complexity or overhead. Here are two general use cases:

- Offline Batch Inference
- Custom Server on Top of the Engine

This document focuses on the offline batch inference, demonstrating four different inference modes:

- Non-streaming synchronous generation
- Streaming synchronous generation
- Non-streaming asynchronous generation
- Streaming asynchronous generation

Additionally, you can easily build a custom server on top of the SGLang offline engine. A detailed example working in a python script can be found in [custom_server](https://github.com/sgl-project/sglang/blob/main/examples/runtime/engine/custom_server.py).



## Nest Asyncio
Note that if you want to use **Offline Engine** in ipython or some other nested loop code, you need to add the following code:
```python
import nest_asyncio

nest_asyncio.apply()

```

## Advanced Usage

The engine supports [vlm inference](https://github.com/sgl-project/sglang/blob/main/examples/runtime/engine/offline_batch_inference_vlm.py) as well as [extracting hidden states](https://github.com/sgl-project/sglang/blob/main/examples/runtime/hidden_states). 

Please see [the examples](https://github.com/sgl-project/sglang/tree/main/examples/runtime/engine) for further use cases.

## Offline Batch Inference

SGLang offline engine supports batch inference with efficient scheduling.


```python
# launch the offline engine
import asyncio

import sglang as sgl
import sglang.test.doc_patch  # noqa: F401
from sglang.utils import async_stream_and_merge, stream_and_merge

llm = sgl.Engine(model_path="qwen/qwen2.5-0.5b-instruct")
```

    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-04-01 20:00:41.370 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 20:00:41] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 20:00:41.370 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 20:00:41] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 20:00:41.370 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 20:00:41] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 20:00:41.370 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 20:00:41] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 20:00:41.371 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 20:00:41] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  3.01it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  3.00it/s]


    2026-04-01 20:00:44,199 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 20:00:44] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:30,  2.63s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:30,  2.63s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<01:04,  1.15s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<01:04,  1.15s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<01:04,  1.15s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:25,  2.13it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:25,  2.13it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:25,  2.13it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:02<00:14,  3.68it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:02<00:14,  3.68it/s]Compiling num tokens (num_tokens=4608):  10%|█         | 6/58 [00:03<00:14,  3.68it/s]Compiling num tokens (num_tokens=4096):  10%|█         | 6/58 [00:03<00:14,  3.68it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:03<00:07,  6.44it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:03<00:07,  6.44it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:03<00:07,  6.44it/s]Compiling num tokens (num_tokens=3328):  16%|█▌        | 9/58 [00:03<00:07,  6.44it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:03<00:04,  9.57it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:03<00:04,  9.57it/s]Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:03<00:04,  9.57it/s]Compiling num tokens (num_tokens=2560):  21%|██        | 12/58 [00:03<00:04,  9.57it/s]Compiling num tokens (num_tokens=2304):  21%|██        | 12/58 [00:03<00:04,  9.57it/s]

    Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:03<00:02, 14.06it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:03<00:02, 14.06it/s]Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:03<00:02, 14.06it/s]Compiling num tokens (num_tokens=1536):  28%|██▊       | 16/58 [00:03<00:02, 14.06it/s]Compiling num tokens (num_tokens=1280):  28%|██▊       | 16/58 [00:03<00:02, 14.06it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:03<00:02, 17.78it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:03<00:02, 17.78it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:03<00:02, 17.78it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:03<00:02, 17.78it/s]

    Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:03<00:02, 17.78it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:03<00:01, 22.05it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:03<00:01, 22.05it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:03<00:01, 22.05it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:03<00:01, 22.05it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:03<00:01, 22.05it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:03<00:01, 24.99it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:03<00:01, 24.99it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:03<00:01, 24.99it/s]

    Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:03<00:01, 24.99it/s]Compiling num tokens (num_tokens=416):  48%|████▊     | 28/58 [00:03<00:01, 24.99it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:00, 27.36it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:00, 27.36it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:00, 27.36it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:00, 27.36it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:00, 27.36it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:03<00:00, 29.29it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:03<00:00, 29.29it/s]

    Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:03<00:00, 29.29it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:03<00:00, 29.29it/s]Compiling num tokens (num_tokens=208):  62%|██████▏   | 36/58 [00:03<00:00, 29.29it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:04<00:00, 30.53it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:04<00:00, 30.53it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:04<00:00, 30.53it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:04<00:00, 30.53it/s]Compiling num tokens (num_tokens=144):  69%|██████▉   | 40/58 [00:04<00:00, 30.53it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:04<00:00, 32.23it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:04<00:00, 32.23it/s]

    Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:04<00:00, 32.23it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:04<00:00, 32.23it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:04<00:00, 32.23it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:04<00:00, 32.12it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:04<00:00, 32.12it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:04<00:00, 32.12it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:04<00:00, 32.12it/s]Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:04<00:00, 32.12it/s]

    Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:04<00:00, 33.16it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:04<00:00, 33.16it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:04<00:00, 33.16it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:04<00:00, 33.16it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:04<00:00, 33.16it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:04<00:00, 32.43it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:04<00:00, 32.43it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:04<00:00, 32.43it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 12.80it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=54.57 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=54.57 GB):   2%|▏         | 1/58 [00:00<00:08,  6.94it/s]Capturing num tokens (num_tokens=7680 avail_mem=54.54 GB):   2%|▏         | 1/58 [00:00<00:08,  6.94it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=54.54 GB):   3%|▎         | 2/58 [00:00<00:07,  7.10it/s]Capturing num tokens (num_tokens=7168 avail_mem=54.53 GB):   3%|▎         | 2/58 [00:00<00:07,  7.10it/s]Capturing num tokens (num_tokens=7168 avail_mem=54.53 GB):   5%|▌         | 3/58 [00:00<00:07,  7.58it/s]Capturing num tokens (num_tokens=6656 avail_mem=54.53 GB):   5%|▌         | 3/58 [00:00<00:07,  7.58it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=54.53 GB):   7%|▋         | 4/58 [00:00<00:06,  7.85it/s]Capturing num tokens (num_tokens=6144 avail_mem=54.53 GB):   7%|▋         | 4/58 [00:00<00:06,  7.85it/s]Capturing num tokens (num_tokens=6144 avail_mem=54.53 GB):   9%|▊         | 5/58 [00:00<00:06,  8.32it/s]Capturing num tokens (num_tokens=5632 avail_mem=54.53 GB):   9%|▊         | 5/58 [00:00<00:06,  8.32it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=54.53 GB):   9%|▊         | 5/58 [00:00<00:06,  8.32it/s]Capturing num tokens (num_tokens=5120 avail_mem=54.53 GB):  12%|█▏        | 7/58 [00:00<00:05,  9.05it/s]Capturing num tokens (num_tokens=4608 avail_mem=54.53 GB):  12%|█▏        | 7/58 [00:00<00:05,  9.05it/s]Capturing num tokens (num_tokens=4096 avail_mem=54.53 GB):  12%|█▏        | 7/58 [00:00<00:05,  9.05it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=54.53 GB):  16%|█▌        | 9/58 [00:01<00:05,  9.71it/s]Capturing num tokens (num_tokens=3840 avail_mem=54.52 GB):  16%|█▌        | 9/58 [00:01<00:05,  9.71it/s]Capturing num tokens (num_tokens=3840 avail_mem=54.52 GB):  17%|█▋        | 10/58 [00:01<00:05,  9.48it/s]Capturing num tokens (num_tokens=3584 avail_mem=53.97 GB):  17%|█▋        | 10/58 [00:01<00:05,  9.48it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=53.97 GB):  19%|█▉        | 11/58 [00:01<00:05,  8.59it/s]Capturing num tokens (num_tokens=3328 avail_mem=53.97 GB):  19%|█▉        | 11/58 [00:01<00:05,  8.59it/s]Capturing num tokens (num_tokens=3328 avail_mem=53.97 GB):  21%|██        | 12/58 [00:01<00:05,  8.08it/s]Capturing num tokens (num_tokens=3072 avail_mem=53.96 GB):  21%|██        | 12/58 [00:01<00:05,  8.08it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=53.96 GB):  22%|██▏       | 13/58 [00:01<00:06,  7.43it/s]Capturing num tokens (num_tokens=2816 avail_mem=53.96 GB):  22%|██▏       | 13/58 [00:01<00:06,  7.43it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=53.96 GB):  24%|██▍       | 14/58 [00:01<00:08,  5.13it/s]Capturing num tokens (num_tokens=2560 avail_mem=53.96 GB):  24%|██▍       | 14/58 [00:01<00:08,  5.13it/s]Capturing num tokens (num_tokens=2560 avail_mem=53.96 GB):  26%|██▌       | 15/58 [00:02<00:08,  5.19it/s]Capturing num tokens (num_tokens=2304 avail_mem=53.96 GB):  26%|██▌       | 15/58 [00:02<00:08,  5.19it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=53.96 GB):  28%|██▊       | 16/58 [00:02<00:07,  5.55it/s]Capturing num tokens (num_tokens=2048 avail_mem=53.95 GB):  28%|██▊       | 16/58 [00:02<00:07,  5.55it/s]Capturing num tokens (num_tokens=2048 avail_mem=53.95 GB):  29%|██▉       | 17/58 [00:02<00:06,  6.18it/s]Capturing num tokens (num_tokens=1792 avail_mem=53.95 GB):  29%|██▉       | 17/58 [00:02<00:06,  6.18it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=53.95 GB):  31%|███       | 18/58 [00:02<00:06,  6.65it/s]Capturing num tokens (num_tokens=1536 avail_mem=53.95 GB):  31%|███       | 18/58 [00:02<00:06,  6.65it/s]Capturing num tokens (num_tokens=1536 avail_mem=53.95 GB):  33%|███▎      | 19/58 [00:02<00:05,  6.97it/s]Capturing num tokens (num_tokens=1280 avail_mem=53.94 GB):  33%|███▎      | 19/58 [00:02<00:05,  6.97it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=53.94 GB):  34%|███▍      | 20/58 [00:02<00:05,  7.29it/s]Capturing num tokens (num_tokens=1024 avail_mem=53.92 GB):  34%|███▍      | 20/58 [00:02<00:05,  7.29it/s]Capturing num tokens (num_tokens=960 avail_mem=53.93 GB):  34%|███▍      | 20/58 [00:02<00:05,  7.29it/s] Capturing num tokens (num_tokens=960 avail_mem=53.93 GB):  38%|███▊      | 22/58 [00:02<00:04,  8.53it/s]Capturing num tokens (num_tokens=896 avail_mem=53.93 GB):  38%|███▊      | 22/58 [00:02<00:04,  8.53it/s]

    Capturing num tokens (num_tokens=832 avail_mem=53.92 GB):  38%|███▊      | 22/58 [00:03<00:04,  8.53it/s]Capturing num tokens (num_tokens=832 avail_mem=53.92 GB):  41%|████▏     | 24/58 [00:03<00:03,  9.63it/s]Capturing num tokens (num_tokens=768 avail_mem=53.92 GB):  41%|████▏     | 24/58 [00:03<00:03,  9.63it/s]Capturing num tokens (num_tokens=704 avail_mem=53.92 GB):  41%|████▏     | 24/58 [00:03<00:03,  9.63it/s]

    Capturing num tokens (num_tokens=704 avail_mem=53.92 GB):  45%|████▍     | 26/58 [00:03<00:03, 10.42it/s]Capturing num tokens (num_tokens=640 avail_mem=53.91 GB):  45%|████▍     | 26/58 [00:03<00:03, 10.42it/s]Capturing num tokens (num_tokens=576 avail_mem=53.91 GB):  45%|████▍     | 26/58 [00:03<00:03, 10.42it/s]Capturing num tokens (num_tokens=576 avail_mem=53.91 GB):  48%|████▊     | 28/58 [00:03<00:02, 10.98it/s]Capturing num tokens (num_tokens=512 avail_mem=53.90 GB):  48%|████▊     | 28/58 [00:03<00:02, 10.98it/s]

    Capturing num tokens (num_tokens=480 avail_mem=53.92 GB):  48%|████▊     | 28/58 [00:03<00:02, 10.98it/s]Capturing num tokens (num_tokens=480 avail_mem=53.92 GB):  52%|█████▏    | 30/58 [00:03<00:02, 10.60it/s]Capturing num tokens (num_tokens=448 avail_mem=53.89 GB):  52%|█████▏    | 30/58 [00:03<00:02, 10.60it/s]

    Capturing num tokens (num_tokens=416 avail_mem=53.43 GB):  52%|█████▏    | 30/58 [00:03<00:02, 10.60it/s]Capturing num tokens (num_tokens=416 avail_mem=53.43 GB):  55%|█████▌    | 32/58 [00:03<00:02,  9.89it/s]Capturing num tokens (num_tokens=384 avail_mem=53.46 GB):  55%|█████▌    | 32/58 [00:03<00:02,  9.89it/s]

    Capturing num tokens (num_tokens=352 avail_mem=53.88 GB):  55%|█████▌    | 32/58 [00:03<00:02,  9.89it/s]Capturing num tokens (num_tokens=352 avail_mem=53.88 GB):  59%|█████▊    | 34/58 [00:04<00:02,  9.58it/s]Capturing num tokens (num_tokens=320 avail_mem=53.88 GB):  59%|█████▊    | 34/58 [00:04<00:02,  9.58it/s]

    Capturing num tokens (num_tokens=320 avail_mem=53.88 GB):  60%|██████    | 35/58 [00:04<00:02,  9.42it/s]Capturing num tokens (num_tokens=288 avail_mem=53.88 GB):  60%|██████    | 35/58 [00:04<00:02,  9.42it/s]Capturing num tokens (num_tokens=288 avail_mem=53.88 GB):  62%|██████▏   | 36/58 [00:04<00:02,  9.43it/s]Capturing num tokens (num_tokens=256 avail_mem=53.88 GB):  62%|██████▏   | 36/58 [00:04<00:02,  9.43it/s]

    Capturing num tokens (num_tokens=256 avail_mem=53.88 GB):  64%|██████▍   | 37/58 [00:04<00:02,  9.49it/s]Capturing num tokens (num_tokens=240 avail_mem=53.88 GB):  64%|██████▍   | 37/58 [00:04<00:02,  9.49it/s]Capturing num tokens (num_tokens=240 avail_mem=53.88 GB):  66%|██████▌   | 38/58 [00:04<00:02,  9.56it/s]Capturing num tokens (num_tokens=224 avail_mem=53.87 GB):  66%|██████▌   | 38/58 [00:04<00:02,  9.56it/s]

    Capturing num tokens (num_tokens=224 avail_mem=53.87 GB):  67%|██████▋   | 39/58 [00:04<00:01,  9.63it/s]Capturing num tokens (num_tokens=208 avail_mem=53.87 GB):  67%|██████▋   | 39/58 [00:04<00:01,  9.63it/s]Capturing num tokens (num_tokens=192 avail_mem=53.87 GB):  67%|██████▋   | 39/58 [00:04<00:01,  9.63it/s]Capturing num tokens (num_tokens=192 avail_mem=53.87 GB):  71%|███████   | 41/58 [00:04<00:01,  9.93it/s]Capturing num tokens (num_tokens=176 avail_mem=53.87 GB):  71%|███████   | 41/58 [00:04<00:01,  9.93it/s]

    Capturing num tokens (num_tokens=160 avail_mem=53.86 GB):  71%|███████   | 41/58 [00:04<00:01,  9.93it/s]Capturing num tokens (num_tokens=160 avail_mem=53.86 GB):  74%|███████▍  | 43/58 [00:05<00:01, 10.27it/s]Capturing num tokens (num_tokens=144 avail_mem=53.86 GB):  74%|███████▍  | 43/58 [00:05<00:01, 10.27it/s]Capturing num tokens (num_tokens=128 avail_mem=53.85 GB):  74%|███████▍  | 43/58 [00:05<00:01, 10.27it/s]

    Capturing num tokens (num_tokens=128 avail_mem=53.85 GB):  78%|███████▊  | 45/58 [00:05<00:01, 10.66it/s]Capturing num tokens (num_tokens=112 avail_mem=53.71 GB):  78%|███████▊  | 45/58 [00:05<00:01, 10.66it/s]Capturing num tokens (num_tokens=96 avail_mem=53.85 GB):  78%|███████▊  | 45/58 [00:05<00:01, 10.66it/s] Capturing num tokens (num_tokens=96 avail_mem=53.85 GB):  81%|████████  | 47/58 [00:05<00:01, 10.98it/s]Capturing num tokens (num_tokens=80 avail_mem=53.84 GB):  81%|████████  | 47/58 [00:05<00:01, 10.98it/s]

    Capturing num tokens (num_tokens=64 avail_mem=53.83 GB):  81%|████████  | 47/58 [00:05<00:01, 10.98it/s]Capturing num tokens (num_tokens=64 avail_mem=53.83 GB):  84%|████████▍ | 49/58 [00:05<00:00, 11.48it/s]Capturing num tokens (num_tokens=48 avail_mem=53.83 GB):  84%|████████▍ | 49/58 [00:05<00:00, 11.48it/s]Capturing num tokens (num_tokens=32 avail_mem=53.82 GB):  84%|████████▍ | 49/58 [00:05<00:00, 11.48it/s]

    Capturing num tokens (num_tokens=32 avail_mem=53.82 GB):  88%|████████▊ | 51/58 [00:05<00:00, 11.63it/s]Capturing num tokens (num_tokens=28 avail_mem=53.81 GB):  88%|████████▊ | 51/58 [00:05<00:00, 11.63it/s]Capturing num tokens (num_tokens=24 avail_mem=53.77 GB):  88%|████████▊ | 51/58 [00:05<00:00, 11.63it/s]Capturing num tokens (num_tokens=24 avail_mem=53.77 GB):  91%|█████████▏| 53/58 [00:05<00:00, 11.91it/s]Capturing num tokens (num_tokens=20 avail_mem=53.80 GB):  91%|█████████▏| 53/58 [00:05<00:00, 11.91it/s]

    Capturing num tokens (num_tokens=16 avail_mem=53.80 GB):  91%|█████████▏| 53/58 [00:05<00:00, 11.91it/s]Capturing num tokens (num_tokens=16 avail_mem=53.80 GB):  95%|█████████▍| 55/58 [00:05<00:00, 12.16it/s]Capturing num tokens (num_tokens=12 avail_mem=53.77 GB):  95%|█████████▍| 55/58 [00:05<00:00, 12.16it/s]Capturing num tokens (num_tokens=8 avail_mem=53.78 GB):  95%|█████████▍| 55/58 [00:06<00:00, 12.16it/s] 

    Capturing num tokens (num_tokens=8 avail_mem=53.78 GB):  98%|█████████▊| 57/58 [00:06<00:00, 11.70it/s]Capturing num tokens (num_tokens=4 avail_mem=53.78 GB):  98%|█████████▊| 57/58 [00:06<00:00, 11.70it/s]Capturing num tokens (num_tokens=4 avail_mem=53.78 GB): 100%|██████████| 58/58 [00:06<00:00,  9.19it/s]


### Non-streaming Synchronous Generation


```python
prompts = [
    "Hello, my name is",
    "The president of the United States is",
    "The capital of France is",
    "The future of AI is",
]

sampling_params = {"temperature": 0.8, "top_p": 0.95}

outputs = llm.generate(prompts, sampling_params)
for prompt, output in zip(prompts, outputs):
    print("===============================")
    print(f"Prompt: {prompt}\nGenerated text: {output['text']}")
```

    ===============================
    Prompt: Hello, my name is
    Generated text:  Anna, and I am a student. My hobbies include reading, playing sports, and watching TV. My favorite color is blue. My job is to help people. I am working towards a degree in computer science. How can I improve my English?
    
    To improve my English, I should study grammar, vocabulary, and listening and speaking skills. Additionally, I should read widely and actively participate in language exchange programs. I also need to listen to some podcasts and watch some TV shows that focus on English language learning. Lastly, I should take an English course to receive personalized guidance and feedback from my teacher.
    To improve my English pronunciation, I can
    ===============================
    Prompt: The president of the United States is
    Generated text:  3 feet tall. His wife is 2/3 as tall as him. Additionally, he has a dog that is 1/4 the height of his wife. How tall is the dog in feet?
    Let's start by determining the height of the dog. We know that the president of the United States is 3 feet tall and his wife is \(\frac{2}{3}\) as tall as him. First, we calculate the height of the wife:
    
    \[
    \text{Height of the wife} = \frac{2}{3} \times 3 = 2 \text{ feet}
    \]
    
    
    ===============================
    Prompt: The capital of France is
    Generated text:  in which region?
    The capital of France is Paris. Paris is the most populous city in France, and it is located in the south of the country, between the Atlantic Ocean to the east and the Paris Basin to the west. It is also the largest city in Europe, with a population of over 2.1 million. The city is known for its historic architecture, museums, and vibrant culture, and it is a popular tourist destination. Paris is located on the Île de la Cité, which is surrounded by the Seine River, and it is separated from the city by the Paris Basin. The capital of France is situated
    ===============================
    Prompt: The future of AI is
    Generated text:  here and it will transform how we live and work. But with that comes a greater risk: more risks. Tech companies, governments, and other organizations are investing billions of dollars in research to understand how to harness the power of AI. But the companies that will drive the greatest impact are those that are more data-oriented, and not data-hungry.
    
    It's time to rethink what data is and how it should be used.
    
    The big data revolution started in the 1990s, when companies began to collect, analyze, and share data. However, the revolution took a different path in the past decade. In the last


### Streaming Synchronous Generation


```python
prompts = [
    "Write a short, neutral self-introduction for a fictional character. Hello, my name is",
    "Provide a concise factual statement about France’s capital city. The capital of France is",
    "Explain possible future trends in artificial intelligence. The future of AI is",
]

sampling_params = {
    "temperature": 0.2,
    "top_p": 0.9,
}

print("\n=== Testing synchronous streaming generation with overlap removal ===\n")

for prompt in prompts:
    print(f"Prompt: {prompt}")
    merged_output = stream_and_merge(llm, prompt, sampling_params)
    print("Generated text:", merged_output)
    print()
```

    
    === Testing synchronous streaming generation with overlap removal ===
    
    Prompt: Write a short, neutral self-introduction for a fictional character. Hello, my name is


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [insert a short description of your profession or experience here]. I enjoy [insert a short description of your hobbies or interests here]. I'm always looking for new challenges and opportunities to grow and learn. What do you do for a living? I'm a [insert a short description of your job or role here]. I'm always looking for ways to improve my skills and stay up-to-date with the latest trends and technologies. What do you like
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic Eiffel Tower and its rich history dating back to the Middle Ages. It is also the country's largest city and the second most populous city in the European Union. Paris is home to many famous landmarks, including the Louvre Museum, the Notre-Dame Cathedral, and the Arc de Triomphe. The city is also known for its fashion industry, with Paris Fashion Week being one of the world's largest and most influential. Paris is a vibrant and diverse city with a rich cultural heritage that continues to thrive today.
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the way we live, work, and interact with technology. Here are some possible future trends in AI:
    
    1. Increased automation and robotics: As AI technology continues to advance, we can expect to see more automation and robotics in various industries. This could lead to increased efficiency and productivity, but it could also lead to job displacement for some workers.
    
    2. Enhanced privacy and security: As AI becomes more integrated into our daily lives, we can expect to see increased emphasis on privacy and security. This could include measures such as data encryption, biometric authentication, and the use
    


### Non-streaming Asynchronous Generation


```python
prompts = [
    "Write a short, neutral self-introduction for a fictional character. Hello, my name is",
    "Provide a concise factual statement about France’s capital city. The capital of France is",
    "Explain possible future trends in artificial intelligence. The future of AI is",
]

sampling_params = {"temperature": 0.8, "top_p": 0.95}

print("\n=== Testing asynchronous batch generation ===")


async def main():
    outputs = await llm.async_generate(prompts, sampling_params)

    for prompt, output in zip(prompts, outputs):
        print(f"\nPrompt: {prompt}")
        print(f"Generated text: {output['text']}")


asyncio.run(main())
```

    
    === Testing asynchronous batch generation ===


    
    Prompt: Write a short, neutral self-introduction for a fictional character. Hello, my name is
    Generated text:  [insert name here]. I'm a [insert age here] year old who has always been a bit of a troublemaker, but I've learned to take a lot of the blame. I love to play games and am always on the lookout for opportunities to challenge myself. I'm incredibly quick at learning and have a natural sense of humor that I use to make people laugh. I enjoy hiking and camping, and I have a lot of skills in cooking and baking, plus I'm a bit of a prankster. I'm always up for a challenge, and I believe that I'm capable of achieving anything I set my mind to.
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. It is the second largest city in the country, located on the western bank of the Seine River, near the center of the Paris Basin. The city is also known as the City of Light and the City of Many Faces. It is home to the Eiffel Tower and numerous other iconic landmarks. Paris is a vibrant and culturally rich city, known for its rich history, beautiful architecture, and popular culture. Its population is estimated to be around 2. 3 million as of 2021. It is the seat of the French government and is home to the headquarters of many important French organizations and institutions.
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  uncertain, and it is difficult to predict with certainty what the next big developments will be. However, there are some possible trends that can be expected based on current research and industry trends.
    
    1. Advancements in Machine Learning: One of the most exciting areas of AI research is machine learning, which involves using algorithms to enable machines to learn from data and make predictions or decisions. This technology is already being used in a variety of applications, from self-driving cars to facial recognition systems to language translation.
    
    2. Development of Deep Learning: Deep learning is a subset of machine learning that involves using multiple layers of neurons to model complex relationships between data.


### Streaming Asynchronous Generation


```python
prompts = [
    "Write a short, neutral self-introduction for a fictional character. Hello, my name is",
    "Provide a concise factual statement about France’s capital city. The capital of France is",
    "Explain possible future trends in artificial intelligence. The future of AI is",
]

sampling_params = {"temperature": 0.8, "top_p": 0.95}

print("\n=== Testing asynchronous streaming generation (no repeats) ===")


async def main():
    for prompt in prompts:
        print(f"\nPrompt: {prompt}")
        print("Generated text: ", end="", flush=True)

        # Replace direct calls to async_generate with our custom overlap-aware version
        async for cleaned_chunk in async_stream_and_merge(llm, prompt, sampling_params):
            print(cleaned_chunk, end="", flush=True)

        print()  # New line after each prompt


asyncio.run(main())
```

    
    === Testing asynchronous streaming generation (no repeats) ===
    
    Prompt: Write a short, neutral self-introduction for a fictional character. Hello, my name is
    Generated text: 

     [

    Name

    ].

     I

    'm

     a

     versatile

     and

     curious

     individual

     who

     is

     always

     looking

     for

     new

     opportunities

    .

     I

     enjoy

     traveling

    ,

     reading

    ,

     and

     trying

     new

     foods

    .

     I

    'm

     also

     a

     great

     listener

    ,

     and

     enjoy

     giving

     and

     receiving

     feedback

    .

     I

    'm

     confident

     in

     my

     abilities

     and

     always

     strive

     to

     learn

     and

     grow

    .

     What

    's

     your

     favorite

     hobby

     or

     activity

    ?

     I

     enjoy

     going

     for

     a

     walk

     in

     nature

     or

     spending

     time

     with

     friends

     and

     family

    .

     Lastly

    ,

     what

    's

     your

     biggest

     accomplishment

     so

     far

    ?

     I

    've

     been

     able

     to

     travel

     to

     new

     places

     and

     meet

     new

     people

    ,

     which

     has

     been

     a

     great

     experience

    .

     I

    'm

     excited

     to

     continue

     on

     this

     journey

     and

     make

     more

     memories

    !

     


    [

    Name

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .


    Paris

     is

     the

     largest

     city

     in

     France

     by

     area

     and

     population

    .

     The

     city

     is

     home

     to

     a

     wide

     range

     of

     cultural

     institutions

     and

     landmarks

    ,

     including

     the

     Lou

    vre

    ,

     the

     E

    iff

    el

     Tower

    ,

     and

     the

     Notre

    -D

    ame

     Cathedral

    .

     Paris

     is

     known

     for

     its

     cuisine

    ,

     fashion

    ,

     and

     art

     scene

    ,

     and

     it

     is

     a

     popular

     tourist

     destination

     throughout

     the

     year

    .

     The

     city

     has

     a

     rich

     history

     and

     is

     a

     hub

     for

     various

     forms

     of

     entertainment

    ,

     including

     cinema

    ,

     opera

    ,

     and

     ballet

    .

     Paris

     is

     also

     known

     for

     its

     contributions

     to

     the

     arts

    ,

     science

    ,

     and

     technology

    ,

     including

     the

     famous

     "

    Lost

     City

     of

     the

     Inc

    as

    "

     and

     the

     design

     of

     iconic

     buildings

     like

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     shaped

     by

     a

     variety

     of

     trends

    ,

     including

    :
    


    1

    .

     Increased

     precision

     and

     accuracy

    :

     AI

     is

     expected

     to

     continue

     improving

     in

     terms

     of

     accuracy

    ,

     speed

    ,

     and

     efficiency

    .

     This

     will

     make

     it

     possible

     to

     perform

     tasks

     that

     were

     previously

     impossible

     or

     impr

    actical

    .
    


    2

    .

     Integration

     of

     AI

     with

     other

     technologies

    :

     AI

     is

     expected

     to

     become

     more

     integrated

     with

     other

     technologies

    ,

     such

     as

     sensors

    ,

     micro

    ch

    ips

    ,

     and

     the

     internet

    ,

     to

     create

     a

     more

     cohesive

     and

     versatile

     system

    .
    


    3

    .

     Em

    phasis

     on

     ethics

     and

     fairness

    :

     As

     AI

     becomes

     more

     pervasive

     in

     our

     lives

    ,

     there

     will

     be

     greater

     emphasis

     on

     ethical

     and

     fairness

     considerations

    .

     This

     will

     require

     a

     greater

     investment

     in

    



```python
llm.shutdown()
```
