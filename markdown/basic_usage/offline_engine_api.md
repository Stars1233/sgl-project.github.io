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


    2026-04-02 06:48:59.128 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:48:59] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 06:48:59.128 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:48:59] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 06:48:59.128 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:48:59] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 06:48:59.128 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:48:59] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 06:48:59.128 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:48:59] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.64it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.63it/s]


    2026-04-02 06:49:04,036 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 06:49:04] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:33,  2.70s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:33,  2.70s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<01:05,  1.17s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<01:05,  1.17s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<01:05,  1.17s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:25,  2.09it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:25,  2.09it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:25,  2.09it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:03<00:25,  2.09it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:03<00:11,  4.35it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:03<00:11,  4.35it/s]Compiling num tokens (num_tokens=4096):  12%|█▏        | 7/58 [00:03<00:11,  4.35it/s]

    Compiling num tokens (num_tokens=3840):  12%|█▏        | 7/58 [00:03<00:11,  4.35it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:03<00:06,  6.90it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:03<00:06,  6.90it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:03<00:06,  6.90it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:03<00:06,  6.90it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:03<00:06,  6.90it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:03<00:04, 10.90it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:03<00:04, 10.90it/s]Compiling num tokens (num_tokens=2304):  24%|██▍       | 14/58 [00:03<00:04, 10.90it/s]

    Compiling num tokens (num_tokens=2048):  24%|██▍       | 14/58 [00:03<00:04, 10.90it/s]Compiling num tokens (num_tokens=1792):  24%|██▍       | 14/58 [00:03<00:04, 10.90it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:02, 15.15it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:02, 15.15it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:02, 15.15it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:02, 15.15it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:03<00:02, 17.66it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:03<00:02, 17.66it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:03<00:02, 17.66it/s]

    Compiling num tokens (num_tokens=832):  36%|███▌      | 21/58 [00:03<00:02, 17.66it/s]Compiling num tokens (num_tokens=768):  36%|███▌      | 21/58 [00:03<00:02, 17.66it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 21.58it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 21.58it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 21.58it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 21.58it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 21.58it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:03<00:01, 24.78it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:03<00:01, 24.78it/s]

    Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:03<00:01, 24.78it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:03<00:01, 24.78it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:03<00:01, 24.78it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:01, 24.74it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:01, 24.74it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:01, 24.74it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:01, 24.74it/s]

    Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:04<00:00, 24.70it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:04<00:00, 24.70it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:04<00:00, 24.70it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:04<00:00, 24.70it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:04<00:00, 23.72it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:04<00:00, 23.72it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:04<00:00, 23.72it/s]

    Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:04<00:00, 23.72it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:04<00:00, 25.01it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:04<00:00, 25.01it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:04<00:00, 25.01it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:04<00:00, 25.01it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:04<00:00, 23.88it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:04<00:00, 23.88it/s]

    Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:04<00:00, 23.88it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:04<00:00, 23.88it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:04<00:00, 24.09it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:04<00:00, 24.09it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:04<00:00, 24.09it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:04<00:00, 24.09it/s]Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:04<00:00, 24.09it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:04<00:00, 26.50it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:04<00:00, 26.50it/s]

    Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:04<00:00, 26.50it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:04<00:00, 26.50it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:04<00:00, 26.50it/s]Compiling num tokens (num_tokens=8):  90%|████████▉ | 52/58 [00:04<00:00, 26.50it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:04<00:00, 30.80it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:04<00:00, 30.80it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 12.08it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.80 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.80 GB):   2%|▏         | 1/58 [00:00<00:07,  7.49it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.77 GB):   2%|▏         | 1/58 [00:00<00:07,  7.49it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=118.77 GB):   3%|▎         | 2/58 [00:00<00:07,  7.98it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.77 GB):   3%|▎         | 2/58 [00:00<00:07,  7.98it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.77 GB):   5%|▌         | 3/58 [00:00<00:06,  7.97it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.77 GB):   5%|▌         | 3/58 [00:00<00:06,  7.97it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=118.77 GB):   7%|▋         | 4/58 [00:00<00:06,  8.47it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.77 GB):   7%|▋         | 4/58 [00:00<00:06,  8.47it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.76 GB):   7%|▋         | 4/58 [00:00<00:06,  8.47it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=118.76 GB):  10%|█         | 6/58 [00:00<00:05,  8.93it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.76 GB):  10%|█         | 6/58 [00:00<00:05,  8.93it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.76 GB):  10%|█         | 6/58 [00:00<00:05,  8.93it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.76 GB):  14%|█▍        | 8/58 [00:00<00:05,  9.39it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.76 GB):  14%|█▍        | 8/58 [00:00<00:05,  9.39it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=119.01 GB):  14%|█▍        | 8/58 [00:00<00:05,  9.39it/s]Capturing num tokens (num_tokens=3584 avail_mem=119.00 GB):  14%|█▍        | 8/58 [00:00<00:05,  9.39it/s]Capturing num tokens (num_tokens=3328 avail_mem=119.00 GB):  14%|█▍        | 8/58 [00:00<00:05,  9.39it/s]Capturing num tokens (num_tokens=3328 avail_mem=119.00 GB):  21%|██        | 12/58 [00:01<00:02, 15.85it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.99 GB):  21%|██        | 12/58 [00:01<00:02, 15.85it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.99 GB):  21%|██        | 12/58 [00:01<00:02, 15.85it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.99 GB):  21%|██        | 12/58 [00:01<00:02, 15.85it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.99 GB):  21%|██        | 12/58 [00:01<00:02, 15.85it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.98 GB):  21%|██        | 12/58 [00:01<00:02, 15.85it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.98 GB):  29%|██▉       | 17/58 [00:01<00:01, 23.25it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.98 GB):  29%|██▉       | 17/58 [00:01<00:01, 23.25it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=118.98 GB):  29%|██▉       | 17/58 [00:01<00:01, 23.25it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.97 GB):  29%|██▉       | 17/58 [00:01<00:01, 23.25it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.95 GB):  29%|██▉       | 17/58 [00:01<00:01, 23.25it/s]Capturing num tokens (num_tokens=960 avail_mem=118.96 GB):  29%|██▉       | 17/58 [00:01<00:01, 23.25it/s] Capturing num tokens (num_tokens=960 avail_mem=118.96 GB):  38%|███▊      | 22/58 [00:01<00:01, 29.05it/s]Capturing num tokens (num_tokens=896 avail_mem=118.96 GB):  38%|███▊      | 22/58 [00:01<00:01, 29.05it/s]Capturing num tokens (num_tokens=832 avail_mem=118.96 GB):  38%|███▊      | 22/58 [00:01<00:01, 29.05it/s]Capturing num tokens (num_tokens=768 avail_mem=118.95 GB):  38%|███▊      | 22/58 [00:01<00:01, 29.05it/s]Capturing num tokens (num_tokens=704 avail_mem=118.95 GB):  38%|███▊      | 22/58 [00:01<00:01, 29.05it/s]Capturing num tokens (num_tokens=640 avail_mem=118.95 GB):  38%|███▊      | 22/58 [00:01<00:01, 29.05it/s]Capturing num tokens (num_tokens=640 avail_mem=118.95 GB):  47%|████▋     | 27/58 [00:01<00:00, 33.60it/s]Capturing num tokens (num_tokens=576 avail_mem=118.95 GB):  47%|████▋     | 27/58 [00:01<00:00, 33.60it/s]

    Capturing num tokens (num_tokens=512 avail_mem=118.94 GB):  47%|████▋     | 27/58 [00:01<00:00, 33.60it/s]Capturing num tokens (num_tokens=480 avail_mem=118.95 GB):  47%|████▋     | 27/58 [00:01<00:00, 33.60it/s]Capturing num tokens (num_tokens=448 avail_mem=118.95 GB):  47%|████▋     | 27/58 [00:01<00:00, 33.60it/s]Capturing num tokens (num_tokens=416 avail_mem=118.95 GB):  47%|████▋     | 27/58 [00:01<00:00, 33.60it/s]Capturing num tokens (num_tokens=416 avail_mem=118.95 GB):  55%|█████▌    | 32/58 [00:01<00:00, 36.87it/s]Capturing num tokens (num_tokens=384 avail_mem=118.94 GB):  55%|█████▌    | 32/58 [00:01<00:00, 36.87it/s]Capturing num tokens (num_tokens=352 avail_mem=118.94 GB):  55%|█████▌    | 32/58 [00:01<00:00, 36.87it/s]Capturing num tokens (num_tokens=320 avail_mem=118.94 GB):  55%|█████▌    | 32/58 [00:01<00:00, 36.87it/s]Capturing num tokens (num_tokens=288 avail_mem=118.93 GB):  55%|█████▌    | 32/58 [00:01<00:00, 36.87it/s]Capturing num tokens (num_tokens=256 avail_mem=118.93 GB):  55%|█████▌    | 32/58 [00:01<00:00, 36.87it/s]Capturing num tokens (num_tokens=256 avail_mem=118.93 GB):  64%|██████▍   | 37/58 [00:01<00:00, 39.22it/s]Capturing num tokens (num_tokens=240 avail_mem=118.93 GB):  64%|██████▍   | 37/58 [00:01<00:00, 39.22it/s]

    Capturing num tokens (num_tokens=224 avail_mem=118.92 GB):  64%|██████▍   | 37/58 [00:01<00:00, 39.22it/s]Capturing num tokens (num_tokens=208 avail_mem=118.92 GB):  64%|██████▍   | 37/58 [00:01<00:00, 39.22it/s]Capturing num tokens (num_tokens=192 avail_mem=118.92 GB):  64%|██████▍   | 37/58 [00:01<00:00, 39.22it/s]Capturing num tokens (num_tokens=176 avail_mem=118.92 GB):  64%|██████▍   | 37/58 [00:01<00:00, 39.22it/s]Capturing num tokens (num_tokens=176 avail_mem=118.92 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.14it/s]Capturing num tokens (num_tokens=160 avail_mem=118.91 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.14it/s]Capturing num tokens (num_tokens=144 avail_mem=118.91 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.14it/s]Capturing num tokens (num_tokens=128 avail_mem=118.91 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.14it/s]Capturing num tokens (num_tokens=112 avail_mem=118.90 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.14it/s]Capturing num tokens (num_tokens=96 avail_mem=118.90 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.14it/s] 

    Capturing num tokens (num_tokens=96 avail_mem=118.90 GB):  81%|████████  | 47/58 [00:01<00:00, 41.66it/s]Capturing num tokens (num_tokens=80 avail_mem=118.90 GB):  81%|████████  | 47/58 [00:01<00:00, 41.66it/s]Capturing num tokens (num_tokens=64 avail_mem=118.89 GB):  81%|████████  | 47/58 [00:01<00:00, 41.66it/s]Capturing num tokens (num_tokens=48 avail_mem=118.89 GB):  81%|████████  | 47/58 [00:01<00:00, 41.66it/s]Capturing num tokens (num_tokens=32 avail_mem=118.88 GB):  81%|████████  | 47/58 [00:01<00:00, 41.66it/s]Capturing num tokens (num_tokens=28 avail_mem=118.88 GB):  81%|████████  | 47/58 [00:01<00:00, 41.66it/s]Capturing num tokens (num_tokens=28 avail_mem=118.88 GB):  90%|████████▉ | 52/58 [00:01<00:00, 39.66it/s]Capturing num tokens (num_tokens=24 avail_mem=118.88 GB):  90%|████████▉ | 52/58 [00:01<00:00, 39.66it/s]Capturing num tokens (num_tokens=20 avail_mem=118.87 GB):  90%|████████▉ | 52/58 [00:01<00:00, 39.66it/s]Capturing num tokens (num_tokens=16 avail_mem=118.87 GB):  90%|████████▉ | 52/58 [00:01<00:00, 39.66it/s]

    Capturing num tokens (num_tokens=12 avail_mem=118.87 GB):  90%|████████▉ | 52/58 [00:01<00:00, 39.66it/s]Capturing num tokens (num_tokens=8 avail_mem=118.87 GB):  90%|████████▉ | 52/58 [00:02<00:00, 39.66it/s] Capturing num tokens (num_tokens=8 avail_mem=118.87 GB):  98%|█████████▊| 57/58 [00:02<00:00, 41.40it/s]Capturing num tokens (num_tokens=4 avail_mem=118.86 GB):  98%|█████████▊| 57/58 [00:02<00:00, 41.40it/s]Capturing num tokens (num_tokens=4 avail_mem=118.86 GB): 100%|██████████| 58/58 [00:02<00:00, 28.15it/s]


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
    Generated text:  Xiaofang. I am a young student who is really curious and enjoys learning. My primary goal is to improve my mathematical skills and explore various topics in my studies. I hope to become a well-rounded individual with a strong foundation in mathematics. 
    
    Could you please provide me with some tips on how to improve my math skills? Additionally, could you suggest some specific online resources that I can use to learn more about math topics? Lastly, could you recommend some specific online courses that I can take to enhance my mathematical knowledge? Please provide the links or descriptions of these courses. 
    
    I am looking forward to learning more about math and appreciate your
    ===============================
    Prompt: The president of the United States is
    Generated text:  a political office. It was first held in 1789 by George Washington. Who was the first president of the United States?
    
    George Washington was the first president of the United States. He was born on June 26, 1732, in Danbury, Connecticut. Washington served as the first president from 1789 until his death on September 19, 1797. His presidency was marked by a significant change in governance, as he replaced the previous president, Thomas Jefferson, who had resigned due to his belief in the authority of the federal government over the states. His administration
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. A certain event lasts for 10 days. The event is scheduled in 14 different days. In how many days will the event end? To determine the end date of the event, we need to consider the total duration of the event and the number of days available for the event to run.
    
    1. Identify the total duration of the event: The event lasts for 10 days.
    2. Identify the number of days available for the event to run: There are 14 different days in total.
    3. Calculate the end date by adding the total duration of the event to the number of days available for the
    ===============================
    Prompt: The future of AI is
    Generated text:  uncertain, but there’s no denying that it is here. The AI that was introduced in the early years of the 20th century was still in its early stages. It was a complex and multifaceted system, one that was not designed by experts, but it was designed for human beings. It was and still is an AI that has no emotion, creativity, and the ability to think independently.
    The evolution of AI is showing no sign of slowing down. As it continues to develop, it will become more and more complex. It will become more and more sophisticated. It will become more and more efficient. It will be able


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


    Generated text:  [Name] and I'm a [Age] year old [Occupation]. I'm a [Skill] who has been [Number of Years] years in the industry. I'm passionate about [What I Love to Do], and I'm always looking for ways to [What I Want to Improve]. I'm a [What I Do Best] person who is always [What I Do Best]. I'm [What I Do Best] and I'm [What I Do Best]. I'm a [What I Do Best] person who is always [What I Do Best]. I'm [What I Do Best] and I'm [
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as the City of Light. It is a historic city with a rich history and a vibrant culture, known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and Louvre Museum. Paris is also known for its fashion industry, art scene, and its role as a cultural hub for Europe. The city is home to many world-renowned museums, including the Louvre, the Musée d'Orsay, and the Musée Rodin. Paris is also a major transportation hub, with its famous Seine River and its numerous bridges and tunnels. The city is known for its cuisine
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the way we live, work, and interact with technology. Here are some of the most likely trends that could be expected in the future of AI:
    
    1. Increased automation and robotics: As AI technology continues to advance, we are likely to see an increase in automation and robotics in various industries. This could lead to the creation of more efficient and cost-effective solutions, but it could also lead to job displacement for some workers.
    
    2. Enhanced privacy and security: As AI becomes more integrated into our daily lives, we are likely to see increased emphasis on privacy and security.
    


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
    Generated text:  _____. I am a/an _____. I have been working in the ____ field for ____ years. I am currently ____ years old. I am a/an ____ person, and I enjoy ____ and ____.
    
    I am confident in my ability to contribute to the success of this project, and I am looking forward to helping you achieve your goals. In my spare time, I enjoy ____ and ____.
    
    Thank you for your time and consideration. I look forward to working with you. 
    
    Best regards,
    [Your Name] 
    
    Note: Replace the placeholders with your own names or other relevant details. Also, you can use emojis, symbols, or
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    
    Paris is the capital city of France. It is known for its rich history, artistic heritage, and vibrant culture, and is a major tourist destination. The city is home to iconic landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. Paris also has a large international population and is a hub of finance, media, and fashion. Its population is estimated to be around 2.1 million people. Paris is considered one of the most exciting and innovative cities in the world. It is also known for its beautiful gardens, including the Park Guimet, and its iconic towers, including the E
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  full of exciting possibilities, and there are many potential areas where technology is heading. Here are some of the most likely future trends in AI:
    
    1. Autonomous vehicles: As autonomous vehicles become more common, AI will play an increasingly important role in transportation. They will be able to communicate with cars and other vehicles, detect obstacles, and make decisions on the road.
    
    2. Smart homes: AI will become more integrated into our homes, making them more efficient and convenient. They will be able to understand and adapt to our needs and preferences, and will help us save energy and reduce waste.
    
    3. Healthcare: AI will be used in healthcare to


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

    Your

     Name

    ],

     and

     I

    'm

     a

     creative

    ,

     independent

     artist

     with

     a

     passion

     for

     making

     beautiful

    ,

     original

     art

     that

     speaks

     to

     the

     human

     experience

    .

     I

    'm

     a

     visual

     artist

     who

     creates

     intricate

     coll

    ages

    ,

     paintings

    ,

     and

     sculptures

     that

     explore

     themes

     of

     identity

    ,

     memory

    ,

     and

     the

     subconscious

    .

     I

     believe

     that

     every

     piece

     I

     create

     is

     a

     work

     of

     art

    ,

     and

     that

     the

     more

     I

     challenge

     myself

     creatively

    ,

     the

     more

     I

     grow

     as

     an

     artist

    .

     I

    'm

     excited

     to

     collaborate

     with

     anyone

     who

     shares

     my

     passion

     for

     art

    .

     Come

     see

     my

     work

     in

     person

     at

     [

    Venue

     Name

    ],

     and

     let

    's

     create

     something

     special

     together

    .

     [

    Your

     Name

    ]

     (

    insert

     name

    )


    You

     are

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .
    


    Paris

     is

     the

     capital

     city

     of

     France

    ,

     the

     country

    's

     largest

     and

     most

     populous

     city

    .

     The

     city

     is

     known

     for

     its

     rich

     history

    ,

     beautiful

     architecture

    ,

     and

     vibrant

     culture

    .

     It

     is

     located

     on

     the

     North

     Bank

     of

     the

     Se

    ine

     river

     and

     is

     home

     to

     the

     E

    iff

    el

     Tower

    ,

     the

     Lou

    vre

     Museum

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

     a

     popular

     tourist

     destination

     and

     a

     major

     business

     and

     economic

     center

     in

     France

    .

     It

     has

     a

     complex

     administrative

     structure

    ,

     with

     the

     government

     being

     headquartered

     in

     the

     Lou

    vre

     and

     the

     French

     National

     Assembly

     located

     in

     the

     Palace

     of

     Vers

    ailles

    .

     
    


    Paris

     has

     a

     diverse

     population

     and

     culture

    ,

     including

     those

     of

     various

     national

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     involve

     a

     range

     of

     technological

    ,

     social

    ,

     and

     policy

     developments

     that

     will

     shape

     its

     direction

     and

     impact

    .

     Some

     possible

     trends

     include

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

     advanced

    ,

     there

     will

     be

     a

     greater

     emphasis

     on

     ethical

     considerations

    ,

     such

     as

     ensuring

     that

     AI

     systems

     are

     fair

    ,

     transparent

    ,

     and

     unbiased

    .

     This

     could

     lead

     to

     a

     shift

     towards

     more

     ethical

     AI

     that

     is

     designed

     to

     maximize

     the

     benefits

     for

     humans

     while

     minimizing

     harm

    .
    


    2

    .

     Integration

     with

     human

     decision

    -making

    :

     AI

     is

     increasingly

     being

     integrated

     into

     human

     decision

    -making

     processes

    ,

     allowing

     for

     more

     complex

     and

     nuanced

     decision

    -making

    .

     This

     could

     lead

     to

     a

     greater

     reliance

     on

     AI

     for

     making

     important

     decisions

    



```python
llm.shutdown()
```
