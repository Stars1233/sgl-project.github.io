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


    2026-04-01 13:54:10.782 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 13:54:10] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 13:54:10.782 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 13:54:10] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 13:54:10.782 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 13:54:10] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 13:54:10.782 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 13:54:10] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 13:54:10.782 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 13:54:10] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.16it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.15it/s]


    2026-04-01 13:54:13,931 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 13:54:13] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:35,  2.73s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:35,  2.73s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:35,  2.73s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:35,  2.73s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:29,  1.84it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:29,  1.84it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:29,  1.84it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:29,  1.84it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:29,  1.84it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:29,  1.84it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:29,  1.84it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.65it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.65it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.65it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.65it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:08,  5.65it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:08,  5.65it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:03<00:08,  5.65it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:03<00:08,  5.65it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:03<00:08,  5.65it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:03, 11.96it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:03, 11.96it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:03, 11.96it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:03, 11.96it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:03, 11.96it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:03<00:03, 11.96it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:03<00:03, 11.96it/s]

    Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:03<00:03, 11.96it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 18.00it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 18.00it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 18.00it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 18.00it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 18.00it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 18.00it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 18.00it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:03<00:01, 18.00it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 24.65it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 24.65it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 24.65it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 24.65it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 24.65it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 24.65it/s]

    Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 24.65it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 29.50it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 29.50it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 29.50it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 29.50it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 29.50it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 29.50it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 29.50it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 34.24it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 34.24it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 34.24it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 34.24it/s] 

    Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 34.24it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 34.24it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 34.24it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 36.80it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 36.80it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 36.80it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 36.80it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 36.80it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 36.80it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 36.80it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 36.80it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 36.80it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 45.36it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.49it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=120.34 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.31 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.31 GB):   3%|▎         | 2/58 [00:00<00:03, 18.23it/s]Capturing num tokens (num_tokens=7168 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 18.23it/s]Capturing num tokens (num_tokens=6656 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 18.23it/s]Capturing num tokens (num_tokens=6144 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 18.23it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=120.30 GB):   9%|▊         | 5/58 [00:00<00:02, 21.21it/s]Capturing num tokens (num_tokens=5632 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 21.21it/s]Capturing num tokens (num_tokens=5120 avail_mem=120.30 GB):   9%|▊         | 5/58 [00:00<00:02, 21.21it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 21.21it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.29 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.24it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.29 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.24it/s]Capturing num tokens (num_tokens=3840 avail_mem=120.29 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.24it/s]Capturing num tokens (num_tokens=3584 avail_mem=120.28 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.24it/s]Capturing num tokens (num_tokens=3328 avail_mem=120.28 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.24it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=120.28 GB):  21%|██        | 12/58 [00:00<00:01, 29.34it/s]Capturing num tokens (num_tokens=3072 avail_mem=120.27 GB):  21%|██        | 12/58 [00:00<00:01, 29.34it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.27 GB):  21%|██        | 12/58 [00:00<00:01, 29.34it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.27 GB):  21%|██        | 12/58 [00:00<00:01, 29.34it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.27 GB):  21%|██        | 12/58 [00:00<00:01, 29.34it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.26 GB):  21%|██        | 12/58 [00:00<00:01, 29.34it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.26 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.93it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.26 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.93it/s]Capturing num tokens (num_tokens=1536 avail_mem=120.26 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.93it/s]Capturing num tokens (num_tokens=1280 avail_mem=120.25 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.93it/s]Capturing num tokens (num_tokens=1024 avail_mem=120.23 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.93it/s]

    Capturing num tokens (num_tokens=960 avail_mem=120.25 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.93it/s] Capturing num tokens (num_tokens=960 avail_mem=120.25 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.56it/s]Capturing num tokens (num_tokens=896 avail_mem=120.24 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.56it/s]Capturing num tokens (num_tokens=832 avail_mem=120.24 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.56it/s]Capturing num tokens (num_tokens=768 avail_mem=120.24 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.56it/s]Capturing num tokens (num_tokens=704 avail_mem=120.23 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.56it/s]Capturing num tokens (num_tokens=640 avail_mem=120.23 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.56it/s]Capturing num tokens (num_tokens=640 avail_mem=120.23 GB):  47%|████▋     | 27/58 [00:00<00:00, 39.53it/s]Capturing num tokens (num_tokens=576 avail_mem=120.23 GB):  47%|████▋     | 27/58 [00:00<00:00, 39.53it/s]Capturing num tokens (num_tokens=512 avail_mem=120.22 GB):  47%|████▋     | 27/58 [00:00<00:00, 39.53it/s]Capturing num tokens (num_tokens=480 avail_mem=120.23 GB):  47%|████▋     | 27/58 [00:00<00:00, 39.53it/s]

    Capturing num tokens (num_tokens=448 avail_mem=120.23 GB):  47%|████▋     | 27/58 [00:00<00:00, 39.53it/s]Capturing num tokens (num_tokens=416 avail_mem=120.23 GB):  47%|████▋     | 27/58 [00:00<00:00, 39.53it/s]Capturing num tokens (num_tokens=416 avail_mem=120.23 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.22it/s]Capturing num tokens (num_tokens=384 avail_mem=120.23 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.22it/s]Capturing num tokens (num_tokens=352 avail_mem=120.22 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.22it/s]Capturing num tokens (num_tokens=320 avail_mem=120.22 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.22it/s]Capturing num tokens (num_tokens=288 avail_mem=120.21 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.22it/s]Capturing num tokens (num_tokens=256 avail_mem=120.21 GB):  55%|█████▌    | 32/58 [00:01<00:00, 41.22it/s]Capturing num tokens (num_tokens=256 avail_mem=120.21 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.27it/s]Capturing num tokens (num_tokens=240 avail_mem=120.21 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.27it/s]Capturing num tokens (num_tokens=224 avail_mem=120.21 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.27it/s]

    Capturing num tokens (num_tokens=208 avail_mem=120.20 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.27it/s]Capturing num tokens (num_tokens=192 avail_mem=120.20 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.27it/s]Capturing num tokens (num_tokens=176 avail_mem=120.20 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.27it/s]Capturing num tokens (num_tokens=176 avail_mem=120.20 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.23it/s]Capturing num tokens (num_tokens=160 avail_mem=120.19 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.23it/s]Capturing num tokens (num_tokens=144 avail_mem=120.19 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.23it/s]Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.23it/s]Capturing num tokens (num_tokens=112 avail_mem=120.19 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.23it/s]Capturing num tokens (num_tokens=96 avail_mem=120.18 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.23it/s] Capturing num tokens (num_tokens=96 avail_mem=120.18 GB):  81%|████████  | 47/58 [00:01<00:00, 43.47it/s]Capturing num tokens (num_tokens=80 avail_mem=120.18 GB):  81%|████████  | 47/58 [00:01<00:00, 43.47it/s]

    Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  81%|████████  | 47/58 [00:01<00:00, 43.47it/s]Capturing num tokens (num_tokens=48 avail_mem=120.17 GB):  81%|████████  | 47/58 [00:01<00:00, 43.47it/s]Capturing num tokens (num_tokens=32 avail_mem=120.17 GB):  81%|████████  | 47/58 [00:01<00:00, 43.47it/s]Capturing num tokens (num_tokens=28 avail_mem=120.16 GB):  81%|████████  | 47/58 [00:01<00:00, 43.47it/s]Capturing num tokens (num_tokens=28 avail_mem=120.16 GB):  90%|████████▉ | 52/58 [00:01<00:00, 43.79it/s]Capturing num tokens (num_tokens=24 avail_mem=120.16 GB):  90%|████████▉ | 52/58 [00:01<00:00, 43.79it/s]Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  90%|████████▉ | 52/58 [00:01<00:00, 43.79it/s]Capturing num tokens (num_tokens=16 avail_mem=120.16 GB):  90%|████████▉ | 52/58 [00:01<00:00, 43.79it/s]Capturing num tokens (num_tokens=12 avail_mem=120.15 GB):  90%|████████▉ | 52/58 [00:01<00:00, 43.79it/s]Capturing num tokens (num_tokens=8 avail_mem=120.15 GB):  90%|████████▉ | 52/58 [00:01<00:00, 43.79it/s] Capturing num tokens (num_tokens=8 avail_mem=120.15 GB):  98%|█████████▊| 57/58 [00:01<00:00, 44.42it/s]Capturing num tokens (num_tokens=4 avail_mem=120.15 GB):  98%|█████████▊| 57/58 [00:01<00:00, 44.42it/s]

    Capturing num tokens (num_tokens=4 avail_mem=120.15 GB): 100%|██████████| 58/58 [00:01<00:00, 38.92it/s]


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
    Generated text:  Caroline, I am a lawyer and the managing director of a law firm in Sydney, Australia. My firm has a reputation for providing high-quality, sensitive and innovative legal services to a wide range of clients.
    As a lawyer and a practicing attorney, I have worked in the legal industry for over 20 years. I have a significant number of years of experience in the area of intellectual property law.
    I have been a practicing attorney in Sydney since 1993, my first case is a patent application. I have been in practice for over 10 years, and my areas of expertise include intellectual property law, copyright law,
    ===============================
    Prompt: The president of the United States is
    Generated text:  a very important person. He or she has a very important job. The president serves for a very long time, and it can take him or her many years to become president. The president is the leader of the country. He or she is the president of the United States. All of the people of the country elect the president to be the leader of the country. There are 5 members in the executive branch (the branch of the government that deals with day-to-day functions). The vice president is always the president's choice. The president is not allowed to be married. In the past, many presidents have not been married. Usually
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. In the city, the most popular street is the Champs-Élysées. You may use the Internet to find out more about this famous street. After a long walk in Paris, you may like to go back home. But before you leave, you may like to know some of the information about the Champs-Élysées. Many people like to walk on the Champs-Élysées. Many people like to eat there. Many people like to drink there. Some people like to listen to music there. Some people like to read books there. Some people like to go shopping there. One day, the
    ===============================
    Prompt: The future of AI is
    Generated text:  here. But the path to achieving this promise is fraught with peril. According to a recent report, 20% of all AI systems that are deployed today are currently in use in certain industries, which include healthcare, transportation, and finance. The rapid adoption of AI in these industries has led to a rapid growth of the AI market, with the global AI market growing at a compound annual growth rate of 44.8% from 2019 to 2021. By 2025, it is expected that the global AI market will reach $470 billion. As AI technology continues to


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


    Generated text:  Paris, also known as the City of Light. It is a historic city with a rich history dating back to the Roman Empire and the Middle Ages. Paris is known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. The city is also famous for its cuisine, fashion, and art scene. Paris is a major tourist destination and a cultural hub, with many world-renowned museums, theaters, and art galleries. It is also home to the French Parliament and the French Parliament building. Paris is a vibrant and dynamic city with a rich history and a diverse population. The city is
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by rapid advancements in areas such as machine learning, natural language processing, and computer vision. Some potential future trends include:
    
    1. Increased integration of AI into everyday life: AI is already being integrated into our daily lives, from voice assistants like Siri and Alexa to self-driving cars. As AI becomes more integrated into our daily lives, we may see even more widespread adoption of AI in areas such as healthcare, finance, and transportation.
    
    2. Greater emphasis on ethical and responsible AI: As AI becomes more integrated into our daily lives, there will be a greater emphasis on ensuring that AI is used ethically and responsibly. This
    


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
    Generated text:  [Name], and I am a [Type] of [Occupation]. I'm currently [Current Position] and I've been working with this company for [Number of Years] years. I'm excited to meet you and discuss my potential work opportunities with you. What can you tell me about yourself? As an AI language model, I don't have personal experiences or emotions, but I can help you brainstorm ideas and provide information on the latest trends and technologies in the field of [Technology/Industry]. Can you tell me a bit more about yourself? As an AI language model, I don't have personal experiences or emotions, but I
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, which is known for its rich history, beautiful architecture, and vibrant cultural scene. It was founded as a French fishing village in 987 and has become a major cultural hub in Europe. Paris has over 7.5 million people and is home to many iconic landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. The city is also known for its cuisine, fashion, and music scene. Paris has a rich history and culture that makes it a popular tourist destination and a must-visit destination for French visitors. Paris has a long and storied history that dates back to ancient times
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  predicted to evolve rapidly, with new technologies and applications emerging constantly. Some of the possible future trends in AI include:
    
    1. Increased Transparency: As AI systems become more sophisticated, we can expect a shift towards greater transparency. This means that users and developers will be able to more easily understand how the system works and the decisions it makes, leading to greater trust and confidence in the technology.
    
    2. Enhanced Personalization: AI is already being used to provide more personalized experiences, and we can expect this trend to continue. AI will be used to analyze user behavior, preferences, and interests to provide more relevant and effective recommendations.
    
    3. Autonomous Systems


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

    specific

     position

     or

     profession

    ]

     with

     experience

     in

     [

    specific

     skill

     or

     role

    ].

     I

     am

     a

     [

    skill

     or

     role

    ]

     and

     I

     enjoy

     [

    reason

     for

     my

     interest

     in

     this

     role

    ].

     I

     am

     passionate

     about

     [

    reason

     for

     my

     interest

     in

     this

     role

    ].

     I

     am

     always

     ready

     to

     help

    ,

     whether

     it

    's

     a

     simple

     task

     or

     a

     difficult

     one

    .

     I

     am

     dedicated

     to

     learning

     new

     things

     and

     staying

     up

    -to

    -date

     with

     my

     field

    .

     I

     am

     always

     seeking

     ways

     to

     grow

     and

     improve

     myself

    ,

     and

     I

     am

     eager

     to

     make

     a

     positive

     impact

     on

     the

     world

    .

     Thank

     you

     for

     considering

     me

     as

     a

     potential

     match

    !

     How

     can

     I

     assist

     you

    ?

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .
    


    Does

     this

     next

     sentence

     follow

    ,

     given

     the

     above

     sentence

    ?

     "

    Paris

     is

     a

     city

    ."

    ?
    


    OPTIONS

    :


     A

    ).

     yes

    .


     B

    ).

     no

    .


    A

    ).

     yes

    .

     
    


    The

     sentence

     "

    Paris

     is

     a

     city

    "

     does

     follow

     logically

     from

     the

     given

     sentence

     "

    The

     capital

     of

     France

     is

     Paris

    ."

     This

     is

     because

     Paris

     is

     indeed

     a

     city

    ,

     and

     the

     fact

     that

     it

     is

     a

     capital

     city

     of

     France

     is

     part

     of

     the

     definition

     of

     a

     city

    .

     "

    Paris

     is

     a

     city

    "

     is

     a

     direct

     and

     accurate

     statement

     about

     the

     status

     of

     Paris

     as

     a

     city

    .

     Thus

    ,

     it

     makes

     sense

     to

     conclude

     that

     Paris

     is

     a

     city

     from

     the

     given

     statement

    .

     However

    ,

     it

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     undoubtedly

     set

     to

     be

     one

     of

     tremendous

     growth

     and

     innovation

    ,

     with

     many

     potential

     paths

     and

     possible

     technologies

     that

     could

     lead

     to

     a

     transformative

     shift

     in

     our

     world

    .

     Here

     are

     some

     of

     the

     possible

     trends

     that

     could

     be

     expected

     in

     the

     AI

     industry

     over

     the

     next

     few

     decades

    :
    


    1

    .

     Increased

     focus

     on

     ethical

     and

     social

     implications

    :

     With

     the

     rise

     of

     ethical

     issues

     in

     the

     AI

     industry

    ,

     the

     focus

     will

     shift

     from

     purely

     technical

     capabilities

     to

     more

     complex

     considerations

     of

     the

     potential

     impact

     of

     AI

     on

     society

     and

     individuals

    .

     This

     could

     lead

     to

     a

     greater

     emphasis

     on

     developing

     AI

     that

     is

     designed

     to

     be

     socially

     responsible

     and

     consider

    ate

     of

     diverse

     societal

     groups

     and

     communities

    .
    


    2

    .

     Increased

     use

     of

     AI

     for

    



```python
llm.shutdown()
```
