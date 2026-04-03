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


    2026-04-03 20:18:30.600 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 20:18:30] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 20:18:30.601 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 20:18:30] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 20:18:30.601 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 20:18:30] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 20:18:30.601 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 20:18:30] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 20:18:30.601 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 20:18:30] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.02it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.01it/s]


    2026-04-03 20:18:32,971 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 20:18:32] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:28,  2.61s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:28,  2.61s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<01:04,  1.15s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<01:04,  1.15s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<01:04,  1.15s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:25,  2.14it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:25,  2.14it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:25,  2.14it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:02<00:14,  3.67it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:02<00:14,  3.67it/s]Compiling num tokens (num_tokens=4608):  10%|█         | 6/58 [00:03<00:14,  3.67it/s]

    Compiling num tokens (num_tokens=4096):  10%|█         | 6/58 [00:03<00:14,  3.67it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:03<00:07,  6.35it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:03<00:07,  6.35it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:03<00:07,  6.35it/s]Compiling num tokens (num_tokens=3328):  16%|█▌        | 9/58 [00:03<00:07,  6.35it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:03<00:04,  9.38it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:03<00:04,  9.38it/s]Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:03<00:04,  9.38it/s]

    Compiling num tokens (num_tokens=2560):  21%|██        | 12/58 [00:03<00:04,  9.38it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:03<00:03, 12.60it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:03<00:03, 12.60it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:03<00:03, 12.60it/s]Compiling num tokens (num_tokens=1792):  26%|██▌       | 15/58 [00:03<00:03, 12.60it/s]Compiling num tokens (num_tokens=1536):  26%|██▌       | 15/58 [00:03<00:03, 12.60it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:02, 17.01it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:02, 17.01it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:02, 17.01it/s]

    Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:02, 17.01it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:02, 17.01it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:03<00:01, 20.51it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:03<00:01, 20.51it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:03<00:01, 20.51it/s]Compiling num tokens (num_tokens=704):  40%|███▉      | 23/58 [00:03<00:01, 20.51it/s]Compiling num tokens (num_tokens=640):  40%|███▉      | 23/58 [00:03<00:01, 20.51it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:03<00:01, 23.91it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:03<00:01, 23.91it/s]

    Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:03<00:01, 23.91it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:03<00:01, 23.91it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:03<00:01, 23.91it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:03<00:01, 25.72it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:03<00:01, 25.72it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:03<00:01, 25.72it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:03<00:01, 25.72it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:03<00:01, 25.72it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:03<00:00, 28.64it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:03<00:00, 28.64it/s]

    Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:03<00:00, 28.64it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:03<00:00, 28.64it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:04<00:00, 28.64it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:04<00:00, 29.75it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:04<00:00, 29.75it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:04<00:00, 29.75it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:04<00:00, 29.75it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:04<00:00, 29.75it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:04<00:00, 29.75it/s]

    Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:04<00:00, 32.55it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:04<00:00, 32.55it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:04<00:00, 32.55it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:04<00:00, 32.55it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:04<00:00, 32.55it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:04<00:00, 32.35it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:04<00:00, 32.35it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:04<00:00, 32.35it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:04<00:00, 32.35it/s]

    Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:04<00:00, 32.35it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:04<00:00, 34.27it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:04<00:00, 34.27it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:04<00:00, 34.27it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:04<00:00, 34.27it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:04<00:00, 34.27it/s]Compiling num tokens (num_tokens=8):  90%|████████▉ | 52/58 [00:04<00:00, 34.27it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:04<00:00, 37.44it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:04<00:00, 37.44it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 12.85it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=58.44 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=58.44 GB):   2%|▏         | 1/58 [00:00<00:07,  7.34it/s]Capturing num tokens (num_tokens=7680 avail_mem=58.41 GB):   2%|▏         | 1/58 [00:00<00:07,  7.34it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=58.41 GB):   3%|▎         | 2/58 [00:00<00:07,  7.31it/s]Capturing num tokens (num_tokens=7168 avail_mem=58.41 GB):   3%|▎         | 2/58 [00:00<00:07,  7.31it/s]Capturing num tokens (num_tokens=7168 avail_mem=58.41 GB):   5%|▌         | 3/58 [00:00<00:08,  6.71it/s]Capturing num tokens (num_tokens=6656 avail_mem=58.41 GB):   5%|▌         | 3/58 [00:00<00:08,  6.71it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=58.41 GB):   7%|▋         | 4/58 [00:00<00:07,  6.80it/s]Capturing num tokens (num_tokens=6144 avail_mem=58.41 GB):   7%|▋         | 4/58 [00:00<00:07,  6.80it/s]Capturing num tokens (num_tokens=6144 avail_mem=58.41 GB):   9%|▊         | 5/58 [00:00<00:07,  7.32it/s]Capturing num tokens (num_tokens=5632 avail_mem=58.40 GB):   9%|▊         | 5/58 [00:00<00:07,  7.32it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=58.40 GB):  10%|█         | 6/58 [00:00<00:06,  7.81it/s]Capturing num tokens (num_tokens=5120 avail_mem=58.41 GB):  10%|█         | 6/58 [00:00<00:06,  7.81it/s]Capturing num tokens (num_tokens=5120 avail_mem=58.41 GB):  12%|█▏        | 7/58 [00:00<00:06,  8.18it/s]Capturing num tokens (num_tokens=4608 avail_mem=58.40 GB):  12%|█▏        | 7/58 [00:00<00:06,  8.18it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=58.40 GB):  14%|█▍        | 8/58 [00:01<00:05,  8.37it/s]Capturing num tokens (num_tokens=4096 avail_mem=58.40 GB):  14%|█▍        | 8/58 [00:01<00:05,  8.37it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=58.40 GB):  16%|█▌        | 9/58 [00:01<00:07,  6.69it/s]Capturing num tokens (num_tokens=3840 avail_mem=58.40 GB):  16%|█▌        | 9/58 [00:01<00:07,  6.69it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=58.40 GB):  17%|█▋        | 10/58 [00:01<00:08,  6.00it/s]Capturing num tokens (num_tokens=3584 avail_mem=58.39 GB):  17%|█▋        | 10/58 [00:01<00:08,  6.00it/s]Capturing num tokens (num_tokens=3584 avail_mem=58.39 GB):  19%|█▉        | 11/58 [00:01<00:06,  6.81it/s]Capturing num tokens (num_tokens=3328 avail_mem=58.39 GB):  19%|█▉        | 11/58 [00:01<00:06,  6.81it/s]Capturing num tokens (num_tokens=3072 avail_mem=58.39 GB):  19%|█▉        | 11/58 [00:01<00:06,  6.81it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=58.39 GB):  22%|██▏       | 13/58 [00:01<00:05,  8.21it/s]Capturing num tokens (num_tokens=2816 avail_mem=58.39 GB):  22%|██▏       | 13/58 [00:01<00:05,  8.21it/s]Capturing num tokens (num_tokens=2560 avail_mem=58.38 GB):  22%|██▏       | 13/58 [00:01<00:05,  8.21it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=58.38 GB):  26%|██▌       | 15/58 [00:01<00:05,  8.46it/s]Capturing num tokens (num_tokens=2304 avail_mem=58.38 GB):  26%|██▌       | 15/58 [00:01<00:05,  8.46it/s]Capturing num tokens (num_tokens=2048 avail_mem=58.37 GB):  26%|██▌       | 15/58 [00:02<00:05,  8.46it/s]Capturing num tokens (num_tokens=2048 avail_mem=58.37 GB):  29%|██▉       | 17/58 [00:02<00:04,  9.33it/s]Capturing num tokens (num_tokens=1792 avail_mem=58.37 GB):  29%|██▉       | 17/58 [00:02<00:04,  9.33it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=58.37 GB):  29%|██▉       | 17/58 [00:02<00:04,  9.33it/s]Capturing num tokens (num_tokens=1536 avail_mem=58.37 GB):  33%|███▎      | 19/58 [00:02<00:03, 10.01it/s]Capturing num tokens (num_tokens=1280 avail_mem=58.36 GB):  33%|███▎      | 19/58 [00:02<00:03, 10.01it/s]Capturing num tokens (num_tokens=1024 avail_mem=58.34 GB):  33%|███▎      | 19/58 [00:02<00:03, 10.01it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=58.34 GB):  36%|███▌      | 21/58 [00:02<00:03, 10.69it/s]Capturing num tokens (num_tokens=960 avail_mem=58.36 GB):  36%|███▌      | 21/58 [00:02<00:03, 10.69it/s] Capturing num tokens (num_tokens=896 avail_mem=58.35 GB):  36%|███▌      | 21/58 [00:02<00:03, 10.69it/s]Capturing num tokens (num_tokens=896 avail_mem=58.35 GB):  40%|███▉      | 23/58 [00:02<00:03, 11.22it/s]Capturing num tokens (num_tokens=832 avail_mem=58.35 GB):  40%|███▉      | 23/58 [00:02<00:03, 11.22it/s]

    Capturing num tokens (num_tokens=768 avail_mem=58.35 GB):  40%|███▉      | 23/58 [00:02<00:03, 11.22it/s]Capturing num tokens (num_tokens=768 avail_mem=58.35 GB):  43%|████▎     | 25/58 [00:02<00:02, 11.72it/s]Capturing num tokens (num_tokens=704 avail_mem=58.34 GB):  43%|████▎     | 25/58 [00:02<00:02, 11.72it/s]Capturing num tokens (num_tokens=640 avail_mem=58.34 GB):  43%|████▎     | 25/58 [00:02<00:02, 11.72it/s]Capturing num tokens (num_tokens=640 avail_mem=58.34 GB):  47%|████▋     | 27/58 [00:02<00:02, 13.23it/s]Capturing num tokens (num_tokens=576 avail_mem=58.34 GB):  47%|████▋     | 27/58 [00:02<00:02, 13.23it/s]

    Capturing num tokens (num_tokens=512 avail_mem=58.33 GB):  47%|████▋     | 27/58 [00:02<00:02, 13.23it/s]Capturing num tokens (num_tokens=480 avail_mem=58.34 GB):  47%|████▋     | 27/58 [00:02<00:02, 13.23it/s]Capturing num tokens (num_tokens=480 avail_mem=58.34 GB):  52%|█████▏    | 30/58 [00:03<00:01, 15.33it/s]Capturing num tokens (num_tokens=448 avail_mem=58.34 GB):  52%|█████▏    | 30/58 [00:03<00:01, 15.33it/s]Capturing num tokens (num_tokens=416 avail_mem=58.34 GB):  52%|█████▏    | 30/58 [00:03<00:01, 15.33it/s]

    Capturing num tokens (num_tokens=416 avail_mem=58.34 GB):  55%|█████▌    | 32/58 [00:03<00:01, 14.49it/s]Capturing num tokens (num_tokens=384 avail_mem=58.34 GB):  55%|█████▌    | 32/58 [00:03<00:01, 14.49it/s]Capturing num tokens (num_tokens=352 avail_mem=58.33 GB):  55%|█████▌    | 32/58 [00:03<00:01, 14.49it/s]Capturing num tokens (num_tokens=352 avail_mem=58.33 GB):  59%|█████▊    | 34/58 [00:03<00:01, 13.77it/s]Capturing num tokens (num_tokens=320 avail_mem=58.33 GB):  59%|█████▊    | 34/58 [00:03<00:01, 13.77it/s]

    Capturing num tokens (num_tokens=288 avail_mem=58.33 GB):  59%|█████▊    | 34/58 [00:03<00:01, 13.77it/s]Capturing num tokens (num_tokens=288 avail_mem=58.33 GB):  62%|██████▏   | 36/58 [00:03<00:01, 13.88it/s]Capturing num tokens (num_tokens=256 avail_mem=58.32 GB):  62%|██████▏   | 36/58 [00:03<00:01, 13.88it/s]Capturing num tokens (num_tokens=240 avail_mem=58.32 GB):  62%|██████▏   | 36/58 [00:03<00:01, 13.88it/s]

    Capturing num tokens (num_tokens=240 avail_mem=58.32 GB):  66%|██████▌   | 38/58 [00:03<00:01, 13.57it/s]Capturing num tokens (num_tokens=224 avail_mem=58.32 GB):  66%|██████▌   | 38/58 [00:03<00:01, 13.57it/s]Capturing num tokens (num_tokens=208 avail_mem=58.31 GB):  66%|██████▌   | 38/58 [00:03<00:01, 13.57it/s]Capturing num tokens (num_tokens=208 avail_mem=58.31 GB):  69%|██████▉   | 40/58 [00:03<00:01, 13.44it/s]Capturing num tokens (num_tokens=192 avail_mem=58.31 GB):  69%|██████▉   | 40/58 [00:03<00:01, 13.44it/s]

    Capturing num tokens (num_tokens=176 avail_mem=58.31 GB):  69%|██████▉   | 40/58 [00:03<00:01, 13.44it/s]Capturing num tokens (num_tokens=176 avail_mem=58.31 GB):  72%|███████▏  | 42/58 [00:03<00:01, 13.38it/s]Capturing num tokens (num_tokens=160 avail_mem=58.31 GB):  72%|███████▏  | 42/58 [00:03<00:01, 13.38it/s]Capturing num tokens (num_tokens=144 avail_mem=58.30 GB):  72%|███████▏  | 42/58 [00:04<00:01, 13.38it/s]

    Capturing num tokens (num_tokens=144 avail_mem=58.30 GB):  76%|███████▌  | 44/58 [00:04<00:01, 13.30it/s]Capturing num tokens (num_tokens=128 avail_mem=58.30 GB):  76%|███████▌  | 44/58 [00:04<00:01, 13.30it/s]Capturing num tokens (num_tokens=112 avail_mem=58.30 GB):  76%|███████▌  | 44/58 [00:04<00:01, 13.30it/s]Capturing num tokens (num_tokens=112 avail_mem=58.30 GB):  79%|███████▉  | 46/58 [00:04<00:00, 13.33it/s]Capturing num tokens (num_tokens=96 avail_mem=58.29 GB):  79%|███████▉  | 46/58 [00:04<00:00, 13.33it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=58.29 GB):  79%|███████▉  | 46/58 [00:04<00:00, 13.33it/s]Capturing num tokens (num_tokens=80 avail_mem=58.29 GB):  83%|████████▎ | 48/58 [00:04<00:00, 13.32it/s]Capturing num tokens (num_tokens=64 avail_mem=58.29 GB):  83%|████████▎ | 48/58 [00:04<00:00, 13.32it/s]Capturing num tokens (num_tokens=48 avail_mem=58.28 GB):  83%|████████▎ | 48/58 [00:04<00:00, 13.32it/s]

    Capturing num tokens (num_tokens=48 avail_mem=58.28 GB):  86%|████████▌ | 50/58 [00:04<00:00, 13.45it/s]Capturing num tokens (num_tokens=32 avail_mem=58.28 GB):  86%|████████▌ | 50/58 [00:04<00:00, 13.45it/s]Capturing num tokens (num_tokens=28 avail_mem=58.27 GB):  86%|████████▌ | 50/58 [00:04<00:00, 13.45it/s]Capturing num tokens (num_tokens=28 avail_mem=58.27 GB):  90%|████████▉ | 52/58 [00:04<00:00, 13.67it/s]Capturing num tokens (num_tokens=24 avail_mem=58.27 GB):  90%|████████▉ | 52/58 [00:04<00:00, 13.67it/s]

    Capturing num tokens (num_tokens=20 avail_mem=58.27 GB):  90%|████████▉ | 52/58 [00:04<00:00, 13.67it/s]Capturing num tokens (num_tokens=20 avail_mem=58.27 GB):  93%|█████████▎| 54/58 [00:04<00:00, 13.38it/s]Capturing num tokens (num_tokens=16 avail_mem=58.27 GB):  93%|█████████▎| 54/58 [00:04<00:00, 13.38it/s]

    Capturing num tokens (num_tokens=12 avail_mem=58.26 GB):  93%|█████████▎| 54/58 [00:05<00:00, 13.38it/s]Capturing num tokens (num_tokens=12 avail_mem=58.26 GB):  97%|█████████▋| 56/58 [00:05<00:00, 11.25it/s]Capturing num tokens (num_tokens=8 avail_mem=58.26 GB):  97%|█████████▋| 56/58 [00:05<00:00, 11.25it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=58.26 GB):  97%|█████████▋| 56/58 [00:05<00:00, 11.25it/s]Capturing num tokens (num_tokens=4 avail_mem=58.26 GB): 100%|██████████| 58/58 [00:05<00:00, 10.26it/s]Capturing num tokens (num_tokens=4 avail_mem=58.26 GB): 100%|██████████| 58/58 [00:05<00:00, 10.86it/s]


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
    Generated text:  Florencia and I am a passionate writer and illustrator. I have created a portfolio that showcases my work and have published my first book, "Subway," on September 10, 2019.
    I have a background in graphic design and I have designed logos, posters, and websites for clients such as Harvard, MCA, and many others. I was also a graphic designer in the animation department of the production company "Entertainment For All" from 2008 to 2010.
    I have always loved to create art and I started to make art in 2006 when I
    ===============================
    Prompt: The president of the United States is
    Generated text:  trying to decide whether to issue an executive order on environmental regulations or not. He has a budget of $100,000 available for this purpose. The cost of issuing the executive order on environmental regulations is $5,000. The president's advisor suggests that the executive order could be implemented if the cost exceeds $5,000. He also believes that if the cost of the order is $5,000 or less, it will be beneficial to the country. He can only implement the order if it is beneficial for the country.
    
    Assuming that the cost of the executive order on environmental regulations
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. It is located on the banks of the Seine River in the north of the country. It is one of the most important cities in Europe. Paris is very beautiful. It is famous for its tall buildings, wide streets, and beautiful parks. It has a long history. Many of the city's historical buildings are in the city center. The city is one of the oldest in Europe. It is often called "The City of Light". Many of the cities attractions include the Louvre museum, the Notre Dame Cathedral, the Eiffel Tower, the Champs-Elysées, the metro system and the Bataclles
    ===============================
    Prompt: The future of AI is
    Generated text:  more complex than ever. More than just a technology, AI is transforming every aspect of society and businesses. Whether it’s the way we learn, work, or communicate, AI is changing the way we live and work.
    AI is used in a wide range of industries and sectors, from healthcare to finance to transportation. It’s used to automate processes, improve accuracy, and enhance decision-making. AI is also being used to develop new technologies and innovations that have the potential to revolutionize the way we live and work.
    However, the use of AI is not without its challenges. One of the biggest challenges is the ethical considerations surrounding AI. How


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


    Generated text:  Paris, also known as the City of Light. It is a bustling metropolis with a rich history and a diverse population of over 10 million people. The city is known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum, as well as its vibrant arts scene and culinary delights. Paris is a popular tourist destination and a major center for business and finance, with many international companies and institutions headquartered there. The city is also home to many cultural institutions, including the Louvre Museum, the Musée d'Orsay, and the Musée Rodin. Overall, Paris
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the way we interact with technology and the world around us. Here are some possible future trends in AI:
    
    1. Increased automation and artificial intelligence: As AI technology continues to advance, we can expect to see more automation and artificial intelligence in our daily lives. This could include things like self-driving cars, robots in manufacturing, and even virtual assistants that can assist with tasks like scheduling appointments and managing finances.
    
    2. Improved privacy and security: As AI technology becomes more advanced, we can expect to see more privacy and security concerns. This could include things like the use of AI
    


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
    Generated text:  [Name] and I am a [Age] year-old male. I come from a humble background, but I have come to realize that I have the unique ability to help others. I enjoy solving complex puzzles and helping people when they need it most. I am a competitive person, and I have a great sense of humor. I love to play sports, particularly baseball, and I enjoy sharing my knowledge with others. I believe in hard work and perseverance, and I strive to do my best for the people I care about. Thank you for considering me for a potential partner. 
    
    Note: Replace [Name] with your real name,
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. 
    
    To answer this question, I will follow these steps:
    
    1. Recall the capital city of France.
    2. Formulate a concise statement incorporating that information.
    
    Step 1: Recalling the capital city of France
    The capital city of France is Paris.
    
    Step 2: Formulating the concise statement
    "Paris is the capital city of France."
    
    This concise statement encapsulates the fact that Paris is the main city in France, serving as its political, cultural, and economic center. The statement is brief yet informative, meeting the requirement of being concise. 
    
    The complete statement could also be: "The capital of France,
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be characterized by rapid advancements and innovations that will continue to reshape the way we live, work, and interact with technology. Here are some possible trends that may shape the future of AI:
    
    1. Increased Personalization: As AI becomes more sophisticated, it will be able to learn and adapt to our personal preferences and needs. This will result in more personalized experiences for users, from recommendation systems to virtual assistants that can understand and respond to our queries.
    
    2. Improved Transparency: As AI systems become more advanced, they will be able to be more transparent and explainable. This will help users understand how the system is making decisions, and


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

     [

    Age

    ].

     I

    'm

     a

     [

    Occup

    ation

    ]

     who

     has

     been

     working

     hard

     to

     achieve

     my

     [

    Career

     Goal

    ].

     If

     there

    's

     anything

     I

     can

     do

     to

     help

     you

    ,

     let

     me

     know

    ,

     and

     I

    'll

     be

     more

     than

     happy

     to

     assist

     you

    .

     I

    'm

     excited

     to

     chat

    !

     You

    ?

     [

    Respond

     to

     the

     question

     with

     "

    I

    'm

    ...

    "].

     Hi

    !

     My

     name

     is

     [

    Name

    ],

     and

     I

    'm

     [

    Age

    ].

     I

    'm

     a

     [

    Occup

    ation

    ]

     who

     has

     been

     working

     hard

     to

     achieve

     my

     [

    Career

     Goal

    ].

     If

     there

    's

     anything

     I

     can

     do

     to

     help

     you

    ,

     let

     me

     know

    ,

     and

     I

    'll

     be

     more

     than

     happy

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

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

     serves

     as

     its

     cultural

    ,

     economic

    ,

     and

     political

     center

    .
    


    That

    's

     correct

    .

     Paris

     is

     the

     capital

     city

     of

     France

    ,

     located

     in

     the

     north

    western

     part

     of

     the

     country

    .

     It

     is

     known

     for

     its

     vibrant

     culture

    ,

     iconic

     landmarks

    ,

     and

     influence

     on

     French

     literature

    ,

     art

    ,

     and

     cuisine

    .

     Paris

     is

     also

     a

     major

     tourist

     destination

    ,

     attracting

     millions

     of

     visitors

     each

     year

    .

     With

     its

     rich

     history

     and

     distinctive

     architecture

    ,

     Paris

     has

     become

     one

     of

     the

     most

     iconic

     cities

     in

     the

     world

    .

     The

     city

    's

     name

     means

     "

    the

     city

     of

     light

    "

     in

     French

    ,

     referring

     to

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

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     involve

     several

     key

     trends

     that

     are

     likely

     to

     shape

     the

     industry

     and

     its

     applications

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

     AI

     Transparency

     and

     Explain

    ability

    :

     With

     the

     increase

     in

     data

     collection

     and

     analysis

    ,

     it

    's

     becoming

     more

     challenging

     to

     know

     how

     AI

     algorithms

     are

     making

     decisions

     and

     interpreting

     the

     data

    .

     Therefore

    ,

     there

     is

     a

     need

     for

     increased

     transparency

     and

     explain

    ability

     in

     AI

     systems

    .

     This

     will

     help

     users

     understand

     how

     AI

     is

     making

     decisions

     and

     ensure

     they

     are

     not

     being

     deceived

     by

     the

     algorithm

    .
    


    2

    .

     AI

     Integration

     with

     Human

     Roles

    :

     AI

     is

     likely

     to

     integrate

     more

     with

     human

     roles

    ,

     particularly

     in

     healthcare

     and

     customer

     service

    ,

    



```python
llm.shutdown()
```
