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


    2026-03-30 14:05:03.712 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 14:05:03] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 14:05:03.712 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 14:05:03] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 14:05:03.712 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 14:05:03] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 14:05:03.712 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 14:05:03] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 14:05:03.712 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 14:05:03] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.70it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.70it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:06,  2.22s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:06,  2.22s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:06,  2.22s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:06,  2.22s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:24,  2.23it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:24,  2.23it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:24,  2.23it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:24,  2.23it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:24,  2.23it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:24,  2.23it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:24,  2.23it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.75it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.75it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.75it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.75it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.75it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.75it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.75it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.75it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.75it/s]Compiling num tokens (num_tokens=1536):  17%|█▋        | 10/58 [00:02<00:07,  6.75it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 14.98it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 14.98it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 14.98it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 14.98it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 14.98it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 14.98it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 14.98it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:02<00:02, 14.98it/s]

    Compiling num tokens (num_tokens=640):  33%|███▎      | 19/58 [00:02<00:02, 14.98it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:02<00:01, 22.85it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:02<00:01, 22.85it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:02<00:01, 22.85it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:02<00:01, 22.85it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:02<00:01, 22.85it/s]Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:02<00:01, 22.85it/s]Compiling num tokens (num_tokens=384):  47%|████▋     | 27/58 [00:02<00:01, 22.85it/s]Compiling num tokens (num_tokens=352):  47%|████▋     | 27/58 [00:02<00:01, 22.85it/s]Compiling num tokens (num_tokens=320):  47%|████▋     | 27/58 [00:02<00:01, 22.85it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:02<00:00, 31.09it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:02<00:00, 31.09it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:02<00:00, 31.09it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:02<00:00, 31.09it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:02<00:00, 31.09it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:02<00:00, 31.09it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:02<00:00, 31.09it/s]

    Compiling num tokens (num_tokens=176):  60%|██████    | 35/58 [00:02<00:00, 31.09it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:02<00:00, 37.75it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:02<00:00, 37.75it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:02<00:00, 37.75it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:02<00:00, 37.75it/s]Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:02<00:00, 37.75it/s]Compiling num tokens (num_tokens=96):  72%|███████▏  | 42/58 [00:02<00:00, 37.75it/s] Compiling num tokens (num_tokens=80):  72%|███████▏  | 42/58 [00:02<00:00, 37.75it/s]Compiling num tokens (num_tokens=64):  72%|███████▏  | 42/58 [00:02<00:00, 37.75it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:02<00:00, 43.49it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:02<00:00, 43.49it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:02<00:00, 43.49it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:03<00:00, 43.49it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:03<00:00, 43.49it/s]Compiling num tokens (num_tokens=20):  84%|████████▍ | 49/58 [00:03<00:00, 43.49it/s]Compiling num tokens (num_tokens=16):  84%|████████▍ | 49/58 [00:03<00:00, 43.49it/s]Compiling num tokens (num_tokens=12):  84%|████████▍ | 49/58 [00:03<00:00, 43.49it/s]Compiling num tokens (num_tokens=8):  84%|████████▍ | 49/58 [00:03<00:00, 43.49it/s] 

    Compiling num tokens (num_tokens=4):  84%|████████▍ | 49/58 [00:03<00:00, 43.49it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 18.93it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=59.18 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=59.15 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=59.15 GB):   3%|▎         | 2/58 [00:00<00:02, 19.12it/s]Capturing num tokens (num_tokens=7168 avail_mem=59.15 GB):   3%|▎         | 2/58 [00:00<00:02, 19.12it/s]Capturing num tokens (num_tokens=6656 avail_mem=59.15 GB):   3%|▎         | 2/58 [00:00<00:02, 19.12it/s]Capturing num tokens (num_tokens=6144 avail_mem=59.15 GB):   3%|▎         | 2/58 [00:00<00:02, 19.12it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=59.15 GB):   9%|▊         | 5/58 [00:00<00:02, 22.43it/s]Capturing num tokens (num_tokens=5632 avail_mem=59.14 GB):   9%|▊         | 5/58 [00:00<00:02, 22.43it/s]Capturing num tokens (num_tokens=5120 avail_mem=59.15 GB):   9%|▊         | 5/58 [00:00<00:02, 22.43it/s]Capturing num tokens (num_tokens=4608 avail_mem=59.14 GB):   9%|▊         | 5/58 [00:00<00:02, 22.43it/s]Capturing num tokens (num_tokens=4096 avail_mem=59.14 GB):   9%|▊         | 5/58 [00:00<00:02, 22.43it/s]Capturing num tokens (num_tokens=4096 avail_mem=59.14 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.51it/s]Capturing num tokens (num_tokens=3840 avail_mem=59.14 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.51it/s]Capturing num tokens (num_tokens=3584 avail_mem=59.13 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.51it/s]Capturing num tokens (num_tokens=3328 avail_mem=59.13 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.51it/s]Capturing num tokens (num_tokens=3072 avail_mem=59.13 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.51it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=59.13 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.51it/s]Capturing num tokens (num_tokens=2816 avail_mem=59.13 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.67it/s]Capturing num tokens (num_tokens=2560 avail_mem=59.12 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.67it/s]Capturing num tokens (num_tokens=2304 avail_mem=59.12 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.67it/s]Capturing num tokens (num_tokens=2048 avail_mem=59.11 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.67it/s]Capturing num tokens (num_tokens=1792 avail_mem=59.11 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.67it/s]Capturing num tokens (num_tokens=1536 avail_mem=59.11 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.67it/s]Capturing num tokens (num_tokens=1280 avail_mem=59.10 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.67it/s]Capturing num tokens (num_tokens=1280 avail_mem=59.10 GB):  34%|███▍      | 20/58 [00:00<00:00, 39.84it/s]Capturing num tokens (num_tokens=1024 avail_mem=59.08 GB):  34%|███▍      | 20/58 [00:00<00:00, 39.84it/s]Capturing num tokens (num_tokens=960 avail_mem=59.10 GB):  34%|███▍      | 20/58 [00:00<00:00, 39.84it/s] Capturing num tokens (num_tokens=896 avail_mem=59.09 GB):  34%|███▍      | 20/58 [00:00<00:00, 39.84it/s]Capturing num tokens (num_tokens=832 avail_mem=59.09 GB):  34%|███▍      | 20/58 [00:00<00:00, 39.84it/s]

    Capturing num tokens (num_tokens=768 avail_mem=59.09 GB):  34%|███▍      | 20/58 [00:00<00:00, 39.84it/s]Capturing num tokens (num_tokens=704 avail_mem=59.08 GB):  34%|███▍      | 20/58 [00:00<00:00, 39.84it/s]Capturing num tokens (num_tokens=704 avail_mem=59.08 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.07it/s]Capturing num tokens (num_tokens=640 avail_mem=59.08 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.07it/s]Capturing num tokens (num_tokens=576 avail_mem=59.08 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.07it/s]Capturing num tokens (num_tokens=512 avail_mem=59.07 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.07it/s]Capturing num tokens (num_tokens=480 avail_mem=59.08 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.07it/s]Capturing num tokens (num_tokens=448 avail_mem=59.08 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.07it/s]Capturing num tokens (num_tokens=416 avail_mem=59.08 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.07it/s]Capturing num tokens (num_tokens=416 avail_mem=59.08 GB):  55%|█████▌    | 32/58 [00:00<00:00, 46.90it/s]Capturing num tokens (num_tokens=384 avail_mem=59.08 GB):  55%|█████▌    | 32/58 [00:00<00:00, 46.90it/s]Capturing num tokens (num_tokens=352 avail_mem=59.07 GB):  55%|█████▌    | 32/58 [00:00<00:00, 46.90it/s]Capturing num tokens (num_tokens=320 avail_mem=59.07 GB):  55%|█████▌    | 32/58 [00:00<00:00, 46.90it/s]

    Capturing num tokens (num_tokens=288 avail_mem=59.07 GB):  55%|█████▌    | 32/58 [00:00<00:00, 46.90it/s]Capturing num tokens (num_tokens=256 avail_mem=59.06 GB):  55%|█████▌    | 32/58 [00:00<00:00, 46.90it/s]Capturing num tokens (num_tokens=240 avail_mem=59.06 GB):  55%|█████▌    | 32/58 [00:00<00:00, 46.90it/s]Capturing num tokens (num_tokens=240 avail_mem=59.06 GB):  66%|██████▌   | 38/58 [00:00<00:00, 48.64it/s]Capturing num tokens (num_tokens=224 avail_mem=59.06 GB):  66%|██████▌   | 38/58 [00:00<00:00, 48.64it/s]Capturing num tokens (num_tokens=208 avail_mem=59.05 GB):  66%|██████▌   | 38/58 [00:00<00:00, 48.64it/s]Capturing num tokens (num_tokens=192 avail_mem=59.05 GB):  66%|██████▌   | 38/58 [00:00<00:00, 48.64it/s]Capturing num tokens (num_tokens=176 avail_mem=59.05 GB):  66%|██████▌   | 38/58 [00:00<00:00, 48.64it/s]Capturing num tokens (num_tokens=160 avail_mem=59.05 GB):  66%|██████▌   | 38/58 [00:00<00:00, 48.64it/s]Capturing num tokens (num_tokens=144 avail_mem=59.04 GB):  66%|██████▌   | 38/58 [00:01<00:00, 48.64it/s]Capturing num tokens (num_tokens=144 avail_mem=59.04 GB):  76%|███████▌  | 44/58 [00:01<00:00, 49.93it/s]Capturing num tokens (num_tokens=128 avail_mem=59.04 GB):  76%|███████▌  | 44/58 [00:01<00:00, 49.93it/s]Capturing num tokens (num_tokens=112 avail_mem=59.04 GB):  76%|███████▌  | 44/58 [00:01<00:00, 49.93it/s]

    Capturing num tokens (num_tokens=96 avail_mem=59.03 GB):  76%|███████▌  | 44/58 [00:01<00:00, 49.93it/s] Capturing num tokens (num_tokens=80 avail_mem=59.03 GB):  76%|███████▌  | 44/58 [00:01<00:00, 49.93it/s]Capturing num tokens (num_tokens=64 avail_mem=59.03 GB):  76%|███████▌  | 44/58 [00:01<00:00, 49.93it/s]Capturing num tokens (num_tokens=48 avail_mem=59.02 GB):  76%|███████▌  | 44/58 [00:01<00:00, 49.93it/s]Capturing num tokens (num_tokens=48 avail_mem=59.02 GB):  86%|████████▌ | 50/58 [00:01<00:00, 50.49it/s]Capturing num tokens (num_tokens=32 avail_mem=59.02 GB):  86%|████████▌ | 50/58 [00:01<00:00, 50.49it/s]Capturing num tokens (num_tokens=28 avail_mem=59.01 GB):  86%|████████▌ | 50/58 [00:01<00:00, 50.49it/s]Capturing num tokens (num_tokens=24 avail_mem=59.01 GB):  86%|████████▌ | 50/58 [00:01<00:00, 50.49it/s]Capturing num tokens (num_tokens=20 avail_mem=59.01 GB):  86%|████████▌ | 50/58 [00:01<00:00, 50.49it/s]Capturing num tokens (num_tokens=16 avail_mem=59.01 GB):  86%|████████▌ | 50/58 [00:01<00:00, 50.49it/s]Capturing num tokens (num_tokens=12 avail_mem=59.00 GB):  86%|████████▌ | 50/58 [00:01<00:00, 50.49it/s]Capturing num tokens (num_tokens=12 avail_mem=59.00 GB):  97%|█████████▋| 56/58 [00:01<00:00, 51.02it/s]Capturing num tokens (num_tokens=8 avail_mem=59.00 GB):  97%|█████████▋| 56/58 [00:01<00:00, 51.02it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=59.00 GB):  97%|█████████▋| 56/58 [00:01<00:00, 51.02it/s]Capturing num tokens (num_tokens=4 avail_mem=59.00 GB): 100%|██████████| 58/58 [00:01<00:00, 44.39it/s]


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
    Generated text:  George. I am a 27-year-old man from Canada. I am a full-time student at the University of Ottawa. My current college course is college history. At the same time, I like to read and watch movies. I have a great interest in Disney movies and Disney animations. I would like to learn more about the history of the Disney company. I am very interested in Disney's history and its great achievements. I would like to read about the company's history from the 1920s to the present day. I would like to learn more about the company's history from the 1920s
    ===============================
    Prompt: The president of the United States is
    Generated text:  from which country?
    The president of the United States is from the United States of America, which is a country located in North America. The United States is known for its long history of political stability, economic growth, and democratic values. The country was established in 1776 after the American Revolution and has since become one of the world's most populous countries, with an estimated population of over 335 million as of 2021. The United States is a federal republic, with a president serving as the head of state and the commander-in-chief of the armed forces. The president is elected to office through a
    ===============================
    Prompt: The capital of France is
    Generated text:  ________.
    A. Paris
    B. London
    C. Moscow
    D. New York
    Answer:
    
    A
    
    For patients with severe trauma, the most critical immediate nursing measure is
    A. Administering sedatives
    B. Performing wound care
    C. Monitoring vital signs
    D. Administering pain relief
    E. Performing skin care
    Answer:
    
    C
    
    When elderly people consume high-calorie foods, the primary concern is
    A. to lose weight
    B. to maintain weight
    C. to reduce fat
    D. to improve energy status
    E. to reduce body weight
    Answer:
    
    D
    
    The ad
    ===============================
    Prompt: The future of AI is
    Generated text:  bright
    10/22/2023
    AI is progressing at an unprecedented rate of innovation, and as such the future of AI is bright.
    For years, the power of AI has been seen as a limited force. The idea that AI can automatically solve complex and challenging problems can be very appealing, but it’s important to note that AI has the potential to do far more. That’s why it’s important to think about the future of AI carefully.
    AI, it’s now, is a force for good, and for the betterment of society. The world has been in a state of constant change for many years


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [insert a short description of your character or personality]. I enjoy [insert a short description of your hobbies or interests]. I'm always looking for new experiences and learning opportunities. What do you like to do in your free time? I enjoy [insert a short description of your hobbies or interests]. I'm always looking for new experiences and learning opportunities. What do you like to do in your free time? I enjoy [insert a short description of
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French Academy of Sciences, and the French National Library. Paris is a cultural and historical center with a rich history dating back to the Roman Empire and the Middle Ages. It is also a major financial and business hub, with many international companies and institutions headquartered there. The city is known for its fashion, art, and cuisine, and is a popular tourist destination. Paris is a vibrant and dynamic city with a diverse population and a rich cultural heritage. It
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends that could be expected in the future:
    
    1. Increased automation: AI is likely to become more prevalent in manufacturing, transportation, and other industries, where it can help automate repetitive tasks and improve efficiency. This could lead to the creation of new jobs, but it could also create new opportunities for people to work in areas like data analysis and machine learning.
    
    2. Improved privacy and security: As AI becomes more integrated into our daily lives, there will be a growing need for secure and private
    


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
    Generated text:  Sarah, and I am a tech-savvy and curious person who is always exploring new things. I enjoy coding, experimenting with new technologies, and learning from the internet. I am also a strong believer in taking risks and trying new things to see what the future holds. I am looking forward to meeting new people and learning more about the world around me. Thank you for asking. Is there anything else you would like me to know about me? I am happy to tell you about myself. What's your favorite hobby or activity? I enjoy playing video games and listening to music. I also love to read and watch movies and TV shows
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. The city is also famous for its cuisine and its role in French culture and politics. 
    
    This statement is factually accurate as it provides the reader with a brief overview of Paris, including its key attractions, cultural significance, and historical context. It is a concise representation of the city's importance and appeal to both locals and visitors. 
    
    For those interested in learning more about Paris, I recommend visiting the official French government website or seeking out cultural events and exhibitions in the city. Alternatively, you can explore the city by
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  bright and full of promise, with several key trends shaping its development and expanding its applications:
    
    1. Increased focus on ethical AI: As concerns about bias and privacy grow, there's a push towards more ethical and transparent AI systems that can help guide decision-making rather than perpetuate bias. This includes initiatives like the European Union's General Data Protection Regulation (GDPR) and the California Consumer Privacy Act (CCPA).
    
    2. Improved hardware and software: As AI algorithms get more complex, the hardware and software required to run them will likely get even more powerful. This could lead to even more sophisticated models, potentially making AI systems more powerful than


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

    ],

     and

     I

    'm

     a

     [

    insert

     profession

    ,

     role

    ,

     etc

    .

    ].

     I

    'm

     a

     [

    insert

     age

    ,

     height

    ,

     weight

    ,

     etc

    .

    ].

     I

     love

     [

    insert

     something

     I

     enjoy

    ,

     such

     as

     music

    ,

     sports

    ,

     art

    ,

     etc

    .

    ].

     And

     I

    'm

     an

     [

    insert

     a

     personality

     trait

    ,

     such

     as

     empath

    etic

    ,

     adventurous

    ,

     kind

    ,

     etc

    .

    ].

     I

     think

     [

    insert

     how

     I

     see

     the

     world

    ,

     such

     as

     smart

    ,

     clever

    ,

     creative

    ,

     etc

    .

    ].

     And

     I

    'm

     always

     learning

     something

     new

    .

     I

    'm

     always

     willing

     to

     take

     risks

    ,

     try

     new

     things

    ,

     and

     be

     open

     to

     new

     experiences

    .

     I

    'm

     a

     [

    insert

     a

     hobby

    ,

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .
    


    Paris

     is

     the

     largest

     city

     in

     France

     and

     one

     of

     the

     most

     visited

     cities

     in

     the

     world

    .

     It

     is

     known

     for

     its

     stunning

     architecture

    ,

     vibrant

     nightlife

    ,

     and

     rich

     history

    ,

     including

     its

     role

     in

     the

     French

     Revolution

    .

     The

     city

     is

     also

     known

     for

     its

     annual

     Les

     B

    acc

    al

    aur

    é

    at

    ,

     a

     French

     annual

     poetry

     competition

    .

     Paris

     is

     a

     world

    -ren

    owned

     city

     with

     a

     rich

     cultural

     and

     artistic

     heritage

    ,

     and

     is

     a

     major

     economic

     and

     cultural

     hub

     of

     Europe

    .

     The

     city

     is

     also

     home

     to

     the

     Lou

    vre

     Museum

    ,

     the

     E

    iff

    el

     Tower

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

     a

     popular

     tourist

     destination

     and

     a

     global

     city

     of

     immense

     significance

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     characterized

     by

     rapid

     developments

     and

     advancements

    ,

     as

     well

     as

     increased

     integration

     with

     other

     fields

     such

     as

     healthcare

    ,

     energy

    ,

     and

     transportation

    .

     Here

     are

     some

     potential

     trends

     that

     may

     occur

     in

     the

     future

    :
    


    1

    .

     Increased

     Integration

     with

     Other

     Fields

    :

     AI

     is

     likely

     to

     become

     more

     integrated

     with

     other

     fields

    ,

     such

     as

     healthcare

    ,

     energy

    ,

     and

     transportation

    ,

     as

     these

     industries

     seek

     to

     optimize

     their

     operations

     and

     reduce

     their

     environmental

     impact

    .

     This

     integration

     could

     lead

     to

     new

     opportunities

     for

     AI

     to

     be

     used

     in

     these

     fields

    ,

     such

     as

     in

     the

     development

     of

     more

     efficient

     medical

     imaging

     systems

     or

     in

     the

     design

     of

     more

     sustainable

     transportation

     systems

    .
    


    2

    .

     Enhanced

     Machine

     Learning

    :

     The

     capabilities

     of

     AI

    



```python
llm.shutdown()
```
