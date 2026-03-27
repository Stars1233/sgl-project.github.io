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


    2026-03-27 00:55:57.663 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 00:55:57] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 00:55:57.663 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 00:55:57] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 00:55:57.663 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 00:55:57] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 00:55:57.663 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 00:55:57] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 00:55:57.663 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 00:55:57] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  4.56it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  4.56it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:24,  2.53s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:24,  2.53s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:24,  2.53s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:24,  2.53s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:27,  1.97it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:27,  1.97it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:27,  1.97it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:27,  1.97it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:27,  1.97it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:27,  1.97it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:02<00:09,  5.35it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:02<00:09,  5.35it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:02<00:09,  5.35it/s]Compiling num tokens (num_tokens=3328):  16%|█▌        | 9/58 [00:02<00:09,  5.35it/s]Compiling num tokens (num_tokens=3072):  16%|█▌        | 9/58 [00:02<00:09,  5.35it/s]Compiling num tokens (num_tokens=2816):  16%|█▌        | 9/58 [00:02<00:09,  5.35it/s]Compiling num tokens (num_tokens=2560):  16%|█▌        | 9/58 [00:02<00:09,  5.35it/s]Compiling num tokens (num_tokens=2304):  16%|█▌        | 9/58 [00:02<00:09,  5.35it/s]Compiling num tokens (num_tokens=2048):  16%|█▌        | 9/58 [00:02<00:09,  5.35it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:02<00:03, 12.11it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:02<00:03, 12.11it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:02<00:03, 12.11it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:02<00:03, 12.11it/s]Compiling num tokens (num_tokens=1024):  29%|██▉       | 17/58 [00:02<00:03, 12.11it/s]Compiling num tokens (num_tokens=960):  29%|██▉       | 17/58 [00:02<00:03, 12.11it/s] Compiling num tokens (num_tokens=896):  29%|██▉       | 17/58 [00:02<00:03, 12.11it/s]

    Compiling num tokens (num_tokens=832):  29%|██▉       | 17/58 [00:02<00:03, 12.11it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:02<00:01, 18.66it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:02<00:01, 18.66it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:02<00:01, 18.66it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:02<00:01, 18.66it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:03<00:01, 18.66it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:03<00:01, 18.66it/s]Compiling num tokens (num_tokens=480):  41%|████▏     | 24/58 [00:03<00:01, 18.66it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:03<00:01, 23.36it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:03<00:01, 23.36it/s]Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:03<00:01, 23.36it/s]Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:03<00:01, 23.36it/s]

    Compiling num tokens (num_tokens=352):  52%|█████▏    | 30/58 [00:03<00:01, 23.36it/s]Compiling num tokens (num_tokens=320):  52%|█████▏    | 30/58 [00:03<00:01, 23.36it/s]Compiling num tokens (num_tokens=288):  52%|█████▏    | 30/58 [00:03<00:01, 23.36it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:03<00:00, 28.10it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:03<00:00, 28.10it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:03<00:00, 28.10it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:03<00:00, 28.10it/s]Compiling num tokens (num_tokens=208):  62%|██████▏   | 36/58 [00:03<00:00, 28.10it/s]Compiling num tokens (num_tokens=192):  62%|██████▏   | 36/58 [00:03<00:00, 28.10it/s]Compiling num tokens (num_tokens=176):  62%|██████▏   | 36/58 [00:03<00:00, 28.10it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:03<00:00, 32.27it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:03<00:00, 32.27it/s]

    Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:03<00:00, 32.27it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:03<00:00, 32.27it/s]Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:03<00:00, 32.27it/s]Compiling num tokens (num_tokens=96):  72%|███████▏  | 42/58 [00:03<00:00, 32.27it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:03<00:00, 34.07it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:03<00:00, 34.07it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:03<00:00, 34.07it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:03<00:00, 34.07it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:03<00:00, 34.07it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:03<00:00, 34.07it/s]

    Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:03<00:00, 37.37it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:03<00:00, 37.37it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:03<00:00, 37.37it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:03<00:00, 37.37it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:03<00:00, 37.37it/s]Compiling num tokens (num_tokens=8):  90%|████████▉ | 52/58 [00:03<00:00, 37.37it/s] Compiling num tokens (num_tokens=4):  90%|████████▉ | 52/58 [00:03<00:00, 37.37it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.86it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=76.80 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=76.77 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=76.77 GB):   3%|▎         | 2/58 [00:00<00:03, 16.64it/s]Capturing num tokens (num_tokens=7168 avail_mem=76.76 GB):   3%|▎         | 2/58 [00:00<00:03, 16.64it/s]Capturing num tokens (num_tokens=6656 avail_mem=76.76 GB):   3%|▎         | 2/58 [00:00<00:03, 16.64it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=76.76 GB):   3%|▎         | 2/58 [00:00<00:03, 16.64it/s]Capturing num tokens (num_tokens=6144 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:02, 20.12it/s]Capturing num tokens (num_tokens=5632 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:02, 20.12it/s]Capturing num tokens (num_tokens=5120 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:02, 20.12it/s]Capturing num tokens (num_tokens=4608 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:02, 20.12it/s]Capturing num tokens (num_tokens=4608 avail_mem=76.76 GB):  14%|█▍        | 8/58 [00:00<00:02, 22.40it/s]Capturing num tokens (num_tokens=4096 avail_mem=76.76 GB):  14%|█▍        | 8/58 [00:00<00:02, 22.40it/s]Capturing num tokens (num_tokens=3840 avail_mem=76.75 GB):  14%|█▍        | 8/58 [00:00<00:02, 22.40it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=76.75 GB):  14%|█▍        | 8/58 [00:00<00:02, 22.40it/s]Capturing num tokens (num_tokens=3328 avail_mem=76.74 GB):  14%|█▍        | 8/58 [00:00<00:02, 22.40it/s]Capturing num tokens (num_tokens=3328 avail_mem=76.74 GB):  21%|██        | 12/58 [00:00<00:01, 26.99it/s]Capturing num tokens (num_tokens=3072 avail_mem=76.74 GB):  21%|██        | 12/58 [00:00<00:01, 26.99it/s]Capturing num tokens (num_tokens=2816 avail_mem=76.74 GB):  21%|██        | 12/58 [00:00<00:01, 26.99it/s]Capturing num tokens (num_tokens=2560 avail_mem=76.73 GB):  21%|██        | 12/58 [00:00<00:01, 26.99it/s]Capturing num tokens (num_tokens=2304 avail_mem=76.73 GB):  21%|██        | 12/58 [00:00<00:01, 26.99it/s]Capturing num tokens (num_tokens=2048 avail_mem=76.73 GB):  21%|██        | 12/58 [00:00<00:01, 26.99it/s]Capturing num tokens (num_tokens=2048 avail_mem=76.73 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.33it/s]Capturing num tokens (num_tokens=1792 avail_mem=76.72 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.33it/s]Capturing num tokens (num_tokens=1536 avail_mem=76.72 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.33it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=76.72 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.33it/s]Capturing num tokens (num_tokens=1024 avail_mem=76.70 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.33it/s]Capturing num tokens (num_tokens=960 avail_mem=76.71 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.33it/s] Capturing num tokens (num_tokens=960 avail_mem=76.71 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.96it/s]Capturing num tokens (num_tokens=896 avail_mem=76.71 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.96it/s]Capturing num tokens (num_tokens=832 avail_mem=76.70 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.96it/s]Capturing num tokens (num_tokens=768 avail_mem=76.70 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.96it/s]Capturing num tokens (num_tokens=704 avail_mem=76.70 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.96it/s]Capturing num tokens (num_tokens=640 avail_mem=76.69 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.96it/s]Capturing num tokens (num_tokens=640 avail_mem=76.69 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.32it/s]Capturing num tokens (num_tokens=576 avail_mem=76.69 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.32it/s]Capturing num tokens (num_tokens=512 avail_mem=76.68 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.32it/s]

    Capturing num tokens (num_tokens=480 avail_mem=76.70 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.32it/s]Capturing num tokens (num_tokens=448 avail_mem=76.70 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.32it/s]Capturing num tokens (num_tokens=416 avail_mem=76.69 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.32it/s]Capturing num tokens (num_tokens=416 avail_mem=76.69 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.97it/s]Capturing num tokens (num_tokens=384 avail_mem=76.69 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.97it/s]Capturing num tokens (num_tokens=352 avail_mem=76.69 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.97it/s]Capturing num tokens (num_tokens=320 avail_mem=76.68 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.97it/s]Capturing num tokens (num_tokens=288 avail_mem=76.68 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.97it/s]Capturing num tokens (num_tokens=256 avail_mem=76.68 GB):  55%|█████▌    | 32/58 [00:01<00:00, 41.97it/s]Capturing num tokens (num_tokens=256 avail_mem=76.68 GB):  64%|██████▍   | 37/58 [00:01<00:00, 43.57it/s]Capturing num tokens (num_tokens=240 avail_mem=76.68 GB):  64%|██████▍   | 37/58 [00:01<00:00, 43.57it/s]Capturing num tokens (num_tokens=224 avail_mem=76.67 GB):  64%|██████▍   | 37/58 [00:01<00:00, 43.57it/s]

    Capturing num tokens (num_tokens=208 avail_mem=76.67 GB):  64%|██████▍   | 37/58 [00:01<00:00, 43.57it/s]Capturing num tokens (num_tokens=192 avail_mem=76.67 GB):  64%|██████▍   | 37/58 [00:01<00:00, 43.57it/s]Capturing num tokens (num_tokens=176 avail_mem=76.66 GB):  64%|██████▍   | 37/58 [00:01<00:00, 43.57it/s]Capturing num tokens (num_tokens=176 avail_mem=76.66 GB):  72%|███████▏  | 42/58 [00:01<00:00, 44.44it/s]Capturing num tokens (num_tokens=160 avail_mem=76.66 GB):  72%|███████▏  | 42/58 [00:01<00:00, 44.44it/s]Capturing num tokens (num_tokens=144 avail_mem=76.66 GB):  72%|███████▏  | 42/58 [00:01<00:00, 44.44it/s]Capturing num tokens (num_tokens=128 avail_mem=76.65 GB):  72%|███████▏  | 42/58 [00:01<00:00, 44.44it/s]Capturing num tokens (num_tokens=112 avail_mem=76.65 GB):  72%|███████▏  | 42/58 [00:01<00:00, 44.44it/s]Capturing num tokens (num_tokens=96 avail_mem=76.65 GB):  72%|███████▏  | 42/58 [00:01<00:00, 44.44it/s] Capturing num tokens (num_tokens=96 avail_mem=76.65 GB):  81%|████████  | 47/58 [00:01<00:00, 44.63it/s]Capturing num tokens (num_tokens=80 avail_mem=76.64 GB):  81%|████████  | 47/58 [00:01<00:00, 44.63it/s]Capturing num tokens (num_tokens=64 avail_mem=76.64 GB):  81%|████████  | 47/58 [00:01<00:00, 44.63it/s]

    Capturing num tokens (num_tokens=48 avail_mem=76.64 GB):  81%|████████  | 47/58 [00:01<00:00, 44.63it/s]Capturing num tokens (num_tokens=32 avail_mem=76.64 GB):  81%|████████  | 47/58 [00:01<00:00, 44.63it/s]Capturing num tokens (num_tokens=28 avail_mem=76.63 GB):  81%|████████  | 47/58 [00:01<00:00, 44.63it/s]Capturing num tokens (num_tokens=28 avail_mem=76.63 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.57it/s]Capturing num tokens (num_tokens=24 avail_mem=76.63 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.57it/s]Capturing num tokens (num_tokens=20 avail_mem=76.62 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.57it/s]Capturing num tokens (num_tokens=16 avail_mem=76.62 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.57it/s]Capturing num tokens (num_tokens=12 avail_mem=76.62 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.57it/s]Capturing num tokens (num_tokens=8 avail_mem=76.61 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.57it/s] Capturing num tokens (num_tokens=8 avail_mem=76.61 GB):  98%|█████████▊| 57/58 [00:01<00:00, 44.80it/s]Capturing num tokens (num_tokens=4 avail_mem=76.61 GB):  98%|█████████▊| 57/58 [00:01<00:00, 44.80it/s]

    Capturing num tokens (num_tokens=4 avail_mem=76.61 GB): 100%|██████████| 58/58 [00:01<00:00, 38.88it/s]


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
    Generated text:  Max, I'm 16 years old and I'm a computer science major. I have a very bright mind but my own thoughts can sometimes get out of control. I'm currently in high school and my passion is learning new things. My hobbies include playing video games, reading books, and hanging out with friends. What is your favorite hobby? As an AI language model, I don't have personal hobbies or interests like humans do. However, I can suggest some popular hobbies that people often enjoy:
    
    1. Gaming: Many people find solace and enjoyment in playing video games, whether it's a console, a mobile app, or
    ===============================
    Prompt: The president of the United States is
    Generated text:  an elected official. How can the president of the United States be elected? Discuss one method you find plausible and why. One plausible method to elect the president of the United States is through the electoral college system. In the United States, the president is elected by the electoral college, which consists of two senators and 538 representatives. The presidential election is held every four years, with the voter casting a vote for one of the two major political parties. The candidates for president are selected based on their platforms and support for their party's platform. The states that are part of the United States have their own electoral college, with each state
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris, it is not a city. It is a city on a hill. The city is at the foot of the Eiffel Tower. It is not on the ground floor. The city is not on the top floor. The city is not in a high building. It is not in a tower. The city is in the middle of Paris.
    The capital of France is not a city.
    What is the answer?
    To determine the correct answer, let's break down the logical reasoning involved:
    
    1. We are provided with information that the capital of France is not a city.
    2. The capital is described as being on a hill
    ===============================
    Prompt: The future of AI is
    Generated text:  dependent on us. But before we develop and incorporate AI, we must understand the potential and limitations of AI and the value it could provide to humanity.
    In 2013, the company founded by a team of researchers in the UK, who have a history of specializing in AI, set out to develop an AI system that would be able to work in the social and business settings. The system would be able to learn from and adapt to the different contexts in which it will be used, and should also be capable of making decisions that are based on the data it gathers.
    The technology was tested in a series of experiments on real people


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


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French Academy of Sciences, and the French Quarter, a historic neighborhood known for its French colonial architecture and French cuisine. Paris is a bustling metropolis with a rich cultural heritage and a diverse population, making it a popular tourist destination. It is also home to many notable museums, including the Musée d'Orsay and the Musée Rodin. The city is known for its fashion industry, with many famous designers and boutiques located in the city
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by rapid advancements in areas such as machine learning, natural language processing, and computer vision. These technologies are expected to continue to evolve and improve, leading to more sophisticated and accurate AI systems that can perform a wide range of tasks with increasing accuracy and efficiency. Some possible future trends in AI include:
    
    1. Increased focus on ethical considerations: As AI systems become more sophisticated, there will be a greater emphasis on ethical considerations and the responsible use of AI. This will include issues such as bias, transparency, and accountability.
    
    2. Greater integration with human intelligence: AI systems are likely to become more integrated with human intelligence, allowing
    


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
    Generated text:  [name] and I am a [role model or role for your character], [role model or role for your character]. I have always been passionate about [what you can do or interests in], and I am always eager to learn about new ideas, cultures, and technologies. I am [number of years since I graduated from college] years old and I love [anything I do], including [what you can do or hobbies]. I am always striving to improve myself and to contribute to my community. I am [number of years since I started] years of volunteering and [number of years since I started] years of community service.
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, the city that is home to the iconic Eiffel Tower, the Louvre Museum, and many other landmarks.
    
    1. Paris is the capital of France.
    2. It is the largest city in Europe and the third largest city in the world by population.
    3. Paris is the cultural and economic center of France.
    4. The city is known for its rich history, art, and cuisine, as well as its landmarks like the Eiffel Tower, Louvre Museum, Notre-Dame Cathedral, and Montmartre.
    5. Paris has a diverse population, with around 1. 4 million people in the city
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be shaped by a variety of factors, including advances in computing power, advancements in natural language processing and machine learning, changes in the way we interact with technology, and growing concerns around the ethical and societal implications of AI development. Here are some possible future trends in AI:
    
    1. Increased automation and automation: As AI continues to improve its ability to perform tasks more efficiently and accurately, we are likely to see more automation in our daily lives. This could include everything from drones and robots for manufacturing to virtual assistants and chatbots for customer service.
    
    2. Personalized and context-aware AI: As AI is better at understanding and interpreting human


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

    name

    ].

     I

    'm

     a

     [

    job

     title

    ],

     and

     I

    'm

     passionate

     about

     [

    professional

     interest

     or

     hobby

    ].

     How

     can

     I

     help

     you

     today

    ?

     [

    Optional

    :

     mention

     a

     specific

     project

     or

     experience

     to

     showcase

     your

     skills

    .]

     And

     if

     you

     can

    ,

     could

     you

     provide

     some

     details

     about

     what

     you

    're

     looking

     for

    ?

     [

    Optional

    :

     provide

     an

     example

     of

     a

     question

     you

     might

     ask

     to

     find

     out

     more

     about

     the

     potential

     client

     or

     customer

    .]

     Let

    's

     get

     started

    !

     Let

    's

     chat

     about

     what

     we

     have

     in

     common

    .

     [

    Optional

    :

     a

     brief

     summary

     of

     what

     you

     do

     or

     what

     you

     specialize

     in

    .]

     And

     if

     you

     have

     any

     questions

    ,

     don

    't

     hesitate

     to

     ask

    .

     [

    Optional

    
    
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

     most

     populous

     city

     in

     France

     by

     population

    ,

     and

     the

     seat

     of

     the

     Government

     of

     France

    .

     It

     is

     located

     in

     the

     Mass

    if

     Central

     region

     of

     the

     north

    -central

     part

     of

     France

    ,

     and

     is

     one

     of

     the

     oldest

     cities

     in

     Europe

    .

     Paris

     is

     renowned

     for

     its

     architecture

    ,

     art

    ,

     and

     cuisine

    .

     It

     is

     home

     to

     many

     famous

     historical

     sites

    ,

     including

     the

     E

    iff

    el

     Tower

     and

     the

     Lou

    vre

     Museum

    .

     It

     also

     hosts

     numerous

     cultural

     events

     and

     festivals

     throughout

     the

     year

    .

     Paris

     is

     a

     popular

     tourist

     destination

    ,

     with

     millions

     of

     visitors

     each

     year

    .

     The

     city

     is

     known

     for

     its

     rich

     cultural

     heritage

    ,

     art

    ,

     and

     cuisine

    ,

     and

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     vast

     and

     rapidly

     evolving

    ,

     with

     many

     possibilities

     and

     promising

     applications

    .

     Here

     are

     some

     potential

     trends

     in

     AI

    :
    


    1

    .

     Increased

     integration

     with

     other

     technologies

    :

     AI

     is

     already

     being

     integrated

     into

     a

     wide

     range

     of

     technologies

    ,

     from

     smart

     homes

     to

     self

    -driving

     cars

    .

     As

     more

     devices

     and

     systems

     become

     connected

    ,

     AI

     will

     become

     even

     more

     integrated

    ,

     allowing

     for

     a

     more

     seamless

     and

     efficient

     interaction

     between

     humans

     and

     machines

    .
    


    2

    .

     AI

     becoming

     more

     human

    -like

    :

     As

     AI

     becomes

     more

     integrated

     into

     our

     daily

     lives

    ,

     it

    's

     possible

     that

     it

     will

     start

     to

     feel

     more

     like

     a

     human

    .

     For

     example

    ,

     we

     may

     start

     to

     see

     AI

     in

     everyday

     situations

    ,

     such

     as

     finding

     the

     best

     restaurant

     or

    



```python
llm.shutdown()
```
