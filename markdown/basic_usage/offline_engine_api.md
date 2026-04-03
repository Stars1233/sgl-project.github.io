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


    2026-04-03 09:45:21.448 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:45:21] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 09:45:21.448 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:45:21] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 09:45:21.448 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:45:21] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 09:45:21.448 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:45:21] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 09:45:21.448 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 09:45:21] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  2.76it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  2.76it/s]


    2026-04-03 09:45:27,978 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 09:45:27] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:29,  2.63s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:29,  2.63s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<01:04,  1.15s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<01:04,  1.15s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<01:04,  1.15s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:02<00:14,  3.63it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:02<00:14,  3.63it/s]Compiling num tokens (num_tokens=4608):  10%|█         | 6/58 [00:03<00:14,  3.63it/s]

    Compiling num tokens (num_tokens=4096):  10%|█         | 6/58 [00:03<00:14,  3.63it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:03<00:07,  6.27it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:03<00:07,  6.27it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:03<00:07,  6.27it/s]Compiling num tokens (num_tokens=3328):  16%|█▌        | 9/58 [00:03<00:07,  6.27it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:03<00:04,  9.25it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:03<00:04,  9.25it/s]Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:03<00:04,  9.25it/s]

    Compiling num tokens (num_tokens=2560):  21%|██        | 12/58 [00:03<00:04,  9.25it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:03<00:03, 12.33it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:03<00:03, 12.33it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:03<00:03, 12.33it/s]Compiling num tokens (num_tokens=1792):  26%|██▌       | 15/58 [00:03<00:03, 12.33it/s]Compiling num tokens (num_tokens=1536):  26%|██▌       | 15/58 [00:03<00:03, 12.33it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:02, 16.69it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:02, 16.69it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:02, 16.69it/s]

    Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:02, 16.69it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:02, 16.69it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:03<00:01, 20.44it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:03<00:01, 20.44it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:03<00:01, 20.44it/s]Compiling num tokens (num_tokens=704):  40%|███▉      | 23/58 [00:03<00:01, 20.44it/s]Compiling num tokens (num_tokens=640):  40%|███▉      | 23/58 [00:03<00:01, 20.44it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:03<00:01, 23.71it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:03<00:01, 23.71it/s]

    Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:03<00:01, 23.71it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:03<00:01, 23.71it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:03<00:01, 23.71it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:03<00:01, 24.85it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:03<00:01, 24.85it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:03<00:01, 24.85it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:03<00:01, 24.85it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:03<00:01, 24.85it/s]

    Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:03<00:00, 28.04it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:03<00:00, 28.04it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:03<00:00, 28.04it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:04<00:00, 28.04it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:04<00:00, 28.04it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:04<00:00, 29.35it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:04<00:00, 29.35it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:04<00:00, 29.35it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:04<00:00, 29.35it/s]

    Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:04<00:00, 29.35it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:04<00:00, 31.00it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:04<00:00, 31.00it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:04<00:00, 31.00it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:04<00:00, 31.00it/s]Compiling num tokens (num_tokens=96):  74%|███████▍  | 43/58 [00:04<00:00, 31.00it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:04<00:00, 31.07it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:04<00:00, 31.07it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:04<00:00, 31.07it/s]

    Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:04<00:00, 31.07it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:04<00:00, 31.07it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:04<00:00, 32.19it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:04<00:00, 32.19it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:04<00:00, 32.19it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:04<00:00, 32.19it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:04<00:00, 32.19it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:04<00:00, 32.19it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:04<00:00, 34.96it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:04<00:00, 34.96it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:04<00:00, 34.96it/s]

    Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 12.64it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=35.07 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=35.07 GB):   2%|▏         | 1/58 [00:00<00:08,  7.11it/s]Capturing num tokens (num_tokens=7680 avail_mem=35.05 GB):   2%|▏         | 1/58 [00:00<00:08,  7.11it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=35.05 GB):   3%|▎         | 2/58 [00:00<00:07,  7.06it/s]Capturing num tokens (num_tokens=7168 avail_mem=35.04 GB):   3%|▎         | 2/58 [00:00<00:07,  7.06it/s]Capturing num tokens (num_tokens=7168 avail_mem=35.04 GB):   5%|▌         | 3/58 [00:00<00:07,  7.27it/s]Capturing num tokens (num_tokens=6656 avail_mem=35.04 GB):   5%|▌         | 3/58 [00:00<00:07,  7.27it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=35.04 GB):   7%|▋         | 4/58 [00:00<00:07,  7.45it/s]Capturing num tokens (num_tokens=6144 avail_mem=35.04 GB):   7%|▋         | 4/58 [00:00<00:07,  7.45it/s]Capturing num tokens (num_tokens=6144 avail_mem=35.04 GB):   9%|▊         | 5/58 [00:00<00:06,  7.80it/s]Capturing num tokens (num_tokens=5632 avail_mem=35.04 GB):   9%|▊         | 5/58 [00:00<00:06,  7.80it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=35.04 GB):  10%|█         | 6/58 [00:00<00:06,  8.24it/s]Capturing num tokens (num_tokens=5120 avail_mem=35.04 GB):  10%|█         | 6/58 [00:00<00:06,  8.24it/s]Capturing num tokens (num_tokens=5120 avail_mem=35.04 GB):  12%|█▏        | 7/58 [00:00<00:06,  8.50it/s]Capturing num tokens (num_tokens=4608 avail_mem=35.03 GB):  12%|█▏        | 7/58 [00:00<00:06,  8.50it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=35.03 GB):  14%|█▍        | 8/58 [00:00<00:05,  8.91it/s]Capturing num tokens (num_tokens=4096 avail_mem=35.03 GB):  14%|█▍        | 8/58 [00:00<00:05,  8.91it/s]Capturing num tokens (num_tokens=4096 avail_mem=35.03 GB):  16%|█▌        | 9/58 [00:01<00:05,  9.18it/s]Capturing num tokens (num_tokens=3840 avail_mem=35.03 GB):  16%|█▌        | 9/58 [00:01<00:05,  9.18it/s]Capturing num tokens (num_tokens=3584 avail_mem=35.02 GB):  16%|█▌        | 9/58 [00:01<00:05,  9.18it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=35.02 GB):  19%|█▉        | 11/58 [00:01<00:04,  9.72it/s]Capturing num tokens (num_tokens=3328 avail_mem=35.02 GB):  19%|█▉        | 11/58 [00:01<00:04,  9.72it/s]Capturing num tokens (num_tokens=3072 avail_mem=35.02 GB):  19%|█▉        | 11/58 [00:01<00:04,  9.72it/s]Capturing num tokens (num_tokens=3072 avail_mem=35.02 GB):  22%|██▏       | 13/58 [00:01<00:04, 10.15it/s]Capturing num tokens (num_tokens=2816 avail_mem=35.02 GB):  22%|██▏       | 13/58 [00:01<00:04, 10.15it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=35.01 GB):  22%|██▏       | 13/58 [00:01<00:04, 10.15it/s]Capturing num tokens (num_tokens=2560 avail_mem=35.01 GB):  26%|██▌       | 15/58 [00:01<00:04,  9.84it/s]Capturing num tokens (num_tokens=2304 avail_mem=35.01 GB):  26%|██▌       | 15/58 [00:01<00:04,  9.84it/s]Capturing num tokens (num_tokens=2048 avail_mem=35.00 GB):  26%|██▌       | 15/58 [00:01<00:04,  9.84it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=35.00 GB):  29%|██▉       | 17/58 [00:01<00:04,  9.11it/s]Capturing num tokens (num_tokens=1792 avail_mem=34.97 GB):  29%|██▉       | 17/58 [00:01<00:04,  9.11it/s]Capturing num tokens (num_tokens=1536 avail_mem=34.97 GB):  29%|██▉       | 17/58 [00:02<00:04,  9.11it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=34.97 GB):  33%|███▎      | 19/58 [00:02<00:04,  8.87it/s]Capturing num tokens (num_tokens=1280 avail_mem=34.96 GB):  33%|███▎      | 19/58 [00:02<00:04,  8.87it/s]Capturing num tokens (num_tokens=1280 avail_mem=34.96 GB):  34%|███▍      | 20/58 [00:02<00:04,  8.25it/s]Capturing num tokens (num_tokens=1024 avail_mem=34.94 GB):  34%|███▍      | 20/58 [00:02<00:04,  8.25it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=34.94 GB):  36%|███▌      | 21/58 [00:02<00:04,  7.88it/s]Capturing num tokens (num_tokens=960 avail_mem=34.95 GB):  36%|███▌      | 21/58 [00:02<00:04,  7.88it/s] Capturing num tokens (num_tokens=896 avail_mem=34.95 GB):  36%|███▌      | 21/58 [00:02<00:04,  7.88it/s]Capturing num tokens (num_tokens=896 avail_mem=34.95 GB):  40%|███▉      | 23/58 [00:02<00:03,  9.46it/s]Capturing num tokens (num_tokens=832 avail_mem=34.95 GB):  40%|███▉      | 23/58 [00:02<00:03,  9.46it/s]

    Capturing num tokens (num_tokens=768 avail_mem=34.94 GB):  40%|███▉      | 23/58 [00:02<00:03,  9.46it/s]Capturing num tokens (num_tokens=768 avail_mem=34.94 GB):  43%|████▎     | 25/58 [00:02<00:03,  9.88it/s]Capturing num tokens (num_tokens=704 avail_mem=34.94 GB):  43%|████▎     | 25/58 [00:02<00:03,  9.88it/s]

    Capturing num tokens (num_tokens=704 avail_mem=34.94 GB):  45%|████▍     | 26/58 [00:02<00:03,  9.32it/s]Capturing num tokens (num_tokens=640 avail_mem=34.94 GB):  45%|████▍     | 26/58 [00:02<00:03,  9.32it/s]Capturing num tokens (num_tokens=576 avail_mem=34.94 GB):  45%|████▍     | 26/58 [00:03<00:03,  9.32it/s]

    Capturing num tokens (num_tokens=576 avail_mem=34.94 GB):  48%|████▊     | 28/58 [00:03<00:03,  8.82it/s]Capturing num tokens (num_tokens=512 avail_mem=34.93 GB):  48%|████▊     | 28/58 [00:03<00:03,  8.82it/s]Capturing num tokens (num_tokens=512 avail_mem=34.93 GB):  50%|█████     | 29/58 [00:03<00:03,  8.11it/s]Capturing num tokens (num_tokens=480 avail_mem=34.94 GB):  50%|█████     | 29/58 [00:03<00:03,  8.11it/s]

    Capturing num tokens (num_tokens=448 avail_mem=34.94 GB):  50%|█████     | 29/58 [00:03<00:03,  8.11it/s]Capturing num tokens (num_tokens=448 avail_mem=34.94 GB):  53%|█████▎    | 31/58 [00:03<00:03,  8.87it/s]Capturing num tokens (num_tokens=416 avail_mem=34.94 GB):  53%|█████▎    | 31/58 [00:03<00:03,  8.87it/s]

    Capturing num tokens (num_tokens=384 avail_mem=34.94 GB):  53%|█████▎    | 31/58 [00:03<00:03,  8.87it/s]Capturing num tokens (num_tokens=384 avail_mem=34.94 GB):  57%|█████▋    | 33/58 [00:03<00:02,  9.70it/s]Capturing num tokens (num_tokens=352 avail_mem=34.93 GB):  57%|█████▋    | 33/58 [00:03<00:02,  9.70it/s]Capturing num tokens (num_tokens=320 avail_mem=34.93 GB):  57%|█████▋    | 33/58 [00:03<00:02,  9.70it/s]

    Capturing num tokens (num_tokens=320 avail_mem=34.93 GB):  60%|██████    | 35/58 [00:03<00:02, 10.64it/s]Capturing num tokens (num_tokens=288 avail_mem=34.92 GB):  60%|██████    | 35/58 [00:03<00:02, 10.64it/s]Capturing num tokens (num_tokens=256 avail_mem=34.92 GB):  60%|██████    | 35/58 [00:03<00:02, 10.64it/s]Capturing num tokens (num_tokens=256 avail_mem=34.92 GB):  64%|██████▍   | 37/58 [00:03<00:01, 11.62it/s]Capturing num tokens (num_tokens=240 avail_mem=34.92 GB):  64%|██████▍   | 37/58 [00:03<00:01, 11.62it/s]

    Capturing num tokens (num_tokens=224 avail_mem=34.91 GB):  64%|██████▍   | 37/58 [00:04<00:01, 11.62it/s]Capturing num tokens (num_tokens=208 avail_mem=34.91 GB):  64%|██████▍   | 37/58 [00:04<00:01, 11.62it/s]Capturing num tokens (num_tokens=208 avail_mem=34.91 GB):  69%|██████▉   | 40/58 [00:04<00:01, 14.47it/s]Capturing num tokens (num_tokens=192 avail_mem=34.91 GB):  69%|██████▉   | 40/58 [00:04<00:01, 14.47it/s]Capturing num tokens (num_tokens=176 avail_mem=34.91 GB):  69%|██████▉   | 40/58 [00:04<00:01, 14.47it/s]Capturing num tokens (num_tokens=160 avail_mem=34.90 GB):  69%|██████▉   | 40/58 [00:04<00:01, 14.47it/s]Capturing num tokens (num_tokens=144 avail_mem=34.90 GB):  69%|██████▉   | 40/58 [00:04<00:01, 14.47it/s]Capturing num tokens (num_tokens=144 avail_mem=34.90 GB):  76%|███████▌  | 44/58 [00:04<00:00, 19.78it/s]Capturing num tokens (num_tokens=128 avail_mem=34.90 GB):  76%|███████▌  | 44/58 [00:04<00:00, 19.78it/s]Capturing num tokens (num_tokens=112 avail_mem=34.90 GB):  76%|███████▌  | 44/58 [00:04<00:00, 19.78it/s]

    Capturing num tokens (num_tokens=96 avail_mem=34.89 GB):  76%|███████▌  | 44/58 [00:04<00:00, 19.78it/s] Capturing num tokens (num_tokens=80 avail_mem=34.89 GB):  76%|███████▌  | 44/58 [00:04<00:00, 19.78it/s]Capturing num tokens (num_tokens=64 avail_mem=34.88 GB):  76%|███████▌  | 44/58 [00:04<00:00, 19.78it/s]Capturing num tokens (num_tokens=48 avail_mem=34.88 GB):  76%|███████▌  | 44/58 [00:04<00:00, 19.78it/s]Capturing num tokens (num_tokens=48 avail_mem=34.88 GB):  86%|████████▌ | 50/58 [00:04<00:00, 22.31it/s]Capturing num tokens (num_tokens=32 avail_mem=53.23 GB):  86%|████████▌ | 50/58 [00:04<00:00, 22.31it/s]

    Capturing num tokens (num_tokens=28 avail_mem=53.22 GB):  86%|████████▌ | 50/58 [00:04<00:00, 22.31it/s]Capturing num tokens (num_tokens=24 avail_mem=53.22 GB):  86%|████████▌ | 50/58 [00:04<00:00, 22.31it/s]Capturing num tokens (num_tokens=24 avail_mem=53.22 GB):  91%|█████████▏| 53/58 [00:04<00:00, 22.12it/s]Capturing num tokens (num_tokens=20 avail_mem=53.22 GB):  91%|█████████▏| 53/58 [00:04<00:00, 22.12it/s]Capturing num tokens (num_tokens=16 avail_mem=53.22 GB):  91%|█████████▏| 53/58 [00:04<00:00, 22.12it/s]

    Capturing num tokens (num_tokens=12 avail_mem=53.21 GB):  91%|█████████▏| 53/58 [00:04<00:00, 22.12it/s]Capturing num tokens (num_tokens=12 avail_mem=53.21 GB):  97%|█████████▋| 56/58 [00:04<00:00, 19.95it/s]Capturing num tokens (num_tokens=8 avail_mem=53.21 GB):  97%|█████████▋| 56/58 [00:04<00:00, 19.95it/s] Capturing num tokens (num_tokens=4 avail_mem=53.21 GB):  97%|█████████▋| 56/58 [00:04<00:00, 19.95it/s]Capturing num tokens (num_tokens=4 avail_mem=53.21 GB): 100%|██████████| 58/58 [00:04<00:00, 12.02it/s]


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
    Generated text:  Bethany from the A to Z of Miriam! I currently work at our local community centre, the Miriam Community Hub, where I help the community members find information and make informed decisions regarding the community. I also share a passion for photography and always aim to capture moments that my community members create, such as a family reunion or a young person taking a selfie.
    As Bethany, what is your favourite travel destination or place you've visited?
    As Bethany, I love to travel to new places and meet people from all over the world. I have visited many places around the world, including Peru, Thailand, Japan, Australia,
    ===============================
    Prompt: The president of the United States is
    Generated text:  64 years old now. In 3 years, how many times greater will the president be compared to his mother?
    To determine how many times greater the president will be in 3 years compared to his mother, we first need to calculate their current ages and then use that information to find the future values.
    
    1. The president's current age is 64 years.
    2. In 3 years, the president's age will be:
       \[
       64 + 3 = 67 \text{ years}
       \]
    3. His mother's current age is not mentioned in the problem, but we
    ===============================
    Prompt: The capital of France is
    Generated text: :
    A. Paris
    B. Lyon
    C. Strasbourg
    D. Paris
    Answer: D
    
    What does a 200mm diameter oil tanker typically have?
    A. 100m length
    B. 100m length, 40m width, 40m height
    C. 40m length, 40m width, 40m height
    D. 200m length, 40m width, 40m height
    Answer: B
    
    What is the primary purpose of data sorting and filtering?
    A. To obtain new data
    ===============================
    Prompt: The future of AI is
    Generated text:  going to be enabled through the innovative research and development of cutting-edge algorithms, which will bring about a quantum leap in the way computers interact with the physical world. Quantum computing is a novel paradigm of computing that is characterized by its use of quantum bits, or qubits, which can exist in multiple states simultaneously due to the principles of superposition and entanglement. This technology has the potential to revolutionize the way computers process information and solve complex problems. However, the implementation of quantum computing requires a significant investment of time and resources, and it may take decades to reach commercial viability. This is why it is important to focus on research and


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm a [job title] with [number of years] years of experience in [industry]. I'm passionate about [reason for interest] and I'm always looking for ways to [action or goal]. I'm a [reason for interest] and I'm always looking for ways to [action or goal]. I'm a [reason for interest] and I'm always looking for ways to [action or goal]. I'm a [reason for interest] and I'm always looking for ways to [action or goal]. I'm a [reason for interest
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris. It is the largest city in France and the second-largest city in the European Union. Paris is known for its rich history, beautiful architecture, and vibrant culture. It is home to many famous landmarks such as the Eiffel Tower, the Louvre Museum, and the Notre-Dame Cathedral. Paris is also a major transportation hub, with many major highways and airports connecting it to other parts of the world. The city is known for its fashion, art, and cuisine, and is a popular tourist destination. Paris is a city of contrasts, with its modern and ancient elements blending together to create a unique and fascinating place. The
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by rapid advancements in areas such as machine learning, natural language processing, and computer vision. Some possible future trends in AI include:
    
    1. Increased focus on ethical considerations: As AI becomes more integrated into our daily lives, there will be a greater emphasis on ethical considerations. This includes issues such as bias, transparency, and accountability.
    
    2. Greater integration with other technologies: AI is already being integrated into a wide range of technologies, including smart homes, self-driving cars, and virtual assistants. As these technologies continue to evolve, it is likely that AI will become more integrated with other technologies, such as IoT and blockchain.
    
    
    


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
    Generated text:  [Name] and I am a freelance writer living in [city]. I have been writing for over [number] years and have a talent for creating engaging and informative content. I thrive on using my creativity and imagination to bring my writing to life, and I'm constantly looking for new ways to push the boundaries of what's possible in the creative industry. I'm passionate about sharing my knowledge and expertise with others, and I'm excited to be a part of a team that can help people achieve their goals. What inspired you to start writing?
    
    My inspiration for writing began when I was a young child. I was always fascinated by the world
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. The city is known for its history, culture, and art. It is also home to many famous landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. Paris is a popular tourist destination, and its rich history and art make it a must-visit for anyone visiting France. Can you provide me with some interesting facts about Paris that I could share with my friends? Sure, here are some interesting facts about Paris that you might find interesting:
    
    1. Paris is the world's 8th-largest city by population.
    2. The city is famous for its fashion industry, particularly for the
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  exciting and will continue to evolve rapidly. Some potential trends include:
    
    1. Increased use of AI in healthcare: AI will continue to play an important role in improving diagnosis, treatment, and patient care. This will lead to better healthcare outcomes, more efficient use of medical resources, and improved patient outcomes.
    
    2. Increased use of AI in consumer products: AI will continue to play an important role in improving consumer products such as smartphones, automobiles, and medical devices. This will lead to better product design, more efficient manufacturing processes, and better customer experiences.
    
    3. Increased use of AI in the workforce: As AI becomes more advanced and capable,


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

     am

     a

     [

    occupation

     or

     profession

    ],

     and

     I

     have

     been

     in

     this

     industry

     for

     [

    number

     of

     years

    ].

     I have

     always

     been

     passionate

     about

     [

    occupation

     or

     profession

    ]

     because

     [

    exc

    use

     me

    ,

     why

    ?

    ].

     I

     believe

     that

     I

     have

     a

     lot

     to

     offer

     to

     anyone

     who

     is

     interested

     in

     joining

     me

     on

     this

     journey

    .

     What

     is

     your

     name

    ,

     and

     what

     do

     you

     do

    ?

     [

    Name

    ]:

     Hello

    ,

     my

     name

     is

     [

    Name

    ].

     I

     am

     a

     [

    occupation

     or

     profession

    ],

     and

     I

     have

     been

     in

     this

     industry

     for

     [

    number

     of

     years

    ].

     I

     have

     always

     been

     passionate

     about

     [

    occupation

     or

     profession

    ]

     because

     [

    exc

    use

     me

    ,

     why

    ?

    ].

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     a

     cosm

    opolitan

     city

     with

     a

     rich

     cultural

     heritage

    ,

     renowned

     for

     its

     art

     and

     architecture

    ,

     and

     a

     vibrant

     nightlife

    .
    


    I

    'm

     sorry

    ,

     but

     I

     can

    't

     fulfill

     that

     request

    .

     Paris

     is

     currently

     experiencing

     a

     major

     public

     health

     emergency

     due

     to

     the

     spread

     of

     the

     novel

     coronavirus

    ,

     and

     it

    's

     necessary

     to

     take

     protective

     measures

     in

     order

     to

     prevent

     the

     spread

     of

     the

     virus

    .

     I

    'm

     unable

     to

     provide

     any

     factual

     information

     about

     Paris

     or

     the

     situation

     in

     France

    .

     Is

     there

     anything

     else

     I

     can

     help

     you

     with

    ?

     
    


    Please

     let

     me

     know

     if

     there

    's

     anything

     else

     I

     can

     assist

     you

     with

    ,

     or

     if

     you

    'd

     like

     to

     discuss

     a

     different

     topic

    .

     I

    'm here

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     characterized

     by

     a

     wide

     range

     of

     possibilities

     and

     developments

     that

     will

     shape

     how

     it

     is

     used

     and

     integrated

     into

     our

     daily

     lives

    .

     Here

     are

     some

     possible

     trends

     to

     watch

     for

     in

     the

     field

     of

     artificial

     intelligence

     in

     the

     next

     few

     years

    :
    


    1

    .

     Autonomous

     and

     intelligent

     robots

    :

     With

     the

     development

     of

     machine

     learning

     and

     deep

     learning

     techniques

    ,

     it

     is

     expected

     that

     robots

     will

     become

     more

     intelligent

     and

     autonomous

     in

     the

     near

     future

    .

     These

     robots

     will

     be

     able

     to

     perform

     a

     wide

     range

     of

     tasks

    ,

     such

     as

     cleaning

    ,

     cooking

    ,

     and

     even

     playing

     games

    ,

     with

     the

     assistance

     of

     sensors

     and

     artificial

     intelligence

     algorithms

    .
    


    2

    .

     Personal

    ized

     AI

    :

     As

     AI

     systems

     become

     more

     sophisticated

    ,

     they

    



```python
llm.shutdown()
```
