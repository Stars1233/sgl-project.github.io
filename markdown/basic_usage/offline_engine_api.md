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


    2026-03-29 07:14:21.517 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 07:14:21] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 07:14:21.518 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 07:14:21] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 07:14:21.518 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 07:14:21] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 07:14:21.518 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 07:14:21] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 07:14:21.518 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 07:14:21] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  2.60it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  2.60it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:04,  2.19s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:04,  2.19s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<00:54,  1.04it/s]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<00:54,  1.04it/s]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<00:54,  1.04it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:21,  2.52it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:21,  2.52it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:21,  2.52it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:21,  2.52it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:02<00:09,  5.10it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:02<00:09,  5.10it/s]Compiling num tokens (num_tokens=4096):  12%|█▏        | 7/58 [00:02<00:09,  5.10it/s]

    Compiling num tokens (num_tokens=3840):  12%|█▏        | 7/58 [00:02<00:09,  5.10it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:05,  8.08it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:05,  8.08it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:05,  8.08it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:05,  8.08it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:05,  8.08it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:02<00:03, 12.31it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:02<00:03, 12.31it/s]Compiling num tokens (num_tokens=2304):  24%|██▍       | 14/58 [00:02<00:03, 12.31it/s]

    Compiling num tokens (num_tokens=2048):  24%|██▍       | 14/58 [00:02<00:03, 12.31it/s]Compiling num tokens (num_tokens=1792):  24%|██▍       | 14/58 [00:02<00:03, 12.31it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:02, 16.78it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:02, 16.78it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:02<00:02, 16.78it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:02<00:02, 16.78it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:02<00:02, 16.78it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:03<00:01, 20.41it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:03<00:01, 20.41it/s]

    Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:03<00:01, 20.41it/s]Compiling num tokens (num_tokens=768):  38%|███▊      | 22/58 [00:03<00:01, 20.41it/s]Compiling num tokens (num_tokens=704):  38%|███▊      | 22/58 [00:03<00:01, 20.41it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 23.91it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 23.91it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 23.91it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 23.91it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 23.91it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:03<00:01, 26.83it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:03<00:01, 26.83it/s]

    Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:03<00:01, 26.83it/s]Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:03<00:01, 26.83it/s]Compiling num tokens (num_tokens=352):  52%|█████▏    | 30/58 [00:03<00:01, 26.83it/s]Compiling num tokens (num_tokens=320):  52%|█████▏    | 30/58 [00:03<00:01, 26.83it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:03<00:00, 30.65it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:03<00:00, 30.65it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:03<00:00, 30.65it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:03<00:00, 30.65it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:03<00:00, 30.65it/s]

    Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 31.51it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 31.51it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 31.51it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 31.51it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 31.51it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 31.51it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 34.10it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 34.10it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 34.10it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 34.10it/s] 

    Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 34.10it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:03<00:00, 34.27it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:03<00:00, 34.27it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:03<00:00, 34.27it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:03<00:00, 34.27it/s]Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:03<00:00, 34.27it/s]Compiling num tokens (num_tokens=24):  83%|████████▎ | 48/58 [00:03<00:00, 34.27it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:03<00:00, 36.54it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:03<00:00, 36.54it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:03<00:00, 36.54it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:03<00:00, 36.54it/s]

    Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:03<00:00, 36.54it/s] Compiling num tokens (num_tokens=4):  91%|█████████▏| 53/58 [00:03<00:00, 36.54it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 14.78it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=57.77 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=57.77 GB):   2%|▏         | 1/58 [00:00<00:07,  7.86it/s]Capturing num tokens (num_tokens=7680 avail_mem=57.74 GB):   2%|▏         | 1/58 [00:00<00:07,  7.86it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=57.74 GB):   3%|▎         | 2/58 [00:00<00:06,  8.05it/s]Capturing num tokens (num_tokens=7168 avail_mem=57.74 GB):   3%|▎         | 2/58 [00:00<00:06,  8.05it/s]Capturing num tokens (num_tokens=7168 avail_mem=57.74 GB):   5%|▌         | 3/58 [00:00<00:06,  8.29it/s]Capturing num tokens (num_tokens=6656 avail_mem=57.74 GB):   5%|▌         | 3/58 [00:00<00:06,  8.29it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=57.74 GB):   7%|▋         | 4/58 [00:00<00:06,  8.60it/s]Capturing num tokens (num_tokens=6144 avail_mem=57.74 GB):   7%|▋         | 4/58 [00:00<00:06,  8.60it/s]Capturing num tokens (num_tokens=6144 avail_mem=57.74 GB):   9%|▊         | 5/58 [00:00<00:05,  8.86it/s]Capturing num tokens (num_tokens=5632 avail_mem=57.73 GB):   9%|▊         | 5/58 [00:00<00:05,  8.86it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=57.73 GB):   9%|▊         | 5/58 [00:00<00:05,  8.86it/s]Capturing num tokens (num_tokens=5120 avail_mem=57.73 GB):  12%|█▏        | 7/58 [00:00<00:05,  9.57it/s]Capturing num tokens (num_tokens=4608 avail_mem=57.73 GB):  12%|█▏        | 7/58 [00:00<00:05,  9.57it/s]Capturing num tokens (num_tokens=4096 avail_mem=57.73 GB):  12%|█▏        | 7/58 [00:00<00:05,  9.57it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=57.73 GB):  16%|█▌        | 9/58 [00:00<00:04, 10.19it/s]Capturing num tokens (num_tokens=3840 avail_mem=55.37 GB):  16%|█▌        | 9/58 [00:00<00:04, 10.19it/s]Capturing num tokens (num_tokens=3584 avail_mem=55.37 GB):  16%|█▌        | 9/58 [00:01<00:04, 10.19it/s]Capturing num tokens (num_tokens=3584 avail_mem=55.37 GB):  19%|█▉        | 11/58 [00:01<00:04, 10.79it/s]Capturing num tokens (num_tokens=3328 avail_mem=55.37 GB):  19%|█▉        | 11/58 [00:01<00:04, 10.79it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=55.36 GB):  19%|█▉        | 11/58 [00:01<00:04, 10.79it/s]Capturing num tokens (num_tokens=3072 avail_mem=55.36 GB):  22%|██▏       | 13/58 [00:01<00:03, 11.31it/s]Capturing num tokens (num_tokens=2816 avail_mem=55.36 GB):  22%|██▏       | 13/58 [00:01<00:03, 11.31it/s]Capturing num tokens (num_tokens=2560 avail_mem=55.36 GB):  22%|██▏       | 13/58 [00:01<00:03, 11.31it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=55.36 GB):  26%|██▌       | 15/58 [00:01<00:03, 11.81it/s]Capturing num tokens (num_tokens=2304 avail_mem=55.35 GB):  26%|██▌       | 15/58 [00:01<00:03, 11.81it/s]Capturing num tokens (num_tokens=2048 avail_mem=55.35 GB):  26%|██▌       | 15/58 [00:01<00:03, 11.81it/s]Capturing num tokens (num_tokens=2048 avail_mem=55.35 GB):  29%|██▉       | 17/58 [00:01<00:03, 12.16it/s]Capturing num tokens (num_tokens=1792 avail_mem=54.50 GB):  29%|██▉       | 17/58 [00:01<00:03, 12.16it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=54.49 GB):  29%|██▉       | 17/58 [00:01<00:03, 12.16it/s]Capturing num tokens (num_tokens=1536 avail_mem=54.49 GB):  33%|███▎      | 19/58 [00:01<00:03, 12.30it/s]Capturing num tokens (num_tokens=1280 avail_mem=54.49 GB):  33%|███▎      | 19/58 [00:01<00:03, 12.30it/s]Capturing num tokens (num_tokens=1024 avail_mem=54.47 GB):  33%|███▎      | 19/58 [00:01<00:03, 12.30it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=54.47 GB):  36%|███▌      | 21/58 [00:01<00:02, 12.78it/s]Capturing num tokens (num_tokens=960 avail_mem=54.48 GB):  36%|███▌      | 21/58 [00:01<00:02, 12.78it/s] Capturing num tokens (num_tokens=896 avail_mem=54.48 GB):  36%|███▌      | 21/58 [00:01<00:02, 12.78it/s]Capturing num tokens (num_tokens=896 avail_mem=54.48 GB):  40%|███▉      | 23/58 [00:02<00:02, 13.08it/s]Capturing num tokens (num_tokens=832 avail_mem=54.48 GB):  40%|███▉      | 23/58 [00:02<00:02, 13.08it/s]

    Capturing num tokens (num_tokens=768 avail_mem=54.47 GB):  40%|███▉      | 23/58 [00:02<00:02, 13.08it/s]Capturing num tokens (num_tokens=768 avail_mem=54.47 GB):  43%|████▎     | 25/58 [00:02<00:02, 13.39it/s]Capturing num tokens (num_tokens=704 avail_mem=54.47 GB):  43%|████▎     | 25/58 [00:02<00:02, 13.39it/s]Capturing num tokens (num_tokens=640 avail_mem=54.47 GB):  43%|████▎     | 25/58 [00:02<00:02, 13.39it/s]

    Capturing num tokens (num_tokens=640 avail_mem=54.47 GB):  47%|████▋     | 27/58 [00:02<00:02, 13.62it/s]Capturing num tokens (num_tokens=576 avail_mem=54.47 GB):  47%|████▋     | 27/58 [00:02<00:02, 13.62it/s]Capturing num tokens (num_tokens=512 avail_mem=54.45 GB):  47%|████▋     | 27/58 [00:02<00:02, 13.62it/s]Capturing num tokens (num_tokens=512 avail_mem=54.45 GB):  50%|█████     | 29/58 [00:02<00:02, 13.58it/s]Capturing num tokens (num_tokens=480 avail_mem=54.47 GB):  50%|█████     | 29/58 [00:02<00:02, 13.58it/s]

    Capturing num tokens (num_tokens=448 avail_mem=54.47 GB):  50%|█████     | 29/58 [00:02<00:02, 13.58it/s]Capturing num tokens (num_tokens=448 avail_mem=54.47 GB):  53%|█████▎    | 31/58 [00:02<00:01, 13.66it/s]Capturing num tokens (num_tokens=416 avail_mem=54.47 GB):  53%|█████▎    | 31/58 [00:02<00:01, 13.66it/s]Capturing num tokens (num_tokens=384 avail_mem=54.46 GB):  53%|█████▎    | 31/58 [00:02<00:01, 13.66it/s]

    Capturing num tokens (num_tokens=384 avail_mem=54.46 GB):  57%|█████▋    | 33/58 [00:02<00:01, 13.71it/s]Capturing num tokens (num_tokens=352 avail_mem=54.46 GB):  57%|█████▋    | 33/58 [00:02<00:01, 13.71it/s]Capturing num tokens (num_tokens=320 avail_mem=54.45 GB):  57%|█████▋    | 33/58 [00:02<00:01, 13.71it/s]Capturing num tokens (num_tokens=320 avail_mem=54.45 GB):  60%|██████    | 35/58 [00:02<00:01, 13.83it/s]Capturing num tokens (num_tokens=288 avail_mem=54.45 GB):  60%|██████    | 35/58 [00:02<00:01, 13.83it/s]

    Capturing num tokens (num_tokens=256 avail_mem=54.45 GB):  60%|██████    | 35/58 [00:02<00:01, 13.83it/s]Capturing num tokens (num_tokens=256 avail_mem=54.45 GB):  64%|██████▍   | 37/58 [00:03<00:01, 14.00it/s]Capturing num tokens (num_tokens=240 avail_mem=54.45 GB):  64%|██████▍   | 37/58 [00:03<00:01, 14.00it/s]Capturing num tokens (num_tokens=224 avail_mem=54.44 GB):  64%|██████▍   | 37/58 [00:03<00:01, 14.00it/s]

    Capturing num tokens (num_tokens=224 avail_mem=54.44 GB):  67%|██████▋   | 39/58 [00:03<00:01, 14.17it/s]Capturing num tokens (num_tokens=208 avail_mem=54.44 GB):  67%|██████▋   | 39/58 [00:03<00:01, 14.17it/s]Capturing num tokens (num_tokens=192 avail_mem=54.44 GB):  67%|██████▋   | 39/58 [00:03<00:01, 14.17it/s]Capturing num tokens (num_tokens=192 avail_mem=54.44 GB):  71%|███████   | 41/58 [00:03<00:01, 14.17it/s]Capturing num tokens (num_tokens=176 avail_mem=54.43 GB):  71%|███████   | 41/58 [00:03<00:01, 14.17it/s]

    Capturing num tokens (num_tokens=160 avail_mem=53.88 GB):  71%|███████   | 41/58 [00:03<00:01, 14.17it/s]Capturing num tokens (num_tokens=160 avail_mem=53.88 GB):  74%|███████▍  | 43/58 [00:03<00:01, 11.98it/s]Capturing num tokens (num_tokens=144 avail_mem=53.88 GB):  74%|███████▍  | 43/58 [00:03<00:01, 11.98it/s]

    Capturing num tokens (num_tokens=128 avail_mem=53.88 GB):  74%|███████▍  | 43/58 [00:03<00:01, 11.98it/s]Capturing num tokens (num_tokens=128 avail_mem=53.88 GB):  78%|███████▊  | 45/58 [00:03<00:01, 10.44it/s]Capturing num tokens (num_tokens=112 avail_mem=53.88 GB):  78%|███████▊  | 45/58 [00:03<00:01, 10.44it/s]

    Capturing num tokens (num_tokens=96 avail_mem=53.87 GB):  78%|███████▊  | 45/58 [00:03<00:01, 10.44it/s] Capturing num tokens (num_tokens=96 avail_mem=53.87 GB):  81%|████████  | 47/58 [00:04<00:01,  9.81it/s]Capturing num tokens (num_tokens=80 avail_mem=53.87 GB):  81%|████████  | 47/58 [00:04<00:01,  9.81it/s]

    Capturing num tokens (num_tokens=64 avail_mem=53.86 GB):  81%|████████  | 47/58 [00:04<00:01,  9.81it/s]Capturing num tokens (num_tokens=64 avail_mem=53.86 GB):  84%|████████▍ | 49/58 [00:04<00:00,  9.50it/s]Capturing num tokens (num_tokens=48 avail_mem=53.86 GB):  84%|████████▍ | 49/58 [00:04<00:00,  9.50it/s]

    Capturing num tokens (num_tokens=48 avail_mem=53.86 GB):  86%|████████▌ | 50/58 [00:04<00:00,  9.48it/s]Capturing num tokens (num_tokens=32 avail_mem=53.86 GB):  86%|████████▌ | 50/58 [00:04<00:00,  9.48it/s]Capturing num tokens (num_tokens=32 avail_mem=53.86 GB):  88%|████████▊ | 51/58 [00:04<00:00,  9.56it/s]Capturing num tokens (num_tokens=28 avail_mem=53.85 GB):  88%|████████▊ | 51/58 [00:04<00:00,  9.56it/s]Capturing num tokens (num_tokens=24 avail_mem=53.85 GB):  88%|████████▊ | 51/58 [00:04<00:00,  9.56it/s]

    Capturing num tokens (num_tokens=24 avail_mem=53.85 GB):  91%|█████████▏| 53/58 [00:04<00:00, 10.29it/s]Capturing num tokens (num_tokens=20 avail_mem=53.85 GB):  91%|█████████▏| 53/58 [00:04<00:00, 10.29it/s]Capturing num tokens (num_tokens=16 avail_mem=53.85 GB):  91%|█████████▏| 53/58 [00:04<00:00, 10.29it/s]Capturing num tokens (num_tokens=16 avail_mem=53.85 GB):  95%|█████████▍| 55/58 [00:04<00:00, 10.87it/s]Capturing num tokens (num_tokens=12 avail_mem=53.84 GB):  95%|█████████▍| 55/58 [00:04<00:00, 10.87it/s]

    Capturing num tokens (num_tokens=8 avail_mem=53.83 GB):  95%|█████████▍| 55/58 [00:04<00:00, 10.87it/s] Capturing num tokens (num_tokens=8 avail_mem=53.83 GB):  98%|█████████▊| 57/58 [00:04<00:00, 11.39it/s]Capturing num tokens (num_tokens=4 avail_mem=53.83 GB):  98%|█████████▊| 57/58 [00:04<00:00, 11.39it/s]Capturing num tokens (num_tokens=4 avail_mem=53.83 GB): 100%|██████████| 58/58 [00:05<00:00, 11.54it/s]


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
    Generated text:  Lee and I am 25 years old. I recently traveled to South Africa and I am planning to visit the Golden Mile Lakes. How can I visit the Golden Mile Lakes in South Africa?
    Traveling to South Africa and experiencing the Golden Mile Lakes sounds like an exciting adventure! Here are some tips to help you plan your visit to these beautiful lakes:
    
    1. **Check Travel Policies and Requirements:**
       - **Travel Insurance:** It's a good idea to have travel insurance that covers your health and emergency needs.
       - **National Parks Opening Hours:** Before you travel, check the official South African parks website to know the opening times
    ===============================
    Prompt: The president of the United States is
    Generated text:  a political office. The president is elected by the people, and the term of the president is four years. What is the term of the president? The term of the president is four years. This is the standard term of office for the United States president, based on the Constitution. The term of office begins on the date of the election and lasts for four years. This term is a fixed length and cannot be changed by the people. It is determined by the election and cannot be shortened. The president serves a two-year term, which lasts from January 20 to January 20 of the following year. After the presidency ends
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris, a city of which type?
    
    A) Ancient  
    B) Medieval  
    C) Modern  
    D) Medieval and Modern
    
    To determine the type of the capital of France, let's break down the information step by step.
    
    1. Identify the capital of France: Paris.
    2. Analyze the type of Paris: Paris is the capital of France, and it is a city.
    3. Compare the given options with the information: The options provided are:
       - A) Ancient
       - B) Medieval
       - C) Modern
       - D) Medieval and Modern
    
    4. Determine the correct option: Since Paris
    ===============================
    Prompt: The future of AI is
    Generated text:  more challenging than it might seem. The field is still quite young and the field of AI is very much in its infancy. The development of AI has been slowed down by several issues, including, the lack of data, the lack of programming languages and the need to focus on the development of the underlying technologies. There are numerous concerns that have been raised regarding the security and privacy of the data being handled by AI systems. The algorithmic bias and the lack of transparency in AI systems has also raised concerns. However, many organizations and individuals are looking at the future of AI with open arms, and there are several exciting developments taking place in the


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


    Generated text:  [Name], and I'm a [Age] year old [Occupation]. I'm currently [Current Location]. I'm a [Favorite Hobby] enthusiast, and I love [Favorite Food]. I'm also a [Favorite Book] lover, and I read [Number] books a year. I'm a [Favorite Movie] fan, and I watch [Number] movies a year. I'm a [Favorite Music] lover, and I listen to [Number] songs a day. I'm a [Favorite Sport] enthusiast, and I play [Number] sports a year. I'm a [Favorite Animal] lover, and I
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, which is known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French Academy of Sciences, and the French National Library. Paris is a bustling city with a rich cultural heritage and is a major tourist destination. It is also a major financial center and a major center for international business. The city is known for its fashion industry, and it is home to many famous fashion houses and boutiques. Paris is a city that is a true reflection of French culture and history. It is a city that is a must-visit
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction. Here are some of the most likely trends:
    
    1. Increased automation: AI is likely to become more prevalent in various industries, including manufacturing, healthcare, and transportation. Automation will likely lead to the development of more efficient and cost-effective systems that can perform tasks that were previously done by humans.
    
    2. AI ethics and privacy: As AI becomes more integrated into our daily lives, there will be increasing concerns about its impact on society. There will likely be a push to develop ethical guidelines and privacy protections for AI systems.
    
    3. AI for human augmentation
    


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
    Generated text:  [Name], and I am an experienced [Type of Expertise] with [Field of Expertise] who has been in the [Industry] industry for [Number of Years] years. My most notable accomplishment in [Industry] has been [Brief description of the accomplishment]. I enjoy [My Passion/Interest/Character trait]. What type of work do you specialize in?
    Thank you for asking.
    I'm [Name] and I specialize in [Industry]. My experience in [Industry] has taught me valuable skills and knowledge that I can share with you. I enjoy [My Passion/Interest/Character trait].
    Tell me more about your
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, also known as the City of Light. It is located in the north-central region of the country and is the cultural and political center of France. The city is known for its historic architecture, charming streets, and vibrant music and film industry. It is also home to the Eiffel Tower, the Louvre Museum, and the Palace of Versailles, and is considered a city of contrasts, with its rich history and modern advancements. Paris is a cultural and economic hub and is a major tourist destination. Paris is also known for its fashion and food scene, and is home to numerous world-renowned museums, theaters, and museums
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  exciting and diverse, with new technologies and applications emerging constantly. Here are some possible trends to expect in the coming years:
    
    1. Increased focus on ethics and transparency: AI systems are becoming more complex, and as such, there is a growing emphasis on improving transparency, accountability, and ethical considerations. This includes measures like explainable AI, which allows for better understanding of how AI systems make decisions and their impact on individuals.
    
    2. Advancements in big data and analytics: As more data becomes available, AI systems are becoming more powerful and capable. The ability to analyze and understand large amounts of data will allow for more accurate predictions and more effective


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

    ]

     and

     I

     am

     a

    /an

     [

    insert

     occupation

    ].

     I

     enjoy

     [

    insert

     reason

     why

     you

     enjoy

     your

     profession

    ].

     I

     have

     always

     been

     passionate

     about

     learning

     and

     improving

     myself

    ,

     which

     has

     led

     me

     to

     pursue

     [

    insert

     a

     specific

     field

     of

     study

    ,

     such

     as

     music

    ,

     science

    ,

     or

     coding

    ]

     at

     [

    insert

     your

     preferred

     school

     or

     institution

    ].

     My

     love

     for

     learning

     is

     infectious

    ,

     and

     I

     am

     always

     eager

     to

     share

     my

     knowledge

     with

     others

    ,

     regardless

     of

     the

     subject

    .

     I

     am

     always

     looking

     for

     new

     challenges

     and

     opportunities

     to

     grow

     and

     develop

     as

     a

     professional

    .

     I

     am

     committed

     to

     continuing

     my

     education

     and

     striving

     to

     be

     the

     best

     version

     of

     myself

    .

     I

     believe

     in

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     the

     

    2

    0

    th

     most

     populous

     city

     in

     the

     world

    ,

     and

     the

     

    2

    5

    th

     most

     populous

     city

     in

     Europe

    .

     Located

     on

     the

     River

     Se

    ine

    ,

     it

     is

     the

     largest

     city

     by

     area

    ,

     with

     an

     estimated

     population

     of

     over

     

    2

     million

     people

    .

     Paris

     is

     a

     UNESCO

     World

     Heritage

     site

     and

     a

     major

     international

     financial

     center

    ,

     hosting

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

     Notre

    -D

    ame

     Cathedral

    ,

     and

     the

     Lou

    vre

     Museum

    .

     The

     city

    's

     unique

     blend

     of

     Gothic

     and

     modern

     architecture

     makes

     it

     a

     UNESCO

     World

     Heritage

     site

     and

     a

     major

     center

     for

     international

     tourism

    .

     Paris

     is

     known

     for

     its

     world

    -ren

    owned

     art

     and

     music

     scenes

    ,

     cuisine

    ,

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     characterized

     by

     several

     key

     trends

    :
    


    1

    .

     Increased

     focus

     on

     ethics

     and

     safety

    :

     As

     AI

     is

     increasingly

     integrated

     into

     everyday

     life

    ,

     there

     is

     a

     growing

     need

     for

     developers

     and

     researchers

     to

     prioritize

     ethical

     considerations

     and

     safety

     concerns

    .

     This

     may

     lead

     to

     more

     rigorous

     testing

     and

     verification

     of

     AI

     systems

    ,

     as

     well

     as

     greater

     scrutiny

     of

     the

     algorithms

     and

     models

     used

     in

     AI

    .
    


    2

    .

     Greater

     integration

     with

     human

     decision

    -making

    :

     AI

     is

     increasingly

     being

     integrated

     into

     decision

    -making

     processes

    ,

     which

     could

     lead

     to

     a

     greater

     emphasis

     on

     human

     oversight

     and

     decision

    -making

    .

     This

     could

     result

     in

     more

     transparent

     and

     accountable

     AI

     systems

     that

     are

     designed

     to

     minimize

     bias

     and

     ensure

     fairness

    .
    


    3

    .

     More

     reliance

    



```python
llm.shutdown()
```
