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


    2026-04-01 04:05:58.682 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 04:05:58] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 04:05:58.682 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 04:05:58] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 04:05:58.682 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 04:05:58] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 04:05:58.682 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 04:05:58] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 04:05:58.682 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 04:05:58] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.27it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.25it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:34,  2.71s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:34,  2.71s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:34,  2.71s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:34,  2.71s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:02<00:13,  3.68it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:02<00:13,  3.68it/s]Compiling num tokens (num_tokens=4096):  12%|█▏        | 7/58 [00:02<00:13,  3.68it/s]Compiling num tokens (num_tokens=3840):  12%|█▏        | 7/58 [00:03<00:13,  3.68it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:03<00:08,  5.92it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:03<00:08,  5.92it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:03<00:08,  5.92it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:03<00:08,  5.92it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:03<00:08,  5.92it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:03<00:04,  9.50it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:03<00:04,  9.50it/s]Compiling num tokens (num_tokens=2304):  24%|██▍       | 14/58 [00:03<00:04,  9.50it/s]Compiling num tokens (num_tokens=2048):  24%|██▍       | 14/58 [00:03<00:04,  9.50it/s]Compiling num tokens (num_tokens=1792):  24%|██▍       | 14/58 [00:03<00:04,  9.50it/s]Compiling num tokens (num_tokens=1536):  24%|██▍       | 14/58 [00:03<00:04,  9.50it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:02, 14.48it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:02, 14.48it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:02, 14.48it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:02, 14.48it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:02, 14.48it/s]

    Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:02, 14.48it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:03<00:01, 19.92it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:03<00:01, 19.92it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:03<00:01, 19.92it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:03<00:01, 19.92it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:03<00:01, 19.92it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:03<00:01, 19.92it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:03<00:01, 25.27it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:03<00:01, 25.27it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:03<00:01, 25.27it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:03<00:01, 25.27it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:03<00:01, 25.27it/s]Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:03<00:01, 25.27it/s]

    Compiling num tokens (num_tokens=320):  50%|█████     | 29/58 [00:03<00:01, 25.27it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:03<00:00, 31.60it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:03<00:00, 31.60it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:03<00:00, 31.60it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:03<00:00, 31.60it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:03<00:00, 31.60it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:03<00:00, 31.60it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:03<00:00, 34.60it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:03<00:00, 34.60it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:03<00:00, 34.60it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:03<00:00, 34.60it/s]Compiling num tokens (num_tokens=144):  69%|██████▉   | 40/58 [00:03<00:00, 34.60it/s]

    Compiling num tokens (num_tokens=128):  69%|██████▉   | 40/58 [00:03<00:00, 34.60it/s]Compiling num tokens (num_tokens=112):  69%|██████▉   | 40/58 [00:03<00:00, 34.60it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:03<00:00, 39.31it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:03<00:00, 39.31it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:03<00:00, 39.31it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:03<00:00, 39.31it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:03<00:00, 39.31it/s]Compiling num tokens (num_tokens=32):  79%|███████▉  | 46/58 [00:03<00:00, 39.31it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 41.26it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 41.26it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 41.26it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 41.26it/s]

    Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 41.26it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 41.26it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:04<00:00, 41.26it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:04<00:00, 41.26it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 48.10it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 14.40it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.79 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.79 GB):   2%|▏         | 1/58 [00:00<00:06,  8.71it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.76 GB):   2%|▏         | 1/58 [00:00<00:06,  8.71it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=118.76 GB):   3%|▎         | 2/58 [00:00<00:06,  9.29it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.76 GB):   3%|▎         | 2/58 [00:00<00:06,  9.29it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.75 GB):   3%|▎         | 2/58 [00:00<00:06,  9.29it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.76 GB):   3%|▎         | 2/58 [00:00<00:06,  9.29it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.76 GB):   9%|▊         | 5/58 [00:00<00:03, 16.32it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.75 GB):   9%|▊         | 5/58 [00:00<00:03, 16.32it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.75 GB):   9%|▊         | 5/58 [00:00<00:03, 16.32it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.75 GB):   9%|▊         | 5/58 [00:00<00:03, 16.32it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=118.75 GB):  14%|█▍        | 8/58 [00:00<00:02, 20.74it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.75 GB):  14%|█▍        | 8/58 [00:00<00:02, 20.74it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.74 GB):  14%|█▍        | 8/58 [00:00<00:02, 20.74it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.74 GB):  14%|█▍        | 8/58 [00:00<00:02, 20.74it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=118.74 GB):  19%|█▉        | 11/58 [00:00<00:02, 16.81it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.74 GB):  19%|█▉        | 11/58 [00:00<00:02, 16.81it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.73 GB):  19%|█▉        | 11/58 [00:00<00:02, 16.81it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.73 GB):  22%|██▏       | 13/58 [00:00<00:02, 17.00it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.73 GB):  22%|██▏       | 13/58 [00:00<00:02, 17.00it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.73 GB):  22%|██▏       | 13/58 [00:00<00:02, 17.00it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=118.72 GB):  22%|██▏       | 13/58 [00:00<00:02, 17.00it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.72 GB):  28%|██▊       | 16/58 [00:00<00:02, 19.02it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.72 GB):  28%|██▊       | 16/58 [00:00<00:02, 19.02it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.72 GB):  28%|██▊       | 16/58 [00:00<00:02, 19.02it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.71 GB):  28%|██▊       | 16/58 [00:00<00:02, 19.02it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.71 GB):  33%|███▎      | 19/58 [00:01<00:01, 20.83it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.71 GB):  33%|███▎      | 19/58 [00:01<00:01, 20.83it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.69 GB):  33%|███▎      | 19/58 [00:01<00:01, 20.83it/s]

    Capturing num tokens (num_tokens=960 avail_mem=118.70 GB):  33%|███▎      | 19/58 [00:01<00:01, 20.83it/s] Capturing num tokens (num_tokens=960 avail_mem=118.70 GB):  38%|███▊      | 22/58 [00:01<00:01, 22.86it/s]Capturing num tokens (num_tokens=896 avail_mem=118.70 GB):  38%|███▊      | 22/58 [00:01<00:01, 22.86it/s]Capturing num tokens (num_tokens=832 avail_mem=118.70 GB):  38%|███▊      | 22/58 [00:01<00:01, 22.86it/s]Capturing num tokens (num_tokens=768 avail_mem=118.69 GB):  38%|███▊      | 22/58 [00:01<00:01, 22.86it/s]Capturing num tokens (num_tokens=704 avail_mem=118.69 GB):  38%|███▊      | 22/58 [00:01<00:01, 22.86it/s]Capturing num tokens (num_tokens=704 avail_mem=118.69 GB):  45%|████▍     | 26/58 [00:01<00:01, 25.64it/s]Capturing num tokens (num_tokens=640 avail_mem=118.69 GB):  45%|████▍     | 26/58 [00:01<00:01, 25.64it/s]Capturing num tokens (num_tokens=576 avail_mem=118.69 GB):  45%|████▍     | 26/58 [00:01<00:01, 25.64it/s]

    Capturing num tokens (num_tokens=512 avail_mem=118.68 GB):  45%|████▍     | 26/58 [00:01<00:01, 25.64it/s]Capturing num tokens (num_tokens=480 avail_mem=118.69 GB):  45%|████▍     | 26/58 [00:01<00:01, 25.64it/s]Capturing num tokens (num_tokens=480 avail_mem=118.69 GB):  52%|█████▏    | 30/58 [00:01<00:01, 27.78it/s]Capturing num tokens (num_tokens=448 avail_mem=118.69 GB):  52%|█████▏    | 30/58 [00:01<00:01, 27.78it/s]Capturing num tokens (num_tokens=416 avail_mem=118.69 GB):  52%|█████▏    | 30/58 [00:01<00:01, 27.78it/s]Capturing num tokens (num_tokens=384 avail_mem=118.69 GB):  52%|█████▏    | 30/58 [00:01<00:01, 27.78it/s]Capturing num tokens (num_tokens=352 avail_mem=118.68 GB):  52%|█████▏    | 30/58 [00:01<00:01, 27.78it/s]Capturing num tokens (num_tokens=352 avail_mem=118.68 GB):  59%|█████▊    | 34/58 [00:01<00:00, 30.66it/s]Capturing num tokens (num_tokens=320 avail_mem=118.68 GB):  59%|█████▊    | 34/58 [00:01<00:00, 30.66it/s]Capturing num tokens (num_tokens=288 avail_mem=118.67 GB):  59%|█████▊    | 34/58 [00:01<00:00, 30.66it/s]

    Capturing num tokens (num_tokens=256 avail_mem=118.67 GB):  59%|█████▊    | 34/58 [00:01<00:00, 30.66it/s]Capturing num tokens (num_tokens=240 avail_mem=118.67 GB):  59%|█████▊    | 34/58 [00:01<00:00, 30.66it/s]Capturing num tokens (num_tokens=224 avail_mem=118.66 GB):  59%|█████▊    | 34/58 [00:01<00:00, 30.66it/s]Capturing num tokens (num_tokens=224 avail_mem=118.66 GB):  67%|██████▋   | 39/58 [00:01<00:00, 34.86it/s]Capturing num tokens (num_tokens=208 avail_mem=118.66 GB):  67%|██████▋   | 39/58 [00:01<00:00, 34.86it/s]Capturing num tokens (num_tokens=192 avail_mem=118.66 GB):  67%|██████▋   | 39/58 [00:01<00:00, 34.86it/s]Capturing num tokens (num_tokens=176 avail_mem=118.66 GB):  67%|██████▋   | 39/58 [00:01<00:00, 34.86it/s]Capturing num tokens (num_tokens=160 avail_mem=118.65 GB):  67%|██████▋   | 39/58 [00:01<00:00, 34.86it/s]Capturing num tokens (num_tokens=144 avail_mem=118.65 GB):  67%|██████▋   | 39/58 [00:01<00:00, 34.86it/s]Capturing num tokens (num_tokens=144 avail_mem=118.65 GB):  76%|███████▌  | 44/58 [00:01<00:00, 38.02it/s]Capturing num tokens (num_tokens=128 avail_mem=118.65 GB):  76%|███████▌  | 44/58 [00:01<00:00, 38.02it/s]Capturing num tokens (num_tokens=112 avail_mem=118.48 GB):  76%|███████▌  | 44/58 [00:01<00:00, 38.02it/s]

    Capturing num tokens (num_tokens=96 avail_mem=118.48 GB):  76%|███████▌  | 44/58 [00:01<00:00, 38.02it/s] Capturing num tokens (num_tokens=80 avail_mem=118.47 GB):  76%|███████▌  | 44/58 [00:01<00:00, 38.02it/s]Capturing num tokens (num_tokens=80 avail_mem=118.47 GB):  83%|████████▎ | 48/58 [00:01<00:00, 33.66it/s]Capturing num tokens (num_tokens=64 avail_mem=118.43 GB):  83%|████████▎ | 48/58 [00:01<00:00, 33.66it/s]Capturing num tokens (num_tokens=48 avail_mem=118.00 GB):  83%|████████▎ | 48/58 [00:01<00:00, 33.66it/s]Capturing num tokens (num_tokens=32 avail_mem=117.99 GB):  83%|████████▎ | 48/58 [00:01<00:00, 33.66it/s]

    Capturing num tokens (num_tokens=28 avail_mem=117.99 GB):  83%|████████▎ | 48/58 [00:01<00:00, 33.66it/s]Capturing num tokens (num_tokens=28 avail_mem=117.99 GB):  90%|████████▉ | 52/58 [00:02<00:00, 31.21it/s]Capturing num tokens (num_tokens=24 avail_mem=117.81 GB):  90%|████████▉ | 52/58 [00:02<00:00, 31.21it/s]Capturing num tokens (num_tokens=20 avail_mem=117.80 GB):  90%|████████▉ | 52/58 [00:02<00:00, 31.21it/s]Capturing num tokens (num_tokens=16 avail_mem=117.80 GB):  90%|████████▉ | 52/58 [00:02<00:00, 31.21it/s]Capturing num tokens (num_tokens=12 avail_mem=117.80 GB):  90%|████████▉ | 52/58 [00:02<00:00, 31.21it/s]Capturing num tokens (num_tokens=12 avail_mem=117.80 GB):  97%|█████████▋| 56/58 [00:02<00:00, 30.56it/s]Capturing num tokens (num_tokens=8 avail_mem=117.80 GB):  97%|█████████▋| 56/58 [00:02<00:00, 30.56it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=117.79 GB):  97%|█████████▋| 56/58 [00:02<00:00, 30.56it/s]Capturing num tokens (num_tokens=4 avail_mem=117.79 GB): 100%|██████████| 58/58 [00:02<00:00, 26.16it/s]


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
    Generated text:  Reina. My name is Reina and I’m a new member of this group. I recently joined on June 15, 2009. I was born in Mexico but I live in California. I also have a big sister. She’s 26 years old. She’s single and we live in a small town. We have our own little garden in our yard, and we grow tomatoes, broccoli, and eggplants. Reina and I are both in the same grade level and class, but we're not in the same grade. I live right next door to my classmate. I think we've
    ===============================
    Prompt: The president of the United States is
    Generated text:  a position of great significance. The American people have a strong political culture that makes them very sensitive to political disagreements and conflicts. So, when the president makes a decision, it is subject to a great deal of debate and negotiation. And the people of the United States have a strong history of not voting against the president when he makes decisions. The US president can only be impeached and removed from office if he is found guilty by the US Congress, and it is usually a unanimous vote of the Senate.
    During the election cycle, the incumbent president is running for re-election. This is also known as a “re-election” because it is
    ===============================
    Prompt: The capital of France is
    Generated text: : B
    
    A. Paris  
    B. Rome  
    C. Berlin  
    D. Warsaw
    The capital of France is Paris. Therefore, the correct answer is:
    
    A. Paris  
    B. Rome  
    C. Berlin  
    D. Warsaw  
    A. Paris  
    B. Rome  
    C. Berlin  
    D. Warsaw  
    Paris is the capital of France and is also known for its historical landmarks such as the Eiffel Tower and the Louvre Museum. 
    
    So the correct answer is A. Paris. 
    
    The other options are not capitals of France, and therefore do not correctly represent their capital cities. Berlin is the capital of Germany
    ===============================
    Prompt: The future of AI is
    Generated text:  virtual reality
    In 2022, we are going to see the rise of a new generation of AI, and it’s going to be powered by virtual reality. According to Dr. François Bonneau, chief engineer and director of the National Center for Virtual Reality at the Massachusetts Institute of Technology, this new generation of AI will be the result of virtual reality. He explained: “Virtual reality will impact all fields of scientific research and even have an impact on the face of the human race.”
    One of the important trends that virtual reality will bring is a new generation of AI. This new generation of AI will be virtual, and it


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


    Generated text:  [Name] and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, which is known for its iconic landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. It is also home to the French Parliament and the French Parliament House. Paris is a bustling city with a rich history and culture, and it is a popular tourist destination. The city is known for its fashion, art, and cuisine, and it is a major economic and financial center in Europe. Paris is a city that is both a cultural and political center of France, and it is a major hub for international trade and diplomacy. The city is also known for its food and drink scene, with a wide
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends that could shape the future of AI:
    
    1. Increased use of AI in healthcare: AI is already being used in healthcare to improve patient outcomes, reduce costs, and improve the quality of care. As AI technology continues to improve, we can expect to see even more widespread use of AI in healthcare, particularly in areas such as diagnosis, treatment planning, and patient monitoring.
    
    2. Increased use of AI in finance: AI is already being used in finance to improve risk management, fraud detection
    


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
    Generated text:  [insert your name], and I am a computer programmer with over five years of experience in the field. I have a passion for exploring new programming languages and technologies, and I am always looking for ways to improve my skills and stay up-to-date with the latest trends. I am a team player, enjoy collaborating with others, and am always looking for ways to make my code more efficient and effective. Thank you for considering me for a position. Good luck with your career! [Your name]. 
    This seems like a good start. Can you tell me more about your background and experience with programming? What are some of the languages and technologies
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, the city known for its art, fashion, and historical significance. It is also the world’s largest city and the fourth-largest by population, with a population of over 2.5 million people. The city is home to the Louvre Museum, the Eiffel Tower, the Notre-Dame Cathedral, and a diverse range of art, music, and cultural events throughout the year. It is a historic center of Europe and an important commercial, cultural, and political hub for France. 
    
    Paris is also a major center for education, research, and innovation, with a wide range of world-renowned universities and research institutions.
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be characterized by several trends, including:
    
    1. More pervasive use of AI: As AI technology continues to improve, we are likely to see a more widespread use of AI in various sectors, such as healthcare, finance, and transportation. This will lead to more automation and efficiency gains, but it will also require human oversight and decision-making.
    
    2. Enhanced ethical considerations: AI will continue to evolve, and we will need to address potential ethical concerns, such as privacy, bias, and transparency. Governments and organizations will need to develop and implement regulations to govern AI development and use.
    
    3. Increased integration with human consciousness: AI will


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

    name

    ]

     and

     I

    'm

     [

    age

    ].

     I

    'm

     a

     [

    type

     of

     skill

    ]

     with

     [

    career

     objective

     or

     past

     experiences

    ].

     I

     enjoy

     [

    reason

     for

     my

     hobby

     or

     interest

    ].

     I

    'm

     passionate

     about

     [

    what

     I

     enjoy

     doing

    ]

     and

     I

     strive

     to

     be

     the

     best

     version

     of

     myself

    .

     I

    'm

     always

     looking

     to

     learn

     new

     things

     and

     grow

     professionally

    .

     What

    's

     your

     favorite

     hobby

     or

     interest

    ?

     Let

     me

     know

     what

     you

     think

    !

     

    🌟

    🌟

    🌟

    
    


    A

     short

    ,

     neutral

     self

    -int

    roduction

     like

     this

     would

     be

     a

     great

     way

     to

     get

     people

     familiar

     with

     your

     character

    .

     It

     would

     allow

     them

     to

     see

     your

     personality

     and

     skills

     from

     a

     neutral

     perspective

    ,

     without

     having

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     known

     for

     its

     iconic

     E

    iff

    el

     Tower

    ,

     as

     well

     as

     its

     diverse

     cultural

     scene

     and

     world

    -class

     museums

    .

     It

     is

     also

     home

     to

     the

     Lou

    vre

     Museum

     and

     the

     Notre

    -D

    ame

     Cathedral

    ,

     both

     of

     which

     are

     UNESCO

     World

     Heritage

     sites

    .

     It

     serves

     as

     the

     French

     capital

     since

     

    1

    8

    7

    8

    ,

     making

     it

     France

    ’s

     largest

     city

     and

     a

     major

     economic

     and

     cultural

     hub

    .

     The

     city

     is

     also

     known

     for

     its

     rich

     French

     heritage

    ,

     including

     its

     ne

    oc

    lass

    ical

     architecture

     and

     its

     role

     in

     French

     history

     and

     politics

    .

     Despite

     its

     fame

    ,

     Paris

     is

     a

     historic

     city

     with

     a

     rich

     history

     dating

     back

     to

     the

     Roman

     Empire

    .

     The

     city

     is

     home

     to

     many

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     exciting

     and

     full

     of

     possibilities

    .

     Here

     are

     some

     possible

     trends

     that

     we

     can

     expect

     in

     the

     AI

     field

    :
    


    1

    .

     Deep

     Learning

    :

     Deep

     learning

     is

     an

     emerging

     technology

     that

     relies

     on

     neural

     networks

     to

     learn

     complex

     patterns

     from

     raw

     data

    .

     It

     is

     expected

     to

     continue

     to

     dominate

     AI

     research

     in

     the

     coming

     years

    ,

     and

     it

     is

     already

     being

     used

     in

     a

     variety

     of

     applications

    ,

     including

     image

     and

     speech

     recognition

    ,

     natural

     language

     processing

    ,

     and

     autonomous

     vehicles

    .
    


    2

    .

     Explain

    ability

    :

     One

     of

     the

     biggest

     challenges

     in

     AI

     is

     the

     difficulty

     of

     understanding

     how

     it

     makes

     decisions

    .

     Researchers

     are

     looking

     for

     ways

     to

     make

     AI

     systems

     more

     explain

    able

    ,

     so

     that

     people

     can

     understand

     how

     the

     system

     arrived

    



```python
llm.shutdown()
```
