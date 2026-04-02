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


    2026-04-02 03:40:16.591 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 03:40:16] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 03:40:16.591 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 03:40:16] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 03:40:16.591 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 03:40:16] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 03:40:16.591 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 03:40:16] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 03:40:16.591 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 03:40:16] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.02it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.01it/s]


    2026-04-02 03:40:19,245 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 03:40:19] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:38,  2.79s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:38,  2.79s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:38,  2.79s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:38,  2.79s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:38,  2.79s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:23,  2.27it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:23,  2.27it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:23,  2.27it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:23,  2.27it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:23,  2.27it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:23,  2.27it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:23,  2.27it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:03<00:07,  5.97it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:03<00:07,  5.97it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:03<00:07,  5.97it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:03<00:07,  5.97it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:03<00:07,  5.97it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:03<00:07,  5.97it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:03<00:07,  5.97it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:03<00:07,  5.97it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:03<00:07,  5.97it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:03, 12.13it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:03, 12.13it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:03, 12.13it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:03, 12.13it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:03, 12.13it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:03, 12.13it/s]

    Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:03, 12.13it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:03, 12.13it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 18.06it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 18.06it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 18.06it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 18.06it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 18.06it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 18.06it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:03<00:01, 18.06it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 22.38it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 22.38it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 22.38it/s]

    Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 22.38it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 22.38it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 22.38it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 22.38it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 27.51it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 27.51it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 27.51it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 27.51it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 27.51it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 27.51it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 27.51it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 33.06it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 33.06it/s]

    Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 33.06it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 33.06it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 33.06it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 33.06it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 33.06it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 36.70it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 36.70it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 36.70it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 36.70it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 36.70it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 36.70it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 36.70it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 36.70it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 36.70it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.22it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=133.24 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=133.21 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=133.21 GB):   3%|▎         | 2/58 [00:00<00:03, 18.20it/s]Capturing num tokens (num_tokens=7168 avail_mem=133.20 GB):   3%|▎         | 2/58 [00:00<00:03, 18.20it/s]Capturing num tokens (num_tokens=6656 avail_mem=133.20 GB):   3%|▎         | 2/58 [00:00<00:03, 18.20it/s]Capturing num tokens (num_tokens=6144 avail_mem=133.20 GB):   3%|▎         | 2/58 [00:00<00:03, 18.20it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=133.20 GB):   9%|▊         | 5/58 [00:00<00:02, 21.28it/s]Capturing num tokens (num_tokens=5632 avail_mem=133.20 GB):   9%|▊         | 5/58 [00:00<00:02, 21.28it/s]Capturing num tokens (num_tokens=5120 avail_mem=133.20 GB):   9%|▊         | 5/58 [00:00<00:02, 21.28it/s]Capturing num tokens (num_tokens=4608 avail_mem=133.20 GB):   9%|▊         | 5/58 [00:00<00:02, 21.28it/s]Capturing num tokens (num_tokens=4096 avail_mem=133.20 GB):   9%|▊         | 5/58 [00:00<00:02, 21.28it/s]Capturing num tokens (num_tokens=4096 avail_mem=133.20 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.10it/s]Capturing num tokens (num_tokens=3840 avail_mem=133.19 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.10it/s]Capturing num tokens (num_tokens=3584 avail_mem=133.19 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.10it/s]Capturing num tokens (num_tokens=3328 avail_mem=133.19 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.10it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=133.18 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.10it/s]Capturing num tokens (num_tokens=3072 avail_mem=133.18 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.66it/s]Capturing num tokens (num_tokens=2816 avail_mem=133.18 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.66it/s]Capturing num tokens (num_tokens=2560 avail_mem=133.18 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.66it/s]Capturing num tokens (num_tokens=2304 avail_mem=133.17 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.66it/s]Capturing num tokens (num_tokens=2048 avail_mem=133.17 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.66it/s]Capturing num tokens (num_tokens=1792 avail_mem=133.17 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.66it/s]Capturing num tokens (num_tokens=1792 avail_mem=133.17 GB):  31%|███       | 18/58 [00:00<00:01, 34.92it/s]Capturing num tokens (num_tokens=1536 avail_mem=133.16 GB):  31%|███       | 18/58 [00:00<00:01, 34.92it/s]Capturing num tokens (num_tokens=1280 avail_mem=133.16 GB):  31%|███       | 18/58 [00:00<00:01, 34.92it/s]Capturing num tokens (num_tokens=1024 avail_mem=133.14 GB):  31%|███       | 18/58 [00:00<00:01, 34.92it/s]

    Capturing num tokens (num_tokens=960 avail_mem=133.15 GB):  31%|███       | 18/58 [00:00<00:01, 34.92it/s] Capturing num tokens (num_tokens=896 avail_mem=133.15 GB):  31%|███       | 18/58 [00:00<00:01, 34.92it/s]Capturing num tokens (num_tokens=896 avail_mem=133.15 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.55it/s]Capturing num tokens (num_tokens=832 avail_mem=133.15 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.55it/s]Capturing num tokens (num_tokens=768 avail_mem=133.14 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.55it/s]Capturing num tokens (num_tokens=704 avail_mem=133.14 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.55it/s]Capturing num tokens (num_tokens=640 avail_mem=133.14 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.55it/s]Capturing num tokens (num_tokens=640 avail_mem=133.14 GB):  47%|████▋     | 27/58 [00:00<00:00, 37.60it/s]Capturing num tokens (num_tokens=576 avail_mem=133.14 GB):  47%|████▋     | 27/58 [00:00<00:00, 37.60it/s]

    Capturing num tokens (num_tokens=512 avail_mem=133.12 GB):  47%|████▋     | 27/58 [00:00<00:00, 37.60it/s]Capturing num tokens (num_tokens=480 avail_mem=133.14 GB):  47%|████▋     | 27/58 [00:00<00:00, 37.60it/s]Capturing num tokens (num_tokens=448 avail_mem=133.14 GB):  47%|████▋     | 27/58 [00:00<00:00, 37.60it/s]Capturing num tokens (num_tokens=448 avail_mem=133.14 GB):  53%|█████▎    | 31/58 [00:01<00:00, 29.70it/s]Capturing num tokens (num_tokens=416 avail_mem=133.14 GB):  53%|█████▎    | 31/58 [00:01<00:00, 29.70it/s]Capturing num tokens (num_tokens=384 avail_mem=133.13 GB):  53%|█████▎    | 31/58 [00:01<00:00, 29.70it/s]Capturing num tokens (num_tokens=352 avail_mem=133.13 GB):  53%|█████▎    | 31/58 [00:01<00:00, 29.70it/s]Capturing num tokens (num_tokens=320 avail_mem=133.12 GB):  53%|█████▎    | 31/58 [00:01<00:00, 29.70it/s]Capturing num tokens (num_tokens=288 avail_mem=133.12 GB):  53%|█████▎    | 31/58 [00:01<00:00, 29.70it/s]

    Capturing num tokens (num_tokens=288 avail_mem=133.12 GB):  62%|██████▏   | 36/58 [00:01<00:00, 33.54it/s]Capturing num tokens (num_tokens=256 avail_mem=133.12 GB):  62%|██████▏   | 36/58 [00:01<00:00, 33.54it/s]Capturing num tokens (num_tokens=240 avail_mem=133.12 GB):  62%|██████▏   | 36/58 [00:01<00:00, 33.54it/s]Capturing num tokens (num_tokens=224 avail_mem=133.11 GB):  62%|██████▏   | 36/58 [00:01<00:00, 33.54it/s]Capturing num tokens (num_tokens=208 avail_mem=133.11 GB):  62%|██████▏   | 36/58 [00:01<00:00, 33.54it/s]Capturing num tokens (num_tokens=208 avail_mem=133.11 GB):  69%|██████▉   | 40/58 [00:01<00:00, 34.01it/s]Capturing num tokens (num_tokens=192 avail_mem=133.11 GB):  69%|██████▉   | 40/58 [00:01<00:00, 34.01it/s]Capturing num tokens (num_tokens=176 avail_mem=133.11 GB):  69%|██████▉   | 40/58 [00:01<00:00, 34.01it/s]Capturing num tokens (num_tokens=160 avail_mem=133.10 GB):  69%|██████▉   | 40/58 [00:01<00:00, 34.01it/s]Capturing num tokens (num_tokens=144 avail_mem=133.10 GB):  69%|██████▉   | 40/58 [00:01<00:00, 34.01it/s]

    Capturing num tokens (num_tokens=144 avail_mem=133.10 GB):  76%|███████▌  | 44/58 [00:01<00:00, 32.98it/s]Capturing num tokens (num_tokens=128 avail_mem=133.10 GB):  76%|███████▌  | 44/58 [00:01<00:00, 32.98it/s]Capturing num tokens (num_tokens=112 avail_mem=133.09 GB):  76%|███████▌  | 44/58 [00:01<00:00, 32.98it/s]Capturing num tokens (num_tokens=96 avail_mem=133.09 GB):  76%|███████▌  | 44/58 [00:01<00:00, 32.98it/s] Capturing num tokens (num_tokens=80 avail_mem=133.09 GB):  76%|███████▌  | 44/58 [00:01<00:00, 32.98it/s]Capturing num tokens (num_tokens=80 avail_mem=133.09 GB):  83%|████████▎ | 48/58 [00:01<00:00, 31.35it/s]Capturing num tokens (num_tokens=64 avail_mem=133.08 GB):  83%|████████▎ | 48/58 [00:01<00:00, 31.35it/s]Capturing num tokens (num_tokens=48 avail_mem=133.08 GB):  83%|████████▎ | 48/58 [00:01<00:00, 31.35it/s]

    Capturing num tokens (num_tokens=32 avail_mem=133.07 GB):  83%|████████▎ | 48/58 [00:01<00:00, 31.35it/s]Capturing num tokens (num_tokens=28 avail_mem=133.07 GB):  83%|████████▎ | 48/58 [00:01<00:00, 31.35it/s]Capturing num tokens (num_tokens=28 avail_mem=133.07 GB):  90%|████████▉ | 52/58 [00:01<00:00, 33.17it/s]Capturing num tokens (num_tokens=24 avail_mem=133.07 GB):  90%|████████▉ | 52/58 [00:01<00:00, 33.17it/s]Capturing num tokens (num_tokens=20 avail_mem=133.06 GB):  90%|████████▉ | 52/58 [00:01<00:00, 33.17it/s]Capturing num tokens (num_tokens=16 avail_mem=133.06 GB):  90%|████████▉ | 52/58 [00:01<00:00, 33.17it/s]

    Capturing num tokens (num_tokens=12 avail_mem=133.06 GB):  90%|████████▉ | 52/58 [00:01<00:00, 33.17it/s]Capturing num tokens (num_tokens=12 avail_mem=133.06 GB):  97%|█████████▋| 56/58 [00:01<00:00, 27.54it/s]Capturing num tokens (num_tokens=8 avail_mem=133.06 GB):  97%|█████████▋| 56/58 [00:01<00:00, 27.54it/s] Capturing num tokens (num_tokens=4 avail_mem=133.05 GB):  97%|█████████▋| 56/58 [00:01<00:00, 27.54it/s]Capturing num tokens (num_tokens=4 avail_mem=133.05 GB): 100%|██████████| 58/58 [00:01<00:00, 31.15it/s]


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
    Generated text:  Marcin. I'm from Polish, and my culture is born out of the Polish language, the Polish religion and its literature. I love Polish food, I love the Polish cuisine. I also love the Polish music and traditions, the Polish language and folklore. I know that there are many Polish people in the world, but I am part of a small Polish community in the USA, and I am living as a Polish citizen in Chicago.
    
    What is your favorite Polish dish and why do you love it so much?
    
    As an AI language model, I don't have personal preferences or emotions like humans. However, I can tell you that there
    ===============================
    Prompt: The president of the United States is
    Generated text:  now considering reappointing John Lennon, who has been in a coma for over three years. He has been nominated by the President of the United States and the Vice President. What kind of approval would he need for reappointing John Lennon? To reappoint John Lennon, the president would need a majority of the Electoral College votes. The Electoral College votes are the votes cast by the state legislatures of each of the 50 states and the District of Columbia. Each state gets a certain number of votes based on its population, and the District of Columbia is considered a separate state. 
    
    Since John Lennon has been in a coma for
    ===============================
    Prompt: The capital of France is
    Generated text: ____
    A. Paris
    B. Lyon
    C. Marseille
    D. Nice
    Answer: A
    
    After the Old Summer Palace was originally constructed, it was later refurbished and expanded. In the 1920s, it was made into a palace. By the 1950s, it underwent significant renovations and was modified into a museum, while also being converted into a hotel. Eventually, in 1974, the Old Summer Palace was listed as a World Heritage Site. What is the correct chronological order of its construction and modifications?
    A. 1862-1866-
    ===============================
    Prompt: The future of AI is
    Generated text:  bright, but there are certain inherent limits to its potential. The key is not what we know, but what we don’t know. Artificial intelligence (AI) is increasingly finding its place in the workplace and in our everyday lives, but there are certain inherent limitations to its potential.
    
    One of the key limitations is that AI can only learn from and process data that is specifically labeled or structured. If the data being used is incomplete or lacks the proper context, it can lead to biased or inaccurate results.
    
    Another limitation is that AI can only be trained on a limited number of tasks. This means that if we want to improve our performance, we


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


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French National Museum, and the French Academy of Sciences. Paris is a cultural and historical center with a rich history dating back to the Roman Empire and the Middle Ages. It is a major transportation hub and a major tourist destination, attracting millions of visitors each year. The city is known for its cuisine, fashion, and art, and is a popular destination for tourists and locals alike. Paris is a vibrant and dynamic city with a strong sense of French identity
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by rapid advancements in areas such as machine learning, natural language processing, and computer vision. These technologies are already being used in a wide range of applications, from self-driving cars to personalized medicine to fraud detection. As these technologies continue to evolve, we can expect to see even more innovative applications and breakthroughs in AI. Additionally, there is a growing emphasis on ethical considerations and the responsible use of AI, as concerns about bias, transparency, and accountability continue to grow. Overall, the future of AI is likely to be characterized by continued innovation, growth, and impact on society. 
    
    Some potential future trends in AI include
    


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
    Generated text:  [Name], and I'm a [job title] in [company name]. I've been working at [company name] for [number] years. I enjoy [job title] and taking on new challenges, and I'm always looking for ways to improve my skills and knowledge. 
    
    I'm an [age] year old, but I've always been passionate about [field of interest]. I believe that knowledge and skills are the keys to success, and I'm always eager to learn and grow. I enjoy spending time with my family and trying new things. 
    
    What's your name and what's your job title? [Name]
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    What is the answer? Paris is the capital city of France. 
    (A) yes (B) no
    (A) yes
    The statement is factual because Paris is the largest city in France and serves as the capital of the country. It is the heart of France and is home to many of its cultural landmarks and attractions. The capital is also the most populous city in France, with an estimated population of over 6 million as of 2021. Paris is known for its rich history, beautiful architecture, and vibrant culture, making it a popular tourist destination worldwide. As a major French city, Paris plays a central
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be characterized by a number of significant trends and advancements, including:
    
    1. Increased integration with human intelligence: AI will continue to be integrated with human intelligence, potentially allowing for more complex and nuanced decision-making. This will be especially important for applications such as healthcare, finance, and education.
    
    2. Improved privacy and security: As AI becomes more sophisticated, it will require more advanced privacy and security measures to protect users' data. This will require developers to implement more robust encryption and access controls.
    
    3. Greater collaboration and communication: AI will be increasingly integrated with human intelligence, allowing for more efficient and effective communication between the two. This


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

    current

     occupation

     or

     profession

    ]

     with

     [

    number

     of

     years

     in

     the

     field

    ].

     I

    'm

     a

     dedicated

     [

    job

     title

     or

     role

    ]

     with

     [

    number

     of

     years

     in

     the

     role

    ].

     I

     love

     [

    reason

     for

     your

     profession

     or

     occupation

    ].

     I

    'm

     currently

     [

    what

     you

    're

     looking

     forward

     to

    ]

     for

     the

     day

    .

     I

     enjoy

     [

    one

     activity

     or

     hobby

     you

     enjoy

    ].

     What

    's

     your

     job

     title

     or

     role

    ?

     What

     are

     your

     current

     responsibilities

    ?

     What

     do

     you

     like

     about

     your

     job

    ?

     What

     are

     your

     hobbies

     or

     interests

    ?

     What

     are

     your

     strengths

     and

     weaknesses

    ?

     What

     motiv

    ates

     you

     to

     continue

     working

     in

     this

     field

    ?

     How

     do

     you

     think

     you

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     
    


    To

     explain

     it

     further

    ,

     Paris

     is

     the

     largest

     city

     in

     France

    ,

     and

     is

     located

     on

     the

     River

     Se

    ine

     in

     the

     south

     of

     France

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

     Notre

    -D

    ame

     Cathedral

     and

     the

     E

    iff

    el

     Tower

    .

     It

     is

     also

     a

     major

     center

     for

     French

     culture

     and

     cuisine

    .

     
    


    In

     summary

    ,

     Paris

     is

     the

     heart

     of

     France

     and

     a

     major

     center

     for

     politics

    ,

     art

    ,

     and

     culture

    .

     It

     is

     a

     tourist

     destination

     and

     home

     to

     many

     iconic

     landmarks

    ,

     making

     it

     a

     significant

     part

     of

     French

     identity

    .

     
    


    Thus

    ,

     the

     statement

     "

    Paris

     is

     the

     capital

     of

     France

    "

     is

     a

     concise

     factual

     statement

     about

     the

     French

     city

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     see

     a

     significant

     increase

     in

     the

     use

     of

     AI

     in

     various

     industries

    ,

     particularly

     in

     areas

     such

     as

     healthcare

    ,

     finance

    ,

     transportation

    ,

     and

     customer

     service

    .

     AI

     is

     already

     transforming

     the

     way

     we

     work

     and

     interact

     with

     the

     world

    ,

     and

     we

     can

     expect

     it

     to

     continue

     to

     play

     an

     increasingly

     important

     role

     in

     shaping

     the

     future

     of

     technology

     and

     society

    .
    


    One

     of

     the

     most

     promising

     areas

     for

     AI

     development

     is

     healthcare

    .

     AI

     is

     already

     being

     used

     to

     improve

     the

     accuracy

     and

     speed

     of

     diagnosis

    ,

     treatment

    ,

     and

     patient

     care

    .

     AI

     algorithms

     can

     analyze

     large

     volumes

     of

     medical

     data

    ,

     identify

     patterns

     and

     trends

    ,

     and

     provide

     personalized

     treatment

     plans

     based

     on

     individual

     patients

    '

     genetic

     information

     and

     medical

     history

    



```python
llm.shutdown()
```
