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


    2026-03-28 06:00:45.307 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 06:00:45] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 06:00:45.307 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 06:00:45] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 06:00:45.307 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 06:00:45] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 06:00:45.308 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 06:00:45] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 06:00:45.308 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 06:00:45] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.09it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.09it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<00:57,  1.03s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<00:57,  1.03s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<00:57,  1.03s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:22,  2.40it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:22,  2.40it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:22,  2.40it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:22,  2.40it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:02<00:10,  4.95it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:02<00:10,  4.95it/s]Compiling num tokens (num_tokens=4096):  12%|█▏        | 7/58 [00:02<00:10,  4.95it/s]Compiling num tokens (num_tokens=3840):  12%|█▏        | 7/58 [00:02<00:10,  4.95it/s]

    Compiling num tokens (num_tokens=3584):  12%|█▏        | 7/58 [00:02<00:10,  4.95it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:05,  8.90it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:05,  8.90it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:05,  8.90it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:05,  8.90it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:05,  8.90it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:05,  8.90it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:02<00:02, 14.52it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:02<00:02, 14.52it/s]Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:02<00:02, 14.52it/s]Compiling num tokens (num_tokens=1536):  28%|██▊       | 16/58 [00:02<00:02, 14.52it/s]Compiling num tokens (num_tokens=1280):  28%|██▊       | 16/58 [00:02<00:02, 14.52it/s]

    Compiling num tokens (num_tokens=1024):  28%|██▊       | 16/58 [00:03<00:02, 14.52it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:03<00:01, 20.18it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:03<00:01, 20.18it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:03<00:01, 20.18it/s]Compiling num tokens (num_tokens=832):  36%|███▌      | 21/58 [00:03<00:01, 20.18it/s]Compiling num tokens (num_tokens=768):  36%|███▌      | 21/58 [00:03<00:01, 20.18it/s]Compiling num tokens (num_tokens=704):  36%|███▌      | 21/58 [00:03<00:01, 20.18it/s]Compiling num tokens (num_tokens=640):  36%|███▌      | 21/58 [00:03<00:01, 20.18it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:03<00:01, 27.59it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:03<00:01, 27.59it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:03<00:01, 27.59it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:03<00:01, 27.59it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:03<00:01, 27.59it/s]Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:03<00:01, 27.59it/s]

    Compiling num tokens (num_tokens=384):  47%|████▋     | 27/58 [00:03<00:01, 27.59it/s]Compiling num tokens (num_tokens=352):  47%|████▋     | 27/58 [00:03<00:01, 27.59it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:03<00:00, 35.74it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:03<00:00, 35.74it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:03<00:00, 35.74it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:03<00:00, 35.74it/s]Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:03<00:00, 35.74it/s]Compiling num tokens (num_tokens=224):  59%|█████▊    | 34/58 [00:03<00:00, 35.74it/s]Compiling num tokens (num_tokens=208):  59%|█████▊    | 34/58 [00:03<00:00, 35.74it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:03<00:00, 39.59it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:03<00:00, 39.59it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:03<00:00, 39.59it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:03<00:00, 39.59it/s]

    Compiling num tokens (num_tokens=144):  69%|██████▉   | 40/58 [00:03<00:00, 39.59it/s]Compiling num tokens (num_tokens=128):  69%|██████▉   | 40/58 [00:03<00:00, 39.59it/s]Compiling num tokens (num_tokens=112):  69%|██████▉   | 40/58 [00:03<00:00, 39.59it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:03<00:00, 42.53it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:03<00:00, 42.53it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:03<00:00, 42.53it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:03<00:00, 42.53it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:03<00:00, 42.53it/s]Compiling num tokens (num_tokens=32):  79%|███████▉  | 46/58 [00:03<00:00, 42.53it/s]Compiling num tokens (num_tokens=28):  79%|███████▉  | 46/58 [00:03<00:00, 42.53it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:03<00:00, 45.26it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:03<00:00, 45.26it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:03<00:00, 45.26it/s]

    Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:03<00:00, 45.26it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:03<00:00, 45.26it/s]Compiling num tokens (num_tokens=8):  90%|████████▉ | 52/58 [00:03<00:00, 45.26it/s] Compiling num tokens (num_tokens=4):  90%|████████▉ | 52/58 [00:03<00:00, 45.26it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.80it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.75 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.69 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.69 GB):   3%|▎         | 2/58 [00:00<00:04, 11.63it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.69 GB):   3%|▎         | 2/58 [00:00<00:04, 11.63it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=118.68 GB):   3%|▎         | 2/58 [00:00<00:04, 11.63it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.68 GB):   7%|▋         | 4/58 [00:00<00:04, 12.52it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.68 GB):   7%|▋         | 4/58 [00:00<00:04, 12.52it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.68 GB):   7%|▋         | 4/58 [00:00<00:04, 12.52it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.68 GB):  10%|█         | 6/58 [00:00<00:03, 13.85it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.68 GB):  10%|█         | 6/58 [00:00<00:03, 13.85it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=118.67 GB):  10%|█         | 6/58 [00:00<00:03, 13.85it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.67 GB):  14%|█▍        | 8/58 [00:00<00:03, 15.64it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.67 GB):  14%|█▍        | 8/58 [00:00<00:03, 15.64it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.66 GB):  14%|█▍        | 8/58 [00:00<00:03, 15.64it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.65 GB):  14%|█▍        | 8/58 [00:00<00:03, 15.64it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.65 GB):  19%|█▉        | 11/58 [00:00<00:02, 18.41it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.65 GB):  19%|█▉        | 11/58 [00:00<00:02, 18.41it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=118.64 GB):  19%|█▉        | 11/58 [00:00<00:02, 18.41it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.64 GB):  19%|█▉        | 11/58 [00:00<00:02, 18.41it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.64 GB):  24%|██▍       | 14/58 [00:00<00:02, 20.99it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.63 GB):  24%|██▍       | 14/58 [00:00<00:02, 20.99it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.62 GB):  24%|██▍       | 14/58 [00:00<00:02, 20.99it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.61 GB):  24%|██▍       | 14/58 [00:00<00:02, 20.99it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.61 GB):  24%|██▍       | 14/58 [00:00<00:02, 20.99it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=118.61 GB):  31%|███       | 18/58 [00:00<00:01, 24.38it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.60 GB):  31%|███       | 18/58 [00:00<00:01, 24.38it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.60 GB):  31%|███       | 18/58 [00:00<00:01, 24.38it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.55 GB):  31%|███       | 18/58 [00:00<00:01, 24.38it/s]Capturing num tokens (num_tokens=960 avail_mem=118.58 GB):  31%|███       | 18/58 [00:01<00:01, 24.38it/s] Capturing num tokens (num_tokens=960 avail_mem=118.58 GB):  38%|███▊      | 22/58 [00:01<00:01, 28.11it/s]Capturing num tokens (num_tokens=896 avail_mem=118.58 GB):  38%|███▊      | 22/58 [00:01<00:01, 28.11it/s]Capturing num tokens (num_tokens=832 avail_mem=118.57 GB):  38%|███▊      | 22/58 [00:01<00:01, 28.11it/s]Capturing num tokens (num_tokens=768 avail_mem=118.56 GB):  38%|███▊      | 22/58 [00:01<00:01, 28.11it/s]Capturing num tokens (num_tokens=704 avail_mem=118.54 GB):  38%|███▊      | 22/58 [00:01<00:01, 28.11it/s]

    Capturing num tokens (num_tokens=704 avail_mem=118.54 GB):  45%|████▍     | 26/58 [00:01<00:01, 31.26it/s]Capturing num tokens (num_tokens=640 avail_mem=118.53 GB):  45%|████▍     | 26/58 [00:01<00:01, 31.26it/s]Capturing num tokens (num_tokens=576 avail_mem=118.53 GB):  45%|████▍     | 26/58 [00:01<00:01, 31.26it/s]Capturing num tokens (num_tokens=512 avail_mem=118.53 GB):  45%|████▍     | 26/58 [00:01<00:01, 31.26it/s]Capturing num tokens (num_tokens=480 avail_mem=118.54 GB):  45%|████▍     | 26/58 [00:01<00:01, 31.26it/s]Capturing num tokens (num_tokens=480 avail_mem=118.54 GB):  52%|█████▏    | 30/58 [00:01<00:00, 33.03it/s]Capturing num tokens (num_tokens=448 avail_mem=118.53 GB):  52%|█████▏    | 30/58 [00:01<00:00, 33.03it/s]Capturing num tokens (num_tokens=416 avail_mem=118.53 GB):  52%|█████▏    | 30/58 [00:01<00:00, 33.03it/s]Capturing num tokens (num_tokens=384 avail_mem=118.52 GB):  52%|█████▏    | 30/58 [00:01<00:00, 33.03it/s]Capturing num tokens (num_tokens=352 avail_mem=118.52 GB):  52%|█████▏    | 30/58 [00:01<00:00, 33.03it/s]

    Capturing num tokens (num_tokens=352 avail_mem=118.52 GB):  59%|█████▊    | 34/58 [00:01<00:00, 34.20it/s]Capturing num tokens (num_tokens=320 avail_mem=118.51 GB):  59%|█████▊    | 34/58 [00:01<00:00, 34.20it/s]Capturing num tokens (num_tokens=288 avail_mem=118.50 GB):  59%|█████▊    | 34/58 [00:01<00:00, 34.20it/s]Capturing num tokens (num_tokens=256 avail_mem=118.49 GB):  59%|█████▊    | 34/58 [00:01<00:00, 34.20it/s]Capturing num tokens (num_tokens=240 avail_mem=118.49 GB):  59%|█████▊    | 34/58 [00:01<00:00, 34.20it/s]Capturing num tokens (num_tokens=240 avail_mem=118.49 GB):  66%|██████▌   | 38/58 [00:01<00:00, 35.20it/s]Capturing num tokens (num_tokens=224 avail_mem=118.48 GB):  66%|██████▌   | 38/58 [00:01<00:00, 35.20it/s]Capturing num tokens (num_tokens=208 avail_mem=118.47 GB):  66%|██████▌   | 38/58 [00:01<00:00, 35.20it/s]Capturing num tokens (num_tokens=192 avail_mem=118.47 GB):  66%|██████▌   | 38/58 [00:01<00:00, 35.20it/s]Capturing num tokens (num_tokens=176 avail_mem=118.47 GB):  66%|██████▌   | 38/58 [00:01<00:00, 35.20it/s]

    Capturing num tokens (num_tokens=176 avail_mem=118.47 GB):  72%|███████▏  | 42/58 [00:01<00:00, 36.11it/s]Capturing num tokens (num_tokens=160 avail_mem=118.46 GB):  72%|███████▏  | 42/58 [00:01<00:00, 36.11it/s]Capturing num tokens (num_tokens=144 avail_mem=118.45 GB):  72%|███████▏  | 42/58 [00:01<00:00, 36.11it/s]Capturing num tokens (num_tokens=128 avail_mem=118.44 GB):  72%|███████▏  | 42/58 [00:01<00:00, 36.11it/s]Capturing num tokens (num_tokens=112 avail_mem=118.44 GB):  72%|███████▏  | 42/58 [00:01<00:00, 36.11it/s]Capturing num tokens (num_tokens=112 avail_mem=118.44 GB):  79%|███████▉  | 46/58 [00:01<00:00, 35.87it/s]Capturing num tokens (num_tokens=96 avail_mem=118.45 GB):  79%|███████▉  | 46/58 [00:01<00:00, 35.87it/s] Capturing num tokens (num_tokens=80 avail_mem=118.44 GB):  79%|███████▉  | 46/58 [00:01<00:00, 35.87it/s]Capturing num tokens (num_tokens=64 avail_mem=118.44 GB):  79%|███████▉  | 46/58 [00:01<00:00, 35.87it/s]Capturing num tokens (num_tokens=48 avail_mem=118.44 GB):  79%|███████▉  | 46/58 [00:01<00:00, 35.87it/s]

    Capturing num tokens (num_tokens=48 avail_mem=118.44 GB):  86%|████████▌ | 50/58 [00:01<00:00, 36.33it/s]Capturing num tokens (num_tokens=32 avail_mem=118.42 GB):  86%|████████▌ | 50/58 [00:01<00:00, 36.33it/s]Capturing num tokens (num_tokens=28 avail_mem=118.42 GB):  86%|████████▌ | 50/58 [00:01<00:00, 36.33it/s]Capturing num tokens (num_tokens=24 avail_mem=118.41 GB):  86%|████████▌ | 50/58 [00:01<00:00, 36.33it/s]Capturing num tokens (num_tokens=20 avail_mem=118.40 GB):  86%|████████▌ | 50/58 [00:01<00:00, 36.33it/s]Capturing num tokens (num_tokens=20 avail_mem=118.40 GB):  93%|█████████▎| 54/58 [00:01<00:00, 37.22it/s]Capturing num tokens (num_tokens=16 avail_mem=118.40 GB):  93%|█████████▎| 54/58 [00:01<00:00, 37.22it/s]Capturing num tokens (num_tokens=12 avail_mem=118.39 GB):  93%|█████████▎| 54/58 [00:01<00:00, 37.22it/s]Capturing num tokens (num_tokens=8 avail_mem=118.39 GB):  93%|█████████▎| 54/58 [00:01<00:00, 37.22it/s] Capturing num tokens (num_tokens=4 avail_mem=118.38 GB):  93%|█████████▎| 54/58 [00:01<00:00, 37.22it/s]

    Capturing num tokens (num_tokens=4 avail_mem=118.38 GB): 100%|██████████| 58/58 [00:01<00:00, 37.90it/s]Capturing num tokens (num_tokens=4 avail_mem=118.38 GB): 100%|██████████| 58/58 [00:01<00:00, 29.30it/s]


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
    Generated text:  a student from Japan. I'm 15 years old. I like to visit different places and I like to eat a lot of different kinds of food. I like to eat chicken, turkey, salmon, fish, eggs, pasta, bread and rice. And I like to drink coffee and milk. My family is very busy, and so I don't have much time to cook. I like to cook meals for my family every day. I have to cook breakfast, lunch, dinner and a special meal every day. I always cook a special meal for my family every day. It's a very important meal for us. I also
    ===============================
    Prompt: The president of the United States is
    Generated text:  a very busy person. He is very tired after a very long day of work. He spends many hours on the phone talking to people he does not know. He likes to go to the beach to relax after work. He goes to the beach once a year to visit his family. He does not like to drink too much alcohol, because he thinks it is not good for him. The president likes to eat lots of different kinds of food and drinks. He drinks milk, fruit juice, bread, cheese, and eggs. He likes to eat chocolate, nuts, and strawberries. He prefers a green or white salad. He drinks orange juice
    ===============================
    Prompt: The capital of France is
    Generated text:  ________. A. Paris B. Brussels C. London D. Moscow
    A. Paris
    
    The capital of France is Paris. Paris is the largest city in France and serves as its capital, with its main city center and numerous suburbs. The city is known for its historic landmarks, including the Eiffel Tower and Notre-Dame Cathedral, as well as its art scene and cultural attractions. Other notable Parisian landmarks include the Louvre Museum and the Musée d'Orsay, and the Gare de Lyon railway station. Paris is a popular tourist destination and holds significant cultural and historical importance in France. While other options like Brussels
    ===============================
    Prompt: The future of AI is
    Generated text:  bright, and for many companies, it is a major focus of research and investment. The possibilities for AI are endless, and some of the biggest companies in the world have already started to see the potential for AI to transform their businesses. In this article, we will explore what AI can do and how it is being used today.
    AI has the potential to transform many industries and make our lives easier in many ways. With the help of AI, we can automate many tasks, reduce costs, and improve efficiency. In the future, AI will be used in fields as diverse as healthcare, finance, transportation, manufacturing, and more.
    AI is


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


    Generated text:  [Name], and I'm a [Age] year old [Occupation]. I'm a [Skill] person who enjoys [Favorite Activity] with my [Favorite Person]. I'm always [Positive Traits]. I'm [Your Name] and I'm [Your Profession]. I'm [Your Age] years old and I'm [Your Occupation]. I'm a [Skill] person who enjoys [Favorite Activity] with my [Favorite Person]. I'm always [Positive Traits]. I'm [Your Name] and I'm [Your Profession]. I'm [Your Age] years old and I'm [Your Occupation]. I'm a [
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as the City of Light. It is the largest city in France and the second-largest city in the European Union. Paris is known for its rich history, beautiful architecture, and vibrant culture. It is home to many famous landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. Paris is also a major center for business, finance, and entertainment. It is a popular tourist destination and a cultural hub for France. The city is home to many museums, theaters, and other cultural institutions. Paris is a city of contrasts, with its modern architecture and historical landmarks blending seamlessly. It
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends that are expected to shape the future of AI:
    
    1. Increased use of AI in healthcare: AI is already being used in healthcare to improve patient outcomes, reduce costs, and improve the quality of care. As AI technology continues to improve, we can expect to see even more widespread use of AI in healthcare, with more sophisticated algorithms and machine learning techniques being developed to improve diagnosis, treatment, and patient care.
    
    2. Increased use of AI in finance: AI is already being used in
    


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
    Generated text:  ____. I am a/an [insert profession] and I love [insert something related to the profession]. I am passionate about [insert a hobby or interest related to the profession] and I enjoy [insert a value or goal related to the profession]. I am a/an [insert occupation] and I believe in [insert a value or philosophy related to the profession]. I have been working in this field for [insert a number of years] and I have always been [insert an adjective related to the profession]. I am a/an [insert an adjective] professional and I am always [insert a verb related to the profession]. I am a
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, which is home to the iconic Eiffel Tower and is known for its historical landmarks and cultural events. It is also one of the most important cities in Europe, hosting the Eiffel Tower, Louvre Museum, and many other attractions. However, it is also the most expensive city in the world to live in, with housing costs often exceeding those in neighboring cities. Paris is known for its vibrant nightlife and food scene, but it is also a city with a strong focus on education and art, as evidenced by the numerous museums and galleries in the city. Its long history and cultural significance make it an important city for both
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be characterized by the continued development and application of new technologies, as well as the integration of AI into increasingly diverse and complex systems. Here are some possible future trends in AI:
    
    1. Increased use of AI in healthcare: AI is already being used in healthcare to improve patient outcomes and streamline medical research. As AI technologies continue to improve, we can expect to see even more widespread use of AI in healthcare, with more sophisticated algorithms and machine learning techniques being developed.
    
    2. AI in transportation: Autonomous vehicles are already being developed, and it is likely that we will see more widespread use of AI in transportation in the future. AI-powered


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

    Occup

    ation

    ]

    !

     I

    'm

     excited

     to

     meet

     you

     today

    .

     What

     can

     I

     help

     you

     with

    ?

     Let

    's

     get

     started

    !

     [

    Name

    ]

     knows

     nothing

     about

     this

     subject

    ,

     so

     [

    Name

    ]

     is

     ready

     to

     learn

     anything

     you

     want

     to

     know

    .

     [

    Name

    ]

     is

     always

     here

     to

     help

     you

     and

     is

     always

     ready

     to

     answer

     any

     questions

     you

     might

     have

    .

     What

    's

     your

     name

    ?

     [

    Name

    ]

     is

     a

     [

    Occup

    ation

    ].

     What

    's

     your

     name

    ?

     [

    Name

    ]

     is

     a

     [

    Occup

    ation

    ].

     What

    's

     your

     name

    ?

     [

    Name

    ]

     is

     a

     [

    Occup

    ation

    ].

     What

    's

     your

     name

    ?

     [

    Name

    ]

     is

     a

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     known

     for

     its

     historic

     landmarks

    ,

     vibrant

     culture

    ,

     and

     iconic

     symbols

     such

     as

     Notre

    -D

    ame

     Cathedral

     and

     E

    iff

    el

     Tower

    .

     The

     city

     is

     also

     a

     major

     center

     for

     fashion

    ,

     art

    ,

     and

     music

    .

     It

     has

     a

     population

     of

     over

     

    2

    .

    3

     million

     people

     and

     is

     one

     of

     the

     world

    's

     most

     liv

    able

     cities

    .

     Additionally

    ,

     Paris

     is

     a

     major

     hub

     for

     science

     and

     technology

    ,

     with

     numerous

     universities

     and

     research

     institutions

    .

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     vast

     and

     varied

    ,

     and

     it

    's

     difficult

     to

     predict

     exactly

     what

     will

     happen

    .

     However

    ,

     there

     are

     several

     potential

     trends

     that

     experts

     believe

     are

     likely

     to

     shape

     the

     field

     in

     the

     coming

     years

    :
    


    1

    .

     Increased

     use

     of

     AI

     in

     healthcare

    :

     AI

     will

     continue

     to

     play

     a

     central

     role

     in

     healthcare

    ,

     with

     developments

     in

     natural

     language

     processing

     and

     computer

     vision

     expected

     to

     lead

     to

     new

     ways

     to

     diagnose

     and

     treat

     diseases

    .

     AI

     could

     also

     help

     improve

     patient

     outcomes

     by

     autom

    ating

     routine

     tasks

     and

     reducing

     the

     risk

     of

     human

     error

    .
    


    2

    .

     Personal

    ized

     healthcare

    :

     AI

     will

     become

     more

     integrated

     into

     healthcare

     systems

    ,

     allowing

     doctors

     and

     healthcare

     providers

     to

     provide

     tailored

     treatments

     based

     on

     an

     individual

     patient

    's

     unique

    



```python
llm.shutdown()
```
