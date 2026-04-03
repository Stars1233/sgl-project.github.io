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


    2026-04-03 13:34:26.107 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 13:34:26] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 13:34:26.107 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 13:34:26] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 13:34:26.107 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 13:34:26] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 13:34:26.107 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 13:34:26] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 13:34:26.107 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 13:34:26] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.16it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.15it/s]


    2026-04-03 13:34:29,126 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 13:34:29] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:34,  2.72s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:34,  2.72s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:34,  2.72s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:34,  2.72s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:34,  2.72s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:22,  2.33it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:22,  2.33it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:22,  2.33it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:22,  2.33it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:22,  2.33it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:22,  2.33it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:22,  2.33it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:07,  6.12it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:07,  6.12it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:07,  6.12it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:07,  6.12it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:07,  6.12it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:07,  6.12it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:03<00:07,  6.12it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:03<00:07,  6.12it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:03<00:07,  6.12it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:03, 12.39it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:03, 12.39it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:03, 12.39it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:03, 12.39it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:03, 12.39it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:03, 12.39it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:03, 12.39it/s]

    Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:03, 12.39it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 18.44it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 18.44it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 18.44it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 18.44it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 18.44it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 18.44it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:03<00:01, 18.44it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:03<00:01, 18.44it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:00, 25.05it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:00, 25.05it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:00, 25.05it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:00, 25.05it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:00, 25.05it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:00, 25.05it/s]

    Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:03<00:00, 25.05it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 30.10it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 30.10it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 30.10it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 30.10it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 30.10it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 30.10it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 30.10it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 35.01it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 35.01it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 35.01it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 35.01it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 35.01it/s]

    Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 35.01it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 35.01it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 39.14it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 39.14it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 39.14it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 39.14it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 39.14it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 39.14it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 39.14it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 39.14it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.76it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=120.76 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.73 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:03, 18.15it/s]Capturing num tokens (num_tokens=7168 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:03, 18.15it/s]Capturing num tokens (num_tokens=6656 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:03, 18.15it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:03, 18.15it/s]Capturing num tokens (num_tokens=6144 avail_mem=120.73 GB):   9%|▊         | 5/58 [00:00<00:02, 20.53it/s]Capturing num tokens (num_tokens=5632 avail_mem=120.72 GB):   9%|▊         | 5/58 [00:00<00:02, 20.53it/s]Capturing num tokens (num_tokens=5120 avail_mem=120.72 GB):   9%|▊         | 5/58 [00:00<00:02, 20.53it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.72 GB):   9%|▊         | 5/58 [00:00<00:02, 20.53it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.72 GB):   9%|▊         | 5/58 [00:00<00:02, 20.53it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.72 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.52it/s]Capturing num tokens (num_tokens=3840 avail_mem=120.72 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.52it/s]Capturing num tokens (num_tokens=3584 avail_mem=120.71 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.52it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=120.71 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.52it/s]Capturing num tokens (num_tokens=3072 avail_mem=120.37 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.52it/s]Capturing num tokens (num_tokens=3072 avail_mem=120.37 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.19it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.28 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.19it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.27 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.19it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.27 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.19it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.27 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.19it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.26 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.19it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.26 GB):  31%|███       | 18/58 [00:00<00:01, 35.08it/s]Capturing num tokens (num_tokens=1536 avail_mem=120.26 GB):  31%|███       | 18/58 [00:00<00:01, 35.08it/s]Capturing num tokens (num_tokens=1280 avail_mem=120.26 GB):  31%|███       | 18/58 [00:00<00:01, 35.08it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=120.24 GB):  31%|███       | 18/58 [00:00<00:01, 35.08it/s]Capturing num tokens (num_tokens=960 avail_mem=120.25 GB):  31%|███       | 18/58 [00:00<00:01, 35.08it/s] Capturing num tokens (num_tokens=896 avail_mem=120.25 GB):  31%|███       | 18/58 [00:00<00:01, 35.08it/s]Capturing num tokens (num_tokens=896 avail_mem=120.25 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.33it/s]Capturing num tokens (num_tokens=832 avail_mem=120.24 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.33it/s]Capturing num tokens (num_tokens=768 avail_mem=120.24 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.33it/s]Capturing num tokens (num_tokens=704 avail_mem=120.24 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.33it/s]Capturing num tokens (num_tokens=640 avail_mem=120.23 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.33it/s]Capturing num tokens (num_tokens=576 avail_mem=120.23 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.33it/s]Capturing num tokens (num_tokens=576 avail_mem=120.23 GB):  48%|████▊     | 28/58 [00:00<00:00, 40.66it/s]Capturing num tokens (num_tokens=512 avail_mem=120.22 GB):  48%|████▊     | 28/58 [00:00<00:00, 40.66it/s]Capturing num tokens (num_tokens=480 avail_mem=120.24 GB):  48%|████▊     | 28/58 [00:00<00:00, 40.66it/s]

    Capturing num tokens (num_tokens=448 avail_mem=120.24 GB):  48%|████▊     | 28/58 [00:00<00:00, 40.66it/s]Capturing num tokens (num_tokens=416 avail_mem=120.23 GB):  48%|████▊     | 28/58 [00:00<00:00, 40.66it/s]Capturing num tokens (num_tokens=384 avail_mem=120.23 GB):  48%|████▊     | 28/58 [00:00<00:00, 40.66it/s]Capturing num tokens (num_tokens=384 avail_mem=120.23 GB):  57%|█████▋    | 33/58 [00:00<00:00, 42.45it/s]Capturing num tokens (num_tokens=352 avail_mem=120.23 GB):  57%|█████▋    | 33/58 [00:00<00:00, 42.45it/s]Capturing num tokens (num_tokens=320 avail_mem=120.22 GB):  57%|█████▋    | 33/58 [00:00<00:00, 42.45it/s]Capturing num tokens (num_tokens=288 avail_mem=120.22 GB):  57%|█████▋    | 33/58 [00:00<00:00, 42.45it/s]Capturing num tokens (num_tokens=256 avail_mem=120.22 GB):  57%|█████▋    | 33/58 [00:00<00:00, 42.45it/s]Capturing num tokens (num_tokens=240 avail_mem=120.21 GB):  57%|█████▋    | 33/58 [00:01<00:00, 42.45it/s]Capturing num tokens (num_tokens=240 avail_mem=120.21 GB):  66%|██████▌   | 38/58 [00:01<00:00, 43.30it/s]Capturing num tokens (num_tokens=224 avail_mem=120.21 GB):  66%|██████▌   | 38/58 [00:01<00:00, 43.30it/s]Capturing num tokens (num_tokens=208 avail_mem=120.20 GB):  66%|██████▌   | 38/58 [00:01<00:00, 43.30it/s]

    Capturing num tokens (num_tokens=192 avail_mem=120.20 GB):  66%|██████▌   | 38/58 [00:01<00:00, 43.30it/s]Capturing num tokens (num_tokens=176 avail_mem=120.20 GB):  66%|██████▌   | 38/58 [00:01<00:00, 43.30it/s]Capturing num tokens (num_tokens=160 avail_mem=120.20 GB):  66%|██████▌   | 38/58 [00:01<00:00, 43.30it/s]Capturing num tokens (num_tokens=160 avail_mem=120.20 GB):  74%|███████▍  | 43/58 [00:01<00:00, 44.23it/s]Capturing num tokens (num_tokens=144 avail_mem=120.19 GB):  74%|███████▍  | 43/58 [00:01<00:00, 44.23it/s]Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  74%|███████▍  | 43/58 [00:01<00:00, 44.23it/s]Capturing num tokens (num_tokens=112 avail_mem=120.19 GB):  74%|███████▍  | 43/58 [00:01<00:00, 44.23it/s]Capturing num tokens (num_tokens=96 avail_mem=120.18 GB):  74%|███████▍  | 43/58 [00:01<00:00, 44.23it/s] Capturing num tokens (num_tokens=80 avail_mem=120.18 GB):  74%|███████▍  | 43/58 [00:01<00:00, 44.23it/s]Capturing num tokens (num_tokens=80 avail_mem=120.18 GB):  83%|████████▎ | 48/58 [00:01<00:00, 44.34it/s]Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  83%|████████▎ | 48/58 [00:01<00:00, 44.34it/s]Capturing num tokens (num_tokens=48 avail_mem=120.17 GB):  83%|████████▎ | 48/58 [00:01<00:00, 44.34it/s]

    Capturing num tokens (num_tokens=32 avail_mem=120.17 GB):  83%|████████▎ | 48/58 [00:01<00:00, 44.34it/s]Capturing num tokens (num_tokens=28 avail_mem=120.16 GB):  83%|████████▎ | 48/58 [00:01<00:00, 44.34it/s]Capturing num tokens (num_tokens=24 avail_mem=120.16 GB):  83%|████████▎ | 48/58 [00:01<00:00, 44.34it/s]Capturing num tokens (num_tokens=24 avail_mem=120.16 GB):  91%|█████████▏| 53/58 [00:01<00:00, 44.92it/s]Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  91%|█████████▏| 53/58 [00:01<00:00, 44.92it/s]Capturing num tokens (num_tokens=16 avail_mem=120.20 GB):  91%|█████████▏| 53/58 [00:01<00:00, 44.92it/s]Capturing num tokens (num_tokens=12 avail_mem=120.19 GB):  91%|█████████▏| 53/58 [00:01<00:00, 44.92it/s]Capturing num tokens (num_tokens=8 avail_mem=120.19 GB):  91%|█████████▏| 53/58 [00:01<00:00, 44.92it/s] Capturing num tokens (num_tokens=4 avail_mem=118.96 GB):  91%|█████████▏| 53/58 [00:01<00:00, 44.92it/s]Capturing num tokens (num_tokens=4 avail_mem=118.96 GB): 100%|██████████| 58/58 [00:01<00:00, 45.47it/s]Capturing num tokens (num_tokens=4 avail_mem=118.96 GB): 100%|██████████| 58/58 [00:01<00:00, 39.59it/s]


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
    Generated text:  Arlette and I am a writer and editor. I’m excited to have you join me on this journey to explore the best of our rich literary heritage. To show you how to approach the world of fiction writing, I’m going to share some of my favorite short stories and poetry. I’ve seen such a wide variety of genres and styles, from romantic love stories to social commentary pieces. But the key to writing great fiction is that you must be true to yourself. I want you to know that as a writer, you need to be honest with yourself about your life, your values, and your beliefs. Then, you need to bring
    ===============================
    Prompt: The president of the United States is
    Generated text:  represented by the Vice President. If the vice president is the President of the Senate and the Vice President is the Chief Justice of the Supreme Court, how many positions are there in total between the president and the vice president? To determine the total number of positions between the president and the vice president, we need to add their respective roles. The president is the President of the United States, and the vice president is the Vice President. Therefore, the total number of positions between the president and the vice president is the sum of their individual roles.
    
    Here are the roles:
    - President of the United States
    - Vice President
    
    So, the total
    ===============================
    Prompt: The capital of France is
    Generated text: :
    A. Paris
    B. Lille
    C. Nice
    D. Dijon
    The capital of France is:
    
    A. Paris
    
    Paris is the capital and largest city of France. It is the 12th largest city in the world by population. The city is located in the western part of the country, on the Mediterranean Sea, and is known for its rich history, art, culture, and cuisine. It is the most populous city in the European Union and is the country's economic and financial center. Paris is home to many famous landmarks, museums, and attractions. While other cities in France, such as
    ===============================
    Prompt: The future of AI is
    Generated text:  promising. In today's world, we face a daunting task to make sure we have the right people to lead in this field. So, what role do they play in the future of AI? We conducted a survey on our website and received feedback from thousands of users. Here are the top three roles that people believe AI will play in the future:
    1. AI will enable new technologies that will revolutionize the way we interact with technology and the world around us.
    2. AI will enable new jobs that require new skills and knowledge.
    3. AI will enable new ways to make our lives more efficient and comfortable.
    These are just a few


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


    Generated text:  [Name] and I am a [Age] year old [Gender] [Occupation]. I am [Occupation] and I have been [Occupation] for [Number of Years] years. I am passionate about [Occupation] and I am always looking for new challenges and opportunities to grow and learn. I am a [Occupation] and I am always looking for new challenges and opportunities to grow and learn. I am a [Occupation] and I am always looking for new challenges and opportunities to grow and learn. I am a [Occupation] and I am always looking for new challenges and opportunities to grow and
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris. It is the largest city in the country and the seat of government and the largest city in the European Union. Paris is known for its rich history, beautiful architecture, and vibrant culture. It is also home to many famous landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. Paris is a popular tourist destination and a major economic center in France. It is also known for its fashion industry, which is one of the largest in the world. The city is home to many important institutions such as the French Academy of Sciences and the French National Library. Paris is a city of contrasts, with
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends, including:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn and adapt to human behavior and preferences.
    
    2. Greater emphasis on ethical considerations: As AI becomes more advanced, there will be a greater emphasis on ethical considerations, including issues such as bias, privacy, and transparency.
    
    3. Increased use of AI in healthcare: AI is likely to play a greater role in healthcare, with machines being used to diagnose and treat diseases, as well as to assist with patient care.
    
    4. Greater focus on AI in education: AI is likely
    


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
    Generated text:  [Your Name]. I am a [Your Occupation] and I am excited to meet you today. I bring a unique perspective to the table, and I have a passion for [Your passion or hobby]. Let's start our conversation by asking you a question that showcases your skills and interests. What brings you to the table today? [Your question]. And what do you think is the most important lesson you have learned so far? [Your answer].
    Your answer should be concise and to the point, and should reflect your unique perspective and personality traits. You should also aim to engage and entertain the reader, whether by sharing a funny anecdote
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. It is known for its vibrant cultural scene, beautiful architecture, and rich history. The city has a population of over 2 million people, making it one of the largest cities in Europe. Paris is also home to numerous art galleries, museums, and theaters, making it a popular tourist destination. The city is known for its exquisite cuisine, including French cuisine, and has a large coffee culture. Paris is a global hub of finance, business, and politics, and is considered one of the most important cities in the world. The city is known for its stunning architecture and has a long and varied history that dates back over 2
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be characterized by several trends, including:
    
      1. Increased focus on ethical considerations: There will be an increasing emphasis on creating AI that is transparent, accountable, and aligned with ethical principles.
      2. Deep learning and neural networks: Deep learning and neural networks will become more prevalent, as they are capable of handling large amounts of data and producing accurate results.
      3. Increased use of AI in healthcare: AI will be increasingly used in healthcare to diagnose and treat diseases, improve patient outcomes, and reduce costs.
      4. Integration of AI with other technologies: AI will be increasingly integrated with other technologies,


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

    Character

    's

     Name

    ],

     and

     I

     am

     [

    Character

    's

     Age

    ].

     I

    've

     always

     been

     fascinated

     by

     the

     intric

    acies

     of

     the

     human

     body

     and

     its

     many

     hidden

     talents

    .

     I

    'm

     a

     tech

     enthusiast

     and

     enjoy

     coding

     in

     languages

     like

     Python

     and

     Swift

    .

     I

    'm

     a

     professional

     who

     enjoys

     working

     on

     challenging

     projects

     and

     learning

     new

     skills

     every

     day

    .

     I

    'm

     a

     leader

     who

     believes

     in

     taking

     risks

     and

     being

     open

     to

     new

     ideas

    .

     I

     enjoy

     creating

     beautiful

     art

     and

     am

     always

     eager

     to

     see

     what

     new

     pieces

     I

     can

     make

    .

     I

    'm

     confident

     in

     my

     abilities

     and

     constantly

     strive

     to

     improve

     my

     skills

    .

     I

    'm

     confident

     in

     my

     abilities

     and

     constantly

     strive

     to

     improve

     my

     skills

    .

     How

     do

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     also

     known

     as

     La

     Gar

    de

    .

     It

     is

     the

     second

     largest

     city

     in

     the

     country

     and

     the

     third

     largest

     in

     Europe

     by

     population

    .

     It

     is

     located

     in

     the

     central

     region

     of

     the

     country

     and

     is

     a

     cultural

     and

     economic

     center

    ,

     particularly

     known

     for

     its

     historical

     landmarks

     and

     museums

    .

     Paris

     is

     famous

     for

     its

     cuisine

    ,

     fashion

    ,

     and

     art

    ,

     and

     it

     is

     the

     birth

    place

     of

     many

     notable

     figures

     such

     as

     Louis

     XIV

    ,

     Vol

    taire

    ,

     and

     Charles

     Dickens

    .

     It

     is

     also

     a

     major

     transportation

     hub

    ,

     with

     the

     Paris

    地铁

    and

     the

     Î

    le

     de

     la

     C

    ité

     as

     its

     main

     transportation

     system

    .

     In

     terms

     of

     its

     status

     as

     a

     cultural

     and

     economic

     center

    ,

     Paris

     is

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     expected

     to

     involve

     significant

     advancements

     in

     several

     key

     areas

    :
    


    1

    .

     Improved

     Natural

     Language

     Processing

    :

     As

     AI

     continues

     to

     improve

    ,

     we

     can

     expect

     to

     see

     a

     more

     natural

     and

     convers

    ational

     interaction

     between

     humans

     and

     machines

    .

     This

     includes

     the

     development

     of

     more

     accurate

     language

     models

     that

     can

     understand

     and

     respond

     to

     natural

     language

     queries

     in

     a

     way

     that

     is

     more

     human

    -like

    .
    


    2

    .

     Enhanced

     Rob

    otic

     Intelligence

    :

     Robotics

     will

     continue

     to

     become

     more

     advanced

    ,

     with

     the

     ability

     to

     create

     more

     complex

    ,

     intelligent

     robots

     that

     can

     perform

     a

     wide

     range

     of

     tasks

    .

     This

     will

     include

     the

     development

     of

     more

     sophisticated

     robots

     that

     can

     learn

     from

     experience

    ,

     adapt

     to

     different

     tasks

    ,

     and

     perform

     tasks

     in

     different

     environments

    .
    


    



```python
llm.shutdown()
```
