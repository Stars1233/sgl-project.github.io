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


    2026-04-02 16:50:05.985 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 16:50:05] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 16:50:05.985 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 16:50:05] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 16:50:05.985 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 16:50:05] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 16:50:05.985 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 16:50:05] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 16:50:05.985 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 16:50:05] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  2.97it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  2.97it/s]


    2026-04-02 16:50:10,979 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 16:50:10] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:37,  2.77s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:37,  2.77s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<01:07,  1.21s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<01:07,  1.21s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<01:07,  1.21s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:26,  2.05it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:26,  2.05it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:03<00:26,  2.05it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:03<00:14,  3.55it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:03<00:14,  3.55it/s]Compiling num tokens (num_tokens=4608):  10%|█         | 6/58 [00:03<00:14,  3.55it/s]Compiling num tokens (num_tokens=4096):  10%|█         | 6/58 [00:03<00:14,  3.55it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:03<00:07,  6.25it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:03<00:07,  6.25it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:03<00:07,  6.25it/s]Compiling num tokens (num_tokens=3328):  16%|█▌        | 9/58 [00:03<00:07,  6.25it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:03<00:04,  9.31it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:03<00:04,  9.31it/s]Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:03<00:04,  9.31it/s]Compiling num tokens (num_tokens=2560):  21%|██        | 12/58 [00:03<00:04,  9.31it/s]

    Compiling num tokens (num_tokens=2304):  21%|██        | 12/58 [00:03<00:04,  9.31it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:03<00:03, 13.54it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:03<00:03, 13.54it/s]Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:03<00:03, 13.54it/s]Compiling num tokens (num_tokens=1536):  28%|██▊       | 16/58 [00:03<00:03, 13.54it/s]Compiling num tokens (num_tokens=1280):  28%|██▊       | 16/58 [00:03<00:03, 13.54it/s]Compiling num tokens (num_tokens=1024):  28%|██▊       | 16/58 [00:03<00:03, 13.54it/s]Compiling num tokens (num_tokens=960):  28%|██▊       | 16/58 [00:03<00:03, 13.54it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:03<00:01, 21.30it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:03<00:01, 21.30it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:03<00:01, 21.30it/s]Compiling num tokens (num_tokens=768):  38%|███▊      | 22/58 [00:03<00:01, 21.30it/s]Compiling num tokens (num_tokens=704):  38%|███▊      | 22/58 [00:03<00:01, 21.30it/s]Compiling num tokens (num_tokens=640):  38%|███▊      | 22/58 [00:03<00:01, 21.30it/s]

    Compiling num tokens (num_tokens=576):  38%|███▊      | 22/58 [00:03<00:01, 21.30it/s]Compiling num tokens (num_tokens=512):  38%|███▊      | 22/58 [00:03<00:01, 21.30it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:03<00:00, 30.31it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:03<00:00, 30.31it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:03<00:00, 30.31it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:03<00:00, 30.31it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:03<00:00, 30.31it/s]Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:03<00:00, 30.31it/s]Compiling num tokens (num_tokens=320):  50%|█████     | 29/58 [00:03<00:00, 30.31it/s]Compiling num tokens (num_tokens=288):  50%|█████     | 29/58 [00:03<00:00, 30.31it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:03<00:00, 37.94it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:03<00:00, 37.94it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:03<00:00, 37.94it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:03<00:00, 37.94it/s]

    Compiling num tokens (num_tokens=208):  62%|██████▏   | 36/58 [00:03<00:00, 37.94it/s]Compiling num tokens (num_tokens=192):  62%|██████▏   | 36/58 [00:03<00:00, 37.94it/s]Compiling num tokens (num_tokens=176):  62%|██████▏   | 36/58 [00:03<00:00, 37.94it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:03<00:00, 41.76it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:03<00:00, 41.76it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:03<00:00, 41.76it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:03<00:00, 41.76it/s]Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:03<00:00, 41.76it/s]Compiling num tokens (num_tokens=96):  72%|███████▏  | 42/58 [00:03<00:00, 41.76it/s] Compiling num tokens (num_tokens=80):  72%|███████▏  | 42/58 [00:04<00:00, 41.76it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:04<00:00, 43.61it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:04<00:00, 43.61it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:04<00:00, 43.61it/s]

    Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:04<00:00, 43.61it/s]Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:04<00:00, 43.61it/s]Compiling num tokens (num_tokens=24):  83%|████████▎ | 48/58 [00:04<00:00, 43.61it/s]Compiling num tokens (num_tokens=20):  83%|████████▎ | 48/58 [00:04<00:00, 43.61it/s]Compiling num tokens (num_tokens=16):  83%|████████▎ | 48/58 [00:04<00:00, 43.61it/s]Compiling num tokens (num_tokens=16):  95%|█████████▍| 55/58 [00:04<00:00, 49.80it/s]Compiling num tokens (num_tokens=12):  95%|█████████▍| 55/58 [00:04<00:00, 49.80it/s]Compiling num tokens (num_tokens=8):  95%|█████████▍| 55/58 [00:04<00:00, 49.80it/s] Compiling num tokens (num_tokens=4):  95%|█████████▍| 55/58 [00:04<00:00, 49.80it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 13.94it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=120.33 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.30 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:02, 18.80it/s]Capturing num tokens (num_tokens=7168 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:02, 18.80it/s]Capturing num tokens (num_tokens=6656 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:02, 18.80it/s]Capturing num tokens (num_tokens=6144 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:02, 18.80it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=120.30 GB):   9%|▊         | 5/58 [00:00<00:02, 21.85it/s]Capturing num tokens (num_tokens=5632 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 21.85it/s]Capturing num tokens (num_tokens=5120 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 21.85it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 21.85it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 21.85it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.29 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.54it/s]Capturing num tokens (num_tokens=3840 avail_mem=120.29 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.54it/s]Capturing num tokens (num_tokens=3584 avail_mem=120.28 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.54it/s]Capturing num tokens (num_tokens=3328 avail_mem=120.28 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.54it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=120.27 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.54it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.27 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.54it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.27 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.31it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.27 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.31it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.27 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.31it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.26 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.31it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.26 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.31it/s]Capturing num tokens (num_tokens=1536 avail_mem=120.26 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.31it/s]Capturing num tokens (num_tokens=1536 avail_mem=120.26 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.43it/s]Capturing num tokens (num_tokens=1280 avail_mem=120.25 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.43it/s]Capturing num tokens (num_tokens=1024 avail_mem=120.23 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.43it/s]

    Capturing num tokens (num_tokens=960 avail_mem=120.25 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.43it/s] Capturing num tokens (num_tokens=896 avail_mem=120.24 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.43it/s]Capturing num tokens (num_tokens=832 avail_mem=120.24 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.43it/s]Capturing num tokens (num_tokens=832 avail_mem=120.24 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.25it/s]Capturing num tokens (num_tokens=768 avail_mem=120.24 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.25it/s]Capturing num tokens (num_tokens=704 avail_mem=120.23 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.25it/s]Capturing num tokens (num_tokens=640 avail_mem=120.23 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.25it/s]Capturing num tokens (num_tokens=576 avail_mem=120.23 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.25it/s]Capturing num tokens (num_tokens=512 avail_mem=120.22 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.25it/s]Capturing num tokens (num_tokens=512 avail_mem=120.22 GB):  50%|█████     | 29/58 [00:00<00:00, 41.06it/s]Capturing num tokens (num_tokens=480 avail_mem=120.23 GB):  50%|█████     | 29/58 [00:00<00:00, 41.06it/s]Capturing num tokens (num_tokens=448 avail_mem=120.23 GB):  50%|█████     | 29/58 [00:00<00:00, 41.06it/s]

    Capturing num tokens (num_tokens=416 avail_mem=120.23 GB):  50%|█████     | 29/58 [00:00<00:00, 41.06it/s]Capturing num tokens (num_tokens=384 avail_mem=120.23 GB):  50%|█████     | 29/58 [00:00<00:00, 41.06it/s]Capturing num tokens (num_tokens=352 avail_mem=120.22 GB):  50%|█████     | 29/58 [00:00<00:00, 41.06it/s]Capturing num tokens (num_tokens=352 avail_mem=120.22 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.40it/s]Capturing num tokens (num_tokens=320 avail_mem=120.22 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.40it/s]Capturing num tokens (num_tokens=288 avail_mem=120.21 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.40it/s]Capturing num tokens (num_tokens=256 avail_mem=120.21 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.40it/s]Capturing num tokens (num_tokens=240 avail_mem=120.21 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.40it/s]Capturing num tokens (num_tokens=224 avail_mem=120.21 GB):  59%|█████▊    | 34/58 [00:01<00:00, 42.40it/s]Capturing num tokens (num_tokens=224 avail_mem=120.21 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.18it/s]Capturing num tokens (num_tokens=208 avail_mem=120.20 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.18it/s]Capturing num tokens (num_tokens=192 avail_mem=120.20 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.18it/s]

    Capturing num tokens (num_tokens=176 avail_mem=120.20 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.18it/s]Capturing num tokens (num_tokens=160 avail_mem=120.19 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.18it/s]Capturing num tokens (num_tokens=144 avail_mem=120.19 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.18it/s]Capturing num tokens (num_tokens=144 avail_mem=120.19 GB):  76%|███████▌  | 44/58 [00:01<00:00, 43.88it/s]Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  76%|███████▌  | 44/58 [00:01<00:00, 43.88it/s]Capturing num tokens (num_tokens=112 avail_mem=120.19 GB):  76%|███████▌  | 44/58 [00:01<00:00, 43.88it/s]Capturing num tokens (num_tokens=96 avail_mem=120.18 GB):  76%|███████▌  | 44/58 [00:01<00:00, 43.88it/s] Capturing num tokens (num_tokens=80 avail_mem=120.18 GB):  76%|███████▌  | 44/58 [00:01<00:00, 43.88it/s]Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  76%|███████▌  | 44/58 [00:01<00:00, 43.88it/s]Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.18it/s]Capturing num tokens (num_tokens=48 avail_mem=120.17 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.18it/s]

    Capturing num tokens (num_tokens=32 avail_mem=120.17 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.18it/s]Capturing num tokens (num_tokens=28 avail_mem=120.16 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.18it/s]Capturing num tokens (num_tokens=24 avail_mem=120.16 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.18it/s]Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.18it/s]Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  93%|█████████▎| 54/58 [00:01<00:00, 44.69it/s]Capturing num tokens (num_tokens=16 avail_mem=120.20 GB):  93%|█████████▎| 54/58 [00:01<00:00, 44.69it/s]Capturing num tokens (num_tokens=12 avail_mem=120.19 GB):  93%|█████████▎| 54/58 [00:01<00:00, 44.69it/s]Capturing num tokens (num_tokens=8 avail_mem=120.19 GB):  93%|█████████▎| 54/58 [00:01<00:00, 44.69it/s] Capturing num tokens (num_tokens=4 avail_mem=120.19 GB):  93%|█████████▎| 54/58 [00:01<00:00, 44.69it/s]Capturing num tokens (num_tokens=4 avail_mem=120.19 GB): 100%|██████████| 58/58 [00:01<00:00, 39.45it/s]


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
    Generated text:  Daniel. I am 21 years old and I am from the United States. I have a part-time job in a restaurant, working as a waiter for about a year. After I finish school, I would like to go to college and I want to major in business. I am a good student and like to talk to people and be friendly. I like to eat at restaurants and I also like to spend time in the mall. What are some ideas for my resume and cover letter? What should I include in my resume and cover letter?
    
    I'm really happy that I have this information, thank you for your time and help.
    ===============================
    Prompt: The president of the United States is
    Generated text:  trying to finalize a bill to address global poverty. The bill must be passed by a two-thirds majority vote. If there are 100 candidates for the positions and the president wants to ensure that at least 50 candidates are on the bill, what is the probability that the president will pass the bill if each candidate's vote is independent and the president must vote for at least one candidate to ensure the bill is passed?
    To determine the probability that the president will pass the bill, we need to follow these steps:
    
    1. **Identify the total number of candidates**: There are 100 candidates for the positions.
    2
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris, an ancient city in the northwestern part of the country. It was founded by the Romans and renamed Paris in 793 AD by the French Emperor Charlemagne. The region was made a duchy of France in 1380, and was given to the Duchy of Paris in 1481. The ancient city of Paris has been an important cultural and historical center of France since the time of the Roman Empire. Paris is often referred to as the "City of Love" and has been the location of the Marlee Matlin voice and the voice of "Blindspot", the fictional
    ===============================
    Prompt: The future of AI is
    Generated text:  fast approaching and in the next few years, people will be using AI in a multitude of ways. It will be used to make your life more efficient, cut costs, and increase productivity. In the meantime, though, there’s a wide range of ethical and societal concerns regarding AI and how it should be used.
    There’s a lot of debate about how to regulate and manage the growing use of AI. How can we ensure that people are using AI in the most beneficial way possible? One solution is to increase transparency and accountability in the development and deployment of AI.
    AI is a complex technology that can have serious ethical implications, especially in terms


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


    Generated text:  [Name], and I'm a [Job Title] at [Company Name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [Type of Person] who is [What you do for a living]. I enjoy [Why you do this job]. I'm always looking for new challenges and opportunities to grow and learn. What's your favorite hobby or activity? I enjoy [What you like to do]. I'm always looking for new ways to challenge myself and expand my horizons. What's your favorite book or movie? I love [Name of Book/Movie]. It
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris. It is the largest city in France and the second-largest city in the European Union. It is located on the Seine River and is the seat of government, administration, and culture for the French Republic. Paris is known for its rich history, art, and cuisine, and is a popular tourist destination. It is also home to the Eiffel Tower, the Louvre Museum, and the Notre-Dame Cathedral. Paris is a major cultural and economic center in Europe and plays a significant role in French politics and society. It is also a major tourist destination, attracting millions of visitors each year. The city is home to many
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by rapid advancements in several key areas, including:
    
    1. Increased integration with human intelligence: AI systems are likely to become more integrated with human intelligence, allowing them to learn from and adapt to human behavior and decision-making processes.
    
    2. Enhanced capabilities in natural language processing: AI systems are likely to become more capable of understanding and generating human-like language, allowing for more natural and intuitive interactions with humans.
    
    3. Greater emphasis on ethical considerations: As AI systems become more integrated with human intelligence, there will be a greater emphasis on ethical considerations and responsible development.
    
    4. Increased use of AI in healthcare: AI is likely to
    


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
    Generated text:  [Name] and I'm a [ occupation ]! I'm a [ age ] year-old who has always loved [ career or hobby ] and dreams of [ long-term goal ]! I've been [ short time in the workforce ], but I'm now excited to [ first job, project, or challenge ] and look forward to [ last but not least, where are you planning to go next? ]. I'm here to [ tell someone about yourself or something that happened recently ]! What can you tell me about yourself, and how can someone get to know you better? [ tell me something about yourself, like your personality, interests
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    
    What is the capital of France? Paris.
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  full of possibilities, and as technology continues to advance, we can expect to see many exciting developments. Here are some of the most likely future trends in AI:
    
    1. Increased integration with human decision-making: As AI becomes more advanced, it will become more integrated with human decision-making, leading to more accurate and reliable predictions. This integration will allow AI to make decisions that are more aligned with human values and principles, and to make better-informed decisions in areas like healthcare, finance, and law enforcement.
    
    2. AI becoming more natural: As AI becomes more sophisticated, we will see more AI systems that are more natural and human-like in


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

     a

     [

    occupation

    ]

    !
    


    As

     you

     know

    ,

     I

     have

     a

     passion

     for

     [

    career

    ],

     and

     my

     goal

     is

     to

     [

    objective

    ].

     I

    'm

     always

     looking

     for

     new

     experiences

     and

     opportunities

     to

     grow

     and

     learn

    .
    


    I

    've

     been

     [

    number

     of

     years

     since

     graduation

     or

     last

     year

    ],

     and

     during

     that

     time

    ,

     I

    've

     [

    sum

    mar

    ize

     accomplishments

     or

     experiences

    ].

     I

    've

     always

     been

     [

    type

     of

     person

    ],

     and

     I

    'm

     always

     looking

     for

     opportunities

     to

     share

     my

     knowledge

     and

     skills

     with

     others

    .
    


    In

     terms

     of

     [

    occupation

    ],

     I

     believe

     that

     [

    reason

     for

     being

     in

     this

     field

    ].

     I

    'm

     always

     eager

     to

     learn

     and

     grow

    ,

     and

     I

    'm

     always

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     The

     city

     is

     home

     to

     the

     E

    iff

    el

     Tower

    ,

     one

     of

     the

     most

     famous

     landmarks

     in

     the

     world

    .

     It

     is

     a

     popular

     tourist

     destination

    ,

     with

     many

     museums

    ,

     cafes

    ,

     and

     shopping

     venues

    .

     The

     city

     is

     also

     known

     for

     its

     rich

     history

    ,

     including

     the

     medieval

     ramp

    arts

    ,

     Renaissance

     architecture

    ,

     and

     the

     influence

     of

     various

     cultures

     throughout

     history

    .

     Paris

     is

     a

     city

     of

     art

    ,

     culture

    ,

     and

     innovation

    ,

     attracting

     visitors

     from

     all

     over

     the

     world

    .

     It

     is

     a

     cultural

     and

     intellectual

     hub

     that

     has

     played

     a

     significant

     role

     in

     the

     development

     of

     modern

     France

    .

     According

     to

     the

     Paris

     Chamber

     of

     Commerce

     and

     Industry

    ,

     the

     city

     has

     a

     total

     population

     of

     approximately

     

    1

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     quite

     exciting

    ,

     with

     many

     possibilities

     and

     advancements

     shaping

     the

     technology

    .

     Here

     are

     some

     potential

     trends

     that

     could

     shape

     AI

     in

     the

     next

     few

     decades

    :
    


    1

    .

     Increased

     Integration

    :

     AI

     is

     already

     becoming

     more

     integrated

     into

     our

     daily

     lives

    ,

     but

     there

     is

     potential

     for

     further

     integration

     of

     AI

     into

     all

     areas

     of

     life

    ,

     from

     healthcare

     to

     transportation

     to

     finance

    .

     This

     could

     lead

     to

     even

     more

     sophisticated

     AI

     systems

     that

     can

     help

     solve

     complex

     problems

     and

     make

     informed

     decisions

    .
    


    2

    .

     Human

    -A

    I

     Collaboration

    :

     The

     future

     of

     AI

     will

     likely

     see

     more

     collaboration

     between

     humans

     and

     AI

    .

     This

     could

     mean

     that

     AI

     systems

     will

     be

     able

     to

     learn

     from

     human

     feedback

     and

     improve

     their

     performance

     over

     time

    .

     It

    



```python
llm.shutdown()
```
