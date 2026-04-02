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


    2026-04-02 22:12:50.455 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 22:12:50] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 22:12:50.456 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 22:12:50] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 22:12:50.456 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 22:12:50] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 22:12:50.456 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 22:12:50] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 22:12:50.456 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 22:12:50] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.36it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.35it/s]


    2026-04-02 22:12:54,852 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 22:12:54] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:32,  2.68s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:32,  2.68s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<01:05,  1.16s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<01:05,  1.16s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<01:05,  1.16s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:25,  2.14it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:25,  2.14it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:25,  2.14it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:25,  2.14it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:03<00:11,  4.49it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:03<00:11,  4.49it/s]Compiling num tokens (num_tokens=4096):  12%|█▏        | 7/58 [00:03<00:11,  4.49it/s]Compiling num tokens (num_tokens=3840):  12%|█▏        | 7/58 [00:03<00:11,  4.49it/s]

    Compiling num tokens (num_tokens=3584):  12%|█▏        | 7/58 [00:03<00:11,  4.49it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:03<00:05,  8.13it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:03<00:05,  8.13it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:03<00:05,  8.13it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:03<00:05,  8.13it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:03<00:05,  8.13it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:03<00:05,  8.13it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:03<00:03, 13.48it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:03<00:03, 13.48it/s]Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:03<00:03, 13.48it/s]Compiling num tokens (num_tokens=1536):  28%|██▊       | 16/58 [00:03<00:03, 13.48it/s]

    Compiling num tokens (num_tokens=1280):  28%|██▊       | 16/58 [00:03<00:03, 13.48it/s]Compiling num tokens (num_tokens=1024):  28%|██▊       | 16/58 [00:03<00:03, 13.48it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:03<00:01, 18.86it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:03<00:01, 18.86it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:03<00:01, 18.86it/s]Compiling num tokens (num_tokens=832):  36%|███▌      | 21/58 [00:03<00:01, 18.86it/s]Compiling num tokens (num_tokens=768):  36%|███▌      | 21/58 [00:03<00:01, 18.86it/s]Compiling num tokens (num_tokens=704):  36%|███▌      | 21/58 [00:03<00:01, 18.86it/s]Compiling num tokens (num_tokens=640):  36%|███▌      | 21/58 [00:03<00:01, 18.86it/s]Compiling num tokens (num_tokens=576):  36%|███▌      | 21/58 [00:03<00:01, 18.86it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:03<00:01, 27.66it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:03<00:01, 27.66it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:03<00:01, 27.66it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:03<00:01, 27.66it/s]

    Compiling num tokens (num_tokens=416):  48%|████▊     | 28/58 [00:03<00:01, 27.66it/s]Compiling num tokens (num_tokens=384):  48%|████▊     | 28/58 [00:03<00:01, 27.66it/s]Compiling num tokens (num_tokens=352):  48%|████▊     | 28/58 [00:03<00:01, 27.66it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:03<00:00, 33.77it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:03<00:00, 33.77it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:03<00:00, 33.77it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:03<00:00, 33.77it/s]Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:03<00:00, 33.77it/s]Compiling num tokens (num_tokens=224):  59%|█████▊    | 34/58 [00:03<00:00, 33.77it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 36.21it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 36.21it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 36.21it/s]

    Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 36.21it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 36.21it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 36.21it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 39.36it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 39.36it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 39.36it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 39.36it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 39.36it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 39.36it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:03<00:00, 39.82it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:03<00:00, 39.82it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:03<00:00, 39.82it/s]

    Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:03<00:00, 39.82it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:03<00:00, 39.82it/s]Compiling num tokens (num_tokens=20):  84%|████████▍ | 49/58 [00:03<00:00, 39.82it/s]Compiling num tokens (num_tokens=16):  84%|████████▍ | 49/58 [00:03<00:00, 39.82it/s]Compiling num tokens (num_tokens=16):  95%|█████████▍| 55/58 [00:04<00:00, 44.70it/s]Compiling num tokens (num_tokens=12):  95%|█████████▍| 55/58 [00:04<00:00, 44.70it/s]Compiling num tokens (num_tokens=8):  95%|█████████▍| 55/58 [00:04<00:00, 44.70it/s] Compiling num tokens (num_tokens=4):  95%|█████████▍| 55/58 [00:04<00:00, 44.70it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 14.32it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.53 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.50 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.50 GB):   3%|▎         | 2/58 [00:00<00:05, 10.61it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.50 GB):   3%|▎         | 2/58 [00:00<00:05, 10.61it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=118.49 GB):   3%|▎         | 2/58 [00:00<00:05, 10.61it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.49 GB):   7%|▋         | 4/58 [00:00<00:04, 11.57it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.50 GB):   7%|▋         | 4/58 [00:00<00:04, 11.57it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.49 GB):   7%|▋         | 4/58 [00:00<00:04, 11.57it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=118.49 GB):  10%|█         | 6/58 [00:00<00:04, 12.79it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.49 GB):  10%|█         | 6/58 [00:00<00:04, 12.79it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.49 GB):  10%|█         | 6/58 [00:00<00:04, 12.79it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.49 GB):  14%|█▍        | 8/58 [00:00<00:03, 14.09it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.49 GB):  14%|█▍        | 8/58 [00:00<00:03, 14.09it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.48 GB):  14%|█▍        | 8/58 [00:00<00:03, 14.09it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=118.48 GB):  14%|█▍        | 8/58 [00:00<00:03, 14.09it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.48 GB):  19%|█▉        | 11/58 [00:00<00:02, 16.60it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.48 GB):  19%|█▉        | 11/58 [00:00<00:02, 16.60it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.47 GB):  19%|█▉        | 11/58 [00:00<00:02, 16.60it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.47 GB):  19%|█▉        | 11/58 [00:00<00:02, 16.60it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.47 GB):  24%|██▍       | 14/58 [00:00<00:02, 18.86it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.47 GB):  24%|██▍       | 14/58 [00:00<00:02, 18.86it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.46 GB):  24%|██▍       | 14/58 [00:00<00:02, 18.86it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=118.46 GB):  24%|██▍       | 14/58 [00:00<00:02, 18.86it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.46 GB):  24%|██▍       | 14/58 [00:00<00:02, 18.86it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.45 GB):  24%|██▍       | 14/58 [00:00<00:02, 18.86it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.45 GB):  33%|███▎      | 19/58 [00:00<00:01, 26.16it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.45 GB):  33%|███▎      | 19/58 [00:00<00:01, 26.16it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.43 GB):  33%|███▎      | 19/58 [00:01<00:01, 26.16it/s]Capturing num tokens (num_tokens=960 avail_mem=118.44 GB):  33%|███▎      | 19/58 [00:01<00:01, 26.16it/s] Capturing num tokens (num_tokens=896 avail_mem=118.44 GB):  33%|███▎      | 19/58 [00:01<00:01, 26.16it/s]Capturing num tokens (num_tokens=832 avail_mem=118.44 GB):  33%|███▎      | 19/58 [00:01<00:01, 26.16it/s]Capturing num tokens (num_tokens=832 avail_mem=118.44 GB):  41%|████▏     | 24/58 [00:01<00:01, 31.54it/s]Capturing num tokens (num_tokens=768 avail_mem=118.43 GB):  41%|████▏     | 24/58 [00:01<00:01, 31.54it/s]

    Capturing num tokens (num_tokens=704 avail_mem=118.43 GB):  41%|████▏     | 24/58 [00:01<00:01, 31.54it/s]Capturing num tokens (num_tokens=640 avail_mem=118.43 GB):  41%|████▏     | 24/58 [00:01<00:01, 31.54it/s]Capturing num tokens (num_tokens=576 avail_mem=118.43 GB):  41%|████▏     | 24/58 [00:01<00:01, 31.54it/s]Capturing num tokens (num_tokens=512 avail_mem=118.42 GB):  41%|████▏     | 24/58 [00:01<00:01, 31.54it/s]Capturing num tokens (num_tokens=512 avail_mem=118.42 GB):  50%|█████     | 29/58 [00:01<00:00, 35.28it/s]Capturing num tokens (num_tokens=480 avail_mem=118.43 GB):  50%|█████     | 29/58 [00:01<00:00, 35.28it/s]Capturing num tokens (num_tokens=448 avail_mem=118.43 GB):  50%|█████     | 29/58 [00:01<00:00, 35.28it/s]Capturing num tokens (num_tokens=416 avail_mem=118.43 GB):  50%|█████     | 29/58 [00:01<00:00, 35.28it/s]Capturing num tokens (num_tokens=384 avail_mem=118.43 GB):  50%|█████     | 29/58 [00:01<00:00, 35.28it/s]Capturing num tokens (num_tokens=352 avail_mem=118.42 GB):  50%|█████     | 29/58 [00:01<00:00, 35.28it/s]

    Capturing num tokens (num_tokens=352 avail_mem=118.42 GB):  59%|█████▊    | 34/58 [00:01<00:00, 38.20it/s]Capturing num tokens (num_tokens=320 avail_mem=118.42 GB):  59%|█████▊    | 34/58 [00:01<00:00, 38.20it/s]Capturing num tokens (num_tokens=288 avail_mem=118.41 GB):  59%|█████▊    | 34/58 [00:01<00:00, 38.20it/s]Capturing num tokens (num_tokens=256 avail_mem=118.41 GB):  59%|█████▊    | 34/58 [00:01<00:00, 38.20it/s]Capturing num tokens (num_tokens=240 avail_mem=118.41 GB):  59%|█████▊    | 34/58 [00:01<00:00, 38.20it/s]Capturing num tokens (num_tokens=224 avail_mem=118.40 GB):  59%|█████▊    | 34/58 [00:01<00:00, 38.20it/s]Capturing num tokens (num_tokens=224 avail_mem=118.40 GB):  67%|██████▋   | 39/58 [00:01<00:00, 40.17it/s]Capturing num tokens (num_tokens=208 avail_mem=118.40 GB):  67%|██████▋   | 39/58 [00:01<00:00, 40.17it/s]Capturing num tokens (num_tokens=192 avail_mem=118.40 GB):  67%|██████▋   | 39/58 [00:01<00:00, 40.17it/s]Capturing num tokens (num_tokens=176 avail_mem=118.40 GB):  67%|██████▋   | 39/58 [00:01<00:00, 40.17it/s]Capturing num tokens (num_tokens=160 avail_mem=118.39 GB):  67%|██████▋   | 39/58 [00:01<00:00, 40.17it/s]

    Capturing num tokens (num_tokens=144 avail_mem=118.39 GB):  67%|██████▋   | 39/58 [00:01<00:00, 40.17it/s]Capturing num tokens (num_tokens=144 avail_mem=118.39 GB):  76%|███████▌  | 44/58 [00:01<00:00, 39.20it/s]Capturing num tokens (num_tokens=128 avail_mem=118.39 GB):  76%|███████▌  | 44/58 [00:01<00:00, 39.20it/s]Capturing num tokens (num_tokens=112 avail_mem=118.38 GB):  76%|███████▌  | 44/58 [00:01<00:00, 39.20it/s]Capturing num tokens (num_tokens=96 avail_mem=118.38 GB):  76%|███████▌  | 44/58 [00:01<00:00, 39.20it/s] Capturing num tokens (num_tokens=80 avail_mem=118.38 GB):  76%|███████▌  | 44/58 [00:01<00:00, 39.20it/s]Capturing num tokens (num_tokens=80 avail_mem=118.38 GB):  83%|████████▎ | 48/58 [00:01<00:00, 37.57it/s]Capturing num tokens (num_tokens=64 avail_mem=118.37 GB):  83%|████████▎ | 48/58 [00:01<00:00, 37.57it/s]Capturing num tokens (num_tokens=48 avail_mem=118.37 GB):  83%|████████▎ | 48/58 [00:01<00:00, 37.57it/s]

    Capturing num tokens (num_tokens=32 avail_mem=118.36 GB):  83%|████████▎ | 48/58 [00:01<00:00, 37.57it/s]Capturing num tokens (num_tokens=28 avail_mem=118.36 GB):  83%|████████▎ | 48/58 [00:01<00:00, 37.57it/s]Capturing num tokens (num_tokens=28 avail_mem=118.36 GB):  90%|████████▉ | 52/58 [00:01<00:00, 35.90it/s]Capturing num tokens (num_tokens=24 avail_mem=118.36 GB):  90%|████████▉ | 52/58 [00:01<00:00, 35.90it/s]Capturing num tokens (num_tokens=20 avail_mem=118.36 GB):  90%|████████▉ | 52/58 [00:01<00:00, 35.90it/s]Capturing num tokens (num_tokens=16 avail_mem=118.36 GB):  90%|████████▉ | 52/58 [00:01<00:00, 35.90it/s]Capturing num tokens (num_tokens=12 avail_mem=118.35 GB):  90%|████████▉ | 52/58 [00:01<00:00, 35.90it/s]Capturing num tokens (num_tokens=12 avail_mem=118.35 GB):  97%|█████████▋| 56/58 [00:01<00:00, 35.69it/s]Capturing num tokens (num_tokens=8 avail_mem=118.35 GB):  97%|█████████▋| 56/58 [00:01<00:00, 35.69it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=118.34 GB):  97%|█████████▋| 56/58 [00:01<00:00, 35.69it/s]Capturing num tokens (num_tokens=4 avail_mem=118.34 GB): 100%|██████████| 58/58 [00:01<00:00, 29.25it/s]


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
    Generated text:  Svetlana, the creator of "Svetlana in the Morning" and "Béla the Great".
    
    You are the main character of the game "Svetlana". The game was created by me as a way to play in multiplayer mode.
    
    You are a hero of the world. You like to play. You are fond of the sun. When the sun rises, you are ready to play. You play with your friends. You are known for your pastime. You are so popular that you are often a target of harassment.
    
    I wish you to be my sidekick and friend, someone to look up to
    ===============================
    Prompt: The president of the United States is
    Generated text:  a new president, what are the chances of winning the presidency? I think winning a presidential election is based on being a natural born citizen of the United States. How can natural born citizens be the new president if they are not an American citizen? Natural born citizen has nothing to do with the nation. Natural born citizens of other countries can not be American citizens. The president of the United States has the right to choose new president. If the president choose a natural born citizen to be the new president, the new president is automatically a natural born citizen.
    I think the chances of winning the presidency are very high because the president has the power to
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. Paris was founded in 787 AD and is the capital of France. The population of Paris in 2019 was 2.3 million. The population of Paris in 2019 was approximately 2.24 million. To get rid of the dependence of Paris on its water supply, the city began to use a different method of water supply, which is called "precipitation". The city was famous for its "precipitation" until the 1980s when the amount of rain in Paris started to decrease. In 2019, Paris recorded a precipitation
    ===============================
    Prompt: The future of AI is
    Generated text:  in the making. The whole world is turning towards AI in various fields like health, entertainment, technology, and manufacturing. However, at the same time, the AI itself is also not fully developed. AI is a very complex and extensive system. It is not only used in medical fields but also in all industries. These include healthcare, manufacturing, finance, education, and more. In a world that is becoming increasingly connected, AI has become a pivotal factor that can transform our lives and make our lives easier.
    AI is built on a foundation of big data, machine learning, and deep learning. With the help of these tools, AI systems


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


    Generated text:  [Name], and I'm a [Job Title] at [Company Name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? As a [Job Title], I'm always looking for ways to improve my skills and knowledge. I'm always eager to learn and grow, and I'm always looking for new challenges and opportunities to grow. What are some of your favorite hobbies or interests? As a [Job Title], I'm always looking for ways to improve my skills and knowledge. I'm always eager to learn and grow, and I'm always looking for new challenges and opportunities to grow.
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris. 
    
    This statement is factually correct and provides a clear and concise overview of the capital city's location and significance. However, it could be expanded to include additional information about Paris's history, culture, or notable landmarks, depending on the audience and purpose of the statement. For example, it could be expanded to mention that Paris is the birthplace of the French Revolution and the birthplace of the French language, or that it is the largest city in France by population. Ultimately, the statement should be specific and informative, providing a well-rounded and accurate representation of Paris's importance and significance. 
    
    In conclusion, the statement "The
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends that could be expected in the future:
    
    1. Increased automation: One of the most significant trends in AI is the increasing automation of tasks that were previously done by humans. This could lead to the creation of more efficient and cost-effective systems that can perform a wide range of tasks, from manufacturing to healthcare.
    
    2. Improved privacy and security: As AI becomes more integrated into our daily lives, there will be an increased need for privacy and security. This could lead to the development of new
    


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
    Generated text:  [Name] and I am a/an [Age] year old. I was born and raised in [Your Location]. I am a [Gender] who is [Your Profession]. I am passionate about [Your Hobby/Interest/Industry], and I am always looking for ways to [Your Goal]. I am always eager to learn and grow, and I am always eager to share my knowledge with others. 
    
    If you had to describe me in one word, it would be [Me]. I believe that [Me] is always ready to help others, and I am always willing to lend a hand when needed. 
    
    I hope to become
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, known for its iconic landmarks such as Notre-Dame Cathedral, the Eiffel Tower, and the Louvre Museum. It is the birthplace of French and European literature and has a rich cultural heritage, with numerous museums, theaters, and artistic institutions. Paris is also a major tourist destination, known for its cosmopolitan atmosphere and its cultural richness. It is a historic and modern city, known for its vibrant nightlife and its many festivals throughout the year. Paris is the world's most populous city and one of the most visited cities in the world. It is also a major center of science and research, with many universities and research
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  uncertain and highly complex, but here are some potential trends that could emerge in the coming years:
    
    1. Increased Integration: AI will become more integrated with everyday technology, such as voice assistants, smart homes, and wearable devices. This will allow machines to better understand and interact with humans, and may even lead to the development of new technologies like self-driving cars and robots that are more intuitive and capable of learning from experience.
    
    2. Personalization: AI will continue to improve, and we will see more personalized experiences as people's needs and preferences become more data-driven. This could result in more efficient and effective use of resources and could lead to


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

     [

    age

    ]

     years

     old

    .

     I

     am

     [

    gender

    ]

     and

     have

     [

    occupation

    ]

     work

    .

     I

     am

     [

    gender

    ]

     and

     have

     [

    occupation

    ]

     work

    .

     I

     am

     [

    gender

    ]

     and

     have

     [

    occupation

    ]

     work

    .

     I

     am

     [

    gender

    ]

     and

     have

     [

    occupation

    ]

     work

    .

     I

     am

     [

    gender

    ]

     and

     have

     [

    occupation

    ]

     work

    .

     I

     am

     [

    gender

    ]

     and

     have

     [

    occupation

    ]

     work

    .

     I

     am

     [

    gender

    ]

     and

     have

     [

    occupation

    ]

     work

    .

     I

     am

     [

    gender

    ]

     and

     have

     [

    occupation

    ]

     work

    .

     I

     am

     [

    gender

    ]

     and

     have

     [

    occupation

    ]

     work

    .

     I

     am

     [

    gender

    ]

     and

     have

     [

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     also

     known

     as

     the

     City

     of

     Light

     and

     the

     Eternal

     City

    .

     It

     is

     the

     largest

     city

     in

     Europe

     and

     one

     of

     the

     most

     visited

     in

     the

     world

    .

     The

     city

     is

     home

     to

     numerous

     landmarks

    ,

     including

     the

     E

    iff

    el

     Tower

    ,

     Notre

    -D

    ame

     Cathedral

    ,

     Lou

    vre

     Museum

    ,

     and

     the

     Mus

    ée

     Rod

    in

    .

     It

     is

     also

     known

     for

     its

     rich

     cultural

     history

     and

     cuisine

    .

     Paris

     is

     a

     vibrant

     and

     diverse

     city

     with

     a

     diverse

     population

     and

     a

     thriving

     arts

     community

    .

     Its

     status

     as

     a

     major

     global

     hub

     for

     commerce

     and

     finance

     has

     also

     made

     it

     a

     popular

     destination

     for

     tourists

     and

     business

     executives

    .

     Paris

     is

     a

     city

     of

     contrasts

     and

     a

     city

     of

     beauty

    ,

     making

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     highly

     uncertain

    ,

     but

     some

     possible

     trends

     that

     could

     influence

     it

     include

    :
    


    1

    .

     Improved

     computational

     power

    :

     As

     technology

     improves

    ,

     AI

     systems

     will

     become

     more

     powerful

     and

     capable

     of

     processing

     complex

     data

     sets

    .

     This

     could

     lead

     to

     significant

     advances

     in

     areas

     such

     as

     speech

     recognition

    ,

     natural

     language

     understanding

    ,

     and

     robotics

    .
    


    2

    .

     Increased

     focus

     on

     ethical

     considerations

    :

     As

     AI

     becomes

     more

     widely

     used

    ,

     there

     will

     be

     increased

     pressure

     to

     consider

     the

     ethical

     implications

     of

     its

     use

    .

     This

     could

     lead

     to

     greater

     attention

     to

     privacy

    ,

     bias

    ,

     and

     other

     issues

    ,

     as

     well

     as

     more

     rigorous

     testing

     and

     verification

     of

     AI

     systems

    .
    


    3

    .

     Greater

     reliance

     on

     AI

     for

     decision

    -making

    :

     As

     AI

     becomes

     more

     integrated

    



```python
llm.shutdown()
```
