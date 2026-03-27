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


    2026-03-27 08:48:18.106 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 08:48:18] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 08:48:18.106 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 08:48:18] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 08:48:18.106 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 08:48:18] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 08:48:18.106 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 08:48:18] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 08:48:18.106 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 08:48:18] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.14it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.13it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:17,  2.41s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:17,  2.41s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:17,  2.41s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:17,  2.41s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:26,  2.05it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:26,  2.05it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:26,  2.05it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:26,  2.05it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:26,  2.05it/s]

    Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:26,  2.05it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:26,  2.05it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.24it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.24it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.24it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.24it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.24it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.24it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.24it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.24it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.24it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:03, 13.07it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:03, 13.07it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:02<00:03, 13.07it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:02<00:03, 13.07it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:02<00:03, 13.07it/s] 

    Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:02<00:03, 13.07it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:02<00:03, 13.07it/s]Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:02<00:03, 13.07it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:02<00:01, 19.40it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:02<00:01, 19.40it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:02<00:01, 19.40it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:02<00:01, 19.40it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:02<00:01, 19.40it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:02<00:01, 19.40it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:02<00:01, 19.40it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:02<00:01, 19.40it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:02<00:00, 26.16it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:02<00:00, 26.16it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:02<00:00, 26.16it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:00, 26.16it/s]

    Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:00, 26.16it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:00, 26.16it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:00, 26.16it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 31.12it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 31.12it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 31.12it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 31.12it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 31.12it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 31.12it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 31.12it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 34.71it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 34.71it/s]

    Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 34.71it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 34.71it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 34.71it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 34.71it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 34.71it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 37.98it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 37.98it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 37.98it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 37.98it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 37.98it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 37.98it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 37.98it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 37.98it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 37.98it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 16.85it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=121.05 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=121.02 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=121.02 GB):   3%|▎         | 2/58 [00:00<00:03, 18.16it/s]Capturing num tokens (num_tokens=7168 avail_mem=121.02 GB):   3%|▎         | 2/58 [00:00<00:03, 18.16it/s]Capturing num tokens (num_tokens=6656 avail_mem=121.02 GB):   3%|▎         | 2/58 [00:00<00:03, 18.16it/s]Capturing num tokens (num_tokens=6144 avail_mem=121.02 GB):   3%|▎         | 2/58 [00:00<00:03, 18.16it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=121.02 GB):   9%|▊         | 5/58 [00:00<00:02, 21.43it/s]Capturing num tokens (num_tokens=5632 avail_mem=121.01 GB):   9%|▊         | 5/58 [00:00<00:02, 21.43it/s]Capturing num tokens (num_tokens=5120 avail_mem=121.01 GB):   9%|▊         | 5/58 [00:00<00:02, 21.43it/s]Capturing num tokens (num_tokens=4608 avail_mem=121.01 GB):   9%|▊         | 5/58 [00:00<00:02, 21.43it/s]Capturing num tokens (num_tokens=4096 avail_mem=121.01 GB):   9%|▊         | 5/58 [00:00<00:02, 21.43it/s]Capturing num tokens (num_tokens=4096 avail_mem=121.01 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.12it/s]Capturing num tokens (num_tokens=3840 avail_mem=121.01 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.12it/s]Capturing num tokens (num_tokens=3584 avail_mem=121.00 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.12it/s]Capturing num tokens (num_tokens=3328 avail_mem=121.00 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.12it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=120.99 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.12it/s]Capturing num tokens (num_tokens=3072 avail_mem=120.99 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.74it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.99 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.74it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.99 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.74it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.99 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.74it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.98 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.74it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.98 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.74it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.98 GB):  31%|███       | 18/58 [00:00<00:01, 35.18it/s]Capturing num tokens (num_tokens=1536 avail_mem=120.98 GB):  31%|███       | 18/58 [00:00<00:01, 35.18it/s]Capturing num tokens (num_tokens=1280 avail_mem=120.97 GB):  31%|███       | 18/58 [00:00<00:01, 35.18it/s]Capturing num tokens (num_tokens=1024 avail_mem=120.95 GB):  31%|███       | 18/58 [00:00<00:01, 35.18it/s]

    Capturing num tokens (num_tokens=960 avail_mem=120.96 GB):  31%|███       | 18/58 [00:00<00:01, 35.18it/s] Capturing num tokens (num_tokens=960 avail_mem=120.96 GB):  38%|███▊      | 22/58 [00:00<00:01, 34.57it/s]Capturing num tokens (num_tokens=896 avail_mem=120.96 GB):  38%|███▊      | 22/58 [00:00<00:01, 34.57it/s]Capturing num tokens (num_tokens=832 avail_mem=120.96 GB):  38%|███▊      | 22/58 [00:00<00:01, 34.57it/s]Capturing num tokens (num_tokens=768 avail_mem=120.95 GB):  38%|███▊      | 22/58 [00:00<00:01, 34.57it/s]Capturing num tokens (num_tokens=704 avail_mem=120.95 GB):  38%|███▊      | 22/58 [00:00<00:01, 34.57it/s]Capturing num tokens (num_tokens=704 avail_mem=120.95 GB):  45%|████▍     | 26/58 [00:00<00:00, 35.34it/s]Capturing num tokens (num_tokens=640 avail_mem=120.95 GB):  45%|████▍     | 26/58 [00:00<00:00, 35.34it/s]Capturing num tokens (num_tokens=576 avail_mem=120.95 GB):  45%|████▍     | 26/58 [00:00<00:00, 35.34it/s]

    Capturing num tokens (num_tokens=512 avail_mem=120.94 GB):  45%|████▍     | 26/58 [00:01<00:00, 35.34it/s]Capturing num tokens (num_tokens=480 avail_mem=120.95 GB):  45%|████▍     | 26/58 [00:01<00:00, 35.34it/s]Capturing num tokens (num_tokens=480 avail_mem=120.95 GB):  52%|█████▏    | 30/58 [00:01<00:01, 23.73it/s]Capturing num tokens (num_tokens=448 avail_mem=120.95 GB):  52%|█████▏    | 30/58 [00:01<00:01, 23.73it/s]Capturing num tokens (num_tokens=416 avail_mem=120.95 GB):  52%|█████▏    | 30/58 [00:01<00:01, 23.73it/s]

    Capturing num tokens (num_tokens=384 avail_mem=120.95 GB):  52%|█████▏    | 30/58 [00:01<00:01, 23.73it/s]Capturing num tokens (num_tokens=384 avail_mem=120.95 GB):  57%|█████▋    | 33/58 [00:01<00:01, 19.76it/s]Capturing num tokens (num_tokens=352 avail_mem=120.94 GB):  57%|█████▋    | 33/58 [00:01<00:01, 19.76it/s]Capturing num tokens (num_tokens=320 avail_mem=120.94 GB):  57%|█████▋    | 33/58 [00:01<00:01, 19.76it/s]Capturing num tokens (num_tokens=288 avail_mem=120.93 GB):  57%|█████▋    | 33/58 [00:01<00:01, 19.76it/s]Capturing num tokens (num_tokens=256 avail_mem=120.93 GB):  57%|█████▋    | 33/58 [00:01<00:01, 19.76it/s]Capturing num tokens (num_tokens=256 avail_mem=120.93 GB):  64%|██████▍   | 37/58 [00:01<00:00, 22.62it/s]Capturing num tokens (num_tokens=240 avail_mem=120.93 GB):  64%|██████▍   | 37/58 [00:01<00:00, 22.62it/s]

    Capturing num tokens (num_tokens=224 avail_mem=120.93 GB):  64%|██████▍   | 37/58 [00:01<00:00, 22.62it/s]Capturing num tokens (num_tokens=208 avail_mem=120.92 GB):  64%|██████▍   | 37/58 [00:01<00:00, 22.62it/s]Capturing num tokens (num_tokens=192 avail_mem=120.92 GB):  64%|██████▍   | 37/58 [00:01<00:00, 22.62it/s]Capturing num tokens (num_tokens=176 avail_mem=120.92 GB):  64%|██████▍   | 37/58 [00:01<00:00, 22.62it/s]Capturing num tokens (num_tokens=176 avail_mem=120.92 GB):  72%|███████▏  | 42/58 [00:01<00:00, 26.40it/s]Capturing num tokens (num_tokens=160 avail_mem=120.91 GB):  72%|███████▏  | 42/58 [00:01<00:00, 26.40it/s]

    Capturing num tokens (num_tokens=144 avail_mem=120.91 GB):  72%|███████▏  | 42/58 [00:01<00:00, 26.40it/s]Capturing num tokens (num_tokens=128 avail_mem=120.91 GB):  72%|███████▏  | 42/58 [00:01<00:00, 26.40it/s]Capturing num tokens (num_tokens=128 avail_mem=120.91 GB):  78%|███████▊  | 45/58 [00:01<00:00, 23.14it/s]Capturing num tokens (num_tokens=112 avail_mem=120.91 GB):  78%|███████▊  | 45/58 [00:01<00:00, 23.14it/s]Capturing num tokens (num_tokens=96 avail_mem=120.90 GB):  78%|███████▊  | 45/58 [00:01<00:00, 23.14it/s] Capturing num tokens (num_tokens=80 avail_mem=120.90 GB):  78%|███████▊  | 45/58 [00:01<00:00, 23.14it/s]Capturing num tokens (num_tokens=64 avail_mem=120.89 GB):  78%|███████▊  | 45/58 [00:01<00:00, 23.14it/s]Capturing num tokens (num_tokens=48 avail_mem=120.89 GB):  78%|███████▊  | 45/58 [00:01<00:00, 23.14it/s]Capturing num tokens (num_tokens=48 avail_mem=120.89 GB):  86%|████████▌ | 50/58 [00:01<00:00, 27.53it/s]Capturing num tokens (num_tokens=32 avail_mem=120.89 GB):  86%|████████▌ | 50/58 [00:01<00:00, 27.53it/s]Capturing num tokens (num_tokens=28 avail_mem=120.88 GB):  86%|████████▌ | 50/58 [00:01<00:00, 27.53it/s]

    Capturing num tokens (num_tokens=24 avail_mem=120.88 GB):  86%|████████▌ | 50/58 [00:01<00:00, 27.53it/s]Capturing num tokens (num_tokens=20 avail_mem=120.88 GB):  86%|████████▌ | 50/58 [00:01<00:00, 27.53it/s]Capturing num tokens (num_tokens=16 avail_mem=120.88 GB):  86%|████████▌ | 50/58 [00:01<00:00, 27.53it/s]Capturing num tokens (num_tokens=16 avail_mem=120.88 GB):  95%|█████████▍| 55/58 [00:01<00:00, 31.62it/s]Capturing num tokens (num_tokens=12 avail_mem=120.87 GB):  95%|█████████▍| 55/58 [00:01<00:00, 31.62it/s]Capturing num tokens (num_tokens=8 avail_mem=120.87 GB):  95%|█████████▍| 55/58 [00:02<00:00, 31.62it/s] Capturing num tokens (num_tokens=4 avail_mem=120.87 GB):  95%|█████████▍| 55/58 [00:02<00:00, 31.62it/s]Capturing num tokens (num_tokens=4 avail_mem=120.87 GB): 100%|██████████| 58/58 [00:02<00:00, 28.06it/s]


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
    Generated text:  Emily and I am 19 years old. I was born in 2010 and I have an older brother, Gabriel. I have two older brothers, Luke and Martin. I have a younger sister named Sara. I do not have a father, but my parents are deceased. I am currently in the third grade, but my education is very challenging. I have very little attention span and I have trouble focusing on anything for long periods of time. My parents are also very worried about me, because I am having trouble with my studies. They have taken me to many doctors but they have not found a solution. They have
    ===============================
    Prompt: The president of the United States is
    Generated text:  a person who leads the country. The president is elected by the people to lead the country. His job is to help the country work well. The president is there to make important decisions to run the country.
    The president of the United States is like the boss of the country. He or she makes sure that the country is safe and that the country has what it needs to run properly. Sometimes, the president has to use a lot of power and it can be hard to understand how the president works. The president also has to make important decisions that are hard for the people to understand.
    The president is in charge of the country. They
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. It is the capital of the country of France. The population of Paris is about 2 million people.
    
    Please answer the following questions:
    
    1. In what language is the capital of France spoken? English
    2. In what language is the Paris of France written? French
    3. What is the capital of France? Paris
    4. Which language is written on the money of France? French
    5. How many people live in Paris?
    6. What is the population of Paris in millions?
    7. How many languages are spoken in the capital of France?
    8. How many languages are written in the capital of France?
    
    ===============================
    Prompt: The future of AI is
    Generated text:  bright, but it will also bring some serious challenges. Here are four things that have to be taken into consideration:
    
    1. The autonomy of the AI system
    
    From a legal perspective, if an AI system has autonomy, it means that the system is able to make decisions on its own. However, this means that the system can do things that are illegal or unethical, such as stealing data or harming other people. This creates some serious legal and ethical issues.
    
    2. The impact of AI on jobs
    
    AI is already having a significant impact on many industries, and it's likely to continue to do so in the future. However, some


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


    Generated text:  [Name] and I'm a [Age] year old [Gender] who has always been fascinated by [Subject] and has a keen interest in [Skill/Interest]. I'm always looking for new challenges and opportunities to learn and grow. I'm always eager to share my knowledge and experience with others and am always willing to help others. I'm a [Type of Person] who is always looking for ways to improve myself and make the world a better place. I'm always looking for new ways to learn and grow, and I'm always eager to share my knowledge and experience with others. I'm a [Type of Person]
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also famous for its rich history, including the French Revolution and the French Revolution Museum. Paris is a bustling city with a diverse population and is a major cultural and economic center in Europe. It is home to many famous landmarks and attractions, including the Louvre, the Eiffel Tower, and the Champs-Élysées. Paris is also known for its cuisine, including French cuisine and its famous cheese, brie. The city is a popular tourist destination and is home to many
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the way we live, work, and interact with technology. Here are some possible future trends in AI:
    
    1. Increased automation and artificial intelligence: As AI technology continues to improve, we can expect to see more automation and artificial intelligence in our daily lives. This could include things like self-driving cars, voice-activated assistants, and even more advanced forms of AI that can perform tasks that were previously done by humans.
    
    2. Enhanced privacy and security: As AI technology becomes more advanced, we can expect to see more privacy and security concerns. This could include things like the
    


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
    Generated text:  [Name], and I'm a [Occupation]! I'm [Age] years old, [Gender] and [Country]. I grew up in [Location] and I'm always up for learning new things, trying new foods, and playing games with my friends. What do you think makes me special? [Add 3-5 points about what makes me unique and why you think they're special about you!]
    [End of Self-Introduction] Now that I've had a chance to be a bit more introspective, let's dive into some of your daily activities. How about you? Do you have any hobbies or
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, the city known for its stunning architecture, rich cultural heritage, and vibrant music scene. The city is home to iconic landmarks such as the Eiffel Tower, the Louvre Museum, and the Notre-Dame Cathedral, as well as numerous museums, theaters, and restaurants that cater to its diverse population. Paris is also known for its French language, literature, and cuisine, making it a cultural hub and a major tourist destination for millions of visitors each year. Its status as the world's most populous city is reflected in its bustling streets and lively atmosphere, and it continues to grow and evolve as a major global city. Paris is
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to involve a range of complex and rapidly evolving technologies, with a significant focus on developing new applications and technologies that can help us achieve our goals and make the world a better place.
    
    One potential future trend in AI is the development of more sophisticated algorithms and models that can learn and adapt to new situations and information more effectively than previous generations. This could lead to the creation of more intuitive and effective decision-making tools and systems, as well as improved communication and collaboration skills between humans and machines.
    
    Another trend is the continued expansion of the AI sector, with more companies and organizations entering the field and investing in AI research and development. This could lead


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

    job

     title

    ]

     with

     [

    number

     of

     years

    ]

     years

     of

     experience

     in

     [

    job

     role

    ].

     I

     have

     a

     passion

     for

     [

    mention

     an

     activity

     you

     enjoy

     or

     a

     hobby

     you

    're

     particularly

     interested

     in

    ].

     I

     love

     [

    mention

     any

     activities

    ,

     sports

    ,

     or

     hobbies

     you

     have

    ].

     I

     have

     a

     strong

     work

     ethic

     and

     always

     strive

     to

     be

     the

     best

     version

     of

     myself

    .

     I

    'm

     highly

     organized

     and

     efficient

    ,

     always

     finding

     the

     best

     solution

     to

     a

     problem

    .

     I

    'm

     confident

     in

     my

     abilities

     and

     always

     aim

     to

     exceed

     expectations

    .

     I

     am

     always

     looking

     for

     ways

     to

     improve

     myself

     and

     the

     people

     around

     me

    .

     I

     thrive

     on

     continuous

     learning

     and

     growth

    .

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .


    Paris

     is

     a

     historical

     city

     in

     western

     France

    ,

     known

     for

     its

     rich

     history

    ,

     art

    ,

     and

     cuisine

    .

     It

     is

     one

     of

     the

     world

    's

     most

     populous

     cities

     and

     one

     of

     the

     world

    's

     most

     important

     cultural

     and

     political

     centers

    .

     Paris

     is

     famous

     for

     its

     E

    iff

    el

     Tower

    ,

     Lou

    vre

     Museum

    ,

     Notre

    -D

    ame

     Cathedral

    ,

     and

     other

     iconic

     landmarks

    .

     The

     city

     is

     also

     known

     for

     its

     extensive

     food

     and

     wine

     industry

    ,

     as

     well

     as

     its

     fashion

     industry

     and

     music

     scene

    .

     Paris

     is

     a

     bustling

    ,

     vibrant

     city

     with

     a

     rich

     culture

     and

     a

     rich

     history

    .

     Its

     status

     as

     a

     major

     global

     city

     means

     that

     it

     is

     a

     world

    -class

     destination

     for

     business

     and

     tourism

    .

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     highly

     uncertain

     and

     uncertain

    .

     However

    ,

     there

     are

     several

     possible

     trends

     that

     can

     be

     expected

    :
    


    1

    .

     Automation

    :

     AI

     systems

     are

     expected

     to

     become

     more

     and

     more

     automated

    ,

     with

     a

     focus

     on

     maximizing

     efficiency

     and

     reducing

     the

     need

     for

     human

     intervention

    .
    


    2

    .

     Autonomous

     agents

    :

     The

     idea

     of

     autonomous

     agents

     is

     gaining

     traction

    ,

     with

     the

     possibility

     of

     machines

     that

     can

     make

     decisions

     and

     take

     actions

     without

     human

     intervention

    .
    


    3

    .

     Deep

     learning

    :

     Deep

     learning

     is

     likely

     to

     continue

     to

     evolve

     and

     become

     more

     powerful

    ,

     with

     the

     potential

     to

     handle

     increasingly

     complex

     tasks

     and

     achieve

     higher

     levels

     of

     accuracy

    .
    


    4

    .

     Ethics

     and

     governance

    :

     The

     future

     of

     AI

     will

     likely

     be

     influenced

     by

     ethical

     considerations

    ,

     such

    



```python
llm.shutdown()
```
