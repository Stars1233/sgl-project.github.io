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


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.71it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.70it/s]


    2026-04-04 01:42:43,491 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 01:42:43] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:29,  2.62s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:29,  2.62s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:29,  2.62s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:29,  2.62s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:28,  1.91it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:28,  1.91it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:28,  1.91it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:28,  1.91it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:28,  1.91it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:28,  1.91it/s]

    Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:28,  1.91it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.84it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.84it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.84it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.84it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:08,  5.84it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:08,  5.84it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:08,  5.84it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:02<00:03, 10.59it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:02<00:03, 10.59it/s]Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:02<00:03, 10.59it/s]Compiling num tokens (num_tokens=1536):  28%|██▊       | 16/58 [00:02<00:03, 10.59it/s]Compiling num tokens (num_tokens=1280):  28%|██▊       | 16/58 [00:02<00:03, 10.59it/s]

    Compiling num tokens (num_tokens=1024):  28%|██▊       | 16/58 [00:03<00:03, 10.59it/s]Compiling num tokens (num_tokens=960):  28%|██▊       | 16/58 [00:03<00:03, 10.59it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:03<00:02, 16.06it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:03<00:02, 16.06it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:03<00:02, 16.06it/s]Compiling num tokens (num_tokens=768):  38%|███▊      | 22/58 [00:03<00:02, 16.06it/s]Compiling num tokens (num_tokens=704):  38%|███▊      | 22/58 [00:03<00:02, 16.06it/s]Compiling num tokens (num_tokens=640):  38%|███▊      | 22/58 [00:03<00:02, 16.06it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:03<00:01, 20.43it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:03<00:01, 20.43it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:03<00:01, 20.43it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:03<00:01, 20.43it/s]

    Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:03<00:01, 20.43it/s]Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:03<00:01, 20.43it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 23.59it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 23.59it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 23.59it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 23.59it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 23.59it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 23.59it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:03<00:00, 26.10it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:03<00:00, 26.10it/s]

    Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:03<00:00, 26.10it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:03<00:00, 26.10it/s]Compiling num tokens (num_tokens=192):  64%|██████▍   | 37/58 [00:03<00:00, 26.10it/s]Compiling num tokens (num_tokens=176):  64%|██████▍   | 37/58 [00:03<00:00, 26.10it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:03<00:00, 28.60it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:03<00:00, 28.60it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:03<00:00, 28.60it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:03<00:00, 28.60it/s]Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:03<00:00, 28.60it/s]

    Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:03<00:00, 29.47it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:03<00:00, 29.47it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:03<00:00, 29.47it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:03<00:00, 29.47it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:03<00:00, 29.47it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 30.56it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 30.56it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 30.56it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 30.56it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 30.56it/s]

    Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 30.56it/s]Compiling num tokens (num_tokens=16):  95%|█████████▍| 55/58 [00:03<00:00, 33.60it/s]Compiling num tokens (num_tokens=12):  95%|█████████▍| 55/58 [00:03<00:00, 33.60it/s]Compiling num tokens (num_tokens=8):  95%|█████████▍| 55/58 [00:03<00:00, 33.60it/s] Compiling num tokens (num_tokens=4):  95%|█████████▍| 55/58 [00:03<00:00, 33.60it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 14.51it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=59.09 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=59.09 GB):   2%|▏         | 1/58 [00:00<00:15,  3.78it/s]Capturing num tokens (num_tokens=7680 avail_mem=59.07 GB):   2%|▏         | 1/58 [00:00<00:15,  3.78it/s]Capturing num tokens (num_tokens=7680 avail_mem=59.07 GB):   3%|▎         | 2/58 [00:00<00:10,  5.21it/s]Capturing num tokens (num_tokens=7168 avail_mem=59.06 GB):   3%|▎         | 2/58 [00:00<00:10,  5.21it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=59.06 GB):   5%|▌         | 3/58 [00:00<00:11,  4.66it/s]Capturing num tokens (num_tokens=6656 avail_mem=59.06 GB):   5%|▌         | 3/58 [00:00<00:11,  4.66it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=59.06 GB):   7%|▋         | 4/58 [00:00<00:11,  4.76it/s]Capturing num tokens (num_tokens=6144 avail_mem=59.06 GB):   7%|▋         | 4/58 [00:00<00:11,  4.76it/s]Capturing num tokens (num_tokens=6144 avail_mem=59.06 GB):   9%|▊         | 5/58 [00:00<00:09,  5.60it/s]Capturing num tokens (num_tokens=5632 avail_mem=59.06 GB):   9%|▊         | 5/58 [00:00<00:09,  5.60it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=59.06 GB):  10%|█         | 6/58 [00:01<00:08,  6.39it/s]Capturing num tokens (num_tokens=5120 avail_mem=59.06 GB):  10%|█         | 6/58 [00:01<00:08,  6.39it/s]Capturing num tokens (num_tokens=4608 avail_mem=59.05 GB):  10%|█         | 6/58 [00:01<00:08,  6.39it/s]Capturing num tokens (num_tokens=4096 avail_mem=59.05 GB):  10%|█         | 6/58 [00:01<00:08,  6.39it/s]Capturing num tokens (num_tokens=4096 avail_mem=59.05 GB):  16%|█▌        | 9/58 [00:01<00:04, 10.87it/s]Capturing num tokens (num_tokens=3840 avail_mem=59.05 GB):  16%|█▌        | 9/58 [00:01<00:04, 10.87it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=59.04 GB):  16%|█▌        | 9/58 [00:01<00:04, 10.87it/s]Capturing num tokens (num_tokens=3584 avail_mem=59.04 GB):  19%|█▉        | 11/58 [00:01<00:04, 11.13it/s]Capturing num tokens (num_tokens=3328 avail_mem=59.04 GB):  19%|█▉        | 11/58 [00:01<00:04, 11.13it/s]Capturing num tokens (num_tokens=3072 avail_mem=59.04 GB):  19%|█▉        | 11/58 [00:01<00:04, 11.13it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=59.04 GB):  22%|██▏       | 13/58 [00:01<00:04, 11.20it/s]Capturing num tokens (num_tokens=2816 avail_mem=59.04 GB):  22%|██▏       | 13/58 [00:01<00:04, 11.20it/s]Capturing num tokens (num_tokens=2560 avail_mem=59.03 GB):  22%|██▏       | 13/58 [00:01<00:04, 11.20it/s]Capturing num tokens (num_tokens=2560 avail_mem=59.03 GB):  26%|██▌       | 15/58 [00:01<00:03, 11.13it/s]Capturing num tokens (num_tokens=2304 avail_mem=59.03 GB):  26%|██▌       | 15/58 [00:01<00:03, 11.13it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=59.03 GB):  26%|██▌       | 15/58 [00:01<00:03, 11.13it/s]Capturing num tokens (num_tokens=2048 avail_mem=59.03 GB):  29%|██▉       | 17/58 [00:01<00:03, 11.20it/s]Capturing num tokens (num_tokens=1792 avail_mem=59.02 GB):  29%|██▉       | 17/58 [00:01<00:03, 11.20it/s]Capturing num tokens (num_tokens=1536 avail_mem=59.02 GB):  29%|██▉       | 17/58 [00:02<00:03, 11.20it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=59.02 GB):  33%|███▎      | 19/58 [00:02<00:03, 11.30it/s]Capturing num tokens (num_tokens=1280 avail_mem=59.01 GB):  33%|███▎      | 19/58 [00:02<00:03, 11.30it/s]Capturing num tokens (num_tokens=1024 avail_mem=59.00 GB):  33%|███▎      | 19/58 [00:02<00:03, 11.30it/s]Capturing num tokens (num_tokens=1024 avail_mem=59.00 GB):  36%|███▌      | 21/58 [00:02<00:02, 12.99it/s]Capturing num tokens (num_tokens=960 avail_mem=59.01 GB):  36%|███▌      | 21/58 [00:02<00:02, 12.99it/s] Capturing num tokens (num_tokens=896 avail_mem=59.01 GB):  36%|███▌      | 21/58 [00:02<00:02, 12.99it/s]Capturing num tokens (num_tokens=832 avail_mem=59.00 GB):  36%|███▌      | 21/58 [00:02<00:02, 12.99it/s]Capturing num tokens (num_tokens=768 avail_mem=59.00 GB):  36%|███▌      | 21/58 [00:02<00:02, 12.99it/s]

    Capturing num tokens (num_tokens=768 avail_mem=59.00 GB):  43%|████▎     | 25/58 [00:02<00:01, 18.93it/s]Capturing num tokens (num_tokens=704 avail_mem=59.00 GB):  43%|████▎     | 25/58 [00:02<00:01, 18.93it/s]Capturing num tokens (num_tokens=640 avail_mem=58.99 GB):  43%|████▎     | 25/58 [00:02<00:01, 18.93it/s]Capturing num tokens (num_tokens=576 avail_mem=58.99 GB):  43%|████▎     | 25/58 [00:02<00:01, 18.93it/s]Capturing num tokens (num_tokens=576 avail_mem=58.99 GB):  48%|████▊     | 28/58 [00:02<00:01, 17.61it/s]Capturing num tokens (num_tokens=512 avail_mem=58.98 GB):  48%|████▊     | 28/58 [00:02<00:01, 17.61it/s]

    Capturing num tokens (num_tokens=480 avail_mem=59.00 GB):  48%|████▊     | 28/58 [00:02<00:01, 17.61it/s]Capturing num tokens (num_tokens=448 avail_mem=59.00 GB):  48%|████▊     | 28/58 [00:02<00:01, 17.61it/s]Capturing num tokens (num_tokens=416 avail_mem=58.99 GB):  48%|████▊     | 28/58 [00:02<00:01, 17.61it/s]Capturing num tokens (num_tokens=384 avail_mem=58.99 GB):  48%|████▊     | 28/58 [00:02<00:01, 17.61it/s]Capturing num tokens (num_tokens=352 avail_mem=58.99 GB):  48%|████▊     | 28/58 [00:02<00:01, 17.61it/s]Capturing num tokens (num_tokens=352 avail_mem=58.99 GB):  59%|█████▊    | 34/58 [00:02<00:00, 26.07it/s]Capturing num tokens (num_tokens=320 avail_mem=58.98 GB):  59%|█████▊    | 34/58 [00:02<00:00, 26.07it/s]Capturing num tokens (num_tokens=288 avail_mem=58.98 GB):  59%|█████▊    | 34/58 [00:02<00:00, 26.07it/s]Capturing num tokens (num_tokens=256 avail_mem=58.98 GB):  59%|█████▊    | 34/58 [00:02<00:00, 26.07it/s]Capturing num tokens (num_tokens=240 avail_mem=58.97 GB):  59%|█████▊    | 34/58 [00:02<00:00, 26.07it/s]Capturing num tokens (num_tokens=224 avail_mem=58.97 GB):  59%|█████▊    | 34/58 [00:02<00:00, 26.07it/s]Capturing num tokens (num_tokens=224 avail_mem=58.97 GB):  67%|██████▋   | 39/58 [00:02<00:00, 31.46it/s]Capturing num tokens (num_tokens=208 avail_mem=58.97 GB):  67%|██████▋   | 39/58 [00:02<00:00, 31.46it/s]

    Capturing num tokens (num_tokens=192 avail_mem=58.97 GB):  67%|██████▋   | 39/58 [00:02<00:00, 31.46it/s]Capturing num tokens (num_tokens=176 avail_mem=58.96 GB):  67%|██████▋   | 39/58 [00:02<00:00, 31.46it/s]Capturing num tokens (num_tokens=160 avail_mem=58.96 GB):  67%|██████▋   | 39/58 [00:02<00:00, 31.46it/s]Capturing num tokens (num_tokens=160 avail_mem=58.96 GB):  74%|███████▍  | 43/58 [00:02<00:00, 31.86it/s]Capturing num tokens (num_tokens=144 avail_mem=58.95 GB):  74%|███████▍  | 43/58 [00:02<00:00, 31.86it/s]Capturing num tokens (num_tokens=128 avail_mem=58.95 GB):  74%|███████▍  | 43/58 [00:02<00:00, 31.86it/s]Capturing num tokens (num_tokens=112 avail_mem=58.95 GB):  74%|███████▍  | 43/58 [00:02<00:00, 31.86it/s]Capturing num tokens (num_tokens=96 avail_mem=58.95 GB):  74%|███████▍  | 43/58 [00:02<00:00, 31.86it/s] Capturing num tokens (num_tokens=80 avail_mem=58.94 GB):  74%|███████▍  | 43/58 [00:02<00:00, 31.86it/s]

    Capturing num tokens (num_tokens=80 avail_mem=58.94 GB):  83%|████████▎ | 48/58 [00:03<00:00, 27.47it/s]Capturing num tokens (num_tokens=64 avail_mem=58.94 GB):  83%|████████▎ | 48/58 [00:03<00:00, 27.47it/s]Capturing num tokens (num_tokens=48 avail_mem=58.94 GB):  83%|████████▎ | 48/58 [00:03<00:00, 27.47it/s]Capturing num tokens (num_tokens=32 avail_mem=58.93 GB):  83%|████████▎ | 48/58 [00:03<00:00, 27.47it/s]

    Capturing num tokens (num_tokens=28 avail_mem=58.93 GB):  83%|████████▎ | 48/58 [00:03<00:00, 27.47it/s]Capturing num tokens (num_tokens=28 avail_mem=58.93 GB):  90%|████████▉ | 52/58 [00:03<00:00, 17.73it/s]Capturing num tokens (num_tokens=24 avail_mem=58.92 GB):  90%|████████▉ | 52/58 [00:03<00:00, 17.73it/s]Capturing num tokens (num_tokens=20 avail_mem=58.92 GB):  90%|████████▉ | 52/58 [00:03<00:00, 17.73it/s]

    Capturing num tokens (num_tokens=16 avail_mem=58.92 GB):  90%|████████▉ | 52/58 [00:03<00:00, 17.73it/s]Capturing num tokens (num_tokens=16 avail_mem=58.92 GB):  95%|█████████▍| 55/58 [00:03<00:00, 16.24it/s]Capturing num tokens (num_tokens=12 avail_mem=58.91 GB):  95%|█████████▍| 55/58 [00:03<00:00, 16.24it/s]Capturing num tokens (num_tokens=8 avail_mem=58.91 GB):  95%|█████████▍| 55/58 [00:03<00:00, 16.24it/s] Capturing num tokens (num_tokens=4 avail_mem=58.91 GB):  95%|█████████▍| 55/58 [00:03<00:00, 16.24it/s]Capturing num tokens (num_tokens=4 avail_mem=58.91 GB): 100%|██████████| 58/58 [00:03<00:00, 15.14it/s]


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
    Generated text:  Rich and my favorite place to be is my home.
    My dad has always encouraged me to pursue an interest that I’m passionate about. I’ve always had a love of the outdoors and golf. Golfing is a sport that I love to play and when I play I love to have the game up to my shoulders, so to speak. One day, after a lot of searching and browsing, I decided that I wanted to get into golf. My dad got me the best golfer in the family, Mike, to help me learn. Mike is a great golfer and he also helped me get a job at a golfing club in
    ===============================
    Prompt: The president of the United States is
    Generated text:  trying to pick a new dish for the country's Thanksgiving table. He has a choice of 8 different pumpkin pies, 5 different savory chicken dishes, and 7 different vegetarian meat dishes. In how many ways can he choose one dish from each category, if he decides to pick the dish that has the most number of vegetarian meat dishes?
    To determine the number of ways the president can choose one dish from each category such that the chosen dish has the most number of vegetarian meat dishes, we need to break down the problem into a few steps.
    
    First, identify the number of vegetarian meat dishes available:
    - There are 7 different vegetarian
    ===============================
    Prompt: The capital of France is
    Generated text: :
    
    A: Paris  
    B: Nice  
    C: Montmartre  
    D: Lausanne
    
    To determine the capital of France, we need to remember which capital of France is typically the largest city and most populous. Let's analyze the options:
    
    A: Paris - Paris is the capital of France and is the largest city in Europe by population. However, it is not the capital in terms of size or importance.
    
    B: Nice - Nice is the capital of France but is not the largest city. It is known for its fine wines and beaches.
    
    C: Montmartre - Montmartre is not a capital of France.
    ===============================
    Prompt: The future of AI is
    Generated text:  uncertain. Only time will tell. For now, AI is making waves in the digital world. With the rise of AI, we see a huge surge in the growth of the Internet, e-commerce, social media, video streaming, and more. In the realm of data, we see the rise of big data and data analytics. With the growth of data, AI will grow even more and become more and more important.
    Why do you think AI will become more important in the future?
    AI will become more important in the future because of its potential to revolutionize the way we live and work. With its ability to process and analyze large amounts


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [insert a short description of your character or personality here]. And what do you do for a living? I'm a [insert a short description of your job here]. And what do you enjoy doing? I enjoy [insert a short description of what you enjoy doing here]. And what do you like to do in your free time? I like to [insert a short description of what you like to do here]. And what do you think is
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic Eiffel Tower and the annual Eiffel Tower Festival. It is the largest city in France and the third-largest city in the world by population. Paris is a cultural and historical center with many famous landmarks and museums. It is also a major transportation hub, with many major highways and rail lines. The city is known for its cuisine, fashion, and art scene. Paris is a popular tourist destination and a major economic center in France. It is also a major center for international business and diplomacy. The city is home to many universities and research institutions, and is a major center for science and
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by rapid advancements in several key areas, including:
    
    1. Increased integration with human intelligence: AI systems are likely to become more integrated with human intelligence, allowing them to learn from and adapt to the behavior and preferences of humans. This could lead to more personalized and adaptive AI systems that can better understand and respond to the needs of their users.
    
    2. Enhanced machine learning capabilities: AI systems are likely to become even more powerful and capable, with the ability to learn from vast amounts of data and make complex decisions based on that data. This could lead to more sophisticated and intelligent AI systems that can perform a wide range of tasks
    


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
    Generated text:  [Name], and I'm a [Character type]. I have [Number of years working in the industry] years of experience, and I have a deep appreciation for [Title or job title]. [Name], I'm passionate about [Why you are passionate about this job]. How do you view your career path, and what do you look forward to doing next?
    
    [Name], what brings you to this position? I am excited to contribute to the team and make a positive impact in the field.
    
    What challenges do you face in your current role, and how do you plan to overcome them? I am always looking for ways to improve my
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, which is the largest city in the country and known for its iconic landmarks such as the Eiffel Tower, the Louvre Museum, and the Notre-Dame Cathedral.
    The capital of France is Paris, which is the largest city in the country and known for its iconic landmarks such as the Eiffel Tower, the Louvre Museum, and the Notre-Dame Cathedral. The capital of France is also home to many important historical and cultural sites, including the Palace of Versailles and the Champs-Élysées. Paris is known for its cuisine, fashion, and annual festivals, such as the Eiffel Tower Festival
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be shaped by several key trends that could significantly transform the way we live, work, and interact with technology. Here are some potential future trends in AI:
    
    1. Increased automation and robotics: As AI technology continues to improve, we can expect automation and robotics to become more prevalent in many industries. This could lead to the creation of more efficient, high-quality jobs and potentially eliminate certain types of manual tasks, freeing up time for human workers to focus on more creative and rewarding activities.
    
    2. Enhanced privacy and security: As AI becomes more advanced, it will likely require more data to process and analyze. However, this increased data collection


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

    职

    種

    /

    教育

    水平

    ]

     student

     at

     [

    School

     Name

    ]

    .
    


    As

     an

     [

    职

    種

    /

    教育

    水平

    ]

     student

    ,

     I

    'm

     passionate

     about

     [

    職

    業

    夢

    想

    ]

     and

     I

    'm

     constantly

     learning

     and

     growing

    .

     I

    'm

     [

    age

    ]

     years

     old

    ,

     and

     I

    've

     always

     been

     interested

     in

     [

    細

    分

    領域

    /

    興趣

    /

    目標

    ]

     because

     [

    reason

     for

     interest

    ].

     I

    'm

     also

     [

    my

     current

     role

    ,

     if

     applicable

    ].
    


    In

     my

     spare

     time

    ,

     I

     love

     [

    my

     hobbies

    ,

     if

     any

    ].
    


    I

    'm

     always

     looking

     for

     challenges

     and

     opportunities

     to

     grow

     and

     improve

    ,

     and

     I

    'm

     always

     eager

     to

     learn

     new

     things

    .

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     known

     for

     its

     iconic

     landmarks

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

     Located

     in

     the

     south

     of

     the

     country

    ,

     Paris

     is

     the

     political

    ,

     cultural

    ,

     and

     financial

     center

     of

     France

    .

     It

     is

     a

     major

     economic

     and

     cultural

     hub

    ,

     known

     for

     its

     rich

     history

     and

     unique

     cultural

     traditions

    ,

     including

     its

     influence

     on

     modern

     French

     cuisine

    ,

     fashion

    ,

     and

     theater

    .

     It

     is

     home

     to

     numerous

     museums

    ,

     art

     galleries

    ,

     and

     notable

     landmarks

    ,

     making

     it

     a

     must

    -

    visit

     destination

     for

     anyone

     traveling

     to

     France

    .

     Paris

     is

     also

     a

     popular

     tourist

     destination

    ,

     with

     a

     large

     population

     and

     a

     rich

     cultural

     heritage

     that

     reflects

     the

     country

    's

     unique

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     rapidly

     evolving

    ,

     and

     there

     are

     several

     potential

     trends

     that

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

    .

     Here

     are

     some

     of

     the

     most

     likely

     trends

    :
    


    1

    .

     Increased

     specialization

    :

     AI

     is

     becoming

     more

     specialized

    ,

     with

     researchers

     and

     developers

     focusing

     on

     specific

     tasks

     such

     as

     language

     translation

    ,

     image

     recognition

    ,

     and

     natural

     language

     processing

    .

     As

     these

     specialized

     areas

     become

     more

     focused

    ,

     the

     complexity

     of

     AI

     systems

     will

     increase

    .
    


    2

    .

     Better

     privacy

     and

     security

    :

     As

     AI

     systems

     become

     more

     integrated

     into

     our

     daily

     lives

    ,

     there

     will

     be

     a

     greater

     need

     for

     privacy

     and

     security

    .

     Developers

     will

     need

     to

     create

     AI

     systems

     that

     are

     more

     transparent

     and

     secure

    .
    


    3

    .

     AI

    -driven

     healthcare

    :

     AI

    



```python
llm.shutdown()
```
