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


    2026-03-31 18:06:11.689 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 18:06:11] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 18:06:11.689 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 18:06:11] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 18:06:11.689 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 18:06:11] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 18:06:11.689 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 18:06:11] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 18:06:11.689 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 18:06:11] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.22it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.20it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:16,  2.39s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:16,  2.39s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:16,  2.39s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:16,  2.39s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:16,  2.39s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:20,  2.62it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:20,  2.62it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:20,  2.62it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:20,  2.62it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:20,  2.62it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:20,  2.62it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:20,  2.62it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:06,  6.84it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:06,  6.84it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:06,  6.84it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:06,  6.84it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:06,  6.84it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:06,  6.84it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:02<00:06,  6.84it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:02<00:06,  6.84it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:02<00:06,  6.84it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 13.69it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 13.69it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 13.69it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 13.69it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 13.69it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 13.69it/s]

    Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 13.69it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:02<00:02, 13.69it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:02<00:01, 20.07it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:02<00:01, 20.07it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:02<00:01, 20.07it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:02<00:01, 20.07it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:02<00:01, 20.07it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:02<00:01, 20.07it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:02<00:01, 20.07it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:02<00:01, 25.78it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:02<00:01, 25.78it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:02<00:01, 25.78it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:02<00:01, 25.78it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:02<00:01, 25.78it/s]

    Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 25.78it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 25.78it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 30.92it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 30.92it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 30.92it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 30.92it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 30.92it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 30.92it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 30.92it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 36.35it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 36.35it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 36.35it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 36.35it/s] 

    Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 36.35it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 36.35it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 36.35it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 39.44it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 39.44it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 39.44it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 39.44it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 39.44it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 39.44it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 39.44it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 39.44it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 39.44it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.20it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=137.42 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=137.39 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=137.39 GB):   3%|▎         | 2/58 [00:00<00:03, 18.66it/s]Capturing num tokens (num_tokens=7168 avail_mem=137.38 GB):   3%|▎         | 2/58 [00:00<00:03, 18.66it/s]Capturing num tokens (num_tokens=6656 avail_mem=137.38 GB):   3%|▎         | 2/58 [00:00<00:03, 18.66it/s]Capturing num tokens (num_tokens=6144 avail_mem=137.38 GB):   3%|▎         | 2/58 [00:00<00:03, 18.66it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=137.38 GB):   9%|▊         | 5/58 [00:00<00:02, 21.82it/s]Capturing num tokens (num_tokens=5632 avail_mem=137.38 GB):   9%|▊         | 5/58 [00:00<00:02, 21.82it/s]Capturing num tokens (num_tokens=5120 avail_mem=137.38 GB):   9%|▊         | 5/58 [00:00<00:02, 21.82it/s]Capturing num tokens (num_tokens=4608 avail_mem=137.38 GB):   9%|▊         | 5/58 [00:00<00:02, 21.82it/s]Capturing num tokens (num_tokens=4096 avail_mem=137.38 GB):   9%|▊         | 5/58 [00:00<00:02, 21.82it/s]Capturing num tokens (num_tokens=4096 avail_mem=137.38 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.39it/s]Capturing num tokens (num_tokens=3840 avail_mem=137.37 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.39it/s]Capturing num tokens (num_tokens=3584 avail_mem=137.37 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.39it/s]Capturing num tokens (num_tokens=3328 avail_mem=137.37 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.39it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=137.36 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.39it/s]Capturing num tokens (num_tokens=3072 avail_mem=137.36 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.37it/s]Capturing num tokens (num_tokens=2816 avail_mem=137.36 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.37it/s]Capturing num tokens (num_tokens=2560 avail_mem=137.36 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.37it/s]Capturing num tokens (num_tokens=2304 avail_mem=137.35 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.37it/s]Capturing num tokens (num_tokens=2048 avail_mem=137.35 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.37it/s]Capturing num tokens (num_tokens=1792 avail_mem=137.35 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.37it/s]Capturing num tokens (num_tokens=1792 avail_mem=137.35 GB):  31%|███       | 18/58 [00:00<00:01, 34.93it/s]Capturing num tokens (num_tokens=1536 avail_mem=137.34 GB):  31%|███       | 18/58 [00:00<00:01, 34.93it/s]Capturing num tokens (num_tokens=1280 avail_mem=137.34 GB):  31%|███       | 18/58 [00:00<00:01, 34.93it/s]Capturing num tokens (num_tokens=1024 avail_mem=137.32 GB):  31%|███       | 18/58 [00:00<00:01, 34.93it/s]

    Capturing num tokens (num_tokens=960 avail_mem=137.33 GB):  31%|███       | 18/58 [00:00<00:01, 34.93it/s] Capturing num tokens (num_tokens=896 avail_mem=137.33 GB):  31%|███       | 18/58 [00:00<00:01, 34.93it/s]Capturing num tokens (num_tokens=896 avail_mem=137.33 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.80it/s]Capturing num tokens (num_tokens=832 avail_mem=137.33 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.80it/s]Capturing num tokens (num_tokens=768 avail_mem=137.32 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.80it/s]Capturing num tokens (num_tokens=704 avail_mem=137.32 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.80it/s]Capturing num tokens (num_tokens=640 avail_mem=137.32 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.80it/s]Capturing num tokens (num_tokens=640 avail_mem=137.32 GB):  47%|████▋     | 27/58 [00:00<00:00, 36.71it/s]Capturing num tokens (num_tokens=576 avail_mem=137.32 GB):  47%|████▋     | 27/58 [00:00<00:00, 36.71it/s]Capturing num tokens (num_tokens=512 avail_mem=137.30 GB):  47%|████▋     | 27/58 [00:00<00:00, 36.71it/s]

    Capturing num tokens (num_tokens=480 avail_mem=137.32 GB):  47%|████▋     | 27/58 [00:00<00:00, 36.71it/s]Capturing num tokens (num_tokens=448 avail_mem=137.32 GB):  47%|████▋     | 27/58 [00:00<00:00, 36.71it/s]Capturing num tokens (num_tokens=448 avail_mem=137.32 GB):  53%|█████▎    | 31/58 [00:00<00:00, 35.35it/s]Capturing num tokens (num_tokens=416 avail_mem=137.32 GB):  53%|█████▎    | 31/58 [00:00<00:00, 35.35it/s]

    Capturing num tokens (num_tokens=384 avail_mem=137.30 GB):  53%|█████▎    | 31/58 [00:01<00:00, 35.35it/s]Capturing num tokens (num_tokens=352 avail_mem=137.29 GB):  53%|█████▎    | 31/58 [00:01<00:00, 35.35it/s]Capturing num tokens (num_tokens=320 avail_mem=137.29 GB):  53%|█████▎    | 31/58 [00:01<00:00, 35.35it/s]Capturing num tokens (num_tokens=320 avail_mem=137.29 GB):  60%|██████    | 35/58 [00:01<00:00, 23.58it/s]Capturing num tokens (num_tokens=288 avail_mem=137.28 GB):  60%|██████    | 35/58 [00:01<00:00, 23.58it/s]Capturing num tokens (num_tokens=256 avail_mem=137.28 GB):  60%|██████    | 35/58 [00:01<00:00, 23.58it/s]Capturing num tokens (num_tokens=240 avail_mem=137.26 GB):  60%|██████    | 35/58 [00:01<00:00, 23.58it/s]

    Capturing num tokens (num_tokens=224 avail_mem=136.78 GB):  60%|██████    | 35/58 [00:01<00:00, 23.58it/s]Capturing num tokens (num_tokens=224 avail_mem=136.78 GB):  67%|██████▋   | 39/58 [00:01<00:00, 26.48it/s]Capturing num tokens (num_tokens=208 avail_mem=136.78 GB):  67%|██████▋   | 39/58 [00:01<00:00, 26.48it/s]Capturing num tokens (num_tokens=192 avail_mem=136.63 GB):  67%|██████▋   | 39/58 [00:01<00:00, 26.48it/s]Capturing num tokens (num_tokens=176 avail_mem=136.61 GB):  67%|██████▋   | 39/58 [00:01<00:00, 26.48it/s]Capturing num tokens (num_tokens=160 avail_mem=136.61 GB):  67%|██████▋   | 39/58 [00:01<00:00, 26.48it/s]Capturing num tokens (num_tokens=144 avail_mem=136.61 GB):  67%|██████▋   | 39/58 [00:01<00:00, 26.48it/s]Capturing num tokens (num_tokens=144 avail_mem=136.61 GB):  76%|███████▌  | 44/58 [00:01<00:00, 30.87it/s]Capturing num tokens (num_tokens=128 avail_mem=136.60 GB):  76%|███████▌  | 44/58 [00:01<00:00, 30.87it/s]Capturing num tokens (num_tokens=112 avail_mem=136.60 GB):  76%|███████▌  | 44/58 [00:01<00:00, 30.87it/s]Capturing num tokens (num_tokens=96 avail_mem=136.60 GB):  76%|███████▌  | 44/58 [00:01<00:00, 30.87it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=136.59 GB):  76%|███████▌  | 44/58 [00:01<00:00, 30.87it/s]Capturing num tokens (num_tokens=64 avail_mem=136.59 GB):  76%|███████▌  | 44/58 [00:01<00:00, 30.87it/s]Capturing num tokens (num_tokens=64 avail_mem=136.59 GB):  84%|████████▍ | 49/58 [00:01<00:00, 34.16it/s]Capturing num tokens (num_tokens=48 avail_mem=136.59 GB):  84%|████████▍ | 49/58 [00:01<00:00, 34.16it/s]Capturing num tokens (num_tokens=32 avail_mem=136.58 GB):  84%|████████▍ | 49/58 [00:01<00:00, 34.16it/s]Capturing num tokens (num_tokens=28 avail_mem=136.58 GB):  84%|████████▍ | 49/58 [00:01<00:00, 34.16it/s]Capturing num tokens (num_tokens=24 avail_mem=136.58 GB):  84%|████████▍ | 49/58 [00:01<00:00, 34.16it/s]Capturing num tokens (num_tokens=20 avail_mem=136.57 GB):  84%|████████▍ | 49/58 [00:01<00:00, 34.16it/s]Capturing num tokens (num_tokens=20 avail_mem=136.57 GB):  93%|█████████▎| 54/58 [00:01<00:00, 36.87it/s]Capturing num tokens (num_tokens=16 avail_mem=136.57 GB):  93%|█████████▎| 54/58 [00:01<00:00, 36.87it/s]Capturing num tokens (num_tokens=12 avail_mem=136.57 GB):  93%|█████████▎| 54/58 [00:01<00:00, 36.87it/s]

    Capturing num tokens (num_tokens=8 avail_mem=136.56 GB):  93%|█████████▎| 54/58 [00:01<00:00, 36.87it/s] Capturing num tokens (num_tokens=4 avail_mem=136.56 GB):  93%|█████████▎| 54/58 [00:01<00:00, 36.87it/s]Capturing num tokens (num_tokens=4 avail_mem=136.56 GB): 100%|██████████| 58/58 [00:01<00:00, 32.68it/s]


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
    Generated text:  Jamie and I'm a busy teen age boy who has been studying music since I was 9 years old. I'm a huge fan of classical music and I'm trying to get my own band together. I've been working on my own music for the past year and I'm very excited to get started. I have some music-related questions that I'd like to ask you. Here's what I'm asking:
    
      1. What is the difference between stringed and wind instruments? Can you explain the difference between a guitar and a banjo?
      2. Is there a difference between keyboards and instruments that are not keyboard instruments
    ===============================
    Prompt: The president of the United States is
    Generated text:  holding a press conference. In the middle of the conference, there is a photo opportunity where reporters can take photos. On this occasion, reporters are to be assigned to two cameras, one in the front row and one in the back row. How many different ways can the reporters be assigned to the two cameras? To determine the number of different ways to assign reporters to two cameras (one in the front row and one in the back row), we can follow these steps:
    
    1. **Identify the choices for the front row:**
       There are 2 choices for the reporter in the front row (either the first or the second reporter
    ===============================
    Prompt: The capital of France is
    Generated text: :
    A. Paris
    B. Rome
    C. Vienna
    D. London
    A. Paris
    
    Paris is the capital city of France, while the other options listed are cities in other countries or countries in Europe. London, on the other hand, is the capital city of the United Kingdom, and Rome is the capital city of Italy. Vienna, on the other hand, is the capital city of Austria. 
    
    Therefore, the correct answer is A. Paris. 
    
    Note: London is the capital city of the United Kingdom and Rome is the capital city of Italy. Vienna is the capital city of Austria. However, since the question
    ===============================
    Prompt: The future of AI is
    Generated text:  not yet here, but it is here already. The technology is developing rapidly, and we are living in an era of continuous innovation. This is the time for us to use AI to help the world. However, it is important to note that the development of AI is not perfect, and there are still many challenges that need to be addressed.
    
    The first challenge is the ethical considerations of AI. The use of AI raises many ethical questions, such as privacy, bias, and safety. The development of AI should be based on clear ethical guidelines that take into account the well-being of individuals and society as a whole. These guidelines should be developed


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [insert a brief description of your job or profession]. I enjoy [insert a brief description of your hobbies or interests]. I'm always looking for new challenges and opportunities to grow and learn. What's your favorite hobby or activity? I love [insert a hobby or activity you enjoy]. I'm always looking for new experiences and opportunities to expand my knowledge and skills. What's your favorite book or movie? I love [insert a favorite book or
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French Academy of Sciences, and the French National Library. Paris is a bustling metropolis with a rich cultural heritage and is a major tourist destination. It is also home to the French Riviera, a popular tourist destination for its beaches and luxury resorts. The city is known for its cuisine, including its famous croissants and its traditional French dishes. Paris is a city of contrasts, with its modern architecture and historical landmarks blending seamlessly. It is a
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction. Here are some of the most likely trends:
    
    1. Increased integration with other technologies: AI is already being integrated with other technologies such as machine learning, natural language processing, and computer vision. It is expected that this integration will continue to grow in the future, with more and more applications being developed that leverage the power of AI.
    
    2. Advancements in hardware: As the cost of computing power continues to decrease, there is a growing expectation that AI will become more efficient and powerful. This will likely lead to the development of even more powerful hardware
    


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
    Generated text:  [name]. I'm a [career or profession] who has a [specific skill or trait] that allows me to excel in this field. I enjoy [insert a hobby, interest, or passion that fits your character]. I'm constantly looking for new opportunities to grow and learn, and I'm always open to new experiences and challenges. Thank you for taking the time to learn more about me! [Name] [Career or Profession] [Name] [Name] Hi, I'm [name] and I'm a [career or profession] who has a [specific skill or trait] that allows me to excel in this field.
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. Its official name is the City of Paris, and it is located on the northern shore of the Seine river, on the left bank of the river, in the region of the Hauts-de-France département. It is the third-largest city in France and the most populous city in the country. As of 2018, the population was estimated at about 2.8 million. Paris is known for its iconic architecture, rich cultural heritage, and annual celebrations such as the Eiffel Tower and World War II Memorial. 
    
    The city was founded by the Romans in the 3rd century BC as the capital
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  exciting and varied, with many possibilities emerging. Here are some potential trends that might shape the AI landscape in the next decade:
    
    1. Increased Integration with Other Technologies: As AI becomes more integrated with other technologies, we can expect to see more complex, interconnected systems. This could lead to new opportunities for AI-powered innovations, such as new ways to optimize supply chains, improve healthcare, and enhance education.
    
    2. Development of More Diverse AI Ethical Considerations: As AI becomes more prevalent, we will need to develop new ethical frameworks for its development and deployment. This could involve developing guidelines for the responsible development and deployment of AI, as


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

    Your

     Name

    ]

     and

     I

    'm

     a

     [

    insert

     your

     job

     or

     profession

    ]

    !

     I

    'm

     currently

     [

    insert

     how

     long

     I

    've

     been

     working

     here

    ].

     I

    'm

     always

     ready

     to

     learn

     and

     grow

    ,

     and

     I

    'm

     always

     looking

     for

     new

     opportunities

     to

     grow

     my

     skills

     and

     experience

    .

     How

     can

     I

     assist

     you

     today

    ?

     Let

    's

     set

     a

     time

     to

     talk

     about

     what

     you

    're

     looking

     for

     or

     what

     you

     need

    !

     [

    Your

     Name

    ]

     [

    insert

     your

     contact

     information

    ]

     ||

    |

    EMAIL

    _ADDRESS

    ||

    |

     [

    insert

     your

     phone

     number

    ]

     ||

    |

    EMAIL

    _ADDRESS

    ||

    |

     [

    insert

     your

     address

    ]

     ||

    |

    PHONE

    _NUMBER

    |

    ]

     [

    insert

     your

     job

     title

    ]

     ||

    |

    EMAIL

    _ADDRESS

    ||

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     
    


    The

     statement

     is

     concise

     as

     it

     only

     requires

     a

     single

     word

     or

     phrase

     to

     describe

     the

     city

    's

     status

     and

     location

     within

     France

    .

     It

     does

     not

     provide

     additional

     details

     or

     context

     about

     Paris

     other

     than

     its

     title

    .

     The

     statement

     is

     factual

     and

     can

     be

     summarized

     as

    :

     "

    Paris

     is

     the

     capital

     city

     of

     France

    ."

     The

     statement

     is

     not

     opinion

    ated

     or

     subjective

    ,

     as

     it

     does

     not

     include

     any

     personal

     opinions

     about

     Paris

    .

     The

     statement

     is

     also

     not

     pres

    criptive

    ,

     as

     it

     does

     not

     provide

     specific

     information

     about

     the

     city

    's

     history

    ,

     culture

    ,

     or

     architecture

    .

     The

     statement

     is

     clear

     and

     un

    ambiguous

    ,

     making

     it

     easy

     for

     people

     to

     understand

     and

     remember

    .

     The

     statement

     also

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     highly

     uncertain

    ,

     but

     there

     are

     several

     potential

     trends

     that

     are

     likely

     to

     shape

     its

     trajectory

    .

     Here

     are

     some

     possible

     future

     trends

     in

     AI

    :
    


    1

    .

     Increased

     focus

     on

     ethical

     AI

    :

     As

     concerns

     around

     AI

     become

     more

     acute

    ,

     there

     will

     be

     increased

     focus

     on

     ethical

     AI

    .

     This

     could

     involve

     developing

     systems

     that

     minimize

     bias

    ,

     enhance

     transparency

    ,

     and

     protect

     privacy

    .
    


    2

    .

     Adv

    ancements

     in

     machine

     learning

     and

     deep

     learning

    :

     Advances

     in

     machine

     learning

     and

     deep

     learning

     are

     likely

     to

     continue

     to

     drive

     the

     future

     of

     AI

    .

     This

     could

     involve

     new

     algorithms

     and

     techniques

     for

     solving

     complex

     problems

    ,

     as

     well

     as

     the

     development

     of

     new

     types

     of

     neural

     networks

    .
    


    3

    .

     Integration

     with

     other

     technologies

    :

     As

    



```python
llm.shutdown()
```
