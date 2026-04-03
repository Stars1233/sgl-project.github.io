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


    2026-04-03 04:35:36.695 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 04:35:36] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 04:35:36.695 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 04:35:36] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 04:35:36.695 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 04:35:36] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 04:35:36.695 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 04:35:36] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 04:35:36.695 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 04:35:36] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.84it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.83it/s]


    2026-04-03 04:35:41,539 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 04:35:41] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:50,  2.99s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:50,  2.99s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:12,  1.30s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:12,  1.30s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:03<01:12,  1.30s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:28,  1.92it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:28,  1.92it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:03<00:28,  1.92it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:03<00:15,  3.33it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:03<00:15,  3.33it/s]Compiling num tokens (num_tokens=4608):  10%|█         | 6/58 [00:03<00:15,  3.33it/s]Compiling num tokens (num_tokens=4096):  10%|█         | 6/58 [00:03<00:15,  3.33it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:03<00:08,  5.90it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:03<00:08,  5.90it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:03<00:08,  5.90it/s]Compiling num tokens (num_tokens=3328):  16%|█▌        | 9/58 [00:03<00:08,  5.90it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:03<00:05,  8.75it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:03<00:05,  8.75it/s]Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:03<00:05,  8.75it/s]Compiling num tokens (num_tokens=2560):  21%|██        | 12/58 [00:03<00:05,  8.75it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:03<00:03, 11.90it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:03<00:03, 11.90it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:03<00:03, 11.90it/s]Compiling num tokens (num_tokens=1792):  26%|██▌       | 15/58 [00:03<00:03, 11.90it/s]Compiling num tokens (num_tokens=1536):  26%|██▌       | 15/58 [00:03<00:03, 11.90it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:02, 16.63it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:02, 16.63it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:02, 16.63it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:02, 16.63it/s] 

    Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:02, 16.63it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:03<00:01, 20.12it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:03<00:01, 20.12it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:03<00:01, 20.12it/s]Compiling num tokens (num_tokens=704):  40%|███▉      | 23/58 [00:03<00:01, 20.12it/s]Compiling num tokens (num_tokens=640):  40%|███▉      | 23/58 [00:03<00:01, 20.12it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:04<00:01, 23.42it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:04<00:01, 23.42it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:04<00:01, 23.42it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:04<00:01, 23.42it/s]

    Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:04<00:01, 23.42it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:04<00:01, 24.57it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:04<00:01, 24.57it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:04<00:01, 24.57it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:04<00:01, 24.57it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:04<00:01, 24.57it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:04<00:00, 26.75it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:04<00:00, 26.75it/s]

    Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:04<00:00, 26.75it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:04<00:00, 26.75it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:04<00:00, 26.75it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:04<00:00, 27.48it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:04<00:00, 27.48it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:04<00:00, 27.48it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:04<00:00, 27.48it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:04<00:00, 27.48it/s]

    Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:04<00:00, 29.33it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:04<00:00, 29.33it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:04<00:00, 29.33it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:04<00:00, 29.33it/s]Compiling num tokens (num_tokens=96):  74%|███████▍  | 43/58 [00:04<00:00, 29.33it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:04<00:00, 28.87it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:04<00:00, 28.87it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:04<00:00, 28.87it/s]

    Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:04<00:00, 28.87it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:04<00:00, 28.87it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:04<00:00, 31.51it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:04<00:00, 31.51it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:04<00:00, 31.51it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:04<00:00, 31.51it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:04<00:00, 31.51it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:04<00:00, 31.51it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:04<00:00, 31.51it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:04<00:00, 31.51it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 40.31it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 11.86it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.80 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.80 GB):   2%|▏         | 1/58 [00:00<00:07,  7.40it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.77 GB):   2%|▏         | 1/58 [00:00<00:07,  7.40it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=118.77 GB):   3%|▎         | 2/58 [00:00<00:07,  7.28it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.77 GB):   3%|▎         | 2/58 [00:00<00:07,  7.28it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.77 GB):   5%|▌         | 3/58 [00:00<00:06,  7.87it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.77 GB):   5%|▌         | 3/58 [00:00<00:06,  7.87it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=118.77 GB):   7%|▋         | 4/58 [00:00<00:06,  8.50it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.77 GB):   7%|▋         | 4/58 [00:00<00:06,  8.50it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.77 GB):   9%|▊         | 5/58 [00:00<00:06,  8.40it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.76 GB):   9%|▊         | 5/58 [00:00<00:06,  8.40it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=118.77 GB):   9%|▊         | 5/58 [00:00<00:06,  8.40it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.77 GB):  12%|█▏        | 7/58 [00:00<00:05,  9.17it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.76 GB):  12%|█▏        | 7/58 [00:00<00:05,  9.17it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=118.76 GB):  12%|█▏        | 7/58 [00:00<00:05,  9.17it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.76 GB):  16%|█▌        | 9/58 [00:00<00:05,  9.78it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.76 GB):  16%|█▌        | 9/58 [00:00<00:05,  9.78it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.75 GB):  16%|█▌        | 9/58 [00:01<00:05,  9.78it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=118.75 GB):  19%|█▉        | 11/58 [00:01<00:04, 10.58it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.75 GB):  19%|█▉        | 11/58 [00:01<00:04, 10.58it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.74 GB):  19%|█▉        | 11/58 [00:01<00:04, 10.58it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.74 GB):  22%|██▏       | 13/58 [00:01<00:04, 11.21it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.74 GB):  22%|██▏       | 13/58 [00:01<00:04, 11.21it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.99 GB):  22%|██▏       | 13/58 [00:01<00:04, 11.21it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=118.99 GB):  22%|██▏       | 13/58 [00:01<00:04, 11.21it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.98 GB):  22%|██▏       | 13/58 [00:01<00:04, 11.21it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.98 GB):  29%|██▉       | 17/58 [00:01<00:02, 17.46it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.98 GB):  29%|██▉       | 17/58 [00:01<00:02, 17.46it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.98 GB):  29%|██▉       | 17/58 [00:01<00:02, 17.46it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.97 GB):  29%|██▉       | 17/58 [00:01<00:02, 17.46it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.95 GB):  29%|██▉       | 17/58 [00:01<00:02, 17.46it/s]Capturing num tokens (num_tokens=960 avail_mem=118.96 GB):  29%|██▉       | 17/58 [00:01<00:02, 17.46it/s] Capturing num tokens (num_tokens=960 avail_mem=118.96 GB):  38%|███▊      | 22/58 [00:01<00:01, 24.41it/s]Capturing num tokens (num_tokens=896 avail_mem=118.96 GB):  38%|███▊      | 22/58 [00:01<00:01, 24.41it/s]Capturing num tokens (num_tokens=832 avail_mem=118.96 GB):  38%|███▊      | 22/58 [00:01<00:01, 24.41it/s]

    Capturing num tokens (num_tokens=768 avail_mem=118.95 GB):  38%|███▊      | 22/58 [00:01<00:01, 24.41it/s]Capturing num tokens (num_tokens=704 avail_mem=118.95 GB):  38%|███▊      | 22/58 [00:01<00:01, 24.41it/s]Capturing num tokens (num_tokens=640 avail_mem=118.95 GB):  38%|███▊      | 22/58 [00:01<00:01, 24.41it/s]Capturing num tokens (num_tokens=640 avail_mem=118.95 GB):  47%|████▋     | 27/58 [00:01<00:01, 29.91it/s]Capturing num tokens (num_tokens=576 avail_mem=118.95 GB):  47%|████▋     | 27/58 [00:01<00:01, 29.91it/s]Capturing num tokens (num_tokens=512 avail_mem=118.94 GB):  47%|████▋     | 27/58 [00:01<00:01, 29.91it/s]Capturing num tokens (num_tokens=480 avail_mem=118.95 GB):  47%|████▋     | 27/58 [00:01<00:01, 29.91it/s]Capturing num tokens (num_tokens=448 avail_mem=118.95 GB):  47%|████▋     | 27/58 [00:01<00:01, 29.91it/s]Capturing num tokens (num_tokens=448 avail_mem=118.95 GB):  53%|█████▎    | 31/58 [00:01<00:00, 32.30it/s]Capturing num tokens (num_tokens=416 avail_mem=118.95 GB):  53%|█████▎    | 31/58 [00:01<00:00, 32.30it/s]Capturing num tokens (num_tokens=384 avail_mem=118.95 GB):  53%|█████▎    | 31/58 [00:01<00:00, 32.30it/s]

    Capturing num tokens (num_tokens=352 avail_mem=118.94 GB):  53%|█████▎    | 31/58 [00:01<00:00, 32.30it/s]Capturing num tokens (num_tokens=320 avail_mem=118.94 GB):  53%|█████▎    | 31/58 [00:01<00:00, 32.30it/s]Capturing num tokens (num_tokens=288 avail_mem=118.93 GB):  53%|█████▎    | 31/58 [00:01<00:00, 32.30it/s]Capturing num tokens (num_tokens=288 avail_mem=118.93 GB):  62%|██████▏   | 36/58 [00:01<00:00, 35.81it/s]Capturing num tokens (num_tokens=256 avail_mem=118.93 GB):  62%|██████▏   | 36/58 [00:01<00:00, 35.81it/s]Capturing num tokens (num_tokens=240 avail_mem=118.93 GB):  62%|██████▏   | 36/58 [00:01<00:00, 35.81it/s]Capturing num tokens (num_tokens=224 avail_mem=118.92 GB):  62%|██████▏   | 36/58 [00:01<00:00, 35.81it/s]Capturing num tokens (num_tokens=208 avail_mem=118.92 GB):  62%|██████▏   | 36/58 [00:01<00:00, 35.81it/s]Capturing num tokens (num_tokens=192 avail_mem=118.92 GB):  62%|██████▏   | 36/58 [00:01<00:00, 35.81it/s]Capturing num tokens (num_tokens=192 avail_mem=118.92 GB):  71%|███████   | 41/58 [00:01<00:00, 38.59it/s]Capturing num tokens (num_tokens=176 avail_mem=118.92 GB):  71%|███████   | 41/58 [00:01<00:00, 38.59it/s]Capturing num tokens (num_tokens=160 avail_mem=118.91 GB):  71%|███████   | 41/58 [00:01<00:00, 38.59it/s]

    Capturing num tokens (num_tokens=144 avail_mem=118.91 GB):  71%|███████   | 41/58 [00:02<00:00, 38.59it/s]Capturing num tokens (num_tokens=128 avail_mem=118.91 GB):  71%|███████   | 41/58 [00:02<00:00, 38.59it/s]Capturing num tokens (num_tokens=112 avail_mem=118.90 GB):  71%|███████   | 41/58 [00:02<00:00, 38.59it/s]Capturing num tokens (num_tokens=112 avail_mem=118.90 GB):  79%|███████▉  | 46/58 [00:02<00:00, 40.36it/s]Capturing num tokens (num_tokens=96 avail_mem=118.90 GB):  79%|███████▉  | 46/58 [00:02<00:00, 40.36it/s] Capturing num tokens (num_tokens=80 avail_mem=118.90 GB):  79%|███████▉  | 46/58 [00:02<00:00, 40.36it/s]Capturing num tokens (num_tokens=64 avail_mem=118.89 GB):  79%|███████▉  | 46/58 [00:02<00:00, 40.36it/s]Capturing num tokens (num_tokens=48 avail_mem=118.89 GB):  79%|███████▉  | 46/58 [00:02<00:00, 40.36it/s]Capturing num tokens (num_tokens=32 avail_mem=118.88 GB):  79%|███████▉  | 46/58 [00:02<00:00, 40.36it/s]Capturing num tokens (num_tokens=32 avail_mem=118.88 GB):  88%|████████▊ | 51/58 [00:02<00:00, 40.83it/s]Capturing num tokens (num_tokens=28 avail_mem=118.88 GB):  88%|████████▊ | 51/58 [00:02<00:00, 40.83it/s]

    Capturing num tokens (num_tokens=24 avail_mem=118.88 GB):  88%|████████▊ | 51/58 [00:02<00:00, 40.83it/s]Capturing num tokens (num_tokens=20 avail_mem=118.87 GB):  88%|████████▊ | 51/58 [00:02<00:00, 40.83it/s]Capturing num tokens (num_tokens=16 avail_mem=118.87 GB):  88%|████████▊ | 51/58 [00:02<00:00, 40.83it/s]Capturing num tokens (num_tokens=12 avail_mem=118.87 GB):  88%|████████▊ | 51/58 [00:02<00:00, 40.83it/s]Capturing num tokens (num_tokens=12 avail_mem=118.87 GB):  97%|█████████▋| 56/58 [00:02<00:00, 42.37it/s]Capturing num tokens (num_tokens=8 avail_mem=118.87 GB):  97%|█████████▋| 56/58 [00:02<00:00, 42.37it/s] Capturing num tokens (num_tokens=4 avail_mem=118.86 GB):  97%|█████████▋| 56/58 [00:02<00:00, 42.37it/s]

    Capturing num tokens (num_tokens=4 avail_mem=118.86 GB): 100%|██████████| 58/58 [00:05<00:00,  9.78it/s]


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
    Generated text:  Lynn. I am the class president of our school. I like football and I am very good at it. I think playing football makes me healthy. I play football with my friends after school. My friend John is also a football player. He plays for a team. John and I are good friends. We are always on good terms with each other. We help each other and we enjoy football. Sports play an important part in our lives. They are good for our health and they help us to relax. How do Lynn and John feel about the following words: "All sports are good"? ____ A. They agree. B. They
    ===============================
    Prompt: The president of the United States is
    Generated text:  trying to decide whether to pardon a convicted criminal. The president is considering two options. In the first option, the president will announce a pardon and issue a certificate of pardon to the convicted criminal. In the second option, the president will issue a certificate of pardon to the convicted criminal, but will not announce a pardon. The president is concerned that announcing a pardon could make the convicted criminal feel that the president is not serious about their sentence. The president is also concerned that issuing a certificate of pardon could cause the convicted criminal to have a negative impression of the president. Which option should the president choose? To determine the best course of action for
    ===============================
    Prompt: The capital of France is
    Generated text: :
    A) Lyon
    B) Paris
    C) Marseille
    D) Nice
    
    The correct answer is B) Paris. Paris, the capital of France, is located in the center of the country and is the largest city in France by population. It is known for its iconic Eiffel Tower and is a major cultural and tourist center. Lyon, Marseille, and Nice are not capitals of France. Therefore, the correct answer is A) Lyon. Paris is the most appropriate choice among the given options.
    ===============================
    Prompt: The future of AI is
    Generated text:  in the air
    
    The future of AI is in the air
    
    Diana’s question: Are we on the brink of an AI revolution?
    
    If you’re curious about what the future holds for AI, your brain is wired to expect it to be in the air. Even when you think about AI in the past, the most common response is that it’s on its way.
    
    While AI has a long way to go, a study in the Proceedings of the National Academy of Sciences last year found that AI’s impact on society is likely to continue to increase at an exponential rate. It is likely to start to become widespread within the next 1


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


    Generated text:  [Name] and I'm a [Age] year old [Occupation]. I'm a [Type of Character] who has always been [Positive Trait or Characteristic]. I'm [Positive Trait or Characteristic] and I'm always [Positive Trait or Characteristic]. I'm [Positive Trait or Characteristic] and I'm always [Positive Trait or Characteristic]. I'm [Positive Trait or Characteristic] and I'm always [Positive Trait or Characteristic]. I'm [Positive Trait or Characteristic] and I'm always [Positive Trait or Characteristic]. I'm [Positive Trait or Characteristic] and I'm always
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French Academy of Sciences, and the French Quarter, a historic district with its own unique culture and cuisine. Paris is a bustling city with a rich history and a vibrant culture, making it a popular tourist destination. It is also known for its fashion industry, with many famous designers and boutiques located in the city. Overall, Paris is a city of contrasts and beauty, making it a must-visit destination for anyone interested in French culture and history. 
    
    This
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the development of the technology in the coming years. Here are some of the most likely trends that are expected to shape the development of AI in the coming years:
    
    1. Increased use of AI in healthcare: AI is already being used in healthcare to improve patient outcomes and reduce costs. As AI technology continues to improve, we can expect to see even more widespread use of AI in healthcare in the coming years.
    
    2. Increased use of AI in finance: AI is already being used in finance to improve fraud detection and risk management. As AI technology continues to improve, we can
    


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
    Generated text:  [Your Name]. I'm a [Your Profession] who is passionate about [Your Area of Expertise]. I love [Your Love Interest] and I strive to make everyone around me feel [Your Positive Emotion] during our interactions. I'm always up for a challenge and always looking for ways to improve [Your Skillset] so I can help you achieve your goals. And when it comes to [Your Area of Expertise], I'm always up for learning and growing. So if you're ready to meet a new friend or someone who wants to learn something new, I'm here to help. Call me [Your Contact Information
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, also known as “the city of light” or “the city of light and shadow,” and has a population of over 2 million people. The city is home to iconic landmarks such as the Eiffel Tower, the Louvre Museum, and Notre-Dame Cathedral. It is also known for its rich history and vibrant culture, including its cultural scene, arts, and food scene. Paris is a bustling city with a diverse population and a vibrant economy. It is a popular tourist destination and a symbol of French culture and identity.
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  expected to be a mix of rapid development, innovation, and complexity. Here are some possible trends in AI:
    
    1. Augmented Intelligence: AI systems will become more capable of performing tasks like language understanding, decision-making, and creativity. This will lead to more natural language processing and virtual assistants.
    
    2. Enhanced Perception: AI will become more capable of detecting patterns and emotions in natural speech and gestures. This will allow for more intuitive and natural language interactions.
    
    3. Improved Accuracy: AI will improve its ability to recognize patterns and detect anomalies in data. This will lead to more accurate predictions and better decision-making.
    
    4. Personalization: AI


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

    ].

     I

     am

     a

     [

    job

     title

    ]

     at

     [

    company

    ].

     I

     am

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

    .

     How

     can

     I

     assist

     you

     today

    ?
    


    I

     look

     forward

     to

     our

     conversation

    .

     [

    name

    ]

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     the

     City

     of

     Light

    ,

     a

     vibrant

     and

     historic

     city

     that

     is

     home

     to

     millions

     of

     people

     and

     plays

     a

     key

     role

     in

     French

     culture

     and

     society

    .

     Paris

     is

     known

     for

     its

     beautiful

     architecture

    ,

     rich

     history

    ,

     and

     annual

     festivals

     such

     as

     the

     E

    iff

    el

     Tower

     celebration

     and

     the

     M

    ardi

     Gr

    as

     festival

    .

     It

     is

     also

     home

     to

     a

     large

     European

     community

     and

     is

     a

     center

     of

     international

     commerce

     and

     tourism

    .

     Paris

     has

     a

     rich

     cultural

     heritage

    ,

     including

     art

    ,

     music

    ,

     literature

    ,

     and

     film

    ,

     and

     is

     a

     popular

     destination

     for

     tourists

     and

     artists

    .

     Its

     population

     has

     increased

     significantly

     over

     the

     years

    ,

     and

     Paris

     remains

     a

     major

     global

     city

    ,

     with

     Paris

    ian

     cuisine

    ,

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     see

     significant

     advancements

     in

     areas

     such

     as

    :
    


    1

    .

     Increasing

    ly

     sophisticated

     natural

     language

     processing

     and

     machine

     learning

     algorithms

    
    


    2

    .

     Improved

     accuracy

     of

     autonomous

     vehicles

     and

     other

     automated

     systems

    
    


    3

    .

     Increased

     integration

     with

     other

     industries

     and

     applications

     such

     as

     healthcare

    ,

     finance

    ,

     and

     entertainment

    
    


    4

    .

     Enhanced

     security

     and

     privacy

     concerns

     around

     AI

     systems

    
    


    5

    .

     Greater

     reliance

     on

     AI

     for

     decision

    -making

     and

     problem

    -solving

     in

     complex

     systems

    
    


    6

    .

     Increased

     integration

     with

     human

     emotions

     and

     social

     behaviors

    
    


    7

    .

     Greater

     use

     of

     AI

     in

     the

     creation

     of

     content

     and

     entertainment

    
    


    8

    .

     Continued

     development

     of

     AI

     ethics

     and

     responsible

     use

    
    


    9

    .

     Increased

     investment

     in

     AI

     research

     and

     development

    
    


    1

    0

    



```python
llm.shutdown()
```
