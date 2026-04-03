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


    2026-04-03 06:59:32.779 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 06:59:32] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 06:59:32.779 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 06:59:32] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 06:59:32.779 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 06:59:32] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 06:59:32.779 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 06:59:32] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 06:59:32.779 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 06:59:32] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  7.10it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  7.09it/s]


    2026-04-03 06:59:37,269 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 06:59:37] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:34,  2.72s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:34,  2.72s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<01:06,  1.18s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<01:06,  1.18s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<01:06,  1.18s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:25,  2.08it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:25,  2.08it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:25,  2.08it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:03<00:25,  2.08it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:03<00:11,  4.32it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:03<00:11,  4.32it/s]Compiling num tokens (num_tokens=4096):  12%|█▏        | 7/58 [00:03<00:11,  4.32it/s]

    Compiling num tokens (num_tokens=3840):  12%|█▏        | 7/58 [00:03<00:11,  4.32it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:03<00:06,  6.99it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:03<00:06,  6.99it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:03<00:06,  6.99it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:03<00:06,  6.99it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:03<00:04,  9.90it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:03<00:04,  9.90it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:03<00:04,  9.90it/s]Compiling num tokens (num_tokens=2304):  22%|██▏       | 13/58 [00:03<00:04,  9.90it/s]

    Compiling num tokens (num_tokens=2048):  22%|██▏       | 13/58 [00:03<00:04,  9.90it/s]Compiling num tokens (num_tokens=1792):  22%|██▏       | 13/58 [00:03<00:04,  9.90it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:02, 15.34it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:02, 15.34it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:02, 15.34it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:02, 15.34it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:02, 15.34it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:03<00:01, 18.74it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:03<00:01, 18.74it/s]

    Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:03<00:01, 18.74it/s]Compiling num tokens (num_tokens=768):  38%|███▊      | 22/58 [00:03<00:01, 18.74it/s]Compiling num tokens (num_tokens=704):  38%|███▊      | 22/58 [00:03<00:01, 18.74it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 22.19it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 22.19it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 22.19it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 22.19it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 22.19it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:03<00:01, 25.43it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:03<00:01, 25.43it/s]

    Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:03<00:01, 25.43it/s]Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:03<00:01, 25.43it/s]Compiling num tokens (num_tokens=352):  52%|█████▏    | 30/58 [00:03<00:01, 25.43it/s]Compiling num tokens (num_tokens=320):  52%|█████▏    | 30/58 [00:03<00:01, 25.43it/s]Compiling num tokens (num_tokens=288):  52%|█████▏    | 30/58 [00:03<00:01, 25.43it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:03<00:00, 32.04it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:03<00:00, 32.04it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:03<00:00, 32.04it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:03<00:00, 32.04it/s]Compiling num tokens (num_tokens=208):  62%|██████▏   | 36/58 [00:03<00:00, 32.04it/s]Compiling num tokens (num_tokens=192):  62%|██████▏   | 36/58 [00:03<00:00, 32.04it/s]Compiling num tokens (num_tokens=176):  62%|██████▏   | 36/58 [00:03<00:00, 32.04it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:04<00:00, 37.58it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:04<00:00, 37.58it/s]

    Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:04<00:00, 37.58it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:04<00:00, 37.58it/s]Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:04<00:00, 37.58it/s]Compiling num tokens (num_tokens=96):  72%|███████▏  | 42/58 [00:04<00:00, 37.58it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:04<00:00, 40.60it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:04<00:00, 40.60it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:04<00:00, 40.60it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:04<00:00, 40.60it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:04<00:00, 40.60it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:04<00:00, 40.60it/s]Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:04<00:00, 40.60it/s]Compiling num tokens (num_tokens=20):  81%|████████  | 47/58 [00:04<00:00, 40.60it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:04<00:00, 47.35it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:04<00:00, 47.35it/s]

    Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:04<00:00, 47.35it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:04<00:00, 47.35it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:04<00:00, 47.35it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 13.59it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=132.42 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=132.39 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=132.39 GB):   3%|▎         | 2/58 [00:00<00:03, 14.43it/s]Capturing num tokens (num_tokens=7168 avail_mem=132.38 GB):   3%|▎         | 2/58 [00:00<00:03, 14.43it/s]Capturing num tokens (num_tokens=6656 avail_mem=132.38 GB):   3%|▎         | 2/58 [00:00<00:03, 14.43it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=132.38 GB):   7%|▋         | 4/58 [00:00<00:03, 16.55it/s]Capturing num tokens (num_tokens=6144 avail_mem=132.38 GB):   7%|▋         | 4/58 [00:00<00:03, 16.55it/s]Capturing num tokens (num_tokens=5632 avail_mem=132.38 GB):   7%|▋         | 4/58 [00:00<00:03, 16.55it/s]Capturing num tokens (num_tokens=5632 avail_mem=132.38 GB):  10%|█         | 6/58 [00:00<00:03, 13.77it/s]Capturing num tokens (num_tokens=5120 avail_mem=132.38 GB):  10%|█         | 6/58 [00:00<00:03, 13.77it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=132.37 GB):  10%|█         | 6/58 [00:00<00:03, 13.77it/s]Capturing num tokens (num_tokens=4096 avail_mem=132.37 GB):  10%|█         | 6/58 [00:00<00:03, 13.77it/s]Capturing num tokens (num_tokens=3840 avail_mem=132.37 GB):  10%|█         | 6/58 [00:00<00:03, 13.77it/s]Capturing num tokens (num_tokens=3840 avail_mem=132.37 GB):  17%|█▋        | 10/58 [00:00<00:02, 20.79it/s]Capturing num tokens (num_tokens=3584 avail_mem=132.37 GB):  17%|█▋        | 10/58 [00:00<00:02, 20.79it/s]Capturing num tokens (num_tokens=3328 avail_mem=132.36 GB):  17%|█▋        | 10/58 [00:00<00:02, 20.79it/s]Capturing num tokens (num_tokens=3072 avail_mem=132.36 GB):  17%|█▋        | 10/58 [00:00<00:02, 20.79it/s]Capturing num tokens (num_tokens=2816 avail_mem=132.36 GB):  17%|█▋        | 10/58 [00:00<00:02, 20.79it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=132.36 GB):  24%|██▍       | 14/58 [00:00<00:01, 24.75it/s]Capturing num tokens (num_tokens=2560 avail_mem=132.35 GB):  24%|██▍       | 14/58 [00:00<00:01, 24.75it/s]Capturing num tokens (num_tokens=2304 avail_mem=132.35 GB):  24%|██▍       | 14/58 [00:00<00:01, 24.75it/s]Capturing num tokens (num_tokens=2048 avail_mem=132.35 GB):  24%|██▍       | 14/58 [00:00<00:01, 24.75it/s]Capturing num tokens (num_tokens=1792 avail_mem=132.34 GB):  24%|██▍       | 14/58 [00:00<00:01, 24.75it/s]Capturing num tokens (num_tokens=1536 avail_mem=132.34 GB):  24%|██▍       | 14/58 [00:00<00:01, 24.75it/s]Capturing num tokens (num_tokens=1536 avail_mem=132.34 GB):  33%|███▎      | 19/58 [00:00<00:01, 30.49it/s]Capturing num tokens (num_tokens=1280 avail_mem=132.34 GB):  33%|███▎      | 19/58 [00:00<00:01, 30.49it/s]Capturing num tokens (num_tokens=1024 avail_mem=132.32 GB):  33%|███▎      | 19/58 [00:00<00:01, 30.49it/s]Capturing num tokens (num_tokens=960 avail_mem=132.33 GB):  33%|███▎      | 19/58 [00:00<00:01, 30.49it/s] Capturing num tokens (num_tokens=896 avail_mem=132.33 GB):  33%|███▎      | 19/58 [00:00<00:01, 30.49it/s]

    Capturing num tokens (num_tokens=832 avail_mem=132.32 GB):  33%|███▎      | 19/58 [00:00<00:01, 30.49it/s]Capturing num tokens (num_tokens=832 avail_mem=132.32 GB):  41%|████▏     | 24/58 [00:00<00:00, 34.98it/s]Capturing num tokens (num_tokens=768 avail_mem=132.32 GB):  41%|████▏     | 24/58 [00:00<00:00, 34.98it/s]Capturing num tokens (num_tokens=704 avail_mem=132.32 GB):  41%|████▏     | 24/58 [00:00<00:00, 34.98it/s]Capturing num tokens (num_tokens=640 avail_mem=132.31 GB):  41%|████▏     | 24/58 [00:00<00:00, 34.98it/s]Capturing num tokens (num_tokens=576 avail_mem=132.31 GB):  41%|████▏     | 24/58 [00:00<00:00, 34.98it/s]Capturing num tokens (num_tokens=512 avail_mem=132.30 GB):  41%|████▏     | 24/58 [00:00<00:00, 34.98it/s]Capturing num tokens (num_tokens=512 avail_mem=132.30 GB):  50%|█████     | 29/58 [00:00<00:00, 38.17it/s]Capturing num tokens (num_tokens=480 avail_mem=132.32 GB):  50%|█████     | 29/58 [00:00<00:00, 38.17it/s]Capturing num tokens (num_tokens=448 avail_mem=132.32 GB):  50%|█████     | 29/58 [00:01<00:00, 38.17it/s]Capturing num tokens (num_tokens=416 avail_mem=132.31 GB):  50%|█████     | 29/58 [00:01<00:00, 38.17it/s]Capturing num tokens (num_tokens=384 avail_mem=132.31 GB):  50%|█████     | 29/58 [00:01<00:00, 38.17it/s]

    Capturing num tokens (num_tokens=352 avail_mem=132.31 GB):  50%|█████     | 29/58 [00:01<00:00, 38.17it/s]Capturing num tokens (num_tokens=352 avail_mem=132.31 GB):  59%|█████▊    | 34/58 [00:01<00:00, 40.47it/s]Capturing num tokens (num_tokens=320 avail_mem=132.30 GB):  59%|█████▊    | 34/58 [00:01<00:00, 40.47it/s]Capturing num tokens (num_tokens=288 avail_mem=132.30 GB):  59%|█████▊    | 34/58 [00:01<00:00, 40.47it/s]Capturing num tokens (num_tokens=256 avail_mem=132.30 GB):  59%|█████▊    | 34/58 [00:01<00:00, 40.47it/s]Capturing num tokens (num_tokens=240 avail_mem=132.29 GB):  59%|█████▊    | 34/58 [00:01<00:00, 40.47it/s]Capturing num tokens (num_tokens=224 avail_mem=132.29 GB):  59%|█████▊    | 34/58 [00:01<00:00, 40.47it/s]Capturing num tokens (num_tokens=224 avail_mem=132.29 GB):  67%|██████▋   | 39/58 [00:01<00:00, 42.00it/s]Capturing num tokens (num_tokens=208 avail_mem=132.29 GB):  67%|██████▋   | 39/58 [00:01<00:00, 42.00it/s]Capturing num tokens (num_tokens=192 avail_mem=132.29 GB):  67%|██████▋   | 39/58 [00:01<00:00, 42.00it/s]Capturing num tokens (num_tokens=176 avail_mem=132.28 GB):  67%|██████▋   | 39/58 [00:01<00:00, 42.00it/s]Capturing num tokens (num_tokens=160 avail_mem=132.28 GB):  67%|██████▋   | 39/58 [00:01<00:00, 42.00it/s]

    Capturing num tokens (num_tokens=144 avail_mem=132.28 GB):  67%|██████▋   | 39/58 [00:01<00:00, 42.00it/s]Capturing num tokens (num_tokens=144 avail_mem=132.28 GB):  76%|███████▌  | 44/58 [00:01<00:00, 43.30it/s]Capturing num tokens (num_tokens=128 avail_mem=132.27 GB):  76%|███████▌  | 44/58 [00:01<00:00, 43.30it/s]Capturing num tokens (num_tokens=112 avail_mem=132.27 GB):  76%|███████▌  | 44/58 [00:01<00:00, 43.30it/s]Capturing num tokens (num_tokens=96 avail_mem=132.27 GB):  76%|███████▌  | 44/58 [00:01<00:00, 43.30it/s] Capturing num tokens (num_tokens=80 avail_mem=132.26 GB):  76%|███████▌  | 44/58 [00:01<00:00, 43.30it/s]Capturing num tokens (num_tokens=64 avail_mem=132.26 GB):  76%|███████▌  | 44/58 [00:01<00:00, 43.30it/s]Capturing num tokens (num_tokens=64 avail_mem=132.26 GB):  84%|████████▍ | 49/58 [00:01<00:00, 43.87it/s]Capturing num tokens (num_tokens=48 avail_mem=132.26 GB):  84%|████████▍ | 49/58 [00:01<00:00, 43.87it/s]Capturing num tokens (num_tokens=32 avail_mem=132.25 GB):  84%|████████▍ | 49/58 [00:01<00:00, 43.87it/s]Capturing num tokens (num_tokens=28 avail_mem=132.25 GB):  84%|████████▍ | 49/58 [00:01<00:00, 43.87it/s]Capturing num tokens (num_tokens=24 avail_mem=132.25 GB):  84%|████████▍ | 49/58 [00:01<00:00, 43.87it/s]

    Capturing num tokens (num_tokens=20 avail_mem=132.24 GB):  84%|████████▍ | 49/58 [00:01<00:00, 43.87it/s]Capturing num tokens (num_tokens=20 avail_mem=132.24 GB):  93%|█████████▎| 54/58 [00:01<00:00, 43.72it/s]Capturing num tokens (num_tokens=16 avail_mem=132.24 GB):  93%|█████████▎| 54/58 [00:01<00:00, 43.72it/s]Capturing num tokens (num_tokens=12 avail_mem=132.24 GB):  93%|█████████▎| 54/58 [00:01<00:00, 43.72it/s]Capturing num tokens (num_tokens=8 avail_mem=132.23 GB):  93%|█████████▎| 54/58 [00:01<00:00, 43.72it/s] Capturing num tokens (num_tokens=4 avail_mem=132.23 GB):  93%|█████████▎| 54/58 [00:01<00:00, 43.72it/s]Capturing num tokens (num_tokens=4 avail_mem=132.23 GB): 100%|██████████| 58/58 [00:01<00:00, 34.01it/s]


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
    Generated text:  Ethan and I am an AI language model. I am here to answer any questions you might have about the world and help you find the information you need. How can I assist you today? Let me know! 📚🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍
    ===============================
    Prompt: The president of the United States is
    Generated text:  running for a second term. He is currently 55 years old. The president is currently 20 years older than the youngest person in the United States Senate. The senator is 35 years old. If the president plans to serve a second term, what is the total age of the first 100 people in the United States Senate?
    To determine the total age of the first 100 people in the United States Senate if the president plans to serve a second term, we need to follow these steps:
    
    1. **Identify the current age of the president:**
       \[
       \text{President
    ===============================
    Prompt: The capital of France is
    Generated text: :
    
    A) Paris
    B) Nice
    C) Lyon
    D) Montreux
    E) Marseille
    The capital of France is Paris. 
    
    To determine the correct answer:
    
    1. Review the given options:
       A) Paris
       B) Nice
       C) Lyon
       D) Montreux
       E) Marseille
    
    2. Identify the capital of France from the provided options:
       - Paris is the capital of France.
    
    3. Conclude that the correct answer is:
    
    A) Paris
    
    Thus, the capital of France is Paris. \boxed{A}
    ===============================
    Prompt: The future of AI is
    Generated text:  different than the past, and in a world where the more advanced the AI, the more the human is doing it, the question of ethics, trust and accountability becomes even more prominent. The rise of self-driving vehicles is a prime example of AI that takes advantage of a lot of our trust, and how we use technology in our daily lives. However, this same technology can be used for harm, like in a situation where the car is programmed to avoid certain pedestrians. The lack of accountability in the use of AI should be a concern for anyone who uses it in their daily life.
    In this article, we will look at AI and its


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


    Generated text:  Paris, which is known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and Louvre Museum. It is also a major center for art, culture, and politics in the country. The city is home to many world-renowned museums, including the Louvre, the Musée d'Orsay, and the Musée Rodin. Paris is also known for its diverse food scene, including its famous French cuisine and its numerous wine regions. The city is a major transportation hub, with many major airports and train stations. Overall, Paris is a vibrant and exciting city with a rich history and culture.
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction. Here are some of the most likely trends:
    
    1. Increased focus on ethical AI: As more people become aware of the potential risks and biases in AI, there will be a greater emphasis on ethical considerations. This could lead to the development of more transparent and accountable AI systems, as well as the implementation of more robust privacy and security measures.
    
    2. Greater integration with human decision-making: AI is likely to become more integrated with human decision-making in the future, as more people become comfortable with the idea of AI making decisions on their behalf. This
    


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
    Generated text:  [Name] and I am a professional professional photographer. I specialize in capturing the beauty of the natural world and making my clients feel empowered to capture their own moments. I believe in the power of art to inspire and connect with others, and I am always looking to learn and improve my craft. Thank you for taking the time to meet me. What's one of your favorite moments to capture? As a professional photographer, capturing moments of beauty and awe is something I really love. It allows me to witness the magic that is possible through the lens of my camera and make my clients feel like they are a part of the moment. How do
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, commonly referred to as "Paris," and is the largest city in Europe by population. It is a popular tourist destination and home to many iconic landmarks such as Notre Dame Cathedral, the Eiffel Tower, and the Louvre Museum. Paris is a cultural and artistic center with a rich history and diverse population. Visitors to Paris can enjoy its beautiful architecture, historic landmarks, and lively nightlife. The city is known for its vibrant music scene, including the famous "Bohemian Rhapsody" by Queen, and its rich culinary heritage. Paris is a major hub of business and finance, with the French Parliament being located in the
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to involve many exciting developments and possibilities. Here are some possible trends:
    
    1. Improved accuracy: As AI technology continues to evolve, we can expect to see more accurate predictions and decisions. This could involve using more advanced algorithms, better data storage and processing power, and a more sophisticated understanding of human decision-making.
    
    2. Enhanced personalization: AI will be able to better understand and predict individual behavior and preferences, resulting in more personalized experiences. This could involve the use of machine learning algorithms to analyze user data and provide more targeted recommendations and interactions.
    
    3. Autonomous vehicles: The future of AI will likely see the development of autonomous vehicles.


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

    'm

     a

     [

    insert

     profession

    ]

    !
    


    My

     journey

     began

     when

     [

    insert

     start

     date

     and

     purpose

    ]

     as

     a

     [

    insert

     profession

    ]

     with

     [

    insert

     company

     name

    ].

     I

     have

     been

     [

    insert

     time

     in

     profession

    ]

     at

     [

    insert

     company

     name

    ]

     for

     [

    insert

     number

     of

     years

    ]

    .
    


    My

     passion

     for

     [

    insert

     profession

    ]

     led

     me

     to

     [

    insert

     current

     job

     title

    ]

     at

     [

    insert

     company

     name

    ].

     My

     expertise

     and

     dedication

     have

     been

     instrumental

     in

     [

    insert

     achievements

     or

     accomplishments

    ].

     I

     have

     a

     strong

     work

     ethic

     and

     enjoy

     [

    insert

     hobby

    ,

     sport

    ,

     or

     activity

    ].
    


    As

     a

     [

    insert

     profession

    ],

     I

     strive

     to

     [

    insert

     personal

     goal

    ]

     and

     [

    insert

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     located

     in

     the

     center

     of

     the

     country

     and

     the

     third

    -largest

     city

     in

     Europe

    .

     It

     has

     a

     rich

     history

     and

     is

     known

     for

     its

     museums

    ,

     art

     museums

    ,

     and

     world

    -ren

    owned

     landmarks

    ,

     such

     as

     the

     E

    iff

    el

     Tower

     and

     the

     Lou

    vre

    .

     Paris

     is

     also

     a

     major

     center

     for

     fashion

     and

     food

    ,

     with

     famous

     cultural

     events

    ,

     such

     as

     the

     E

    iff

    el

     Tower

     party

    .

     The

     city

     is

     an

     important

     hub

     for

     business

     and

     trade

    ,

     and

     is

     home

     to

     the

     European

     Parliament

    ,

     the

     United

     Nations

    ,

     and

     the

     European

     Space

     Agency

    .

     As

     the

     capital

    ,

     Paris

     plays

     a

     crucial

     role

     in

     the

     country

    's

     cultural

    ,

     economic

    ,

     and

     political

     landscape

    .

     Therefore

    ,

     it

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     characterized

     by

     an

     ever

    -in

    creasing

     ability

     to

     learn

     and

     adapt

     to

     new

     situations

    .

     AI

     systems

     are

     becoming

     more

     capable

     of

     analyzing

     complex

     data

     sets

     and

     making

     decisions

     that

     are

     beyond

     the

     capabilities

     of

     humans

    .

     This

     leads

     to

     the

     development

     of

     new

     applications

     and

     industries

    ,

     such

     as

     autonomous

     vehicles

    ,

     smart

     homes

    ,

     and

     virtual

     assistants

    ,

     which

     can

     help

     improve

     our

     lives

     and

     the

     efficiency

     of

     our

     work

    .

     AI

     is

     also

     being

     integrated

     into

     our

     daily

     lives

     through

     voice

     assistants

    ,

     chat

    bots

    ,

     and

     other

     smart

     technology

    ,

     which

     allows

     for

     a

     more

     seamless

     and

     efficient

     interaction

     with

     the

     world

     around

     us

    .

     However

    ,

     as

     AI

     continues

     to

     evolve

    ,

     it

     is

     also

     raising

     ethical

     and

     societal

     concerns

    



```python
llm.shutdown()
```
