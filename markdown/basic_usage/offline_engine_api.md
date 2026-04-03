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


    2026-04-03 05:18:38.479 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 05:18:38] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 05:18:38.479 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 05:18:38] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 05:18:38.479 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 05:18:38] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 05:18:38.479 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 05:18:38] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 05:18:38.479 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 05:18:38] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.09it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.08it/s]


    2026-04-03 05:18:43,061 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 05:18:43] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:33,  2.69s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:33,  2.69s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<01:05,  1.17s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<01:05,  1.17s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<01:05,  1.17s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:25,  2.13it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:25,  2.13it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:25,  2.13it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:25,  2.13it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:03<00:11,  4.46it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:03<00:11,  4.46it/s]Compiling num tokens (num_tokens=4096):  12%|█▏        | 7/58 [00:03<00:11,  4.46it/s]Compiling num tokens (num_tokens=3840):  12%|█▏        | 7/58 [00:03<00:11,  4.46it/s]

    Compiling num tokens (num_tokens=3584):  12%|█▏        | 7/58 [00:03<00:11,  4.46it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:03<00:05,  8.10it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:03<00:05,  8.10it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:03<00:05,  8.10it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:03<00:05,  8.10it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:03<00:05,  8.10it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:03<00:05,  8.10it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:03<00:03, 13.34it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:03<00:03, 13.34it/s]Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:03<00:03, 13.34it/s]Compiling num tokens (num_tokens=1536):  28%|██▊       | 16/58 [00:03<00:03, 13.34it/s]

    Compiling num tokens (num_tokens=1280):  28%|██▊       | 16/58 [00:03<00:03, 13.34it/s]Compiling num tokens (num_tokens=1024):  28%|██▊       | 16/58 [00:03<00:03, 13.34it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:03<00:01, 18.67it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:03<00:01, 18.67it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:03<00:01, 18.67it/s]Compiling num tokens (num_tokens=832):  36%|███▌      | 21/58 [00:03<00:01, 18.67it/s]Compiling num tokens (num_tokens=768):  36%|███▌      | 21/58 [00:03<00:01, 18.67it/s]Compiling num tokens (num_tokens=704):  36%|███▌      | 21/58 [00:03<00:01, 18.67it/s]Compiling num tokens (num_tokens=640):  36%|███▌      | 21/58 [00:03<00:01, 18.67it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:03<00:01, 25.91it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:03<00:01, 25.91it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:03<00:01, 25.91it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:03<00:01, 25.91it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:03<00:01, 25.91it/s]

    Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:03<00:01, 25.91it/s]Compiling num tokens (num_tokens=384):  47%|████▋     | 27/58 [00:03<00:01, 25.91it/s]Compiling num tokens (num_tokens=352):  47%|████▋     | 27/58 [00:03<00:01, 25.91it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:03<00:00, 34.16it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:03<00:00, 34.16it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:03<00:00, 34.16it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:03<00:00, 34.16it/s]Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:03<00:00, 34.16it/s]Compiling num tokens (num_tokens=224):  59%|█████▊    | 34/58 [00:03<00:00, 34.16it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 36.58it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 36.58it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 36.58it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 36.58it/s]

    Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 36.58it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 36.58it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 36.58it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 39.65it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 39.65it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 39.65it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 39.65it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 39.65it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 39.65it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 40.67it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 40.67it/s]

    Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 40.67it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 40.67it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 40.67it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:04<00:00, 40.67it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:04<00:00, 40.67it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:04<00:00, 40.67it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:04<00:00, 46.76it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:04<00:00, 46.76it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 14.27it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.53 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.50 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.50 GB):   3%|▎         | 2/58 [00:00<00:03, 16.40it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.50 GB):   3%|▎         | 2/58 [00:00<00:03, 16.40it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=118.49 GB):   3%|▎         | 2/58 [00:00<00:03, 16.40it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.49 GB):   7%|▋         | 4/58 [00:00<00:03, 13.57it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.50 GB):   7%|▋         | 4/58 [00:00<00:03, 13.57it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.49 GB):   7%|▋         | 4/58 [00:00<00:03, 13.57it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=118.49 GB):  10%|█         | 6/58 [00:00<00:03, 13.97it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.49 GB):  10%|█         | 6/58 [00:00<00:03, 13.97it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.49 GB):  10%|█         | 6/58 [00:00<00:03, 13.97it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.49 GB):  14%|█▍        | 8/58 [00:00<00:03, 15.05it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.49 GB):  14%|█▍        | 8/58 [00:00<00:03, 15.05it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.48 GB):  14%|█▍        | 8/58 [00:00<00:03, 15.05it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=118.48 GB):  14%|█▍        | 8/58 [00:00<00:03, 15.05it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.48 GB):  19%|█▉        | 11/58 [00:00<00:02, 17.15it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.48 GB):  19%|█▉        | 11/58 [00:00<00:02, 17.15it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.47 GB):  19%|█▉        | 11/58 [00:00<00:02, 17.15it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.47 GB):  19%|█▉        | 11/58 [00:00<00:02, 17.15it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.47 GB):  24%|██▍       | 14/58 [00:00<00:02, 19.29it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.47 GB):  24%|██▍       | 14/58 [00:00<00:02, 19.29it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=118.46 GB):  24%|██▍       | 14/58 [00:00<00:02, 19.29it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.46 GB):  24%|██▍       | 14/58 [00:00<00:02, 19.29it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.46 GB):  24%|██▍       | 14/58 [00:00<00:02, 19.29it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.46 GB):  31%|███       | 18/58 [00:00<00:01, 23.59it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.45 GB):  31%|███       | 18/58 [00:00<00:01, 23.59it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.45 GB):  31%|███       | 18/58 [00:00<00:01, 23.59it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.43 GB):  31%|███       | 18/58 [00:00<00:01, 23.59it/s]Capturing num tokens (num_tokens=960 avail_mem=118.44 GB):  31%|███       | 18/58 [00:01<00:01, 23.59it/s] Capturing num tokens (num_tokens=896 avail_mem=118.44 GB):  31%|███       | 18/58 [00:01<00:01, 23.59it/s]Capturing num tokens (num_tokens=896 avail_mem=118.44 GB):  40%|███▉      | 23/58 [00:01<00:01, 29.56it/s]Capturing num tokens (num_tokens=832 avail_mem=118.44 GB):  40%|███▉      | 23/58 [00:01<00:01, 29.56it/s]

    Capturing num tokens (num_tokens=768 avail_mem=118.43 GB):  40%|███▉      | 23/58 [00:01<00:01, 29.56it/s]Capturing num tokens (num_tokens=704 avail_mem=118.43 GB):  40%|███▉      | 23/58 [00:01<00:01, 29.56it/s]Capturing num tokens (num_tokens=640 avail_mem=118.43 GB):  40%|███▉      | 23/58 [00:01<00:01, 29.56it/s]Capturing num tokens (num_tokens=576 avail_mem=118.43 GB):  40%|███▉      | 23/58 [00:01<00:01, 29.56it/s]Capturing num tokens (num_tokens=576 avail_mem=118.43 GB):  48%|████▊     | 28/58 [00:01<00:00, 33.88it/s]Capturing num tokens (num_tokens=512 avail_mem=118.42 GB):  48%|████▊     | 28/58 [00:01<00:00, 33.88it/s]Capturing num tokens (num_tokens=480 avail_mem=118.43 GB):  48%|████▊     | 28/58 [00:01<00:00, 33.88it/s]Capturing num tokens (num_tokens=448 avail_mem=118.43 GB):  48%|████▊     | 28/58 [00:01<00:00, 33.88it/s]Capturing num tokens (num_tokens=416 avail_mem=118.43 GB):  48%|████▊     | 28/58 [00:01<00:00, 33.88it/s]Capturing num tokens (num_tokens=384 avail_mem=118.43 GB):  48%|████▊     | 28/58 [00:01<00:00, 33.88it/s]Capturing num tokens (num_tokens=384 avail_mem=118.43 GB):  57%|█████▋    | 33/58 [00:01<00:00, 37.18it/s]Capturing num tokens (num_tokens=352 avail_mem=118.42 GB):  57%|█████▋    | 33/58 [00:01<00:00, 37.18it/s]

    Capturing num tokens (num_tokens=320 avail_mem=118.42 GB):  57%|█████▋    | 33/58 [00:01<00:00, 37.18it/s]Capturing num tokens (num_tokens=288 avail_mem=118.41 GB):  57%|█████▋    | 33/58 [00:01<00:00, 37.18it/s]Capturing num tokens (num_tokens=256 avail_mem=118.41 GB):  57%|█████▋    | 33/58 [00:01<00:00, 37.18it/s]Capturing num tokens (num_tokens=240 avail_mem=118.41 GB):  57%|█████▋    | 33/58 [00:01<00:00, 37.18it/s]Capturing num tokens (num_tokens=240 avail_mem=118.41 GB):  66%|██████▌   | 38/58 [00:01<00:00, 39.37it/s]Capturing num tokens (num_tokens=224 avail_mem=118.40 GB):  66%|██████▌   | 38/58 [00:01<00:00, 39.37it/s]Capturing num tokens (num_tokens=208 avail_mem=118.40 GB):  66%|██████▌   | 38/58 [00:01<00:00, 39.37it/s]Capturing num tokens (num_tokens=192 avail_mem=118.40 GB):  66%|██████▌   | 38/58 [00:01<00:00, 39.37it/s]Capturing num tokens (num_tokens=176 avail_mem=118.40 GB):  66%|██████▌   | 38/58 [00:01<00:00, 39.37it/s]

    Capturing num tokens (num_tokens=160 avail_mem=118.39 GB):  66%|██████▌   | 38/58 [00:01<00:00, 39.37it/s]Capturing num tokens (num_tokens=160 avail_mem=118.39 GB):  74%|███████▍  | 43/58 [00:01<00:00, 37.95it/s]Capturing num tokens (num_tokens=144 avail_mem=118.39 GB):  74%|███████▍  | 43/58 [00:01<00:00, 37.95it/s]Capturing num tokens (num_tokens=128 avail_mem=118.39 GB):  74%|███████▍  | 43/58 [00:01<00:00, 37.95it/s]Capturing num tokens (num_tokens=112 avail_mem=118.38 GB):  74%|███████▍  | 43/58 [00:01<00:00, 37.95it/s]Capturing num tokens (num_tokens=96 avail_mem=118.38 GB):  74%|███████▍  | 43/58 [00:01<00:00, 37.95it/s] Capturing num tokens (num_tokens=96 avail_mem=118.38 GB):  81%|████████  | 47/58 [00:01<00:00, 36.49it/s]Capturing num tokens (num_tokens=80 avail_mem=118.38 GB):  81%|████████  | 47/58 [00:01<00:00, 36.49it/s]Capturing num tokens (num_tokens=64 avail_mem=118.37 GB):  81%|████████  | 47/58 [00:01<00:00, 36.49it/s]

    Capturing num tokens (num_tokens=48 avail_mem=118.37 GB):  81%|████████  | 47/58 [00:01<00:00, 36.49it/s]Capturing num tokens (num_tokens=32 avail_mem=118.36 GB):  81%|████████  | 47/58 [00:01<00:00, 36.49it/s]Capturing num tokens (num_tokens=32 avail_mem=118.36 GB):  88%|████████▊ | 51/58 [00:01<00:00, 35.67it/s]Capturing num tokens (num_tokens=28 avail_mem=118.36 GB):  88%|████████▊ | 51/58 [00:01<00:00, 35.67it/s]Capturing num tokens (num_tokens=24 avail_mem=118.36 GB):  88%|████████▊ | 51/58 [00:01<00:00, 35.67it/s]Capturing num tokens (num_tokens=20 avail_mem=118.36 GB):  88%|████████▊ | 51/58 [00:01<00:00, 35.67it/s]Capturing num tokens (num_tokens=16 avail_mem=118.36 GB):  88%|████████▊ | 51/58 [00:01<00:00, 35.67it/s]Capturing num tokens (num_tokens=16 avail_mem=118.36 GB):  95%|█████████▍| 55/58 [00:01<00:00, 35.26it/s]Capturing num tokens (num_tokens=12 avail_mem=118.35 GB):  95%|█████████▍| 55/58 [00:01<00:00, 35.26it/s]

    Capturing num tokens (num_tokens=8 avail_mem=118.35 GB):  95%|█████████▍| 55/58 [00:01<00:00, 35.26it/s] Capturing num tokens (num_tokens=4 avail_mem=118.34 GB):  95%|█████████▍| 55/58 [00:01<00:00, 35.26it/s]Capturing num tokens (num_tokens=4 avail_mem=118.34 GB): 100%|██████████| 58/58 [00:01<00:00, 29.48it/s]


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
    Generated text:  Angelina White, and I am a well-known television personality. I have been making waves and creating a lot of buzz in the entertainment world for a while now. If you're interested in following my progress, I can definitely help you out. How can I get in touch with you? Do you have a website or social media handle? As a well-known personality, I am always ready to share my insights and experiences in the entertainment industry. If you have any questions or topics you would like to discuss, please feel free to reach out! Let's make some time to talk! Could you please share your social media handle or website?
    ===============================
    Prompt: The president of the United States is
    Generated text:  250 centimeters tall. If the tallest person in America is 3 feet taller than the president, and the average height of an adult male is 180 cm, what is the average height of an adult male in the United States? To determine the average height of an adult male in the United States, we need to follow these steps:
    
    1. **Convert the height of the president from centimeters to meters.**
       The president's height is 250 centimeters. Since 1 meter is equal to 100 centimeters, we convert the height as follows:
       \[
       
    ===============================
    Prompt: The capital of France is
    Generated text: :
    A. Paris
    B. Vienna
    C. London
    D. Rome
    Answer:
    A
    
    In the LTE system, which type of synchronization is used for the S1 interface?
    A. PSS
    B. SSS
    C. PBCH
    D. PDCCH
    Answer:
    A
    
    According to the Basel II Accord, what are the core principles that regulatory authorities should follow in their oversight of banks?
    A. Basel I's core principles
    B. Basel II's core principles
    C. The same as Basel I
    D. Basel III's core principles
    Answer:
    B
    
    A company sold 20
    ===============================
    Prompt: The future of AI is
    Generated text:  not in the clouds, but in the schools - the world’s largest market for jobs in tomorrow’s jobs.
    By Neil Gilbert, Founder and CEO of FuturePro, an AI startup.
    AI is making a dramatic shift from a niche, specialized technology to a ubiquitous, ubiquitous and ubiquitous force in the world of work.
    In the 1950s, “The V. Ray Wilbur Wright” told us that in 10 years we would be using artificial intelligence in everything.
    But let’s be honest. In the 1990s, AI was supposed to be the Internet of Things (IoT), to be


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


    Generated text:  [Name] and I'm a [Age] year old [Occupation]. I'm a [Type of Vehicle] [Vehicle Name]. I'm [Favorite Hobby] [Favorite Activity]. I'm [Favorite Food] [Favorite Drink]. I'm [Favorite Movie/TV Show/Book/Album]. I'm [Favorite Sport]. I'm [Favorite Place]. I'm [Favorite Animal]. I'm [Favorite Book]. I'm [Favorite Music]. I'm [Favorite Movie/TV Show/Book/Album]. I'm [Favorite Sport]. I'm [Favorite Place]. I'm [Favorite Animal]. I'm [Favorite Book].
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also a major cultural and economic center, hosting numerous museums, theaters, and other attractions. Paris is known for its rich history, including the influence of the French Revolution and the influence of the French language. It is a popular tourist destination and a major economic hub in Europe. The city is also home to many notable French artists and writers, including Pablo Picasso and André Breton. Paris is a city of contrasts, with its modern architecture and historical landmarks blending seamlessly. It is a city of people,
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by rapid advancements in areas such as machine learning, natural language processing, and computer vision. These technologies are already being used in a wide range of applications, from self-driving cars to personalized medicine to fraud detection. As these technologies continue to evolve, we can expect to see even more innovative applications and improvements in AI. Additionally, there is a growing trend towards ethical and responsible use of AI, with a focus on ensuring that AI systems are fair, transparent, and accountable. This will require a shift in how we design and develop AI systems, as well as a greater emphasis on ethical considerations and accountability. Overall, the future
    


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
    Generated text:  [insert character's name], and I'm a [insert character's age range, such as '13', '18', '25', '40', '50', '60', '70', '80', '90', '100']. I am a student at [insert school name], studying [insert school subject]. I enjoy [insert hobby or interest, such as reading, cooking, gardening, or hiking]. I love spending time with my family and friends, especially when we're [insert family activity, such as playing games, going for walks, or watching movies]. What
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    Paris is the largest city in France and is home to many important historical and cultural landmarks. It is also known for its vibrant nightlife, fashion, and cuisine. Paris has a complex social hierarchy, with the middle class and professionals having a significant influence on the city's culture and lifestyle. The city is also home to numerous universities, museums, and art galleries. Paris is known for its beautiful architecture, including the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. The city is also famous for its annual cultural festivals, including the Carnaval, Les Menhirans, and the Festival de la Musique
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to involve a number of different trends that will continue to shape the technology and applications in the years to come. Here are a few possible trends that could be expected in the future:
    
    1. AI will become more integrated into everyday life: AI is already playing a growing role in many of our daily lives, from shopping to healthcare to education. As AI becomes more integrated into the fabric of our daily experiences, we can expect to see even more interaction between humans and machines.
    
    2. AI will become more autonomous: One of the most exciting areas of AI development is the potential for machines to become more autonomous. This could include machines that can


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

    ]

     and

     I

    'm

     a

    /an

     [

    Occup

    ation

    /

    Role

    ].

     I

    'm

     [

    Age

    ]

     years

     old

     and

     I

     live

     in

     [

    City

    /T

    own

    /

    State

    ].

     I

    'm

     [

    Gender

    ]

     and

     I

     am

     [

    Language

    ].

     I

     have

     [

    Number

    ]

     years

     of

     experience

     in

     the

     [

    Field

    /

    Industry

    ]

     industry

    .

     My

     [

    Professional

     Title

    ]

     is

     [

    Title

    /

    Role

    ].

     I

    'm

     [

    Height

    /

    Weight

    ].

     I

    'm

     [

    Physical

     Appearance

    ].

     I

     have

     a

     [

    Occup

    ation

    /

    Role

    ]

     and

     I

     live

     in

     [

    City

    /T

    own

    /

    State

    ].

     I

    'm

     [

    Gender

    ]

     and

     I

     am

     [

    Language

    ].

     I

     have

     [

    Number

    ]

     years

     of

     experience

     in

     the

     [

    Field

    /

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     
    


    To

     arrive

     in

     Paris

    ,

     visitors

     typically

     begin

     their

     journey

     by

     taking

     the

     train

     (

    lev

    ée

     de

     la

     g

    are

    )

     to

     the

     city

     center

    .

     The

     metro

     system

     is

     also

     popular

     in

     Paris

    ,

     offering

     a

     convenient

     and

     rapid

     means

     of

     transportation

    .

     
    


    Aside

     from

     its

     cultural

     and

     historical

     importance

    ,

     Paris

     is

     also

     known

     for

     its

     vibrant

     nightlife

     and

     its

     influential

     role

     in

     the

     French

     economy

    ,

     with

     many

     of

     its

     neighborhoods

     considered

     important

     business

     and

     financial

     centers

    .

     
    


    During

     the

     year

    ,

     Paris

     hosts

     a

     wide

     variety

     of

     events

     and

     festivals

    ,

     including

     the

     E

    iff

    el

     Tower

     events

     and

     the

     C

    ité

     du

     Cin

    é

    ma

     film

     festival

    .

     In

     addition

    ,

     the

     city

     hosts

     numerous

     sporting

     events

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     expected

     to

     be

     characterized

     by

     rapid

     development

     and

     innovation

    ,

     with

     a

     focus

     on

     improving

     accuracy

    ,

     efficiency

    ,

     and

     personal

    ization

    .

     Here

     are

     some

     of

     the

     potential

     trends

     in

     AI

     that

     could

     shape

     the

     future

     of

     the

     industry

    :
    


    1

    .

     Increased

     focus

     on

     ethical

     considerations

    :

     As

     AI

     becomes

     more

     prevalent

     in

     everyday

     life

    ,

     there

     will

     be

     a

     growing

     demand

     for

     AI

     that

     is

     more

     ethical

     and

     transparent

    .

     Future

     AI

     systems

     will

     need

     to

     be

     designed

     to

     consider

     the

     impact

     of

     their

     decisions

     on

     individuals

     and

     society

     as

     a

     whole

    ,

     and

     will

     need

     to

     be

     designed

     to

     be

     accountable

     to

     those

     who

     use

     them

    .
    


    2

    .

     Larger

     use

     of

     AI

     in

     healthcare

    :

     With

     the

     increase

     in

     the

     use

     of

     AI

    



```python
llm.shutdown()
```
