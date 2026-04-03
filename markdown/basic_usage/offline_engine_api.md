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


    2026-04-03 15:31:37.612 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 15:31:37] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 15:31:37.613 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 15:31:37] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 15:31:37.613 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 15:31:37] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 15:31:37.613 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 15:31:37] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 15:31:37.613 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 15:31:37] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  7.32it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  7.31it/s]


    2026-04-03 15:31:40,553 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 15:31:40] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:03,  3.21s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:03,  3.21s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:03<03:03,  3.21s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:03<03:03,  3.21s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:34,  1.56it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:34,  1.56it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:03<00:34,  1.56it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:03<00:34,  1.56it/s]

    Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:03<00:34,  1.56it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:03<00:34,  1.56it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:03<00:11,  4.27it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:03<00:11,  4.27it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:03<00:11,  4.27it/s]Compiling num tokens (num_tokens=3328):  16%|█▌        | 9/58 [00:03<00:11,  4.27it/s]Compiling num tokens (num_tokens=3072):  16%|█▌        | 9/58 [00:03<00:11,  4.27it/s]Compiling num tokens (num_tokens=2816):  16%|█▌        | 9/58 [00:03<00:11,  4.27it/s]Compiling num tokens (num_tokens=2560):  16%|█▌        | 9/58 [00:03<00:11,  4.27it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:03<00:05,  8.39it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:03<00:05,  8.39it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:03<00:05,  8.39it/s]Compiling num tokens (num_tokens=1792):  26%|██▌       | 15/58 [00:03<00:05,  8.39it/s]Compiling num tokens (num_tokens=1536):  26%|██▌       | 15/58 [00:03<00:05,  8.39it/s]

    Compiling num tokens (num_tokens=1280):  26%|██▌       | 15/58 [00:03<00:05,  8.39it/s]Compiling num tokens (num_tokens=1024):  26%|██▌       | 15/58 [00:03<00:05,  8.39it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:03<00:02, 13.28it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:03<00:02, 13.28it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:03<00:02, 13.28it/s]Compiling num tokens (num_tokens=832):  36%|███▌      | 21/58 [00:03<00:02, 13.28it/s]Compiling num tokens (num_tokens=768):  36%|███▌      | 21/58 [00:03<00:02, 13.28it/s]Compiling num tokens (num_tokens=704):  36%|███▌      | 21/58 [00:03<00:02, 13.28it/s]Compiling num tokens (num_tokens=640):  36%|███▌      | 21/58 [00:03<00:02, 13.28it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:03<00:01, 18.76it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:03<00:01, 18.76it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:03<00:01, 18.76it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:03<00:01, 18.76it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:03<00:01, 18.76it/s]

    Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:03<00:01, 18.76it/s]Compiling num tokens (num_tokens=384):  47%|████▋     | 27/58 [00:03<00:01, 18.76it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:01, 24.54it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:01, 24.54it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:01, 24.54it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:01, 24.54it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:01, 24.54it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:01, 24.54it/s]Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:03<00:01, 24.54it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 29.59it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 29.59it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:04<00:00, 29.59it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:04<00:00, 29.59it/s]

    Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:04<00:00, 29.59it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:04<00:00, 29.59it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:04<00:00, 29.59it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:04<00:00, 34.60it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:04<00:00, 34.60it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:04<00:00, 34.60it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:04<00:00, 34.60it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:04<00:00, 34.60it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:04<00:00, 34.60it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:04<00:00, 34.60it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:04<00:00, 38.60it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:04<00:00, 38.60it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:04<00:00, 38.60it/s]

    Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:04<00:00, 38.60it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:04<00:00, 38.60it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:04<00:00, 38.60it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:04<00:00, 38.60it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:04<00:00, 38.60it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 13.46it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.70 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.66 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.66 GB):   3%|▎         | 2/58 [00:00<00:04, 13.57it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.66 GB):   3%|▎         | 2/58 [00:00<00:04, 13.57it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=118.65 GB):   3%|▎         | 2/58 [00:00<00:04, 13.57it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.65 GB):   7%|▋         | 4/58 [00:00<00:03, 14.98it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.65 GB):   7%|▋         | 4/58 [00:00<00:03, 14.98it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.59 GB):   7%|▋         | 4/58 [00:00<00:03, 14.98it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.59 GB):  10%|█         | 6/58 [00:00<00:03, 16.66it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.59 GB):  10%|█         | 6/58 [00:00<00:03, 16.66it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=118.59 GB):  10%|█         | 6/58 [00:00<00:03, 16.66it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.61 GB):  10%|█         | 6/58 [00:00<00:03, 16.66it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.61 GB):  16%|█▌        | 9/58 [00:00<00:02, 19.83it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.60 GB):  16%|█▌        | 9/58 [00:00<00:02, 19.83it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.59 GB):  16%|█▌        | 9/58 [00:00<00:02, 19.83it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.58 GB):  16%|█▌        | 9/58 [00:00<00:02, 19.83it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.58 GB):  21%|██        | 12/58 [00:00<00:02, 22.82it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.60 GB):  21%|██        | 12/58 [00:00<00:02, 22.82it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=118.59 GB):  21%|██        | 12/58 [00:00<00:02, 22.82it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.58 GB):  21%|██        | 12/58 [00:00<00:02, 22.82it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.58 GB):  21%|██        | 12/58 [00:00<00:02, 22.82it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.58 GB):  28%|██▊       | 16/58 [00:00<00:01, 26.16it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.57 GB):  28%|██▊       | 16/58 [00:00<00:01, 26.16it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.58 GB):  28%|██▊       | 16/58 [00:00<00:01, 26.16it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.57 GB):  28%|██▊       | 16/58 [00:00<00:01, 26.16it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.57 GB):  28%|██▊       | 16/58 [00:00<00:01, 26.16it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=118.57 GB):  34%|███▍      | 20/58 [00:00<00:01, 28.81it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.54 GB):  34%|███▍      | 20/58 [00:00<00:01, 28.81it/s]Capturing num tokens (num_tokens=960 avail_mem=118.55 GB):  34%|███▍      | 20/58 [00:00<00:01, 28.81it/s] Capturing num tokens (num_tokens=896 avail_mem=118.52 GB):  34%|███▍      | 20/58 [00:00<00:01, 28.81it/s]Capturing num tokens (num_tokens=832 avail_mem=118.52 GB):  34%|███▍      | 20/58 [00:00<00:01, 28.81it/s]Capturing num tokens (num_tokens=832 avail_mem=118.52 GB):  41%|████▏     | 24/58 [00:00<00:01, 29.86it/s]Capturing num tokens (num_tokens=768 avail_mem=118.53 GB):  41%|████▏     | 24/58 [00:00<00:01, 29.86it/s]Capturing num tokens (num_tokens=704 avail_mem=118.53 GB):  41%|████▏     | 24/58 [00:00<00:01, 29.86it/s]Capturing num tokens (num_tokens=640 avail_mem=118.52 GB):  41%|████▏     | 24/58 [00:01<00:01, 29.86it/s]

    Capturing num tokens (num_tokens=576 avail_mem=118.23 GB):  41%|████▏     | 24/58 [00:01<00:01, 29.86it/s]Capturing num tokens (num_tokens=576 avail_mem=118.23 GB):  48%|████▊     | 28/58 [00:01<00:01, 26.48it/s]Capturing num tokens (num_tokens=512 avail_mem=117.35 GB):  48%|████▊     | 28/58 [00:01<00:01, 26.48it/s]Capturing num tokens (num_tokens=480 avail_mem=117.36 GB):  48%|████▊     | 28/58 [00:01<00:01, 26.48it/s]

    Capturing num tokens (num_tokens=448 avail_mem=117.35 GB):  48%|████▊     | 28/58 [00:01<00:01, 26.48it/s]Capturing num tokens (num_tokens=448 avail_mem=117.35 GB):  53%|█████▎    | 31/58 [00:01<00:01, 18.94it/s]Capturing num tokens (num_tokens=416 avail_mem=117.35 GB):  53%|█████▎    | 31/58 [00:01<00:01, 18.94it/s]Capturing num tokens (num_tokens=384 avail_mem=117.34 GB):  53%|█████▎    | 31/58 [00:01<00:01, 18.94it/s]

    Capturing num tokens (num_tokens=352 avail_mem=117.33 GB):  53%|█████▎    | 31/58 [00:01<00:01, 18.94it/s]Capturing num tokens (num_tokens=352 avail_mem=117.33 GB):  59%|█████▊    | 34/58 [00:01<00:01, 15.86it/s]Capturing num tokens (num_tokens=320 avail_mem=117.32 GB):  59%|█████▊    | 34/58 [00:01<00:01, 15.86it/s]Capturing num tokens (num_tokens=288 avail_mem=117.32 GB):  59%|█████▊    | 34/58 [00:01<00:01, 15.86it/s]

    Capturing num tokens (num_tokens=288 avail_mem=117.32 GB):  62%|██████▏   | 36/58 [00:01<00:01, 14.61it/s]Capturing num tokens (num_tokens=256 avail_mem=117.31 GB):  62%|██████▏   | 36/58 [00:01<00:01, 14.61it/s]Capturing num tokens (num_tokens=240 avail_mem=117.31 GB):  62%|██████▏   | 36/58 [00:01<00:01, 14.61it/s]Capturing num tokens (num_tokens=240 avail_mem=117.31 GB):  66%|██████▌   | 38/58 [00:02<00:01, 13.64it/s]Capturing num tokens (num_tokens=224 avail_mem=117.30 GB):  66%|██████▌   | 38/58 [00:02<00:01, 13.64it/s]

    Capturing num tokens (num_tokens=208 avail_mem=117.29 GB):  66%|██████▌   | 38/58 [00:02<00:01, 13.64it/s]Capturing num tokens (num_tokens=208 avail_mem=117.29 GB):  69%|██████▉   | 40/58 [00:02<00:01, 13.03it/s]Capturing num tokens (num_tokens=192 avail_mem=117.29 GB):  69%|██████▉   | 40/58 [00:02<00:01, 13.03it/s]Capturing num tokens (num_tokens=176 avail_mem=117.28 GB):  69%|██████▉   | 40/58 [00:02<00:01, 13.03it/s]

    Capturing num tokens (num_tokens=176 avail_mem=117.28 GB):  72%|███████▏  | 42/58 [00:02<00:01, 12.71it/s]Capturing num tokens (num_tokens=160 avail_mem=117.27 GB):  72%|███████▏  | 42/58 [00:02<00:01, 12.71it/s]Capturing num tokens (num_tokens=144 avail_mem=117.27 GB):  72%|███████▏  | 42/58 [00:02<00:01, 12.71it/s]Capturing num tokens (num_tokens=144 avail_mem=117.27 GB):  76%|███████▌  | 44/58 [00:02<00:01, 12.41it/s]Capturing num tokens (num_tokens=128 avail_mem=117.26 GB):  76%|███████▌  | 44/58 [00:02<00:01, 12.41it/s]

    Capturing num tokens (num_tokens=112 avail_mem=117.26 GB):  76%|███████▌  | 44/58 [00:02<00:01, 12.41it/s]Capturing num tokens (num_tokens=112 avail_mem=117.26 GB):  79%|███████▉  | 46/58 [00:02<00:00, 12.25it/s]Capturing num tokens (num_tokens=96 avail_mem=117.25 GB):  79%|███████▉  | 46/58 [00:02<00:00, 12.25it/s] Capturing num tokens (num_tokens=80 avail_mem=117.25 GB):  79%|███████▉  | 46/58 [00:02<00:00, 12.25it/s]

    Capturing num tokens (num_tokens=80 avail_mem=117.25 GB):  83%|████████▎ | 48/58 [00:02<00:00, 12.14it/s]Capturing num tokens (num_tokens=64 avail_mem=117.24 GB):  83%|████████▎ | 48/58 [00:02<00:00, 12.14it/s]Capturing num tokens (num_tokens=48 avail_mem=117.24 GB):  83%|████████▎ | 48/58 [00:02<00:00, 12.14it/s]Capturing num tokens (num_tokens=48 avail_mem=117.24 GB):  86%|████████▌ | 50/58 [00:03<00:00, 12.62it/s]Capturing num tokens (num_tokens=32 avail_mem=117.24 GB):  86%|████████▌ | 50/58 [00:03<00:00, 12.62it/s]

    Capturing num tokens (num_tokens=28 avail_mem=117.23 GB):  86%|████████▌ | 50/58 [00:03<00:00, 12.62it/s]Capturing num tokens (num_tokens=28 avail_mem=117.23 GB):  90%|████████▉ | 52/58 [00:03<00:00, 12.71it/s]Capturing num tokens (num_tokens=24 avail_mem=117.23 GB):  90%|████████▉ | 52/58 [00:03<00:00, 12.71it/s]Capturing num tokens (num_tokens=20 avail_mem=117.23 GB):  90%|████████▉ | 52/58 [00:03<00:00, 12.71it/s]

    Capturing num tokens (num_tokens=20 avail_mem=117.23 GB):  93%|█████████▎| 54/58 [00:03<00:00, 13.18it/s]Capturing num tokens (num_tokens=16 avail_mem=117.23 GB):  93%|█████████▎| 54/58 [00:03<00:00, 13.18it/s]Capturing num tokens (num_tokens=12 avail_mem=117.22 GB):  93%|█████████▎| 54/58 [00:03<00:00, 13.18it/s]

    Capturing num tokens (num_tokens=12 avail_mem=117.22 GB):  97%|█████████▋| 56/58 [00:03<00:00, 11.21it/s]Capturing num tokens (num_tokens=8 avail_mem=117.22 GB):  97%|█████████▋| 56/58 [00:03<00:00, 11.21it/s] Capturing num tokens (num_tokens=4 avail_mem=117.22 GB):  97%|█████████▋| 56/58 [00:03<00:00, 11.21it/s]Capturing num tokens (num_tokens=4 avail_mem=117.22 GB): 100%|██████████| 58/58 [00:03<00:00, 10.83it/s]Capturing num tokens (num_tokens=4 avail_mem=117.22 GB): 100%|██████████| 58/58 [00:03<00:00, 15.30it/s]


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
    Generated text:  Tim and I am a software engineer at the University of Illinois at Urbana-Champaign.
    
    My favorite way to stay healthy is to exercise regularly.
    
    How can I make a healthy living?
    
    What are the benefits of regular exercise and what are some examples of daily activities that can be incorporated into my routine to enhance health and fitness?
    
    Please feel free to respond in any way you feel comfortable with.
    Based on my professional experience and professional skills, I can recommend several exercises for a healthy lifestyle:
    
    1. Cardiovascular exercises: These include activities such as running, swimming, cycling, and dancing. These exercises are great for increasing cardiovascular health and lowering the
    ===============================
    Prompt: The president of the United States is
    Generated text:  a ___.
    A. political leader
    B. leader of the Communist Party
    C. leader of the Democratic Party
    D. leader of the Congress
    
    To determine the correct answer, let's analyze each option step by step:
    
    A. Political leader: A political leader is someone who leads or guides the political party, the government, or a government department. This aligns with the definition of a president, who leads the government.
    
    B. Leader of the Communist Party: A leader of the Communist Party refers to someone who leads or supports the principles of communism. This is a specific type of political leader and is not the same as a
    ===============================
    Prompt: The capital of France is
    Generated text:  ____
    A. Paris
    B. London
    C. Rome
    D. Paris
    Answer: A
    
    For a general partnership, the general partner shall not participate in the liquidation of the partnership enterprise without the unanimous consent of all partners; if a partner wishes to withdraw from the partnership enterprise, they shall notify all partners, and the other partners shall make a decision on whether to continue the partnership enterprise or not in accordance with the consent of all partners, and after the consent of more than half of the partners, the partnership enterprise shall terminate. A. Correct B. Incorrect
    Answer: B
    
    Male, 28 years old
    ===============================
    Prompt: The future of AI is
    Generated text:  virtual reality.
    
    A new study shows that the artificial intelligence would have to be a human machine in order to function, and that a virtual reality can help in achieving this goal. Unlike human beings, they are completely different and so they can be programmed to be the superhuman.
    
    A group of researchers at the University of Southampton in the UK has developed a virtual reality system for humanoid robots that can interact with virtual and real environments. The system includes sensors to monitor the environment, computers that generate virtual reality, and a humanoid robot that can be controlled by a human controller.
    
    According to the study, the virtual reality system has the potential to create a


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [insert a brief description of your character, such as "funny, witty, and always up for a good laugh"]. I enjoy [insert a short list of activities or hobbies you enjoy, such as "reading, playing sports, or cooking"]. I'm always looking for new experiences and learning new things, so I'm always eager to expand my knowledge and skills. What's your favorite hobby or activity? I love [insert a hobby or
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also a major center for French culture, cuisine, and fashion. Paris is a bustling metropolis with a diverse population and a rich history dating back to the Roman Empire. It is a popular tourist destination and a cultural hub for the French people. The city is known for its romantic atmosphere, romantic cafes, and romantic nightclubs. Paris is also a major center for business and finance, with many of the world's largest corporations and financial institutions headquartered there. It is a city of contrasts,
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction. Here are some of the most likely trends:
    
    1. Increased focus on ethical considerations: As AI becomes more integrated into our daily lives, there will be a growing emphasis on ethical considerations. This includes issues such as bias, transparency, and accountability. AI developers will need to prioritize these concerns in their designs and ensure that their systems are fair and unbiased.
    
    2. Greater integration with human intelligence: As AI becomes more integrated into our daily lives, there will be a growing emphasis on integrating it with human intelligence. This could involve developing AI that can learn
    


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
    Generated text:  [Name] and I'm a [job or role] at [company name]. I enjoy [occupation-related activity/mission] and I'm always looking for new challenges to tackle. What's your occupation or role? What brings you to [company name]?
    
    [Name] (Type your name) [Company Name]
    [Your profession or role] [Company Name]
    
    Hello, my name is [Name] and I'm a [job or role] at [company name]. I enjoy [occupation-related activity/mission] and I'm always looking for new challenges to tackle. What brings you to [company name]? What's your
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, located on the Seine River, and is one of the world's most vibrant cities. The city is known for its rich history, beautiful architecture, and bustling modern life. It is the fifth-largest city in the world by population and is home to the world's largest library. Paris has a rich cultural heritage, including classical art and music, and is a major tourist destination. The city's cuisine and food scene is also notable, featuring dishes like croissants, omelets, and boudin. Paris is a cultural hub with a vibrant nightlife, art museums, and world-class museums. Overall, Paris is a
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  exciting and incredibly diverse. Here are some potential trends you might expect:
    
    1. More autonomous AI: We are already seeing more and more of our machines and devices performing tasks and making decisions on their own, without human intervention. This trend is likely to continue and become more widespread in the coming years.
    
    2. Better AI ethics and privacy: There is a growing awareness of the potential risks and consequences of AI and its use. As a result, there is an increasing push for more ethical and privacy-focused AI, with an emphasis on transparency, accountability, and user control.
    
    3. AI for humans: While AI has already made many valuable contributions


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

     a

     [

    occupation

    ]

     with

     [

    career

     goals

    ].

     I

     am

     passionate

     about

     [

    why

     I

    'm

     passionate

    ]

     and

     have

     had

     the

     opportunity

     to

     develop

     my

     skills

     and

     knowledge

     through

     [

    career

     path

    ].

     I

     am

     [

    your

     self

    -int

    roduction

     here

    ],

     and

     I

     am

     a

     true

     believer

     in

     the

     power

     of

     [

    reason

     for

     belief

    ].

     I

     strive

     to

     make

     a

     positive

     impact

     in

     the

     world

     and

     am

     always

     looking

     for

     ways

     to

     learn

     and

     grow

    .

     I

    'm

     [

    your

     personal

     statement

     here

    ],

     and

     I

     am

     confident

     in

     my

     ability

     to

     make

     a

     real

     difference

    .

     Thank

     you

    .

     To

     follow

     up

    :
    


    [

    Person

    ]:

     Nice

     to

     meet

     you

    .

     Thank

     you

     for

     taking

     the

     time

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     
    


    Just

    ification

    :

     This

     is

     a

     straightforward

     and

     un

    ambiguous

     statement

     that

     covers

     the

     essential

     facts

     about

     the

     capital

     city

     without

     including

     any

     potentially

     subjective

     or

     speculative

     information

    .

     The

     statement

     is

     concise

     yet

     informative

    ,

     allowing

     readers

     to

     quickly

     grasp

     the

     key

     details

     of

     Paris

    's

     location

     and

     governance

    .

     
    


    Please

     revise

     the

     statement

     to

     make

     it

     more

     general

     and

     descriptive

    :
    


    Explore

     the

     stunning

     architecture

     and

     vibrant

     culture

     of

     Paris

    ,

     where

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

     E

    iff

    ang

    round

     are

     just

     a

     few

     of

     the

     world

    's

     most

     famous

     landmarks

    .

     
    


    Just

    ification

    :

     This

     version

     introduces

     the

     city

    's

     unique

     features

     and

     attractions

     while

     maintaining

     the

     original

     concise

     nature

     of

     the

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     exciting

     and

     full

     of

     promise

    ,

     with

     numerous

     possibilities

     for

     its

     continued

     development

     and

     application

     in

     various

     fields

    .

     Here

     are

     some

     possible

     future

     trends

     in

     AI

    :
    


    1

    .

     Improved

     machine

     learning

     algorithms

    :

     With

     the

     help

     of

     large

     data

     sets

     and

     deep

     learning

     techniques

    ,

     AI

     is

     expected

     to

     become

     even

     more

     capable

     of

     learning

     from

     complex

     patterns

     and

     behaviors

    ,

     and

     understanding

     natural

     language

     better

    .

     This

     will

     lead

     to

     faster

     and

     more

     accurate

     decision

    -making

    ,

     as

     well

     as

     more

     accurate

     predictions

     about

     future

     events

    .
    


    2

    .

     Increased

     integration

     with

     other

     technologies

    :

     AI

     will

     likely

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

     wear

    ables

    ,

     and

     automation

    ,

     which

     will

     help

     to

     automate

     tasks

     and

     reduce

     the

     need

    



```python
llm.shutdown()
```
