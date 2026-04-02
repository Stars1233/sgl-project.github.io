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


    2026-04-02 18:36:12.664 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 18:36:12] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 18:36:12.664 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 18:36:12] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 18:36:12.664 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 18:36:12] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 18:36:12.664 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 18:36:12] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 18:36:12.664 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 18:36:12] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  3.44it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  3.44it/s]


    2026-04-02 18:36:17,105 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 18:36:17] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:40,  2.81s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:40,  2.81s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<01:08,  1.23s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<01:08,  1.23s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:03<01:08,  1.23s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:27,  2.00it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:27,  2.00it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:03<00:27,  2.00it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:03<00:15,  3.44it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:03<00:15,  3.44it/s]Compiling num tokens (num_tokens=4608):  10%|█         | 6/58 [00:03<00:15,  3.44it/s]

    Compiling num tokens (num_tokens=4096):  10%|█         | 6/58 [00:03<00:15,  3.44it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:03<00:08,  5.99it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:03<00:08,  5.99it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:03<00:08,  5.99it/s]Compiling num tokens (num_tokens=3328):  16%|█▌        | 9/58 [00:03<00:08,  5.99it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:03<00:05,  8.93it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:03<00:05,  8.93it/s]Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:03<00:05,  8.93it/s]Compiling num tokens (num_tokens=2560):  21%|██        | 12/58 [00:03<00:05,  8.93it/s]

    Compiling num tokens (num_tokens=2304):  21%|██        | 12/58 [00:03<00:05,  8.93it/s]Compiling num tokens (num_tokens=2048):  21%|██        | 12/58 [00:03<00:05,  8.93it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:03<00:02, 14.67it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:03<00:02, 14.67it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:03<00:02, 14.67it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:03<00:02, 14.67it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:03<00:02, 17.13it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:03<00:02, 17.13it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:03<00:02, 17.13it/s] 

    Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:03<00:02, 17.13it/s]Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:03<00:02, 17.13it/s]Compiling num tokens (num_tokens=768):  34%|███▍      | 20/58 [00:03<00:02, 17.13it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 22.40it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 22.40it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 22.40it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 22.40it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 22.40it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:03<00:01, 24.63it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:03<00:01, 24.63it/s]

    Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:03<00:01, 24.63it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:03<00:01, 24.63it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:03<00:01, 24.63it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:04<00:00, 26.79it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:04<00:00, 26.79it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:04<00:00, 26.79it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:04<00:00, 26.79it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:04<00:00, 26.79it/s]

    Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:04<00:00, 28.08it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:04<00:00, 28.08it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:04<00:00, 28.08it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:04<00:00, 28.08it/s]Compiling num tokens (num_tokens=192):  64%|██████▍   | 37/58 [00:04<00:00, 28.08it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:04<00:00, 29.58it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:04<00:00, 29.58it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:04<00:00, 29.58it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:04<00:00, 29.58it/s]

    Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:04<00:00, 29.58it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:04<00:00, 30.46it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:04<00:00, 30.46it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:04<00:00, 30.46it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:04<00:00, 30.46it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:04<00:00, 30.46it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:04<00:00, 30.95it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:04<00:00, 30.95it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:04<00:00, 30.95it/s]

    Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:04<00:00, 30.95it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:04<00:00, 30.95it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:04<00:00, 32.75it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:04<00:00, 32.75it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:04<00:00, 32.75it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:04<00:00, 32.75it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:04<00:00, 32.75it/s] Compiling num tokens (num_tokens=4):  91%|█████████▏| 53/58 [00:04<00:00, 32.75it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 35.85it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 12.24it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=45.28 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=45.28 GB):   2%|▏         | 1/58 [00:00<00:07,  7.28it/s]Capturing num tokens (num_tokens=7680 avail_mem=45.25 GB):   2%|▏         | 1/58 [00:00<00:07,  7.28it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=45.25 GB):   3%|▎         | 2/58 [00:00<00:07,  7.22it/s]Capturing num tokens (num_tokens=7168 avail_mem=45.25 GB):   3%|▎         | 2/58 [00:00<00:07,  7.22it/s]Capturing num tokens (num_tokens=7168 avail_mem=45.25 GB):   5%|▌         | 3/58 [00:00<00:07,  7.40it/s]Capturing num tokens (num_tokens=6656 avail_mem=45.25 GB):   5%|▌         | 3/58 [00:00<00:07,  7.40it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=45.25 GB):   7%|▋         | 4/58 [00:00<00:07,  7.57it/s]Capturing num tokens (num_tokens=6144 avail_mem=45.25 GB):   7%|▋         | 4/58 [00:00<00:07,  7.57it/s]Capturing num tokens (num_tokens=6144 avail_mem=45.25 GB):   9%|▊         | 5/58 [00:00<00:06,  7.80it/s]Capturing num tokens (num_tokens=5632 avail_mem=45.25 GB):   9%|▊         | 5/58 [00:00<00:06,  7.80it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=45.25 GB):  10%|█         | 6/58 [00:00<00:06,  8.13it/s]Capturing num tokens (num_tokens=5120 avail_mem=45.25 GB):  10%|█         | 6/58 [00:00<00:06,  8.13it/s]Capturing num tokens (num_tokens=5120 avail_mem=45.25 GB):  12%|█▏        | 7/58 [00:00<00:06,  8.43it/s]Capturing num tokens (num_tokens=4608 avail_mem=45.24 GB):  12%|█▏        | 7/58 [00:00<00:06,  8.43it/s]Capturing num tokens (num_tokens=4096 avail_mem=45.24 GB):  12%|█▏        | 7/58 [00:00<00:06,  8.43it/s]Capturing num tokens (num_tokens=3840 avail_mem=45.24 GB):  12%|█▏        | 7/58 [00:00<00:06,  8.43it/s]Capturing num tokens (num_tokens=3584 avail_mem=45.23 GB):  12%|█▏        | 7/58 [00:00<00:06,  8.43it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=45.23 GB):  19%|█▉        | 11/58 [00:00<00:02, 16.73it/s]Capturing num tokens (num_tokens=3328 avail_mem=45.23 GB):  19%|█▉        | 11/58 [00:00<00:02, 16.73it/s]Capturing num tokens (num_tokens=3072 avail_mem=45.23 GB):  19%|█▉        | 11/58 [00:01<00:02, 16.73it/s]Capturing num tokens (num_tokens=2816 avail_mem=45.23 GB):  19%|█▉        | 11/58 [00:01<00:02, 16.73it/s]Capturing num tokens (num_tokens=2560 avail_mem=45.22 GB):  19%|█▉        | 11/58 [00:01<00:02, 16.73it/s]Capturing num tokens (num_tokens=2304 avail_mem=45.22 GB):  19%|█▉        | 11/58 [00:01<00:02, 16.73it/s]Capturing num tokens (num_tokens=2304 avail_mem=45.22 GB):  28%|██▊       | 16/58 [00:01<00:01, 25.42it/s]Capturing num tokens (num_tokens=2048 avail_mem=45.22 GB):  28%|██▊       | 16/58 [00:01<00:01, 25.42it/s]Capturing num tokens (num_tokens=1792 avail_mem=45.21 GB):  28%|██▊       | 16/58 [00:01<00:01, 25.42it/s]Capturing num tokens (num_tokens=1536 avail_mem=45.21 GB):  28%|██▊       | 16/58 [00:01<00:01, 25.42it/s]Capturing num tokens (num_tokens=1280 avail_mem=45.20 GB):  28%|██▊       | 16/58 [00:01<00:01, 25.42it/s]Capturing num tokens (num_tokens=1024 avail_mem=45.18 GB):  28%|██▊       | 16/58 [00:01<00:01, 25.42it/s]

    Capturing num tokens (num_tokens=960 avail_mem=45.20 GB):  28%|██▊       | 16/58 [00:01<00:01, 25.42it/s] Capturing num tokens (num_tokens=960 avail_mem=45.20 GB):  38%|███▊      | 22/58 [00:01<00:01, 33.36it/s]Capturing num tokens (num_tokens=896 avail_mem=45.20 GB):  38%|███▊      | 22/58 [00:01<00:01, 33.36it/s]Capturing num tokens (num_tokens=832 avail_mem=45.19 GB):  38%|███▊      | 22/58 [00:01<00:01, 33.36it/s]Capturing num tokens (num_tokens=768 avail_mem=45.19 GB):  38%|███▊      | 22/58 [00:01<00:01, 33.36it/s]Capturing num tokens (num_tokens=704 avail_mem=45.19 GB):  38%|███▊      | 22/58 [00:01<00:01, 33.36it/s]Capturing num tokens (num_tokens=640 avail_mem=45.18 GB):  38%|███▊      | 22/58 [00:01<00:01, 33.36it/s]Capturing num tokens (num_tokens=640 avail_mem=45.18 GB):  47%|████▋     | 27/58 [00:01<00:00, 37.58it/s]Capturing num tokens (num_tokens=576 avail_mem=45.18 GB):  47%|████▋     | 27/58 [00:01<00:00, 37.58it/s]Capturing num tokens (num_tokens=512 avail_mem=45.17 GB):  47%|████▋     | 27/58 [00:01<00:00, 37.58it/s]Capturing num tokens (num_tokens=480 avail_mem=45.19 GB):  47%|████▋     | 27/58 [00:01<00:00, 37.58it/s]Capturing num tokens (num_tokens=448 avail_mem=45.18 GB):  47%|████▋     | 27/58 [00:01<00:00, 37.58it/s]

    Capturing num tokens (num_tokens=416 avail_mem=45.18 GB):  47%|████▋     | 27/58 [00:01<00:00, 37.58it/s]Capturing num tokens (num_tokens=416 avail_mem=45.18 GB):  55%|█████▌    | 32/58 [00:01<00:00, 37.51it/s]Capturing num tokens (num_tokens=384 avail_mem=45.18 GB):  55%|█████▌    | 32/58 [00:01<00:00, 37.51it/s]Capturing num tokens (num_tokens=352 avail_mem=45.17 GB):  55%|█████▌    | 32/58 [00:01<00:00, 37.51it/s]Capturing num tokens (num_tokens=320 avail_mem=45.17 GB):  55%|█████▌    | 32/58 [00:01<00:00, 37.51it/s]Capturing num tokens (num_tokens=288 avail_mem=45.17 GB):  55%|█████▌    | 32/58 [00:01<00:00, 37.51it/s]Capturing num tokens (num_tokens=288 avail_mem=45.17 GB):  62%|██████▏   | 36/58 [00:01<00:00, 37.14it/s]Capturing num tokens (num_tokens=256 avail_mem=45.17 GB):  62%|██████▏   | 36/58 [00:01<00:00, 37.14it/s]Capturing num tokens (num_tokens=240 avail_mem=45.16 GB):  62%|██████▏   | 36/58 [00:01<00:00, 37.14it/s]

    Capturing num tokens (num_tokens=224 avail_mem=45.16 GB):  62%|██████▏   | 36/58 [00:01<00:00, 37.14it/s]Capturing num tokens (num_tokens=208 avail_mem=45.16 GB):  62%|██████▏   | 36/58 [00:01<00:00, 37.14it/s]Capturing num tokens (num_tokens=208 avail_mem=45.16 GB):  69%|██████▉   | 40/58 [00:01<00:00, 36.86it/s]Capturing num tokens (num_tokens=192 avail_mem=45.16 GB):  69%|██████▉   | 40/58 [00:01<00:00, 36.86it/s]Capturing num tokens (num_tokens=176 avail_mem=45.15 GB):  69%|██████▉   | 40/58 [00:01<00:00, 36.86it/s]Capturing num tokens (num_tokens=160 avail_mem=45.15 GB):  69%|██████▉   | 40/58 [00:01<00:00, 36.86it/s]Capturing num tokens (num_tokens=144 avail_mem=45.14 GB):  69%|██████▉   | 40/58 [00:01<00:00, 36.86it/s]

    Capturing num tokens (num_tokens=144 avail_mem=45.14 GB):  76%|███████▌  | 44/58 [00:01<00:00, 29.02it/s]Capturing num tokens (num_tokens=128 avail_mem=45.14 GB):  76%|███████▌  | 44/58 [00:01<00:00, 29.02it/s]Capturing num tokens (num_tokens=112 avail_mem=45.14 GB):  76%|███████▌  | 44/58 [00:01<00:00, 29.02it/s]Capturing num tokens (num_tokens=96 avail_mem=45.14 GB):  76%|███████▌  | 44/58 [00:02<00:00, 29.02it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=45.13 GB):  76%|███████▌  | 44/58 [00:02<00:00, 29.02it/s]Capturing num tokens (num_tokens=80 avail_mem=45.13 GB):  83%|████████▎ | 48/58 [00:02<00:00, 19.90it/s]Capturing num tokens (num_tokens=64 avail_mem=45.13 GB):  83%|████████▎ | 48/58 [00:02<00:00, 19.90it/s]Capturing num tokens (num_tokens=48 avail_mem=45.13 GB):  83%|████████▎ | 48/58 [00:02<00:00, 19.90it/s]

    Capturing num tokens (num_tokens=32 avail_mem=45.12 GB):  83%|████████▎ | 48/58 [00:02<00:00, 19.90it/s]Capturing num tokens (num_tokens=32 avail_mem=45.12 GB):  88%|████████▊ | 51/58 [00:02<00:00, 17.84it/s]Capturing num tokens (num_tokens=28 avail_mem=45.12 GB):  88%|████████▊ | 51/58 [00:02<00:00, 17.84it/s]Capturing num tokens (num_tokens=24 avail_mem=45.11 GB):  88%|████████▊ | 51/58 [00:02<00:00, 17.84it/s]Capturing num tokens (num_tokens=20 avail_mem=45.11 GB):  88%|████████▊ | 51/58 [00:02<00:00, 17.84it/s]Capturing num tokens (num_tokens=16 avail_mem=45.11 GB):  88%|████████▊ | 51/58 [00:02<00:00, 17.84it/s]Capturing num tokens (num_tokens=16 avail_mem=45.11 GB):  95%|█████████▍| 55/58 [00:02<00:00, 20.88it/s]Capturing num tokens (num_tokens=12 avail_mem=45.10 GB):  95%|█████████▍| 55/58 [00:02<00:00, 20.88it/s]

    Capturing num tokens (num_tokens=8 avail_mem=45.10 GB):  95%|█████████▍| 55/58 [00:02<00:00, 20.88it/s] Capturing num tokens (num_tokens=4 avail_mem=45.10 GB):  95%|█████████▍| 55/58 [00:02<00:00, 20.88it/s]Capturing num tokens (num_tokens=4 avail_mem=45.10 GB): 100%|██████████| 58/58 [00:02<00:00, 21.94it/s]


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
    Generated text:  Ziv and I am a PhD student in the Centre for Advanced Information Science and Knowledge Engineering at the University of Oxford.
    In this paper, I introduce a novel, expressive, and interactive encoding for the formal language of textual image analysis. This encoding employs a layered model that translates textual descriptions of images into their corresponding representation, such as 3D models, abstract representations, and summaries. The paper describes the reasoning mechanisms, its encoding, and its application to a large-scale dataset of images. It highlights the potential of this encoding for improving the transparency, flexibility, and adaptability of textual image analysis.
    One key outcome of the work is the
    ===============================
    Prompt: The president of the United States is
    Generated text:  getting ready to give a speech. He has 5 different facts about his speech to share. He wants to make sure that each fact is prominent enough to grab the audience's attention. To do this, he decides to create a new format for the facts. The format will be a sequence of facts, with each fact appearing consecutively and in order, separated by commas. The format will be in the form of "Fact 1, Fact 2, Fact 3, Fact 4, Fact 5". 
    
    Given that he has 5 facts, how many different ways can he arrange these facts in the format "Fact
    ===============================
    Prompt: The capital of France is
    Generated text:  ( )
    A. Paris
    B. Moscow
    C. Moscow
    D. London
    Answer: A
    
    In the internet, to ensure the security of sensitive data, which of the following security technologies should be used?
    A. Encryption technology
    B. Security authentication technology
    C. Access control technology
    D. Port scanning technology
    Answer: A
    
    For a 3x3 array, how many sub-arrays can be formed by selecting 3 out of the 9 positions? The number of sub-arrays is:
    A. 10
    B. 12
    C. 15
    D. 1
    ===============================
    Prompt: The future of AI is
    Generated text:  all about robots and data, so what can we do to make sure that our robots are equipped to be ethical and responsible? We might think about issues like autonomy, moral decision-making, and fairness, but there are other important areas to consider as well. It's important to understand that the goal of AI is to make the world a better place by increasing efficiency and productivity, so it's not just about making robots do things in a certain way. There are also important issues about the responsibility of humans to our machines and how we should be working with them.
    Is there a specific area in the field of AI that you think is particularly important


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


    Generated text:  [Name] and I'm a [Age] year old [Occupation]. I'm a [Type of Vehicle] [Vehicle Name] that I've been driving for [Number of Years] years. I'm currently [Current Location] and I'm always [Current Mood]. I'm a [Favorite Hobby] that I enjoy [Brief Description]. I'm [Current Goal] and I'm always [Current Goal]. I'm a [Favorite Quote] that I often [Brief Description]. I'm [Current Personality] and I'm always [Current Personality]. I'm [Current Strengths] and I'm always [Current Strengths].
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as "La Ville de Paris" and "La Grande Ouvrière." It is the largest city in France and the third largest in the world, with a population of over 2. 5 million people. Paris is known for its rich history, art, and culture, and is a major tourist destination. It is also home to many famous landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. Paris is a vibrant and dynamic city with a rich cultural scene and a strong sense of French identity. The city is also home to many important institutions and organizations, including the
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn from and adapt to human behavior. This could lead to more sophisticated and personalized AI systems that can better understand and respond to human needs.
    
    2. Greater emphasis on ethical considerations: As AI becomes more integrated with human intelligence, there will be a greater emphasis on ethical considerations. This could lead to more robust AI systems that are designed to be transparent, accountable, and responsible.
    
    3. Increased focus on AI ethics: As AI becomes more integrated with human intelligence, there will be a greater
    


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
    Generated text:  __________ and I have __________ years of experience in the __________ industry. I'm a __________, having __________ years of experience in the __________ industry, where I have __________ years of experience in __________. I have a __________, having __________ years of experience in __________. I am __________, having __________ years of experience in __________. I am __________, having __________ years of experience in __________. I have a __________, having __________ years of experience in __________. I am __________, having __________ years of experience in __________. I have a __________
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    
    What are some key facts about Paris?
    
    Pick your answer from:
     a). None of the above questions is correct;
     b). Paris has over 200 museums and is the largest city in Europe;
     c). Paris is the birthplace of the French Revolution and the home of the Eiffel Tower;
     d). Paris is the world's oldest city and has a population of over 10 million people. Based on the given options, the key facts about Paris are:
    
    c). Paris is the birthplace of the French Revolution and the home of the Eiffel Tower.
    
    This option is correct as:
    
    1. Paris
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  set to be a bright one, as the technology continues to evolve and become more sophisticated. Here are some potential future trends in AI:
    
    1. Autonomous vehicles: The increasing demand for self-driving cars is likely to drive the development of AI in this area. Autonomous vehicles can use machine learning algorithms to make decisions about traffic, lane usage, and other factors to ensure safety and efficiency.
    
    2. Advanced language models: AI is increasingly being used for language translation and generation, as well as for creating natural language processing systems that can understand and generate human language. These systems are being used for everything from customer service to business communication.
    
    3. Cybersecurity


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

     profession

     or

     role

    ]

     at

     [

    insert

     company

     or

     organization

    ].

     I

     am

     excited

     to

     meet

     you

     and

     see

     what

     you

     bring

     to

     the

     table

    .

     Please

     let

     me

     know

     if

     there

     is

     anything

     specific

     I

     should

     know

     about

     you

    .

     Good

     luck

    !

     [

    insert

     enthusiastic

     tone

    ]

     [

    insert

     character

    's

     name

    ]

     [

    insert

     character

    's

     name

    ]

     [

    insert

     character

    's

     name

    ]

     [

    insert

     character

    's

     name

    ]

     [

    insert

     character

    's

     name

    ]

     [

    insert

     character

    's

     name

    ]

     [

    insert

     character

    's

     name

    ]

     [

    insert

     character

    's

     name

    ]

     [

    insert

     character

    's

     name

    ]

     Hello

    ,

     my

     name

     is

     [

    insert

     character

    's

     name

    ].

     I

     am

     a

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     
    


    To

     elaborate

    :


    -

     Paris

     is

     the

     largest

     city

     in

     France

     by

     population

    .


    -

     It

     is

     located

     on

     the

     Î

    le

     de

     la

     C

    ité

     and

     is

     the

     main

     urban

     center

     of

     the

     country

    .


    -

     The

     city

     has

     a

     rich

     history

    ,

     particularly

     in

     the

     context

     of

     the

     French

     Revolution

     and

     the

     French

     Revolution

    .


    -

     Paris

     is

     renowned

     for

     its

     museums

    ,

     landmarks

    ,

     and

     fashion

    .


    -

     The

     city

     is

     also

     known

     for

     its

     architecture

    ,

     including

     the

     E

    iff

    el

     Tower

     and

     Notre

    -D

    ame

     Cathedral

    .

     
    


    The

     complete

     statement

    :
    


    The

     capital

     of

     France

     is

     Paris

    .

     It

     is

     the

     largest

     city

     in

     France

     by

     population

    ,

     located

     on

     the

     Î

    le

     de

     la

     C

    ité

     and

     the

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     characterized

     by

     a

     combination

     of

     rapid

     progress

     and

     a

     growing

     emphasis

     on

     ethical

     and

     social

     implications

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

     automation

     and

     deep

     learning

    :

     As

     AI

     continues

     to

     become

     more

     sophisticated

    ,

     it

     is

     likely

     to

     become

     more

     widely

     used

     in

     manufacturing

    ,

     transportation

    ,

     and

     other

     industries

    .

     This

     will

     create

     new

     job

     opportunities

     and

     potentially

     drive

     economic

     growth

    .
    


    2

    .

     AI

    -powered

     healthcare

    :

     With

     advances

     in

     AI

    -powered

     diagnostics

     and

     treatment

     algorithms

    ,

     AI

     is

     likely

     to

     become

     even

     more

     integrated

     into

     healthcare

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

    ,

     but

     also

     raise

     concerns

     about

     privacy

     and

     security

    .
    


    3

    .

     AI

     for

     climate

     change

    :

     AI

    



```python
llm.shutdown()
```
