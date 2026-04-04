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


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.76it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.74it/s]


    2026-04-04 04:43:57,203 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 04:43:57] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:34,  2.71s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:34,  2.71s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:34,  2.71s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:34,  2.71s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:34,  2.71s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:22,  2.33it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:22,  2.33it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:22,  2.33it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:22,  2.33it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:22,  2.33it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:22,  2.33it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:22,  2.33it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:07,  6.14it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:07,  6.14it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:07,  6.14it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:07,  6.14it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:07,  6.14it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:07,  6.14it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:02<00:07,  6.14it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:03<00:07,  6.14it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:03<00:07,  6.14it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:03, 12.43it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:03, 12.43it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:03, 12.43it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:03, 12.43it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:03, 12.43it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:03, 12.43it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:03, 12.43it/s]

    Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:03, 12.43it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 18.48it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 18.48it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 18.48it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 18.48it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 18.48it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 18.48it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:03<00:01, 18.48it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:03<00:01, 18.48it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:00, 25.09it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:00, 25.09it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:00, 25.09it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:00, 25.09it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:00, 25.09it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:00, 25.09it/s]

    Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:03<00:00, 25.09it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 30.11it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 30.11it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 30.11it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 30.11it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 30.11it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 30.11it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 30.11it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 34.75it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 34.75it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 34.75it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 34.75it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 34.75it/s]

    Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 34.75it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 34.75it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 38.78it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 38.78it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 38.78it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 38.78it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 38.78it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 38.78it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 38.78it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 38.78it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.77it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=119.08 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=119.05 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=119.05 GB):   3%|▎         | 2/58 [00:00<00:03, 18.15it/s]Capturing num tokens (num_tokens=7168 avail_mem=119.02 GB):   3%|▎         | 2/58 [00:00<00:03, 18.15it/s]Capturing num tokens (num_tokens=6656 avail_mem=119.02 GB):   3%|▎         | 2/58 [00:00<00:03, 18.15it/s]Capturing num tokens (num_tokens=6144 avail_mem=119.02 GB):   3%|▎         | 2/58 [00:00<00:03, 18.15it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=119.02 GB):   9%|▊         | 5/58 [00:00<00:02, 21.76it/s]Capturing num tokens (num_tokens=5632 avail_mem=119.01 GB):   9%|▊         | 5/58 [00:00<00:02, 21.76it/s]Capturing num tokens (num_tokens=5120 avail_mem=119.02 GB):   9%|▊         | 5/58 [00:00<00:02, 21.76it/s]Capturing num tokens (num_tokens=4608 avail_mem=119.01 GB):   9%|▊         | 5/58 [00:00<00:02, 21.76it/s]Capturing num tokens (num_tokens=4096 avail_mem=119.01 GB):   9%|▊         | 5/58 [00:00<00:02, 21.76it/s]Capturing num tokens (num_tokens=4096 avail_mem=119.01 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.58it/s]Capturing num tokens (num_tokens=3840 avail_mem=119.01 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.58it/s]Capturing num tokens (num_tokens=3584 avail_mem=119.00 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.58it/s]Capturing num tokens (num_tokens=3328 avail_mem=119.00 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.58it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=118.97 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.58it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.97 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.58it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.97 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.18it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.96 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.18it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.96 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.18it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.95 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.18it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.95 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.18it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.95 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.18it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.95 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.34it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.94 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.34it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.92 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.34it/s]

    Capturing num tokens (num_tokens=960 avail_mem=118.94 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.34it/s] Capturing num tokens (num_tokens=896 avail_mem=118.93 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.34it/s]Capturing num tokens (num_tokens=832 avail_mem=118.93 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.34it/s]Capturing num tokens (num_tokens=832 avail_mem=118.93 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.29it/s]Capturing num tokens (num_tokens=768 avail_mem=118.93 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.29it/s]Capturing num tokens (num_tokens=704 avail_mem=118.92 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.29it/s]Capturing num tokens (num_tokens=640 avail_mem=118.92 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.29it/s]Capturing num tokens (num_tokens=576 avail_mem=118.92 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.29it/s]Capturing num tokens (num_tokens=512 avail_mem=118.91 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.29it/s]Capturing num tokens (num_tokens=512 avail_mem=118.91 GB):  50%|█████     | 29/58 [00:00<00:00, 41.29it/s]Capturing num tokens (num_tokens=480 avail_mem=118.92 GB):  50%|█████     | 29/58 [00:00<00:00, 41.29it/s]Capturing num tokens (num_tokens=448 avail_mem=118.92 GB):  50%|█████     | 29/58 [00:00<00:00, 41.29it/s]

    Capturing num tokens (num_tokens=416 avail_mem=118.92 GB):  50%|█████     | 29/58 [00:00<00:00, 41.29it/s]Capturing num tokens (num_tokens=384 avail_mem=118.92 GB):  50%|█████     | 29/58 [00:00<00:00, 41.29it/s]Capturing num tokens (num_tokens=352 avail_mem=118.91 GB):  50%|█████     | 29/58 [00:00<00:00, 41.29it/s]Capturing num tokens (num_tokens=352 avail_mem=118.91 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.67it/s]Capturing num tokens (num_tokens=320 avail_mem=118.91 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.67it/s]Capturing num tokens (num_tokens=288 avail_mem=118.90 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.67it/s]Capturing num tokens (num_tokens=256 avail_mem=118.90 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.67it/s]Capturing num tokens (num_tokens=240 avail_mem=118.90 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.67it/s]Capturing num tokens (num_tokens=224 avail_mem=118.90 GB):  59%|█████▊    | 34/58 [00:01<00:00, 42.67it/s]Capturing num tokens (num_tokens=224 avail_mem=118.90 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.53it/s]Capturing num tokens (num_tokens=208 avail_mem=118.89 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.53it/s]

    Capturing num tokens (num_tokens=192 avail_mem=118.89 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.53it/s]Capturing num tokens (num_tokens=176 avail_mem=118.89 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.53it/s]Capturing num tokens (num_tokens=160 avail_mem=118.89 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.53it/s]Capturing num tokens (num_tokens=144 avail_mem=118.88 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.53it/s]Capturing num tokens (num_tokens=144 avail_mem=118.88 GB):  76%|███████▌  | 44/58 [00:01<00:00, 35.92it/s]Capturing num tokens (num_tokens=128 avail_mem=118.88 GB):  76%|███████▌  | 44/58 [00:01<00:00, 35.92it/s]

    Capturing num tokens (num_tokens=112 avail_mem=118.88 GB):  76%|███████▌  | 44/58 [00:01<00:00, 35.92it/s]Capturing num tokens (num_tokens=96 avail_mem=118.87 GB):  76%|███████▌  | 44/58 [00:01<00:00, 35.92it/s] Capturing num tokens (num_tokens=80 avail_mem=118.87 GB):  76%|███████▌  | 44/58 [00:01<00:00, 35.92it/s]Capturing num tokens (num_tokens=80 avail_mem=118.87 GB):  83%|████████▎ | 48/58 [00:01<00:00, 29.16it/s]

    Capturing num tokens (num_tokens=64 avail_mem=118.87 GB):  83%|████████▎ | 48/58 [00:01<00:00, 29.16it/s]Capturing num tokens (num_tokens=48 avail_mem=118.83 GB):  83%|████████▎ | 48/58 [00:01<00:00, 29.16it/s]

    Capturing num tokens (num_tokens=32 avail_mem=118.82 GB):  83%|████████▎ | 48/58 [00:01<00:00, 29.16it/s]Capturing num tokens (num_tokens=28 avail_mem=118.82 GB):  83%|████████▎ | 48/58 [00:01<00:00, 29.16it/s]Capturing num tokens (num_tokens=28 avail_mem=118.82 GB):  90%|████████▉ | 52/58 [00:01<00:00, 20.14it/s]Capturing num tokens (num_tokens=24 avail_mem=118.82 GB):  90%|████████▉ | 52/58 [00:01<00:00, 20.14it/s]Capturing num tokens (num_tokens=20 avail_mem=118.81 GB):  90%|████████▉ | 52/58 [00:01<00:00, 20.14it/s]Capturing num tokens (num_tokens=16 avail_mem=118.81 GB):  90%|████████▉ | 52/58 [00:01<00:00, 20.14it/s]Capturing num tokens (num_tokens=12 avail_mem=118.81 GB):  90%|████████▉ | 52/58 [00:01<00:00, 20.14it/s]Capturing num tokens (num_tokens=8 avail_mem=118.80 GB):  90%|████████▉ | 52/58 [00:01<00:00, 20.14it/s] Capturing num tokens (num_tokens=8 avail_mem=118.80 GB):  98%|█████████▊| 57/58 [00:01<00:00, 24.26it/s]Capturing num tokens (num_tokens=4 avail_mem=118.80 GB):  98%|█████████▊| 57/58 [00:01<00:00, 24.26it/s]

    Capturing num tokens (num_tokens=4 avail_mem=118.80 GB): 100%|██████████| 58/58 [00:01<00:00, 29.86it/s]


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
    Generated text:  Zade and I'm a full-stack web developer. I specialize in building custom solutions for all types of businesses. I have a wealth of experience with React, Node.js, and a great deal of experience with building custom web applications. I can help with a variety of types of projects, including front-end and back-end development, API integration, and project management. I'm open to working with a wide range of clients and industries and enjoy the challenge of building custom solutions for a wide variety of businesses. Back to my profile! Sure, could you provide some examples of projects or areas of expertise that you specialize in and how your skills can
    ===============================
    Prompt: The president of the United States is
    Generated text:  34 years older than the president of Brazil. The president of Brazil is 2 times older than the president of Mexico. If the president of Mexico is currently 20 years old, how old will the president of Mexico be in 10 years?
    To determine the current age of the president of Mexico, we start by noting the relationship between the ages of the presidents of the United States and Brazil. According to the problem, the president of Brazil is 2 times older than the president of Mexico. If the president of Mexico is currently 20 years old, then the president of Brazil is:
    
    \[ 2 \times
    ===============================
    Prompt: The capital of France is
    Generated text: :
    A. Paris
    B. Berlin
    C. London
    D. Madrid
    
    A. Paris is the capital of France. The name "Paris" comes from the Latin word "Parisus," which means "the place of the river."
    
    Some interesting facts about Paris:
    - It is the largest city in France by population.
    - It has the world's second-largest metro system.
    - It is the world's most populous city by land area.
    - It is known for its iconic Eiffel Tower and Louvre Museum.
    - It is a major cultural and financial center.
    
    The other options are not capitals of France:
    - Berlin is
    ===============================
    Prompt: The future of AI is
    Generated text:  predicated on increased data, machine learning, and the ability to develop and apply new technologies. When these technologies are used in a safe and ethical manner, they can be utilized to make the world a better place. However, when they are not, they can lead to harm. This is the case with artificial intelligence (AI) technology. AI is an incredibly powerful tool that can be used for good or bad. There are two types of AI that are being developed – one that is designed to help humans and the other that is designed to harm humans. This is a critical issue that needs to be addressed if we want to make the best


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [age] year old, and I have [number] years of experience in [industry]. I'm a [gender] and I have [number] children. I'm a [gender] and I have [number] children. I'm a [gender] and I have [number] children. I'm a [gender] and I have [number] children. I'm a [gender] and I have [number] children.
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as the City of Light, a historic city with a rich history dating back to the Roman Empire. It is the largest city in France and the second-largest city in the European Union, with a population of over 10 million people. Paris is known for its iconic landmarks such as the Eiffel Tower, Louvre Museum, Notre-Dame Cathedral, and the Palace of Versailles. The city is also home to many world-renowned museums, theaters, and restaurants. Paris is a cultural and artistic center, with a vibrant nightlife and a rich history of art, literature, and philosophy. It is a major
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn and adapt to human behavior and preferences. This could lead to more personalized and efficient AI systems.
    
    2. Enhanced ethical considerations: As AI becomes more advanced, there will be a growing need for ethical guidelines and regulations to ensure that AI systems are used in a responsible and ethical manner. This could lead to increased investment in ethical AI research and development.
    
    3. Greater reliance on machine learning: Machine learning is becoming increasingly important in AI, allowing machines to learn from data and make predictions and
    


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
    Generated text:  Emily. I am a friendly, down-to-earth young woman with a natural talent for storytelling. I enjoy working on projects that require creativity and imagination, and I thrive in a fast-paced, high-energy environment. I'm passionate about exploring new ideas and experimenting with different ways of thinking, and I'm always looking for ways to keep myself updated with the latest trends and technologies in the industry. If you're interested in learning more about me, feel free to let me know and I'll be happy to share some of my experiences and insights with you. #EmilySF #YoungSheerEyes #CreativeWoman #TechTalent #Industry
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, a vibrant city with a rich history and diverse culture. It is known for its magnificent Eiffel Tower, medieval Notre-Dame Cathedral, and romantic romantic architecture, including the Grand Palais and the Louvre Museum. Paris is also a major center for fashion, film, and music, and is recognized as a global cultural hub. Visitors to the city are drawn by its beautiful architecture, diverse food, and lively nightlife, which reflect the city's status as Europe's cultural capital. Additionally, Paris is a popular destination for tourists and expats seeking a unique cultural experience. As of 2021, Paris has over
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be a combination of rapid evolution and adaptation, driven by increasing complexity, enhanced computational power, and integration with various emerging technologies. Some key trends that are expected to shape AI's future include:
    
    1. Increased use of natural language processing: As more data is generated and analyzed, there will be an increased demand for natural language processing capabilities in AI systems. This will likely lead to greater adoption of conversational AI and chatbots.
    
    2. Enhanced security and privacy concerns: As AI becomes more ubiquitous, there will be increased need to address security and privacy concerns. This will require more robust security measures, such as encryption and biometric authentication


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

    'm

     a

     [

    age

    ]

     year

     old

     [

    occupation

    ].

     I

    'm

     currently

     [

    job

     title

    ]

     at

     [

    company

     name

    ].

     I

    'm

     excited

     to

     be

     here

     to

     help

     you

     with

     any

     questions

     you

     may

     have

    ,

     and

     I

    'm

     always

     ready

     to

     learn

     and

     grow

    .

     What

     can

     I

     help

     you

     with

     today

    ?

     I

     look

     forward

     to

     having

     the

     opportunity

     to

     work

     with

     you

    .

     What

     brings

     you

     to

     this

     place

    ?

     Hi

     there

    !

     I

    'm

     [

    Name

    ]

     and

     I

    'm

     a

     [

    age

    ]

     year

     old

     [

    occupation

    ].

     I

     have

     been

     [

    number

     of

     years

    ]

     in

     the

     field

     of

     [

    specific

     field

     of

     expertise

    ],

     and

     I

    'm

     looking

     for

     a

     new

     challenge

     and

     opportunity

    
    
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

     the

     eighth

    -largest

     city

     in

     the

     world

     by

     population

    .

     The

     city

     is

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

     iconic

     landmarks

     such

     as

     Notre

    -D

    ame

     Cathedral

    ,

     the

     E

    iff

    el

     Tower

    ,

     and

     the

     Lou

    vre

     Museum

    .

     Paris

     is

     home

     to

     many

     world

    -ren

    owned

     museums

    ,

     art

     galleries

    ,

     and

     cultural

     institutions

    ,

     as

     well

     as

     a

     vibrant

     music

     and

     art

     scene

     that

     attracts

     millions

     of

     visitors

     each

     year

    .

     The

     city

     is

     also

     the

     birth

    place

     of

     numerous

     influential

     figures

    ,

     including

     Napoleon

     Bon

    ap

    arte

     and

     Gust

    ave

     Fl

    aub

    ert

    ,

     and

     has

     played

     a

     major

     role

     in

     shaping

     French

     culture

     and

     politics

    .

     Paris

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     very

     exciting

     and

     promising

    .

     Here

     are

     some

     possible

     trends

     in

     AI

    :
    


    1

    .

     Increased

     accuracy

     and

     reliability

    :

     As

     AI

     becomes

     more

     advanced

    ,

     we

     can

     expect

     it

     to

     perform

     better

     and

     more

     accurately

    .

     This

     could

     lead

     to

     breakthrough

    s

     in

     healthcare

    ,

     transportation

    ,

     and

     manufacturing

    ,

     among

     others

    .
    


    2

    .

     Automation

    :

     AI

     is

     already

     being

     used

     to

     automate

     many

     tasks

    ,

     such

     as

     manufacturing

     and

     logistics

    .

     As

     AI

     continues

     to

     improve

    ,

     we

     can

     expect

     it

     to

     automate

     even

     more

     tasks

    ,

     potentially

     leading

     to

     even

     greater

     efficiency

     and

     productivity

    .
    


    3

    .

     Increased

     cognitive

     capabilities

    :

     AI

     is

     already

     capable

     of

     processing

     and

     analyzing

     large

     amounts

     of

     data

    ,

     but

     there

     is

     still

     much

     that

     we

    



```python
llm.shutdown()
```
