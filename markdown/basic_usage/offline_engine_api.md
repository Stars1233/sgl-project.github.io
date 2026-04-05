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


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.81it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.79it/s]


    2026-04-05 07:40:28,288 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-05 07:40:28] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:37,  2.76s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:37,  2.76s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:37,  2.76s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:37,  2.76s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:29,  1.82it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:29,  1.82it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:29,  1.82it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:29,  1.82it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:29,  1.82it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:29,  1.82it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:29,  1.82it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.59it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.59it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.59it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:03<00:08,  5.59it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:03<00:08,  5.59it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:03<00:08,  5.59it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:03<00:08,  5.59it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:03<00:08,  5.59it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:03<00:08,  5.59it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:03, 11.81it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:03, 11.81it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:03, 11.81it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:03, 11.81it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:03, 11.81it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:03<00:03, 11.81it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:03<00:03, 11.81it/s]

    Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:03<00:03, 11.81it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 17.87it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 17.87it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 17.87it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 17.87it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 17.87it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 17.87it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 17.87it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:03<00:01, 17.87it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 24.55it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 24.55it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 24.55it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 24.55it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 24.55it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 24.55it/s]

    Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 24.55it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 29.63it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 29.63it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 29.63it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 29.63it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 29.63it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 29.63it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 29.63it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 34.85it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 34.85it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 34.85it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 34.85it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 34.85it/s]

    Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 34.85it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 34.85it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 38.29it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 38.29it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 38.29it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 38.29it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 38.29it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 38.29it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 38.29it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 38.29it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 38.29it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.52it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.51 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.32 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.32 GB):   3%|▎         | 2/58 [00:00<00:03, 16.35it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.32 GB):   3%|▎         | 2/58 [00:00<00:03, 16.35it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.31 GB):   3%|▎         | 2/58 [00:00<00:03, 16.35it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=118.32 GB):   3%|▎         | 2/58 [00:00<00:03, 16.35it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.32 GB):   9%|▊         | 5/58 [00:00<00:02, 19.96it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.31 GB):   9%|▊         | 5/58 [00:00<00:02, 19.96it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.31 GB):   9%|▊         | 5/58 [00:00<00:02, 19.96it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.31 GB):   9%|▊         | 5/58 [00:00<00:02, 19.96it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.31 GB):  14%|█▍        | 8/58 [00:00<00:02, 23.65it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.31 GB):  14%|█▍        | 8/58 [00:00<00:02, 23.65it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.30 GB):  14%|█▍        | 8/58 [00:00<00:02, 23.65it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=118.30 GB):  14%|█▍        | 8/58 [00:00<00:02, 23.65it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.30 GB):  14%|█▍        | 8/58 [00:00<00:02, 23.65it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.30 GB):  21%|██        | 12/58 [00:00<00:01, 28.90it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.29 GB):  21%|██        | 12/58 [00:00<00:01, 28.90it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.29 GB):  21%|██        | 12/58 [00:00<00:01, 28.90it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.29 GB):  21%|██        | 12/58 [00:00<00:01, 28.90it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.29 GB):  21%|██        | 12/58 [00:00<00:01, 28.90it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.28 GB):  21%|██        | 12/58 [00:00<00:01, 28.90it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.28 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.47it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.24 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.47it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.24 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.47it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=118.24 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.47it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.22 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.47it/s]Capturing num tokens (num_tokens=960 avail_mem=118.23 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.47it/s] Capturing num tokens (num_tokens=960 avail_mem=118.23 GB):  38%|███▊      | 22/58 [00:00<00:01, 35.74it/s]Capturing num tokens (num_tokens=896 avail_mem=118.23 GB):  38%|███▊      | 22/58 [00:00<00:01, 35.74it/s]Capturing num tokens (num_tokens=832 avail_mem=118.22 GB):  38%|███▊      | 22/58 [00:00<00:01, 35.74it/s]Capturing num tokens (num_tokens=768 avail_mem=118.22 GB):  38%|███▊      | 22/58 [00:00<00:01, 35.74it/s]Capturing num tokens (num_tokens=704 avail_mem=118.22 GB):  38%|███▊      | 22/58 [00:00<00:01, 35.74it/s]Capturing num tokens (num_tokens=640 avail_mem=118.21 GB):  38%|███▊      | 22/58 [00:00<00:01, 35.74it/s]

    Capturing num tokens (num_tokens=640 avail_mem=118.21 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.11it/s]Capturing num tokens (num_tokens=576 avail_mem=118.21 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.11it/s]Capturing num tokens (num_tokens=512 avail_mem=118.20 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.11it/s]Capturing num tokens (num_tokens=480 avail_mem=118.22 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.11it/s]Capturing num tokens (num_tokens=448 avail_mem=118.22 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.11it/s]Capturing num tokens (num_tokens=416 avail_mem=118.21 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.11it/s]Capturing num tokens (num_tokens=416 avail_mem=118.21 GB):  55%|█████▌    | 32/58 [00:00<00:00, 40.23it/s]Capturing num tokens (num_tokens=384 avail_mem=118.21 GB):  55%|█████▌    | 32/58 [00:00<00:00, 40.23it/s]Capturing num tokens (num_tokens=352 avail_mem=118.21 GB):  55%|█████▌    | 32/58 [00:00<00:00, 40.23it/s]Capturing num tokens (num_tokens=320 avail_mem=118.20 GB):  55%|█████▌    | 32/58 [00:00<00:00, 40.23it/s]Capturing num tokens (num_tokens=288 avail_mem=118.20 GB):  55%|█████▌    | 32/58 [00:01<00:00, 40.23it/s]

    Capturing num tokens (num_tokens=256 avail_mem=118.20 GB):  55%|█████▌    | 32/58 [00:01<00:00, 40.23it/s]Capturing num tokens (num_tokens=256 avail_mem=118.20 GB):  64%|██████▍   | 37/58 [00:01<00:00, 40.12it/s]Capturing num tokens (num_tokens=240 avail_mem=118.19 GB):  64%|██████▍   | 37/58 [00:01<00:00, 40.12it/s]Capturing num tokens (num_tokens=224 avail_mem=118.19 GB):  64%|██████▍   | 37/58 [00:01<00:00, 40.12it/s]Capturing num tokens (num_tokens=208 avail_mem=118.19 GB):  64%|██████▍   | 37/58 [00:01<00:00, 40.12it/s]Capturing num tokens (num_tokens=192 avail_mem=118.19 GB):  64%|██████▍   | 37/58 [00:01<00:00, 40.12it/s]Capturing num tokens (num_tokens=176 avail_mem=118.18 GB):  64%|██████▍   | 37/58 [00:01<00:00, 40.12it/s]Capturing num tokens (num_tokens=176 avail_mem=118.18 GB):  72%|███████▏  | 42/58 [00:01<00:00, 40.70it/s]Capturing num tokens (num_tokens=160 avail_mem=118.18 GB):  72%|███████▏  | 42/58 [00:01<00:00, 40.70it/s]Capturing num tokens (num_tokens=144 avail_mem=118.17 GB):  72%|███████▏  | 42/58 [00:01<00:00, 40.70it/s]Capturing num tokens (num_tokens=128 avail_mem=118.17 GB):  72%|███████▏  | 42/58 [00:01<00:00, 40.70it/s]

    Capturing num tokens (num_tokens=112 avail_mem=118.17 GB):  72%|███████▏  | 42/58 [00:01<00:00, 40.70it/s]Capturing num tokens (num_tokens=96 avail_mem=118.16 GB):  72%|███████▏  | 42/58 [00:01<00:00, 40.70it/s] Capturing num tokens (num_tokens=96 avail_mem=118.16 GB):  81%|████████  | 47/58 [00:01<00:00, 39.66it/s]Capturing num tokens (num_tokens=80 avail_mem=118.16 GB):  81%|████████  | 47/58 [00:01<00:00, 39.66it/s]Capturing num tokens (num_tokens=64 avail_mem=118.16 GB):  81%|████████  | 47/58 [00:01<00:00, 39.66it/s]Capturing num tokens (num_tokens=48 avail_mem=118.16 GB):  81%|████████  | 47/58 [00:01<00:00, 39.66it/s]Capturing num tokens (num_tokens=32 avail_mem=118.15 GB):  81%|████████  | 47/58 [00:01<00:00, 39.66it/s]Capturing num tokens (num_tokens=28 avail_mem=118.15 GB):  81%|████████  | 47/58 [00:01<00:00, 39.66it/s]Capturing num tokens (num_tokens=28 avail_mem=118.15 GB):  90%|████████▉ | 52/58 [00:01<00:00, 40.74it/s]Capturing num tokens (num_tokens=24 avail_mem=118.14 GB):  90%|████████▉ | 52/58 [00:01<00:00, 40.74it/s]Capturing num tokens (num_tokens=20 avail_mem=118.14 GB):  90%|████████▉ | 52/58 [00:01<00:00, 40.74it/s]

    Capturing num tokens (num_tokens=16 avail_mem=118.14 GB):  90%|████████▉ | 52/58 [00:01<00:00, 40.74it/s]Capturing num tokens (num_tokens=12 avail_mem=118.13 GB):  90%|████████▉ | 52/58 [00:01<00:00, 40.74it/s]Capturing num tokens (num_tokens=8 avail_mem=118.13 GB):  90%|████████▉ | 52/58 [00:01<00:00, 40.74it/s] Capturing num tokens (num_tokens=8 avail_mem=118.13 GB):  98%|█████████▊| 57/58 [00:01<00:00, 42.48it/s]Capturing num tokens (num_tokens=4 avail_mem=118.13 GB):  98%|█████████▊| 57/58 [00:01<00:00, 42.48it/s]Capturing num tokens (num_tokens=4 avail_mem=118.13 GB): 100%|██████████| 58/58 [00:01<00:00, 37.23it/s]


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
    Generated text:  Darryl Smith, and I'm a retired high school history teacher. I teach English literature, and I've done this since 1977.
    
    I have over 30 years of experience teaching in high school English literature, and I love my job. I have a passion for literature, and I teach it with enthusiasm. I have always been an avid reader, and I love sharing my knowledge through my classes and my writings.
    
    I have a Ph. D. in English Literature from the University of Chicago. I was a faculty member at the University of Chicago for 20 years, and I have taught there for 
    ===============================
    Prompt: The president of the United States is
    Generated text:  a man. Which of the following options is the most logical and coherent way to complete this sentence?
    A) An American woman
    B) A member of the executive branch of the U. S. government
    C) A member of Congress
    D) A member of the judicial branch of the U. S. government
    E) A member of the military
    The answer is A) An American woman. In the U. S. government, a president is typically appointed by the president of the United States, not by a woman. Therefore, the president cannot be a woman, and she would not be a member of the executive branch
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris, and it is one of the most important cities in Europe. The city has many historical sites and attractions, including the Notre-Dame Cathedral, the Eiffel Tower, and the Louvre Museum.
    Paris is also famous for its fashion industry, including couturiers like Yves Saint Laurent and Dior, as well as the iconic French bakery, Le Bon Appétit.
    Paris, France, is also known as the City of Light, as it is noted for its vibrant and colorful streets and buildings, which are decorated with colorful lights and fabrics.
    Paris is also known for its rich and complex history, which can be seen
    ===============================
    Prompt: The future of AI is
    Generated text:  uncertain, and there are many different types of AI that are used for various tasks. However, none of these are perfect and some of them are not yet fully developed. In this article, we will explore some of the potential areas of development and the different types of AI that have been developed.
    
    One of the most widely used types of AI is machine learning. Machine learning is a type of AI that uses algorithms to learn patterns in data and make predictions or decisions. The goal of machine learning is to create systems that can learn from data and improve their performance over time.
    
    Another type of AI that is gaining popularity is natural language processing (N


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


    Generated text:  Paris, which is known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and Louvre Museum. It is also home to the French Parliament and the French Parliament building. Paris is a cultural and historical center with a rich history dating back to ancient times. It is a major transportation hub and a major tourist destination. The city is known for its cuisine, fashion, and art. Paris is a city of contrasts, with its modern architecture and historical landmarks blending together seamlessly. It is a city that is constantly evolving and changing, with new developments and attractions being added to the city's skyline. Paris is a
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction. Here are some of the most likely trends:
    
    1. Increased focus on ethical considerations: As AI becomes more integrated into our daily lives, there will be a growing emphasis on ethical considerations. This will include issues such as bias, transparency, accountability, and privacy.
    
    2. Greater integration with other technologies: AI is likely to become more integrated with other technologies, such as machine learning, natural language processing, and computer vision. This will allow for more sophisticated and personalized applications of AI.
    
    3. Increased use of AI in healthcare: AI is already being
    


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
    Generated text:  [Name], and I work at [Company Name]. I am a passionate [career goal or skill] and I am always looking for ways to improve myself. I enjoy [job-related activity or hobby] and I strive to be a positive role model for my colleagues. I am confident in my abilities and I am eager to contribute to [career goal or company] in whatever way I can. Thank you. [Name] [Company Name] [Date] You have entered the realm of mysterious internet forums, where the vast spectrum of interests diverges like a coral reef. You're here, a soul searching, data-driven individual who seeks
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    Paris is the largest city and the capital of France. It is a cultural and political center of the country. It is located in the north of the country and is the largest metropolitan area in Europe by population. Paris is known for its art, literature, and cuisine, as well as its long history and architecture. The city is famous for its Eiffel Tower and Notre-Dame Cathedral, as well as for its annual festivals and events. Paris is also the birthplace of many important figures in history, including Napoleon Bonaparte and Frédéric Chopin. Overall, Paris is a fascinating city with a rich cultural heritage
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  exciting and diverse, with numerous possibilities and predictions. Some possible trends include:
    
    1. Increased integration with human intelligence: AI is already becoming more integrated with human intelligence, thanks to advancements in machine learning and natural language processing. It's likely that we'll see more AI systems that can mimic human decision-making processes and social behavior.
    
    2. Emergence of more advanced models: As AI technology continues to evolve, we may see the emergence of more advanced models that are able to learn from more complex data and adapt to new situations.
    
    3. Expansion of AI applications: AI is already being used in a variety of fields, from healthcare and finance to


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

    ]

     and

     I

     am

     a

     [

    insert

     profession

     or

     role

    ]

     who

     has

     been

     [

    insert

     a

     brief

     history

     of

     your

     journey

     so

     far

    ]

     and

     [

    insert

     a

     brief

     summary

     of

     your

     personal

     background

    ].

     My

     [

    insert

     a

     specific

     skill

     or

     attribute

    ]

     is

     [

    insert

     the

     specific

     skill

     or

     attribute

    ],

     which

     [

    insert

     a

     personal

     description

     of

     how

     you

     got

     to

     this

     position

    ].

     I

     am

     always

     looking

     for

     ways

     to

     [

    insert

     how

     you

     are

     using

     your

     skills

    /

    abilities

     to

     help

     others

    ].

     I

     have

     a

     broad

     range

     of

     interests

     and

     hobbies

    ,

     [

    insert

     a

     specific

     example

     of

     an

     interest

     or

     hobby

    ].

     I

     am

     always

     up

     for

     trying

     new

     things

     and

     [

    insert

     a

     personal

     preference

     about

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     the

     largest

     and

     most

     populous

     city

     in

     Europe

    .


    You

     are

     an

     AI

     assistant

     that

     helps

     you

     understand

     the

     logic

     behind

     programs

    .

     No

     embeddings

     are

     needed

    .

     Don

    't

     write

     outputs

    .
    


    Here

     is

     a

     Python

     function

     that

     uses

     a

     dictionary

     to

     find

     the

     capital

     of

     France

     and

     a

     list

     of

     cities

     in

     France

    .

     The

     function

     converts

     the

     list

     of

     cities

     into

     a

     dictionary

     and

     then

     returns

     the

     capital

     city

    's

     name

    .

     Use

     this

     function

     to

     check

     your

     answer

    .


    ```

    python

    


    def

     find

    _cap

    ital

    ():


       

     return

     "

    Paris

    "
    


    capital

     =

     find

    _cap

    ital

    ()


    print

    (f

    "The

     capital

     of

     France

     is

     {

    capital

    }.

    ")


    ```

     Here

    ,

     the

     function

     `

    find

    _cap

    ital

    `

     is

     called

    ,

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     vast

     and

     rapidly

     evolving

    ,

     with

     many

     potential

     directions

     and

     trends

     shaping

     its

     direction

     and

     impact

     on

     society

    ,

     economy

    ,

     and

     our

     daily

     lives

    .
    


    One

     of

     the

     most

     exciting

     and

     promising

     areas

     of

     AI

     development

     is

     the

     field

     of

     natural

     language

     processing

     (

    N

    LP

    ).

     This

     involves

     the

     development

     of

     advanced

     algorithms

     and

     machine

     learning

     techniques

     that

     allow

     computers

     to

     understand

    ,

     process

    ,

     generate

    ,

     and

     interpret

     human

     language

    .

     The

     potential

     applications

     of

     N

    LP

     are

     vast

    ,

     from

     chat

    bots

     and

     virtual

     assistants

     that

     can

     assist

     with

     everyday

     tasks

    ,

     to

     translation

     and

     interpretation

     services

     that

     can

     help

     people

     communicate

     across

     languages

     and

     cultures

    .
    


    Another

     area

     of

     growing

     interest

     is

     the

     development

     of

     AI

     that

     can

     perform

     tasks

     more

     accurately

     and

    



```python
llm.shutdown()
```
