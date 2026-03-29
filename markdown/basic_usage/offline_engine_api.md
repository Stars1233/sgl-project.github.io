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


    2026-03-29 19:45:43.560 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 19:45:43] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 19:45:43.561 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 19:45:43] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 19:45:43.561 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 19:45:43] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 19:45:43.561 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 19:45:43] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 19:45:43.561 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 19:45:43] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.09it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.08it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:14,  2.36s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:14,  2.36s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:14,  2.36s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:14,  2.36s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:14,  2.36s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:19,  2.66it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:19,  2.66it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:19,  2.66it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:19,  2.66it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:19,  2.66it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:19,  2.66it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:19,  2.66it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:06,  6.92it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:06,  6.92it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:06,  6.92it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:06,  6.92it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:06,  6.92it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:06,  6.92it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:02<00:06,  6.92it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:02<00:06,  6.92it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:02<00:06,  6.92it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 13.73it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 13.73it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 13.73it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 13.73it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 13.73it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 13.73it/s]

    Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 13.73it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:02<00:02, 13.73it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:02<00:01, 20.16it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:02<00:01, 20.16it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:02<00:01, 20.16it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:02<00:01, 20.16it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:02<00:01, 20.16it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:02<00:01, 20.16it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:02<00:01, 20.16it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:02<00:01, 20.16it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:02<00:00, 26.98it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:02<00:00, 26.98it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:02<00:00, 26.98it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:02<00:00, 26.98it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:02<00:00, 26.98it/s]

    Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:02<00:00, 26.98it/s]Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:03<00:00, 26.98it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 31.94it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 31.94it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 31.94it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 31.94it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 31.94it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 31.94it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 31.94it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 36.66it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 36.66it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 36.66it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 36.66it/s]

    Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 36.66it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 36.66it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 36.66it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 40.48it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 40.48it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 40.48it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 40.48it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 40.48it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 40.48it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 40.48it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 40.48it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.39it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=121.74 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=121.71 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=121.71 GB):   3%|▎         | 2/58 [00:00<00:02, 18.68it/s]Capturing num tokens (num_tokens=7168 avail_mem=121.70 GB):   3%|▎         | 2/58 [00:00<00:02, 18.68it/s]Capturing num tokens (num_tokens=6656 avail_mem=121.70 GB):   3%|▎         | 2/58 [00:00<00:02, 18.68it/s]Capturing num tokens (num_tokens=6144 avail_mem=121.43 GB):   3%|▎         | 2/58 [00:00<00:02, 18.68it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=121.43 GB):   9%|▊         | 5/58 [00:00<00:02, 21.10it/s]Capturing num tokens (num_tokens=5632 avail_mem=121.43 GB):   9%|▊         | 5/58 [00:00<00:02, 21.10it/s]Capturing num tokens (num_tokens=5120 avail_mem=120.78 GB):   9%|▊         | 5/58 [00:00<00:02, 21.10it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.72 GB):   9%|▊         | 5/58 [00:00<00:02, 21.10it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.72 GB):  14%|█▍        | 8/58 [00:00<00:02, 23.99it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.72 GB):  14%|█▍        | 8/58 [00:00<00:02, 23.99it/s]Capturing num tokens (num_tokens=3840 avail_mem=120.72 GB):  14%|█▍        | 8/58 [00:00<00:02, 23.99it/s]Capturing num tokens (num_tokens=3584 avail_mem=120.71 GB):  14%|█▍        | 8/58 [00:00<00:02, 23.99it/s]Capturing num tokens (num_tokens=3328 avail_mem=120.71 GB):  14%|█▍        | 8/58 [00:00<00:02, 23.99it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=120.71 GB):  21%|██        | 12/58 [00:00<00:01, 28.94it/s]Capturing num tokens (num_tokens=3072 avail_mem=120.70 GB):  21%|██        | 12/58 [00:00<00:01, 28.94it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.70 GB):  21%|██        | 12/58 [00:00<00:01, 28.94it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.70 GB):  21%|██        | 12/58 [00:00<00:01, 28.94it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.70 GB):  21%|██        | 12/58 [00:00<00:01, 28.94it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.69 GB):  21%|██        | 12/58 [00:00<00:01, 28.94it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.69 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.50it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.69 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.50it/s]Capturing num tokens (num_tokens=1536 avail_mem=120.69 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.50it/s]Capturing num tokens (num_tokens=1280 avail_mem=120.68 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.50it/s]Capturing num tokens (num_tokens=1024 avail_mem=120.66 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.50it/s]

    Capturing num tokens (num_tokens=960 avail_mem=120.68 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.50it/s] Capturing num tokens (num_tokens=960 avail_mem=120.68 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.18it/s]Capturing num tokens (num_tokens=896 avail_mem=120.67 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.18it/s]Capturing num tokens (num_tokens=832 avail_mem=120.67 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.18it/s]Capturing num tokens (num_tokens=768 avail_mem=120.67 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.18it/s]Capturing num tokens (num_tokens=704 avail_mem=120.33 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.18it/s]Capturing num tokens (num_tokens=640 avail_mem=120.23 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.18it/s]Capturing num tokens (num_tokens=640 avail_mem=120.23 GB):  47%|████▋     | 27/58 [00:00<00:00, 39.48it/s]Capturing num tokens (num_tokens=576 avail_mem=120.23 GB):  47%|████▋     | 27/58 [00:00<00:00, 39.48it/s]Capturing num tokens (num_tokens=512 avail_mem=120.22 GB):  47%|████▋     | 27/58 [00:00<00:00, 39.48it/s]Capturing num tokens (num_tokens=480 avail_mem=120.24 GB):  47%|████▋     | 27/58 [00:00<00:00, 39.48it/s]

    Capturing num tokens (num_tokens=448 avail_mem=120.24 GB):  47%|████▋     | 27/58 [00:00<00:00, 39.48it/s]Capturing num tokens (num_tokens=416 avail_mem=120.23 GB):  47%|████▋     | 27/58 [00:00<00:00, 39.48it/s]Capturing num tokens (num_tokens=416 avail_mem=120.23 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.15it/s]Capturing num tokens (num_tokens=384 avail_mem=120.23 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.15it/s]Capturing num tokens (num_tokens=352 avail_mem=120.23 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.15it/s]Capturing num tokens (num_tokens=320 avail_mem=120.22 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.15it/s]Capturing num tokens (num_tokens=288 avail_mem=120.22 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.15it/s]Capturing num tokens (num_tokens=256 avail_mem=120.22 GB):  55%|█████▌    | 32/58 [00:01<00:00, 41.15it/s]Capturing num tokens (num_tokens=256 avail_mem=120.22 GB):  64%|██████▍   | 37/58 [00:01<00:00, 41.75it/s]Capturing num tokens (num_tokens=240 avail_mem=120.21 GB):  64%|██████▍   | 37/58 [00:01<00:00, 41.75it/s]Capturing num tokens (num_tokens=224 avail_mem=120.21 GB):  64%|██████▍   | 37/58 [00:01<00:00, 41.75it/s]

    Capturing num tokens (num_tokens=208 avail_mem=120.20 GB):  64%|██████▍   | 37/58 [00:01<00:00, 41.75it/s]Capturing num tokens (num_tokens=192 avail_mem=120.20 GB):  64%|██████▍   | 37/58 [00:01<00:00, 41.75it/s]Capturing num tokens (num_tokens=176 avail_mem=120.20 GB):  64%|██████▍   | 37/58 [00:01<00:00, 41.75it/s]Capturing num tokens (num_tokens=176 avail_mem=120.20 GB):  72%|███████▏  | 42/58 [00:01<00:00, 42.86it/s]Capturing num tokens (num_tokens=160 avail_mem=120.19 GB):  72%|███████▏  | 42/58 [00:01<00:00, 42.86it/s]Capturing num tokens (num_tokens=144 avail_mem=120.19 GB):  72%|███████▏  | 42/58 [00:01<00:00, 42.86it/s]Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  72%|███████▏  | 42/58 [00:01<00:00, 42.86it/s]Capturing num tokens (num_tokens=112 avail_mem=120.19 GB):  72%|███████▏  | 42/58 [00:01<00:00, 42.86it/s]Capturing num tokens (num_tokens=96 avail_mem=120.18 GB):  72%|███████▏  | 42/58 [00:01<00:00, 42.86it/s] Capturing num tokens (num_tokens=96 avail_mem=120.18 GB):  81%|████████  | 47/58 [00:01<00:00, 43.43it/s]Capturing num tokens (num_tokens=80 avail_mem=120.18 GB):  81%|████████  | 47/58 [00:01<00:00, 43.43it/s]Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  81%|████████  | 47/58 [00:01<00:00, 43.43it/s]

    Capturing num tokens (num_tokens=48 avail_mem=120.17 GB):  81%|████████  | 47/58 [00:01<00:00, 43.43it/s]Capturing num tokens (num_tokens=32 avail_mem=120.17 GB):  81%|████████  | 47/58 [00:01<00:00, 43.43it/s]Capturing num tokens (num_tokens=28 avail_mem=120.16 GB):  81%|████████  | 47/58 [00:01<00:00, 43.43it/s]Capturing num tokens (num_tokens=28 avail_mem=120.16 GB):  90%|████████▉ | 52/58 [00:01<00:00, 43.91it/s]Capturing num tokens (num_tokens=24 avail_mem=120.16 GB):  90%|████████▉ | 52/58 [00:01<00:00, 43.91it/s]Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  90%|████████▉ | 52/58 [00:01<00:00, 43.91it/s]Capturing num tokens (num_tokens=16 avail_mem=120.16 GB):  90%|████████▉ | 52/58 [00:01<00:00, 43.91it/s]Capturing num tokens (num_tokens=12 avail_mem=120.15 GB):  90%|████████▉ | 52/58 [00:01<00:00, 43.91it/s]Capturing num tokens (num_tokens=8 avail_mem=120.15 GB):  90%|████████▉ | 52/58 [00:01<00:00, 43.91it/s] Capturing num tokens (num_tokens=8 avail_mem=120.15 GB):  98%|█████████▊| 57/58 [00:01<00:00, 44.55it/s]Capturing num tokens (num_tokens=4 avail_mem=120.15 GB):  98%|█████████▊| 57/58 [00:01<00:00, 44.55it/s]Capturing num tokens (num_tokens=4 avail_mem=120.15 GB): 100%|██████████| 58/58 [00:01<00:00, 38.81it/s]


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
    Generated text:  Felipe. I am a 15-year-old boy. I'm from Brazil. I'm here to teach you about a special kind of dog called an African wild dog. Let me tell you about its color, size, and where it lives.
    African wild dogs are very different from the dogs you see in the pet stores. They have dark hair in their ears, ears, and tails. Their bodies are covered with a thick coat. They have very long legs. They can run very fast, and they are very brave. African wild dogs are not wild dogs. They're not wild animals. They're wild dogs from the African
    ===============================
    Prompt: The president of the United States is
    Generated text:  trying to choose a new logo for the country. There are three main options for the logo: Option A, Option B, and Option C. The president has conducted a survey to see how many people in the United States consider each option appealing.
    
    The survey results are as follows:
    
    - 20% of people prefer Option A
    - 50% of people prefer Option B
    - 30% of people prefer Option C
    
    The president decides to use the weighted average of these preferences to determine the final logo choice. He will assign a weight of 0.8 to Option A, 0.6 to Option B
    ===============================
    Prompt: The capital of France is
    Generated text: :
    
    A) Paris  
    B) Brussels  
    C) Lyon  
    D) Toulouse
    
    To determine the capital of France, we need to recall that the capital of France is typically the largest city in the country, not the smallest. Let's consider the options given:
    
    A) Paris
    B) Brussels
    C) Lyon
    D) Toulouse
    
    Since Paris is the largest city in France, it is likely to be the capital. However, we should also consider the options provided.
    
    If Paris is the capital, then the largest city in France would be Lyon. But Lyon is not one of the options.
    
    If Brussels is the capital
    ===============================
    Prompt: The future of AI is
    Generated text:  now. The current state of AI is not bad, but it is not great either. The greatest need today is a change in how AI is used. It needs to be a tool, not a weapon, and it needs to be used responsibly. Therefore, the training of AI is essential. In this article, we will explore the current status of AI training, the need for AI training, and the future of AI training. AI training is a subject of great interest to the machine learning community, and it will continue to grow in importance in the coming years. However, it is important to understand that the training of AI is a complex


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


    Generated text:  [Name] and I'm a [occupation] with [number of years] years of experience in [field]. I'm a [type of person] who is always [positive trait]. I'm [age] years old and I'm [gender]. I'm [occupation] with [number of years] years of experience in [field]. I'm a [type of person] who is always [positive trait]. I'm [age] years old and I'm [gender]. I'm [occupation] with [number of years] years of experience in [field]. I'm a [type of person] who is always [positive
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris. 
    
    The statement is concise and accurately describes the capital city of France, providing only the key information needed to understand the statement. It does not include any additional details or context. 
    
    The statement is factual and accurate, as it is based on official information provided by the French government. It does not contain any errors or omissions, and it accurately represents the capital city of France. 
    
    The statement is clear and easy to understand, making it suitable for use in various contexts, such as news articles, travel guides, or educational materials. 
    
    The statement is relevant and useful, as it provides information about the capital city of France that
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: As AI becomes more sophisticated, it is likely to become more integrated with human intelligence, allowing it to learn from and adapt to human behavior and decision-making processes.
    
    2. Greater emphasis on ethical considerations: As AI becomes more advanced, there will be a greater emphasis on ethical considerations, including issues such as bias, transparency, and accountability.
    
    3. Development of more advanced AI: As AI continues to advance, there will be a greater focus on developing more advanced AI systems that can perform tasks more efficiently and effectively than current systems.
    
    4. Increased use of AI
    


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
    Generated text:  __________. I am a _________. I am passionate about __________. My goal is to be _________. My personality is _____, and I have a strong sense of ____. My work ethic is ________. I am a talented ____. I am an ___.
    Certainly! Here is a short, neutral self-introduction for a fictional character:
    
    ---
    
    **Hello, my name is [Your Name]. I am a [Occupation] with [ passion / goal ] in mind. I am a [Personality] who is passionate about [Your Profession] and striving to be [Your Goal]. I am a talented [Occupation
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. It is the largest city in France and the 10th largest city in the world. It was founded in the 12th century and is the country's political, economic, cultural, and educational capital. It has a population of 2.1 million people and is a major world center of art, music, fashion, and cuisine. The city is known for its architecture, museums, and theaters, and has been a UNESCO World Heritage site for several decades. Paris is often referred to as "The City of Light" due to its iconic architecture and vibrant night life. It is home to many famous landmarks such
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to involve many developments and trends. Here are a few possible future trends in AI:
    
    1. Increased Integration: More AI systems will be integrated into everyday devices, such as smartphones, computers, and household appliances, making them more useful and efficient.
    
    2. Automation: AI systems will become more autonomous and will be able to perform tasks that typically require human intervention, such as making decisions, optimizing processes, and managing finances.
    
    3. AI Ethics: There will be growing concerns about the ethical implications of AI, such as bias, privacy, and accountability. Governments and organizations will be pushing for more robust AI regulations and standards.
    
    4. AI


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

    ].

     I

     am

     a

     [

    age

    ]

     year

     old

     [

    gender

    ]

     who

     was

     raised

     in

     [

    location

    ]

     and

     trained

     at

     [

    school

    ].

     I

     have

     a

     love

     for

     [

    interest

     or

     hobby

    ],

     and

     I

     am

     always

     seeking

     knowledge

     and

     learning

    .

     I

     am

     currently

     [

    current

     occupation

     or

     position

    ].

     What

    's

     your

     name

     and

     what

     do

     you

     do

    ?

     How

     would

     you

     describe

     your

     personality

     and

     what

     makes

     you

     unique

    ?

     You

     have

     a

     unique

     ability

     and

     talent

     that

     stands

     out

     from

     the

     rest

    .

     What

     makes

     you

     unique

    ?

     Let

     me

     know

    !

     [

    Your

     Name

    ].

     What

     do

     you

     do

    ?

     [

    Your

     Name

    ].

     What

    's

     your

     name

    ?

     [

    Your

     Name

    ].

     What

     do

     you

     do

    ?

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     It

     is

     the

     largest

     city

     and

     the

     second

     largest

     metropolitan

     area

     in

     the

     world

    .

     The

     city

     is

     also

     the

     country

    's

     cultural

     and

     economic

     center

    ,

     with

     a

     rich

     history

     dating

     back

     to

     the

     Roman

     period

    .

     Paris

     is

     known

     for

     its

     iconic

     landmarks

     such

     as

     the

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

     Dame

     Cathedral

    ,

     and

     Mont

    mart

    re

    .

     The

     city

     is

     also

     home

     to

     numerous

     cultural

     institutions

     and

     a

     thriving

     arts

     scene

    ,

     with

     many

     notable

     artists

     and

     musicians

    .

     As

     a

     historical

     and

     cultural

     center

    ,

     Paris

     has

     played

     a

     significant

     role

     in

     the

     country

    's

     history

    ,

     including

     the

     French

     Revolution

    ,

     the

     French

     Revolution

    ,

     and

     the

     influence

     of

     Enlightenment

     thinkers

     and

     philosophers

    .

     The

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     involve

     the

     development

     of

     more

     sophisticated

     and

     efficient

     algorithms

     that

     can

     handle

     a

     wide

     range

     of

     tasks

     and

     inputs

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

     near

     and

     long

     term

    :
    


    1

    .

     Increased

     focus

     on

     natural

     language

     processing

     (

    N

    LP

    ):

     N

    LP

     is

     a

     rapidly

     growing

     field

     that

     is

     expected

     to

     continue

     to

     play

     an

     increasingly

     important

     role

     in

     AI

    .

     As

     AI

     becomes

     more

     capable

     of

     understanding

     human

     language

     and

     context

    ,

     we

     may

     see

     a

     shift

     towards

     more

     nuanced

     and

     natural

    -s

    ounding

     language

    .
    


    2

    .

     Improved

     machine

     learning

     techniques

    :

     As

     AI

     becomes

     more

     sophisticated

    ,

     we

     may

     see

     a

     greater

     focus

     on

     developing

     more

     effective

     machine

     learning

     techniques

     that

     can

     handle

     large

     amounts

     of

    



```python
llm.shutdown()
```
