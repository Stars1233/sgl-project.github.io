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


    2026-03-26 23:38:41.860 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 23:38:41] Persistent cache disabled, using in-memory JIT cache
    2026-03-26 23:38:41.860 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 23:38:41] Persistent cache disabled, using in-memory JIT cache
    2026-03-26 23:38:41.860 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 23:38:41] Persistent cache disabled, using in-memory JIT cache
    2026-03-26 23:38:41.860 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 23:38:41] Persistent cache disabled, using in-memory JIT cache
    2026-03-26 23:38:41.860 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 23:38:41] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.34it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.33it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:14,  2.37s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:14,  2.37s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<00:58,  1.04s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<00:58,  1.04s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<00:58,  1.04s/it]Compiling num tokens (num_tokens=6144):   3%|▎         | 2/58 [00:02<00:58,  1.04s/it]Compiling num tokens (num_tokens=5632):   3%|▎         | 2/58 [00:02<00:58,  1.04s/it]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:02<00:13,  3.87it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:02<00:13,  3.87it/s]Compiling num tokens (num_tokens=4608):  10%|█         | 6/58 [00:02<00:13,  3.87it/s]Compiling num tokens (num_tokens=4096):  10%|█         | 6/58 [00:02<00:13,  3.87it/s]Compiling num tokens (num_tokens=3840):  10%|█         | 6/58 [00:02<00:13,  3.87it/s]Compiling num tokens (num_tokens=3584):  10%|█         | 6/58 [00:02<00:13,  3.87it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:05,  8.30it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:05,  8.30it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:05,  8.30it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:05,  8.30it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:05,  8.30it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:02<00:03, 12.11it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:02<00:03, 12.11it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:02<00:03, 12.11it/s]Compiling num tokens (num_tokens=1792):  26%|██▌       | 15/58 [00:02<00:03, 12.11it/s]Compiling num tokens (num_tokens=1536):  26%|██▌       | 15/58 [00:02<00:03, 12.11it/s]Compiling num tokens (num_tokens=1280):  26%|██▌       | 15/58 [00:02<00:03, 12.11it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:02<00:02, 17.25it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:02<00:02, 17.25it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:02<00:02, 17.25it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:02<00:02, 17.25it/s]Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:02<00:02, 17.25it/s]Compiling num tokens (num_tokens=768):  34%|███▍      | 20/58 [00:02<00:02, 17.25it/s]

    Compiling num tokens (num_tokens=704):  34%|███▍      | 20/58 [00:02<00:02, 17.25it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 23.79it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 23.79it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 23.79it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 23.79it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 23.79it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 23.79it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:03<00:00, 28.77it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:03<00:00, 28.77it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:03<00:00, 28.77it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:03<00:00, 28.77it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:03<00:00, 28.77it/s]Compiling num tokens (num_tokens=288):  53%|█████▎    | 31/58 [00:03<00:00, 28.77it/s]

    Compiling num tokens (num_tokens=256):  53%|█████▎    | 31/58 [00:03<00:00, 28.77it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:03<00:00, 33.83it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:03<00:00, 33.83it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:03<00:00, 33.83it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:03<00:00, 33.83it/s]Compiling num tokens (num_tokens=192):  64%|██████▍   | 37/58 [00:03<00:00, 33.83it/s]Compiling num tokens (num_tokens=176):  64%|██████▍   | 37/58 [00:03<00:00, 33.83it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:03<00:00, 37.14it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:03<00:00, 37.14it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:03<00:00, 37.14it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:03<00:00, 37.14it/s]Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:03<00:00, 37.14it/s]

    Compiling num tokens (num_tokens=96):  72%|███████▏  | 42/58 [00:03<00:00, 37.14it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:03<00:00, 39.86it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:03<00:00, 39.86it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:03<00:00, 39.86it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:03<00:00, 39.86it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:03<00:00, 39.86it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:03<00:00, 39.86it/s]Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:03<00:00, 39.86it/s]Compiling num tokens (num_tokens=20):  81%|████████  | 47/58 [00:03<00:00, 39.86it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:03<00:00, 46.56it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:03<00:00, 46.56it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:03<00:00, 46.56it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:03<00:00, 46.56it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:03<00:00, 46.56it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 16.11it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.87 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.87 GB):   2%|▏         | 1/58 [00:00<00:06,  9.14it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.84 GB):   2%|▏         | 1/58 [00:00<00:06,  9.14it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=118.84 GB):   3%|▎         | 2/58 [00:00<00:05,  9.59it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.84 GB):   3%|▎         | 2/58 [00:00<00:05,  9.59it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.84 GB):   3%|▎         | 2/58 [00:00<00:05,  9.59it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.84 GB):   7%|▋         | 4/58 [00:00<00:05, 10.61it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.84 GB):   7%|▋         | 4/58 [00:00<00:05, 10.61it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=118.83 GB):   7%|▋         | 4/58 [00:00<00:05, 10.61it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.83 GB):  10%|█         | 6/58 [00:00<00:04, 11.81it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.83 GB):  10%|█         | 6/58 [00:00<00:04, 11.81it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.83 GB):  10%|█         | 6/58 [00:00<00:04, 11.81it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.83 GB):  10%|█         | 6/58 [00:00<00:04, 11.81it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.83 GB):  16%|█▌        | 9/58 [00:00<00:03, 15.86it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.83 GB):  16%|█▌        | 9/58 [00:00<00:03, 15.86it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=118.82 GB):  16%|█▌        | 9/58 [00:00<00:03, 15.86it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.82 GB):  16%|█▌        | 9/58 [00:00<00:03, 15.86it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.82 GB):  21%|██        | 12/58 [00:00<00:02, 18.56it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.81 GB):  21%|██        | 12/58 [00:00<00:02, 18.56it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.81 GB):  21%|██        | 12/58 [00:00<00:02, 18.56it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.81 GB):  21%|██        | 12/58 [00:00<00:02, 18.56it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.81 GB):  21%|██        | 12/58 [00:00<00:02, 18.56it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.80 GB):  21%|██        | 12/58 [00:00<00:02, 18.56it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=118.80 GB):  29%|██▉       | 17/58 [00:00<00:01, 25.49it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.64 GB):  29%|██▉       | 17/58 [00:00<00:01, 25.49it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.64 GB):  29%|██▉       | 17/58 [00:00<00:01, 25.49it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.63 GB):  29%|██▉       | 17/58 [00:00<00:01, 25.49it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.61 GB):  29%|██▉       | 17/58 [00:00<00:01, 25.49it/s]Capturing num tokens (num_tokens=960 avail_mem=118.62 GB):  29%|██▉       | 17/58 [00:00<00:01, 25.49it/s] Capturing num tokens (num_tokens=960 avail_mem=118.62 GB):  38%|███▊      | 22/58 [00:01<00:01, 30.88it/s]Capturing num tokens (num_tokens=896 avail_mem=118.62 GB):  38%|███▊      | 22/58 [00:01<00:01, 30.88it/s]Capturing num tokens (num_tokens=832 avail_mem=118.62 GB):  38%|███▊      | 22/58 [00:01<00:01, 30.88it/s]Capturing num tokens (num_tokens=768 avail_mem=118.61 GB):  38%|███▊      | 22/58 [00:01<00:01, 30.88it/s]Capturing num tokens (num_tokens=704 avail_mem=118.61 GB):  38%|███▊      | 22/58 [00:01<00:01, 30.88it/s]

    Capturing num tokens (num_tokens=640 avail_mem=118.61 GB):  38%|███▊      | 22/58 [00:01<00:01, 30.88it/s]Capturing num tokens (num_tokens=640 avail_mem=118.61 GB):  47%|████▋     | 27/58 [00:01<00:00, 35.02it/s]Capturing num tokens (num_tokens=576 avail_mem=118.61 GB):  47%|████▋     | 27/58 [00:01<00:00, 35.02it/s]Capturing num tokens (num_tokens=512 avail_mem=118.60 GB):  47%|████▋     | 27/58 [00:01<00:00, 35.02it/s]Capturing num tokens (num_tokens=480 avail_mem=118.61 GB):  47%|████▋     | 27/58 [00:01<00:00, 35.02it/s]Capturing num tokens (num_tokens=448 avail_mem=118.61 GB):  47%|████▋     | 27/58 [00:01<00:00, 35.02it/s]Capturing num tokens (num_tokens=416 avail_mem=118.61 GB):  47%|████▋     | 27/58 [00:01<00:00, 35.02it/s]Capturing num tokens (num_tokens=416 avail_mem=118.61 GB):  55%|█████▌    | 32/58 [00:01<00:00, 37.76it/s]Capturing num tokens (num_tokens=384 avail_mem=118.39 GB):  55%|█████▌    | 32/58 [00:01<00:00, 37.76it/s]Capturing num tokens (num_tokens=352 avail_mem=118.13 GB):  55%|█████▌    | 32/58 [00:01<00:00, 37.76it/s]

    Capturing num tokens (num_tokens=320 avail_mem=118.13 GB):  55%|█████▌    | 32/58 [00:01<00:00, 37.76it/s]Capturing num tokens (num_tokens=288 avail_mem=118.12 GB):  55%|█████▌    | 32/58 [00:01<00:00, 37.76it/s]Capturing num tokens (num_tokens=288 avail_mem=118.12 GB):  62%|██████▏   | 36/58 [00:01<00:00, 32.85it/s]Capturing num tokens (num_tokens=256 avail_mem=117.94 GB):  62%|██████▏   | 36/58 [00:01<00:00, 32.85it/s]Capturing num tokens (num_tokens=240 avail_mem=117.94 GB):  62%|██████▏   | 36/58 [00:01<00:00, 32.85it/s]Capturing num tokens (num_tokens=224 avail_mem=117.94 GB):  62%|██████▏   | 36/58 [00:01<00:00, 32.85it/s]Capturing num tokens (num_tokens=208 avail_mem=117.93 GB):  62%|██████▏   | 36/58 [00:01<00:00, 32.85it/s]

    Capturing num tokens (num_tokens=208 avail_mem=117.93 GB):  69%|██████▉   | 40/58 [00:01<00:00, 32.98it/s]Capturing num tokens (num_tokens=192 avail_mem=117.93 GB):  69%|██████▉   | 40/58 [00:01<00:00, 32.98it/s]Capturing num tokens (num_tokens=176 avail_mem=117.93 GB):  69%|██████▉   | 40/58 [00:01<00:00, 32.98it/s]Capturing num tokens (num_tokens=160 avail_mem=117.92 GB):  69%|██████▉   | 40/58 [00:01<00:00, 32.98it/s]Capturing num tokens (num_tokens=144 avail_mem=117.92 GB):  69%|██████▉   | 40/58 [00:01<00:00, 32.98it/s]Capturing num tokens (num_tokens=144 avail_mem=117.92 GB):  76%|███████▌  | 44/58 [00:01<00:00, 33.21it/s]Capturing num tokens (num_tokens=128 avail_mem=117.92 GB):  76%|███████▌  | 44/58 [00:01<00:00, 33.21it/s]Capturing num tokens (num_tokens=112 avail_mem=117.92 GB):  76%|███████▌  | 44/58 [00:01<00:00, 33.21it/s]Capturing num tokens (num_tokens=96 avail_mem=117.91 GB):  76%|███████▌  | 44/58 [00:01<00:00, 33.21it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=117.91 GB):  76%|███████▌  | 44/58 [00:01<00:00, 33.21it/s]Capturing num tokens (num_tokens=80 avail_mem=117.91 GB):  83%|████████▎ | 48/58 [00:01<00:00, 32.65it/s]Capturing num tokens (num_tokens=64 avail_mem=117.90 GB):  83%|████████▎ | 48/58 [00:01<00:00, 32.65it/s]Capturing num tokens (num_tokens=48 avail_mem=117.90 GB):  83%|████████▎ | 48/58 [00:01<00:00, 32.65it/s]Capturing num tokens (num_tokens=32 avail_mem=117.90 GB):  83%|████████▎ | 48/58 [00:01<00:00, 32.65it/s]Capturing num tokens (num_tokens=28 avail_mem=117.89 GB):  83%|████████▎ | 48/58 [00:01<00:00, 32.65it/s]Capturing num tokens (num_tokens=28 avail_mem=117.89 GB):  90%|████████▉ | 52/58 [00:01<00:00, 33.21it/s]Capturing num tokens (num_tokens=24 avail_mem=117.89 GB):  90%|████████▉ | 52/58 [00:01<00:00, 33.21it/s]Capturing num tokens (num_tokens=20 avail_mem=117.89 GB):  90%|████████▉ | 52/58 [00:01<00:00, 33.21it/s]Capturing num tokens (num_tokens=16 avail_mem=117.89 GB):  90%|████████▉ | 52/58 [00:01<00:00, 33.21it/s]

    Capturing num tokens (num_tokens=12 avail_mem=117.88 GB):  90%|████████▉ | 52/58 [00:01<00:00, 33.21it/s]Capturing num tokens (num_tokens=8 avail_mem=117.88 GB):  90%|████████▉ | 52/58 [00:01<00:00, 33.21it/s] Capturing num tokens (num_tokens=8 avail_mem=117.88 GB):  98%|█████████▊| 57/58 [00:02<00:00, 35.81it/s]Capturing num tokens (num_tokens=4 avail_mem=117.88 GB):  98%|█████████▊| 57/58 [00:02<00:00, 35.81it/s]Capturing num tokens (num_tokens=4 avail_mem=117.88 GB): 100%|██████████| 58/58 [00:02<00:00, 28.61it/s]


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
    Generated text:  Mina. I'm a seventh-grader from China. I'm an outgoing girl. I like playing basketball, swimming and cooking. My favorite food is pizza. My parents have two children. They are my mom and my dad. My mom is very kind and she cooks delicious food for me and my brother and me. My father is very strict. He is the head of the Chinese basketball team. He plays basketball well. When I go to school, I like to play computer games. I have a big book about my favorite sportsman. I like reading and listening to music. I like my brother and me. We are good
    ===============================
    Prompt: The president of the United States is
    Generated text:  trying to decide between two mutually exclusive projects. Project A requires an initial investment of $25 million and has a lifespan of 10 years. Project B requires an initial investment of $50 million and has a lifespan of 15 years. The present value of Project A is $500 million, and the present value of Project B is $400 million. Should the president choose to invest in Project A or Project B, based on the present value of the benefits compared to the initial investment?
    
    To determine whether the president should choose to invest in Project A or Project B based on the present value of the benefits
    ===============================
    Prompt: The capital of France is
    Generated text:  ____.
    A. London
    B. Paris
    C. New York
    D. Tokyo
    Answer: B
    
    A 7-year-old child who has just started school. Recently, the child has been experiencing frequent crying and restlessness. Physical examination reveals that there is a grade 3 systolic murmur in the apical area of the heart, with a loud and rough systolic click, and a grade 4/6 systolic ejection murmur at the apex. The most likely diagnosis for this child is ____. 
    A. Patent ductus arteriosus
    B. Ventricular septal defect
    C. A
    ===============================
    Prompt: The future of AI is
    Generated text:  changing the way that we all work. Whether we are building a robot that can help us with housework or building a robot that can help us with science experiments, the technology is here to make us work more efficiently, and we will see some truly amazing advancements in the future of AI. Here are some ideas for how AI will change the way we work and how we can use AI to our advantage.
    First, AI will enable us to work more efficiently. AI can be programmed to perform tasks that are typically done by humans, such as sorting emails, answering questions, and even translating languages. This will make our work more efficient, and


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [insert a short description of your job or profession]. I enjoy [insert a short description of your hobbies or interests]. What's your favorite hobby or activity? I love [insert a short description of your favorite activity]. What's your favorite book or movie? I love [insert a short description of your favorite book or movie]. What's your favorite place to go? I love [insert a short description of your favorite place]. What's your
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also the birthplace of French literature and a popular tourist destination. Paris is a cultural and historical center with a rich history dating back to the Roman Empire and the French Revolution. It is also known for its vibrant nightlife, fashion, and cuisine. Paris is a major transportation hub, with the Eiffel Tower serving as a landmark and the metro system serving as a major mode of public transportation. The city is also home to numerous museums, including the Louvre, the Musée d'
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction. Here are some of the most likely trends:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn and adapt to human behavior and preferences. This could lead to more personalized and efficient AI systems.
    
    2. Greater emphasis on ethical considerations: As AI becomes more integrated with human intelligence, there will be a greater emphasis on ethical considerations. This could lead to more stringent regulations and standards for AI development and deployment.
    
    3. Increased use of AI in healthcare: AI is already being used in healthcare
    


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
    Generated text:  [insert character's name]. I'm [insert character's age and gender] and I'm a [insert character's occupation] or professional. I'm [insert character's profession or occupation] and I have been working in this field for [insert number of years] years. I have [insert any relevant skills, experience, or achievements in this field]. I enjoy [insert any hobbies or interests related to this field], and I'm passionate about [insert any values or beliefs about this field that make me strong]. Overall, I am [insert any personality traits or qualities that make me unique and interesting]. What do you think of me
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    
    Fact: The capital of France, Paris, is known for its rich cultural heritage and iconic landmarks such as the Eiffel Tower, the Louvre Museum, Notre-Dame Cathedral, and Montmartre. The city is also known for its diverse population of millions of residents, which have contributed to its thriving economy. Paris has a long history dating back to the Roman Empire and is home to numerous museums, galleries, and theaters. The city is also home to many notable individuals, including Jeanne Calment, the world-record-breaking longevity, and Émile Zola. Overall, Paris is a vibrant and dynamic city that is
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  highly uncertain and unpredictable, but several trends are likely to shape it in the coming years.
    
    1. Increased focus on AI ethics: As more companies adopt AI, they will become more conscious of their impact on society and the environment. As a result, there will be increasing efforts to create AI that is ethical and responsible. This will involve creating guidelines for AI developers and policymakers to ensure that AI is developed and used in a way that is beneficial for society as a whole.
    
    2. Integration of AI with other technologies: In the coming years, we are likely to see a greater integration of AI with other technologies, such as物联网（IoT


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

     a

     [

    Age

    ]

     year

     old

     [

    Prof

    ession

    ]

     from

     [

    Location

    ].

     I

     have

     always

     been

     [

    Why

    ]

     and

     I

     enjoy

     [

    What

     I

     Do

     Best

    ].

     If

     you

     haven

    't

     heard

     of

     me

     before

    ,

     I

     am

     [

    What

     I

     Do

     Not

     Know

    ].

     My

     goal

     is

     to

     [

    What

     I

     Want

     to

     Achie

    ve

    ].

     I

     am

     confident

     in

     [

    Why

    ],

     and

     I

    'm

     excited

     to

     see

     what

     the

     future

     holds

     for

     me

    .

     


    [

    Professional

     self

    -int

    roduction

     (

    without

     me

    ,

     just

     a

     placeholder

     for

     the

     rest

     of

     the

     self

    -int

    roduction

    )]

    .


    Hello

    !

     My

     name

     is

     [

    Name

    ]

     and

     I

     am

     a

     [

    Age

    ]

     year

     old

     [

    Prof

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .
    


    That

    's

     correct

    !

     Paris

     is

     the

     capital

     city

     of

     France

     and

     is

     known

     for

     its

     iconic

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

     and

     many

     other

     landmarks

    .

     It

     is

     a

     major

     center

     for

     politics

    ,

     culture

    ,

     and

     industry

     and

     is

     often

     referred

     to

     as

     the

     "

    city

     of

     love

    "

     and

     "

    la

     ville

     du

     sper

    me

    "

     (

    the

     city

     of

     sperm

    ).

     Paris

     is

     one

     of

     the

     most

     visited

     cities

     in

     the

     world

     and

     is

     a

     popular

     tourist

     destination

    .

     
    


    Here

     are

     some

     additional

     facts

     about

     Paris

    :


    -

     The

     city

     is

     located

     on

     the

     Î

    le

     de

     France

    ,

     which

     is

     the

     largest

     island

     in

     France

    .


    -

     Paris

     is

     home

     to

     many

     of

     the

     world

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     expected

     to

     be

     characterized

     by

     several

     key

     trends

    ,

     including

    :
    


    1

    .

     Increased

     focus

     on

     ethical

     considerations

    :

     With

     increasing

     concerns

     about

     AI

    's

     impact

     on

     society

    ,

     there

     is

     a

     growing

     focus

     on

     ethical

     considerations

     such

     as

     bias

    ,

     transparency

    ,

     accountability

    ,

     and

     accountability

    .

     This

     includes

     the

     development

     of

     algorithms

     and

     systems

     that

     are

     more

     transparent

     and

     less

     prone

     to

     bias

    .
    


    2

    .

     Greater

     integration

     with

     other

     technologies

    :

     AI

     is

     already

     being

     integrated

     with

     other

     technologies

     such

     as

     sensors

    ,

     data

     analytics

    ,

     and

     machine

     learning

    ,

     and

     it

     is

     expected

     to

     continue

     to

     be

     integrated

     with

     these

     technologies

     in

     future

     developments

    .

     This

     integration

     is

     expected

     to

     make

     AI

     more

     efficient

     and

     effective

    .
    


    3

    .

     Growth

     in

     AI

    -powered

     self

    



```python
llm.shutdown()
```
