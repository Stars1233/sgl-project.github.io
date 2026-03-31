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


    2026-03-31 19:59:58.101 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 19:59:58] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 19:59:58.102 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 19:59:58] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 19:59:58.102 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 19:59:58] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 19:59:58.102 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 19:59:58] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 19:59:58.102 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 19:59:58] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.45it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.45it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:26,  2.57s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:26,  2.57s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<01:03,  1.13s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<01:03,  1.13s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<01:03,  1.13s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:24,  2.17it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:24,  2.17it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:24,  2.17it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:02<00:13,  3.72it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:02<00:13,  3.72it/s]Compiling num tokens (num_tokens=4608):  10%|█         | 6/58 [00:02<00:13,  3.72it/s]Compiling num tokens (num_tokens=4096):  10%|█         | 6/58 [00:03<00:13,  3.72it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:03<00:07,  6.47it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:03<00:07,  6.47it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:03<00:07,  6.47it/s]Compiling num tokens (num_tokens=3328):  16%|█▌        | 9/58 [00:03<00:07,  6.47it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:03<00:04,  9.51it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:03<00:04,  9.51it/s]Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:03<00:04,  9.51it/s]Compiling num tokens (num_tokens=2560):  21%|██        | 12/58 [00:03<00:04,  9.51it/s]

    Compiling num tokens (num_tokens=2304):  21%|██        | 12/58 [00:03<00:04,  9.51it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:03<00:03, 13.91it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:03<00:03, 13.91it/s]Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:03<00:03, 13.91it/s]Compiling num tokens (num_tokens=1536):  28%|██▊       | 16/58 [00:03<00:03, 13.91it/s]Compiling num tokens (num_tokens=1280):  28%|██▊       | 16/58 [00:03<00:03, 13.91it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:03<00:02, 17.54it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:03<00:02, 17.54it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:03<00:02, 17.54it/s] 

    Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:03<00:02, 17.54it/s]Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:03<00:02, 17.54it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:03<00:01, 21.62it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:03<00:01, 21.62it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:03<00:01, 21.62it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:03<00:01, 21.62it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:03<00:01, 21.62it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:03<00:01, 24.42it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:03<00:01, 24.42it/s]

    Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:03<00:01, 24.42it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:03<00:01, 24.42it/s]Compiling num tokens (num_tokens=416):  48%|████▊     | 28/58 [00:03<00:01, 24.42it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:00, 26.90it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:00, 26.90it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:00, 26.90it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:00, 26.90it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:00, 26.90it/s]

    Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:03<00:00, 28.78it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:03<00:00, 28.78it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:03<00:00, 28.78it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:03<00:00, 28.78it/s]Compiling num tokens (num_tokens=208):  62%|██████▏   | 36/58 [00:03<00:00, 28.78it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:03<00:00, 30.13it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:03<00:00, 30.13it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:04<00:00, 30.13it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:04<00:00, 30.13it/s]Compiling num tokens (num_tokens=144):  69%|██████▉   | 40/58 [00:04<00:00, 30.13it/s]

    Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:04<00:00, 31.74it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:04<00:00, 31.74it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:04<00:00, 31.74it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:04<00:00, 31.74it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:04<00:00, 31.74it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:04<00:00, 31.42it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:04<00:00, 31.42it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:04<00:00, 31.42it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:04<00:00, 31.42it/s]

    Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:04<00:00, 31.42it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:04<00:00, 32.41it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:04<00:00, 32.41it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:04<00:00, 32.41it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:04<00:00, 32.41it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:04<00:00, 32.41it/s]Compiling num tokens (num_tokens=8):  90%|████████▉ | 52/58 [00:04<00:00, 32.41it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:04<00:00, 35.81it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:04<00:00, 35.81it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 12.94it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=59.25 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=59.25 GB):   2%|▏         | 1/58 [00:00<00:05,  9.65it/s]Capturing num tokens (num_tokens=7680 avail_mem=59.22 GB):   2%|▏         | 1/58 [00:00<00:05,  9.65it/s]Capturing num tokens (num_tokens=7168 avail_mem=59.21 GB):   2%|▏         | 1/58 [00:00<00:05,  9.65it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=59.21 GB):   5%|▌         | 3/58 [00:00<00:05, 10.64it/s]Capturing num tokens (num_tokens=6656 avail_mem=59.21 GB):   5%|▌         | 3/58 [00:00<00:05, 10.64it/s]Capturing num tokens (num_tokens=6144 avail_mem=59.21 GB):   5%|▌         | 3/58 [00:00<00:05, 10.64it/s]Capturing num tokens (num_tokens=6144 avail_mem=59.21 GB):   9%|▊         | 5/58 [00:00<00:04, 10.63it/s]Capturing num tokens (num_tokens=5632 avail_mem=59.21 GB):   9%|▊         | 5/58 [00:00<00:04, 10.63it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=59.21 GB):   9%|▊         | 5/58 [00:00<00:04, 10.63it/s]Capturing num tokens (num_tokens=4608 avail_mem=59.20 GB):   9%|▊         | 5/58 [00:00<00:04, 10.63it/s]Capturing num tokens (num_tokens=4608 avail_mem=59.20 GB):  14%|█▍        | 8/58 [00:00<00:03, 14.37it/s]Capturing num tokens (num_tokens=4096 avail_mem=59.20 GB):  14%|█▍        | 8/58 [00:00<00:03, 14.37it/s]Capturing num tokens (num_tokens=3840 avail_mem=59.20 GB):  14%|█▍        | 8/58 [00:00<00:03, 14.37it/s]Capturing num tokens (num_tokens=3584 avail_mem=59.19 GB):  14%|█▍        | 8/58 [00:00<00:03, 14.37it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=59.19 GB):  19%|█▉        | 11/58 [00:00<00:02, 17.90it/s]Capturing num tokens (num_tokens=3328 avail_mem=59.19 GB):  19%|█▉        | 11/58 [00:00<00:02, 17.90it/s]Capturing num tokens (num_tokens=3072 avail_mem=59.18 GB):  19%|█▉        | 11/58 [00:00<00:02, 17.90it/s]Capturing num tokens (num_tokens=2816 avail_mem=59.18 GB):  19%|█▉        | 11/58 [00:00<00:02, 17.90it/s]Capturing num tokens (num_tokens=2816 avail_mem=59.18 GB):  24%|██▍       | 14/58 [00:00<00:02, 19.68it/s]Capturing num tokens (num_tokens=2560 avail_mem=59.18 GB):  24%|██▍       | 14/58 [00:00<00:02, 19.68it/s]Capturing num tokens (num_tokens=2304 avail_mem=59.17 GB):  24%|██▍       | 14/58 [00:00<00:02, 19.68it/s]Capturing num tokens (num_tokens=2048 avail_mem=59.17 GB):  24%|██▍       | 14/58 [00:00<00:02, 19.68it/s]Capturing num tokens (num_tokens=1792 avail_mem=59.17 GB):  24%|██▍       | 14/58 [00:00<00:02, 19.68it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=59.16 GB):  24%|██▍       | 14/58 [00:00<00:02, 19.68it/s]Capturing num tokens (num_tokens=1536 avail_mem=59.16 GB):  33%|███▎      | 19/58 [00:00<00:01, 27.21it/s]Capturing num tokens (num_tokens=1280 avail_mem=59.16 GB):  33%|███▎      | 19/58 [00:00<00:01, 27.21it/s]Capturing num tokens (num_tokens=1024 avail_mem=59.14 GB):  33%|███▎      | 19/58 [00:00<00:01, 27.21it/s]Capturing num tokens (num_tokens=960 avail_mem=59.15 GB):  33%|███▎      | 19/58 [00:01<00:01, 27.21it/s] Capturing num tokens (num_tokens=896 avail_mem=59.15 GB):  33%|███▎      | 19/58 [00:01<00:01, 27.21it/s]Capturing num tokens (num_tokens=832 avail_mem=59.15 GB):  33%|███▎      | 19/58 [00:01<00:01, 27.21it/s]Capturing num tokens (num_tokens=768 avail_mem=59.14 GB):  33%|███▎      | 19/58 [00:01<00:01, 27.21it/s]Capturing num tokens (num_tokens=768 avail_mem=59.14 GB):  43%|████▎     | 25/58 [00:01<00:00, 33.75it/s]Capturing num tokens (num_tokens=704 avail_mem=59.14 GB):  43%|████▎     | 25/58 [00:01<00:00, 33.75it/s]Capturing num tokens (num_tokens=640 avail_mem=59.14 GB):  43%|████▎     | 25/58 [00:01<00:00, 33.75it/s]

    Capturing num tokens (num_tokens=576 avail_mem=59.14 GB):  43%|████▎     | 25/58 [00:01<00:00, 33.75it/s]Capturing num tokens (num_tokens=512 avail_mem=59.13 GB):  43%|████▎     | 25/58 [00:01<00:00, 33.75it/s]Capturing num tokens (num_tokens=512 avail_mem=59.13 GB):  50%|█████     | 29/58 [00:01<00:00, 30.81it/s]Capturing num tokens (num_tokens=480 avail_mem=57.95 GB):  50%|█████     | 29/58 [00:01<00:00, 30.81it/s]Capturing num tokens (num_tokens=448 avail_mem=57.95 GB):  50%|█████     | 29/58 [00:01<00:00, 30.81it/s]

    Capturing num tokens (num_tokens=416 avail_mem=59.11 GB):  50%|█████     | 29/58 [00:01<00:00, 30.81it/s]Capturing num tokens (num_tokens=384 avail_mem=59.10 GB):  50%|█████     | 29/58 [00:01<00:00, 30.81it/s]Capturing num tokens (num_tokens=384 avail_mem=59.10 GB):  57%|█████▋    | 33/58 [00:01<00:01, 19.47it/s]Capturing num tokens (num_tokens=352 avail_mem=58.11 GB):  57%|█████▋    | 33/58 [00:01<00:01, 19.47it/s]

    Capturing num tokens (num_tokens=320 avail_mem=58.11 GB):  57%|█████▋    | 33/58 [00:01<00:01, 19.47it/s]Capturing num tokens (num_tokens=288 avail_mem=59.09 GB):  57%|█████▋    | 33/58 [00:01<00:01, 19.47it/s]

    Capturing num tokens (num_tokens=288 avail_mem=59.09 GB):  62%|██████▏   | 36/58 [00:01<00:01, 15.50it/s]Capturing num tokens (num_tokens=256 avail_mem=58.17 GB):  62%|██████▏   | 36/58 [00:01<00:01, 15.50it/s]Capturing num tokens (num_tokens=240 avail_mem=58.17 GB):  62%|██████▏   | 36/58 [00:02<00:01, 15.50it/s]Capturing num tokens (num_tokens=224 avail_mem=58.16 GB):  62%|██████▏   | 36/58 [00:02<00:01, 15.50it/s]

    Capturing num tokens (num_tokens=224 avail_mem=58.16 GB):  67%|██████▋   | 39/58 [00:02<00:01, 12.20it/s]Capturing num tokens (num_tokens=208 avail_mem=59.08 GB):  67%|██████▋   | 39/58 [00:02<00:01, 12.20it/s]Capturing num tokens (num_tokens=192 avail_mem=58.23 GB):  67%|██████▋   | 39/58 [00:02<00:01, 12.20it/s]Capturing num tokens (num_tokens=192 avail_mem=58.23 GB):  71%|███████   | 41/58 [00:02<00:01, 12.56it/s]Capturing num tokens (num_tokens=176 avail_mem=58.22 GB):  71%|███████   | 41/58 [00:02<00:01, 12.56it/s]

    Capturing num tokens (num_tokens=160 avail_mem=58.31 GB):  71%|███████   | 41/58 [00:02<00:01, 12.56it/s]Capturing num tokens (num_tokens=160 avail_mem=58.31 GB):  74%|███████▍  | 43/58 [00:02<00:01, 13.02it/s]Capturing num tokens (num_tokens=144 avail_mem=59.08 GB):  74%|███████▍  | 43/58 [00:02<00:01, 13.02it/s]Capturing num tokens (num_tokens=128 avail_mem=58.29 GB):  74%|███████▍  | 43/58 [00:02<00:01, 13.02it/s]

    Capturing num tokens (num_tokens=128 avail_mem=58.29 GB):  78%|███████▊  | 45/58 [00:02<00:01, 12.94it/s]Capturing num tokens (num_tokens=112 avail_mem=58.29 GB):  78%|███████▊  | 45/58 [00:02<00:01, 12.94it/s]Capturing num tokens (num_tokens=96 avail_mem=59.07 GB):  78%|███████▊  | 45/58 [00:02<00:01, 12.94it/s] Capturing num tokens (num_tokens=96 avail_mem=59.07 GB):  81%|████████  | 47/58 [00:02<00:00, 13.57it/s]Capturing num tokens (num_tokens=80 avail_mem=58.34 GB):  81%|████████  | 47/58 [00:02<00:00, 13.57it/s]

    Capturing num tokens (num_tokens=64 avail_mem=58.34 GB):  81%|████████  | 47/58 [00:02<00:00, 13.57it/s]Capturing num tokens (num_tokens=64 avail_mem=58.34 GB):  84%|████████▍ | 49/58 [00:03<00:00, 13.56it/s]Capturing num tokens (num_tokens=48 avail_mem=59.07 GB):  84%|████████▍ | 49/58 [00:03<00:00, 13.56it/s]Capturing num tokens (num_tokens=32 avail_mem=58.40 GB):  84%|████████▍ | 49/58 [00:03<00:00, 13.56it/s]

    Capturing num tokens (num_tokens=32 avail_mem=58.40 GB):  88%|████████▊ | 51/58 [00:03<00:00, 12.57it/s]Capturing num tokens (num_tokens=28 avail_mem=59.09 GB):  88%|████████▊ | 51/58 [00:03<00:00, 12.57it/s]Capturing num tokens (num_tokens=24 avail_mem=59.06 GB):  88%|████████▊ | 51/58 [00:03<00:00, 12.57it/s]Capturing num tokens (num_tokens=24 avail_mem=59.06 GB):  91%|█████████▏| 53/58 [00:03<00:00, 13.17it/s]Capturing num tokens (num_tokens=20 avail_mem=58.46 GB):  91%|█████████▏| 53/58 [00:03<00:00, 13.17it/s]Capturing num tokens (num_tokens=16 avail_mem=59.06 GB):  91%|█████████▏| 53/58 [00:03<00:00, 13.17it/s]

    Capturing num tokens (num_tokens=16 avail_mem=59.06 GB):  95%|█████████▍| 55/58 [00:03<00:00, 14.43it/s]Capturing num tokens (num_tokens=12 avail_mem=59.05 GB):  95%|█████████▍| 55/58 [00:03<00:00, 14.43it/s]Capturing num tokens (num_tokens=8 avail_mem=58.52 GB):  95%|█████████▍| 55/58 [00:03<00:00, 14.43it/s] Capturing num tokens (num_tokens=8 avail_mem=58.52 GB):  98%|█████████▊| 57/58 [00:03<00:00, 14.80it/s]Capturing num tokens (num_tokens=4 avail_mem=59.05 GB):  98%|█████████▊| 57/58 [00:03<00:00, 14.80it/s]Capturing num tokens (num_tokens=4 avail_mem=59.05 GB): 100%|██████████| 58/58 [00:03<00:00, 15.91it/s]


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
    Generated text:  Peter and I’m the owner of a local bicycle store. Recently I saw a poster for a new bike that I really like, and I want to learn more about it. I heard it sells 15 different types of tires. How can I find the exact price of each type of tire on the bike? And what is the most economical way to purchase this bike? Can you provide an example of how I can use a machine learning model to predict the price of each type of tire based on the number of tires in a given order?
    To find the exact price of each type of tire on the bike, you can use the following
    ===============================
    Prompt: The president of the United States is
    Generated text:  seeking a new term. He has been in office since 2009. The president has a total of 100 days in office. How many days does he have left to complete his term?
    To determine how many days the president has left to complete his term, we need to subtract the number of days he has been in office from the total number of days he has left.
    
    1. The president has been in office since 2009. To find out how many days he has been in office, we subtract the number of years he has been in office from the total number of years.
    2. He
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris, the largest city in Europe. Of the many cultural and historical landmarks in Paris, one that must be seen for any visitor is the Louvre. The Louvre is an impressive place of historical and cultural importance, and the following sections will provide you with information on the history of the Louvre and some of the most popular attractions.
    
    The Louvre: The History of the Louvre
    
    The Louvre is a museum located in Paris, France. It is the largest museum in Europe by area, with more than 25 million objects in its collection. The Louvre is the home of the famous Mona Lisa and other works of
    ===============================
    Prompt: The future of AI is
    Generated text:  bringing in new opportunities for businesses, but it also presents a number of challenges. In today’s world, AI is making it easier to interact with machines and software, and this could lead to a more efficient and convenient way of working. However, it also raises concerns about privacy, security, and fairness in the AI systems we build.
    In this article, we will explore the pros and cons of AI, and how to mitigate its potential negative impacts.
    Pros of AI:
    1. Improved Efficiency: AI can automate tasks that were once done manually, freeing up employees to focus on more complex tasks. This can lead to increased productivity and cost savings


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


    Generated text:  [Name] and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you and learn more about you. What
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and Louvre Museum. It is also home to the French Parliament, the French Academy of Sciences, and the French National Library. Paris is a cultural and economic center with a rich history and a diverse population of over 10 million people. It is a major hub for international trade and diplomacy, and is a popular tourist destination. The city is known for its romantic atmosphere and its role as a symbol of French culture and identity. Paris is a vibrant and dynamic city that continues to evolve and adapt to the changing needs of its residents and
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends that are expected to shape the future of AI:
    
    1. Increased automation and robotics: As AI technology continues to advance, we are likely to see an increase in automation and robotics in various industries. This will lead to the development of new jobs, but also create new opportunities for people to work in areas such as data analysis, machine learning, and robotics.
    
    2. AI ethics and privacy: As AI technology becomes more advanced, there will be a need to address the ethical and privacy concerns
    


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
    Generated text:  [Your Name] and I'm a [Occupation/Status] who has always been passionate about [Your Interest/Interest], and I've been following the [Year/Period/Season] of [Season/Period/Year] for [Number of Years]. My journey has been filled with [List of Achievements] and I'm determined to continue [What You Want to Do/What You Want to Achieve]. I believe in the [Statement of Importance or Ideal], and I'm always striving to be [Your Desired Quality or Lifestyle], [Your Current State or Goal]. If you have any questions or would like to know more
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. 
    
    This response contains the following key facts:
    
      * It is the largest city in France by population
      * It is the seat of the Government of France
      * It is located in the Île de Paris region of the northwestern suburbs of the French capital Paris, France
      * It is situated on the Seine River, which flows through its center and extends across the city's center
      * It has a population of over 11 million people as of 2020
      * It is the country's most populous city and largest metropolitan area
      * It is one of the most
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  exciting and dynamic, and there are several key trends that are shaping its direction. Here are some of the most likely trends we can expect to see in the coming years:
    
    1. Autonomous vehicles: AI is already being used in cars to improve safety and efficiency, but it has the potential to revolutionize transportation by developing self-driving vehicles. Autonomous vehicles could greatly reduce traffic accidents, increase efficiency, and make travel more convenient and affordable.
    
    2. Personalized healthcare: AI has the potential to revolutionize healthcare by improving the accuracy and efficiency of medical diagnoses, developing new treatments, and personalizing treatments based on individual patients' unique characteristics. AI-powered


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

    ].

     I

    'm

     a

     

    2

    7

    -year

    -old

     software

     engineer

     with

     a

     passion

     for

     technology

     and

     innovation

    .

     I

     believe

     in

     the

     importance

     of

     creativity

     and

     problem

    -solving

    ,

     and

     I

    'm

     always

     looking

     for

     new

     ways

     to

     improve

     my

     skills

     and

     stay

     up

    -to

    -date

     with

     the

     latest

     trends

     in

     the

     tech

     industry

    .

     I

    'm

     confident

     in

     my

     ability

     to

     work

     independently

     and

     in

     a

     team

    ,

     and

     I

     enjoy

     collaborating

     with

     others

     to

     achieve

     our

     goals

    .

     In

     my

     free

     time

    ,

     I

     enjoy

     reading

    ,

     hiking

    ,

     and

     spending

     time

     with

     my

     family

     and

     friends

    .

     I

    'm

     excited

     to

     embark

     on

     this

     new

     adventure

     and

     learn

     more

     about

     the

     world

     around

     me

    .

     How

     about

     you

    ?

     What

     brings

     you

    
    
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

    .

     It

     is

     the

     largest

     city

     in

     the

     European

     Union

     and

     is

     home

     to

     many

     world

    -ren

    owned

     attractions

     such

     as

     the

     E

    iff

    el

     Tower

    ,

     Lou

    vre

     Museum

    ,

     and

     Notre

    -D

    ame

     Cathedral

    .

     Paris

     is

     a

     bustling

     city

     with

     a

     rich

     cultural

     heritage

     and

     is

     known

     for

     its

     fashion

    ,

     art

    ,

     and

     gastr

    onomy

    .

     It

     is

     also

     a

     popular

     tourist

     destination

    ,

     drawing

     millions

     of

     visitors

     each

     year

    .

     The

     city

    's

     history

    ,

     architecture

    ,

     and

     cuisine

     are

     all

     influenced

     by

     its

     diverse

     French

     population

    ,

     culture

    ,

     and

     geography

    .

     With

     its

     beautiful

     views

    ,

     charming

     streets

    ,

     and

     lively

     atmosphere

    ,

     Paris

     is

     a

     city

     that

     has

     stood

     the

     test

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     bright

     and

     varied

    ,

     with

     many

     exciting

     trends

     shaping

     the

     development

     of

     this

     technology

    .

     Here

     are

     some

     potential

     future

     trends

     in

     AI

    :
    


    1

    .

     Increased

     Integration

     with

     Existing

     Technologies

    :

     As

     AI

     systems

     become

     more

     sophisticated

    ,

     they

     will

     increasingly

     integrate

     with

     existing

     technologies

    ,

     such

     as

     sensors

    ,

     cameras

    ,

     and

     computers

    .

     This

     integration

     will

     enable

     AI

     to

     become

     more

     capable

     of

     performing

     tasks

     that

     are

     traditionally

     difficult

     or

     impossible

     to

     achieve

     by

     humans

    .
    


    2

    .

     Autonomous

     Agents

    :

     Autonomous

     agents

     will

     become

     more

     prevalent

     in

     various

     domains

    ,

     such

     as

     transportation

    ,

     healthcare

    ,

     and

     security

    .

     Autonomous

     agents

     will

     be

     able

     to

     perform

     tasks

     without

     human

     intervention

    ,

     which

     will

     improve

     efficiency

     and

     reduce

     errors

    .
    


    3

    .

     Personal

    ization

     and

    



```python
llm.shutdown()
```
