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


    2026-04-01 07:32:56.776 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 07:32:56] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 07:32:56.776 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 07:32:56] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 07:32:56.776 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 07:32:56] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 07:32:56.776 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 07:32:56] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 07:32:56.776 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 07:32:56] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  3.83it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  3.82it/s]


    2026-04-01 07:32:59,615 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 07:32:59] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:33,  2.69s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:33,  2.69s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:33,  2.69s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:33,  2.69s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:28,  1.86it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:28,  1.86it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:28,  1.86it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:28,  1.86it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:28,  1.86it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:28,  1.86it/s]

    Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:28,  1.86it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.71it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.71it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.71it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.71it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:08,  5.71it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:08,  5.71it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:08,  5.71it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:08,  5.71it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:03<00:03, 11.27it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:03<00:03, 11.27it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:03<00:03, 11.27it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:03<00:03, 11.27it/s]

    Compiling num tokens (num_tokens=1024):  29%|██▉       | 17/58 [00:03<00:03, 11.27it/s]Compiling num tokens (num_tokens=960):  29%|██▉       | 17/58 [00:03<00:03, 11.27it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:03<00:02, 14.09it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:03<00:02, 14.09it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:03<00:02, 14.09it/s]Compiling num tokens (num_tokens=768):  38%|███▊      | 22/58 [00:03<00:02, 14.09it/s]Compiling num tokens (num_tokens=704):  38%|███▊      | 22/58 [00:03<00:02, 14.09it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 16.80it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 16.80it/s]

    Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 16.80it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 16.80it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 16.80it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:03<00:01, 19.14it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:03<00:01, 19.14it/s]Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:03<00:01, 19.14it/s]Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:03<00:01, 19.14it/s]Compiling num tokens (num_tokens=352):  52%|█████▏    | 30/58 [00:03<00:01, 19.14it/s]

    Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:03<00:01, 22.24it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:03<00:01, 22.24it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:03<00:01, 22.24it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:03<00:01, 22.24it/s]Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:03<00:01, 22.24it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 24.22it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 24.22it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 24.22it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 24.22it/s]

    Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 24.22it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 24.22it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 24.22it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 30.49it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 30.49it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 30.49it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 30.49it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 30.49it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 30.49it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 30.49it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 36.88it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 36.88it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 36.88it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 36.88it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 36.88it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 36.88it/s]

    Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 36.88it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 36.88it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 36.88it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 14.48it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=63.24 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=63.21 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=63.21 GB):   3%|▎         | 2/58 [00:00<00:03, 15.61it/s]Capturing num tokens (num_tokens=7168 avail_mem=63.21 GB):   3%|▎         | 2/58 [00:00<00:03, 15.61it/s]Capturing num tokens (num_tokens=6656 avail_mem=63.21 GB):   3%|▎         | 2/58 [00:00<00:03, 15.61it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=63.21 GB):   7%|▋         | 4/58 [00:00<00:03, 15.49it/s]Capturing num tokens (num_tokens=6144 avail_mem=63.21 GB):   7%|▋         | 4/58 [00:00<00:03, 15.49it/s]Capturing num tokens (num_tokens=5632 avail_mem=63.20 GB):   7%|▋         | 4/58 [00:00<00:03, 15.49it/s]Capturing num tokens (num_tokens=5632 avail_mem=63.20 GB):  10%|█         | 6/58 [00:00<00:03, 17.16it/s]Capturing num tokens (num_tokens=5120 avail_mem=63.20 GB):  10%|█         | 6/58 [00:00<00:03, 17.16it/s]Capturing num tokens (num_tokens=4608 avail_mem=63.20 GB):  10%|█         | 6/58 [00:00<00:03, 17.16it/s]Capturing num tokens (num_tokens=4096 avail_mem=63.20 GB):  10%|█         | 6/58 [00:00<00:03, 17.16it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=63.20 GB):  16%|█▌        | 9/58 [00:00<00:02, 18.87it/s]Capturing num tokens (num_tokens=3840 avail_mem=63.20 GB):  16%|█▌        | 9/58 [00:00<00:02, 18.87it/s]Capturing num tokens (num_tokens=3584 avail_mem=63.19 GB):  16%|█▌        | 9/58 [00:00<00:02, 18.87it/s]Capturing num tokens (num_tokens=3328 avail_mem=63.19 GB):  16%|█▌        | 9/58 [00:00<00:02, 18.87it/s]Capturing num tokens (num_tokens=3328 avail_mem=63.19 GB):  21%|██        | 12/58 [00:00<00:02, 21.49it/s]Capturing num tokens (num_tokens=3072 avail_mem=63.18 GB):  21%|██        | 12/58 [00:00<00:02, 21.49it/s]Capturing num tokens (num_tokens=2816 avail_mem=63.18 GB):  21%|██        | 12/58 [00:00<00:02, 21.49it/s]Capturing num tokens (num_tokens=2560 avail_mem=63.18 GB):  21%|██        | 12/58 [00:00<00:02, 21.49it/s]Capturing num tokens (num_tokens=2304 avail_mem=63.18 GB):  21%|██        | 12/58 [00:00<00:02, 21.49it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=63.18 GB):  28%|██▊       | 16/58 [00:00<00:01, 24.48it/s]Capturing num tokens (num_tokens=2048 avail_mem=63.17 GB):  28%|██▊       | 16/58 [00:00<00:01, 24.48it/s]Capturing num tokens (num_tokens=1792 avail_mem=63.17 GB):  28%|██▊       | 16/58 [00:00<00:01, 24.48it/s]Capturing num tokens (num_tokens=1536 avail_mem=63.17 GB):  28%|██▊       | 16/58 [00:00<00:01, 24.48it/s]Capturing num tokens (num_tokens=1280 avail_mem=63.16 GB):  28%|██▊       | 16/58 [00:00<00:01, 24.48it/s]Capturing num tokens (num_tokens=1280 avail_mem=63.16 GB):  34%|███▍      | 20/58 [00:00<00:01, 26.72it/s]Capturing num tokens (num_tokens=1024 avail_mem=63.14 GB):  34%|███▍      | 20/58 [00:00<00:01, 26.72it/s]Capturing num tokens (num_tokens=960 avail_mem=63.16 GB):  34%|███▍      | 20/58 [00:00<00:01, 26.72it/s] Capturing num tokens (num_tokens=896 avail_mem=63.15 GB):  34%|███▍      | 20/58 [00:00<00:01, 26.72it/s]Capturing num tokens (num_tokens=832 avail_mem=63.15 GB):  34%|███▍      | 20/58 [00:00<00:01, 26.72it/s]

    Capturing num tokens (num_tokens=832 avail_mem=63.15 GB):  41%|████▏     | 24/58 [00:00<00:01, 29.36it/s]Capturing num tokens (num_tokens=768 avail_mem=63.15 GB):  41%|████▏     | 24/58 [00:00<00:01, 29.36it/s]Capturing num tokens (num_tokens=704 avail_mem=63.14 GB):  41%|████▏     | 24/58 [00:01<00:01, 29.36it/s]Capturing num tokens (num_tokens=640 avail_mem=63.14 GB):  41%|████▏     | 24/58 [00:01<00:01, 29.36it/s]Capturing num tokens (num_tokens=576 avail_mem=63.14 GB):  41%|████▏     | 24/58 [00:01<00:01, 29.36it/s]Capturing num tokens (num_tokens=512 avail_mem=63.13 GB):  41%|████▏     | 24/58 [00:01<00:01, 29.36it/s]Capturing num tokens (num_tokens=512 avail_mem=63.13 GB):  50%|█████     | 29/58 [00:01<00:00, 33.45it/s]Capturing num tokens (num_tokens=480 avail_mem=63.14 GB):  50%|█████     | 29/58 [00:01<00:00, 33.45it/s]Capturing num tokens (num_tokens=448 avail_mem=63.14 GB):  50%|█████     | 29/58 [00:01<00:00, 33.45it/s]Capturing num tokens (num_tokens=416 avail_mem=63.14 GB):  50%|█████     | 29/58 [00:01<00:00, 33.45it/s]

    Capturing num tokens (num_tokens=384 avail_mem=63.14 GB):  50%|█████     | 29/58 [00:01<00:00, 33.45it/s]Capturing num tokens (num_tokens=384 avail_mem=63.14 GB):  57%|█████▋    | 33/58 [00:01<00:00, 34.49it/s]Capturing num tokens (num_tokens=352 avail_mem=63.13 GB):  57%|█████▋    | 33/58 [00:01<00:00, 34.49it/s]Capturing num tokens (num_tokens=320 avail_mem=63.13 GB):  57%|█████▋    | 33/58 [00:01<00:00, 34.49it/s]Capturing num tokens (num_tokens=288 avail_mem=63.12 GB):  57%|█████▋    | 33/58 [00:01<00:00, 34.49it/s]Capturing num tokens (num_tokens=256 avail_mem=63.12 GB):  57%|█████▋    | 33/58 [00:01<00:00, 34.49it/s]

    Capturing num tokens (num_tokens=256 avail_mem=63.12 GB):  64%|██████▍   | 37/58 [00:01<00:00, 27.19it/s]Capturing num tokens (num_tokens=240 avail_mem=63.12 GB):  64%|██████▍   | 37/58 [00:01<00:00, 27.19it/s]Capturing num tokens (num_tokens=224 avail_mem=63.12 GB):  64%|██████▍   | 37/58 [00:01<00:00, 27.19it/s]Capturing num tokens (num_tokens=208 avail_mem=63.11 GB):  64%|██████▍   | 37/58 [00:01<00:00, 27.19it/s]Capturing num tokens (num_tokens=192 avail_mem=63.11 GB):  64%|██████▍   | 37/58 [00:01<00:00, 27.19it/s]Capturing num tokens (num_tokens=192 avail_mem=63.11 GB):  71%|███████   | 41/58 [00:01<00:00, 29.10it/s]Capturing num tokens (num_tokens=176 avail_mem=63.11 GB):  71%|███████   | 41/58 [00:01<00:00, 29.10it/s]Capturing num tokens (num_tokens=160 avail_mem=63.10 GB):  71%|███████   | 41/58 [00:01<00:00, 29.10it/s]Capturing num tokens (num_tokens=144 avail_mem=63.10 GB):  71%|███████   | 41/58 [00:01<00:00, 29.10it/s]Capturing num tokens (num_tokens=128 avail_mem=63.10 GB):  71%|███████   | 41/58 [00:01<00:00, 29.10it/s]

    Capturing num tokens (num_tokens=128 avail_mem=63.10 GB):  78%|███████▊  | 45/58 [00:01<00:00, 29.92it/s]Capturing num tokens (num_tokens=112 avail_mem=63.10 GB):  78%|███████▊  | 45/58 [00:01<00:00, 29.92it/s]Capturing num tokens (num_tokens=96 avail_mem=63.09 GB):  78%|███████▊  | 45/58 [00:01<00:00, 29.92it/s] Capturing num tokens (num_tokens=80 avail_mem=63.09 GB):  78%|███████▊  | 45/58 [00:01<00:00, 29.92it/s]Capturing num tokens (num_tokens=64 avail_mem=63.08 GB):  78%|███████▊  | 45/58 [00:01<00:00, 29.92it/s]Capturing num tokens (num_tokens=64 avail_mem=63.08 GB):  84%|████████▍ | 49/58 [00:01<00:00, 31.07it/s]Capturing num tokens (num_tokens=48 avail_mem=63.08 GB):  84%|████████▍ | 49/58 [00:01<00:00, 31.07it/s]Capturing num tokens (num_tokens=32 avail_mem=63.08 GB):  84%|████████▍ | 49/58 [00:01<00:00, 31.07it/s]Capturing num tokens (num_tokens=28 avail_mem=63.07 GB):  84%|████████▍ | 49/58 [00:01<00:00, 31.07it/s]Capturing num tokens (num_tokens=24 avail_mem=63.07 GB):  84%|████████▍ | 49/58 [00:01<00:00, 31.07it/s]

    Capturing num tokens (num_tokens=24 avail_mem=63.07 GB):  91%|█████████▏| 53/58 [00:01<00:00, 32.36it/s]Capturing num tokens (num_tokens=20 avail_mem=63.07 GB):  91%|█████████▏| 53/58 [00:01<00:00, 32.36it/s]Capturing num tokens (num_tokens=16 avail_mem=63.07 GB):  91%|█████████▏| 53/58 [00:01<00:00, 32.36it/s]Capturing num tokens (num_tokens=12 avail_mem=63.06 GB):  91%|█████████▏| 53/58 [00:01<00:00, 32.36it/s]Capturing num tokens (num_tokens=8 avail_mem=63.06 GB):  91%|█████████▏| 53/58 [00:01<00:00, 32.36it/s] Capturing num tokens (num_tokens=8 avail_mem=63.06 GB):  98%|█████████▊| 57/58 [00:02<00:00, 33.31it/s]Capturing num tokens (num_tokens=4 avail_mem=63.05 GB):  98%|█████████▊| 57/58 [00:02<00:00, 33.31it/s]Capturing num tokens (num_tokens=4 avail_mem=63.05 GB): 100%|██████████| 58/58 [00:02<00:00, 28.41it/s]


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
    Generated text:  Anjali. I am a student of fifth year in the class of 2023 at the University of Ottawa in Canada. My favorite thing to do is to play video games, especially the latest ones, the latest ones I've played. My favorite movie to watch is "Inception." I like to eat pizza and ice cream, and I have a crush on my crushes in the sense that they always catch my eye. On weekends, I like to do the odd activity, like riding a bike or going for a run. 
    
    My favorite animal is a cat. I always love going on a cat-related trip.
    ===============================
    Prompt: The president of the United States is
    Generated text:  a government official who serves as the leader of the executive branch of the federal government. He or she is elected by the people to a term of six years.
    The vice president of the United States is a government official who serves as the second in command of the executive branch of the federal government. He or she is appointed by the president and is elected by the people to a term of five years. The vice president and the president have the same powers, but they serve in the same office and cannot be the same person at the same time.
    The President and Vice President share many responsibilities. The vice president acts as a backup or a temporary
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. What is the capital of the United Kingdom? The capital of the United Kingdom is London. 
    
    To break it down:
    
    1. The United Kingdom consists of two parts: England and Wales.
    2. London is the capital of England, not the capital of the United Kingdom.
    3. The capital of the United Kingdom is London, which is the largest and most populous city in the country.
    
    Therefore, the answer is:
    
    \boxed{London} (or "London" for clarity)
    ===============================
    Prompt: The future of AI is
    Generated text:  an exciting one. How will it affect our daily lives? How will the future of AI impact our personal relationships? As an AI language model, I don't have personal views or beliefs, but I can provide some general information on the topic.
    
    The future of AI is likely to have a significant impact on our daily lives in various ways. AI has already made our lives more efficient and convenient by automating tasks, reducing costs, and improving accuracy in various fields such as healthcare, finance, and transportation. However, there are also concerns about the potential ethical and societal implications of AI, such as job loss, privacy concerns, and the possibility


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


    Generated text:  Paris, also known as the City of Light. It is a historic city with a rich history dating back to the Middle Ages, and is known for its beautiful architecture, vibrant culture, and annual festivals. Paris is also home to many world-renowned museums, including the Louvre, the Musée d'Orsay, and the Musée Rodin. The city is also known for its fashion industry, with many famous designers and boutiques. Paris is a popular tourist destination, with millions of visitors each year. It is a cultural and economic hub of France, and is a major transportation hub for the country. The city is also
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by rapid advancements in areas such as machine learning, natural language processing, and computer vision. Some potential future trends include:
    
    1. Increased integration of AI into everyday life: AI is already being integrated into our daily lives, from voice assistants like Siri and Alexa to self-driving cars. As AI becomes more integrated into our daily lives, we can expect to see even more widespread adoption of AI in our daily routines.
    
    2. AI becoming more autonomous: As AI becomes more integrated into our daily lives, we can expect to see more autonomous AI systems that can perform tasks without human intervention. This could lead to a more efficient and
    


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
    Generated text:  [Name], and I'm a [Age] year-old. I started writing novels at the age of [Year] and have always been passionate about writing. I believe that writing is a powerful way to express my emotions, and I enjoy exploring different genres and themes. I believe that my unique voice and style are what set me apart from other writers, and I am always looking for new experiences and challenges in my writing journey. What's your favorite book or novel to read and why? As an AI language model, I don't have personal experiences or preferences, but I can tell you that many people enjoy reading books like "The Hob
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. It is the most populous city in Europe and the world, with an estimated population of around 2 million. Paris is known for its rich history, fine cuisine, and diverse culture, and it has a strong cultural and political influence in Europe and beyond. It is a popular tourist destination and a popular meeting place for artists, writers, and intellectuals. The city is also home to the famous Eiffel Tower and is recognized as the 20th most populous city in the world. It is often referred to as the "city of lights" due to its beautiful lights, including the Eiffel Tower, the Louvre
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be characterized by rapid advancements in machine learning, deep learning, and other areas of research. Here are some possible future trends in AI:
    
    1. Increased use of AI in healthcare: AI will play a key role in improving the accuracy and efficiency of healthcare delivery, by assisting doctors and nurses in diagnosing and treating diseases, monitoring patients, and even assisting in patient care coordination.
    
    2. AI-powered delivery of remote healthcare services: As more people become connected to the internet, we can expect to see more AI-powered delivery of remote healthcare services, such as telemedicine, remote patient monitoring, and remote health management.
    
    3. AI-enabled


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

     [

    Age

    ].

     I

     enjoy

     [

    What

     I

     Enjoy

     Doing

    ]

     with

     [

    A

     Partner

     or

     a

     F

    urry

     Friend

    ].

     I

     am

     passionate

     about

     [

    My

     Hobby

     or

     Passion

    ].

     I

     have

     been

     [

    Duration

     of

     My

     Career

     or

     Growth

    ]

     since

     [

    Year

    ],

     and

     I

     strive

     to

     [

    My

     Goal

     or

     Responsibility

    ].

     My

     journey

     so

     far

     has

     been

     [

    Short

     Biography

    ],

     and

     I

     am

     always

     looking

     for

     opportunities

     to

     [

    New

     Thing

    ].

     My

     future

     aspirations

     are

     [

    Future

     Goal

     or

     Hobby

    ].

     I

     believe

     that

     [

    My

     Education

     or

     Career

     Objective

    ]

     is

     a

     [

    Future

     Goal

     or

     Hobby

    ].

     I

     am

     dedicated

     to

     [

    My

     Life

     Purpose

     or

     Eth

    ical

     Values

    ]

     and

     I

     strive

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     It

     is

     a

     city

     known

     for

     its

     rich

     history

    ,

     beautiful

     architecture

    ,

     and

     vibrant

     culture

    .

     The

     city

     is

     home

     to

     many

     famous

     landmarks

    ,

     including

     the

     E

    iff

    el

     Tower

    ,

     the

     Lou

    vre

     Museum

    ,

     and

     the

     Notre

    -D

    ame

     Cathedral

    .

     Paris

     is

     also

     a

     major

     tourist

     destination

    ,

     with

     millions

     of

     visitors

     annually

     exploring

     its

     beautiful

     streets

    ,

     museums

    ,

     and

     food

     scene

    .

     The

     city

     has

     a

     strong

     emphasis

     on

     the

     arts

    ,

     with

     many

     art

     galleries

     and

     theaters

     serving

     as

     important

     parts

     of

     its

     cultural

     life

    .

     Overall

    ,

     Paris

     is

     a

     beloved

     and

     impressive

     city

     with

     a

     rich

     history

     and

     a

     vibrant

     community

    .

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     full

     of

     potential

     and

     promise

    ,

     but

     it

     is

     also

     full

     of

     challenges

     and

     uncertainties

    .

     Some

     of

     the

     possible

     trends

     in

     AI

     include

    :
    


    1

    .

     Increased

     AI

     integration

     with

     human

     AI

    :

     As

     AI

     technology

     becomes

     more

     advanced

     and

     integrated

     with

     human

     AI

    ,

     more

     human

     AI

     will

     be

     developed

     and

     used

     in

     various

     applications

    ,

     including

     healthcare

    ,

     finance

    ,

     and

     transportation

    .
    


    2

    .

     Enhanced

     AI

     ethics

     and

     responsibility

    :

     As

     AI

     technology

     becomes

     more

     complex

    ,

     it

     will

     become

     increasingly

     difficult

     to

     define

     the

     ethical

     and

     moral

     responsibilities

     of

     AI

     systems

    .

     Governments

     and

     other

     stakeholders

     will

     need

     to

     develop

     guidelines

     and

     regulations

     to

     ensure

     that

     AI

     systems

     are

     used

     in

     a

     responsible

     and

     ethical

     manner

    .
    


    3

    .

     AI

     democrat

    ization

    :

    



```python
llm.shutdown()
```
