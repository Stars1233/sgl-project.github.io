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


    2026-04-02 20:04:11.094 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 20:04:11] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 20:04:11.094 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 20:04:11] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 20:04:11.094 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 20:04:11] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 20:04:11.094 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 20:04:11] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 20:04:11.094 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 20:04:11] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.76it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.76it/s]


    2026-04-02 20:04:15,347 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 20:04:15] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:26,  2.57s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:26,  2.57s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<01:03,  1.13s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<01:03,  1.13s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<01:03,  1.13s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:24,  2.17it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:24,  2.17it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:24,  2.17it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:24,  2.17it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:02<00:11,  4.46it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:02<00:11,  4.46it/s]Compiling num tokens (num_tokens=4096):  12%|█▏        | 7/58 [00:03<00:11,  4.46it/s]

    Compiling num tokens (num_tokens=3840):  12%|█▏        | 7/58 [00:03<00:11,  4.46it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:03<00:06,  7.07it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:03<00:06,  7.07it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:03<00:06,  7.07it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:03<00:06,  7.07it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:03<00:06,  7.07it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:03<00:04, 10.99it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:03<00:04, 10.99it/s]

    Compiling num tokens (num_tokens=2304):  24%|██▍       | 14/58 [00:03<00:04, 10.99it/s]Compiling num tokens (num_tokens=2048):  24%|██▍       | 14/58 [00:03<00:04, 10.99it/s]Compiling num tokens (num_tokens=1792):  24%|██▍       | 14/58 [00:03<00:04, 10.99it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:02, 15.30it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:02, 15.30it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:02, 15.30it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:02, 15.30it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:02, 15.30it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:03<00:01, 18.98it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:03<00:01, 18.98it/s]

    Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:03<00:01, 18.98it/s]Compiling num tokens (num_tokens=768):  38%|███▊      | 22/58 [00:03<00:01, 18.98it/s]Compiling num tokens (num_tokens=704):  38%|███▊      | 22/58 [00:03<00:01, 18.98it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 22.45it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 22.45it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 22.45it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 22.45it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 22.45it/s]

    Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:03<00:01, 24.71it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:03<00:01, 24.71it/s]Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:03<00:01, 24.71it/s]Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:03<00:01, 24.71it/s]Compiling num tokens (num_tokens=352):  52%|█████▏    | 30/58 [00:03<00:01, 24.71it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:03<00:00, 28.13it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:03<00:00, 28.13it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:03<00:00, 28.13it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:03<00:00, 28.13it/s]Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:03<00:00, 28.13it/s]

    Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 29.13it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 29.13it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 29.13it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 29.13it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 29.13it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:04<00:00, 31.36it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:04<00:00, 31.36it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:04<00:00, 31.36it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:04<00:00, 31.36it/s]

    Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:04<00:00, 31.36it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:04<00:00, 31.09it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:04<00:00, 31.09it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:04<00:00, 31.09it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:04<00:00, 31.09it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:04<00:00, 31.09it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:04<00:00, 32.08it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:04<00:00, 32.08it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:04<00:00, 32.08it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:04<00:00, 32.08it/s]

    Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:04<00:00, 32.08it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:04<00:00, 32.08it/s]Compiling num tokens (num_tokens=16):  95%|█████████▍| 55/58 [00:04<00:00, 35.69it/s]Compiling num tokens (num_tokens=12):  95%|█████████▍| 55/58 [00:04<00:00, 35.69it/s]Compiling num tokens (num_tokens=8):  95%|█████████▍| 55/58 [00:04<00:00, 35.69it/s] Compiling num tokens (num_tokens=4):  95%|█████████▍| 55/58 [00:04<00:00, 35.69it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 13.10it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=71.12 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=71.09 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=71.09 GB):   3%|▎         | 2/58 [00:00<00:02, 18.70it/s]Capturing num tokens (num_tokens=7168 avail_mem=71.09 GB):   3%|▎         | 2/58 [00:00<00:02, 18.70it/s]Capturing num tokens (num_tokens=6656 avail_mem=71.09 GB):   3%|▎         | 2/58 [00:00<00:02, 18.70it/s]Capturing num tokens (num_tokens=6144 avail_mem=71.09 GB):   3%|▎         | 2/58 [00:00<00:02, 18.70it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=71.09 GB):   9%|▊         | 5/58 [00:00<00:02, 22.68it/s]Capturing num tokens (num_tokens=5632 avail_mem=71.08 GB):   9%|▊         | 5/58 [00:00<00:02, 22.68it/s]Capturing num tokens (num_tokens=5120 avail_mem=71.09 GB):   9%|▊         | 5/58 [00:00<00:02, 22.68it/s]Capturing num tokens (num_tokens=4608 avail_mem=71.08 GB):   9%|▊         | 5/58 [00:00<00:02, 22.68it/s]Capturing num tokens (num_tokens=4096 avail_mem=71.08 GB):   9%|▊         | 5/58 [00:00<00:02, 22.68it/s]Capturing num tokens (num_tokens=4096 avail_mem=71.08 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.33it/s]Capturing num tokens (num_tokens=3840 avail_mem=71.08 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.33it/s]Capturing num tokens (num_tokens=3584 avail_mem=71.07 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.33it/s]Capturing num tokens (num_tokens=3328 avail_mem=71.07 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.33it/s]Capturing num tokens (num_tokens=3072 avail_mem=71.07 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.33it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=71.07 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.33it/s]Capturing num tokens (num_tokens=2816 avail_mem=71.07 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.86it/s]Capturing num tokens (num_tokens=2560 avail_mem=71.06 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.86it/s]Capturing num tokens (num_tokens=2304 avail_mem=71.06 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.86it/s]Capturing num tokens (num_tokens=2048 avail_mem=71.05 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.86it/s]Capturing num tokens (num_tokens=1792 avail_mem=71.05 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.86it/s]Capturing num tokens (num_tokens=1536 avail_mem=71.05 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.86it/s]Capturing num tokens (num_tokens=1280 avail_mem=71.04 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.86it/s]Capturing num tokens (num_tokens=1280 avail_mem=71.04 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.47it/s]Capturing num tokens (num_tokens=1024 avail_mem=71.02 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.47it/s]Capturing num tokens (num_tokens=960 avail_mem=71.04 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.47it/s] Capturing num tokens (num_tokens=896 avail_mem=71.03 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.47it/s]Capturing num tokens (num_tokens=832 avail_mem=71.03 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.47it/s]

    Capturing num tokens (num_tokens=768 avail_mem=71.03 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.47it/s]Capturing num tokens (num_tokens=704 avail_mem=71.02 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.47it/s]Capturing num tokens (num_tokens=704 avail_mem=71.02 GB):  45%|████▍     | 26/58 [00:00<00:00, 45.04it/s]Capturing num tokens (num_tokens=640 avail_mem=71.02 GB):  45%|████▍     | 26/58 [00:00<00:00, 45.04it/s]Capturing num tokens (num_tokens=576 avail_mem=71.02 GB):  45%|████▍     | 26/58 [00:00<00:00, 45.04it/s]Capturing num tokens (num_tokens=512 avail_mem=71.01 GB):  45%|████▍     | 26/58 [00:00<00:00, 45.04it/s]Capturing num tokens (num_tokens=480 avail_mem=71.02 GB):  45%|████▍     | 26/58 [00:00<00:00, 45.04it/s]Capturing num tokens (num_tokens=448 avail_mem=71.02 GB):  45%|████▍     | 26/58 [00:00<00:00, 45.04it/s]Capturing num tokens (num_tokens=416 avail_mem=71.02 GB):  45%|████▍     | 26/58 [00:00<00:00, 45.04it/s]Capturing num tokens (num_tokens=416 avail_mem=71.02 GB):  55%|█████▌    | 32/58 [00:00<00:00, 48.17it/s]Capturing num tokens (num_tokens=384 avail_mem=71.02 GB):  55%|█████▌    | 32/58 [00:00<00:00, 48.17it/s]Capturing num tokens (num_tokens=352 avail_mem=71.01 GB):  55%|█████▌    | 32/58 [00:00<00:00, 48.17it/s]Capturing num tokens (num_tokens=320 avail_mem=71.01 GB):  55%|█████▌    | 32/58 [00:00<00:00, 48.17it/s]

    Capturing num tokens (num_tokens=288 avail_mem=71.00 GB):  55%|█████▌    | 32/58 [00:00<00:00, 48.17it/s]Capturing num tokens (num_tokens=256 avail_mem=71.00 GB):  55%|█████▌    | 32/58 [00:00<00:00, 48.17it/s]Capturing num tokens (num_tokens=240 avail_mem=71.00 GB):  55%|█████▌    | 32/58 [00:00<00:00, 48.17it/s]Capturing num tokens (num_tokens=240 avail_mem=71.00 GB):  66%|██████▌   | 38/58 [00:00<00:00, 50.23it/s]Capturing num tokens (num_tokens=224 avail_mem=71.00 GB):  66%|██████▌   | 38/58 [00:00<00:00, 50.23it/s]Capturing num tokens (num_tokens=208 avail_mem=70.99 GB):  66%|██████▌   | 38/58 [00:00<00:00, 50.23it/s]Capturing num tokens (num_tokens=192 avail_mem=70.99 GB):  66%|██████▌   | 38/58 [00:00<00:00, 50.23it/s]Capturing num tokens (num_tokens=176 avail_mem=70.99 GB):  66%|██████▌   | 38/58 [00:00<00:00, 50.23it/s]Capturing num tokens (num_tokens=160 avail_mem=70.98 GB):  66%|██████▌   | 38/58 [00:00<00:00, 50.23it/s]Capturing num tokens (num_tokens=144 avail_mem=70.98 GB):  66%|██████▌   | 38/58 [00:00<00:00, 50.23it/s]Capturing num tokens (num_tokens=144 avail_mem=70.98 GB):  76%|███████▌  | 44/58 [00:01<00:00, 51.77it/s]Capturing num tokens (num_tokens=128 avail_mem=70.98 GB):  76%|███████▌  | 44/58 [00:01<00:00, 51.77it/s]Capturing num tokens (num_tokens=112 avail_mem=70.98 GB):  76%|███████▌  | 44/58 [00:01<00:00, 51.77it/s]Capturing num tokens (num_tokens=96 avail_mem=70.97 GB):  76%|███████▌  | 44/58 [00:01<00:00, 51.77it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=70.97 GB):  76%|███████▌  | 44/58 [00:01<00:00, 51.77it/s]Capturing num tokens (num_tokens=64 avail_mem=70.97 GB):  76%|███████▌  | 44/58 [00:01<00:00, 51.77it/s]Capturing num tokens (num_tokens=48 avail_mem=70.96 GB):  76%|███████▌  | 44/58 [00:01<00:00, 51.77it/s]Capturing num tokens (num_tokens=48 avail_mem=70.96 GB):  86%|████████▌ | 50/58 [00:01<00:00, 52.41it/s]Capturing num tokens (num_tokens=32 avail_mem=70.96 GB):  86%|████████▌ | 50/58 [00:01<00:00, 52.41it/s]Capturing num tokens (num_tokens=28 avail_mem=70.95 GB):  86%|████████▌ | 50/58 [00:01<00:00, 52.41it/s]Capturing num tokens (num_tokens=24 avail_mem=70.95 GB):  86%|████████▌ | 50/58 [00:01<00:00, 52.41it/s]Capturing num tokens (num_tokens=20 avail_mem=70.95 GB):  86%|████████▌ | 50/58 [00:01<00:00, 52.41it/s]Capturing num tokens (num_tokens=16 avail_mem=70.95 GB):  86%|████████▌ | 50/58 [00:01<00:00, 52.41it/s]Capturing num tokens (num_tokens=12 avail_mem=70.94 GB):  86%|████████▌ | 50/58 [00:01<00:00, 52.41it/s]Capturing num tokens (num_tokens=12 avail_mem=70.94 GB):  97%|█████████▋| 56/58 [00:01<00:00, 53.02it/s]Capturing num tokens (num_tokens=8 avail_mem=70.48 GB):  97%|█████████▋| 56/58 [00:01<00:00, 53.02it/s] Capturing num tokens (num_tokens=4 avail_mem=70.48 GB):  97%|█████████▋| 56/58 [00:01<00:00, 53.02it/s]

    Capturing num tokens (num_tokens=4 avail_mem=70.48 GB): 100%|██████████| 58/58 [00:01<00:00, 45.54it/s]


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
    Generated text:  Luke and I'm a very experienced naturalist and wildlife educator with a degree in zoology from the University of Alberta. I'm interested in the natural world and its inhabitants, and my work focuses on wildlife conservation, education, and environmental research. I have a passion for learning and helping people connect with the natural world in a meaningful way. I'm currently a part-time naturalist at the Calgary Zoo and spend my time providing educational presentations and guiding visitors through the exhibits. I'm also involved in volunteering and community work in the Calgary and Edmonton areas. I have a Bachelor of Science degree in Zoology from the University of Alberta and a Master
    ===============================
    Prompt: The president of the United States is
    Generated text:  a leader of the United States. He is a member of the government of the country. The president has the power to make laws and the power to sign them into law. The president is also in charge of national defense and foreign policy.
    
    Does it follow that "The president is not the only leader in the United States."?
    Choices: (I) yes; (II) it is not possible to tell; (III) no; Let's be accurate as possible and think first. The president is the leader of the United States. The president has the power to make laws, defend the country, and negotiate with other countries. The
    ===============================
    Prompt: The capital of France is
    Generated text:  ______. A. Paris B. Berlin C. Moscow D. New York
    A. Paris
    B. Berlin
    C. Moscow
    D. New York
    Answer:
    
    A
    
    Which of the following sentences is an example of an expression of emotion?
    A. I am very tired.
    B. My phone number is 1234567890.
    C. My dog is sleeping.
    D. The teacher said he would come.
    Answer:
    
    D
    
    Which of the following best describes a capital city?
    A. A city where the majority of the population lives
    B. A city that is predominantly inhabited by military
    ===============================
    Prompt: The future of AI is
    Generated text:  bright, and it's already here.
    The rise of the interconnected world of technology, coupled with the exponential growth of the Internet and cloud computing, has made it possible to build and deploy complex, highly sophisticated AI applications.
    There are already many, many smart machines that are capable of understanding, learning, and making predictions. These machines are like super-powered computers that are constantly learning and improving. They can detect patterns, recognize things, and interpret data more deeply.
    For example, Google's "DeepMind" machine, which it claims "can play at the highest levels of the board game of Go, learn new languages and recognize patterns in its


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris. It is the largest city in the country and the seat of government and administration for the country. It is also the most populous city in the European Union. Paris is known for its rich history, beautiful architecture, and vibrant culture. It is home to many famous landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. Paris is also known for its fashion industry and its role in the French Revolution. It is a popular tourist destination and a major economic center in Europe. Paris is a city that has a rich history and culture that has been shaped by many different civilizations over the centuries. It is
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: As AI becomes more advanced, it is likely to become more integrated with human intelligence, allowing for more complex and nuanced interactions. This could lead to more sophisticated forms of AI, such as those that can understand and respond to human emotions and emotions.
    
    2. Greater emphasis on ethical considerations: As AI becomes more advanced, there will be a greater emphasis on ethical considerations, such as privacy, bias, and transparency. This will require a more rigorous approach to AI development and deployment, with greater attention to the potential impacts of AI on society.
    
    3. Increased use
    


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
    Generated text:  [insert character name], and I'm a [insert occupation, such as "writer", "teacher", or "journalist"]. I'm currently [insert a relevant sentence or a fact about me]. My favorite thing to do is [insert a favorite activity or hobby, such as "reading a book", "traveling", "writing", or "photography"]. I'm a [insert a profession, such as "engineer", "artist", or "businessperson"]. I'm [insert a personal trait or characteristic, such as "innocent", "determined", "brave", "funny", or "creative"].
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, known for its historical sites, modern architecture, and vibrant culture. It's the world's most populous city and home to the Eiffel Tower and Notre-Dame Cathedral. It's also known for its fashion and food scene, as well as being a major center for European arts and sciences. Paris is a UNESCO World Heritage site and a UNESCO Creative City. Despite its fame, Paris is still a city that people want to visit for its delicious cuisine, lively nightlife, and passionate public culture. It's a vibrant, yet rather unique, place. France's capital city is Paris. The Eiffel Tower and Notre-Dame
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  expected to be a rapidly evolving field, characterized by unprecedented levels of complexity and sophistication. Some of the potential future trends in AI include:
    
    1. Greater emphasis on ethical and regulatory standards: With the increasing awareness of the negative consequences of AI on society, there will be a growing emphasis on creating ethical guidelines and regulations for its development and deployment. This could lead to the development of more sophisticated ethical AI systems that consider not only the benefits but also the risks associated with AI.
    
    2. Enhanced automation: AI is already being used in manufacturing, transportation, and other sectors to automate tasks, but there is potential for even greater automation in the future.


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

     profession

    ]

     who

    's

     always

     been

     passionate

     about

     [

    Your

     interest

     or

     hobby

    ].

     I

     believe

     that

     art

     is

     the

     key

     to

     expressing

     my

     creativity

     and

     understanding

     of

     the

     world

     around

     me

    .

     I

     enjoy

     using

     my

     creativity

     to

     solve

     complex

     problems

     and

     develop

     new

     ideas

     for

     projects

    .

     I

     also

     enjoy

     traveling

     to

     new

     places

     and

     trying

     new

     foods

    .

     What

    's

     your

     name

    ?

     And

     how

     are

     you

     today

    ?

     H

    ola

    ,

     soy

     [

    Your

     Name

    ],

     y

     soy

     un

     [

    Your

     profession

    ].

     Me

     s

    iento

     feliz

     de

     conoc

    erte

     y

     de

     decir

    te

     que

     siempre

     he

     estado

     interes

    ado

     en

     [

    Your

     interest

     or

     hobby

    ].

     Cre

    o

     que

     la

     arte

     es

     el

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     It

     is

     an

     historic

     city

     famous

     for

     its

     magnificent

     cath

    ed

    r

    als

    ,

     majestic

     pal

    aces

    ,

     and

     beautiful

     parks

    .

     Paris

     is

     the

     city

     of

     love

    ,

     fashion

    ,

     and

     music

    ,

     and

     it

     is

     also

     a

     significant

     center

     for

     science

     and

     art

    .

     The

     city

     is

     also

     known

     as

     "

    la

     liberté

    "

     (

    fre

    edom

    )

     and

     the

     "

    par

    ad

    ise

     of

     the

     human

     mind

    "

     by

     various

     authors

    ,

     and

     it

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

     by

     many

     people

    .

     Paris

     is

     home

     to

     the

     E

    iff

    el

     Tower

    ,

     Notre

     Dame

     Cathedral

    ,

     and

     the

     Lou

    vre

     Museum

    .

     It

     is

     also

     known

     as

     the

     "

    city

     of

     love

    "

     by

     many

     people

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     diverse

     and

     constantly

     evolving

    ,

     with

     many

     potential

     trends

     shaping

     the

     direction

     of

     this

     technology

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

     considerations

    :

     As

     AI

     becomes

     more

     integrated

     into

     our

     daily

     lives

    ,

     it

     is

     becoming

     increasingly

     important

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

     include

     issues

     such

     as

     privacy

    ,

     bias

    ,

     and

     the

     potential

     for

     unintended

     consequences

    .
    


    2

    .

     Improved

     transparency

     and

     accountability

    :

     As

     AI

     systems

     become

     more

     sophisticated

    ,

     it

     is

     becoming

     increasingly

     important

     for

     them

     to

     be

     transparent

     and

     accountable

     for

     their

     decisions

    .

     This

     could

     involve

     developing

     more

     sophisticated

     ways

     to

     explain

     how

     AI

     systems

     make

     decisions

    ,

     as

     well

     as

     developing

     new

     ways

     to

     hold

     AI

    



```python
llm.shutdown()
```
