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


    2026-04-02 17:19:00.770 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 17:19:00] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 17:19:00.770 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 17:19:00] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 17:19:00.770 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 17:19:00] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 17:19:00.770 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 17:19:00] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 17:19:00.770 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 17:19:00] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  7.36it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  7.35it/s]


    2026-04-02 17:19:05,472 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 17:19:05] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:40,  2.82s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:40,  2.82s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<01:08,  1.23s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<01:08,  1.23s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<01:08,  1.23s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:26,  2.01it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:26,  2.01it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:03<00:26,  2.01it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:03<00:26,  2.01it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:03<00:12,  4.21it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:03<00:12,  4.21it/s]Compiling num tokens (num_tokens=4096):  12%|█▏        | 7/58 [00:03<00:12,  4.21it/s]

    Compiling num tokens (num_tokens=3840):  12%|█▏        | 7/58 [00:03<00:12,  4.21it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:03<00:07,  6.69it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:03<00:07,  6.69it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:03<00:07,  6.69it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:03<00:07,  6.69it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:03<00:07,  6.69it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:03<00:04, 10.58it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:03<00:04, 10.58it/s]Compiling num tokens (num_tokens=2304):  24%|██▍       | 14/58 [00:03<00:04, 10.58it/s]

    Compiling num tokens (num_tokens=2048):  24%|██▍       | 14/58 [00:03<00:04, 10.58it/s]Compiling num tokens (num_tokens=1792):  24%|██▍       | 14/58 [00:03<00:04, 10.58it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:02, 14.72it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:02, 14.72it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:02, 14.72it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:02, 14.72it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:03<00:02, 17.26it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:03<00:02, 17.26it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:03<00:02, 17.26it/s]

    Compiling num tokens (num_tokens=832):  36%|███▌      | 21/58 [00:03<00:02, 17.26it/s]Compiling num tokens (num_tokens=768):  36%|███▌      | 21/58 [00:03<00:02, 17.26it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 20.10it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 20.10it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 20.10it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 20.10it/s]

    Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:03<00:01, 20.86it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:03<00:01, 20.86it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:03<00:01, 20.86it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:03<00:01, 20.86it/s]Compiling num tokens (num_tokens=416):  48%|████▊     | 28/58 [00:04<00:01, 20.86it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:04<00:01, 23.96it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:04<00:01, 23.96it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:04<00:01, 23.96it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:04<00:01, 23.96it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:04<00:01, 23.96it/s]

    Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:04<00:00, 26.81it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:04<00:00, 26.81it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:04<00:00, 26.81it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:04<00:00, 26.81it/s]Compiling num tokens (num_tokens=208):  62%|██████▏   | 36/58 [00:04<00:00, 26.81it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:04<00:00, 27.90it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:04<00:00, 27.90it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:04<00:00, 27.90it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:04<00:00, 27.90it/s]

    Compiling num tokens (num_tokens=144):  69%|██████▉   | 40/58 [00:04<00:00, 27.90it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:04<00:00, 30.04it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:04<00:00, 30.04it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:04<00:00, 30.04it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:04<00:00, 30.04it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:04<00:00, 30.04it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:04<00:00, 28.82it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:04<00:00, 28.82it/s]

    Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:04<00:00, 28.82it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:04<00:00, 28.82it/s]Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:04<00:00, 28.82it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:04<00:00, 30.08it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:04<00:00, 30.08it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:04<00:00, 30.08it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:04<00:00, 30.08it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:04<00:00, 30.08it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:04<00:00, 32.06it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:04<00:00, 32.06it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:04<00:00, 32.06it/s]

    Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 12.08it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.80 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.80 GB):   2%|▏         | 1/58 [00:00<00:07,  7.87it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.77 GB):   2%|▏         | 1/58 [00:00<00:07,  7.87it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=118.77 GB):   3%|▎         | 2/58 [00:00<00:07,  7.46it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.77 GB):   3%|▎         | 2/58 [00:00<00:07,  7.46it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.77 GB):   5%|▌         | 3/58 [00:00<00:06,  7.94it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.77 GB):   5%|▌         | 3/58 [00:00<00:06,  7.94it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=119.02 GB):   5%|▌         | 3/58 [00:00<00:06,  7.94it/s]Capturing num tokens (num_tokens=6144 avail_mem=119.02 GB):   9%|▊         | 5/58 [00:00<00:05, 10.58it/s]Capturing num tokens (num_tokens=5632 avail_mem=119.01 GB):   9%|▊         | 5/58 [00:00<00:05, 10.58it/s]Capturing num tokens (num_tokens=5120 avail_mem=119.01 GB):   9%|▊         | 5/58 [00:00<00:05, 10.58it/s]Capturing num tokens (num_tokens=4608 avail_mem=119.01 GB):   9%|▊         | 5/58 [00:00<00:05, 10.58it/s]Capturing num tokens (num_tokens=4096 avail_mem=119.01 GB):   9%|▊         | 5/58 [00:00<00:05, 10.58it/s]Capturing num tokens (num_tokens=4096 avail_mem=119.01 GB):  16%|█▌        | 9/58 [00:00<00:02, 17.94it/s]Capturing num tokens (num_tokens=3840 avail_mem=119.01 GB):  16%|█▌        | 9/58 [00:00<00:02, 17.94it/s]Capturing num tokens (num_tokens=3584 avail_mem=119.00 GB):  16%|█▌        | 9/58 [00:00<00:02, 17.94it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=119.00 GB):  16%|█▌        | 9/58 [00:00<00:02, 17.94it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.99 GB):  16%|█▌        | 9/58 [00:00<00:02, 17.94it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.99 GB):  16%|█▌        | 9/58 [00:00<00:02, 17.94it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.99 GB):  24%|██▍       | 14/58 [00:00<00:01, 25.42it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.99 GB):  24%|██▍       | 14/58 [00:00<00:01, 25.42it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.99 GB):  24%|██▍       | 14/58 [00:00<00:01, 25.42it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.98 GB):  24%|██▍       | 14/58 [00:00<00:01, 25.42it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.98 GB):  24%|██▍       | 14/58 [00:00<00:01, 25.42it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.98 GB):  24%|██▍       | 14/58 [00:00<00:01, 25.42it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.98 GB):  33%|███▎      | 19/58 [00:00<00:01, 31.03it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.97 GB):  33%|███▎      | 19/58 [00:00<00:01, 31.03it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=118.95 GB):  33%|███▎      | 19/58 [00:00<00:01, 31.03it/s]Capturing num tokens (num_tokens=960 avail_mem=118.96 GB):  33%|███▎      | 19/58 [00:00<00:01, 31.03it/s] Capturing num tokens (num_tokens=896 avail_mem=118.96 GB):  33%|███▎      | 19/58 [00:00<00:01, 31.03it/s]Capturing num tokens (num_tokens=832 avail_mem=118.96 GB):  33%|███▎      | 19/58 [00:00<00:01, 31.03it/s]Capturing num tokens (num_tokens=832 avail_mem=118.96 GB):  41%|████▏     | 24/58 [00:00<00:00, 35.03it/s]Capturing num tokens (num_tokens=768 avail_mem=118.95 GB):  41%|████▏     | 24/58 [00:00<00:00, 35.03it/s]Capturing num tokens (num_tokens=704 avail_mem=118.95 GB):  41%|████▏     | 24/58 [00:01<00:00, 35.03it/s]Capturing num tokens (num_tokens=640 avail_mem=118.95 GB):  41%|████▏     | 24/58 [00:01<00:00, 35.03it/s]Capturing num tokens (num_tokens=576 avail_mem=118.95 GB):  41%|████▏     | 24/58 [00:01<00:00, 35.03it/s]Capturing num tokens (num_tokens=512 avail_mem=118.94 GB):  41%|████▏     | 24/58 [00:01<00:00, 35.03it/s]

    Capturing num tokens (num_tokens=512 avail_mem=118.94 GB):  50%|█████     | 29/58 [00:01<00:00, 34.21it/s]Capturing num tokens (num_tokens=480 avail_mem=118.95 GB):  50%|█████     | 29/58 [00:01<00:00, 34.21it/s]Capturing num tokens (num_tokens=448 avail_mem=118.95 GB):  50%|█████     | 29/58 [00:01<00:00, 34.21it/s]Capturing num tokens (num_tokens=416 avail_mem=118.95 GB):  50%|█████     | 29/58 [00:01<00:00, 34.21it/s]Capturing num tokens (num_tokens=384 avail_mem=118.94 GB):  50%|█████     | 29/58 [00:01<00:00, 34.21it/s]Capturing num tokens (num_tokens=384 avail_mem=118.94 GB):  57%|█████▋    | 33/58 [00:01<00:00, 33.30it/s]Capturing num tokens (num_tokens=352 avail_mem=118.94 GB):  57%|█████▋    | 33/58 [00:01<00:00, 33.30it/s]Capturing num tokens (num_tokens=320 avail_mem=118.94 GB):  57%|█████▋    | 33/58 [00:01<00:00, 33.30it/s]Capturing num tokens (num_tokens=288 avail_mem=118.93 GB):  57%|█████▋    | 33/58 [00:01<00:00, 33.30it/s]

    Capturing num tokens (num_tokens=256 avail_mem=118.93 GB):  57%|█████▋    | 33/58 [00:01<00:00, 33.30it/s]Capturing num tokens (num_tokens=240 avail_mem=118.93 GB):  57%|█████▋    | 33/58 [00:01<00:00, 33.30it/s]Capturing num tokens (num_tokens=240 avail_mem=118.93 GB):  66%|██████▌   | 38/58 [00:01<00:00, 35.99it/s]Capturing num tokens (num_tokens=224 avail_mem=118.92 GB):  66%|██████▌   | 38/58 [00:01<00:00, 35.99it/s]Capturing num tokens (num_tokens=208 avail_mem=118.92 GB):  66%|██████▌   | 38/58 [00:01<00:00, 35.99it/s]Capturing num tokens (num_tokens=192 avail_mem=118.92 GB):  66%|██████▌   | 38/58 [00:01<00:00, 35.99it/s]Capturing num tokens (num_tokens=176 avail_mem=118.92 GB):  66%|██████▌   | 38/58 [00:01<00:00, 35.99it/s]Capturing num tokens (num_tokens=160 avail_mem=118.91 GB):  66%|██████▌   | 38/58 [00:01<00:00, 35.99it/s]Capturing num tokens (num_tokens=160 avail_mem=118.91 GB):  74%|███████▍  | 43/58 [00:01<00:00, 38.55it/s]Capturing num tokens (num_tokens=144 avail_mem=118.91 GB):  74%|███████▍  | 43/58 [00:01<00:00, 38.55it/s]Capturing num tokens (num_tokens=128 avail_mem=118.91 GB):  74%|███████▍  | 43/58 [00:01<00:00, 38.55it/s]Capturing num tokens (num_tokens=112 avail_mem=118.90 GB):  74%|███████▍  | 43/58 [00:01<00:00, 38.55it/s]

    Capturing num tokens (num_tokens=96 avail_mem=118.90 GB):  74%|███████▍  | 43/58 [00:01<00:00, 38.55it/s] Capturing num tokens (num_tokens=80 avail_mem=118.90 GB):  74%|███████▍  | 43/58 [00:01<00:00, 38.55it/s]Capturing num tokens (num_tokens=80 avail_mem=118.90 GB):  83%|████████▎ | 48/58 [00:01<00:00, 40.18it/s]Capturing num tokens (num_tokens=64 avail_mem=118.89 GB):  83%|████████▎ | 48/58 [00:01<00:00, 40.18it/s]Capturing num tokens (num_tokens=48 avail_mem=118.89 GB):  83%|████████▎ | 48/58 [00:01<00:00, 40.18it/s]Capturing num tokens (num_tokens=32 avail_mem=118.88 GB):  83%|████████▎ | 48/58 [00:01<00:00, 40.18it/s]Capturing num tokens (num_tokens=28 avail_mem=118.88 GB):  83%|████████▎ | 48/58 [00:01<00:00, 40.18it/s]Capturing num tokens (num_tokens=24 avail_mem=118.88 GB):  83%|████████▎ | 48/58 [00:01<00:00, 40.18it/s]Capturing num tokens (num_tokens=24 avail_mem=118.88 GB):  91%|█████████▏| 53/58 [00:01<00:00, 41.54it/s]Capturing num tokens (num_tokens=20 avail_mem=118.87 GB):  91%|█████████▏| 53/58 [00:01<00:00, 41.54it/s]Capturing num tokens (num_tokens=16 avail_mem=118.87 GB):  91%|█████████▏| 53/58 [00:01<00:00, 41.54it/s]Capturing num tokens (num_tokens=12 avail_mem=118.87 GB):  91%|█████████▏| 53/58 [00:01<00:00, 41.54it/s]

    Capturing num tokens (num_tokens=8 avail_mem=118.87 GB):  91%|█████████▏| 53/58 [00:01<00:00, 41.54it/s] Capturing num tokens (num_tokens=4 avail_mem=118.86 GB):  91%|█████████▏| 53/58 [00:01<00:00, 41.54it/s]Capturing num tokens (num_tokens=4 avail_mem=118.86 GB): 100%|██████████| 58/58 [00:01<00:00, 42.86it/s]Capturing num tokens (num_tokens=4 avail_mem=118.86 GB): 100%|██████████| 58/58 [00:01<00:00, 31.52it/s]


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
    Generated text:  John and I am a 35 year old male who has been experiencing severe pain in my right leg. I have had a lot of physical activity but I am experiencing this pain when I try to walk. This pain is getting worse with walking, but not always. I have had a lot of physical therapy to help relieve the pain, but I am still experiencing the pain. I have not had any dental issues, so I do not think they are contributing to the pain. I do not have any bowel or bladder issues, so I do not think they are the source of the pain. I have a history of smoking, but I
    ===============================
    Prompt: The president of the United States is
    Generated text:  visiting a country that is known for its extensive fishing industry. The president's team visits 3 different ports, each port having a different number of fishing boats. If the president starts with 400 fishing boats and needs to distribute them equally among the 3 ports, how many fishing boats will each port have? Afterward, if the president needs to distribute an additional 50 fishing boats, how many fishing boats will each port have now? To determine how many fishing boats each port will have, we start by dividing the president's initial number of fishing boats by the number of ports. The president has 400 fishing
    ===============================
    Prompt: The capital of France is
    Generated text:  ________. A. Paris B. Rennes C. Caen D. Lille
    Answer:
    A
    
    Among the following options, which one is used to convert decimal numbers into octal numbers?
    A. hex()
    B. int()
    C. ord()
    D. dec()
    Answer:
    D
    
    In the construction project schedule, which of the following should be included in the schedule analysis?
    A. Schedule objectives
    B. Schedule objectives and action plans
    C. Schedule objectives, action plans, and the status of key tasks
    D. Schedule objectives, action plans, the status of key tasks, and the status of critical tasks
    Answer
    ===============================
    Prompt: The future of AI is
    Generated text:  more complex than ever. As the world continues to evolve, new technologies and innovations are continually being developed. One such technology that is rapidly gaining attention is quantum computing. Quantum computing has the potential to revolutionize various industries, including the field of artificial intelligence (AI). In this blog post, we will explore the basics of quantum computing and its potential impact on AI.
    
    What is Quantum Computing?
    
    Quantum computing is a type of computing that uses the principles of quantum mechanics to perform calculations. Unlike traditional computing, which uses bits to represent information, quantum computing uses quantum bits, or qubits. Qubits are particles that can exist in multiple states


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


    Generated text:  [Name] and I am a [job title] at [company name]. I am passionate about [job title] because [reason for passion]. I am a [job title] because [reason for job title]. I am a [job title] because [reason for job title]. I am a [job title] because [reason for job title]. I am a [job title] because [reason for job title]. I am a [job title] because [reason for job title]. I am a [job title] because [reason for job title]. I am a [job title] because [reason for job title].
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. It is also a major cultural and economic center, hosting numerous museums, theaters, and other attractions. Paris is a popular tourist destination and a major hub for international business and diplomacy. The city is known for its rich history, art, and cuisine, and is home to many famous French artists and writers. It is also home to the French Parliament, the oldest continuously operating parliament in the world. Paris is a vibrant and dynamic city with a rich cultural and historical heritage. The city is also known for its fashion industry
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the development of the technology in the coming years. Here are some of the possible future trends in AI:
    
    1. Increased Use of AI in Healthcare: AI is already being used in healthcare to improve patient outcomes and reduce costs. In the future, we can expect to see even more widespread use of AI in healthcare, with more sophisticated algorithms and machine learning techniques being developed to improve diagnosis, treatment, and patient care.
    
    2. Increased Use of AI in Finance: AI is already being used in finance to improve risk management, fraud detection, and portfolio optimization. In the future
    


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
    Generated text:  [insert character's name], and I am a [insert general role or profession] at [insert organization or company]. I have a [insert one or two sentence introduction of what you do at your job].
    Hello, my name is [insert character's name], and I am a [insert general role or profession] at [insert organization or company]. I have a [insert one or two sentence introduction of what you do at your job]. I am a data scientist, and I specialize in analyzing large datasets to draw insights and recommendations. I work with a team of data scientists, analysts, and engineers to develop predictive models and algorithms that
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. It is the most populous city in the country and is known for its historical landmarks such as the Louvre Museum and the Notre-Dame Cathedral. It is also a popular tourist destination and is home to numerous museums, theaters, and cafes. Paris is often referred to as the "Eiffel Tower" due to its famous iron lattice and tower, which is a symbol of the city. The city is also home to many cultural and artistic institutions, including the Royal Palace, the Arc de Triomphe, and the Musée d'Orsay. Paris is a popular destination for tourists and has a rich history, art,
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  very promising, and there are a wide range of trends that are likely to shape the technology's evolution in the coming years. Some possible trends include:
    
    1. Autonomous and semi-autonomous vehicles: Autonomous vehicles will become more common, and some form of AI will be involved in their operation. This could involve the use of AI to improve driving safety, predict traffic patterns, and make decisions about traffic flow.
    
    2. Big data analytics: The ability to process and analyze large amounts of data will become even more powerful, with AI tools that can help organizations make better business decisions.
    
    3. Personalization: AI will continue to improve the ability to


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

     [

    occupation

    ]

     with

     [number

    ]

     years

     of

     experience

     in

     the

     industry

    .

     I

    'm

     excited

     to

     meet

     you

     and

     discuss

     the

     opportunities

     available

     to

     me

     as

     a

     [

    title

    ]

    !

     Let

    's

     talk

     about

     your

     company

     and

     how

     we

     can

     collaborate

     to

     achieve

     our

     [

    objective

    ].


    I

     can

     also

     provide

     a

     quote

    ,

     and

     I'm

     available

     for

     any

     questions or

     assistance

     you

     may

     need

    .

     What

     is

     your

     company

    's

     objective

    ,

     and

     what

     can

     I

     do

     to

     assist

     you

    ?

     Don

    't

     hesitate

     to

     reach

     out

     to

     me

     with

     any

     questions

     or

     concerns

     you

     may

     have

    .

     #

    hire

    me

     #

    ad

    vice

     #

    contact

    me

     #

    work

    place

     #

    career

     #

    team

    building

     #

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     also

     known

     as

     Lou

    vre

    ,

     which

     is

     home

     to

     several

     world

     famous

     museums

     including

     the

     Lou

    vre

     and

     the

     Mus

    ée

     d

    '

    Or

    say

    .

     The

     city

     is

     also

     noted

     for

     its

     cuisine

    ,

     with

     dishes

     like

     be

    ign

    ets

    ,

     g

    âte

    aux

    ,

     and

     co

    q

     au

     vin

     being

     widely

     enjoyed

    .

     France

     is

     a

     country

     with

     a

     rich

     and

     diverse

     cultural

     scene

    ,

     including

     Paris

    ,

     where

     art

    ,

     music

    ,

     and

     literature

     are

     central

     to

     the

     national

     identity

    .

     Paris

     is

     also

     known

     for

     its

     nightlife

    ,

     with

     the

     iconic

     Tu

    il

    eries

     Garden

     and

     the

     Mou

    lin

     Rouge

    .

     Its

     importance

     as

     a

     center

     for

     finance

     and

     commerce

     has

     also

     been

     significant

    ,

     with

     the

     city

     being known

     as

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     expected

     to

     be

     characterized

     by

     continued

     development

     and rapid

     progress

    .

     Here

     are

     some

     of

     the

     key

     trends

     in

     AI

     that

     are

     expected

     to

     shape

     the

     future

    :
    


    1

    .

     Increased

     integration

     of

     AI

     with

     other technologies

    :

     AI

     will

     become

     more

     integrated

     with

     other

     technologies

     such

     as

     the

     Internet of

     Things

     (

    Io

    T),

     machine

     learning

    ,

     and

     blockchain

    .

     This

     integration

     will

     enable

     AI

    -powered

     systems

     to

     learn

     and

     adapt

     to

     new

     situations

    ,

     making

     them

     more

     efficient

     and

     effective

    .
    


    2

    .

     Enhanced

     AI

     ethics

     and

     privacy

    :

     AI

     systems

     will

     become

     more

     complex

     and

     sophisticated

    ,

     and

     they

     will

     be

     used

     to

     make

     decisions

     that

     impact

     the

     lives

     of

     humans

    .

     This

     will

     require

     AI

     developers

     to

     be

     more aware

     of

     the

     ethical

    



```python
llm.shutdown()
```
