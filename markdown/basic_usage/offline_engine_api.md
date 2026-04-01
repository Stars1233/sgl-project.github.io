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


    2026-04-01 06:23:19.797 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 06:23:19] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 06:23:19.797 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 06:23:19] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 06:23:19.798 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 06:23:19] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 06:23:19.798 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 06:23:19] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 06:23:19.798 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 06:23:19] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.95it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.94it/s]


    2026-04-01 06:23:22,695 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 06:23:22] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:38,  2.78s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:38,  2.78s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<01:07,  1.21s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<01:07,  1.21s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<01:07,  1.21s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:26,  2.05it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:26,  2.05it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:03<00:26,  2.05it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:03<00:26,  2.05it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:03<00:11,  4.27it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:03<00:11,  4.27it/s]Compiling num tokens (num_tokens=4096):  12%|█▏        | 7/58 [00:03<00:11,  4.27it/s]

    Compiling num tokens (num_tokens=3840):  12%|█▏        | 7/58 [00:03<00:11,  4.27it/s]Compiling num tokens (num_tokens=3584):  12%|█▏        | 7/58 [00:03<00:11,  4.27it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:03<00:05,  7.94it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:03<00:05,  7.94it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:03<00:05,  7.94it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:03<00:05,  7.94it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:03<00:05,  7.94it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:03<00:05,  7.94it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:03<00:05,  7.94it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:03<00:05,  7.94it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:03<00:05,  7.94it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:02, 17.06it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:02, 17.06it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:02, 17.06it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:02, 17.06it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:02, 17.06it/s]

    Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:02, 17.06it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:02, 17.06it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:02, 17.06it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 24.76it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 24.76it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 24.76it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 24.76it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 24.76it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 24.76it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:03<00:01, 24.76it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:03<00:01, 24.76it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:00, 32.36it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:00, 32.36it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:00, 32.36it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:00, 32.36it/s]

    Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:00, 32.36it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:00, 32.36it/s]Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:03<00:00, 32.36it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 37.15it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 37.15it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 37.15it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 37.15it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 37.15it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 37.15it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 37.15it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 40.40it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 40.40it/s]

    Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 40.40it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 40.40it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 40.40it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 40.40it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 40.40it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 41.80it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 41.80it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 41.80it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 41.80it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 41.80it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:04<00:00, 41.80it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:04<00:00, 41.80it/s] 

    Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:04<00:00, 41.80it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 47.81it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 14.33it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.79 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.79 GB):   2%|▏         | 1/58 [00:00<00:06,  8.91it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.76 GB):   2%|▏         | 1/58 [00:00<00:06,  8.91it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.76 GB):   2%|▏         | 1/58 [00:00<00:06,  8.91it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=118.76 GB):   5%|▌         | 3/58 [00:00<00:03, 14.43it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.75 GB):   5%|▌         | 3/58 [00:00<00:03, 14.43it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.76 GB):   5%|▌         | 3/58 [00:00<00:03, 14.43it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.75 GB):   5%|▌         | 3/58 [00:00<00:03, 14.43it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.75 GB):  10%|█         | 6/58 [00:00<00:02, 19.55it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.75 GB):  10%|█         | 6/58 [00:00<00:02, 19.55it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.75 GB):  10%|█         | 6/58 [00:00<00:02, 19.55it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.75 GB):  10%|█         | 6/58 [00:00<00:02, 19.55it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=118.74 GB):  10%|█         | 6/58 [00:00<00:02, 19.55it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.74 GB):  17%|█▋        | 10/58 [00:00<00:01, 25.16it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.58 GB):  17%|█▋        | 10/58 [00:00<00:01, 25.16it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.57 GB):  17%|█▋        | 10/58 [00:00<00:01, 25.16it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.57 GB):  17%|█▋        | 10/58 [00:00<00:01, 25.16it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.57 GB):  17%|█▋        | 10/58 [00:00<00:01, 25.16it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.57 GB):  24%|██▍       | 14/58 [00:00<00:01, 28.23it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.35 GB):  24%|██▍       | 14/58 [00:00<00:01, 28.23it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.09 GB):  24%|██▍       | 14/58 [00:00<00:01, 28.23it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=118.09 GB):  24%|██▍       | 14/58 [00:00<00:01, 28.23it/s]Capturing num tokens (num_tokens=1792 avail_mem=117.91 GB):  24%|██▍       | 14/58 [00:00<00:01, 28.23it/s]Capturing num tokens (num_tokens=1792 avail_mem=117.91 GB):  31%|███       | 18/58 [00:00<00:01, 26.41it/s]Capturing num tokens (num_tokens=1536 avail_mem=117.90 GB):  31%|███       | 18/58 [00:00<00:01, 26.41it/s]Capturing num tokens (num_tokens=1280 avail_mem=117.90 GB):  31%|███       | 18/58 [00:00<00:01, 26.41it/s]Capturing num tokens (num_tokens=1024 avail_mem=117.88 GB):  31%|███       | 18/58 [00:00<00:01, 26.41it/s]Capturing num tokens (num_tokens=960 avail_mem=117.89 GB):  31%|███       | 18/58 [00:00<00:01, 26.41it/s] 

    Capturing num tokens (num_tokens=960 avail_mem=117.89 GB):  38%|███▊      | 22/58 [00:00<00:01, 27.79it/s]Capturing num tokens (num_tokens=896 avail_mem=117.89 GB):  38%|███▊      | 22/58 [00:00<00:01, 27.79it/s]Capturing num tokens (num_tokens=832 avail_mem=117.89 GB):  38%|███▊      | 22/58 [00:00<00:01, 27.79it/s]Capturing num tokens (num_tokens=768 avail_mem=117.88 GB):  38%|███▊      | 22/58 [00:00<00:01, 27.79it/s]Capturing num tokens (num_tokens=704 avail_mem=117.88 GB):  38%|███▊      | 22/58 [00:00<00:01, 27.79it/s]Capturing num tokens (num_tokens=704 avail_mem=117.88 GB):  45%|████▍     | 26/58 [00:00<00:01, 29.28it/s]Capturing num tokens (num_tokens=640 avail_mem=117.88 GB):  45%|████▍     | 26/58 [00:00<00:01, 29.28it/s]Capturing num tokens (num_tokens=576 avail_mem=117.88 GB):  45%|████▍     | 26/58 [00:01<00:01, 29.28it/s]Capturing num tokens (num_tokens=512 avail_mem=117.86 GB):  45%|████▍     | 26/58 [00:01<00:01, 29.28it/s]

    Capturing num tokens (num_tokens=480 avail_mem=117.88 GB):  45%|████▍     | 26/58 [00:01<00:01, 29.28it/s]Capturing num tokens (num_tokens=480 avail_mem=117.88 GB):  52%|█████▏    | 30/58 [00:01<00:00, 30.43it/s]Capturing num tokens (num_tokens=448 avail_mem=117.88 GB):  52%|█████▏    | 30/58 [00:01<00:00, 30.43it/s]Capturing num tokens (num_tokens=416 avail_mem=117.88 GB):  52%|█████▏    | 30/58 [00:01<00:00, 30.43it/s]Capturing num tokens (num_tokens=384 avail_mem=117.87 GB):  52%|█████▏    | 30/58 [00:01<00:00, 30.43it/s]Capturing num tokens (num_tokens=352 avail_mem=117.87 GB):  52%|█████▏    | 30/58 [00:01<00:00, 30.43it/s]Capturing num tokens (num_tokens=352 avail_mem=117.87 GB):  59%|█████▊    | 34/58 [00:01<00:00, 31.29it/s]Capturing num tokens (num_tokens=320 avail_mem=117.86 GB):  59%|█████▊    | 34/58 [00:01<00:00, 31.29it/s]Capturing num tokens (num_tokens=288 avail_mem=117.86 GB):  59%|█████▊    | 34/58 [00:01<00:00, 31.29it/s]

    Capturing num tokens (num_tokens=256 avail_mem=117.86 GB):  59%|█████▊    | 34/58 [00:01<00:00, 31.29it/s]Capturing num tokens (num_tokens=240 avail_mem=117.86 GB):  59%|█████▊    | 34/58 [00:01<00:00, 31.29it/s]Capturing num tokens (num_tokens=240 avail_mem=117.86 GB):  66%|██████▌   | 38/58 [00:01<00:00, 33.21it/s]Capturing num tokens (num_tokens=224 avail_mem=117.85 GB):  66%|██████▌   | 38/58 [00:01<00:00, 33.21it/s]Capturing num tokens (num_tokens=208 avail_mem=117.85 GB):  66%|██████▌   | 38/58 [00:01<00:00, 33.21it/s]Capturing num tokens (num_tokens=192 avail_mem=117.85 GB):  66%|██████▌   | 38/58 [00:01<00:00, 33.21it/s]Capturing num tokens (num_tokens=176 avail_mem=117.85 GB):  66%|██████▌   | 38/58 [00:01<00:00, 33.21it/s]Capturing num tokens (num_tokens=176 avail_mem=117.85 GB):  72%|███████▏  | 42/58 [00:01<00:00, 33.85it/s]Capturing num tokens (num_tokens=160 avail_mem=117.84 GB):  72%|███████▏  | 42/58 [00:01<00:00, 33.85it/s]Capturing num tokens (num_tokens=144 avail_mem=117.84 GB):  72%|███████▏  | 42/58 [00:01<00:00, 33.85it/s]

    Capturing num tokens (num_tokens=128 avail_mem=118.72 GB):  72%|███████▏  | 42/58 [00:01<00:00, 33.85it/s]Capturing num tokens (num_tokens=112 avail_mem=118.72 GB):  72%|███████▏  | 42/58 [00:01<00:00, 33.85it/s]Capturing num tokens (num_tokens=112 avail_mem=118.72 GB):  79%|███████▉  | 46/58 [00:01<00:00, 24.85it/s]Capturing num tokens (num_tokens=96 avail_mem=118.72 GB):  79%|███████▉  | 46/58 [00:01<00:00, 24.85it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=137.27 GB):  79%|███████▉  | 46/58 [00:01<00:00, 24.85it/s]Capturing num tokens (num_tokens=64 avail_mem=137.26 GB):  79%|███████▉  | 46/58 [00:01<00:00, 24.85it/s]Capturing num tokens (num_tokens=64 avail_mem=137.26 GB):  84%|████████▍ | 49/58 [00:01<00:00, 21.32it/s]Capturing num tokens (num_tokens=48 avail_mem=137.26 GB):  84%|████████▍ | 49/58 [00:01<00:00, 21.32it/s]Capturing num tokens (num_tokens=32 avail_mem=137.25 GB):  84%|████████▍ | 49/58 [00:01<00:00, 21.32it/s]Capturing num tokens (num_tokens=28 avail_mem=137.25 GB):  84%|████████▍ | 49/58 [00:01<00:00, 21.32it/s]Capturing num tokens (num_tokens=24 avail_mem=137.25 GB):  84%|████████▍ | 49/58 [00:01<00:00, 21.32it/s]Capturing num tokens (num_tokens=20 avail_mem=137.24 GB):  84%|████████▍ | 49/58 [00:02<00:00, 21.32it/s]Capturing num tokens (num_tokens=20 avail_mem=137.24 GB):  93%|█████████▎| 54/58 [00:02<00:00, 26.39it/s]Capturing num tokens (num_tokens=16 avail_mem=137.24 GB):  93%|█████████▎| 54/58 [00:02<00:00, 26.39it/s]Capturing num tokens (num_tokens=12 avail_mem=137.24 GB):  93%|█████████▎| 54/58 [00:02<00:00, 26.39it/s]

    Capturing num tokens (num_tokens=8 avail_mem=137.24 GB):  93%|█████████▎| 54/58 [00:02<00:00, 26.39it/s] Capturing num tokens (num_tokens=4 avail_mem=137.23 GB):  93%|█████████▎| 54/58 [00:02<00:00, 26.39it/s]Capturing num tokens (num_tokens=4 avail_mem=137.23 GB): 100%|██████████| 58/58 [00:02<00:00, 29.25it/s]Capturing num tokens (num_tokens=4 avail_mem=137.23 GB): 100%|██████████| 58/58 [00:02<00:00, 27.22it/s]


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
    Generated text:  Braden.
    
    I'm 17, and I'm an avid bookworm who has been reading for years. I have always been passionate about history, so I have always wanted to work on a book about it. I'm currently reading a book called "The Great Age of Imperial Power" by Alexander Stenger.
    
    Can you tell me about the book "The Great Age of Imperial Power"? What does it have to say about the history of the time? Also, I'm curious to know more about the historical background of the time. 
    Sure, thank you for your interest in the book! "The Great Age of Imperial Power"
    ===============================
    Prompt: The president of the United States is
    Generated text:  trying to decide how many military bases to have. He has 100 bases in the plan. Each base costs $10 million to build. However, the cost to maintain each base is $4 million per year. If he has 5 bases that have been in service for more than 10 years, how much money would he have spent on military bases?
    
    To determine the total cost of the military bases, we need to consider both the initial cost of building the bases and the annual maintenance cost. Here's a step-by-step breakdown:
    
    1. **Calculate the number of bases that have been in service for more than
    ===============================
    Prompt: The capital of France is
    Generated text:  _______.
    A. Paris
    B. Strasbourg
    C. Toulouse
    D. Lyon
    Answer:
    A
    
    The component that can be seen under the microscope is ____
    A. Chorionic villi
    B. Trophoblast
    C. Trophoblast cells
    D. Blastocyst
    E. Oocyte
    Answer:
    C
    
    The component that can be seen under the microscope is ____
    A. Chorion
    B. Chorionic villi
    C. Trophoblast
    D. Trophoblast cells
    E. Oocyte
    Answer:
    C
    
    The component that
    ===============================
    Prompt: The future of AI is
    Generated text:  bright, but it is also complex and challenging. The integration of AI systems with the real world is a major challenge. In 2017, the National Institutes of Health (NIH) released a report, "AI and Biomedical Research," emphasizing the importance of interdisciplinary collaboration and the need for research focused on addressing the intersection of AI and medical research.
    One way to address this challenge is to develop a comprehensive understanding of the complex interactions between AI and the real world, including how AI systems interact with medical devices, patients, and healthcare providers. This requires a multidisciplinary approach that includes researchers from various fields, such as medicine


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you and learn more about you. What
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as the City of Light, and is located in the south of the country. It is the largest city in France and the second-largest city in the European Union. Paris is known for its rich history, art, and culture, and is home to many famous landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. It is also a major transportation hub, with many major highways and rail lines passing through the city. Paris is a popular tourist destination and is home to many cultural institutions, including the Louvre Museum and the Notre-Dame Cathedral. The city is also known for its
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction. Here are some of the most likely trends:
    
    1. Increased focus on ethical considerations: As AI becomes more integrated into our daily lives, there will be a growing emphasis on ethical considerations. This will include issues such as bias, transparency, accountability, and privacy.
    
    2. Development of more advanced AI systems: As AI technology continues to advance, we are likely to see the development of more advanced AI systems that can perform tasks that were previously considered impossible. This could include tasks such as autonomous vehicles, medical diagnosis, and personalized education.
    
    3. Integration
    


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
    Generated text:  [Name] and I am a [Title] at [Company Name]. I am excited to meet you and I look forward to learning about your interests and what you are looking for in a job. Are there any particular skills or qualities that you have that I should be aware of? As an AI language model, I can also offer some general tips on how to become a more effective job seeker, such as making sure you have a clear understanding of your goals and what you are looking for in a job, and developing a strong resume and cover letter to showcase your skills and experience. What would you like to know more about? [Name
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, located on the Seine River in the heart of the French countryside. It is the most populous city in France and the seventh-largest city in the world. The city is known for its historical architecture, vibrant culture, and iconic landmarks such as the Louvre Museum and Notre-Dame Cathedral. It is also known for its annual celebration of the Eiffel Tower and its annual fashion week. Paris is a major transportation hub and has a reputation for being safe and safe for foreigners, including tourists. It is the seat of the French government and the country's main cultural and economic center. The city is also home to many of the
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  uncertain, but it is projected to continue to evolve rapidly. Here are some of the possible trends that could shape the future of AI:
    
    1. Advanced machine learning and deep learning: AI experts are predicting that machine learning and deep learning will continue to advance, with more complex algorithms and algorithms that can learn from large datasets. This will make AI systems more powerful and capable of solving complex problems.
    
    2. Increased use of AI in healthcare: AI is already being used to help doctors diagnose and treat diseases. As AI technology continues to improve, it may become even more useful in healthcare. For example, AI could be used to analyze medical images and


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

    ],

     and

     I

     am

     [

    Age

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

     industry

    .

     I

     have

     a

     background

     in

     [

    field

     of

     expertise

    ],

     and

     I

     am

     dedicated

     to

     [

    focus

     area

    ].

     I

     love

     [

    reason

     why

     I

     enjoy

     my

     work

    ]

     and

     I

     strive

     to

     be

     [

    level

     of

     dedication

    ].

     What

     is

     your

     work

     style

    ?

     I

    'm

     always

     on

     the

     lookout

     for

     new

     opportunities

     and

     always

     eager

     to

     learn

    .

     I

     enjoy

     spending

     time

     outdoors

     and

     traveling

    ,

     and

     I

    'm

     always

     looking

     for

     new

     experiences

     to

     share

     with

     my

     coworkers

    .

     I

    'm

     a

     [

    respons

    ibility

     level

    ]

     person

    ,

     always

     willing

     to

     help

     those

     in

     need

    .

     What

     are

     your

     hobbies

    ?

     I

     love

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .


    Paris

     is

     the

     largest

     city

     and

     the

     capital

     of

     France

    ,

     located

     on

     the

     Se

    ine

     River

     in

     the

     region

     of

     Paris

    .

     It

     is

     also

     the

     cultural

     and

     political

     center

     of

     France

    ,

     home

     to

     numerous

     museums

    ,

     pal

    aces

    ,

     and

     other

     historic

     landmarks

    .

     Paris

     is

     known

     for

     its

     rich

     history

    ,

     art

    ,

     and

     cuisine

    ,

     and

     is

     a

     world

    -ren

    owned

     destination

     for

     tourists

     and

     locals

     alike

    .

     The

     city

     is

     also

     home

     to

     a

     diverse

     and

     multicultural

     population

    ,

     with

     many

     French

     and

     international

     residents

    .

     Paris

     has

     a

     rich

     history

     dating

     back

     to

     the

     

    7

    th

     century

     and

     is

     a

     major

     center

     of

     learning

     and

     culture

     in

     Europe

    ,

     with

     many

     notable

     institutions

     such

     as

     the

     Lou

    vre

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     expected

     to

     continue

     to

     evolve

     rapidly

    ,

     driven

     by

     a

     number

     of

     key

     trends

    :
    


    1

    .

     Increased

     Integration

     with

     Other

     Technologies

    :

     AI

     will

     continue

     to

     be

     integrated

     with

     other

     technologies

     such

     as

     blockchain

    ,

     IoT

    ,

     and

     quantum

     computing

    ,

     creating

     a

     more

     complex

     and

     dynamic

     ecosystem

    .
    


    2

    .

     Automation

    :

     As

     AI

     becomes

     more

     sophisticated

    ,

     it

     will

     be

     able

     to

     perform

     tasks

     that

     require

     human

    -like

     intelligence

    ,

     leading

     to

     automation

     in

     various

     sectors

    .
    


    3

    .

     Personal

    ization

    :

     AI

     will

     be

     used

     to

     analyze

     data

     and

     provide

     personalized

     recommendations

     to

     users

    ,

     leading

     to

     more

     efficient

     and

     effective

     user

     experiences

    .
    


    4

    .

     Eth

    ical

     Consider

    ations

    :

     As

     AI

     becomes

     more

     pervasive

    ,

     there

     will

     be

     increasing

     concern

     about

     issues

    



```python
llm.shutdown()
```
