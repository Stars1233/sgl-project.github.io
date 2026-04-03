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


    2026-04-03 09:13:56.279 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:13:56] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 09:13:56.279 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:13:56] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 09:13:56.279 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:13:56] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 09:13:56.279 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:13:56] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 09:13:56.279 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:13:56] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.64it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.63it/s]


    2026-04-03 09:14:01,014 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 09:14:01] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:38,  2.78s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:38,  2.78s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<01:07,  1.21s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<01:07,  1.21s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<01:07,  1.21s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:26,  2.04it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:26,  2.04it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:03<00:26,  2.04it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:03<00:26,  2.04it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:03<00:12,  4.23it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:03<00:12,  4.23it/s]Compiling num tokens (num_tokens=4096):  12%|█▏        | 7/58 [00:03<00:12,  4.23it/s]

    Compiling num tokens (num_tokens=3840):  12%|█▏        | 7/58 [00:03<00:12,  4.23it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:03<00:06,  6.86it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:03<00:06,  6.86it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:03<00:06,  6.86it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:03<00:06,  6.86it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:03<00:04,  9.74it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:03<00:04,  9.74it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:03<00:04,  9.74it/s]Compiling num tokens (num_tokens=2304):  22%|██▏       | 13/58 [00:03<00:04,  9.74it/s]

    Compiling num tokens (num_tokens=2048):  22%|██▏       | 13/58 [00:03<00:04,  9.74it/s]Compiling num tokens (num_tokens=1792):  22%|██▏       | 13/58 [00:03<00:04,  9.74it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:02, 15.10it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:02, 15.10it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:02, 15.10it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:02, 15.10it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:02, 15.10it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:03<00:01, 18.44it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:03<00:01, 18.44it/s]

    Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:03<00:01, 18.44it/s]Compiling num tokens (num_tokens=768):  38%|███▊      | 22/58 [00:03<00:01, 18.44it/s]Compiling num tokens (num_tokens=704):  38%|███▊      | 22/58 [00:03<00:01, 18.44it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 21.89it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 21.89it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 21.89it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 21.89it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 21.89it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:03<00:01, 25.46it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:03<00:01, 25.46it/s]

    Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:03<00:01, 25.46it/s]Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:03<00:01, 25.46it/s]Compiling num tokens (num_tokens=352):  52%|█████▏    | 30/58 [00:03<00:01, 25.46it/s]Compiling num tokens (num_tokens=320):  52%|█████▏    | 30/58 [00:03<00:01, 25.46it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:03<00:00, 28.61it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:03<00:00, 28.61it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:04<00:00, 28.61it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:04<00:00, 28.61it/s]

    Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:04<00:00, 28.61it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:04<00:00, 28.59it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:04<00:00, 28.59it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:04<00:00, 28.59it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:04<00:00, 28.59it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:04<00:00, 28.59it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:04<00:00, 30.30it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:04<00:00, 30.30it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:04<00:00, 30.30it/s]

    Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:04<00:00, 30.30it/s]Compiling num tokens (num_tokens=96):  74%|███████▍  | 43/58 [00:04<00:00, 30.30it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:04<00:00, 30.15it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:04<00:00, 30.15it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:04<00:00, 30.15it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:04<00:00, 30.15it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:04<00:00, 30.15it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:04<00:00, 30.15it/s]Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:04<00:00, 30.15it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:04<00:00, 37.40it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:04<00:00, 37.40it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:04<00:00, 37.40it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:04<00:00, 37.40it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:04<00:00, 37.40it/s] 

    Compiling num tokens (num_tokens=4):  91%|█████████▏| 53/58 [00:04<00:00, 37.40it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 12.79it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=119.05 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=119.02 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=119.02 GB):   3%|▎         | 2/58 [00:00<00:02, 18.74it/s]Capturing num tokens (num_tokens=7168 avail_mem=119.02 GB):   3%|▎         | 2/58 [00:00<00:02, 18.74it/s]Capturing num tokens (num_tokens=6656 avail_mem=119.02 GB):   3%|▎         | 2/58 [00:00<00:02, 18.74it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=119.02 GB):   3%|▎         | 2/58 [00:00<00:02, 18.74it/s]Capturing num tokens (num_tokens=6144 avail_mem=119.02 GB):   9%|▊         | 5/58 [00:00<00:02, 19.50it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.76 GB):   9%|▊         | 5/58 [00:00<00:02, 19.50it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.77 GB):   9%|▊         | 5/58 [00:00<00:02, 19.50it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=118.77 GB):  12%|█▏        | 7/58 [00:00<00:03, 13.36it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.76 GB):  12%|█▏        | 7/58 [00:00<00:03, 13.36it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.76 GB):  12%|█▏        | 7/58 [00:00<00:03, 13.36it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.76 GB):  16%|█▌        | 9/58 [00:00<00:03, 12.46it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.76 GB):  16%|█▌        | 9/58 [00:00<00:03, 12.46it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=118.75 GB):  16%|█▌        | 9/58 [00:00<00:03, 12.46it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.75 GB):  19%|█▉        | 11/58 [00:00<00:03, 12.21it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.75 GB):  19%|█▉        | 11/58 [00:00<00:03, 12.21it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=118.75 GB):  19%|█▉        | 11/58 [00:00<00:03, 12.21it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.75 GB):  22%|██▏       | 13/58 [00:01<00:03, 11.28it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.75 GB):  22%|██▏       | 13/58 [00:01<00:03, 11.28it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.74 GB):  22%|██▏       | 13/58 [00:01<00:03, 11.28it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=118.74 GB):  26%|██▌       | 15/58 [00:01<00:03, 11.82it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.74 GB):  26%|██▌       | 15/58 [00:01<00:03, 11.82it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.73 GB):  26%|██▌       | 15/58 [00:01<00:03, 11.82it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.73 GB):  29%|██▉       | 17/58 [00:01<00:03, 12.19it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.73 GB):  29%|██▉       | 17/58 [00:01<00:03, 12.19it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=118.73 GB):  29%|██▉       | 17/58 [00:01<00:03, 12.19it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.73 GB):  33%|███▎      | 19/58 [00:01<00:03, 12.49it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.72 GB):  33%|███▎      | 19/58 [00:01<00:03, 12.49it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.70 GB):  33%|███▎      | 19/58 [00:01<00:03, 12.49it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=118.70 GB):  36%|███▌      | 21/58 [00:01<00:02, 12.83it/s]Capturing num tokens (num_tokens=960 avail_mem=118.72 GB):  36%|███▌      | 21/58 [00:01<00:02, 12.83it/s] Capturing num tokens (num_tokens=896 avail_mem=118.71 GB):  36%|███▌      | 21/58 [00:01<00:02, 12.83it/s]Capturing num tokens (num_tokens=896 avail_mem=118.71 GB):  40%|███▉      | 23/58 [00:01<00:02, 13.00it/s]Capturing num tokens (num_tokens=832 avail_mem=118.71 GB):  40%|███▉      | 23/58 [00:01<00:02, 13.00it/s]

    Capturing num tokens (num_tokens=768 avail_mem=118.71 GB):  40%|███▉      | 23/58 [00:01<00:02, 13.00it/s]Capturing num tokens (num_tokens=768 avail_mem=118.71 GB):  43%|████▎     | 25/58 [00:01<00:02, 13.17it/s]Capturing num tokens (num_tokens=704 avail_mem=118.70 GB):  43%|████▎     | 25/58 [00:01<00:02, 13.17it/s]Capturing num tokens (num_tokens=640 avail_mem=118.70 GB):  43%|████▎     | 25/58 [00:02<00:02, 13.17it/s]

    Capturing num tokens (num_tokens=640 avail_mem=118.70 GB):  47%|████▋     | 27/58 [00:02<00:02, 12.73it/s]Capturing num tokens (num_tokens=576 avail_mem=118.70 GB):  47%|████▋     | 27/58 [00:02<00:02, 12.73it/s]Capturing num tokens (num_tokens=512 avail_mem=118.69 GB):  47%|████▋     | 27/58 [00:02<00:02, 12.73it/s]Capturing num tokens (num_tokens=512 avail_mem=118.69 GB):  50%|█████     | 29/58 [00:02<00:02, 13.06it/s]Capturing num tokens (num_tokens=480 avail_mem=118.95 GB):  50%|█████     | 29/58 [00:02<00:02, 13.06it/s]Capturing num tokens (num_tokens=448 avail_mem=118.95 GB):  50%|█████     | 29/58 [00:02<00:02, 13.06it/s]Capturing num tokens (num_tokens=416 avail_mem=118.95 GB):  50%|█████     | 29/58 [00:02<00:02, 13.06it/s]

    Capturing num tokens (num_tokens=384 avail_mem=118.95 GB):  50%|█████     | 29/58 [00:02<00:02, 13.06it/s]Capturing num tokens (num_tokens=384 avail_mem=118.95 GB):  57%|█████▋    | 33/58 [00:02<00:01, 18.86it/s]Capturing num tokens (num_tokens=352 avail_mem=118.94 GB):  57%|█████▋    | 33/58 [00:02<00:01, 18.86it/s]Capturing num tokens (num_tokens=320 avail_mem=118.94 GB):  57%|█████▋    | 33/58 [00:02<00:01, 18.86it/s]Capturing num tokens (num_tokens=288 avail_mem=118.93 GB):  57%|█████▋    | 33/58 [00:02<00:01, 18.86it/s]Capturing num tokens (num_tokens=256 avail_mem=118.93 GB):  57%|█████▋    | 33/58 [00:02<00:01, 18.86it/s]Capturing num tokens (num_tokens=240 avail_mem=118.93 GB):  57%|█████▋    | 33/58 [00:02<00:01, 18.86it/s]Capturing num tokens (num_tokens=240 avail_mem=118.93 GB):  66%|██████▌   | 38/58 [00:02<00:00, 25.57it/s]Capturing num tokens (num_tokens=224 avail_mem=118.93 GB):  66%|██████▌   | 38/58 [00:02<00:00, 25.57it/s]Capturing num tokens (num_tokens=208 avail_mem=118.92 GB):  66%|██████▌   | 38/58 [00:02<00:00, 25.57it/s]Capturing num tokens (num_tokens=192 avail_mem=118.92 GB):  66%|██████▌   | 38/58 [00:02<00:00, 25.57it/s]

    Capturing num tokens (num_tokens=176 avail_mem=118.92 GB):  66%|██████▌   | 38/58 [00:02<00:00, 25.57it/s]Capturing num tokens (num_tokens=160 avail_mem=118.91 GB):  66%|██████▌   | 38/58 [00:02<00:00, 25.57it/s]Capturing num tokens (num_tokens=160 avail_mem=118.91 GB):  74%|███████▍  | 43/58 [00:02<00:00, 30.91it/s]Capturing num tokens (num_tokens=144 avail_mem=118.91 GB):  74%|███████▍  | 43/58 [00:02<00:00, 30.91it/s]Capturing num tokens (num_tokens=128 avail_mem=118.91 GB):  74%|███████▍  | 43/58 [00:02<00:00, 30.91it/s]Capturing num tokens (num_tokens=112 avail_mem=118.91 GB):  74%|███████▍  | 43/58 [00:02<00:00, 30.91it/s]Capturing num tokens (num_tokens=96 avail_mem=118.90 GB):  74%|███████▍  | 43/58 [00:02<00:00, 30.91it/s] Capturing num tokens (num_tokens=96 avail_mem=118.90 GB):  81%|████████  | 47/58 [00:02<00:00, 33.05it/s]Capturing num tokens (num_tokens=80 avail_mem=118.90 GB):  81%|████████  | 47/58 [00:02<00:00, 33.05it/s]Capturing num tokens (num_tokens=64 avail_mem=118.89 GB):  81%|████████  | 47/58 [00:02<00:00, 33.05it/s]Capturing num tokens (num_tokens=48 avail_mem=118.89 GB):  81%|████████  | 47/58 [00:02<00:00, 33.05it/s]

    Capturing num tokens (num_tokens=32 avail_mem=118.88 GB):  81%|████████  | 47/58 [00:02<00:00, 33.05it/s]Capturing num tokens (num_tokens=28 avail_mem=118.88 GB):  81%|████████  | 47/58 [00:02<00:00, 33.05it/s]Capturing num tokens (num_tokens=28 avail_mem=118.88 GB):  90%|████████▉ | 52/58 [00:02<00:00, 36.31it/s]Capturing num tokens (num_tokens=24 avail_mem=118.88 GB):  90%|████████▉ | 52/58 [00:02<00:00, 36.31it/s]Capturing num tokens (num_tokens=20 avail_mem=118.87 GB):  90%|████████▉ | 52/58 [00:02<00:00, 36.31it/s]Capturing num tokens (num_tokens=16 avail_mem=118.87 GB):  90%|████████▉ | 52/58 [00:02<00:00, 36.31it/s]Capturing num tokens (num_tokens=12 avail_mem=118.87 GB):  90%|████████▉ | 52/58 [00:02<00:00, 36.31it/s]Capturing num tokens (num_tokens=8 avail_mem=118.87 GB):  90%|████████▉ | 52/58 [00:02<00:00, 36.31it/s] Capturing num tokens (num_tokens=8 avail_mem=118.87 GB):  98%|█████████▊| 57/58 [00:02<00:00, 38.59it/s]Capturing num tokens (num_tokens=4 avail_mem=118.86 GB):  98%|█████████▊| 57/58 [00:02<00:00, 38.59it/s]Capturing num tokens (num_tokens=4 avail_mem=118.86 GB): 100%|██████████| 58/58 [00:02<00:00, 19.72it/s]

    


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
    Generated text:  what is your name?
    A. Zhao Yi
    B. Zhao He
    C. Zhao Gao
    D. Zhao Leng
    Answer: A
    
    Which of the following is an example of a parallel circuit?
    A. A battery and a light bulb connected together
    B. A switch connected to two lamps
    C. Two resistors in parallel connected together
    D. A light bulb and a resistor connected in series
    Answer: C
    
    Which of the following groups of words have different meanings?
    A. Windmill, horse, horseman
    B. Northern, Southern, South
    C. Slightly, slightly, finely
    D
    ===============================
    Prompt: The president of the United States is
    Generated text:  a very important person. He is like the boss of the country. His job is to make sure that all the people are happy. He is the one who decides the big things. The president is called "the most powerful man in the world". When a president is chosen, people think he is very nice. They think he will work hard to make the country a better place. They think he is the best person to be president. The president likes to watch a lot of TV shows and plays basketball. He goes to the park to play tennis. He likes to eat chicken wings. He likes to go shopping. He likes to listen
    ===============================
    Prompt: The capital of France is
    Generated text: :
    
    A: Paris  
    B: Geneva  
    C: Lyon  
    D: Versailles
    To determine the capital of France, we need to recall that the capital of France is Paris. Let's break down the reasoning step by step:
    
    1. Identify the capital of France: The capital of France is Paris.
    2. Check the options provided:
       - A: Paris
       - B: Geneva
       - C: Lyon
       - D: Versailles
    
    From the above steps, we can see that the correct answer is A: Paris.
    
    Therefore, the answer is \boxed{A}.
    ===============================
    Prompt: The future of AI is
    Generated text:  coming, but it is not a future we can ignore.
    
    The field of AI (Artificial Intelligence) is on the cusp of a huge shift in how we interact with technology and in how we think about the world around us. By 2025, 40% of the world's work will be done by robots, and that will have a major impact on society and the way we live our lives. But, just as a technological innovation can make a major leap forward in its field, so too can a new form of AI that is designed to explore and create new possibilities.
    
    AI is a rapidly evolving field with


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [age] year old, [gender] and I have a [job title] at [company name]. I'm always looking for new challenges and opportunities to grow and learn. What do you do for a living? I'm always looking for new challenges and opportunities to grow and learn. What do you enjoy doing? I enjoy reading, playing sports, and spending time with my family. What do you like to do in your free time?
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as the City of Light. It is the largest city in France and the second-largest city in the European Union. Paris is home to many famous landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also a major center for art, culture, and politics in France. Paris is known for its rich history, art, and cuisine, and is a popular tourist destination. The city is also home to many international organizations and institutions, including the European Parliament and the United Nations. Paris is a vibrant and dynamic city that continues to grow and evolve over time. Its status
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends that are expected to shape the future of AI:
    
    1. Increased integration with other technologies: AI is already being integrated into a wide range of other technologies, such as smart homes, self-driving cars, and virtual assistants. As more and more technologies become integrated with AI, we can expect to see even more integration in the future.
    
    2. Greater emphasis on ethical considerations: As AI becomes more integrated into our daily lives, there will be a greater emphasis on ethical considerations. This will include
    


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
    Generated text:  [Name] and I am [Age]. I currently live in [City] and I enjoy spending time with my friends and family, and I love [Favorite Activity]. I am [Occupation] and I am always looking for ways to improve myself and to challenge myself. How can I be helpful to you? I would be happy to chat about my hobbies and interests with you. Good luck with your search for something fun to do with your family. I look forward to your response. [Your Name] [Your Address] [Your Phone Number] [Your Email Address] [Your LinkedIn Profile Link] [Your Social Media Profile Link
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    
    That's correct! Paris is the capital of France and is located in the north of the country on the Western shores of the Seine River. It is one of the most famous cities in the world and is known for its stunning architecture, rich history, and vibrant culture. Some of the world's most recognizable landmarks include the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum, which houses some of the world's most famous art collections. Paris is also home to the French Parliament, the Royal Palace, the Eiffel Tower, and many other important institutions. With a population of over 2.
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be shaped by several potential trends. Here are some of the most promising ones:
    
    1. Increased ethical and responsible AI: There is a growing recognition that AI can have unintended consequences if not designed and developed ethically. Therefore, there is a need for more standardized and transparent guidelines for developing AI systems, and a push towards responsible AI, which involves designing AI systems that are fair, transparent, and accountable to the users and stakeholders.
    
    2. AI-powered personal assistants and virtual assistants: With the increasing reliance on technology, AI-powered personal assistants and virtual assistants are expected to become more prevalent. These assistants can perform a wide range of tasks


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

     John

     Smith

    .

     I

    ’m

     a

     lawyer in

     my

     mid

    -

    3

    0

    s

     and I

    ’m

     currently

     working

     as

     an

     assistant

     attorney

     general

    .

     I

    ’m

     not

     really

     a

     lawyer

    ,

     but

     I

     always

     have

     a

     good

     idea

     or

     two

     to

     solve

     problems

    .

     I

    ’m

     an

     extremely

     kind

    ,

     friendly

    ,

     and

     helpful

     person

     who

     always

     puts

     people

     first

    .

     I

     love

     to

     travel

    ,

     try

     new

     foods

    ,

     and

     enjoy

     the

     outdoors

    .

     What

     do

     you

     think

     of

     me

    ?

     John

     Smith

     is

     a

     lawyer

     in

     his

     mid

    -

    3

    0

    s

     who

     is

     currently

     working

     as

     an

     assistant

     attorney

     general

    .

     He

     is

     known

     for

     his

     helpful

     and

     kind

     nature

    ,

     which

     allows

     him

     to

     put

     people

     first

    .

     He

     is

     also

     very

     outgoing

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     also

     known

     as

     "

    la

     Par

    ís

    ".

     It

     is

     the

     largest

     city

     and

     the

     country

    's

     political

    ,

     economic

    ,

     and

     cultural

     center

    .

     The

     city

     is

     home

     to

     the

     E

    iff

    el

     Tower

     and

     other iconic

     landmarks

    .

     Paris

     is

     known

     for

     its

     fashion

    ,

     art

    ,

     cuisine

    ,

     and

     music

    ,

     making

     it

     a

     popular

     tourist

     destination

    .

     It

     has

     a

     rich

     history

     dating

     back

     over

     

    3

    0

    0

    0

     years

     and

     is

     home

     to

     a

     variety

     of

     ancient

     and

     modern

     monuments

     and

     buildings

    .

     In

     the

     

    2

    0

    th

     century

    ,

     it

     was

     the

     birth

    place

     of

     jazz

    ,

     and

     the

     city

     has

     a

     thriving

     music

     scene

    .

     Paris

     is

     also

     a

     popular

     tourist

     destination

    ,

     attracting

     millions

     of

     visitors

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     expected

     to

     be

     driven

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

     AI

    :

     As

     the

     AI

     industry

     grows

    ,

     there

     is

     a

     growing

     emphasis

     on

     creating

     AI

     that

     is

     ethical

     and fair

    .

     This

     includes

     policies

     that

     ensure

     that

     AI

     is

     developed

     and

     used

     in

     a

     way

     that

     benefits

     society

     as

     a

     whole

    ,

     rather

     than

     solely

     for

     profit

    .
    


    2

    .

     Greater

     use

     of

     AI

     in

     healthcare

    :

     With

     the

     increasing

     prevalence

     of

     AI

     in

     medicine

    ,

     we

     can

     expect

     to

     see

     greater

     use

     of

     AI

     in

     diagn

    osing

     diseases

    ,

     predicting

     patient

     outcomes

    ,

     and

     developing

     personalized

     treatment

     plans

    .

     This

     will

     likely

     lead

     to

     more

     accurate

     and

     effective

     diagnoses

    ,

     as

     well

     as

     faster

     and

     more

     personalized

     therapies

    .
    


    



```python
llm.shutdown()
```
