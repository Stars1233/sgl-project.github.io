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


    2026-04-03 16:45:25.494 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 16:45:25] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 16:45:25.494 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 16:45:25] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 16:45:25.494 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 16:45:25] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 16:45:25.494 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 16:45:25] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 16:45:25.494 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 16:45:25] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  7.63it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  7.62it/s]


    2026-04-03 16:45:28,392 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 16:45:28] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:36,  2.74s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:36,  2.74s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:36,  2.74s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:36,  2.74s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:36,  2.74s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:23,  2.30it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:23,  2.30it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:23,  2.30it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:23,  2.30it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:23,  2.30it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:23,  2.30it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:23,  2.30it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:07,  6.06it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:07,  6.06it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:07,  6.06it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:07,  6.06it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:03<00:07,  6.06it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:03<00:07,  6.06it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:03<00:07,  6.06it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:03<00:07,  6.06it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:03<00:07,  6.06it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:03, 12.25it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:03, 12.25it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:03, 12.25it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:03, 12.25it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:03, 12.25it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:03, 12.25it/s]

    Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:03, 12.25it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:03, 12.25it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 18.15it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 18.15it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 18.15it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 18.15it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 18.15it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 18.15it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:03<00:01, 18.15it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:03<00:01, 18.15it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:01, 24.50it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:01, 24.50it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:01, 24.50it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:01, 24.50it/s]

    Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:01, 24.50it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:01, 24.50it/s]Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:03<00:01, 24.50it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 29.26it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 29.26it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 29.26it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 29.26it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 29.26it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 29.26it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 29.26it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 33.96it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 33.96it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 33.96it/s] 

    Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 33.96it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 33.96it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 33.96it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 33.96it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 37.84it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 37.84it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 37.84it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 37.84it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 37.84it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 37.84it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 37.84it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 37.84it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.49it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=120.76 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.73 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:03, 17.93it/s]Capturing num tokens (num_tokens=7168 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:03, 17.93it/s]Capturing num tokens (num_tokens=6656 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:03, 17.93it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:03, 17.93it/s]Capturing num tokens (num_tokens=6144 avail_mem=120.73 GB):   9%|▊         | 5/58 [00:00<00:02, 20.08it/s]Capturing num tokens (num_tokens=5632 avail_mem=120.72 GB):   9%|▊         | 5/58 [00:00<00:02, 20.08it/s]Capturing num tokens (num_tokens=5120 avail_mem=120.72 GB):   9%|▊         | 5/58 [00:00<00:02, 20.08it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.72 GB):   9%|▊         | 5/58 [00:00<00:02, 20.08it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.72 GB):  14%|█▍        | 8/58 [00:00<00:02, 23.45it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.72 GB):  14%|█▍        | 8/58 [00:00<00:02, 23.45it/s]Capturing num tokens (num_tokens=3840 avail_mem=120.72 GB):  14%|█▍        | 8/58 [00:00<00:02, 23.45it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=120.71 GB):  14%|█▍        | 8/58 [00:00<00:02, 23.45it/s]Capturing num tokens (num_tokens=3328 avail_mem=120.71 GB):  14%|█▍        | 8/58 [00:00<00:02, 23.45it/s]Capturing num tokens (num_tokens=3328 avail_mem=120.71 GB):  21%|██        | 12/58 [00:00<00:01, 28.55it/s]Capturing num tokens (num_tokens=3072 avail_mem=120.70 GB):  21%|██        | 12/58 [00:00<00:01, 28.55it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.70 GB):  21%|██        | 12/58 [00:00<00:01, 28.55it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.70 GB):  21%|██        | 12/58 [00:00<00:01, 28.55it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.37 GB):  21%|██        | 12/58 [00:00<00:01, 28.55it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.27 GB):  21%|██        | 12/58 [00:00<00:01, 28.55it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.27 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.27it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.26 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.27it/s]Capturing num tokens (num_tokens=1536 avail_mem=120.26 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.27it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=120.26 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.27it/s]Capturing num tokens (num_tokens=1024 avail_mem=120.24 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.27it/s]Capturing num tokens (num_tokens=960 avail_mem=120.25 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.27it/s] Capturing num tokens (num_tokens=960 avail_mem=120.25 GB):  38%|███▊      | 22/58 [00:00<00:00, 36.20it/s]Capturing num tokens (num_tokens=896 avail_mem=120.25 GB):  38%|███▊      | 22/58 [00:00<00:00, 36.20it/s]Capturing num tokens (num_tokens=832 avail_mem=120.24 GB):  38%|███▊      | 22/58 [00:00<00:00, 36.20it/s]Capturing num tokens (num_tokens=768 avail_mem=120.24 GB):  38%|███▊      | 22/58 [00:00<00:00, 36.20it/s]Capturing num tokens (num_tokens=704 avail_mem=120.24 GB):  38%|███▊      | 22/58 [00:00<00:00, 36.20it/s]Capturing num tokens (num_tokens=640 avail_mem=120.23 GB):  38%|███▊      | 22/58 [00:00<00:00, 36.20it/s]Capturing num tokens (num_tokens=640 avail_mem=120.23 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.10it/s]Capturing num tokens (num_tokens=576 avail_mem=120.23 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.10it/s]

    Capturing num tokens (num_tokens=512 avail_mem=120.22 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.10it/s]Capturing num tokens (num_tokens=480 avail_mem=120.24 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.10it/s]Capturing num tokens (num_tokens=448 avail_mem=120.24 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.10it/s]Capturing num tokens (num_tokens=416 avail_mem=120.23 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.10it/s]Capturing num tokens (num_tokens=416 avail_mem=120.23 GB):  55%|█████▌    | 32/58 [00:00<00:00, 39.47it/s]Capturing num tokens (num_tokens=384 avail_mem=120.23 GB):  55%|█████▌    | 32/58 [00:00<00:00, 39.47it/s]Capturing num tokens (num_tokens=352 avail_mem=120.23 GB):  55%|█████▌    | 32/58 [00:00<00:00, 39.47it/s]Capturing num tokens (num_tokens=320 avail_mem=120.22 GB):  55%|█████▌    | 32/58 [00:00<00:00, 39.47it/s]Capturing num tokens (num_tokens=288 avail_mem=120.22 GB):  55%|█████▌    | 32/58 [00:01<00:00, 39.47it/s]Capturing num tokens (num_tokens=256 avail_mem=120.22 GB):  55%|█████▌    | 32/58 [00:01<00:00, 39.47it/s]

    Capturing num tokens (num_tokens=256 avail_mem=120.22 GB):  64%|██████▍   | 37/58 [00:01<00:00, 40.25it/s]Capturing num tokens (num_tokens=240 avail_mem=120.21 GB):  64%|██████▍   | 37/58 [00:01<00:00, 40.25it/s]Capturing num tokens (num_tokens=224 avail_mem=120.21 GB):  64%|██████▍   | 37/58 [00:01<00:00, 40.25it/s]Capturing num tokens (num_tokens=208 avail_mem=120.21 GB):  64%|██████▍   | 37/58 [00:01<00:00, 40.25it/s]Capturing num tokens (num_tokens=192 avail_mem=120.20 GB):  64%|██████▍   | 37/58 [00:01<00:00, 40.25it/s]Capturing num tokens (num_tokens=176 avail_mem=120.20 GB):  64%|██████▍   | 37/58 [00:01<00:00, 40.25it/s]Capturing num tokens (num_tokens=176 avail_mem=120.20 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.05it/s]Capturing num tokens (num_tokens=160 avail_mem=120.20 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.05it/s]Capturing num tokens (num_tokens=144 avail_mem=120.19 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.05it/s]Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.05it/s]Capturing num tokens (num_tokens=112 avail_mem=120.19 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.05it/s]

    Capturing num tokens (num_tokens=96 avail_mem=120.18 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.05it/s] Capturing num tokens (num_tokens=96 avail_mem=120.18 GB):  81%|████████  | 47/58 [00:01<00:00, 41.53it/s]Capturing num tokens (num_tokens=80 avail_mem=120.18 GB):  81%|████████  | 47/58 [00:01<00:00, 41.53it/s]Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  81%|████████  | 47/58 [00:01<00:00, 41.53it/s]Capturing num tokens (num_tokens=48 avail_mem=120.17 GB):  81%|████████  | 47/58 [00:01<00:00, 41.53it/s]Capturing num tokens (num_tokens=32 avail_mem=120.17 GB):  81%|████████  | 47/58 [00:01<00:00, 41.53it/s]Capturing num tokens (num_tokens=28 avail_mem=120.16 GB):  81%|████████  | 47/58 [00:01<00:00, 41.53it/s]Capturing num tokens (num_tokens=28 avail_mem=120.16 GB):  90%|████████▉ | 52/58 [00:01<00:00, 38.28it/s]Capturing num tokens (num_tokens=24 avail_mem=120.16 GB):  90%|████████▉ | 52/58 [00:01<00:00, 38.28it/s]

    Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  90%|████████▉ | 52/58 [00:01<00:00, 38.28it/s]Capturing num tokens (num_tokens=16 avail_mem=120.16 GB):  90%|████████▉ | 52/58 [00:01<00:00, 38.28it/s]Capturing num tokens (num_tokens=12 avail_mem=120.15 GB):  90%|████████▉ | 52/58 [00:01<00:00, 38.28it/s]Capturing num tokens (num_tokens=12 avail_mem=120.15 GB):  97%|█████████▋| 56/58 [00:01<00:00, 38.68it/s]Capturing num tokens (num_tokens=8 avail_mem=120.15 GB):  97%|█████████▋| 56/58 [00:01<00:00, 38.68it/s] Capturing num tokens (num_tokens=4 avail_mem=120.15 GB):  97%|█████████▋| 56/58 [00:01<00:00, 38.68it/s]Capturing num tokens (num_tokens=4 avail_mem=120.15 GB): 100%|██████████| 58/58 [00:01<00:00, 36.25it/s]


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
    Generated text:  Karina. I'm from Georgia, USA, and I like to write about various topics like things to do, learning, travel, and life. In my free time, I enjoy reading, and spending time with my family.
    
    My favorite hobby is photography. I love the process of taking photos, the way the results look, the feeling of the camera, the experience of being in the moment, and the joy of sharing my photos with others. 
    
    My second hobby is learning languages. I am excited to learn new languages, as they can open up opportunities for travel, job opportunities, and a deeper understanding of the world. I enjoy
    ===============================
    Prompt: The president of the United States is
    Generated text:  5 feet 5 inches tall. If someone is taller than the president and the president is 6 feet and 3 inches tall, how tall is the person?
    To determine the height of the person, we need to add the height of the president to the height of the person who is taller than the president. We will convert all measurements to the same unit for easy addition.
    
    First, let's convert the president's height from feet and inches to just inches:
    - The president's height is 5 feet 5 inches.
    - Since there are 12 inches in a foot, we can convert 5 feet to inches by
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. It’s the oldest capital of the European Union, and it was also the capital of the former French Empire from 1871 to 1870. After the re-establishment of the French Republic, it was renamed as “Palais Garnier” (garden of kings) in 1878 by Emperor Napoleon III. In 1990, it became the capital of a new autonomous region called the Île-de-France.
    In the 19th century, Paris was the capital of the French Empire. It was the largest and most populous city in the world during the 
    ===============================
    Prompt: The future of AI is
    Generated text:  moving forward in a fast-paced and exciting way, and the technology is evolving at a remarkable rate. Here are some of the most exciting developments in AI today that could potentially impact society in the near future:
    
    1. Natural Language Processing (NLP): This technology is already being used for a wide range of applications, including voice recognition, language translation, and chatbots. In the future, we can expect to see even more advanced NLP that can understand and generate human-like language.
    
    2. Computer Vision: This technology is being used for a variety of applications, including image and video processing, autonomous vehicles, and facial recognition. In the


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


    Generated text:  Paris, located in the south of the country and is the largest city in Europe by population. It is known for its rich history, art, and culture, and is home to many famous landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. Paris is also a major center for business, finance, and tourism, and is a popular destination for tourists and locals alike. The city is known for its vibrant nightlife, delicious cuisine, and diverse cultural scene. Paris is a city of contrasts, with its modern architecture and historical landmarks blending seamlessly into the city's vibrant atmosphere. The city is also home to
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn and adapt to human behavior and preferences. This could lead to more sophisticated and personalized AI systems that can better understand and respond to human needs.
    
    2. Enhanced ethical considerations: As AI becomes more integrated with human intelligence, there will be increased scrutiny of its ethical implications. This could lead to more stringent regulations and guidelines for AI development and deployment.
    
    3. Greater reliance on AI for decision-making: AI is likely to become more integrated with human decision-making processes, allowing machines to make more
    


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
    Generated text:  [name] and I'm a [job title] at [company name]. I have over [number of years of experience] years of experience in [specific field or industry]. What inspired you to pursue this career path? My passion for [job title] is fueled by [brief, positive or negative reasons]. What do you enjoy doing the most? I enjoy [mention activities or hobbies you enjoy]. What do you hope to achieve with this position? I hope to [mention your goals or aspirations for this position]. How do you stay motivated? I maintain a healthy work-life balance, [mention a positive action to maintain motivation]. What
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, the Louvre Museum, and Notre Dame Cathedral. 
    
    Paris, France's political, economic, and cultural capital, is known for its rich history, vibrant art scene, and diverse cultural scene. It's home to the European Parliament and the headquarters of many major French companies. 
    
    Paris is also famous for its annual Eiffel Tower, which attracts millions of visitors every year. The city is home to numerous museums, museums, and art galleries, such as the Musée d'Orsay and the Musée Rodin. 
    
    In addition to its
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  shaped by a number of factors, including advances in computing power, developments in machine learning, and changes in society and technology. Here are some possible future trends in AI that are currently being explored and debated:
    
    1. Improved accuracy and reliability: As AI systems become more sophisticated, they are likely to become even more accurate and reliable in their predictions and decisions. This could lead to a wider range of applications for AI, from healthcare and finance to transportation and manufacturing.
    
    2. Enhanced cognitive ability: As AI systems become more capable of learning and adapting, they may be able to perform tasks that were previously thought impossible, such as understanding and predicting human


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

     name

    ].

     I

     am

     [

    insert

     profession

     or

     role

    ],

     and

     I

     enjoy

     [

    insert

     hobbies

     or

     interests

    ].

     I

     love

     spending

     my

     free

     time

     [

    insert

     activities

    ],

     and

     I

    'm

     always

     eager

     to

     learn

     new

     things

     and

     expand

     my

     knowledge

    .
    


    In

     my

     free

     time

    ,

     I

     enjoy

     [

    insert

     interests

     or

     hobbies

    ],

     and

     I

     like

     to

     read

     [

    insert

     books

     or

     articles

    ]

     and

     watch

     [

    insert

     movies

     or

     shows

    ].

     I

     also

     enjoy

     [

    insert

     sports

     or

     activities

    ],

     and

     I

     love

     to

     [

    insert

     favorite

     things

    ].

     I

     am

     [

    insert

     personality

     trait

     or

     quality

    ],

     and

     I

     am

     always

     friendly

    ,

     helpful

    ,

     and

     kind

    .
    


    Overall

    ,

     I

     am

     a

     [

    insert

     occupation

     or

     title

    ],

     and

     I

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .
    


    Paris

     is

     the

     capital

     city

     of

     France

    .

     It

     is

     located

     on

     the

     north

     bank

     of

     the

     Se

    ine

     River

    ,

     in

     the

     Paris

     Basin

    ,

     and

     is

     a

     major

     cultural

    ,

     economic

    ,

     and

     political

     center

     in

     Western

     Europe

    .

     It

     was

     founded

     in

     the

     

    7

    th

     century

     as

     the

     capital

     of

     the

     Mer

    oving

    ian

     Kingdom

     and

     became

     the

     capital

     of

     France

     after

     the

     fall

     of

     the

     Carol

    ing

    ian

     Empire

     in

     the

     

    8

    th

     century

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

     the

     Lou

    vre

     Museum

    ,

     the

     Notre

    -D

    ame

     Cathedral

    ,

     the

     Lou

    vre

     Museum

    ,

     and

     numerous

     museums

     and

     art

     galleries

    .

     It

     is

     also

     the

     center

     of

     the

     French

     language

    ,

     literature

    ,

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     looking

     increasingly

     positive

    ,

     with

     many

     potential

     applications

     that

     could

     change

     the

     way

     we

     live

     and

     work

    .

     Here

     are

     some

     possible

     trends

     in

     AI

     that

     are

     likely

     to

     shape

     the

     future

    :
    


    1

    .

     Increased

     accessibility

     and

     affordability

    :

     As

     AI

     technologies

     continue

     to

     improve

    ,

     it

     is

     likely

     that

     the

     cost

     and

     accessibility

     of

     AI

     systems

     will

     decrease

    ,

     making

     them

     more

     accessible

     to

     individuals

     and

     businesses

    .

     This

     will

     make

     AI

     a

     more

     powerful

     tool

     for

     businesses

     and

     individuals

     alike

    ,

     and

     could

     lead

     to

     increased

     adoption

     of

     AI

     in

     various

     industries

    .
    


    2

    .

     Increased

     integration

     with

     other

     technologies

    :

     As

     AI

     technology

     continues

     to

     advance

    ,

     it

     is

     likely

     that

     it

     will

     become

     more

     integrated

     with

     other

     technologies

    ,

     such

     as

     machine

     learning

    



```python
llm.shutdown()
```
