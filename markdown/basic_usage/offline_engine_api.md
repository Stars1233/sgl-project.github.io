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


    2026-03-27 14:47:21.110 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 14:47:21] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 14:47:21.110 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 14:47:21] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 14:47:21.110 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 14:47:21] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 14:47:21.110 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 14:47:21] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 14:47:21.110 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 14:47:21] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  2.19it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  2.19it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:36,  2.75s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:36,  2.75s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<01:07,  1.21s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<01:07,  1.21s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<01:07,  1.21s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:26,  2.06it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:26,  2.06it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:03<00:26,  2.06it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:03<00:26,  2.06it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:03<00:11,  4.25it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:03<00:11,  4.25it/s]Compiling num tokens (num_tokens=4096):  12%|█▏        | 7/58 [00:03<00:11,  4.25it/s]

    Compiling num tokens (num_tokens=3840):  12%|█▏        | 7/58 [00:03<00:11,  4.25it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:03<00:07,  6.71it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:03<00:07,  6.71it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:03<00:07,  6.71it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:03<00:07,  6.71it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:03<00:07,  6.71it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:03<00:07,  6.71it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:03<00:03, 11.72it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:03<00:03, 11.72it/s]

    Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:03<00:03, 11.72it/s]Compiling num tokens (num_tokens=1792):  26%|██▌       | 15/58 [00:03<00:03, 11.72it/s]Compiling num tokens (num_tokens=1536):  26%|██▌       | 15/58 [00:03<00:03, 11.72it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:02, 15.26it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:02, 15.26it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:02, 15.26it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:02, 15.26it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:02, 15.26it/s]

    Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:03<00:01, 19.30it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:03<00:01, 19.30it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:03<00:01, 19.30it/s]Compiling num tokens (num_tokens=704):  40%|███▉      | 23/58 [00:03<00:01, 19.30it/s]Compiling num tokens (num_tokens=640):  40%|███▉      | 23/58 [00:03<00:01, 19.30it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:03<00:01, 22.45it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:03<00:01, 22.45it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:03<00:01, 22.45it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:03<00:01, 22.45it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:03<00:01, 22.45it/s]

    Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:03<00:01, 22.45it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:00, 27.27it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:00, 27.27it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:00, 27.27it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:00, 27.27it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:00, 27.27it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:03<00:00, 28.95it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:03<00:00, 28.95it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:04<00:00, 28.95it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:04<00:00, 28.95it/s]

    Compiling num tokens (num_tokens=208):  62%|██████▏   | 36/58 [00:04<00:00, 28.95it/s]Compiling num tokens (num_tokens=192):  62%|██████▏   | 36/58 [00:04<00:00, 28.95it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:04<00:00, 32.11it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:04<00:00, 32.11it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:04<00:00, 32.11it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:04<00:00, 32.11it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:04<00:00, 32.11it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:04<00:00, 32.11it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:04<00:00, 35.29it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:04<00:00, 35.29it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:04<00:00, 35.29it/s]

    Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:04<00:00, 35.29it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:04<00:00, 35.29it/s]Compiling num tokens (num_tokens=32):  79%|███████▉  | 46/58 [00:04<00:00, 35.29it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:04<00:00, 36.48it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:04<00:00, 36.48it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:04<00:00, 36.48it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:04<00:00, 36.48it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:04<00:00, 36.48it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:04<00:00, 36.48it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:04<00:00, 36.48it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:04<00:00, 36.48it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 13.12it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=43.54 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=43.54 GB):   2%|▏         | 1/58 [00:00<00:06,  8.24it/s]Capturing num tokens (num_tokens=7680 avail_mem=43.15 GB):   2%|▏         | 1/58 [00:00<00:06,  8.24it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=43.18 GB):   2%|▏         | 1/58 [00:00<00:06,  8.24it/s]Capturing num tokens (num_tokens=7168 avail_mem=43.18 GB):   5%|▌         | 3/58 [00:00<00:05, 10.19it/s]Capturing num tokens (num_tokens=6656 avail_mem=43.51 GB):   5%|▌         | 3/58 [00:00<00:05, 10.19it/s]Capturing num tokens (num_tokens=6656 avail_mem=43.51 GB):   7%|▋         | 4/58 [00:00<00:05, 10.08it/s]Capturing num tokens (num_tokens=6144 avail_mem=43.21 GB):   7%|▋         | 4/58 [00:00<00:05, 10.08it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=43.50 GB):   7%|▋         | 4/58 [00:00<00:05, 10.08it/s]Capturing num tokens (num_tokens=5632 avail_mem=43.50 GB):  10%|█         | 6/58 [00:00<00:04, 12.07it/s]Capturing num tokens (num_tokens=5120 avail_mem=43.51 GB):  10%|█         | 6/58 [00:00<00:04, 12.07it/s]Capturing num tokens (num_tokens=4608 avail_mem=43.27 GB):  10%|█         | 6/58 [00:00<00:04, 12.07it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=43.27 GB):  14%|█▍        | 8/58 [00:00<00:03, 13.69it/s]Capturing num tokens (num_tokens=4096 avail_mem=43.50 GB):  14%|█▍        | 8/58 [00:00<00:03, 13.69it/s]Capturing num tokens (num_tokens=3840 avail_mem=43.50 GB):  14%|█▍        | 8/58 [00:00<00:03, 13.69it/s]Capturing num tokens (num_tokens=3840 avail_mem=43.50 GB):  17%|█▋        | 10/58 [00:00<00:03, 14.77it/s]Capturing num tokens (num_tokens=3584 avail_mem=43.32 GB):  17%|█▋        | 10/58 [00:00<00:03, 14.77it/s]Capturing num tokens (num_tokens=3328 avail_mem=43.49 GB):  17%|█▋        | 10/58 [00:00<00:03, 14.77it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=43.48 GB):  17%|█▋        | 10/58 [00:00<00:03, 14.77it/s]Capturing num tokens (num_tokens=3072 avail_mem=43.48 GB):  22%|██▏       | 13/58 [00:00<00:02, 17.10it/s]Capturing num tokens (num_tokens=2816 avail_mem=43.48 GB):  22%|██▏       | 13/58 [00:00<00:02, 17.10it/s]Capturing num tokens (num_tokens=2560 avail_mem=43.35 GB):  22%|██▏       | 13/58 [00:00<00:02, 17.10it/s]Capturing num tokens (num_tokens=2304 avail_mem=43.47 GB):  22%|██▏       | 13/58 [00:00<00:02, 17.10it/s]Capturing num tokens (num_tokens=2304 avail_mem=43.47 GB):  28%|██▊       | 16/58 [00:01<00:02, 19.17it/s]Capturing num tokens (num_tokens=2048 avail_mem=43.46 GB):  28%|██▊       | 16/58 [00:01<00:02, 19.17it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=43.45 GB):  28%|██▊       | 16/58 [00:01<00:02, 19.17it/s]Capturing num tokens (num_tokens=1536 avail_mem=43.35 GB):  28%|██▊       | 16/58 [00:01<00:02, 19.17it/s]Capturing num tokens (num_tokens=1536 avail_mem=43.35 GB):  33%|███▎      | 19/58 [00:01<00:01, 21.23it/s]Capturing num tokens (num_tokens=1280 avail_mem=43.42 GB):  33%|███▎      | 19/58 [00:01<00:01, 21.23it/s]Capturing num tokens (num_tokens=1024 avail_mem=43.41 GB):  33%|███▎      | 19/58 [00:01<00:01, 21.23it/s]Capturing num tokens (num_tokens=960 avail_mem=43.43 GB):  33%|███▎      | 19/58 [00:01<00:01, 21.23it/s] Capturing num tokens (num_tokens=960 avail_mem=43.43 GB):  38%|███▊      | 22/58 [00:01<00:01, 22.52it/s]Capturing num tokens (num_tokens=896 avail_mem=43.42 GB):  38%|███▊      | 22/58 [00:01<00:01, 22.52it/s]

    Capturing num tokens (num_tokens=832 avail_mem=43.41 GB):  38%|███▊      | 22/58 [00:01<00:01, 22.52it/s]Capturing num tokens (num_tokens=768 avail_mem=43.36 GB):  38%|███▊      | 22/58 [00:01<00:01, 22.52it/s]Capturing num tokens (num_tokens=704 avail_mem=43.40 GB):  38%|███▊      | 22/58 [00:01<00:01, 22.52it/s]Capturing num tokens (num_tokens=704 avail_mem=43.40 GB):  45%|████▍     | 26/58 [00:01<00:01, 24.90it/s]Capturing num tokens (num_tokens=640 avail_mem=43.40 GB):  45%|████▍     | 26/58 [00:01<00:01, 24.90it/s]Capturing num tokens (num_tokens=576 avail_mem=43.39 GB):  45%|████▍     | 26/58 [00:01<00:01, 24.90it/s]Capturing num tokens (num_tokens=512 avail_mem=43.38 GB):  45%|████▍     | 26/58 [00:01<00:01, 24.90it/s]Capturing num tokens (num_tokens=480 avail_mem=43.39 GB):  45%|████▍     | 26/58 [00:01<00:01, 24.90it/s]

    Capturing num tokens (num_tokens=480 avail_mem=43.39 GB):  52%|█████▏    | 30/58 [00:01<00:01, 26.88it/s]Capturing num tokens (num_tokens=448 avail_mem=43.38 GB):  52%|█████▏    | 30/58 [00:01<00:01, 26.88it/s]Capturing num tokens (num_tokens=416 avail_mem=43.38 GB):  52%|█████▏    | 30/58 [00:01<00:01, 26.88it/s]Capturing num tokens (num_tokens=384 avail_mem=43.37 GB):  52%|█████▏    | 30/58 [00:01<00:01, 26.88it/s]Capturing num tokens (num_tokens=352 avail_mem=43.36 GB):  52%|█████▏    | 30/58 [00:01<00:01, 26.88it/s]Capturing num tokens (num_tokens=352 avail_mem=43.36 GB):  59%|█████▊    | 34/58 [00:01<00:00, 28.22it/s]Capturing num tokens (num_tokens=320 avail_mem=43.35 GB):  59%|█████▊    | 34/58 [00:01<00:00, 28.22it/s]Capturing num tokens (num_tokens=288 avail_mem=43.35 GB):  59%|█████▊    | 34/58 [00:01<00:00, 28.22it/s]Capturing num tokens (num_tokens=256 avail_mem=43.28 GB):  59%|█████▊    | 34/58 [00:01<00:00, 28.22it/s]

    Capturing num tokens (num_tokens=240 avail_mem=43.27 GB):  59%|█████▊    | 34/58 [00:01<00:00, 28.22it/s]Capturing num tokens (num_tokens=240 avail_mem=43.27 GB):  66%|██████▌   | 38/58 [00:01<00:00, 29.37it/s]Capturing num tokens (num_tokens=224 avail_mem=43.28 GB):  66%|██████▌   | 38/58 [00:01<00:00, 29.37it/s]Capturing num tokens (num_tokens=208 avail_mem=43.27 GB):  66%|██████▌   | 38/58 [00:01<00:00, 29.37it/s]Capturing num tokens (num_tokens=192 avail_mem=43.26 GB):  66%|██████▌   | 38/58 [00:01<00:00, 29.37it/s]Capturing num tokens (num_tokens=176 avail_mem=43.26 GB):  66%|██████▌   | 38/58 [00:01<00:00, 29.37it/s]Capturing num tokens (num_tokens=176 avail_mem=43.26 GB):  72%|███████▏  | 42/58 [00:01<00:00, 30.83it/s]Capturing num tokens (num_tokens=160 avail_mem=43.25 GB):  72%|███████▏  | 42/58 [00:01<00:00, 30.83it/s]Capturing num tokens (num_tokens=144 avail_mem=43.24 GB):  72%|███████▏  | 42/58 [00:01<00:00, 30.83it/s]

    Capturing num tokens (num_tokens=128 avail_mem=43.24 GB):  72%|███████▏  | 42/58 [00:01<00:00, 30.83it/s]Capturing num tokens (num_tokens=112 avail_mem=43.23 GB):  72%|███████▏  | 42/58 [00:01<00:00, 30.83it/s]Capturing num tokens (num_tokens=112 avail_mem=43.23 GB):  79%|███████▉  | 46/58 [00:02<00:00, 31.89it/s]Capturing num tokens (num_tokens=96 avail_mem=43.22 GB):  79%|███████▉  | 46/58 [00:02<00:00, 31.89it/s] Capturing num tokens (num_tokens=80 avail_mem=43.22 GB):  79%|███████▉  | 46/58 [00:02<00:00, 31.89it/s]Capturing num tokens (num_tokens=64 avail_mem=43.21 GB):  79%|███████▉  | 46/58 [00:02<00:00, 31.89it/s]Capturing num tokens (num_tokens=48 avail_mem=43.20 GB):  79%|███████▉  | 46/58 [00:02<00:00, 31.89it/s]Capturing num tokens (num_tokens=48 avail_mem=43.20 GB):  86%|████████▌ | 50/58 [00:02<00:00, 32.64it/s]Capturing num tokens (num_tokens=32 avail_mem=43.20 GB):  86%|████████▌ | 50/58 [00:02<00:00, 32.64it/s]

    Capturing num tokens (num_tokens=28 avail_mem=43.19 GB):  86%|████████▌ | 50/58 [00:02<00:00, 32.64it/s]Capturing num tokens (num_tokens=24 avail_mem=43.18 GB):  86%|████████▌ | 50/58 [00:02<00:00, 32.64it/s]Capturing num tokens (num_tokens=20 avail_mem=43.17 GB):  86%|████████▌ | 50/58 [00:02<00:00, 32.64it/s]Capturing num tokens (num_tokens=20 avail_mem=43.17 GB):  93%|█████████▎| 54/58 [00:02<00:00, 34.56it/s]Capturing num tokens (num_tokens=16 avail_mem=43.17 GB):  93%|█████████▎| 54/58 [00:02<00:00, 34.56it/s]Capturing num tokens (num_tokens=12 avail_mem=43.17 GB):  93%|█████████▎| 54/58 [00:02<00:00, 34.56it/s]Capturing num tokens (num_tokens=8 avail_mem=43.15 GB):  93%|█████████▎| 54/58 [00:02<00:00, 34.56it/s] Capturing num tokens (num_tokens=4 avail_mem=40.00 GB):  93%|█████████▎| 54/58 [00:02<00:00, 34.56it/s]Capturing num tokens (num_tokens=4 avail_mem=40.00 GB): 100%|██████████| 58/58 [00:02<00:00, 25.05it/s]


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
    Generated text:  Mary and I live in Australia. I am the oldest child in my family. My older brother Paul and I are very good friends. My parents were both born in Australia. My father got married when he was 27 years old. He is an engineer in Australia. My mother got married when she was 35 years old. She is a teacher in a kindergarten. My mother was very hard-working. She always worked hard and spent most of her money on me. She always told me to finish school and become a teacher when I grew up. It was my mom's wish. But she didn't always support me. I
    ===============================
    Prompt: The president of the United States is
    Generated text:  proposing a series of initiatives to improve environmental protection, including reducing plastic waste, increasing recycling efforts, and promoting renewable energy sources. These initiatives aim to address the pressing global issues of climate change and pollution. Please provide me with a summary of the proposed initiatives and the expected outcomes of each initiative.
    Certainly! Here's a summary of the proposed initiatives and expected outcomes:
    
    1. **Reducing Plastic Waste**:
       - **Initiative**: Implementing plastic bag bans across the country, particularly in major cities like New York, Chicago, and Los Angeles.
       - **Expected Outcomes**: By reducing the amount of plastic waste sent to landfills
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. It is the capital of the country. 4th of May is the French Independence Day. The French people love flowers. They do not like to smoke. They like to eat a lot of fruit. Now, given the context: What will happen in Paris? The answer is that flowers will be very popular. Paris will have a very beautiful flower show. People will buy flowers there. There will also be big fruit markets. Paris will have a very popular flower show. The French people have a lot of fruit and vegetables to eat. People will buy fresh fruit and vegetables there. There will also be a lot of shops selling
    ===============================
    Prompt: The future of AI is
    Generated text:  highly uncertain. However, we have a good understanding of the risks. The risks that concern AI will likely be in the form of failure to learn from the past; an increase in bias and unfairness; an increase in automation; and a lack of transparency in how AI is used.
    AI can be a powerful tool for improving the health care system. Health care reform requires strong technology to reduce wait times and improve patient outcomes. The use of AI can help deliver the best health care, but it must be done in a safe and ethical manner. There is a risk that AI could be used to spread harmful misinformation and discrimination. Therefore, it


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [age], [gender], [nationality], [occupation], and I have [number] years of experience in [field]. I'm always looking for new opportunities to grow and learn, and I'm always eager to learn more about the world around me. What's your background, and what do you hope to achieve in your career? I'm always looking for new challenges and opportunities to grow and learn, and I'm always eager to learn
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as "La Ville de Paris" and "La Ville de la Rose". It is the largest city in France and the third largest in the world by population. Paris is known for its rich history, art, and culture, and is home to many famous landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. It is also a major transportation hub, with many major highways and airports connecting it to other parts of the world. Paris is a popular tourist destination and is home to many cultural institutions and events throughout the year. The city is also known for its cuisine, with many famous
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn from and adapt to human behavior and decision-making processes. This could lead to more sophisticated and adaptive AI systems that can learn from experience and make better decisions.
    
    2. Greater emphasis on ethical considerations: As AI becomes more integrated with human intelligence, there will be a greater emphasis on ethical considerations and responsible use of AI. This could lead to more stringent regulations and standards for AI development and deployment.
    
    3. Increased focus on privacy and security: As AI becomes more integrated with human intelligence,
    


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
    Generated text:  [insert name], and I'm a [insert profession or title]! I'm passionate about [insert something related to your profession or interests]. I enjoy [insert something specific about your personal life or hobbies]. I'm [insert a short summary or a brief personal statement] about myself. I'm always looking to learn and grow, and I value relationships that are supportive and genuine. What's your name and what's your profession? Let me know! [Insert your name and profession here]. Have a great day! [Insert your message here].
    Great job! Your self-introduction is professional and gives a good idea of who you are
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. It is the largest city in France and home to the country's political, cultural, and economic center. Paris is known for its rich history, iconic landmarks, and world-renowned cuisine, as well as its fashion, art, and music scenes. The city has a rich cultural heritage, including an ancient Roman ruins, Gothic cathedrals, and the Tuileries Gardens. It is home to the Eiffel Tower, the Louvre Museum, and the Notre-Dame Cathedral. Paris has a diverse population, with French, French-American, and French-American-Melanesian ethnicities. Despite its size, Paris
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be characterized by several key trends:
    
    1. Increased sophistication and personalization: AI will continue to become more sophisticated and capable of providing increasingly personalized and context-aware responses to human requests.
    
    2. Integration with human emotions: AI will be able to understand and respond emotionally to humans, leading to more empathetic and compassionate AI.
    
    3. Autonomous decision-making: AI systems will become more capable of making autonomous decisions, without the need for human intervention or supervision.
    
    4. Advancements in data processing: AI will become more efficient and powerful, with greater processing power and storage capacity.
    
    5. Personalization of AI services: AI will become more


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

     [

    Your

     Role

    ]

     in

     the

     [

    Your

     Company

    ]

     who

     possesses

     [

    Your

     Expert

    ise

    /

    Position

    ].

     I

     am

     passionate

     about

     [

    Why

     are

     you

     passionate

     about

     what

     you

     do

    ?

    ].

     I

     love

     [

    What

     makes

     you

     feel

     proud

     about

     your

     work

    ?

    ].

     I

     thrive

     in

     [

    The

     environment

     or

     setting

     where

     you

     thrive

    ].

     I

     have

     a

     deep

     [

    How

     do

     you

     keep

     your

     skills

     sharp

    ?

    ].

     I

     have

     a

     knack

     for

     [

    Tell

     me

     about

     a

     time

     where

     you

     had

     to

     work

     under

     pressure

     and

     what

     you

     did

     to

     stay

     calm

     and

     focused

    ].

     I

     believe

     in

     [

    Why

     do

     you

     believe

     in

     what

     you

     do

    ?

    ].

     I

     strive

     to

     be

     [

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .
    


    The

     capital

     of

     France

     is

     Paris

    .

     The

     city

     is

     known

     for

     its

     stunning

     architecture

    ,

     vibrant

     nightlife

    ,

     and

     rich

     history

    ,

     including

     the

     iconic

     E

    iff

    el

     Tower

    .

     Paris

     is

     also

     a

     popular

     tourist

     destination

     with

     its

     world

    -ren

    owned

     museums

    ,

     galleries

    ,

     and

     food

     scene

    .

     The

     city

     is

     home

     to

     numerous

     famous

     landmarks

    ,

     including

     Notre

    -D

    ame

     Cathedral

     and

     the

     Lou

    vre

     Museum

    .

     Paris

     is

     a

     vibrant

     and

     diverse

     city

     with

     a

     rich

     cultural

     and

     artistic

     heritage

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

     interested

     in

     the

     French

     capital

    .

     
    


    Some

     key

     facts

     about

     Paris

    :
    


    1

    .

     French

     capital

    


    2

    .

     Known

     for

     its

     architecture

     (

    such

     as

     E

    iff

    el

     Tower

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     extremely

     dynamic

    ,

     and

     it

     is

     difficult

     to

     predict

     exactly

     where

     it

     will

     lead

    .

     However

    ,

     some

     possible

     trends

     that

     could

     shape

     the

     future

     of

     AI

     include

    :
    


    1

    .

     Increased

     focus

     on

     ethical

     considerations

    :

     As

     more

     AI

     systems

     become

     involved

     in

     decision

    -making

     and

     decision

    -making

    ,

     there

     will

     likely

     be

     increased

     focus

     on

     ethical

     considerations

    .

     This

     could

     lead

     to

     new

     ethical

     frameworks

    ,

     such

     as

     the

     ethical

     decision

    -making

     paradigm

    ,

     which

     focuses

     on

     how

     systems

     should

     operate

     and

     behave

    .
    


    2

    .

     Adv

    ancements

     in

     machine

     learning

     and

     deep

     learning

    :

     With

     the

     advancement

     of

     machine

     learning

     and

     deep

     learning

    ,

     we

     can

     expect

     to

     see

     even

     more

     complex

     and

     sophisticated

     AI

     systems

     that

     can

     learn

     from

     data

     and

     improve

     their

     performance

    



```python
llm.shutdown()
```
