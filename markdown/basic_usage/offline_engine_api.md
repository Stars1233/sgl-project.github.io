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


    2026-04-01 03:37:56.354 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 03:37:56] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 03:37:56.354 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 03:37:56] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 03:37:56.354 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 03:37:56] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 03:37:56.354 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 03:37:56] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 03:37:56.354 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 03:37:56] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.07it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.06it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:41,  2.83s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:41,  2.83s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:41,  2.83s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:41,  2.83s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:41,  2.83s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:23,  2.24it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:23,  2.24it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:23,  2.24it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:23,  2.24it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:03<00:23,  2.24it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:03<00:23,  2.24it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:03<00:23,  2.24it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:03<00:07,  5.89it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:03<00:07,  5.89it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:03<00:07,  5.89it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:03<00:07,  5.89it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:03<00:07,  5.89it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:03<00:07,  5.89it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:03<00:07,  5.89it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:03<00:07,  5.89it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:03<00:07,  5.89it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:03, 11.98it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:03, 11.98it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:03, 11.98it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:03, 11.98it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:03, 11.98it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:03, 11.98it/s]

    Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:03, 11.98it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:03, 11.98it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 17.86it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 17.86it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 17.86it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 17.86it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 17.86it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 17.86it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:03<00:01, 17.86it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 23.30it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 23.30it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 23.30it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 23.30it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 23.30it/s]

    Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 23.30it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 23.30it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 28.50it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 28.50it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 28.50it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 28.50it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 28.50it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 28.50it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 28.50it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 34.06it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 34.06it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 34.06it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 34.06it/s] 

    Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 34.06it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 34.06it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 34.06it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 37.54it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 37.54it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 37.54it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 37.54it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 37.54it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 37.54it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 37.54it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 37.54it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 37.54it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.21it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=135.00 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=127.94 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=127.94 GB):   3%|▎         | 2/58 [00:00<00:03, 16.71it/s]Capturing num tokens (num_tokens=7168 avail_mem=121.70 GB):   3%|▎         | 2/58 [00:00<00:03, 16.71it/s]Capturing num tokens (num_tokens=6656 avail_mem=121.70 GB):   3%|▎         | 2/58 [00:00<00:03, 16.71it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=121.70 GB):   3%|▎         | 2/58 [00:00<00:03, 16.71it/s]Capturing num tokens (num_tokens=6144 avail_mem=121.70 GB):   9%|▊         | 5/58 [00:00<00:02, 20.48it/s]Capturing num tokens (num_tokens=5632 avail_mem=121.70 GB):   9%|▊         | 5/58 [00:00<00:02, 20.48it/s]Capturing num tokens (num_tokens=5120 avail_mem=121.70 GB):   9%|▊         | 5/58 [00:00<00:02, 20.48it/s]Capturing num tokens (num_tokens=4608 avail_mem=121.70 GB):   9%|▊         | 5/58 [00:00<00:02, 20.48it/s]Capturing num tokens (num_tokens=4096 avail_mem=121.70 GB):   9%|▊         | 5/58 [00:00<00:02, 20.48it/s]Capturing num tokens (num_tokens=4096 avail_mem=121.70 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.47it/s]Capturing num tokens (num_tokens=3840 avail_mem=121.69 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.47it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=121.41 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.47it/s]Capturing num tokens (num_tokens=3328 avail_mem=121.41 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.47it/s]Capturing num tokens (num_tokens=3072 avail_mem=121.13 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.47it/s]Capturing num tokens (num_tokens=3072 avail_mem=121.13 GB):  22%|██▏       | 13/58 [00:00<00:01, 28.85it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.70 GB):  22%|██▏       | 13/58 [00:00<00:01, 28.85it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.70 GB):  22%|██▏       | 13/58 [00:00<00:01, 28.85it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.70 GB):  22%|██▏       | 13/58 [00:00<00:01, 28.85it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.69 GB):  22%|██▏       | 13/58 [00:00<00:01, 28.85it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.69 GB):  22%|██▏       | 13/58 [00:00<00:01, 28.85it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.69 GB):  31%|███       | 18/58 [00:00<00:01, 33.93it/s]Capturing num tokens (num_tokens=1536 avail_mem=120.69 GB):  31%|███       | 18/58 [00:00<00:01, 33.93it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=120.68 GB):  31%|███       | 18/58 [00:00<00:01, 33.93it/s]Capturing num tokens (num_tokens=1024 avail_mem=120.66 GB):  31%|███       | 18/58 [00:00<00:01, 33.93it/s]Capturing num tokens (num_tokens=960 avail_mem=120.68 GB):  31%|███       | 18/58 [00:00<00:01, 33.93it/s] Capturing num tokens (num_tokens=960 avail_mem=120.68 GB):  38%|███▊      | 22/58 [00:00<00:01, 34.78it/s]Capturing num tokens (num_tokens=896 avail_mem=120.67 GB):  38%|███▊      | 22/58 [00:00<00:01, 34.78it/s]Capturing num tokens (num_tokens=832 avail_mem=120.67 GB):  38%|███▊      | 22/58 [00:00<00:01, 34.78it/s]Capturing num tokens (num_tokens=768 avail_mem=120.67 GB):  38%|███▊      | 22/58 [00:00<00:01, 34.78it/s]Capturing num tokens (num_tokens=704 avail_mem=120.66 GB):  38%|███▊      | 22/58 [00:00<00:01, 34.78it/s]Capturing num tokens (num_tokens=640 avail_mem=120.66 GB):  38%|███▊      | 22/58 [00:00<00:01, 34.78it/s]Capturing num tokens (num_tokens=640 avail_mem=120.66 GB):  47%|████▋     | 27/58 [00:00<00:00, 37.23it/s]Capturing num tokens (num_tokens=576 avail_mem=120.66 GB):  47%|████▋     | 27/58 [00:00<00:00, 37.23it/s]

    Capturing num tokens (num_tokens=512 avail_mem=120.65 GB):  47%|████▋     | 27/58 [00:00<00:00, 37.23it/s]Capturing num tokens (num_tokens=480 avail_mem=120.66 GB):  47%|████▋     | 27/58 [00:00<00:00, 37.23it/s]Capturing num tokens (num_tokens=448 avail_mem=120.66 GB):  47%|████▋     | 27/58 [00:00<00:00, 37.23it/s]Capturing num tokens (num_tokens=448 avail_mem=120.66 GB):  53%|█████▎    | 31/58 [00:00<00:00, 37.01it/s]Capturing num tokens (num_tokens=416 avail_mem=120.23 GB):  53%|█████▎    | 31/58 [00:00<00:00, 37.01it/s]Capturing num tokens (num_tokens=384 avail_mem=120.23 GB):  53%|█████▎    | 31/58 [00:00<00:00, 37.01it/s]Capturing num tokens (num_tokens=352 avail_mem=120.23 GB):  53%|█████▎    | 31/58 [00:00<00:00, 37.01it/s]Capturing num tokens (num_tokens=320 avail_mem=120.22 GB):  53%|█████▎    | 31/58 [00:01<00:00, 37.01it/s]Capturing num tokens (num_tokens=288 avail_mem=120.22 GB):  53%|█████▎    | 31/58 [00:01<00:00, 37.01it/s]Capturing num tokens (num_tokens=288 avail_mem=120.22 GB):  62%|██████▏   | 36/58 [00:01<00:00, 39.33it/s]Capturing num tokens (num_tokens=256 avail_mem=120.22 GB):  62%|██████▏   | 36/58 [00:01<00:00, 39.33it/s]

    Capturing num tokens (num_tokens=240 avail_mem=120.21 GB):  62%|██████▏   | 36/58 [00:01<00:00, 39.33it/s]Capturing num tokens (num_tokens=224 avail_mem=120.21 GB):  62%|██████▏   | 36/58 [00:01<00:00, 39.33it/s]Capturing num tokens (num_tokens=208 avail_mem=120.21 GB):  62%|██████▏   | 36/58 [00:01<00:00, 39.33it/s]Capturing num tokens (num_tokens=208 avail_mem=120.21 GB):  69%|██████▉   | 40/58 [00:01<00:00, 38.03it/s]Capturing num tokens (num_tokens=192 avail_mem=120.21 GB):  69%|██████▉   | 40/58 [00:01<00:00, 38.03it/s]Capturing num tokens (num_tokens=176 avail_mem=120.20 GB):  69%|██████▉   | 40/58 [00:01<00:00, 38.03it/s]Capturing num tokens (num_tokens=160 avail_mem=120.20 GB):  69%|██████▉   | 40/58 [00:01<00:00, 38.03it/s]Capturing num tokens (num_tokens=144 avail_mem=120.19 GB):  69%|██████▉   | 40/58 [00:01<00:00, 38.03it/s]Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  69%|██████▉   | 40/58 [00:01<00:00, 38.03it/s]

    Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  78%|███████▊  | 45/58 [00:01<00:00, 39.79it/s]Capturing num tokens (num_tokens=112 avail_mem=120.19 GB):  78%|███████▊  | 45/58 [00:01<00:00, 39.79it/s]Capturing num tokens (num_tokens=96 avail_mem=120.18 GB):  78%|███████▊  | 45/58 [00:01<00:00, 39.79it/s] Capturing num tokens (num_tokens=80 avail_mem=120.18 GB):  78%|███████▊  | 45/58 [00:01<00:00, 39.79it/s]Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  78%|███████▊  | 45/58 [00:01<00:00, 39.79it/s]Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.31it/s]Capturing num tokens (num_tokens=48 avail_mem=120.17 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.31it/s]Capturing num tokens (num_tokens=32 avail_mem=120.17 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.31it/s]Capturing num tokens (num_tokens=28 avail_mem=120.16 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.31it/s]Capturing num tokens (num_tokens=24 avail_mem=120.16 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.31it/s]Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.31it/s]

    Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  93%|█████████▎| 54/58 [00:01<00:00, 39.54it/s]Capturing num tokens (num_tokens=16 avail_mem=120.16 GB):  93%|█████████▎| 54/58 [00:01<00:00, 39.54it/s]Capturing num tokens (num_tokens=12 avail_mem=120.15 GB):  93%|█████████▎| 54/58 [00:01<00:00, 39.54it/s]Capturing num tokens (num_tokens=8 avail_mem=120.15 GB):  93%|█████████▎| 54/58 [00:01<00:00, 39.54it/s] Capturing num tokens (num_tokens=4 avail_mem=120.15 GB):  93%|█████████▎| 54/58 [00:01<00:00, 39.54it/s]Capturing num tokens (num_tokens=4 avail_mem=120.15 GB): 100%|██████████| 58/58 [00:01<00:00, 36.10it/s]


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
    Generated text:  Jolyon Tarrant and I am a neuroscientist at the University of East Anglia. My research focuses on understanding the brain and how it learns, from the earliest moments of an embryo to the last moments of life. My current project investigates how the brain adapts to the stresses of learning and memory in healthy people as well as those with neurological disorders. My lab is also interested in how the brain adapts to the stresses of learning and memory in people with epilepsy. I work on the ways in which the brain forms memories and how the brain adapts to the stresses of learning and memory in healthy and diseased people. My
    ===============================
    Prompt: The president of the United States is
    Generated text:  a wealthy man with a wealth of experience and knowledge of various political systems. He is interested in the upcoming election and has researched the voting records of previous presidents to understand their voting patterns. He has also read about the political systems of several countries and has gathered data on the voting patterns of different groups within each country. Based on his research and analysis, he wants to create a voting prediction model for the upcoming election.
    
    Given that the president is interested in the voting patterns of different groups within each country, can you provide a method to estimate the voting patterns of different groups within each country? Assume that the voting patterns are represented by a vector of
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. The longest word in French is "France", which has 12 letters. Can you find the longest word in the English language? Yes, I can help you with that! The longest word in the English language is "universe". It has 14 letters, but the answer you're looking for is the longest word in the English language, not the longest word in the world. The longest word in the English language is "universe". Is there anything else I can help you with?
    ===============================
    Prompt: The future of AI is
    Generated text:  uncertain, but there are several areas where we can use AI to create systems that can adapt to new challenges and improve the quality of our lives. AI can help us with tasks such as data analysis, fraud detection, and even medical diagnosis. Additionally, AI can be used to improve our relationships with others, such as social media platforms, which have the potential to help us build stronger, more connected communities.
    
    Moreover, AI can be used to make our transportation system more efficient. AI can help us predict traffic patterns, optimize routes, and even improve our overall safety on the roads. This, in turn, can reduce accidents and improve traffic flow


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


    Generated text:  [Name] and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm passionate about [job title] and [job title]. I'm always looking for new challenges and opportunities to grow and learn. What's your favorite hobby or activity? I'm a [job title] at [company name], and I'm always looking for new challenges and opportunities to grow and learn. What's your favorite hobby or activity? I'm a [job title] at [
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as the City of Light. It is the largest city in France and the third-largest city in the world by population. Paris is known for its rich history, art, and culture, and is a popular tourist destination. The city is home to many famous landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. Paris is also known for its cuisine, including its famous French fries and croissants. The city is a major economic and cultural center in Europe and plays a significant role in the country's political and social life. Paris is a vibrant and dynamic city that continues to be
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends:
    
    1. Increased focus on ethical AI: As more people become aware of the potential risks of AI, there will be a greater emphasis on developing ethical AI systems that are designed to minimize harm and maximize benefits. This will likely involve developing AI that is transparent, accountable, and accountable to humans.
    
    2. Greater integration with other technologies: AI is likely to become more integrated with other technologies, such as machine learning, natural language processing, and robotics. This will allow for more complex and
    


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
    Generated text:  [Your Name], and I am [Your Profession]. I have always been fascinated by [Your Profession] and have always been curious about the world. I am always trying to learn and grow, and I am always eager to share my knowledge and experiences with anyone who is interested in learning about [Your Profession]. Please let me know how I can be of assistance to you. Good luck with your [Your Profession] journey. [Your Name] [Your Profession] [Your Profession] [Your Profession] [Your Profession] [Your Profession] [Your Profession] [Your Profession] [Your Profession] [Your Profession] [Your Profession
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, known as the "City of Light" and the "City of Art". It is situated on the western bank of the Seine river and is home to numerous famous attractions such as the Louvre Museum, the Notre-Dame Cathedral, and the Arc de Triomphe. The city also houses many art museums and cultural institutions, making it an important center for the arts. Paris is home to numerous international landmarks, including the Eiffel Tower and the Louvre Pyramid. It is a vibrant and cosmopolitan city with a rich history and a strong sense of French identity. According to the latest data from the European Central Bank (
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  an exciting one, with potential to revolutionize many industries and shapes our lives in new ways. Here are some potential trends and innovations that are likely to shape AI in the coming years:
    
    1. AI will become more accessible: With the increasing availability of data and computational power, AI will become more accessible to people and businesses of all sizes. This will lead to a democratization of AI, where anyone can use it to solve complex problems.
    
    2. AI will become more integrated with other technologies: AI will continue to integrate with other technologies, such as machine learning, blockchain, and the internet of things. This integration will bring about new business


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

     John

     Smith

     and

     I

    'm

     a

     professional

     writer

    .

     I

    've

     been

     working

     in

     the

     field

     of

     creative

     writing

     for

     

    1

    5

     years

     and

     have

     published

     numerous

     books

    .

     I

    'm

     passionate

     about

     writing

     and

     love

     to

     explore

     new

     genres

    .

     I

    'm

     always

     ready

     to

     share

     my

     work

     and

     share

     some

     of

     my

     writing

     tips

    .

     How

     can

     I

     make

     you

     interested

     in

     my

     work

    ?

     Hi

    ,

     I

    'm

     John

     Smith

    ,

     a

     professional

     writer

    .

     I

    've

     been

     working

     in

     the

     field

     of

     creative

     writing

     for

     

    1

    5

     years

     and

     have

     published

     numerous

     books

    .

     I

    'm

     passionate

     about

     writing

     and

     love

     to

     explore

     new

     genres

    .

     I

    'm

     always

     ready

     to

     share

     my

     work

     and

     share

     some

     of

     my

     writing

     tips

    .

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     It

     is

     the

     largest

     city

     in

     the

     European

     Union

     and

     has

     a

     population

     of

     approximately

     

    2

    .

    7

     million

     people

    .

     The

     city

     is

     home

     to

     many

     of

     France

    's

     most

     iconic

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

     known

     for

     its

     vibrant

     cultural

     scene

    ,

     from

     the

     classical

     French

     art

     and

     music

     scene

     to

     the

     modern

     fashion

     and

     film

     industry

    .

     The

     city

     has

     a

     rich

     history

    ,

     dating

     back

     to

     the

     time

     of

     the

     Roman

     Empire

     and

     having

     been

     a

     major

     trade

     center

     for

     centuries

    .

     It

     is

     considered

     a

     city

     of

     many

     faces

    ,

     with

     its

     diverse

     population

     and

     a

     rich

     tape

    stry

     of

     cultural

    
    
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

    ,

     including

    :
    


    1

    .

     Increased

     personal

    ization

    :

     As

     AI

     becomes

     more

     advanced

    ,

     it

     will

     be

     able

     to

     learn

     and

     adapt

     to

     individual

     users

    ,

     leading

     to

     more

     personalized

     and

     tailored

     experiences

    .
    


    2

    .

     Improved

     security

    :

     AI

     systems

     will

     become

     more

     sophisticated

    ,

     enabling

     them

     to

     detect

     and

     respond

     to

     security

     threats

     more

     effectively

    ,

     leading

     to

     better

     overall

     security

    .
    


    3

    .

     Autonomous

     agents

    :

     AI

     will

     become

     more

     capable

     of

     making

     decisions

     and

     taking

     actions

     on

     their

     own

    ,

     allowing

     for

     more

     autonomous

     and

     flexible

     agents

    .
    


    4

    .

     Language

     translation

    :

     AI

     will

     become

     more

     accurate

     and

     efficient

     at

     translating

     languages

    ,

     allowing

     for

     greater

     cross

    -cultural

     communication

    .
    


    5

    .

     Economic

     impact

    



```python
llm.shutdown()
```
