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


    2026-03-30 23:32:00.773 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 23:32:00] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 23:32:00.773 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 23:32:00] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 23:32:00.773 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 23:32:00] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 23:32:00.773 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 23:32:00] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 23:32:00.773 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 23:32:00] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.39it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.37it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:32,  2.68s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:32,  2.68s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:32,  2.68s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:32,  2.68s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:32,  2.68s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:22,  2.36it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:22,  2.36it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:22,  2.36it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:22,  2.36it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:22,  2.36it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:22,  2.36it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:22,  2.36it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:03, 12.54it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:03, 12.54it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:03, 12.54it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:03, 12.54it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:03, 12.54it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:03, 12.54it/s]

    Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:03, 12.54it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:03, 12.54it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 18.55it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 18.55it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 18.55it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 18.55it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 18.55it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 18.55it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:03<00:01, 18.55it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:03<00:01, 18.55it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:00, 25.13it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:00, 25.13it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:00, 25.13it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:00, 25.13it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:00, 25.13it/s]

    Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:00, 25.13it/s]Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:03<00:00, 25.13it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 30.12it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 30.12it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 30.12it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 30.12it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 30.12it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 30.12it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 30.12it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 34.98it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 34.98it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 34.98it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 34.98it/s]

    Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 34.98it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 34.98it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 34.98it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 39.00it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 39.00it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 39.00it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 39.00it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 39.00it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 39.00it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 39.00it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 39.00it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.89it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=121.74 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=121.71 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=121.71 GB):   3%|▎         | 2/58 [00:00<00:03, 18.65it/s]Capturing num tokens (num_tokens=7168 avail_mem=121.70 GB):   3%|▎         | 2/58 [00:00<00:03, 18.65it/s]Capturing num tokens (num_tokens=6656 avail_mem=121.43 GB):   3%|▎         | 2/58 [00:00<00:03, 18.65it/s]Capturing num tokens (num_tokens=6144 avail_mem=121.41 GB):   3%|▎         | 2/58 [00:00<00:03, 18.65it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=121.41 GB):   9%|▊         | 5/58 [00:00<00:02, 21.13it/s]Capturing num tokens (num_tokens=5632 avail_mem=120.72 GB):   9%|▊         | 5/58 [00:00<00:02, 21.13it/s]Capturing num tokens (num_tokens=5120 avail_mem=120.72 GB):   9%|▊         | 5/58 [00:00<00:02, 21.13it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.72 GB):   9%|▊         | 5/58 [00:00<00:02, 21.13it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.72 GB):   9%|▊         | 5/58 [00:00<00:02, 21.13it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.72 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.16it/s]Capturing num tokens (num_tokens=3840 avail_mem=120.72 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.16it/s]Capturing num tokens (num_tokens=3584 avail_mem=120.71 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.16it/s]Capturing num tokens (num_tokens=3328 avail_mem=120.71 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.16it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=120.70 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.16it/s]Capturing num tokens (num_tokens=3072 avail_mem=120.70 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.39it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.70 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.39it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.70 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.39it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.70 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.39it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.69 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.39it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.69 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.39it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.69 GB):  31%|███       | 18/58 [00:00<00:01, 34.75it/s]Capturing num tokens (num_tokens=1536 avail_mem=120.69 GB):  31%|███       | 18/58 [00:00<00:01, 34.75it/s]Capturing num tokens (num_tokens=1280 avail_mem=120.68 GB):  31%|███       | 18/58 [00:00<00:01, 34.75it/s]Capturing num tokens (num_tokens=1024 avail_mem=120.66 GB):  31%|███       | 18/58 [00:00<00:01, 34.75it/s]

    Capturing num tokens (num_tokens=960 avail_mem=120.68 GB):  31%|███       | 18/58 [00:00<00:01, 34.75it/s] Capturing num tokens (num_tokens=896 avail_mem=120.67 GB):  31%|███       | 18/58 [00:00<00:01, 34.75it/s]Capturing num tokens (num_tokens=896 avail_mem=120.67 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.99it/s]Capturing num tokens (num_tokens=832 avail_mem=120.67 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.99it/s]Capturing num tokens (num_tokens=768 avail_mem=120.34 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.99it/s]Capturing num tokens (num_tokens=704 avail_mem=120.24 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.99it/s]Capturing num tokens (num_tokens=640 avail_mem=120.23 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.99it/s]Capturing num tokens (num_tokens=576 avail_mem=120.23 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.99it/s]Capturing num tokens (num_tokens=576 avail_mem=120.23 GB):  48%|████▊     | 28/58 [00:00<00:00, 39.90it/s]Capturing num tokens (num_tokens=512 avail_mem=120.22 GB):  48%|████▊     | 28/58 [00:00<00:00, 39.90it/s]Capturing num tokens (num_tokens=480 avail_mem=120.24 GB):  48%|████▊     | 28/58 [00:00<00:00, 39.90it/s]

    Capturing num tokens (num_tokens=448 avail_mem=120.24 GB):  48%|████▊     | 28/58 [00:00<00:00, 39.90it/s]Capturing num tokens (num_tokens=416 avail_mem=120.23 GB):  48%|████▊     | 28/58 [00:00<00:00, 39.90it/s]Capturing num tokens (num_tokens=384 avail_mem=120.23 GB):  48%|████▊     | 28/58 [00:00<00:00, 39.90it/s]Capturing num tokens (num_tokens=384 avail_mem=120.23 GB):  57%|█████▋    | 33/58 [00:00<00:00, 41.13it/s]Capturing num tokens (num_tokens=352 avail_mem=120.23 GB):  57%|█████▋    | 33/58 [00:00<00:00, 41.13it/s]Capturing num tokens (num_tokens=320 avail_mem=120.22 GB):  57%|█████▋    | 33/58 [00:00<00:00, 41.13it/s]Capturing num tokens (num_tokens=288 avail_mem=120.22 GB):  57%|█████▋    | 33/58 [00:00<00:00, 41.13it/s]Capturing num tokens (num_tokens=256 avail_mem=120.21 GB):  57%|█████▋    | 33/58 [00:00<00:00, 41.13it/s]Capturing num tokens (num_tokens=240 avail_mem=120.21 GB):  57%|█████▋    | 33/58 [00:01<00:00, 41.13it/s]Capturing num tokens (num_tokens=240 avail_mem=120.21 GB):  66%|██████▌   | 38/58 [00:01<00:00, 41.82it/s]Capturing num tokens (num_tokens=224 avail_mem=120.21 GB):  66%|██████▌   | 38/58 [00:01<00:00, 41.82it/s]

    Capturing num tokens (num_tokens=208 avail_mem=120.20 GB):  66%|██████▌   | 38/58 [00:01<00:00, 41.82it/s]Capturing num tokens (num_tokens=192 avail_mem=120.20 GB):  66%|██████▌   | 38/58 [00:01<00:00, 41.82it/s]Capturing num tokens (num_tokens=176 avail_mem=120.20 GB):  66%|██████▌   | 38/58 [00:01<00:00, 41.82it/s]Capturing num tokens (num_tokens=160 avail_mem=120.19 GB):  66%|██████▌   | 38/58 [00:01<00:00, 41.82it/s]Capturing num tokens (num_tokens=160 avail_mem=120.19 GB):  74%|███████▍  | 43/58 [00:01<00:00, 42.84it/s]Capturing num tokens (num_tokens=144 avail_mem=120.19 GB):  74%|███████▍  | 43/58 [00:01<00:00, 42.84it/s]Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  74%|███████▍  | 43/58 [00:01<00:00, 42.84it/s]Capturing num tokens (num_tokens=112 avail_mem=120.19 GB):  74%|███████▍  | 43/58 [00:01<00:00, 42.84it/s]Capturing num tokens (num_tokens=96 avail_mem=120.18 GB):  74%|███████▍  | 43/58 [00:01<00:00, 42.84it/s] Capturing num tokens (num_tokens=80 avail_mem=120.18 GB):  74%|███████▍  | 43/58 [00:01<00:00, 42.84it/s]Capturing num tokens (num_tokens=80 avail_mem=120.18 GB):  83%|████████▎ | 48/58 [00:01<00:00, 43.31it/s]Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  83%|████████▎ | 48/58 [00:01<00:00, 43.31it/s]

    Capturing num tokens (num_tokens=48 avail_mem=120.17 GB):  83%|████████▎ | 48/58 [00:01<00:00, 43.31it/s]Capturing num tokens (num_tokens=32 avail_mem=120.17 GB):  83%|████████▎ | 48/58 [00:01<00:00, 43.31it/s]Capturing num tokens (num_tokens=28 avail_mem=120.16 GB):  83%|████████▎ | 48/58 [00:01<00:00, 43.31it/s]Capturing num tokens (num_tokens=24 avail_mem=120.16 GB):  83%|████████▎ | 48/58 [00:01<00:00, 43.31it/s]Capturing num tokens (num_tokens=24 avail_mem=120.16 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.81it/s]Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.81it/s]Capturing num tokens (num_tokens=16 avail_mem=120.16 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.81it/s]Capturing num tokens (num_tokens=12 avail_mem=120.15 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.81it/s]Capturing num tokens (num_tokens=8 avail_mem=120.15 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.81it/s] Capturing num tokens (num_tokens=4 avail_mem=120.15 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.81it/s]Capturing num tokens (num_tokens=4 avail_mem=120.15 GB): 100%|██████████| 58/58 [00:01<00:00, 44.55it/s]Capturing num tokens (num_tokens=4 avail_mem=120.15 GB): 100%|██████████| 58/58 [00:01<00:00, 39.01it/s]


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
    Generated text:  Amey. I'm in Grade 12 and I used to like to play tennis. I enjoyed playing it, and I thought that it was a sport that was challenging and could improve my skills. Therefore, I decided to start practicing my tennis skills. However, I also made a decision to pay attention to my physical health. Since I was in Grade 12, I wanted to increase my physical fitness. I decided to take up running because I thought that it would be a good way to help me improve my running speed. I also wanted to exercise regularly and lose some weight. 
    
    As a result, I decided to practice
    ===============================
    Prompt: The president of the United States is
    Generated text:  a man. If he were to meet with the United States embassy in London, what is the type of speech he would likely deliver?
    
    Select from the following.
     A). formal
     B). informal
     C). political
     D). abstract
    
    C). political
    
    The president of the United States would likely deliver a political speech to the United States embassy in London. Such a speech would typically cover topics related to foreign policy, global issues, and the country's role in international affairs. The president would likely make statements, outline their agenda, and discuss how they plan to contribute to the global community. This type of speech is commonly used to inform
    ===============================
    Prompt: The capital of France is
    Generated text:  _______. ____
    A. Paris
    B. Vienna
    C. Rome
    D. Berlin
    Answer:
    
    A
    
    Which of the following statements about the three northeastern provinces of China is true?
    A. The ice-free season is the longest.
    B. The distance between each province is the shortest.
    C. The climate is extremely cold throughout the year.
    D. The main crop is tobacco.
    Answer:
    
    B
    
    The reason why Russia is rich in oil and natural gas resources is due to its ____. 
    A. large area
    B. large population
    C. abundant rivers and lakes
    D. abundant mineral deposits
    Answer:
    
    D
    ===============================
    Prompt: The future of AI is
    Generated text:  coming fast, and some of the ways it’s coming is quite unexpected. In a recent presentation at the AI Week event in Chicago, we spoke to Mark Miller about the future of AI and the steps it’s taking to transform the world.
    In the video, he talked about a variety of exciting projects and innovations that are being made with the aim of making the future of AI better. At the core, he explained that we’re moving towards a more human-centric AI that will help people interact with the world in more intuitive, natural ways.
    He also pointed out that we’re seeing advancements in the AI that are making it more efficient and accurate


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [age] year old, [gender] and [occupation]. I have a [job title] at [company name] and I enjoy [job title] work. I'm always looking for new challenges and opportunities to grow and learn. What's your favorite hobby or activity? I love [hobby or activity], and I find it very rewarding to be able to help others and make a difference in the world. What's your favorite book
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also a major cultural and economic center, hosting numerous museums, theaters, and other attractions. Paris is a popular tourist destination and a major hub for international business and diplomacy. The city is known for its rich history, including the influence of the French Revolution and the influence of the French language. Paris is also home to many famous French artists, writers, and musicians. The city is a major center for the arts and culture, with many museums, theaters, and other cultural institutions. It is also a
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction. Here are some of the most likely trends:
    
    1. Increased focus on ethical AI: As more people become aware of the potential risks of AI, there is a growing emphasis on developing AI that is designed to be ethical and responsible. This could mean developing AI that is designed to minimize harm to individuals, protect privacy, and promote fairness and equality.
    
    2. Integration of AI with other technologies: AI is likely to become more integrated with other technologies, such as the Internet of Things (IoT), blockchain, and quantum computing. This integration could lead
    


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
    Generated text:  __________ and I'm a/an ___________. I'm an __________. I'm __________. I'm __________. I'm __________. I'm __________. I'm __________. I'm __________. I'm __________. I'm __________. I'm __________. I'm __________. I'm __________. I'm __________. I'm __________. I'm __________. I'm __________. I'm __________. I'm __________. I'm __________. I'm __________. I'm __________. I'm __________. I'm __________. I'm
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. It is the largest city in France by population and is the second-largest city by area. The city has a rich history, including the ancient Roman city of Paris and the medieval cities of Notre-Dame and Notre Dame de Paris. It is known for its iconic landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. Paris is also renowned for its food, culture, and fashion, making it a popular tourist destination. Its nickname "The City of Light" refers to its vibrant and colorful atmosphere. The city is home to many world-renowned museums, theaters, and restaurants, making it a
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  expected to be shaped by several trends and developments, including:
    
    1. Increasing integration of AI into everyday life: As AI technologies continue to advance, we can expect to see more integration into our daily lives, such as in smart home devices, self-driving cars, and more.
    
    2. AI becoming more ethical and responsible: As AI becomes more integrated into our lives, we can expect to see a rise in ethical concerns and regulations surrounding AI development and use.
    
    3. AI becoming more ubiquitous: As more and more devices and systems become connected to the internet, we can expect to see a rise in AI-based applications and services that we use every


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

     am

     a

    /an

     [

    occupation

    ].

     I

     have

     [

    number

    ]

     years

     of

     experience

     in

     the

     [

    occupation

    ]

     industry

    ,

     and

     I

     have

     always

     been

     passionate

     about

     [

    occupation

    ]

     and

     have

     been

     a

     long

    -time

     supporter

     of

     [

    organization

    ]

     initiatives

    .

     I

     am

     a

     real

    ist

     and

     detail

    -oriented

    ,

     and

     I

     am

     constantly

     striving

     to

     improve

     myself

     and

     reach

     new

     heights

    .

     I

     value

     the

     importance

     of

     [

    occupation

    ]

     and

     I

     am

     dedicated

     to

     helping

     others

     succeed

     in

     this

     field

    .

     I

     am

     here

     to

     help

     you

     succeed

     in

     your

     career

     and

     provide

     you

     with

     the

     tools

     and

     resources

     you

     need

     to

     achieve

     your

     goals

    .

     So

    ,

     if

     you

    're

     looking

     to

     achieve

     your

     career

     goals

     in

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     also

     known

     as

     “

    La

     Grande

    -B

    ret

    agne

    .”


    Paris

     is

     the

     capital

     city

     of

     France

    ,

     located

     on

     the

     River

     Se

    ine

    ,

     and

     is

     one

     of

     the

     largest

     cities

     in

     the

     world

    .

     It

     is

     home

     to

     many

     of

     the

     country

    's

     most

     important

     cultural

     and

     historical

     landmarks

    ,

     including

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

     the

     E

    iff

    el

     Tower

    .

     The

     city

     is

     also

     known

     for

     its

     rich

     culinary

     traditions

    ,

     including

     regional

     cuisine

     and

     haute

     cuisine

    .

     Paris

     has

     a

     vibrant

     and

     diverse

     cultural

     scene

    ,

     with

     many

     renowned

     artists

    ,

     musicians

    ,

     and

     writers

     living

     and

     working

     in

     the

     city

    .

     The

     city

     is

     a

     major

     hub

     for

     international

     trade

     and

     commerce

    ,

     and

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     very

     promising

    ,

     and

     it

     is

     expected

     to

     continue

     to

     evolve

     and

     advance

     in

     the

     following

     trends

    :
    


    1

    .

     Increased

     autonomy

     and

     decision

    -making

    :

     As

     AI

     continues

     to

     become

     more

     advanced

    ,

     we

     can

     expect

     to

     see

     a

     greater

     emphasis

     on

     enabling

     machines

     to

     make

     decisions

     independently

    ,

     rather

     than

     just

     following

     pre

    -program

    med

     instructions

    .

     This

     could

     lead

     to

     more

     autonomous

     vehicles

    ,

     self

    -driving

     cars

    ,

     and

     other

     automated

     systems

    .
    


    2

    .

     Enhanced

     human

    -com

    puter

     interaction

    :

     As

     AI

     becomes

     more

     capable

    ,

     it

     is

     also

     becoming

     possible

     to

     create

     interfaces

     that

     feel

     more

     natural

     and

     intuitive

    ,

     with

     the

     human

     operator

     being

     able

     to

     guide

     the

     machine

     in

     a

     more

     human

    -like

     way

    .
    


    3

    .

     Increased

     use

     of

     AI

    



```python
llm.shutdown()
```
