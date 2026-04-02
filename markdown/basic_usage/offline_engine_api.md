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


    2026-04-02 00:57:54.034 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 00:57:54] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 00:57:54.034 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 00:57:54] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 00:57:54.034 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 00:57:54] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 00:57:54.034 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 00:57:54] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 00:57:54.034 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 00:57:54] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  3.24it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  3.24it/s]


    2026-04-02 00:57:56,775 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 00:57:56] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:37,  2.77s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:37,  2.77s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<01:09,  1.24s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<01:09,  1.24s/it]

    Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:03<01:09,  1.24s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:27,  1.95it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:27,  1.95it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:03<00:27,  1.95it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:03<00:15,  3.32it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:03<00:15,  3.32it/s]Compiling num tokens (num_tokens=4608):  10%|█         | 6/58 [00:03<00:15,  3.32it/s]Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:03<00:10,  4.92it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:03<00:10,  4.92it/s]Compiling num tokens (num_tokens=3840):  14%|█▍        | 8/58 [00:03<00:10,  4.92it/s]Compiling num tokens (num_tokens=3584):  14%|█▍        | 8/58 [00:03<00:10,  4.92it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:03<00:05,  7.84it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:03<00:05,  7.84it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:03<00:05,  7.84it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:03<00:05,  7.84it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:03<00:04, 10.87it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:03<00:04, 10.87it/s]Compiling num tokens (num_tokens=2304):  24%|██▍       | 14/58 [00:03<00:04, 10.87it/s]Compiling num tokens (num_tokens=2048):  24%|██▍       | 14/58 [00:03<00:04, 10.87it/s]

    Compiling num tokens (num_tokens=1792):  24%|██▍       | 14/58 [00:03<00:04, 10.87it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:02, 14.99it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:02, 14.99it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:02, 14.99it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:02, 14.99it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:03<00:02, 17.58it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:03<00:02, 17.58it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:03<00:02, 17.58it/s]

    Compiling num tokens (num_tokens=832):  36%|███▌      | 21/58 [00:03<00:02, 17.58it/s]Compiling num tokens (num_tokens=768):  36%|███▌      | 21/58 [00:03<00:02, 17.58it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 21.29it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 21.29it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 21.29it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:04<00:01, 21.29it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:04<00:01, 23.13it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:04<00:01, 23.13it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:04<00:01, 23.13it/s]

    Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:04<00:01, 23.13it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:04<00:01, 24.63it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:04<00:01, 24.63it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:04<00:01, 24.63it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:04<00:01, 24.63it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:04<00:01, 24.63it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:04<00:00, 27.70it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:04<00:00, 27.70it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:04<00:00, 27.70it/s]

    Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:04<00:00, 27.70it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:04<00:00, 27.70it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:04<00:00, 29.18it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:04<00:00, 29.18it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:04<00:00, 29.18it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:04<00:00, 29.18it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:04<00:00, 29.18it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:04<00:00, 31.34it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:04<00:00, 31.34it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:04<00:00, 31.34it/s]

    Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:04<00:00, 31.34it/s]Compiling num tokens (num_tokens=96):  74%|███████▍  | 43/58 [00:04<00:00, 31.34it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:04<00:00, 29.91it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:04<00:00, 29.91it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:04<00:00, 29.91it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:04<00:00, 29.91it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:04<00:00, 29.91it/s]

    Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:04<00:00, 29.65it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:04<00:00, 29.65it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:04<00:00, 29.65it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:04<00:00, 29.65it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:04<00:00, 29.65it/s]Compiling num tokens (num_tokens=16):  95%|█████████▍| 55/58 [00:04<00:00, 30.82it/s]Compiling num tokens (num_tokens=12):  95%|█████████▍| 55/58 [00:04<00:00, 30.82it/s]Compiling num tokens (num_tokens=8):  95%|█████████▍| 55/58 [00:04<00:00, 30.82it/s] Compiling num tokens (num_tokens=4):  95%|█████████▍| 55/58 [00:04<00:00, 30.82it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 11.67it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=53.63 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=53.63 GB):   2%|▏         | 1/58 [00:00<00:11,  5.09it/s]Capturing num tokens (num_tokens=7680 avail_mem=53.63 GB):   2%|▏         | 1/58 [00:00<00:11,  5.09it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=53.63 GB):   3%|▎         | 2/58 [00:00<00:10,  5.43it/s]Capturing num tokens (num_tokens=7168 avail_mem=53.96 GB):   3%|▎         | 2/58 [00:00<00:10,  5.43it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=53.96 GB):   5%|▌         | 3/58 [00:00<00:10,  5.20it/s]Capturing num tokens (num_tokens=6656 avail_mem=53.96 GB):   5%|▌         | 3/58 [00:00<00:10,  5.20it/s]Capturing num tokens (num_tokens=6656 avail_mem=53.96 GB):   7%|▋         | 4/58 [00:00<00:09,  5.59it/s]Capturing num tokens (num_tokens=6144 avail_mem=53.73 GB):   7%|▋         | 4/58 [00:00<00:09,  5.59it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=53.73 GB):   9%|▊         | 5/58 [00:00<00:08,  5.90it/s]Capturing num tokens (num_tokens=5632 avail_mem=53.96 GB):   9%|▊         | 5/58 [00:00<00:08,  5.90it/s]Capturing num tokens (num_tokens=5632 avail_mem=53.96 GB):  10%|█         | 6/58 [00:01<00:08,  6.33it/s]Capturing num tokens (num_tokens=5120 avail_mem=53.96 GB):  10%|█         | 6/58 [00:01<00:08,  6.33it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=53.96 GB):  12%|█▏        | 7/58 [00:01<00:07,  6.69it/s]Capturing num tokens (num_tokens=4608 avail_mem=53.96 GB):  12%|█▏        | 7/58 [00:01<00:07,  6.69it/s]Capturing num tokens (num_tokens=4608 avail_mem=53.96 GB):  14%|█▍        | 8/58 [00:01<00:07,  7.14it/s]Capturing num tokens (num_tokens=4096 avail_mem=53.80 GB):  14%|█▍        | 8/58 [00:01<00:07,  7.14it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=53.80 GB):  16%|█▌        | 9/58 [00:01<00:06,  7.61it/s]Capturing num tokens (num_tokens=3840 avail_mem=53.95 GB):  16%|█▌        | 9/58 [00:01<00:06,  7.61it/s]Capturing num tokens (num_tokens=3840 avail_mem=53.95 GB):  17%|█▋        | 10/58 [00:01<00:05,  8.15it/s]Capturing num tokens (num_tokens=3584 avail_mem=53.94 GB):  17%|█▋        | 10/58 [00:01<00:05,  8.15it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=53.85 GB):  17%|█▋        | 10/58 [00:01<00:05,  8.15it/s]Capturing num tokens (num_tokens=3328 avail_mem=53.85 GB):  21%|██        | 12/58 [00:01<00:05,  9.02it/s]Capturing num tokens (num_tokens=3072 avail_mem=53.93 GB):  21%|██        | 12/58 [00:01<00:05,  9.02it/s]Capturing num tokens (num_tokens=2816 avail_mem=53.93 GB):  21%|██        | 12/58 [00:01<00:05,  9.02it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=53.93 GB):  24%|██▍       | 14/58 [00:01<00:04,  9.61it/s]Capturing num tokens (num_tokens=2560 avail_mem=53.92 GB):  24%|██▍       | 14/58 [00:01<00:04,  9.61it/s]Capturing num tokens (num_tokens=2304 avail_mem=53.91 GB):  24%|██▍       | 14/58 [00:01<00:04,  9.61it/s]Capturing num tokens (num_tokens=2304 avail_mem=53.91 GB):  28%|██▊       | 16/58 [00:02<00:04, 10.20it/s]Capturing num tokens (num_tokens=2048 avail_mem=53.91 GB):  28%|██▊       | 16/58 [00:02<00:04, 10.20it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=53.90 GB):  28%|██▊       | 16/58 [00:02<00:04, 10.20it/s]Capturing num tokens (num_tokens=1792 avail_mem=53.90 GB):  31%|███       | 18/58 [00:02<00:03, 10.62it/s]Capturing num tokens (num_tokens=1536 avail_mem=53.88 GB):  31%|███       | 18/58 [00:02<00:03, 10.62it/s]Capturing num tokens (num_tokens=1280 avail_mem=53.89 GB):  31%|███       | 18/58 [00:02<00:03, 10.62it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=53.89 GB):  34%|███▍      | 20/58 [00:02<00:03, 10.49it/s]Capturing num tokens (num_tokens=1024 avail_mem=53.86 GB):  34%|███▍      | 20/58 [00:02<00:03, 10.49it/s]Capturing num tokens (num_tokens=960 avail_mem=53.88 GB):  34%|███▍      | 20/58 [00:02<00:03, 10.49it/s] Capturing num tokens (num_tokens=960 avail_mem=53.88 GB):  38%|███▊      | 22/58 [00:02<00:03, 11.00it/s]Capturing num tokens (num_tokens=896 avail_mem=53.87 GB):  38%|███▊      | 22/58 [00:02<00:03, 11.00it/s]

    Capturing num tokens (num_tokens=832 avail_mem=53.87 GB):  38%|███▊      | 22/58 [00:02<00:03, 11.00it/s]Capturing num tokens (num_tokens=832 avail_mem=53.87 GB):  41%|████▏     | 24/58 [00:02<00:02, 11.44it/s]Capturing num tokens (num_tokens=768 avail_mem=53.86 GB):  41%|████▏     | 24/58 [00:02<00:02, 11.44it/s]Capturing num tokens (num_tokens=704 avail_mem=53.87 GB):  41%|████▏     | 24/58 [00:02<00:02, 11.44it/s]

    Capturing num tokens (num_tokens=704 avail_mem=53.87 GB):  45%|████▍     | 26/58 [00:02<00:02, 11.75it/s]Capturing num tokens (num_tokens=640 avail_mem=53.84 GB):  45%|████▍     | 26/58 [00:02<00:02, 11.75it/s]Capturing num tokens (num_tokens=576 avail_mem=53.86 GB):  45%|████▍     | 26/58 [00:02<00:02, 11.75it/s]Capturing num tokens (num_tokens=576 avail_mem=53.86 GB):  48%|████▊     | 28/58 [00:03<00:02, 11.89it/s]Capturing num tokens (num_tokens=512 avail_mem=53.84 GB):  48%|████▊     | 28/58 [00:03<00:02, 11.89it/s]

    Capturing num tokens (num_tokens=480 avail_mem=53.86 GB):  48%|████▊     | 28/58 [00:03<00:02, 11.89it/s]Capturing num tokens (num_tokens=480 avail_mem=53.86 GB):  52%|█████▏    | 30/58 [00:03<00:02, 10.72it/s]Capturing num tokens (num_tokens=448 avail_mem=53.85 GB):  52%|█████▏    | 30/58 [00:03<00:02, 10.72it/s]Capturing num tokens (num_tokens=416 avail_mem=53.85 GB):  52%|█████▏    | 30/58 [00:03<00:02, 10.72it/s]

    Capturing num tokens (num_tokens=416 avail_mem=53.85 GB):  55%|█████▌    | 32/58 [00:03<00:02, 11.04it/s]Capturing num tokens (num_tokens=384 avail_mem=53.84 GB):  55%|█████▌    | 32/58 [00:03<00:02, 11.04it/s]Capturing num tokens (num_tokens=352 avail_mem=53.83 GB):  55%|█████▌    | 32/58 [00:03<00:02, 11.04it/s]Capturing num tokens (num_tokens=352 avail_mem=53.83 GB):  59%|█████▊    | 34/58 [00:03<00:02, 11.26it/s]Capturing num tokens (num_tokens=320 avail_mem=53.83 GB):  59%|█████▊    | 34/58 [00:03<00:02, 11.26it/s]

    Capturing num tokens (num_tokens=288 avail_mem=53.82 GB):  59%|█████▊    | 34/58 [00:03<00:02, 11.26it/s]Capturing num tokens (num_tokens=288 avail_mem=53.82 GB):  62%|██████▏   | 36/58 [00:03<00:01, 11.39it/s]Capturing num tokens (num_tokens=256 avail_mem=53.81 GB):  62%|██████▏   | 36/58 [00:03<00:01, 11.39it/s]Capturing num tokens (num_tokens=240 avail_mem=53.81 GB):  62%|██████▏   | 36/58 [00:03<00:01, 11.39it/s]

    Capturing num tokens (num_tokens=240 avail_mem=53.81 GB):  66%|██████▌   | 38/58 [00:03<00:01, 11.61it/s]Capturing num tokens (num_tokens=224 avail_mem=53.80 GB):  66%|██████▌   | 38/58 [00:03<00:01, 11.61it/s]Capturing num tokens (num_tokens=208 avail_mem=53.79 GB):  66%|██████▌   | 38/58 [00:04<00:01, 11.61it/s]Capturing num tokens (num_tokens=208 avail_mem=53.79 GB):  69%|██████▉   | 40/58 [00:04<00:01, 11.76it/s]Capturing num tokens (num_tokens=192 avail_mem=53.79 GB):  69%|██████▉   | 40/58 [00:04<00:01, 11.76it/s]

    Capturing num tokens (num_tokens=176 avail_mem=53.79 GB):  69%|██████▉   | 40/58 [00:04<00:01, 11.76it/s]Capturing num tokens (num_tokens=176 avail_mem=53.79 GB):  72%|███████▏  | 42/58 [00:04<00:01, 11.59it/s]Capturing num tokens (num_tokens=160 avail_mem=53.78 GB):  72%|███████▏  | 42/58 [00:04<00:01, 11.59it/s]

    Capturing num tokens (num_tokens=144 avail_mem=53.78 GB):  72%|███████▏  | 42/58 [00:04<00:01, 11.59it/s]Capturing num tokens (num_tokens=144 avail_mem=53.78 GB):  76%|███████▌  | 44/58 [00:04<00:01, 10.19it/s]Capturing num tokens (num_tokens=128 avail_mem=53.78 GB):  76%|███████▌  | 44/58 [00:04<00:01, 10.19it/s]Capturing num tokens (num_tokens=112 avail_mem=53.77 GB):  76%|███████▌  | 44/58 [00:04<00:01, 10.19it/s]

    Capturing num tokens (num_tokens=112 avail_mem=53.77 GB):  79%|███████▉  | 46/58 [00:04<00:01, 10.73it/s]Capturing num tokens (num_tokens=96 avail_mem=53.77 GB):  79%|███████▉  | 46/58 [00:04<00:01, 10.73it/s] Capturing num tokens (num_tokens=80 avail_mem=53.77 GB):  79%|███████▉  | 46/58 [00:04<00:01, 10.73it/s]Capturing num tokens (num_tokens=80 avail_mem=53.77 GB):  83%|████████▎ | 48/58 [00:04<00:00, 11.09it/s]Capturing num tokens (num_tokens=64 avail_mem=53.76 GB):  83%|████████▎ | 48/58 [00:04<00:00, 11.09it/s]

    Capturing num tokens (num_tokens=48 avail_mem=53.76 GB):  83%|████████▎ | 48/58 [00:04<00:00, 11.09it/s]Capturing num tokens (num_tokens=48 avail_mem=53.76 GB):  86%|████████▌ | 50/58 [00:05<00:00, 11.04it/s]Capturing num tokens (num_tokens=32 avail_mem=53.76 GB):  86%|████████▌ | 50/58 [00:05<00:00, 11.04it/s]Capturing num tokens (num_tokens=28 avail_mem=53.75 GB):  86%|████████▌ | 50/58 [00:05<00:00, 11.04it/s]

    Capturing num tokens (num_tokens=28 avail_mem=53.75 GB):  90%|████████▉ | 52/58 [00:05<00:00, 11.33it/s]Capturing num tokens (num_tokens=24 avail_mem=53.75 GB):  90%|████████▉ | 52/58 [00:05<00:00, 11.33it/s]Capturing num tokens (num_tokens=20 avail_mem=53.74 GB):  90%|████████▉ | 52/58 [00:05<00:00, 11.33it/s]Capturing num tokens (num_tokens=20 avail_mem=53.74 GB):  93%|█████████▎| 54/58 [00:05<00:00, 11.53it/s]Capturing num tokens (num_tokens=16 avail_mem=53.74 GB):  93%|█████████▎| 54/58 [00:05<00:00, 11.53it/s]

    Capturing num tokens (num_tokens=12 avail_mem=53.74 GB):  93%|█████████▎| 54/58 [00:05<00:00, 11.53it/s]Capturing num tokens (num_tokens=12 avail_mem=53.74 GB):  97%|█████████▋| 56/58 [00:05<00:00, 11.72it/s]Capturing num tokens (num_tokens=8 avail_mem=53.74 GB):  97%|█████████▋| 56/58 [00:05<00:00, 11.72it/s] Capturing num tokens (num_tokens=4 avail_mem=53.73 GB):  97%|█████████▋| 56/58 [00:05<00:00, 11.72it/s]

    Capturing num tokens (num_tokens=4 avail_mem=53.73 GB): 100%|██████████| 58/58 [00:05<00:00, 12.07it/s]Capturing num tokens (num_tokens=4 avail_mem=53.73 GB): 100%|██████████| 58/58 [00:05<00:00, 10.14it/s]


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
    Generated text:  Steve and I was an engineer before I became a startup entrepreneur. If you have any questions about engineering, I'd be happy to answer them.
    What is the most interesting and memorable experience you have had in engineering?
    As an engineer, my most interesting and memorable experience was developing a new type of sensor for a wireless communication system. It was a challenging and complex task that required me to work closely with other engineers, researchers, and other stakeholders to ensure that the sensor would work effectively and meet the needs of the communication system.
    One of the most exciting parts of developing this sensor was the feedback we received from the research community. We received feedback
    ===============================
    Prompt: The president of the United States is
    Generated text:  a term of office for four years. Is there a president in the United States who has held the office for exactly 224 years? If so, what was the year of their inaugural date? To determine if there is a president in the United States who has held the office for exactly 224 years, we need to calculate the number of years of the president's term and check if it is exactly 224 years.
    
    First, let's understand the term of office. The president serves a four-year term. We need to find out how many four-year periods fit into 224 years. This can
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. It is located on the island of Île-de-France. The capital of France is a complex of cities and administrative districts. It is also the largest city in France and the second largest city in the world, after New York City.
    The capital is built around the city’s historical center, which includes many historic churches, castles, and castles. The city of Paris is one of the oldest continuously inhabited communities in the world, and has a rich history. It was founded around 500 B.C. The center of the city has a high density of historic buildings and monuments, including the Eiffel Tower
    ===============================
    Prompt: The future of AI is
    Generated text:  more than just robotics and machine learning. These are just the emerging frontiers of technology. The future of AI includes a whole lot more.
    In this post, we'll take a look at how artificial intelligence is changing the way we live and work. We'll also explore how AI is changing the way we interact with each other, the environment, and our society. We'll also look at the benefits and challenges of artificial intelligence and how it will shape the future of our lives.
    We'll also explore the future of AI in education and how it will revolutionize the way we learn and prepare for the future.
    We'll look at how


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


    Generated text:  Paris, which is known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and Louvre Museum. It is also home to the French Parliament, the French Academy of Sciences, and the French National Library. Paris is a bustling city with a rich history and culture, and it is a popular tourist destination. The city is known for its cuisine, fashion, and art, and it is home to many famous museums and landmarks. Paris is a city that is a true reflection of France's rich history and culture. It is a city that is a must-visit for anyone interested in French culture and history.
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends:
    
    1. Increased focus on ethical considerations: As AI becomes more integrated into our daily lives, there will be a growing emphasis on ethical considerations. This will include issues such as bias, transparency, accountability, and the impact of AI on society as a whole.
    
    2. Greater integration with other technologies: AI is likely to become more integrated with other technologies, such as machine learning, natural language processing, and computer vision. This will allow for more sophisticated and personalized interactions between humans and machines
    


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
    Generated text:  [Name], and I'm a [job title] at [company name]. I enjoy [reason for your job], and I strive to [specific goal or accomplishment]. In my free time, I like [something enjoyable or relaxing]. I'm excited to meet you and discuss how I can contribute to your team. Good luck! Let's get started! [Name] [Job Title] at [Company Name] [Company Name]: [Company Description]
    Hello, my name is [Name], and I'm a [job title] at [Company Name]. I enjoy [reason for your job], and I strive to [specific goal or
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    
    Paris is the capital city of France, known for its rich history, stunning architecture, and vibrant culture. The city is home to the Louvre Museum, the Eiffel Tower, and the Notre-Dame Cathedral, among other iconic landmarks. Paris is also famous for its annual Carnaval and for being the birthplace of French fries. As the heart of France, Paris plays a crucial role in the country's economy and culture. The city is home to several museums, including the National Museum of Modern Art and the Musée d'Orsay, and is a popular tourist destination, with millions of visitors each year. As
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  expected to be highly advanced and complex, with many different trends and possibilities emerging. Here are some potential future trends in AI:
    
    1. Autonomous and AI-powered vehicles: The advent of autonomous driving will likely revolutionize the transportation industry. AI-powered vehicles will be able to navigate roads, identify obstacles, and react to traffic signals in real-time. This will not only reduce traffic congestion and accidents but also improve efficiency and reduce carbon emissions.
    
    2. Personalized healthcare: AI-powered medical devices will be able to analyze patient data and provide personalized treatment plans. This will improve the accuracy and effectiveness of medical treatments, and will also reduce the cost of healthcare


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

    ].

     I

    'm

     a

     computer

     programmer

    ,

     and

     I

    'm

     excited

     to

     bring

     my

     skills

     to

     the

     world

     of

     game

     development

     and

     create

     innovative

     experiences

     that

     change

     the

     way

     we

     play

    .

     I

    'm

     always

     looking

     to

     improve

     my

     programming

     knowledge

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

     industry

    .

     I

     have

     a

     passion

     for

     creating

     games

     that

     are

     fun

     and

     engaging

    ,

     and

     I

    'm

     always

     striving

     to

     push

     the

     boundaries

     of

     what

    's

     possible

     in

     the

     gaming

     world

    .

     I

    'm

     a

     positive

    ,

     energetic

    ,

     and

     collaborative

     person

     who

     loves

     to

     work

     hard

     and

     make

     things

     happen

    .

     I

    'm

     looking

     forward

     to

     the

     opportunity

     to

     work

     with

     you

     and

     bring

     my

     skills

     to

     the

     table

    .

     [

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     
    


    Options

     are

    :


    A

    ).

     No

    


    B

    ).

     Yes

    


    B

    ).

     Yes

    


    Paris

     is

     the

     capital

     city

     of

     France

    .

     It

     is

     the

     largest

     city

     in

     the

     country

     and

     has

     a

     rich

     history

    .

     It

     is

     also

     one

     of

     the

     world

    's

     most

     popular

     tourist

     destinations

    .

     The

     city

     has

     a

     diverse

     population

     of

     around

     

    1

    0

     million

     people

    ,

     and

     it

     is

     home

     to

     many

     important

     institutions

     and

     landmarks

    .

     The

     city

     is

     also

     known

     for

     its

     culture

    ,

     cuisine

    ,

     and

     entertainment

    ,

     making

     it

     a

     popular

     tourist

     destination

    .

     Paris

     has

     many

     world

    -f

    amous

     landmarks

     such

     as

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

     Mont

    mart

    re

    .

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     highly

     uncertain

    ,

     and

     there

     are

     many

     possible

     trends

     that

     could

     occur

    .

     Some

     of

     the

     most

     promising

     trends

     include

    :
    


    1

    .

     Increased

     automation

    :

     As

     AI

     becomes

     more

     sophisticated

    ,

     it

     will

     become

     increasingly

     automated

    ,

     taking

     over

     many

     tasks

     that

     were

     previously

     performed

     by

     humans

    .

     This

     could

     lead

     to

     significant

     job

     losses

    ,

     but

     it

     could

     also

     lead

     to

     new

     industries

     and

     jobs

     being

     created

    .
    


    2

    .

     More

     personalized

     and

     targeted

     AI

    :

     As

     AI

     is

     able

     to

     learn

     and

     improve

     on

     its

     own

    ,

     it

     will

     become

     more

     personalized

     and

     targeted

    .

     This

     could

     lead

     to

     more

     personalized

     and

     effective

     treatments

     for

     diseases

    ,

     more

     accurate

     predictions

     of

     weather

     patterns

    ,

     and

     more

     efficient

     management

     of

     supply

     chains

    .
    


    3

    .

     Better

    



```python
llm.shutdown()
```
