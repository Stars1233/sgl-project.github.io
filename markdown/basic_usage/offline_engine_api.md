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


    2026-04-02 09:00:51.193 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:00:51] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 09:00:51.193 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:00:51] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 09:00:51.193 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:00:51] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 09:00:51.193 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:00:51] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 09:00:51.194 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:00:51] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  4.26it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  4.25it/s]


    2026-04-02 09:00:56,584 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 09:00:56] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:31,  2.66s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:31,  2.66s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<01:05,  1.17s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<01:05,  1.17s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<01:05,  1.17s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:25,  2.09it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:25,  2.09it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:25,  2.09it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:03<00:14,  3.59it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:03<00:14,  3.59it/s]Compiling num tokens (num_tokens=4608):  10%|█         | 6/58 [00:03<00:14,  3.59it/s]Compiling num tokens (num_tokens=4096):  10%|█         | 6/58 [00:03<00:14,  3.59it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:03<00:07,  6.32it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:03<00:07,  6.32it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:03<00:07,  6.32it/s]Compiling num tokens (num_tokens=3328):  16%|█▌        | 9/58 [00:03<00:07,  6.32it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:03<00:04,  9.30it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:03<00:04,  9.30it/s]Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:03<00:04,  9.30it/s]Compiling num tokens (num_tokens=2560):  21%|██        | 12/58 [00:03<00:04,  9.30it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:03<00:03, 12.37it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:03<00:03, 12.37it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:03<00:03, 12.37it/s]Compiling num tokens (num_tokens=1792):  26%|██▌       | 15/58 [00:03<00:03, 12.37it/s]Compiling num tokens (num_tokens=1536):  26%|██▌       | 15/58 [00:03<00:03, 12.37it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:02, 16.76it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:02, 16.76it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:02, 16.76it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:02, 16.76it/s] 

    Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:02, 16.76it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:03<00:01, 20.39it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:03<00:01, 20.39it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:03<00:01, 20.39it/s]Compiling num tokens (num_tokens=704):  40%|███▉      | 23/58 [00:03<00:01, 20.39it/s]Compiling num tokens (num_tokens=640):  40%|███▉      | 23/58 [00:03<00:01, 20.39it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:03<00:01, 23.76it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:03<00:01, 23.76it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:03<00:01, 23.76it/s]

    Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:03<00:01, 23.76it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:03<00:01, 23.76it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:03<00:01, 25.25it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:03<00:01, 25.25it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:03<00:01, 25.25it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:03<00:01, 25.25it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:03<00:01, 25.25it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:03<00:00, 27.48it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:03<00:00, 27.48it/s]

    Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:04<00:00, 27.48it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:04<00:00, 27.48it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:04<00:00, 27.48it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:04<00:00, 28.77it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:04<00:00, 28.77it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:04<00:00, 28.77it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:04<00:00, 28.77it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:04<00:00, 28.77it/s]

    Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:04<00:00, 30.77it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:04<00:00, 30.77it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:04<00:00, 30.77it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:04<00:00, 30.77it/s]Compiling num tokens (num_tokens=96):  74%|███████▍  | 43/58 [00:04<00:00, 30.77it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:04<00:00, 31.29it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:04<00:00, 31.29it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:04<00:00, 31.29it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:04<00:00, 31.29it/s]

    Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:04<00:00, 31.29it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:04<00:00, 32.31it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:04<00:00, 32.31it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:04<00:00, 32.31it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:04<00:00, 32.31it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:04<00:00, 32.31it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:04<00:00, 32.31it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:04<00:00, 35.08it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:04<00:00, 35.08it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:04<00:00, 35.08it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 12.56it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=59.25 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=59.25 GB):   2%|▏         | 1/58 [00:00<00:09,  5.79it/s]Capturing num tokens (num_tokens=7680 avail_mem=59.22 GB):   2%|▏         | 1/58 [00:00<00:09,  5.79it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=59.22 GB):   3%|▎         | 2/58 [00:00<00:10,  5.48it/s]Capturing num tokens (num_tokens=7168 avail_mem=59.22 GB):   3%|▎         | 2/58 [00:00<00:10,  5.48it/s]Capturing num tokens (num_tokens=7168 avail_mem=59.22 GB):   5%|▌         | 3/58 [00:00<00:07,  6.88it/s]Capturing num tokens (num_tokens=6656 avail_mem=59.21 GB):   5%|▌         | 3/58 [00:00<00:07,  6.88it/s]Capturing num tokens (num_tokens=6144 avail_mem=59.22 GB):   5%|▌         | 3/58 [00:00<00:07,  6.88it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=59.22 GB):   9%|▊         | 5/58 [00:00<00:05,  9.07it/s]Capturing num tokens (num_tokens=5632 avail_mem=59.21 GB):   9%|▊         | 5/58 [00:00<00:05,  9.07it/s]Capturing num tokens (num_tokens=5120 avail_mem=59.21 GB):   9%|▊         | 5/58 [00:00<00:05,  9.07it/s]Capturing num tokens (num_tokens=5120 avail_mem=59.21 GB):  12%|█▏        | 7/58 [00:00<00:04, 11.18it/s]Capturing num tokens (num_tokens=4608 avail_mem=59.21 GB):  12%|█▏        | 7/58 [00:00<00:04, 11.18it/s]Capturing num tokens (num_tokens=4096 avail_mem=59.21 GB):  12%|█▏        | 7/58 [00:00<00:04, 11.18it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=59.21 GB):  16%|█▌        | 9/58 [00:00<00:03, 13.22it/s]Capturing num tokens (num_tokens=3840 avail_mem=59.20 GB):  16%|█▌        | 9/58 [00:00<00:03, 13.22it/s]Capturing num tokens (num_tokens=3584 avail_mem=59.20 GB):  16%|█▌        | 9/58 [00:00<00:03, 13.22it/s]Capturing num tokens (num_tokens=3584 avail_mem=59.20 GB):  19%|█▉        | 11/58 [00:01<00:03, 12.66it/s]Capturing num tokens (num_tokens=3328 avail_mem=59.20 GB):  19%|█▉        | 11/58 [00:01<00:03, 12.66it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=59.19 GB):  19%|█▉        | 11/58 [00:01<00:03, 12.66it/s]Capturing num tokens (num_tokens=3072 avail_mem=59.19 GB):  22%|██▏       | 13/58 [00:01<00:03, 13.97it/s]Capturing num tokens (num_tokens=2816 avail_mem=59.19 GB):  22%|██▏       | 13/58 [00:01<00:03, 13.97it/s]Capturing num tokens (num_tokens=2560 avail_mem=59.18 GB):  22%|██▏       | 13/58 [00:01<00:03, 13.97it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=59.18 GB):  26%|██▌       | 15/58 [00:01<00:03, 12.72it/s]Capturing num tokens (num_tokens=2304 avail_mem=59.18 GB):  26%|██▌       | 15/58 [00:01<00:03, 12.72it/s]Capturing num tokens (num_tokens=2048 avail_mem=59.17 GB):  26%|██▌       | 15/58 [00:01<00:03, 12.72it/s]Capturing num tokens (num_tokens=1792 avail_mem=59.17 GB):  26%|██▌       | 15/58 [00:01<00:03, 12.72it/s]Capturing num tokens (num_tokens=1536 avail_mem=59.17 GB):  26%|██▌       | 15/58 [00:01<00:03, 12.72it/s]Capturing num tokens (num_tokens=1280 avail_mem=59.16 GB):  26%|██▌       | 15/58 [00:01<00:03, 12.72it/s]Capturing num tokens (num_tokens=1280 avail_mem=59.16 GB):  34%|███▍      | 20/58 [00:01<00:01, 21.06it/s]Capturing num tokens (num_tokens=1024 avail_mem=59.14 GB):  34%|███▍      | 20/58 [00:01<00:01, 21.06it/s]Capturing num tokens (num_tokens=960 avail_mem=59.16 GB):  34%|███▍      | 20/58 [00:01<00:01, 21.06it/s] Capturing num tokens (num_tokens=896 avail_mem=59.16 GB):  34%|███▍      | 20/58 [00:01<00:01, 21.06it/s]Capturing num tokens (num_tokens=832 avail_mem=59.15 GB):  34%|███▍      | 20/58 [00:01<00:01, 21.06it/s]Capturing num tokens (num_tokens=768 avail_mem=59.15 GB):  34%|███▍      | 20/58 [00:01<00:01, 21.06it/s]

    Capturing num tokens (num_tokens=704 avail_mem=59.15 GB):  34%|███▍      | 20/58 [00:01<00:01, 21.06it/s]Capturing num tokens (num_tokens=704 avail_mem=59.15 GB):  45%|████▍     | 26/58 [00:01<00:01, 29.08it/s]Capturing num tokens (num_tokens=640 avail_mem=59.14 GB):  45%|████▍     | 26/58 [00:01<00:01, 29.08it/s]Capturing num tokens (num_tokens=576 avail_mem=59.14 GB):  45%|████▍     | 26/58 [00:01<00:01, 29.08it/s]Capturing num tokens (num_tokens=512 avail_mem=59.13 GB):  45%|████▍     | 26/58 [00:01<00:01, 29.08it/s]Capturing num tokens (num_tokens=480 avail_mem=59.15 GB):  45%|████▍     | 26/58 [00:01<00:01, 29.08it/s]Capturing num tokens (num_tokens=480 avail_mem=59.15 GB):  52%|█████▏    | 30/58 [00:01<00:00, 31.01it/s]Capturing num tokens (num_tokens=448 avail_mem=59.14 GB):  52%|█████▏    | 30/58 [00:01<00:00, 31.01it/s]Capturing num tokens (num_tokens=416 avail_mem=59.14 GB):  52%|█████▏    | 30/58 [00:01<00:00, 31.01it/s]Capturing num tokens (num_tokens=384 avail_mem=59.14 GB):  52%|█████▏    | 30/58 [00:01<00:00, 31.01it/s]

    Capturing num tokens (num_tokens=352 avail_mem=57.94 GB):  52%|█████▏    | 30/58 [00:01<00:00, 31.01it/s]Capturing num tokens (num_tokens=352 avail_mem=57.94 GB):  59%|█████▊    | 34/58 [00:01<00:01, 22.68it/s]Capturing num tokens (num_tokens=320 avail_mem=58.00 GB):  59%|█████▊    | 34/58 [00:01<00:01, 22.68it/s]Capturing num tokens (num_tokens=288 avail_mem=59.09 GB):  59%|█████▊    | 34/58 [00:02<00:01, 22.68it/s]Capturing num tokens (num_tokens=256 avail_mem=59.09 GB):  59%|█████▊    | 34/58 [00:02<00:01, 22.68it/s]

    Capturing num tokens (num_tokens=256 avail_mem=59.09 GB):  64%|██████▍   | 37/58 [00:02<00:00, 21.07it/s]Capturing num tokens (num_tokens=240 avail_mem=58.10 GB):  64%|██████▍   | 37/58 [00:02<00:00, 21.07it/s]Capturing num tokens (num_tokens=224 avail_mem=58.10 GB):  64%|██████▍   | 37/58 [00:02<00:00, 21.07it/s]Capturing num tokens (num_tokens=208 avail_mem=58.10 GB):  64%|██████▍   | 37/58 [00:02<00:00, 21.07it/s]

    Capturing num tokens (num_tokens=208 avail_mem=58.10 GB):  69%|██████▉   | 40/58 [00:02<00:01, 17.32it/s]Capturing num tokens (num_tokens=192 avail_mem=59.08 GB):  69%|██████▉   | 40/58 [00:02<00:01, 17.32it/s]Capturing num tokens (num_tokens=176 avail_mem=58.16 GB):  69%|██████▉   | 40/58 [00:02<00:01, 17.32it/s]

    Capturing num tokens (num_tokens=160 avail_mem=58.31 GB):  69%|██████▉   | 40/58 [00:02<00:01, 17.32it/s]Capturing num tokens (num_tokens=160 avail_mem=58.31 GB):  74%|███████▍  | 43/58 [00:02<00:01, 14.59it/s]Capturing num tokens (num_tokens=144 avail_mem=59.08 GB):  74%|███████▍  | 43/58 [00:02<00:01, 14.59it/s]Capturing num tokens (num_tokens=128 avail_mem=58.22 GB):  74%|███████▍  | 43/58 [00:02<00:01, 14.59it/s]

    Capturing num tokens (num_tokens=128 avail_mem=58.22 GB):  78%|███████▊  | 45/58 [00:02<00:00, 13.82it/s]Capturing num tokens (num_tokens=112 avail_mem=58.22 GB):  78%|███████▊  | 45/58 [00:02<00:00, 13.82it/s]Capturing num tokens (num_tokens=96 avail_mem=59.08 GB):  78%|███████▊  | 45/58 [00:02<00:00, 13.82it/s] Capturing num tokens (num_tokens=96 avail_mem=59.08 GB):  81%|████████  | 47/58 [00:02<00:00, 14.31it/s]Capturing num tokens (num_tokens=80 avail_mem=58.28 GB):  81%|████████  | 47/58 [00:02<00:00, 14.31it/s]

    Capturing num tokens (num_tokens=64 avail_mem=58.28 GB):  81%|████████  | 47/58 [00:03<00:00, 14.31it/s]Capturing num tokens (num_tokens=64 avail_mem=58.28 GB):  84%|████████▍ | 49/58 [00:03<00:00, 13.64it/s]Capturing num tokens (num_tokens=48 avail_mem=58.28 GB):  84%|████████▍ | 49/58 [00:03<00:00, 13.64it/s]Capturing num tokens (num_tokens=32 avail_mem=59.06 GB):  84%|████████▍ | 49/58 [00:03<00:00, 13.64it/s]

    Capturing num tokens (num_tokens=32 avail_mem=59.06 GB):  88%|████████▊ | 51/58 [00:03<00:00, 13.87it/s]Capturing num tokens (num_tokens=28 avail_mem=58.33 GB):  88%|████████▊ | 51/58 [00:03<00:00, 13.87it/s]Capturing num tokens (num_tokens=24 avail_mem=58.33 GB):  88%|████████▊ | 51/58 [00:03<00:00, 13.87it/s]Capturing num tokens (num_tokens=24 avail_mem=58.33 GB):  91%|█████████▏| 53/58 [00:03<00:00, 14.17it/s]Capturing num tokens (num_tokens=20 avail_mem=59.05 GB):  91%|█████████▏| 53/58 [00:03<00:00, 14.17it/s]

    Capturing num tokens (num_tokens=16 avail_mem=58.39 GB):  91%|█████████▏| 53/58 [00:03<00:00, 14.17it/s]Capturing num tokens (num_tokens=16 avail_mem=58.39 GB):  95%|█████████▍| 55/58 [00:03<00:00, 13.77it/s]Capturing num tokens (num_tokens=12 avail_mem=58.39 GB):  95%|█████████▍| 55/58 [00:03<00:00, 13.77it/s]Capturing num tokens (num_tokens=8 avail_mem=59.05 GB):  95%|█████████▍| 55/58 [00:03<00:00, 13.77it/s] 

    Capturing num tokens (num_tokens=8 avail_mem=59.05 GB):  98%|█████████▊| 57/58 [00:03<00:00, 14.51it/s]Capturing num tokens (num_tokens=4 avail_mem=58.45 GB):  98%|█████████▊| 57/58 [00:03<00:00, 14.51it/s]Capturing num tokens (num_tokens=4 avail_mem=58.45 GB): 100%|██████████| 58/58 [00:03<00:00, 15.44it/s]


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
    Generated text:  Bob and I am an attorney who has been practicing law for many years. I specialize in family law and my clients include family members and children.
    
    What is my first step in a divorce case?
    
    The first step in a divorce case is often to reach a settlement with the other party. In many cases, the parties do not agree to settle, but if they do, a court will be appointed to hear their case and make a decision.
    
    What does the court do in a divorce case?
    
    In a divorce case, the court will listen to the parties' arguments and decide which side of the case will win. If the parties agree on the
    ===============================
    Prompt: The president of the United States is
    Generated text:  very popular. He is the President of the United States. He is a very important man. It is his job to be the President of the United States. He is the leader of the United States. He is always busy. Sometimes he is very busy. He is very busy. He likes to eat many kinds of food. Sometimes he is very happy. He likes to eat many kinds of food. Sometimes he is very happy. His favorite color is blue. He likes to wear blue clothes. He is very tall. He likes to talk a lot. Sometimes he talks a lot. He is very patient. He is very smart.
    ===============================
    Prompt: The capital of France is
    Generated text:  located in the country's south. It is the third largest city in the country. Its population is about 3 million. It has many well-known landmarks, like the Eiffel Tower, Notre-Dame Cathedral and the Louvre Museum.
    
    Does it follow that if "The capital of France is not located in the country's south"?
    Select from the following.
     A). yes
     B). it is not possible to tell
     C). no
    C).
    ===============================
    Prompt: The future of AI is
    Generated text:  in the hands of the human beings. A team of researchers has designed a new machine learning algorithm that can interpret and translate the human language, and hence interpret the human's emotions and thoughts and understand the underlying meaning of a person's words. This new algorithm is called Multi-Modal Interpretative Language Understanding (MILE). The researchers at the University of Michigan have already developed and tested this algorithm with a large set of data, and are confident that it can achieve a high level of accuracy in interpreting human language. The researchers have also conducted extensive testing on the MILE algorithm, and have found that it can accurately interpret human emotions and thoughts,


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [insert a brief description of your character, such as "fun-loving, adventurous, or creative"]. I enjoy [insert a short description of your hobbies or interests, such as "reading, hiking, or playing music"]. I'm always looking for new experiences and challenges, and I'm always eager to learn and grow. What do you like to do in your free time? I enjoy [insert a short description of your hobbies or interests, such
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also a major cultural and economic center, hosting numerous museums, theaters, and festivals throughout the year. Paris is a popular tourist destination and a major hub for international business and diplomacy. The city is known for its rich history, including the influence of French Revolution and Napoleon Bonaparte, and its role in shaping modern French culture and politics. Paris is a city of contrasts, with its modern architecture and vibrant nightlife, as well as its historical charm and cultural heritage. The city is a UNESCO World Heritage
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by rapid advancements in areas such as machine learning, natural language processing, and computer vision. These technologies are expected to continue to evolve and improve, leading to more sophisticated and accurate AI systems that can perform a wide range of tasks with increasing accuracy and efficiency. Some potential future trends in AI include:
    
    1. Increased focus on ethical considerations: As AI systems become more advanced, there will be a growing emphasis on ensuring that they are used ethically and responsibly. This may include developing guidelines and standards for AI development and deployment, as well as addressing issues such as bias, transparency, and accountability.
    
    2. Greater integration with human
    


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
    Generated text:  ____. I am a/an ____. I have been writing about ____. I am currently living and working in ____. I enjoy writing ____. I have a passion for ____. What are your thoughts on ____?
    
    Please write a self-introduction in a tone that is neutral and respectful. Your introduction should include your name, profession, your main goal in writing, the context in which you have been working, and your personal passion or interest in the topic of your writing. Additionally, please include a brief summary of your personal life and how it has shaped your writing style. Finally, leave your personal opinion on the topic at hand.
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. It is a popular tourist destination and a major center of French culture and economy.
    
    What is the capital of Brazil? The capital of Brazil is Brasilia. It is the capital and largest city of Brazil. Brasilia is located in the state of Rio de Janeiro and is home to the federal government, the Ministry of the Interior, and many of Brazil's other important government offices. The city is also a popular tourist destination, known for its colorful buildings and vibrant culture. However, the capital city of Brazil is currently experiencing a change in leadership and has been renamed to Rio de Janeiro. Despite this, Brasilia remains a popular destination
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to involve a range of trends and developments, including:
    
    1. More advanced algorithms: The development of more complex and sophisticated algorithms will enable AI systems to learn from more data and make better predictions and decisions.
    
    2. Increased integration with human intelligence: AI systems will become more integrated with human intelligence, leading to a more intelligent and adaptive AI environment.
    
    3. Enhanced ethical considerations: As AI systems become more advanced and complex, there will be a need for more ethical considerations and regulations to ensure that AI is used responsibly.
    
    4. AI-driven automation: AI is likely to become a dominant force in the workforce, automating tasks and reducing the


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

     [

    short

    ,

     engaging

     personal

     pron

    oun

    ]

     from

     [

    Country

    ].

     I

     have

     [

    number

    ]

     years

     of

     experience

     in

     [

    industry

    ],

     and

     I

     am

     known

     for

     my

     [

    major

     accomplishment

    ].

     Whether

     I

    'm

     [

    role

     

    1

    ]

     or

     [

    role

     

    2

    ],

     I

     am

     [

    role

     

    1

    /

    2

    ].

     I

     am

     [

    age

    ],

     and

     I

     love

     [

    d

    ual

     purpose

    ],

     [

    what

     you

    're

     most

     passionate

     about

    ].

     I

     am

     [

    your

     best

     friend

    ],

     [

    you

    're

     an

     athlete

    ],

     or

     [

    you

    're

     a

     [

    special

    ty

    ]

     expert

    ].

     I

     am

     [

    your

     best

     friend

    ],

     [

    you

    're

     an

     athlete

    ],

     or

     [

    you

    're

     a

     [

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .


    Paris

     is

     the

     largest

     city

     in

     France

     and

     one

     of

     the

     largest

     in

     the

     world

    .

     It

     has

     a

     rich

     history

    ,

     including

     being

     the

     capital

     of

     France

     from

     

    1

    8

    0

    4

     to

     

    1

    9

    6

    9

    ,

     and

     is

     home

     to

     various

     museums

    ,

     theaters

    ,

     and

     fashion

     designers

    .

     Paris

     is

     also

     known

     for

     its

     culture

    ,

     including

     the

     famous

     E

    iff

    el

     Tower

     and

     the

     Notre

    -D

    ame

     Cathedral

    .

     The

     city

     is

     home

     to

     many

     famous

     landmarks

     and

     is

     a

     popular

     tourist

     destination

    .

     Paris

     has

     a

     diverse

     population

     and

     culture

    ,

     making

     it

     an

     exciting

     and

     multicultural

     city

    .

     France

    's

     capital

     city

     is

     Paris

    .

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     shaped

     by

     a

     number

     of

     different

     trends

     and

     technologies

     that

     are

     currently

     emerging

    .

     Here

     are

     some

     potential

     trends

     that

     have

     the

     potential

     to

     shape

     the

     future

     of

     AI

    :
    


    1

    .

     Deep

     Learning

    :

     Deep

     learning

     is

     the

     branch

     of

     machine

     learning

     that

     focuses

     on

     building

     artificial

     neural

     networks

     that

     can

     learn

     from

     large

     and

     complex

     datasets

    .

     As

     the

     size

     and

     complexity

     of

     data

     sets

     continue

     to

     increase

    ,

     the

     power

     of

     deep

     learning

     is

     likely

     to

     grow

    ,

     allowing

     machines

     to

     learn

     and

     solve

     increasingly

     complex

     problems

    .
    


    2

    .

     Speech

     and

     Vision

     Technologies

    :

     As

     voice

     assistants

     and

     facial

     recognition

     technology

     become

     more

     advanced

    ,

     we

     can

     expect

     to

     see

     more

     sophisticated

     methods

     for

     processing

     speech

     and

     vision

     data

    .

     This

    



```python
llm.shutdown()
```
