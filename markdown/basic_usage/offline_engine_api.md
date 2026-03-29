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


    2026-03-29 09:01:11.458 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 09:01:11] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 09:01:11.459 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 09:01:11] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 09:01:11.459 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 09:01:11] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 09:01:11.459 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 09:01:11] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 09:01:11.459 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 09:01:11] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.82it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.81it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:34,  2.72s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:34,  2.72s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:34,  2.72s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:34,  2.72s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:34,  2.72s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:22,  2.32it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:22,  2.32it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:22,  2.32it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:22,  2.32it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:22,  2.32it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:22,  2.32it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:22,  2.32it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:07,  6.10it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:07,  6.10it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:07,  6.10it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:07,  6.10it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:07,  6.10it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:03<00:07,  6.10it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:03<00:07,  6.10it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:03<00:07,  6.10it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:03<00:07,  6.10it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:03, 12.32it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:03, 12.32it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:03, 12.32it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:03, 12.32it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:03, 12.32it/s]

    Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:03, 12.32it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:03, 12.32it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 17.45it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 17.45it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 17.45it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 17.45it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 17.45it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 17.45it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 17.45it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:03<00:01, 17.45it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 24.25it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 24.25it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 24.25it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 24.25it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 24.25it/s]

    Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 24.25it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 24.25it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 29.41it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 29.41it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 29.41it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 29.41it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 29.41it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 29.41it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 29.41it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 34.92it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 34.92it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 34.92it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 34.92it/s] 

    Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 34.92it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 34.92it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 34.92it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 38.32it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 38.32it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 38.32it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 38.32it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 38.32it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 38.32it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 38.32it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 38.32it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 38.32it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 47.01it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.66it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=121.74 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=121.71 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=121.71 GB):   3%|▎         | 2/58 [00:00<00:02, 18.77it/s]Capturing num tokens (num_tokens=7168 avail_mem=121.70 GB):   3%|▎         | 2/58 [00:00<00:02, 18.77it/s]Capturing num tokens (num_tokens=6656 avail_mem=121.70 GB):   3%|▎         | 2/58 [00:00<00:02, 18.77it/s]Capturing num tokens (num_tokens=6144 avail_mem=121.70 GB):   3%|▎         | 2/58 [00:00<00:02, 18.77it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=121.70 GB):   9%|▊         | 5/58 [00:00<00:02, 22.08it/s]Capturing num tokens (num_tokens=5632 avail_mem=121.70 GB):   9%|▊         | 5/58 [00:00<00:02, 22.08it/s]Capturing num tokens (num_tokens=5120 avail_mem=121.43 GB):   9%|▊         | 5/58 [00:00<00:02, 22.08it/s]Capturing num tokens (num_tokens=4608 avail_mem=121.42 GB):   9%|▊         | 5/58 [00:00<00:02, 22.08it/s]Capturing num tokens (num_tokens=4608 avail_mem=121.42 GB):  14%|█▍        | 8/58 [00:00<00:01, 25.07it/s]Capturing num tokens (num_tokens=4096 avail_mem=121.15 GB):  14%|█▍        | 8/58 [00:00<00:01, 25.07it/s]Capturing num tokens (num_tokens=3840 avail_mem=120.72 GB):  14%|█▍        | 8/58 [00:00<00:01, 25.07it/s]Capturing num tokens (num_tokens=3584 avail_mem=120.71 GB):  14%|█▍        | 8/58 [00:00<00:01, 25.07it/s]Capturing num tokens (num_tokens=3328 avail_mem=120.71 GB):  14%|█▍        | 8/58 [00:00<00:01, 25.07it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=120.71 GB):  21%|██        | 12/58 [00:00<00:01, 30.08it/s]Capturing num tokens (num_tokens=3072 avail_mem=120.70 GB):  21%|██        | 12/58 [00:00<00:01, 30.08it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.70 GB):  21%|██        | 12/58 [00:00<00:01, 30.08it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.70 GB):  21%|██        | 12/58 [00:00<00:01, 30.08it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.70 GB):  21%|██        | 12/58 [00:00<00:01, 30.08it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.69 GB):  21%|██        | 12/58 [00:00<00:01, 30.08it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.69 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.11it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.69 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.11it/s]Capturing num tokens (num_tokens=1536 avail_mem=120.69 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.11it/s]Capturing num tokens (num_tokens=1280 avail_mem=120.68 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.11it/s]Capturing num tokens (num_tokens=1024 avail_mem=120.66 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.11it/s]

    Capturing num tokens (num_tokens=960 avail_mem=120.68 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.11it/s] Capturing num tokens (num_tokens=960 avail_mem=120.68 GB):  38%|███▊      | 22/58 [00:00<00:00, 38.24it/s]Capturing num tokens (num_tokens=896 avail_mem=120.67 GB):  38%|███▊      | 22/58 [00:00<00:00, 38.24it/s]Capturing num tokens (num_tokens=832 avail_mem=120.67 GB):  38%|███▊      | 22/58 [00:00<00:00, 38.24it/s]Capturing num tokens (num_tokens=768 avail_mem=120.67 GB):  38%|███▊      | 22/58 [00:00<00:00, 38.24it/s]Capturing num tokens (num_tokens=704 avail_mem=120.66 GB):  38%|███▊      | 22/58 [00:00<00:00, 38.24it/s]Capturing num tokens (num_tokens=640 avail_mem=120.66 GB):  38%|███▊      | 22/58 [00:00<00:00, 38.24it/s]Capturing num tokens (num_tokens=640 avail_mem=120.66 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.60it/s]Capturing num tokens (num_tokens=576 avail_mem=120.66 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.60it/s]Capturing num tokens (num_tokens=512 avail_mem=120.65 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.60it/s]Capturing num tokens (num_tokens=480 avail_mem=120.24 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.60it/s]Capturing num tokens (num_tokens=448 avail_mem=120.24 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.60it/s]

    Capturing num tokens (num_tokens=416 avail_mem=120.23 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.60it/s]Capturing num tokens (num_tokens=416 avail_mem=120.23 GB):  55%|█████▌    | 32/58 [00:00<00:00, 42.05it/s]Capturing num tokens (num_tokens=384 avail_mem=120.23 GB):  55%|█████▌    | 32/58 [00:00<00:00, 42.05it/s]Capturing num tokens (num_tokens=352 avail_mem=120.23 GB):  55%|█████▌    | 32/58 [00:00<00:00, 42.05it/s]Capturing num tokens (num_tokens=320 avail_mem=120.22 GB):  55%|█████▌    | 32/58 [00:00<00:00, 42.05it/s]Capturing num tokens (num_tokens=288 avail_mem=120.22 GB):  55%|█████▌    | 32/58 [00:00<00:00, 42.05it/s]Capturing num tokens (num_tokens=256 avail_mem=120.22 GB):  55%|█████▌    | 32/58 [00:00<00:00, 42.05it/s]Capturing num tokens (num_tokens=256 avail_mem=120.22 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.66it/s]Capturing num tokens (num_tokens=240 avail_mem=120.21 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.66it/s]Capturing num tokens (num_tokens=224 avail_mem=120.21 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.66it/s]Capturing num tokens (num_tokens=208 avail_mem=120.21 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.66it/s]Capturing num tokens (num_tokens=192 avail_mem=120.20 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.66it/s]

    Capturing num tokens (num_tokens=176 avail_mem=120.20 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.66it/s]Capturing num tokens (num_tokens=176 avail_mem=120.20 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.19it/s]Capturing num tokens (num_tokens=160 avail_mem=120.19 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.19it/s]Capturing num tokens (num_tokens=144 avail_mem=120.19 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.19it/s]Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.19it/s]Capturing num tokens (num_tokens=112 avail_mem=120.19 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.19it/s]Capturing num tokens (num_tokens=96 avail_mem=120.18 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.19it/s] Capturing num tokens (num_tokens=96 avail_mem=120.18 GB):  81%|████████  | 47/58 [00:01<00:00, 43.74it/s]Capturing num tokens (num_tokens=80 avail_mem=120.18 GB):  81%|████████  | 47/58 [00:01<00:00, 43.74it/s]Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  81%|████████  | 47/58 [00:01<00:00, 43.74it/s]Capturing num tokens (num_tokens=48 avail_mem=120.17 GB):  81%|████████  | 47/58 [00:01<00:00, 43.74it/s]

    Capturing num tokens (num_tokens=32 avail_mem=120.17 GB):  81%|████████  | 47/58 [00:01<00:00, 43.74it/s]Capturing num tokens (num_tokens=28 avail_mem=120.16 GB):  81%|████████  | 47/58 [00:01<00:00, 43.74it/s]Capturing num tokens (num_tokens=28 avail_mem=120.16 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.26it/s]Capturing num tokens (num_tokens=24 avail_mem=120.16 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.26it/s]Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.26it/s]Capturing num tokens (num_tokens=16 avail_mem=120.16 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.26it/s]Capturing num tokens (num_tokens=12 avail_mem=120.15 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.26it/s]Capturing num tokens (num_tokens=8 avail_mem=120.15 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.26it/s] Capturing num tokens (num_tokens=8 avail_mem=120.15 GB):  98%|█████████▊| 57/58 [00:01<00:00, 45.04it/s]Capturing num tokens (num_tokens=4 avail_mem=120.15 GB):  98%|█████████▊| 57/58 [00:01<00:00, 45.04it/s]Capturing num tokens (num_tokens=4 avail_mem=120.15 GB): 100%|██████████| 58/58 [00:01<00:00, 39.64it/s]


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
    Generated text:  Nevin Sandburg. I am a computer engineer and a teacher, and I am looking for a new job that aligns with my career goals and skills. Can you help me find a job that could fulfill my needs? I am looking for a job that is in a field that is likely to provide me with opportunities to learn new things, and that will also allow me to work with different types of people. I have some knowledge and skills related to programming, but I need more experience in areas such as design, architecture, and materials science. I have a degree in Computer Science and have experience working in a variety of industries, including
    ===============================
    Prompt: The president of the United States is
    Generated text:  a member of the U.S. Congress, which has one or more senators. One of the duties of the president is to nominate and recommend new members to the Senate. Suppose that the Senate is a bicameral legislature, with two chambers: the House of Representatives and the Senate. Assume that there are $n$ members in the Senate, and each senator is assigned to a specific chamber. The president nominates a new member to each chamber by an unannounced process. Each senator is assigned a unique number from 1 to $n$, and the president chooses the senator with the highest number to be the new member to their chamber
    ===============================
    Prompt: The capital of France is
    Generated text: :
    A. Paris
    B. London
    C. Berlin
    D. Rome
    
    The capital of France is Paris. Therefore, the correct answer is:
    
    A. Paris
    
    Note: Rome is the capital of Italy, not France. London, Berlin, and Washington, D.C. are the other capitals of these countries. Rome is the capital of Italy. 
    
    To answer the question more generally: The capital of a country is the largest city that is not controlled by another country or region. The capital of a nation-state is the one that holds the most power and influence, typically determined by its population. The capital is often in a
    ===============================
    Prompt: The future of AI is
    Generated text:  this: AI is a living creature, and it’s not always a good thing when it goes rogue. This is particularly true for the artificial intelligence of the future, which is becoming more and more prevalent in our world as more and more advanced technologies come to fruition.
    As the world is making progress and changing, AI can come to a point of becoming less effective. As a result, we need to look at ways to keep the AI from being a threat to us or our lives. That means keeping the AI under a handle and making sure that we’re aware of its true intentions.
    Therefore, the first thing that we need to do is


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


    Generated text:  [Name], and I'm a [Age] year old [Occupation]. I'm a [Type of Character] who has [Number of Years in the Profession] years of experience in [Field of Work]. I'm [Type of Character] and I'm [Name]! I'm a [Type of Character] who has [Number of Years in the Profession] years of experience in [Field of Work]. I'm [Name]! I'm a [Type of Character] who has [Number of Years in the Profession] years of experience in [Field of Work]. I'm [Name]! I'm a [Type
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris. It is the largest city in Europe and the third-largest city in the world by population. It is also the seat of government and the country's cultural, political, and economic center. Paris is known for its rich history, beautiful architecture, and vibrant culture. It is home to many famous landmarks such as the Eiffel Tower, the Louvre Museum, and the Notre-Dame Cathedral. Paris is also known for its fashion industry, with many famous fashion designers and boutiques. The city is also home to many museums, including the Louvre and the Musée d'Orsay. Paris is a popular tourist destination and
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends that are expected to shape the future of AI:
    
    1. Increased use of AI in healthcare: AI is already being used in healthcare to improve patient outcomes, reduce costs, and improve the quality of care. As AI technology continues to improve, we can expect to see even more widespread use of AI in healthcare, with more sophisticated algorithms and machine learning techniques being developed to improve diagnosis, treatment, and patient care.
    
    2. Increased use of AI in finance: AI is already being used in
    


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
    Generated text:  [Name] and I'm a [occupation]. I'm a [number of years of experience] professional [position], and I've always been passionate about [interest or hobby] and have dedicated [number of years] of my life to it. My current role involves [current job title] and I'm constantly striving to [personal goal or achievement]. I'm currently [current status] and I enjoy [reason for remaining engaged] in the industry. I believe in [reason for pursuing my career]. How can I help someone today? When it comes to [occupation], what do you do? [Type in the relevant details]. Let
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, also known as “La Grande-Bretagne” and the “City of Love” due to its romantic reputation.
    
    The capital of France is Paris, also known as "La Grande-Bretagne" and the "City of Love" due to its romantic reputation. The city is also famous for its stunning architecture, vibrant arts scene, and rich cultural heritage. Paris has been a major hub for the arts since the Middle Ages and is home to numerous museums, theaters, and galleries. The city is also famous for its famous fashion and music scenes, with fashion houses like Louis Vuitton and Parisian couture, and hip
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  exciting and constantly evolving. Here are some possible trends that we can expect to see in the AI industry in the next few years:
    
    1. Increased Integration of AI into Everyday Life: As AI becomes more integrated into our daily lives, we can expect to see more sophisticated AI systems being integrated into our homes, workplaces, and transportation systems. For example, we can expect to see more intelligent assistants like voice assistants, robots, and drones that can interact with humans in a more natural and efficient way.
    
    2. AI in Healthcare: As AI technology continues to advance, we can expect to see more intelligent systems being used in healthcare to diagnose and treat


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

    role

    -playing

     game

     character

    ],

     [

    character

    ]

    !
    


    I

     am

     a

     very

     talented

     and

     experienced

     player

     in

     the

     [

    role

    -playing

     game

    ]

     community

    .

     I

     have

     been

     playing

     for

     [

    number

    ]

     years

     and

     have

     gained

     a

     lot

     of

     experience

     in

     the

     game

    .

     I

     am

     very

     passionate

     about

     the

     game

     and

     enjoy

     exploring

     new

     ideas

     and

     concepts

     in

     the

     game

     world

    .

     I

     also

     have

     a

     great

     sense

     of

     humor

     and

     can

     make

     the

     most

     of

     any

     situation

    .
    


    I

     love

     going

     on

     adventures

     and

     have

     a

     good

     sense

     of

     direction

    .

     I

     am

     always

     looking

     for

     new

     challenges

     and

     new

     opportunities

     to

     learn

     and

     grow

     as

     a

     player

    .

     I

    'm

     always

     eager

     to

     learn

     more

    
    
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

     country

     and

     is

     known

     for

     its

     elegant

     architecture

    ,

     historical

     landmarks

    ,

     and

     vibrant

     cultural

     scene

    .

     The

     city

    's

     famous

     E

    iff

    el

     Tower

     and

     Notre

    -D

    ame

     Cathedral

     are

     just

     a

     few

     of

     its

     most

     famous

     attractions

    .

     Paris

     is

     also

     home

     to

     the

     city

    's

     metropolitan

     area

    ,

     which

     includes

     the

     nearby

     city

     of

     Lyon

    .

     The

     city

     is

     located

     on

     the

     Se

    ine

     River

     and

     is

     a

     popular

     tourist

     destination

     for

     French

     and

     international

     visitors

     alike

    .

     Additionally

    ,

     Paris

     is

     a

     major

     transportation

     hub

     for

     the

     country

    ,

     serving

     as

     the

     country

    's

     primary

     gateway

     to

     the

     outside

     world

    .

     With

     its

     rich

     history

     and

     stunning

     views

    ,

     Paris

     is

     a

     city

     that

     is

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     quite

     exciting

     and

     is

     likely

     to

     continue

     to

     evolve

     rapidly

    .

     Here

     are

     some

     possible

     trends

     in

     the

     AI

     field

    :
    


    1

    .

     Increasing

    ly

     predictive

     and

     proactive

    :

     As

     AI

     becomes

     more

     advanced

    ,

     it

     will

     be

     able

     to

     anticipate

     future

     events

    ,

     predict

     trends

    ,

     and

     identify

     potential

     risks

     before

     they

     become

     problematic

    .

     This

     could

     lead

     to

     proactive

     and

     predictive

     approaches

     to

     problem

    -solving

    ,

     as

     well

     as

     more

     effective

     decision

    -making

    .
    


    2

    .

     Integration

     of

     artificial

     intelligence

     with

     other

     technologies

    :

     AI

     is

     becoming

     increasingly

     integrated

     with

     other

     technologies

    ,

     such

     as

     machine

     learning

    ,

     to

     create

     more

     complex

     and

     powerful

     systems

    .

     This

     could

     lead

     to

     even

     more

     sophisticated

     and

     adaptive

     AI

     systems

     that

     can

     learn

     from

     new

     data

     and

     adapt

     to

    



```python
llm.shutdown()
```
