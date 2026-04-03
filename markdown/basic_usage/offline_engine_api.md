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


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.85it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.83it/s]


    2026-04-03 23:39:33,765 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 23:39:33] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:32,  2.68s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:32,  2.68s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:32,  2.68s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:32,  2.68s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:32,  2.68s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:22,  2.35it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:22,  2.35it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:22,  2.35it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:22,  2.35it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:22,  2.35it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:22,  2.35it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:22,  2.35it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:07,  6.18it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:07,  6.18it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:07,  6.18it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:07,  6.18it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:07,  6.18it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:07,  6.18it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:02<00:07,  6.18it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:02<00:07,  6.18it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:02<00:07,  6.18it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:03, 12.53it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:03, 12.53it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:03, 12.53it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:03, 12.53it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:03, 12.53it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:03, 12.53it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:03, 12.53it/s]

    Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:03, 12.53it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 18.63it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 18.63it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 18.63it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 18.63it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 18.63it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 18.63it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:03<00:01, 18.63it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:03<00:01, 18.63it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:00, 25.28it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:00, 25.28it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:00, 25.28it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:00, 25.28it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:00, 25.28it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:00, 25.28it/s]

    Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:03<00:00, 25.28it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 30.33it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 30.33it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 30.33it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 30.33it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 30.33it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 30.33it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 30.33it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 35.01it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 35.01it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 35.01it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 35.01it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 35.01it/s]

    Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 35.01it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 35.01it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 39.05it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 39.05it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 39.05it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 39.05it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 39.05it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 39.05it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 39.05it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 39.05it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.88it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=121.06 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.73 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:03, 18.40it/s]Capturing num tokens (num_tokens=7168 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:03, 18.40it/s]Capturing num tokens (num_tokens=6656 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:03, 18.40it/s]Capturing num tokens (num_tokens=6144 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:03, 18.40it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=120.73 GB):   9%|▊         | 5/58 [00:00<00:02, 21.20it/s]Capturing num tokens (num_tokens=5632 avail_mem=120.72 GB):   9%|▊         | 5/58 [00:00<00:02, 21.20it/s]Capturing num tokens (num_tokens=5120 avail_mem=120.72 GB):   9%|▊         | 5/58 [00:00<00:02, 21.20it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.72 GB):   9%|▊         | 5/58 [00:00<00:02, 21.20it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.72 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.35it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.72 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.35it/s]Capturing num tokens (num_tokens=3840 avail_mem=120.72 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.35it/s]Capturing num tokens (num_tokens=3584 avail_mem=120.71 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.35it/s]Capturing num tokens (num_tokens=3328 avail_mem=120.71 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.35it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=120.71 GB):  21%|██        | 12/58 [00:00<00:01, 29.69it/s]Capturing num tokens (num_tokens=3072 avail_mem=120.70 GB):  21%|██        | 12/58 [00:00<00:01, 29.69it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.70 GB):  21%|██        | 12/58 [00:00<00:01, 29.69it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.68 GB):  21%|██        | 12/58 [00:00<00:01, 29.69it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.37 GB):  21%|██        | 12/58 [00:00<00:01, 29.69it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.37 GB):  28%|██▊       | 16/58 [00:00<00:01, 29.13it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.27 GB):  28%|██▊       | 16/58 [00:00<00:01, 29.13it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.26 GB):  28%|██▊       | 16/58 [00:00<00:01, 29.13it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=120.26 GB):  28%|██▊       | 16/58 [00:00<00:01, 29.13it/s]Capturing num tokens (num_tokens=1536 avail_mem=120.26 GB):  33%|███▎      | 19/58 [00:00<00:01, 28.43it/s]Capturing num tokens (num_tokens=1280 avail_mem=120.26 GB):  33%|███▎      | 19/58 [00:00<00:01, 28.43it/s]Capturing num tokens (num_tokens=1024 avail_mem=120.24 GB):  33%|███▎      | 19/58 [00:00<00:01, 28.43it/s]Capturing num tokens (num_tokens=960 avail_mem=120.25 GB):  33%|███▎      | 19/58 [00:00<00:01, 28.43it/s] 

    Capturing num tokens (num_tokens=960 avail_mem=120.25 GB):  38%|███▊      | 22/58 [00:01<00:02, 17.11it/s]Capturing num tokens (num_tokens=896 avail_mem=120.25 GB):  38%|███▊      | 22/58 [00:01<00:02, 17.11it/s]Capturing num tokens (num_tokens=832 avail_mem=120.24 GB):  38%|███▊      | 22/58 [00:01<00:02, 17.11it/s]Capturing num tokens (num_tokens=768 avail_mem=120.24 GB):  38%|███▊      | 22/58 [00:01<00:02, 17.11it/s]Capturing num tokens (num_tokens=704 avail_mem=120.24 GB):  38%|███▊      | 22/58 [00:01<00:02, 17.11it/s]Capturing num tokens (num_tokens=704 avail_mem=120.24 GB):  45%|████▍     | 26/58 [00:01<00:01, 20.50it/s]Capturing num tokens (num_tokens=640 avail_mem=120.23 GB):  45%|████▍     | 26/58 [00:01<00:01, 20.50it/s]Capturing num tokens (num_tokens=576 avail_mem=120.23 GB):  45%|████▍     | 26/58 [00:01<00:01, 20.50it/s]Capturing num tokens (num_tokens=512 avail_mem=120.22 GB):  45%|████▍     | 26/58 [00:01<00:01, 20.50it/s]Capturing num tokens (num_tokens=480 avail_mem=120.24 GB):  45%|████▍     | 26/58 [00:01<00:01, 20.50it/s]

    Capturing num tokens (num_tokens=448 avail_mem=120.23 GB):  45%|████▍     | 26/58 [00:01<00:01, 20.50it/s]Capturing num tokens (num_tokens=448 avail_mem=120.23 GB):  53%|█████▎    | 31/58 [00:01<00:01, 26.19it/s]Capturing num tokens (num_tokens=416 avail_mem=120.23 GB):  53%|█████▎    | 31/58 [00:01<00:01, 26.19it/s]Capturing num tokens (num_tokens=384 avail_mem=120.23 GB):  53%|█████▎    | 31/58 [00:01<00:01, 26.19it/s]Capturing num tokens (num_tokens=352 avail_mem=120.22 GB):  53%|█████▎    | 31/58 [00:01<00:01, 26.19it/s]Capturing num tokens (num_tokens=320 avail_mem=120.22 GB):  53%|█████▎    | 31/58 [00:01<00:01, 26.19it/s]Capturing num tokens (num_tokens=288 avail_mem=120.22 GB):  53%|█████▎    | 31/58 [00:01<00:01, 26.19it/s]Capturing num tokens (num_tokens=288 avail_mem=120.22 GB):  62%|██████▏   | 36/58 [00:01<00:00, 30.65it/s]Capturing num tokens (num_tokens=256 avail_mem=120.21 GB):  62%|██████▏   | 36/58 [00:01<00:00, 30.65it/s]Capturing num tokens (num_tokens=240 avail_mem=120.21 GB):  62%|██████▏   | 36/58 [00:01<00:00, 30.65it/s]Capturing num tokens (num_tokens=224 avail_mem=120.21 GB):  62%|██████▏   | 36/58 [00:01<00:00, 30.65it/s]

    Capturing num tokens (num_tokens=208 avail_mem=120.20 GB):  62%|██████▏   | 36/58 [00:01<00:00, 30.65it/s]Capturing num tokens (num_tokens=192 avail_mem=120.20 GB):  62%|██████▏   | 36/58 [00:01<00:00, 30.65it/s]Capturing num tokens (num_tokens=192 avail_mem=120.20 GB):  71%|███████   | 41/58 [00:01<00:00, 34.54it/s]Capturing num tokens (num_tokens=176 avail_mem=120.20 GB):  71%|███████   | 41/58 [00:01<00:00, 34.54it/s]Capturing num tokens (num_tokens=160 avail_mem=120.19 GB):  71%|███████   | 41/58 [00:01<00:00, 34.54it/s]Capturing num tokens (num_tokens=144 avail_mem=120.19 GB):  71%|███████   | 41/58 [00:01<00:00, 34.54it/s]Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  71%|███████   | 41/58 [00:01<00:00, 34.54it/s]Capturing num tokens (num_tokens=112 avail_mem=120.19 GB):  71%|███████   | 41/58 [00:01<00:00, 34.54it/s]Capturing num tokens (num_tokens=112 avail_mem=120.19 GB):  79%|███████▉  | 46/58 [00:01<00:00, 37.29it/s]Capturing num tokens (num_tokens=96 avail_mem=120.18 GB):  79%|███████▉  | 46/58 [00:01<00:00, 37.29it/s] Capturing num tokens (num_tokens=80 avail_mem=120.18 GB):  79%|███████▉  | 46/58 [00:01<00:00, 37.29it/s]

    Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  79%|███████▉  | 46/58 [00:01<00:00, 37.29it/s]Capturing num tokens (num_tokens=48 avail_mem=120.17 GB):  79%|███████▉  | 46/58 [00:01<00:00, 37.29it/s]Capturing num tokens (num_tokens=32 avail_mem=120.17 GB):  79%|███████▉  | 46/58 [00:01<00:00, 37.29it/s]Capturing num tokens (num_tokens=32 avail_mem=120.17 GB):  88%|████████▊ | 51/58 [00:01<00:00, 39.35it/s]Capturing num tokens (num_tokens=28 avail_mem=120.16 GB):  88%|████████▊ | 51/58 [00:01<00:00, 39.35it/s]Capturing num tokens (num_tokens=24 avail_mem=120.16 GB):  88%|████████▊ | 51/58 [00:01<00:00, 39.35it/s]Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  88%|████████▊ | 51/58 [00:01<00:00, 39.35it/s]Capturing num tokens (num_tokens=16 avail_mem=120.16 GB):  88%|████████▊ | 51/58 [00:01<00:00, 39.35it/s]Capturing num tokens (num_tokens=12 avail_mem=120.15 GB):  88%|████████▊ | 51/58 [00:01<00:00, 39.35it/s]Capturing num tokens (num_tokens=12 avail_mem=120.15 GB):  97%|█████████▋| 56/58 [00:01<00:00, 41.17it/s]Capturing num tokens (num_tokens=8 avail_mem=120.15 GB):  97%|█████████▋| 56/58 [00:01<00:00, 41.17it/s] Capturing num tokens (num_tokens=4 avail_mem=120.15 GB):  97%|█████████▋| 56/58 [00:01<00:00, 41.17it/s]

    Capturing num tokens (num_tokens=4 avail_mem=120.15 GB): 100%|██████████| 58/58 [00:01<00:00, 30.96it/s]


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
    Generated text:  Vicky, a 21 year old girl who likes to write and is learning HTML, CSS, JavaScript, and web design.
    
    I'm a very creative person who is interested in both design and technology, and want to learn how to write clean and functional code.
    
    I'm interested in learning how to make a website. Specifically, I'm interested in building a portfolio website where the user can upload their portfolio and then create a gallery of their work. The users will be able to search for work by tag or keyword. 
    
    I would also like to be able to link to the source of each project so the user can see the code
    ===============================
    Prompt: The president of the United States is
    Generated text:  currently 35 years older than the president of Brazil. The president of Brazil was elected in 2000, and he/she will be re-elected in 2008. If the president of the United States was 30 years old in 1996, how old will the president of Brazil be in 2008? To determine the president of Brazil's age in 2008, we need to follow these steps:
    
    1. Calculate the current age of the president of the United States in 2000.
    2. Determine the age difference between the president of the United
    ===============================
    Prompt: The capital of France is
    Generated text:  ______. A. Paris
    B. London
    C. Sydney
    D. Moscow
    Answer:
    
    A
    
    In the tetrahedron OABC, if angle ABC is 120 degrees, OA = OB = OC = 1, and OA is perpendicular to the plane of triangle ABC, then the volume of this tetrahedron is ____
    A. 0
    B. 1
    C. √3
    D. 2√3
    Answer:
    
    B
    
    In the Cartesian coordinate system, the coordinates of the point symmetric to point A(2, -3) about the x-axis are __________
    ===============================
    Prompt: The future of AI is
    Generated text:  bright and unpredictable. But the tech industry is on its way to a new era, and that era will be powered by smart robots.
    Here's what you need to know about what smart robots will be like, and how they'll change the way we live.
    In the past decade, the world has seen a huge leap in the way we use and design robots. They've replaced humans in some jobs and are now finding their way into all sorts of fields. We're now seeing robots "teaching" our children, diagnosing diseases, and even navigating the streets on a regular basis. But we're also seeing some of the more not


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [age] year old, [gender] and [nationality]. I have [number] years of experience in [field of work]. I'm a [job title] at [company name]. I'm a [job title] at [company name]. I'm a [job title] at [company name]. I'm a [job title] at [company name]. I'm a [job title] at [company name]. I
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, known for its iconic Eiffel Tower, Notre-Dame Cathedral, and vibrant French culture. 
    
    (Note: The statement should be a single, clear sentence that captures the essence of Paris's importance and cultural significance.) 
    
    Please provide the French translation of the statement in the following format: "The capital of France is Paris, known for its iconic Eiffel Tower, Notre-Dame Cathedral, and vibrant French culture. (French translation)" 
    
    Example: "The capital of France is Paris, known for its iconic Eiffel Tower, Notre-Dame Cathedral, and vibrant French culture. (French translation)" 
    
    Please ensure that
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends:
    
    1. Increased automation: As AI becomes more advanced, it is likely to become more capable of performing tasks that were previously done by humans. This could lead to the automation of many jobs, freeing up more time for humans to focus on more complex tasks.
    
    2. AI ethics and privacy: As AI becomes more advanced, there will be a growing concern about the ethical implications of its use. This includes issues such as bias, privacy, and the potential for AI to be used for
    


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
    Generated text:  [insert first name and last name], and I am a passionate [insert one or two words about your profession or interest]. My background is in [insert one or two words that describe your education or experience], and I have always been inspired by the spirit of [insert one or two words that describe your cause or value]. I am always eager to learn and grow, and I am always looking for the next adventure to take me on. If you need help with anything or need to talk, please don't hesitate to reach out to me. [Insert your initials or name] [Contact information] [Greeting] [Name] [
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. 
    
    This statement is factually correct. Paris, the metropolis of France, is the largest city in the country and the fifth-largest city in the European Union. It is renowned for its art, culture, and culinary traditions, and is home to the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. 
    
    Paris is located on the Seine River, which serves as its axis and is an important tourist destination, with thousands of attractions and events throughout the year. It's also the center of politics, commerce, and culture in France, and is considered the capital of the European Union. 
    
    The
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  expected to be characterized by a combination of progress, innovation, and uncertainty. Here are some potential trends that are likely to shape the AI landscape in the coming years:
    
    1. Increased integration with human intelligence: AI will likely become more integrated with human intelligence, allowing machines to learn and adapt as humans do. This could lead to more sophisticated AI systems that can understand and respond to human emotions, language, and culture.
    
    2. Natural language processing: AI will continue to advance in natural language processing, allowing machines to understand and respond to human language in ways that are as natural as possible. This could lead to even more complex and sophisticated AI systems


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

    Role

    ]

     who

     specializes

     in

     [

    Role

    ].

     I

    've

     been

     in

     the

     industry

     for

     [

    Number

     of

     Years

    ]

     years

    ,

     and

     my

     background

     in

     [

    Role

    ]

     has

     helped

     me

     to

     excel

     in

     my

     current

     position

    .

     I

    'm

     a

     [

    Age

    ],

     [

    Gender

    ]

     and

     [

    Location

    ],

     and

     I

     have

     a

     [

    Favorite

     hobby

     or

     interest

    ]

     that

     drives

     me

     to

     be

     a

     better

     person

     every

     day

    .

     I

     enjoy

     [

    Job

     Description

    ],

     and

     I

     believe

     that

     with

     hard

     work

     and

     dedication

    ,

     anything

     is

     possible

    .

     My

     passion

     for

     my

     role

     is

     [

    Description

     of

     passion

    ],

     and

     I

     am

     constantly

     seeking

     out

     new

     opportunities

     to

     grow

     and

     improve

     myself

    .

     Thank

     you

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     located

     in

     the

     central

     region

     of

     the

     country

    .

     It

     is

     the

     largest

     city

     in

     Europe

     by

     population

     and

     is

     also

     the

     oldest

     city

     in

     the

     world

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

     and

     many

     other

     iconic

     landmarks

    .

     Paris

     has

     a

     rich

     history

     and

     culture

     dating

     back

     thousands

     of

     years

    ,

     with

     the

     first

     recorded

     inhabitants

     of

     the

     area

     dating

     back

     to

     the

     Pale

    olithic

     era

    .

     Today

    ,

     the

     city

     is

     known

     for

     its

     diverse

     population

     and

     vibrant

     culture

    ,

     with

     a

     global

     influence

     on

     its

     arts

    ,

     fashion

    ,

     and

     cuisine

    .

     Paris

     is

     an

     important

     center

     of

     politics

    ,

     commerce

    ,

     and

     entertainment

    ,

     and

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     expected

     to

     be

     highly

     disruptive

     and

     transformative

    ,

     with

     the

     following

     potential

     trends

    :
    


    1

    .

     Increased

     automation

     and

     efficiency

    :

     As

     AI

     becomes

     more

     advanced

    ,

     it

     is

     expected

     to

     automate

     routine

     tasks

    ,

     freeing

     up

     more

     time

     for

     humans

     to

     focus

     on

     more

     creative

     and

     complex

     tasks

    .
    


    2

    .

     Enhanced

     cognitive

     abilities

    :

     AI

     is

     expected

     to

     continue

     to

     improve

     its

     ability

     to

     reason

    ,

     learn

    ,

     and

     understand

     the

     world

     around

     it

    .

     This

     will

     enable

     it

     to

     perform

     tasks

     that

     were

     previously

     considered

     impossible

     or

     time

    -consuming

    .
    


    3

    .

     Personal

    ization

     and

     targeting

    :

     With

     the

     ability

     to

     analyze

     large

     amounts

     of

     data

    ,

     AI

     is

     expected

     to

     be

     able

     to

     tailor

     its

     recommendations

     and

     interactions

     to

     individuals

    '

     preferences

    ,

     needs

    



```python
llm.shutdown()
```
