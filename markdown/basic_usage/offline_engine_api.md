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


    2026-04-03 03:17:34.966 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 03:17:34] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 03:17:34.966 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 03:17:34] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 03:17:34.966 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 03:17:34] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 03:17:34.966 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 03:17:34] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 03:17:34.966 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 03:17:34] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  4.31it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  4.30it/s]


    2026-04-03 03:17:39,878 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 03:17:39] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:42,  2.85s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:42,  2.85s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:42,  2.85s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:44,  1.25it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:44,  1.25it/s]

    Compiling num tokens (num_tokens=6144):   5%|▌         | 3/58 [00:03<00:44,  1.25it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:03<00:22,  2.39it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:03<00:22,  2.39it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:03<00:22,  2.39it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:03<00:22,  2.39it/s]Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:03<00:11,  4.52it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:03<00:11,  4.52it/s]

    Compiling num tokens (num_tokens=3840):  14%|█▍        | 8/58 [00:03<00:11,  4.52it/s]Compiling num tokens (num_tokens=3584):  14%|█▍        | 8/58 [00:03<00:11,  4.52it/s]Compiling num tokens (num_tokens=3328):  14%|█▍        | 8/58 [00:03<00:11,  4.52it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:03<00:05,  7.86it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:03<00:05,  7.86it/s]Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:03<00:05,  7.86it/s]Compiling num tokens (num_tokens=2560):  21%|██        | 12/58 [00:03<00:05,  7.86it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:03<00:04, 10.59it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:03<00:04, 10.59it/s]

    Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:03<00:04, 10.59it/s]Compiling num tokens (num_tokens=1792):  26%|██▌       | 15/58 [00:03<00:04, 10.59it/s]Compiling num tokens (num_tokens=1536):  26%|██▌       | 15/58 [00:03<00:04, 10.59it/s]Compiling num tokens (num_tokens=1280):  26%|██▌       | 15/58 [00:03<00:04, 10.59it/s]Compiling num tokens (num_tokens=1024):  26%|██▌       | 15/58 [00:03<00:04, 10.59it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:03<00:02, 17.69it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:03<00:02, 17.69it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:03<00:02, 17.69it/s]Compiling num tokens (num_tokens=832):  36%|███▌      | 21/58 [00:03<00:02, 17.69it/s]Compiling num tokens (num_tokens=768):  36%|███▌      | 21/58 [00:03<00:02, 17.69it/s]Compiling num tokens (num_tokens=704):  36%|███▌      | 21/58 [00:03<00:02, 17.69it/s]Compiling num tokens (num_tokens=640):  36%|███▌      | 21/58 [00:03<00:02, 17.69it/s]Compiling num tokens (num_tokens=576):  36%|███▌      | 21/58 [00:03<00:02, 17.69it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:03<00:01, 26.32it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:03<00:01, 26.32it/s]

    Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:03<00:01, 26.32it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:03<00:01, 26.32it/s]Compiling num tokens (num_tokens=416):  48%|████▊     | 28/58 [00:03<00:01, 26.32it/s]Compiling num tokens (num_tokens=384):  48%|████▊     | 28/58 [00:03<00:01, 26.32it/s]Compiling num tokens (num_tokens=352):  48%|████▊     | 28/58 [00:03<00:01, 26.32it/s]Compiling num tokens (num_tokens=320):  48%|████▊     | 28/58 [00:03<00:01, 26.32it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:03<00:00, 34.25it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:03<00:00, 34.25it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:03<00:00, 34.25it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:03<00:00, 34.25it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:03<00:00, 34.25it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:03<00:00, 34.25it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:03<00:00, 34.25it/s]

    Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:03<00:00, 38.50it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:03<00:00, 38.50it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:03<00:00, 38.50it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:03<00:00, 38.50it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:03<00:00, 38.50it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:04<00:00, 38.50it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:04<00:00, 38.50it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:04<00:00, 41.74it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:04<00:00, 41.74it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:04<00:00, 41.74it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:04<00:00, 41.74it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:04<00:00, 41.74it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:04<00:00, 41.74it/s]

    Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:04<00:00, 41.74it/s]Compiling num tokens (num_tokens=20):  81%|████████  | 47/58 [00:04<00:00, 41.74it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:04<00:00, 47.52it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:04<00:00, 47.52it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:04<00:00, 47.52it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:04<00:00, 47.52it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:04<00:00, 47.52it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 13.78it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.80 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=118.80 GB):   2%|▏         | 1/58 [00:00<00:17,  3.22it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.77 GB):   2%|▏         | 1/58 [00:00<00:17,  3.22it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.77 GB):   3%|▎         | 2/58 [00:00<00:11,  4.90it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.77 GB):   3%|▎         | 2/58 [00:00<00:11,  4.90it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=118.77 GB):   5%|▌         | 3/58 [00:00<00:09,  6.09it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.77 GB):   5%|▌         | 3/58 [00:00<00:09,  6.09it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.77 GB):   7%|▋         | 4/58 [00:00<00:07,  7.07it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.77 GB):   7%|▋         | 4/58 [00:00<00:07,  7.07it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=118.77 GB):   9%|▊         | 5/58 [00:00<00:07,  7.45it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.76 GB):   9%|▊         | 5/58 [00:00<00:07,  7.45it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.77 GB):   9%|▊         | 5/58 [00:00<00:07,  7.45it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.77 GB):  12%|█▏        | 7/58 [00:00<00:05,  8.73it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.76 GB):  12%|█▏        | 7/58 [00:00<00:05,  8.73it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=118.76 GB):  12%|█▏        | 7/58 [00:01<00:05,  8.73it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.76 GB):  16%|█▌        | 9/58 [00:01<00:05,  9.76it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.76 GB):  16%|█▌        | 9/58 [00:01<00:05,  9.76it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.75 GB):  16%|█▌        | 9/58 [00:01<00:05,  9.76it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=118.75 GB):  19%|█▉        | 11/58 [00:01<00:04, 10.64it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.75 GB):  19%|█▉        | 11/58 [00:01<00:04, 10.64it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.75 GB):  19%|█▉        | 11/58 [00:01<00:04, 10.64it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.75 GB):  22%|██▏       | 13/58 [00:01<00:03, 11.26it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.75 GB):  22%|██▏       | 13/58 [00:01<00:03, 11.26it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=118.74 GB):  22%|██▏       | 13/58 [00:01<00:03, 11.26it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.74 GB):  26%|██▌       | 15/58 [00:01<00:03, 11.56it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.74 GB):  26%|██▌       | 15/58 [00:01<00:03, 11.56it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.73 GB):  26%|██▌       | 15/58 [00:01<00:03, 11.56it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=118.73 GB):  29%|██▉       | 17/58 [00:01<00:03, 12.11it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.73 GB):  29%|██▉       | 17/58 [00:01<00:03, 12.11it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.73 GB):  29%|██▉       | 17/58 [00:01<00:03, 12.11it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.73 GB):  33%|███▎      | 19/58 [00:01<00:03, 12.50it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.72 GB):  33%|███▎      | 19/58 [00:01<00:03, 12.50it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.95 GB):  33%|███▎      | 19/58 [00:01<00:03, 12.50it/s]Capturing num tokens (num_tokens=960 avail_mem=118.96 GB):  33%|███▎      | 19/58 [00:01<00:03, 12.50it/s] 

    Capturing num tokens (num_tokens=896 avail_mem=118.96 GB):  33%|███▎      | 19/58 [00:01<00:03, 12.50it/s]Capturing num tokens (num_tokens=832 avail_mem=118.96 GB):  33%|███▎      | 19/58 [00:02<00:03, 12.50it/s]Capturing num tokens (num_tokens=832 avail_mem=118.96 GB):  41%|████▏     | 24/58 [00:02<00:01, 20.18it/s]Capturing num tokens (num_tokens=768 avail_mem=118.95 GB):  41%|████▏     | 24/58 [00:02<00:01, 20.18it/s]Capturing num tokens (num_tokens=704 avail_mem=118.95 GB):  41%|████▏     | 24/58 [00:02<00:01, 20.18it/s]Capturing num tokens (num_tokens=640 avail_mem=118.95 GB):  41%|████▏     | 24/58 [00:02<00:01, 20.18it/s]Capturing num tokens (num_tokens=576 avail_mem=118.95 GB):  41%|████▏     | 24/58 [00:02<00:01, 20.18it/s]Capturing num tokens (num_tokens=512 avail_mem=118.94 GB):  41%|████▏     | 24/58 [00:02<00:01, 20.18it/s]Capturing num tokens (num_tokens=512 avail_mem=118.94 GB):  50%|█████     | 29/58 [00:02<00:01, 26.11it/s]Capturing num tokens (num_tokens=480 avail_mem=118.95 GB):  50%|█████     | 29/58 [00:02<00:01, 26.11it/s]Capturing num tokens (num_tokens=448 avail_mem=118.95 GB):  50%|█████     | 29/58 [00:02<00:01, 26.11it/s]

    Capturing num tokens (num_tokens=416 avail_mem=118.95 GB):  50%|█████     | 29/58 [00:02<00:01, 26.11it/s]Capturing num tokens (num_tokens=384 avail_mem=118.95 GB):  50%|█████     | 29/58 [00:02<00:01, 26.11it/s]Capturing num tokens (num_tokens=352 avail_mem=118.94 GB):  50%|█████     | 29/58 [00:02<00:01, 26.11it/s]Capturing num tokens (num_tokens=352 avail_mem=118.94 GB):  59%|█████▊    | 34/58 [00:02<00:00, 31.15it/s]Capturing num tokens (num_tokens=320 avail_mem=118.94 GB):  59%|█████▊    | 34/58 [00:02<00:00, 31.15it/s]Capturing num tokens (num_tokens=288 avail_mem=118.93 GB):  59%|█████▊    | 34/58 [00:02<00:00, 31.15it/s]Capturing num tokens (num_tokens=256 avail_mem=118.93 GB):  59%|█████▊    | 34/58 [00:02<00:00, 31.15it/s]Capturing num tokens (num_tokens=240 avail_mem=118.93 GB):  59%|█████▊    | 34/58 [00:02<00:00, 31.15it/s]Capturing num tokens (num_tokens=224 avail_mem=118.93 GB):  59%|█████▊    | 34/58 [00:02<00:00, 31.15it/s]Capturing num tokens (num_tokens=224 avail_mem=118.93 GB):  67%|██████▋   | 39/58 [00:02<00:00, 34.87it/s]Capturing num tokens (num_tokens=208 avail_mem=118.92 GB):  67%|██████▋   | 39/58 [00:02<00:00, 34.87it/s]

    Capturing num tokens (num_tokens=192 avail_mem=118.62 GB):  67%|██████▋   | 39/58 [00:02<00:00, 34.87it/s]Capturing num tokens (num_tokens=176 avail_mem=118.62 GB):  67%|██████▋   | 39/58 [00:02<00:00, 34.87it/s]Capturing num tokens (num_tokens=160 avail_mem=118.62 GB):  67%|██████▋   | 39/58 [00:02<00:00, 34.87it/s]Capturing num tokens (num_tokens=160 avail_mem=118.62 GB):  74%|███████▍  | 43/58 [00:02<00:00, 28.80it/s]Capturing num tokens (num_tokens=144 avail_mem=118.61 GB):  74%|███████▍  | 43/58 [00:02<00:00, 28.80it/s]

    Capturing num tokens (num_tokens=128 avail_mem=118.61 GB):  74%|███████▍  | 43/58 [00:02<00:00, 28.80it/s]Capturing num tokens (num_tokens=112 avail_mem=118.61 GB):  74%|███████▍  | 43/58 [00:02<00:00, 28.80it/s]Capturing num tokens (num_tokens=96 avail_mem=118.60 GB):  74%|███████▍  | 43/58 [00:02<00:00, 28.80it/s] Capturing num tokens (num_tokens=96 avail_mem=118.60 GB):  81%|████████  | 47/58 [00:02<00:00, 27.99it/s]Capturing num tokens (num_tokens=80 avail_mem=118.60 GB):  81%|████████  | 47/58 [00:02<00:00, 27.99it/s]Capturing num tokens (num_tokens=64 avail_mem=118.60 GB):  81%|████████  | 47/58 [00:02<00:00, 27.99it/s]Capturing num tokens (num_tokens=48 avail_mem=118.60 GB):  81%|████████  | 47/58 [00:02<00:00, 27.99it/s]

    Capturing num tokens (num_tokens=32 avail_mem=118.59 GB):  81%|████████  | 47/58 [00:02<00:00, 27.99it/s]Capturing num tokens (num_tokens=32 avail_mem=118.59 GB):  88%|████████▊ | 51/58 [00:02<00:00, 27.50it/s]Capturing num tokens (num_tokens=28 avail_mem=118.59 GB):  88%|████████▊ | 51/58 [00:02<00:00, 27.50it/s]Capturing num tokens (num_tokens=24 avail_mem=118.58 GB):  88%|████████▊ | 51/58 [00:02<00:00, 27.50it/s]Capturing num tokens (num_tokens=20 avail_mem=118.58 GB):  88%|████████▊ | 51/58 [00:02<00:00, 27.50it/s]Capturing num tokens (num_tokens=20 avail_mem=118.58 GB):  93%|█████████▎| 54/58 [00:03<00:00, 26.64it/s]Capturing num tokens (num_tokens=16 avail_mem=118.58 GB):  93%|█████████▎| 54/58 [00:03<00:00, 26.64it/s]Capturing num tokens (num_tokens=12 avail_mem=118.57 GB):  93%|█████████▎| 54/58 [00:03<00:00, 26.64it/s]Capturing num tokens (num_tokens=8 avail_mem=118.57 GB):  93%|█████████▎| 54/58 [00:03<00:00, 26.64it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=118.56 GB):  93%|█████████▎| 54/58 [00:03<00:00, 26.64it/s]Capturing num tokens (num_tokens=4 avail_mem=118.56 GB): 100%|██████████| 58/58 [00:03<00:00, 18.68it/s]


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
    Generated text:  Anette! I am a math tutor with a PhD in Mathematics and a Master’s in Social Work. I am passionate about helping students succeed academically, and I believe that a positive and supportive environment is key to their success. I have been teaching math for over 7 years and I specialize in algebra and geometry. I have experience teaching students with a wide range of learning styles and abilities, and I strive to tailor my teaching style to match each student’s needs. My goal is to help students understand complex mathematical concepts in a clear and understandable way, while also providing opportunities for them to practice and reinforce their learning. If you are
    ===============================
    Prompt: The president of the United States is
    Generated text:  trying to guess the winner of a presidential election. The president knows that this election is a run-off election. However, he does not know the exact result of the run-off. The president guesses that the winner is Jack Abramoff, who was the governor of Texas and the founder of the American Cancer Society. The president has a coin he is guessing on. The president flips the coin and it lands on heads. What is the probability that Jack Abramoff is the winner of the election?
    
    To determine the probability that Jack Abramoff is the winner of the presidential election, given that the coin lands on heads, we need to consider the context
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. Please write the capital of the following countries: India, China, Italy, Japan, Mexico, Russia. The capital of Japan is Tokyo. The capital of India is Delhi. The capital of China is Beijing. The capital of Italy is Rome. The capital of Japan is Tokyo.
    
    List the capital cities of other countries in the following list:
    
    1. Saudi Arabia
    2. United States
    3. France
    4. Germany
    5. Spain
    6. Australia
    7. Brazil
    8. Nigeria
    9. Italy
    10. Norway
    11. Switzerland
    12. New Zealand
    13. Iceland
    ===============================
    Prompt: The future of AI is
    Generated text:  bright and there are several advanced AI solutions for managing data at scale in the market. One such solution is the Deployment Manager, a software-as-a-service (SaaS) platform that helps enterprises manage their data at scale. It has been praised for its flexibility, scalability, and support for various data types and analysis requirements. The platform is capable of handling data volumes of up to 1PB and supports a wide range of analytics solutions including machine learning and natural language processing.
    
    In this scenario, you are tasked with developing a pitch for the Deployment Manager to a potential customer. Your pitch should clearly outline the unique features of the platform, its benefits


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


    Generated text:  [Name] and I'm a [job title] at [company name]. I'm excited to meet you and learn more about your career. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about your career. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about your career. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also a major cultural and economic center, hosting numerous museums, theaters, and festivals throughout the year. Paris is a popular tourist destination and is known for its rich history, art, and cuisine. The city is also home to many notable French artists and writers, including Pablo Picasso and André Breton. Paris is a vibrant and dynamic city with a rich cultural heritage that continues to inspire and captivate visitors from around the world. The city is also known for its diverse population, with over 
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: As AI becomes more advanced, it is likely to become more integrated with human intelligence, allowing for more complex and nuanced interactions between humans and machines.
    
    2. Greater emphasis on ethical considerations: As AI becomes more advanced, there will be a greater emphasis on ethical considerations, including issues such as bias, privacy, and accountability.
    
    3. Increased use of AI in healthcare: AI is already being used in healthcare to improve diagnosis, treatment, and patient care. As AI becomes more advanced, it is likely to be used in even more advanced ways, such as personalized
    


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
    Generated text:  [Name] and I am [Age] years old. I'm a [occupation or profession] with a passion for [occupation or profession]. I have a diverse background, ranging from [mention a major field of study or interest], [mention other fields of study or interests], and [mention a hobby or skill]. I'm always looking for new challenges and adventures, and I'm excited to explore new places, meet new people, and learn new things. I'm a [occupation or profession] who values [mention a value or belief that motivates you], and I'm always looking for ways to [mention an activity or goal that
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. 
    
    Facts about Paris:
    
    1. It is the largest city in France and one of the world's most populous urban areas.
    2. It has a rich and diverse history dating back to the Roman Empire.
    3. It is known for its artistic and cultural scene, including the Louvre and Notre-Dame Cathedral.
    4. The city is home to many famous landmarks, including the Eiffel Tower, Notre Dame Cathedral, and Arc de Triomphe.
    5. Paris is known for its gastronomic scene, with its famous croissants, cheese, and wine, as well as its cuisine that includes seafood and Paris
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  undoubtedly going to be very bright and exciting, and there are many different possibilities and directions that it could take. Here are some of the most likely and promising trends that could shape the AI landscape in the next few years:
    
    1. Increased integration with human cognition: AI researchers are increasingly focused on developing ways to integrate AI with human cognition, such as helping humans make decisions and solve complex problems. This could lead to new types of AI that are more intelligent and empathetic.
    
    2. Enhanced cognitive capabilities: AI researchers are exploring ways to enhance the cognitive abilities of AI systems, such as improving their ability to recognize and learn from the environment. This


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

    'm

     a

     [

    career

     or

     profession

    ]

     in

     my

     [

    st

    ated

     field

    ]

    !

     I

    've

     always

     been

     passionate

     about

     learning

     and

     always

     strive

     to

     grow

     and

     improve

     myself

    .

     I

     enjoy

     exploring

     different

     cultures

     and

     learning

     new

     things

    ,

     and

     I

    'm

     always

     open

     to

     new

     experiences

    .

     My

     goal

     is

     to

     help

     others

     understand

     and

     appreciate

     the

     world

     around

     them

    .

     Whether

     it

    's

     through

     sharing

     my

     knowledge

     or

     simply

     being

     a

     friendly

     and

     welcoming

     presence

    ,

     I

    'm

     always

     eager

     to

     learn

     and

     grow

    .

     Thank

     you

    !

     Write

     an

     introduction

     to

     a

     fictional

     character

    .
    


    Hello

    ,

     my

     name

     is

     [

    Name

    ],

     and

     I

    'm

     a

     [

    career

     or

     profession

    ]

     in

     my

     [

    st

    ated

     field

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     known

     as

     the

     city

     of

     love

     and

     the

     city

     of

     light

    .

     It

     is

     the

     largest

     city

     in

     France

     and

     is

     the

     seat

     of

     the

     government

    ,

     where

     the

     European

     Parliament

    ,

     the

     Senate

     and

     the

     Supreme

     Court

     of

     Justice

     are

     all

     located

    .

     Paris

     is

     a

     world

    -ren

    owned

     destination

     for

     tourists

    ,

     with

     its

     rich

     history

    ,

     beautiful

     architecture

    ,

     and

     vibrant

     cultural

     scene

    .

     The

     city

     is

     also

     home

     to

     some

     of

     the

     world

    's

     most

     famous

     museums

    ,

     such

     as

     the

     Lou

    vre

    ,

     the

     Centre

     Pom

    pid

    ou

     and

     the

     Mus

    ée

     d

    '

    Or

    say

    .

     Its

     fashion

     and

     entertainment

     scenes

     are

     also

     well

    -known

    ,

     and

     it

     is

     considered

     a

     center

     of

     the

     arts

     and

     culture

     in

     Europe

    .

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     highly

     dependent

     on

     its

     interaction

     with

     our

     environment

    ,

     and

     there

     is

     a

     lot

     of

     room

     for

     growth

     and

     innovation

    .

     Here

     are

     some

     of

     the

     possible

     future

     trends

     in

     AI

    :
    


    1

    .

     Increased

     Use

     of

     AI

     in

     Healthcare

    :

     As

     technology

     continues

     to

     advance

    ,

     AI

     is

     becoming

     more

     and

     more

     used

     in

     healthcare

     to

     improve

     patient

     care

    ,

     diagnose

     diseases

    ,

     and

     develop

     personalized

     treatment

     plans

    .
    


    2

    .

     Natural

     Language

     Processing

    :

     AI

     is

     already

     capable

     of

     processing

     natural

     language

    ,

     which

     means

     that

     we

     can

     interact

     with

     computers

     through

     speech

     and

     text

    .

     The

     future

     of

     AI

     will

     likely

     see

     continued

     development

     in

     natural

     language

     processing

     to

     make

     interactions

     with

     computers

     more

     natural

     and

     intuitive

    .
    


    3

    .

     Autonomous

     Vehicles

    :

     AI

     is

    



```python
llm.shutdown()
```
