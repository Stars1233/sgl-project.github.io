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


    2026-03-30 20:30:24.070 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 20:30:24] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 20:30:24.071 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 20:30:24] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 20:30:24.071 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 20:30:24] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 20:30:24.071 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 20:30:24] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 20:30:24.071 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 20:30:24] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.31it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.30it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:14,  2.35s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:14,  2.35s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<00:57,  1.03s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<00:57,  1.03s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<00:57,  1.03s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:22,  2.38it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:22,  2.38it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:22,  2.38it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:22,  2.38it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:02<00:10,  4.86it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:02<00:10,  4.86it/s]Compiling num tokens (num_tokens=4096):  12%|█▏        | 7/58 [00:02<00:10,  4.86it/s]

    Compiling num tokens (num_tokens=3840):  12%|█▏        | 7/58 [00:02<00:10,  4.86it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:06,  7.76it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:06,  7.76it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:06,  7.76it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:06,  7.76it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:06,  7.76it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:02<00:03, 12.19it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:02<00:03, 12.19it/s]Compiling num tokens (num_tokens=2304):  24%|██▍       | 14/58 [00:02<00:03, 12.19it/s]Compiling num tokens (num_tokens=2048):  24%|██▍       | 14/58 [00:02<00:03, 12.19it/s]

    Compiling num tokens (num_tokens=1792):  24%|██▍       | 14/58 [00:02<00:03, 12.19it/s]Compiling num tokens (num_tokens=1536):  24%|██▍       | 14/58 [00:03<00:03, 12.19it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:02, 17.99it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:02, 17.99it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:02, 17.99it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:02, 17.99it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:02, 17.99it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:02, 17.99it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:02, 17.99it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 25.87it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 25.87it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 25.87it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 25.87it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 25.87it/s]

    Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 25.87it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 25.87it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:03<00:01, 25.87it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:00, 34.69it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:00, 34.69it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:00, 34.69it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:00, 34.69it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:00, 34.69it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:00, 34.69it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:00, 34.69it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 39.45it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 39.45it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 39.45it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 39.45it/s]

    Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 39.45it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 39.45it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 39.45it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 44.31it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 44.31it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 44.31it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 44.31it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 44.31it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 44.31it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 44.31it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 43.96it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 43.96it/s]

    Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 43.96it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 43.96it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 43.96it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 43.96it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 43.96it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:03<00:00, 47.88it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:03<00:00, 47.88it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:03<00:00, 47.88it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.56it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.79 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.79 GB):   2%|▏         | 1/58 [00:00<00:06,  9.00it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.76 GB):   2%|▏         | 1/58 [00:00<00:06,  9.00it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.76 GB):   2%|▏         | 1/58 [00:00<00:06,  9.00it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=118.76 GB):   5%|▌         | 3/58 [00:00<00:04, 12.91it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.75 GB):   5%|▌         | 3/58 [00:00<00:04, 12.91it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.76 GB):   5%|▌         | 3/58 [00:00<00:04, 12.91it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.75 GB):   5%|▌         | 3/58 [00:00<00:04, 12.91it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.75 GB):  10%|█         | 6/58 [00:00<00:02, 18.66it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.75 GB):  10%|█         | 6/58 [00:00<00:02, 18.66it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.75 GB):  10%|█         | 6/58 [00:00<00:02, 18.66it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.75 GB):  10%|█         | 6/58 [00:00<00:02, 18.66it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=118.58 GB):  10%|█         | 6/58 [00:00<00:02, 18.66it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.58 GB):  17%|█▋        | 10/58 [00:00<00:01, 24.63it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.58 GB):  17%|█▋        | 10/58 [00:00<00:01, 24.63it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.57 GB):  17%|█▋        | 10/58 [00:00<00:01, 24.63it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.57 GB):  17%|█▋        | 10/58 [00:00<00:01, 24.63it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.57 GB):  17%|█▋        | 10/58 [00:00<00:01, 24.63it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.45 GB):  17%|█▋        | 10/58 [00:00<00:01, 24.63it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.45 GB):  26%|██▌       | 15/58 [00:00<00:01, 29.69it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.09 GB):  26%|██▌       | 15/58 [00:00<00:01, 29.69it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.09 GB):  26%|██▌       | 15/58 [00:00<00:01, 29.69it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=118.09 GB):  26%|██▌       | 15/58 [00:00<00:01, 29.69it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.09 GB):  31%|███       | 18/58 [00:00<00:01, 28.40it/s]Capturing num tokens (num_tokens=1536 avail_mem=117.90 GB):  31%|███       | 18/58 [00:00<00:01, 28.40it/s]Capturing num tokens (num_tokens=1280 avail_mem=117.90 GB):  31%|███       | 18/58 [00:00<00:01, 28.40it/s]Capturing num tokens (num_tokens=1024 avail_mem=117.88 GB):  31%|███       | 18/58 [00:00<00:01, 28.40it/s]Capturing num tokens (num_tokens=1024 avail_mem=117.88 GB):  36%|███▌      | 21/58 [00:00<00:01, 28.00it/s]Capturing num tokens (num_tokens=960 avail_mem=117.89 GB):  36%|███▌      | 21/58 [00:00<00:01, 28.00it/s] Capturing num tokens (num_tokens=896 avail_mem=117.89 GB):  36%|███▌      | 21/58 [00:00<00:01, 28.00it/s]

    Capturing num tokens (num_tokens=832 avail_mem=117.89 GB):  36%|███▌      | 21/58 [00:00<00:01, 28.00it/s]Capturing num tokens (num_tokens=768 avail_mem=117.88 GB):  36%|███▌      | 21/58 [00:00<00:01, 28.00it/s]Capturing num tokens (num_tokens=768 avail_mem=117.88 GB):  43%|████▎     | 25/58 [00:00<00:01, 28.28it/s]Capturing num tokens (num_tokens=704 avail_mem=117.88 GB):  43%|████▎     | 25/58 [00:00<00:01, 28.28it/s]Capturing num tokens (num_tokens=640 avail_mem=117.88 GB):  43%|████▎     | 25/58 [00:01<00:01, 28.28it/s]Capturing num tokens (num_tokens=576 avail_mem=117.88 GB):  43%|████▎     | 25/58 [00:01<00:01, 28.28it/s]Capturing num tokens (num_tokens=512 avail_mem=117.86 GB):  43%|████▎     | 25/58 [00:01<00:01, 28.28it/s]

    Capturing num tokens (num_tokens=512 avail_mem=117.86 GB):  50%|█████     | 29/58 [00:01<00:01, 28.28it/s]Capturing num tokens (num_tokens=480 avail_mem=117.88 GB):  50%|█████     | 29/58 [00:01<00:01, 28.28it/s]Capturing num tokens (num_tokens=448 avail_mem=117.88 GB):  50%|█████     | 29/58 [00:01<00:01, 28.28it/s]

    Capturing num tokens (num_tokens=416 avail_mem=117.88 GB):  50%|█████     | 29/58 [00:01<00:01, 28.28it/s]Capturing num tokens (num_tokens=416 avail_mem=117.88 GB):  55%|█████▌    | 32/58 [00:01<00:01, 21.04it/s]Capturing num tokens (num_tokens=384 avail_mem=117.87 GB):  55%|█████▌    | 32/58 [00:01<00:01, 21.04it/s]Capturing num tokens (num_tokens=352 avail_mem=117.87 GB):  55%|█████▌    | 32/58 [00:01<00:01, 21.04it/s]Capturing num tokens (num_tokens=320 avail_mem=117.86 GB):  55%|█████▌    | 32/58 [00:01<00:01, 21.04it/s]Capturing num tokens (num_tokens=288 avail_mem=117.86 GB):  55%|█████▌    | 32/58 [00:01<00:01, 21.04it/s]Capturing num tokens (num_tokens=288 avail_mem=117.86 GB):  62%|██████▏   | 36/58 [00:01<00:00, 24.48it/s]Capturing num tokens (num_tokens=256 avail_mem=117.86 GB):  62%|██████▏   | 36/58 [00:01<00:00, 24.48it/s]

    Capturing num tokens (num_tokens=240 avail_mem=118.75 GB):  62%|██████▏   | 36/58 [00:01<00:00, 24.48it/s]Capturing num tokens (num_tokens=224 avail_mem=118.74 GB):  62%|██████▏   | 36/58 [00:01<00:00, 24.48it/s]Capturing num tokens (num_tokens=224 avail_mem=118.74 GB):  67%|██████▋   | 39/58 [00:01<00:01, 16.50it/s]Capturing num tokens (num_tokens=208 avail_mem=137.29 GB):  67%|██████▋   | 39/58 [00:01<00:01, 16.50it/s]

    Capturing num tokens (num_tokens=192 avail_mem=137.29 GB):  67%|██████▋   | 39/58 [00:01<00:01, 16.50it/s]Capturing num tokens (num_tokens=176 avail_mem=137.29 GB):  67%|██████▋   | 39/58 [00:01<00:01, 16.50it/s]Capturing num tokens (num_tokens=160 avail_mem=137.28 GB):  67%|██████▋   | 39/58 [00:01<00:01, 16.50it/s]Capturing num tokens (num_tokens=144 avail_mem=137.28 GB):  67%|██████▋   | 39/58 [00:01<00:01, 16.50it/s]Capturing num tokens (num_tokens=144 avail_mem=137.28 GB):  76%|███████▌  | 44/58 [00:01<00:00, 21.71it/s]Capturing num tokens (num_tokens=128 avail_mem=137.28 GB):  76%|███████▌  | 44/58 [00:01<00:00, 21.71it/s]Capturing num tokens (num_tokens=112 avail_mem=137.27 GB):  76%|███████▌  | 44/58 [00:01<00:00, 21.71it/s]Capturing num tokens (num_tokens=96 avail_mem=137.27 GB):  76%|███████▌  | 44/58 [00:01<00:00, 21.71it/s] Capturing num tokens (num_tokens=80 avail_mem=137.27 GB):  76%|███████▌  | 44/58 [00:01<00:00, 21.71it/s]Capturing num tokens (num_tokens=64 avail_mem=137.26 GB):  76%|███████▌  | 44/58 [00:02<00:00, 21.71it/s]

    Capturing num tokens (num_tokens=64 avail_mem=137.26 GB):  84%|████████▍ | 49/58 [00:02<00:00, 26.55it/s]Capturing num tokens (num_tokens=48 avail_mem=137.26 GB):  84%|████████▍ | 49/58 [00:02<00:00, 26.55it/s]Capturing num tokens (num_tokens=32 avail_mem=137.25 GB):  84%|████████▍ | 49/58 [00:02<00:00, 26.55it/s]Capturing num tokens (num_tokens=28 avail_mem=137.25 GB):  84%|████████▍ | 49/58 [00:02<00:00, 26.55it/s]Capturing num tokens (num_tokens=24 avail_mem=137.25 GB):  84%|████████▍ | 49/58 [00:02<00:00, 26.55it/s]Capturing num tokens (num_tokens=20 avail_mem=137.24 GB):  84%|████████▍ | 49/58 [00:02<00:00, 26.55it/s]Capturing num tokens (num_tokens=20 avail_mem=137.24 GB):  93%|█████████▎| 54/58 [00:02<00:00, 30.89it/s]Capturing num tokens (num_tokens=16 avail_mem=137.24 GB):  93%|█████████▎| 54/58 [00:02<00:00, 30.89it/s]Capturing num tokens (num_tokens=12 avail_mem=137.24 GB):  93%|█████████▎| 54/58 [00:02<00:00, 30.89it/s]Capturing num tokens (num_tokens=8 avail_mem=137.24 GB):  93%|█████████▎| 54/58 [00:02<00:00, 30.89it/s] Capturing num tokens (num_tokens=4 avail_mem=137.23 GB):  93%|█████████▎| 54/58 [00:02<00:00, 30.89it/s]Capturing num tokens (num_tokens=4 avail_mem=137.23 GB): 100%|██████████| 58/58 [00:02<00:00, 25.90it/s]


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
    Generated text:  Celeste Grey. I live in Dublin with my family and I get up at 6:30 a.m. on weekdays. I get up at 5:30 a.m. on weekends. I have one subject to choose from on weekdays. I have three subjects on weekends. I usually go to bed around 9:00 p.m. Every day I exercise for 30 minutes at least before bed and I go to bed at 10:00 p.m. The next day I wake up at 6:00 a.m. I go to the gym. I do cardio exercises, stretch
    ===============================
    Prompt: The president of the United States is
    Generated text:  a person. A. 正确 B. 错误
    
    正确
    
    The US presidential election has taken place for many years. A. 正确 B. 错误
    
    正确
    
    For the US presidential election to succeed, it must be won by a majority of voters. A. 正确 B. 错误
    
    错误
    
    The United States Constitution outlines its political structure. A. 正确 B. 错误
    
    正确
    
    The people in the US do not have a direct say in what happens in the US government. A. 正确 B. 错误
    
    正确
    
    Which of the
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. The capital of India is New Delhi. In each of the two capitals, there is a city called the "Gates of Paris" and a city called the "Gate of Delhi". In the city called "Gate of Paris", a restaurant called "The Golden Gate", "Castle of Paris", and a church called "Church of the Gate of Paris" are located. In the city called "Gate of Delhi", a restaurant called "The Golden Gate", "Castle of Delhi", and a church called "Church of the Gate of Delhi" are located. In both cities, there are no other restaurants, churches or castles.
    ===============================
    Prompt: The future of AI is
    Generated text:  in the hands of young people
    
    # In the future of AI is in the hands of young people
    
    by
    
    In the future of AI is in the hands of young people
    
    As AI evolves and becomes more complex, the future of AI is in the hands of young people. According to a recent report from the Center for Technology and Innovation, this is likely due to two factors. First, young people are more open to learning and trying new things. Second, they are more comfortable with the idea of artificial intelligence.
    
    The Center for Technology and Innovation report suggests that young people have a higher acceptance of AI. A study conducted by the University


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


    Generated text:  [Name] and I'm a [job title] at [company name]. I'm a [insert your profession or role] who has been [insert your professional experience or achievements] and have a passion for [insert something you enjoy or are passionate about]. I'm always looking for new challenges and opportunities to grow and learn, and I'm excited to be a part of [insert your company or team]. Thank you for taking the time to meet me! [Name] [Company Name] [Phone Number] [Email Address] [LinkedIn Profile] [About Me] [Professional Summary] [Skills] [Education] [Certifications
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as the City of Light. It is a historic city with a rich history and is home to many famous landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. Paris is also known for its vibrant culture, art, and cuisine, and is a popular tourist destination. It is the largest city in France and is home to many international organizations and institutions. The city is also known for its annual Eiffel Tower Festival, which attracts millions of visitors each year. Paris is a city of contrasts, with its modern architecture and historical landmarks blending seamlessly together. It is a city that
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the way we live, work, and interact with technology. Here are some of the most likely trends in AI that are expected to shape the future:
    
    1. Increased automation and robotics: As AI technology continues to advance, we are likely to see an increase in automation and robotics in various industries. This could lead to the creation of new jobs, but it could also lead to the creation of new jobs in areas such as software development, data analysis, and robotics.
    
    2. AI-powered healthcare: AI is already being used in healthcare to improve patient outcomes and reduce costs.
    


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
    Generated text:  [name] and I'm a [insert occupation or hobby]. I have a genuine interest in [describe one interest, achievement, or hobby that you feel strongly about] and am constantly striving to improve my skills and knowledge in this field. I'm always open to new experiences and adventures, so please feel free to share any interesting experiences or adventures you may have. 
    [Insert a brief introduction of who you are and your interests or hobbies, such as "Hello, my name is [name] and I'm a [insert occupation or hobby]. I have a genuine interest in [describe one interest, achievement, or hobby that you feel
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    
    The capital of France is Paris. - Answer by:
    
    Paris is the capital of France. The capital city of France is Paris. 
    
    Explanation for an 8-year-old:
    
    Paris is like a big city with a big name. It's the city where all the important people work in France. It's also the city where the French people live and work. Paris is very nice and has lots of pretty buildings and colorful buildings. It's the most important city in France! It's like the home for all the important French people. So Paris is the capital of France. Isn't that cool? Paris is home to so many
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  uncertain and unpredictable. However, several possible trends that could potentially shape the field include:
    
    1. More autonomous machines: As AI continues to advance, we are likely to see more autonomous machines that can make decisions and take actions on their own without human intervention. This could lead to significant improvements in efficiency and accuracy, but it also raises ethical concerns about job displacement and privacy.
    
    2. Increased reliance on AI for routine tasks: As AI becomes more sophisticated, we may see more routine tasks that are now handled by machines, such as financial transactions, healthcare, and customer service. This could lead to a decrease in the need for human workers, but


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

    name

    ]

     and

     I

     am

     a

     [

    job

     or

     title

    ].

     I

     started

     my

     professional

     journey

     [

    insert

     date

    ]

     in

     [

    location

    ],

     where

     I

     have

     hon

    ed

     my

     skills

     in

     [

    specific

     skill

     or

     field

    ].

     I

     am

     a

     [

    career

     goal

    ]

     with

     [

    number

     of

     years

     of

     experience

    ]

     years

     of

     experience

    ,

     and

     I

     am

     always

     looking

     to

     [

    what

     they

     can

     do

     to

     improve

     or

     learn

    ].

     I

     am

     a

     [

    type

     of

     person

    ]

     with

     a

     [

    d

    ut

    iful

    ,

     curious

    ,

     responsible

    ,

     etc

    .

    ].

     I

     am

     [

    job

     title

    ]

     and

     I

     am

     a

     [

    position

    ],

     with

     [

    number

     of

     years

     of

     experience

    ]

     years

     of

     experience

    .

     I

     am

     a

     [

    role

     model

    ],

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     known

     for

     its

     historical

     landmarks

    ,

     vibrant

     arts

     scene

    ,

     and

     renowned

     museums

     such

     as

     the

     Lou

    vre

     and

     Mus

    ée

     d

    '

    Or

    say

    .

     Its

     status

     as

     the

     world

    's

     second

    -largest

     city

     is

     also

     recognized

    ,

     with

     many

     major

     corporations

     and

     institutions

     headquartered

     here

    .

     Paris

     is

     renowned

     for

     its

     cuisine

    ,

     including

     its

     iconic

     French

     cuisine

    ,

     and

     is

     a

     popular

     tourist

     destination

    .

     Despite

     its

     size

    ,

     Paris

     is

     also

     a

     beautiful

    ,

     tranquil

     city

    ,

     with

     its

     medieval

     architecture

     and

     picturesque

     can

    als

    .

     It

     is

     a

     city

     with

     a

     rich

     history

     and

     culture

    ,

     making

     it

     a

     significant

     part

     of

     France

    's

     cultural

     identity

    .

     The

     French

     government

     recognizes

     the

     city

    's

     importance

     and

     invest

    s

     heavily

     in

     it

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     rapidly

     changing

    ,

     and

     it

     is

     unlikely

     to

     be

     the

     same

     as

     it

     is

     today

    .

     Here

     are

     some

     possible

     future

     trends

     in

     artificial

     intelligence

    :
    


    1

    .

     Autonomous

     machines

    :

     We

     are

     already

     seeing

     the

     development

     of

     self

    -driving

     cars

    ,

     but

     we

     will

     see

     more

     autonomous

     machines

     in

     the

     future

    .

     These

     machines

     will

     be

     able

     to

     make

     decisions

     on

     their

     own

     and

     perform

     tasks

     in

     a

     way

     that

     is

     safer

     and

     more

     efficient

     than

     human

     drivers

    .
    


    2

    .

     Machine

     learning

    :

     Machine

     learning

     is

     a

     field

     of

     AI

     that

     allows

     computers

     to

     learn

     from

     data

     without

     being

     explicitly

     programmed

    .

     This

     technology

     is

     already

     being

     used

     in

     various

     industries

    ,

     from

     healthcare

     to

     finance

    .
    


    3

    .

     AI

     ethics

    :

     The

     future

     of

     AI

    



```python
llm.shutdown()
```
