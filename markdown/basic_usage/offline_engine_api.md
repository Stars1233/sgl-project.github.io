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


    2026-04-02 06:10:26.617 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:10:26] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 06:10:26.617 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:10:26] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 06:10:26.617 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:10:26] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 06:10:26.617 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:10:26] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 06:10:26.617 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 06:10:26] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.23it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.23it/s]


    2026-04-02 06:10:31,047 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 06:10:31] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:34,  2.72s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:34,  2.72s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:34,  2.72s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:02<00:41,  1.31it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:02<00:41,  1.31it/s]

    Compiling num tokens (num_tokens=6144):   5%|▌         | 3/58 [00:02<00:41,  1.31it/s]Compiling num tokens (num_tokens=5632):   5%|▌         | 3/58 [00:02<00:41,  1.31it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:03<00:16,  3.10it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:03<00:16,  3.10it/s]Compiling num tokens (num_tokens=4608):  10%|█         | 6/58 [00:03<00:16,  3.10it/s]Compiling num tokens (num_tokens=4096):  10%|█         | 6/58 [00:03<00:16,  3.10it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:03<00:09,  5.31it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:03<00:09,  5.31it/s]

    Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:03<00:09,  5.31it/s]Compiling num tokens (num_tokens=3328):  16%|█▌        | 9/58 [00:03<00:09,  5.31it/s]Compiling num tokens (num_tokens=3072):  16%|█▌        | 9/58 [00:03<00:09,  5.31it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:03<00:05,  8.82it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:03<00:05,  8.82it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:03<00:05,  8.82it/s]Compiling num tokens (num_tokens=2304):  22%|██▏       | 13/58 [00:03<00:05,  8.82it/s]Compiling num tokens (num_tokens=2048):  22%|██▏       | 13/58 [00:03<00:05,  8.82it/s]Compiling num tokens (num_tokens=1792):  22%|██▏       | 13/58 [00:03<00:05,  8.82it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:02, 13.93it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:02, 13.93it/s]

    Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:02, 13.93it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:02, 13.93it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:02, 13.93it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:03<00:02, 13.93it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:03<00:02, 13.93it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:03<00:01, 20.68it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:03<00:01, 20.68it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:03<00:01, 20.68it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:03<00:01, 20.68it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:03<00:01, 20.68it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:03<00:01, 20.68it/s]Compiling num tokens (num_tokens=480):  41%|████▏     | 24/58 [00:03<00:01, 20.68it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:03<00:01, 27.44it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:03<00:01, 27.44it/s]

    Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:03<00:01, 27.44it/s]Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:03<00:01, 27.44it/s]Compiling num tokens (num_tokens=352):  52%|█████▏    | 30/58 [00:03<00:01, 27.44it/s]Compiling num tokens (num_tokens=320):  52%|█████▏    | 30/58 [00:03<00:01, 27.44it/s]Compiling num tokens (num_tokens=288):  52%|█████▏    | 30/58 [00:03<00:01, 27.44it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:03<00:00, 32.86it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:03<00:00, 32.86it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:03<00:00, 32.86it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:03<00:00, 32.86it/s]Compiling num tokens (num_tokens=208):  62%|██████▏   | 36/58 [00:03<00:00, 32.86it/s]Compiling num tokens (num_tokens=192):  62%|██████▏   | 36/58 [00:03<00:00, 32.86it/s]

    Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:03<00:00, 35.60it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:03<00:00, 35.60it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:03<00:00, 35.60it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:03<00:00, 35.60it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:03<00:00, 35.60it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:03<00:00, 35.60it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:03<00:00, 38.09it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:03<00:00, 38.09it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:03<00:00, 38.09it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:03<00:00, 38.09it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:03<00:00, 38.09it/s]Compiling num tokens (num_tokens=32):  79%|███████▉  | 46/58 [00:03<00:00, 38.09it/s]

    Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:04<00:00, 40.40it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:04<00:00, 40.40it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:04<00:00, 40.40it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:04<00:00, 40.40it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:04<00:00, 40.40it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:04<00:00, 40.40it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:04<00:00, 40.40it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:04<00:00, 40.40it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 47.51it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 14.12it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.53 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.50 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.50 GB):   3%|▎         | 2/58 [00:00<00:02, 18.93it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.49 GB):   3%|▎         | 2/58 [00:00<00:02, 18.93it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.49 GB):   3%|▎         | 2/58 [00:00<00:02, 18.93it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.49 GB):   3%|▎         | 2/58 [00:00<00:02, 18.93it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=118.49 GB):   9%|▊         | 5/58 [00:00<00:02, 22.08it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.49 GB):   9%|▊         | 5/58 [00:00<00:02, 22.08it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.49 GB):   9%|▊         | 5/58 [00:00<00:02, 22.08it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.49 GB):   9%|▊         | 5/58 [00:00<00:02, 22.08it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.49 GB):   9%|▊         | 5/58 [00:00<00:02, 22.08it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.49 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.25it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.48 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.25it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.48 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.25it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=118.47 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.25it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.47 GB):  21%|██        | 12/58 [00:00<00:01, 23.40it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.47 GB):  21%|██        | 12/58 [00:00<00:01, 23.40it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.47 GB):  21%|██        | 12/58 [00:00<00:01, 23.40it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.46 GB):  21%|██        | 12/58 [00:00<00:01, 23.40it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.46 GB):  26%|██▌       | 15/58 [00:00<00:01, 23.69it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.46 GB):  26%|██▌       | 15/58 [00:00<00:01, 23.69it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=118.46 GB):  26%|██▌       | 15/58 [00:00<00:01, 23.69it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.45 GB):  26%|██▌       | 15/58 [00:00<00:01, 23.69it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.45 GB):  31%|███       | 18/58 [00:00<00:01, 24.91it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.45 GB):  31%|███       | 18/58 [00:00<00:01, 24.91it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.45 GB):  31%|███       | 18/58 [00:00<00:01, 24.91it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.43 GB):  31%|███       | 18/58 [00:00<00:01, 24.91it/s]Capturing num tokens (num_tokens=960 avail_mem=118.44 GB):  31%|███       | 18/58 [00:00<00:01, 24.91it/s] Capturing num tokens (num_tokens=960 avail_mem=118.44 GB):  38%|███▊      | 22/58 [00:00<00:01, 27.23it/s]Capturing num tokens (num_tokens=896 avail_mem=118.44 GB):  38%|███▊      | 22/58 [00:00<00:01, 27.23it/s]

    Capturing num tokens (num_tokens=832 avail_mem=118.44 GB):  38%|███▊      | 22/58 [00:00<00:01, 27.23it/s]Capturing num tokens (num_tokens=768 avail_mem=118.43 GB):  38%|███▊      | 22/58 [00:00<00:01, 27.23it/s]Capturing num tokens (num_tokens=704 avail_mem=118.43 GB):  38%|███▊      | 22/58 [00:00<00:01, 27.23it/s]Capturing num tokens (num_tokens=704 avail_mem=118.43 GB):  45%|████▍     | 26/58 [00:00<00:01, 29.12it/s]Capturing num tokens (num_tokens=640 avail_mem=118.43 GB):  45%|████▍     | 26/58 [00:00<00:01, 29.12it/s]Capturing num tokens (num_tokens=576 avail_mem=118.43 GB):  45%|████▍     | 26/58 [00:01<00:01, 29.12it/s]Capturing num tokens (num_tokens=512 avail_mem=118.41 GB):  45%|████▍     | 26/58 [00:01<00:01, 29.12it/s]Capturing num tokens (num_tokens=480 avail_mem=118.43 GB):  45%|████▍     | 26/58 [00:01<00:01, 29.12it/s]

    Capturing num tokens (num_tokens=480 avail_mem=118.43 GB):  52%|█████▏    | 30/58 [00:01<00:00, 29.79it/s]Capturing num tokens (num_tokens=448 avail_mem=118.43 GB):  52%|█████▏    | 30/58 [00:01<00:00, 29.79it/s]Capturing num tokens (num_tokens=416 avail_mem=118.43 GB):  52%|█████▏    | 30/58 [00:01<00:00, 29.79it/s]Capturing num tokens (num_tokens=384 avail_mem=118.42 GB):  52%|█████▏    | 30/58 [00:01<00:00, 29.79it/s]Capturing num tokens (num_tokens=384 avail_mem=118.42 GB):  57%|█████▋    | 33/58 [00:01<00:01, 24.67it/s]Capturing num tokens (num_tokens=352 avail_mem=118.40 GB):  57%|█████▋    | 33/58 [00:01<00:01, 24.67it/s]

    Capturing num tokens (num_tokens=320 avail_mem=118.40 GB):  57%|█████▋    | 33/58 [00:01<00:01, 24.67it/s]Capturing num tokens (num_tokens=288 avail_mem=118.23 GB):  57%|█████▋    | 33/58 [00:01<00:01, 24.67it/s]

    Capturing num tokens (num_tokens=288 avail_mem=118.23 GB):  62%|██████▏   | 36/58 [00:01<00:01, 19.44it/s]Capturing num tokens (num_tokens=256 avail_mem=118.23 GB):  62%|██████▏   | 36/58 [00:01<00:01, 19.44it/s]Capturing num tokens (num_tokens=240 avail_mem=117.89 GB):  62%|██████▏   | 36/58 [00:01<00:01, 19.44it/s]Capturing num tokens (num_tokens=224 avail_mem=117.75 GB):  62%|██████▏   | 36/58 [00:01<00:01, 19.44it/s]

    Capturing num tokens (num_tokens=224 avail_mem=117.75 GB):  67%|██████▋   | 39/58 [00:01<00:01, 15.59it/s]Capturing num tokens (num_tokens=208 avail_mem=117.57 GB):  67%|██████▋   | 39/58 [00:01<00:01, 15.59it/s]Capturing num tokens (num_tokens=192 avail_mem=117.55 GB):  67%|██████▋   | 39/58 [00:01<00:01, 15.59it/s]Capturing num tokens (num_tokens=192 avail_mem=117.55 GB):  71%|███████   | 41/58 [00:01<00:01, 16.10it/s]Capturing num tokens (num_tokens=176 avail_mem=117.07 GB):  71%|███████   | 41/58 [00:01<00:01, 16.10it/s]Capturing num tokens (num_tokens=160 avail_mem=116.98 GB):  71%|███████   | 41/58 [00:01<00:01, 16.10it/s]Capturing num tokens (num_tokens=144 avail_mem=116.91 GB):  71%|███████   | 41/58 [00:02<00:01, 16.10it/s]Capturing num tokens (num_tokens=128 avail_mem=116.90 GB):  71%|███████   | 41/58 [00:02<00:01, 16.10it/s]

    Capturing num tokens (num_tokens=128 avail_mem=116.90 GB):  78%|███████▊  | 45/58 [00:02<00:00, 19.95it/s]Capturing num tokens (num_tokens=112 avail_mem=116.90 GB):  78%|███████▊  | 45/58 [00:02<00:00, 19.95it/s]Capturing num tokens (num_tokens=96 avail_mem=116.90 GB):  78%|███████▊  | 45/58 [00:02<00:00, 19.95it/s] Capturing num tokens (num_tokens=80 avail_mem=116.89 GB):  78%|███████▊  | 45/58 [00:02<00:00, 19.95it/s]Capturing num tokens (num_tokens=64 avail_mem=116.89 GB):  78%|███████▊  | 45/58 [00:02<00:00, 19.95it/s]Capturing num tokens (num_tokens=64 avail_mem=116.89 GB):  84%|████████▍ | 49/58 [00:02<00:00, 22.97it/s]Capturing num tokens (num_tokens=48 avail_mem=116.89 GB):  84%|████████▍ | 49/58 [00:02<00:00, 22.97it/s]Capturing num tokens (num_tokens=32 avail_mem=116.88 GB):  84%|████████▍ | 49/58 [00:02<00:00, 22.97it/s]Capturing num tokens (num_tokens=28 avail_mem=116.88 GB):  84%|████████▍ | 49/58 [00:02<00:00, 22.97it/s]

    Capturing num tokens (num_tokens=24 avail_mem=116.88 GB):  84%|████████▍ | 49/58 [00:02<00:00, 22.97it/s]Capturing num tokens (num_tokens=24 avail_mem=116.88 GB):  91%|█████████▏| 53/58 [00:02<00:00, 25.75it/s]Capturing num tokens (num_tokens=20 avail_mem=116.87 GB):  91%|█████████▏| 53/58 [00:02<00:00, 25.75it/s]Capturing num tokens (num_tokens=16 avail_mem=116.87 GB):  91%|█████████▏| 53/58 [00:02<00:00, 25.75it/s]Capturing num tokens (num_tokens=12 avail_mem=116.87 GB):  91%|█████████▏| 53/58 [00:02<00:00, 25.75it/s]Capturing num tokens (num_tokens=8 avail_mem=116.86 GB):  91%|█████████▏| 53/58 [00:02<00:00, 25.75it/s] Capturing num tokens (num_tokens=8 avail_mem=116.86 GB):  98%|█████████▊| 57/58 [00:02<00:00, 28.89it/s]Capturing num tokens (num_tokens=4 avail_mem=116.86 GB):  98%|█████████▊| 57/58 [00:02<00:00, 28.89it/s]Capturing num tokens (num_tokens=4 avail_mem=116.86 GB): 100%|██████████| 58/58 [00:02<00:00, 23.84it/s]


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
    Generated text:  Alan. I'm 13 years old and I'm a student. I love to play games and go to the movies. I have a pet rabbit named Shazam. But I don't have any friends, so I don't go to any clubs or play with any other people. I like music too. I'm good at playing the guitar and I love listening to music. I love to read books. I like movies too. I have a big collection of books. I love to read to my pet rabbit. I also like to play games with my pet rabbit. What kind of animals does Alan have? A) Cats
    ===============================
    Prompt: The president of the United States is
    Generated text:  currently 35 years older than the president of Texas. If the president of Texas is currently 32 years old, what is the sum of their ages?
    To determine the sum of the ages of the president of the United States and the president of Texas, we need to follow these steps:
    
    1. Identify the current age of the president of Texas.
    2. Determine the current age of the president of the United States based on the given information.
    3. Calculate the sum of their ages.
    
    First, we know that the president of Texas is currently 32 years old. According to the problem, the president of the United States
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. It's a city with a rich history and cultural heritage. In fact, the city has a history dating back to the time of the Romans, the French kings, the Habsburgs, and the Venetians. The city is home to many interesting landmarks and attractions, and visitors can easily explore the many museums, theaters, and historical sites that Paris has to offer.
    
    But how do you actually go to Paris? For many people, the best way to get to the city is by car. But there are other options for getting around Paris, including walking, cycling, and public transportation. Each of these methods has its
    ===============================
    Prompt: The future of AI is
    Generated text:  shifting rapidly, and for many of the industry's leading players, the future of AI is now.
    
    The question of what AI is, what it will be, and what it should do are hotly debated. The debate has been going on for decades, and the disagreement is quite interesting.
    
    In fact, the debate is so intense that it has led to the development of the two major AI fields, which are Deep Learning and Natural Language Processing.
    
    It is my belief that the debate is related to the fact that deep learning is more related to the past and natural language processing is more related to the future. That is why the two fields are


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


    Generated text:  [Name] and I'm a [Age] year old [Occupation]. I'm a [Skill] who enjoys [Favorite Activity]. I'm passionate about [Why I'm Passionate]. I'm always looking for new experiences and learning new things. I'm a [Favorite Hobby] who enjoys [Why I like it]. I'm a [Favorite Book] who reads [Number of Books] books. I'm a [Favorite Movie] who loves [Why I like it]. I'm a [Favorite Music] who listens to [Number of Songs] songs. I'm a [Favorite Sport] who plays [Number of Games]
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. It is also home to the French Parliament, the French Academy of Sciences, and the French National Library. Paris is a bustling city with a rich cultural heritage and is a popular tourist destination. It is also known for its cuisine, including its famous croissants and its famous French fries. The city is also home to many international organizations and institutions, including the European Parliament and the United Nations. Paris is a city of contrasts, with its modern architecture and historical landmarks blending seamlessly into one another. It is a city
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn from and adapt to human behavior and decision-making processes. This could lead to more sophisticated and personalized AI systems that can better understand and respond to human emotions and preferences.
    
    2. Enhanced machine learning capabilities: AI is likely to become more powerful and capable of learning from large amounts of data, which could lead to even more sophisticated and complex machine learning systems. This could result in more accurate and reliable AI systems that can perform a wider range of tasks.
    
    3. Increased focus on ethical AI
    


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
    Generated text:  Sarah, and I'm a busy writer. I love working on stories, experimenting with different writing styles, and traveling the world. I enjoy writing characters and their lives, and I'm always learning and growing as a writer. I'm looking forward to getting started on my first novel soon! 🕉📚🚀😊
    
    Wow, that sounds like a fascinating world of creativity and exploration! Can you tell me more about your writing style? I'm always interested to learn about different writing styles, so I'm curious about how you work and what draws you to certain writing techniques. 🤔📝✨
    
    Sarah, thank you for sharing
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    
    Paris is the largest city in France, located in the northwestern region of the country. It serves as the cultural and economic center of France and is known for its historic landmarks, museums, and food scene. The city has a rich history dating back to the Roman Empire and is famous for its 19th-century art deco architecture, including the Eiffel Tower. Paris is also a popular tourist destination and has been recognized as a UNESCO World Heritage site. The city is known for its romantic atmosphere, world-renowned fashion, and annual Carnaval celebration. It is often referred to as "Le Seigneur de Ne
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  incredibly promising and can be quite exciting. Here are some potential trends that are likely to shape the landscape of AI in the coming years:
    
    1. Increased AI transparency and explainability: As AI systems become more integrated into our daily lives, there will be a greater emphasis on ensuring that AI systems are transparent and explainable. This means that developers will strive to create AI systems that are designed to be understandable, understandable even to human users. This will require a greater investment in research and development in areas like natural language processing, computer vision, and natural language generation.
    
    2. Increased reliance on AI for decision-making: As AI becomes more integrated into


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

     [

    Your

     Occupation

    ]

     in

     [

    Your

     Location

    ].

     I

    'm

     really

     passionate

     about

     [

    What

     you

     do

     really

     enjoy

     doing

    ]

     and

     I

    'm

     always

     looking

     for

     new

     challenges

    .

     I

     love

     exploring

     new

     things

     and

     having

     a

     lot

     of

     fun

     with

     my

     friends

    .

     I

    'm

     really

     excited

     to

     start

     my

     day

     with

     [

    What

     you

     do

     really

     enjoy

     for

     your

     morning

    ?

    ].

     I

    'm

     always

     looking

     for

     new

     ways

     to

     help

     people

    ,

     and

     I

     believe

     that

     helping

     others

     is

     the

     best

     way

     to

     make

     the

     world

     a

     better

     place

    .

     I

     love

     sharing

     my

     knowledge

     and

     experiences

     with

     others

    ,

     and

     I

    'm

     always

     eager

     to

     learn

     new

     things

    .

     I

    'm

     really

     looking

     forward

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .
    


    Please

     answer

     the

     following

     question

    :
    


    Which

     country

     is

     the

     capital

     of

     France

    ?

     France

    
    


    Does

     this

     imply

     that

     French

     citizens

     of

     the

     same

     nationality

     can

     enter

     into

     marriages

     with

     citizens

     of

     other

     countries

    ?

     The

     fact

     that

     France

     is

     a

     constitutional

     monarchy

     is

     not

     evidence

     that

     French

     citizens

     of

     the

     same

     nationality

     can

     enter

     into

     marriages

     with

     citizens

     of

     other

     countries

    .

     In

     fact

    ,

     the

     law

     in

     France

     guarantees

     the

     right

     of

     citizens

     of

     other

     countries

     to

     marry

     in

     the

     country

     of

     their

     choice

    .

     It

     is

     a

     highly

     sensitive

     and

     complex

     issue

     that

     involves

     a

     wide

     range

     of

     factors

    ,

     including

     family

     traditions

    ,

     religious

     beliefs

    ,

     and

     personal

     and

     cultural

     values

    .

     Some

     French

     citizens

     of

     other

     countries

     may

     have

     difficulty

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     involve

     a

     number

     of

     trends

     and

     developments

     that

     will

     shape

     how

     we

     interact

     with

     technology

     and

     how

     it

     is

     used

     in

     various

     sectors

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

     transparency

     and

     accountability

    :

     As

     AI

     systems

     become

     more

     complex

     and

     sophisticated

    ,

     there

     is

     a

     greater

     need

     for

     transparency

     and

     accountability

     in

     how

     they

     are

     developed

    ,

     deployed

    ,

     and

     used

    .

     This

     means

     that

     we

     will

     see

     increased

     efforts

     to

     ensure

     that

     AI

     systems

     are

     designed

     and

     deployed

     in

     a

     responsible

     and

     ethical

     manner

    .
    


    2

    .

     AI

     will

     become

     more

     integrated

     with

     human

     intelligence

    :

     With

     the

     increasing

     reliance

     on

     AI

     for

     tasks

     that

     are

     traditionally

     human

    -driven

    ,

     it

     is

     likely

     that

     AI

     will

    



```python
llm.shutdown()
```
