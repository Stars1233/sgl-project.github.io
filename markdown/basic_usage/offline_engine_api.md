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


    2026-03-26 18:31:00.636 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 18:31:00] Persistent cache disabled, using in-memory JIT cache
    2026-03-26 18:31:00.637 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 18:31:00] Persistent cache disabled, using in-memory JIT cache
    2026-03-26 18:31:00.637 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 18:31:00] Persistent cache disabled, using in-memory JIT cache
    2026-03-26 18:31:00.637 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 18:31:00] Persistent cache disabled, using in-memory JIT cache
    2026-03-26 18:31:00.637 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-26 18:31:00] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.00it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.98it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:14,  2.35s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:14,  2.35s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:14,  2.35s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:02<00:35,  1.54it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:02<00:35,  1.54it/s]Compiling num tokens (num_tokens=6144):   5%|▌         | 3/58 [00:02<00:35,  1.54it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:18,  2.92it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:18,  2.92it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:18,  2.92it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:18,  2.92it/s]Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:02<00:09,  5.40it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:02<00:09,  5.40it/s]Compiling num tokens (num_tokens=3840):  14%|█▍        | 8/58 [00:02<00:09,  5.40it/s]Compiling num tokens (num_tokens=3584):  14%|█▍        | 8/58 [00:02<00:09,  5.40it/s]

    Compiling num tokens (num_tokens=3328):  14%|█▍        | 8/58 [00:02<00:09,  5.40it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:02<00:04,  9.21it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:02<00:04,  9.21it/s]Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:02<00:04,  9.21it/s]Compiling num tokens (num_tokens=2560):  21%|██        | 12/58 [00:02<00:04,  9.21it/s]Compiling num tokens (num_tokens=2304):  21%|██        | 12/58 [00:02<00:04,  9.21it/s]Compiling num tokens (num_tokens=2048):  21%|██        | 12/58 [00:02<00:04,  9.21it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:02<00:02, 14.58it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:02<00:02, 14.58it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:02<00:02, 14.58it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:02<00:02, 14.58it/s]

    Compiling num tokens (num_tokens=1024):  29%|██▉       | 17/58 [00:03<00:02, 14.58it/s]Compiling num tokens (num_tokens=960):  29%|██▉       | 17/58 [00:03<00:02, 14.58it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:03<00:01, 19.82it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:03<00:01, 19.82it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:03<00:01, 19.82it/s]Compiling num tokens (num_tokens=768):  38%|███▊      | 22/58 [00:03<00:01, 19.82it/s]Compiling num tokens (num_tokens=704):  38%|███▊      | 22/58 [00:03<00:01, 19.82it/s]Compiling num tokens (num_tokens=640):  38%|███▊      | 22/58 [00:03<00:01, 19.82it/s]Compiling num tokens (num_tokens=576):  38%|███▊      | 22/58 [00:03<00:01, 19.82it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:03<00:01, 27.09it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:03<00:01, 27.09it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:03<00:01, 27.09it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:03<00:01, 27.09it/s]Compiling num tokens (num_tokens=416):  48%|████▊     | 28/58 [00:03<00:01, 27.09it/s]

    Compiling num tokens (num_tokens=384):  48%|████▊     | 28/58 [00:03<00:01, 27.09it/s]Compiling num tokens (num_tokens=352):  48%|████▊     | 28/58 [00:03<00:01, 27.09it/s]Compiling num tokens (num_tokens=320):  48%|████▊     | 28/58 [00:03<00:01, 27.09it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:03<00:00, 34.56it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:03<00:00, 34.56it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:03<00:00, 34.56it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:03<00:00, 34.56it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:03<00:00, 34.56it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:03<00:00, 34.56it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:03<00:00, 36.81it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:03<00:00, 36.81it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:03<00:00, 36.81it/s]

    Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:03<00:00, 36.81it/s]Compiling num tokens (num_tokens=144):  69%|██████▉   | 40/58 [00:03<00:00, 36.81it/s]Compiling num tokens (num_tokens=128):  69%|██████▉   | 40/58 [00:03<00:00, 36.81it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 39.21it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 39.21it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 39.21it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 39.21it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 39.21it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 39.21it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 40.35it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 40.35it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 40.35it/s]

    Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 40.35it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 40.35it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 40.35it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 40.35it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:03<00:00, 45.28it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:03<00:00, 45.28it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:03<00:00, 45.28it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.51it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.87 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=118.87 GB):   2%|▏         | 1/58 [00:00<00:17,  3.21it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.84 GB):   2%|▏         | 1/58 [00:00<00:17,  3.21it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.84 GB):   3%|▎         | 2/58 [00:00<00:10,  5.23it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.84 GB):   3%|▎         | 2/58 [00:00<00:10,  5.23it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=118.84 GB):   3%|▎         | 2/58 [00:00<00:10,  5.23it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.84 GB):   7%|▋         | 4/58 [00:00<00:06,  7.78it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.84 GB):   7%|▋         | 4/58 [00:00<00:06,  7.78it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.83 GB):   7%|▋         | 4/58 [00:00<00:06,  7.78it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.83 GB):   7%|▋         | 4/58 [00:00<00:06,  7.78it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=118.83 GB):  12%|█▏        | 7/58 [00:00<00:03, 12.92it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.83 GB):  12%|█▏        | 7/58 [00:00<00:03, 12.92it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.83 GB):  12%|█▏        | 7/58 [00:00<00:03, 12.92it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.83 GB):  12%|█▏        | 7/58 [00:00<00:03, 12.92it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.82 GB):  12%|█▏        | 7/58 [00:00<00:03, 12.92it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.82 GB):  19%|█▉        | 11/58 [00:00<00:02, 19.49it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.82 GB):  19%|█▉        | 11/58 [00:00<00:02, 19.49it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.81 GB):  19%|█▉        | 11/58 [00:00<00:02, 19.49it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.81 GB):  19%|█▉        | 11/58 [00:00<00:02, 19.49it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.81 GB):  19%|█▉        | 11/58 [00:00<00:02, 19.49it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=118.81 GB):  19%|█▉        | 11/58 [00:00<00:02, 19.49it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.81 GB):  28%|██▊       | 16/58 [00:00<00:01, 26.51it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.80 GB):  28%|██▊       | 16/58 [00:00<00:01, 26.51it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.80 GB):  28%|██▊       | 16/58 [00:00<00:01, 26.51it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.80 GB):  28%|██▊       | 16/58 [00:00<00:01, 26.51it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.79 GB):  28%|██▊       | 16/58 [00:01<00:01, 26.51it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.77 GB):  28%|██▊       | 16/58 [00:01<00:01, 26.51it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.77 GB):  36%|███▌      | 21/58 [00:01<00:01, 31.85it/s]Capturing num tokens (num_tokens=960 avail_mem=118.79 GB):  36%|███▌      | 21/58 [00:01<00:01, 31.85it/s] Capturing num tokens (num_tokens=896 avail_mem=118.78 GB):  36%|███▌      | 21/58 [00:01<00:01, 31.85it/s]Capturing num tokens (num_tokens=832 avail_mem=118.78 GB):  36%|███▌      | 21/58 [00:01<00:01, 31.85it/s]Capturing num tokens (num_tokens=768 avail_mem=118.78 GB):  36%|███▌      | 21/58 [00:01<00:01, 31.85it/s]

    Capturing num tokens (num_tokens=704 avail_mem=118.77 GB):  36%|███▌      | 21/58 [00:01<00:01, 31.85it/s]Capturing num tokens (num_tokens=704 avail_mem=118.77 GB):  45%|████▍     | 26/58 [00:01<00:00, 35.81it/s]Capturing num tokens (num_tokens=640 avail_mem=118.77 GB):  45%|████▍     | 26/58 [00:01<00:00, 35.81it/s]Capturing num tokens (num_tokens=576 avail_mem=118.77 GB):  45%|████▍     | 26/58 [00:01<00:00, 35.81it/s]Capturing num tokens (num_tokens=512 avail_mem=118.76 GB):  45%|████▍     | 26/58 [00:01<00:00, 35.81it/s]Capturing num tokens (num_tokens=480 avail_mem=118.77 GB):  45%|████▍     | 26/58 [00:01<00:00, 35.81it/s]Capturing num tokens (num_tokens=480 avail_mem=118.77 GB):  52%|█████▏    | 30/58 [00:01<00:00, 36.47it/s]Capturing num tokens (num_tokens=448 avail_mem=118.77 GB):  52%|█████▏    | 30/58 [00:01<00:00, 36.47it/s]Capturing num tokens (num_tokens=416 avail_mem=118.77 GB):  52%|█████▏    | 30/58 [00:01<00:00, 36.47it/s]Capturing num tokens (num_tokens=384 avail_mem=118.77 GB):  52%|█████▏    | 30/58 [00:01<00:00, 36.47it/s]

    Capturing num tokens (num_tokens=352 avail_mem=118.76 GB):  52%|█████▏    | 30/58 [00:01<00:00, 36.47it/s]Capturing num tokens (num_tokens=352 avail_mem=118.76 GB):  59%|█████▊    | 34/58 [00:01<00:00, 35.35it/s]Capturing num tokens (num_tokens=320 avail_mem=118.76 GB):  59%|█████▊    | 34/58 [00:01<00:00, 35.35it/s]Capturing num tokens (num_tokens=288 avail_mem=118.75 GB):  59%|█████▊    | 34/58 [00:01<00:00, 35.35it/s]Capturing num tokens (num_tokens=256 avail_mem=118.75 GB):  59%|█████▊    | 34/58 [00:01<00:00, 35.35it/s]Capturing num tokens (num_tokens=240 avail_mem=118.75 GB):  59%|█████▊    | 34/58 [00:01<00:00, 35.35it/s]Capturing num tokens (num_tokens=240 avail_mem=118.75 GB):  66%|██████▌   | 38/58 [00:01<00:00, 34.65it/s]Capturing num tokens (num_tokens=224 avail_mem=118.75 GB):  66%|██████▌   | 38/58 [00:01<00:00, 34.65it/s]Capturing num tokens (num_tokens=208 avail_mem=118.74 GB):  66%|██████▌   | 38/58 [00:01<00:00, 34.65it/s]

    Capturing num tokens (num_tokens=192 avail_mem=118.74 GB):  66%|██████▌   | 38/58 [00:01<00:00, 34.65it/s]Capturing num tokens (num_tokens=176 avail_mem=118.74 GB):  66%|██████▌   | 38/58 [00:01<00:00, 34.65it/s]Capturing num tokens (num_tokens=176 avail_mem=118.74 GB):  72%|███████▏  | 42/58 [00:01<00:00, 34.22it/s]Capturing num tokens (num_tokens=160 avail_mem=118.73 GB):  72%|███████▏  | 42/58 [00:01<00:00, 34.22it/s]Capturing num tokens (num_tokens=144 avail_mem=118.73 GB):  72%|███████▏  | 42/58 [00:01<00:00, 34.22it/s]Capturing num tokens (num_tokens=128 avail_mem=118.73 GB):  72%|███████▏  | 42/58 [00:01<00:00, 34.22it/s]Capturing num tokens (num_tokens=112 avail_mem=118.73 GB):  72%|███████▏  | 42/58 [00:01<00:00, 34.22it/s]Capturing num tokens (num_tokens=112 avail_mem=118.73 GB):  79%|███████▉  | 46/58 [00:01<00:00, 34.22it/s]Capturing num tokens (num_tokens=96 avail_mem=118.72 GB):  79%|███████▉  | 46/58 [00:01<00:00, 34.22it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=118.72 GB):  79%|███████▉  | 46/58 [00:01<00:00, 34.22it/s]Capturing num tokens (num_tokens=64 avail_mem=118.72 GB):  79%|███████▉  | 46/58 [00:01<00:00, 34.22it/s]Capturing num tokens (num_tokens=48 avail_mem=118.71 GB):  79%|███████▉  | 46/58 [00:01<00:00, 34.22it/s]Capturing num tokens (num_tokens=48 avail_mem=118.71 GB):  86%|████████▌ | 50/58 [00:01<00:00, 33.83it/s]Capturing num tokens (num_tokens=32 avail_mem=118.71 GB):  86%|████████▌ | 50/58 [00:01<00:00, 33.83it/s]Capturing num tokens (num_tokens=28 avail_mem=118.70 GB):  86%|████████▌ | 50/58 [00:01<00:00, 33.83it/s]Capturing num tokens (num_tokens=24 avail_mem=118.70 GB):  86%|████████▌ | 50/58 [00:01<00:00, 33.83it/s]Capturing num tokens (num_tokens=20 avail_mem=118.70 GB):  86%|████████▌ | 50/58 [00:01<00:00, 33.83it/s]Capturing num tokens (num_tokens=20 avail_mem=118.70 GB):  93%|█████████▎| 54/58 [00:01<00:00, 35.22it/s]Capturing num tokens (num_tokens=16 avail_mem=118.70 GB):  93%|█████████▎| 54/58 [00:01<00:00, 35.22it/s]

    Capturing num tokens (num_tokens=12 avail_mem=118.69 GB):  93%|█████████▎| 54/58 [00:01<00:00, 35.22it/s]Capturing num tokens (num_tokens=8 avail_mem=118.69 GB):  93%|█████████▎| 54/58 [00:02<00:00, 35.22it/s] Capturing num tokens (num_tokens=4 avail_mem=118.69 GB):  93%|█████████▎| 54/58 [00:02<00:00, 35.22it/s]Capturing num tokens (num_tokens=4 avail_mem=118.69 GB): 100%|██████████| 58/58 [00:02<00:00, 28.16it/s]


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
    Generated text:  Li Qiu, I am a 17-year-old female, 165 cm tall. Recently, I have been experiencing severe fatigue, poor appetite, dizziness, and insomnia. I often feel anxious and irritable, and I'm worried that I might be going through a period of illness or that I might be going through a mental health crisis. 
    
    I'm worried that if I continue to act this way, I could harm myself or my loved ones. I believe I need to seek professional help to understand the cause of these symptoms and manage them effectively. What should I do? Do I need to take medication? Should
    ===============================
    Prompt: The president of the United States is
    Generated text:  currently 32 years older than the president of Mexico. The president of Mexico will reach an age three times that of the president of the United States in 15 years. How old are the presidents now? Let's denote the current age of the president of the United States as \( U \) and the current age of the president of Mexico as \( M \).
    
    According to the problem, the president of the United States is currently 32 years older than the president of Mexico. This can be written as:
    \[ U = M + 32 \]
    
    In 15 years, the president of the United States will
    ===============================
    Prompt: The capital of France is
    Generated text:  the _______.
    A. center of culture
    B. center of economy
    C. center of politics
    D. center of society
    
    The capital of France is Paris. Therefore, the answer is C. center of politics.
    ===============================
    Prompt: The future of AI is
    Generated text:  here, and it’s already hitting the commercial space.
    
    As AI technology continues to improve, it’s becoming more and more important to ensure that it’s used responsibly and ethically. This is where the DAI team at AI42 comes in.
    
    AI42 is a non-profit organization that is dedicated to advancing the field of artificial intelligence. They work to ensure that AI is used in a responsible and ethical manner, by providing resources and tools for AI researchers and developers to create and share AI solutions.
    
    One of the ways in which AI42 is doing this is by providing a platform for researchers and developers to collaborate and share their work


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


    Generated text:  [Name] and I am a [job title] at [company name]. I am passionate about [job title] and have been working in this field for [number of years] years. I am always looking for new challenges and opportunities to grow and learn. I am a [job title] and I am always looking for ways to improve my skills and knowledge. I am a [job title] and I am always looking for ways to improve my skills and knowledge. I am a [job title] and I am always looking for ways to improve my skills and knowledge. I am a [job title] and I am always looking
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as the City of Light, and is the largest city in the European Union and the second-largest city in the world by population. It is located on the Seine River and is home to many of France's most famous landmarks, including the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. Paris is also known for its rich cultural heritage, including its art, music, and cuisine. The city is home to many important institutions, including the French Academy of Sciences and the French National Library. Paris is a vibrant and dynamic city with a rich history and a strong sense of community. Its status
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends that could emerge in the coming years:
    
    1. Increased integration of AI into everyday life: As AI becomes more integrated into our daily lives, we may see more widespread adoption of AI-powered technologies such as voice assistants, self-driving cars, and virtual assistants. This could lead to a more seamless and intuitive use of AI in our daily routines.
    
    2. Greater emphasis on ethical and responsible AI: As AI becomes more advanced, there will be a greater emphasis on ensuring that it is used eth
    


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
    Generated text:  ___________ and I am a/an [insert profession or age group, e.g., [insert age group], [insert profession]]. I have been in this field for [insert number of years], and I have always been passionate about [insert one or two things you are passionate about, e.g., [insert hobbies, interests, talents], [insert hobbies, interests, talents], [insert hobbies, interests, talents]]. I am a professional with experience in [insert two words that best describe my field, e.g., [engineer], [marketing], [human resources]]. I am a/an ___________ with experience in [
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, located in the northwestern part of the country and is the largest city in the European Union. It is known for its rich history, architectural styles, and vibrant culture. The city is also famous for its world-renowned fashion and gastronomy, and has played a significant role in France's cultural and political history. As of 2021, Paris has a population of approximately 2.2 million residents. It is the most populated city in France by area, and is often referred to as the "City of Light" and "The Eternal City." Paris is also a major center for business and tourism, and is
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  looking increasingly exciting with the integration of more advanced technologies, such as machine learning, deep learning, and natural language processing. These advanced AI techniques allow for more complex and sophisticated AI systems, which can perform a wide range of tasks and solve complex problems that were previously difficult for humans to solve.
    One possible future trend in AI is the development of AI systems that can perform tasks that are currently beyond the capabilities of humans, such as understanding and interpreting human emotions and behaviors, making decisions based on complex data, and learning and adapting to new situations. Additionally, AI systems are becoming increasingly capable of handling large amounts of data and recognizing patterns in large datasets


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

     a

     [

    occupation

    ]

    !

     My

     professional

     background

     includes

     [

    what

     you

     know

    ],

     [

    and

     [

    any

     other

     relevant

     experience

     or

     education

    ]],

     and

     [

    what

     you

    're

     passionate

     about

     or

     what

     you

     love

     to

     do

    ].

     I

     love

     making

     things

     come

     to

     life

    ,

     [

    what

     makes

     you

     a

     good

     writer

     or

     speaker

    ]?

     My

     favorite

     thing

     to

     do

     is

     [

    mention

     an

     activity

     you

     enjoy

    ].

     [

    Name

    ]

     enjoys

     [

    describe

     something

     that

     makes

     you

     feel

     joy

     or

     happiness

    ].

     I

     love

     [

    mention

     a

     hobby

     or

     activity

     that

     makes

     you

     feel

     relaxed

     and

     at

     peace

    ].

     


    Tell

     me

     more

     about

     your

     interests

     and

     experiences

    .


    As

     a

     seasoned

     professional

     in

     my

     field

    ,

     I

     have

     developed

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     which

     is

     the

     

    1

    5

    th

     largest

     city

     in

     the

     world

     by

     population

    .

     Paris

     is

     home

     to

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

     numerous

     other

     attractions

    .

     The

     city

     is

     known

     for

     its

     rich

     cultural

     history

    ,

     vibrant

     nightlife

    ,

     and

     unique

     blend

     of

     classical

     and

     modern

     architecture

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

     each

     year

    .

     It

    's

     a

     city

     of

     innovation

    ,

     art

    ,

     and

     culture

    ,

     with

     a

     diverse

     population

     of

     over

     

    2

     million

     people

    .

     
    


    Paris

    ,

     also

     known

     as

     "

    La

     Ville

     Lum

    ière

    ,"

     is

     a

     bustling

     met

    ropolis

     of

     over

     

    8

     million

     people

    ,

     making

     it

     the

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     continue

     to

     evolve

     rapidly

    ,

     with

     a

     wide

     range

     of

     possible

     trends

     shaping

     its

     direction

    .

     Some

     of

     the

     potential

     trends

     include

    :
    


    1

    .

     Artificial

     General

     Intelligence

    :

     This

     is

     a

     future

     goal

     where

     AI

     systems

     can

     perform

     tasks

     that

     typically

     require

     human

     intelligence

    ,

     such

     as

     recognizing

     patterns

    ,

     understanding

     language

    ,

     and

     making

     decisions

    .
    


    2

    .

     Cognitive

     Computing

    :

     This

     is

     an

     area

     of

     AI

     that

     focuses

     on

     developing

     software

     systems

     that

     can

     learn

     and

     adapt

     from

     data

    ,

     rather

     than

     just

     follow

     pre

    -program

    med

     rules

    .
    


    3

    .

     Neural

     Networks

    :

     Neural

     networks

     are

     a

     key

     technology

     in

     AI

    ,

     and

     they

     are

     likely

     to

     continue

     to

     improve

     as

     researchers

     develop

     new

     algorithms

     and

     architectures

    .
    


    4

    .

     Edge

     Computing

    



```python
llm.shutdown()
```
