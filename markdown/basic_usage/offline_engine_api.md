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


    2026-03-30 08:23:02.522 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:23:02] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 08:23:02.523 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:23:02] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 08:23:02.523 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:23:02] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 08:23:02.523 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:23:02] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 08:23:02.523 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:23:02] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  2.37it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  2.37it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:11,  2.31s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:11,  2.31s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<00:57,  1.02s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<00:57,  1.02s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<00:57,  1.02s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:22,  2.37it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:22,  2.37it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:22,  2.37it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:22,  2.37it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:22,  2.37it/s]Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:02<00:08,  5.85it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:02<00:08,  5.85it/s]Compiling num tokens (num_tokens=3840):  14%|█▍        | 8/58 [00:02<00:08,  5.85it/s]Compiling num tokens (num_tokens=3584):  14%|█▍        | 8/58 [00:02<00:08,  5.85it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:05,  8.63it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:05,  8.63it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:05,  8.63it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:05,  8.63it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:02<00:03, 11.63it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:02<00:03, 11.63it/s]Compiling num tokens (num_tokens=2304):  24%|██▍       | 14/58 [00:02<00:03, 11.63it/s]Compiling num tokens (num_tokens=2048):  24%|██▍       | 14/58 [00:02<00:03, 11.63it/s]Compiling num tokens (num_tokens=1792):  24%|██▍       | 14/58 [00:02<00:03, 11.63it/s]

    Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:02, 15.88it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:02, 15.88it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:02, 15.88it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:02, 15.88it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:02, 15.88it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:03<00:01, 19.77it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:03<00:01, 19.77it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:03<00:01, 19.77it/s]Compiling num tokens (num_tokens=768):  38%|███▊      | 22/58 [00:03<00:01, 19.77it/s]

    Compiling num tokens (num_tokens=704):  38%|███▊      | 22/58 [00:03<00:01, 19.77it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 23.23it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 23.23it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 23.23it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 23.23it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 23.23it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:03<00:01, 25.59it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:03<00:01, 25.59it/s]Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:03<00:01, 25.59it/s]

    Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:03<00:01, 25.59it/s]Compiling num tokens (num_tokens=352):  52%|█████▏    | 30/58 [00:03<00:01, 25.59it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:03<00:00, 28.67it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:03<00:00, 28.67it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:03<00:00, 28.67it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:03<00:00, 28.67it/s]Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:03<00:00, 28.67it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 29.68it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 29.68it/s]

    Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 29.68it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 29.68it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 29.68it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:03<00:00, 31.68it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:03<00:00, 31.68it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:03<00:00, 31.68it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:03<00:00, 31.68it/s]Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:03<00:00, 31.68it/s]

    Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:03<00:00, 31.83it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:03<00:00, 31.83it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:03<00:00, 31.83it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:03<00:00, 31.83it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:03<00:00, 31.83it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 32.60it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 32.60it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 32.60it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:04<00:00, 32.60it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:04<00:00, 32.60it/s]

    Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:04<00:00, 32.60it/s]Compiling num tokens (num_tokens=16):  95%|█████████▍| 55/58 [00:04<00:00, 35.07it/s]Compiling num tokens (num_tokens=12):  95%|█████████▍| 55/58 [00:04<00:00, 35.07it/s]Compiling num tokens (num_tokens=8):  95%|█████████▍| 55/58 [00:04<00:00, 35.07it/s] Compiling num tokens (num_tokens=4):  95%|█████████▍| 55/58 [00:04<00:00, 35.07it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 14.05it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=48.89 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=48.89 GB):   2%|▏         | 1/58 [00:00<00:08,  7.05it/s]Capturing num tokens (num_tokens=7680 avail_mem=48.81 GB):   2%|▏         | 1/58 [00:00<00:08,  7.05it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=48.81 GB):   3%|▎         | 2/58 [00:00<00:07,  7.16it/s]Capturing num tokens (num_tokens=7168 avail_mem=48.80 GB):   3%|▎         | 2/58 [00:00<00:07,  7.16it/s]Capturing num tokens (num_tokens=7168 avail_mem=48.80 GB):   5%|▌         | 3/58 [00:00<00:07,  7.26it/s]Capturing num tokens (num_tokens=6656 avail_mem=48.80 GB):   5%|▌         | 3/58 [00:00<00:07,  7.26it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=48.80 GB):   7%|▋         | 4/58 [00:00<00:06,  7.96it/s]Capturing num tokens (num_tokens=6144 avail_mem=48.75 GB):   7%|▋         | 4/58 [00:00<00:06,  7.96it/s]Capturing num tokens (num_tokens=5632 avail_mem=48.75 GB):   7%|▋         | 4/58 [00:00<00:06,  7.96it/s]Capturing num tokens (num_tokens=5120 avail_mem=48.75 GB):   7%|▋         | 4/58 [00:00<00:06,  7.96it/s]Capturing num tokens (num_tokens=5120 avail_mem=48.75 GB):  12%|█▏        | 7/58 [00:00<00:03, 14.64it/s]Capturing num tokens (num_tokens=4608 avail_mem=48.74 GB):  12%|█▏        | 7/58 [00:00<00:03, 14.64it/s]Capturing num tokens (num_tokens=4096 avail_mem=48.74 GB):  12%|█▏        | 7/58 [00:00<00:03, 14.64it/s]Capturing num tokens (num_tokens=3840 avail_mem=48.74 GB):  12%|█▏        | 7/58 [00:00<00:03, 14.64it/s]Capturing num tokens (num_tokens=3584 avail_mem=48.73 GB):  12%|█▏        | 7/58 [00:00<00:03, 14.64it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=48.73 GB):  19%|█▉        | 11/58 [00:00<00:02, 22.10it/s]Capturing num tokens (num_tokens=3328 avail_mem=48.73 GB):  19%|█▉        | 11/58 [00:00<00:02, 22.10it/s]Capturing num tokens (num_tokens=3072 avail_mem=48.73 GB):  19%|█▉        | 11/58 [00:00<00:02, 22.10it/s]Capturing num tokens (num_tokens=2816 avail_mem=48.73 GB):  19%|█▉        | 11/58 [00:00<00:02, 22.10it/s]Capturing num tokens (num_tokens=2560 avail_mem=48.72 GB):  19%|█▉        | 11/58 [00:00<00:02, 22.10it/s]Capturing num tokens (num_tokens=2304 avail_mem=48.72 GB):  19%|█▉        | 11/58 [00:00<00:02, 22.10it/s]Capturing num tokens (num_tokens=2304 avail_mem=48.72 GB):  28%|██▊       | 16/58 [00:00<00:01, 29.92it/s]Capturing num tokens (num_tokens=2048 avail_mem=48.72 GB):  28%|██▊       | 16/58 [00:00<00:01, 29.92it/s]Capturing num tokens (num_tokens=1792 avail_mem=48.71 GB):  28%|██▊       | 16/58 [00:00<00:01, 29.92it/s]Capturing num tokens (num_tokens=1536 avail_mem=48.71 GB):  28%|██▊       | 16/58 [00:00<00:01, 29.92it/s]Capturing num tokens (num_tokens=1280 avail_mem=48.70 GB):  28%|██▊       | 16/58 [00:00<00:01, 29.92it/s]Capturing num tokens (num_tokens=1024 avail_mem=48.68 GB):  28%|██▊       | 16/58 [00:00<00:01, 29.92it/s]

    Capturing num tokens (num_tokens=960 avail_mem=48.70 GB):  28%|██▊       | 16/58 [00:00<00:01, 29.92it/s] Capturing num tokens (num_tokens=960 avail_mem=48.70 GB):  38%|███▊      | 22/58 [00:00<00:00, 36.97it/s]Capturing num tokens (num_tokens=896 avail_mem=48.70 GB):  38%|███▊      | 22/58 [00:00<00:00, 36.97it/s]Capturing num tokens (num_tokens=832 avail_mem=48.69 GB):  38%|███▊      | 22/58 [00:00<00:00, 36.97it/s]Capturing num tokens (num_tokens=768 avail_mem=48.69 GB):  38%|███▊      | 22/58 [00:00<00:00, 36.97it/s]Capturing num tokens (num_tokens=704 avail_mem=48.69 GB):  38%|███▊      | 22/58 [00:01<00:00, 36.97it/s]Capturing num tokens (num_tokens=640 avail_mem=48.68 GB):  38%|███▊      | 22/58 [00:01<00:00, 36.97it/s]Capturing num tokens (num_tokens=576 avail_mem=48.68 GB):  38%|███▊      | 22/58 [00:01<00:00, 36.97it/s]Capturing num tokens (num_tokens=576 avail_mem=48.68 GB):  48%|████▊     | 28/58 [00:01<00:00, 41.56it/s]Capturing num tokens (num_tokens=512 avail_mem=48.67 GB):  48%|████▊     | 28/58 [00:01<00:00, 41.56it/s]Capturing num tokens (num_tokens=480 avail_mem=48.69 GB):  48%|████▊     | 28/58 [00:01<00:00, 41.56it/s]Capturing num tokens (num_tokens=448 avail_mem=48.68 GB):  48%|████▊     | 28/58 [00:01<00:00, 41.56it/s]Capturing num tokens (num_tokens=416 avail_mem=48.68 GB):  48%|████▊     | 28/58 [00:01<00:00, 41.56it/s]

    Capturing num tokens (num_tokens=384 avail_mem=48.68 GB):  48%|████▊     | 28/58 [00:01<00:00, 41.56it/s]Capturing num tokens (num_tokens=384 avail_mem=48.68 GB):  57%|█████▋    | 33/58 [00:01<00:00, 39.79it/s]Capturing num tokens (num_tokens=352 avail_mem=48.67 GB):  57%|█████▋    | 33/58 [00:01<00:00, 39.79it/s]Capturing num tokens (num_tokens=320 avail_mem=48.67 GB):  57%|█████▋    | 33/58 [00:01<00:00, 39.79it/s]

    Capturing num tokens (num_tokens=288 avail_mem=48.67 GB):  57%|█████▋    | 33/58 [00:01<00:00, 39.79it/s]Capturing num tokens (num_tokens=256 avail_mem=48.67 GB):  57%|█████▋    | 33/58 [00:01<00:00, 39.79it/s]Capturing num tokens (num_tokens=240 avail_mem=48.66 GB):  57%|█████▋    | 33/58 [00:01<00:00, 39.79it/s]Capturing num tokens (num_tokens=240 avail_mem=48.66 GB):  66%|██████▌   | 38/58 [00:01<00:00, 28.71it/s]Capturing num tokens (num_tokens=224 avail_mem=48.66 GB):  66%|██████▌   | 38/58 [00:01<00:00, 28.71it/s]Capturing num tokens (num_tokens=208 avail_mem=48.66 GB):  66%|██████▌   | 38/58 [00:01<00:00, 28.71it/s]Capturing num tokens (num_tokens=192 avail_mem=48.66 GB):  66%|██████▌   | 38/58 [00:01<00:00, 28.71it/s]Capturing num tokens (num_tokens=176 avail_mem=48.65 GB):  66%|██████▌   | 38/58 [00:01<00:00, 28.71it/s]Capturing num tokens (num_tokens=160 avail_mem=48.65 GB):  66%|██████▌   | 38/58 [00:01<00:00, 28.71it/s]

    Capturing num tokens (num_tokens=160 avail_mem=48.65 GB):  74%|███████▍  | 43/58 [00:01<00:00, 30.41it/s]Capturing num tokens (num_tokens=144 avail_mem=48.64 GB):  74%|███████▍  | 43/58 [00:01<00:00, 30.41it/s]Capturing num tokens (num_tokens=128 avail_mem=48.64 GB):  74%|███████▍  | 43/58 [00:01<00:00, 30.41it/s]Capturing num tokens (num_tokens=112 avail_mem=48.64 GB):  74%|███████▍  | 43/58 [00:01<00:00, 30.41it/s]

    Capturing num tokens (num_tokens=96 avail_mem=48.64 GB):  74%|███████▍  | 43/58 [00:01<00:00, 30.41it/s] Capturing num tokens (num_tokens=96 avail_mem=48.64 GB):  81%|████████  | 47/58 [00:01<00:00, 22.21it/s]Capturing num tokens (num_tokens=80 avail_mem=48.63 GB):  81%|████████  | 47/58 [00:01<00:00, 22.21it/s]Capturing num tokens (num_tokens=64 avail_mem=48.58 GB):  81%|████████  | 47/58 [00:02<00:00, 22.21it/s]

    Capturing num tokens (num_tokens=48 avail_mem=48.58 GB):  81%|████████  | 47/58 [00:02<00:00, 22.21it/s]Capturing num tokens (num_tokens=48 avail_mem=48.58 GB):  86%|████████▌ | 50/58 [00:02<00:00, 18.39it/s]Capturing num tokens (num_tokens=32 avail_mem=48.58 GB):  86%|████████▌ | 50/58 [00:02<00:00, 18.39it/s]Capturing num tokens (num_tokens=28 avail_mem=48.57 GB):  86%|████████▌ | 50/58 [00:02<00:00, 18.39it/s]

    Capturing num tokens (num_tokens=24 avail_mem=48.57 GB):  86%|████████▌ | 50/58 [00:02<00:00, 18.39it/s]Capturing num tokens (num_tokens=24 avail_mem=48.57 GB):  91%|█████████▏| 53/58 [00:02<00:00, 14.25it/s]Capturing num tokens (num_tokens=20 avail_mem=48.56 GB):  91%|█████████▏| 53/58 [00:02<00:00, 14.25it/s]

    Capturing num tokens (num_tokens=16 avail_mem=48.56 GB):  91%|█████████▏| 53/58 [00:02<00:00, 14.25it/s]Capturing num tokens (num_tokens=16 avail_mem=48.56 GB):  95%|█████████▍| 55/58 [00:02<00:00, 13.59it/s]Capturing num tokens (num_tokens=12 avail_mem=48.55 GB):  95%|█████████▍| 55/58 [00:02<00:00, 13.59it/s]Capturing num tokens (num_tokens=8 avail_mem=48.55 GB):  95%|█████████▍| 55/58 [00:02<00:00, 13.59it/s] 

    Capturing num tokens (num_tokens=8 avail_mem=48.55 GB):  98%|█████████▊| 57/58 [00:02<00:00, 12.92it/s]Capturing num tokens (num_tokens=4 avail_mem=48.55 GB):  98%|█████████▊| 57/58 [00:02<00:00, 12.92it/s]Capturing num tokens (num_tokens=4 avail_mem=48.55 GB): 100%|██████████| 58/58 [00:03<00:00, 18.85it/s]


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
    Generated text:  Rafał. I'm a student at the University of Wroclaw. I'm 21 years old and I have a degree in mechanical engineering. I'm from Poland and I love to play sports and music. I'm an avid fan of the Polish football club 2018-2019, which plays in the Liga 1. I play on the 2018-2019 team of 2018-2019. I also play football for the Limpopo team of the South Africa. I'm a good swimmer and I love to watch
    ===============================
    Prompt: The president of the United States is
    Generated text:  a very important person. He/she is in charge of the country and makes decisions. The president is a very important person because they make sure that everyone in the country gets what they are supposed to get. The president is also in charge of the country's money. They are like the boss of the country, like a boss of a family or a home. The president has to get a new job every four years. The new president has to be in the White House for about two years. The president is also very important because they are like the leader of the country. The president also has to deal with the other important people in the
    ===============================
    Prompt: The capital of France is
    Generated text: 
    A. Paris
    B. London
    C. Budapest
    D. Athens
    Answer: A
    
    Which of the following correctly describes the current cultural and historical background of Paris?
    A. The French capital, Paris is a landmark of Western civilization.
    B. Paris has been the world's cultural and historical center since ancient times.
    C. Paris is the oldest capital city in the world.
    D. The capital of France is Paris, a famous city for its historical significance.
    Answer: D
    
    The capital of France is
    A. Paris
    B. London
    C. Budapest
    D. Athens
    Answer: A
    
    Which of the
    ===============================
    Prompt: The future of AI is
    Generated text:  here, and this is a time of upheaval. No longer can we simply enjoy the benefits of artificial intelligence today. In fact, it is not possible to predict how our future will be shaped by the future of AI. In the coming years, we will see major changes in the way we live, work, and interact with each other. The future of AI will be a blend of complex algorithms and real-world data. It will not just be a force to be reckoned with, but a game-changer that will shape the world we live in.
    The following are some of the ways in which AI will change our future:
    1


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


    Generated text:  [Name], and I'm a [Job Title] at [Company Name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as the City of Light. It is a historic city with a rich history dating back to the Roman Empire and the Middle Ages. Paris is known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. The city is also famous for its cuisine, fashion, and art scene. Paris is a vibrant and dynamic city with a diverse population and a rich cultural heritage. It is a popular tourist destination and a major economic hub in Europe. The city is home to many world-renowned museums, theaters, and art galleries, and is a hub for business, finance,
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by rapid advancements in several key areas, including:
    
    1. Increased accuracy and precision: AI systems are becoming more accurate and precise in their predictions and decisions, leading to better outcomes in various fields such as healthcare, finance, and transportation.
    
    2. Integration with human intelligence: AI systems are likely to become more integrated with human intelligence, allowing for more complex and nuanced decision-making.
    
    3. Personalization and customization: AI systems are likely to become more personalized and customizable, allowing for more efficient and effective use of resources.
    
    4. Ethical and responsible development: As AI systems become more advanced, there will be a need for
    


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
    Generated text:  [Name], and I come from [Location]. I have always been fascinated by the wild and untamed beauty of the natural world and have been traveling around the world, experiencing the vastness of the natural world through my experiences. I have a passion for adventure, and I strive to always expand my knowledge and skills in order to grow as a leader in the field of environmentalism. And I'm always learning, and I don't hesitate to share my knowledge and experiences with others. What excites me most about my career is the opportunity to make a difference, to create change, and to have a positive impact on the world. And
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, the city where the Eiffel Tower and the Louvre Museum are located. It has a rich history dating back to the Roman Empire and the Middle Ages, and is known for its beautiful architecture, culture, and annual celebrations such as the Eiffel Tower Launch and the Paris Marathon. Paris is a popular tourist destination and is home to numerous museums, restaurants, and shopping districts. The city is also a hub for business and finance in Europe. Its inhabitants are known for their unique cuisine, and Parisians are considered to be one of the most popular and exciting cities in the world. The city has an important role in French
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be characterized by a number of different trends, depending on the specific areas of research and development. Some potential trends include:
    
      1. Increased integration with human intelligence: As AI becomes more capable of performing tasks that are currently the domain of humans, it may become more integrated with human intelligence to the extent that it can perform tasks that are difficult or impossible for humans to perform.
      2. AI ethics and safety concerns: As AI becomes more pervasive in our lives, there will be a growing concern about the ethical and safety implications of using AI in ways that are not aligned with human values. This could include issues such as


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

    /an

     [

    job

     title

    ]

     who

     has

     been

     studying

     for

     [

    number

    ]

     years

     in

     [

    field

     of

     study

    ].

     I

    'm

     a

    /an

     [

    age

    ]

     year

     old

     with

     [

    number

    ]

     years

     of

     experience

     in

     the

     industry

    .

     I

     am

     [

    job

     title

    ]

     at

     [

    company

     name

    ]

     and

     I

     have

     a

     strong

     [

    strength

     or

     expertise

    ].

     I

     love

     [

    reason

     why

     I

     love

     my

     job

    ].

     I

     enjoy

     [

    reason

     why

     I

     love

     my

     job

    ]

     by

     [

    reason

     why

    ].

     I

     am

     [

    reason

     why

     I

     am

     a

     good

     fit

     for

     this

     role

    ].

     I

     look

     forward

     to

     [

    reason

     why

    ].


    Dear

     [

    Name

    ],


    I

     am

     thrilled

     to

     meet

     you

    ,

     [

    Name

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .
    


    Paris

     is

     the

     capital

     of

     France

     and

     is

     the

     most

     populous

     city

     in

     the

     country

    ,

     with

     an

     estimated

     population

     of

     over

     

    2

    .

     

    3

     million

     people

    .

     The

     city

     is

     located

     on

     the

     banks

     of

     the

     Se

    ine

     River

     and

     is

     home

     to

     many

     of

     France

    's

     cultural

     institutions

    ,

     including

     the

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

     the

     E

    iff

    el

     Tower

    .

     Paris

     is

     also

     known

     for

     its

     iconic

     landmarks

     such

     as

     the

     E

    iff

    el

     Tower

    ,

     the

     Pont

     Ne

    uf

    ,

     and

     the

     Arc

     de

     Tri

    omp

    he

    .

     Its

     rich

     history

    ,

     romantic

     atmosphere

    ,

     and

     vibrant

     culture

     make

     it

     a

     popular

     tourist

     destination

     for

     many

    .

     As

     the

     capital

    ,

     Paris

     plays

     a

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     bound

     to

     be

     unpredictable

    ,

     but

     there

     are

     several

     potential

     trends

     that

     are

     likely

     to

     shape

     the

     development

     of

     this

     technology

    .

     Some

     of

     the

     most

     notable

     trends

     include

    :
    


    1

    .

     Increased

     integration

     of

     AI

     into

     industries

    :

     As

     AI

     becomes

     more

     prevalent

     in

     various

     industries

    ,

     we

     can

     expect

     to

     see

     even

     more

     integration

     of

     AI

     into

     our

     daily

     lives

    .

     For

     example

    ,

     we

     can

     expect

     to

     see

     more

     automation

     in

     manufacturing

    ,

     transportation

    ,

     healthcare

    ,

     and

     education

    .
    


    2

    .

     Development

     of

     ethical

     AI

    :

     The

     future

     of

     AI

     will

     also

     involve

     the

     development

     of

     AI

     that

     is

     designed

     to

     be

     ethical

     and

     transparent

    .

     This

     will

     involve

     the

     use

     of

     AI

     to

     address

     social

     issues

     such

     as

     bias

    ,

     transparency

    ,

     and

    



```python
llm.shutdown()
```
