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


    2026-04-02 09:53:24.955 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:53:24] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 09:53:24.955 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:53:24] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 09:53:24.955 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:53:24] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 09:53:24.955 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:53:24] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 09:53:24.955 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:53:24] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  7.30it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  7.29it/s]


    2026-04-02 09:53:29,359 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 09:53:29] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:34,  2.72s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:34,  2.72s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<01:06,  1.19s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<01:06,  1.19s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<01:06,  1.19s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:25,  2.10it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:25,  2.10it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:25,  2.10it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:03<00:14,  3.61it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:03<00:14,  3.61it/s]Compiling num tokens (num_tokens=4608):  10%|█         | 6/58 [00:03<00:14,  3.61it/s]Compiling num tokens (num_tokens=4096):  10%|█         | 6/58 [00:03<00:14,  3.61it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:03<00:07,  6.37it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:03<00:07,  6.37it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:03<00:07,  6.37it/s]Compiling num tokens (num_tokens=3328):  16%|█▌        | 9/58 [00:03<00:07,  6.37it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:03<00:04,  9.36it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:03<00:04,  9.36it/s]Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:03<00:04,  9.36it/s]Compiling num tokens (num_tokens=2560):  21%|██        | 12/58 [00:03<00:04,  9.36it/s]Compiling num tokens (num_tokens=2304):  21%|██        | 12/58 [00:03<00:04,  9.36it/s]

    Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:03<00:02, 14.05it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:03<00:02, 14.05it/s]Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:03<00:02, 14.05it/s]Compiling num tokens (num_tokens=1536):  28%|██▊       | 16/58 [00:03<00:02, 14.05it/s]Compiling num tokens (num_tokens=1280):  28%|██▊       | 16/58 [00:03<00:02, 14.05it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:03<00:02, 17.99it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:03<00:02, 17.99it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:03<00:02, 17.99it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:03<00:02, 17.99it/s]

    Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:03<00:02, 17.99it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:03<00:01, 21.70it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:03<00:01, 21.70it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:03<00:01, 21.70it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:03<00:01, 21.70it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:03<00:01, 21.70it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:03<00:01, 24.56it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:03<00:01, 24.56it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:03<00:01, 24.56it/s]

    Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:03<00:01, 24.56it/s]Compiling num tokens (num_tokens=416):  48%|████▊     | 28/58 [00:03<00:01, 24.56it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:00, 28.03it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:00, 28.03it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:00, 28.03it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:00, 28.03it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:00, 28.03it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:03<00:00, 29.80it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:03<00:00, 29.80it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:03<00:00, 29.80it/s]

    Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:04<00:00, 29.80it/s]Compiling num tokens (num_tokens=208):  62%|██████▏   | 36/58 [00:04<00:00, 29.80it/s]Compiling num tokens (num_tokens=192):  62%|██████▏   | 36/58 [00:04<00:00, 29.80it/s]Compiling num tokens (num_tokens=176):  62%|██████▏   | 36/58 [00:04<00:00, 29.80it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:04<00:00, 36.35it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:04<00:00, 36.35it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:04<00:00, 36.35it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:04<00:00, 36.35it/s]Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:04<00:00, 36.35it/s]Compiling num tokens (num_tokens=96):  72%|███████▏  | 42/58 [00:04<00:00, 36.35it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:04<00:00, 39.72it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:04<00:00, 39.72it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:04<00:00, 39.72it/s]

    Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:04<00:00, 39.72it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:04<00:00, 39.72it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:04<00:00, 39.72it/s]Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:04<00:00, 39.72it/s]Compiling num tokens (num_tokens=20):  81%|████████  | 47/58 [00:04<00:00, 39.72it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:04<00:00, 46.58it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:04<00:00, 46.58it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:04<00:00, 46.58it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:04<00:00, 46.58it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:04<00:00, 46.58it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 13.41it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=132.42 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=132.39 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=132.39 GB):   3%|▎         | 2/58 [00:00<00:02, 18.80it/s]Capturing num tokens (num_tokens=7168 avail_mem=132.38 GB):   3%|▎         | 2/58 [00:00<00:02, 18.80it/s]Capturing num tokens (num_tokens=6656 avail_mem=132.38 GB):   3%|▎         | 2/58 [00:00<00:02, 18.80it/s]Capturing num tokens (num_tokens=6144 avail_mem=132.38 GB):   3%|▎         | 2/58 [00:00<00:02, 18.80it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=132.38 GB):   9%|▊         | 5/58 [00:00<00:02, 22.03it/s]Capturing num tokens (num_tokens=5632 avail_mem=132.38 GB):   9%|▊         | 5/58 [00:00<00:02, 22.03it/s]Capturing num tokens (num_tokens=5120 avail_mem=132.38 GB):   9%|▊         | 5/58 [00:00<00:02, 22.03it/s]Capturing num tokens (num_tokens=4608 avail_mem=132.37 GB):   9%|▊         | 5/58 [00:00<00:02, 22.03it/s]Capturing num tokens (num_tokens=4608 avail_mem=132.37 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.88it/s]Capturing num tokens (num_tokens=4096 avail_mem=132.37 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.88it/s]Capturing num tokens (num_tokens=3840 avail_mem=132.37 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.88it/s]Capturing num tokens (num_tokens=3584 avail_mem=132.37 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.88it/s]Capturing num tokens (num_tokens=3328 avail_mem=132.36 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.88it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=132.36 GB):  21%|██        | 12/58 [00:00<00:01, 30.04it/s]Capturing num tokens (num_tokens=3072 avail_mem=132.36 GB):  21%|██        | 12/58 [00:00<00:01, 30.04it/s]Capturing num tokens (num_tokens=2816 avail_mem=132.36 GB):  21%|██        | 12/58 [00:00<00:01, 30.04it/s]Capturing num tokens (num_tokens=2560 avail_mem=132.35 GB):  21%|██        | 12/58 [00:00<00:01, 30.04it/s]Capturing num tokens (num_tokens=2304 avail_mem=132.35 GB):  21%|██        | 12/58 [00:00<00:01, 30.04it/s]Capturing num tokens (num_tokens=2048 avail_mem=132.35 GB):  21%|██        | 12/58 [00:00<00:01, 30.04it/s]Capturing num tokens (num_tokens=2048 avail_mem=132.35 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.39it/s]Capturing num tokens (num_tokens=1792 avail_mem=132.34 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.39it/s]Capturing num tokens (num_tokens=1536 avail_mem=132.34 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.39it/s]Capturing num tokens (num_tokens=1280 avail_mem=132.34 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.39it/s]Capturing num tokens (num_tokens=1024 avail_mem=132.32 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.39it/s]

    Capturing num tokens (num_tokens=960 avail_mem=132.33 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.39it/s] Capturing num tokens (num_tokens=960 avail_mem=132.33 GB):  38%|███▊      | 22/58 [00:00<00:00, 38.24it/s]Capturing num tokens (num_tokens=896 avail_mem=132.33 GB):  38%|███▊      | 22/58 [00:00<00:00, 38.24it/s]Capturing num tokens (num_tokens=832 avail_mem=132.32 GB):  38%|███▊      | 22/58 [00:00<00:00, 38.24it/s]Capturing num tokens (num_tokens=768 avail_mem=132.32 GB):  38%|███▊      | 22/58 [00:00<00:00, 38.24it/s]Capturing num tokens (num_tokens=704 avail_mem=132.32 GB):  38%|███▊      | 22/58 [00:00<00:00, 38.24it/s]Capturing num tokens (num_tokens=640 avail_mem=132.31 GB):  38%|███▊      | 22/58 [00:00<00:00, 38.24it/s]Capturing num tokens (num_tokens=640 avail_mem=132.31 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.73it/s]Capturing num tokens (num_tokens=576 avail_mem=132.31 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.73it/s]Capturing num tokens (num_tokens=512 avail_mem=132.30 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.73it/s]Capturing num tokens (num_tokens=480 avail_mem=132.32 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.73it/s]

    Capturing num tokens (num_tokens=448 avail_mem=132.32 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.73it/s]Capturing num tokens (num_tokens=416 avail_mem=132.31 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.73it/s]Capturing num tokens (num_tokens=416 avail_mem=132.31 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.80it/s]Capturing num tokens (num_tokens=384 avail_mem=132.31 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.80it/s]Capturing num tokens (num_tokens=352 avail_mem=132.31 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.80it/s]Capturing num tokens (num_tokens=320 avail_mem=132.30 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.80it/s]Capturing num tokens (num_tokens=288 avail_mem=132.30 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.80it/s]Capturing num tokens (num_tokens=256 avail_mem=132.30 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.80it/s]Capturing num tokens (num_tokens=256 avail_mem=132.30 GB):  64%|██████▍   | 37/58 [00:00<00:00, 42.95it/s]Capturing num tokens (num_tokens=240 avail_mem=132.29 GB):  64%|██████▍   | 37/58 [00:00<00:00, 42.95it/s]Capturing num tokens (num_tokens=224 avail_mem=132.29 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.95it/s]Capturing num tokens (num_tokens=208 avail_mem=132.29 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.95it/s]

    Capturing num tokens (num_tokens=192 avail_mem=132.29 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.95it/s]Capturing num tokens (num_tokens=176 avail_mem=132.28 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.95it/s]Capturing num tokens (num_tokens=176 avail_mem=132.28 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.98it/s]Capturing num tokens (num_tokens=160 avail_mem=132.28 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.98it/s]Capturing num tokens (num_tokens=144 avail_mem=132.28 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.98it/s]Capturing num tokens (num_tokens=128 avail_mem=132.27 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.98it/s]Capturing num tokens (num_tokens=112 avail_mem=132.27 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.98it/s]Capturing num tokens (num_tokens=96 avail_mem=132.27 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.98it/s] Capturing num tokens (num_tokens=96 avail_mem=132.27 GB):  81%|████████  | 47/58 [00:01<00:00, 44.39it/s]Capturing num tokens (num_tokens=80 avail_mem=132.26 GB):  81%|████████  | 47/58 [00:01<00:00, 44.39it/s]Capturing num tokens (num_tokens=64 avail_mem=132.26 GB):  81%|████████  | 47/58 [00:01<00:00, 44.39it/s]Capturing num tokens (num_tokens=48 avail_mem=132.26 GB):  81%|████████  | 47/58 [00:01<00:00, 44.39it/s]

    Capturing num tokens (num_tokens=32 avail_mem=132.25 GB):  81%|████████  | 47/58 [00:01<00:00, 44.39it/s]Capturing num tokens (num_tokens=28 avail_mem=132.25 GB):  81%|████████  | 47/58 [00:01<00:00, 44.39it/s]Capturing num tokens (num_tokens=28 avail_mem=132.25 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.30it/s]Capturing num tokens (num_tokens=24 avail_mem=132.25 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.30it/s]Capturing num tokens (num_tokens=20 avail_mem=132.24 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.30it/s]Capturing num tokens (num_tokens=16 avail_mem=132.24 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.30it/s]Capturing num tokens (num_tokens=12 avail_mem=132.24 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.30it/s]Capturing num tokens (num_tokens=8 avail_mem=132.23 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.30it/s] Capturing num tokens (num_tokens=8 avail_mem=132.23 GB):  98%|█████████▊| 57/58 [00:01<00:00, 44.91it/s]Capturing num tokens (num_tokens=4 avail_mem=132.23 GB):  98%|█████████▊| 57/58 [00:01<00:00, 44.91it/s]Capturing num tokens (num_tokens=4 avail_mem=132.23 GB): 100%|██████████| 58/58 [00:01<00:00, 39.72it/s]


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
    Generated text:  Christine, a sophomore majoring in Business Administration. My parents are both teachers. I have been involved in a lot of community service, especially working with the homeless. My family is very supportive and understanding to me, and I’m a bit ambitious. I have a lot of interest in technology and business. When I am free I like to spend time reading, watching movies, and socializing with friends. What are your hobbies and interests? What are your strengths? What are your weaknesses? What is your long-term career goal? What are your top three favorite books of all time? Let's have a conversation on these topics with me.
    ===============================
    Prompt: The president of the United States is
    Generated text:  a职位的英语翻译是____。
    A. President of the United States
    B. The President of the United States
    C. the United States President
    D. United States President
    答案: A
    
    某企业2019年1月1日租入一栋商铺用于经营，支付租金10万元，该商铺在租赁期开始时即作为固定资产入账。假设该企业适用的契税税率为3%，该企业2019年应缴纳的契税为____万元。
    A. 0.3
    B. 0.4
    C. 0.6
    D
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. It is the largest city in Europe and the third largest in the world. The population of Paris is over 2.5 million. It is the second largest city in the world by population. Paris is located on the banks of the Seine River. It is known as the "City of Love" because of its romantic and picturesque streets and parks. There are many museums in the city. The most famous one is the Musée d'Orsay. It is housed in a former palace. The Louvre is the world's largest museum of art. It is in the heart of Paris. The Louvre is open 
    ===============================
    Prompt: The future of AI is
    Generated text:  real but you don't need a supercomputer to get started
    
    In the early 2000s, I started working at Microsoft, a company with an ambitious and proud history of innovation. I knew I had been a special case; many of my peers had been roboticists, scientists, and engineers; the language was simply a matter of advancing the concept of the world. But as I began to apply AI to the business side of Microsoft, I realized that there was no single “language.” To build a machine that could analyze vast amounts of data and make predictions about the market, you needed a mix of expertise from many areas


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


    Generated text:  Paris, the city known for its iconic Eiffel Tower and its rich history dating back to the Middle Ages. It is also home to the Louvre Museum, the most famous art museum in the world, and the Notre-Dame Cathedral, a stunning Gothic structure that has stood for over 800 years. Paris is a vibrant and diverse city with a rich cultural scene, and it is a popular tourist destination for many people around the world. The city is also known for its fashion industry, with Paris Fashion Week being one of the largest and most prestigious fashion events in the world. Overall, Paris is a city of contrasts and
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the development of the technology in the coming years. Here are some of the potential trends that are likely to shape the future of AI:
    
    1. Increased use of AI in healthcare: AI is already being used in healthcare to improve patient outcomes and reduce costs. As the technology continues to improve, we can expect to see even more widespread use of AI in healthcare, with more sophisticated algorithms and machine learning techniques being developed to improve diagnosis, treatment, and patient care.
    
    2. Increased use of AI in finance: AI is already being used in finance to improve fraud detection, risk
    


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
    Generated text:  [insert name] and I am a [insert occupation or profession] from [insert country]. I am currently [insert current location] and have been working in this field for [insert number of years] years.
    
    I am a [insert age] year old individual with [insert relevant personal information, such as a photo or a brief bio]. I am a [insert personal trait or characteristic, such as "energetic" or "ambitious"} who is always [insert a characteristic or trait that matches your background and experiences, such as "outgoing" or "team-oriented"]. I enjoy [insert hobbies or interests, such as [
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, a city located in the south of the country, known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also a major cultural and political center, hosting numerous museums, theaters, and opera houses, and is a popular tourist destination. The city is known for its diverse and multicultural population, and is home to many international institutions and organizations. Paris is a bustling city with a rich history and culture, and is a popular destination for tourists and scholars alike. Its importance as the capital of France makes it an integral part of French culture and identity. According to the
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be characterized by several trends that are likely to impact various industries, including healthcare, transportation, manufacturing, and education. Here are some possible future trends in AI:
    
    1. Personalized AI: One of the most promising trends in AI is the development of personalized AI that can be tailored to each individual user's needs and preferences. AI will be able to learn from user data to better understand the behavior and preferences of different users, and provide customized recommendations and solutions. This could lead to more efficient use of resources and improved customer satisfaction.
    
    2. Improved safety and security: AI is already playing a significant role in industries that require high levels


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

     name

    ],

     and

     I

    'm

     an

     [

    insert

     age

     and

     profession

    ]

     who

     works

     at

     [

    insert

     job

     title

     or

     company

     name

    ].

     I

     enjoy

     [

    insert

     something

     enjoyable

     or

     meaningful

     you

     do

    ,

     like

     solving

     puzzles

     or

     running

     in

     the

     park

    ].

     I

    'm

     here

     to

     help

     you

     [

    insert

     something

     you

     can

     assist

     with

    ,

     like

     make

     a

     reservation

    ,

     fill

     out

     a

     survey

    ,

     or

     get

     directions

    ].

     If

     you

     have

     any

     questions

     or

     need

     any

     help

    ,

     feel

     free

     to

     ask

    !

     

    🎉

    ✨

    ✨

    
    


    What

     do

     you

     do

     for

     a

     living

    ?

     What

    's

     your

     favorite

     hobby

     or

     sport

     to

     enjoy

    ?

     

    🏃

    ‍

    ♂

    ️

    💨

    🏃

    ‍

    ♂

    ️

    
    


    Hey

     there

    
    
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

    .

     It

     is

     known

     for

     its

     iconic

     landmarks

    ,

     cultural

     institutions

    ,

     and

     cuisine

    .

     The

     city

     is

     home

     to

     the

     E

    iff

    el

     Tower

    ,

     Lou

    vre

     Museum

    ,

     Notre

    -D

    ame

     Cathedral

    ,

     and

     many

     other

     world

    -ren

    owned

     attractions

    .

     Paris

     is

     also

     a

     popular

     tourist

     destination

    ,

     known

     for

     its

     cosm

    opolitan

     culture

    ,

     delicious

     food

    ,

     and

     rich

     history

    .

     The

     city

     has

     been

     a

     cultural

     hub

     for

     over

     

    2

    0

    0

    0

     years

     and

     has

     played

     a

     significant

     role

     in

     French

     history

    .

     The

     French

     government

     recognizes

     Paris

     as

     the

     capital

     of

     France

    ,

     and

     the

     city

     is

     represented

     by

     the

     President

     of

     France

    .

     French

     cuisine

     is

     a

     major

     part

     of

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     more

     diverse

    ,

     advanced

     and

     challenging

    ,

     with

     many

     unfore

    seen

     applications

     and

     interactions

     emerging

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

     Increased

     focus

     on

     ethical

     AI

    :

     As

     the

     AI

     industry

     grows

     and

     becomes

     more

     complex

    ,

     ethical

     issues

     will

     become

     more

     important

    .

     There

     will

     be

     increased

     focus

     on

     developing

     AI

     systems

     that

     are

     transparent

    ,

     accountable

    ,

     and

     can

     be

     trusted

     to

     operate

     without

     unintended

     consequences

    .
    


    2

    .

     AI

     will

     become

     more

     ubiquitous

    :

     AI

     is

     becoming

     more

     integrated

     into

     daily

     life

    ,

     from

     smartphones

     and

     computers

     to

     smart

     homes

     and

     autonomous

     vehicles

    .

     This

     will

     lead

     to

     more

     widespread

     adoption

     of

     AI

     in

     various

     fields

     such

     as

     healthcare

    ,

     finance

    ,

     and

     transportation

    .
    


    3

    



```python
llm.shutdown()
```
