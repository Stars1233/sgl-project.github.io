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


    2026-04-02 17:59:48.075 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 17:59:48] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 17:59:48.075 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 17:59:48] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 17:59:48.075 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 17:59:48] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 17:59:48.075 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 17:59:48] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 17:59:48.075 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 17:59:48] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.40it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.38it/s]


    2026-04-02 17:59:52,649 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 17:59:52] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:42,  2.85s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:42,  2.85s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<01:09,  1.24s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<01:09,  1.24s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:03<01:09,  1.24s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:27,  2.00it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:27,  2.00it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:03<00:27,  2.00it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:03<00:14,  3.47it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:03<00:14,  3.47it/s]Compiling num tokens (num_tokens=4608):  10%|█         | 6/58 [00:03<00:14,  3.47it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:03<00:09,  5.20it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:03<00:09,  5.20it/s]Compiling num tokens (num_tokens=3840):  14%|█▍        | 8/58 [00:03<00:09,  5.20it/s]Compiling num tokens (num_tokens=3584):  14%|█▍        | 8/58 [00:03<00:09,  5.20it/s]Compiling num tokens (num_tokens=3328):  14%|█▍        | 8/58 [00:03<00:09,  5.20it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:03<00:04,  9.34it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:03<00:04,  9.34it/s]Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:03<00:04,  9.34it/s]Compiling num tokens (num_tokens=2560):  21%|██        | 12/58 [00:03<00:04,  9.34it/s]

    Compiling num tokens (num_tokens=2304):  21%|██        | 12/58 [00:03<00:04,  9.34it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:03<00:03, 13.88it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:03<00:03, 13.88it/s]Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:03<00:03, 13.88it/s]Compiling num tokens (num_tokens=1536):  28%|██▊       | 16/58 [00:03<00:03, 13.88it/s]Compiling num tokens (num_tokens=1280):  28%|██▊       | 16/58 [00:03<00:03, 13.88it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:03<00:02, 17.62it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:03<00:02, 17.62it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:03<00:02, 17.62it/s] 

    Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:03<00:02, 17.62it/s]Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:03<00:02, 17.62it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:03<00:01, 21.34it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:03<00:01, 21.34it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:03<00:01, 21.34it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:03<00:01, 21.34it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:03<00:01, 21.34it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:03<00:01, 24.67it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:03<00:01, 24.67it/s]

    Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:03<00:01, 24.67it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:03<00:01, 24.67it/s]Compiling num tokens (num_tokens=416):  48%|████▊     | 28/58 [00:03<00:01, 24.67it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:04<00:00, 26.43it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:04<00:00, 26.43it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:04<00:00, 26.43it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:04<00:00, 26.43it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:04<00:00, 26.43it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:04<00:00, 28.96it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:04<00:00, 28.96it/s]

    Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:04<00:00, 28.96it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:04<00:00, 28.96it/s]Compiling num tokens (num_tokens=208):  62%|██████▏   | 36/58 [00:04<00:00, 28.96it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:04<00:00, 29.42it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:04<00:00, 29.42it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:04<00:00, 29.42it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:04<00:00, 29.42it/s]Compiling num tokens (num_tokens=144):  69%|██████▉   | 40/58 [00:04<00:00, 29.42it/s]

    Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:04<00:00, 30.95it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:04<00:00, 30.95it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:04<00:00, 30.95it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:04<00:00, 30.95it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:04<00:00, 30.95it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:04<00:00, 32.87it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:04<00:00, 32.87it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:04<00:00, 32.87it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:04<00:00, 32.87it/s]Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:04<00:00, 32.87it/s]Compiling num tokens (num_tokens=24):  83%|████████▎ | 48/58 [00:04<00:00, 32.87it/s]Compiling num tokens (num_tokens=20):  83%|████████▎ | 48/58 [00:04<00:00, 32.87it/s]Compiling num tokens (num_tokens=16):  83%|████████▎ | 48/58 [00:04<00:00, 32.87it/s]

    Compiling num tokens (num_tokens=16):  95%|█████████▍| 55/58 [00:04<00:00, 42.14it/s]Compiling num tokens (num_tokens=12):  95%|█████████▍| 55/58 [00:04<00:00, 42.14it/s]Compiling num tokens (num_tokens=8):  95%|█████████▍| 55/58 [00:04<00:00, 42.14it/s] Compiling num tokens (num_tokens=4):  95%|█████████▍| 55/58 [00:04<00:00, 42.14it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 12.61it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=119.44 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=119.41 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=119.41 GB):   3%|▎         | 2/58 [00:00<00:03, 17.82it/s]Capturing num tokens (num_tokens=7168 avail_mem=119.40 GB):   3%|▎         | 2/58 [00:00<00:03, 17.82it/s]Capturing num tokens (num_tokens=6656 avail_mem=119.40 GB):   3%|▎         | 2/58 [00:00<00:03, 17.82it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=119.40 GB):   3%|▎         | 2/58 [00:00<00:03, 17.82it/s]Capturing num tokens (num_tokens=6144 avail_mem=119.40 GB):   9%|▊         | 5/58 [00:00<00:02, 19.87it/s]Capturing num tokens (num_tokens=5632 avail_mem=119.40 GB):   9%|▊         | 5/58 [00:00<00:02, 19.87it/s]Capturing num tokens (num_tokens=5120 avail_mem=119.40 GB):   9%|▊         | 5/58 [00:00<00:02, 19.87it/s]Capturing num tokens (num_tokens=4608 avail_mem=119.39 GB):   9%|▊         | 5/58 [00:00<00:02, 19.87it/s]Capturing num tokens (num_tokens=4608 avail_mem=119.39 GB):  14%|█▍        | 8/58 [00:00<00:02, 22.50it/s]Capturing num tokens (num_tokens=4096 avail_mem=119.39 GB):  14%|█▍        | 8/58 [00:00<00:02, 22.50it/s]Capturing num tokens (num_tokens=3840 avail_mem=119.39 GB):  14%|█▍        | 8/58 [00:00<00:02, 22.50it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=119.38 GB):  14%|█▍        | 8/58 [00:00<00:02, 22.50it/s]Capturing num tokens (num_tokens=3328 avail_mem=119.38 GB):  14%|█▍        | 8/58 [00:00<00:02, 22.50it/s]Capturing num tokens (num_tokens=3328 avail_mem=119.38 GB):  21%|██        | 12/58 [00:00<00:01, 26.58it/s]Capturing num tokens (num_tokens=3072 avail_mem=119.38 GB):  21%|██        | 12/58 [00:00<00:01, 26.58it/s]Capturing num tokens (num_tokens=2816 avail_mem=119.38 GB):  21%|██        | 12/58 [00:00<00:01, 26.58it/s]Capturing num tokens (num_tokens=2560 avail_mem=119.37 GB):  21%|██        | 12/58 [00:00<00:01, 26.58it/s]Capturing num tokens (num_tokens=2304 avail_mem=119.37 GB):  21%|██        | 12/58 [00:00<00:01, 26.58it/s]Capturing num tokens (num_tokens=2304 avail_mem=119.37 GB):  28%|██▊       | 16/58 [00:00<00:01, 29.89it/s]Capturing num tokens (num_tokens=2048 avail_mem=119.37 GB):  28%|██▊       | 16/58 [00:00<00:01, 29.89it/s]Capturing num tokens (num_tokens=1792 avail_mem=119.36 GB):  28%|██▊       | 16/58 [00:00<00:01, 29.89it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=119.36 GB):  28%|██▊       | 16/58 [00:00<00:01, 29.89it/s]Capturing num tokens (num_tokens=1280 avail_mem=119.36 GB):  28%|██▊       | 16/58 [00:00<00:01, 29.89it/s]Capturing num tokens (num_tokens=1280 avail_mem=119.36 GB):  34%|███▍      | 20/58 [00:00<00:01, 32.13it/s]Capturing num tokens (num_tokens=1024 avail_mem=119.34 GB):  34%|███▍      | 20/58 [00:00<00:01, 32.13it/s]Capturing num tokens (num_tokens=960 avail_mem=119.35 GB):  34%|███▍      | 20/58 [00:00<00:01, 32.13it/s] Capturing num tokens (num_tokens=896 avail_mem=119.35 GB):  34%|███▍      | 20/58 [00:00<00:01, 32.13it/s]Capturing num tokens (num_tokens=832 avail_mem=119.34 GB):  34%|███▍      | 20/58 [00:00<00:01, 32.13it/s]Capturing num tokens (num_tokens=832 avail_mem=119.34 GB):  41%|████▏     | 24/58 [00:00<00:01, 32.24it/s]Capturing num tokens (num_tokens=768 avail_mem=119.34 GB):  41%|████▏     | 24/58 [00:00<00:01, 32.24it/s]

    Capturing num tokens (num_tokens=704 avail_mem=119.34 GB):  41%|████▏     | 24/58 [00:00<00:01, 32.24it/s]Capturing num tokens (num_tokens=640 avail_mem=119.33 GB):  41%|████▏     | 24/58 [00:00<00:01, 32.24it/s]Capturing num tokens (num_tokens=576 avail_mem=119.33 GB):  41%|████▏     | 24/58 [00:00<00:01, 32.24it/s]Capturing num tokens (num_tokens=576 avail_mem=119.33 GB):  48%|████▊     | 28/58 [00:00<00:00, 32.64it/s]Capturing num tokens (num_tokens=512 avail_mem=119.32 GB):  48%|████▊     | 28/58 [00:00<00:00, 32.64it/s]Capturing num tokens (num_tokens=480 avail_mem=119.34 GB):  48%|████▊     | 28/58 [00:00<00:00, 32.64it/s]Capturing num tokens (num_tokens=448 avail_mem=119.34 GB):  48%|████▊     | 28/58 [00:01<00:00, 32.64it/s]Capturing num tokens (num_tokens=416 avail_mem=119.33 GB):  48%|████▊     | 28/58 [00:01<00:00, 32.64it/s]Capturing num tokens (num_tokens=416 avail_mem=119.33 GB):  55%|█████▌    | 32/58 [00:01<00:00, 34.23it/s]Capturing num tokens (num_tokens=384 avail_mem=119.33 GB):  55%|█████▌    | 32/58 [00:01<00:00, 34.23it/s]

    Capturing num tokens (num_tokens=352 avail_mem=119.33 GB):  55%|█████▌    | 32/58 [00:01<00:00, 34.23it/s]Capturing num tokens (num_tokens=320 avail_mem=119.32 GB):  55%|█████▌    | 32/58 [00:01<00:00, 34.23it/s]Capturing num tokens (num_tokens=288 avail_mem=119.32 GB):  55%|█████▌    | 32/58 [00:01<00:00, 34.23it/s]Capturing num tokens (num_tokens=288 avail_mem=119.32 GB):  62%|██████▏   | 36/58 [00:01<00:00, 35.44it/s]Capturing num tokens (num_tokens=256 avail_mem=119.32 GB):  62%|██████▏   | 36/58 [00:01<00:00, 35.44it/s]Capturing num tokens (num_tokens=240 avail_mem=119.31 GB):  62%|██████▏   | 36/58 [00:01<00:00, 35.44it/s]Capturing num tokens (num_tokens=224 avail_mem=119.31 GB):  62%|██████▏   | 36/58 [00:01<00:00, 35.44it/s]Capturing num tokens (num_tokens=208 avail_mem=119.31 GB):  62%|██████▏   | 36/58 [00:01<00:00, 35.44it/s]Capturing num tokens (num_tokens=208 avail_mem=119.31 GB):  69%|██████▉   | 40/58 [00:01<00:00, 35.78it/s]Capturing num tokens (num_tokens=192 avail_mem=119.31 GB):  69%|██████▉   | 40/58 [00:01<00:00, 35.78it/s]

    Capturing num tokens (num_tokens=176 avail_mem=119.30 GB):  69%|██████▉   | 40/58 [00:01<00:00, 35.78it/s]Capturing num tokens (num_tokens=160 avail_mem=119.30 GB):  69%|██████▉   | 40/58 [00:01<00:00, 35.78it/s]Capturing num tokens (num_tokens=144 avail_mem=119.29 GB):  69%|██████▉   | 40/58 [00:01<00:00, 35.78it/s]Capturing num tokens (num_tokens=144 avail_mem=119.29 GB):  76%|███████▌  | 44/58 [00:01<00:00, 36.52it/s]Capturing num tokens (num_tokens=128 avail_mem=119.29 GB):  76%|███████▌  | 44/58 [00:01<00:00, 36.52it/s]Capturing num tokens (num_tokens=112 avail_mem=119.29 GB):  76%|███████▌  | 44/58 [00:01<00:00, 36.52it/s]Capturing num tokens (num_tokens=96 avail_mem=119.28 GB):  76%|███████▌  | 44/58 [00:01<00:00, 36.52it/s] Capturing num tokens (num_tokens=80 avail_mem=119.28 GB):  76%|███████▌  | 44/58 [00:01<00:00, 36.52it/s]Capturing num tokens (num_tokens=80 avail_mem=119.28 GB):  83%|████████▎ | 48/58 [00:01<00:00, 37.17it/s]Capturing num tokens (num_tokens=64 avail_mem=119.28 GB):  83%|████████▎ | 48/58 [00:01<00:00, 37.17it/s]

    Capturing num tokens (num_tokens=48 avail_mem=119.28 GB):  83%|████████▎ | 48/58 [00:01<00:00, 37.17it/s]Capturing num tokens (num_tokens=32 avail_mem=119.27 GB):  83%|████████▎ | 48/58 [00:01<00:00, 37.17it/s]Capturing num tokens (num_tokens=28 avail_mem=119.27 GB):  83%|████████▎ | 48/58 [00:01<00:00, 37.17it/s]Capturing num tokens (num_tokens=28 avail_mem=119.27 GB):  90%|████████▉ | 52/58 [00:01<00:00, 37.42it/s]Capturing num tokens (num_tokens=24 avail_mem=119.27 GB):  90%|████████▉ | 52/58 [00:01<00:00, 37.42it/s]Capturing num tokens (num_tokens=20 avail_mem=119.26 GB):  90%|████████▉ | 52/58 [00:01<00:00, 37.42it/s]Capturing num tokens (num_tokens=16 avail_mem=119.26 GB):  90%|████████▉ | 52/58 [00:01<00:00, 37.42it/s]Capturing num tokens (num_tokens=12 avail_mem=119.26 GB):  90%|████████▉ | 52/58 [00:01<00:00, 37.42it/s]Capturing num tokens (num_tokens=12 avail_mem=119.26 GB):  97%|█████████▋| 56/58 [00:01<00:00, 38.12it/s]Capturing num tokens (num_tokens=8 avail_mem=119.25 GB):  97%|█████████▋| 56/58 [00:01<00:00, 38.12it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=119.25 GB):  97%|█████████▋| 56/58 [00:01<00:00, 38.12it/s]Capturing num tokens (num_tokens=4 avail_mem=119.25 GB): 100%|██████████| 58/58 [00:01<00:00, 33.45it/s]


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
    Generated text:  Alex and I'm a computer scientist and developer. I'm passionate about creating programs that solve problems and make the world better. I'm also a lifelong learner and have been using my skills to improve my understanding of the world. I'm passionate about making my work accessible to everyone, regardless of their skill level. 
    I enjoy working on projects that require teamwork and collaboration, and I'm always looking for new ways to improve my code and make it more efficient. I'm also really good at problem-solving and can often find creative solutions to complex problems. I enjoy coding in languages like Python and JavaScript, but I'm open to trying out other
    ===============================
    Prompt: The president of the United States is
    Generated text:  an elected office, and the president has no term limits. However, presidents may not be directly elected by the people of the United States. Instead, they are directly elected by their fellow citizens, either by a proportional representation system or a winner-take-all system. What is the name of the system used by the United States?
    The system used by the United States for electing the president is called the single transferable vote system. This system allows the citizens of the United States to rank their preferred candidates for the presidency. Candidates are then eliminated based on the number of votes they receive and the number of votes they receive from the person who
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. Paris is the cultural center of France. For example, the Louvre is the oldest museum in the world. The Eiffel Tower is also one of the most famous landmarks in the world. Paris is known for its beautiful architecture and its culinary delights, so people often visit Paris to eat and drink. There are many historical places to visit in Paris, such as the Eiffel Tower, the Louvre, Notre Dame Cathedral and the Champs-Élysées. The city also has many museums, including the Musée d'Orsay and the Musée d'Orsay. Paris is a very big city,
    ===============================
    Prompt: The future of AI is
    Generated text:  very bright, with a wide range of applications in various industries and fields. However, the current level of AI development and its impact on society are still not significant. In order to facilitate the continuous progress of AI technology, the National Administration of Science and Technology (NAST) has formulated and implemented the "Plan for the Development and Application of Artificial Intelligence (AI) 2020-2022" (hereinafter referred to as the "Plan") to coordinate and promote the development of AI technology. The Plan determines the main tasks of the National Administration of Science and Technology (NAST) and the current level of


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


    Generated text:  [Name] and I'm a [job title] at [company name]. I'm a [job title] at [company name], and I'm excited to be here today. I'm a [job title] at [company name], and I'm excited to be here today. I'm a [job title] at [company name], and I'm excited to be here today. I'm a [job title] at [company name], and I'm excited to be here today. I'm a [job title] at [company name], and I'm excited to be here today. I'm a [job title]
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French National Library, and the French Academy of Sciences. Paris is a cultural and economic hub, known for its rich history, art, and cuisine. It is a popular tourist destination, attracting millions of visitors each year. The city is also home to the French Parliament, the French National Library, and the French Academy of Sciences. Paris is a cultural and economic hub, known for its rich history, art, and cuisine. It is a popular tourist
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by rapid advancements in areas such as machine learning, natural language processing, and computer vision. Some potential future trends include:
    
    1. Increased use of AI in healthcare: AI is already being used in medical diagnosis and treatment, but there is potential for even greater use in the future. AI-powered diagnostic tools could identify diseases earlier and more accurately than current methods, leading to better patient outcomes.
    
    2. AI in manufacturing: AI is already being used in manufacturing to optimize production processes and improve quality control. As AI technology continues to improve, we may see even greater use in manufacturing, with AI-powered robots and autonomous vehicles becoming more
    


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
    Generated text:  [insert name]. I am [insert number]. And I live in [insert location]. I enjoy [insert hobby or activity]. I am [insert age], and I am [insert gender]. I believe in [insert religion or belief system]. I am [insert occupation]. And I love [insert favorite activity or hobby]. I am [insert personality trait or style]. And I strive to [insert personal goal or aspiration]. I am [insert health or fitness level]. And I am always looking for [insert reasons why]. Thank you for asking! #SelfIntroduction #PersonalityTest #CharacterDetails #FriendlyFriendly
    
    Hey there! My
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, located on the Left Bank of the Seine and is a major cultural and economic center. It is the 11th largest city by population in the world and is known for its unique architecture, rich history, and bustling street life. Visitors often come to Paris to enjoy its art, history, and delicious cuisine. Paris is a bustling city with many attractions, from stunning museums to breathtaking nightlife, making it a popular destination for tourists and locals alike. The city is home to many famous landmarks, including Notre-Dame Cathedral, the Louvre Museum, and the Eiffel Tower. Paris also has a rich cultural heritage,
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be characterized by increased automation and integration with other technologies, including the rise of deep learning, neural networks, and machine learning. AI systems are likely to become more sophisticated, enabling them to learn and adapt to new situations faster than ever before. This could lead to more efficient decision-making, better healthcare outcomes, and improved productivity for businesses. However, there are also concerns about job displacement and privacy issues associated with AI, as well as ethical considerations around the use of AI in shaping society. As AI continues to evolve, it is likely to become an increasingly important part of our lives and our economy. AI is likely to become more integrated


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

    Character

     Name

    ],

     and

     I

     am

     a

     [

    Title

    /

    Role

    ].

     I

     am

     [

    Age

    ]

     years

     old

    ,

     [

    Role

    ],

     and

     I

     currently

     [

    Location

    ].

     I

     have

     always

     been

     [

    Mot

    iv

    ational

     Factor

    ]

     and

     have

     always

     [

    Achie

    ved

     Goal

    ].

     I

     am

     [

    Person

    ality

     Trait

    ],

     [

    Ad

    apt

    ability

    ],

     [

    Int

    elligence

    ],

     [

    M

    aturity

    ],

     [

    Character

    istics

    ],

     and

     [

    Strength

    s

    ].

     I

     am

     [

    Your

     Job

     Title

    ]

     with

     [

    Your

     Name

    ],

     and

     I

     am

     always

     [

    Your

     Passion

    /

    Interest

    ].

     I

     love

     [

    Name

     of

     My

     Hobby

    /

    Activity

    ]

     and

     I

     strive

     to

     [

    Your

     Goal

     or

     Dream

    ].

     I

     am

     [

    Your

     Unique

     Character

    istic

     or

     Feature

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     
    


    To

     verify

     this

     statement

    ,

     consider

     the

     following

     reasoning

    :
    


    1

    .

     A

     city

     is

     a

     place

     where

     people

     live

    ,

     work

    ,

     and

     interact

     with

     one

     another

    .

     In

     France

    ,

     Paris

     is

     undoubtedly

     a

     city

    .


    2

    .

     Being

     the

     capital

     of

     a

     country

    ,

     Paris

     serves

     as

     the

     main

     administrative

     center

     and

     economic

     hub

     of

     France

    .


    3

    .

     As

     a

     major

     city

    ,

     Paris

     is

     also

     a

     cultural

     and

     educational

     center

     where

     people

     of

     all

     ages

     can

     visit

     and

     learn

     about

     different

     aspects

     of

     French

     culture

     and

     history

    .
    


    Therefore

    ,

     based

     on

     these

     observations

    ,

     we

     can

     confidently

     state

     that

     the

     capital

     of

     France

    ,

     Paris

    ,

     is

     indeed

     a

     city

    .

     The

     answer

     is

    :

     Paris

     is

     the

     capital

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     characterized

     by

     rapid

     progress

    ,

     innovation

    ,

     and

     integration

     with

     other

     technologies

    ,

     leading

     to

     a

     variety

     of

     potential

     trends

    .

     Here

     are

     some

     potential

     future

     trends

     that

     may

     emerge

    :
    


    1

    .

     Increased

     AI

     performance

    :

     As

     AI

     technology

     continues

     to

     improve

    ,

     we

     can

     expect

     to

     see

     greater

     performance

     in

     areas

     like

     natural

     language

     processing

    ,

     image

     recognition

    ,

     and

     autonomous

     vehicles

    .

     The

     next

     few

     years

     will

     be

     critical

     for

     this

     progress

    ,

     as

     researchers

     and

     manufacturers

     will

     work

     to

     improve

     the

     efficiency

     and

     accuracy

     of

     AI

     systems

    .
    


    2

    .

     AI

     integration

     with

     other

     technologies

    :

     As

     AI

     becomes

     more

     integrated

     with

     other

     technologies

    ,

     we

     can

     expect

     to

     see

     more

     advanced

     applications

     and

     use

     cases

    .

     This

     will

     likely

     lead

    



```python
llm.shutdown()
```
