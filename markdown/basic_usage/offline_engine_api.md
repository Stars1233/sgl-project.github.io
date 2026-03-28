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


    2026-03-28 04:28:34.900 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 04:28:34] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 04:28:34.900 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 04:28:34] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 04:28:34.900 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 04:28:34] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 04:28:34.900 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 04:28:34] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 04:28:34.901 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 04:28:34] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  2.77it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  2.76it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:07,  2.23s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:07,  2.23s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<00:55,  1.02it/s]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<00:55,  1.02it/s]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<00:55,  1.02it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:21,  2.47it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:21,  2.47it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:21,  2.47it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:02<00:12,  4.20it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:02<00:12,  4.20it/s]Compiling num tokens (num_tokens=4608):  10%|█         | 6/58 [00:02<00:12,  4.20it/s]Compiling num tokens (num_tokens=4096):  10%|█         | 6/58 [00:02<00:12,  4.20it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:02<00:06,  7.19it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:02<00:06,  7.19it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:02<00:06,  7.19it/s]Compiling num tokens (num_tokens=3328):  16%|█▌        | 9/58 [00:02<00:06,  7.19it/s]Compiling num tokens (num_tokens=3072):  16%|█▌        | 9/58 [00:02<00:06,  7.19it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:02<00:03, 11.44it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:02<00:03, 11.44it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:02<00:03, 11.44it/s]Compiling num tokens (num_tokens=2304):  22%|██▏       | 13/58 [00:02<00:03, 11.44it/s]

    Compiling num tokens (num_tokens=2048):  22%|██▏       | 13/58 [00:02<00:03, 11.44it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:02<00:02, 16.04it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:02<00:02, 16.04it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:02<00:02, 16.04it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:02<00:02, 16.04it/s]Compiling num tokens (num_tokens=1024):  29%|██▉       | 17/58 [00:03<00:02, 16.04it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:03<00:01, 19.65it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:03<00:01, 19.65it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:03<00:01, 19.65it/s]

    Compiling num tokens (num_tokens=832):  36%|███▌      | 21/58 [00:03<00:01, 19.65it/s]Compiling num tokens (num_tokens=768):  36%|███▌      | 21/58 [00:03<00:01, 19.65it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 23.51it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 23.51it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 23.51it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 23.51it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 23.51it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:03<00:01, 26.35it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:03<00:01, 26.35it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:03<00:01, 26.35it/s]

    Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:03<00:01, 26.35it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:03<00:01, 26.35it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:00, 29.29it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:00, 29.29it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:00, 29.29it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:00, 29.29it/s]

    Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:00, 29.29it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:03<00:00, 24.03it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:03<00:00, 24.03it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:03<00:00, 24.03it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:03<00:00, 24.03it/s]Compiling num tokens (num_tokens=192):  64%|██████▍   | 37/58 [00:03<00:00, 24.03it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:03<00:00, 26.94it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:03<00:00, 26.94it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:03<00:00, 26.94it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:03<00:00, 26.94it/s]

    Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:03<00:00, 26.94it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 28.93it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 28.93it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 28.93it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 28.93it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 28.93it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:03<00:00, 30.70it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:03<00:00, 30.70it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:03<00:00, 30.70it/s]

    Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:04<00:00, 30.70it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:04<00:00, 30.70it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:04<00:00, 31.20it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:04<00:00, 31.20it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:04<00:00, 31.20it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:04<00:00, 31.20it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:04<00:00, 31.20it/s] Compiling num tokens (num_tokens=4):  91%|█████████▏| 53/58 [00:04<00:00, 31.20it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 13.90it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=53.44 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=53.44 GB):   2%|▏         | 1/58 [00:00<00:07,  7.43it/s]Capturing num tokens (num_tokens=7680 avail_mem=53.41 GB):   2%|▏         | 1/58 [00:00<00:07,  7.43it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=53.41 GB):   3%|▎         | 2/58 [00:00<00:07,  7.82it/s]Capturing num tokens (num_tokens=7168 avail_mem=53.41 GB):   3%|▎         | 2/58 [00:00<00:07,  7.82it/s]Capturing num tokens (num_tokens=7168 avail_mem=53.41 GB):   5%|▌         | 3/58 [00:00<00:06,  8.13it/s]Capturing num tokens (num_tokens=6656 avail_mem=53.41 GB):   5%|▌         | 3/58 [00:00<00:06,  8.13it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=53.41 GB):   7%|▋         | 4/58 [00:00<00:06,  8.38it/s]Capturing num tokens (num_tokens=6144 avail_mem=53.41 GB):   7%|▋         | 4/58 [00:00<00:06,  8.38it/s]Capturing num tokens (num_tokens=6144 avail_mem=53.41 GB):   9%|▊         | 5/58 [00:00<00:06,  8.75it/s]Capturing num tokens (num_tokens=5632 avail_mem=53.40 GB):   9%|▊         | 5/58 [00:00<00:06,  8.75it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=53.40 GB):   9%|▊         | 5/58 [00:00<00:06,  8.75it/s]Capturing num tokens (num_tokens=5120 avail_mem=53.40 GB):  12%|█▏        | 7/58 [00:00<00:05,  9.24it/s]Capturing num tokens (num_tokens=4608 avail_mem=53.40 GB):  12%|█▏        | 7/58 [00:00<00:05,  9.24it/s]Capturing num tokens (num_tokens=4096 avail_mem=53.40 GB):  12%|█▏        | 7/58 [00:00<00:05,  9.24it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=53.40 GB):  16%|█▌        | 9/58 [00:00<00:04,  9.95it/s]Capturing num tokens (num_tokens=3840 avail_mem=53.40 GB):  16%|█▌        | 9/58 [00:00<00:04,  9.95it/s]Capturing num tokens (num_tokens=3584 avail_mem=53.39 GB):  16%|█▌        | 9/58 [00:01<00:04,  9.95it/s]Capturing num tokens (num_tokens=3584 avail_mem=53.39 GB):  19%|█▉        | 11/58 [00:01<00:04, 10.57it/s]Capturing num tokens (num_tokens=3328 avail_mem=53.39 GB):  19%|█▉        | 11/58 [00:01<00:04, 10.57it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=53.39 GB):  19%|█▉        | 11/58 [00:01<00:04, 10.57it/s]Capturing num tokens (num_tokens=3072 avail_mem=53.39 GB):  22%|██▏       | 13/58 [00:01<00:04, 10.92it/s]Capturing num tokens (num_tokens=2816 avail_mem=53.39 GB):  22%|██▏       | 13/58 [00:01<00:04, 10.92it/s]Capturing num tokens (num_tokens=2560 avail_mem=53.38 GB):  22%|██▏       | 13/58 [00:01<00:04, 10.92it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=53.38 GB):  26%|██▌       | 15/58 [00:01<00:03, 11.86it/s]Capturing num tokens (num_tokens=2304 avail_mem=53.38 GB):  26%|██▌       | 15/58 [00:01<00:03, 11.86it/s]Capturing num tokens (num_tokens=2048 avail_mem=53.37 GB):  26%|██▌       | 15/58 [00:01<00:03, 11.86it/s]Capturing num tokens (num_tokens=2048 avail_mem=53.37 GB):  29%|██▉       | 17/58 [00:01<00:03, 11.96it/s]Capturing num tokens (num_tokens=1792 avail_mem=53.37 GB):  29%|██▉       | 17/58 [00:01<00:03, 11.96it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=53.37 GB):  29%|██▉       | 17/58 [00:01<00:03, 11.96it/s]Capturing num tokens (num_tokens=1536 avail_mem=53.37 GB):  33%|███▎      | 19/58 [00:01<00:03, 11.97it/s]Capturing num tokens (num_tokens=1280 avail_mem=53.36 GB):  33%|███▎      | 19/58 [00:01<00:03, 11.97it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=53.34 GB):  33%|███▎      | 19/58 [00:01<00:03, 11.97it/s]Capturing num tokens (num_tokens=1024 avail_mem=53.34 GB):  36%|███▌      | 21/58 [00:02<00:03, 10.46it/s]Capturing num tokens (num_tokens=960 avail_mem=53.36 GB):  36%|███▌      | 21/58 [00:02<00:03, 10.46it/s] Capturing num tokens (num_tokens=896 avail_mem=53.35 GB):  36%|███▌      | 21/58 [00:02<00:03, 10.46it/s]

    Capturing num tokens (num_tokens=896 avail_mem=53.35 GB):  40%|███▉      | 23/58 [00:02<00:03, 11.12it/s]Capturing num tokens (num_tokens=832 avail_mem=53.35 GB):  40%|███▉      | 23/58 [00:02<00:03, 11.12it/s]Capturing num tokens (num_tokens=768 avail_mem=53.35 GB):  40%|███▉      | 23/58 [00:02<00:03, 11.12it/s]Capturing num tokens (num_tokens=768 avail_mem=53.35 GB):  43%|████▎     | 25/58 [00:02<00:02, 11.70it/s]Capturing num tokens (num_tokens=704 avail_mem=53.34 GB):  43%|████▎     | 25/58 [00:02<00:02, 11.70it/s]

    Capturing num tokens (num_tokens=640 avail_mem=53.34 GB):  43%|████▎     | 25/58 [00:02<00:02, 11.70it/s]Capturing num tokens (num_tokens=640 avail_mem=53.34 GB):  47%|████▋     | 27/58 [00:02<00:02, 12.14it/s]Capturing num tokens (num_tokens=576 avail_mem=53.34 GB):  47%|████▋     | 27/58 [00:02<00:02, 12.14it/s]Capturing num tokens (num_tokens=512 avail_mem=53.33 GB):  47%|████▋     | 27/58 [00:02<00:02, 12.14it/s]

    Capturing num tokens (num_tokens=512 avail_mem=53.33 GB):  50%|█████     | 29/58 [00:02<00:02, 12.50it/s]Capturing num tokens (num_tokens=480 avail_mem=53.34 GB):  50%|█████     | 29/58 [00:02<00:02, 12.50it/s]Capturing num tokens (num_tokens=448 avail_mem=53.34 GB):  50%|█████     | 29/58 [00:02<00:02, 12.50it/s]Capturing num tokens (num_tokens=448 avail_mem=53.34 GB):  53%|█████▎    | 31/58 [00:02<00:02, 12.15it/s]Capturing num tokens (num_tokens=416 avail_mem=52.81 GB):  53%|█████▎    | 31/58 [00:02<00:02, 12.15it/s]

    Capturing num tokens (num_tokens=384 avail_mem=52.80 GB):  53%|█████▎    | 31/58 [00:02<00:02, 12.15it/s]Capturing num tokens (num_tokens=384 avail_mem=52.80 GB):  57%|█████▋    | 33/58 [00:03<00:02, 10.37it/s]Capturing num tokens (num_tokens=352 avail_mem=52.80 GB):  57%|█████▋    | 33/58 [00:03<00:02, 10.37it/s]

    Capturing num tokens (num_tokens=320 avail_mem=52.79 GB):  57%|█████▋    | 33/58 [00:03<00:02, 10.37it/s]Capturing num tokens (num_tokens=320 avail_mem=52.79 GB):  60%|██████    | 35/58 [00:03<00:02,  9.64it/s]Capturing num tokens (num_tokens=288 avail_mem=52.79 GB):  60%|██████    | 35/58 [00:03<00:02,  9.64it/s]

    Capturing num tokens (num_tokens=256 avail_mem=52.79 GB):  60%|██████    | 35/58 [00:03<00:02,  9.64it/s]Capturing num tokens (num_tokens=256 avail_mem=52.79 GB):  64%|██████▍   | 37/58 [00:03<00:02,  9.09it/s]Capturing num tokens (num_tokens=240 avail_mem=52.79 GB):  64%|██████▍   | 37/58 [00:03<00:02,  9.09it/s]

    Capturing num tokens (num_tokens=240 avail_mem=52.79 GB):  66%|██████▌   | 38/58 [00:03<00:02,  8.96it/s]Capturing num tokens (num_tokens=224 avail_mem=52.78 GB):  66%|██████▌   | 38/58 [00:03<00:02,  8.96it/s]Capturing num tokens (num_tokens=224 avail_mem=52.78 GB):  67%|██████▋   | 39/58 [00:03<00:02,  8.91it/s]Capturing num tokens (num_tokens=208 avail_mem=52.78 GB):  67%|██████▋   | 39/58 [00:03<00:02,  8.91it/s]

    Capturing num tokens (num_tokens=208 avail_mem=52.78 GB):  69%|██████▉   | 40/58 [00:03<00:02,  8.95it/s]Capturing num tokens (num_tokens=192 avail_mem=52.78 GB):  69%|██████▉   | 40/58 [00:03<00:02,  8.95it/s]Capturing num tokens (num_tokens=192 avail_mem=52.78 GB):  71%|███████   | 41/58 [00:04<00:01,  8.92it/s]Capturing num tokens (num_tokens=176 avail_mem=52.77 GB):  71%|███████   | 41/58 [00:04<00:01,  8.92it/s]

    Capturing num tokens (num_tokens=176 avail_mem=52.77 GB):  72%|███████▏  | 42/58 [00:04<00:01,  8.93it/s]Capturing num tokens (num_tokens=160 avail_mem=52.77 GB):  72%|███████▏  | 42/58 [00:04<00:01,  8.93it/s]Capturing num tokens (num_tokens=160 avail_mem=52.77 GB):  74%|███████▍  | 43/58 [00:04<00:01,  9.07it/s]Capturing num tokens (num_tokens=144 avail_mem=52.77 GB):  74%|███████▍  | 43/58 [00:04<00:01,  9.07it/s]Capturing num tokens (num_tokens=128 avail_mem=52.76 GB):  74%|███████▍  | 43/58 [00:04<00:01,  9.07it/s]

    Capturing num tokens (num_tokens=128 avail_mem=52.76 GB):  78%|███████▊  | 45/58 [00:04<00:01,  9.99it/s]Capturing num tokens (num_tokens=112 avail_mem=52.76 GB):  78%|███████▊  | 45/58 [00:04<00:01,  9.99it/s]Capturing num tokens (num_tokens=96 avail_mem=52.76 GB):  78%|███████▊  | 45/58 [00:04<00:01,  9.99it/s] Capturing num tokens (num_tokens=96 avail_mem=52.76 GB):  81%|████████  | 47/58 [00:04<00:01, 10.58it/s]Capturing num tokens (num_tokens=80 avail_mem=52.75 GB):  81%|████████  | 47/58 [00:04<00:01, 10.58it/s]

    Capturing num tokens (num_tokens=64 avail_mem=52.75 GB):  81%|████████  | 47/58 [00:04<00:01, 10.58it/s]Capturing num tokens (num_tokens=64 avail_mem=52.75 GB):  84%|████████▍ | 49/58 [00:04<00:00, 11.08it/s]Capturing num tokens (num_tokens=48 avail_mem=52.75 GB):  84%|████████▍ | 49/58 [00:04<00:00, 11.08it/s]Capturing num tokens (num_tokens=32 avail_mem=52.74 GB):  84%|████████▍ | 49/58 [00:04<00:00, 11.08it/s]

    Capturing num tokens (num_tokens=32 avail_mem=52.74 GB):  88%|████████▊ | 51/58 [00:04<00:00, 11.46it/s]Capturing num tokens (num_tokens=28 avail_mem=52.73 GB):  88%|████████▊ | 51/58 [00:04<00:00, 11.46it/s]Capturing num tokens (num_tokens=24 avail_mem=52.73 GB):  88%|████████▊ | 51/58 [00:04<00:00, 11.46it/s]Capturing num tokens (num_tokens=24 avail_mem=52.73 GB):  91%|█████████▏| 53/58 [00:05<00:00, 11.83it/s]Capturing num tokens (num_tokens=20 avail_mem=52.73 GB):  91%|█████████▏| 53/58 [00:05<00:00, 11.83it/s]

    Capturing num tokens (num_tokens=16 avail_mem=52.73 GB):  91%|█████████▏| 53/58 [00:05<00:00, 11.83it/s]Capturing num tokens (num_tokens=16 avail_mem=52.73 GB):  95%|█████████▍| 55/58 [00:05<00:00, 12.13it/s]Capturing num tokens (num_tokens=12 avail_mem=52.72 GB):  95%|█████████▍| 55/58 [00:05<00:00, 12.13it/s]Capturing num tokens (num_tokens=8 avail_mem=52.72 GB):  95%|█████████▍| 55/58 [00:05<00:00, 12.13it/s] 

    Capturing num tokens (num_tokens=8 avail_mem=52.72 GB):  98%|█████████▊| 57/58 [00:05<00:00, 12.03it/s]Capturing num tokens (num_tokens=4 avail_mem=52.72 GB):  98%|█████████▊| 57/58 [00:05<00:00, 12.03it/s]Capturing num tokens (num_tokens=4 avail_mem=52.72 GB): 100%|██████████| 58/58 [00:05<00:00, 10.62it/s]


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
    Generated text:  Kim. I am a student at Fudan University in Shanghai. I'm a junior at this time. I'm 17 years old. I'm in the second year of the Senior School. I'm not very good at math. I like sports very much. I enjoy playing soccer. I have a soccer ball and a basketball, and my favorite team is the Beijing Youth Soccer Team. I'm 15 years old. I'm in the first year of the Senior School. I'm a junior. I'm 15 years old. I'm a student at Fudan University in Shanghai. I'm 1
    ===============================
    Prompt: The president of the United States is
    Generated text:  32 years older than 2 times the age of Joe. If Joe is currently 25 years old, calculate the president's age 30 years from now.
    To solve this problem, we'll first establish the relationship between the president's age and Joe's age, then calculate the president's age 30 years from now, considering the given conditions.
    
    1. Let's denote Joe's current age by \(J\), which is 25 years old.
    2. The president's age is 32 years more than 2 times Joe's age, so the president's age can be expressed as \(P
    ===============================
    Prompt: The capital of France is
    Generated text:  ______.
    A. Paris
    B. London
    C. Moscow
    D. Rome
    Answer:
    
    A
    
    A certain organization has 50 employees, with 10% of the employees being part-time. Among the full-time employees, 50% have a certain skill. What percentage of the employees with a certain skill are part-time?
    A. 20%
    B. 30%
    C. 40%
    D. 50%
    Answer:
    
    B
    
    Which of the following options does NOT represent the meaning of "step-by-step learning"?
    A. Learning by following a structured plan
    B.
    ===============================
    Prompt: The future of AI is
    Generated text:  very bright. So we can expect to see more and more features of AI in the next decade. However, the future of AI is not fully understood yet. As a result, there are some technologies that we can control and that are less exposed to human control. They are natural language processing, machine learning and computer vision. These technologies, in addition to helping us in many aspects of our life, could help us predict the future. 
    
    Natural Language Processing (NLP) is the ability to understand, analyze and generate human language. The most important part of NLP is to understand language, which requires many skills like recognizing grammar, recognizing


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you and learn more about you. What
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic Eiffel Tower and the annual Eiffel Tower Festival. It is also home to the Louvre Museum, the Notre-Dame Cathedral, and the Palace of Versailles. Paris is a bustling metropolis with a rich history and culture, and is a popular tourist destination. It is also known for its fashion industry, with Paris Fashion Week being one of the world's largest and most influential fashion events. The city is also home to the French Parliament and the French Academy of Sciences. Paris is a city of contrasts, with its modern architecture and historical landmarks blending seamlessly into one another. It is
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn and adapt to human behavior and preferences. This could lead to more personalized and efficient AI systems.
    
    2. Enhanced ethical considerations: As AI becomes more integrated with human intelligence, there will be increased scrutiny of its ethical implications. This could lead to more stringent regulations and guidelines for AI development and deployment.
    
    3. Greater reliance on AI for decision-making: AI is likely to become more integrated with human decision-making, allowing machines to make more informed and accurate decisions. This could lead to more
    


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
    Generated text:  [Name], and I'm a friendly and helpful personality. What can you tell me about yourself? As a friendly and helpful personality, I'm always ready to assist others with any questions or concerns they may have. I'm always available to help, whether it's by phone, email, or in person. What are some of the most exciting things that you've done or are currently doing? As a friendly and helpful personality, I've always been curious and eager to learn new things, so I've taken on many exciting projects and tasks that have helped me grow as a person and in my field. I'm always on the lookout for
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    
    What is the capital of France? Paris. Paris is the capital of France and the largest city in the country. It is located on the northeastern coast of the Mediterranean Sea and is the seat of government for the French Republic. The city is known for its historical landmarks, such as the Eiffel Tower, the Louvre Museum, Notre-Dame Cathedral, and the Musée du Louvre. It is also famous for its fashion industry and has a rich culture and arts scene, including the Notre-Dame Cathedral, the Louvre Museum, and the Musée du Louvre. Paris is the largest city in France by population
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  incredibly promising and it is likely to continue to evolve and advance in many different areas. Here are some possible trends in AI that we can expect to see:
    
    1. Increased relevance in healthcare: AI is already being used in healthcare to help doctors and researchers analyze vast amounts of patient data to develop new treatments and diagnose diseases. As AI gets more sophisticated, we can expect it to become even more relevant in healthcare in the future.
    
    2. More autonomous vehicles: With advancements in AI, we are likely to see more autonomous vehicles become more prevalent in the future. This would mean that cars and trucks wouldn't need human drivers, and would be more


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

    'm

     a

     human

     with

     a

     deep

     love

     for

     exploring

     the

     world

     and

     discovering

     new

     cultures

    .

     I

    'm

     always

     up

     for

     an

     adventure

    ,

     whether

     it

    's

     trying

     out

     new

     cuis

    ines

    ,

     traveling

     the

     world

    ,

     or

     simply

     chatting

     with

     people

     from

     all

     over

     the

     place

    .

     I

     enjoy

     learning

     new

     things

     and

     never

     get

     bored

     of

     the

     same

     stuff

    ,

     so

     I

     always

     have

     something

     interesting

     to

     say

    .

     I

    'm

     a

     curious

     and

     adventurous

     soul

     who

     loves

     to

     push

     boundaries

     and

     make

     new

     friends

    .

     I

    'm

     always

     looking

     for

     new

     experiences

     and

     new

     ways

     to

     grow

     as

     a

     person

    .

     How

     can

     I

     meet

     someone

     like

     [

    insert

     someone

    's

     name

    ]

    ?
    


    I

    'm

     a

     friend

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     also

     known

     as

     the

     "

    City

     of

     Love

    "

     as

     it

     is

     home

     to

     many

     famous

     landmarks

     such

     as

     Notre

    -D

    ame

     Cathedral

    ,

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

     the

     Arc

     de

     Tri

    omp

    he

    .

     
    


    This

     statement

     encaps

    ulates

     the

     main

     characteristics

     of

     Paris

    ,

     including

     its

     historical

     significance

    ,

     cultural

     attractions

    ,

     and

     iconic

     landmarks

    ,

     while

     also

     being

     brief

     and

     direct

    .

     It

     sets

     the

     city

     in

     a

     romantic

     and

     historical

     context

     for

     visitors

     and

     locals

     alike

    .

     
    


    The

     capital

     of

     France

    ,

     Paris

     is

     also

     known

     for

     its

     romantic

     and

     historical

     atmosphere

    .

     The

     city

     is

     famous

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

     Notre

    -D

    ame

     Cathedral

    ,

     Lou

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     characterized

     by

     rapid

     development

     and

     progress

    ,

     with

     new

     technologies

     and

     techniques

     emerging

     in

     areas

     such

     as

     natural

     language

     processing

    ,

     machine

     learning

    ,

     computer

     vision

    ,

     robotics

    ,

     and

     autonomous

     systems

    .

     Some

     potential

     trends

     in

     AI

     include

    :
    


    1

    .

     Increased

     integration

     of

     AI

     into

     everyday

     life

    :

     AI

     is

     becoming

     more

     integrated

     into

     our

     daily

     lives

    ,

     from

     self

    -driving

     cars

     to

     smart

     home

     devices

    .

     As

     AI

     becomes

     more

     widespread

    ,

     it

     is

     expected

     to

     be

     integrated

     into

     our

     physical

     and

     digital

     environments

    .
    


    2

    .

     AI

    -driven

     automation

    :

     AI

     is

     expected

     to

     play

     a

     key

     role

     in

     autom

    ating

     many

     tasks

     that

     would

     otherwise

     require

     human

     labor

    .

     This

     could

     lead

     to

     new

     job

     roles

     and

     a

     shift

     towards

    



```python
llm.shutdown()
```
