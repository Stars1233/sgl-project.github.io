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


    2026-03-29 08:04:24.646 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 08:04:24] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 08:04:24.646 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 08:04:24] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 08:04:24.646 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 08:04:24] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 08:04:24.646 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 08:04:24] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 08:04:24.646 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 08:04:24] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.34it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.33it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:19,  2.66it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:19,  2.66it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:19,  2.66it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:19,  2.66it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:19,  2.66it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:19,  2.66it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:19,  2.66it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:06,  6.93it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:06,  6.93it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:06,  6.93it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:06,  6.93it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:06,  6.93it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:06,  6.93it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:02<00:06,  6.93it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:02<00:06,  6.93it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:02<00:06,  6.93it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 13.87it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 13.87it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 13.87it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 13.87it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 13.87it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 13.87it/s]

    Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 13.87it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:02<00:01, 19.34it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:02<00:01, 19.34it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:02<00:01, 19.34it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:02<00:01, 19.34it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:02<00:01, 19.34it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:02<00:01, 19.34it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:02<00:01, 19.34it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:02<00:01, 19.34it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:02<00:00, 26.42it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:02<00:00, 26.42it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:02<00:00, 26.42it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:02<00:00, 26.42it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:02<00:00, 26.42it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:02<00:00, 26.42it/s]

    Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:02<00:00, 26.42it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 31.47it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 31.47it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 31.47it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 31.47it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 31.47it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 31.47it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 31.47it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 36.85it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 36.85it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 36.85it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 36.85it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 36.85it/s]

    Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 36.85it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 36.85it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 39.30it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 39.30it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 39.30it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 39.30it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 39.30it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 39.30it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 39.30it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 39.30it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 39.30it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 48.33it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.37it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=120.76 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.73 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:03, 17.42it/s]Capturing num tokens (num_tokens=7168 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:03, 17.42it/s]Capturing num tokens (num_tokens=6656 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:03, 17.42it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:03, 17.42it/s]Capturing num tokens (num_tokens=6144 avail_mem=120.73 GB):   9%|▊         | 5/58 [00:00<00:02, 21.21it/s]Capturing num tokens (num_tokens=5632 avail_mem=120.72 GB):   9%|▊         | 5/58 [00:00<00:02, 21.21it/s]Capturing num tokens (num_tokens=5120 avail_mem=120.72 GB):   9%|▊         | 5/58 [00:00<00:02, 21.21it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.39 GB):   9%|▊         | 5/58 [00:00<00:02, 21.21it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 21.21it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.29 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.98it/s]Capturing num tokens (num_tokens=3840 avail_mem=120.29 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.98it/s]Capturing num tokens (num_tokens=3584 avail_mem=120.28 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.98it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=120.28 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.98it/s]Capturing num tokens (num_tokens=3072 avail_mem=120.28 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.98it/s]Capturing num tokens (num_tokens=3072 avail_mem=120.28 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.49it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.28 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.49it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.27 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.49it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.27 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.49it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.27 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.49it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.26 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.49it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.26 GB):  31%|███       | 18/58 [00:00<00:01, 34.92it/s]Capturing num tokens (num_tokens=1536 avail_mem=120.26 GB):  31%|███       | 18/58 [00:00<00:01, 34.92it/s]Capturing num tokens (num_tokens=1280 avail_mem=120.25 GB):  31%|███       | 18/58 [00:00<00:01, 34.92it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=120.23 GB):  31%|███       | 18/58 [00:00<00:01, 34.92it/s]Capturing num tokens (num_tokens=960 avail_mem=120.25 GB):  31%|███       | 18/58 [00:00<00:01, 34.92it/s] Capturing num tokens (num_tokens=896 avail_mem=120.24 GB):  31%|███       | 18/58 [00:00<00:01, 34.92it/s]Capturing num tokens (num_tokens=896 avail_mem=120.24 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.03it/s]Capturing num tokens (num_tokens=832 avail_mem=120.24 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.03it/s]Capturing num tokens (num_tokens=768 avail_mem=120.24 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.03it/s]Capturing num tokens (num_tokens=704 avail_mem=120.23 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.03it/s]Capturing num tokens (num_tokens=640 avail_mem=120.23 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.03it/s]Capturing num tokens (num_tokens=576 avail_mem=120.23 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.03it/s]Capturing num tokens (num_tokens=576 avail_mem=120.23 GB):  48%|████▊     | 28/58 [00:00<00:00, 40.24it/s]Capturing num tokens (num_tokens=512 avail_mem=120.22 GB):  48%|████▊     | 28/58 [00:00<00:00, 40.24it/s]Capturing num tokens (num_tokens=480 avail_mem=120.23 GB):  48%|████▊     | 28/58 [00:00<00:00, 40.24it/s]

    Capturing num tokens (num_tokens=448 avail_mem=120.23 GB):  48%|████▊     | 28/58 [00:00<00:00, 40.24it/s]Capturing num tokens (num_tokens=416 avail_mem=120.23 GB):  48%|████▊     | 28/58 [00:00<00:00, 40.24it/s]Capturing num tokens (num_tokens=384 avail_mem=120.23 GB):  48%|████▊     | 28/58 [00:00<00:00, 40.24it/s]Capturing num tokens (num_tokens=384 avail_mem=120.23 GB):  57%|█████▋    | 33/58 [00:00<00:00, 40.08it/s]Capturing num tokens (num_tokens=352 avail_mem=120.22 GB):  57%|█████▋    | 33/58 [00:00<00:00, 40.08it/s]Capturing num tokens (num_tokens=320 avail_mem=120.22 GB):  57%|█████▋    | 33/58 [00:00<00:00, 40.08it/s]Capturing num tokens (num_tokens=288 avail_mem=120.21 GB):  57%|█████▋    | 33/58 [00:00<00:00, 40.08it/s]Capturing num tokens (num_tokens=256 avail_mem=120.21 GB):  57%|█████▋    | 33/58 [00:01<00:00, 40.08it/s]Capturing num tokens (num_tokens=240 avail_mem=120.21 GB):  57%|█████▋    | 33/58 [00:01<00:00, 40.08it/s]Capturing num tokens (num_tokens=240 avail_mem=120.21 GB):  66%|██████▌   | 38/58 [00:01<00:00, 41.52it/s]Capturing num tokens (num_tokens=224 avail_mem=120.21 GB):  66%|██████▌   | 38/58 [00:01<00:00, 41.52it/s]

    Capturing num tokens (num_tokens=208 avail_mem=120.20 GB):  66%|██████▌   | 38/58 [00:01<00:00, 41.52it/s]Capturing num tokens (num_tokens=192 avail_mem=120.20 GB):  66%|██████▌   | 38/58 [00:01<00:00, 41.52it/s]Capturing num tokens (num_tokens=176 avail_mem=120.20 GB):  66%|██████▌   | 38/58 [00:01<00:00, 41.52it/s]Capturing num tokens (num_tokens=160 avail_mem=120.19 GB):  66%|██████▌   | 38/58 [00:01<00:00, 41.52it/s]Capturing num tokens (num_tokens=160 avail_mem=120.19 GB):  74%|███████▍  | 43/58 [00:01<00:00, 42.78it/s]Capturing num tokens (num_tokens=144 avail_mem=120.19 GB):  74%|███████▍  | 43/58 [00:01<00:00, 42.78it/s]Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  74%|███████▍  | 43/58 [00:01<00:00, 42.78it/s]Capturing num tokens (num_tokens=112 avail_mem=120.19 GB):  74%|███████▍  | 43/58 [00:01<00:00, 42.78it/s]Capturing num tokens (num_tokens=96 avail_mem=120.18 GB):  74%|███████▍  | 43/58 [00:01<00:00, 42.78it/s] Capturing num tokens (num_tokens=80 avail_mem=120.18 GB):  74%|███████▍  | 43/58 [00:01<00:00, 42.78it/s]Capturing num tokens (num_tokens=80 avail_mem=120.18 GB):  83%|████████▎ | 48/58 [00:01<00:00, 43.36it/s]Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  83%|████████▎ | 48/58 [00:01<00:00, 43.36it/s]

    Capturing num tokens (num_tokens=48 avail_mem=120.17 GB):  83%|████████▎ | 48/58 [00:01<00:00, 43.36it/s]Capturing num tokens (num_tokens=32 avail_mem=120.17 GB):  83%|████████▎ | 48/58 [00:01<00:00, 43.36it/s]Capturing num tokens (num_tokens=28 avail_mem=120.16 GB):  83%|████████▎ | 48/58 [00:01<00:00, 43.36it/s]Capturing num tokens (num_tokens=24 avail_mem=120.16 GB):  83%|████████▎ | 48/58 [00:01<00:00, 43.36it/s]Capturing num tokens (num_tokens=24 avail_mem=120.16 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.93it/s]Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.93it/s]Capturing num tokens (num_tokens=16 avail_mem=120.16 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.93it/s]Capturing num tokens (num_tokens=12 avail_mem=120.15 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.93it/s]Capturing num tokens (num_tokens=8 avail_mem=120.15 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.93it/s] Capturing num tokens (num_tokens=4 avail_mem=120.15 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.93it/s]Capturing num tokens (num_tokens=4 avail_mem=120.15 GB): 100%|██████████| 58/58 [00:01<00:00, 44.71it/s]Capturing num tokens (num_tokens=4 avail_mem=120.15 GB): 100%|██████████| 58/58 [00:01<00:00, 38.93it/s]


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
    Generated text:  Eric. I'm a 15-year-old boy from the United States. I have a big family. I have two brothers and a sister. My mom is a teacher. She likes to cook. She loves cooking lots of delicious dishes. She makes a lot of delicious food for us. I have a big sister. She likes to be the leader. She likes to make sure we do our homework. She does not like to play with me. My brother likes to draw. He likes to be cool. He likes to take pictures in the park. He is often playing with his pet dog. What's your favorite food? I
    ===============================
    Prompt: The president of the United States is
    Generated text:  in the White House. The president has a vehicle that can carry 500 people. One day, the president needs to visit 3 local hospitals, each with 200 patients waiting. How many people will be waiting in the hospital when the president arrives? To determine how many people will be waiting in the hospital when the president arrives, we need to follow these steps:
    
    1. Identify the total number of patients waiting in the hospital.
    2. Multiply the number of patients by the number of vehicles available.
    
    Step 1: Identify the total number of patients waiting in the hospital.
    The problem states that there are 3
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. In 1900, the population of Paris was 2.1 million, and in 1905 it was 3.1 million. In 1900, the population of each country was the same, and the population of Paris was 2.1 million. Let \( x \) be the ratio of the population of each country in 1905 to 1900. Find the value of \( x \) and determine the population of Paris in 1905.
    
    To find the value of \( x \), the ratio of the population of each country
    ===============================
    Prompt: The future of AI is
    Generated text:  rich and promising, and it's a field that will continue to evolve rapidly over the next decade. It's a field that will allow us to solve the world's problems in new ways, such as healthcare, education, and transportation. The best AI developers will be the ones who can create the most useful and effective AI systems that will improve the lives of people around the world. This is why it's crucial to keep up with the latest developments in AI technology and to stay informed about the latest trends and techniques in the field. As AI becomes more and more powerful, we'll see more and more of its applications being used in the real


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm a [job title] with [number of years] years of experience in [industry]. I'm a [job title] with [number of years] years of experience in [industry]. I'm a [job title] with [number of years] years of experience in [industry]. I'm a [job title] with [number of years] years of experience in [industry]. I'm a [job title] with [number of years] years of experience in [industry]. I'm a [job title] with [number of years
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as "La Ville Flottante" (floating city). It is the largest city in Europe and the third largest city in the world by population. Paris is known for its rich history, art, and culture, and is a major tourist destination. It is also home to many famous landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. The city is also known for its cuisine, including French cuisine and its famous dishes such as croissants, escargot, and escargot. Paris is a city of contrasts, with its modern architecture and historical landmarks blending seamlessly together
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by rapid advancements in areas such as machine learning, natural language processing, and computer vision. Here are some possible future trends in AI:
    
    1. Increased integration with other technologies: AI is likely to become more integrated with other technologies, such as blockchain, IoT, and autonomous vehicles. This integration could lead to new applications and opportunities for AI.
    
    2. Enhanced privacy and security: As AI systems become more complex and sophisticated, there will be increased concerns about privacy and security. There will be a need for new technologies and regulations to address these concerns.
    
    3. Increased focus on ethical AI: As AI systems become more complex and
    


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
    Generated text:  [insert name]. I'm a [insert occupation] with a background in [insert relevant field or industry]. Throughout my career, I've [insert achievements or skills that demonstrate my abilities and expertise in the field]. I'm always looking for new opportunities and always eager to learn and grow. Please let me know if you'd like to get to know me better. [insert any additional information you'd like to include in your self-introduction, such as a quote, a humorous anecdote, or a personal anecdote]. [insert your name here] Hello! I'm [insert your name], and I'm a [insert occupation]
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, known for its iconic Eiffel Tower, landmarks such as the Louvre Museum, and a rich cultural scene. 
    
    Please note that due to copyright concerns, I cannot generate a complete list of all the city attractions in France. However, I can provide a list of some of the most popular attractions:
    
    1. Eiffel Tower
    2. Louvre Museum
    3. Notre-Dame Cathedral
    4. Musée d'Orsay
    5. Palace of Versailles
    6. Palace of Versailles (close to the Louvre)
    7. Palace of Versailles (close to the Eiffel Tower)
    8
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  full of potential, exciting developments. Here are some possible trends in the coming years:
    
    1. More automation and autonomous systems: As AI technology advances, we're likely to see more automation and autonomous systems being developed. These systems could be used in a wide range of applications, from factory automation to personal assistants.
    
    2. AI in healthcare: AI is already being used in healthcare, from medical imaging to drug development. In the future, we could see even more advanced AI technologies being integrated into healthcare, such as personalized medicine and virtual surgery.
    
    3. AI in education: AI is already being used in education, from chatbots to personalized learning


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

     talented

     [

    insert

     a

     specific

     skill

     or

     profession

    ].

     I

     am

     always

     looking

     for

     new

     challenges

     and

     opportunities

     to

     improve

     myself

     and

     grow

    .

     I

     enjoy

     problem

    -solving

     and

     collaborating

     with

     others

    ,

     and

     I

     thrive

     in

     a

     fast

    -paced

    ,

     creative

     environment

    .

     My

     love

     for

     learning

     is

     infectious

    ,

     and

     I

     am

     always

     eager

     to

     learn

     new

     things

     and

     broaden

     my

     hor

    izons

    .

     I

     believe

     in

     the

     power

     of

     resilience

     and

     perseverance

    ,

     and

     I

     am

     committed

     to

     achieving

     my

     goals

     with

     hard

     work

     and

     dedication

    .

     I

     am

     a

     great

     listener

     and

     love

     to

     get

     to

     know

     people

    ,

     so

     I

     am

     always

     looking

     for

     ways

     to

     connect

     with

     others

     and

     build

     meaningful

     relationships

    .

     Thank

     you

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     It

     is

     a

     bustling

     and

     historical

     city

     located

     in

     the

     north

    western

     part

     of

     the

     country

    .

     Paris

     is

     known

     for

     its

     world

    -ren

    owned

     landmarks

     such

     as

     the

     E

    iff

    el

     Tower

    ,

     Notre

    -D

    ame

     Cathedral

    ,

     and

     Lou

    vre

     Museum

    .

     It

     is

     also

     home

     to

     many

     fine

     restaurants

    ,

     museums

    ,

     and

     cultural

     institutions

    .

     The

     city

     is

     known

     for

     its

     vibrant

     nightlife

    ,

     fashion

     industry

    ,

     and

     annual

     festivals

     like

     the

     C

    ôte

     d

    '

    Az

    ur

    .

     France

    's

     capital

     city

     Paris

     is

     a

     major

     hub

     for

     business

     and

     economic

     activity

    ,

     and

     it

     continues

     to

     be

     an

     influential

     and

     dynamic

     city

    .

     It

     is

     one

     of

     the

     most

     visited

     cities

     in

     the

     world

     and

     has

     a

     global

     reputation

     as

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     promising

    ,

     and

     there

     are

     many

     possible

     trends

     that

     could

     shape

     the

     direction

     of

     the

     technology

    .

     Here

     are

     some

     potential

     areas

     that

     could

     lead

     to

     further

     advancements

    :
    


    1

    .

     Increased

     integration

    :

     One

     of

     the

     key

     trends

     in

     AI

     is

     the

     increasing

     integration

     of

     AI

     with

     other

     technologies

    .

     This

     could

     include

     the

     use

     of

     AI

     in

     cars

    ,

     robots

    ,

     and

     other

     machines

     that

     are

     being

     integrated

     into

     our

     daily

     lives

    .

     This

     integration

     could

     lead

     to

     more

     sophisticated

     AI

     systems

     that

     can

     learn

     from

     and

     adapt

     to

     new

     data

    .
    


    2

    .

     Personal

    ization

    :

     AI

     is

     becoming

     increasingly

     personalized

    ,

     with

     machines

     able

     to

     learn

     from

     user

     data

     and

     make

     recommendations

     based

     on

     that

     data

    .

     This

     could

     lead

     to

     more

     personalized

     experiences

    



```python
llm.shutdown()
```
