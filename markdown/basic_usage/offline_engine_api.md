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


    2026-03-30 08:41:57.461 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:41:57] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 08:41:57.461 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:41:57] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 08:41:57.461 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:41:57] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 08:41:57.461 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:41:57] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 08:41:57.462 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:41:57] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  4.35it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  4.35it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:09,  2.27s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:09,  2.27s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<00:56,  1.00s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<00:56,  1.00s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<00:56,  1.00s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:22,  2.44it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:22,  2.44it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:22,  2.44it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:02<00:12,  4.15it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:02<00:12,  4.15it/s]Compiling num tokens (num_tokens=4608):  10%|█         | 6/58 [00:02<00:12,  4.15it/s]Compiling num tokens (num_tokens=4096):  10%|█         | 6/58 [00:02<00:12,  4.15it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:02<00:06,  7.32it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:02<00:06,  7.32it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:02<00:06,  7.32it/s]Compiling num tokens (num_tokens=3328):  16%|█▌        | 9/58 [00:02<00:06,  7.32it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:02<00:04, 10.51it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:02<00:04, 10.51it/s]Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:02<00:04, 10.51it/s]Compiling num tokens (num_tokens=2560):  21%|██        | 12/58 [00:02<00:04, 10.51it/s]Compiling num tokens (num_tokens=2304):  21%|██        | 12/58 [00:02<00:04, 10.51it/s]

    Compiling num tokens (num_tokens=2048):  21%|██        | 12/58 [00:02<00:04, 10.51it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:02<00:02, 16.98it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:02<00:02, 16.98it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:02<00:02, 16.98it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:02<00:02, 16.98it/s]Compiling num tokens (num_tokens=1024):  29%|██▉       | 17/58 [00:03<00:02, 16.98it/s]Compiling num tokens (num_tokens=960):  29%|██▉       | 17/58 [00:03<00:02, 16.98it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:03<00:01, 23.13it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:03<00:01, 23.13it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:03<00:01, 23.13it/s]Compiling num tokens (num_tokens=768):  38%|███▊      | 22/58 [00:03<00:01, 23.13it/s]Compiling num tokens (num_tokens=704):  38%|███▊      | 22/58 [00:03<00:01, 23.13it/s]Compiling num tokens (num_tokens=640):  38%|███▊      | 22/58 [00:03<00:01, 23.13it/s]

    Compiling num tokens (num_tokens=576):  38%|███▊      | 22/58 [00:03<00:01, 23.13it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:03<00:00, 30.77it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:03<00:00, 30.77it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:03<00:00, 30.77it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:03<00:00, 30.77it/s]Compiling num tokens (num_tokens=416):  48%|████▊     | 28/58 [00:03<00:00, 30.77it/s]Compiling num tokens (num_tokens=384):  48%|████▊     | 28/58 [00:03<00:00, 30.77it/s]Compiling num tokens (num_tokens=352):  48%|████▊     | 28/58 [00:03<00:00, 30.77it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:03<00:00, 36.65it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:03<00:00, 36.65it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:03<00:00, 36.65it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:03<00:00, 36.65it/s]Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:03<00:00, 36.65it/s]

    Compiling num tokens (num_tokens=224):  59%|█████▊    | 34/58 [00:03<00:00, 36.65it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 39.47it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 39.47it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 39.47it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 39.47it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 39.47it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 39.47it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 39.47it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 42.39it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 42.39it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 42.39it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 42.39it/s]

    Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 42.39it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 42.39it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 44.13it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 44.13it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 44.13it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 44.13it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 44.13it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 44.13it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 44.13it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 44.13it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:03<00:00, 50.14it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:03<00:00, 50.14it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.68it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=55.69 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=55.66 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=55.66 GB):   3%|▎         | 2/58 [00:00<00:04, 11.35it/s]Capturing num tokens (num_tokens=7168 avail_mem=55.66 GB):   3%|▎         | 2/58 [00:00<00:04, 11.35it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=55.65 GB):   3%|▎         | 2/58 [00:00<00:04, 11.35it/s]Capturing num tokens (num_tokens=6656 avail_mem=55.65 GB):   7%|▋         | 4/58 [00:00<00:04, 12.66it/s]Capturing num tokens (num_tokens=6144 avail_mem=55.66 GB):   7%|▋         | 4/58 [00:00<00:04, 12.66it/s]Capturing num tokens (num_tokens=5632 avail_mem=55.65 GB):   7%|▋         | 4/58 [00:00<00:04, 12.66it/s]Capturing num tokens (num_tokens=5632 avail_mem=55.65 GB):  10%|█         | 6/58 [00:00<00:03, 13.96it/s]Capturing num tokens (num_tokens=5120 avail_mem=55.65 GB):  10%|█         | 6/58 [00:00<00:03, 13.96it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=55.65 GB):  10%|█         | 6/58 [00:00<00:03, 13.96it/s]Capturing num tokens (num_tokens=4608 avail_mem=55.65 GB):  14%|█▍        | 8/58 [00:00<00:03, 15.74it/s]Capturing num tokens (num_tokens=4096 avail_mem=55.65 GB):  14%|█▍        | 8/58 [00:00<00:03, 15.74it/s]Capturing num tokens (num_tokens=3840 avail_mem=55.64 GB):  14%|█▍        | 8/58 [00:00<00:03, 15.74it/s]Capturing num tokens (num_tokens=3584 avail_mem=55.64 GB):  14%|█▍        | 8/58 [00:00<00:03, 15.74it/s]Capturing num tokens (num_tokens=3584 avail_mem=55.64 GB):  19%|█▉        | 11/58 [00:00<00:02, 18.52it/s]Capturing num tokens (num_tokens=3328 avail_mem=55.64 GB):  19%|█▉        | 11/58 [00:00<00:02, 18.52it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=55.63 GB):  19%|█▉        | 11/58 [00:00<00:02, 18.52it/s]Capturing num tokens (num_tokens=2816 avail_mem=55.63 GB):  19%|█▉        | 11/58 [00:00<00:02, 18.52it/s]Capturing num tokens (num_tokens=2816 avail_mem=55.63 GB):  24%|██▍       | 14/58 [00:00<00:02, 21.12it/s]Capturing num tokens (num_tokens=2560 avail_mem=55.63 GB):  24%|██▍       | 14/58 [00:00<00:02, 21.12it/s]Capturing num tokens (num_tokens=2304 avail_mem=55.62 GB):  24%|██▍       | 14/58 [00:00<00:02, 21.12it/s]Capturing num tokens (num_tokens=2048 avail_mem=55.62 GB):  24%|██▍       | 14/58 [00:00<00:02, 21.12it/s]Capturing num tokens (num_tokens=2048 avail_mem=55.62 GB):  29%|██▉       | 17/58 [00:00<00:01, 23.64it/s]Capturing num tokens (num_tokens=1792 avail_mem=55.62 GB):  29%|██▉       | 17/58 [00:00<00:01, 23.64it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=55.61 GB):  29%|██▉       | 17/58 [00:00<00:01, 23.64it/s]Capturing num tokens (num_tokens=1280 avail_mem=55.61 GB):  29%|██▉       | 17/58 [00:00<00:01, 23.64it/s]Capturing num tokens (num_tokens=1024 avail_mem=55.59 GB):  29%|██▉       | 17/58 [00:00<00:01, 23.64it/s]Capturing num tokens (num_tokens=1024 avail_mem=55.59 GB):  36%|███▌      | 21/58 [00:01<00:01, 27.12it/s]Capturing num tokens (num_tokens=960 avail_mem=55.60 GB):  36%|███▌      | 21/58 [00:01<00:01, 27.12it/s] Capturing num tokens (num_tokens=896 avail_mem=55.60 GB):  36%|███▌      | 21/58 [00:01<00:01, 27.12it/s]Capturing num tokens (num_tokens=832 avail_mem=55.60 GB):  36%|███▌      | 21/58 [00:01<00:01, 27.12it/s]Capturing num tokens (num_tokens=768 avail_mem=55.59 GB):  36%|███▌      | 21/58 [00:01<00:01, 27.12it/s]Capturing num tokens (num_tokens=768 avail_mem=55.59 GB):  43%|████▎     | 25/58 [00:01<00:01, 30.37it/s]Capturing num tokens (num_tokens=704 avail_mem=55.59 GB):  43%|████▎     | 25/58 [00:01<00:01, 30.37it/s]

    Capturing num tokens (num_tokens=640 avail_mem=55.59 GB):  43%|████▎     | 25/58 [00:01<00:01, 30.37it/s]Capturing num tokens (num_tokens=576 avail_mem=55.59 GB):  43%|████▎     | 25/58 [00:01<00:01, 30.37it/s]Capturing num tokens (num_tokens=512 avail_mem=55.58 GB):  43%|████▎     | 25/58 [00:01<00:01, 30.37it/s]Capturing num tokens (num_tokens=512 avail_mem=55.58 GB):  50%|█████     | 29/58 [00:01<00:00, 29.22it/s]Capturing num tokens (num_tokens=480 avail_mem=55.59 GB):  50%|█████     | 29/58 [00:01<00:00, 29.22it/s]Capturing num tokens (num_tokens=448 avail_mem=55.59 GB):  50%|█████     | 29/58 [00:01<00:00, 29.22it/s]Capturing num tokens (num_tokens=416 avail_mem=55.59 GB):  50%|█████     | 29/58 [00:01<00:00, 29.22it/s]

    Capturing num tokens (num_tokens=416 avail_mem=55.59 GB):  55%|█████▌    | 32/58 [00:01<00:00, 29.08it/s]Capturing num tokens (num_tokens=384 avail_mem=55.58 GB):  55%|█████▌    | 32/58 [00:01<00:00, 29.08it/s]Capturing num tokens (num_tokens=352 avail_mem=55.58 GB):  55%|█████▌    | 32/58 [00:01<00:00, 29.08it/s]Capturing num tokens (num_tokens=320 avail_mem=55.58 GB):  55%|█████▌    | 32/58 [00:01<00:00, 29.08it/s]Capturing num tokens (num_tokens=288 avail_mem=55.57 GB):  55%|█████▌    | 32/58 [00:01<00:00, 29.08it/s]Capturing num tokens (num_tokens=288 avail_mem=55.57 GB):  62%|██████▏   | 36/58 [00:01<00:00, 30.95it/s]Capturing num tokens (num_tokens=256 avail_mem=55.57 GB):  62%|██████▏   | 36/58 [00:01<00:00, 30.95it/s]Capturing num tokens (num_tokens=240 avail_mem=55.57 GB):  62%|██████▏   | 36/58 [00:01<00:00, 30.95it/s]Capturing num tokens (num_tokens=224 avail_mem=55.56 GB):  62%|██████▏   | 36/58 [00:01<00:00, 30.95it/s]Capturing num tokens (num_tokens=208 avail_mem=55.56 GB):  62%|██████▏   | 36/58 [00:01<00:00, 30.95it/s]Capturing num tokens (num_tokens=192 avail_mem=55.56 GB):  62%|██████▏   | 36/58 [00:01<00:00, 30.95it/s]

    Capturing num tokens (num_tokens=176 avail_mem=55.56 GB):  62%|██████▏   | 36/58 [00:01<00:00, 30.95it/s]Capturing num tokens (num_tokens=176 avail_mem=55.56 GB):  72%|███████▏  | 42/58 [00:01<00:00, 37.13it/s]Capturing num tokens (num_tokens=160 avail_mem=55.55 GB):  72%|███████▏  | 42/58 [00:01<00:00, 37.13it/s]Capturing num tokens (num_tokens=144 avail_mem=55.55 GB):  72%|███████▏  | 42/58 [00:01<00:00, 37.13it/s]Capturing num tokens (num_tokens=128 avail_mem=55.55 GB):  72%|███████▏  | 42/58 [00:01<00:00, 37.13it/s]Capturing num tokens (num_tokens=112 avail_mem=55.55 GB):  72%|███████▏  | 42/58 [00:01<00:00, 37.13it/s]

    Capturing num tokens (num_tokens=96 avail_mem=76.64 GB):  72%|███████▏  | 42/58 [00:01<00:00, 37.13it/s] Capturing num tokens (num_tokens=96 avail_mem=76.64 GB):  81%|████████  | 47/58 [00:01<00:00, 22.78it/s]Capturing num tokens (num_tokens=80 avail_mem=76.64 GB):  81%|████████  | 47/58 [00:01<00:00, 22.78it/s]Capturing num tokens (num_tokens=64 avail_mem=76.64 GB):  81%|████████  | 47/58 [00:02<00:00, 22.78it/s]Capturing num tokens (num_tokens=48 avail_mem=76.63 GB):  81%|████████  | 47/58 [00:02<00:00, 22.78it/s]Capturing num tokens (num_tokens=32 avail_mem=76.63 GB):  81%|████████  | 47/58 [00:02<00:00, 22.78it/s]Capturing num tokens (num_tokens=28 avail_mem=76.62 GB):  81%|████████  | 47/58 [00:02<00:00, 22.78it/s]Capturing num tokens (num_tokens=28 avail_mem=76.62 GB):  90%|████████▉ | 52/58 [00:02<00:00, 27.25it/s]Capturing num tokens (num_tokens=24 avail_mem=76.62 GB):  90%|████████▉ | 52/58 [00:02<00:00, 27.25it/s]Capturing num tokens (num_tokens=20 avail_mem=76.62 GB):  90%|████████▉ | 52/58 [00:02<00:00, 27.25it/s]Capturing num tokens (num_tokens=16 avail_mem=76.62 GB):  90%|████████▉ | 52/58 [00:02<00:00, 27.25it/s]Capturing num tokens (num_tokens=12 avail_mem=76.61 GB):  90%|████████▉ | 52/58 [00:02<00:00, 27.25it/s]

    Capturing num tokens (num_tokens=8 avail_mem=76.61 GB):  90%|████████▉ | 52/58 [00:02<00:00, 27.25it/s] Capturing num tokens (num_tokens=4 avail_mem=76.61 GB):  90%|████████▉ | 52/58 [00:02<00:00, 27.25it/s]Capturing num tokens (num_tokens=4 avail_mem=76.61 GB): 100%|██████████| 58/58 [00:02<00:00, 32.72it/s]Capturing num tokens (num_tokens=4 avail_mem=76.61 GB): 100%|██████████| 58/58 [00:02<00:00, 26.30it/s]


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
    Generated text:  Aleksandar. I’m a software engineer who was born in Belgrade, Serbia. I graduated from the University of Belgrade with a Bachelor’s degree in computer science and a Master’s degree in software engineering. I have a great passion for technology and I love to code. I am now pursuing a Master’s degree in Cybersecurity from the University of Glasgow, UK.
    I'm passionate about helping people gain knowledge and skills in the field of software development and cybersecurity. I'm an advocate for open-source software, and I'm always looking for ways to improve and learn more about the latest software development trends and practices.
    Looking forward to chatting
    ===============================
    Prompt: The president of the United States is
    Generated text:  running for a second term. When is the next election scheduled? The next presidential election is scheduled for November 3, 2024. The election is a two-term presidential election, meaning that there will be two terms in office, with the winner of the second-round election having to win in a majority to be the new president. This is the same election that was scheduled to take place in 2020 but was postponed due to the coronavirus pandemic. The next election is scheduled to be held in 2024. Unfortunately, due to the spread of the virus, the election was canceled in 20
    ===============================
    Prompt: The capital of France is
    Generated text:  _______. A: Paris B: Strasbourg C: Lyon D: Marseille
    To determine the capital of France, we need to recall the names of the official capitals of France. The official capital of France is Paris. Therefore, the correct answer is:
    
    A: Paris
    
    Let's verify this by checking the other options:
    
    - Strasbourg is the capital of France.
    - Lyon is the capital of France.
    - Marseille is the capital of France.
    
    Since Paris is the only option that is explicitly stated to be the capital of France, the correct answer is:
    
    \boxed{A}
    ===============================
    Prompt: The future of AI is
    Generated text:  far from certain. In the words of the author, James T. Berry, “The future of AI will likely be defined by the role of AI in healthcare, which will be the most significant in the next 5 years.” With advances in both machine learning and deep learning, doctors can now use AI to treat diseases, assist in drug development, and even reduce the cost of healthcare. However, these advances also raise questions about data privacy, the ethical implications of AI, and the overall impact on society. As the field of AI continues to evolve, it will be important for researchers, policymakers, and the general public to work together to


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


    Generated text:  [Name] and I'm a [Age] year old [Gender] who has always been fascinated by the world of [Occupation]. I'm a [Skill or Hobby] enthusiast and have a passion for [Reason for Passion]. I'm always looking for new experiences and learning new things, and I'm always eager to share my knowledge with others. I'm a [Personality Trait] person and I'm always ready to learn and grow. I'm a [Future Goal] and I'm excited to see where my journey takes me. Thank you for having me! [Name] [Age] [Gender] [Occupation]
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as the City of Light, a city with a rich history and culture. It is located on the Seine River and is the largest city in Europe by population. Paris is famous for its iconic landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. The city is also known for its fashion industry, art scene, and food culture. Paris is a popular tourist destination and is home to many museums, theaters, and other cultural institutions. The city is also known for its cuisine, with dishes like croissants, escargot, and charcuterie being popular. Paris is
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn from and adapt to human behavior. This could lead to more sophisticated and personalized AI systems that can better understand and respond to human needs.
    
    2. Greater use of machine learning: Machine learning is likely to become more prevalent, allowing AI systems to learn from data and improve their performance over time. This could lead to more efficient and effective AI systems that can handle a wider range of tasks.
    
    3. Increased focus on ethical AI: As AI becomes more integrated with human intelligence, there will
    


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
    Generated text:  [Name] and I'm a [character's profession or role] at [the fictional organization or company I work for]. I'm excited to meet you and learn about your background, and I look forward to our future collaborations. What is your name and how can I get in touch with you? [Name] [Phone Number] [Email Address] [LinkedIn Profile] [Other relevant information if applicable]
    Hello, my name is [Name] and I'm a [character's profession or role] at [the fictional organization or company I work for]. I'm excited to meet you and learn about your background, and I look forward
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, located in the French region of Paris and is the largest city in the country. It is known for its historical significance, renowned art and architecture, and vibrant cultural scene. Paris is also one of the world's most visited cities, attracting millions of visitors each year. The city is home to many UNESCO World Heritage sites, including the Eiffel Tower, Louvre Museum, Notre-Dame Cathedral, and the Louvre Museum. It is also home to numerous museums, including the Musée d'Orsay, the Musée Rodin, and the Musée d'Art Moderne. Paris is a global center for art
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be characterized by several key trends, including:
    
    1. Increased accuracy and reliability: As AI becomes more integrated into various industries, it is likely that we will see greater emphasis on improving accuracy and reliability in our systems. This could include improving the efficiency of AI algorithms, reducing the risk of bias in AI models, and developing more sophisticated methods of data analysis and prediction.
    
    2. Greater diversity and inclusion: AI is increasingly being used for a wide range of tasks, from natural language processing to robotics and autonomous vehicles. As this technology becomes more widespread, we may see an increase in diversity and inclusion in the development and deployment of AI systems


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

    insert

     character

    's

     name

    ].

     I

     am

     a

     [

    insert

     character

    's

     profession

     or

     role

    ]

     with

     a

     passion

     for

     [

    insert

     a

     trait

     or

     interest

    ].

     Let

    's

     chat

     about

     [

    insert

     a

     topic

     of

     interest

    ].

     Let

    's

     get

     to

     know

     each

     other

    ,

     and

     let

    's

     see

     where

     this

     conversation

     leads

     us

    .

     [

    insert

     character

    's

     name

    ]

     

    🏃

    ‍

    ♂

    ️

    💼

    
    


    Hey

     there

    !

     

    👋

     I

    'm

     [

    insert

     character

    's

     name

    ],

     the

     [

    insert

     character

    's

     profession

     or

     role

    ].

     

    😊

     I

    'm

     a

     [

    insert

     character

    's

     trait

     or

     interest

    ]

     with

     a

     passion

     for

     [

    insert

     a

     trait

     or

     interest

    ].

     Let

    's

     chat

     about

     [

    insert

     a

     topic

     of

     interest

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     The

     city

     is

     located

     on

     the

     River

     Se

    ine

     and

     is

     the

     center

     of

     France

    's

     administrative

    ,

     economic

    ,

     and

     cultural

     life

    .

     It

     is

     home

     to

     many

     of

     the

     country

    's

     most

     iconic

     landmarks

    ,

     including

     the

     E

    iff

    el

     Tower

    ,

     the

     Lou

    vre

     Museum

    ,

     and

     the

     Notre

    -D

    ame

     Cathedral

    .

     Paris

     is

     also

     known

     for

     its

     rich

     culinary

     traditions

     and

     famous

     cuisine

    .

     The

     city

     has

     a

     diverse

     population

     of

     over

     

    2

     million

     people

     and

     is

     the

     world

    's

     

    3

    5

    th

     most

     populous

     city

    .

     Paris

     is

     a

     popular

     tourist

     destination

     for

     visitors

     from

     around

     the

     globe

    .

     It

     is

     also

     the

     seat

     of

     the

     French

     government

     and

     is

     considered

     to

     be

     a

     cultural

     and

     political

     hub

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     characterized

     by

     increasing

     complexity

    ,

     personal

    ization

    ,

     and

     integration

     with

     other

     technologies

    .

     Here

     are

     some

     possible

     future

     trends

     in

     artificial

     intelligence

    :
    


    1

    .

     Increased

     Personal

    ization

    :

     As

     AI

     becomes

     more

     sophisticated

    ,

     it

     will

     be

     able

     to

     better

     understand

     and

     interpret

     human

     behavior

     and

     preferences

    .

     This

     will

     allow

     AI

     systems

     to

     provide

     more

     personalized

     recommendations

     and

     experiences

    ,

     which

     will

     ultimately

     benefit

     users

    .
    


    2

    .

     Integration

     with

     Other

     Technologies

    :

     AI

     is

     already

     being

     integrated

     with

     other

     technologies

    ,

     such

     as

     the

     Internet

     of

     Things

     (

    Io

    T

    ),

     smart

     homes

    ,

     and

     bi

    ometric

     systems

    .

     This

     trend

     is

     likely

     to

     continue

     as

     AI

     becomes

     more

     powerful

     and

     capable

     of

     understanding

     and

     interpreting

     complex

     data

     sets

    .
    


    3

    



```python
llm.shutdown()
```
