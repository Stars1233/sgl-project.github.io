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


    2026-03-27 07:45:26.473 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 07:45:26] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 07:45:26.473 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 07:45:26] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 07:45:26.473 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 07:45:26] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 07:45:26.473 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 07:45:26] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 07:45:26.474 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 07:45:26] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.49it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.48it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:14,  2.36s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:14,  2.36s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:14,  2.36s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:02<00:36,  1.49it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:02<00:36,  1.49it/s]Compiling num tokens (num_tokens=6144):   5%|▌         | 3/58 [00:02<00:36,  1.49it/s]

    Compiling num tokens (num_tokens=5632):   5%|▌         | 3/58 [00:02<00:36,  1.49it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:02<00:14,  3.53it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:02<00:14,  3.53it/s]Compiling num tokens (num_tokens=4608):  10%|█         | 6/58 [00:02<00:14,  3.53it/s]Compiling num tokens (num_tokens=4096):  10%|█         | 6/58 [00:02<00:14,  3.53it/s]Compiling num tokens (num_tokens=3840):  10%|█         | 6/58 [00:02<00:14,  3.53it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.81it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.81it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.81it/s]

    Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.81it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.81it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.81it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:02<00:03, 11.74it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:02<00:03, 11.74it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:02<00:03, 11.74it/s]Compiling num tokens (num_tokens=1792):  26%|██▌       | 15/58 [00:02<00:03, 11.74it/s]Compiling num tokens (num_tokens=1536):  26%|██▌       | 15/58 [00:02<00:03, 11.74it/s]Compiling num tokens (num_tokens=1280):  26%|██▌       | 15/58 [00:02<00:03, 11.74it/s]Compiling num tokens (num_tokens=1024):  26%|██▌       | 15/58 [00:02<00:03, 11.74it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:02<00:02, 18.35it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:02<00:02, 18.35it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:02<00:02, 18.35it/s]

    Compiling num tokens (num_tokens=832):  36%|███▌      | 21/58 [00:03<00:02, 18.35it/s]Compiling num tokens (num_tokens=768):  36%|███▌      | 21/58 [00:03<00:02, 18.35it/s]Compiling num tokens (num_tokens=704):  36%|███▌      | 21/58 [00:03<00:02, 18.35it/s]Compiling num tokens (num_tokens=640):  36%|███▌      | 21/58 [00:03<00:02, 18.35it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:03<00:01, 25.06it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:03<00:01, 25.06it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:03<00:01, 25.06it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:03<00:01, 25.06it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:03<00:01, 25.06it/s]Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:03<00:01, 25.06it/s]Compiling num tokens (num_tokens=384):  47%|████▋     | 27/58 [00:03<00:01, 25.06it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:00, 31.69it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:00, 31.69it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:00, 31.69it/s]

    Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:00, 31.69it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:00, 31.69it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:00, 31.69it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 35.37it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 35.37it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 35.37it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 35.37it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 35.37it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 35.37it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 35.37it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 40.40it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 40.40it/s]

    Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 40.40it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 40.40it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 40.40it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 40.40it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 40.40it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 42.04it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 42.04it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 42.04it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 42.04it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 42.04it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 42.04it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 42.04it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 42.04it/s] 

    Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 42.04it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 50.80it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 16.02it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.42 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.42 GB):   2%|▏         | 1/58 [00:00<00:05,  9.63it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.44 GB):   2%|▏         | 1/58 [00:00<00:05,  9.63it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.44 GB):   2%|▏         | 1/58 [00:00<00:05,  9.63it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=118.44 GB):   5%|▌         | 3/58 [00:00<00:04, 11.54it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.46 GB):   5%|▌         | 3/58 [00:00<00:04, 11.54it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.48 GB):   5%|▌         | 3/58 [00:00<00:04, 11.54it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.48 GB):   9%|▊         | 5/58 [00:00<00:04, 13.11it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.48 GB):   9%|▊         | 5/58 [00:00<00:04, 13.11it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.50 GB):   9%|▊         | 5/58 [00:00<00:04, 13.11it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=118.50 GB):  12%|█▏        | 7/58 [00:00<00:03, 14.87it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.51 GB):  12%|█▏        | 7/58 [00:00<00:03, 14.87it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.52 GB):  12%|█▏        | 7/58 [00:00<00:03, 14.87it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.53 GB):  12%|█▏        | 7/58 [00:00<00:03, 14.87it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.53 GB):  17%|█▋        | 10/58 [00:00<00:02, 17.99it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.53 GB):  17%|█▋        | 10/58 [00:00<00:02, 17.99it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.53 GB):  17%|█▋        | 10/58 [00:00<00:02, 17.99it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=118.53 GB):  17%|█▋        | 10/58 [00:00<00:02, 17.99it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.53 GB):  22%|██▏       | 13/58 [00:00<00:02, 20.90it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.54 GB):  22%|██▏       | 13/58 [00:00<00:02, 20.90it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.53 GB):  22%|██▏       | 13/58 [00:00<00:02, 20.90it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.54 GB):  22%|██▏       | 13/58 [00:00<00:02, 20.90it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.54 GB):  28%|██▊       | 16/58 [00:00<00:01, 23.36it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.56 GB):  28%|██▊       | 16/58 [00:00<00:01, 23.36it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.55 GB):  28%|██▊       | 16/58 [00:00<00:01, 23.36it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.56 GB):  28%|██▊       | 16/58 [00:00<00:01, 23.36it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=118.55 GB):  28%|██▊       | 16/58 [00:00<00:01, 23.36it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.55 GB):  34%|███▍      | 20/58 [00:00<00:01, 26.42it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.55 GB):  34%|███▍      | 20/58 [00:00<00:01, 26.42it/s]Capturing num tokens (num_tokens=960 avail_mem=118.56 GB):  34%|███▍      | 20/58 [00:00<00:01, 26.42it/s] Capturing num tokens (num_tokens=896 avail_mem=118.56 GB):  34%|███▍      | 20/58 [00:01<00:01, 26.42it/s]Capturing num tokens (num_tokens=832 avail_mem=118.55 GB):  34%|███▍      | 20/58 [00:01<00:01, 26.42it/s]Capturing num tokens (num_tokens=832 avail_mem=118.55 GB):  41%|████▏     | 24/58 [00:01<00:01, 29.98it/s]Capturing num tokens (num_tokens=768 avail_mem=118.54 GB):  41%|████▏     | 24/58 [00:01<00:01, 29.98it/s]Capturing num tokens (num_tokens=704 avail_mem=118.53 GB):  41%|████▏     | 24/58 [00:01<00:01, 29.98it/s]Capturing num tokens (num_tokens=640 avail_mem=118.55 GB):  41%|████▏     | 24/58 [00:01<00:01, 29.98it/s]

    Capturing num tokens (num_tokens=576 avail_mem=118.56 GB):  41%|████▏     | 24/58 [00:01<00:01, 29.98it/s]Capturing num tokens (num_tokens=576 avail_mem=118.56 GB):  48%|████▊     | 28/58 [00:01<00:00, 32.30it/s]Capturing num tokens (num_tokens=512 avail_mem=118.54 GB):  48%|████▊     | 28/58 [00:01<00:00, 32.30it/s]Capturing num tokens (num_tokens=480 avail_mem=118.55 GB):  48%|████▊     | 28/58 [00:01<00:00, 32.30it/s]Capturing num tokens (num_tokens=448 avail_mem=118.55 GB):  48%|████▊     | 28/58 [00:01<00:00, 32.30it/s]Capturing num tokens (num_tokens=416 avail_mem=118.54 GB):  48%|████▊     | 28/58 [00:01<00:00, 32.30it/s]Capturing num tokens (num_tokens=416 avail_mem=118.54 GB):  55%|█████▌    | 32/58 [00:01<00:00, 33.67it/s]Capturing num tokens (num_tokens=384 avail_mem=118.54 GB):  55%|█████▌    | 32/58 [00:01<00:00, 33.67it/s]Capturing num tokens (num_tokens=352 avail_mem=118.53 GB):  55%|█████▌    | 32/58 [00:01<00:00, 33.67it/s]Capturing num tokens (num_tokens=320 avail_mem=118.52 GB):  55%|█████▌    | 32/58 [00:01<00:00, 33.67it/s]

    Capturing num tokens (num_tokens=288 avail_mem=118.52 GB):  55%|█████▌    | 32/58 [00:01<00:00, 33.67it/s]Capturing num tokens (num_tokens=288 avail_mem=118.52 GB):  62%|██████▏   | 36/58 [00:01<00:00, 34.60it/s]Capturing num tokens (num_tokens=256 avail_mem=118.51 GB):  62%|██████▏   | 36/58 [00:01<00:00, 34.60it/s]Capturing num tokens (num_tokens=240 avail_mem=118.51 GB):  62%|██████▏   | 36/58 [00:01<00:00, 34.60it/s]Capturing num tokens (num_tokens=224 avail_mem=118.50 GB):  62%|██████▏   | 36/58 [00:01<00:00, 34.60it/s]Capturing num tokens (num_tokens=208 avail_mem=118.49 GB):  62%|██████▏   | 36/58 [00:01<00:00, 34.60it/s]Capturing num tokens (num_tokens=208 avail_mem=118.49 GB):  69%|██████▉   | 40/58 [00:01<00:00, 35.03it/s]Capturing num tokens (num_tokens=192 avail_mem=118.49 GB):  69%|██████▉   | 40/58 [00:01<00:00, 35.03it/s]Capturing num tokens (num_tokens=176 avail_mem=118.48 GB):  69%|██████▉   | 40/58 [00:01<00:00, 35.03it/s]Capturing num tokens (num_tokens=160 avail_mem=118.47 GB):  69%|██████▉   | 40/58 [00:01<00:00, 35.03it/s]

    Capturing num tokens (num_tokens=144 avail_mem=118.47 GB):  69%|██████▉   | 40/58 [00:01<00:00, 35.03it/s]Capturing num tokens (num_tokens=144 avail_mem=118.47 GB):  76%|███████▌  | 44/58 [00:01<00:00, 35.75it/s]Capturing num tokens (num_tokens=128 avail_mem=118.46 GB):  76%|███████▌  | 44/58 [00:01<00:00, 35.75it/s]Capturing num tokens (num_tokens=112 avail_mem=118.45 GB):  76%|███████▌  | 44/58 [00:01<00:00, 35.75it/s]Capturing num tokens (num_tokens=96 avail_mem=118.44 GB):  76%|███████▌  | 44/58 [00:01<00:00, 35.75it/s] Capturing num tokens (num_tokens=80 avail_mem=118.44 GB):  76%|███████▌  | 44/58 [00:01<00:00, 35.75it/s]Capturing num tokens (num_tokens=80 avail_mem=118.44 GB):  83%|████████▎ | 48/58 [00:01<00:00, 36.13it/s]Capturing num tokens (num_tokens=64 avail_mem=118.44 GB):  83%|████████▎ | 48/58 [00:01<00:00, 36.13it/s]Capturing num tokens (num_tokens=48 avail_mem=118.43 GB):  83%|████████▎ | 48/58 [00:01<00:00, 36.13it/s]Capturing num tokens (num_tokens=32 avail_mem=118.42 GB):  83%|████████▎ | 48/58 [00:01<00:00, 36.13it/s]

    Capturing num tokens (num_tokens=28 avail_mem=118.41 GB):  83%|████████▎ | 48/58 [00:01<00:00, 36.13it/s]Capturing num tokens (num_tokens=28 avail_mem=118.41 GB):  90%|████████▉ | 52/58 [00:01<00:00, 37.00it/s]Capturing num tokens (num_tokens=24 avail_mem=118.40 GB):  90%|████████▉ | 52/58 [00:01<00:00, 37.00it/s]Capturing num tokens (num_tokens=20 avail_mem=118.40 GB):  90%|████████▉ | 52/58 [00:01<00:00, 37.00it/s]Capturing num tokens (num_tokens=16 avail_mem=118.39 GB):  90%|████████▉ | 52/58 [00:01<00:00, 37.00it/s]Capturing num tokens (num_tokens=12 avail_mem=118.38 GB):  90%|████████▉ | 52/58 [00:01<00:00, 37.00it/s]Capturing num tokens (num_tokens=12 avail_mem=118.38 GB):  97%|█████████▋| 56/58 [00:01<00:00, 32.98it/s]Capturing num tokens (num_tokens=8 avail_mem=118.38 GB):  97%|█████████▋| 56/58 [00:01<00:00, 32.98it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=118.37 GB):  97%|█████████▋| 56/58 [00:02<00:00, 32.98it/s]Capturing num tokens (num_tokens=4 avail_mem=118.37 GB): 100%|██████████| 58/58 [00:02<00:00, 28.52it/s]


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
    Generated text:  Tom Smith, a seasoned software engineer with over 10 years of experience in web development. I'm passionate about designing clean, efficient, and scalable web applications. My skills include HTML, CSS, JavaScript, and RESTful APIs. My current projects include building a simple e-commerce website and a mobile app for mobile payments. I'm also an active member of the open-source community, contributing to a variety of projects. Outside of work, I enjoy hiking, reading books, and playing basketball. I'm excited to see what the future holds and what new technologies will revolutionize web development. Can you provide some information on the latest web development
    ===============================
    Prompt: The president of the United States is
    Generated text:  a public official holding the title of President of the United States, and serves as the head of government of the United States. The term of the President of the United States is four years. How do you spell the title of the president of the United States? The correct spelling of the title of the president of the United States is "President of the United States." The title is spelled using the standard English spelling rules. No additional letters or special characters are required for this spelling. The title "President of the United States" is commonly used and widely recognized in English-speaking countries. Here is a summary of the other options:
    
    - "President
    ===============================
    Prompt: The capital of France is
    Generated text:  ______. A. Paris B. Lyon C. Marseille D. London
    Answer:
    A
    
    The place where the Spring Festival Gala was broadcast in 2011 was in ___.
    A. Beijing
    B. Shanghai
    C. Guangzhou
    D. Nanjing
    Answer:
    B
    
    The place where the Spring Festival Gala was broadcast in 2011 was in ______. A. Beijing B. Shanghai C. Guangzhou D. Nanjing
    Answer:
    B
    
    The place where the Spring Festival Gala was broadcast in 2011 was in ______.
    A. Beijing
    B. Shanghai
    C. Guang
    ===============================
    Prompt: The future of AI is
    Generated text:  good for us and for the economy. It can make technology accessible to all, from everyone with a smartphone or computer, to those who are struggling to afford the latest consumer electronics.
    The key is how we bring the world of AI to every person with an eye on the future. That is why IBM and the Future of Life Institute are working together to create a technology ecosystem that empowers everyone. We're focused on building a community of passionate individuals from all walks of life who are passionate about what's next and committed to finding new ways to make the world a better place.
    The Future of Life Institute is a 501(c)(


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm a [job title] with [number of years] years of experience in [industry]. I'm passionate about [reason why you're passionate about your job], and I'm always looking for ways to [what you're looking to improve]. I'm a [type of person] who is [what you do best]. I'm always looking for ways to [what you're looking to improve]. I'm a [type of person] who is [what you do best]. I'm always looking for ways to [what you're looking to improve].
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris. It is the largest city in Europe and the third largest city in the world by population. Paris is known for its iconic landmarks such as the Eiffel Tower, Louvre Museum, Notre-Dame Cathedral, and the Arc de Triomphe. The city is also famous for its rich history, including the French Revolution and the French Revolution Museum. Paris is a cultural and economic hub of France and a major tourist destination. It is home to many world-renowned museums, theaters, and restaurants. The city is also known for its fashion industry, with many famous fashion designers and boutiques. Paris is a city of contrasts,
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by rapid advancements in several key areas, including:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing for more sophisticated and nuanced decision-making. This could lead to more personalized and context-aware AI systems that can better understand and respond to the needs of individuals.
    
    2. Enhanced machine learning capabilities: AI is likely to become even more powerful and capable, with the ability to learn from vast amounts of data and adapt to new situations. This could lead to more efficient and effective AI systems that can perform a wider range of tasks.
    
    3. Increased focus on ethical and social implications:
    


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
    Generated text:  [Name], and I'm an [Occupation] with a [Duration] of [Years] in the industry. I'm passionate about [What I like to do], and I'm always looking for ways to [What I want to achieve] in my career. What's your name, and what's your occupation? [Name] [Name] [Name] [Name] [Name] [Name] [Name] [Name] [Name] [Name] [Name] [Name] [Name] [Name] [Name]
    A self-introduction in the fictional genre typically focuses on sharing personal information, skills, and
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    
    The statement is: "Paris is the capital of France." 
    
    This is a factual statement about the capital city of France. It provides a clear and concise description of the capital's location and identity. 
    
    Please let me know if you need any other information about Paris or its significance in French culture and politics. 
    
    If you have any questions related to the capital city, please feel free to ask. I'm here to assist you. Let me know if you would like any other information about Paris. 
    
    Please go ahead and ask me about Paris, and I'll do my best to provide you with relevant and accurate information. 
    
    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be characterized by the rapid development and widespread adoption of machine learning and deep learning algorithms. AI systems will continue to improve their ability to process and analyze large amounts of data, allowing them to better recognize patterns, make predictions, and generate insights. Additionally, AI will be increasingly integrated into various industries, including healthcare, finance, and transportation, as well as into human interactions and interactions with the physical world.
    However, there are also potential challenges and risks associated with the rapid development and widespread adoption of AI, including concerns about job displacement, ethical issues, and the potential for AI systems to be hacked or manipulated. As AI continues to advance


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

     am

     a

    /an

     [

    age

    ]

     year

     old

     [

    national

    ity

    ]

     [

    occupation

    ].

     I

     have

     a

    /an

     [

    job

     title

    ]

     at

     [

    company

     name

    ].

     I

     am

     passionate

     about

     [

    reason

     for

     passion

    ],

     and

     I

     am

     constantly

     striving

     to

     [

    describe

     a

     specific

     goal

     or

     achievement

    ].

     I

     am

     always

     looking

     for

     ways

     to

     [

    provide

     a

     positive

     outlook

     or

     engage

     with

     others

     in

     a

     way

     that

     inspires

     others

    ].

     I

     am

     also

     [

    add

     a

     personal

     statement

     or

     statement

     about

     myself

     that

     reflects

     who

     I

     am

     as

     a

     person

    ].

     Thank

     you

     for

     having

     me

    .

     [

    Name

    ]

     (

    2

    )

     
    


    PS

    .

     I

    'm

     sorry

     to

     bother

     you

    ,

     but

     I

     didn

    't

     expect

     you

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     which

     is

     known

     as

     the

     "

    City

     of

     Love

    "

     due

     to

     its

     romantic

     charm

     and

     traditional

     charm

    .

     It

     is

     located

     in

     the

     Lo

    ire

     Valley

     on

     the

     banks

     of

     the

     River

     Se

    ine

    .

     The

     city

     is

     home

     to

     iconic

     landmarks

     such

     as

     Notre

    -D

    ame

     Cathedral

    ,

     the

     E

    iff

    el

     Tower

    ,

     and

     the

     Lou

    vre

     Museum

    ,

     as

     well

     as

     a

     diverse

     and

     vibrant

     cultural

     scene

    .

     Paris

     is

     a

     popular

     tourist

     destination

     for

     its

     rich

     history

    ,

     beautiful

     architecture

    ,

     and

     annual

     Paris

    ian

     festivals

     and

     events

    .

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     certainly

     uncertain

    ,

     but

     here

     are

     some

     potential

     trends

     that

     could

     shape

     the

     technology

     in

     the

     coming

     years

    :
    


    1

    .

     Improved

     machine

     learning

     and

     natural

     language

     processing

     will

     enable

     AI

     systems

     to

     perform

     more

     complex

     tasks

     and

     understand

     more

     complex

     languages

    .
    


    2

    .

     AI

     will

     continue

     to

     be

     used

     in

     manufacturing

     to

     optimize

     production

     processes

     and

     increase

     efficiency

    .
    


    3

    .

     AI

     will

     become

     more

     integrated

     into

     everyday

     life

    ,

     from

     virtual

     assistants

     to

     smart

     home

     devices

    .
    


    4

    .

     AI

     will

     be

     used

     to

     improve

     healthcare

     by

     enabling

     better

     diagnosis

     and

     personalized

     treatment

     plans

    .
    


    5

    .

     AI

     will

     be

     used

     in

     financial

     services

     to

     help

     automate

     trading

    ,

     risk

     management

    ,

     and

     fraud

     detection

    .
    


    6

    .

     AI

     will

     be

     used

     to

     develop

     new

    



```python
llm.shutdown()
```
