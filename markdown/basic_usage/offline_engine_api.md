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


    2026-04-01 12:04:30.675 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 12:04:30] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 12:04:30.675 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 12:04:30] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 12:04:30.675 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 12:04:30] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 12:04:30.675 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 12:04:30] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 12:04:30.675 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 12:04:30] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.88it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.87it/s]


    2026-04-01 12:04:33,389 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 12:04:33] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:36,  2.75s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:36,  2.75s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:36,  2.75s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:36,  2.75s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:29,  1.83it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:29,  1.83it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:29,  1.83it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:29,  1.83it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:29,  1.83it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:02<00:11,  4.27it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:02<00:11,  4.27it/s]Compiling num tokens (num_tokens=3840):  14%|█▍        | 8/58 [00:03<00:11,  4.27it/s]Compiling num tokens (num_tokens=3584):  14%|█▍        | 8/58 [00:03<00:11,  4.27it/s]Compiling num tokens (num_tokens=3328):  14%|█▍        | 8/58 [00:03<00:11,  4.27it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:03<00:06,  7.24it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:03<00:06,  7.24it/s]Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:03<00:06,  7.24it/s]Compiling num tokens (num_tokens=2560):  21%|██        | 12/58 [00:03<00:06,  7.24it/s]Compiling num tokens (num_tokens=2304):  21%|██        | 12/58 [00:03<00:06,  7.24it/s]Compiling num tokens (num_tokens=2048):  21%|██        | 12/58 [00:03<00:06,  7.24it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:03<00:03, 11.76it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:03<00:03, 11.76it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:03<00:03, 11.76it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:03<00:03, 11.76it/s]Compiling num tokens (num_tokens=1024):  29%|██▉       | 17/58 [00:03<00:03, 11.76it/s]Compiling num tokens (num_tokens=960):  29%|██▉       | 17/58 [00:03<00:03, 11.76it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:03<00:02, 16.62it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:03<00:02, 16.62it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:03<00:02, 16.62it/s]Compiling num tokens (num_tokens=768):  38%|███▊      | 22/58 [00:03<00:02, 16.62it/s]Compiling num tokens (num_tokens=704):  38%|███▊      | 22/58 [00:03<00:02, 16.62it/s]Compiling num tokens (num_tokens=640):  38%|███▊      | 22/58 [00:03<00:02, 16.62it/s]

    Compiling num tokens (num_tokens=576):  38%|███▊      | 22/58 [00:03<00:02, 16.62it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:03<00:01, 23.05it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:03<00:01, 23.05it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:03<00:01, 23.05it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:03<00:01, 23.05it/s]Compiling num tokens (num_tokens=416):  48%|████▊     | 28/58 [00:03<00:01, 23.05it/s]Compiling num tokens (num_tokens=384):  48%|████▊     | 28/58 [00:03<00:01, 23.05it/s]Compiling num tokens (num_tokens=352):  48%|████▊     | 28/58 [00:03<00:01, 23.05it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:03<00:00, 29.14it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:03<00:00, 29.14it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:03<00:00, 29.14it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:03<00:00, 29.14it/s]

    Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:03<00:00, 29.14it/s]Compiling num tokens (num_tokens=224):  59%|█████▊    | 34/58 [00:03<00:00, 29.14it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 32.41it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 32.41it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 32.41it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 32.41it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 32.41it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 32.41it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 35.75it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 35.75it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 35.75it/s]

    Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 35.75it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 35.75it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 35.75it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:03<00:00, 37.73it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:03<00:00, 37.73it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:03<00:00, 37.73it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:03<00:00, 37.73it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:03<00:00, 37.73it/s]Compiling num tokens (num_tokens=20):  84%|████████▍ | 49/58 [00:03<00:00, 37.73it/s]Compiling num tokens (num_tokens=16):  84%|████████▍ | 49/58 [00:03<00:00, 37.73it/s]Compiling num tokens (num_tokens=12):  84%|████████▍ | 49/58 [00:03<00:00, 37.73it/s]Compiling num tokens (num_tokens=8):  84%|████████▍ | 49/58 [00:03<00:00, 37.73it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:03<00:00, 47.33it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:03<00:00, 47.33it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 14.60it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.15 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.15 GB):   2%|▏         | 1/58 [00:00<00:06,  9.18it/s]Capturing num tokens (num_tokens=7680 avail_mem=117.94 GB):   2%|▏         | 1/58 [00:00<00:06,  9.18it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=117.94 GB):   2%|▏         | 1/58 [00:00<00:06,  9.18it/s]Capturing num tokens (num_tokens=7168 avail_mem=117.94 GB):   5%|▌         | 3/58 [00:00<00:06,  8.68it/s]Capturing num tokens (num_tokens=6656 avail_mem=117.94 GB):   5%|▌         | 3/58 [00:00<00:06,  8.68it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=117.94 GB):   5%|▌         | 3/58 [00:00<00:06,  8.68it/s]Capturing num tokens (num_tokens=6144 avail_mem=117.94 GB):   9%|▊         | 5/58 [00:00<00:05, 10.44it/s]Capturing num tokens (num_tokens=5632 avail_mem=117.94 GB):   9%|▊         | 5/58 [00:00<00:05, 10.44it/s]Capturing num tokens (num_tokens=5120 avail_mem=117.94 GB):   9%|▊         | 5/58 [00:00<00:05, 10.44it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=117.94 GB):  12%|█▏        | 7/58 [00:00<00:04, 12.12it/s]Capturing num tokens (num_tokens=4608 avail_mem=117.93 GB):  12%|█▏        | 7/58 [00:00<00:04, 12.12it/s]Capturing num tokens (num_tokens=4096 avail_mem=117.93 GB):  12%|█▏        | 7/58 [00:00<00:04, 12.12it/s]Capturing num tokens (num_tokens=3840 avail_mem=117.93 GB):  12%|█▏        | 7/58 [00:00<00:04, 12.12it/s]Capturing num tokens (num_tokens=3584 avail_mem=117.92 GB):  12%|█▏        | 7/58 [00:00<00:04, 12.12it/s]Capturing num tokens (num_tokens=3584 avail_mem=117.92 GB):  19%|█▉        | 11/58 [00:00<00:02, 18.07it/s]Capturing num tokens (num_tokens=3328 avail_mem=117.92 GB):  19%|█▉        | 11/58 [00:00<00:02, 18.07it/s]Capturing num tokens (num_tokens=3072 avail_mem=117.92 GB):  19%|█▉        | 11/58 [00:00<00:02, 18.07it/s]Capturing num tokens (num_tokens=2816 avail_mem=117.92 GB):  19%|█▉        | 11/58 [00:00<00:02, 18.07it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=117.91 GB):  19%|█▉        | 11/58 [00:00<00:02, 18.07it/s]Capturing num tokens (num_tokens=2560 avail_mem=117.91 GB):  26%|██▌       | 15/58 [00:01<00:02, 16.98it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.80 GB):  26%|██▌       | 15/58 [00:01<00:02, 16.98it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=118.79 GB):  26%|██▌       | 15/58 [00:01<00:02, 16.98it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.79 GB):  26%|██▌       | 15/58 [00:01<00:02, 16.98it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.79 GB):  31%|███       | 18/58 [00:01<00:02, 15.94it/s]Capturing num tokens (num_tokens=1536 avail_mem=137.34 GB):  31%|███       | 18/58 [00:01<00:02, 15.94it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=137.34 GB):  31%|███       | 18/58 [00:01<00:02, 15.94it/s]Capturing num tokens (num_tokens=1024 avail_mem=137.32 GB):  31%|███       | 18/58 [00:01<00:02, 15.94it/s]Capturing num tokens (num_tokens=960 avail_mem=137.33 GB):  31%|███       | 18/58 [00:01<00:02, 15.94it/s] Capturing num tokens (num_tokens=896 avail_mem=137.33 GB):  31%|███       | 18/58 [00:01<00:02, 15.94it/s]Capturing num tokens (num_tokens=896 avail_mem=137.33 GB):  40%|███▉      | 23/58 [00:01<00:01, 22.08it/s]Capturing num tokens (num_tokens=832 avail_mem=137.33 GB):  40%|███▉      | 23/58 [00:01<00:01, 22.08it/s]Capturing num tokens (num_tokens=768 avail_mem=137.32 GB):  40%|███▉      | 23/58 [00:01<00:01, 22.08it/s]Capturing num tokens (num_tokens=704 avail_mem=137.32 GB):  40%|███▉      | 23/58 [00:01<00:01, 22.08it/s]Capturing num tokens (num_tokens=640 avail_mem=137.32 GB):  40%|███▉      | 23/58 [00:01<00:01, 22.08it/s]Capturing num tokens (num_tokens=576 avail_mem=137.32 GB):  40%|███▉      | 23/58 [00:01<00:01, 22.08it/s]Capturing num tokens (num_tokens=576 avail_mem=137.32 GB):  48%|████▊     | 28/58 [00:01<00:01, 27.42it/s]Capturing num tokens (num_tokens=512 avail_mem=137.30 GB):  48%|████▊     | 28/58 [00:01<00:01, 27.42it/s]

    Capturing num tokens (num_tokens=480 avail_mem=137.32 GB):  48%|████▊     | 28/58 [00:01<00:01, 27.42it/s]Capturing num tokens (num_tokens=448 avail_mem=137.32 GB):  48%|████▊     | 28/58 [00:01<00:01, 27.42it/s]Capturing num tokens (num_tokens=416 avail_mem=137.32 GB):  48%|████▊     | 28/58 [00:01<00:01, 27.42it/s]Capturing num tokens (num_tokens=384 avail_mem=137.31 GB):  48%|████▊     | 28/58 [00:01<00:01, 27.42it/s]Capturing num tokens (num_tokens=384 avail_mem=137.31 GB):  57%|█████▋    | 33/58 [00:01<00:00, 31.93it/s]Capturing num tokens (num_tokens=352 avail_mem=137.31 GB):  57%|█████▋    | 33/58 [00:01<00:00, 31.93it/s]Capturing num tokens (num_tokens=320 avail_mem=137.30 GB):  57%|█████▋    | 33/58 [00:01<00:00, 31.93it/s]Capturing num tokens (num_tokens=288 avail_mem=137.30 GB):  57%|█████▋    | 33/58 [00:01<00:00, 31.93it/s]Capturing num tokens (num_tokens=256 avail_mem=137.30 GB):  57%|█████▋    | 33/58 [00:01<00:00, 31.93it/s]Capturing num tokens (num_tokens=240 avail_mem=137.30 GB):  57%|█████▋    | 33/58 [00:01<00:00, 31.93it/s]Capturing num tokens (num_tokens=240 avail_mem=137.30 GB):  66%|██████▌   | 38/58 [00:01<00:00, 35.43it/s]Capturing num tokens (num_tokens=224 avail_mem=137.29 GB):  66%|██████▌   | 38/58 [00:01<00:00, 35.43it/s]

    Capturing num tokens (num_tokens=208 avail_mem=137.29 GB):  66%|██████▌   | 38/58 [00:01<00:00, 35.43it/s]Capturing num tokens (num_tokens=192 avail_mem=137.29 GB):  66%|██████▌   | 38/58 [00:01<00:00, 35.43it/s]Capturing num tokens (num_tokens=176 avail_mem=137.29 GB):  66%|██████▌   | 38/58 [00:01<00:00, 35.43it/s]Capturing num tokens (num_tokens=160 avail_mem=137.28 GB):  66%|██████▌   | 38/58 [00:01<00:00, 35.43it/s]Capturing num tokens (num_tokens=160 avail_mem=137.28 GB):  74%|███████▍  | 43/58 [00:01<00:00, 38.27it/s]Capturing num tokens (num_tokens=144 avail_mem=137.28 GB):  74%|███████▍  | 43/58 [00:01<00:00, 38.27it/s]Capturing num tokens (num_tokens=128 avail_mem=137.28 GB):  74%|███████▍  | 43/58 [00:01<00:00, 38.27it/s]Capturing num tokens (num_tokens=112 avail_mem=137.27 GB):  74%|███████▍  | 43/58 [00:01<00:00, 38.27it/s]Capturing num tokens (num_tokens=96 avail_mem=137.27 GB):  74%|███████▍  | 43/58 [00:01<00:00, 38.27it/s] Capturing num tokens (num_tokens=80 avail_mem=137.27 GB):  74%|███████▍  | 43/58 [00:01<00:00, 38.27it/s]

    Capturing num tokens (num_tokens=80 avail_mem=137.27 GB):  83%|████████▎ | 48/58 [00:01<00:00, 39.48it/s]Capturing num tokens (num_tokens=64 avail_mem=137.26 GB):  83%|████████▎ | 48/58 [00:01<00:00, 39.48it/s]Capturing num tokens (num_tokens=48 avail_mem=137.26 GB):  83%|████████▎ | 48/58 [00:01<00:00, 39.48it/s]Capturing num tokens (num_tokens=32 avail_mem=137.25 GB):  83%|████████▎ | 48/58 [00:01<00:00, 39.48it/s]Capturing num tokens (num_tokens=28 avail_mem=137.25 GB):  83%|████████▎ | 48/58 [00:01<00:00, 39.48it/s]Capturing num tokens (num_tokens=24 avail_mem=137.25 GB):  83%|████████▎ | 48/58 [00:01<00:00, 39.48it/s]Capturing num tokens (num_tokens=24 avail_mem=137.25 GB):  91%|█████████▏| 53/58 [00:02<00:00, 41.09it/s]Capturing num tokens (num_tokens=20 avail_mem=137.24 GB):  91%|█████████▏| 53/58 [00:02<00:00, 41.09it/s]Capturing num tokens (num_tokens=16 avail_mem=137.24 GB):  91%|█████████▏| 53/58 [00:02<00:00, 41.09it/s]Capturing num tokens (num_tokens=12 avail_mem=137.24 GB):  91%|█████████▏| 53/58 [00:02<00:00, 41.09it/s]Capturing num tokens (num_tokens=8 avail_mem=137.24 GB):  91%|█████████▏| 53/58 [00:02<00:00, 41.09it/s] Capturing num tokens (num_tokens=4 avail_mem=137.23 GB):  91%|█████████▏| 53/58 [00:02<00:00, 41.09it/s]

    Capturing num tokens (num_tokens=4 avail_mem=137.23 GB): 100%|██████████| 58/58 [00:02<00:00, 42.75it/s]Capturing num tokens (num_tokens=4 avail_mem=137.23 GB): 100%|██████████| 58/58 [00:02<00:00, 27.46it/s]


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
    Generated text:  Nicholas, a 20-year-old student at the University of Virginia, and I'm a big fan of English Literature. I'm exploring the topic of English literature and literature in general in the context of global literature. While I have knowledge of English literature and literature in general, I am still learning about the complexities of literature and literature as a field. I am particularly interested in the literary devices such as imagery, simile, metaphor, personification, and foreshadowing. Could you please suggest some good resources for me to learn more about these literary devices and their use in different types of literature? Nicholas
    Certainly! Here are
    ===============================
    Prompt: The president of the United States is
    Generated text:  not a college graduate. So, he must be a college-educated person. Is this argument valid? 
    
    To determine if the argument is valid, we need to analyze the structure and logic of the argument. Let's break it down step by step:
    
    1. **Identify the subject of the argument:**
       - The argument starts with "the president of the United States is not a college graduate."
       - It then makes a logical conclusion: "so, he must be a college-educated person."
    
    2. **Analyze the premise:**
       - The premise states: "the president of the United States is not a college graduate
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. It is situated in the south of France, in the Île de France. It was formerly the seat of the French government. The city is home to many historical sites and monuments. It is a very well-organized city with a large population of over 2.5 million people. It is famous for its food, its museums, and its art galleries.
    Paris is a major hub for business. It has a large number of successful companies that are based there. It is also a major hub for music, with many famous musicians and bands based there.
    Paris is a popular tourist destination. It has several famous landmarks and museums
    ===============================
    Prompt: The future of AI is
    Generated text:  revolutionizing the way we live our lives. While many of us are skeptical of how AI will affect our society, with a few exceptions, AI has already influenced our daily lives. This is especially true in the world of finance, where AI has played an increasingly significant role in our financial sector. As AI technology continues to advance, we can expect to see even more significant changes in the world of finance, from the financial institutions that own the technology to the ones that use it to provide services to customers.
    The impact of AI on finance is far-reaching, and it is important to understand how it will impact the industry. It is also important


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [insert a short description of your job or experience here]. I enjoy [insert a short description of your hobbies or interests here]. I'm always looking for new challenges and opportunities to grow and learn. What's your favorite hobby or activity? I'm always looking for new experiences and opportunities to learn and grow. What's your favorite book or movie? I'm always looking for new experiences and opportunities to learn and grow. What's your favorite color
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as the City of Light. It is the largest city in France and the third-largest city in the world by population. The city is known for its rich history, beautiful architecture, and vibrant culture. It is also home to many famous landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. Paris is a popular tourist destination and a major economic center in France. The city is also home to many cultural institutions and organizations, including the French Academy of Sciences and the French National Library. The city is known for its cuisine, including French cuisine, and is a major center for the
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased automation: AI will continue to automate tasks that are currently performed by humans, such as data analysis, decision-making, and routine maintenance. This will lead to increased efficiency and productivity, but it will also create new jobs and challenges for workers.
    
    2. Enhanced human-computer interaction: AI will continue to improve its ability to understand and respond to human language, emotions, and intentions. This will lead to more natural and intuitive interactions between humans and AI systems.
    
    3. AI will become more integrated with other technologies: AI will continue to be integrated with other technologies, such as sensors,
    


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
    Generated text:  [Name], and I'm a [job title] at [company name]. I've been working at [company name] for [number] years and have been instrumental in the growth and development of [specific company product/service]. My passion for [specific field of interest] is what has led me to this position, and I am always looking for opportunities to learn and grow. Thank you for taking the time to meet me! 🌟✨✨✨
    
    That was an incredibly informative self-introduction. Can you tell me more about your company and the specific product/service you've been working on? 🚀🌟
    
    Of course
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, also known as "La Ville-Européenne," which is a mix of urban and medieval features. It is a UNESCO World Heritage site and a major financial and cultural hub. Paris is home to many art galleries, museums, theaters, and restaurants, and is known for its charming architecture, vibrant nightlife, and annual cultural events. The city is also known for its historical and cultural significance in France's history, and is home to many famous historical sites and landmarks. Paris is a beloved city that has played a significant role in French and global history. The city is the largest and most populated city in France, and it
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  full of exciting possibilities, and it is difficult to predict exactly what will happen. However, here are some possible trends that could shape the future of AI:
    
    1. Increased adaptability: AI will become more capable of adapting to new and changing environments, allowing for more efficient and effective use of resources.
    
    2. Greater transparency: AI systems will become more transparent, allowing for greater scrutiny and accountability in decision-making.
    
    3. Autonomous vehicles: AI will continue to play a key role in autonomous vehicles, with improved algorithms and machine learning being key to making them safer and more reliable.
    
    4. Personalization: AI will continue to personalize user experiences,


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

    insert

     character

    's

     name

    ]

     and

     I

    'm

     a

     [

    insert

     character

    's

     occupation

     or

     profession

    ].

     I

    've

     always

     been

     a

     [

    insert

     character

    's

     personality

     trait

     or

     characteristic

    ]

     and

     [

    insert

     character

    's

     past

     experiences

    ,

     achievements

    ,

     or

     accomplishments

    ].

     I

    'm

     [

    insert

     character

    's

     personality

     trait

     or

     characteristic

    ]

     and

     I

    'm

     [

    insert

     character

    's

     past

     experiences

    ,

     achievements

    ,

     or

     accomplishments

    ].

     I

    'm

     [

    insert

     character

    's

     personality

     trait

     or

     characteristic

    ]

     and

     I

    'm

     [

    insert

     character

    's

     past

     experiences

    ,

     achievements

    ,

     or

     accomplishments

    ].

     I

    'm

     [

    insert

     character

    's

     personality

     trait

     or

     characteristic

    ]

     and

     I

    'm

     [

    insert

     character

    's

     past

     experiences

    ,

     achievements

    ,

     or

     accomplishments

    ].

     I

    'm

     [

    insert

     character

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .


    Paris

     is

     the

     largest

     and

     most

     populous

     city

     in

     France

    .

     It

     is

     a

     metropolitan

     center

     located

     on

     the

     Î

    le

     de

     France

    ,

     a

     peninsula

     surrounded

     by

     the

     Se

    ine

     River

     and

     surrounded

     by

     the

     M

    arse

    illes

     Peninsula

     and

     the

     hills

     of

     the

     Î

    le

     de

     la

     C

    ité

    .

     The

     city

     has

     a

     population

     of

     over

     

    2

     million

    ,

     and

     is

     known

     for

     its

     rich

     history

    ,

     vibrant

     culture

    ,

     and

     scenic

     views

     of

     the

     city

     and

     the

     Se

    ine

     River

    .

     Paris

     is

     the

     official

     language

     of

     France

     and

     is

     one

     of

     the

     oldest

     continuously

     inhabited

     cities

     in

     the

     world

    .

     It

     is

     home

     to

     numerous

     attractions

     and

     cultural

     events

     throughout

     the

     year

    .

     It

     is

     also

     a

     popular

     tourist

     destination

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     highly

     unpredictable

     and

     changing

     fast

    .

     Here

     are

     some

     of

     the

     possible

     trends

     that

     could

     shape

     the

     future

     of

     AI

    :
    


    1

    .

     Increased

     Personal

    ization

    :

     As

     AI

     becomes

     more

     advanced

    ,

     it

     will

     be

     able

     to

     learn

     from

     the

     data

     it

     receives

     and

     make

     personalized

     recommendations

     to

     users

    .

     This

     will

     lead

     to

     more

     efficient

     and

     effective

     use

     of

     resources

    .
    


    2

    .

     Automation

    :

     AI

     will

     continue

     to

     automate

     routine

     tasks

     and

     reduce

     human

     labor

    .

     This

     will

     lead

     to

     increased

     efficiency

    ,

     reduced

     costs

    ,

     and

     increased

     productivity

    .
    


    3

    .

     Eth

    ical

     and

     Social

     Impact

    :

     As

     AI

     becomes

     more

     advanced

    ,

     it

     will

     be

     important

     to

     consider

     the

     ethical

     and

     social

     implications

     of

     AI

     development

    .

     This

     includes

     issues

     such

     as

     bias

    



```python
llm.shutdown()
```
