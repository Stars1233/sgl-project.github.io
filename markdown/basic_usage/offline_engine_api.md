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


    2026-04-02 23:04:46.926 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:04:46] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:04:46.926 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:04:46] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:04:46.926 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:04:46] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:04:46.926 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:04:46] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:04:46.926 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:04:46] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.04it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.03it/s]


    2026-04-02 23:04:51,639 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 23:04:51] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:34,  2.71s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:34,  2.71s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<01:05,  1.18s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<01:05,  1.18s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<01:05,  1.18s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:25,  2.09it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:25,  2.09it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:25,  2.09it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:03<00:14,  3.62it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:03<00:14,  3.62it/s]Compiling num tokens (num_tokens=4608):  10%|█         | 6/58 [00:03<00:14,  3.62it/s]Compiling num tokens (num_tokens=4096):  10%|█         | 6/58 [00:03<00:14,  3.62it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:03<00:07,  6.42it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:03<00:07,  6.42it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:03<00:07,  6.42it/s]Compiling num tokens (num_tokens=3328):  16%|█▌        | 9/58 [00:03<00:07,  6.42it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:03<00:04,  9.40it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:03<00:04,  9.40it/s]Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:03<00:04,  9.40it/s]Compiling num tokens (num_tokens=2560):  21%|██        | 12/58 [00:03<00:04,  9.40it/s]Compiling num tokens (num_tokens=2304):  21%|██        | 12/58 [00:03<00:04,  9.40it/s]

    Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:03<00:02, 14.03it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:03<00:02, 14.03it/s]Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:03<00:02, 14.03it/s]Compiling num tokens (num_tokens=1536):  28%|██▊       | 16/58 [00:03<00:02, 14.03it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:02, 16.64it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:02, 16.64it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:02, 16.64it/s]

    Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:02, 16.64it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:03<00:02, 17.99it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:03<00:02, 17.99it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:03<00:02, 17.99it/s]Compiling num tokens (num_tokens=768):  38%|███▊      | 22/58 [00:03<00:02, 17.99it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 19.81it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 19.81it/s]

    Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 19.81it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 19.81it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:03<00:01, 20.93it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:03<00:01, 20.93it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:03<00:01, 20.93it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:03<00:01, 20.93it/s]

    Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:03<00:01, 21.82it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:03<00:01, 21.82it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:04<00:01, 21.82it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:04<00:01, 21.82it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:04<00:01, 22.94it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:04<00:01, 22.94it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:04<00:01, 22.94it/s]

    Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:04<00:01, 22.94it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:04<00:00, 22.54it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:04<00:00, 22.54it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:04<00:00, 22.54it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:04<00:00, 22.54it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:04<00:00, 22.97it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:04<00:00, 22.97it/s]

    Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:04<00:00, 22.97it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:04<00:00, 22.97it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:04<00:00, 24.60it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:04<00:00, 24.60it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:04<00:00, 24.60it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:04<00:00, 24.60it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:04<00:00, 25.72it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:04<00:00, 25.72it/s] 

    Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:04<00:00, 25.72it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:04<00:00, 25.72it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:04<00:00, 26.38it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:04<00:00, 26.38it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:04<00:00, 26.38it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:04<00:00, 26.38it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:04<00:00, 26.38it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:04<00:00, 30.01it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:04<00:00, 30.01it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:04<00:00, 30.01it/s]

    Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:04<00:00, 30.01it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:04<00:00, 30.01it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:04<00:00, 32.31it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:04<00:00, 32.31it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 11.86it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.80 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.80 GB):   2%|▏         | 1/58 [00:00<00:07,  7.23it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.77 GB):   2%|▏         | 1/58 [00:00<00:07,  7.23it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=118.77 GB):   3%|▎         | 2/58 [00:00<00:07,  7.88it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.77 GB):   3%|▎         | 2/58 [00:00<00:07,  7.88it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.77 GB):   5%|▌         | 3/58 [00:00<00:06,  8.56it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.77 GB):   5%|▌         | 3/58 [00:00<00:06,  8.56it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=118.77 GB):   7%|▋         | 4/58 [00:00<00:06,  8.29it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.77 GB):   7%|▋         | 4/58 [00:00<00:06,  8.29it/s]Capturing num tokens (num_tokens=5632 avail_mem=119.01 GB):   7%|▋         | 4/58 [00:00<00:06,  8.29it/s]Capturing num tokens (num_tokens=5120 avail_mem=119.01 GB):   7%|▋         | 4/58 [00:00<00:06,  8.29it/s]Capturing num tokens (num_tokens=5120 avail_mem=119.01 GB):  12%|█▏        | 7/58 [00:00<00:03, 14.10it/s]Capturing num tokens (num_tokens=4608 avail_mem=119.01 GB):  12%|█▏        | 7/58 [00:00<00:03, 14.10it/s]Capturing num tokens (num_tokens=4096 avail_mem=119.01 GB):  12%|█▏        | 7/58 [00:00<00:03, 14.10it/s]Capturing num tokens (num_tokens=3840 avail_mem=119.01 GB):  12%|█▏        | 7/58 [00:00<00:03, 14.10it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=119.00 GB):  12%|█▏        | 7/58 [00:00<00:03, 14.10it/s]Capturing num tokens (num_tokens=3584 avail_mem=119.00 GB):  19%|█▉        | 11/58 [00:00<00:02, 21.27it/s]Capturing num tokens (num_tokens=3328 avail_mem=119.00 GB):  19%|█▉        | 11/58 [00:00<00:02, 21.27it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.99 GB):  19%|█▉        | 11/58 [00:00<00:02, 21.27it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.99 GB):  19%|█▉        | 11/58 [00:00<00:02, 21.27it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.99 GB):  19%|█▉        | 11/58 [00:00<00:02, 21.27it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.99 GB):  19%|█▉        | 11/58 [00:00<00:02, 21.27it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.99 GB):  28%|██▊       | 16/58 [00:00<00:01, 28.47it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.98 GB):  28%|██▊       | 16/58 [00:00<00:01, 28.47it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.98 GB):  28%|██▊       | 16/58 [00:00<00:01, 28.47it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.98 GB):  28%|██▊       | 16/58 [00:00<00:01, 28.47it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=118.97 GB):  28%|██▊       | 16/58 [00:00<00:01, 28.47it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.95 GB):  28%|██▊       | 16/58 [00:00<00:01, 28.47it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.95 GB):  36%|███▌      | 21/58 [00:00<00:01, 33.54it/s]Capturing num tokens (num_tokens=960 avail_mem=118.96 GB):  36%|███▌      | 21/58 [00:00<00:01, 33.54it/s] Capturing num tokens (num_tokens=896 avail_mem=118.96 GB):  36%|███▌      | 21/58 [00:00<00:01, 33.54it/s]Capturing num tokens (num_tokens=832 avail_mem=118.96 GB):  36%|███▌      | 21/58 [00:00<00:01, 33.54it/s]Capturing num tokens (num_tokens=768 avail_mem=118.95 GB):  36%|███▌      | 21/58 [00:01<00:01, 33.54it/s]Capturing num tokens (num_tokens=704 avail_mem=118.95 GB):  36%|███▌      | 21/58 [00:01<00:01, 33.54it/s]Capturing num tokens (num_tokens=704 avail_mem=118.95 GB):  45%|████▍     | 26/58 [00:01<00:00, 37.14it/s]Capturing num tokens (num_tokens=640 avail_mem=118.95 GB):  45%|████▍     | 26/58 [00:01<00:00, 37.14it/s]Capturing num tokens (num_tokens=576 avail_mem=118.95 GB):  45%|████▍     | 26/58 [00:01<00:00, 37.14it/s]Capturing num tokens (num_tokens=512 avail_mem=118.94 GB):  45%|████▍     | 26/58 [00:01<00:00, 37.14it/s]

    Capturing num tokens (num_tokens=480 avail_mem=118.95 GB):  45%|████▍     | 26/58 [00:01<00:00, 37.14it/s]Capturing num tokens (num_tokens=448 avail_mem=118.95 GB):  45%|████▍     | 26/58 [00:01<00:00, 37.14it/s]Capturing num tokens (num_tokens=448 avail_mem=118.95 GB):  53%|█████▎    | 31/58 [00:01<00:00, 39.66it/s]Capturing num tokens (num_tokens=416 avail_mem=118.95 GB):  53%|█████▎    | 31/58 [00:01<00:00, 39.66it/s]Capturing num tokens (num_tokens=384 avail_mem=118.94 GB):  53%|█████▎    | 31/58 [00:01<00:00, 39.66it/s]Capturing num tokens (num_tokens=352 avail_mem=118.94 GB):  53%|█████▎    | 31/58 [00:01<00:00, 39.66it/s]Capturing num tokens (num_tokens=320 avail_mem=118.94 GB):  53%|█████▎    | 31/58 [00:01<00:00, 39.66it/s]Capturing num tokens (num_tokens=288 avail_mem=118.93 GB):  53%|█████▎    | 31/58 [00:01<00:00, 39.66it/s]Capturing num tokens (num_tokens=288 avail_mem=118.93 GB):  62%|██████▏   | 36/58 [00:01<00:00, 41.34it/s]Capturing num tokens (num_tokens=256 avail_mem=118.93 GB):  62%|██████▏   | 36/58 [00:01<00:00, 41.34it/s]Capturing num tokens (num_tokens=240 avail_mem=118.93 GB):  62%|██████▏   | 36/58 [00:01<00:00, 41.34it/s]Capturing num tokens (num_tokens=224 avail_mem=118.92 GB):  62%|██████▏   | 36/58 [00:01<00:00, 41.34it/s]

    Capturing num tokens (num_tokens=208 avail_mem=118.92 GB):  62%|██████▏   | 36/58 [00:01<00:00, 41.34it/s]Capturing num tokens (num_tokens=192 avail_mem=118.92 GB):  62%|██████▏   | 36/58 [00:01<00:00, 41.34it/s]Capturing num tokens (num_tokens=192 avail_mem=118.92 GB):  71%|███████   | 41/58 [00:01<00:00, 42.37it/s]Capturing num tokens (num_tokens=176 avail_mem=118.92 GB):  71%|███████   | 41/58 [00:01<00:00, 42.37it/s]Capturing num tokens (num_tokens=160 avail_mem=118.91 GB):  71%|███████   | 41/58 [00:01<00:00, 42.37it/s]Capturing num tokens (num_tokens=144 avail_mem=118.91 GB):  71%|███████   | 41/58 [00:01<00:00, 42.37it/s]Capturing num tokens (num_tokens=128 avail_mem=118.91 GB):  71%|███████   | 41/58 [00:01<00:00, 42.37it/s]Capturing num tokens (num_tokens=112 avail_mem=118.90 GB):  71%|███████   | 41/58 [00:01<00:00, 42.37it/s]Capturing num tokens (num_tokens=112 avail_mem=118.90 GB):  79%|███████▉  | 46/58 [00:01<00:00, 43.25it/s]Capturing num tokens (num_tokens=96 avail_mem=118.90 GB):  79%|███████▉  | 46/58 [00:01<00:00, 43.25it/s] Capturing num tokens (num_tokens=80 avail_mem=118.90 GB):  79%|███████▉  | 46/58 [00:01<00:00, 43.25it/s]

    Capturing num tokens (num_tokens=64 avail_mem=118.89 GB):  79%|███████▉  | 46/58 [00:01<00:00, 43.25it/s]Capturing num tokens (num_tokens=48 avail_mem=118.89 GB):  79%|███████▉  | 46/58 [00:01<00:00, 43.25it/s]Capturing num tokens (num_tokens=32 avail_mem=118.88 GB):  79%|███████▉  | 46/58 [00:01<00:00, 43.25it/s]Capturing num tokens (num_tokens=32 avail_mem=118.88 GB):  88%|████████▊ | 51/58 [00:01<00:00, 43.68it/s]Capturing num tokens (num_tokens=28 avail_mem=118.88 GB):  88%|████████▊ | 51/58 [00:01<00:00, 43.68it/s]Capturing num tokens (num_tokens=24 avail_mem=118.88 GB):  88%|████████▊ | 51/58 [00:01<00:00, 43.68it/s]Capturing num tokens (num_tokens=20 avail_mem=118.87 GB):  88%|████████▊ | 51/58 [00:01<00:00, 43.68it/s]Capturing num tokens (num_tokens=16 avail_mem=118.87 GB):  88%|████████▊ | 51/58 [00:01<00:00, 43.68it/s]Capturing num tokens (num_tokens=12 avail_mem=118.87 GB):  88%|████████▊ | 51/58 [00:01<00:00, 43.68it/s]Capturing num tokens (num_tokens=12 avail_mem=118.87 GB):  97%|█████████▋| 56/58 [00:01<00:00, 44.55it/s]Capturing num tokens (num_tokens=8 avail_mem=118.87 GB):  97%|█████████▋| 56/58 [00:01<00:00, 44.55it/s] Capturing num tokens (num_tokens=4 avail_mem=118.86 GB):  97%|█████████▋| 56/58 [00:01<00:00, 44.55it/s]

    Capturing num tokens (num_tokens=4 avail_mem=118.86 GB): 100%|██████████| 58/58 [00:01<00:00, 32.90it/s]


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
    Generated text:  Ellen. I have two cats named Charlie and Daisy. I have a big house in Florida. I have four dogs named Max, Bella, Kiki, and Liam. I have a big backyard in Florida with lots of tall grass and trees. My house has a yard in the middle.
    Write the brief summary in Turkish. Ellen, büyük bir hağna ziyaret eden bir kişi. Her ikisi olan Charlie ve Daisy, büyük bir kahvesi için yok edilmiş. Büyük bir ağacın arkasını temsil eden Max, Bella, Kiki ve Liam, büyük bir bahçesina yakın.
    ===============================
    Prompt: The president of the United States is
    Generated text:  a very important person. He has a very important job. He is very important. He is the head of the government. He is the president of the United States. President Obama is the president of the United States. He is very important. He is the president of the United States. He is the president of the United States. He is the president of the United States. He is the president of the United States. He is the president of the United States. He is the president of the United States. He is the president of the United States. He is the president of the United States. He is the president of the United
    ===============================
    Prompt: The capital of France is
    Generated text: :
    
    A. Paris
    
    B. London
    
    C. Rome
    
    D. Moscow
    
    E. Berlin
    To determine the capital of France, we need to recall the correct answer choices. Let's analyze each option:
    
    A. Paris - This is the capital of France.
    B. London - This is the capital of England.
    C. Rome - This is the capital of Italy.
    D. Moscow - This is the capital of Russia.
    E. Berlin - This is the capital of Germany.
    
    Since the question asks for the capital of France, the correct answer is:
    
    A. Paris
    
    Therefore, the answer is \boxed{A}.
    ===============================
    Prompt: The future of AI is
    Generated text:  in the process of developing and improving. Which of the following is not a key component of this development?
    A. Machine Learning
    B. Natural Language Processing
    C. Deep Learning
    D. Cybersecurity
    Answer:
    
    D
    
    In Excel, if the cell contains a formula, then after performing cell copy and paste operations, the cell will ____.
    A. Not change
    B. The formula will change
    C. Change based on the current content of the cell
    D. Change based on the content of the worksheet
    Answer:
    
    B
    
    Which of the following is NOT a characteristic of a good friendship?
    A. Harmony
    B.


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm a [job title] at [company name], and I'm excited to meet you. I'm a [job title] at [company name], and I'm excited to meet you. I'm a [job title] at [company name], and I'm excited to meet you. I'm a [job title] at [company name], and I'm excited to meet you. I'm a [job title] at [company name], and I'm excited to meet you. I'm a [job title] at [company name],
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also a major cultural and economic center, hosting numerous museums, theaters, and other attractions. Paris is a popular tourist destination and a major hub for international business and diplomacy. The city is known for its rich history, diverse culture, and vibrant nightlife. It is also home to the French Parliament, the French Academy of Sciences, and the French National Library. Paris is a city of contrasts, with its modern architecture and historical landmarks blending seamlessly into one another. The city is known for its cuisine
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends:
    
    1. Increased integration with human intelligence: As AI becomes more advanced, it is likely to become more integrated with human intelligence, allowing it to learn and adapt to new situations and tasks. This integration could lead to more sophisticated and adaptive AI systems that can perform tasks that are currently beyond the capabilities of human intelligence.
    
    2. Greater emphasis on ethical and social considerations: As AI becomes more integrated with human intelligence, there will be a greater emphasis on ethical and social considerations. This could lead
    


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
    Generated text:  [Name]. I am a friendly and knowledgeable individual who enjoys learning about new things. I have a unique way of approaching problems and challenges, and I'm always willing to help those in need. I'm passionate about the arts, especially music, and I enjoy exploring new genres and styles. My hobbies include reading, painting, and playing sports, and I'm always up for a good challenge. I'm friendly, helpful, and always looking for opportunities to learn and grow. I'm excited to meet you and talk about all the great things that you have going on in your life! [Name] [Age] [Interests] [
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, also known as "La Revue" and officially "la ville la plus grande du monde."
    
    What is the name of the capital city of the United Kingdom? The capital of the United Kingdom is London.
    
    What is the capital city of Australia? Australia's capital is Canberra.
    
    What is the capital city of Argentina? Buenos Aires is the capital city of Argentina.
    
    What is the capital city of France? Paris is the capital city of France.
    
    What is the capital city of Brazil? Brasilia is the capital city of Brazil. 
    
    Which country is both an island and a country? The country that is both an island and a country
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be characterized by a proliferation of highly specialized AI systems that are capable of performing tasks that were once beyond the capabilities of humans. This could include, for example, speech recognition, image recognition, and natural language processing. As these tasks become more complex, they may become more similar to those that are traditionally handled by humans. However, it is also possible that some of these specialized AI systems may become more capable and widespread, and that the impact of AI on society could be both positive and negative. For example, AI could be used to improve medical treatments, to create more efficient manufacturing processes, and to develop new technologies that are both


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

    ].

     I

    'm

     a

     [

    age

    ]

     year

     old

     [

    gender

    ]

     [

    occupation

    ],

     and

     I

    'm

     currently

     [

    job

     title

     or

     profession

    ].

     I

     spend

     my

     days

     [

    main

     job

     or

     hobby

    ],

     and

     I

     have

     a

     love

     for

     [

    something

     or

     someone

     that

     interests

     you

    ].

     I

     hope

     you

    'll

     take

     the

     time

     to

     meet

     me

     in

     person

    .

     What

    's

     your

     job

     or

     profession

    ?

     [

    Tell

     me

     about

     your

     job

     or

     profession

    ,

     including

     any

     relevant

     experiences

     or

     achievements

    ].

     


    I

    'm

     excited

     to

     meet

     you

    .

     How

     do

     you

     like

     to

     spend

     your

     time

     outside

     of

     work

    ?

     [

    List

     any

     hobbies

     or

     interests

     you

     have

    ,

     including

     anything

     unusual

    ].

     I

     love

     to

     spend

     time

     with

     my

     family

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     also

     known

     as

     Lou

    vre

    .

     It

     is

     a

     historic

     city

     with

     a

     rich

     cultural

     heritage

    ,

     including

     the

     iconic

     E

    iff

    el

     Tower

    ,

     where

     the

     

    1

    9

    0

    0

     World

    's

     Fair

     was

     held

    .

     It

     is

     also

     a

     major

     financial

     and

     economic

     center

    ,

     and

     has

     a

     diverse

     population

     of

     

    1

    0

     million

     people

    .

     Paris

     is

     known

     for

     its

     iconic

     landmarks

     such

     as

     the

     Lou

    vre

    ,

     the

     Notre

    -D

    ame

     Cathedral

    ,

     and

     the

     Mont

    mart

    re

     neighborhood

    .

     It

     is

     also

     home

     to

     many

     art

     museums

    ,

     including

     the

     Centre

     Pom

    pid

    ou

     and

     the

     Lou

    vre

     Museum

    .

     The

     city

     has

     a

     reputation

     for

     its

     romantic

     and

     artistic

     atmosphere

    ,

     and

     is

     a

     popular

     tourist

     destination

     for

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     very

     exciting

     and

     promising

    .

     Here

     are

     some

     possible

     trends

     to

     expect

    :
    


    1

    .

     Increased

     integration

    :

     As

     AI

     becomes

     more

     integrated

     into

     various

     sectors

    ,

     it

     will

     become

     more

     accessible

     to

     a

     wider

     range

     of

     people

    .

     This

     will

     enable

     more

     complex

     AI

     solutions

     to

     be

     developed

     and

     deployed

    .
    


    2

    .

     Improved

     data

     privacy

    :

     With

     the

     increasing

     amount

     of

     personal

     data

     being

     collected

     and

     shared

     online

    ,

     there

     is

     a

     growing

     need

     for

     improved

     data

     privacy

     laws

     and

     regulations

     to

     protect

     individuals

    '

     privacy

    .
    


    3

    .

     Greater

     use

     of

     AI

     in

     healthcare

    :

     AI

     will

     play

     an

     increasingly

     important

     role

     in

     healthcare

    ,

     from

     diagn

    osing

     diseases

     to

     developing

     personalized

     treatment

     plans

    .

     This

     will

     require

     the

     development

     of

     new

     AI

     technologies

     that

    



```python
llm.shutdown()
```
